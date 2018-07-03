---
title: Verwalten virtueller Geräte mit Android Device Manager
description: In diesem Artikel wird erläutert, wie Sie Android Device Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die physische Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a7c1aeafd94d7e2639617cda13312ee8a09e2c94
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935327"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Verwalten virtueller Geräte mit Android Device Manager

_In diesem Artikel wird erläutert, wie Sie Android Device Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die physische Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein._

## <a name="overview"></a>Übersicht

Nachdem Sie überprüft haben, dass die Hardwarebeschleunigung aktiviert ist (wie unter [Hardwarebeschleunigung für verbesserte Leistung des Emulators](~/android/get-started/installation/android-emulator/hardware-acceleration.md) beschrieben), besteht der nächste Schritt darin, _Android Device Manager_ (auch als _Xamarin Android Device Manager_ bezeichnet) zu verwenden, um virtuelle Geräte zu erstellen, die Sie zum Testen und Debuggen Ihrer App verwenden können.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Artikel wird erläutert, wie virtuelle Android-Geräte mit Android Device Manager erstellt, dupliziert, angepasst und gestartet werden.

[![Screenshot von Android Device Manager auf der Registerkarte „Geräte“](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Mit dem Android Device Manager können _virtuelle Android-Geräte_ (AVDs) erstellt und konfiguriert werden, die sich im [Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) ausführen lassen.
Jedes virtuelle Android-Gerät stellt eine Emulatorkonfiguration dar, die ein physisches Android-Gerät simuliert. Dadurch wird das Ausführen und Testen Ihrer App mit einer Vielzahl von Konfigurationen ermöglicht, die verschiedene physische Android-Geräte simulieren.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um Android Device Manager verwenden zu können:

-   Visual Studio 2017, Version 15.7 oder höher. Visual Studio Community, Professional und Enterprise werden unterstützt.

-   Visual Studio-Tools für Xamarin Version 4.9 oder höher.

-   Das Android SDK muss installiert sein (siehe [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md)), und Version 26.1.1 oder höher von Android SDK Tools muss (gemäß den Erläuterungen im folgenden Abschnitt) installiert sein. Achten Sie darauf, Android SDK an folgendem Speicherort zu installieren (wenn das SDK nicht bereits installiert ist): **C:\\Programme (x86)\\Android\\android-sdk**.


## <a name="launching-the-device-manager"></a>Starten des Geräte-Managers

Starten Sie Android Device Manager über das Menü **Extras**, indem Sie auf **Extras > Android > Android Device Manager** klicken:

[![Start über das Menü „Extras“](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

Wenn beim Starten das folgende Fehlerdialogfeld angezeigt wird, finden Sie unter [Android-Emulator-Problembehandlung](~/android/get-started/installation/android-emulator/troubleshooting.md) Anweisungen zur Umgehung dieses Problems:

![Android SDK: Instanzfehler](device-manager-images/win/32-sdk-error.png)

Bevor Sie Android Device Manager verwenden können, müssen Sie Version 26.1.1 oder höher von Android SDK Tools installieren. Wenn Android SDK Tools 26.1.1 oder höher nicht installiert ist, wird beim Starten möglicherweise folgendes Fehlerdialogfeld angezeigt:

![Android SDK: Instanzfehler (Dialogfeld)](device-manager-images/win/02-sdk-instance-error.png)

Wenn Ihnen dieses Fehlerdialogfeld angezeigt wird, klicken Sie auf **SDK-Manager öffnen**, um den Android SDK-Manager zu öffnen. Klicken Sie im Android SDK-Manager auf die Registerkarte **Extras**, und installieren Sie Folgendes:

-   **Android SDK Tools 26.1.1** oder höher 
-   **Android SDK-Plattformtools 27.0.1** oder höher  
-   **Android SDK-Buildtools 27.0.3** oder höher

Bei diesen Paketen sollte wie im folgenden Screenshot dargestellt der Status **Installiert** angezeigt werden:

[![Installieren von Android SDK Tools 27.0](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

Nachdem diese Pakete installiert wurden, können Sie den SDK-Manager schließen und den Android-Geräte-Manager erneut starten.

## <a name="main-screen"></a>Hauptbildschirm

Wenn Sie den Android-Geräte-Manager zum ersten Mal starten, wird ein Bildschirm mit allen derzeit konfigurierten virtuellen Geräten angezeigt. Für jedes Gerät werden der **Name**, das **Betriebssystem** (Ebene der Android-API), die **CPU**, die Größe des **Arbeitsspeichers** und die Bildschirmauflösung angezeigt:

[![Liste der installierten Geräte und deren Parameter](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Wenn Sie ein Gerät in der Liste anklicken, wird die Schaltfläche **Starten** auf der rechten Seite angezeigt. Sie können auf die Schaltfläche **Starten** klicken, um den Emulator mit diesem virtuellen Gerät zu starten:

[![Schaltfläche „Starten“ für ein Geräteimage](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

Nachdem der Emulator mit dem ausgewählten virtuellen Gerät startet, ändert sich die Schaltfläche **Starten** in die Schaltfläche **Beenden**. Diese können Sie verwenden, um den Emulator anzuhalten:

[![Schaltfläche „Beenden“ für das ausgeführte Gerät](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Neues Gerät

Klicken Sie auf die Schaltfläche **Neu** im oberen rechten Bereich des Bildschirms, um ein neues Gerät zu erstellen:

[![Schaltfläche „Neu“ zum Erstellen eines neuen Geräts](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

Durch das Klicken auf **Neu** wird der Bildschirm **Neues Gerät** angezeigt:

[![Bildschirm „Neues Gerät“ des Geräte-Managers](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Befolgen Sie diese Schritte, um ein neues Gerät im Bildschirm **Neues Gerät** zu konfigurieren:

1. Wählen Sie das zu emulierende physische Gerät aus, indem Sie auf das Pulldownmenü **Gerät** klicken:

    [![Pulldownmenü des Geräts](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. Wählen Sie ein Systemimage aus, das mit diesem virtuellen Gerät verwendet werden soll, indem Sie auf das Pulldownmenü **System image** (Systemimage) klicken. In diesem Menü werden die installierten Systemimages für Device Manager unter **Installiert** aufgeführt. Im Abschnitt **Herunterladen** werden Systemimages für Device Manager aufgeführt, die derzeit nicht auf Ihrem Entwicklungscomputer verfügbar sind, aber automatisch installiert werden können:

    [![Pulldownmenü „Systemimage“](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. Weisen Sie dem Gerät einen neuen Namen zu. Im folgenden Beispiel lautet der Name des Geräts **Nexus 5 API 25**:

    [![Benennen des neuen Geräts](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. Bearbeiten Sie alle Eigenschaften, die Sie ändern müssen. Weitere Informationen zum Ändern von Eigenschaften finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).

5. Fügen Sie sämtliche zusätzliche Eigenschaften hinzu, die Sie explizit festlegen müssen. Auf dem Bildschirm **Neues Gerät** werden nur häufig geänderte Eigenschaften angezeigt. Sie können jedoch auf das Pulldownmenü **Eigenschaft hinzufügen** im unteren linken Bereich klicken, um zusätzliche Eigenschaften hinzuzufügen. Im folgenden Beispiel wird die `hw.lcd.backlight`-Eigenschaft hinzugefügt:

    [![Pulldownmenü „Eigenschaft hinzufügen“](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. Klicken Sie auf die Schaltfläche **Erstellen** im unteren rechten Bereich, um ein neues Gerät zu erstellen:

    ![Schaltfläche „Erstellen“](device-manager-images/win/14-create-button.png)

7. Möglicherweise wird Ihnen der Bildschirm **Zustimmung zur Lizenz** angezeigt. Klicken Sie auf **Zustimmen**, wenn Sie den Lizenzbedingungen zustimmen:

    ![Bildschirm „Zustimmung zur Lizenz“](device-manager-images/win/15-license-acceptance.png)

8. Der Android Device Manager fügt das neue Gerät zur Liste der installierten virtuellen Geräte hinzu. Dabei zeigt die Statusanzeige **Wird erstellt** an, während das Gerät erstellt wird:

    [![Statusanzeige „Erstellung“](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. Wenn der Erstellungsvorgang abgeschlossen ist, ist das neue Gerät startbereit und wird in der Liste der installierten virtuellen Geräte mit der Schaltfläche **Starten** angezeigt:

   [![Das startbereite neu erstellte Gerät](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>Bearbeiten von Geräten

Wählen Sie zum Bearbeiten eines vorhandenen virtuellen Geräts ein Gerät aus, und klicken Sie auf die Schaltfläche **Bearbeiten**, die sich im oberen rechten Bereich des Bildschirms befindet:

[![Schaltfläche „Bearbeiten“ zum Bearbeiten eines neuen Geräts](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

Wenn Sie auf **Bearbeiten** klicken, wird der Geräte-Editor für das ausgewählte virtuelle Gerät gestartet:

[![Bildschirm „Geräte-Editor“](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

Der Bildschirm **Device Editor** (Geräte-Editor) führt in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt.

Im folgenden Screenshot wird beispielsweise die Eigenschaft `hw.lcd.density` von **420** in **240** geändert:

[![Beispiel für das Bearbeiten eines Geräts](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

Klicken Sie auf die Schaltfläche **Speichern**, nachdem Sie die erforderlichen Änderungen an der Konfiguration vorgenommen haben.
Weitere Informationen zum Ändern der Eigenschaften von virtuellen Geräten finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Zusätzliche Optionen

Zusätzliche Optionen für die Arbeit mit Geräten sind im &hellip;-Menü in der oberen rechten Ecke verfügbar:

[![Position des Menüs „Zusätzliche Optionen“](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

Das Menü „Zusätzliche Optionen“ enthält folgende Elemente:

-   **Duplizieren und Bearbeiten:** Dupliziert das derzeit ausgewählte Gerät und öffnet dieses im Bildschirm **New Device** mit einem anderen eindeutigen Namen. Wenn Sie beispielsweise **VisualStudio_android-23_x86_phone** auswählen und auf **Duplicate and Edit** (Duplizieren und Bearbeiten) klicken, wird ein Zähler an den Namen angefügt:

    [![Bildschirm „Duplizieren und Bearbeiten“](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Im Explorer anzeigen:** Öffnet ein Fenster des Windows-Explorers in dem Ordner, der die Dateien für das virtuelle Gerät enthält. Wenn Sie beispielsweise **Nexus 5X API 25** auswählen und auf **Im Explorer anzeigen** klicken, wird folgendes Fenster geöffnet:

    [![Ergebnis beim Klicken auf „Im Explorer anzeigen“](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dabei wird die aktuelle [Quick Boot](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)-Momentaufnahme gelöscht (falls vorhanden). Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Wipe user data** (Benutzerdaten löschen), um das Zurücksetzen zu bestätigen.

-   **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft.
    Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Artikel wird erläutert, wie virtuelle Android-Geräte mit Android Device Manager erstellt, dupliziert, angepasst und gestartet werden.

[![Screenshot von Android Device Manager auf der Registerkarte „Geräte“](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Dieser Leitfaden gilt nur für Visual Studio für Mac.
Xamarin Studio ist nicht mit Android Device Manager kompatibel.

Mit dem Android Device Manager können *virtuelle Android-Geräte* (AVDs) erstellt und konfiguriert werden, die sich im [Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) ausführen lassen.
Jedes virtuelle Android-Gerät stellt eine Emulatorkonfiguration dar, die ein physisches Android-Gerät simuliert. Dadurch wird das Ausführen und Testen Ihrer App mit einer Vielzahl von Konfigurationen ermöglicht, die verschiedene physische Android-Geräte simulieren.

## <a name="requirements"></a>Anforderungen

- Visual Studio für Mac 7.5 oder höher.

- Android SDK 8.0 (API 26) oder höher muss über den Android SDK-Manager installiert werden.


## <a name="launching-the-device-manager"></a>Starten des Geräte-Managers

Starten Sie Android Device Manager, indem Sie auf **Extras > Android Device Manager** klicken:

[![Start über das Menü „Extras“](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Bevor Sie Android Device Manager verwenden können, müssen Sie Version 26.0.2 oder höher von Android SDK Tools installieren. Wenn Android SDK Tools 26.0.2 oder höher nicht installiert ist, wird beim Starten folgendes Fehlerdialogfeld angezeigt:

![Android SDK: Instanzfehler (Dialogfeld)](device-manager-images/mac/02-sdk-instance-error.png)

Wenn Ihnen dieses Fehlerdialogfeld angezeigt wird, klicken Sie auf **OK**, um den Android SDK-Manager zu öffnen. Klicken Sie im Android SDK-Manager auf die Registerkarte **Extras**, und installieren Sie Folgendes:

-   **Android SDK Tools 26.0.2** oder höher 
-   **Android SDK-Plattformtools 26.0.0** oder höher 
-   **Android SDK-Buildtools 26.0.0** oder höher

Bei diesen Paketen sollte wie im folgenden Screenshot dargestellt der Status **Installiert** angezeigt werden:

[![Installieren von Android SDK Tools 26.0](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>Hauptbildschirm

Wenn Sie den Android-Geräte-Manager zum ersten Mal starten, wird ein Bildschirm mit allen derzeit konfigurierten virtuellen Geräten angezeigt. Für jedes Gerät werden der **Name**, das **Systemimage** (Ebene der Android-API), die **CPU**, die Größe des **Arbeitsspeichers** und die Bildschirmauflösung angezeigt:

[![Liste der installierten Geräte und deren Parameter](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Klicken Sie auf die Schaltfläche **Wiedergeben**, um den Emulator mit dem gewünschten virtuellen Gerät zu starten:

[![Schaltfläche „Starten“ für ein Geräteimage](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

Nachdem der Emulator mit dem ausgewählten virtuellen Gerät startet, ändert sich die Schaltfläche **Wiedergeben** in die Schaltfläche **Beenden**. Diese können Sie verwenden, um den Emulator anzuhalten:

[![Schaltfläche „Beenden“ für das ausgeführte Gerät](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>Neues Gerät

Klicken Sie auf die Schaltfläche **Neues Gerät** im oberen rechten Bereich des Bildschirms, um ein neues Gerät zu erstellen:

[![Schaltfläche „Neu“ zum Erstellen eines neuen Geräts](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

Durch das Klicken auf **Neues Gerät** wird der Bildschirm **Neues Gerät** angezeigt:

[![Bildschirm „Neues Gerät“ des Geräte-Managers](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

Befolgen Sie diese Schritte, um ein neues Gerät im Bildschirm **Neues Gerät** zu konfigurieren:

1. Wählen Sie das zu emulierende physische Gerät aus, indem Sie auf das Pulldownmenü **Gerät** klicken:

    [![Pulldownmenü des Geräts](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. Wählen Sie ein Systemimage aus, das mit diesem virtuellen Gerät verwendet werden soll, indem Sie auf das Pulldownmenü **System image** (Systemimage) klicken. In diesem Menü werden die installierten Systemimages für Device Manager unter **Installiert** aufgeführt. Im Abschnitt **Herunterladen** (wenn dieser angezeigt wird) werden Systemimages für Device Manager aufgeführt, die derzeit nicht auf Ihrem Entwicklungscomputer verfügbar sind, aber automatisch installiert werden können:

    [![Pulldownmenü „Systemimage“](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Weisen Sie dem Gerät einen neuen Namen zu. Im folgenden Beispiel lautet der Name des Geräts **Nexus 5X API 25**:

    [![Benennen des neuen Geräts](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. Bearbeiten Sie alle Eigenschaften, die Sie ändern müssen. Weitere Informationen zum Ändern von Eigenschaften finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).

5. Fügen Sie sämtliche zusätzliche Eigenschaften hinzu, die Sie explizit festlegen müssen. Auf dem Bildschirm **Neues Gerät** werden nur häufig geänderte Eigenschaften angezeigt. Sie können jedoch auf das Pulldownmenü **Eigenschaft hinzufügen** im unteren linken Bereich klicken, um zusätzliche Eigenschaften hinzuzufügen:

    [![Pulldownmenü „Eigenschaft hinzufügen“](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Sie können ebenfalls auf **Benutzerdefiniert** klicken, um eine neue Eigenschaft für das Gerät zu definieren:

    ![Schaltfläche „Erstellen“](device-manager-images/mac/14-custom-button.png)

7. Klicken Sie auf die Schaltfläche **Erstellen** im unteren rechten Bereich, um ein neues Gerät zu erstellen:

    ![Schaltfläche „Erstellen“](device-manager-images/mac/15-create-button.png)

8. Möglicherweise wird Ihnen der Bildschirm **Zustimmung zur Lizenz** angezeigt. Klicken Sie auf **Zustimmen**, wenn Sie den Lizenzbedingungen zustimmen.

9. Der Android Device Manager fügt das neue Gerät zur Liste der installierten virtuellen Geräte hinzu. Dabei zeigt die Statusanzeige **Wird erstellt** an, während das Gerät erstellt wird:

    [![Statusanzeige „Erstellung“](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Wenn der Erstellungsvorgang abgeschlossen ist, ist das neue Gerät startbereit und wird in der Geräteliste mit der Schaltfläche **Wiedergeben** angezeigt:

   [![Das startbereite neu erstellte Gerät](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>Bearbeiten von Geräten

Klicken Sie auf das Pulldownmenü **Zusätzliche Optionen** (Zahnradsymbol) und dann auf **Bearbeiten**, um ein vorhandenes virtuelles Gerät zu bearbeiten:
 
[![Menüauswahl „Bearbeiten“ zum Bearbeiten eines neuen Geräts](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

Wenn Sie auf **Bearbeiten** klicken, wird der Geräte-Editor für das ausgewählte virtuelle Gerät gestartet:
 
[![Bildschirm „Geräte-Editor“](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

Der Bildschirm **Device Editor** (Geräte-Editor) führt in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt.

Im folgenden Screenshot wird beispielsweise die Eigenschaft `hw.lcd.density` von **320** in **240** und die Eigenschaft `hw.ramSize` in **768** geändert:
 
[![Beispiel für das Bearbeiten eines Geräts](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

Klicken Sie auf die Schaltfläche **Speichern**, nachdem Sie die erforderlichen Änderungen an der Konfiguration vorgenommen haben.
Weitere Informationen zum Ändern der Eigenschaften von virtuellen Geräten finden Sie unter [Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Zusätzliche Optionen

Zusätzliche Optionen für die Arbeit mit einem Gerät sind in dem Pulldownmenü verfügbar, das sich auf der linken Seite der Schaltfläche **Wiedergeben** befindet:

[![Position des Menüs „Zusätzliche Optionen“](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

Das Menü „Zusätzliche Optionen“ enthält folgende Elemente:

-   **Bearbeiten:** Öffnet das derzeit ausgewählte Gerät wie zuvor beschrieben im Geräte-Editor.

-   **Duplizieren und Bearbeiten:** Dupliziert das derzeit ausgewählte Gerät und öffnet dieses im Bildschirm **New Device** mit einem anderen eindeutigen Namen.
    Wenn Sie beispielsweise **Nexus 5X API 25** auswählen und auf **Duplicate and Edit** (Duplizieren und Bearbeiten) klicken, wird ein Zähler an den Namen angefügt:

    [![Bildschirm „Duplizieren und Bearbeiten“](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Im Finder zeigen:** Öffnet ein Fenster des macOS-Finders in dem Ordner, der die Dateien für das virtuelle Gerät enthält. Wenn Sie beispielsweise **Nexus 5X API 25** auswählen und auf **Im Finder zeigen** klicken, wird folgendes Fenster geöffnet:

    [![Ergebnis beim Klicken auf „Im Explorer anzeigen“](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Wipe user data** (Benutzerdaten löschen), um das Zurücksetzen zu bestätigen.

-   **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft.
    Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.

-----

## <a name="troubleshooting"></a>Problembehandlung

In den folgenden Abschnitten wird erläutert, wie Sie Probleme diagnostizieren und umgehen, die auftreten können, wenn Sie den Android Device Manager zum Konfigurieren virtueller Geräte verwenden.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Speicherort von Android SDK weicht vom Standard ab

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

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Momentaufnahme deaktiviert WLAN unter Android Oreo

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

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Momentaufnahme deaktiviert WLAN unter Android Oreo

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

### <a name="generating-a-bug-report"></a>Erstellen eines Fehlerberichts

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie mit dem Android Device Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie einen Rechtsklick auf die Titelleiste ausführen und dann auf **Fehlerbericht erstellen** klicken:

[![Ort des Menüelements zum Erstellen eines Fehlerberichts](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Sie mit dem Android Device Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie auf **Hilfe > Fehlerbericht erstellen** klicken:

[![Ort des Menüelements zum Erstellen eines Fehlerberichts](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)


-----

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde die Anwendung „Android Device Manager“ vorgestellt, die in Visual Studio für Mac und Visual Studio-Tools für Xamarin verfügbar ist. Es wurden grundlegende Features wie das Starten und Beenden des Android-Emulators, das Auswählen eines virtuellen Android-Geräts für die Ausführung, das Erstellen von neuen virtuellen Geräten sowie das Bearbeiten von virtuellen Geräten erläutert. Außerdem wurde beschrieben, wie durch die Bearbeitung von Profilhardwareeigenschaften zusätzliche Anpassungen vorgenommen werden können. Des Weiteren wurden Tipps zum Umgang mit häufigen Problemen dargestellt.


## <a name="related-links"></a>Verwandte Links

- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
