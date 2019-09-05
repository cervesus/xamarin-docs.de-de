---
title: 'Inspector: Installation und Vorraussetzungen'
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Inspector installieren und die unterstützten Betriebssysteme, IDES und App-Plattformen erörtert werden.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: conceptdev
ms.author: crdun
ms.date: 06/19/2018
ms.openlocfilehash: 1273a51d29d7abcbecb9b19ae42e111db8ccc06c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292299"
---
# <a name="inspector-installation-and-requirements"></a>Inspector: Installation und Vorraussetzungen

## <a name="download-and-installation"></a>Herunterladen und installieren

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Laden Sie [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) herunter, und führen Sie die Installation durch. Wählen Sie die Workload **Mobile-Entwicklung mit .NET** aus.
1. [Melden Sie sich an](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio), um Ihr Enterprise-Abonnement zu aktivieren.
1. [Überprüfen Sie](~/tools/inspector/inspect.md) Ihre eigene App!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Laden Sie [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) herunter, und führen Sie die Installation durch.
1. [Melden Sie sich an](https://docs.microsoft.com/visualstudio/mac/activation), um Ihr Enterprise-Abonnement zu aktivieren.
1. [Überprüfen Sie](~/tools/inspector/inspect.md) Ihre eigene App!

-----

## <a name="requirements"></a>Anforderungen

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10,11 oder höher
- **Windows** -Windows 7 oder höher (mit Internet Explorer 11 oder höher und .NET 4.6.1 oder höher)

### <a name="supported-ides"></a>Unterstützte IDEs

- Visual Studio für Mac
- Visual Studio 2017 mit der Workload **Mobile-Entwicklung mit .NET**

Die Überprüfung von Live-Apps steht für Enterprise-Kunden zur Verfügung.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Unterstützte Anwendungsplattformen

|App-Plattform|IDE-Unterstützung|Hinweise|
|--- |--- |--- |
|Mac|Wird nur in Visual Studio für Mac unterstützt.|
|iOS|Unterstützt in Visual Studio 2017 und Visual Studio für Mac| Das Linker-Verhalten muss auf " **nicht verknüpfen** " festgelegt werden (unter **IOS** -buildprojektoptionen) |
|Android|Unterstützt in Visual Studio 2017 und Visual Studio für Mac|Muss Android mit Version 4.0.3 oder höher anzielen, für die **Fastdev** aktiviert ist.<br />Google-, Visual Studio- oder Xamarin Android-Emulatoren müssen verwendet werden. In Android 7-Emulatoren sind Überprüfungen derzeit womöglich nicht zulässig.|
|WPF|Wird nur in Visual Studio 2017 unterstützt.|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Melden von Fehlern

Fehler müssen direkt über Visual Studio gemeldet werden:

- **Hilfe > Senden von Feedback > Melden eines Problems**

Geben Sie auch alle der folgenden Informationen an:

### <a name="platform-version-information"></a>Informationen zur Platt Form Version

Geben Sie die folgenden Informationen unbedingt an:

Visual Studio für Mac

- **Visual Studio-> zu Visual Studio > Details > Kopier Informationen anzeigen**
- Einfügen in den Fehlerbericht

Visual Studio

- **Hilfe > zu Visual Studio > Kopieren von Informationen**
- Geben Sie Ihre Betriebssystemversion an und ob Sie die Windows 32-Bit-Version oder die Windows 64-Bit-Version ausführen.

### <a name="log-files"></a>Protokolldateien

Fügen Sie immer die IDE und die Protokolldateien des Inspector-Clients an.

Inspector-Client

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

Version 1.4.x bietet außerdem die Möglichkeit, die Protokolldatei im Finder (macOS) oder im Explorer (Windows) direkt über das Hauptmenü auszuwählen.

- **Hilfe > Anzeigen der Protokolldatei**

Visual Studio für Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Der Inhalt des **Ausgabefensters** in Visual Studio ist möglicherweise informativ.

### <a name="project-settings"></a>Projekteinstellungen

Es wäre sehr hilfreich, wenn Sie die **csproj**-Datei für das Projekt, das Sie untersuchen möchten, anfügen können Dies ist einfacher, als Ihnen Fragen zu individuellen Einstellungen zu stellen.

Bestätigen Sie außerdem, dass die Debugkonfiguration ausgewählt ist.

### <a name="selected-devices"></a>Ausgewählte Geräte

Für Android und iOS ist es wichtig, dass wir wissen, welches Gerät Sie zum Debuggen ausgewählt haben: Wir müssen Folgendes wissen:

- Name des in der IDE dargestellten Geräts
- Betriebssystemversion Ihres Geräts
- Android: Überprüfen Sie, ob Sie einen x86-Emulator verwenden
- Android: Welche Emulatorplattform verwenden Sie? Google-Emulatoren? Visual Studio-Emulator für Android? Xamarin Android Player?
- Funktioniert die App, die Sie debuggen wollen, ordnungsgemäßauf dem Gerät?
- Hat das Gerät Netzwerk Konnektivität (überprüfen Sie den Webbrowser)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
