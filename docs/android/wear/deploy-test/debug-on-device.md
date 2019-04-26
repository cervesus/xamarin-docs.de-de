---
title: Debuggen Sie auf einem Wear-Gerät
description: In diesem Artikel wird erläutert, wie Sie das Debuggen einer Xamarin.Android-Wear-Anwendung auf einem Wear-Gerät.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 232fcd1d369eba1daad170986f2e2c4c913a3649
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307862"
---
# <a name="debug-on-a-wear-device"></a>Debuggen Sie auf einem Wear-Gerät

_In diesem Artikel wird erläutert, wie Sie das Debuggen einer Xamarin.Android-Wear-Anwendung auf einem Wear-Gerät._


## <a name="overview"></a>Übersicht

Wenn Sie ein Android Wear-Gerät, z.B. einer Android Wear Smartwatch verfügen, können Sie die app auf dem Gerät anstatt mit einem Emulator ausführen. (Wenn Sie noch nicht vertraut sind, mit dem Bereitstellen und Ausführen von apps für Android Wear, finden Sie unter [Hallo Wear](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Bereiten Sie vor dem Wear-Gerät:

Verwenden Sie die folgenden Schritte aus, um auf dem Android Wear-Gerät Debuggen zu aktivieren:

1.  Öffnen der **Einstellungen** im Menü auf dem Android Wear-Gerät.

2.  Führen Sie einen Bildlauf zum unteren Rand des Menüs und tippen Sie auf **zu**.

3.  Tippen Sie auf die Buildnummer 7-Mal.

4.  Auf der **Einstellungen** Menü, tippen Sie auf **Entwickleroptionen**.

5.  Überprüfen Sie, ob **ADB Debuggen** aktiviert ist.


## <a name="debugging-over-usb"></a>Über USB-Debuggen

Verfügt Ihr Wear-Gerät über einen USB-Anschluss, Sie können verbinden Sie das Wear-Gerät auf Ihrem Computer, auf dieses bereitstellen und Ausführen/Debuggen die app wie würden Sie ein Android-Telefon verwenden (Weitere Informationen finden Sie unter [Debuggen auf einem Gerät](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Das Debugging über Bluetooth

Wenn Ihre Wear-Gerät nicht über einen USB-Anschluss besitzt, können Sie die app mit dem Wear-Gerät über Bluetooth, Routing von der Ausgabe der Anwendung Debuggen auf einem Android-Telefon, das auf Ihrem Computer verbunden ist bereitstellen. 

### <a name="prepare-your-phone"></a>Vorbereiten von Ihrem Telefon

Verwenden Sie die folgenden Schritte aus, um Ihr Smartphone zum Durchführen von Bluetooth-Verbindungen mit dem Wear-Gerät vorbereiten: 

1.  Wenn Sie nicht bereits geschehen, richten Sie Ihr Telefon für die Entwicklung von Xamarin.Android, wie unter [Set Up Device Geräts für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md).

2.  Herunterladen und installieren Sie die kostenlose [Android Wear](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) -app aus dem Google Play Store.

### <a name="connect-the-device"></a>Verbinden des Geräts

Verwenden Sie die folgenden Schritte aus, um Ihr Wear-Gerät an Ihr Telefon zu verbinden:

1.  Auf dem Telefon, fungiert als Bluetooth zwischengeschalteten (oben konfiguriert), die Android Wear-app zu starten. 

2.  Tippen Sie auf die **Einstellungen** Symbol.

3.  Aktivieren Sie **Debuggen über Bluetooth**. Die folgende Status angezeigt, auf dem Bildschirm der Android Wear-app sollte angezeigt werden:

        Host: disconnected
        Target: connected

4.  Verbinden Sie das Telefon über USB mit Ihrem Computer. Geben Sie auf Ihrem Computer die folgenden Befehle aus:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Wenn Port 4444 nicht verfügbar ist, können Sie einen beliebigen anderen verfügbaren Port auf dem Sie Zugriff haben. 

    **Hinweis:** Wenn Sie Visual Studio oder Visual Studio für Mac neu starten, führen Sie diese Befehle erneut aus, um eine Verbindung mit dem Wear-Gerät einzurichten.

5.  Wenn die Wear-Gerät aufgefordert werden, vergewissern Sie sich, dass Sie die erlauben **ADB Debuggen**. In der Android Wear-app sehen Sie den Status zu ändern:

        Host: connected
        Target: connected

6.  Nachdem Sie die oben genannten Schritte abgeschlossen haben, ausführen `adb devices` zeigt den Status des Telefons und das Android Wear-Gerät:

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

An diesem Punkt können Sie Ihre app mit dem Wear-Gerät bereitstellen.

<a name="screenshots" />

### <a name="taking-screenshots"></a>Screenshots erstellen

Sie können einen Screenshot der Wear-Gerät ausführen, mithilfe des folgenden Befehls: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Kopieren Sie im Screenshot auf den Computer mithilfe des folgenden Befehls:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Löschen Sie den Screenshot, auf dem Gerät mithilfe des folgenden Befehls:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Eine app deinstalliert

Sie können eine app aus dem Wear-Gerät deinstallieren, mithilfe des folgenden Befehls:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Beispielsweise, um die app durch den Namen des Pakets entfernt `com.xamarin.weartest`, geben Sie den folgenden Befehl aus:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Weitere Informationen zum Debuggen von Android Wear-Geräte über Bluetooth finden Sie unter [Debuggen über Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Debuggen einer Wear-app mit einer Begleit-Phone-app

Android Wear-apps mit einer Begleit-Android-Telefon-app für die Verteilung auf Google Play verpackt sind (Weitere Informationen finden Sie unter [arbeiten mit der Paketerstellung](~/android/wear/deploy-test/packaging.md)). Allerdings entwickeln Sie weiterhin die Wear-app und eine Begleit-app getrennt. Wenn Sie Ihre app über den Google Play Store freigeben, die Wear-app mit der Companion-app verpackt und automatisch installiert, wenn möglich.

So debuggen Sie die Wear-app mit einer Begleit-app: 

1.  Erstellen und Bereitstellen der Companion-app auf dem Telefon.

2.  Mit der rechten Maustaste in des Wear-Projekts, und legen Sie sie als Standardprojekt starten.

3.  Bereitstellen Sie das Wear-Projekt für das tragbare Gerät.

4.  Führen Sie aus und Debuggen Sie die Wear-app auf dem Gerät.

 
## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie Sie ein Android Wear-Gerät für Wear Debuggen aus Visual Studio über Bluetooth zu konfigurieren und das Debuggen einer Wear-app mit einer Begleit-Phone-app. Sie werden auch allgemeine Tipps zum Debuggen für das Debuggen von einer Wear-Apps per Bluetooth bereitgestellt.
