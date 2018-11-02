---
title: Behebung von Verbindungsproblemen für einen Xamarin.iOS-Buildhost
description: Dieses Handbuch enthält die Schritte zur Problembehandlung für Probleme, die bei der Verwendung des neuen Verbindungs-Managers auftreten, einschließlich Konnektivitäts- und SSH-Problemen.
ms.prod: xamarin
ms.assetid: A1508A15-1997-4562-B537-E4A9F3DD1F06
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: cfc4ecc5bf7ebc5e4c4dae8094fe3eb4ece34068
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112498"
---
# <a name="connection-troubleshooting-for-a-xamarinios-build-host"></a>Behebung von Verbindungsproblemen für einen Xamarin.iOS-Buildhost

_Dieses Handbuch enthält die Schritte zur Behandlung von Problemen, die bei der Verwendung des neuen Verbindungs-Managers auftreten, einschließlich Konnektivitäts- und SSH-Problemen._

## <a name="log-file-location"></a>Speicherort der Protokolldatei

- **Mac** – ~/Library/Logs/Xamarin-[MAJOR.MINOR]
- **Windows** – %LOCALAPPDATA%\Xamarin\Logs

Die Protokolldateien können gefunden werden, indem Sie in Visual Studio zu **Hilfe &gt; Xamarin &gt; Zip-Protokolle** navigieren.


## <a name="wheres-the-xamarin-build-host-app"></a>Wo befindet sich die Xamarin-Buildhostanwendung?

Die Xamarin-Buildhostanwendung aus älteren Versionen von Xamarin.iOS ist nicht mehr erforderlich. Visual Studio stellt den Agent nun automatisch über die Remoteanmeldung bereit und führt ihn im Hintergrund aus. Es gibt keine zusätzliche App, die auf den Mac- oder Windows-Computern ausgeführt wird.


## <a name="troubleshooting-remote-login"></a>Problembehandlung bei der Remoteanmeldung

> [!IMPORTANT]
> Diese Schritte zur Problembehandlung sind in erster Linie für Probleme gedacht, die während der ersten Einrichtung auf einem neuen System auftreten.  Wenn Sie die Verbindung zuvor erfolgreich in einer bestimmten Umgebung genutzt haben und diese plötzlich oder zeitweilig nicht mehr funktioniert, können Sie (in den meisten Fällen) direkt mit der Überprüfung, ob eine der folgenden Methoden hilfreich ist, fortfahren: 
>   * Beenden Sie die verbliebenen Prozesse, wie unten unter [Fehler aufgrund von vorhandenen Buildprozessen](#errors) beschrieben wird. 
>   * Deaktivieren Sie die Agents, wie unter [Deaktivieren der Broker-, IDB-, Build- und Designer-Agents](#clearing) beschrieben, und verwenden Sie dann eine LAN-Internetverbindung, und stellen Sie eine Verbindung direkt über die IP-Adresse her wie unter [Es konnte keine Verbindung mit MacBuildHost.local hergestellt werden. Versuchen Sie es erneut.](#tryagain) beschrieben.  
> Wenn keine dieser Maßnahmen das Problem lösen kann, befolgen Sie die Anweisungen in [Schritt 9](#stepnine), um einen neuen Fehlerbericht einzureichen.

1. Überprüfen Sie, dass Sie kompatible Xamarin.iOS-Versionen auf Ihrem Mac installiert haben. Um dies mit Visual Studio 2017 durchzuführen, stellen Sie sicher, dass Sie sich im **stabilen** Verteilungskanal in Visual Studio für Mac befinden. Stellen Sie in Visual Studio 2015 und früher sicher, dass Sie sich auf beiden IDEs auf dem gleichen Vertriebskanal befinden.
    * Wechseln Sie in Visual Studio für Mac zu **Visual Studio für Mac > Nach Updates suchen...**, um den **Updatekanal** anzuzeigen oder zu ändern.
    * Überprüfen Sie in Visual Studio 2015 und früher den Verteilungskanal unter **Extras > Optionen > Xamarin > Andere**.

2. Stellen Sie sicher, dass die **Remoteanmeldung** auf dem Mac aktiviert ist. Legen Sie den Zugriff für **Nur diese Benutzer** fest, und stellen Sie sicher, dass Ihr Mac-Benutzer in der Liste oder Gruppe inbegriffen ist:

    [![](troubleshooting-images/troubleshooting-image1.png "Zugriff nur für diese Benutzer festlegen")](troubleshooting-images/troubleshooting-image1.png#lightbox)

3. Überprüfen Sie, dass die Firewall eingehende Verbindungen über Port 22 – den Standardwert für SSH – zulässt:

    [![](troubleshooting-images/troubleshooting-image2.png "Überprüfen Sie, ob die Firewall eingehende Verbindungen über Port 22 zulässt")](troubleshooting-images/troubleshooting-image2.png#lightbox)

    Wenn Sie **Automatically allow signed software to receive incoming connections** (Signierte Software automatisch für den Empfang eingehender Verbindungen zulassen) deaktiviert haben, wird in OS X während des Verbindungsaufbaus ein Dialogfeld angezeigt, das Sie auffordert, `mono-sgen` oder `mono-sgen32` für den Empfang von eingehenden Verbindungen zuzulassen. Klicken Sie in diesem Dialogfeld auf **Zulassen**:

    [![](troubleshooting-images/troubleshooting-image4a.png "Klicken Sie in diesem Dialogfeld auf „Zulassen“")](troubleshooting-images/troubleshooting-image4a.png#lightbox)

4. Vergewissern Sie sich, dass Sie über das Benutzerkonto auf diesem Mac angemeldet sind und über eine aktive GUI-Sitzung verfügen.

5. Stellen Sie sicher, dass Sie eine Verbindung mit dem Mac über den _Benutzernamen_ statt über den _Vollständigen Namen_ herstellen. Dies vermeidet eine bekannte Einschränkung für vollständige Namen, die Sonderzeichen enthalten.

    Sie finden Ihren _Benutzernamen_, indem Sie den `whoami`-Befehl in **Terminal.app** ausführen.

    Beispielsweise wird der Kontoname des folgenden Screenshots **amyb** und nicht **Amy Burns** lauten:

    [![](troubleshooting-images/troubleshooting-image5a.png "Abrufen des Kontonamens aus der Terminal-App")](troubleshooting-images/troubleshooting-image5a.png#lightbox)


6. Überprüfen Sie, dass die IP-Adresse, die Sie für den Mac verwenden, richtig ist. Sie finden die IP-Adresse unter **Systemeinstellungen > Freigabe > Remoteanmeldung** auf dem Mac.

    [![](troubleshooting-images/troubleshooting-image17.png "Die IP-Adresse in der App „Systemeinstellungen“")](troubleshooting-images/troubleshooting-image17.png#lightbox)

7. Wenn Sie die IP-Adresse für den Mac bestätigt haben, führen Sie einen `ping` an diese Adresse in `cmd.exe` unter Windows durch:

    ```
    ping 10.1.8.95
    ```
    
    Wenn der Ping fehlschlägt, ist der Mac vom Windows-Computer nicht _routingfähig_. Dieses Problem muss auf der Ebene der LAN-Konfiguration zwischen den zwei Computern behoben werden. Beide Computer müssen sich im selben lokalen Netzwerk befinden.

8. Als Nächstes überprüfen Sie, ob der `ssh`-Client aus OpenSSH von Windows aus erfolgreich eine Verbindung mit dem Mac herstellen kann. Eine Möglichkeit zur Installation dieses Programms ist die Installation von [Git für Windows](https://git-for-windows.github.io/). Sie können dann eine **Git Bash**-Eingabeaufforderung starten und versuchen, sich mit `ssh` mit Ihrem Benutzernamen und der IP-Adresse beim Mac anzumelden:

    ```bash
    ssh amyb@10.1.8.95
    ```
    <a name="stepnine" />

9. Wenn **Schritt 8 erfolgreich ist**, können Sie versuchen, einen einfachen Befehl wie `ls` über die Verbindung auszuführen:

    ```bash
    ssh amyb@10.1.8.95 'ls'
    ```
    
    Dies sollte die Inhalte des Basisverzeichnisses auf dem Mac auflisten. Wenn der `ls`-Befehl ordnungsgemäß funktioniert, die Verbindung von Visual Studio jedoch weiterhin fehlschlägt, sehen Sie sich den Abschnitt [Bekannte Probleme und Einschränkungen](#knownissues) zu spezifischen Komplikationen von Xamarin an. Wenn keines dieser Probleme mit Ihrem übereinstimmt, übermitteln Sie in der Entwicklercommunity einen neuen Fehlerbericht, indem Sie in Visual Studio zu **Hilfe > Feedback senden > Problem melden** navigieren und die unter [Ausführliche Protokolldateien](#verboselogs) beschriebenen Protokolle anhängen.

10. Wenn **Schritt 8 fehlschlägt**, können Sie den folgenden Befehl im Terminal auf dem Mac ausführen, um festzustellen, ob der SSH-Server _alle_ Verbindungen akzeptiert:

    ```bash
    ssh localhost
    ```
    
11. Wenn Schritt 8 fehlschlägt, aber **Schritt 10 erfolgreich ausgeführt wurde**, liegt das Problem wahrscheinlich darin, dass der Port 22 auf dem Mac-Buildhost aufgrund der Netzwerkkonfiguration nicht von Windows aus zugänglich ist. Mögliche Konfigurationsprobleme sind:

    - Die Einstellungen der OS X-Firewall verhindern die Verbindung. Achten Sie darauf, dass Sie Schritt 3 erneut überprüfen.

        Gelegentlich kann die Konfiguration pro App für die OS X-Firewall auch in einem ungültigen Zustand enden, wenn die in den Systemeinstellungen angezeigten Einstellungen nicht das tatsächliche Verhalten widerspiegeln. Wenn Sie die Konfigurationsdatei (**/Library/Preferences/com.apple.alf.plist**) löschen und den Computer neu starten, kann dies das Standardverhalten Ihres Computers wiederherstellen. Eine Möglichkeit zum Löschen der Datei ist, **/Library/Preferences** unter **Gehe zu &gt; Gehe zu Ordner** im Finder einzugeben und dann die Datei **com.apple.alf.plist** in den Papierkorb zu verschieben.

    - Die Firewall-Einstellungen von einem der Router zwischen dem Macintosh- und dem Windows-Computer blockiert die Verbindung.

    - Ausgehende Verbindungen auf Remoteport 22 werden von Windows selbst untersagt. Dies wäre ungewöhnlich. Es ist möglich, die Windows-Firewall so zu konfigurieren, dass ausgehende Verbindungen nicht zugelassen sind, aber die Standardeinstellung ermöglicht alle ausgehenden Verbindungen.

    - Der Mac-Buildhost untersagt den Zugriff auf Port 22 von allen externen Hosts über eine `pfctl`-Regel. Dies ist unwahrscheinlich, es sei denn, Sie haben `pfctl` in der Vergangenheit konfiguriert.

12. Wenn Schritt 8 fehlschlägt und **Schritt 10 fehlschlägt**, dann liegt das Problem wahrscheinlich darin, dass der SSH-Serverprozess auf dem Mac nicht ausgeführt wird oder nicht so konfiguriert ist, dass der aktuelle Benutzer sich anmelden kann. Überprüfen Sie in diesem Fall die Einstellungen der Remoteanmeldung aus Schritt 2 erneut, bevor Sie etwas kompliziertere Möglichkeiten untersuchen.

<a name="knownissues" />

### <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

> [!NOTE]
> Dieser Abschnitt gilt nur, wenn Sie bereits eine erfolgreiche Verbindung mit dem Mac-Buildhost mit Ihrem Mac-Benutzernamen und dem Kennwort mithilfe des OpenSSH-SSH-Clients aufgebaut haben. Dies wurde in den Schritten 8 und 9 oben erläutert.

#### <a name="invalid-credentials-please-try-again"></a>„Ungültige Anmeldeinformationen. Versuchen Sie es erneut.“

Bekannte Ursachen:

- **Einschränkung**: Dieser Fehler kann bei dem Versuch auftreten, sich im Buildhost mit dem Konto _Vollständiger Name_ anzumelden, wenn der Name ein Zeichen mit Akzent enthält. Dies ist eine Einschränkung der [SSH.NET-Bibliothek](https://sshnet.codeplex.com/), die Xamarin für die SSH-Verbindung verwendet. **Problemumgehung**: Siehe Schritt 5 oben.

#### <a name="unable-to-authenticate-with-ssh-keys-please-try-to-log-in-with-credentials-first"></a>„Die Authentifizierung kann mit den SSH-Schlüsseln nicht ausgeführt werden. Versuchen Sie zuerst die Anmeldung mit den Anmeldeinformationen"

Bekannte Ursachen:

- **SSH-Sicherheitseinschränkung**: Diese Meldung bedeutet meist, dass eine der Dateien oder eines der Verzeichnisse im vollständig qualifizierten Pfad von **$HOME/.ssh/authorized\_keys** auf dem Mac über aktivierte Schreibberechtigungen für die Elemente _Andere_ oder _Gruppe_ verfügt. **Allgemeine Problemlösung**: Führen Sie `chmod og-w "$HOME"` in einer Eingabeaufforderung des Terminals auf dem Mac aus. Für weitere Informationen dazu, welche bestimmte Datei oder welches Verzeichnis das Problem auslöst, führen Sie `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` im Terminalfenster aus, und öffnen Sie dann die **sshd.log**-Datei von Ihrem Desktop aus, und suchen Sie nach „Authentifizierung abgelehnt: Ungültiger Besitz oder ungültige Modi“.

#### <a name="trying-to-connect-never-completes"></a>„Es wird versucht, eine Verbindung herzustellen...“ wird nicht abgeschlossen

- **Fehler [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**: Dieses Problem kann bei Xamarin 4.1 auftreten, wenn die **Anmelde-Shell** im Kontextmenü **Erweiterte Optionen** für den Mac-Benutzer in **Systemeinstellungen &gt; Benutzer &amp; Gruppen** auf einen anderen Wert als **/bin/bash** festgelegt ist. (Ab Xamarin 4.2 führt dieses Szenario stattdessen zur Fehlermeldung „Verbindung konnte nicht hergestellt werden“.) **Problemumgehung**: Ändern Sie die **Anmelde-Shell** zurück in die ursprüngliche Standardeinstellung **/bin/bash**.

<a name="tryagain" />

#### <a name="couldnt-connect-to-macbuildhostlocal-please-try-again"></a>„Es konnte keine Verbindung mit MacBuildHost.local hergestellt werden. Versuchen Sie es erneut.“

Gemeldete Ursachen:

- **Fehler**: Einigen Benutzern wurde diese Fehlermeldung zusammen mit einem ausführlicheren Fehler („Unerwarteter Fehler beim Konfigurieren von SSH für den Benutzer...Timeout beim Sitzungsvorgang) in den Protokolldateien angezeigt, als sie versucht haben, sich mithilfe von Active Directory oder einem anderen Benutzerkonto einer Verzeichnisdienstdomäne beim Buildhost anzumelden. **Problemumgehung:** Melden Sie sich mit einem lokalen Benutzerkonto beim Buildhost an.

- **Fehler**: Einigen Benutzern wurde dieser Fehler bei einem Verbindungsversuch mit dem Buildhost durch Doppelklicken auf den Namen des Mac-Computers im Verbindungsdialogfeld angezeigt. **Mögliche Problemumgehung**: [Fügen Sie den Mac manuell hinzu](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manually-add-a-mac), indem Sie die IP-Adresse verwenden.

- **Fehler [#35971](https://bugzilla.xamarin.com/show_bug.cgi?id=35971)**: Einigen Benutzern wurde dieser Fehler angezeigt, wenn sie eine drahtlose Netzwerkverbindung zwischen dem Mac-Buildhost und Windows verwendet haben. **Mögliche Problemumgehung**: Verschieben Sie beide Computer in eine verkabelte Netzwerkverbindung.

- **Fehler [#36642](https://bugzilla.xamarin.com/show_bug.cgi?id=36642)**: Diese Meldung wird bei Xamarin 4.0 angezeigt, wenn die **$HOME/.bashrc**-Datei auf dem Mac einen Fehler enthält. (Ab Xamarin 4.1 wirken sich Fehler in der **BASHRC**-Datei nicht mehr auf den Verbindungsprozess aus.) **Problemumgehung**: Verschieben Sie die **BASHRC**-Datei an einen Sicherungsspeicherort (oder löschen Sie sie, wenn Sie wissen, dass Sie sie nicht benötigen).

- **Fehler [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**: Dieser Fehler kann auftreten, wenn die **Anmelde-Shell** im Kontextmenü **Erweiterte Optionen** für den Mac-Benutzer in **Systemeinstellungen > Benutzer und Gruppen** auf einen anderen Wert als **/bin/bash** festgelegt ist. **Problemumgehung**: Ändern Sie die **Anmelde-Shell** zurück in die ursprüngliche Standardeinstellung **/bin/bash**.

- **Einschränkung**: Dieser Fehler kann auftreten, wenn der Mac-Buildhost mit einem Router verbunden ist, der keinen Internetzugriff hat (oder wenn der Mac einen DNS-Server verwendet, bei dem ein Timeout auftritt, wenn nach der Reverse-DNS-Suche des Windows-Computers gefragt wird). Visual Studio benötigt zum Abrufen des SSH-Fingerabdrucks ungefähr 30 Sekunden und kann möglicherweise keine Verbindung herstellen.

    **Mögliche Problemumgehung**: Fügen Sie „UseDNS no“ zur **sshd\_config**-Datei hinzu. Achten Sie darauf, sich über die SSH-Einstellung zu informieren, bevor Sie sie ändern. Informationen finden Sie beispielsweise unter [unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option](http://unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option).

    Die folgenden Schritte beschreiben eine Möglichkeit, die Einstellung zu ändern. Sie müssen mit einem Administratorkonto auf dem Mac angemeldet sein, um diese Schritte durchführen zu können.

    1. Überprüfen Sie den Speicherort der **sshd\_config**-Datei durch Ausführen von `ls /etc/ssh/sshd_config` und `ls /etc/sshd_config` in einer Eingabeaufforderung eines Terminals. Für alle verbleibenden Schritte müssen Sie den Speicherort verwenden, der _nicht_ „Datei oder Verzeichnis nicht vorhanden“ zurückgibt.

        [![](troubleshooting-images/troubleshooting-image18.png "Ausführen von „ls /etc/ssh/sshd_config“ und „ls /etc/sshd_config“ im Terminal")](troubleshooting-images/troubleshooting-image18.png#lightbox)

    3. Führen Sie `cp /etc/ssh/sshd_config "$HOME/Desktop/"` im Terminal aus, um die Datei auf Ihren Desktop zu kopieren.

    4. Öffnen Sie die Datei von Ihrem Desktop aus in einem Text-Editor. Sie können `open -a TextEdit "$HOME/Desktop/sshd_config"` z.B. im Terminal ausführen.

    5. Fügen Sie die folgende Zeile am Ende der Datei hinzu:

        ```
        UseDNS no
        ```
        
    6. Entfernen Sie alle Zeilen mit dem Text `UseDNS yes`, um sicherzustellen, dass die neue Einstellung wirksam ist.

    7. Speichern Sie die Datei.

    8. Führen Sie `sudo cp "$HOME/Desktop/sshd_config" /etc/ssh/sshd_config` im Terminal aus, um die bearbeitete Datei zurück zu kopieren. Geben Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.

    9. Deaktivieren und aktivieren Sie die **Remoteanmeldung** erneut unter **Systemeinstellungen &gt; Freigabe &gt; Remoteanmeldung**, um den SSH-Server neu zu starten.

<a name="clearing" />

#### <a name="clearing-the-broker-idb-build-and-designer-agents-on-the-mac"></a>Deaktivieren des Brokers, IDB, Builds und Designer-Agents auf dem Mac

Wenn die Protokolldateien ein Problem während der Schritte „Installieren“, „Hochladen“ oder „Start“ für alle Mac-Agents feststellen, können Sie versuchen, den Cacheordner **XMA** zu löschen, um Visual Studio zu einem Neustart zu zwingen.

1. Geben Sie im Terminal auf dem Mac folgenden Befehl ein:

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```
    
2. Klicken Sie bei gleichzeitigem Drücken von STRG auf den **XMA**-Ordner, und wählen Sie **In den Papierkorb verschieben** aus:

    [![](troubleshooting-images/troubleshooting-image8.png "Verschieben des Ordners „XMA“ in den Papierkorb")](troubleshooting-images/troubleshooting-image8.png#lightbox)

3. Auf Windows gibt es auch einen Zwischenspeicher, dessen Bereinigung hilfreich sein kann. Öffnen Sie eine Befehlseingabeaufforderung als Administrator unter Windows:

    ```
    del %localappdata%\Temp\Xamarin\XMA
    ```
    
### <a name="warning-messages"></a>Warnmeldungen

Dieser Abschnitt beschreibt einige Meldungen, die im Ausgabefenster auftreten können, sowie Protokolle, die in der Regel ignoriert werden können.

#### <a name="there-is-a-mismatch-between-the-installed-xamarinios--and-the-local-xamarinios"></a>„Es besteht ein Konflikt zwischen dem installierten Xamarin.iOS...und dem lokalen Xamarin.iOS“

Solange Sie sich überzeugt haben, dass Mac und Windows über denselben Xamarin-Verteilungskanal verfügen, kann diese Warnung ignoriert werden.

#### <a name="failed-to-execute-ls-usrbinmono-exitstatus1"></a>„Fehler beim Ausführen von 'ls /usr/bin/mono': ExitStatus=1“

Diese Meldung kann ignoriert werden, solange der Mac OS X 10.11 (El Capitan) oder höher ausführt. Diese Meldung ist kein Problem für OS X 10.11, da Xamarin auch **/usr/local/bin/mono** überprüft, also den richtigen Speicherort für `mono` unter OS X 10.11.

#### <a name="bonjour-service-macbuildhost-did-not-respond-with-its-ip-address"></a>„Der Bonjour-Dienst 'MacBuildHost' hat nicht mit seiner IP-Adresse geantwortet.“

Diese Meldung kann ignoriert werden, es sei denn, Sie stellen fest, dass das Verbindungsdialogfeld die IP-Adresse des Mac-Buildhosts nicht anzeigt. Wenn die IP-Adresse in diesem Dialogfeld _fehlt_, können Sie auch weiterhin [den Mac manuell hinzufügen](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manually-add-a-mac).

#### <a name="invalid-user-a-from-101895-and-inputuserauthrequest-invalid-user-a-preauth"></a>„Ungültiger Benutzer a von 10.1.8.95“ und „input\_userauth\_request: invalid user a [preauth]"

Möglicherweise fallen Ihnen diese Meldungen auf, wenn Sie sich **sshd.log** ansehen. Diese Meldungen sind Teil des normalen Verbindungsprozesses. Sie werden angezeigt, da Xamarin den Benutzernamen **a** vorübergehend beim Abrufen des _SSH-Fingerabdrucks_ verwendet.

### <a name="output-window-and-log-files"></a>Ausgabefenster und Protokolldateien

Wenn Visual Studio bei der Herstellung einer Verbindung mit dem Buildhost auf einen Fehler stößt, müssen Sie 2 Speicherorte auf zusätzliche Meldungen überprüfen: das Ausgabefenster und die Protokolldateien.

#### <a name="output-window"></a>Ausgabefenster

Das Ausgabefenster ist der beste Startpunkt. Es zeigt Meldungen über Hauptverbindungsschritte und -fehler an. So zeigen Sie die Xamarin-Meldungen im Ausgabefenster an:

1. Wählen Sie **Ansicht > Ausgabe** über die Menüs aus, oder klicken Sie auf die Registerkarte **Ausgabe**.
2. Wählen Sie in der Dropdownliste **Ausgabe anzeigen von** aus.
3. Wählen Sie **Xamarin** aus.

[![](troubleshooting-images/troubleshooting-image11.png "Wählen Sie „Xamarin“ auf der Registerkarte „Ausgabe“ aus")](troubleshooting-images/troubleshooting-image11.png#lightbox)

#### <a name="log-files"></a>Protokolldateien

Wenn das Ausgabefenster nicht genügend Informationen enthält, um das Problem zu diagnostizieren, werden zuerst die Protokolldateien überprüft. Die Protokolldateien enthalten zusätzliche Diagnosemeldungen, die nicht im Ausgabefenster angezeigt werden. So zeigen Sie die Protokolldateien an:

1. Starten Sie Visual Studio.

    > [!IMPORTANT]
    > Beachten Sie, dass **.svclogs** nicht standardmäßig aktiviert sind. Starten Sie Visual Studio wie im Handbuch [Versionsprotokolle](~/cross-platform/troubleshooting/questions/version-logs.md#visual-studio-startup-verbose-logs) beschrieben mit ausführlichen Protokollen, um darauf zuzugreifen. Weitere Informationen finden Sie im Blog [Troubleshooting Extensions with the Activity Log (Problembehandlung bei Erweiterungen mit Aktivitätsprotokoll)](https://blogs.msdn.microsoft.com/visualstudio/2010/02/24/troubleshooting-extensions-with-the-activity-log/).

2. Versuchen Sie, eine Verbindung mit dem Buildhost herzustellen.

3. Nachdem Visual Studio einen Verbindungsfehler aufweist, sammeln Sie die Protokolle von **Hilfe > Xamarin > Zip-Protokolle**:

    [![](troubleshooting-images/troubleshooting-image12.png "Sammeln Sie die Protokolle aus „Hilfe > Xamarin > Protokolle zippen“")](troubleshooting-images/troubleshooting-image12.png#lightbox)

4. Wenn Sie die ZIP-Datei öffnen, wird Ihnen eine Liste der Dateien angezeigt, die dem folgenden Beispiel ähneln. Bei Verbindungsfehlern sind die wichtigsten Dateien **\*Ide.log** und **\*Ide.svclog**. Diese Dateien enthalten die gleichen Meldungen in zwei etwas unterschiedlichen Formaten. Die **.svclog** steht im XML-Format zur Verfügung und ist nützlich, wenn Sie die Nachrichten durchsuchen möchten. Bei **.log** handelt es sich um Klartext. Dies ist nützlich, wenn Nachrichten mithilfe von Befehlszeilentools gefiltert werden sollen.

    Um alle Nachrichten zu durchsuchen, wählen Sie die **.svclog**-Datei aus, und öffnen Sie sie:

    [![](troubleshooting-images/troubleshooting-image13.png "Wählen Sie die SVCLOG-Datei aus")](troubleshooting-images/troubleshooting-image13.png#lightbox)

5. Die **.svclog**-Datei wird in **Microsoft Service Trace Viewer** geöffnet. Sie können die Nachrichten nach Threads durchsuchen, um verwandte Nachrichtengruppen anzuzeigen. Zum Durchsuchen nach Threads wählen Sie zuerst die Registerkarte **Graph** aus, klicken dann auf das Dropdownmenü **Layoutmodus** und wählen **Thread** aus:

    [![](troubleshooting-images/troubleshooting-image14.png "Klicken Sie auf das Dropdownmenü „Layoutmodus“, und wählen Sie „Thread“ aus")](troubleshooting-images/troubleshooting-image14.png#lightbox)

<a name="verboselogs" />

#### <a name="verbose-log-files"></a>Ausführliche Protokolldateien

Wenn die normalen Protokolldateien noch keine ausreichenden Informationen zur Problemdiagnose geben, gibt es eine letzte Technik, bei der die ausführliche Protokollierung aktiviert wird. Die ausführlichen Protokolle sind auch auf Problemberichten bevorzugt.

1. Verlassen Sie Visual Studio.

2. Starten Sie eine [**Developer-Eingabeaufforderung**](https://msdn.microsoft.com/library/ms229859(v=vs.110).aspx).

3. Führen Sie den folgenden Befehl in der Befehlszeile zum Starten von Visual Studio mit ausführlicher Protokollierung aus:

    ```bash
    devenv /log
    ```

4. Es wird versucht, eine Verbindung mit dem Buildhost von Visual Studio herzustellen.

5. Nachdem Visual Studio einen Verbindungsfehler aufweist, sammeln Sie die Protokolle aus **Hilfe > Xamarin > Zip-Protokolle**.

6. Führen Sie den folgenden Befehl im Terminal auf dem Mac aus, um alle aktuellen Protokollmeldungen vom SSH-Server in eine Datei auf Ihrem Desktop zu kopieren:

    ```bash
    grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"
   ```

Wenn diese ausführlichen Protokolldateien nicht genügend Informationen zur direkten Problembehebung angeben, [reichen Sie einen neuen Fehlerbericht ein](https://bugzilla.xamarin.com/newbug), und fügen Sie die ZIP-Datei aus Schritt 5 und die Log-Datei aus Schritt 6 hinzu.

## <a name="troubleshooting-automatic-mac-provisioning"></a>Problembehandlung bei der automatischen Bereitstellung eines Macs

### <a name="ide-log-files"></a>IDE-Protokolldateien

Wenn bei der [automatischen Bereitstellung eines Macs](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) Probleme auftreten, können Sie sich die Protokolle der Visual Studio 2017-IDE ansehen, die unter **%LOCALAPPDATA%\Xamarin\Logs\15.0** gespeichert werden.

## <a name="troubleshooting-build-and-deployment-errors"></a>Problembehandlung von Build- und Bereitstellungsfehlern

Dieser Abschnitt enthält einige Probleme, die auftreten können, wenn Visual Studio eine erfolgreiche Verbindung mit dem Buildhost herstellt.

### <a name="unable-to-connect-to-address1921681222-with-usermacuser"></a>„Es kann keine Verbindung mit Address='192.168.1.2:22' und User='macuser' hergestellt werden.“

Bekannte Ursachen:

- **Xamarin 4.1-Sicherheitsfunktion**: Dieser Fehler _wird_ auftreten, wenn Sie auf Xamarin 4.0 herabstufen, nachdem Sie Xamarin 4.1 oder höher verwendet haben. In diesem Fall wird der Fehler von der zusätzlichen Warnung „Privater Schlüssel ist verschlüsselt, aber die Passphrase ist leer“ begleitet. Diese Veränderung ist aufgrund der neuen Sicherheitsfunktion in Xamarin 4.1 _beabsichtigt_. **Empfohlene Lösung**: Löschen Sie **id\_rsa** und **id\_rsa.pub** aus **%LOCALAPPDATA%\Xamarin\MonoTouch**, und stellen Sie dann eine Verbindung mit dem Mac-Buildhost her.

- **SSH-Sicherheitseinschränkung** – Wenn diese Meldung von der zusätzlichen Warnung „Konnte den Benutzer mithilfe des vorhandenen SSH-Schlüssels nicht authentifizieren“ begleitet wird, bedeutet dies meistens, dass eine der Dateien oder Verzeichnisse im vollständig qualifizierten Pfad von **$HOME/.ssh/authorized\_keys** auf dem Mac über aktivierte Schreibberechtigungen für die Elemente _Andere_ oder _Gruppe_ verfügt. **Allgemeine Problemlösung**: Führen Sie `chmod og-w "$HOME"` in einer Eingabeaufforderung des Terminals auf dem Mac aus. Für weitere Informationen dazu, welche bestimmte Datei oder welches Verzeichnis das Problem auslöst, führen Sie `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` im Terminalfenster aus, und öffnen Sie dann die **sshd.log**-Datei von Ihrem Desktop aus, und suchen Sie nach „Authentifizierung abgelehnt: Ungültiger Besitz oder ungültige Modi“.

### <a name="solutions-cannot-be-loaded-from-a-network-share"></a>Projektmappen können nicht von einer Netzwerkfreigabe aus geladen werden

Projektmappen werden nur kompiliert, wenn sie sich auf dem lokalen Windows-Dateisystem oder einem zugeordneten Laufwerk befinden.

Projektmappen, die in einer Netzwerkfreigabe gespeichert sind, können Fehler auslösen oder die Kompilierung vollständig verweigern. Alle **SLN**-Dateien, die in Visual Studio verwendet werden, sollten auf dem lokalen Windows-Dateisystem gespeichert werden.

Aufgrund dieses Problems wird der folgende Fehler ausgelöst:

```bash
error : Building from a network share path is not supported at the moment. Please map a network drive to '\\SharedSources\HelloWorld\HelloWorld' or copy the source to a local directory.
```

Verwandter Fehler: [#36195](https://bugzilla.xamarin.com/show_bug.cgi?id=36195)

### <a name="missing-provisioning-profiles-or-failed-to-create-the-a-fat-library-error"></a>Fehlende Bereitstellungsprofile oder der Fehler „Fehler beim Erstellen der fat-Bibliothek“

Starten Sie Xcode auf dem Mac, und stellen Sie sicher, dass Ihr Apple-Entwicklerkonto angemeldet und Ihr iOS-Entwicklungsprofil heruntergeladen ist:

[![](troubleshooting-images/troubleshooting-image7.png "Stellen Sie sicher, dass Ihr Apple-Entwicklerkonto angemeldet und das iOS-Entwicklungsprofil heruntergeladen ist")](troubleshooting-images/troubleshooting-image7.png#lightbox)

### <a name="a-socket-operation-was-attempted-to-an-unreachable-network"></a>„Ein Socketvorgang für ein nicht erreichbares Netzwerk wurde versucht“

Gemeldete Ursachen:

- **Erweiterung [#36118](https://bugzilla.xamarin.com/show_bug.cgi?id=36118)**: Dieser Fehler kann erfolgreiche Builds verhindern, wenn Visual Studio eine IPv6-Adresse für die Verbindung mit dem Buildhost verwendet. (Die Buildhostverbindung unterstützt noch keine IPv6-Adressen.)

### <a name="xamarinios-visual-studio-plugin-fails-to-load-after-reinstallation-of-betaalpha-channel"></a>Das Xamarin.iOS Visual Studio-Plug-In kann nach der erneuten Installation des Beta/Alpha-Kanals nicht geladen werden.

Relevanter Fehler [#40781](https://bugzilla.xamarin.com/show_bug.cgi?id=40781).

Dieses Problem kann auftreten, wenn Visual Studio nicht den MEF-Komponentencache aktualisiert. In diesem Fall hilft möglicherweise die Installation dieser Visual Studio-Erweiterung: [https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)

Hierdurch wird der Visual Studio MEF-Komponentencache zum Beheben von Beschädigungen des Caches gelöscht.

<a name="errors" />

### <a name="errors-due-to-existing-build-host-processes-on-the-mac"></a>Fehler aufgrund von vorhandenen Buildhostprozessen auf dem Mac

Das Verhalten der aktuellen aktiven Verbindung kann manchmal von Prozessen von vorherigen Buildhostverbindungen beeinträchtigt werden. Schließen Sie Visual Studio, und führen Sie die folgenden Befehle im Terminal auf dem Mac aus, um nach bestehenden Vorgängen zu suchen:

```bash
ps -A | grep mono
```

[![](troubleshooting-images/troubleshooting-image10.png "Ausführen von Befehlen im Terminal auf dem Mac")](troubleshooting-images/troubleshooting-image10.png#lightbox)

Verwenden Sie zum Beenden der vorhandenen Prozesse den folgenden Befehl:

```bash
killall mono
```

### <a name="clearing-the-mac-build-cache"></a>Löschen des Caches des Mac-Builds

Wenn Sie eine Problembehandlung für einen Build durchführen und sicher stellen möchten, dass das Verhalten nicht auf eine temporäre Builddatei zurückzuführen ist, die auf dem Mac gespeichert ist, können Sie den Cacheordner des Builds löschen.

1. Geben Sie im Terminal auf dem Mac folgenden Befehl ein:

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```

2. Klicken Sie bei gleichzeitigem Drücken von STRG auf den **mtbs**-Ordner, und wählen Sie **In den Papierkorb verschieben** aus:

    [![](troubleshooting-images/troubleshooting-image9.png "Verschieben des Ordners „mtbs“ in den Papierkorb")](troubleshooting-images/troubleshooting-image9.png#lightbox)


## <a name="related-links"></a>Verwandte Links

- [Durchführen einer Kopplung mit einem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Xamarin Mac Build Agent - Xamarin University Lightning Lecture (Xamarin Mac-Build-Agent: Xamarin University-Kurzvorstellung)](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
