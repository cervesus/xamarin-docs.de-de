---
title: Zugreifen auf SkiaSharp Pixelbits
description: Entdecken Sie die verschiedenen Techniken zum Zugreifen auf und Ändern von Pixelbits von SkiaSharp-Bitmaps.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: charlespetzold
ms.author: chape
ms.date: 07/11/2018
ms.openlocfilehash: 51252a1c18602c704b87f7b01efe7cc5a7549ac1
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131289"
---
# <a name="accessing-skiasharp-pixel-bits"></a>Zugreifen auf SkiaSharp Pixelbits

In diesem Artikel haben Sie gesehen [ **Speichern von SkiaSharp-Bitmaps, die Dateien**](saving.md), Bitmaps werden in der Regel in Dateien in einem komprimierten Format, z. B. JPEG oder PNG-Datei gespeichert. Im Gegensatz dazu lässt wird eine SkiaSharp-Bitmap im Arbeitsspeicher gespeichert, nicht komprimiert. Es wird als eine sequenzielle Reihe von Pixel gespeichert. Dieses Format nicht komprimierte ermöglicht die Übertragung von Bitmaps in einer Oberfläche angezeigt.

Der Speicherblock belegt wird durch eine SkiaSharp-Bitmap ist auf sehr einfache Weise organisiert: er beginnt mit der ersten Zeile der Pixel von links nach rechts, und klicken Sie dann mit der zweiten Zeile wird fortgesetzt. Für Farbe Bitmaps besteht aus jedes Pixel von vier Bytes, was bedeutet, dass es sich bei der gesamten Speicherplatz erforderlich sind, indem die Bitmap viermal das Produkt der die Breite und Höhe ist.

In diesem Artikel wird beschrieben, wie eine Anwendung Zugriff auf diese Pixel, entweder direkt über den Zugriff auf die Bitmap Pixel Speicherblock erhalten oder indirekt. In einigen Fällen kann ein Programm möchten die Pixel eines Bilds zu analysieren und erstellen Sie ein Histogramm irgendeiner Art. In der Regel können Anwendungen eindeutige Bilder erstellen, indem Sie algorithmisch Erstellen der Pixel, die die Bitmap bilden:

![Pixel Bits Beispiele](pixel-bits-images/PixelBitsSample.png "Pixel Bits-Beispiel")

## <a name="the-techniques"></a>Die Techniken

SkiaSharp bietet verschiedene Methoden für den Zugriff auf eine Bitmap des Pixelbits. Welche Methode Sie auswählen, ist normalerweise ein Kompromiss zwischen Codierung der Einfachheit halber (die zur Wartung und Debuggen zu erleichtern verknüpft ist) und Leistung. In den meisten Fällen verwenden Sie eine der folgenden Methoden und Eigenschaften der `SKBitmap` für den Zugriff auf die Pixel der Bitmap:

- Die `GetPixel` und `SetPixel` Methoden ermöglichen es Ihnen, Abrufen oder Festlegen der Farbe eines einzelnen Pixels.
- Die `Pixels` Eigenschaft ruft ein Array von von Pixelfarben für das gesamte Bitmap ab, oder legt das Array der Farben.
- `GetPixels` Gibt die Adresse des Speichers ein, die die Bitmap Pixel zurück.
- `SetPixels` ersetzt die Adresse des Speichers Pixel, die von der Bitmap verwendet.

Sie können den ersten beiden Verfahren als "high-Level" und die zweiten beiden als "niedrige Ebene auf." vorstellen. Es gibt einige andere Methoden und Eigenschaften, die Sie verwenden können, aber dies sind die wichtigsten.

Können Sie sehen, die Leistungsunterschiede zwischen diesen Funktionen und Techniken der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Anwendung enthält eine Seite namens **Farbverlauf Bitmap** , erstellt eine Bitmap mit Pixeln, die rote und blaue grauschattierungen zum Erstellen eines Verlaufs zu kombinieren. Das Programm erstellt acht verschiedene Kopien der Bitmap, verwenden alle verschiedene Verfahren zum Festlegen der Bitmap in Pixel. Jede dieser acht Bitmaps wird in einer separaten Methode erstellt, die auch eine kurze textbeschreibung des Verfahrens legt sie fest, und berechnet den Zeitaufwand für alle Pixel festgelegt. Jede Methode durchläuft die Pixel-Setting-Logik 100-Mal um eine bessere Schätzung für die Leistung zu erhalten. 

### <a name="the-setpixel-method"></a>Die SetPixel-Methode

Wenn Sie nur festlegen oder Abrufen von mehreren einzelnen Pixel, müssen die [ `SetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixel/p/System.Int32/System.Int32/SkiaSharp.SKColor/) und [ `GetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixel/p/System.Int32/System.Int32/) Methoden eignen sich ideal. Für jeden dieser beiden Methoden geben Sie die Spalte mit ganzen Zahlen und die Zeile an. Unabhängig von das Pixelformat, diese beiden Methoden können Sie abrufen oder Festlegen der Pixel als eine `SKColor` Wert:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

Die `col` Argument muss im Bereich von 0 auf eine kleiner als der `Width` Eigenschaft der Bitmap und `row` liegt zwischen 0 und einer kleiner als der `Height` Eigenschaft.

Hier ist die Methode **Farbverlauf Bitmap** , festlegt, dass der Inhalt eines Bitmap mit den `SetPixel` Methode. Die Bitmap ist 256 x 256 Pixel und die `for` Schleifen sind mit den Bereich der Werte hartcodiert:

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

Die `SetPixel` Methode ist 65.536 Mal aufgerufen wurde, und unabhängig davon, wie effizient dieser Methode werden kann, ist in der Regel keine gute Idee, dass viele API-Aufrufe ist eine Alternative zur Verfügung. Es gibt mehrere Alternativen.

### <a name="the-pixels-property"></a>Die Pixel-Eigenschaft

`SKBitmap` definiert eine [ `Pixels` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Pixels/) Eigenschaft, die ein Array von zurückgibt `SKColor` Werte für das gesamte Bitmap. Sie können auch `Pixels` ein Array von Werten der Farbe für die Bitmap festlegen:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Die Pixel in das Array, beginnend mit der ersten Zeile von links nach rechts, klicken Sie dann die zweite Zeile angeordnet sind und so weiter. Die Gesamtanzahl der Farben im Array ist gleich dem Produkt der Bitmap-Breite und Höhe.

Auch wenn diese Eigenschaft effizient zu sein scheint, sollten Sie bedenken, die die Pixel der Bitmap in das Array, und aus dem Array zurück in die Bitmap kopiert werden, und aus dem und in Pixel konvertiert `SKColor` Werte.

Hier ist die Methode der `GradientBitmapPage` -Klasse, die die Bitmap mit legt die `Pixels` Eigenschaft. Die Methode ordnet eine `SKColor` Array die erforderliche Größe, aber es hätte die `Pixels` Eigenschaft, um dieses Array zu erstellen:

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

Beachten Sie, dass der Index des dem `pixels` Array aus berechnet werden muss die `row` und `col` Variablen. Die Zeile wird die Anzahl der Pixel in jeder Zeile (in diesem Fall 256) multipliziert, und klicken Sie dann die Spalte wird hinzugefügt.

`SKBitmap` definiert auch eine ähnliche `Bytes` -Eigenschaft, ein Byte-Array für das gesamte Bitmap zurückgegeben, ist aber umständlicher für Farbe Bitmaps.

### <a name="the-getpixels-pointer"></a>Der Zeiger GetPixels

Potenziell die leistungsfähigste Methode die Pixel auf besteht [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixels()/), nicht zu verwechseln mit der `GetPixel` Methode oder der `Pixels` Eigenschaft. Sehen Sie sofort einen Unterschied mit `GetPixels` darin, dass es etwas in c#-Programmierung nicht sehr häufig zurückgegeben:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [ `IntPtr` ](xref:System.IntPtr) Typ stellt einen Zeiger dar. Es heißt `IntPtr` , da es die Länge einer ganzen Zahl für den systemeigenen Prozessor des Computers, auf dem das Programm ausgeführt wird, in der Regel entweder 32-Bit oder 64 Bit lang, ist. Die `IntPtr` , `GetPixels` gibt ist die Adresse des tatsächlichen Speicherblocks, der das Bitmapobjekt verwendet wird, um seine Pixel zu speichern. 

Sie konvertieren die `IntPtr` in einer C#-Zeiger mit dem [ `ToPointer` ](xref:System.IntPtr.ToPointer) Methode. Die C#-Zeiger-Syntax ist identisch mit C- und C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

Die `ptr` Variable ist vom Typ _Bytezeiger_. Dies `ptr` Variable können Sie auf die einzelnen Bytes an Arbeitsspeicher zugreifen, die verwendet werden, um die Pixel der Bitmap zu speichern. Verwenden Sie Code wie den folgenden liest ein Byte aus diesem Speicher oder einen Byte in den Arbeitsspeicher zu schreiben:

```sharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

In diesem Kontext das Sternchen ist der C#- _Dereferenzierungsoperator_ und wird verwendet, um den Inhalt des Arbeitsspeichers verweist verweisen `ptr`. Zunächst `ptr` verweist auf das erste Byte der das erste Pixel der ersten Zeile der Bitmap, aber Sie können arithmetische Operationen ausführen, auf die `ptr` Variablen, verschieben Sie es an anderen Speicherorten innerhalb der Bitmap.

Ein Nachteil besteht darin, dass Sie, dies verwenden können `ptr` Variable nur in einem Codeblock mit markiert die `unsafe` Schlüsselwort. Darüber hinaus muss die Assembly gekennzeichnet sein, damit eine unsichere Blöcke. Dies erfolgt in den Eigenschaften des Projekts.

Verwenden von Zeigern in C# geschrieben ist sehr leistungsfähig, aber auch sehr gefährlich. Sie müssen darauf achten, dass Sie nicht mehr, was soll der Zeiger auf Speicher zugreifen. Daher ist Zeiger verwenden zugeordnet, mit dem Wort "unsafe".

Hier ist die Methode der `GradientBitmapPage` -Klasse, verwendet der `GetPixels` Methode. Beachten Sie, dass die `unsafe` Block, der gesamten Code mit den Byte-Zeiger umfasst:

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

Wenn die `ptr` Variable zunächst aus einer der `ToPointer` -Methode, diese verweist auf das erste Byte der ganz linke Pixel der ersten Zeile der Bitmap. Die `for` für Schleifen `row` und `col` eingerichtet werden, damit `ptr` inkrementiert werden kann, mit der `++` Operator jedes Byte der einzelnen Pixel festgelegt ist. Für das andere 99 durchläuft die Pixel der `ptr` müssen zurück an den Anfang der Bitmap festgelegt werden.

Jedes Pixel ist vier Bytes an Arbeitsspeicher, weshalb jedes Byte einzeln festgelegt werden. Hier der Code wird davon ausgegangen, dass die Bytes in der Reihenfolge Rot, Grün, Blau werden und alpha, die konsistent mit der `SKColorType.Rgba8888` color-Typ. Sie erinnern sich, dass dies der Farbe für iOS und Android, aber nicht für die universelle Windows-Plattform ist. Standardmäßig erstellt die UWP Bitmaps mit der `SKColorType.Bgra8888` color-Typ. Aus diesem Grund erwarten, dass auf dieser Plattform andere Ergebnisse zu sehen!

Es ist möglich, der von zurückgegebene Wert umgewandelt `ToPointer` zu einem `uint` Zeiger anstelle eines `byte` Zeiger. Dadurch wird eine gesamte Pixel in einer Anweisung zugegriffen werden kann. Anwenden der `++` Operator, um diesen Zeiger erhöht ihn um 4 Bytes, um auf das nächste Pixel zu verweisen:

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

Das Pixel festgelegt ist, mit der `MakePixel` -Methode, die eine ganze Zahl Pixel von Rot, Grün, Blau und alpha-Komponenten erstellt. Beachten Sie, dass die `SKColorType.Rgba8888` Format hat, ein Pixel bytesortierung wie folgt:

RR, GG BB AA

Aber die ganze Zahl, die für diese Bytes ist:

AABBGGRR

In Übereinstimmung mit little-Endian-Architektur wird zuerst das niederwertigste Byte der Ganzzahl gespeichert sind. Dies `MakePixel` -Methode funktioniert nicht ordnungsgemäß für Bitmaps mit der `Bgra8888` color-Typ.

Die `MakePixel` Methode wird gekennzeichnet, mit der [ `MethodImplOptions.AggressiveInlining` ](xref:System.Runtime.CompilerServices.MethodImplOptions) Option zur Förderung des Compilers zu vermeiden, dass dies eine separate Methode, sondern stattdessen zum Kompilieren des Codes, in denen die Methode aufgerufen wird. Dies sollte die Leistung verbessern.

Interessanterweise der `SKColor` Struktur definiert eine explizite Konvertierung vom `SKColor` Ganzzahl ohne Vorzeichen, was bedeutet, dass ein `SKColor` Wert erstellt werden, und eine Konvertierung in `uint` kann verwendet werden, anstelle von `MakePixel`:

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

Die einzige Frage lautet: ist das Format ganze Zahl, der die `SKColor` in der Reihenfolge der Wert der `SKColorType.Rgba8888` color-Typ, oder die `SKColorType.Bgra8888` color-Typ, oder ist es etwas anderes vollständig? Die Antwort auf diese Frage wird in Kürze offengelegt werden.

### <a name="the-setpixels-method"></a>Die SetPixels-Methode

`SKBitmap` definiert auch eine Methode namens [ `SetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixels/p/System.IntPtr/), die Sie wie folgt aufrufen:

```csharp
bitmap.SetPixels(intPtr);
```

Zur Erinnerung: `GetPixels` erhält eine `IntPtr` verweisen auf den Speicherblock, der von der Bitmap verwendet wird, um seine Pixel zu speichern. Die `SetPixels` Aufrufen _ersetzt_ , Speicherblock mit den Speicherblock, der auf die verwiesen wird durch die `IntPtr` angegebenen als der `SetPixels` Argument. Die Bitmap frei klicken Sie dann auf den Speicherblock, die, den Sie zuvor verwendet wurde. Das nächste Mal `GetPixels` wird aufgerufen, er erhält den Speicherblock mit festgelegt `SetPixels`.

Zunächst erscheint es als ob `SetPixels` bietet mehr Leistung und Leistung als `GetPixels` während Sie weniger einfach. Mit `GetPixels` Sie erhalten Sie den Bitmap-Speicherblock und darauf zugreifen. Mit `SetPixels` Sie zuordnen und Zugriff auf bestimmte Menge an Arbeitsspeicher, und legen Sie dieses dann als Bitmap Speicherblocks.

Während mit `SetPixels` bietet einen unterschiedlichen syntaktischen Vorteil: sie Pixelbits Bitmap mit einem Array zugreifen können. Hier ist die Methode `GradientBitmapPage` , die diese Technik veranschaulicht. Die Methode definiert zuerst ein mehrdimensionales Byte-Array, das für die Bytes, der die Pixel der Bitmap. Die erste Dimension ist die Zeile, die zweite Dimension ist die Spalte, und die dritte Dimension entspricht die vier Komponenten der einzelnen Pixel:

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

Nach das Array mit Pixel gefüllt wurde ein `unsafe` Block und ein `fixed` Anweisung verwendet, um ein Byte-Zeiger abzurufen, die auf dieses Array verweist. Byte-Zeiger, klicken Sie dann auf umgewandelt werden kann ein `IntPtr` Übergabe an `SetPixels`.

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

Die `MakePixel` Methode wird erneut verwendet, um die Komponenten der Farbe in eine 32-Bit-Pixel zu kombinieren.

Nur aus Gründen der Vollständigkeit, hier ist der gleiche Code jedoch mit einer `SKColor` Wert in eine Ganzzahl ohne Vorzeichen umgewandelt:

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

Der Konstruktor, der die **Verlaufsfarbe** Seite alle acht Versionen der oben gezeigten Methoden aufruft, und speichert die Ergebnisse:

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

Der Konstruktor wird abgeschlossen, indem Sie erstellen eine `SKCanvasView` die resultierenden Bitmaps angezeigt. Die `PaintSurface` Handler teilt seine Oberfläche in acht Rechtecke und Aufrufe `Display` jeweils angezeigt:

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

Um den Compiler an, den Code optimieren zu ermöglichen, auf dieser Seite ausgeführt wurde, im **Version** Modus. Hier ist diese Seite, die auf einem iPhone 8-Simulator auf einem MacBook Pro, einem Nexus 5 Android-Telefon und Surface Pro 3 unter Windows 10 ausgeführt wird. Aufgrund der Hardwareunterschiede zu vermeiden Sie, vergleichen die leistungszeiten zwischen den Geräten, aber stattdessen sehen Sie sich die relative Häufigkeit auf jedem Gerät aus:

[![Farbverlauf Bitmap](pixel-bits-images/GradientBitmap.png "Farbverlauf Bitmap")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Hier ist eine Tabelle, die die Ausführung der Zeitüberschreitung in Millisekunden konsolidiert:

| API       | Datentyp | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel verwendet wird  |           | 3.17 |   10.77 | 3.49 |
| Okkludierte    |           | 0,32 |    "1,23" | 0,07 |
| GetPixels | byte      | 0,09 |    0,24 | 0.10 |
|           | uint      | 0,06 |    0,26 | 0,05 |
|           | SKColor   | 0,29 |    0,99 | 0,07 |
| SetPixels | byte      | 1.33 |    6.78 | 0.11 |
|           | uint      | 0,14 |    0.69 | 0,06 |
|           | SKColor   | 0.35 |    1.93 | 0.10 |

Aufrufen von erwartungsgemäß funktioniert, `SetPixel` 65.536 Zeiten lässt am geringsten Effeicient der Bitmap Pixel festgelegt. Ausfüllen einer `SKColor` Array und die Einstellung der `Pixels` Eigenschaft ist viel besser, und vergleicht auch zeigt, mit einigen der `GetPixels` und `SetPixels` Techniken. Arbeiten mit `uint` Pixelwerte ist im Allgemeinen schneller als separate Einstellung `byte` Komponenten und zum Konvertieren der `SKColor` Ganzzahl ohne Vorzeichen Mehrwert Mehraufwand an den Prozess.

Es ist auch interessant, die verschiedene Farbverläufe verglichen werden soll: die obersten Zeilen aller drei Plattformen identisch sind, und der Gradient zeigen, wie gewünscht. Dies bedeutet, dass die `SetPixel` Methode und die `Pixels` Eigenschaft ordnungsgemäß Pixel von Farben, unabhängig vom zugrunde liegenden Pixelformat erstellen.

Die nächsten beiden Zeilen der IOS- und Android Screenshots sind identisch, die Bestätigung, die das kleine `MakePixel` Methode ordnungsgemäß definiert ist, für den standardmäßigen `Rgba8888` Pixelformat für diese Plattformen.

Die untersten Zeile der IOS- und Android Screenshots ist abwärtskompatibel mit der gibt an, dass die Ganzzahl ohne Vorzeichen durch Umwandeln erhalten eine `SKColor` Wert hat das Format:

AARRGGBB

Die Bytes werden in der Reihenfolge:

BB GG RR AA

Dies ist die `Bgra8888` Reihenfolge statt der `Rgba8888` Sortierung. Die `Brga8888` Format ist das Standardformat für die universelle Windows-Plattform, weshalb die Farbverläufe in diesem Screenshot der letzten Zeile die erste Zeile identisch. Aber die beiden mittleren Spalten sind falsch, da davon ausgegangen, der Code, der diese Bitmaps erstellen dass eine `Rgba8888` Sortierung.

Wenn Sie den gleichen Code für den Zugriff auf Pixelbits auf allen drei Plattformen verwenden möchten, können Sie explizit erstellen eine `SKBitmap` entweder die `Rgba8888` oder `Bgra8888` Format. Wenn Sie umwandeln möchten `SKColor` verwenden Sie die Werte in Pixel der Bitmap, `Bgra8888`.

## <a name="random-access-of-pixels"></a>Wahlfreien Zugriff von Pixeln

Die `FillBitmapBytePtr` und `FillBitmapUintPtr` Methoden in der **Farbverlauf Bitmap** Seite Profil `for` Schleifen entworfen, um die Bitmap nacheinander von der obersten Zeile, die auf der untersten Zeile, und klicken Sie in den einzelnen Zeilen von links nach rechts zu füllen. Das Pixel konnte mit der gleichen Anweisung festgelegt werden, die den Zeiger erhöht. 

Manchmal ist es erforderlich, die die Pixel nacheinander, sondern nach dem Zufallsprinzip zugreifen. Bei Verwendung der `GetPixels` Ansatz müssen Sie Zeiger basierend auf der Zeile und Spalte zu berechnen. Dies wird veranschaulicht, der **Rainbow Sinus** Seite erstellt eine Bitmap mit einem Rainbow in Form eines Zyklus einer Sinus-Kurve. 

Die Farben des Regenbogens sind am einfachsten, erstellen mit dem Modell der HSL (Farbton, Sättigung, Helligkeit)-Farbe. Die `SKColor.FromHsl` -Methode erstellt eine `SKColor` Wert mit Hue-Werten, die zwischen 0 und 360 (z. B. den Winkeln einen Kreis, aber für den Wechsel von Rot über Grün und Blau und zurück in Rot) liegen und Sättigung und Helligkeit-Werte, die im Bereich von 0 bis 100. Für die Farben des Regenbogens sollte die Sättigung auf ein Maximum von 100 und die Helligkeit zu einem Mittelpunkt von 50 festgelegt werden.

**Rainbow Sinus** erstellt dieses Bild durch durchläuft die Zeilen der Bitmap, und klicken Sie dann auf 360 Hue Werte durchlaufen. Von jedem Hue-Wert berechnet es sich um eine Bitmap-Spalte, die auch auf einen Sinuswert basiert:

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

Beachten Sie, dass der Konstruktor die Bitmap, die auf der Grundlage erstellt die `SKColorType.Bgra8888` Format:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Dadurch kann die Anwendung verwenden Sie die Konvertierung von `SKColor` von Datumswerten in `uint` Pixel ohne Sorge. Obwohl es eine Rolle in der dieses Programm nicht abgespielt wird, bei Verwendung der `SKColor` Konvertierung in Pixel festgelegt, Sie sollten auch angeben, `SKAlphaType.Unpremul` da `SKColor` nicht vorzumultiplizieren, damit das seine Farbe-Komponenten von der alpha-Wert.

Dann verwendet der Konstruktor der `GetPixels` Methode, um einen Zeiger auf das erste Pixel der Bitmap abzurufen:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Für jede bestimmte Zeile und Spalte, ein Offsetwert muss hinzugefügt werden `basePtr`. Dieser Offset ist die Zeile Mal die Breite der Bitmap, plus der Spalte:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

Die `SKColor` Wert im Speicher mithilfe des this-Zeiger gespeichert ist:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

In der `PaintSurface` Handler, der die `SKCanvasView`, die Bitmap wird gestreckt, um den Anzeigebereich ausfüllen:

[![Rainbow Sinus](pixel-bits-images/RainbowSine.png "Rainbow Sinus")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Aus einer einzigen Bitmap in eine andere

Sehr viele bildverarbeitung Aufgaben zur grundlegenden netzwerkprotokollverwaltung Pixel ändern, wie sie aus einer einzigen Bitmap in eine andere übertragen werden. Dieses Verfahren wird veranschaulicht, der **Anpassung** Seite. Die Seite lädt eine Bitmap-Ressource und dann können Sie so ändern Sie das Image mithilfe von drei `Slider` Ansichten:

[![Farbe, die Anpassung](pixel-bits-images/ColorAdjustment.png "Color-Anpassung")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Für jede Pixelfarbe der ersten `Slider` Fügt einen Wert zwischen 0 und 360 liegen in den Farbton, aber dann verwendet der modulo-Operator, um das Ergebnis zwischen 0 und 360 zu halten, effektiv Verschiebung die Farben entlang des Spektrums (wie der UWP-Screenshot veranschaulicht). Die zweite `Slider` können Sie, einen Multiplikationsfaktor zwischen 0,5 und 2 auswählen, um für die Sättigung und das dritte gelten `Slider` führt für die Helligkeit, wie in der Android-Screenshot veranschaulicht.

Das Programm verwaltet zwei Bitmaps, die ursprüngliche Quell-Bitmap, die mit dem Namen `srcBitmap` und die angepasste Zielbitmap mit dem Namen `dstBitmap`. Jedes Mal eine `Slider` verschoben wird, wird die Anwendung berechnet alle neuen Pixel im `dstBitmap`. Natürlich werden Benutzer experimentieren, indem Sie das Verschieben der `Slider` Ansichten sehr schnell, daher sollten Sie die beste Leistung können Sie verwalten. Dies umfasst die `GetPixels` -Methode für den Quell- und Ziel-Bitmaps. 

Die **Anpassung** Seite keine Kontrolle über das Farbformat der Bitmaps, Quelle und Ziel. Stattdessen enthält er leicht abweichender Logik für `SKColorType.Rgba8888` und `SKColorType.Bgra8888` Formate. Die Quelle und Ziel können verschiedene Formate werden, und das Programm weiterhin funktionieren.

Hier ist das Programm mit Ausnahme der entscheidende `TransferPixels` -Methode, die die Pixel überträgt bilden die Quelle zum Ziel. Der Kopierkonstruktor legt `dstBitmap` gleich `srcBitmap`. Die `PaintSurface` Ereignishandler zeigt `dstBitmap`:

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

Die `ValueChanged` Handler für die `Slider` Ansichten berechnet die Anpassungswerte und ruft `TransferPixels`.

Die gesamte `TransferPixels` Methode als gekennzeichnet ist `unsafe`. Er beginnt mit dem Abrufen der Byte-Zeiger auf die Pixelbits der beiden Bitmaps und durchläuft dann, bis alle Zeilen und Spalten. Aus der Quell-Bitmap Ruft die Methode vier Bytes für jedes Pixel ab. Dabei kann es sich entweder in der `Rgba8888` oder `Bgra8888` Reihenfolge. Überprüfen der Farbtyp ermöglicht eine `SKColor` Wert erstellt werden. Die HSL-Komponenten werden dann extrahiert, angepasst und neu erstellt die `SKColor` Wert. Je nachdem, ob die Zielbitmap `Rgba8888` oder `Bgra8888`, die Bytes werden in der Ziel-Bitmp gespeichert:

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

Es ist wahrscheinlich, dass die Leistung dieser Methode kann noch verbessert werden, indem Sie separate Methoden für die verschiedenen Kombinationen von Farbe bitmaptypen der Quelle und Ziel erstellt werden, und zu überprüfen, ob des Typs für jedes Pixel. Eine andere Möglichkeit ist, können nicht mehrere `for` Schleifen für die `col` Variable basierend auf dem Farbe.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
