---
title: Android-Emulator-Problembehandlung
description: In diesem Artikel wird erläutert, wie Sie Probleme diagnostizieren und umgehen, die bei der Verwendung des Android-Emulators auftreten können.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 241f38cbfe013776b2e36b8102ae4b90cf610d80
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935281"
---
# <a name="android-emulator-troubleshooting"></a>Android-Emulator-Problembehandlung

_In diesem Artikel werden die gängigsten Warnmeldungen und Probleme beschrieben, die beim Konfigurieren und Ausführen des Android-Emulators auftreten können. Außerdem finden Sie hier Tipps und Hinweise zum Umgehen von Problemen._

<a name="perfwarn" />

## <a name="performance-warnings"></a>Leistungswarnungen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ab Visual Studio 2017 Version 15.4 wird möglicherweise ein Dialogfeld mit einer Leistungswarnung angezeigt, wenn Sie Ihre App zum ersten Mal im Android-Emulator bereitstellen. Diese Warnungsdialogfelder werden unten erklärt.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Computer Does Not Contain an Intel Processor (Der Computer enthält keinen Intel-Prozessor).

![Computer Does Not Contain an Intel Processor (Der Computer enthält keinen Intel-Prozessor).](troubleshooting-images/01-no-intel-processor.png)

Wenn dieses Dialogfeld angezeigt wird, verfügt Ihr Computer über einen Prozessor, der kein Intel-Prozessor ist. Dieser ist jedoch für die Beschleunigung des Android SDK Emulators erforderlich. Wenn Ihr Computer nicht über einen Intel-Prozessor verfügt, wird empfohlen, dass Sie ein physisches Android-Gerät für die Entwicklung verwenden.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V Is Installed or Active (Hyper-V ist installiert oder aktiv).

![Hyper-V Is Installed or Active (Hyper-V ist installiert oder aktiv).](troubleshooting-images/02-hyper-v-active.png)

Wenn dieses Dialogfeld angezeigt wird, ist Hyper-V installiert oder aktiv und muss deaktiviert werden. In [Disabling Hyper-V (Deaktivieren von Hyper-V)](#disable-hyperv) wird beschrieben, wie Sie dieses Problem beheben können.

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

Wenn `STATE` nicht auf `RUNNING` festgelegt ist, erfahren Sie unter [How to Use the Intel Hardware Accelerated Execution Manager (Verwenden von Intel Hardware Accelerated Execution Manager)](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator), wie Sie das Problem beheben.


### <a name="other-failures"></a>Andere Fehler

![Andere Fehler](troubleshooting-images/05-other-failure.png)

Dieses Dialogfeld wird angezeigt, wenn Ihr Computer zwar über einen Intel-Prozessor verfügt und Intel HAXM installiert ist, der HAXM-Prozess ausgeführt wird, aber der Emulator aus unbekannten Gründen nicht gestartet werden kann.
Um dieses Problem zu beheben, schauen Sie sich folgenden Artikel an: [How to Use the Intel Hardware Accelerated Execution Manager (So verwenden Sie Intel Hardware Accelerated Execution Manager (HAXM))](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Deaktivieren von Leistungswarnungen

Wenn Sie es vorziehen, keine Leistungswarnungen zu erhalten, können Sie diese deaktivieren. Klicken Sie in Visual Studio auf **Extras > Optionen > Xamarin > Android-Einstellungen**, und deaktivieren Sie die Option **Warn if AVD acceleration is not supported (HAXM)** (Warnen, wenn AVD-Beschleunigung nicht unterstützt wird (HAXM)):

[![Deaktivieren von Warnungen zu AVD-Beschleunigung](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Ab Release 7.2 (Build 559) von Visual Studio für Mac wird möglicherweise ein Dialogfeld mit einer Leistungswarnung angezeigt, wenn Sie Ihre App zum ersten Mal im Android-Emulator bereitstellen. Diese Warnungsdialogfelder werden unten erklärt.

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

## <a name="deployment-issues"></a>Probleme bei der Bereitstellung

Wenn Sie eine Fehlermeldung erhalten, dass das APK auf dem Emulator nicht installiert werden konnte, oder dass Android Debug Bridge (**adb**) nicht ausgeführt werden konnte, stellen Sie sicher, dass das Android SDK eine Verbindung mit Ihrem Emulator herstellen kann. Gehen Sie dazu folgendermaßen vor:

1. Starten Sie den Emulator über den **Android Device Manager** (wählen Sie Ihr virtuelles Gerät aus, und klicken Sie auf **Start**).

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


## <a name="haxm-issues"></a>Probleme mit HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn der Android-Emulator nicht ordnungsgemäß gestartet wird, liegt dies häufig an Problemen mit HAXM. Probleme mit HAXM werden häufig durch Konflikte mit anderen Virtualisierungstechnologien, durch falsche Einstellungen oder durch einen veralteten HAXM-Treiber verursacht.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>HAXM-Virtualisierungskonflikte

HAXM kann mit anderen Technologien, die die Virtualisierung verwenden, in Konflikt stehen, wie etwa Hyper-V, Windows Device Guard und einige Antivirussoftware:

- **Hyper-V:** Wenn Sie eine Version von Windows vor dem **Windows 10-Update vom April 2018 (Build 1803)** verwenden und Hyper-V aktiviert ist, führen Sie die Schritte unter [Deaktivieren von Hyper-V](#disable-hyperv) aus.

- **Device Guard** &ndash; Device Guard und Credential Guard können verhindern, dass Hyper-V unter Windows deaktiviert werden kann. Informationen zum Deaktivieren von Device Guard und Credential Guard finden Sie unter [Deaktivieren von Device Guard](#disable-devguard).

- **Antivirussoftware** &ndash; Wenn Sie eine Antivirussoftware ausführen, die die hardwareunterstützte Virtualisierung verwendet, wie etwa Avast, deaktivieren oder deinstallieren Sie diese Software, starten Sie den Computer neu, und versuchen Sie erneut, den Android SDK Emulator zu starten.


### <a name="incorrect-bios-settings"></a>Falsche BIOS-Einstellungen

Wenn Sie HAXM auf einem Windows-PC verwenden, funktioniert HAXM nur dann, wenn Virtualization Technology (Intel VT-x) im BIOS aktiviert ist. Wenn VT-x deaktiviert ist, erhalten Sie eine Fehlermeldung wie die folgende, wenn Sie versuchen, den Android-Emulator zu starten:

**This computer meets the requirements for HAXM, but Intel Virtualization Technology (VT-x) is not turned on.** (Dieser Computer erfüllt die HAXM-Anforderungen, aber Intel Virtualization Technology ist nicht aktiviert).

Um diesen Fehler zu korrigieren, starten Sie den Computer im BIOS, aktivieren Sie sowohl VT-x als auch SLAT (Second Level Address Translation), und starten Sie den Computer dann wieder unter Windows.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Hyper-V deaktivieren

Wenn Sie eine Version von Windows vor dem **Windows 10-Update vom April 2018 (Build 1803)** verwenden und Hyper-V aktiviert ist, müssen Sie Hyper-V deaktivieren und den Computer neu starten, um HAXM installieren und verwenden zu können. Wenn Sie das **Windows 10-Update vom April 2018 (Build 1803)** oder höher verwenden, kann der Android-Emulator Version 27.2.7 oder höher Hyper-V (anstelle von HAXM) für die Hardwarebeschleunigung verwenden. Es ist also nicht notwendig, Hyper-V zu deaktivieren.

Sie können Hyper-V über die Systemsteuerung mit den folgenden Schritten deaktivieren:

1. Geben Sie im Windows-Suchfeld **Programme** ein, und klicken Sie anschließend auf das Suchergebnis **Programme und Funktionen**.

2. Wählen Sie über die Systemsteuerung die Option **Programme und Funktionen**, und klicken Sie anschließend auf **Windows-Funktionen ein- oder ausschalten**:

    ![Windows-Features aktivieren oder deaktivieren](troubleshooting-images/win/07-turn-windows-features.png)

3. Entfernen Sie das Häkchen bei **Hyper-V**, und starten Sie dann den Computer neu:

    ![Deaktivieren von Hyper-V in den Windows-Funktionen (Dialogfeld)](troubleshooting-images/win/08-uncheck-hyper-v.png)

Alternativ können Sie folgendes PowerShell-Cmdlet verwenden, um Hyper-V zu deaktivieren:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM und Microsoft Hyper-V können nicht gleichzeitig aktiviert sein. Es gibt keine Möglichkeit, zwischen Hyper-V und HAXM zu wechseln, ohne den Computer neu starten zu müssen. 

In einigen Fällen wird Hyper-V durch die oben aufgeführten Schritte nicht deaktiviert, wenn Device Guard und Credential Guard aktiviert sind. Wenn Sie Hyper-V nicht deaktivieren können (oder es vermeintlich deaktiviert ist, aber die Installation von HAXM dennoch fehlschlägt), führen Sie die Schritte im nächsten Abschnitt durch, um Device Guard und Credential Guard zu deaktivieren.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Deaktivieren von Device Guard

Device Guard und Credential Guard können verhindern, dass Hyper-V unter Windows deaktiviert werden kann. Dies stellt häufig ein Problem für mit einer Domäne verknüpfte Computer dar, die von einer besitzenden Organisation konfiguriert und gesteuert werden.
Führen Sie unter Windows 10 die folgenden Schritte aus, um zu prüfen, ob **Device Guard** ausgeführt wird:

1. Geben Sie in **Windows Search** **Systeminfo** ein, um die Anwendung **Systeminformationen** zu starten.

2. Suchen Sie in der **Systemübersicht** nach **Device Guard: virtualisierungsbasierte Sicherheit**, und überprüfen Sie, ob es sich im Status **Wird ausgeführt** befindet:

   [![Device Guard ist vorhanden und wird ausgeführt](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Wenn Device Guard aktiviert ist, können Sie es mit den folgenden Schritten deaktivieren:

1. Achten Sie wie im vorherigen Abschnitt beschrieben darauf, dass **Hyper-V** deaktiviert ist (unter **Windows-Features ein- oder ausschalten**).

2. Geben Sie im Windows-Suchfeld **gpedit** ein, und klicken Sie auf das Suchergebnis **Edit group policy** (Gruppenrichtlinie bearbeiten). Dadurch wird der **Editor für lokale Gruppenrichtlinien** gestartet.

3. Navigieren Sie im **Editor für lokale Gruppenrichtlinien** zu **Computerkonfiguration > Administrative Vorlagen > System > Device Guard**:

   [![Device Guard im Editor für lokale Gruppenrichtlinien](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

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

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn der Android-Emulator nicht ordnungsgemäß gestartet wird, liegt dies häufig an Problemen mit HAXM. Probleme mit HAXM werden häufig durch Konflikte mit anderen Virtualisierungstechnologien, durch falsche Einstellungen oder durch einen veralteten HAXM-Treiber verursacht. Installieren Sie den HAXM-Treiber erneut. Die dazu erforderlichen Schritte werden unter [Installieren von HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) beschrieben.

-----

