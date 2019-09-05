---
title: Wo finde ich meine Versionsinformationen und Protokolle?
description: In diesem Dokument wird beschrieben, wo Sie nach xamarin-Versionsinformationen und-Protokollen suchen können. Diese Informationen sind hilfreich bei der Diagnose von Problemen, beim Senden von Fehlern oder beim erhalten von Support.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: f2d9921795d2a788a6646aad36712a0691c07d50
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291173"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Wo finde ich meine Versionsinformationen und Protokolle?

## <a name="outline"></a>Umriss

- [Versionsinformationen](#version-information)
  - Informationen zur Windows-Version
  - Mac-Versionsinformationen
  - Android SDK Tools, Platt Form Tools, Build-Tools
- [IDE-und Installer-Protokolle](#ide-and-installer-logs)
  - [Windows-Protokolle](#windows-logs)
    - Xamarin Studio
    - Xamarin für Visual Studio
    - Universeller xamarin-Installer
    - Einzelne `.msi` Installationsprogramme, ausführliche Protokolle
    - Visual Studio-Start, ausführliche Protokolle
  - [Mac-Protokolle](#mac-logs)
    - Buildhost
  - Visual Studio für Mac
    - Xamarin Studio
    - Xamarin-Installer
- [Ausführliche Buildausgabe](#verbose-build-output-logs)
- [Debugprotokolle für xamarin. Android-und xamarin. IOS-apps](#debug-logs-for-xamarin-apps)
  - Android `adb` logcat-Protokolle
  - IOS-simulatorprotokolle (auf Mac)
  - IOS-Geräte Protokolle (auf Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Versionsinformationen

Es ist in der Regel am besten, alle Informationen über die Schaltflächen zum **Kopieren von Informationen** zurückzusenden. Andernfalls müssen häufig zusätzliche Informationen angefordert werden. Beispielsweise können Betriebssystemversionen, Xcode-Version, installierte Android-API-Ebenen und .NET-Version bei der Behebung eines Problems von Bedeutung sein.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Informationen zur Windows-Version

#### <a name="xamarin-studio"></a>Xamarin Studio

**Hilfe > zum Anzeigen von Details > Kopieren von Informationen > [Schaltfläche]**

#### <a name="visual-studio"></a>Visual Studio

**Hilfe > zu Microsoft Visual Studio > Kopieren von Informationen [Schaltfläche]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac-Versionsinformationen

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Visual Studio-> zu Visual Studio > Anzeigen von Details > Kopieren von Informationen [Schaltfläche]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, Platt Form Tools, Build-Tools

Öffnen Sie den Android SDK-Manager, und sehen Sie sich einen Screenshot des Abschnitts "Top **Tools** " an.

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Tools > Android SDK Manager öffnen**

#### <a name="visual-studio"></a>Visual Studio

**Tools > Android-> Android SDK Manager öffnen...**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE-und Installer-Protokolle

Stellen Sie sicher, dass Sie für jeden Protokoll Speicherort den gesamten Protokoll Ordner zippen und anfügen.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows-Protokolle

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" />Visual Studio-Tools für xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" />Visual Studio 2017

[So erhalten Sie die Visual Studio-Installations Protokolle](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" />Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" />Xamarin Universal-Installer

`%LOCALAPPDATA%\Xamarin\Universal`

Dabei handelt es sich um die `XamarinInstaller.exe` Protokolle des Installers.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Einzelne `.msi` Installationsprogramme, ausführliche Protokolle

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Angabe [Befehlszeilenoptionen](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio-Start, ausführliche Protokolle

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Referenz: [/Log ("tovenv. exe")](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac-Protokolle

Sie können das Menü Element Go **> Gehe zu Ordner** in Finder auswählen und dann diese Pfade kopieren und in das Dialogfeld einfügen.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio für Mac

`~/Library/Logs/VisualStudio/7.0`(diese Zahl kann sich abhängig von der verwendeten Version ändern.)

Dieser Ordner kann auch über die Hilfe > geöffneten Protokoll Verzeichnis geöffnet werden.

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0`(diese Zahl kann sich abhängig von der verwendeten Version ändern.)

Dieser Ordner kann auch über die Hilfe > geöffneten Protokoll Verzeichnis geöffnet werden.

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin Universal-Installer

`~/Library/Logs/XamarinInstaller/Universal`

Dabei handelt es sich um die `XamarinInstaller.dmg` Protokolle des Installers.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin-buildhost

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Ausführliche Buildausgabe

1. [Diagnose-MSBuild-Ausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)aktivieren

2. Aktivieren Sie für IOS-apps auch die ausführliche **mtouchscreen** -Ausgabe `-v -v -v -v` , indem Sie unter **Projekteigenschaften > IOS-Build > Allgemein (Registerkarte) > zusätzlichen Optionen > zusätzlichen mberührungs-Argumenten**hinzufügen.

3. Bereinigen und erneutes Erstellen des Projekts.

4. Kopieren Sie die Buildausgabe aus der IDE in eine Textdatei, und fügen Sie Sie ein.
     - Visual Studio (Windows): **Anzeigen > Ausgabe > Ausgabe anzeigen von: Erstellen**
     - Visual Studio für Mac: **Anzeigen von > Pads > Fehlern > Buildausgabe (Registerkarte)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Debugprotokolle für xamarin. Android-und xamarin. IOS-apps

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Anzeigen von > Pads > Anwendungs Ausgabe**

(Beachten Sie, dass dieses Menü Element nur angezeigt wird, nachdem die APP gestartet wurde.)

### <a name="visual-studio"></a>Visual Studio

**Anzeigen > Ausgabe > Ausgabe anzeigen von: Gen**

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpsdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [`adb`](https://developer.android.com/tools/help/adb.html) logcat-Protokolle

Fügen Sie nach `adb` dem Ausführen des Befehls die Datei **android_logcat. txt** von Ihrem Desktop zurück. Bei diesen Anweisungen wird angenommen, dass Sie nur ein Gerät angefügt haben.

Siehe auch die Seite [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) .

#### <a name="visual-studio"></a>Visual Studio

1. **Tools > Android > Android ADB-Eingabeaufforderung starten**
2. Bereinigen Sie das Protokoll:`adb logcat -c`
3. Reproduzieren Sie das Problem.
4. Ausgabe des Protokolls:`adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. **Tools > Android SDK Eingabeaufforderung öffnen**
2. Bereinigen Sie das Protokoll:`adb logcat -c`
3. Reproduzieren Sie das Problem.
4. Ausgabe des Protokolls:`adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />IOS-simulatorprotokolle (auf Mac)

- Um auf das System Protokoll zuzugreifen, wählen Sie **Debuggen > System Protokoll öffnen...** in der IOS-Simulator-App aus.

- Öffnen Sie zum Anzeigen von Absturzberichten aus dem Simulator Console. app, und `~/Library/Logs > DiagnosticReports`navigieren Sie zu.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />IOS-Geräte Protokolle (auf Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Anzeigen von > Pads > IOS-Geräte Protokoll**

#### <a name="xcode"></a>Xcode

**Fenster > Geräte > $ {DeviceName}**

Absturzberichte sind unter der Schaltfläche **Geräte Protokolle anzeigen** verfügbar. Das System Protokoll für das Gerät wird unten im Fenster unter dem Pfeil zur Offenlegung angezeigt.

#### <a name="xcode-5"></a>Xcode 5

**Fenster > Planer > Geräte (Registerkarte) > $ {DeviceName}**
