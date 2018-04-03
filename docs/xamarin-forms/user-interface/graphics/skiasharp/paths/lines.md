---
title: Linien und Strich Caps
description: Erfahren Sie, wie SkiaSharp verwenden Sie zum Zeichnen von Linien mit anderen Strichabschlüssen
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 96b8a990f4644d5e4c9c8ffe6cdb6c173c50657c
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="lines-and-stroke-caps"></a>Linien und Strich Caps

_Erfahren Sie, wie SkiaSharp verwenden Sie zum Zeichnen von Linien mit anderen Strichabschlüssen_

Rendern einer einzelnen Zeile wird in SkiaSharp unterscheidet sich von einer Reihe verbundener gerader Linien zu rendern. Auch beim Zeichnen von Linien, allerdings ist es oft erforderlich, damit den Zeilen eine bestimmte Strichbreite und desto breiter die Linie erhält, umso wichtiger ist die Darstellung des Endes der Zeilen, die aufgerufen der *Strich Cap*:

![](lines-images/strokecapsexample.png "Die drei Strich Caps-Optionen")

Zum Zeichnen von Linien, `SKCanvas` definiert eine einfache [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) Methode, deren Argumente angeben, die Start- und End-Koordinaten der Zeile mit, einem `SKPaint` Objekt:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Wird standardmäßig die `StrokeWidth` -Eigenschaft des neu instanziierten `SKPaint` Objekt ist 0, d. h. dieselbe Wirkung wie der Wert 1 hat in einer Zeile von einem Pixel in Stärke rendern. Dies wird auf hochauflösende Geräten wie Telefonen sehr dünne angezeigt, sodass Sie wahrscheinlich festlegen möchten die `StrokeWidth` auf einen höheren Wert. Aber wenn Sie das Zeichnen von Linien eine beträchtliche Anzahl an Dicke anfangen, ein anderes Problem auslöst: wie den Beginn und Ende #kommentare thick gerendert werden soll?

Die Darstellung der den Beginn und Ende der Zeilen wird aufgerufen, eine *Linienende* oder Skia, eine *Strich Cap*. Das Wort "Abdeckung" in diesem Kontext bezieht sich auf eine Art von Hat &mdash; etwas, das am Ende der Zeile befindet. Festlegen der [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) Eigenschaft von der `SKPaint` -Objekts auf einen der folgenden Elemente von der [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) Enumeration:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (Standard)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Diese werden am besten mit einem Beispielprogramm veranschaulicht. Der zweite Abschnitt der Startseite von der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm beginnt mit einer Seite mit der Bezeichnung **Strich Caps** basierend auf den [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) Klasse. Diese Seite definiert eine `PaintSurface` -Ereignishandler, der die drei Elemente von durchläuft die `SKStrokeCap` Enumeration, die beide den Namen der dem Enumerationsmember anzeigen und Zeichnen einer Linie, Strich-Cap mit:

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

Für jedes Mitglied der `SKStrokeCap` -Enumeration, die Handler zeichnet zwei Zeilen mit Strichstärke 50 Pixeln und einer anderen Zeile im Vordergrund mit Strichstärke 2 Pixel positioniert. Diese zweite Zeile dient die geometrische Start- und Ende der Zeile, die unabhängig von der Linienstärke und eine Obergrenze Strich veranschaulichen:

[![](lines-images/strokecaps-small.png "Dreifacher Screenshot der Seite Strich Caps")](lines-images/strokecaps-large.png#lightbox "dreifacher Screenshot der Seite Strich Caps")

Wie Sie sehen können, die `Square` und `Round` Strich Caps effektiv die Länge der Zeile erweitern, indem Sie halbe Strichbreite am Anfang der Zeile und erneut am Ende. Diese Erweiterung ist wichtig, wenn es erforderlich, die Dimensionen eines gerenderten Graphics-Objekts zu ermitteln ist.

Die `SKCanvas` Klasse enthält auch eine andere Methode für mehrere Linien zu zeichnen, das etwas spezielle ist:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

Die `points` Parameter ist ein Array von `SKPoint` Werte und `mode` ist ein Mitglied der [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) -Enumeration, die drei Member verfügt:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) um die einzelnen Punkte zu rendern.
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) Verbindung für jedes Paar von Punkten
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) die Verbindung alle aufeinander folgenden Punkten

Die **mehrzeiligen** dieser Methode veranschaulicht. Die [ `MultipleLinesPage` XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) instanziiert zwei `Picker` Ansichten, mit denen Sie auswählen, ein Mitglied der `SKPointMode` Enumeration und Mitglied der `SKStrokeCap` Enumeration:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
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
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Die `SelectedIndexChanged` Handler für beide `Picker` Ansichten einfach erklärt die `SKCanvasView` Objekt:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Dieser Handler für das Vorhandensein des prüfen muss die `SKCanvasView` Objekt, da der erste des ereignishandlers ist wird aufgerufen, wenn die `SelectedIndex` Eigenschaft von der `Picker` auf 0 festgelegt ist, die XAML-Datei, und in diesem Fall vor der `SKCanvasView` instanziiert wurde.

Die `PaintSurface` Handler greift auf eine generische Methode zum Abrufen von zwei ausgewählten Elemente aus der `Picker` Ansichten und sie in der Enumerationswerte konvertieren:

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
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

Der Screenshot zeigt eine Vielzahl von `Picker` Auswahl wird auf drei Plattformen:

[![](lines-images/multiplelines-small.png "Dreifacher Screenshot der Seite mehrere Zeilen")](lines-images/multiplelines-large.png#lightbox "dreifacher Screenshot der Seite mehrere Zeilen")

Das iPhone auf der linken zeigt wie die `SKPointMode.Points` bewirkt, dass Enumerationsmember `DrawPoints` zum Rendern aller Punkte in der `SKPoint` array als Quadrat ist das Linienende `Butt` oder `Square`. Kreise werden gerendert, wenn das Linienende ist `Round`.

Wenn Sie stattdessen `SKPointMode.Lines`, wie auf dem Android Bildschirm in der Mitte dargestellt die `DrawPoints` Methode zeichnet eine verbindende Linie zwischen jedem Paar von `SKPoint` Werte, die Abdeckung angegebene Zeile in diesem Fall mit `Round`.

Das Windows mobile-Gerät zeigt das Ergebnis der `SKPointMode.Polygon` Wert. Wird eine Linie zwischen den aufeinanderfolgenden Punkten im Array, aber wenn Sie sehr genau hinsehen, sehen Sie, dass diese Zeilen nicht miteinander verbunden sind. Jede dieser separaten Zeilen beginnt und endet mit dem angegebenen Linienende. Bei Auswahl der `Round` Caps, scheinen die Zeilen verbunden sein, aber sie eigentlich nicht verbunden.

Ob Zeilen sind oder nicht verbunden ist ein wesentlicher Aspekt der Arbeit mit Grafikpfade.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
