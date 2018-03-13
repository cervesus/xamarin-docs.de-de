---
title: Installations- und Anforderungen
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 0bdcd00b64d581d4c5179bc39ded4018c9374229
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="installation-and-requirements"></a>Installations- und Anforderungen

<script> Var InspectorOnLoad Funktion () = {Var PrimaryTextBase = "Xamarin Arbeitsmappen & Inspektor für" Var SecondaryTextBase = "oder für herunterladen"; Var InspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; Var InspectorDownloadUrlWin = "https://dl.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  Var aMac = aPrimary; Var aWin = aSecondary; Var MacTextBase = PrimaryTextBase; Var WinTextBase = SecondaryTextBase;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>Download und Installation

<ol>
  <li>Herunterladen und installieren <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin Arbeitsmappen & Inspektor für Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">oder Download für Windows</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> Überprüfen Sie Ihre eigene app!</a>
    </li>
</ol>

## <a name="requirements"></a>Anforderungen

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10.11 oder größer
- **Windows** – Windows 7 oder höher (mit InternetExplorer 11 oder höher und .NET 4.6.1 oder höher)

### <a name="supported-ides"></a>Unterstützte IDEs

- Xamarin Studio 6.2 oder höher
- Visual Studio für Mac Preview 4 oder höher
- Visual Studio 2015 mit Xamarin 4.3.x oder höher
- Visual Studio 2017 mit Xamarin-Arbeitslast

Aktive app Inspektion ist für Enterprise-Kunden zur Verfügung.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Unterstützte App-Plattformen

<table>
<thead>
  <tr>
    <th>App-Plattform</th>
    <th>IDE-Unterstützung</th>
    <th>Hinweise</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (Unified)</td>
    <td>Nur unterstützt auf Mac.</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (Unified)</td>
    <td>Unterstützt XS und Visual Studio</td>
    <td>Überprüfen die iOS-apps aus Windows muss die gleiche Version des Inspektors auch auf dem Mac-Build-Host installiert werden.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Unterstützt XS und Visual Studio</td>
    <td>
      <ul>
        <li>Android abzielen muss > = 4.0.3</li>
        <li>Fastdev haben, muss aktiviert werden</li>
        <li>Google, Visual Studio oder Xamarin Android-Emulatoren muss verwendet werden. 7 Android-Emulatoren können die Überprüfung zu diesem Zeitpunkt nicht zulässig.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Nur unterstützt in Visual Studio unter Windows.</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Melden von Fehlern

Der gemeldete Fehler sollte direkt über Visual Studio werden:

- **Hilfe → Send Feedback → Bericht ein Problem**

Geben Sie auch alle der folgenden Informationen:

### <a name="platform-version-information"></a>Informationen zur Version der Plattform

Diese Informationen unbedingt ausgiebig vor.

Visual Studio für Mac

- **Visual Studio → Informationen über Visual Studio → anzeigen → Kopie Detailinformationen**
- Fügen Sie in den Bericht "Fehlerstatus"

Xamarin Studio

- **Xamarin Studio → zum Anzeigen von Xamarin Studio → Details → Kopieinformationen**
- Fügen Sie in den Bericht "Fehlerstatus"

Visual Studio

- **Hilfe > Informationen zu Visual Studio > Info kopieren**
- Informieren Sie uns Ihre Betriebssystemversion und gibt an, ob Sie 32-Bit oder 64-Bit-Windows ausgeführt werden.

### <a name="log-files"></a>Protokolldateien

Fügen Sie immer die IDE und Inspektor Clientprotokolldateien.

Inspector-client

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x installiert sein bietet außerdem die Möglichkeit, die Protokolldatei im Finder (MacOS) oder im Explorer (Windows) direkt über das Hauptmenü auswählen:

- **Protokolldatei für Hilfe → anzeigen**

Visual Studio für Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Der Inhalt von Visual Studio `Output` Bereich möglicherweise informativ.

### <a name="project-settings"></a>Projekteinstellungen

Wenn Sie anfügen können die `.csproj` für das Projekt, das Sie untersuchen möchten, würde es sehr hilfreich sein. Dies ist einfacher, als Sie Fragen zu individuellen Einstellungen.

Bestätigen Sie außerdem, dass der Debug-Konfiguration ist.

### <a name="selected-devices"></a>Ausgewählte Geräte

Für Android und iOS ist es wichtig, dass wir, welches Gerät Sie eine Verknüpfung wissen auf Debuggen, wenn Sie überprüfen möchten. Wir müssen wissen:

- Name des Geräts in der IDE dargestellten
- Die Betriebssystemversion Ihres Geräts
- Android: Stellen Sie sicher, dass die Verwendung von x X86 Emulator
- Android: Welche Emulatorplattform verwenden Sie? -Emulators für Google? Visual Studio-Android-Emulator? Xamarin Android Player?
- Die app ordnungsgemäß debuggenden angezeigt werden und die Funktion im Gerät?
- Verfügt das Gerät über eine Netzwerkverbindung (Überprüfen über Webbrowser)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Deinstallieren

### <a name="windows"></a>Windows

Je nachdem, wie Sie Arbeitsmappen & Inspektor erworben haben müssen Sie möglicherweise zwei Verfahren für die Deinstallation ausführen. Überprüfen Sie beide auf die Software zu deinstallieren.

#### <a name="visual-studio-installer"></a>Visual Studio-Installer

Wenn Sie Visual Studio 2017 haben, öffnen Sie "Installer für Visual Studio", und suchen Sie nach "Xamarin-Arbeitsmappen" in "Einzelkomponenten". Wenn diese Option aktiviert ist, deaktivieren Sie es, und klicken Sie dann auf "Ändern", um zu deinstallieren.

#### <a name="system-uninstall"></a>System-Deinstallation

Wenn Sie Arbeitsmappen & Inspektor selbst mit einem heruntergeladenen Installer installiert haben, müssen sie über deinstalliert werden die **Apps & Features** System Seite "Einstellungen" unter Windows 10 oder über **Programme hinzufügen/entfernen**in der Systemsteuerung unter älteren Versionen von Windows.

> **Starten Sie → Einstellungen → System → Apps & Features**

![](install-images/windows-remove.png "Xamarin-Arbeitsmappen und Inspektor gemäß "Apps & Features"")

**Befolgen Sie die Prozedur für Installer für Visual Studio, stellen Sie sicher, dass Arbeitsmappen noch & Inspektor wird nicht erneut ohne Ihr Wissen installiert abrufen.**

### <a name="macos"></a>macOS

Beginnend mit [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), Xamarin-Arbeitsmappen & Inspektor können von einem abschließenden deinstalliert werden, durch ausführen:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Das Deinstallationsprogramm enthält ausführliche Informationen, die Dateien und Verzeichnisse, die entfernt er und fragt nach einer Bestätigung, bevor Sie fortfahren.

Übergeben der `-help` Argument an die `uninstall` -Skript für erweiterte Szenarien.

Für frühere Versionen müssen Sie manuell Folgendes entfernen:

1. Löschen Sie die Workbooks-App unter `"/Applications/Xamarin Workbooks.app"`.
2. Löschen Sie die Inspector-App unter `"Applications/Xamarin Inspector.app"`.
2. Löschen Sie die Add-ins `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` und `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`.
3. Löschen Sie Inspector und die unterstützenden Dateien hier: `/Library/Frameworks/Xamarin.Interactive.framework` und `/Library/Frameworks/Xamarin.Inspector.framework`.

