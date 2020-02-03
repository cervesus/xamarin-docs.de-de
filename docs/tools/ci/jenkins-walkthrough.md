---
title: Verwenden von Jenkins mit Xamarin
description: In diesem Dokument wird beschrieben, wie Jenkins für Continuous Integration mit xamarin-Anwendungen verwendet wird. Es wird erläutert, wie Jenkins installiert, konfiguriert und verwendet wird.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 4f91e683b826657a9740de7e0b98137858130042
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725377"
---
# <a name="using-jenkins-with-xamarin"></a>Verwenden von Jenkins mit Xamarin

_In dieser Anleitung wird veranschaulicht, wie Sie Jenkins als Continuous Integration Server einrichten und die Kompilierung mobiler Anwendungen automatisieren, die mit xamarin erstellt wurden. Es wird beschrieben, wie Jenkins unter OS X installiert, konfiguriert und Aufträge für die Kompilierung von xamarin. IOS-und xamarin. Android-Anwendungen eingerichtet werden, wenn Änderungen an das Quell Code Verwaltungssystem übertragen werden._

Die [Einführung in Continuous Integration mit xamarin](~/tools/ci/intro-to-ci.md) führt Continuous Integration als nützliche Software Entwicklungspraxis ein, die eine frühe Warnung bei fehlerlichem oder inkompatiblem Code bereitstellt. Mit CI können Entwickler Probleme und Probleme beheben, wenn Sie auftreten, und die Software in einem geeigneten Zustand für die Bereitstellung behalten. In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie den Inhalt aus beiden Dokumenten gemeinsam verwenden.

In dieser Anleitung erfahren Sie, wie Sie Jenkins auf einem dedizierten Computer mit OS X installieren und so konfigurieren, dass er beim Starten des Computers automatisch ausgeführt wird. Nachdem Jenkins installiert wurde, werden zusätzliche Plug-ins installiert, um MS Build zu unterstützen. Jenkins unterstützt git standardmäßig. Wenn TFS für die Quell Code Verwaltung verwendet wird, müssen auch ein zusätzliches Plug-in und Befehlszeilen-Hilfsprogramm installiert werden.

Nachdem Jenkins konfiguriert wurde und alle erforderlichen Plug-ins installiert wurden, erstellen wir einen oder mehrere Aufträge, um die xamarin. Android-und xamarin. IOS-Projekte zu kompilieren. Ein Auftrag ist eine Sammlung von Schritten und Metadaten, die erforderlich sind, um einige Aufgaben auszuführen. Ein Auftrag besteht in der Regel aus folgendem:

- **Quell Code Verwaltung (Source Code Management, SCM)** – Dies ist ein Metadateneintrag in den Jenkins-Konfigurationsdateien, der Informationen darüber enthält, wie eine Verbindung mit der Quell Code Verwaltung hergestellt und welche Dateien abgerufen werden sollen.
- **Trigger** – Trigger werden verwendet, um einen Auftrag auf der Grundlage bestimmter Aktionen zu starten, z. b. Wenn ein Entwickler Änderungen an dem Quellcoderepository committet.
- **Buildanweisungen** – hierbei handelt es sich um ein Plug-in oder ein Skript, mit dem der Quellcode kompiliert und eine Binärdatei erstellt wird, die auf mobilen Geräten installiert werden kann.
- **Optionale Buildaktionen** – Dies kann das Ausführen von Komponententests, das Ausführen statischer Analysen des Codes, das Signieren von Code oder das Starten eines anderen Auftrags zum Ausführen anderer buildbezogener Aufgaben umfassen.
- **Benachrichtigungen** – ein Auftrag sendet möglicherweise eine Benachrichtigung über den Status eines Builds.
- **Sicherheit** – Obwohl optional, wird dringend empfohlen, die Jenkins-Sicherheitsfeatures ebenfalls zu aktivieren.

In diesem Handbuch wird erläutert, wie Sie einen Jenkins-Server einrichten, der die einzelnen Punkte abdeckt. Am Ende dieses Projekts sollten Sie sich mit dem Einrichten und Konfigurieren von Jenkins vertraut machen, um IPA und APK für unsere mobilen xamarin-Projekte zu erstellen.

## <a name="requirements"></a>Requirements (Anforderungen)

Der ideale Buildserver ist ein eigenständiger Computer, der ausschließlich für das Erstellen und möglicherweise das Testen der Anwendung vorgesehen ist. Ein dedizierter Computer stellt sicher, dass Artefakte, die möglicherweise für andere Rollen (z. b. die eines Webservers) erforderlich sind, den Build nicht verunreinigen. Wenn der Buildserver z. b. auch als Webserver fungiert, benötigt der Webserver möglicherweise eine widersprüchliche Version einer gemeinsamen Bibliothek. Aufgrund dieses Konflikts funktioniert der Webserver möglicherweise nicht ordnungsgemäß, oder Jenkins erstellt Builds, die bei der Bereitstellung für Benutzer nicht funktionieren.

Der Buildserver für Mobile xamarin-Apps ist so eingerichtet, wie die Arbeitsstation eines Entwicklers. Es verfügt über ein Benutzerkonto, in dem Jenkins, Visual Studio für Mac und xamarin. IOS und xamarin. Android installiert werden. Alle Code Signatur Zertifikate, Bereitstellungs Profile und Schlüsselspeicher müssen ebenfalls installiert werden. *In der Regel ist das Benutzerkonto des Buildservers getrennt von ihren Entwickler Konten. Achten Sie darauf, dass Sie alle Software, Schlüssel und Zertifikate installieren und konfigurieren, während Sie mit dem Benutzerkonto für den Buildserver angemeldet sind.*

Im folgenden Diagramm werden alle diese Elemente auf einem typischen Jenkins-Buildserver veranschaulicht:

[![](jenkins-walkthrough-images/image1.png "This diagram illustrates all of these elements on a typical Jenkins build server")](jenkins-walkthrough-images/image1.png#lightbox)

IOS-Anwendungen können nur auf einem Computer mit macOS erstellt und signiert werden. Eine Mac-Mini Anwendung ist eine sinnvolle kostengünstigere Option, aber jeder Computer, der OS X 10,10 (Yosemite) oder höher ausführen kann, ist ausreichend.

Wenn TFS für die Quell Code Verwaltung verwendet wird, müssen Sie [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)installieren. Team Explorer Everywhere bietet plattformübergreifenden Zugriff auf TFS über das Terminal in macOS.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Installieren von Jenkins

Die erste Aufgabe bei der Verwendung von Jenkins besteht darin, Sie zu installieren. Es gibt drei Möglichkeiten, Jenkins unter OS X auszuführen:

- Als Daemon, der im Hintergrund ausgeführt wird.
- In einem Servlet-Container, z. b. Tomcat, Jetty oder JBoss.
- Als normaler Prozess, der unter einem Benutzerkonto ausgeführt wird.

Die meisten herkömmlichen Continuous Integration Anwendungen werden im Hintergrund ausgeführt, entweder als Daemon (unter OS X oder \*nix) oder als Dienst (unter Windows). Dies eignet sich für Szenarien, in denen keine GUI-Interaktion erforderlich ist und die Einrichtung der Buildumgebung problemlos durchgeführt werden kann. Mobile Apps erfordern außerdem Keystores und Signatur Zertifikate, für die der Zugriff möglicherweise problematisch ist, wenn Jenkins als Daemon ausgeführt wird. Aus diesen Gründen konzentriert sich dieses Dokument auf das dritte Szenario – das Ausführen von Jenkins unter einem Benutzerkonto auf dem Buildserver.

Jenkins. app ist eine praktische Möglichkeit zum Installieren von Jenkins. Dies ist ein AppleScript-Wrapper, der das Starten und Beenden eines Jenkins-Servers vereinfacht. Anstatt in einer bash-Shell ausgeführt zu werden, wird Jenkins als APP mit dem Symbol im Dock ausgeführt, wie im folgenden Screenshot zu sehen:

[![](jenkins-walkthrough-images/image2.png "Instead of running in a bash shell, Jenkins runs as an app with icon in the Dock, as shown in this screenshot")](jenkins-walkthrough-images/image2.png#lightbox)

Das Starten oder Beenden von Jenkins ist so einfach wie das Starten oder Beenden von Jenkins. app.

Zum Installieren von Jenkins. app laden Sie die neueste Version von der Downloadseite des Projekts herunter, die im folgenden Screenshot dargestellt wird:

[![](jenkins-walkthrough-images/image3.png "App, download the latest version from the projects download page, pictured in this screenshot")](jenkins-walkthrough-images/image3.png#lightbox)

Extrahieren Sie die ZIP-Datei in den Ordner "`/Applications`" auf dem Buildserver, und starten Sie Sie wie jede andere OS X-Anwendung.
Wenn Sie Jenkins. app zum ersten Mal starten, wird ein Dialogfeld angezeigt, in dem Sie darüber informiert werden, dass Jenkins heruntergeladen wird:

[![](jenkins-walkthrough-images/image4.png "App, it will present a dialog informing you that it will download Jenkins")](jenkins-walkthrough-images/image4.png#lightbox)

Nachdem Jenkins. app den Download abgeschlossen hat, wird ein weiteres Dialogfeld angezeigt, in dem Sie gefragt werden, ob Sie den Jenkins-Start anpassen möchten, wie im folgenden Screenshot zu sehen:

[![](jenkins-walkthrough-images/image5.png "App has finished its download, it will display another dialog asking you if you would like to customize the Jenkins startup, as seen in this screenshot")](jenkins-walkthrough-images/image5.png#lightbox)

Das Anpassen von Jenkins ist optional und muss nicht jedes Mal ausgeführt werden, wenn die APP gestartet wird – die Standardeinstellungen für Jenkins funktionieren in den meisten Situationen.

Wenn Jenkins angepasst werden muss, klicken Sie auf die Schaltfläche **Standard ändern** . Dadurch werden zwei aufeinander folgende Dialogfelder angezeigt: eine, die Java-Befehlszeilenparameter anfordert, und eine weitere, die Jenkins-Befehlszeilenparameter anfordert. In den folgenden zwei Screenshots werden diese beiden Dialogfelder angezeigt:

[![](jenkins-walkthrough-images/image6.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image7.png#lightbox)

Nachdem Jenkins ausgeführt wurde, können Sie es als Anmelde Element festlegen, damit es jedes Mal gestartet wird, wenn sich der Benutzer beim Computer anmeldet. Klicken Sie hierzu mit der rechten Maustaste auf das Jenkins-Symbol im Dock, und wählen Sie **Optionen... > Bei der Anmeldung geöffnet**, wie im folgenden Screenshot zu sehen:

[![](jenkins-walkthrough-images/image8.png "You can do this by right-clicking on the Jenkins icon in the Dock and choosing OptionsOpen at Login, as shown in this screenshot")](jenkins-walkthrough-images/image8.png#lightbox)

Dies bewirkt, dass Jenkins. app automatisch gestartet wird, wenn sich der Benutzer anmeldet, jedoch nicht, wenn der Computer startet. Es ist möglich, ein Benutzerkonto anzugeben, das OS X zum automatischen Anmelden bei bei der Startzeit verwendet. Öffnen Sie die **System Einstellungen**, und wählen Sie das Symbol **Benutzer & Gruppen** aus, wie in diesem Screenshot gezeigt:

[![](jenkins-walkthrough-images/image9.png "Open the System Preferences, and select the User  Groups icon as shown in this screenshot")](jenkins-walkthrough-images/image9.png#lightbox)

Klicken Sie auf die Schaltfläche **Anmelde Optionen** , und wählen Sie dann das Konto aus, das OS X zum Anmelden bei der Startzeit verwendet.

An diesem Punkt wurde Jenkins installiert. Wenn wir jedoch Mobile xamarin-Anwendungen erstellen möchten, müssen wir einige Plug-ins installieren.

### <a name="installing-plugins"></a>Plug-ins installieren

Wenn das Installationsprogramm Jenkins. app abgeschlossen ist, startet es Jenkins und startet den Webbrowser mit der URL- http://localhost:8080, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image10.png "8080, as shown in this screenshot")](jenkins-walkthrough-images/image10.png#lightbox)

Wählen Sie auf dieser Seite im Menü in der oberen linken Ecke **Jenkins > Manage Jenkins > Manage Plugins** aus, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image11.png "From this page, select Jenkins  Manage Jenkins  Manage Plugins from the menu in the upper left hand corner")](jenkins-walkthrough-images/image11.png#lightbox)

Dadurch wird die Seite **Jenkins-Plug** -in-Manager angezeigt. Wenn Sie auf die Registerkarte "verfügbar" klicken, wird eine Liste mit mehr als 600 Plug-Ins angezeigt, die heruntergeladen und installiert werden können. Dies wird im folgenden Screenshot dargestellt:

[![](jenkins-walkthrough-images/image12.png "If you click on the Available tab, you will see a list of over 600 plugins that can be downloaded and installed")](jenkins-walkthrough-images/image12.png#lightbox)

Das Scrollen durch alle 600-Plug-ins, um einige zu finden, kann mühsam und fehleranfällig sein. Jenkins stellt ein Filter Suchfeld in der oberen rechten Ecke der-Schnittstelle bereit. Durch die Verwendung dieses Filter Felds für die Suche wird das Suchen und Installieren eines oder aller der folgenden Plug-ins vereinfacht:

- **Jenkins MSBuild-Plug** -in – dieses Plug-in ermöglicht das Erstellen von Visual Studio-und Visual Studio für Mac-Projektmappen (. sln) und-Projekten (. csproj).
- **Umgebungs-injektorplug** -in – dies ist ein optionales, aber nützliches Plug-in, das es ermöglicht, Umgebungsvariablen auf Auftrags-und Buildebene festzulegen. Außerdem bietet Sie zusätzlichen Schutz für Variablen, wie z. b. die Kenn Wörter, die zum Codieren der Anwendung verwendet werden Sie wird manchmal als das- *Plug* -in-Plug-in abgekürzt.
- **Team Foundation Server-Plug** -in – dies ist ein optionales Plug-in, das nur erforderlich ist, wenn Sie Team Foundation Server oder Team Foundation Services für die Quell Code Verwaltung verwenden.

Jenkins unterstützt git ohne zusätzliche Plug-ins.

Nachdem Sie alle Plug-ins installiert haben, möchten Sie Jenkins neu starten und die globalen Einstellungen für jedes Plug-in konfigurieren. Sie finden die globalen Einstellungen für ein Plug-in, indem Sie **Jenkins > Manage Jenkins > Configure System** in der oberen linken Ecke auswählen, wie im folgenden Screenshot gezeigt:

[![](jenkins-walkthrough-images/image13.png "The global settings for a plugin can be found by selecting Jenkins / Manage Jenkins / Configure System from the upper left hand corner")](jenkins-walkthrough-images/image13.png#lightbox)

Wenn Sie diese Menüoption auswählen, werden Sie auf die Seite **System [Jenkins] konfigurieren** gelangen. Diese Seite enthält Abschnitte zum Konfigurieren von Jenkins und zum Festlegen einiger der globalen Plug-in-Werte.  Der folgende Screenshot zeigt ein Beispiel für diese Seite:

[![](jenkins-walkthrough-images/image14.png "This screenshot illustrates an example of this page")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>Konfigurieren des MSBuild-Plug-ins

Das MSBuild-Plug-in muss für die Verwendung von **/Library/Frameworks/Mono.Framework/Commands/xbuild** zum Kompilieren Visual Studio für Mac Projektmappen-und Projektdateien konfiguriert werden Scrollen Sie nach unten auf der Seite **Configure System [Jenkins]** , bis die Schaltfläche **MSBuild hinzufügen** angezeigt wird, wie im folgenden Screenshot zu sehen:

 [![](jenkins-walkthrough-images/image15.png "Scroll down the Configure System Jenkins page until the Add MSBuild button appears")](jenkins-walkthrough-images/image15.png#lightbox)

Klicken Sie auf diese Schaltfläche, und geben Sie im angezeigten Formular den **Namen** und den **Pfad** zu den **MSBuild** -Feldern ein. Der Name der **MSBuild** -Installation sollte etwas aussagekräftig sein, während der **Pfad zu MSBuild** der Pfad zu `xbuild`sein sollte, d. h. in der Regel **/Library/Frameworks/Mono.Framework/Commands/xbuild**. Nachdem Sie die Änderungen gespeichert haben, indem Sie unten auf der Seite auf die Schaltfläche "Speichern" oder "anwenden" klicken, können Sie mit `xbuild` Ihre Projektmappen kompilieren.

#### <a name="configuring-the-tfs-plugin"></a>Konfigurieren des TFS-Plug-ins

Dieser Abschnitt ist obligatorisch, wenn Sie TFS für die Quell Code Verwaltung verwenden möchten.

Damit eine macOS-Arbeitsstation mit einem TFS-Server interagiert, müssen [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) auf der Arbeitsstation installiert sein. Team Explorer Everywhere ist ein Satz von Tools von Microsoft, der einen plattformübergreifenden Befehlszeilen Client für den Zugriff auf TFS umfasst. Team Explorer Everywhere können von Microsoft heruntergeladen und in drei Schritten installiert werden:

1. Entpacken Sie die Archivdatei in ein Verzeichnis, auf das das Benutzerkonto zugreifen kann. Beispielsweise können Sie die Datei in **~/Tee**entzippen.
2. Konfigurieren Sie die Shell oder den Systempfad so, dass Sie den Ordner mit den Dateien enthält, die in der obigen Schritt 1 entzippt wurden. Beispiel:

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Öffnen Sie eine Terminalsitzung, und führen Sie den `tf`-Befehl aus, um zu bestätigen, dass Team Explorer Everywhere installiert ist. Wenn TF ordnungsgemäß konfiguriert ist, wird die folgende Ausgabe in der Terminalsitzung angezeigt:

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

Nachdem der Befehlszeilen Client für TFS installiert wurde, muss Jenkins mit dem vollständigen Pfad zum `tf` Befehlszeilen Client konfiguriert werden. Führen Sie einen Bildlauf nach unten auf der Seite **Configure System [Jenkins]** aus, bis Sie den Abschnitt Team Foundation Server finden, wie im folgenden Screenshot zu sehen:

[![](jenkins-walkthrough-images/image17.png "Scroll down the Configure System Jenkins page until you find the Team Foundation Server section")](jenkins-walkthrough-images/image17.png#lightbox)

Geben Sie den vollständigen Pfad zum `tf`-Befehl ein, und klicken Sie auf die Schaltfläche **Speichern** .

### <a name="configure-jenkins-security"></a>Konfigurieren der Jenkins-Sicherheit

Bei der Erstinstallation von Jenkins ist die Sicherheit deaktiviert, sodass jeder Benutzer jede Art von Auftrag anonym einrichten und ausführen kann. In diesem Abschnitt wird erläutert, wie die Sicherheit mithilfe der Jenkins-Benutzerdatenbank zum Konfigurieren der Authentifizierung und Autorisierung konfiguriert wird.

Sicherheitseinstellungen finden Sie durch Auswählen von **Jenkins > Manage Jenkins > Konfigurieren globaler Sicherheit**, wie in diesem Screenshot gezeigt:

[![](jenkins-walkthrough-images/image18.png "Security settings can be found by selecting Jenkins / Manage Jenkins / Configure Global Security")](jenkins-walkthrough-images/image18.png#lightbox)

Aktivieren Sie auf der Seite **globale Sicherheit konfigurieren** das Kontrollkästchen **Sicherheit aktivieren** , und das **Access Control** Formular sollte ähnlich wie im folgenden Screenshot aussehen:

[![](jenkins-walkthrough-images/image19.png "On the Configure Global Security page, check the Enable Security checkbox and the Access Control form should appear, similar to this screenshot")](jenkins-walkthrough-images/image19.png#lightbox)

Schalten Sie das Optionsfeld für die **eigene Benutzerdatenbank von Jenkins** im **Bereich Sicherheitsbereich**ein, und stellen **Sie** sicher, dass die Option Benutzern die Registrierung gestatten ebenfalls aktiviert ist, wie im folgenden Screenshot veranschaulicht:

[![](jenkins-walkthrough-images/image20.png "Toggle the radio button for Jenkins own user database in the Security Realm Section, and ensure that Allow users to sign up is also checked")](jenkins-walkthrough-images/image20.png#lightbox)

Starten Sie schließlich Jenkins neu, und erstellen Sie ein neues Konto. Das erste Konto, das erstellt wird, ist das Root-Konto, und dieses Konto wird automatisch zu einem Administrator herauf gestuft. Navigieren Sie zurück zur Seite **globale Sicherheit konfigurieren** , und aktivieren Sie das Optionsfeld **Matrix basierte Sicherheit** . Dem Stammkonto sollte Vollzugriff gewährt werden, und dem anonymen Konto sollte Schreib geschützter Zugriff erteilt werden, wie im folgenden Screenshot zu sehen:

[![](jenkins-walkthrough-images/image21.png "The root account should be granted full access, and the anonymous account should be given read-only access")](jenkins-walkthrough-images/image21.png#lightbox)

Nachdem diese Einstellungen gespeichert und Jenkins neu gestartet wurde, wird die Sicherheit aktiviert.

#### <a name="disabling-security"></a>Deaktivieren der Sicherheit

Im Fall eines vergessenen Kennworts oder einer Jenkins-weiten Sperrung ist es möglich, die Sicherheit zu deaktivieren, indem Sie die folgenden Schritte ausführen:

1. Beendet Jenkins. Wenn Sie Jenkins. app verwenden, klicken Sie mit der rechten Maustaste auf das Jenkins. app-Symbol im Dock, und wählen Sie im angezeigten Menü beenden aus:

    ![App-Symbol im Dock, und wählen Sie im angezeigten Menü die Option beenden aus.](jenkins-walkthrough-images/image19.png)

2. Öffnen Sie die Datei **~/.Jenkins/config.XML** in einem Text-Editor.
3. Ändern Sie den Wert des `<usesecurity></usesecurity>`-Elements von `true` in `false`.
4. Löschen Sie die `<authorizationstrategy></authorizationstrategy>` und die `<securityrealm></securityrealm>` Elemente aus der Datei.
5. Starten Sie Jenkins neu.

## <a name="setting-up-a-job"></a>Einrichten eines Auftrags

Auf der obersten Ebene organisiert Jenkins alle verschiedenen Aufgaben, die erforderlich sind, um Software in einem *Auftrag*zu erstellen. Einem Auftrag sind auch Metadaten zugeordnet, der Informationen über den Build bereitstellt, z. b. wie der Quellcode, die Häufigkeit, mit der der Build ausgeführt werden soll, alle speziellen Variablen, die für die Erstellung erforderlich sind, und wie Entwickler benachrichtigt werden, wenn der Build fehlschlägt.

Aufträge werden erstellt, indem im Menü in der oberen rechten Ecke **Jenkins > neuer Auftrag** ausgewählt wird, wie im folgenden Screenshot zu sehen:

![](jenkins-walkthrough-images/image22.png "Jobs are created by selecting Jenkins  New Job from the menu in the upper right hand corner")

Dadurch wird die Seite **Neuer Auftrag [Jenkins]** angezeigt. Geben Sie einen Namen für den Auftrag ein, und wählen Sie das Optionsfeld **Kostenloses Softwareprojekt erstellen** aus. Der folgende Screenshot zeigt ein Beispiel für Folgendes:

![](jenkins-walkthrough-images/image23.png "Enter a name for the job, and select the Build a free-style software project radio button")

Wenn Sie auf die Schaltfläche **OK** klicken, wird die Konfigurationsseite für den Auftrag angezeigt. Dies sollte in etwa wie im folgenden Screenshot aussehen:

![](jenkins-walkthrough-images/image24.png "This should resemble this screenshot")

Jenkins organisiert Aufträge in einem Verzeichnis auf der Festplatte unter folgendem Pfad: **~/.Jenkins/Jobs/[Auftrags Name]**

Dieser Ordner enthält alle Dateien und Artefakte, die für den Auftrag spezifisch sind, z. b. Protokolle, Konfigurationsdateien und den Quellcode, der kompiliert werden muss.

Nachdem der anfängliche Auftrag erstellt wurde, muss er mit einem oder mehreren der folgenden Schritte konfiguriert werden:

- Das Quell Code Verwaltungssystem muss angegeben werden.
- Mindestens eine *Buildaktion* muss dem Projekt hinzugefügt werden. Dies sind die Schritte oder Aufgaben, die zum Erstellen der Anwendung erforderlich sind.
- Dem Auftrag muss ein *buildauslösers* zugewiesen werden – ein Satz von Anweisungen, der Jenkins mitteilt, wie oft der Code abgerufen und das endgültige Projekt erstellt wird.

### <a name="configuring-source-code-control"></a>Konfigurieren der Quell Code Verwaltung

Der erste Task Jenkins Ruft den Quellcode aus dem Quell Code Verwaltungssystem ab. Jenkins unterstützt viele beliebte Quell Code Verwaltungssysteme, die heute verfügbar sind. In diesem Abschnitt werden zwei gängige Systeme behandelt: git und Team Foundation Server. Jedes dieser Quell Code Verwaltungssysteme wird in den folgenden Abschnitten ausführlicher erläutert.

#### <a name="using-git-for-source-code-control"></a>Verwenden von git für die Quell Code Verwaltung

Wenn Sie TFS für die Quell Code Verwaltung verwenden, über [springen](#using-tfs-for-source-code-management) Sie diesen Abschnitt, und fahren Sie mit dem nächsten Abschnitt mithilfe von TFS fort.

Jenkins unterstützt git standardmäßig – es sind keine zusätzlichen Plug-Ins erforderlich. Wenn Sie git verwenden möchten, klicken Sie auf das Optionsfeld " **git** ", und geben Sie die URL für das git-Repository ein, wie im folgenden Screenshot zu sehen:

![](jenkins-walkthrough-images/image25.png "To use Git, click on the Git radio button and enter the URL for the Git repository")

Nachdem die Änderungen gespeichert wurden, ist die git-Konfiguration fertiggestellt.

#### <a name="using-tfs-for-source-code-management"></a>Verwenden von TFS für die Quell Code Verwaltung

Dieser Abschnitt gilt nur für TFS-Benutzer.

Klicken Sie auf das Optionsfeld **Team Foundation Server** , und der TFS-Konfigurations Abschnitt sollte ähnlich wie im folgenden Screenshot angezeigt werden:

![](jenkins-walkthrough-images/image26.png "Click on the Team Foundation Server radio button and the TFS configuration section should appear")

Geben Sie die erforderlichen Informationen für TFS an. Der folgende Screenshot zeigt ein Beispiel für das abgeschlossene Formular:

![](jenkins-walkthrough-images/image27.png "This screenshot shows an example of the completed form")

#### <a name="testing-the-source-code-control-configuration"></a>Testen der Konfiguration der Quell Code Verwaltung

Nachdem die entsprechende Quell Code Verwaltung konfiguriert wurde, klicken Sie auf **Speichern** , um die Änderungen zu speichern. Dadurch gelangen Sie zur Startseite für den Auftrag, die dem folgenden Screenshot ähnelt:

![](jenkins-walkthrough-images/image28.png "This will return you to the home page for the job, which will resemble this screenshot")

Die einfachste Möglichkeit, um zu überprüfen, ob die Quell Code Verwaltung ordnungsgemäß konfiguriert ist, besteht darin, einen Build manuell zu initiieren, obwohl keine Buildaktionen angegeben sind. Um einen Build manuell zu starten, hat die Startseite des Auftrags im Menü auf der linken Seite einen Link " **Build Now" (Build now** ), wie im folgenden Screenshot zu sehen:

![](jenkins-walkthrough-images/image29.png "To start a build manually, the home page of the job has a Build Now link in the menu on the left hand side")

Wenn ein Build gestartet wurde, wird im Dialogfeld "buildverlauf" ein blinkender blauer Kreis, eine Statusanzeige, die Buildnummer und die Uhrzeit, zu der der Build gestartet wurde, ähnlich wie im folgenden Screenshot angezeigt:

![](jenkins-walkthrough-images/image30.png "When a build has been started, the Build History dialog displays a flashing blue circle, a progress bar, the build number and the time that the build started")

Wenn der Auftrag erfolgreich ist, wird ein blauer Kreis angezeigt. Wenn der Auftrag fehlschlägt, wird ein roter Kreis angezeigt.

Zur Unterstützung bei der Behebung von Problemen, die möglicherweise als Teil des Builds auftreten, erfasst Jenkins alle Konsolen Ausgaben für den Auftrag. Klicken Sie zum Anzeigen der Konsolenausgabe in **buildverlauf**auf den Auftrag, und klicken Sie dann im linken Menü auf den Link **Konsolenausgabe** . Der folgende Screenshot zeigt den **Konsolenausgabe** Link sowie einen Teil der Ausgabe eines erfolgreichen Auftrags:

![](jenkins-walkthrough-images/image31.png "This screenshot shows the Console Output link, as well as some of the output from a successful job")

#### <a name="location-of-build-artifacts"></a>Speicherort der Build-Artefakte

Jenkins Ruft den gesamten Quellcode in einen speziellen Ordner mit dem Namen " *Workspace*" ab. Dieses Verzeichnis befindet sich im Ordner am folgenden Speicherort:

```
~/.jenkins/jobs/[JOB NAME]/workspace
```

Der Pfad zum Arbeitsbereich wird in einer Umgebungsvariablen mit dem Namen `$WORKSPACE`gespeichert.

Es ist möglich, den Arbeitsbereichs Ordner in Jenkins zu durchsuchen, indem Sie zur Landing Page für einen Auftrag navigieren und dann im linken Menü auf den Link für den **Arbeitsbereich** klicken. Der folgende Screenshot zeigt ein Beispiel für den Arbeitsbereich für einen Auftrag mit dem Namen " **HelloWorld**":

![](jenkins-walkthrough-images/image32.png "This screenshot shows an example of the workspace for a job named HelloWorld")

### <a name="build-triggers"></a>Buildtrigger

Es gibt mehrere verschiedene Strategien zum Initiieren von Builds in Jenkins – diese werden als *Buildtrigger*bezeichnet. Ein buildauslösers unterstützt Jenkins bei der Entscheidung, wann ein Auftrag gestartet und das Projekt erstellt wird. Zwei der gängigeren Buildtrigger sind:

- **Regelmäßiges Erstellen** – dieser Vorgang bewirkt, dass Jenkins einen Auftrag in angegebenen Intervallen startet, z. b. alle zwei Stunden oder Mitternacht an Wochentagen. Der Build wird unabhängig davon gestartet, ob Änderungen im Quellcoderepository vorgenommen wurden.
- Abruf **SCM** – mit diesem Triggern wird die Quell Code Verwaltung regelmäßig abgerufen. Wenn Änderungen an das Quellcoderepository übertragen wurden, startet Jenkins einen neuen Build.

Der Abruf SCM ist ein beliebter-Triggertyp, da er ein schnelles Feedback liefert, wenn ein Entwickler Änderungen durchführt, die den Build beeinträchtigen. Dies ist nützlich für Warnungs Teams, dass einige vor kurzem committem Code Probleme verursachen, und die Entwickler können das Problem beheben, während die Änderungen noch immer im Hinterkopf sind.

Periodische Builds werden häufig verwendet, um eine Version der Anwendung zu erstellen, die an Tester verteilt werden kann. Beispielsweise kann ein periodischer Build für Freitagabend geplant werden, damit Mitglieder des QA-Teams die Arbeit der vorangegangenen Woche testen können.

### <a name="compiling-a-xamarinios-applications"></a>Kompilieren von xamarin. IOS-Anwendungen
Xamarin. IOS-Projekte können mithilfe von `xbuild` oder `msbuild`in der Befehlszeile kompiliert werden. Der Shellbefehl wird im Kontext des Benutzerkontos ausgeführt, auf dem Jenkins ausgeführt wird. Es ist wichtig, dass das Benutzerkonto Zugriff auf das Bereitstellungs Profil hat, damit die Anwendung ordnungsgemäß für die Verteilung gepackt werden kann. Es ist möglich, diesen Shellbefehl der Auftrags Konfigurationsseite hinzuzufügen.

Scrollen Sie nach unten zum Abschnitt **Build** . Klicken Sie auf die Schaltfläche **Buildschritt hinzufügen** , und wählen Sie **Shell ausführen**aus, wie im folgenden Screenshot veranschaulicht:

![](jenkins-walkthrough-images/image33.png "Click the Add build step button and select Execute shell")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Entwickeln eines xamarin. Android-Projekts

Das Entwickeln eines xamarin. Android-Projekts ähnelt dem Konzept des Aufbaus eines xamarin. IOS-Projekts. Um ein APK aus einem xamarin. Android-Projekt zu erstellen, muss Jenkins so konfiguriert werden, dass die folgenden beiden Schritte ausgeführt werden:

- Kompilieren des Projekts mithilfe des MSBuild-Plug-ins
- Sign und ZIP richten das APK mit einem gültigen releasekeystore aus.

Diese beiden Schritte werden in den nächsten beiden Abschnitten ausführlicher behandelt.

### <a name="creating-the-apk"></a>Erstellen des APK

Klicken Sie auf die Schaltfläche **Buildschritt hinzufügen** , und wählen Sie **ein Visual Studio-Projekt oder eine Projekt Mappe mit MSBuild erstellen**aus, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image36.png "Creating the APK  Click on the Add build step button, and select Build a Visual Studio project or solution using MSBuild")

Nachdem der Buildschritt dem Projekt hinzugefügt wurde, füllen Sie die Formularfelder aus, die angezeigt werden. Der folgende Screenshot zeigt ein Beispiel für das abgeschlossene Formular:

![](jenkins-walkthrough-images/image37.png "Once the build step is added to the project, fill in the form fields that appear")

Mit diesem Buildschritt wird `xbuild` im **$Workspace** Ordner ausgeführt. Die MSBuild-Builddatei wird auf die **xamarin. Android. csproj** -Datei festgelegt. Mit den **Befehlszeilen Argumenten** wird ein Releasebuild des Ziels **packageforandroid**angegeben. Bei diesem Schritt handelt es sich um ein APK an folgendem Speicherort:

```
$WORKSPACE/[PROJECT NAME]/bin/Release
```

Der folgende Screenshot zeigt ein Beispiel für dieses APK:

![](jenkins-walkthrough-images/image38.png "This screenshot shows an example of this APK")

Dieses APK ist nicht bereit für die Bereitstellung, da es nicht mit einem privaten Keystore signiert wurde und in der ZIP-Datei ausgerichtet werden muss.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>Signieren und zipalign des APK für die Freigabe

Das Signieren und zipalign des APK sind technisch zwei separate Aufgaben, die von zwei separaten Befehlszeilen Tools aus der Android SDK ausgeführt werden. Es ist jedoch praktisch, Sie in einer Buildaktion auszuführen. Weitere Informationen zum Signieren und zipalign eines APK finden Sie in der xamarin-Dokumentation zum Vorbereiten einer Android-Anwendung für die Veröffentlichung.

Für beide Befehle sind Befehlszeilenparameter erforderlich, die von Projekt zu Projekt abweichen können. Außerdem handelt es sich bei einigen dieser Befehlszeilenparameter um Kenn Wörter, die nicht in der Konsolenausgabe angezeigt werden sollen, wenn der Build ausgeführt wird. Einige dieser Befehlszeilenparameter werden in Umgebungsvariablen gespeichert. In der folgenden Tabelle werden die Umgebungsvariablen beschrieben, die für das Signieren und/oder die ZIP-Ausrichtung erforderlich sind:

|Umgebungsvariable|BESCHREIBUNG|
|--- |--- |
|KEYSTORE_FILE|Dies ist der Pfad zum Keystore zum Signieren des APK.|
|KEYSTORE_ALIAS|Der Schlüssel im Keystore, der zum Signieren des APK verwendet wird.|
|INPUT_APK|Das APK, das von `xbuild`erstellt wird.|
|SIGNED_APK|Das signierte APK, das von `jarsigner`erstellt wurde.|
|FINAL_APK|Dabei handelt es sich um das per ZIP ausgerichtete APK, das von `zipalign`erstellt wird.|
|STORE_PASS|Dies ist das Kennwort, das für den Zugriff auf den Inhalt des Keystores zum Abonnieren der Datei verwendet wird.|

Wie im Abschnitt "Anforderungen" beschrieben, können diese Umgebungsvariablen während des Builds mit dem envinject-Plug-in festgelegt werden. Für den Auftrag sollte basierend auf den Umgebungsvariablen einfügen ein neuer Buildschritt hinzugefügt werden, wie im folgenden Screenshot zu sehen:

![](jenkins-walkthrough-images/image39.png "The job should have a new build step added based on the Inject environment variables")

Im Feld **Eigenschaften Inhalts** Formular werden Umgebungsvariablen (eine pro Zeile) im folgenden Format hinzugefügt:

```
ENVIRONMENT_VARIABLE_NAME = value
```

Der folgende Screenshot zeigt die Umgebungsvariablen, die zum Signieren des APK erforderlich sind:

![](jenkins-walkthrough-images/image40.png "This screenshot shows the environment variables that are required for signing the APK")

Beachten Sie, dass einige Umgebungsvariablen für die APK-Dateien auf der `WORKSPACE`-Umgebungsvariablen erstellt werden.

Die letzte Umgebungsvariable ist das Kennwort für den Zugriff auf den Inhalt des Keystores: `STORE_PASS`. Kenn Wörter sind sensible Werte, die in Protokolldateien verdeckt oder ausgelassen werden sollten. Das Plug-in-Plug-in kann so konfiguriert werden, dass diese Werte so geschützt werden, dass Sie nicht in Protokollen angezeigt werden.

Unmittelbar vor dem Abschnitt **Build** der Auftrags Konfiguration ist ein Abschnitt für die **Buildumgebung** . Wenn das Kontrollkästchen Kenn **Wörter einfügen** ein-/ausgeschaltet wird, werden einige Formularfelder angezeigt. Diese Formularfelder werden verwendet, um den Namen und den Wert der Umgebungsvariablen aufzuzeichnen. Der folgende Screenshot zeigt ein Beispiel für das Hinzufügen der `STORE_PASS`-Umgebungsvariablen:

![](jenkins-walkthrough-images/image41.png "This screenshot is an example of adding the STOREPASS environment variable")

Nachdem die Umgebungsvariablen initialisiert wurden, besteht der nächste Schritt im Hinzufügen eines Buildschritts für das Signieren und ZIP-Ausrichten des APK. Unmittelbar nach dem Buildschritt zum Einfügen der Umgebungsvariablen ist ein weiterer **Execute Shell** Command-Build, der `jarsigner` und `zipalign`ausführt. Jeder Befehl wird eine Zeile in Anspruch nehmen, wie im folgenden Code Ausschnitt gezeigt:

```
jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
zipalign -f -v 4 $SIGNED_APK $FINAL_APK
```

Der folgende Screenshot zeigt ein Beispiel für die Eingabe der `jarsigner` und `zipalign` Befehle in den folgenden Schritt:

![](jenkins-walkthrough-images/image42.png "This screenshot shows an example of how to enter the jarsigner and zipalign commands into the step")

Nachdem alle Buildaktionen durchgeführt wurden, empfiehlt es sich, einen manuellen Build zu initiieren, um zu überprüfen, ob alles funktioniert. Wenn der Buildvorgang fehlschlägt, sollte die **Konsolenausgabe** überprüft werden, um zu überprüfen, was zu einem Fehler beim Build geführt hat.

### <a name="submitting-tests-to-test-cloud"></a>Senden von Tests an Test Cloud

Automatisierte Tests können mithilfe von Shellbefehlen an Test Cloud übermittelt werden. Weitere Informationen zum Einrichten eines Testlaufs in Xamarin Test Cloud finden Sie unter [Vorbereiten von xamarin. Android-Apps](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) und Vorbereiten von [xamarin. IOS-apps](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch haben wir Jenkins als Buildserver unter macOS eingeführt und so konfiguriert, dass Mobile xamarin-Anwendungen für die Veröffentlichung kompiliert und vorbereitet werden. Wir haben Jenkins auf einem macOS-Computer installiert und mehrere Plug-Ins zur Unterstützung des Buildprozesses erstellt. Wir haben einen Auftrag erstellt und konfiguriert, der Code aus TFS oder git per Pull abruft und diesen Code dann in eine Release Ready-Anwendung kompiliert. Wir haben auch zwei verschiedene Möglichkeiten zum Planen der Ausführung von Aufträgen untersucht.

## <a name="related-links"></a>Verwandte Links

- [Continuous Integration](~/tools/ci/index.md)
- [App Center Test (App Center-Test)](https://docs.microsoft.com/appcenter/test-cloud/)
