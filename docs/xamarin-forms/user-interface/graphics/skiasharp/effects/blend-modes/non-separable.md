---
title: Die nicht trennbare Füllmethoden einheitlich
description: Verwenden Sie die nicht trennbare Füllmethoden einheitlich Farbton, Sättigung und Helligkeit zu ändern.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97FA2730-87C0-4914-8C9F-C64A02CF9EEF
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 07df66e69186803e3322bd71a4b3b37710655de4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131493"
---
# <a name="the-non-separable-blend-modes"></a>Die nicht trennbare Füllmethoden einheitlich

In diesem Artikel haben Sie gesehen [ **SkiaSharp trennbare Füllmethoden**](separable.md), die trennbare Füllmethoden einheitlich Vorgänge für den Rot-, Grün- und blauen-Kanälen separat ausführen. Die Modi nicht trennbare Blend nicht der Fall ist. Durch das Arbeiten auf den Farbton, Sättigung und Helligkeit Ebenen der Farbe, können der nicht trennbare Füllmethoden einheitlich Farben auf interessante Weise zu ändern:

![Nicht trennbare Beispiel](non-separable-images/NonSeparableSample.png "nicht trennbare-Beispiel")

## <a name="the-hue-saturation-luminosity-model"></a>Der Farbton-Sättigung-Helligkeit-Modell

Um die nicht trennbare Füllmethoden einheitlich zu verstehen, ist es erforderlich, die Pixel der Ziel- und als Farben im Modell Farbton-Sättigung-Helligkeit behandelt werden sollen. (Helligkeit wird auch als Helligkeit bezeichnet.)

Das Modell der HSL-Farbe wurde in diesem Artikel erläuterten [ **Integrieren von Xamarin.Forms** ](../../basics/integration.md) und ein Beispielprogramm in diesem Artikel ermöglicht das Experimentieren mit HSL-Farben. Sie erstellen eine `SKColor` -Wert mithilfe der Werte für Farbton, Sättigung und Helligkeit mit der statischen [ `SKColor.FromHsl` ](xref:SkiaSharp.SKColor.FromHsl*) Methode.

Der Farbton stellt die vorherrschende Wellenlänge der Farbe dar. Hue Werte zwischen 0 und 360 liegen, und durchlaufen Sie die primärstandorte additiv und Subtraktive: Red ist der Wert 0 (null) Gelb ist 60, Grün ist 120, Cyan ist 180, Blau ist 240 Magenta ist 300 und der Zyklus wird wieder in Rot auf 360.

Es ist keine dominanten Farbe &mdash; ist z. B. die Farbe Weiß oder Schwarz oder eine grauschattierung &mdash; der Farbton ist nicht definiert und in der Regel auf 0 festgelegt. 

Die Sättigung-Werte können zwischen 0 und 100 liegen und die Reinheit der Farbe anzugeben. Der Sättigungswert 100 ist die ursprünglichen Farbe an, während Werte kleiner als 100 dazu führen, die Farbe dass, die mehr grayish werden. Führt eine graue Schattierung der Sättigungswert 0.

Die Brillanz (oder Helligkeit) Wert gibt an, wie die helle Farbe ist. Die der Helligkeitswert 0 ist schwarz, unabhängig von den anderen Einstellungen. Auf ähnliche Weise ist ein Helligkeitswert von 100 weiß. 

Der HSL-Wert (0, 100, 50) ist der RGB-Wert (FF, 00, 00), der reine Rot ist. Der HSL-Wert ("180", "100", "50") ist der RGB-Wert (00, FF, FF), reine Cyan. Wie die Sättigung verringert wird, die dominanten Farbe-Komponente wird verringert, und die anderen Komponenten werden erhöht. Auf einer Überlastung des Netzwerks Ebene 0 alle Komponenten sind identisch, und die Farbe ist eine grauschattierung. Verringern Sie die Helligkeit zu auf Schwarz festgelegt; Erhöhen Sie die Helligkeit zu Weiß.

## <a name="the-blend-modes-in-detail"></a>Die Blend-Modi im detail

Wie die anderen Blend-Modi umfassen die vier nicht trennbare Füllmethoden einheitlich an ein Ziel (die häufig ein Bitmap-Bild) und eine Quelle, dies ist häufig eine einzelne Farbe oder einen Farbverlauf. Die Blend-Modi kombinieren Farbton, Sättigung und Helligkeit Werte aus dem Ziel und Quelle:

| Blend-Modus   | Komponenten von der Quelle | Ziel-Komponenten |
| ------------ | ---------------------- | --------------------------- |
| `Hue`        | Farbton                    | Sättigung und Helligkeit   |
| `Saturation` | Sättigung             | Hue und Helligkeit          |
| `Color`      | Hue und Sättigung     | Helligkeit                  | 
| `Luminosity` | Helligkeit             | Hue und Sättigung          | 

Finden Sie in der W3C [ **zusammensetzt und auf Ebene1 Blending** ](https://www.w3.org/TR/compositing-1/) Spezifikation für die Algorithmen.

Die **nicht trennbare Blend-Modi** Seite enthält eine `Picker` zum Auswählen eines diese blend-Modi und drei `Slider` Ansichten eine HSL-Farbe auswählen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.NonSeparableBlendModesPage"
             Title="Non-Separable Blend Modes">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Hue" />
                    <x:Static Member="skia:SKBlendMode.Saturation" />
                    <x:Static Member="skia:SKBlendMode.Color" />
                    <x:Static Member="skia:SKBlendMode.Luminosity" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="satSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="lumSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <StackLayout Orientation="Horizontal">
            <Label x:Name="hslLabel"
                   HorizontalOptions="CenterAndExpand" />

            <Label x:Name="rgbLabel"
                   HorizontalOptions="CenterAndExpand" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

Um Platz zu den drei sparen `Slider` Ansichten in der Benutzeroberfläche des Programms nicht erkannt. Sie müssen bedenken, dass die Reihenfolge Farbton, Sättigung und Helligkeit ist. Zwei `Label` Ansichten am unteren Rand der Seite zeigen die HSL und RGB-Farbwerte.

Die Code-Behind-Datei lädt eine Bitmap-Ressource, die anzeigt, die so groß wie möglich auf den Zeichenbereich, und klicken Sie dann behandelt der Canvas mit einem Rechteck. Die Farbe des Rechtecks basiert darauf, dass die drei `Slider` Ansichten und Blend-Modus ist der im ausgewählten der `Picker`:

```csharp
public partial class NonSeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(NonSeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKColor color;

    public NonSeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        // Calculate new color based on sliders
        color = SKColor.FromHsl((float)hueSlider.Value,
                                (float)satSlider.Value,
                                (float)lumSlider.Value);

        // Use labels to display HSL and RGB color values
        color.ToHsl(out float hue, out float sat, out float lum);

        hslLabel.Text = String.Format("HSL = {0:F0} {1:F0} {2:F0}",
                                      hue, sat, lum);

        rgbLabel.Text = String.Format("RGB = {0:X2} {1:X2} {2:X2}",
                                      color.Red, color.Green, color.Blue);

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        // Get blend mode from Picker
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Beachten Sie, dass das Programm nicht den Farbwert HSL als durch die drei Schieberegler ausgewählt angezeigt wird. Stattdessen einen Farbwert aus diese Schieberegler erstellt und verwendet dann die [ `ToHsl` ](xref:SkiaSharp.SKColor.ToHsl*) Methode, um die Werte für Farbton, Sättigung und Helligkeit zu erhalten. Grund hierfür ist die `FromHsl` -Methode konvertiert eine HSL-Farbe in eine RGB-Farbe, die intern in gespeichert wird, wird der `SKColor` Struktur. Die `ToHsl` -Methode konvertiert von RGB in HSL, aber das Ergebnis wird nicht immer der ursprüngliche Wert sein. 

Z. B. `FromHsl` den HSL-Wert ("180", "50", "0") in die RGB-Farbe (0, 0, 0) konvertiert werden, da die `Luminosity` ist 0 (null). Die `ToHsl` -Methode konvertiert die RGB-Farbe (0, 0, 0) in die HSL-Farbe (0, 0, 0), da die Werte für Farbton und Überlastung des Netzwerks nicht relevant sind. Wenn Sie dieses Programm verwenden zu können, es empfiehlt sich, dass die Darstellung der HSL-Farbe, die die Anwendung anstelle der verwendet wird angezeigt, Sie mit den Schiebereglern angegeben.

Die `SKBlendModes.Hue` Blendingmodus verwendet die Hue-Ebene, der die Quelle und behalten Sie die Ebenen Sättigung und Helligkeit des Ziels. Beim Testen dieser Blend-Modus müssen die Sättigung und Helligkeit Schieberegler etwas ungleich 0 oder 100 festgelegt werden, da in diesen Fällen der Farbton nicht eindeutig definiert ist.

[![Nicht trennbare Füllmethoden einheitlich - Hue](non-separable-images/NonSeparableBlendModes-Hue.png "nicht trennbare Füllmethoden einheitlich - Hue")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

Wenn Sie mithilfe des Schiebereglers auf 0 festgelegt (wie bei der iOS-Screenshot auf der linken Seite), alles Rötlich aktiviert. Dies bedeutet jedoch nicht, dass das Image vollständig fehlt der Grün und Blau. Es gibt natürlich noch grauschattierungen im Resultset vorhanden. Z. B. die RGB-Farbe (40, 40, C0) entspricht die HSL-Farbe (240, 50, 50). Der Farbton ist Blau, aber der Sättigungswert der 50 gibt an, dass es auch auf die Komponenten rote, grüne. Wenn der Farbton auf 0 festgelegt ist `SKBlendModes.Hue`, die HSL-Farbe ist (0, 50, 50), die die RGB-Farbe (C0, 40, 40). Es gibt noch blaue und grüne Komponenten, aber die vorherrschende Komponente ist jetzt Rot.

Die `SKBlendModes.Saturation` Mischmodus kombiniert die Sättigung der Quelle mit den Farbton und die Helligkeit des Ziels. Wie der Farbton ist die Sättigung nicht klar definiert, wenn die Helligkeit 0 oder 100 ist. Theoretisch sollte eine Einstellung Helligkeit zwischen diesen beiden extremen funktionieren. Scheint jedoch die Helligkeit-Einstellung, um das Ergebnis beeinflussen, mehr als sinnvoll. Die Helligkeit auf 50 festgelegt, und sehen Sie, wie Sie die Sättigung des Bilds festlegen können:

[![Nicht trennbare Füllmethoden einheitlich - Sättigung](non-separable-images/NonSeparableBlendModes-Saturation.png "nicht trennbare Füllmethoden einheitlich - Sättigung")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

Verwenden Sie diesen Blend-Modus, um die Farbe eines Images gesamteinwirkung Überlastung des Netzwerks zu erhöhen, oder Sie können die Sättigung auf 0 (null), (wie in der iOS-Screenshot auf der linken Seite) für die resultierende Image besteht aus nur grauschattierungen verringern.

Die `SKBlendModes.Color` Mischmodus behält die Helligkeit des Ziels verwendet jedoch die Hue und Sättigung der Quelle. In diesem Fall bedeutet, dass sich, dass jede Einstellung des Schiebereglers Helligkeit an einer beliebigen Stelle zwischen den extremen arbeiten sollten. 

[![Farbe, die nicht trennbare Füllmethoden einheitlich -](non-separable-images/NonSeparableBlendModes-Color.png "nicht trennbare Füllmethoden einheitlich - Farbe")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

Eine Anwendung dieses Blend-Modus wird in Kürze angezeigt werden.

Zum Schluss die `SKBlendModes.Luminosity` Blend-Modus ist das Gegenteil von `SKBlendModes.Color`. Er behält die Hue und die Sättigung des Ziels jedoch die Helligkeit der Quelle verwendet. Die `Luminosity` Blendingmodus ist die am häufigsten mysteriöse des Batches: der Farbton und Sättigung Schieberegler Auswirkungen auf das Bild, aber auch auf die mittlere Helligkeit, das Image ist nicht eindeutig:

[![Nicht trennbare Füllmethoden einheitlich - Helligkeit](non-separable-images/NonSeparableBlendModes-Luminosity.png "nicht trennbare Füllmethoden einheitlich - Helligkeit")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

Theoretisch sollte erhöhen oder verringern die Helligkeit eines Bilds heller oder dunkler zu vereinfachen. Interessanterweise der [Helligkeit-Beispiel in der Skia **SkBlendMode Verweis** ](https://skia.org/user/api/SkBlendMode_Reference#Luminosity) sehr ähnlich ist.

Es ist in der Regel nicht der Fall, den Sie möchten einen der Modi nicht trennbare Blend mit einer Quelle zu verwenden, die besteht aus einer einzelnen Farbe, die auf das Zielbild für die gesamte angewendet werden. Der Effekt ist einfach zu groß. Sie sollten die Auswirkungen auf einen Teil des Bilds zu beschränken. In diesem Fall die Quelle wird wahrscheinlich Transparancy integrieren, oder vielleicht wird die Quelle auf eine kleinere Grafik beschränkt sein.

## <a name="a-matte-for-a-separable-mode"></a>Eine Maske für einen trennbare Modus

Hier ist eine der Bitmaps enthalten, die als Ressource in der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel. Der Dateiname ist **Banana.jpg**:

![Banane Monkey](non-separable-images/Banana.jpg "Banane Monkey-Objekt")

Es ist möglich, eine Maske zu erstellen, die nur die Banane umfasst. Dies ist auch eine Ressource in der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel. Der Dateiname ist **BananaMatte.png**:

![Banane Matte](non-separable-images/BananaMatte.png "Banane-Maske")

Abgesehen von der Form "Schwarz Banane" ist der Rest der Bitmap transparent.

Die **blaue Banane** Seite verwendet diese Maske zum Ändern der Farbton und die Sättigung der Banane, die das Monkey-Objekt enthält, jedoch nichts in der Abbildung zu ändern. 

In der folgenden `BlueBananaPage` -Klasse, die **Banana.jpg** Bitmap als Feld geladen wird. Das Laden der Konstruktor der **BananaMatte.png** als bitmap der `matteBitmap` -Objekt, aber es wird das Objekt länger als der Konstruktor nicht beibehalten. Eine dritte Bitmap stattdessen mit dem Namen `blueBananaBitmap` erstellt wird. Die `matteBitmap` Zeichnen auf `blueBananaBitmap` gefolgt von einer `SKPaint` mit der `Color` auf Blau festgelegt und die zugehörige `BlendMode` festgelegt `SKBlendMode.SrcIn`. Die `blueBananaBitmap` bleibt größtenteils transparent, aber es wird ein solid reine Blau-Image von der Banane:

```csharp
public class BlueBananaPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BlueBananaPage),
        "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap blueBananaBitmap;

    public BlueBananaPage()
    {
        Title = "Blue Banana";

        // Load banana matte bitmap (black on transparent)
        SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
            typeof(BlueBananaPage),
            "SkiaSharpFormsDemos.Media.BananaMatte.png");

        // Create a bitmap with a solid blue banana and transparent otherwise
        blueBananaBitmap = new SKBitmap(matteBitmap.Width, matteBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(blueBananaBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(matteBitmap, new SKPoint(0, 0));

            using (SKPaint paint = new SKPaint())
            {
                paint.Color = SKColors.Blue;
                paint.BlendMode = SKBlendMode.SrcIn;
                canvas.DrawPaint(paint);
            }
        }

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = SKBlendMode.Color;
            canvas.DrawBitmap(blueBananaBitmap, 
                              info.Rect, 
                              BitmapStretch.Uniform, 
                              paint: paint);
        }
    }
}
```

Die `PaintSurface` Handler für die Bitmap mit das Monkey-Objekt enthält die Banane gezeichnet. Dieser Code wird die Anzeige von gefolgt `blueBananaBitmap` mit `SKBlendMode.Color`. Jedes Pixels Farbton und Sättigung wird über die Oberfläche der Banane durch die stetig blau, der Wert 240 Hue und der Sättigungswert 100 entspricht ersetzt. Die Helligkeit, bleibt jedoch gleich, was bedeutet, dass die Banane weiterhin eine realistische Textur trotz der neuen Farbe:

[![Blau Banane](non-separable-images/BlueBanana.png "Blau Banane")](non-separable-images/BlueBanana-Large.png#lightbox)

Versuchen Sie es ändern des Blendingmodus auf `SKBlendMode.Saturation`. Die Banane bleibt gelb, aber es ist ein intensiver gelb. Dies kann in einer realen Anwendung einen wünschenswerter Effekt als aktivieren die Banane Blau sein.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)