---
Title: "zugreifen auf skiasharp Bitmap Pixel Bits" Description: "Entdecken Sie die verschiedenen Techniken für den Zugriff auf und die Änderung der Pixel Bits von skiasharp-Bitmaps."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6 Author: davidbritch ms. Author: dabritch ms. Date: 07/11/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>Zugreifen auf skiasharp-Bitmap-Pixel Bits

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Wie Sie im Artikel [**Speichern von skiasharp-Bitmaps in Dateien**](saving.md)gesehen haben, werden Bitmaps im Allgemeinen in Dateien in komprimiertem Format gespeichert, z. b. JPEG oder PNG. In "einschränst" wird eine skiasharp-Bitmap, die im Arbeitsspeicher gespeichert ist, nicht komprimiert. Sie wird als sequenzielle Reihe von Pixeln gespeichert. Dieses unkomprimierte Format ermöglicht die Übertragung von Bitmaps auf eine Anzeige Oberfläche.

Der Speicherblock, der von einer skiasharp-Bitmap belegt wird, ist auf einfache Weise organisiert: er beginnt mit der ersten Zeile der Pixel von links nach rechts und wird dann mit der zweiten Zeile fortgesetzt. Für vollfarbige Bitmaps besteht jedes Pixel aus vier Bytes. Dies bedeutet, dass der gesamte für die Bitmap erforderliche Speicherplatz viermal dem Produkt seiner Breite und Höhe entspricht.

In diesem Artikel wird beschrieben, wie eine Anwendung auf diese Pixel zugreifen kann, entweder direkt durch den Zugriff auf den Pixel Speicherblock der Bitmap oder indirekt. In einigen Fällen kann es vorkommen, dass ein Programm die Pixel eines Bilds analysieren und ein Histogramm einer Sortierung erstellen kann. Im Allgemeinen können Anwendungen eindeutige Bilder erstellen, indem Sie die Pixel, aus denen die Bitmap besteht, algorithmisch erstellen:

![Pixel Bits-Beispiele](pixel-bits-images/PixelBitsSample.png "Pixel Bits-Beispiel")

## <a name="the-techniques"></a>Die Techniken

Skiasharp bietet verschiedene Techniken für den Zugriff auf die Pixel Bits einer Bitmap. Welche Option Sie auswählen, ist in der Regel ein Kompromiss zwischen der Programmier Freundlichkeit (im Zusammenhang mit der Wartung und dem einfachen Debuggen) und der Leistung. In den meisten Fällen verwenden Sie eine der folgenden Methoden und Eigenschaften von für den `SKBitmap` Zugriff auf die Pixel der Bitmap:

- Die `GetPixel` -Methode und die- `SetPixel` Methode ermöglichen es Ihnen, die Farbe eines einzelnen Pixels abzurufen oder festzulegen.
- Die- `Pixels` Eigenschaft ruft ein Array von Pixel Farben für die gesamte Bitmap ab oder legt das Array von Farben fest.
- `GetPixels`Gibt die Adresse des von der Bitmap verwendeten Pixel Speichers zurück.
- `SetPixels`ersetzt die Adresse des von der Bitmap verwendeten Pixel Arbeitsspeichers.

Sie können sich die ersten beiden Techniken als "hohe Ebene" und die zweiten beiden als "niedrige Ebene" vorstellen. Es gibt noch einige andere Methoden und Eigenschaften, die Sie verwenden können. diese sind jedoch besonders wertvoll.

Damit Sie die Leistungsunterschiede zwischen diesen Techniken anzeigen können, enthält die [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Anwendung eine Seite namens **Gradient Bitmap** , die eine Bitmap mit Pixeln erstellt, die rote und blaue Schattierungen zum Erstellen eines Farbverlaufs kombinieren. Das Programm erstellt acht verschiedene Kopien dieser Bitmap, wobei alle unterschiedlichen Techniken zum Festlegen der bitmappixel verwendet werden. Jede dieser acht Bitmaps wird in einer separaten Methode erstellt, die auch eine kurze Textbeschreibung der Technik festlegt und die Zeit berechnet, die benötigt wird, um alle Pixel festzulegen. Jede Methode durchläuft die Pixel Einstellungs Logik 100-Mal, um eine bessere Schätzung der Leistung zu erzielen.

### <a name="the-setpixel-method"></a>Die SetPixel-Methode

Wenn Sie nur mehrere einzelne Pixel festlegen oder erhalten müssen, sind die [`SetPixel`](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor)) -Methode und die- [`GetPixel`](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32)) Methode ideal. Für jede dieser beiden Methoden geben Sie die ganzzahlige Spalte und Zeile an. Unabhängig vom Pixel Format können Sie mit diesen beiden Methoden das Pixel als Wert abrufen oder festlegen `SKColor` :

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

Das `col` -Argument muss zwischen 0 und eins kleiner als die `Width` -Eigenschaft der Bitmap liegen und `row` zwischen 0 und 1 liegen `Height` .

Hier ist die Methode in der **Farbverlaufs Bitmap** , die den Inhalt einer Bitmap mithilfe der- `SetPixel` Methode festlegt. Die Bitmap ist 256 x 256 Pixel, und die `for` Schleifen sind mit dem Wertebereich hart codiert:

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;

    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

Der Farbsatz für jedes Pixel weist eine rote Komponente auf, die gleich der bitmapspalte ist, und eine blaue Komponente gleich der Zeile. Die resultierende Bitmap ist in der oberen linken Ecke rot, rot oben rechts, blau links unten und Magenta unten rechts, mit Farbverläufen an anderer Stelle.

Die `SetPixel` -Methode wird 65.536-Mal aufgerufen, und unabhängig davon, wie effizient diese Methode sein könnte, ist es im Allgemeinen nicht ratsam, diese Zahl von API-aufrufen vorzunehmen, wenn eine Alternative verfügbar ist. Glücklicherweise gibt es mehrere Alternativen.

### <a name="the-pixels-property"></a>Die Pixel-Eigenschaft

`SKBitmap`definiert eine [`Pixels`](xref:SkiaSharp.SKBitmap.Pixels) Eigenschaft, die ein Array von `SKColor` Werten für die gesamte Bitmap zurückgibt. Sie können auch verwenden `Pixels` , um ein Array von Farbwerten für die Bitmap festzulegen:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Die Pixel werden im Array, beginnend mit der ersten Zeile, von links nach rechts, dann von der zweiten Zeile usw., angeordnet. Die Gesamtzahl der Farben im Array ist gleich dem Produkt der bitmapbreite und-Höhe.

Obwohl diese Eigenschaft scheinbar effizient ist, sollten Sie Bedenken, dass die Pixel aus der Bitmap in das Array kopiert werden, von dem Array zurück in die Bitmap und die Pixel aus-und in-Werte konvertiert werden `SKColor` .

Hier ist die-Methode in der- `GradientBitmapPage` Klasse, die die Bitmap mithilfe der- `Pixels` Eigenschaft festlegt. Die-Methode ordnet ein `SKColor` Array der erforderlichen Größe zu, kann jedoch die-Eigenschaft verwenden, `Pixels` um dieses Array zu erstellen:

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Beachten Sie, dass der Index des `pixels` -Arrays aus den Variablen und berechnet werden muss `row` `col` . Die Zeile wird mit der Anzahl der Pixel in jeder Zeile multipliziert (in diesem Fall 256), und anschließend wird die Spalte hinzugefügt.

`SKBitmap`definiert auch eine ähnliche `Bytes` Eigenschaft, die ein Bytearray für die gesamte Bitmap zurückgibt, ist aber für vollfarbliche Bitmaps schwieriger.

### <a name="the-getpixels-pointer"></a>Der getPixels-Zeiger

Möglicherweise ist die leistungsfähigste Methode für den Zugriff auf die Bitmap-Pixel [`GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels) nicht mit der- `GetPixel` Methode oder der-Eigenschaft verwechselt werden `Pixels` . Sie werden sofort feststellen, dass ein Unterschied zu `GetPixels` in der c#-Programmierung nicht sehr häufig angezeigt wird:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

Der .net- [`IntPtr`](xref:System.IntPtr) Typ stellt einen Zeiger dar. Sie wird aufgerufen `IntPtr` , weil es sich um die Länge einer Ganzzahl für den nativen Prozessor des Computers handelt, auf dem das Programm ausgeführt wird, in der Regel entweder 32 Bits oder 64 Bits. Der `IntPtr` , der `GetPixels` zurückgibt, ist die Adresse des tatsächlichen Speicherblocks, den das Bitmap-Objekt verwendet, um die Pixel zu speichern.

Sie können den `IntPtr` in einen c#-Zeigertyp konvertieren, indem Sie die- [`ToPointer`](xref:System.IntPtr.ToPointer) Methode verwenden. Die c#-Zeiger Syntax ist identisch mit C und C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

Die `ptr` Variable ist vom Typ " _Byte Zeiger_". Mit dieser `ptr` Variablen können Sie auf die einzelnen Bytes des Arbeitsspeichers zugreifen, die zum Speichern der Bitmap-Pixel verwendet werden. Verwenden Sie Code wie diesen, um ein Byte aus diesem Arbeitsspeicher zu lesen oder ein Byte in den Arbeitsspeicher zu schreiben:

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

In diesem Kontext ist das Sternchen der c#- _indirection operator_ Dereferenzierungsoperator und wird verwendet, um auf den Inhalt des Speichers zu verweisen, auf den von gezeigt wird `ptr` . Zuerst `ptr` zeigt auf das erste Byte des ersten Pixels der ersten Zeile der Bitmap, aber Sie können Arithmetik für die Variable ausführen, `ptr` um Sie an andere Positionen innerhalb der Bitmap zu verschieben.

Ein Nachteil ist, dass Sie diese `ptr` Variable nur in einem Codeblock verwenden können, der mit dem- `unsafe` Schlüsselwort gekennzeichnet ist. Außerdem muss die Assembly als zulassen unsicherer Blöcke gekennzeichnet werden. Dies erfolgt in den Eigenschaften des Projekts.

Die Verwendung von Zeigern in c# ist sehr leistungsstark, aber auch äußerst gefährlich. Sie müssen darauf achten, dass Sie nicht auf den Arbeitsspeicher zugreifen, der über den Verweis auf den Zeiger hinausgeht. Dies ist der Grund, warum die Zeiger Verwendung dem Wort "unsicher" zugeordnet ist.

Hier ist die-Methode in der- `GradientBitmapPage` Klasse, die die- `GetPixels` Methode verwendet. Beachten `unsafe` Sie den Block, der den gesamten Code mit dem Byte Zeiger umfasst:

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Wenn die `ptr` Variable erstmalig aus der-Methode abgerufen wird `ToPointer` , verweist Sie auf das erste Byte des äußersten linken Pixels der ersten Zeile der Bitmap. Die `for` Schleifen für `row` und `col` werden so eingerichtet, dass `ptr` mit dem-Operator inkrementiert werden kann, `++` nachdem jedes Byte jedes Pixels festgelegt wurde. Bei den anderen 99-Schleifen durch die Pixel `ptr` muss auf den Anfang der Bitmap zurückgesetzt werden.

Jedes Pixel ist vier Bytes Arbeitsspeicher, sodass jedes Byte separat festgelegt werden muss. Im Code wird davon ausgegangen, dass sich die Bytes in der Reihenfolge rot, grün, blau und Alpha befinden, die mit dem `SKColorType.Rgba8888` Farbtyp konsistent ist. Möglicherweise erinnern Sie sich, dass dies der Standard Farbtyp für IOS und Android ist, aber nicht für die universelle Windows-Plattform. Standardmäßig erstellt die UWP Bitmaps mit dem `SKColorType.Bgra8888` Farbtyp. Aus diesem Grund sollten einige andere Ergebnisse auf dieser Plattform angezeigt werden.

Es ist möglich, den von zurückgegebenen Wert `ToPointer` in einen- `uint` Zeiger und nicht in einen- `byte` Zeiger umzuwandeln. Dadurch kann in einer Anweisung auf ein gesamtes Pixel zugegriffen werden. `++`Wenn Sie den-Operator auf diesen Zeiger anwenden, wird dieser um vier Bytes erhöht, um auf das nächste Pixel zu zeigen:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Das Pixel wird mithilfe der- `MakePixel` Methode festgelegt, die ein ganzzahliges Pixel aus den Komponenten rot, grün, blau und Alpha erstellt. Beachten Sie, dass das `SKColorType.Rgba8888` Format eine Pixel Byte-Reihenfolge aufweist, die wie folgt aussieht:

RR GG BB AA

Die ganze Zahl, die den Bytes entspricht, ist:

Aabbggrr

Das am wenigsten signifikante Byte der Ganzzahl wird zuerst in Übereinstimmung mit der Little-Endian-Architektur gespeichert. Diese `MakePixel` Methode funktioniert für Bitmaps mit dem Farbtyp nicht ordnungsgemäß `Bgra8888` .

Die- `MakePixel` Methode wird mit der-Option gekennzeichnet, um zu [`MethodImplOptions.AggressiveInlining`](xref:System.Runtime.CompilerServices.MethodImplOptions) verhindern, dass dies eine separate Methode ist, sondern stattdessen den Code zu kompilieren, in dem die-Methode aufgerufen wird. Dadurch wird die Leistung verbessert.

Interessanterweise definiert die- `SKColor` Struktur eine explizite Konvertierung von `SKColor` in eine ganze Zahl ohne Vorzeichen, was bedeutet, dass ein `SKColor` -Wert erstellt und eine Konvertierung in `uint` anstelle von verwendet werden kann `MakePixel` :

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Die einzige Frage ist: ist das ganzzahlige Format des `SKColor` Werts in der Reihenfolge des `SKColorType.Rgba8888` Farbtyps, oder der `SKColorType.Bgra8888` Farbtyp, oder ist es ganz anders? Die Antwort auf diese Frage sollte in Kürze offengelegt werden.

### <a name="the-setpixels-method"></a>Die setPixels-Methode

`SKBitmap`definiert außerdem eine Methode [`SetPixels`](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr)) mit dem Namen, die wie folgt aufgerufen wird:

```csharp
bitmap.SetPixels(intPtr);
```

Rückruf, `GetPixels` der einen `IntPtr` Verweis auf den Speicherblock erhält, der von der Bitmap zum Speichern der Pixel verwendet wird. Der-Befehl `SetPixels` _ersetzt_ diesen Speicherblock durch den Speicherblock, auf den vom angegebenen verwiesen wird, `IntPtr` als das- `SetPixels` Argument. Die Bitmap gibt dann den Speicherblock frei, der zuvor verwendet wurde. Wenn Sie das nächste Mal aufrufen `GetPixels` , wird der mit festgelegte Speicherblock abgerufen `SetPixels` .

Zunächst scheint es so, als wären `SetPixels` Sie nicht mehr Leistung und Leistung als `GetPixels` weniger praktisch. Mit `GetPixels` erhalten Sie den Bitmap-Speicherblock, und greifen Sie darauf zu. Mit ordnen `SetPixels` Sie Arbeitsspeicher zu, und greifen Sie darauf zu, und legen Sie diesen dann als Bitmap-Speicherblock fest.

Die Verwendung `SetPixels` von bietet jedoch einen unterschiedlichen syntaktischen Vorteil: Sie ermöglicht den Zugriff auf die Bitmap-Pixel Bits mithilfe eines Arrays. Hier ist die Methode in `GradientBitmapPage` , die diese Technik veranschaulicht. Die-Methode definiert zuerst ein mehrdimensionales Bytearray, das den Bytes der Bitmap-Pixel entspricht. Die erste Dimension ist die Zeile, die zweite Dimension ist die Spalte, und die dritte Dimension ist mit den vier Komponenten der einzelnen Pixel verknüpft:

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }

    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Nachdem das Array mit Pixeln aufgefüllt wurde, `unsafe` wird ein-Block und eine- `fixed` Anweisung verwendet, um einen Byte Zeiger zu erhalten, der auf dieses Array zeigt. Dieser Byte Zeiger kann dann in einen umgewandelt werden, der `IntPtr` an übergeben wird `SetPixels` .

Das Array, das Sie erstellen, muss kein Bytearray sein. Dabei kann es sich um ein ganzzahliges Array mit nur zwei Dimensionen für die Zeile und Spalte handeln:

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Die- `MakePixel` Methode wird erneut verwendet, um die Farbkomponenten in einem 32-Bit-Pixel zu kombinieren.

Nur aus Gründen der Vollständigkeit ist hier derselbe Code, aber mit einem `SKColor` Wert, der in eine ganze Zahl ohne Vorzeichen umgewandelt wird:

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>Vergleichen der Techniken

Der Konstruktor der Farb **Verlaufs Farbseite** Ruft alle acht oben gezeigten Methoden auf und speichert die Ergebnisse:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Der Konstruktor wird beendet, indem ein erstellt wird `SKCanvasView` , um die resultierenden Bitmaps anzuzeigen. Der `PaintSurface` Handler teilt seine Oberfläche in acht Rechtecke `Display` auf und ruft auf, um die einzelnen Flächen anzuzeigen:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index],
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

Damit der Compiler den Code optimieren kann, wurde diese Seite im **Releasemodus** ausgeführt. Dies ist die Seite, die auf einem iPhone 8-Simulator auf einem MacBook Pro, einem Nexus 5 Android Phone und Surface pro 3 ausgeführt wird, auf dem Windows 10 ausgeführt wird. Vermeiden Sie aufgrund der Hardware Unterschiede, dass die Leistungszeiten zwischen den Geräten verglichen werden, sondern sehen Sie sich stattdessen die relativen Zeiten auf jedem Gerät an:

[![Farbverlaufs Bitmap](pixel-bits-images/GradientBitmap.png "Farbverlaufs Bitmap")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Im folgenden finden Sie eine Tabelle, in der die Ausführungszeiten in Millisekunden konsolidiert werden:

| API       | Datentyp | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3,17 |   10,77 | 3.49 |
| Okkludierte    |           | 0,32 |    1.23 | 0,07 |
| GetPixels | byte      | 0,09 |    0.24 | 0,10 |
|           | uint      | 0.06 |    0,26 | 0.05 |
|           | Skcolor   | 0,29 |    0,99 | 0,07 |
| SetPixel | byte      | 1.33 |    6,78 | 0,11 |
|           | uint      | 0.14 |    0.69 | 0.06 |
|           | Skcolor   | 0,35 |    1,93 | 0,10 |

Erwartungsgemäß `SetPixel` ist das Aufrufen von 65.536-mal der geringste Weg, um die Pixel einer Bitmap festzulegen. Das Ausfüllen eines `SKColor` Arrays und das Festlegen der `Pixels` -Eigenschaft ist viel besser, und es wird sogar positiv mit einigen der `GetPixels` Techniken und verglichen `SetPixels` . Das Arbeiten mit `uint` Pixelwerten ist im Allgemeinen schneller als das Festlegen separater `byte` Komponenten, und das Umrechnen des `SKColor` Werts in eine ganze Zahl ohne Vorzeichen erhöht den Aufwand für den Prozess.

Es ist auch interessant, die verschiedenen Farbverläufe zu vergleichen: die obersten Zeilen der einzelnen Plattformen sind identisch, und Sie zeigen den Farbverlauf an, wie er beabsichtigt war. Dies bedeutet, dass die `SetPixel` -Methode und die- `Pixels` Eigenschaft Pixel aus Farben unabhängig vom zugrunde liegenden Pixel Format ordnungsgemäß erstellen.

Die nächsten zwei Zeilen der IOS-und Android-Screenshots sind ebenfalls identisch, wodurch bestätigt wird, dass die kleine `MakePixel` Methode ordnungsgemäß für das Standard `Rgba8888` Pixel Format für diese Plattformen definiert ist.

Die untere Zeile der IOS-und Android-Screenshots ist abwärts, was darauf hinweist, dass die durch Umwandeln eines-Werts abgewanddete Ganzzahl ohne Vorzeichen das folgende Format hat `SKColor` :

AARRGGBB

Die Bytes sind in der folgenden Reihenfolge:

BB GG RR AA

Dies ist die `Bgra8888` Reihenfolge anstelle der `Rgba8888` Reihenfolge. Das `Brga8888` Format ist die Standardeinstellung für die universelle Windows-Plattform. aus diesem Grund sind die Farbverläufe in der letzten Zeile dieses Screenshots identisch mit der ersten Zeile. Die mittleren beiden Zeilen sind jedoch falsch, da der Code, der diese Bitmaps erstellt, eine Reihenfolge annimmt `Rgba8888` .

Wenn Sie den gleichen Code für den Zugriff auf Pixel Bits auf jeder Plattform verwenden möchten, können Sie mithilfe des-oder-Formats explizit ein erstellen `SKBitmap` `Rgba8888` `Bgra8888` . Wenn Sie `SKColor` Werte in bitmappixel umwandeln möchten, verwenden Sie `Bgra8888` .

## <a name="random-access-of-pixels"></a>Zufälliger Zugriff auf Pixel

Die `FillBitmapBytePtr` -Methode und die- `FillBitmapUintPtr` Methode auf der **Farbverlaufs Bitmap** -Seite haben von Schleifen profitiert, `for` die das sequenzielle Auffüllen der Bitmap, von der obersten Zeile bis zur untersten Zeile und von links nach rechts. Das Pixel kann mit derselben Anweisung festgelegt werden, die den Zeiger inkrementiert hat.

Manchmal ist es erforderlich, auf die Pixel nach dem Zufallsprinzip anstatt sequenziell zuzugreifen. Wenn Sie den Ansatz verwenden `GetPixels` , müssen Sie Zeiger basierend auf der Zeile und der Spalte berechnen. Dies wird auf der Seite " **Regenbogen Sinus** " veranschaulicht, die eine Bitmap erstellt, die einen Regenbogen in Form eines Zyklen einer Sinuskurve anzeigt.

Die Farben der Regenbogen Größe können am einfachsten mit dem Farbmodell HSL (Hue, Sättigung, Helligkeit) erstellt werden. Die- `SKColor.FromHsl` Methode erstellt einen- `SKColor` Wert mit Hue-Werten zwischen 0 und 360 (z. b. die Winkel eines Kreises, aber von rot, grün und blau und zurück zu rot) und die Werte für Sättigung und Helligkeit im Bereich von 0 bis 100. Bei den Farben eines Regen Stroms sollte die Sättigung auf maximal 100 und die Helligkeit auf einen Mittelwert von 50 festgelegt werden.

Der **Regenbogen Sinus** erstellt dieses Bild, indem es die Zeilen der Bitmap durchläuft und dann durch 360 Hue-Werte durchlaufen wird. Von jedem Hue-Wert wird eine Bitmap-Spalte berechnet, die auch auf einem Sinus Wert basiert:

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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

Beachten Sie, dass der Konstruktor die Bitmap basierend auf dem `SKColorType.Bgra8888` Format erstellt:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Dies ermöglicht es dem Programm, die Konvertierung von `SKColor` Werten `uint` ohne sorgen in Pixel zu verwenden. Obwohl Sie in diesem Programm keine Rolle spielt, sollten Sie bei jeder Verwendung der `SKColor` Konvertierung zum Festlegen von Pixeln auch angeben, `SKAlphaType.Unpremul` dass die `SKColor` Farbkomponenten nicht durch den Alpha-Wert vorab multipliziert werden.

Der Konstruktor verwendet dann die- `GetPixels` Methode, um einen Zeiger auf das erste Pixel der Bitmap abzurufen:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Für eine bestimmte Zeile und Spalte muss ein Offset Wert hinzugefügt werden `basePtr` . Dieser Offset ist die Zeilen Zeit der bitmapbreite und die Spalte:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

Der `SKColor` Wert wird im Arbeitsspeicher mithilfe dieses Zeigers gespeichert:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

Im- `PaintSurface` Handler von `SKCanvasView` wird die Bitmap gestreckt, um den Anzeigebereich auszufüllen:

[![Regenbogen Sinus](pixel-bits-images/RainbowSine.png "Regenbogen Sinus")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Von einer Bitmap zu einer anderen

Bei sehr vielen Bildverarbeitungsaufgaben müssen Pixel geändert werden, da Sie von einer Bitmap in eine andere übertragen werden. Diese Technik wird auf der Seite **Farbanpassung** veranschaulicht. Die Seite lädt eine der Bitmapressourcen und ermöglicht dann das Ändern des Bilds mithilfe von drei `Slider` Ansichten:

[![Farbanpassung](pixel-bits-images/ColorAdjustment.png "Farbanpassung")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Für jede Pixelfarbe fügt der erste `Slider` einen Wert zwischen 0 und 360 in den Farbton ein, verwendet jedoch den Modulo-Operator, um das Ergebnis zwischen 0 und 360 beizubehalten, wodurch die Farben entlang des Spektrums verschoben werden (wie der UWP-Bildschirm Abbildung zeigt). Mit dem zweiten `Slider` können Sie einen Multiplikationsfaktor zwischen 0,5 und 2 auswählen, der auf die Sättigung angewendet werden soll, und der dritte `Slider` führt die gleiche für die Helligkeit aus, wie im Android-Bildschirmfoto gezeigt.

Das Programm verwaltet zwei Bitmaps, die ursprüngliche Quell Bitmap mit dem Namen `srcBitmap` und die angepasste Ziel Bitmap mit dem Namen `dstBitmap` . Jedes Mal, wenn ein `Slider` verschoben wird, berechnet das Programm alle neuen Pixel in `dstBitmap` . Benutzer werden natürlich experimentieren `Slider` , indem Sie die Ansichten sehr schnell verschieben, sodass Sie die beste Leistung erzielen möchten, die Sie verwalten können. Dies umfasst die `GetPixels` -Methode für die Quell-und Ziel Bitmaps.

Die Seite **Farbanpassung** steuert nicht das Farb Format der Quell-und Ziel Bitmaps. Stattdessen enthält Sie eine etwas andere Logik für das `SKColorType.Rgba8888` -Format und das- `SKColorType.Bgra8888` Format. Quelle und Ziel können unterschiedliche Formate aufweisen, und das Programm funktioniert weiterhin.

Hier ist das Programm mit Ausnahme der entscheidenden `TransferPixels` Methode, mit der die Pixel von der Quelle an das Ziel übertragen werden. Der Konstruktor legt den Wert `dstBitmap` gleich fest `srcBitmap` . Der `PaintSurface` Handler zeigt Folgendes an `dstBitmap` :

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Der `ValueChanged` -Handler für die `Slider` Sichten berechnet die Anpassungs Werte und-Aufrufe `TransferPixels` .

Die gesamte- `TransferPixels` Methode wird als markiert `unsafe` . Zunächst erhalten Sie Byte Zeiger auf die Pixel Bits beider Bitmaps und durchlaufen dann alle Zeilen und Spalten. Aus der Quell Bitmap erhält die-Methode für jedes Pixel vier Bytes. Diese können sich entweder in der-oder der- `Rgba8888` `Bgra8888` Reihenfolge befinden. Durch Überprüfen auf den Farbtyp kann ein `SKColor` Wert erstellt werden. Anschließend werden die HSL-Komponenten extrahiert, angepasst und zum erneuten Erstellen des `SKColor` Werts verwendet. Abhängig davon, ob die Ziel Bitmap `Rgba8888` oder ist `Bgra8888` , werden die Bytes im zielbitmp gespeichert:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

Es ist wahrscheinlich, dass die Leistung dieser Methode noch weiter verbessert werden kann, indem separate Methoden für die verschiedenen Kombinationen von Farbtypen der Quell-und Ziel Bitmaps erstellt werden, und es wird vermieden, dass der Typ für jedes Pixel überprüft wird. Eine andere Möglichkeit besteht darin, mehrere `for` Schleifen für die `col` Variable basierend auf dem Farbtyp zu haben.

## <a name="posterization"></a>Posteriarisierung

Eine weitere häufige Aufgabe, bei der der Zugriff auf Pixel Bits umfasst, ist die nach _Alterung_ Die Zahl, wenn in den Pixeln einer Bitmap codierte Farben reduziert werden, sodass das Ergebnis mit einer begrenzten Farbpalette einem Hand gezeichnet-Poster ähnelt.

Auf **der Seite mit** der folgenden Seite wird dieser Prozess auf einem der affabbilder durchführt:

```csharp
public class PosterizePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public PosterizePage()
    {
        Title = "Posterize";

        unsafe
        {
            uint* ptr = (uint*)bitmap.GetPixels().ToPointer();
            int pixelCount = bitmap.Width * bitmap.Height;

            for (int i = 0; i < pixelCount; i++)
            {
                *ptr++ &= 0xE0E0E0FF;
            }
        }

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
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform;
    }
}
```

Der Code im Konstruktor greift auf jedes Pixel zu, führt eine bitweise AND-Operation mit dem Wert 0xe0e0e0ff aus und speichert das Ergebnis dann wieder in der Bitmap. Die Werte 0xe0e0e0ff behalten die hohen 3 Bits jeder Farbkomponente bei und legt die unteren 5 Bits auf 0 fest. Anstelle von 2<sup>24</sup> oder 16.777.216 Farben wird die Bitmap auf zwei<sup>9</sup> -oder 512-Farben reduziert:

[![Tontrennung](pixel-bits-images/Posterize.png "Tontrennung")](pixel-bits-images/Posterize-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
