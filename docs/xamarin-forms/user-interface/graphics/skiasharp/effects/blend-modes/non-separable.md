---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 52be7641ac3b2983f537e11bccd76f2a5b52574d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130181"
---
# <a name="the-non-separable-blend-modes"></a>Die nicht trennbaren Blend-Modi

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Wie Sie im Artikel [**skiasharp trennbare Blend-Modi**](separable.md)gesehen haben, führen die trennbaren Blend-Modi Vorgänge für die roten, grünen und blauen Kanäle separat aus. Die nicht trennbaren Blend-Modi. Durch die Funktionsweise der Farb-, Sättigungs-und Helligkeits Ebenen von Farben können die Farben durch die nicht trennbaren Blend-Modi auf interessante Weise geändert werden:

![Nicht trennbare Stichprobe](non-separable-images/NonSeparableSample.png "Nicht trennbare Stichprobe")

## <a name="the-hue-saturation-luminosity-model"></a>Das Hue-Sättigung-Luminosity-Modell

Um die nicht trennbaren Blend-Modi zu verstehen, ist es erforderlich, die Ziel-und Quell Pixel als Farben im Hue-Sättigung-Luminosity-Modell zu behandeln. (Helligkeit wird auch als Helligkeit bezeichnet.)

Das HSL-Farbmodell wurde im Artikel [**integrieren Xamarin.Forms **](../../basics/integration.md) in und ein Beispielprogramm in diesem Artikel erläutert, das Experimente mit HSL-Farben ermöglicht. `SKColor`Mit der statischen-Methode können Sie einen-Wert mit Hue-, Sättigungs-und Helligkeits Werten erstellen [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl*) .

Der Farbton stellt die dominierende Wellen Wellen der Farbe dar. Die Hue-Werte reichen von 0 bis 360 und durchlaufen die Additiven und subtraktiven primären Replikate: rot ist der Wert 0, gelb ist 60, grün ist 120, Zyan ist 180, Blue ist 240, Magenta ist 300, und der Cycle wechselt wieder zu rot bei 360.

Wenn z. b. keine vorherrschende Farbe vorhanden ist &mdash; , ist die Farbe weiß oder schwarz oder ein grauer Farbton, &mdash; dann ist der Farbton nicht definiert und wird normalerweise auf 0 festgelegt. 

Die Sättigungswerte können zwischen 0 und 100 liegen und die Reinheit der Farbe angeben. Ein Sättigungswert von 100 ist die reinste Farbe, während Werte kleiner als 100 bewirken, dass die Farbe stärker grau wird. Ein Sättigungswert von 0 führt zu einem grauen Farbton.

Der Wert für Helligkeit (oder Helligkeit) gibt an, wie hell die Farbe ist. Der Wert für die Leuchtkraft 0 ist unabhängig von den anderen Einstellungen schwarz. Analog dazu ist der Wert für die Leuchtkraft 100 weiß. 

Der HSL-Wert (0, 100, 50) ist der RGB-Wert (FF, 00, 00), der rein rot ist. Der HSL-Wert (180, 100, 50) ist der RGB-Wert (00, FF, FF), reiner Cyan. Wenn die Sättigung verringert wird, wird die dominierende Farbkomponente reduziert, und die anderen Komponenten werden erweitert. Bei einem Sättigungsgrad von 0 sind alle Komponenten identisch, und die Farbe ist ein grauer Farbton. Verringern Sie die Helligkeit, um zu schwarz zu wechseln. erhöhen Sie die Leuchtkraft, um zu weiß zu wechseln.

## <a name="the-blend-modes-in-detail"></a>Die Blend-Modi im Detail

Wie bei den anderen Blend-Modi beinhalten die vier nicht trennbaren Blend-Modi ein Ziel (häufig ein Bitmapbild) und eine Quelle, bei der es sich häufig um eine Farbe oder einen Farbverlauf handelt. Die Blend-Modi kombinieren Farbton-, Sättigungs-und Helligkeitswerte vom Ziel und der Quelle:

| Blend-Modus   | Komponenten aus Quelle | Komponenten vom Ziel |
| ---
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

------ | ---Titel: Description: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

----------- | ---Titel: Description: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
Title: Beschreibung: ms. Prod: ms. Technology: ms. assetid: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-------------- | | `Hue`        | Farbton | Sättigung und Helligkeit | | `Saturation` | Sättigung | Farbton und Helligkeit | | `Color`      | Farbton und Sättigung | Helligkeit | | `Luminosity` | Helligkeit | Farbton und Sättigung | 

Weitere Informationen finden Sie in der Spezifikation der W3C-Zusammensetzung [**und-Blending der Ebene 1**](https://www.w3.org/TR/compositing-1/) .

Die Seite **nicht getrennte Blend-Modi** enthält eine `Picker` zum Auswählen eines dieser Blend-Modi und drei `Slider` Ansichten, um eine HSL-Farbe auszuwählen:

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

Um Speicherplatz zu sparen, `Slider` werden die drei Sichten nicht in der Benutzeroberfläche des Programms identifiziert. Sie müssen daran denken, dass die Reihenfolge Hue, Sättigung und Helligkeit ist. `Label`In zwei Ansichten unten auf der Seite werden die HSL-und RGB-Farbwerte angezeigt.

Die Code-Behind-Datei lädt eine der Bitmapressourcen, zeigt diese so groß wie möglich im Zeichenbereich an und deckt dann den Zeichenbereich mit einem Rechteck ab. Die Rechteck Farbe basiert auf den drei `Slider` Ansichten, und der Blend-Modus ist der, der in der ausgewählt wurde `Picker` :

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

Beachten Sie, dass das Programm den von den drei Schiebereglern ausgewählten HSL-Farbwert nicht anzeigt. Stattdessen wird von diesen Schiebereglern ein Farbwert erstellt und dann die-Methode verwendet, [`ToHsl`](xref:SkiaSharp.SKColor.ToHsl*) um die Werte für Farbton, Sättigung und Helligkeit zu erhalten. Dies liegt daran, dass die- `FromHsl` Methode eine HSL-Farbe in eine RGB-Farbe konvertiert, die intern in der-Struktur gespeichert wird `SKColor` . Die- `ToHsl` Methode konvertiert von RGB in HSL, das Ergebnis ist jedoch nicht immer der ursprüngliche Wert. 

`FromHsl`Konvertiert z. b. den HSL-Wert (180, 50, 0) in die RGB-Farbe (0, 0, 0), da der 0 ( `Luminosity` null) ist. Die `ToHsl` -Methode konvertiert die RGB-Farbe (0, 0, 0) in die HSL-Farbe (0, 0, 0), da die Farbton-und Sättigungswerte irrelevant sind. Wenn Sie dieses Programm verwenden, ist es besser, die Darstellung der vom Programm verwendeten HSL-Farbe und nicht die von Ihnen mit den Schiebereglern angegebene Darstellung der HSL-Farbe zu sehen.

Der `SKBlendModes.Hue` Blend-Modus verwendet die Hue-Ebene der Quelle und behält dabei die Sättigungs-und Helligkeitsstufen des Ziels bei. Wenn Sie diesen Blend-Modus testen, müssen die Schieberegler für Sättigung und Helligkeit auf einen anderen Wert als 0 oder 100 festgelegt werden, da der Farbton in diesen Fällen nicht eindeutig definiert ist.

[![Nicht trennbare Blend-Modi-Hue](non-separable-images/NonSeparableBlendModes-Hue.png "Nicht trennbare Blend-Modi-Hue")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

Wenn Sie den Schieberegler auf 0 festlegen (wie beim IOS-Screenshot auf der linken Seite), wird alles rot dargestellt. Dies bedeutet jedoch nicht, dass das Bild vollständig von Grün und blau fehlt. Natürlich sind im Ergebnis noch graue Schattierungen vorhanden. Beispielsweise entspricht die RGB-Farbe (40, 40, C0) der HSL-Farbe (240, 50, 50). Der Farbton ist blau, aber der Sättigungswert von 50 zeigt an, dass auch rote und grüne Komponenten vorhanden sind. Wenn für Hue der Wert 0 (null) festgelegt ist `SKBlendModes.Hue` , ist die HSL-Farbe (0, 50, 50), die RGB-Farbe (C0, 40, 40). Es gibt immer noch blaue und grüne Komponenten, aber jetzt ist die dominierende Komponente rot.

Der `SKBlendModes.Saturation` Blend-Modus kombiniert den Sättigungsgrad der Quelle mit dem Farbton und der Helligkeit des Ziels. Wie bei Hue ist die Sättigung nicht wohl definiert, wenn die Helligkeit 0 oder 100 ist. Theoretisch sollte jede Leuchtkraft Einstellung zwischen diesen beiden extremen funktionieren. Die Einstellung für die Helligkeit wirkt sich allerdings nicht auf das Ergebnis aus. Legen Sie die Helligkeit auf 50 fest, und Sie können sehen, wie Sie den Sättigungsgrad des Bilds festlegen können:

[![Nicht trennbare Blend-Modi-Sättigung](non-separable-images/NonSeparableBlendModes-Saturation.png "Nicht trennbare Blend-Modi-Sättigung")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

Sie können diesen Blend-Modus verwenden, um die Farbsättigung eines trüben Bilds zu vergrößern, oder Sie können die Sättigung auf NULL (wie im IOS-Screenshot auf der linken Seite) für ein Ergebnisbild verringern, das nur aus grauen Schattierungen besteht.

Der `SKBlendModes.Color` Blend-Modus behält die Helligkeit des Ziels bei, verwendet jedoch den Farbton und die Sättigung der Quelle. Dies bedeutet auch, dass jede Einstellung des Schiebereglers für die Helligkeit irgendwo zwischen den extremen funktionieren sollte. 

[![Nicht trennbare Blend-Modi-Farbe](non-separable-images/NonSeparableBlendModes-Color.png "Nicht trennbare Blend-Modi-Farbe")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

In Kürze wird eine Anwendung dieses Blend-Modus angezeigt.

Schließlich ist der `SKBlendModes.Luminosity` Blend-Modus das Gegenteil von `SKBlendModes.Color` . Er behält den Farbton und die Sättigung des Ziels bei, verwendet jedoch die Helligkeit der Quelle. Der `Luminosity` Blend-Modus ist der größte Teil des Batches: die Schieberegler Hue und Sättigung wirken sich auf das Bild aus, aber selbst bei mittlerer Helligkeit ist das Bild nicht eindeutig:

[![Nicht trennbare Blend-Modi: Helligkeit](non-separable-images/NonSeparableBlendModes-Luminosity.png "Nicht trennbare Blend-Modi: Helligkeit")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

Theoretisch sollten Sie die Helligkeit eines Bilds vergrößern oder verkleinern, um es heller oder dunkler zu machen. Interessanterweise ist das [Beispiel für die Leuchtkraft in der Skia- **skblendmode-Referenz** ](https://skia.org/user/api/SkBlendMode_Reference#Luminosity) sehr ähnlich.

Es ist in der Regel nicht der Fall, dass Sie einen der nicht trennbaren Blend-Modi mit einer Quelle verwenden möchten, die aus einer einzelnen Farbe besteht, die auf das gesamte Zielbild angewendet wird. Der Effekt ist einfach zu groß. Sie sollten die Auswirkung auf einen Teil des Bilds einschränken. In diesem Fall enthält die Quelle wahrscheinlich Transparenz, oder die Quelle wird auf eine kleinere Grafik beschränkt.

## <a name="a-matte-for-a-separable-mode"></a>Eine Matte für einen trennbaren Modus

Im folgenden finden Sie eine der Bitmaps, die als Ressource im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel enthalten sind. Der Dateiname ist " **Banane. jpg**":

![Banane-Affe](non-separable-images/Banana.jpg "Banane-Affe")

Es ist möglich, eine Matte zu erstellen, die nur die Banane umfasst. Dies ist auch eine Ressource im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel. Der Dateiname ist " **bananamatte. png**":

![Banane Matt](non-separable-images/BananaMatte.png "Banane Matt")

Abgesehen von der schwarzen Bananenform ist der Rest der Bitmap transparent.

Die **blaue Bananen** Seite verwendet diese Matte, um den Farbton und die Sättigung der Banane zu ändern, die der Affe hält, aber um nichts anderes im Bild zu ändern. 

In der folgenden `BlueBananaPage` Klasse wird die " **Banane. jpg** "-Bitmap als Feld geladen. Der Konstruktor lädt die " **bananamatte. png** "-Bitmap als `matteBitmap` Objekt, aber dieses Objekt wird nicht über den Konstruktor hinaus beibehalten. Stattdessen wird eine dritte Bitmap mit dem Namen `blueBananaBitmap` erstellt. Der `matteBitmap` wird am gezeichnet `blueBananaBitmap` , gefolgt von einem `SKPaint` `Color` , dessen auf blau und dessen auf `BlendMode` festgelegt ist `SKBlendMode.SrcIn` . Die `blueBananaBitmap` bleibt größtenteils transparent, aber mit einem reinen reinen blauen Bild der Banane:

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

Der `PaintSurface` Handler zeichnet die Bitmap mit dem Monkey, der die Banane hält. Auf diesen Code folgt die Anzeige von `blueBananaBitmap` mit `SKBlendMode.Color` . Auf der Oberfläche der Banane wird der Farbton und die Sättigung jedes Pixels durch das Vollton-blau ersetzt, das einem Hue-Wert von 240 und einem Sättigungswert von 100 entspricht. Die Helligkeit bleibt jedoch unverändert, was bedeutet, dass die Banane trotz der neuen Farbe weiterhin eine realistische Textur hat:

[![Blaue Banane](non-separable-images/BlueBanana.png "Blaue Banane")](non-separable-images/BlueBanana-Large.png#lightbox)

Versuchen Sie, den Blend-Modus in zu ändern `SKBlendMode.Saturation` . Die Banane bleibt Gelb, aber es ist ein intensiveres gelb. In einer realen Anwendung ist dies möglicherweise ein Sinn füglicheren Effekt als das Durchführen der Banane blau.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
