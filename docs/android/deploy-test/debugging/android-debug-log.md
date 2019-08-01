---
title: Android-Debugprotokoll
description: Erfahren Sie, wie Sie mit dem Debugprotokoll Xamarin.Android-Anwendungen debuggen.
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: edd6fc92603783dc9de64b10304e8d48f97bdef3
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509213"
---
# <a name="android-debug-log"></a>Android-Debugprotokoll

Ein sehr gängiger Trick, den Entwickler verwenden, um ihre Anwendungen zu debuggen, ist das Aufrufen von `Console.WriteLine`. Auf einer mobilen Plattform wie Android steht jedoch keine Konsole zur Verfügung. Android-Geräte stellen ein Protokoll bereit, das Sie beim Schreiben von Apps verwenden können. Dies wird aufgrund des Befehls, den Sie eingeben, um es abzurufen, manchmal als _Logcat_ bezeichnet. Verwenden Sie das **Debugprotokolltool** zum Anzeigen der protokollierten Daten.

## <a name="android-debug-log-overview"></a>Übersicht: Android-Debugprotokoll

Das **Debugprotokolltool** bietet die Möglichkeit, während des Debuggens einer App durch Visual Studio Protokollausgaben anzuzeigen. Das Debugprotokoll unterstützt die folgenden Geräte:

-   Physische Android-Smartphones, -Tablets und -Wearables
-   ein virtuelles Android-Gerät, das auf dem Android-Emulator ausgeführt wird 

> [!NOTE]
> Das **Debugprotokolltool** funktioniert nicht mit dem Xamarin Live Player.

Das **Debugprotokoll** zeigt keine Protokollmeldungen an, die generiert werden, während die App eigenständig auf dem Gerät ausgeführt wird (z.B. während es von Visual Studio getrennt ist).


## <a name="accessing-the-debug-log-from-visual-studio"></a>Zugreifen auf das Debugprotokoll von Visual Studio

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Klicken Sie auf das **Geräteprotokollsymbol (Logcat)** , um das **Debugprotokolltool** zu öffnen:

[![Position des Geräteprotokolltools in der Symbolleiste](android-debug-log-images/vswin-01-logcat-sml.png)](android-debug-log-images/vswin-01-logcat.png#lightbox)

Alternativ können Sie das **Geräteprotokolltool** über eine der folgenden Menüoptionen starten:

-   **Ansicht > Andere Fenster > Geräteprotokoll**
-   **Extras > Android > Geräteprotokoll**

Der folgende Screenshot veranschaulicht die verschiedenen Teile des **Debugtoolfensters**:

[![Bestandteile des Debugtoolfensters](android-debug-log-images/vswin-03-features-sml.png)](android-debug-log-images/vswin-03-features.png#lightbox)

-   **Geräteauswahl**: Wählt aus, welches physische Gerät oder welcher ausgeführter Emulator überwacht werden soll.

-   **Protokolleinträge**: Eine Tabelle mit Protokollmeldungen von Logcat.

-   **Protokolleinträge löschen**: Löscht alle aktuellen Protokolleinträge aus der Tabelle.

-   **Wiedergeben/Pause**: Wechsel zwischen Aktualisieren und Anhalten der Anzeige von neuen Protokolleinträgen.

-   **Beenden**: Hält das Anzeigen von neuen Protokolleinträgen an.

-   **Suchfeld**: Geben Sie Suchzeichenfolgen in dieses Feld ein, um nach einer Teilmenge von Protokolleinträgen zu filtern.


Wenn das **Debugprotokoll**-Toolfenster angezeigt wird, verwenden Sie das Pulldownmenü des Geräts, um das Android-Gerät auszuwählen, das überwacht werden soll.

[![Position der Geräteauswahl](android-debug-log-images/vswin-02-devices-combo-sml.png)](android-debug-log-images/vswin-02-devices-combo.png#lightbox)

Nachdem Sie das Gerät ausgewählt haben, fügt das **Geräteprotokoll** automatisch Protokolleinträge aus einer ausgeführten App hinzu. Diese Protokolleinträge werden in der Tabelle für Protokolleinträge angezeigt. Wenn Sie zwischen Geräten wechseln, wird die Geräteprotokollierung abgebrochen und erneut gestartet. Beachten Sie, dass ein Android-Projekt geladen werden muss, bevor Geräte in der Geräteauswahl angezeigt werden. Wenn das Gerät nicht in der Geräteauswahl angezeigt wird, sollten Sie überprüfen, ob es im Geräte-Dropdownmenü von Visual Studio neben der Schaltfläche **Start** verfügbar ist.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Klicken Sie auf **Ansicht > Bereiche > Geräteprotokoll**, um das **Geräteprotokoll** zu öffnen.

[![Position des Menüelements „Geräteprotokoll“](android-debug-log-images/vsmac-01-logcat-sml.png)](android-debug-log-images/vsmac-01-logcat.png#lightbox)

Der folgende Screenshot veranschaulicht die verschiedenen Teile des **Debugtoolfensters**:

[![Features des Debugtoolfensters](android-debug-log-images/vsmac-03-features-sml.png)](android-debug-log-images/vsmac-03-features.png#lightbox)

-   **Geräteauswahl**: Wählt aus, welches physische Gerät oder welcher ausgeführter Emulator überwacht werden soll.

-   **Protokolleinträge**: Eine Tabelle mit Protokollmeldungen von Logcat.

-   **Protokolleinträge löschen**: Löscht alle aktuellen Protokolleinträge aus der Tabelle.

-   **Suchfeld**: Geben Sie Suchzeichenfolgen in dieses Feld ein, um nach einer Teilmenge von Protokolleinträgen zu filtern.

-   **Meldungen anzeigen**: Schaltet die Anzeige von Informationsmeldungen um.

-   **Warnungen anzeigen**: Schaltet die Anzeige von Warnmeldungen um (Warnmeldungen werden in gelb dargestellt).

-   **Fehler anzeigen**: Schaltet die Anzeige von Fehlermeldungen um (Warnmeldungen werden in rot dargestellt).

-   **Verbindung wiederherstellen**: Stellt erneut eine Verbindung zum Gerät her und aktualisiert die Anzeige des Protokolleintrags.

-   **Marker hinzufügen**: Fügt eine Markernachricht (wie z.B. `--- Marker N ---`) nach dem neuesten Protokolleintrag ein. Dabei ist _N_ ein Zähler, der bei 1 beginnt und bei jedem neu hinzugefügten Marker um 1 erhöht wird.

Wenn das Debugprotokoll-Toolfenster angezeigt wird, verwenden Sie das Pulldownmenü des Geräts, um das Android-Gerät auszuwählen, das überwacht werden soll.

[![Position der Geräteauswahl](android-debug-log-images/vsmac-02-devices-combo-sml.png)](android-debug-log-images/vsmac-02-devices-combo.png#lightbox)

Nachdem Sie das Gerät ausgewählt haben, fügt das **Geräteprotokoll** automatisch Protokolleinträge aus einer ausgeführten App hinzu. Diese Protokolleinträge werden in der Tabelle für Protokolleinträge angezeigt. Wenn Sie zwischen Geräten wechseln, wird die Geräteprotokollierung abgebrochen und erneut gestartet. Beachten Sie, dass ein Android-Projekt geladen werden muss, bevor Geräte in der Geräteauswahl angezeigt werden. Wenn das Gerät nicht in der Geräteauswahl angezeigt wird, sollten Sie überprüfen, ob es im Geräte-Dropdownmenü von Visual Studio neben der Schaltfläche **Start** verfügbar ist.

-----


## <a name="accessing-from-the-command-line"></a>Zugreifen über die Befehlszeile

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Eine weitere Möglichkeit für den Zugriff auf das Debugprotokoll stellt der Zugriff über die Befehlszeile dar. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Android SDK-Ordner „platform-tools“ (dieser befindet sich normalerweise unter **C:\\Programme (x86)\\Android\\android-sdk\\platform-tools**).

Wenn nur ein Gerät (physisches Gerät oder Emulator) angefügt ist, kann der Bericht angezeigt werden, indem folgender Befehl eingegeben wird:

```shell
$ adb logcat
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Eine weitere Möglichkeit für den Zugriff auf das Debugprotokoll stellt der Zugriff über die Befehlszeile dar. Öffnen Sie ein Terminalfenster, und navigieren Sie zum Android SDK-Ordner „platform-tools“ (dieser befindet sich normalerweise unter **/Benutzer/Benutzername/Library/Developer/Xamarin/android-sdk-macosx/platform-tools**).

Wenn nur ein Gerät (physisches Gerät oder Emulator) angefügt ist, kann der Bericht angezeigt werden, indem folgender Befehl eingegeben wird:

```shell
$ ./adb logcat
```

-----


Wenn mehrere Geräte angefügt sind, muss das Gerät explizit identifiziert werden. Beispiel: **adb -d logcat** zeigt das Protokoll des einzigen angefügten physischen Geräts an, während **adb -e logcat** das Protokoll des einzigen ausgeführten Emulators anzeigt.

Weitere Befehle können gefunden werden, indem **adb** eingegeben wird oder die Hilfemeldungen durchgelesen werden.


## <a name="writing-to-the-debug-log"></a>Schreiben in das Debugprotokoll

Nachrichten können mit Methoden der Klasse [Android.Util.Log**in das**Debugprotokoll](xref:Android.Util.Log) geschrieben werden.
Beispiel: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Dadurch wird eine Ausgabe erzeugt, die der folgenden ähnelt:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

Sie haben auch die Möglichkeit, mithilfe von `Console.WriteLine` in das **Debugprotokoll** &ndash; zu schreiben. Dann werden die Nachrichten in Logcat in einem etwas anderen Ausgabeformat angezeigt. Dies ist besonders nützlich, wenn Sie Xamarin.Forms-Apps unter Android debuggen:

```csharp
System.Console.WriteLine ("DEBUG - Button Clicked!");
```

Dadurch wird eine Ausgabe erzeugt, die der Folgenden ähnelt:

```
Info (19543) / mono-stdout: DEBUG - Button Clicked!
```

## <a name="interesting-messages"></a>Interessante Nachrichten

Beim Lesen des Protokolls (und besonders bei der Bereitstellung von Protokollausschnitten für andere) ist eine sorgfältige Prüfung der gesamten Protokolldatei oft zu aufwendig.
Um das Navigieren durch die Protokollnachrichten zu vereinfachen, suchen Sie anfangs nach einem Protokolleintrag, der dem folgenden ähnelt:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Suchen Sie insbesondere nach einer Zeile, die mit dem regulären Ausdruck übereinstimmt und außerdem den Namen des Anwendungspakets enthält:

```shell
^I.*ActivityManager.*Starting: Intent
```

Diese Zeile entspricht dem Start einer Aktivität, und der *Großteil* (aber nicht alle) der folgenden Nachrichten sollte sich auf die Anwendung beziehen.

Beachten Sie, dass die einzelnen Nachrichten die Prozesskennung (PID) des Prozesses enthalten, der die Nachrichten generiert. In der obigen `ActivityManager`-Nachricht hat Prozess `12944` die Nachricht generiert. Suchen Sie nach der **mono.MonoRuntimeProvider**-Meldung, um zu bestimmen, welcher Prozess der gedebuggte Prozess der Anwendung ist: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Diese Meldung stammt aus dem Prozess, der gestartet wurde. Alle nachfolgenden Meldungen, die diese PID enthalten, stammen aus dem gleichen Prozess.
