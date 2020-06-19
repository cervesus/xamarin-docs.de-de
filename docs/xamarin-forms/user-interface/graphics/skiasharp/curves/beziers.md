---
title: Drei Typen von Bézier-Kurven
description: In diesem Artikel wird erläutert, wie Sie skiasharp zum Rendering von kubischen, quadratischen und in-Anwendungen in- Xamarin.Forms Anwendungen verwenden, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1ad548846500ccbacc2a3d117919bfb4df1a1d79
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138683"
---
# <a name="three-types-of-bzier-curves"></a>Drei Typen von Bézier-Kurven

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie skiasharp zum Rendering von kubischen, quadratischen und Konstanten Bézier-Kurven verwenden können._

Die Bézier-Kurve heißt nach Pierre Bézier (1910 – 1999), einem französischen Techniker bei der Automobilfirma Renault, der die Kurve für das computergestützte Design von Auto Körpern verwendet hat.

Die Bézier-Kurven sind bekanntermaßen für einen interaktiven Entwurf geeignet: Sie Verhalten sich &mdash; mit anderen Worten, es gibt keine Singularitäten, die bewirken, dass die Kurve unendlich oder unhandlich wird, &mdash; und Sie sind im allgemeinen ästhetisch ansprechend:

![Ein Beispiel für eine Bezier-Kurve](beziers-images/beziersample.png)

Zeichen umrissen von computerbasierten Schriftarten werden in der Regel mit Bézier-Kurven definiert.

Der Wikipedia-Artikel über die [**Bézier-Kurve**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) enthält einige nützliche Hintergrundinformationen. Der Begriff " *Bézier-Kurve* " bezieht sich tatsächlich auf eine Familie ähnlicher Kurven. Skiasharp unterstützt drei Arten von Bézier-Kurven, die als *kubisch*, das *quadratische*und das *konische*-Zeichen bezeichnet werden. Die Konstante wird auch als " *rational quadratischen*" bezeichnet.

## <a name="the-cubic-bzier-curve"></a>Die kubische Bézier-Kurve

Der kubische Typ ist der Typ der Bézier-Kurve, den die meisten Entwickler vorstellen, wenn der Betreff der Bézier-Kurven angezeigt wird.

Sie können einem Objekt eine kubische Bézier-Kurve hinzufügen `SKPath` , indem Sie die [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) -Methode mit drei `SKPoint` Parametern oder die-Überladung [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) mit separaten `x` Parametern und verwenden `y` :

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Die Kurve beginnt am aktuellen Punkt der Kontur. Die gesamte kubische Bezier-Kurve wird von vier Punkten definiert:

- Startpunkt: aktueller Punkt in der Kontur oder (0,0), wenn `MoveTo` nicht aufgerufen wurde.
- erster Kontrollpunkt: `point1` im- `CubicTo` Befehl
- zweiter Kontrollpunkt: `point2` im- `CubicTo` Befehl
- Endpunkt: `point3` im-Befehl `CubicTo`

Die resultierende Kurve beginnt am Ausgangspunkt und endet am Endpunkt. Die Kurve übergibt in der Regel nicht die beiden Kontrollpunkte. Stattdessen arbeiten die Steuerungs Punkte ähnlich wie die-Magnete, um die Kurve in die Kurve zu ziehen.

Die beste Methode, um sich einen Überblick über die kubische Bézier-Kurve zu verschaffen, ist das Experimentieren. Dies ist der Zweck der **Bezier-Kurven** Seite, die von abgeleitet wird `InteractivePage` . Die Datei [**BezierCurvePage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) instanziiert `SKCanvasView` und `TouchEffect` . Mit der [**BezierCurvePage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) -Code Behind-Datei werden vier- `TouchPoint` Objekte im Konstruktor erstellt. Der `PaintSurface` Ereignishandler erstellt eine `SKPath` , um eine Bézier-Kurve basierend auf den vier Objekten zu erzeugen `TouchPoint` , und zeichnet außerdem gepunktete Tangenten Linien von den Steuer Punkten bis zu den Endpunkten:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Hier wird ausgeführt:

[![Dreifacher Screenshot der Bezier-kurvenseite](beziers-images/beziercurve-small.png)](beziers-images/beziercurve-large.png#lightbox)

Mathematisch gesehen handelt es sich bei der Kurve um einen kubischen Polynomen. Die Kurve schneidet eine gerade Linie an drei Punkten höchstens ab. Am Startpunkt ist die Kurve immer tangengent zu und in der gleichen Richtung wie eine gerade Linie vom Startpunkt bis zum ersten Kontrollpunkt. Am Endpunkt ist die Kurve immer tangengent zu und in der gleichen Richtung wie eine gerade Linie vom zweiten Steuerungspunkt bis zum Endpunkt.

Die kubische Bézier-Kurve wird immer durch ein Konvexes Viereck gebunden, das die vier Punkte verbindet. Dies wird als " *konforme Hülle*" bezeichnet. Wenn sich die Steuerpunkte in der geraden Linie zwischen dem Anfangs-und dem Endpunkt befinden, wird die Bézier-Kurve als gerade Linie gerendert. Die Kurve kann sich aber auch selbst überqueren, wie der dritte Screenshot zeigt.

Eine Pfad Kontur kann mehrere verbundene kubische Bézier-Kurven enthalten, aber die Verbindung zwischen zwei kubischen Bézier-Kurven ist nur dann glatt, wenn die folgenden drei Punkte colinear sind (d. h., Sie befinden sich in einer geraden Linie):

- der zweite Kontrollpunkt der ersten Kurve.
- der Endpunkt der ersten Kurve, bei dem es sich auch um den Startpunkt der zweiten Kurve handelt.
- der erste Kontrollpunkt der zweiten Kurve

Im nächsten Artikel zu [**SVG-Pfaddaten**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)finden Sie eine Möglichkeit, um die Definition von glatt verbundenen Bézier-Kurven zu vereinfachen.

Es ist manchmal hilfreich, die zugrunde liegenden parametrischen Gleichungen zu kennen, die eine kubische Bézier-Kurve darstellen. Für *t* im Bereich von 0 bis 1 lauten die parametrischen Gleichungen wie folgt:

x (t) = (1 – t) ³ x ₀ + 3T (1 – t) ² x ₁ + 3T ² (1 – t) x, + t ³ x ₃

y (t) = (1 – t) ³ y ₀ + 3T (1 – t) ² y ₁ + 3T ² (1 – t) y, + t ³ y ₃

Der höchste Exponent von 3 bestätigt, dass es sich hierbei um kubische Polynomen handelt. Es ist leicht zu überprüfen, ob bei `t` 0 (null), der Punkt (x ₀, y ₀), der Startpunkt, und wenn `t` 1 ist, der Punkt (x ₃, y ₃), der Endpunkt ist. In der Nähe des Startpunkts (bei niedrigen Werten von `t` ) hat der erste Kontrollpunkt (x ₁, y ₁) einen starken Effekt, und in der Nähe des Endpunkts (hohe Werte von 't ') hat der zweite Kontrollpunkt (x und y-Wert) einen starken Effekt.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>Bezier-Kurven Näherung zu Zirkel Arcs

Es ist manchmal praktisch, eine Bézier-Kurve zu verwenden, um einen Kreisbogen zu erzeugen. Eine kubische Bézier-Kurve kann einen Zirkel Bogen sehr gut in einen Viertelkreis angleichen, sodass vier verbundene Bézier-Kurven einen ganzen Kreis definieren können. Diese Näherung wird in zwei Artikeln erläutert, die vor mehr als 25 Jahren veröffentlicht wurden:

> Tor Dokken, et al, "gute Näherung der Kreise durch Krümmung-kontinuierliche Bézier-Kurven", *Computer gestützter geometrischer Entwurf 7* (1990), 33-41.

> Michael goldapp, "Näherungs kreisförmige Arcs durch kubische Polynomen", *Computer gestützter geometrischer Entwurf 8* (1991), 227-238.

Das folgende Diagramm zeigt vier Punkte mit `pto` der Bezeichnung, `pt1` , `pt2` , und die `pt3` Definition einer Bézier-Kurve (rot dargestellt), die einem Kreisbogen entspricht:

![Näherung eines kreisförmigen Bogens mit einer Bézier-Kurve](beziers-images/bezierarc45.png)

Die Zeilen vom Start-und Endpunkt bis hin zu den Steuerungs Punkten sind tangengent zum Kreis und zur Bézier-Kurve und haben die Länge *L*. Der erste Artikel oben zeigt an, dass die Bézier-Kurve am besten einem Kreisbogen entspricht, wenn diese Länge *L* wie folgt berechnet wird:

L = 4 × Tan (α/4)/3

Die Abbildung zeigt einen Winkel von 45 Grad, sodass L gleich 0,265 ist. Im Code wird dieser Wert mit dem gewünschten Radius des Kreises multipliziert.

Mithilfe der **Kreisbogen Seite "Bezier** " können Sie experimentieren, indem Sie eine Bézier-Kurve definieren, um einen Kreisbogen für Winkel von bis zu 180 Grad zu nähern. Die Datei " [**beziercirculararcpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) " instanziiert den `SKCanvasView` und einen `Slider` zum Auswählen des Winkels. Der `PaintSurface` -Ereignishandler in der [**BezierCircularArgPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) -Code Behind-Datei verwendet eine Transformation, um den Punkt (0,0) auf den Mittelpunkt der Canvas festzulegen. Er zeichnet einen Kreis, der auf diesem Punkt für Vergleiche zentriert ist, und berechnet dann die beiden Steuerungs Punkte für die Bézier-Kurve:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
    }
}

// Vector methods
SKPoint Normalize(SKPoint v)
{
    float magnitude = Magnitude(v);
    return new SKPoint(v.X / magnitude, v.Y / magnitude);
}

float Magnitude(SKPoint v)
{
    return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
}

```

Die Start-und Endpunkte ( `point0` und `point3` ) werden basierend auf den normalen parametrischen Gleichungen für den Kreis berechnet. Da der Kreis auf (0,0) zentriert ist, können diese Punkte auch als radiale Vektoren von der Mitte des Kreises bis zum Umfang behandelt werden. Die Kontrollpunkte befinden sich in Linien, die sich im Kreis befinden, sodass Sie im rechten Winkel zu diesen radialen Vektoren sind. Ein Vektor im rechten Winkel zu einem anderen ist einfach der ursprüngliche Vektor, bei dem die X-und Y-Koordinaten ausgetauscht und eine von Ihnen negativ gemacht wurde.

Hier ist das Programm, das mit unterschiedlichen Winkeln ausgeführt wird:

[![Dreifacher Screenshot der Bezier-Kreisbogen Seite](beziers-images/beziercirculararc-small.png)](beziers-images/beziercirculararc-large.png#lightbox)

Sehen Sie sich den dritten Screenshot genauer an. Sie werden feststellen, dass die Bézier-Kurve besonders von einem semikreis abweicht, wenn der Winkel 180 Grad beträgt, aber der IOS-Bildschirm zeigt, dass er einen Quarter-Kreis genau in Ordnung hat, wenn der Winkel 90 Grad beträgt.

Das Berechnen der Koordinaten der beiden Steuerpunkte ist recht einfach, wenn der Quarter-Kreis wie folgt ausgerichtet ist:

![Näherung eines Quartals Kreises mit einer Bézier-Kurve](beziers-images/bezierarc90.png)

Wenn der Radius des Kreises 100 ist, ist *L* 55, und das ist eine leicht zu merken.

Durch die **quadratung der Kreis** Seite wird eine Abbildung zwischen einem Kreis und einem Quadrat animiert. Der Kreis entspricht vier Bézier-Kurven, deren Koordinaten in der ersten Spalte dieser Array Definition in der Klasse angezeigt werden [`SquaringTheCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) :

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

Die zweite Spalte enthält die Koordinaten von vier Bézier-Kurven, die ein Quadrat definieren, dessen Bereich ungefähr dem Bereich des Kreises entspricht. (Das Zeichnen eines Quadrats mit dem *exakten* Bereich in einem bestimmten Kreis ist das klassische, nicht lösbare geometrische Problem bei der [quadraterung des Kreises](https://en.wikipedia.org/wiki/Squaring_the_circle).) Zum Rendern eines Quadrats mit Bézier-Kurven sind die beiden Steuerungs Punkte für jede Kurve identisch, und Sie sind mit dem Start-und Endpunkt verbunden, sodass die Bézier-Kurve als gerade Linie gerendert wird.

Die dritte Spalte des Arrays ist für interinterpolierte Werte für eine Animation. Auf der Seite wird ein Timer für 16 Millisekunden festgelegt, und der `PaintSurface` Handler wird an dieser Rate aufgerufen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Die Punkte werden auf Grundlage eines sinusoidinaloszierenden Werts von interpoliert `t` . Die interkurierten Punkte werden dann verwendet, um eine Reihe von vier verbundenen Bézier-Kurven zu erstellen. Hier ist die Animation, die ausgeführt wird:

[![Dreifacher Screenshot der quadvon der kreisseite](beziers-images/squaringthecircle-small.png)](beziers-images/squaringthecircle-large.png#lightbox)

Eine solche Animation wäre ohne Kurven unmöglich, die algorithmisch flexibel genug sind, um sowohl als kreisförmige Bögen als auch als gerade Linien gerendert zu werden.

Die Seite " **Bezier unendlich** " nutzt auch die Fähigkeit einer Bézier-Kurve, einen Kreisbogen zu nähern. Hier ist der `PaintSurface` Handler der- [`BezierInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) Klasse:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
                                     info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Es ist möglicherweise eine gute Übung, diese Koordinaten im Diagramm Dokument zu zeichnen, um zu sehen, wie Sie verknüpft sind. Das Unendlichkeitszeichen wird um den Punkt (0, 0) zentriert, und die zwei Schleifen haben Mittelpunkte von (– 150, 0) und (150, 0) und Radien von 100. In der Reihe von `CubicTo` Befehlen sehen Sie X-Koordinaten von Steuerungs Punkten, die die Werte von – 95 und – 205 (diese Werte lauten – 150 Plus und minus 55), 205 und 95 (150 Plus und minus 55) sowie 250 und – 250 für die Rechte und linke Seite. Die einzige Ausnahme besteht darin, dass sich das Unendlichkeitszeichen in der Mitte befindet. In diesem Fall verfügen Kontrollpunkte über Koordinaten mit einer Kombination aus 50 und – 50, um die Kurve in der Mitte der Mitte zu korrigieren.

Hier ist das Unendlichkeitszeichen:

[![Dreifacher Screenshot der Bézier-unendlich Seite](beziers-images/bezierinfinity-small.png)](beziers-images/bezierinfinity-large.png#lightbox)

Es ist etwas komplizierter als das Unendlichkeitszeichen, das von der **Arc Infinity** -Seite gerendert wird, von den [**drei Möglichkeiten, einen Bogen Artikel zu zeichnen**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) .

## <a name="the-quadratic-bzier-curve"></a>Die Quadratische Bézier-Kurve

Die Quadratische Bézier-Kurve hat nur einen Kontrollpunkt, und die Kurve wird durch nur drei Punkte definiert: den Anfangspunkt, den Steuerungspunkt und den Endpunkt. Die parametrischen Gleichungen sind der kubischen Bézier-Kurve sehr ähnlich, mit dem Unterschied, dass der höchste Exponent 2 ist, sodass die Kurve ein quadratisches Polynomial ist:

x (t) = (1 – t) ² x ₀ + 2T (1 – t) x ₁ + t ² x 100

y (t) = (1 – t) ² y ₀ + 2T (1 – t) y ₁ + t ² y 100

Verwenden Sie die [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) -Methode oder die [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) -Überladung mit separaten `x` Koordinaten und, um einem Pfad eine quadratische Bézier-Kurve hinzuzufügen `y` :

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Die-Methoden fügen eine Kurve von der aktuellen Position zu `point2` mit `point1` als Kontrollpunkt hinzu.

Sie können mit den quadratischen Bézier-Kurven mit der **quadratischen Kurven** Seite experimentieren, die der **Bezier** -kurvenseite ähnlich ist, es sei denn, Sie enthält nur drei Berührungspunkte. Dies ist der `PaintSurface` Handler in der [**QuadraticCurve.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) -Code Behind-Datei:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Und hier wird ausgeführt:

[![Dreifacher Screenshot der Seite mit der quadratischen Kurve](beziers-images/quadraticcurve-small.png)](beziers-images/quadraticcurve-large.png#lightbox)

Die gepunkteten Linien sind für die Kurve am Anfang und am Endpunkt Tangens und werden am Steuerungspunkt angezeigt.

Die Quadratische Bézier-Struktur ist gut, wenn Sie eine Kurve einer allgemeinen Form benötigen, aber Sie bevorzugen lediglich einen Steuerungspunkt anstatt zwei. Die Quadratische Bézier-Datei wird effizienter als jede andere Kurve gerendert, weshalb Sie intern in Skia zum Rendern elliptischer Arcs verwendet wird.

Die Form einer quadratischen Bézier-Kurve ist jedoch nicht elliptisch. aus diesem Grund ist es erforderlich, dass mehrere quadratische bézierer einen elliptischen Bogen annähern. Die Quadratische Bézier ist stattdessen ein Segment einer Parabel.

## <a name="the-conic-bzier-curve"></a>Die Konstante Bézier-Kurve

Die Konstante Bézier-Kurve &mdash; , die auch als "Rational quadratischen Bézier-Kurve" bezeichnet &mdash; wird, ist eine relativ aktuelle Ergänzung zur Familie der Bézier-Kurven. Wie bei der quadratischen Bézier-Kurve umfasst die rational quadratischen Bézier-Kurve einen Startpunkt, einen Endpunkt und einen Steuerungspunkt. Die rational quadratischen Bézier-Kurve erfordert jedoch auch einen *Gewichtungs* Wert. Sie wird als *rationell* quadratisch bezeichnet, da die parametrischen Formeln die Verhältnisse einschließen.

Die parametrischen Gleichungen für X und Y sind Verhältnisse, die denselben Nenner aufweisen. Hier ist die Gleichung für den Nenner für " *t* " im Bereich von 0 bis 1 und einem Gewichtungswert von *w*:

d (t) = (1 – t) ² + 2WT (1 – t) + t ²

Theoretisch kann ein rationeller quadratischer Wert drei separate Gewichtungswerte enthalten, eine für jeden der drei Begriffe, aber Sie können auf einen Gewichtungswert in der Mitte vereinfacht werden.

Die parametrischen Gleichungen für die X-und Y-Koordinaten ähneln den parametrischen Gleichungen für den quadratischen-Bézier, mit dem Unterschied, dass der mittlere Begriff ebenfalls den Gewichtungswert enthält und der Ausdruck durch den Nenner dividiert ist:

x (t) = ((1 – t) ² x ₀ + 2WT (1 – t) x ₁ + t ² x 100)), d (t)

y (t) = ((1 – t) ² y ₀ + 2WT (1 – t) y ₁ + t ² y 100)), d (t)

Rational quadratischen Bézier-Kurven werden auch *als "als"* bezeichnet, da Sie genau Segmente eines beliebigen gleich großen Abschnitts &mdash; hyperbolas, Parabeln, Ellipsen und Kreise darstellen können.

Verwenden Sie die [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) -Methode oder die [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) -Überladung mit separaten `x` -und-Koordinaten, um einen Pfad einer rationalen quadratischen Bézier-Kurve hinzuzufügen `y` :

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Beachten Sie den letzten `weight` Parameter.

Auf der Seite für die zusammen gewollte **Kurve** können Sie mit diesen Kurven experimentieren. Die `ConicCurvePage`-Klasse wird von `InteractivePage` abgeleitet. Die Datei [**ConicCurvePage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) instanziiert eine `Slider` , um einen Gewichtungswert zwischen – 2 und 2 auszuwählen. Mit der [**ConicCurvePage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) -Code Behind-Datei `TouchPoint` werden drei Objekte erstellt, und der `PaintSurface` Handler rendert die resultierende Kurve einfach mit den Tangenten Linien zu den Steuer Punkten:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Hier wird ausgeführt:

[![Dreifacher Screenshot der Seite mit der Seite "Seite"](beziers-images/coniccurve-small.png)](beziers-images/coniccurve-large.png#lightbox)

Wie Sie sehen können, scheint der Steuerungspunkt mehr darüber zu erfahren, wenn die Gewichtung höher ist. Wenn die Gewichtung 0 (null) ist, wird die Kurve zu einer geraden Linie vom Startpunkt bis zum Endpunkt.

Theoretisch sind negative Gewichtungen zulässig und bewirken, dass die Kurve vom Steuerungspunkt *entfernt* wird. Gewichtungen von – 1 oder niedriger bewirken jedoch, dass der Nenner in den parametrischen Gleichungen für bestimmte Werte von " *t*" negativ wird. Aus diesem Grund werden negative Gewichtungen in den Methoden ignoriert `ConicTo` . Mit dem Programm für die zusammengestellte **Kurve** können Sie negative Gewichtungen festlegen, aber wie Sie durch das Experimentieren erkennen können, haben negative Gewichtungen denselben Effekt wie eine Gewichtung von 0 (null) und bewirken, dass eine gerade Linie gerendert wird.

Es ist sehr leicht, den Steuerungspunkt und die Gewichtung abzuleiten, um die-Methode zu verwenden, `ConicTo` um einen Kreisbogen bis zu einem semikreis zu zeichnen (aber nicht einschließlich). In der folgenden Abbildung treffen die Tangenten Linien von den Start-und Endpunkten am Steuerungspunkt.

![Ein konstanter Bogen Rendering eines Kreis Bogens](beziers-images/conicarc.png)

Sie können trigonometrische verwenden, um den Abstand des Steuerungs Punkts von der Mitte des Kreises zu bestimmen: Es handelt sich um den Radius des Kreises dividiert durch den Kosinus der Hälfte des Winkels. Um einen Kreisbogen zwischen dem Anfangs-und dem Endpunkt zu zeichnen, legen Sie die Gewichtung auf denselben Kosinus des halben Winkels fest. Beachten Sie Folgendes: Wenn der Winkel 180 Grad beträgt, werden die Tangenten Linien niemals eingehende und die Gewichtung ist 0 (null). Bei Winkeln, die kleiner als 180 Grad sind, funktioniert die mathematische Funktion jedoch einwandfrei.

Dies wird auf der Seite "gleich **Kreisbogen** " veranschaulicht. Die Datei "Configuration Manager [**. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) " instanziiert einen `Slider` zum Auswählen des Winkels. Der `PaintSurface` -Handler in der [**ConicCircularArc.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) -Code Behind-Datei berechnet den Steuerungspunkt und die Gewichtung:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Wie Sie sehen können, gibt es keinen visuellen Unterschied zwischen dem `ConicTo` in rot angezeigten Pfad und dem zugrunde liegenden Kreis, der als Verweis angezeigt wird:

[![Dreifacher Screenshot der Seite "Konkel Kreisbogen"](beziers-images/coniccirculararc-small.png)](beziers-images/coniccirculararc-large.png#lightbox)

Legen Sie den Winkel aber auf 180 Grad fest, und die Mathematik schlägt fehl.

In diesem Fall ist es in diesem Fall `ConicTo` nicht möglich, negative Gewichtungen zu unterstützen, da der Kreis theoretisch (basierend auf den parametrischen Gleichungen) mit einem weiteren-Befehl `ConicTo` mit denselben Punkten, aber einem negativen Wert der Gewichtung, abgeschlossen werden kann. Dies ermöglicht die Erstellung eines ganzen Kreises mit nur zwei `ConicTo` Kurven, die auf einem beliebigen Winkel zwischen (aber nicht einschließlich) 0 Grad und 180 Grad liegen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
