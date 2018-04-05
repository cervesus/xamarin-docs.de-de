---
title: Ist es möglich, mit der Android-Emulatoren Verbinden von Windows-VM auf einem Mac ausgeführt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 4afe2b9fab8eeebb3f0451379b8e3937cc8d8f55
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Ist es möglich, mit der Android-Emulatoren Verbinden von Windows-VM auf einem Mac ausgeführt?

Zur Verbindung mit einem Android-Emulators für Google aus einem virtuellen Windows-Computer auf einem Mac ausführen, verwenden Sie die folgenden Schritte aus:

1.  Starten des Emulators auf dem Mac

2.  Beenden der `adb` Server auf dem Mac:

    ```bash
    adb kill-server
    ```

3.  Beachten Sie, dass der Emulator 2 TCP-Ports auf der Loopback-Netzwerkschnittstelle hat:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Der Port mit ungerader Nummer ist diejenige, die für die Verbindung verwendet `adb`. Siehe auch [ http://developer.android.com/tools/devices/emulator.html#emulatornetworking ](http://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4.  _Option 1_: Verwendung [ `nc` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html) auf TCP-Pakete weiterleiten eingehender empfangen extern auf Port 5555 (oder einen anderen Anschluss gewünscht) mit dem Port ungerader, auf die Loopback-Schnittstelle (**127.0.0.1 5555** in diesem Beispiel), und zum Weiterleiten der ausgehenden Pakete umgekehrt zurück:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Solange die `nc` Befehle übernachten die Mitarbeiter in ein Terminal-Fenster ausführen, die Pakete werden wie erwartet weitergeleitet werden. Sie können die STRG-C eingeben, in der Terminal-Fenster zum Beenden der `nc` Befehle, sobald Sie fertig sind mit dem Emulator.

    (Option 1 wird in der Regel leichter Option 2, insbesondere wenn **Systemeinstellungen > Sicherheit und Datenschutz > Firewall** eingeschaltet ist.) 

    _Option 2_: Verwendung [ `pfctl` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html) TCP-Pakete von Port umleiten `5555` (oder einem anderen Port aus, die Ihnen gefällt) auf die [Shared Networking](http://kb.parallels.com/en/4948) -Schnittstelle an den Port mit ungerader Nummer für die Loopback-Schnittstelle (`127.0.0.1:5555` in diesem Beispiel):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Mit diesem Befehl richtet portweiterleitung mithilfe der `pf packet filter` -Systemdienst. Die Zeilenumbrüche sind wichtig. Achten Sie darauf, dass sie intakt bleiben, wenn die Kopie einfügen. Sie müssen auch anpassen, der Schnittstellenname aus *vmnet8* bei Verwendung von Parallels. `vmnet8` Der Name der speziellen *NAT-Gerät* für die *Shared Networking* Modus in VMWare Fusion. Die entsprechende Netzwerk-Schnittstelle in Parallels ist wahrscheinlich [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5.  Verbinden Sie mit dem Emulator aus dem Windows-Computer:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Ersetzen Sie "Ip-Adresse-des-der-Mac" mit der IP-Adresse der Mac, z. B. durch aufgelisteten `ifconfig vmnet8 | grep 'inet '`. Ersetzen Sie bei Bedarf `5555` mit den anderen Port, die Sie wie in Schritt 4\. (Hinweis: eine Möglichkeit zum Abrufen von befehlszeilengestützten Zugriff auf die `adb` erfolgt über [ **Tools > Android > Android Adb Eingabeaufforderung** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) in Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Alternative Verfahren verwenden. `ssh`

Wenn Sie aktiviert haben _Remoteanmeldung_ auf dem Mac, dann können Sie `ssh` portweiterleitung für die Verbindung mit dem Emulator.

1.  Installieren Sie einen SSH-Client unter Windows. Eine Möglichkeit besteht darin, installieren Sie [Git für Windows](https://git-for-windows.github.io/). Die `ssh` Befehl werden Sie zur Verfügung, in der **Git Bash** Eingabeaufforderung.

2.  Führen Sie die Schritte 1-3 von oben auf den Emulator starten, beenden die `adb` Server, auf dem Mac, und die Emulator-Ports zu identifizieren.

3.  Führen Sie `ssh` unter Windows zum Einrichten von bidirektionalen portweiterleitung zwischen eines lokalen Ports für Windows (`localhost:15555` in diesem Beispiel) und den Emulator ungerader Port auf dem Mac-Loopback-Schnittstelle (`127.0.0.1:5555` in diesem Beispiel):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Ersetzen Sie `mac-username` mit Ihrem Mac Benutzernamen wie durch die aufgeführte `whoami`. Ersetzen Sie `ip-address-of-the-mac` mit der IP-Adresse der MACS ab.

4.  Eine Verbindung herstellen Sie, um den Emulator über den lokalen Port unter Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Hinweis: eine einfache Möglichkeit für das Abrufen von befehlszeilengestützten Zugriff auf die `adb` erfolgt über [ **Tools > Android > Android Adb Eingabeaufforderung** in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Eine kleine Vorsicht: Wenn Sie Port verwenden `5555` für den lokalen Port `adb` wird möglich ist, dass der Emulator lokal auf dem Windows ausgeführt wird. Dies keine Probleme verursachen, in Visual Studio, jedoch in Visual Studio für Mac wird die app sofort nach dem Start beendet.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Alternative Methode verwenden `adb -H` wird noch nicht unterstützt

Ein anderer Ansatz bestünde in der Theorie verwenden `adb`des integrierten Funktionen für die Verbindung ein `adb` Server auf einem Remotecomputer ausgeführt wird (finden Sie z. B. [ http://stackoverflow.com/a/18551325 ](http://stackoverflow.com/a/18551325)).
Aber die Xamarin.Android IDE-Erweiterungen bieten derzeit keine Möglichkeit, diese Option zu konfigurieren.

## <a name="contact-information"></a>Kontaktinformationen

Dieses Dokument beschreibt das aktuelle Verhalten seit März 2016. Die in diesem Dokument beschriebene Technik ist nicht Teil der Tests stabil Suite für Xamarin, damit es in Zukunft unmöglich.

Wenn Sie feststellen, dass die Technik nicht mehr funktioniert oder wenn Sie feststellen, dass alle anderen Fehler im Dokument, gerne zur Diskussion im folgenden Forum Thread hinzufügen: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm ](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Danke!

