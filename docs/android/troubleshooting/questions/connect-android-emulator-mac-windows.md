---
title: Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 35bfdb92ccfffe54f0ca10dc001d8919703a5bd8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153359"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?

Verwenden Sie zum Verbinden mit dem Android-Emulator auf einem Mac ausführen, von einem virtuellen Windows-Computer die folgenden Schritte aus:

1.  Starten des Emulators auf dem Mac.

2.  Beenden der `adb` Server, auf dem Mac:

    ```bash
    adb kill-server
    ```

3.  Beachten Sie, dass der Emulator 2 TCP-Ports auf der Loopback-Schnittstelle abgehört wird:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Der Port mit ungerader Nummer wird für die Verbindung verwendet `adb`. Siehe auch [ https://developer.android.com/tools/devices/emulator.html#emulatornetworking ](https://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4.  _Option 1_: Verwendung [`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    TCP-Pakete weiterleiten eingehender empfangen extern auf Port 5555 verwendet (oder einen anderen Port gewünscht) auf den Port ungerade der Loopback-Schnittstelle (**127.0.0.1 5555** in diesem Beispiel), und zum Weiterleiten der ausgehenden Pakete an andere Art und Weise sichern:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Solange die `nc` Befehle bleiben Sie in einem Terminalfenster ausführen, werden die Pakete weitergeleitet werden, wie erwartet. Sie können die STRG-C eingeben, in der Terminal-Fenster zum Beenden der `nc` Befehle, sobald Sie fertig sind mit dem Emulator.

    (Option 1 ist in der Regel einfacher, als Option 2, insbesondere dann, wenn **Systemeinstellungen > Sicherheit und Datenschutz > Firewall** aktiviert ist.) 

    _Option 2_: Verwendung [`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    TCP-Pakete vom Port umleiten `5555` (oder einen anderen Port gewünscht) für die [freigegebenes Netzwerk](http://kb.parallels.com/en/4948) Schnittstelle mit dem Port ungerade, auf der Loopback-Schnittstelle (`127.0.0.1:5555` in diesem Beispiel):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Dieser Befehl richtet portweiterleitung mithilfe der `pf packet filter` -Systemdienst. Die Zeilenumbrüche sind wichtig. Achten Sie darauf, dass sie bleiben beim Kopieren und einfügen. Sie müssen auch den Schnittstellennamen von anpassen *vmnet8* bei Verwendung von Parallels. `vmnet8` Der Name der speziellen *NAT-Gerät* für die *freigegebenes Netzwerk* -Modus in VMWare Fusion. Die entsprechende Netzwerkschnittstelle in Parallels ist wahrscheinlich [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5.  Verbinden Sie mit dem Emulator auf dem Windows-Computer:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Ersetzen Sie "-IP-Adresse-von-Mac" mit der IP-Adresse des Mac-Computers, wie von aufgeführt werden, z. B. `ifconfig vmnet8 | grep 'inet '`. Ersetzen Sie bei Bedarf `5555` mit den anderen Port aus, die Sie wie in Schritt 4\. (Hinweis: eine Möglichkeit, um befehlszeilengestützten Zugriff auf die erste `adb` erfolgt über [ **Tools > Android > Android Adb-Eingabeaufforderung** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) in Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Alternative Technik verwenden `ssh`

Wenn Sie aktiviert haben _Remoteanmeldung_ auf dem Mac, dann können Sie `ssh` portweiterleitung für die Verbindung mit dem Emulator.

1.  Installieren Sie einen SSH-Client für Windows. Eine Möglichkeit ist, installieren Sie [Git für Windows](https://git-for-windows.github.io/). Die `ssh` Befehl dann stehen in der **Git Bash** Eingabeaufforderung.

2.  Führen Sie die Schritte 1 bis 3 oben zum Starten des Serveremulators starten, beenden die `adb` Server, auf dem Mac, und identifizieren Sie den Emulator-Ports.

3.  Führen Sie `ssh` für Windows einrichten bidirektionale portweiterleitung zwischen einem lokalen Port auf Windows (`localhost:15555` in diesem Beispiel) und den Emulator ungerade Port auf der Mac Loopback-Schnittstelle (`127.0.0.1:5555` in diesem Beispiel):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Ersetzen Sie dies `mac-username` mit Ihrem Mac-Benutzernamen von aufgeführten `whoami`. Ersetzen Sie dies `ip-address-of-the-mac` mit der IP-Adresse des Mac

4.  Verbinden Sie mit dem Emulator über den lokalen Port für Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Hinweis: eine einfache Möglichkeit, das den befehlszeilengestützten Zugriff auf erhalten `adb` erfolgt über [ **Tools > Android > Android Adb-Eingabeaufforderung** in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Eine kleine Warnung: bei Verwendung von Port `5555` für den lokalen Port, `adb` wird denken, dass der Emulator lokal auf dem Windows ausgeführt wird. Dies keine Probleme verursachen, in Visual Studio, aber in Visual Studio für Mac es bewirkt, dass die app sofort nach dem Start zu beenden.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Alternative Technik `adb -H` wird noch nicht unterstützt

Theoretisch kann ein anderer Ansatz wäre `adb`die integrierte Funktion für die Verbindung ein `adb` Server, die auf einem Remotecomputer ausgeführt wird (finden Sie z. B. [ https://stackoverflow.com/a/18551325 ](https://stackoverflow.com/a/18551325)).
Aber die Xamarin.Android-IDE-Erweiterungen bieten derzeit keine Möglichkeit, diese Option zu konfigurieren.

## <a name="contact-information"></a>Kontaktinformationen

Dieses Dokument erläutert das aktuelle Verhalten seit März 2016. In diesem Dokument beschriebene Technik ist nicht Teil der Tests stabil Suite für Xamarin, damit es in der Zukunft beschädigen könnten.

Wenn Sie feststellen, dass das Verfahren funktioniert nicht mehr, oder wenn Sie feststellen, dass alle anderen Fehler im Dokument, können Sie in der Diskussion im folgenden Forum-Thread hinzufügen: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm ](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Danke!

