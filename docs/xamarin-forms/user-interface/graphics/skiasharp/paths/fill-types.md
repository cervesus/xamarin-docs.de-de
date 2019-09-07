---
title: Die Fülltypen für Pfade
description: In diesem Artikel untersucht die verschiedenen Effekte, die mögliche mit SkiaSharp-Fülltypen für Pfade, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 924b66b3bdb66c2197b708d87e20eeb6f3ed9f46
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770513"
---
# <a name="the-path-fill-types"></a>Die Fülltypen für Pfade

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Ermitteln Sie die verschiedenen Effekte, die mit SkiaSharp-Fülltypen für Pfade_

Zwei Konturen in einem Pfad können sich überschneiden, und die Zeilen, aus denen eine einzelne Contour Global besteht, können sich überschneiden. Potenziell kann eingeschlossene Bereich gefüllt werden, aber Sie möchten möglicherweise nicht alle eingeschlossene Bereiche zu füllen. Im Folgenden ein Beispiel:

![](fill-types-images/filltypeexample.png "Fünf Spitzen Sternen teilweise Filles")

Sie haben ein wenig Kontrolle über diese. Der Algorithmus aufgefüllt werden, unterliegt die [ `SKFillType` ](xref:SkiaSharp.SKPath.FillType) Eigenschaft `SKPath`, festgelegt auf einen Member der [ `SKPathFillType` ](xref:SkiaSharp.SKPathFillType) Enumeration:

- `Winding`, der Standardwert
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

Die wicklungsreihenfolgen und gerade-ungerade Algorithmen bestimmen, ob alle eingeschlossene Bereich gefüllt ist, oder nicht basierend auf einer hypothetischen Linie unendlich von diesem Bereich ausgefüllt. Diese Zeile überschreitet eine oder mehrere Grenzen Zeilen, die den Pfad zu bilden. Mit dem wicklungsreihenfolgen Modus ist die Anzahl der in einer Richtung Ausgleich der die Anzahl der Zeilen, die in die andere Richtung, und klicken Sie dann auf den Bereich gezeichnet gezeichneten Grenzlinien verwendet nicht gefüllt. Andernfalls wird der Bereich gefüllt. Der Algorithmus gerade-ungerade füllt einen Bereich aus, wenn die Anzahl der Zeilen der Grenze ungerade ist.

Mit vielen routinemäßige Pfaden füllt der wicklungsreihenfolgen Algorithmus häufig die eingeschlossene Bereiche eines Pfads. Der gerade-ungerade Algorithmus erzeugt in der Regel Weitere interessante Ergebnisse.

Das klassische Beispiel ist ein Stern fünf zwischengespeichert, wie in der **Five-Pointed Stern** Seite. Die [ **FivePointedStarPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) Datei instanziiert zwei `Picker` Ansichten, wählen Sie den Pfad zu füllen, Typ und gibt an, ob der Pfad mit Strichen gezeichnet oder gefüllt ist oder beide, und in welcher Reihenfolge:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei verwendet `Picker` Werte zum Zeichnen von eines Sterns fünf Spitzen:

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
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
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

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Der Pfadtyp sollte normalerweise beeinflussen, nur gefüllt, und nicht die Striche, doch die beiden `Inverse` Modi wirken sich sowohl Flächen und Konturen. Für gefüllt, die beiden `Inverse` Typen Bereiche füllen oppositely, damit Sie der Bereich außerhalb der Stern gefüllt wird. Für die Striche, die beiden `Inverse` Typen Farbe, die alles mit Ausnahme des Strichs. Die Verwendung dieser Füllungstypen inverse kann einige ungerade Effekte, erzeugt, wie der iOS-Screenshot veranschaulicht:

[![](fill-types-images/fivepointedstar-small.png "Dreifacher Screenshot der Seite Five-Pointed Stern")](fill-types-images/fivepointedstar-large.png#lightbox "dreifachen Screenshot der Seite Five-Pointed Stern")

Die Android- und UWP-Screenshots zeigen die typischen Auswirkungen der gerade-ungerade und wicklungsreihenfolgen, aber die Reihenfolge der Strich und Füllung wirkt sich auch auf die Ergebnisse.

Der Algorithmus wicklungsreihenfolgen ist abhängig von die Richtung von Linien gezeichnet werden. In der Regel können Wenn Sie einen Pfad erstellen, Sie die entsprechende Richtung steuern, wie Sie angeben, dass Zeilen, die von einem Punkt in eine andere gezeichnet werden. Allerdings die `SKPath` -Klasse definiert außerdem Methoden wie `AddRect` und `AddCircle` , die gesamte Konturen zeichnen. Um zu steuern, wie diese Objekte gezeichnet werden, enthalten die Methoden einen Parameter vom Typ [ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection), die über zwei Member verfügt:

- `Clockwise`
- `CounterClockwise`

Die Methoden in `SKPath` , enthalten ein `SKPathDirection` Parameter Geben Sie ihm den Standardwert `Clockwise`.

Die **überlappende Kreise** Seite wird ein Pfad mit vier überlappende Kreise mit einem gerade-ungerade Pfad Fill-Typ erstellt:

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

Es ist ein interessantes Bild mit einem Minimum an Code erstellt:

[![](fill-types-images/overlappingcircles-small.png "Dreifacher Screenshot der Seite für überlappende Kreise")](fill-types-images/overlappingcircles-large.png#lightbox "dreifachen Screenshot der Seite für überlappende Kreise")

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
