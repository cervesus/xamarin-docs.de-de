---
title: Skiasharp-Rauschen und Komposition
description: Generieren von Perlin-Rausch-Shadern und kombinieren mit anderen Shadern.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 45ec48c0b7b58e26fa47d7343e96bb49591cb339
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127763"
---
# <a name="skiasharp-noise-and-composing"></a>Skiasharp-Rauschen und Komposition

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Einfache Vektorgrafiken sehen tendenziell unnatürlich aus. Die geraden Linien, weichen Kurven und voll Tonfarben ähneln nicht den Unzulänglichkeiten von realen Objekten. Beim Arbeiten an den computergenerierten Grafiken für das 1982 Movie _Tron_begann der Computer Analyst Ken Perlin mit der Entwicklung von Algorithmen, die zufällige Prozesse verwendet haben, um diese Bilder realistischer Texturen zuzuweisen. In 1997 hat Ken Perlin eine Academy-Auszeichnung für technische Leistungen gewonnen. Seine Arbeit wurde als Perlin-Rauschen bezeichnet und wird in skiasharp unterstützt. Hier sehen Sie ein Beispiel:

![Perlin-Rausch Beispiel](noise-images/NoiseSample.png "Perlin-Rausch Beispiel")

Wie Sie sehen können, ist jedes Pixel kein zufälliger Farbwert. Die Kontinuität zwischen Pixel und Pixel führt zu zufälligen Formen.

Die Unterstützung von Perlin-Rauschen in Skia basiert auf der W3C-Spezifikation für CSS und SVG. Abschnitt 8,20 der [**Filter Effekte Modulebene 1**](https://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) enthält die zugrunde liegenden Perlin-Rausch Algorithmen in C-Code.

## <a name="exploring-perlin-noise"></a>Untersuchen von Perlin-Rauschen

Die [`SKShader`](xref:SkiaSharp.SKShader) -Klasse definiert zwei verschiedene statische Methoden zum Erzeugen von Perlin [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) -Rauschen: und [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*) . Die Parameter sind identisch:

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

Beide Methoden sind auch in überladenen Versionen mit einem zusätzlichen `SKPointI` Parameter vorhanden. In diesem [**Abschnitt werden**](#tiling-perlin-noise) diese über Ladungen erörtert.

Die beiden `baseFrequency` Argumente sind positive Werte, die in der skiasharp-Dokumentation als Bereich von 0 bis 1 definiert sind. Sie können jedoch auch auf höhere Werte festgelegt werden. Je höher der Wert ist, desto größer ist die Änderung des Zufalls Bilds in horizontaler und vertikaler Richtung.

Der `numOctaves` Wert ist eine Ganzzahl von 1 oder höher. Er bezieht sich auf einen Iterations Faktor in den Algorithmen. Jede zusätzliche Oktave trägt zu einem Effekt bei, der der Hälfte der vorherigen Oktave entspricht, sodass der Effekt mit höheren oktaatenwerten abnimmt.

Der- `seed` Parameter ist der Ausgangspunkt für den Zufallszahlengenerator. Obwohl es als Gleit Komma Wert angegeben wird, wird der Bruchteil abgeschnitten, bevor es verwendet wird, und 0 ist gleich 1.

Die Seite " **Perlin-Rauschen** " im [ **skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel ermöglicht Ihnen das Experimentieren mit verschiedenen Werten der `baseFrequency` `numOctaves` Argumente und. Hier ist die XAML-Datei:

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

Es verwendet zwei `Slider` Sichten für die beiden `baseFrequency` Argumente. Um den Bereich der niedrigeren Werte zu erweitern, sind die Schieberegler logarithmisch. Die Code-Behind-Datei berechnet die Argumente der `SKShader` Methoden aus den Werten der- `Slider` Werte. In den `Label` Sichten werden die berechneten Werte angezeigt:

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

`Slider`Der Wert 1 entspricht 0,001, ein `Slider` Wert OS 2 0,01, `Slider` der Wert 3 entspricht 0,1, und `Slider` der Wert 4 entspricht 1.

Hier ist die Code-Behind-Datei, die den folgenden Code enthält:

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

Hier ist das Programm, das auf Ios-, Android-und universelle Windows-Plattform-Geräten (UWP) ausgeführt wird. Das Dezimaltrennzeichen wird in der oberen Hälfte der Canvas angezeigt. Das Turbulenzen Rauschen liegt in der unteren Hälfte:

[![Perlin-Rauschen](noise-images/PerlinNoise.png "Perlin-Rauschen")](noise-images/PerlinNoise-Large.png#lightbox)

Dieselben Argumente ergeben immer dasselbe Muster, das in der oberen linken Ecke beginnt. Diese Konsistenz ist offensichtlich, wenn Sie die Breite und Höhe des UWP-Fensters anpassen. Wenn Windows 10 den Bildschirm neu zeichnet, bleibt das Muster in der oberen Hälfte der Canvas unverändert.

Das Rauschmuster beinhaltet verschiedene Transparenz Grade. Die Transparenz wird offensichtlich, wenn Sie im-Befehl eine Farbe festlegen `canvas.Clear()` . Diese Farbe wird im Muster hervorgehoben. Diese Auswirkung sehen Sie auch im Abschnitt [**Kombinieren mehrerer Shader**](#combining-multiple-shaders).

Diese Perlin-Rauschmuster werden nur selten verwendet. Häufig werden Sie den Blend-Modi und Farbfiltern unterzogen, die in späteren Artikeln erläutert werden.

## <a name="tiling-perlin-noise"></a>Das Ticken von Perlin-Rauschen

Die beiden statischen `SKShader` Methoden zum Erstellen von Perlin-Rauschen sind auch in Überladungs Versionen vorhanden. Die [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) -und- [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) über Ladungen verfügen über einen zusätzlichen `SKPointI` Parameter:

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

Die- [`SKPointI`](xref:SkiaSharp.SKPointI) Struktur ist die ganzzahlige Version der vertrauten- [`SKPoint`](xref:SkiaSharp.SKPoint) Struktur. `SKPointI`definiert `X` -und- `Y` Eigenschaften vom Typ `int` anstelle von `float` .

Diese Methoden erstellen ein sich wiederholendes Muster mit der angegebenen Größe. In jeder Kachel ist der Rechte Rand mit dem linken Rand identisch, und der obere Rand ist mit dem unteren Rand identisch. Diese Eigenschaft wird auf der Seite **gekachelte Perlin-Rauschen** veranschaulicht. Die XAML-Datei ähnelt dem vorherigen Beispiel, aber Sie verfügt nur über eine `Stepper` Sicht zum Ändern des `seed` Arguments:

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

Mit der Code Behind-Datei wird eine Konstante für die Kachel Größe definiert. Der `PaintSurface` Handler erstellt eine Bitmap dieser Größe und eine `SKCanvas` zum Zeichnen in diese Bitmap. Die- `SKShader.CreatePerlinNoiseTurbulence` Methode erstellt einen Shader mit dieser Kachel Größe. Dieser Shader wird in der Bitmap gezeichnet:

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

Nachdem die Bitmap erstellt wurde, `SKPaint` wird ein anderes-Objekt verwendet, um ein gekacheltes Bitmap-Muster durch Aufrufen von zu erstellen `SKShader.CreateBitmap` . Beachten Sie die beiden Argumente von `SKShaderTileMode.Repeat` :

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

Dieser Shader dient zum Abdecken der Canvas. Schließlich `SKPaint` wird ein anderes-Objekt verwendet, um ein Rechteck zu zeichnen, das die Größe der ursprünglichen Bitmap anzeigt.

Nur der `seed` Parameter kann von der Benutzeroberfläche aus ausgewählt werden. Wenn das gleiche `seed` Muster auf jeder Plattform verwendet wird, zeigen Sie das gleiche Muster an. Unterschiedliche `seed` Werte führen zu unterschiedlichen Mustern:

[![Gekachelte Perlin-Rauschen](noise-images/TiledPerlinNoise.png "Gekachelte Perlin-Rauschen")](noise-images/TiledPerlinNoise-Large.png#lightbox)

Das quadratische Muster 200 Pixel in der oberen linken Ecke verläuft nahtlos in die anderen Kacheln.

## <a name="combining-multiple-shaders"></a>Kombinieren mehrerer Shader

Die- `SKShader` Klasse enthält eine- [`CreateColor`](xref:SkiaSharp.SKShader.CreateColor*) Methode, die einen Shader mit einer angegebenen voll Tonfarbe erstellt. Dieser Shader ist nicht sehr nützlich, da Sie diese Farbe einfach auf die `Color` -Eigenschaft des `SKPaint` -Objekts festlegen und die- `Shader` Eigenschaft auf NULL festlegen können.

Diese `CreateColor` Methode wird in einer anderen Methode nützlich, die `SKShader` definiert. Diese Methode ist [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader)) , mit der zwei Shader kombiniert werden. Hier ist die Syntax:

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

Der `srcShader` (quellshader) wird tatsächlich über den `dstShader` (zielshader) gezeichnet. Wenn der quellshader eine voll Tonfarbe oder einen Farbverlauf ohne Transparenz ist, wird der zielshader vollständig verdeckt.

Ein Perlin-Rausch-Shader enthält Transparenz. Wenn dieser Shader die Quelle ist, wird der zielshader über die transparenten Bereiche angezeigt.

Die zusammen **gesetzte Perlin-Rausch** Seite verfügt über eine XAML-Datei, die mit der ersten **Perlin-Rausch** Seite nahezu identisch ist. Die Code-Behind-Datei ist ebenfalls ähnlich. Die ursprüngliche **Perlin-Rausch** Seite legt jedoch die `Shader` -Eigenschaft von `SKPaint` auf den Shader fest, der von den statischen Methoden und zurückgegeben wird `CreatePerlinNoiseFractalNoise` `CreatePerlinNoiseTurbulence` . Diese zusammen **gesetzte Perlin-Rausch** Seitenaufrufe `CreateCompose` für einen Kombinations-Shader. Das Ziel ist ein solider blauer Shader, der mithilfe von erstellt wurde `CreateColor` . Die Quelle ist ein Perlin-Rausch-Shader:

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

Der obere Füll-Shader befindet sich oben. der Turbulenz-Shader befindet sich am Ende:

[![Zusammengesetzte Perlin-Rauschen](noise-images/ComposedPerlinNoise.png "Zusammengesetzte Perlin-Rauschen")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

Beachten Sie, wie viel bluler diese Shader sind, als diejenigen, die von der **Perlin-Rausch** Seite angezeigt werden. Der Unterschied zeigt den Umfang der Transparenz in den Rauschen-Shadern.

Außerdem gibt es eine Überladung der- [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) Methode:

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

Der letzte Parameter ist ein Member der- `SKBlendMode` Enumeration, eine Enumeration mit 29 Membern, die in der nächsten Reihe von Artikeln zu [**skiasharp Zusammensetzung-und Blend-Modi**](../blend-modes/index.md)erläutert wird.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
