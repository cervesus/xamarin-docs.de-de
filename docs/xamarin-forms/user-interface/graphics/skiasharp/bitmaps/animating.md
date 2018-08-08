---
title: Animieren von SkiaSharp-bitmaps
description: Erfahren Sie, wie Sie die Bitmap-Animation ausgeführt wird, indem Sie nacheinander anzeigen einer Reihe von Bitmaps und Rendern animierte GIF-Dateien.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: charlespetzold
ms.author: chape
ms.date: 07/12/2018
ms.openlocfilehash: 45a009757d84aa98acc41f6cd2bf672c8472c5bb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615576"
---
# <a name="animating-skiasharp-bitmaps"></a>Animieren von SkiaSharp-bitmaps

Rufen Sie die Anwendungen, die in der Regel Animieren von Grafiken von SkiaSharp `InvalidateSurface` auf die `SKCanvasView` mit einer festen Rate, häufig alle 16 Millisekunden. Die Oberfläche für ungültig zu erklären, startet einen Aufruf an die `PaintSurface` Handler, der die Anzeige neu gezeichnet werden. Wie die visuellen Elemente erneut die 60-Mal pro Sekunde gezeichnet werden, scheinen sie reibungslos animiert werden.

Wenn die Grafik in 16 Millisekunden für den zu rendernden zu komplex sind, kann die Animation jedoch ganz nervös werden. Der Programmierer empfiehlt sich die Aktualisierungsrate auf 30 Mal oder 15 Mal pro Sekunde zu reduzieren, aber manchmal selbst das ist nicht ausreichend. Manchmal sind Grafiken so komplex, dass sie einfach in Echtzeit gerendert werden können.

Eine Lösung besteht im Voraus für die Animation vorbereiten, indem Sie die einzelnen Frames der Animation auf eine Reihe von Bitmaps zu rendern. Wenn die Animation anzeigen möchten, ist nur notwendig, diese Bitmaps sequenziell 60-Mal pro Sekunde angezeigt. 

Natürlich, dies ist möglicherweise viele Bitmaps, aber wie großen Budgets 3D ist animierter Filme erfolgen. Die 3D-Grafiken sind viel zu komplex ist, in Echtzeit gerendert werden soll. Ein Großteil der Verarbeitungszeit ist erforderlich, um jeden Frame zu rendern. Was Sie sehen, wenn Sie den Film ansehen, ist im Wesentlichen eine Reihe von Bitmaps.

Sie können etwas Ähnliches SkiaSharp durchführen. Dieser Artikel veranschaulicht zwei Arten von Bitmap-Animation. Im erste Beispiel wird eine Animation von der Mandelbrot-Menge an:

![Beispiel animieren](animating-images/AnimatingSample.png "Beispiel animieren")

Das zweite Beispiel zeigt, wie SkiaSharp verwenden, um eine animierte GIF-Datei zu rendern.

## <a name="bitmap-animation"></a>Bitmap-animation

Die Mandelbrot-Menge ist visuell faszinierende aber computionally lange. (Eine Erläuterung der Mandelbrot-Menge und die hier verwendete Mathematik, finden Sie unter [Kapitel 20 _Erstellen mobiler Apps mit Xamarin.Forms_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) ab Seite 666. Die folgende Beschreibung geht davon aus, Hintergrundwissen.)

Die [ **Mandelbrot-Animation** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/) Beispiel Bitmap-Animation verwendet, um einen kontinuierlichen Zooms von einer festen Position in der Mandelbrot-Menge zu simulieren. Durch Vergrößern folgt verkleinern, und klicken Sie dann der Zyklus wiederholt, endlos oder bis das Programm zu beenden. 

Die Anwendung bereitet bei dieser Animation erstellt bis zu 50 Bitmaps, die in lokalen Anwendungsspeicher gespeichert wird. Jede Bitmap umfasst die halbe Breite und Höhe der komplexen Ebene, wie die vorherige Bitmap. (Diese Bitmaps gelten als im Programm zur Darstellung von ganzzahligen _Zoomfaktoren_.) Die Bitmaps werden dann nacheinander angezeigt. Die Skalierung der jede Bitmap wird animiert, um eine reibungslose Entwicklung aus einer einzigen Bitmap in eine andere bereitzustellen.

Wie die endgültige Programm in Kapitel 20 beschriebenen _Erstellen mobiler Apps mit Xamarin.Forms_, der Berechnung der Mandelbrot-Menge in **Mandelbrot-Animation** ist eine asynchrone Methode mit acht Parameter. Die Parameter enthalten einen komplexen Mittelpunkt, und eine Breite und Höhe der komplexen Ebene, um die den Mittelpunkt. Die nächsten drei Parameter sind die Breite in Pixel und Höhe der Bitmap erstellt werden und eine maximale Anzahl von Iterationen für die Berechnung der rekursiven. Die `progress` Parameter wird verwendet, um den Status dieser Berechnung angezeigt werden soll. Die `cancelToken` Parameter wird nicht an diesem Programm verwendet:

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

Die Methode gibt ein Objekt vom Typ `BitmapInfo` , Informationen zum Erstellen einer Bitmap bereitstellt:

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

Die **Mandelbrot-Animation** XAML-Datei enthält zwei `Label` Ansichten einer `ProgressBar`, und ein `Button` als auch die `SKCanvasView`:

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand" 
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei beginnt mit dem drei wichtige Konstanten und ein Array von Bitmaps definieren:

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

Irgendwann, Sie sollten so ändern Sie die `COUNT` Wert mit 50, um das gesamte Spektrum der Animation anzuzeigen. Werte über 50 sind nicht nützlich. Um einen Zoomfaktor 48 oder folgt wird die Auflösung von Gleitkommazahlen mit doppelter Genauigkeit für die Berechnung der Mandelbrot-Menge nicht genügend. Dieses Problem wird erläutert, auf der Seite 684 _Erstellen mobiler Apps mit Xamarin.Forms_.

Die `center` Wert ist sehr wichtig. Dies ist der Schwerpunkt für den Zoomfaktor der Animation. Die drei Werte in der Datei werden in den letzten drei Screenshots in Kapitel 20 der verwendeten _Erstellen mobiler Apps mit Xamarin.Forms_ auf der Seite 684, aber Sie können experimentieren, mit dem Programm in diesem Kapitel, um Ihre eigenen Werte. 

Die **Mandelbrot-Animation** Beispiel speichert diese `COUNT` Bitmaps im Speicher des lokalen Anwendung. 50 Bitmaps erfordern mehr als 20 MB Speicher auf Ihrem Gerät aus, daher sollten Sie wissen, wie viel Speicher diese Bitmaps belegt sind, und irgendwann möchten sie alle zu löschen. Das ist der Zweck dieser beiden Methoden am unteren Rand der `MainPage` Klasse:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

Sie können die Bitmaps im lokalen Speicher löschen, während das Programm diese gleichen Bitmaps animiert werden, da das Programm im Arbeitsspeicher beibehalten. Beim nächsten des Programms ausführen, es müssen jedoch die Bitmaps neu zu erstellen.

Die Bitmaps, die im lokalen Anwendungsspeicher gespeichert zu integrieren der `center` Wert im Dateinamen, wenn Sie ändern die `center` festlegen, die vorhandenen Bitmaps werden im Speicher nicht ersetzt werden, und weiterhin Platz einnehmen.

Hier sind die Methoden, `MainPage` verwendet zum Erstellen der Dateinamen sowie einen `MakePixel` Methode zum Definieren von eines Pixelwert auf der Grundlage von Farbe Komponenten:

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() => 
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) => 
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Die `zoomLevel` Parameter `FilePath` liegt zwischen 0 und der `COUNT` Konstanten minus 1.

Die `MainPage` Konstruktor ruft die `LoadAndStartAnimation` Methode:

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

Die `LoadAndStartAnimation` Methode ist verantwortlich für den Zugriff auf lokalen Anwendungsspeicher um Bitmaps zu laden, die erstellt wurden möglicherweise, wenn das Programm zuvor ausgeführt wurde. Es durchläuft `zoomLevel` Werte von 0 bis `COUNT`. Wenn die Datei vorhanden ist, lädt er ihn in das `bitmaps` Array. Andernfalls muss zum Erstellen einer Bitmap für den jeweiligen `center` und `zoomLevel` Werte durch den Aufruf `Mandelbrot.CalculateAsync`. Diese Methode ruft die Anzahl der Iteration für jedes Pixel, das diese Methode in Farben konvertiert:

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation 
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

Beachten Sie, dass das Programm diese Bitmaps im Speicher des lokalen Anwendung nicht in der Fotobibliothek des Geräts speichert. Der .NET Standard 2.0-Bibliothek können mithilfe der vertrauten `File.OpenRead` und `File.WriteAllBytes` Methoden für diese Aufgabe.

Nachdem alle Bitmaps erstellt oder in den Arbeitsspeicher geladen wurden, wird die Methode beginnt eine `Stopwatch` -Objekt und ruft `Device.StartTimer`. Die `OnTimerTick` Methode wird aufgerufen, alle 16 Millisekunden.

`OnTimerTick` berechnet eine `time` Wert in Millisekunden, die von 0 zu 6000 Zeiten bis `COUNT`, die sechs Sekunden für die Anzeige von jede Bitmap weist. Die `progress` Wert verwendet die `Math.Sin` Wert, der um eine sinusförmiger Animation zu erstellen, die am Anfang des Zyklus langsamer werden und am Ende als es langsamer kehrt die Richtung. 

Die `progress` Wert liegt zwischen 0 und `COUNT`. Dies bedeutet, dass den ganzzahlige Teil `progress` ist ein Index in die `bitmaps` Array ist, während der Bruchteil des `progress` einen Zoomfaktor für diese bestimmte Bitmap angibt. Diese Werte werden gespeichert, der `bitmapIndex` und `bitmapProgress` Felder und angezeigt werden, indem die `Label` und `Slider` in der XAML-Datei. Die `SKCanvasView` ungültig wird, um die Bitmap-Anzeige zu aktualisieren:

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

Zum Schluss die `PaintSurface` Handler die `SKCanvasView` berechnet ein Zielrechteck, um die Bitmap so groß wie möglich anzuzeigen, während das Seitenverhältnis beibehalten. Einem Quellrechteck basiert auf der `bitmapProgress` Wert. Die `fraction` Wert berechnet hier reicht von 0, wenn `bitmapProgress` liegt zwischen 0 und Anzeigen der gesamte Bitmap 0,25 wird, wenn `bitmapProgress` ist 1, um die halbe Breite und Höhe der Bitmap, effektiv vergrößern anzuzeigen:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height, 
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

Hier wird das Programm auf allen drei Plattformen ausgeführt wird:

[![Mandelbrot-Animation](animating-images/MandelbrotAnimation.png "Mandelbrot-Animation")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF-animation

Die Graphics Interchange Format (GIF)-Spezifikation umfasst ein Feature, können eine einzelne GIF-Datei kann mehrere sequenzielle Frames einer Szene enthalten, die häufig in einer Schleife nacheinander angezeigt werden können. Diese Dateien werden als bezeichnet _animierte GIF-_. Webbrowser können animierte GIF-Dateien wiedergeben und SkiaSharp ermöglicht einer Anwendung aus, um die Frames aus eine animierte GIF-Datei zu extrahieren und diese sequenziell anzuzeigen.

Die [SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel enthält eine animierte GIF-Ressource, die mit dem Namen **Newtons_cradle_animation_book_2.gif** DemonDeLuxe erstellt und heruntergeladen haben, aus der [Newton auf den Basisstation ](https://en.wikipedia.org/wiki/Newton%27s_cradle) Seite in Wikipedia. Die **animiertes GIF** Seite enthält eine XAML-Datei, enthält diese Informationen und instanziiert, einen `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei ist nicht generalisiert, um alle animierte GIF-Datei wiederzugeben. Sie einige der Informationen, die insbesondere verfügbar ist, wird eine Wiederholungsanzahl und ignoriert einfach die animierte GIF zu den in einer Schleife wiedergegeben wird. 

Die Verwendung von SkisSharp, um die Frames, der eine animierte GIF-Datei zu extrahieren scheint nicht überall und zu dokumentieren, also die Beschreibung des Codes, der folgende ausführlichere als üblich:

Tritt auf, in den Konstruktor der Seite die Decodierung der animierten GIF-Datei, und erfordert, dass die `Stream` Objekt, das auf die Bitmap, die zum Erstellen verwendet werden eine `SKManagedStream` Objekt und klicken Sie dann eine [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) Objekt. Die [ `FrameCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameCount/) Eigenschaft gibt die Anzahl der Frames, aus denen die Animation an. 

Diese Frames werden schließlich als einzelne Bitmaps gespeichert, damit der Konstruktor verwendet `FrameCount` ein Array vom Typ zuordnen `SKBitmap` sowie zwei `int` arrays für die Dauer der einzelnen Videoframes und (die Animationslogik zu verbessern) die kumulierte Dauer.

Die [ `FrameInfo` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameInfo/) Eigenschaft `SKCodec` Klasse ist ein Array von [ `SKCodecFrameInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodecFrameInfo/) Werte, jeweils eines für jeden Frame, aber das einzige, der dieses Programm wird aus dieser Struktur ist die [ `Duration` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodecFrameInfo.Duration/) des Frames, in Millisekunden.

`SKCodec` definiert eine Eigenschaft namens [ `Info` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.Info/) des Typs [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/), aber dieser `SKImageInfo` Wert gibt an (mindestens für dieses Bild), dass die Farbe ist `SKColorType.Index8`, was bedeutet, dass jedes Pixel ist ein Index in einen Typ für die Farbe an. Um zu vermeiden, mit Farbtabellen einzubeziehen, das Programm verwendet die [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Width/) und [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Height/) Informationen aus dieser Struktur zu ihrer Erstellung ist farbig besitzen `ImageInfo` Wert. Jede `SKBitmap` aus, die erstellt wird.

Die `GetPixels` -Methode der `SKBitmap` gibt ein `IntPtr` Pixelbits dieser Bitmap verweisen. Diese Pixelbits wurden noch nicht festgelegt. Dass `IntPtr` übergeben wird, eines der [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCodec.GetPixels/p/SkiaSharp.SKImageInfo/System.IntPtr/SkiaSharp.SKCodecOptions/) Methoden `SKCodec`. Diese Methode kopiert den Frame aus der GIF-Datei in den Speicherbereich verweist die `IntPtr`. Die [ `SKCodecOptions` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCodecOptions.SKCodecOptions/p/System.Int32/System.Boolean/) Konstruktor gibt an, die Framenummer:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations 
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] + 
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

Trotz der `IntPtr` Wert Nein `unsafe` Code ist erforderlich, da die `IntPtr` nie auf einen Zeigerwert c# konvertiert wird.

Nach jeder Frame extrahiert wurde, wird der Konstruktor summiert, um die Dauer von alle Frames, und initialisiert dann ein anderes Array mit einer der kumulierte Dauer.

Der Rest des Code-Behind-Datei ist für die Animation vorgesehen. Die `Device.StartTimer` Methode dient zum Starten eines Timers wechseln, und die `OnTimerTick` Rückruf verwendet eine `Stopwatch` Objekt, um zu bestimmen, die verstrichene Zeit in Millisekunden. Durchlaufen von Arrays kumulierte Dauer ist ausreichend, um den aktuellen Frame finden:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);
            
        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

Jedes Mal die `currentframe` Änderungen an, die `SKCanvasView` für ungültig erklärt und der neue Rahmen angezeigt wird:

[![Animierte GIF-Datei](animating-images/AnimatedGif.png "animierte GIF-Datei")](animating-images/AnimatedGif-Large.png#lightbox)

Natürlich möchten Sie das Programm ausführen selbst zum Anzeigen der Animation.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Mandelbrot-Animation (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/)
