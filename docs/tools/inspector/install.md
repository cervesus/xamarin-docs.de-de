---
title: 'Inspector: Installation und Vorraussetzungen'
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Inspector installieren und die unterstützten Betriebssysteme, IDES und App-Plattformen erörtert werden.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 19c4a15fb2490c7bace4798b0cb8e062b1379a04
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306302"
---
# <a name="inspector-installation-and-requirements"></a>Inspector: Installation und Vorraussetzungen

## <a name="download-and-installation"></a>Herunterladen und installieren

# <a name="windows"></a>[Windows](#tab/windows)

1. Laden Sie [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) herunter, und installieren Sie die **Mobile-Entwicklung mit .net** -Arbeitsauslastung.
1. [Melden](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) Sie sich an, um Ihr Unternehmens Abonnement zu aktivieren.
1. Über [prüfen](~/tools/inspector/inspect.md) Sie Ihre eigene APP!

# <a name="macos"></a>[macOS](#tab/macos)

1. [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/)herunterladen und installieren.
1. [Melden](https://docs.microsoft.com/visualstudio/mac/activation) Sie sich an, um Ihr Unternehmens Abonnement zu aktivieren.
1. Über [prüfen](~/tools/inspector/inspect.md) Sie Ihre eigene APP!

-----

## <a name="requirements"></a>Requirements (Anforderungen)

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10,11 oder höher
- **Windows** -Windows 7 oder höher (mit Internet Explorer 11 oder höher und .NET 4.6.1 oder höher)

### <a name="supported-ides"></a>Unterstützte IDEs

- Visual Studio für Mac
- Visual Studio 2017 mit **Mobile-Entwicklung mit .net** -Arbeitsauslastung

Die Überprüfung von Live-Apps steht für Enterprise-Kunden zur Verfügung.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Unterstützte Anwendungsplattformen

|App-Plattform|IDE-Unterstützung|Notizen|
|--- |--- |--- |
|Mac|Wird nur in Visual Studio für Mac unterstützt.|
|iOS|Unterstützt in Visual Studio 2017 und Visual Studio für Mac| Das Linker-Verhalten muss auf " **nicht verknüpfen** " festgelegt werden (unter **IOS** -buildprojektoptionen) |
|Android|Unterstützt in Visual Studio 2017 und Visual Studio für Mac|Muss auf Android > = 4.0.3 mit aktiviertem **fastdev** abzielen.<br />Google-, Visual Studio- oder Xamarin Android-Emulatoren müssen verwendet werden. In Android 7-Emulatoren sind Überprüfungen derzeit womöglich nicht zulässig.|
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
- Der Inhalt des **Ausgabe** Bereichs von Visual Studio kann auch informativ sein.

### <a name="project-settings"></a>„Project Settings“ (Projekteinstellungen)

Wenn Sie die **csproj** -Datei für das Projekt anfügen können, das Sie überprüfen möchten, wäre es äußerst hilfreich. Dies ist einfacher, als Ihnen Fragen zu individuellen Einstellungen zu stellen.

Bestätigen Sie außerdem, dass die Debugkonfiguration ausgewählt ist.

### <a name="selected-devices"></a>Ausgewählte Geräte

Für Android und iOS ist es wichtig, dass wir wissen, welches Gerät Sie zum Debuggen ausgewählt haben: Wir müssen Folgendes wissen:

- Name des in der IDE dargestellten Geräts
- Betriebssystemversion Ihres Geräts
- Android: Stellen Sie sicher, dass Sie den x86-Emulator verwenden.
- Android: welche Emulatorplattform verwenden Sie? Google-Emulatoren? Visual Studio-Emulator für Android? Xamarin Android Player?
- Funktioniert die App, die Sie debuggen wollen, ordnungsgemäßauf dem Gerät?
- Hat das Gerät Netzwerk Konnektivität (überprüfen Sie den Webbrowser)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
