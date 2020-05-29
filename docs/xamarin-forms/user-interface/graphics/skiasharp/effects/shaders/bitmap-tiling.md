---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6a28dd20eb8978334365ac217df1241e5288fd28
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137422"
---
# <a name="skiasharp-bitmap-tiling"></a>Skiasharp-Bitmap-tiult

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)

Wie Sie in den beiden vorherigen Artikeln gesehen haben, kann die- [`SKShader`](xref:SkiaSharp.SKShader) Klasse lineare oder zirkuläre Farbverläufe erstellen. Dieser Artikel konzentriert sich auf das- `SKShader` Objekt, das eine Bitmap verwendet, um einen Bereich zu Kacheln. Die Bitmap kann horizontal und vertikal wiederholt werden, entweder in der ursprünglichen Ausrichtung oder alternativ horizontal und vertikal. Beim Kippen werden Diskontinuitäten zwischen den Kacheln vermieden:

![Beispiel für Bitmap-tiult](bitmap-tiling-images/BitmapTilingSample.png "Beispiel für Bitmap-tiult")

Die statische [`SKShader.CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode)) Methode, die diesen Shader erstellt, verfügt über einen `SKBitmap` -Parameter und zwei Member der- [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) Enumeration:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

Die beiden Parameter geben die Modi an, die für horizontales und vertikales ticken verwendet werden. Dabei handelt es sich um dieselbe `SKShaderTileMode` Enumeration, die auch mit den Farbverlaufs Methoden verwendet wird.

Eine [`CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung enthält ein `SKMatrix` Argument zum Ausführen einer Transformation für die gekachelten Bitmaps:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

Dieser Artikel enthält mehrere Beispiele für die Verwendung dieser Matrix Transformation mit gekachelten Bitmaps.

## <a name="exploring-the-tile-modes"></a>Untersuchen der Kachel Modi

Das erste Programm im Abschnitt **Bitmap-tiult** der Seite **Shader und andere Effekte** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel veranschaulicht die Auswirkungen der beiden `SKShaderTileMode` Argumente. Die XAML-Datei der **bitmapkachel-Flip-Modi** instanziiert eine `SKCanvasView` und zwei `Picker` Sichten, die es Ihnen ermöglichen, einen `SKShaderTilerMode` Wert für horizontales und vertikales ticken auszuwählen. Beachten Sie, dass ein Array der Member `SKShaderTileMode` im-Abschnitt definiert ist `Resources` :

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

Der Konstruktor der Code Behind-Datei wird in der Bitmap-Ressource geladen, die eine affensitzung anzeigt. Zuerst wird das Bild mithilfe der- [`ExtractSubset`](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI)) Methode von `SKBitmap` , sodass die Kopfzeile und die Fußzeile die Ränder der Bitmap berühren. Der Konstruktor verwendet dann die- [`Resize`](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod)) Methode, um eine weitere Bitmap der Hälfte der Größe zu erstellen. Diese Änderungen machen die Bitmap etwas passender für das Ticken:

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

Der `PaintSurface` Handler Ruft die `SKShaderTileMode` Einstellungen aus den beiden `Picker` Ansichten ab und erstellt ein `SKShader` Objekt auf der Grundlage der Bitmap und dieser beiden Werte. Dieser Shader wird verwendet, um den Zeichenbereich auszufüllen:

[![Flip-Modi der Bitmap-Kachel](bitmap-tiling-images/BitmapTileFlipModes.png "Flip-Modi der Bitmap-Kachel")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

Der IOS-Bildschirm links zeigt die Auswirkung der Standardwerte von `SKShaderTileMode.Clamp` . Die Bitmap befindet sich in der oberen linken Ecke. Unterhalb der Bitmap wird die untere Zeile der Pixel wiederholt. Rechts von der Bitmap wird die Spalte ganz rechts von Pixel wiederholt. Der Rest der Canvas wird durch das dunkle braune Pixel in der unteren rechten Ecke der Bitmap gefärbt. Es sollte offensichtlich sein, dass die `Clamp` Option fast nie mit Bitmap-tiches verwendet wird.

Der Android-Bildschirm in der Mitte zeigt das Ergebnis von `SKShaderTileMode.Repeat` für beide Argumente. Die Kachel wird horizontal und vertikal wiederholt. Der Bildschirm universelle Windows-Plattform wird angezeigt `SKShaderTileMode.Mirror` . Die Kacheln werden wiederholt, aber abwechselnd horizontal und vertikal gekippt. Der Vorteil dieser Option besteht darin, dass es keine Diskontinuitäten zwischen den Kacheln gibt.

Denken Sie daran, dass Sie für die horizontale und vertikale Wiederholung verschiedene Optionen verwenden können. Sie können `SKShaderTileMode.Mirror` als zweites Argument für angeben, `CreateBitmap` jedoch `SKShaderTileMode.Repeat` als drittes Argument. In jeder Zeile wechseln die Affen weiterhin zwischen dem normalen Bild und dem Spiegelbild, aber keine der Affen ist von oben nach unten.

## <a name="patterned-backgrounds"></a>Gemusterte Hintergründe

Bitmap-tiult wird häufig verwendet, um einen gemusterten Hintergrund aus einer relativ kleinen Bitmap zu erstellen. Das klassische Beispiel ist eine Brick-Wand.

Auf der Seite **algorithmische Brick Wall** wird eine kleine Bitmap erstellt, die einem ganzen Brick und zwei Hälften eines Bricks durch Mörtel getrennt ähnelt. Da dieses Brick auch im nächsten Beispiel verwendet wird, wird es von einem statischen Konstruktor erstellt und mit einer statischen Eigenschaft öffentlich gemacht:

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

![Algorithmische Brick Wall-Kachel](bitmap-tiling-images/AlgorithmicBrickWallTile.png "Algorithmische Brick Wall-Kachel")

Im restlichen Abschnitt der **algorithmischen Brick Wall** -Seite wird ein `SKShader` Objekt erstellt, das dieses Bild horizontal und vertikal wiederholt:

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

Das Ergebnis lautet wie folgt:

[![Algorithmische Ziegelwand](bitmap-tiling-images/AlgorithmicBrickWall.png "Algorithmische Ziegelwand")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

Möglicherweise bevorzugen Sie etwas realistischeres. In diesem Fall können Sie ein Foto einer eigentlichen Brick-Wand erstellen und diese dann zuschneiden. Diese Bitmap ist 300 Pixel breit und 150 Pixel hoch:

![Ziegelwand Kachel](bitmap-tiling-images/BrickWallTile.jpg "Ziegelwand Kachel")

Diese Bitmap wird auf der Seite für die **Foto Ziegel Wall** verwendet:

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

Beachten Sie, dass die `SKShaderTileMode` Argumente für `CreateBitmap` beide sind `Mirror` . Diese Option ist normalerweise erforderlich, wenn Sie Kacheln verwenden, die aus realen Bildern erstellt wurden. Die Spiegelung der Kacheln vermeidet Diskontinuitäten:

[![Foto Ziegelwand](bitmap-tiling-images/PhotographicBrickWall.png "Foto Ziegelwand")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

Einige Arbeiten sind erforderlich, um eine passende Bitmap für die Kachel zu erhalten. Diese Funktion funktioniert nicht sehr gut, da das dunklere Brick zu viel hervorsticht. Sie wird regelmäßig innerhalb der wiederholten Bilder angezeigt und zeigt die Tatsache an, dass diese Brick-Wand aus einer kleineren Bitmap erstellt wurde.

Der **Medien** Ordner des [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiels enthält auch das Bild einer Stein Wand:

![Kachel "Stein Wand"](bitmap-tiling-images/StoneWallTile.jpg "Kachel "Stein Wand"")

Die ursprüngliche Bitmap ist jedoch für eine Kachel etwas zu groß. Die Größe kann geändert werden, aber die- `SKShader.CreateBitmap` Methode kann auch die Größe der Kachel ändern, indem Sie eine Transformation anwendet. Diese Option wird auf der Seite " **Stein Wand** " veranschaulicht:

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

Ein- `SKMatrix` Wert wird erstellt, um das Bild auf die Hälfte seiner ursprünglichen Größe zu skalieren:

[![Stein Wand](bitmap-tiling-images/StoneWall.png "Stein Wand")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

Arbeitet die Transformation mit der ursprünglichen Bitmap, die in der-Methode verwendet wird `CreateBitmap` ? Oder wandelt er das resultierende Array von Kacheln um? 

Eine einfache Möglichkeit, diese Frage zu beantworten, besteht darin, eine Drehung als Teil der Transformation einzuschließen:

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

Wenn die Transformation auf die einzelne Kachel angewendet wird, sollte jedes wiederholte Bild der Kachel gedreht werden, und das Ergebnis würde viele Diskontinuitäten enthalten. In diesem Screenshot ist jedoch ersichtlich, dass das zusammengesetzte Array von Kacheln transformiert wird:

[![Stein Wand gedreht](bitmap-tiling-images/StoneWallRotated.png "Stein Wand gedreht")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

Im Abschnitt [**Kachel Ausrichtung**](#tile-alignment)sehen Sie ein Beispiel für eine Translation-Transformation, die auf den Shader angewendet wird.

Das Beispiel für die eigenständige [**Cat-Uhr**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock) (nicht Teil von **skiasharpformsdemos**) simuliert einen Hintergrund mit der Bitmap, die auf dieser 240-Pixel-quadratischen Bitmap basiert:

![Holzkorn](bitmap-tiling-images/WoodGrain.png "Holzkorn")

Das ist ein Foto eines Holzbodens. Die `SKShaderTileMode.Mirror` Option ermöglicht es, in einem viel größeren Bereich von Holz zu erscheinen:

[![Cat-Uhr](bitmap-tiling-images/CatClock.png "Cat-Uhr")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>Kachel Ausrichtung

Alle bisher gezeigten Beispiele haben den Shader verwendet, der von erstellt wurde `SKShader.CreateBitmap` , um den gesamten Zeichenbereich abzudecken. In den meisten Fällen verwenden Sie Bitmap-tiult zum Ablegen kleinerer Bereiche oder (seltener) zum Auffüllen des Inneren von Thick Lines. Hier sehen Sie die Kachel "Photo Brick-Wall", die für ein kleineres Rechteck verwendet wird:

[![Kachel Ausrichtung](bitmap-tiling-images/TileAlignment.png "Kachel Ausrichtung")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

Dies kann für Sie gut oder nicht möglich sein. Vielleicht sind Sie beunruhigt, dass das Tick Muster nicht mit einem vollständigen Brick in der oberen linken Ecke des Rechtecks beginnt. Das liegt daran, dass Shader an der Canvas und nicht an dem grafischen Objekt ausgerichtet sind, das Sie schmücken.

Die Problembehebung ist einfach. Erstellen Sie einen- `SKMatrix` Wert, der auf einer Übersetzungs Transformation basiert. Die Transformation verschiebt das Kachel Muster effektiv an den Punkt, an dem die obere linke Ecke der Kachel ausgerichtet werden soll. Diese Vorgehensweise wird auf der Seite **Kachel Ausrichtung** veranschaulicht, die das Bild der oben gezeigten nicht ausgerichteten Kacheln erstellt hat:

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

Die Seite **Kachel Ausrichtung** enthält eine `TapGestureRecognizer` . Tippen oder klicken Sie auf den Bildschirm, und das Programm wechselt zur `SKShader.CreateBitmap` Methode mit einem `SKMatrix` Argument. Diese Transformation verschiebt das Muster so, dass die obere linke Ecke ein vollständiges Brick enthält:

[![Kachel Ausrichtung getippt](bitmap-tiling-images/TileAlignmentTapped.png "Kachel Ausrichtung getippt")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

Sie können dieses Verfahren auch verwenden, um sicherzustellen, dass das gekachelte Bitmap-Muster in dem Bereich zentriert ist, den es zeichnet. Auf der Seite " **zentrierte Kacheln** " `PaintSurface` berechnet der Handler zuerst die Koordinaten so, als ob er die einzelne Bitmap in der Mitte der Canvas anzeigen soll. Anschließend werden diese Koordinaten verwendet, um eine Translation-Transformation für zu erstellen `SKShader.CreateBitmap` . Diese Transformation verlagert das gesamte Muster, sodass eine Kachel zentriert ist:

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

Der `PaintSurface` Handler schließt das Zeichnen eines Kreises in der Mitte der Canvas. Und die anderen Kacheln sind genau in der Mitte des Kreises und die anderen in einem symmetrischen Muster angeordnet:

[![Zentrierte Kacheln](bitmap-tiling-images/CenteredTiles.png "Zentrierte Kacheln")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

Ein weiterer zentrieren-Ansatz ist wirklich etwas einfacher. Anstatt eine Translation-Transformation zu erstellen, die eine Kachel in der Mitte einfügt, können Sie eine Ecke des gekachelten Musters zentrieren. Verwenden Sie im-Befehl `SKMatrix.MakeTranslation` Argumente für den Mittelpunkt der Canvas:

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

Das Muster ist weiterhin zentriert und symmetrisch, aber es ist keine Kachel in der Mitte:

[![Alternative zentrierte Kacheln](bitmap-tiling-images/CenteredTilesAlternate.png "Alternative zentrierte Kacheln")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>Vereinfachung durch Drehung

Manchmal kann die Bitmap-Kachel durch die Verwendung einer Transformation zum Drehen in der- `SKShader.CreateBitmap` Methode vereinfacht werden. Dies wird offensichtlich, wenn versucht wird, eine Kachel für einen Ketten Link-Fence zu definieren. Die Datei **ChainLinkTile.cs** erstellt die hier gezeigte Kachel (mit einem rosa Hintergrund zum Zweck der Übersichtlichkeit):

![Kachel "hardchain-Link"](bitmap-tiling-images/HardChainLinkTile.png "Kachel "hardchain-Link"")

Die Kachel muss zwei Links enthalten, sodass der Code die Kachel in vier Quadranten dividiert. Die oberen linken und unteren rechten Quadranten sind identisch, aber Sie sind nicht fertig. Die Drähte haben nur wenige Zeichen, die mit einigen zusätzlichen Zeichen in den Quadranten oben rechts und Links unten behandelt werden müssen. Die Datei, die alle diese Aufgaben erledigt, ist 174 Zeilen lang.

Es ist viel einfacher, diese Kachel zu erstellen:

![Einfachere Ketten Link Kachel](bitmap-tiling-images/EasierChainLinkTile.png "Einfachere Ketten Link Kachel")

Wenn der Bitmap-Kachel-Shader um 90 Grad gedreht wird, sind die visuellen Elemente beinahe identisch.

Der Code zum Erstellen der einfacheren Ketten Link Kachel ist Teil der Seite " **Ketten Link Kachel** ". Der Konstruktor bestimmt eine Kachel Größe basierend auf dem Gerätetyp, auf dem das Programm ausgeführt wird, und ruft dann `CreateChainLinkTile` auf, das die Bitmap mithilfe von Linien, Pfaden und Gradient-Shadern zeichnet:

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

Mit Ausnahme der Drähte ist die Kachel transparent, was bedeutet, dass Sie Sie auf einer anderen Stelle anzeigen können. Das Programm wird in eine der Bitmapressourcen geladen, zeigt es an, um den Zeichenbereich auszufüllen, und zeichnet dann den Shader auf der obersten Ebene:

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

Beachten Sie, dass der Shader 45 Grad gedreht wird, sodass er wie ein echter Ketten Link-Fence ausgerichtet ist:

[![Ketten Link-Fence](bitmap-tiling-images/ChainLinkFence.png "Ketten Link-Fence")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>Animieren von Bitmap-Kacheln

Sie können ein vollständiges Bitmap-Kachel Muster animieren, indem Sie die Matrix Transformation animieren. Vielleicht möchten Sie, dass das Muster horizontal oder vertikal oder beides verläuft. Dies können Sie erreichen, indem Sie eine Übersetzungs Transformation auf der Grundlage der verschiebenden Koordinaten erstellen.

Es ist auch möglich, auf eine kleine Bitmap zu zeichnen oder die Pixel Bits der Bitmap mit der Rate von 60 mal pro Sekunde zu bearbeiten. Diese Bitmap kann dann für das Ticken verwendet werden, und das gesamte Kachel Muster kann als animiert erscheinen. 

Auf der Seite **animierte Bitmap-Kachel** wird dieser Ansatz veranschaulicht. Eine Bitmap wird als Feld mit dem Quadrat 64-Pixel instanziiert. Der Konstruktor ruft `DrawBitmap` auf, um ihm eine anfängliche Darstellung zu verschaffen. Wenn das `angle` Feld 0 (null) ist (wie beim ersten Aufruf der-Methode), enthält die Bitmap zwei Zeilen, die als X überschritten werden. Die Zeilen werden so lange so gestaltet, dass Sie unabhängig vom Wert immer an den Rand der Bitmap gelangen `angle` : 

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

Der Aufwand für die Animation erfolgt in den `OnAppearing` über schreibungen und `OnDisappearing` . Die- `OnTimerTick` Methode animiert den `angle` Wert von 0 Grad bis 360 Grad alle 10 Sekunden, um die X-Abbildung innerhalb der Bitmap zu drehen:

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

Aufgrund der Symmetrie der X-Abbildung entspricht dies dem Drehen des `angle` Werts von 0 Grad auf 90 Grad alle 2,5 Sekunden.

Der `PaintSurface` Handler erstellt einen Shader aus der Bitmap und verwendet das Paint-Objekt, um den gesamten Zeichenbereich zu färben:

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

Mit den- `SKShaderTileMode.Mirror` Optionen wird sichergestellt, dass die Arme der x-Datei in jeder Bitmap mit dem x in den angrenzenden Bitmaps verknüpft werden, um ein umfassendes animiertes Muster zu erstellen, das weitaus komplexer erscheint als die einfache Animation:

[![Animierte Bitmap-Kachel](bitmap-tiling-images/AnimatedBitmapTile.png "Animierte Bitmap-Kachel")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Catcher (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)
