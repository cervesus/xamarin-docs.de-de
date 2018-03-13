---
title: Android-Debugprotokoll
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26ac42e4b7acbe19dee746130fc335fdf18ffc46
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="android-debug-log"></a>Android-Debugprotokoll

## <a name="android-debug-log-overview"></a>Übersicht: Android-Debugprotokoll

Ein sehr gängiger Trick, den Entwickler verwenden, um ihre Anwendungen zu debuggen, ist das Aufrufen von `Console.WriteLine`. Auf einer mobilen Plattform wie Android steht jedoch keine Konsole zur Verfügung. Android-Geräte stellen ein Protokoll bereit, das Sie beim Schreiben von Apps verwenden können. Dies wird aufgrund des Befehls, den Sie eingeben, um es abzurufen, manchmal als „Logcat“ bezeichnet.

## <a name="accessing-from-visual-studio"></a>Zugreifen über Visual Studio

In Visual Studio 2015 und höher werden die Android- und iOS-Protokollpad vereinheitlicht.

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 & 2017

In dem neuen Toolfenster „Geräteprotokoll“ für Visual Studio werden die Protokolle für Android- und iOS-Geräte angezeigt. Es kann angezeigt werden, indem Sie einen der folgenden Befehle ausführen: 

-   **Ansicht > Andere Fenster > Geräteprotokoll**
-   **Extras > Android > Geräteprotokoll**
-   **Android-Symbolleiste > Geräteprotokoll**

Sobald das Toolfenster angezeigt wird, kann das physische Gerät aus dem Kombinationsfeld für Geräte ausgewählt werden. Nach Auswahl des Geräts wird automatisch mit dem Hinzufügen von Protokolleinträgen aus einer ausgeführten App in der Tabelle begonnen. Wenn Sie zwischen Geräten wechseln, wird die Geräteprotokollierung abgebrochen und erneut gestartet. Ein Android-Projekt muss geladen werden, damit die Geräte im Kombinationsfeld angezeigt werden können. Falls die Geräte nicht im Kombinationsfeld angezeigt werden, müssen Sie zunächst überprüfen, ob sie im Dropdownmenü „Start Debug“ (Debug starten) verfügbar sind. 

Über dieses Toolfenster haben Sie Zugriff auf Folgendes: eine Tabelle mit Protokolleinträgen, ein Kombinationsfeld zur Geräteauswahl, eine Möglichkeit zum Löschen von Protokolleinträgen, ein Suchfeld und die Schaltflächen „Wiedergabe“, „Anhalten“ und „Pause“. 



## <a name="accessing-from-the-command-line"></a>Zugreifen über die Befehlszeile

Eine weitere Möglichkeit für den Zugriff auf das Debugprotokoll stellt der Zugriff über die Befehlszeile dar. Öffnen Sie ein Konsolenfenster, und navigieren Sie zum Android SDK-Ordner „platform-tools“ (Beispiel: **C:\android-sdk-windows\platform-tools**). 

Wenn nur ein Gerät angeschlossen ist, kann das Protokoll angezeigt werden mit:

```shell
$ adb logcat
```

Wenn mehrere Geräte angeschlossen sind, muss das Gerät identifiziert werden. Beispiel: `adb -d logcat` zeigt das Protokoll des einzigen angeschlossenen physischen Geräts an, während `adb -e logcat` das Protokoll des einzigen ausgeführten Emulators anzeigt. 

Weitere Befehle können gefunden werden, indem nur **adb** ausgeführt wird.



## <a name="writing-to-the-debug-log"></a>Schreiben in das Debugprotokoll

Nachrichten können mit Methoden der Klasse [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) in das Debugprotokoll geschrieben werden: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Dadurch wird Folgendes generiert:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```


## <a name="interesting-messages"></a>Interessante Nachrichten

Beim Lesen des Protokolls und insbesondere bei der Bereitstellung von Protokollausschnitten für andere (da eine Bereitstellung der vollständigen Protokolldatei mit (1) zu viel Aufwand verbunden ist und die Datei (2) nicht lesbar ist), sollte zunächst mit der *wichtigsten* Zeile begonnen werden, die der folgenden ähnelt:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Insbesondere eine Zeile, die mit dem regulären Ausdruck übereinstimmt:

```shell
^I.*ActivityManager.*Starting: Intent
```

und den Namen des Anwendungspakets enthält. Diese Zeile entspricht dem Start einer Aktivität, und der *Großteil* (aber nicht alle) der folgenden Nachrichten sollte sich auf die Anwendung beziehen. 

Die einzelnen Nachrichten enthalten die Prozesskennung (PID) des Prozesses, der die Nachrichten generiert. In der obigen `ActivityManager`-Nachricht hat Prozess `12944` die Nachricht generiert. Suchen Sie nach der mono.MonoRuntimeProvider-Nachricht, um zu bestimmen, welcher Prozess der gedebuggte Prozess der Anwendung ist: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Diese Nachricht stammt aus dem Prozess, der gestartet wurde. Alle nachfolgenden Nachrichten mit übereinstimmender PID stammen aus demselben Prozess. 
