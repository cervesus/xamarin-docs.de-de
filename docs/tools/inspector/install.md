---
title: Inspektor Installations- und Anforderungen
description: Dieses Dokument enthält Anweisungen zur Installation der Xamarin-Inspektor und erläutert die unterstützten Betriebssysteme, IDEs und app-Plattformen.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: f7c5217a9c2d3881ca29094c3186e448975db6a3
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268968"
---
# <a name="inspector-installation-and-requirements"></a>Inspektor Installations- und Anforderungen

## <a name="download-and-installation"></a>Download und Installation

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Herunterladen und installieren [Visual Studio Enterprise](https://www.visualstudio.com/vs/) , und wählen Sie die **Mobile Entwicklung mit .NET** arbeitsauslastung.
1. [Melden Sie sich bei](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) Ihr Enterprise-Abonnement zu aktivieren.
1. [Überprüfen Sie](~/tools/inspector/inspect.md) Ihrer eigenen app!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Herunterladen und installieren [Visual Studio für Mac](https://www.visualstudio.com/vs/mac/).
1. [Melden Sie sich bei](https://docs.microsoft.com/visualstudio/mac/activation) Ihr Enterprise-Abonnement zu aktivieren.
1. [Überprüfen Sie](~/tools/inspector/inspect.md) Ihrer eigenen app!

-----

## <a name="requirements"></a>Anforderungen

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10.11 oder größer
- **Windows** – Windows 7 oder höher (mit InternetExplorer 11 oder höher und .NET 4.6.1 oder höher)

### <a name="supported-ides"></a>Unterstützte IDEs

- Visual Studio für Mac
- Visual Studio 2017 mit **Mobile Entwicklung mit .NET** arbeitsauslastung

Aktive app Inspektion ist für Enterprise-Kunden zur Verfügung.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Unterstützte App-Plattformen

|App-Plattform|IDE-Unterstützung|Hinweise|
|--- |--- |--- |
|Mac|Nur unterstützt in Visual Studio für Mac|
|iOS|Unterstützt in Visual Studio 2017 und Visual Studio für Mac| |
|Android|Unterstützt in Visual Studio 2017 und Visual Studio für Mac|Android abzielen muss > = 4.0.3 mit **Fastdev** aktiviert.<br />Google, Visual Studio oder Xamarin Android-Emulatoren muss verwendet werden. 7 Android-Emulatoren können die Überprüfung zu diesem Zeitpunkt nicht zulässig.|
|WPF|Nur unterstützt in Visual Studio 2017|

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
