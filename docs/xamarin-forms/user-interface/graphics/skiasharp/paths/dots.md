---
title: Punkte und Striche in SkiaSharp
description: In diesem Artikel wird untersucht, wie die feinheiten der Zeichnen von punktierter und gestrichelter Linien im SkiaSharp-Masterknoten, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 229f60cbb96058454a1c634e53a7bb00ec725bcf
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723754"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Punkte und Striche in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Meistern Sie die feinheiten der punktierte und gestrichelte Linien im SkiaSharp zeichnen_

SkiaSharp können Sie das Zeichnen von Linien, die nicht solid aber stattdessen aus Punkten und Bindestrichen zusammengesetzt sind:

![](dots-images/dottedlinesample.png "Dotted line")

Dies erledigen Sie über eine *Pfad Auswirkungen*, dies ist eine Instanz von der [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) -Klasse, die Sie, um festlegen die [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) Eigenschaft `SKPaint`. Sie können einen Pfad erstellen Auswirkungen (oder kombinieren pfadeffekte) mit einer der definierten Erstellungsmethoden statische `SKPathEffect`. (`SKPathEffect` ist eine der sechs Auswirkungen von SkiaSharp unterstützt werden; die anderen sind im Abschnitt beschriebenen [ **Auswirkung von SkiaSharp**](../effects/index.md).)

Um gepunkteten oder gestrichelten Linien zu zeichnen, verwenden Sie die [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) statische Methode. Es gibt zwei Argumente: Dies ist zunächst ein Array von `float` Werte, die die Länge der die Punkte und Bindestriche enthalten und die Länge der Leerzeichen dazwischen angeben. Dieses Array muss eine gerade Anzahl von Elementen verwendet, und es sollte mindestens zwei Elemente vorhanden sein. (Es können 0 Elemente im Array vorhanden sein, dies führt jedoch zu einer voll soliden Linie.) Wenn zwei Elemente vorhanden sind, ist die erste die Länge eines Punkts oder Strichs, und die zweite ist die Länge der Lücke vor dem nächsten Punkt oder Bindestrich. Wenn mehr als zwei Elemente vorhanden sind, sie sind in der folgenden Reihenfolge: dash, Länge, Länge der Lücke, Dash-Länge, Länge der Lücke und So weiter.

Im Allgemeinen sollten Sie den Striche und Lücken Längen ein Vielfaches der Strichbreite vornehmen. Wenn die Strichbreite 10 Pixel ist, wird z. B. Klicken Sie dann das Array {10, 10} eine gepunktete Linie gezeichnet, wo sind die Punkte und Lücken. für die gleiche Länge wie die Stärke des Strichs.

Allerdings die `StrokeCap` festlegen, der die `SKPaint` Objekt wirkt sich auch auf diese Punkte und Bindestriche enthalten. Wie Sie bald sehen werden, hat, die Auswirkungen auf die Elemente dieses Arrays.

Gepunktete und gestrichelte Linien werden auf veranschaulicht die **Punkte und Gedankenstriche** Seite. Die [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml) Datei instanziiert zwei `Picker` anzeigt, für die in dem Sie eine Obergrenze für die Kontur und die zweite Auswahl ein Array von Dash auswählen:

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

 Die ersten drei Elemente in der `dashArrayPicker` wird vorausgesetzt, dass die Strichbreite 10 Pixel. Die {10, 10} Array für eine gepunktete Linie ist {30, 10} ist für eine gestrichelte Linie, und {10, 10, 30, 10} wird für Punkt-und-gestrichelte Linie als Trennzeichen. (Die anderen drei wird in Kürze erläutert.)

Die [ `DotsAndDashesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs) Code-Behind-Datei enthält die `PaintSurface` -Ereignishandler und Hilfsroutinen, die für den Zugriff auf die `Picker` Ansichten:

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

In den folgenden Screenshots wird der iOS-Bildschirms ganz links eine gepunktete Linie auf:

[![](dots-images/dotsanddashes-small.png "Triple screenshot of the Dots and Dashes page")](dots-images/dotsanddashes-large.png#lightbox "Triple screenshot of the Dots and Dashes page")

Die Android-Bildschirm sollte auch eine gepunktete Linie mit dem Array {10, 10} zeigen jedoch die Zeile ist stattdessen solid. Worin besteht die Ursache? Das Problem besteht darin, dass die Android-Bildschirm auch Stroke Caps Einstellung wurde `Square`. Dies erweitert alle Gedankenstriche halbe Strichbreite, veranlassen, um die Lücken zu füllen.

Um dieses Problem umgehen, bei Verwendung einer Stroke Begrenzung von `Square` oder `Round`, müssen Sie verringern die Längen Dash in Arrays durch die Stroke-Länge (in einigen Fällen führt ein Dash-Länge von 0), und erhöhen Sie die Länge der Lücke durch die Länge des Strichs. Dies ist wie die letzten drei Arrays in dash der `Picker` in der XAML-Datei wurden berechnet:

- {10, 10} ist {0, 20} für eine gepunktete Linie
- {30, 10} ist {20, 20} für eine gestrichelte Linie
- {"10", "10", "30", "10"} {0, 20, 20, 20} für eine punktierte und gestrichelte Linie wird

Die UWP Bildschirm zeigt an, die gepunktete und gestrichelte Linie für einen Strich der Obergrenze `Round`. Die `Round` Stroke Cap bietet die beste Darstellung der Punkte und Bindestriche enthalten häufig in thick-Zeilen.

Bisher nicht erwähnt wurde von der zweite Parameter für die `SKPathEffect.CreateDash` Methode. Dieser Parameter heißt `phase` und sie bezieht sich auf einen Offset innerhalb des Punkt-und-Dash-Musters für den Anfang der Zeile. Wenn das Array Dash ist z. B. {10, 10} und die `phase` ist 10, und klicken Sie dann die Zeile beginnt mit einem Punkt, anstatt eine Lücke.

Eine interessante Anwendungsmöglichkeit für die `phase` Parameter ist in einer Animation. Die **animiert Spirale** Seite ähnelt der **Archimedean Spirale** Seite, außer dass die [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) Klasse animiert die `phase` Parameter mit der Xamarin.Forms `Device.Timer` Methode:

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

Natürlich müssen Sie das Programm zum Anzeigen der Animation tatsächlich auszuführen:

[![](dots-images/animatedspiral-small.png "Triple screenshot of the Animated Spiral page")](dots-images/animatedspiral-large.png#lightbox "Triple screenshot of the Animated Spiral page")

## <a name="related-links"></a>Verwandte Themen

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
