---
Title: "segmentiertes Anzeigen von skiasharp-Bitmaps" Description: "zeigt eine skiasharp-Bitmap an, sodass ein Bereich gestreckt und einige Bereiche nicht sind."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 79ae2033-c41c-4447-95a6-76d22e913d19 Author: davidbritch ms. Author: dabritch ms. Date: 07/17/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="segmented-display-of-skiasharp-bitmaps"></a>Segmentierte Anzeige von skiasharp-Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Das skiasharp `SKCanvas` -Objekt definiert eine Methode mit dem Namen `DrawBitmapNinePatch` und zwei Methoden mit dem Namen `DrawBitmapLattice` , die sehr ähnlich sind. Beide Methoden Rendern eine Bitmap in der Größe eines Ziel Rechtecks. statt jedoch die Bitmap gleichmäßig zu Strecken, werden Teile der Bitmap in ihren Pixel Dimensionen angezeigt, und andere Teile der Bitmap werden gestreckt, damit Sie dem Rechteck entspricht:

![Segmentierte Beispiele](segmented-images/SegmentedSample.png "Segmentiertes Beispiel")

Diese Methoden werden in der Regel zum Rendern von Bitmaps verwendet, die Teil von Benutzeroberflächen Objekten wie Schaltflächen sind. Beim Entwerfen einer Schaltfläche möchten Sie im Allgemeinen, dass die Größe einer Schaltfläche auf dem Inhalt der Schaltfläche basiert, aber wahrscheinlich möchten Sie, dass der Rahmen der Schaltfläche unabhängig vom Inhalt der Schaltfläche dieselbe Breite hat. Das ist eine ideale Anwendung von `DrawBitmapNinePatch` .

`DrawBitmapNinePatch`ist ein Sonderfall von `DrawBitmapLattice` , aber es ist einfacher, die beiden Methoden zu verwenden und zu verstehen.

## <a name="the-nine-patch-display"></a>Die neun-Patch-Anzeige 

Konzeptionell [`DrawBitmapNinePatch`](xref:SkiaSharp.SKCanvas.DrawBitmapNinePatch(SkiaSharp.SKBitmap,SkiaSharp.SKRectI,SkiaSharp.SKRect,SkiaSharp.SKPaint)) dividiert eine Bitmap in neun Rechtecke:

![9 Patch](segmented-images/NinePatch.png "9 Patch")

Die Rechtecke in den vier Ecken werden in ihren Pixelgrößen angezeigt. Wie die Pfeile zeigen, werden die anderen Bereiche an den Rändern der Bitmap horizontal oder vertikal auf den Bereich des Ziel Rechtecks gestreckt. Das Rechteck in der Mitte wird sowohl horizontal als auch vertikal gestreckt. 

Wenn nicht genügend Speicherplatz im Ziel Rechteck vorhanden ist, um sogar die vier Ecken in ihren Pixel Dimensionen anzuzeigen, werden Sie auf die verfügbare Größe herab skaliert, und es werden nur die vier Ecken angezeigt.

Um eine Bitmap in diese neun Rechtecke aufzuteilen, muss nur das Rechteck in der Mitte angegeben werden. Dies ist die Syntax der- `DrawBitmapNinePatch` Methode:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

Das zentrienrechteck ist relativ zur Bitmap. Dabei handelt es sich um einen `SKRectI` -Wert (die ganzzahlige Version von `SKRect` ), und alle Koordinaten und Größen befinden sich in Pixel Einheiten. Das Ziel Rechteck ist relativ zur Anzeige Oberfläche. Das `paint`-Argument ist optional.

Die Seite **neun Patch-Anzeige** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel verwendet zunächst einen statischen Konstruktor, um eine öffentliche statische Eigenschaft des Typs zu erstellen `SKBitmap` :

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

Zwei andere Seiten in diesem Artikel verwenden dieselbe Bitmap. Die Bitmap ist 500 Pixel quadratisch und besteht aus einem Array aus 25 Kreisen, das die gleiche Größe hat, wobei jeweils eine 100-Pixel-Quadrat Flächen belegt werden:

![Kreis Raster](segmented-images/CircleGrid.png "Kreis Raster")

Der Instanzkonstruktor des Programms erstellt eine `SKCanvasView` mit einem `PaintSurface` Handler, `DrawBitmapNinePatch` der verwendet, um die Bitmap, die auf die gesamte Anzeige Oberfläche gestreckt ist, anzuzeigen:

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

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

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

Das `centerRect` Rechteck umfasst das zentrale Array von 16 Kreisen. Die Kreise in den Ecken werden in ihren Pixel Dimensionen angezeigt, und alles andere wird entsprechend gestreckt:

[![Neun-Patchanzeige](segmented-images/NinePatchDisplay.png "Neun-Patchanzeige")](segmented-images/NinePatchDisplay-Large.png#lightbox)

Die UWP-Seite ist 500 Pixel breit und zeigt daher die oberen und unteren Zeilen als eine Reihe von Kreisen derselben Größe an. Andernfalls werden alle Kreise, die sich nicht in den Ecken befinden, gestreckt, um Ellipsen zu bilden.

Für eine seltsame Anzeige von Objekten, die aus einer Kombination aus Kreisen und Ellipsen bestehen, versuchen Sie, das zentrierende Rechteck so zu definieren, dass es Zeilen und Spalten von Kreisen überlappt:

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Die Gitter Anzeige

Die beiden `DrawBitmapLattice` Methoden ähneln `DrawBitmapNinePatch` , Sie sind jedoch für eine beliebige Anzahl horizontaler oder vertikaler Bereiche generalisiert. Diese Bereiche werden durch Arrays ganzer Zahlen definiert, die Pixel entsprechen. 

Die [`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,System.Int32[],System.Int32[],SkiaSharp.SKRect,SkiaSharp.SKPaint)) Methode mit Parametern für diese Arrays von ganzen Zahlen scheint nicht zu funktionieren. Die [`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,SkiaSharp.SKLattice,SkiaSharp.SKRect,SkiaSharp.SKPaint)) -Methode mit einem Parameter vom Typ funktioniert `SKLattice` , und das ist das, das in den unten gezeigten Beispielen verwendet wird.

Die [`SKLattice`](xref:SkiaSharp.SKLattice) Struktur definiert vier Eigenschaften:

- [`XDivs`](xref:SkiaSharp.SKLattice.XDivs), ein Array aus ganzen Zahlen
- [`YDivs`](xref:SkiaSharp.SKLattice.YDivs), ein Array aus ganzen Zahlen
- [`Flags`](xref:SkiaSharp.SKLattice.Flags), ein Array von `SKLatticeFlags` , ein Enumerationstyp
- [`Bounds`](xref:SkiaSharp.SKLattice.Bounds)vom Typ `Nullable<SKRectI>` zum Angeben eines optionalen Quell Rechtecks innerhalb der Bitmap

Das `XDivs` Array dividiert die Breite der Bitmap in vertikale Striche. Der erste Strip erstreckt sich von Pixel 0 auf der linken Seite auf `XDivs[0]` . Dieser Strip wird in seiner Pixel Breite gerendert. Der zweite Bereich erstreckt sich von `XDivs[0]` auf `XDivs[1]` , und wird gestreckt. Der dritte Strip erstreckt `XDivs[1]` sich von auf `XDivs[2]` und wird in der Pixel Breite gerendert. Der letzte Bereich erstreckt sich vom letzten Element des Arrays bis zum rechten Rand der Bitmap. Wenn das Array über eine gerade Anzahl von Elementen verfügt, wird es in seiner Pixel Breite angezeigt. Andernfalls wird Sie gestreckt. Die Gesamtanzahl vertikaler Striche ist ein Wert größer als die Anzahl der Elemente im Array.

Das `YDivs` Array ist ähnlich. Sie dividiert die Höhe des Arrays in horizontale Striche.

Das `XDivs` -Array und `YDivs` das-Array teilen die Bitmap in Rechtecke. Die Anzahl der Rechtecke entspricht dem Produkt der Anzahl horizontaler Striche und der Anzahl vertikaler Striche.

Gemäß der Skia-Dokumentation `Flags` enthält das Array ein-Element für jedes Rechteck, zuerst die obere Zeile der Rechtecke, dann die zweite Zeile usw. Das `Flags` Array weist den Typ auf [`SKLatticeFlags`](xref:SkiaSharp.SKLatticeFlags) , eine Enumeration mit den folgenden Membern:

- `Default`mit dem Wert 0
- `Transparent`mit Wert 1

Diese Flags scheinen jedoch anscheinend nicht zu funktionieren, und Sie sollten Sie ignorieren. Legen Sie die- `Flags` Eigenschaft jedoch nicht auf fest `null` . Legen Sie es auf ein Array von `SKLatticeFlags` Werten fest, das groß genug ist, um die Gesamtzahl der Rechtecke zu umfassen

Die Seite für das **Gitter neun Patchen** verwendet `DrawBitmapLattice` zum imitieren `DrawBitmapNinePatch` . Dabei wird die gleiche Bitmap verwendet, die in erstellt wurde `NinePatchDisplayPage` :

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

Sowohl die `XDivs` -Eigenschaft als auch die-Eigenschaft `YDivs` werden auf Arrays von nur zwei ganzen Zahlen festgelegt, wobei die Bitmap sowohl horizontal als auch vertikal in drei Striche aufgeteilt wird: von Pixel 0 bis Pixel 100 (gerendert in der Pixelgröße), von Pixel 100 auf Pixel 400 (gestreckt) und von Pixel 400 zu Pixel 500 (Pixelgröße). `XDivs`Und `YDivs` definieren insgesamt 9 Rechtecke, d. h. die Größe des `Flags` Arrays. Die einfache Erstellung des Arrays reicht aus, um ein Array mit Werten zu erstellen `SKLatticeFlags.Default` .

Die Anzeige ist mit dem vorherigen Programm identisch:

[![Gitter 9-Patch](segmented-images/LatticeNinePatch.png "Gitter 9-Patch")](segmented-images/LatticeNinePatch-Large.png#lightbox)

Die **Gitter Anzeigeseite** dividiert die Bitmap in 16 Rechtecke:

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

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

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

Die `XDivs` -und- `YDivs` Arrays unterscheiden sich etwas, sodass die Anzeige nicht so symmetrisch wie die vorherigen Beispiele ist:

[![Gitter Anzeige](segmented-images/LatticeDisplay.png "Gitter Anzeige")](segmented-images/LatticeDisplay-Large.png#lightbox)

In den IOS-und Android-Images auf der linken Seite werden nur die kleineren Kreise in ihren Pixelgrößen gerendert. Alles andere wird gestreckt.

Die **Gitter Anzeige** Seite generalisiert die Erstellung des `Flags` Arrays, sodass Sie mit und leichter experimentieren `XDivs` können `YDivs` . Insbesondere möchten Sie sehen, was geschieht, wenn Sie das erste Element des- `XDivs` oder- `YDivs` Arrays auf 0 festlegen. 

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
