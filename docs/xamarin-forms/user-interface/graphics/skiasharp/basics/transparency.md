---
Title: "skiasharp-Transparenz" Beschreibung: "verwenden Sie Transparenz, um mehrere Objekte in einer einzigen Szene zu kombinieren".
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: B62F9487-C30E-4C63-BAB1-4C091FF50378 Author: davidbritch ms. Author: dabritch ms. Date: 08/23/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-transparency"></a>Skiasharp-Transparenz

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Wie Sie gesehen haben, enthält die- [`SKPaint`](xref:SkiaSharp.SKPaint) Klasse eine- [`Color`](xref:SkiaSharp.SKPaint.Color) Eigenschaft des Typs [`SKColor`](xref:SkiaSharp.SKColor) . `SKColor`schließt einen Alphakanal ein, sodass alles, was Sie mit einem-Wert aufweisen, `SKColor` teilweise transparent sein kann. 

Im Artikel " [**grundlegende Animation" in skiasharp**](animation.md) wurde eine gewisse Transparenz veranschaulicht. Dieser Artikel geht ausführlicher auf die Transparenz ein, um mehrere Objekte in einer einzigen Szene zu kombinieren, eine Technik, die manchmal auch als _Blending_bezeichnet wird. Erweiterte Mischungs Techniken werden in den Artikeln im Abschnitt [**skiasharp-Shader**](../effects/shaders/index.md) behandelt.

Sie können die Transparenzstufe festlegen, wenn Sie eine Farbe mit dem vier [`SKColor`](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte)) parameterkonstruktor erstellen:

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

Der Alpha-Wert 0 ist vollständig transparent, und ein Alpha-Wert von 0xFF ist vollständig undurchsichtig. Werte zwischen diesen beiden extremen erstellen Farben, die teilweise transparent sind.

Außerdem `SKColor` definiert eine praktische [`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*) Methode, die eine neue Farbe aus einer vorhandenen Farbe, aber mit der angegebenen Alpha Ebene, erstellt:

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

Die Verwendung von teilweise transparentem Text wird in der **Codepage Code more** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel veranschaulicht. Diese Seite blendet zwei Text Zeichenfolgen ein und aus, indem Sie Transparenz in die `SKColor` Werte einschließen:

```csharp
public class CodeMoreCodePage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    double transparency;

    public CodeMoreCodePage ()
    {
        Title = "Code More Code";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 5;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        transparency = 0.5 * (1 + Math.Sin(progress * 2 * Math.PI));
        canvasView.InvalidateSurface();

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        const string TEXT1 = "CODE";
        const string TEXT2 = "MORE";

        using (SKPaint paint = new SKPaint())
        {
            // Set text width to fit in width of canvas
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT1);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Center first text string
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT1, ref textBounds);

            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
            canvas.DrawText(TEXT1, xText, yText, paint);

            // Center second text string
            textBounds = new SKRect();
            paint.MeasureText(TEXT2, ref textBounds);

            xText = info.Width / 2 - textBounds.MidX;
            yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
            canvas.DrawText(TEXT2, xText, yText, paint);
        }
    }
}
```

Das `transparency` Feld ist so animiert, dass es zwischen 0 und 1 und wieder in einem sinusoialen Rhythmus variiert. Die erste Text Zeichenfolge wird mit einem Alpha Wert angezeigt, der durch Subtraktion des `transparency` Werts von 1 berechnet wird:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

Die- [`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*) Methode legt die Alpha Komponente auf eine vorhandene Farbe fest, die hier ist `SKColors.Blue` . Die zweite Text Zeichenfolge verwendet einen Alpha-Wert, der aus dem `transparency` Wert selbst berechnet wurde:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

Die Animation wechselt zwischen den beiden Wörtern und fordert den Benutzer auf, mehr Code zu programmieren (oder vielleicht "weiteren Code" anzufordern):

[![Code mehr Code](transparency-images/CodeMoreCode.png "Code mehr Code")](transparency-images/CodeMoreCode-Large.png#lightbox)

Im vorherigen Artikel zu [**Bitmap-Grundlagen in skiasharp**](bitmaps.md)haben Sie erfahren, wie Bitmaps mit einer der- [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap*) Methoden von angezeigt werden `SKCanvas` . Alle- `DrawBitmap` Methoden enthalten ein- `SKPaint` Objekt als letzten Parameter. Standardmäßig ist dieser Parameter auf festgelegt, `null` und Sie können ihn ignorieren. 

Alternativ können Sie die- `Color` Eigenschaft dieses- `SKPaint` Objekts festlegen, um eine Bitmap mit gewisser Transparenz anzuzeigen. Durch Festlegen einer Transparenz Ebene in der `Color` -Eigenschaft von `SKPaint` können Sie Bitmaps ein-und ausblenden oder eine Bitmap in eine andere auflösen. 

Die bitmaptransparenz wird auf der Seite zum **Auflösen der Bitmap** veranschaulicht. Die XAML-Datei instanziiert ein `SKCanvasView` und einen `Slider` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapDissolvePage"
             Title="Bitmap Dissolve">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei lädt zwei Bitmap-Ressourcen. Diese Bitmaps haben nicht die gleiche Größe, Sie sind jedoch identisch mit dem Seitenverhältnis:

```csharp
public partial class BitmapDissolvePage : ContentPage
{
    SKBitmap bitmap1;
    SKBitmap bitmap2;

    public BitmapDissolvePage()
    {
        InitializeComponent();

        // Load two bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg"))
        {
            bitmap1 = SKBitmap.Decode(stream);
        }
        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.FacePalm.jpg"))
        {
            bitmap2 = SKBitmap.Decode(stream);
        }
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

        canvas.Clear();

        // Find rectangle to fit bitmap
        float scale = Math.Min((float)info.Width / bitmap1.Width,
                                (float)info.Height / bitmap1.Height);
        SKRect rect = SKRect.Create(scale * bitmap1.Width,
                                    scale * bitmap1.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Get progress value from Slider
        float progress = (float)progressSlider.Value;

        // Display two bitmaps with transparency
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = paint.Color.WithAlpha((byte)(0xFF * (1 - progress)));
            canvas.DrawBitmap(bitmap1, rect, paint);

            paint.Color = paint.Color.WithAlpha((byte)(0xFF * progress));
            canvas.DrawBitmap(bitmap2, rect, paint);
        }
    }
}
```

Die- `Color` Eigenschaft des- `SKPaint` Objekts wird auf zwei ergänzende Alpha Stufen für die beiden Bitmaps festgelegt. Wenn `SKPaint` Sie mit Bitmaps verwenden, ist es unerheblich, was der restliche `Color` Wert ist. Alles, was wichtig ist, ist der Alphakanal. Der Code hier ruft einfach die- `WithAlpha` Methode für den Standardwert der- `Color` Eigenschaft auf.

Verschieben der `Slider` Auflösungen zwischen einer Bitmap und der anderen:

[![Bitmap-Auflösung](transparency-images/BitmapDissolve.png "Bitmap-Auflösung")](transparency-images/BitmapDissolve-Large.png#lightbox)

In den letzten Artikeln haben Sie erfahren, wie Sie mit skiasharp Text, Kreise, Ellipsen, abgerundete Rechtecke und Bitmaps zeichnen. Der nächste Schritt sind [skiasharp-Linien und-Pfade](../paths/index.md) , in denen Sie erfahren, wie Sie verbundene Linien in einem Grafik Pfad zeichnen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
