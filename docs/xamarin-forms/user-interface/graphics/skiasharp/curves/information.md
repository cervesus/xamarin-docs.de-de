---
title: Pfadinformationen und -enumeration
description: In diesem Artikel erläutert das Abrufen von Informationen zu SkiaSharp-Pfade und den Inhalt auflisten, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6f4f4e6253c14d86e2057f13d6232a07a83b4d26
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045079"
---
# <a name="path-information-and-enumeration"></a>Pfadinformationen und -enumeration

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Informationen zu Pfaden und zum Aufzählen der Inhalte_

Die [`SKPath`](xref:SkiaSharp.SKPath) -Klasse definiert verschiedene Eigenschaften und Methoden, mit denen Sie Informationen zum Pfad abrufen können. Die Eigenschaften " [`Bounds`](xref:SkiaSharp.SKPath.Bounds) " und " [`TightBounds`](xref:SkiaSharp.SKPath.TightBounds) " (und verwandte Methoden) erhalten die metrischen Dimensionen eines Pfads. Mit der [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) -Methode können Sie feststellen, ob sich ein bestimmter Punkt innerhalb eines Pfads befindet.

Manchmal ist es sinnvoll, um zu bestimmen, die gesamte Länge aller Linien und Kurven, die einen Pfad zu bilden. Das Berechnen dieser Länge ist keine algorithmisch einfache Aufgabe, sodass eine ganze Klasse mit dem Namen " [`PathMeasure`](xref:SkiaSharp.SKPathMeasure) " dafür verwendet wird.

Außerdem ist es manchmal sinnvoll, erhalten alle zeichnen-Vorgänge und Punkte, die einen Pfad zu bilden. Zunächst diese Funktion mag unnötig: Wenn Ihr Programm den Pfad erstellt hat, weiß das Programm bereits den Inhalt. Sie haben jedoch gesehen, dass Pfade auch durch [Pfad Effekte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) und durch die Umstellung von Text Zeichenfolgen [in Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)erstellt werden können. Sie erhalten auch alle zeichnen-Vorgänge und Punkte, die diese Pfade bilden. Eine Möglichkeit besteht darin, auf die Punkte, z. B. eine algorithmische Transformation anwenden, um den Textfluss um eine Hemisphäre:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

## <a name="getting-the-path-length"></a>Die Länge des Pfads abrufen

Im Artikel [**Pfade und Text**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) haben Sie erfahren, wie Sie die [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) -Methode verwenden, um eine Text Zeichenfolge zu zeichnen, deren Baseline dem Pfad folgt. Aber was geschieht, wenn Sie den Text, damit er den Pfad genau passt die Größe möchten? Zeichnen von Text auf ein Kreis ist einfach, da der Umfang eines Kreises einfach zu berechnen ist. Der Umfang einer Ellipse oder die Länge des eine Bézierkurve ist jedoch nicht so einfach.

Die [`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure) -Klasse kann helfen. Der [Konstruktor](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) akzeptiert ein `SKPath` Argument, und die [`Length`](xref:SkiaSharp.SKPathMeasure.Length) -Eigenschaft zeigt seine Länge an.

Diese Klasse wird im Beispiel " **path length** " veranschaulicht, das auf der **Bezier-Kurven** Seite basiert. Die Datei " [**pathverlängert Page. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) " wird von `InteractivePage` abgeleitet und enthält eine Berührungs Schnittstelle:

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

Die `Length`-Eigenschaft des neu erstellten `SKPathMeasure`-Objekts erhält die Länge des Pfads. Die Pfadlänge wird durch den `baseTextWidth` Wert dividiert (d. h. die Breite des Texts auf der Grundlage einer Textgröße von 10) und dann mit der Basis Textgröße von 10 multipliziert. Das Ergebnis ist eine neue Textgröße für die Anzeige von Text an, dass der Pfad:

[![](information-images/pathlength-small.png "Triple screenshot of the Path Length page")](information-images/pathlength-large.png#lightbox "Triple screenshot of the Path Length page")

Da die Bézierkurve länger oder kürzer wird, sehen Sie die Textgröße ändern.

## <a name="traversing-the-path"></a>Durchlaufen den Pfad

`SKPathMeasure` können mehr tun als nur die Länge des Pfads zu messen. Für einen beliebigen Wert zwischen 0 (null) und der Pfadlänge kann ein `SKPathMeasure` Objekt die Position auf dem Pfad und den Tangens der Pfad Kurve an diesem Punkt abrufen. Der Tangens ist als Vektor in Form eines `SKPoint` Objekts oder als Drehung verfügbar, die in einem `SKMatrix` Objekt gekapselt ist. Im folgenden finden Sie die Methoden von `SKPathMeasure`, die diese Informationen auf verschiedene und flexible Weise abrufen:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Die Member der [`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags) -Enumeration lauten wie folgt:

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

Die Seite " **Unicycle-halbpipe** " animiert eine Stift Figur in einem Unicycle, der sich entlang einer kubischen Bézier-Kurve hin-und herzieht:

[![](information-images/unicyclehalfpipe-small.png "Triple screenshot of the Unicycle Half-Pipe page")](information-images/unicyclehalfpipe-large.png#lightbox "Triple screenshot of the Unicycle Half-Pipe page")

Das `SKPaint`-Objekt, das für das Durchsuchen der Hälfte Pipe und des Unicycle verwendet wird, wird als Feld in der `UnicycleHalfPipePage` Klasse definiert. Außerdem ist das `SKPath` Objekt für den Unicycle definiert:

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

Die-Klasse enthält die Standard Überschreibungen der `OnAppearing` und `OnDisappearing` Methoden für Animation. Der `PaintSurface` Handler erstellt den Pfad für die halbpipe und zeichnet ihn anschließend. Ein `SKPathMeasure`-Objekt wird dann basierend auf diesem Pfad erstellt:

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

Der `PaintSurface`-Handler berechnet den Wert `t`, der alle fünf Sekunden zwischen 0 und 1 liegt. Anschließend wird die `Math.Cos`-Funktion verwendet, um diese in einen Wert von `t` zu konvertieren, der zwischen 0 und 1 liegt, und zurück auf 0 (null), wobei 0 dem Unicycle am Anfang Links entspricht, während 1 dem Unicycle oben rechts entspricht. Die Kosinusfunktion bewirkt, dass die Geschwindigkeit der langsamste am oberen Rand der Pipe und schnellsten am unteren Rand werden.

Beachten Sie, dass dieser Wert `t` mit der Pfadlänge für das erste Argument `GetMatrix`multipliziert werden muss. Die Matrix wird dann auf das `SKCanvas` Objekt zum Zeichnen des Unicycle-Pfads angewendet.

## <a name="enumerating-the-path"></a>Den Pfad auflisten

Zwei eingebettete Klassen von `SKPath` es Ihnen ermöglichen, den Inhalt von Path aufzuzählen. Diese Klassen sind [`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator) und [`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator). Die beiden Klassen sind sehr ähnlich, aber `SKPath.Iterator` können Elemente im Pfad mit einer Länge von 0 (null) oder bis zu einer Länge von 0 (null) eliminieren. Im folgenden Beispiel wird der `RawIterator` verwendet.

Sie können ein Objekt vom Typ `SKPath.RawIterator` abrufen, indem Sie die [`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator) -Methode von `SKPath`aufrufen. Das Auflisten durch den Pfad erfolgt durch wiederholtes Aufrufen der [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*) -Methode. Übergeben Sie ein Array aus vier `SKPoint` Werten:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

Die `Next`-Methode gibt einen Member des [`SKPathVerb`](xref:SkiaSharp.SKPathVerb) Enumerationstyps zurück. Diese Werte geben einen bestimmten Zeichnen-Befehl im Pfad. Die Anzahl der gültigen Punkte im Array eingefügt, hängt von dieses Verb ab:

- `Move` mit einem einzigen Punkt
- `Line` mit zwei Punkten
- `Cubic` mit vier Punkten
- `Quad` mit drei Punkten
- `Conic` mit drei Punkten (und auch die [`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) -Methode für die Gewichtung)
- `Close` mit einem Punkt
- `Done`

Das `Done` Verb gibt an, dass die pfadenumeration beendet ist.

Beachten Sie, dass keine `Arc` Verben vorhanden sind. Dies gibt an, dass alle Bögen Bézierkurven umgerechnet, bei dem Pfad hinzugefügt.

Einige der Informationen im `SKPoint` Array sind redundant. Wenn z. b. auf ein `Move` Verb ein `Line` Verb folgt, ist der erste der beiden Punkte, die die `Line` begleiten, mit dem `Move` Punkt identisch. In der Praxis ist diese Redundanz sehr hilfreich. Wenn Sie ein `Cubic` Verb erhalten, werden alle vier Punkte begleitet, die die kubische Bézier-Kurve definieren. Sie müssen nicht die aktuelle Position hergestellt, indem das vorherige Verb beibehalten.

Das problematische Verb ist jedoch `Close`. Dieser Befehl zeichnet eine gerade Linie von der aktuellen Position bis zum Anfang der Kontur, die zuvor durch den `Move`-Befehl festgelegt wurde. Im Idealfall sollte das `Close` Verb diese beiden Punkte anstelle eines Punkts bereitstellen. Schlimmer noch: der Punkt, der das `Close` Verb begleitet, ist immer (0,0). Wenn Sie einen Pfad auflisten, müssen Sie wahrscheinlich den `Move` Punkt und die aktuelle Position beibehalten.

## <a name="enumerating-flattening-and-malforming"></a>Auflisten von vereinfachen und Malforming

Manchmal ist es wünschenswert sein, eine algorithmische anwenden transformiert einen Pfad zu Malform in irgendeiner Weise:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

Die meisten dieser Buchstaben bestehen aus geraden, aber diese geraden haben offensichtlich in Kurven Verdrehte wurde. Wie ist dies möglich?

Der Schlüssel ist, dass die ursprünglichen geraden Linien in eine Reihe von kleineren geraden beeinträchtigt werden. Diese einzelnen geraden Linien an kleinere können klicken Sie dann auf unterschiedliche Weisen für die form einer Kurve bearbeitet werden.

Um dieses Verfahren zu unterstützen, enthält das [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel eine statische [`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) -Klasse mit einer `Interpolate`-Methode, die eine gerade Linie in zahlreiche kurze Zeilen unterteilt, die nur eine Einheit haben. Darüber hinaus enthält die Klasse mehrere Methoden, die die drei Typen von Bézierkurven in eine Reihe von kleinen gerade Linien konvertieren, die die Kurve annähern. (Die parametrischen Formeln wurden im Artikel [**drei Typen von Bézier-Kurven**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)dargestellt.) Dieser Vorgang wird als _vereinfachende_ Kurve bezeichnet:

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

Auf alle diese Methoden wird von der Erweiterungsmethode verwiesen `CloneWithTransform` die auch in dieser Klasse enthalten und unten dargestellt ist. Diese Methode wird einen Pfad durch Aufzählen der Path-Befehle aus, und erstellen einen neuen Pfad basierend auf den Daten geklont. Der neue Pfad besteht jedoch nur aus `MoveTo`-und `LineTo` aufrufen. Alle Kurven und gerade Linien werden auf einer Reihe von kleinen Zeilen reduziert.

Wenn Sie `CloneWithTransform`aufrufen, übergeben Sie an die-Methode eine `Func<SKPoint, SKPoint>`, bei der es sich um eine Funktion mit einem `SKPaint`-Parameter handelt, der einen `SKPoint` Wert zurückgibt. Diese Funktion wird für jede, zeigen Sie auf eine benutzerdefinierte algorithmische Transformation anwenden aufgerufen:

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

Da der geklonte Pfad auf kleinen geraden reduziert wird, verfügt über die Transform-Funktion auf die Funktion gerade Linien, Kurven konvertieren.

Beachten Sie, dass die-Methode den ersten Punkt jeder Kontur in der Variablen mit dem Namen `firstPoint` und die aktuelle Position nach jedem Zeichnungs Befehl in der Variablen `lastPoint`beibehält. Diese Variablen sind erforderlich, um die abschließende schließende Zeile zu erstellen, wenn ein `Close` Verb gefunden wird.

Das **globulartext** -Beispiel verwendet diese Erweiterungsmethode, um Text mit einer Hemisphäre in einem 3D-Effekt scheinbar zu umschließen:

[![](information-images/globulartext-small.png "Triple screenshot of the Globular Text page")](information-images/globulartext-large.png#lightbox "Triple screenshot of the Globular Text page")

Der [`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) -Klassenkonstruktor führt diese Transformation aus. Er erstellt ein `SKPaint`-Objekt für den Text und ruft dann ein `SKPath` Objekt aus der `GetTextPath`-Methode ab. Dies ist der Pfad, der der `CloneWithTransform`-Erweiterungsmethode zusammen mit einer Transformations Funktion weitergegeben wird:

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

Die Transformations Funktion berechnet zuerst zwei Werte mit dem Namen `longitude` und `latitude`, die von –./2 am oberen und linken Rand des Texts liegen, bis auf den Text am rechten und unteren Rand des Texts. Der Wertebereich nicht visuell zufriedenstellenden, damit sie durch Multiplizieren mit 0,75 reduziert werden. (Verwenden Sie den Code ohne die Anpassungen aus. Der Text wird auf der Nord-und der Südpol zu unbeschädigt und auf der Seite zu dünn.) Diese dreidimensionalen kugelförmigen Koordinaten werden in zweidimensionale `x` und `y` Koordinaten nach Standardformeln konvertiert.

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

Dies ist eine sehr vielseitige Technik. Wenn das Array von Pfad Effekten, das im Artikel " [**path Effects**](effects.md) " beschrieben wird, nicht ganz so eingeschlossen ist, dass Sie es einschließen müssen, ist dies eine Möglichkeit, die Lücken zu füllen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
