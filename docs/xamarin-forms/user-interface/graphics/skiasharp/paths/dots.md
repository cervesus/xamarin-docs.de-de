---
title: Punkte und Bindestriche enthalten
description: "Die Komplexität der Zeichnung SkiaSharp punktierter und gestrichelter Linien \"Master\""
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 30b7e322d618492164fac5e439c5187616d61717
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="dots-and-dashes"></a>Punkte und Bindestriche enthalten

_Die Komplexität der Zeichnung SkiaSharp punktierter und gestrichelter Linien "Master"_

SkiaSharp können Sie das Zeichnen von Linien, die sind keine durchgehende sondern stattdessen aus Punkten und Bindestrichen bestehen:

![](dots-images/dottedlinesample.png "Gepunktete Linie")

Dazu mit einer *Pfad Auswirkungen*, also in eine Instanz von der [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) -Klasse, die Sie, um festlegen die [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) Eigenschaft `SKPaint`. Sie können einen Pfad erstellen wirksam (oder kombinieren Pfad Effekte) mithilfe der statischen `Create` definierten Methoden `SKPathEffect`.

Um gepunkteten oder gestrichelten Linien zu zeichnen, verwenden Sie die [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) statische Methode. Es gibt zwei Argumente: zuerst ist dies ein Array von `float` Werte, die die Länge des Punkte und Bindestriche enthalten und die Länge der Leerzeichen dazwischen angeben. Dieses Array muss eine gerade Anzahl von Elementen, und sollte über mindestens zwei Elemente vorhanden sein. (Es kann sein, aber 0 (null) Elemente im Array, der das Ergebnis in eine durchgehende Linie.) Wenn zwei Elemente vorhanden sind, die erste ist die Länge von einem Punkt oder Gedankenstrich, und die zweite ist die Länge des Abstands vor dem nächsten Punkt oder Gedankenstrich. Wenn mehr als zwei Elemente vorhanden sind, werden in dieser Reihenfolge: Bindestrich Länge, Lücke Länge, Dash-Länge, Lücke Länge und So weiter.

Im Allgemeinen sollten Sie den Längen Striche und Lücken ein Vielfaches der Strichbreite vornehmen. Wenn Strichbreite 10 Pixel ist, wird z. B. Klicken Sie dann das Array {10, 10} eine gepunktete Linie gezeichnet, wo sind die Punkte und Lücken die gleiche Länge wie die Strichstärke.

Allerdings die `StrokeCap` festlegen, der die `SKPaint` Objekt wirkt sich auch auf diese Punkte und Bindestriche enthalten. Wie Sie in Kürze sehen, hat, die Auswirkungen auf die Elemente dieses Arrays.

Gepunktete und gestrichelte Linien auf demonstriert die **Punkte und Bindestriche** Seite. Die [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) Datei instanziiert zwei `Picker` anzeigt, eine für die in dem Sie eine Obergrenze Strich und das zweite auf ein Array Dash auswählen:

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

Die ersten drei Elemente in der `dashArrayPicker` wird davon ausgegangen, dass die Strichbreite 10 Pixel beträgt. Der {10, 10} Array für eine gepunktete Linie ist {30, 10} ist für eine gestrichelte Linie und {10, 10, 30, 10} ist für eine Linie an Punkt Strich. (Die anderen drei wird in Kürze erläutert werden.)

Die [ `DotsAndDashesPage` Code-Behind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) enthält die `PaintSurface` -Ereignishandler und eine Reihe von Routinen der Hilfsfunktion für den Zugriff auf die `Picker` Ansichten:

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

In den folgenden Screenshots wird der iOS-Bildschirm auf die äußerst linke eine gepunktete Linie auf:

[![](dots-images/dotsanddashes-small.png "Dreifacher Screenshot der Seite Punkte und Bindestriche enthalten")](dots-images/dotsanddashes-large.png "dreifacher Screenshot der Seite Punkte und Bindestriche enthalten")

Jedoch Android Bildschirm sollte auch eine gepunktete Linie unter Verwendung des Arrays {10, 10} anzeigen, sondern stattdessen die Linie ist Durchgezogen. Was ist passiert? Das Problem besteht darin, dass Android Bildschirm auch Strich Caps Einstellung wurde `Square`. Erweitert alle Bindestriche halbe Strichbreite verursacht werden, um die Lücken zu füllen.

Um dieses Problem umgehen, bei Verwendung einer Kontur Cap von `Square` oder `Round`, müssen Sie die Längen der Bindestrich im Array von der Länge (was in einigen Fällen Dash Länge 0) verringern und erhöhen die Längen der Abstand von der Länge. Dies ist wie die letzten drei Arrays in den Bindestrich der `Picker` in der XAML-Datei wurden berechnet:

- {10, 10} ist {0, 20} für eine gepunktete Linie
- {30, 10} ist {20, 20} für eine gestrichelte Linie
- {"10", "10", "30", "10"} {0, 20, 20, 20} für eine punktierte und gestrichelte Linie wird

Die Windows-Bildschirm an, dass der gepunktete und gestrichelte Linie für einen Strich der cap `Round`. Die `Round` Strich Cap bietet die beste Darstellung der Punkte und Bindestriche enthalten häufig in thick Zeilen.

Bisher nicht erwähnt wurde des zweiten Parameters, der die `SKPathEffect.CreateDash` Methode. Dieser Parameter heißt `phase` und verweist auf einen Offset innerhalb des Punkt und Dash-Musters für den Anfang der Zeile. Wenn das Array Dash ist z. B. {10, 10} und der `phase` ist 10, und klicken Sie dann die Zeile beginnt mit einem Punkt, anstatt eine Lücke.

Eine interessante Anwendungsmöglichkeit der `phase` Parameter ist in einer Animation. Die **Spirale animiert** Seite ähnelt der **Archimedean Spirale** Seite, außer dass die [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) Klasse animiert das `phase` Parameter. Die Seite zeigt außerdem ein weiteres Verfahren zum Animation. Das vorangegangene Beispiel dem [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) verwendet die `Task.Delay` Methode zum Steuern der Animation. In diesem Beispiel wird stattdessen verwendet die Xamarin.Forms `Device.Timer` Methode:


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

[![](dots-images/animatedspiral-small.png "Dreifacher Screenshot der Seite animiert Spirale")](dots-images/animatedspiral-large.png "dreifacher Screenshot der Seite animiert Spirale")

Sie haben nun gesehen, wie zum Zeichnen von Linien und Kurven mit parametrische Formeln definieren werden. Ein Abschnitt später veröffentlicht werden sollen die verschiedenen Typen von Kurven, `SKPath` unterstützt.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
