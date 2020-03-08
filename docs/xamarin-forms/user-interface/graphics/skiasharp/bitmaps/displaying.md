---
title: Anzeigen von SkiaSharp-bitmaps
description: Erfahren Sie, wie SkiaSharp Bitmaps in Pixeln Größen und erweitert, um die Rechtecke ausfüllt, wobei das Seitenverhältnis wird beibehalten.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 9955b68346c74435a3a141c69d02e1bec5856bd3
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915898"
---
# <a name="displaying-skiasharp-bitmaps"></a>Anzeigen von SkiaSharp-bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Der Betreff von skiasharp-Bitmaps wurde im Artikel **[Bitmap-Grundlagen in skiasharp](../basics/bitmaps.md)** vorgestellt. Dieser Artikel wurde gezeigt, drei Möglichkeiten zum Laden von Bitmaps und drei Möglichkeiten zum Anzeigen von Bitmaps. Dieser Artikel befasst sich mit den Techniken zum Laden von Bitmaps und vertieft die Verwendung der `DrawBitmap` Methoden `SKCanvas`.

![Anzeigen von Beispielen](displaying-images/DisplayingSample.png "Anzeigen von Beispielen")

Die Methoden `DrawBitmapLattice` und `DrawBitmapNinePatch` werden im Artikel **[segmentierte Anzeige von skiasharp-Bitmaps](segmented.md)** erläutert.

Die Beispiele auf dieser Seite stammen aus der Anwendung **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** . Wählen Sie auf der Startseite dieser Anwendung **skiasharp-Bitmaps**aus, und navigieren Sie dann zum Abschnitt **Anzeigen von Bitmaps** .

## <a name="loading-a-bitmap"></a>Laden eine bitmap

Eine Bitmap, die in der Regel von einer SkiaSharp-Anwendung verwendet stammt aus einer der drei verschiedenen Quellen:

- Über das Internet
- Aus einer Ressource in die ausführbare Datei eingebettet
- Aus der Fotobibliothek des Benutzers

Es ist auch möglich, für eine Anwendung SkiaSharp, erstellen eine neue Bitmap, und klicken Sie dann darauf zeichnen oder der Bitmap-Bits algorithmisch festgelegt. Diese Techniken werden in den Artikeln **[Erstellen und Zeichnen von skiasharp-Bitmaps](drawing.md)** und **[zugreifen auf skiasharp-Bitmap-Pixel](pixel-bits.md)** erläutert.

In den folgenden drei Codebeispielen für das Laden einer Bitmap wird angenommen, dass die-Klasse ein Feld vom Typ "`SKBitmap`" enthält:

```csharp
SKBitmap bitmap;
```

Da der Artikel **[Bitmap-Grundlagen in skiasharp](../basics/bitmaps.md)** besagt, ist die beste Möglichkeit, eine Bitmap über das Internet zu laden, mit der [`HttpClient`](xref:System.Net.Http.HttpClient) -Klasse. Eine einzelne Instanz der Klasse kann als Feld definiert werden:

```csharp
HttpClient httpClient = new HttpClient();
```

Wenn Sie `HttpClient` mit IOS-und Android-Anwendungen verwenden, möchten Sie die Projekteigenschaften wie in den Dokumenten auf **[Transport Layer Security (TLS) 1,2](~/cross-platform/app-fundamentals/transport-layer-security.md)** beschrieben festlegen.

Code, der `HttpClient` verwendet, umfasst häufig den `await`-Operator, sodass er sich in einer `async`-Methode befinden muss:

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

Beachten Sie, dass das `Stream` Objekt, das von `GetStreamAsync` abgerufen wird, in eine `MemoryStream`kopiert wird. Android lässt nicht zu, dass die `Stream` von `HttpClient` vom Haupt Thread verarbeitet werden, außer in asynchronen Methoden. 

Der [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) führt viel Arbeit aus: das an ihn über gegebene `Stream` Objekt verweist auf einen Speicherblock, der eine vollständige Bitmap in einem der allgemeinen Bitmapdateiformate (im allgemeinen JPEG, PNG oder GIF) enthält. Die `Decode`-Methode muss das Format bestimmen und dann die Bitmapdatei in das interne Bitmap-Format von skiasharp decodieren.

Nachdem der Code `SKBitmap.Decode`aufgerufen hat, wird er wahrscheinlich die `CanvasView` ungültig machen, damit der `PaintSurface`-Handler die neu geladene Bitmap anzeigen kann.

Die zweite Möglichkeit zum Laden einer Bitmaps wird durch die einzelnen Plattformprojekte dazu die Bitmap als eingebettete Ressource in der .NET Standard-Bibliothek verwiesen. Eine Ressourcen-ID wird an die [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) -Methode übermittelt. Diese Ressourcen-ID besteht aus den Assemblynamen, Ordnernamen und Dateinamen der Ressource durch Punkte getrennt sind:

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

Bitmap-Dateien können auch als Ressourcen im plattformprojekt einzelne für iOS, Android und die universelle Windows-Plattform (UWP) gespeichert werden. Laden diese Bitmaps erfordert jedoch Code, der im plattformprojekt befindet.

Ein dritter Ansatz beim Abrufen einer Bitmaps wird von der Bildbibliothek des Benutzers. Im folgenden Code wird ein Abhängigkeits Dienst verwendet, der in der **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Anwendung enthalten ist. Die **skiasharpformsdemo** -.NET Standard Bibliothek umfasst die `IPhotoLibrary` Schnittstelle, während jedes der Platt Form Projekte eine `PhotoLibrary` Klasse enthält, die diese Schnittstelle implementiert.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

Im Allgemeinen wird der `CanvasView` durch solchen Code ebenfalls für ungültig erklärt, sodass der `PaintSurface` Handler die neue Bitmap anzeigen kann.

Die `SKBitmap`-Klasse definiert mehrere nützliche Eigenschaften, einschließlich [`Width`](xref:SkiaSharp.SKBitmap.Width) und [`Height`](xref:SkiaSharp.SKBitmap.Height), die die Pixel Dimensionen der Bitmap sowie viele Methoden, einschließlich der Methoden zum Erstellen von Bitmaps, zum Kopieren und zum verfügbar machen der Pixel Bits, enthalten. 

## <a name="displaying-in-pixel-dimensions"></a>Anzeigen von in die Pixeldimensionen

Die skiasharp- [`Canvas`](xref:SkiaSharp.SKCanvas) -Klasse definiert vier `DrawBitmap`-Methoden. Diese Methoden ermöglichen die Bitmaps auf zwei grundlegend verschiedene Arten angezeigt werden: 

- Wenn Sie einen `SKPoint` Wert angeben (oder separate Werte für `x` und `y`), wird die Bitmap in ihren Pixel Dimensionen angezeigt. Die Pixel der Bitmap werden direkt in Pixel der Videoanzeige zugeordnet.
- Angeben eines Rechtecks bewirkt, dass die Bitmap, auf die Größe und Form des Rechtecks gestreckt wird. 

Sie zeigen eine Bitmap in ihren Pixel Dimensionen an, indem Sie [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint)) mit einem `SKPoint`-Parameter oder [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) mit separaten `x`-und `y`-Parametern verwenden:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Diese beiden Methoden sind funktional identisch. Der angegebene Punkt gibt den Speicherort der oberen linken Ecke der Bitmap relativ zur Leinwand an. Da die Pixel-Auflösung von mobilen Geräten so hoch ist, werden kleinere Bitmaps in der Regel ziemlich winzig auf diesen Geräten angezeigt.

Mit dem optionalen `SKPaint`-Parameter können Sie die Bitmap mithilfe von Transparenz anzeigen. Erstellen Sie zu diesem Zweck ein `SKPaint` Objekt, und legen Sie die `Color`-Eigenschaft auf einen beliebigen `SKColor` Wert mit einem Alphakanal unter 1 fest. Beispiel:

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

Der 0 x 80, der als letztes Argument übergeben, gibt die Transparenz von 50 % an. Sie können auch einen alpha-Kanal auf eine der vordefinierten Farben festlegen:

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

Die Farbe an sich ist jedoch nicht relevant. Nur der Alphakanal wird untersucht, wenn Sie das `SKPaint`-Objekt in einem `DrawBitmap`-Befehl verwenden.

Das `SKPaint`-Objekt spielt außerdem eine Rolle, wenn Bitmaps mithilfe von Blend-Modi oder Filtereffekten angezeigt werden. Diese werden in den Artikeln [skiasharp Zusammensetzung und Blend-Modi](../effects/blend-modes/index.md) und [skiasharp-Bild Filtern](../effects/image-filters.md)veranschaulicht.

Die Seite **Pixel Dimensionen** im Beispielprogramm **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** zeigt eine Bitmap-Ressource an, die 320 Pixel breit und 240 Pixel hoch ist:

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Der `PaintSurface` Handler zentriert die Bitmap, indem `x` und `y` Werte basierend auf den Pixel Dimensionen der Anzeige Oberfläche und den Pixel Dimensionen der Bitmap berechnet werden:

[![Pixel Dimensionen](displaying-images/PixelDimensions.png "Pixel Dimensionen")](displaying-images/PixelDimensions-Large.png#lightbox)

Wenn möchte, dass die Anwendung in der oberen linken Ecke der Bitmap anzuzeigen, übergeben sie einfach Koordinaten (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Eine Methode zum Laden von Bitmaps für Ressource

Viele der Beispiele an, Bitmapressourcen geladen werden müssen. Die statische `BitmapExtensions`-Klasse in der **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Projekt Mappe enthält eine Methode, um Folgendes zu unterstützen:

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

Beachten Sie den `Type`-Parameter. Dabei kann es sich um das `Type` Objekt handeln, das einem beliebigen Typ in der Assembly zugeordnet ist, die die Bitmapressource speichert.

Diese `LoadBitmapResource` Methode wird in allen nachfolgenden Beispielen verwendet, die Bitmapressourcen erfordern.

## <a name="stretching-to-fill-a-rectangle"></a>Strecken, um ein Rechteck zu füllen.

Die `SKCanvas`-Klasse definiert außerdem eine [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) Methode, die die Bitmap in ein Rechteck rendert, und eine weitere [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) -Methode, die eine rechteckige Teilmenge der Bitmap in einem Rechteck rendert:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

In beiden Fällen wird die Bitmap gestreckt, um das Rechteck mit dem Namen `dest`auszufüllen. In der zweiten Methode können Sie mithilfe des `source` Rechtecks eine Teilmenge der Bitmap auswählen. Das `dest` Rechteck ist relativ zum Ausgabegerät. Das `source` Rechteck ist relativ zur Bitmap.

Die Seite **Füll Rechteck** zeigt die ersten dieser beiden Methoden an, indem die gleiche Bitmap angezeigt wird, die im vorherigen Beispiel in einem Rechteck mit derselben Größe wie der Canvas verwendet wurde: 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

Beachten Sie die Verwendung der neuen `BitmapExtensions.LoadBitmapResource`-Methode, um das `SKBitmap` Feld festzulegen. Das Ziel Rechteck wird aus der [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) -Eigenschaft von `SKImageInfo`abgerufen, die die Größe der Anzeige Oberfläche einschränkt:

[![Füll Rechteck](displaying-images/FillRectangle.png "Füll Rechteck")](displaying-images/FillRectangle-Large.png#lightbox)

Dies ist in der Regel _nicht_ das gewünschte. Durch anders in horizontaler und vertikaler Richtung gestreckt wird, wird das Bild verzerrt. Wenn Sie eine Bitmap in einen anderen Wert als die Größe in Pixel anzeigen zu können, möchten in der Regel die Bitmap Originalseitenverhältnis beibehalten.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>Dehnen von Beibehaltung des Seitenverhältnisses

Das Strecken einer Bitmap unter Beibehaltung des Seitenverhältnisses ist ein Prozess, der auch als _einheitliche Skalierung_bezeichnet wird. Dieser Begriff Lösungsansätze algorithmische. Eine mögliche Lösung wird auf der Seite " **einheitliche Skalierung** " angezeigt:

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

Der `PaintSurface`-Handler berechnet einen `scale` Faktor, bei dem es sich um das minimale Verhältnis der Anzeigebreite und Höhe zur bitmapbreite und-Höhe handelt. Die Werte für `x` und `y` können dann für das Zentrieren der skalierten Bitmap innerhalb der Anzeigebreite und-Höhe berechnet werden. Das Ziel Rechteck hat eine linke obere Ecke von `x` und `y` sowie eine untere rechte Ecke dieser Werte zuzüglich der skalierten Breite und Höhe der Bitmap:

[![Einheitliche Skalierung](displaying-images/UniformScaling.png "Einheitliche Skalierung")](displaying-images/UniformScaling-Large.png#lightbox)

Aktivieren Sie das Telefon seitwärts drehen, um die Bitmap gestreckt, um Sie zu diesem Bereich finden Sie unter:

[![Einheitliche Skalierungs Landschaft](displaying-images/UniformScaling-Landscape.png "Einheitliche Skalierungs Landschaft")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Der Vorteil der Verwendung dieses `scale` Faktors wird offensichtlich, wenn Sie einen etwas anderen Algorithmus implementieren möchten. Angenommen Sie, Sie möchten die Bitmap-Seitenverhältnis beibehalten, aber auch das Zielrechteck auszufüllen. Dies ist nur möglich, indem Sie einen Teil des Bilds zuschneiden, aber Sie können diesen Algorithmus einfach implementieren, indem Sie `Math.Max` `Math.Min` im obigen Code ändern. Das Ergebnis lautet wie folgt: 

[![Einheitliche Skalierungs Alternative](displaying-images/UniformScaling-Alternative.png "Einheitliche Skalierungs Alternative")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Die Bitmap-Seitenverhältnis wird beibehalten, jedoch Bereiche, links und rechts neben die Bitmap zugeschnitten sind.

## <a name="a-versatile-bitmap-display-function"></a>Eine vielseitige Bitmap-Anzeige-Funktion

XAML-basierte programmierumgebungen (z. B. UWP- und Xamarin.Forms) haben eine Funktion zum Erweitern oder verkleinern, Bitmaps und gleichzeitig ihre Seitenverhältnisse. Obwohl dieses Feature von SkiaSharp nicht enthalten ist, können Sie es selbst implementieren. Die `BitmapExtensions`-Klasse, die in der [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Anwendung enthalten ist, zeigt, wie. Die-Klasse definiert zwei neue `DrawBitmap` Methoden, die die Berechnung des Seitenverhältnisses ausführen. Diese neuen Methoden sind Erweiterungs Methoden von `SKCanvas`.

Die neuen `DrawBitmap` Methoden enthalten einen Parameter vom Typ "`BitmapStretch`", eine in der **BitmapExtensions.cs** -Datei definierte Enumeration:

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

Die `None`-, `Fill`-, `Uniform`-und `UniformToFill`-Member sind identisch mit denen in der UWP- [`Stretch`](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) -Enumeration. Die ähnliche xamarin. Forms- [`Aspect`](xref:Xamarin.Forms.Aspect) -Enumeration definiert Member `Fill`, `AspectFit`und `AspectFill`.

Die oben gezeigte **einheitliche Skalierungs** Seite zentriert die Bitmap innerhalb des Rechtecks, aber Sie möchten möglicherweise andere Optionen, z. b. das Positionieren der Bitmap auf der linken oder rechten Seite des Rechtecks oder das obere oder untere Ende. Dies ist der Zweck der `BitmapAlignment`-Enumeration:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Ausrichtungs Einstellungen haben keine Auswirkungen, wenn Sie mit `BitmapStretch.Fill`verwendet werden.

Die erste `DrawBitmap` Erweiterungs Funktion enthält ein Ziel Rechteck, aber kein Quell Rechteck. Standardwerte werden definiert, sodass Sie, wenn Sie die Bitmap zentriert haben möchten, nur einen `BitmapStretch` Member angeben müssen:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

Der Hauptzweck dieser Methode besteht darin, einen Skalierungsfaktor namens `scale` zu berechnen, der dann beim Aufrufen der `CalculateDisplayRect`-Methode auf die Breite und Höhe der Bitmap angewendet wird. Dies ist die Methode, die ein Rechteck für die Anzeige der Bitmap, die basierend auf der horizontalen und vertikalen Ausrichtung berechnet:

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

Die `BitmapExtensions`-Klasse enthält eine zusätzliche `DrawBitmap`-Methode mit einem Quell Rechteck zum Angeben einer Teilmenge der Bitmap. Diese Methode ähnelt der ersten, mit der Ausnahme, dass der Skalierungsfaktor auf der Grundlage des `source` Rechtecks berechnet und dann auf das `source` Rechteck im `CalculateDisplayRect`aufgerufen wird:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

Die erste dieser zwei neuen `DrawBitmap` Methoden wird auf der Seite **Skalierungs Modi** veranschaulicht. Die XAML-Datei enthält drei `Picker` Elemente, mit denen Sie Member der `BitmapStretch` und `BitmapAlignment` Enumerationen auswählen können:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei macht die `CanvasView` einfach ungültig, wenn sich ein `Picker` Element geändert hat. Der `PaintSurface` Handler greift auf die drei `Picker` Ansichten zum Aufrufen der `DrawBitmap`-Erweiterungsmethode zu:

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

Hier sind einige Kombinationen von Optionen aus:

[![Skalierungs Modi](displaying-images/ScalingModes.png "Skalierungs Modi")](displaying-images/ScalingModes-Large.png#lightbox)

Die **untergeordnete Seite des Rechtecks** enthält praktisch die gleiche XAML-Datei wie die **Skalierungs Modi**, aber die Code-Behind-Datei definiert eine rechteckige Teilmenge der Bitmap, die vom `SOURCE` Feld angegeben wird: 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

Diese Quelle Rechteck isoliert Head für das Monkey-Objekt, wie im folgenden Screenshots gezeigt:

[![Rechteck Teilmenge](displaying-images/RectangleSubset.png "Rechteck Teilmenge")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
