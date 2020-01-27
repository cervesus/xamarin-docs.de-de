---
title: DevOps mit Xamarin
ms.assetid: ff978cc2-5a25-46d6-921b-e51adaa65992
author: davidortinau
ms.author: daortin
manager: crdun
ms.workload:
- xamarin
ms.date: 10/23/2018
ms.openlocfilehash: bd1a728dfe14ff9243402bedae74db7be93bf466
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725131"
---
# <a name="devops-with-xamarin"></a>DevOps mit Xamarin

Mit Xamarin können Sie plattformübergreifende mobile Apps für Android, iOS und Windows mit C#, .NET und Visual Studio erstellen. Xamarin ermöglicht die gemeinsame Verwendung eines großen Teils des Codes für alle Plattformen, sodass nur ein kleiner Prozentsatz plattformspezifisch angepasst werden muss.

Das Entwickeln von Apps für moderne Plattformen umfasst viele weitere Aktivitäten neben dem Schreiben von Code. Diese als DevOps (Development + Operations) bezeichneten Aktivitäten decken den gesamten Lebenszyklus der App ab. Dazu gehören das Planen und Nachverfolgen der Arbeit, das Entwickeln und Implementieren von Code, das Verwalten eines Quellcoderepositorys, das Ausführen von Builds, das Verwalten von Continuous Integration und Deployment, das Testen (einschließlich Komponententests und Tests der Benutzeroberfläche), das Durchführen verschiedener Formen der Diagnose in Entwicklungs- und Produktionsumgebungen und das Überwachen der App-Leistung und des Benutzerverhaltens in Echtzeit über Telemetrie und Analysen.

Visual Studio bietet zusammen mit Azure DevOps Services und Team Foundation Server eine Vielzahl von DevOps-Funktionen. Viele dieser Funktionen sind vollständig auf plattformübergreifende Projekte anwendbar. Dies gilt vor allem für Xamarin-Apps, da sie mit C# und .NET erstellt werden, worauf einige DevOps-Tools basieren. Andere Tools erfordern die enge Integration in Build- und Laufzeitumgebungen. Da Xamarin-Apps nicht auf Windows-Plattformen ausgeführt werden und die Mono-Implementierung von .NET verwenden, bietet Xamarin spezielle Tools für bestimmte Anforderungen.

In den folgenden Tabellen wird beschrieben, welche DevOps-Features in Visual Studio ordnungsgemäß bzw. eingeschränkt funktionieren. Ausführliche Informationen über die Funktionen selbst finden Sie in der verknüpften Dokumentation.

## <a name="agile-tools"></a>Agile-Tools

Verweislink: **[Informationen zu Agile-Tools und Agile-Projektverwaltung](/azure/devops/boards/backlogs/backlogs-overview?view=azure-devops)**

Allgemeiner Kommentar: alle Planungs- und Nachverfolgungsfunktionen sind vom Projekttyp und den Programmiersprachen unabhängig.

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|Verwalten von Rückständen und Sprints|Ja||
|Arbeitsüberwachung|Ja||
|Teamraumzusammenarbeit|Ja||
|Kanban-Boards|Ja||
|Berichte und Visualisierung des Fortschritts|Ja||

## <a name="modeling"></a>Modellierung

Verweislink: **[Analyse und Modellarchitektur](/visualstudio/modeling/analyze-and-model-your-architecture)**

Die Entwicklungsfeatures sind unabhängig von der Programmiersprache oder funktionieren mit .NET-Sprachen wie C#. Mit Code zusammenhängende Aspekte finden Sie unter [Rollen von Architektur- und Modellierungsdiagrammen in der Softwareentwicklung](/visualstudio/modeling/scenario-change-your-design-using-visualization-and-modeling#ModelingDiagramsTools).

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|Sequenzdiagramme|Ja||
|Abhängigkeitsdiagramme|Ja||
|Aufrufhierarchie|Ja||
|Klassen-Designer|Ja||
|Architektur-Explorer|Ja||
|UML-Diagramme (Anwendungsfall, Aktivität, Klasse, Komponente, Sequenz und DSL)|Ja||
|Ebenendiagramme|Ja||
|Ebenenüberprüfung|Ja||

## <a name="code"></a>Code

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|[Verwenden der Team Foundation-Versionskontrolle (TFVC)](/azure/devops/repos/tfvc/overview?view=vsts) oder Azure Repos|Ja||
|[Erste Schritte mit Git in Azure Repos](/azure/devops/repos/git/gitquickstart?view=vsts&tabs=visual-studio)|Ja||
|[Verbessern der Codequalität](/visualstudio/test/improve-code-quality)|Ja||
|[Ermitteln von Änderungen am Code und andere Verläufe](/visualstudio/ide/find-code-changes-and-other-history-with-codelens)|Ja|Außer über plattformspezifische Grenzen hinweg, in denen die Implementierung bis zur Laufzeit nicht aufgelöst wird.|
|[Verwenden von Code Maps zum Debuggen von Anwendungen](/visualstudio/modeling/use-code-maps-to-debug-your-applications)|Ja||

## <a name="build"></a>Erstellen

Referenzlink: **[Azure Pipelines](/azure/devops/pipelines/index?view=vsts)**

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|Lokaler TFS-Server|Ja|Auf Buildcomputern muss Xamarin installiert sein, und sie können mit einem OSX-Computer verknüpft werden, um Builds für iOS zu erstellen. Siehe [Verwenden von TFVC](/azure/devops/repos/tfvc/overview?view=vsts).|
|Lokaler Buildserver mit Verbindung zu Azure Pipelines|Ja|Anweisungen finden Sie unter [Build and release agents (Build- und Release-Agents)](/azure/devops/pipelines/agents/agents?view=vsts).|
|Gehosteter Controllerdienst von Azure Pipelines|Ja|Siehe [Erstellen Ihrer Xamarin-App](/azure/devops/pipelines/languages/xamarin?view=vsts&tabs=vsts).|
|Builddefinitionen mit Vor- und Nachskripts|Ja||
|Fortlaufende Integration einschließlich abgegrenzter Eincheckvorgänge|Ja|Abgegrenzte Eincheckvorgänge für TFVC, nur wenn Git auf einem Pull-Request-Modell statt mit Eincheckvorgängen arbeitet.|

## <a name="test"></a>Testen

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|Planen von Tests, Erstellen von Testfällen und Organisieren von Testauflistungen|Ja||
|Manuelle Tests|Ja||
|Test-Manager (Aufzeichnung und Wiedergabe von Tests)|Ja|Nur Windows-Geräte und Android-Emulatoren von Visual Studio.|
|Codeabdeckung|nicht verfügbar||
|[Ausführen von Komponententests für Code](/visualstudio/test/unit-test-your-code/)|Ja|Für Windows- und Android-Ziele können die integrierten MSTest-Tools verwendet werden. Zum Ausführen von Komponententests für Windows, Android und iOS empfiehlt Xamarin NUnit. Siehe [Verwenden von TFVC](/azure/devops/repos/tfvc/overview?view=vsts).|
|[Verwenden der Benutzeroberflächenautomatisierung zum Testen des Codes](/visualstudio/test/use-ui-automation-to-test-your-code/)|Nur Windows|Testaufzeichnung der Benutzeroberfläche in Visual Studio ist nur unter Windows möglich. Alle Plattformen finden Sie unter [Xamarin.UITest](/appcenter/test-cloud/uitest/).|

## <a name="improve-code-quality"></a>Verbessern der Codequalität

Verweislink: **[Verbessern der Codequalität](/visualstudio/test/improve-code-quality)**

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|[Analysieren der Qualität von verwaltetem Code](/visualstudio/code-quality/analyzing-managed-code-quality-by-using-code-analysis)|Ja||
|[Suchen von doppeltem Code mit der Codeklonerkennung](https://msdn.microsoft.com/library/hh205279.aspx)|Ja||
|[Messen von Komplexität und Verwaltbarkeit verwalteten Codes](/visualstudio/code-quality/measuring-complexity-and-maintainability-of-managed-code)|Ja||
|[Performance Explorer (Leistungs-Explorer)](/visualstudio/profiling/performance-explorer)|Nein|Verwenden Sie stattdessen die [Xamarin Profiler](/xamarin/tools/profiler/) durch Visual Studio für Mac. Beachten Sie, dass der Xamarin Profiler sich derzeit in der Vorschauversion befindet und noch nicht für Windows-Ziele funktioniert.|
|[Analysieren von .NET Framework-Arbeitsspeicherproblemen](https://msdn.microsoft.com/library/dn342825.aspx)|Nein|Visual Studio-Tools haben keinen Zugriff auf das Mono-Framework für die Profilerstellung.|

## <a name="release-management"></a>Release Management:

Referenzlink: **[Build und Release in Azure Pipelines und TFS](/azure/devops/pipelines/overview?view=vsts)**

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|Verwalten von Releaseprozessen|Ja||
|Bereitstellen auf Servern für das Sideloading über Skripts|Ja||
|Hochladen in den App Store|Partial|Es stehen Erweiterungen zur Verfügung, die diesen Prozess für manche App-Stores automatisieren können.  Siehe [Extensions for Azure DevOps Services (Erweiterungen für Azure DevOps Services)](https://marketplace.visualstudio.com/VSTS); beispielsweise die [Extension for Google Play (Erweiterung für Google Play)](https://marketplace.visualstudio.com/items?itemName=ms-vsclient.google-play).|

## <a name="monitor-with-hockeyapp"></a>Überwachen mit HockeyApp

Verweislink: **[Überwachen mit HockeyApp](https://www.hockeyapp.net/features/)**

|Feature|Unterstützt von Xamarin|Zusätzliche Kommentare|
|-------------|----------------------------|-------------------------|
|Absturzanalysen, Telemetrie und Betaverteilung|Ja||
