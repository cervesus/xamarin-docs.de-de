---
title: Allgemein, häufig gestellte Fragen
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.openlocfilehash: b81f5c09ea06acf87449448f43b6c0474a6737b0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="general-frequently-asked-questions"></a>Allgemein, häufig gestellte Fragen

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?](pcl-support-libraries.md)
Dieses Handbuch enthält Ressourcen und Ihre Methoden um zu ermitteln, ob Ihre vorhandene Bibliothek von den verschiedenen PCL-Zielplattformen unterstützt wird oder einem Profil PCL konvertiert werden kann.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL-Reflektions-API](pcl-reflection.md)
Microsoft entwickelt, eine neue Reflektions-API für die Verwendung in portablen Klassenbibliotheken. Wenn Sie vorhandenen Reflektionscode, die Sie in einer PCL verschieben möchten verfügen, möglicherweise die funktioniert nicht.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL-Fallstudie: Wie kann ich Probleme im Zusammenhang mit System.Diagnostics.Tracing für das NuGet-Paket für den Microsoft-TPL-Datenfluss lösen?](pcl-case-study.md)
Xamarin.iOS und Xamarin.Android implementieren nicht 100 % der jedes Profil PCL, die sie als Verweise zu ermöglichen. Praktische zur Vereinfachung in Visual Studio für Mac und Visual Studio NuGet-Paket-Manager ermöglichen Xamarin-Projekte die Verwendung von mehreren Profilen, die nur unvollständige Implementierungen. Weder Xamarin.iOS und Xamarin.Android derzeit enthält beispielsweise eine vollständige Implementierung der Typen in der `System.Diagnostics.Tracing` PCL-Namespace. Durch den Wechsel der app-Projekt verweisen, die Portable net45 + win8 wp8 + wpa81 können dies umgehen Version die TPL-Datenflussbibliothek.

## <a name="nuget-packages--xamarin-components"></a>NuGet-Pakete & Xamarin-Komponenten
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Wie kann ich NuGet aktualisieren?](nuget-update.md)
NuGet-Updates, Erweiterungen und -add-ins finden Sie unter der **Updates** Registerkarte der **NuGet Package Manager**. Detaillierte Navigation zu den Updates in Visual Studio für Mac und Visual Studio zu suchen ist in diesem Handbuch.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Wie stufe ich ein NuGet-Paket herab?](nuget-package-downgrade.md)
Visual Studio für Mac und Visual Studio verfügen über die Funktionen für ältere Versionen der Pakete auswählen und Installieren dieser Komponenten automatisch; ähnlich wie Works aktualisieren Pakete.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Fehler wegen fehlender Pakete nach dem Aktualisieren von NuGet-Paketen](nuget-packages-missing.md)
Dieses Problem wurde in erster Linie auf Xamarin.Forms app Beispielprojektmappen gemeldet, aber das Potenzial für dieses Problem kann auftreten, für jedes Projekt, das NuGet-Pakete verwendet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Vereinheitlichen von Google Play-Dienstkomponenten und NuGet](gps-components-nuget.md)
Es verwendet mehrere Google-Dienstekomponenten wiedergeben und NuGet-Pakete werden, sondern für Entwickler vereinfachen, wir haben jetzt vereinheitlichte unsere Komponenten und NuGet-Pakete in zwei. In nahezu jedem Fall sollte die Google Play-Dienste verwendet werden. Der einzige Grund für das Paket (Froyo) zu verwenden ist, wenn Sie aktiv Froyo abzielen.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Wo werden die Komponenten auf meinem Computer gespeichert?](component-storage.md)
Wenn Sie eine Xamarin-Komponente in ein App-Projekt installieren, ruft es in den beiden Standorten, die in diesem Handbuch angegebenen eingefügt.


## <a name="troubleshooting"></a>Problembehandlung
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Wo finde ich meine Versionsinformationen und Protokolle?](version-logs.md)
Dieser Leitfaden enthält, wo die meisten Diagnoseinformationen zu finden, die Xamarin-Problemen verwendet werden können.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Wann und wie sollte ich einen Fehlerbericht speichern?](howto-file-bug.md)
Dieses Handbuch enthält Tipps zum Ablegen von hoher Qualität Problemberichte, sodass unsere Engineers die Ursache (und alle potenziellen Fehlerbehebungen) effizienter für ein Problem ermitteln können.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Warum wird Jenkins nicht von Xamarin unterstützt?](xamarin-jenkins.md)
Jenkins ist ein Open Source-CI; Viele Probleme, die direkt durch die Jenkins entstehen aufgrund von *selbst* müssen als Probleme vor, in dem Sie den Code erhalten haben, z. B. abgelegt werden die [main Jenkins-Repository](https://github.com/jenkinsci/jenkins), oder das Repository für [ Jenkins.app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Welche Projekteinstellungen sind für den Debugger erforderlich?](debugger-settings.md)
In der Reihenfolge für den Debugger so funktionieren wie erwartet (Haltepunkte, Anzeige Debugprotokolle usw.) muss angezeigten Informationen Instrumentation und Debug Developer aktiviert sein. Dieses Handbuch enthält ausführliche Informationen zum Suchen und diese Einstellungen zu aktivieren.

