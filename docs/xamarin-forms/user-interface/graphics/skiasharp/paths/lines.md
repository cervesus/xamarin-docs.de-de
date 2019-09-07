---
title: Linien und Strichenden
description: In diesem Artikel wird erläutert, wie SkiaSharp, die zum Zeichnen von Linien mit anderen Strichenden in Xamarin.Forms-Anwendungen verwenden, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: cc62ca4656a845a261c56424aa1ea1331c994994
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759215"
---
# <a name="lines-and-stroke-caps"></a>Linien und Strichenden

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie SkiaSharp verwenden Sie zum Zeichnen von Linien mit anderen Strichenden_

In SkiaSharp unterscheidet rendern eine einzelne Zeile aus einer Reihe verbundener gerader Linien zu rendern. Selbst wenn einzelne Zeilen zu zeichnen, ist es jedoch oft erforderlich, um den Zeilen zu eine bestimmten Strichbreite gewähren. Sobald diese Zeilen breiter sind, wird die Darstellung der Enden der Zeilen auch wichtig. Die Darstellung der das Ende der Zeile wird aufgerufen, die *Stroke Cap*:

![](lines-images/strokecapsexample.png "Die drei Stroke Caps-Optionen")

Für das Zeichnen von Linien, `SKCanvas` definiert eine einfache [ `DrawLine` ](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) Methode, deren Argumente angeben, der Anfangs- und Endkoordinaten der Linie mit, einem `SKPaint` Objekt:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

In der Standardeinstellung die [ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth) Eigenschaft des neu instanziierten `SKPaint` Objekt ist 0 (null) und denselben Effekt wie der Wert 1 hat in beim Rendern einer Zeile von einem Pixel im Stärke. Dies scheint sehr dünne auf Geräten wie Smartphones, mit hoher Auflösung, sollten Sie wahrscheinlich zum Festlegen der `StrokeWidth` auf einen höheren Wert. Wenn Sie jedoch mit dem Zeichnen von Linien mit einer hervor generischen Stärke beginnen, wird ein anderes Problem ausgelöst: Wie sollen die Starts und enden dieser Thick Lines gerendert werden?

Die Darstellung der den Beginn und Ende von Zeilen wird aufgerufen, eine *Linienende* oder Skia, eine *Stroke Cap*. Das Wort "Obergrenze" in diesem Kontext bezieht sich auf eine Art von Hat &mdash; etwas, das auf das Ende der Zeile befindet. Festlegen der [ `StrokeCap` ](xref:SkiaSharp.SKPaint.StrokeCap) Eigenschaft der `SKPaint` -Objekts auf einen der folgenden Elemente von der [ `SKStrokeCap` ](xref:SkiaSharp.SKStrokeCap) Enumeration:

- `Butt` (Standard)
- `Square`
- `Round`

Diese werden am besten mit einem Beispiel veranschaulicht. Die **SkiaSharp-Linien und-Pfade** Teil der [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Programm beginnt mit einer Seite mit dem Titel **Strichenden** basierend auf der [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) Klasse. Auf dieser Seite wird eine `PaintSurface` -Ereignishandler, der die drei Elemente von durchläuft die `SKStrokeCap` Enumeration, die den Namen des Enumerationsmembers angezeigt, und Zeichnen einer Linie mit diese Obergrenze, Strich:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Für jedes Element der `SKStrokeCap` Enumeration, der Handler zeichnet zwei Zeilen, die mit einer Strichstärke von 50 Pixeln und eine neue Zeile, die im Vordergrund positioniert wird, mit der Strichstärke zwei Pixel. Diese zweite Zeile soll lediglich die geometrische Start und Ende der Zeile, die unabhängig von der Linienstärke und eine Obergrenze Strich zu veranschaulichen:

[![](lines-images/strokecaps-small.png "Dreifacher Screenshot der Seite Strichenden")](lines-images/strokecaps-large.png#lightbox "dreifachen Screenshot der Seite Strichenden")

Wie Sie sehen können, die `Square` und `Round` Strichenden effektiv die Länge der Zeile erweitern, indem Sie halbe Strichbreite am Anfang der Zeile und erneut am Ende. Diese Erweiterung wird wichtig, wenn es erforderlich, um zu bestimmen, die Abmessungen eines gerenderten Graphics-Objekts ist.

Die `SKCanvas` -Klasse enthält auch eine andere Methode für das Zeichnen von mehreren Zeilen, die etwas ungewöhnliche ist:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

Die `points` -Parameter ist ein Array von `SKPoint` Werte und `mode` ist ein Mitglied der [ `SKPointMode` ](xref:SkiaSharp.SKPointMode) -Enumeration, die hat drei Member:

- `Points` um die einzelnen Punkte zu rendern.
- `Lines` jedem Standortpaar der Verbindung
- `Polygon` die Verbindung alle aufeinander folgenden Punkten

Die **mehrere Zeilen** Seite veranschaulicht diese Methode. Die [ **MultipleLinesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) Datei instanziiert zwei `Picker` Ansichten, mit denen Sie auswählen, ein Mitglied der `SKPointMode` Enumeration und ein Mitglied der `SKStrokeCap` Enumeration:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPointMode}">
                    <x:Static Member="skia:SKPointMode.Points" />
                    <x:Static Member="skia:SKPointMode.Lines" />
                    <x:Static Member="skia:SKPointMode.Polygon" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Beachten Sie, dass die Namespacedeklarationen SkiaSharp da ein wenig anders sind die `SkiaSharp` Namespace ist erforderlich, um auf die Member der der `SKPointMode` und `SKStrokeCap` Enumerationen. Die `SelectedIndexChanged` Handler für beide `Picker` Ansichten einfach erklärt die `SKCanvasView` Objekt:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Dieser Handler muss prüfen, ob das Vorhandensein der `SKCanvasView` Objekt, da der erste des ereignishandlers ist wird aufgerufen, wenn die `SelectedIndex` Eigenschaft der `Picker` auf 0 festgelegt ist, in der XAML-Datei liegt, die die `SKCanvasView` instanziiert wurde.

Die `PaintSurface` Handler Ruft die beiden Enumerationswerte aus der `Picker` Ansichten:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = (SKPointMode)pointModePicker.SelectedItem;
    canvas.DrawPoints(pointMode, points, paint);
}
```

Die Screenshots zeigen eine Vielzahl von `Picker` Auswahl:

[![](lines-images/multiplelines-small.png "Dreifacher Screenshot der Seite mehrere Zeilen")](lines-images/multiplelines-large.png#lightbox "dreifachen Screenshot der Seite mehrere Zeilen")

Das iPhone auf der linken wie die `SKPointMode.Points` bewirkt, dass Enumerationsmember `DrawPoints` zum Rendern jeder der Punkte in der `SKPoint` array als ein Quadrat ist das Linienende `Butt` oder `Square`. Kreise werden gerendert, wenn das Linienende `Round`.

Wenn Sie stattdessen verwenden `SKPointMode.Lines`auf dem Android-Bildschirm in der Mitte, siehe die `DrawPoints` Methode zeichnet eine Linie zwischen jedem Tabellenpaar `SKPoint` Werte, verwenden die Obergrenze für die angegebene Zeile, in diesem Fall `Round`.

Der UWP-Screenshot zeigt das Ergebnis der `SKPointMode.Polygon` Wert. Wird eine Linie zwischen den aufeinander folgenden Punkten im Array, aber wenn Sie sehr genau hinsehen, sehen Sie, dass diese Zeilen nicht verbunden sind. Jede dieser separaten Zeilen beginnt und endet mit dem angegebenen Linienende. Bei Auswahl der `Round` Großbuchstaben, scheinen die Zeilen verbunden sein, aber sie eigentlich nicht verbunden sind.

Ist ein entscheidender Aspekt für die Arbeit mit Grafikpfade, ob Zeilen oder nicht verbunden sind.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
