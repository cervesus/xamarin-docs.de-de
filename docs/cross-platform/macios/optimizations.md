---
title: Build-Optimierungen
description: Dieses Dokument erläutert die verschiedenen Optimierungen, die zur Buildzeit für Xamarin.iOS- und Xamarin.Mac-apps angewendet werden.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: conceptdev
ms.author: crdun
ms.date: 04/16/2018
ms.openlocfilehash: f1aa805b9b7a16ad1e8af573cf4170f885eb0197
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870351"
---
# <a name="build-optimizations"></a>Build-Optimierungen

Dieses Dokument erläutert die verschiedenen Optimierungen, die zur Buildzeit für Xamarin.iOS- und Xamarin.Mac-apps angewendet werden.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Remove UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Entfernt die Aufrufe von [UIApplication.EnsureUIThread] [ 1] (für Xamarin.iOS) oder `NSApplication.EnsureUIThread` (für xamarin.Mac erläutert).

Diese Optimierung wird die folgende Art von Code ändern:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

in den folgenden:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Erstellt standardmäßig, die sie für die Veröffentlichung aktiviert ist.

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]remove-uithread-checks` zum Mtouch/Mmp.

[1]: https://docs.microsoft.com/dotnet/api/UIKit.UIApplication.EnsureUIThread

## <a name="inline-intptrsize"></a>Inline IntPtr.Size

Wird der Konstante Wert des `IntPtr.Size` entsprechend der Zielplattform.

Diese Optimierung wird die folgende Art von Code ändern:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

in den folgenden (Wenn für eine 64-Bit-Plattform zu erstellen):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Standardmäßig ist aktiviert, wenn auf eine einzelne Architektur oder für die Plattformassembly (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** oder  **"Xamarin.Mac.dll"**).

Wenn mehrere Architekturen verwenden zu können, wird diese Optimierung verschiedene Assemblys für die 32-Bit-Version und 64-Bit-Version der app erstellt, und beide Versionen müssen Sie in der app, die effektiv die endgültige app vergrößern, statt abzunehmen enthalten sein Es ist.

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-intptr-size` zum Mtouch/Mmp.

## <a name="inline-nsobjectisdirectbinding"></a>Inline NSObject.IsDirectBinding

`NSObject.IsDirectBinding` ist eine Instanzeigenschaft, der bestimmt, ob eine bestimmte Instanz eines Typs Wrapper oder nicht (ein Wrappertyp ist ein verwalteter Typ, der in einen systemeigenen Typ zugeordnet, für die verwaltete Instanz `UIKit.UIView` Zuordnungen auf den systemeigenen Typ `UIView` Typ – das Gegenteil ist ein Benutzertyp in diesem Fall `class MyUIView : UIKit.UIView` Benutzertyp wäre).

Es ist notwendig, den Wert kennen `IsDirectBinding` beim Aufrufen in Objective-C, da der Wert, welche Version der bestimmt `objc_msgSend` verwenden.

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

Wir können ermitteln, die in `UIView.SomeProperty` den Wert der `IsDirectBinding` ist keine Konstante und kann nicht eingebettet werden:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Es ist jedoch möglich, die alle Typen in der app untersuchen und feststellen, dass es keine Typen, die sind von erben `NSUrl`, und es ist daher sicherer, Inline der `IsDirectBinding` Wert, der eine Konstante `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Diese Optimierung wird vor allem folgende Art von Code ändern (Dies ist der Bindungscode für `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

in den folgenden (Wenn sie kann bestimmt werden, es keine Unterklassen von gibt `NSUrl` in der app):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Dabei handelt es sich immer für Xamarin.iOS standardmäßig aktiviert, wird standardmäßig für Xamarin.Mac immer deaktiviert (da es das dynamische Laden von Assemblys in Xamarin.Mac möglich ist, kann nicht bestimmt werden, ob eine bestimmte Klasse nicht als Unterklasse definiert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-isdirectbinding` zum Mtouch/Mmp.

## <a name="inline-runtimearch"></a>Inline-Runtime.Arch

Diese Optimierung wird die folgende Art von Code ändern:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

in den folgenden (beim für das Gerät):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer standardmäßig für Xamarin.iOS aktiviert (es ist nicht verfügbar für xamarin.Mac erläutert).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-runtime-arch` zum Mtouch.

## <a name="dead-code-elimination"></a>Beseitigung von totem code

Diese Optimierung wird die folgende Art von Code ändern:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

into:

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

und zu ermitteln, die den Ausdruck `8 == 8` ist eine immer "true", und es zu verkleinern:

```csharp
Console.WriteLine ("Doing this");
```

Dies ist eine leistungsstarke Optimierung, bei der Verwendung zusammen mit inlining Optimierungen, da es die folgende Art von Code transformieren kann (Dies ist der Bindungscode für `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

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

in diesen (, wenn für ein 64-Bit-Gerät erstellen und auch möglich, um sicherzustellen, dass keine `NFCIso15693ReadMultipleBlocksConfiguration` Unterklassen, die in der app):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Der AOT-Compiler ist bereits in der Lage, toten Code wie folgt zu vermeiden,, aber diese Optimierung erfolgt in den Linker integrieren – dies, dass den Linker können sehen bedeutet, dass es mehrere Methoden, die nicht mehr verwendet werden gibt, und können daher entfernt werden, (es sei denn, die an anderer Stelle verwendet) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer standardmäßig aktiviert (wenn der Linker aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]dead-code-elimination` zum Mtouch/Mmp.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Aufrufe von BlockLiteral.SetupBlock optimieren

Die Runtime Xamarin.iOS/Mac muss die Blocksignatur beim Erstellen eines Blocks Objective-C für einen verwalteten Delegaten kennen. Dies ist möglicherweise eine relativ aufwändige Operation. Diese Optimierung berechnet die Blocksignatur zum Zeitpunkt der Erstellung, und ändern Sie den IL-Code zum Aufrufen einer `SetupBlock` -Methode, die die Signatur stattdessen als Argument akzeptiert. Dadurch wird die Notwendigkeit für die Berechnung der Signatur zur Laufzeit vermieden.

Vergleichtests zeigen, dass das Aufrufen von einem Block mit einem Faktor von 10 bis 15 beschleunigt.

Die folgende Transformation wird [Code](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

into:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist standardmäßig aktiviert, wenn mit der statischen Registrierungsstelle (in Xamarin.iOS, der statische Registrierungsstelle ist standardmäßig aktiviert, für die Geräte-Builds und builds in Xamarin.Mac, die die statische Registrierungsstelle für Version standardmäßig aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]blockliteral-setupblock` zum Mtouch/Mmp.

## <a name="optimize-support-for-protocols"></a>Optimieren Sie die Unterstützung für Protokolle

Die Laufzeit Xamarin.iOS/Mac benötigt Informationen zur Funktionsweise von verwalteten Typen implementiert Objective-C-Protokolle. Diese Informationen werden gespeichert, in Schnittstellen (und Attribute für diese Schnittstellen), das ist nicht sehr effizient Format und Linker geeignet ist.

Ein Beispiel hierfür ist, dass diese Schnittstellen Speichern von Informationen über alle Protokoll-Elemente in einem `[ProtocolMember]` -Attribut, das unter anderem die Verweise auf die Parametertypen für die Elemente enthalten. Dies bedeutet, dass die einfache Implementierung einer Schnittstelle kann den Linker alle in dieser Schnittstelle verwendete Typen beibehalten werden, sogar für optionale Member die app nie aufruft oder implementiert.

Diese Optimierung wird erstellt, die statische Registrierungsstelle, die alle erforderliche Informationen in einem effizienten Format zu speichern, die wenig Speicher verwendet, die einfach und schnell zur Laufzeit zu finden ist.

Sie lernen auch dem Linker, dass es nicht unbedingt diese Schnittstellen, und die zugehörigen Attribute beibehalten.

Diese Optimierung erfordert der Linker sowohl die statische Registrierung aktiviert werden.

In Xamarin.iOS ist diese Optimierung standardmäßig aktiviert, wenn sowohl der Linker und der statischen Registrierungsstelle aktiviert sind.

Auf Xamarin.Mac ist diese Optimierung nicht standardmäßig aktiviert, da Xamarin.Mac unterstützt das Laden von Assemblys dynamisch, und diese Assemblys können nicht zur Buildzeit bekannt (und wurden daher nicht optimiert).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=-register-protocols` zum Mtouch/Mmp.

## <a name="remove-the-dynamic-registrar"></a>Die dynamische Registrierung entfernen

Sowohl die Xamarin.iOS- und die Xamarin.Mac-Laufzeit umfassen Unterstützung für [Registrieren von verwalteten Typen](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) mit Objective-C-Laufzeit. Es kann entweder ausgeführt werden, zum Zeitpunkt der Erstellung oder zur Laufzeit (oder teilweise auf den Zeitpunkt der Erstellung und den Rest zur Laufzeit), aber wenn dies zum Zeitpunkt der Erstellung vollständig abgeschlossen ist, es bedeutet, dass des unterstützenden Codes für die sie zur Laufzeit entfernt werden kann. Dies führt zu einer signifikanten Abfall der app-Größe, insbesondere für kleinere apps wie z. B. Erweiterungen oder WatchOS-apps.

Diese Optimierung ist erforderlich, sowohl die statische Registrierungsstelle und der Linker aktiviert werden.

Der Linker versucht zu bestimmen, ob es sicher ist, entfernen Sie die dynamische Registrierung, und wenn dies der Fall versucht, ihn zu entfernen ist.

Da Xamarin.Mac unterstützt dynamisch Laden von Assemblys zur Laufzeit (der zum Zeitpunkt der Erstellung nicht bekannt waren), ist es unmöglich, die zum Zeitpunkt der Erstellung zu bestimmen, ob es sich um eine sichere Optimierung handelt. Dies bedeutet, dass diese Optimierung nicht für Xamarin.Mac-apps standardmäßig aktiviert ist.

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]remove-dynamic-registrar` zum Mtouch/Mmp.

Wenn der Standardwert überschrieben wird, um die dynamische Registrierung zu entfernen, gibt der Linker Warnungen, wenn erkannt wird, dass es ist nicht sicher (aber die dynamische Registrierung noch entfernt werden).

## <a name="inline-runtimedynamicregistrationsupported"></a>Inline-Runtime.DynamicRegistrationSupported

Wird der Wert des `Runtime.DynamicRegistrationSupported` , die zum Zeitpunkt der Erstellung festgelegt.

Wenn die dynamische Registrierung entfernt wird (finden Sie unter den [entfernen die dynamischen Registrierungsstelle](#remove-the-dynamic-registrar) Optimierung), dies ist eine Konstante `false` Wert, der andernfalls handelt es sich um eine Konstante `true` Wert.

Diese Optimierung wird die folgende Art von Code ändern:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

in den folgenden, wenn die dynamische Registrierung entfernt wird:

```csharp
throw new Exception ("dynamic registration is not supported");
```

in den folgenden, wenn die dynamische Registrierung nicht entfernt wird:

```csharp
Console.WriteLine ("do something");
```

Diese Optimierung erfordert den Linker aktiviert werden, und gilt nur für Methoden mit dem `[BindingImpl (BindingImplOptions.Optimizable)]` Attribut.

Es ist immer standardmäßig aktiviert (wenn der Linker aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]inline-dynamic-registration-supported` zum Mtouch/Mmp.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Führt eine vorberechnung Methoden zum Erstellen von verwalteter Delegaten für Objective-C-Blöcke

Wenn Objective-C einen Selektor aufruft, einen Block als Parameter, und klicken Sie dann verwalteter Code hat außer Kraft gesetzte diese Methode, die Xamarin.iOS / Xamarin.Mac Runtime muss einen Delegaten für den Block zu erstellen.

Der generierte vom Generator Bindung Bindungscode enthält eine `[BlockProxy]` -Attribut, das den Typ mit gibt an, eine `Create` -Methode, die dies tun kann.

Betrachten Sie den folgenden Objective-C-Code:

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

Wenn Objective-C-Aufrufe `[ObjCBlockTester callClassCallback]`, die Xamarin.iOS / Xamarin.Mac-Laufzeit wird erläutert, wie die `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` Attribut für den Parameter. Anschließend überprüft er, um die `Create` Methode für diesen Typ und Aufrufen dieser Methode auf, um den Delegaten zu erstellen.

Diese Optimierung findet die `Create` Methode am Zeitpunkt der Erstellung und der statischen Registrierungsstelle generiert Code, der die Methode zur Laufzeit mit die Metadatentoken anstelle des Attributs und Reflektion (Dies ist wesentlich schneller und ermöglicht auch das den Linker sucht den entsprechende Common Language Runtime-Code, wodurch die app auch kleinere entfernen).

Wenn Mmp/Mtouch wurde nicht gefunden wird. die `Create` Methode, klicken Sie dann eine MT4174/MM4174 Warnung wird angezeigt, und die Suche wird stattdessen zur Laufzeit ausgeführt werden.
Die wahrscheinlichste Ursache ist ohne die erforderliche Code für die Datenbindung manuell geschrieben `[BlockProxy]` Attribute.

Diese Optimierung erfordert die statische Registrierung aktiviert werden.

Es ist immer standardmäßig aktiviert (solange die statische Registrierung aktiviert ist).

Das Standardverhalten kann überschrieben werden, indem übergeben `--optimize=[+|-]static-delegate-to-block-lookup` zum Mtouch/Mmp.
