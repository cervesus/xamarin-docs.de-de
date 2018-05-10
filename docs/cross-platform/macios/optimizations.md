---
title: Erstellen von Optimierungen
description: Dieses Dokument erläutert die verschiedenen Optimierungen, die während des Buildvorgangs für Xamarin.iOS und Xamarin.Mac apps angewendet werden.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: bradumbaugh
ms.author: brumbaug
dateupdated: 04/16/2018
ms.openlocfilehash: abdd1223c0105156580b8f23fc2c020f2f45caa6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="build-optimizations"></a>Erstellen von Optimierungen

Dieses Dokument erläutert die verschiedenen Optimierungen, die während des Buildvorgangs für Xamarin.iOS und Xamarin.Mac apps angewendet werden.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Entfernen Sie UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Entfernt die Aufrufe an [UIApplication.EnsureUIThread] [ 1] (für Xamarin.iOS) oder `NSApplication.EnsureUIThread` (für Xamarin.Mac).

Diese Optimierung ändert sich auf folgende Art von Code:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

in der folgenden:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Standardmäßig ist dieser für die Veröffentlichung aktiviert wird erstellt.

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]remove-uithread-checks` zu Mtouch/Mmp.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>Inline-IntPtr.Size

Inlines den konstanten Wert des `IntPtr.Size` entsprechend der Zielplattform.

Diese Optimierung ändert sich auf folgende Art von Code:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

in der folgenden (Wenn für eine 64-Bit-Plattform zu erstellen):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Standardmäßig ist aktiviert, wenn eine einzelne-Architektur abzielen, oder für die Plattformassembly (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** oder  **Xamarin.Mac.dll**).

Wenn mehrere Architekturen abzielen, wird diese Optimierung verschiedene Assemblys für die 32-Bit-Version und die 64-Bit-Version der app erstellt, und beide Versionen ist, in der app, die effektiv die endgültigen app vergrößern, statt abzunehmen eingeschlossen werden sollen Es ist.

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-intptr-size` zu Mtouch/Mmp.

## <a name="inline-nsobjectisdirectbinding"></a>Inline-NSObject.IsDirectBinding

`NSObject.IsDirectBinding` ist eine Instanzeigenschaft, der bestimmt, ob eine bestimmte Instanz eines Wrappertyps oder nicht (ein Wrappertyp ist ein verwalteter Typ, der ein systemeigener Typ zugeordnet; für die verwaltete Instanz `UIKit.UIView` geben Zuordnungen dem systemeigenen `UIView` Typ - genau das Gegenteil ist eine Benutzertyp in diesem Fall `class MyUIView : UIKit.UIView` ein Benutzertyp wäre).

Es ist notwendig, den Wert `IsDirectBinding` beim Aufrufen von in Objective-C, da der Wert, welche Version von bestimmt `objc_msgSend` verwenden.

Betrachten Sie nur den folgenden Code:

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

Wir können Sie ermitteln, in `UIView.SomeProperty` den Wert des `IsDirectBinding` ist keine Konstante und kann nicht als Inlinefunktion umgesetzt werden:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Allerdings ist es möglich, in der alle Typen in der app suchen und bestimmen, dass es keine Typen, die sind von erben `NSUrl`, und daher ist es sicher Inline zu setzen die `IsDirectBinding` Wert auf eine Konstante `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Diese Optimierung wird insbesondere folgende Art von Code ändern (Dies ist der Code für die Datenbindung für `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

in der folgenden (Wenn sie kann bestimmt werden, es keine Unterklassen von gibt `NSUrl` in der app):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer für Xamarin.iOS standardmäßig aktiviert und für Xamarin.Mac immer standardmäßig deaktiviert (ist es möglich, Assemblys in Xamarin.Mac dynamisch geladen wird, kann nicht bestimmt werden, ob eine bestimmte Klasse nie als Unterklasse ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-isdirectbinding` zu Mtouch/Mmp.

## <a name="inline-runtimearch"></a>Inline-Runtime.Arch

Diese Optimierung ändert sich auf folgende Art von Code:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

in der folgenden (Erstellung für Gerät):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer in der Standardeinstellung für Xamarin.iOS aktiviert (es ist nicht verfügbar für Xamarin.Mac).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-runtime-arch` auf Mtouch.

## <a name="dead-code-elimination"></a>Totem Code für Eliminierung von Duplikaten

Diese Optimierung ändert sich auf folgende Art von Code:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

in:

```csharp
Console.WriteLine ("Doing this");
```

Es wird auch Konstante vergleichen, wie folgt ausgewertet:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

und zu bestimmen, die den Ausdruck `8 == 8` ist eine immer "true", und es zu verringern:

```csharp
Console.WriteLine ("Doing this");
```

Dies ist eine leistungsstarke Optimierung bei Verwendung zusammen mit der inlining Optimierungen, da er den folgenden Typ des Codes transformieren kann (Dies ist der Code für die Datenbindung für `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

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

in diesen (Wenn für ein 64-Bit-Gerät erstellen, und können auch sicher, dass sich keine `NFCIso15693ReadMultipleBlocksConfiguration` Unterklassen in der app):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Der Compiler AOT ist bereits in der Lage, inaktiven Code wie folgt zu vermeiden, führen Sie jedoch diese Optimierung erfolgt innerhalb des Linkers, der Dies bedeutet, den Linker können dass Sie feststellen, ob es mehrere Methoden, die nicht mehr verwendet und können deshalb entfernt werden gibt, (es sei denn, die an anderer Stelle verwendet) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer standardmäßig aktiviert (wenn der Linker aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]dead-code-elimination` zu Mtouch/Mmp.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Aufrufe von BlockLiteral.SetupBlock optimieren

Die Laufzeit Xamarin.iOS/Mac muss die Blocksignatur für die Erstellung eines Objective-C-Blocks für ein verwaltetes delegieren kennen. Dies ist möglicherweise eine relativ aufwändige Operation. Diese Optimierung wird während des Buildvorgangs die Blocksignatur zu berechnen, und ändern Sie den IL-Code zum Aufrufen einer `SetupBlock` -Methode, die Signatur stattdessen als Argument annimmt. Auf diese Weise wird vermieden, die Notwendigkeit für die Berechnung der Signatur zur Laufzeit.

Vergleichtests zeigen, dass dies einen Block mit einem Faktor von 10 bis 15 aufrufen beschleunigt.

Es wird Folgendes transformieren [Code](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

in:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist standardmäßig aktiviert, wenn mit der statischen Registrierungsstelle (in Xamarin.iOS statische Registrierungsstelle ist standardmäßig für Builds vom Gerät aktiviert und erstellt in Xamarin.Mac statische Registrierungsstelle für Version standardmäßig aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]blockliteral-setupblock` zu Mtouch/Mmp.

## <a name="optimize-support-for-protocols"></a>Optimieren Sie die Unterstützung für Protokolle

Die Laufzeit Xamarin.iOS/Mac benötigt Informationen zur Funktionsweise von verwalteten Typen implementiert Objective-C-Protokolle. Diese Informationen werden gespeichert, in Schnittstellen (und Attribute auf diese Schnittstellen), also nicht sehr effizient-Format, noch ist es Linker-freundliche.

Ein Beispiel hierfür ist, dass diese Schnittstellen Speichern von Informationen über alle Protokoll-Elemente in einem `[ProtocolMember]` -Attribut, das unter anderem Verweise auf die Parametertypen für die Elemente enthalten. Dies bedeutet, dass einfach implementieren eine solche Schnittstelle wird den Linker alle Typen in dieser Schnittstelle verwendet beibehalten, auch für die optionalen Elemente die app nie aufruft oder implementiert.

Diese Optimierung stellen die statische Registrierungsstelle die erforderliche Informationen in einem effiziente Format zu speichern, die wenig Arbeitsspeicher verwendet, die schnelle und einfache zur Laufzeit gefunden wird.

Es wird auch dem Linker erfahren Sie, dass es nicht unbedingt diese Schnittstellen, noch eines der verknüpften Attribute beibehalten.

Diese Optimierung erfordert sowohl der Linker und die statische Registrierungsstelle aktiviert werden.

Diese Optimierung ist auf Xamarin.iOS standardmäßig aktiviert, wenn der Linker und die statische Registrierungsstelle aktiviert sind.

Auf Xamarin.Mac ist diese Optimierung nicht standardmäßig aktiviert, da Xamarin.Mac unterstützt, die dynamisch Laden von Assemblys, und diese Assemblys möglicherweise nicht bekannt, die während des Buildvorgangs (und wurden daher nicht optimiert).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=-register-protocols` zu Mtouch/Mmp.

## <a name="remove-the-dynamic-registrar"></a>Entfernen Sie die dynamische Registrierungsstelle

Die Xamarin.iOS und die Laufzeit Xamarin.Mac enthalten Unterstützung für [Registrieren von verwalteten Typen](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) mit Objective-C-Laufzeit. Es kann entweder ausgeführt werden, während des Buildvorgangs oder zur Laufzeit (oder teilweise zur Buildzeit und den Rest Laufzeit), aber es während des Buildvorgangs vollständig abgeschlossen ist, bedeutet dies, dass die unterstützende Code dafür zur Laufzeit entfernt werden kann. Dadurch wird eine erhebliche Verringerung der Größe der app, insbesondere für kleinere z. B. Erweiterungen oder WatchOS-apps.

Diese Optimierung ist erforderlich, die statische Registrierungsstelle und der Linker aktiviert werden.

Der Linker versucht zu bestimmen, ob es sicherer ist, entfernen Sie die dynamische Registrierungsstelle und wenn dies der Fall versuchen wird, um ihn zu entfernen.

Da Xamarin.Mac unterstützt dynamisch Laden von Assemblys zur Laufzeit (die während des Buildvorgangs nicht bekannt waren), ist es unmöglich, die während des Buildvorgangs zu bestimmen, ob es sich um eine sichere Optimierung handelt. Dies bedeutet, dass diese Optimierung wird standardmäßig für Xamarin.Mac apps nie aktiviert ist.

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]remove-dynamic-registrar` zu Mtouch/Mmp.

Wenn der Standardwert überschrieben wird, um die dynamische Registrierungsstelle zu entfernen, Ausgeben der Linker Warnungen, wenn erkannt wird, dass es ist nicht sicher (aber die dynamische Registrierungsstelle werden dennoch entfernt).

## <a name="inline-runtimedynamicregistrationsupported"></a>Inline-Runtime.DynamicRegistrationSupported

Inlines den Wert der `Runtime.DynamicRegistrationSupported` , die während des Buildvorgangs festgelegt.

Wenn die dynamische Registrierungsstelle entfernt wird (finden Sie unter der [entfernen Sie die dynamische Registrierungsstelle](#remove-the-dynamic-registrar) Optimierung), dies ist eine Konstante `false` -Wert, der andernfalls handelt es sich um eine Konstante `true` Wert.

Diese Optimierung ändert sich auf folgende Art von Code:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

in das folgende, wenn die dynamische Registrierungsstelle entfernt wird:

```csharp
throw new Exception ("dynamic registration is not supported");
```

in der folgenden, wenn die dynamische Registrierungsstelle nicht entfernt wird:

```csharp
Console.WriteLine ("do something");
```

Diese Optimierung erfordert den Linker aktiviert werden soll, und gilt nur für Methoden mit den `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer standardmäßig aktiviert (wenn der Linker aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-dynamic-registration-supported` zu Mtouch/Mmp.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Führt eine vorberechnung Methoden zum Erstellen von verwalteter Delegaten für Objective-C-Blöcke

Wenn Objective-C-Auswahl, die aufruft, einen Block als Parameter, und klicken Sie dann verwalteter Code hat außer Kraft gesetzte diese Methode, die die Xamarin.iOS / Xamarin.Mac Laufzeit benötigt, um einen Delegaten für den Block zu erstellen.

Enthält die von der Bindung Generator generierten Code für die Datenbindung einer `[BlockProxy]` -Attribut, das den Typ mit gibt eine `Create` Methode, die dazu.

Betrachten Sie die folgenden Objective-C-Code:

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

und den folgenden Bindungscode:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

der Generator erstellt:

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

Wenn aufruft, Objective-C `[ObjCBlockTester callClassCallback]`, die Xamarin.iOS / Xamarin.Mac Runtime wird untersucht, die `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` Attribut für den Parameter. Wird dann Nachschlagen der `Create` Methode für diesen Typ und Aufrufen dieser Methode, um den Delegaten zu erstellen.

Diese Optimierung findet die `Create` Methode zur Buildzeit und die statische Registrierungsstelle generiert Code, der die Methode zur Laufzeit mit die Metadatentoken stattdessen mithilfe des Attributs und Reflektion (Dies ist wesentlich schneller und kann auch den Linker sucht So entfernen Sie den entsprechenden Common Language Runtime-Code, wie Sie die app kleinere erstellen.)

Wenn Mmp/Mtouch finden kann die `Create` Methode, wird eine Warnung MT4174/MM4174 angezeigt werden, und die Suche wird stattdessen zur Laufzeit ausgeführt werden.
Die wahrscheinlichste Ursache ist die Bindungscode ohne die erforderliche manuell geschrieben `[BlockProxy]` Attribute.

Diese Optimierung ist erforderlich, die statische Registrierungsstelle aktiviert werden.

Es ist immer standardmäßig aktiviert (solange die statische Registrierungsstelle aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]static-delegate-to-block-lookup` zu Mtouch/Mmp.
