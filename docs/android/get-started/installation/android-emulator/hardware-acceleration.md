---
title: Hardwarebeschleunigung für verbesserte Leistung des Emulators (Hyper-V und HAXM)
description: In diesem Artikel wird erläutert, wie Sie die Features für die Hardwarebeschleunigung Ihres Computers verwenden können, um die Leistung des Android-Emulators zu optimieren.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: jondouglas
ms.author: jodou
ms.date: 02/13/2020
ms.openlocfilehash: faab613d88a7f59d1095021d2b21faf9223ae33b
ms.sourcegitcommit: 3fb407841dbe46b8b23573f08591228b7c0e2726
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488909"
---
# <a name="hardware-acceleration-for-emulator-performance-hyper-v--haxm"></a>Hardwarebeschleunigung für verbesserte Leistung des Emulators (Hyper-V und HAXM)

_In diesem Artikel wird erläutert, wie Sie die Features für die Hardwarebeschleunigung Ihres Computers verwenden können, um die Leistung des Android-Emulators zu optimieren._

Visual Studio erleichtert es Entwicklern, ihre Xamarin.Android-Anwendungen mithilfe des Android-Emulators zu testen und zu debuggen, wenn kein Android-Gerät verfügbar oder dessen Verwendung unpraktisch ist.
Der Android-Emulator ist jedoch zu langsam, wenn keine Hardwarebeschleunigung auf dem verwendeten Computer verfügbar ist. Sie können die Leistung des Android-Emulators erheblich steigern, indem Sie spezielle Images für virtuelle x86-Geräte gemeinsam mit den Virtualisierungsfeatures Ihres Computers verwenden.

| Szenario    | HAXM        | WHPX       | Hypervisor.Framework |
| ----------- | ----------- | -----------| ----------- |
| Sie verfügen über einen Intel-Prozessor | X | X | X |
| Sie verfügen über einen AMD-Prozessor   |   | X |   |
| Sie möchten Hyper-V unterstützen |   | X |   |
| Sie möchten geschachtelte Virtualisierung unterstützen |   | Eingeschränkt |   |
| Sie möchten Technologien wie Docker verwenden  |   | X | X |

::: zone pivot="windows"

## <a name="accelerating-android-emulators-on-windows"></a>Beschleunigen des Android-Emulators unter Windows

Ihnen stehen die folgenden Virtualisierungstechnologien zum Beschleunigen des Android-Emulators zur Verfügung:

1. **Microsoft Hyper-V und die Windows Hypervisor-Plattform (WHPX)** .
   [Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/) ist ein Virtualisierungsfeature von Windows, mit dem virtualisierte Computersysteme auf einem physischen Hostcomputer ausgeführt werden können.

2. **Intel Hardware Accelerated Execution Manager (HAXM)**
   Bei HAXM handelt es sich um eine Virtualisierungs-Engine für Computer mit Intel-CPUs.

Für die beste Benutzererfahrung unter Windows empfiehlt es sich, WHPX zum Beschleunigen des Android-Emulators zu verwenden. Wenn WHPX nicht für Ihren Computer verfügbar ist, können Sie HAXM verwenden. Der Android-Emulator verwendet die Hardwarebeschleunigung automatisch, wenn folgende Kriterien erfüllt sind:

- Die Hardwarebeschleunigung ist verfügbar und auf Ihrem Entwicklungscomputer aktiviert.

- Der Emulator führt ein Systemimage aus, das für ein **x86**-basiertes virtuelles Gerät erstellt wurde.

> [!IMPORTANT]
> Sie können einen Emulator mit VM-Beschleunigung nicht innerhalb einer anderen VM ausführen, z.B. einer VM, die von VirtualBox, VMware oder Docker gehostet wird. Sie müssen den Android-Emulator [direkt auf Ihrer Systemhardware](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) ausführen.

Weitere Informationen zum Starten und Debuggen von Apps mit dem Android-Emulator finden Sie unter [Debuggen auf dem Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

<a name="hyper-v-win" />

## <a name="accelerating-with-hyper-v"></a>Beschleunigen mit Hyper-V

Lesen Sie den folgenden Abschnitt, um sicherzustellen, dass Ihr Computer Hyper-V unterstützt, bevor Sie Hyper-V aktivieren.

### <a name="verifying-support-for-hyper-v"></a>Überprüfen der Unterstützung für Hyper-V

Hyper-V wird auf der Windows Hypervisor-Plattform ausgeführt. Ihr Computer muss die folgenden Kriterien zur Unterstützung der Windows Hypervisor-Plattform erfüllen, um den Android-Emulator mit Hyper-V verwenden zu können:

- Ihre Computerhardware muss die folgenden Anforderungen erfüllen:

  - Eine 64-Bit-CPU von Intel oder eine AMD Ryzen-CPU mit SLAT (Second Level Address Translation).
  - CPU-Unterstützung für die Erweiterung für den VM-Überwachungsmodus (VT-c bei Intel-CPUs).
  - Mindestens 4 GB Arbeitsspeicher.

- Im BIOS Ihres Computers müssen die folgenden Optionen aktiviert sein:

  - Virtualisierungstechnologie (Bezeichnung kann je nach Motherboard-Hersteller variieren)
  - Von Hardware erzwungene Datenausführungsverhinderung

- Ihr Computer muss auf das April 2018-Update von Windows 10 (Build 1803) oder höher aktualisiert sein. Sie können überprüfen, ob Ihre Windows-Version aktuell ist, indem Sie die folgenden Schritte ausführen:

  1. Geben Sie **About** (Info) in das Windows-Suchfeld ein.
  2. Wählen Sie **PC-Infos** in den Suchergebnissen aus.
  3. Scrollen Sie im Dialogfeld **Info** nach unten zum Abschnitt **Windows-Spezifikationen**.
  4. Überprüfen Sie, ob mindestens **Version** 1803 vorhanden ist:

      [![Windows-Spezifikationen](hardware-acceleration-images/win/01-about-windows-w10-sml.png)](hardware-acceleration-images/win/01-about-windows-w10.png#lightbox)

Öffnen Sie eine Eingabeaufforderung und geben Sie den folgenden Befehl ein, um zu überprüfen, ob Ihre Computerhardware und -software mit Hyper-V kompatibel ist:

```cmd
systeminfo
```

Wenn alle Hyper-V-Anforderungen den Wert **Yes** (Ja) aufweisen, wird Hyper-V von Ihrem Computer unterstützt. Zum Beispiel:

[![Beispielausgabe der Systeminformationen](hardware-acceleration-images/win/02-systeminfo-w158-sml.png)](hardware-acceleration-images/win/02-systeminfo-w158.png#lightbox)

### <a name="enabling-hyper-v-acceleration"></a>Aktivieren der Hyper-V-Beschleunigung

Wenn Ihr Computer die obigen Kriterien erfüllt, führen Sie die folgenden Schritte aus, um den Android-Emulator mit Hyper-V zu beschleunigen:

1. Geben Sie **windows features** in das Windows-Suchfeld ein, und klicken Sie in den Suchergebnissen auf **Windows-Features aktivieren oder deaktivieren**. Aktivieren Sie im Dialogfeld **Windows-Features** die Optionen **Hyper-V** und **Windows Hypervisor-Plattform**:

    [![Aktivieren von Hyper-V und Windows Hypervisor-Plattform](hardware-acceleration-images/win/03-hyper-v-settings-w158-sml.png)](hardware-acceleration-images/win/03-hyper-v-settings-w158.png#lightbox)

   Nachdem Sie die Änderungen vorgenommen haben, starten Sie den Computer neu.
   
> [!IMPORTANT]
>
> Für das Windows-Update vom 10. Oktober 2018 (RS5) und höhere Versionen müssen Sie nur Hyper-V aktivieren, da hier die Windows-Hypervisor-Plattform (WHPX) automatisch verwendet wird.

2. **Installieren Sie [Visual Studio 15.8 oder höher](https://visualstudio.microsoft.com/vs/)** (diese Version von Visual Studio stellt IDE-Unterstützung zum Ausführen des Android-Emulators mit Hyper-V bereit).

3. **Installieren Sie den Android-Emulator (Paket 27.2.7 oder höher).** Navigieren Sie in Visual Studio zu **Extras > Android > Android SDK-Manager**, um dieses Paket zu installieren. Klicken Sie auf die Registerkarte **Extras**, und vergewissern Sie sich, dass die Version des Android-Emulators mindestens 27.2.7 ist. Stellen Sie ebenfalls sicher, dass Sie Version 26.1.1 oder höher von Android SDK Tools verwenden:

    [![Dialogfeld Android SDKs und Tools](hardware-acceleration-images/win/04-sdk-manager-w158-sml.png)](hardware-acceleration-images/win/04-sdk-manager-w158.png#lightbox)

Wenn Sie ein virtuelles Gerät erstellen (siehe [Verwalten virtueller Geräte mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)), müssen Sie unbedingt ein **x86**-basiertes Systemimage auswählen. Wenn Sie ein ARM-basiertes Systemimage verwenden, wird das virtuelle Gerät nicht beschleunigt, und die Ausführung ist langsam.

## <a name="accelerating-with-haxm"></a>Beschleunigen mit HAXM

Verwenden Sie HAXM zum Beschleunigen des Android-Emulators, wenn Ihr Computer Hyper-V nicht unterstützt. Sie müssen [Device Guard deaktivieren](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#disable-devguard), wenn Sie HAXM verwenden möchten.

### <a name="verifying-haxm-support"></a>Überprüfen der HAXM-Unterstützung

Führen Sie die Schritte unter [Unterstützt mein Prozessor die Intel Virtualisierungstechnik?](https://www.intel.com/content/www/us/en/support/processors/000005486.html) aus, um zu ermitteln, ob Ihre Hardware HAXM unterstützt.
Wenn Ihre Hardware HAXM unterstützt, können Sie überprüfen, ob HAXM bereits installiert ist, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten, und geben Sie den folgenden Befehl ein:

    ```cmd
    sc query intelhaxm
    ```

2. Überprüfen Sie die Ausgabe, um festzustellen, ob der HAXM-Prozess ausgeführt wird. Wenn er ausgeführt wird, wird der Zustand `intelhaxm` in der Ausgabe als `RUNNING` aufgeführt. Zum Beispiel:

    ![Ausgabe des Befehls „sc query“, wenn HAXM verfügbar ist](hardware-acceleration-images/win/05-sc_query-w158.png)

   Wenn `STATE` nicht auf `RUNNING` festgelegt ist, ist HAXM nicht installiert.

Wenn Ihr Computer HAXM unterstützen kann, aber HAXM nicht installiert ist, führen Sie die Schritte im nächsten Abschnitt aus, um HAXM zu installieren.

<a name="install-haxm-win" />

### <a name="installing-haxm"></a>Installieren von HAXM

HAXM-Installationspakete für Windows können über die Seite [Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager) heruntergeladen werden. Führen Sie die folgenden Schritte durch, um HAXM herunterzuladen und zu installieren:

1. Laden Sie von der Intel-Website den aktuellsten Windows-Installer [HAXM-Virtualisierungs-Engine](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/) herunter. Wenn Sie den HAXM-Installer direkt von der Intel-Website herunterladen, können Sie sich sicher sein, dass Sie die aktuellste Version verwenden.

2. Führen Sie **intelhaxm-android.exe** aus, um den HAXM-Installer zu starten. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   ![Intel Hardware Accelerated Execution Manager (Setupfenster)](hardware-acceleration-images/win/06-haxm-installer.png)

Wenn Sie ein virtuelles Gerät erstellen (siehe [Verwalten virtueller Geräte mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)), müssen Sie unbedingt ein **x86**-basiertes Systemimage auswählen. Wenn Sie ein ARM-basiertes Systemimage verwenden, wird das virtuelle Gerät nicht beschleunigt, und die Ausführung ist langsam.

## <a name="troubleshooting"></a>Problembehandlung

Hilfe zur Problembehandlung bei der Hardwarebeschleunigung finden Sie im Handbuch zur [Problembehandlung](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#accel-issues-win) für den Android-Emulator.

::: zone-end
::: zone pivot="macos"

## <a name="accelerating-android-emulators-on-macos"></a>Beschleunigen des Android-Emulators unter macOS

Ihnen stehen die folgenden Virtualisierungstechnologien zum Beschleunigen des Android-Emulators zur Verfügung:

1. **Das Hypervisor-Framework von Apple**.
   [Hypervisor](https://developer.apple.com/documentation/hypervisor) ist ein Feature von macOS 10.10 und höher, das das Ausführen von virtuellen Computern auf einem Mac-Computer ermöglicht.

2. **Intel Hardware Accelerated Execution Manager (HAXM)**
   Bei [HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm) handelt es sich um eine Virtualisierungs-Engine für Computer mit Intel-CPUs.

Es empfiehlt sich, das Hypervisor-Framework zum Beschleunigen des Android-Emulators zu verwenden. Wenn das Hypervisor-Framework auf Ihrem Mac nicht verfügbar ist, können Sie HAXM verwenden. Der Android-Emulator verwendet die Hardwarebeschleunigung automatisch, wenn folgende Kriterien erfüllt sind:

- Die Hardwarebeschleunigung ist verfügbar und auf dem Entwicklungscomputer aktiviert.

- Der Emulator führt ein Systemimage aus, das für ein **x86**-basiertes virtuelles Gerät erstellt wurde.

> [!IMPORTANT]
>
> Sie können einen Emulator mit VM-Beschleunigung nicht innerhalb einer anderen VM ausführen, z.B. einer VM, die von VirtualBox, VMware oder Docker gehostet wird. Sie müssen den Android-Emulator [direkt auf Ihrer Systemhardware](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) ausführen.

Weitere Informationen zum Starten und Debuggen von Apps mit dem Android-Emulator finden Sie unter [Debuggen auf dem Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

<a name="hypervisor" />

## <a name="accelerating-with-the-hypervisor-framework"></a>Beschleunigen mit dem Hypervisor-Framework

Ihr Mac muss die folgenden Kriterien erfüllen, damit der Android-Emulator mit dem Hypervisor-Framework verwendet werden kann:

- Sie benötigen macOS 10.10 oder höher.

- Die CPU Ihres Macs muss das Hypervisor-Framework unterstützen können.

Wenn Ihr Mac die Kriterien erfüllt, verwendet der Android-Emulator automatisch das Hypervisor-Framework für die Beschleunigung. Wenn Sie sich nicht sicher sind, ob das Hypervisor-Framework auf Ihrem Mac unterstützt wird, finden Sie Möglichkeiten zur Überprüfung der Hypervisor-Unterstützung im Handbuch zur [Problembehandlung](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#hypervisor-issues).

Wenn Ihr Mac das Hypervisor-Framework nicht unterstützt, können Sie HAXM zur Beschleunigung des Android-Emulators verwenden (wird im Folgenden beschrieben).

<a name="haxm-mac" />

## <a name="accelerating-with-haxm"></a>Beschleunigen mit HAXM

Wenn Ihr Mac das Hypervisor-Framework nicht unterstützt (oder Sie eine Version vor macOS 10.10 verwenden), können Sie den **Hardware Accelerated Execution Manager von Intel** ([HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm)) zum Beschleunigen den Android-Emulators verwenden.

Vor dem ersten Gebrauch des Android-Emulators mit HAXM sollten Sie überprüfen, ob HAXM installiert ist und dem Android-Emulator zur Verfügung steht.

### <a name="verifying-haxm-support"></a>Überprüfen der HAXM-Unterstützung

Sie können überprüfen, ob HAXM bereits installiert ist, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie ein Terminal, und geben Sie den folgenden Befehl ein:

    ```bash
    ~/Library/Developer/Xamarin/android-sdk-macosx/tools/emulator -accel-check
    ```

   Bei diesem Befehl wird davon ausgegangen, dass das Android SDK am Standardspeicherort installiert ist ( **~/Library/Developer/Xamarin/android-sdk-macosx**). Falls dies nicht der Fall ist, passen Sie den obigen Pfad für den Speicherort des Android SDK auf Ihrem Mac-Computer entsprechend an.

2. Wenn HAXM installiert ist, gibt der obige Befehl eine Meldung zurück, die dem folgenden Ergebnis ähnelt:

    ```bash
    HAXM version 7.2.0 (3) is installed and usable.
    ```

   Wenn HAXM *nicht* installiert ist, wird eine Nachricht zurückgegeben, die der folgenden Ausgabe ähnelt:

    ```bash
    HAXM is not installed on this machine (/dev/HAX is missing).
    ```

Wenn HAXM nicht installiert ist, führen Sie die Schritte im nächsten Abschnitt aus, um HAXM zu installieren.

<a name="install-haxm-mac" />

### <a name="installing-haxm"></a>Installieren von HAXM

HAXM-Installationspakete für macOS können über die Seite [Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager) heruntergeladen werden. Führen Sie die folgenden Schritte durch, um HAXM herunterzuladen und zu installieren:

1. Laden Sie von der Intel-Website den aktuellsten macOS-Installer [HAXM-Virtualisierungs-Engine](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/) herunter.

2. Führen Sie den HAXM-Installer aus. Nehmen Sie die Standardwerte in den Installer-Dialogfeldern an:

   [![Intel Hardware Accelerated Execution Manager (HAXM)](hardware-acceleration-images/mac/01-haxm-installer-sml.png)](hardware-acceleration-images/mac/01-haxm-installer.png#lightbox)

## <a name="troubleshooting"></a>Problembehandlung

Hilfe zur Problembehandlung bei der Hardwarebeschleunigung finden Sie im Handbuch zur [Problembehandlung](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#accel-issues-mac) für den Android-Emulator.

::: zone-end

## <a name="related-links"></a>Verwandte Links

- [Ausführen von Apps auf dem Android-Emulator](https://developer.android.com/studio/run/emulator)
