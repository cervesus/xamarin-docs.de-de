---
title: 'Xamarin.Essentials: Anzeige-Geräteinformationen'
description: Dieses Dokument beschreibt die Klasse DeviceDisplay in Xamarin.Essentials, auf dem Bildschirm Metriken für das Gerät enthält, auf dem die Anwendung ausgeführt wird.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 830b96bcc21397047cb5aaacb5c568bc2ee863c4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782371"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Anzeige-Geräteinformationen

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **DeviceDisplay** -Klasse stellt Informationen über das Gerät Bildschirm Metriken, die die Anwendung ausgeführt wird.

## <a name="using-devicedisplay"></a>Verwenden von DeviceDisplay

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Bildschirm Metriken

Zusätzlich zu den grundlegenden Geräteinformationen der **DeviceDisplay** -Klasse enthält Informationen über den Bildschirm und die Ausrichtung des Geräts.

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

Die **DeviceDisplay** Klasse auch verfügbar macht und jedes öffentliche Ereignis, die abonniert werden kann, die ein Ereignis aus, wenn eine metrische Änderungen Bildschirm auslöst:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API-Dokumentation](xref:Xamarin.Essentials.DeviceDisplay)
