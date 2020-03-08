---
title: Die zirkuläre SkiaSharp-Farbverläufe
description: Erfahren Sie mehr über die verschiedenen Typen von Farbverläufen, die basierend auf der Kreise.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: d56cc499112a937cd1a22664adeedd54c4397341
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916391"
---
# <a name="the-skiasharp-circular-gradients"></a>Die zirkuläre SkiaSharp-Farbverläufe

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die [`SKShader`](xref:SkiaSharp.SKShader) -Klasse definiert statische Methoden zum Erstellen von vier verschiedenen Typen von Farbverläufen. Der [**lineare skiasharp-Artikel zum linearen Farbverlauf**](linear-gradient.md) erläutert die [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) -Methode. Dieser Artikel behandelt die anderen drei Typen von Farbverläufen an, die auf Kreise basieren.

Die [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*) -Methode erstellt einen Farbverlauf, der von der Mitte eines Kreises ausgeht:

![Beispiel für radialen Farbverlauf](circular-gradients-images/RadialGradientSample.png)

Die [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*) -Methode erstellt einen Farbverlauf, der um den Mittelpunkt eines Kreises herum zieht:

![Sweep-Farbverlauf-Beispiel](circular-gradients-images/SweepGradientSample.png)

Der dritte Typ des Farbverlaufs ist sehr ungewöhnlich. Sie wird als zwei-Punkt-konischen Farbverlauf bezeichnet und wird durch die [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) -Methode definiert. Der Gradient, die in eine andere aus einem einzelnen Kreis erweitert werden:

![Beispiel für konische Farbverlauf](circular-gradients-images/ConicalGradientSample.png)

Wenn die beiden Kreise unterschiedlicher Größe sind, wird der Gradient der Form eines Kegels.

In diesem Artikel untersucht diese Farbverläufe im Detail.

## <a name="the-radial-gradient"></a>Radialen Farbverlauf

Die [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) -Methode weist die folgende Syntax auf:

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

Eine [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung enthält auch einen Transform Matrix-Parameter.

Die ersten beiden Argumente geben Sie den Mittelpunkt eines Kreises und dem Radius. Der Farbverlauf beginnt bei diesem Mittelpunkt und wird für `radius` Pixel nach außen erweitert. Was über `radius` hinausgeht, hängt vom [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) -Argument ab. Der `colors`-Parameter ist ein Array aus mindestens zwei Farben (genau wie in den linearen Farbverlaufs Methoden), und `colorPos` ist ein Array von ganzen Zahlen im Bereich von 0 bis 1. Diese ganzzahligen Werte geben die relativen Positionen der Farben entlang der `radius` Zeile an. Sie können dieses Argument auf `null` festlegen, um die Farben gleichmäßig zu leer zu lassen.

Wenn Sie `CreateRadialGradient` zum Auffüllen eines Kreises verwenden, können Sie den Mittelpunkt des Farbverlaufs auf den Mittelpunkt des Kreises und den Radius des Farbverlaufs auf den Radius des Kreises festlegen. In diesem Fall hat das `SKShaderTileMode`-Argument keine Auswirkung auf das Rendering des Farbverlaufs. Wenn jedoch der Bereich, der durch den Farbverlauf gefüllt ist, größer als der durch den Farbverlauf definierte Kreis ist, hat das `SKShaderTileMode`-Argument eine Tiefe Auswirkung auf das, was außerhalb des Kreises passiert.

Die Auswirkung von `SKShaderMode` wird im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel auf der **radialen Farbverlaufs** Seite veranschaulicht. Die XAML-Datei für diese Seite instanziiert eine `Picker`, die es Ihnen ermöglicht, eines der drei Member der `SKShaderTileMode`-Enumeration auszuwählen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.RadialGradientPage"
             Title="Radial Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="0"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei Farben den gesamten Zeichenbereich mit einem radialen Farbverlauf. Die Mitte des Farbverlaufs in der Mitte des die Leinwand festgelegt ist, und der Radius auf 100 Pixel festgelegt ist. Der Gradient besteht aus zwei Farben, Schwarz oder weiß:

```csharp
public partial class RadialGradientPage : ContentPage
{
    public RadialGradientPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                new SKPoint(info.Rect.MidX, info.Rect.MidY),
                                100,
                                new SKColor[] { SKColors.Black, SKColors.White },
                                null,
                                tileMode);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Dieser Code erstellt einen Farbverlauf mit Schwarz, zentriert, um 100 Pixel aus dem Center weiß allmählich eingeblendet. Was über diesen RADIUS hinausgeht, hängt vom `SKShaderTileMode` Argument ab:

[![Radialer Farbverlauf](circular-gradients-images/RadialGradient.png "Radialer Farbverlauf")](circular-gradients-images/RadialGradient-Large.png#lightbox)

In allen drei Fällen füllt der Farbverlauf für den Zeichenbereich. Der Farbverlauf außerhalb der Radius wird auf dem Bildschirm "iOS" auf der linken Seite die letzte Farbe an, die weiß ist fortgesetzt. Dies ist das Ergebnis `SKShaderTileMode.Clamp`. Der Android-Bildschirm zeigt die Auswirkung von `SKShaderTileMode.Repeat`: bei 100 Pixel von der Mitte beginnt der Farbverlauf erneut mit der ersten Farbe, die schwarz ist. Der Farbverlauf wird jeder 100 Pixel von Radius wiederholt. 

Der Bildschirm universelle Windows-Plattform auf der rechten Seite zeigt, wie `SKShaderTileMode.Mirror` die Farbverläufe in eine Alternative Richtung auslöst. Der erste Farbverlauf liegt zwischen Schwarz, zentriert und weiß, an einen Radius von 100 Pixel. Das nächste ist von den Radius von 100 Pixel auf Schwarz festgelegt, an einen Radius von 200 Pixel weiß, und weiter Farbverlaufs wieder rückgängig gemacht.

Sie können mehr als zwei Farben in einem radialen Farbverlauf. Mit dem Beispiel für den **Regenbogen Bogen Verlauf** wird ein Array mit acht Farben erstellt, das den Farben des Regenbogen Werts entspricht und mit rot endet, sowie ein Array von acht Positions Werten:

```csharp
public class RainbowArcGradientPage : ContentPage
{
    public RainbowArcGradientPage ()
    {
        Title = "Rainbow Arc Gradient";

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
            float rainbowWidth = Math.Min(info.Width, info.Height) / 4f;

            // Center of arc and gradient is lower-right corner
            SKPoint center = new SKPoint(info.Width, info.Height);

            // Find outer, inner, and middle radius
            float outerRadius = Math.Min(info.Width, info.Height);
            float innerRadius = outerRadius - rainbowWidth;
            float radius = outerRadius - rainbowWidth / 2;

            // Calculate the colors and positions
            SKColor[] colors = new SKColor[8];
            float[] positions = new float[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
                positions[i] = (i + (7f - i) * innerRadius / outerRadius) / 7f;
            }

            // Create sweep gradient based on center and outer radius
            paint.Shader = SKShader.CreateRadialGradient(center, 
                                                         outerRadius, 
                                                         colors, 
                                                         positions, 
                                                         SKShaderTileMode.Clamp);
            // Draw a circle with a wide line
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = rainbowWidth;

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Angenommen, das minimale Breite und die Höhe der Canvas beträgt 1000, was bedeutet, dass der `rainbowWidth` Wert 250 ist. Die Werte für `outerRadius` und `innerRadius` werden auf 1000 bzw. 750 festgelegt. Diese Werte werden zum Berechnen des `positions` Arrays verwendet. die acht Werte liegen zwischen 0,75 f und 1. Der `radius`-Wert wird zum Durchsuchen des Kreises verwendet. Der Wert 875 bedeutet, dass die Strichbreite 250 Pixel zwischen den Radius der 750 Pixel und den Radius der 1000 Pixel erweitert:

[![Verlauf des Regenbogen Bogens](circular-gradients-images/RainbowArcGradient.png "Verlauf des Regenbogen Bogens")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

Wenn Sie mit diesem Verlauf die gesamte Canvas ausgefüllt haben, sehen Sie sich, dass es innerhalb des inneren Radius Rot ist. Dies liegt daran, dass das `positions` Array nicht mit 0 beginnt. Die erste Farbe wird für die Offsets von 0 bis das erste Arraywert verwendet. Der Gradient ist auch über den Radius des äußeren Rot. Das ist das Ergebnis des `Clamp` Kachel Modus. Da der Gradient für die Kontur zuweisen eine starke Linie verwendet wird, sind diese roten Bereiche nicht sichtbar.

## <a name="radial-gradients-for-masking"></a>Radiale Farbverläufe für die Maskierung

Wie linearen Farbverläufen können radiale Farbverläufen transparente oder teilweise transparente Farben integrieren. Diese Funktion ist nützlich für einen Prozess namens _Maskierung_, der einen Teil eines Bilds verbirgt, um einen anderen Teil des Bilds hervorzuheben.

Die Seite **radiale Farbverlaufs Maske** zeigt ein Beispiel an. Das Programm lädt eine der Bitmaps Ressource. Die Felder `CENTER` und `RADIUS` wurden von einer Untersuchung der Bitmap bestimmt und verweisen auf einen Bereich, der hervorgehoben werden soll. Der `PaintSurface` Handler beginnt mit dem Berechnen eines Rechtecks, um die Bitmap anzuzeigen, und zeigt Sie dann in diesem Rechteck an:

```csharp
public class RadialGradientMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(RadialGradientMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public RadialGradientMaskPage ()
    {
        Title = "Radial Gradient Mask";

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

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                                (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                     scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Transparent,
                                                SKColors.White },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            // Display rectangle using that gradient
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Nach dem Zeichnen der Bitmap konvertiert ein einfacher Code `CENTER` und `RADIUS` in `center` und `radius`, die auf den markierten Bereich in der Bitmap verweisen, der zum Anzeigen skaliert und verschoben wurde. Diese Werte werden verwendet, zum Erstellen von eines radialen Farbverlaufs mit diesem Mittelpunkt und dem Radius. Die zwei Farben beginnen bei in der Mitte und für die ersten 60 % des Radius transparent. Der Farbverlauf wird dann weiß:

[![Radiale Verlaufs Maske](circular-gradients-images/RadialGradientMask.png "Radiale Verlaufs Maske")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

Dieser Ansatz ist nicht die beste Möglichkeit, eine Bitmap zu maskieren. Das Problem ist, dass die Maske größtenteils eine Farbe Weiß, das den Hintergrund des Zeichenbereichs entsprechend ausgewählt wurde. Wenn der Hintergrund eine andere Farbe &mdash; oder vielleicht ein Farbverlauf &mdash;, stimmt er nicht mit. Ein besserer Ansatz für die Maskierung ist im Artikel [skiasharp Porter-Duff Blend-Modi](../blend-modes/porter-duff.md)dargestellt.

## <a name="radial-gradients-for-specular-highlights"></a>Radiale Farbverläufe für Glanzlichter

Wenn Sie ein Licht eine abgerundete Oberfläche eintritt, es Licht in viele Richtungen widerspiegelt, aber einige der Lichtquelle direkt in des anzeigenden Benutzers Auge springt. Dies führt häufig dazu, dass ein fuzzyweißer Bereich auf der Oberfläche, _der als Glanz_Lichter bezeichnet wird, dargestellt wird.

Dreidimensionale Grafiken führen Glanzlichter häufig von den Algorithmen verwendet, um die light-Pfade und Schattierung zu bestimmen. In zweidimensionalen Grafiken werden Glanzlichter manchmal hinzugefügt, um die Darstellung einer 3D-Oberfläche vorzuschlagen. Eine Lichtreflexe kann einen flachen roten Kreis in eine rote Kugel umwandeln.

Die **radiale Glanz hervor** gehobene Seite verwendet einen radialen Farbverlauf, um genau das zu erledigen. Die `PaintSurface` handlerwesen durch Berechnen eines RADIUS für den Kreis und zwei `SKPoint` Werte &mdash; ein `center` und eine `offCenter`, die sich in der Mitte zwischen dem Mittelpunkt und dem oberen linken Rand des Kreises befindet:

```csharp
public class RadialSpecularHighlightPage : ContentPage
{
    public RadialSpecularHighlightPage()
    {
        Title = "Radial Specular Highlight";

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

        float radius = 0.4f * Math.Min(info.Width, info.Height);
        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        SKPoint offCenter = center - new SKPoint(radius / 2, radius / 2);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                offCenter,
                                radius / 2,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Der `CreateRadialGradient`-Befehl erstellt einen Farbverlauf, der bei diesem `offCenter` Punkt mit weiß beginnt und mit rot in einem Abstand von der Hälfte des RADIUS endet. Und so sieht es aus:

[![Radiale Glanz Markierung](circular-gradients-images/RadialSpecularHighlight.png "Radiale Glanz Markierung")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

Wenn Sie diesen Farbverlauf genauer betrachten, könnten Sie, dass er fehlerhaft ist. Farbverlaufs basiert auf einem bestimmten Zeitpunkt aus, und Sie möchten möglicherweise, er wäre etwas weniger symmetrische entsprechend die abgerundete Oberfläche. In diesem Fall bevorzugen Sie möglicherweise die unten gezeigte Glanz Markierung im Abschnitt " [**kegelförmige Farbverläufe" für Glanzlichter**](#conical-gradients-for-specular-highlights).

## <a name="the-sweep-gradient"></a>Die geleerte Farbverlauf

Die [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[])) -Methode verfügt über die einfachste Syntax aller Methoden zur Farbverlauf Erstellung:

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Es ist nur in einem Rechenzentrum, ein Array von Farben und die Positionen der Farbe. Den Farbverlauf auf der rechten Seite des Mittelpunkts beginnt und zieht 360 Grad im Uhrzeigersinn um den Mittelpunkt. Beachten Sie, dass es keine `SKShaderTileMode` Parameter gibt.

Außerdem ist eine [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix)) Überladung mit einem Matrix Transformationsparameter verfügbar. Sie können einen drehungsübergang, ändern Sie den Anfangspunkt, an den Farbverlauf anwenden. Sie können auch eine Skalierungstransformation, um die Richtung aus im Uhrzeigersinn um gegen den Uhrzeigersinn ändern anwenden.

Die Seite " **Sweep Gradient** " verwendet einen Sweep-Farbverlauf, um einen Kreis mit einer Strichbreite von 50 Pixeln zu färben:

[![Sweep-Farbverlauf](circular-gradients-images/SweepGradient.png "Sweep-Farbverlauf")](circular-gradients-images/SweepGradient-Large.png#lightbox)

Die `SweepGradientPage`-Klasse definiert ein Array mit acht Farben mit unterschiedlichen Farbton Werten. Beachten Sie, dass das Array beginnt und endet mit roten (eine Hue-Wert von 0 oder 360), die ganz rechts in den Screenshots angezeigt wird:

```csharp
public class SweepGradientPage : ContentPage
{
    bool drawBackground;

    public SweepGradientPage ()
    {
        Title = "Sweep Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            drawBackground ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Define an array of rainbow colors
            SKColor[] colors = new SKColor[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
            }

            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);

            // Create sweep gradient based on center of canvas
            paint.Shader = SKShader.CreateSweepGradient(center, colors, null);

            // Draw a circle with a wide line
            const int strokeWidth = 50;
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = strokeWidth;

            float radius = (Math.Min(info.Width, info.Height) - strokeWidth) / 2;
            canvas.DrawCircle(center, radius, paint);

            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                paint.Style = SKPaintStyle.Fill;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

Das Programm implementiert auch eine `TapGestureRecognizer`, die Code am Ende des `PaintSurface` Handlers ermöglicht. Dieser Code verwendet den gleichen Farbverlauf auf den Zeichenbereich ausfüllen:

[![Sweep-Farbverlauf voll](circular-gradients-images/SweepGradientFull.png "Sweep-Farbverlauf voll")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

Diese Screenshots zeigen, dass das graduelle Füllungen unabhängig von dem Bereich von ihm gefärbt ist. Wenn der Farbverlauf nicht beginnen und mit der gleichen Farbe enden, wird eine Diskontinuität auf der rechten Seite des Mittelpunkts vorhanden sein.

## <a name="the-two-point-conical-gradient"></a>Zwei-zackiger konische Farbverlaufs

Die [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) -Methode weist die folgende Syntax auf:

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

Die Parameter beginnen mit Mittelpunkten und Radien für zwei Kreise, die als _Start_ Kreis und _Endkreis_ bezeichnet werden. Die verbleibenden drei Parameter sind identisch mit denen für `CreateLinearGradient` und `CreateRadialGradient`. Eine [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung enthält eine Matrix Transformation.

Des Farbverlaufs an den Kreis Start beginnt und endet mit der End-Kreis. Der `SKShaderTileMode`-Parameter bestimmt, was über die beiden Kreise hinausgeht. Zwei-zackiger konische Farbverlaufs ist nur Farbverlaufs, der nicht vollständig über einen Bereich ausfüllt. Wenn die beiden Kreise identischem Radius verfügen, ist der Gradient auf ein Rechteck mit einer Breite, die der Durchmesser der Kreise identisch ist beschränkt. Wenn die beiden Kreise verschiedene Radien verfügen, bildet der Gradient ein Kegels.

Es ist wahrscheinlich, dass Sie mit dem zweistufigen kegelförmigen Farbverlauf experimentieren möchten, sodass die Seite " **kegelförmige Farbverlauf** " von `InteractivePage` abgeleitet wird, damit zwei Berührungspunkte für die beiden kreisförmigen Radii bewegt werden können:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.ConicalGradientPage"
                       Title="Conical Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <Grid BackgroundColor="White"
              Grid.Row="0">
            <skiaforms:SKCanvasView x:Name="canvasView"
                                    PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</local:InteractivePage>
```

Die Code-Behind-Datei definiert die beiden `TouchPoint`-Objekte mit dem festgelegten bogenwert 50 und 100:

```csharp
public partial class ConicalGradientPage : InteractivePage
{
    const int RADIUS1 = 50;
    const int RADIUS2 = 100;

    public ConicalGradientPage ()
    {
        touchPoints = new TouchPoint[2];

        touchPoints[0] = new TouchPoint
        {
            Center = new SKPoint(100, 100),
            Radius = RADIUS1
        };

        touchPoints[1] = new TouchPoint
        {
            Center = new SKPoint(300, 300),
            Radius = RADIUS2
        };

        InitializeComponent();
        baseCanvasView = canvasView;
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKColor[] colors = { SKColors.Red, SKColors.Green, SKColors.Blue };
        SKShaderTileMode tileMode = 
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ? 
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(touchPoints[0].Center,
                                                                  RADIUS1,
                                                                  touchPoints[1].Center,
                                                                  RADIUS2,
                                                                  colors,
                                                                  null,
                                                                  tileMode);
            canvas.DrawRect(info.Rect, paint);
        }

        // Display the touch points here rather than by TouchPoint
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;

            foreach (TouchPoint touchPoint in touchPoints)
            {
                canvas.DrawCircle(touchPoint.Center, touchPoint.Radius, paint);
            }
        }
    }
}
```

Das `colors` Array ist rot, grün und blau. Der Code im unteren Bereich des `PaintSurface` Handlers zeichnet die beiden Berührungspunkte als schwarze Kreise, sodass Sie den Farbverlauf nicht behindern.

Beachten Sie, dass `DrawRect`-Aufrufe den Farbverlauf verwendet, um den gesamten Zeichenbereich zu Farben Im Allgemeinen bleibt ein Großteil der Leinwand jedoch nicht durch den Verlauf farbigen. Hier ist das Programm zeigt drei mögliche Konfigurationen:

[![Kegelförmiger Farbverlauf](circular-gradients-images/ConicalGradient.png "Kegelförmiger Farbverlauf")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

Der IOS-Bildschirm auf der linken Seite zeigt die Auswirkung der `SKShaderTileMode` Einstellung von `Clamp`an. Der Gradient beginnt mit Rot in den Rand des Kreises kleinere, das Gegenstück der Seite auf den zweiten Kreis am nächsten ist. Der `Clamp` Wert bewirkt auch, dass der rote-Wert auch an den Punkt des Kegel weiter geht. Der Gradient endet mit blauen am äußeren Rand des Kreises größer, die auf den ersten Kreis am nächsten ist, aber weiterhin mit Blau innerhalb dieser Kreises und darüber hinaus.

Der Android-Bildschirm ist ähnlich, aber mit einer `SKShaderTileMode` `Repeat`. Jetzt ist es klarer, dass den Farbverlauf innerhalb des Kreises erste beginnt und endet, die außerhalb der zweiten Kreis. Die `Repeat` Einstellung bewirkt, dass der Farbverlauf erneut mit rot innerhalb des größeren Kreises wiederholt wird.

Der UWP-Bildschirm zeigt an, was geschieht, wenn es sich bei der kleinere Kreis vollständig innerhalb der größeren Kreis bewegt wird. Der Gradient beendet, der einen Kegel und stattdessen füllt den gesamten Bereich. Die Auswirkungen sind vergleichbar mit der radialen Farbverlauf, aber es asymmetrisch ist, wenn es sich bei der kleinere Kreis nicht genau innerhalb der größeren Kreis zentriert ist.

Sie können den praktischen Nutzen des Farbverlaufs entspricht bei einem einzelnen Kreis in einer anderen geschachtelt ist, aber es empfiehlt sich für eine Lichtreflexe bezweifle.

## <a name="conical-gradients-for-specular-highlights"></a>Konische Gradienten für Glanzlichter

In diesem Artikel wurde erläutert, wie einen radialen Farbverlauf zu verwenden, um eine Lichtreflexe zu erstellen. Sie können auch zwei Punkt konische Farbverlaufs zu diesem Zweck verwenden, und möglicherweise bevorzugen Sie aussehen:

[![Kegelförmige Hervorhebung](circular-gradients-images/ConicalSpecularHighlight.png "Kegelförmige Hervorhebung")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

Die asymmetrische Darstellung schlägt besser vor der abgerundeten Oberfläche des Objekts. 

Der Zeichnungs Code auf der Seite " **kegelförmige Hervorhebung** " ist mit Ausnahme des Shaders identisch mit der **radialen** Glanz Seite:

```csharp
public class ConicalSpecularHighlightPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(
                                offCenter,
                                1,
                                center,
                                radius,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Die beiden Kreise haben Mittel aus `offCenter` und `center`. Der in `center` zentrierte Kreis ist einem RADIUS zugeordnet, der die gesamte Kugel umfasst, aber der Kreis, der sich auf `offCenter` zentriert befindet, verfügt über einen Radius von nur einem Pixel. Der Gradient ist effektiv an diesem Punkt beginnt und endet am Rand des Balls.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
