---
title: Debuggen Sie auf einem Gerät Abnutzung
description: In diesem Artikel wird erläutert, wie zum Debuggen einer Anwendung Xamarin.Android Abnutzung auf einem Gerät Abnutzung wird.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3f3143dcda4017bbabfbd34a58a40665beea6f75
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="debug-on-a-wear-device"></a>Debuggen Sie auf einem Gerät Abnutzung

_In diesem Artikel wird erläutert, wie zum Debuggen einer Anwendung Xamarin.Android Abnutzung auf einem Gerät Abnutzung wird._


## <a name="overview"></a>Übersicht

Wenn Sie ein Gerät Android Dach, z. B. ein Android Dach Smartwatch verfügen, können Sie die app auf dem Gerät, anstatt einen Emulator ausführen. (Wenn Sie noch nicht vertraut sind, mit der Prozess der Bereitstellung und Ausführung von Dach Android-apps finden Sie unter [Hallo, Abnutzung](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Vorbereiten des Abnutzung-Geräts:

Verwenden Sie die folgenden Schritte aus, um auf dem Gerät Android Dach Debuggen zu aktivieren:

1.  Öffnen der **Einstellungen** Menü auf dem Gerät Android Dach.

2.  Führen Sie einen Bildlauf zum unteren Rand des Menüs und Tap **zu**.

3.  Tippen Sie auf die Buildnummer 7-Mal.

4.  Auf der **Einstellungen** Menü, tippen Sie auf **Entwickleroptionen**.

5.  Überprüfen Sie, ob **ADB Debuggen** aktiviert ist.


## <a name="debugging-over-usb"></a>Über USB-Debuggen

Wenn Ihr Gerät Abnutzung einen USB-Anschluss aufweist, Sie können das Abnutzung-Gerät mit Ihrem Computer verbinden, ihm bereitstellen und Ausführen/Debuggen die app wie bei der Verwendung von Android-Telefon (Weitere Informationen finden Sie unter [Debuggen auf einem Gerät](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Das Debuggen über Bluetooth

Wenn Ihr Gerät Abnutzung nicht mit einen USB-Anschluss verfügt, können Sie die app auf dem Gerät Abnutzung über Bluetooth, von routing Debugausgabe für die app auf einem Android-Telefon, das auf dem Computer verbunden ist bereitstellen. 

### <a name="prepare-your-phone"></a>Vorbereiten von Ihrem Telefon

Verwenden Sie die folgenden Schritte aus, um Ihr Telefon für das Herstellen von Verbindungen von Bluetooth auf dem Gerät Abnutzung vorbereiten: 

1.  Wenn nicht bereits geschehen, richten Sie Ihr Telefon für die Entwicklung von Xamarin.Android, wie in beschrieben [Festlegen von Geräten für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md).

2.  Herunterladen und installieren Sie die kostenlose [Android Dach](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) app aus dem Google Play Store.

### <a name="connect-the-device"></a>Verbinden Sie das Gerät

Verwenden Sie die folgenden Schritte aus, für die Verbindung Ihres Geräts Abnutzung an Ihr Telefon:

1.  Auf dem Telefon, fungieren als Bluetooth zwischengeschalteten (oben konfigurierten), starten Sie die Dach Android-app. 

2.  Tippen Sie auf die **Einstellungen** Symbol.

3.  Aktivieren Sie **Debuggen über Bluetooth**. Die folgenden Status angezeigt, auf dem Bildschirm der Dach Android-app sollte angezeigt werden:

        Host: disconnected
        Target: connected

4.  Verbinden Sie Telefon mit Ihrem Computer über USB. Geben Sie auf Ihrem Computer die folgenden Befehle aus:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Wenn Port 4444 nicht verfügbar ist, können Sie einen beliebigen anderen verfügbaren Port verwenden, auf dem Sie Zugriff haben. 

    **Hinweis**: nach dem Neustart von Visual Studio oder Visual Studio für Mac müssen, führen Sie diese Befehle erneut aus, um eine Verbindung mit dem Gerät Abnutzung einrichten.

5.  Wenn Sie das Gerät Abnutzung aufgefordert werden, vergewissern Sie sich, dass Sie die erlauben **ADB Debuggen**. In der app Android Dach sehen Sie den Status zu ändern:

        Host: connected
        Target: connected

6.  Nachdem Sie die oben genannten Schritte abgeschlossen haben, ausgeführt `adb devices` zeigt den Status des Telefon und das Dach Android-Gerät:

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

An diesem Punkt können Sie Ihre app auf dem Gerät Abnutzung bereitstellen.

<a name="screenshots" />

### <a name="taking-screenshots"></a>Screenshots erstellen

Sie können einen Screenshot des Geräts Abnutzung ergreifen, durch Eingabe des folgenden Befehls: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Kopieren Sie den Screenshot auf Ihren Computer durch Eingabe des folgenden Befehls:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Löschen Sie den Screenshot auf dem Gerät durch Eingabe des folgenden Befehls:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Eine app deinstalliert

Sie können eine app vom Gerät Abnutzung deinstallieren, durch Eingabe des folgenden Befehls:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Z. B. die app mit den Paketnamen `com.xamarin.weartest`, geben Sie den folgenden Befehl aus:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Weitere Informationen zum Debuggen von Android Dach Geräte über Bluetooth, finden Sie unter [Debuggen über Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Debuggen einer app Abnutzung mit einer Begleit-Phone-app

Apps für Android Abnutzung werden verpackt, wobei eine Begleit-Android-Telefon-app für die Verteilung auf Google Play (Weitere Informationen finden Sie unter [arbeiten mit Verpackung](~/android/wear/deploy-test/packaging.md)). Allerdings entwickeln Sie weiterhin die Abnutzung-app und eine Begleit-app getrennt. Wenn Sie Ihre app über Google Play Store veröffentlichen, wird die app Abnutzung verpackt, wobei die Begleit-app und automatisch installiert, wenn möglich.

So debuggen Sie die Abnutzung-app mit einer Begleit-app: 

1.  Erstellen und Bereitstellen der Begleit-app auf dem Telefon.

2.  Mit der rechten Maustaste des Projekts Abnutzung und als Standardprojekt Start festgelegt haben.

3.  Bereitstellen Sie das Projekt Abnutzung auf dem wearable Gerät.

4.  Führen Sie aus und Debuggen Sie die Anwendung Abnutzung auf dem Gerät.

 
## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde erläutert, wie ein Dach Android-Gerät für Abnutzung Debuggen in Visual Studio über Bluetooth konfigurieren und zum Debuggen einer app Abnutzung mit einer Begleit-Phone-app. Sie werden auch allgemeine Tipps zum Debuggen für das Debuggen einer app Abnutzung über Bluetooth bereitgestellt.
