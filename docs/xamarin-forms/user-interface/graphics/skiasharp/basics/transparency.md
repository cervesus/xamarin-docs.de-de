---
title: SkiaSharp-Transparenz
description: Verwenden Sie Transparenz, um mehrere Objekte in einer einzigen Szene zu kombinieren.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B62F9487-C30E-4C63-BAB1-4C091FF50378
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 74335de66e74f6adc7c9488a1b78c31d36d03f14
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759404"
---
# <a name="skiasharp-transparency"></a>SkiaSharp-Transparenz

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Wie Sie gesehen haben, die [ `SKPaint` ](xref:SkiaSharp.SKPaint) Klasse enthält eine [ `Color` ](xref:SkiaSharp.SKPaint.Color) Eigenschaft vom Typ [ `SKColor` ](xref:SkiaSharp.SKColor). `SKColor` keinen alpha-Kanal also etwas, dass Sie mit Farbe enthält ein `SKColor` Wert kann teilweise transparent sein. 

Einige Transparenz wurde gezeigt, der [ **grundlegende Animation in SkiaSharp** ](animation.md) Artikel. In diesem Artikel werden mit der Transparenz zum Kombinieren von mehreren Objekten in einer einzigen Szene Technik manchmal etwas tiefer _blending_. Informationen zu erweiterten blending Techniken werden in den Artikeln erläutert die [ **SkiaSharp-Shader** ](../effects/shaders/index.md) Abschnitt.

Sie können die der Transparenzebene festlegen, wenn Sie eine Farbe, die mithilfe des vier-Parameters erstellen [ `SKColor` ](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte)) Konstruktor:

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

Der Alphawert 0 vollständig transparent ist und ein Alphawert von 0xFF vollständig deckend ist. Werte zwischen diesen beiden extremen erstellen, Farben, die teilweise transparent sind.

Darüber hinaus `SKColor` definiert einen produktionsnamespace [ `WithAlpha` ](xref:SkiaSharp.SKColor.WithAlpha*) -Methode, eine neue Farbe aus einer vorhandenen Farbe jedoch mit der angegebenen alpha Ebene erstellt:

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

Die Verwendung von teilweise transparente Text wird veranschaulicht, der **Code mehr Code** auf der Seite die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel. Auf dieser Seite eingeblendet zwei Textzeichenfolgen ein-und durch die Aufnahme der Transparenz in der `SKColor` Werte:

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

Die `transparency` Feld wird zum unterscheiden sich von 0, 1 und wieder zurück in einen sinusförmiger Rhythmus animiert. Die erste Textzeichenfolge wird angezeigt, mit einem Alphawert berechnet, indem die `transparency` Wert von 1:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

Die [ `WithAlpha` ](xref:SkiaSharp.SKColor.WithAlpha*) Methode legt die alpha-Komponente auf einer vorhandenen Farbe, handelt es sich hier `SKColors.Blue`. Die zweite Textzeichenfolge verwendet einen Alphawert von berechnet die `transparency` -Wert selbst:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

Die Animation wechselt zwischen den beiden Wörtern, drängen den Benutzer auf "mehr code" (oder vielleicht anfordern "mehr Code"):

[![Code mehr Code](transparency-images/CodeMoreCode.png "Code mehr Code")](transparency-images/CodeMoreCode-Large.png#lightbox)

Im vorherigen Artikel auf [ **Bitmap-Grundlagen in SkiaSharp**](bitmaps.md), Sie haben gesehen, Vorgehensweise beim Anzeigen von Bitmaps mit einer der der [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap*) Methoden `SKCanvas`. Alle der `DrawBitmap` Methoden umfassen eine `SKPaint` Objekt als letzter Parameter. Dieser Parameter ist standardmäßig auf festgelegt `null` und kann ignoriert werden. 

Sie können alternativ Festlegen der `Color` -Eigenschaft dieses `SKPaint` Objekt, um eine Bitmap mit gewissen Grad transparent anzuzeigen. Festlegen der Grad der Transparenz in der `Color` Eigenschaft `SKPaint` können Sie die Bitmaps ein-und ausgeblendet oder eine Bitmap in ein anderes auflösen. 

Die Transparenz einer Bitmap wird veranschaulicht, der **Bitmap auflösen** Seite. Die XAML-Datei instanziiert ein `SKCanvasView` und `Slider`:

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

Die Code-Behind-Datei werden zwei Bitmapressourcen geladen. Diese Bitmaps sind nicht die gleiche Größe, aber sie sind das gleiche Seitenverhältnis:

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

Die `Color` Eigenschaft der `SKPaint` Objekt auf zwei sich ergänzende alpha-Ebenen für die zwei Bitmaps festgelegt ist. Bei Verwendung `SKPaint` mit Bitmaps, es spielt keine Rolle, welche die restlichen den `Color` Wert ist. Wichtig ist der alpha-Kanal. Ruft der Code hier einfach die `WithAlpha` Methode hängt vom Standardwert von der `Color` Eigenschaft.

Verschieben der `Slider` zwischen einer Bitmap und die andere auflöst:

[![Auflösen von Bitmaps](transparency-images/BitmapDissolve.png "Bitmap auflösen")](transparency-images/BitmapDissolve-Large.png#lightbox)

In den letzten mehrere Artikel wurde gezeigt, wie SkiaSharp zum Zeichnen von Text, Kreisen, Ellipsen, abgerundete Rechtecke und Bitmaps verwenden. Im nächsten Schritt [SkiaSharp-Linien und-Pfade](../paths/index.md) in der erfahren Sie, wie zum Zeichnen von miteinander verbundenen Linien in einem Grafikpfad.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
