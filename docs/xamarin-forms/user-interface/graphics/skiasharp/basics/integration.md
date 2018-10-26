---
title: Integrieren von Xamarin.Forms
description: In diesem Artikel erläutert das Erstellen von SkiaSharp-Grafiken, die auf Fingereingabe reagieren und Xamarin.Forms-Elemente, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
ms.openlocfilehash: bf9b0388ff3b024439cfc3488e4057ba32fdab6b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115080"
---
# <a name="integrating-with-xamarinforms"></a>Integrieren von Xamarin.Forms

_Erstellen von SkiaSharp-Grafiken, die auf Fingereingabe und Xamarin.Forms-Elemente reagieren_

SkiaSharp-Grafiken können den Rest von Xamarin.Forms auf verschiedene Weise integriert werden. Sie können einen SkiaSharp-Zeichenbereich und Xamarin.Forms-Elemente auf derselben Seite und sogar Positionieren von Xamarin.Forms-Elementen auf einer Leinwand SkiaSharp kombinieren:

![](integration-images/integrationexample.png "Auswählen einer Farbe mit Schieberegler")

Ein anderer Ansatz zum Erstellen von interaktiver Grafiken von SkiaSharp in Xamarin.Forms erfolgt über Touch.
Die zweite Seite in der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm berechtigt ist **tippen ein-/ausschalten geben**. Es wird einen einfachen Kreis auf zwei Arten gezeichnet &mdash; ohne eine Fläche und eine Füllung &mdash; von einem fingertipp auf eine ein-/ausgeschaltet. Die [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Klasse zeigt, wie Sie SkiaSharp-Grafiken in Reaktion auf eine Benutzereingabe ändern können.

Für diese Seite die `SKCanvasView` Klasse instanziiert wird, der [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) -Datei, die außerdem eine Xamarin.Forms wird [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) in der Ansicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Beachten Sie, dass die `skia` XML-Namespacedeklaration.

Die `Tapped` Handler für die `TapGestureRecognizer` Objekt einfach Schaltet den Wert ein boolesches Feld und ruft die [ `InvalidateSurface` ](xref:SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface) -Methode der `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Der Aufruf von `InvalidateSurface` effektiv generiert einen Aufruf an die `PaintSurface` Handler, der verwendet die `showFill` Feld füllen, oder geben Sie den Kreis nicht:

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
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

Die `StrokeWidth` Eigenschaft ist auf 50 festgelegt wurde, den Unterschied hervorzuheben. Sie können auch die gesamte Linienstärke zeichnen das innere zuerst und dann auf die Gliederung sehen. In der Standardeinstellung Grafiken ermittelt stammen, die weiter unten in der `PaintSurface` Ereignishandler verdecken die weiter oben im Ereignishandler gezeichnet.

Die **Farbe untersuchen** Seite veranschaulicht, wie Sie SkiaSharp-Grafiken auch mit anderen Elementen Xamarin.Forms integrieren können, und außerdem veranschaulicht den Unterschied zwischen zwei alternative Methoden zum Definieren von Farben in SkiaSharp. Die statische [ `SKColor.FromHsl` ](xref:SkiaSharp.SKColor.FromHsl(System.Single,System.Single,System.Single,System.Byte)) -Methode erstellt eine `SKColor` Wert basierend auf den Farbton-Sättigung und Helligkeit-Modell:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Die statische [ `SKColor.FromHsv` ](xref:SkiaSharp.SKColor.FromHsv(System.Single,System.Single,System.Single,System.Byte)) -Methode erstellt eine `SKColor` -Wert auf Grundlage der ähnlich wie Hue-Saturation-Value-Modell:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

In beiden Fällen die `h` Argument liegt zwischen 0 und 360 liegen. Die `s`, `l`, und `v` Argumente, die zwischen 0 und 100 liegen. Die `a` (Alpha oder der Durchlässigkeit) Argument liegt zwischen 0 und 255.

Die [ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) Datei erstellt zwei `SKCanvasView` Objekte in einer `StackLayout` parallel `Slider` und `Label` Ansichten, mit denen der Benutzer die Auswahl von HSL und HSV RGB-Werte:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Die beiden `SKCanvasView` Elemente befinden sich in einer einzelnen Zelle `Grid` mit einem `Label` auf Top, um die resultierende RGB-Farbwert anzeigen.

Die [ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) Code-Behind-Datei ist relativ einfach. Der gemeinsam verwendeten `ValueChanged` Handler für die drei `Slider` Elemente erklärt einfach beide `SKCanvasView` Elemente. Die `PaintSurface` Handler Löschen der Canvas mit der Farbe, angegeben durch die `Slider` Elemente, und legen Sie außerdem die `Label` sitzen auf der die `SKCanvasView` Elemente:

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

Der Farbtonwert im sowohl die HSL und HSV Farbe reicht von 0 bis 360, und den vorherrschenden Farbton der Farbe angibt. Hierbei handelt es sich um die herkömmlichen Farben des Regenbogens: Rot, Orange, Gelb, Grün, Blau, Indigo, Violet und zurück in einen Kreis zu rot.

Klicken Sie im HSL-Modell einen 0-Wert für die Helligkeit ist immer Schwarz, und ein 100-Wert ist immer weiß. Wenn der Sättigungswert 0 ist, sind Helligkeitswerte zwischen 0 und 100 grauschattierungen an. Erhöhen die Sättigung fügt Weitere Farbe. Reine Farben (die RGB-Werte mit einer Komponente, die gleich 255, eine andere gleich 0 und der dritte im Bereich von 0 bis 255 sind) auftreten, wenn die Sättigung 100 ist ein, und die Helligkeit 50 ist.

Führen im Modell HSV reine Farben so ein, wenn sowohl der Überlastung des Netzwerks als auch der Wert 100 sind. Wenn der Wert 0 ist, unabhängig davon, alle anderen Einstellungen, ist die Farbe Schwarz. Grauschattierungen auftreten, wenn die Sättigung 0 und der Wert liegt zwischen 0 und 100 ist.

Die beste Möglichkeit, ein Gefühl für die zwei Modelle mit ihnen experimentieren selbst ist jedoch:

[![](integration-images/colorexplore-large.png "Dreifacher Screenshot der Seite untersuchen Farbe")](integration-images/colorexplore-small.png#lightbox "dreifachen Screenshot der Seite untersuchen Farbe")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
