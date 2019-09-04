---
title: Drei Möglichkeiten, einen Bogen zu zeichnen
description: In diesem Artikel wird erläutert, wie SkiaSharp verwenden, um Bögen auf drei verschiedene Arten zu definieren, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
ms.openlocfilehash: ada7ce8fc9365ab4133ddf439353e97e640f39d6
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228115"
---
# <a name="three-ways-to-draw-an-arc"></a>Drei Möglichkeiten, einen Bogen zu zeichnen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie SkiaSharp zu verwenden, um Bögen auf drei verschiedene Arten zu definieren._

Ein Bogen ist eine Kurve auf den Umfang einer Ellipse, z. B. die abgerundeten Teile dieses Infinity-Zeichen:

![Unendlichkeitszeichen](arcs-images/arcsample.png)

Trotz der Einfachheit dieser Definition, es gibt keine Möglichkeit, eine Bogen-Zeichnung-Funktion definiert, die jeden Anlass, erfüllt und daher keine Konsens Grafiksysteme die beste Möglichkeit, einen Bogen zu zeichnen. Aus diesem Grund die `SKPath` Klasse beschränkt sich nicht auf nur einen Ansatz.

`SKPath` definiert eine [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc*) -Methode, die fünf verschiedenen [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo*) Methoden und zwei relativ [ `RArcTo` ](xref:SkiaSharp.SKPath.RArcTo*) Methoden. Diese Methoden werden in drei Kategorien, die drei sehr Ansätze zur Angabe eines Bogens darstellt. Welche Sie verwenden hängt von der verfügbaren Informationen zu den Bogen mit Strichen und darüber, wie diese Bogen sich mit den anderen Grafiken einfügt, die Sie zeichnen definieren.

## <a name="the-angle-arc"></a>Der Winkel Bogen

Der Winkel Bogen Ansatz zum Zeichnen von Bögen erfordert die Angabe eines Rechtecks, das eine Ellipse umschließt. Der Bogen in der Umfang dieser Ellipse wird durch Winkel aus der Mitte der Ellipse angegeben, die den Anfang des Bogens und seine Länge angeben. Zwei verschiedene Installationsmethoden Winkel Bögen zu zeichnen. Dies sind die [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) Methode und die [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean)) Methode:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Diese Methoden sind identisch mit den Methoden [`AddArc`](xref:Android.Graphics.Path.AddArc*) Android und`ArcTo`[] Xref: Android. Graphics. Path. ArcTo *). IOS [ `AddArc` ](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean)) Methode ähnelt aber auf Bögen auf den Umfang eines Kreises beschränkt ist, anstatt in eine Ellipse generalisiert.

Beide Methoden beginnen mit einem `SKRect` -Wert, der Speicherort und die Größe einer Ellipse definiert:

![Das Oval, das einen Winkel Bogen startet.](arcs-images/anglearcoval.png)

Der Bogen ist ein Teil der Umfang dieser Ellipse.

Die `startAngle` Argument ist, einen Winkel im Uhrzeigersinn in Grad relativ zu einer horizontalen Linie, die von der Mitte der Ellipse gezeichnet wird, auf der rechten Seite. Die `sweepAngle` Argument ist relativ zu den `startAngle`. Hier sind `startAngle` und `sweepAngle` bzw. Werte von 60 Grad und 100 Grad:

![Die Winkel, die einen Winkel Bogen definieren.](arcs-images/anglearcangles.png)

Der Startwinkel beginnt des Bogens. Der mittelpunktswinkel unterliegt längenbeschränkungen. Der Bogen wird in Rot hier gezeigt:

![Der hervorgehobene Winkel Bogen](arcs-images/anglearchighlight.png)

Die Kurve, die hinzugefügt werden, um den Pfad mit der `AddArc` oder `ArcTo` Methode ist einfach, Teil der Ellipse Umfang:

![Der Winkel Bogen allein](arcs-images/anglearc.png)

Das `startAngle` - `sweepAngle` Argument oder das-Argument kann negativ sein: Der Bogen steht im Uhrzeigersinn für positive `sweepAngle` Werte von und gegen den Uhrzeigersinn für negative Werte.

Allerdings `AddArc` ist *nicht* definieren eine geschlossene Kontur. Wenn Sie aufrufen `LineTo` nach `AddArc`, eine Zeile am Ende der Bogen gezeichnet wird, zu dem Punkt in der `LineTo` -Methode, und dasselbe gilt für `ArcTo`.

`AddArc` automatisch startet eine neue Kontur und die ist funktionell gleichwertig mit einem Aufruf von `ArcTo` mit einem letzten Argument vom `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Dass das letzte Argument aufgerufen wird, `forceMoveTo`, und es effektiv bewirkt, dass eine `MoveTo` rufen Sie am Anfang des Bogens. Beginnt eine neue Kontur. Ist dies nicht der Fall mit einem letzten Argument der `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Diese Version von `ArcTo` zeichnet eine Linie von der aktuellen Position, an den Anfang des Bogens. Dies bedeutet, dass das Kreissegment an einer beliebigen Stelle in der Mitte einer größeren Contour global sein kann.

Die **Winkel Bogen** Seite können Sie zwei Schiebereglern Geben Sie die Start- und beim Aufräumen bearbeitete Winkel. Die XAML-Datei instanziiert zwei `Slider` Elemente und ein `SKCanvasView`. Die `PaintCanvas` Ereignishandler in der [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) Datei zeichnet, sowohl für die "-Oval" als auch für den Bogen mit zwei `SKPaint` Objekte als Felder definiert:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Wie Sie sehen können, können sowohl den Startwinkel und den mittelpunktswinkel negative Werte annehmen:

[![Dreifacher Screenshot der Seite "Winkel Bogen"](arcs-images/anglearc-small.png)](arcs-images/anglearc-large.png#lightbox)

Diese Vorgehensweise zum Erstellen von einen Bogen algorithmisch am einfachsten ist, und es ist einfach, die parametrischen Formeln abgeleitet werden, die den Bogen mit Strichen zu beschreiben. Kennen die Größe und Position des der Ellipse, die Start- und Sweep Winkel, kann die Start- und Endpunkt des Bogens berechnet werden mithilfe von einfachen trigonometrische:

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

Die `angle` Wert lautet entweder `startAngle` oder `startAngle + sweepAngle`.

Für Fälle, in dem Sie die angular-Länge des Bogens bekannt sein, z. B. für Draw-, um ein Kreisdiagramm, empfiehlt sich die Verwendung der beiden Winkel um einen Bogen zu definieren. Die **Kreisdiagramm explodiert** Seite veranschaulicht dies. Die [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Klasse verwendet eine interne Klasse, um einige erstellte Daten und die Farben zu definieren:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

Die `PaintSurface` Handler durchläuft die Elemente, berechnen Sie zuerst eine `totalValues` Anzahl. An diesem kann die Größe des Elements als der Anteil der gesamten bestimmen und zu konvertieren, einen Winkel:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Ein neues `SKPath` -Objekt wird erstellt für jeden kreisslice angezeigt. Der Pfad besteht aus einer Zeile aus dem Center ein `ArcTo` den Bogen mit Strichen und eine neue Zeile zurück an die Center-Ergebnisse aus gezeichnet der `Close` aufrufen. Dieses Programm zeigt "explodierten" Kreissegmente durch Verschieben all aus dem Center 50 Pixel. Diese Aufgabe erfordert einen Vektor in Richtung der Mitte des der mittelpunktswinkel für jeden Slice:

[![Dreifacher Screenshot der Seite "explodiertes Kreis Diagramm"](arcs-images/explodedpiechart-small.png)](arcs-images/explodedpiechart-large.png#lightbox)

Um anzuzeigen, ohne die "Auflösung" sieht es, einfach Auskommentieren der `Translate` aufrufen:

[![Dreifacher Screenshot der explodierten Kreis Diagramm Seite ohne die Explosion](arcs-images/explodedpiechartunexploded-small.png)](arcs-images/explodedpiechartunexploded-large.png#lightbox)

## <a name="the-tangent-arc"></a>Der Tangens Bogen

Der zweite Typ des Bogens von unterstützten `SKPath` ist die *Tangens Bogen*, daraus, dass der Bogen der Umfang eines Kreises ist, der tangential zur zwei verbundene Linien.

Ein Tangens Bogen wird in einen Pfad hinzugefügt, mit einem Aufruf von der [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) Methode mit zwei `SKPoint` Parameter oder die [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single)) Überladung mit separaten `Single` Parameter für die Punkte:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Dies `ArcTo` Methode ähnelt der PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (Seite 532)-Funktion und den iOS- [ `AddArcToPoint` ](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat)) Methode.

Die `ArcTo` Methode umfasst die drei Punkte:

- Die aktuelle zeigen die Kontur oder den Punkt (0, 0), wenn `MoveTo` nicht aufgerufen wurde
- Das erste Argument der Punkt für die `ArcTo` aufgerufene Methode die *Ecke zeigen*
- Das zweite Argument der Punkt für `ArcTo`Namens der *Zielpunkt*:

![Drei Punkte, die einen Tangens Bogen beginnen](arcs-images/tangentarcthreepoints.png)

Diese drei Punkte definieren zwei verbundene Linien:

![Zeilen, die die drei Punkte eines Tangens Bogens verbinden](arcs-images/tangentarcconnectinglines.png)

Wenn die drei Punkte kollineare sind &mdash; , also wenn sie auf einer geraden Linie liegen &mdash; kein Bogen gezeichnet.

Die `ArcTo` -Methode enthält auch eine `radius` Parameter. Den Radius eines Kreises definiert:

![Der Kreis eines Tangens Bogens.](arcs-images/tangentarccircle.png)

Der Tangente Bogen ist nicht für eine Ellipse generalisiert werden.

Wenn die beiden Zeilen in einem beliebigen Winkel erfüllen, kann diese Kreis zwischen diesen Zeilen eingefügt werden, damit es tangential zur beide Zeilen ist:

![Der Tangens Bogen Kreis zwischen den beiden Linien.](arcs-images/tangentarctangentcircle.png)

Die Kurve, die die Kontur hinzugefügt wird, nicht touch entweder von der Punkte in der `ArcTo` Methode. Sie besteht aus einer geraden Linie vom aktuellen Punkt der ersten tangenspunkt und einen Bogen, der an den zweiten tangenspunkt, hier in Rot dargestellt:

![Der markierte Tangens Bogen zwischen den beiden Zeilen.](arcs-images/tangentarchighlight.png)

Hier ist die endgültige gerade Linie und Bogen, der die Kontur hinzugefügt wird:

![Der markierte Tangens Bogen zwischen den beiden Zeilen.](arcs-images/tangentarc.png)

Die Kontur kann aus der zweiten tangenspunkt fortgesetzt werden.

Die **Tangens Bogen** Seite ermöglicht Ihnen das Experimentieren mit der Tangente Bogen. Dies ist die erste von mehreren Seiten, die abgeleitet [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs), die definiert, ein paar praktische `SKPaint` Objekte und führt `TouchPoint` verarbeiten:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

Die `TangentArcPage`-Klasse wird von `InteractivePage` abgeleitet. Der Konstruktor in der [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) Datei ist verantwortlich für das Instanziieren und Initialisieren der `touchPoints` Arrays und die Einstellung `baseCanvasView` (in `InteractivePage`) auf der `SKCanvasView` Objekt instanziiert wird, der [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) Datei:

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Die `PaintSurface` Ereignishandler verwendet die `ArcTo` Methode, um den Bogen mit Strichen zu zeichnen auf der Grundlage von Touch-Punkt und ein `Slider`, aber auch algorithmisch berechnet den Kreis, der der Winkel basiert:

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
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
}
```

Hier ist die **Tangens Bogen** Seite ausgeführt wird:

[![Dreifacher Screenshot der Tangens Bogen Seite](arcs-images/tangentarc-small.png)](arcs-images/tangentarc-large.png#lightbox)

Der Tangente Bogen ist ideal zum Erstellen der abgerundeten Ecken, z. B. ein abgerundetes Rechteck. Da `SKPath` enthält bereits eine `AddRoundedRect` -Methode, die **gerundet Siebeneck** Seite veranschaulicht, wie `ArcTo` zum Runden der Ecken eines Polygons Gleichseitiges. (Der Code ist für alle regulären Polygon generalisiert werden.)

Die `PaintSurface` Handler die [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) Klasse enthält eine `for` Schleife, um den Koordinaten von den sieben Vertices für die Siebeneck und ein zweites zum Berechnen der dann der sieben Seiten aus diesen zu berechnen. Eckpunkte. Diese dann werden dann verwendet, um den Pfad zu erstellen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Hier wird das Programm ausgeführt wird:

[![Dreifacher Screenshot der gerundeten heptagon-Seite](arcs-images/roundedheptagon-small.png)](arcs-images/roundedheptagon-large.png#lightbox)

## <a name="the-elliptical-arc"></a>Elliptischen Bogens

Elliptischen Bogens wurde auf einen Pfad mit einem Aufruf von der [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint)) -Methode, die zwei `SKPoint` Parameter oder die [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single)) -Überladung mit separaten X- und Y-Koordinaten:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Elliptischen Bogens ist konsistent mit der [elliptischen Bogens](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) in Scalable Vector Graphics (SVG) und die universelle Windows-Plattform enthalten [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) Klasse.

Diese `ArcTo` zeichnen Methoden einen Bogen zwischen zwei Punkten, die dem aktuellen Punkt der Kontur sind, und der letzte Parameter, die `ArcTo` Methode (die `xy` Parameter oder die separaten `x` und `y` Parameter):

![Die zwei Punkte, die einen elliptischen Bogen definiert haben](arcs-images/ellipticalarcpoints.png)

Der erste Punktparameter für die `ArcTo` Methode (`r`, oder `rx` und `ry`) ist kein Punkt überhaupt aber stattdessen gibt die horizontalen und senkrechten RADIUS einer Ellipse;

![Die Ellipse, die einen elliptischen Bogen definiert hat.](arcs-images/ellipticalarcellipse.png)

Die `xAxisRotate` Parameter entspricht der Anzahl von Grad im Uhrzeigersinn um dieser Ellipse gedreht:

![Die gekippte Ellipse, die einen elliptischen Bogen definiert hat.](arcs-images/ellipticalarctiltedellipse.png)

Wenn dieser Geneigter Ellipse dann so, dass es sich um die beiden Punkte berührt positioniert ist, sind die Punkte durch zwei unterschiedliche Bögen verbunden:

![Der erste Satz elliptischer Bögen](arcs-images/ellipticalarcellipse1.png)

Diese beiden Bögen können auf zwei Arten unterschieden werden: Der obere Bogen ist größer als der untere Bogen, und wenn der Bogen von links nach rechts gezeichnet wird, wird der obere Bogen in einer Richtung im Uhrzeigersinn gezeichnet, während der untere Bogen in einer Richtung gegen den Uhrzeigersinn gezeichnet wird.

Es ist auch möglich, das die Ellipse zwischen den beiden Punkten auf andere Weise anpassen:

![Der zweite Satz elliptischer Bögen](arcs-images/ellipticalarcellipse2.png)

Jetzt ist gibt es ein kleiner Bogen im Vordergrund, das im Uhrzeigersinn gezeichnet wird, und eine größere Bogen im unteren Bereich, der gezeichnet wird gegen den Uhrzeigersinn.

Diese beiden Punkte können durch einen Bogen, der durch die Geneigter Ellipse insgesamt vier Arten definiert daher verbunden werden:

![Alle vier Ellipsen Bögen](arcs-images/ellipticalarccolors.png)

Diese vier Bögen unterscheiden sich durch die vier Kombinationen von der [ `SKPathArcSize` ](xref:SkiaSharp.SKPathArcSize) und [ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection) Enumeration Typargumente für die `ArcTo` Methode:

- Red Skpatharcsize. Large und skpathdirection. Uhrzeigersinn
- Grünbuchs Skpatharcsize. Small und skpathdirection. Uhrzeigersinn
- blauen Skpatharcsize. Small und skpathdirection. gegen den Uhrzeigersinn
- Magenta Skpatharcsize. Large und skpathdirection. gegen den Uhrzeigersinn

Geneigter Ellipse ist nicht groß genug für zwischen den beiden Punkten, und klicken Sie dann es gleichmäßig skaliert wird, bis es groß genug ist. Nur zwei eindeutige Bögen die beiden Punkte in diesem Fall eine Verbindung herstellen. Diese unterschieden werden können, mit der `SKPathDirection` Parameter.

Obwohl dieser Ansatz für die Definition eines Bogens auf den ersten komplexen vorkommt, ist der einzige Ansatz, der ermöglicht, einen Bogen mit einer Ellipse gedreht definieren, und es ist häufig der einfachste Ansatz, wenn Sie andere Teile der Kontur Bögen integrieren möchten.

Die **elliptischen Bogens** Seite können Sie interaktiv die beiden Punkte, und die Größe und Drehung der Ellipse festzulegen. Die `EllipticalArcPage` Klasse leitet sich von `InteractivePage`, und die `PaintSurface` Ereignishandler in der [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) Code-Behind-Datei, zeichnet die vier Bogen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Hier wird ausgeführt:

[![Dreifacher Screenshot der elliptischen Bogen Seite](arcs-images/ellipticalarc-small.png)](arcs-images/ellipticalarc-large.png#lightbox)

Die **Bogen unendlich** Seite elliptischen Bogens verwendet, um eine Anmeldung unendlich zu zeichnen. Die Vorzeichen unendlich basiert auf zwei Kreise mit Radien von 100 Einheiten von 100 Einheiten getrennt:

![Zwei Kreise](arcs-images/infinitycircles.png)

Zwei überschreiten miteinander Linien sind Tangens auf beide Kreise:

![Zwei Kreise mit Tangenten Linien](arcs-images/infinitycircleslines.png)

Die Vorzeichen unendlich ist eine Kombination von Teilen von diesen Gruppen und die beiden Zeilen. Um elliptischen Bogens verwenden, die die Vorzeichen Unendlich gezeichnet werden soll, müssen die Koordinaten, wo die beiden Zeilen Tangens auf die Kreise sind, ermittelt werden.

Erstellen Sie eine richtige Rechteck in einen der Kreise:

![Zwei Kreise mit Tangenten Linien und eingebettetem Kreis](arcs-images/infinitytriangle.png)

Der Radius des Kreises ist 100 Einheiten und Hypotenuse des Dreiecks ist 150 Einheiten, daher ist der Winkel α den Arkussinus (Arkussinus) von 100 dividiert x 150 oder 41.8 Grad. Die Länge der anderen Seite des Dreiecks ist 150 Mal den Kosinus des 41.8 Grad oder 112, der auch von den Satz des Pythagoras berechnet werden kann.

Mithilfe dieser Informationen können dann die Koordinaten des Punkts Tangens berechnet werden:

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

Die vier Tangentenpunkten sind alles, die was erforderlich ist, ein unendlich Vorzeichen zentriert auf den Punkt (0, 0) mit Kreis Radien 100 gezeichnet werden soll:

![Zwei Kreise mit Tangenten Linien und Koordinaten](arcs-images/infinitycoordinates.png)

Die `PaintSurface` Ereignishandler in der [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) Klasse positioniert die Infinity-Zeichen, damit die (0, 0) ist in der Mitte der Seite positioniert und skaliert den Pfad zu die Größe des Bildschirms:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
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

Der Code verwendet die `Bounds` Eigenschaft `SKPath` um die Abmessungen des Sinus unendlich für die Skalierung der Größe des Zeichenbereichs zu bestimmen:

[![Dreifacher Screenshot der Bogen unendlich Seite](arcs-images/arcinfinity-small.png)](arcs-images/arcinfinity-large.png#lightbox)

Es erscheint das Ergebnis ein wenig klein ist, was darauf hindeutet, die die `Bounds` Eigenschaft `SKPath` meldet eine Größe, die größer als der Pfad.

Intern erfolgt eine Skia Annäherung an den Bogen mit mehreren quadratischen Bézierkurven. Diese Kurven enthalten (wie Sie im nächsten Abschnitt sehen werden), muss der Control-Punkte, die steuern, wie die Kurve gezeichnet wird, jedoch sind nicht Teil der gerenderten Kurve. Die `Bounds` Eigenschaft enthält diese Kontrollpunkte.

Um eine engere Wahl abzurufen, verwenden Sie die `TightBounds` -Eigenschaft, die die Control-Punkte ausschließt. Hier ist das Programm im Querformatmodus ausgeführt werden soll, und Verwenden der `TightBounds` Eigenschaft, um die Grenzen des Pfads zu erhalten:

[![Dreifacher Screenshot der Bogen unendlich Seite mit engen Begrenzungen](arcs-images/arcinfinitytightbounds-small.png)](arcs-images/arcinfinitytightbounds-large.png#lightbox)

Auch die Verbindungen zwischen dem Bögen und gerade Linien mathematisch smooth sind, scheinen die Änderung von Bogen gerade etwas abrupten. Ein besseren Infinity-Zeichen werden im nächsten Artikel angezeigt, auf [ **drei Typen von Bézierkurven**](beziers.md).

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
