---
Title: "Porter-Duff-Blend-Modi" Description: "verwenden Sie die" Porter-Duff Blend "-Modi, um Szenen basierend auf Quell-und Ziel Images zu verfassen.
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 57f172 F8-ba03-43ec-A215-ed6b78696bb5 Author: davidbritch ms. Author: dabritch ms. Date: 08/23/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="porter-duff-blend-modes"></a>Porter-Duff-Blend-Modi

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die Blend-Modi "Porter-Duff" werden nach Thomas Porter und Tom Duff benannt, die beim Arbeiten mit Lucasfilm eine Algebra von Zusammensetzung entwickelt haben. Der Artikel zum Zusammenstellen [_digitaler Images_](https://graphics.pixar.com/library/Compositing/paper.pdf) wurde im Juli 1984-Problem der _Computer Grafiken_veröffentlicht, Seiten 253 bis 259. Diese Blend-Modi sind für die Zusammensetzung von Bedeutung, bei der verschiedene Bilder in einer zusammengesetzten Szene zusammengesetzt werden:

![Beispiel für Porter-Duff](porter-duff-images/PorterDuffSample.png "Beispiel für Porter-Duff")

## <a name="porter-duff-concepts"></a>Porter-Duff-Konzepte

Angenommen, ein durch Such Rechteck füllt die linke und die obere zwei Drittel der Anzeige Oberfläche aus:

![Porter-Duff-Ziel](porter-duff-images/PorterDuffDst.png "Porter-Duff-Ziel")

Dieser Bereich wird als _Ziel_ oder manchmal als _Hintergrund_ _oder Hintergrund bezeichnet._

Sie möchten das folgende Rechteck zeichnen, das dieselbe Größe wie das Ziel hat. Das Rechteck ist transparent, außer bei einem blauen Bereich, der die Rechte und die untersten zwei Drittel einnimmt:

![Porter-Duff-Quelle](porter-duff-images/PorterDuffSrc.png "Porter-Duff-Quelle")

Dies wird als _Quelle_ oder manchmal als _Vordergrund_bezeichnet.

Wenn Sie die Quelle auf dem Ziel anzeigen, gehen Sie wie folgt vor:

![Porter-Duff-Quelle über](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff-Quelle über")

Durch die transparenten Pixel der Quelle kann der Hintergrund durch angezeigt werden, während die Blusen Quell Pixel den Hintergrund verdecken. Dies ist der Normalfall, und in skiasharp wird als bezeichnet `SKBlendMode.SrcOver` . Dieser Wert ist die Standardeinstellung der- `BlendMode` Eigenschaft, wenn ein `SKPaint` Objekt zum ersten Mal instanziiert wird.

Es ist jedoch möglich, einen anderen Blend-Modus für einen anderen Effekt anzugeben. Wenn Sie angeben `SKBlendMode.DstOver` , wird das Ziel in dem Bereich, in dem sich die Quelle und das Ziel schneiden, anstelle der Quelle angezeigt:

![Porter-Duff-Ziel über](porter-duff-images/PorterDuffDstOver.png "Porter-Duff-Ziel über")

Im `SKBlendMode.DstIn` Blend-Modus wird nur der Bereich angezeigt, in dem sich das Ziel und die Quelle mithilfe der Zielfarbe überschneiden:

![Porter-Duff-Ziel in](porter-duff-images/PorterDuffDstIn.png "Porter-Duff-Ziel in")

Der Blend-Modus von `SKBlendMode.Xor` (exklusiv oder) bewirkt, dass nichts angezeigt wird, wenn sich die beiden Bereiche überschneiden:

![Porter-Duff exklusiv oder](porter-duff-images/PorterDuffXor.png "Porter-Duff exklusiv oder")

Der farbige Ziel-und Quell Rechtecke unterteilt die Anzeige Oberfläche effektiv in vier eindeutige Bereiche, die auf verschiedene Weise gefärbt werden können, was dem vorhanden sein der Ziel-und Quell Rechtecke entspricht:

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter-Duff")

Die Rechte oben rechts und Links unten sind immer leer, da sowohl das Ziel als auch die Quelle in diesen Bereichen transparent sind. Die Ziel Farbe nimmt den oberen linken Bereich ein, sodass der Bereich entweder mit der Zielfarbe oder überhaupt nicht angezeigt werden kann. Entsprechend nimmt die Quellfarbe den unteren rechten Bereich an, sodass der Bereich mit der Quellfarbe oder überhaupt nicht gefärbt werden kann. Die Schnittmenge des Ziels und der Quelle in der Mitte kann mit der Zielfarbe, der Quellfarbe oder gar nicht gefärbt werden.

Die Gesamtanzahl der Kombinationen beträgt 2 (für die linke obere Seite) 2 (für die untere rechte), 3 (für Mittel) oder 12. Dabei handelt es sich um die 12 grundlegenden "Porter-Duff"-Kompositions Modi.

Am Ende der Zusammensetzung _digitaler Images_ (Seite 256) fügen Porter und Duff einen 13. Modus mit dem Namen _plus_ hinzu (entsprechend dem skiasharp `SKBlendMode.Plus` -Member und dem _aushelleren_ Modus für W3C), der nicht mit dem W3C- _Lighten_ -Modus verwechselt werden muss. `Plus`In diesem Modus werden die Ziel-und Quell Farben hinzugefügt. Dies ist ein Prozess, der in Kürze ausführlicher beschrieben wird.

Skia fügt einen 14. Modus mit dem Namen hinzu `Modulate` , der sehr ähnlich ist, `Plus` mit dem Unterschied, dass die Ziel-und Quell Farben multipliziert werden. Sie kann als zusätzlicher Porter-Duff-Blend-Modus behandelt werden.

Hier sind die 14 "Porter-Duff"-Modi, wie in skiasharp definiert. Die Tabelle zeigt, wie Sie die einzelnen drei nicht leeren Bereiche in der obigen Abbildung färben:

| Mode       | Destination | Kreuz | `Source` |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | `Source`       | X      |
| `Dst`      | X           | Destination  |        |
| `SrcOver`  | X           | `Source`       | X      |
| `DstOver`  | X           | Destination  | X      |
| `SrcIn`    |             | `Source`       |        |
| `DstIn`    |             | Destination  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | `Source`       |        |
| `DstATop`  |             | Destination  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | SUM          | X      |
| `Modulate` |             | Produkt      |        | 

Diese Blend-Modi sind symmetrisch. Quelle und Ziel können ausgetauscht werden, und alle Modi sind immer noch verfügbar.

Die Benennungs Konvention der Modi folgt einigen einfachen Regeln:

- **Src** oder **DST** allein bedeutet, dass nur die Quell-oder Ziel Pixel sichtbar sind.
- Das **over** -Suffix gibt an, was in der Schnittmenge sichtbar ist. Entweder die Quelle oder das Ziel wird "Over" gezeichnet.
- Das **in** -Suffix bedeutet, dass nur die Schnittmenge gefärbt ist. Die Ausgabe ist auf den Teil der Quelle oder des Ziels beschränkt, der "in" der anderen ist.
- Das **out** -Suffix bedeutet, dass die Schnittmenge nicht farbig ist. Bei der Ausgabe handelt es sich nur um den Teil der Quelle oder des Ziels, der aus der Schnittmenge "ausgehend" ist.
- Das on **-Suffix ist die** Union **von** ein **-und**ausgehend. Sie enthält den Bereich, in dem die Quelle oder das Ziel "oberhalb" der anderen Seite ist.

Beachten Sie den Unterschied zu den `Plus` `Modulate` Modi und. Diese Modi führen eine andere Art der Berechnung für die Quell-und Ziel Pixel aus. Sie werden in Kürze ausführlicher beschrieben.

Auf der Raster Seite " **Porter-Duff** " werden alle 14 Modi auf einem Bildschirm in Form eines Rasters angezeigt. Jeder Modus ist eine separate Instanz von `SKCanvasView` . Aus diesem Grund wird eine Klasse vom Namen abgeleitet `SKCanvasView` `PorterDuffCanvasView` . Der statische Konstruktor erstellt zwei Bitmaps derselben Größe, eine mit einem durch Such Rechteck in seinem linken oberen Bereich und eine andere mit einem Blusen Rechteck:

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    static SKBitmap srcBitmap, dstBitmap;

    static PorterDuffCanvasView()
    {
        dstBitmap = new SKBitmap(300, 300);
        srcBitmap = new SKBitmap(300, 300);

        using (SKPaint paint = new SKPaint())
        {
            using (SKCanvas canvas = new SKCanvas(dstBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0xC0, 0x80, 0x00);
                canvas.DrawRect(new SKRect(0, 0, 200, 200), paint);
            }
            using (SKCanvas canvas = new SKCanvas(srcBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0x00, 0x80, 0xC0);
                canvas.DrawRect(new SKRect(100, 100, 300, 300), paint);
            }
        }
    }
    ···
}
```

Der Instanzkonstruktor verfügt über einen Parameter vom Typ `SKBlendMode` . Dieser Parameter wird in einem Feld gespeichert. 

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    ···
    SKBlendMode blendMode;

    public PorterDuffCanvasView(SKBlendMode blendMode)
    {
        this.blendMode = blendMode;
    }

    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest square that fits
        float rectSize = Math.Min(info.Width, info.Height);
        float x = (info.Width - rectSize) / 2;
        float y = (info.Height - rectSize) / 2;
        SKRect rect = new SKRect(x, y, x + rectSize, y + rectSize);

        // Draw destination bitmap
        canvas.DrawBitmap(dstBitmap, rect);

        // Draw source bitmap
        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = blendMode;
            canvas.DrawBitmap(srcBitmap, rect, paint);
        }

        // Draw outline
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 2;
            rect.Inflate(-1, -1);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Durch die Überschreibung werden `OnPaintSurface` die beiden Bitmaps gezeichnet. Der erste wird normal gezeichnet:

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

Die zweite wird mit einem- `SKPaint` Objekt gezeichnet, bei dem die- `BlendMode` Eigenschaft auf das Konstruktorargument festgelegt wurde:

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

Der Rest der `OnPaintSurface` außer Kraft Setzung zeichnet ein Rechteck um die Bitmap, um deren Größe anzugeben.

Die- `PorterDuffGridPage` Klasse erstellt 14 Instanzen von `PorterDurffCanvasView` , eine für jedes Element des `blendModes` Arrays. Die Reihenfolge der Elemente `SKBlendModes` im Array unterscheidet sich geringfügig von der Tabelle, um ähnliche Modi nebeneinander zu positionieren. Die 14 Instanzen von `PorterDuffCanvasView` werden zusammen mit den Bezeichnungen in einer organisiert `Grid` :

```csharp
public class PorterDuffGridPage : ContentPage
{
    public PorterDuffGridPage()
    {
        Title = "Porter-Duff Grid";

        SKBlendMode[] blendModes =
        {
            SKBlendMode.Src, SKBlendMode.Dst, SKBlendMode.SrcOver, SKBlendMode.DstOver,
            SKBlendMode.SrcIn, SKBlendMode.DstIn, SKBlendMode.SrcOut, SKBlendMode.DstOut,
            SKBlendMode.SrcATop, SKBlendMode.DstATop, SKBlendMode.Xor, SKBlendMode.Plus,
            SKBlendMode.Modulate, SKBlendMode.Clear
        };

        Grid grid = new Grid
        {
            Margin = new Thickness(5)
        };

        for (int row = 0; row < 4; row++)
        {
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });
        }

        for (int col = 0; col < 3; col++)
        {
            grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
        }

        for (int i = 0; i < blendModes.Length; i++)
        {
            SKBlendMode blendMode = blendModes[i];
            int row = 2 * (i / 4);
            int col = i % 4;

            Label label = new Label
            {
                Text = blendMode.ToString(),
                HorizontalTextAlignment = TextAlignment.Center
            };
            Grid.SetRow(label, row);
            Grid.SetColumn(label, col);
            grid.Children.Add(label);

            PorterDuffCanvasView canvasView = new PorterDuffCanvasView(blendMode);

            Grid.SetRow(canvasView, row + 1);
            Grid.SetColumn(canvasView, col);
            grid.Children.Add(canvasView);
        }

        Content = grid;
    }
}
```

So sieht das Ergebnis aus:

[![Porter-Duff-Raster](porter-duff-images/PorterDuffGrid.png "Porter-Duff-Raster")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Sie sollten selbst überzeugen, dass Transparenz für die ordnungsgemäße Funktionsweise der Blend-Modi "Porter-Duff" entscheidend ist. Die- `PorterDuffCanvasView` Klasse enthält insgesamt drei Aufrufe der- `Canvas.Clear` Methode. Alle von Ihnen verwenden die Parameter lose-Methode, mit der alle Pixel transparent festgelegt werden:

```csharp
canvas.Clear();
```

Versuchen Sie, einen dieser Aufrufe so zu ändern, dass die Pixel auf undurchsichtiges weiß festgelegt sind:

```csharp
canvas.Clear(SKColors.White);
```

Nach dieser Änderung scheinen einige der Blend-Modi zu funktionieren, andere jedoch nicht. Wenn Sie den Hintergrund der Quell Bitmap auf weiß festlegen, `SrcOver` funktioniert der Modus nicht, da es keine transparenten Pixel in der Quell Bitmap gibt, damit das Ziel durch die Anzeige ermöglicht wird. Wenn Sie den Hintergrund der Ziel Bitmap oder der Canvas auf White festlegen, `DstOver` funktioniert nicht, da das Ziel keine transparenten Pixel hat.

Möglicherweise besteht eine Versuchung, die Bitmaps in der " **Porter-Duff"-Raster** Seite durch einfachere Aufrufe zu ersetzen `DrawRect` . Dies funktioniert für das Ziel Rechteck, jedoch nicht für das Quell Rechteck. Das Quell Rechteck muss mehr als nur den blau gefärbten Bereich umfassen. Das Quell Rechteck muss einen transparenten Bereich enthalten, der dem farbigen Bereich des Ziels entspricht. Nur dann funktionieren diese Blend-Modi.

## <a name="using-mattes-with-porter-duff"></a>Verwenden von mattes mit Porter-Duff

Die zusammengesetzte Seite " **Brick-Wall Zusammensetzung** " zeigt ein Beispiel für eine klassische Zusammensetzung-Aufgabe: ein Bild muss aus mehreren Teilen zusammengestellt werden, einschließlich einer Bitmap mit einem Hintergrund, der entfernt werden muss. Im folgenden finden Sie die **SeatedMonkey.jpg** Bitmap mit dem problematischen Hintergrund:

![Sitzender Affe](porter-duff-images/SeatedMonkey.jpg "Sitzender Affe")

Als Vorbereitung für die Zusammensetzung wurde eine entsprechende _Matte_ erstellt, bei der es sich um eine weitere Bitmap handelt, bei der es sich um eine andere Bitmap handelt. Diese Datei hat den Namen **SeatedMonkeyMatte.png** und befindet sich im Beispiel " [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) " unter den Ressourcen im Ordner " **Media** ":

![Sitzender affmatt](porter-duff-images/SeatedMonkeyMatte.png "Sitzender affmatt")

Dabei handelt es sich _nicht_ um eine sachkundig erstellte Matte. Im Idealfall sollte die Matte teilweise transparente Pixel um den Rand der schwarzen Pixel enthalten, und diese Matte ist nicht.

Die XAML-Datei für die zusammengesetzte **Brick-Wall-** Seite instanziiert einen `SKCanvasView` und einen `Button` , der den Benutzer durch den Prozess des Ersetzens des endgültigen Bilds führt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BrickWallCompositingPage"
             Title="Brick-Wall Compositing">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Show sitting monkey"
                HorizontalOptions="Center"
                Margin="0, 10"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei lädt die beiden benötigten Bitmaps und behandelt das- `Clicked` Ereignis von `Button` . Für jeden `Button` Klick wird das `step` Feld inkrementiert, und eine neue `Text` Eigenschaft wird für das festgelegt `Button` . Wenn `step` 5 erreicht, wird der Wert auf 0 (null) zurückgesetzt:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkeyMatte.png");

    int step = 0;

    public BrickWallCompositingPage ()
    {
        InitializeComponent ();
    }

    void OnButtonClicked(object sender, EventArgs args)
    {
        Button btn = (Button)sender;
        step = (step + 1) % 5;

        switch (step)
        {
            case 0: btn.Text = "Show sitting monkey"; break;
            case 1: btn.Text = "Draw matte with DstIn"; break;
            case 2: btn.Text = "Draw sidewalk with DstOver"; break;
            case 3: btn.Text = "Draw brick wall with DstOver"; break;
            case 4: btn.Text = "Reset"; break;
        }

        canvasView.InvalidateSurface();
    }
    
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        ···
    }
}
```

Wenn das Programm zum ersten Mal ausgeführt wird, ist nur Folgendes sichtbar `Button` :

[![Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 0)](porter-duff-images/BrickWallCompositing0.png "Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 0)")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

Wenn Sie die `Button` einmal `step` -Taste drücken, wird zu 1 erhöht, und der `PaintSurface` Handler zeigt nun **SeatedMonkey.jpg**an:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        float x = (info.Width - monkeyBitmap.Width) / 2;
        float y = info.Height - monkeyBitmap.Height;

        // Draw monkey bitmap
        if (step >= 1)
        {
            canvas.DrawBitmap(monkeyBitmap, x, y);
        }
        ···
    }
}
```

Es gibt kein- `SKPaint` Objekt und daher keinen Blend-Modus. Die Bitmap wird unten auf dem Bildschirm angezeigt:

[![Schritt 1: zusammengesetzte Brick-Wall-Zusammensetzung](porter-duff-images/BrickWallCompositing1.png "Schritt 1: zusammengesetzte Brick-Wall-Zusammensetzung")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

Drücken `Button` `step` Sie erneut, und Inkrementen zu 2. Dies ist der entscheidende Schritt beim Anzeigen der **SeatedMonkeyMatte.png** Datei:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw matte to exclude monkey's surroundings
        if (step >= 2)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.BlendMode = SKBlendMode.DstIn;
                canvas.DrawBitmap(matteBitmap, x, y, paint);
            }
        }
        ···
    }
}
```

Der Blend-Modus ist `SKBlendMode.DstIn` , was bedeutet, dass das Ziel in Bereichen beibehalten wird, die nicht transparenten Bereichen der Quelle entsprechen. Der Rest des Ziel Rechtecks, das der ursprünglichen Bitmap entspricht, wird transparent:

[![Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 2](porter-duff-images/BrickWallCompositing2.png "Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

Der Hintergrund wurde entfernt. 

Der nächste Schritt ist das Zeichnen eines Rechtecks, das einem Gehweg ähnelt, auf dem sich der Affe befindet. Die Darstellung dieses Gehens basiert auf einer Komposition von zwei Shadern: einem Vollbild-Shader und einem Perlin-Rausch-Shader:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        const float sidewalkHeight = 80;
        SKRect rect = new SKRect(info.Rect.Left, info.Rect.Bottom - sidewalkHeight,
                                 info.Rect.Right, info.Rect.Bottom);

        // Draw gravel sidewalk for monkey to sit on
        if (step >= 3)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateCompose(
                                    SKShader.CreateColor(SKColors.SandyBrown),
                                    SKShader.CreatePerlinNoiseTurbulence(0.1f, 0.3f, 1, 9));

                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(rect, paint);
            }
        }
        ···
    }
}
```

Da dieser Pullmodus hinter dem Monkey liegen muss, ist der Blend-Modus `DstOver` . Das Ziel wird nur angezeigt, wenn der Hintergrund transparent ist:

[![Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 3)](porter-duff-images/BrickWallCompositing3.png "Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 3)")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

Der letzte Schritt besteht darin, eine Brick-Wand hinzuzufügen. Das Programm verwendet die Kachel "Brick-Wall Bitmap", die als statische Eigenschaft `BrickWallTile` in der-Klasse verfügbar ist `AlgorithmicBrickWallPage` . Dem-Befehl wird eine Übersetzungs Transformation hinzugefügt, um `SKShader.CreateBitmap` die Kacheln so zu verschieben, dass die untere Zeile eine vollständige Kachel ist:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw bitmap tiled brick wall behind monkey
        if (step >= 4)
        {
            using (SKPaint paint = new SKPaint())
            {
                SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;
                float yAdjust = (info.Height - sidewalkHeight) % bitmap.Height;

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     SKMatrix.MakeTranslation(0, yAdjust));
                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

Der Vorteil ist, dass der-Befehl `DrawRect` diesen Shader über dem gesamten Zeichenbereich anzeigt, aber der `DstOver` Modus schränkt die Ausgabe auf den Bereich der Canvas ein, der noch transparent ist:

[![Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 4](porter-duff-images/BrickWallCompositing4.png "Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

Natürlich gibt es noch weitere Möglichkeiten, diese Szene zu verfassen. Sie kann mit dem Hintergrund erstellt werden und sich im Vordergrund befinden. Die Verwendung der Blend-Modi bietet Ihnen jedoch mehr Flexibilität. Insbesondere ermöglicht die Verwendung der Matte, dass der Hintergrund einer Bitmap aus der zusammengesetzten Szene ausgeschlossen wird.

Wie Sie im Artikel [Clipping mit Pfaden und Regionen](../../curves/clipping.md)gelernt haben, definiert die- `SKCanvas` Klasse drei clippingtypen, die den [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*) [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*) -,-und- [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) Methoden entsprechen. Die Blend-Modi "Porter-Duff" fügen einen weiteren Clipping-Typ hinzu, der das Einschränken eines Bilds auf alles ermöglicht, das Sie zeichnen können (einschließlich Bitmaps). Die beim Zusammensetzen von Bricks verwendete Matte definiert **im Wesentlichen einen** Clippingbereich.

## <a name="gradient-transparency-and-transitions"></a>Transparenz und Übergänge im Verlauf

Die Beispiele der in diesem Artikel gezeigten Porter-Duff-Blend-Modi enthalten alle Beteiligten Bilder, die aus nicht transparenten Pixeln und transparenten Pixeln, aber nicht teilweise transparenten Pixeln besteht. Die Funktionen des Blend-Modus werden ebenfalls für diese Pixel definiert. Die folgende Tabelle ist eine formellere Definition der "Porter-Duff Blend"-Modi, in der die in der Skia- [**skblendmode-Referenz**](https://skia.org/user/api/SkBlendMode_Reference)gefundene Notation verwendet wird. (Da der **skblendmode-Verweis** ein Skia-Verweis ist, wird die C++-Syntax verwendet.)

Konzeptionell werden die Teile rot, grün, blau und Alpha der einzelnen Pixel von Bytes in Gleit Komma Zahlen im Bereich von 0 bis 1 konvertiert. Für den Alphakanal ist 0 vollständig transparent, und 1 ist vollständig undurchsichtig.

In der Notation in der folgenden Tabelle werden die folgenden Abkürzungen verwendet:

- **Da** ist der Ziel-Alphakanal.
- **DC** ist die Ziel-RGB-Farbe.
- **Sa** ist der Quell-Alphakanal.
- **SC** ist die RGB-Quellfarbe

Die RGB-Farben werden vorab mit dem Alpha-Wert multipliziert. Wenn **SC** z. b. den reinen roten Wert darstellt, aber **sa** den Wert 0x80 hat, ist die RGB-Farbe **(0x80, 0, 0)**. Wenn **sa** gleich 0 ist, sind alle RGB-Komponenten ebenfalls 0 (null).

Das Ergebnis wird in eckigen Klammern angezeigt, wobei der Alphakanal und die RGB-Farbe durch ein Komma getrennt sind: **[alpha, Color]**. Für die Farbe wird die Berechnung separat für die roten, grünen und blauen Komponenten durchgeführt:

| Mode       | Vorgang |
| ---------- | --------- |
| `Clear`    | [0,0]    |
| `Src`      | [SA, SC]  |
| `Dst`      | [Da, DC]  |
| `SrcOver`  | [SA + da (1 – SA), SC + DC (1 – SA) | 
| `DstOver`  | [Da + SA = (1 – da), DC + SC (1 – da) |
| `SrcIn`    | SAS Da, SC · De |
| `DstIn`    | De Sa, DC SAS |
| `SrcOut`   | SAS (1 – da), SC (1 – da)] |
| `DstOut`   | De (1 – SA), DC (1 – SA)] |
| `SrcATop`  | [Da, SC Da + DC (1 – SA)] |
| `DstATop`  | [SA, DC Sa + SC (1 – da)] |
| `Xor`      | [SA + da – 2 SAS Da, SC · (1 – da) + DC (1 – SA)] |
| `Plus`     | [SA + da, SC + DC] |
| `Modulate` | SAS Da, SC · DC | 

Diese Vorgänge sind einfacher zu analysieren, wenn **da** und **sa** entweder 0 oder 1 sind. Wenn z. b. für den Standardmodus der Wert für "SA" den Wert "0" hat, `SrcOver` ist " **SC** " ebenfalls 0, und das Ergebnis ist **[da, DC]**, das zielalpha und die Farbe. **Sa** Wenn **sa** den Wert 1 hat, ist das Ergebnis **[SA, SC]**, Alpha und Farbe der Quelle oder **[1, SC]**.

Die `Plus` `Modulate` Modi und unterscheiden sich geringfügig von den anderen, da neue Farben aus der Kombination aus Quelle und Ziel resultieren können. Der `Plus` Modus kann entweder mit Byte Komponenten oder Gleit Komma Komponenten interpretiert werden. Auf der zuvor gezeigten Raster Seite " **Porter-Duff** " ist die Zielfarbe **(0xC0, 0x80, 0x00)** , und die Quellfarbe ist **(0x00, 0x80, 0xC0)**. Jedes Paar von Komponenten wird hinzugefügt, aber die Summe wird bei 0xFF geklammert. Das Ergebnis ist die Farbe **(0xC0, 0xFF, 0xC0)**. Das ist die Farbe, die in der Schnittmenge angezeigt wird.

Für den- `Modulate` Modus müssen die RGB-Werte in den Gleit Komma Wert konvertiert werden. Die Zielfarbe ist **(0,75, 0,5, 0)** , und die Quelle ist **(0, 0,5, 0,75)**. Die RGB-Komponenten werden jeweils miteinander multipliziert, und das Ergebnis ist **(0, 0,25, 0)**. Das ist die Farbe, die in der Schnittmenge auf der Seite " **Porter-Duff** " für diesen Modus angezeigt wird.

Auf der Seite " **Porter-Duff-Transparenz** " können Sie untersuchen, wie die Porter-Duff-Blend-Modi auf grafisch transparenten Objekten angewendet werden. Die XAML-Datei enthält eine `Picker` mit dem Porter-Duff-Modus:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PorterDuffTransparencyPage"
             Title="Porter-Duff Transparency">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Clear" />
                    <x:Static Member="skia:SKBlendMode.Src" />
                    <x:Static Member="skia:SKBlendMode.Dst" />
                    <x:Static Member="skia:SKBlendMode.SrcOver" />
                    <x:Static Member="skia:SKBlendMode.DstOver" />
                    <x:Static Member="skia:SKBlendMode.SrcIn" />
                    <x:Static Member="skia:SKBlendMode.DstIn" />
                    <x:Static Member="skia:SKBlendMode.SrcOut" />
                    <x:Static Member="skia:SKBlendMode.DstOut" />
                    <x:Static Member="skia:SKBlendMode.SrcATop" />
                    <x:Static Member="skia:SKBlendMode.DstATop" />
                    <x:Static Member="skia:SKBlendMode.Xor" />
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                3
            </Picker.SelectedIndex>
        </Picker>
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei füllt zwei Rechtecke derselben Größe mithilfe eines linearen Farbverlaufs aus. Der Ziel Farbverlauf liegt von oben rechts nach unten links. Es wird in der oberen rechten Ecke durchsucht, aber in Richtung des Centers beginnt die Ausblendung zu transparent und ist in der unteren linken Ecke transparent. 

Das Quell Rechteck hat einen Farbverlauf von oben links nach unten rechts. Die obere linke Ecke ist ausgeblendet, wird aber wieder transparent und ist in der unteren rechten Ecke transparent. 

```csharp
public partial class PorterDuffTransparencyPage : ContentPage
{
    public PorterDuffTransparencyPage()
    {
        InitializeComponent();
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

        // Make square display rectangle smaller than canvas
        float size = 0.9f * Math.Min(info.Width, info.Height);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        SKRect rect = new SKRect(x, y, x + size, y + size);

        using (SKPaint paint = new SKPaint())
        {
            // Draw destination
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Right, rect.Top),
                                new SKPoint(rect.Left, rect.Bottom),
                                new SKColor[] { new SKColor(0xC0, 0x80, 0x00),
                                                new SKColor(0xC0, 0x80, 0x00, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            canvas.DrawRect(rect, paint);

            // Draw source
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { new SKColor(0x00, 0x80, 0xC0), 
                                                new SKColor(0x00, 0x80, 0xC0, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            // Get the blend mode from the picker
            paint.BlendMode = blendModePicker.SelectedIndex == -1 ? 0 :
                                    (SKBlendMode)blendModePicker.SelectedItem;

            canvas.DrawRect(rect, paint);

            // Stroke surrounding rectangle
            paint.Shader = null;
            paint.BlendMode = SKBlendMode.SrcOver;
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Dieses Programm veranschaulicht, dass die Blend-Modi "Porter-Duff" mit anderen Grafikobjekten als Bitmaps verwendet werden können. Allerdings muss die Quelle einen transparenten Bereich enthalten. Dies ist der Fall, da der Farbverlauf das Rechteck füllt, aber ein Teil des Farbverlaufs ist transparent.

Im folgenden finden Sie drei Beispiele:

[![Porter-Duff-Transparenz](porter-duff-images/PorterDuffTransparency.png "Porter-Duff-Transparenz")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

Die Konfiguration des Ziels und der Quelle ähnelt den Diagrammen, die auf der Seite 255 des ursprünglichen Dokuments "Porter-Duff [_Compositing Digital Images_](https://graphics.pixar.com/library/Compositing/paper.pdf) " angezeigt werden. Diese Seite zeigt jedoch, dass die Blend-Modi für Bereiche mit teilweiser Transparenz gut zu Verhalten sind.

Sie können transparente Farbverläufe für einige unterschiedliche Effekte verwenden. Eine Möglichkeit ist die Maskierung, die der Technik ähnelt, die im Abschnitt [**radiale Farbverläufe für die Maskierung**](../shaders/circular-gradients.md#radial-gradients-for-masking) der **Seite skiasharp-zirkuläre Farbverläufe**angezeigt wird. Ein Großteil der zusammen **gesetzten Masken** Seite ähnelt diesem früheren Programm. Es lädt eine Bitmap-Ressource und bestimmt ein Rechteck, in dem es angezeigt werden soll. Ein Radialer Farbverlauf wird basierend auf einem vordefinierten Mittelpunkt und RADIUS erstellt:

```csharp
public class CompositingMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(CompositingMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public CompositingMaskPage ()
    {
        Title = "Compositing Mask";

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
                                new SKColor[] { SKColors.Black,
                                                SKColors.Transparent },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            paint.BlendMode = SKBlendMode.DstIn;

            // Display rectangle using that gradient and blend mode
            canvas.DrawRect(rect, paint);
        }

        canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
    }
}
```

Der Unterschied zu diesem Programm besteht darin, dass der Farbverlauf in der Mitte mit Schwarz beginnt und mit Transparenz endet. Es wird in der Bitmap mit einem Blend-Modus von angezeigt `DstIn` , der das Ziel nur in den Bereichen der Quelle anzeigt, die nicht transparent sind.

Nach dem-Befehl `DrawRect` ist die gesamte Oberfläche der Canvas transparent, mit Ausnahme des durch den radialen Farbverlauf definierten Kreises. Es wird ein abschließender-Rückruf durchgeführt:

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

Alle transparenten Bereiche der Canvas sind rosa gefärbt:

[![Compositing-Maske](porter-duff-images/CompositingMask.png "Compositing-Maske")](porter-duff-images/CompositingMask-Large.png#lightbox)

Sie können auch den Modus "Porter-Duff" und teilweise transparente Farbverläufe für Übergänge von einem Bild zu einem anderen verwenden. Die Seite **Farbverlaufs Übergänge** enthält ein `Slider` -Wert, mit dem eine Fortschritts Ebene für den Übergang von 0 auf 1 und ein angegeben wird, `Picker` um den Typ des gewünschten Übergangs auszuwählen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.GradientTransitionsPage"
             Title="Gradient Transitions">
    
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference progressSlider},
                              Path=Value,
                              StringFormat='Progress = {0:F2}'}"
               HorizontalTextAlignment="Center" />

        <Picker x:Name="transitionPicker" 
                Title="Transition" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />
        
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei lädt zwei Bitmap-Ressourcen, um den Übergang zu veranschaulichen. Dies sind die gleichen zwei Images, die auf der Seite **Bitmap-Auflösung** weiter oben in diesem Artikel verwendet werden. Der Code definiert auch eine Enumeration mit drei Membern, die drei Typen von Farbverläufen &mdash; linear, radialen und Sweep entsprechen. Diese Werte werden in das geladen `Picker` :

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    SKBitmap bitmap1 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap bitmap2 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.FacePalm.jpg");

    enum TransitionMode
    {
        Linear,
        Radial,
        Sweep
    };

    public GradientTransitionsPage ()
    {
        InitializeComponent ();

        foreach (TransitionMode mode in Enum.GetValues(typeof(TransitionMode)))
        {
            transitionPicker.Items.Add(mode.ToString());
        }

        transitionPicker.SelectedIndex = 0;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }
    ···
}
```

Die Code-Behind-Datei erstellt drei `SKPaint` Objekte. Das- `paint0` Objekt verwendet keinen Blend-Modus. Dieses Paint-Objekt wird verwendet, um ein Rechteck mit einem Farbverlauf zu zeichnen, der gemäß der Angabe im-Array von schwarz zu transparent verläuft `colors` . Das `positions` Array basiert auf der Position des, wird `Slider` jedoch etwas angepasst. Wenn der `Slider` Minimal oder der Höchstwert ist, `progress` sind die Werte 0 oder 1, und eine der beiden Bitmaps sollte vollständig sichtbar sein. Das `positions` Array muss entsprechend für diese Werte festgelegt werden.

Wenn der `progress` Wert 0 ist, enthält das `positions` Array die Werte-0,1 und 0. Skiasharp passt den ersten Wert so an, dass er gleich 0 ist. Dies bedeutet, dass der Farbverlauf nur bei 0 schwarz ist und andernfalls transparent ist. Wenn `progress` 0,5 ist, enthält das Array die Werte 0,45 und 0,55. Der Farbverlauf ist schwarz zwischen 0 und 0,45, wechselt dann in transparent und ist von 0,55 bis 1 vollständig transparent. Wenn `progress` den Wert 1 hat, `positions` ist das Array 1 und 1,1. Dies bedeutet, dass der Farbverlauf von 0 bis 1 schwarz ist.

Die `colors` -und- `position` Arrays werden beide in den drei Methoden von verwendet `SKShader` , die einen Farbverlauf erstellen. Nur einer dieser Shader wird basierend auf der Auswahl erstellt `Picker` : 

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Assume both bitmaps are square for display rectangle
        float size = Math.Min(info.Width, info.Height);
        SKRect rect = SKRect.Create(size, size);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        rect.Offset(x, y);

        using (SKPaint paint0 = new SKPaint())
        using (SKPaint paint1 = new SKPaint())
        using (SKPaint paint2 = new SKPaint())
        {
            SKColor[] colors = new SKColor[] { SKColors.Black,
                                               SKColors.Transparent };

            float progress = (float)progressSlider.Value;

            float[] positions = new float[]{ 1.1f * progress - 0.1f,
                                             1.1f * progress };

            switch ((TransitionMode)transitionPicker.SelectedIndex)
            {
                case TransitionMode.Linear:
                    paint0.Shader = SKShader.CreateLinearGradient(
                                        new SKPoint(rect.Left, 0),
                                        new SKPoint(rect.Right, 0),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Radial:
                    paint0.Shader = SKShader.CreateRadialGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        (float)Math.Sqrt(Math.Pow(rect.Width / 2, 2) +
                                                         Math.Pow(rect.Height / 2, 2)),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Sweep:
                    paint0.Shader = SKShader.CreateSweepGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        colors,
                                        positions);
                    break;
            }

            canvas.DrawRect(rect, paint0);

            paint1.BlendMode = SKBlendMode.SrcOut;
            canvas.DrawBitmap(bitmap1, rect, paint1);

            paint2.BlendMode = SKBlendMode.DstOver;
            canvas.DrawBitmap(bitmap2, rect, paint2);
        }
    }
}
```

Dieser Farbverlauf wird im Rechteck ohne Blend-Modus angezeigt. Nach diesem-Befehl `DrawRect` enthält der Zeichenbereich einfach einen Farbverlauf von Schwarz bis transparent. Der schwarze Wert erhöht sich mit höheren `Slider` Werten.

In den letzten vier Anweisungen des- `PaintSurface` Handlers werden die beiden Bitmaps angezeigt. Der `SrcOut` Blend-Modus bedeutet, dass die erste Bitmap nur in den transparenten Bereichen des Hintergrunds angezeigt wird. Der `DstOver` Modus für die zweite Bitmap bedeutet, dass die zweite Bitmap nur in den Bereichen angezeigt wird, in denen die erste Bitmap nicht angezeigt wird.

Die folgenden Screenshots zeigen die drei verschiedenen Übergangs Typen, jeweils an der 50%-Markierung:

[![Farbverlaufs Übergänge](porter-duff-images/GradientTransitions.png "Farbverlaufs Übergänge")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
