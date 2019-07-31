---
title: Anzeigen von SkiaSharp-bitmaps
description: Erfahren Sie, wie SkiaSharp Bitmaps in Pixeln Größen und erweitert, um die Rechtecke ausfüllt, wobei das Seitenverhältnis wird beibehalten.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 47b4a1bb0249bc21bd75e82067cb00b3f272e202
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642679"
---
# <a name="displaying-skiasharp-bitmaps"></a>Anzeigen von SkiaSharp-bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Das Subjekt der Bitmaps, SkiaSharp wurde eingeführt, in diesem Artikel  **[Bitmap-Grundlagen in SkiaSharp](../basics/bitmaps.md)** . Dieser Artikel wurde gezeigt, drei Möglichkeiten zum Laden von Bitmaps und drei Möglichkeiten zum Anzeigen von Bitmaps. In diesem Artikel werden die Techniken zum Laden von Bitmaps und geht tiefer in die Verwendung der `DrawBitmap` Methoden `SKCanvas`.

![Anzeigen von Beispiel](displaying-images/DisplayingSample.png "Beispiel anzeigen")

Die `DrawBitmapLattice` und `DrawBitmapNinePatch` Methoden sind in diesem Artikel erläuterten  **[segmentierte Anzeige der SkiaSharp-Bitmaps](segmented.md)** .

Beispiele auf dieser Seite stammen aus der **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** Anwendung. Wählen Sie auf der Startseite der Anwendung **SkiaSharp Bitmaps**, und navigieren Sie zu der **Anzeigen von Bitmaps** Abschnitt.

## <a name="loading-a-bitmap"></a>Laden eine bitmap

Eine Bitmap, die in der Regel von einer SkiaSharp-Anwendung verwendet stammt aus einer der drei verschiedenen Quellen:

- Über das Internet
- Aus einer Ressource in die ausführbare Datei eingebettet
- Aus der Fotobibliothek des Benutzers

Es ist auch möglich, für eine Anwendung SkiaSharp, erstellen eine neue Bitmap, und klicken Sie dann darauf zeichnen oder der Bitmap-Bits algorithmisch festgelegt. Diese Techniken werden in den Artikeln erläutert **[erstellen und Zeichnen auf SkiaSharp Bitmaps](drawing.md)** und **[Bitmap-Pixel für den Zugriff auf SkiaSharp](pixel-bits.md)** .

In den folgenden drei Codebeispielen, laden Sie eine Bitmap, die Klasse wird davon ausgegangen, dass ein Feld des Typs enthalten `SKBitmap`:

```csharp
SKBitmap bitmap;
```

Wie der Artikel **[Bitmap-Grundlagen in SkiaSharp](../basics/bitmaps.md)** erwähnt, ist die beste Möglichkeit zum Laden einer Bitmaps über das Internet mit dem [ `HttpClient` ](xref:System.Net.Http.HttpClient) Klasse. Eine einzelne Instanz der Klasse kann als Feld definiert werden:

```csharp
HttpClient httpClient = new HttpClient();
```

Bei Verwendung `HttpClient` mit iOS und Android-Anwendungen, sollten Sie Projekteigenschaften festlegen, wie beschrieben in den Dokumenten auf  **[Transport Layer Security (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** .

Code, der verwendet `HttpClient` beinhaltet häufig die `await` Operator, sodass es im befinden muss ein `async` Methode:

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

Beachten Sie, dass die `Stream` -Objekts vom `GetStreamAsync` in kopiert eine `MemoryStream`. Android lässt nicht zu den `Stream` aus `HttpClient` durch den Hauptthread in asynchronen Methoden außer verarbeitet werden. 

Der [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) erledigt viele Aufgaben: Das `Stream` an das Objekt über gegebene Objekt verweist auf einen Speicherblock, der eine vollständige Bitmap in einem der allgemeinen Bitmapdateiformate (im allgemeinen JPEG, PNG oder GIF) enthält. Die `Decode` Methode bestimmen Sie das Format muss, und die Bitmapdatei klicken Sie dann in SkiaSharps-interne Bitmap-Format zu decodieren.

Nach dem der Code ruft `SKBitmap.Decode`, es werden möglicherweise ungültig. die `CanvasView` , damit die `PaintSurface` Handler kann anzeigen, die neu geladene Bitmap.

Die zweite Möglichkeit zum Laden einer Bitmaps wird durch die einzelnen Plattformprojekte dazu die Bitmap als eingebettete Ressource in der .NET Standard-Bibliothek verwiesen. Eine Ressource, die ID wird an die [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) Methode. Diese Ressourcen-ID besteht aus den Assemblynamen, Ordnernamen und Dateinamen der Ressource durch Punkte getrennt sind:

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

Ein dritter Ansatz beim Abrufen einer Bitmaps wird von der Bildbibliothek des Benutzers. Der folgende Code verwendet einen Abhängigkeitsdienst, der in enthalten ist das **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** Anwendung. Die **SkiaSharpFormsDemo** .NET Standard-Bibliothek enthält die `IPhotoLibrary` Schnittstelle, während jede der Plattformprojekte enthält eine `PhotoLibrary` -Klasse, die diese Schnittstelle implementiert.

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

In der Regel solchen Code auch erklärt die `CanvasView` , damit die `PaintSurface` Handler kann die neue Bitmap angezeigt.

Die `SKBitmap` -Klasse definiert einige nützliche Eigenschaften, einschließlich [ `Width` ](xref:SkiaSharp.SKBitmap.Width) und [ `Height` ](xref:SkiaSharp.SKBitmap.Height), der die Pixeldimensionen der Bitmap als auch viele Methoden, einschließlich anzeigen Methoden zum Erstellen von Bitmaps, um sie zu kopieren, und um Pixelbits verfügbar zu machen. 

## <a name="displaying-in-pixel-dimensions"></a>Anzeigen von in die Pixeldimensionen

Die SkiaSharp [ `Canvas` ](xref:SkiaSharp.SKCanvas) Klasse definiert vier `DrawBitmap` Methoden. Diese Methoden ermöglichen die Bitmaps auf zwei grundlegend verschiedene Arten angezeigt werden: 

- Angeben einer `SKPoint` Wert (oder separate `x` und `y` Werte) werden Sie in die Pixeldimensionen die Bitmap angezeigt. Die Pixel der Bitmap werden direkt in Pixel der Videoanzeige zugeordnet.
- Angeben eines Rechtecks bewirkt, dass die Bitmap, auf die Größe und Form des Rechtecks gestreckt wird. 

Anzeigen eine Bitmap in die Pixeldimensionen mit [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint)) mit einer `SKPoint` Parameter oder [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) mit separaten `x` und `y` Parameter:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Diese beiden Methoden sind funktional identisch. Der angegebene Punkt gibt den Speicherort der oberen linken Ecke der Bitmap relativ zur Leinwand an. Da die Pixel-Auflösung von mobilen Geräten so hoch ist, werden kleinere Bitmaps in der Regel ziemlich winzig auf diesen Geräten angezeigt.

Der optionale `SKPaint` Parameter können Sie die Bitmap Transparenz mit anzuzeigen. Zu diesem Zweck erstellen Sie eine `SKPaint` Objekt, und legen die `Color` -Eigenschaft auf eine `SKColor` Wert mit einem Alpha-Kanal kleiner als 1. Zum Beispiel:

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

Der 0 x 80, der als letztes Argument übergeben, gibt die Transparenz von 50 % an. Sie können auch einen alpha-Kanal auf eine der vordefinierten Farben festlegen:

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

Die Farbe an sich ist jedoch nicht relevant. Nur der Alphakanal wird untersucht, bei der Verwendung der `SKPaint` -Objekt in ein `DrawBitmap` aufrufen.

Die `SKPaint` Objekt spielt ebenfalls eine Rolle beim Anzeigen von Bitmaps mit Füllmethoden oder Auswirkungen zu filtern. Diese werden in den Artikeln veranschaulicht [SkiaSharp-Zusammensetzung und Blend-Modi](../effects/blend-modes/index.md) und [SkiaSharp Bildfilter](../effects/image-filters.md).

Die **Pixeldimensionen** auf der Seite die **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** Beispielprogramm zeigt eine Bitmapressource, die 320 Pixel breit und 240 Pixel hoch ist:

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

Die `PaintSurface` Handler Rechenzentren die Bitmap durch das berechnen `x` und `y` Werte basierend auf die Pixeldimensionen der Anzeigeoberfläche und die Pixeldimensionen der Bitmap:

[![Pixeldimensionen](displaying-images/PixelDimensions.png "Pixeldimensionen")](displaying-images/PixelDimensions-Large.png#lightbox)

Wenn möchte, dass die Anwendung in der oberen linken Ecke der Bitmap anzuzeigen, übergeben sie einfach Koordinaten (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Eine Methode zum Laden von Bitmaps für Ressource

Viele der Beispiele an, Bitmapressourcen geladen werden müssen. Die statische `BitmapExtensions` -Klasse in der **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** Lösung enthält eine Methode, die helfen:

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

Beachten Sie, dass die `Type` Parameter. Dies kann sein, die `Type` Objekt beliebigen Typs in der Assembly, die die Bitmapressource speichert zugeordnet.

Dies `LoadBitmapResource` Methode in alle folgenden Samplings, die Bitmapressourcen erfordern, verwendet werden.

## <a name="stretching-to-fill-a-rectangle"></a>Strecken, um ein Rechteck zu füllen.

Die `SKCanvas` -Klasse definiert außerdem eine [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) -Methode, die Bitmap um ein Rechteck und ein anderes rendert [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) -Methode, eine rechteckige Teilmenge der Bitmap auf rendert, eine Rechteck:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

In beiden Fällen-Bitmap wird gestreckt, um das Rechteck mit dem Namen füllen `dest`. Bei der zweiten Methode die `source` Rechteck ermöglicht die Auswahl eine Teilmenge der Bitmap. Die `dest` Rechteck ist relativ zum Ausgabegerät; die `source` Rechteck ist relativ zum der Bitmap.

Die **füllen Rechteck** Seite zeigt die erste dieser beiden Methoden mit der gleichen Bitmap verwendet im vorangehenden Beispiel in einem Rechteck die gleiche Größe des Zeichenbereichs: 

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

Beachten Sie die Verwendung der neuen `BitmapExtensions.LoadBitmapResource` Methode zum Festlegen der `SKBitmap` Feld. Das Zielrechteck aus einer der [ `Rect` ](xref:SkiaSharp.SKImageInfo.Rect) Eigenschaft `SKImageInfo`, beschreibt die Größe der Anzeigeoberfläche:

[![Ausfüllen des Rechtecks](displaying-images/FillRectangle.png "Ausfüllen des Rechtecks")](displaying-images/FillRectangle-Large.png#lightbox)

Dies ist normalerweise _nicht_ erwünscht. Durch anders in horizontaler und vertikaler Richtung gestreckt wird, wird das Bild verzerrt. Wenn Sie eine Bitmap in einen anderen Wert als die Größe in Pixel anzeigen zu können, möchten in der Regel die Bitmap Originalseitenverhältnis beibehalten.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>Dehnen von Beibehaltung des Seitenverhältnisses

Auch bekannt als stretching einer Bitmaps beim unter Beibehaltung des Seitenverhältnisses ist ein Prozess _einheitliche Skalierung_. Dieser Begriff Lösungsansätze algorithmische. Eine mögliche Lösung wird angezeigt, der **einheitliche Skalierung** Seite:

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

Die `PaintSurface` Ereignishandler berechnet eine `scale` Faktor, der die mindestens das Verhältnis zwischen der Anzeigebreite und Höhe der Bitmap-Breite und Höhe entspricht. Die `x` und `y` Werte können dann für zentrieren skalierte Bitmap in der Anzeigebreite und Höhe berechnet werden. Das Zielrechteck verfügt über einen oberen linken Ecke des `x` und `y` und einer unteren rechten Ecke der diese Werte sowie die skalierte Breite und Höhe der Bitmap:

[![Einheitliche Skalierung](displaying-images/UniformScaling.png "einheitliche Skalierung")](displaying-images/UniformScaling-Large.png#lightbox)

Aktivieren Sie das Telefon seitwärts drehen, um die Bitmap gestreckt, um Sie zu diesem Bereich finden Sie unter:

[![Skalieren von Daten im Querformat mit einheitlicher](displaying-images/UniformScaling-Landscape.png "einheitliche Skalierung Querformat")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Der Vorteil der Verwendung dieser `scale` Faktor ist offensichtlich, wenn Sie einen etwas anderen Algorithmus implementieren möchten. Angenommen Sie, Sie möchten die Bitmap-Seitenverhältnis beibehalten, aber auch das Zielrechteck auszufüllen. Die einzige Möglichkeit, dies möglich ist, ist Teil des Bilds zuschneiden, aber Sie können diesen Algorithmus implementieren, durch einfaches ändern `Math.Min` zu `Math.Max` im obigen Code. Hier ist das Ergebnis: 

[![Einheitliche Skalierung Alternative](displaying-images/UniformScaling-Alternative.png "Alternative einheitliche Skalierung")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Die Bitmap-Seitenverhältnis wird beibehalten, jedoch Bereiche, links und rechts neben die Bitmap zugeschnitten sind.

## <a name="a-versatile-bitmap-display-function"></a>Eine vielseitige Bitmap-Anzeige-Funktion

XAML-basierte programmierumgebungen (z. B. UWP- und Xamarin.Forms) haben eine Funktion zum Erweitern oder verkleinern, Bitmaps und gleichzeitig ihre Seitenverhältnisse. Obwohl dieses Feature von SkiaSharp nicht enthalten ist, können Sie es selbst implementieren. Die `BitmapExtensions` Klasse enthalten, die der [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Anwendung zeigt wie. Die Klasse definiert zwei neue `DrawBitmap` Methoden, die die Seitenverhältnis Berechnung ausführen. Diese neuen Methoden sind Erweiterungsmethoden `SKCanvas`.

Die neue `DrawBitmap` Methoden enthalten einen Parameter vom Typ `BitmapStretch`, eine Enumeration, die definiert, der **BitmapExtensions.cs** Datei:

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

Die `None`, `Fill`, `Uniform`, und `UniformToFill` Mitglieder sind identisch mit denen auf der UWP [ `Stretch` ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) Enumeration. Der ähnlich wie Xamarin.Forms [ `Aspect` ](xref:Xamarin.Forms.Aspect) Enumeration definiert Member `Fill`, `AspectFit`, und `AspectFill`.

Die **einheitliche Skalierung** Seite oben zentriert die Bitmap innerhalb des Rechtecks, aber Sie sollten die anderen Optionen, z. B. die Bitmap auf der linken oder rechten Seite des Rechtecks oder oben oder unten positionieren. Dies ist der Zweck der `BitmapAlignment` Enumeration:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Ausrichtungseinstellungen wirken sich nicht bei der Verwendung mit `BitmapStretch.Fill`.

Die erste `DrawBitmap` Erweiterungsfunktion enthält, ein Zielrechteck, aber keine Quellrechtecks. Standardwerte definiert wurden, sodass, wenn die Bitmap zentriert werden soll, Sie nur angeben, müssen eine `BitmapStretch` Member:

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

Der Hauptzweck dieser Methode wird zum Berechnen von eines Skalierungsfaktor, der mit dem Namen `scale` , wird dann auf die Bitmap-Breite und Höhe angewendet, beim Aufrufen der `CalculateDisplayRect` Methode. Dies ist die Methode, die ein Rechteck für die Anzeige der Bitmap, die basierend auf der horizontalen und vertikalen Ausrichtung berechnet:

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

Die `BitmapExtensions` -Klasse enthält eine zusätzliche `DrawBitmap` Methode mit einem Quellrechteck für eine Teilmenge der Bitmap angibt. Diese Methode ist ähnlich wie bei der ersten, mit der Ausnahme, dass der Skalierungsfaktor auf Grundlage berechnet wird den `source` Rechteck, und dann auf die `source` Rechteck im Aufruf von `CalculateDisplayRect`:

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

Die erste dieser beiden neuen `DrawBitmap` Methoden wird veranschaulicht, der **Skalierung Modi** Seite. Die XAML-Datei enthält drei `Picker` Elemente, mit denen Sie auswählen, Mitglied der `BitmapStretch` und `BitmapAlignment` Enumerationen:

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

Die Code-Behind-Datei einfach erklärt die `CanvasView` Wenn ein `Picker` Element geändert wurde. Die `PaintSurface` Handler greift auf die drei `Picker` Ansichten für den Aufruf der `DrawBitmap` Erweiterungsmethode:

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

[![Skalieren von Modi](displaying-images/ScalingModes.png "Modi Skalierung")](displaying-images/ScalingModes-Large.png#lightbox)

Die **Rechteck Teilmenge** Seite enthält praktisch die gleiche XAML-Datei als **Skalierung Modi**, aber die Code-Behind-Datei definiert eine rechteckige Teilmenge der Bitmap, die vom der `SOURCE` Feld: 

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

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

