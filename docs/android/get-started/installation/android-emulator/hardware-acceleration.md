---
title: Hardwarebeschleunigung für Android-Emulator
description: So bereiten Sie Ihren Computer auf die höchstmögliche Leistung des Android SDK Emulators vor
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/04/2018
ms.openlocfilehash: d5921c549c299197bdc442c9b883b49064655f76
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Hardwarebeschleunigung für Android-Emulator

Da der Android SDK Emulator ohne Hardwarebeschleunigung sehr langsam ist und so sehr viel Zeit in Anspruch nimmt, wird HAXM von Intel (Hardware Accelerated Execution Manager) empfohlen, um die Leistung des Android SDK Emulators deutlich zu verbessern.


## <a name="haxm-overview"></a>HAXM-Übersicht

HAXM ist ein hardwareunterstütztes Virtualisierungsmodul (Hypervisor), das Intel Virtualization Technology (VT) verwendet, um die Android-App-Emulation auf einem Hostcomputer zu beschleunigen. In Kombination mit Android x86-Emulatorimages, die von Intel und dem offiziellen Android SDK Manager bereitgestellt werden, ermöglicht HAXM eine schnellere Android-Emulation auf VT-fähigen Systemen. 

Wenn Sie auf einem Computer mit einer Intel CPU entwickeln, der über VT-Funktionen verfügt, können Sie mit HAXM den Android SDK Emulator deutlich beschleunigen. Falls Sie sich nicht sicher sind, ob Ihre CPU VT unterstützt, sehen Sie sich den Artikel [Determine If Your Processor Supports Intel Virtualization Technology (Unterstützt mein Prozessor Intel Virtualization Technology?)](https://www.intel.com/content/www/us/en/support/processors/000005486.html) an.

> [!NOTE]
> Sie können einen Emulator mit VM-Beschleunigung nicht innerhalb einer anderen VM ausführen, z.B. einer VM, die von VirtualBox, VMware oder Docker gehostet wird. Sie müssen den Google Android-Emulator [direkt auf Ihrer Systemhardware](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) ausführen.

Der Android SDK Emulator verwendet HAXM automatisch, wenn es verfügbar ist. Wenn Sie ein **x86**-basiertes virtuelles Gerät auswählen (wie in [Configuration and Use (Konfiguration und Verwendung)](~/android/deploy-test/debugging/android-sdk-emulator/index.md) beschrieben), verwendet dieses virtuelle Gerät HAXM zur Hardwarebeschleunigung. Vor dem ersten Gebrauch des Android SDK Emulators sollten Sie überprüfen, ob HAXM installiert ist und dem Android SDK Emulator zur Verfügung steht.

## <a name="verifying-haxm-installation"></a>Überprüfen der HAXM-Installation

Sehen Sie sich das Fenster **Starting Android Emulator (Android Emulator wird gestartet)** an, während der Emulator gestartet wird, um zu überprüfen, ob HAXM verfügbar ist. So starten Sie den Android SDK Emulator:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten sie den Android-Emulator-Manager, indem Sie auf **Tools > Android > Android-Emulator-Manager** klicken:

    [![Stelle des Menüelements „Android-Emulator-Manager“](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Wenn Ihnen ein Dialogfenster mit einer **Leistungswarnung** wie unten angezeigt wird, ist HAXM noch nicht installiert oder nicht ordnungsgemäß auf Ihrem Computer konfiguriert:

    ![Dialogfeld „Leistungswarnung“, dass HAXM nicht bereit ist](hardware-acceleration-images/win/11-perf-warn.png)

   Wenn ein derartiges Dialogfeld mit einer **Leistungswarnung** angezeigt wird, sehen Sie sich die [Leistungswarnungen](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) an, um den Grund zu bestimmen und das zugrunde liegende Problem zu beheben.

3. Wählen Sie ein **x86**-Image (z.B. **VisualStudio\_android.23\_x86\_phone**), klicken Sie auf **Start** und anschließend auf **Starten**:

    ![Starten des Android SDK Emulators mit einem Standardimage eines virtuellen Geräts](hardware-acceleration-images/win/02-start-default-avd.png)

4. Achten Sie auf das Dialogfeld **Starting Android Emulator** (Android-Emulator wird gestartet), während der Emulator startet. Wenn HAXM installiert ist, wird die Meldung **HAX is working and emulator runs in fast virt mode** (HAXM funktioniert und Emulator wird in schnellen virtuellen Modus ausgeführt.) wie in folgendem Screenshot angezeigt:

    ![HAXM wird im Dialogfeld „Android-Emulator wird gestartet“ als verfügbar angezeigt](hardware-acceleration-images/win/03-haxm-detected.png)

   Wenn Ihnen diese Meldung nicht angezeigt wird, ist HAXM wahrscheinlich nicht installiert. Hier sehen Sie beispielsweise einen Screenshot einer Meldung, die angezeigt, wird, wenn HAXM nicht verfügbar ist:

    ![HAXM ist auf diesem Computer nicht verfügbar](hardware-acceleration-images/win/04-haxm-error.png)

   Wenn HAXM auf Ihrem Computer nicht verfügbar ist, führen Sie die im folgenden Abschnitt dargelegten Schritte durch.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten sie den Android-Emulator-Manager, indem Sie auf **Tools > Google-Emulator-Manager** klicken:

    [![Stelle des Menüelements „Android-Emulator-Manager“](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Wenn Ihnen ein Dialogfenster mit einer **Leistungswarnung** wie unten angezeigt wird, ist HAXM noch nicht installiert oder nicht ordnungsgemäß auf Ihrem Computer konfiguriert:

    ![Dialogfeld „Leistungswarnung“, dass HAXM nicht bereit ist](hardware-acceleration-images/mac/04-avd-warning.png)

   Wenn ein derartiges Dialogfeld mit einer **Leistungswarnung** angezeigt wird, sehen Sie sich die [Leistungswarnungen](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) an, um den Grund zu bestimmen und das zugrunde liegende Problem zu beheben.

3. Wählen Sie das **x86**-Image (z.B. **Android\_Beschleunigt\_x86**), klicken Sie auf **Start** und dann auf **Starten**:

    [![Starten des Android SDK Emulators mit einem Standardimage eines virtuellen Geräts](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Achten Sie auf das Dialogfeld **Starting Android Emulator** (Android-Emulator wird gestartet), während der Emulator startet. Wenn HAXM installiert ist, wird die Meldung **HAX is working and emulator runs in fast virt mode** (HAXM funktioniert und Emulator wird in schnellen virtuellen Modus ausgeführt.) wie in folgendem Screenshot angezeigt:

    ![HAXM wird im Dialogfeld „Android-Emulator wird gestartet“ als verfügbar angezeigt](hardware-acceleration-images/mac/03-haxm-detected.png)

   Wenn HAXM auf Ihrem Computer nicht zur Verfügung steht (wenn Ihnen z.B. eine Fehlermeldung wie die folgende Angezeigt wird: _Please ensure Intel HAXM is properly installed and usable_ (Stellen Sie sicher, dass Intel HAXM ordnungsgemäß installiert wurde und verwendet werden kann)), führen Sie die Schritte im nächsten Abschnitt durch, um HAXM zu installieren.


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>Installieren von HAXM

Wenn der Emulator nicht gestartet wird, muss HAXM möglicherweise manuell installiert werden. HAXM-Installationspakete sowohl für Windows als auch für macOS können über die Seite [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) heruntergeladen werden. Führen Sie die folgenden Schritte durch, um HAXM herunterzuladen und manuell zu installieren:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Laden Sie von der Intel-Website den aktuellsten Windows-Installer [HAXM-Virtualisierungsmodul](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) herunter. Wenn Sie den HAXM-Installer direkt von der Intel-Website herunterladen, können Sie sich sicher sein, dass Sie die aktuellste Version verwenden.

   Alternativ können Sie den SDK Manager verwenden, um den HAXM-Installer herunterzuladen. Klicken Sie dazu im SDK Manager auf **Tools > Extras > Intel x86 Emulator Accelerator (HAXM-Installer)**. Das Android SDK lädt den HAXM-Installer normalerweise an den folgenden Speicherort herunter:

   **C:\\Programme (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   Beachten Sie, dass der SDK-Manager HAXM nicht installiert, sondern lediglich das HAXM-Installationsprogramm am oben genannten Speicherort herunterlädt. Sie müssen diesen trotzdem manuell starten.

2. Führen Sie **intelhaxm-android.exe** aus, um den HAXM-Installer zu starten. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   ![Intel Hardware Accelerated Execution Manager (Setupfenster)](hardware-acceleration-images/win/05-haxm-installer.png)

Wenn Ihnen folgendes Fehlerdialogfeld angezeigt wird (_This computer does not support Intel Virtualization Technology (VT-x) or it is being exclusively used by Hyper-V_ (Dieser Computer unterstützt Intel Virtualization Technology (VT-x) nicht oder er wird ausschließlich von Hyper-V verwendet)), muss Hyper-V deaktiviert werden, bevor Sie HAXM installieren können:

![HAXM cannot be installed due to Hyper-V conflict (HAXM kann aufgrund eines Hyper-V-Konflikts nicht installiert werden).](hardware-acceleration-images/win/06-cant-install-haxm.png)

Im nächsten Abschnitt wird beschrieben, wie Hyper-V deinstalliert werden kann.

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Hyper-V deaktivieren

Wenn Sie Windows mit aktiviertem Hyper-V verwenden, müssen Sie es deaktivieren und Ihren Computer neu starten, um HAXM installieren und verwenden zu können. Sie können Hyper-V über die Systemsteuerung mit den folgenden Schritten deaktivieren:

1. Geben Sie im Windows-Suchfeld **Programme** ein, und klicken Sie anschließend auf das Suchergebnis **Programme und Funktionen**.

2. Wählen Sie über die Systemsteuerung die Option **Programme und Funktionen**, und klicken Sie anschließend auf **Windows-Funktionen ein- oder ausschalten**:

    ![Windows-Features aktivieren oder deaktivieren](hardware-acceleration-images/win/07-turn-windows-features.png)

3. Entfernen Sie das Häkchen bei **Hyper-V**, und starten Sie dann den Computer neu:

    ![Deaktivieren von Hyper-V in den Windows-Funktionen (Dialogfeld)](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

Alternativ können Sie folgendes PowerShell-Cmdlet verwenden, um Hyper-V zu deaktivieren:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM und Microsoft Hyper-V können nicht gleichzeitig aktiviert sein. Es gibt momentan keine Möglichkeit, zwischen Hyper-V und HAXM zu wechseln, ohne den Computer neu starten zu müssen. Wenn Sie den [Visual Studio-Emulator für Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) verwenden möchten, der von Hyper-V abhängig ist, können Sie den Android SDK Emulator nicht ohne Neustart verwenden. Um sowohl Hyper-V als auch HAXM verwenden zu können, erstellen Sie wie unter [Creating a no hypervisor boot entry (Erstellen eines Booteintrags ohne Hypervisor)](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/) beschrieben ein Multibootsetup.

In einigen Fällen wird Hyper-V durch die oben aufgeführten Schritte nicht deaktiviert, wenn Device Guard und Credential Guard aktiviert sind. Wenn Sie Hyper-V nicht deaktivieren können (oder es vermeintlich deaktiviert ist, aber die Installation von HAXM dennoch fehlschlägt), führen Sie die Schritte im nächsten Abschnitt durch, um Device Guard und Credential Guard zu deaktivieren.

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Deaktivieren von Device Guard

Device Guard und Credential Guard können verhindern, dass Hyper-V unter Windows deaktiviert werden kann. Dies stellt häufig ein Problem für mit einer Domäne verknüpfte Computer dar, die von einer besitzenden Organisation konfiguriert und gesteuert werden.
Führen Sie unter Windows 10 die folgenden Schritte aus, um zu prüfen, ob **Device Guard** ausgeführt wird:

1. Geben Sie in **Windows Search** **Systeminfo** ein, um die Anwendung **Systeminformationen** zu starten.

2. Suchen Sie in der **Systemübersicht** nach **Device Guard: virtualisierungsbasierte Sicherheit**, und überprüfen Sie, ob es sich im Status **Wird ausgeführt** befindet:

   [![Device Guard ist vorhanden und wird ausgeführt](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png#lightbox)

Wenn Device Guard aktiviert ist, können Sie es mit den folgenden Schritten deaktivieren:

1. Achten Sie wie im vorherigen Abschnitt beschrieben darauf, dass **Hyper-V** deaktiviert ist (unter **Windows-Features ein- oder ausschalten**).

2. Geben Sie im Windows-Suchfeld **gpedit** ein, und klicken Sie auf das Suchergebnis **Edit group policy** (Gruppenrichtlinie bearbeiten). Dadurch wird der **Editor für lokale Gruppenrichtlinien** gestartet.

3. Navigieren Sie im **Editor für lokale Gruppenrichtlinien** zu **Computerkonfiguration > Administrative Vorlagen > System > Device Guard**:

   [![Device Guard im Editor für lokale Gruppenrichtlinien](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png#lightbox)

4. Ändern Sie **Virtualisierungsbasierte Sicherheit aktivieren** in **Deaktiviert** (wie oben gezeigt), und schließen Sie den **Editor für lokale Gruppenrichtlinien**.

5. Geben Sie im Windows-Suchfeld **cmd** ein. Wenn die **Eingabeaufforderung** in den Suchergebnissen angezeigt wird, klicken Sie mit der rechten Maustaste auf **Eingabeaufforderung** und dann auf **Als Administrator ausführen**.

6. Kopieren Sie die folgenden Befehle und fügen Sie sie in das Eingabeaufforderungsfenster (wenn **Z:** verwendet wird, wählen Sie stattdessen einen nicht verwendeten Laufwerkbuchstaben aus):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Starten Sie den Computer neu. Auf dem Startbildschirm sollten Sie eine Aufforderung wie die folgende sehen:

   **Do you want to disable Credential Guard?** (Möchten Sie Credential Guard deaktivieren?)

   Drücken Sie die angegebene Taste, um Credential Guard wie aufgefordert zu deaktivieren.

8. Nachdem der Computer neu gestartet wurde, stellen Sie erneut sicher, das Hyper-V deaktiviert ist (wie im vorherigen Schritt beschrieben).

Wenn Hyper-V immer noch nicht deaktiviert ist, verhindern die Richtlinien Ihres mit einer Domäne verknüpften Computers möglicherweise das Deaktivieren von Device Guard oder Credential Guard. In diesem Fall können Sie von Ihrem Domänenadministrator eine Ausnahme anfordern, die es Ihnen ermöglicht, Credential Guard zu deaktivieren. Alternativ können Sie einen Computer verwenden, der nicht mit einer Domäne verknüpft ist, um HAXM verwenden zu können.

## <a name="hardware-acceleration-and-amd-cpus"></a>Hardwarebeschleunigung und AMD-CPUs

Da der Android-Emulator von Google die AMD-Hardwarebeschleunigung derzeit [nur unter Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies) unterstützt, ist die Hardwarebeschleunigung für AMD-basierte Computer unter Windows nicht verfügbar.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Laden Sie von der Intel-Website den aktuellsten macOS-Installer [HAXM-Virtualisierungsmodul](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) herunter.

2. Führen Sie den HAXM-Installer aus. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   [![Intel Hardware Accelerated Execution Manager (HAXM)](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----
