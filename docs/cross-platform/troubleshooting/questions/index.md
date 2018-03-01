---
title: "Allgemein, häufig gestellte Fragen"
ms.topic: article
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 0c01bfb08d525fd336b29f405304efaeaf749f35
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="general-frequently-asked-questions"></a>Allgemein, häufig gestellte Fragen

## <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate
### <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarinvisualstudio-2017-rcmd"></a>[Kann ich Visual Studio 2017 Release Candidate mit Xamarin verwenden?](visualstudio-2017-rc.md)
Eine Beschreibung der aktuellen Auswirkungen auf die mithilfe von Xamarin mit Visual Studio 2017 Release Candidate (RC) als auch für Informationen zum Installieren von Xamarin in Visual Studio2017 RC.

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Wie kann ich anzeigen, welche Bibliotheken in einer PCL unterstützt werden?](pcl-support-libraries.md)
Dieses Handbuch enthält Ressourcen und Ihre Methoden um zu ermitteln, ob Ihre vorhandene Bibliothek von den verschiedenen PCL-Zielplattformen unterstützt wird oder einem Profil PCL konvertiert werden kann.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL-Reflektions-API](pcl-reflection.md)
Microsoft entwickelt, eine neue Reflektions-API für die Verwendung in portablen Klassenbibliotheken. Wenn Sie vorhandenen Reflektionscode, die Sie in einer PCL verschieben möchten verfügen, möglicherweise die funktioniert nicht.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL-Fallstudie: wie können Sie Probleme im Zusammenhang mit System.Diagnostics.Tracing für das Microsoft TPL Dataflow-NuGet-Paket lösen?](pcl-case-study.md)
Xamarin.iOS und Xamarin.Android implementieren nicht 100 % der jedes Profil PCL, die sie als Verweise zu ermöglichen. Praktische zur Vereinfachung in Visual Studio für Mac und Visual Studio NuGet-Paket-Manager ermöglichen Xamarin-Projekte die Verwendung von mehreren Profilen, die nur unvollständige Implementierungen. Weder Xamarin.iOS und Xamarin.Android derzeit enthält beispielsweise eine vollständige Implementierung der Typen in der `System.Diagnostics.Tracing` PCL-Namespace. Durch den Wechsel der app-Projekt verweisen, die Portable net45 + win8 wp8 + wpa81 können dies umgehen Version die TPL-Datenflussbibliothek.

## <a name="nuget-packages--xamarin-components"></a>NuGet-Pakete & Xamarin-Komponenten
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Wie kann ich NuGet aktualisieren?](nuget-update.md)
NuGet-Updates, Erweiterungen und -add-ins finden Sie unter der **Updates** Registerkarte der **NuGet Package Manager**. Detaillierte Navigation zu den Updates in Visual Studio für Mac und Visual Studio zu suchen ist in diesem Handbuch.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Wie werden ich ein NuGet-Paket heruntergestuft?](nuget-package-downgrade.md)
Visual Studio für Mac und Visual Studio verfügen über die Funktionen für ältere Versionen der Pakete auswählen und Installieren dieser Komponenten automatisch; ähnlich wie Works aktualisieren Pakete.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Fehlende Pakete Fehler nach dem Aktualisieren von NuGet-Pakete](nuget-packages-missing.md)
Dieses Problem wurde in erster Linie auf Xamarin.Forms app Beispielprojektmappen gemeldet, aber das Potenzial für dieses Problem kann auftreten, für jedes Projekt, das NuGet-Pakete verwendet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Vereinheitlichung der Google Play Services, Komponenten und NuGet](gps-components-nuget.md)
Es verwendet mehrere Google-Dienstekomponenten wiedergeben und NuGet-Pakete werden, sondern für Entwickler vereinfachen, wir haben jetzt vereinheitlichte unsere Komponenten und NuGet-Pakete in zwei. In nahezu jedem Fall sollte die Google Play-Dienste verwendet werden. Der einzige Grund für das Paket (Froyo) zu verwenden ist, wenn Sie aktiv Froyo abzielen.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Wo werden die Komponenten auf meinem Computer gespeichert?](component-storage.md)
Wenn Sie eine Xamarin-Komponente in ein App-Projekt installieren, ruft es in den beiden Standorten, die in diesem Handbuch angegebenen eingefügt.


## <a name="troubleshooting"></a>Problembehandlung
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Wo finde ich meine Versionsinformationen und Protokolle?](version-logs.md)
Dieser Leitfaden enthält, wo die meisten Diagnoseinformationen zu finden, die Xamarin-Problemen verwendet werden können.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Wann und wie sollte ich einen Fehlerbericht Datei?](howto-file-bug.md)
Dieses Handbuch enthält Tipps zum Ablegen von hoher Qualität Problemberichte, sodass unsere Engineers die Ursache (und alle potenziellen Fehlerbehebungen) effizienter für ein Problem ermitteln können.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Warum wird von Xamarin nicht Jenkins unterstützt?](xamarin-jenkins.md)
Jenkins ist ein Open Source-CI; Viele Probleme, die direkt durch die Jenkins entstehen aufgrund von *selbst* müssen als Probleme vor, in dem Sie den Code erhalten haben, z. B. abgelegt werden die [main Jenkins-Repository](https://github.com/jenkinsci/jenkins), oder das Repository für [ Jenkins.app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Welche projekteinstellungen sind erforderlich, damit der Debugger?](debugger-settings.md)
In der Reihenfolge für den Debugger so funktionieren wie erwartet (Haltepunkte, Anzeige Debugprotokolle usw.) muss angezeigten Informationen Instrumentation und Debug Developer aktiviert sein. Dieses Handbuch enthält ausführliche Informationen zum Suchen und diese Einstellungen zu aktivieren.

