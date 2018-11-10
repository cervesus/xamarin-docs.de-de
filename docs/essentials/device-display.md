---
title: 'Xamarin.Essentials: Geräteanzeigeinformationen'
description: In diesem Dokument wird die Klasse „DeviceDisplay“ in Xamarin.Essentials beschrieben, die Bildschirmmetriken für das Gerät bereitstellt, auf dem die Anwendung ausgeführt wird.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ebe97cf7fbb78bff17196110e835bd35ff76b826
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674885"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Geräteanzeigeinformationen

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **DeviceDisplay** stellt Informationen zu Bildschirmmetriken des Geräts bereit, auf dem die Anwendung ausgeführt wird.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>Verwenden von Geräteanzeigeinformationen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Bildschirmmetriken

Neben den grundlegenden Geräteinformationen enthält die Klasse **DeviceDisplay** Informationen zum Bildschirm des Geräts und dessen Ausrichtung.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

Die Klasse **DeviceDisplay** macht auch ein Ereignis verfügbar, das abonniert werden kann und ausgelöst wird, wenn sich eine Bildschirmmetrik ändert:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine Unterschiede.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Der Zugriff auf `ScreenMetrics` muss im UI-Thread erfolgen. Sonst wird eine Ausnahme ausgelöst. Sie können die Methode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) verwenden, um diesen Code im UI-Thread auszuführen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Unterschiede.

--------------


## <a name="api"></a>API

- [DeviceDisplay-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay-API-Dokumentation](xref:Xamarin.Essentials.DeviceDisplay)
