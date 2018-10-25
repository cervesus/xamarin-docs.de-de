---
title: Pfadinformationen und-Enumeration
description: In diesem Artikel erläutert das Abrufen von Informationen zu SkiaSharp-Pfade und den Inhalt auflisten, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6efefe11b31428f41bfa945aff93aa70aa764870
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615274"
---
# <a name="path-information-and-enumeration"></a>Pfadinformationen und-Enumeration

_Abrufen von Informationen zu Pfaden und den Inhalt auflisten_

Die [ `SKPath` ](xref:SkiaSharp.SKPath) -Klasse definiert verschiedene Eigenschaften und Methoden, die Sie zum Abrufen von Informationen über den Pfad zu ermöglichen. Die [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds) und [ `TightBounds` ](xref:SkiaSharp.SKPath.TightBounds) Eigenschaften (und verwandter Methoden) erhalten Sie die metrische Dimensionen eines Pfads. Die [ `Contains` ](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) Methode können Sie bestimmen, ob es sich bei ein bestimmter Zeitpunkt innerhalb eines Pfads ist.

Manchmal ist es sinnvoll, um zu bestimmen, die gesamte Länge aller Linien und Kurven, die einen Pfad zu bilden. Berechnet die Länge ist keine algorithmisch einfache Aufgabe, also eine ganze Klasse mit dem Namen [ `PathMeasure` ](xref:SkiaSharp.SKPathMeasure) verwendet wird, in es.

Außerdem ist es manchmal sinnvoll, erhalten alle zeichnen-Vorgänge und Punkte, die einen Pfad zu bilden. Zunächst diese Funktion mag unnötig: Wenn Ihr Programm den Pfad erstellt hat, weiß das Programm bereits den Inhalt. Allerdings haben Sie gesehen, dass es sich bei Pfaden auch erstellt werden können [pfadeffekte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) und durch umwandeln [Zeichenfolgen in Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Sie erhalten auch alle zeichnen-Vorgänge und Punkte, die diese Pfade bilden. Eine Möglichkeit besteht darin, auf die Punkte, z. B. eine algorithmische Transformation anwenden, um den Textfluss um eine Hemisphäre:

![](information-images/pathenumerationsample.png "Text in eine Hemisphäre eingeschlossen")

## <a name="getting-the-path-length"></a>Die Länge des Pfads abrufen

In diesem Artikel [ **Pfade und Text** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) Sie haben gesehen, wie Sie mit der [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) Methode, um eine Textzeichenfolge zeichnen, deren Baseline den Kurs eines Pfads folgt. Aber was geschieht, wenn Sie den Text, damit er den Pfad genau passt die Größe möchten? Zeichnen von Text auf ein Kreis ist einfach, da der Umfang eines Kreises einfach zu berechnen ist. Der Umfang einer Ellipse oder die Länge des eine Bézierkurve ist jedoch nicht so einfach.

Die [ `SKPathMeasure` ](xref:SkiaSharp.SKPathMeasure) Klasse kann dazu beitragen. Die [Konstruktor](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) akzeptiert eine `SKPath` -Argument, und die [ `Length` ](xref:SkiaSharp.SKPathMeasure.Length) Eigenschaft zeigt die Länge.

Diese Klasse wird veranschaulicht, der **Pfadlänge** Beispiel, das basierend auf den **Bezier-Kurve** Seite. Die [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) Datei leitet sich von `InteractivePage` und enthält eine Touch-Schnittstelle:

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

Die [ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) Code-Behind-Datei ermöglicht es Ihnen, definieren die Endpunkte und Steuern von Fehlerquellen eine kubische Bézierkurve vier Berührungspunkte zu verschieben. Drei Felder definieren Sie eine Textzeichenfolge, einem `SKPaint` Objekt und eine berechnete Breite des Texts:

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

Die `baseTextWidth` Feld ist die Breite des Texts basierend auf einer `TextSize` von 10 festlegen.

Die `PaintSurface` Ereignishandler zeichnet die Bézierkurve, und klicken Sie dann die Größe des Texts entlang der vollständigen Länge angepasst:

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

Die `Length` Eigenschaft der neu erstellten `SKPathMeasure` Objekt ruft die Länge des Pfads ab. Die Länge des Pfads wird geteilt durch die `baseTextWidth` Wert (Dies ist die Breite des Texts basierend auf einem Textgröße von 10), und klicken Sie dann die basistexts Größe von 10 multipliziert. Das Ergebnis ist eine neue Textgröße für die Anzeige von Text an, dass der Pfad:

[![](information-images/pathlength-small.png "Dreifacher Screenshot der Seite Pfadlänge")](information-images/pathlength-large.png#lightbox "dreifachen Screenshot der Seite Pfadlänge")

Da die Bézierkurve länger oder kürzer wird, sehen Sie die Textgröße ändern.

## <a name="traversing-the-path"></a>Durchlaufen den Pfad

`SKPathMeasure` mehr als nur Measure die Länge des Pfads ist möglich. Für einen beliebigen Wert zwischen 0 (null) und die Länge des Pfads eine `SKPathMeasure` Objekt kann die Position in den Pfad und die Tangente der Kurve Pfad zu diesem Zeitpunkt zu erhalten. Der Tangens steht als Vektor in Form von ein `SKPoint` Objekt, oder als eine Drehung gekapselt werden soll, eine `SKMatrix` Objekt. Hier sind die Methoden der `SKPathMeasure` , die diese Informationen auf verschiedene und flexible Weise abrufen:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Die Mitglieder der [ `SKPathMeasureMatrixFlags` ](xref:SkiaSharp.SKPathMeasureMatrixFlags) Enumeration sind:

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

Die **Einrad halb-Pipe** Seite erstellt eine Animation ein Strichmännchen auf ein Einrad, das anscheinend hin und her an eine kubische Bézierkurve zeigt:

[![](information-images/unicyclehalfpipe-small.png "Dreifacher Screenshot der Seite Einrad halb-Pipe")](information-images/unicyclehalfpipe-large.png#lightbox "dreifachen Screenshot der Seite Einrad halb-Pipe")

Die `SKPaint` für Kontur zuweisen, sowohl die Hälfte Pipes als auch die Einrad verwendete Objekt ist definiert als Feld in der [ `UnicycleHalfPipePage` ]() Klasse. Wird definiert auch die `SKPath` -Objekt für die Einrad:

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

Die Klasse enthält die standard-überschreibungen von der `OnAppearing` und `OnDisappearing` Methoden für die Animation. Die `PaintSurface` Handler erstellt den Pfad für die Hälfte-Pipe und anschließend wird gezeichnet. Ein `SKPathMeasure` Objekt wird dann basierend auf diesen Pfad erstellt:

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

Die `PaintSurface` Handler berechnet einen Wert von `t` , die geht von 0 auf 1 alle fünf Sekunden. Anschließend wird mithilfe der `Math.Cos` Funktion, die auf den Wert konvertieren `t` , die reicht von 0 auf 1 und 0 (null) zurück, wobei entspricht 0 die Einrad am Anfang auf der oberen linken Ecke, während der Einrad oben rechts 1 entspricht. Die Kosinusfunktion bewirkt, dass die Geschwindigkeit der langsamste am oberen Rand der Pipe und schnellsten am unteren Rand werden.

Beachten Sie, dass der Wert `t` multipliziert werden muss, wird die Pfadlänge für das erste Argument für `GetMatrix`. Die Matrix wird dann angewendet, um die `SKCanvas` -Objekt zum Zeichnen des Pfads Einrad.

## <a name="enumerating-the-path"></a>Den Pfad auflisten

Zwei Klassen von eingebetteten `SKPath` ermöglichen es Ihnen, den Inhalt des Pfads aufzulisten. Diese Klassen sind [ `SKPath.Iterator` ](xref:SkiaSharp.SKPath.Iterator) und [ `SKPath.RawIterator` ](xref:SkiaSharp.SKPath.RawIterator). Die beiden Klassen sind sehr ähnlich, aber `SKPath.Iterator` können Elemente im Pfad mit der Länge 0 (null) oder in der Nähe einer Länge von 0 (null) vermeiden. Die `RawIterator` wird im folgenden Beispiel verwendet.

Sie können ein Objekt des Typs abrufen `SKPath.RawIterator` durch Aufrufen der [ `CreateRawIterator` ](xref:SkiaSharp.SKPath.CreateRawIterator) -Methode der `SKPath`. Auflisten von Pfad wird erreicht, indem Sie durch wiederholtes Aufrufen der [ `Next` ](xref:SkiaSharp.SKPath.RawIterator.Next*) Methode. Ein Array mit vier an die Klasse weitergeben `SKPoint` Werte:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

Die `Next` Methodenrückgabe ein Mitglied der [ `SKPathVerb` ](xref:SkiaSharp.SKPathVerb) Enumerationstyp. Diese Werte geben einen bestimmten Zeichnen-Befehl im Pfad. Die Anzahl der gültigen Punkte im Array eingefügt, hängt von dieses Verb ab:

- `Move` mit einem einzigen Punkt
- `Line` mit zwei Punkten
- `Cubic` mit vier Punkten
- `Quad` mit den drei Punkten
- `Conic` mit den drei Punkten (und auch aufrufen, die [ `ConicWeight` ](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) -Methode für die Gewichtung)
- `Close` mit einem Punkt
- `Done`

Die `Done` Verb gibt an, dass die Pfad-Enumeration abgeschlossen ist.

Beachten Sie, dass es keine `Arc` Verben. Dies gibt an, dass alle Bögen Bézierkurven umgerechnet, bei dem Pfad hinzugefügt.

Einige der Informationen in den `SKPoint` Array ist redundant. Z. B. wenn ein `Move` Verb folgt eine `Line` Verb, und klicken Sie dann auf das erste der beiden Punkte, die gemeinsam mit der `Line` ist identisch mit der `Move` zeigen. In der Praxis ist diese Redundanz sehr hilfreich. Wenn Sie erhalten eine `Cubic` -Verb, es umfasst alle vier Punkte, die die kubische Bézierkurve definieren. Sie müssen nicht die aktuelle Position hergestellt, indem das vorherige Verb beibehalten.

Das problematische-Verb, ist jedoch `Close`. Mit diesem Befehl zeichnet eine gerade Linie von der aktuellen Position, an den Anfang der Kontur, die zuvor von hergestellt, die `Move` Befehl. Im Idealfall die `Close` Verb sollten diese beiden Punkte statt nur einem Punkt bereitstellen. Schlimmer noch ist, zu dem Punkt der `Close` Verb ist immer (0, 0). Wenn Sie einen Pfad durchlaufen haben, müssen Sie wahrscheinlich beibehalten der `Move` Punkt und die aktuelle Position.

## <a name="enumerating-flattening-and-malforming"></a>Auflisten von vereinfachen und Malforming

Manchmal ist es wünschenswert sein, eine algorithmische anwenden transformiert einen Pfad zu Malform in irgendeiner Weise:

![](information-images/pathenumerationsample.png "Text in eine Hemisphäre eingeschlossen")

Die meisten dieser Buchstaben bestehen aus geraden, aber diese geraden haben offensichtlich in Kurven Verdrehte wurde. Wie ist dies möglich?

Der Schlüssel ist, dass die ursprünglichen geraden Linien in eine Reihe von kleineren geraden beeinträchtigt werden. Diese einzelnen geraden Linien an kleinere können klicken Sie dann auf unterschiedliche Weisen für die form einer Kurve bearbeitet werden. 

Zur Unterstützung dieses Vorgangs die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel enthält eine statische [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) -Klasse mit einer `Interpolate` -Methode, die unterteilt ein die gerade in zahlreichen kurze Zeilen, die nur eine Einheit lang sind. Darüber hinaus enthält die Klasse mehrere Methoden, die die drei Typen von Bézierkurven in eine Reihe von kleinen gerade Linien konvertieren, die die Kurve annähern. (Die parametrischen Formeln wurden in diesem Artikel präsentiert [ **drei Typen von Bézierkurven**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) Dieser Prozess heißt _vereinfachen_ Kurve:

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
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

Alle diese Methoden werden auf die verwiesen wird von der Erweiterungsmethode `CloneWithTransform` auch in dieser Klasse enthalten, und unten. Diese Methode wird einen Pfad durch Aufzählen der Path-Befehle aus, und erstellen einen neuen Pfad basierend auf den Daten geklont. Allerdings der neue Pfad besteht nur aus `MoveTo` und `LineTo` aufrufen. Alle Kurven und gerade Linien werden auf einer Reihe von kleinen Zeilen reduziert.

Beim Aufrufen von `CloneWithTransform`, Sie an die Methode übergeben, eine `Func<SKPoint, SKPoint>`, dies ist eine Funktion mit einer `SKPaint` Parameter, der zurückgegeben ein `SKPoint` Wert. Diese Funktion wird für jede, zeigen Sie auf eine benutzerdefinierte algorithmische Transformation anwenden aufgerufen:

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

Beachten Sie, dass die Methode den ersten Punkt der einzelnen Contour Global in der Variablen behält `firstPoint` und die aktuelle Position nach einzelnen Zeichnen-Befehl in der Variablen `lastPoint`. Diese Variablen werden zum Erstellen der letzten schließendes Zeile, wenn eine `Close` Verb festgestellt wird.

Die **GlobularText** Beispiel verwendet diese Erweiterungsmethode, um scheinbar Text in eine Hemisphäre eine 3D-Effekt umbrochen:

[![](information-images/globulartext-small.png "Dreifacher Screenshot der Seite Globular Text")](information-images/globulartext-large.png#lightbox "dreifachen Screenshot der Seite Globular Text")

Die [ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Klassenkonstruktor führt diese Transformation. Erstellt eine `SKPaint` -Objekt für den Text und erhält dann eine `SKPath` -Objekt aus der `GetTextPath` Methode. Dies ist der Pfad, die an die `CloneWithTransform` Erweiterungsmethode zusammen mit einer Transform-Funktion:

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

Die Transform-Funktion berechnet zuerst die beiden Werte, die mit dem Namen `longitude` und `latitude` zwischen – π/2 oben und links vom Text, π/2 auf den rechten und unteren Rand des Texts. Der Wertebereich nicht visuell zufriedenstellenden, damit sie durch Multiplizieren mit 0,75 reduziert werden. (Verwenden Sie den Code ohne die Anpassungen aus. Der Text wird auf dem Norden und Süden Polen undurchsichtig, und an den Seiten zu dünn.) Konvertiert diese dreidimensionalen sphärischen Koordinaten zu zweidimensionalen `x` und `y` Koordinaten standard Formeln.

Der neue Pfad wird als Feld gespeichert. Die `PaintSurface` muss Handler dann lediglich einen zentrieren und skalieren den Pfad ein, auf dem Bildschirm angezeigt:

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

Dies ist eine sehr vielseitige Technik. Wenn das Array von pfadeffekte in beschrieben die [ **Pfadeffekte** ](effects.md) Artikel nicht sehr umfassen, etwas Sie der Ansicht enthalten ist, muss dies ist eine Möglichkeit zum Füllen der Lücken.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
