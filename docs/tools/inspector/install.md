---
title: Inspektor Installations- und Anforderungen
description: Zum Herunterladen, installieren und verwenden Sie die Xamarin-Inspektor.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: 5bbd5c64f53e191d5ac629e20df87c2b7ca4ec00
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="inspector-installation-and-requirements"></a>Inspektor Installations- und Anforderungen

## <a name="download-and-installation"></a>Download und Installation


# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Herunterladen und installieren [Xamarin Arbeitsmappen & Inspektor-Fenstern für](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
2. [Überprüfen Sie Ihre eigene app!](~/tools/inspector/inspect.md)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Herunterladen und installieren [Xamarin Arbeitsmappen & Inspektor für Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
2. [Überprüfen Sie Ihre eigene app!](~/tools/inspector/inspect.md)

-----

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

|App-Plattform|IDE-Unterstützung|Hinweise|
|--- |--- |--- |
|Mac (Unified)|Nur unterstützt auf Mac.|
|iOS (Unified)|Unterstützt XS und Visual Studio|Überprüfen die iOS-apps aus Windows muss die gleiche Version des Inspektors auch auf dem Mac-Build-Host installiert werden.|
|Android|Unterstützt XS und Visual Studio|Android abzielen muss > = 4.0.3 mit **Fastdev** aktiviert.<br />Google, Visual Studio oder Xamarin Android-Emulatoren muss verwendet werden. 7 Android-Emulatoren können die Überprüfung zu diesem Zeitpunkt nicht zulässig.|
|WPF|Nur unterstützt in Visual Studio unter Windows.|


<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Melden von Fehlern

Der gemeldete Fehler sollte direkt über Visual Studio werden:

- **Hilfe > Senden Sie Feedback > ein Problem melden**

Geben Sie auch alle der folgenden Informationen:

### <a name="platform-version-information"></a>Informationen zur Version der Plattform

Diese Informationen unbedingt ausgiebig vor.

Visual Studio für Mac

- **Visual Studio > Informationen zu Visual Studio > Details anzeigen > Informationen kopieren**
- Fügen Sie in den Bericht "Fehlerstatus"

Xamarin Studio

- **Xamarin Studio > Informationen zu Xamarin Studio > Details anzeigen > Informationen kopieren**
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

- **Hilfe > Protokolldatei anzeigen**

Visual Studio für Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Der Inhalt von Visual Studio **Ausgabe** Bereich möglicherweise informativ.

### <a name="project-settings"></a>Projekteinstellungen

Wenn Sie anfügen können, die **csproj** für das Projekt, das Sie untersuchen möchten, würde es sehr hilfreich sein. Dies ist einfacher, als Sie Fragen zu individuellen Einstellungen.

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

Wenn Sie Visual Studio 2017 haben, öffnen Sie **Installer für Visual Studio**, und suchen Sie im **Einzelkomponenten** für **Xamarin Arbeitsmappen**. Wenn diese Option aktiviert ist, deaktivieren Sie es, und klicken Sie dann auf "Ändern", um zu deinstallieren.

#### <a name="system-uninstall"></a>System-Deinstallation

Wenn Sie Arbeitsmappen & Inspektor selbst mit einem heruntergeladenen Installer installiert haben, müssen sie über deinstalliert werden die **Apps & Features** System Seite "Einstellungen" unter Windows 10 oder über **Programme hinzufügen/entfernen**in der Systemsteuerung unter älteren Versionen von Windows.

> **Start > Einstellungen > System > Apps & Features**

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

