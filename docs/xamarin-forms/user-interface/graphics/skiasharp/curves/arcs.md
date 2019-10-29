---
title: Drei Möglichkeiten, einen Bogen zu zeichnen
description: In diesem Artikel wird erläutert, wie Sie mit skiasharp Bögen auf drei verschiedene Arten definieren und dies mit Beispielcode veranschaulichen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
ms.openlocfilehash: 56c2da48c596fa35dd1a7658a38eee23c939e2fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029541"
---
# <a name="three-ways-to-draw-an-arc"></a>Drei Möglichkeiten, einen Bogen zu zeichnen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie skiasharp zum Definieren von Bögen auf drei verschiedene Arten verwenden können._

Ein Bogen ist eine Kurve des Umfangs einer Ellipse, wie z. b. die abgerundeten Teile dieses unendlichen Zeichens:

![Unendlichkeitszeichen](arcs-images/arcsample.png)

Trotz der Einfachheit dieser Definition gibt es keine Möglichkeit, eine Bogen Zeichnungs Funktion zu definieren, die alle Anforderungen erfüllt, und daher keinen Konsens zwischen den Grafiksystemen der besten Methode zum Zeichnen eines Bogens. Aus diesem Grund beschränkt sich die `SKPath`-Klasse nicht auf nur einen Ansatz.

`SKPath` definiert eine [`AddArc`](xref:SkiaSharp.SKPath.AddArc*) Methode, fünf verschiedene [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) Methoden und zwei relative [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*) Methoden. Diese Methoden werden in drei Kategorien unterteilt, die drei sehr unterschiedliche Ansätze zum Angeben eines Bogens darstellen. Welche Informationen Sie verwenden, hängt von den Informationen ab, die zum Definieren des Bogens verfügbar sind, und wie dieser Bogen in die anderen Grafiken passt, die Sie zeichnen.

## <a name="the-angle-arc"></a>Der Winkel Bogen

Der Winkel Bogen Ansatz zum Zeichnen von Bögen erfordert, dass Sie ein Rechteck angeben, das eine Ellipse umschließt. Der Bogen im Umfang dieser Ellipse wird durch Winkel aus der Mitte der Ellipse angegeben, die den Anfang des Bogens und seine Länge angeben. Zwei verschiedene Methoden zeichnen Winkel Arcs. Dies sind die [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) -Methode und die [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean)) -Methode:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Diese Methoden sind identisch mit den Methoden Android [`AddArc`](xref:Android.Graphics.Path.AddArc*) und [`ArcTo`] Xref: Android. Graphics. Path. ArcTo *). Die IOS- [`AddArc`](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean)) -Methode ist ähnlich, aber auf Bögen im Umkreis eines Kreises beschränkt und nicht auf eine Ellipse verallgemeinert.

Beide Methoden beginnen mit einem `SKRect` Wert, der sowohl den Speicherort als auch die Größe einer Ellipse definiert:

![Das Oval, das einen Winkel Bogen startet.](arcs-images/anglearcoval.png)

Der Bogen ist ein Teil des Umfangs dieser Ellipse.

Das `startAngle`-Argument ist ein Winkel im Uhrzeigersinn in Grad, der relativ zu einer horizontalen Linie ist, die von der Mitte der Ellipse nach rechts gezeichnet wird. Das `sweepAngle`-Argument ist relativ zum `startAngle`. Dies sind die `startAngle`-und `sweepAngle` Werte von 60 Grad bzw. 100 Grad:

![Die Winkel, die einen Winkel Bogen definieren.](arcs-images/anglearcangles.png)

Der Bogen beginnt am Anfang des Winkels. Die Länge wird durch den Mittelpunktswinkel gesteuert. Der Bogen wird hier rot dargestellt:

![Der hervorgehobene Winkel Bogen](arcs-images/anglearchighlight.png)

Die Kurve, die dem Pfad mit der `AddArc`-oder `ArcTo`-Methode hinzugefügt wurde, ist einfach ein Teil des Umfangs der Ellipse:

![Der Winkel Bogen allein](arcs-images/anglearc.png)

Die `startAngle`-oder `sweepAngle` Argumente können negativ sein: der Bogen steht im Uhrzeigersinn für positive Werte `sweepAngle` und gegen den Uhrzeigersinn für negative Werte.

In `AddArc` wird jedoch *keine* geschlossene Kontur definiert. Wenn Sie `LineTo` nach `AddArc`aufzurufen, wird eine Linie vom Ende des Bogens bis zum Punkt in der `LineTo`-Methode gezeichnet, und dasselbe gilt für `ArcTo`.

`AddArc` startet automatisch eine neue Kontur und ist funktional äquivalent zu einem `ArcTo` mit dem letzten Argument `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Das letzte Argument wird `forceMoveTo`aufgerufen und bewirkt, dass es einen `MoveTo` Aufruf am Anfang des Bogens auslöst. Dadurch wird eine neue Kontur begonnen. Dies ist nicht der Fall mit dem letzten Argument von `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Diese Version von `ArcTo` zeichnet eine Linie von der aktuellen Position bis zum Anfang des Bogens. Dies bedeutet, dass der Bogen irgendwo in der Mitte einer größeren Kontur liegen kann.

Auf der Seite " **Winkel Bogen** " können Sie zwei Schieberegler verwenden, um die Start-und Sweep-Winkel anzugeben. Die XAML-Datei instanziiert zwei `Slider` Elemente und eine `SKCanvasView`. Der `PaintCanvas` Handler in der [**AngleArcPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) -Datei zeichnet sowohl das Oval als auch den Bogen mithilfe von zwei `SKPaint` Objekten, die als Felder definiert sind:

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

Wie Sie sehen können, können sowohl der Start Winkel als auch der Mittelpunktswinkel negative Werte annehmen:

[![dreifacher Screenshot der Seite "Winkel Bogen"](arcs-images/anglearc-small.png)](arcs-images/anglearc-large.png#lightbox)

Diese Methode zum Erstellen eines Bogens ist algorithmisch, und es ist einfach, die parametrischen Gleichungen abzuleiten, die den Bogen beschreiben. Wenn Sie die Größe und Position der Ellipse und die Start-und Sweep-Winkel kennen, können die Start-und Endpunkte des Bogens mithilfe von einfachem Trigonometry berechnet werden:

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

Der `angle` Wert ist entweder `startAngle` oder `startAngle + sweepAngle`.

Die Verwendung von zwei Winkeln zum Definieren eines Bogens eignet sich am besten für Fälle, in denen Sie die Angular-Länge des Bogens kennen, der gezeichnet werden soll, z. b. um ein Kreis Diagramm zu erstellen. Dies wird auf der Seite **explodiertes Kreis Diagramm** veranschaulicht. Die [`ExplodedPieChartPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) -Klasse verwendet eine interne Klasse zum Definieren von fabrizierten Daten und Farben:

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

Der `PaintSurface` Handler durchläuft zuerst die Elemente, um eine `totalValues` Zahl zu berechnen. Dadurch kann die Größe jedes Elements als Bruchteil des Gesamtwerts ermittelt und in einen Winkel konvertiert werden:

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

Ein neues `SKPath`-Objekt wird für jeden Kreis Slice erstellt. Der Pfad besteht aus einer Zeile aus der Mitte, einem `ArcTo` zum Zeichnen des Bogens, und eine weitere Zeile zurück zum Mittelpunkt ergibt sich aus dem `Close`-Befehl. Dieses Programm zeigt "explodierte" Kreis Scheiben an, indem Sie alle von der Mitte um 50 Pixel verschoben werden. Diese Aufgabe erfordert einen Vektor in der Richtung des Mittelpunkts des Sweep-Winkels für jeden Slice:

[![dreifacher Screenshot der explodierten Kreis Diagramm Seite](arcs-images/explodedpiechart-small.png)](arcs-images/explodedpiechart-large.png#lightbox)

Um zu sehen, wie es ohne die "Explosion" aussieht, kommentieren Sie einfach den `Translate`-Befehl aus:

[![dreifacher Screenshot der explodierten Kreis Diagramm Seite ohne die Explosion](arcs-images/explodedpiechartunexploded-small.png)](arcs-images/explodedpiechartunexploded-large.png#lightbox)

## <a name="the-tangent-arc"></a>Der Tangens Bogen

Der zweite Arkus Typ, der von `SKPath` unterstützt wird, ist der *Tangens Bogen*, der aufgerufen wird, da der Bogen der Umfang eines Kreises ist, der sich auf zwei verbundene Linien Tangens.

Einem Pfad wird ein Tangens Bogen mit einem aufzurufenden [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) -Methode mit zwei `SKPoint` Parametern oder der [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single)) -Überladung mit separaten `Single`-Parametern für die Punkte hinzugefügt:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Diese `ArcTo` Methode ähnelt der Funktion PostScript [`arct`](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (Page 532) und der IOS [`AddArcToPoint`](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat)) -Methode.

Die `ArcTo`-Methode umfasst drei Punkte:

- Der aktuelle Punkt der Kontur oder der Punkt (0, 0), wenn `MoveTo` nicht aufgerufen wurde.
- Das erste Punkt Argument für die `ArcTo`-Methode, die als *Eckpunkt* bezeichnet wird.
- Das zweite Punkt Argument, das als *Zielpunkt*bezeichnet wird, `ArcTo`:

![Drei Punkte, die einen Tangens Bogen beginnen](arcs-images/tangentarcthreepoints.png)

Mit diesen drei Punkten werden zwei verbundene Zeilen definiert:

![Zeilen, die die drei Punkte eines Tangens Bogens verbinden](arcs-images/tangentarcconnectinglines.png)

Wenn die drei Punkte colinear sind &mdash; d. h., wenn Sie sich auf derselben geraden Linie befinden &mdash; wird kein Bogen gezeichnet.

Die `ArcTo`-Methode enthält auch einen `radius`-Parameter. Hiermit wird der Radius eines Kreises definiert:

![Der Kreis eines Tangens Bogens.](arcs-images/tangentarccircle.png)

Der Tangens Bogen ist für eine Ellipse nicht generalisiert.

Wenn die beiden Zeilen in einem beliebigen Winkel übereinstimmen, kann dieser Kreis zwischen diesen Linien eingefügt werden, sodass er in beide Zeilen Tangens ist:

![Der Tangens Bogen Kreis zwischen den beiden Linien.](arcs-images/tangentarctangentcircle.png)

Die Kurve, die der Kontur hinzugefügt wird, berührt keines der Punkte, die in der `ArcTo`-Methode angegeben sind. Er besteht aus einer geraden Linie zwischen dem aktuellen Punkt und dem ersten Tangens Punkt und einem Bogen, der am zweiten Tangenten Punkt endet, der hier rot dargestellt wird:

![Der markierte Tangens Bogen zwischen den beiden Zeilen.](arcs-images/tangentarchighlight.png)

Hier sehen Sie die abschließende gerade Linie und den Bogen, der der Kontur hinzugefügt wird:

![Der markierte Tangens Bogen zwischen den beiden Zeilen.](arcs-images/tangentarc.png)

Die Kontur kann vom zweiten Tangenten Punkt aus fortgesetzt werden.

Die Seite **Tangens Bogen** ermöglicht Ihnen das Experimentieren mit dem Tangens Bogen. Dies ist die erste von mehreren Seiten, die von [`InteractivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs)abgeleitet werden, der einige praktische `SKPaint` Objekte definiert und `TouchPoint` Verarbeitung ausführt:

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

Die `TangentArcPage`-Klasse wird von `InteractivePage` abgeleitet. Der Konstruktor in der [**TangentArcPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) -Datei ist für das Instanziieren und Initialisieren des `touchPoints` Arrays und das Festlegen von `baseCanvasView` (in `InteractivePage`) auf das `SKCanvasView` Objekt zuständig, das in der [**tangentarcpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) -Datei instanziiert wird:

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

Der `PaintSurface` Handler verwendet die `ArcTo`-Methode, um den Bogen basierend auf den Berührungspunkten und einem `Slider`zu zeichnen, aber auch algorithmisch den Kreis berechnet, auf dem der Winkel basiert:

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

Hier ist die **Tangens Bogen** Seite, die ausgeführt wird:

[![dreifacher Screenshot der Tangens Bogen Seite](arcs-images/tangentarc-small.png)](arcs-images/tangentarc-large.png#lightbox)

Der Tangens Bogen eignet sich ideal zum Erstellen von abgerundeten Ecken, z. b. ein abgerundetes Rechteck. Da `SKPath` bereits eine `AddRoundedRect`-Methode enthält, zeigt die **abgerundete heptagon** -Seite, wie `ArcTo` zum Runden der Ecken eines siebenseitigen Polygons verwendet werden kann. (Der Code ist für ein beliebiges reguläres Polygon generalisiert.)

Der `PaintSurface` Handler der [`RoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) Klasse enthält eine `for` Schleife, um die Koordinaten der sieben Scheitel Punkte des heptagon zu berechnen, und eine zweite, um die Mittelpunkte der sieben Seiten aus diesen Scheitel Punkten zu berechnen. Diese Mittelpunkte werden dann verwendet, um den Pfad zu erstellen:

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

Dies ist das Programm, das ausgeführt wird:

[![dreifacher Screenshot der gerundeten heptagon-Seite](arcs-images/roundedheptagon-small.png)](arcs-images/roundedheptagon-large.png#lightbox)

## <a name="the-elliptical-arc"></a>Der elliptische Bogen

Der elliptische Bogen wird einem Pfad mit einem aufzurufenden [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint)) Methode mit zwei `SKPoint` Parametern oder der [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single)) -Überladung mit separaten X-und Y-Koordinaten hinzugefügt:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Der elliptische Bogen ist mit dem [Ellipsen Bogen](https://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) konsistent, der in der universelle Windows-Plattform [`ArcSegment`](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) Klasse enthalten ist.

Diese `ArcTo` Methoden zeichnen einen Bogen zwischen zwei Punkten (dem aktuellen Punkt der Kontur) und dem letzten Parameter der `ArcTo` Methode (dem `xy`-Parameter oder den separaten `x`-und `y`-Parametern):

![Die zwei Punkte, die einen elliptischen Bogen definiert haben](arcs-images/ellipticalarcpoints.png)

Der erste Punkt Parameter für die `ArcTo` Methode (`r`oder `rx` und `ry`) ist kein Punkt, sondern gibt den horizontalen und vertikalen Radien einer Ellipse an.

![Die Ellipse, die einen elliptischen Bogen definiert hat.](arcs-images/ellipticalarcellipse.png)

Der `xAxisRotate`-Parameter ist die Anzahl der Uhrzeigersinn, um diese Ellipse zu drehen:

![Die gekippte Ellipse, die einen elliptischen Bogen definiert hat.](arcs-images/ellipticalarctiltedellipse.png)

Wenn diese schrägen Ellipse dann so positioniert wird, dass Sie die beiden Punkte berührt, werden die Punkte durch zwei verschiedene Arcs verbunden:

![Der erste Satz elliptischer Bögen](arcs-images/ellipticalarcellipse1.png)

Diese beiden Bögen können auf zwei Arten unterschieden werden: der obere Bogen ist größer als der untere Bogen, und wenn der Bogen von links nach rechts gezeichnet wird, wird der obere Bogen in einer Richtung im Uhrzeigersinn gezeichnet, während der untere Bogen gegen den Uhrzeigersinn gezeichnet wird.

Es ist auch möglich, die Ellipse zwischen den beiden Punkten auf eine andere Weise anzupassen:

![Der zweite Satz elliptischer Bögen](arcs-images/ellipticalarcellipse2.png)

Nun gibt es einen kleineren Bogen im Uhrzeigersinn und einen größeren Bogen im unteren Bereich, der gegen den Uhrzeigersinn gezeichnet wird.

Diese beiden Punkte können daher durch einen von der gekippten Ellipse definierten Bogen auf eine Gesamtzahl von vier Arten verbunden werden:

![Alle vier Ellipsen Bögen](arcs-images/ellipticalarccolors.png)

Diese vier Bögen unterscheiden sich durch die vier Kombinationen der [`SKPathArcSize`](xref:SkiaSharp.SKPathArcSize) und [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) Enumerationstyp Argumente für die `ArcTo`-Methode:

- Rot: skpatharcsize. Large und skpathdirection. Uhrzeigersinn
- Grün: skpatharcsize. Small und skpathdirection. Uhrzeigersinn
- Blue: skpatharcsize. Small und skpathdirection. gegen den Uhrzeigersinn
- Magenta: skpatharcsize. Large und skpathdirection. gegen den Uhrzeigersinn

Wenn die gekippte Ellipse nicht groß genug ist, um zwischen den beiden Punkten zu passen, wird Sie einheitlich skaliert, bis Sie groß genug ist. Nur zwei eindeutige Bögen verbinden die beiden Punkte in diesem Fall. Diese können mit dem `SKPathDirection`-Parameter unterschieden werden.

Obwohl dieser Ansatz zum Definieren eines Bogen Sounds bei der ersten Begegnung Komplex ist, ist dies der einzige Ansatz, der das Definieren eines Bogens mit einer gedrehten Ellipse ermöglicht. Dies ist häufig der einfachste Ansatz, wenn Sie Bögen in andere Teile der Kontur integrieren müssen.

Mithilfe der **Ellipsen Bogen** Seite können Sie die beiden Punkte sowie die Größe und Drehung der Ellipse interaktiv festlegen. Die `EllipticalArcPage`-Klasse wird von `InteractivePage`abgeleitet, und der `PaintSurface`-Handler in der [**EllipticalArcPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) -Code Behind-Datei zeichnet die vier Arcs:

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

[![Triple-Bildschirm Abbildung der elliptischen Bogen Seite](arcs-images/ellipticalarc-small.png)](arcs-images/ellipticalarc-large.png#lightbox)

Die Seite " **Arc Infinity** " verwendet den elliptischen Bogen, um ein Unendlichkeitszeichen zu zeichnen. Das Unendlichkeitszeichen basiert auf zwei Kreisen mit einem Radien von 100 Einheiten, die durch 100 Einheiten getrennt sind:

![Zwei Kreise](arcs-images/infinitycircles.png)

Zwei Zeilen, die einander überschreiten, sind Tangens beider Kreise:

![Zwei Kreise mit Tangenten Linien](arcs-images/infinitycircleslines.png)

Das Unendlichkeitszeichen ist eine Kombination aus Teilen dieser Kreise und den beiden Zeilen. Um den elliptischen Bogen zum Zeichnen des unendlichen Zeichens zu verwenden, müssen die Koordinaten, an denen die beiden Linien Tangens der Kreise sind, bestimmt werden.

Erstellen Sie ein rechtes Rechteck in einem der Kreise:

![Zwei Kreise mit Tangenten Linien und eingebettetem Kreis](arcs-images/infinitytriangle.png)

Der Radius des Kreises beträgt 100 Einheiten, und die Hypotenuse des Dreiecks ist 150 Einheiten, sodass der Winkel "α" der Arkus Sinus (umgekehrter Sinus) 100 dividiert durch 150 oder 41,8 Grad ist. Die Länge der anderen Seite des Dreiecks ist 150 mal der Kosinus von 41,8 Grad oder 112, der auch durch das Pythagorean-Theorem berechnet werden kann.

Die Koordinaten des Tangenten Punkts können dann anhand dieser Informationen berechnet werden:

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

Die vier Tangenten Punkte sind alles, was erforderlich ist, um ein Unendlichkeitszeichen zu zeichnen, das auf dem Punkt (0,0) mit Kreis Radien (100) zentriert ist:

![Zwei Kreise mit Tangenten Linien und Koordinaten](arcs-images/infinitycoordinates.png)

Der `PaintSurface` Handler in der [`ArcInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) -Klasse positioniert das Unendlichkeitszeichen, sodass sich der (0,0) Punkt in der Mitte der Seite befindet, und skaliert den Pfad auf die Bildschirmgröße:

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

Im Code wird die `Bounds`-Eigenschaft von `SKPath` verwendet, um die Dimensionen des unendlichen Sinus zu bestimmen, um Sie auf die Größe der Canvas zu skalieren:

[![Triple-Bildschirm Abbildung der Bogen unendlich Seite](arcs-images/arcinfinity-small.png)](arcs-images/arcinfinity-large.png#lightbox)

Das Ergebnis scheint etwas klein zu sein. Dies deutet darauf hin, dass die `Bounds`-Eigenschaft von `SKPath` eine größere Größe als der Pfad meldet.

Intern gleicht die Skia den Bogen mithilfe mehrerer quadratischer Bézier-Kurven. Diese Kurven (wie Sie im nächsten Abschnitt sehen werden) enthalten Steuerungs Punkte, die bestimmen, wie die Kurve gezeichnet wird, aber nicht Teil der gerenderten Kurve ist. Die `Bounds`-Eigenschaft enthält diese Steuerungs Punkte.

Um eine strengere Anpassung zu erzielen, verwenden Sie die `TightBounds`-Eigenschaft, die die Steuerpunkte ausschließt. Hier ist das Programm, das im Querformat ausgeführt wird, und das Verwenden der `TightBounds`-Eigenschaft zum Abrufen der Pfad Begrenzungen:

[![dreifacher Screenshot der Bogen unendlich Seite mit engen Begrenzungen](arcs-images/arcinfinitytightbounds-small.png)](arcs-images/arcinfinitytightbounds-large.png#lightbox)

Obwohl die Verbindungen zwischen den Bögen und geraden Linien mathematisch glatt sind, kann die Änderung von Bogen in gerade Linie etwas abrupt erscheinen. Im nächsten Artikel zu [**drei Typen von Bézier-Kurven**](beziers.md)wird ein besseres Unendlichkeitszeichen dargestellt.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
