---
title: Arbeitsmappen-Installation und-Anforderungen
description: In diesem Dokument wird beschrieben, wie Sie Xamarin Workbooks herunterladen und installieren, welche unterstützten Plattformen und Systemanforderungen erörtert werden
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: conceptdev
ms.author: crdun
ms.date: 06/19/2018
ms.openlocfilehash: 5292a052d4f93af9b21cc7cbc51891c99d6f9403
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285250"
---
# <a name="workbooks-installation-and-requirements"></a>Arbeitsmappen-Installation und-Anforderungen

<a name="install" />

## <a name="download-and-install"></a>Herunterladen und installieren

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Überprüfen Sie die unten aufgeführten [Anforderungen](#requirements) .
2. Laden Sie [Xamarin Workbooks für Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi)herunter, und installieren Sie es.
3. [Spielen](~/tools/workbooks/workbook.md) Sie mit Arbeitsmappen.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Überprüfen Sie die unten aufgeführten [Anforderungen](#requirements) .
2. Laden Sie [Xamarin Workbooks für Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg)herunter, und installieren Sie es.
3. Beginnen Sie mit der wieder [Gabe](~/tools/workbooks/workbook.md).

-----

## <a name="requirements"></a>Anforderungen

#### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10,11 oder höher
- **Windows** -Windows 7 oder höher (mit Internet Explorer 11 oder höher und .NET 4.6.1 oder höher)

#### <a name="supported-app-platforms"></a>Unterstützte Anwendungsplattformen

|App-Plattform|Betriebssystemunterstützung|Hinweise|
|--- |--- |--- |
|Mac|Nur auf Mac unterstützt|
|iOS|Unterstützt unter Mac und Windows|Xamarin. IOS 11,0 und Xcode 9,0 oder höher müssen auf dem Mac installiert sein. Zum Ausführen von IOS-Arbeitsmappen unter Windows ist ein Mac-buildhost erforderlich, auf dem alle oben aufgeführten und der [Remote-IOS-Simulator](~/tools/ios-simulator/index.md) unter Windows installiert ist.|
|Android|Unterstützt unter Mac und Windows|Muss Google, Visual Studio oder den xamarin Android-Emulator mit einem virtuellen Gerät > = 5,0|
|WPF|Nur unter Windows unterstützt|
|Konsole (.NET Framework)|Unterstützt unter Mac und Windows|
|Konsole (.net Core)|Unterstützt unter Mac und Windows|


## <a name="reporting-bugs"></a>Melden von Fehlern

[Melden Sie Probleme auf GitHub][bugs], und schließen Sie alle der folgenden Informationen ein:

### <a name="log-files"></a>Protokolldateien

Arbeitsmappen-Client Protokolldateien immer anfügen:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Version 1.4.x bietet außerdem die Möglichkeit, die Protokolldatei im Finder (macOS) oder im Explorer (Windows) direkt über das Hauptmenü auszuwählen.

- **Hilfe > Anzeigen der Protokolldatei**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Protokoll Pfade für Arbeitsmappen 1,3 und früher:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informationen zur Platt Form Version

Es ist sehr hilfreich, Details zu Ihrem Betriebs System und den installierten xamarin-Produkten zu kennen.

Über das Hauptmenü in-Arbeitsmappen:

- **Hilfe > Kopieren von Versionsinformationen**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Anweisungen für Arbeitsmappen 1,3 und früher:

Visual Studio für Mac

- **Visual Studio-> zu Visual Studio > Details > Kopier Informationen anzeigen**
- Einfügen in den Fehlerbericht

Visual Studio

- **Hilfe > zu Visual Studio > Kopieren von Informationen**
- Geben Sie Ihre Betriebssystemversion an und ob Sie die Windows 32-Bit-Version oder die Windows 64-Bit-Version ausführen.

### <a name="samples"></a>Proben

Wenn Sie die **Arbeits** Mappen-Datei anfügen oder verknüpfen können, mit der Sie Probleme haben, können Sie den Fehler schneller lösen.

### <a name="devices"></a>Geräte

Wenn Sie Probleme beim Verbinden Ihrer IOS-oder Android-Arbeitsmappe haben und die [Seite zur Problem](~/tools/workbooks/troubleshooting/index.md)Behandlung bereits aktiviert haben, müssen wir Folgendes wissen:

- Der Name des Geräts, mit dem Sie eine Verbindung herstellen möchten.
- Betriebssystemversion Ihres Geräts
- Android: Überprüfen Sie, ob Sie einen x86-Emulator verwenden
- Android: Welche Emulatorplattform verwenden Sie? Google-Emulatoren?
  Visual Studio-Emulator für Android? Xamarin Android Player?
- IOS unter Windows: Welche Version des IOS-remoteios-Simulators haben Sie installiert **(überprüfen Sie die** Option "Software" in der **Systemsteuerung**)?
- IOS unter Windows: Geben Sie auch Informationen zur Platt Form Version für den Mac-buildhost an
- Hat das Gerät Netzwerk Konnektivität (überprüfen Sie den Webbrowser)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Deinstallieren

### <a name="windows"></a>Windows

Je nachdem, wie Sie Arbeitsmappen erworben haben, müssen Sie möglicherweise zwei Installations Prozeduren ausführen. Überprüfen Sie beide, um die Software vollständig zu deinstallieren.

#### <a name="visual-studio-installer"></a>Visual Studio-Installer

Wenn Sie über Visual Studio 2017 verfügen, öffnen Sie **Visual Studio-Installer**, und suchen Sie in den **einzelnen Komponenten** nach **Xamarin Workbooks**. Wenn diese Option aktiviert ist, deaktivieren Sie Sie, und klicken Sie dann auf zum Deinstallieren **ändern** .

#### <a name="system-uninstall"></a>System Deinstallation

Wenn Sie Arbeitsmappen selbst mit einem heruntergeladenen Installationsprogramm installiert haben, muss es über die Seite " **Apps & Features** Systemeinstellungen" unter Windows 10 oder über "Software **" in der Systemsteuerung unter älteren** Versionen von Windows deinstalliert werden.

> **Starten der > Einstellungen > System > Apps & Features**

![](install-images/windows-remove.png "Xamarin Workbooks wie unter &quot;APP &amp; -Features aufgelistet&quot;")

**Befolgen Sie die Schritte für die Visual Studio-Installer, um sicherzustellen, dass Arbeitsmappen ohne ihr wissen nicht neu installiert werden.**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

Ab [1.2.2](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/interactive/interactive-1.2.md)können Xamarin Workbooks über ein Terminal deinstalliert werden, indem Sie Folgendes ausführen:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Der Deinstallationsprogramm erläutert die Dateien und Verzeichnisse, die entfernt werden, und fordert die Bestätigung an, bevor der Vorgang fortgesetzt wird.

Übergeben Sie `-help` das Argument für `uninstall` Erweiterte Szenarien an das Skript.

Für frühere Versionen müssen Sie manuell Folgendes entfernen:

1. Löschen Sie die Workbooks-App unter `"/Applications/Xamarin Workbooks.app"`.
2. Löschen Sie die Inspector-App unter `"Applications/Xamarin Inspector.app"`.
3. Löschen Sie die Add-ins `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` und `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`.
4. Löschen Sie Inspector und die unterstützenden Dateien hier: `/Library/Frameworks/Xamarin.Interactive.framework` und `/Library/Frameworks/Xamarin.Inspector.framework`.

## <a name="downgrading"></a>Downgrade

Die Bündel-ID für **Applications/xamarin Workbooks. app** wurde `com.xamarin.Inspector` `com.xamarin.Workbooks` in der Version 1,4 von in geändert, da Arbeitsmappen und Inspektor nun vollständig aufgeteilt sind.

Aufgrund eines Fehlers in älteren Installationsprogrammen ist es nicht möglich, 1,4 oder neuere Versionen mit den Installationsprogrammen von 1.3.2 oder älter herabzusetzen.

So stufen Sie ein Downgrade von 1,4 oder höher auf 1.3.2 oder älter durch:

1. [Manuelles Deinstallieren von Arbeitsmappen & Inspektor](#uninstall-macos)
2. Ausführen des Installationsprogramms von `.pkg` 1.3.2 oder älter
