---
Title: "die skiasharp-kreisförmige Farbverläufe" Beschreibung: "Informationen zu den verschiedenen Typen von Farbverläufen auf der Grundlage von Kreisen."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 400ae23a-6a0b-4fa8-bd6b-de4146b04732 Author: davidbritch ms. Author: dabritch ms. Date: 08/23/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-skiasharp-circular-gradients"></a>Die kreisförmigen Farbverläufe skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die- [`SKShader`](xref:SkiaSharp.SKShader) Klasse definiert statische Methoden zum Erstellen von vier verschiedenen Typen von Farbverläufen. Der [**lineare skiasharp-Verlaufs**](linear-gradient.md) Artikel erläutert die- [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) Methode. In diesem Artikel werden die anderen drei Typen von Farbverläufen behandelt, die alle auf Kreise basieren.

Die- [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*) Methode erstellt einen Farbverlauf, der von der Mitte eines Kreises ausgeht:

![Beispiel Radialer Farbverlauf](circular-gradients-images/RadialGradientSample.png)

Die- [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*) Methode erstellt einen Farbverlauf, der um den Mittelpunkt eines Kreises herum zieht:

![Beispiel für Sweep Gradient](circular-gradients-images/SweepGradientSample.png)

Der dritte Typ des Farbverlaufs ist ziemlich ungewöhnlich. Sie wird als zwei-Punkt-kegelförmiger Farbverlauf bezeichnet und wird durch die- [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) Methode definiert. Der Farbverlauf reicht von einem Kreis zu einem anderen:

![Beispiel für ein "Kegel](circular-gradients-images/ConicalGradientSample.png)

Wenn es sich bei den beiden Kreisen um unterschiedliche Größen handelt, hat der Farbverlauf die Form eines Kegel.

In diesem Artikel werden diese Farbverläufe ausführlicher erläutert.

## <a name="the-radial-gradient"></a>Der radiale Farbverlauf

Die- [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) Methode weist die folgende Syntax auf:

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

Eine-Überladung [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) enthält auch einen Transform Matrix-Parameter.

Die ersten beiden Argumente geben den Mittelpunkt eines Kreises und einen Radius an. Der Farbverlauf beginnt an diesem Mittelpunkt und wird für Pixel nach außen erweitert `radius` . Darüber hinaus `radius` hängt von dem [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) Argument ab. Der `colors` -Parameter ist ein Array aus mindestens zwei Farben (genau wie in den linearen Farbverlaufs Methoden) und `colorPos` ist ein Array von ganzen Zahlen im Bereich von 0 bis 1. Diese ganzzahligen Werte geben die relativen Positionen der Farben entlang dieser `radius` Zeile an. Sie können dieses Argument auf festlegen, `null` um die Farben gleichmäßig zu leer zu lassen.

Wenn Sie verwenden, `CreateRadialGradient` um einen Kreis zu füllen, können Sie den Mittelpunkt des Farbverlaufs auf den Mittelpunkt des Kreises und den Radius des Farbverlaufs auf den Radius des Kreises festlegen. In diesem Fall hat das- `SKShaderTileMode` Argument keine Auswirkung auf das Rendering des Farbverlaufs. Wenn jedoch der Bereich, der durch den Farbverlauf gefüllt ist, größer als der durch den Farbverlauf definierte Kreis ist, `SKShaderTileMode` hat das Argument eine Tiefe Auswirkung darauf, was außerhalb des Kreises passiert.

Die Auswirkung von `SKShaderMode` wird im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel auf der **radialen Farbverlaufs** Seite veranschaulicht. Die XAML-Datei für diese Seite instanziiert ein-Element, mit dem `Picker` Sie eines der drei Member der- `SKShaderTileMode` Enumeration auswählen können:

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

Die Code-Behind-Datei färbt den gesamten Zeichenbereich mit einem radialen Farbverlauf ab. Die Mitte des Farbverlaufs wird auf den Mittelpunkt des Zeichen Bereichs festgelegt, und der RADIUS wird auf 100 Pixel festgelegt. Der Farbverlauf besteht aus nur zwei Farben: schwarz und weiß:

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

Mit diesem Code wird ein Farbverlauf im Mittelpunkt mit schwarz und in den weißen 100 Pixeln aus dem Mittelpunkt erstellt. Was über diesen RADIUS hinausgeht, hängt von dem `SKShaderTileMode` Argument ab:

[![Radialer Farbverlauf](circular-gradients-images/RadialGradient.png "Radialer Farbverlauf")](circular-gradients-images/RadialGradient-Large.png#lightbox)

In allen drei Fällen füllt der Farbverlauf den Zeichenbereich. Auf dem IOS-Bildschirm auf der linken Seite wird der Farbverlauf über den RADIUS hinaus mit der letzten Farbe, die weiß ist, fortgesetzt. Das ist das Ergebnis von `SKShaderTileMode.Clamp` . Der Android-Bildschirm zeigt die Auswirkung von `SKShaderTileMode.Repeat` : bei 100 Pixel von der Mitte beginnt der Farbverlauf erneut mit der ersten Farbe, die schwarz ist. Der Farbverlauf wird alle 100 Pixel des RADIUS wiederholt. 

Der Bildschirm universelle Windows-Plattform auf der rechten Seite zeigt `SKShaderTileMode.Mirror` , wie die Farbverläufe in Alternative Richtungen bewirkt. Der erste Farbverlauf ist von schwarz an der Mitte (weiß) mit einem Radius von 100 Pixeln. Das nächste ist weiß vom 100-Pixel-Radius zu schwarz bei einem Radius von 200 Pixel, und der nächste Farbverlauf wird wieder umgekehrt.

Sie können in einem radialen Farbverlauf mehr als zwei Farben verwenden. Mit dem Beispiel für den **Regenbogen Bogen Verlauf** wird ein Array mit acht Farben erstellt, das den Farben des Regenbogen Werts entspricht und mit rot endet, sowie ein Array von acht Positions Werten:

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

Angenommen, das minimale Breite und die Höhe der Canvas beträgt 1000, was bedeutet, dass der `rainbowWidth` Wert 250 ist. Der `outerRadius` -Wert und der- `innerRadius` Wert werden auf 1000 bzw. 750 festgelegt. Diese Werte werden zum Berechnen des `positions` Arrays verwendet; die acht Werte liegen zwischen 0,75 f und 1. Der- `radius` Wert wird zum Durchsuchen des Kreises verwendet. Der Wert 875 bedeutet, dass die Breite des 250-Pixel-Strichs zwischen dem Radius von 750 Pixel und dem Radius von 1000 Pixel liegt:

[![Verlauf des Regenbogen Bogens](circular-gradients-images/RainbowArcGradient.png "Verlauf des Regenbogen Bogens")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

Wenn Sie den gesamten Zeichenbereich mit diesem Farbverlauf ausgefüllt haben, sehen Sie, dass er im inneren Radius rot ist. Dies liegt daran, dass das `positions` Array nicht mit 0 beginnt. Die erste Farbe wird für Offsets von 0 bis zum ersten Array Wert verwendet. Der Farbverlauf wird auch über den äußeren Radius hinaus rot angezeigt. Das ist das Ergebnis des `Clamp` Kachel Modus. Da der Farbverlauf zum Durchsuchen einer dicken Linie verwendet wird, sind diese roten Bereiche nicht sichtbar.

## <a name="radial-gradients-for-masking"></a>Radiale Farbverläufe für Maskierung

Wie bei linearen Farbverläufen können radiale Farbverläufe transparente oder teilweise transparente Farben enthalten. Diese Funktion ist nützlich für einen Prozess namens _Maskierung_, der einen Teil eines Bilds verbirgt, um einen anderen Teil des Bilds hervorzuheben.

Die Seite **radiale Farbverlaufs Maske** zeigt ein Beispiel an. Das Programm lädt eine der Ressourcen Bitmaps. Die `CENTER` `RADIUS` Felder und wurden von einer Untersuchung der Bitmap bestimmt und verweisen auf einen Bereich, der hervorgehoben werden soll. Der `PaintSurface` Handler beginnt mit dem Berechnen eines Rechtecks, um die Bitmap anzuzeigen, und zeigt Sie dann in diesem Rechteck an:

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

Nach dem Zeichnen der Bitmap werden einige einfache Codes `CENTER` und `RADIUS` in `center` und konvertiert `radius` , die auf den markierten Bereich in der Bitmap verweisen, der zum Anzeigen skaliert und verschoben wurde. Diese Werte werden verwendet, um einen radialen Farbverlauf mit diesem Mittelpunkt und RADIUS zu erstellen. Die beiden Farben beginnen bei transparent in der Mitte und für die ersten 60% des RADIUS. Der Farbverlauf wird dann weiß angezeigt:

[![Radiale Verlaufs Maske](circular-gradients-images/RadialGradientMask.png "Radiale Verlaufs Maske")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

Diese Vorgehensweise ist nicht die beste Methode, um eine Bitmap zu maskieren. Das Problem besteht darin, dass die Maske größtenteils eine Farbe weiß hat, die für den Hintergrund der Canvas ausgewählt wurde. Wenn der Hintergrund eine andere Farbe &mdash; oder vielleicht ein Farbverlauf ist, &mdash; entspricht er nicht. Ein besserer Ansatz für die Maskierung ist im Artikel [skiasharp Porter-Duff Blend-Modi](../blend-modes/porter-duff.md)dargestellt.

## <a name="radial-gradients-for-specular-highlights"></a>Radiale Farbverläufe für Glanzlichter

Wenn ein Licht auf eine abgerundete Oberfläche trifft, spiegelt es das Licht in vielerlei Richtung wider, aber einige der hellen springen direkt in den Blick des Viewers. Dies führt häufig dazu, dass ein fuzzyweißer Bereich auf der Oberfläche, _der als Glanz_Lichter bezeichnet wird, dargestellt wird.

In dreidimensionalen Grafiken resultieren Glanzlichter häufig aus den Algorithmen, die zur Ermittlung von hellen Pfaden und Schattierung verwendet werden. In zweidimensionalen Grafiken werden manchmal Glanzlichter hinzugefügt, um die Darstellung einer 3D-Oberfläche vorzuschlagen. Eine Glanz Markierung kann einen flachen roten Kreis in eine Runde rote Kugel umwandeln.

Die **radiale Glanz hervor** gehobene Seite verwendet einen radialen Farbverlauf, um genau das zu erledigen. Die `PaintSurface` Handlerfunktionen durch Berechnen eines RADIUS für den Kreis und zwei `SKPoint` Werte &mdash; a `center` und eine `offCenter` , die sich in der Mitte zwischen dem Mittelpunkt und dem oberen linken Rand des Kreises befindet:

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

Der-Befehl `CreateRadialGradient` erstellt einen Farbverlauf, der an diesem `offCenter` Punkt mit weiß beginnt und mit rot in einem Abstand von der Hälfte des RADIUS endet. Hier sehen Sie, wie es aussieht:

[![Radiale Glanz Markierung](circular-gradients-images/RadialSpecularHighlight.png "Radiale Glanz Markierung")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

Wenn Sie diesen Farbverlauf genau betrachten, können Sie sich entscheiden, dass er fehlerhaft ist. Der Farbverlauf wird um einen bestimmten Punkt zentriert, und Sie möchten vielleicht etwas weniger symmetrisch sein, um die abgerundete Oberfläche widerzuspiegeln. In diesem Fall bevorzugen Sie möglicherweise die unten gezeigte Glanz Markierung im Abschnitt " [**kegelförmige Farbverläufe" für Glanzlichter**](#conical-gradients-for-specular-highlights).

## <a name="the-sweep-gradient"></a>Der Sweep-Farbverlauf

Die [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[])) -Methode verfügt über die einfachste Syntax aller Methoden zur Farbverlauf Erstellung:

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Es ist nur ein Mittelpunkt, ein Array mit Farben und die Farb Positionen. Der Farbverlauf beginnt auf der rechten Seite des Mittelpunkts und durchlaufen 360 Grad im Uhrzeigersinn um den Mittelpunkt. Beachten Sie, dass es keinen `SKShaderTileMode` Parameter gibt.

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix))Es ist auch eine Überladung mit einem Matrix Transformationsparameter verfügbar. Sie können eine Rotations Transformation auf den Farbverlauf anwenden, um den Startpunkt zu ändern. Sie können auch eine Skalierungs Transformation anwenden, um die Richtung von Uhrzeigersinn in gegen den Uhrzeigersinn zu ändern.

Die Seite " **Sweep Gradient** " verwendet einen Sweep-Farbverlauf, um einen Kreis mit einer Strichbreite von 50 Pixeln zu färben:

[![Sweep-Farbverlauf](circular-gradients-images/SweepGradient.png "Sweep-Farbverlauf")](circular-gradients-images/SweepGradient-Large.png#lightbox)

Die- `SweepGradientPage` Klasse definiert ein Array aus acht Farben mit unterschiedlichen Farbton Werten. Beachten Sie, dass das Array beginnt und mit rot (einem Hue-Wert von 0 oder 360) endet, der in den Screenshots ganz rechts angezeigt wird:

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

Das Programm implementiert auch einen `TapGestureRecognizer` , der Code am Ende des `PaintSurface` Handlers ermöglicht. Dieser Code verwendet denselben Farbverlauf, um den Zeichenbereich auszufüllen:

[![Sweep-Farbverlauf voll](circular-gradients-images/SweepGradientFull.png "Sweep-Farbverlauf voll")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

Diese Screenshots veranschaulichen, dass der Farbverlauf den von ihm farbigen Bereich füllt. Wenn der Farbverlauf nicht mit derselben Farbe beginnt und endet, gibt es eine Diskontinuität rechts neben dem Mittelpunkt.

## <a name="the-two-point-conical-gradient"></a>Der zweistufige kegelförmige Farbverlauf

Die- [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) Methode weist die folgende Syntax auf:

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

Die Parameter beginnen mit Mittelpunkten und Radien für zwei Kreise, die als _Start_ Kreis und _Endkreis_ bezeichnet werden. Die verbleibenden drei Parameter sind identisch mit denen für `CreateLinearGradient` und `CreateRadialGradient` . Eine Überladung [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) schließt eine Matrix Transformation ein.

Der Farbverlauf beginnt am startkreis und endet am Endkreis. Der- `SKShaderTileMode` Parameter bestimmt, was über die beiden Kreise hinausgeht. Der zweistufige kegelförmige Farbverlauf ist der einzige Farbverlauf, der einen Bereich nicht vollständig ausfüllt. Wenn die beiden Kreise denselben RADIUS aufweisen, ist der Farbverlauf auf ein Rechteck mit einer Breite beschränkt, die mit dem Durchmesser der Kreise identisch ist. Wenn die beiden Kreise über unterschiedliche Radii verfügen, bildet der Farbverlauf einen Kegel.

Es ist wahrscheinlich, dass Sie mit dem zweistufigen kegelförmigen Farbverlauf experimentieren möchten, sodass die Seite " **kegelförmige Farbverlauf** " von abgeleitet ist, damit `InteractivePage` zwei Berührungspunkte für die beiden kreisförmigen Radii bewegt werden können:

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

Die Code-Behind-Datei definiert die beiden `TouchPoint` Objekte mit einem festgelegtem Radien von 50 und 100:

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

Beachten Sie, dass der-Befehl den Farb `DrawRect` Verlauf verwendet, um den gesamten Zeichenbereich zu Im allgemeinen Fall bleibt jedoch ein Großteil des Canvas durch den Farbverlauf unfarbig. Hier ist das Programm, das drei mögliche Konfigurationen anzeigt:

[![Kegelförmiger Farbverlauf](circular-gradients-images/ConicalGradient.png "Kegelförmiger Farbverlauf")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

Auf der linken Seite des IOS-Bildschirms wird die Auswirkung der `SKShaderTileMode` Einstellung von angezeigt `Clamp` . Der Farbverlauf beginnt mit rot innerhalb des Randes kleiner Kreises, der der Seite am nächsten liegt, die dem zweiten Kreis am nächsten ist. Der `Clamp` Wert bewirkt auch, dass Rot an den Punkt des Kegel weiter geht. Der Farbverlauf endet mit blau am äußeren Rand des größeren Kreises, der dem ersten Kreis am nächsten ist, aber mit blau innerhalb dieses Kreises und darüber hinaus fortgesetzt wird.

Der Android-Bildschirm ist ähnlich, aber mit `SKShaderTileMode` der von `Repeat` . Nun ist es klarer, dass der Farbverlauf innerhalb des ersten Kreises beginnt und außerhalb des zweiten Kreises endet. Die- `Repeat` Einstellung bewirkt, dass der Farbverlauf erneut mit rot innerhalb des größeren Kreises wiederholt wird.

Der UWP-Bildschirm zeigt, was geschieht, wenn der kleinere Kreis vollständig innerhalb des größeren Kreises bewegt wird. Der Farbverlauf hält als Kegel an und füllt stattdessen den gesamten Bereich. Der Effekt ähnelt dem radialen Farbverlauf, ist aber asymmetrisch, wenn der kleinere Kreis nicht genau innerhalb des größeren Kreises zentriert ist.

Möglicherweise Zweifeln Sie die praktische Nützlichkeit des Farbverlaufs, wenn ein Kreis in einem anderen Kreis eingebettet ist, aber er eignet sich ideal für eine Glanz Hervorhebung.

## <a name="conical-gradients-for-specular-highlights"></a>Kegelförmige Farbverläufe für Glanzlichter

Weiter oben in diesem Artikel haben Sie erfahren, wie Sie mit einem radialen Farbverlauf eine Glanz Markierung erstellen. Für diesen Zweck können Sie auch den zweistufigen Kegel-Farbverlauf verwenden, und Sie können die Vorgehensweise bevorzugen:

[![Kegelförmige Hervorhebung](circular-gradients-images/ConicalSpecularHighlight.png "Kegelförmige Hervorhebung")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

Die asymmetrische Darstellung deutet besser auf die abgerundete Oberfläche des Objekts hin. 

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

Die beiden Kreise haben die Zentren von `offCenter` und `center` . Der mit zentrierte Kreis `center` ist einem RADIUS zugeordnet, der die gesamte Kugel umfasst, aber der Kreis, auf dem zentriert ist, `offCenter` verfügt über einen Radius von nur einem Pixel. Der Farbverlauf beginnt an diesem Punkt und endet am Rand der Kugel.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
