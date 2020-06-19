---
title: Integration inXamarin.Forms
description: In diesem Artikel wird erläutert, wie skiasharp-Grafiken erstellt werden, die auf berühren und Xamarin.Forms Elemente reagieren, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9e763184f38719cda4526eb0a2dfdf39b2191a03
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137695"
---
# <a name="integrating-with-xamarinforms"></a>Integration inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erstellen von skiasharp-Grafiken, die auf berühren und Xamarin.Forms Elemente reagieren_

Skiasharp-Grafiken können Xamarin.Forms auf verschiedene Arten in den Rest von integriert werden. Sie können eine skiasharp-Canvas und- Xamarin.Forms Elemente auf derselben Seite kombinieren und sogar Xamarin.Forms Elemente oberhalb eines skiasharp-Canvas positionieren:

![](integration-images/integrationexample.png "Selecting a color with sliders")

Ein weiterer Ansatz zum Erstellen interaktiver skiasharp-Grafiken in Xamarin.Forms ist die Berührungs Weise.
Die zweite Seite im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programm hat den Titel **Tap UMSCHALT**Taste. Er zeichnet einen einfachen Kreis auf zwei Arten &mdash; ohne Füllung und mit einem durch Tippen umgeschalteten Füllvorgang &mdash; . Die- [`TapToggleFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Klasse zeigt, wie Sie skiasharp-Grafiken als Reaktion auf Benutzereingaben ändern können.

Für diese Seite wird die- `SKCanvasView` Klasse in der Datei " [tapdegglefill. XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) " instanziiert, die auch einen Xamarin.Forms [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) für die Ansicht festlegt:

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

Beachten Sie die `skia` XML-Namespace Deklaration.

Der `Tapped` Handler für das- `TapGestureRecognizer` Objekt schaltet einfach den Wert eines booleschen Felds um und ruft die- [`InvalidateSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface) Methode von auf `SKCanvasView` :

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Der-Befehl `InvalidateSurface` generiert effektiv einen aufzurufenden- `PaintSurface` Handler, der das-Feld verwendet, `showFill` um den Kreis auszufüllen oder nicht auszufüllen:

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

Die- `StrokeWidth` Eigenschaft wurde auf 50 festgelegt, um den Unterschied hervorzuheben. Sie können auch die gesamte Linienstärke sehen, indem Sie zuerst das innere und dann die Kontur zeichnen. Standardmäßig verbergen Grafik Abbildungen, die später im-Ereignishandler gezeichnet werden, jene, die `PaintSurface` zuvor im-Handler gezeichnet wurden.

Die Seite **Farb Erkundung** zeigt, wie Sie skiasharp-Grafiken auch mit anderen Xamarin.Forms Elementen integrieren können. Außerdem wird der Unterschied zwischen zwei alternativen Methoden zum Definieren von Farben in skiasharp veranschaulicht. Die statische [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl(System.Single,System.Single,System.Single,System.Byte)) -Methode erstellt einen- `SKColor` Wert, der auf dem Hue-Sättigung-Helligkeit-Modell basiert:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Die statische [`SKColor.FromHsv`](xref:SkiaSharp.SKColor.FromHsv(System.Single,System.Single,System.Single,System.Byte)) -Methode erstellt einen- `SKColor` Wert auf Grundlage des ähnlichen Hue-Sättigungswert-Modells:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

In beiden Fällen liegt das `h` Argument zwischen 0 und 360. Die `s` `l` Argumente, und `v` reichen von 0 bis 100. Das `a` Argument (Alpha oder Deckkraft) liegt zwischen 0 und 255.

Die [**colorexplorepage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) -Datei erstellt zwei `SKCanvasView` -Objekte `StackLayout` nebeneinander mit `Slider` `Label` den Sichten und, die es dem Benutzer ermöglichen, HSL-und HSV-Farbwerte auszuwählen:

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

Die beiden `SKCanvasView` Elemente befinden sich in einer Einzelzelle, in `Grid` `Label` der sich der resultierende RGB-Farbwert befindet.

Die [**ColorExplorePage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) -Code Behind-Datei ist relativ einfach. Der freigegebene `ValueChanged` Handler für die drei `Slider` Elemente macht einfach beide `SKCanvasView` Elemente ungültig. Die `PaintSurface` Handler löschen den Zeichenbereich mit der Farbe, die von den `Slider` Elementen angegeben wird, und legen außerdem die `Label` sitzungsete auf den `SKCanvasView` Elementen fest:

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

Sowohl im HSL-als auch im HSV-Farbmodell reicht der Hue-Wert von 0 bis 360 und gibt den vorherrschenden Farbton der Farbe an. Dabei handelt es sich um die traditionellen Farben des Regenbogens: rot, Orange, gelb, grün, blau, Indigo, violett und zurück in einem Kreis zu rot.

Im HSL-Modell ist der Wert 0 für Helligkeit immer schwarz, und ein 100-Wert ist immer weiß. Wenn der Sättigungswert 0 beträgt, sind die Helligkeit-Werte zwischen 0 und 100 Grauschattierungen. Das Erhöhen der Sättigung erhöht die Farbe. Reine Farben (bei denen es sich um RGB-Werte handelt, bei denen eine Komponente gleich 255, eine andere gleich 0 und der dritte Bereich zwischen 0 und 255 ist), wenn die Sättigung 100 und die Helligkeit 50 ist.

Im HSV-Modell ergeben sich reine Farben, wenn die Sättigung und der Wert 100 sind. Wenn value gleich 0 (null) ist, ist die Farbe unabhängig von anderen Einstellungen schwarz. Graue Schattierungen treten auf, wenn die Sättigung 0 ist und der Wert zwischen 0 und 100 liegt.

Die beste Möglichkeit, ein Gefühl für die beiden Modelle zu bekommen, besteht darin, sich selbst mit Ihnen zu experimentieren:

[![](integration-images/colorexplore-large.png "Triple screenshot of the Color Explore page")](integration-images/colorexplore-small.png#lightbox "Triple screenshot of the Color Explore page")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
