---
title: Xamarin Live Player-App
description: In diesem Dokument wird die Xamarin Live Player-App beschrieben, die verwendet werden kann, um eine Vorschau der Codeänderungen auf dem Gerät anzuzeigen. Es werden Setup, Beispiele, Protokolle, Einstellungen, Verwaltung von Geräten und mehr erläutert.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: b09ea4e8ec09db5e9bf443476cd3a67e16f02d19
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651223"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player-App

![Feature der Vorschauversion](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin Live Player Vorschau wurde beendet. Die APP ist nicht mehr verfügbar. Die folgenden Anweisungen werden für Kunden bereitgestellt, die die Vorschauversion mit Visual Studio 2017 weiterhin verwenden.

> [!TIP]
> Sie können den [XAML-Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md) in Visual Studio 2019 oder Visual Studio für Mac verwenden, um die Bildschirm Entwürfe während der Bearbeitung anzuzeigen.

Beim Start sieht die Xamarin Live Player-App wie folgt aus:

![Bildschirm Abbildung von Live Player Android-App](player-images/app-android-sml.png)

Wenn Sie **mit Visual Studio**auf "Pair" klicken, wird der auf dem Computer angezeigte Barcode mit der Kamera gescannt:

![Screenshot des Android-Barcode Scanners](player-images/scan-android-sml.png)

Wenn die Verbindung erfolgreich ist, sollte der Code auf dem Gerät fast sofort ausgeführt werden (z. b. im [Beispiel Rechner](https://github.com/xamarin/mobile-samples/tree/master/LivePlayer/BasicCalculator):

![Beispiel Rechner-APP wird auf dem Gerät ausgeführt](player-images/basic-calculator-sml.png)

## <a name="options"></a>Optionen

Klicken Sie unten in der APP auf die Schaltfläche "Informationen" **(i)** , um das Menü **Optionen** anzuzeigen:

[![Screenshot des Menüs "Optionen"](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Protokolle

Anzeigen von Protokollen zur Diagnose von Problemen.

### <a name="settings"></a>Einstellungen

- Anzeige von Kompilierungs-und Laufzeitfehlern umschalten.
- Versionsinformationen.
- Senden Sie Feedback.

[![Screenshot der Einstellungen](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Verwalten von Geräten

Befolgen Sie die Anweisungen unter [Anforderungen & Setup](~/tools/live-player/install.md), um ein Gerät zum ersten Mal zu verbinden. Sie können mehrere Geräte koppeln und über die IDE verwalten.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

Wählen Sie in Visual Studio Extras **> Xamarin Live Player > Geräte verwalten..** .

![Fenster "Geräte verwalten"](player-images/manage-tools-menu-vs.png)

In diesem Fenster können Sie folgende Aufgaben ausführen:

- Koppeln eines neuen Geräts durch Scannen des Codes
- Alternativ können Sie ein Gerät koppeln, indem Sie den im Bildschirm angezeigten Code eingeben.
- Vorhandene Geräte aus der Liste entfernen

Sie können auch in der Geräteliste auf dieses Fenster zugreifen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wählen Sie in Visual Studio für Mac **Tools > (Xamarin Live Player) Geräte verwalten...**

![Fenster "Geräte verwalten"](player-images/manage-tools-menu.png)

In diesem Fenster können Sie folgende Aufgaben ausführen:

- Koppeln eines neuen Geräts durch Scannen des Codes
- Alternativ können Sie ein Gerät koppeln, indem Sie den im Bildschirm angezeigten Code eingeben.
- Vorhandene Geräte aus der Liste entfernen

![Fenster "Geräte verwalten"](player-images/manage.png)

Sie können dieses Fenster auch über die Geräteliste aufrufen:

![Wählen Sie Xamarin Live Player Geräte aus der Geräteliste aus.](player-images/manage-device-menu.png)

-----

Wenn Probleme auftreten, finden Sie unter [Einschränkungen und Problem](~/tools/live-player/troubleshooting.md)Behandlung Weitere Informationen.

## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)

