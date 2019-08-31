---
title: Erstellen und Zeichnen auf SkiaSharp-bitmaps
description: Erfahren Sie, wie SkiaSharp-Bitmaps erstellen und zeichnen Sie dann auf diese Bitmaps, durch das Erstellen einer Canvas basierend darauf.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 68d6cb1df8557b6055feb81b21ed5513592c71c4
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198147"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>Erstellen und Zeichnen auf SkiaSharp-bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Sie haben gesehen, wie eine Anwendung Bitmaps aus dem Web aus Anwendungsressourcen und aus der Fotobibliothek des Benutzers laden kann. Es ist auch möglich, neue Bitmaps in Ihrer Anwendung zu erstellen. Die einfachste Möglichkeit besteht darin, eines Konstruktors von [ `SKBitmap` ](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)):

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

Die `width` und `height` Parameter sind ganze Zahlen, und geben die Pixeldimensionen der Bitmap. Dieser Konstruktor erstellt eine Farbe Bitmap mit 4 Bytes pro Pixel: jede ein Byte für die Rot, Grün, Blau und Alpha (Deckkraft)-Komponenten. 

Nachdem Sie eine neue Bitmap erstellt haben, müssen Sie etwas auf der Oberfläche der Bitmap zu erhalten. Hierzu in der Regel in zwei Arten:

- Zeichnen Sie auf die Bitmap, die mit standardmäßigen `Canvas` Zeichnungsmethoden.
- Greifen Sie direkt auf die Pixelbits.

In diesem Artikel wird die erste Methode veranschaulicht:

![Zeichnen Beispiel](drawing-images/DrawingSample.png "zeichnen-Beispiel")

Der zweite Ansatz ist in diesem Artikel erläuterten [ **Bitmap-Pixel für den Zugriff auf SkiaSharp**](pixel-bits.md).

## <a name="drawing-on-the-bitmap"></a>Klicken Sie auf die Bitmap zeichnen

Zeichnen auf der Oberfläche einer Bitmap wird auf eine der Videoanzeige identisch. Um auf eine der Videoanzeige zu zeichnen, Sie erhalten eine `SKCanvas` -Objekt aus der `PaintSurface` Ereignisargumente. Um eine Bitmap zu zeichnen, erstellen Sie eine `SKCanvas` -Objekt unter Verwendung der [ `SKCanvas` ](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap)) Konstruktor:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

Wenn Sie auf die Bitmap Zeichnen fertig sind, können gelöscht werden, die `SKCanvas` Objekt. Aus diesem Grund die `SKCanvas` Konstruktor wird in der Regel aufgerufen, einem `using` Anweisung:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

Die Bitmap kann dann angezeigt werden. Zu einem späteren Zeitpunkt, das Programm kann erstellen Sie ein neues `SKCanvas` Objekt auf Grundlage, dass die gleichen bitmap, und Zeichnen auf noch mehr.

Die **Hello Bitmap** auf der Seite die **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** Anwendung schreibt den Text "Hello, Bitmap!" auf eine Bitmap, und klicken Sie dann angezeigt, das mehrere Male bitmap.  

Der Konstruktor der der `HelloBitmapPage` beginnt mit der Erstellung einer `SKPaint` Objekt zum Anzeigen von Text. Bestimmt die Dimensionen einer Textzeichenfolge und erstellt eine Bitmap mit diesen Dimensionen. Er erstellt dann ein `SKCanvas` -Objekt auf Grundlage dieser Bitmap Aufrufe `Clear`, und ruft dann `DrawText`. Es ist immer eine gute Idee, rufen Sie `Clear` mit einer neuen Bitmap, da eine neu erstellte Bitmap zufällige Daten enthalten kann.

Der Konstruktor wird abgeschlossen, indem Sie erstellen eine `SKCanvasView` Objekt, um die Bitmap anzuzeigen:

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result 
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

Die `PaintSurface` Handler rendert die Bitmap mehrere Male in Zeilen und Spalten der Anzeige. Beachten Sie, dass die `Clear` -Methode in der der `PaintSurface` Handler verfügt über ein Argument vom `SKColors.Aqua`, der den Hintergrund der Anzeigeoberfläche Farben:

[![Hello, Bitmap! ](drawing-images/HelloBitmap.png "Hello, Bitmap!")](drawing-images/HelloBitmap-Large.png#lightbox)

Die Darstellung der Aquamarin-Hintergrund zeigt, dass die Bitmap mit Ausnahme des Texts transparent ist.

## <a name="clearing-and-transparency"></a>Das Löschen und Transparenz

Die Anzeige der **Hello Bitmap** Seite veranschaulicht, dass die Bitmap erstellte Programm mit Ausnahme der schwarze Text transparent ist. Deshalb zeigt Sie die Farbe "Aquamarin" der Anzeigeoberfläche durch.

In der Dokumentation `Clear` der Methoden von `SKCanvas` werden diese mit der-Anweisung beschrieben: "Ersetzt alle Pixel im" aktuellen Clip "der Canvas. Durch die Verwendung des Worts "Replace" wird ein wichtiges Merkmal dieser Methoden angezeigt: Alle Zeichnungs Methoden von `SKCanvas` fügen der vorhandenen Anzeige Oberfläche etwas hinzu. Die `Clear` Methoden _ersetzen_ was bereits vorhanden ist.

`Clear` ist in zwei verschiedenen Versionen vorhanden: 

- Die [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor)) -Methode mit einem `SKColor` Parameter ersetzt die Pixel der Anzeigeoberfläche durch Pixel dieser Farbe.

- Die [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear) Methode ohne Parameter ersetzt die Pixel mit den [ `SKColors.Empty` ](xref:SkiaSharp.SKColors.Empty) Farbe, die eine Farbe in dem alle Komponenten (Rot, Grün, Blau und Alpha wird) sind 0 (null) festgelegt. Diese Farbe wird manchmal als "transparentes Schwarz." bezeichnet

Aufrufen von `Clear` ohne Argumente auf eine neue Bitmap initialisiert die gesamte Bitmap völlig transparent sein. Etwas später auf die Bitmap gezeichnet wird in der Regel nicht transparente oder teilweise nicht transparent sein.

Hier ist etwas zu versuchen: Ersetzen Sie auf der Seite **Hello Bitmap** die `Clear` `bitmapCanvas` Methode, die auf angewendet wird, durch diese Methode:

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

Die Reihenfolge der `SKColor` Konstruktorparameter ist rot, Grün, Blau und alpha, wobei jeder Wert zwischen 0 und 255 liegen kann. Denken Sie daran, dass der Alphawert 0 transparent, während ein Alphawert von 255 ist, ist nicht transparent.

Der Wert (255, 0, 0, 128) Löscht die Pixel auf Rot Pixel mit einer Deckkraft von 50 %. Dies bedeutet, dass die Bitmaphintergrund teilweise transparent ist. Die teilweise transparente rote Hintergrund der Bitmap kombiniert mit dem Aquamarin-Hintergrund der Anzeigeoberfläche grauen Hintergrund zu erstellen.

Wiederholen Sie die Farbe des Texts auf transparentes Schwarz festlegen, indem Sie Sie in der folgenden Zuweisung Einfügen der `SKPaint` Initialisierer:

```csharp
Color = new SKColor(0, 0, 0, 0)
```

Sie denken möglicherweise, dass es sich bei diesem transparente Text vollständig transparente Bereiche der Bitmap erstellen würden, die über die Sie den Aquamarin-Hintergrund der Anzeigeoberfläche sehen würden. Aber nicht der Fall. Der Text gezeichnet wird, auf was bereits in der Bitmap ist. Der transparente Text werden überhaupt nicht angezeigt.

Keine `Draw` Methode jemals ist eine Bitmap transparenter. Nur `Clear` möglich.

## <a name="bitmap-color-types"></a>Bitmap-Farbtypen

Die einfachste `SKBitmap` Konstruktor können Sie eine Ganzzahl Pixelbreite und Höhe der Bitmap an. Andere `SKBitmap` Konstruktoren sind komplexer. Diese Konstruktoren erfordern Argumente von beiden Enumerationstypen: [ `SKColorType` ](xref:SkiaSharp.SKColorType) und [ `SKAlphaType` ](xref:SkiaSharp.SKAlphaType). Verwenden Sie die anderen Konstruktoren der [ `SKImageInfo` ](xref:SkiaSharp.SKImageInfo) -Struktur, die diese Informationen konsolidiert.

Die `SKColorType` Enumeration verfügt über 9 Elemente. Jeder dieser Member beschreibt eine bestimmte Weise speichern die Pixel:

- `Unknown`
- `Alpha8` &mdash; jedes Pixel ist, 8 Bits, die einen Alphawert von vollkommen transparent bis vollkommen durchlässig darstellt
- `Rgb565` &mdash; jedes Pixel beträgt 16 Bit und 5 Bits für rote und blaue und 6 für Grün
- `Argb4444` &mdash; jedes Pixel beträgt 16 Bit und 4 für den Alpha-, Rot-, Grün- und Blau
- `Rgba8888` &mdash; jedes Pixel ist 32-Bit, 8 für Rot, Grün, Blau und alpha
- `Bgra8888` &mdash; jedes Pixel ist 32-Bit, 8 für Blau, Grün, Rot und alpha
- `Index8` &mdash; jedes Pixel beträgt 8 Bit, und stellt einen Index in ein [`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8` &mdash; jedes Pixel ist, 8 Bits, die eine grauschattierung von Schwarz zu Weiß darstellt
- `RgbaF16` &mdash; jedes Pixel ist 64 Bit, mit Rot, Grün, Blau und Alpha in einem 16-Bit-Gleitkommazahl-format

Formate, in dem jedes Pixel x 32 Pixel besitzen (4 Bytes) ist, werden oft als _farbig_ Formate. Viele andere Formate Datum von einem Zeitpunkt, wenn sich ein Bild anzeigt, waren nicht der vollständige Farbe kann. Bitmaps beschränkt Farbe für diese anzeigen wurden und Bitmaps belegen weniger Speicherplatz im Arbeitsspeicher zulässig. 

Heutzutage Programmierer fast immer Farbe Bitmaps verwenden und benötigen Sie keine anderen Formaten. Die Ausnahme ist die `RgbaF16` -Format, das größer farbauflösung als auch die Farbe Formate ermöglicht. Allerdings wird dieses Format wird für spezielle Zwecke, z. B. medizinische Bilddaten verwendet und nicht viel Sinn bei Verwendung mit standard farbig angezeigt.

Diese Artikelreihe verhindert, dass sich selbst, um die `SKBitmap` Farbe, die nicht standardmäßig verwendeten Formate `SKColorType` Element angegeben ist. Diese Standard-Format basiert auf der zugrunde liegenden Plattform. Für die Plattformen, die von Xamarin.Forms unterstützt wird ist der Standardtyp für die Farbe:

- `Rgba8888` für iOS und Android
- `Bgra8888` für die UWP

Der einzige Unterschied ist die Reihenfolge der 4 Bytes im Arbeitsspeicher, und dies nur ein Problem wird, wenn Sie direkt auf die Pixelbits zugreifen. Dies wird nicht wichtig werden, bis Sie auf den Artikel [ **Bitmap-Pixel für den Zugriff auf SkiaSharp**](pixel-bits.md).

Die `SKAlphaType` Enumeration verfügt über vier Mitglieder:

- `Unknown`
- `Opaque` &mdash; die Bitmap wurde keine Transparenz
- `Premul` &mdash; Farbkomponenten werden mit der alpha-Komponente voraus multipliziert.
- `Unpremul` &mdash; Farbkomponenten sind nicht mit der alpha-Komponente vorab multipliziert

Hier ist eine 4-Byte-Rot-Bitmap-Pixel mit Transparenz von 50 %, mit der Bytes, die in der Reihenfolge Rot, Grün, Blau, Alpha dargestellt:

0xFF 0x00 0x00 0x80

Wenn eine Bitmap mit halbtransparenten Pixel auf einer Anzeigeoberfläche gerendert wird, müssen die Komponenten der Farbe der einzelnen Pixel der Bitmap des Pixels Alphawert multipliziert, und die Komponenten der Farbe des entsprechenden Pixels der Anzeigeoberfläche multipliziert werden müssen durch 255 minus der alpha-Wert. Die beiden Pixel können dann kombiniert werden. Die Bitmap kann schneller gerendert werden, wenn die Color-Komponenten in die Pixel bereits vorab-Mulitplied wurden von der alpha-Wert. Dieses gleiche roten Pixel würde wie folgt in ein vorab multipliziertes Format gespeichert werden:

0x80 0x00 0x00 0x80

Diese leistungsverbesserung wird deshalb `SkiaSharp` Bitmaps standardmäßig erstellt werden, mit einem `Premul` Format. In diesem Fall müssen Sie dies wissen nur, wenn Sie Zugriff auf und Bearbeiten von Pixelbits wird aber.

## <a name="drawing-on-existing-bitmaps"></a>Zeichnen in vorhandenen bitmaps

Es ist nicht erforderlich, erstellen eine neue Bitmap darauf zeichnen. Sie können auch auf einer vorhandenen Bitmap zeichnen. 

Die **Monkey Schnurrbart** Seite verwendet ihren Konstruktor zum Laden der **MonkeyFace.png** Image. Er erstellt dann ein `SKCanvas` -Objekt auf Grundlage dieser Bitmap, und verwendet `SKPaint` und `SKPath` Objekte eine Schnurrbart darauf Zeichnen:

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result 
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
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Der Konstruktor wird abgeschlossen, indem Sie erstellen eine `SKCanvasView` , deren `PaintSurface` Handler einfach das Ergebnis wird angezeigt:

[![Monkey Schnurrbart](drawing-images/MonkeyMoustache.png "Monkey Schnurrbart")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>Kopieren und Ändern von bitmaps

Die Methoden der `SKCanvas` , Sie verwenden können, zeichnet eine Bitmap enthalten `DrawBitmap`. Dies bedeutet, dass Sie eine Bitmap auf einem anderen, ändern sie in der Regel in irgendeiner Form zeichnen können.

Der vielseitigste Möglichkeit, eine Bitmap zu ändern ist bis zum Zugriff auf den eigentlichen Pixelbits, ein Thema in diesem Artikel behandelten  **[Pixel für den Zugriff auf SkiaSharp Bitmap](pixel-bits.md)** . Aber es gibt viele Techniken, Bitmaps zu ändern, die Zugriff auf die Pixelbits nicht erforderlich.

Die folgenden Bitmap enthalten, mit der **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Anwendung ist 360 Pixel breit und 480 Pixel hoch:

![Bergsteiger](drawing-images/MountainClimbers.jpg "Bergsteiger")

Angenommen, Sie nicht über das Monkey-Objekt auf der linken Seite dieses Foto veröffentlichen Berechtigung erhalten haben. Eine Lösung besteht darin, das Monkey-Objekt Fläche, die mit einer Technik namens verdecken _Pixelization_. Die Pixel der Oberfläche werden Blöcke von Farbe ersetzt, damit Sie die Features vornehmen können. Die Farbblöcke werden in der Regel vom ursprünglichen Abbild abgeleitet, indem Sie die Farben der Pixel für diese Blöcke Mitteln. Sie müssen jedoch nicht, führen Sie diese mittelwertbildung selbst. Dies erfolgt automatisch, wenn Sie eine Bitmap in eine kleinere Pixeldimensionen kopieren. 

Die linke Monkey Gesicht belegt ungefähr eine quadratische 72 Pixel-Fläche, mit einer oberen linken Ecke, an dem Punkt ("112", "238"). Ersetzen Sie diese 72 Pixel quadratischen Fläche lassen Sie uns mit einem 9 x 9-Array der farbigen, von denen jedes Pixel im Quadrat 8-von-8 ist.

Die **Pixelize Image** Seite wird in diese Bitmap geladen und erstellt zunächst eine kleine wird aufgerufen, quadratische 9-Pixel-Bitmap `faceBitmap`. Dies ist ein Ziel für das Kopieren nur das Monkey-Objekt des Gesichts. Das Zielrechteck ist nur 9-Pixel im Quadrat, aber das Quellrechteck ist 72-Pixel im Quadrat. Jeder Block 8 von 8 Quelle Pixel auf nur ein Pixel durch mittelwertbildung der Farben konsolidiert.

Der nächste Schritt besteht, zum Kopieren der ursprünglichen Bitmap in eine neue Bitmap mit derselben Größe namens `pixelizedBitmap`. Die kleinen `faceBitmap` wird dann mit einem quadratischen 72 Pixel Zielrechteck, kopiert, damit jedes Pixel des `faceBitmap` 8 Mal die Größe erweitert wird:

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap, 
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Der Konstruktor wird abgeschlossen, indem Sie erstellen eine `SKCanvasView` um das Ergebnis anzuzeigen:

[![Image pixelize](drawing-images/PixelizeImage.png "Pixelize Image")](drawing-images/PixelizeImage-Large.png#lightbox)

<a name="rotating-bitmaps" />

## <a name="rotating-bitmaps"></a>Drehen von bitmaps

Eine weitere häufige Aufgabe ist Bitmaps drehen. Dies ist besonders nützlich, wenn Bitmaps aus einem iPhone oder iPad-fotomediathek abrufen. Wenn das Gerät in einer bestimmten Ausrichtung gespeichert wurde, wenn das Foto aufgenommen wurde, handelt es sich wahrscheinlich Kopf oder zur Seite.

Aktivieren einer Bitmaps nach unten zeigende erfordert, erstellen einen anderen Bitmap die gleiche Größe wie die erste festlegen und dann eine Transformation beim Kopieren der ersten zum zweiten um 180 Grad zu drehen. In allen Beispielen in diesem Abschnitt `bitmap` ist die `SKBitmap` -Objekt, das Sie drehen möchten:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Wenn Sie um 90 Grad drehen, müssen Sie eine Bitmap zu erstellen, die eine andere Größe hat als die ursprüngliche durch Austauschen der die Höhe und Breite. Wenn die ursprüngliche Bitmap 1200 Pixel breit und 800 Pixel hoch ist, ist die gedrehte Bitmap z. B. 800 Pixel breit und 1200 Pixel breit. Legen Sie Translation und Rotation, sodass die Bitmap um seine linke obere Ecke gedreht und klicken Sie dann in die Ansicht verschoben wird. (Beachten Sie, dass die `Translate` und `RotateDegrees` Methoden werden aufgerufen, in umgekehrter Reihenfolge der Möglichkeit, die sie angewendet werden.) Hier ist der Code für um 90 Grad im Uhrzeigersinn drehen:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```                        

Und hier ist eine ähnliche Funktion für um 90 Grad gegen den Uhrzeigersinn drehen:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Diese beiden Methoden werden verwendet, der **Foto Rätsel** Seiten, die in diesem Artikel beschriebenen [ **Zuschneiden SkiaSharp Bitmaps**](cropping.md#tile-division).

Ein Programm, das dem Benutzer ermöglicht, eine Bitmap in 90-Grad-Schritten zu drehen, muss nur eine Funktion für um 90 Grad drehen implementieren. Der Benutzer kann dann durch die wiederholte Ausführung dieses einer Funktion in jedem Inkrement von 90 Grad drehen.

Ein Programm kann auch eine Bitmap von beliebigem Umfang drehen. Ein einfacher Ansatz ist, so ändern Sie die Funktion, die um 180 Grad gedreht werden soll, indem 180 durch einen generalisierten ersetzen `angle` Variable:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Allerdings wird im Allgemeinen, diese Logik aus der Ecken der Bitmap für die gedrehte zuschneiden. Ein besserer Ansatz ist die Größe der gedrehte Bitmap mit trigonometrische für die Ecken berechnet. 

Diese trigonometrische wird angezeigt, der **Bitmap Rotator** Seite. Die XAML-Datei instanziiert ein `SKCanvasView` und eine `Slider` , der Bereich von 0 bis 360 Grad mit einem `Label` mit dem aktuellen Wert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei lädt eine Bitmapressource und speichert es als mit dem Namen eines statischen schreibgeschützten Felds `originalBitmap`. Die Bitmap angezeigt, der `PaintSurface` Handler `rotatedBitmap`, die Anfangs auf festgelegt wird `originalBitmap`:

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap = 
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

Die `ValueChanged` Handler, der die `Slider` führt die Vorgänge, die ein neues erstellen `rotatedBitmap` basierend auf den Drehwinkel für Bezeichnungen. Die neue Breite und Höhe basiert auf Absolute values des Sinus definieren und Kosinus definieren, der die ursprüngliche Breite und Höhe. Die Transformationen verwendet, um die gedrehte Bitmap die ursprüngliche Bitmap zeichnen die ursprünglichen Bitmap Mitte verschieben, an den Ursprungsserver, dann durch die angegebene Anzahl von Grad drehen, und klicken Sie dann zu übersetzen, die von Center in die Mitte der gedrehten Bitmap. (Die `Translate` und `RotateDegrees` Methoden werden in umgekehrter Reihenfolge aufgerufen, wie sie angewendet werden.)

Beachten Sie die Verwendung der `Clear` Methode, um den Hintergrund, `rotatedBitmap` eine Hellrosa. Dies ist ausschließlich zur Veranschaulichung der Größe der `rotatedBitmap` auf die Anzeige:

[![Bitmap-Rotator](drawing-images/BitmapRotator.png "Bitmap-Rotator")](drawing-images/BitmapRotator-Large.png#lightbox)

Die gedrehte Bitmap ist gerade groß genug ist, um die gesamte ursprüngliche Bitmap, aber nicht größer sind.

## <a name="flipping-bitmaps"></a>Kippen von bitmaps

Ein anderer Vorgang, die häufig für Bitmaps ausgeführt heißt _kippen_. Im Prinzip wird die Bitmap in drei Dimensionen auf einer vertikale Achse oder der horizontalen Achse durch den Mittelpunkt der Bitmap gedreht. Vertikales Kippen erstellt ein Spiegelbild.

Die **Bitmap Flipper** auf der Seite die **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Anwendung veranschaulicht diese Prozesse. Die XAML-Datei enthält eine `SKCanvasView` und zwei Schaltflächen für ein vertikales und horizontales:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei implementiert diese beiden Vorgänge in der `Clicked` Ereignishandler für die Schaltflächen: 

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

Die vertikale Kippen erfolgt durch eine Skalierung Transformation mit einem Faktor für die horizontale Skalierung von &ndash;1. Den Mittelpunkt der Skalierung ist vertikalen Mitte der Bitmap. Die horizontale kippen ist eine Skalierung Transformation mit einem vertikalen Skalierungsfaktor von &ndash;1. 

Wie Sie in der umgekehrten Behandlung von das Monkey-Objekt "Shirt" sehen, kippen entspricht nicht der Drehung. Wie der UWP-Screenshot auf der rechten Seite veranschaulicht wird, sowohl kippen, horizontal und vertikal ist jedoch die gleiche als 180 Grad drehen:

[![Bitmap-Flipper](drawing-images/BitmapFlipper.png "Bitmap Flipper")](drawing-images/BitmapFlipper-Large.png#lightbox)

Eine weitere häufige Aufgabe, die unter Verwendung ähnlicher Techniken behandelt werden können, ist eine Bitmap eine rechteckige Teilmenge zuschneiden. Dies wird im nächsten Artikel beschrieben [ **Zuschneiden SkiaSharp Bitmaps**](cropping.md).

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
