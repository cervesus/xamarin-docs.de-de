---
title: SkiaSharp-Bitmap-Kacheln
description: Kachel einen Bereich mithilfe von Bitmaps vertikal und horizontal wiederholt.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9ED14E07-4DC8-4B03-8A33-772838BF51EA
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 7d323aa6616f7547ab91dfe2b394c339e273d61c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650025"
---
# <a name="skiasharp-bitmap-tiling"></a>SkiaSharp-Bitmap-Kacheln

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)

Wie Sie in den zwei vorherigen Artikel gesehen haben die [ `SKShader` ](xref:SkiaSharp.SKShader) -Klasse lineare oder zirkuläre Farbverläufe erstellt werden können. Dieser Artikel konzentriert sich auf die `SKShader` -Objekt, das eine Bitmap wird ein Bereich verwendet. Die Bitmap horizontal und vertikal wiederholt werden kann entweder in der ursprünglichen Ausrichtung oder alternativ gekippt horizontal und vertikal. Die kippen vermeidet Diskontinuitäten zwischen den Kacheln:

![Bitmapbeispiel Tiling](bitmap-tiling-images/BitmapTilingSample.png "Tiling Bitmapbeispiel")

Die statische [ `SKShader.CreateBitmap` ](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode)) Methode, die diesen Shader erstellt hat eine `SKBitmap` Parameter und zwei Member der [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) Enumeration:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

Die zwei Parameter geben die Modi für die horizontale Unterteilung und vertikalen Tiling verwendet. Dies ist das gleiche `SKShaderTileMode` -Enumeration, die auch mit den Farbverlauf-Methoden verwendet wird.

Ein [ `CreateBitmap` ](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung umfasst ein `SKMatrix` Argument an eine Transformation auf die unterteilten Bitmaps ausführen:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

Dieser Artikel enthält einige Beispiele für angeordnete Bitmaps mit dieser Matrixtransformation.

## <a name="exploring-the-tile-modes"></a>Untersuchen die Kachel "-Modi

Das erste Programm in der **Bitmap Tiling** Teil der **Shader und andere Effekte** auf der Seite die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel veranschaulicht die Auswirkungen der beiden `SKShaderTileMode` Argumente. Die **Bitmap Kachelmodi kippen** XAML-Datei instanziiert ein `SKCanvasView` und zwei `Picker` Ansichten, mit denen Sie auswählen, können eine `SKShaderTilerMode` Wert für die horizontale und vertikale Tiling. Beachten Sie, dass ein Array von der `SKShaderTileMode` Member definiert ist, der `Resources` Abschnitt:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapTileFlipModesPage"
             Title="Bitmap Tile Flip Modes">

    <ContentPage.Resources>
        <x:Array x:Key="tileModes"
                 Type="{x:Type skia:SKShaderTileMode}">
            <x:Static Member="skia:SKShaderTileMode.Clamp" />
            <x:Static Member="skia:SKShaderTileMode.Repeat" />
            <x:Static Member="skia:SKShaderTileMode.Mirror" />
        </x:Array>
    </ContentPage.Resources>

    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="xModePicker"
                Title="Tile X Mode"
                Margin="10, 0"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

        <Picker x:Name="yModePicker"
                Title="Tile Y Mode"
                Margin="10, 10"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

    </StackLayout>
</ContentPage>
```

Der Konstruktor der CodeBehind-Datei lädt, in der Bitmapressource, die eine ungenutzte Monkey anzeigt. Es zuerst schneidet das Bild mithilfe der [ `ExtractSubset` ](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI)) -Methode der `SKBitmap` so, dass die Kopf und Fuß die Ränder der Bitmap berührt werden. Dann verwendet der Konstruktor der [ `Resize` ](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod)) Methode, um einen anderen Bitmap der Hälfte der Größe zu erstellen. Diese Änderungen stellen die Bitmap für Kacheln etwas besser geeignet:

```csharp
public partial class BitmapTileFlipModesPage : ContentPage
{
    SKBitmap bitmap;

    public BitmapTileFlipModesPage ()
    {
        InitializeComponent ();

        SKBitmap origBitmap = BitmapExtensions.LoadBitmapResource(
            GetType(), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

        // Define cropping rect
        SKRectI cropRect = new SKRectI(5, 27, 296, 260);

        // Get the cropped bitmap
        SKBitmap croppedBitmap = new SKBitmap(cropRect.Width, cropRect.Height);
        origBitmap.ExtractSubset(croppedBitmap, cropRect);

        // Resize to half the width and height
        SKImageInfo info = new SKImageInfo(cropRect.Width / 2, cropRect.Height / 2);
        bitmap = croppedBitmap.Resize(info, SKBitmapResizeMethod.Box);
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

        // Get tile modes from Pickers
        SKShaderTileMode xTileMode =
            (SKShaderTileMode)(xModePicker.SelectedIndex == -1 ?
                                        0 : xModePicker.SelectedItem);
        SKShaderTileMode yTileMode =
            (SKShaderTileMode)(yModePicker.SelectedIndex == -1 ?
                                        0 : yModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap, xTileMode, yTileMode);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Die `PaintSurface` Handler Ruft die `SKShaderTileMode` Einstellungen aus den beiden `Picker` Ansichten und erstellt eine `SKShader` -Objekt auf Grundlage der Bitmap und diese beiden Werte. Dieser Shader verwendet wird, auf den Zeichenbereich ausfüllen:

[![Bitmap-Flip Tile-Modi](bitmap-tiling-images/BitmapTileFlipModes.png "Bitmap-Flip Tile-Modi")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

IOS-Bildschirms auf der linken Seite zeigt die Auswirkungen der die Standardwerte der `SKShaderTileMode.Clamp`. Die Bitmap befindet sich in der oberen linken Ecke. Klicken Sie unten die Bitmap wird die untersten Zeile der Pixel, ganz nach unten wiederholt. Rechts neben der Bitmap wird die Spalte ganz rechts Pixeln ganz über wiederholt. Der Rest des Zeichenbereichs wird in der Bitmap in der unteren rechten Ecke das dunkle braunen Pixel gefärbt. Es sollte klar sein, die die `Clamp` Option fast nie zusammen mit Bitmap Tiling!

Die Android-Bildschirm in der Mitte, zeigt das Ergebnis der `SKShaderTileMode.Repeat` für beide Argumente. Die Kachel wird horizontal und vertikal wiederholt. Die universelle Windows-Plattform Bildschirm zeigt `SKShaderTileMode.Mirror`. Die Kacheln werden wiederholt, aber Alternativ gekippt horizontal und vertikal. Der Vorteil dieser Option ist, dass keine Diskontinuitäten zwischen den Kacheln.

Bedenken Sie, dass Sie verschiedene Optionen für die horizontale und vertikale Wiederholung verwenden können. Sie können angeben, `SKShaderTileMode.Mirror` als das zweite Argument für `CreateBitmap` aber `SKShaderTileMode.Repeat` als drittes Argument. Für jede Zeile sind die Affen weiterhin wechseln Sie zwischen den normalen Image und die Spiegelung, aber keines der Affen Kopf.

## <a name="patterned-backgrounds"></a>Gemusterten Hintergründen

Bitmap-Tiling wird häufig verwendet, um einen gemusterten Hintergrund aus einer relativ kleinen Bitmap zu erstellen. Das klassische Beispiel ist eine Durchbruch.

Die **algorithmische Durchbruch** -Seite erstellt eine kleine Bitmap, die eine gesamte Brick und beiden Hälften eines Bricks, getrennt durch Internetshop ähnelt. Da dieses Brick in auch im nächsten Beispiel verwendet wird, ist es von einem statischen Konstruktor erstellt und mit einer statischen Eigenschaft öffentlich gemacht:

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    static AlgorithmicBrickWallPage()
    {
        const int brickWidth = 64;
        const int brickHeight = 24;
        const int morterThickness = 6;
        const int bitmapWidth = brickWidth + morterThickness;
        const int bitmapHeight = 2 * (brickHeight + morterThickness);

        SKBitmap bitmap = new SKBitmap(bitmapWidth, bitmapHeight);

        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint brickPaint = new SKPaint())
        {
            brickPaint.Color = new SKColor(0xB2, 0x22, 0x22);

            canvas.Clear(new SKColor(0xF0, 0xEA, 0xD6));
            canvas.DrawRect(new SKRect(morterThickness / 2,
                                       morterThickness / 2,
                                       morterThickness / 2 + brickWidth,
                                       morterThickness / 2 + brickHeight),
                                       brickPaint);

            int ySecondBrick = 3 * morterThickness / 2 + brickHeight;

            canvas.DrawRect(new SKRect(0,
                                       ySecondBrick,
                                       bitmapWidth / 2 - morterThickness / 2,
                                       ySecondBrick + brickHeight),
                                       brickPaint);

            canvas.DrawRect(new SKRect(bitmapWidth / 2 + morterThickness / 2,
                                       ySecondBrick,
                                       bitmapWidth,
                                       ySecondBrick + brickHeight),
                                       brickPaint);
        }

        // Save as public property for other programs
        BrickWallTile = bitmap;
    }

    public static SKBitmap BrickWallTile { private set; get; }
    ···
}
```

Die resultierende Bitmap ist 70 Pixel breit und 60 Pixel hoch:

![Algorithmische Brick Wall Kachel](bitmap-tiling-images/AlgorithmicBrickWallTile.png "algorithmische Brick Wall-Kachel")

Die restlichen den **algorithmische Durchbruch** -Seite erstellt eine `SKShader` -Objekt, das dieses Bild horizontal und vertikal wiederholt:

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    ···
    public AlgorithmicBrickWallPage ()
    {
        Title = "Algorithmic Brick Wall";

        // Create SKCanvasView
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
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(BrickWallTile,
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Hier ist das Ergebnis:

[![Algorithmische Durchbruch](bitmap-tiling-images/AlgorithmicBrickWall.png "algorithmische Durchbruch")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

Möglicherweise ein etwas realistischeres bevorzugen. In diesem Fall können Sie ein Foto des eine tatsächliche Durchbruch dauern und es zuschneiden. Diese Bitmap ist 300 Pixel breit und 150 Pixel hoch:

![Brick Wall Kachel](bitmap-tiling-images/BrickWallTile.jpg "Brick Wall-Kachel")

Diese Bitmap wird verwendet, der **Photographic Durchbruch** Seite:

```csharp
public class PhotographicBrickWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PhotographicBrickWallPage),
                        "SkiaSharpFormsDemos.Media.BrickWallTile.jpg");

    public PhotographicBrickWallPage()
    {
        Title = "Photographic Brick Wall";

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
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Beachten Sie, dass die `SKShaderTileMode` Argumente `CreateBitmap` sind beide `Mirror`. Diese Option ist in der Regel erforderlich, bei der Verwendung von Kacheln, die aus realen Images erstellt wurden. Die Kacheln Spiegelung vermeidet Diskontinuitäten:

[![Photographic Durchbruch](bitmap-tiling-images/PhotographicBrickWall.png "Photographic Durchbruch")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

Einige Aufgaben ist erforderlich, um eine geeignete Bitmap für die Kachel zu erhalten. Dieser funktioniert nicht sehr gut, da die dunkleren Brick sich hebt zuviel. Es wird regelmäßig innerhalb der wiederholten Bilder, die Offenlegung durch die Tatsache, dass diese Durchbruch aus keine kleinere Bitmap erstellt wurde.

Die **Media** Ordner die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel umfasst auch dieses Bild einer Stone Wand:

![Stein Wall Kachel](bitmap-tiling-images/StoneWallTile.jpg "Stein Wall-Kachel")

Die ursprüngliche Bitmap ist jedoch ein wenig zu groß für eine Kachel. Sie können die geändert werden, aber die `SKShader.CreateBitmap` Methode kann auch durch Anwenden einer Transformation, Größe der Kachel. Diese Option wird veranschaulicht, der **Stein Wand** Seite:

```csharp
public class StoneWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(StoneWallPage),
                        "SkiaSharpFormsDemos.Media.StoneWallTile.jpg");

    public StoneWallPage()
    {
        Title = "Stone Wall";

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
            // Create scale transform
            SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);

            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror,
                                                 matrix);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Ein `SKMatrix` Wert erstellt, um das Bild, um die Hälfte der ursprünglichen Größe zu skalieren:

[![Minimale Wand](bitmap-tiling-images/StoneWall.png "minimale Wand")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

Kann die Transformation ausgeführt werden, auf der ursprünglichen Bitmap in verwendet die `CreateBitmap` Methode? Oder das resultierende Array der Kacheln transformieren? 

Eine einfache Möglichkeit zur Beantwortung dieser Frage ist eine Drehung im Rahmen der Transformation eingeschlossen:

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

Wenn die Transformation auf die einzelnen Kachel angewendet wird, klicken Sie dann jedes wiederholtes Bild der Kachel gedreht werden soll, und das Ergebnis würde viele Diskontinuitäten enthalten. Aber es ergibt sich aus dieser Screenshot zeigt, dass das kombinierte Array der Kacheln transformiert wird:

[![Minimale Wand gedreht](bitmap-tiling-images/StoneWallRotated.png "minimale Wand gedreht")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

Im Abschnitt [ **Kachel Ausrichtung**](#tile-alignment), sehen Sie ein Beispiel für TranslateTransform an den Shader angewendet.

Die eigenständige [ **Cat Uhr** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock) Beispiel (nicht Teil der **SkiaSharpFormsDemos**) simuliert einen Bitmap-Kacheln, die auf der Grundlage dieser quadratischen 240 Pixel-Bitmap mit Holz differenziertere-Hintergrund:

![Aggregationsintervall Holz](bitmap-tiling-images/WoodGrain.png "Holz Granularität")

Dies ist ein Foto des Holz Untergrenze. Die `SKShaderTileMode.Mirror` Option können sie als einen viel größeren Bereich des Holz angezeigt werden:

[![CAT Uhr](bitmap-tiling-images/CatClock.png "Cat Uhr")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>Kachel-Ausrichtung

Alle Beispiele, die bisher gezeigten hätte den Shader erstellt `SKShader.CreateBitmap` behandeln den gesamten Zeichenbereich. In den meisten Fällen verwenden Sie Bitmap-Kacheln für die Archivierung kleinere Bereiche, oder (selten mehr) für das füllen das Innere der thick-Zeilen. Hier ist die Kachel "photographic Durchbruch" für ein kleineres Rechteck verwendet:

[![Kachel Ausrichtung](bitmap-tiling-images/TileAlignment.png "Kachel Ausrichtung")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

Dies kann in Ordnung, oder vielleicht auch nicht vorkommen. Vielleicht sind Sie nicht gestört, dass das Kachelmuster nicht mit einer vollständigen Bricks in der oberen linken Ecke des Rechtecks beginnt. Der Grund Shader ausgerichtet sind, mit der Leinwand und nicht das grafisches Objekt, das sie Verzieren ist.

Die Lösung ist einfach. Erstellen Sie eine `SKMatrix` -Wert auf Grundlage einer Übersetzung-Transformation. Die Transformation wird effektiv der Kachelmuster verlagert, zu dem Punkt der oberen linken Ecke der Kachel ausgerichtet werden sollen. Dieser Ansatz wird veranschaulicht, der **Kachel Ausrichtung** Seite, die das Image der oben gezeigten nicht ausgerichteten Kacheln erstellt:

```csharp
public class TileAlignmentPage : ContentPage
{
    bool isAligned;

    public TileAlignmentPage()
    {
        Title = "Tile Alignment";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Add tap handler
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isAligned ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

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
            SKRect rect = new SKRect(info.Width / 7,
                                     info.Height / 7,
                                     6 * info.Width / 7,
                                     6 * info.Height / 7);

            // Get bitmap from other program
            SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;

            // Create bitmap tiling
            if (!isAligned)
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
            }
            else
            {
                SKMatrix matrix = SKMatrix.MakeTranslation(rect.Left, rect.Top);

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     matrix);
            }

            // Draw rectangle
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Die **Kachel Ausrichtung** Seite enthält eine `TapGestureRecognizer`. Tippen oder klicken Sie auf dem Bildschirm und das Programm wechselt zu den `SKShader.CreateBitmap` -Methode mit einem `SKMatrix` Argument. Diese Transformation verschiebt das Muster, damit die oberen linken Ecke eines vollständigen Bricks enthält:

[![Kachel getippt Ausrichtung](bitmap-tiling-images/TileAlignmentTapped.png "Kachel getippt Ausrichtung")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

Sie können dieses Verfahren auch verwenden, um sicherzustellen, dass der gekachelten Bitmapmuster innerhalb des Bereichs, zentriert wird, die sie zeichnet. In der **zentriert Kacheln** Seite die `PaintSurface` Handler berechnet zuerst Koordinaten, als ob es geht, um die einzelnen Bitmap in der Mitte des Zeichenbereichs angezeigt. Er verwendet dann diese Koordinaten zum Erstellen von TranslateTransform für `SKShader.CreateBitmap`. Diese Transformation verschiebt das gesamte Muster, damit eine Kachel zentriert ist:

```csharp
public class CenteredTilesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.monkey.png");

    public CenteredTilesPage ()
    {
        Title = "Centered Tiles";

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

        // Find coordinates to center bitmap in canvas...
        float x = (info.Width - bitmap.Width) / 2f;
        float y = (info.Height - bitmap.Height) / 2f;

        using (SKPaint paint = new SKPaint())
        {
            // ... but use them to create a translate transform
            SKMatrix matrix = SKMatrix.MakeTranslation(x, y);
            paint.Shader = SKShader.CreateBitmap(bitmap, 
                                                 SKShaderTileMode.Repeat, 
                                                 SKShaderTileMode.Repeat, 
                                                 matrix);

            // Use that tiled bitmap pattern to fill a circle
            canvas.DrawCircle(info.Rect.MidX, info.Rect.MidY,
                              Math.Min(info.Width, info.Height) / 2,
                              paint);
        }
    }
}
```

Die `PaintSurface` Handler wird abgeschlossen, indem das Zeichnen eines Kreises in der Mitte der Canvas. Natürlich eine der Kacheln wird genau in der Mitte des Kreises, und die anderen werden in einem Muster eines symmetrischen angeordnet:

[![Zentriert Kacheln](bitmap-tiling-images/CenteredTiles.png "zentriert Kacheln")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

Ein anderer zentrieren Ansatz ist tatsächlich etwas einfacher. Anstatt TranslateTransform zu erstellen, die eine Kachel in der Mitte setzt, können Sie eine Ecke des der Kachelmuster zentrieren. In der `SKMatrix.MakeTranslation` aufzurufen, verwenden Sie die Argumente für die Mitte der Canvas:

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

Das Muster ist weiterhin zentriert und symmetrisch, aber keine Kachel ist in der Mitte:

[![Zentriert Kacheln alternative](bitmap-tiling-images/CenteredTilesAlternate.png "Kacheln alternative zentriert")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>Vereinfachung durch Drehung

Verwenden manchmal eine Rotationstransformation in die `SKShader.CreateBitmap` Methode kann die Kachel "Bitmap" vereinfachen. Dies wird deutlich, bei dem Versuch, eine Kachel für einen Fence Kette-Link zu definieren. Die **ChainLinkTile.cs** -Datei erstellt, die Kachel, die hier gezeigten (mit einem rosa Hintergrund zur Veranschaulichung):

![Kette die hardlink-Kachel](bitmap-tiling-images/HardChainLinkTile.png "Kette die hardlink-Kachel")

Die Kachel "muss zwei Links enthalten, damit der Code die Kachel in vier Quadranten unterteilt. Die linke obere und der unteren rechten Quadranten sind identisch, aber sie sind nicht vollständig. Die Drähten haben wenig Stufen, die verarbeitet werden müssen, mit einigen zusätzlichen Zeichnen in der oberen rechten und linken unteren Quadranten. Die Datei, die all diese Arbeit ist 174 Zeilen lang.

Es stellt sich heraus, viel einfacher, diese Kachel erstellt werden:

![Kachel "einfacher Kette-Link"](bitmap-tiling-images/EasierChainLinkTile.png "einfacher Kette-Link-Kachel")

Wenn die Bitmap-Kachel-Shader um 90 Grad gedreht wird, ist die visuellen Elemente nahezu identisch.

Der Code zum Erstellen der Kachel "einfacheren Kette-Link" ist Teil der **Kette-Link-Kachel** Seite. Der Konstruktor bestimmt eine Größe basierend auf den Typ des Geräts, das das Programm ausgeführt wird, und ruft dann `CreateChainLinkTile`, die zeichnet auf der Bitmap mit Zeilen, Pfade und Farbverlauf-Shader:

```csharp
public class ChainLinkFencePage : ContentPage
{
    ···
    SKBitmap tileBitmap;

    public ChainLinkFencePage ()
    {
        Title = "Chain-Link Fence";

        // Create bitmap for chain-link tiling
        int tileSize = Device.Idiom == TargetIdiom.Desktop ? 64 : 128;
        tileBitmap = CreateChainLinkTile(tileSize);

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    SKBitmap CreateChainLinkTile(int tileSize)
    {
        tileBitmap = new SKBitmap(tileSize, tileSize);
        float wireThickness = tileSize / 12f;

        using (SKCanvas canvas = new SKCanvas(tileBitmap))
        using (SKPaint paint = new SKPaint())
        {
            canvas.Clear();
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = wireThickness;
            paint.IsAntialias = true;

            // Draw straight wires first
            paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                         new SKPoint(0, tileSize),
                                                         new SKColor[] { SKColors.Silver, SKColors.Black },
                                                         new float[] { 0.4f, 0.6f },
                                                         SKShaderTileMode.Clamp);

            canvas.DrawLine(0, tileSize / 2,
                            tileSize / 2, tileSize / 2 - wireThickness / 2, paint);

            canvas.DrawLine(tileSize, tileSize / 2,
                            tileSize / 2, tileSize / 2 + wireThickness / 2, paint);

            // Draw curved wires
            using (SKPath path = new SKPath())
            {
                path.MoveTo(tileSize / 2, 0);
                path.LineTo(tileSize / 2 - wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 + wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.Silver, SKColors.Black },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);

                path.Reset();
                path.MoveTo(tileSize / 2, tileSize);
                path.LineTo(tileSize / 2 + wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 - wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.White, SKColors.Silver },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);
            }
            return tileBitmap;
        }
    }
    ···
}
```

Mit Ausnahme der verbindet ist die Kachel transparent, was bedeutet, dass Sie ihn auf etwas anderes anzeigen können. Das Programm in eine Bitmap-Ressource lädt, zeigt an, um den Zeichenbereich ausfüllen und zeichnet dann oben auf den Shader:

```csharp
public class ChainLinkFencePage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(ChainLinkFencePage), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");
    ···

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.UniformToFill, 
                            BitmapAlignment.Center, BitmapAlignment.Start);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(tileBitmap, 
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat,
                                                 SKMatrix.MakeRotationDegrees(45));
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Beachten Sie, dass der Shader den Wert um 45 Grad gedreht, damit es wie einen echten Kette-Link-Fence ausgerichtet ist:

[![Kette-Link-Fence](bitmap-tiling-images/ChainLinkFence.png "Kette-Link-Umgrenzung")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>Die Animation von Bitmap-Kacheln

Sie können eine Muster für die gesamte Bitmap-Kachel der Matrixtransformation Animation animieren. Vielleicht möchten Sie das entsprechende Muster, die horizontal oder vertikal zu verschieben oder beides. Sie können dazu erstellen eine Übersetzung-Transformation, die basierend auf den wechselnden Koordinaten.

Es ist auch möglich, um auf eine kleine Bitmap zeichnen oder zum Bearbeiten von Pixelbits für die Bitmap mit einer Rate von 60-Mal pro Sekunde. Diese Bitmap kann für Kacheln verwendet werden, und die gesamte Kachelmuster kann scheinen animiert werden. 

Die **Kachel "Bitmap" animiert** Seite veranschaulicht diesen Ansatz. Eine Bitmap wird als ein Feld für die 64-Pixel im Quadrat werden instanziiert. Der Konstruktor ruft `DrawBitmap` um er einen anfänglichen aussehen zu verleihen. Wenn die `angle` Feld ist NULL (wie beim ersten der Methode Aufruf), dann das Bitmuster zwei Zeilen, die ein "X" überschritten enthält. Die Zeilen erfolgen lang genug, um immer an den Rand der Bitmap unabhängig von erreichen die `angle` Wert: 

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    const int SIZE = 64;

    SKCanvasView canvasView;
    SKBitmap bitmap = new SKBitmap(SIZE, SIZE);
    float angle;
    ···

    public AnimatedBitmapTilePage ()
    {
        Title = "Animated Bitmap Tile";

        // Initialize bitmap prior to animation
        DrawBitmap();

        // Create SKCanvasView 
        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
    void DrawBitmap()
    {
        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = SIZE / 8;

            canvas.Clear();
            canvas.Translate(SIZE / 2, SIZE / 2);
            canvas.RotateDegrees(angle);
            canvas.DrawLine(-SIZE, -SIZE, SIZE, SIZE, paint);
            canvas.DrawLine(-SIZE, SIZE, SIZE, -SIZE, paint);
        }
    }
    ···
}
```

Der Aufwand für die Animation tritt auf, der `OnAppearing` und `OnDisappearing` überschreibt. Die `OnTimerTick` Methode erstellt eine Animation die `angle` -Wert von 0 Grad und 360 Grad alle 10 Sekunden in der Abbildung X innerhalb der Bitmap drehen:

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    ···

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
        const int duration = 10;     // seconds
        angle = (float)(360f * (stopwatch.Elapsed.TotalSeconds % duration) / duration);
        DrawBitmap();
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Aufgrund der die Symmetrie der Abbildung X, dies entspricht dem Drehen der `angle` Wert von 0 Grad bis 90 Grad 2,5 Sekunden.

Die `PaintSurface` Handler erstellt einen Shader aus der Bitmap und verwendet die Paint-Objekt, um die Farbe, die des gesamten Zeichenbereichs:

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Die `SKShaderTileMode.Mirror` Optionen stellen Sie sicher, dass die Arme, der das X in jede Bitmap, mit dem X in die angrenzende Bitmaps einbinden, um ein allgemeines animiertes Muster zu erstellen, die viel scheint komplexer als einfache Animation hinweist:

[![Kachel "Bitmap" animiert](bitmap-tiling-images/AnimatedBitmapTile.png "animiert Bitmap-Kachel")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [CatClock (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)
