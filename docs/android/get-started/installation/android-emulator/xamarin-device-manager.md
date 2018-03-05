---
title: "Xamarin Android-Geräte-Manager"
description: "Der Xamarin Android-Geräte-Manager, der sich derzeit in der Vorschauversion befindet, ersetzt den älteren Google Geräte-Manager. In diesem Handbuch wird erläutert, wie Sie den Xamarin Android-Geräte-Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 20c7c5a9aaaf13cd9f4050254c7234ada78d926d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-android-device-manager"></a>Xamarin Android-Geräte-Manager

_Der Xamarin Android-Geräte-Manager, der sich derzeit in der Vorschauversion befindet, ersetzt den älteren Google Geräte-Manager. In diesem Handbuch wird erläutert, wie Sie den Xamarin Android-Geräte-Manager verwenden, um virtuelle Android-Geräte (AVDs) zu erstellen und zu konfigurieren, die Android-Geräte emulieren. Sie können diese virtuellen Geräte verwenden, um Ihre App auszuführen und zu testen, ohne von einem physischen Gerät abhängig zu sein._

![Derzeit als Vorschauversion verfügbar](~/media/shared/preview.png)

<a name="overview" />
 
## <a name="overview"></a>Übersicht

Nachdem Sie überprüft haben, dass die Hardwarebeschleunigung aktiviert ist (wie unter [Hardwarebeschleunigung](~/android/get-started/installation/android-emulator/hardware-acceleration.md) beschrieben), ist der nächste Schritt das Erstellen von virtuellen Geräten für das Testen und Debuggen Ihrer App. Sie können den Xamarin Android-Geräte-Manager verwenden, um virtuelle Geräte zu erstellen, die vom Android SDK-Emulator verwendet werden können.

Warum sollten Sie den Xamarin Android-Geräte-Manager statt dem [Google Geräte-Manager](~/android/get-started/installation/android-emulator/google-emulator-manager.md) verwenden?
Mit der Veröffentlichung von Version 26.0.1 von Android SDK Tools hat Google die Unterstützung für den AVD- und den SDK-Manager, die auf der Benutzeroberfläche basieren, zugunsten der neuen CLI-Tools (Command Line Interface) eingestellt. Aufgrund dieser Änderung müssen Sie [Xamarin SDK Manager](~/android/get-started/installation/android-sdk.md) und den Xamarin Android-Geräte-Manager verwenden, wenn Sie auf Android SDK Tools 26.0.1 und höher aktualisieren (dies ist für die Android 8.0 Oreo-Entwicklung erforderlich).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Handbuch wird erklärt, wie Sie den Xamarin Android-Geräte-Manager für Visual Studio unter Windows (oder [Mac](?tabs=vsmac)) installieren und verwenden:

[![Screenshot des Xamarin Android-Geräte-Managers auf der Registerkarte „Geräte“](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Handbuch wird erklärt, wie Sie den Xamarin Android-Geräte-Manager für Visual Studio unter Mac (oder [Windows](?tabs=vswin)) installieren und verwenden:

[![Screenshot des Xamarin Android-Geräte-Managers auf der Registerkarte „Geräte“](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png)

> [!NOTE]
> **Hinweis:** Dieser Leitfaden gilt nur für Visual Studio für Mac.
Xamarin Studio ist nicht mit dem Xamarin Android-Geräte-Manager kompatibel.

-----

Der Xamarin Android-Geräte-Manager wird verwendet, um *virtuelle Android-Geräte* (AVDs) zu erstellen und zu konfigurieren, die im [Android SDK-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md) ausgeführt werden.
Jedes virtuelle Android-Gerät stellt eine Emulatorkonfiguration dar, die ein physisches Android-Gerät simuliert. Dadurch wird das Ausführen und Testen Ihrer App mit einer Vielzahl von Konfigurationen ermöglicht, die verschiedene physische Android-Geräte simulieren. Der Xamarin Android-Geräte-Manager ersetzt den eigenständigen AVD-Manager von Google (der als veraltet gilt).

In diesem Handbuch erfahren Sie, wie der Android-Geräte-Manager installiert und gestartet wird. Sie erfahren zudem, wie virtuelle Geräte erstellt, dupliziert, angepasst und gestartet werden. Dieses Handbuch erläutert außerdem die Konfiguration von Eigenschaften für jedes virtuelle Gerät (z.B. API-Ebene, CPU, Arbeitsspeicher und Auflösung), das Aktivieren und Deaktivieren von simulierten Sensoren wie dem Beschleunigungsmesser, GPS und dem Ausrichtungs- und Lichtsensor sowie die Konfiguration der Art der Hardwarebeschleunigung, die vom virtuellen Gerät verwendet wird.


<a name="requirements" />

## <a name="requirements"></a>Anforderungen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie benötigen Folgendes, um den Xamarin Android-Geräte-Manager verwenden zu können:

-   Visual Studio 2017 Version 15.5 oder höher. Visual Studio Community Edition und höher wird unterstützt.

-   Xamarin für Visual Studio Version 4.8 oder höher. Weitere Informationen zum Updaten von Xamarin finden Sie unter [Change the Updates Channel (Ändern des Updatekanals)](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).

-   Die aktuelle Version des [Installers für den Xamarin-Geräte-Manager](https://go.microsoft.com/fwlink/?linkid=865528) für Windows.

-   **Android SDK:** Android SDK muss installiert sein (siehe [Android SDK Setup (Setup von Android SDK)](~/android/get-started/installation/android-sdk.md)), und Version 26.0 von Android SDK Tools muss (gemäß den Erläuterungen im folgenden Abschnitt) installiert sein. Achten Sie darauf, Android SDK an folgendem Speicherort zu installieren (wenn das SDK nicht bereits installiert ist): **C:\\Programme (x86)\\Android\\android-sdk**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

-   Visual Studio für Mac 7.4 oder höher.

-   Die aktuelle Version des [Installers für den Xamarin-Geräte-Manager](https://go.microsoft.com/fwlink/?linkid=865527) für macOS.

-   **Android SDK:** Android SDK 8.0 (API 26) oder höher muss über den SDK-Manager installiert werden.

-----

 
## <a name="installing-the-device-manager"></a>Installieren des Geräte-Managers

Befolgen Sie diese Schritte, um den Xamarin Android-Geräte-Manager zu installieren:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Laden Sie den [Installer für den Xamarin-Geräte-Manager](https://go.microsoft.com/fwlink/?linkid=865528) für Windows herunter.

2. Doppelklicken Sie auf **Xamarin.DeviceManager.msi**, und befolgen Sie die Anleitung für die Installation: 

    ![Setup-Assistent des Xamarin Android-Geräte-Managers](xamarin-device-manager-images/win/30-installer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Laden Sie den [Installer für den Xamarin-Geräte-Manager](https://go.microsoft.com/fwlink/?linkid=865527) für macOS herunter.

2. Doppelklicken Sie auf **AndroidDevices.pkg**, und befolgen Sie die Anleitung für die Installation: 

    [![Setup-Assistent des Xamarin Android-Geräte-Managers](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png)

-----

<a name="dev-manager" /> 
 
## <a name="launching-the-device-manager"></a>Starten des Geräte-Managers

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Visual Studio 15.6 Vorschau 3 und höher können Sie den Xamarin Android-Geräte-Manager über das Menü **Extras** öffnen. Wenn Sie Visual Studio 15.6 Vorschau 3 oder höher verwenden, starten Sie den Geräte-Manager, indem Sie auf **Extras > Android Emulator-Manager** klicken:

[![Start über das Menü „Extras“](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png)

Wenn Sie eine frühere Version von Visual Studio verwenden, muss der Xamarin Android-Geräte-Manager über das Windows-**Startmenü** gestartet werden.

![Der Xamarin Android-Geräte-Manager im Startmenü](xamarin-device-manager-images/win/31-start-menu.png)

Klicken Sie mit der rechten Maustaste auf **Xamarin Android-Geräte-Manager** (Xamarin Android-Geräte-Manager), und wählen Sie **Mehr > Als Administrator ausführen** aus. Wenn beim Starten folgendes Fehlerdialogfeld angezeigt wird, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Anweisungen zur Umgehung dieses Problems:

![Android SDK: Instanzfehler](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In Visual Studio für Mac 7.6 Vorschau 3 (derzeit im Alphakanal) oder höher können Sie den Xamarin Android-Geräte-Manager starten, indem Sie auf **Extras > Emulator-Manager** klicken:

[![Start über das Menü „Extras“](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png)

Wenn Sie eine frühere Version von Visual Studio für Mac verwenden, muss der Xamarin Android-Geräte-Manager unabhängig gestartet werden. Suchen Sie **Android-Geräte** im Ordner **Anwendungen**, und doppelklicken Sie darauf, um ihn zu starten:

[![ Speicherort des Xamarin Android-Geräte-Managers im Finder](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png)


-----

Bevor Sie den Android-Geräte-Manager verwenden können, müssen Sie Version 26.0.0 oder höher von Android SDK Tools installieren. Wenn Android SDK Tools 26.0.0 oder höher nicht installiert ist, wird beim Starten folgendes Fehlerdialogfeld angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android SDK: Instanzfehler (Dialogfeld)](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Android SDK: Instanzfehler (Dialogfeld)](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

Wenn Ihnen dieses Fehlerdialogfeld angezeigt wird, klicken Sie auf **OK**, um den Android SDK-Manager zu öffnen. Klicken Sie im Android SDK-Manager auf die Registerkarte **Tools**, und installieren Sie **Android SDK Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** oder höher:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Installieren von Android SDK Tools 26.0](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png)

Nachdem diese Pakete installiert wurden, können Sie den SDK-Manager schließen und den Android-Geräte-Manager erneut starten.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Installieren von Android SDK Tools 26.0](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png)

-----

<a name="devices" />
 
## <a name="main-screen"></a>Hauptbildschirm

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie den Android-Geräte-Manager zum ersten Mal starten, wird ein Bildschirm mit allen derzeit konfigurierten virtuellen Geräten angezeigt. Für jedes Gerät werden der **Name**, das **Betriebssystem** (Ebene der Android-API), die **CPU**, die Größe des **Arbeitsspeichers** und die Bildschirmauflösung angezeigt:

[![Liste der installierten Geräte und deren Parameter](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Sie den Android-Geräte-Manager zum ersten Mal starten, wird ein Bildschirm mit allen derzeit konfigurierten virtuellen Geräten angezeigt. Für jedes Gerät werden der **Name**, das **Systemimage** (Ebene der Android-API), die **CPU**, die Größe des **Arbeitsspeichers** und die Bildschirmauflösung angezeigt:

[![Liste der installierten Geräte und deren Parameter](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie ein Gerät in der Liste anklicken, wird die Schaltfläche **Starten** auf der rechten Seite angezeigt. Sie können auf die Schaltfläche **Starten** klicken, um den Emulator mit diesem virtuellen Gerät zu starten:

[![Schaltfläche „Starten“ für ein Geräteimage](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Klicken Sie auf die Schaltfläche **Wiedergeben**, um den Emulator mit dem gewünschten virtuellen Gerät zu starten:
 
[![Schaltfläche „Starten“ für ein Geräteimage](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nachdem der Emulator mit dem ausgewählten virtuellen Gerät startet, ändert sich die Schaltfläche **Starten** in die Schaltfläche **Beenden**. Diese können Sie verwenden, um den Emulator anzuhalten:

[![Schaltfläche „Beenden“ für das ausgeführte Gerät](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Nachdem der Emulator mit dem ausgewählten virtuellen Gerät startet, ändert sich die Schaltfläche **Wiedergeben** in die Schaltfläche **Beenden**. Diese können Sie verwenden, um den Emulator anzuhalten:
 
[![Schaltfläche „Beenden“ für das ausgeführte Gerät](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png)
 
-----

<a name="device-new" />
 
### <a name="new-device"></a>Neues Gerät

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Klicken Sie auf die Schaltfläche **Neu** im oberen rechten Bereich des Bildschirms, um ein neues Gerät zu erstellen:

[![Schaltfläche „Neu“ zum Erstellen eines neuen Geräts](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Klicken Sie auf die Schaltfläche **Neues Gerät** im oberen rechten Bereich des Bildschirms, um ein neues Gerät zu erstellen:
 
[![Schaltfläche „Neu“ zum Erstellen eines neuen Geräts](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Durch das Klicken auf **Neu** wird der Bildschirm **Neues Gerät** angezeigt:

[![Bildschirm „Neues Gerät“ des Geräte-Managers](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png)

Befolgen Sie diese Schritte, um ein neues Gerät im Bildschirm **Neues Gerät** zu konfigurieren:

1. Wählen Sie das zu emulierende physische Gerät aus, indem Sie auf das Pulldownmenü **Gerät** klicken:

    [![Pulldownmenü des Geräts](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png)

2. Wählen Sie ein Systemimage aus, das mit diesem virtuellen Gerät verwendet werden soll, indem Sie auf das Pulldownmenü **System image** (Systemimage) klicken. In diesem Menü werden die installierten Systemimages unter **Installiert** aufgeführt. Im Abschnitt **Herunterladen** werden Systemimages aufgeführt, die derzeit nicht auf Ihrem Entwicklungscomputer verfügbar sind, aber automatisch installiert werden können:

    [![Pulldownmenü „Systemimage“](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png)

3. Weisen Sie dem Gerät einen neuen Namen zu. Im folgenden Beispiel lautet der Name des Geräts **Nexus 5 API 25**:

    [![Benennen des neuen Geräts](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png)

4. Bearbeiten Sie alle Eigenschaften, die Sie ändern müssen. Weitere Informationen zum Ändern von Eigenschaften finden Sie weiter unten in diesem Handbuch unter [Profileigenschaften](#properties).

5. Fügen Sie sämtliche zusätzliche Eigenschaften hinzu, die Sie explizit festlegen müssen. Auf dem Bildschirm **Neues Gerät** werden nur häufig geänderte Eigenschaften angezeigt. Sie können jedoch auf das Pulldownmenü **Eigenschaft hinzufügen** im unteren linken Bereich klicken, um zusätzliche Eigenschaften hinzuzufügen. Im folgenden Beispiel wird die `hw.lcd.backlight`-Eigenschaft hinzugefügt:

    [![Pulldownmenü „Eigenschaft hinzufügen“](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png)

6. Klicken Sie auf die Schaltfläche **Erstellen** im unteren rechten Bereich, um ein neues Gerät zu erstellen:

    ![Schaltfläche „Erstellen“](xamarin-device-manager-images/win/14-create-button.png)

7. Möglicherweise wird Ihnen der Bildschirm **Zustimmung zur Lizenz** angezeigt. Klicken Sie auf **Zustimmen**, wenn Sie den Lizenzbedingungen zustimmen:

    ![Bildschirm „Zustimmung zur Lizenz“](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Der Android-Geräte-Manager fügt das neue Gerät zur Liste der installierten virtuellen Geräte hinzu. Dabei zeigt die Statusanzeige **Creating** (Wird erstellt) an, während das Gerät erstellt wird:

    [![Statusanzeige „Erstellung“](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png)

9. Wenn der Erstellungsvorgang abgeschlossen ist, ist das neue Gerät startbereit und wird in der Liste der installierten virtuellen Geräte mit der Schaltfläche **Starten** angezeigt:

   [![Das startbereite neu erstellte Gerät](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Durch das Klicken auf **Neues Gerät** wird der Bildschirm **Neues Gerät** angezeigt:

[![Bildschirm „Neues Gerät“ des Geräte-Managers](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png)

Befolgen Sie diese Schritte, um ein neues Gerät im Bildschirm **Neues Gerät** zu konfigurieren:

1. Wählen Sie das zu emulierende physische Gerät aus, indem Sie auf das Pulldownmenü **Gerät** klicken:

    [![Pulldownmenü des Geräts](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png)

2. Wählen Sie ein Systemimage aus, das mit diesem virtuellen Gerät verwendet werden soll, indem Sie auf das Pulldownmenü **System image** (Systemimage) klicken. In diesem Menü werden die installierten Systemimages unter **Installiert** aufgeführt. Im Abschnitt **Herunterladen** (wenn dieser angezeigt wird) werden Systemimages aufgeführt, die derzeit nicht auf Ihrem Entwicklungscomputer verfügbar sind, aber automatisch installiert werden können:

    [![Pulldownmenü „Systemimage“](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png)

3. Weisen Sie dem Gerät einen neuen Namen zu. Im folgenden Beispiel lautet der Name des Geräts **Nexus 5X API 25**:

    [![Benennen des neuen Geräts](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png)

4. Bearbeiten Sie alle Eigenschaften, die Sie ändern müssen. Weitere Informationen zum Ändern von Eigenschaften finden Sie weiter unten in diesem Handbuch unter [Profileigenschaften](#properties).

5. Fügen Sie sämtliche zusätzliche Eigenschaften hinzu, die Sie explizit festlegen müssen. Auf dem Bildschirm **Neues Gerät** werden nur häufig geänderte Eigenschaften angezeigt. Sie können jedoch auf das Pulldownmenü **Eigenschaft hinzufügen** im unteren linken Bereich klicken, um zusätzliche Eigenschaften hinzuzufügen:

    [![Pulldownmenü „Eigenschaft hinzufügen“](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png)

6. Sie können ebenfalls auf **Benutzerdefiniert** klicken, um eine neue Eigenschaft für das Gerät zu definieren:

    ![Schaltfläche „Erstellen“](xamarin-device-manager-images/mac/14-custom-button.png)

7. Klicken Sie auf die Schaltfläche **Erstellen** im unteren rechten Bereich, um ein neues Gerät zu erstellen:

    ![Schaltfläche „Erstellen“](xamarin-device-manager-images/mac/15-create-button.png)

8. Möglicherweise wird Ihnen der Bildschirm **Zustimmung zur Lizenz** angezeigt. Klicken Sie auf **Zustimmen**, wenn Sie den Lizenzbedingungen zustimmen.

9. Während das Gerät erstellt wird, fügt der Android-Geräte-Manager das neue Gerät mit der Statusanzeige **Creating** (Wird erstellt) zur Geräteliste hinzu:

    [![Statusanzeige „Erstellung“](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png)

10. Wenn der Erstellungsvorgang abgeschlossen ist, ist das neue Gerät startbereit und wird in der Geräteliste mit der Schaltfläche **Wiedergeben** angezeigt:

   [![Das startbereite neu erstellte Gerät](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png)

-----


<a name="device-edit" />
 
### <a name="edit-device"></a>Bearbeiten von Geräten

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wählen Sie zum Bearbeiten eines vorhandenen virtuellen Geräts ein Gerät aus, und klicken Sie auf die Schaltfläche **Bearbeiten**, die sich im oberen rechten Bereich des Bildschirms befindet:

[![Schaltfläche „Bearbeiten“ zum Bearbeiten eines neuen Geräts](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Klicken Sie auf das Pulldownmenü **Zusätzliche Optionen** (Zahnradsymbol) und dann auf **Bearbeiten**, um ein vorhandenes virtuelles Gerät zu bearbeiten:
 
[![Menüauswahl „Bearbeiten“ zum Bearbeiten eines neuen Geräts](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png)
 
-----

Wenn Sie auf **Bearbeiten** klicken, wird der Geräte-Editor für das ausgewählte virtuelle Gerät gestartet:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bildschirm „Geräte-Editor“](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
 
[![Bildschirm „Geräte-Editor“](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png)
 
-----

Der Bildschirm **Device Editor** (Geräte-Editor) führt in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Im folgenden Screenshot wird beispielsweise die Eigenschaft `hw.lcd.density` von **420** in **240** geändert:

[![Beispiel für das Bearbeiten eines Geräts](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Im folgenden Screenshot wird beispielsweise die Eigenschaft `hw.lcd.density` von **320** in **240** und die Eigenschaft `hw.ramSize` in **768** geändert:
 
[![Beispiel für das Bearbeiten eines Geräts](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png)
 
-----

Klicken Sie auf die Schaltfläche **Speichern**, nachdem Sie die erforderlichen Änderungen an der Konfiguration vorgenommen haben.
Weitere Informationen zum Ändern der Eigenschaften von virtuellen Geräten finden Sie weiter unten in diesem Handbuch unter [Profileigenschaften](#properties).


<a name="addopt" />
 
### <a name="additional-options"></a>Zusätzliche Optionen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zusätzliche Optionen für die Arbeit mit Geräten sind im &hellip;-Menü in der oberen rechten Ecke verfügbar:

[![Position des Menüs „Zusätzliche Optionen“](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Zusätzliche Optionen für die Arbeit mit einem Gerät sind in dem Pulldownmenü verfügbar, das sich auf der linken Seite der Schaltfläche **Wiedergeben** befindet:

[![Position des Menüs „Zusätzliche Optionen“](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Das Menü „Zusätzliche Optionen“ enthält folgende Elemente:

-   **Duplizieren und Bearbeiten:** Dupliziert das derzeit ausgewählte Gerät und öffnet dieses im Bildschirm **New Device** mit einem anderen eindeutigen Namen. Wenn Sie beispielsweise **VisualStudio_android-23_x86_phone** auswählen und auf **Duplicate and Edit** (Duplizieren und Bearbeiten) klicken, wird ein Zähler an den Namen angefügt:

    [![Bildschirm „Duplizieren und Bearbeiten“](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png)

-   **Im Explorer anzeigen:** Öffnet ein Fenster des Windows-Explorers in dem Ordner, der die Dateien für das virtuelle Gerät enthält. Wenn Sie beispielsweise **Nexus 5X API 25** auswählen und auf **Im Explorer anzeigen** klicken, wird folgendes Fenster geöffnet:

    [![Ergebnis beim Klicken auf „Im Explorer anzeigen“](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png)

-   **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Wipe user data** (Benutzerdaten löschen), um das Zurücksetzen zu bestätigen.

-   **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft.
    Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Das Menü „Zusätzliche Optionen“ enthält folgende Elemente:

-   **Bearbeiten:** Öffnet das derzeit ausgewählte Gerät wie zuvor in [Gerät bearbeiten](#device-edit) beschrieben im Geräte-Editor.

-   **Duplizieren und Bearbeiten:** Dupliziert das derzeit ausgewählte Gerät und öffnet dieses im Bildschirm **New Device** mit einem anderen eindeutigen Namen.
    Wenn Sie beispielsweise **Nexus 5X API 25** auswählen und auf **Duplicate and Edit** (Duplizieren und Bearbeiten) klicken, wird ein Zähler an den Namen angefügt:

    [![Bildschirm „Duplizieren und Bearbeiten“](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png)

-   **Im Finder zeigen:** Öffnet ein Fenster des macOS-Finders in dem Ordner, der die Dateien für das virtuelle Gerät enthält. Wenn Sie beispielsweise **Nexus 5X API 25** auswählen und auf **Im Finder zeigen** klicken, wird folgendes Fenster geöffnet:

    [![Ergebnis beim Klicken auf „Im Explorer anzeigen“](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png)

-   **Auf Werkseinstellungen zurücksetzen:** Setzt das ausgewählte Gerät auf die Standardeinstellungen zurück. Dabei werden alle Benutzeränderungen am internen Zustand des Geräts, die während der Ausführung vorgenommen wurden, gelöscht. Dies wirkt sich nicht auf Änderungen aus, die während der Erstellung und Bearbeitung des virtuellen Geräts vorgenommen werden. Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Zurücksetzen nicht rückgängig gemacht werden kann. Klicken Sie auf **Wipe user data** (Benutzerdaten löschen), um das Zurücksetzen zu bestätigen.

-   **Löschen:** Löscht das ausgewählte virtuelle Gerät dauerhaft.
    Es wird ein Dialogfeld mit der Erinnerung angezeigt, dass das Löschen eines Geräts nicht rückgängig gemacht werden kann. Klicken Sie auf **Löschen**, wenn Sie sich sicher sind, dass Sie das Gerät löschen möchten.

-----

<a name="properties" />
 
## <a name="profile-properties"></a>Profileigenschaften

Die Bildschirme **Neues Gerät** und **Device Editor** (Geräte-Editor) führen in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt. Sie können die *Hardwareprofileigenschaften* und die *AVD-Eigenschaften* bearbeiten.
Hardwareprofileigenschaften (z.B. `hw.ramSize` und `hw.accelerometer`) beschreiben die physischen Merkmale des emulierten Geräts. Zu diesen Merkmalen gehören die Bildschirmgröße, die Menge des verfügbaren Arbeitsspeichers und ob ein Beschleunigungsmesser vorhanden ist. AVD-Eigenschaften geben die Vorgänge des virtuellen Android-Geräts bei der Ausführung an. AVD-Eigenschaften können beispielsweise konfiguriert werden, um anzugeben, wie das virtuelle Android-Gerät die Grafikkarte Ihres Entwicklungscomputers für das Rendering verwendet.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie können Eigenschaften mithilfe der folgenden Anleitung ändern:

-   Aktivieren Sie das Kontrollkästchen auf der rechten Seite einer booleschen Eigenschaft, um diese zu ändern:

    ![Ändern einer booleschen Eigenschaft](xamarin-device-manager-images/win/25-boolean-value.png)

-   Klicken Sie zum Ändern einer *enumerierten* Eigenschaft auf den Dropdownpfeil auf der rechten Seite der Eigenschaft, und wählen Sie einen neuen Wert aus.

    ![Ändern einer enumerierten Eigenschaft](xamarin-device-manager-images/win/27-enum-value.png)

-   Doppelklicken Sie zum Ändern einer Zeichenfolge oder einer ganzzahligen Eigenschaft auf die aktuelle Zeichenfolge oder auf die ganzzahlige Einstellung in der Wertespalte, und geben Sie einen neuen Wert ein.

    ![Ändern einer ganzzahligen Eigenschaft](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Sie können Eigenschaften mithilfe der folgenden Anleitung ändern:

-   Aktivieren Sie das Kontrollkästchen auf der rechten Seite einer booleschen Eigenschaft, um diese zu ändern:

    ![Ändern einer booleschen Eigenschaft](xamarin-device-manager-images/mac/25-boolean-value.png)

-   Klicken Sie zum Ändern einer *enumerierten* Eigenschaft auf das Pulldownmenü auf der rechten Seite der Eigenschaft, und wählen Sie einen neuen Wert aus.

    ![Ändern einer enumerierten Eigenschaft](xamarin-device-manager-images/mac/27-enum-value.png)

-   Doppelklicken Sie zum Ändern einer Zeichenfolge oder einer ganzzahligen Eigenschaft auf die aktuelle Zeichenfolge oder auf die ganzzahlige Einstellung in der Wertespalte, und geben Sie einen neuen Wert ein.

    ![Ändern einer ganzzahligen Eigenschaft](xamarin-device-manager-images/mac/26-integer-value.png)


-----

In der folgenden Tabellen werden die Eigenschaften näher erläutert, die in den Bildschirmen **Neues Gerät** und **Device Editor** (Geräte-Editor) aufgeführt sind:

[!include[](~/android/includes/table.html)]

Weitere Informationen zu diesen Eigenschaften finden Sie unter [Hardwareprofileigenschaften](https://developer.android.com/studio/run/managing-avds.html#hpproperties).


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Problembehandlung

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Im Folgenden werden häufige Probleme mit dem Xamarin Android-Geräte-Manager sowie Problemumgehungen beschrieben:

### <a name="android-sdk-in-non-standard-location"></a>Speicherort von Android SDK weicht vom Standard ab

Üblicherweise wird Android SDK an folgendem Speicherort installiert:

**C:\\Programme (x86)\\Android\\android-sdk**

Wenn die SDK nicht an diesem Speicherort installiert wurde, wird Ihnen beim Starten möglicherweise folgende Fehlermeldung angezeigt:

![Android SDK: Instanzfehler](xamarin-device-manager-images/win/32-sdk-error.png)

Gehen Sie folgendermaßen vor, um dieses Problem zu umgehen:

1. Navigieren Sie vom Windows-Desktop zu **C:\\Benutzer\\*Benutzername*\\AppData\\Roaming\\XamarinDeviceManager**:

    ![Speicherort der Protokolldatei des Xamarin-Geräte-Managers](xamarin-device-manager-images/win/33-log-files.png)

2. Doppelklicken Sie auf eine der Protokolldateien, um diese zu öffnen und den **Konfigurationsdateipfad** zu suchen. Zum Beispiel:

    [![Konfigurationsdateipfad in der Protokolldatei](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png)

3. Navigieren Sie zu diesem Speicherort, und doppelklicken Sie auf **user.config**, um die Datei zu öffnen. 

4. Suchen Sie in **user.config** das Element **&lt;UserSettings&gt;**, und fügen Sie diesem das Attribut **AndroidSdkPath** hinzu. Legen Sie dieses Attribut auf den Pfad fest, unter dem Android SDK auf Ihrem Computer installiert wurde, und speichern Sie die Datei. **&lt;UserSettings&gt;** würde beispielsweise folgendermaßen aussehen, wenn Android SDK unter **C:\\Programme\\Android\\SDK** installiert wäre:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Nachdem Sie diese Änderung an **user.config** vorgenommen haben, sollten Sie den Xamarin Android-Geräte-Manager starten können.

### <a name="generating-a-bug-report"></a>Erstellen eines Fehlerberichts

Wenn Sie mit dem Xamarin Android-Geräte-Manager auf einen Fehler stoßen, der mit den oben genannten Tipps zur Problembehandlung nicht gelöst werden kann, erstellen Sie bitte einen Fehlerbericht, indem Sie einen Rechtsklick auf die Titelleiste ausführen und dann auf **Generate Bug Report** (Fehlerbericht erstellen) klicken:

![Ort des Menüelements zum Erstellen eines Fehlerberichts](xamarin-device-manager-images/win/35-bug-report.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Derzeit gibt es keine bekannten Probleme und zugehörige Problemumgehungen für den Xamarin Android-Geräte-Manager in Visual Studio für Mac. 

### <a name="generating-a-bug-report"></a>Erstellen eines Fehlerberichts

Wenn Sie auf ein Problem stoßen, erstellen Sie bitte einen Fehlerbericht, indem Sie auf **Help > Generate Bug Report** (Hilfe > Fehlerbericht erstellen) klicken:

![Ort des Menüelements zum Erstellen eines Fehlerberichts](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
<a name="summary" />
 
## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde der Xamarin Android-Geräte-Manager vorgestellt, der in Visual Studio für Mac und Xamarin für Visual Studio verfügbar ist. Es wurden grundlegende Features wie das Starten und Beenden des Android-Emulators, das Auswählen eines virtuellen Android-Geräts für die Ausführung, das Erstellen von neuen virtuellen Geräten sowie das Bearbeiten von virtuellen Geräten erläutert. Darüber hinaus wurde erläutert, wie Hardwareprofileigenschaften für die weitere Anpassung bearbeitet werden können.


## <a name="related-links"></a>Verwandte Links

- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Debuggen mit dem Android SDK-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
