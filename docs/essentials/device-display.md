---
title: 'Xamarin.Essentials: Geräteanzeigeinformationen'
description: In diesem Dokument wird die Klasse „DeviceDisplay“ in Xamarin.Essentials beschrieben, die Bildschirmmetriken für das Gerät bereitstellt, auf dem die Anwendung ausgeführt wird.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d3102f0a4ed5f16c77c4a4768feb4a1565f2dd1a
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898890"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Geräteanzeigeinformationen

Die **DeviceDisplay**-Klasse enthält Informationen über die Bildschirmmetriken des Geräts, auf dem die Anwendung ausgeführt wird, und kann anfordern, dass der Bildschirm daran gehindert wird, in den Standbymodus zu schalten.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>Verwenden von Geräteanzeigeinformationen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="main-display-info"></a>Informationen zum Hauptbildschirm

Neben den grundlegenden Geräteinformationen enthält die Klasse **DeviceDisplay** Informationen zum Bildschirm des Geräts und dessen Ausrichtung.

```csharp
// Get Metrics
var mainDisplayInfo = DeviceDisplay.MainDisplayInfo;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = mainDisplayInfo.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = mainDisplayInfo.Rotation;

// Width (in pixels)
var width = mainDisplayInfo.Width;

// Height (in pixels)
var height = mainDisplayInfo.Height;

// Screen density
var density = mainDisplayInfo.Density;
```

Die Klasse **DeviceDisplay** macht auch ein Ereignis verfügbar, das abonniert werden kann und ausgelöst wird, wenn sich eine Bildschirmmetrik ändert:

```csharp
public class DisplayInfoTest
{
    public DisplayInfoTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.MainDisplayInfoChanged += OnMainDisplayInfoChanged;
    }

    void OnMainDisplayInfoChanged(DisplayInfoChangedEventArgs  e)
    {
        // Process changes
        var displayInfo = e.DisplayInfo;
    }
}
```

Die **DeviceDisplay**-Klasse macht eine `bool`-Eigenschaft verfügbar, die `KeepScreenOn` heißt, und festgelegt werden kann, um den Gerätebildschirm daran zu hindern, sich auszuschalten oder zu sperren.

```csharp
public class KeepScreenOnTest
{
    public void ToggleScreenLock()
    {
        DeviceDisplay.KeepScreenOn = !DeviceDisplay.KeepScreenOn;
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine Unterschiede.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Der Zugriff auf `DeviceDisplay` muss im UI-Thread erfolgen. Sonst wird eine Ausnahme ausgelöst. Sie können die Methode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) verwenden, um diesen Code im UI-Thread auszuführen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Unterschiede.

--------------


## <a name="api"></a>API

- [DeviceDisplay-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay-API-Dokumentation](xref:Xamarin.Essentials.DeviceDisplay)
