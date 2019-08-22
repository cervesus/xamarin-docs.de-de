---
title: 'Xamarin.Essentials: Plattformerweiterungen'
description: Xamarin.Essentials umfasst mehrere Plattformerweiterungsmethoden für die Arbeit mit Plattformtypen wie „Rect“, „Size“ und „Point“.
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 6092c4449e58655b0d08b87d0b5ad76f89abc7a8
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620662"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials: Plattformerweiterungen

Xamarin.Essentials umfasst mehrere Plattformerweiterungsmethoden für die Arbeit mit Plattformtypen wie „Rect“, „Size“ und „Point“. Das bedeutet, dass Sie die `System`-Version dieser Typen in die iOS-, Android- und UWP-spezifischen Typen konvertieren können. 

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>Verwenden von Plattformerweiterungen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Alle Plattformerweiterungen können nur über das iOS-, Android- oder UWP-Projekt aufgerufen werden.

### <a name="point"></a>Punkt

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Size
var system2 = platform.ToSystemPoint();
```

### <a name="size"></a>Größe

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>Rechteck

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Size
var system2 = platform.ToSystemRectangle();
```

## <a name="api"></a>API

- [Converters source code (Quellcode für den Konverter)](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [Point Converters API documentation (Dokumentation der Punktkonverter-API)](xref:Xamarin.Essentials.PointExtensions)
- [Rectangle Converters API documentation (Dokumentation der Rechteckkonverter-API)](xref:Xamarin.Essentials.RectangleExtensions)
- [Size Converters API documentation (Dokumentation der Größenkonverter-API)](xref:Xamarin.Essentials.SizeExtensions)
