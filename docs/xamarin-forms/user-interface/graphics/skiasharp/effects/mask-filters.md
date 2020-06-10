---
Title: "skiasharp Mask Filters" Description: "erfahren Sie, wie Sie den maskenfilter zum Erstellen von verwischt und anderen Alpha Effekten verwenden."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 940422a1-8bc0-4039-8ad7-26c61320f858 Author: davidbritch ms. Author: dabritch ms. Date: 08/27/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-mask-filters"></a>Skiasharp-maskenfilter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Maskenfilter sind Effekte, die die Geometrie und den Alphakanal grafischer Objekte verändern. Wenn Sie einen maskenfilter verwenden möchten, legen Sie die- [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) Eigenschaft von `SKPaint` auf ein Objekt des Typs fest, den [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) Sie durch Aufrufen einer der statischen-Methoden erstellt haben `SKMaskFilter` .

Die beste Möglichkeit, sich mit Masken Filtern vertraut zu machen, besteht darin, mit diesen statischen Methoden zu experimentieren. Der nützlichste maskenfilter erstellt einen Weichzeichner:

![Beispiel für weich](mask-filters-images/MaskFilterExample.png "Beispiel für weich")

Das ist das einzige maskenfilter Feature, das in diesem Artikel beschrieben wird. Im nächsten Artikel zu [**skiasharp-Bild Filtern**](image-filters.md) wird auch ein Weichzeichnereffekt beschrieben, den Sie möglicherweise bevorzugen. 

Die statische- [`SKMaskFilter.CreateBlur`](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single)) Methode weist die folgende Syntax auf:

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

Über Ladungen ermöglichen das Angeben von Flags für den Algorithmus, der zum Erstellen des weichmucks verwendet wird, und ein Rechteck, um die verwischt in Bereichen zu vermeiden, die mit anderen grafischen Objekten abgedeckt werden.

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle)ist eine Enumeration mit den folgenden Membern:

- `Normal`
- `Solid`
- `Outer`
- `Inner`

Die Auswirkungen dieser Stile sind in den folgenden Beispielen dargestellt. Der- `sigma` Parameter gibt den Umfang des weich Zeichnens an. In älteren Versionen der Skia wurde der Umfang des weichwerts durch einen RADIUS-Wert angegeben. Wenn ein RADIUS-Wert für Ihre Anwendung vorzuziehen ist, gibt es eine statische [`SKMaskFilter.ConvertRadiusToSigma`](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*) Methode, die eine Konvertierung von einer in die andere durchsetzen kann. Mit der-Methode wird der Radius um 0,57735 multipliziert und 0,5 hinzugefügt.

Die Seite **mask mask Experiment** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel ermöglicht Ihnen das Experimentieren mit den weichzeichenstilen und Sigma-Werten. Die XAML-Datei instanziiert ein `Picker` -Element mit den vier Enumerationsmembern `SKBlurStyle` und einem `Slider` zum Angeben des Sigma-Werts:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.MaskBlurExperimentPage"
             Title="Mask Blur Experiment">
    
    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blurStylePicker" 
                Title="Filter Blur Style" 
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlurStyle}">
                    <x:Static Member="skia:SKBlurStyle.Normal" />
                    <x:Static Member="skia:SKBlurStyle.Solid" />
                    <x:Static Member="skia:SKBlurStyle.Outer" />
                    <x:Static Member="skia:SKBlurStyle.Inner" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="sigmaSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaSlider},
                              Path=Value,
                              StringFormat='Sigma = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei verwendet diese Werte, um ein `SKMaskFilter` -Objekt zu erstellen und es auf die- `MaskFilter` Eigenschaft eines-Objekts festzulegen `SKPaint` . Dieses `SKPaint` Objekt wird verwendet, um sowohl eine Text Zeichenfolge als auch eine Bitmap zu zeichnen:

```csharp
public partial class MaskBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage), 
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public MaskBlurExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Pink);

        // Get values from XAML controls
        SKBlurStyle blurStyle =
            (SKBlurStyle)(blurStylePicker.SelectedIndex == -1 ?
                                        0 : blurStylePicker.SelectedItem);

        float sigma = (float)sigmaSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);

            // Get text bounds and calculate display rectangle
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);
            SKRect textRect = new SKRect(0, 0, info.Width, textBounds.Height + 50);

            // Center the text in the display rectangle
            float xText = textRect.Width / 2 - textBounds.MidX;
            float yText = textRect.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);

            // Calculate rectangle for bitmap
            SKRect bitmapRect = new SKRect(0, textRect.Bottom, info.Width, info.Height);
            bitmapRect.Inflate(-50, -50);

            canvas.DrawBitmap(bitmap, bitmapRect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Hier ist das Programm, das unter IOS, Android und der universelle Windows-Plattform (UWP) mit dem `Normal` weichzeichnerstil und höheren Stufen ausgeführt wird `sigma` :

[![Mask mask-Experiment (normal)](mask-filters-images/MaskBlurExperiment-Normal.png "Mask mask-Experiment (normal)")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

Sie werden bemerken, dass der Weichzeichner nur die Ränder der Bitmap beeinflusst. Die- `SKMaskFilter` Klasse ist nicht der richtige Effekt, wenn Sie ein gesamtes Bitmapbild verwischen möchten. Dafür sollten Sie die-Klasse verwenden, [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) wie im nächsten Artikel über [**skiasharp-Bild Filter**](image-filters.md)beschrieben.

Der Text wird mit den zunehmenden Werten des Arguments nicht mehr verwischt `sigma` . Wenn Sie mit diesem Programm experimentieren, werden Sie feststellen, dass der Weichzeichner für einen bestimmten `sigma` Wert auf dem Windows 10-Desktop eher extrem ist. Dieser Unterschied tritt auf, weil die Pixeldichte auf einem Desktop Monitor niedriger ist als auf mobilen Geräten, sodass die Texthöhe in Pixel geringer ist. Der `sigma` Wert ist proportional zu einem Weichzeichnerwert in Pixel, sodass für einen gegebenen `sigma` Wert der Effekt bei niedrigerer Auflösung genauer angezeigt wird. In einer Produktionsanwendung möchten Sie wahrscheinlich einen Wert berechnen, der `sigma` proportional zur Grafik Größe ist. 

Probieren Sie mehrere Werte aus, bevor Sie sich auf einer weichungs Ebene befinden, die für Ihre Anwendung am besten geeignet ist Versuchen Sie beispielsweise, auf der Seite **Maske Mask-Experiment** die Einstellung `sigma` wie folgt:

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

Der `Slider` hat jetzt keine Auswirkung, aber der Grad der weich Zeichnung ist zwischen den Plattformen konsistent:

[![Masken Maske-Experiment-konsistent](mask-filters-images/MaskBlurExperiment-Consistent.png "Masken Maske-Experiment-konsistent")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

Alle bisher gezeigten Screenshots haben mit dem-Enumerationsmember eine weichzeichenfolge gezeigt `SKBlurStyle.Normal` . Die folgenden Screenshots zeigen die Auswirkungen der `Solid` Stile, `Outer` und weich `Inner` :

[![Mask mask-Experiment](mask-filters-images/MaskBlurExperiment.png "Mask mask-Experiment")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

Der IOS-Screenshot zeigt den `Solid` Stil: die Textzeichen sind immer noch als Solid Black-Striche vorhanden, und der weich Zeichenbereich wird außerhalb dieser Textzeichen hinzugefügt. 

Der Android-Screenshot in der Mitte zeigt den `Outer` Stil: die Zeichen Striche selbst werden entfernt (wie die Bitmap), und der weich Zeichenbereich umgibt den leeren Bereich, in dem die Textzeichen angezeigt werden. 

Der UWP-Bildschirm auf der rechten Seite zeigt den `Inner` Stil. Der Weichzeichner ist auf den Bereich beschränkt, der normalerweise von den Textzeichen besetzt ist.

Der [**lineare Überlaufs Artikel skiasharp**](shaders/linear-gradient.md#transparency-and-gradients) beschreibt ein **reflektionsgradientenprogramm** , das einen linearen Farbverlauf und eine Transformation verwendet, um eine Reflektion einer Text Zeichenfolge zu imitieren:

[![Reflektionstyp](shaders/linear-gradient-images/ReflectionGradient.png "Reflektionstyp")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Die Seite " **Blurry Reflection** " fügt dem Code eine einzige-Anweisung hinzu:

```csharp
public class BlurryReflectionPage : ContentPage
{
    const string TEXT = "Reflection";

    public BlurryReflectionPage()
    {
        Title = "Blurry Reflection";

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

        using (SKPaint paint = new SKPaint())
        {
            // Set text color to blue
            paint.Color = SKColors.Blue;

            // Set text size to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to position text above center
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2;

            // Draw unreflected text
            canvas.DrawText(TEXT, xText, yText, paint);

            // Shift textBounds to match displayed text
            textBounds.Offset(xText, yText);

            // Use those offsets to create a gradient for the reflected text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, textBounds.Top),
                                new SKPoint(0, textBounds.Bottom),
                                new SKColor[] { paint.Color.WithAlpha(0),
                                                paint.Color.WithAlpha(0x80) },
                                null,
                                SKShaderTileMode.Clamp);

            // Create a blur mask filter
            paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Die neue Anweisung fügt einen weichzeichenfilter für den reflektierten Text hinzu, der auf der Textgröße basiert:

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

Dieser weichzeichnerfilter bewirkt, dass die Reflektion weitaus realistischer erscheint:

[![Blurry-Reflektion](mask-filters-images/BlurryReflection.png "Blurry-Reflektion")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
