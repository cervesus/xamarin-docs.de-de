---
title: Behandlung von Problemen beim Setup des Emulators
description: In diesem Artikel wird erläutert, wie Sie Probleme diagnostizieren und umgehen, die bei der Verwendung des Android Device Managers auftreten können.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733654"
---
# <a name="troubleshooting-emulator-setup-problems"></a>Behandlung von Problemen beim Setup des Emulators

_In diesem Artikel wird erläutert, wie Sie Probleme diagnostizieren und umgehen, die auftreten können, wenn Sie den Android Device Manager zum Konfigurieren des Android-Emulators verwenden. Informationen zum Beheben von Problemen, während der Android-Emulator ausgeführt wird, finden Sie unter [Google Android Emulator Troubleshooting (Behandeln von Problemen mit dem Google Android-Emulator)](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)._

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>Speicherort von Android SDK weicht vom Standard ab

Üblicherweise wird Android SDK an folgendem Speicherort installiert:

**C:\\Programme (x86)\\Android\\android-sdk**

Wenn das SDK nicht an diesem Speicherort installiert wurde, wird Ihnen beim Starten des Android Device Managers möglicherweise diese Fehlermeldung angezeigt:

![Android SDK: Instanzfehler](troubleshooting-images/win/01-sdk-error.png)

Gehen Sie folgendermaßen vor, um dieses Problem zu umgehen:

1. Navigieren Sie vom Windows-Desktop zu **C:\\Benutzer\\*Benutzername*\\AppData\\Roaming\\XamarinDeviceManager**:

    ![Speicherort der Protokolldatei von Android Device Manager](troubleshooting-images/win/02-log-files.png)

2. Doppelklicken Sie auf eine der Protokolldateien, um diese zu öffnen und den **Konfigurationsdateipfad** zu suchen. Zum Beispiel:

    [![Konfigurationsdateipfad in der Protokolldatei](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. Navigieren Sie zu diesem Speicherort, und doppelklicken Sie auf **user.config**, um die Datei zu öffnen. 

4. Suchen Sie in **user.config** das Element **&lt;UserSettings&gt;**, und fügen Sie diesem das Attribut **AndroidSdkPath** hinzu. Legen Sie dieses Attribut auf den Pfad fest, unter dem Android SDK auf Ihrem Computer installiert wurde, und speichern Sie die Datei. **&lt;UserSettings&gt;** würde beispielsweise folgendermaßen aussehen, wenn Android SDK unter **C:\\Programme\\Android\\SDK** installiert wäre:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Nachdem Sie diese Änderung an **user.config** vorgenommen haben, sollten Sie den Android Device Manager starten können.

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Momentaufnahme deaktiviert WLAN unter Android Oreo

Wenn Sie ein virtuelles Android-Gerät für Android Oreo mit simuliertem WLAN-Zugriff konfiguriert haben, verursacht ein Neustart des virtuellen Android-Geräts nach einer Momentaufnahme eventuell eine Deaktivierung des WLAN-Zugriffs.

Sie können Folgendes tun, um dieses Problem zu umgehen:

1. Wählen Sie das virtuelle Android-Gerät im Android Device Manager aus.

2. Klicken Sie im Menü „Zusätzliche Optionen“ auf **Im Explorer anzeigen**.

3. Navigieren Sie zu **Momentaufnahme > default_boot**.

4. Löschen Sie die Datei **snapshot.pb**:

    ![Speicherort der Datei „snapshot.pb“](troubleshooting-images/win/05-delete-snapshot.png)

5. Starten Sie das virtuelle Android-Gerät neu. 

Nach diesen Veränderungen startet das virtuelle Android-Gerät neu in einem Zustand, in dem das WLAN wieder funktioniert.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Momentaufnahme deaktiviert WLAN unter Android Oreo

Wenn Sie ein virtuelles Android-Gerät für Android Oreo mit simuliertem WLAN-Zugriff konfiguriert haben, verursacht ein Neustart des virtuellen Android-Geräts nach einer Momentaufnahme eventuell eine Deaktivierung des WLAN-Zugriffs.

Sie können Folgendes tun, um dieses Problem zu umgehen:

1. Wählen Sie das virtuelle Android-Gerät im Android Device Manager aus.

2. Klicken Sie im Menü „Zusätzliche Optionen“ auf **Im Finder zeigen**.

3. Navigieren Sie zu **Momentaufnahme > default_boot**.

4. Löschen Sie die Datei **snapshot.pb**:

    [![Speicherort der snapshot.pb-Datei](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. Starten Sie das virtuelle Android-Gerät neu. 

Nach diesen Veränderungen startet das virtuelle Android-Gerät neu in einem Zustand, in dem das WLAN wieder funktioniert.


-----

## <a name="generating-a-bug-report"></a>Erstellen eines Fehlerberichts

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie mit dem Android Device Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie einen Rechtsklick auf die Titelleiste ausführen und dann auf **Fehlerbericht erstellen** klicken:

[![Ort des Menüelements zum Erstellen eines Fehlerberichts](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Sie mit dem Android Device Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie auf **Hilfe > Fehlerbericht erstellen** klicken:

[![Ort des Menüelements zum Erstellen eines Fehlerberichts](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
