---
title: Xamarin Live Player-App
description: Dieses Dokument beschreibt den Xamarin Live Player-app, die verwendet werden kann, für die Vorschau von codeänderungen auf Gerät live. Es wird erläutert, Setup, Beispiele, Protokolle, Einstellungen, die Verwaltung von Geräten und mehr.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: fce0eeae4ef5776842ea1b45c36163118042dc49
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157736"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player-App

![Feature der Vorschauversion](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin Live Player-Vorschauversion wurde beendet. Die app ist nicht mehr verfügbar. Die folgenden Anweisungen dienen für Kunden, die Sie weiterhin die Vorschau mit Visual Studio 2017 verwenden.

> [!TIP]
> Sie können die [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer/index.md) in 2019 für Visual Studio oder Visual Studio für Mac Ihre Entwürfe Bildschirm anzuzeigen, wie Sie sie bearbeiten.

Klicken Sie auf Start sieht die Xamarin Live Player-app folgendermaßen aus:

![Live Player-Android-app-screenshot](player-images/app-android-sml.png)

Beim Drücken **mit Visual Studio koppeln**, verwenden Sie die Kamera, überprüfen Sie den Barcode auf Ihrem Computer angezeigt:

![Screenshot des Android Barcodescanner](player-images/scan-android-sml.png)

Wenn die Verbindung erfolgreich ist, der Code sollte auf dem Gerät ausgeführt fast sofort (z. B. die [-Rechnerbeispiel](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Beispiel-rechneranwendung, die auf dem Gerät ausgeführt wird](player-images/basic-calculator-sml.png)

## <a name="options"></a>Optionen

Drücken Sie die Schaltfläche "Information" **(i)** am unteren Rand der app zum Anzeigen der **Optionen** Menü:

[![Screenshot, der das Menü "Optionen"](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Protokolle

Anzeigen von Protokollen, um Probleme zu diagnostizieren.

### <a name="settings"></a>Einstellungen

- Schaltet die Anzeige der kompilierungs-und Laufzeitfehler.
- Versionsinformationen.
- Senden Sie Feedback.

[![Screenshot der Einstellungen](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Verwalten von Geräten

Um ein Gerät zum ersten Mal zu verbinden, folgen Sie den Anweisungen im [Datenbankanforderungen & Setup](~/tools/live-player/install.md). Sie können mehrere Geräte gekoppelt und über die IDE verwalten.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

Wählen Sie in Visual Studio **Tools > Xamarin Live Player > Geräte verwalten...**

![Verwalten der Gerätefenster](player-images/manage-tools-menu-vs.png)

In diesem Fenster können Sie die folgenden Schritte ausführen:

- Ein neues Gerät gekoppelt werden durch den Code Scannen
- Koppeln Sie können auch den Code auf dem Bildschirm angezeigten eines Geräts
- Entfernen Sie vorhandene Geräte aus der Liste

Sie können dieses Fenster auch aus der Liste der Geräte zugreifen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wählen Sie in Visual Studio für Mac **Tools > Verwalten von Geräten (Xamarin Live Player)...**

![Verwalten der Gerätefenster](player-images/manage-tools-menu.png)

In diesem Fenster können Sie die folgenden Schritte ausführen:

- Ein neues Gerät gekoppelt werden durch den Code Scannen
- Koppeln Sie können auch den Code auf dem Bildschirm angezeigten eines Geräts
- Entfernen Sie vorhandene Geräte aus der Liste

![Verwalten der Gerätefenster](player-images/manage.png)

Sie können dieses Fenster auch aus der Geräteliste zugreifen:

![Wählen Sie die Xamarin Live Player-Geräte aus der Geräteliste](player-images/manage-device-menu.png)

-----

Wenn Sie feststellen, dass alle Probleme finden Sie unter [Einschränkungen und Problembehandlung](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)

