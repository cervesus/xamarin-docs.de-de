---
title: 'Xamarin.Essentials: Farbkonverter'
description: Die Klasse „ColorConverters“ in Xamarin.Essentials stellt mehrere Hilfs- und Erweiterungsmethoden für System.Drawing.Color bereit.
ms.assetid: B10428D6-89E2-4714-A39F-7E6E626391B2
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 5f26edf9515be79660574de0ae621daab3d1ea7d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756913"
---
# <a name="xamarinessentials-color-converters"></a>Xamarin.Essentials: Farbkonverter

Die Klasse **ColorConverters** in Xamarin.Essentials stellt mehrere Hilfsmethoden für System.Drawing.Color bereit.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-color-converters"></a>Verwenden von Farbkonvertern

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Wenn Sie `System.Drawing.Color` verwenden, können Sie die integrierten Konverter von Xamarin.Forms nutzen, um eine Farbe aus HSL, Hex oder UInt zu erstellen.

```csharp
var blueHex = ColorConverters.FromHex("#3498db");
var blueHsl = ColorConverters.FromHsl(204, 70, 53);
var blueUInt = ColorConverers.FromUInt(3447003);
```

## <a name="using-color-extensions"></a>Verwenden von Farberweiterungen

Mit den Erweiterungsmethoden für `System.Drawing.Color` können Sie verschiedene Eigenschaften anwenden:

```csharp
var blue = ColorConverters.FromHex("#3498db");

// Multiplies the current alpha by 50%
var blueWithAlpha = blue.MultiplyAlpha(.5f);
```

Es gibt verschiedene andere Erweiterungsmethoden, einschließlich:

- ToUInt
- MultiplyAlpha
- WithHue
- WithAlpha
- WithSaturation
- WithLuminosity

## <a name="using-platform-extensions"></a>Verwenden von Plattformerweiterungen

Außerdem können Sie System.Drawing.Color in die plattformspezifische Farbstruktur konvertieren. Diese Methoden können nur über die iOS-, Android- und UWP-Projekte aufgerufen werden.

```csharp
var system = System.Drawing.Color.FromArgb(255, 52, 152, 219);

// Extension to convert to Android.Graphics.Color, UIKit.UIColor, or Windows.UI.Color
var platform = system.ToPlatformColor();
```

```csharp
var platform = new Android.Graphics.Color(52, 152, 219, 255);

// Back to System.Drawing.Color
var system = platform.ToSystemColor();
```

Die Methode `ToSystemColor` ist für Android.Graphics.Color, UIKit.UIColor und Windows.UI.Color zulässig.

## <a name="api"></a>API

- [Color Converters source code (Quellcode für den Farbkonverter)](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Color Converters API documentation (Dokumentation der Farbkonverter-API)](xref:Xamarin.Essentials.ColorConverters)
- [Color Extensions source code (Quellcode für die Farberweiterung)](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Color Extensions API documentation (Dokumentation der Farberweiterungs-API)](xref:Xamarin.Essentials.ColorExtensions)
