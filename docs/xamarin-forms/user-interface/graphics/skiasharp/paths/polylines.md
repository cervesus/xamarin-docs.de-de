---
title: Polylinien und parametrische Formeln
description: In diesem Artikel wird erläutert, wie zum Verwenden von SkiaSharp zum Rendern einer Zeile, mit parametrische Formeln definieren können, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: e7327deead917f55d1e7ac8af5302b6dccf6fead
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61290803"
---
# <a name="polylines-and-parametric-equations"></a>Polylinien und parametrische Formeln

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Verwenden von SkiaSharp zum Rendern jeder Zeile, die Sie, mit parametrische Formeln definieren können_

In der [ **SkiaSharp-Kurven und-Pfade** ](../curves/index.md) Abschnitt dieses Handbuchs, sehen Sie die verschiedenen Methoden, die [ `SKPath` ](xref:SkiaSharp.SKPath) definiert werden, um bestimmte Arten von Kurven zu rendern. Allerdings ist es manchmal notwendig, einen Typ der Kurve zu zeichnen, die direkt von nicht unterstützt wird `SKPath`. In diesem Fall können Sie jede Kurve zu zeichnen, die Sie, mathematisch definieren können eine Polylinie (eine Sammlung von miteinander verbundenen Linien) verwenden. Wenn Sie die Zeilen klein und zahlreiche reicht das Ergebnis wie eine Kurve sieht. Diese Spirale ist tatsächlich 3.600 wenig Zeilen:

![](polylines-images/spiralexample.png "Eine Spirale")

Im Allgemeinen empfiehlt es sich um eine Kurve in Bezug auf ein Paar von parametrische Formeln zu definieren. Hierbei handelt es sich um Gleichungen, für die X- und Y, die koordiniert eine dritte Variable bezeichnet hängen `t` Zeit. Die folgenden parametrischen Formeln definieren z. B. einen Kreis mit einem Radius von 1, zentriert am Punkt (0, 0) für *t* von 0 bis 1:

`x = cos(2πt)`

`y = sin(2πt)`

 Wenn Sie einen Radius verwenden möchten, die größer als 1, können Sie einfach den Sinus und Cosinus Werte Multiplizieren mit, Radius und wenn Sie die Mitte an einen anderen Speicherort verschieben möchten, fügen diese Werte:

`x = xCenter + radius·cos(2πt)`

`y = yCenter + radius·sin(2πt)`

Für eine Ellipse mit der horizontalen und vertikalen Achsen Parallel sind zwei Radien beteiligt:

`x = xCenter + xRadius·cos(2πt)`

`y = yCenter + yRadius·sin(2πt)`

Sie können dann den entsprechenden SkiaSharp-Code in einer Schleife einfügen, die die verschiedenen Punkte berechnet, und fügt diese in einen Pfad. Der folgende SkiaSharp-Code erstellt ein `SKPath` -Objekt für eine Ellipse, die die Anzeigeoberfläche ausfüllt. Die Schleife durchläuft die 360 Grad direkt aus. Das Center ist die Hälfte der Breite und Höhe der Anzeigeoberfläche und deshalb die zwei Radien:

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

Dies führt zu einer Ellipse, die durch 360 kleine Linien definiert. Wenn er gerendert wurde, wird es smooth angezeigt.

Natürlich, Sie müssen nicht erstellen Sie eine Ellipse mit einem Polyline-Objekt, da `SKPath` enthält ein `AddOval` -Methode, die es für Sie übernimmt. Jedoch empfiehlt es sich um ein visuelles Objekt zu zeichnen, die nicht zur Verfügung `SKPath`.

Die **Archimedean Spirale** verfügt über Code, ähnlich wie der Ellipse-Code, aber mit einem bedeutsamen Unterschied. Es führt eine Schleife der 360 Grad des Kreises 10-Mal fortlaufend Anpassen des Radius:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Das Ergebnis ist die Abkürzung ein *arithmetische Spirale* , da die Abweichung zwischen dem foreach-Schleife konstant ist:

[![](polylines-images/archimedeanspiral-small.png "Dreifacher Screenshot der Seite Archimedean Spirale")](polylines-images/archimedeanspiral-large.png#lightbox "dreifachen Screenshot der Seite Archimedean Spirale")

Beachten Sie, dass die `SKPath` wird erstellt, einem `using` Block. Dies `SKPath` belegt mehr Arbeitsspeicher als die `SKPath` Objekte in der vorherigen Programme, die deutet auf eine `using` Block ist besser geeignet ist, nicht verwalteten Ressourcen freizugeben.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
