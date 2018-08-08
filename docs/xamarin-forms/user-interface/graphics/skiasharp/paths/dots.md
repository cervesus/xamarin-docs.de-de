---
title: Punkte und Striche in SkiaSharp
description: In diesem Artikel wird untersucht, wie die feinheiten der Zeichnen von punktierter und gestrichelter Linien im SkiaSharp-Masterknoten, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7c336e6b5224f61ff84eb39652788b23f52b806e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615417"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Punkte und Striche in SkiaSharp

_Meistern Sie die feinheiten der punktierte und gestrichelte Linien im SkiaSharp zeichnen_

SkiaSharp können Sie das Zeichnen von Linien, die nicht solid aber stattdessen aus Punkten und Bindestrichen zusammengesetzt sind:

![](dots-images/dottedlinesample.png "Gepunktete Linie")

Dies erledigen Sie über eine *Pfad Auswirkungen*, dies ist eine Instanz von der [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) -Klasse, die Sie, um festlegen die [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) Eigenschaft `SKPaint`. Sie können einen Pfad erstellen Auswirkungen (oder kombinieren pfadeffekte) mit der statischen `Create` definierten Methoden `SKPathEffect`.

Um gepunkteten oder gestrichelten Linien zu zeichnen, verwenden Sie die [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) statische Methode. Es gibt zwei Argumente: Dies ist zunächst ein Array von `float` Werte, die die Länge der die Punkte und Bindestriche enthalten und die Länge der Leerzeichen dazwischen angeben. Dieses Array muss eine gerade Anzahl von Elementen verwendet, und es sollte mindestens zwei Elemente vorhanden sein. (Es kann sein, aber dies führt zu einer durchgezogenen Linie 0 (null) Elemente im Array.) Wenn zwei Elemente vorhanden sind, wird die erste ist die Länge von einem Punkt oder Gedankenstrich, und die zweite ist die Länge der Lücke, bevor der nächste Punkt oder Gedankenstrich. Wenn mehr als zwei Elemente vorhanden sind, sie sind in der folgenden Reihenfolge: dash, Länge, Länge der Lücke, Dash-Länge, Länge der Lücke und So weiter.

Im Allgemeinen sollten Sie den Striche und Lücken Längen ein Vielfaches der Strichbreite vornehmen. Wenn die Strichbreite 10 Pixel ist, wird z. B. Klicken Sie dann das Array {10, 10} eine gepunktete Linie gezeichnet, wo sind die Punkte und Lücken. für die gleiche Länge wie die Stärke des Strichs.

Allerdings die `StrokeCap` festlegen, der die `SKPaint` Objekt wirkt sich auch auf diese Punkte und Bindestriche enthalten. Wie Sie bald sehen werden, hat, die Auswirkungen auf die Elemente dieses Arrays.

Gepunktete und gestrichelte Linien werden auf veranschaulicht die **Punkte und Gedankenstriche** Seite. Die [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) Datei instanziiert zwei `Picker` anzeigt, für die in dem Sie eine Obergrenze für die Kontur und die zweite Auswahl ein Array von Dash auswählen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
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
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
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

Die ersten drei Elemente in der `dashArrayPicker` wird vorausgesetzt, dass die Strichbreite 10 Pixel. Die {10, 10} Array für eine gepunktete Linie ist {30, 10} ist für eine gestrichelte Linie, und {10, 10, 30, 10} wird für Punkt-und-gestrichelte Linie als Trennzeichen. (Die anderen drei wird in Kürze erläutert.)

Die [ `DotsAndDashesPage` Code-Behind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) enthält die `PaintSurface` -Ereignishandler und Hilfsroutinen, die für den Zugriff auf die `Picker` Ansichten:

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
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

In den folgenden Screenshots wird der iOS-Bildschirms ganz links eine gepunktete Linie auf:

[![](dots-images/dotsanddashes-small.png "Dreifacher Screenshot der Seite für Punkte und Striche")](dots-images/dotsanddashes-large.png#lightbox "dreifachen Screenshot der Seite für Punkte und Bindestriche enthalten")

Die Android-Bildschirm sollte auch eine gepunktete Linie mit dem Array {10, 10} zeigen jedoch die Zeile ist stattdessen solid. Was ist passiert? Das Problem besteht darin, dass die Android-Bildschirm auch Stroke Caps Einstellung wurde `Square`. Dies erweitert alle Gedankenstriche halbe Strichbreite, veranlassen, um die Lücken zu füllen.

Um dieses Problem umgehen, bei Verwendung einer Stroke Begrenzung von `Square` oder `Round`, müssen Sie verringern die Längen Dash in Arrays durch die Stroke-Länge (in einigen Fällen führt ein Dash-Länge von 0), und erhöhen Sie die Länge der Lücke durch die Länge des Strichs. Dies ist wie die letzten drei Arrays in dash der `Picker` in der XAML-Datei wurden berechnet:

- {10, 10} ist {0, 20} für eine gepunktete Linie
- {30, 10} ist {20, 20} für eine gestrichelte Linie
- {"10", "10", "30", "10"} {0, 20, 20, 20} für eine punktierte und gestrichelte Linie wird

Die UWP Bildschirm zeigt an, die gepunktete und gestrichelte Linie für einen Strich der Obergrenze `Round`. Die `Round` Stroke Cap bietet die beste Darstellung der Punkte und Bindestriche enthalten häufig in thick-Zeilen.

Bisher nicht erwähnt wurde von der zweite Parameter für die `SKPathEffect.CreateDash` Methode. Dieser Parameter heißt `phase` und sie bezieht sich auf einen Offset innerhalb des Punkt-und-Dash-Musters für den Anfang der Zeile. Wenn das Array Dash ist z. B. {10, 10} und die `phase` ist 10, und klicken Sie dann die Zeile beginnt mit einem Punkt, anstatt eine Lücke.

Eine interessante Anwendungsmöglichkeit für die `phase` Parameter ist in einer Animation. Die **animiert Spirale** Seite ähnelt der **Archimedean Spirale** Seite, außer dass die [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) Klasse animiert die `phase` Parameter. Die Seite zeigt außerdem ein weiteres Verfahren zum Animation. Im obigen Beispiel die [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) verwendet die `Task.Delay` Methode, um die Animation steuern. In diesem Beispiel wird stattdessen der Xamarin.Forms `Device.Timer` Methode:


```csharp
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
```

Natürlich müssen Sie das Programm zum Anzeigen der Animation tatsächlich auszuführen:

[![](dots-images/animatedspiral-small.png "Dreifacher Screenshot der Seite animiert Spirale")](dots-images/animatedspiral-large.png#lightbox "dreifachen Screenshot der Seite animiert Spirale")

Sie haben jetzt gesehen, wie zum Zeichnen von Linien und Kurven, die mithilfe von Parametrischer Formeln definiert werden. Ein Abschnitt zu einem späteren Zeitpunkt veröffentlicht werden, behandelt die verschiedenen Typen von Kurven, `SKPath` unterstützt.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
