---
title: Porter-Duff Füllmethoden einheitlich
description: Verwenden Sie die Porter-Duff Blend-Modi, um im Hintergrund basierend auf Quelle und Ziel-Images zu erstellen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: c15dd4606a75cc3cdffbad71f15299568157213a
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915840"
---
# <a name="porter-duff-blend-modes"></a>Porter-Duff Füllmethoden einheitlich

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die Blend-Modi Porter-Duff heißen nach Thomas Porter und Tom Duff, der eine Algebra der Zusammensetzung bei der Arbeit für Lucasfilm entwickelt hat. Der Artikel zum Zusammenstellen [_digitaler Images_](https://graphics.pixar.com/library/Compositing/paper.pdf) wurde im Juli 1984-Problem der _Computer Grafiken_veröffentlicht, Seiten 253 bis 259. Diese Füllmethoden für das zusammensetzen, die verschiedenen Bilder in einer zusammengesetzten Szene zusammenstellen, ist von entscheidender Bedeutung sind:

![Beispiel für Porter-Duff](porter-duff-images/PorterDuffSample.png "Beispiel für Porter-Duff")

## <a name="porter-duff-concepts"></a>Porter-Duff-Konzepte

Nehmen wir an, dass ein brownish Rechteck auf der linken und oberen zwei Drittel der Anzeigeoberfläche belegt:

![Porter-Duff-Ziel](porter-duff-images/PorterDuffDst.png "Porter-Duff-Ziel")

Dieser Bereich wird als _Ziel_ oder manchmal als _Hintergrund_ _oder Hintergrund bezeichnet._

Möchten Sie die folgende Rechteck zeichnen wird die gleiche Größe des Ziels. Das Rechteck ist transparent, mit Ausnahme von einem bläuliches Bereich, der am rechten und unteren zwei Drittel belegt:

![Porter-Duff-Quelle](porter-duff-images/PorterDuffSrc.png "Porter-Duff-Quelle")

Dies wird als _Quelle_ oder manchmal als _Vordergrund_bezeichnet.

Wenn Sie auf dem Ziel die Quelle anzeigen, ist hier was Sie erwarten:

![Porter-Duff-Quelle über](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff-Quelle über")

Die transparente Pixel von der Quelle ermöglichen den Hintergrund, angezeigt werden, während die Pixel bläuliches Quelle Hintergrund verdecken. Dies ist der Normalfall, und es wird in skiasharp als `SKBlendMode.SrcOver`bezeichnet. Dieser Wert ist die Standardeinstellung der `BlendMode`-Eigenschaft, wenn ein `SKPaint` Objekt zum ersten Mal instanziiert wird.

Allerdings ist es möglich, einen anderen Blend-Modus für unterschiedliche Wirkungen anzugeben. Wenn Sie `SKBlendMode.DstOver`angeben, wird das Ziel in dem Bereich, in dem sich die Quelle und das Ziel schneiden, anstelle der Quelle angezeigt:

![Porter-Duff-Ziel über](porter-duff-images/PorterDuffDstOver.png "Porter-Duff-Ziel über")

Im `SKBlendMode.DstIn` Blend-Modus wird nur der Bereich angezeigt, in dem sich Ziel und Quelle mithilfe der Zielfarbe überschneiden:

![Porter-Duff-Ziel in](porter-duff-images/PorterDuffDstIn.png "Porter-Duff-Ziel in")

Der Blend-Modus `SKBlendMode.Xor` (exklusiv oder) bewirkt, dass nichts angezeigt wird, wenn sich die beiden Bereiche überschneiden:

![Porter-Duff exklusiv oder](porter-duff-images/PorterDuffXor.png "Porter-Duff exklusiv oder")

Farbigen Rechtecke Ziel- und unterteilen die Anzeigeoberfläche effektiv in vier eindeutige Bereiche, die auf verschiedene Weise, die für das Vorhandensein der Rechtecke Ziel- und Quellserver zugewiesen werden können:

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter-Duff")

Die Rechtecke von rechten, oberen und unteren linken sind immer leer, da sowohl die Ziel- und in diesen Bereichen transparent sind. Der Zielfarbe nimmt den oberen linken Bereich, sodass Bereich entweder mit der Zielfarbe oder gar nicht zugewiesen werden kann. Auf ähnliche Weise nimmt die Quellfarbe Bereich unten rechts, damit ein Bereich mit der Quellfarbe oder gar nicht zugewiesen werden kann. Die Schnittmenge der Ziel- und in der Mitte kann mit der Zielfarbe, die Quellfarbe oder überhaupt nicht zugewiesen werden.

Die Gesamtanzahl der Kombinationen ist 2 (bei der linken oberen Ecke), multipliziert 2 (für der unteren rechten Ecke)-Mal 3 (für die der Mitte) und 12. Hierbei handelt es sich um die 12 grundlegende Porter-Duff Compositing-Modi.

Am Ende der Zusammensetzung _digitaler Images_ (Seite 256) fügen Porter und Duff einen 13. Modus mit dem Namen _plus_ hinzu (entsprechend dem skiasharp-`SKBlendMode.Plus`-Element und dem _aushelleren_ W3C-Modus (der nicht mit dem Modus der W3C- _Lighten_ verwechselt werden muss). Dieser `Plus` Modus fügt die Ziel-und Quell Farben hinzu, einen Prozess, der in Kürze ausführlicher beschrieben wird.

Skia fügt einen 14. Modus mit dem Namen `Modulate` hinzu, der `Plus` sehr ähnlich ist, mit dem Unterschied, dass die Ziel-und Quell Farben multipliziert werden. Sie können eine zusätzliche Porter-Duff Blend-Modus behandelt werden.

Hier sind die 14 Porter-Duff Modi wie in SkiaSharp definiert. Die Tabelle zeigt, wie sie jede der drei Bereiche in der Abbildung oben nicht leere Farbe:

| Mode       | Destination | Schnittmenge | `Source` |
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

Diese Füllmethoden sind symmetrisch. Die Quelle und Ziel können ausgetauscht werden, und allen Modi stehen weiterhin zur Verfügung.

Die Namenskonvention der Modi,: einige einfache Regeln

- **Src** oder **DST** allein bedeutet, dass nur die Quell-oder Ziel Pixel sichtbar sind.
- Das **over** -Suffix gibt an, was in der Schnittmenge sichtbar ist. Die Quelle oder das Ziel wird "sink" gezeichnet.
- Das **in** -Suffix bedeutet, dass nur die Schnittmenge gefärbt ist. Die Ausgabe ist auf nur den Teil der Quelle oder Ziel "in" anderen beschränkt.
- Das **out** -Suffix bedeutet, dass die Schnittmenge nicht farbig ist. Die Ausgabe ist nur der Teil der Quelle oder Ziel, das die Schnittmenge "out" ist.
- Das on **-Suffix ist die** Union **von** ein **-und**ausgehend. Sie enthält den Bereich, in dem die Quelle oder das Ziel "oberhalb" der anderen Seite ist.

Beachten Sie den Unterschied zu den Modi für `Plus` und `Modulate`. Diese Modi ausführen ein anderen Typs, der Berechnung auf den Quell- und Zielserver Pixel. Sie werden in Kürze ausführlicher beschrieben.

Auf der Raster Seite " **Porter-Duff** " werden alle 14 Modi auf einem Bildschirm in Form eines Rasters angezeigt. Jeder Modus ist eine separate Instanz von `SKCanvasView`. Aus diesem Grund wird eine Klasse von `SKCanvasView` namens `PorterDuffCanvasView`abgeleitet. Der statische Konstruktor erstellt zwei Bitmaps der gleichen Größe, eine mit einem brownish Rechteck in der oberen linken Bereich und eine mit einem bläuliches Rechteck:

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

Der Instanzkonstruktor verfügt über einen Parameter vom Typ `SKBlendMode`. Dieser Parameter wird in einem Feld gespeichert. 

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

Durch die `OnPaintSurface` Überschreibung werden die beiden Bitmaps gezeichnet. Die erste wird normalerweise gezeichnet werden:

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

Die zweite wird mit einem `SKPaint` Objekt gezeichnet, bei dem die `BlendMode`-Eigenschaft auf das Konstruktorargument festgelegt wurde:

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

Der Rest der `OnPaintSurface` Überschreibung zeichnet ein Rechteck um die Bitmap, um deren Größe anzugeben.

Die `PorterDuffGridPage`-Klasse erstellt 14 Instanzen von `PorterDurffCanvasView`, eine für jedes Element des `blendModes` Arrays. Die Reihenfolge der `SKBlendModes` Elemente im Array unterscheidet sich geringfügig von der Tabelle, um ähnliche Modi nebeneinander positionieren zu können. Die 14 Instanzen `PorterDuffCanvasView` werden zusammen mit den Bezeichnungen in einem `Grid`organisiert:

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

Das Ergebnis lautet wie folgt:

[![Porter-Duff-Raster](porter-duff-images/PorterDuffGrid.png "Porter-Duff-Raster")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Sie sollten sich selbst überzeugen, dass Transparenz entscheidend für das ordnungsgemäße Funktionieren der Porter-Duff Füllmethoden einheitlich ist. Die `PorterDuffCanvasView`-Klasse enthält insgesamt drei Aufrufe der `Canvas.Clear`-Methode. Alle verwenden die parameterlose Methode, die alle Pixel auf transparentes festlegt:

```csharp
canvas.Clear();
```

Versuchen Sie es ändern dieser Aufrufe, damit die Pixel auf ein nicht transparenter weiß festgelegt werden:

```csharp
canvas.Clear(SKColors.White);
```

Nach dieser Änderung werden einige der Füllmethoden einheitlich, scheint zu funktionieren, aber andere hingegen nicht. Wenn Sie den Hintergrund der Quell Bitmap auf weiß festgelegt haben, funktioniert der `SrcOver` Modus nicht, da es keine transparenten Pixel in der Quell Bitmap gibt, damit das Ziel durch die Anzeige ermöglicht wird. Wenn Sie den Hintergrund der Ziel Bitmap oder der Canvas auf White festlegen, funktioniert `DstOver` nicht, da das Ziel keine transparenten Pixel hat.

Möglicherweise besteht eine Versuchung, die Bitmaps in der " **Porter-Duff"-Raster** Seite durch einfachere `DrawRect` Aufrufe zu ersetzen. Dies wird für das Zielrechteck jedoch nicht für das Quellrechteck funktioniert. Das Quellrechteck muss mehr als nur den bläuliches farbig Bereich umfassen. Das Quellrechteck muss es sich um einen transparenten Bereich enthalten, der die die Farbiger Bereich, der das Ziel entspricht. Klicken Sie dann nur einfügen diese Modi arbeiten.

## <a name="using-mattes-with-porter-duff"></a>Verwenden von Masken mit Porter-Duff

Die zusammengesetzte Seite " **Brick-Wall Zusammensetzung** " zeigt ein Beispiel für eine klassische Zusammensetzung-Aufgabe: ein Bild muss aus mehreren Teilen zusammengestellt werden, einschließlich einer Bitmap mit einem Hintergrund, der entfernt werden muss. Hier ist die Bitmap " **seatedmonkey. jpg** " mit dem problematischen Hintergrund:

![Sitzender Affe](porter-duff-images/SeatedMonkey.jpg "Sitzender Affe")

Als Vorbereitung für die Zusammensetzung wurde eine entsprechende _Matte_ erstellt, bei der es sich um eine weitere Bitmap handelt, bei der es sich um eine andere Bitmap handelt. Diese Datei trägt den Namen " **SeatedMonkeyMatte. png** " und befindet sich im Beispiel " [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) " unter den Ressourcen im Ordner " **Media** ":

![Sitzender affmatt](porter-duff-images/SeatedMonkeyMatte.png "Sitzender affmatt")

Dabei handelt es sich _nicht_ um eine sachkundig erstellte Matte. Optimal Maske sollte teilweise transparente Pixel entlang der Bildschirmkante die schwarze Pixel enthalten, und dieser Maske nicht.

Die XAML-Datei für die zusammengesetzte Seite " **Brick-Wall Compositing** " instanziiert eine `SKCanvasView` und eine `Button`, die den Benutzer durch den Prozess des Ersetzens des endgültigen Bilds führt:

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

Die Code-Behind-Datei lädt die beiden benötigten Bitmaps und behandelt das `Clicked`-Ereignis des `Button`. Für jede `Button` klicken Sie auf das Feld `step`, und für das `Button`wird eine neue `Text`-Eigenschaft festgelegt. Wenn `step` 5 erreicht, wird es auf 0 zurückgesetzt:

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

Wenn das Programm zum ersten Mal ausgeführt wird, ist nur der `Button`sichtbar:

[![Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 0)](porter-duff-images/BrickWallCompositing0.png "Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 0)")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

Wenn Sie die `Button` einmal drücken, wird `step` auf 1 erhöht, und der `PaintSurface`-Handler zeigt jetzt " **seatedmonkey. jpg**" an:

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

Es gibt kein `SKPaint` Objekt und daher keinen Blend-Modus. Die Bitmap wird am unteren Rand des Bildschirms angezeigt:

[![Schritt 1: zusammengesetzte Brick-Wall-Zusammensetzung](porter-duff-images/BrickWallCompositing1.png "Schritt 1: zusammengesetzte Brick-Wall-Zusammensetzung")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

Drücken Sie erneut die `Button`, und `step` Inkrementen zu 2. Dies ist der entscheidende Schritt beim Anzeigen der Datei **SeatedMonkeyMatte. png** :

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

Der Blend-Modus ist `SKBlendMode.DstIn`. Dies bedeutet, dass das Ziel in Bereichen beibehalten wird, die nicht transparenten Bereichen der Quelle entsprechen. Der Rest des Zielrechtecks entspricht der ursprünglichen Bitmap wird transparent:

[![Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 2](porter-duff-images/BrickWallCompositing2.png "Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

Der Hintergrund wurde entfernt. 

Der nächste Schritt ist zum Zeichnen eines Rechtecks, das eine Fußweg ähnelt, die das Monkey-Objekt befindet. Die Darstellung dieser Fußweg basiert darauf, dass eine Zusammensetzung von zwei Shadern: eine Volltonfarbe-Shader und einen Perlin-Noise-Shader:

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

Da dieser Pullmodus hinter dem Monkey liegen muss, ist der Blend-Modus `DstOver`. Das Ziel wird nur angezeigt, in dem Hintergrund transparent ist:

[![Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 3)](porter-duff-images/BrickWallCompositing3.png "Zusammengesetzte Brick-Wall-Zusammensetzung (Schritt 3)")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

Der letzte Schritt einen Durchbruch hinzugefügt werden. Das Programm verwendet die Kachel "Brick-Wall Bitmap", die als statische Eigenschaften `BrickWallTile` in der `AlgorithmicBrickWallPage`-Klasse verfügbar ist. Eine Übersetzungs Transformation wird dem `SKShader.CreateBitmap`-aufrufsbefehl hinzugefügt, um die Kacheln so zu verschieben, dass die untere Zeile eine vollständige Kachel ist:

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

Der `DrawRect`-Befehl zeigt den Shader für den gesamten Zeichenbereich an, aber der `DstOver` Modus schränkt die Ausgabe auf den Bereich der Canvas ein, der noch transparent ist:

[![Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 4](porter-duff-images/BrickWallCompositing4.png "Zusammengesetzte Brick-Wall-Zusammensetzung Schritt 4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

Es gibt natürlich weitere Möglichkeiten, diese Szene zu erstellen. Es kann erstellt werden, beginnend mit den Hintergrund und in den Vordergrund fortgesetzt. Jedoch können Sie mithilfe der Füllmethoden einheitlich flexibler. Insbesondere ermöglicht die Verwendung der Maske den Hintergrund einer Bitmap aus der zusammengesetzten Szene ausgeschlossen werden sollen.

Wie Sie im Artikel [Clipping mit Pfaden und Regionen](../../curves/clipping.md)gelernt haben, werden in der `SKCanvas`-Klasse drei clippingtypen definiert, die den Methoden [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*), [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*)und [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) entsprechen. Die Porter-Duff Füllmethoden einheitlich fügen Sie eine andere Art von Clipping, dadurch kann ein Bild auf alle Elemente, die Sie zeichnen können, einschließlich Bitmaps, zu beschränken. Die beim Zusammensetzen von Bricks verwendete Matte definiert **im Wesentlichen einen** Clippingbereich.

## <a name="gradient-transparency-and-transitions"></a>Farbverlauf Transparenz und Übergänge

Die Beispiele für die Porter-Duff Füllmethoden, die weiter oben gezeigt, in diesem Artikel alle Images beteiligt sein, die nicht transparenten Pixel und transparente Pixel, jedoch nicht teilweise transparente Pixel aus. Die Blend-Modus-Funktionen sind für diese Pixel ebenfalls definiert. Die folgende Tabelle ist eine formellere Definition der "Porter-Duff Blend"-Modi, in der die in der Skia- [**skblendmode-Referenz**](https://skia.org/user/api/SkBlendMode_Reference)gefundene Notation verwendet wird. (Da es sich bei der **skblendmode-Referenz** um C++ einen Skia-Verweis handelt, wird Syntax verwendet.)

Im Prinzip werden die Komponenten roten, grünen, blauen und alpha der einzelnen Pixel von Bytes in Gleitkommazahlen im Bereich von 0 bis 1 konvertiert. Für den Alphakanal 0 vollständig transparent und 1 vollständig undurchsichtig ist

Die Notation in der folgenden Tabelle werden die folgenden Abkürzungen verwendet:

- **Da** ist der Ziel-Alphakanal.
- **DC** ist die Ziel-RGB-Farbe.
- **Sa** ist der Quell-Alphakanal.
- **SC** ist die RGB-Quellfarbe

RGB-Farben werden vor dem Alphawert multipliziert. Wenn **SC** z. b. den reinen roten Wert darstellt, aber **sa** den Wert 0x80 hat, ist die RGB-Farbe **(0x80, 0, 0)** . Wenn **sa** gleich 0 ist, sind alle RGB-Komponenten ebenfalls 0 (null).

Das Ergebnis wird in eckigen Klammern angezeigt, wobei der Alphakanal und die RGB-Farbe durch ein Komma getrennt sind: **[alpha, Color]** . Für die Farbe erfolgt die Berechnung für die Komponenten roten, grünen und blauen getrennt:

| Mode       | Vorgang |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [Sa, Sc]  |
| `Dst`      | [Dashboardbereich Dc]  |
| `SrcOver`  | [Sa + Da· (1 – Sa), Sc + Dc· (1 – Sa) | 
| `DstOver`  | [Da + Sa· (1 – Da), Dc + Sc· (1 – Da) |
| `SrcIn`    | [Sa· Da, Sc· Da] |
| `DstIn`    | [Da· SA, Dc· SA] |
| `SrcOut`   | [Sa· (1 – Da), Sc· (1 – Da]) |
| `DstOut`   | [Da· (1 – Sa), Dc· (1 – Sa)] |
| `SrcATop`  | [Dashboardbereich Sc· Da + Dc· (1 – Sa)] |
| `DstATop`  | [Sa, Dc· SA + Sc· (1 – Da]) |
| `Xor`      | [Sa + Da – 2· SA· Da, Sc· (1 – Da) + Dc· (1 – Sa)] |
| `Plus`     | ["Sa" + "Daten", "Sc" + "Dc"] |
| `Modulate` | [Sa· Da, Sc· DC] | 

Diese Vorgänge sind einfacher zu analysieren, wenn **da** und **sa** entweder 0 oder 1 sind. Wenn z. b. für den standardmäßigen `SrcOver` Modus der Wert für " **sa** " den Wert "0" hat, ist " **SC** " ebenfalls 0, und das Ergebnis ist **[da, DC]** , das zielalpha und die Farbe Wenn **sa** den Wert 1 hat, ist das Ergebnis **[SA, SC]** , Alpha und Farbe der Quelle oder **[1, SC]** .

Die `Plus`-und `Modulate` Modi unterscheiden sich geringfügig von den anderen, da neue Farben aus der Kombination aus Quelle und Ziel resultieren können. Der `Plus` Modus kann entweder mit Byte Komponenten oder Gleit Komma Komponenten interpretiert werden. Auf der zuvor gezeigten Raster Seite " **Porter-Duff** " ist die Zielfarbe **(0xC0, 0x80, 0x00)** , und die Quellfarbe ist **(0x00, 0x80, 0xC0)** . Jedes Paar von Komponenten hinzugefügt, aber die Summe wird am 0xFF gebunden sind. Das Ergebnis ist die Farbe **(0xC0, 0xFF, 0xC0)** . Dies ist die Farbe, die in die Schnittmenge angezeigt.

Für den `Modulate` Modus müssen die RGB-Werte in den Gleit Komma Wert konvertiert werden. Die Zielfarbe ist **(0,75, 0,5, 0)** , und die Quelle ist **(0, 0,5, 0,75)** . Die RGB-Komponenten werden jeweils miteinander multipliziert, und das Ergebnis ist **(0, 0,25, 0)** . Das ist die Farbe, die in der Schnittmenge auf der Seite " **Porter-Duff** " für diesen Modus angezeigt wird.

Auf der Seite " **Porter-Duff-Transparenz** " können Sie untersuchen, wie die Porter-Duff-Blend-Modi auf grafisch transparenten Objekten angewendet werden. Die XAML-Datei enthält eine `Picker` mit den "Porter-Duff"-Modi:

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

Die Code-Behind-Datei füllt zwei Rechtecke der gleichen Größe mit einem linearen Farbverlauf. Der Ziel-Gradient ist von oben rechts auf der linken unteren Ecke. Dabei handelt es sich in der oberen rechten Ecke brownish aber klicken Sie dann in Richtung der Mitte beginnt, Ausblenden von Fenstern transparent, transparent in der unteren linken Ecke. 

Das Quellrechteck hat es sich um einen Farbverlauf von oben links auf der unteren rechten Ecke. Die linke obere Ecke ist bläuliches jedoch erneut transparent wird, und Transparenz in der unteren rechten Ecke. 

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

Dieses Programm zeigt, dass es sich bei der Porter-Duff Füllmethoden einheitlich mit Grafikobjekten als Bitmaps verwendet werden können. Allerdings muss die Quelle einen transparenten Bereich enthalten. Dies ist hier der Fall, da der Gradient füllt das Rechteck, jedoch Teil des Farbverlaufs transparent ist.

Hier sind drei Beispiele:

[![Porter-Duff-Transparenz](porter-duff-images/PorterDuffTransparency.png "Porter-Duff-Transparenz")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

Die Konfiguration des Ziels und der Quelle ähnelt den Diagrammen, die auf der Seite 255 des ursprünglichen Dokuments "Porter-Duff [_Compositing Digital Images_](https://graphics.pixar.com/library/Compositing/paper.pdf) " angezeigt werden. Diese Seite zeigt jedoch, dass die Blend-Modi für Bereiche mit teilweiser Transparenz gut zu Verhalten sind.

Sie können für einige unterschiedliche Auswirkungen transparente Farbverläufe verwenden. Eine Möglichkeit ist die Maskierung, die der Technik ähnelt, die im Abschnitt [**radiale Farbverläufe für die Maskierung**](../shaders/circular-gradients.md#radial-gradients-for-masking) der **Seite skiasharp-zirkuläre Farbverläufe**angezeigt wird. Ein Großteil der zusammen **gesetzten Masken** Seite ähnelt diesem früheren Programm. Lädt eine Bitmapressource und bestimmt ein Rechteck, in dem es angezeigt werden soll. Ein Radialer Farbverlauf wird auf einem vorher festgelegten Center und die RADIUS-Basis erstellt:

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

Der Unterschied bei der dieses Programm ist, dass der Farbverlauf mit Schwarz, in der Mitte mit beginnt und Transparenz endet. Er wird in der Bitmap mit dem Blend-Modus `DstIn`angezeigt, der das Ziel nur in den Bereichen der Quelle anzeigt, die nicht transparent sind.

Nach dem `DrawRect`-Befehl ist die gesamte Oberfläche der Canvas transparent, mit Ausnahme des durch den radialen Farbverlauf definierten Kreises. Letzte wird aufgerufen:

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

Die transparenten Bereiche des Zeichenbereichs werden rosa gefärbt:

[![Compositing-Maske](porter-duff-images/CompositingMask.png "Compositing-Maske")](porter-duff-images/CompositingMask-Large.png#lightbox)

Sie können auch Porter-Duff Modi und teilweise transparente Gradienten für Übergänge aus einem Bild in einen anderen verwenden. Die Seite **Farbverlaufs Übergänge** enthält eine `Slider`, die auf eine Fortschritts Ebene beim Übergang von 0 auf 1 und eine `Picker` zur Auswahl des gewünschten Übergangs Typs hinweist:

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

Die Code-Behind-Datei lädt zwei Bitmapressourcen, um den Übergang zu veranschaulichen. Dies sind die gleichen zwei Images, die auf der Seite **Bitmap-Auflösung** weiter oben in diesem Artikel verwendet werden. Der Code definiert auch eine Enumeration mit drei Membern, die drei Typen von Farbverläufen entsprechen &mdash; linear, radialen und Sweep. Diese Werte werden in die `Picker`geladen:

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

Die Code-Behind-Datei erstellt drei `SKPaint`-Objekte. Das `paint0`-Objekt verwendet keinen Blend-Modus. Dieses Paint-Objekt wird verwendet, um ein Rechteck mit einem Farbverlauf zu zeichnen, der wie im `colors` Array von schwarz zu transparent wechselt. Das `positions` Array basiert auf der Position des `Slider`, wird jedoch etwas angepasst. Wenn der `Slider` den minimalen oder maximalen Wert hat, sind die `progress` Werte 0 oder 1, und eine der beiden Bitmaps sollte vollständig sichtbar sein. Das `positions` Array muss für diese Werte entsprechend festgelegt werden.

Wenn der `progress` Wert 0 ist, enthält das `positions` Array die Werte-0,1 und 0. SkiaSharp wird angepasst, diesen ersten Wert gleich 0 ist, was bedeutet, dass der Farbverlauf nur bei 0 Schwarz ist und transparent andernfalls. Wenn `progress` 0,5 ist, enthält das Array die Werte 0,45 und 0,55. Der Gradient ist schwarz von 0, 0,45, und klicken Sie dann transparent übergeht, und ist vollständig transparent von 0,55 auf 1. Wenn `progress` 1 ist, ist das `positions` Array 1 und 1,1. Dies bedeutet, dass der Farbverlauf von 0 bis 1 schwarz ist.

Die `colors`-und `position` Arrays werden beide in den drei Methoden von `SKShader` verwendet, die einen Farbverlauf erstellen. Nur einer dieser Shader wird basierend auf der `Picker` Auswahl erstellt: 

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

Dieser Verlauf wird in das Rechteck ohne Mischmodus angezeigt. Nachdem dieser `DrawRect` aufgerufen hat, enthält der Canvas einfach einen Farbverlauf von Schwarz bis transparent. Der schwarze Wert erhöht sich mit höheren `Slider` Werten.

In den letzten vier Anweisungen des `PaintSurface` Handlers werden die beiden Bitmaps angezeigt. Der `SrcOut` Blend-Modus bedeutet, dass die erste Bitmap nur in den transparenten Bereichen des Hintergrunds angezeigt wird. Der `DstOver` Modus für die zweite Bitmap bedeutet, dass die zweite Bitmap nur in den Bereichen angezeigt wird, in denen die erste Bitmap nicht angezeigt wird.

Die folgenden Screenshots zeigen die drei Typen von verschiedenen Übergänge, jede mit der Markierung von 50 %:

[![Farbverlaufs Übergänge](porter-duff-images/GradientTransitions.png "Farbverlaufs Übergänge")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
