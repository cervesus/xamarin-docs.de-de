---
title: Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 2c1f571efb9ec3fb726912eb1e30496bc51fe26e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026991"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?

Führen Sie die folgenden Schritte aus, um eine Verbindung mit dem Android-Emulator auf einem Mac von einem virtuellen Windows-Computer aus herzustellen:

1. Starten Sie den Emulator auf dem Mac.

2. Beenden Sie den `adb` Server auf dem Mac:

    ```bash
    adb kill-server
    ```

3. Beachten Sie, dass der Emulator an zwei TCP-Ports in der Loopback-Netzwerkschnittstelle lauscht:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Der ungerade nummerierte Port ist der Port, der zum Herstellen einer Verbindung mit `adb`verwendet wird. Siehe auch [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4. _Option 1_: Verwenden von [`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    So leiten Sie eingehende TCP-Pakete, die extern an Port 5555 (oder einem beliebigen anderen Port) empfangen werden, an den ungeraden Port der Loopback Schnittstelle (in diesem Beispiel**127.0.0.1 5555** ) weiter

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Solange die `nc` Befehle in einem Terminal Fenster ausgeführt werden, werden die Pakete erwartungsgemäß weitergeleitet. Sie können im Terminal Fenster Control-C eingeben, um die `nc` Befehle zu beenden, sobald Sie mit dem Emulator fertig sind.

    (Option 1 ist in der Regel einfacher als Option 2, insbesondere dann, wenn **System Einstellungen > Sicherheits & Datenschutz > Firewall** aktiviert ist.) 

    _Option 2_: Verwenden von [`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    zum Umleiten von TCP-Paketen von Port `5555` (oder einem anderen Port, den Sie möchten) auf der frei [gegebenen Netzwerk](https://kb.parallels.com/en/4948) Schnittstelle an den ungeraden Port der Loopback Schnittstelle (`127.0.0.1:5555` in diesem Beispiel):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Mit diesem Befehl wird die Port Weiterleitung mit dem `pf packet filter` Systemdienst festgelegt. Die Zeilenumbrüche sind wichtig. Stellen Sie sicher, dass Sie beim Einfügen von kopieren intakt bleiben. Sie müssen auch den Schnittstellennamen von *vmnet8* anpassen, wenn Sie Parallels verwenden. `vmnet8` ist der Name des speziellen *NAT-Geräts* für den frei *gegebenen Netzwerk* Modus in VMware Fusion. Die entsprechende Netzwerkschnittstelle in Parallels ist wahrscheinlich [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5. Stellen Sie vom Windows-Computer aus eine Verbindung mit dem Emulator her:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Ersetzen Sie "IP-address-of-the-Mac" durch die IP-Adresse des Mac, z. b. durch `ifconfig vmnet8 | grep 'inet '`aufgelistet. Ersetzen Sie bei Bedarf `5555` durch den anderen Port, den Sie aus Schritt 4 erhalten\. (Hinweis: eine Möglichkeit zum Abrufen des Befehlszeilen Zugriffs auf `adb` ist über [**Tools > > Android ADB-Eingabeaufforderung**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) in Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Alternatives Verfahren mit `ssh`

Wenn Sie _entfernte Anmeldung_ auf dem Mac aktiviert haben, können Sie `ssh` Port Weiterleitung verwenden, um eine Verbindung mit dem Emulator herzustellen.

1. Installieren Sie einen SSH-Client unter Windows. Eine Möglichkeit ist die Installation [von git für Windows](https://git-for-windows.github.io/). Der Befehl "`ssh`" ist dann in der **git bash** -Eingabeaufforderung verfügbar.

2. Führen Sie die oben beschriebenen Schritte 1-3 aus, um den Emulator zu starten, den `adb` Server auf dem Mac zu beenden und die emulatorports zu identifizieren.

3. Führen Sie `ssh` unter Windows aus, um die bidirektionale Port Weiterleitung zwischen einem lokalen Port unter Windows (`localhost:15555` in diesem Beispiel) und dem ungeraden emulatorport in der Loopback Schnittstelle des Macs (in diesem Beispiel`127.0.0.1:5555`) einzurichten:

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Ersetzen Sie `mac-username` durch Ihren Mac-Benutzernamen, wie in `whoami`aufgelistet. Ersetzen Sie `ip-address-of-the-mac` durch die IP-Adresse des Macs.

4. Stellen Sie eine Verbindung mit dem Emulator über den lokalen Port unter Windows her:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Hinweis: eine einfache Möglichkeit zum Abrufen des Befehlszeilen Zugriffs auf `adb` finden Sie über [ **Tools > Android-> Android ADB-Eingabeaufforderung** in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Eine kleine Vorsicht: Wenn Sie Port `5555` für den lokalen Port verwenden, ist `adb` der Ansicht, dass der Emulator lokal unter Windows ausgeführt wird. Dies verursacht keine Probleme in Visual Studio, aber in Visual Studio für Mac bewirkt dies, dass die APP sofort nach dem Start beendet wird.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Eine Alternative Technik mit `adb -H` wird noch nicht unterstützt.

Theoretisch besteht ein anderer Ansatz darin, die integrierte Funktion `adb`zum Herstellen einer Verbindung mit einem `adb` Server zu verwenden, der auf einem Remote Computer ausgeführt wird (siehe beispielsweise [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)).
Die xamarin. Android-IDE-Erweiterungen bieten derzeit jedoch keine Möglichkeit, diese Option zu konfigurieren.

## <a name="contact-information"></a>Kontaktinformationen

In diesem Dokument wird das aktuelle Verhalten von März 2016 erläutert. Das in diesem Dokument beschriebene Verfahren ist nicht Teil der stabilen Testsuite für xamarin, sodass es in Zukunft unterbrechen kann.

Wenn Sie bemerken, dass das Verfahren nicht mehr funktioniert, oder wenn Sie andere Fehler im Dokument bemerken, können Sie die Diskussion im folgenden Forums Thread hinzufügen: [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Danke!
