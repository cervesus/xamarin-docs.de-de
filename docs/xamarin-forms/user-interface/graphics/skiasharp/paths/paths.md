---
title: Pfad Grundlagen in skiasharp
description: In diesem Artikel wird das skiasharp-skpath-Objekt zum kombinieren verbundener Linien und Kurven erläutert und mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ceac2d866e67af5cf3496fcf8c072ae83ecfe38
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140243"
---
# <a name="path-basics-in-skiasharp"></a>Pfad Grundlagen in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Untersuchen Sie das skiasharp skpath-Objekt zum kombinieren verbundener Linien und Kurven._

Eines der wichtigsten Features des Grafik Pfades ist die Möglichkeit, zu definieren, wann mehrere Zeilen verbunden werden sollen und wann keine Verbindung hergestellt werden sollte. Der Unterschied kann signifikant sein, da die Spitze dieser beiden Dreiecke Folgendes veranschaulichen:

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

Ein Grafik Pfad wird vom-Objekt gekapselt [`SKPath`](xref:SkiaSharp.SKPath) . Ein Pfad ist eine Auflistung von mindestens einer *Kontur*. Jede Kontur ist eine Auflistung *verbundener* gerader Linien und Kurven. Konturen sind nicht miteinander verbunden, können sich jedoch visuell überlappen. Manchmal kann sich eine einzelne Kontur selbst überlappen.

Eine Kontur beginnt in der Regel mit einem Rückruf der folgenden Methode von `SKPath` :

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*)So beginnen Sie mit einer neuen Kontur

Das Argument für diese Methode ist ein einzelner Punkt, den Sie entweder als- `SKPoint` Wert oder als separate X-und Y-Koordinaten Ausdrücken können. Der `MoveTo` -Befehl richtet einen Punkt am Anfang der Kontur und einen anfänglichen *aktuellen Punkt*ein. Sie können die folgenden Methoden aufzurufen, um die Kontur mit einer Linie oder Kurve vom aktuellen Punkt bis zu einem in der-Methode angegebenen Punkt fortzusetzen, der dann zum neuen aktuellen Punkt wird:

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*)So fügen Sie dem Pfad eine gerade Linie hinzu
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*)zum Hinzufügen eines Bogens, bei dem es sich um eine Linie im Umfang eines Kreises oder einer Ellipse handelt.
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*)So fügen Sie eine kubische Bezier-Spline hinzu
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*)So fügen Sie eine quadratische Bezier-Spline hinzu
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*)So fügen Sie einen rationalen quadratischen Bezier-Spline hinzu, der zusammen zulasene Abschnitte (Ellipsen, parabolas und hyperbolas) präzise darstellen kann

Keine dieser fünf Methoden enthält alle Informationen, die erforderlich sind, um die Linie oder die Kurve zu beschreiben. Jede dieser fünf Methoden funktioniert in Verbindung mit dem aktuellen Punkt, der durch den oben genannten Methoden Befehl festgelegt wird. Beispielsweise fügt die- `LineTo` Methode der-Kontur eine gerade Linie basierend auf dem aktuellen Punkt hinzu, sodass der-Parameter zu `LineTo` nur ein einziger Punkt ist.

Die- `SKPath` Klasse definiert auch Methoden, die die gleichen Namen wie diese sechs Methoden haben, jedoch `R` am Anfang:

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

Die `R` steht für *relative*. Diese Methoden haben dieselbe Syntax wie die entsprechenden Methoden ohne `R` , sind aber relativ zum aktuellen Punkt. Diese sind praktisch zum Zeichnen ähnlicher Teile eines Pfades in einer Methode, die Sie mehrmals aufzurufen.

Eine Kontur endet mit einem anderen-oder-aufzurufen, `MoveTo` `RMoveTo` der eine neue Kontur beginnt, oder einem-Befehl `Close` , der die Kontur schließt. Die `Close` Methode fügt automatisch eine gerade Linie vom aktuellen Punkt an den ersten Punkt der Kontur an und markiert den Pfad als geschlossen, was bedeutet, dass Sie ohne Strich Striche gerendert wird.

Der Unterschied zwischen offenen und geschlossenen Kontur wird auf der Seite **zwei Dreiecks Konturen** veranschaulicht, die ein `SKPath` Objekt mit zwei Konturen zum Rengen zweier Dreiecke verwendet. Die erste Kontur ist geöffnet, und die zweite ist geschlossen. Hier ist die- [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs) Klasse:

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

Die erste Kontur besteht aus einem Aufruf zur [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) Verwendung von X-und Y-Koordinaten anstelle eines- `SKPoint` Werts, gefolgt von drei Aufrufen von, [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) um die drei Seiten des Dreiecks zu zeichnen. Die zweite Kontur hat nur zwei Aufrufe von `LineTo` , aber Sie schließt die Kontur mit einem Aufruf von ab [`Close`](xref:SkiaSharp.SKPath.Close) , der die Kontur schließt. Der Unterschied ist signifikant:

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

Wie Sie sehen können, ist die erste Kontur offensichtlich eine Reihe von drei verbundenen Linien, aber das Ende stellt keine Verbindung mit dem Anfang her. Die beiden Zeilen überlappen sich am oberen Rand. Die zweite Kontur ist offensichtlich geschlossen und wurde mit einem geringeren Aufruf erreicht, `LineTo` da die `Close` Methode automatisch eine letzte Zeile hinzufügt, um die Kontur zu schließen.

`SKCanvas`definiert nur eine [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) Methode, die in dieser Demonstration zweimal aufgerufen wird, um den Pfad auszufüllen und zu zeichnen. Alle Konturen werden ausgefüllt, auch diejenigen, die nicht geschlossen sind. Um nicht geschlossene Pfade zu füllen, wird davon ausgegangen, dass zwischen dem Start-und dem Endpunkt der Kontur eine gerade Linie vorhanden ist. Wenn Sie den letzten `LineTo` aus der ersten Kontur entfernen oder den-Befehl `Close` aus der zweiten Kontur entfernen, verfügt jede Kontur nur über zwei Seiten, wird jedoch so aufgefüllt, als ob es sich um ein Dreieck handelt.

`SKPath`definiert viele andere Methoden und Eigenschaften. Die folgenden Methoden fügen dem Pfad ganze Kontur hinzu, die je nach Methode geschlossen oder nicht geschlossen werden können:

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))So fügen Sie eine Kurve für den Umfang einer Ellipse hinzu
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*)So fügen Sie einen weiteren Pfad zum aktuellen Pfad hinzu
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath))So fügen Sie einen weiteren Pfad hinzu

Beachten Sie, dass ein- `SKPath` Objekt nur eine Geometrie &mdash; einer Reihe von Punkten und Verbindungen definiert. Nur wenn eine `SKPath` mit einem Objekt kombiniert wird `SKPaint` , wird der Pfad mit einer bestimmten Farbe, einer Strichbreite usw. gerendert. Beachten Sie außerdem, dass das- `SKPaint` Objekt, das an die-Methode weitergegeben wird, `DrawPath` Merkmale des gesamten Pfads definiert. Wenn Sie etwas zeichnen möchten, das mehrere Farben erfordert, müssen Sie für jede Farbe einen separaten Pfad verwenden.

Ebenso wie die Darstellung des Starts und des Endes einer Linie durch eine Strich Abdeckung definiert wird, wird die Darstellung der Verbindung zwischen zwei Zeilen durch einen *Strich Beitritt*definiert. Dies können Sie angeben, indem Sie die- [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin) Eigenschaft von `SKPaint` auf einen Member der- [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) Enumeration festlegen:

- `Miter`für einen kommt Join
- `Round`für einen abgerundeten Join
- `Bevel`für einen abgeschnittenen Join

Auf der Seite **Strich Joins** werden diese drei Strich Joins mit dem Code angezeigt, der der Seite **Strich Caps** ähnelt. Dies ist der `PaintSurface` Ereignishandler in der- [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) Klasse:

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

Dies ist das Programm, das ausgeführt wird:

[![](paths-images/strokejoins-small.png "Triple screenshot of the Stroke Joins page")](paths-images/strokejoins-large.png#lightbox "Triple screenshot of the Stroke Joins page")

Der Gehrungs Join besteht aus einem scharfen Punkt, an dem die Linien eine Verbindung herstellen. Wenn zwei Zeilen in einem kleinen Winkel verknüpft werden, kann der Gehrungs Join recht lang werden. Um übermäßig lange Gehrungs-Joins zu vermeiden, wird die Länge des Gehrungs-Joins durch den Wert der- [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) Eigenschaft von beschränkt `SKPaint` . Ein Gehrungs Join, der diese Länge überschreitet, wird abgeschnitten, um zu einer Abschrägung zu werden.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
