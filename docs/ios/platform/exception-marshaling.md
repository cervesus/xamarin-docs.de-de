---
title: Ausnahme Marshalling in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie in einer xamarin. IOS-App mit nativen und verwalteten Ausnahmen arbeiten. Er erläutert Probleme, die auftreten können, und eine Lösung für diese Probleme.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/05/2017
ms.openlocfilehash: 936c5b91a27fed1c00f3cf0c61d0184d5532c25a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753088"
---
# <a name="exception-marshaling-in-xamarinios"></a>Ausnahme Marshalling in xamarin. IOS

_Xamarin. IOS enthält neue Ereignisse zur Reaktion auf Ausnahmen, insbesondere in nativem Code._

Sowohl verwalteter Code als auch Ziel-C verfügen über Unterstützung für Lauf Zeit Ausnahmen (try/catch/endlich-Klauseln).

Ihre Implementierungen unterscheiden sich jedoch, was bedeutet, dass die Laufzeitbibliotheken (die Mono-Laufzeit und die Ziel-C-Laufzeitbibliotheken) Probleme verursachen, wenn Sie Ausnahmen behandeln und dann Code ausführen müssen, der in anderen Sprachen geschrieben wurde.

In diesem Dokument werden die Probleme, die auftreten können, und die möglichen Lösungen erläutert.

Sie enthält auch ein Beispiel Projekt, das [Ausnahme](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)Marshalling, das verwendet werden kann, um verschiedene Szenarien und deren Lösungen zu testen.

## <a name="problem"></a>Problem

Das Problem tritt auf, wenn eine Ausnahme ausgelöst wird. während der Stapel Auflösung wird ein Frame gefunden, der nicht mit dem Typ der ausgelösten Ausnahme identisch ist.

Ein typisches Beispiel hierfür für xamarin. IOS oder xamarin. Mac ist, wenn eine Native API eine "Ziel-c"-Ausnahme auslöst und diese "Ziel-c"-Ausnahme irgendwie behandelt werden muss, wenn der Stapel Entwicklungsprozess einen verwalteten Frame erreicht.

Die Standardaktion besteht darin, nichts zu Unternehmen. Im obigen Beispiel bedeutet dies, dass die Ziel-C-Laufzeit verwaltete Frames entladen soll. Das ist problematisch, da die Ziel-C-Laufzeit nicht weiß, wie verwaltete Frames entladen werden. Beispielsweise werden keine `catch` -oder `finally` -Klauseln in diesem Frame ausgeführt.

### <a name="broken-code"></a>Fehlerhafter Code

Beachten Sie das folgende Codebeispiel:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Dadurch wird eine "Ziel-C nsinvalidargumentexception" in nativem Code ausgelöst:

```
NSInvalidArgumentException *** setObjectForKey: key cannot be nil
```

Die Stapel Überwachung sieht etwa folgendermaßen aus:

```
0   CoreFoundation          __exceptionPreprocess + 194
1   libobjc.A.dylib         objc_exception_throw + 52
2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
3   libobjc.A.dylib         objc_msgSend + 102
4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()
```

Frames 0-3 sind systemeigene Frames, und der Stapel entwinder in der Ziel-C-Laufzeit _kann_ diese Frames entladen. Insbesondere führt Sie alle Ziele-C `@catch` -oder `@finally` -Klauseln aus.

Der Entlader des Ziel-C-Stapels ist jedoch _nicht_ in der Lage, die verwalteten Frames (Frames 4-6) ordnungsgemäß zu entladen, da die Frames entladen werden, aber verwaltete Ausnahme Logik nicht ausgeführt wird.

Dies bedeutet, dass es in der Regel nicht möglich ist, diese Ausnahmen auf folgende Weise abzufangen:

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

Dies liegt daran, dass der Entlader des Ziel-C-Stapels die `catch` verwaltete Klausel nicht kennt und keine `finally` der Klauseln ausgeführt wird.

Wenn das obige Codebeispiel effektiv _ist_ , liegt dies daran, dass "Ziel-c" eine Methode für die Benachrichtigung über nicht behandelte Ziel- [`NSSetUncaughtExceptionHandler`][2]c-Ausnahmen,, die von xamarin. IOS und xamarin. Mac verwendet werden, und zu diesem Zeitpunkt versucht, beliebige Ziel-c-Ausnahmen zu konvertieren. zu verwalteten Ausnahmen.

## <a name="scenarios"></a>Szenarien

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Szenario 1: Abfangen von Ziel-C-Ausnahmen mit einem verwalteten catch-Handler

Im folgenden Szenario ist es möglich, Ziel-C-Ausnahmen mithilfe von verwalteten `catch` Handlern abzufangen:

1. Eine "Ziel-C"-Ausnahme wird ausgelöst.
2. Die Ziel-C-Laufzeit führt den Stapel durch (aber nicht entladen), sucht nach einem `@catch` systemeigenen Handler, der die Ausnahme behandeln kann.
3. Die Ziel-C-Laufzeit findet keine `@catch` Handler, ruft `NSGetUncaughtExceptionHandler`auf und ruft den Handler auf, der von xamarin. IOS/xamarin. Mac installiert wurde.
4. Der Handler von xamarin. IOS/xamarin. Mac konvertiert die "Ziel-C"-Ausnahme in eine verwaltete Ausnahme und löst Sie aus. Da die Ziel-c-Laufzeit den Stapel nicht entladen hat (nur durchlaufen), ist der aktuelle Frame derselbe, in dem die Ziel-c-Ausnahme ausgelöst wurde.

Ein anderes Problem tritt auf, weil die Mono-Laufzeit nicht weiß, wie Sie die Frame-C-Frames ordnungsgemäß entladen.

Wenn xamarin. IOS ' nicht abgefangener Ziel-C-Ausnahme Rückruf aufgerufen wird, sieht der Stapel wie folgt aus:

```
 0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
 1 CoreFoundation           __handleUncaughtException + 809
 2 libobjc.A.dylib          _objc_terminate() + 100
 3 libc++abi.dylib          std::__terminate(void (*)()) + 14
 4 libc++abi.dylib          __cxa_throw + 122
 5 libobjc.A.dylib          objc_exception_throw + 337
 6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
 7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
 8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
 9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]
```

Hier sind die einzigen verwalteten Frames Frames 8-10, aber die verwaltete Ausnahme wird in Frame 0 ausgelöst. Dies bedeutet, dass die Mono-Laufzeit die systemeigenen Frames 0-7 entladen muss. Dies führt zu einem Problem, das dem oben beschriebenen Problem entspricht: Obwohl die Mono-Laufzeit die systemeigenen Frames entladen wird, `@catch` werden `@finally` keine Ziele-C-oder-Klauseln ausgeführt. .

Code Beispiel:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

Und die `@finally` -Klausel wird nicht ausgeführt, da die Mono-Laufzeit, die diesen Frame entlädt, nicht weiß.

Eine Variation hierfür besteht darin, eine verwaltete Ausnahme in verwaltetem Code auszulösen und dann durch systemeigene Frames zu entwickeln, um zur ersten `catch` verwalteten Klausel zu gelangen:

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

Die verwaltete `UIApplication:Main` Methode ruft die native `UIApplicationMain` -Methode auf, und dann führt IOS eine Menge System eigener Codeausführung aus, bevor Sie die `AppDelegate:FinishedLaunching` verwaltete-Methode aufruft, wobei nach wie vor viele Native Frames auf dem Stapel verwendet werden, wenn die verwaltete Ausnahme geraten

```
 0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
 1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
 2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
 5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
 6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
 7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
 8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
 9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
17: FrontBoardServices      -[FBSSerialQueue _performNext]
18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
21: CoreFoundation          __CFRunLoopDoSources0
22: CoreFoundation          __CFRunLoopRun
23: CoreFoundation          CFRunLoopRunSpecific
24: CoreFoundation          CFRunLoopRunInMode
25: UIKit                   -[UIApplication _run]
26: UIKit                   UIApplicationMain
27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])
```

Die Frames 0-1 und 27-30 werden verwaltet, während alle dazwischen liegen. Wenn Mono diese Frames durchläuft, werden keine Ziel-C `@catch` - `@finally` oder-Klauseln ausgeführt.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Szenario 2: nicht in der Lage, Ziel-C-Ausnahmen abzufangen

Im folgenden Szenario ist es _nicht_ möglich, Ziel-c-Ausnahmen mithilfe von verwalteten `catch` Handlern abzufangen, weil die Ausnahme "Ziel-c" auf andere Weise behandelt wurde:

1. Eine "Ziel-C"-Ausnahme wird ausgelöst.
2. Die Ziel-C-Laufzeit führt den Stapel durch (aber nicht entladen), sucht nach einem `@catch` systemeigenen Handler, der die Ausnahme behandeln kann.
3. Die Ziel-C-Laufzeit findet `@catch` einen Handler, entlädt den Stapel und beginnt mit der `@catch` Ausführung des Handlers.

Dieses Szenario wird häufig in xamarin. IOS-apps gefunden, da es im Haupt Thread normalerweise Code wie den folgenden gibt:

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

Dies bedeutet, dass es im Haupt Thread nie wirklich zu einer unbehandelten Ziel-c-Ausnahme kommt und dass der Rückruf, der die Ziel-c-Ausnahmen in verwaltete Ausnahmen konvertiert, nie aufgerufen wird.

Dies ist auch beim Debuggen von xamarin. Mac-apps auf einer früheren macOS-Version üblich, als xamarin. Mac unterstützt, da bei der Überprüfung der meisten UI-Objekte im Debugger versucht wird, Eigenschaften abzurufen, die auf der ausgeführten Plattform nicht vorhanden sind. Da xamarin. Mac Unterstützung für eine höhere macOS-Version bietet). Das aufrufen solcher Selektoren löst eine `NSInvalidArgumentException` ("unbekannte Auswahl an...") aus, die letztendlich dazu führt, dass der Prozess abstürzt.

Zusammenfassend lässt sich sagen, dass entweder die Ziel-C-Laufzeit oder die Mono-Lauf Zeitrahmen-entladen, die nicht für die Verarbeitung programmiert sind, zu nicht definiertem Verhalten führen können, wie z. b. Abstürze, Speicher Verluste und andere Typen unvorhersehbarer Verhalten (falsch).

## <a name="solution"></a>Lösung

In xamarin. IOS 10 und xamarin. Mac 2,10 haben wir Unterstützung für das Abfangen von verwalteten und Ziel-C-Ausnahmen für jede verwaltete native Grenze sowie für das umrechnen dieser Ausnahme in den anderen Typ hinzugefügt.

In Pseudo Code sieht es in etwa wie folgt aus:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

P/Aufruf an objc_msgSend wird abgefangen, und stattdessen wird dies aufgerufen:

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

Und Ähnliches gilt für den umgekehrten Fall (Mars Hallen von verwalteten Ausnahmen zu Ziel-C-Ausnahmen).

Das Abfangen von Ausnahmen auf der verwalteten nativen Grenze ist nicht kostenfrei, daher ist es nicht immer standardmäßig aktiviert:

- Xamarin. IOS/tvos: das Abfangen von Ziel-C-Ausnahmen ist im Simulator aktiviert.
- Xamarin. watchos: die Abfang Funktion wird in allen Fällen erzwungen, da das Ausführen von verwalteten Frames durch die Ziel-C-Laufzeitumgebung das Garbage Collector verwechselt und entweder nicht mehr reagiert.
- Xamarin. Mac: das Abfangen von Ziel-C-Ausnahmen ist für Debugbuilds aktiviert.

Im Abschnitt " [buildzeitflags](#build_time_flags) " wird erläutert, wie die Abfang Funktion aktiviert wird, wenn Sie nicht standardmäßig aktiviert ist.

## <a name="events"></a>Ereignisse

Wenn eine Ausnahme abgefangen wird, werden zwei neue Ereignisse ausgelöst: `Runtime.MarshalManagedException` und. `Runtime.MarshalObjectiveCException`

An beide Ereignisse wird ein `EventArgs` Objekt mit der ursprünglichen Ausnahme, die ausgelöst wurde (die `Exception` -Eigenschaft), und `ExceptionMode` einer-Eigenschaft, mit der definiert wird, wie die Ausnahme gemarshallt werden soll, übermittelt.

Die `ExceptionMode` -Eigenschaft kann im-Ereignishandler geändert werden, um das Verhalten gemäß einer benutzerdefinierten Verarbeitung im-Handler zu ändern. Ein Beispiel wäre, den Prozess abzubrechen, wenn eine bestimmte Ausnahme auftritt.

Das ändern `ExceptionMode` der Eigenschaft gilt für das einzelne Ereignis und wirkt sich nicht auf Ausnahmen aus, die in der Zukunft abgefangen werden.

Die folgenden Modi sind verfügbar:

- `Default`: Der Standardwert variiert je nach Plattform. Dies ist `ThrowObjectiveCException` der Fall, wenn sich die GC im kooperativen Modus (watchos `UnwindNativeCode` ) befindet, und andernfalls (IOS/watchos/macOS). Der Standardwert kann sich in Zukunft ändern.
- `UnwindNativeCode`: Dies ist das vorherige (undefinierte) Verhalten. Diese Option ist nicht verfügbar, wenn die GC im kooperativen Modus verwendet wird (Dies ist die einzige Option für watchos; daher ist dies keine gültige Option für watchos), aber Sie ist die Standardoption für alle anderen Plattformen.
- `ThrowObjectiveCException`: Konvertieren der verwalteten Ausnahme in eine "Ziel-c"-Ausnahme und Auslösen der "Ziel-c"-Ausnahme. Dies ist die Standardeinstellung für watchos.
- `Abort`: Brechen Sie den Prozess ab.
- `Disable`: Deaktiviert das Abfangen von Ausnahmen, daher ist es nicht sinnvoll, diesen Wert im Ereignishandler festzulegen, aber sobald das Ereignis ausgelöst wird, ist es zu spät, es zu deaktivieren. Falls festgelegt, verhält es sich wie `UnwindNativeCode`.

Für das Mars Hallen von Ziel-C-Ausnahmen zu verwaltetem Code sind die folgenden Modi verfügbar:

- `Default`: Der Standardwert variiert je nach Plattform. Dies ist `ThrowManagedException` der Fall, wenn sich die GC im kooperativen Modus (watchos `UnwindManagedCode` ) befindet, und andernfalls (IOS/tvos/macOS). Der Standardwert kann sich in Zukunft ändern.
- `UnwindManagedCode`: Dies ist das vorherige (undefinierte) Verhalten. Diese Option ist nicht verfügbar, wenn die GC im kooperativen Modus verwendet wird (Dies ist der einzige gültige GC-Modus auf watchos; Dies ist daher keine gültige Option für watchos), aber dies ist die Standardeinstellung für alle anderen Plattformen.
- `ThrowManagedException`: Konvertieren Sie die "Ziel-C"-Ausnahme in eine verwaltete Ausnahme, und lösen Sie die verwaltete Ausnahme aus. Dies ist die Standardeinstellung für watchos.
- `Abort`: Brechen Sie den Prozess ab.
- `Disable`:D isables das Abfangen von Ausnahmen, ist es nicht sinnvoll, diesen Wert im Ereignishandler festzulegen, aber sobald das Ereignis ausgelöst wird, ist es zu spät, es zu deaktivieren. Falls festgelegt, wird der Prozess abgebrochen.

Um zu sehen, wann eine Ausnahme gemarshallt wird, können Sie dies tun:

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>Buildzeitflags

Es ist möglich, die folgenden Optionen an **mberührungs** (für xamarin. IOS-Apps) und **MMP** (bei xamarin. Mac-Apps) zu übergeben, die bestimmen, ob die Ausnahme Abfang Funktion aktiviert ist, und die Standardaktion festlegen, die auftreten soll:

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

Mit Ausnahme `disable`von sind diese Werte mit den `ExceptionMode` Werten identisch, die an das `MarshalManagedException` -Ereignis `MarshalObjectiveCException` und das-Ereignis übermittelt werden.

Mit `disable` der Option wird die Abfang Funktion _größtenteils_ deaktiviert, außer wenn kein Ausführungs Aufwand hinzugefügt wird. Die marshallingereignisse werden für diese Ausnahmen weiterhin ausgelöst, wobei der Standardmodus für die ausführende Plattform der Standardmodus ist.

## <a name="limitations"></a>Einschränkungen

Bei dem Versuch, Ziel-C- `objc_msgSend` Ausnahmen abzufangen, werden nur P/Aufrufe an die-Funktions Familie abgefangen. Dies bedeutet, dass ein P/Aufrufen einer anderen C-Funktion, die dann alle Ziel-c-Ausnahmen auslöst, weiterhin das alte und nicht definierte Verhalten findet (Dies wird in Zukunft möglicherweise verbessert).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc

## <a name="related-links"></a>Verwandte Links

- [Ausnahme Marshalling (Beispiel)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
