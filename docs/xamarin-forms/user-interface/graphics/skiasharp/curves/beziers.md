---
title: Drei Typen von Bézier-Kurven
description: Durchsuchen Sie mit SkiaSharp um quadratische, konischen und kubische Bézier-Kurven zu rendern.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 7b7bd83c474c7e0d32a693e06b5f12696ec5efa2
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="three-types-of-bzier-curves"></a>Drei Typen von Bézier-Kurven

_Durchsuchen Sie mit SkiaSharp um quadratische, konischen und kubische Bézier-Kurven zu rendern._

Bézier-Kurve wird nach Pierre Bézier (1910 – 1999), einer französischen Techniker den Automobilhersteller Renault, mit dem Namen, die die Kurve für den Entwurf computergestützten Car-Texte verwendet.

Bézier-Kurven wird sich gut für interaktives designerlebnis bekannt sind: sie sind gut konzipierte &mdash; in anderen Worten: Es stehen nicht Singularitäten, die dazu führen, dass die Kurve unendlich oder unhandlich werden &mdash; und sind im Allgemeinen ansprechend . Zeichenumrisse von computerbasierte Schriftarten sind in der Regel mit Bézier-Kurven definiert:

![](beziers-images/beziersample.png "Eine Beispiel-Bézier-Kurve")

Der Wikipedia-Artikel zu [Bézier-Kurve](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) enthält einige nützliche Informationen. Der Begriff *Bézier-Kurve* tatsächlich bezieht sich auf eine Familie von ähnlichen Kurven. SkiaSharp unterstützt drei Typen von Bézier-Kurven wird aufgerufen, die *kubische*, *quadratische*, und die *konischen*. Die Conic ist auch bekannt als die *rational "quadratisch"*.

## <a name="the-cubic-bzier-curve"></a>Die kubische Bézier-Kurve

Die kubische ist der Typ der Bézier-Kurve, die meisten Entwickler vorstellen, wenn das Subjekt der Bézier-Kurven hochgefahren.

Sie können eine kubische Bézier-Kurve zum Hinzufügen ein `SKPath` -Objekt unter Verwendung der [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) Methode mit drei `SKPoint` Parameter, oder die [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) Überladung mit separaten `x` und `y` Parameter:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Die Kurve beginnt am aktuellen Punkt der Kontur. Die vollständige quadratische Bézier-Kurve wird durch vier Punkte definiert:

- Startpunkt: aktuelle zeigen in der Kontur oder (0, 0), wenn `MoveTo` nicht aufgerufen wurde
- zuerst, Utility control Point: `point1` in die `CubicTo` aufrufen
- Zweitens, Utility control Point: `point2` in die `CubicTo` aufrufen
- Endpunkt: `point3` in die `CubicTo` aufrufen

Die resultierende Kurve am Anfangspunkt beginnt und endet am Endpunkt. Die Kurve verläuft in der Regel nicht über die beiden Steuerpunkte; Stattdessen funktionieren viel like Magnete, um die Kurve wird in diese Richtung ziehen.

Die beste Möglichkeit, ein Gefühl für die kubische Bézier-Kurve wird durch Experimentieren ermittelt. Dies ist der Zweck der **Bézier-Kurve** Seite abgeleitet von `InteractivePage`. Die [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) Datei instanziiert den `SKCanvasView` und ein `TouchEffect`. Die [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) Code-Behind-Datei erstellt vier `TouchPoint` Objekte im entsprechenden Konstruktor. Die `PaintSurface` -Ereignishandler erstellt ein `SKPath` zum Rendern einer Bézier-Kurve, die basierend auf den vier `TouchPoint` Objekte und zeichnet auch gepunktete Tangenten aus den Punkten für die Steuerung an den Endpunkten:

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

Hier wird die Anwendung auf allen drei Plattformen ausgeführt:

[![](beziers-images/beziercurve-small.png "Dreifacher Screenshot der Seite Bézier-Kurve")](beziers-images/beziercurve-large.png#lightbox "dreifacher Screenshot der Seite Bézier-Kurve")

Mathematisch, ist die Kurve eine kubische Polynoms. Die Kurve wird eine gerade Linie an drei Punkten darf höchstens überschneidet. Klicken Sie auf den Startpunkt. die Kurve ist immer eine gerade Linie vom Beginn Tangenten, und klicken Sie in der gleichen Richtung wie, zeigen Sie auf den ersten Kontrollpunkt. An den Endpunkt die Kurve ist immer eine gerade Linie von das zweite Steuerelement Tangenten, und klicken Sie in der gleichen Richtung wie, zeigen Sie auf den Endpunkt.

Die kubische Bézier-Kurve wird immer durch eine konvexe Quadrat, das die vier Punkte verbindet festgelegt. Hierbei spricht einen *konvexe Hülle*. Wenn das Steuerelement zeigt auf die gerade Linie zwischen den Start- und Endpunkt liegen, wird der Bézier-Kurve als eine gerade Linie gerendert. Aber die Kurve kann auch überqueren selbst, wie der dritten Screenshot veranschaulicht.

Eine Pfad Kontur kann mehrere verbundene kubische Bézier-Kurven enthalten, die Verbindung zwischen zwei kubische Bézier-Kurven wird smooth nur, wenn die folgenden drei Punkte kollineare sind (d. h. auf einer geraden Linie liegen):

- der zweite Kontrollpunkt der Kurve an der ersten
- der Endpunkt der ersten Kurve, die auch den Ausgangspunkt der Kurve an der zweiten ist
- der erste Kontrollpunkt der Kurve an der zweiten

Im nächsten Artikel auf [ **SVG-Pfaddaten** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) erfahren Sie, eine Funktion, die die Definition der smooth verbundenen Bézier-Kurven zu erleichtern.

Manchmal ist es hilfreich zu wissen, die zugrunde liegenden parametrischen Formeln, die eine kubische Bézier-Kurve zu rendern. Für *t* zwischen 0 und 1 steht, die parametrischen Gleichungen lauten wie folgt:

x(t) = (1 – t) ³x₀ + 3 t (1 – t) ²x₁ + 3t² (1 – t) X₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3 t (1 – t) ²y₁ + 3t² (1 – t) Y₂ + t³y₃

Der höchsten Exponenten 3 bestätigt, dass kubische Polynomials handelt. Es ist einfach, überprüfen, dass bei `t` 0 entspricht, wird der Punkt ist (X₀ Y₀) und den Ausgangspunkt und wann `t` gleich 1 ist, der Punkt (X₃ Y₃), also der Endpunkt. In der Nähe der Startpunkt (für niedrige Werte von `t`), der erste Kontrollpunkt (X₁, Y₁) hat ein sicheres wirksam und in der Nähe der Endpunkt (hohe Werte von ' t ') der zweite Kontrollpunkt (X₂, Y₂) hat eine starke Auswirkungen.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Bézier-Kurve Näherung Kreisbögen

Manchmal ist es praktisch sein, eine Bézier-Kurve verwenden, um einen Kreisbogen zu rendern. Eine kubische Bézier-Kurve kann eine sehr gute Leistung bis zu ein Viertel Kreis, Kreisbogen damit vier verbundenen Bézier-Kurven können einen gesamten Kreis zu definieren. Diese Schätzung wird in zwei Artikeln, die vor mehr als 25 Jahre veröffentlicht erläutert:

> Tor Dokken, u. a., "Gute Näherung der Kreise von Krümmung fortlaufende Bézier-Kurven" *Computer Aided geometrische Design 7* (1990), 33 41.

> Michael Goldapp, "Approximation Kreisbögen von kubische Polynomials" *Computer-Aided geometrische Design 8* (1991), 227 238.

Die folgende Abbildung zeigt vier Punkte, die mit der Bezeichnung `pto`, `pt1`, `pt2`, und `pt3` definieren eine Bézier-Kurve (in Rot dargestellt), die ein Kreisbogensegment entspricht:

![](beziers-images/bezierarc45.png "Approximation einen Kreisbogen mit einer Bézierkurve")

Die Zeilen aus der Start- und Endpunkte, die die Steuerpunkte sind Tangens des Kreises und den Bézier-Kurve und verfügen über eine Länge von *L*. Der erste Artikel, die oben genannten gibt an, dass die beste Bézier-Kurve einen Kreisbogen entspricht in etwa wenn diese Länge *L* wird wie folgt berechnet:

L = 4 x tan(α / 4) / 3

Die Abbildung zeigt einen Winkel von 45 Grad an, damit L 0.265 entspricht. Im Code würde dieser Wert mit den gewünschten Radius des Kreises multipliziert werden.

Die **Bézier-Kreisbogen** Seite können Sie definieren eine Bézier-Kurve, um einen Kreisbogen für Winkel im Bereich von bis zu 180 Grad experimentieren. Die [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) Datei instanziiert den `SKCanvasView` und ein `Slider` für die Auswahl des Winkels. Die `PaintSurface` -Ereignishandler in der [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) Code-Behind-Datei verwendet eine Transformation, um den Punkt (0, 0) in die Mitte des Zeichenbereichs festzulegen. Es zeichnet einen Kreis, dessen Mitte sich an diesem Punkt für den Vergleich und berechnet dann die beiden Steuerpunkte der Bézier-Kurve:

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

Die Start- und Endpunkte (`point0` und `point3`) werden auf die normale parametrischen Formeln für den Kreis berechnet. Da auf der Kreis zentriert ist (0, 0), diese Punkte können auch als behandelt werden radiale Vektoren vom Mittelpunkt des Kreises, der bei. Die Steuerpunkte befinden sich auf Zeilen, die Tangens auf den Kreis sind, damit sie im rechten Winkel zu diese radialen Vektoren sind. Ein Vektor in einem rechten Winkel ist einfach der ursprünglichen Vektor mit den X- und Y-Koordinaten, die ausgetauscht und eine von ihnen vorgenommenen negativ.

Hier wird das Programm ausgeführt wird, auf die drei Plattformen mit drei verschiedenen Blickwinkeln dar:

[![](beziers-images/beziercirculararc-small.png "Dreifacher Screenshot der Seite Kreisbogen Bézier-")](beziers-images/beziercirculararc-large.png#lightbox "dreifacher Screenshot der Kreisbogen Bézier-Seite")

Sehen Sie sich die dritte Screenshot, und Sie sehen, dass der Bézier-Kurve wird vor allem aus ein Halbkreis abweicht, wird bei der des Winkels um 180 Grad Unzulässiger der iOS-Bildschirm zeigt, dass es scheint, passen einen Quartal Kreis einwandfrei aus, wenn der Winkel 90 Grad ist.

Berechnen die Koordinaten der die beiden Steuerpunkte ist recht einfach, wenn das Quartal Kreis objektorientierte sieht ist:

![](beziers-images/bezierarc90.png "Näherung für ein Quartal Kreis mit einem Bézier-Kurve")

Wenn der Radius des Kreises 100, dann ist *L* 55 ist und eine einfache Anzahl zu merken ist.

Die **Quadrieren des Kreises** Seite eine Animation, eine Zahl zwischen einen Kreis oder ein Quadrat. Der Kreis wird ermittelt, werden vier Bézier-Kurven, dessen Koordinaten werden in diesem Array-Definition in der ersten Spalte angezeigt, der [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) Klasse:

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

Die zweite Spalte enthält die Koordinaten der vier Bézier-Kurven, die ein Quadrat definieren, deren Bereich ungefähr den Bereich des Kreises identisch ist. (Zeichnen ein Quadrat mit der *genaue* Bereich als angegebenen Kreis ist das klassische unlösbaren geometrische Problem der [Quadrieren des Kreises](https://en.wikipedia.org/wiki/Squaring_the_circle).) Für das Rendern eines Quadrats mit Bézier-Kurven, die beiden Steuerpunkte für jede Kurve identisch sind, und sie sind mit den Start- und Endpunkte, kollineare, sodass der Bézier-Kurve als eine gerade Linie gerendert wird.

Die dritte Spalte des Arrays ist interpolierte Werte für eine Animation. Die Seite wird ein Timer für 16 Millisekunden und dem `PaintSurface` Handler wird aufgerufen, bei dieser Rate:

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

Die Punkte werden basierend auf Sinusförmig oszillierende Wert interpoliert `t`. Die interpolierten Punkte werden dann verwendet, um eine Reihe von vier verbundenen Bézier-Kurven zu erstellen. So sieht die Animation ausgeführt wird, auf die drei Plattformen, die mit dem Status von einem Kreis in ein Quadrat aus:

[![](beziers-images/squaringthecircle-small.png "Dreifacher Screenshot, der die Squaring der Seite "Kreis"")](beziers-images/squaringthecircle-large.png#lightbox "dreifacher Screenshot, der die Squaring der Seite "Kreis"")

Solche eine Animation wäre ohne Kurven unmöglich, die algorithmisch flexibel genug ist, als Kreisbögen und gerade Linien dargestellt werden.

Die **Bézier-unendlich** Seite nutzt auch die Möglichkeit einer Bézier-Kurve, die einen Kreisbogen. So sieht die `PaintSurface` Handler aus der [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) Klasse:

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

Es kann sein, dass eine gute Übung, zeichnen diese Koordinaten auf Papier Diagramm, um festzustellen, wie diese miteinander verknüpft sind. Die Vorzeichen unendlich ist zentriert, um den Punkt (0, 0), und zwei Schleifen haben zentriert (–150, 0) und (150, 0) und Radii von 100. In der Reihe der `CubicTo` Befehle, sehen Sie X-Koordinaten der Steuerpunkte, die auf Werten –95 und –205 dauert (diese Werte sind –150 Plus- und Minuszeichen 55), 205 und 95 (150 Plus- und Minuszeichen 55) als auch 250 und –250 für den linken und rechten Seiten. Die einzige Ausnahme ist, wenn die Infinity-Zeichen in der Mitte überschreitet. In diesem Fall haben Steuerpunkte Koordinaten mit einer Kombination von 50 und –50 zu Strecken, die Kurve wird in der Mitte.

Hier wird die Vorzeichen unendlich auf allen drei Plattformen ein:

[![](beziers-images/bezierinfinity-small.png "Dreifacher Screenshot der Seite Bézier unendlich")](beziers-images/bezierinfinity-large.png#lightbox "dreifacher Screenshot der Seite Bézier unendlich")

Es ist etwas glattere Richtung der Mitte als das unendlich Vorzeichen von gerendert der **Bogen unendlich** Seite aus der [ **drei Möglichkeiten, einen Bogen gezeichnet werden soll** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) Artikel.

## <a name="the-quadratic-bzier-curve"></a>Der quadratische Bézier-Kurve

Quadratische Bézier-Kurve hat nur eine Kontrollpunkt, und die Kurve wird von nur drei Punkte definiert: den Ausgangspunkt der Kontrollpunkt und den Endpunkt. Die parametrischen Gleichungen sind die kubische Bézier-Kurve sehr ähnlich, außer, dass der höchste Exponent. die Kurve eine quadratische Polynoms ist 2, ist:

x(t) = (1 – t) ²x₀ + 2 t (1 – t) X₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2 t (1 – t) Y₁ + t²y₂

Um einen Pfad eine quadratische Bézier-Kurve hinzugefügt haben, verwenden die [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) Methode oder die [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) Überladung mit separaten `x` und `y` Koordinaten:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Die Methoden fügen eine Kurve aus der aktuellen Position bis `point2` mit `point1` als der Kontrollpunkt.

Sie können experimentieren mit quadratische Bézier-Kurven mit der **quadratische Kurve** Seite, die sehr ähnlich sind die **Bézier-Kurve wird** Seite, außer es nur drei Berührungspunkte hat. So sieht die `PaintSurface` Ereignishandler in der [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) Code-Behind-Datei:

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

Und hier ist er auf allen drei Plattformen ausgeführt werden:

[![](beziers-images/quadraticcurve-small.png "Dreifacher Screenshot der Seite quadratische Kurve")](beziers-images/quadraticcurve-large.png#lightbox "dreifacher Screenshot der Seite quadratische Kurve")

Die gepunkteten Linien sind Tangens der Kurve an der Start- und Endpunkt und der Kontrollpunkt treffen.

Der quadratische Bézier eignet sich, eine Kurve mit einer Form "Allgemein", und ziehen Sie den Komfort von nur einem Kontrollpunkt statt zwei. Der quadratische Bézier rendert effizienter als alle anderen Kurve, weshalb es intern in Skia verwendet wird, zum Rendern des elliptischen Bogens.

Allerdings ist die Form des eine quadratische Bézier-Kurve nicht elliptischen, weshalb mehrere quadratische Béziers erforderlich sind, um einen elliptischen Bogen zu ermitteln. Der quadratische Bézier wird stattdessen ein Segment auf einem Parabola.

## <a name="the-conic-bzier-curve"></a>Die konischen Bézier-Kurve

Die konischen Bézier-Kurve &mdash; auch bekannt als die rational quadratische Bézier-Kurve &mdash; ist eine relativ neue Erweiterung für die Familie der Bézier-Kurven. Wie die quadratische Bézier-Kurve umfasst rational quadratische Bézier-Kurve an einem Startpunkt, einen Endpunkt und Steuerungspunkts für das ein. Rational quadratische Bézier-Kurve erfordert jedoch auch eine *Gewichtung* Wert. Sie wird aufgerufen, eine *rational* quadratische, da die parametrischen Formeln Verhältnissen beinhalten.

Die parametrischen Formeln für X und Y Verhältnissen sind, die den gleichen Nenner gemeinsam nutzen. So sieht die Gleichung für den Nenner für *t* im Bereich von 0 auf 1 und einen Gewichtungswert von *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

Klicken Sie in der Theorie eine vernünftige "quadratisch" kann drei separate Gewichtung-Werte; einen für jede der drei Begriffe enthalten, zwar, aber diese an nur einem Gewichtungswert nach dem Begriff mittleren vereinfacht werden.

Die parametrischen Formeln für die X- und Y-Koordinaten ähneln parametrischen Formeln für das quadratische Bézier identisch, die mittleren Begriff auch den Wert für die Linienstärke umfasst und der Ausdruck Nenner und ist:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) X₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) Y₁ + t²y₂)) ÷ d(t)

Rational quadratische Bézier-Kurven heißen auch *Conics* , da sie für jeden Bereich Conic Segmente genau darstellen können &mdash; Hyperbeln, Parabeln und Ellipsen, Kreisen.

Um einen Pfad eine vernünftige quadratische Bézier-Kurve hinzugefügt haben, verwenden die [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) Methode oder die [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) Überladung mit separaten `x` und `y` Koordinaten:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Beachten Sie die endgültige `weight` Parameter.

Die **konischen Kurve** Seite können Sie diese Kurven experimentieren. Die `ConicCurvePage`-Klasse wird von `InteractivePage` abgeleitet. Die [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) Datei instanziiert einen `Slider` einen Gewichtungswert zwischen – 2 und 2 auswählen. Die [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) Code-Behind-Datei erstellt drei `TouchPoint` Objekte, und die `PaintSurface` Handler einfach rendert die resultierenden Kurve mit der Tangenten an das Steuerelement Punkte:

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

Hier wird die Anwendung auf allen drei Plattformen ausgeführt:

[![](beziers-images/coniccurve-small.png "Dreifacher Screenshot der Seite konischen Kurve")](beziers-images/coniccurve-large.png#lightbox "dreifacher Screenshot der Seite konischen Kurve")

Wie Sie sehen können, scheint der Kontrollpunkt die Kurve für sie weitere pull, wenn das Gewicht größer ist. Wenn die Gewichtung auf 0 (null) ist, wird die Kurve eine gerade Linie von der Startpunkt zum Endpunkt an.

In der Theorie negative Gewichtungen sind zulässig und dazu führen, dass die Kurve Verbiegen *sofort* aus der Kontrollpunkt. Allerdings von – 1 oder unter Ursache Nenner in die parametrischen Formeln für bestimmte Werte von negativ gewichtet *t*. Aus diesem Grund werden negative Gewichtungen wahrscheinlich ignoriert die `ConicTo` Methoden. Die **konischen Kurve** Programm können Sie die negative Gewichtungen festgelegt, wie Sie lieber sehen können, negative Gewichtungen haben jedoch dieselbe Wirkung wie eine Gewichtung von 0 (null), und dazu führen, dass eine gerade Linie gerendert werden soll.

Es ist sehr einfach, leiten Sie den Kontrollpunkt und Gewichtung zu verwenden die `ConicTo` -Methode, um einen Kreisbogen bis zu zeichnen, (aber nicht einschließlich) ein Halbkreis. Im folgenden Diagramm treffen Tangenten aus der Start- und Endpunkte der Kontrollpunkt.

![](beziers-images/conicarc.png "Ein Rendering konischen Bogen in einen Kreisbogen")

Können Sie so bestimmen Sie die Entfernung des Kontrollpunkts aus Kreismittelpunkts trigonometrische: den Radius des Kreises geteilt durch den Kosinus der Hälfte der Winkel α ist. Um einen Kreisbogen zwischen den Start- und Endpunkte zu zeichnen, legen Sie die Gewichtung, dieselbe Kosinus des Winkels Hälfte ein. Beachten Sie, dass der Drehwinkel um 180 Grad ist, klicken Sie dann die Tangenten nie erfüllen und die Gewichtung 0 (null). Aber für Winkel kleiner als 180 Grad ist, ist die mathematischen hervorragend.

Die **konischen Kreisbogen** veranschaulicht dies. Die [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) Datei instanziiert einen `Slider` für die Auswahl des Winkels. Die `PaintSurface` Ereignishandler in der [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) Code-Behind-Datei wird der Kontrollpunkt und die Gewichtung berechnet:

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

Wie Sie sehen können, ist es keinen visuellen Unterschied zwischen der `ConicTo` Pfad Rot und der zugrunde liegenden Kreis Referenzzwecken angezeigt:

[![](beziers-images/coniccirculararc-small.png "Dreifacher Screenshot der Seite konischen Kreisbogen")](beziers-images/coniccirculararc-large.png#lightbox "dreifacher Screenshot der Seite konischen Kreisbogen konvertiert.")

Sondern Sie den Winkel um 180 Grad und der Mathematik fehl.

Es ist in diesem Fall unglücklicher, `ConicTo` unterstützt keine negative Gewichtungen aus, da in der Theorie (basierend auf der parametrischen Gleichungen) der Kreis mit einem weiteren Aufruf von abgeschlossen werden kann `ConicTo` mit demselben Punkt, aber einen negativen Wert der Gewichtung. Dies würde ermöglichen, erstellen einen gesamten Kreis mit nur zwei `ConicTo` Kurven basierend auf einem beliebigen Winkel zwischen (aber nicht einschließlich) 0 (null) Grad und 180 Grad liegen.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
