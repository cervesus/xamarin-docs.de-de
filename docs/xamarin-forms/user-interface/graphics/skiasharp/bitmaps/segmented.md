---
title: Segmentierte Anzeige der SkiaSharp-bitmaps
description: Zeigen Sie eine Bitmap SkiaSharp, damit einige Bereich werden gestreckt, und einige Bereiche nicht sind.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: ca6c8fafe4352bac83e5ae60b43627d4c7fdc10f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648668"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>Segmentierte Anzeige der SkiaSharp-bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die SkiaSharp `SKCanvas` Objekt definiert eine Methode namens `DrawBitmapNinePatch` und zwei Methoden, die mit dem Namen `DrawBitmapLattice` , sind sehr ähnlich. Sowohl diese Methoden Rendern einer Bitmap auf die Größe eines Rechtecks Ziel, aber statt Dehnen die Bitmap gleichmäßig, Teile der Bitmap in die Pixeldimensionen anzeigen und andere Teile der Bitmap gestreckt, so, dass sie das Rechteck passt:

![Beispiele für segmentierte](segmented-images/SegmentedSample.png "segmentiert-Beispiel")

Diese Methoden werden in der Regel für das Rendern von Bitmaps, die Bestandteil der Benutzeroberflächen-Objekte wie z. B. Schaltflächen verwendet. Beim Entwerfen einer Schaltfläche in der Regel sollen die Größe einer Schaltfläche auf den Inhalt der Schaltfläche basieren soll, aber Sie möchten sicherlich der Rahmen der Schaltfläche auf dieselbe Breite unabhängig von den Inhalt der Schaltfläche. Dies ist eine ideale Anwendung `DrawBitmapNinePatch`.

`DrawBitmapNinePatch` ist ein Sonderfall des `DrawBitmapLattice` , aber es ist die einfachere der beiden Methoden zu verwenden und zu verstehen.

## <a name="the-nine-patch-display"></a>Die neun-Patch-Anzeige 

Im Prinzip [ `DrawBitmapNinePatch` ](xref:SkiaSharp.SKCanvas.DrawBitmapNinePatch(SkiaSharp.SKBitmap,SkiaSharp.SKRectI,SkiaSharp.SKRect,SkiaSharp.SKPaint)) eine Bitmap in neun Rechtecke unterteilt:

![Neun Patch](segmented-images/NinePatch.png "neun Patch")

Die Rechtecke an den vier Ecken werden in ihre Pixelgrößen angezeigt. Wie Sie die Pfeile zeigen, werden die anderen Bereiche am Rand der Bitmap in den Bereich des Zielrechtecks horizontal oder vertikal gestreckt. Das Rechteck in der Mitte ist horizontal und vertikal gestreckt. 

Wenn nicht genügend Speicherplatz in das Zielrechteck noch die vier Ecken die Pixeldimensionen anzuzeigenden vorhanden ist, sie werden nach unten skaliert, um die verfügbare Größe, und nichts, aber die vier Ecken werden angezeigt.

Um eine Bitmap in diese neun Rechtecke zu unterteilen, ist es nur erforderlich, geben Sie das Rechteck in der Mitte. Dies ist die Syntax der `DrawBitmapNinePatch` Methode:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

Das Center-Rechteck ist relativ zu der Bitmap. Es ist ein `SKRectI` Wert (die Integer-Version des `SKRect`) und alle Koordinaten und Größen sind in Pixel. Das Zielrechteck ist relativ zu der Anzeigeoberfläche. Das `paint`-Argument ist optional.

Die **neun Patch Anzeige** auf der Seite die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel verwendet einen statischen Konstruktor zuerst erstellen Sie eine öffentliche statische Eigenschaft vom Typ `SKBitmap`:

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

Zwei andere Seiten in diesem Artikel verwenden Sie diese gleichen Bitmap. Die Bitmap ist 500 Pixel im Quadrat und besteht aus einem Array von 25 Kreisen dieselben Größe jedes eine 100-Pixel-quadratische Fläche einnimmt:

![Kreis, Raster](segmented-images/CircleGrid.png "Kreis Raster")

Instanzenkonstruktor des Programms erstellt eine `SKCanvasView` mit einem `PaintSurface` Handler, der verwendet `DrawBitmapNinePatch` anzuzeigende Bitmap gestreckt, um die gesamte Anzeigeoberfläche zu:

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

Die `centerRect` Rechteck umfasst das zentrale Array von 16 Kreise. Die Kreise in den Ecken werden in die Pixeldimensionen angezeigt, und alles andere wird gestreckt, entsprechend:

[![Anzeigen von neun-Patch](segmented-images/NinePatchDisplay.png "neun-Patch-Anzeige")](segmented-images/NinePatchDisplay-Large.png#lightbox)

Die Seite "UWP" 500 Pixel breit ist, und zeigt daher die oberste und unterste Zeile als eine Reihe von Kreisen mit derselben Größe. Andernfalls werden die Kreise aus, die nicht in den Ecken sind auf Formular Ellipsen gestreckt.

Für ungewöhnliche Anzeige von Objekten, die aus einer Kombination von Kreisen und Ellipsen versuchen Sie es definieren Rechteck in der Mitte, sodass sie Zeilen und Spalten der Kreise überlappt:

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Die Anzeige lattice

Die beiden `DrawBitmapLattice` Methoden ähneln `DrawBitmapNinePatch`, aber sie sind für eine beliebige Anzahl horizontaler oder vertikaler Unterteilungen generalisiert. Diese Bereiche werden von Arrays mit Ganzzahlen, Pixel entspricht definiert. 

Die [ `DrawBitmapLattice` ](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,System.Int32[],System.Int32[],SkiaSharp.SKRect,SkiaSharp.SKPaint)) Methode mit Parametern für diese Arrays von ganzen Zahlen scheint nicht funktioniert. Die [ `DrawBitmapLattice` ](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,SkiaSharp.SKLattice,SkiaSharp.SKRect,SkiaSharp.SKPaint)) Methode mit einem Parameter vom Typ `SKLattice` funktioniert, und das ist die in den unten gezeigten Beispielen verwendet.

Die [ `SKLattice` ](xref:SkiaSharp.SKLattice) Struktur definiert vier Eigenschaften:

- [`XDivs`](xref:SkiaSharp.SKLattice.XDivs), ein Array von Ganzzahlen
- [`YDivs`](xref:SkiaSharp.SKLattice.YDivs), ein Array von Ganzzahlen
- [`Flags`](xref:SkiaSharp.SKLattice.Flags), ein Array von `SKLatticeFlags`, ein Enumerationstyp
- [`Bounds`](xref:SkiaSharp.SKLattice.Bounds) Der Typ `Nullable<SKRectI>` ein optionaler Quellrechteck innerhalb der Bitmap an

Die `XDivs` Array unterteilt die Breite der Bitmap in vertikalen leisten. Die erste Strip erstreckt sich von 0 Pixel auf der linken Seite auf `XDivs[0]`. Diese entfernen, die in die Breite in Pixel gerendert wird. Die zweite Strip erstreckt sich von `XDivs[0]` zu `XDivs[1]`, und gestreckt wird. Erstreckt sich von der dritten Streifen `XDivs[1]` zu `XDivs[2]` und in die Breite in Pixel gerendert wird. Die letzte Streifen erstreckt sich von dem letzten Element des Arrays an den rechten Rand der Bitmap. Wenn das Array eine gerade Anzahl von Elementen hat, und klicken Sie dann die Breite in Pixel angezeigt wird. Andernfalls wird es gestreckt. Die Gesamtanzahl der vertikalen Streifen ist eine weitere als die Anzahl der Elemente im Array.

Die `YDivs` Array entspricht. Die Höhe des Arrays dividiert in horizontale Streifen.

Zusammen, die `XDivs` und `YDivs` Array die Bitmap in Rechtecke unterteilen. Die Anzahl der Rechtecke ist gleich dem Produkt, der die Anzahl der horizontale Leisten und die Anzahl der vertikalen leisten.

Gemäß Skia Dokumentation der `Flags` Array enthält ein Element für jedes Rechtecks, zuerst die oberste Zeile von Rechtecken, und klicken Sie dann der zweiten Zeile und So weiter. Die `Flags` Array ist vom Typ [ `SKLatticeFlags` ](xref:SkiaSharp.SKLatticeFlags), eine Enumeration mit folgenden Membern:

- `Default` mit dem Wert 0
- `Transparent` mit dem Wert 1

Allerdings nicht, diese Flags scheint zu funktionieren, wie sie sollen, und es empfohlen wird, die ignoriert werden. Nicht festgelegt, aber die `Flags` Eigenschaft `null`. Legen Sie ihn auf ein Array von `SKLatticeFlags` Werte groß genug, um die Gesamtzahl der Rechtecke umfassen.

Die **Lattice neun Patch** verwendet `DrawBitmapLattice` nachahmen `DrawBitmapNinePatch`. Wird auf die gleiche Bitmap, die im erstellten `NinePatchDisplayPage`:

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

Sowohl die `XDivs` und `YDivs` Eigenschaften werden für Arrays von zwei ganzen Zahlen, unterteilen die Bitmap in drei leisten, horizontal und vertikal festgelegt: vom Pixel 0 Pixel (dargestellt in der Größe in Pixel), 100 vom Pixel 100 Pixel 400 (gedehnt), und vom Pixel 400 Pixel 500 (Größe in Pixel). Zusammen `XDivs` und `YDivs` definieren insgesamt 9 Rechtecke, die die Größe des der `Flags` Array. Erstellen einfach das Array ist ausreichend, um ein Array von erstellen `SKLatticeFlags.Default` Werte.

Die Anzeige ist identisch mit den oben stehenden Programms:

[![Lattice neun-Patch](segmented-images/LatticeNinePatch.png "Lattice neun-Patch")](segmented-images/LatticeNinePatch-Large.png#lightbox)

Die **Lattice Anzeige** Seite verwendet wird, wird die Bitmap in 16 Rechtecke unterteilt:

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

Die `XDivs` und `YDivs` Arrays unterscheiden, verursacht die Anzeige nicht ganz so wie in den vorherigen Beispielen symmetrische sind:

[![Lattice Anzeige](segmented-images/LatticeDisplay.png "Lattice anzeigen")](segmented-images/LatticeDisplay-Large.png#lightbox)

In den IOS- und Android-Images auf der linken Seite werden nur die Kreise in ihrer Größe Pixel gerendert. Alles andere wird gestreckt.

Die **Lattice Anzeige** Seite generalisiert und dadurch die Erstellung der `Flags` Array, sodass Sie zum Experimentieren mit `XDivs` und `YDivs` leichter. Insbesondere möchten Sie sehen, was geschieht, wenn Sie festlegen, dass das erste Element der `XDivs` oder `YDivs` Array auf 0. 

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
