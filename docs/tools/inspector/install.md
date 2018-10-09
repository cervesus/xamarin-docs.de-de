---
title: 'Inspector: Installation und Vorraussetzungen'
description: Dieses Dokument enthält Anweisungen zur Installation der Xamarin-Inspektor und erläutert die unterstützten Betriebssysteme, IDEs und app-Plattformen.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 690329aa1577c66b3aa2794342a8e367477d3a74
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066922"
---
# <a name="inspector-installation-and-requirements"></a>Inspector: Installation und Vorraussetzungen

## <a name="download-and-installation"></a>Download und Installation

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Herunterladen und installieren [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) , und wählen Sie die **Mobile Entwicklung mit .NET** arbeitsauslastung.
1. [Melden Sie sich an](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio), um Ihr Enterprise-Abonnement zu aktivieren.
1. [Überprüfen Sie](~/tools/inspector/inspect.md) Ihre eigene App!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Laden Sie [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) herunter, und führen Sie die Installation durch.
1. [Melden Sie sich an](https://docs.microsoft.com/visualstudio/mac/activation), um Ihr Enterprise-Abonnement zu aktivieren.
1. [Überprüfen Sie](~/tools/inspector/inspect.md) Ihre eigene App!

-----

## <a name="requirements"></a>Anforderungen

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10.11 oder größer
- **Windows** – Windows 7 oder höher (mit InternetExplorer 11 oder höher und .NET 4.6.1 oder höher)

### <a name="supported-ides"></a>Unterstützte IDEs

- Visual Studio für Mac
- Visual Studio 2017 mit der Workload **Mobile-Entwicklung mit .NET**

Die Überprüfung von Live-Apps steht für Enterprise-Kunden zur Verfügung.

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

Fehler müssen direkt über Visual Studio gemeldet werden:

- **Hilfe > Senden Sie Feedback > ein Problem melden**

Geben Sie auch alle der folgenden Informationen an:

### <a name="platform-version-information"></a>Informationen zur Version der Plattform

Geben Sie die folgenden Informationen unbedingt an:

Visual Studio für Mac

- **Visual Studio > Informationen zu Visual Studio > Details anzeigen > Informationen kopieren**
- Einfügen in den Fehlerbericht

Visual Studio

- **Hilfe > Informationen zu Visual Studio > Info kopieren**
- Geben Sie Ihre Betriebssystemversion an und ob Sie die Windows 32-Bit-Version oder die Windows 64-Bit-Version ausführen.

### <a name="log-files"></a>Protokolldateien

Fügen Sie immer die IDE und die Protokolldateien des Inspector-Clients an.

Inspector-Client

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

Version 1.4.x bietet außerdem die Möglichkeit, die Protokolldatei im Finder (macOS) oder im Explorer (Windows) direkt über das Hauptmenü auszuwählen.

- **Hilfe > Protokolldatei anzeigen**

Visual Studio für Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Der Inhalt des **Ausgabefensters** in Visual Studio ist möglicherweise informativ.

### <a name="project-settings"></a>Projekteinstellungen

Wenn Sie anfügen können, die **csproj** für das Projekt, das Sie untersuchen möchten, würde es sehr hilfreich sein. Dies ist einfacher, als Sie Fragen zu individuellen Einstellungen.

Bestätigen Sie außerdem, dass die Debugkonfiguration ausgewählt ist.

### <a name="selected-devices"></a>Ausgewählte Geräte

Für Android und iOS ist es wichtig, dass wir, welches Gerät Sie eine Verknüpfung wissen auf Debuggen, wenn Sie überprüfen möchten. Wir müssen wissen:

- Name des in der IDE dargestellten Geräts
- Die Betriebssystemversion Ihres Geräts
- Android: Stellen Sie sicher, dass Sie den x86-Emulator verwenden.
- Android: Welche Emulatorplattform verwenden Sie? Google-Emulatoren? Visual Studio-Emulator für Android? Xamarin Android Player?
- Funktioniert die App, die Sie debuggen wollen, ordnungsgemäßauf dem Gerät?
- Verfügt das Gerät über eine Netzwerkverbindung (Überprüfen über Webbrowser)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
