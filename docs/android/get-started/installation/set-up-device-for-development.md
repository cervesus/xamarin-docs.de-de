---
title: Einrichten eines Geräts für die Entwicklung
description: In diesem Artikel wird beschrieben, wie ein Android-Gerät eingerichtet und mit einem Computer verbunden wird, sodass das Gerät zum Ausführen und Debuggen von Xamarin.Android-Anwendungen verwendet werden kann.
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 72e0a2adc79796b3df7b6fb4eca62448f1a1a7a4
ms.sourcegitcommit: 997f7b6a1a1bc50b98c3ca5bbc75d6875ba2ae9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510730"
---
# <a name="set-up-device-for-development"></a>Einrichten eines Geräts für die Entwicklung

_In diesem Artikel wird erklärt, wie ein Android-Gerät eingerichtet und mit einem Computer verbunden wird, sodass das Gerät zum Ausführen und Debuggen von Xamarin.Android-Anwendungen verwendet werden kann._

Nach dem Testen auf einem Android-Emulator sollten Sie die Ausführung Ihre Apps auf einem Android-Gerät testen. Dafür müssen Sie das Debuggen aktivieren und das Gerät mit dem Computer verbinden.

Jeder dieser Schritte wird in den Abschnitten unten ausführlicher erläutert.

## <a name="enable-debugging-on-the-device"></a>Aktivieren des Debuggens auf dem Gerät

Ein Gerät muss für das Debuggen aktiviert sein, um darauf eine Android-Anwendung testen zu können. Standardmäßig sind Entwickleroptionen unter Android ab Version 4.2 ausgeblendet. Je nach Android-Version unterscheidet sich die Vorgehensweise zur Aktivierung der Optionen.

### <a name="android-90"></a>Android 9.0 und höher

Bei Android 9.0 und höher wird das Debuggen durch die folgenden Schritte aktiviert:

1. Navigieren Sie zum Bildschirm **Einstellungen**.
2. Klicken Sie auf **Telefoninfo**.
3. Tippen Sie sieben Mal auf den Eintrag **Buildnummer**, bis der Text **Sie sind jetzt Entwickler!** angezeigt wird.

### <a name="android-80-and-android-81"></a>Android 8.0 und Android 8.1

1. Navigieren Sie zum Bildschirm **Einstellungen**.
2. Klicken Sie auf **System**.
3. Klicken Sie auf **Telefoninfo**.
4. Tippen Sie sieben Mal auf den Eintrag **Buildnummer**, bis der Text **Sie sind jetzt Entwickler!** angezeigt wird.

### <a name="android-71-and-lower"></a>Android 7.1 und früher

1. Navigieren Sie zum Bildschirm **Einstellungen**.
2. Klicken Sie auf **Telefoninfo**.
3. Tippen Sie sieben Mal auf den Eintrag **Buildnummer**, bis der Text **Sie sind jetzt Entwickler!** angezeigt wird.

[![Bildschirm zu den Entwickleroptionen unter Android 9.0](set-up-device-for-development-images/build-version-sml.png)](set-up-device-for-development-images/build-version.png#lightbox)

### <a name="verify-that-usb-debugging-is-enabled"></a>Überprüfen, ob USB-Debuggen aktiviert ist

Nachdem Sie den Entwicklermodus auf Ihrem Gerät aktiviert haben, müssen Sie sicherstellen, dass auch das USB-Debuggen aktiviert ist. Auch hier unterscheidet sich die Vorgehensweise je nach Android-Version.

### <a name="android-90"></a>Android 9.0 und höher

Navigieren Sie zu **Einstellungen > System > Erweitert > Entwickleroptionen**, und aktivieren Sie **USB-Debuggen**.

### <a name="android-80-and-android-81"></a>Android 8.0 und Android 8.1

Navigieren Sie zu **Einstellungen > System > Entwickleroptionen**, und aktivieren Sie **USB-Debuggen**.

### <a name="android-71-and-lower"></a>Android 7.1 und früher

Navigieren Sie zu **Einstellungen > Entwickleroptionen**, und aktivieren Sie **USB-Debuggen**.

Sobald die Registerkarte **Entwickleroptionen** unter **Einstellungen > System** verfügbar sind, öffnen Sie diese, um Entwickleroptionen anzuzeigen:

[![Bildschirm zu den Entwickleroptionen unter Android 9.0](set-up-device-for-development-images/usb-debugging-sml.png)](set-up-device-for-development-images/usb-debugging.png#lightbox)

An diesem Ort können Sie Entwickleroptionen wie das USB-Debugging und den „Wach bleiben“-Modus aktivieren.

## <a name="connect-the-device-to-the-computer"></a>Verbinden des Geräts mit dem Computer

Der letzte Schritt ist das Verbinden des Geräts mit dem Computer. Am einfachsten und zuverlässigsten funktioniert dies über USB.

Sie werden auf Ihrem Gerät dazu aufgefordert, dem Computer zu vertrauen (sofern Sie ihn nicht schon zuvor zum Debuggen verwendet haben). Wenn Sie diese Aufforderung nicht bei jeder Verbindung des Geräts erneut erhalten möchten, aktivieren Sie das Kontrollkästchen **Von diesem Computer immer zulassen**.

![](set-up-device-for-development-images/trust-computer-for-usb-debugging.png "Google USB")

## <a name="alternate-connection-via-wifi"></a>Verbinden über WLAN

Alternativ können Sie ein Android-Gerät auch ohne USB-Kabel über WLAN mit einem Computer verbinden. Diese Vorgehensweise ist zwar aufwändiger, kann aber hilfreich sein, wenn sich das Gerät nicht in unmittelbarer Nähe des Computers befindet und daher nicht ständig über Kabel verbunden bleiben kann. 

### <a name="connecting-over-wifi"></a>Herstellen einer Verbindung über WLAN

[Android Debug Bridge](https://developer.android.com/tools/help/adb.html) (*ADB*) wird standardmäßig so konfiguriert, dass die Kommunikation mit einem Android-Gerät über USB erfolgt. Es ist möglich, ADB neu zu konfigurieren, damit TCP/IP statt USB verwendet wird. Zu diesem Zweck müssen sich sowohl das Gerät als auch der Computer im gleichen WLAN-Netzwerk befinden. Führen Sie die folgenden Schritte über die Befehlszeile aus, um Ihre Umgebung für das Debuggen über WLAN einzurichten:

1. Bestimmen Sie die IP-Adresse Ihres Android-Geräts. Eine Möglichkeit zum Ermitteln der IP-Adresse sieht folgendermaßen aus: Navigieren Sie zu **Einstellungen > Netzwerk und Internet > WLAN**, tippen Sie auf das WLAN-Netzwerk, mit dem das Gerät verbunden ist, und anschließend auf **Erweitert**. Dadurch wird ein Dropdownmenü mit Informationen zur Netzwerkverbindung angezeigt, das etwa folgendermaßen aussieht:

    [![IP-Adresse](set-up-device-for-development-images/ip-settings-sml.png)](set-up-device-for-development-images/ip-settings.png#lightbox)

    Bei einigen Android-Versionen wird die IP-Adresse an dieser Stelle nicht angezeigt, sie ist stattdessen aber unter **Einstellungen > Telefoninfo > Status** zu finden.

2. Verbinden Sie Ihr Android-Gerät über USB mit Ihrem Computer.

3. Starten Sie ADB anschließend neu, sodass TCP auf Port 5555 verwendet wird. Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein:

    ```command
    adb tcpip 5555
    ```

    Nachdem dieser Befehl ausgeführt wurde, kann Ihr Computer nicht an Geräten lauschen, die über USB verbunden sind.

4. Trennen Sie die Verbindung über das USB-Kabel zwischen Ihrem Gerät und Ihrem Computer.

5. Konfigurieren Sie ADB so, dass die Verbindung mit Ihrem Android-Gerät über den Port hergestellt wird, der in Schritt 1 oben angegeben wurde:

    ```command
    adb connect 192.168.1.28:5555
    ```

    Nach Abschluss dieses Befehls ist das Android-Gerät über WLAN mit dem Computer verbunden.

    Wenn das Debuggen über WLAN abgeschlossen ist, können Sie ADB mithilfe des folgenden Befehls wieder auf den USB-Modus zurücksetzen:
    
    ```command
    adb usb
    ```
    
    Sie können ADB auffordern, die Geräte aufzulisten, die mit dem Computer verbunden sind. Unabhängig davon, wie die Geräte verbunden sind, können Sie an der Eingabeaufforderung den folgenden Befehl ausführen, um die verbundenen Geräte anzuzeigen:
    
    ```command
    adb devices
    ```

## <a name="troubleshooting"></a>Problembehandlung

Es kann vorkommen, dass sich Ihr Gerät nicht mit dem Computer verbinden lässt. In diesem Fall sollten Sie überprüfen, ob die USB-Treiber installiert sind.

## <a name="install-usb-drivers"></a>Installieren von USB-Treibern

Dieser Schritt ist für macOS-Geräte nicht erforderlich. Verbinden Sie das Gerät einfach über ein USB-Kabel mit dem Mac.

Es ist möglicherweise erforderlich, einige zusätzliche Treiber zu installieren, bevor ein Windows-Computer ein über USB verbundenes Android-Gerät erkennt.

> [!NOTE]
> Hierbei handelt es sich um die Schritte zum Einrichten eines Google Nexus-Geräts, die als Referenz angegeben sind. Die Schritte für Ihr bestimmtes Gerät können abweichen, erfolgen jedoch nach einem ähnlichen Muster. Suchen Sie im Internet nach Ihrem Gerät, wenn Sie Probleme haben.

Führen Sie die Anwendung **android.bat** im Verzeichnis **[Installationspfad des Android SDKs]\tools** aus. Das Xamarin.Android-Installationsprogramm legt das Android SDK standardmäßig am folgenden Speicherort auf einem Windows-Computer ab:

`C:\Users\[username]\AppData\Local\Android\android-sdk`

### <a name="download-the-usb-drivers"></a>Herunterladen der USB-Treiber

Google Nexus-Geräte (mit Ausnahme des Galaxy Nexus) erfordern den Google-USB-Treiber. Der Treiber für das Galaxy Nexus wird [von Samsung verteilt](https://www.samsung.com/us/support/downloads/).
Alle anderen Android-Geräte sollten den [USB-Treiber des jeweiligen Herstellers](https://developer.android.com/tools/extras/oem-usb.html#Drivers) verwenden.

Installieren Sie das **Google-USB-Treiberpaket**, indem Sie wie im folgenden Screenshot gezeigt den Android SDK-Manager starten und den Ordner **Extras** erweitern:

![](set-up-device-for-development-images/google-usb-driver.png "Google USB driver selected")

Aktivieren Sie das Kontrollkästchen **Google-USB-Treiber**, und klicken Sie auf die Schaltfläche **Änderungen übernehmen**.
Die Treiberdateien werden an den folgenden Speicherort heruntergeladen:

`[Android SDK install path]\extras\google\usb\_driver`

Der Standardpfad für eine Xamarin.Android-Installation lautet wie folgt:

`C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver`

### <a name="installing-the-usb-driver"></a>Installieren des USB-Treibers

Nachdem die USB-Treiber heruntergeladen wurden, müssen sie installiert werden.
So installieren Sie die Treiber unter Windows 7:

1. Stellen Sie mithilfe eines USB-Kabels eine Verbindung zwischen Ihrem Gerät und dem Computer her.

2. Klicken Sie auf Ihrem Desktop oder im Windows Explorer mit der rechten Maustaste auf den Computer, und wählen Sie **Verwalten** aus.

3. Wählen Sie im linken Bereich **Geräte** aus.

4. Suchen und erweitern Sie im rechten Bereich **Andere Geräte**.

5. Klicken Sie mit der rechten Maustaste auf den Gerätenamen, und wählen Sie **Treibersoftware aktualisieren** aus.
    Hierdurch wird der Assistent für Hardwareupdates gestartet.

6. Wählen Sie **Auf dem Computer nach Treibersoftware suchen** aus, und klicken Sie auf **Weiter**.

7. Klicken Sie auf **Durchsuchen**, und suchen Sie den USB-Treiber-Ordner. Der Google-USB-Treiber befindet sich unter **[Installationspfad des Android SDK]\extras\google\usb_driver**.

8. Klicken Sie auf **Weiter**, um den Treiber zu installieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie ein Android-Gerät für die Entwicklung konfiguriert wird, indem das Debuggen auf dem Gerät aktiviert wird. Es wurde zudem beschrieben, wie das Gerät entweder über USB oder über WLAN mit einem Computer verbunden werden kann.

## <a name="related-links"></a>Verwandte Links

- [Android Debug Bridge](https://developer.android.com/tools/help/adb.html)
- [Using Hardware Devices (Verwenden von Hardwaregeräten)](https://developer.android.com/tools/device.html)
- [Samsung Driver Downloads (Samsung-Treiberdownloads)](https://www.samsung.com/us/support/downloads/)
- [OEM USB Drivers (OEM-USB-Treiber)](https://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google-USB-Treiber](https://developer.android.com/sdk/win-usb.html)
- [XDA-Entwickler: Windows 8 – ADB/fastboot driver problem solved (ADB-/Fastboot-Treiberproblem gelöst)](https://forum.xda-developers.com/showthread.php?t=1583801)
