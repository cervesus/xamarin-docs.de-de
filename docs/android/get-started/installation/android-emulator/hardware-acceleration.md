---
title: Hardwarebeschleunigung für Android-Emulator
description: So bereiten Sie Ihren Computer auf die höchstmögliche Leistung des Google Android-Emulators vor
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/10/2018
ms.openlocfilehash: 2f0bb6f1371b9ce1b925b876851d58f3c4d01419
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Hardwarebeschleunigung für Android-Emulator

Der Google Android-Emulator ist ohne Hardwarebeschleunigung viel zu langsam. Die Leistung des Google Android-Emulators lässt sich durch Verwendung spezieller Emulatorhardwareimages, die auf x86-Hardware und eine von zwei Virtualisierungstechnologien abzielen, drastisch verbessern:

1. **Hyper-V und die Hypervisor-Plattform von Microsoft** &ndash; Hyper-V ist eine Virtualisierungskomponente, die unter Windows 10 verfügbar ist und das Ausführen virtualisierter Computersysteme auf einem physischen Host ermöglicht. Dies ist die empfohlene Virtualisierungstechnologie für die beschleunigten Google Android-Emulatorimages. Um mehr über Hyper-V zu erfahren, konsultieren Sie das Handbuch [Hyper-V unter Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).
2. **Hardware Accelerated Execution Manager (HAXM) von Intel** &ndash; Dies ist eine Virtualisierungs-Engine für Computer mit Intel-CPUs. Diese Virtualisierungs-Engine wird Entwicklern empfohlen, die Hyper-V nicht verwenden können.

Der Android SDK-Manager nutzt automatisch die Hardwarebeschleunigung, wenn sie verfügbar ist und ein Emulatorimage speziell für ein **x86**-basiertes virtuelles Gerät ausführt (wie in [Konfiguration und Verwendung](~/android/deploy-test/debugging/android-sdk-emulator/index.md) beschrieben).

## <a name="hyper-v-overview"></a>Übersicht über Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V-Unterstützung befindet sich derzeit in der Vorschau.

Entwicklern, die Windows 10 (Update von April 2018) verwenden, wird dringend empfohlen, Hyper-V von Microsoft zu verwenden. Die Visual Studio Tools für Xamarin erleichtern es Entwicklern, ihre Xamarin.Android-Anwendungen zu testen und zu debuggen, wenn kein Android-Gerät verfügbar oder dessen Verwendung unpraktisch ist.

Erste Schritte mit Hyper-V und dem Google Android-Emulator:

1. **Update auf Windows 10-Update von April 2018 (Build 1803)** &ndash; Um zu überprüfen, welche Version von Windows ausgeführt wird, klicken Sie in die Cortana-Suchleiste und geben Sie **Info** ein. Wählen Sie **PC-Infos** in den Suchergebnissen aus. Blättern Sie im **Info**-Dialogfeld nach unten zum Abschnitt **Windows-Spezifikationen**. Die **Version** muss mindestens 1803 sein:

    [![Windows-Spezifikationen](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Aktivieren Sie Hyper-V und die Windows Hypervisor-Plattform** &ndash; Geben Sie **Windows-Features aktivieren oder deaktivieren** in die Cortana-Suchleiste ein.
   Blättern Sie im Dialogfeld **Windows-Features** nach unten, und vergewissern Sie sich, dass **Windows Hypervisor-Plattform** aktiviert ist.

    [![Hyper-V und Windows Hypervisor-Plattform sind aktiviert](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Nach der Aktivierung von Hyper-V und der Windows Hypervisor-Plattform müssen Sie Windows möglicherweise neu starten.

3. **Installieren Sie [Visual Studio 15.8 Preview 1 oder höher](https://www.visualstudio.com/vs/preview/)** &ndash; Diese Version von Visual Studio bietet IDE-Unterstützung zum Starten des Google Android-Emulators mit Hyper-V-Unterstützung.

4. **Installieren Sie das Google Android-Emulatorpaket, Version 27.2.7 oder höher** &ndash; Zum Installieren dieses Pakets navigieren Sie zu **Extras > Android > Android SDK-Manager** in Visual Studio. Wählen Sie die Registerkarte **Extras** aus, und vergewissern Sie sich, dass die Android-Emulatorkomponente mindestens in Version 27.2.7 vorliegt.

    [![Dialogfeld Android SDKs und Tools](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Wenn die Android-Emulatorversion kleiner als 27.3.1 ist, wenden Sie den zusätzlichen Problemumgehungsschritt an, der im nächsten Abschnitt unter **Bekannte Probleme**  erklärt wird.


### <a name="known-issues"></a>Bekannte Probleme

-   Wenn die Emulatorversion mindestens 27.2.7 aber kleiner als 27.3.1 ist, ist die folgende Problemumgehung erforderlich, um Hyper-V verwenden zu können:
    1.  Erstellen Sie im Ordner **C:\\Users\\_Benutzername_\\.android** eine Datei mit dem Namen **advancedFeatures.ini**, sofern sie nicht bereits vorhanden ist.
    2.  Fügen Sie die folgende Zeile zu **advancedFeatures.ini** hinzu:
        ```
        WindowsHypervisorPlatform = on
        ```

-   Bei Verwendung bestimmter Intel- und AMD-basierter Prozessoren kann es zu Leistungseinbußen kommen.

-   Das Laden der Android-Anwendung kann bei der Bereitstellung extrem lange dauern.

-   Ein MMIO-Zugriffsfehler kann das Booten des Android-Emulators sporadisch verhindern. Ein Neustart des Emulators kann dieses Problem beheben.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Hyper-V-Unterstützung erfordert Windows 10. Weitere Details finden Sie unter [Hyper-V-Anforderungen](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements).

-----

## <a name="haxm-overview"></a>HAXM-Übersicht

HAXM ist eine hardwareunterstützte Virtualisierungs-Engine (Hypervisor), das Intel Virtualization Technology (VT) verwendet, um die Android-App-Emulation auf einem Hostcomputer zu beschleunigen. In Kombination mit Android x86-Emulatorimages, die von Intel und dem offiziellen Android SDK Manager bereitgestellt werden, ermöglicht HAXM eine schnellere Android-Emulation auf VT-fähigen Systemen. 

Wenn Sie auf einem Computer mit einer Intel CPU entwickeln, der über VT-Funktionen verfügt, können Sie mit HAXM den Google Android-Emulator deutlich beschleunigen. Falls Sie sich nicht sicher sind, ob Ihre CPU VT unterstützt, sehen Sie sich den Artikel [Determine If Your Processor Supports Intel Virtualization Technology (Unterstützt mein Prozessor Intel Virtualization Technology?)](https://www.intel.com/content/www/us/en/support/processors/000005486.html) an.

> [!NOTE]
> Sie können einen Emulator mit VM-Beschleunigung nicht innerhalb einer anderen VM ausführen, z.B. einer VM, die von VirtualBox, VMware oder Docker gehostet wird. Sie müssen den Google Android-Emulator [direkt auf Ihrer Systemhardware](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) ausführen.

Vor dem ersten Gebrauch des Google Android-Emulators sollten Sie überprüfen, ob HAXM installiert ist und dem Google Android-Emulator zur Verfügung steht.

### <a name="verifying-haxm-installation"></a>Überprüfen der HAXM-Installation

Sehen Sie sich das Fenster **Starting Android Emulator (Android Emulator wird gestartet)** an, während der Emulator gestartet wird, um zu überprüfen, ob HAXM verfügbar ist. So starten Sie den Google Android-Emulator:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten sie den Android-Emulator-Manager, indem Sie auf **Tools > Android > Android-Emulator-Manager** klicken:

    [![Stelle des Menüelements „Android-Emulator-Manager“](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Wenn Ihnen ein Dialogfenster mit einer **Leistungswarnung** wie unten angezeigt wird, ist HAXM noch nicht installiert oder nicht ordnungsgemäß auf Ihrem Computer konfiguriert:

    ![Dialogfeld „Leistungswarnung“, dass HAXM nicht bereit ist](hardware-acceleration-images/win/11-perf-warn.png)

   Wenn ein derartiges Dialogfeld mit einer **Leistungswarnung** angezeigt wird, sehen Sie sich die [Leistungswarnungen](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) an, um den Grund zu bestimmen und das zugrunde liegende Problem zu beheben.

3. Wählen Sie ein **x86**-Image (z.B. **VisualStudio\_android.23\_x86\_phone**), klicken Sie auf **Start** und anschließend auf **Starten**:

    ![Starten des Google Android-Emulators mit einem Standardimage eines virtuellen Geräts](hardware-acceleration-images/win/02-start-default-avd.png)

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

    [![Starten des Google Android-Emulators mit einem Standardimage eines virtuellen Geräts](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Achten Sie auf das Dialogfeld **Starting Android Emulator** (Android-Emulator wird gestartet), während der Emulator startet. Wenn HAXM installiert ist, wird die Meldung **HAX is working and emulator runs in fast virt mode** (HAXM funktioniert und Emulator wird in schnellen virtuellen Modus ausgeführt.) wie in folgendem Screenshot angezeigt:

    ![HAXM wird im Dialogfeld „Android-Emulator wird gestartet“ als verfügbar angezeigt](hardware-acceleration-images/mac/03-haxm-detected.png)

   Wenn HAXM auf Ihrem Computer nicht zur Verfügung steht (wenn Ihnen z.B. eine Fehlermeldung wie die folgende Angezeigt wird: _Please ensure Intel HAXM is properly installed and usable_ (Stellen Sie sicher, dass Intel HAXM ordnungsgemäß installiert wurde und verwendet werden kann)), führen Sie die Schritte im nächsten Abschnitt durch, um HAXM zu installieren.


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Installieren von HAXM

Wenn der Emulator nicht gestartet wird, muss HAXM möglicherweise manuell installiert werden. HAXM-Installationspakete sowohl für Windows als auch für macOS können über die Seite [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) heruntergeladen werden. Führen Sie die folgenden Schritte durch, um HAXM herunterzuladen und manuell zu installieren:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Laden Sie von der Intel-Website den aktuellsten Windows-Installer [HAXM-Virtualisierungs-Engine](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) herunter. Wenn Sie den HAXM-Installer direkt von der Intel-Website herunterladen, können Sie sich sicher sein, dass Sie die aktuellste Version verwenden.

   Alternativ können Sie den SDK Manager verwenden, um den HAXM-Installer herunterzuladen. Klicken Sie dazu im SDK Manager auf **Tools > Extras > Intel x86 Emulator Accelerator (HAXM-Installer)**. Das Android SDK lädt den HAXM-Installer normalerweise an den folgenden Speicherort herunter:

   **C:\\Programme (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   Beachten Sie, dass der SDK-Manager HAXM nicht installiert, sondern lediglich das HAXM-Installationsprogramm am oben genannten Speicherort herunterlädt. Sie müssen diesen trotzdem manuell starten.

2. Führen Sie **intelhaxm-android.exe** aus, um den HAXM-Installer zu starten. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   ![Intel Hardware Accelerated Execution Manager (Setupfenster)](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Hardwarebeschleunigung und AMD-CPUs

Da der Android-Emulator von Google die AMD-Hardwarebeschleunigung derzeit [nur unter Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies) unterstützt, ist die Hardwarebeschleunigung für AMD-basierte Computer unter Windows nicht verfügbar.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Laden Sie von der Intel-Website den aktuellsten macOS-Installer [HAXM-Virtualisierungs-Engine](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) herunter.

2. Führen Sie den HAXM-Installer aus. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   [![Intel Hardware Accelerated Execution Manager (HAXM)](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Verwandte Links

* [Ausführen von Apps auf dem Android-Emulator](https://developer.android.com/studio/run/emulator)
