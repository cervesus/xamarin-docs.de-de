---
title: Buildoptimierungen
description: In diesem Dokument werden die verschiedenen Optimierungen erläutert, die zur Buildzeit für xamarin. IOS-und xamarin. Mac-Apps verwendet werden.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: conceptdev
ms.author: crdun
ms.date: 04/16/2018
ms.openlocfilehash: 4fdc40a8aed4b3137e418d6123fc000c2b36b6dd
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226205"
---
# <a name="build-optimizations"></a>Buildoptimierungen

In diesem Dokument werden die verschiedenen Optimierungen erläutert, die zur Buildzeit für xamarin. IOS-und xamarin. Mac-Apps verwendet werden.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Remove UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Entfernt Aufrufe an [UIApplication. ensureuithread][1] (für xamarin. IOS) oder `NSApplication.EnsureUIThread` (für xamarin. Mac).

Durch diese Optimierung wird der folgende Codetyp geändert:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

in Folgendes:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Standardmäßig ist es für Releasebuilds aktiviert.

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]remove-uithread-checks` Sie an mtouchscreen/MMP übergeben werden.

[1]: https://docs.microsoft.com/dotnet/api/UIKit.UIApplication.EnsureUIThread

## <a name="inline-intptrsize"></a>Inline-IntPtr. Size

Inlines den konstanten Wert von `IntPtr.Size` entsprechend der Zielplattform.

Durch diese Optimierung wird der folgende Codetyp geändert:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Im folgenden (bei der Erstellung für eine 64-Bit-Plattform):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Diese Option ist standardmäßig aktiviert, wenn eine einzelne Architektur als Ziel verwendet wird, oder für die Plattformassembly (**xamarin. IOS. dll**, **xamarin. tvos. dll**, **xamarin. watchos. dll** oder **xamarin. Mac. dll**).

Wenn Sie mehrere Architekturen als Ziel verwenden, erstellt diese Optimierung verschiedene Assemblys für die 32-Bit-Version und die 64-Bit-Version der APP, und beide Versionen müssen in der App enthalten sein, wodurch die endgültige App-Größe erhöht wird, anstatt die Größe zu verringern. ihm.

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]inline-intptr-size` Sie an mtouchscreen/MMP übergeben werden.

## <a name="inline-nsobjectisdirectbinding"></a>Inline-NSObject. isdirectbinding

`NSObject.IsDirectBinding`ist eine Instanzeigenschaft, die bestimmt, ob eine bestimmte Instanz einen Wrappertyp hat oder nicht (ein Wrappertyp ist ein verwalteter Typ, der einem systemeigenen `UIKit.UIView` Typ zugeordnet ist. der `UIView` verwaltete Typ ist beispielsweise dem systemeigenen Typ zugeordnet. das Gegenteil ist ein Benutzertyp. ist in diesem Fall `class MyUIView : UIKit.UIView` ein Benutzertyp.)

Der Wert von muss beim Aufrufen von `IsDirectBinding` "Ziel-C" bekannt sein, da der Wert bestimmt, welche Version von `objc_msgSend` verwendet werden soll.

Nur der folgende Code:

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

Wir können feststellen, `UIView.SomeProperty` dass im Wert `IsDirectBinding` von keine Konstante ist und nicht Inline sein kann:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Allerdings ist es möglich, alle Typen in der APP zu überprüfen und festzustellen, dass keine Typen vorhanden sind, die `NSUrl`von erben, und daher ist es sicher, `IsDirectBinding` den Wert in eine `true`Konstante Inline zu bringen:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Insbesondere wird die folgende Art von Code durch diese Optimierung geändert (Dies ist der Bindungs Code für `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

in Folgendes (wenn festgestellt werden kann, dass in der APP keine Unterklassen `NSUrl` von vorhanden sind):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Sie ist für xamarin. IOS immer standardmäßig aktiviert und für xamarin. Mac immer standardmäßig deaktiviert (da Assemblys in xamarin. Mac dynamisch geladen werden können, ist es nicht möglich, zu bestimmen, ob eine bestimmte Klasse niemals untergeordnet ist).

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]inline-isdirectbinding` Sie an mtouchscreen/MMP übergeben werden.

## <a name="inline-runtimearch"></a>Inline Laufzeit. Arch

Durch diese Optimierung wird der folgende Codetyp geändert:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Im folgenden (bei der Erstellung für das Gerät):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Er ist für xamarin. IOS immer standardmäßig aktiviert (er ist für xamarin. Mac nicht verfügbar).

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]inline-runtime-arch` es an mtouchscreen übergeben wird.

## <a name="dead-code-elimination"></a>Beseitigung von unzustellbaren

Durch diese Optimierung wird der folgende Codetyp geändert:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

in

```csharp
Console.WriteLine ("Doing this");
```

Außerdem werden Konstante Vergleiche wie folgt ausgewertet:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

und stellen Sie fest, `8 == 8` dass der Ausdruck immer true ist, und reduzieren Sie ihn auf:

```csharp
Console.WriteLine ("Doing this");
```

Dies ist eine leistungsstarke Optimierung, wenn Sie in Verbindung mit den Inlining-Optimierungen verwendet wird, da Sie den folgenden Codetyp transformieren kann (Dies ist `NFCIso15693ReadMultipleBlocksConfiguration.Range`der Bindungs Code für):

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

in diesen Fall (bei der Erstellung für ein 64-Bit-Gerät und wenn auch sichergestellt werden kann `NFCIso15693ReadMultipleBlocksConfiguration` , dass in der APP keine Unterklassen vorhanden sind):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Der AOT-Compiler ist bereits in der Lage, unzustellbaren Code wie diesen zu eliminieren. diese Optimierung erfolgt jedoch im Linker. Dies bedeutet, dass der Linker sehen kann, dass mehrere Methoden nicht mehr verwendet werden und daher entfernt werden können (sofern nicht an anderer Stelle verwendet). :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Sie ist immer standardmäßig aktiviert (wenn der Linker aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]dead-code-elimination` Sie an mtouchscreen/MMP übergeben werden.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Optimieren von Aufrufen von blockliteral. setupblock

Die xamarin. IOS/Mac-Laufzeit muss die Block Signatur kennen, wenn ein Ziel-C-Block für einen verwalteten Delegaten erstellt wird. Dies kann ein ziemlich kostspieliger Vorgang sein. Durch diese Optimierung wird die Block Signatur zur Buildzeit berechnet und die Il so geändert, `SetupBlock` dass eine Methode aufgerufen wird, die die Signatur stattdessen als Argument annimmt. Dadurch entfällt die Notwendigkeit, die Signatur zur Laufzeit zu berechnen.

Benchmarks zeigen, dass dies das Aufrufen eines Blocks mit einem Faktor von 10 bis 15 beschleunigt.

Der folgende [Code](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211)wird transformiert:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

in

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Sie ist standardmäßig aktiviert, wenn die statische Registrierungsstelle verwendet wird (in xamarin. IOS ist die statische Registrierungsstelle standardmäßig für gerätebuilds aktiviert, und in xamarin. Mac ist die statische Registrierungsstelle für Releasebuilds standardmäßig aktiviert).

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]blockliteral-setupblock` Sie an mtouchscreen/MMP übergeben werden.

## <a name="optimize-support-for-protocols"></a>Optimieren der Unterstützung für Protokolle

Die xamarin. IOS/Mac-Laufzeit benötigt Informationen darüber, wie verwaltete Typen Ziele-C-Protokolle implementieren. Diese Informationen werden in Schnittstellen (und Attributen auf diesen Schnittstellen) gespeichert, die kein sehr effizientes Format aufweisen und auch nicht Linker-freundlich sind.

Ein Beispiel hierfür ist, dass diese Schnittstellen Informationen zu allen protokollmembern in einem `[ProtocolMember]` -Attribut speichern, die unter anderem Verweise auf die Parametertypen dieser Member enthalten. Dies bedeutet, dass die einfache Implementierung einer solchen Schnittstelle dazu führt, dass der Linker alle in dieser Schnittstelle verwendeten Typen beibehält, auch bei optionalen Membern, die die APP nie aufruft oder implementiert.

Durch diese Optimierung speichert die statische Registrierungsstelle alle erforderlichen Informationen in einem effizienten Format, das wenig Arbeitsspeicher verwendet, der zur Laufzeit leicht und schnell auffindbar ist.

Außerdem wird der Linker angewiesen, dass diese Schnittstellen und keine der verknüpften Attribute unbedingt beibehalten werden müssen.

Diese Optimierung erfordert, dass sowohl der Linker als auch die statische Registrierungsstelle aktiviert werden.

Bei xamarin. IOS ist diese Optimierung standardmäßig aktiviert, wenn sowohl der Linker als auch die statische Registrierungsstelle aktiviert sind.

Bei xamarin. Mac ist diese Optimierung nicht standardmäßig aktiviert, da xamarin. Mac das dynamische Laden von Assemblys unterstützt. diese Assemblys sind bei der Buildzeit möglicherweise nicht bekannt (und somit nicht optimiert).

Das Standardverhalten kann überschrieben werden, indem `--optimize=-register-protocols` Sie an mtouchscreen/MMP übergeben werden.

## <a name="remove-the-dynamic-registrar"></a>Entfernen der dynamischen Registrierungsstelle

Sowohl das xamarin. IOS-als auch das xamarin. Mac-Lauf Zeit Modul unterstützen die [Registrierung verwalteter Typen](~/ios/internals/registrar.md) mit der Ziel-C-Laufzeit. Dies kann entweder zur Buildzeit oder zur Laufzeit (oder teilweise zur Buildzeit und zur Laufzeit zur Laufzeit) erfolgen. Wenn Sie jedoch zur Buildzeit vollständig abgeschlossen ist, bedeutet dies, dass der unterstützende Code zur Laufzeit entfernt werden kann. Dies führt zu einer erheblichen Verringerung der APP-Größe, insbesondere für kleinere apps wie Erweiterungen oder watchos-apps.

Diese Optimierung erfordert, dass sowohl die statische Registrierungsstelle als auch der Linker aktiviert werden.

Der Linker versucht zu bestimmen, ob die dynamische Registrierungsstelle sicher entfernt werden kann, und wenn dies der Fall ist, wird versucht, Sie zu entfernen.

Da xamarin. Mac das dynamische Laden von Assemblys zur Laufzeit unterstützt (die zur Buildzeit nicht bekannt waren), ist es nicht möglich, zur Buildzeit zu bestimmen, ob es sich um eine sichere Optimierung handelt. Dies bedeutet, dass diese Optimierung für xamarin. Mac-apps nie standardmäßig aktiviert ist.

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]remove-dynamic-registrar` Sie an mtouchscreen/MMP übergeben werden.

Wenn der Standardwert außer Kraft gesetzt wird, um die dynamische Registrierungsstelle zu entfernen, gibt der Linker Warnungen aus, wenn er feststellt, dass er nicht sicher ist (die dynamische Registrierungsstelle wird jedoch trotzdem entfernt).

## <a name="inline-runtimedynamicregistrationsupported"></a>Inline Laufzeit. dynamicregistrationsupported

Inlines den Wert von `Runtime.DynamicRegistrationSupported` , der zur Buildzeit bestimmt wird.

Wenn die dynamische Registrierungsstelle entfernt wird (siehe " [Entfernen der dynamischen Registrierungs](#remove-the-dynamic-registrar) Stelle"), handelt es `false` sich hierbei um einen konstanten Wert; `true` andernfalls handelt es sich um einen konstanten Wert.

Durch diese Optimierung wird der folgende Codetyp geändert:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

in Folgendes, wenn die dynamische Registrierungsstelle entfernt wird:

```csharp
throw new Exception ("dynamic registration is not supported");
```

in Folgendes, wenn die dynamische Registrierungsstelle nicht entfernt wird:

```csharp
Console.WriteLine ("do something");
```

Diese Optimierung erfordert, dass der Linker aktiviert ist, und wird nur auf Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` -Attribut angewendet.

Sie ist immer standardmäßig aktiviert (wenn der Linker aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]inline-dynamic-registration-supported` Sie an mtouchscreen/MMP übergeben werden.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Precompute-Methoden zum Erstellen verwalteter Delegaten für Ziel-C-Blöcke

Wenn Ziel-C einen Selektor aufruft, der einen-Block als Parameter annimmt, und der verwaltete Code diese Methode überschreibt, muss die xamarin. IOS/xamarin. Mac-Laufzeit einen Delegaten für diesen Block erstellen.

Der Bindungs Code, der vom Bindungs Generator generiert wird, `[BlockProxy]` enthält ein-Attribut, das den Typ `Create` mit einer Methode angibt, die dies tun kann.

Der folgende Ziel-C-Code wird angegeben:

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

und den folgenden Bindungs Code:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

der Generator erstellt Folgendes:

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }

    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...

    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;

        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }

    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Wenn Ziel-C aufruft `[ObjCBlockTester callClassCallback]`, betrachtet die xamarin. IOS/xamarin. Mac-Laufzeit das `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` -Attribut für den-Parameter. Anschließend wird die `Create` -Methode für diesen Typ gesucht und diese Methode aufgerufen, um den Delegaten zu erstellen.

Diese Optimierung findet die `Create` Methode zur Buildzeit, und die statische Registrierungsstelle generiert Code, der die Methode zur Laufzeit durchsucht, indem die Metadatentoken verwendet werden, wobei das Attribut und die Reflektion verwendet werden (Dies ist viel schneller und ermöglicht es dem Linker auch , um den entsprechenden Lauf Zeit Code zu entfernen, sodass die APP kleiner wird.)

Wenn MMP/mberührungs die `Create` Methode nicht finden kann, wird eine MT4174/MM4174-Warnung angezeigt, und die Suche wird stattdessen zur Laufzeit ausgeführt.
Die wahrscheinlichste Ursache besteht darin, den Bindungs Code ohne die `[BlockProxy]` erforderlichen Attribute manuell zu schreiben.

Diese Optimierung erfordert, dass die statische Registrierungsstelle aktiviert ist.

Sie ist immer standardmäßig aktiviert (solange die statische Registrierungsstelle aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem `--optimize=[+|-]static-delegate-to-block-lookup` Sie an mtouchscreen/MMP übergeben werden.
