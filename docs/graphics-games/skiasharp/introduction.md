---
title: Beispiele für plattformunabhängige skiasharp-Beispiele
description: Dieses Dokument enthält eine kurze Einführung in die grundlegenden skiasharp-Konzepte. Insbesondere werden das Abrufen und zeichnen auf einem skcanvas erläutert.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2018
ms.openlocfilehash: 4d0e57b98a479112b9fdf4f9c503418f3966cc73
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "64749930"
---
# <a name="skiasharp-platform-independent-examples"></a>Beispiele für plattformunabhängige skiasharp-Beispiele

_Dies stellt eine kurze plattformunabhängige Einführung in die Konzepte hinter skiasharp bereit._

Skiasharp stellt eine umfangreiche und leistungsstarke 2D-Grafik-API bereit, die zum Rendervorgang in 2D-Puffer verwendet werden kann.  Sie können diese verwenden, um benutzerdefinierte Benutzeroberflächen Elemente und 2D-Grafiken zu implementieren, die in Ihre Anwendung integriert werden können. Skiasharp ist eine .net-Bindung an die [Skia](https://skia.org) -Bibliothek und erbt die Features und Leistungsfähigkeit dieser Bibliothek.

Die Bibliothek ist derzeit als plattformübergreifendes [nuget-Paket](https://www.nuget.org/packages/SkiaSharp)verfügbar. Sie können Sie dem Projekt hinzufügen, indem Sie den nuget-Verweis hinzufügen.

Zum Zeichnen erstellt der Code einen `SkCanvas` , der die Oberfläche beschreibt, in der die Zeichnungsvorgänge stattfinden.

## <a name="obtaining-an-skcanvas"></a>Abrufen einer skcanvas

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>Zeichnen in skcanvas

Der `SKCanvas` verwendet ein Zeichen Modell, das in der gleichen Form wie andere Zeichnungsmodelle verwendet wird, mit denen Sie möglicherweise vertraut sind. es verwendet Farben mit einem optionalen Transparenz Kanal und kann Linien, Arcs, Text und Bilder zeichnen.

Im folgenden finden Sie nur einige der vielen unterschiedlichen Dinge, die mit skiasharp möglich sind.  In den folgenden Beispielen ist die `canvas` Variable vom Typ skcanvas.

### <a name="drawing-xamagon"></a>Zeichnen von xamagon

In diesem Beispiel wird das xamarin-Logo von xamseck gezeichnet:

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>Zeichnen von Text

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>Zeichnen von Bitmaps

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>Zeichnen mit Bild Filtern

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zur Verwendung von skiasharp finden Sie in der [API-Dokumentation](https://docs.microsoft.com/dotnet/api/skiasharp) .
