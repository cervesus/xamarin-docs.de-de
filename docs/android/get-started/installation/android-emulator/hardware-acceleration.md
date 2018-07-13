---
title: Hardwarebeschleunigung für verbesserte Leistung des Emulators
description: In diesem Artikel wird erläutert, wie Sie die Features für die Hardwarebeschleunigung Ihres Computers verwenden können, um die Leistung des Android-Emulators zu optimieren.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: c2bef2c614d4cc0655deb9732ccefec223a8318a
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848466"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Hardwarebeschleunigung für verbesserte Leistung des Emulators

_In diesem Artikel wird erläutert, wie Sie die Features für die Hardwarebeschleunigung Ihres Computers verwenden können, um die Leistung des Android-Emulators zu optimieren._

## <a name="overview"></a>Übersicht

Visual Studio erleichtert es Entwicklern, ihre Xamarin.Android-Anwendungen mithilfe des Android-Emulators zu testen und zu debuggen, wenn kein Android-Gerät verfügbar oder dessen Verwendung unpraktisch ist.
Der Android-Emulator ist jedoch zu langsam, wenn keine Hardwarebeschleunigung auf dem verwendeten Computer verfügbar ist. Sie können die Leistung des Android-Emulators erheblich steigern, indem Sie spezielle Images für virtuelle Geräte verwenden, die x86-Hardware anzielen und eine der folgenden Virtualisierungstechnologien verwenden:

1. **Microsoft Hyper-V und die Hypervisor-Plattform** Hyper-V ist ein Virtualisierungsfeature von Windows, mit dem virtualisierte Computersysteme auf einem physischen Hostcomputer ausgeführt werden können. Dies ist die empfohlene Virtualisierungstechnologie zur Beschleunigung des Android-Emulators. Weitere Informationen zu Hyper-V finden Sie unter [Hyper-V unter Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Intel Hardware Accelerated Execution Manager (HAXM)** 
   Bei HAXM handelt es sich um eine Virtualisierungs-Engine für Computer mit Intel-CPUs.
   Diese Virtualisierungs-Engine wird für Computer empfohlen, die Hyper-V nicht ausführen können.

Der Android-Emulator verwendet die Hardwarebeschleunigung automatisch, wenn folgende Kriterien erfüllt sind:

-   Die Hardwarebeschleunigung ist verfügbar und auf dem Entwicklungscomputer aktiviert.

-   Der Emulator führt ein Emulatorimage aus, das speziell für ein **x86**-basiertes virtuelles Gerät erstellt wurde.

Weitere Informationen zum Starten und Debuggen von Apps mit dem Android-Emulator finden Sie unter [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V-Unterstützung befindet sich derzeit in der Vorschau.

Entwicklern, die Windows 10 (Update vom April 2018 oder höher) verwenden, wird dringend empfohlen, Microsoft Hyper-V zur Beschleunigung des Android-Emulators zu verwenden. So verwenden Sie den Android-Emulator mit Hyper-V:

1. **Führen Sie ein Update auf das Windows 10-Update vom April 2018 (Build 1803) oder höher durch.**
   Klicken Sie in die Cortana-Suchleiste, und geben Sie **Info** ein, um zu überprüfen, welche Version von Windows ausgeführt wird. Wählen Sie **PC-Infos** in den Suchergebnissen aus. Scrollen Sie im Dialogfeld **Info** nach unten zum Abschnitt **Windows-Spezifikationen**. Die **Version** muss mindestens 1803 sein:

    [![Windows-Spezifikationen](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Aktivieren Sie die Windows Hypervisor-Plattform.**
   Geben Sie in der Cortana-Suchleiste **Windows-Features aktivieren oder deaktivieren** ein.
   Scrollen Sie im Dialogfeld **Windows-Features** nach unten, und vergewissern Sie sich, dass **Windows Hypervisor-Plattform** aktiviert ist.

    [![Windows Hypervisor-Plattform ist aktiviert](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Durch das Aktivieren der **Windows Hypervisor-Plattform** wird Hyper-V automatisch aktiviert. Sie sollten Windows neu starten, nachdem Sie diese Änderung vorgenommen haben.

3. **Installieren Sie [Visual Studio 15.8 Preview 1 oder höher](https://visualstudio.microsoft.com/vs/preview/).**
   Diese Version von Visual Studio bietet IDE-Unterstützung für das Ausführen des Android-Emulators mit Hyper-V.
 
4. **Installieren Sie den Android-Emulator (Paket 27.2.7 oder höher).** Navigieren Sie in Visual Studio zu **Extras > Android > Android SDK-Manager**, um dieses Paket zu installieren. Klicken Sie auf die Registerkarte **Extras**, und vergewissern Sie sich, dass die Version des Android-Emulators mindestens 27.2.7 ist. Stellen Sie ebenfalls sicher, dass Sie Version 26.1.1 oder höher von Android SDK Tools verwenden:

    [![Dialogfeld Android SDKs und Tools](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Wenn die Emulatorversion mindestens 27.2.7 aber kleiner als 27.3.1 ist, ist die folgende Problemumgehung erforderlich, um Hyper-V verwenden zu können:

    1.  Erstellen Sie im Ordner **C:\\Benutzer\\_Benutzername_\\.android** eine Datei namens **advancedFeatures.ini**, sofern diese nicht bereits vorhanden ist.

    2.  Fügen Sie die folgende Zeile zu **advancedFeatures.ini** hinzu:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Bekannte Probleme

-   Wenn Sie kein Update auf Version 27.2.7 oder höher des Emulators durchführen können, nachdem Sie ein Update auf eine Vorschauversion von Visual Studio durchgeführt haben, müssen Sie den [Preview-Installer](http://aka.ms/hyperv-emulator-dl) direkt installieren, um neuere Emulatorversionen zu aktivieren.

-   Bei Verwendung bestimmter Intel- und AMD-basierter Prozessoren kann es zu Leistungseinbußen kommen.

-   Das Laden der Android-Anwendung kann bei der Bereitstellung extrem lange dauern.

-   Ein MMIO-Zugriffsfehler kann das Booten des Android-Emulators sporadisch verhindern. Ein Neustart des Emulators kann dieses Problem beheben.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Hyper-V-Unterstützung erfordert Windows 10. Weitere Details finden Sie unter [Hyper-V-Anforderungen](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements).

-----

## <a name="haxm"></a>HAXM

HAXM ist eine hardwareunterstützte Virtualisierungs-Engine (Hypervisor), das Intel Virtualization Technology (VT) verwendet, um die Android-App-Emulation auf einem Hostcomputer zu beschleunigen. Wenn Sie HAXM in Kombination mit von Intel bereitgestellten x86-Emulatorimages für Android verwenden, können Android-Emulationen auf VT-fähigen Systemen schneller ausgeführt werden.

Wenn Sie zur Entwicklung einen Computer mit einer Intel-CPU verwenden, die VT nutzen kann, können Sie mit HAXM den Android-Emulator deutlich beschleunigen. Falls Sie sich nicht sicher sind, ob Ihre CPU VT unterstützt, finden Sie unter [Unterstützt mein Prozessor die Intel® Virtualisierungstechnik?](https://www.intel.com/content/www/us/en/support/processors/000005486.html) weitere Informationen.

> [!NOTE]
> Sie können einen Emulator mit VM-Beschleunigung nicht innerhalb einer anderen VM ausführen, z.B. einer VM, die von VirtualBox, VMware oder Docker gehostet wird. Sie müssen den Android-Emulator [direkt auf Ihrer Systemhardware](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) ausführen.

Vor dem ersten Gebrauch des Android-Emulators sollten Sie überprüfen, ob HAXM installiert ist und dem Android-Emulator zur Verfügung steht.

### <a name="verifying-haxm-installation"></a>Überprüfen der HAXM-Installation

Sehen Sie sich das Fenster **Starting Android Emulator (Android Emulator wird gestartet)** an, während der Emulator gestartet wird, um zu überprüfen, ob HAXM verfügbar ist. So starten Sie den Android-Emulator:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Android Device Manager, indem Sie auf **Extras > Android > Android Device Manager** klicken:

    [![Position des Menüelements „Android Device Manager“](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Wenn Ihnen ein Dialogfenster mit einer **Leistungswarnung** wie unten angezeigt wird, ist HAXM noch nicht installiert oder nicht ordnungsgemäß auf Ihrem Computer konfiguriert:

    ![Dialogfeld „Leistungswarnung“, dass HAXM nicht bereit ist](hardware-acceleration-images/win/11-perf-warn.png)

   Wenn ein derartiges Dialogfeld mit einer **Leistungswarnung** angezeigt wird, sehen Sie sich die [Leistungswarnungen](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) an, um den Grund zu bestimmen und das zugrunde liegende Problem zu beheben.

3. Wählen Sie ein **x86**-Image (z.B. **VisualStudio\_android.23\_x86\_phone**) aus, und klicken Sie auf **Start**:

    ![Starten des Android-Emulators mit einem Standardimage eines virtuellen Geräts](hardware-acceleration-images/win/02-start-default-avd.png)

4. Achten Sie auf das Dialogfeld **Starting Android Emulator** (Android-Emulator wird gestartet), während der Emulator startet. Wenn HAXM installiert ist, wird die Meldung **HAX is working and emulator runs in fast virt mode** (HAXM funktioniert und Emulator wird in schnellen virtuellen Modus ausgeführt.) wie in folgendem Screenshot angezeigt:

    ![HAXM wird im Dialogfeld „Android-Emulator wird gestartet“ als verfügbar angezeigt](hardware-acceleration-images/win/03-haxm-detected.png)

   Wenn Ihnen diese Meldung nicht angezeigt wird, ist HAXM wahrscheinlich nicht installiert. Hier sehen Sie beispielsweise einen Screenshot einer Meldung, die angezeigt, wird, wenn HAXM nicht verfügbar ist:

    ![HAXM ist auf diesem Computer nicht verfügbar](hardware-acceleration-images/win/04-haxm-error.png)

   Wenn HAXM auf Ihrem Computer nicht verfügbar ist, führen Sie die im folgenden Abschnitt dargelegten Schritte durch.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Android Device Manager, indem Sie auf **Extras > Android Device Manager** klicken:

    [![Position des Menüelements „Android Device Manager“](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Wenn Ihnen ein Dialogfenster mit einer **Leistungswarnung** wie unten angezeigt wird, ist HAXM noch nicht installiert oder nicht ordnungsgemäß auf Ihrem Computer konfiguriert:

    ![Dialogfeld „Leistungswarnung“, dass HAXM nicht bereit ist](hardware-acceleration-images/mac/04-avd-warning.png)

   Wenn ein derartiges Dialogfeld mit einer **Leistungswarnung** angezeigt wird, sehen Sie sich die [Leistungswarnungen](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) an, um den Grund zu bestimmen und das zugrunde liegende Problem zu beheben.

3. Wählen Sie das **x86**-Image (z.B. **Android\_Accelerated\_x86**), und klicken Sie auf **Starten**:

    [![Starten des Android-Emulators mit einem Standardimage eines virtuellen Geräts](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

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

Da der Android-Emulator die AMD-Hardwarebeschleunigung derzeit [nur unter Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies) unterstützt, ist die Hardwarebeschleunigung für AMD-basierte Computer unter Windows nicht verfügbar.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Laden Sie von der Intel-Website den aktuellsten macOS-Installer [HAXM-Virtualisierungs-Engine](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) herunter.

2. Führen Sie den HAXM-Installer aus. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   [![Intel Hardware Accelerated Execution Manager (HAXM)](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Verwandte Links

* [Ausführen von Apps auf dem Android-Emulator](https://developer.android.com/studio/run/emulator)
