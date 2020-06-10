---
Title: "die trennbaren Blend-Modi" Description: "verwenden Sie die trennbaren Blend-Modi, um die Farben rot, grün und blau zu ändern."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 66d1a537-A247-484e-b5b9-fbcb7838fbe9 Author: davidbritch ms. Author: dabritch ms. Date: 08/23/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-separable-blend-modes"></a>Die trennbaren Blend-Modi

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Wie Sie im Artikel [**skiasharp Porter-Duff Blend-Modi**](porter-duff.md)gesehen haben, führen die Porter-Duff-Blend-Modi im allgemeinen Clipping-Vorgänge aus. Die trennbaren Blend-Modi sind unterschiedlich. Die trennbaren Modi ändern die einzelnen roten, grünen und blauen Farbkomponenten eines Bilds. Die trennbaren Blend-Modi können Farben vermischen, um zu veranschaulichen, dass die Kombination von rot, grün und blau tatsächlich weiß ist:

![Primärfarben](separable-images/SeparableSample.png "Primärfarben")

## <a name="lighten-and-darken-two-ways"></a>Zwei Arten von Lighten und Abdunkeln 

Es kommt häufig vor, dass eine Bitmap etwas zu dunkel oder zu hell ist. Sie können den trennbaren Blend-Modus verwenden, um die Imabe aufzuhellen oder zu verdunkeln.  Tatsächlich werden zwei der trennbaren Blend-Modi in der [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) -Enumeration mit dem Namen `Lighten` und benannt `Darken` . 

Diese beiden Modi werden auf der Seite " **Lighten und Darken** " veranschaulicht. In der XAML-Datei werden zwei `SKCanvasView` Objekte und zwei Sichten instanziiert `Slider` :

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

Der erste `SKCanvasView` und `Slider` der Veranschaulichung `SKBlendMode.Lighten` und das zweite Paar zeigen `SKBlendMode.Darken` . Die beiden `Slider` Sichten verwenden denselben `ValueChanged` Handler, und beide verwenden `SKCanvasView` denselben `PaintSurface` Handler gemeinsam. Beide Ereignishandler überprüfen, welches Objekt das Ereignis auslöst:

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

Der `PaintSurface` Handler berechnet ein Rechteck, das für die Bitmap geeignet ist. Der Handler zeigt diese Bitmap an und zeigt dann mithilfe eines-Objekts, dessen- `SKPaint` `BlendMode` Eigenschaft auf oder festgelegt ist, ein Rechteck über der Bitmap an `SKBlendMode.Lighten` `SKBlendMode.Darken` . Die- `Color` Eigenschaft ist ein grauer Farbton auf der Grundlage von `Slider` . Für den- `Lighten` Modus reicht die Farbe von Schwarz bis weiß aus, aber für den `Darken` Modus reicht sie von weiß bis schwarz aus.

Die Screenshots von links nach rechts zeigen zunehmend größere `Slider` Werte, da das obere Bild heller wird und das untere Bild dunkler wird:

[![Lighten und Darken](separable-images/LightenAndDarken.png "Lighten und Darken")](separable-images/LightenAndDarken-Large.png#lightbox)

Dieses Programm veranschaulicht die normale Art und Weise, in der die trennbaren Blend-Modi verwendet werden: das Ziel ist ein Bild einiger Art, häufig eine Bitmap. Die Quelle ist ein Rechteck, das mit einem-Objekt angezeigt wird `SKPaint` , dessen- `BlendMode` Eigenschaft auf einen trennbaren Blend-Modus festgelegt ist. Das Rechteck kann eine voll Tonfarbe (wie hier) oder ein Farbverlauf sein. Transparenz wird in der Regel _nicht_ mit den trennbaren Blend-Modi verwendet.

Wenn Sie mit diesem Programm experimentieren, werden Sie feststellen, dass diese beiden Blend-Modi das Bild nicht gleichmäßig Abblenden und Abdunkeln. Stattdessen `Slider` scheint einen Schwellenwert festzulegen. Wenn Sie z. b `Slider` . für den- `Lighten` Modus erhöhen, werden die dunkleren Bereiche des Bilds zuerst angezeigt, während die leichteren Bereiche gleich bleiben.

Wenn das `Lighten` Zielpixel für den-Modus der RGB-Farbwert (Dr, DG, DB) und das Quell Pixel die Farbe (SR, SG, SB) ist, wird die Ausgabe wie folgt berechnet (oder, og, ob):

 `Or = max(Dr, Sr)` `Og = max(Dg, Sg)`
 `Ob = max(Db, Sb)`

Für Rot, grün und blau separat ist das Ergebnis das größere Ziel und die Quelle. Dies führt dazu, dass die dunklen Bereiche des Ziels zuerst beleuchtet werden.

Der `Darken` Modus ist ähnlich, mit dem Unterschied, dass das Ergebnis das kleinere Ziel und die Quelle ist:

 `Or = min(Dr, Sr)` `Og = min(Dg, Sg)`
 `Ob = min(Db, Sb)`

Die roten, grünen und blauen Komponenten werden einzeln behandelt, weshalb diese Blend-Modi als _Trenn Bare_ Blend-Modi bezeichnet werden. Aus diesem Grund können die Abkürzungen **DC** und **SC** für die Ziel-und Quell Farben verwendet werden, und es ist zu verstehen, dass die Berechnungen für jede der roten, grünen und blauen Komponenten separat gelten.

In der folgenden Tabelle werden alle trennbaren Blend-Modi mit kurzen Erläuterungen dazu angezeigt, was Sie tun. Die zweite Spalte zeigt die Quellfarbe, die keine Änderung erzeugt:

| Blend-Modus   | Keine Änderung | Vorgang |
| ------------ | --------- | --------- |
| `Plus`       | Schwarz     | Lighdutzende durch Hinzufügen von Farben: SC + DC |
| `Modulate`   | White     | Darkens durch Multiplikation von Farben: SC DC | 
| `Screen`     | Schwarz     | Ergänzt das Produkt von Ergänzungen: SC + DC &ndash; SC DC |
| `Overlay`    | Grau      | Inversen von`HardLight` |
| `Darken`     | White     | Minimum der Farben: min (SC, DC) |
| `Lighten`    | Schwarz     | Maximum von Farben: Max (SC, DC) |
| `ColorDodge` | Schwarz     | Ziel basierend auf Quelle aufhellt |
| `ColorBurn`  | White     | Darkens des Ziels basierend auf der Quelle | 
| `HardLight`  | Grau      | Vergleichbar mit der Auswirkung von hartem Spotlight |
| `SoftLight`  | Grau      | Vergleichbar mit der Auswirkung von Soft Spotlight | 
| `Difference` | Schwarz     | Subtrahiert den dunkleren von dem helleren: ABS (DC &ndash; SC) | 
| `Exclusion`  | Schwarz     | Vergleichbar mit, `Difference` aber niedrigerer Kontrast |
| `Multiply`   | White     | Darkens durch Multiplikation von Farben: SC DC |

Ausführlichere Algorithmen finden Sie in der Spezifikation der W3C-Zusammensetzung [**und-Blending der Ebene 1**](https://www.w3.org/TR/compositing-1/) und der Skia- [**skblendmode-Referenz**](https://skia.org/user/api/SkBlendMode_Reference), obwohl die Notation in diesen beiden Quellen nicht identisch ist. Beachten Sie, dass in `Plus` der Regel als Porter-Duff-Blend-Modus angesehen wird und `Modulate` nicht Teil der W3C-Spezifikation ist.

Wenn die Quelle transparent ist, hat der Blend-Modus für alle trennbaren Blend-Modi `Modulate` keine Auswirkung. Wie Sie bereits gesehen haben, `Modulate` enthält der Blend-Modus den Alphakanal in der Multiplikation. Andernfalls `Modulate` hat denselben Effekt wie `Multiply` . 

Beachten Sie die beiden Modi `ColorDodge` und `ColorBurn` . Die Wörter " _Dodge_ " und " _Burn_ " entstanden in fotografischem Verfahren. Durch eine größere Zahl wird ein Foto mit einem negativen Licht durch ein negatives Licht gedruckt. Ohne Licht ist der Druck weiß. Der Druck wird dunkler, da mehr Licht für einen längeren Zeitraum auf den Druck fällt. Druckhersteller nutzten häufig ein Hand-oder ein kleines Objekt, um das Licht eines bestimmten Teils des Druckes zu blockieren, sodass der Bereich heller wird. Dies wird als " _Dodging_" bezeichnet. Umgekehrt könnte das nicht transparente Material mit einem Loch darin (oder Hände, das den größten Teil des Lichts blockiert) dazu verwendet werden, mehr Licht an einem bestimmten Punkt anzuweisen, es als _Brennen_zu bezeichnen.

Das Programm " **Dodge and Burn** " ähnelt stark den **Lighten und Darken**. Die XAML-Datei ist so strukturiert, aber mit unterschiedlichen Elementnamen, und die Code-Behind-Datei ist ebenfalls sehr ähnlich, aber die Auswirkung dieser beiden Blend-Modi unterscheidet sich erheblich:

[![Ausweichen und brennen](separable-images/DodgeAndBurn.png "Ausweichen und brennen")](separable-images/DodgeAndBurn-Large.png#lightbox)

Bei kleinen `Slider` Werten werden die `Lighten` dunklen Bereiche im Modus zuerst beleuchtet, während Sie gleich `ColorDodge` mäßiger heller werden.

Bei der Verarbeitung von Anwendungsprogrammen kann das Dodging und brennen häufig auf bestimmte Bereiche beschränkt werden, ebenso wie in einem Dunkelraum. Dies kann durch Farbverläufe oder durch eine Bitmap mit unterschiedlichen Grautönen erreicht werden.

## <a name="exploring-the-separable-blend-modes"></a>Untersuchen der trennbaren Blend-Modi

Auf der Seite **Trenn Bare Blend-Modi** können Sie alle trennbaren Blend-Modi überprüfen. Mit einem der Blend-Modi werden ein bitmapziel und eine farbige Rechteck Quelle angezeigt. 

Die XAML-Datei definiert eine `Picker` (um den Blend-Modus auszuwählen) und vier Schieberegler. Mit den ersten drei Schiebereglern können Sie die roten, grünen und blauen Komponenten der Quelle festlegen. Der vierte Schieberegler soll diese Werte überschreiben, indem ein grauer Farbton festgelegt wird. Die einzelnen Schieberegler werden nicht identifiziert, Farben geben jedoch ihre Funktion an:

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

Die Code-Behind-Datei lädt eine der Bitmapressourcen und zeichnet Sie zweimal, einmal in der oberen Hälfte der Canvas und wieder in der unteren Hälfte der Canvas:

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

Im unteren Bereich des `PaintSurface` Handlers wird ein Rechteck über der zweiten Bitmap mit dem ausgewählten Blend-Modus und der ausgewählten Farbe gezeichnet. Sie können die geänderte Bitmap unten mit der ursprünglichen Bitmap im oberen Bereich vergleichen:

[![Trennbare Füllmethoden](separable-images/SeparableBlendModes.png "Trennbare Füllmethoden")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>Additive und subtraktive Primärfarben

Die Seite " **primäre Farben** " zeichnet drei überlappende Kreise von rot, grün und blau:

[![Additive Primärfarben](separable-images/PrimaryColors-Additive.png "Additive Primärfarben")](separable-images/PrimaryColors-Additive.png#lightbox)

Dies sind die Additiven Primärfarben. Kombinationen aus zwei beiden erzeugt Cyan, Magenta und gelb, und eine Kombination aller drei ist weiß.

Diese drei Kreise werden mit dem- `SKBlendMode.Plus` Modus gezeichnet, Sie können jedoch auch `Screen` , `Lighten` oder `Difference` für denselben Effekt verwenden. Hier ist das Programm:

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

Das Programm enthält eine `TabGestureRecognizer` . Wenn Sie auf den Bildschirm tippen oder klicken, verwendet das Programm, `SKBlendMode.Multiply` um die drei subtraktiven primär Punkte anzuzeigen:

[![Subtraktive Primärfarben](separable-images/PrimaryColors-Subtractive.png "Subtraktive Primärfarben")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

Der- `Darken` Modus funktioniert auch für denselben Effekt.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
