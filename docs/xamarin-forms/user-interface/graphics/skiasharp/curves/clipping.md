---
title: Schneiden mit Pfaden und Regionen
description: In diesem Artikel erläutert die Verwendung von SkiaSharp-Pfade zu Clip Grafiken auf bestimmte Bereiche und Regionen zu erstellen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 8978bd386ec2f2ea0f9960f079ce82750941cfad
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655952"
---
# <a name="clipping-with-paths-and-regions"></a>Schneiden mit Pfaden und Regionen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Pfade zu Clip-Grafiken zu verwenden, auf bestimmte Bereiche und Regionen erstellen._

Manchmal ist es erforderlich, um das Rendern von Grafiken auf einem bestimmten Bereich zu beschränken. Dies bezeichnet man als *Clipping*. Sie können die Clipping für Spezialeffekte zu erzeugen, z. B. mit diesem Image von einem Monkey-Objekt über eine Keyhole angezeigt:

![](clipping-images/clippingsample.png "Monkey-Objekt über eine keyhole")

Die *Clippingbereichs* ist der Bereich des Bildschirms in der Grafik gerendert werden. Alle Elemente, die außerhalb des Clippingbereichs angezeigt wird, wird nicht gerendert. Clippingbereichs in der Regel durch ein Rechteck definiert ist oder ein [ `SKPath` ](xref:SkiaSharp.SKPath) -Objekt, aber Sie können alternativ definieren ein Clipping mithilfe einer [ `SKRegion` ](xref:SkiaSharp.SKRegion) Objekt. Diese beiden Arten von Objekten an scheint zunächst Verwandte, da Sie eine Region aus einem Pfad erstellen können. Es ist jedoch nicht möglich, einen Pfad aus einer Region zu erstellen, und Sie unterscheiden sich intern stark: Ein Pfad besteht aus einer Reihe von Linien und Kurven, während ein Bereich durch eine Reihe von horizontalen Scan Linien definiert wird.

In der Abbildung oben wurde erstellt, indem die **Monkey-Objekt über Keyhole** Seite. Die [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) -Klasse definiert einen Pfad mit SVG-Daten und wird der Konstruktor verwendet, um eine Bitmap aus Programmressourcen zu laden:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

Obwohl die `keyholePath` Objekt beschreibt den Umriss einer Keyhole, die Koordinaten sind beliebig und widerspiegeln, was praktisch war, wenn die Pfaddaten gedacht war. Aus diesem Grund die `PaintSurface` Handler Ruft die Begrenzungen des diesen Pfad und Aufrufe `Translate` und `Scale` , den Pfad in der Mitte des Bildschirms zu verschieben und fast so groß sein wie der Bildschirm zu machen:


```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

Aber der Pfad wird nicht gerendert. Stattdessen wird folgende Transformationen, der Pfad verwendet, um eine Clippingbereichs mit dieser Anweisung festgelegt:

```csharp
canvas.ClipPath(keyholePath);
```

Die `PaintSurface` Handler setzt dann die Transformationen durch einen Aufruf von `ResetMatrix` und zeichnet die Bitmap um auf die Höhe des Bildschirms zu erweitern. Dieser Code wird davon ausgegangen, dass die Bitmap Quadrat, ist diese spezielle Bitmap. Die Bitmap wird nur innerhalb des Bereichs, der durch den Clipping-Pfad definierten gerendert:

[![](clipping-images/monkeythroughkeyhole-small.png "Dreifacher Screenshot, der das Monkey-Objekt über die Seite \"Keyhole\"")](clipping-images/monkeythroughkeyhole-large.png#lightbox "dreifachen Screenshot, der das Monkey-Objekt über die Seite \"Keyhole\"")

Des Freistellungspfads unterliegt die Transformationen in Kraft bei der `ClipPath` Methode wird aufgerufen, und nicht zu den Transformationen in Kraft bei ein grafisches Objekts (z. B. eine Bitmap) wird angezeigt. Die Freistellungspfad ist Teil der Canvas-Zustand, der mit gespeichert wird die `Save` Methode und wiederhergestellt, mit der `Restore` Methode.

## <a name="combining-clipping-paths"></a>Vereinen von Pfaden für Clipping

Genau genommen Clippingbereichs ist kein "set" durch die `ClipPath` Methode. Stattdessen wird dieser mit den vorhandenen Clipping-Pfad, kombiniert der als gleicher Größe auf den Zeichenbereich Rechteck beginnt. Sie erhalten die rechteckigen Grenzen der Clipping Bereich mithilfe der [ `ClipBounds` ](xref:SkiaSharp.SKCanvas.ClipBounds) Eigenschaft oder das [ `ClipDeviceBounds` ](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) Eigenschaft. Die `ClipBounds` -Eigenschaft gibt ein `SKRect` Wert ein, entspricht einem umwandelt, möglicherweise in Kraft. Die `ClipDeviceBounds` -Eigenschaft gibt eine `RectI` Wert. Dies ist ein Rechteck mit ganzzahligen Dimensionen und den Clippingbereich, in der tatsächlichen Pixeldimensionen beschreibt.

Jeder Aufruf von `ClipPath` Clippingbereichs durch die Kombination des Clippingbereichs mit einem neuen Bereich reduziert. Die vollständige Syntax der [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) Methode ist:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Es gibt auch eine [ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean)) -Methode, die den Clippingbereich mit einem Rechteck kombiniert:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Resultierende Clippingbereichs wird standardmäßig eine Schnittmenge von vorhandenen Clippingbereichs und `SKPath` oder `SKRect` angegeben, die der `ClipPath` oder `ClipRect` Methode. Dies wird veranschaulicht, der **vier Kreisen Intersect Clip** Seite. Die `PaintSurface` Ereignishandler in der [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) Klasse verwendet wieder die gleichen `SKPath` Objekt, das vier überlappende Kreise, zu erstellen, von denen jeder durch aufeinander folgende Aufrufe von Clippingbereichs reduziert `ClipPath`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

Nun muss nur noch ist die Schnittmenge von diesen vier Gruppen:

[![](clipping-images//fourcircleintersectclip-small.png "Dreifacher Screenshot der Seite für vier Kreis Intersect Clip")](clipping-images/fourcircleintersectclip-large.png#lightbox "dreifachen Screenshot, der vier Kreis Intersect Clip-Seite")

Die [ `SKClipOperation` ](xref:SkiaSharp.SKClipOperation) -Enumeration hat nur zwei Mitglieder:

- `Difference` Entfernt die angegebene Pfad oder das Rechteck aus vorhandenen Clippingbereichs

- `Intersect` Schneidet die angegebene Pfad oder ein Rechteck mit vorhandenen Clippingbereichs

Wenn Sie die vier ersetzen `SKClipOperation.Intersect` Argumente in der `FourCircleIntersectClipPage` Klasse mit `SKClipOperation.Difference`, sehen Sie Folgendes:

[![](clipping-images//fourcircledifferenceclip-small.png "Dreifacher Screenshot der Seite vier Kreis Intersect Clip mit differenzbildung")](clipping-images/fourcircledifferenceclip-large.png#lightbox "dreifachen Screenshot der Seite vier Kreis Intersect Clip mit differenzbildung")

Vier überlappende Kreise wurden aus Clippingbereichs entfernt.

Die **Clip Vorgänge** Seite veranschaulicht den Unterschied zwischen diese beiden Vorgänge mit nur einem Paar von Kreisen. Der Kreis auf der linken Seite wird hinzugefügt, in den Clippingbereich mit der standardmäßigen Clip-Operation von `Intersect`, während der zweite Kreis auf der rechten Seite mit den Clip-Operation, angegeben durch die textbezeichnung Clippingbereichs hinzugefügt wird:

[![](clipping-images//clipoperations-small.png "Dreifacher Screenshot der Seite Clip Vorgänge")](clipping-images/clipoperations-large.png#lightbox "dreifachen Screenshot der Seite Clip-Vorgänge")

Die [ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) -Klasse definiert zwei `SKPaint` Objekte als Felder, und klicken Sie dann den Bildschirm nach oben in zwei rechteckige Bereiche unterteilt. Diese Bereiche unterscheiden sich abhängig davon, ob das Smartphone im Hoch-oder Querformat. Die `DisplayClipOp` Klasse zeigt dann die Text und ruft `ClipPath` mit den zwei Kreis-Pfaden zu jeder Clip-Operation zu veranschaulichen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

Aufrufen von `DrawPaint` wird normalerweise auf den gesamten Zeichenbereich mit, die gefüllt werden soll `SKPaint` Objekt aber in diesem Fall die Methode nur zeichnet innerhalb des Clippingbereichs,.

## <a name="exploring-regions"></a>Untersuchen von Regionen

Sie können auch eine Clippingbereichs hinsichtlich der definieren eine [ `SKRegion` ](xref:SkiaSharp.SKRegion) Objekt.

Ein neu erstelltes `SKRegion` Objekt beschreibt, einen leeren Bereich. Der erste Aufruf für das Objekt in der Regel ist [ `SetRect` ](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) so, dass die Region ein rechteckiges Bereichs beschreibt. Der Parameter für `SetRect` ist ein `SKRectI` Wert &mdash; ein Rechteck mit ganzzahligen koordiniert werden, da sie das Rechteck in Pixel angibt. Sie können dann aufrufen [ `SetPath` ](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) mit einer `SKPath` Objekt. Dies erstellt eine Region aus, die ist identisch mit das Innere des Pfads, aber der anfänglichen rechteckige Bereich zugeschnitten.

Die Region kann auch durch Aufrufen einer der geändert werden die [ `Op` ](xref:SkiaSharp.SKRegion.Op*) -methodenüberladungen, wie diese:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Die [ `SKRegionOperation` ](xref:SkiaSharp.SKRegionOperation) Enumeration ist vergleichbar mit `SKClipOperation` , aber er hat mehr Mitglieder:

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

Die Region, die Sie vornehmen, die `Op` Aufruf kombiniert wird, mit der Region, die als Parameter basierend auf den `SKRegionOperation` Member. Wenn Sie schließlich eine Region für das Clipping geeignete erhalten, Sie können festlegen, wie Clippingbereichs der Canvas mit der [ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) -Methode der `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Der folgende Screenshot zeigt Clipping Bereiche basieren auf den sechs Region-Vorgängen. Der linke Kreis ist die Region, die die `Op` für die Methode aufgerufen wird, und der richtige Kreis ist die Region, die an die `Op` Methode:

[![](clipping-images//regionoperations-small.png "Dreifacher Screenshot der Seite Bereichsvorgänge")](clipping-images/regionoperations-large.png#lightbox "dreifachen Screenshot der Seite Bereichsvorgänge")

Sind diese alle die Möglichkeiten der Kombination dieser beiden Kreise? Betrachten Sie das resultierende Image als eine Kombination aus drei Komponenten, die sich in angezeigt werden die `Difference`, `Intersect`, und `ReverseDifference` Vorgänge. Die Gesamtanzahl der Kombinationen wird zwei die dritte Potenz oder acht. Die fehlen handelt es sich um die ursprüngliche Region (was dazu führt, aus dem Aufruf nicht `Op` überhaupt) und eine Region vollständig leer.

Es ist schwieriger, verwenden Regionen, für das Zuschneiden, da müssen Sie zuerst erstellen Sie einen Pfad, und klicken Sie dann eine Region aus, dass der Pfad, und klicken Sie dann Kombinieren von mehreren Regionen. Die allgemeine Struktur von den **Bereichsvorgänge** Seite ähnelt **Clip-Vorgänge** jedoch [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) Klasse wird der Bildschirm nach oben an, in sechs Kategorien und Zeigt den zusätzlichen Aufwand erforderlich, um Regionen für diesen Auftrag verwenden:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

Hier ist ein großer Unterschied zwischen der `ClipPath` Methode und die `ClipRegion` Methode:

> [!IMPORTANT]
> Im Gegensatz zu den `ClipPath` -Methode, die `ClipRegion` Methode wird durch Transformationen nicht beeinflusst.

Um die Gründe für diesen Unterschied zu verstehen, ist es hilfreich zu verstehen, welche ein Bereich ist. Wenn Sie daran gedacht haben, zu wie der Clip oder Region Vorgänge intern implementiert werden können, scheint es wahrscheinlich sehr kompliziert. Sind mehrere potenziell sehr komplexe Pfaden kombiniert werden, und die Kontur des der resultierende Pfad ist wahrscheinlich eine algorithmische Albtraum.

Dieser Auftrag wird erheblich vereinfacht, wenn jeder Pfad in eine Reihe von Zeilen, z. B. in altmodischen Vakuum Tube TVs reduziert wird. Jede Scanzeile ist einfach eine horizontale Linie mit einem Start- und Endpunkt. Beispielsweise kann ein Kreis mit einem Radius von 10 Pixel in 20 Zeilen, zerlegt werden, von denen jedes an den linken Teil des Kreises beginnt, und endet mit den rechten Teil. Kombinieren von zwei Kreise mit jeder Region-Vorgang wird sehr einfach, da sie lediglich die Anfangs- und Endkoordinaten der einzelnen Zeilen der entsprechenden Überprüfung untersucht wird.

Eine Region ist: Eine Reihe von horizontalen Scan Linien, die einen Bereich definieren.

Aber wenn ein Bereich in eine Reihe von Scan reduziert wird Zeilen, diese Überprüfung aus, auf denen ein bestimmtes Pixel Dimension Zeilen basieren. Genau genommen ist die Region kein Vector Graphics-Objekt. Es ähnelt in der Art, eine komprimierte monochrome Bitmap als einem Pfad. Daher können nicht Regionen skaliert oder rotiert ohne Verlust der Genauigkeit und aus diesem Grund, die sie nicht transformiert werden, wenn für Clipping-Bereiche verwendet werden.

Sie können jedoch Transformationen auf Regionen zu zeichnen anwenden. Die **Region Paint** Programm Lernziele veranschaulicht, welche inneren Regionen. Die [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) -Klasse erstellt eine `SKRegion` Objekt auf Grundlage einer `SKPath` eines Kreises mit 10 Einheiten Radius. Eine Transformation wird dann erweitert, Kreis, um die Seite auszufüllen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

Die `DrawRegion` Aufruf füllt den Bereich in orange dargestellt, während die `DrawPath` Aufruf die Striche des ursprünglichen Pfads in Blau dargestellt, für den Vergleich:

[![](clipping-images//regionpaint-small.png "Dreifacher Screenshot der Seite Region Paint")](clipping-images/regionpaint-large.png#lightbox "dreifachen Screenshot der Seite für die Region Paint")

Die Region ist natürlich eine Reihe von diskreten Koordinaten.

Wenn Sie mithilfe von Transformationen in Verbindung mit Ihrem Clipping Bereiche benötigen, können Sie Regionen für das Clipping, verwenden, als die **vier – Blatt Clover** Seite erläutert. Die [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Klasse erstellt eine zusammengesetzte Region aus vier zirkuläre Regionen dieser zusammengesetzten Region als Clippingbereichs festlegt und dann zeichnet eine Reihe von 360 gerade Linien, die von der Mitte der Seite ausgehen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

Sehen sie nicht wirklich wie ein vier – Leaf Clover, aber es ist ein Bild an, die andernfalls schwer ohne Clipping gerendert werden kann:

[![](clipping-images//fourleafclover-small.png "Dreifacher Screenshot der Seite für vier – Blatt Clover")](clipping-images/fourleafclover-large.png#lightbox "dreifachen Screenshot der Seite für vier – Blatt Clover")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
