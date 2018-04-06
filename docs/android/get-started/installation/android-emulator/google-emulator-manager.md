---
title: Google Emulator Manager
description: Erstellen und Verwalten von virtuellen Android-Geräten mithilfe des Google Emulator Managers
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a399aa1c314f1e93377a7831b430e563d9fd1b13
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="google-emulator-manager"></a>Google Emulator Manager

Nachdem Sie überprüft haben, dass die Hardwarebeschleunigung aktiviert ist (wie unter [Android Emulator Hardware Acceleration (Hardwarebeschleunigung für Android-Emulator)](~/android/get-started/installation/android-emulator/hardware-acceleration.md) beschrieben), ist der nächste Schritt das Erstellen virtueller Geräte für das Testen und Debuggen Ihrer App. Sie können den älteren Google Emulator Manager (auch als *Android Virtual Device (AVD) Manager* bekannt) verwenden, um virtuelle Geräte zu erstellen, die vom Android SDK-Emulator verwendet werden sollen.

> [!NOTE]
> Wenn Sie Android 8.0 Oreo als Ziel verwenden, müssen Sie den [Android Xamarin-Geräte-Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) verwenden, um virtuelle Geräte zu erstellen und zu konfigurieren.


## <a name="installing-system-images"></a>Installieren von Systemimages

Abhängig davon, welche Android-API-Ebenen Sie als Ziel verwenden möchten, müssen Sie für API-Ebenen spezifische Systemimages herunterladen und installieren, die vom Android SDK Emulator verwendet werden. Für jede Android-API-Ebene gibt es eine Reihe von **x86**-Systemimages, die Sie zum Erstellen von virtuellen Geräten herunterladen und installieren müssen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Um die erforderlichen Systemimages zu installieren, starten Sie den Android SDK-Manager (**Extras > Android > Android SDK-Manager**), und scrollen Sie zu den API-Ebenen, die Sie unterstützen möchten. Aktivieren Sie bei jeder API-Ebene das Kontrollkästchen neben den folgenden Systemimages:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Um die erforderlichen Systemimages zu installieren, starten Sie den Android SDK-Manager (**Extras > SDK-Manager**), und scrollen Sie zu den API-Ebenen, die Sie unterstützen möchten. Aktivieren Sie bei jeder API-Ebene das Kontrollkästchen neben den folgenden Systemimages:

-----

-   **Intel x86 Atom System Image**
-   **Google APIs Intel x86 Atom System Image**

Das letztere Systemimage fügt Google-APIs (z.B. Google Maps-APIs) zum virtuellen Gerät hinzu. 

Im folgenden Screenshot werden **Intel x86 Atom**-Images installiert, sodass virtuelle Geräte mit Android 6.0 erstellt werden können:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Auswählen von Android 6.0-x86-Systemimages für den Android-Emulator](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Auswählen von Android 6.0-x86-Systemimages für den Android-Emulator](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png#lightbox)

-----

Wenn Sie 64-Bit-Apps entwickeln, installieren Sie stattdessen die folgenden Systemimages:

-   **Intel x86 Atom_64 System Image**
-   **Google APIs Intel x86 Atom_64 System Image**

Sie können diese 64-Bit-Systemimages verwenden, um 32-Bit-Apps auszuführen. Jedoch wird das 32-Bit-Image **Intel x86 Atom System Image** im Android SDK Emulator etwas schneller ausgeführt.

Wenn Sie Apps für Android Wear entwickeln, installieren Sie die folgenden Systemimages:

-   **Android Wear Intel x86 Atom System Image**
-   **Google APIs Intel x86 Atom System Image**

Nachdem diese Systemimages installiert wurden, können Sie **x86**-basierte virtuelle Android-Geräte erstellen, indem Sie während der Konfiguration des virtuellen Geräts die entsprechende API-Ebene und CPU/ABI auswählen (dies wird als Nächstes beschrieben).


## <a name="configuring-virtual-devices"></a>Konfigurieren von virtuellen Geräten

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Virtuelle Geräte werden über den **Android-Emulator-Manager** (auch als _Android Virtual Device Manager_ oder _AVD Manager_ bezeichnet) konfiguriert. Um den Android-Emulator-Manager von Visual Studio aus zu starten, klicken Sie auf das **Android-Emulator-Manager**-Symbol in der Symbolleiste:

[![Stelle des AVD-Symbols](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png#lightbox)

Sie können den Android-Emulator-Manager auch über die Menüleiste starten, indem Sie **Extras > Android > Android-Emulator-Manager** auswählen:

[![Stelle des Menüelements „Android-Emulator-Manager“](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png#lightbox)

Das Dialogfeld **Android Virtual Device (AVD) Manager** (Android Virtual Device Manager (AVD)) zeigt die Liste vorhandener virtueller Android-Geräte an:

[![Android Virtual Device Manager](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Virtuelle Geräte werden über den **Android-Emulator-Manager** (auch als _Android Virtual Device Manager_ oder _AVD Manager_ bezeichnet) konfiguriert. 

Sie können den Android-Emulator-Manager über die Menüleiste starten, indem Sie **Extras > Android-Emulator-Manager** auswählen:

[![Stelle des Menüelements „Android-Emulator-Manager“](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png#lightbox)

Das Dialogfeld **Android Virtual Device (AVD) Manager** (Android Virtual Device Manager (AVD)) zeigt die Liste vorhandener virtueller Android-Geräte an:

[![Android Virtual Device Manager](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png#lightbox)

-----

Sie können neue Images für virtuelle Geräte mit verschiedenen Gerätecharakteristika und API-Ebenen erstellen &ndash; im nächsten Abschnitt wird erklärt, wie benutzerdefinierte Gerätedefinitionen und virtuelle Geräte erstellt werden.


### <a name="creating-a-custom-device-definition"></a>Erstellen einer benutzerdefinierten Gerätedefinition

Um eine benutzerdefinierte Gerätedefinition zu erstellen, klicken Sie im **Android Virtual Device Manager (AVD)** auf **Erstellen...** Dadurch öffnet sich das Dialogfeld **Create new Android Virtual Device (AVD)** (Neues Android Virtual Device (AVD) erstellen):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefinierte Gerätedefinition basierend auf Nexus 6](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefinierte Gerätedefinition basierend auf Nexus 6](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png#lightbox)

-----

Konfigurieren Sie in diesem Dialogfeld die folgenden Optionen:

-   **AVD Name** (AVD-Name): eindeutiger Name für die Gerätedefinition. Im oben gezeigten Beispielscreenshot wird als Name **MyNexus** festgelegt. Beachten Sie, dass der AVD-Name keine Leerzeichen enthalten darf &ndash; die Schaltfläche **OK** wird deaktiviert, wenn Sie versuchen, Leerzeichen zu verwenden.

-   **Gerät**: Wählen Sie das Hardwareprofil aus, das Sie emulieren möchten (z.B. **Nexus 5** oder **Nexus 6**).

-   **Ziel**: Wählen Sie die Android-API-Ebene für das virtuelle Gerät aus. Bei dieser Einstellung sollte die Android-Mindestversion Ihrer App oder eine neuere Version angegeben werden.

-   **CPU/ABI**: Wählen Sie **Google APIs Intel Atom (x86)** aus, damit Google-APIs in Ihrer Gerätedefinition zur Verfügung stehen.

-   **Design**: Wählen Sie die Darstellung des virtuellen Geräts aus. Im obigen Beispielscreenshot wird das **HVGA**-Design ausgewählt. ist Der Screenshot des Emulators am Ende dieses Artikels ist ein Beispiel für das **HVGA**-Design.

-   **Memory Options** (Arbeitsspeicheroptionen): üblicherweise wird standardmäßig zu viel RAM festgelegt. Dies führt unter Windows dazu, dass die Warnung **emulating RAM greater than 768M may fail** (Emulieren von 768 MB RAM oder mehr schlägt möglicherweise fehl) angezeigt wird. Für die meisten Benutzer werden 768 MB RAM als Einstellung empfohlen (wie im Screenshot oben gezeigt). Hohe RAM-Werte können den Emulator verlangsamen.

-   **Use Host GPU** (Host-GPU verwenden): Durch die Auswahl dieser Option verwendet der Emulator den Grafikprozessor (Graphics Processing Unit, GPU) des Hostcomputers zur Ausführung von Grafikoperationen. Das Aktivieren dieser Option wird empfohlen, damit eine noch höhere Leistung des Emulators erzielt werden kann. Weitere Informationen zum Abschnitt **Emulation Options** (Emulationsoptionen) finden Sie unter [What are Snapshot and Use Host GPU emulation options used for? (Wozu dienen die Optionen „Snapshot“ (Momentaufnahme) und „Use Host GPU“ (Host-GPU verwenden)?)](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for).


Für die übrigen Optionen können die Standardeinstellungen übernommen werden. Klicken Sie anschließend auf **OK**, um das neue virtuelle Gerät zu erstellen. Die Ergebnisse für die Konfiguration des neuen virtuellen Geräts werden im folgenden Dialogfeld aufgeführt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogfeld mit Ergebnissen nach Erstellen eines neuen AVD](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Dialogfeld mit Ergebnissen nach Erstellen eines neuen AVD](google-emulator-manager-images/mac/07-create-results.png)

-----

Eine ausführliche Erläuterung der Konfigurationseigenschaften in diesem Dialogfeld finden Sie unter [Hardware Profile Properties (Hardwareprofileigenschaften)](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
Nachdem Sie auf **OK** klicken, wird die neue Gerätekonfiguration in der Liste der vorhandenen virtuellen Android-Geräte angezeigt. Im folgenden Screenshot wurde **MyNexus** der Liste hinzugefügt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus zur Geräteliste hinzugefügt](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![MyNexus zur Geräteliste hinzugefügt](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png#lightbox)

-----

Das neue benutzerdefinierte virtuelle Gerät wird auch dem Geräte-Pulldownmenü hinzugefügt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus zum Geräte-Pulldownmenü hinzugefügt](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![MyNexus zum Geräte-Pulldownmenü hinzugefügt](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png#lightbox)

-----



### <a name="cloning-a-device-definition"></a>Klonen einer Gerätedefinition

Sie können eine vorhandene Gerätedefinition auswählen und *klonen*, um eine neue benutzerdefinierte Gerätedefinition zu erstellen. Diese Strategie empfiehlt sich, wenn eine Gerätedefinition vorhanden ist, die zur Erfüllung Ihrer Anforderungen nur geringfügig geändert werden muss. Im Dialogfeld **Android Virtual Device (AVD) Manager** (Android Virtual Device Manager (AVD)) werden auf der Registerkarte **Device Definitions** (Gerätedefinitionen) alle verfügbaren Gerätedefinitionen aufgeführt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Liste der verfügbaren Gerätedefinitionen](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Liste der verfügbaren Gerätedefinitionen](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png#lightbox)

-----

Die vorkonfigurierten Geräte in dieser Liste können nicht geändert werden &ndash; nur von Benutzern erstellte virtuelle Geräte können bearbeitet werden. Sie können eine neue Gerätedefinition auf der Grundlage einer vorkonfigurierten Gerätedefinition erstellen, indem Sie die Gerätedefinition auswählen und auf **Klonen** klicken. Wenn Sie z.B. die Definition **Nexus 5** auswählen und auf **Klonen** klicken, wird das folgende Dialogfeld angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dialogfeld „Clone device“ (Gerät klonen)](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Dialogfeld „Clone device“ (Gerät klonen)](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png#lightbox)

-----

Im nächsten Screenshot wird der Name in **Nexus 5 Custom** geändert. Außerdem werden die Geräteparameter zur Erstellung einer neuen benutzerdefinierten Gerätedefinition geändert:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Benutzerdefiniertes AVD Nexus 5](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefiniertes AVD Nexus 5](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png#lightbox)

-----

Sobald Sie auf **Clone Device** (Gerät klonen) klicken, wird die neue Gerätedefinition erstellt und in der Liste **Device Definitions** (Gerätedefinitionen) angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5 Custom wird als neue Benutzergerätedefinition angezeigt](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Nexus 5 Custom wird als neue Benutzergerätedefinition angezeigt](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png#lightbox)

-----

Beachten Sie, dass jede von Benutzern erstellte Gerätedefinition wie oben zu sehen zusammen mit einem grünen Symbol angezeigt wird. Mit der neuen Gerätedefinition können Sie ein neues AVD erstellen. Wählen Sie hierzu die Definition aus, und klicken Sie auf **AVD erstellen**. Dadurch wird das Dialogfeld **Create new Android Virtual Device (AVD)** (Neues Android Virtual Device (AVD) erstellen) angezeigt. Im folgenden Beispiel wurde der Name **AVD\_für\_Nexus\_5\_Custom** automatisch für das neue virtuelle Gerät generiert:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Erstellen von AVD mit Nexus 5 Custom-Gerätedefinition](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Erstellen von AVD mit Nexus 5 Custom-Gerätedefinition](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png#lightbox)

-----

Nachdem Sie auf **OK** klicken, wird die benutzerdefinierte Gerätekonfiguration in der Liste der vorhandenen virtuellen Android-Geräte angezeigt. Außerdem wird die Gerätedefinition dem Geräte-Pulldownmenü hinzugefügt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Neues benutzerdefiniertes AVD zum Geräte-Dropdownmenü hinzugefügt](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Neues benutzerdefiniertes AVD zum Geräte-Dropdownmenü hinzugefügt](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png#lightbox)

-----

