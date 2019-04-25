---
title: Einrichten eines Geräts für die Entwicklung
description: In diesem Artikel wird beschrieben, wie ein Android-Gerät eingerichtet und mit einem Computer verbunden wird, sodass das Gerät zum Ausführen und Debuggen von Xamarin.Android-Anwendungen verwendet werden kann.
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: d1e43d211f639c422bbed3a6afad9f2136551071
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59690289"
---
# <a name="set-up-device-for-development"></a>Einrichten eines Geräts für die Entwicklung

_In diesem Artikel wird erklärt, wie ein Android-Gerät eingerichtet und mit einem Computer verbunden wird, sodass das Gerät zum Ausführen und Debuggen von Xamarin.Android-Anwendungen verwendet werden kann._

Nach dem Testen auf einem Android-Emulator sollten Sie die Ausführung Ihre Apps auf einem Android-Gerät testen. Im Folgenden sind die Schritte zum Herstellen einer Verbindung zwischen einem Gerät und einem Computer zum Debuggen beschrieben:

1. **Aktivieren des Debuggens auf dem Gerät:** Standardmäßig ist es nicht möglich, Anwendungen auf einem Android-Gerät zu debuggen.

2. **Installieren von USB-Treibern:** Dieser Schritt ist für macOS-Computer nicht erforderlich. Windows-Computer erfordern möglicherweise die Installation von USB-Treibern.

3. **Verbinden des Geräts mit dem Computer:** Der finale Schritt umfasst das Herstellen einer Verbindung des Geräts mit dem Computer, entweder über USB oder WLAN.

Jeder dieser Schritte wird in den Abschnitten unten ausführlicher erläutert.

## <a name="enable-debugging-on-the-device"></a>Aktivieren des Debuggens auf dem Gerät

Es ist möglich, ein beliebiges Android-Gerät zu verwenden, um eine Android-Anwendung zu testen. Das Gerät muss jedoch ordnungsgemäß konfiguriert werden, bevor das Debuggen erfolgen kann. Die notwendigen Schritte unterscheiden sich leicht, je nachdem, welche Android-Version auf Ihrem Gerät installiert ist.

### <a name="android-40-to-android-41"></a>Android 4.0 bis Android 4.1

Bei Android 4.0.x bis Android 4.1.x wird das Debuggen durch die folgenden Schritte aktiviert:

1. Navigieren Sie zum Bildschirm **Einstellungen**.
2. Wählen Sie **Entwickleroptionen** aus.
3. Aktivieren Sie die Option **USB-Debugging**.

Dieser Screenshot zeigt den Bildschirm **Entwickleroptionen** auf einem Gerät an, auf dem Android 4.0.3 ausgeführt wird:

[![Entwickleroptionen](set-up-device-for-development-images/developer-options-sml.png)](set-up-device-for-development-images/developer-options.png#lightbox)

### <a name="android-42-and-higher"></a>Android 4.2 und höher

Ab Android 4.2 und höher sind die **Entwickleroptionen** standardmäßig ausgeblendet. Um diese verfügbar zu machen, navigieren Sie zu **Einstellungen > Telefoninfo**, und tippen Sie sieben mal auf das Element **Buildnummer**, um die Registerkarte **Entwickleroptionen** anzuzeigen:

[![Element „Buildnummer“](set-up-device-for-development-images/about-phone-sml.png)](set-up-device-for-development-images/about-phone.png#lightbox)

Sobald die Registerkarte **Entwickleroptionen** unter **Einstellungen > System** verfügbar sind, öffnen Sie diese, um Entwickleroptionen anzuzeigen:

[![Bildschirm „Entwicklereinstellungen“](set-up-device-for-development-images/developer3.png)](set-up-device-for-development-images/developer3.png#lightbox)

An diesem Ort können Sie Entwickleroptionen wie das USB-Debugging und den „Wach bleiben“-Modus aktivieren.

## <a name="install-usb-drivers"></a>Installieren von USB-Treibern

Dieser Schritt ist für macOS-Geräte nicht erforderlich. Verbinden Sie einfach das Gerät mithilfe eines USB-Kabels mit dem Mac.

Es ist möglicherweise erforderlich, einige zusätzliche Treiber zu installieren, bevor ein Windows-Computer ein über USB verbundenes Android-Gerät erkennt.

> [!NOTE]
> Hierbei handelt es sich um die Schritte zum Einrichten eines Google Nexus-Geräts, die als Referenz angegeben sind. Die Schritte für Ihr bestimmtes Gerät können abweichen, erfolgen jedoch nach einem ähnlichen Muster. Suchen Sie im Internet nach Ihrem Gerät, wenn Sie Probleme haben.

Führen Sie die Anwendung **android.bat** im Verzeichnis **[Installationspfad des Android SDKs]\tools** aus. Das Xamarin.Android-Installationsprogramm legt das Android SDK standardmäßig am folgenden Speicherort auf einem Windows-Computer ab:

`C:\Users\[username]\AppData\Local\Android\android-sdk`

### <a name="download-the-usb-drivers"></a>Herunterladen der USB-Treiber

Google Nexus-Geräte (mit Ausnahme des Galaxy Nexus) erfordern den Google-USB-Treiber. Der Treiber für das Galaxy Nexus wird [von Samsung verteilt](http://www.samsung.com/us/support/downloads/).
Alle anderen Android-Geräte sollten den [USB-Treiber des jeweiligen Herstellers](https://developer.android.com/tools/extras/oem-usb.html#Drivers) verwenden.

Installieren Sie das **Google-USB-Treiberpaket**, indem Sie wie im folgenden Screenshot gezeigt den Android SDK-Manager starten und den Ordner **Extras** erweitern:

[![Google-USB-Treiberpaket ausgewählt](set-up-device-for-development-images/usbdriverpackage.png)](set-up-device-for-development-images/usbdriverpackage.png#lightbox)

Überprüfen Sie das Feld **Google-USB-Treiber**, und klicken Sie auf die Schaltfläche **Installieren**.
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

### <a name="installing-unverified-drivers-in-windows-8"></a>Installieren nicht überprüfter Treiber unter Windows 8

Es können zusätzliche Schritte erforderlich sein, um einen nicht überprüften Treiber unter Windows 8 zu installieren. In den folgenden Schritten wird die Installation der Treiber für ein Galaxy Nexus beschrieben:

1. **Zugreifen auf die erweiterten Startoptionen von Windows 8**: Dieser Schritt umfasst den Neustart des Computers, um auf die erweiterten Startoptionen zuzugreifen. Starten Sie eine Eingabeaufforderung, und starten Sie den Computer mithilfe des folgenden Befehls neu:

    ```command
    shutdown.exe /r /o
    ```

2. **Verbinden des Geräts**: Verbinden Sie das Gerät mit dem Computer

3. **Starten des Geräte-Managers:** Führen Sie **devmgmt.msc** aus. Ihr Gerät sollte mit einem gelben Dreieck darüber aufgelistet sein.

4. **Installieren der Gerätetreiber:** Installieren Sie die Gerätetreiber wie oben beschrieben.

## <a name="connect-the-device-to-the-computer"></a>Verbinden des Geräts mit dem Computer

Der letzte Schritt ist das Verbinden des Geräts mit dem Computer. Hierfür gibt es zwei Möglichkeiten:

- **USB-Kabel**: Dies ist die einfachste und häufigste Methode. Stecken Sie einfach das USB-Kabel in das Gerät und dann in den Computer.

- **WLAN**: Es ist möglich, ein Android-Gerät ohne ein USB-Kabel über WLAN mit einem Computer zu verbinden. Diese Vorgehensweise ist zwar etwas aufwändiger, aber sie kann hilfreich sein, wenn sich das Gerät nicht in unmittelbarer Nähe des Computers befindet und darum eine ständige Verbindung über Kabel eher ungünstig ist. Das Herstellen einer Verbindung über WLAN wird im nächsten Abschnitt beschrieben.

### <a name="connecting-over-wifi"></a>Herstellen einer Verbindung über WLAN

[Android Debug Bridge](https://developer.android.com/tools/help/adb.html) (*ADB*) wird standardmäßig so konfiguriert, dass die Kommunikation mit einem Android-Gerät über USB erfolgt. Es ist möglich, ADB neu zu konfigurieren, damit TCP/IP statt USB verwendet wird. Zu diesem Zweck müssen sich sowohl das Gerät als auch der Computer im gleichen WLAN-Netzwerk befinden. Um Ihre Umgebung für das Debuggen über WLAN einzurichten, führen Sie die folgenden Schritte über die Befehlszeile aus:

1. Bestimmen Sie die IP-Adresse Ihres Android-Geräts. Eine Möglichkeit, die IP-Adresse herauszufinden, liegt darin, unter **Einstellungen > WLAN** nachzusehen und dann auf das WLAN-Netzwerk zu tippen, mit dem das Gerät verbunden ist. Dadurch erscheint ein Bildschirm mit Einstellungen, auf dem wie auf dem nachstehenden Screenshot Informationen zur Netzwerkverbindung angezeigt werden:

    ![IP-Adresse](set-up-device-for-development-images/ip-settings.png)

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

    Sobald dieser Befehl abgeschlossen ist, ist das Android-Gerät über WLAN mit dem Computer verbunden.

Wenn das Debuggen über WLAN abgeschlossen ist, ist es möglich, ADB mithilfe des folgenden Befehls wieder auf den USB-Modus zurückzusetzen:

```command
adb usb
```

Es ist möglich, ADB aufzufordern, die Geräte aufzulisten, die mit dem Computer verbunden sind. Unabhängig davon, wie die Geräte verbunden sind, können Sie an der Eingabeaufforderung den folgenden Befehl ausführen, um die verbundenen Geräte anzuzeigen:

```command
adb devices
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie ein Android-Gerät für die Entwicklung konfiguriert wird, indem das Debuggen auf dem Gerät aktiviert wird. Es wurde zudem beschrieben, wie das Gerät entweder über USB oder über WLAN mit einem Computer verbunden werden kann.

## <a name="related-links"></a>Verwandte Links

- [Android Debug Bridge](https://developer.android.com/tools/help/adb.html)
- [Using Hardware Devices (Verwenden von Hardwaregeräten)](https://developer.android.com/tools/device.html)
- [Samsung Driver Downloads (Samsung-Treiberdownloads)](http://www.samsung.com/us/support/downloads/)
- [OEM USB Drivers (OEM-USB-Treiber)](https://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google-USB-Treiber](https://developer.android.com/sdk/win-usb.html)
- [XDA-Entwickler: Windows 8 – ADB/fastboot driver problem solved (ADB-/Fastboot-Treiberproblem gelöst)](http://forum.xda-developers.com/showthread.php?t=1583801)
