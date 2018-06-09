---
title: Pfad und -Enumeration
description: In diesem Artikel wird erläutert, wie das Abrufen von Informationen zu SkiaSharp Pfade und aufführen des Inhalts und wird dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 53d1fce20a0e3bc75ba34ab84b2549211567e222
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243791"
---
# <a name="path-information-and-enumeration"></a>Pfad und -Enumeration

_Abrufen von Informationen über Pfade und aufführen des Inhalts_

Die [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Klasse definiert verschiedene Eigenschaften und Methoden, die Sie zum Abrufen von Informationen über den Pfad zu ermöglichen. Die [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) und [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) Eigenschaften (und die zugehörigen Methoden) erhalten Sie die metrische Dimensionen eines Pfads. Die [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) -Methode können Sie feststellen, ob es sich bei einem bestimmten Zeitpunkt innerhalb eines Pfads ist.

Es ist manchmal hilfreich, um zu bestimmen, die Gesamtlänge der alle Linien und Kurven, aus denen ein Pfad besteht. Dies ist kein algorithmisch einfache Aufgabe, damit eine gesamte Objektklasse mit dem Namen [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) gewidmet ist.

Außerdem ist es manchmal sinnvoll, erhalten alle zeichnen Operationen und Punkte, aus denen ein Pfad besteht. Anfangs dieser Funktion unnötige erscheinen möglicherweise: Wenn das Programm den Pfad erstellt hat, das Programm bereits kennt den Inhalt. Allerdings haben Sie gesehen, dass Pfade auch können, können Sie durch erstellt werden [Pfad Effekte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) und zurückgewandelt [Textzeichenfolgen in Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Sie erhalten auch alle zeichnen Operationen und Punkte, die diese Pfade bilden. Eine Möglichkeit besteht darin, eine Transformation algorithmische für alle Punkte gelten. Dadurch können Techniken wie z. B. das Umbrechen von Text um eine Hemisphäre:

![](information-images/pathenumerationsample.png "Text, der auf eine Hemisphäre umschlossen")

## <a name="getting-the-path-length"></a>Abrufen von der Pfadlänge

Im Artikel [ **Pfade und Text** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) gelernt zum Verwenden der [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) Methode, um eine Textzeichenfolge gezeichnet werden soll, deren Baseline den Kurs eines Pfads folgt. Aber was geschieht, wenn Sie den Text zu skalieren, damit sie den Pfad genau passen möchten? Zum Zeichnen von Text um einen Kreis, ist dies einfach, da der Umfang eines Kreises einfach zu berechnen ist. Der bei einer Ellipse, oder die Länge der Bézier-Kurve ist jedoch nicht so einfach.

Die [ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Klasse kann dazu beitragen. Die [Konstruktor](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) akzeptiert ein `SKPath` Argument, und die [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) Eigenschaft zeigt die Länge.

Dies wird dargestellt, der **Pfadlänge** Beispiel wird basierend auf der **Bézier-Kurve** Seite. Die [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) Datei leitet sich von `InteractivePage` und eine Schnittstelle für die Fingereingabe umfasst:

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

Die [ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) Code-Behind-Datei können Sie definieren den Endpunkten und Steuerpunkte eine kubische Bézier-Kurve vier Berührungspunkte zu verschieben. Drei Felder definieren eine Textzeichenfolge ein `SKPaint` Objekt und eine berechnete Breite des Texts:

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

Die `PaintSurface` Handler der Bézier-Kurve gezeichnet, und klicken Sie dann die Größe des Texts, der auf die volle Länge passen:

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

Die `Length` -Eigenschaft des neu erstellten `SKPathMeasure` Objekt ruft die Länge des Pfads ab. Dies ist die dividiert durch die `baseTextWidth` Wert (Hierbei handelt es sich um die Breite des Texts basierend auf einer Textgröße von 10), und klicken Sie dann die Basistext Größe von 10 multipliziert. Das Ergebnis ist eine neue Textgröße für die Anzeige von Text an, dass der Pfad:

[![](information-images/pathlength-small.png "Dreifacher Screenshot der Seite Pfadlänge")](information-images/pathlength-large.png#lightbox "dreifacher Screenshot der Seite Pfadlänge")

Die Bézier-Kurve länger oder kürzer wird, sehen Sie die Textgröße zu ändern.

## <a name="traversing-the-path"></a>Durchlaufen den Pfad

`SKPathMeasure` mehr als nur Measure die Länge des Pfads ist möglich. Für einen beliebigen Wert zwischen 0 (null) und die Pfadlänge ein `SKPathMeasure` Objekt kann die Position auf den Pfad und den Tangens der Kurve Pfad zu diesem Zeitpunkt abrufen. Der Tangens steht als einen Vektor in Form von einer `SKPoint` -Objekt oder als eine Drehung im gekapselte ein `SKMatrix` Objekt. Hier werden die Methoden der `SKPathMeasure` , die diese Informationen auf verschiedene und flexible Weise abrufen:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Die [ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/) sind:

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

Die **Einrad Hälfte-Pipe** Seite eine Animation auf eine Einrad, die zusammen eine kubische Bézier-Kurve hin-und zeigt scheint ein Strichmännchen:

[![](information-images/unicyclehalfpipe-small.png "Dreifacher Screenshot der Seite Einrad Hälfte-Pipe")](information-images/unicyclehalfpipe-large.png#lightbox "dreifacher Screenshot der Seite Einrad Hälfte-Pipe")

Die `SKPaint` für den Verlauf der Hälfte-Pipe und die Einrad verwendete Objekt ist definiert als Feld in der [ `UnicycleHalfPipePage` ]() Klasse. Wird auch definiert die `SKPath` Objekt für die Einrad:

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

Die Klasse enthält die standardmäßigen Außerkraftsetzungen von der `OnAppearing` und `OnDisappearing` Methoden für die Animation. Die `PaintSurface` Handler erstellt den Pfad für die Hälfte-Pipe und zeichnet Sie es. Ein `SKPathMeasure` Objekt wird dann basierend auf diesen Pfad erstellt:

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

Die `PaintSurface` Handler berechnet einen Wert von `t` , die geht von 0 auf 1 alle fünf Sekunden. Es verwendet dann die `Math.Cos` Funktion, die auf den Wert konvertieren `t` , die reicht von 0 auf 1 und 0 (null) zurück, wobei entspricht 0 dem Einrad am Anfang auf der oberen linken Ecke, während der Einrad oben rechts 1 entspricht. Die Kosinusfunktion bewirkt, dass die Geschwindigkeit der langsamste am oberen Rand der Pipe und schnellste im unteren Bereich werden.

Beachten Sie, dass dieser Wert der `t` muss für das erste Argument für die Pfadlänge multipliziert `GetMatrix`. Klicken Sie dann auf die Matrix angewendet wird die `SKCanvas` -Objekt zum Zeichnen des Einrad Pfads.

## <a name="enumerating-the-path"></a>Den Pfad auflisten

Zwei Klassen von eingebetteten `SKPath` ermöglichen es Ihnen, den Inhalt des Pfads aufzulisten. Diese Klassen sind [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) und [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). Die beiden Klassen sind sehr ähnlich, aber `SKPath.Iterator` können Elemente im Pfad mit der Länge 0 (null), oder in der Nähe der Länge 0 beseitigen. Die `RawIterator` wird im folgenden Beispiel verwendet.

Sie erhalten ein Objekt des Typs `SKPath.RawIterator` durch Aufrufen der [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) Methode `SKPath`. Auflisten von über den Pfad wird erreicht, indem Sie wiederholt Aufrufen der [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) Methode. Ein Array von vier an diesen weitergegeben `SKPoint` Werte:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

Die `Next` Methodenrückgabe ein Mitglied der [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) Enumeration. Diese Werte geben an, die bestimmten Zeichnen-Befehl im Pfad. Die Anzahl der gültigen Punkte, die in das Array eingefügten hängt das Verb ab:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) mit einem einzigen Punkt
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) mit zwei Punkten
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) mit vier Punkte
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) mit den drei Punkten
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) mit den drei Punkten (und auch aufrufen, die [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) Methode für die Gewichtung)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) durch einen Punkt
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

Die `Done` Verb gibt an, dass die Enumeration abgeschlossen ist.

Beachten Sie, dass es keine `Arc` Verben. Dies gibt an, dass alle Bögen in Bézier-Kurven, bei dem Pfad hinzugefügt konvertiert werden.

Einige der Informationen in den `SKPoint` Array ist redundant. Z. B. wenn ein `Move` Verb folgt eine `Line` Verb, und klicken Sie dann auf das erste der beiden Punkte, die gemeinsam mit der `Line` ist identisch mit der `Move` zeigen. In der Praxis ist diese Redundanz sehr hilfreich. Wenn erhalten Sie eine `Cubic` Verb, ist es beiliegen, bis alle vier Punkte, die die kubische Bézier-Kurve zu definieren. Sie müssen nicht die aktuelle Position, die durch die vorherigen Verb beibehalten werden sollen.

Verbs problematisch ist jedoch `Close`. Mit diesem Befehl zeichnet eine gerade Linie von der aktuellen Position, an den Anfang der Kontur hergestellt zuvor von der `Move` Befehl. Im Idealfall der `Close` Verb sollte nur einem Punkt, anstatt diese zwei Punkte enthalten. Schlimmer noch: ist, die den Punkt begleitende der `Close` Verb ist immer (0, 0). Dies bedeutet, dass wenn Sie einen Pfad durchlaufen haben, Sie wahrscheinlich behalten müssen die `Move` Punkt- und der aktuellen Position.

Die statische [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) Klasse enthält mehrere Methoden, die die drei Typen von Bézier-Kurven in einer Reihe von kleinen gerade Linien konvertieren, der die Kurve zu ermitteln. (Die parametrischen Formeln wurden so präsentiert, in dem Artikel [ **drei Typen von Bézier-Kurven**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) Die `Interpolate` Methode gliedert einer geraden Linie in zahlreichen kurze Zeilen, die nur eine Einheit lang sind:

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

Alle diese Methoden werden von der Erweiterungsmethode verwiesen `CloneWithTransform` unten angezeigt. Diese Methode klont einen Pfad durch Auflisten der Path-Befehle aus, und erstellen einen neuen Pfad basierend auf den Daten. Allerdings der neue Pfad besteht nur aus `MoveTo` und `LineTo` aufrufen. Alle Kurven und gerade Linien sind auf eine Reihe von kleinen Zeilen reduziert.

Beim Aufrufen von `CloneWithTransform`, übergeben Sie an die Methode eine `Func<SKPoint, SKPoint>`, dies ist eine Funktion mit ein `SKPaint` Parameter, der zurückgegeben ein `SKPoint` Wert. Diese Funktion wird für jeden Punkt um eine benutzerdefinierte algorithmische Transformation anzuwenden:

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

Da der geklonte Pfad denkbar geraden Zeilen reduziert wird, kann die Transformationsfunktion gerade Linien, Kurven konvertieren.

Beachten Sie, dass die Methode den ersten Punkt der einzelnen Kontur in die Variable mit dem Namen behält `firstPoint` und die aktuelle Position nach einzelnen Zeichnen-Befehl in der Variablen `lastPoint`. Diese sind erforderlich zum Erstellen der letzten schließendes Zeile eine `Close` Verb festgestellt wird.

Die **GlobularText** Beispiel verwendet diese Erweiterungsmethode scheinbar Umbrechen von Text um eine Hemisphäre sind in einem 3D-Effekt:

[![](information-images/globulartext-small.png "Dreifacher Screenshot der Seite Globular Text")](information-images/globulartext-large.png#lightbox "dreifacher Screenshot der Seite Globular Text")

Die [ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Klassenkonstruktor führt dieser Transformation. Erstellt ein `SKPaint` -Objekt für den Text ein, und klicken Sie dann erhält ein `SKPath` -Objekt aus der `GetTextPath` Methode. Dies ist der Pfad zum Übergeben der `CloneWithTransform` Erweiterungsmethode zusammen mit einer Transform-Funktion:

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

Transform-Funktion berechnet zuerst die beiden Werte, die mit dem Namen `longitude` und `latitude` , von – π/2 oben und links neben dem Text bis π/2 auf den rechten und unteren Rand der Text hin. Der Wertebereich nicht visuell zufriedenstellend, damit sie durch Multiplizieren mit 0,75 reduziert werden. (Verwenden Sie den Code ohne diese Anpassungen. Der Text wird an die North "und" Süd Polen undurchsichtig und an den Seiten zu dünn.) Diese dreidimensionalen sphärischen Koordinaten werden in einem zweidimensionalen konvertiert `x` und `y` Koordinaten von standard-Formeln.

Der neue Pfad wird als Feld gespeichert. Die `PaintSurface` muss Handler dann lediglich einen zentrieren und den Pfad für die Anzeige auf dem Bildschirm zu skalieren:

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

Dies ist ein sehr vielseitig Verfahren. Wenn das Array von Pfad Effekte in beschrieben die [ **Pfad Effekte** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) Artikel nicht sehr umfassen, etwa Sie sind enthalten, muss dies ist eine Möglichkeit zum Füllen der Lücken.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
