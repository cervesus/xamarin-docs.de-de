---
Title: "skiasharp-Bild Filter" Beschreibung: "erfahren Sie, wie Sie mit dem Bild Filter verwischt erstellen und Schatten ablegen."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 173e7b22-aec8-4f12-b657-1c0cee01ad63 Author: davidbritch ms. Author: dabritch ms. Date: 08/27/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-image-filters"></a>Skiasharp-Bild Filter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Bild Filter sind Effekte, die auf alle Farbbits von Pixeln angewendet werden, die ein Bild bilden. Sie sind vielseitiger als maskenfilter, die nur auf dem Alphakanal ausgeführt werden, wie im Artikel [**skiasharp Mask Filters**](mask-filters.md)beschrieben. Um einen Bild Filter zu verwenden, legen [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) Sie die-Eigenschaft von `SKPaint` auf ein Objekt des Typs fest, den [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) Sie durch Aufrufen einer der statischen Methoden der Klasse erstellt haben.

Die beste Möglichkeit, sich mit Masken Filtern vertraut zu machen, besteht darin, mit diesen statischen Methoden zu experimentieren. Sie können einen maskenfilter verwenden, um eine gesamte Bitmap zu verwischen:

![Beispiel für weich](image-filters-images/ImageFilterExample.png "Beispiel für weich")

In diesem Artikel wird auch veranschaulicht, wie Sie einen Bild Filter zum Erstellen eines Schlag Schattens und für die Prägung und Gravur verwenden.

## <a name="blurring-vector-graphics-and-bitmaps"></a>Verwischen von Vektorgrafiken und Bitmaps

Der von der statischen-Methode erstellte weichzeicheneffekt [`SKImageFilter.CreateBlur`](xref:SkiaSharp.SKImageFilter.CreateBlur*) hat gegenüber den weichgrafiemethoden in der-Klasse einen erheblichen Vorteil [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) : der Bild Filter kann eine ganze Bitmap verwischen. Die-Methode weist die folgende Syntax auf:

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

Die-Methode verfügt über zwei Sigma-Werte, &mdash; die erste für den weichzeichnerbereich in der horizontalen Richtung und der zweite für die vertikale Richtung. Sie können Bild Filter kaskadieren, indem Sie einen anderen Bild Filter als optionales drittes Argument angeben. Außerdem kann ein zuschneigsrechteck angegeben werden.

Die **Experiment Seite Bild** [**Maske in skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) enthält zwei `Slider` Ansichten, mit denen Sie experimentieren können, indem Sie verschiedene Ebenen der weich Zeichnung festlegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.ImageBlurExperimentPage"
             Title="Image Blur Experiment">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei verwendet die beiden `Slider` Werte zum Aufrufen `SKImageFilter.CreateBlur` des `SKPaint` -Objekts, das zum Anzeigen von Text und einer Bitmap verwendet wird:

```csharp
public partial class ImageBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage),
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public ImageBlurExperimentPage ()
    {
        InitializeComponent ();
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

        // Get values from sliders
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.ImageFilter = SKImageFilter.CreateBlur(sigmaX, sigmaY);

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

Die drei Screenshots zeigen verschiedene Einstellungen für die `sigmaX` -und- `sigmaY` Einstellungen:

[![Bild weich Experiment](image-filters-images/ImageBlurExperiment.png "Bild weich Experiment")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

Legen Sie `sigmaX` und auf Werte fest, die `sigmaY` proportional zur gerenderten Pixelgröße des Bilds sind, auf das der Weichzeichner angewendet wird, um die weich Schrift bei verschiedenen Anzeige Größen und Auflösungen konsistent zu halten.

## <a name="drop-shadow"></a>Schlagschatten

Die [`SKImageFilter.CreateDropShadow`](xref:SkiaSharp.SKImageFilter.CreateDropShadow*) statische-Methode erstellt ein- `SKImageFilter` Objekt für einen Ablage Schatten:

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

Legen Sie dieses Objekt auf die `ImageFilter` -Eigenschaft eines `SKPaint` Objekts fest, und alle Elemente, die Sie mit diesem Objekt zeichnen, haben einen Schlag Schatten dahinter.

Der `dx` -Parameter und der- `dy` Parameter geben die horizontale und vertikale Offsets des Schattens in Pixel aus dem grafischen Objekt an. Die Konvention in 2D-Grafiken besteht darin, eine helle Quelle von oben links zu übernehmen. Dies impliziert, dass beide Argumente positiv sein sollten, um den Schatten unterhalb und rechts vom grafischen Objekt zu positionieren.

Der `sigmaX` -Parameter und der- `sigmaY` Parameter sind verschwommen-Faktoren für den Schlag Schatten.

Der- `color` Parameter ist die Farbe des Schlag Schattens. Dieser `SKColor` Wert kann Transparenz einschließen. Eine Möglichkeit ist der Farbwert `SKColors.Black.WithAlpha(0x80)` , mit dem ein beliebiger Hintergrund angezeigt wird.

Die beiden letzten Parameter sind optional.

Mit dem **Drop Shadow Experiment** -Programm können Sie mit Werten von `dx` ,, und experimentieren, `dy` `sigmaX` `sigmaY` um eine Text Zeichenfolge mit einem Schlag Schatten anzuzeigen. Die XAML-Datei instanziiert vier `Slider` Sichten, um diese Werte festzulegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.DropShadowExperimentPage"
             Title="Drop Shadow Experiment">
    <ContentPage.Resources>
        <Style TargetType="Slider">
            <Setter Property="Margin" Value="10, 0" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="dxSlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dxSlider},
                              Path=Value,
                              StringFormat='Horizontal offset = {0:F1}'}" />

        <Slider x:Name="dySlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dySlider},
                              Path=Value,
                              StringFormat='Vertical offset = {0:F1}'}" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei verwendet diese Werte zum Erstellen eines roten Schlag Schattens in einer blauen Text Zeichenfolge:

```csharp
public partial class DropShadowExperimentPage : ContentPage
{
    const string TEXT = "Drop Shadow";

    public DropShadowExperimentPage ()
    {
        InitializeComponent ();
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

        // Get values from sliders
        float dx = (float)dxSlider.Value;
        float dy = (float)dySlider.Value;
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = info.Width / 7;
            paint.Color = SKColors.Blue;
            paint.ImageFilter = SKImageFilter.CreateDropShadow(
                                    dx,
                                    dy,
                                    sigmaX,
                                    sigmaY,
                                    SKColors.Red,
                                    SKDropShadowImageFilterShadowMode.DrawShadowAndForeground);

            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Center the text in the display rectangle
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Dies ist das Programm, das ausgeführt wird:

[![Schlag Schatten Experiment](image-filters-images/DropShadowExperiment.png "Schlag Schatten Experiment")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

Die negativen Offset Werte im universelle Windows-Plattform Bildschirm Abbildung ganz rechts bewirken, dass der Schatten oberhalb und links neben dem Text angezeigt wird. Dadurch wird eine Lichtquelle in der unteren rechten Ecke vorgeschlagen, bei der es sich nicht um die Konvention für Computergrafiken handelt. Es scheint aber nicht falsch zu sein, vielleicht, weil der Schatten ebenfalls sehr unscharf ist und mehr ornamental als die meisten Schlag Schatten erscheint.

## <a name="lighting-effects"></a>Beleuchtungseffekte

Die- `SKImageFilter` Klasse definiert sechs Methoden mit ähnlichen Namen und Parametern, die in der Reihenfolge der zunehmenden Komplexität aufgeführt werden:

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

Mit diesen Methoden werden Bild Filter erstellt, die die Auswirkung verschiedener Arten von Licht auf dreidimensionalen Oberflächen imitieren. Der resultierende Bild Filter beleuchtet zweidimensionale Objekte so, als wären Sie im 3D-Raum vorhanden, was dazu führen kann, dass diese Objekte mit erhöhten Rechten oder mit einer Glanz Markierung angezeigt werden.

Die `Distant` Light-Methoden gehen davon aus, dass das Licht von weitem entfernt wird. Zum Zweck der Beleuchtung von Objekten wird davon ausgegangen, dass das Licht in einer konsistenten Richtung in 3D-Raum steht, ähnlich wie die Sonne in einem kleinen Bereich der Erde. Die `Point` Light-Methoden imitieren eine Glühbirne, die sich in einem 3D-Raum befindet und Licht in alle Richtungen ausgibt. Das `Spot` Licht hat eine Position und eine Richtung, ähnlich wie eine Taschenlampe.

Standorte und Richtungen im 3D-Raum werden mit Werten der- [`SKPoint3`](xref:SkiaSharp.SKPoint3) Struktur angegeben, die mit vergleichbar ist, `SKPoint` aber mit drei Eigenschaften namens `X` , `Y` und `Z` .

Die Anzahl und Komplexität der Parameter für diese Methoden erschwert das Experimentieren. Um Ihnen den Einstieg zu erleichtern, können Sie mit der Seite " **entferntes Licht Experiment** " mit Parametern für die- `CreateDistantLightDiffuse` Methode experimentieren:

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

Die Seite verwendet nicht die letzten beiden optionalen Parameter.

`Slider`Mit drei Ansichten in der XAML-Datei können Sie die `Z` Koordinaten des `SKPoint3` Werts, den `surfaceScale` -Parameter und den-Parameter auswählen, der `kd` in der API-Dokumentation als "diffuse Beleuchtungs Konstante" definiert ist:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaLightExperiment.MainPage"
             Title="Distant Light Experiment">

    <StackLayout>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           VerticalOptions="FillAndExpand" />

        <Slider x:Name="zSlider"
                Minimum="-10"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zSlider},
                              Path=Value,
                              StringFormat='Z = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="surfaceScaleSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference surfaceScaleSlider},
                              Path=Value,
                              StringFormat='Surface Scale = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="lightConstantSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference lightConstantSlider},
                              Path=Value,
                              StringFormat='Light Constant = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei erhält diese drei Werte und verwendet Sie, um einen Bild Filter zum Anzeigen einer Text Zeichenfolge zu erstellen:

```csharp
public partial class DistantLightExperimentPage : ContentPage
{
    const string TEXT = "Lighting";

    public DistantLightExperimentPage()
    {
        InitializeComponent();
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

        float z = (float)zSlider.Value;
        float surfaceScale = (float)surfaceScaleSlider.Value;
        float lightConstant = (float)lightConstantSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.IsAntialias = true;

            // Size text to 90% of canvas width
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Find coordinates to center text
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            float xText = info.Rect.MidX - textBounds.MidX;
            float yText = info.Rect.MidY - textBounds.MidY;

            // Create distant light image filter
            paint.ImageFilter = SKImageFilter.CreateDistantLitDiffuse(
                                    new SKPoint3(2, 3, z),
                                    SKColors.White,
                                    surfaceScale,
                                    lightConstant);

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Das erste Argument von `SKImageFilter.CreateDistantLitDiffuse` ist die Richtung des Lichts. Die positiven X-und Y-Koordinaten zeigen an, dass das Licht nach rechts und unten gezeigt wird. Positive Z-Koordinaten zeigen auf den Bildschirm. Mit der XAML-Datei können Sie negative z-Werte auswählen, aber das ist nur so, dass Sie sehen, was geschieht: konzeptionell werden negative z-Koordinaten bewirken, dass das Licht auf den Bildschirm verweist. Bei allen anderen dann kleinen negativen Werten wird der Beleuchtungs Effekt nicht mehr funktionieren.

Das `surfaceScale` Argument kann zwischen – 1 und 1 liegen. (Höhere oder niedrigere Werte haben keine weiteren Auswirkungen.) Dabei handelt es sich um relative Werte auf der Z-Achse, die die Verschiebung des grafischen Objekts (in diesem Fall die Text Zeichenfolge) von der Canvas-Oberfläche angeben. Verwenden Sie negative Werte, um die Text Zeichenfolge oberhalb der Oberfläche der Canvas aufzurichten, und positive Werte, um Sie in den Zeichenbereich zu drücken.

Der `lightConstant` Wert muss positiv sein. (Das Programm lässt negative Werte zu, sodass Sie erkennen können, dass die Wirkung nicht mehr funktioniert.) Höhere Werte führen zu intensiveren Licht.

Diese Faktoren können ausgeglichen werden, um einen geprägten Effekt zu erhalten `surfaceScale` , wenn negativ ist (wie bei den IOS-und Android-Screenshots) und ein eingravierende Effekt `surfaceScale` , wenn positiv ist, wie bei dem Bildschirm Abbildung von UWP auf der rechten Seite:

[![Entferntes helles Experiment](image-filters-images/DistantLightExperiment.png "Entferntes helles Experiment")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Der Android-Bildschirmfoto weist einen Z-Wert von 0 auf, was bedeutet, dass das Licht nur nach unten und nach rechts zeigt. Der Hintergrund wird nicht beleuchtet, und die Oberfläche der Text Zeichenfolge wird ebenfalls nicht beleuchtet. Das Licht wirkt sich nur auf den Rand des Texts aus, um eine sehr feine Wirkung zu erzielen.

Eine alternative Methode zum darstellen und Einreihen von Text wurde im Artikel [Translation Transform](../transforms/translate.md): die Text Zeichenfolge wird zweimal mit unterschiedlichen Farben angezeigt, die leicht voneinander abweichen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
