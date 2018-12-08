---
title: SkiaSharp-Rauschen und zusammenstellen
description: Perlin-Noise-Shader generieren und mit anderen Shaders kombinieren.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 4801aa12acf8eca2384cc5b41d677f7cb0bdd90d
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53052050"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp-Rauschen und zusammenstellen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Einfache Vektorgrafiken tendenziell unnatürlichen suchen. Die geraden, geglättete Kurven und Volltonfarben entsprechen nicht den Fehlern von realen Objekten. Bei der Arbeit an den Computer generierte Grafiken für den Film 1982 _Tron_, Computerwissenschaftler Ken Perlin zu Beginn der Entwicklung von Algorithmen, die zufällige Prozesse verwendet wird, um diese Images realistischere Texturen geben. Im Jahre 1997 gewonnen Ken Perlin Academy Auszeichnung für den technischen Kompetenz. Verfügt über seine Arbeit Perlin-Noise genannt, stammen, und wird in die SkiaSharp unterstützt. Im Folgenden ein Beispiel:

![Beispiel des Perlin-Noise](noise-images/NoiseSample.png "Perlin-Noise-Beispiel")

Wie Sie sehen können, ist jedes Pixel keinen zufälligen Farbwert. Die Kontinuität von Pixeln mit zufälligen Formen Pixel.

Die Unterstützung des Perlin-Noise in Skia basiert auf einer W3C-Spezifikation für CSS- und SVG. Abschnitt 8.20 von [ **Filter Effekte Modul Level 1** ](http://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) die zugrunde liegenden Perlin-Noise-Algorithmen in C-Code enthält.

## <a name="exploring-perlin-noise"></a>Untersuchen des Perlin-noise

Die [ `SKShader` ](xref:SkiaSharp.SKShader) -Klasse definiert zwei verschiedene statische Methoden zum Generieren von Perlin-Noise: [ `CreatePerlinNoiseFractalNoise` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) und [ `CreatePerlinNoiseTurbulence` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*). Die Parameter sind identisch:

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

Beide Methoden auch vorhanden sein, in überladenen Versionen mit einem zusätzlichen `SKPointI` Parameter. Der Abschnitt [ **Tiling Perlin-Noise** ](#tiling-perlin-noise) erläutert diese Überladungen.

Die beiden `baseFrequency` Argumente werden positive Werte, die in der Dokumentation SkiaSharp als Zahl zwischen 0 und 1 definiert, aber sie können auch höhere Werte festgelegt werden. Je höher der Wert, desto größer die Änderung in der zufälligen Abbildung in horizontaler und vertikaler Richtung.

Die `numOctaves` Wert ist eine ganze Zahl von 1 oder höher. Dieser bezieht sich auf eine Iteration Faktor für die Algorithmen. Jede zusätzliche Oktave trägt einen Effekt, der ist die Hälfte der vorherigen Oktave, damit der Effekt mit höheren Oktave-Werten verringert.

Die `seed` Parameter ist der Ausgangspunkt für den Zufallszahlen-Generator. Zwar als Gleitkommawert angegeben wird, wird der Bruch abgeschnitten, bevor sie wird verwendet, und 0 der gleiche wie 1 ist.

Die **Perlin-Noise** auf der Seite die [ **SkiaSharpFormsDemos**)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel können Sie experimentieren mit verschiedenen Werten von der `baseFrequency` und `numOctaves` Argumente. Hier ist die XAML-Datei ein:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PerlinNoisePage"
             Title="Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="baseFrequencyXSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyXText"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="baseFrequencyYSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyYText"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference octavesStepper},
                                  Path=Value,
                                  StringFormat='Number of Octaves: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="octavesStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Es verwendet zwei `Slider` Ansichten für die beiden `baseFrequency` Argumente. Um den Bereich der niedrigere Werte zu erweitern, werden die Schieberegler logarithmische. Die Code-Behind-Datei wird berechnet, die Argumente für die `SKShader`Potenzen von Methoden der `Slider` Werte. Die `Label` Ansichten die berechneten Werte anzeigen:

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

Ein `Slider` 0,001, entspricht der Wert 1 ein `Slider` os-2-Wert 0,01, entspricht einer `Slider` Werte 3 bis 0,1, entspricht und ein `Slider` Wert 4 entspricht 1.

So sieht die Code-Behind-Datei, die diesen Code enthält:

```csharp
public partial class PerlinNoisePage : ContentPage
{
    public PerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader =
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0);

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader =
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0);

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Hier ist das Programm Geräte unter iOS, Android und universelle Windows-Plattform (UWP) ausgeführt wird. Das Fraktal Rauschen wird in der oberen Hälfte des Zeichenbereichs angezeigt. Das Rauschen Turbulenzen ist in der unteren Hälfte:

[![Perlin-Noise](noise-images/PerlinNoise.png "Perlin-Noise")](noise-images/PerlinNoise-Large.png#lightbox)

Die gleichen Argumente erzeugt immer das gleiche Muster, das in der oberen linken Ecke beginnt. Diese Konsistenz ist offensichtlich, wenn Sie die Breite und Höhe des Fensters UWP anpassen. Wie Windows 10 auf den Bildschirm zeichnet, bleibt das Muster in der oberen Hälfte des Zeichenbereichs gleich.

Das Rauschen-Muster umfasst mehrere Stufen der Transparenz. Die Transparenz ist offensichtlich, wenn Sie eine Farbe, in Festlegen der `canvas.Clear()` aufrufen. Diese Farbe wird gut sichtbaren im Muster. Außerdem erfahren Sie, diesen Effekt in Abschnitt [ **Kombinieren mehrerer Shader**](#combining-multiple-shaders).

Diese Perlin-Noise-Muster werden selten allein verwendet werden. Sie werden häufig unterzogen, um blend-Modi "und" Filter "Farbe" in späteren Artikeln behandelt.

## <a name="tiling-perlin-noise"></a>Tiling Perlin-noise

Die zwei statische `SKShader` in Überladungsversionen auch Methoden zum Erstellen des Perlin-Noise vorhanden sind. Die [ `CreatePerlinNoiseFractalNoise` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) und [ `CreatePerlinNoiseTurbulence` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) Überladungen verfügen über eine zusätzliche `SKPointI` Parameter:

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

Die [ `SKPointI` ](xref:SkiaSharp.SKPointI) Struktur ist die Integer-Version von der bekannten [ `SKPoint` ](xref:SkiaSharp.SKPoint) Struktur. `SKPointI` definiert `X` und `Y` Eigenschaften des Typs `int` statt `float`.

Diese Methoden erstellen, ein sich wiederholendes Muster der angegebenen Größe. In den einzelnen Kacheln entspricht der rechte Rand am linken Rand und der obere Rand ist identisch mit dem unteren Rand. Dieses Merkmal wird veranschaulicht, der **Perlin-Noise gekachelt** Seite. Die XAML-Datei ähnelt dem vorherigen Beispiel, aber es hat nur eine `Stepper` Ansicht zum Ändern der `seed` Argument:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.TiledPerlinNoisePage"
             Title="Tiled Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference seedStepper},
                                  Path=Value,
                                  StringFormat='Seed: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="seedStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei definiert eine Konstante für die Kachelgröße. Die `PaintSurface` Handler erstellt eine Bitmap, Größe und einem `SKCanvas` zum Zeichnen in diese Bitmap. Die `SKShader.CreatePerlinNoiseTurbulence` Methode erstellt einen Shader, mit dieser Größe. Dieser Shader wird auf die Bitmap gezeichnet:

```csharp
public partial class TiledPerlinNoisePage : ContentPage
{
    const int TILE_SIZE = 200;

    public TiledPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get seed value from stepper
        float seed = (float)seedStepper.Value;

        SKRect tileRect = new SKRect(0, 0, TILE_SIZE, TILE_SIZE);

        using (SKBitmap bitmap = new SKBitmap(TILE_SIZE, TILE_SIZE))
        {
            using (SKCanvas bitmapCanvas = new SKCanvas(bitmap))
            {
                bitmapCanvas.Clear();

                // Draw tiled turbulence noise on bitmap
                using (SKPaint paint = new SKPaint())
                {
                    paint.Shader = SKShader.CreatePerlinNoiseTurbulence(
                                        0.02f, 0.02f, 1, seed,
                                        new SKPointI(TILE_SIZE, TILE_SIZE));

                    bitmapCanvas.DrawRect(tileRect, paint);
                }
            }

            // Draw tiled bitmap shader on canvas
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
                canvas.DrawRect(info.Rect, paint);
            }

            // Draw rectangle showing tile
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 2;

                canvas.DrawRect(tileRect, paint);
            }
        }
    }
}
```

Nachdem die Bitmap erstellt wurde, eine andere `SKPaint` Objekt wird verwendet, um ein Bitmapmuster gekachelten zu erstellen, indem `SKShader.CreateBitmap`. Beachten Sie, dass die zwei Argumente von `SKShaderTileMode.Repeat`:

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

Dieser Shader verwendet, um im Zeichenbereich zu abzudecken. Zum Schluss eine andere `SKPaint` Objekt dient zum Zeichnen eines Rechtecks, das die Größe der ursprünglichen Bitmap anzeigt.

Nur die `seed` Parameter ausgewählt ist, über die Benutzeroberfläche. Wenn die gleiche `seed` Muster, die auf jeder Plattform verwendet wird, würden sie das gleiche Muster angezeigt. Verschiedene `seed` Werte führen verschiedene Muster:

[![Kacheleffekt Perlin-Noise](noise-images/TiledPerlinNoise.png "Perlin-Noise gekachelt")](noise-images/TiledPerlinNoise-Large.png#lightbox)

Das Quadrat 200 Pixel-Muster in der oberen linken Ecke fließen nahtlos in die anderen Kacheln verwendet werden.

## <a name="combining-multiple-shaders"></a>Kombinieren von mehreren Shader

Die `SKShader` Klasse enthält eine [ `CreateColor` ](xref:SkiaSharp.SKShader.CreateColor*) Methode, die einen Shader mit einer angegebenen Volltonfarbe erstellt. Dieser Shader ist nicht allein sehr nützlich, da Sie einfach die entsprechende Farbe, um festlegen können die `Color` Eigenschaft der `SKPaint` Objekt, und legen die `Shader` Eigenschaft auf null.

Dies `CreateColor` Methode ist nützlich, in eine andere Methode, die `SKShader` definiert. Diese Methode ist [ `CreateCompose` ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader)), die zwei Shader kombiniert. Hier ist die Syntax aus:

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

Die `srcShader` (Quelle Shader) effektiv auf der gezeichnet wird die `dstShader` (Ziel Shader). Ist der Source-Shader eine Volltonfarbe oder eines Farbverlaufs ohne Transparenz, wird die Ziel-Shader vollständig verdeckt werden.

Ein Perlin-Noise-Shader enthält Transparenz. Wenn dieser Shader über die Quelle ist, wird die Ziel-Shader durch die transparenten Bereiche angezeigt.

Die **Perlin-Noise zusammengesetzt** Seite verfügt über eine XAML-Datei, die nahezu identisch mit dem ersten **Perlin-Noise** Seite. Es ist auch die Code-Behind-Datei vergleichbar. Aber das ursprüngliche **Perlin-Noise** Seite legt die `Shader` Eigenschaft `SKPaint` an den Shader, der zurückgegeben wird, aus der statischen `CreatePerlinNoiseFractalNoise` und `CreatePerlinNoiseTurbulence` Methoden. Dies **Perlin-Noise zusammengesetzt** Seite Aufrufe `CreateCompose` für eine Kombination aus Shader. Das Ziel ist ein solid blaue Shader mit erstellt `CreateColor`. Die Quelle ist ein Perlin-Noise-Shader:

```csharp
public partial class ComposedPerlinNoisePage : ContentPage
{
    public ComposedPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0));

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0));

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Der Fraktal Rauschen-Shader ist am oberen Rand. der Turbulenzen Shader den Wert unten:

[![Besteht aus Perlin-Noise](noise-images/ComposedPerlinNoise.png "besteht aus Perlin-Noise")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

Beachten Sie, wie viel Bildfarben, diese Shader finden werden, als die angezeigt werden, indem die **Perlin-Noise** Seite. Der Unterschied wird veranschaulicht, die Menge an Transparenz in Shader Rauschen.

Es gibt auch eine Überladung von der [ `CreateCompose` ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) Methode:

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

Der letzte Parameter ist ein Mitglied der `SKBlendMode` -Enumeration, die eine Enumeration mit 29 Membern, die in den nächsten Artikeln erläutert wird, auf [ **SkiaSharp-Zusammensetzung und Blend-Modi**](../blend-modes/index.md).

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
