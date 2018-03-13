---
title: Clipping mit Pfaden und Regionen
description: Verwenden Sie Pfade zu Clip-Grafiken auf bestimmte Bereiche und Regionen erstellen
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: bb99984f93f494cfb5ad3d37ccb25f0b91d0b489
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="clipping-with-paths-and-regions"></a>Clipping mit Pfaden und Regionen

_Verwenden Sie Pfade zu Clip-Grafiken auf bestimmte Bereiche und Regionen erstellen_

Manchmal ist es erforderlich, um das Rendern von Grafiken auf einen bestimmten Bereich zu beschränken. Dies bezeichnet man *Clipping*. Sie können Clipping Spezialeffekte, z. B. dieses Bild von einer Affe über eine Keyhole angezeigt:

![](clipping-images/clippingsample.png "Affe über eine keyhole")

Die *Clippingbereichs* ist der Bereich des Bildschirms in der Grafik gerendert werden. Elemente, die außerhalb des Clippingbereichs angezeigt wird, wird nicht gerendert. Clippingbereichs ist in der Regel definiert ein [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) -Objekt, aber Sie können alternativ Definieren eines Clipping Bereich mithilfe einer [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) Objekt. Diese beiden Typen von Objekten auf scheint zunächst Verwandte, da Sie eine Region aus einem Pfad erstellen können. Allerdings können Sie einen Pfad aus einer Region erstellen, und diese intern sehr verschieden sind: ein Pfad umfasst eine Reihe von Linien und Kurven, während eine Region durch eine Reihe von Zeilen definiert wird.

Der Abbildung oben erstellt wurde, indem Sie die **Affe über Keyhole** Seite. Die [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Klasse definiert einen Pfad mithilfe der SVG-Daten und den Konstruktor verwendet, um eine Bitmap von Programmressourcen zu laden:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

Obwohl die `keyholePath` Objekt beschreibt den Umriss einer Keyhole, die Koordinaten sind vollständig beliebig und wider, was praktisch wurde bei der Pfaddaten entwickelt wurde. Aus diesem Grund die `PaintSurface` Ereignishandler ruft die Grenzen des diesem Pfad und die Aufrufe `Translate` und `Scale` so verschieben Sie den Pfad in der Mitte des Bildschirms und fast so groß wie der Bildschirm zu machen:


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

Der Pfad wird jedoch nicht gerendert. Stattdessen wird nach den Transformationen, der Pfad verwendet, um eine Clippingbereichs mit dieser Anweisung festzulegen:

```csharp
canvas.ClipPath(keyholePath);
```

Die `PaintSurface` Handler dann zurückgesetzt, die Transformationen mit einem Aufruf von `ResetMatrix` zu erweitern, um die vollständige Höhe des Bildschirms die Bitmap gezeichnet. Dieser Code wird davon ausgegangen, dass die Bitmap quadratische, ist also diese bestimmte Bitmap. Die Bitmap wird nur innerhalb des Bereichs, der durch den Beschneidungspfad definierten gerendert:

[![](clipping-images/monkeythroughkeyhole-small.png "Dreifacher Screenshot, der die Affe über die Seite "Keyhole"")](clipping-images/monkeythroughkeyhole-large.png#lightbox "dreifacher Screenshot, der die Affe über die Seite "Keyhole"")

Des Freistellungspfads ist beim aktiviert die Transformationen unterliegen die `ClipPath` Methode wird aufgerufen, und nicht zu den Transformationen faktisch Wenn ein Objekt (z. B. eine Bitmap) angezeigt wird. Beschneidungspfad ist Teil der Canvas-Zustand, der mit gespeichert wird die `Save` Methode und wiederhergestellt, mit der `Restore` Methode.

## <a name="combining-clipping-paths"></a>Kombinieren von Pfaden Clipping

Streng genommen ist Clippingbereichs ist kein "set" durch die `ClipPath` Methode. Stattdessen wird mit den vorhandenen Beschneidungspfad kombiniert, das als gleicher Größe auf dem Bildschirm Rechteck beginnt. Sie erhalten die rechteckigen Grenzen der Clipping Bereich unter Verwendung der [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) Eigenschaft oder die [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) Eigenschaft. Die `ClipBounds` -Eigenschaft gibt ein `SKRect` Wert an, die widerspiegelt, alle Transformationen, die möglicherweise wirksam werden. Die `ClipDeviceBounds` -Eigenschaft gibt ein `RectI` Wert. Dies ist ein Rechteck mit ganzzahligen Dimensionen und Clippingbereichs in tatsächliche Pixelmaßen beschreibt.

Jeder Aufruf von `ClipPath` Clippingbereichs durch Kombinieren des Clippingbereichs mit einem neuen Bereich reduziert. Die vollständige Syntax der [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) Methode ist:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Es gibt auch eine [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) -Methode, die mit einem Rechteck Clippingbereichs kombiniert:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Standardmäßig ist der resultierende Clippingbereichs eine Schnittmenge des vorhandenen Clippingbereichs und `SKPath` oder `SKRect` angegeben, die der `ClipPath` oder `ClipRect` Methode. Dies wird dargestellt, der **vier Kreise Intersect Clip** Seite. Die `PaintSurface` Ereignishandler in der [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) Klasse verwendet die gleiche `SKPath` Objekt, das vier überlappende Kreise, erstellen, von denen jede Clippingbereichs über aufeinander folgende Aufrufe von reduziert `ClipPath`:

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

Was bleibt, ist die Schnittmenge des diese vier Kreise:

[![](clipping-images//fourcircleintersectclip-small.png "Dreifacher Screenshot der Seite vier Kreis Intersect Clip")](clipping-images/fourcircleintersectclip-large.png#lightbox "dreifacher Screenshot der Seite vier Kreis Intersect Clip")

Die [ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) -Enumeration hat nur zwei Member:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Entfernt den angegebenen Pfad oder ein Rechteck aus vorhandenen Clippingbereichs

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) Schneidet den angegebenen Pfad oder ein Rechteck mit vorhandenen Clippingbereichs

Wenn Sie die vier ersetzen `SKClipOperation.Intersect` Argumente in der `FourCircleIntersectClipPage` -Klasse mit `SKClipOperation.Difference`, sehen Sie Folgendes:

[![](clipping-images//fourcircledifferenceclip-small.png "Dreifacher Screenshot der Seite vier Kreis Intersect Clip Unterschied Vorgang")](clipping-images/fourcircledifferenceclip-large.png#lightbox "dreifacher Screenshot der Seite vier Kreis Intersect Clip Unterschied-Vorgang")

Vier überlappende Kreise wurden aus Clippingbereichs entfernt.

Die **Clip Vorgänge** Seite veranschaulicht den Unterschied zwischen diesen beiden Operation mit nur einem Paar von Kreisen. Der erste Kreis auf der linken Seite mit Standard-Clip-Operation des Clippingbereichs hinzugefügt `Intersect`, während die zweite Kreis auf der rechten Seite mit Clip-Operation, die die Beschriftung erkennbar Clippingbereichs hinzugefügt wird:

[![](clipping-images//clipoperations-small.png "Dreifacher Screenshot der Seite Clip Vorgänge")](clipping-images/clipoperations-large.png#lightbox "dreifacher Screenshot der Seite Clip-Vorgänge")

Die [ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Klasse definiert zwei `SKPaint` -Objekte als Felder aus, und klicken Sie dann den Bildschirm einrichten in zwei rechteckige Bereiche unterteilt. Diese Bereiche sind unterschiedlich, je nachdem, ob das Telefon im Hochformat oder Querformat. Die `DisplayClipOp` Klasse zeigt den Text und ruft dann `ClipPath` mit den Pfaden zwei Kreis, um jede Clip-Operation zu veranschaulichen:

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

Aufrufen von `DrawPaint` normalerweise bewirkt, dass den gesamten Zeichenbereich mit, die gefüllt werden soll `SKPaint` -Objekt, aber in diesem Fall die Methode nur zeichnet innerhalb des Clippingbereichs,.

## <a name="exploring-regions"></a>Untersuchen von Regionen

Wenn Sie die API-Dokumentation für untersucht haben `SKCanvas`, haben Sie gesehen Überladungen der der `ClipPath` und `ClipRect` Methoden, sondern stattdessen vergleichbar mit den sind oben beschriebenen Methoden, haben einen Parameter namens [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) statt `SKClipOperation`. `SKRegionOperation` verfügt über sechs Elemente, die eine etwas mehr Flexibilität beim Kombinieren von Pfade zum Formular Clipping Bereiche:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Allerdings Überladungen der `ClipPath` und `ClipRect` mit `SKRegionOperation` Parameter sind veraltet und kann nicht verwendet werden.

Weiterhin können Sie die `SKRegionOperation` -Enumeration, aber es erfordert, dass Sie eine Clippingbereichs im Sinne von definieren eine [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) Objekt.

Ein neu erstelltes `SKRegion` Objekt beschreibt einen leeren Bereich. Der erste Aufruf für das Objekt in der Regel ist [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) , damit die Region beschreiben ein rechteckiges Bereichs. Der Parameter für `SetRect` ist eine eine `SKRectI` Wert & #x 2014; der Rechteckwert mit ganzzahligen Eigenschaften. Sie können dann aufrufen [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) mit einem `SKPath` Objekt. Dies erstellt eine Region, die unterscheidet sich das Innere des Pfads, aber die anfängliche rechteckigen Bereich abgeschnitten.

Die `SKRegionOperation` Enumeration wird nur eine sprachbasierte stammt, wenn der Aufruf einer der der [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) -methodenüberladungen, wie diese:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Die Region, die Sie vornehmen, die `Op` Anruf an, mit der Region, die als Parameter basierend auf angegebenen kombiniert die `SKRegionOperation` Member. Wenn Sie schließlich eine Region für Clipping geeignet erhalten, Sie können festlegen, als Clippingbereichs der Canvas mit der [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) Methode `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Der folgende Screenshot zeigt basierend auf der sechs bereichsvorgängen Clipping-Bereiche. Der linken Kreis entspricht der Region, die die `Op` Methode aufgerufen wird, und der richtige Kreis entspricht der Region, die zum Übergeben der `Op` Methode:

[![](clipping-images//regionoperations-small.png "Dreifacher Screenshot der Seite Bereichsvorgängen")](clipping-images/regionoperations-large.png#lightbox "dreifacher Screenshot der Seite Bereichsvorgänge")

Sind diese alle Möglichkeiten einer Kombination aus diesen zwei Kreise? Betrachten Sie das resultierende Bild, das eine Kombination aus drei Komponenten, die selbst in angezeigt werden die `Difference`, `Intersect`, und `ReverseDifference` Vorgänge. Die Gesamtanzahl der Kombinationen wird zwei und die dritte Potenz oder acht. Die beiden, die fehlen sind die ursprünglichen Region (die Ergebnisse des Aufrufs von nicht `Op` überhaupt) und einer Region vollständig leer.

Es ist schwieriger Regionen zum Kürzen, da müssen Sie zunächst einen Pfad, und klicken Sie dann eine Region aus, dass der Pfad zu erstellen, und kombinieren Sie mehrere Bereiche verwendet werden soll. Die allgemeine Struktur von den **Bereichsvorgängen** Seite ähnelt **Clip-Vorgänge** aber die [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) Klasse wird der Bildschirm einrichten, in sechs Kategorien und Zeigt den zusätzlichen Aufwand erforderlich, um Bereiche für diesen Auftrag verwenden:

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
> Im Gegensatz zu den `ClipPath` -Methode, die `ClipRegion` Methode ist nicht betroffen, von Transformationen.

Um die Gründe für diesen Unterschied zu verstehen, ist es hilfreich zu verstehen, welche ein Bereich ist. Wenn Sie über die Clip-Vorgänge oder bereichsvorgängen interne Implementierung werden möglicherweise angesehen haben, scheint es wahrscheinlich sehr kompliziert. Mehrere potenziell sehr komplexe Pfaden kombiniert werden, und die Gliederung der resultierende Pfad ist wahrscheinlich eine algorithmische des Verlusts.

Aber dieser Auftrag wird erheblich vereinfacht, wenn jeder Pfad auf eine Reihe von Zeilen, z. B. die altmodisch Vakuumröhre Spielkonsolen reduziert wird. Jede Scanzeile ist einfach eine horizontale Linie mit einem Start- und Endpunkt. Beispielsweise kann ein Kreis mit einem Radius von 10 in 20 Zeilen, zerlegt werden, von denen jedes an den linken Teil des Kreises beginnt und endet mit den rechten Teil. Kombinieren von zwei Kreise mit jeder Region-Vorgang wird sehr einfach, ist es lediglich die Anfangs- und Enddatum Koordinaten jedes Paars aus der entsprechenden Zeilen zu untersuchen.

Dadurch wird ein Bereich ist: eine Reihe von Zeilen, die einen Bereich definiert.

Jedoch, wenn ein Bereich reduziert wird, auf eine Reihe von Scan Zeilen, diese Überprüfung aus, auf denen ein bestimmtes Pixel Dimension Zeilen basieren. Streng genommen ist die Region keinen Vector Graphics-Objekts. Es ähnelt in der Art, eine komprimierte monochrome Bitmap als einem Pfad. Folglich können nicht Regionen skaliert oder gedreht werden, ohne Verlust der Genauigkeit und aus diesem Grund, die sie nicht transformiert werden, wenn für Clipping Bereiche verwendet werden.

Sie können jedoch Transformationen auf Regionen zu zeichnen anwenden. Die **Region Paint** Programm vividly zeigt den inneren Charakter Regionen. Die [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) -Klasse erstellt eine `SKRegion` -Objekt auf Grundlage einer `SKPath` Umfang eines Kreises 10 Einheiten Radius. Eine Transformation kann dann Kreises zum Ausfüllen der Seite erweitert werden:

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

Die `DrawRegion` Aufruf füllt den Bereich in Orange ändert, während die `DrawPath` aufrufen, die den ursprünglichen Pfad in Blau dargestellt, für den Vergleich Striche:

[![](clipping-images//regionpaint-small.png "Dreifacher Screenshot der Seite Region Paint")](clipping-images/regionpaint-large.png#lightbox "dreifacher Screenshot der Seite Region Paint")

Die Region ist ganz klar eine Reihe von diskreten Koordinaten.

Wenn Sie keine Transformationen im Zusammenhang mit Ihrem Clipping-Bereiche verwenden müssen, können Sie Bereiche für das Clipping, als die **vier--Endknoten Clover** veranschaulicht. Die [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Klasse erstellt eine zusammengesetzte Region über vier zirkuläre Regionen zusammengesetzten Region als Clippingbereichs festlegt und dann zeichnet eine Reihe von 360 gerade Linien, die von der Mitte der Seite ausgehen:

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

Es tatsächlich scheint eine vier – Leaf Clover allerdings handelt es sich um ein Bild, die sonst schwer zu ohne Clipping gerendert werden kann:

[![](clipping-images//fourleafclover-small.png "Dreifacher Screenshot der Seite vier--Endknoten Clover")](clipping-images/fourleafclover-large.png#lightbox "dreifacher Screenshot der Seite vier--Endknoten Clover")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
