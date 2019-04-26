---
title: Workbooks-Installation und Anforderungen
description: Dieses Dokument beschreibt die Vorgehensweise zum Herunterladen und Installieren von Xamarin Workbooks, erläutern unterstützte Plattformen und die Systemanforderungen.
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: a1001163d89a9a9cda16a7ee5e644307fcc9875c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218591"
---
# <a name="workbooks-installation-and-requirements"></a>Workbooks-Installation und Anforderungen

<a name="install" />

## <a name="download-and-install"></a>Herunterladen und installieren

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Überprüfen Sie die [Anforderungen](#requirements) unten.
2. Herunterladen und installieren [Xamarin Workbooks für Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
3. Starten Sie [ein wenig herumspielen](~/tools/workbooks/workbook.md) mit Arbeitsmappen oder probieren Sie die [Beispiele](https://developer.xamarin.com/workbooks)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Überprüfen Sie die [Anforderungen](#requirements) unten.
2. Herunterladen und installieren [Xamarin Workbooks für Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
3. Starten Sie [ein wenig herumspielen](~/tools/workbooks/workbook.md) mit Arbeitsmappen oder probieren Sie die [Beispiele](https://developer.xamarin.com/workbooks)

-----

## <a name="requirements"></a>Anforderungen

#### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10.11 oder höher
- **Windows** – Windows 7 oder höher (mit InternetExplorer 11 oder höher und .NET 4.6.1 oder höher)

#### <a name="supported-app-platforms"></a>Unterstützte App-Plattformen

|App-Plattform|Betriebssystemunterstützung|Hinweise|
|--- |--- |--- |
|Mac|Nur unterstützt auf Mac.|
|iOS|Unterstützt auf Mac und Windows|Xamarin.iOS 11.0 und Xcode 9.0 oder höher muss auf einem Mac installiert werden Das Ausführen von iOS-Workbooks auf Windows erfordert eine Mac-buildhost, der alle oben genannten ausgeführt und die [iOS-Remotesimulator](~/tools/ios-simulator/index.md) auf Windows installiert.|
|Android|Unterstützt auf Mac und Windows|Verwenden Sie Google, Visual Studio oder Xamarin Android-Emulator muss mit einem virtuellen Gerät > = 5.0|
|WPF|Nur unterstützt unter Windows.|
|Konsole ((.NET Framework)|Unterstützt auf Mac und Windows|
|Konsole ((.NET Core)|Unterstützt auf Mac und Windows|


## <a name="reporting-bugs"></a>Melden von Fehlern

Bitte [Probleme auf GitHub melden][bugs], und fügen Sie die folgenden Informationen:

### <a name="log-files"></a>Protokolldateien

Fügen Sie immer die Arbeitsmappen Clientprotokolldateien aufgeführt:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Version 1.4.x bietet außerdem die Möglichkeit, die Protokolldatei im Finder (macOS) oder im Explorer (Windows) direkt über das Hauptmenü auszuwählen.

- **Hilfe > Protokolldatei anzeigen**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Log-Pfade für Arbeitsmappen 1.3 und früher:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informationen zur Version

Es ist sehr hilfreich zu wissen, Details zu Ihrem Betriebssystem und die Xamarin-Produkte installiert.

Über das Hauptmenü in Arbeitsmappen:

* **Hilfe > Versionsinformationen kopieren**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Anweisungen für Arbeitsmappen 1.3 und früher:

Visual Studio für Mac

- **Visual Studio > Informationen zu Visual Studio > Details anzeigen > Informationen kopieren**
- Einfügen in den Fehlerbericht

Visual Studio

- **Hilfe > Informationen zu Visual Studio > Info kopieren**
- Geben Sie Ihre Betriebssystemversion an und ob Sie die Windows 32-Bit-Version oder die Windows 64-Bit-Version ausführen.

### <a name="samples"></a>Proben

Wenn Sie anfügen oder verknüpfen, können die **.workbooks** Datei, die Probleme mit, die den Fehler schneller lösen kann.

### <a name="devices"></a>Geräte

Wenn Sie Probleme beim Herstellen einer IOS- oder Android-Arbeitsmappe und wurden bereits überprüft [unsere Problembehandlungsseite](~/tools/workbooks/troubleshooting/index.md), wir kennen müssen:

- Name des Geräts zum Herstellen einer Verbindung mit gewünschten
- Die Betriebssystemversion Ihres Geräts
- Android: Stellen Sie sicher, dass die Verwendung von x X86 Emulator
- Android: Welche Emulatorplattform verwenden Sie? Google-Emulatoren?
  Visual Studio-Emulator für Android? Xamarin Android Player?
- iOS auf Windows: Welche Version der Xamarin-Remote-iOS-Simulator Sie installiert haben (Überprüfen Sie **Programme hinzufügen/entfernen** in **Systemsteuerung**)?
- iOS auf Windows: Außerdem geben Sie Informationen zur Version für Ihre Mac-buildhost
- Besitzt das Gerät über eine Netzwerkverbindung (Überprüfen über Webbrowser)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Deinstallieren

### <a name="windows"></a>Windows

Je nachdem, wie Sie Arbeitsmappen erworben haben müssen Sie möglicherweise zwei Deinstallation Verfahren ausführen. Überprüfen Sie beide an die Software deinstallieren.

#### <a name="visual-studio-installer"></a>Visual Studio-Installer

Wenn Sie Visual Studio 2017 haben, öffnen Sie **Visual Studio-Installer**, und suchen Sie im **Einzelkomponenten** für **Xamarin Workbooks**. Wenn diese Option aktiviert ist, deaktivieren Sie es, und klicken Sie dann auf **ändern** deinstallieren.

#### <a name="system-uninstall"></a>System-Deinstallation

Wenn Sie Arbeitsmappen selbst mit einem heruntergeladenen Installationsprogramm installiert, es müssen deinstalliert werden, über die **Apps & Features** System Seite "Einstellungen" unter Windows 10 oder über **Programme hinzufügen/entfernen** im Steuerelement Bereich für ältere Versionen von Windows.

> **Start > Einstellungen > System > Apps & Features**

![](install-images/windows-remove.png "Xamarin Workbooks gemäß &quot;Apps &amp; Features&quot;")

**Folgen Sie weiterhin das Verfahren für die Visual Studio-Installer, um sicherzustellen, dass Arbeitsmappen werden nicht erneut ohne Ihr Wissen installiert zu erhalten.**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

Beginnend mit [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), Xamarin Workbooks über das Terminal deinstalliert werden können, indem Sie ausführen:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Das Deinstallationsprogramm wird ausführlich beschrieben, die Dateien und Verzeichnisse, die entfernt er und fragt nach einer Bestätigung, bevor Sie fortfahren.

Übergeben Sie die `-help` Argument für die `uninstall` Skript für erweiterte Szenarien.

Für frühere Versionen müssen Sie manuell Folgendes entfernen:

1. Löschen Sie die Workbooks-App unter `"/Applications/Xamarin Workbooks.app"`.
2. Löschen Sie die Inspector-App unter `"Applications/Xamarin Inspector.app"`.
2. Löschen Sie die Add-ins `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` und `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`.
3. Löschen Sie Inspector und die unterstützenden Dateien hier: `/Library/Frameworks/Xamarin.Interactive.framework` und `/Library/Frameworks/Xamarin.Inspector.framework`.

## <a name="downgrading"></a>Ein Downgrade

Die Bündel-ID für **Applications/Xamarin Workbooks.app** geändert `com.xamarin.Inspector` zu `com.xamarin.Workbooks` in der Version 1.4 als Workbooks und Inspector vollständig geteilt.

Aufgrund eines Fehlers in älteren Installationsprogrammen ist es nicht möglich, 1.4 oder neuere Releases durch die Verwendung der 1.3.2 oder ältere Installationsprogramme herabgestuft.

Um das Downgrade vom 1.4 oder neuere 1.3.2 oder älter ausgeführt:

1. [Manuelles Deinstallieren von Workbooks & Inspektor](#uninstall-macos)
2. Führen Sie die 1.3.2 oder eine frühere `.pkg` Installer
