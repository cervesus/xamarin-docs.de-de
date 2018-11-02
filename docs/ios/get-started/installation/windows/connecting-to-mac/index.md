---
title: Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung
description: In diesem Leitfaden wird beschrieben, wie Sie das Feature „Mit Mac koppeln“ verwenden, um Visual Studio 2017 mit einem Mac-Buildhost zu verbinden. Dabei werden die Aktivierung der Remoteanmeldung auf dem Mac, das Herstellen einer Verbindung mit dem Mac von Visual Studio 2017 aus und das manuelle Hinzufügen eines Mac-Buildhosts zum Windows-Computer erläutert.
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/29/2018
ms.openlocfilehash: befeb92529aa066f6106e855ed1fbdfe49c51e66
ms.sourcegitcommit: c59e1882aa4af3ce36fba5c6eaeb5cf73a9cb289
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50919041"
---
# <a name="pair-to-mac-for-xamarinios-development"></a>Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung

_In diesem Leitfaden wird beschrieben, wie Sie die Option „Mit Mac koppeln“ verwenden, um Visual Studio 2017 mit einem Mac-Buildhost verbinden._

## <a name="overview"></a>Übersicht

Zum Erstellen nativer iOS-Anwendungen sind die Apple-Buildtools erforderlich, die nur auf einem Mac ausgeführt werden können. Aus diesem Grund muss Visual Studio 2017 eine Verbindung zu einem Mac herstellen, der über ein Netzwerk zugänglich ist, da andernfalls keine Xamarin.iOS-Anwendungen erstellt werden können.

Durch das Visual Studio 2017-Feature „Mit Mac koppeln“ werden zuerst Mac-Buildhosts ermittelt. Anschließend wird eine Verbindung mit diesen hergestellt, die Authentifizierung wird vorgenommen, und die Buildhosts werden gespeichert. Hierdurch können iOS-Entwickler unter Windows produktiv arbeiten.

Das Feature „Mit Mac koppeln“ ermöglicht den folgenden Entwicklungsworkflow:

- Entwickler können in Visual Studio 2017 Xamarin.iOS-Code schreiben.

- Visual Studio 2017 stellt eine Netzwerkverbindung mit einem Mac-Buildhost her und verwendet die Buildtools auf diesem Computer, um die iOS-App zu kompilieren und zu signieren.

- Auf dem Mac muss keine eigenständige Anwendung ausgeführt werden, da Visual Studio 2017 über eine sichere SSH-Verbindung die Mac-Builds aufruft.

- Visual Studio 2017 wird unmittelbar über Änderungen informiert. Wenn beispielsweise ein iOS-Gerät an den Mac angeschlossen oder ein Gerät im Netzwerk verfügbar wird, wird die iOS-Symbolleiste sofort aktualisiert.

- Mehrere Instanzen von Visual Studio 2017 können gleichzeitig eine Verbindung mit dem Mac herstellen.

- Die Windows-Befehlszeile kann zur Erstellung von iOS-Anwendungen verwendet werden.

> [!NOTE]
> 
> Führen Sie zunächst die folgenden Schritte aus, bevor Sie die Anweisungen in diesem Leitfaden beachten:
> 
> - Installieren Sie auf einem Windows-Computer [Visual Studio 2017](~/cross-platform/get-started/installation/windows.md).
> - Installieren Sie auf einem Mac [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) und [Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation).
>    - _Nach der Installation müssen Sie Xcode manuell öffnen_, damit sie weitere Komponenten hinzufügen können.
>
> Wenn Sie Visual Studio für Mac nicht installieren möchten, kann Visual Studio 2017 automatisch den Mac-Buildhost mit Xamarin.iOS und Mono konfigurieren.
> Sie müssen noch Xcode installieren und ausführen.
> Weitere Informationen finden Sie unter [Automatische Bereitstellung eines Macs](#automatic-mac-provisioning).

## <a name="enable-remote-login-on-the-mac"></a>Aktivieren der Remoteanmeldung auf dem Mac

Aktivieren Sie zur Einrichtung des Mac-Buildhosts zunächst die Remoteanmeldung:

1. Rufen Sie auf dem Mac die Systemeinstellungen auf, und navigieren Sie zum Bereich **Freigaben**.

2. Aktivieren Sie in der Liste **Dienst** das Kontrollkästchen **Entfernte Anmeldung**.

    ![Aktivieren der Option „Entfernte Anmeldung“](images/sharing.png "Enabling Remote Login")

    Stellen Sie sicher, dass für diese Option die Zugriffseinstellung **Alle Benutzer** festgelegt ist oder dass Ihr Mac-Benutzername oder Ihre Mac-Gruppe in der Liste der zugelassenen Benutzer enthalten ist.

3. Konfigurieren Sie die macOS-Firewall, wenn Sie dazu aufgefordert werden.

    Falls Sie Ihre macOS-Firewall so konfiguriert haben, dass eingehende Verbindungen blockiert werden, sollten Sie für `mono-sgen` den Empfang eingehender Verbindungen zulassen. In diesem Fall werden Sie durch eine Benachrichtigung dazu aufgefordert.

4. Ihr Mac sollte nun von Visual Studio 2017 erkannt werden, wenn dieser sich in demselben Netzwerk wie der Windows-Computer befindet. Wenn der Mac immer noch nicht erkannt wird, können Sie versuchen, diesen [manuell hinzuzufügen](#manually-add-a-mac). Alternativ können Sie sich auch den [Leitfaden zur Problembehebung](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md) ansehen.

## <a name="connect-to-the-mac-from-visual-studio-2017"></a>Herstellen einer Verbindung mit dem Mac über Visual Studio 2017

Nachdem Sie die Remoteanmeldung aktiviert haben, können Sie Visual Studio 2017 mit dem Mac verbinden.

1. Öffnen Sie in Visual Studio 2017 ein vorhandenes iOS-Projekt, oder erstellen Sie über **Datei > Neu > Projekt** ein neues, und wählen Sie anschließend eine iOS-Projektvorlage aus.

2. Öffnen Sie das Dialogfeld **Mit Mac koppeln**. 

    - Klicken Sie dazu auf die Schaltfläche **Mit Mac koppeln** in der iOS-Symbolleiste:

        ![iOS-Symbolleiste mit der hervorgehobenen Schaltfläche „Mit Mac koppeln“](images/ios-toolbar.png "The iOS toolbar, with the Pair to Mac button highlighted")

    - Alternativ können Sie auch **Extras > iOS > Mit Mac koppeln** aufrufen.

    - Im Dialogfeld **Mit Mac koppeln** wird eine Liste aller zuvor verbundenen und zurzeit verfügbaren Mac-Buildhosts angezeigt:

        ![Dialogfeld „Mit Mac koppeln“](images/pairtomac.png "The Pair to Mac dialog")

3. Wählen Sie einen Mac aus der Liste aus. Klicken Sie auf **Verbinden**. 

4. Geben Sie Ihren Benutzernamen und Ihr Kennwort ein.

    - Beim erstmaligen Herstellen einer Verbindung mit einem Mac werden Sie aufgefordert, Ihren Benutzernamen und Ihr Kennwort für diesen Computer einzugeben:

        ![Eingeben eines Benutzernamens und eines Kennworts für den Mac](images/auth.png "Entering a username and password for the Mac")

        > [!TIP]
        > Verwenden Sie bei der Anmeldung Ihren Systembenutzernamen anstelle des vollständigen Namens.

    - Die Anmeldeinformationen werden vom Feature „Mit Mac koppeln“ verwendet, um eine neue SSH-Verbindung mit dem Mac herzustellen. Wenn dieser Vorgang erfolgreich ist, wird der Datei **authorized_keys** auf dem Mac ein Schlüssel hinzugefügt. Bei allen weiteren Verbindungen mit demselben Mac wird die Anmeldung automatisch vorgenommen.

5. Das Feature „Mit Mac koppeln“ konfiguriert den Mac automatisch.

    Ab der [Version 15.6 von Visual Studio 2017](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) werden Mono und Xamarin.iOS auf einem verbundenen Mac-Buildhost bei Bedarf installiert oder aktualisiert. Beachten Sie, dass Xcode nach wie vor manuell installiert werden muss. Weitere Informationen finden Sie unter [Automatische Bereitstellung eines Macs](#automatic-mac-provisioning).

6. Suchen Sie nach dem Symbol für den Verbindungsstatus.

    - Wenn Visual Studio 2017 mit einem Mac verbunden wird, wird für das Mac-Element im Dialogfeld **Mit Mac koppeln** ein Symbol angezeigt, das darauf hinweist, dass aktuell eine Verbindung besteht.

        ![Ein verbundener Mac](images/connected.png "A connected Mac")

      Es kann immer nur ein Mac gleichzeitig verbunden sein.

      > [!TIP]
      > Wenn Sie mit der rechten Maustaste auf einen Mac in der Liste **Mit Mac koppeln** klicken, wird ein Kontextmenü angezeigt, in dem Sie die Aktion **Verbinden...**, **Diesen Mac vergessen** oder **Verbindung trennen** ausführen können.
      >
      > ![Kontextmenüelemente des Features „Mit Mac koppeln“](images/contextmenu.png "The Pair to Mac context menus")
      >
      > Falls Sie auf **Diesen Mac vergessen** klicken, werden die Anmeldeinformationen für den ausgewählten Mac verworfen. Wenn Sie erneut eine Verbindung mit diesem Mac herstellen möchten, müssen Sie Ihren Benutzernamen und Ihr Kennwort erneut eingeben.

Sobald Sie eine Kopplung mit dem Mac-Buildhost erfolgreich durchgeführt haben, können Sie Xamarin.iOS-Apps in Visual Studio 2017 erstellen. Informationen hierzu finden Sie im Leitfaden [Einführung in Xamarin.iOS für Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md).

Wenn die Kopplung fehlschlägt, können Sie versuchen, den Mac [manuell hinzuzufügen](#manually-add-a-mac). Alternativ können Sie sich auch den [Leitfaden zur Problembehebung](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md) ansehen.

## <a name="manually-add-a-mac"></a>Manuelles Hinzufügen eines Macs

Wenn ein bestimmter Mac nicht im Dialogfeld **Mit Mac koppeln** aufgeführt wird, können Sie diesen manuell hinzufügen:

1. Ermitteln Sie die IP-Adresse Ihres Macs. 

    - Öffnen Sie **Systemeinstellungen > Freigaben > Entfernte Anmeldung** auf Ihrem Mac:

        [![Die IP-Adresse des Macs unter „Systemeinstellungen“ > „Freigaben“](images/sharing-ipaddress.png "The Mac's IP address in System Preferences > Sharing")](images/sharing.png#lightbox)

    - Alternativ können Sie auch die Befehlszeile verwenden. Führen Sie im Terminal den folgenden Befehl aus: 

        ```bash
        $ ipconfig getifaddr en0
        196.168.1.8
        ```

      Abhängig von Ihrer Netzwerkkonfiguration müssen Sie möglicherweise nicht `en0`, sondern einen anderen Schnittstellennamen verwenden. Beispiele sind `en1` und `en2`.

2. Klicken Sie in Visual Studio 2017 im Dialogfeld **Mit Mac koppeln** auf **Mac hinzufügen...**:

    [![Schaltfläche „Mac hinzufügen...“ im Dialogfeld „Mit Mac koppeln“](images/addtomac.png "The Add Mac button in the Pair to Mac dialog")](images/addtomac-large.png#lightbox)

3. Geben Sie die IP-Adresse des Macs ein, und klicken Sie anschließend auf **Hinzufügen**:

    ![Eingeben der IP-Adresse des Macs](images/enteripaddress.png "Entering the Mac's IP address")

4. Geben Sie Ihren Benutzernamen und Ihr Kennwort für den Mac ein:

    ![Eingeben des Benutzernamens und des Kennworts](images/auth.png "Entering a username and password")

   > [!TIP]
   > Verwenden Sie bei der Anmeldung Ihren Systembenutzernamen anstelle des vollständigen Namens.

5. Klicken Sie auf **Anmelden**, um Visual Studio 2017 über SSH mit dem Mac zu verbinden und den Mac der Liste der bekannten Computer hinzuzufügen.

## <a name="automatic-mac-provisioning"></a>Automatische Bereitstellung eines Macs

Ab der [Version 15.6 von Visual Studio 2017](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) stellt das Feature „Mit Mac koppeln“ automatisch einen Mac mit Softwarekomponenten bereit, die zum Erstellen von Xamarin.iOS-Anwendungen erforderlich sind. Zu diesen Komponenten zählen Mono, Xamarin.iOS (das Softwareframework, nicht die Visual Studio für Mac-IDE) und verschiedene Tools im Zusammenhang mit Xcode (nicht jedoch Xcode selbst).

> [!IMPORTANT]
> - Xcode kann nicht durch das Feature „Mit Mac koppeln“ installiert werden. Sie müssen die IDE daher manuell auf dem Mac-Buildhost installieren. Xcode ist für die Entwicklung mit Xamarin.iOS erforderlich.
> - Für die automatische Bereitstellung eines Macs muss die Remoteanmeldung auf dem Mac aktiviert sein. Außerdem muss ein Windows-Computer über ein Netzwerk auf den Mac zugreifen können. Weitere Informationen finden Sie unter [Aktivieren der Remoteanmeldung auf dem Mac](#enable-remote-login-on-the-mac).
> - Für die automatische Mac-Bereitstellung sind 3 GB freier Speicherplatz auf dem Mac erforderlich, um Xamarin.iOS zu installieren.

Das Feature „Mit Mac koppeln“ installiert die erforderliche Software oder aktualisiert diese, wenn Visual Studio 2017 [eine Verbindung mit dem Mac herstellt](#connect-to-the-mac-from-visual-studio-2017).

### <a name="mono"></a>Mono

Das Feature „Mit Mac koppeln“ überprüft, ob Mono installiert ist. Wenn dies nicht der Fall ist, lädt das Feature die neueste stabile Version von Mono auf den Mac herunter. 

In mehreren Eingabeaufforderungen wird der Status angezeigt. Dies ist auf den folgenden Screenshots zu sehen (zum Vergrößern klicken):

||Überprüfung auf vorhandene Installation|Ausgeführter Downloadvorgang|Installation
|---|---|---|---|
|Mono|[![Mono-Installation nicht vorhanden](images/mono-missing.png "Missing Mono installation")](images/mono-missing-large.png#lightbox)|[![Herunterladen von Mono](images/mono-downloading.png "Downloading Mono")](images/mono-downloading-large.png#lightbox)|[![Installieren von Mono](images/mono-installing.png "Installing Mono")](images/mono-installing-large.png#lightbox)|

### <a name="xamarinios"></a>Xamarin.iOS

Das Feature „Mit Mac koppeln“ führt auf dem Mac für Xamarin.iOS ein Upgrade auf die Version durch, die auf dem Windows-Computer installiert ist.

> [!IMPORTANT]
> Auf dem Mac findet keine Herabstufung einer Alpha- oder Betaversion von Xamarin.iOS zu einer stabilen Version statt. Wenn Sie Visual Studio für Mac installiert haben, müssen Sie Ihren [Releasekanal](https://docs.microsoft.com/visualstudio/mac/update) wie folgt konfigurieren:
> - Wählen Sie bei Verwendung von Visual Studio 2017 den Updatekanal **Stabil** in Visual Studio für Mac aus.
> - Wählen Sie bei Verwendung der Vorschauversion von Visual Studio 2017 den Updatekanal **Alpha** in Visual Studio für Mac aus.

In mehreren Eingabeaufforderungen wird der Status angezeigt. Dies ist auf den folgenden Screenshots zu sehen (zum Vergrößern klicken):

||Überprüfung auf vorhandene Installation|Ausgeführter Downloadvorgang|Installation
|---|---|---|---|
|Xamarin.iOS|[![Xamarin.iOS-Installation nicht vorhanden](images/xamios-missing.png "Missing Xamarin.iOS installation")](images/xamios-missing-large.png#lightbox)|[![Herunterladen von Xamarin.iOS](images/xamios-downloading.png "Downloading Xamarin.iOS")](images/xamios-downloading-large.png#lightbox)|[![Installieren von Xamarin.iOS](images/xamios-installing.png "Installing Xamarin.iOS")](images/xamios-installing-large.png#lightbox)|

### <a name="xcode-tools-and-license"></a>Xcode-Tools und -Lizenz

Das Feature „Mit Mac koppeln“ überprüft auch, ob Xcode installiert wurde und Sie der Lizenz zugestimmt haben. Obwohl das Feature Xcode nicht installiert, werden Sie dazu aufgefordert der Lizenz zuzustimmen. Dies ist auf den folgenden Screenshots zu sehen (zum Vergrößern klicken):

||Überprüfung auf vorhandene Installation|Zustimmung zur Lizenz|
|---|---|---|
|Xcode|[![Xcode-Installation nicht vorhanden](images/xcode-missing.png "Missing Xcode installation")](images/xcode-missing-large.png#lightbox)|[![Xcode-Lizenz](images/xcode-license.png "Xcode license")](images/xcode-license-large.png#lightbox)|

Zusätzlich installiert oder aktualisiert das Feature „Mit Mac koppeln“ verschiedene Pakete, die mit Xcode verteilt werden. Zum Beispiel:

- **MobileDeviceDevelopment.pkg**
- **XcodeExtensionSupport.pkg**
- **MobileDevice.pkg**
- **XcodeSystemResources.pkg**

Die Installation dieser Pakete erfolgt schnell, und es wird keine Eingabeaufforderung angezeigt.

> [!NOTE]
> Diese Tools unterscheiden sich von den Xcode-Befehlszeilentools, die ab macOS 10.9 [zusammen mit Xcode installiert werden](https://developer.apple.com/library/content/technotes/tn2339/_index.html).

### <a name="troubleshooting-automatic-mac-provisioning"></a>Problembehandlung bei der automatischen Bereitstellung eines Macs

Wenn bei der automatischen Bereitstellung eines Macs Probleme auftreten, können Sie sich die Protokolle der Visual Studio 2017-IDE ansehen, die unter **%LOCALAPPDATA%\Xamarin\Logs\15.0** gespeichert werden. Diese Protokolle enthalten möglicherweise Fehlermeldungen, die Sie zur effizienteren Diagnose des Fehlers oder für Supportanfragen verwenden können.

## <a name="build-ios-apps-from-the-windows-command-line"></a>Erstellen von iOS-Apps über die Windows-Befehlszeile

Das Feature „Mit Mac koppeln“ unterstützt die Erstellung von Xamarin.iOS-Anwendungen über die Befehlszeile. Zum Beispiel:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

Die im obigen Beispiel an `msbuild` übergebenen Parameter sind:

- `ServerAddress`: die IP-Adresse des Mac-Buildhosts.
- `ServerUser`: der Benutzername, der zur Anmeldung bei dem Mac-Buildhost verwendet wird.
  Verwenden Sie Ihren Systembenutzernamen anstelle des vollständigen Namens.
- `ServerPassword`: das Kennwort, das zur Anmeldung bei dem Mac-Buildhost verwendet wird.

> [!NOTE]
> Visual Studio 2017 speichert `msbuild` im Verzeichnis **C:\Programme (x86)\Microsoft Visual Studio\2017\\&lt;Version&gt;\MSBuild\15.0\Bin**.

Wenn sich das Feature „Mit Mac koppeln“ zum ersten Mal über Visual Studio 2017 oder über die Befehlszeile bei einem Mac-Buildhost anmeldet, werden SSH-Schlüssel eingerichtet. Durch die Verwendung dieser Schlüssel entfällt bei weiteren Anmeldungen die Angabe des Benutzernamens oder des Kennworts. Neu erstellte Schlüssel werden unter **%LOCALAPPDATA%\Xamarin\MonoTouch** gespeichert.

Wenn der `ServerPassword`-Parameter bei einem Buildaufruf über die Befehlszeile nicht angegeben wird, versucht das Feature „Mit Mac koppeln“, sich mithilfe der gespeicherten SSH-Schlüssel bei dem Mac-Buildhost anzumelden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie mit dem Feature „Mit Mac koppeln“ Visual Studio 2017 mit einem Mac Buildhost verbunden werden kann. Hierdurch können Visual Studio 2017-Entwickler mit Xamarin.iOS native iOS-Anwendungen erstellen.

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung bei der Verbindung](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin Mac Build Agent - Xamarin University Lightning Lecture (Xamarin Mac-Build-Agent: Xamarin University-Kurzvorstellung)](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
- [Einführung in Xamarin.iOS für Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [iOS-Remotesimulator für Windows](~/tools/ios-simulator/index.md)
- [Drahtlose Bereitstellung](~/ios/deploy-test/wireless-deployment.md)
