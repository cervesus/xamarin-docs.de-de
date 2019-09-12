---
title: Android-Emulator-Problembehandlung
description: In diesem Artikel wird erläutert, wie Sie Probleme diagnostizieren und umgehen, die bei der Verwendung des Android-Emulators auftreten können.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/27/2018
ms.openlocfilehash: d5de743cdbef1358450a2f358acb86dce2d373c7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758011"
---
# <a name="android-emulator-troubleshooting"></a>Behandlung von Problemen mit dem Android-Emulator

_In diesem Artikel werden die gängigsten Warnmeldungen und Probleme beschrieben, die beim Konfigurieren und Ausführen des Android-Emulators auftreten können. Darüber hinaus finden Sie Lösungen für die Behebung dieser Fehler sowie verschiedene Tipps zur Problembehandlung, um Probleme mit dem Emulator zu diagnostizieren._

::: zone pivot="windows"

## <a name="deployment-issues-on-windows"></a>Probleme bei der Bereitstellung unter Windows

Bei der Bereitstellung Ihrer App werden möglicherweise einige Fehlermeldungen vom Emulator angezeigt. Die am häufigsten auftretenden Fehler und die entsprechenden Lösungen werden im Folgenden erläutert.

### <a name="deployment-errors"></a>Bereitstellungsfehler

Wenn Sie eine Fehlermeldung erhalten, dass das APK nicht im Emulator installiert oder Android Debug Bridge (**adb**) nicht ausgeführt werden konnte, stellen Sie sicher, dass das Android SDK eine Verbindung mit Ihrem Emulator herstellen kann. Führen Sie die folgenden Schritte aus, um die Emulatorkonnektivität zu überprüfen:

1. Starten Sie den Emulator über den **Android Device Manager** (wählen Sie Ihr virtuelles Gerät aus, und klicken Sie auf **Start**).

2. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner, in dem **adb** installiert ist. Wenn das Android SDK am Standardspeicherort installiert ist, befindet sich **adb** unter **C:\\Programmdateien (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**. Falls dies nicht der Fall ist, ändern Sie diesen Pfad für den Speicherort des Android SDK auf Ihrem Computer.

3. Geben Sie folgenden Befehl ein:

   ```shell
   adb devices
   ```

4. Wenn auf den Emulator vom Android SDK aus zugegriffen werden kann, sollte der Emulator in der Liste der angehängten Geräte aufgelistet sein. Beispiel:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Wenn der Emulator nicht in dieser Liste angezeigt wird, starten Sie den **Android SDK Manager**, wenden Sie alle Updates an, und versuchen Sie erneut, den Emulator zu starten.

### <a name="mmio-access-error"></a>MMIO-Zugriffsfehler

Wenn die Meldung **Ein MMIO-Zugriffsfehler ist aufgetreten.** angezeigt wird, starten Sie den Emulator neu.

<a name="gps-win" />

## <a name="missing-google-play-services"></a>Fehlende Google Play Services

Wenn Google Play Services oder der Google Play Store nicht auf dem virtuellen Gerät installiert ist, das im Emulator ausgeführt wird, wird diese Bedingung häufig durch die Erstellung eines virtuellen Geräts ohne Einschließen dieser Pakete verursacht. Wenn Sie ein virtuelles Gerät erstellen (siehe [Verwalten von virtuellen Geräten mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)), müssen Sie eine oder beide der folgenden Optionen auswählen:

- **Google-APIs** &ndash; Beinhalten Google Play Services auf dem virtuellen Gerät.
- **Google Play Store** &ndash; Beinhaltet Google Play Store auf dem virtuellen Gerät.

Dieses virtuelle Gerät beinhaltet z.B. Google Play Services und den Google Play Store:

[![Beispiel-AVD mit aktivierten Google Play Services und Google Play Store](troubleshooting-images/win/00-add-gps-w158-sml.png)](troubleshooting-images/win/00-add-gps-w158.png#lightbox)

> [!NOTE]
> Google Play Store-Images sind nur für einige grundlegende Gerätetypen wie Pixel, Pixel 2, Nexus 5 und Nexus 5X verfügbar.

<a name="perf-win" />

## <a name="performance-issues"></a>Leistungsprobleme

Leistungsprobleme werden in der Regel durch eines der folgenden Probleme verursacht:

- Emulator wird ohne Hardwarebeschleunigung ausgeführt

- Das virtuelle Gerät, das im Emulator ausgeführt wird, verwendet kein x86-basiertes Systemimage.

In den folgenden Abschnitten werden diese Szenarien ausführlicher erläutert.

### <a name="hardware-acceleration-is-not-enabled"></a>Hardwarebeschleunigung nicht aktiviert

Wenn die Hardwarebeschleunigung nicht aktiviert ist, wird beim Starten eines virtuellen Geräts über den Device Manager ein Dialogfeld mit der Fehlermeldung angezeigt, dass die Windows-Hypervisor-Plattform (WHPX) nicht ordnungsgemäß konfiguriert ist:

![Beispielwarnung von Device Manager](troubleshooting-images/win/01-dev-mgr-warning-w158.png)

Wenn diese Fehlermeldung angezeigt wird, lesen Sie [Probleme bei der Hardwarebeschleunigung](#accel-issues-win), um zu erfahren, welche Schritte Sie zum Überprüfen und Aktivieren der Hardwarebeschleunigung durchführen können.

### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>Emulator wird trotz aktivierter Beschleunigung zu langsam ausgeführt 

Eine häufige Ursache für dieses Problem ist, dass kein x86-basiertes Image auf Ihrem virtuellen Android-Gerät (Android Virtual Device, AVD) verwendet wird. Wenn Sie ein virtuelles Gerät erstellen (siehe [Verwalten von virtuellen Geräten mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)), müssen Sie unbedingt ein x86-basiertes Systemimage auswählen:

[![Auswählen eines x86-Systemimage für ein virtuelles Gerät](troubleshooting-images/win/02-x86-virtual-device-w158-sml.png)](troubleshooting-images/win/02-x86-virtual-device-w158.png#lightbox)

<a name="accel-issues-win" />

## <a name="hardware-acceleration-issues"></a>Probleme bei der Hardwarebeschleunigung

Sowohl bei der Verwendung von Hyper-V als auch von HAXM zur Hardwarebeschleunigung können Konfigurationsprobleme oder Konflikte mit anderer Software auf Ihrem Computer auftreten. Sie können überprüfen, ob die Hardwarebeschleunigung aktiviert ist (und welche Beschleunigungsmethode vom Emulator verwendet wird), indem Sie eine Eingabeaufforderung öffnen und den folgenden Befehl eingeben:

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator-check.exe" accel
```

Bei diesem Befehl wird davon ausgegangen, dass das Android SDK am Standardspeicherort installiert ist (**C:\\Programmdateien (x86)\\Android\\android-sdk**). Falls dies nicht der Fall ist, ändern Sie den obigen Pfad für den Speicherort des Android SDK auf Ihrem Computer.

### <a name="hardware-acceleration-not-available"></a>Hardwarebeschleunigung nicht verfügbar

Wenn Hyper-V verfügbar ist, wird eine Meldung ähnlich wie im folgenden Beispiel vom Befehl **emulator-check.exe accel** zurückgegeben:

```cmd
HAXM is not installed, but Windows Hypervisor Platform is available.
```

Wenn HAXM verfügbar ist, wird eine Meldung ähnlich wie im folgenden Beispiel zurückgegeben:

```cmd
HAXM version 6.2.1 (4) is installed and usable.
```

Wenn keine Hardwarebeschleunigung verfügbar ist, wird eine Meldung ähnlich wie im folgenden Beispiel angezeigt (der Emulator sucht HAXM, wenn Hyper-V nicht gefunden werden kann):

```cmd
HAXM is not installed on this machine
```

Wenn die Hardwarebeschleunigung nicht verfügbar ist, lesen Sie [Beschleunigung mit Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vswin#hyper-v-win), um zu erfahren, wie Sie die Hardwarebeschleunigung auf Ihrem Computer aktivieren.

### <a name="incorrect-bios-settings"></a>Falsche BIOS-Einstellungen

Wenn das BIOS nicht ordnungsgemäß für die Unterstützung der Hardwarebeschleunigung konfiguriert wurde, wird beim Ausführen des Befehls **emulator-check.exe accel** eine Meldung ähnlich wie im folgenden Beispiel angezeigt:

```cmd
VT feature disabled in BIOS/UEFI
```

Um dieses Problem zu beheben, starten Sie Ihren Computer im BIOS neu und aktivieren die folgenden Optionen:

- Virtualisierungstechnologie (Bezeichnung kann je nach Hauptplatinenhersteller variieren)
- Von Hardware erzwungene Datenausführungsverhinderung

Wenn die Hardwarebeschleunigung aktiviert ist und das BIOS ordnungsgemäß konfiguriert ist, sollte der Emulator erfolgreich mit der Hardwarebeschleunigung ausgeführt werden.
Es können jedoch weiterhin Probleme auftreten, die spezifisch für Hyper-V und HAXM sind. Dies wird nachfolgend erörtert.

### <a name="hyper-v-issues"></a>Probleme mit Hyper-V

In einigen Fällen wird Hyper-V möglicherweise nicht ordnungsgemäß aktiviert, wenn im Dialogfeld **Windows-Features aktivieren oder deaktivieren** **Hyper-V** und **Windows-Hypervisor-Plattform** aktiviert werden. Um sicherzustellen, dass Hyper-V aktiviert wird, führen Sie die folgenden Schritte aus:

1. Geben Sie **powershell** in das Windows-Suchfeld ein.

2. Klicken Sie in den Suchergebnissen mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie auf **Als Administrator ausführen**.

3. Geben Sie in der PowerShell-Konsole folgenden Befehl ein:

    ```powershell
    Get-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online
    ```

    Wenn Hyper-V nicht aktiviert ist, wird eine Meldung ähnlich wie im folgenden Beispiel angezeigt, die angibt, dass der Status von Hyper-V **Deaktiviert** ist:

    ```
    FeatureName      : Microsoft-Hyper-V-All
    DisplayName      : Hyper-V
    Description      : Provides services and management tools for creating and running virtual machines and their resources.
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

4. Geben Sie in der PowerShell-Konsole folgenden Befehl ein:

    ```powershell
    Get-WindowsOptionalFeature -FeatureName HypervisorPlatform -Online
    ```

    Wenn der Hypervisor nicht aktiviert ist, wird eine Meldung ähnlich wie im folgenden Beispiel angezeigt, die angibt, dass der Status von HypervisorPlatform **Deaktiviert** ist:

    ```
    FeatureName      : HypervisorPlatform
    DisplayName      : Windows Hypervisor Platform
    Description      : Enables virtualization software to run on the Windows hypervisor
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

Wenn Hyper-V und/oder HypervisorPlatform nicht aktiviert sind, aktivieren Sie sie anhand der folgenden PowerShell-Befehle:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
```

Starten Sie den Computer nach Ausführung dieser Befehle neu. 

Weitere Informationen zum Aktivieren von Hyper-V (sowie den Methoden zum Aktivieren von Hyper-V mit dem DISM-Tool zur Imageverwaltung für die Bereitstellung) finden Sie unter [Installieren von Hyper-V unter Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### <a name="haxm-issues"></a>Probleme mit HAXM

Probleme mit HAXM werden häufig durch Konflikte mit anderen Virtualisierungstechnologien, durch falsche Einstellungen oder durch einen veralteten HAXM-Treiber verursacht.

### <a name="haxm-process-is-not-running"></a>HAXM process is not running (HAXM-Prozess wird nicht ausgeführt).

Wenn HAXM installiert ist, können Sie überprüfen, ob der HAXM-Prozess ausgeführt wird, indem Sie eine Eingabeaufforderung öffnen und den folgenden Befehl eingeben:

```cmd
sc query intelhaxm
```

Wenn der HAXM-Prozess ausgeführt wird, sollten Sie diese oder eine ähnliche Ausgabe erhalten:

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

Wenn `STATE` nicht auf `RUNNING` festgelegt ist, erfahren Sie unter [How to Use the Intel Hardware Accelerated Execution Manager (Verwenden von Intel Hardware Accelerated Execution Manager)](https://software.intel.com/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator), wie Sie das Problem beheben.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM-Virtualisierungskonflikte

HAXM kann mit anderen Technologien, die die Virtualisierung verwenden, in Konflikt stehen, wie etwa Hyper-V, Windows Device Guard und einige Antivirussoftware:

- **Hyper-V** &ndash; Wenn Sie eine Version von Windows vor dem **Windows 10-Update vom April 2018 (Build 1803)** verwenden und Hyper-V aktiviert ist, führen Sie die Schritte unter [Deaktivieren von Hyper-V](#disable-hyperv) aus, damit HAXM aktiviert werden kann.

- **Device Guard** &ndash; Device Guard und Credential Guard können verhindern, dass Hyper-V unter Windows deaktiviert werden kann. Informationen zum Deaktivieren von Device Guard und Credential Guard finden Sie unter [Deaktivieren von Device Guard](#disable-devguard).

- **Antivirensoftware** &ndash; Wenn Sie eine Antivirensoftware ausführen, die die hardwareunterstützte Virtualisierung verwendet (z.B. Avast), deaktivieren oder deinstallieren Sie diese Software, starten Sie den Computer neu, und versuchen Sie erneut, den Android-Emulator zu starten.

#### <a name="incorrect-bios-settings"></a>Falsche BIOS-Einstellungen

Wenn Sie HAXM auf einem Windows-PC verwenden, funktioniert HAXM nur dann, wenn Virtualization Technology (Intel VT-x) im BIOS aktiviert ist. Wenn VT-x deaktiviert ist, erhalten Sie beim Starten des Android-Emulators eine Fehlermeldung wie die folgende:

**This computer meets the requirements for HAXM, but Intel Virtualization Technology (VT-x) is not turned on.** (Dieser Computer erfüllt die HAXM-Anforderungen, aber Intel Virtualization Technology ist nicht aktiviert).

Um diesen Fehler zu korrigieren, starten Sie den Computer im BIOS, aktivieren Sie sowohl VT-x als auch SLAT (Second Level Address Translation), und starten Sie den Computer dann wieder unter Windows neu.

<a name="disable-hyperv" />

#### <a name="disabling-hyper-v"></a>Hyper-V deaktivieren

Wenn Sie eine Version von Windows vor dem **Windows 10-Update vom April 2018 (Build 1803)** verwenden und Hyper-V aktiviert ist, müssen Sie Hyper-V deaktivieren und den Computer neu starten, um HAXM installieren und verwenden zu können. Wenn Sie das **Windows 10-Update vom April 2018 (Build 1803)** oder höher verwenden, kann der Android-Emulator Version 27.2.7 oder höher Hyper-V (anstelle von HAXM) für die Hardwarebeschleunigung verwenden. Es ist also nicht notwendig, Hyper-V zu deaktivieren.

Sie können Hyper-V über die Systemsteuerung mit den folgenden Schritten deaktivieren:

1. Geben Sie **windows features** in das Windows-Suchfeld ein, und klicken Sie in den Suchergebnissen auf **Windows-Features aktivieren oder deaktivieren**.

2. Deaktivieren Sie **Hyper-V**:

    ![Deaktivieren von Hyper-V in den Windows-Funktionen (Dialogfeld)](troubleshooting-images/win/03-uncheck-hyper-v.png)

3. Starten Sie den Computer neu.

Alternativ können Sie folgenden PowerShell-Befehl verwenden, um den Hyper-V-Hypervisor zu deaktivieren:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM und Microsoft Hyper-V können nicht gleichzeitig aktiviert sein. Es gibt keine Möglichkeit, zwischen Hyper-V und HAXM zu wechseln, ohne den Computer neu starten zu müssen. 

In einigen Fällen wird Hyper-V durch die oben aufgeführten Schritte nicht deaktiviert, wenn Device Guard und Credential Guard aktiviert sind. Wenn Sie Hyper-V nicht deaktivieren können (oder es vermeintlich deaktiviert ist, aber die Installation von HAXM dennoch fehlschlägt), führen Sie die Schritte im nächsten Abschnitt durch, um Device Guard und Credential Guard zu deaktivieren.

<a name="disable-devguard" />

#### <a name="disabling-device-guard"></a>Deaktivieren von Device Guard

Device Guard und Credential Guard können verhindern, dass Hyper-V unter Windows deaktiviert werden kann. Dies stellt häufig ein Problem für in eine Domäne eingebundene Computer dar, die von einer besitzenden Organisation konfiguriert und gesteuert werden. Führen Sie unter Windows 10 die folgenden Schritte aus, um zu prüfen, ob **Device Guard** ausgeführt wird:

1. Geben Sie **System info** in das Windows-Suchfeld ein, und klicken Sie in den Suchergebnissen auf **Systeminformationen**.

2. Suchen Sie in der **Systemübersicht** nach **Device Guard: virtualisierungsbasierte Sicherheit**, und überprüfen Sie, ob es sich im Status **Wird ausgeführt** befindet:

   [![Device Guard ist vorhanden und wird ausgeführt](troubleshooting-images/win/04-device-guard-sml.png)](troubleshooting-images/win/04-device-guard.png#lightbox)

Wenn Device Guard aktiviert ist, können Sie es mit den folgenden Schritten deaktivieren:

1. Achten Sie wie im vorherigen Abschnitt beschrieben darauf, dass **Hyper-V** deaktiviert ist (unter **Windows-Features ein- oder ausschalten**).

2. Geben Sie im Windows-Suchfeld **gpedit** ein, und klicken Sie auf das Suchergebnis **Gruppenrichtlinie bearbeiten**. Durch diese Schritte wird der **Editor für lokale Gruppenrichtlinien** gestartet.

3. Navigieren Sie im **Editor für lokale Gruppenrichtlinien** zu **Computerkonfiguration > Administrative Vorlagen > System > Device Guard**:

   [![Device Guard im Editor für lokale Gruppenrichtlinien](troubleshooting-images/win/05-group-policy-editor-sml.png)](troubleshooting-images/win/05-group-policy-editor.png#lightbox)

4. Ändern Sie **Virtualisierungsbasierte Sicherheit aktivieren** in **Deaktiviert** (wie oben gezeigt), und schließen Sie den **Editor für lokale Gruppenrichtlinien**.

5. Geben Sie im Windows-Suchfeld **cmd** ein. Wenn die **Eingabeaufforderung** in den Suchergebnissen angezeigt wird, klicken Sie mit der rechten Maustaste auf **Eingabeaufforderung** und dann auf **Als Administrator ausführen**.

6. Kopieren Sie die folgenden Befehle und fügen Sie sie in das Eingabeaufforderungsfenster (wenn **Z:** verwendet wird, wählen Sie stattdessen einen nicht verwendeten Laufwerkbuchstaben aus):

    ```cmd
    mountvol Z: /s
    copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
    bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
    bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
    mountvol Z: /d
    ```

7. Starten Sie den Computer neu. Auf dem Startbildschirm sollten Sie eine Aufforderung ähnlich wie die folgende Meldung sehen:

   **Do you want to disable Credential Guard?** (Möchten Sie Credential Guard deaktivieren?)

   Drücken Sie die angegebene Taste, um Credential Guard wie aufgefordert zu deaktivieren.

8. Nachdem der Computer neu gestartet wurde, stellen Sie erneut sicher, das Hyper-V deaktiviert ist (wie im vorherigen Schritt beschrieben).

Wenn Hyper-V immer noch nicht deaktiviert ist, verhindern die Richtlinien Ihres mit einer Domäne verknüpften Computers möglicherweise das Deaktivieren von Device Guard oder Credential Guard. In diesem Fall können Sie von Ihrem Domänenadministrator eine Ausnahme anfordern, die es Ihnen ermöglicht, Credential Guard zu deaktivieren. Alternativ können Sie einen Computer verwenden, der nicht in eine Domäne eingebunden ist, wenn Sie HAXM verwenden müssen.

## <a name="additional-troubleshooting-tips"></a>Weitere Tipps zur Problembehandlung

Die folgenden Vorschläge sind oftmals hilfreich bei der Diagnose von Problemen mit dem Android-Emulator.

### <a name="starting-the-emulator-from-the-command-line"></a>Starten des Emulators über die Befehlszeile

Wenn der Emulator noch nicht ausgeführt wird, können Sie ihn über die Befehlszeile starten (statt von Visual Studio aus), um die Ausgabe anzuzeigen. Die AVD-Images des Android-Emulators werden in der Regel im folgenden Speicherort gespeichert (ersetzen Sie *Benutzername* durch Ihren Windows-Benutzernamen):

**C:\\Benutzer\\*Benutzername*\\.android\\avd**

Sie können den Emulator mit einem AVD-Image von diesem Speicherort starten, indem Sie den Ordnernamen des AVD übergeben. Dieser Befehl startet z.B. ein AVD mit dem Namen **Pixel_API_27**:

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator.exe" -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_API_27 -prop monodroid.avdname=Pixel_API_27
```

Bei diesem Beispiel wird davon ausgegangen, dass das Android SDK am Standardspeicherort installiert ist (**C:\\Programmdateien (x86)\\Android\\android-sdk**). Falls dies nicht der Fall ist, ändern Sie den obigen Pfad für den Speicherort des Android SDK auf Ihrem Computer.

Wenn Sie diesen Befehl ausführen, wird eine Vielzahl von Ausgabezeilen erzeugt, während der Emulator gestartet wird. Insbesondere Zeilen wie im folgenden Beispiel werden ausgegeben, wenn die Hardwarebeschleunigung aktiviert ist und ordnungsgemäß funktioniert (in diesem Beispiel wird HAXM zur Hardwarebeschleunigung verwendet):

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: HAXM version 6.2.1 (4) is installed and usable.
```

### <a name="viewing-device-manager-logs"></a>Anzeigen von Device Manager-Protokollen

Häufig können Sie Probleme mit dem Emulator diagnostizieren, indem Sie die Device Manager-Protokolle anzeigen. Diese Protokolle werden am folgenden Speicherort ausgegeben:

**C:\\Users\\*Benutzername*\\AppData\\Roaming\\XamarinDeviceManager**

Sie können Dateien mit dem Namen **DeviceManager.log** in einem Text-Editor wie Editor anzeigen. Der folgende Beispielprotokolleintrag gibt an, dass HAXM nicht auf dem Computer gefunden wurde:

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end
::: zone pivot="macos"

## <a name="deployment-issues-on-macos"></a>Probleme bei der Bereitstellung unter macOS

Bei der Bereitstellung Ihrer App werden möglicherweise einige Fehlermeldungen vom Emulator angezeigt. Die am häufigsten auftretenden Fehler und die entsprechenden Lösungen werden im Folgenden erläutert.

### <a name="deployment-errors"></a>Bereitstellungsfehler

Wenn Sie eine Fehlermeldung erhalten, dass das APK nicht im Emulator installiert oder Android Debug Bridge (**adb**) nicht ausgeführt werden konnte, stellen Sie sicher, dass das Android SDK eine Verbindung mit Ihrem Emulator herstellen kann. Führen Sie die folgenden Schritte aus, um die Konnektivität zu überprüfen:

1. Starten Sie den Emulator über den **Android Device Manager** (wählen Sie Ihr virtuelles Gerät aus, und klicken Sie auf **Start**).

2. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner, in dem **adb** installiert ist. Wenn das Android SDK am Standardspeicherort installiert ist, befindet sich **adb** unter **~/Library/Developer/Xamarin/android-sdk-macosx/platform-tools/adb**. Falls dies nicht der Fall ist, ändern Sie diesen Pfad für den Speicherort des Android SDK auf Ihrem Computer.

3. Geben Sie folgenden Befehl ein:

   ```shell
   adb devices
   ```

4. Wenn auf den Emulator vom Android SDK aus zugegriffen werden kann, sollte der Emulator in der Liste der angehängten Geräte aufgelistet sein. Beispiel:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Wenn der Emulator nicht in dieser Liste angezeigt wird, starten Sie den **Android SDK Manager**, wenden Sie alle Updates an, und versuchen Sie erneut, den Emulator zu starten.

### <a name="mmio-access-error"></a>MMIO-Zugriffsfehler

Wenn **Ein MMIO-Zugriffsfehler ist aufgetreten.** angezeigt wird, starten Sie den Emulator neu.

<a name="gps-mac" />

## <a name="missing-google-play-services"></a>Fehlende Google Play Services

Wenn Google Play Services oder der Google Play Store nicht auf dem virtuellen Gerät installiert ist, das im Emulator ausgeführt wird, wird diese Bedingung in der Regel durch die Erstellung eines virtuellen Geräts ohne Einschließen dieser Pakete verursacht. Wenn Sie ein virtuelles Gerät erstellen (siehe [Verwalten von virtuellen Geräten mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)), müssen Sie eine oder beide der folgenden Optionen auswählen:

- **Google-APIs** &ndash; Beinhalten Google Play Services auf dem virtuellen Gerät.
- **Google Play Store** &ndash; Beinhaltet Google Play Store auf dem virtuellen Gerät.

Dieses virtuelle Gerät beinhaltet z.B. Google Play Services und den Google Play Store:

[![Beispiel-AVD mit aktivierten Google Play Services und Google Play Store](troubleshooting-images/mac/01-google-play-services-m75-sml.png)](troubleshooting-images/mac/01-google-play-services-m75.png#lightbox)

> [!NOTE]
> Google Play Store-Images sind nur für einige grundlegende Gerätetypen wie Pixel, Pixel 2, Nexus 5 und Nexus 5X verfügbar.

<a name="perf-mac" />

## <a name="performance-issues"></a>Leistungsprobleme

Leistungsprobleme werden in der Regel durch eines der folgenden Probleme verursacht:

- Emulator wird ohne Hardwarebeschleunigung ausgeführt

- Das virtuelle Gerät, das im Emulator ausgeführt wird, verwendet kein x86-basiertes Systemimage.

In den folgenden Abschnitten werden diese Szenarien ausführlicher erläutert.

### <a name="hardware-acceleration-is-not-enabled"></a>Hardwarebeschleunigung nicht aktiviert

Wenn die Hardwarebeschleunigung nicht aktiviert ist, wird bei der Bereitstellung Ihrer App im Android-Emulator möglicherweise eine Meldung wie etwa die Folgende angezeigt: **Gerät wird ohne Beschleunigung ausgeführt**. Wenn Sie nicht sicher sind, ob die Hardwarebeschleunigung auf Ihrem Computer aktiviert ist (oder wenn Sie wissen möchten, welche Technologie die Beschleunigung bereitstellt), führen Sie die Schritte unter [Probleme bei der Hardwarebeschleunigung](#accel-issues-mac) durch, um die Hardwarebeschleunigung zu überprüfen und zu aktivieren.

### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>Emulator wird trotz aktivierter Beschleunigung zu langsam ausgeführt 

Eine häufige Ursache für dieses Problem ist, dass kein x86-basiertes Image auf Ihrem virtuellen Android-Gerät verwendet wird. Wenn Sie ein virtuelles Gerät erstellen (siehe [Verwalten von virtuellen Geräten mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)), müssen Sie unbedingt ein x86-basiertes Systemimage auswählen:

[![Auswählen eines x86-Systemimage für ein virtuelles Gerät](troubleshooting-images/mac/02-x86-virtual-device-m75-sml.png)](troubleshooting-images/mac/02-x86-virtual-device-m75.png#lightbox)

<a name="accel-issues-mac" />

## <a name="hardware-acceleration-issues"></a>Probleme bei der Hardwarebeschleunigung

Sowohl bei der Verwendung des Hypervisorframeworks als auch von HAXM zur Hardwarebeschleunigung des Emulators können Probleme im Zusammenhang mit der Installation oder aufgrund einer veralteten macOS-Version auftreten. In den folgenden Abschnitten wird beschrieben, wie Sie dieses Problem beheben können.

<a name="hypervisor-issues" />

### <a name="hypervisor-framework-issues"></a>Probleme mit dem Hypervisorframework

Wenn Sie macOS 10.10 oder höher auf einem neueren Mac verwenden, verwendet der Android-Emulator automatisch das Hypervisorframework für die Hardwarebeschleunigung. Allerdings unterstützen einige ältere Mac-Computer oder Mac-Computer mit einer älteren Version als 10.10 möglicherweise nicht das Hypervisorframework.

Um zu bestimmen, ob Ihr Mac das Hypervisorframework unterstützt, öffnen Sie ein Terminal, und geben Sie den folgenden Befehl ein:

```bash
sysctl kern.hv_support
```

Wenn Ihr Mac das Hypervisorframework unterstützt, gibt der obige Befehl folgendes Ergebnis zurück:

```bash
kern.hv_support: 1
```

Wenn das Hypervisorframework nicht auf Ihrem Mac verfügbar ist, können Sie die Schritte unter [Beschleunigung mit HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#haxm-mac) durchführen, um stattdessen HAXM für die Beschleunigung zu verwenden.

### <a name="haxm-issues"></a>Probleme mit HAXM

Wenn der Android-Emulator nicht ordnungsgemäß gestartet wird, liegt dies häufig an Problemen mit HAXM. Probleme mit HAXM werden häufig durch Konflikte mit anderen Virtualisierungstechnologien, durch falsche Einstellungen oder durch einen veralteten HAXM-Treiber verursacht. Installieren Sie den HAXM-Treiber erneut. Die dazu erforderlichen Schritte werden unter [Installieren von HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#install-haxm-mac) beschrieben.

## <a name="additional-troubleshooting-tips"></a>Weitere Tipps zur Problembehandlung

Die folgenden Vorschläge sind oftmals hilfreich bei der Diagnose von Problemen mit dem Android-Emulator.

### <a name="starting-the-emulator-from-the-command-line"></a>Starten des Emulators über die Befehlszeile

Wenn der Emulator noch nicht ausgeführt wird, können Sie ihn über die Befehlszeile starten (statt von Visual Studio für Mac aus), um die Ausgabe anzuzeigen. Die AVD-Images des Android-Emulators werden in der Regel an folgendem Speicherort gespeichert:

**~/.android/avd**

Sie können den Emulator mit einem AVD-Image von diesem Speicherort starten, indem Sie den Ordnernamen des AVD übergeben. Dieser Befehl startet z.B. ein AVD mit dem Namen **Pixel_2_API_28**:

```cmd
~/Library/Developer/Xamarin/android-sdk-macosx/emulator/emulator -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_2_API_28 -prop monodroid.avdname=Pixel_2_API_28
```

Wenn das Android SDK am Standardspeicherort installiert ist, befindet sich der Emulator im Verzeichnis **~/Library/Developer/Xamarin/android-sdk-macosx/emulator**. Falls dies nicht der Fall ist, ändern Sie diesen Pfad für den Speicherort des Android SDK auf Ihrem Mac-Computer.

Wenn Sie diesen Befehl ausführen, wird eine Vielzahl von Ausgabezeilen erzeugt, während der Emulator gestartet wird. Insbesondere Zeilen wie im folgenden Beispiel werden ausgegeben, wenn die Hardwarebeschleunigung aktiviert ist und ordnungsgemäß funktioniert (in diesem Beispiel wird das Hypervisorframework zur Hardwarebeschleunigung verwendet):

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: Hypervisor.Framework OS X Version 10.13
```

### <a name="viewing-device-manager-logs"></a>Anzeigen von Device Manager-Protokollen

Häufig können Sie Probleme mit dem Emulator diagnostizieren, indem Sie die Device Manager-Protokolle anzeigen. Diese Protokolle werden am folgenden Speicherort ausgegeben:

**~/Library/Logs/XamarinDeviceManager**

Sie können Dateien mit dem Namen **Android Devices.log** durch Doppelklick anzeigen und in der Konsolen-App öffnen. Der folgende Beispielprotokolleintrag gibt an, dass HAXM nicht gefunden wurde:

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end
