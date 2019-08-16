---
title: Debuggen Sie auf einem Wear-Gerät
description: In diesem Artikel wird erläutert, wie eine xamarin. Android-Wear-Anwendung auf einem Wear-Gerät debuggt wird.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2a0c21f5a985e7a0bbe5b2afac1520280a0bd5e8
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522130"
---
# <a name="debug-on-a-wear-device"></a>Debuggen Sie auf einem Wear-Gerät

_In diesem Artikel wird erläutert, wie eine xamarin. Android-Wear-Anwendung auf einem Wear-Gerät debuggt wird._


## <a name="overview"></a>Übersicht

Wenn Sie über ein Android Wear-Gerät wie z. b. eine Android Wear-Smartwatch verfügen, können Sie die APP auf dem Gerät ausführen, anstatt einen Emulator zu verwenden. (Wenn Sie noch nicht mit dem Prozess der Bereitstellung und Ausführung von Android Wear-apps vertraut sind, finden Sie weitere Informationen unter [Hello, Wear](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Bereiten Sie das Wear-Gerät vor:

Führen Sie die folgenden Schritte aus, um das Debugging auf dem Android Wear-Gerät zu aktivieren:

1. Öffnen Sie das Menü " **Einstellungen** " auf dem Android Wear-Gerät.

2. Scrollen Sie zum unteren Rand des Menüs, undTippen Sie auf Info.

3. Tippen Sie auf die Buildnummer 7 Mal.

4. Tippen Sie im Menü " **Einstellungen** " auf " **Entwickler Optionen**".

5. Vergewissern Sie sich, dass **ADB-Debugging** aktiviert ist.


## <a name="debugging-over-usb"></a>Debuggen über USB

Wenn Ihr Wear-Gerät über einen USB-Anschluss verfügt, können Sie das Wear-Gerät mit Ihrem Computer verbinden, es darauf bereitstellen und die APP so ausführen/Debuggen, wie Sie ein Android-Telefon verwenden (Weitere Informationen finden Sie unter [Debuggen auf einem Gerät](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Debuggen über Bluetooth

Wenn Ihr Wear-Gerät keinen USB-Anschluss hat, können Sie die APP auf dem Wear-Gerät über Bluetooth bereitstellen, indem Sie die Debug-Ausgabe der APP an ein Android-Telefon weiterleiten, das mit Ihrem Computer verbunden ist. 

### <a name="prepare-your-phone"></a>Vorbereiten Ihres Telefons

Führen Sie die folgenden Schritte aus, um Ihr Telefon zum Herstellen von Bluetooth-Verbindungen mit dem Wear-Gerät vorzubereiten: 

1. Wenn Sie dies noch nicht getan haben, richten Sie Ihr Telefon für die xamarin. Android-Entwicklung ein, wie unter [Einrichten des Geräts für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md)erläutert.

2. Laden Sie die kostenlose [Android Wear](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) -APP vom Google Play Store herunter, und installieren Sie Sie.

### <a name="connect-the-device"></a>Gerät verbinden

Führen Sie die folgenden Schritte aus, um das Wear-Gerät mit Ihrem Telefon zu verbinden:

1. Starten Sie auf dem Telefon, das als Bluetooth-Vermittler fungiert (oben konfiguriert), die Android Wear-app. 

2. Tippen Sie auf das **Einstellungs** Symbol.

3. Aktivieren Sie das **Debugging über Bluetooth**. Der folgende Status sollte auf dem Bildschirm der Android Wear-App angezeigt werden:

    ```
    Host: disconnected
    Target: connected
    ```

4. Verbinden Sie das Telefon über USB mit Ihrem Computer. Geben Sie auf dem Computer die folgenden Befehle ein:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Wenn Port 4444 nicht verfügbar ist, können Sie einen beliebigen anderen verfügbaren Port verwenden, auf den Sie Zugriff haben. 

    > [!NOTE]
    > Wenn Sie Visual Studio neu starten oder Visual Studio für Mac, müssen Sie diese Befehle erneut ausführen, um eine Verbindung mit dem Wear-Gerät einzurichten.

5. Wenn Sie vom Gerät zum Laden aufgefordert werden, bestätigen Sie, dass Sie das **Debuggen von ADB**zulassen. In der Android Wear-app sollte die Statusänderung angezeigt werden:

    ```
    Host: connected
    Target: connected
    ```

6. Nachdem Sie die obigen Schritte ausgeführt `adb devices` haben, wird in der Status des Telefons und des Android Wear-Geräts angezeigt:

    ```
    List of devices attached
    127.0.0.1:4444    device
    019ad61df0a69399  device
    ```

An diesem Punkt können Sie Ihre APP auf dem Wear-Gerät bereitstellen.

<a name="screenshots" />

### <a name="taking-screenshots"></a>Bildschirmaufnahmen

Sie können einen Screenshot des Wear-Geräts erstellen, indem Sie den folgenden Befehl eingeben: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Kopieren Sie den Screenshot auf Ihren Computer, indem Sie den folgenden Befehl eingeben:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Löschen Sie den Screenshot auf dem Gerät, indem Sie den folgenden Befehl eingeben:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Deinstallieren einer APP

Sie können eine APP über das Wear-Gerät deinstallieren, indem Sie den folgenden Befehl eingeben:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Wenn Sie die APP beispielsweise mit dem Paketnamen `com.xamarin.weartest`entfernen möchten, geben Sie den folgenden Befehl ein:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Weitere Informationen zum Debuggen von Android Wear-Geräten über Bluetooth finden Sie unter [Debuggen über Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Debuggen einer Wear-App mit einer begleitenden Phone-App

Android Wear-apps sind mit einer begleitenden Android Phone-App für die Verteilung auf Google Play verpackt (Weitere Informationen finden Sie unter [Arbeiten mit der Paket](~/android/wear/deploy-test/packaging.md)Erstellung). Allerdings entwickeln Sie die Wear-APP und die zugehörige Begleit-App weiterhin separat. Wenn Sie Ihre APP über das Google Play Store freigeben, wird die Wear-App mit der begleitenden App verpackt und nach Möglichkeit automatisch installiert.

So debuggen Sie die Wear-App mit einer begleitenden App: 

1. Erstellen Sie die Begleit Anwendung, und stellen Sie Sie auf dem Telefon bereit.

2. Klicken Sie mit der rechten Maustaste auf das Projekt Wear, und legen Sie es als Standard Startprojekt fest.

3. Stellen Sie das Wear-Projekt auf dem tragbaren-Gerät bereit.

4. Ausführen und Debuggen der Wear-App auf dem Gerät.

 
## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie ein Android Wear-Gerät für das Debuggen von Visual Studio über Bluetooth und das Debuggen einer Wear-App mit einer begleitenden Phone-App konfigurieren. Außerdem wurden gängige Debug-Tipps zum Debuggen einer Wear-App über Bluetooth bereitgestellt.
