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
ms.openlocfilehash: 6c5e340818b702d79a1157f29c1ecec19bf1db76
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139944"
---
# <a name="cropping-skiasharp-bitmaps"></a>Zuschneiden von skiasharp-Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Im Artikel [**Erstellen und Zeichnen von skiasharp-Bitmaps**](drawing.md) wird beschrieben, wie ein `SKBitmap` Objekt an einen Konstruktor übergeben werden kann `SKCanvas` . Jede Zeichnungs Methode, die auf dieser Canvas aufgerufen wird, bewirkt, dass Grafiken auf der Bitmap gerendert werden. Diese Zeichnungs Methoden enthalten `DrawBitmap` , was bedeutet, dass diese Technik das übertragen eines Teils oder aller einer Bitmap auf eine andere Bitmap ermöglicht, möglicherweise mit angewendeten Transformationen.

Sie können diese Technik zum Zuschneiden einer Bitmap verwenden, indem Sie die [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) -Methode mit Quell-und Ziel Rechtecke aufrufen:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

Anwendungen, die das Zuschneiden implementieren, bieten jedoch häufig eine Schnittstelle für den Benutzer, um das zuschnittrechteck interaktiv auszuwählen:

![Beispiel für das Zuschneiden](cropping-images/CroppingSample.png "Beispiel für das Zuschneiden")

Dieser Artikel konzentriert sich auf diese Schnittstelle.

## <a name="encapsulating-the-cropping-rectangle"></a>Kapseln des zuschneigsrechtecks

Es ist hilfreich, einen Teil der Zustellungs Logik in einer Klasse mit dem Namen zu isolieren `CroppingRectangle` . Die Konstruktorparameter enthalten ein maximales Rechteck, bei dem es sich in der Regel um die Größe der zugeschnittenen Bitmap handelt, sowie um ein optionales Seitenverhältnis. Der Konstruktor definiert zuerst ein anfängliches zuschneigsrechteck, das in der- `Rect` Eigenschaft des-Typs öffentlich ist `SKRect` . Dieses anfängliche Zuordnungs Rechteck ist 80% der Breite und Höhe des bitmaprechtecks, wird jedoch angepasst, wenn ein Seitenverhältnis angegeben wird:

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

Ein nützliches Informationselement, das `CroppingRectangle` auch verfügbar gemacht wird, ist ein Array von `SKPoint` Werten, die den vier Ecken des zuschnittrechtecks in der linken oberen linken, oberen rechten, unteren rechten und unteren linken Seite entsprechen:

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

Dieses Array wird in der folgenden Methode verwendet, die aufgerufen wird `HitTest` . Der- `SKPoint` Parameter ist ein Punkt, der einem Finger Touch oder einem Mausklick entspricht. Die-Methode gibt einen Index (0, 1, 2 oder 3) zurück, der der Ecke entspricht, die der Finger oder Mauszeiger berührt hat, innerhalb eines vom-Parameter angegebenen Abstands `radius` : 

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

Wenn sich der Fingerabdruck oder der Mauszeiger nicht innerhalb `radius` der Einheiten einer beliebigen Ecke befand, gibt die Methode &ndash; 1 zurück.

Die letzte Methode in `CroppingRectangle` wird aufgerufen `MoveCorner` , die als Reaktion auf die Berührungs-oder Mausbewegung aufgerufen wird. Die beiden Parameter geben den Index der zu verschogenden Ecke und den neuen Speicherort dieser Ecke an. Die erste Hälfte der Methode passt das zuschneigsrechteck basierend auf der neuen Position der Ecke an, aber immer innerhalb der Grenzen von `maxRect` , d. h. der Größe der Bitmap. Diese Logik berücksichtigt auch das `MINIMUM` Feld, um zu vermeiden, dass das umschließende Rechteck in nichts reduziert wird:

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

In der zweiten Hälfte der Methode wird das optionale Seitenverhältnis angepasst.

Beachten Sie, dass alles in dieser Klasse in Pixel Einheiten liegt.

## <a name="a-canvas-view-just-for-cropping"></a>Eine Canvas-Ansicht nur zum Zuschneiden

Die `CroppingRectangle` Klasse, die Sie gerade gesehen haben, wird von der- `PhotoCropperCanvasView` Klasse verwendet, die von abgeleitet wird `SKCanvasView` . Diese Klasse ist verantwortlich für die Anzeige der Bitmap und des Zuordnungs Rechtecks sowie für die Behandlung von Berührungs-oder Mausereignissen zum Ändern des zuschneigsrechtecks.

Der `PhotoCropperCanvasView` Konstruktor erfordert eine Bitmap. Ein Seitenverhältnis ist optional. Der-Konstruktor instanziiert ein Objekt vom Typ `CroppingRectangle` auf der Grundlage dieses Bitmap-und Seitenverhältnisses und speichert es als Feld:

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

Da diese Klasse von abgeleitet `SKCanvasView` wird, muss kein Handler für das-Ereignis installiert werden `PaintSurface` . Sie kann stattdessen die- `OnPaintSurface` Methode überschreiben. Die-Methode zeigt die Bitmap an und verwendet einige `SKPaint` Objekte, die als Felder zum Zeichnen des aktuellen zuschnittsrechtecks gespeichert werden:

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

Der Code in der- `CroppingRectangle` Klasse basiert auf dem zuschneigsrechteck auf der Pixelgröße der Bitmap. Die Anzeige der Bitmap durch die- `PhotoCropperCanvasView` Klasse wird jedoch auf Grundlage der Größe des Anzeige Bereichs skaliert. Der `bitmapScaleMatrix` , der in der Überschreibung berechnet `OnPaintSurface` wird, wird von den bitmappixeln der Größe und Position der Bitmap angezeigt, wenn Sie angezeigt wird. Diese Matrix wird dann verwendet, um das zuschneigsrechteck so zu transformieren, dass es relativ zur Bitmap angezeigt werden kann.

Die letzte Zeile der `OnPaintSurface` außer Kraft Setzung nimmt die Umkehrung von `bitmapScaleMatrix` und speichert Sie als- `inverseBitmapMatrix` Feld. Diese wird für die Berührungs Verarbeitung verwendet.

Ein `TouchEffect` -Objekt wird als-Feld instanziiert, und der-Konstruktor fügt einen Handler an das- `TouchAction` Ereignis an, muss jedoch der `TouchEffect` -Auflistung des übergeordneten Elements der Ableitung hinzugefügt werden `Effects` _parent_ `SKCanvasView` , sodass dies in der Überschreibung erfolgt `OnParentSet` :

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

Die vom Handler verarbeiteten Berührungs Ereignisse `TouchAction` sind geräteunabhängige Einheiten. Diese müssen zunächst mithilfe der `ConvertToPixel` -Methode am Ende der-Klasse in Pixel konvertiert und dann mithilfe von in Einheiten konvertiert werden `CroppingRectangle` `inverseBitmapMatrix` .

Für- `Pressed` Ereignisse `TouchAction` ruft der Handler die- `HitTest` Methode von auf `CroppingRectangle` . Wenn ein anderer Index als 1 zurückgegeben wird &ndash; , wird einer der Ecken des zuschneigsrechtecks bearbeitet. Dieser Index und ein Offset des eigentlichen Berührungs Punkts aus der Ecke werden in einem `TouchPoint` -Objekt gespeichert und dem `touchPoints` Wörterbuch hinzugefügt.

Für das- `Moved` Ereignis wird die- `MoveCorner` Methode von `CroppingRectangle` aufgerufen, um die Ecke mit möglichen Anpassungen für das Seitenverhältnis zu verschieben.

Ein Programm, das verwendet, kann jederzeit auf `PhotoCropperCanvasView` die- `CroppedBitmap` Eigenschaft zugreifen. Diese Eigenschaft verwendet die- `Rect` Eigenschaft des `CroppingRectangle` , um eine neue Bitmap der zugeschnittenen Größe zu erstellen. Die Version von `DrawBitmap` mit Ziel-und Quell Rechtecke extrahiert dann eine Teilmenge der ursprünglichen Bitmap:

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

## <a name="hosting-the-photo-cropper-canvas-view"></a>Hosting der Canvas-Ansicht des Foto-croppers

Mit diesen beiden Klassen, die die Zustellungs Logik verarbeiten, hat die Seite zum Zuschneiden von **Fotos** in der **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Anwendung nur wenig Arbeit. Die XAML-Datei instanziiert eine `Grid` zum Hosten von `PhotoCropperCanvasView` und eine **done** -Schaltfläche:

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

Die `PhotoCropperCanvasView` kann nicht in der XAML-Datei instanziiert werden, da Sie einen Parameter vom Typ erfordert `SKBitmap` .

Stattdessen wird der `PhotoCropperCanvasView` im Konstruktor der Code Behind-Datei mit einer der Ressourcen Bitmaps instanziiert:

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

Der Benutzer kann dann das zuschnittrechteck bearbeiten:

[![Fotocropper1](cropping-images/PhotoCropping1.png "Fotocropper1")](cropping-images/PhotoCropping1-Large.png#lightbox)

Wenn ein gutes zuschnittrechteck definiert wurde, klicken Sie auf die Schaltfläche **done** . Der `Clicked` Handler Ruft die abgeschnittene Bitmap aus der `CroppedBitmap` -Eigenschaft von ab `PhotoCropperCanvasView` und ersetzt den gesamten Inhalt der Seite durch ein neues- `SKCanvasView` Objekt, das diese abgeschnittene Bitmap anzeigt:

[![Photo cropper2](cropping-images/PhotoCropping2.png "Photo cropper2")](cropping-images/PhotoCropping2-Large.png#lightbox)

Versuchen Sie, das zweite Argument von `PhotoCropperCanvasView` auf 1,78 f festzulegen (z. b.):

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

Sie sehen, dass das zuschneigsrechteck auf ein 16-bis 9-Seitenverhältnis Merkmal von High-Definition-TV beschränkt ist.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>Aufteilen einer Bitmap in Kacheln

Xamarin.FormsIn Kapitel 22 des Buchs, das [_Mobile Apps mit xamarin. Forms erstellt_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) und als [**xamagonxuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)heruntergeladen werden kann, ist eine Version des berühmten 14-15-Rätsels aufgetreten. Das Rätsel wird jedoch mehr Spaß (und oft auch schwieriger), wenn es auf einem Image aus ihrer eigenen Fotobibliothek basiert.

Diese Version des 14-15-Puzzles ist Teil der Anwendung **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** und besteht aus einer Reihe von Seiten mit dem Titel **Photo Puzzle**.

Die Datei **PhotoPuzzlePage1. XAML** besteht aus einem `Button` :

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

Die Code-Behind-Datei implementiert einen `Clicked` Handler, der den `IPhotoLibrary` Abhängigkeits Dienst verwendet, um dem Benutzer das Auswählen eines Fotos aus der Fotobibliothek zu ermöglichen:

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

Die Methode navigiert dann zu `PhotoPuzzlePage2` und übergibt an den konstuktor der ausgewählten Bitmap.

Es ist möglich, dass das aus der Bibliothek ausgewählte Foto nicht so ausgerichtet ist, wie es in der Fotobibliothek enthalten ist, aber gedreht oder nach unten gedreht wird. (Dies ist insbesondere ein Problem mit IOS-Geräten.) Aus diesem Grund `PhotoPuzzlePage2` können Sie das Bild in eine gewünschte Ausrichtung drehen. Die XAML-Datei enthält drei Schaltflächen mit der Bezeichnung **90&#x00B0; rechts** (d.h. im Uhrzeigersinn), **90&#x00B0; Left** (gegen den Uhrzeigersinn) und **done**.

Die Code-Behind-Datei implementiert die Logik der Bitmap-Rotation, die im Artikel **[Erstellen und Zeichnen von skiasharp-Bitmaps](drawing.md#rotating-bitmaps)** gezeigt wird. Der Benutzer kann das Bild um 90 Grad im Uhrzeigersinn oder gegen den Uhrzeigersinn beliebig oft drehen: 

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

Wenn der Benutzer auf die Schaltfläche " **done** " klickt, `Clicked` navigiert der Handler zu `PhotoPuzzlePage3` und übergibt die abschließende gedrehte Bitmap im Konstruktor der Seite.

`PhotoPuzzlePage3`ermöglicht das Abschneiden des Fotos. Für das Programm ist eine quadratische Bitmap erforderlich, um Sie in ein 4 x 4-Raster von Kacheln aufzuteilen.

Die Datei **PhotoPuzzlePage3. XAML** enthält eine `Label` , eine `Grid` zum Hosten von `PhotoCropperCanvasView` und eine weitere Schaltfläche " **done** ":

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

Die Code-Behind-Datei instanziiert den `PhotoCropperCanvasView` mit der Bitmap, die an seinen Konstruktor übergeben wird. Beachten Sie, dass 1 als zweites Argument an die Übergabe von lautet `PhotoCropperCanvasView` . Dieses Seitenverhältnis von 1 erzwingt, dass das zuschnittrechteck ein Quadrat ist:

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

Der Handler für die **done** -Schaltfläche erhält die Breite und Höhe der zugeschnittenen Bitmap (diese beiden Werte sollten identisch sein) und teilt Sie dann in 15 separate Bitmaps auf, von denen jeder 1/4 die Breite und Höhe des Originals ist. (Die letzte der möglichen 16 Bitmaps wird nicht erstellt.) Die- `DrawBitmap` Methode mit Quell-und Ziel Rechteck ermöglicht das Erstellen einer Bitmap basierend auf einer Teilmenge einer größeren Bitmap.

## <a name="converting-to-xamarinforms-bitmaps"></a>Wandeln in Xamarin.Forms Bitmaps

In der- `OnDoneButtonClicked` Methode hat das für die 15 Bitmaps erstellte Array den folgenden Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource) :

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource`der Xamarin.Forms Basistyp, der eine Bitmap kapselt. Glücklicherweise ermöglicht skiasharp das Umstellen von skiasharp-Bitmaps in Xamarin.Forms Bitmaps. Die **skiasharp. views. Forms** -Assembly definiert eine [`SKBitmapImageSource`](xref:SkiaSharp.Views.Forms.SKBitmapImageSource) Klasse, die von abgeleitet `ImageSource` ist, aber auf der Grundlage eines skiasharp-Objekts erstellt werden kann `SKBitmap` . `SKBitmapImageSource`definiert sogar Konvertierungen zwischen `SKBitmapImageSource` und. `SKBitmap` auf diese Weise `SKBitmap` werden Objekte als Bitmaps in einem Array gespeichert Xamarin.Forms :

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

Dieses Array von Bitmaps wird als Konstruktor an übergeben `PhotoPuzzlePage4` . Diese Seite ist vollständig Xamarin.Forms und verwendet keine skiasharp. Sie ähnelt [**xamagonxuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)und wird daher hier nicht beschrieben, aber Ihr ausgewähltes Foto wird in 15 Quadrat Kacheln unterteilt:

[![Foto Rätsel 1](cropping-images/PhotoPuzzle1.png "Foto Rätsel 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

Durch Drücken der Schaltfläche **Randomize** werden alle Kacheln gemischt:

[![Foto Rätsel 2](cropping-images/PhotoPuzzle2.png "Foto Rätsel 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

Nun können Sie Sie wieder in der richtigen Reihenfolge ablegen. Alle Kacheln in derselben Zeile oder Spalte wie das leere Quadrat können abgetippt werden, um Sie in das leere Quadrat zu verschieben. 

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
