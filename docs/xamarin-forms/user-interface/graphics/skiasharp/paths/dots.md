---
title: Punkte und Bindestriche in skiasharp
description: In diesem Artikel wird erläutert, wie Sie die Feinheiten der Zeichnung von gepunkteten und gestrichelten Linien in skiasharp beherrschen und dies mit Beispielcode veranschaulichen.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 47182578a6583dde34cb7f06e3433cdb2703f6ba
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937682"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Punkte und Bindestriche in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Die Feinheiten der Zeichnung gepunkteter und gestrichelter Linien in skiasharp beherrschen_

Skiasharp ermöglicht das Zeichnen von Zeilen, die nicht Solid sind, sondern aus Punkten und Bindestrichen bestehen:

![Gepunktete Linie](dots-images/dottedlinesample.png)

Dies erfolgt mit einem *Pfad Effekt*, bei dem es sich um eine Instanz der-Klasse handelt, die [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) Sie auf die-Eigenschaft von festgelegt haben [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) `SKPaint` . Mithilfe einer der statischen Erstellungs Methoden, die von definiert werden, können Sie einen Pfad Effekt erstellen (oder Pfad Effekte kombinieren) `SKPathEffect` . ( `SKPathEffect` ist einer von sechs Effekten, die von skiasharp unterstützt werden; die anderen werden im Abschnitt [**skiasharp-Effekt**](../effects/index.md)beschrieben.)

Um gepunktete oder gestrichelte Linien zu zeichnen, verwenden Sie die [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) statische-Methode. Es gibt zwei Argumente: das erste ist ein Array von `float` Werten, die die Länge der Punkte und Bindestriche sowie die Länge der Leerzeichen dazwischen angeben. Dieses Array muss eine gerade Anzahl von Elementen aufweisen, und es sollten mindestens zwei Elemente vorhanden sein. (Es können 0 Elemente im Array vorhanden sein, dies führt jedoch zu einer voll soliden Linie.) Wenn zwei Elemente vorhanden sind, ist die erste die Länge eines Punkts oder Strichs, und die zweite ist die Länge der Lücke vor dem nächsten Punkt oder Bindestrich. Wenn mehr als zwei Elemente vorhanden sind, sind Sie in dieser Reihenfolge: Strichlänge, Länge des Strichs, Strichlänge, Länge der Lücke usw.

Im Allgemeinen sollten Sie den Bindestrich und die Lücke zu einem Vielfaches der Strichbreite machen. Wenn die Strichbreite z. b. 10 Pixel beträgt, zeichnet das Array {10, 10} eine gepunktete Linie, bei der die Punkte und Lücken die gleiche Länge wie die Strichstärke haben.

Die `StrokeCap` -Einstellung des- `SKPaint` Objekts wirkt sich aber auch auf diese Punkte und Bindestriche aus. Wie Sie sehen werden, wirkt sich dies auf die Elemente dieses Arrays aus.

Gepunktete und gestrichelte Linien werden auf der Seite **Punkte und Bindestriche** veranschaulicht. Die Datei " [**dopoanddashespage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml) " instanziiert zwei `Picker` Sichten, eine für die Auswahl einer Strich Abdeckung und die zweite zum Auswählen eines Bindestrich Arrays:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
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

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
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

 Die ersten drei Elemente in der `dashArrayPicker` nehmen an, dass die Strichbreite 10 Pixel beträgt. Das Array "{10, 10}" ist für eine gepunktete Linie, "{30, 10}" für eine gestrichelte Linie und "{10, 10, 30, 10}" ist für eine Punkt-und Binde Linie vorgesehen. (Die anderen drei werden in Kürze erläutert.)

Die [`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs) Code-Behind-Datei enthält den `PaintSurface` Ereignishandler und einige Hilfsroutinen für den Zugriff `Picker` auf die Ansichten:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

In den folgenden Screenshots zeigt der IOS-Bildschirm ganz links eine gepunktete Linie:

[![Dreifacher Screenshot der Seite "Punkte und Bindestriche"](dots-images/dotsanddashes-small.png)](dots-images/dotsanddashes-large.png#lightbox "Dreifacher Screenshot der Seite "Punkte und Bindestriche"")

Der Android-Bildschirm sollte jedoch auch eine gepunktete Linie mit dem Array "{10, 10}" anzeigen, aber die Linie ist "Solid". Was ist passiert? Das Problem besteht darin, dass auf dem Android-Bildschirm auch eine Strich Obergrenzen von festgelegt ist `Square` . Dadurch werden alle Bindestriche um die Hälfte der Strichbreite erweitert, sodass Sie die Lücken auffüllen.

Um dieses Problem zu umgehen, wenn Sie eine Strich Abdeckung von `Square` oder verwenden `Round` , müssen Sie die Strich Längen im Array um die Strichlänge verringern (manchmal führt dies zu einer Bindestrich Länge von 0) und die GAP-Längen um die Strichlänge erhöhen. Auf diese Weise werden die letzten drei Bindestriche in der `Picker` in der XAML-Datei berechnet:

- {10, 10} wird für eine gepunktete Linie {0, 20}.
- {30, 10} wird für eine gestrichelte Linie zu {20, 20}.
- {10, 10, 30, 10} wird für eine gepunktete und gestrichelte Linie {0, 20, 20, 20}.

Der UWP-Bildschirm zeigt die gepunktete und gestrichelte Linie für eine Strich Abdeckung von `Round` . Die `Round` Strich Abdeckung ermöglicht oft die beste Darstellung von Punkten und Bindestrichen in dicken Linien.

Bisher gab es keine Erwähnung des zweiten Parameters für die `SKPathEffect.CreateDash` Methode. Dieser Parameter hat `phase` den Namen und verweist auf einen Offset innerhalb des dot-and-Dash-Musters für den Anfang der Zeile. Wenn das Bindestrich-Array z. b. {10, 10} und den Wert `phase` 10 hat, beginnt die Zeile mit einer Lücke und nicht mit einem Punkt.

Eine interessante Anwendung des- `phase` Parameters ist in einer Animation. Die **animierte Spiral** Seite ähnelt der **Archimedischen Spiral** Page, mit der Ausnahme, dass die- [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) Klasse den- `phase` Parameter mit der- Xamarin.Forms `Device.Timer` Methode animiert:

```csharp
public class AnimatedSpiralPage : ContentPage
{
    const double cycleTime = 250;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float dashPhase;

    public AnimatedSpiralPage()
    {
        Title = "Animated Spiral";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            dashPhase = (float)(10 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }
    ···  
}
```

Natürlich müssen Sie das Programm tatsächlich ausführen, um die Animation zu sehen:

[![Dreifacher Screenshot der animierten Spiral Seite](dots-images/animatedspiral-small.png)](dots-images/animatedspiral-large.png#lightbox "Dreifacher Screenshot der animierten Spiral Seite")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
