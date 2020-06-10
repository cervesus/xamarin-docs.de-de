---
Title: "erstellen und Zeichnen von skiasharp-Bitmaps" Beschreibung: "erfahren Sie, wie Sie skiasharp-Bitmaps erstellen und dann auf diese Bitmaps zeichnen, indem Sie eine Canvas erstellen."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 79bd3266-D457-4e50bddf-33450035fa0f Author: davidbritch ms. Author: dabritch ms. Date: 07/17/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>Erstellen und Zeichnen von skiasharp-Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Sie haben gesehen, wie eine Anwendung Bitmaps aus dem Internet, aus Anwendungs Ressourcen und aus der Fotobibliothek des Benutzers laden kann. Es ist auch möglich, neue Bitmaps innerhalb der Anwendung zu erstellen. Der einfachste Ansatz umfasst einen der Konstruktoren von [`SKBitmap`](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)) :

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

Der `width` -Parameter und der- `height` Parameter sind ganze Zahlen und geben die Pixel Dimensionen der Bitmap an. Dieser Konstruktor erstellt eine vollfarbige Bitmap mit vier Bytes pro Pixel: jeweils ein Byte für die rot-, grün-, blau-und Alpha-Komponenten (Deckkraft).

Nachdem Sie eine neue Bitmap erstellt haben, müssen Sie auf der Oberfläche der Bitmap etwas erhalten. Dies geschieht in der Regel auf zwei Arten:

- Zeichnen Sie mithilfe von Standard Zeichnungs Methoden auf der Bitmap `Canvas` .
- Greifen Sie direkt auf die Pixel Bits zu.

Dieser Artikel veranschaulicht den ersten Ansatz:

![Zeichnungs Beispiel](drawing-images/DrawingSample.png "Zeichnungs Beispiel")

Die zweite Vorgehensweise wird im Artikel [**zugreifen auf skiasharp-Bitmap-Pixel**](pixel-bits.md)erläutert.

## <a name="drawing-on-the-bitmap"></a>Zeichnen auf der Bitmap

Das Zeichnen auf der Oberfläche einer Bitmap ist das gleiche wie das Zeichnen in einer Videoanzeige. Um auf einer Videoanzeige zu zeichnen, rufen Sie ein- `SKCanvas` Objekt aus den `PaintSurface` Ereignis Argumenten ab. Um auf eine Bitmap zu zeichnen, erstellen Sie ein- `SKCanvas` Objekt mit dem- [`SKCanvas`](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap)) Konstruktor:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

Wenn Sie mit dem Zeichnen der Bitmap fertig sind, können Sie das `SKCanvas` Objekt verwerfen. Aus diesem Grund wird der `SKCanvas` Konstruktor in der Regel in einer- `using` Anweisung aufgerufen:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

Die Bitmap kann dann angezeigt werden. Zu einem späteren Zeitpunkt kann das Programm ein neues `SKCanvas` -Objekt auf der Grundlage der gleichen Bitmap erstellen und einige weitere Objekte zeichnen.

Die Seite **Hello Bitmap** in der Anwendung **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** schreibt den Text "Hello, Bitmap!" in einer Bitmap und zeigt diese Bitmap dann mehrmals an.

Der Konstruktor von `HelloBitmapPage` beginnt mit dem Erstellen eines- `SKPaint` Objekts zum Anzeigen von Text. Er bestimmt die Dimensionen einer Text Zeichenfolge und erstellt eine Bitmap mit diesen Dimensionen. Anschließend `SKCanvas` wird ein-Objekt erstellt, das auf dieser Bitmap basiert, aufruft `Clear` und dann aufruft `DrawText` . Es ist immer eine gute Idee, `Clear` mit einer neuen Bitmap aufzurufen, da eine neu erstellte Bitmap zufällige Daten enthalten könnte.

Der Konstruktor wird beendet, indem ein-Objekt erstellt wird `SKCanvasView` , um die Bitmap anzuzeigen:

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

Der `PaintSurface` Handler rendert die Bitmap mehrmals in den Zeilen und Spalten der Anzeige. Beachten Sie, dass die- `Clear` Methode im- `PaintSurface` Handler über ein Argument von verfügt `SKColors.Aqua` , das den Hintergrund der Anzeige Oberfläche Farben:

[![Hallo, Bitmap!](drawing-images/HelloBitmap.png "Hallo, Bitmap!")](drawing-images/HelloBitmap-Large.png#lightbox)

Die Darstellung des Aqua-Hintergrunds zeigt, dass die Bitmap außer dem Text transparent ist.

## <a name="clearing-and-transparency"></a>Löschen und Transparenz

Die Anzeige der Seite **Hello Bitmap** zeigt, dass die vom Programm erstellte Bitmap transparent ist, mit Ausnahme des schwarzen Texts. Aus diesem Grund wird die Aqua-Farbe der Anzeige Oberfläche durch angezeigt.

In der Dokumentation der `Clear` Methoden von werden `SKCanvas` diese mit der-Anweisung beschrieben: "ersetzt alle Pixel im aktuellen Clip der Canvas". Die Verwendung des Worts "Replace" zeigt ein wichtiges Merkmal dieser Methoden an: alle Zeichnungs Methoden von fügen der `SKCanvas` vorhandenen Anzeige Oberfläche etwas hinzu. Die `Clear` -Methoden _ersetzen_ , was bereits vorhanden ist.

`Clear`ist in zwei verschiedenen Versionen vorhanden:

- Die- [`Clear`](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor)) Methode mit einem- `SKColor` Parameter ersetzt die Pixel der Anzeige Oberfläche durch Pixel dieser Farbe.

- Die- [`Clear`](xref:SkiaSharp.SKCanvas.Clear) Methode ohne Parameter ersetzt die Pixel durch die [`SKColors.Empty`](xref:SkiaSharp.SKColors.Empty) Farbe, bei der es sich um eine Farbe handelt, in der alle Komponenten (rot, grün, blau und Alpha) auf 0 (null) festgelegt sind. Diese Farbe wird manchmal als "transparent Black" bezeichnet.

Wenn `Clear` Sie ohne Argumente in einer neuen Bitmap aufrufen, wird die gesamte Bitmap so initialisiert, dass Sie vollständig transparent ist. Alles, was anschließend auf der Bitmap gezeichnet wird, ist in der Regel nicht transparent oder teilweise undurchsichtig.

Probieren Sie Folgendes aus: Ersetzen Sie auf der Seite " **Hello Bitmap** " die-Methode, die `Clear` auf das angewendet wird, durch die folgende Methode `bitmapCanvas` :

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

Die Reihenfolge der `SKColor` Konstruktorparameter ist rot, grün, blau und Alpha, wobei jeder Wert zwischen 0 und 255 liegen kann. Beachten Sie, dass ein Alpha-Wert von 0 transparent ist, während der Alpha Wert 255 nicht transparent ist.

Der Wert (255, 0, 0, 128) löscht die Bitmap-Pixel in rote Pixel mit einer Deckkraft von 50%. Dies bedeutet, dass der Bitmaphintergrund semitransparent ist. Der semitransparente rote Hintergrund der Bitmap kombiniert mit dem Aqua-Hintergrund der Anzeige Oberfläche, um einen grauen Hintergrund zu erstellen.

Legen Sie die Farbe des Texts auf Transparent Black fest, indem Sie die folgende Zuweisung in den `SKPaint` Initialisierer einfügen:

```csharp
Color = new SKColor(0, 0, 0, 0)
```

Vielleicht denken Sie, dass durch diesen transparenten Text vollständig transparente Bereiche der Bitmap erstellt werden, über die Sie den Aqua-Hintergrund der Anzeige Oberfläche sehen würden. Das ist jedoch nicht der Fall. Der Text wird auf der Bitmap gezeichnet, die bereits vorhanden ist. Der transparente Text ist überhaupt nicht sichtbar.

Mit keiner `Draw` Methode wird eine Bitmap transparenter. `Clear`Dies ist nur möglich.

## <a name="bitmap-color-types"></a>Bitmap-Farbtypen

Mit dem einfachsten `SKBitmap` Konstruktor können Sie eine ganzzahlige Pixel Breite und-Höhe für die Bitmap angeben. Andere `SKBitmap` Konstruktoren sind komplexer. Diese Konstruktoren erfordern Argumente von zwei Enumerationstypen: [`SKColorType`](xref:SkiaSharp.SKColorType) und [`SKAlphaType`](xref:SkiaSharp.SKAlphaType) . Andere Konstruktoren verwenden die- [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) Struktur, die diese Informationen konsolidiert.

Die- `SKColorType` Enumeration weist 9 Member auf. Jedes dieser Member beschreibt eine bestimmte Methode zum Speichern der bitmappixel:

- `Unknown`
- `Alpha8`&mdash;jedes Pixel ist 8 Bits und stellt einen Alpha-Wert von vollständig transparent bis vollständig transparent dar.
- `Rgb565`&mdash;jedes Pixel ist 16 Bits, 5 Bits für Rot und blau und 6 für grün.
- `Argb4444`&mdash;jedes Pixel ist 16 Bits, 4 für Alpha, rot, grün und blau.
- `Rgba8888`&mdash;jedes Pixel ist 32 Bits, jeweils 8 für Rot, grün, blau und Alpha
- `Bgra8888`&mdash;jedes Pixel ist 32 Bits, jeweils 8 für blau, grün, rot und Alpha
- `Index8`&mdash;jedes Pixel ist 8 Bits und stellt einen Index in einem[`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8`&mdash;jedes Pixel besteht aus 8 Bits, die einen grauen Farbton von Schwarz nach weiß darstellen.
- `RgbaF16`&mdash;jedes Pixel ist 64 Bits (rot, grün, blau und Alpha) in einem 16-Bit-Gleit Komma Format.

Die beiden Formate, in denen jedes Pixel 32 Pixel (4 Bytes) ist, werden häufig als _voll Farb_ Formate bezeichnet. Viele der anderen Formate sind von einem Zeitpunkt aus, an dem Video angezeigt wird, nicht vollständig vollständig. Bitmaps mit eingeschränkter Farbe waren für diese anzeigen ausreichend, und zulässige Bitmaps belegen weniger Speicherplatz im Speicher.

Heutzutage verwenden Programmierer fast immer vollfarbige Bitmaps und stören sich nicht mit anderen Formaten. Bei der Ausnahme handelt es sich um das- `RgbaF16` Format, das eine größere Farbauflösung zulässt, als sogar die voll Farb Formate. Dieses Format wird jedoch für spezielle Zwecke, wie z. b. die medizinische Abbild Erstellung, verwendet und ist nicht sinnvoll, wenn es mit standardmäßigen Vollfarben angezeigt wird.

Diese Artikel Reihe beschränkt sich auf die `SKBitmap` Farb Formate, die standardmäßig verwendet werden, wenn kein `SKColorType` Member angegeben wird. Dieses Standardformat basiert auf der zugrunde liegenden Plattform. Für Plattformen, die von unterstützt Xamarin.Forms werden, lautet der Standard Farbtyp wie folgt:

- `Rgba8888`für IOS und Android
- `Bgra8888`für UWP

Der einzige Unterschied ist die Reihenfolge der 4 Bytes im Arbeitsspeicher, und dies wird nur zu einem Problem, wenn Sie direkt auf die Pixel Bits zugreifen. Dies wird erst dann wichtig, wenn Sie den Artikel [**zugreifen auf skiasharp-Bitmap-Pixel**](pixel-bits.md)erhalten.

Die- `SKAlphaType` Enumeration hat vier Member:

- `Unknown`
- `Opaque`&mdash;die Bitmap hat keine Transparenz.
- `Premul`&mdash;Farbkomponenten werden mit der Alpha Komponente vorab multipliziert.
- `Unpremul`&mdash;Farbkomponenten werden von der Alpha Komponente nicht vorab multipliziert.

Hier ist ein 4-Byte-rote Bitmap-Pixel mit einer Transparenz von 50%, wobei die Bytes in der Reihenfolge rot, grün, blau und Alpha angezeigt werden:

0xFF 0x00 0x00 0x80

Wenn eine Bitmap mit semitransparenten Pixeln auf einer Anzeige Oberfläche gerendert wird, müssen die Farbkomponenten jedes bitmappixels mit dem Alpha Wert dieses Pixels multipliziert werden, und die Farbkomponenten des entsprechenden Pixels der Anzeige Oberfläche müssen mit 255 minus dem Alpha-Wert multipliziert werden. Die beiden Pixel können dann kombiniert werden. Die Bitmap kann schneller gerendert werden, wenn die Farbkomponenten in den bitmappixeln bereits durch den Alpha-Wert vorgezeichnet wurden. Das gleiche rote Pixel würde wie folgt in einem vorab multiplizierten Format gespeichert:

0x80 0x00 0x00 0x80

Diese Verbesserung der Leistung liegt darin begründet, dass `SkiaSharp` Bitmaps standardmäßig mit einem-Format erstellt werden `Premul` . Aber auch hier ist es notwendig, dies nur zu wissen, wenn Sie auf Pixel Bits zugreifen und diese bearbeiten.

## <a name="drawing-on-existing-bitmaps"></a>Zeichnen auf vorhandenen Bitmaps

Es ist nicht erforderlich, eine neue Bitmap zu erstellen, um darauf zu zeichnen. Sie können auch auf einer vorhandenen Bitmap zeichnen.

Die **Monkey Moustache** -Seite verwendet den Konstruktor, um das **monkeyface. png** -Bild zu laden. Anschließend `SKCanvas` wird ein Objekt erstellt, das auf dieser Bitmap basiert, und verwendet die `SKPaint` `SKPath` Objekte und, um eine Moustache darauf zu zeichnen:

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

Der Konstruktor wird beendet, indem ein erstellt wird, `SKCanvasView` dessen `PaintSurface` Handler einfach das Ergebnis anzeigt:

[![Affanz Moustache](drawing-images/MonkeyMoustache.png "Affanz Moustache")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>Kopieren und Ändern von Bitmaps

Die Methoden von `SKCanvas` , die Sie verwenden können, um eine Bitmap zu zeichnen, sind include `DrawBitmap` . Dies bedeutet, dass Sie eine Bitmap auf einem anderen zeichnen und in der Regel auf irgendeine Weise ändern können.

Die vielseitigste Methode zum Ändern einer Bitmap besteht darin, auf die eigentlichen Pixel Bits zuzugreifen. Dies ist ein Betreff, der im Artikel **[zugreifen auf skiasharp Bitmap Pixel](pixel-bits.md)** behandelt wird. Es gibt jedoch viele weitere Verfahren zum Ändern von Bitmaps, die keinen Zugriff auf die Pixel Bits erfordern.

Die folgende Bitmap, die in der **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Anwendung enthalten ist, ist 360 Pixel breit und 480 Pixel in Höhe:

![Bergsteiger](drawing-images/MountainClimbers.jpg "Bergsteiger")

Angenommen, Sie haben nicht die Berechtigung von Links auf der linken Seite erhalten, um das Foto zu veröffentlichen. Eine Lösung besteht darin, das Gesicht des affels mithilfe einer Methode namens " _Pixelization_" zu verbergen. Die Pixel der Vorderseite werden durch Farbblöcke ersetzt, sodass Sie die Funktionen nicht mehr erstellen können. Die Farbblöcke werden normalerweise aus dem ursprünglichen Bild abgeleitet, indem die Farben der Pixel, die diesen Blöcken entsprechen, mittelmäßig durchlaufen werden. Aber Sie müssen diesen Mittelwert nicht selbst durchführen. Dies geschieht automatisch, wenn Sie eine Bitmap in eine kleinere Pixel Dimension kopieren.

Die Vorderseite des linken Affen einnimmt ungefähr einen 72-Pixel-quadratischen Bereich mit einer oberen linken Ecke am Punkt (112, 238). Ersetzen wir die Quadrat Flächen 72-Pixel durch ein 9-mal-9-Array von farbigen Blöcken, von denen jedes ein Quadrat von 8 bis 8 Pixel ist.

Die **Bildseite "pixelize** " wird in dieser Bitmap geladen und erstellt zunächst eine kleine 9-Pixel-Quadratische Bitmap mit dem Namen `faceBitmap` . Dies ist ein Ziel zum Kopieren der Gegenseite des Affens. Das Ziel Rechteck ist nur 9-Pixel-quadratisch, aber das Quell Rechteck ist 72-Pixel quadratisch. Jeder 8-mal-8-Block von Quell Pixeln wird durch Mittelwert der Farben auf nur ein Pixel konsolidiert.

Der nächste Schritt besteht darin, die ursprüngliche Bitmap in eine neue Bitmap mit dem Namen zu kopieren `pixelizedBitmap` . Der kleine `faceBitmap` wird dann über diesen mit einem 72-Pixel-quadratischen Ziel Rechteck kopiert, sodass jedes Pixel von `faceBitmap` auf das 8-fache der Größe erweitert wird:

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

Der Konstruktor wird beendet, indem ein erstellt wird `SKCanvasView` , um das Ergebnis anzuzeigen:

[![Bild "pixelize"](drawing-images/PixelizeImage.png "Bild "pixelize"")](drawing-images/PixelizeImage-Large.png#lightbox)

## <a name="rotating-bitmaps"></a>Drehen von Bitmaps

Eine weitere häufige Aufgabe ist das Drehen von Bitmaps. Dies ist besonders nützlich, wenn Bitmaps aus einer iPhone-oder iPad-Fotobibliothek abgerufen werden. Wenn das Gerät bei der Erstellung des Fotos nicht in einer bestimmten Ausrichtung gehalten wurde, ist das Bild wahrscheinlich in der Mitte oder auf der anderen Seite.

Zum Aktivieren einer Bitmap-aufwärts-Down-Funktion muss eine andere Bitmap mit der gleichen Größe wie die erste erstellt werden. Anschließend wird eine Transformation so festgelegt, dass Sie um 180 Grad gedreht wird, während der erste zum zweiten kopiert wird In allen Beispielen in diesem Abschnitt `bitmap` ist das `SKBitmap` Objekt, das Sie drehen müssen:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Wenn Sie um 90 Grad drehen, müssen Sie eine Bitmap erstellen, bei der es sich um eine andere Größe als die ursprüngliche handelt, indem Sie die Höhe und Breite austauschen. Wenn die ursprüngliche Bitmap z. b. 1200 Pixel breit und 800 Pixel hoch ist, ist die gedrehte Bitmap 800 Pixel breit und 1200 Pixel breit. Legen Sie Übersetzung und Drehung fest, damit die Bitmap um die linke obere Ecke herum gedreht und dann in die Ansicht verschoben wird. (Beachten Sie, dass die `Translate` -Methode und die- `RotateDegrees` Methode in umgekehrter Reihenfolge der Art und Weise aufgerufen werden, in der Sie angewendet werden.) Hier ist der Code für die Rotation von 90 Grad im Uhrzeigersinn:

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

Und hier ist eine ähnliche Funktion, um 90 Grad gegen den Uhrzeigersinn zu drehen:

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

Diese beiden Methoden werden in den **Foto Rätsel** Seiten verwendet, die im Artikel zum [**Zuschneiden von skiasharp-Bitmaps**](cropping.md#cropping-skiasharp-bitmaps)beschrieben werden.

Ein Programm, das es dem Benutzer ermöglicht, eine Bitmap in 90-Grad-Schritten zu drehen, erfordert nur eine Funktion für die Rotation um 90 Grad. Der Benutzer kann dann in jedem Inkrement von 90 Grad drehen, indem er die wiederholte Ausführung dieser eine Funktion wiederholt.

Ein Programm kann eine Bitmap auch nach einem beliebigen Betrag drehen. Ein einfacher Ansatz besteht darin, die Funktion zu ändern, die um 180 Grad gedreht wird, indem 180 durch eine verallgemeinerte Variable ersetzt wird `angle` :

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Im Allgemeinen wird diese Logik jedoch von den Ecken der gedrehten Bitmap entfernt. Ein besserer Ansatz ist die Berechnung der Größe der gedrehten Bitmap mithilfe von trigonometrische, um diese Ecken einzubeziehen.

Dieser trigonometrische wird auf der Seite **Bitmap-Rotator** angezeigt. Die XAML-Datei instanziiert ein `SKCanvasView` -und einen- `Slider` Wert, der zwischen 0 und 360 Grad liegen kann und `Label` den aktuellen Wert anzeigt:

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

Die Code-Behind-Datei lädt eine Bitmap-Ressource und speichert Sie als statisches Schreib geschütztes Feld mit dem Namen `originalBitmap` . Die im-Handler angezeigte Bitmap `PaintSurface` ist `rotatedBitmap` , die anfänglich auf festgelegt ist `originalBitmap` :

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

Der- `ValueChanged` Handler von `Slider` führt die Vorgänge aus, die `rotatedBitmap` auf der Grundlage des Drehwinkels einen neuen erstellen. Die neue Breite und Höhe basiert auf absoluten Werten von sinen und kosinen der ursprünglichen Breite und Höhe. Mit den Transformationen, die zum Zeichnen der ursprünglichen Bitmap auf der gedrehten Bitmap verwendet werden, wird das ursprüngliche Bitmap-Center in den Ursprung verschoben, dann um die angegebene Anzahl von Grad gedreht und dann in den Mittelpunkt der gedrehten Bitmap übersetzt. (Die `Translate` -Methode und die- `RotateDegrees` Methode werden in umgekehrter Reihenfolge aufgerufen, als Sie angewendet werden.)

Beachten Sie die Verwendung der- `Clear` Methode, um den Hintergrund eines `rotatedBitmap` hellen Rosa zu machen. Dies dient nur zur Veranschaulichung der Größe von `rotatedBitmap` auf der Anzeige:

[![Bitmap-Rotator](drawing-images/BitmapRotator.png "Bitmap-Rotator")](drawing-images/BitmapRotator-Large.png#lightbox)

Die gedrehte Bitmap ist nur groß genug, um die gesamte ursprüngliche Bitmap einzuschließen, jedoch nicht größer.

## <a name="flipping-bitmaps"></a>Flipping von Bitmaps

Ein anderer Vorgang, der häufig für Bitmaps ausgeführt wird, wird als _kippen_bezeichnet. Konzeptionell wird die Bitmap in drei Dimensionen um eine vertikale Achse oder eine horizontale Achse durch die Mitte der Bitmap gedreht. Beim vertikalen kippen wird ein Spiegelbild erstellt.

Diese Prozesse werden von der **Bitmap-Flipper** Seite in der **[skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** -Anwendung veranschaulicht. Die XAML-Datei enthält eine `SKCanvasView` und zwei Schaltflächen für das vertikale und horizontale kippen:

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

Die Code-Behind-Datei implementiert diese beiden Vorgänge in den `Clicked` Handlern für die Schaltflächen:

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

Der vertikale Flip wird durch eine Skalierungs Transformation mit einem horizontalen Skalierungsfaktor von &ndash; 1 erreicht. Das Skalierungs Center ist die vertikale Mitte der Bitmap. Der horizontale kippen ist eine Skalierungs Transformation mit einem vertikalen Skalierungsfaktor von &ndash; 1.

Wie Sie aus der umgekehrten Beschriftung im-Shirt des affels sehen können, ist das Kippen nicht identisch mit der Drehung. Doch wie der UWP-Bildschirm Abbildung auf der rechten Seite veranschaulicht, ist das Kippen sowohl horizontal als auch vertikal identisch mit dem Drehen von 180 Grad:

[![Bitmap-Flipper](drawing-images/BitmapFlipper.png "Bitmap-Flipper")](drawing-images/BitmapFlipper-Large.png#lightbox)

Eine weitere häufige Aufgabe, die mit ähnlichen Techniken behandelt werden kann, ist das Zuschneiden einer Bitmap auf eine rechteckige Teilmenge. Dies wird im nächsten Artikel zum [**Zuschneiden von skiasharp-Bitmaps**](cropping.md)beschrieben.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
