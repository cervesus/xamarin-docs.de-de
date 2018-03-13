---
title: Behandlung von Problemen des Android SDK-Emulators
description: So erkennen Sie Probleme des Android SDK Emulators und beheben diese
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 486df3bbee3f8af511140e2d287f9f95571c7b3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Behandlung von Problemen des Android SDK-Emulators

In diesem Artikel werden die häufigsten Warnmeldungen und Probleme des Android SDK-Emulators und entsprechende Lösungen vorgestellt.
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>Leistungswarnungen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ab Visual Studio 2017 Version 15.4 wird möglicherweise ein Leistungsdialogfeld angezeigt, wenn Sie Ihre App zum ersten Mal im Android SDK Emulator bereitstellen. Diese Warnungsdialogfelder werden unten erklärt.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Computer Does Not Contain an Intel Processor (Der Computer enthält keinen Intel-Prozessor).

![Computer Does Not Contain an Intel Processor (Der Computer enthält keinen Intel-Prozessor).](troubleshooting-images/01-no-intel-processor.png)

Wenn dieses Dialogfeld angezeigt wird, verfügt Ihr Computer über einen Prozessor, der kein Intel-Prozessor ist. Dieser ist jedoch für die Beschleunigung des Android SDK Emulators erforderlich. Wenn Ihr Computer nicht über einen Intel-Prozessor verfügt, wird empfohlen, dass Sie ein physisches Android-Gerät für die Entwicklung verwenden.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V Is Installed or Active (Hyper-V ist installiert oder aktiv).

![Hyper-V Is Installed or Active (Hyper-V ist installiert oder aktiv).](troubleshooting-images/02-hyper-v-active.png)

Wenn dieses Dialogfeld angezeigt wird, ist Hyper-V installiert oder aktiv und muss deaktiviert werden. In [Disabling Hyper-V (Deaktivieren von Hyper-V)](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) wird beschrieben, wie Sie dieses Problem beheben können. 

### <a name="haxm-is-not-installed"></a>HAXM is Not Installed (HAXM ist nicht installiert).

![HAXM is Not Installed (HAXM ist nicht installiert).](troubleshooting-images/03-haxm-not-installed.png)

Dieses Dialogfeld gibt an, dass Ihr Computer zwar über einen Intel-Prozessor verfügt und Hyper-V deaktiviert ist, dass jedoch HAXM nicht installiert ist.
In [Installing HAXM (Installieren von HAXM)](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) wird beschrieben, wie Sie HAXM installieren können.

### <a name="haxm-process-not-running"></a>HAXM process is not running (HAXM-Prozess wird nicht ausgeführt).

![HAXM process is not running (HAXM-Prozess wird nicht ausgeführt).](troubleshooting-images/04-haxm-process-not-running.png)

Dieses Dialogfeld wird angezeigt, wenn Ihr Computer zwar über einen Intel-Prozessor verfügt und Intel HAXM installiert ist, aber der HAXM-Prozess nicht ausgeführt wird. Um dieses Problem zu beheben, öffnen Sie ein Befehlszeilenfenster mit erhöhten Rechten, und geben Sie den folgenden Befehl ein:

```cmd
sc query intelhaxm
```

Wenn der HAXM-Prozess ausgeführt wird, sollten Sie diese oder eine ähnlich Ausgaben erhalten:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


Wenn der **Status** nicht auf **Wird ausgeführt** festgelegt ist, sehen Sie sich [How to Use the Intel Hardware Accelerated Execution Manager (So verwenden Sie Intel Hardware Accelerated Execution Manager (HAXM))](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) an, um das Problem zu beheben.


### <a name="other-failures"></a>Andere Fehler

![Andere Fehler](troubleshooting-images/05-other-failure.png)

Dieses Dialogfeld wird angezeigt, wenn Ihr Computer zwar über einen Intel-Prozessor verfügt und Intel HAXM installiert ist, der HAXM-Prozess ausgeführt wird, aber der Emulator aus unbekannten Gründen nicht gestartet werden kann.
Um dieses Problem zu beheben, schauen Sie sich folgenden Artikel an: [How to Use the Intel Hardware Accelerated Execution Manager (So verwenden Sie Intel Hardware Accelerated Execution Manager (HAXM))](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Deaktivieren von Leistungswarnungen

Wenn Sie es vorziehen, keine Leistungswarnungen zu erhalten, können Sie diese deaktivieren. Klicken Sie in Visual Studio auf **Extras > Optionen > Xamarin > Android-Einstellungen**, und deaktivieren Sie die Option **Warn if AVD acceleration is not supported (HAXM)** (Warnen, wenn AVD-Beschleunigung nicht unterstützt wird (HAXM)):

[![Deaktivieren von Warnungen zu AVD-Beschleunigung](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Ab Visual Studio für Mac, Build 7.2 (Build 559) oder höher wird möglicherweise ein Leistungsdialogfeld angezeigt, wenn Sie Ihre App zum ersten Mal im Android SDK-Emulator bereitstellen. Diese Warnungsdialogfelder werden unten erklärt.

### <a name="haxm-is-not-installed"></a>HAXM is Not Installed (HAXM ist nicht installiert).

![HAXM is Not Installed (HAXM ist nicht installiert).](troubleshooting-images/03-haxm-not-installed.png)

Dieses Dialogfeld gibt an, dass HAXM nicht installiert ist.
In [Installing HAXM (Installieren von HAXM)](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) wird beschrieben, wie Sie HAXM installieren können.

### <a name="haxm-process-not-running"></a>HAXM process is not running (HAXM-Prozess wird nicht ausgeführt).

![HAXM process is not running (HAXM-Prozess wird nicht ausgeführt).](troubleshooting-images/04-haxm-process-not-running.png)

Dieses Dialogfeld wird angezeigt, wenn der HAXM-Prozess nicht ausgeführt wird. Ausführliche Informationen zum Beheben dieses Problems finden Sie in folgendem Artikel: [How to Use the Intel Hardware Accelerated Execution Manager (So verwenden Sie Intel Hardware Accelerated Execution Manager (HAXM))](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="other-failures"></a>Andere Fehler

![Andere Fehler](troubleshooting-images/05-other-failure.png)

Dieses Dialogfeld wird angezeigt, wenn der Emulator aus unbekannten Gründen nicht gestartet werden kann. Schauen Sie sich folgenden Artikel an, um dieses Problem zu lösen: [How to Use the Intel Hardware Accelerated Execution Manager (So verwenden Sie Intel Hardware Accelerated Execution Manager (HAXM))](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

-----


## <a name="solutions-to-common-problems"></a>Lösungen für häufige Probleme

Viele häufig auftretende Probleme mit dem Android SDK Emulator können behoben werden, indem Sie die Konfigurationen Ihres Computers ändern oder zusätzliche Software installieren. Im folgenden Abschnitt werden diese Probleme sowie mögliche Lösungen beschrieben.


### <a name="deployment-issues"></a>Probleme bei der Bereitstellung

Wenn Sie eine Fehlermeldung erhalten, dass das APK auf dem Emulator nicht installiert werden konnte, oder dass Android Debug Bridge (**adb**) nicht ausgeführt werden konnte, stellen Sie sicher, dass das Android SDK eine Verbindung mit Ihrem Emulator herstellen kann. Gehen Sie dazu folgendermaßen vor:

1. Starten Sie den Emulator über den **AVD-Manager** (Android Virtual Device). Klicken Sie dazu auf Ihr virtuelles Gerät und anschließend auf **Start**.

2. Öffnen Sie eine Eingabeaufforderung, und wechseln Sie zum Ordner, in dem **adb** installiert ist. Unter Windows kann dies z.B. **C:\\Programme (x86)\\Android\\android-sdk\\platform-tools\\adb.exe** sein.

3. Geben Sie folgenden Befehl ein:

   ```shell
   adb devices
   ```

4. Wenn auf den Emulator vom Android SDK aus zugegriffen werden kann, sollte der Emulator in der Liste der angehängten Geräte aufgelistet sein. Zum Beispiel:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Wenn der Emulator nicht in dieser Liste angezeigt wird, starten Sie den **Android SDK Manager**, wenden Sie alle Updates an, und versuchen Sie erneut, den Emulator zu starten.



### <a name="haxm-issues"></a>Probleme mit HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn der Android SDK Emulator nicht ordnungsgemäß gestartet wird, liegt dies häufig an Problemen mit HAXM. Probleme mit HAXM werden häufig durch Konflikte mit anderen Virtualisierungstechnologien, durch falsche Einstellungen oder durch einen veralteten HAXM-Treiber verursacht.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM-Virtualisierungskonflikte

HAXM kann mit anderen Technologien, die die Virtualisierung verwenden, in Konflikt stehen, wie etwa Hyper-V, Windows Device Guard und einige Antivirussoftware:

- **Hyper-V** &ndash; Wenn Sie Windows mit aktiviertem Hyper-V verwenden, führen Sie die Schritte in [Deaktivieren von Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) durch.

- **Device Guard** &ndash; Device Guard und Credential Guard können verhindern, dass Hyper-V unter Windows deaktiviert werden kann. Informationen zum Deaktivieren von Device Guard und Credential Guard finden Sie unter [Deaktivieren von Device Guard](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard).

- **Antivirussoftware** &ndash; Wenn Sie eine Antivirussoftware ausführen, die die hardwareunterstützte Virtualisierung verwendet, wie etwa Avast, deaktivieren oder deinstallieren Sie diese Software, starten Sie den Computer neu, und versuchen Sie erneut, den Android SDK Emulator zu starten.


#### <a name="incorrect-bios-settings"></a>Falsche BIOS-Einstellungen

Wenn Sie HAXM auf einem Windows-PC verwenden, funktioniert HAXM nur dann, wenn Virtualization Technology (Intel VT-x) im BIOS aktiviert ist. Wenn VT-x deaktiviert ist, erhalten Sie eine Fehlermeldung wie die folgende, wenn Sie versuchen, den Android SDK Emulator zu starten:

**This computer meets the requirements for HAXM, but Intel Virtualization Technology (VT-x) is not turned on.** (Dieser Computer erfüllt die HAXM-Anforderungen, aber Intel Virtualization Technology ist nicht aktiviert).

Um diesen Fehler zu korrigieren, starten Sie den Computer im BIOS, aktivieren Sie sowohl VT-x als auch SLAT (Second Level Address Translation), und starten Sie den Computer dann wieder unter Windows.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn der Android SDK Emulator nicht ordnungsgemäß gestartet wird, liegt dies häufig an Problemen mit HAXM. Probleme mit HAXM werden häufig durch Konflikte mit anderen Virtualisierungstechnologien, durch falsche Einstellungen oder durch einen veralteten HAXM-Treiber verursacht. Installieren Sie den HAXM-Treiber erneut. Die dazu erforderlichen Schritte werden unter [Installieren von HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) beschrieben.

-----
