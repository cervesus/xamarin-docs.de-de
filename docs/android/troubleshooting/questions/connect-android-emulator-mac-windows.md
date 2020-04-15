---
title: Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 49d1eea60f766f4cb61484a6e441506cf8f046ff
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725078"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?

Führen Sie die folgenden Schritte aus, um aus einer Windows-VM eine Verbindung mit dem Android-Emulator herzustellen, der auf einem Mac ausgeführt wird:

1. Starten Sie den Emulator auf dem Mac.

2. Beenden Sie den `adb`-Server auf dem Mac:

    ```bash
    adb kill-server
    ```

3. Beachten Sie, dass der Emulator an 2 TCP-Ports auf der Loopback-Netzwerkschnittstelle lauscht:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Der Port mit der ungeraden Nummer ist der Port, der für die Verbindung mit `adb` verwendet wird. Siehe auch [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4. _Option 1_: Verwenden Sie `nc`, um eingehende TCP-Pakete, die extern an Port 5555 empfangen wurden (oder an einem beliebigen anderen Port Ihrer Wahl), an den Port mit ungerader Nummer auf der Loopbackschnittstelle (**127.0.0.1 5555** in diesem Beispiel) weiterzuleiten und die ausgehenden Pakete umgekehrt zurück weiterzuleiten:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Solange die `nc`-Befehle in einem Terminalfenster ausgeführt werden, werden die Pakete erwartungsgemäß weitergeleitet. Sie können im Terminalfenster STRG-C eingeben, um die Ausführung der `nc`-Befehle zu beenden, sobald Sie die Verwendung des Emulators abgeschlossen haben.

    (Option 1 ist in der Regel einfacher als Option 2, insbesondere dann, wenn **Systemeinstellungen > Sicherheit und Datenschutz > Firewall** aktiviert ist.)

    _Option 2_: Verwenden Sie `pfctl`, um TCP-Pakete von Port `5555` (oder einem beliebigen anderen Port) auf der [gemeinsam genutzten Netzwerkschnittstelle](https://kb.parallels.com/en/4948) an den ungeraden Port auf der Loopbackschnittstelle weiterzuleiten (in diesem Beispiel `127.0.0.1:5555`):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Mit diesem Befehl wird Portweiterleitung mithilfe des Systemdiensts `pf packet filter` eingerichtet. Die Zeilenumbrüche sind wichtig. Achten Sie darauf, dass sie beim Kopieren und Einfügen erhalten bleiben. Wenn Sie Parallels verwenden, müssen Sie außerdem den Schnittstellennamen von *vmnet8* anpassen. `vmnet8` ist der Name des speziellen *NAT-Geräts* für den *gemeinsamen Netzwerkmodus* in VMware Fusion. Die entsprechende Netzwerkschnittstelle in Parallels ist wahrscheinlich [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5. Stellen Sie vom Windows-Computer aus eine Verbindung mit dem Emulator her:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Ersetzen Sie „ip-address-of-the-mac“ durch die IP-Adresse des Macs, wie sie beispielsweise von `ifconfig vmnet8 | grep 'inet '` aufgelistet wird. Ersetzen Sie ggf. `5555` durch den anderen Port Ihrer Wahl aus Schritt 4\. (Hinweis: Eine Möglichkeit, Befehlszeilenzugriff auf `adb` zu erhalten, ist über [**Tools > Android > Android ADB-Eingabeaufforderung**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) in Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Alternatives Verfahren mit `ssh`

Wenn Sie _Entfernte Anmeldung_ auf dem Mac aktiviert haben, können Sie `ssh`-Portweiterleitung verwenden, um eine Verbindung mit dem Emulator herzustellen.

1. Installieren Sie einen SSH-Client unter Windows. Eine Option besteht darin, [Git für Windows](https://git-for-windows.github.io/) zu installieren. Der `ssh`-Befehl steht dann an der **Git Bash**-Eingabeaufforderung zur Verfügung.

2. Führen Sie die Schritte 1–3 oben aus, um den Emulator zu starten, beenden Sie den `adb`-Server auf dem Mac und identifizieren Sie die Emulatorports.

3. Führen Sie `ssh` unter Windows aus, um die bidirektionale Portweiterleitung zwischen einem lokalen Port unter Windows (in diesem Beispiel `localhost:15555`) und dem ungeraden Emulatorport auf der Loopback-Schnittstelle des Macs (in diesem Beispiel `127.0.0.1:5555`) einzurichten:

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Ersetzen Sie `mac-username` durch Ihren Mac-Benutzernamen, wie von `whoami` aufgelistet. Ersetzen Sie `ip-address-of-the-mac` durch die IP-Adresse des Macs.

4. Stellen Sie eine Verbindung mit dem Emulator über den lokalen Port unter Windows her:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Hinweis: Eine komfortable Möglichkeit, Befehlszeilenzugriff auf `adb` zu erhalten, ist über [**Tools > Android > Android ADB-Eingabeaufforderung** in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Eine kleine Mahnung: Wenn Sie Port `5555` als lokalen Port verwenden, nimmt `adb` an, dass der Emulator lokal unter Windows ausgeführt wird. Dies führt zwar nicht zu Problemen in Visual Studio, in Visual Studio für Mac bewirkt es aber das sofortige Schließen der App nach dem Start.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Es wird noch keine alternative Technik unterstützt, die `adb -H` verwendet.

Theoretisch besteht ein anderer Ansatz darin, die integrierte `adb`-Funktion zum Herstellen einer Verbindung mit einem `adb`-Server zu verwenden, der auf einem Remotecomputer ausgeführt wird (siehe z. B. [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)).
Die IDE-Erweiterungen von Xamarin.Android bieten jedoch aktuell keine Möglichkeit, diese Option zu konfigurieren.

## <a name="contact-information"></a>Kontaktinformationen

In diesem Dokument wird das im März 2016 aktuelle Verhalten besprochen. Die in diesem Artikel beschriebene Technik ist kein Teil einer stabilen Testsammlung für Xamarin. Sie könnte daher in Zukunft nicht mehr unterstützt werden.

Wenn Sie feststellen, dass das Verfahren nicht mehr funktioniert oder Ihnen andere Fehler im Dokument auffallen, beteiligen Sie sich an der Diskussion im folgenden Forenthread: [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Danke!
