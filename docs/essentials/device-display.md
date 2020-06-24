---
title: 'Xamarin.Essentials: Geräteanzeigeinformationen'
description: In diesem Dokument wird die DeviceDisplay-Klasse in Xamarin.Essentials beschrieben, die Bildschirmmetriken für das Gerät bereitstellt, auf dem die Anwendung ausgeführt wird.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 867e6bd1828d4158f70226dbaad678f9d6037bb0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802396"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Geräteanzeigeinformationen

Die **DeviceDisplay**-Klasse enthält Informationen über die Bildschirmmetriken des Geräts, auf dem die Anwendung ausgeführt wird, und kann anfordern, dass der Bildschirm daran gehindert wird, in den Standbymodus zu schalten.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>Verwenden von Geräteanzeigeinformationen

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

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

    void OnMainDisplayInfoChanged(object sender, DisplayInfoChangedEventArgs  e)
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

# <a name="android"></a>[Android](#tab/android)

Keine Unterschiede.

# <a name="ios"></a>[iOS](#tab/ios)

- Der Zugriff auf `DeviceDisplay` muss im UI-Thread erfolgen. Sonst wird eine Ausnahme ausgelöst. Sie können die Methode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) verwenden, um diesen Code im UI-Thread auszuführen.

# <a name="uwp"></a>[UWP](#tab/uwp)

Keine Unterschiede.

--------------

## <a name="api"></a>API

- [DeviceDisplay-Quellcode](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay-API-Dokumentation](xref:Xamarin.Essentials.DeviceDisplay)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Device-Display-Information-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
