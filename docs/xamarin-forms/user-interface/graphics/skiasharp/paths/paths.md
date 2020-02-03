---
title: Grundlagen zu Pfaden in SkiaSharp
description: In diesem Artikel untersucht das SkiaSharp SKPath-Objekt, für das Kombinieren von miteinander verbundenen Linien und Kurven und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: c892adf2f75ec00c4a9ee171ded78f79bb8227e9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725194"
---
# <a name="path-basics-in-skiasharp"></a>Grundlagen zu Pfaden in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Untersuchen Sie das skiasharp skpath-Objekt zum kombinieren verbundener Linien und Kurven._

Eines der wichtigsten Features des Pfads Grafiken ist die Möglichkeit zum definieren, wenn mehrere Zeilen verbunden werden sollen, und wenn sie nicht verbunden werden soll. Der Unterschied kann erheblich ist, sein, wie in den oberen Bereich dieser zwei Dreiecken veranschaulicht:

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

Ein Grafik Pfad wird vom [`SKPath`](xref:SkiaSharp.SKPath) Objekt gekapselt. Ein Pfad ist eine Auflistung von mindestens einer *Kontur*. Jede Kontur ist eine Auflistung *verbundener* gerader Linien und Kurven. Konturen nicht miteinander verbunden sind, jedoch können sie visuell die sich überlappen. Manchmal kann eine einzelne Kontur selbst überlappen.

Eine Kontur beginnt in der Regel mit einem Rückruf der folgenden Methode von `SKPath`:

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*) , um eine neue Kontur zu beginnen.

Das Argument für diese Methode ist ein einzelner Punkt, den Sie entweder als `SKPoint` Wert oder als separate X-und Y-Koordinaten Ausdrücken können. Der `MoveTo`-Befehl richtet einen Punkt am Anfang der Kontur und einen anfänglichen *aktuellen Punkt*ein. Sie können die folgenden Methoden zum Fortfahren der Kontur mit einer Zeile oder die Kurve, die vom aktuellen Punkt an einem Punkt in der Methode, die dann den neuen aktuellen Punkt wird angegeben, aufrufen:

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*) , eine gerade Linie zum Pfad hinzuzufügen.
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) das Hinzufügen eines Bogens, einer Linie im Umfang eines Kreises oder einer Ellipse.
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*) , um eine kubische Bezier-Spline hinzuzufügen.
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*) , um eine quadratische Bezier-Spline hinzuzufügen.
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*) , einen rationalen quadratischen Bezier-Spline hinzuzufügen, der zusammen zulasene Abschnitte (Ellipsen, parabolas und hyperbolas) präzise darstellen kann.

Keine dieser fünf Methoden enthalten alle Informationen, die erforderlich sind, die Linien- oder Kurvensegmente beschreiben. Jede dieser fünf Methoden funktioniert in Verbindung mit dem aktuellen Punkt hergestellt, indem der Aufruf der Methode unmittelbar vorausgeht. Beispielsweise fügt die `LineTo`-Methode der Kontur eine gerade Linie basierend auf dem aktuellen Punkt hinzu, sodass der Parameter für `LineTo` nur ein einziger Punkt ist.

Die `SKPath`-Klasse definiert auch Methoden mit denselben Namen wie diese sechs Methoden, aber mit einer `R` am Anfang:

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

Der `R` steht für *relative*. Diese Methoden haben dieselbe Syntax wie die entsprechenden Methoden ohne die `R`, sind aber relativ zum aktuellen Punkt. Diese Tools sind praktisch, für das Zeichnen von ähnlicher Teile eines Pfads in einer Methode, die mehrere Male aufgerufen werden.

Eine Kontur endet mit einem weiteren `MoveTo`-oder `RMoveTo`, der eine neue Kontur beginnt, oder einem `Close`aufzurufenden, der die Kontur schließt. Die `Close`-Methode fügt automatisch eine gerade Linie vom aktuellen Punkt an den ersten Punkt der Kontur an und markiert den Pfad als geschlossen, was bedeutet, dass Sie ohne Strich Striche gerendert wird.

Der Unterschied zwischen offenen und geschlossenen Kontur wird auf der Seite **zwei Dreiecks Konturen** veranschaulicht, die ein `SKPath` Objekt mit zwei Konturen zum Renderern zweier Dreiecke verwendet. Die erste Kontur öffnen und die zweite geschlossen ist. Im folgenden finden Sie die [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs) -Klasse:

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

Die erste Kontur besteht aus einem Aufruf von [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) mit X-und Y-Koordinaten anstelle eines `SKPoint` Werts, gefolgt von drei Aufrufen von [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) , um die drei Seiten des Dreiecks zu zeichnen. Die zweite Kontur hat nur zwei Aufrufe von `LineTo` aber die Kontur wird mit einem Aufruf von [`Close`](xref:SkiaSharp.SKPath.Close)abgeschlossen, der die Kontur schließt. Der Unterschied ist signifikant:

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

Wie Sie sehen können, die erste Kontur ist natürlich eine Reihe von drei miteinander verbundenen Linien, aber nicht das Ende mit dem Anfang verbunden. Die beiden Zeilen überlappen sich am Anfang. Die zweite Kontur ist offensichtlich geschlossen und wurde mit einem geringeren `LineTo` aufrufen erreicht, da die `Close`-Methode automatisch eine letzte Zeile hinzufügt, um die Kontur zu schließen.

`SKCanvas` definiert nur eine [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) Methode, die in dieser Demonstration zweimal aufgerufen wird, um den Pfad auszufüllen und zu zeichnen. Alle Konturen werden gefüllt, auch solche, die nicht geschlossen werden. Für die Zwecke offene Pfade zu füllen wird angenommen, dass eine gerade Linie zwischen den Start- und Endpunkte von der Pfadkonturen vorhanden sind. Wenn Sie die letzte `LineTo` aus der ersten Kontur entfernen oder den `Close`-Befehl aus der zweiten Kontur entfernen, verfügt jede Kontur nur über zwei Seiten, wird jedoch so aufgefüllt, als ob es sich um ein Dreieck handelt.

`SKPath` definiert viele andere Methoden und Eigenschaften. Die folgenden Methoden fügen die gesamte Konturen auf den Pfad, der geschlossen oder abhängig von der Methode nicht geschlossen werden kann:

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) , um eine Kurve für den Umfang einer Ellipse hinzuzufügen.
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*) , einen weiteren Pfad zum aktuellen Pfad hinzuzufügen.
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath)) , einen weiteren Pfad in umgekehrter Reihenfolge hinzuzufügen

Beachten Sie, dass ein `SKPath`-Objekt nur eine Geometrie &mdash; eine Reihe von Punkten und Verbindungen definiert. Nur wenn ein `SKPath` mit einem `SKPaint` Objekt kombiniert wird, wird der Pfad mit einer bestimmten Farbe, einer Strichbreite usw. gerendert. Beachten Sie außerdem, dass das `SKPaint` Objekt, das an die `DrawPath`-Methode weitergegeben wird, Merkmale des gesamten Pfads definiert. Wenn Sie etwas, erfordern einige Farben zeichnen möchten, müssen Sie einen separaten Pfad für jede Farbe verwenden.

Ebenso wie die Darstellung des Starts und des Endes einer Linie durch eine Strich Abdeckung definiert wird, wird die Darstellung der Verbindung zwischen zwei Zeilen durch einen *Strich Beitritt*definiert. Dies können Sie angeben, indem Sie die [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin) -Eigenschaft `SKPaint` auf einen Member der [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) -Enumeration festlegen:

- `Miter` für einen kommt Join
- `Round` für einen abgerundeten Join
- `Bevel` für einen abgeschnittenen Join

Auf der Seite **Strich Joins** werden diese drei Strich Joins mit dem Code angezeigt, der der Seite **Strich Caps** ähnelt. Dies ist der `PaintSurface` Ereignishandler in der [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) -Klasse:

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

[![](paths-images/strokejoins-small.png "Triple screenshot of the Stroke Joins page")](paths-images/strokejoins-large.png#lightbox "Triple screenshot of the Stroke Joins page")

Die gehrungsverbindung besteht aus einem sharp Punkt, der die Zeilen, in dem eine Verbindung herstellen. Wenn zwei Zeilen in einem kleinen Winkel verbinden, kann die Gehrungsgrenze recht lang sind. Um übermäßig lange Gehrungs-Joins zu vermeiden, wird die Länge des Gehrungs-Joins durch den Wert der [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) -Eigenschaft `SKPaint`beschränkt. Eine gehrungsverbindung, die diese Länge überschreiten ist deaktiviert aufgeteilt, um eine abgeschrägte Verbindung werden.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
