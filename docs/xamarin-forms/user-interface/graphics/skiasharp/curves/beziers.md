---
title: Drei Arten von Bézierkurven
description: In diesem Artikel wird erläutert, wie SkiaSharp, die zum Rendern von kubische, quadratischen und konischen Bézierkurven in Xamarin.Forms-Anwendungen verwenden, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
ms.openlocfilehash: 1da0ee6155548a38057e4c7bf49ae5b90d445d79
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615339"
---
# <a name="three-types-of-bzier-curves"></a>Drei Arten von Bézierkurven

_Erfahren Sie, wie SkiaSharp, die zum Rendern von kubische, quadratischen und konischen Bézierkurven verwenden_

Die Bézierkurve ist nach Pierre Bézier (1910: 1999), einem französischen Engineer bei der Automobilhersteller Renault, mit dem Namen, die die Kurve für den Computer-gestützten Entwurf Car-Nachrichtentexte verwendet.

Bézierkurven werden für die sich gut für die interaktive Design bezeichnet: werden verhaltenden &mdash; heißt, es sind nicht Singularitäten, die dazu führen, die Kurve dass wird unendlich oder schwer verwaltbar erweist &mdash; und sie sind im Allgemeinen ästhetisch ansprechende :

![](beziers-images/beziersample.png "Ein Beispiel Bezier-Kurve")

Zeichenumrisse computerbasierte Schriftarten werden in der Regel mit Bézierkurven definiert.

Der Wikipedia-Artikel zu [ **Bézierkurve** ](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) enthält einige wichtige Hintergrundinformationen. Der Begriff *Bézierkurve* verweist auf eine Gruppe von ähnlichen Kurven. SkiaSharp unterstützt drei Arten von Bézierkurven, dem Namen der *kubische*, *quadratischen*, und die *konischen*. Die Conic ist auch bekannt als die *rational Quadratic*.

## <a name="the-cubic-bzier-curve"></a>Die kubische Bézierkurve

Die kubische ist der Typ des Bézierkurve, die meisten Entwickler vorstellen, wenn der Antragsteller des Bézierkurven angezeigt wird.

Können Sie eine kubische Bézierkurve zum Hinzufügen einer `SKPath` -Objekt unter Verwendung der [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) Methode mit drei `SKPoint` Parameter oder die [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) Überladung mit separaten `x` und `y` Parameter:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Die Kurve beginnt am aktuellen Punkt der Kontur. Die vollständige kubische Bezier-Kurve, wird durch vier Punkte definiert:

- Startpunkt: aktuelle zeigen in der Kontur oder (0, 0), wenn `MoveTo` nicht aufgerufen wurde
- zuerst, Utility control Point: `point1` in die `CubicTo` aufrufen
- Zweitens, Utility control Point: `point2` in die `CubicTo` aufrufen
- Endpunkt: `point3` in die `CubicTo` aufrufen

Die resultierende Kurve beginnt am Anfangspunkt und endet mit dem Endpunkt. Die Kurve übergibt in der Regel nicht über die beiden Steuerpunkte; Stattdessen zeigt das Steuerelement Funktion ähnlich wie Magnetquellen Kurve hauptneuerungen ihnen abrufen.

Die beste Möglichkeit, ein Gefühl für die die kubische Bézierkurve ist durch Experimentieren ermittelt. Dies ist der Zweck der **Bezier-Kurve** Seite abgeleitet `InteractivePage`. Die [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) Datei instanziiert den `SKCanvasView` und `TouchEffect`. Die [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) Code-Behind-Datei erstellt vier `TouchPoint` Objekte in seinem Konstruktor. Die `PaintSurface` -Ereignishandler erstellt ein `SKPath` zum Rendern einer Bézierkurve basierend auf den vier `TouchPoint` Objekte und zeichnet auch die gepunktete Tangenten aus die Kontrollpunkte für die Endpunkte:

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

[![](beziers-images/beziercurve-small.png "Dreifacher Screenshot der Seite Bezier-Kurve")](beziers-images/beziercurve-large.png#lightbox "dreifachen Screenshot der Seite Bezier-Kurve")

Aus mathematischer Sicht ist die Kurve einen kubischen Polynom. Die Kurve wird eine gerade Linie an drei Punkten höchstens überschneidet. Auf den Anfangspunkt die Kurve ist immer eine gerade Linie von Anfang Tangens, und klicken Sie in die gleiche Richtung wie, zeigen Sie auf den ersten Kontrollpunkt. Auf den Endpunkt die Kurve ist immer eine gerade Linie von das zweite Steuerelement Tangens, und klicken Sie in die gleiche Richtung wie, zeigen Sie auf den Endpunkt.

Die kubische Bézierkurve wird immer durch eine konvexe Viereck zwischen den vier Punkten begrenzt. Dies wird als bezeichnet ein *konvexe Hülle*. Wenn das Steuerelement zeigt auf die gerade Linie zwischen den Start- und Endpunkt befinden, wird die Bézierkurve als eine gerade Linie gerendert. Aber die Kurve kann auch plattformübergreifende selbst, wie im dritten Screenshot veranschaulicht.

Eine Kontur Pfad kann mehrere verbundene kubische Bézierkurven enthalten, die Verbindung zwischen zwei kubische Bézierkurven sind jedoch nur, wenn die folgenden drei Punkte kollineare smooth (d. h. auf einer geraden Linie liegen):

- der zweite Kontrollpunkt der Kurve für den ersten
- der Endpunkt der erste Kurve, die auch den Anfangspunkt der Kurve für die zweite ist
- der erste Kontrollpunkt, der die zweite Kurve

Im nächsten Artikel auf [ **SVG-Pfaddaten**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md), erfahren Sie, eine Funktion, um die Definition der smooth verbundenen Bézierkurven zu erleichtern.

Manchmal ist es hilfreich zu wissen, die zugrunde liegenden parametrischen Formeln, die eine kubische Bézierkurve zu rendern. Für *t* zwischen 0 und 1, parametrischen Formeln lauten wie folgt:

x(t) = (1 – t) ³x₀ + 3 t (1 – t) ²x₁ + 3t² (1 – t) X₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3 t (1 – t) ²y₁ + 3t² (1 – t) Y₂ + t³y₃

Der höchste Exponent 3 bestätigt, dass diese kubische Polynomials sind. Es ist einfach, überprüfen Sie, ob bei `t` gleich 0, die Frage ist (X₀, Y₀): Dies ist der Ausgangspunkt, und wann `t` gleich 1 ist, der Punkt (X₃, Y₃), dies ist der Endpunkt. In der Nähe der Anfangspunkt (für niedrige Werte für `t`), der erste Kontrollpunkt (X₁, Y₁) verfügt über eine starke Auswirkung und in der Nähe der Endpunkt (hohe Werte von t ") der zweite Kontrollpunkt (X₂, Y₂) hat einen großen Einfluss.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>Bezier-Kurve Näherung Kreisbögen

Manchmal ist es sinnvoll, eine Bézierkurve verwenden, um ein Kreisbogensegment zu rendern. Eine kubische Bézierkurve kann ein Kreisbogensegment sehr gut bis zu ein Viertel Kreis, Ungefährer also vier verbundenen Bézierkurven können einen gesamten Kreis zu definieren. Diese Schätzung wird in zwei Artikeln, die vor mehr als 25 Jahren veröffentlicht erläutert:

> Tor Dokken, u. a., "Gute Annäherung des Kreise von Krümmung ununterbrochen Bézierkurven," *Computer Aided geometrische Design 7* (1990), 33: 41.

> Michael Goldapp, "Approximation Kreisbögen von kubische Polynomials," *CAE-geometrische Design 8* (1991), 227 238.

Das folgende Diagramm zeigt die vier Punkte, die mit der Bezeichnung `pto`, `pt1`, `pt2`, und `pt3` definieren eine Bézierkurve (in Rot dargestellt), die ein Kreisbogensegment entspricht:

![](beziers-images/bezierarc45.png "Ein Kreisbogensegment mit eine Bézierkurve Näherung")

Die Zeilen aus der Start- und Endpunkt der Control-Punkte sind Tangens auf den Kreis und die Bézierkurve aus, und sie haben eine Länge von *L*. Im erste Artikel, die oben genannten gibt an, dass die beste Bézierkurve ein Kreisbogensegment entspricht bei der diese Länge *L* wird wie folgt berechnet:

L = 4 × tan(α / 4) / 3

Die Abbildung zeigt einen Drehwinkel um 45 Grad, sodass L 0.265 entspricht. Im Code würde diesen Wert mit den gewünschten Radius des Kreises multipliziert werden.

Die **Bezier-Kreisbogen** Seite ermöglicht Ihnen das Experimentieren mit der Definition einer Bézierkurve, um einen Kreisbogen für Winkel, die im Bereich von bis zu 180 Grad Ungefährer. Die [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) Datei instanziiert den `SKCanvasView` und `Slider` für die Auswahl des Winkels. Die `PaintSurface` -Ereignishandler in der [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) Code-Behind-Datei verwendet eine Transformation, um die Mitte der Canvas den Punkt (0, 0) fest. Es zeichnet einen Kreis, dessen Mitte sich an diesem Punkt für den Vergleich und berechnet dann die beiden Steuerpunkte, die für die Bézierkurve:

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

Die Start- und Endpunkt (`point0` und `point3`) werden basierend auf den normalen parametrischen Formeln für den Kreis berechnet. Da auf der Kreis zentriert ist (0, 0), diese Punkte können auch als behandelt werden radiale Vektoren aus der Mitte des Kreises auf den Umfang. Die Control-Punkte sind in den Zeilen, die Tangente auf den Kreis, sodass sie auf diese radialen Vektoren rechtwinklig sind. Im rechten Winkel in eine andere besteht einfach dem ursprünglichen Vektor mit den X- und Y-Koordinaten, die ausgetauscht und das andere von ihnen vorgenommenen negativ.

So sieht das Programm ausgeführt wird, auf die drei Plattformen mit drei verschiedenen Blickwinkeln aus:

[![](beziers-images/beziercirculararc-small.png "Dreifacher Screenshot der Seite Bezier-Kreisbogen")](beziers-images/beziercirculararc-large.png#lightbox "dreifachen Screenshot der Kreisbogen Bezier-Seite")

Sehen Sie sich im dritten Screenshot aus, und sehen Sie, dass die Bézierkurve wird vor allem aus ein Halbkreis abweicht, wenn der Winkel um 180 Grad, aber iOS-Bildschirms zeigt, dass es scheint, passen Viertelkreis-einwandfrei, wenn der Winkel 90 Grad ist.

Berechnen die Koordinaten der die beiden Steuerpunkte ist recht einfach, wenn die Viertelkreis wie folgt ausgerichtet ist:

![](beziers-images/bezierarc90.png "Ein Viertel Kreis mit einem Bézierkurve Näherung")

Wenn der Radius des Kreises 100, dann ist *L* 55 ist und das ist eine einfache Anzahl merken müssen.

Die **Quadrieren des Kreises** Seite erstellt eine Animation eine Abbildung zwischen einen Kreis oder ein Quadrat. Der Kreis wird durch vier Bézierkurven, dessen Koordinaten werden in dieser Definition Array, in der ersten Spalte angezeigt, angeglichen der [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) Klasse:

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

Die zweite Spalte enthält die Koordinaten der vier Bézierkurven, die ein Quadrat definieren, deren Bereich ungefähr die Fläche des Kreises identisch ist. (Zeichnen ein Quadrat mit der *genaue* Bereich wie einen bestimmten Kreis besteht in der klassischen unlösbar geometrische Problem [Quadrieren des Kreises](https://en.wikipedia.org/wiki/Squaring_the_circle).) Für das Rendern eines Quadrats mit Bézierkurven an, die beiden Steuerpunkte für jede Kurve sind identisch, und sie sind mit den Start- und Endpunkt, kollineare, sodass die Bézierkurve als gerade Linie gerendert wird.

Die dritte Spalte des Arrays ist für interpolierte Werte für eine Animation. Die Seite wird ein Timer bei 16 Millisekunden festgelegt und die `PaintSurface` Handler wird aufgerufen, bei dieser Rate:

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

Die Punkte werden basierend auf den Sinusförmig oszillierenden Wert interpoliert `t`. Die interpolierte Punkte werden dann verwendet, um eine Reihe von vier verbundenen Bézierkurven zu erstellen. Hier ist die Animation, die auf den Status von einem Kreis, um ein Quadrat mit drei Plattformen ausgeführt werden:

[![](beziers-images/squaringthecircle-small.png "Dreifacher Screenshot, der die Squaring der Seite \"Kreis\"")](beziers-images/squaringthecircle-large.png#lightbox "dreifachen Screenshot, der die Squaring der Seite \"Kreis\"")

Eine solche Animation wäre ohne Kurven unmöglich, die algorithmisch flexibel genug ist, als Kreisbögen und gerade Linien gerendert werden soll.

Die **Bezier-Infinity** Seite zudem nutzt die Fähigkeit einer Bézierkurve ein Kreisbogensegment Annäherung an. Hier ist die `PaintSurface` -Handler aus der [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) Klasse:

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

Es kann sein, dass eine gute Übung zum Zeichnen diese Koordinaten auf Graph Papier, um anzuzeigen, wie diese miteinander verknüpft sind. Die Vorzeichen unendlich ist zentriert, um den Punkt (0, 0) und die zwei Schleifen haben, wird von (–150, 0) und (150, 0) und die Radien der 100. In den `CubicTo` Befehle sehen Sie X-Koordinaten des Steuerelements auf den Werten der –95 und –205 dauert (diese Werte sind –150 Plus- und Minuszeichen 55), 205 und 95 (150 Plus- und Minuszeichen 55) als auch 250 und –250 für die Rechte und linke Seite. Die einzige Ausnahme ist, wenn die Infinity-Zeichen in der Mitte überschreitet. In diesem Fall haben Steuerungspunkte Koordinaten mit einer Kombination von 50 und –50 Biegen Sie sich die Kurve in der Mitte.

Hier ist der Infinity-Anmeldung auf allen drei Plattformen ein:

[![](beziers-images/bezierinfinity-small.png "Dreifacher Screenshot der Seite Bézier unendlich")](beziers-images/bezierinfinity-large.png#lightbox "dreifachen Screenshot der Seite Bézier unendlich")

Es ist etwas glatter in Richtung der Mitte als das Infinity-Zeichen, die von gerendert der **Bogen unendlich** Seite die [ **drei Möglichkeiten, einen Bogen zu zeichnen** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) Artikel.

## <a name="the-quadratic-bzier-curve"></a>Die quadratische Bézierkurve

Die quadratische Bézierkurve hat nur ein Steuerelement zeigen und die Kurve durch nur drei Punkte definiert ist: den Startpunkt der Kontrollpunkt und den Endpunkt. Parametrischen Formeln sind die kubische Bézierkurve, sehr ähnlich, außer dass der höchsten Exponenten 2, damit die Kurve eine quadratische Polynom ist:

x(t) = (1 – t) ²x₀ + 2 t (1 – t) X₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2 t (1 – t) Y₁ + t²y₂

Verwenden Sie zum Hinzufügen einer quadratischen Bézierkurve auf einen Pfad der [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) Methode oder der [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) Überladung mit separaten `x` und `y` Koordinaten:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Die Methoden hinzufügen eine Kurve, aus der aktuellen Position bis `point2` mit `point1` als Steuerungspunkts für das.

Sie können mit quadratischen Bézierkurven mit experimentieren der **quadratischen Kurve** Seite ähnelt der **Bezier-Kurve** Seite aber nur drei Touch-Punkte. Hier ist die `PaintSurface` Ereignishandler in der [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) Code-Behind-Datei:

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

[![](beziers-images/quadraticcurve-small.png "Dreifacher Screenshot der Seite quadratischen Kurve")](beziers-images/quadraticcurve-large.png#lightbox "dreifachen Screenshot der Seite quadratischen Kurve")

Die gepunkteten Linien Tangente der Kurve in den Startpunkt und Endpunkt sind und an dem Punkt des Steuerelements zu erfüllen.

Der quadratischen Bézier ist gut, wenn eine Kurve einer allgemeinen Form erforderlich, aber Sie die Vorteile von nur einem Kontrollpunkt statt zwei lieber. Die quadratische Bézier rendert effizienter als alle anderen Kurve, weshalb sie intern in Skia verwendet wird, um elliptische Bogen zu rendern.

Allerdings ist die Form einer quadratischen Kurve der Bézier nicht elliptischen, deshalb mehrere quadratischen Béziers erforderlich sind, um einen elliptischen Bogen zu ermitteln. Die quadratische Bézier wird stattdessen ein Segment einer Parabola.

## <a name="the-conic-bzier-curve"></a>Die konischen Bézierkurve

Die konischen Bézierkurve &mdash; auch bekannt als die rationale quadratischen Bézierkurve &mdash; ist eine relativ neue Erweiterung der Bézierkurven-Familie. Wie die quadratische Bézierkurve umfasst die rationale quadratische Bézierkurve einen Startpunkt, einen Endpunkt und Steuerungspunkts für das ein. Die rationale quadratische Bézierkurve erfordert jedoch auch eine *Gewichtung* Wert. Es heißt eine *rational* quadratische, da die parametrischen Formeln Verhältnisse beinhalten.

Die parametrischen Formeln für die X- und Y Verhältnisse sind, die den gleichen Nenner gemeinsam nutzen. Hier ist die Gleichung für den Nenner für *t* im Bereich von 0 auf 1 und einen Gewichtungswert von *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

In der Theorie eine rationale Quadratic kann drei separate Gewichtung-Werte, eine für jede der drei Begriffe enthalten, umfassen, aber diese nur eine Gewichtung Wert nach dem Begriff mittleren vereinfacht werden.

Die parametrischen Formeln für die X- und Y-Koordinaten ähneln den parametrischen Formeln für die quadratische Bézier mit dem Unterschied, dass der mittleren Begriff außerdem den Wert der Stärke enthält und der Ausdruck wird durch den Nenner dividiert:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) X₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) Y₁ + t²y₂)) ÷ d(t)

Rationale quadratische Bézierkurven werden auch als bezeichnet *Conics* , da sie Segmente einem Conic Abschnitt genau darstellen können &mdash; Hyperbeln, Parabeln, Ellipsen und Kreisen.

Um eine rationale quadratischen Bézierkurve auf einen Pfad hinzuzufügen, verwenden die [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) Methode oder der [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) Überladung mit separaten `x` und `y` Koordinaten:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Beachten Sie, dass die endgültige `weight` Parameter.

Die **konischen Kurve** Seite ermöglicht Ihnen das Experimentieren mit diese Kurven. Die `ConicCurvePage`-Klasse wird von `InteractivePage` abgeleitet. Die [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) Datei instanziiert ein `Slider` einen Gewichtungswert von – 2 bis 2 auswählen. Die [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) Code-Behind-Datei erstellt drei `TouchPoint` Objekte, und die `PaintSurface` Handler einfach rendert die resultierende Kurve mit die Tangenten Zeilen, die das Steuerelement Punkte:

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

[![](beziers-images/coniccurve-small.png "Dreifacher Screenshot der Seite konischen Kurve")](beziers-images/coniccurve-large.png#lightbox "dreifachen Screenshot der Seite konischen Kurve")

Wie Sie sehen können, scheint der Kontrollpunkt die Kurve für es abrufen, wenn das Gewicht größer ist. Wenn die Gewichtung auf 0 (null) ist, wird die Kurve eine gerade Linie von den Anfangspunkt, an den Endpunkt an.

In der Theorie negative Gewichtungen zulässig ist, und führen Sie die Kurve Biegen *sofort* aus der Kontrollpunkt. Allerdings von – 1 oder unter Ursache Nenner der parametrischen Formeln, die für bestimmte Werte negativ werden gewichtet *t*. Aus diesem Grund werden negative Gewichtungen wahrscheinlich im ignoriert die `ConicTo` Methoden. Die **konischen Kurve** Programm können Sie die negative Gewichtungen festgelegt, aber wie Sie durch Experimentieren sehen können, negative Gewichtungen haben dieselbe Wirkung wie eine Gewichtung von 0 (null), und dazu führen, dass eine gerade Linie gerendert werden soll.

Es ist sehr einfach, den Kontrollpunkt und die zu verwendende seitengewichtung, leiten die `ConicTo` -Methode zum Zeichnen von bis zu ein Kreisbogensegment (aber nicht einschließlich) ein Halbkreis. Im folgenden Diagramm erfüllen Tangenten aus der Start- und Endpunkt am Steuerungspunkts für das.

![](beziers-images/conicarc.png "Eine Darstellung konischen Bogen ein Kreisbogensegment")

Sie können trigonometrische verwenden, um zu bestimmen, die Entfernung des Steuerungspunkts für das von den Mittelpunkt: den Radius des Kreises geteilt durch den Kosinus der Hälfte der Winkel α ist. Um einen Kreisbogen zwischen den Start- und Endpunkt zu zeichnen, legen Sie die Gewichtung dieses gleiche Kosinus der Hälfte der Winkel aus. Beachten Sie, dass wenn der Winkel um 180 Grad ist, klicken Sie dann die Tangenten nie erfüllen und die Gewichtung ist 0 (null). Aber für Winkel ist kleiner als 180 Grad, wird die mathematischen Grundlagen von gut funktioniert.

Die **konischen Kreisbogen** Seite veranschaulicht dies. Die [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) Datei instanziiert ein `Slider` für die Auswahl des Winkels. Die `PaintSurface` Ereignishandler in der [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) Code-Behind-Datei wird berechnet, der Kontrollpunkt und die Gewichtung:

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

Wie Sie sehen können, besteht keine visuellen Unterschied zwischen der `ConicTo` in Rot gezeigten Pfad und den zugrunde liegenden Kreis zu Referenzzwecken angezeigt:

[![](beziers-images/coniccirculararc-small.png "Dreifacher Screenshot der Seite konischen Kreisbogen")](beziers-images/coniccirculararc-large.png#lightbox "dreifachen Screenshot der Seite konischen Kreisbogensegment verwendet")

Aber legen Sie den Winkel um 180 Grad und der Mathematik fehlschlägt.

Es ist in diesem Fall unglücklich, `ConicTo` negative Gewichtungen nicht unterstützt, da in der Theorie (basierend auf den parametrischen Formeln) der Kreis mit einem anderen Aufruf abgeschlossen werden kann `ConicTo` mit denselben Punkten, aber einen negativen Wert der Gewichtung. Dies würde ermöglichen, erstellen einen gesamten Kreis mit nur zwei `ConicTo` Kurven basierend auf einen beliebigen Winkel zwischen (aber nicht einschließlich) Null Grad und 180 Grad.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
