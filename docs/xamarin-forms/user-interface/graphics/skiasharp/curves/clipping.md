---
Title: "Abschneiden mit Pfaden und Regionen" Beschreibung: "in diesem Artikel wird erläutert, wie Sie skiasharp-Pfade verwenden können, um Grafiken in bestimmte Bereiche zu schneiden und um Regionen zu erstellen, und dies mit Beispielcode veranschaulicht."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 8022fbf9-2208-43DB-94d8-0a4e9a5da07f Author: davidbritch ms. Author: dabritch ms. Date: 06/16/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="clipping-with-paths-and-regions"></a>Schneiden mit Pfaden und Regionen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden von Pfaden zum Ausschneiden von Grafiken an bestimmte Bereiche und zum Erstellen von Regionen_

Manchmal ist es erforderlich, das Rendering von Grafiken auf einen bestimmten Bereich zu beschränken. Dies wird als *Clipping*bezeichnet. Sie können Clipping für besondere Effekte verwenden, wie z. b. dieses Bild eines Affen, der durch eine Keyhole angezeigt wird:

![Affen durch eine Keyhole](clipping-images/clippingsample.png)

Der *Clippingbereich* ist der Bereich des Bildschirms, in dem Grafiken gerendert werden. Alle Elemente, die außerhalb des Clippingbereichs angezeigt werden, werden nicht gerendert. Der Clippingbereich wird normalerweise durch ein Rechteck oder ein [`SKPath`](xref:SkiaSharp.SKPath) Objekt definiert, aber Sie können alternativ einen Clippingbereich mithilfe eines [`SKRegion`](xref:SkiaSharp.SKRegion) Objekts definieren. Diese beiden Typen von Objekten werden zuerst verknüpft, da Sie einen Bereich aus einem Pfad erstellen können. Es ist jedoch nicht möglich, einen Pfad aus einer Region zu erstellen, und Sie unterscheiden sich intern: ein Pfad besteht aus einer Reihe von Linien und Kurven, während ein Bereich durch eine Reihe horizontaler Scan Linien definiert wird.

Das Bild oben wurde von der Seite " **affell durch Keyhole** " erstellt. Die [`MonkeyThroughKeyholePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) -Klasse definiert einen Pfad mithilfe von SVG-Daten und verwendet den-Konstruktor, um eine Bitmap aus den Programmressourcen zu laden:

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

Obwohl das- `keyholePath` Objekt die Gliederung einer Keyhole beschreibt, sind die Koordinaten ganz willkürlich und geben an, was beim Entwerfen der Pfaddaten praktisch war. Aus diesem Grund erhält der `PaintSurface` Handler die Grenzen dieses Pfads und ruft und auf, um `Translate` `Scale` den Pfad in den Mittelpunkt des Bildschirms zu verschieben und ihn so hoch wie den Bildschirm zu machen:

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

Der Pfad wird jedoch nicht gerendert. Stattdessen wird der Pfad verwendet, um einen Clippingbereich mit folgender Anweisung festzulegen:

```csharp
canvas.ClipPath(keyholePath);
```

Der `PaintSurface` Handler setzt dann die Transformationen mit einem-Rückruf zurück `ResetMatrix` und zeichnet die Bitmap, um die vollständige Höhe des Bildschirms zu erweitern. Bei diesem Code wird davon ausgegangen, dass die Bitmap quadratisch ist. Die Bitmap wird nur innerhalb des Bereichs gerendert, der durch den clippingpfad definiert wird:

[![Dreifacher Screenshot der Seite "Monkey through Keyhole"](clipping-images/monkeythroughkeyhole-small.png)](clipping-images/monkeythroughkeyhole-large.png#lightbox)

Der clippingpfad unterliegt den Transformationen, die wirksam werden, wenn die- `ClipPath` Methode aufgerufen wird, und nicht den Transformationen, die wirksam werden, wenn ein grafisches Objekt (z. b. eine Bitmap) angezeigt wird. Der clippingpfad ist Teil des Canvas-Zustands, der mit der `Save` -Methode gespeichert und mit der-Methode wieder hergestellt wird `Restore` .

## <a name="combining-clipping-paths"></a>Kombinieren von clippingpfaden

Streng genommen wird der Clippingbereich nicht durch die-Methode festgelegt `ClipPath` . Stattdessen wird Sie mit dem vorhandenen clippingpfad kombiniert, der als Rechteck mit der Größe des Zeichen Bereichs beginnt. Sie können die rechteckigen Begrenzungen des Clippingbereichs mithilfe der- [`ClipBounds`](xref:SkiaSharp.SKCanvas.ClipBounds) Eigenschaft oder der- [`ClipDeviceBounds`](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) Eigenschaft abrufen. Die- `ClipBounds` Eigenschaft gibt einen- `SKRect` Wert zurück, der alle Transformationen widerspiegelt, die möglicherweise wirksam sind. Die- `ClipDeviceBounds` Eigenschaft gibt einen- `RectI` Wert zurück. Dabei handelt es sich um ein Rechteck mit ganzzahligen Dimensionen, und der Clippingbereich wird in den eigentlichen Pixel Dimensionen beschrieben.

Jeder-Befehl `ClipPath` reduziert den Clippingbereich, indem der Clippingbereich mit einem neuen Bereich kombiniert wird. Die vollständige Syntax der- [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) Methode lautet wie folgt:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Es gibt auch eine [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean)) Methode, die den Clippingbereich mit einem Rechteck kombiniert:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Standardmäßig ist der resultierende Clippingbereich eine Schnittmenge des vorhandenen Clippingbereichs und des `SKPath` oder `SKRect` , das in der- `ClipPath` Methode oder der-Methode angegeben ist `ClipRect` . Dies wird auf der **Intersect-Clip Seite der vier Kreise** veranschaulicht. Der- `PaintSurface` Handler in der- [`FourCircleInteresectClipPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) Klasse verwendet dasselbe- `SKPath` Objekt, um vier überlappende Kreise zu erstellen, von denen jede den Clippingbereich durch aufeinander folgende Aufrufe von reduziert `ClipPath` :

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

Links ist die Schnittmenge dieser vier Kreise:

[![Dreifacher Screenshot der vier Kreis übergreifenden Intersect-Clip page](clipping-images//fourcircleintersectclip-small.png)](clipping-images/fourcircleintersectclip-large.png#lightbox)

Die- [`SKClipOperation`](xref:SkiaSharp.SKClipOperation) Enumeration verfügt nur über zwei Member:

- `Difference`entfernt den angegebenen Pfad oder das angegebene Rechteck aus dem vorhandenen Clippingbereich.

- `Intersect`schneidet den angegebenen Pfad oder das Rechteck mit dem vorhandenen Clippingbereich.

Wenn Sie die vier `SKClipOperation.Intersect` Argumente in der- `FourCircleIntersectClipPage` Klasse durch ersetzen `SKClipOperation.Difference` , wird Folgendes angezeigt:

[![Dreifacher Screenshot der vier Kreis übergreifenden Intersect-Clip Page mit Differenz Vorgang](clipping-images//fourcircledifferenceclip-small.png)](clipping-images/fourcircledifferenceclip-large.png#lightbox)

Aus dem Clippingbereich wurden vier überlappende Kreise entfernt.

Auf der Seite **Clip Vorgänge** wird der Unterschied zwischen diesen beiden Vorgängen mit nur einem Paar von Kreisen veranschaulicht. Der erste Kreis auf der linken Seite wird dem Clippingbereich mit dem Standard Clip Vorgang hinzugefügt `Intersect` , während der zweite Kreis auf der rechten Seite dem Clippingbereich mit dem Clip Vorgang hinzugefügt wird, der durch die Bezeichnung Text angegeben ist:

[![Dreifacher Screenshot der Seite "Clip Vorgänge"](clipping-images//clipoperations-small.png)](clipping-images/clipoperations-large.png#lightbox)

Die [`ClipOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) -Klasse definiert zwei `SKPaint` -Objekte als Felder und teilt den Bildschirm dann in zwei rechteckige Bereiche auf. Diese Bereiche unterscheiden sich abhängig davon, ob sich das Telefon im hoch-oder Querformat befindet. Die `DisplayClipOp` Klasse zeigt dann den Text an und ruft `ClipPath` mit den beiden Kreis Pfaden auf, um die einzelnen Clip Vorgänge zu veranschaulichen:

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

Das Aufrufen `DrawPaint` von normal bewirkt, dass der gesamte Zeichenbereich mit diesem Objekt gefüllt wird `SKPaint` . in diesem Fall zeichnet die Methode jedoch nur innerhalb des Clippingbereichs.

## <a name="exploring-regions"></a>Erkunden von Regionen

Sie können auch einen Clippingbereich in Bezug auf ein [`SKRegion`](xref:SkiaSharp.SKRegion) Objekt definieren.

Ein neu erstelltes `SKRegion` Objekt beschreibt einen leeren Bereich. Normalerweise ist der erste-Befehl für das-Objekt [`SetRect`](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) , sodass der Bereich einen rechteckigen Bereich beschreibt. Der-Parameter für `SetRect` ist `SKRectI` ein &mdash; Rechteck mit ganzzahligen Koordinaten, da es das Rechteck in Bezug auf Pixel angibt. Sie können dann [`SetPath`](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) mit einem- `SKPath` Objekt aufzurufen. Dadurch wird ein Bereich erstellt, der mit dem Inneren des Pfads identisch ist, aber auf den ursprünglichen rechteckigen Bereich zugeschnitten ist.

Der Bereich kann auch durch Aufrufen einer der- [`Op`](xref:SkiaSharp.SKRegion.Op*) Methoden Überladungen geändert werden, wie z. b.:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Die- [`SKRegionOperation`](xref:SkiaSharp.SKRegionOperation) Enumeration ähnelt, `SKClipOperation` weist jedoch mehr Elemente auf:

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

Der Bereich, für den Sie den-Befehl ausführen, `Op` wird mit der als Parameter angegebenen Region kombiniert, die auf dem- `SKRegionOperation` Member basiert. Wenn Sie schließlich eine für Clipping geeignete Region erhalten, können Sie diese mithilfe der-Methode von als Ausschneide Bereich der Canvas festlegen [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) `SKCanvas` :

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Der folgende Screenshot zeigt Clippingbereiche basierend auf den sechs Regions Vorgängen. Der linke Kreis ist der Bereich `Op` , für den die Methode aufgerufen wird, und der Rechte Kreis ist der an die Methode übergebenen Bereich `Op` :

[![Dreifacher Screenshot der Seite "Regions Vorgänge"](clipping-images//regionoperations-small.png)](clipping-images/regionoperations-large.png#lightbox)

Gibt es alle Möglichkeiten, diese beiden Kreise zu kombinieren? Betrachten Sie das resultierende Image als eine Kombination aus drei Komponenten, die selbst in den `Difference` `Intersect` Vorgängen, und sichtbar sind `ReverseDifference` . Die Gesamtanzahl der Kombinationen beträgt zwei bis zum dritten Strom oder acht. Die zwei, die fehlen, sind der ursprüngliche Bereich (der nicht aufgerufen wird `Op` ) und ein vollständig leerer Bereich.

Es ist schwieriger, Bereiche für das Abschneiden zu verwenden, da Sie zuerst einen Pfad erstellen und dann eine Region aus diesem Pfad erstellen und dann mehrere Regionen kombinieren müssen. Die Gesamtstruktur der Seite " **Regions Vorgänge** " ähnelt Clip- **Vorgängen** , aber die [`RegionOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) Klasse teilt den Bildschirm in sechs Bereiche auf und zeigt die zusätzliche Arbeit, die für die Verwendung von Bereichen für diesen Auftrag erforderlich ist:

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

Im folgenden finden Sie einen großen Unterschied zwischen der- `ClipPath` Methode und der- `ClipRegion` Methode:

> [!IMPORTANT]
> Anders als bei der- `ClipPath` Methode wird die- `ClipRegion` Methode von Transformationen nicht beeinträchtigt.

Um die Begründung für diesen Unterschied zu verstehen, ist es hilfreich zu verstehen, was eine Region ist. Wenn Sie sich Gedanken darüber gemacht haben, wie die cliingvorgänge oder Regions Vorgänge intern implementiert werden können, scheint dies sehr kompliziert zu sein. Mehrere potenziell sehr komplexe Pfade werden kombiniert, und der Umriss des resultierenden Pfades ist wahrscheinlich ein algorithmischer Alptraum.

Dieser Auftrag wird erheblich vereinfacht, wenn jeder Pfad auf eine Reihe horizontaler Scan Zeilen reduziert wird, z. b Jede Scan Zeile ist einfach eine horizontale Linie mit einem Startpunkt und einem Endpunkt. So kann z. b. ein Kreis mit einem Radius von 10 Pixeln in 20 horizontale Scan Zeilen zerlegt werden, von denen jeder im linken Bereich des Kreises beginnt und am rechten Teil endet. Die Kombination zweier Kreise mit einem beliebigen Regions Vorgang wird sehr einfach, da es sich lediglich um die Untersuchung der Start-und Endkoordinaten der einzelnen Paare entsprechender Überprüfungs Linien handelt.

Dies ist eine Region: eine Reihe von horizontalen Scan Linien, die einen Bereich definieren.

Wenn ein Bereich jedoch auf eine Reihe von Scan Zeilen reduziert wird, basieren diese Scan Zeilen auf einer bestimmten Pixel Dimension. Streng genommen ist die Region kein Vektorgrafik Objekt. Es ist eher eine komprimierte, monochrome Bitmap als ein Pfad. Folglich können Regionen nicht ohne Verlust der Genauigkeit skaliert oder gedreht werden. aus diesem Grund werden Sie nicht transformiert, wenn Sie für Clippingbereiche verwendet werden.

Sie können jedoch Transformationen für Zeichnungs Zwecke auf Regionen anwenden. Das **Regions** Zeichnungsprogramm veranschaulicht anschaulich die innere Natur der Regionen. Die- [`RegionPaintPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Klasse erstellt ein- `SKRegion` Objekt auf der Grundlage eines `SKPath` eines RADIUS-Kreises von 10 Einheiten. Eine Transformation erweitert dann den Kreis, um die Seite auszufüllen:

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

Der-Befehl `DrawRegion` füllt den Bereich in Orange, während der-Befehl `DrawPath` den ursprünglichen Pfad für den Vergleich in blau aufzeichnet:

[![Dreifacher Screenshot der Seite "Regions Farbe"](clipping-images//regionpaint-small.png)](clipping-images/regionpaint-large.png#lightbox)

Der Bereich ist eindeutig eine Reihe diskreter Koordinaten.

Wenn Sie in Verbindung mit ihren Clippingbereichen keine Transformationen verwenden müssen, können Sie die Bereiche für das Abschneiden verwenden, wie die Seite mit den **vier Blatt-Clover** veranschaulicht. Die- [`FourLeafCloverPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Klasse erstellt einen Verbund Bereich aus vier kreisförmigen Bereichen, legt diesen Verbund Bereich als Clippingbereich fest und zeichnet dann eine Reihe von 360 geraden Linien aus dem Mittelpunkt der Seite:

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

Es sieht nicht wirklich wie ein vier-Blatt-Clover aus, aber es ist ein Bild, das andernfalls schwierig zu renderingohne Clipping werden kann:

[![Dreifacher Screenshot der vier Blatt-Clover-Seite](clipping-images//fourleafclover-small.png)](clipping-images/fourleafclover-large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
