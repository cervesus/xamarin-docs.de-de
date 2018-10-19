---
title: Einführung in Continuous Integration mit Xamarin
description: Dieses Dokument beschreibt die fortlaufende Integration in Xamarin. Es wird erläutert, Versionskontrolle und verschiedenen Umgebungen für fortlaufende Integration.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: lobrien
ms.author: laobri
ms.date: 07/19/2017
ms.openlocfilehash: 5468495885e3af2afa2692ccad9191b669fa3328
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "37066506"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Einführung in Continuous Integration mit Xamarin

_Continuous Integration ist eine Software-Engineering-Methode in der ein automatisiertes Builds kompiliert und überprüft optional eine app aus, wenn Code hinzugefügt oder, indem Entwickler im Repository der Versionskontrolle des Projekts geändert. In diesem Artikel besprechen die allgemeinen Konzepte der Continuous Integration und einige der verfügbaren Optionen für Continuous Integration mit Xamarin-Projekte._

Es ist häufig bei Softwareprojekten für Entwickler gleichzeitig arbeiten. An einem gewissen Punkt ist es erforderlich, alle diese Integration parallele Datenströme mit der Arbeit in einer Codebasis, aus denen das endgültige Produkt besteht. In den Anfängen der Softwareentwicklung erfolgt diese Integration am Ende eines Projekts, das war ein schwieriger und riskanten Prozess.

Continuous Integration (CI) vermeiden eine solche Komplexität, indem Sie die Codebasis auf kontinuierlicher Basis jedes Entwicklers Änderungen zusammenführen, in der Regel, wenn alle Entwickler checkt freigegebene Änderungen des Projekts Code-Repository. Jedes Eincheckvorgangs in einen automatisierten Buildvorgang auslöst, und führt automatisierte Tests aus, um sicherzustellen, dass der neu eingeführte Code kein vorhandenen Code beeinträchtigt haben.  Auf diese Weise CI deckt Fehler und Probleme sofort und stellt sicher, dass alle Teammitglieder auf dem neuesten Stand mit anderer Arbeit bleiben. Dies führt zu einer kohärenteren und stabile Codebasis.

Continuous Integration-Systeme haben zwei Hauptkomponenten:

- **Versionskontrolle** – Version Control (VC), auch als Datenquellen-Steuerelement oder die quellcodeverwaltung, eines Projekts Code in ein einzelnes freigegebenes Repository konsolidiert und behält einen vollständigen Verlauf aller Änderungen in jeder Datei. Dieses Repository, bezeichnet als die *mainline* oder *master* verzweigen, enthält den Quellcode, die letztlich auf die Produktion zu erstellen oder eine Releaseversion der app verwendet werden. Es gibt viele open Source- und kommerziellen Produkten für diese Aufgabe, die in der Regel ermöglichen Teams oder Einzelpersonen, die eine Kopie des Codes in sekundären Verzweigungen zu verzweigen, wenn sie umfangreiche Änderungen vornehmen können, oder führen Sie Experimente, ohne das Risiko für den master-Branch. Sobald Änderungen an einem sekundären Branch überprüft werden, können sie dann alle zusammen werden wieder in den master-Branch zusammengeführt.
- **Continuous Integration-Server** – die Continuous Integration-Server ist verantwortlich für das Sammeln aller Elemente eines Projekts, (Quellcode, Bilder, Videos, Datenbanken, automatisierte Tests, usw.), die app zu kompilieren und Ausführen von automatisierten Tests. In diesem Fall stehen viele open Source- und kommerziellen CI-Server-Tools.

Entwickler müssen in der Regel eine Arbeitskopie der einem oder mehreren Branches auf ihren Arbeitsstationen, die anfänglich, in denen gearbeitet wird. Nachdem Sie ein entsprechenden Satz von Aufgaben abgeschlossen ist, werden die Änderungen "eingecheckt" oder "Commit", um den entsprechenden Branch, der sie an den Arbeitskopien von anderen Entwicklern weitergegeben. Dies ist wie ein Team sorgt dafür, die sie alle den gleichen Code arbeiten.

Mit continuous Integration verursacht der Vorgang des Commit für Änderungen in diesem Fall den CI-Server, auf das Projekt erstellen und Ausführen von automatisierten Tests, um die Richtigkeit des Quellcodes zu überprüfen. Wenn Fehler oder fehlgeschlagenen Tests vorhanden sind, benachrichtigt ein CI-Server den verantwortlichen Entwickler (per e-Mail, Instant-Messaging, Twitter, Growl, usw.), damit er oder sie können das Problem beheben. (CI-Server können auch den Commit ablehnen, wenn Fehler, die aufgerufen wird, eine auftreten "gated-Check-in".)

Das folgende Diagramm veranschaulicht diesen Prozess:

[![](intro-to-ci-images/intro01-small.png "Dieses Diagramm veranschaulicht diesen Prozess")](intro-to-ci-images/intro01.png#lightbox)

Mobile apps stellen besondere Herausforderungen für die kontinuierliche Integration vor. Apps unter Umständen Sensoren, z. B. GPS- oder Kamera, die nur auf physischen Geräten verfügbar sind. Darüber hinaus Simulatoren oder Emulatoren sind nur eine Näherung der Hardware zu verbergen oder Probleme verdecken. Letztlich ist es zum Testen einer mobilen app auf echter Hardware, um sicher zu sein, dass es sich um echte Kunden bereit ist.

Die [App Center Test](https://docs.microsoft.com/appcenter/test-cloud) löst dieses bestimmte Problem durch das Testen von apps direkt auf Hunderten von physischen Geräten. Entwickler schreiben automatisierter Annahme-Tests, die leistungsstarke UI-Tests zu ermöglichen. Nachdem diese Tests in App Center hochgeladen wurden, kann der CI-Server werden automatisch im Rahmen eines CI-Prozesses ausführen wie im folgenden Diagramm dargestellt:

[![](intro-to-ci-images/intro02-small.png "Nachdem diese Tests in App Center hochgeladen wurden, kann der CI-Server diese automatisch im Rahmen eines CI-Prozesses wie in diesem Diagramm gezeigt ausführen")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Komponenten der fortlaufenden Integration

Es ist einem umfangreichen Ökosystem aus kommerziellen und Open-Source-Tools entwickelt, um Unterstützung für CI. In diesem Abschnitt wird erläutert, nur einige der am häufigsten verwendeten Seiten.

## <a name="version-control"></a>Quellcodeverwaltung

### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps und Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/) und [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) werden die Zusammenarbeit von Microsoft-Tools für continuous Integration Services, nachverfolgung von Aufgaben, agile Planung und Berichterstellungstools, und Versionskontrolle erstellen. Mit Versionskontrolle kann Azure DevOps und TFS mit seinem eigenen System (Team Foundation-Versionskontrolle oder TFVC) oder mit Projekten, die auf GitHub gehostet verwendet werden.

- Visual Studio Team Services stellt die Dienste über die Cloud bereit. Der wichtigste Vorteil ist, dass es keine dedizierte Hardware oder die Infrastruktur erfordert und zugegriffen werden kann von überall über Webbrowser und gängige Entwicklungstools wie Visual Studio, das Produkt attraktiver für Teams, die geografisch liegen verteilt. Es ist kostenlos für Teams aus fünf Entwicklern oder weniger nach der weiteren Lizenzen erworben werden können, die zur Aufnahme Team weiter wächst.
- TFS ist für lokalen Windows-Servern entworfen und erfolgt über ein lokales Netzwerk oder eine VPN-Verbindung mit diesem Netzwerk. Ihr primäre Vorteil ist, dass Sie vollständig steuern die Konfiguration der Buildserver und installieren Sie können beliebige zusätzliche Software oder Dienste erforderlich sind. TFS verfügt über eine kostenlose auf Einstiegsebene Express-Edition für kleine Teams.

Sowohl TFS als auch Azure DevOps sind eng in Visual Studio integriert und ermöglichen es Entwicklern, viele Versionskontrolle und CI-Aufgaben aus, in dem Komfort von einer einzigen IDE ausführen. Der Team Explorer Everywhere-Plug-In für Eclipse (siehe unten) ist ebenfalls verfügbar. Visual Studio für Mac verfügt über [eine Vorschau der TFVC verfügbaren](/visualstudio/mac/tf-version-control/).

[Azure DevOps-Pipelines](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/) hat direkte Unterstützung für Xamarin-Projekte, in denen Sie eine Builddefinition für jede Plattform, die Sie Ziel (Android, iOS und Windows möchten) erstellen. Für jede Builddefinition wird die entsprechende Xamarin-Lizenz benötigt. Es ist auch möglich, eine lokale Verbindung, die Xamarin-fähige TFS-build Azure DevOps-Server für diesen Zweck. Mit diesem Setup werden Builds, die auf Azure DevOps in der Warteschlange befinden, auf dem lokalen Server delegiert werden. Weitere Informationen finden Sie unter [Build und release Agents](https://docs.microsoft.com/azure/devops/pipelines/agents/agents). Alternativ können Sie einen anderen Build-Tools wie Jenkins oder ein Team Ort.

Eine vollständige Zusammenfassung der alle Funktionen von Application Lifecycle Management (ALM) von Visual Studio, Azure DevOps und Team Foundation Server finden Sie unter [DevOps mit Xamarin-Apps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps).

### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) bringt die Leistungsfähigkeit von Team Foundation Server und Visual Studio Team Services für Teams, die außerhalb von Visual Studio entwickeln. Sie ermöglicht Entwicklern, die Verbindung zu Teamprojekten lokal oder in der Cloud über Eclipse oder den plattformübergreifenden Befehlszeilenclient für OS X und Linux. Team Explorer Everywhere, die bietet umfassenden Zugriff auf Arbeitsaufgaben in die Versionskontrolle (einschließlich Git), sowie auf Buildfunktionen für nicht-Windows-Plattformen.

### <a name="git"></a>Git

[Git](http://git-scm.com) ist eine beliebte open-Source-Version-Steuerelement-Lösung, die ursprünglich entwickelt wurde, um den Quellcode für den Linux-Kernel zu verwalten. Es ist ein sehr schnelle, flexible System, das häufig bei Softwareprojekten jeglicher Größe verwendet wird. Skalierung erfolgt einfach von einzelnen Entwicklern mit einer schlechten Internetzugriff auf große Teams, die die ganze Welt umspannen. Git ist auch eine Verzweigung sehr einfach, mit denen wiederum parallele Datenströme mit der Entwicklung mit minimalem Risiko empfehlen kann.

Git kann ausgeführt werden, ausschließlich mit einem Webbrowser oder über [GUI Clients](http://git-scm.com/downloads/guis) , die unter Linux, Mac OS x und Windows ausgeführt. Es ist kostenlos für öffentlichen Repositorys. Private Repositorys erfordern eine [kostenpflichtigen Plan](https://github.com/pricing).

Visual Studio 2015 und Visual Studio für Mac bieten systemeigenen Unterstützung für Git; Bei älteren Versionen Microsoft bietet eine [herunterladbare-Erweiterung für Git](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Wie bereits erwähnt, können Visual Studio Team Services und TFS Git für anstelle von TFVC-Versionskontrolle verwenden.

### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) ist ein beliebter open-Source-Versionskontrollsystem, die in Verwendung seit 2000 war. SVN wird auf allen modernen-Versionen von Windows, OS X, FreeBSD, Linux und Unix ausgeführt werden. Visual Studio für Mac verfügt über native Unterstützung für SVN. Es gibt Erweiterungen von Drittanbietern, die SVN-Unterstützung in Visual Studio bereitstellen.

## <a name="continuous-integration-environments"></a>Continuous Integration-Umgebungen

Das Einrichten einer Umgebung mit kontinuierlicher Integration bedeutet, dass ein System zur Versionskontrolle mit einem Builddienst kombinieren.  Im letzteren Fall sind die zwei häufigsten Gründe:

- [Azure-Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) ist das Buildsystem von Azure DevOps und TFS. Er arbeitet eng mit Visual Studio, welche macht es für Entwickler zum Auslösen erstellt wurde, automatisch Tests ausführen und die Ergebnisse angezeigt.
- Jenkins ist ein Open-Source-CI-Server, die mit einem umfangreichen Ökosystem mit unterstützen alle Arten der Entwicklung von Software-Plug-Ins. Es wird auf Windows und Mac OS X. Jenkins nicht in eine beliebige spezifische IDE integriert ist. Es wird stattdessen konfiguriert und die über eine Weboberfläche verwaltet. Jenkins CI ist auch einfach zu installieren und konfigurieren, die es attraktiv, die kleinen Teams ermöglicht.

Können Sie TFS/Azure DevOps selbst, oder Sie können Jenkins in Kombination mit TFS/Azure DevOps oder Git verwenden, wie in den folgenden Abschnitten beschrieben.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services und Team Foundation Server

Wie bereits erwähnt, bietet Visual Studio Team Services und Team Foundation Server sowohl Version steuern und Erstellung von Diensten. Builddienste erfordern immer eine Xamarin Business oder Enterprise-Lizenz für jede Zielplattform.

Mit Visual Studio Team Services erstellen eine separate Builddefinition für jede Zielplattform und geben Sie die entsprechende Lizenz vorhanden. Sobald konfiguriert ist, Azure DevOps wird ausführen erstellt, und Sie werden in der Cloud testet. Finden Sie unter [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) Weitere Details.

Mit Team Foundation Server konfigurieren Sie einen Buildcomputer für bestimmte Zielplattformen wie folgt aus:

- **Android und Windows:** Installieren von Visual Studio und Xamarin-tools (für Android und Windows beide), und mit Ihrem Xamarin-Lizenzen konfigurieren. Es ist auch notwendig ist, verschieben, dass das Android SDK an einem freigegebenen Speicherort auf dem Server, in dem TFS--Agent Build-gefunden werden kann. Weitere Informationen finden Sie unter [Konfigurieren von TFVC](https://docs.microsoft.com/azure/devops/repos/tfvc/overview).
- **iOS und Xamarin:** Installieren von Visual Studio und Xamarin-Tools auf dem Windows-Server mit der entsprechenden Lizenz. Danach installieren Sie Visual Studio für Mac, auf ein Netzwerk zugänglich Mac OS X-Computer, der dient als buildhost für und der endgültigen app-Paket für iOS-APP für OS X erstellen.

Das folgende Diagramm veranschaulicht diese Topografie:

[![](intro-to-ci-images/intro03-small.png "Dieses Diagramm veranschaulicht diese Topografie")](intro-to-ci-images/intro03.png#lightbox)

Es ist auch möglich, verknüpfen einen lokalen TFS-Server mit einem Visual Studio Team Services-Projekt, damit Azure DevOps-builds werden auf dem lokalen Server delegiert. Weitere Informationen finden Sie unter [Build und release Agents](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/).

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services und Jenkins

Wenn Sie Jenkins verwenden, um Ihre apps zu erstellen, können Sie Speichern von Code in Visual Studio Team Services oder Team Foundation Server und weiterhin Jenkins für die CI-Builds zu verwenden. Sie können einen Jenkins-Build auslösen, wenn Sie Code, der das Teamprojekt Git-Repository oder wenn Sie Code in TFVC Einchecken übertragen. Weitere Informationen finden Sie unter [Jenkins mit Azure DevOps](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Wenn Sie Jenkins verwenden, um Ihre apps zu erstellen, können Sie Speichern von Code in Visual Studio Team Services oder Team Foundation Server und verwenden Sie Jenkins für die CI-Builds weiterhin")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git und Jenkins

Eine andere allgemeine CI-Umgebung kann vollständig OS X-basiert sein. Dieses Szenario beinhaltet die Verwendung von Git für die quellcodeverwaltung und Jenkins für den Buildserver. Beide werden auf einem einzelnen Mac OS X-Computer mit Visual Studio für Mac installiert ausgeführt werden. Dies ist sehr ähnlich ist, auf Visual Studio Team Services und Jenkins-Umgebung, die im vorherigen Abschnitt erläutert:

[![](intro-to-ci-images/intro05-small.png "Dies ist sehr ähnlich ist, auf Visual Studio Team Services und Jenkins-Umgebung, die im vorherigen Abschnitt erläuterten")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins ist [nicht von Microsoft unterstützt](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**

# <a name="summary"></a>Zusammenfassung

In diesem Dokument wurde eingeführt, das Konzept der continuous Integration und die Vorteile, die für die Software-Entwicklungsteams bietet. Die Wichtigkeit der Versionskontrolle wurde zusammen mit den Rollen und Verantwortlichkeiten des Buildservers erläutert. Das Dokument hat sich auf einige der Tools behandelt, die für die quellcodeverwaltung verwendet werden können, und erstellen Server klicken Sie dann auf. Wir haben außerdem wird die App Center Test, wodurch Entwickler großartige apps zu veröffentlichen, durch Ausführen von automatisierten Tests, die die Qualität und Funktionalität ihrer Apps bestätigen werden eingeführt. Dokumentation zum Übermitteln von apps und Tests in App Center finden Sie ausführlichere [hier](https://docs.microsoft.com/appcenter/test-cloud). Schließlich, um zu erfahren, wie alle diese Tools und Komponenten miteinander verbunden sind, erläutert es mehrere verschiedene CI-Umgebungen, die Organisationen für die kontinuierliche Integration herstellen können. Weitere Informationen zur Verwendung von Visual Studio Team Services und Team Foundation Server mit Xamarin-Projekte, finden Sie unter [Konfigurieren von TFVC](https://docs.microsoft.com/azure/devops/repos/tfvc/overview/) und [Continuous Integration-Einführung](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer/). Auch wenn Sie Jenkins verwenden, finden Sie unter [mithilfe von Jenkins mit Xamarin](~/tools/ci/jenkins-walkthrough.md) ausführliche Informationen zum Einrichten einer continuous Integration.
