---
title: Ausnahme in Xamarin.iOS-Marshalling
description: Dieses Dokument beschreibt, wie Sie mit systemeigenen und verwalteten Ausnahmen in einer Xamarin.iOS-app arbeiten. Es beschreibt Probleme, die auftreten können und eine Lösung für diese Probleme.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/05/2017
ms.openlocfilehash: 167d6ac421bdd2652e7f8474e1ea21bd9040723f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075089"
---
# <a name="exception-marshaling-in-xamarinios"></a>Ausnahme in Xamarin.iOS-Marshalling

_Xamarin.iOS enthält neue Ereignisse, um die Hilfe auf Ausnahmen, insbesondere in nativem Code zu reagieren._

Sowohl Objective-C-als auch verwalteten Code bieten Unterstützung für Runtime-Ausnahmen (Try/Catch/finally-Klauseln).

Allerdings unterscheiden sich ihre Implementierungen, was bedeutet, dass die Common Language Runtime-Bibliotheken (die Mono-Laufzeit und die Objective-C-Laufzeitbibliotheken) Probleme auftreten, wenn müssen sie Ausnahmen behandeln, und führen Sie dann in anderen Sprachen geschriebenen Code.

Dieses Dokument erläutert die Probleme, die auftreten können und die möglichen Lösungen.

Es enthält auch ein Beispielprojekt [Ausnahme beim Marshaling](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), die können verwendet werden, um verschiedene Szenarien und Lösungen zu testen.

## <a name="problem"></a>Problem

Das Problem tritt auf, wenn eine Ausnahme ausgelöst wird, und während der stapelentladung, die einen Frame aufgetreten ist, die nicht den Typ der Ausnahme übereinstimmen, die ausgelöst wurde.

Ein typisches Beispiel der für Xamarin.iOS oder Xamarin.Mac ist, wenn eine systemeigene API löst eine Objective-C-Ausnahme aus, und klicken Sie dann die Objective-C-Ausnahme irgendwie bearbeitet werden muss Wenn des Entladens Stack einen verwalteten Frame erreicht.

Die Standardaktion ist, nichts zu tun. Für das obige Beispiel bedeutet dies, lassen die Objective-C-Runtime Entladung verwalteten Frames. Dies ist problematisch, da die Objective-C-Laufzeit nicht verwalteten Frames entladen Informationen verfügt; z. B. es wird nicht ausgeführt `catch` oder `finally` Klauseln in diesen Frame.

### <a name="broken-code"></a>Fehlerhaften code

Beachten Sie im folgenden Codebeispiel wird ein:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Dadurch wird ein Objective-C-NSInvalidArgumentException in systemeigenem Code ausgelöst:

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

Und die stapelüberwachung werden etwa wie folgt:

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

0-3 werden systemeigene Rahmen und der stapelentlader in der Objective-C-Laufzeit _können_ die Rahmen entladen. Insbesondere führt alle Objective-C `@catch` oder `@finally` Klauseln.

Ist Sie jedoch die Objective-C-stapelentlader _nicht_ ordnungsgemäß verwalteten Frames (Frames 4 bis 6), entladen, die Frames entladen werden, jedoch nicht verwaltete Ausnahme Logik ausgeführt werden kann.

Das bedeutet, dass es in der Regel nicht möglich, diese Ausnahmen zu erfassen, auf folgende Weise:

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

Dies ist, da die Objective-C-stapelentlader nicht zu den verwalteten weiß `catch` -Klausel, und weder wird der `finally` -Klausel ausgeführt werden.

Wenn im obigen Beispiel _ist_ effektiv ist da Objective-C eine Methode benachrichtigt wird, der nicht behandelten Ausnahmen für Objective-C, verfügt [`NSSetUncaughtExceptionHandler`][2], welche Xamarin.iOS und Xamarin.Mac verwenden und zu diesem Zeitpunkt versucht, alle Objective-C-Ausnahmen in verwalteten Ausnahmen zu konvertieren.

## <a name="scenarios"></a>Szenarien

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Szenario 1: Abfangen von Objective-C-Ausnahmen mit einem verwalteten Catch-handler

Im folgenden Szenario, es ist möglich, Objective-C-Ausnahmen abfangen mithilfe von verwalteten `catch` Handler:

1. Eine Objective-C-Ausnahme ausgelöst.
2. Die Objective-C-Laufzeit durchläuft den Stapel (aber es wird nicht entladen), Suchen nach einem systemeigenen `@catch` Handler, der die Ausnahme behandeln kann.
3. Die Objective-C-Laufzeit nicht finden kann, `@catch` Handler, ruft `NSGetUncaughtExceptionHandler`, und ruft den Handler für Xamarin.iOS/Xamarin.Mac installiert.
4. Xamarin.iOS/Xamarin.Mac's-Handler setzt die Objective-C-Ausnahme in eine verwaltete Ausnahme konvertieren, und löst es. Da die Objective-C Runtime nicht entlädt die Aufrufliste (nur wurde erläutert, wie es), der aktuelle Frame ist dasjenige, in denen die Objective-C-Ausnahme ausgelöst wurde.

Hier tritt ein anderes Problem auf, da die Mono-Laufzeit wie Objective-C-Frames ordnungsgemäß entladen nicht bekannt ist.

Wenn Xamarin.iOS nicht abgefangene Objective-C-Ausnahme-Rückruf aufgerufen wird, ist im Stapel wie folgt aus:

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

Hier die einzige verwaltete Frames sind Frames 8 bis 10, aber die verwaltete Ausnahme wird ausgelöst, im Bereich 0. Dies bedeutet, dass die Mono-Laufzeit die systemeigenen Rahmen 0-7, entladen werden muss, wodurch ein Problem, das Problem weiter oben erläuterten entspricht: Obwohl die Mono-Laufzeit die systemeigenen Rahmen entladen wird, wird keine Objective-C-Ausführung `@catch` oder `@finally` Klauseln .

Codebeispiel:

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

Und die `@finally` Klausel wird nicht ausgeführt werden, da die Mono-Laufzeit, die diesem Rahmen entladen sie unbekannt ist.

Eine Variante hiervon ist, um auszulösen, eine verwaltete Ausnahme in verwaltetem Code und dann entladen über systemeigene Rahmen zum Abrufen auf den ersten verwalteten `catch` Klausel:

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

Die verwaltete `UIApplication:Main` Methodenaufruf wird das systemeigene `UIApplicationMain` -Methode, und klicken Sie dann auf iOS wird bieten die native codeausführung, bevor schließlich ein Aufruf der verwalteten `AppDelegate:FinishedLaunching` Methode immer noch eine Vielzahl von systemeigene Rahmen im Stapel, wenn die verwaltete Ausnahme ist ausgelöst:

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

0-1-Frames und 27. bis 30. verwaltet werden, während alle zwischen systemeigenen sind. Wenn Mono über diese Frames, keine Objective-C entlädt `@catch` oder `@finally` Klauseln ausgeführt werden.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Szenario 2: kann nicht zum Abfangen von Objective-C-Ausnahmen

Im folgenden Szenario ist es _nicht_ möglich, Objective-C-Ausnahmen abfangen, Verwendung von verwalteten `catch` Handler, da die Objective-C-Ausnahme auf andere Weise behandelt wurde:

1. Eine Objective-C-Ausnahme ausgelöst.
2. Die Objective-C-Laufzeit durchläuft den Stapel (aber es wird nicht entladen), Suchen nach einem systemeigenen `@catch` Handler, der die Ausnahme behandeln kann.
3. Die Objective-C-Laufzeit sucht nach einer `@catch` Handler auf, den Stapel entlädt, und startet die Ausführung der `@catch` Handler.

Dieses Szenario wird häufig in Xamarin.iOS-apps gefunden, weil auf dem Hauptthread in der Regel solchen Code:

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

Dies bedeutet, dass auf dem Hauptthread nie wirklich eine nicht behandelte Ausnahme von Objective-C, und daher unsere, die Objective-C-Ausnahmen in verwalteten Ausnahmen konvertiert Rückruf nie aufgerufen.

Dies ist auch üblich, beim Debuggen von Xamarin.Mac-apps auf einer früheren MacOS-Version als Xamarin.Mac unterstützt, da die meisten UI-Objekte im Debugger überprüfen, rufen Sie Eigenschaften entsprechen, Selektoren, die nicht vorhanden sind, auf den ausgeführten Plattform (versucht Da Xamarin.Mac-Unterstützung für eine höhere Version von MacOS umfasst). Solche Selektoren Aufruf löst eine `NSInvalidArgumentException` ("Unbekannte Auswahl an gesendet..."), dies führt letztendlich zu den Prozess abstürzt.

Zusammenfassend lässt sich sagen, dass die Objective-C-Runtime oder die Mono-Laufzeit entladen Frames, die sie nicht auf programmiert werden Handles kann zu nicht definierten Verhalten, wie Abstürze, Speicherverluste und andere Arten von unvorhersehbaren (mis) Verhalten führen.

## <a name="solution"></a>Lösung

In Xamarin.iOS 10 und Xamarin.Mac 2.10 haben wir Unterstützung zum Abfangen von verwalteter und Objective-C-Ausnahmen an alle verwalteten nativen Grenze und für diese Ausnahme wird in den anderen Typ konvertieren hinzugefügt.

In Pseudocode sieht etwa wie folgt:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Der P/Invoke auf Objc_msgSend abgefangen wird, und diese stattdessen aufgerufen wird:

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

Und etwas Ähnliches erfolgt für den umgekehrten Fall (Marshallen von verwalteten Ausnahmen in Objective-C-Ausnahmen).

Abfangen von Ausnahmen für die verwaltete, systemeigene Grenze ist nicht ohne Kosten, sodass es nicht immer standardmäßig aktiviert:

- Xamarin.iOS/tvOS: Abfangen von Objective-C-Ausnahmen im Simulator aktiviert ist.
- Xamarin.watchOS: Abfangfunktion wird in allen Fällen erzwungen, da die Objective-C-Runtime-Entladung verwaltet lassen verwirrt Frames der Garbage Collector und machen Sie es nicht mehr reagiert oder abstürzt.
- Xamarin.Mac: Abfangen von Objective-C-Ausnahmen ist aktiviert, für Debugbuilds.

Die [Buildzeit Flags](#build_time_flags) Abschnitt wird erläutert, wie Sie die Aktivierung der Abfangfunktion, wenn sie nicht standardmäßig aktiviert ist.

## <a name="events"></a>Ereignisse

Es gibt zwei neue Ereignisse, die ausgelöst werden, sobald eine Ausnahme abgefangen wird: `Runtime.MarshalManagedException` und `Runtime.MarshalObjectiveCException`.

Werden beide Ereignisse übergeben eine `EventArgs` Objekt, das die ursprüngliche Ausnahme enthält, die ausgelöst wurde (die `Exception` Eigenschaft), und ein `ExceptionMode` Eigenschaft, die definiert, wie die Ausnahme gemarshallt werden sollen.

Die `ExceptionMode` Eigenschaft kann geändert werden, im Ereignishandler, um das Verhalten entsprechend jede benutzerdefinierte Verarbeitung im Ereignishandler ausgeführt. Ein Beispiel dafür ist, um den Prozess abzubrechen, wenn eine bestimmte Ausnahme auftritt.

Ändern der `ExceptionMode` Eigenschaft gilt für das einzelne Ereignis, er wirkt sich keine Ausnahmen abgefangen, in der Zukunft.

Die folgenden Modi sind verfügbar:

- `Default`: Der Standardwert hängt von der Plattform. Es ist `ThrowObjectiveCException` im kooperativen-Modus (WatchOS), ist der Garbage Collector und `UnwindNativeCode` andernfalls (iOS / WatchOS / MacOS). Der Standardwert kann in der Zukunft ändern.
- `UnwindNativeCode`: Dies ist das vorherige Verhalten für die (nicht definiert). Dies ist nicht verfügbar, wenn der Garbage Collector im kooperativen Modus verwendet (Dies ist die einzige Option in WatchOS, daher Dies ist keine gültige Option für WatchOS), aber es ist die Standardoption für alle anderen Plattformen.
- `ThrowObjectiveCException`: Konvertieren Sie die verwaltete Ausnahme in eine Objective-C-Ausnahme aus, und lösen Sie die Objective-C-Ausnahme aus. Dies ist die Standardeinstellung für WatchOS.
- `Abort`: Den Prozess wird abgebrochen.
- `Disable`: Die Ausnahme abfangen, deaktiviert, sodass es nicht sinnvoll, diesen Wert im Ereignishandler festzulegen, sobald das Ereignis ausgelöst wird es jedoch zu spät um zu deaktivieren. In jedem Fall, wenn festgelegt, es als Verhalten `UnwindNativeCode`.

Für das Marshalling von Objective-C-Ausnahmen in verwalteten Code, stehen die folgenden Modi:

- `Default`: Der Standardwert hängt von der Plattform. Es ist `ThrowManagedException` im kooperativen-Modus (WatchOS), ist der Garbage Collector und `UnwindManagedCode` andernfalls (iOS / TvOS / MacOS). Der Standardwert kann in der Zukunft ändern.
- `UnwindManagedCode`: Dies ist das vorherige Verhalten für die (nicht definiert). Dies ist nicht verfügbar, wenn der Garbage Collector im kooperativen Modus verwendet (Dies ist der einzige gültige GC-Modus auf WatchOS, daher Dies ist keine gültige Option für WatchOS), aber es ist die Standardeinstellung für alle anderen Plattformen.
- `ThrowManagedException`: Die Objective-C-Ausnahme in eine verwaltete Ausnahme konvertieren, und der verwaltete Ausnahme ausgelöst. Dies ist die Standardeinstellung für WatchOS.
- `Abort`: Den Prozess wird abgebrochen.
- `Disable`: Deaktiviert die Ausnahme abfangen, damit es sinnvoll, legen Sie nicht diesen Wert im Ereignis-Handler, aber sobald das Ereignis ausgelöst wird, ist es zu spät um zu deaktivieren. In jedem Fall, wenn festgelegt, es den Prozess abgebrochen wird.

Daher können jedes Mal, wenn eine Ausnahme gemarshallt wird, erhalten Sie so vorgehen:

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

## <a name="build-time-flags"></a>Buildzeit-Flags

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

Die `disable` option wird _größtenteils_ abfangen, deaktivieren, mit der Ausnahme wir weiterhin Ausnahmen abfangen müssen, wenn keine Mehraufwand für die Ausführung hinzugefügt wird. Die Marshalling Ereignisse werden immer noch für diese Ausnahmen, mit dem Standardmodus wird der Standardmodus für das ausgeführte ausgelöst.

## <a name="limitations"></a>Einschränkungen

Wir nur P/Invokes in Abfangen der `objc_msgSend` Funktionsreihe beim Versuch, Objective-C-Ausnahmen zu erfassen. Dies bedeutet, dass eine P/Invoke auf einen anderen C-Funktion, die dann alle Objective-C-Ausnahmen auslöst, noch das alte und nicht definierten Verhalten ausgeführt wird (Dies kann in Zukunft verbessert werden).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>Verwandte Links

- [Ausnahme-Marshalling (Beispiel)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
