---
title: Installation und Anforderungen des Inspektors
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Inspector installieren und die unterstützten Betriebssysteme, IDES und App-Plattformen erörtert werden.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 19c4a15fb2490c7bace4798b0cb8e062b1379a04
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029696"
---
# <a name="inspector-installation-and-requirements"></a>Installation und Anforderungen des Inspektors

## <a name="download-and-installation"></a>Herunterladen und installieren

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Laden Sie [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) herunter, und installieren Sie die **Mobile-Entwicklung mit .net** -Arbeitsauslastung.
1. [Melden](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) Sie sich an, um Ihr Unternehmens Abonnement zu aktivieren.
1. Über [prüfen](~/tools/inspector/inspect.md) Sie Ihre eigene APP!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/)herunterladen und installieren.
1. [Melden](https://docs.microsoft.com/visualstudio/mac/activation) Sie sich an, um Ihr Unternehmens Abonnement zu aktivieren.
1. Über [prüfen](~/tools/inspector/inspect.md) Sie Ihre eigene APP!

-----

## <a name="requirements"></a>Voraussetzungen

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **Mac** -OS X 10,11 oder höher
- **Windows** -Windows 7 oder höher (mit Internet Explorer 11 oder höher und .NET 4.6.1 oder höher)

### <a name="supported-ides"></a>Unterstützte IDEs

- Visual Studio für Mac
- Visual Studio 2017 mit **Mobile-Entwicklung mit .net** -Arbeitsauslastung

Die Live-App-Prüfung ist für Unternehmenskunden verfügbar.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Unterstützte Anwendungsplattformen

|App-Plattform|IDE-Unterstützung|Hinweise|
|--- |--- |--- |
|Mac|Wird nur in Visual Studio für Mac unterstützt.|
|iOS|In Visual Studio 2017 und Visual Studio für Mac unterstützt| Das Linker-Verhalten muss auf " **nicht verknüpfen** " festgelegt werden (unter **IOS** -buildprojektoptionen) |
|Android|In Visual Studio 2017 und Visual Studio für Mac unterstützt|Muss auf Android > = 4.0.3 mit aktiviertem **fastdev** abzielen.<br />Muss Google-, Visual Studio-oder xamarin Android-Emulatoren verwenden. Android 7-Emulatoren können zu diesem Zeitpunkt keine Überprüfung zulassen.|
|WPF|Wird nur in Visual Studio 2017 unterstützt.|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Melden von Fehlern

Fehler sollten direkt über Visual Studio gemeldet werden:

- **Hilfe > Senden von Feedback > Melden eines Problems**

Fügen Sie alle folgenden Informationen ein:

### <a name="platform-version-information"></a>Informationen zur Platt Form Version

Diese Informationen sind von entscheidender Bedeutung.

Visual Studio für Mac

- **Visual Studio-> zu Visual Studio > Details > Kopier Informationen anzeigen**
- In Fehlerbericht einfügen

Visual Studio

- **Hilfe > zu Visual Studio > Kopieren von Informationen**
- Informieren Sie uns über die Betriebs System Version und darüber, ob Sie 32-Bit-oder 64-Bit-Windows ausführen.

### <a name="log-files"></a>Protokolldateien

Fügen Sie immer IDE-und Inspektor-Client Protokolldateien an.

Inspektor-Client

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4. x bietet auch die Möglichkeit, die Protokolldatei im Finder (macOS) oder Explorer (Windows) direkt über das Hauptmenü auszuwählen:

- **Hilfe > Anzeigen der Protokolldatei**

Visual Studio für Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Der Inhalt des **Ausgabe** Bereichs von Visual Studio kann auch informativ sein.

### <a name="project-settings"></a>Projekteinstellungen

Wenn Sie die **csproj** -Datei für das Projekt anfügen können, das Sie überprüfen möchten, wäre es äußerst hilfreich. Dies ist einfacher als die Frage nach einzelnen Einstellungen.

Vergewissern Sie sich außerdem, dass Sie sich in der Debugkonfiguration befinden.

### <a name="selected-devices"></a>Ausgewählte Geräte

Für Android und IOS ist es wichtig, dass Sie wissen, auf welchem Gerät Sie Debuggen, wenn Sie es überprüfen möchten. Wir müssen Folgendes wissen:

- Der Name des Geräts, wie in der IDE angezeigt.
- Betriebssystemversion Ihres Geräts
- Android: Überprüfen Sie, ob Sie einen x86-Emulator verwenden
- Android: Welche Emulatorplattform verwenden Sie? Google-Emulator? Visual Studio-Android-Emulator? Xamarin Android Player?
- Wird die APP, die Sie Debuggen, ordnungsgemäß angezeigt und funktioniert im Gerät?
- Hat das Gerät Netzwerk Konnektivität (überprüfen Sie den Webbrowser)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
