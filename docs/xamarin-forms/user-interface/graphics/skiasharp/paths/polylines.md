---
title: Polylinien und parametrische Formeln
description: In diesem Artikel wird erläutert, wie Sie skiasharp zum Rendering von Zeilen verwenden, die mit parametrischen Gleichungen definiert werden können, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b435e99180791b64e0a8ad975527fb3cb5316b7d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140217"
---
# <a name="polylines-and-parametric-equations"></a>Polylinien und parametrische Formeln

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden Sie skiasharp zum Rendering beliebiger Zeilen, die mit parametrischen Gleichungen definiert werden können._

Im Abschnitt [**skiasharp-Kurven und-Pfade**](../curves/index.md) dieses Handbuchs sehen Sie die verschiedenen Methoden, die [`SKPath`](xref:SkiaSharp.SKPath) definieren, um bestimmte Arten von Kurven zu erzeugen. Es ist jedoch manchmal notwendig, eine Art von Kurve zu zeichnen, die nicht direkt von unterstützt wird `SKPath` . In einem solchen Fall können Sie eine Polylinie (eine Auflistung verbundener Linien) verwenden, um eine beliebige Kurve zu zeichnen, die Sie mathematisch definieren können. Wenn Sie die Zeilen klein genug und zahlreich genug machen, sieht das Ergebnis wie eine Kurve aus. Diese Spirale ist eigentlich 3.600 kleine Zeilen:

![](polylines-images/spiralexample.png "A spiral")

Im Allgemeinen ist es am besten, eine Kurve in Bezug auf ein paar parametrischer Gleichungen zu definieren. Dabei handelt es sich um Gleichungen für X-und Y-Koordinaten, die von einer dritten Variablen abhängig sind `t` . Die folgenden parametrischen Gleichungen definieren z. b. einen Kreis mit einem Radius von 1, der am Punkt (0, 0) zentriert ist, für *t* zwischen 0 und 1:

`x = cos(2πt)`

`y = sin(2πt)`

 Wenn Sie einen RADIUS benötigen, der größer als 1 ist, können Sie einfach den Sinus-und Kosinus-Wert mit diesem RADIUS multiplizieren. Wenn Sie den Mittelpunkt an einen anderen Speicherort verschieben möchten, fügen Sie die folgenden Werte hinzu:

`x = xCenter + radius·cos(2πt)`

`y = yCenter + radius·sin(2πt)`

Für eine Ellipse, die die Achsen parallel zur horizontalen und vertikalen Achse hat, sind zwei Radien beteiligt:

`x = xCenter + xRadius·cos(2πt)`

`y = yCenter + yRadius·sin(2πt)`

Anschließend können Sie den entsprechenden skiasharp-Code in einer Schleife platzieren, die die verschiedenen Punkte berechnet und diese einem Pfad hinzufügt. Der folgende skiasharp-Code erstellt ein- `SKPath` Objekt für eine Ellipse, die die Anzeige Oberfläche füllt. Die Schleife durchläuft die 360 Grad direkt. Der Mittelpunkt liegt in der Hälfte der Breite und Höhe der Anzeige Oberfläche, und die beiden Radii lauten wie folgt:

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

Dies führt zu einer Ellipse, die durch 360 kleine Zeilen definiert ist. Wenn es gerendert wird, wird es glatt angezeigt.

Natürlich müssen Sie keine Ellipse mithilfe einer Polylinie erstellen, da `SKPath` eine `AddOval` Methode enthält, die dies für Sie erledigt. Möglicherweise möchten Sie jedoch ein visuelles Objekt zeichnen, das nicht von bereitgestellt wird `SKPath` .

Die **archimedische Spiral** Seite verfügt über Code, der mit dem Ellipse-Code vergleichbar ist, aber mit einem entscheidenden Unterschied. Dabei werden die 360 Grad des Kreises 10 mal durchlaufen, und der RADIUS wird fortlaufend angepasst:

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

Das Ergebnis wird auch als *arithmetische Spirale* bezeichnet, da der Offset zwischen den einzelnen Schleifen konstant ist:

[![](polylines-images/archimedeanspiral-small.png "Triple screenshot of the Archimedean Spiral page")](polylines-images/archimedeanspiral-large.png#lightbox "Triple screenshot of the Archimedean Spiral page")

Beachten Sie, dass `SKPath` in einem- `using` Block erstellt wird. Dies beansprucht `SKPath` mehr Arbeitsspeicher als die `SKPath` Objekte in den vorherigen Programmen. Dies deutet darauf hin, dass ein `using` Block besser geeignet ist, um alle nicht verwalteten Ressourcen freizugeben.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
