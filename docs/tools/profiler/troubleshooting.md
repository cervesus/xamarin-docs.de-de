---
title: Xamarin Profiler, Problembehandlung
description: Dieses Dokument enthält Informationen zur Problembehandlung im Zusammenhang mit der Xamarin Profiler. Probleme im Zusammenhang mit Protokollierung und Diagnoseinformationen, die IDE und anderen Themen beschrieben.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 71faf79ef9b783480dbb6ff4674859a9148abca3
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066909"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler, Problembehandlung

## <a name="logging-and-diagnostics"></a>Protokollierung und-Diagnose

Die Xamarin-Team helfen, Probleme zu verfolgen, wenn Sie uns Informationen bereitstellen einschließlich:

- Ein Screencast das Problem, Absturz oder fehlerhaft war und der Workflow darauf vorausgegangen.
- Log-Ausgaben (siehe unten).
- Die **.mlpd** für die Profilerstellungssitzung (siehe unten) generiert wird.

### <a name="getting-log-outputs"></a>Abrufen von Protokoll Ausgaben

Auf einem Mac Protokolle gespeichert werden, um `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

Unter Windows werden diese auf gespeichert `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` Geben Sie auch das aktuelle Protokoll, wenn Sie ein Problem übermitteln.

Weitere protokollieren, wie wir uns zuwenden, damit diese Ausgabe sollte Wachstum und mit der Zeit weitere nützliche werden, hinzugefügt.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Generieren von .mlpd-Dateien

Ein **.mlpd** Datei ist die komprimierte Ausgabe des Profilers mono-Laufzeit. Der Xamarin Profiler GUI liest die Daten aus einer **.mlpd** und für den Benutzer angezeigt. **.mlpd** Dateien eignen sich debugging-Tools für Xamarin, da sie unsere Ingenieure, die Probleme zu diagnostizieren der Profiler mit Ihren Daten Probleme kann helfen.

Die **.mlpd** für die aktuelle Sitzung automatisch in Ihrem Mac gespeichert `/tmp` Verzeichnis und den Timestamp identifiziert werden können. Wenn Sie die Protokollierung aktivieren, werden die erste Ausgabe der Pfad zu der **.mlpd** Datei. Die **.mlpd** Datei wird normalerweise in das Verzeichnis ab ~/var/Ordner gespeichert werden...

Die **.mlpd** für eine aktuelle Sitzung auch kann, können Sie durch Auswahl gespeichert werden **Datei > Speichern unter...** der Profiler im Menü:

**Visual Studio für Mac**:

![](troubleshooting-images/image17.png "Speichern von .mlpd-Datei in Visual Studio für Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Speichern von .mlpd-Datei in Visual Studio")

Es ist wichtig zu beachten, dass **.mlpd** enthalten eine Vielzahl von Informationen und die Dateigröße wird groß sein.

## <a name="troubleshooting"></a>Problembehandlung

Die folgende Liste zeigt allgemeine Einschränkungen, problemumgehungen, sowie Tipps und Tricks für die Verwendung des Profilers.

> [!NOTE]
> **Hinweis**: Sie müssen eine Visual Studio **Enterprise** Abonnenten zum Entsperren von dieser Funktion in dem entweder Visual Studio Enterprise unter Windows oder im Visual Studio für Mac.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Die iOS-Profiler-Option wird nicht angezeigt, oder es ist abgeblendet, [Visual Studio und Visual Studio für Mac]

Überprüfen Sie die folgenden Einstellungen aus, um dieses Problem zu beheben:

- Stellen Sie sicher, dass Sie die Debugkonfiguration verwenden
- Stellen Sie sicher, dass Sie den SGen-Garbage Collector verwenden.
- Sicherstellen, dass die Plattform ist [unterstützt](~/tools/profiler/index.md#Profiler_Support).
- Stellen Sie sicher, dass Sie über die richtige Lizenz verfügen.
- Stellen Sie sicher, dass Sie angemeldet sind in und ordnungsgemäß authentifiziert.
- [Visual Studio] Sie müssen unter Verwendung [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) und eine gültige Enterprise-Lizenz verfügen.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Ich erhalte eine Fehlermeldung, wenn ich versuche, den Profiler zu starten.

Wenn Sie in diesem Fehlerdialogfeld ausführen, wenn den Profiler in Visual Studio verwenden:

![](troubleshooting-images/error.png "Fehlerdialogfeld, wenn den Profiler in Visual Studio verwenden")

Wird normalerweise aufgrund eines konnte nicht gestartet werden, den Simulator / Emulator. Versuchen Sie es und führen Sie die Anwendung wie gewohnt, beheben Sie die Probleme, die es ermöglicht, und wiederholen Sie den Profiler erneut verwenden.

#### <a name="to-watch-a-specific-thread"></a>Um einen bestimmten Thread zu beobachten.

Wenn Sie einen Thread, die speziell überwacht werden sollen verfügen, wäre es ideal zum Benennen von Threads an, die sehr ab seiner Erstellung, sodass Get abrufen `ThreadName` anstelle von `0x0`. Beispielsweise Threadname als UI festlegen können Sie den folgenden Code:

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise – mit dem Xamarin-Profiler](~/tools/profiler/index.md)
- [Arbeitsspeicher- und Leistungsproblemen bewährte Methoden](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/profiler/preview/)
