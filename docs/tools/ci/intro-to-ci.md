---
title: Einführung in die fortlaufende Integration mit Xamarin
description: Fortlaufende Integration ist eine Software Engineering-Methode in der ein automatischen Buildvorgang kompiliert und optional testet eine app, wenn Code hinzugefügt oder von Entwicklern im Repository der Versionskontrolle des Projekts geändert. In diesem Artikel werden die allgemeinen Konzepte der kontinuierlichen Integration und einige der verfügbaren Optionen für die kontinuierliche Integration mit Xamarin-Projekten erläutert.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 017691ece68f979eea1627c0442f49018d5742fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Einführung in die fortlaufende Integration mit Xamarin

_Fortlaufende Integration ist eine Software Engineering-Methode in der ein automatischen Buildvorgang kompiliert und optional testet eine app, wenn Code hinzugefügt oder von Entwicklern im Repository der Versionskontrolle des Projekts geändert. In diesem Artikel werden die allgemeinen Konzepte der kontinuierlichen Integration und einige der verfügbaren Optionen für die kontinuierliche Integration mit Xamarin-Projekten erläutert._

Es ist üblich auf Softwareprojekten für Entwickler, die parallel arbeiten. Zu einem späteren Zeitpunkt ist es notwendig, alle diese integrieren parallele Datenströme von Arbeit in eine codebase, die das fertige Produkt bilden. In den Anfängen der Softwareentwicklung wurde diese Integration am Ende eines Projekts ausgeführt ein Prozess schwierig und riskant wurde.

Fortlaufende Integration (CI) eine solche Komplexität vermeiden, indem Sie das Zusammenführen jeder Entwickler Änderungen in der gemeinsamen Codebasis kontinuierlich, in der Regel auf, wenn alle Entwickler eingecheckten freigegebene Änderungen am Projekt Code Repository. Jedem Einchecken löst einen automatischen Buildvorgang und führt automatisierte Tests, um sicherzustellen, dass die neu eingeführte Code ggf. vorhandenen Code unterbrochen hat nicht.  Auf diese Weise CI Flächen Fehlern und Problemen sofort und wird sichergestellt, dass alle Teammitglieder auf dem neuesten Stand mit anderen Arbeitsaufgaben. Dies führt zu einer Codebasis mit einer einheitlicheren und stabil.

Fortlaufende Integration Systeme haben zwei Hauptkomponenten:

-  **Versionskontrolle** – Version Control (VC), auch als Datenquellen-Steuerelements oder Quellcode-, alle Code eines Projekts in ein einzelnes freigegebenes Repository konsolidiert und und dabei eine vollständige Versionsgeschichte jeder Änderung auf jede Datei. Dieses Repository genannte der *neuen* oder *master* verzweigen, enthält den Quellcode, die letztlich auf die Produktion zu erstellen oder eine Releaseversion der app verwendet werden. Es gibt viele open-Source und kommerziellen Produkten für diese Aufgabe, die in der Regel ermöglichen Teams oder Einzelpersonen, um eine Kopie des Codes in sekundären Verzweigungen zu verzweigen, wenn sie eine umfangreiche Änderungen vornehmen oder Experimente ohne Risiko an die hauptverzweigung durchführen können. Nachdem die Änderungen in einer sekundären Verzweigung überprüft werden, können sie dann alle zusammen werden wieder in die hauptverzweigung zusammengeführt.
-  **Continuous Integration-Server** – die Continuous Integration-Server ist verantwortlich für alle eines Projekts Elemente (Quellcode, Bilder, Videos, Datenbanken, automatisierte Tests, usw.) sammeln, die app kompilieren und Ausführen automatisierter Tests. Erneut stehen viele open-Source und kommerzielle CI-Servertools.

Entwickler müssen eine Arbeitskopie der einer oder mehreren Verzweigungen in der Regel auf ihren Arbeitsstationen, die anfänglich, in dem gearbeitet wird. Nachdem Sie ein entsprechenden Satz von Aufgaben abgeschlossen ist, sind die Änderungen "eingecheckt" oder "Commit", um die entsprechende Verzweigung angezeigt, die sie an die Arbeitskopien von anderen Entwicklern zurückgibt. Dies ist wie ein Team wird sichergestellt, die sie alle auf den gleichen Code arbeiten.

Erneut, bewirkt, dass mit continuous Integration die Act Commit der Änderungen den CI-Server auf das Projekt erstellen und Ausführen von automatisierten Tests, um die Richtigkeit des Quellcodes zu überprüfen. Wenn Buildfehler oder testen Fehler vorhanden sind, benachrichtigt ein CI-Server den verantwortlichen Entwickler (per e-Mail, Instant Messaging-Diensten, Twitter, Growl, usw.), damit er oder sie können das Problem beheben. (CI-Servern können auch das Commit ablehnen treten Fehler auf, der aufgerufen wird, eine "Abgegrenzter Eincheckvorgang".)

Das folgende Diagramm veranschaulicht diesen Prozess:

[![](intro-to-ci-images/intro01-small.png "Dieses Diagramm veranschaulicht diesen Prozess")](intro-to-ci-images/intro01.png#lightbox)

Mobile apps stellen individuelle Herausforderungen für die fortlaufende Integration vor. Apps erfordern Sensoren z. B. GPS- oder Kamera, die nur auf physischen Geräten verfügbar sind. Darüber hinaus Simulatoren Emulatoren sind nur eine Schätzung der Hardware und möglicherweise zu verbergen oder Probleme verdecken. Letztlich ist es zum Testen einer mobilen app auf echter Hardware sicher sein, dass sie tatsächlich Kunden bereit ist.

Die [App Center Test](https://docs.microsoft.com/en-us/appcenter/test-cloud) behebt dieses Problem durch das Testen von apps direkt auf Hunderten von physischen Geräten. Entwickler schreiben automatisierter Akzepte Tests, die leistungsstarke Testen der Benutzeroberfläche ermöglichen. Nachdem diese Tests App Center hochgeladen wurden, kann die CI-Server diese dort ausführen automatisch als Teil einer CI-Prozess wie im folgenden Diagramm dargestellt:

[![](intro-to-ci-images/intro02-small.png "Nachdem diese Tests App Center hochgeladen wurden, kann der CI-Server werden automatisch als Teil einer CI-Prozess ausführen, wie in diesem Diagramm angezeigt")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Komponenten von Continuous Integration

Es ist eine umfassende Ökosystem von kommerziellen und Open-Source-Tools für die Unterstützung CI ausgelegt. Dieser Abschnitt erklärt die gängigsten wenige.

## <a name="version-control"></a>Quellcodeverwaltung

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services und Team Foundation Server

[Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs) (VSTS) und [Team Foundation Server](http://msdn.microsoft.com/en-us/vstudio/ff637362.aspx) (TFS) sind Microsofts Zusammenarbeitstools für die fortlaufende Integration erstellen Dienste, Aufgabe nachverfolgen, agile Planung und reporting-Tools und Version -Steuerelement. Mit der Versionskontrolle können VSTS und TFS mit seinem eigenen System (Team Foundation-Versionskontrolle oder TFVC) oder mit Projekte auf GitHub gehostet arbeiten.

 - Visual Studio Team Services stellt Dienste über die Cloud bereit. Der wichtigste Vorteil ist, dass keine dedizierte Hardware oder der Infrastruktur und möglich, die von überall über Webbrowser und gängige Entwicklungstools wie Visual Studio, somit ansprechend für Teams, die geografisch sind verteilt. Es ist kostenlos Entwicklerteams betreut fünf oder weniger nach, welche zusätzlichen Lizenzen erworben werden können, um einen wachsenden Team aufzunehmen.
 - TFS ist für lokale Windows-Servern entworfen und erfolgt über ein lokales Netzwerk oder eine VPN-Verbindung mit dem Netzwerk. Der wichtigste Vorteil ist, dass Sie vollständig die Konfiguration der Buildserver können und können beliebige zusätzliche Software oder Dienste erforderlich sind. TFS weist auf eine kostenlose Einstiegsebene Express-Edition für kleine Teams.

TFS und der VSTS sind eng in Visual Studio integriert, und es Entwicklern ermöglichen, die viele Versionskontrolle sowie die CI-Aufgaben aus dem Komfort von einer einzigen IDE ausführen. Die Team Explorer Everywhere-Plug-In für Eclipse (siehe unten) ist ebenfalls verfügbar. Visual Studio für Mac bietet keine Unterstützung für TFS oder VSTS.

Visual Studio Team Service Buildsystem hat direkte Unterstützung für die Xamarin-Projekten, anhand derer Sie eine Builddefinition für jede Plattform erstellen, die Sie Ziel (Android, iOS und Windows möchten). Für jede Builddefinition ist die entsprechende Xamarin-Lizenz erforderlich. Es ist auch möglich, eine lokale Verbindung, Xamarin-fähige TFS-Buildserver zu Visual Studio Team Services für diesen Zweck. Mit diesem Setup werden Builds, die in VSTS Warteschlange befinden, auf dem lokalen Server delegiert werden. Weitere Informationen finden Sie in [bereitstellen und Konfigurieren eines Buildservers](https://msdn.microsoft.com/en-us/library/ms181712.aspx). Alternativ können Sie einen anderen Build-Tools wie Jenkins oder ein Team Ort.

Eine vollständige Zusammenfassung aller Funktionen von Application Lifecycle Management (ALM) von Visual Studio, Visual Studio Team Services und Team Foundation Server finden Sie unter [Application Lifecycle Management mit Xamarin-Apps](https://msdn.microsoft.com/en-us/library/mt162217(v=vs.140).aspx) auf MSDN.


### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](http://msdn.microsoft.com/en-us/library/gg413285.aspx) die Leistungsfähigkeit von Team Foundation Server und Visual Studio Team Services, die Teams, die außerhalb von Visual Studio entwickeln. Sie ermöglicht Entwicklern von Verbindung mit Teamprojekten, die lokal oder in der Cloud Eclipse oder die plattformübergreifenden Befehlszeilenclient für OS X und Linux. Team Explorer Everywhere vollständige bietet Zugriff auf Arbeitsaufgaben zur Versionskontrolle (z. B. Git,), und erstellen Funktionen für nicht-Windows-Plattformen.


### <a name="git"></a>Git

[Git](http://git-scm.com) ist eine beliebte open Source-Version Steuerelement-Lösung, die ursprünglich zum Verwalten des Quellcodes für den Linux-Kernel entwickelt wurde. Es ist ein sehr schnelle, flexible System, das häufig mit von Softwareprojekten beliebiger Größe verwendet wird. Skalierung ist hier einfach von einzelnen Entwicklern mit schlechter Zugang zum Internet mit großen Teams, die die Kugel umfassen. Git macht auch das Verzweigen sehr einfach, mit denen wiederum parallele Datenströme der Entwicklung mit minimalem Risiko empfehlen können.

Git kann ausgeführt werden, vollständig über einen Webbrowser oder über [GUI Clients](http://git-scm.com/downloads/guis) , die unter Linux, Mac OS x und Windows ausgeführt. Es ist kostenlos für öffentlichen Repositorys; Private-Repositorys erfordern eine [kostenpflichtigen Plan](https://github.com/pricing).

Visual Studio 2015 und Visual Studio für Mac bieten systemeigene Unterstützung für Git; für frühere Versionen, Microsoft bietet eine [herunterladbare Erweiterung für Git](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Wie oben bereits erwähnt, kann Visual Studio Team Services und TFS Git für die anstelle der TFVC-Versionskontrolle verwenden.


### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) ist eine beliebte open Source-Versionskontrollsystem, die seit 2000 verwendet wurde. SVN wird auf allen modernen Versionen OS X, Windows, FreeBSD, Linux und Unix ausgeführt werden. Visual Studio für Mac wurde die systemeigene Unterstützung für SVN. Es sind Erweiterungen von Drittanbietern, die SVN-Unterstützung in Visual Studio zu versetzen.


## <a name="continuous-integration-environments"></a>Continuous Integration-Umgebung

Einrichten einer Umgebung mit continuous Integration bedeutet ein Versionskontrollsystem mit Builddienst kombinieren.  Sind für die letztgenannte Aufgabe die zwei gängigsten aus:

- [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) ist das Buildsystem von Visual Studio Team Services und TFS. Er arbeitet eng mit Visual Studio, welche macht es zweckmäßig sein, für Entwickler zum Auslösen erstellt wurde, automatisch Tests ausführen und die Ergebnisse anzuzeigen.
- Jenkins ist ein Open Source-CI-Server, die mit einem rich-Ökosystem von Plug-Ins aus, um alle Arten der Softwareentwicklung zu unterstützen. Unter Windows ausgeführt wird, und Mac OS X. Jenkins ist nicht in jeder bestimmte IDE integriert. Stattdessen wird konfiguriert und verwaltet eine Web-Schnittstelle. Jenkins CI ist auch zum Installieren und konfigurieren, für kleine Teams ansprechend ist einfach.

Sie können TFS/VSTS allein verwenden, oder Sie können Jenkins verwenden, in Kombination mit TFS/VSTS oder Git, wie in den folgenden Abschnitten beschrieben.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services und Team Foundation Server

Wie bereits erwähnt, bietet Visual Studio Team Services und Team Foundation Server sowohl Version steuern und Dienste zu erstellen. Builddienste erfordern immer eine Xamarin Business oder Enterprise-Lizenz für jede Zielplattform.

Mit Visual Studio Team Services erstellen eine separate Builddefinition für jede Zielplattform, und geben Sie die entsprechende Lizenz vorhanden. Nach der Konfiguration ausführen VSTS Builds und Tests in der Cloud. Finden Sie unter [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) Weitere Details.

Mit Team Foundation Server konfigurieren Sie einen Buildcomputer für bestimmte Zielplattformen wie folgt:

- **Android und Windows:** Visual Studio installieren und die Xamarin-tools (für Android und Windows beide) und mit der Xamarin-Lizenzen konfigurieren. Es ist auch erforderlich, verschieben Sie die Android-SDK auf einem freigegebenen Speicherort auf dem Server, auf dem das TFS--Agent Build-gefunden werden kann. Weitere Informationen finden Sie unter [TFVC konfigurieren](https://docs.microsoft.com/vsts/tfvc/overview).
- **iOS und Xamarin:** installieren Sie Visual Studio und Xamarin-Tools auf dem WindowsServer mit der entsprechenden Lizenz. Installieren Sie Visual Studio für Mac auf ein Netzwerk zugänglichen Mac OS X-Computer, die dient als Host für Build und erstellen die endgültigen app-Paket (IPA für iOS-APP für OS X).

Das folgende Diagramm veranschaulicht diese Topografie:

[![](intro-to-ci-images/intro03-small.png "Dieses Diagramm veranschaulicht diese Topografie")](intro-to-ci-images/intro03.png#lightbox)

Es ist auch möglich, einen lokalen TFS-Server zu einem Visual Studio Team Services-Projekt verknüpfen, sodass VSTS-Builds auf den lokalen Server delegiert werden. Weitere Informationen finden Sie unter [bereitstellen und Konfigurieren eines Buildservers](http://msdn.microsoft.com/en-us/library/ms181712.aspx) auf MSDN.

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services und Jenkins

Wenn Sie Jenkins verwenden, um Ihre apps zu erstellen, können Sie Speichern Ihres Codes in Visual Studio Team Services oder Team Foundation Server und weiter Jenkins CI Builds verwendet. Wenn Sie einen Code push an das Teamprojekt Git-Repository oder wenn Sie Code in TFVC einchecken, können Sie einen Build Jenkins auslösen. Weitere Informationen finden Sie unter [Jenkins mit Visual Studio Team Services](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Wenn Sie Jenkins verwenden, um Ihre apps zu erstellen, können Sie Speichern Ihres Codes in Visual Studio Team Services oder Team Foundation Server und weiter Jenkins CI Builds verwendet")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git und Jenkins

Eine andere häufige CI-Umgebung kann OS X-Basis vollständig sein. Dieses Szenario müssen Sie Git für die quellcodeverwaltung und Jenkins mit dem für den Buildserver. Beide werden auf einem einzelnen Mac OS X-Computer mit Visual Studio für Mac installiert ausgeführt. Dies ist vergleichbar mit der Visual Studio Team Services + Jenkins-Umgebung, die im vorherigen Abschnitt erläutert:

[![](intro-to-ci-images/intro05-small.png "Dies ist vergleichbar mit der Visual Studio Team Services + Jenkins-Umgebung, die im vorherigen Abschnitt erläuterten")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Hinweis: Jenkins ist [nicht unterstützt von Xamarin](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**


# <a name="summary"></a>Zusammenfassung

Dieses Dokument wurde eingeführt, das Konzept der kontinuierlichen Integration und die Vorteile, die es von Softwareentwicklungsteams messbar Skriptentwicklern bereitstellt. Die Wichtigkeit der Versionskontrolle wurde zusammen mit der Rolle und Verantwortung der Build-Server behandelt. Das Dokument ging anschließend auf, um einige der Tools zu erörtern, die für die quellcodeverwaltung verwendet werden können und Buildserver. Darüber hinaus wird die App Center Test, der können Entwickler großartige apps durch Ausführen von automatisierten Tests, die Qualität und Funktionalität von apps nachweisen werden veröffentlicht wurde. Dokumentation zum Senden von apps und Tests App Center finden Sie ausführlichere [hier](https://docs.microsoft.com/en-us/appcenter/test-cloud). Schließlich, um zu erfahren, wie diese Tools und Komponenten miteinander verbunden sind, erläutert wir mehrere unterschiedliche CI-Umgebungen, die Organisationen für die fortlaufende Integration herstellen können. Weitere Informationen zum Verwenden von Visual Studio Team Services und Team Foundation Server mit Xamarin-Projekten finden Sie unter [konfigurieren TFVC](https://docs.microsoft.com/vsts/tfvc/overview) und dadurch [Continuous Integration-Einführung](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1). Ebenso, wenn Sie mit der Jenkins verwenden, finden Sie unter [Jenkins mit Xamarin verwenden](~/tools/ci/jenkins-walkthrough.md) ausführliche Informationen zum Einrichten einer kontinuierlichen Integration.