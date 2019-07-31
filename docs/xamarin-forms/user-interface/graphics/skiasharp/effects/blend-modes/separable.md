---
title: Die trennbare Füllmethoden einheitlich
description: Verwenden Sie die trennbare Füllmethoden einheitlich, um die Farben rote, grüne und blaue zu ändern.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 66D1A537-A247-484E-B5B9-FBCB7838FBE9
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 4fb5bd1e883adc3be89bde7cc0e1529e77165247
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655043"
---
# <a name="the-separable-blend-modes"></a>Die trennbare Füllmethoden einheitlich

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

In diesem Artikel haben Sie gesehen [ **SkiaSharp Porter-Duff Füllmethoden**](porter-duff.md), die Porter-Duff Blend-Modi werden in der Regel Clipping-Vorgänge ausführen. Die trennbare Füllmethoden einheitlich unterscheiden. Die trennbare Modi geändert werden, die einzelne roten, grünen und blauen Farbkomponenten eines Bilds. Trennbare Füllmethoden einheitlich können mischen Sie Farbe zu zeigen, dass die Kombination von Rot, Grün und Blau in der Tat weiß ist:

![Primärfarben](separable-images/SeparableSample.png "Primärfarben")

## <a name="lighten-and-darken-two-ways"></a>Aufhellen und auf zwei Arten Abdunkeln 

Es ist üblich, eine Bitmap zu verwenden, die ein wenig zu dunkel oder zu hell. Sie können trennbare Füllmethoden einheitlich heller oder dunkler der Imabe.  Tatsächlich zwei die trennbare Blend-Modi, in der [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode) Enumeration heißen `Lighten` und `Darken`. 

Diesen beiden Modi werden veranschaulicht die **heller und dunkler** Seite. Die XAML-Datei instanziiert zwei `SKCanvasView` Objekte und zwei `Slider` Ansichten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.LightenAndDarkenPage"
             Title="Lighten and Darken">
    <StackLayout>
        <skia:SKCanvasView x:Name="lightenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="lightenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />

        <skia:SKCanvasView x:Name="darkenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="darkenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

Die erste `SKCanvasView` und `Slider` veranschaulichen `SKBlendMode.Lighten` und veranschaulicht, das zweite Paar `SKBlendMode.Darken`. Die beiden `Slider` Ansichten verwenden dieselbe `ValueChanged` Handler und die beiden `SKCanvasView` verwenden dieselbe `PaintSurface` Handler. Beide Event Handler Check-Objekt, das Ereignis ausgelöst wird:

```csharp
public partial class LightenAndDarkenPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                typeof(SeparableBlendModesPage),
                "SkiaSharpFormsDemos.Media.Banana.jpg");

    public LightenAndDarkenPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if ((Slider)sender == lightenSlider)
        {
            lightenCanvasView.InvalidateSurface();
        }
        else
        {
            darkenCanvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest size rectangle in canvas
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);
        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap
        canvas.DrawBitmap(bitmap, rect);

        // Display gray rectangle with blend mode
        using (SKPaint paint = new SKPaint())
        {
            if ((SKCanvasView)sender == lightenCanvasView)
            {
                byte value = (byte)(255 * lightenSlider.Value);
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Lighten;
            }
            else
            {
                byte value = (byte)(255 * (1 - darkenSlider.Value));
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Darken;
            }

            canvas.DrawRect(rect, paint);
        }
    }
}
```

Die `PaintSurface` Handler berechnet ein Rechteck, das für die Bitmap geeignet ist. Der Ereignishandler zeigt diese Bitmap und anschließend ein Rechteck über die Bitmap mit einer `SKPaint` Objekt mit der `BlendMode` -Eigenschaftensatz auf `SKBlendMode.Lighten` oder `SKBlendMode.Darken`. Die `Color` -Eigenschaft ist eine grauschattierung basierend auf den `Slider`. Für die `Lighten` Modus, der Farbe reichen von Schwarz in weiß, aber die `Darken` Modus liegt der Bereich von weiß zu Schwarz.

Die Screenshots von links nach rechts zeigen immer größeren `Slider` Werte, wie das oberste Bild heller abgerufen, und das untere Bild dunkler:

[![Heller und dunkler](separable-images/LightenAndDarken.png "heller und dunkler angezeigt")](separable-images/LightenAndDarken-Large.png#lightbox)

Dieses Programm veranschaulicht die normale Art und Weise, in der die trennbaren Blend-Modi verwendet werden: Das Ziel ist ein Image einiger Art, sehr häufig eine Bitmap. Die Quelle ist ein Rechteck, das angezeigt wird, mit einer `SKPaint` Objekt mit der `BlendMode` -Eigenschaft auf einen trennbare Blend-Modus festgelegt. Das Rechteck kann eine Volltonfarbe werden (wie es hier der Fall ist) oder einem Farbverlauf. Transparenz ist _nicht_ in der Regel mit der trennbare Füllmethoden einheitlich verwendet.

Beim Experimentieren mit diesem Programm werden Sie feststellen, dass diese zwei Füllmethoden einheitlich nicht aufgehellt werden soll, und das Bild gleichmäßig Abdunkeln. Stattdessen die `Slider` scheint einen Schwellenwert für eine Art festzulegen. Erhöhen Sie zum Beispiel die `Slider` für die `Lighten` -Modus die dunkleren Bereiche des Bilds rufen Licht zuerst während der hellere Bereiche unverändert bleiben.

Für die `Lighten` Modus, wenn das Zielpixel den RGB-Farbwert (Dr, GD,-Db), und das Pixel Quelle befindet sich die Farbe (Sr, Sg, Sb), und klicken Sie dann die Ausgabe ist (oder Og, Ob) wie folgt berechnet:

 `Or = max(Dr, Sr)` `Og = max(Dg, Sg)`
 `Ob = max(Db, Sb)`

Für Rot, Grün und Blau ist das Ergebnis, desto größer der Quelle und Ziel. Hierdurch werden die Auswirkungen der Aufhellen zuerst die dunklen Bereiche des Ziels.

Die `Darken` Modus ist ähnlich, außer dass das Ergebnis kleiner als das Ziel und Quelle ist:

 `Or = min(Dr, Sr)` `Og = min(Dg, Sg)`
 `Ob = min(Db, Sb)`

Die Komponenten roten, grünen und blauen sind jeweils separat behandelt diese Modi blend wird als bezeichnet die _trennbare_ Füllmethoden. Aus diesem Grund die Abkürzungen **Dc** und **Sc** kann für das Ziel und die Farben einer Quelle verwendet werden, und es wird davon ausgegangen, dass Berechnungen separat für jede der Komponenten roten, grünen und blauen gelten.

Die folgende Tabelle zeigt die trennbare Blend-Modi mit kurze erläuterungen zu ihrer Funktion. Die zweite Spalte zeigt die Quellfarbe, die keine Änderung erzeugt:

| Blend-Modus   | Keine Änderung | Vorgang |
| ------------ | --------- | --------- |
| `Plus`       | Schwarz     | Lighdutzende durch Hinzufügen von Farben: SC + DC |
| `Modulate`   | Weiß     | Darkens durch Multiplikation von Farben: SC DC | 
| `Screen`     | Schwarz     | Ergänzt das Produkt von Ergänzungen: SC + DC &ndash; SC DC |
| `Overlay`    | Grau      | Quantile `HardLight` |
| `Darken`     | Weiß     | Mindestanzahl von Farben: min (Sc, Dc) |
| `Lighten`    | Schwarz     | Maximale von Farben: Max (Sc, Dc) |
| `ColorDodge` | Schwarz     | Aufgehellt Ziel wird basierend auf Quelle |
| `ColorBurn`  | Weiß     | Dunkelt Ziel wird basierend auf Quelle | 
| `HardLight`  | Grau      | Auswirkungen von Spot ähnelt |
| `SoftLight`  | Grau      | Ähnlich wie bei der Auswirkung der weiche spotlight | 
| `Difference` | Schwarz     | Subtrahiert die dunkleren vom helleren: Abs (DC &ndash; SC) | 
| `Exclusion`  | Schwarz     | Ähnlich wie `Difference` jedoch niedrigere Kontrast |
| `Multiply`   | Weiß     | Darkens durch Multiplikation von Farben: SC DC |

Ausführlichere Algorithmen finden Sie in der W3C [ **zusammensetzt und auf Ebene1 Blending** ](https://www.w3.org/TR/compositing-1/) -Spezifikation und die Skia [ **SkBlendMode Verweis** ](https://skia.org/user/api/SkBlendMode_Reference), obwohl die Schreibweise in diesen beiden Quellen nicht identisch ist. Beachten Sie, dass `Plus` wird häufig als Mischmodus Porter-Duff, betrachtet und `Modulate` ist nicht Teil der W3C-Spezifikation.

Wenn die Quelle transparent ist, klicken Sie dann für die trennbare Füllmethoden außer `Modulate`, Blend-Modus hat keine Auswirkungen. Wie Sie zuvor gesehen haben die `Modulate` Blend-Modus umfasst den alpha-Kanal in der Multiplikation. Andernfalls `Modulate` hat dieselbe Wirkung wie das `Multiply`. 

Beachten Sie, dass die beiden Modi, die mit dem Namen `ColorDodge` und `ColorBurn`. Die Wörter _umgehen_ und _brennen_ photographic Dunkelkammer Vorgehensweisen stammt. Ein Enlarger macht einen photographic Druck von Licht fort, über eine Negative. Mit kein Licht ist das Drucken weiß. Drucken ruft dunkler, je mehr Licht auf das Print für einen längeren Zeitraum fällt. Drucken-Entwickler verwendet häufig Hand oder kleinen Teil der Licht von einem bestimmten Teil des drucken vornehmen, die diesem Bereich heller zugeschrieben werden blockiert. Dies bezeichnet man als _abwedeln_. Dagegen nicht transparente Material mit eine Lücke in den (oder blockieren die meisten des Lichts Hände) verwendet werden, um mehr Licht an einer bestimmten Stelle nichtlineare Abdunkeln direkte _brennen_.

Die **umgehen und Burn** Programm ähnelt **heller und dunkler**. Die XAML-Datei ist die gleichen, jedoch mit anderen Elementnamen, strukturiert und Code-Behind-Datei ist ebenfalls sehr ähnlich, aber die Auswirkungen dieser beiden Füllmethoden einheitlich unterscheidet:

[![Umgehen und Burn](separable-images/DodgeAndBurn.png "umgehen und Burn")](separable-images/DodgeAndBurn-Large.png#lightbox)

Für kleine `Slider` Werte, die `Lighten` Modus hellt dunkle Bereiche zunächst zwar `ColorDodge` hellt mehr einheitlich.

Bildverarbeitung Anwendungsprogramme können häufig abwedeln und Brennen von CDs zu bestimmten Bereichen, genau wie in einem Dunkelkammer begrenzt werden. Dies kann durch Farbverläufe oder durch eine Bitmap mit unterschiedlichen grauschattierungen erfolgen.

## <a name="exploring-the-separable-blend-modes"></a>Untersuchen die trennbare Füllmethoden einheitlich

Die **trennbare Blend-Modi** auf der Seite können Sie alle trennbare Blend-Modi zu untersuchen. Ein Bitmap-Ziel und Quelle über eine der Füllmethoden einheitlich farbiges Rechteck angezeigt. 

Die XAML-Datei definiert eine `Picker` (Auswahl Blend-Modus) und vier Schieberegler. Die ersten drei Schieberegler können Sie die Komponenten roten, grünen und blauen der Quelle festlegen. Der vierte Schieberegler soll diese Werte zu überschreiben, indem Sie eine grauschattierung festlegen. Die einzelnen Schieberegler nicht erkannt, aber Farben kennzeichnen ihrer Funktion:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.SeparableBlendModesPage"
             Title="Separable Blend Modes">

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
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                    <x:Static Member="skia:SKBlendMode.Screen" />
                    <x:Static Member="skia:SKBlendMode.Overlay" />
                    <x:Static Member="skia:SKBlendMode.Darken" />
                    <x:Static Member="skia:SKBlendMode.Lighten" />
                    <x:Static Member="skia:SKBlendMode.ColorDodge" />
                    <x:Static Member="skia:SKBlendMode.ColorBurn" />
                    <x:Static Member="skia:SKBlendMode.HardLight" />
                    <x:Static Member="skia:SKBlendMode.SoftLight" />
                    <x:Static Member="skia:SKBlendMode.Difference" />
                    <x:Static Member="skia:SKBlendMode.Exclusion" />
                    <x:Static Member="skia:SKBlendMode.Multiply" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="redSlider"
                MinimumTrackColor="Red"
                MaximumTrackColor="Red"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="greenSlider"
                MinimumTrackColor="Green"
                MaximumTrackColor="Green"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="blueSlider"
                MinimumTrackColor="Blue"
                MaximumTrackColor="Blue"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="graySlider"
                MinimumTrackColor="Gray"
                MaximumTrackColor="Gray"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="colorLabel"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei lädt eine Bitmap-Ressource und zeichnet es zweimal einmal in der oberen Hälfte des Zeichenbereichs und erneut in der unteren Hälfte des Zeichenbereichs:

```csharp
public partial class SeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(SeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg"); 

    public SeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        if (sender == graySlider)
        {
            redSlider.Value = greenSlider.Value = blueSlider.Value = graySlider.Value;
        }

        colorLabel.Text = String.Format("Color = {0:X2} {1:X2} {2:X2}",
                                        (byte)(255 * redSlider.Value),
                                        (byte)(255 * greenSlider.Value),
                                        (byte)(255 * blueSlider.Value));

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw bitmap in top half
        SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Draw bitmap in bottom halr
        rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Get values from XAML controls
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        SKColor color = new SKColor((byte)(255 * redSlider.Value),
                                    (byte)(255 * greenSlider.Value),
                                    (byte)(255 * blueSlider.Value));

        // Draw rectangle with blend mode in bottom half
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Gegen Ende der `PaintSurface` Handler, ein Rechteck über die zweite Bitmap mit der ausgewählten Blendingmodus und die ausgewählte Farbe gezeichnet wird. Sie können die geänderte Bitmap unten mit der ursprünglichen Bitmap am oberen vergleichen:

[![Trennbare Füllmethoden einheitlich](separable-images/SeparableBlendModes.png "trennbare Füllmethoden einheitlich")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>Additiv und Subtraktive Primärfarben

Die **Primärfarben** Seite zeichnet die drei überlappende Kreise von Rot, Grün und Blau:

[![Additive Primärfarben](separable-images/PrimaryColors-Additive.png "additiv Primärfarben")](separable-images/PrimaryColors-Additive.png#lightbox)

Hierbei handelt es sich um die additive Grundfarben. Kombinationen von zwei erzeugen, Cyan, Magenta und Gelb, und eine Kombination aus allen drei ist weiß.

Diese drei Kreisen gezeichnet werden, mit der `SKBlendMode.Plus` Modus, aber Sie können auch `Screen`, `Lighten`, oder `Difference` für den gleichen Effekt. Hier ist das Programm:

```csharp
public class PrimaryColorsPage : ContentPage
{
    bool isSubtractive;

    public PrimaryColorsPage ()
    {
        Title = "Primary Colors";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Switch between additive and subtractive primaries at tap
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isSubtractive ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        float radius = Math.Min(info.Width, info.Height) / 4;
        float distance = 0.8f * radius;     // from canvas center to circle center
        SKPoint center1 = center + 
            new SKPoint(distance * (float)Math.Cos(9 * Math.PI / 6),
                        distance * (float)Math.Sin(9 * Math.PI / 6));
        SKPoint center2 = center +
            new SKPoint(distance * (float)Math.Cos(1 * Math.PI / 6),
                        distance * (float)Math.Sin(1 * Math.PI / 6));
        SKPoint center3 = center +
            new SKPoint(distance * (float)Math.Cos(5 * Math.PI / 6),
                        distance * (float)Math.Sin(5 * Math.PI / 6));

        using (SKPaint paint = new SKPaint())
        {
            if (!isSubtractive)
            {
                paint.BlendMode = SKBlendMode.Plus; 
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Red;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Lime;    // == (00, FF, 00)
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Blue;
                canvas.DrawCircle(center3, radius, paint);
            }
            else
            {
                paint.BlendMode = SKBlendMode.Multiply
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Cyan;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Magenta;
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Yellow;
                canvas.DrawCircle(center3, radius, paint);
            }
        }
    }
}
```

Das Programm enthält eine `TabGestureRecognizer`. Wenn Sie tippen oder klicken Sie auf dem Bildschirm, verwendet die Anwendung `SKBlendMode.Multiply` die drei Subtraktive primären Replikate angezeigt:

[![Subtraktive Primärfarben](separable-images/PrimaryColors-Subtractive.png "Subtraktive Primärfarben")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

Die `Darken` Modus funktioniert auch für diesen Effekt.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
