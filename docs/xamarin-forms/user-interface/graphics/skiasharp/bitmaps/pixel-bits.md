---
title: Zugreifen auf SkiaSharp Bitmap Pixelbits
description: Entdecken Sie die verschiedenen Techniken zum Zugreifen auf und Ändern von Pixelbits von SkiaSharp-Bitmaps.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: 6c066f89dc8f558a9154138bf38ad4326fe21291
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918202"
---
# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>Zugreifen auf SkiaSharp Bitmap Pixelbits

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Wie Sie im Artikel [**Speichern von skiasharp-Bitmaps in Dateien**](saving.md)gesehen haben, werden Bitmaps im Allgemeinen in Dateien in komprimiertem Format gespeichert, z. b. JPEG oder PNG. Im Gegensatz dazu lässt wird eine SkiaSharp-Bitmap im Arbeitsspeicher gespeichert, nicht komprimiert. Es wird als eine sequenzielle Reihe von Pixel gespeichert. Dieses Format nicht komprimierte ermöglicht die Übertragung von Bitmaps in einer Oberfläche angezeigt.

Der Speicherblock belegt wird durch eine SkiaSharp-Bitmap ist auf sehr einfache Weise organisiert: er beginnt mit der ersten Zeile der Pixel von links nach rechts, und klicken Sie dann mit der zweiten Zeile wird fortgesetzt. Für Farbe Bitmaps besteht aus jedes Pixel von vier Bytes, was bedeutet, dass es sich bei der gesamten Speicherplatz erforderlich sind, indem die Bitmap viermal das Produkt der die Breite und Höhe ist.

In diesem Artikel wird beschrieben, wie eine Anwendung Zugriff auf diese Pixel, entweder direkt über den Zugriff auf die Bitmap Pixel Speicherblock erhalten oder indirekt. In einigen Fällen kann ein Programm möchten die Pixel eines Bilds zu analysieren und erstellen Sie ein Histogramm irgendeiner Art. In der Regel können Anwendungen eindeutige Bilder erstellen, indem Sie algorithmisch Erstellen der Pixel, die die Bitmap bilden:

![Pixel Bits-Beispiele](pixel-bits-images/PixelBitsSample.png "Pixel Bits-Beispiel")

## <a name="the-techniques"></a>Die Techniken

SkiaSharp bietet verschiedene Methoden für den Zugriff auf eine Bitmap des Pixelbits. Welche Methode Sie auswählen, ist normalerweise ein Kompromiss zwischen Codierung der Einfachheit halber (die zur Wartung und Debuggen zu erleichtern verknüpft ist) und Leistung. In den meisten Fällen verwenden Sie eine der folgenden Methoden und Eigenschaften von `SKBitmap` für den Zugriff auf die Pixel der Bitmap:

- Mit den Methoden `GetPixel` und `SetPixel` können Sie die Farbe eines einzelnen Pixels abrufen oder festlegen.
- Die `Pixels`-Eigenschaft ruft ein Array von Pixel Farben für die gesamte Bitmap ab oder legt das Array von Farben fest.
- `GetPixels` gibt die Adresse des von der Bitmap verwendeten Pixel Speichers zurück.
- `SetPixels` ersetzt die Adresse des von der Bitmap verwendeten Pixel Arbeitsspeichers.

Sie können den ersten beiden Verfahren als "high-Level" und die zweiten beiden als "niedrige Ebene auf." vorstellen. Es gibt einige andere Methoden und Eigenschaften, die Sie verwenden können, aber dies sind die wichtigsten.

Damit Sie die Leistungsunterschiede zwischen diesen Techniken anzeigen können, enthält die [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Anwendung eine Seite namens **Gradient Bitmap** , die eine Bitmap mit Pixeln erstellt, die rote und blaue Schattierungen zum Erstellen eines Farbverlaufs kombinieren. Das Programm erstellt acht verschiedene Kopien der Bitmap, verwenden alle verschiedene Verfahren zum Festlegen der Bitmap in Pixel. Jede dieser acht Bitmaps wird in einer separaten Methode erstellt, die auch eine kurze textbeschreibung des Verfahrens legt sie fest, und berechnet den Zeitaufwand für alle Pixel festgelegt. Jede Methode durchläuft die Pixel-Setting-Logik 100-Mal um eine bessere Schätzung für die Leistung zu erhalten.

### <a name="the-setpixel-method"></a>Die SetPixel-Methode

Wenn Sie nur mehrere einzelne Pixel festlegen oder erhalten müssen, sind die Methoden [`SetPixel`](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor)) und [`GetPixel`](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32)) ideal. Für jeden dieser beiden Methoden geben Sie die Spalte mit ganzen Zahlen und die Zeile an. Unabhängig vom Pixel Format können Sie mit diesen beiden Methoden das Pixel als `SKColor` Wert abrufen oder festlegen:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

Das `col`-Argument muss zwischen 0 und eins liegen, das kleiner als die `Width`-Eigenschaft der Bitmap ist, und `row` Bereich von 0 bis eins kleiner als die `Height`-Eigenschaft.

Hier ist die Methode in der **Farbverlaufs Bitmap** , die den Inhalt einer Bitmap mithilfe der `SetPixel`-Methode festlegt. Die Bitmap ist 256 x 256 Pixel, und die `for` Schleifen sind mit dem Wertebereich hart codiert:

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

Die Farben für jedes Pixel hat einen Rotanteils gleich der Bitmap-Spalte und eine blaue Komponente gleich auf die Zeile an. Die resultierende Bitmap wird auf der linken oberen Ecke, klicken Sie auf der rechten oberen Ecke, an der unteren linken, und Magenta auf der rechten unteren Ecke, mit einem Farbverlauf an anderer Stelle Blau, Rot, Schwarz.

Die `SetPixel`-Methode heißt 65.536-Mal, und unabhängig davon, wie effizient diese Methode sein könnte, ist es im Allgemeinen nicht ratsam, diese Zahl von API-aufrufen vorzunehmen, wenn eine Alternative verfügbar ist. Es gibt mehrere Alternativen.

### <a name="the-pixels-property"></a>Die Pixel-Eigenschaft

`SKBitmap` definiert eine [`Pixels`](xref:SkiaSharp.SKBitmap.Pixels) Eigenschaft, die ein Array von `SKColor` Werten für die gesamte Bitmap zurückgibt. Sie können auch `Pixels` verwenden, um ein Array von Farbwerten für die Bitmap festzulegen:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Die Pixel in das Array, beginnend mit der ersten Zeile von links nach rechts, klicken Sie dann die zweite Zeile angeordnet sind und so weiter. Die Gesamtanzahl der Farben im Array ist gleich dem Produkt der Bitmap-Breite und Höhe.

Obwohl diese Eigenschaft scheinbar effizient ist, sollten Sie Bedenken, dass die Pixel aus der Bitmap in das Array und aus dem Array zurück in die Bitmap kopiert werden und die Pixel aus und in `SKColor` Werte konvertiert werden.

Hier ist die-Methode in der `GradientBitmapPage`-Klasse, die die Bitmap mithilfe der `Pixels`-Eigenschaft festlegt. Die-Methode ordnet ein `SKColor` Array der erforderlichen Größe zu, kann jedoch die `Pixels`-Eigenschaft verwenden, um dieses Array zu erstellen:

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

Beachten Sie, dass der Index des `pixels` Arrays aus den `row`-und `col` Variablen berechnet werden muss. Die Zeile wird die Anzahl der Pixel in jeder Zeile (in diesem Fall 256) multipliziert, und klicken Sie dann die Spalte wird hinzugefügt.

`SKBitmap` definiert auch eine ähnliche `Bytes`-Eigenschaft, die ein Bytearray für die gesamte Bitmap zurückgibt, das jedoch für vollfarbige Bitmaps schwieriger ist.

### <a name="the-getpixels-pointer"></a>Der Zeiger GetPixels

Möglicherweise ist die leistungsfähigste Methode für den Zugriff auf die Bitmap-Pixel [`GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels), nicht mit der `GetPixel`-Methode oder der `Pixels`-Eigenschaft verwechselt werden. Sie sehen sofort einen Unterschied zu `GetPixels` darin, dass beim C# Programmieren etwas nicht ganz häufig angezeigt wird:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

Der .net- [`IntPtr`](xref:System.IntPtr) Typ stellt einen Zeiger dar. Sie wird `IntPtr` aufgerufen, weil es sich um die Länge einer Ganzzahl auf dem systemeigenen Prozessor des Computers handelt, auf dem das Programm ausgeführt wird, im Allgemeinen entweder 32 Bits oder 64 Bits. Der `IntPtr`, der `GetPixels` zurückgibt, ist die Adresse des tatsächlichen Speicherblocks, den das Bitmap-Objekt verwendet, um die Pixel zu speichern.

Sie können den `IntPtr` mithilfe der C# [`ToPointer`](xref:System.IntPtr.ToPointer) -Methode in einen Zeigertyp konvertieren. Die C#-Zeiger-Syntax ist identisch mit C- und C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

Die `ptr` Variable ist vom Typ " _Byte Zeiger_". Diese `ptr` Variable ermöglicht Ihnen den Zugriff auf die einzelnen Bytes des Arbeitsspeichers, die zum Speichern der Bitmap-Pixel verwendet werden. Verwenden Sie Code wie den folgenden liest ein Byte aus diesem Speicher oder einen Byte in den Arbeitsspeicher zu schreiben:

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

In diesem Kontext ist das Sternchen der C# Dereferenzierungsoperator und wird verwendet, um auf den Inhalt des Speichers zu verweisen, auf den von `ptr`gezeigt wird. Zunächst zeigt `ptr` auf das erste Byte des ersten Pixels der ersten Zeile der Bitmap, aber Sie können Arithmetik für die `ptr` Variable ausführen, um Sie an andere Positionen innerhalb der Bitmap zu verschieben.

Ein Nachteil ist, dass Sie diese `ptr` Variable nur in einem Codeblock verwenden können, der mit dem Schlüsselwort "`unsafe`" gekennzeichnet ist. Darüber hinaus muss die Assembly gekennzeichnet sein, damit eine unsichere Blöcke. Dies erfolgt in den Eigenschaften des Projekts.

Verwenden von Zeigern in C# geschrieben ist sehr leistungsfähig, aber auch sehr gefährlich. Sie müssen darauf achten, dass Sie nicht mehr, was soll der Zeiger auf Speicher zugreifen. Daher ist Zeiger verwenden zugeordnet, mit dem Wort "unsafe".

Hier ist die-Methode in der `GradientBitmapPage`-Klasse, die die `GetPixels`-Methode verwendet. Beachten Sie den `unsafe` Block, der den gesamten Code mit dem Byte Zeiger umfasst:

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

Wenn die `ptr` Variable erstmalig aus der `ToPointer`-Methode abgerufen wird, verweist Sie auf das erste Byte des äußersten linken Pixels der ersten Zeile der Bitmap. Die `for` Schleifen für `row` und `col` sind so eingerichtet, dass `ptr` mit dem Operator `++` inkrementiert werden können, nachdem jedes Byte jedes Pixels festgelegt wurde. Bei den anderen 99-Schleifen durch die Pixel muss die `ptr` wieder auf den Anfang der Bitmap festgelegt werden.

Jedes Pixel ist vier Bytes an Arbeitsspeicher, weshalb jedes Byte einzeln festgelegt werden. Im Code wird davon ausgegangen, dass sich die Bytes in der Reihenfolge rot, grün, blau und Alpha befinden, die mit dem `SKColorType.Rgba8888` Farbtyp konsistent ist. Sie erinnern sich, dass dies der Farbe für iOS und Android, aber nicht für die universelle Windows-Plattform ist. Standardmäßig erstellt die UWP Bitmaps mit dem `SKColorType.Bgra8888` Farbtyp. Aus diesem Grund erwarten, dass auf dieser Plattform andere Ergebnisse zu sehen!

Es ist möglich, den von `ToPointer` zurückgegebenen Wert in einen `uint` Zeiger anstatt einen `byte` Zeiger umzuwandeln. Dadurch wird eine gesamte Pixel in einer Anweisung zugegriffen werden kann. Wenn Sie den `++`-Operator auf diesen Zeiger anwenden, wird dieser um vier Bytes erhöht, um auf das nächste Pixel zu zeigen:

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

Das Pixel wird mithilfe der `MakePixel`-Methode festgelegt, die ein ganzzahliges Pixel aus den Komponenten rot, grün, blau und Alpha erstellt. Beachten Sie, dass das `SKColorType.Rgba8888`-Format eine Pixel Byte-Reihenfolge aufweist, die wie folgt aussieht:

RR, GG BB AA

Aber die ganze Zahl, die für diese Bytes ist:

AABBGGRR

In Übereinstimmung mit little-Endian-Architektur wird zuerst das niederwertigste Byte der Ganzzahl gespeichert sind. Diese `MakePixel` Methode funktioniert nicht ordnungsgemäß für Bitmaps mit dem `Bgra8888` Farbtyp.

Die `MakePixel`-Methode wird mit der [`MethodImplOptions.AggressiveInlining`](xref:System.Runtime.CompilerServices.MethodImplOptions) -Option gekennzeichnet, um den Compiler zu ermutigen, diese Methode nicht als separate Methode zu verwenden, sondern stattdessen den Code zu kompilieren, in dem die-Methode aufgerufen wird. Dies sollte die Leistung verbessern.

Interessanterweise definiert die `SKColor` Struktur eine explizite Konvertierung von `SKColor` in eine ganze Zahl ohne Vorzeichen, was bedeutet, dass ein `SKColor` Wert erstellt und eine Konvertierung in `uint` anstelle von `MakePixel`verwendet werden kann:

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

Die einzige Frage ist: ist das ganzzahlige Format des `SKColor` Werts in der Reihenfolge des `SKColorType.Rgba8888` Farbtyps oder des `SKColorType.Bgra8888` Farbtyps, oder ist es etwas anderes. Die Antwort auf diese Frage wird in Kürze offengelegt werden.

### <a name="the-setpixels-method"></a>Die SetPixels-Methode

`SKBitmap` auch eine Methode namens [`SetPixels`](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr))definiert, die wie folgt aufgerufen wird:

```csharp
bitmap.SetPixels(intPtr);
```

Denken Sie daran, dass `GetPixels` eine `IntPtr` die auf den Speicherblock verweist, der von der Bitmap zum Speichern der Pixel verwendet wird. Der `SetPixels`-Befehl _ersetzt_ diesen Speicherblock durch den Speicherblock, auf den von der `IntPtr` verwiesen wird, die als `SetPixels`-Argument angegeben ist. Die Bitmap frei klicken Sie dann auf den Speicherblock, die, den Sie zuvor verwendet wurde. Wenn `GetPixels` das nächste Mal aufgerufen wird, wird der mit `SetPixels`festgelegte Speicherblock abgerufen.

Zunächst scheint es so, als ob `SetPixels` Ihnen nicht mehr Leistung und Leistung als `GetPixels` bietet, während es weniger praktisch ist. Mit `GetPixels` Sie den Bitmap-Speicherblock abrufen und darauf zugreifen. Mit `SetPixels` Sie Speicherplatz zuweisen und darauf zugreifen, und legen Sie diesen dann als Bitmap-Speicherblock fest.

Die Verwendung von `SetPixels` bietet jedoch einen unterschiedlichen syntaktischen Vorteil: Sie ermöglicht den Zugriff auf die Bitmap-Pixel Bits mithilfe eines Arrays. Hier ist die Methode in `GradientBitmapPage`, die diese Technik veranschaulicht. Die Methode definiert zuerst ein mehrdimensionales Byte-Array, das für die Bytes, der die Pixel der Bitmap. Die erste Dimension ist die Zeile, die zweite Dimension ist die Spalte, und die dritte Dimension entspricht die vier Komponenten der einzelnen Pixel:

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

Nachdem das Array mit Pixeln aufgefüllt wurde, wird ein `unsafe`-Block und eine `fixed`-Anweisung verwendet, um einen Byte Zeiger abzurufen, der auf dieses Array zeigt. Dieser Byte Zeiger kann dann in eine `IntPtr` umgewandelt werden, die an `SetPixels`übergeben werden soll.

Das Array, das Sie erstellen kein Byte-Array sein. Ein ganzzahliges Array mit nur zwei Dimensionen für die Zeile und Spalte sind möglich:

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

Die `MakePixel`-Methode wird erneut verwendet, um die Farbkomponenten in einem 32-Bit-Pixel zu kombinieren.

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

### <a name="comparing-the-techniques"></a>Vergleichen die Techniken

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

Der Konstruktor wird beendet, indem ein `SKCanvasView` erstellt wird, um die resultierenden Bitmaps anzuzeigen. Der `PaintSurface`-Handler dividiert seine Oberfläche in acht Rechtecke und ruft `Display` auf, um jeden anzuzeigen:

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

Damit der Compiler den Code optimieren kann, wurde diese Seite im **Releasemodus** ausgeführt. Hier ist diese Seite, die auf einem iPhone 8-Simulator auf einem MacBook Pro, einem Nexus 5 Android-Telefon und Surface Pro 3 unter Windows 10 ausgeführt wird. Aufgrund der Hardwareunterschiede zu vermeiden Sie, vergleichen die leistungszeiten zwischen den Geräten, aber stattdessen sehen Sie sich die relative Häufigkeit auf jedem Gerät aus:

[![Farbverlaufs Bitmap](pixel-bits-images/GradientBitmap.png "Farbverlaufs Bitmap")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Hier ist eine Tabelle, die die Ausführung der Zeitüberschreitung in Millisekunden konsolidiert:

| API       | Datentyp | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel verwendet wird  |           | 3,17 |   10.77 | 3.49 |
| Okkludierte    |           | 0,32 |    1.23 | 0,07 |
| GetPixels | byte      | 0,09 |    0,24 | 0,10 |
|           | uint      | 0.06 |    0,26 | 0.05 |
|           | SKColor   | 0,29 |    0,99 | 0,07 |
| SetPixels | byte      | 1.33 |    6.78 | 0,11 |
|           | uint      | 0.14 |    0.69 | 0.06 |
|           | SKColor   | 0,35 |    1,93 | 0,10 |

Erwartungsgemäß ist das Aufrufen von `SetPixel` 65.536-mal der geringste Weg, um die Pixel einer Bitmap festzulegen. Das Auffüllen eines `SKColor` Arrays und das Festlegen der `Pixels`-Eigenschaft ist viel besser, und es wird sogar mit einigen der `GetPixels`-und `SetPixels` Techniken verglichen. Das Arbeiten mit `uint` Pixel-Werten ist im Allgemeinen schneller als das Festlegen separater `byte` Komponenten, und das Umrechnen des `SKColor` Werts in eine ganze Zahl ohne Vorzeichen erhöht den Aufwand für den Prozess.

Es ist auch interessant, die verschiedene Farbverläufe verglichen werden soll: die obersten Zeilen für jede Plattform entsprechen, und der Gradient zeigen, wie gewünscht. Dies bedeutet, dass die `SetPixel`-Methode und die `Pixels`-Eigenschaft Pixel aus Farben unabhängig vom zugrunde liegenden Pixel Format ordnungsgemäß erstellen.

Die nächsten zwei Zeilen der IOS-und Android-Screenshots sind ebenfalls identisch, wodurch bestätigt wird, dass die kleine `MakePixel`-Methode ordnungsgemäß für das standardmäßige `Rgba8888` Pixel Format für diese Plattformen definiert ist.

Die untere Zeile der IOS-und Android-Screenshots ist abwärts, was darauf hinweist, dass die durch Umwandeln eines `SKColor` Werts abgewanddete Ganzzahl ohne Vorzeichen das folgende Format hat:

AARRGGBB

Die Bytes werden in der Reihenfolge:

BB GG RR AA

Dies ist die `Bgra8888` Reihenfolge anstelle der `Rgba8888` Reihenfolge. Das `Brga8888` Format ist die Standardeinstellung für die universelle Windows-Plattform. aus diesem Grund sind die Farbverläufe in der letzten Zeile dieses Screenshots identisch mit der ersten Zeile. Die mittleren beiden Zeilen sind jedoch falsch, da der Code, der diese Bitmaps erstellt, eine `Rgba8888` Reihenfolge annimmt.

Wenn Sie den gleichen Code für den Zugriff auf Pixel Bits auf jeder Plattform verwenden möchten, können Sie explizit eine `SKBitmap` mithilfe des `Rgba8888`-oder `Bgra8888`-Formats erstellen. Wenn Sie `SKColor` Werte in Bitmap-Pixel umwandeln möchten, verwenden Sie `Bgra8888`.

## <a name="random-access-of-pixels"></a>Wahlfreien Zugriff von Pixeln

Die Methoden `FillBitmapBytePtr` und `FillBitmapUintPtr` auf der **Farbverlaufs Bitmap** -Seite haben von `for` Schleifen profitiert, mit denen die Bitmap sequenziell ausgefüllt wird, von der obersten Zeile bis zur untersten Zeile und von links nach rechts. Das Pixel konnte mit der gleichen Anweisung festgelegt werden, die den Zeiger erhöht.

Manchmal ist es erforderlich, die die Pixel nacheinander, sondern nach dem Zufallsprinzip zugreifen. Wenn Sie den `GetPixels` Ansatz verwenden, müssen Sie Zeiger basierend auf der Zeile und der Spalte berechnen. Dies wird auf der Seite " **Regenbogen Sinus** " veranschaulicht, die eine Bitmap erstellt, die einen Regenbogen in Form eines Zyklen einer Sinuskurve anzeigt.

Die Farben des Regenbogens sind am einfachsten, erstellen mit dem Modell der HSL (Farbton, Sättigung, Helligkeit)-Farbe. Mit der `SKColor.FromHsl`-Methode wird ein `SKColor` Wert mit Hue-Werten erstellt, die zwischen 0 und 360 liegen (z. b. die Winkel eines Kreises, aber von rot, grün und blau und zurück zu rot), sowie die Sättigung und die Leuchtkraft Werte zwischen 0 und 100. Für die Farben des Regenbogens sollte die Sättigung auf ein Maximum von 100 und die Helligkeit zu einem Mittelpunkt von 50 festgelegt werden.

Der **Regenbogen Sinus** erstellt dieses Bild, indem es die Zeilen der Bitmap durchläuft und dann durch 360 Hue-Werte durchlaufen wird. Von jedem Hue-Wert berechnet es sich um eine Bitmap-Spalte, die auch auf einen Sinuswert basiert:

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

Beachten Sie, dass der Konstruktor die Bitmap basierend auf dem `SKColorType.Bgra8888`-Format erstellt:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Dies ermöglicht es dem Programm, die Konvertierung von `SKColor` Werten in `uint` Pixel zu verwenden, ohne sich Gedanken machen zu müssen. Obwohl es in diesem Programm keine Rolle spielt, sollten Sie beim Verwenden der `SKColor` Konvertierung zum Festlegen von Pixeln auch `SKAlphaType.Unpremul` angeben, da `SKColor` seine Farbkomponenten nicht durch den Alpha-Wert vorab multipliziert.

Der Konstruktor verwendet dann die `GetPixels`-Methode, um einen Zeiger auf das erste Pixel der Bitmap zu erhalten:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Für eine bestimmte Zeile und Spalte muss `basePtr`ein Offset Wert hinzugefügt werden. Dieser Offset ist die Zeile Mal die Breite der Bitmap, plus der Spalte:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

Der `SKColor` Wert wird mithilfe dieses Zeigers im Arbeitsspeicher gespeichert:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

Im `PaintSurface` Handler der `SKCanvasView`wird die Bitmap gestreckt, um den Anzeigebereich auszufüllen:

[![Regenbogen Sinus](pixel-bits-images/RainbowSine.png "Regenbogen Sinus")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Aus einer einzigen Bitmap in eine andere

Sehr viele bildverarbeitung Aufgaben zur grundlegenden netzwerkprotokollverwaltung Pixel ändern, wie sie aus einer einzigen Bitmap in eine andere übertragen werden. Diese Technik wird auf der Seite **Farbanpassung** veranschaulicht. Die Seite lädt eine der Bitmapressourcen und ermöglicht dann das Ändern des Bilds mithilfe von drei `Slider` Ansichten:

[![Farbanpassung](pixel-bits-images/ColorAdjustment.png "Farbanpassung")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Für jede Pixelfarbe fügt der erste `Slider` einen Wert zwischen 0 und 360 in den Farbton ein, verwendet jedoch den Modulo-Operator, um das Ergebnis zwischen 0 und 360 zu bewegen, wodurch sich die Farben entlang des Spektrums verändern (wie der UWP-Bildschirm Abbildung zeigt). Mit dem zweiten `Slider` können Sie einen Multiplikationsfaktor zwischen 0,5 und 2 auswählen, der auf die Sättigung angewendet werden soll, und der dritte `Slider` führt die gleiche für die Helligkeit aus, wie im Android-Bildschirmfoto gezeigt.

Das Programm verwaltet zwei Bitmaps, die ursprüngliche Quell Bitmap namens `srcBitmap` und die angepasste Ziel Bitmap mit dem Namen `dstBitmap`. Jedes Mal, wenn ein `Slider` verschoben wird, berechnet das Programm alle neuen Pixel in `dstBitmap`. Benutzer werden natürlich experimentieren, indem Sie die `Slider` Ansichten sehr schnell verschieben, sodass Sie die beste Leistung erzielen möchten, die Sie verwalten können. Dies umfasst die `GetPixels`-Methode für die Quell-und Ziel Bitmaps.

Die Seite **Farbanpassung** steuert nicht das Farb Format der Quell-und Ziel Bitmaps. Stattdessen enthält Sie eine etwas andere Logik für `SKColorType.Rgba8888`-und `SKColorType.Bgra8888` Formate. Die Quelle und Ziel können verschiedene Formate werden, und das Programm weiterhin funktionieren.

Hier ist das Programm mit Ausnahme der entscheidenden `TransferPixels` Methode, die die Pixel der Quelle an das Ziel überträgt. Der Konstruktor legt `dstBitmap` gleich `srcBitmap`fest. Der `PaintSurface`-Handler zeigt `dstBitmap`an:

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

Der `ValueChanged` Handler für die `Slider` Sichten berechnet die Anpassungs Werte und ruft `TransferPixels`auf.

Die gesamte `TransferPixels`-Methode wird als `unsafe`markiert. Er beginnt mit dem Abrufen der Byte-Zeiger auf die Pixelbits der beiden Bitmaps und durchläuft dann, bis alle Zeilen und Spalten. Aus der Quell-Bitmap Ruft die Methode vier Bytes für jedes Pixel ab. Diese können entweder in der `Rgba8888` oder `Bgra8888` Reihenfolge vorliegen. Durch das Überprüfen auf den Farbtyp kann ein `SKColor` Wert erstellt werden. Anschließend werden die HSL-Komponenten extrahiert, angepasst und zum erneuten Erstellen des `SKColor` Werts verwendet. Abhängig davon, ob die Ziel Bitmap `Rgba8888` oder `Bgra8888`ist, werden die Bytes im zielbitmp gespeichert:

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

Es ist wahrscheinlich, dass die Leistung dieser Methode kann noch verbessert werden, indem Sie separate Methoden für die verschiedenen Kombinationen von Farbe bitmaptypen der Quelle und Ziel erstellt werden, und zu überprüfen, ob des Typs für jedes Pixel. Eine andere Möglichkeit besteht darin, mehrere `for` Schleifen für die `col` Variable basierend auf dem Farbtyp zu haben.

## <a name="posterization"></a>Keine Farbsprünge auftreten

Eine weitere häufige Aufgabe, bei der der Zugriff auf Pixel Bits umfasst, ist die nach _Alterung_ Die Anzahl, wenn Farben in der Bitmap Pixel codiert wird reduziert, so, dass das Ergebnis als Hand gezeichnetes Poster mit einer begrenzten Farbpalette ähnelt.

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

Der Code im Konstruktor greift auf jedes Pixel, führt eine bitweise AND-Operation mit dem Wert 0xE0E0E0FF und speichert dann das Ergebnis wieder in der Bitmap. Die Werte 0xE0E0E0FF behält die oberen 3 Bits der einzelnen Komponenten der Farbe und legt die unteren 5 Bits auf 0 fest. Anstelle von 2<sup>24</sup> oder 16.777.216 Farben wird die Bitmap auf zwei<sup>9</sup> -oder 512-Farben reduziert:

[![Nach unten](pixel-bits-images/Posterize.png "Tontrennung")](pixel-bits-images/Posterize-Large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
