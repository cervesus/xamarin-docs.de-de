---
title: Polylinien und parametrische Formeln
description: Verwenden Sie SkiaSharp, um eine beliebige Zeile zu rendern, die können Sie mit parametrische Formeln definieren
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: efd2dbac0f4a1190fac646d8e9e3120ee4d245a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="polylines-and-parametric-equations"></a>Polylinien und parametrische Formeln

_Verwenden Sie SkiaSharp, um eine beliebige Zeile zu rendern, die können Sie mit parametrische Formeln definieren_

In einem späteren Abschnitt dieses Handbuchs, sehen Sie die verschiedenen Methoden, die `SKPath` definiert werden, um bestimmte Arten von Kurven zu rendern. Allerdings ist es manchmal notwendig, einen Typ der Kurve zu zeichnen, die nicht direkt vom unterstützt `SKPath`. In einem solchen Fall können Sie jede Kurve gezeichnet werden soll, die Sie, mathematisch definieren können Polylinie (eine Sammlung von miteinander verbundenen Linien) verwenden. Wenn Sie die Zeilen klein genug und zahlreiche reicht das Ergebnis wie folgt eine Kurve sieht. Diese Spirale ist tatsächlich 3.600 wenig Zeilen:

![](polylines-images/spiralexample.png "Eine Spirale")

Im Allgemeinen ist es am besten, eine Kurve in Bezug auf ein Paar von Gleichungen parametrische definieren. Dies sind Formeln für X und Y, die Koordinaten eine dritte Variable bezeichnet hängen `t` Zeit. Die folgenden parametrischen Formeln definieren z. B. einen Kreis mit dem Radius 1 zentriert, an dem Punkt (0, 0) für *t* von 0 bis 1:

 X = y cos(2πt) = sin(2πt)

 Wenn Sie einen Radius verwenden möchten, die größer als 1, können Sie einfach die Sinus- und Kosinuswert Werte Multiplizieren mit diesem Radius und Sie ggf. die Mitte an einen anderen Speicherort verschieben, fügen Sie diese Werte hinzu:

 X = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Für eine Ellipse mit der horizontalen und vertikalen Achsen Parallel sind zwei Radien beteiligt:

X = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Sie können dann den entsprechenden SkiaSharp Code einfügen, die in einer Schleife, die die verschiedenen Punkte berechnet, und fügt diese an einen Pfad. Der folgende SkiaSharp Code erstellt ein `SKPath` Objekt für eine Ellipse, die die Anzeigeoberfläche ausfüllt. Die Schleife durchläuft die 360 Grad direkt aus. Das Center wird die Hälfte der Breite und Höhe der Anzeigeoberfläche und deshalb zwei Radien:

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

Dies führt zu einer Ellipse, die durch 360 wenig Zeilen definiert. Wenn er gerendert wurde, wird diese smooth angezeigt.

Natürlich, Sie erstellen eine Ellipse, die eine Polylinie verwenden, da müssen `SKPath` enthält eine `AddOval` Methode, die es für Sie erledigt. Allerdings sollten Sie ein visuelles Objekt gezeichnet werden soll, die nicht von `SKPath`.

Die **Archimedean Spirale** verfügt über Code, ähnlich dem Ellipse Code jedoch einen wichtigen Unterschied. Es führt eine Schleife 360 Grad des Kreises 10-Mal fortlaufend Radius anpassen:

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

Das Ergebnis wird auch bezeichnet ein *arithmetische Spirale* , da das Offset zwischen jeder Schleife konstant ist:

[![](polylines-images/archimedeanspiral-small.png "Dreifacher Screenshot der Seite Archimedean Spirale")](polylines-images/archimedeanspiral-large.png#lightbox "dreifacher Screenshot der Seite Archimedean Spirale")

Beachten Sie, dass die `SKPath` wird erstellt, einem `using` Block. Dies `SKPath` benötigt mehr Arbeitsspeicher als die `SKPath` Objekte in den vorherigen Programmen, die anzeigt, die eine `using` Block ist besser geeignet ist, nicht verwalteten Ressourcen freizugeben.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
