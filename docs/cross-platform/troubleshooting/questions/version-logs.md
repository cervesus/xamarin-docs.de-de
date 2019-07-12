---
title: Wo finde ich meine Versionsinformationen und Protokolle?
description: Dieses Dokument beschreibt, wo Sie suchen, um Xamarin-Versionsinformationen und Protokolle zu suchen. Diese Informationen sind nützlich, beim Diagnostizieren von Problemen, Fehlern übermitteln oder Support anfordern.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: e389bc33538ec3c3d36eb749c746f5a4723aab3c
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830353"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Wo finde ich meine Versionsinformationen und Protokolle?

## <a name="outline"></a>Umriss

- [Versionsinformationen](#version-information)
    - Windows-Versionsinformationen
    - Mac-Versionsinformationen
    - Android SDK Tools, plattformtools, Build-tools
- [IDE und Installer-Protokolle](#ide-and-installer-logs)
    - [Windows-Protokolle](#windows-logs)
        - Xamarin Studio
        - Xamarin für Visual Studio
        - Xamarin-Universal-installer
        - Einzelne `.msi` Installationsprogramme, ausführliche Protokolle
        - Start von Visual Studio, ausführliche Protokolle
    - [Mac-Protokolle](#mac-logs)
        - Erstellen von Hosts
    - Visual Studio für Mac
        - Xamarin Studio
        - Xamarin-Installers
- [Ausführliche Buildausgabe](#verbose-build-output-logs)
- [Debuggen von Protokollen für Xamarin.Android und Xamarin.iOS-apps](#debug-logs-for-xamarin-apps)
    - Android `adb` Logcat-Protokolle
    - iOS-Simulator anmeldet (Mac)
    - iOS-Gerät anmeldet (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Versionsinformationen

Es ist in der Regel empfiehlt es sich, senden Sie sichern alle Informationen aus der **Informationen kopieren** Schaltflächen. Andernfalls müssen wir oft zusätzliche Informationen anzufordern. Z. B. Betriebssystemversionen, Xcode-Version installiert, Android-API-Ebenen, und die Version von .NET kann alle wichtig sein, wenn bei der Behandlung eines Problems helfen.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows-Versionsinformationen

#### <a name="xamarin-studio"></a>Xamarin Studio

**Hilfe > über > Details anzeigen > Informationen kopieren [Schaltfläche]**

#### <a name="visual-studio"></a>Visual Studio

**Hilfe > Info zu Microsoft Visual Studio > Info kopieren [Schaltfläche]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac-Versionsinformationen

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Visual Studio > Info zu Visual Studio > Details anzeigen > Informationen kopieren [Schaltfläche]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, plattformtools, Build-tools

Öffnen Sie den Android SDK-Manager, und erstellen Sie einen Screenshot des oberen **Tools** Abschnitt.

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Extras > Android SDK-Manager öffnen**

#### <a name="visual-studio"></a>Visual Studio

**Extras > Android > Android SDK-Manager öffnen...**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE und Installer-Protokolle

Achten Sie für jeden Speicherort Log zippen und fügen Sie den gesamten Protokollordner.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows-Protokolle

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio-Tools für Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Gewusst wie: Abrufen der Visual Studio-Installationsprotokolle](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> "Universelle" Xamarin-Installers

`%LOCALAPPDATA%\Xamarin\Universal`

Hierbei handelt es sich um die Protokolle aus der `XamarinInstaller.exe` Installer.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Einzelne `.msi` Installationsprogramme, ausführliche Protokolle

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Referenz: [Befehlszeilenoptionen](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Start von Visual Studio, ausführliche Protokolle

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Referenz:  [ /Log (devenv.exe)](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac-Protokolle

Sie können auswählen, die **wechseln > wechseln Sie zum Ordner** Menü im Finder nach Element und und fügen Sie dann eine der folgenden Pfade in das Dialogfeld.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio für Mac

`~/Library/Logs/VisualStudio/7.0` (diese Zahl kann je nach verwendetem Version ändern.)

Dieser Ordner kann auch über geöffnet werden "Hilfe-> Protokollverzeichnis öffnen".

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (diese Zahl kann je nach verwendetem Version ändern.)

Dieser Ordner kann auch über geöffnet werden "Hilfe-> Protokollverzeichnis öffnen".

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />"Universelle" Xamarin-Installers

`~/Library/Logs/XamarinInstaller/Universal`

Hierbei handelt es sich um die Protokolle aus der `XamarinInstaller.dmg` Installer.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin-Buildhost

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Ausführliche Buildausgabe

1.  Aktivieren Sie [Diagnoseausgabe von MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  Für iOS-apps auch aktivieren, **ausführliche Mtouch-Ausgabe** durch Hinzufügen von `-v -v -v -v` unter **Projekteigenschaften > iOS-Build > Allgemein (Registerkarte) > zusätzliche Optionen > Weitere Mtouch-Argumente**.

3.  Bereinigen und das Projekt neu erstellen.

4.  Kopieren Sie die Buildausgabe aus der IDE in eine Textdatei.
     - Visual Studio (Windows): **Ansicht > Ausgabe > Ausgabe anzeigen von: Build**
     - Visual Studio für Mac: **Ansicht > Pads > Fehler > Buildausgabe (Registerkarte)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Debuggen von Protokollen für Xamarin.Android und Xamarin.iOS-apps

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Ansicht > Pads > Anwendungsausgabe**

(Beachten Sie, dass dieses Menüelement wird nur angezeigt, nachdem die app gestartet wurde.)

### <a name="visual-studio"></a>Visual Studio

**Ansicht > Ausgabe > Ausgabe anzeigen von: Debuggen**

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpsdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](https://developer.android.com/tools/help/adb.html) Logcat-Protokolle

Nach dem Ausführen der `adb` Befehl ein, und schließen Sie Back der **android_logcat.txt** Datei von Ihrem Desktop. Diese Anweisungen setzen voraus, dass Sie nur ein Gerät angeschlossen haben.

Siehe auch die [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) Seite.

#### <a name="visual-studio"></a>Visual Studio

1. **Extras > Android > Android Adb-Eingabeaufforderung starten**
2. Bereinigen Sie das Protokoll an: `adb logcat -c`
3. Das Problem zu reproduzieren.
4. Ausgabe des Protokolls: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. **Extras > Android SDK-Eingabeaufforderung öffnen**
2. Bereinigen Sie das Protokoll an: `adb logcat -c`
3. Das Problem zu reproduzieren.
4. Ausgabe des Protokolls: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS-Simulator anmeldet (Mac)

* Wählen Sie den Zugriff auf das Systemprotokoll **Debuggen > System-Protokoll öffnen...**  in der iOS-Simulator-app.

* Klicken Sie zum Anzeigen der Berichte zu Abstürzen des Simulators Console.app öffnen, und navigieren Sie zu `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS-Gerät anmeldet (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Ansicht > Pads > iOS-Gerät-Protokoll**

#### <a name="xcode"></a>Xcode

**Window > Devices > ${DeviceName}**

Berichte zu Abstürzen finden Sie unter den **Gerät in den Protokollen** Schaltfläche. Das Systemprotokoll auf dem Gerät wird am unteren Rand des Fensters unter den Abwärtspfeil angezeigt.

#### <a name="xcode-5"></a>Xcode 5

**Fenster "> Organisator > Geräte (Registerkarte) > ${DeviceName}**
