---
title: Debuggen eines nativen Absturzes in einer Xamarin.Mac-App
description: In dieser Dokument wird beschrieben, wie Sie Ausnahmen debuggen, die aus der Objective-C-Runtime stammen. Dabei werden unter anderem Assertionsfehler, Rückruffehler und das Bubbling von Ausnahmen erläutert.
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 40d849ad403f2f47c00be9d3da7b59fc27ce8002
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725492"
---
# <a name="debugging-a-native-crash-in-a-xamarinmac-app"></a>Debuggen eines nativen Absturzes in einer Xamarin.Mac-App

## <a name="overview"></a>Übersicht

Mitunter können Programmierfehler zu Abstürzen in der nativen Objective-C-Laufzeit führen. Im Gegensatz zu C#-Ausnahmen zeigen diese nicht auf eine bestimmte Zeile in Ihrem Code, die Sie korrigieren können. Manchmal kann es sehr einfach sein, sie zu finden und zu beheben. Doch manchmal ist es überaus schwierig, sie aufzuspüren.

Lassen Sie uns ein paar Beispiele aus der Praxis für native Abstürze durchgehen und einen Blick darauf werfen.

## <a name="example-1-assertion-failure"></a>Beispiel 1: Assertionsfehler

Hier sind die ersten Zeilen eines Absturzes in einer einfachen Testanwendung (diese Information sind im Bereich **Anwendungsausgabe** enthalten):

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
  0   CoreFoundation                      0x91888343 __raiseError + 195
  1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
  2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
  3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
  4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
  5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
  6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
  7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
  8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
  9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
  10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

Die Zeilen, denen Zahlen vorangestellt sind, sind die native Stapelüberwachung. Daran lässt sich erkennen, dass der Absturz irgendwo innerhalb von `NSTableView` bei der Behandlung der Zeilenhöhen aufgetreten ist. Dann löst `NSAssertionHandler` eine `NSException (objc_exception_throw)` aus, und wir erkennen den Assertionsfehler:

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

Sobald Sie dies sehen, ist es ziemlich klar, dass eine `NSOutlineViewDelegate`-Methode eine negative Zahl zurückgibt. Das war das Problem:

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## <a name="example-2-callback-jumped-into-middle-of-nowhere"></a>Beispiel 2: Rückruf erfolgte an ein falsches Ziel

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

Dies ist ein Problem, das viel schwieriger aufzuspüren war. Wenn Sie oben in der verwalteten Stapelüberwachung `at <unknown> <0xffffffff>` oder `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)` sehen, deutet dies darauf hin, dass wir versuchen, verwalteten Code mit einem Objekt auszuführen, für das eine Garbage Collection erfolgt ist. Die native Stapelüberwachung zeigt `trackMouse:inRect:ofView:untilMouseUp` in `NSCell _sendActionFrom`, sodass wir irgendwo ein Klickereignis verarbeiten, einen Rückruf in C# versuchen und nicht ans Ziel kommen.

Im Allgemeinen ist diese Art von Fehlern schwierig aufzuspüren. Ich habe `GC.Collect(2)` einem Schaltflächenhandler hinzugefügt, um zu helfen, dieses Problem aufzuspüren (eine Garbage Collection zu erzwingen), damit das Problem reproduzierbar wird. Nachdem ich für das Beispiel eine Weile eine Zweiteilung vorgenommen und Codeabschnitte entfernt hatte, bis das Problem verschwunden war, stellte sich heraus, dass es [dieser Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=23378) war:

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

Die `NSButton`, die von `StandardWindowButton()` zurückgegeben wurde, wurde erfasst, obwohl ein Ereignis dafür registriert war (das ist der Fehler). Wenn wir versuchen, dieses Ereignis durch Klicken aufzurufen, kommt es zum Absturz, sobald für die Schaltfläche eine Garbage Collection erfolgt.

Obwohl es nicht die eigentliche Ursache für dieses Problem war, können solche Stapelüberwachungen auch durch fehlerhafte Methodensignaturen in Funktionen verursacht werden, für die `[Export]` in Objective-C verwendet wurde. Wenn zum Beispiel eine Methode erwartet, dass ein Parameter `out string` ist und Sie ihn als `string` eingeben, kann auf die gleiche Weise ein Absturz verursacht werden.

## <a name="example-3-callbacks-and-managed-objects"></a>Beispiel 3: Rückrufe und verwaltete Objekte

Viele Cocoa-APIs sehen vor, dass ihr Rückruf durch die Bibliothek erfolgt, wenn ein Ereignis eintritt. Dadurch haben Sie die Möglichkeit zu reagieren, wenn z. B. Daten für die Ausführung einer Aufgabe benötigt werden. Obwohl Sie in erster Linie an die Muster **Delegate** und **DataSource** denken, gibt es eine Vielzahl von APIs, die auf diese Weise funktionieren. Wenn Sie z.B. die Methoden einer `NSView` überschreiben und sie dann in die visuelle Struktur einfügen, erwarten Sie, dass AppKit einen Rückruf an Sie richtet, sobald bestimmte Ereignisse eintreten.

In nahezu allen Fällen wird Xamarin.Mac ordnungsgemäß verhindern, dass das verwaltete Objektziel dieser Rückrufe einer Garbage Collection unterzogen wird, solange sie noch zurückgerufen werden können. Allerdings können Fehler in der Bindung dies selten stören. Wenn dies geschieht, können unangenehme Abstürze, ähnlich wie diese, beobachtet werden:

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

In dieser Anleitung erfahren Sie, wie Sie Fehler dieser Art aufspüren können, wenn sie auftauchen, sie ordnungsgemäß melden, damit sie behoben werden können, und sie bis dahin in Ihrem Code umgehen können.

### <a name="locating"></a>Suchen

In fast allen Fällen mit Fehlern dieser Art ist das primäre Symptom native Abstürze, normalerweise mit etwas Ähnlichem wie `mono_sigsegv_signal_handler` oder `_sigtrap` in den oberen Frames des Stapels. Cocoa versucht einen Rückruf an Ihren C#-Code, trifft auf ein Objekt, für das eine Garbage Collection erfolgt ist, und stürzt ab. Jedoch wird nicht jeder Absturz mit diesen Symbolen durch ein Bindungsproblem wie dieses verursacht. Sie müssen einige zusätzliche Aufgaben erledigen, um zu bestätigen, dass dies das Problem ist.

Das Aufspüren dieser Fehler wird dadurch erschwert, dass sie erst auftreten, **nachdem** eine Garbage Collection das betreffende Objekt entfernt hat. Wenn Sie glauben, dass Sie einen dieser Fehler gefunden haben, fügen Sie den folgenden Code an beliebiger Stelle in Ihrer Startsequenz hinzu:

```csharp
new System.Threading.Thread (() =>
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start ();
```

Dadurch wird Ihre Anwendung gezwungen, den Garbage Collector im Sekundentakt auszuführen. Führen Sie Ihre Anwendung erneut aus, und versuchen Sie, den Fehler zu reproduzieren. Wenn es sofort zum Absturz kommt oder beständig statt zufällig, sind Sie auf dem richtigen Weg.

### <a name="reporting"></a>Berichterstellung

Der nächste Schritt ist das Melden des Problems an Xamarin, damit die Bindung für künftige Releases korrigiert werden kann. Wenn Sie eine Unternehmenslizenz haben, öffnen Sie ein Ticket unter

[visualstudio.microsoft.com/vs/support/](https://visualstudio.microsoft.com/vs/support/)

Suchen Sie andernfalls nach einem bestehenden Problem:

- Überprüfen Sie die [Xamarin.Mac-Foren](https://forums.xamarin.com/categories/xamarin-mac).
- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues).
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden.

Fügen Sie möglichst viele der folgenden Informationen hinzu:

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich.
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt.   

### <a name="working-around"></a>Problemumgehung

Sobald Sie das Problem aufgespürt haben, kann es einfach sein, das Problem mithilfe einer Umgehung zu beheben, bis die Bindung korrigiert werden kann. Ziel ist es zu verhindern, dass das Objekt (**View**, **Delegate**, **DataSource**), das fälschlicherweise entfernt wird, den Speicher verlässt, indem ein offener Verweis beibehalten wird.

In einfachen Fällen, bei denen es nur eine einzige Instanz des Objekts gibt, ändern Sie den Code von:

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

In das Verwenden einer statischen Variablen wie dieser:

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

In Fällen, in denen mehrere Instanzen erstellt werden können, kann ein statisches `HashSet` verwendet werden:

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

Sobald die Bindung korrigiert wurde und Sie ein Upgrade auf die Version von Xamarin.Mac durchgeführt haben, die die Korrektur enthält, kann der Code der Umgehung entfernt werden.

## <a name="exception-bubbling-and-objective-c"></a>Bubbling von Ausnahmen und Objective-C

Sie dürfen niemals zulassen, dass eine C#-Ausnahme einen Escapevorgang für verwalteten Code an die aufrufende Objective-C-Methode durchführt. Wenn Sie dies tun, sind die Ergebnisse undefiniert, aber in der Regel mit einem Absturz verbunden. Im Allgemeinen tun wir alles, was wir können, um nützliche Informationen zu Abstürzen von nativem und verwaltetem Code bereitzustellen, damit Sie Ihre Probleme schnell lösen können.

Ohne sich allzu sehr mit den technischen Gründen zu verzetteln, ist die Einrichtung der Infrastruktur zum Abfangen von Ausnahmen bei verwaltetem Code an jeder verwalteten/nativen Grenze aufwändig und es gibt _viele_ Übergänge, die in vielen gemeinsamen Vorgängen vorkommen. Viele Vorgänge, insbesondere solche, die mit dem UI-Thread zu tun haben, müssen schnell beendet werden, weil sonst Ihre Anwendung ins Stocken gerät und unakzeptable Leistungsmerkmale aufweist. Viele dieser Rückrufe machen sehr einfache Dinge, die selten die Möglichkeit des Auslösens von Ausnahmen bieten, weshalb dieser Mehraufwand in diesen Fällen sowohl teuer als auch unnötig wäre.

Deshalb richten wir diese Try/-Catch-Anweisungen nicht für Sie ein. Für Stellen, an denen Ihr Code wichtige Aufgaben erledigt (was über die Rückgabe boolescher Werte oder einfache Berechnungen hinausgeht), können Sie versuchen, selbst mit Try/Catch-Blöcken zu arbeiten.
