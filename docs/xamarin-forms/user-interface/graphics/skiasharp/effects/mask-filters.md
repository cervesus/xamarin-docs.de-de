---
title: SkiaSharp-Maske-Filter
description: Erfahren Sie, wie Sie mithilfe des Filters Maske um Weichzeichner und andere alpha Effekte zu erstellen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 940422A1-8BC0-4039-8AD7-26C61320F858
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
ms.openlocfilehash: 36a8b5c32261d4f508c82feea1e6127574db6a20
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198244"
---
# <a name="skiasharp-mask-filters"></a>SkiaSharp-Maske-Filter

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Maske Filter sind die Auswirkungen, die die Geometrie und die alpha-Kanal von grafischen Objekten ändern. Legen Sie zum Verwenden eines Filters für die Maske der [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) Sie erstellt haben, durch Aufrufen einer der der `SKMaskFilter` statische Methoden.

Die beste Möglichkeit, mit der Maske Filter vertraut ist durch Experimentieren mit diesen statischen Methoden. Die nützlichste Maske Filter wird einen Weichzeichnungseffekt erstellt:

![Blur-Beispiel](mask-filters-images/MaskFilterExample.png "Blur-Beispiel")

Das ist der einzige Maske-Filterfunktion, die in diesem Artikel beschriebenen. Im nächsten Artikel auf [ **SkiaSharp Bildfilter** ](image-filters.md) beschreibt auch einen Weichzeichnereffekt, die Sie in dieses Objekt lieber. 

Die statische [ `SKMaskFilter.CreateBlur` ](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single)) Methode hat die folgende Syntax:

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

-Überladungen ermöglichen das Angeben von Flags für den Algorithmus zum Erstellen des Weichzeichnens und ein Rechteck, vermeiden Sie eine undeutliche in Bereichen, die mit anderen grafischen Objekten behandelt werden.

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle) ist eine Enumeration mit folgenden Membern:

- `Normal`
- `Solid`
- `Outer`
- `Inner`

Die Auswirkungen dieser Stile sind in den folgenden Beispielen gezeigt. Die `sigma` Parameter gibt den Umfang der des Weichzeichnens. In früheren Versionen von Skia können wurde der Wertebereich des Weichzeichnens mit einem Radiuswert angegeben. Wenn ein Radiuswert für Ihre Anwendung vorzuziehen ist, ist eine statische [ `SKMaskFilter.ConvertRadiusToSigma` ](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*) -Methode, die von einem zum anderen konvertieren kann. Die Methode 0.57735 Radius multipliziert und 0,5 hinzugefügt.

Die **Maske Blur experimentieren** auf der Seite die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel ermöglicht Ihnen das Experimentieren mit dem Blur-Stile und Sigma-Werte. Die XAML-Datei instanziiert ein `Picker` mit den vier `SKBlurStyle` Enumerationsmember und `Slider` zum Angeben des Werts Sigma:

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

Code-Behind-Datei verwendet diese Werte zum Erstellen einer `SKMaskFilter` Objekt aus, und legen Sie sie auf die `MaskFilter` Eigenschaft eine `SKPaint` Objekt. Dies `SKPaint` Objekt wird verwendet, um sowohl eine Textzeichenfolge und einer Bitmap gezeichnet werden soll:

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

Hier ist das Programm ausgeführt wird, unter iOS, Android und die universelle Windows-Plattform (UWP) mit der `Normal` blur-Stil und einem erhöhen `sigma` Ebenen:

[![Maskieren Blur-Experiments: Normal](mask-filters-images/MaskBlurExperiment-Normal.png "Maske Blur-Experiment – Normal")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

Sie werden feststellen, dass nur die Ränder der Bitmap des Weichzeichnens betroffen sind. Die `SKMaskFilter` Klasse ist nicht die richtigen Auswirkungen verwenden, wenn Sie eine gesamte Bitmap-Bild blur möchten. Dafür sollten Sie verwenden die [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) -Klasse wie im nächsten Artikel beschrieben [ **SkiaSharp Bildfilter**](image-filters.md).

Der Text wird mit ansteigenden Werten von mehr verwischt die `sigma` Argument. Experimentieren mit dem Programm, sehen Sie im, die für einen bestimmten `sigma` Wert, der die Unschärfe ist extremeren auf dem Windows 10-Desktop. Dieser Unterschied tritt auf, da die Pixeldichte weiter unten auf einen Monitor als auf mobilen Geräten und die Texthöhe in Pixel somit niedriger ist. Die `sigma` Wert ist proportional zur einen Blur-Block in Pixel für einen bestimmten `sigma` Wert, der Effekt ist extremeren in niedriger Auflösung angezeigt. In einer produktionsanwendung, Sie sollten zum Berechnen einer `sigma` Wert proportional zur Größe der Grafik. 

Versuchen Sie mehrere Werte, ehe Sie eine Blur-Ebene, die die am besten für Ihre Anwendung aussieht. Z. B. in der **Maske Blur-Experiment** Seite, und versuchen Sie die Einstellung `sigma` wie folgt aus:

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

Jetzt die `Slider` wirkt sich nicht, aber das Maß an Blur wird von den Plattformen:

[![Maskieren Blur-Experiments: konsistente](mask-filters-images/MaskBlurExperiment-Consistent.png "maskieren Blur-Experiments: konsistent")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

Alle in den Screenshots bis jetzt wurde gezeigt, Weichzeichnungseffekt erstellt, mit der `SKBlurStyle.Normal` -Enumerationsmember. Die folgenden Screenshots zeigen die Auswirkungen der `Solid`, `Outer`, und `Inner` blur-Formate:

[![Maskieren Blur-Experiment](mask-filters-images/MaskBlurExperiment.png "maskieren Blur-Experiment")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

Der IOS-Screenshot zeigt `Solid` den Stil: Die Textzeichen sind immer noch als solide schwarze Striche vorhanden, und der weich Strich wird der außerhalb dieser Textzeichen hinzugefügt. 

Der Android-Bildschirm Abbildung in der Mitte `Outer` zeigt den Stil: Die Zeichen Striche selbst werden entfernt (wie die Bitmap), und der weich Zeichenbereich umgibt den leeren Bereich, in dem die Textzeichen angezeigt werden. 

Die UWP-Screenshot, auf die richtigen zeigt die `Inner` Stil. Die Unschärfe ist auf des Bereichs, der normalerweise durch die Textzeichen beschränkt.

Die [ **SkiaSharp linearer Farbverlauf** ](shaders/linear-gradient.md#transparency-and-gradients) Artikel beschrieben eine **Reflektion Farbverlauf** Programm, das einen linearen Farbverlauf und eine Transformation verwendet werden, um eine Spiegelung einer Textzeichenfolge zu imitieren:

[![Reflektion Farbverlauf](shaders/linear-gradient-images/ReflectionGradient.png "Reflektion Farbverlauf")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Die **unscharf Reflektion** Seite fügt einer einzelnen Anweisung für diesen Code hinzu:

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

Die neue Anweisung fügt einem unschärfefilter für den reflektierten Text, der für die Textgröße basiert:

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

Diese unschärfefilter bewirkt, dass die Reflektion, um die viel realistischer erscheinen:

[![Unscharf Reflektion](mask-filters-images/BlurryReflection.png "unscharf Reflektion")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
