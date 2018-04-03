---
title: Die Typen der Pfad ausfüllen
description: Ermitteln Sie die verschiedenen Effekte mit SkiaSharp Pfad ausfüllen Typen möglich
ms.topic: article
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 16471c10028c2e4bc91e1cfc1bddeed153680d49
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="the-path-fill-types"></a>Die Typen der Pfad ausfüllen

_Ermitteln Sie die verschiedenen Effekte mit SkiaSharp Pfad ausfüllen Typen möglich_

Zwei Konturen in einem Pfad können sich überschneiden, und die Zeilen, die eine einzelne Kontur bilden können sich überschneiden. Alle eingeschlossenen Bereich potenziell ausgefüllt werden kann, aber Sie möchten nicht, dass alle eingeschlossenen Bereiche zu füllen. Im Folgenden ein Beispiel:

![](fill-types-images/filltypeexample.png "Fünf Spitzen Stern teilweise Filles")

Sie haben ein wenig steuern. Der Algorithmus aufgefüllt werden, unterliegt die [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) Eigenschaft `SKPath`, das Sie auf einen Member Festlegen der [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) Enumeration:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), der Standardwert
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Die Algorithmen winding und gerade-ungerade bestimmen, ob alle eingeschlossenen Bereich gefüllt oder nicht basierend auf einer hypothetischen Linie von diesen Bereich unendlich gefüllt ist. Dieser Zeile überschreitet, einer oder mehreren Grenze Zeilen, die den Pfad bilden. Mit dem winding Modus ist die Anzahl der in einer Richtung Lastenausgleich, die Anzahl der Zeilen in die andere Richtung, und klicken Sie dann auf den Bereich gezeichnet gezeichneten Grenzlinien verwendet nicht gefüllt. Andernfalls wird der Bereich gefüllt. Der gerade-ungerade Algorithmus füllt einen Bereich aus, wenn die Anzahl der Zeilen Grenze ungerade ist.

Mit viele routinemäßige Pfade füllt der winding Algorithmus häufig die eingeschlossenen Bereiche eines Pfads. Die gerade-ungerade Algorithmus erzeugt in der Regel Weitere interessante Ergebnisse.

Ist das klassische Beispiel einen Stern fünf gezeigt, wie in der **Five-Pointed Stern** Seite. Die [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) Datei instanziiert zwei `Picker` Ansichten, um den Pfad auszuwählen ausfüllen, Typ und gibt an, ob der Pfad gezeichnet oder gefüllt ist oder beide, und in welcher Reihenfolge:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei verwendet beide `Picker` Werte einen Stern fünf Spitzen gezeichnet werden soll:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

In der Regel wird die Pfad-Fülltyp betrifft nur fortgesetzt werden kann und keine Striche, doch die beiden `Inverse` Modi Auswirkungen auf Flächen und Konturen. Für fortgesetzt werden kann, die beiden `Inverse` Typen Bereiche füllen oppositely, damit Sie der Bereich außerhalb der Stern gefüllt ist. Für Striche, die beiden `Inverse` Typen Farbe alles mit Ausnahme der Kontur. Verwendung dieser Füllungstypen inverse kann einige ungerade Effekte erzeugt, wie der iOS-Screenshot veranschaulicht:

[![](fill-types-images/fivepointedstar-small.png "Dreifacher Screenshot der Seite Five-Pointed Stern")](fill-types-images/fivepointedstar-large.png#lightbox "dreifacher Screenshot der Seite Five-Pointed Stern")

Die Android- und Windows mobile-Screenshots zeigen die üblichen gerade-ungerade und winding Effekte, aber die Reihenfolge der Kontur und Füllung wirkt sich auch auf die Ergebnisse.

Winding Algorithmus ist abhängig von der Richtung der Linien gezeichnet werden. In der Regel können Wenn Sie einen Pfad erstellen, Sie die entsprechende Richtung steuern, wie Sie angeben, dass Zeilen, die von einem Punkt in eine andere gezeichnet werden. Allerdings die `SKPath` Klasse definiert auch Methoden wie z. B. `AddRect` und `AddCircle` , die gesamte Kontur zeichnen. Um zu steuern, wie diese Objekte gezeichnet werden, enthalten die Methoden einen Parameter vom Typ [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), die zwei Member aufweist:

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Die Methoden in `SKPath` , enthalten ein `SKPathDirection` Parameter Geben Sie ihm den Standardwert `Clockwise`.

Die **überlappende Kreise** Seite wird ein Pfad mit vier überlappende Kreise mit einem gerade-ungerade Pfad Fülltyp erstellt:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Es handelt sich um eine interessante Image mit einem Minimum von Code erstellt:

[![](fill-types-images/overlappingcircles-small.png "Dreifacher Screenshot der Seite überlappende Kreise")](fill-types-images/overlappingcircles-large.png#lightbox "dreifacher Screenshot der Seite überlappende Kreise")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
