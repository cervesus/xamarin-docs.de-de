---
title: Installations- und Anforderungen
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: dffea94b309b3c945a37a116668486f404f544b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Installations- und Anforderungen

<script> Var InspectorOnLoad Funktion () = {Var PrimaryTextBase = "Für Arbeitsmappen Xamarin;" Var SecondaryTextBase = "oder für herunterladen"; Var InspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; Var InspectorDownloadUrlWin = " https://DL.xamarin.com/Interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  Var aMac = aPrimary; Var aWin = aSecondary; Var MacTextBase = PrimaryTextBase; Var WinTextBase = SecondaryTextBase;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>Herunterladen und installieren

<ol>
  <li>Überprüfen Sie die <a href="#Requirements"> Anforderungen</a> unten.</li>
  <li>Herunterladen und installieren <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin-Arbeitsmappen für Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">oder Download für Windows</a>).
  </li>
  <li>Starten Sie <a href="~/tools/workbooks/workbook.md"> Spielereien</a> -Arbeitsmappen oder ausprobieren der <a href="https://developer.xamarin.com/workbooks/">Beispiele</a>.
    </li>
</ol>

## <a name="requirements"></a>Anforderungen

#### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10.11 oder größer
- **Windows** – Windows 7 oder höher (mit InternetExplorer 11 oder höher und .NET 4.6.1 oder höher)

#### <a name="supported-app-platforms"></a>Unterstützte App-Plattformen

<table>
<thead>
  <tr>
    <th>App-Plattform</th>
    <th>Unterstützte Betriebssysteme</th>
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
    <td>Unterstützt für Macintosh und Windows</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 und Xcode 9.0 oder höher muss auf einem Mac installiert sein</li>
        <li>Unter Windows iOS-Arbeitsmappen ist ein Mac-Build-Host ausführen alle oben genannten erforderlich und die <a href="~/tools/ios-simulator.md">Remote-iOS-Simulator</a> unter Windows installiert.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Unterstützt für Macintosh und Windows</td>
    <td>Verwenden Sie Google, Visual Studio oder Xamarin Android-Emulator die Zeichenfolge muss mit einem virtuellen Gerät > = 5.0</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Nur unterstützt unter Windows.</td>
    <td/>
  </tr>
  <tr>
    <td>Konsole ((.NET Framework)</td>
    <td>Unterstützt für Macintosh und Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Konsole (.NET Core)</td>
    <td>Unterstützt für Macintosh und Windows</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>Melden von Fehlern

Bitte [Melden von Problemen auf GitHub][bugs], und alle der folgenden Informationen enthalten:

### <a name="log-files"></a>Protokolldateien

Fügen Sie immer die Clientprotokolldateien Arbeitsmappen:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x installiert sein bietet außerdem die Möglichkeit, die Protokolldatei im Finder (MacOS) oder im Explorer (Windows) direkt über das Hauptmenü auswählen:

- **Protokolldatei für Hilfe → anzeigen**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Log-Pfade für Arbeitsmappen 1.3 und früher:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informationen zur Version der Plattform

Es ist sehr hilfreich zu wissen, Details zu Ihrem Betriebssystem und Xamarin-Produkte installiert.

Über das Hauptmenü in Arbeitsmappen:

* **Hilfe → Kopie-Versionsinformationen**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Anweisungen für Arbeitsmappen 1.3 und früher:

Visual Studio für Mac

- **Visual Studio → Informationen über Visual Studio → anzeigen → Kopie Detailinformationen**
- Fügen Sie in den Bericht "Fehlerstatus"

Visual Studio

- **Hilfe zu Visual Studio → Kopie Informationen →**
- Informieren Sie uns Ihre Betriebssystemversion und gibt an, ob Sie 32-Bit oder 64-Bit-Windows ausgeführt werden.

### <a name="samples"></a>Proben

Wenn Sie anfügen oder verknüpfen, können die `.workbooks` treten Probleme mit, Datei, die möglicherweise helfen, den Fehler schneller zu lösen.

### <a name="devices"></a>Geräte

Wenn Sie Probleme beim Herstellen einer Verbindung Ihres IOS- oder Android-Arbeitsmappe und wurden bereits überprüft [unsere zur Problembehandlung Seite](~/tools/workbooks/troubleshooting/index.md), benötigen wir wissen:

- Name des Geräts, die Sie herstellen möchten
- Die Betriebssystemversion Ihres Geräts
- Android: Stellen Sie sicher, dass die Verwendung von x X86 Emulator
- Android: Welche Emulatorplattform verwenden Sie? -Emulators für Google?
  Visual Studio-Android-Emulator? Xamarin Android Player?
- iOS unter Windows: Welche Version der Xamarin-Remote-iOS-Simulator Sie installiert haben (Überprüfen Sie `Add/Remove Programs` in `Control Panel`)?
- iOS unter Windows: auch Geben Sie Informationen zur Version der Plattform für den Mac-Build-Host
- Verfügt das Gerät über eine Netzwerkverbindung (Überprüfen über Webbrowser)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Deinstallieren

### <a name="windows"></a>Windows

Je nachdem, wie Sie Arbeitsmappen & Inspektor erworben haben müssen Sie möglicherweise zwei Verfahren für die Deinstallation ausführen. Überprüfen Sie beide auf die Software zu deinstallieren.

#### <a name="visual-studio-installer"></a>Visual Studio-Installer

Wenn Sie Visual Studio 2017 haben, öffnen Sie **Installer für Visual Studio**, und suchen Sie im **Einzelkomponenten** für **Xamarin Arbeitsmappen**. Wenn diese Option aktiviert ist, deaktivieren Sie es, und klicken Sie dann auf **ändern** deinstallieren.

#### <a name="system-uninstall"></a>System-Deinstallation

Wenn Sie Arbeitsmappen & Inspektor selbst mit einem heruntergeladenen Installer installiert haben, müssen sie über deinstalliert werden die **Apps & Features** System Seite "Einstellungen" unter Windows 10 oder über **Programme hinzufügen/entfernen**in der Systemsteuerung unter älteren Versionen von Windows.

> **Starten Sie → Einstellungen → System → Apps & Features**

![](install-images/windows-remove.png "Xamarin-Arbeitsmappen und Inspektor gemäß &quot;Apps &amp; Funktionen&quot;")

**Befolgen Sie die Prozedur für Installer für Visual Studio, stellen Sie sicher, dass Arbeitsmappen noch & Inspektor wird nicht erneut ohne Ihr Wissen installiert abrufen.**

<a name="uninstall-macos" />

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

## <a name="downgrading"></a>Ausführen eines Downgrades für

Die Paket-ID für `/Applications/Xamarin Workbooks.app` angebotskennzeichen `com.xamarin.Inspector` zu `com.xamarin.Workbooks` in der Version 1,4, um zu ermöglichen, eine zukünftige Aufteilen der Xamarin-Arbeitsmappen & Inspektor Installationsprogramme.

Aufgrund eines Fehlers in älteren Installationsprogramme ist es nicht möglich, 1.4 oder neuere Versionen mit dem 1.3.2 oder ältere Installationsprogramme herabgestuft werden.

Von 1.4 oder älter oder neuer auf 1.3.2 herabgestuft werden:

1. [Deinstallieren Sie Arbeitsmappen & Inspektor manuell](#macOS)
2. Führen Sie die 1.3.2 oder eine ältere `.pkg` Installer