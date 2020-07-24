---
title: Die Fülltypen für Pfade
description: In diesem Artikel werden die unterschiedlichen möglichen Effekte mit skiasharp-Pfad Füll Typen untersucht, und es wird ein Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e82572d88e380997fb2435179dba824c1b3f0c2f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936187"
---
# <a name="the-path-fill-types"></a>Die Fülltypen für Pfade

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Entdecken Sie die verschiedenen Effekte, die mit skiasharp-Pfad Füll Typen möglich sind._

Zwei Kontur in einem Pfad können sich überlappen, und die Linien, die eine einzelne Kontur bilden, können sich überlappen. Alle eingeschlossenen Bereiche können möglicherweise ausgefüllt werden, Sie möchten jedoch möglicherweise nicht alle eingeschlossenen Bereiche ausfüllen. Ein Beispiel:

![Fünf-pointierte Sterne teilweise](fill-types-images/filltypeexample.png)

Dies haben Sie nur wenig Kontrolle. Der Füllungs Algorithmus wird von der- [`SKFillType`](xref:SkiaSharp.SKPath.FillType) Eigenschaft von gesteuert `SKPath` , die Sie auf einen Member der- [`SKPathFillType`](xref:SkiaSharp.SKPathFillType) Enumeration festlegen:

- `Winding`, der Standardwert
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

Sowohl der Auszieh-als auch der gerade ungerade-Algorithmus bestimmen, ob ein eingeschlossener Bereich auf der Grundlage einer hypothetischen Linie ausgefüllt oder nicht ausgefüllt wird, die von diesem Bereich auf unendlich Diese Zeile überschreitet eine oder mehrere Begrenzungs Linien, die den Pfad bilden. Wenn die Anzahl der in einer Richtung gezeichneten Begrenzungs Linien die Anzahl der in der anderen Richtung gezeichneten Linien ausgeglichen, wird der Bereich nicht ausgefüllt Andernfalls wird der Bereich ausgefüllt. Der gerade ungerade-Algorithmus füllt einen Bereich, wenn die Anzahl der Begrenzungs Linien ungerade ist.

Bei vielen Routine Pfaden füllt der wicklungs Algorithmus häufig alle eingeschlossenen Bereiche eines Pfades aus. Der gerade ungerade-Algorithmus erzeugt in der Regel interessantere Ergebnisse.

Das klassische Beispiel ist ein fünf zeiiger Stern, wie auf der **fünf-Sterne-Stern** Seite gezeigt. Die Datei " [**filevepointedstarpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FivePointedStarPage.xaml) " instanziiert zwei `Picker` Sichten, um den Pfad Fülltyp auszuwählen, und gibt an, ob der Pfad gestreichelt oder ausgefüllt ist, und in welcher Reihenfolge:

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

Die Code-Behind-Datei verwendet beide `Picker` Werte, um einen fünf-Spitzen-Stern zu zeichnen:

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

Normalerweise sollte sich der Pfad Fülltyp nur auf Füll-und nicht-Striche auswirken, aber die beiden `Inverse` Modi wirken sich sowohl auf Fill als auch auf Striche aus. Für Fill werden die Bereiche durch die beiden Typen gegenseitig `Inverse` ausgefüllt, sodass der Bereich außerhalb des Sterns ausgefüllt ist. Bei Strichen wird durch die beiden `Inverse` Typen alles außer dem Strich Farben angezeigt. Die Verwendung dieser umgekehrten Füll Typen kann zu ungeraden Effekten führen, wie der IOS-Bildschirmfoto veranschaulicht:

[![Dreifacher Screenshot der fünf-Spitze-Stern Seite](fill-types-images/fivepointedstar-small.png)](fill-types-images/fivepointedstar-large.png#lightbox "Dreifacher Screenshot der fünf-Spitze-Stern Seite")

Der Android-Screenshot zeigt die typischen, geraden und auffüllenden Effekte, aber die Reihenfolge der Striche und der Füllung wirkt sich auch auf die Ergebnisse aus.

Der wicklungs Algorithmus hängt von der Richtung ab, in der Linien gezeichnet werden. Wenn Sie einen Pfad erstellen, können Sie diese Richtung normalerweise steuern, wenn Sie angeben, dass Linien von einem Punkt zu einem anderen gezeichnet werden. Die `SKPath` -Klasse definiert jedoch auch Methoden wie `AddRect` und `AddCircle` , die ganze Kontur zeichnen. Um die Art und Weise zu steuern, wie diese Objekte gezeichnet werden, enthalten die Methoden einen Parameter vom Typ [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) , der zwei Member hat:

- `Clockwise`
- `CounterClockwise`

Die Methoden in `SKPath` , die einen-Parameter enthalten, `SKPathDirection` geben dem Standardwert `Clockwise` .

Auf der Seite über **Lapp Ende Kreise** wird ein Pfad mit vier überlappenden Kreisen mit einem gerade ungeraden Pfad Fülltyp erstellt:

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

Es handelt sich um ein interessantes Image, das mit mindestens einem Code erstellt wurde:

[![Dreifacher Screenshot der Seite mit überlappenden Kreisen](fill-types-images/overlappingcircles-small.png)](fill-types-images/overlappingcircles-large.png#lightbox "Dreifacher Screenshot der Seite mit überlappenden Kreisen")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
