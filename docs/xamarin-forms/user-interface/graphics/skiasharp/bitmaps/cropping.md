---
title: Zuschneiden von SkiaSharp-bitmaps
description: Erfahren Sie, wie SkiaSharp verwenden, um eine Benutzeroberfläche für interaktive unterteilt ein zuschnittrechteck zu entwerfen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: cf31f3bd6f84a040d21420e865737417c374d947
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61078480"
---
# <a name="cropping-skiasharp-bitmaps"></a>Zuschneiden von SkiaSharp-bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Die [ **erstellen und zeichnen SkiaSharp Bitmaps** ](drawing.md) Artikel beschrieben, wie ein `SKBitmap` Objekt übergeben werden, um eine `SKCanvas` Konstruktor. Jeder zeichnen-Methode wird aufgerufen, auf die Grafiken, Canvas bewirkt, dass auf die Bitmap gerendert werden soll. Einzubeziehen Zeichnungsmethoden `DrawBitmap`, was bedeutet, dass diese Technik ermöglicht das Übertragen von teilweise oder vollständig eine Bitmap in einem anderen Bitmap, vielleicht mit Transformationen angewendet.

Können Sie diese Methode für das Abschneiden einer Bitmaps durch Aufrufen der [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) -Methode mit der Quelle und Ziel Rechtecke:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

Anwendungen, die implementieren oft zuschneiden, bieten jedoch eine Schnittstelle für den Benutzer auf das zuschnittrechteck interaktiv auszuwählen:

![Zuschneiden Beispiel](cropping-images/CroppingSample.png "Beispiel Zuschneiden")

Dieser Artikel konzentriert sich auf dieser Schnittstelle.

## <a name="encapsulating-the-cropping-rectangle"></a>Kapselt das zuschnittrechteck

Es ist hilfreich, einen Teil der Zuschneiden Logik in einer Klasse namens isolieren `CroppingRectangle`. Parameter des Konstruktors enthalten eine maximale Rechteck, das in der Regel wird die Größe der Bitmap zugeschnitten wird, und eine optionale Seitenverhältnis beibehalten. Der Konstruktor definiert zuerst eine anfängliche zuschnittrechteck, dadurch wird es öffentlich, in der `Rect` Eigenschaft vom Typ `SKRect`. Dieser anfängliche zuschnittrechteck ist 80 % der Breite und Höhe des Rechtecks Bitmap, aber es wird dann angepasst, wenn ein Seitenverhältnis angegeben wird:

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }
    
    public SKRect Rect { set; get; }
    ···
}
```

Eine nützliche Information, `CroppingRectangle` verfügbar gemacht ist auch ein Array von `SKPoint` Werte, die den vier Ecken des das zuschnittrechteck in der Reihenfolge, die linke obere, rechts, rechts und unten links:

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

Dieses Array wird in der folgenden Methode, die aufgerufen wird, verwendet `HitTest`. Die `SKPoint` -Parameter ist ein Punkt, eine Fingereingabe Finger entspricht, oder klicken mit der Maus. Die Methode gibt einen Index (0, 1, 2 oder 3) zurück in die Ecke, die der Finger oder der Maus Zeiger bearbeitet wird, innerhalb einer Entfernung vom entsprechenden die `radius` Parameter: 

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];
                
            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

Wenn der Punkt oder nicht in `radius` Einheiten von einer beliebigen Ecke der Methodenrückgabe &ndash;1.

Die letzte Methode in `CroppingRectangle` heißt `MoveCorner`, die aufgerufen wird, als Reaktion auf berühren oder mit der Maus verschieben. Die zwei Parameter geben den Index der Ecke verschoben werden und die neue Position der dieser Ecke. Die erste Hälfte der Methode passt das zuschnittrechteck basierend auf den neuen Speicherort der Ecke, aber immer innerhalb der Grenzen des `maxRect`, d.h. die Größe der Bitmap. Diese Logik auch berücksichtigt die `MINIMUM` Feld, um zu vermeiden, reduzieren das zuschnittrechteck in "nothing":

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

Die zweite Hälfte der Methode passt für das optionale Seitenverhältnis wird beibehalten.

Bedenken Sie, die alles, was in dieser Klasse in Pixel.

## <a name="a-canvas-view-just-for-cropping"></a>Eine Canvas-Ansicht nur für das Zuschneiden

Die `CroppingRectangle` Klasse, die Sie haben gerade gesehen wird verwendet, durch die `PhotoCropperCanvasView` -Klasse, die abgeleitet `SKCanvasView`. Diese Klasse dient zum Anzeigen von Bitmap und das zuschnittrechteck sowie behandeln oder Ereignisse für das zuschnittrechteck ändern.

Die `PhotoCropperCanvasView` -Konstruktor erfordert eine Bitmap. Ein Seitenverhältnis ist optional. Der Konstruktor instanziiert ein Objekt des Typs `CroppingRectangle` basierend auf dieser Bitmap und das Originalseitenverhältnis beibehalten, und speichert es als ein Feld:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

Da von dieser Klasse abgeleitet wird `SKCanvasView`, sie muss nicht installieren einen Handler für die `PaintSurface` Ereignis. Sie können stattdessen außer Kraft setzen die `OnPaintSurface` Methode. Die Methode wird in der Bitmap und verwendet eine Reihe von `SKPaint` Objekte als Felder, die zum Zeichnen der aktuellen zuschnittrechteck gespeichert:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap 
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

Der Code in die `CroppingRectangle` Klasse verwendet als Basis für das zuschnittrechteck Größe in Pixel der Bitmap. Jedoch die Anzeige der Bitmap für die von der `PhotoCropperCanvasView` Klasse auf der Größe des Anzeigebereichs basierend skaliert wird. Die `bitmapScaleMatrix` berechnete in die `OnPaintSurface` ordnet die Pixel auf die Größe und Position der Bitmap zu überschreiben, wie er angezeigt wird. Die Matrix wird dann zum das zuschnittrechteck zu verwandeln, sodass es Bezug auf die Bitmap angezeigt werden kann.

Die letzte Zeile des der `OnPaintSurface` Überschreibung verwendet die Umkehrung der der `bitmapScaleMatrix` und speichert sie als die `inverseBitmapMatrix` Feld. Dies wird für die Touch-Verarbeitung verwendet.

Ein `TouchEffect` Objekt als Feld instanziiert wird, und der Konstruktor fügt einen Handler, der die `TouchAction` -Ereignis, aber die `TouchEffect` muss hinzugefügt werden die `Effects` Auflistung von der _übergeordneten_ von der `SKCanvasView`Ableitung, sodass erfolgt ist, der `OnParentSet` außer Kraft setzen:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking 
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex, 
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

Die touchereignisse verarbeitet, durch die `TouchAction` Ereignishandler befinden sich in geräteunabhängigen Einheiten. Diese zunächst in Pixel mit konvertiert werden die `ConvertToPixel` Methode am unteren Rand der Klasse, und klicken Sie dann in `CroppingRectangle` Einheiten mithilfe von `inverseBitmapMatrix`.

Für `Pressed` Ereignisse, die `TouchAction` Ereignishandler ruft die `HitTest` -Methode der `CroppingRectangle`. Wenn einen Index zurückgegeben wird als &ndash;1, klicken Sie dann eine der Ecken des Rechtecks Zuschneiden ist, das manipuliert wird. Dass der Index und einem Offset von der tatsächlichen Touch-Punkt in der Ecke befindet sich in einem `TouchPoint` Objekt hinzugefügt, und wählen Sie die `touchPoints` Wörterbuch.

Für die `Moved` -Ereignis, das `MoveCorner` -Methode der `CroppingRectangle` wird aufgerufen, um die Ecke, mit möglichen Anpassungen für das Seitenverhältnis zu verschieben.

Jederzeit eine Anwendung mit `PhotoCropperCanvasView` stehen die `CroppedBitmap` Eigenschaft. Diese Eigenschaft verwendet die `Rect` Eigenschaft der `CroppingRectangle` um eine neue Bitmap der zugeschnittenen Größe zu erstellen. Die Version des `DrawBitmap` mit Ziel- und Rechtecke klicken Sie dann eine Teilmenge der ursprünglichen Bitmap extrahiert:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width, 
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top, 
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Hosten die Foto Cropper Canvas-Ansicht

Mit diesen zwei Klassen, die das Zuschneiden Anwendungslogik die **Fotos Zuschneiden** auf der Seite die **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** Anwendung befindet sich nur sehr wenig. Die XAML-Datei instanziiert ein `Grid` auf Host der `PhotoCropperCanvasView` und **Fertig** Schaltfläche:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

Die `PhotoCropperCanvasView` kann nicht in der XAML-Datei instanziiert werden, da es sich um einen Parameter vom Typ erfordert `SKBitmap`.

Stattdessen die `PhotoCropperCanvasView` im Konstruktor der CodeBehind-Datei mit dem eine der Bitmaps Ressource instanziiert wird:

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

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
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Anschließend kann der Benutzer das zuschnittrechteck bearbeitet werden:

[![Foto Cropper 1](cropping-images/PhotoCropping1.png "Foto Cropper 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

Wenn ein guter zuschnittrechteck definiert wurde, klicken Sie auf die **Fertig** Schaltfläche. Die `Clicked` Handler Ruft das zugeschnittene Bitmap aus der `CroppedBitmap` Eigenschaft `PhotoCropperCanvasView`, und ersetzt alle Inhalte der Seite mit einem neuen `SKCanvasView` -Objekt, das diese zugeschnittene Bitmap anzeigt:

[![Foto Cropper 2](cropping-images/PhotoCropping2.png "Foto Cropper 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

Legen Sie das zweite Argument der `PhotoCropperCanvasView` zu 1.78f (z. B.):

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

Sie sehen, dass das zuschnittrechteck auf ein 16-zu-9-Seitenverhältnis von High Definition Television beschränkt.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>Eine Bitmap unterteilen in Kacheln

Eine Xamarin.Forms-Version der berühmtheiten gelangen 14-15-Rätsel angezeigt, in Kapitel 22 des Buchs [ _Erstellen mobiler Apps mit Xamarin.Forms_ ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) heruntergeladen werden können, als [  **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle). Das Rätsel wird jedoch mehr Spaß (und häufig schwieriger) Wenn sie auf ein Image aus Ihrer eigenen Fotobibliothek basiert.

Diese Version des Rätsels 14-15 ist Teil der **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** -Anwendung und besteht aus einer Reihe von Seiten, die mit dem Titel **Foto Rätsel**.

Die **PhotoPuzzlePage1.xaml** -Datei besteht aus einem `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">
    
    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand" 
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>
    
</ContentPage>
```

Die CodeBehind-Datei implementiert eine `Clicked` Ereignishandler, der mithilfe der `IPhotoLibrary` Abhängigkeitsdienst ermöglichen den Benutzer ein Foto aus der Fotobibliothek auswählen:

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

Die Methode navigiert dann zu `PhotoPuzzlePage2`, und übergeben Sie an den Konstruktor der ausgewählten Bitmap.

Es ist möglich, dass das Foto, das aus der Bibliothek ausgewählt, soweit es ist in der Fotobibliothek aufgetreten ist, gedreht oder nach unten zeigende nicht ausgerichtet ist. (Dies ist besonders dann ein Problem mit iOS-Geräten). Aus diesem Grund `PhotoPuzzlePage2` können Sie das Image in die gewünschte Ausrichtung zu rotieren. Die XAML-Datei enthält drei Schaltflächen, die mit der Bezeichnung **90&#x00B0; rechts** (d. h. im Uhrzeigersinn), **90&#x00B0; Links** (gegen den Uhrzeigersinn) und **Fertig**.

Die Code-Behind-Datei implementiert die Bitmap für die Rotation-Logik, die in diesem Artikel gezeigten  **[erstellen und Zeichnen auf SkiaSharp Bitmaps](drawing.md#rotating-bitmaps)**. Der Benutzer kann dem Bild um 90 Grad im Uhrzeigersinn oder gegen den Uhrzeigersinn oft drehen: 

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

Klickt der Benutzer die **Fertig** Schaltfläche der `Clicked` Handler navigiert zur `PhotoPuzzlePage3`, übergeben die endgültige gedreht Bitmap im Konstruktor der Seite.

`PhotoPuzzlePage3` ermöglicht das Foto zugeschnitten wird. Das Programm erfordert eine quadratische Bitmap in einem 4 x 4-Raster von Kacheln unterteilen.

Die **PhotoPuzzlePage3.xaml** -Datei enthält eine `Label`, `Grid` auf Host der `PhotoCropperCanvasView`, und ein weiteres **Fertig** Schaltfläche:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei instanziiert den `PhotoCropperCanvasView` mit der Bitmap, die an den Konstruktor übergeben. Beachten Sie, das eine 1, als das zweite Argument für übergeben wird `PhotoCropperCanvasView`. Dieses Verhältnis 1 erzwingt das zuschnittrechteck ein Quadrat sein:

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

Die **Fertig** Schaltflächenhandler, ruft die Breite und Höhe der Bitmap für die zugeschnittenen (diese beiden Werte sollten identisch sein), und teilt diese dann in 15 separate Bitmaps, von denen jeder die 1/4 ist die Breite und Höhe des Originals. (Das letzte mögliche 16 Bitmaps wird nicht erstellt.) Die `DrawBitmap` -Methode mit der Quelle und Ziel-Rechteck ermöglicht eine Bitmap, die basierend auf den Teil einer größeren Bitmap erstellt werden.

## <a name="converting-to-xamarinforms-bitmaps"></a>Konvertieren in Xamarin.Forms-bitmaps

In der `OnDoneButtonClicked` -Methode, das für die 15 Bitmaps erstellte Array ist, vom Typ [ `ImageSource` ](xref:Xamarin.Forms.ImageSource):

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource` ist der Basistyp für Xamarin.Forms, der eine Bitmap kapselt. Glücklicherweise kann SkiaSharp, Konvertieren von Bitmaps von SkiaSharp in Xamarin.Forms-Bitmaps. Die **SkiaSharp.Views.Forms** Assembly definiert eine [ `SKBitmapImageSource` ](xref:SkiaSharp.Views.Forms.SKBitmapImageSource) abgeleitete Klasse `ImageSource` können jedoch erstellt werden basierend auf einer SkiaSharp `SKBitmap` Objekt. `SKBitmapImageSource` definiert auch Konvertierungen zwischen `SKBitmapImageSource` und `SKBitmap`, und das ist wie `SKBitmap` Objekte werden in einem Array als Xamarin.Forms Bitmaps gespeichert:

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

Dieses Array von Bitmaps als einen Konstruktor zu übergeben wird `PhotoPuzzlePage4`. Diese Seite ist eine vollständig Xamarin.Forms und keine SkiaSharp. Es ist sehr ähnlich [ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle), daher wird nicht hier beschrieben werden, es wird jedoch Ihre ausgewählten Fotos in 15 quadratische Kacheln unterteilt:

[![Foto Rätsel 1](cropping-images/PhotoPuzzle1.png "Foto Rätsel 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

Drücken Sie die **Randomize** Schaltfläche kombiniert, um alle Kacheln:

[![Foto Rätsel 2](cropping-images/PhotoPuzzle2.png "Foto Rätsel 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

Jetzt können Sie sie in der richtigen Reihenfolge wieder platzieren. Alle Kacheln in der gleichen Zeile oder Spalte als das leere Quadrat können getippt werden, um sie in das leere Quadrat zu verschieben. 

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
