---
title: Die zirkuläre SkiaSharp-Farbverläufe
description: Erfahren Sie mehr über die verschiedenen Typen von Farbverläufen, die basierend auf der Kreise.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: a17ddf438856600870c9bb3da60a5f4667128d57
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056044"
---
# <a name="the-skiasharp-circular-gradients"></a>Die zirkuläre SkiaSharp-Farbverläufe

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Die [ `SKShader` ](xref:SkiaSharp.SKShader) -Klasse definiert statische Methoden zum Erstellen von vier verschiedenen Typen von Farbverläufen an. Die [ **SkiaSharp linearer Farbverlauf** ](linear-gradient.md) Artikel behandelt die [ `CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient*) Methode. Dieser Artikel behandelt die anderen drei Typen von Farbverläufen an, die auf Kreise basieren.

Die [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient*) Methode erstellt einen Farbverlauf, der von der Mitte des einen Kreis Sicherheitsprotokollen:

![Beispiel für radialen Farbverlauf](circular-gradients-images/RadialGradientSample.png)

Die [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient*) Methode erstellt einen Farbverlauf an, die um den Mittelpunkt eines Kreises führt ein Sweep:

![Sweep-Farbverlauf-Beispiel](circular-gradients-images/SweepGradientSample.png)

Der dritte Typ des Farbverlaufs ist sehr ungewöhnlich. Es heißt, dass zwei Punkt konische Farbverlaufs und wird definiert, indem die [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) Methode. Der Gradient, die in eine andere aus einem einzelnen Kreis erweitert werden:

![Beispiel für konische Farbverlauf](circular-gradients-images/ConicalGradientSample.png)

Wenn die beiden Kreise unterschiedlicher Größe sind, wird der Gradient der Form eines Kegels.

In diesem Artikel untersucht diese Farbverläufe im Detail.

## <a name="the-radial-gradient"></a>Radialen Farbverlauf

Die [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) Methode hat die folgende Syntax:

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

Ein [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung umfasst auch einen Transformation Matrix-Parameter.

Die ersten beiden Argumente geben Sie den Mittelpunkt eines Kreises und dem Radius. Der Farbverlauf an, die von Center beginnt und für nach außen `radius` Pixel. Was geschieht, über `radius` hängt von der [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) Argument. Die `colors` -Parameter ist ein Array von zwei oder mehr Farben (genau wie in der linearen Farbverlauf Methoden), und `colorPos` ist ein Array von ganzen Zahlen im Bereich von 0 bis 1. Diese ganze Zahlen geben die relativen Positionen der Farben an, die `radius` Zeile. Legen Sie dieses Argument auf `null` , ebenso die Farben Leerzeichen.

Bei Verwendung von `CreateRadialGradient` um einen Kreis zu füllen, können Sie die Mitte des Farbverlaufs auf den Mittelpunkt des Kreises und den Radius des Farbverlaufs entspricht, der den Radius des Kreises festlegen. In diesem Fall die `SKShaderTileMode` Argument hat keine Auswirkungen auf das Rendering des Farbverlaufs entspricht. Aber wenn die von den Farbverlauf ausgefüllten Bereich größer als den Kreis, der von der Gradient, definiert die `SKShaderTileMode` Argument hat nachhaltige Auswirkungen auf außerhalb des Kreises Vorgänge.

Die Auswirkungen der `SKShaderMode` wird veranschaulicht, der **Radialer Farbverlauf** auf der Seite die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel. Die XAML-Datei für diese Seite instanziiert ein `Picker` , ermöglicht Ihnen die Auswahl eines der drei Elemente von der `SKShaderTileMode` Enumeration:

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

Dieser Code erstellt einen Farbverlauf mit Schwarz, zentriert, um 100 Pixel aus dem Center weiß allmählich eingeblendet. Was geschieht, über diese Radius hängt von der `SKShaderTileMode` Argument:

[![Radialen Farbverlauf](circular-gradients-images/RadialGradient.png "Radialer Farbverlauf")](circular-gradients-images/RadialGradient-Large.png#lightbox)

In allen drei Fällen füllt der Farbverlauf für den Zeichenbereich. Der Farbverlauf außerhalb der Radius wird auf dem Bildschirm "iOS" auf der linken Seite die letzte Farbe an, die weiß ist fortgesetzt. Das ist das Ergebnis der `SKShaderTileMode.Clamp`. Die Android-Bildschirm zeigt die Auswirkungen der `SKShaderTileMode.Repeat`: unter 100 Pixel von der Mitte des Farbverlaufs beginnt erneut mit der ersten Farbe, die Schwarz ist. Der Farbverlauf wird jeder 100 Pixel von Radius wiederholt. 

Die universelle Windows-Plattform-Bildschirm auf die richtige wie `SKShaderTileMode.Mirror` bewirkt, dass die Gradienten an, die alternative Richtungen. Der erste Farbverlauf liegt zwischen Schwarz, zentriert und weiß, an einen Radius von 100 Pixel. Das nächste ist von den Radius von 100 Pixel auf Schwarz festgelegt, an einen Radius von 200 Pixel weiß, und weiter Farbverlaufs wieder rückgängig gemacht.

Sie können mehr als zwei Farben in einem radialen Farbverlauf. Die **Rainbow Bogens graduell** Beispiel erstellt ein Array von acht Farben, die Farben der Rainbow und bis hin zu rot entspricht, und auch ein Array von acht Positionswerte:

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

Nehmen Sie an das Minimum der Breite und Höhe des Zeichenbereichs ist 1000. Dies bedeutet, dass die `rainbowWidth` Wert ist 250. Die `outerRadius` und `innerRadius` Werte auf 1000 festgelegt und 750, bzw. Diese Werte werden verwendet, für die Berechnung der `positions` Array; Bereich acht Werten von 0.75f bis 1. Die `radius` Wert für die Kontur zuweisen des Kreises verwendet wird. Der Wert 875 bedeutet, dass die Strichbreite 250 Pixel zwischen den Radius der 750 Pixel und den Radius der 1000 Pixel erweitert:

[![Rainbow Bogen Farbverlauf](circular-gradients-images/RainbowArcGradient.png "Rainbow Bogen Farbverlauf")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

Wenn Sie mit diesem Verlauf die gesamte Canvas ausgefüllt haben, sehen Sie sich, dass es innerhalb des inneren Radius Rot ist. Grund hierfür ist die `positions` Array mit 0 beginnt nicht. Die erste Farbe wird für die Offsets von 0 bis das erste Arraywert verwendet. Der Gradient ist auch über den Radius des äußeren Rot. Dies ist das Ergebnis der `Clamp` nebeneinander. Da der Gradient für die Kontur zuweisen eine starke Linie verwendet wird, sind diese roten Bereiche nicht sichtbar.

## <a name="radial-gradients-for-masking"></a>Radiale Farbverläufe für die Maskierung

Wie linearen Farbverläufen können radiale Farbverläufen transparente oder teilweise transparente Farben integrieren. Diese Funktion ist nützlich für einen Prozess namens _maskiert_, der Teil eines Betriebssystemabbilds auf einem anderen Teil des Bilds fortlaufenden Hauptinhalts verborgen.

Die **radialen Farbverlauf Maske** Seite zeigt ein Beispiel. Das Programm lädt eine der Bitmaps Ressource. Die `CENTER` und `RADIUS` Felder, die durch eine Untersuchung der Bitmap bestimmt wurden, und verweisen auf einen Bereich, der hervorgehoben werden soll. Die `PaintSurface` Handler beginnt mit der Berechnung eines Rechtecks um die Bitmap anzuzeigen, und klicken Sie dann in dieses Rechteck angezeigt:

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

Nach dem Zeichnen der Bitmaps, einfacher Code konvertiert `CENTER` und `RADIUS` zu `center` und `radius`, beziehen sich auf den markierten Bereich, in der Bitmap, die skaliert, und für die Anzeige verschoben wurde. Diese Werte werden verwendet, zum Erstellen von eines radialen Farbverlaufs mit diesem Mittelpunkt und dem Radius. Die zwei Farben beginnen bei in der Mitte und für die ersten 60 % des Radius transparent. Der Farbverlauf wird dann weiß:

[![Radialen Farbverlauf Maske](circular-gradients-images/RadialGradientMask.png "radialen Farbverlauf-Maske")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

Dieser Ansatz ist nicht die beste Möglichkeit, eine Bitmap zu maskieren. Das Problem ist, dass die Maske größtenteils eine Farbe Weiß, das den Hintergrund des Zeichenbereichs entsprechend ausgewählt wurde. Wenn der Hintergrund einige andere Farbe wird &mdash; oder vielleicht einen Farbverlauf selbst &mdash; es nicht überein. Ein besserer Ansatz zur Maskierung wird angezeigt, in diesem Artikel [SkiaSharp Porter-Duff Füllmethoden](../blend-modes/porter-duff.md).

## <a name="radial-gradients-for-specular-highlights"></a>Radiale Farbverläufe für Glanzlichter

Wenn Sie ein Licht eine abgerundete Oberfläche eintritt, es Licht in viele Richtungen widerspiegelt, aber einige der Lichtquelle direkt in des anzeigenden Benutzers Auge springt. Dadurch wird die Darstellung einer Fuzzysuche weiße Fläche häufig auf der Oberfläche namens erstellt eine _Lichtreflexe_.

Dreidimensionale Grafiken führen Glanzlichter häufig von den Algorithmen verwendet, um die light-Pfade und Schattierung zu bestimmen. In zweidimensionalen Grafiken werden Glanzlichter manchmal hinzugefügt, um die Darstellung einer 3D-Oberfläche vorzuschlagen. Eine Lichtreflexe kann einen flachen roten Kreis in eine rote Kugel umwandeln.

Die **radiale glänzende markieren** Seite einen radialen Farbverlauf verwendet, genau dies. Die `PaintSurface` Handler ab, durch das Berechnen von eines Radius für den Kreis und die beiden `SKPoint` Werte &mdash; eine `center` und `offCenter` , die in der Mitte zwischen dem Mittelpunkt und dem oberen linken Rand des Kreises ist:

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

Die `CreateRadialGradient` Aufruf erstellt einen Farbverlauf an, die beginnt, die `offCenter` mit weißen und endet mit roten in einem Abstand von der Hälfte der Radius. Hier sehen Sie, wie es aussieht:

[![Radiale Lichtreflexe](circular-gradients-images/RadialSpecularHighlight.png "radiale Lichtreflexe")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

Wenn Sie diesen Farbverlauf genauer betrachten, könnten Sie, dass er fehlerhaft ist. Farbverlaufs basiert auf einem bestimmten Zeitpunkt aus, und Sie möchten möglicherweise, er wäre etwas weniger symmetrische entsprechend die abgerundete Oberfläche. In diesem Fall möglicherweise bevorzugen Sie im Abschnitt unten Lichtreflexe [ **konische Gradienten für Glanzlichter**](#conical-gradients-for-specular-highlights).

## <a name="the-sweep-gradient"></a>Die geleerte Farbverlauf

Die [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[])) Methode hat die einfachste Syntax aller Farbverlauf-Erstellung Methoden:

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Es ist nur in einem Rechenzentrum, ein Array von Farben und die Positionen der Farbe. Den Farbverlauf auf der rechten Seite des Mittelpunkts beginnt und zieht 360 Grad im Uhrzeigersinn um den Mittelpunkt. Beachten Sie, dass es keine `SKShaderTileMode` Parameter.

Ein [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix)) Überladung mit einem Matrix-transformieren-Parameter ist auch verfügbar. Sie können einen drehungsübergang, ändern Sie den Anfangspunkt, an den Farbverlauf anwenden. Sie können auch eine Skalierungstransformation, um die Richtung aus im Uhrzeigersinn um gegen den Uhrzeigersinn ändern anwenden.

Die **Sweep Farbverlauf** Seite verwendet Sweep Farbverläufe, um einen Kreis mit einem Strichbreite von 50 Pixeln Farbe:

[![Beim Aufräumen bearbeitete Farbverlauf](circular-gradients-images/SweepGradient.png "Sweep Farbverlauf")](circular-gradients-images/SweepGradient-Large.png#lightbox)

Die `SweepGradientPage` -Klasse definiert ein Array von acht Farben mit verschiedenen Hue-Werten. Beachten Sie, dass das Array beginnt und endet mit roten (eine Hue-Wert von 0 oder 360), die ganz rechts in den Screenshots angezeigt wird:

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

Das Programm implementiert auch eine `TapGestureRecognizer` , ermöglicht Code am Ende der `PaintSurface` Handler. Dieser Code verwendet den gleichen Farbverlauf auf den Zeichenbereich ausfüllen:

[![Beim Aufräumen bearbeitete Farbverlauf vollständige](circular-gradients-images/SweepGradientFull.png "Sweep Farbverlauf voll")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

Diese Screenshots zeigen, dass das graduelle Füllungen unabhängig von dem Bereich von ihm gefärbt ist. Wenn der Farbverlauf nicht beginnen und mit der gleichen Farbe enden, wird eine Diskontinuität auf der rechten Seite des Mittelpunkts vorhanden sein.

## <a name="the-two-point-conical-gradient"></a>Zwei-zackiger konische Farbverlaufs

Die [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) Methode hat die folgende Syntax:

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

Die Parameter, die mit Mittelpunkten und Radien für die beiden Kreise, um genannte beginnen die _starten_ Kreis und _End_ Kreis. Die verbleibenden drei Parameter entsprechen denen für `CreateLinearGradient` und `CreateRadialGradient`. Ein [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung umfasst ein Matrixtransformation.

Des Farbverlaufs an den Kreis Start beginnt und endet mit der End-Kreis. Die `SKShaderTileMode` Parameter steuert, was geschieht, über die beiden Kreise. Zwei-zackiger konische Farbverlaufs ist nur Farbverlaufs, der nicht vollständig über einen Bereich ausfüllt. Wenn die beiden Kreise identischem Radius verfügen, ist der Gradient auf ein Rechteck mit einer Breite, die der Durchmesser der Kreise identisch ist beschränkt. Wenn die beiden Kreise verschiedene Radien verfügen, bildet der Gradient ein Kegels.

Ist es wahrscheinlich ist, sollten Sie zum Experimentieren mit zwei-zackiger konische Verlaufs, also die **konische Farbverlauf** Seite leitet sich von `InteractivePage` um zwei Berührungspunkte, die verschoben werden, für die beiden Radien Kreis zu ermöglichen:

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

Die Code-Behind-Datei definiert die beiden `TouchPoint` Objekte mit festen Radien der 50 und 100:

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

Die `colors` Array ist rot, Grün und Blau. Der Code unten auf der `PaintSurface` Handler zeichnet die zwei Berührungspunkte als Schwarz Kreise, damit sie den Farbverlauf antidebugverhalten nicht.

Beachten Sie, dass `DrawRect` Aufruf des Farbverlaufs verwendet, um Farbe, die den gesamten Zeichenbereich. Im Allgemeinen bleibt ein Großteil der Leinwand jedoch nicht durch den Verlauf farbigen. Hier ist das Programm zeigt drei mögliche Konfigurationen:

[![Konische Farbverlauf](circular-gradients-images/ConicalGradient.png "konische Farbverlauf")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

IOS-Bildschirms auf der linken Seite zeigt die Auswirkung der `SKShaderTileMode` -Einstellung `Clamp`. Der Gradient beginnt mit Rot in den Rand des Kreises kleinere, das Gegenstück der Seite auf den zweiten Kreis am nächsten ist. Die `Clamp` Wert bewirkt auch, dass Rot, um den Punkt der Lichtkegel zu öffnen. Der Gradient endet mit blauen am äußeren Rand des Kreises größer, die auf den ersten Kreis am nächsten ist, aber weiterhin mit Blau innerhalb dieser Kreises und darüber hinaus.

Die Android-Bildschirm ähnelt, aber mit einer `SKShaderTileMode` von `Repeat`. Jetzt ist es klarer, dass den Farbverlauf innerhalb des Kreises erste beginnt und endet, die außerhalb der zweiten Kreis. Die `Repeat` Einstellung bewirkt, dass der Farbverlauf mit roten innerhalb der größeren Kreis wiederholen.

Der UWP-Bildschirm zeigt an, was geschieht, wenn es sich bei der kleinere Kreis vollständig innerhalb der größeren Kreis bewegt wird. Der Gradient beendet, der einen Kegel und stattdessen füllt den gesamten Bereich. Die Auswirkungen sind vergleichbar mit der radialen Farbverlauf, aber es asymmetrisch ist, wenn es sich bei der kleinere Kreis nicht genau innerhalb der größeren Kreis zentriert ist.

Sie können den praktischen Nutzen des Farbverlaufs entspricht bei einem einzelnen Kreis in einer anderen geschachtelt ist, aber es empfiehlt sich für eine Lichtreflexe bezweifle.

## <a name="conical-gradients-for-specular-highlights"></a>Konische Gradienten für Glanzlichter

In diesem Artikel wurde erläutert, wie einen radialen Farbverlauf zu verwenden, um eine Lichtreflexe zu erstellen. Sie können auch zwei Punkt konische Farbverlaufs zu diesem Zweck verwenden, und möglicherweise bevorzugen Sie aussehen:

[![Konische Lichtreflexe](circular-gradients-images/ConicalSpecularHighlight.png "konische Lichtreflexe")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

Die asymmetrische Darstellung schlägt besser vor der abgerundeten Oberfläche des Objekts. 

Der Code zum Zeichnen in der **konische glänzende markieren** Seite ist identisch mit der **radiale glänzende markieren** Seite mit Ausnahme der Shader:

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

Die beiden Kreise haben, wird von `offCenter` und `center`. Der Kreis zentriert `center` bezieht sich auf einen Radius, der die gesamte Kugel, aber den Kreis, der zentriert umfasst `offCenter` hat einen Radius von nur einem Pixel. Der Gradient ist effektiv an diesem Punkt beginnt und endet am Rand des Balls.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
