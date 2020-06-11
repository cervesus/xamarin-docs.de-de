---
Title: "Pfadinformationen und Enumeration" Beschreibung: "in diesem Artikel wird erläutert, wie Sie Informationen zu skiasharp-Pfaden erhalten und den Inhalt auflisten. Dies wird mit Beispielcode veranschaulicht."
ms. Prod: xamarin ms. assetid: 8e8c5c6a-F324-4155-8652-7a77d231b3e5 ms. Technology: xamarin-skiasharp Author: davidbritch ms. Author: dabritch ms. Date: 09/12/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="path-information-and-enumeration"></a>Pfadinformationen und -enumeration

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Informationen zu Pfaden und zum Aufzählen der Inhalte_

Die [`SKPath`](xref:SkiaSharp.SKPath) -Klasse definiert verschiedene Eigenschaften und Methoden, mit denen Sie Informationen zum Pfad abrufen können. Die-Eigenschaft und die-Eigenschaft [`Bounds`](xref:SkiaSharp.SKPath.Bounds) [`TightBounds`](xref:SkiaSharp.SKPath.TightBounds) (und verwandte Methoden) erhalten die metrischen Dimensionen eines Pfads. Mit der- [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) Methode können Sie feststellen, ob sich ein bestimmter Punkt innerhalb eines Pfads befindet.

Es ist manchmal hilfreich, die Gesamtlänge aller Linien und Kurven zu ermitteln, die einen Pfad bilden. Wenn Sie diese Länge berechnen, handelt es sich nicht um eine algorithmisch einfache Aufgabe. Daher wird ihr eine ganze Klasse [`PathMeasure`](xref:SkiaSharp.SKPathMeasure) mit dem Namen hinzu.

Manchmal ist es auch sinnvoll, alle Zeichnungsvorgänge und Punkte zu erhalten, aus denen ein Pfad besteht. Diese Anlage mag zunächst unnötig erscheinen: Wenn das Programm den Pfad erstellt hat, weiß das Programm den Inhalt bereits. Sie haben jedoch gesehen, dass Pfade auch durch [Pfad Effekte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) und durch die Umstellung von Text Zeichenfolgen [in Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)erstellt werden können. Sie können auch alle Zeichnungsvorgänge und Punkte abrufen, die diese Pfade bilden. Eine Möglichkeit besteht darin, eine algorithmische Transformation auf alle Punkte anzuwenden, um z. b. Text um eine Hemisphäre zu umschließen:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

## <a name="getting-the-path-length"></a>Die Pfadlänge wird erhalten.

In den Artikeln [**Pfaden und Text**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) haben Sie erfahren, wie Sie die- [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) Methode verwenden, um eine Text Zeichenfolge zu zeichnen, deren Baseline dem Pfad folgt. Aber was ist, wenn Sie den Text so anpassen möchten, dass er genau dem Pfad entspricht? Das Zeichnen von Text um einen Kreis ist einfach, da der Umfang eines Kreises einfach zu berechnen ist. Der Umfang einer Ellipse oder die Länge einer Bézier-Kurve ist jedoch nicht so einfach.

Die- [`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure) Klasse kann dabei helfen. Der- [Konstruktor](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) akzeptiert ein `SKPath` -Argument, und die- [`Length`](xref:SkiaSharp.SKPathMeasure.Length) Eigenschaft zeigt seine Länge an.

Diese Klasse wird im Beispiel " **path length** " veranschaulicht, das auf der **Bezier-Kurven** Seite basiert. Die Datei [**pathlängen Page. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) wird von abgeleitet `InteractivePage` und enthält eine Berührungs Schnittstelle:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

Mit der [**PathLengthPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) -Code Behind-Datei können Sie vier Berührungspunkte verschieben, um die Endpunkte und Steuerungs Punkte einer kubischen Bézier-Kurve zu definieren. Drei Felder definieren eine Text Zeichenfolge, ein `SKPaint` Objekt und eine berechnete Breite des Texts:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

Das `baseTextWidth` Feld ist die Breite des Texts basierend auf der `TextSize` Einstellung 10.

Der `PaintSurface` Handler zeichnet die Bézier-Kurve und passt dann die Größe des Texts an seine vollständige Länge an:

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

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

Die- `Length` Eigenschaft des neu erstellten- `SKPathMeasure` Objekts erhält die Länge des Pfads. Die Pfadlänge wird durch den-Wert (d. h. `baseTextWidth` die Breite des Texts auf der Grundlage einer Textgröße von 10) dividiert und dann mit der Basis Textgröße von 10 multipliziert. Das Ergebnis ist eine neue Textgröße zum Anzeigen des Texts entlang dieses Pfads:

[![](information-images/pathlength-small.png "Triple screenshot of the Path Length page")](information-images/pathlength-large.png#lightbox "Triple screenshot of the Path Length page")

Wenn die Bézier-Kurve länger oder kürzer wird, können Sie die Änderung der Textgröße sehen.

## <a name="traversing-the-path"></a>Durchlaufen des Pfads

`SKPathMeasure`kann mehr als nur die Länge des Pfads messen. Für einen beliebigen Wert zwischen 0 (null) und der Pfadlänge `SKPathMeasure` kann ein Objekt die Position auf dem Pfad und den Tangens der Pfad Kurve an diesem Punkt abrufen. Der Tangens ist als Vektor in Form eines `SKPoint` Objekts oder als in einem-Objekt gekapselt Drehung verfügbar `SKMatrix` . Im folgenden finden Sie die Methoden von `SKPathMeasure` , die diese Informationen auf verschiedene und flexible Weise erhalten:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Die Member der- [`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags) Enumeration lauten:

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

Die Seite " **Unicycle-halbpipe** " animiert eine Stift Figur in einem Unicycle, der sich entlang einer kubischen Bézier-Kurve hin-und herzieht:

[![](information-images/unicyclehalfpipe-small.png "Triple screenshot of the Unicycle Half-Pipe page")](information-images/unicyclehalfpipe-large.png#lightbox "Triple screenshot of the Unicycle Half-Pipe page")

Das `SKPaint` -Objekt, das zum Durchsuchen der Hälfte Pipe und des Unicycle verwendet wird, wird als ein Feld in der- `UnicycleHalfPipePage` Klasse definiert. Außerdem ist das- `SKPath` Objekt für den Unicycle definiert:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Die-Klasse enthält die Standard Überschreibungen der `OnAppearing` -Methode und der- `OnDisappearing` Methode für die Animation. Der `PaintSurface` Handler erstellt den Pfad für die halbpipe und zeichnet ihn anschließend. Ein `SKPathMeasure` Objekt wird dann basierend auf diesem Pfad erstellt:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

Der- `PaintSurface` Handler berechnet einen Wert von `t` , der alle fünf Sekunden zwischen 0 und 1 liegt. Anschließend wird die- `Math.Cos` Funktion verwendet, um diese in einen-Wert zu konvertieren, der `t` zwischen 0 und 1 liegt, und zurück auf 0, wobei 0 dem Unicycle am Anfang Links entspricht, während 1 dem Unicycle oben rechts entspricht. Die Cosinus-Funktion bewirkt, dass die Geschwindigkeit am oberen Rand der Pipe am oberen Rand und am unteren Rand am unteren Rand liegt.

Beachten Sie, dass dieser Wert von `t` mit der Pfadlänge für das erste Argument für multipliziert werden muss `GetMatrix` . Die Matrix wird dann auf das- `SKCanvas` Objekt angewendet, um den Unicycle-Pfad zu zeichnen.

## <a name="enumerating-the-path"></a>Auflisten des Pfads

Zwei eingebettete Klassen von `SKPath` ermöglichen es Ihnen, den Inhalt von Path aufzuzählen. Diese Klassen sind [`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator) und [`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator) . Die beiden Klassen sind sehr ähnlich, `SKPath.Iterator` können jedoch Elemente im Pfad mit einer Länge von 0 (null) oder bis zu einer Länge von 0 (null) eliminieren. Die `RawIterator` wird im folgenden Beispiel verwendet.

Sie können ein Objekt vom Typ abrufen, `SKPath.RawIterator` indem Sie die- [`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator) Methode von Aufrufen `SKPath` . Das Auflisten durch den Pfad erfolgt durch wiederholtes Aufrufen der- [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*) Methode. Übergeben Sie ein Array aus vier `SKPoint` Werten:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

Die- `Next` Methode gibt einen Member des- [`SKPathVerb`](xref:SkiaSharp.SKPathVerb) Enumerationstyps zurück. Diese Werte geben den jeweiligen Zeichnungs Befehl im Pfad an. Die Anzahl der gültigen Punkte, die in das Array eingefügt werden, hängt von diesem Verb ab:

- `Move`mit einem einzigen Punkt
- `Line`mit zwei Punkten
- `Cubic`mit vier Punkten
- `Quad`mit drei Punkten
- `Conic`mit drei Punkten (und auch die- [`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) Methode für das Gewicht aufzurufen)
- `Close`mit einem Punkt
- `Done`

Das `Done` Verb gibt an, dass die pfadenumeration beendet ist.

Beachten Sie, dass keine `Arc` Verben vorhanden sind. Dies gibt an, dass alle Bögen in Bézier-Kurven konvertiert werden, wenn Sie dem Pfad hinzugefügt werden.

Einige der Informationen im `SKPoint` Array sind redundant. Wenn z. b. auf ein `Move` Verb ein `Line` Verb folgt, ist der erste der beiden Punkte, die mit dem einhergehen, mit dem `Line` `Move` Punkt identisch. In der Praxis ist diese Redundanz sehr hilfreich. Wenn Sie ein `Cubic` Verb erhalten, werden alle vier Punkte begleitet, die die kubische Bézier-Kurve definieren. Sie müssen die aktuelle Position nicht beibehalten, die durch das vorherige Verb festgelegt wurde.

Das problematische Verb ist jedoch `Close` . Dieser Befehl zeichnet eine gerade Linie von der aktuellen Position bis zum Anfang der Kontur, die zuvor durch den Befehl festgelegt wurde `Move` . Im Idealfall `Close` sollte das Verb diese beiden Punkte anstelle eines Punkts bereitstellen. Schlimmer noch ist, dass der Punkt, der das Verb begleitet, `Close` immer (0,0) ist. Wenn Sie einen Pfad auflisten, müssen Sie den `Move` Punkt und die aktuelle Position wahrscheinlich beibehalten.

## <a name="enumerating-flattening-and-malforming"></a>Auflisten, vereinfachen und Fehlbildung

Es ist manchmal wünschenswert, eine algorithmische Transformation auf einen Pfad anzuwenden, um ihn auf irgendeine Weise zu malformen:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

Die meisten dieser Buchstaben bestehen aus geraden Linien, aber diese geraden Linien wurden anscheinend in Kurven gedreht. Wie ist das möglich?

Der Schlüssel besteht darin, dass die ursprünglichen geraden Linien in eine Reihe von kleineren geraden Zeilen unterteilt werden. Diese einzelnen kleineren geraden Linien können dann auf unterschiedliche Weise bearbeitet werden, um eine Kurve zu bilden.

Zur Unterstützung dieses Prozesses enthält das [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel eine statische- [`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) Klasse mit einer- `Interpolate` Methode, die eine gerade Linie in zahlreiche kurze Zeilen unterteilt, die nur eine Einheit in der Länge haben. Darüber hinaus enthält die-Klasse mehrere Methoden, die die drei Typen der Bézier-Kurven in eine Reihe von kleinen geraden Linien konvertieren, die der Kurve annähernd entsprechen. (Die parametrischen Formeln wurden im Artikel [**drei Typen von Bézier-Kurven**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)dargestellt.) Dieser Vorgang wird als _vereinfachende_ Kurve bezeichnet:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

Auf alle diese Methoden wird von der Erweiterungsmethode verwiesen, die `CloneWithTransform` auch in dieser Klasse enthalten ist, und unten dargestellt. Mit dieser Methode wird ein Pfad geklont, indem die Pfad Befehle aufgelistet und ein neuer Pfad auf der Grundlage der Daten erstellt wird. Der neue Pfad besteht jedoch nur aus `MoveTo` -und- `LineTo` aufrufen. Alle Kurven und geraden Linien werden auf eine Reihe von kleinen Linien reduziert.

Wenn `CloneWithTransform` Sie aufrufen, übergeben Sie an die-Methode `Func<SKPoint, SKPoint>` , die eine Funktion mit einem-Parameter ist, der `SKPaint` einen-Wert zurückgibt `SKPoint` . Diese Funktion wird für jeden Punkt aufgerufen, um eine benutzerdefinierte algorithmische Transformation anzuwenden:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Da der geklonte Pfad zu kleinen geraden Zeilen reduziert wird, kann die Transform-Funktion gerade Linien in Kurven umwandeln.

Beachten Sie, dass die-Methode den ersten Punkt jeder Kontur in der Variablen mit dem Namen `firstPoint` und die aktuelle Position nach jedem Zeichnungs Befehl in der Variablen beibehält `lastPoint` . Diese Variablen sind erforderlich, um die abschließende schließende Zeile zu erstellen, wenn ein `Close` Verb gefunden wird.

Das **globulartext** -Beispiel verwendet diese Erweiterungsmethode, um Text mit einer Hemisphäre in einem 3D-Effekt scheinbar zu umschließen:

[![](information-images/globulartext-small.png "Triple screenshot of the Globular Text page")](information-images/globulartext-large.png#lightbox "Triple screenshot of the Globular Text page")

Der [`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Klassenkonstruktor führt diese Transformation aus. Er erstellt ein `SKPaint` -Objekt für den Text und ruft dann ein- `SKPath` Objekt aus der- `GetTextPath` Methode ab. Dies ist der Pfad, der der `CloneWithTransform` Erweiterungsmethode zusammen mit einer Transformations Funktion weitergegeben wird:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Die Transform-Funktion berechnet zuerst zwei Werte mit dem Namen `longitude` und den `latitude` Bereich von –./2 am oberen und linken Rand des Texts bis zum Wert von "" auf "". Der Bereich dieser Werte ist nicht visuell zufriedenstellend, sodass Sie durch Multiplikation um 0,75 reduziert werden. (Testen Sie den Code ohne diese Anpassungen. Der Text wird auf der Nord-und der Südpol zu unbeschädigt und auf der Seite zu dünn.) Diese dreidimensionalen kugelförmigen Koordinaten werden `x` von Standardformeln in zweidimensionale-und- `y` Koordinaten konvertiert.

Der neue Pfad wird als Feld gespeichert. Der `PaintSurface` Handler muss dann lediglich den Pfad zentrieren und skalieren, um ihn auf dem Bildschirm anzuzeigen:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

Dies ist eine sehr vielseitige Methode. Wenn das Array von Pfad Effekten, das im Artikel " [**path Effects**](effects.md) " beschrieben wird, nicht ganz so eingeschlossen ist, dass Sie es einschließen müssen, ist dies eine Möglichkeit, die Lücken zu füllen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
