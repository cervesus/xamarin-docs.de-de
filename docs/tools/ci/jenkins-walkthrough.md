---
title: Verwendung von Jenkins mit Xamarin
description: "Diese Anleitung wird veranschaulicht, wie Jenkins als fortlaufende Integration-Server einrichten und automatisieren Kompilieren von mobilen Anwendungen mit Xamarin erstellt wurden. Es wird beschrieben, wie Jenkins unter OS X installieren, um ihn zu konfigurieren, richten Sie Aufträge Xamarin.iOS und Xamarin.Android Anwendungen zu kompilieren, wenn Änderungen an das Verwaltungssystem von Source Code übergeben werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: eb1602a96b304919fe563d1bb9ea0a15722e436b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="using-jenkins-with-xamarin"></a>Verwendung von Jenkins mit Xamarin

_Diese Anleitung wird veranschaulicht, wie Jenkins als fortlaufende Integration-Server einrichten und automatisieren Kompilieren von mobilen Anwendungen mit Xamarin erstellt wurden. Es wird beschrieben, wie Jenkins unter OS X installieren, um ihn zu konfigurieren, richten Sie Aufträge Xamarin.iOS und Xamarin.Android Anwendungen zu kompilieren, wenn Änderungen an das Verwaltungssystem von Source Code übergeben werden._

[Einführung in die fortlaufende Integration mit Xamarin](~/tools/ci/intro-to-ci.md) führt fortlaufenden Integration als ein nützlich Softwareentwicklungsverfahren, die und frühzeitige Warnungen der Code unterbrochen oder nicht kompatibel liefert. CI ermöglicht Entwicklern wird auf Punkte eingegangen und Probleme, sobald sie auftreten, und behält die Software in einen geeigneten Zustand für die Bereitstellung. In dieser exemplarischen Vorgehensweise beschrieben, wie den Inhalt von beide Dokumente zusammen verwenden.

Diese Anleitung zeigt, wie Jenkins auf einem dedizierten Computer mit OS X installieren und konfigurieren Sie ihn, die automatisch ausgeführt, wenn der Computer wird gestartet. Nach Jenkins installiert ist, wird es zur Unterstützung von MS Build zusätzliche Plug-Ins installiert werden. Jenkins unterstützt Git ausgegeben. Wenn TFS für die quellcodeverwaltung verwendet wird, müssen auch eine zusätzliche-Plug-Ins und der Befehlszeilen-Hilfsprogramme installiert sein.

Sobald Jenkins konfiguriert ist, und alle erforderlichen-Plug-Ins installiert wurde, erstellen wir einen oder mehrere Aufträge, um die Projekte Xamarin.Android und Xamarin.iOS zu kompilieren. Ein Auftrag ist eine Auflistung von Schritten und Metadaten, die erforderlich sind, um einige Aufgaben ausführen. Ein Auftrag umfasst i. d. r. Folgendes:

-  **Source Code Management (SCM)** – Dies ist eine Meta-Data-Eintrag in den Konfigurationsdateien Jenkins, Informationen zum Herstellen einer Verbindung mit der quellcodeverwaltung und welche Dateien enthält abgerufen.
-  **Trigger** – Trigger dienen zum Starten eines Auftrags basierend auf bestimmte Aktionen, z. B. wenn ein Entwickler Änderungen auf das Quellcoderepository ausgeführt.
-  **Erstellen von Anweisungen** – Dies ist ein Plug-in oder ein Skript, das den Quellcode kompilieren und erstellen eine Binärdatei, die auf mobilen Geräten installiert werden kann.
-  **Optionale erstellen Aktionen** – Dies können z. B. Ausführen von Komponententests, die statische Analyse auf das Codesignaturzertifikat Code ausgeführt, oder Starten von einem anderen Auftrag zum Ausführen anderer verwandten Arbeitsaufgaben erstellen.
-  **Benachrichtigungen** – ein Auftrag kann eine Art der Benachrichtigung über den Status eines Builds versenden.
-  **Sicherheit** – zwar optional, es wird dringend empfohlen, dass die Sicherheitsfunktionen Jenkins ebenfalls aktiviert werden.


Zum Einrichten eines Jenkins-Servers für jede dieser Punkte wird dieses Handbuch erläutert. Durch das Ende des Zertifikats sollten wir haben ein durchgehendes Verständnis der Vorgehensweise Einrichten und Konfigurieren von Jenkins um IPA- und des APK für unsere Xamarin-mobile-Projekte zu erstellen.

# <a name="requirements"></a>Anforderungen

Die ideale Build-Server ist ein eigenständiger Computer zu erstellen und testen die Anwendung möglicherweise der einzige Zweck reserviert. Ein dedizierter Computer wird sichergestellt, dass Elemente, die für andere Rollen (z. B. die von einem Webserver) erforderlich sind, nicht den Build verfälschen. Wenn der Build-Server auch als Web-Server fungiert, kann beispielsweise der Webserver eine in Konflikt stehende Version einige allgemeine Bibliothek erforderlich ist. Aufgrund dieses Konflikts des Servers möglicherweise nicht ordnungsgemäß, oder Jenkins möglicherweise erstellen Sie Builds, die nicht funktionsfähig sind, wenn für Benutzer bereitgestellt.

Xamarin für mobile apps für den Build-Server ist sehr ähnlich wie ein Entwickler Arbeitsstation einrichten. Verfügt über ein Benutzerkonto in Visual Studio für Mac, welche Jenkins und Xamarin.iOS und Xamarin.Android installiert werden soll. Alle Zertifikate, Profile und Schlüsselspeicher Bereitstellung Signieren von Code muss ebenfalls installiert werden. *In der Regel dem Build-Server-Benutzerkonto ist unabhängig von Ihrer entwicklerkonten - Achten Sie darauf, dass Sie zum Installieren und Konfigurieren der Software, Schlüssel und Zertifikate, während mit dem Build-Server-Benutzer-Konto angemeldet.*

Das folgende Diagramm zeigt alle diese Elemente in einem typischen Jenkins-Build-Server:

 [ ![](jenkins-walkthrough-images/image1.png "Dieses Diagramm zeigt alle diese Elemente in einem typischen Jenkins-Build-server")](jenkins-walkthrough-images/image1.png)

iOS-Anwendungen können nur erstellt und auf einem Computer mit Mac OS X signiert werden. Ein Mac Mini ist eine sinnvolle Option mit niedrigeren Kosten, aber einem beliebigen Computer ausführen OS X 10.10 (Yosemite) oder höher ist ausreichend.

Wenn TFS für die quellcodeverwaltung verwendet wird, sollten Sie zum Installieren [Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785), von Microsoft verfügbar. Team Explorer Everywhere bietet plattformübergreifende-Zugriff auf TFS auf dem Terminalserver unter OS X.

[!include[](~/tools/ci/includes/firewall-information.md)]

# <a name="installing-jenkins"></a>Installieren von Jenkins

Die erste Aufgabe, die mit der Jenkins ist, es zu installieren. Es gibt drei Möglichkeiten zum Ausführen von Jenkins auf OS X:

-  Als Daemon, im Hintergrund ausgeführt.
-  In einem Servletcontainer, z. B. Tomcat, Jetty oder JBoss.
-  Als normale Prozess unter einem Benutzerkonto ausgeführt wird.


Die meisten herkömmliche continuous integrationsanwendungen im Hintergrund ausgeführt werden, entweder als Daemon (unter OS X oder \*Nix) oder als Dienst (unter Windows). Dies ist für Szenarien, in denen es keine GUI-Interaktion erforderlich ist, und das Setup von der Buildumgebung problemlos ausgeführt werden kann. Außerdem benötigen die mobilen apps Schlüsselspeichern und Signieren von Zertifikaten, die für den Zugriff auf problematisch sein können, wenn Jenkins als Daemon ausgeführt wird. Wegen dieser Sicherheitsrisiken konzentriert sich dieses Dokument auf das dritte Szenario – Jenkins unter einem Benutzerkonto auf dem Buildserver ausgeführt.

Jenkins.App ist eine praktische Möglichkeit zum Jenkins zu installieren. Dies ist ein AppleScript-Wrapper, die die vereinfacht das Starten und Beenden eines Jenkins-Servers. Statt in der Bash-Shell ausgeführt wird Jenkins als eine app mit dem Symbol im andocken, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image2.png "Statt in der Bash-Shell ausgeführt wird Jenkins als app mit dem Symbol im andocken, wie in diesem Screenshot gezeigt")](jenkins-walkthrough-images/image2.png)

Starten oder Beenden von Jenkins ist so einfach wie das Starten oder Beenden von Jenkins.App.

Um Jenkins.App zu installieren, laden Sie die neueste Version des Projekts auf der Downloadseite auf im Screenshot unten abgebildet:

 [ ![](jenkins-walkthrough-images/image3.png "Die neueste Version der Projekte Downloadseite, in diesem Screenshot abgebildet Download-App")](jenkins-walkthrough-images/image3.png)

Extrahieren der Zip-Datei in die `/Applications` Ordner auf Ihrem Build-Server, und starten Sie es ebenso wie jede andere OS X-Anwendung.
Jenkins.App, Sie starten zum ersten Mal bietet es ein Dialogfeld darüber informiert, dass es Jenkins heruntergeladen wird:

 [ ![](jenkins-walkthrough-images/image4.png "App, er bietet ein Dialogfeld darüber informiert, dass es Jenkins herunterlädt")](jenkins-walkthrough-images/image4.png)

Sobald Jenkins.App ihren Download beendet wurde, zeigt er ein weiteres Dialogfeld gefragt werden, ob Sie den Start Jenkins anpassen wie im folgenden Screenshot dargestellt möchten:

 [ ![](jenkins-walkthrough-images/image5.png "App ihren Download abgeschlossen ist, wird ein weiteres Dialogfeld gefragt werden, ob Sie den Start Jenkins anpassen wie in diesem Screenshot möchten angezeigt")](jenkins-walkthrough-images/image5.png)

Anpassen von Jenkins ist optional und muss nicht jedes Mal, wenn die app gestartet wird – die Standardeinstellungen ausgeführt werden, damit Jenkins für die meisten Situationen verwendet werden.

Wenn es zum Anpassen von Jenkins erforderlich ist, klicken Sie auf die **Standardeinstellungen ändern** Schaltfläche. Dies zeigt Ihnen, mit zwei aufeinander folgenden Dialogfelder: eine, die Java-Befehlszeilenparameter anfordert, und ein anderes, das Jenkins-Befehlszeilenparameter anfordert. Die folgenden zwei Screenshots zeigen diese beiden Dialogfelder:

 [ ![](jenkins-walkthrough-images/image6.png "Diese bildschirmabbildung zeigt die Dialogfelder")](jenkins-walkthrough-images/image6.png)

 [ ![](jenkins-walkthrough-images/image7.png "Diese bildschirmabbildung zeigt die Dialogfelder")](jenkins-walkthrough-images/image7.png)

Sobald Jenkins ausgeführt wird, empfiehlt es sich, sie als ein Element für die Anmeldung festzulegen, damit sie jedes Mal die Benutzeranmeldenamen in auf dem Computer gestartet wird. Hierzu können Sie indem Sie mit der rechten Maustaste auf das Symbol "Jenkins" im Dock und **Optionen... Öffnen Sie bei der Anmeldung**, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image8.png "Sie können dazu mit der rechten Maustaste auf das Symbol "Jenkins" im Dock und Auswählen von OptionsOpen bei der Anmeldung, wie in diesem Screenshot dargestellt")](jenkins-walkthrough-images/image8.png)

Dies führt dazu, dass Jenkins.App automatisch bei jedem Starten der Benutzer anmeldet, aber nicht bei der Computer hochgefahren. Es ist möglich, ein Benutzerkonto angeben, die OS X zum automatisch anmelden beim Systemstart verwendet. Öffnen der **Systemeinstellungen**, und wählen Sie die **Benutzer & Gruppen** Symbol wie in diesem Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image9.png "Öffnen Sie die Systemeinstellungen, und wählen Sie das Symbol "Benutzergruppen" aus, wie in diesem Screenshot dargestellt.")](jenkins-walkthrough-images/image9.png)

Klicken Sie auf die **Anmeldeoptionen** aus, und klicken Sie dann das Konto, OS X für die Anmeldung beim Systemstart verwendet.

An diesem Punkt hat Jenkins installiert wurde. Wenn wir Xamarin mobile Anwendungen erstellen möchten, müssen wir einige-Plug-Ins installieren.


## <a name="installing-plugins"></a>Installieren von Plug-Ins

Nach Abschluss der Jenkins.App Installationsprogramm er startet Jenkins und starten den Webbrowser, mit der URL http://localhost: 8080, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image10.png "8080, wie in diesem Screenshot dargestellt.")](jenkins-walkthrough-images/image10.png)

Wählen Sie auf dieser Seite **Jenkins > Jenkins verwalten > Verwalten von Plug-Ins** über das Menü in der oberen linken Ecke, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image11.png "Wählen Sie auf dieser Seite Jenkins verwalten Jenkins Verwalten von Plug-Ins über das Menü in der oberen linken Ecke")](jenkins-walkthrough-images/image11.png)

Dies zeigt die **Jenkins Plug-In-Manager** Seite. Wenn Sie auf der Registerkarte "verfügbar" klicken, sehen Sie eine Übersicht über 600-Plug-Ins, die heruntergeladen und installiert werden kann. Dies wird im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image12.png "Wenn Sie auf der Registerkarte "verfügbar" klicken, sehen eine Liste von mehr als 600-Plug-Ins Sie, die heruntergeladen und installiert werden kann")](jenkins-walkthrough-images/image12.png)

Durchführen eines Bildlaufs durch alle 600-Plug-Ins ermitteln, ein paar zeitraubend sein können und fehleranfällig. Jenkins bietet ein Suchfeld Filter in der oberen rechten Ecke der Schnittstelle. Verwenden dieses Feld Filter zum Suchen wird suchen und installiert eine oder alle der folgenden Erweiterungen vereinfacht:

-  **Jenkins MSBuild-Plug-Ins** – dieses Plug-in ermöglicht das Erstellen von Visual Studio und Visual Studio für Mac-Lösungen (sln) und Projekte (csproj).
-  **Umgebung Injector-Plug-Ins** – Dies ist eine optionale, aber nützlich-Plug-Ins, die es ermöglicht, Festlegen von Umgebungsvariablen auf den Auftrag, und erstellen Sie die Ebene. Es bietet außerdem zusätzlichen Schutz für Variablen, wie z. B. Kennwörter Code zum Signieren der Anwendung. Es ist manchmal abgekürzt als die *EnvInject Plugin* .
-  **Team Foundation Server-Plug-Ins** – Dies ist ein optionaler Plug-in, die nur erforderlich, wenn Sie Team Foundation Server oder Team Foundation-Diensten für die quellcodeverwaltung verwenden.

Jenkins unterstützt Git ohne zusätzliche Plug-Ins.

Möchten Sie nach der Installation die Plug-Ins alle Jenkins starten und konfigurieren Sie die globalen Einstellungen für jedes Plug-in. Die globalen Einstellungen für ein Plug-in finden Sie dazu **Jenkins > Jenkins verwalten > System konfigurieren** aus der oberen linken Ecke, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image13.png "Die globalen Einstellungen für ein Plug-in verwendbaren dazu Jenkins / verwalten Jenkins / übergeben, System konfigurieren, von der oberen linken Ecke")](jenkins-walkthrough-images/image13.png)

Bei Auswahl dieser Option Sie gelangen auf die **System konfigurieren [Jenkins]** Seite. Diese Seite enthält Abschnitte Jenkins selbst konfigurieren und einige der globalen-Plug-Ins Werte festlegen.  Der folgende Screenshot zeigt ein Beispiel für diese Seite:

 [ ![](jenkins-walkthrough-images/image14.png "Diese Abbildung zeigt ein Beispiel dieser Seite")](jenkins-walkthrough-images/image14.png)


### <a name="configuring-the-msbuild-plugin"></a>Konfigurieren die MSBuild-Plug-in

Die MSBuild-Plug-in muss so konfiguriert werden, dass verwenden **/Library/Frameworks/Mono.framework/Commands/xbuild** zum Kompilieren von Visual Studio für Mac Projektmappen- und Projektdateien-Dateien. Führen Sie einen Bildlauf nach unten der **System konfigurieren [Jenkins]** bis Seite der **MSBuild hinzufügen** Schaltfläche angezeigt wird, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image15.png "Nach unten der Seite "System Jenkins konfigurieren", bis die Schaltfläche "MSBuild hinzufügen" angezeigt wird")](jenkins-walkthrough-images/image15.png)

Klicken Sie auf diese Schaltfläche, und füllen Sie die **Namen** und **Pfad** auf **MSBuild** Felder des Formulars, das angezeigt wird. Der Name des Ihrer **MSBuild** Installation muss einen aussagekräftigeren Namen, während die **Pfad zum MSBuild** muss der Pfad zur `xbuild`, dem es sich gewöhnlich   **/Library/Frameworks / Mono.framework/Commands/xbuild**. Nachdem wir die Änderungen speichern, indem Sie auf die speichern "oder" am unteren Rand der Seite die Schaltfläche "anwenden", Jenkins zeilenfilterausdruck verwenden `xbuild` Ihre Lösungen zu kompilieren.

### <a name="configuring-the-tfs-plugin"></a>Konfigurieren der TFS-Plug-in

Dieser Abschnitt ist obligatorisch, wenn Sie TFS für die quellcodeverwaltung verwenden möchten.

In der Reihenfolge für eine OS X-Arbeitsstation für die Interaktion mit einem TFS-Server muss das Team Explorer Everywhere auf der Arbeitsstation installiert sein. Team Explorer Everywhere ist eine Reihe von Tools von Microsoft, plattformübergreifenden Befehlszeilenclient für den Zugriff auf TFS enthält. Team Explorer Everywhere können von Microsoft heruntergeladen und installiert werden in drei Schritten:

1. Entpacken Sie die Archivdatei, zu einem Verzeichnis, das dem Benutzerkonto zugegriffen werden kann. Sie können z. B. Entzippen Sie die Datei auf **~/tee**.
2. Konfigurieren Sie den Pfad-Shell oder das System, um den Ordner, der die Dateien enthält, die in Schritt 1 oben extrahiert wurden. Ein auf ein Objekt angewendeter

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Um zu bestätigen, dass Team Explorer Everywhere installiert ist, öffnen Sie eine terminal-Sitzung, und führen die `tf` Befehl. Wenn Sie Tf ordnungsgemäß konfiguriert ist, sehen Sie die folgende Ausgabe in der Terminaldienste-Sitzung:

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

Sobald der Client über die Befehlszeile für TFS installiert ist, muss Jenkins konfiguriert werden, durch den vollständigen Pfad zu der `tf` Befehlszeilenclient. Führen Sie einen Bildlauf nach unten der **System konfigurieren [Jenkins]** Seite bis Sie im Team Foundation Server-Abschnitt finden, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image17.png "Bildlauf zum Konfigurieren von System Jenkins bis Sie im Abschnitt für Team Foundation Server finden")](jenkins-walkthrough-images/image17.png)

Geben Sie den vollständigen Pfad zu der `tf` Befehl, und klicken Sie auf die **speichern** Schaltfläche.

## <a name="configure-jenkins-security"></a>Konfigurieren von Jenkins-Sicherheit

Beim ersten Mal installiert, hat Jenkins Sicherheitsfunktionen deaktiviert sind, daher ist es möglich, dass jeder Benutzer einrichten und jede Art von Auftrag anonym ausführen. In diesem Abschnitt wird beschrieben, wie die Sicherheitsfunktion mit der Jenkins-Benutzerdatenbank so konfigurieren Sie die Authentifizierung und Autorisierung konfiguriert.

Sicherheitseinstellungen finden Sie dazu **Jenkins > Jenkins verwalten > Konfigurieren der globalen Sicherheit**, wie in diesem Screenshot dargestellt:

 [ ![](jenkins-walkthrough-images/image18.png "Sicherheitseinstellungen finden Sie dazu Jenkins / verwalten Jenkins / globalen Sicherheit konfigurieren")](jenkins-walkthrough-images/image18.png)

Auf der **globalen Sicherheit konfigurieren** Seite Überprüfung der **Sicherheit aktivieren** Kontrollkästchen und **Access Control** Formular sollte angezeigt werden, der nächste Screenshot ähnelt:

 [ ![](jenkins-walkthrough-images/image19.png "Überprüfen Sie auf der Seite globalen Sicherheit Konfigurieren der Sicherheit aktivieren das Kontrollkästchen und das Access Control-Formular sollte angezeigt werden, ähnlich wie in diesem Screenshot")](jenkins-walkthrough-images/image19.png)

Schalten Sie das Optionsfeld für **'Jenkins-Benutzerdatenbank** in der **Realm-Abschnitt "Sicherheit"**, und sicherstellen, dass **Benutzerberechtigungen zum Registrieren** wird überprüft, wie im veranschaulicht die bildschirmabbildung von folgenden:

 [ ![](jenkins-walkthrough-images/image20.png "Schalten Sie das Optionsfeld für Jenkins eigenen Benutzerdatenbank im Abschnitt Bereich "Sicherheit", und stellen Sie sicher, dass Benutzern erlauben, registrieren Sie sich auch aktiviert ist")](jenkins-walkthrough-images/image20.png)

Schließlich starten Sie Jenkins und erstellen Sie ein neues Konto. Das erste Konto, das erstellt wird, ist das Root-Konto, und dieses Konto wird automatisch an einen Administrator heraufgestuft werden. Navigieren Sie zurück zu den **globalen Sicherheit konfigurieren** Seite, und überprüfen Sie deaktivieren die **Matrix basierende Sicherheit** Optionsfeld. Der Root-Konto sollte Vollzugriff gewährt werden, und das anonyme Konto sollten nur-Lese Zugriff erhält, wie im folgenden Screenshot gezeigt:

 [ ![](jenkins-walkthrough-images/image21.png "Der Root-Konto Vollzugriff gewährt werden sollten und das anonyme Konto sollte nur-Lese-Zugriff zugewiesen werden")](jenkins-walkthrough-images/image21.png)

Sobald diese Einstellungen gespeichert werden, und Jenkins neu gestartet wird, wird der Sicherheit eingeschaltet sein.


### <a name="disabling-security"></a>Deaktivierung der Sicherheit

Im Falle eines Kennwort vergessen haben oder die Sperre Jenkins-Wide ist es möglich, die Sicherheit zu deaktivieren, indem Sie die folgenden Schritte:

1. Jenkins zu beenden. Wenn Sie Jenkins.app verwenden, können Sie dazu mit der rechten Maustaste auf das Symbol "Jenkins.App" im Dock und Auswählen von Quit im Menü, das wird angezeigt:

    ![](jenkins-walkthrough-images/image19.png "Symbol "App" in der Andocken und Quit im Menü auswählen wird angezeigt, die")
2. Öffnen Sie die Datei **~/.jenkins/config.xml** in einem Text-Editor.
3. Ändern Sie den Wert, der die `<usesecurity></usesecurity>` Element aus `true` auf `false`.
4. Löschen der `<authorizationstrategy></authorizationstrategy>` und die `<securityrealm></securityrealm>` Elemente aus der Datei.
5. Starten Sie Jenkins neu.

# <a name="setting-up-a-job"></a>Einrichten eines Auftrags

Auf der obersten Ebene Jenkins organisiert alle verschiedene Aufgaben erforderlich sind, zum Erstellen von Software in einer *Auftrag*. Ein Auftrag wurde auch Metadaten zugeordnet, Bereitstellen von Informationen über den Build, z. B. zum Abrufen von Quellcode, wie oft der Build ausgeführt werden soll, speziellen Variablen, die zum Erstellen von erforderlich sind und wie Entwickler zu benachrichtigen, wenn der Buildvorgang scheitert.

Aufträge werden erstellt, indem Sie auswählen **Jenkins > neuer Auftrag** über das Menü in der oberen rechten Ecke, wie im folgenden Screenshot gezeigt:


![](jenkins-walkthrough-images/image22.png "Aufträge werden erstellt, indem Sie im Menü in der oberen rechten Ecke Jenkins neuer Auftrag auswählen")

Dies zeigt die **neuer Auftrag [Jenkins]** Seite. Geben Sie einen Namen für den Auftrag, und wählen Sie die **erstellt Softwareprojekten-Stil** Optionsfeld. Der folgende Screenshot zeigt ein Beispiel dafür:

![](jenkins-walkthrough-images/image23.png "Geben Sie einen Namen für den Auftrag, und wählen Sie den Build ein Optionsfeld für Software-Stil-Projekt")

Klicken auf die **OK** Schaltfläche zeigt die Seite "Konfiguration" für den Auftrag. Dies sollte den folgenden Screenshot ähneln:

![](jenkins-walkthrough-images/image24.png "Dies sollte diesem Screenshot ähneln.")

Jenkins organisiert Aufträge in einem Verzeichnis auf der Festplatte befindet sich unter folgendem Pfad: **~/.jenkins/jobs/[JOB NAME]**

Dieser Ordner enthält alle Dateien und Elemente, die spezifisch für den Auftrag z. B. Protokolle, Konfigurationsdateien und den Quellcode, der kompiliert werden muss.

Sobald der erste Auftrag erstellt wurde, muss er mit einem oder mehreren der folgenden konfiguriert werden:

 - Das Quellcodeverwaltungssystem muss angegeben werden.
 - Eine oder mehrere *Buildvorgänge* muss dem Projekt hinzugefügt werden. Dies sind die Schritte oder Tasks, die zum Erstellen der Anwendung erforderlich sind.
 - Der Auftrag muss zugewiesen werden, eine *build-Trigger* – ein Satz von Anweisungen, wie oft Jenkins zum Abrufen des Codes und erstellen Sie das endgültige Projekt zu informieren.

## <a name="configuring-source-code-control"></a>Konfigurieren der Quellcodeverwaltung

Die erste Aufgabe, die Jenkins ausführt, wird der Quellcode vom Verwaltungssystem Source-Code abgerufen werden. Jenkins unterstützt viele der gängigen Source Code Verwaltungssystemen heute verfügbar. Dieser Abschnitt enthält zwei gängige Systeme Git- und Team Foundation Server. Jede dieser Quellcode-Verwaltungssystemen ist in den folgenden Abschnitten ausführlicher erläutert.

### <a name="using-git-for-source-code-control"></a>Verwenden Git für die Quellcodekontrolle

Bei Verwendung von TFS für Quellcodeverwaltungssystem [überspringen](#Using_TFS_for_Source_Code_Management) diesem Abschnitt und fahren Sie mit dem nächsten Abschnitt mit TFS.

Jenkins unterstützt Git gebrauchsfertigen – keine zusätzliche Plug-Ins erforderlich sind. Wenn Sie Git verwenden, klicken Sie auf die **Git** Optionsfeld aus, und geben Sie die URL für das Git-Repository, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image25.png "Wenn Sie Git verwenden, klicken Sie auf das Optionsfeld Git, und geben Sie die URL für das Git-repository")

Nachdem die Änderungen gespeichert wurden, ist Git-Konfiguration abgeschlossen.

### <a name="using-tfs-for-source-code-management"></a>Verwendung von TFS für Quellcode-Management

Dieser Abschnitt gilt nur für TFS-Benutzern.

Klicken Sie auf die **Team Foundation Server** Optionsfeld und TFS-Konfigurationsabschnitts sollte angezeigt werden, ähnlich wie im folgenden Screenshot ist:


![](jenkins-walkthrough-images/image26.png "Klicken Sie auf das Optionsfeld für Team Foundation Server und TFS-Konfigurationsabschnitts sollte angezeigt werden.")

Geben Sie die erforderlichen Informationen für TFS. Der folgende Screenshot zeigt ein Beispiel für das abgeschlossene Formular:

![](jenkins-walkthrough-images/image27.png "Diese bildschirmabbildung zeigt ein Beispiel für das abgeschlossene Formular")

### <a name="testing-the-source-code-control-configuration"></a>Testen der Source Code Control-Konfiguration

Sobald die entsprechende quellcodeverwaltung konfiguriert wurde, klicken Sie auf **speichern** um die Änderungen zu speichern. Dies wird Sie auf der Startseite für den Auftrag zurückgegeben, der den folgenden Screenshot ähnelt:

![](jenkins-walkthrough-images/image28.png "Dies wird Sie auf der Startseite für den Auftrag zurückgegeben, der diesem Screenshot aussehen wird")

Die einfachste Methode zum Überprüfen, ob das Quellcodeverwaltungssystem ordnungsgemäß konfiguriert ist ist, manuell einen Build auszulösen, obwohl es keine Build Aktionen angegeben gibt. Um einen Build manuell zu starten, hat der Startseite des Auftrags eine **erstellen jetzt** link im Menü auf der linken Seite, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image29.png "Um einen Build manuell zu starten, hat der Startseite des Auftrags einen Link erstellen jetzt im Menü auf der linken Seite")

Wenn ein Build gestartet wurde, zeigt das Dialogfeld "Verlauf erstellen" eine blinkende blaue Kreis, eine Statusanzeige angezeigt, die Build-Nummer und die Uhrzeit, die der Build, ähnlich wie im folgenden Bildschirmfoto gestartet:

![](jenkins-walkthrough-images/image30.png "Wenn ein Build gestartet wurde, zeigt das Dialogfeld "Verlauf erstellen" eine blinkende blaue Kreis, eine Statusanzeige angezeigt, die Build-Nummer und die Uhrzeit, die zu der Build gestartet")

Wenn der Auftrag erfolgreich ist, wird ein Kreis angezeigt. Wenn der Auftrag ein Fehler auftritt, wird ein roter Kreis angezeigt.

Zur Unterstützung bei der Behandlung von Problemen, die als Teil des Builds auftreten können, werden die Jenkins alle von der Konsolenausgabe für den Auftrag erfasst. Um die Konsolenausgabe anzuzeigen, klicken Sie auf den Auftrag im **erstellen Verlauf**, und klicken Sie dann auf die **Konsolenausgabe** Link im linken Menü. Der folgende Screenshot zeigt die **Konsolenausgabe** Link sowie einige der Ausgabe aus einem Auftrag erfolgreich ausgeführt:

![](jenkins-walkthrough-images/image31.png "Diese bildschirmabbildung zeigt die Konsolenausgabe Link sowie einige der Ausgabe aus einem erfolgreichen Auftrag")

### <a name="location-of-build-artifacts"></a>Speicherort der Buildartefakte

Jenkins Ruft den gesamten Quellcode in einem speziellen Ordner mit dem Namen einer *Arbeitsbereich*. Dieses Verzeichnis kann dem Ordner "" an folgender Stelle ermittelt werden:

    ~/.jenkins/jobs/[JOB NAME]/workspace

Der Pfad zum Arbeitsbereich wird in einer Umgebungsvariablen mit dem Namen gespeichert werden `$WORKSPACE`.

Es ist möglich, den Arbeitsbereichsordner in Jenkins zu durchsuchen, indem Navigieren auf der Angebotsseite für einen Auftrag, und klicken Sie dann auf die **Arbeitsbereich** Link im linken Menü. Der folgende Screenshot zeigt ein Beispiel für die im Arbeitsbereich "für einen Auftrag mit dem Namen" **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Diese Abbildung zeigt ein Beispiel des Arbeitsbereichs für einen Auftrag mit dem Namen "HelloWorld"")

## <a name="build-triggers"></a>Erstellen von Triggern

Es gibt mehrere verschiedene Strategien zum Initiieren des Builds in Jenkins – diese werden als bezeichnet *erstellen Sie Trigger*. Ein Buildtrigger kann Jenkins entscheiden, wann einen Auftrag starten, und erstellen Sie das Projekt. Es sind zwei der gängigeren Buildtrigger:

- **Erstellen Sie in regelmäßigen Abständen** – dieser Trigger bewirkt, dass Jenkins zum Starten eines Auftrags in angegebenen Intervallen, z. B. alle zwei Stunden oder Arbeitstage Mitternacht. Der Buildvorgang wird gestartet, unabhängig davon, ob es wurden Änderungen in das Quellcoderepository.
- **Abruf SCM** – dieses Triggers Quellcodeverwaltungssystem in regelmäßigen Abständen abruft. Wenn alle Änderungen auf das Quellcoderepository ein Commit ausgeführt wurde, wird die Jenkins einen neuen Build gestartet.

Abrufen von SCM ist beliebten Trigger, da schnell ein Feedback bereitgestellt wird, wenn ein Entwickler Änderungen ein Commit ausgeführt, die dazu führen, den Build wird dass unterbrochen. Dies ist hilfreich für Warnungen von Teams, die einige kürzlich ein Commit ausgeführt wurde Code Probleme verursacht, und ermöglicht Entwicklern, das das Problem zu beheben, während die Änderungen noch frisch Bedenken sind.

Regelmäßige Builds werden häufig verwendet, erstellen Sie eine Version der Anwendung, die Verteilung an Tester. Ein regelmäßiger Build kann z. B. Freitagabend geplant werden, damit Mitglieder der QA-Teams die Arbeit von der vorherigen Woche testen können.


## <a name="compiling-a-xamarinios-applications"></a>Kompilieren von einem Xamarin.iOS-Anwendungen
Xamarin.iOS Projekte kompiliert werden können, an der Befehlszeile mit `xbuild` oder `msbuild`. Shell-Befehl wird im Kontext des Benutzerkontos ausgeführt, die Jenkins ausgeführt wird. Es ist wichtig, dass das Benutzerkonto Zugriff auf das Bereitstellungsprofil verfügt, damit die Anwendung ordnungsgemäß für die Verteilung gepackt werden kann. Es ist möglich, diese Shell-Befehle zur Konfigurationsseite Auftrags hinzufügen.

Führen Sie einen Bildlauf nach unten, um die **erstellen** Abschnitt. Klicken Sie auf der **Add Buildschritt** Schaltfläche und wählen Sie **ausführen Shell**, wie der folgende Screenshot veranschaulicht:

![](jenkins-walkthrough-images/image33.png "Klicken Sie auf die Schaltfläche "Build-Schritt hinzufügen", und wählen Sie die Execute-shell")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

## <a name="building-a-xamarinandroid-project"></a>Erstellen eines Projekts Xamarin.Android

Erstellen eines Projekts Xamarin.Android ist ein Konzept zum Erstellen eines Projekts Xamarin.iOS sehr ähnlich. Um ein APK aus einem Xamarin.Android-Projekt zu erstellen, muss Jenkins konfiguriert werden, um die folgenden beiden Schritte ausführen:

 - Kompilieren Sie das Projekt mit der MSBuild-Plug-in
 - Anmeldung und Zip richten Sie die APK mit einer gültigen Version Schlüsselspeicher aus.

Diese beiden Schritte werden in den nächsten beiden Abschnitten ausführlicher erläutert.

## <a name="creating-the-apk"></a>Erstellt die APK

Klicken Sie auf die **Add Buildschritt** aus, und wählen Sie **Erstellen eines Visual Studio-Projekt oder einer Projektmappe mithilfe von MSBuild**, wie im folgenden Screenshot gezeigt:

![](jenkins-walkthrough-images/image36.png "Erstellen der APK klicken Sie auf die Schaltfläche Build Schritt "hinzufügen", und wählen Sie Build ein Visual Studio-Projekt oder eine Projektmappe mithilfe von MSBuild")

Nachdem das Projekt mit der Buildschritt hinzugefügt wird, geben Sie die Felder, die angezeigt werden. Der folgende Screenshot ist ein Beispiel für das abgeschlossene Formular:

![](jenkins-walkthrough-images/image37.png "Nachdem das Projekt mit der Buildschritt hinzugefügt wird, geben Sie die Felder, die angezeigt werden")


Führt diese Buildschritt `xbuild` in der **$WORKSPACE** Ordner. Die MSBuild-Datei erstellen wird festgelegt, um die **Xamarin.Android.csproj** Datei. Die **Befehlszeilenargumente** Geben Sie einen Releasebuild des Ziels **PackageForAndroid**. Das Produkt in diesem Schritt werden ein APK, am folgenden Speicherort:

    $WORKSPACE/[PROJECT NAME]/bin/Release

Der folgende Screenshot zeigt ein Beispiel für diese APK:

![](jenkins-walkthrough-images/image38.png "Diese bildschirmabbildung zeigt ein Beispiel für diese APK")

Diese APK ist nicht bereit für Bereitstellung, wurde nicht mit einem privaten Keystore signiert und Zip ausgerichtet sein muss.

### <a name="signing-and-zipaligning-the-apk-for-release"></a>Signierung und Zipaligning der APK für Version

Signieren und Zipaligning der APK sind technisch zwei getrennten Aufgaben, die von zwei separaten Befehlszeilentools aus dem Android SDK ausgeführt werden. Allerdings ist es praktisch, den sie in einem Build-Aktion ausführen. Weitere Informationen zum Anmelden und Zipaligning ein APK finden Sie unter Xamarins-Dokumentation zum Vorbereiten einer Android-Anwendung für Version.

Beide Befehle erfordern Befehlszeilenparameter, die zwischen den Projekten variieren können. Darüber hinaus sind einige dieser Befehlszeilenparameter Kennwörter, die nicht in der Konsolenausgabe angezeigt werden sollen, wenn der Build ausgeführt wird. Wir werden einige dieser Parameter über die Befehlszeile in Umgebungsvariablen speichern. Die Umgebungsvariablen für die Anmeldung und/oder Zip Ausrichten von erforderlich sind, werden in der folgenden Tabelle beschrieben:

<table>
    <thead>
        <tr>
            <td>Umgebungsvariablen</td>
            <td>Beschreibung</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>KEYSTORE_FILE</td>
            <td>Dies ist der Pfad zur Keystore zum Signieren der APK</td>
        </tr>
        <tr>
            <td>KEYSTORE_ALIAS</td>
            <td>Der Schlüssel im Schlüsselspeicher, der zum Signieren der APK verwendet wird.</td>
        </tr>
        <tr>
            <td>INPUT_APK</td>
            <td>Die durch die erstellte APK `xbuild`.</td>
        </tr>
        <tr>
            <td>SIGNED_APK</td>
            <td>Signierte APK von erzeugten `jarsigner`.</td>
        </tr>
        <tr>
            <td>FINAL_APK</td>
            <td>Dies ist die ZIP-Datei APK, die vom erzeugt wird ausgerichtet `zipalign`.</td>
        </tr>
        <tr>
            <td>STORE_PASS</td>
            <td>Dies ist das Kennwort, das Zugriff auf den Inhalt des Schlüsselspeichers für die Datei singend verwendet wird.</td>
        </tr>
    </tbody>
</table>

Wie im Abschnitt beschrieben wird, können diese Umgebungsvariablen während des Erstellungsvorgangs Verwenden des Plug-Ins EnvInject festgelegt werden. Der Auftrag müssen einen neuen Build Schritt hinzugefügt basierend auf die Inject-Umgebungsvariablen, wie in der nächste Screenshot dargestellt:

![](jenkins-walkthrough-images/image39.png "Der Auftrag müssen einen neuen Build Schritt hinzugefügt, basierend auf den Inject-Umgebungsvariablen")

In der **Eigenschaften Inhalten** bilden Feld, das angezeigt wird, Umgebung, die Variablen hinzugefügt werden, jeweils eines pro Zeile im folgenden Format:

    ENVIRONMENT_VARIABLE_NAME = value

Der folgende Screenshot zeigt die Umgebungsvariablen, die zum Signieren der APK erforderlich sind:

![](jenkins-walkthrough-images/image40.png "Diese bildschirmabbildung zeigt die Umgebungsvariablen, die zum Signieren der APK erforderlich sind")

Beachten Sie, dass einige der Umgebungsvariablen für die APK-Dateien erstellt werden, auf die `WORKSPACE` -Umgebungsvariablen angegeben.

Die endgültige Umgebungsvariable ist das Kennwort zum Zugreifen auf den Inhalt des Keystore: `STORE_PASS`. Kennwörter dienen als vertrauliche Werte, die verdeckt oder in Protokolldateien ausgelassen werden sollen. Das Plug-in EnvInject kann konfiguriert werden, um diese Werte zu schützen, sodass sie nicht in Protokolle angezeigt werden.

Unmittelbar vor der **erstellen** Teil der Auftragskonfiguration ist eine **Build-Umgebung** Abschnitt. Wenn die **einfügen Kennwörter** Kontrollkästchen ein-oder ausgeschaltet ist, eine Form Felder angezeigt werden. Diese Felder werden verwendet, um den Namen und Wert der Umgebungsvariablen zu erfassen. Der folgende Screenshot ist ein Beispiel zum Hinzufügen der `STORE_PASS` -Umgebungsvariablen angegeben:

![](jenkins-walkthrough-images/image41.png "Diesem Screenshot ist ein Beispiel für die Umgebungsvariable STOREPASS hinzufügen")

Sobald die Umgebungsvariablen initialisiert wurden, besteht der nächste Schritt fügen einen Schritt der Erstellung für das Signieren und Ausrichten von der APK zip. Unmittelbar nach der Buildschritt einzufügende Umgebungsvariablen für die anderen werden **ausführen Shell** Befehl Build, der ausgeführt wird, `jarsigner` und `zipalign`. Jeder Befehl wird eine Zeile nach oben ausgeführt, wie im folgenden Codeausschnitt gezeigt:


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

Der folgende Screenshot zeigt ein Beispiel zum Eingeben der `jarsigner` und `zipalign` Befehle in dem Schritt:

![](jenkins-walkthrough-images/image42.png "Diese Abbildung zeigt ein Beispiel zum Eingeben von Jarsigner und Zipalign-Befehle in dem Schritt")

Sobald die Buildvorgänge vorhanden sind, ist es empfiehlt sich, das Auslösen eines manuellen Builds ein, um sicherzustellen, dass alles funktioniert. Wenn der Buildvorgang scheitert, die **Konsolenausgabe** sollte überprüft werden, Informationen zu zum Fehlschlagen des Buildvorgangs Ursache.

## <a name="submitting-tests-to-test-cloud"></a>Übermitteln von Tests mit Test-Cloud

Automatisierte Tests können Test Cloud Shellbefehle übermittelt werden. Weitere Informationen zum Einrichten eines Testlaufs in Xamarin Test Cloud haben wir Handbücher für die Verwendung von [Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/) oder [Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).


#<a name="summary"></a>Zusammenfassung

In diesem Handbuch wir Jenkins als einem Build-Server unter Mac OS X eingeführt und konfiguriert, um zu kompilieren und Vorbereiten von Xamarin mobile Anwendungen für Version. Wir haben Jenkins auf einem Mac OS X-Computer zusammen mit mehreren-Plug-Ins zur Unterstützung des Buildprozesses installiert. Es erstellt und konfiguriert einen Auftrag, der Code aus TFS oder Git abrufen, und kompilieren Sie diesen Code in einer Freigabe-Anwendung bereit. Wir untersucht auch zwei unterschiedliche Möglichkeiten zum Planen, wann Aufträge ausgeführt werden soll.

## <a name="related-links"></a>Verwandte Links

- [Continuous Integration](https://developer.xamarin.com~/testcloud/ci/)
- [Übermitteln von Tests mit Xamarin Test Cloud](https://developer.xamarin.com~/testcloud/submitting/)
- [Warum wird Jenkins von Xamarin nicht werden unterstützt?](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
