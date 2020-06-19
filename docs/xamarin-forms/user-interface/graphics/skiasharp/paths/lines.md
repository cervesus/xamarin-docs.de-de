---
title: Linien und Strichenden
description: In diesem Artikel wird erläutert, wie Sie skiasharp zum Zeichnen von Linien mit unterschiedlichen Strich Strichen in Xamarin.Forms Anwendungen verwenden, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 87b97ad913e08c42d16bbf055f168c07b9bd60e8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137204"
---
# <a name="lines-and-stroke-caps"></a>Linien und Strichenden

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie skiasharp zum Zeichnen von Linien mit unterschiedlichen Strich Strichen verwenden können._

In skiasharp unterscheidet sich das Rendern einer einzelnen Zeile stark vom Rendern einer Reihe verbundener gerader Linien. Auch wenn einzelne Zeilen gezeichnet werden, ist es oft erforderlich, den Linien eine bestimmte Strichbreite zuzuweisen. Wenn diese Linien breiter werden, wird die Darstellung der Zeilenenden ebenfalls wichtig. Die Darstellung des Endes der Zeile wird als *Strich*Abdeckung bezeichnet:

![](lines-images/strokecapsexample.png "The three stroke caps options")

Zum Zeichnen einzelner Zeilen `SKCanvas` definiert eine einfache [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) Methode, deren Argumente die Start-und Endkoordinaten der Zeile mit einem `SKPaint` Objekt angeben:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Standardmäßig ist die- [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) Eigenschaft eines neu instanziierten `SKPaint` Objekts 0. Dies hat dieselbe Auswirkung wie der Wert 1 beim Rendern einer Zeile mit einem Pixel in der Stärke. Dies erscheint sehr schlank auf Geräten mit hoher Auflösung, wie z. b. Smartphones, sodass Sie den `StrokeWidth` Wert wahrscheinlich auf einen größeren Wert festlegen möchten. Wenn Sie jedoch mit dem Zeichnen von Linien einer hervor generischen Stärke beginnen, wird ein anderes Problem ausgelöst: wie sollen die Starts und enden dieser dicken Linien gerendert werden?

Die Darstellung des Starts und der Zeilenenden wird als *Linien* Abdeckung oder in Skia *als Strich Abdeckung bezeichnet.* Das Wort "Cap" in diesem Kontext bezieht sich auf eine Art von hat &mdash; etwas, das sich am Ende der Linie befindet. Legen Sie die- [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap) Eigenschaft des- `SKPaint` Objekts auf einen der folgenden Member der- [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) Enumeration fest:

- `Butt`(Standard)
- `Square`
- `Round`

Diese werden am besten mit einem Beispielprogramm veranschaulicht. Der Abschnitt **skiasharp-Linien und Pfade** des [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programms beginnt mit einer Seite, die auf der-Klasse mit dem Titel " **Stroke Caps** " angezeigt wird [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) . Diese Seite definiert einen- `PaintSurface` Ereignishandler, der die drei Member der- `SKStrokeCap` Enumeration durchläuft, wobei sowohl der Name des Enumerationsmembers als auch das Zeichnen einer Linie mit dieser Strich Abdeckung angezeigt wird:

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

Für jeden Member der- `SKStrokeCap` Enumeration zeichnet der Handler zwei Zeilen, eine mit einer Strichstärke von 50 Pixeln und eine andere Zeile, die mit einer Strichstärke von zwei Pixeln positioniert ist. Diese zweite Zeile soll den geometrischen Anfang und das Ende der Linie unabhängig von der Linienstärke und der Strich Abdeckung veranschaulichen:

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

Wie Sie sehen können, wird `Square` die `Round` Länge der Linie durch die Striche-und-Striche effektiv um die Hälfte der Strichbreite am Anfang der Zeile und wieder am Ende erweitert. Diese Erweiterung wird wichtig, wenn es erforderlich ist, die Dimensionen eines gerenderten Grafik Objekts zu bestimmen.

Die- `SKCanvas` Klasse enthält auch eine andere Methode zum Zeichnen mehrerer Zeilen, die etwas Besonderes ist:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

Der `points` -Parameter ist ein Array von `SKPoint` -Werten und `mode` ist ein Member der- [`SKPointMode`](xref:SkiaSharp.SKPointMode) Enumeration, die über drei Member verfügt:

- `Points`so Rendering Sie die einzelnen Punkte
- `Lines`So verbinden Sie jedes Paar von Punkten
- `Polygon`So verbinden Sie alle aufeinander folgenden Punkte

Diese Methode wird auf der Seite **mehrere Zeilen** veranschaulicht. Die [**multiplelinespage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml) -Datei instanziiert zwei `Picker` Sichten, mit denen Sie einen Member der `SKPointMode` -Enumeration und einen Member der- `SKStrokeCap` Enumeration auswählen können:

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

Beachten Sie, dass die skiasharp-Namespace Deklarationen etwas unterschiedlich sind, da der `SkiaSharp` Namespace erforderlich ist, um auf die Member der `SKPointMode` `SKStrokeCap` Enumerationen und zu verweisen. Der- `SelectedIndexChanged` Handler für beide `Picker` Sichten macht das Objekt einfach ungültig `SKCanvasView` :

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Dieser Handler muss das vorhanden sein des-Objekts überprüfen `SKCanvasView` , da der-Ereignishandler zuerst aufgerufen wird, wenn die- `SelectedIndex` Eigenschaft von `Picker` in der XAML-Datei auf 0 festgelegt ist, und vor der `SKCanvasView` instanziiert wird.

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

Die Screenshots zeigen eine Vielzahl von `Picker` Auswahlmöglichkeiten:

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

Das iPhone auf der linken Seite zeigt, wie das `SKPointMode.Points` Enumerationsmember bewirkt `DrawPoints` , dass alle Punkte im `SKPoint` Array als Quadrat darstellen, wenn das Linien Ende `Butt` oder ist `Square` . Kreise werden gerendert, wenn das Linien Ende ist `Round` .

Der Android-Screenshot zeigt das Ergebnis von `SKPointMode.Lines` . Die- `DrawPoints` Methode zeichnet eine Linie zwischen jedem Wertepaar `SKPoint` , wobei das angegebene Linien Ende verwendet wird, in diesem Fall `Round` .

Wenn Sie stattdessen verwenden `SKPointMode.Polygon` , wird eine Linie zwischen den aufeinander folgenden Punkten im Array gezeichnet. Wenn Sie jedoch sehr genau sehen, werden Sie feststellen, dass diese Zeilen nicht verbunden sind. Jede dieser separaten Zeilen beginnt und endet mit dem angegebenen Zeilenende. Wenn Sie die Ober `Round` Grenzen auswählen, werden die Zeilen möglicherweise als verbunden angezeigt, aber Sie sind nicht verbunden.

Ob Zeilen verbunden oder nicht verbunden sind, ist ein wichtiger Aspekt bei der Arbeit mit Grafik Pfaden.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
