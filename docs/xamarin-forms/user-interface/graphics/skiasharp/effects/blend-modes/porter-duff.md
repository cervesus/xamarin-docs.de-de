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
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199875"
---
# <a name="porter-duff-blend-modes"></a>Porter-Duff Füllmethoden einheitlich

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die Blend-Modi Porter-Duff heißen nach Thomas Porter und Tom Duff, der eine Algebra der Zusammensetzung bei der Arbeit für Lucasfilm entwickelt hat. Die Artikel [ _Zusammensetzung Digitalbilder_ ](https://graphics.pixar.com/library/Compositing/paper.pdf) veröffentlicht wurde, in der Ausgabe vom Juli 1984 _Computergrafiken_, Seiten von 253 auf 259. Diese Füllmethoden für das zusammensetzen, die verschiedenen Bilder in einer zusammengesetzten Szene zusammenstellen, ist von entscheidender Bedeutung sind:

![Porter-Duff Beispiel](porter-duff-images/PorterDuffSample.png "Porter-Duff-Beispiel")

## <a name="porter-duff-concepts"></a>Porter-Duff-Konzepte

Nehmen wir an, dass ein brownish Rechteck auf der linken und oberen zwei Drittel der Anzeigeoberfläche belegt:

![Porter-Duff Ziel](porter-duff-images/PorterDuffDst.png "Porter-Duff-Ziel")

Dieser Bereich wird aufgerufen, die _Ziel_ oder manchmal die _Hintergrund_ oder _Backdrop_.

Möchten Sie die folgende Rechteck zeichnen wird die gleiche Größe des Ziels. Das Rechteck ist transparent, mit Ausnahme von einem bläuliches Bereich, der am rechten und unteren zwei Drittel belegt:

![Porter-Duff Quelle](porter-duff-images/PorterDuffSrc.png "Porter-Duff-Quelle")

Dies wird aufgerufen, die _Quelle_ oder manchmal die _Vordergrund_.

Wenn Sie auf dem Ziel die Quelle anzeigen, ist hier was Sie erwarten:

![Quelle Porter-Duff](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff Quelle stationär")

Die transparente Pixel von der Quelle ermöglichen den Hintergrund, angezeigt werden, während die Pixel bläuliches Quelle Hintergrund verdecken. Das ist der Normalfall, und es wird gemäß SkiaSharp als `SKBlendMode.SrcOver`. Dass der Wert der Standardeinstellung ist die `BlendMode` Eigenschaft bei einem `SKPaint` -Objekt instanziiert wird.

Allerdings ist es möglich, einen anderen Blend-Modus für unterschiedliche Wirkungen anzugeben. Bei Angabe von `SKBlendMode.DstOver`, und klicken Sie dann in den Bereich, in dem die Quelle und Ziel überschneiden, das Ziel anstelle der Quelle angezeigt wird:

![Porter-Duff Ziel über](porter-duff-images/PorterDuffDstOver.png "Porter-Duff Ziel über")

Die `SKBlendMode.DstIn` Blend-Modus wird nur den Bereich, in dem das Ziel und Quelle überschneiden, mit der Zielfarbe, angezeigt:

![Ziel Porter-Duff](porter-duff-images/PorterDuffDstIn.png "Porter-Duff Ziel")

Blend-Modus von `SKBlendMode.Xor` (exklusives OR) bewirkt, dass nichts angezeigt werden, in dem die zwei Bereiche überschneiden:

![Porter-Duff exklusiv oder](porter-duff-images/PorterDuffXor.png "Porter-Duff exklusiv oder")

Farbigen Rechtecke Ziel- und unterteilen die Anzeigeoberfläche effektiv in vier eindeutige Bereiche, die auf verschiedene Weise, die für das Vorhandensein der Rechtecke Ziel- und Quellserver zugewiesen werden können:

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter-Duff")

Die Rechtecke von rechten, oberen und unteren linken sind immer leer, da sowohl die Ziel- und in diesen Bereichen transparent sind. Der Zielfarbe nimmt den oberen linken Bereich, sodass Bereich entweder mit der Zielfarbe oder gar nicht zugewiesen werden kann. Auf ähnliche Weise nimmt die Quellfarbe Bereich unten rechts, damit ein Bereich mit der Quellfarbe oder gar nicht zugewiesen werden kann. Die Schnittmenge der Ziel- und in der Mitte kann mit der Zielfarbe, die Quellfarbe oder überhaupt nicht zugewiesen werden.

Die Gesamtanzahl der Kombinationen ist 2 (bei der linken oberen Ecke), multipliziert 2 (für der unteren rechten Ecke)-Mal 3 (für die der Mitte) und 12. Hierbei handelt es sich um die 12 grundlegende Porter-Duff Compositing-Modi.

Gegen Ende _Zusammensetzung Digitalbilder_ (Seite 256) Porter und Duff fügen Sie einen Modus 13. _plus_ (entspricht der SkiaSharp `SKBlendMode.Plus` Member und der W3C _heller_  Modus (d.h. nicht zu verwechseln mit der W3C _Aufhellen_ Modus.) Dies `Plus` Modus fügt die Ziel- und Farben, ein Prozess, der in Kürze wird ausführlicher beschrieben werden.

Skia Fügt einen 14. Modus `Modulate` ähnelt sehr `Plus` mit dem Unterschied, dass die Ziel- und Farben multipliziert werden. Sie können eine zusätzliche Porter-Duff Blend-Modus behandelt werden.

Hier sind die 14 Porter-Duff Modi wie in SkiaSharp definiert. Die Tabelle zeigt, wie sie jede der drei Bereiche in der Abbildung oben nicht leere Farbe:

| Modus       | Ziel | Schnittmenge | Source |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | Source       | X      |
| `Dst`      | X           | Ziel  |        |
| `SrcOver`  | X           | Source       | X      |
| `DstOver`  | X           | Ziel  | X      |
| `SrcIn`    |             | Source       |        |
| `DstIn`    |             | Ziel  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | Source       |        |
| `DstATop`  |             | Ziel  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | Summe          | X      |
| `Modulate` |             | Produkt      |        | 

Diese Füllmethoden sind symmetrisch. Die Quelle und Ziel können ausgetauscht werden, und allen Modi stehen weiterhin zur Verfügung.

Die Namenskonvention der Modi,: einige einfache Regeln

- **Src** oder **Dst** allein bedeutet, dass nur die Quelle oder Ziel Pixel sichtbar sind.
- Die **über** -Suffix gibt an, was in der Schnittmenge angezeigt wird. Die Quelle oder das Ziel wird "sink" gezeichnet.
- Die **In** Suffix bedeutet, dass nur die Schnittmenge gefärbt ist. Die Ausgabe ist auf nur den Teil der Quelle oder Ziel "in" anderen beschränkt.
- Die **Out** Suffix bedeutet, dass die Schnittmenge nicht gefärbt ist. Die Ausgabe ist nur der Teil der Quelle oder Ziel, das die Schnittmenge "out" ist.
- Die **auf** Suffix ist die Vereinigung der **In** und **Out**. Es enthält den Bereich, in denen die Quelle oder das Ziel "auf" ist, der jeweils anderen.

Beachten Sie, dass den Unterschied bei der `Plus` und `Modulate` Modi. Diese Modi ausführen ein anderen Typs, der Berechnung auf den Quell- und Zielserver Pixel. Sie werden in Kürze ausführlicher beschrieben.

Die **Porter-Duff Raster** Seite zeigt alle 14-Modus auf einem Bildschirm in Form eines Rasters. Jeder Modus ist eine separate Instanz des `SKCanvasView`. Aus diesem Grund stammt aus eine Klasse `SKCanvasView` mit dem Namen `PorterDuffCanvasView`. Der statische Konstruktor erstellt zwei Bitmaps der gleichen Größe, eine mit einem brownish Rechteck in der oberen linken Bereich und eine mit einem bläuliches Rechteck:

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

Der Instanzkonstruktor weist einen Parameter vom Typ `SKBlendMode`. Dieser Parameter wird in einem Feld gespeichert. 

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

Die `OnPaintSurface` außer Kraft setzen, zeichnet die zwei Bitmaps. Die erste wird normalerweise gezeichnet werden:

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

Die zweite gezeichnet wird, mit einer `SKPaint` Objekt, in dem der `BlendMode` Eigenschaft auf das Konstruktorargument festgelegt wurde:

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

Der Rest der `OnPaintSurface` Außerkraftsetzung zeichnet ein Rechteck um die Bitmap an, dass ihre Größen angezeigt werden.

Die `PorterDuffGridPage` Klasse erstellt 14 Instanzen von `PorterDurffCanvasView`, eine für jedes Mitglied der `blendModes` Array. Die Reihenfolge der `SKBlendModes` Elemente im Array ist etwas anders als in der Tabelle, um ähnliche Modi nebeneinander positionieren. Die 14 Instanzen von `PorterDuffCanvasView` werden organisiert, zusammen mit den Bezeichnungen in einem `Grid`:

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

Hier ist das Ergebnis:

[![Porter-Duff Raster](porter-duff-images/PorterDuffGrid.png "Porter-Duff Raster")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Sie sollten sich selbst überzeugen, dass Transparenz entscheidend für das ordnungsgemäße Funktionieren der Porter-Duff Füllmethoden einheitlich ist. Die `PorterDuffCanvasView` -Klasse enthält insgesamt drei Aufrufe an die `Canvas.Clear` Methode. Alle verwenden die parameterlose Methode, die alle Pixel auf transparentes festlegt:

```csharp
canvas.Clear();
```

Versuchen Sie es ändern dieser Aufrufe, damit die Pixel auf ein nicht transparenter weiß festgelegt werden:

```csharp
canvas.Clear(SKColors.White);
```

Nach dieser Änderung werden einige der Füllmethoden einheitlich, scheint zu funktionieren, aber andere hingegen nicht. Setzen Sie den Hintergrund des Quellbitmaps in weiß, und klicken Sie dann die `SrcOver` Modus nicht funktionieren, da keine transparent Pixel in der Quell-Bitmap, die das Ziel durchscheinen zu lassen. Setzen Sie den Hintergrund der Zielbitmap oder den Zeichenbereich, um dann weiß `DstOver` funktioniert nicht, da das Ziel keine transparente Pixel.

Es gibt möglicherweise Versuchung, ersetzen Sie die Bitmaps in der **Porter-Duff Raster** Seite mit einfacher `DrawRect` Aufrufe. Dies wird für das Zielrechteck jedoch nicht für das Quellrechteck funktioniert. Das Quellrechteck muss mehr als nur den bläuliches farbig Bereich umfassen. Das Quellrechteck muss es sich um einen transparenten Bereich enthalten, der die die Farbiger Bereich, der das Ziel entspricht. Klicken Sie dann nur einfügen diese Modi arbeiten.

## <a name="using-mattes-with-porter-duff"></a>Verwenden von Masken mit Porter-Duff

Die zusammengesetzte Seite " **Brick-Wall Zusammensetzung** " zeigt ein Beispiel für eine klassische Zusammensetzung-Aufgabe: Ein Bild muss aus mehreren Teilen zusammengestellt werden, einschließlich einer Bitmap mit einem Hintergrund, der entfernt werden muss. Hier ist die **SeatedMonkey.jpg** Bitmap mit den problematischen Hintergrund:

![Sitzen Monkey](porter-duff-images/SeatedMonkey.jpg "sitzen Monkey-Objekt")

Als Vorbereitung für die Zusammensetzung zu einer entsprechenden _Matte_ erstellt wurde, dies ist eine andere Bitmap, die andernfalls Schwarz, wobei das Bild angezeigt werden soll und transparent ist. Diese Datei heißt **SeatedMonkeyMatte.png** und gehört zu den Ressourcen in der **Media** Ordner in der [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel :

![Sitzen Monkey Matte](porter-duff-images/SeatedMonkeyMatte.png "sitzen Monkey-Maske")

Dies ist _nicht_ ein fachbuch erstellten Maske. Optimal Maske sollte teilweise transparente Pixel entlang der Bildschirmkante die schwarze Pixel enthalten, und dieser Maske nicht.

Die XAML-Datei für die **Durchbruch Zusammensetzung** Seite instanziiert ein `SKCanvasView` und `Button` , die den Benutzer durch den Prozess zum Erstellen von das endgültige Bild führt:

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

Die Code-Behind-Datei lädt, die zwei Bitmaps, die er benötigt, und behandelt die `Clicked` Ereignis die `Button`. Für jede `Button` klicken, die `step` Feld wird erhöht, und es wird eine neue `Text` Eigenschaft wird festgelegt, für die `Button`. Wenn `step` 5, erreicht er wieder auf 0 festgelegt ist:

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

Beim ersten des Programms ausführen, wird nichts angezeigt, mit Ausnahme der `Button`:

[![Durchbruch Zusammensetzung ist Schritt 0](porter-duff-images/BrickWallCompositing0.png "Durchbruch Zusammensetzung ist Schritt 0")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

Drücken Sie die `Button` einmal bewirkt, dass `step` um 1 erhöht und die `PaintSurface` Ereignishandler zeigt nun an **SeatedMonkey.jpg**:

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

Es gibt keine `SKPaint` Objekt und somit keine Blend-Modus. Die Bitmap wird am unteren Rand des Bildschirms angezeigt:

[![Schritt1 Durchbruch Zusammensetzung](porter-duff-images/BrickWallCompositing1.png "Durchbruch Zusammensetzung Schritt 1")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

Drücken Sie die `Button` erneut und `step` Schritten auf 2. Dies ist zum Anzeigen von wichtigen Schritt der **SeatedMonkeyMatte.png** Datei:

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

Blend-Modus ist `SKBlendMode.DstIn`, was bedeutet, dass das Ziel in Bereichen, nicht transparenten Bereiche der Quelle entspricht beibehalten wird. Der Rest des Zielrechtecks entspricht der ursprünglichen Bitmap wird transparent:

[![Schritt2 Durchbruch Zusammensetzung](porter-duff-images/BrickWallCompositing2.png "Durchbruch Zusammensetzung Schritt 2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

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

Da diese Fußweg hinter das Monkey-Objekt zurückkehren muss, ist Blend-Modus `DstOver`. Das Ziel wird nur angezeigt, in dem Hintergrund transparent ist:

[![Durchbruch Zusammensetzung Schritt 3](porter-duff-images/BrickWallCompositing3.png "Durchbruch Zusammensetzung Schritt 3")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

Der letzte Schritt einen Durchbruch hinzugefügt werden. Die Anwendung verwendet die Durchbruch Bitmap-Kachel zur Verfügung, als die statische Eigenschaft `BrickWallTile` in die `AlgorithmicBrickWallPage` Klasse. Eine Transformation für die Übersetzung wird hinzugefügt. die `SKShader.CreateBitmap` Aufruf, um die Kacheln verschoben, damit die letzte Zeile einer vollständigen Kachel ist:

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

Der Einfachheit halber die `DrawRect` Aufruf zeigt diesen Shader über den gesamten Zeichenbereich, aber die `DstOver` Modus schränkt die Ausgabe nur der Bereich des Zeichenbereichs, die immer noch transparent ist:

[![Durchbruch Zusammensetzung Schritt 4](porter-duff-images/BrickWallCompositing4.png "Durchbruch Zusammensetzung Schritt 4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

Es gibt natürlich weitere Möglichkeiten, diese Szene zu erstellen. Es kann erstellt werden, beginnend mit den Hintergrund und in den Vordergrund fortgesetzt. Jedoch können Sie mithilfe der Füllmethoden einheitlich flexibler. Insbesondere ermöglicht die Verwendung der Maske den Hintergrund einer Bitmap aus der zusammengesetzten Szene ausgeschlossen werden sollen.

Wie in diesem Artikel haben Sie gelernt [Schneiden mit Pfaden und Regionen](../../curves/clipping.md), `SKCanvas` -Klasse definiert drei Arten von Clipping, für die [ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect*), [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath*), und [ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion*) Methoden. Die Porter-Duff Füllmethoden einheitlich fügen Sie eine andere Art von Clipping, dadurch kann ein Bild auf alle Elemente, die Sie zeichnen können, einschließlich Bitmaps, zu beschränken. Die Maske verwendet **Durchbruch Zusammensetzung** im Wesentlichen eine Clippingbereichs definiert.

## <a name="gradient-transparency-and-transitions"></a>Farbverlauf Transparenz und Übergänge

Die Beispiele für die Porter-Duff Füllmethoden, die weiter oben gezeigt, in diesem Artikel alle Images beteiligt sein, die nicht transparenten Pixel und transparente Pixel, jedoch nicht teilweise transparente Pixel aus. Die Blend-Modus-Funktionen sind für diese Pixel ebenfalls definiert. Die folgende Tabelle enthält eine formalere Definition der Porter-Duff Füllmethoden einheitlich, die Notation finden Sie in der Skia verwendet [ **SkBlendMode Verweis**](https://skia.org/user/api/SkBlendMode_Reference). (Da **SkBlendMode Verweis** ist ein Verweis Skia, C++-Syntax wird verwendet.)

Im Prinzip werden die Komponenten roten, grünen, blauen und alpha der einzelnen Pixel von Bytes in Gleitkommazahlen im Bereich von 0 bis 1 konvertiert. Für den Alphakanal 0 vollständig transparent und 1 vollständig undurchsichtig ist

Die Notation in der folgenden Tabelle werden die folgenden Abkürzungen verwendet:

- **Da** ist der Ziel-alpha-Kanal
- **DC** ist das Ziel RGB-Farbe
- **SA** wird von der Quelle alpha-Kanal
- **SC** ist die Quelle RGB-Farbe

RGB-Farben werden vor dem Alphawert multipliziert. Z. B. wenn **Sc** reines Rot stellt jedoch **Sa** ist 0 x 80, dann ist die RGB-Farbe **(0 x 80, 0, 0)** . Wenn **Sa** ist 0, und klicken Sie dann alle RGB-Komponenten sind auch 0 (null).

Das Ergebnis wird angezeigt, in Klammern mit der alpha-Kanal und die RGB-Farbe, die durch ein Komma getrennt: **[Alphafarbe]** . Für die Farbe erfolgt die Berechnung für die Komponenten roten, grünen und blauen getrennt:

| Modus       | Vorgang |
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

Diese Vorgänge sind einfacher, wenn analysieren **Da** und **Sa** sind entweder 0 oder 1. Z. B. für die Standardeinstellung `SrcOver` Modus, wenn **Sa** gleich 0 ist, klicken Sie dann **Sc** ist auch 0, und das Ergebnis ist **[Da, Dc]** , die Ziel-Alpha und die Farbe. Wenn **Sa** 1 ist, lautet das Ergebnis **[Sa, Sc]** , die Alpha für Quelle und die Farbe, oder **[1, Sc]** .

Die `Plus` und `Modulate` Modi unterscheiden sich geringfügig von den anderen, neue Farben aus der Kombination von der Quelle und Ziel führen können. Die `Plus` Modus interpretiert werden kann, entweder mit Byte oder Gleitkomma-Komponenten. In der **Porter-Duff Raster** Seite, die zuvor gezeigte, wird der Zielfarbe **("0xC0", 0 x 80, 0 x 00)** und die Quellfarbe **(0 x 00, 0 x 80, "0xC0")** . Jedes Paar von Komponenten hinzugefügt, aber die Summe wird am 0xFF gebunden sind. Das Ergebnis ist die Farbe **("0xC0", 0xFF "0xC0")** . Dies ist die Farbe, die in die Schnittmenge angezeigt.

Für die `Modulate` -Modus die RGB-Werte müssen zu Gleitkommazahl konvertiert werden. Ist der Zielfarbe **(0,75, 0,5, 0)** und die Quelle **(0, 0,5, 0,75)** . Die RGB-Komponenten sind miteinander multipliziert jede, und das Ergebnis ist **("0", "0.25", "0")** . Dies ist der Farbe angezeigt, in der Schnittmenge in der **Porter-Duff Raster** Seite für diesen Modus.

Die **Porter-Duff Transparenz** auf der Seite können Sie untersuchen, wie die Porter-Duff Füllmethoden einheitlich für grafische Objekte verwendet werden, die teilweise transparent sind. Die XAML-Datei enthält eine `Picker` mit den Porter-Duff Modi:

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

[![Porter-Duff Transparenz](porter-duff-images/PorterDuffTransparency.png "Porter-Duff Transparenz")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

Die Konfiguration der Quelle und Ziel ähnelt in den Diagrammen angezeigt werden, auf 255, der die ursprüngliche Porter-Duff [ _Zusammensetzung Digitalbilder_ ](https://graphics.pixar.com/library/Compositing/paper.pdf) Papier, aber diese Seite zeigt, dass die Füllmethoden einheitlich sind für die teilweise transparente Bereiche Speicherzeigern.

Sie können für einige unterschiedliche Auswirkungen transparente Farbverläufe verwenden. Eine Möglichkeit ist maskiert, die ähnelt die Technik in der [ **radialen Farbverläufen für die Maskierung** ](../shaders/circular-gradients.md#radial-gradients-for-masking) Teil der **SkiaSharp zirkuläre Farbverläufe Seite**. Anteil der **Zusammensetzung Maske** Seite ähnelt früheren Programms. Lädt eine Bitmapressource und bestimmt ein Rechteck, in dem es angezeigt werden soll. Ein Radialer Farbverlauf wird auf einem vorher festgelegten Center und die RADIUS-Basis erstellt:

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

Der Unterschied bei der dieses Programm ist, dass der Farbverlauf mit Schwarz, in der Mitte mit beginnt und Transparenz endet. Es wird angezeigt, auf die Bitmap mit einem Blend-Modus von `DstIn`, zeigt das Ziel nur in den Bereichen der Quelle, die nicht transparent sind.

Nach der `DrawRect` Aufruf, der die gesamte Oberfläche der Leinwand ist transparent, mit Ausnahme der Kreis von radialen Farbverlaufs definiert. Letzte wird aufgerufen:

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

Die transparenten Bereiche des Zeichenbereichs werden rosa gefärbt:

[![Zusammensetzung Maske](porter-duff-images/CompositingMask.png "Compositing-Maske")](porter-duff-images/CompositingMask-Large.png#lightbox)

Sie können auch Porter-Duff Modi und teilweise transparente Gradienten für Übergänge aus einem Bild in einen anderen verwenden. Die **Farbverlauf Übergänge** Seite enthält eine `Slider` an eine Status-Ebene in der Übergang von 0 auf 1 fest, und ein `Picker` um den Typ des Übergangs auszuwählen:

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

Die Code-Behind-Datei lädt zwei Bitmapressourcen, um den Übergang zu veranschaulichen. Hierbei handelt es sich um die gleichen beiden Images in verwendet die **Bitmap auflösen** Seite oben in diesem Artikel. Der Code definiert außerdem eine Enumeration mit drei Membern, die drei Typen von Farbverläufen für &mdash; linear und Radial, und löschen. Diese Werte werden geladen, in der `Picker`:

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

Die Code-Behind-Datei erstellt drei `SKPaint` Objekte. Die `paint0` Objekt keinen Blend-Modus verwenden. Dieses Paint-Objekt wird verwendet, um ein Rechteck mit einem Farbverlauf zu zeichnen, die von Schwarz festgelegt, um transparent in aufgeführt wird die `colors` Array. Die `positions` Array basiert darauf, dass die Position des der `Slider`, aber ein wenig angepasst. Wenn die `Slider` an dessen Minimum oder Maximum, wird die `progress` Werte sind 0 oder 1, und eine der beiden Bitmaps vollständig angezeigt werden. Die `positions` Array muss entsprechend festgelegt werden, für die Werte.

Wenn die `progress` Wert ist 0 (null) und dann die `positions` Array die Werte-0.1 und 0 enthält. SkiaSharp wird angepasst, diesen ersten Wert gleich 0 ist, was bedeutet, dass der Farbverlauf nur bei 0 Schwarz ist und transparent andernfalls. Wenn `progress` ist ' 0,5 ', und klicken Sie dann das Array enthält die Werte 0,45 und 0,55. Der Gradient ist schwarz von 0, 0,45, und klicken Sie dann transparent übergeht, und ist vollständig transparent von 0,55 auf 1. Wenn `progress` 1 ist, die `positions` Array ist 1- und 1.1, das bedeutet, dass der Gradient ist schwarz von 0 auf 1.

Die `colors` und `position` Arrays werden die drei Methoden der `SKShader` , die einen Farbverlauf erstellen. Nur eines dieser Shader auf der Basis erstellen die `Picker` Auswahl: 

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

Dieser Verlauf wird in das Rechteck ohne Mischmodus angezeigt. Danach `DrawRect` Aufruf wird im Zeichenbereich enthält lediglich einen Farbverlauf von Schwarz transparent. Die Menge an Schwarz steigt mit höheren `Slider` Werte.

In den letzten vier Anweisungen von der `PaintSurface` Handler auf, die zwei Bitmaps angezeigt werden. Die `SrcOut` Blendingmodus bedeutet, dass nur in den transparenten Bereichen des Hintergrunds die erste Bitmap angezeigt wird. Die `DstOver` Modus für die zweite Bitmap bedeutet, dass die zweite Bitmap, die nur in diesen Bereichen angezeigt wird, in dem die erste Bitmap wird nicht angezeigt.

Die folgenden Screenshots zeigen die drei Typen von verschiedenen Übergänge, jede mit der Markierung von 50 %:

[![Farbverlauf Übergänge](porter-duff-images/GradientTransitions.png "Farbverlauf Übergänge")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
