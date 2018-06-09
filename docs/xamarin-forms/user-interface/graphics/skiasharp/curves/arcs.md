---
title: Drei Möglichkeiten, einen Bogen gezeichnet werden soll.
description: In diesem Artikel wird erläutert, wie mit SkiaSharp Bögen auf drei verschiedene Arten definieren, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 9e0ed04543436ec7a83d13fa6a56637fc7916338
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244151"
---
# <a name="three-ways-to-draw-an-arc"></a>Drei Möglichkeiten, einen Bogen gezeichnet werden soll.

_Erfahren Sie, wie mit SkiaSharp Bögen auf drei verschiedene Arten definieren_

Ein Bogen ist eine Kurve, die auf den Umfang einer Ellipse, z. B. die abgerundeten Teile dieses Infinity-Zeichen:

![](arcs-images/arcsample.png "Infinity-Zeichen")

Trotz der Einfachheit dieser Definition, besteht keine Möglichkeit, eine Bogen zeichnen-Funktion zu definieren, die allen Ansprüchen erfüllt und daher keine Übereinstimmung zwischen Grafiksysteme die beste Möglichkeit, einen Bogen gezeichnet werden soll. Aus diesem Grund die `SKPath` Klasse schränkt selbst nicht auf nur einem Ansatz.

`SKPath` definiert eine `AddArc` -Methode, die fünf verschiedenen `ArcTo` Methoden und zwei relativen `RArcTo` Methoden. Diese Methoden fallen in drei Kategorien, die drei sehr unterschiedliche Ansätze zur Angabe des Bogens darstellt. Welche Sie verwenden hängt von Informationen zum Definieren des Bogens und wie diese Bogen sich mit den anderen Grafiken passt, die Sie zeichnen verfügbar sind.

## <a name="the-angle-arc"></a>Der Winkel Bogen

Der Winkel Bogen-Ansatz zum Zeichnen von Bögen erfordert die Angabe eines Rechtecks, das eine Ellipse umschließt. Der Bogen auf den Umfang der Ellipse wird durch Winkel aus der Mitte der Ellipse, wodurch der Beginn des Bogens und seine Länge angegeben. Zwei verschiedene Installationsmethoden Winkel Bögen zu zeichnen. Dies sind die [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Methode und die [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) Methode:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Diese Methoden sind identisch mit der Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) und [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) Methoden. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) Methode gleicht aber auf Bögen auf den Umfang eines Kreises beschränkt ist, anstatt in eine Ellipse generalisiert.

Beide Methoden beginnen mit einem `SKRect` -Wert, der den Speicherort und die Größe einer Ellipse definiert:

![](arcs-images/anglearcoval.png "Die Ellipse, die einen Winkel Bogen beginnt.")

Der Bogen ist ein Teil der bei der Ellipse.

Die `startAngle` Argument ist ein gegen den Uhrzeigersinn Winkel in Grad relativ zu einer horizontalen Linie von der Mitte der Ellipse auf der rechten Seite gezeichnet. Die `sweepAngle` Argument ist relativ zu den `startAngle`. Hier werden `startAngle` und `sweepAngle` Werte von 60 bis 100 Grad bzw.:

![](arcs-images/anglearcangles.png "Der Winkel, die einen Winkel Bogen definieren")

Der Bogen beginnt bei der Startwinkel. Der mittelpunktswinkel unterliegt seine Länge:

![](arcs-images/anglearchighlight.png "Der Bogen hervorgehobene Spitzen")

Die Kurve wird hinzugefügt, um den Pfad mit der `AddArc` oder `ArcTo` Methode ist einfach, dass ein Teil der Ellipse Kreisumfangs, hier in Rot dargestellt:

![](arcs-images/anglearc.png "Der Winkel Bogen allein")

Die `startAngle` oder `sweepAngle` Argumente können negativ sein: der Bogen wird gegen den Uhrzeigersinn für positive Werte von `sweepAngle` und für negative Werte gegen den Uhrzeigersinn.

Allerdings `AddArc` ist *nicht* definieren eine geschlossene Kontur. Beim Aufrufen `LineTo` nach `AddArc`, eine Zeile am Ende der Bogen gezeichnet wird, zum Zeitpunkt der `LineTo` -Methode und die gleiche ist "true", der `ArcTo`.

`AddArc` Startet eine neue Kontur und ist funktionell gleichwertig mit einem Aufruf von automatisch `ArcTo` mit einem endgültigen Argument der `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Letzte Argument aufgerufen wird `forceMoveTo`, und er effektiv bewirkt, dass eine `MoveTo` rufen Sie am Anfang des Bogens. Beginnt eine neue Kontur. Das nicht der Fall ist mit einer letzten Arguments der `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Diese Version des `ArcTo` zeichnet eine Linie von der aktuellen Position, an den Anfang des Bogens. Dies bedeutet, dass das Kreissegment an einer beliebigen Stelle in der Mitte einer größeren Kontur sein kann.

Die **Winkel Bogen** Seite können Sie zwei Schieberegler Geben Sie die Start- und sweep Winkel. Die XAML-Datei instanziiert zwei `Slider` Elemente und ein `SKCanvasView`. Die `PaintCanvas` Ereignishandler in der [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) Datei zeichnet das Oval und den Bogen mit zwei `SKPaint` Objekte als Felder definiert:

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

Wie Sie sehen können, können der Startwinkel und den mittelpunktswinkel negative Werte annehmen:

[![](arcs-images/anglearc-small.png "Dreifacher Screenshot der Seite Winkel Bogen")](arcs-images/anglearc-large.png#lightbox "dreifacher Screenshot der der Winkel Bogen-Seite")

Dieser Ansatz zum Generieren von einen Bogen algorithmisch am einfachsten ist, und es ist einfach, die parametrischen Gleichungen abgeleitet werden, die den Bogen beschreiben. Zu wissen, die Größe und Position des der Ellipse, die Start- und Sweep Winkel, können die Start- und Endpunkte des Bogens berechnet werden mithilfe der einfachen Trigonometrie (trigonometry):

X = Ellipse. MidX + (Ellipse. Breite / 2) * cos(angle)

y = Ellipse. MidY + (Ellipse. Höhe / 2) * sin(angle)

Die `angle` Wert lautet entweder `startAngle` oder `startAngle + sweepAngle`.

Die Verwendung von zwei Winkel definieren einen Bogen ist für Fälle, in dem Sie die angular Länge des Bogens bekannt sein, z. B. gezeichnet werden soll, um ein Kreisdiagramm, am besten geeignet. Die **Kreisdiagramm explodiert** veranschaulicht dies. Die [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Klasse verwendet eine interne Klasse, um einige Daten erstellt wurde und die Farben definiert:

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

Die `PaintSurface` Handler durchläuft die Elemente berechnet zuerst eine `totalValues` Anzahl. Aus, die sie Größe des Elements als der Anteil der insgesamt bestimmen, und zu konvertieren, um einen Winkel:

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

Ein neues `SKPath` Objekt für jeden kreisslice erstellt wird. Der Pfad besteht aus einer Zeile aus der Mitte ein `ArcTo` des Bogens und eine andere Zeile zurück an die Ergebnisse Center gezeichnet werden soll die `Close` aufrufen. Dieses Programm zeigt "explodierten" Kreissegmente durch Verschieben aller out aus der Mitte von 50 Pixel. Diese Aufgabe erfordert einen Vektor in Richtung der Mitte des der mittelpunktswinkel für jedes Segment:

[![](arcs-images/explodedpiechart-small.png "Dreifacher Screenshot der Seite Kreisdiagramm explodiert")](arcs-images/explodedpiechart-large.png#lightbox "dreifacher Screenshot der Seite Kreisdiagramm explodiert")

Um anzuzeigen, wie es ohne die "Auflösung" aussieht, einfach kommentieren Sie Sie aus der `Translate` aufrufen:

[![](arcs-images/explodedpiechartunexploded-small.png "Dreifacher Screenshot der Seite Kreisdiagramm explodiert ohne der Explosion")](arcs-images/explodedpiechartunexploded-large.png#lightbox "dreifacher Screenshot der Seite Kreisdiagramm explodiert ohne die Auflösung")

## <a name="the-tangent-arc"></a>Der Bogen Tangenten

Der zweite Typ der Bogen von unterstützten `SKPath` ist die *Tangenten Bogen*, also aufgerufen werden, da der Bogen der Umfang eines Kreises befindet, die zwei verbundene Linien Tangens ist.

Ein Tangenten Bogen wird in einen Pfad hinzugefügt, durch einen Aufruf der [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) Methode mit zwei `SKPoint` Parameter, oder die [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) Überladung mit separaten `Single` Parameter für die Punkte:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Dies `ArcTo` Methode ist vergleichbar mit der PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (Seite 532 in das PDF-Dokument)-Funktion und den iOS- [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) Methode.

Die `ArcTo` Methode umfasst drei Punkte:

- Die aktuelle zeigen die Kontur oder den Punkt (0, 0), wenn `MoveTo` nicht aufgerufen wurde
- Das erste Argument der Punkt für die `ArcTo` Methode, mit dem Namen der *Ecke Punkt*
- Der zweite Punkt Argument `ArcTo`Namens der *Zielpunkt*:

![](arcs-images/tangentarcthreepoints.png "Drei Punkte, die einen Tangenten Bogen beginnen")

Diese drei Punkte definieren zwei miteinander verbundenen Linien:

![](arcs-images/tangentarcconnectinglines.png "Zeilen, die zwischen den drei Punkten einem Tangens Bogen")

Wenn die drei Punkte kollineare sind &mdash; d. h. Wenn sie auf einer geraden Linie liegen &mdash; kein Bogen gezeichnet wird.

Die `ArcTo` -Methode enthält auch eine `radius` Parameter. Dadurch wird den Umfang eines Kreises Radius definiert:

![](arcs-images/tangentarccircle.png "Der Kreis einen Tangens Bogens")

Der Tangente Bogen ist nicht für eine Ellipse, die verallgemeinert werden.

Wenn die beiden Zeilen in einem beliebigen Winkel zu erfüllen, kann Kreises zwischen diesen Zeilen eingefügt werden, sodass Tangens auf beide Zeilen werden:

![](arcs-images/tangentarctangentcircle.png "Der Kreis Tangenten Bogen zwischen den zwei Zeilen")

Die Kurve, die die Kontur hinzugefügt wird, nicht berühren entweder den angegebenen Punkten der `ArcTo` Methode. Es besteht eine gerade Linie aus den aktuellen Punkt der ersten Tangentialpunkt und einen Bogen, der an den zweiten Tangentialpunkt beendet:

![](arcs-images/tangentarchighlight.png "Die hervorgehobenen Tangenten Bogen zwischen den zwei Zeilen")

So sieht die endgültige gerade Linie und Bogen, der die Kontur hinzugefügt wird:

![](arcs-images/tangentarc.png "Die hervorgehobenen Tangenten Bogen zwischen den zwei Zeilen")

Die Kontur kann aus der zweiten Tangentialpunkt fortgesetzt werden.

Die **Tangens Bogen** Seite ermöglicht Ihnen das Experimentieren mit den Tangens Bogen. Dies ist die erste von mehreren Seiten, die abgeleitet [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs), die ein paar praktische definiert `SKPaint` Objekte und führt `TouchPoint` verarbeiten:

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

Die `TangentArcPage`-Klasse wird von `InteractivePage` abgeleitet. Konstruktor in der [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) Datei ist verantwortlich für das Instanziieren und Initialisieren der `touchPoints` Arrays und die Einstellung `baseCanvasView` (in `InteractivePage`), die `SKCanvasView` Objekt instanziiert wird, der [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) Datei:

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

Die `PaintSurface` Handler verwendet die `ArcTo` Methode, um den Bogen zeichnen auf Grundlage der Berührungspunkte und ein `Slider`, aber auch algorithmisch berechnet den Kreis, der der Winkel basiert:

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

So sieht die **Tangens Bogen** Seite, die auf allen drei Plattformen ausgeführt:

[![](arcs-images/tangentarc-small.png "Dreifacher Screenshot der Seite Tangens Bogen")](arcs-images/tangentarc-large.png#lightbox "dreifacher Screenshot der Seite Tangens Bogen")

Der Tangente Bogen ist ideal zum Erstellen von abgerundeten Ecken, z. B. ein abgerundetes Rechteck. Da `SKPath` enthält bereits ein `AddRoundedRect` -Methode, die **gerundet Siebeneck** Seite veranschaulicht, wie `ArcTo` für die Rundung der Ecken eines Polygons sieben Seiten. (Der Code ist für alle regulären Polygon verallgemeinert.)

Die `PaintSurface` Handler, der die [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) Klasse enthält mindestens eine `for` Schleife, um die Koordinaten der sieben Scheitelpunkte des der Siebeneck und ein zweites zum Berechnen der Mittelpunkte sieben Seiten aus diesen zu berechnen Scheitelpunkte. Diese Mittelpunkte werden dann verwendet, um den Pfad zu erstellen:

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

Hier wird das Programm auf drei Plattformen ausgeführt wird:

[![](arcs-images/roundedheptagon-small.png "Dreifacher Screenshot der Seite gerundet Siebeneck")](arcs-images/roundedheptagon-large.png#lightbox "dreifacher Screenshot der Seite Siebeneck gerundet")

## <a name="the-elliptical-arc"></a>Elliptischen Bogens

Elliptischen Bogens wird zu einem Pfad hinzugefügt, mit einem Aufruf von der [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) Methode, die zwei `SKPoint` Parameter, oder die [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) -Überladung mit separaten X- und Y-Koordinaten:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Elliptischen Bogens ist konsistent mit der [elliptischen Bogens](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) (Scalable Vector Graphics, SVG) und der universellen Windows-Plattform enthalten [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) Klasse.

Diese `ArcTo` zeichnen Methoden einen Bogen zwischen zwei Punkten, die den aktuellen Stand der Kontur ist, und der letzte Parameter der `ArcTo` -Methode (die `xy` Parameter oder der separaten `x` und `y` Parameter):

![](arcs-images/ellipticalarcpoints.png "Die beiden Punkte, die einen elliptischen Bogen definiert.")

Der erste Punktparameter für die `ArcTo` Methode (`r`, oder `rx` und `ry`) ist kein Punkt überhaupt jedoch stattdessen gibt die horizontalen und vertikalen Radien einer Ellipse;

![](arcs-images/ellipticalarcellipse.png "Die Ellipse, die einen elliptischen Bogen definiert.")

Die `xAxisRotate` Parameter entspricht der Anzahl der Grad im Uhrzeigersinn um diese Ellipse drehen:

![](arcs-images/ellipticalarctiltedellipse.png "Geneigter Ellipse, die einen elliptischen Bogen definiert.")

Wenn diese Geneigter Ellipse dann so, dass es sich um die beiden Punkte berührt positioniert ist, sind die Punkte durch zwei unterschiedliche Bögen verbunden:

![](arcs-images/ellipticalarcellipse1.png "Der erste Satz von elliptischen Bogens")

Diese zwei Bögen können auf zwei Arten unterschieden werden: der oberste Bogen ist größer als die unteren Bogen und wie von links nach rechts der Bogen gezeichnet wird, wird der oberste Bogen im Uhrzeigersinn gezeichnet, während der unteren Bogen in entgegen dem Uhrzeigersinn gezeichnet wird.

Es ist auch möglich, das die Ellipse zwischen den zwei Punkten auf andere Weise anpassen:

![](arcs-images/ellipticalarcellipse2.png "Die zweite Gruppe von elliptischen Bogens")

Nun besteht eine kleinere Bogen an erster Stelle, die im Uhrzeigersinn gezeichnet wird, und ein größeres Bogen im unteren Bereich, der gezeichnet wird gegen den Uhrzeigersinn.

Diese beiden Punkte können daher über einen Bogen, der durch die Geneigter Ellipse in insgesamt vier Arten definiert verbunden werden:

![](arcs-images/ellipticalarccolors.png "Alle vier elliptischen Bogen")

Diese vier Bögen unterscheiden sich durch die vier Kombinationen der [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) und [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) Typargumente für die Enumeration der `ArcTo` Methode:

- Rot: SKPathArcSize.Large und SKPathDirection.Clockwise
- Grün: SKPathArcSize.Small und SKPathDirection.Clockwise
- Blau: SKPathArcSize.Small und SKPathDirection.CounterClockwise
- Magenta: SKPathArcSize.Large und SKPathDirection.CounterClockwise

Ist die Geneigter Ellipse nicht groß genug, um zwischen den beiden Punkten entsprechen, wird es gleichmäßig skaliert bis er groß genug ist. Nur zwei eindeutige Bögen die beiden Punkte in diesem Fall eine Verbindung herstellen. Diese unterschieden werden können, mit der `SKPathDirection` Parameter.

Obwohl diese Verfahren zur Definition eines Bogens komplexe ersten Auftreten vorkommt, ist der einzige Ansatz, der ermöglicht, einen Bogen mit eine gedrehte Ellipse definiert, und es ist häufig der einfachste Ansatz, wenn Sie andere Teile der Kontur Bögen integrieren müssen.

Die **elliptischen Bogens** Seite können Sie die beiden Punkte, und die Größe und Rotation der Ellipse interaktiv festgelegt. Die `EllipticalArcPage` Klasse abgeleitet `InteractivePage`, und die `PaintSurface` Ereignishandler in der [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) Code-Behind-Datei, zeichnet die vier Bögen:

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

Hier ist er auf den drei Plattformen ausgeführt:

[![](arcs-images/ellipticalarc-small.png "Dreifacher Screenshot der Seite elliptischen Bogens")](arcs-images/ellipticalarc-large.png#lightbox "dreifacher Screenshot der Seite elliptischen Bogens")

Die **Bogen unendlich** Seite verwendet elliptischen Bogens einem Vorzeichen Unendlich gezeichnet werden soll. Die Vorzeichen unendlich basiert auf zwei Kreise mit Radii von 100 Einheiten von 100 Einheiten getrennt:

![](arcs-images/infinitycircles.png "Zwei Kreise")

Zwei Zeilen miteinander ausgeschöpfte sind Tangens auf beide Kreise:

![](arcs-images/infinitycircleslines.png "Zwei Kreise mit Tangenten")

Die Vorzeichen unendlich ist eine Kombination der Teile des diese Kreise und zwei Zeilen. Um elliptischen Bogens verwenden, um das Vorzeichen Unendlich gezeichnet werden soll, müssen die Koordinaten, in denen die beiden Zeilen Tangens auf die Kreise sind, bestimmt werden.

Erstellen Sie einen rechten Rechteck in einem Kreise:

![](arcs-images/infinitytriangle.png "Zwei Kreise Tangenten mit eingebetteten Kreis")

Der Radius des Kreises ist 100 Einheiten, und Hypotenuse des Dreiecks 150 Einheiten, daher ist der Winkel α den Arkussinus (Arkussinus) von 100, 150 oder 41.8 Grad unterteilt. Die Länge der anderen Seite des Dreiecks ist 150 Zeiten den Kosinus des 41.8 Grad oder 112, die auch von den Satz des Pythagoras berechnet werden können.

Mithilfe dieser Informationen können dann die Koordinaten des Punkts Tangenten berechnet werden:

X = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Die vier Tangentenpunkten sind alle, die zum Zeichnen von einem unendlich Vorzeichen zentriert auf den Punkt (0, 0) mit Kreis Radien 100 erforderlich ist:

![](arcs-images/infinitycoordinates.png "Zwei Kreise Tangenten mit Koordinaten")

Die `PaintSurface` Ereignishandler in der [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) Klasse positioniert die Infinity-Zeichen, damit die (0, 0) befindet sich in der Mitte der Seite und den Pfad zu der Bildschirmgröße skaliert:

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

Der Code verwendet die `Bounds` Eigenschaft `SKPath` um zu bestimmen, die Dimensionen von den Sinus unendlich, um auf die Größe des Zeichenbereichs zu skalieren:

[![](arcs-images/arcinfinity-small.png "Dreifacher Screenshot der Seite Bogen unendlich")](arcs-images/arcinfinity-large.png#lightbox "dreifacher Screenshot der Seite Bogen unendlich")

Es erscheint das Ergebnis ein wenig "klein", legt nahe, dass die `Bounds` Eigenschaft `SKPath` meldet einen Wert größer als der Pfad.

Die ungefähre Skia intern des Bogens mit mehreren quadratische Bézier-Kurven. Diese Kurven, (wie Sie im nächsten Abschnitt sehen) Steuerpunkte, die steuern, wie die Kurve gezeichnet wird, jedoch sind nicht Teil der gerenderten Kurve enthalten. Die `Bounds` Eigenschaft enthält die Steuerpunkte.

Um eine engere Anpassung zu erhalten, verwenden die `TightBounds` -Eigenschaft, der die Steuerpunkte ausgeschlossen sind. Hier wird das Programm ausführen im Querformat, und die `TightBounds` Eigenschaft, um die Grenzen der Pfad zu erhalten:

[![](arcs-images/arcinfinitytightbounds-small.png "Dreifacher Screenshot der Seite Bogen unendlich mit enge Grenzen")](arcs-images/arcinfinitytightbounds-large.png#lightbox "dreifacher Screenshot der Seite Bogen unendlich mit enge Grenzen")

Obwohl die Verbindungen zwischen dem Bögen und gerade Linien mathematisch smooth sind, scheinen die Änderung von Bogen gerade Linie etwas abrupten. Eine bessere Infinity-Anmeldung wird in der nächsten Seite angezeigt.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
