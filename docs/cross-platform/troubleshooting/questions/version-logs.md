---
title: Wo finde ich meine Versionsinformationen und Protokolle?
description: Dieses Dokument beschreibt, wo Sie nachschauen können um Versionsinformationen Xamarin und Protokolle zu suchen. Diese Informationen sind nützlich, beim Diagnostizieren von Problemen, Programmfehler oder Support erhalten.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 0164c5b5cad972b2d8854aefd4403e287a50a6e0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782049"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Wo finde ich meine Versionsinformationen und Protokolle?

## <a name="outline"></a>Gliederung

- [Versionsinformationen](#version-information)
    - Windows-Versionsinformationen
    - Mac-Versionsinformationen
    - Android SDK-Tools, Platform-Tools-Buildtools
- [IDE und Installer-Protokolle](#ide-and-installer-logs)
    - [Windows-Protokolle](#windows-logs)
        - Xamarin Studio
        - Xamarin für Visual Studio
        - Xamarin-Universal-installer
        - Einzelne `.msi` Installationsprogramme, ausführliche Protokolle
        - Visual Studio starten, ausführliche Protokolle
    - [Mac-Protokolle](#mac-logs)
        - Host erstellen
    - Visual Studio für Mac
        - Xamarin Studio
        - Xamarin-installer
- [Ausführliche Buildausgabe](#verbose-build-output-logs)
- [Debugprotokolle für Xamarin.Android und Xamarin.iOS-apps](#debug-logs-for-xamarin-apps)
    - Android `adb` Logcat Protokolle
    - iOS-Simulator anmeldet (Mac)
    - iOS-Gerät anmeldet (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Versionsinformationen

Es ist in der Regel empfiehlt es sich, senden Sichern aller Daten aus der **Informationen kopieren** Schaltflächen. Andernfalls müssen wir oft zusätzliche Informationen anzufordern. Z. B. Betriebssystemversionen, Xcode-Version installiert, Android-API-Ebenen und .NET Version kann alle wichtig sein, wenn bei der Behandlung eines Problems zu helfen.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows-Versionsinformationen

#### <a name="xamarin-studio"></a>Xamarin Studio

**Hilfe > über > Details anzeigen > Informationen kopieren [Schaltfläche]**

#### <a name="visual-studio"></a>Visual Studio

**Hilfe > zu Microsoft Visual Studio > Info kopieren [Schaltfläche]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac-Versionsinformationen

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Visual Studio > Visual Studio > Details anzeigen > Informationen kopieren [Schaltfläche]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK-Tools, Platform-Tools-Buildtools

Öffnen Sie den Android SDK-Manager, und erstellen Sie einen Screenshot des oberen Rands **Tools** Abschnitt.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Screenshot des Android SDK-Manager > Ordner \"Tools\"")

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Extras > Öffnen Sie den Android SDK-Manager**

#### <a name="visual-studio"></a>Visual Studio

Wählen Sie die SDK-Manager-Symbolleiste auf das Symbol ein:

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Starten Sie das Symbol für Android SDK Manager-Symbolleiste in Visual Studio")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE und Installer-Protokolle

Achten Sie für jede Protokollspeicherort komprimieren, und fügen Sie den gesamten Protokollordner.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows-Protokolle

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio-Tools für Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio-2017

[Gewusst wie: Abrufen der Visual Studio-Installationsprotokolle](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> "Universal" Xamarin-installer

`%LOCALAPPDATA%\Xamarin\Universal`

Hierbei handelt es sich um die Protokolle von der `XamarinInstaller.exe` Installer.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Einzelne `.msi` Installationsprogramme, ausführliche Protokolle

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Referenz: [Befehlszeilenoptionen](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio starten, ausführliche Protokolle

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Referenz:  [ /Log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac-Protokolle

Sie können auswählen, die **wechseln > wechseln Sie zum Ordner** im Menü Element im Finder, und klicken Sie dann kopieren Sie diesen Pfaden in das Dialogfeld ".

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio für Mac

`~/Library/Logs/VisualStudio/7.0` (diese Zahl kann abhängig von der verwendeten Version ändern.)

Dieser Ordner kann auch über geöffnet werden "-> Open Log Directory Help".

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (diese Zahl kann abhängig von der verwendeten Version ändern.)

Dieser Ordner kann auch über geöffnet werden "-> Open Log Directory Help".

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />"Universal" Xamarin-installer

`~/Library/Logs/XamarinInstaller/Universal`

Hierbei handelt es sich um die Protokolle von der `XamarinInstaller.dmg` Installer.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin-Build-Host

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Ausführliche Buildausgabe

1.  Aktivieren Sie [Diagnoseausgabe MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  Für iOS-apps auch aktivieren **Mtouch ausführliche Ausgabe** durch Hinzufügen von `-v -v -v -v` unter **Projekteigenschaften > iOS-Build > Allgemein [Tab] > Zusatzoptionen > Mtouch zusätzliche Argumente**.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "Geben Sie vier Dash V in das Eingabefeld für die zusätzliche Mtouch Argumente")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio für Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "Geben Sie vier Dash V in das Eingabefeld für die zusätzliche Mtouch Argumente")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  Bereinigen und das Projekt neu.

4.  Kopieren Sie die Buildausgabe aus der IDE in eine Textdatei.
     - Visual Studio (Windows): **Ansicht > Ausgabe > Ausgabe anzeigen von: Build**
     - Visual Studio für Mac: **Ansicht > füllt > Fehler > Ausgabe erstellen [Tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Debugprotokolle für Xamarin.Android und Xamarin.iOS-apps

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Ansicht > füllt > Anwendungsausgabe**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Pad-Symbol für Ausgabe der Anwendung in Visual Studio für Mac")


(Beachten Sie, dass dieses Menüelement wird nur angezeigt, nachdem die app gestartet wurde.)

### <a name="visual-studio"></a>Visual Studio

**Ansicht > Ausgabe > Ausgabe anzeigen von: Debuggen**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Ausgabe mit Leerstellen Auffüllen mit Debug-Option in Visual Studio")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) Logcat Protokolle

Nach dem Ausführen der `adb` Befehl ein, und schließen Sie wieder die **android_logcat.txt** Datei von Ihrem Desktop. Diese Anweisungen gehen davon aus, dass Sie nur ein Gerät angefügt haben.

Siehe auch die [Android Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) Seite.

#### <a name="visual-studio"></a>Visual Studio

1.  **Extras > Android > Android Adb Eingabeaufforderung starten**
2.  Bereinigen Sie das Protokoll an: `adb logcat -c`
3.  Das Problem zu reproduzieren.
4.  Ausgabe des Protokolls: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1.  **Extras > Android SDK-Eingabeaufforderung öffnen**
2.  Bereinigen Sie das Protokoll an: `adb logcat -c`
3.  Das Problem zu reproduzieren.
4.  Ausgabe des Protokolls: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS-Simulator anmeldet (Mac)

*   Wählen Sie den Zugriff auf das Systemprotokoll **Debuggen > System-Protokoll öffnen...**  in die app für iOS-Simulator.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "Mit der Option \"Open Systemprotokoll\" Menü \"Debuggen\"")

*   Klicken Sie zum Anzeigen von Absturzberichten im Simulator Console.app öffnen, und navigieren Sie zu `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS-Gerät anmeldet (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

**Ansicht > füllt > iOS-Gerät-Protokoll**

#### <a name="xcode"></a>Xcode 

**Fenster > Geräte > ${DeviceName}**

Absturzberichte stehen unter den **Ansicht Geräteprotokolle** Schaltfläche. Das Systemprotokoll auf dem Gerät angezeigt wird, am unteren Rand des Fensters unter der Offenlegung von-oben-Taste <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />.

#### <a name="xcode-5"></a>Xcode 5

**Fenster > Planer > Geräte [Tab] > ${DeviceName}**
