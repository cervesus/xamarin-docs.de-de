---
title: Verwenden von Jenkins mit Xamarin
description: Dieses Dokument beschreibt die Verwendung von Jenkins für die fortlaufende Integration in Xamarin-Anwendungen. Es wird erläutert, wie installieren, konfigurieren und Verwenden von Jenkins.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: 7f66c97ce4b7880d32dfd87aec0691a26a08cfd2
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669426"
---
# <a name="using-jenkins-with-xamarin"></a>Verwenden von Jenkins mit Xamarin

_Dieser Leitfaden veranschaulicht, wie Sie Jenkins als continuous Integrationsserver einrichten und automatisieren, kompilieren mobile Anwendungen mit Xamarin erstellt wurden. Es wird beschrieben, wie zum Installieren von Jenkins auf OS X, konfigurieren und Einrichten der Aufträge, Xamarin.iOS und Xamarin.Android-Anwendungen kompilieren, wenn Änderungen an der Quellcodeverwaltungssystem übergeben werden._

[Einführung in die fortlaufende Integration in Xamarin](~/tools/ci/intro-to-ci.md) kontinuierliche Integration wie eine nützliche Softwareentwicklungsverfahren, die und frühzeitige Warnungen von fehlerhaften oder nicht kompatiblen Code liefert führt. CI ermöglicht Entwicklern, Probleme und Probleme, sobald sie auftreten, und speichert in einem geeigneten Zustand für die Bereitstellung die Software. In dieser exemplarischen Vorgehensweise wird beschrieben, wie den Inhalt aus beiden Dokumente zusammen verwenden werden.

Dieses Handbuch veranschaulicht, wie Jenkins auf einem dedizierten Computer mit OS X installieren und konfigurieren Sie ihn, die automatisch ausgeführt, wenn der Computer wird gestartet. Nachdem Jenkins installiert ist, wird zur Unterstützung von MS Build zusätzliche Plug-Ins installiert. Jenkins unterstützt Git von der. Wenn Sie TFS für quellcodeverwaltung verwendet wird, müssen auch eine zusätzliche-Plug-Ins und -Befehlszeile Hilfsprogramme installiert werden.

Nachdem Jenkins konfiguriert ist, und alle erforderlichen Plug-Ins installiert wurde, erstellen wir ein oder mehrere Aufträge zum Kompilieren der Xamarin.Android- und Xamarin.iOS-Projekte. Ein Auftrag ist eine Sammlung von Schritten und Metadaten erforderlich, um einige Aufgaben ausführen. Ein Auftrag besteht aus in der Regel Folgendes:

- **Source Code Management (SCM)** – Dies ist eine Meta-Data-Eintrag in den Jenkins-Konfigurationsdateien, Informationen zum Herstellen einer Verbindung mit quellcodeverwaltung und die Dateien enthält abgerufen.
- **Trigger** – Trigger verwendet, um das Starten eines Auftrags basierend auf bestimmte Aktionen ausführen, z. B. wenn ein Entwickler Änderungen an der Quellcode-Repository committet.
- **Einrichtungsanweisungen** – Dies ist ein Plug-in oder ein Skript, das den Quellcode kompilieren und erstellen eine Binärdatei, die auf mobilen Geräten installiert werden kann.
- **Optionale erstellen Aktionen** – dazu gehören die Ausführen von Komponententests, statischen Analyse auf das Codesignaturzertifikat, Code ausgeführt, oder starten einen anderen Auftrag zum Ausführen anderer verwandten Arbeitsaufgaben erstellen.
- **Benachrichtigungen** – ein Auftrag kann eine Art von Benachrichtigung über den Status eines Builds senden.
- **Sicherheit** – zwar optional, es wird dringend empfohlen, dass auch die Jenkins-Sicherheitsfunktionen.

Dieses Handbuch führt durch die Schritte zum Einrichten eines Jenkins-Servers, der alle diese Punkte behandelt. Am Ende des Zertifikats sollten wir ein gutes Verständnis der Funktionsweise zum Einrichten und Konfigurieren von Jenkins zum Erstellen der IPA-Datei und APKS für unsere mobile Xamarin-Projekte sind.

## <a name="requirements"></a>Anforderungen

Die ideale Build-Server ist einem eigenständigen Computer zu erstellen und testen die Anwendung möglicherweise der einzige Zweck dediziert. Ein dedizierter Computer wird sichergestellt, dass Elemente, die für andere Rollen (z. B. eines Webservers) erforderlich sind, nicht den Build verfälschen. Wenn der Build-Server auch als Webserver fungiert, kann der Webserver z. B. eine in Konflikt stehende Version einige allgemeine Bibliothek erfordern. Aufgrund dieses Konflikts des Servers möglicherweise nicht ordnungsgemäß, oder Jenkins möglicherweise erstellen Sie Builds, die bei der Bereitstellung für Benutzer nicht funktionieren.

Der Build-Server für die mobile Xamarin-apps ist sehr ähnlich wie die Arbeitsstation eines Entwicklers einrichten. Es verfügt über ein Benutzerkonto in der Jenkins, Visual Studio für Mac, und Xamarin.iOS und Xamarin.Android installiert werden. Der gesamte Code Signieren von Zertifikaten, die Bereitstellung von Profilen und Schlüsselspeicher muss ebenfalls installiert werden. *In der Regel die Buildserver-Benutzerkontos unterscheidet sich von Ihrem entwicklerkonten – Achten Sie darauf, dass Sie zum Installieren und konfigurieren alle der Software, Schlüssel und Zertifikate, die während der Anmeldung über die Build-Server-Benutzerkontos ein.*

Das folgende Diagramm zeigt alle diese Elemente in einem typischen Jenkins-Build-Server:

[![](jenkins-walkthrough-images/image1.png "Dieses Diagramm veranschaulicht die alle diese Elemente auf einem typischen Jenkins-Buildserver")](jenkins-walkthrough-images/image1.png#lightbox)

iOS-Anwendungen können nur erstellt und auf einem Computer unter MacOS signiert werden. Ein einfacher Mac ist eine sinnvolle Option für die Kosten, aber alle Computer, die OS X 10.10 (Yosemite) ausgeführt werden kann oder höher ist ausreichend.

Wenn Sie TFS für quellcodeverwaltung verwendet wird, sollten Sie installieren [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/). Team Explorer Everywhere bietet plattformübergreifenden Zugriff auf TFS an einem Terminal in MacOS.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Installieren von Jenkins

Die erste Aufgabe, die mithilfe von Jenkins ist installiert. Es gibt drei Möglichkeiten zur Ausführung von Jenkins auf OS X:

- Als Daemon, im Hintergrund ausgeführt.
- In einem Servletcontainer, z. B. Tomcat oder Jetty, JBoss werden soll.
- Als normalen Prozess unter einem Benutzerkonto ausgeführt wird.

Die meisten herkömmliche continuous integrationsanwendungen im Hintergrund ausgeführt werden, entweder als Daemon (unter OS X oder \*Nix) oder als Dienst (unter Windows). Dies ist für Szenarien geeignet, in denen es gibt keine GUI-Interaktion erforderlich sind, und das Setup von der Buildumgebung problemlos ausgeführt werden kann. Mobile apps erfordern auch Schlüsselspeichern und Signieren von Zertifikaten, die für den Zugriff auf problematisch sein können, wenn Jenkins als Daemon ausgeführt wird. Aufgrund dieser Aspekte können Schwerpunkt das dritte Szenario – Jenkins unter einem Benutzerkonto ausgeführt, auf dem Buildserver dieses Dokuments.

Jenkins.App ist eine praktische Möglichkeit zum Installieren von Jenkins. Dies ist ein AppleScript-Wrapper, der vereinfacht das Starten und Beenden eines Jenkins-Servers. Statt in einer Bash-Shell ausführen, führt Jenkins als eine app mit Symbol im Dock, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image2.png "Statt in einer Bash-Shell ausführen, führt Jenkins als eine app mit Symbol im Dock, wie im folgenden Screenshot gezeigt")](jenkins-walkthrough-images/image2.png#lightbox)

Starten oder Beenden von Jenkins ist so einfach wie das Starten oder Beenden von Jenkins.App.

Um Jenkins.App installieren zu können, laden Sie die neueste Version des Projekts auf der Downloadseite im folgenden Screenshot dargestellt:

[![](jenkins-walkthrough-images/image3.png "Herunterladen der neuesten Version der Projekte in diesem Screenshot dargestellte Seite Download-App")](jenkins-walkthrough-images/image3.png#lightbox)

Extrahieren Sie Zip-Datei in die `/Applications` Ordner auf Ihrem Build-Server, und starten Sie ihn genau wie jede andere OS X-Anwendung.
Beim ersten Sie Jenkins.App starten, wird er ein Dialogfeld, das Sie darüber informiert, dass sie Jenkins herunterlädt darstellen:

[![](jenkins-walkthrough-images/image4.png "App, es bietet ein Dialogfeld, das Sie darüber informiert, dass sie Jenkins herunterlädt")](jenkins-walkthrough-images/image4.png#lightbox)

Wenn Jenkins.App der Download abgeschlossen ist, wird es ein weiteres Dialogfeld gefragt werden, ob Sie den Jenkins-Start, anpassen wie im folgenden Screenshot dargestellt möchten angezeigt:

[![](jenkins-walkthrough-images/image5.png "App die ihren Download abgeschlossen ist, wird ein weiteres Dialogfeld gefragt werden, ob Sie den Jenkins-Start, anpassen wie in diesem Screenshot gezeigt möchten angezeigt")](jenkins-walkthrough-images/image5.png#lightbox)

Anpassen von Jenkins ist optional und muss nicht jedes Mal, wenn die app gestartet wird – die Standardeinstellungen ausgeführt werden, für die Jenkins für die meisten Situationen geeignet ist.

Wenn es zum Anpassen von Jenkins erforderlich ist, klicken Sie auf die **Standardeinstellungen ändern,** Schaltfläche. Diese bieten Ihnen zwei aufeinander folgenden Dialogfelder: eine, die Java-Befehlszeilenparameter abfragt und eine, die Jenkins-Befehlszeilenparameter abfragt. Die beiden folgenden Screenshots zeigen diese beiden Dialogfelder:

[![](jenkins-walkthrough-images/image6.png "Dieser Screenshot zeigt die Dialogfelder")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "Dieser Screenshot zeigt die Dialogfelder")](jenkins-walkthrough-images/image7.png#lightbox)

Sobald Jenkins ausgeführt wird, empfiehlt es sich als ein Element für die Anmeldung festgelegt, damit sie jedes Mal die Benutzeranmeldenamen in auf dem Computer gestartet wird. Sie hierzu mit der rechten Maustaste auf das Jenkins-Symbol im Dock und Auswahl **Optionen... > öffnen, bei der Anmeldung**, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image8.png "Sie können dazu mit der rechten Maustaste auf das Jenkins-Symbol im Dock und Auswählen von OptionsOpen bei der Anmeldung, wie im folgenden Screenshot gezeigt.")](jenkins-walkthrough-images/image8.png#lightbox)

Dies bewirkt, dass Jenkins.App jedes Mal automatisch starten der Benutzer anmeldet, aber nicht wenn der Computer hochgefahren. Es ist möglich, ein Benutzerkonto angeben, die OS X für die automatisch Anmeldung beim Systemstart verwenden. Öffnen der **Systemeinstellungen**, und wählen Sie die **Benutzer und Gruppen** Symbol wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image9.png "Öffnen Sie die Systemeinstellungen, und wählen Sie das Symbol \"Benutzergruppen\", wie im folgenden Screenshot gezeigt.")](jenkins-walkthrough-images/image9.png#lightbox)

Klicken Sie auf die **Anmeldeoptionen** Schaltfläche, und wählen Sie dann das Konto, OS X für die Anmeldung beim Systemstart verwenden wird.

An diesem Punkt wurde Jenkins installiert. Wenn wir die mobile Xamarin-Anwendungen erstellen möchten, müssen wir jedoch einige Plug-Ins zu installieren.

### <a name="installing-plugins"></a>Installieren von Plug-Ins

Wenn das Installationsprogramm Jenkins.App abgeschlossen ist, wird Jenkins starten und starten Sie den Webbrowser mit der URL http://localhost:8080, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image10.png "8080, wie im folgenden Screenshot gezeigt.")](jenkins-walkthrough-images/image10.png#lightbox)

Wählen Sie auf dieser Seite **Jenkins > Manage Jenkins > Manage Plugins** Menü in der oberen linken Ecke, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image11.png "Wählen Sie auf dieser Seite Jenkins verwalten Jenkins verwalten-Plug-Ins über das Menü in der oberen linken Ecke")](jenkins-walkthrough-images/image11.png#lightbox)

Dies zeigt die **Jenkins-Plug-In Manager** Seite. Wenn Sie auf der Registerkarte "verfügbar" klicken, sehen Sie eine Liste mit mehr als 600-Plug-Ins, die heruntergeladen und installiert werden können. Dies wird im folgenden Screenshot dargestellt:

[![](jenkins-walkthrough-images/image12.png "Wenn Sie auf der Registerkarte \"verfügbar\" klicken, sehen Sie eine Liste von mehr als 600-Plug-Ins, die heruntergeladen und installiert werden können")](jenkins-walkthrough-images/image12.png#lightbox)

Scrollen durch alle 600-Plug-Ins finden Sie ein paar können mühsam sein, dass und fehleranfällig. Jenkins stellt ein Filterfeld für die Suche in der oberen rechten Ecke der Schnittstelle bereit. Verwenden zum Suchen dieses Feld wird Product und installiert eine oder alle der folgenden Plug-Ins vereinfachen:

- **Jenkins-MSBuild-Plug-Ins** – dieses Plug-in ermöglicht das Erstellen von Visual Studio und Visual Studio für Mac-Lösungen (sln) und Projekte (.csproj).
- **Umgebung Injector-Plug-Ins** – Dies ist eine optionale, aber nützlich-Plug-Ins, die es ermöglicht, Festlegen von Umgebungsvariablen auf den Auftrag, und erstellen auf. Er bietet auch die zusätzlichen Schutz für Variablen, wie z. B. Kennwörter Code zum Signieren der Anwendung. Er wird manchmal als abgekürzt der *EnvInject Plugin* .
- **Team Foundation Server-Plug-Ins** – Dies ist eine optionale-Plug-Ins, die nur erforderlich, wenn Sie Team Foundation Server oder Team Foundation-Diensten für die quellcodeverwaltung verwenden.

Jenkins unterstützt Git ohne zusätzliche Plug-Ins.

Nach der Installation aller die Plug-Ins, sollten Sie zum Starten Sie Jenkins neu, und konfigurieren die globalen Einstellungen für jedes Plug-in. Die globalen Einstellungen für ein Plug-in finden Sie dazu **Jenkins > Manage Jenkins > System konfigurieren** aus der oberen linken Ecke, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image13.png "Die globalen Einstellungen für ein Plug-in finden Sie dazu die Jenkins / Manage Jenkins / System konfigurieren, in der oberen linken Ecke zu übergeben")](jenkins-walkthrough-images/image13.png#lightbox)

Wenn Sie mit dieser Option auswählen, werden Sie weitergeleitet die **System konfigurieren [Jenkins]** Seite. Diese Seite enthält Abschnitte, um Jenkins selbst konfigurieren und einige der Werte globale-Plug-in festzulegen.  Der folgende Screenshot zeigt ein Beispiel für diese Seite:

[![](jenkins-walkthrough-images/image14.png "Dieser Screenshot zeigt ein Beispiel für diese Seite")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>Konfigurieren die MSBuild-Plug-in

Die MSBuild-Plug-in muss so konfiguriert werden, dass verwenden **/Library/Frameworks/Mono.framework/Commands/xbuild** Visual Studio für Mac Projektmappen- und Projektdateien-Dateien zu kompilieren. Scrollen Sie nach unten der **System konfigurieren [Jenkins]** bis Seite der **hinzufügen MSBuild** Schaltfläche angezeigt wird, wie im folgenden Screenshot gezeigt:

 [![](jenkins-walkthrough-images/image15.png "Führen Sie einen Bildlauf nach unten Konfigurieren einer Jenkins-System aus, bis die Schaltfläche \"Hinzufügen von MSBuild\" angezeigt wird")](jenkins-walkthrough-images/image15.png#lightbox)

Klicken Sie auf diese Schaltfläche, und füllen Sie die **Namen** und **Pfad** zu **MSBuild** Felder auf dem Formular, das angezeigt wird. Der Name des Ihre **MSBuild** Installation muss einen aussagekräftigeren Namen, während der **Pfad zu MSBuild** muss der Pfad zur `xbuild`, dies ist in der Regel   **/Library/Frameworks / Mono.framework/Commands/xbuild**. Nachdem wir die Änderungen speichern, indem Sie auf das Speichern oder die Schaltfläche "anwenden" am unteren Rand der Seite, Jenkins kann verwenden `xbuild` Ihrer Lösungen zu kompilieren.

#### <a name="configuring-the-tfs-plugin"></a>Konfigurieren der TFS-Plug-in

Dieser Abschnitt ist erforderlich, wenn Sie TFS für die quellcodeverwaltung für Code verwenden möchten.

In der Reihenfolge für eine MacOS-Arbeitsstation für die Interaktion mit einem TFS-Server, [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) muss auf der Arbeitsstation installiert sein. Team Explorer Everywhere ist ein Satz von Tools von Microsoft, plattformübergreifenden Befehlszeilenclient für den Zugriff auf TFS enthält. Team Explorer Everywhere kann in drei Schritten von Microsoft heruntergeladen und installiert werden:

1. Entzippen Sie die Archivdatei in ein Verzeichnis, das dem Benutzerkonto zugegriffen werden kann. Sie können z. B. Entpacken Sie die Datei zu **~/tee**.
2. Konfigurieren Sie die Shell oder Ihren Systempfad zum Ordner einschließen, der die Dateien enthält, die im Schritt oben extrahiert wurden. Ein auf ein Objekt angewendeter

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Um zu bestätigen, dass Team Explorer Everywhere installiert ist, öffnen Sie eine Terminalsitzung, und führen Sie die `tf` Befehl. Wenn Sie Tf ordnungsgemäß konfiguriert ist, sehen Sie die folgende Ausgabe in der terminal-Sitzung:

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

Sobald der Client über die Befehlszeile für TFS installiert ist, muss Jenkins konfiguriert werden, durch den vollständigen Pfad zu der `tf` -Befehlszeilenclient. Scrollen Sie nach unten der **System konfigurieren [Jenkins]** Seite bis Sie die Team Foundation Server-Abschnitt finden, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image17.png "Scrollen Sie nach unten der Seite \"Konfigurieren einer Jenkins-System\", bis Sie die Team Foundation Server-Abschnitt gefunden")](jenkins-walkthrough-images/image17.png#lightbox)

Geben Sie den vollständigen Pfad zu der `tf` -Befehl aus, und klicken Sie auf die **speichern** Schaltfläche.

### <a name="configure-jenkins-security"></a>Konfigurieren der Sicherheit von Jenkins

Nach der Installation hat Jenkins Sicherheitsfunktionen deaktiviert sind, daher ist es möglich, dass allen Benutzern das Einrichten und jede Art von Auftrag anonym ausführen. In diesem Abschnitt wird beschrieben, wie die Sicherheitsfunktion mit der Jenkins-Benutzerdatenbank so konfigurieren Sie die Authentifizierung und Autorisierung konfiguriert werden.

Sicherheitseinstellungen finden Sie dazu **Jenkins > Jenkins verwalten > Globale Sicherheit konfigurieren**, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image18.png "Sicherheitseinstellungen finden Sie dazu die Jenkins / Manage Jenkins / Configure Global Security")](jenkins-walkthrough-images/image18.png#lightbox)

Auf der **Configure Global Security** Seite die **Sicherheit aktivieren** Kontrollkästchen und die **Zugriffssteuerung** Formular wird angezeigt, ähnlich wie im nächsten Screenshot:

[![](jenkins-walkthrough-images/image19.png "Überprüfen Sie die Sicherheit zu aktivieren, auf der Seite \"Configure Global Security\" das Kontrollkästchen und die Access Control-Form sollte angezeigt werden, ähnlich wie in diesem Screenshot")](jenkins-walkthrough-images/image19.png#lightbox)

Schalten Sie das Optionsfeld für **'Jenkins-Benutzerdatenbank** in der **Realm-Abschnitt "Sicherheit"**, und stellen sicher, dass **ermöglichen Benutzern, sich anzumelden** ebenfalls aktiviert ist, wie in der bildschirmabbildung von folgenden:

[![](jenkins-walkthrough-images/image20.png "Wechseln Sie das Optionsfeld für Jenkins eigenen Benutzerdatenbank im Abschnitt Bereich \"Sicherheit\", und stellen Sie sicher, dass Benutzern erlauben, registrieren Sie sich auch aktiviert ist")](jenkins-walkthrough-images/image20.png#lightbox)

Abschließend starten Sie Jenkins neu, und ein neues Konto erstellen. Das erste Konto, das erstellt wird, ist das Root-Konto, und dieses Konto wird automatisch an einen Administrator heraufgestuft werden. Navigieren Sie zurück zu den **Configure Global Security** Seite, und aktivieren die **Matrix basierende Sicherheit** Optionsfeld. Das Root-Konto Vollzugriff gewährt werden soll, und das anonyme Konto gegeben werden nur-Lese Zugriff, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image21.png "Das Root-Konto Vollzugriff gewährt werden soll, und das anonyme Konto gegeben werden nur-Lese Zugriff")](jenkins-walkthrough-images/image21.png#lightbox)

Nachdem diese Einstellungen gespeichert werden und Jenkins neu gestartet wird, wird Sicherheit aktiviert.

#### <a name="disabling-security"></a>Das Deaktivieren der Sicherheit

Im Falle eines vergessenen Kennworts oder die Sperre der gesamten Jenkins ist es möglich, die Sicherheit zu deaktivieren, indem Sie die folgenden Schritte:

1. Beenden Sie Jenkins. Wenn Sie Jenkins.app verwenden, können Sie dazu mit der rechten Maustaste auf das Jenkins.App-Symbol im Dock, und beenden auswählen, über das Menü, das wird angezeigt:

    ![App-Symbol im Dock und Quit auswählen, über das Menü, das wird angezeigt](jenkins-walkthrough-images/image19.png)

2. Öffnen Sie die Datei **~/.jenkins/config.xml** in einem Text-Editor.
3. Ändern Sie den Wert von der `<usesecurity></usesecurity>` Element `true` zu `false`.
4. Löschen der `<authorizationstrategy></authorizationstrategy>` und `<securityrealm></securityrealm>` Elemente aus der Datei.
5. Starten Sie Jenkins neu.

## <a name="setting-up-a-job"></a>Das Einrichten eines Auftrags

Jenkins auf der obersten Ebene, organisiert die verschiedenen Aufgaben erforderlich, um das Erstellen von Software in einer *Auftrag*. Ein Auftrag besitzt außerdem Metadaten zugeordnet, mit Informationen über den Build, z. B. zum Abrufen des Quellcodes, wie oft der Build ausgeführt werden soll, speziellen Variablen, die zum Erstellen von erforderlich sind und wie Entwickler zu benachrichtigen, wenn der Buildvorgang fehlschlägt.

Aufträge werden erstellt, indem Sie auswählen **Jenkins > neuer Auftrag** Menü in der oberen rechten Ecke, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image22.png "Aufträge werden erstellt, indem Sie neue Jenkins-Auftrag im Menü in der oberen rechten Ecke auswählen")

Dies zeigt die **neuer Auftrag [Jenkins]** Seite. Geben Sie einen Namen für den Auftrag, und wählen die **beim Erstellen eines Softwareprojekts free-Style** Optionsfeld. Der folgende Screenshot zeigt ein Beispiel hierfür:

![](jenkins-walkthrough-images/image23.png "Geben Sie einen Namen für den Auftrag, und wählen Sie den Build ein free-Style Software Project-Optionsfeld")

Klicken auf die **OK** Schaltfläche wird auf der Konfigurationsseite "für den Auftrag. Dies sollte den folgenden Screenshot entsprechen:

![](jenkins-walkthrough-images/image24.png "Dies sollte diesem Screenshot entsprechen.")

Jenkins organisiert Aufträge in einem Verzeichnis auf der Festplatte befindet sich unter folgendem Pfad: **~/.jenkins/jobs/[JOB NAME]**

Dieser Ordner enthält alle Dateien und Artefakte, die spezifisch für den Auftrag z. B. Protokolle, Konfigurationsdateien und der Quellcode, der kompiliert werden muss.

Nachdem der erste Auftrag erstellt wurde, muss es mit einem oder mehreren der folgenden konfiguriert werden:

- Das Quellcodeverwaltungssystem muss angegeben werden.
- Eine oder mehrere *Buildvorgänge* muss dem Projekt hinzugefügt werden. Dies sind die Schritte oder Tasks, die zum Erstellen der Anwendung erforderlich sind.
- Der Auftrag muss zugewiesen werden, eine *Buildtrigger* – eine Reihe von Anweisungen, wie oft Jenkins zum Abrufen des Codes und erstellen Sie das letzte Projekt informiert.

### <a name="configuring-source-code-control"></a>Konfigurieren der Quellcodeverwaltung

Die erste Aufgabe wie Jenkins ist den Quellcode aus dem Quellcodeverwaltungssystem abgerufen werden. Jenkins unterstützt viele der beliebten Source Code Management-Systeme sind bereits heute verfügbar. Dieser Abschnitt enthält zwei beliebte Systeme, Git und Team Foundation Server. Jeder dieser Source Code Management-Systeme wird in den folgenden Abschnitten ausführlicher erläutert.

#### <a name="using-git-for-source-code-control"></a>Mithilfe von Git für die Quellcodeverwaltung

Bei Verwendung von TFS für quellcodeverwaltung [überspringen](#Using-TFS-for-Source-Code-Management) diesen Abschnitt, und fahren Sie mit dem nächsten Abschnitt unter Verwendung von TFS.

Jenkins unterstützt Git von der – es sind keine zusätzlichen Plug-Ins erforderlich sind. Um Git verwenden, klicken Sie auf die **Git** Optionsfeld aus, und geben Sie die URL des Git-Repositorys wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image25.png "Um Git verwenden, klicken Sie auf das Optionsfeld \"Git\", und geben Sie die URL für das Git-repository")

Nachdem die Änderungen gespeichert wurden, ist die Git-Konfiguration abgeschlossen.

#### <a name="using-tfs-for-source-code-management"></a>Verwendung von TFS für Quellcodeverwaltung

Dieser Abschnitt gilt nur für TFS-Benutzer.

Klicken Sie auf die **Team Foundation Server** Optionsfeld und den TFS-Konfigurationsabschnitt sollte entsprechend angezeigt werden, was im folgenden Screenshot ist:

![](jenkins-walkthrough-images/image26.png "Klicken Sie auf das Optionsfeld für Team Foundation Server und der TFS-Konfigurationsabschnitt sollte angezeigt werden.")

Geben Sie die erforderlichen Informationen für TFS. Der folgende Screenshot zeigt ein Beispiel für das abgeschlossene Formular:

![](jenkins-walkthrough-images/image27.png "Dieser Screenshot zeigt ein Beispiel für das abgeschlossene Formular")

#### <a name="testing-the-source-code-control-configuration"></a>Testen die Konfiguration der Quellcodeverwaltung Code

Nachdem Sie die entsprechenden quellcodeverwaltung konfiguriert wurde, klicken Sie auf **speichern** um die Änderungen zu speichern. Dies gibt Sie zur Startseite für den Auftrag zurück, die im folgenden Screenshot ähnelt:

![](jenkins-walkthrough-images/image28.png "Dies gibt Sie zur Startseite für den Auftrag zurück, die diesem Screenshot entsprechen, werden")

Die einfachste Möglichkeit zum Überprüfen, ob die quellcodeverwaltung ordnungsgemäß konfiguriert ist, ist, einen Build manuell auszulösen, obwohl sind keine Buildaktionen angegeben. Um einen Build manuell zu starten, die auf der Startseite des Auftrags ist ein **Build Now** link im Menü auf der linken Seite, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image29.png "Um einen Build manuell zu starten, verfügt über die auf der Startseite des Auftrags einen Link jetzt erstellen Sie im Menü auf der linken Seite")

Wenn ein Build gestartet wurde, zeigt das Dialogfeld "Build History" eine blinkende blaue Kreis, eine Statusanzeige, die Build-Nummer und der Startzeit des Builds, ähnlich wie im folgenden Screenshot an:

![](jenkins-walkthrough-images/image30.png "Wenn ein Build gestartet wurde, zeigt das Dialogfeld \"Build History\" auf, eine blinkende blaue Kreis, eine Statusanzeige, die Nummer des Builds und die Zeit, die der Build gestartet")

Wenn der Auftrag erfolgreich ist, wird ein blauer Kreis angezeigt. Wenn der Auftrag fehlschlägt, wird ein roter Kreis angezeigt.

Damit können bei der Behandlung von Problemen, die als Teil des Builds auftreten können, werden die Jenkins aller die Ausgabe der Konsole für den Auftrag erfasst. Um die Konsolenausgabe anzuzeigen, klicken Sie auf den Auftrag auf **Buildverlauf**, und klicken Sie dann auf die **Konsolenausgabe** Link im linken Menü. Der folgende Screenshot zeigt die **Konsolenausgabe** Link als auch einen Teil der Ausgabe aus einem erfolgreichen Auftrag:

![](jenkins-walkthrough-images/image31.png "Dieser Screenshot zeigt die Konsolenausgabe Link als auch einen Teil der Ausgabe aus einem erfolgreichen Auftrag")

#### <a name="location-of-build-artifacts"></a>Speicherort des Build-Artefakte

Jenkins wird den gesamte Quellcode in einem speziellen Ordner mit dem Namen Abrufen einer *Arbeitsbereich*. Dieses Verzeichnis finden Sie im Ordner "" an folgender Stelle:

    ```
    ~/.jenkins/jobs/[JOB NAME]/workspace
    ```

Der Pfad zu der Arbeitsbereich wird in eine Umgebungsvariable namens gespeichert `$WORKSPACE`.

Es ist möglich, den Arbeitsbereichsordner in Jenkins zu durchsuchen, Navigieren auf der Angebotsseite für einen Auftrag, und klicken Sie dann auf die **Arbeitsbereich** Link im linken Menü. Der folgende Screenshot zeigt ein Beispiel für den Arbeitsbereich für einen Auftrag mit dem Namen **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Dieser Screenshot zeigt ein Beispiel für den Arbeitsbereich für einen Auftrag mit dem Namen \"HelloWorld\"")

### <a name="build-triggers"></a>Erstellen von Triggern

Es gibt mehrere verschiedene Strategien für die Initiierung von Builds in Jenkins – diese werden als bezeichnet *Buildtrigger*. Buildtrigger kann Jenkins entscheiden, wann einen Auftrag starten, und erstellen Sie das Projekt. Zwei der häufigsten Buildtrigger sind:

- **Erstellen Sie in regelmäßigen Abständen** : dieser Trigger bewirkt, dass Jenkins zum Starten eines Auftrags in angegebenen Intervallen, z. B. alle zwei Stunden oder an Wochentagen um Mitternacht. Der Build wird gestartet, unabhängig davon, ob es wurden Änderungen in der Quellcode-Repository.
- **Poll SCM** : dieser Trigger quellcodeverwaltung in regelmäßigen Abständen abgefragt wird. Wenn Änderungen an der Quellcode-Repository übertragen wurden, wird in Jenkins einen neuen Build gestartet.

SCM abrufen ist einen gängigen Trigger aus, da sie dann schnell rückmeldungen bereitstellt, wenn ein Entwickler Änderungen ein Commit, die dazu führen, das Build ausgeführt dass unterbrochen. Dies ist nützlich für Warnungen von Teams, dass einige kürzlich ein Commit ausgeführt Code Probleme verursacht, und kann die Entwickler das Problem zu beheben, während die Änderungen weiterhin neue Bedenken sind.

Regelmäßige Builds werden häufig verwendet, um eine Version der Anwendung, die Verteilung an Tester zu erstellen. Beispielsweise kann eine regelmäßige Builds für Freitag Abend geplant werden, damit Mitglieder des QA-Teams die Arbeit von der vorherigen Woche testen können.

### <a name="compiling-a-xamarinios-applications"></a>Kompilieren eine Xamarin.iOS-Anwendungen
Xamarin.iOS-Projekte kompiliert werden können, an der Befehlszeile mit `xbuild` oder `msbuild`. Der Shellbefehl wird im Kontext des Benutzerkontos ausgeführt, auf dem Jenkins ausgeführt wird. Es ist wichtig, dass das Benutzerkonto, das Zugriff auf das Bereitstellungsprofil verfügt, damit, dass die Anwendung ordnungsgemäß für die Verteilung verpackt werden kann. Es ist möglich, diese Shellbefehl auf der Konfigurationsseite des Auftrags hinzuzufügen.

Führen Sie einen Bildlauf nach unten, um die **erstellen** Abschnitt. Klicken Sie auf die **Buildschritt hinzufügen** Schaltfläche, und wählen **Execute Shell**, wie im folgenden Screenshot dargestellt:

![](jenkins-walkthrough-images/image33.png "Klicken Sie auf die Schaltfläche \"Build-Schritt hinzufügen\", und wählen Sie die Execute-shell")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Erstellen einer Xamarin.Android-Projekt

Erstellen einer Xamarin.Android-Projekt ist sehr ähnlich Konzept zum Erstellen einer Xamarin.iOS-Projekt. Um ein APK aus einer Xamarin.Android-Projekt zu erstellen, müssen Jenkins konfiguriert werden, um die folgenden zwei Schritte ausführen:

- Kompilieren Sie das Projekt mit der MSBuild-Plug-in
- Anmelden und Zip Ausrichten des APK mit einem Keystore gültige Version.

Diese beiden Schritte werden ausführlicher in den nächsten beiden Abschnitten behandelt werden.

### <a name="creating-the-apk"></a>Erstellen das APK

Klicken Sie auf die **Buildschritt hinzufügen** , und klicken **erstellen Sie ein Visual Studio-Projekt oder eine Projektmappe, die mithilfe von MSBuild**, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image36.png "Erstellen der APK klicken Sie auf die Schaltfläche \"Add Build Schritt\" und wählen ein Visual Studio-Projekt oder eine Projektmappe, die mithilfe von MSBuild")

Nachdem das Projekt der Buildschritt hinzugefügt wird, geben Sie die Formularfelder, die angezeigt werden. Im folgende Screenshot ist ein Beispiel für das abgeschlossene Formular:

![](jenkins-walkthrough-images/image37.png "Nachdem das Projekt der Buildschritt hinzugefügt wird, geben Sie die Felder, die angezeigt werden")

Dieser Buildschritt wird ausgeführt. `xbuild` in die **$WORKSPACE** Ordner. Die MSBuild-Build-Datei festgelegt ist, auf die **Xamarin.Android.csproj** Datei. Die **Befehlszeilenargumente** Geben Sie einen Releasebuild des Ziels **PackageForAndroid**. Das Produkt in diesem Schritt werden ein Android-Anwendungspaket, das am folgenden Speicherort:

    ```
    $WORKSPACE/[PROJECT NAME]/bin/Release
    ```

Der folgende Screenshot zeigt ein Beispiel für dieses APK:

![](jenkins-walkthrough-images/image38.png "Dieser Screenshot zeigt ein Beispiel für dieses APK")

Dieses APK ist nicht bereit für die Bereitstellung, da es nicht durch einen privaten Keystore signiert wurde und Zip ausgerichtet sein muss.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>Signierung und Zipalignen das APK für Version

Signierung und zipalignen das APK sind technisch zwei getrennten Aufgaben, die von zwei separaten Befehlszeilentools aus dem Android SDK ausgeführt werden. Allerdings ist es sinnvoll, die in einem Build-Aktion durchgeführt werden müssen. Weitere Informationen zum Anmelden und zipalignen ein APK finden Sie zur Vorbereitung einer Android-Anwendung für die Version Xamarins-Dokumentation.

Beide Befehle erfordern Befehlszeilenparameter, die von verschiedenen Projekten variieren können. Darüber hinaus sind einige dieser Parameter über die Befehlszeile Kennwörter, die nicht in der Konsolenausgabe angezeigt werden sollen, wenn der Build ausgeführt wird. Wir werden einige dieser Parameter über die Befehlszeile in Umgebungsvariablen gespeichert. Die Umgebungsvariablen für die Anmeldung und/oder Zip Ausrichten von erforderlich sind, werden in der folgenden Tabelle beschrieben:

|Umgebungsvariable|Beschreibung|
|--- |--- |
|KEYSTORE_FILE|Dies ist der Pfad zum Keystore ein. zum Signieren des APKS|
|KEYSTORE_ALIAS|Der Schlüssel im Schlüsselspeicher, der zum Signieren des APK verwendet wird.|
|INPUT_APK|Das APK erstellt wird, die `xbuild`.|
|SIGNED_APK|Das signierte APK erzeugten `jarsigner`.|
|FINAL_APK|Dies ist die ZIP-Datei ausgerichtet APK, das vom erzeugt wird `zipalign`.|
|STORE_PASS|Dies ist das Kennwort, das Zugriff auf den Inhalt des Keystores zum Signieren der das verwendet wird.|

Wie im Abschnitt mit Anforderungen beschrieben, können diese Umgebungsvariablen während des Buildvorgangs die EnvInject Plug-in festgelegt werden. Der Auftrag müssen einen neuen Build Schritt hinzugefügt werden basierend auf die Inject-Umgebungsvariablen, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image39.png "Der Auftrag müssen einen neuen Build Schritt hinzugefügt, basierend auf den Inject-Umgebungsvariablen")

In der **Eigenschaften Inhalten** bilden Feld, das angezeigt wird, Umgebung, die Variablen hinzugefügt werden, eine pro Zeile, in folgendem Format:

    ```
    ENVIRONMENT_VARIABLE_NAME = value
    ```

Der folgende Screenshot zeigt die Umgebungsvariablen, die zum Signieren des APKS erforderlich sind:

![](jenkins-walkthrough-images/image40.png "Dieser Screenshot zeigt die Umgebungsvariablen, die zum Signieren des APKS erforderlich sind")

Beachten Sie, dass einige der Umgebungsvariablen für die APK-Dateien erstellt werden, auf die `WORKSPACE` -Umgebungsvariablen angegeben.

Die endgültige Umgebungsvariable ist das Kennwort für den Zugriff des Keystores: `STORE_PASS`. Kennwörter sind vertrauliche Werte, die verdeckt oder in Protokolldateien ausgelassen werden sollen. Die EnvInject-Plug-in kann konfiguriert werden, um diese Werte zu schützen, damit sie in Protokollen nicht angezeigt werden.

Unmittelbar vor der **erstellen** Abschnitt der Auftragskonfiguration ist eine **Buildumgebung** Abschnitt. Wenn die **einfügen Kennwörter** Kontrollkästchen ein-/ausgeschaltet ist, eine Form Felder angezeigt werden. Diese Felder werden verwendet, um den Namen und Wert der Umgebungsvariablen zu erfassen. Im folgende Screenshot ist ein Beispiel zum Hinzufügen der `STORE_PASS` -Umgebungsvariablen angegeben:

![](jenkins-walkthrough-images/image41.png "In diesem Screenshot ist ein Beispiel für die Umgebungsvariable STOREPASS hinzufügen")

Nachdem Sie die Umgebungsvariablen initialisiert wurden, besteht der nächste Schritt, Hinzufügen eines Buildschritts für das Signieren und zip-Ausrichten des APKS. Unmittelbar nach der Buildschritt, legen Sie Umgebungsvariablen für die eine andere werden **Execute Shell** Befehl erstellen, die ausgeführt wird, `jarsigner` und `zipalign`. Jeder Befehl wird eine Zeile einnimmt, wie im folgenden Codeausschnitt gezeigt:

    ```
    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK
    ```

Der folgende Screenshot zeigt ein Beispiel zur Eingabe der `jarsigner` und `zipalign` Befehle in der Schritt:

![](jenkins-walkthrough-images/image42.png "Dieser Screenshot zeigt ein Beispiel dafür, wie die Jarsigner und Anwenden von Zipalign-Befehle in der Schritt eingeben")

Nach die Buildvorgängen vorhanden sind, ist es empfehlenswert, die zum Auslösen eines manuellen Builds aus, um sicherzustellen, dass alles funktioniert. Wenn der Build schlägt fehl, die **Konsolenausgabe** überprüft werden, Informationen zur Ursache zum Fehlschlagen des Buildvorgangs.

### <a name="submitting-tests-to-test-cloud"></a>Senden von Tests in Testcloud

Automatisierte Tests können in Test Cloud Shell-Befehle mit übermittelt werden. Weitere Informationen zum Einrichten eines Testlaufs in Xamarin Test Cloud finden Sie unter diesem Handbuch für die Verwendung von [Xamarin.UITest](/appcenter/test-cloud/preparing-for-upload/uitest/).

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wir einen Buildserver auf MacOS Jenkins eingeführt und konfiguriert, um zu kompilieren und das mobile Xamarin-Anwendungen für die Veröffentlichung vorbereiten. Wir können Jenkins auf einen MacOS-Computer sowie mehrere-Plug-Ins zur Unterstützung des Build-Prozesses installiert. Es erstellt und konfiguriert einen Auftrag, der Abrufen von Code aus TFS oder Git, und kompilieren Sie dann diesen Code in die Releaseversion einer Anwendung bereit. Außerdem haben wir untersucht, zwei verschiedene Möglichkeiten zum Planen, wann Aufträge ausgeführt werden soll.

## <a name="related-links"></a>Verwandte Links

- [Continuous Integration](~/tools/ci/index.md)
- [App Center Test (App Center-Test)](https://docs.microsoft.com/appcenter/test-cloud/)
