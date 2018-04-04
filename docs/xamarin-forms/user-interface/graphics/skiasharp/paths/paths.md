---
title: Pfad-Grundlagen
description: Untersuchen Sie das SkiaSharp SKPath-Objekt für das Kombinieren von miteinander verbundenen Linien und Kurven
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 3a828baccda83822237d2564d771bcd89c9099e5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="path-basics"></a>Pfad-Grundlagen

_Untersuchen Sie das SkiaSharp SKPath-Objekt für das Kombinieren von miteinander verbundenen Linien und Kurven_

Eines der wichtigsten Features von der Grafikpfad ist die Möglichkeit zum definieren, wenn mehrere Zeilen verbunden werden sollen, und wenn sie nicht verbunden werden soll. Der Unterschied kann ziemlich sichtbar sein, wie in den oberen Bereich dieser beiden Dreiecke veranschaulicht:

![](paths-images/connectedlinesexample.png "Zwei Dreiecke mit den Unterschieden zwischen verbundenen und nicht verbundenen Linien")

Ein Grafikpfad gekapselt, durch die [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Objekt. Ein Pfad ist eine Auflistung einer oder mehrerer *Konturen*. Jede Kontur ist eine Sammlung von *verbunden* gerade Linien und Kurven. Konturen nicht miteinander verbunden sind, aber visuell möglicherweise überlappen. In einigen Fällen kann eine einzelne Kontur sich überlappen.

Eine Kontur beginnt in der Regel durch einen Aufruf an die folgende Methode der `SKPath`:

- `MoveTo` Um eine neue Kontur zu beginnen.

Das Argument an diese Methode stellt einen einzelnen Fehlerpunkt, die Sie entweder als express können eine `SKPoint` Wert oder als separate X- und Y-Koordinaten. Die `MoveTo` Aufruf richtet einen Punkt am Anfang der Kontur und eine anfängliche *aktuellen Punkt*. Sie können die folgenden Methoden zum Fortfahren der Kontur mit einer Linie oder eines Kurve an der aktuellen Stelle zu einem Zeitpunkt in der Methode, die Sie dann den neuen aktuellen Punkt wird angegeben, aufrufen:

- `LineTo` der Pfad einer geraden Linie hinzu
- `ArcTo` hinzuzufügende einen Bogen, der eine Zeile auf den Umfang eines Kreises oder einer ellipse
- `CubicTo` So fügen Sie eine kubische Bézier-Spline hinzu
- `QuadTo` So fügen Sie eine quadratische Bézier-Spline hinzu
- `ConicTo` ein vernünftige Quadratisches Bézier-Spline hinzufügen, das präzise Conic Abschnitte (Ellipsen, Parabeln und Hyperbeln) gerendert werden können

Keines dieser fünf Methoden enthalten alle Informationen, die erforderlich sind, um die Befehlszeile oder die Kurve zu beschreiben. Jede dieser fünf Methoden funktioniert in Verbindung mit dem aktuellen Punkt durch Aufruf der Methode, die unmittelbar vorhergehenden hergestellt. Z. B. die `LineTo` Methode fügt eine gerade Linie die Kontur basierend auf den aktuellen Stand, also den Parameter für `LineTo` stellt nur einen einzelnen Fehlerpunkt.

Die `SKPath` Klasse definiert auch Methoden, die denselben Namen wie diese sechs Methoden jedoch mit einer `R` am Anfang:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

Die `R` steht für *relative*. Sie haben die gleiche Syntax wie die entsprechenden Methoden ohne das `R` jedoch relativ zum aktuellen Zeitpunkt sind. Dies sind praktisch, zeichnen ähnliche Teile eines Pfadsegments in einer Methode, die mehrere Male aufgerufen werden.

Kontur endet mit einem weiteren Aufruf von `MoveTo` oder `RMoveTo`, dem beginnt eine neue Kontur oder einen Aufruf von `Close`, die die Kontur schließt. Die `Close` Methode automatisch Fügt eine gerade Linie von den aktuellen Zeitpunkt mit dem ersten Punkt der Kontur und kennzeichnet den Pfad als "geschlossen", was bedeutet, dass es ohne alle Stroke-Caps gerendert wird.

Der Unterschied zwischen offenen und geschlossenen Konturen wird veranschaulicht, der **zwei Dreieck Konturen** Seite verwendet ein `SKPath` Objekt mit zwei Konturen zwei Dreiecke gerendert. Die erste Kontur geöffnet ist, und die zweite ist geschlossen. So sieht die [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) Klasse:

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

Die erste Kontur besteht aus einem Aufruf von [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) mit X- und Y-Koordinaten anstelle einer `SKPoint` Werts, gefolgt von drei Aufrufe [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) zum Zeichnen von der drei Seiten des der Dreieck. Die zweite Kontur hat nur zwei Aufrufe `LineTo` jedoch am Ende der Kontur mit einem Aufruf von [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), die die Kontur schließt. Der Unterschied ist signifikant:

[![](paths-images/twotrianglecontours-small.png "Dreifacher Screenshot der Seite zwei Dreieck Konturen")](paths-images/twotrianglecontours-large.png#lightbox "dreifacher Screenshot der Seite zwei Dreieck Konturen")

Wie Sie sehen können, die erste Kontur ist offensichtlich eine Reihe von drei miteinander verbundenen Linien, aber am Ende wird keine Verbindung mit dem Anfang hergestellt. Die beiden Zeilen werden am oberen überlappen. Die zweite Kontur offensichtlich geschlossen wird und mit einem weniger musste `LineTo` aufruft, da die `Close` Methode fügt automatisch eine letzte Zeile aus, um die Kontur zu schließen.

`SKCanvas` definiert nur eine [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) -Methode, die in dieser Demo zweimal aufgerufen wird, zum Füllen und den Pfad zu zeichnen. Alle Konturen werden gefüllt, selbst solche, die nicht geschlossen sind. Rahmen, ausfüllen Pfade nicht geschlossen wird eine gerade Linie zwischen den Start- und Endpunkte der Konturen vorhanden angenommen. Wenn Sie den letzten entfernen `LineTo` aus dem ersten Kontur, oder Entfernen der `Close` Aufruf aus der zweiten Kontur, jede Kontur hat nur zwei Seiten wird jedoch aufgefüllt werden, als handele es sich um ein Dreieck.

`SKPath` definiert andere Methoden und Eigenschaften. Die folgenden Methoden fügen gesamte Konturen auf den Pfad, der geschlossen oder je nach der Methode nicht geschlossen werden kann:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) der Umfang einer Ellipse, die bei einer Kurve hinzu
- `AddPath` So fügen Sie einen anderen Pfad auf den aktuellen Pfad hinzu
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) So fügen Sie einen anderen Pfad in umgekehrter Reihenfolge hinzu

Beachten Sie, dass ein `SKPath` Objekt definiert, nur eine Geometrie &mdash; eine Reihe von Punkten und Verbindungen. Nur, wenn ein `SKPath` mit kombiniert eine `SKPaint` Objekt ist der Pfad, der mit einer bestimmten Farbe, die Strichbreite usw. gerendert. Außerdem Beachten Sie, dass die `SKPaint` -Objekt übergeben, um die `DrawPath` Methode Merkmale des gesamten Pfads definiert. Wenn Sie etwas, erfordern verschiedene Farben zeichnen möchten, müssen Sie einen separaten Pfad für jede Farbe verwenden.

Ebenso wie die Darstellung der Start- und Ende einer Zeile durch eine Obergrenze Strich definiert ist, wird die Darstellung der Verbindung zwischen zwei Zeilen definiert, indem eine *Strich Join*. Geben Sie dies durch Festlegen der [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) Eigenschaft `SKPaint` an ein Mitglied der [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) Enumeration:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) für einen Join kommt
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) für einen abgerundeten join
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) für einen Join gehackt deaktivieren

Die **Strich Joins** zeigt drei Joins mit Code wie Schraffieren den **Stroke-Caps** Seite. Dies ist die `PaintSurface` -Ereignishandler in der [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) Klasse:

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

Hier wird das Programm auf drei Plattformen ausgeführt wird:

[![](paths-images/strokejoins-small.png "Dreifacher Screenshot der Seite verknüpft Strich")](paths-images/strokejoins-large.png#lightbox "dreifacher Screenshot der Seite Strich verknüpft")

Spitz Joins besteht aus einem spitzen Punkt mit den Zeilen, in denen eine Verbindung herstellen. Spitz Joins kann recht lang sind, nach dem Eintritt in zwei Zeilen in einem kleinen Winkel. Übermäßig lange gehrungsverbindungen verhindern möchten, ist die Länge des Joins Spitz durch den Wert des beschränkt die [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) Eigenschaft `SKPaint`. Ein Spitz-Join, der überschreitet diese Länge ist deaktiviert einen Join der Abschrägung sind gehackt.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
