---
title: Integrieren von Xamarin.Forms
description: Erstellen von SkiaSharp Grafiken, die auf die Fingereingabe und Xamarin.Forms-Elemente reagieren
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 3ebe153ead2bb62b19ad6b25bf0093e20bf15c04
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="integrating-with-xamarinforms"></a>Integrieren von Xamarin.Forms

_Erstellen von SkiaSharp Grafiken, die auf die Fingereingabe und Xamarin.Forms-Elemente reagieren_

SkiaSharp Grafiken können mit dem Rest des Xamarin.Forms auf verschiedene Weise integrieren. Sie können einen Zeichenbereich SkiaSharp und Xamarin.Forms auf derselben Seite und sogar Position Xamarin.Forms Elemente auf einem Zeichenbereich SkiaSharp kombinieren:

![](integration-images/integrationexample.png "Auswählen einer Farbe mit Schieberegler")

Ein weiteres Verfahren zum Erstellen von interaktiven SkiaSharp Grafiken in Xamarin.Forms erfolgt über Touch.
Die zweite Seite in der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm berechtigt ist **tippen ein-/ausschalten füllen**. Es zeichnet einen einfachen Kreis auf zwei Arten &mdash; ohne Füllung und mit einem "füllen" &mdash; von einer Tap ein-/ausgeschaltet. Die [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Klasse zeigt, wie Sie SkiaSharp Grafiken in Reaktion auf eine Benutzereingabe ändern können.

Für diese Seite die `SKCanvasView` Klasse instanziiert wird, der [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) -Datei, die eine Xamarin.Forms auch setzt [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) für die Sicht:

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

Die `Tapped` Handler für das `TapGestureRecognizer` Objekt schaltet einfach den Wert ein boolesches Feld und ruft die [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) Methode `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Der Aufruf von `InvalidateSurface` effektiv generiert einen Aufruf der `PaintSurface` Handler, der verwendet die `showFill` Feld aufzufüllen, oder den Kreis nicht aufgefüllt werden:

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

Die `StrokeWidth` Eigenschaft auf 50 festgelegt wurde, um den Unterschied hervorzuheben. Sie können auch die gesamte Linienstärke durch Zeichnen zuerst das innere und klicken Sie dann auf die Gliederung anzeigen. Standardmäßig Abbildungen Grafiken gezeichneten weiter unten in der `PaintSurface` Ereignishandler verdecken, die weiter oben in der Handler gezeichnet.

Die **Farbe untersuchen** Seite veranschaulicht, wie Sie SkiaSharp Grafiken auch bei anderen Elementen Xamarin.Forms integrieren können, und auch zeigt den Unterschied zwischen zwei alternative Methoden zum Definieren von Farben in SkiaSharp. Die statische [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) Methode erstellt ein `SKColor` Wert auf Grundlage des Farbton-Sättigung-Helligkeit-Modells:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Die statische [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) Methode erstellt ein `SKColor` Wert auf Grundlage des ähnliche Farbton-Sättigung-Value-Modells:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

In beiden Fällen die `h` Argument liegt zwischen 0 und 360. Die `s`, `l`, und `v` Argumente, die zwischen 0 und 100 liegen. Die `a` (Alpha oder Deckkraft) Argument liegt zwischen 0 und 255.

Die [ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) Datei erstellt zwei `SKCanvasView` Objekte in einem `StackLayout` Seite-an-Seite mit `Slider` und `Label` Ansichten, mit denen den Benutzer zur Auswahl von HSL können und HSV Farbwerte anzeigt:

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

Die beiden `SKCanvasView` Elemente sind in einer einzelnen Zelle `Grid` mit einem `Label` auf nach oben zum Anzeigen der resultierenden RGB-Wert.

Die [ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) Code-Behind-Datei ist relativ einfach. Die gemeinsam verwendete `ValueChanged` Handler für die drei `Slider` Elemente erklärt beide einfach `SKCanvasView` Elemente. Die `PaintSurface` Handler Deaktivieren der Canvas mit der Farbe, angegeben durch die `Slider` Elemente, und legen Sie außerdem die `Label` sich direkt auf der Basis von der `SKCanvasView` Elemente:

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

Im sowohl die HSL- und HSV Farbe der Farbtonwert reicht von 0 bis 360 und den bestimmenden Farbton der Farbe angibt. Hierbei handelt es sich um herkömmliche Farben der Rainbow: Rot, Orange, Gelb, Grün, Blau, Indigo, Violett geändert und erneut in einem Kreis in Rot.

Im Modell HSL einen 0-Wert für die Helligkeit ist immer Schwarz und eine 100-Wert ist immer weiß. Wenn der Sättigungswert 0 ist, sind Helligkeitswerte zwischen 0 und 100 Graustufen. Erhöhen die Sättigung fügt Weitere Farben. Reine Farben (die RGB-Werte mit einer Komponente gleich 255, eine andere gleich 0 und der dritte im Bereich von 0 bis 255) auftreten, wenn die Sättigung liegt zwischen 100 und die Helligkeit "50 lautet".

Führen im Modell HSV reine Farben aus, wenn es sich bei dem Sättigung und den Wert 100 sind. Wenn der Wert 0 ist, unabhängig von der alle anderen Einstellungen wird ist die Farbe Schwarz. Graustufen auftreten, wenn die Sättigung 0 und der Wert liegt zwischen 0 und 100 ist.

Die beste Möglichkeit, ein Gefühl für die beiden Modelle mit ihnen experimentieren selbst ist jedoch:

[![](integration-images/colorexplore-large.png "Dreifacher Screenshot der Seite untersuchen Farbe")](integration-images/colorexplore-small.png#lightbox "dreifacher Screenshot der Seite Farbe durchsuchen")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
