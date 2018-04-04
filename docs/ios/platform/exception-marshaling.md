---
title: Ausnahme Marshalling
description: Xamarin.iOS enthält neue Ereignisse, um Hilfe zu Ausnahmen, besonders in systemeigenem Code zu reagieren.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/05/2017
ms.openlocfilehash: bb9c16985d958772193093434350435ce477956a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="exception-marshaling"></a>Ausnahme Marshalling

_Xamarin.iOS enthält neue Ereignisse, um Hilfe zu Ausnahmen, besonders in systemeigenem Code zu reagieren._

Verwalteter Code und Objective-C verfügen über die Unterstützung für Common Language Runtime-Ausnahmen (Try/Catch/finally-Klauseln).

Allerdings unterscheiden sich ihre Implementierungen, was bedeutet, dass die Common Language Runtime-Bibliotheken (Mono / Runtime und der Objective-C-Laufzeitbibliotheken) Probleme haben, um Ausnahmen zu behandeln, und führen Sie dann in anderen Sprachen geschriebenen Code.

Dieses Dokument erläutert die Probleme, die auftreten können, und die möglichen Lösungen.

Es enthält auch ein Beispielprojekt [Ausnahme Marshalling](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), die verwendet werden können, um verschiedene Szenarien und ihre Lösungen zu testen.

## <a name="problem"></a>Problem

Das Problem tritt auf, wenn eine Ausnahme wird ausgelöst, und während der stapelentladung eines Frames aufgetreten ist, die nicht den Typ der Ausnahme übereinstimmt, die ausgelöst wurde.

Ein typisches Beispiel dafür für Xamarin.iOS oder Xamarin.Mac ist, wenn eine systemeigene API löst eine Objective-C-Ausnahme aus, und klicken Sie dann diese Objective-C-Ausnahme irgendwie behandelt werden muss des Entladungsprozesses Stapel einen verwalteten Frame erreicht.

Die Standardaktion ist, geschieht nichts. Für das Beispiel oben bedeutet dies, lassen die Objective-C-Laufzeit entladen verwaltet Frames. Dies ist problematisch, da die Objective-C-Laufzeit nicht weiß, wie verwaltete Rahmen entladen kann; Es wird nicht z. B. Ausführen einer `catch` oder `finally` Klauseln in diesem Frame.

### <a name="broken-code"></a>Fehlerhafte code

Betrachten Sie das folgende Codebeispiel:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Dadurch wird ein Objective-C-NSInvalidArgumentException in systemeigenem Code ausgelöst:

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

Und die stapelüberwachung werden etwa so aussehen:

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

0-3-Frames werden systemeigene Rahmen und die stapelentlader in Objective-C-Laufzeit _können_ die Rahmen entladen. Insbesondere führt er Objective-C `@catch` oder `@finally` Klauseln.

Ist Sie jedoch die Objective-C-stapelentlader _nicht_ Entladung ordnungsgemäß verwalteten Frames (Frames 4 bis 6), werden die Frames entladen werden, jedoch nicht verwaltete Ausnahme Logik ausgeführt werden kann.

Das bedeutet, dass es in der Regel nicht möglich, diese Ausnahmen auf folgende Weise zu erfassen:

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

Dies ist, da die Objective-C-stapelentlader nicht zu den verwalteten kennt `catch` -Klausel, und weder wird die `finally` -Klausel ausgeführt werden.

Wenn im obigen Beispiel _ist_ effektiv ist da Objective-C eine Methode benachrichtigt wird, der nicht behandelten Ausnahmen für Objective-C, verfügt [ `NSSetUncaughtExceptionHandler` ] [ 2], welche Xamarin.iOS und Xamarin.Mac verwenden und zu diesem Zeitpunkt versucht, alle Objective-C-Ausnahmen in verwalteten Ausnahmen zu konvertieren.

## <a name="scenarios"></a>Szenarien

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Szenario 1: Abfangen von Ausnahmen in Objective-C mit einer verwalteten Catch-handler

Im folgenden Szenario, es ist möglich, Objective-C-Ausnahmen abfangen mit verwalteten `catch` Handler:

1. Eine Objective-C-Ausnahme ausgelöst.
2. Objective-C-Laufzeit den Stackwalk (jedoch ist es nicht entladen), suchen für ein systemeigenes `@catch` Handler, der die Ausnahme behandeln kann.
3. Objective-C-Laufzeit nicht finden kann, `@catch` Handler, ruft `NSGetUncaughtExceptionHandler`, und ruft den Handler durch Xamarin.iOS/Xamarin.Mac installiert.
4. Xamarin.iOS/Xamarin.Mac's-Handler die Objective-C-Ausnahme in eine verwaltete Ausnahme konvertieren, und wirft den Zettel. Seit der Objective-C-Laufzeit nicht entlädt die Aufrufliste (nur er vertraut gemacht), der aktuelle Frame ist dasjenige, in dem die Objective-C-Ausnahme ausgelöst wurde.

Hier tritt ein anderes Problem auf, weil die Mono-Laufzeit nicht weiß, wie Objective-C-Frames ordnungsgemäß entladen kann.

Wenn Xamarin.iOS nicht abgefangene Objective-C-Ausnahmerückruf aufgerufen wird, ist der Stapel wie folgt:

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

Hier die einzigen verwalteten Frames werden Frames 8 bis 10, aber im Frame 0 der verwaltete Ausnahme ausgelöst wird. Dies bedeutet, dass die Mono / Common Language Runtime die systemeigenen Rahmen 0-7, entladen werden muss daraufhin ein Problem, das Problem weiter oben erläuterten entspricht: Obwohl Mono / Runtime systemeigenen Rahmen entladen wird, wird keine Objective-C ausführen `@catch` oder `@finally` Klauseln .

Codebeispiel:

``` objective-c
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

Und die `@finally` -Klausel wird nicht ausgeführt werden, da die Mono-Laufzeit, die dieses Rahmens entlädt dazu nicht kennt.

Eine Variante dieses verwaltete Ausnahme in verwaltetem Code und anschließend über systemeigene Rahmen abzurufenden entladen wird auf den ersten verwalteten `catch` Klausel:

``` csharp
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

Die verwaltete `UIApplication:Main` -Methodenaufruf systemeigenen `UIApplicationMain` -Methode, und klicken Sie dann auf iOS erfolgt einen Großteil der Ausführung von systemeigenem Code bevor schließlich ein Aufruf der verwalteten `AppDelegate:FinishedLaunching` Methode noch zahlreiche systemeigene Rahmen im Stapel wird von die verwaltete Ausnahme ausgelöst:

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

Frames 0-1 und 27-30 werden verwaltet, während alle dazwischen native sind. Wenn Sie über diese Frames, die keine Objective-C Mono entlädt `@catch` oder `@finally` Klauseln ausgeführt werden.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Szenario 2 – kann nicht zum Abfangen von Ausnahmen für Objective-C

Im folgenden Szenario ist es _nicht_ Objective-C-Ausnahmen abfangen können mittels für managed `catch` Handler, da die Objective-C-Ausnahme auf andere Weise behandelt wurde:

1. Eine Objective-C-Ausnahme ausgelöst.
2. Objective-C-Laufzeit den Stackwalk (jedoch ist es nicht entladen), suchen für ein systemeigenes `@catch` Handler, der die Ausnahme behandeln kann.
3. Objective-C-Laufzeit sucht nach einem `@catch` Handler, entlädt den Stapel, und startet die Ausführung der `@catch` Handler.

Dieses Szenario wird häufig in Xamarin.iOS apps gefunden, da auf dem Hauptthread in der Regel Code wie folgt wird:

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

Dies bedeutet, dass auf den Hauptthread nie wirklich Objective-C-Ausnahmefehler ist und unsere Rückruf, der Objective-C-Ausnahmen in verwalteten Ausnahmen konvertiert werden, wird daher nie aufgerufen.

Dies ist auch üblich, das beim Debuggen Xamarin.Mac-apps auf einer früheren Version der MacOS als Xamarin.Mac unterstützt, da die meisten Benutzeroberflächenobjekte im Debugger überprüfen versucht, um auf den ausgeführten Plattform (abzurufen, Eigenschaften entsprechen, Selektoren, die nicht vorhanden Da Xamarin.Mac Unterstützung für eine höhere Version von MacOS umfasst). Aufrufen von solchen Selektoren löst ein `NSInvalidArgumentException` ("Unbekannte Selektor an gesendet..."), wodurch schließlich den Prozess abstürzen.

Zusammenfassend lässt sich sagen, dass Objective-C-Laufzeit oder die Mono-Laufzeit entladen Frames, die sie nicht auf programmiert werden Handle kann dazu führen, dass nicht definierten Verhalten, wie Abstürze, Speicherverluste und andere Arten von Verhalten unvorhersehbar (mis).

## <a name="solution"></a>Lösung

In Xamarin.iOS 10 und Xamarin.Mac 2.10 haben wir Unterstützung zum Abfangen von Ausnahmen von sowohl verwalteter als auch Objective-C auf alle verwalteten systemeigenen Grenze und für diese Ausnahme in den anderen Typ konvertieren hinzugefügt.

In Pseudocode sieht es etwa wie folgt aus:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Der P/Invoke auf Objc_msgSend abgefangen wird, und wird stattdessen aufgerufen:

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

Und etwas Ähnliches erfolgt für den umgekehrten Fall (Marshalling verwaltete Ausnahmen auf Objective-C-Ausnahmen).

Abfangen von Ausnahmen für die verwaltete systemeigene Grenze ist kein kostenlose, sodass es nicht immer standardmäßig aktiviert:

- Xamarin.iOS/tvOS: Abfangen von Ausnahmen für Objective-C im Simulator aktiviert ist.
- Xamarin.watchOS: Abfangen wird in allen Fällen erzwungen, da die Objective-C-Laufzeit-Entladung verwaltet verwirrt Frames der Garbage Collector und nehmen sie Hängenbleiben oder Absturz (Crash).
- Xamarin.Mac: Abfangen von Ausnahmen für Objective-C ist aktiviert, für Debugbuilds.

Die [zur Buildzeit Flags](#build_time_flags) Abschnitt wird erläutert, wie abfangen aktiviert, wenn es nicht standardmäßig aktiviert ist.

## <a name="events"></a>Ereignisse

Es gibt zwei neue Ereignisse, die ausgelöst werden, nachdem eine Ausnahme abgefangen wird: `Runtime.MarshalManagedException` und `Runtime.MarshalObjectiveCException`.

Werden beide Ereignisse übergeben ein `EventArgs` Objekt, das die ursprüngliche Ausnahme enthält, die ausgelöst wurde (die `Exception` Eigenschaft), und ein `ExceptionMode` Eigenschaft definieren, wie die Ausnahme gemarshallt werden sollen.

Die `ExceptionMode` Eigenschaft kann geändert werden, in der ereignismeldung Ereignishandler, um das Verhalten entsprechend jede benutzerdefinierte Verarbeitung im Handler ausgeführt. Ein Beispiel wäre, den Prozess abzubrechen, wenn eine bestimmte Ausnahme auftritt.

Ändern der `ExceptionMode` Eigenschaft gilt für einzelnes Ereignis, er wirkt sich keine Ausnahmen, die in der Zukunft abgefangen.

Die folgenden Modi sind verfügbar:

- `Default`: Der Standardwert hängt von der Plattform. Es ist `ThrowObjectiveCException` im kooperativen Modus (WatchOS), ist der globale Katalogserver und `UnwindNativeCode` andernfalls (iOS / WatchOS / MacOS). Die Standardeinstellung kann in der Zukunft ändern.
- `UnwindNativeCode`: Dies ist das Verhalten des vorherigen (nicht definiert). Dies ist nicht verfügbar, wenn der globale Katalogserver im kooperativen Modus verwendet (Dies ist die einzige Option für WatchOS; daher Dies ist keine gültige Option für WatchOS), aber es ist die Standardoption für alle anderen Plattformen.
- `ThrowObjectiveCException`: Die verwaltete Ausnahme in eine Objective-C-Ausnahme konvertieren und die Objective-C-Ausnahme auslösen. Dies ist die Standardeinstellung auf WatchOS.
- `Abort`: Der Prozess abgebrochen.
- `Disable`: Deaktiviert die Ausnahme abfangen, damit es nicht sinnvoll, diesen Wert im Ereignishandler festzulegen, nachdem das Ereignis ausgelöst wird, ist jedoch zu spät um zu deaktivieren. In jedem Fall, wenn festgelegt, es als Verhalten `UnwindNativeCode`.

Für das Marshalling von Objective-C-Ausnahmen in verwalteten Code, stehen die folgenden Modi:

- `Default`: Der Standardwert hängt von der Plattform. Es ist `ThrowManagedException` im kooperativen Modus (WatchOS), ist der globale Katalogserver und `UnwindManagedCode` andernfalls (iOS / tvos. außerdem wurden / MacOS). Die Standardeinstellung kann in der Zukunft ändern.
- `UnwindManagedCode`: Dies ist das Verhalten des vorherigen (nicht definiert). Dies ist nicht verfügbar, wenn der globale Katalogserver im kooperativen Modus verwendet (Dies ist der einzige gültige GC-Modus auf WatchOS; daher Dies ist keine gültige Option für WatchOS), aber es ist die Standardeinstellung für alle anderen Plattformen.
- `ThrowManagedException`: Die Objective-C-Ausnahme in eine verwaltete Ausnahme konvertieren und die verwaltete Ausnahme auszulösen. Dies ist die Standardeinstellung auf WatchOS.
- `Abort`: Der Prozess abgebrochen.
- `Disable`: Deaktiviert die Ausnahme abfangen, damit es sinnvoll, legen Sie nicht diesen Wert werden im Ereignisprotokoll Handler jedoch einmal auf das Ereignis wird ausgelöst, ist es zu spät um zu deaktivieren. In jedem Fall, wenn festgelegt, es den Prozess abgebrochen werden.

Dabei wird jedes Mal, wenn eine Ausnahme gemarshallt wird, erhalten Sie dies tun können:

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

## <a name="build-time-flags"></a>Flags zur Buildzeit

Es ist möglich, übergeben Sie die folgenden Optionen zum **Mtouch** (für Xamarin.iOS-apps) und **Mmp** (für Xamarin.Mac-apps), wird die zu bestimmen, ob die Ausnahme abfangen aktiviert ist, und legen Sie der Standardaktion, die sollte auftreten:

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

Mit Ausnahme von `disable`, diese Werte sind identisch mit der `ExceptionMode` Werte, die übergeben werden, die `MarshalManagedException` und `MarshalObjectiveCException` Ereignisse.

Die `disable` option wird _größtenteils_ deaktivieren Sie abfangen, außer es weiterhin Ausnahmen abfangen müssen, wenn keine Ausführung Verwaltungsaufwand hinzugefügt wird. Die Marshalling Ereignisse werden weiterhin mit dem Standardmodus wird der Standardmodus für die ausgeführte Plattform für diese Ausnahmen werden ausgelöst.

## <a name="limitations"></a>Einschränkungen

Wir nur P/Invokes zum Abfangen der `objc_msgSend` Funktionsreihe beim Objective-C-Ausnahmen abfangen. Dies bedeutet, dass eine P/Invoke auf einem anderen C-Funktion, die dann alle Objective-C-Ausnahmen auslöst, in der alten und undefinierte Verhalten weiterhin ausgeführt wird (Dies kann in der Zukunft verbessert werden).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>Verwandte Links

- [Ausnahme Marshalling (Beispiel)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
