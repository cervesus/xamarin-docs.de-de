---
title: Grundlagen zu Pfaden in SkiaSharp
description: In diesem Artikel untersucht das SkiaSharp SKPath-Objekt, für das Kombinieren von miteinander verbundenen Linien und Kurven und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 370fec5b9323187f6345d3e6bf9d3e38145cedff
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652780"
---
# <a name="path-basics-in-skiasharp"></a>Grundlagen zu Pfaden in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Untersuchen Sie das SkiaSharp SKPath-Objekt, für das Kombinieren von miteinander verbundenen Linien und Kurven_

Eines der wichtigsten Features des Pfads Grafiken ist die Möglichkeit zum definieren, wenn mehrere Zeilen verbunden werden sollen, und wenn sie nicht verbunden werden soll. Der Unterschied kann erheblich ist, sein, wie in den oberen Bereich dieser zwei Dreiecken veranschaulicht:

![](paths-images/connectedlinesexample.png "Zwei Dreiecken, die mit den Unterschieden zwischen verbundenen und nicht verbundenen Zeilen")

Ein Grafikpfad gekapselt ist, durch die [ `SKPath` ](xref:SkiaSharp.SKPath) Objekt. Ein Pfad ist eine Auflistung einer oder mehreren *Konturen*. Jede Contour Global ist eine Sammlung von *verbunden* geraden Linien und Kurven. Konturen nicht miteinander verbunden sind, jedoch können sie visuell die sich überlappen. Manchmal kann eine einzelne Kontur selbst überlappen.

Eine Kontur beginnt gewöhnlich mit einem Aufruf an die folgende Methode der `SKPath`:

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*) Um eine neue Kontur zu beginnen.

Das Argument an diese Methode ist ein einzelner Punkt, mit denen Sie, entweder als Ausdrücken können eine `SKPoint` Wert oder als separate X- und Y-Koordinaten. Die `MoveTo` Aufruf wird einen Punkt, an den Anfang der Kontur und die anfängliche *aktuellen Punkt*. Sie können die folgenden Methoden zum Fortfahren der Kontur mit einer Zeile oder die Kurve, die vom aktuellen Punkt an einem Punkt in der Methode, die dann den neuen aktuellen Punkt wird angegeben, aufrufen:

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*) der Pfad einer geraden Linie hinzu
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) hinzuzufügende einen Bogen, der eine Zeile in der Umfang eines Kreises oder einer ellipse
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*) Hinzufügen ein kubische Bezier-Splines
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*) hinzufügen einen quadratischen Bezier-spline
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*) ein rationales Quadratisches Bezier-Spline hinzufügen, das genau Conic Abschnitte (Ellipsen, Parabeln und Hyperbeln) gerendert werden kann

Keine dieser fünf Methoden enthalten alle Informationen, die erforderlich sind, die Linien- oder Kurvensegmente beschreiben. Jede dieser fünf Methoden funktioniert in Verbindung mit dem aktuellen Punkt hergestellt, indem der Aufruf der Methode unmittelbar vorausgeht. Z. B. die `LineTo` Methode fügt eine gerade Linie mit der Kontur basierend auf dem aktuellen Zeitpunkt, also den Parameter, um `LineTo` ist auf ein bestimmten Punkt.

Die `SKPath` Klasse definiert auch Methoden, die den gleichen wie diese sechs Methoden jedoch Namen eine `R` am Anfang:

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

Die `R` steht für *relative*. Diese Methoden verfügen über die gleiche Syntax wie die entsprechenden Methoden ohne das `R` aber relativ zum aktuellen Zeitpunkt sind. Diese Tools sind praktisch, für das Zeichnen von ähnlicher Teile eines Pfads in einer Methode, die mehrere Male aufgerufen werden.

Eine Kontur endet mit einem weiteren Aufruf von `MoveTo` oder `RMoveTo`, beginnt eine neue Kontur oder einen Aufruf von `Close`, die die Kontur schließt. Die `Close` Methode automatisch eine gerade Linie von der aktuellen Punkt und dem ersten Punkt der Kontur angefügt und markiert den Pfad als geschlossen, was bedeutet, dass es ohne jede Strichenden gerendert wird.

Der Unterschied zwischen offenen und geschlossenen Konturen wird veranschaulicht, der **zwei Dreieck Konturen** Seite verwendet ein `SKPath` Objekt mit zwei Konturen auf zwei Dreiecken zu rendern. Die erste Kontur öffnen und die zweite geschlossen ist. Hier ist die [ `TwoTriangleContoursPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) Klasse:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Die erste Contour Global besteht aus einem Aufruf von [ `MoveTo` ](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) mit X- und Y-Koordinaten anstelle einer `SKPoint` Werts, gefolgt von drei Aufrufe [ `LineTo` ](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) zum Zeichnen von der drei Seiten des der Dreieck. Die zweite Kontur hat nur zwei Aufrufe `LineTo` , aber er abgeschlossen ist, die Kontur mit einem Aufruf von [ `Close` ](xref:SkiaSharp.SKPath.Close), die schließt der Kontur. Der Unterschied ist signifikant:

[![](paths-images/twotrianglecontours-small.png "Dreifacher Screenshot der Seite zwei Dreieck Konturen")](paths-images/twotrianglecontours-large.png#lightbox "dreifachen Screenshot der Seite zwei Dreieck Konturen")

Wie Sie sehen können, die erste Kontur ist natürlich eine Reihe von drei miteinander verbundenen Linien, aber nicht das Ende mit dem Anfang verbunden. Die beiden Zeilen überlappen sich am Anfang. Die zweite Kontur wird offensichtlich geschlossen, und wurde erreicht, mit einem weniger `LineTo` aufgerufen werden, da die `Close` Methode fügt automatisch eine letzte Zeile aus, um die Kontur zu schließen.

`SKCanvas` definiert nur eine [ `DrawPath` ](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) -Methode, die in dieser Demonstration zweimal aufgerufen wird, füllen und den Pfad zu zeichnen. Alle Konturen werden gefüllt, auch solche, die nicht geschlossen werden. Für die Zwecke offene Pfade zu füllen wird angenommen, dass eine gerade Linie zwischen den Start- und Endpunkte von der Pfadkonturen vorhanden sind. Wenn Sie die letzte entfernen `LineTo` aus der ersten Contour Global, oder Entfernen der `Close` Aufruf aus der zweiten Kontur, jede Contour Global hat nur zwei Seiten jedoch ausgefüllt werden, als handele es sich um ein Dreieck.

`SKPath` definiert viele andere Methoden und Eigenschaften. Die folgenden Methoden fügen die gesamte Konturen auf den Pfad, der geschlossen oder abhängig von der Methode nicht geschlossen werden kann:

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) der Umfang einer Ellipse eine Kurve hinzufügen
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*) Um einen anderen Pfad auf den aktuellen Pfad hinzuzufügen.
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath)) Um einen anderen Pfad in umgekehrter Reihenfolge hinzuzufügen.

Beachten Sie, dass ein `SKPath` Objekt definiert nur eine Geometrie &mdash; eine Reihe von Punkten und Verbindungen. Nur, wenn ein `SKPath` kombiniert mit einer `SKPaint` Objekt ist der Pfad, der mit einer bestimmten Farbe an, die Strichbreite usw. gerendert. Außerdem Beachten Sie, dass die `SKPaint` , übergeben die `DrawPath` Methode definiert die Merkmale der den vollständigen Pfad ein. Wenn Sie etwas, erfordern einige Farben zeichnen möchten, müssen Sie einen separaten Pfad für jede Farbe verwenden.

Genau wie die Darstellung des Start- und Ende einer Zeile durch eine Obergrenze des Strichs definiert ist, wird die Darstellung der Verbindung zwischen zwei Zeilen definiert, indem eine *Stroke Join*. Geben Sie dies durch Festlegen der [ `StrokeJoin` ](xref:SkiaSharp.SKPaint.StrokeJoin) Eigenschaft `SKPaint` auf einen Member der [ `SKStrokeJoin` ](xref:SkiaSharp.SKStrokeJoin) Enumeration:

- `Miter` für einen ein join
- `Round` für einen gerundeten join
- `Bevel` für einen Join Systemschicht-deaktiviert

Die **Stroke Joins** Seite zeigt drei Joins mit Code wie zeichnen den **Strichenden** Seite. Dies ist die `PaintSurface` -Ereignishandler in der [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) Klasse:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Hier wird das Programm ausgeführt wird:

[![](paths-images/strokejoins-small.png "Dreifacher Screenshot der Seite verknüpft Stroke")](paths-images/strokejoins-large.png#lightbox "dreifachen Screenshot der Seite Stroke verknüpft")

Die gehrungsverbindung besteht aus einem sharp Punkt, der die Zeilen, in dem eine Verbindung herstellen. Wenn zwei Zeilen in einem kleinen Winkel verbinden, kann die Gehrungsgrenze recht lang sind. Um übermäßig lange gehrungsverbindungen zu verhindern, wird die Länge der Gehrungsgrenze durch den Wert des beschränkt die [ `StrokeMiter` ](xref:SkiaSharp.SKPaint.StrokeMiter) Eigenschaft `SKPaint`. Eine gehrungsverbindung, die diese Länge überschreiten ist deaktiviert aufgeteilt, um eine abgeschrägte Verbindung werden.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
