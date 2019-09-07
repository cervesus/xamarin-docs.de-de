---
title: Allgemeine häufig gestellte Fragen
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: de70eda2898f29a1e7afed9440d5f5fae496e069
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765113"
---
# <a name="general-frequently-asked-questions"></a>Allgemeine häufig gestellte Fragen

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?](pcl-support-libraries.md)
In diesem Handbuch werden Ressourcen und Methoden aufgelistet, mit denen ermittelt werden kann, ob Ihre vorhandene Bibliothek von den verschiedenen PCL-Zielplattformen unterstützt wird oder in ein PCL-Profil konvertiert werden kann.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL-Reflektions-API](pcl-reflection.md)
Microsoft hat eine neue reflektionsapi für die Verwendung in portablen Klassenbibliotheken entwickelt. Wenn Sie über einen vorhandenen Reflektionscode verfügen, den Sie in eine PCL verschieben möchten, funktioniert das möglicherweise nicht.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL-Fallstudie: Wie kann ich Probleme im Zusammenhang mit System. Diagnostics. Tracing für das nuget-Paket "Microsoft TPL DataFlow" beheben?](pcl-case-study.md)
Xamarin. IOS und xamarin. Android implementieren nicht 100% jedes PCL-Profils, das Sie als Verweise zulassen. Der praktische Vorteil in Visual Studio für Mac, Visual Studio und dem nuget-Paket-Manager ermöglichen xamarin-Projekte die Verwendung mehrerer Profile, die nur unvollständige Implementierungen aufweisen. Beispielsweise enthält weder xamarin. IOS noch xamarin. Android derzeit eine vollständige Implementierung der Typen im `System.Diagnostics.Tracing` PCL-Namespace. Sie können dieses Problem umgehen, indem Sie das App-Projekt so wechseln, dass es auf die Portable-net45 + win8 + WP8 + wpa81-Version der TPL-Datenfluss Bibliothek verweist.

## <a name="nuget-packages--xamarin-components"></a>Nuget-Pakete & xamarin-Komponenten
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Wie kann ich NuGet aktualisieren?](nuget-update.md)
Nuget-Updates,-Erweiterungen und-Add-Ins finden Sie im **nuget-Paket-Manager**auf der Registerkarte " **Updates** ". Ausführliche Navigation zum Suchen der Updates in Visual Studio für Mac & Visual Studio finden Sie in diesem Handbuch.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Wie stufe ich ein NuGet-Paket herab?](nuget-package-downgrade.md)
Visual Studio für Mac & Visual Studio über Features verfügen, mit denen ältere Versionen von Paketen ausgewählt und automatisch installiert werden können. ähnlich wie das Aktualisieren von Paketen funktioniert.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Fehler wegen fehlender Pakete nach dem Aktualisieren von NuGet-Paketen](nuget-packages-missing.md)
Dieses Problem wurde hauptsächlich bei xamarin. Forms-Beispiel-App-Lösungen gemeldet, aber das Potenzial dieses Problems kann für jedes Projekt auftreten, das nuget-Pakete verwendet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Vereinheitlichen von Google Play-Dienstkomponenten und NuGet](gps-components-nuget.md)
Es gab mehrere Google Play Services Komponenten und nuget-Pakete, aber um Entwicklern die Arbeit zu erleichtern, haben wir nun unsere Komponenten und nuget-Pakete in zwei vereinheitlicht. In nahezu jedem Fall sollten Google Play Services verwendet werden. Der einzige Grund für die Verwendung des Pakets (Froyo) ist, dass Sie "Froyo" aktiv als Ziel verwenden.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Wo werden die Komponenten auf meinem Computer gespeichert?](component-storage.md)
Jedes Mal, wenn Sie eine xamarin-Komponente in einem App-Projekt installieren, wird Sie an den beiden in diesem Handbuch aufgeführten Speicherorten platziert.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Wo finde ich meine Versionsinformationen und Protokolle?](version-logs.md)
In dieser Anleitung wird erläutert, wo Sie die meisten Diagnoseinformationen finden, die für die Problembehandlung bei xamarin-Problemen verwendet werden können.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Wann und wie sollte ich einen Fehlerbericht speichern?](howto-file-bug.md)
Dieses Handbuch enthält Tipps zum Einreichen hochwertiger Fehlerberichte, damit unsere Techniker die Ursache (und alle möglichen Korrekturen) für ein Problem effizienter ermitteln können.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Warum wird Jenkins nicht von Xamarin unterstützt?](xamarin-jenkins.md)
Jenkins ist eine Open-Source-CI-Suite. Aufgrund dieser zahlreichen Probleme, die direkt von der Jenkins-Datei verursacht werden, müssen Sie als Probleme bei der Position des Codes *protokolliert werden.* beispielsweise das [Jenkins-Hauptrepository](https://github.com/jenkinsci/jenkins)oder das Repository für [Jenkins. app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Welche Projekteinstellungen sind für den Debugger erforderlich?](debugger-settings.md)
Damit der Debugger erwartungsgemäß funktioniert (Haltepunkte erreichen, Debugprotokolle anzeigen usw.), müssen die Entwickler Instrumentation und die Anzeige der Debuginformationen aktiviert werden. In dieser Anleitung wird erläutert, wie Sie diese Einstellungen suchen und aktivieren.
