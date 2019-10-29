---
title: Einführung in Continuous Integration mit xamarin
description: In diesem Dokument wird Continuous Integration mit xamarin beschrieben. In diesem Thema werden die Versionskontrolle und verschiedene Continuous Integration Umgebungen erläutert.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: davidortinau
ms.author: daortin
ms.date: 07/19/2017
ms.openlocfilehash: 2862f05f2d183c9345d2b92268ddf2101cc2492e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029814"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Einführung in Continuous Integration mit xamarin

_Continuous Integration ist ein Software Entwicklungsverfahren, bei dem ein automatisierter Build eine APP kompiliert und optional testet, wenn Code von Entwicklern im Versionskontrollrepository des Projekts hinzugefügt oder geändert wird. In diesem Artikel werden die allgemeinen Konzepte der kontinuierlichen Integration und einige der Optionen erläutert, die für die fortlaufende Integration in xamarin-Projekte verfügbar sind._

Es ist üblich, dass Entwickler in Softwareprojekten parallel arbeiten. An einem gewissen Punkt ist es notwendig, alle diese parallelen Datenströme in eine Codebasis zu integrieren, aus der das endgültige Produkt besteht. In den frühen Tagen der Softwareentwicklung wurde diese Integration am Ende eines Projekts durchgeführt, was ein schwieriger und riskanter Prozess war.

Continuous Integration (CI) vermeidet solche Komplexität, indem alle Entwickler Änderungen regelmäßig in die gemeinsame Codebasis zusammengeführt werden. Dies ist in der Regel der Fall, wenn Entwickler Änderungen am freigegebenen Coderepository des Projekts einchecken. Jeder Eincheck Vorgang löst einen automatisierten Build aus und führt automatisierte Tests aus, um zu überprüfen, ob der neu eingeführte Code den vorhandenen Code nicht unterbricht.  Auf diese Weise werden Fehler und Probleme sofort durch CI behoben, und es wird sichergestellt, dass alle Teammitglieder auf dem neuesten Stand sind. Dies führt zu einer zusammenhängenden und stabilen CodeBase.

Continuous Integration-Systeme bestehen aus zwei Hauptkomponenten:

- Mit der **Versions** Kontrolle – Version Control (VC), auch als Quell Code Verwaltung oder Quell Code Verwaltung bezeichnet, wird der gesamte Code eines Projekts in einem einzelnen freigegebenen Repository konsolidiert, und es wird ein vollständiger Verlauf aller Änderungen an jeder Datei aufbewahrt. Dieses Repository, das häufig als *Haupt* oder *masterb* Ranch bezeichnet wird, enthält den Quellcode, der letztendlich verwendet wird, um die Produktions-oder Releaseversion der APP zu erstellen. Es gibt zahlreiche Open Source-und kommerzielle Produkte für diese Aufgabe, die es Teams oder Personen ermöglichen, eine Kopie des Codes in sekundäre Verzweigungen zu verzweigen, in denen Sie umfangreiche Änderungen vornehmen oder Experimente durchführen können, ohne dass die Haupt Verzweigung gefährdet ist. Nachdem die Änderungen in einer sekundären Verzweigung überprüft wurden, können Sie alle zusammen in den masterb Ranch zusammengeführt werden.
- **Continuous Integration-Server** – der Continuous Integration-Server ist für das Erfassen aller Artefakte eines Projekts (Quellcode, Bilder, Videos, Datenbanken, automatisierte Tests usw.), das Kompilieren der APP und das Ausführen der automatisierten Tests verantwortlich. Auch hier gibt es viele Open-Source-und kommerzielle CI-Server Tools.

Entwickler verfügen in der Regel über eine funktionierende Kopie von einer oder mehreren Verzweigungen auf ihren Arbeitsstationen, bei denen die Arbeit anfänglich erledigt ist. Sobald ein entsprechender Arbeits Satz abgeschlossen ist, werden die Änderungen in der entsprechenden Verzweigung, die Sie an die Arbeitskopien anderer Entwickler weitergeben, in der entsprechenden Verzweigung eingecheckt. Auf diese Weise stellt ein Team sicher, dass alle am gleichen Code arbeiten.

Mit Continuous Integration bewirkt das Ausführen eines Commit für Änderungen, dass der CI-Server das Projekt erstellt und automatisierte Tests ausführt, um die Richtigkeit des Quellcodes zu überprüfen. Wenn Buildfehler oder Test Fehler auftreten, benachrichtigt ein CI-Server den verantwortlichen Entwickler (per e-Mail, im, Twitter, Growl usw.), damit er das Problem beheben kann. (CI-Server können den Commit sogar ablehnen, wenn Fehler auftreten, was als "Gated Check-in" bezeichnet wird.)

Dieses Verfahren wird im folgenden Diagramm veranschaulicht:

[![](intro-to-ci-images/intro01-small.png "This diagram illustrates this process")](intro-to-ci-images/intro01.png#lightbox)

Mobile Apps stellen besondere Herausforderungen für Continuous Integration dar. Für apps sind möglicherweise Sensoren wie GPS oder Kameras erforderlich, die nur auf physischen Geräten verfügbar sind. Außerdem sind Simulatoren oder Emulatoren nur eine Näherung der Hardware und können Probleme verbergen oder verschleiern. Letztendlich ist es erforderlich, eine Mobile App auf echter Hardware zu testen, um sicherzustellen, dass Sie wirklich Kunden bereit ist.

Der [App Center Test](https://docs.microsoft.com/appcenter/test-cloud) adressiert dieses bestimmte Problem, indem apps direkt auf Hunderten von physischen Geräten getestet werden. Entwickler schreiben automatisierte Akzeptanz Tests, die leistungsstarke UI-Tests ermöglichen. Nachdem diese Tests in App Center hochgeladen wurden, kann der CI-Server Sie automatisch als Teil eines CI-Prozesses ausführen, wie in der folgenden Abbildung dargestellt:

[![](intro-to-ci-images/intro02-small.png "Once these tests are uploaded to App Center, the CI server can run them automatically as part of a CI process as shown in this diagram")](intro-to-ci-images/intro02.png#lightbox)

## <a name="components-of-continuous-integration"></a>Komponenten der kontinuierlichen Integration

Es gibt ein umfassendes Ökosystem aus kommerziellen und Open-Source-Tools, die für die Unterstützung von CI entwickelt wurden. In diesem Abschnitt werden einige der gängigsten erläutert.

### <a name="version-control"></a>Quellcodeverwaltung

#### <a name="azure-devops-and-team-foundation-server"></a>Azure devops und Team Foundation Server

[Azure devops](https://azure.microsoft.com/services/devops/) und [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) sind die Zusammenarbeits Tools von Microsoft für Continuous Integration Builddienste, Aufgaben Verfolgung, Agile-Planungs-und Bericht Erstellungs Tools sowie Versionskontrolle. Mit der Versionskontrolle können Azure devops und TFS mit einem eigenen System (Team Foundation-Versionskontrolle oder tfvc) oder mit Projekten arbeiten, die auf GitHub gehostet werden.

- Azure devops bietet Dienste über die Cloud. Der Hauptvorteil besteht darin, dass keine dedizierte Hardware oder Infrastruktur erforderlich ist und von jedem beliebigen Standort aus über Webbrowser und über gängige Entwicklungs Tools wie Visual Studio zugegriffen werden kann. Dies macht es für Teams attraktiv, die geografisch verteilt sind. . Es ist kostenlos für Teams mit fünf Entwicklern oder weniger, nach denen zusätzliche Lizenzen erworben werden können, um ein wachsendes Team zu unterstützen.
- TFS ist für lokale Windows-Server konzipiert, auf die über ein lokales Netzwerk oder eine VPN-Verbindung mit diesem Netzwerk zugegriffen wird. Der Hauptvorteil besteht darin, dass Sie die Konfiguration der Buildserver vollständig steuern und jede beliebige zusätzliche Software oder alle benötigten Dienste installieren können. TFS verfügt über eine kostenlose Express Edition auf Einstiegsebene für kleine Teams.

Sowohl TFS als auch Azure devops sind eng in Visual Studio integriert und ermöglichen Entwicklern das Ausführen vieler Versionskontrolle und CI-Aufgaben aus dem Komfort einer einzelnen IDE. Das Team Explorer Everywhere-Plug-in für Eclipse (siehe unten) ist ebenfalls verfügbar. Visual Studio für Mac [eine Vorschau von tfvc verfügbar](/visualstudio/mac/tf-version-control/).

[Azure devops-Pipelines](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/) verfügen über direkte Unterstützung für xamarin-Projekte, in denen Sie eine Builddefinition für jede Zielplattform erstellen (Android, IOS und Windows). Die entsprechende xamarin-Lizenz ist für jede Builddefinition erforderlich. Zu diesem Zweck ist es auch möglich, einen lokalen, xamarin-fähigen TFS-Buildserver mit Azure devops zu verbinden. Bei diesem Setup werden Builds, die in Azure devops in die Warteschlange eingereiht werden, an den lokalen Server delegiert. Weitere Informationen finden Sie unter [Build-und Release-Agents](https://docs.microsoft.com/azure/devops/pipelines/agents/agents). Alternativ dazu können Sie auch ein anderes Buildtool verwenden, z. b. Jenkins oder teamstadt.

Eine vollständige Zusammenfassung aller Application Lifecycle Management (ALM)-Features von Visual Studio, Azure devops und Team Foundation Server finden Sie unter [devops mit xamarin-apps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps).

#### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) bringt die Leistungsfähigkeit von Team Foundation Server und Azure devops für Teams, die sich außerhalb von Visual Studio entwickeln. Sie ermöglicht es Entwicklern, über Eclipse oder den plattformübergreifenden Befehlszeilen Client für OS X und Linux eine Verbindung mit Team Projekten lokal oder in der Cloud herzustellen. Team Explorer Everywhere bietet Vollzugriff auf die Versionskontrolle (einschließlich git), Arbeitsaufgaben und Buildfunktionen für nicht-Windows-Plattformen.

#### <a name="git"></a>Git

[Git](https://git-scm.com) ist eine beliebte Open Source-Versions Kontrolllösung, die ursprünglich zum Verwalten des Quellcodes für den Linux-Kernel entwickelt wurde. Es handelt sich um ein sehr schnelles, flexibles System, das bei Softwareprojekten aller Größen beliebt ist. Es ist problemlos von einzelnen Entwicklern mit mangelhafter Internet Zugang zu großen Teams skaliert, die sich auf der ganzen Welt befinden. Git sorgt auch für eine sehr einfache Verzweigung, die wiederum parallele entwicklungsströme mit minimalem Risiko fördern kann.

Git kann vollständig über Webbrowser oder GUI- [Clients](https://git-scm.com/downloads/guis) ausgeführt werden, die unter Linux, Mac OSX und Windows ausgeführt werden. Es ist kostenlos für öffentliche Depots verfügbar. Private Depots benötigen einen [kostenpflichtigen Plan](https://github.com/pricing).

Die aktuellen Versionen von Visual Studio für Windows und Mac bieten native Unterstützung für git. Microsoft bietet eine [herunterladbare Erweiterung für git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c) für ältere Versionen von Visual Studio. Wie bereits erwähnt, können Azure devops und TFS git anstelle von tfvc für die Versionskontrolle verwenden.

#### <a name="subversion"></a>Sub

[Subversion](https://subversion.apache.org) (SVN) ist ein gängiges Open-Source-System für die Versionskontrolle, das seit 2000 verwendet wird. SVN kann auf allen modernen Versionen von OS X, Windows, FreeBSD, Linux und UNIX ausgeführt werden. Visual Studio für Mac verfügt über native Unterstützung für svn. Es gibt Erweiterungen von Drittanbietern, die die SVN-Unterstützung für Visual Studio bieten.

### <a name="continuous-integration-environments"></a>Continuous Integration-Umgebungen

Das Einrichten einer Continuous Integration Umgebung bedeutet das Kombinieren eines Versionskontrollsystems mit einem Builddienst.  Letztere sind die beiden häufigsten:

- [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) ist das Buildsystem von Azure devops und TFS. Es ist eng in Visual Studio integriert, sodass Entwickler das Auslassen von Builds, Automatisches Ausführen von Tests und Anzeigen der Ergebnisse erleichtern können.
- Jenkins ist ein Open-Source-CI-Server mit einem umfangreichen Ökosystem von Plug-ins, um alle Arten der Softwareentwicklung zu unterstützen. Es wird unter Windows und Mac OS X ausgeführt. Jenkins ist nicht in eine bestimmte IDE integriert. Stattdessen wird es über eine Weboberfläche konfiguriert und verwaltet. Jenkins CI ist auch einfach zu installieren und zu konfigurieren, sodass es für kleine Teams attraktiv ist.

Sie können TFS/Azure devops eigenständig verwenden, oder Sie können Jenkins in Kombination mit TFS/Azure devops oder git verwenden, wie in den folgenden Abschnitten beschrieben.

#### <a name="azure-devops-and-team-foundation-server"></a>Azure devops und Team Foundation Server

Wie bereits erläutert, bietet Azure devops und Team Foundation Server sowohl Versionskontrolle als auch Builddienste. Builddienste benötigen immer eine xamarin Business-oder Enterprise-Lizenz für jede Zielplattform.

Mit Azure devops erstellen Sie eine separate Builddefinition für jede Zielplattform und geben dort die entsprechende Lizenz ein. Nach der Konfiguration führt Azure devops Builds und Tests in der Cloud aus. Weitere Informationen finden Sie unter [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) .

Mit Team Foundation Server konfigurieren Sie einen Buildcomputer für bestimmte Zielplattformen wie folgt:

- **Android und Windows:** Installieren Sie Visual Studio und die xamarin-Tools (für Android und Windows), und konfigurieren Sie Sie mit ihren xamarin-Lizenzen. Außerdem ist es erforderlich, die Android SDK an einen freigegebenen Speicherort auf dem Server zu verschieben, auf dem der TFS-Build-Agent Sie finden kann. Weitere Informationen finden Sie unter [Konfigurieren von tfvc](https://docs.microsoft.com/azure/devops/repos/tfvc/overview).
- **IOS und xamarin:** Installieren Sie Visual Studio und die xamarin-Tools auf dem Windows-Server mit der entsprechenden Lizenz. Installieren Sie dann Visual Studio für Mac auf einem über das Netzwerk zugänglichen Mac OS X Computer, der als buildhost fungieren und das endgültige App-Paket erstellen soll (IPA für IOS, App für OS X).

Die folgende Abbildung veranschaulicht diese Topografie:

[![](intro-to-ci-images/intro03-small.png "This diagram illustrates this topography")](intro-to-ci-images/intro03.png#lightbox)

Es ist auch möglich, einen lokalen TFS-Server mit einem Azure devops-Projekt zu verknüpfen, sodass Azure devops-Builds an den lokalen Server delegiert werden. Weitere Informationen finden Sie unter [Build-und Release-Agents](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/).

#### <a name="azure-devops-and-jenkins"></a>Azure devops und Jenkins

Wenn Sie Ihre apps mit Jenkins erstellen, können Sie Ihren Code in Azure devops oder Team Foundation Server speichern und weiterhin Jenkins für Ihre CI-Builds verwenden. Sie können einen Jenkins-Build auslöst, wenn Sie den Code per Push in das git-Repository Ihres Teamprojekts übersetzen oder Code in tfvc einchecken. Weitere Informationen finden Sie unter [Jenkins mit Azure devops](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "If you use Jenkins to build your apps, you can store your code in Azure DevOps or Team Foundation Server and continue to use Jenkins for your CI builds")](intro-to-ci-images/intro04.png#lightbox)

#### <a name="git-and-jenkins"></a>Git und Jenkins

Eine andere allgemeine CI-Umgebung kann vollständig OS X-basiert sein. Dieses Szenario umfasst die Verwendung von git für die Quell Code Verwaltung und Jenkins für den Buildserver. Beide werden auf einem einzelnen Mac OS X Computer ausgeführt, auf dem Visual Studio für Mac installiert ist. Dies ähnelt der Azure devops + Jenkins-Umgebung, die im vorherigen Abschnitt erläutert wurde:

[![](intro-to-ci-images/intro05-small.png "This is very similar to the Azure DevOps + Jenkins environment discussed in the previous section")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins wird [von Microsoft nicht unterstützt](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**
