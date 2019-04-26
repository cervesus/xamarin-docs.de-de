---
title: Allgemeine häufig gestellte Fragen
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61157192"
---
# <a name="general-frequently-asked-questions"></a>Allgemeine häufig gestellte Fragen

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?](pcl-support-libraries.md)
Dieses Handbuch enthält Ressourcen und Methoden, um festzustellen, ob Ihre vorhandene Bibliothek durch die verschiedenen Zielplattformen der PCL-Ziel unterstützt wird oder in ein PCL-Profil konvertiert werden kann.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL-Reflektions-API](pcl-reflection.md)
Microsoft entwickelt einen neuen Reflektions-API für die Verwendung in portablen Klassenbibliotheken. Wenn Sie vorhandenen Reflektionscode, den Sie in eine PCL verschieben möchten verfügen, funktioniert es nicht.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL-Fallstudie: Wie kann ich die Probleme im Zusammenhang mit System.Diagnostics.Tracing für das Microsoft-TPL Dataflow-NuGet-Paket beheben?](pcl-case-study.md)
Xamarin.iOS und Xamarin.Android implementieren nicht 100 % der jedes PCL-Profil, die sie als Referenzen zu ermöglichen. Praktische der Einfachheit halber in Visual Studio für Mac und Visual Studio den NuGet-Paket-Manager können Xamarin-Projekte die Verwendung von mehrere Profile, die nur unvollständige Implementierungen aufweisen. Weder Xamarin.iOS und Xamarin.Android derzeit enthält beispielsweise eine vollständige Implementierung der Typen in der `System.Diagnostics.Tracing` PCL-Namespace. Sie können diese umgehen durch den Wechsel von der app-Projekt verweisen, das Portable-net45 + win8 + wp8 + wpa81 Version von der TPL-Datenflussbibliothek.

## <a name="nuget-packages--xamarin-components"></a>NuGet-Pakete und Xamarin-Komponenten
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Wie kann ich NuGet aktualisieren?](nuget-update.md)
NuGet-Updates, Erweiterungen und -add-ins finden Sie unter den **Updates** Registerkarte die **NuGet Package Manager**. Ausführliche Navigation zu den Updates in Visual Studio für Mac und Visual Studio zu suchen, ist in diesem Handbuch.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Wie stufe ich ein NuGet-Paket herab?](nuget-package-downgrade.md)
Visual Studio für Mac und Visual Studio haben die Features für ältere Versionen der Pakete auswählen und diese automatisch installiert werden; ähnlich wie das Aktualisieren von funktioniert Paketen.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Fehler wegen fehlender Pakete nach dem Aktualisieren von NuGet-Paketen](nuget-packages-missing.md)
Dieses Problem wurde in erster Linie auf Xamarin.Forms-Beispiel-app-Lösungen gemeldet, aber das Potenzial für dieses Problem kann auftreten, in einem Projekt, das NuGet-Pakete verwendet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Vereinheitlichen von Google Play-Dienstkomponenten und NuGet](gps-components-nuget.md)
Es verwendet, werden mehrere Google Play-Dienstkomponenten und NuGet-Pakete, sondern einfacher für Entwickler zu machen, wir haben jetzt unified unsere Dienstkomponenten und NuGet Pakete in zwei. In fast allen Fällen sollte die Google Play-Dienste verwendet werden. Der einzige Grund für das Paket (Froyo) verwenden, ist, wenn Sie aktiv Froyo verwenden möchten.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Wo werden die Komponenten auf meinem Computer gespeichert?](component-storage.md)
Wenn Sie eine Xamarin-Komponente in einem App-Projekt installieren, ruft es in den beiden Standorten, die in diesem Handbuch angegebenen eingefügt.


## <a name="troubleshooting"></a>Problembehandlung
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Wo finde ich meine Versionsinformationen und Protokolle?](version-logs.md)
Dieses Handbuch wird erläutert, wo die meisten Diagnoseinformationen zu finden, die verwendet werden können, um Xamarin-Probleme zu beheben.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Wann und wie sollte ich einen Fehlerbericht speichern?](howto-file-bug.md)
Dieses Handbuch enthält Tipps für das Einreichen von hoher Qualität, Fehlerberichte, damit unsere Techniker, die Ursache (und alle potenziellen Fehlerbehebungen) für ein Problem effizienter zu bestimmen können.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Warum wird Jenkins nicht von Xamarin unterstützt?](xamarin-jenkins.md)
Jenkins ist ein Open-Source-CI-Suite. aufgrund von diesem vieler Probleme, die direkt von der Jenkins verursacht werden *selbst* müssen gesendet werden, wie Probleme vor, in dem Sie den Code abgerufen haben, z.B. die [main Jenkins-Repository](https://github.com/jenkinsci/jenkins), oder das Repository für [ Jenkins.app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Welche Projekteinstellungen sind für den Debugger erforderlich?](debugger-settings.md)
Damit kann der Debugger funktioniert wie erwartet (Haltepunkte, Anzeige Debugprotokollen usw.) muss Entwickler Instrumentation und Debuggen Informationen anzeigen aktiviert sein. Dieses Handbuch enthält Informationen zum Suchen und diese Einstellungen zu aktivieren.

