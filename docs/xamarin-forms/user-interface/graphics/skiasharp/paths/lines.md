---
title: Linien und Strichenden
description: In diesem Artikel wird erläutert, wie SkiaSharp, die zum Zeichnen von Linien mit anderen Strichenden in Xamarin.Forms-Anwendungen verwenden, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 9aaecb8c63ff28111097dce81954f523b4c7731b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725212"
---
# <a name="lines-and-stroke-caps"></a>Linien und Strichenden

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie skiasharp zum Zeichnen von Linien mit unterschiedlichen Strich Strichen verwenden können._

In SkiaSharp unterscheidet rendern eine einzelne Zeile aus einer Reihe verbundener gerader Linien zu rendern. Selbst wenn einzelne Zeilen zu zeichnen, ist es jedoch oft erforderlich, um den Zeilen zu eine bestimmten Strichbreite gewähren. Sobald diese Zeilen breiter sind, wird die Darstellung der Enden der Zeilen auch wichtig. Die Darstellung des Endes der Zeile wird als *Strich*Abdeckung bezeichnet:

![](lines-images/strokecapsexample.png "The three stroke caps options")

Zum Zeichnen einzelner Zeilen definiert `SKCanvas` eine einfache [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) Methode, deren Argumente die Start-und Endkoordinaten der Zeile mit einem `SKPaint`-Objekt angeben:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Standardmäßig ist die [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) -Eigenschaft eines neu instanziierten `SKPaint`-Objekts 0 (null), was denselben Effekt hat wie der Wert 1 beim Rendern einer Zeile mit einem Pixel in der Stärke. Dies erscheint sehr schlank auf Geräten mit hoher Auflösung, wie z. b. Smartphones, sodass Sie den `StrokeWidth` wahrscheinlich auf einen größeren Wert festlegen möchten. Aber sobald Sie das Zeichnen von Linien mit einer beträchtliche Dicke starten, ein weiteres Problem auslöst: wie den Beginn und Ende diese dicken Zeilen gerendert werden soll?

Die Darstellung des Starts und der Zeilenenden wird als *Linien* Abdeckung oder in Skia *als Strich Abdeckung bezeichnet.* Das Wort "Cap" in diesem Kontext bezieht sich auf eine Art von hat &mdash; etwas, das sich am Ende der Linie befindet. Legen Sie die [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap) -Eigenschaft des `SKPaint`-Objekts auf einen der folgenden Member der [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) -Enumeration fest:

- `Butt` (Standard)
- `Square`
- `Round`

Diese werden am besten mit einem Beispiel veranschaulicht. Der Abschnitt **skiasharp-Linien und Pfade** des [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programms beginnt mit einer Seite mit dem Titel **Stroke Caps** , die auf der [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) -Klasse basiert. Diese Seite definiert einen `PaintSurface` Ereignishandler, der die drei Member der `SKStrokeCap` Enumeration durchläuft, wobei sowohl der Name des Enumerationsmembers als auch das Zeichnen einer Linie mit der folgenden Strich Abdeckung angezeigt wird:

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

Für jedes Element der `SKStrokeCap` Enumeration zeichnet der Handler zwei Zeilen, eine mit einer Strichstärke von 50 Pixeln und eine andere Zeile, die mit einer Strichstärke von zwei Pixeln positioniert ist. Diese zweite Zeile soll lediglich die geometrische Start und Ende der Zeile, die unabhängig von der Linienstärke und eine Obergrenze Strich zu veranschaulichen:

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

Wie Sie sehen können, wird die Länge der Linie durch die `Square`-und `Round` Strich Kappen effektiv um die Hälfte der Strichbreite am Anfang der Zeile und wieder am Ende erweitert. Diese Erweiterung wird wichtig, wenn es erforderlich, um zu bestimmen, die Abmessungen eines gerenderten Graphics-Objekts ist.

Die `SKCanvas`-Klasse enthält auch eine andere Methode zum Zeichnen mehrerer Zeilen, die etwas Besonderes ist:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

Der `points`-Parameter ist ein Array von `SKPoint` Werten, und `mode` ist ein Member der [`SKPointMode`](xref:SkiaSharp.SKPointMode) -Enumeration, die drei Member hat:

- `Points` zum Rendering der einzelnen Punkte
- `Lines`, um jedes Paar von Punkten zu verbinden.
- `Polygon`, um alle aufeinander folgenden Punkte zu verbinden.

Diese Methode wird auf der Seite **mehrere Zeilen** veranschaulicht. Die [**multiplelinespage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml) -Datei instanziiert zwei `Picker` Sichten, mit denen Sie einen Member der `SKPointMode` Enumeration und einen Member der `SKStrokeCap`-Enumeration auswählen können:

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

Beachten Sie, dass die skiasharp-Namespace Deklarationen etwas unterschiedlich sind, da der `SkiaSharp`-Namespace erforderlich ist, um auf die Member der `SKPointMode` und `SKStrokeCap` Enumerationen zu verweisen. Der `SelectedIndexChanged` Handler für beide `Picker` Sichten macht das `SKCanvasView` Objekt einfach ungültig:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Dieser Handler muss prüfen, ob das `SKCanvasView` Objekt vorhanden ist, da der Ereignishandler zuerst aufgerufen wird, wenn die `SelectedIndex`-Eigenschaft des `Picker` in der XAML-Datei auf 0 festgelegt ist, und vor der instanziierten `SKCanvasView`.

Der `PaintSurface` Handler Ruft die beiden Enumerationswerte aus den `Picker` Sichten ab:

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

Die Screenshots zeigen eine Reihe von `Picker` Auswahl:

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

Das iPhone auf der linken Seite zeigt, wie das `SKPointMode.Points` Enumerationsmember bewirkt, dass `DrawPoints` jeden der Punkte im `SKPoint` Array als quadratisches Element, wenn das Linien Ende `Butt` oder `Square`ist. Kreise werden gerendert, wenn das Linien Ende `Round`ist.

Der Android-Screenshot zeigt das Ergebnis der `SKPointMode.Lines`. Die `DrawPoints`-Methode zeichnet eine Linie zwischen jedem Paar von `SKPoint` Werten, wobei das angegebene Linien Ende verwendet wird (in diesem Fall `Round`).

Wenn Sie stattdessen `SKPointMode.Polygon`verwenden, wird eine Linie zwischen den aufeinander folgenden Punkten im Array gezeichnet. Wenn Sie jedoch sehr genau sehen, werden Sie feststellen, dass diese Zeilen nicht verbunden sind. Jede dieser separaten Zeilen beginnt und endet mit dem angegebenen Linienende. Wenn Sie die `Round` Caps auswählen, werden die Zeilen möglicherweise als verbunden angezeigt, aber Sie sind nicht verbunden.

Ist ein entscheidender Aspekt für die Arbeit mit Grafikpfade, ob Zeilen oder nicht verbunden sind.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
