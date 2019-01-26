---
title: Verwalten virtueller Geräte mit Android Device Manager
description: In diesem Artikel wird erläutert, wie Sie Android Device Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die physische Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: 5618f15d60a26d2ad3d84ff0e3674936c0c01ca3
ms.sourcegitcommit: 2ee36611ef667affee7d417db947fbb614d75315
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479783"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Verwalten virtueller Geräte mit Android Device Manager

_In diesem Artikel wird erläutert, wie Sie Android Device Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die physische Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein._

Nachdem Sie überprüft haben, dass die Hardwarebeschleunigung aktiviert ist (wie unter [Hardwarebeschleunigung für verbesserte Leistung des Emulators](~/android/get-started/installation/android-emulator/hardware-acceleration.md) beschrieben), besteht der nächste Schritt darin, _Android Device Manager_ (auch als _Xamarin Android Device Manager_ bezeichnet) zu verwenden, um virtuelle Geräte zu erstellen, die Sie zum Testen und Debuggen Ihrer App verwenden können.

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Android Device Manager unter Windows

In diesem Artikel wird erläutert, wie virtuelle Android-Geräte mit Android Device Manager erstellt, dupliziert, angepasst und gestartet werden.

[![Screenshot von Android Device Manager auf der Registerkarte „Geräte“](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Mit dem Android Device Manager können _virtuelle Android-Geräte_ (AVDs) erstellt und konfiguriert werden, die sich im [Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) ausführen lassen.
Jedes virtuelle Android-Gerät stellt eine Emulatorkonfiguration dar, die ein physisches Android-Gerät simuliert. Dadurch wird das Ausführen und Testen Ihrer App mit einer Vielzahl von Konfigurationen ermöglicht, die verschiedene physische Android-Geräte simulieren.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um Android Device Manager verwenden zu können:

- Visual Studio 2017, Version 15.8 oder höher. Visual Studio Community, Professional und Enterprise werden unterstützt.

- Visual Studio-Tools für Xamarin Version 4.9 oder höher.

- Das Android SDK muss installiert sein (siehe [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md)).
  Achten Sie darauf, dass Sie das Android SDK in dessen Standardspeicherort installieren, wenn das SDK noch nicht installiert ist: **C:\\Programme (x86)\\Android\\android-sdk**.

- Die folgenden Pakete müssen (über den [Android SDK-Manager](~/android/get-started/installation/android-sdk.md)) installiert werden: 
    - **Android SDK Tools, Version 26.1.1** oder höher
    - **Android SDK-Plattformtools 27.0.1** oder höher
    - **Android SDK-Buildtools 27.0.3** oder höher 
    - **Android-Emulator 27.2.7** oder höher 

  Bei diesen Paketen sollte wie im folgenden Screenshot dargestellt der Status **Installiert** angezeigt werden:

  [![Installieren von Android SDK Tools](device-manager-images/win/02-sdk-tools-sml.png)](device-manager-images/win/02-sdk-tools.png#lightbox)


## <a name="launching-the-device-manager"></a>Starten des Geräte-Managers

Starten Sie Android Device Manager über das Menü **Extras**, indem Sie auf **Extras > Android > Android Device Manager** klicken:

[![Starten des Geräte-Managers über das Menü „Extras“](device-manager-images/win/03-tools-menu-sml.png)](device-manager-images/win/03-tools-menu.png#lightbox)

Wenn beim Starten folgendes Fehlerdialogfeld angezeigt wird, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Anweisungen zum Beheben dieses Problems:

![Android SDK: Instanzfehler (Dialogfeld)](device-manager-images/win/04-sdk-error.png)


## <a name="main-screen"></a>Hauptbildschirm

Wenn Sie den Android-Geräte-Manager zum ersten Mal starten, wird ein Bildschirm mit allen derzeit konfigurierten virtuellen Geräten angezeigt. Für jedes virtuelle Gerät werden der **Name**, das **Betriebssystem** (Android-Version), der **Prozessor**, der **Arbeitsspeicher** und die **Bildschirmauflösung** angezeigt:

[![Liste der installierten Geräte und deren Parameter](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Wenn Sie ein Gerät aus der Liste auswählen, wird auf der rechten Seite die Schaltfläche **Starten** angezeigt. Sie können auf die Schaltfläche **Starten** klicken, um den Emulator mit diesem virtuellen Gerät zu starten:

[![Schaltfläche „Starten“ für ein Geräteimage](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

Nachdem der Emulator mit dem ausgewählten virtuellen Gerät startet, ändert sich die Schaltfläche **Starten** in die Schaltfläche **Beenden**. Diese können Sie verwenden, um den Emulator anzuhalten:

[![Schaltfläche „Beenden“ für das ausgeführte Gerät](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Neues Gerät

Klicken Sie auf die Schaltfläche **Neu** im oberen rechten Bereich des Bildschirms, um ein neues Gerät zu erstellen:

[![Schaltfläche „Neu“ zum Erstellen eines neuen Geräts](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

Durch das Klicken auf **Neu** wird der Bildschirm **Neues Gerät** angezeigt:

[![Bildschirm „Neues Gerät“ des Geräte-Managers](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Befolgen Sie diese Schritte, um ein neues Gerät im Bildschirm **Neues Gerät** zu konfigurieren:

1. Weisen Sie dem Gerät einen neuen Namen zu. Im folgenden Beispiel lautet der Name des Geräts **Pixel_API_27**:

   [![Benennen des neuen Geräts](device-manager-images/win/10-device-name-sml.png)](device-manager-images/win/10-device-name.png#lightbox)

2. Wählen Sie das zu emulierende physische Gerät aus, indem Sie auf das Pulldownmenü **Basisgerät** klicken:

   [![Auswählen des zu emulierenden physischen Geräts](device-manager-images/win/11-device-menu-sml.png)](device-manager-images/win/11-device-menu.png#lightbox)

3. Wählen Sie für dieses virtuelle Gerät einen Prozessortyp aus, indem Sie auf das Pulldownmenü **Prozessor** klicken. Die beste Leistung erzielen Sie durch die Auswahl von **x86**, da dem Emulator dadurch die Nutzung der [Hardwarebeschleunigung](~/android/get-started/installation/android-emulator/hardware-acceleration.md) ermöglicht wird.
   Zwar ermöglicht auch die Option **x86_64** die Hardwarebeschleunigung, jedoch ist die Ausführung etwas langsamer als **x86** (**x86_64** wird normalerweise zum Testen von 64-Bit-Apps verwendet):

   [![Auswählen des Prozessortyps](device-manager-images/win/12-processor-type-menu-sml.png)](device-manager-images/win/12-processor-type-menu.png#lightbox)

4. Wählen Sie die Android-Version (API-Ebene) über das Pulldownmenü **Betriebssystem** aus. Wählen Sie beispielsweise **Oreo 8.1 – API 27** aus, um ein virtuelles Gerät für die API-Ebene 27 zu erstellen:

   [![Auswählen der Android-Version](device-manager-images/win/13-android-version-w158-sml.png)](device-manager-images/win/13-android-version-w158.png#lightbox)

   Wenn Sie eine Android-API-Ebene auswählen, die noch nicht installiert wurde, zeigt der Geräte-Manager die Meldung **A new device image will be downloaded (Ein neues Geräteimage wird heruntergeladen)** am unteren Rand des Bildschirms an. Während das neue virtuelle Gerät erstellt wird, werden die erforderlichen Dateien heruntergeladen und installiert:

   ![Ein neues Geräteimage wird heruntergeladen](device-manager-images/win/14-automatic-download-w158.png)

5. Wenn Sie Google Play Services-APIs auf Ihrem virtuellen Gerät verwenden möchten, aktivieren Sie die Option **Google APIs**. Aktivieren Sie die Option **Google Play Store**, um die Google Play Store-App einzuschließen:

   [![Auswählen von Google Play Services und Google Play Store](device-manager-images/win/15-google-play-services-sml.png)](device-manager-images/win/15-google-play-services.png#lightbox)

   Beachten Sie, dass Google Play Store-Images nur für einige grundlegende Gerätetypen wie Pixel, Pixel 2, Nexus 5 und Nexus 5X verfügbar sind.

6. Bearbeiten Sie alle Eigenschaften, die Sie ändern müssen. Weitere Informationen zum Ändern von Eigenschaften finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).

7. Fügen Sie sämtliche zusätzliche Eigenschaften hinzu, die Sie explizit festlegen müssen. Auf dem Bildschirm **Neues Gerät** werden nur häufig geänderte Eigenschaften angezeigt. Sie können jedoch unten auf das Pulldownmenü **Eigenschaft hinzufügen** klicken, um zusätzliche Eigenschaften hinzuzufügen:

   [![Pulldownmenü „Eigenschaft hinzufügen“](device-manager-images/win/16-add-property-menu-sml.png)](device-manager-images/win/16-add-property-menu.png#lightbox)

    Sie können auch eine benutzerdefinierte Eigenschaft definieren, indem Sie am Anfang der Eigenschaftenliste auf **Benutzerdefiniert...** klicken.

8. Klicken Sie auf die Schaltfläche **Erstellen** im unteren rechten Bereich, um ein neues Gerät zu erstellen:

   [![Schaltfläche „Erstellen“](device-manager-images/win/17-create-button-sml.png)](device-manager-images/win/17-create-button.png#lightbox)

9. Möglicherweise wird Ihnen der Bildschirm **Zustimmung zur Lizenz** angezeigt. Klicken Sie auf **Zustimmen**, wenn Sie den Lizenzbedingungen zustimmen:

   [![Anzeige „Zustimmung zur Lizenz“](device-manager-images/win/18-license-acceptance-sml.png)](device-manager-images/win/18-license-acceptance.png#lightbox)

10. Der Android Device Manager fügt das neue Gerät zur Liste der installierten virtuellen Geräte hinzu. Dabei zeigt die Statusanzeige **Wird erstellt** an, während das Gerät erstellt wird:

    [![Statusanzeige „Erstellung“](device-manager-images/win/19-creating-the-device-sml.png)](device-manager-images/win/19-creating-the-device.png#lightbox)

11. Wenn der Erstellungsvorgang abgeschlossen ist, ist das neue Gerät startbereit und wird in der Liste der installierten virtuellen Geräte mit der Schaltfläche **Starten** angezeigt:

    [![Das startbereite neu erstellte Gerät](device-manager-images/win/20-created-device-sml.png)](device-manager-images/win/20-created-device.png#lightbox)


### <a name="edit-device"></a>Bearbeiten von Geräten

Wählen Sie zum Bearbeiten eines vorhandenen virtuellen Geräts ein Gerät aus, und klicken Sie auf die Schaltfläche **Bearbeiten**, die sich im oberen rechten Bereich des Bildschirms befindet:

[![Schaltfläche „Bearbeiten“ zum Bearbeiten eines Geräts](device-manager-images/win/21-edit-button-sml.png)](device-manager-images/win/21-edit-button.png#lightbox)

Wenn Sie auf **Bearbeiten** klicken, wird der Geräte-Editor für das ausgewählte virtuelle Gerät gestartet:

[![Bildschirm „Geräte-Editor“](device-manager-images/win/22-device-editor-sml.png)](device-manager-images/win/22-device-editor.png#lightbox)

Auf dem Bildschirm **Device Editor** (Geräte-Editor) werden in der Spalte **Eigenschaften** die Eigenschaften des virtuellen Geräts aufgeführt. Die entsprechenden Werte der Eigenschaften werden in der Spalte **Wert** aufgeführt. Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt.

Bearbeiten Sie zum Ändern einer Eigenschaft den Wert in der Spalte **Wert**.
Im folgenden Screenshot wird beispielsweise die Eigenschaft `hw.lcd.density` von **480** in **240** geändert:

[![Beispiel für das Bearbeiten eines Geräts](device-manager-images/win/23-device-editing-sml.png)](device-manager-images/win/23-device-editing.png#lightbox)

Klicken Sie auf die Schaltfläche **Speichern**, nachdem Sie die erforderlichen Änderungen an der Konfiguration vorgenommen haben.
Weitere Informationen zum Ändern der Eigenschaften von virtuellen Geräten finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).


### <a name="additional-options"></a>Zusätzliche Optionen

Zusätzliche Optionen für die Arbeit mit Geräten sind im Pulldownmenü **Zusätzliche Optionen** (&hellip;) in der oberen rechten Ecke verfügbar:

[![Position des Menüs „Zusätzliche Optionen“](device-manager-images/win/24-overflow-menu-sml.png)](device-manager-images/win/24-overflow-menu.png#lightbox)

Das Menü „Zusätzliche Optionen“ enthält folgende Elemente:

- **Duplizieren und Bearbeiten:** Dupliziert das derzeit ausgewählte Gerät und öffnet dieses im Bildschirm **New Device** mit einem anderen eindeutigen Namen. Wenn Sie beispielsweise **Pixel_API_27** auswählen und auf **Duplizieren und Bearbeiten** klicken, wird ein Zähler an den Namen angefügt:

  [![Bildschirm „Duplizieren und Bearbeiten“](device-manager-images/win/25-dupe-and-edit-sml.png)](device-manager-images/win/25-dupe-and-edit.png#lightbox)

- **Im Explorer anzeigen:** Öffnet ein Fenster des Windows-Explorers in dem Ordner, der die Dateien für das virtuelle Gerät enthält. Wenn Sie beispielsweise **Pixel_API_27** auswählen und auf **Im Explorer anzeigen** klicken, wird ein Fenster wie im folgenden Beispiel geöffnet:

  [![Ergebnis beim Klicken auf „Im Explorer anzeigen“](device-manager-images/win/26-reveal-in-explorer-sml.png)](device-manager-images/win/26-reveal-in-explorer.png#lightbox)

- **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dabei wird die aktuelle [Quick Boot](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)-Momentaufnahme gelöscht (falls vorhanden). Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Auf Werkseinstellungen zurücksetzen**, um die Zurücksetzung zu bestätigen:

  ![Dialogfeld „Auf Werkseinstellungen zurücksetzen“](device-manager-images/win/27-factory-reset.png)

- **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.

  ![Dialogfeld „Gerät löschen“](device-manager-images/win/28-delete-device-w158.png)


::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>Android Device Manager unter macOS

In diesem Artikel wird erläutert, wie virtuelle Android-Geräte mit Android Device Manager erstellt, dupliziert, angepasst und gestartet werden.

[![Screenshot von Android Device Manager auf der Registerkarte „Geräte“](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Dieser Leitfaden gilt nur für Visual Studio für Mac.
Xamarin Studio ist nicht mit Android Device Manager kompatibel.

Mit dem Android Device Manager können *virtuelle Android-Geräte* (AVDs) erstellt und konfiguriert werden, die sich im [Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) ausführen lassen.
Jedes virtuelle Android-Gerät stellt eine Emulatorkonfiguration dar, die ein physisches Android-Gerät simuliert. Dadurch wird das Ausführen und Testen Ihrer App mit einer Vielzahl von Konfigurationen ermöglicht, die verschiedene physische Android-Geräte simulieren.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um Android Device Manager verwenden zu können:

- Visual Studio für Mac 7.6 oder höher

- Das Android SDK muss installiert sein (siehe [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md)).

- Die folgenden Pakete müssen (über den [Android SDK-Manager](~/android/get-started/installation/android-sdk.md)) installiert werden: 
    -  **SDK Tools-Version 26.1.1** oder höher
    -  **Android SDK-Plattformtools 28.0.1** oder höher 
    -  **Android SDK-Buildtools 26.0.3** oder höher

  Bei diesen Paketen sollte wie im folgenden Screenshot dargestellt der Status **Installiert** angezeigt werden:

  [![Installieren von Android SDK Tools](device-manager-images/mac/02-sdk-tools-sml.png)](device-manager-images/mac/02-sdk-tools.png#lightbox)


## <a name="launching-the-device-manager"></a>Starten des Geräte-Managers

Starten Sie Android Device Manager, indem Sie auf **Extras > Android Device Manager** klicken:

[![Starten des Geräte-Managers über das Menü „Extras“](device-manager-images/mac/03-tools-menu-sml.png)](device-manager-images/mac/03-tools-menu.png#lightbox)

Wenn beim Starten folgendes Fehlerdialogfeld angezeigt wird, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Anweisungen zum Beheben dieses Problems:

![Android SDK: Instanzfehler (Dialogfeld)](device-manager-images/mac/04-sdk-instance-error.png)


## <a name="main-screen"></a>Hauptbildschirm

Wenn Sie den Android-Geräte-Manager zum ersten Mal starten, wird ein Bildschirm mit allen derzeit konfigurierten virtuellen Geräten angezeigt. Für jedes virtuelle Gerät werden der **Name**, das **Betriebssystem** (Android-Version), der **Prozessor**, der **Arbeitsspeicher** und die **Bildschirmauflösung** angezeigt:

[![Liste der installierten Geräte und deren Parameter](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Wenn Sie ein Gerät aus der Liste auswählen, wird auf der rechten Seite eine **Wiedergabeschaltfläche** angezeigt. Sie können auf die Schaltfläche **Wiedergeben** klicken, um den Emulator mit diesem virtuellen Gerät zu starten:

[![Schaltfläche „Wiedergabe“ für ein Geräteimage](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

Nachdem der Emulator mit dem ausgewählten virtuellen Gerät startet, ändert sich die Schaltfläche **Wiedergeben** in die Schaltfläche **Beenden**. Diese können Sie verwenden, um den Emulator anzuhalten:

[![Schaltfläche „Beenden“ für das ausgeführte Gerät](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

Wenn Sie den Emulator beenden, werden Sie möglicherweise gefragt, ob Sie den aktuellen Zustand für den nächsten Quick Boot speichern möchten:

![Dialogfeld zum Speichern des aktuellen Zustands für den Quick Boot](device-manager-images/mac/08-save-for-quick-boot-m76.png)

Durch das Speichern des aktuellen Zustands wird der Startvorgang des Emulators beschleunigt, wenn das virtuelle Gerät erneut gestartet wird. Weitere Informationen zum Quick Boot finden Sie unter [Quick Boot](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot).

### <a name="new-device"></a>Neues Gerät

Klicken Sie auf die Schaltfläche **Neues Gerät** im oberen linken Bereich des Bildschirms, um ein neues Gerät zu erstellen:

[![Schaltfläche „Neu“ zum Erstellen eines neuen Geräts](device-manager-images/mac/09-new-button-sml.png)](device-manager-images/mac/09-new-button.png#lightbox)

Durch das Klicken auf **Neues Gerät** wird der Bildschirm **Neues Gerät** angezeigt:

[![Bildschirm „Neues Gerät“ des Geräte-Managers](device-manager-images/mac/10-new-device-editor-sml.png)](device-manager-images/mac/10-new-device-editor.png#lightbox)

Befolgen Sie diese Schritte, um ein neues Gerät im Bildschirm **Neues Gerät** zu konfigurieren:

1. Weisen Sie dem Gerät einen neuen Namen zu. Im folgenden Beispiel lautet der Name des Geräts **Pixel_API_27**:

   [![Benennen des neuen Geräts](device-manager-images/mac/11-device-name-m76-sml.png)](device-manager-images/mac/11-device-name-m76.png#lightbox)

2. Wählen Sie das zu emulierende physische Gerät aus, indem Sie auf das Pulldownmenü **Basisgerät** klicken:

   [![Auswählen des zu emulierenden physischen Geräts](device-manager-images/mac/12-device-menu-m76-sml.png)](device-manager-images/mac/12-device-menu-m76.png#lightbox)

3. Wählen Sie für dieses virtuelle Gerät einen Prozessortyp aus, indem Sie auf das Pulldownmenü **Prozessor** klicken. Die beste Leistung erzielen Sie durch die Auswahl von **x86**, da dem Emulator dadurch die Nutzung der [Hardwarebeschleunigung](~/android/get-started/installation/android-emulator/hardware-acceleration.md) ermöglicht wird.
   Zwar ermöglicht auch die Option **x86_64** die Hardwarebeschleunigung, jedoch ist die Ausführung etwas langsamer als **x86** (**x86_64** wird normalerweise zum Testen von 64-Bit-Apps verwendet):

   [![Auswählen des Prozessortyps](device-manager-images/mac/13-processor-type-menu-m76-sml.png)](device-manager-images/mac/13-processor-type-menu-m76.png#lightbox)

4. Wählen Sie die Android-Version (API-Ebene) über das Pulldownmenü **Betriebssystem** aus. Wählen Sie beispielsweise **Oreo 8.1 – API 27** aus, um ein virtuelles Gerät für die API-Ebene 27 zu erstellen:

   [![Auswählen der Android-Version](device-manager-images/mac/14-android-screenshot-m76-sml.png)](device-manager-images/mac/14-android-screenshot-m76.png#lightbox)

   Wenn Sie eine Android-API-Ebene auswählen, die noch nicht installiert wurde, zeigt der Geräte-Manager die Meldung **A new device image will be downloaded (Ein neues Geräteimage wird heruntergeladen)** am unteren Rand des Bildschirms an. Während das neue virtuelle Gerät erstellt wird, werden die erforderlichen Dateien heruntergeladen und installiert:

   ![Ein neues Geräteimage wird heruntergeladen](device-manager-images/mac/15-automatic-download-m76.png)

5. Wenn Sie Google Play Services-APIs auf Ihrem virtuellen Gerät verwenden möchten, aktivieren Sie die Option **Google APIs**. Aktivieren Sie die Option **Google Play Store**, um die Google Play Store-App einzuschließen:

   [![Auswählen von Google Play Services und Google Play Store](device-manager-images/mac/16-google-play-services-m76-sml.png)](device-manager-images/mac/16-google-play-services-m76.png#lightbox)

   Beachten Sie, dass Google Play Store-Images nur für einige grundlegende Gerätetypen wie Pixel, Pixel 2, Nexus 5 und Nexus 5X verfügbar sind.

6. Bearbeiten Sie alle Eigenschaften, die Sie ändern müssen. Weitere Informationen zum Ändern von Eigenschaften finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).

7. Fügen Sie sämtliche zusätzliche Eigenschaften hinzu, die Sie explizit festlegen müssen. Auf dem Bildschirm **Neues Gerät** werden nur häufig geänderte Eigenschaften angezeigt. Sie können jedoch unten auf das Pulldownmenü **Eigenschaft hinzufügen** klicken, um zusätzliche Eigenschaften hinzuzufügen:

   [![Pulldownmenü „Eigenschaft hinzufügen“](device-manager-images/mac/17-add-property-menu-m76-sml.png)](device-manager-images/mac/17-add-property-menu-m76.png#lightbox)

   Sie können auch eine benutzerdefinierte Eigenschaft definieren, indem Sie am Anfang der Eigenschaftenliste auf **Benutzerdefiniert...** klicken.

8. Klicken Sie auf die Schaltfläche **Erstellen** im unteren rechten Bereich, um ein neues Gerät zu erstellen:

   ![Schaltfläche „Erstellen“](device-manager-images/mac/18-create-button-m76.png)

9. Der Android Device Manager fügt das neue Gerät zur Liste der installierten virtuellen Geräte hinzu. Dabei zeigt die Statusanzeige **Wird erstellt** an, während das Gerät erstellt wird:

   [![Statusanzeige „Erstellung“](device-manager-images/mac/19-creating-the-device-m76-sml.png)](device-manager-images/mac/19-creating-the-device-m76.png#lightbox)

10. Wenn der Erstellungsvorgang abgeschlossen ist, ist das neue Gerät startbereit und wird in der Liste der installierten virtuellen Geräte mit der Schaltfläche **Starten** angezeigt:

    [![Das startbereite neu erstellte Gerät](device-manager-images/mac/20-created-device-m76-sml.png)](device-manager-images/mac/20-created-device-m76.png#lightbox)


### <a name="edit-device"></a>Bearbeiten von Geräten

Klicken Sie auf das Pulldownmenü **Zusätzliche Optionen** (Zahnradsymbol) und dann auf **Bearbeiten**, um ein vorhandenes virtuelles Gerät zu bearbeiten:

[![Menüauswahl „Bearbeiten“ zum Bearbeiten eines neuen Geräts](device-manager-images/mac/21-edit-button-m76-sml.png)](device-manager-images/mac/21-edit-button-m76.png#lightbox)

Wenn Sie auf **Bearbeiten** klicken, wird der Geräte-Editor für das ausgewählte virtuelle Gerät gestartet:

[![Bildschirm „Geräte-Editor“](device-manager-images/mac/22-device-editor-sml.png)](device-manager-images/mac/22-device-editor.png#lightbox)

Auf dem Bildschirm **Device Editor** (Geräte-Editor) werden in der Spalte **Eigenschaften** die Eigenschaften des virtuellen Geräts aufgeführt. Die entsprechenden Werte der Eigenschaften werden in der Spalte **Wert** aufgeführt. Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt.

Bearbeiten Sie zum Ändern einer Eigenschaft den Wert in der Spalte **Wert**.
Im folgenden Screenshot wird beispielsweise die Eigenschaft `hw.lcd.density` von **480** in **240** geändert:

[![Beispiel für das Bearbeiten eines Geräts](device-manager-images/mac/23-device-editing-sml.png)](device-manager-images/mac/23-device-editing.png#lightbox)

Klicken Sie auf die Schaltfläche **Speichern**, nachdem Sie die erforderlichen Änderungen an der Konfiguration vorgenommen haben.
Weitere Informationen zum Ändern der Eigenschaften von virtuellen Geräten finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).


### <a name="additional-options"></a>Zusätzliche Optionen

Zusätzliche Optionen für die Arbeit mit einem Gerät sind in dem Pulldownmenü verfügbar, das sich auf der linken Seite der Schaltfläche **Wiedergeben** befindet:

[![Position des Menüs „Zusätzliche Optionen“](device-manager-images/mac/24-overflow-menu-sml.png)](device-manager-images/mac/24-overflow-menu.png#lightbox)

Das Menü „Zusätzliche Optionen“ enthält folgende Elemente:

- **Bearbeiten:** Öffnet das derzeit ausgewählte Gerät wie zuvor beschrieben im Geräte-Editor.

- **Duplizieren und Bearbeiten:** Dupliziert das derzeit ausgewählte Gerät und öffnet dieses im Bildschirm **New Device** mit einem anderen eindeutigen Namen. Wenn Sie beispielsweise **Pixel 2 API 28** auswählen und auf **Duplizieren und Bearbeiten** klicken, wird ein Zähler an den Namen angefügt:

  [![Bildschirm „Duplizieren und Bearbeiten“](device-manager-images/mac/25-dupe-and-edit-sml.png)](device-manager-images/mac/25-dupe-and-edit.png#lightbox)

- **Im Finder zeigen:** Öffnet ein Fenster des macOS-Finders in dem Ordner, der die Dateien für das virtuelle Gerät enthält. Wenn Sie beispielsweise **Pixel 2 API 28** auswählen und auf **Im Finder anzeigen** klicken, wird ein Fenster wie im folgenden Beispiel geöffnet:

  [![Ergebnis vom Klicken auf „Im Finder anzeigen“](device-manager-images/mac/26-reveal-in-finder-sml.png)](device-manager-images/mac/26-reveal-in-finder.png#lightbox)

- **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dabei wird die aktuelle [Quick Boot](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)-Momentaufnahme gelöscht (falls vorhanden). Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Auf Werkseinstellungen zurücksetzen**, um die Zurücksetzung zu bestätigen.

  ![Dialogfeld „Auf Werkseinstellungen zurücksetzen“](device-manager-images/mac/27-factory-reset-m76.png)

- **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.

  ![Dialogfeld „Gerät löschen“](device-manager-images/mac/28-delete-device-m76.png)

-----


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Problembehandlung

In den folgenden Abschnitten wird erläutert, wie Sie Probleme diagnostizieren und umgehen, die auftreten können, wenn Sie den Android Device Manager zum Konfigurieren virtueller Geräte verwenden.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="android-sdk-in-non-standard-location"></a>Speicherort von Android SDK weicht vom Standard ab

Üblicherweise wird Android SDK an folgendem Speicherort installiert:

**C:\\Programme (x86)\\Android\\android-sdk**

Wenn das SDK nicht an diesem Speicherort installiert wurde, wird Ihnen beim Starten des Android Device Managers möglicherweise diese Fehlermeldung angezeigt:

![Android SDK: Instanzfehler](device-manager-images/win/29-sdk-error.png)

Führen Sie die folgenden Schritte aus, um dieses Problem zu umgehen:

1. Navigieren Sie vom Windows-Desktop zu **C:\\Benutzer\\*Benutzername*\\AppData\\Roaming\\XamarinDeviceManager**:

   ![Speicherort der Protokolldatei von Android Device Manager](device-manager-images/win/30-log-files.png)

2. Doppelklicken Sie auf eine der Protokolldateien, um diese zu öffnen und den **Konfigurationsdateipfad** zu suchen. Beispiel:

   [![Konfigurationsdateipfad in der Protokolldatei](device-manager-images/win/31-config-file-path-sml.png)](device-manager-images/win/31-config-file-path.png#lightbox)

3. Navigieren Sie zu diesem Speicherort, und doppelklicken Sie auf **user.config**, um die Datei zu öffnen.

4. Suchen Sie in Konfigurationsdatei **user.config** das Element `<UserSettings>`, und fügen Sie diesem das Attribut **AndroidSdkPath** hinzu. Legen Sie dieses Attribut auf den Pfad fest, unter dem Android SDK auf Ihrem Computer installiert wurde, und speichern Sie die Datei. `<UserSettings>` würde beispielsweise folgendermaßen aussehen, wenn Android SDK unter **C:\\Programme\\Android\\SDK** installiert wäre:

   ```xml
   <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:ProgramsAndroidSDK" />
   ```

Nachdem Sie diese Änderung an **user.config** vorgenommen haben, sollten Sie den Android Device Manager starten können.


### <a name="wrong-version-of-android-sdk-tools"></a>Falsche Version von Android SDK Tools

Wenn Android SDK Tools 26.1.1 oder höher nicht installiert ist, wird beim Starten möglicherweise folgendes Fehlerdialogfeld angezeigt:

![Android SDK: Instanzfehler (Dialogfeld)](device-manager-images/win/32-sdk-instance-error.png)

Wenn Ihnen dieses Fehlerdialogfeld angezeigt wird, klicken Sie auf **SDK-Manager öffnen**, um den Android SDK-Manager zu öffnen. Klicken Sie im Android SDK-Manager auf die Registerkarte **Tools**, und installieren Sie folgende Pakete:

- **Android SDK Tools 26.1.1** oder höher
- **Android SDK-Plattformtools 27.0.1** oder höher
- **Android SDK-Buildtools 27.0.3** oder höher


### <a name="snapshot-disables-wifi-on-android-oreo"></a>Momentaufnahme deaktiviert WLAN unter Android Oreo

Wenn Sie ein virtuelles Android-Gerät für Android Oreo mit simuliertem WLAN-Zugriff konfiguriert haben, verursacht ein Neustart des virtuellen Android-Geräts nach einer Momentaufnahme eventuell eine Deaktivierung des WLAN-Zugriffs.

Sie können Folgendes tun, um dieses Problem zu umgehen:

1. Wählen Sie das virtuelle Android-Gerät im Android Device Manager aus.

2. Klicken Sie im Menü „Zusätzliche Optionen“ auf **Im Explorer anzeigen**.

3. Navigieren Sie zu **Momentaufnahme > default_boot**.

4. Löschen Sie die Datei **snapshot.pb**:

   ![Speicherort der Datei „snapshot.pb“](device-manager-images/win/33-delete-snapshot.png)

5. Starten Sie das virtuelle Android-Gerät neu.

Nach diesen Veränderungen startet das virtuelle Android-Gerät neu in einem Zustand, in dem das WLAN wieder funktioniert.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

### <a name="wrong-version-of-android-sdk-tools"></a>Falsche Version von Android SDK Tools

Wenn Android SDK Tools 26.1.1 oder höher nicht installiert ist, wird beim Starten möglicherweise folgendes Fehlerdialogfeld angezeigt:

![Android SDK: Instanzfehler (Dialogfeld)](device-manager-images/mac/29-sdk-instance-error.png)

Wenn Ihnen dieses Fehlerdialogfeld angezeigt wird, klicken Sie auf **OK**, um den Android SDK-Manager zu öffnen. Klicken Sie im Android SDK-Manager auf die Registerkarte **Tools**, und installieren Sie folgende Pakete:

- **Android SDK Tools 26.1.1** oder höher
- **Android SDK-Plattformtools 28.0.1** oder höher
- **Android SDK-Buildtools 26.0.3** oder höher

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Momentaufnahme deaktiviert WLAN unter Android Oreo

Wenn Sie ein virtuelles Android-Gerät für Android Oreo mit simuliertem WLAN-Zugriff konfiguriert haben, verursacht ein Neustart des virtuellen Android-Geräts nach einer Momentaufnahme eventuell eine Deaktivierung des WLAN-Zugriffs.

Sie können Folgendes tun, um dieses Problem zu umgehen:

1. Wählen Sie das virtuelle Android-Gerät im Android Device Manager aus.

2. Klicken Sie im Menü „Zusätzliche Optionen“ auf **Im Finder zeigen**.

3. Navigieren Sie zu **Momentaufnahme > default_boot**.

4. Löschen Sie die Datei **snapshot.pb**:

   [![Speicherort der snapshot.pb-Datei](device-manager-images/mac/30-delete-snapshot-sml.png)](device-manager-images/mac/30-delete-snapshot.png#lightbox)

5. Starten Sie das virtuelle Android-Gerät neu.

Nach diesen Veränderungen startet das virtuelle Android-Gerät neu in einem Zustand, in dem das WLAN wieder funktioniert.

-----

### <a name="generating-a-bug-report"></a>Erstellen eines Fehlerberichts

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wenn Sie mit dem Android Device Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie einen Rechtsklick auf die Titelleiste ausführen und dann auf **Fehlerbericht erstellen** klicken:

[![Ort des Menüelements zum Erstellen eines Fehlerberichts](device-manager-images/win/34-bug-report-sml.png)](device-manager-images/win/34-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie mit dem Android Device Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie auf **Hilfe > Problem melden** klicken:

[![Ort des Menüelements zum Erstellen eines Fehlerberichts](device-manager-images/mac/31-bug-report-sml.png)](device-manager-images/mac/31-bug-report.png#lightbox)

::: zone-end

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde der Android Device Manager vorgestellt, der in Visual Studio-Tools für Xamarin und Visual Studio für Mac verfügbar ist. Es wurden grundlegende Features wie das Starten und Beenden des Android-Emulators, das Auswählen eines virtuellen Android-Geräts für die Ausführung, das Erstellen von neuen virtuellen Geräten sowie das Bearbeiten von virtuellen Geräten erläutert. Außerdem wurde beschrieben, wie durch die Bearbeitung von Profilhardwareeigenschaften zusätzliche Anpassungen vorgenommen werden können. Des Weiteren wurden Tipps zum Umgang mit häufigen Problemen dargestellt.


## <a name="related-links"></a>Verwandte Links

- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/How-to-Create-and-Manage-Your-Own-Android-Emulators/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
