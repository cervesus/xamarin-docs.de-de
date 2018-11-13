---
title: Xamarin Profiler-Problembehandlung
description: Dieses Dokument enthält Informationen zur Problembehandlung im Zusammenhang mit der Xamarin Profiler. Probleme im Zusammenhang mit der Protokollierung und Diagnose, die IDE und anderen Themen beschrieben.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: lobrien
ms.author: laobri
ms.date: 10/27/2017
ms.openlocfilehash: f9b4da5b6dfe3f0254340d9175b08198bd52a45a
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563341"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler-Problembehandlung

## <a name="logging-and-diagnostics"></a>Protokollierung und Diagnose

Das Xamarin-Team können Probleme nachverfolgen, wenn Sie uns Informationen bieten einschließlich:

- Ein Standbild aus dem für das Problem, Absturz oder fehlerhaft war und den Workflow, führt.
- Protokoll gibt (siehe unten).
- Die **.mlpd** generiert wird, für die Profilerstellungssitzung (siehe unten).

### <a name="getting-log-outputs"></a>Abrufen von Log-Ausgaben

Auf einem Mac Protokolle gespeichert werden sollen `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

In Windows werden diese auf gespeichert `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` fügen Sie das aktuelle Protokoll aus, wenn Sie ein Problem zu senden.

Wir fügen weitere protokollieren, wie wir gehen also diese Ausgabe sollte wachsen und im Laufe der Zeit noch nützlicher werden.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Generieren von .mlpd-Dateien

Ein **.mlpd** Datei ist die komprimierte Ausgabe des Profilers mono-Laufzeit. Der Xamarin Profiler-GUI liest die Daten aus einer **.mlpd** und für den Benutzer angezeigt. **.mlpd** -Dateien sind nützlich, die debugging-Tools für Xamarin können unsere Techniker, die diagnose von Problemen der Profiler mit Ihren Daten Probleme kann.

Die **.mlpd** für die aktuelle Sitzung werden automatisch im Ihres MACS gespeichert `/tmp` Verzeichnis, und nach dem Zeitstempel identifiziert werden können. Wenn Sie Protokollierung aktivieren, werden die erste Ausgabe der Pfad zu der **.mlpd** Datei. Die **.mlpd** Datei wird normalerweise im Verzeichnis starten ~/var bzw. Ordner gespeichert...

Die **.mlpd** für eine aktuelle Sitzung auch kann, können Sie durch Auswahl gespeichert werden **Datei > Speichern unter...** der Profiler Menü:

**Visual Studio für Mac**:

![](troubleshooting-images/image17.png "Speichern von .mlpd-Datei in Visual Studio für Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Speichern von .mlpd-Datei in Visual Studio")

Es ist wichtig zu beachten, dass **.mlpd** enthalten eine Vielzahl von Informationen und die Dateigröße wird groß sein.

## <a name="troubleshooting"></a>Problembehandlung

Die folgende Liste zeigt die allgemeine Probleme, problemumgehungen und Tipps und Tricks für die Verwendung von den Profiler.

> [!NOTE]
> **Beachten Sie**: Sie müssen Visual Studio **Enterprise** Abonnenten dieses Feature in entweder Visual Studio Enterprise auf Windows oder Visual Studio für Mac entsperren

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Die Option "iOS-Profiler" wird nicht angezeigt oder wird Sie abgeblendet [Visual Studio und Visual Studio für Mac]

Überprüfen Sie die folgenden Einstellungen zum Beheben dieses Problems:

- Stellen Sie sicher, dass Sie die Debugkonfiguration verwenden
- Stellen Sie sicher, dass Sie den SGen Garbage Collector verwenden.
- Stellen Sie sicher, die Plattform ist [unterstützt](~/tools/profiler/index.md#Profiler_Support).
- Stellen Sie sicher, dass Sie die richtige Lizenz verfügen.
- Sicherstellen Sie, dass Sie angemeldet sind, im und ordnungsgemäß authentifiziert.
- [Visual Studio] Sie müssen verwenden [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) und über eine gültige Enterprise-Lizenz verfügen.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Ich erhalte einen Fehler, wenn ich versuche, den Profiler zu starten

Wenn Sie in diesem Fehlerdialogfeld ausführen, wenn den Profiler in Visual Studio verwenden:

![](troubleshooting-images/error.png "Im Fehler, wenn den Profiler in Visual Studio verwenden")

Es handelt sich normalerweise aufgrund eines kann nicht zum Simulator gestartet / Emulator. Versuchen Sie es und führen Sie die app wie gewohnt, beheben Sie die Probleme, die es ermöglicht, und klicken Sie dann erneut versuchen, den Profiler.

#### <a name="to-watch-a-specific-thread"></a>Überwacht einen bestimmten thread

Wenn Sie einen Thread, die Sie speziell ansehen möchten verfügen, wäre es ideal, benennen Sie den Thread ganz am Anfang seiner Erstellung abzurufenden `ThreadName` anstelle von `0x0`. Z. B. den Threadnamen als festlegen `UI`, können Sie den folgenden Code verwenden:

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise: verwenden den Xamarin Profiler](~/tools/profiler/index.md)
- [Arbeitsspeicher- und Leistungsproblemen bewährte Methoden](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/profiler/preview/)
