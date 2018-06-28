---
title: Verwalten virtueller Geräte mit Android Device Manager
description: In diesem Artikel wird erläutert, wie Sie Android Device Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die physische Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 888f126d3e58b0300ba7ce3ad1cb5a8001fc545a
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733587"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Verwalten virtueller Geräte mit Android Device Manager

_In diesem Artikel wird erläutert, wie Sie Android Device Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die physische Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein._

## <a name="overview"></a>Übersicht

Nachdem Sie überprüft haben, dass die Hardwarebeschleunigung aktiviert ist (wie unter [Hardwarebeschleunigung für verbesserte Leistung des Emulators](~/android/get-started/installation/android-emulator/hardware-acceleration.md) beschrieben), besteht der nächste Schritt darin, _Android Device Manager_ (auch als _Xamarin Android Device Manager_ bezeichnet) zu verwenden, um virtuelle Geräte zu erstellen, die Sie zum Testen und Debuggen Ihrer App verwenden können.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Artikel wird erläutert, wie virtuelle Android-Geräte mit Android Device Manager erstellt, dupliziert, angepasst und gestartet werden.

[![Screenshot von Android Device Manager auf der Registerkarte „Geräte“](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Android Device Manager wird verwendet, um _virtuelle Android-Geräte_ (AVDs) zu erstellen und zu konfigurieren, die im [Google Android-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md) ausgeführt werden.
Jedes virtuelle Android-Gerät stellt eine Emulatorkonfiguration dar, die ein physisches Android-Gerät simuliert. Dadurch wird das Ausführen und Testen Ihrer App mit einer Vielzahl von Konfigurationen ermöglicht, die verschiedene physische Android-Geräte simulieren.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um Android Device Manager verwenden zu können:

-   Visual Studio 2017, Version 15.7 oder höher. Visual Studio Community, Professional und Enterprise werden unterstützt.

-   Visual Studio-Tools für Xamarin Version 4.9 oder höher.

-   Das Android SDK muss installiert sein (siehe [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md)), und Version 26.1.1 oder höher von Android SDK Tools muss (gemäß den Erläuterungen im folgenden Abschnitt) installiert sein. Achten Sie darauf, Android SDK an folgendem Speicherort zu installieren (wenn das SDK nicht bereits installiert ist): **C:\\Programme (x86)\\Android\\android-sdk**.


## <a name="launching-the-device-manager"></a>Starten des Geräte-Managers

Starten Sie Android Device Manager über das Menü **Extras**, indem Sie auf **Extras > Android > Android Device Manager** klicken:

[![Start über das Menü „Extras“](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

Wenn beim Starten folgendes Fehlerdialogfeld angezeigt wird, finden Sie unter [Behandlung von Problemen beim Setup des Emulators](~/android/get-started/installation/android-emulator/troubleshooting.md) Anweisungen zur Umgehung dieses Problems:

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

-   **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dabei wird die aktuelle [Quick Boot](~/android/deploy-test/debugging/android-sdk-emulator/running-the-emulator.md#quick-boot)-Momentaufnahme gelöscht (falls vorhanden). Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Wipe user data** (Benutzerdaten löschen), um das Zurücksetzen zu bestätigen.

-   **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft.
    Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Artikel wird erläutert, wie virtuelle Android-Geräte mit Android Device Manager erstellt, dupliziert, angepasst und gestartet werden.

[![Screenshot von Android Device Manager auf der Registerkarte „Geräte“](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Dieser Leitfaden gilt nur für Visual Studio für Mac.
Xamarin Studio ist nicht mit Android Device Manager kompatibel.

Android Device Manager wird verwendet, um *virtuelle Android-Geräte* (AVDs) zu erstellen und zu konfigurieren, die im [Google Android-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md) ausgeführt werden.
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

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde die Anwendung „Android Device Manager“ vorgestellt, die in Visual Studio für Mac und Visual Studio-Tools für Xamarin verfügbar ist. Es wurden grundlegende Features wie das Starten und Beenden des Android-Emulators, das Auswählen eines virtuellen Android-Geräts für die Ausführung, das Erstellen von neuen virtuellen Geräten sowie das Bearbeiten von virtuellen Geräten erläutert. Darüber hinaus wurde erläutert, wie Hardwareprofileigenschaften für die weitere Anpassung bearbeitet werden können.


## <a name="related-links"></a>Verwandte Links

- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Debuggen mit dem Android SDK-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
