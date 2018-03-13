---
title: Herstellen einer Verbindung mit dem Mac
description: "Mit Xamarin.iOS für Visual Studio können Entwickler iOS-Anwendungen auf einem Windows-Computer mithilfe der Visual Studio-IDE erstellen und debuggen. In diesem Leitfaden werden die Funktionen von Xamarin.iOS für Visual Studio und das Herstellen einer Verbindung zum Mac-Buildhost erklärt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: c60927593f062c8ac9694d889ffbf581c09bab82
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-to-the-mac"></a>Herstellen einer Verbindung mit dem Mac

_Mit Xamarin.iOS für Visual Studio können Entwickler iOS-Anwendungen auf einem Windows-Computer mithilfe der Visual Studio-IDE erstellen und debuggen. In diesem Leitfaden werden die Funktionen von Xamarin.iOS für Visual Studio und das Herstellen einer Verbindung zum Mac-Buildhost erklärt._

Visual Studio stellt über SSH eine Verbindung mit dem Mac her. Dies bietet zahlreiche Vorteile:

- Der Build-Agent kann mit Visual Studio direkt gestartet und gesteuert werden. Es gibt keine für den Benutzer sichtbare Anwendung mehr, die manuell gestartet und beendet werden muss.

- Der neue Verbindungs-Manager in Visual Studio erkennt, authentifiziert und speichert den Mac-Buildhost.

- Da die gesamte Kommunikation über SSH sicher getunnelt wird, ist nur eine einzelne Portverbindung mit Port 22 erforderlich.

- Visual Studio wird unmittelbar über Änderungen informiert. So wird beispielsweise die Symbolleiste sofort aktualisiert, wenn ein iOS-Gerät angeschlossen wird.

- Mehrere Instanzen von Visual Studio können gleichzeitig eine Verbindung herstellen.

- Die Verbindung wirkt sich nicht störend auf die Entwicklung aus. Zur Herstellung einer Verbindung mit dem Mac wird nur aufgefordert, wenn ein Vorgang ausgeführt wird, für den der Mac erforderlich ist, z.B. beim Debuggen oder der Verwendung des iOS-Designers.

Die Verbindung zum Mac besteht aus mehreren Prozessen für die unterschiedlichen Funktionsbestandteile, wie z.B. den iOS-Designer-Agent und den Build-Agent, die von einem Broker verwaltet werden. Dieser Broker wird von Visual Studio verwaltet und aktualisiert. Er startet die einzelnen unabhängigen Prozesse automatisch neu, falls sie abstürzen.

Das folgende Diagramm zeigt eine einfache Übersicht über den Xamarin.iOS-Entwicklungsworkflow:

[![iOS-Entwicklungsworkflow](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  Visual Studio startet einen separaten MSBuild-Prozess zum Erstellen der Projekte. Dieser Prozess stellt eine neue Verbindung mit dem Mac her. Dadurch bestehen tatsächlich zwei SSH-Verbindungen zwischen Windows und Mac während des Build-Prozess von Visual Studio. Bei der Verbindungsherstellung über die [Befehlszeile](#commandline) wird nur der eine MSBuild-Prozess erstellt. Zur Vereinfachung des Diagramms werden die gesamten Verbindungen durch einen Pfeil dargestellt.

## <a name="requirements"></a>Anforderungen

Mit Xamarin.iOS für Visual Studio können Entwickler Erstaunliches vollbringen: Sie erstellen und debuggen iOS-Anwendungen auf einem Windows-Computer mithilfe der Visual Studio-IDE. Doch dies ist nicht allein mit Xamarin.iOS für Visual Studio möglich. iOS-Anwendungen können nicht ohne den Compiler von Apple erstellt und auch nicht ohne die Zertifikate und Tools zum Codesignieren von Apple bereitgestellt werden. Daher benötigen Sie für die Installation von Xamarin.iOS für Visual Studio eine Verbindung zu einem Mac OS X-Computer im Netzwerk (als *Host* oder *Buildhost* bezeichnet), der diese Aufgaben ausführt. Sobald die Xamarin-Tools konfiguriert sind, wird der Prozess so nahtlos wie möglich ausgeführt.

### <a name="system-requirements"></a>Systemanforderungen

Die Systemanforderungen finden Sie im Handbuch [Installieren von Xamarin.iOS unter Windows](~/ios/get-started/installation/windows/index.md#system-requirements)


#### <a name="compatibility"></a>Kompatibilität

> [!IMPORTANT]
>  Der Windows-Computer muss die gleiche Version von Xamarin.iOS verwenden wie der Mac-Computer, mit dem er verbunden ist. So stellen Sie sicher, dass dies der Fall ist:                                                    
>                                                                                                                 
> - **Visual Studio 2015 und frühere Versionen**: Stellen Sie sicher, dass den gleichen [Updatekanal](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) eingestellt haben wie Visual Studio für Mac.
>                                                                                                                 
> - **Visual Studio 2017, Releaseversion**: Stellen Sie sicher, dass Sie in Visual Studio für Mac den **stabilen** Kanal eingestellt haben.
>                                                                                                                 
> - **Visual Studio 2017, Vorschauversion**: Stellen Sie sicher, dass Sie in Visual Studio für Mac den **Alpha**-Kanal eingestellt haben. Visual Studio überprüft nicht, ob Xamarin.iOS SDK und Xcode vorhanden sind und kompatible Versionen aufweisen.
>   Dies wird vom Build-Agent überprüft und kann zu Buildfehlern führen. Es wird außerdem vom iOS-Designer-Agent überprüft, was zu Designer-Fehlern führen kann.

### <a name="connecting-to-the-mac"></a>Herstellen einer Verbindung mit dem Mac

#### <a name="mac-setup"></a>Mac-Setup

Zum Einrichten des Mac-Hosts müssen Sie die Kommunikation zwischen der Xamarin-Erweiterung für Visual Studio und Ihrem Mac aktivieren. Lassen Sie dafür die **entfernte Anmeldung** auf Ihrem Mac zu, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie *Spotlight* (**⌘-Bereich**), und suchen Sie nach *Entfernte Anmeldung*. Wählen Sie anschließend den Ergebniseintrag *Freigabe* aus. Dadurch werden im *Freigabe*-Bereich die *Systemeinstellungen* geöffnet:

   [![Spotlight-Suche für Remoteanmeldung](images/spotlight.png)](images/spotlight.png#lightbox)

2. Aktivieren Sie in der *Dienst*-Liste auf der linken Seite das Kontrollkästchen *Entfernte Anmeldung*, um die Verbindung von Xamarin für Visual Studio zu Ihrem Mac zuzulassen:

   [![Aktivieren der Option „Remoteanmeldung“ in der Liste der Dienste](images/sharing.png)](images/sharing.png#lightbox)

3. Stellen Sie sicher, dass die *entfernte Anmeldung* so festgelegt ist, dass der Zugriff entweder für *Alle Benutzer* zugelassen wird oder dass Ihr Mac-Benutzername oder -Gruppe in der Liste der zugelassenen Benutzer auf der rechten Seite enthalten ist.

Falls Ihre OS X-Firewall so festgelegt ist, dass signierte Anwendungen standardmäßig gesperrt werden, sollten Sie zudem `mono-sgen` zulassen, um eingehende Verbindungen empfangen zu können. In diesem Fall werden Sie in einem Warnungsdialogfeld dazu aufgefordert.

Eine aktuelle, geöffnete Sitzung auf Ihrem Mac sollte jetzt von Visual Studio erkannt werden, sofern sie sich im selben Netzwerk befindet.

Der Agent auf Ihrem Mac wird von Visual Studio gestartet und beendet. Sie als Benutzer müssen keine weiteren Aktionen ausführen.

### <a name="windows-setup"></a>Windows-Setup

[Installieren](~/ios/get-started/installation/windows/index.md) Sie Xamarin-Tools auf Ihrem Windows-Computer.

### <a name="connecting-to-the-mac-build-host"></a>Verbinden mit dem Mac-Buildhost

Es gibt zwei Möglichkeiten, eine Verbindung mit dem Mac-Buildhost herzustellen:

Über die iOS-Symbolleiste:

[![Die iOS-Symbolleiste](images/image1.png)](images/image1.png#lightbox)

Oder in Visual Studio über **Tools > Optionen**: Wählen Sie **Xamarin > iOS-Einstellungen** aus, und klicken Sie auf die Schaltfläche **Xamarin Mac-Agent suchen**:

[![Ermitteln des Xamarin Mac-Agents](images/image2.png)](images/image2.png#lightbox)

In beiden Fällen wird das folgende **Mac-Agent**-Dialogfeld angezeigt:

[![Das Dialogfeld „Mac-Agent“](images/image3.png)](images/image3.png#lightbox)

Es enthält eine Liste aller Computer, die entweder bereits verbunden und als bekannte Computer gespeichert wurden, oder die für eine *entfernte Anmeldung* verfügbar sind.

Doppelklicken Sie auf einen Mac, um ihn auszuwählen und eine Verbindung herzustellen. Wenn Sie zum ersten Mal eine Verbindung mit einem Mac herstellen, werden Sie zur Eingabe Ihrer Mac-Benutzeranmeldeinformationen aufgefordert, um die Remoteverbindung zuzulassen:

[![Mac-Benutzeranmeldeinformationen eingeben](images/image4.png)](images/image4.png#lightbox)

Die Anmeldeinformationen werden vom Agent verwendet, um eine neue SSH-Verbindung mit dem Mac herzustellen. Bei erfolgreicher Anmeldung wird ein SSH-Schlüssel erstellt und in der `authorized_keys`-Datei auf diesem Mac [registriert](#commandline). Bei nachfolgenden Verbindungen verwendet der Agent den Benutzernamen und die Schlüsseldatei, um eine Verbindung mit dem zuletzt verbundenen bekannten Buildhost herzustellen.

> [!NOTE]
>  **Hinweis:** Verwenden Sie bei der Eingabe der Anmeldeinformationen Ihren _Benutzernamen_, nicht Ihren _vollständigen Namen_.  Sie können sich Ihren Benutzernamen über den `whoami`-Befehl im Terminal anzeigen lassen.  Beispielsweise wird der Kontoname des folgenden Screenshots **amyb** und nicht **Amy Burns** lauten:
>
> ![Ermitteln des Benutzernamens in der Terminal-App](images/image5.png)

Wurde die Verbindung erfolgreich hergestellt, wird dies im Hostauswahl-Dialogfeld wie unten dargestellt durch ein **Verbunden**-Symbol angezeigt:

[![Das Dialogfeld für die Hostauswahl mit dem Symbol „Verbunden“ daneben](images/image6.png)](images/image6.png#lightbox)

Es kann immer nur ein Mac gleichzeitig verbunden sein.

Für jeden Computer in der Liste, ob verbunden oder nicht, wird durch Klicken mit der rechten Maustaste ein Kontextmenü angezeigt, das die Optionen **Verbinden**, **Verbindung trennen** und **Diesen Mac ignorieren** enthält:

[![Die Kontextmenüs „Verbinden“, „Verbindung trennen“ und „Diesen Mac ignorieren“](images/image7.png)](images/image7.png#lightbox)

Bei der Option **Diesen Mac ignorieren** müssen Sie Ihre Anmeldeinformationen zum Herstellen einer Verbindung erneut eingeben.

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>Einen Mac manuell hinzufügen

Unter bestimmten Umständen müssen Sie einen Mac manuell hinzufügen, wenn der mDNS-Name im Hostauswahl-Dialogfeld nicht angezeigt wird. Führen Sie dazu folgende Schritte aus:

1. Suchen Sie die IP-Adresse Ihres Macs entweder über **Systemeinstellungen > Freigabe > entfernte Anmeldung** auf Ihrem Mac:

   [![Die IP-Adresse des Mac-Computers in den Systemeinstellungen](images/image8.png)](images/image8.png#lightbox)

   Oder suchen Sie Ihre IP-Adresse, indem Sie im Terminal `ipconfig getifaddr en0` eingeben (je nach Art der Verbindung kann die Variable auch `en1`, `en2` usw. sein):

   [![Die IP-Adresse in der Terminal-App](images/image9.png)](images/image9.png#lightbox)

2. Kehren Sie zu Visual Studio zurück, und wählen Sie im Hostauswahl-Dialogfeld die Option **Mac hinzufügen...** aus:

   [![Das Dialogfeld für die Hostauswahl](images/image10.png)](images/image10.png#lightbox)

3. Geben Sie die IP-Adresse Ihres Macs in das Dialogfeld „Mac hinzufügen“ ein, und klicken Sie auf **Hinzufügen**:

   [![IP-Adresse des Mac-Computers in das Dialogfeld „Mac hinzufügen“ eingeben](images/image11.png)](images/image11.png#lightbox)

4. Geben Sie anschließend den Benutzernamen (nicht den vollständigen Namen) Ihres Mac-Administratorkontos und das entsprechende Kennwort ein:

   [![Benutzernamen und Kennwort eingeben](images/image12.png)](images/image12.png#lightbox)

Durch Klicken auf **Anmelden** wird Visual Studio bei Ihrem Mac-Computer mithilfe von SSH angemeldet und der Mac als bekannter Computer hinzugefügt.

<a name="commandline"/>

### <a name="command-line-support"></a>Befehlszeilenunterstützung

Der Agent unterstützt auch das Erstellen einer Xamarin.iOS-Konfiguration über die Befehlszeile.  Übergeben Sie hierzu die folgenden erforderlichen Parameter an MSBuild:

- `ServerAddress`: Die IP-Adresse des Mac-Servers.

- `ServerUser`: Den Benutzernamen (nicht den vollständigen Namen), der zur Anmeldung beim Mac-Server verwendet wird.

- `ServerPassword`: Das Kennwort zur Anmeldung beim Mac-Host (optional).

Der `ServerPassword`-Parameter ist nicht erforderlich.

Stattdessen wird bei der ersten Übergabe eines Kennworts für diese bestimmte Windows-, Mac- und Benutzerkonfiguration (unabhängig davon, ob über Visual Studio oder über die Befehlszeile) ein Schlüsselpaar generiert und für die zukünftige Verwendung auf dem Windows-Computer gespeichert. Sie finden es unter **%localappdata%\Xamarin\MonoTouch\id_rsa**.  Wenn Sie den `ServerPassword`-Parameter nicht übergeben, wird die Schlüsseldatei `id_rsa` zur Authentifizierung verwendet.

Der folgende Beispielbefehl zeigt, wie Sie mithilfe des **xamUser**-Kontos und des Kennworts **MeinKennwort** eine Verbindung zu dem Mac 10.211.55.2 herstellen:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>Zusammenfassung

In diesem Artikel wird die Verbindung zwischen Visual Studio und den iOS-Build- und -Designer-Tools auf dem Mac ausführlich beschrieben. Dadurch sind Sie nun in der Lage, Xamarin.iOS-Apps mit Visual Studio zu erstellen.

### <a name="related-links"></a>Verwandte Links

- [Installation](~/ios/get-started/installation/windows/index.md)
- [Problembehandlung bei der Verbindung](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Connecting a Mac to your Visual Studio environment with XMA (Herstellen einer Verbindung zwischen einem Mac und der Visual Studio-Umgebung mit XMA) (Video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
