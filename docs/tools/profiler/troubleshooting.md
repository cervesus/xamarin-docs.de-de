---
title: Xamarin Profiler Problembehandlung
description: Dieses Dokument enthält Informationen zur Problembehandlung im Zusammenhang mit dem Xamarin Profiler. Es werden Probleme im Zusammenhang mit Protokollierung und Diagnose, der IDE und anderen Themen beschrieben.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: davidortinau
ms.author: daortin
ms.date: 10/27/2017
ms.openlocfilehash: 915f7df80e3ae29ab3c598ea95fabbc054e916dd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019215"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler Problembehandlung

## <a name="logging-and-diagnostics"></a>Protokollierung und Diagnose

Das xamarin-Team kann Ihnen helfen, Probleme zu verfolgen, wenn Sie Informationen bereitstellen, einschließlich:

- Ein Screencast für das Problem, den Absturz oder den Fehler, und Ihr Workflow führt zu diesem.
- Protokoll Ausgaben (siehe unten).
- Die **. MLPD** , die für die Profil Erstellungs Sitzung generiert wird (siehe unten).

### <a name="getting-log-outputs"></a>Erhalten von Protokoll Ausgaben

Unter Mac werden Protokolle in `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`gespeichert.

Unter Windows werden diese in `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` gespeichert, wenn Sie ein Problem melden.

Wir fügen nun weitere Protokollierung hinzu, sodass diese Ausgabe größer werden und sich im Laufe der Zeit noch nützlicher machen sollte.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Erstellen von MLPD-Dateien

Eine **. MLPD** -Datei ist die komprimierte Ausgabe des Mono-Lauf Zeit Profilers. Die Xamarin Profiler GUI liest die Daten aus einer **. MLPD** und zeigt Sie für den Benutzer an. **MLPD** -Dateien sind nützliche Debuggingtools für xamarin, da Sie unseren Technikern helfen, Probleme zu diagnostizieren, die der Profiler möglicherweise mit Ihren Daten hat.

Die **. MLPD** für die aktuelle Sitzung wird automatisch in Ihrem Mac-`/tmp` Verzeichnis gespeichert und kann durch den Zeitstempel identifiziert werden. Wenn Sie die Protokollierung aktivieren, ist die erste Ausgabe der Pfad zur **. MLPD** -Datei. Die **MLPD** -Datei wird normalerweise in dem Verzeichnis gespeichert, beginnend ~/var/Folders...

Die **. MLPD** -Datei für eine aktuelle Sitzung kann auch durch Auswählen von **Datei > Speichern unter..** . im Menü des Profilers:

**Visual Studio für Mac**:

![](troubleshooting-images/image17.png "Saving .mlpd file in Visual Studio for Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Saving .mlpd file in Visual Studio")

Beachten Sie, dass " **. MLPD** " viele Informationen enthält und dass die Dateigröße groß ist.

## <a name="troubleshooting"></a>Problembehandlung

Die nachstehende Liste zeigt allgemeine Probleme, Problem Umgehungen und Tipps und Tricks für die Verwendung von Profiler.

> [!NOTE]
> Sie müssen ein Visual Studio **Enterprise** -Abonnent sein, um dieses Feature in Visual Studio Enterprise unter Windows oder Visual Studio für Mac entsperren zu können.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Die Option für den IOS-Profiler kann nicht angezeigt werden, oder Sie ist abgeblendet [Visual Studio und Visual Studio für Mac]

Überprüfen Sie die folgenden Einstellungen, um dies zu beheben:

- Stellen Sie sicher, dass Sie die Debugkonfiguration verwenden.
- Stellen Sie sicher, dass Sie den Sgen-Garbage Collector verwenden.
- Stellen Sie sicher, dass Plattform [unterstützt](~/tools/profiler/index.md#Profiler_Support)wird.
- Stellen Sie sicher, dass Sie über die richtige Lizenz verfügen.
- Stellen Sie sicher, dass Sie angemeldet sind und ordnungsgemäß authentifiziert sind.
- [Visual Studio] Sie müssen [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) verwenden und über eine gültige Enterprise-Lizenz verfügen.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Ich erhalte einen Fehler, wenn ich versuche, den Profiler zu starten.

Wenn Sie dieses Fehler Feld bei der Verwendung des Profilers in Visual Studio ausführen:

![](troubleshooting-images/error.png "Error box when using the profiler in Visual Studio")

Dies ist normalerweise darauf zurückzuführen, dass der Simulator/Emulator nicht gestartet werden kann. Versuchen Sie, die APP normal auszuführen, beheben Sie die Probleme, die Sie erhalten, und versuchen Sie dann erneut, den Profiler zu verwenden.

#### <a name="to-watch-a-specific-thread"></a>So beobachten Sie einen bestimmten Thread

Wenn Sie über einen Thread verfügen, der speziell überwacht werden soll, wäre es ideal, den Thread ganz am Anfang seiner Erstellung zu benennen, um `ThreadName` statt `0x0`zu erhalten. Wenn Sie z. b. den Thread Namen als `UI`festlegen möchten, können Sie den folgenden Code verwenden:

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise: Verwenden des Xamarin Profiler](~/tools/profiler/index.md)
- [Bewährte Methoden für Arbeitsspeicher und Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Anmerkungen zu dieser Version](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/profiler/preview/index.md)
