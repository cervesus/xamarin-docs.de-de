---
title: Animieren von skiasharp-Bitmaps
description: Erfahren Sie, wie Sie eine bitmapanimation durchführen, indem Sie eine Reihe von Bitmaps sequenziell anzeigen und animierte GIF-Dateien rendern.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 763f44c26d653aa32429b2aa764989e18e8b8078
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139970"
---
# <a name="animating-skiasharp-bitmaps"></a>Animieren von skiasharp-Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Anwendungen, die skiasharp-Grafiken animieren `InvalidateSurface` , werden in der Regel mit `SKCanvasView` einer festgelegten Rate aufgerufen, häufig alle 16 Millisekunden. Wenn Sie die Oberfläche ungültig machen, wird ein Aufrufen des `PaintSurface` Handlers ausgelöst, um die Anzeige neu zu zeichnen. Wenn die visuellen Elemente 60 mal pro Sekunde neu gezeichnet werden, scheinen Sie nahtlos animiert zu werden.

Wenn die Grafiken jedoch zu komplex sind, um in 16 Millisekunden gerendert zu werden, kann die Animation Jittery werden. Der Programmierer kann die Aktualisierungsrate auf das 30-fache oder das 15-fache reduzieren, manchmal aber auch das nicht genug. Manchmal sind Grafiken so komplex, dass Sie einfach nicht in Echtzeit gerendert werden können.

Eine Lösung besteht darin, die Animation vorher vorzubereiten, indem die einzelnen Frames der Animation für eine Reihe von Bitmaps gerendert werden. Zum Anzeigen der Animation ist es nur notwendig, diese Bitmaps sequenziell 60 mal pro Sekunde anzuzeigen.

Natürlich ist das möglicherweise eine Menge von Bitmaps, aber das ist, wie groß-Budget-3D-animierte Filme gemacht werden. Die 3D-Grafiken sind viel zu komplex, um in Echtzeit gerendert zu werden. Zum Rendering der einzelnen Frames ist viel Verarbeitungszeit erforderlich. Was Sie sehen, wenn Sie den Film sehen, ist im Wesentlichen eine Reihe von Bitmaps.

Sie können in skiasharp etwas ähnliches tun. In diesem Artikel werden zwei Arten der bitmapanimation veranschaulicht. Das erste Beispiel ist eine Animation der Mandelbrot-Menge:

![Animieren von Beispielen](animating-images/AnimatingSample.png "Animieren von Beispielen")

Im zweiten Beispiel wird gezeigt, wie Sie skiasharp zum Rendering einer animierten GIF-Datei verwenden.

## <a name="bitmap-animation"></a>Bitmapanimation

Die Mandelbrot-Menge ist visuell faszinierend, aber Rechen mäßig langwierig. (Eine Erläuterung der Mandelbrot-Menge und der hier verwendeten Mathematik finden Sie in [Kapitel 20 der _Erstellung von Mobile Apps mit xamarin. Forms beginnend mit_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) Seite 666. In der folgenden Beschreibung wird davon ausgegangen, dass Hintergrundwissen.)

Im Beispiel für die [**Mandelbrot-Animation**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima) wird die bitmapanimation verwendet, um einen kontinuierlichen Zoom eines festgelegten Punkts in der Mandelbrot-Menge zu simulieren. Beim Zoomen wird ein Zoom durchgeführt, und dann wird der-Schleifen Wert immer wiederholt, oder bis Sie das Programm beenden.

Das Programm bereitet diese Animation vor, indem bis zu 50 Bitmaps erstellt werden, die im lokalen Anwendungs Speicher gespeichert werden. Jede Bitmap umfasst die Hälfte der Breite und Höhe der komplexen Ebene als vorherige Bitmap. (Im Programm werden diese Bitmaps als ganzzahlige _Zoomwerte_bezeichnet.) Die Bitmaps werden dann nacheinander angezeigt. Die Skalierung der einzelnen Bitmap ist animiert, um einen reibungslosen Übergang von einer Bitmap zu einer anderen zu ermöglichen.

Wie das endgültige Programm, das in Kapitel 20 der _Erstellung von Mobile Apps mit xamarin. Forms_beschrieben wird, ist die Berechnung der Mandelbrot-Menge in der **Mandelbrot-Animation** eine asynchrone Methode mit acht Parametern. Die Parameter umfassen einen komplexen Mittelpunkt und eine Breite und Höhe der komplexen Ebene, die den Mittelpunkt umgibt. Die nächsten drei Parameter sind die Pixel Breite und-Höhe der zu erstellenden Bitmap und eine maximale Anzahl von Iterationen für die rekursive Berechnung. Der- `progress` Parameter wird verwendet, um den Fortschritt dieser Berechnung anzuzeigen. Der- `cancelToken` Parameter wird in diesem Programm nicht verwendet:

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

Die-Methode gibt ein Objekt vom Typ zurück `BitmapInfo` , das Informationen zum Erstellen einer Bitmap bereitstellt:

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

Die XAML-Datei der **Mandelbrot-Animation** enthält zwei `Label` Ansichten `ProgressBar` : a und sowie `Button` die `SKCanvasView` :

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

Die Code-Behind-Datei beginnt mit der Definition von drei entscheidenden Konstanten und einem Array von Bitmaps:

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

Zu einem bestimmten Zeitpunkt möchten Sie wahrscheinlich den `COUNT` Wert in 50 ändern, um den vollständigen Bereich der Animation anzuzeigen. Werte oberhalb von 50 sind nicht nützlich. Um einen Zoomfaktor von 48 oder so wird die Auflösung von Gleit Komma Zahlen mit doppelter Genauigkeit für die Berechnung der Mandelbrot-Menge unzureichend. Dieses Problem wird auf Seite 684 der _Erstellung von Mobile Apps mit xamarin. Forms_erläutert.

Der `center` Wert ist sehr wichtig. Dies ist der Schwerpunkt des Animations Zoom. Die drei Werte in der Datei werden in den drei abschließenden Screenshots in Kapitel 20 der Erstellung von _Mobile Apps mit xamarin. Forms_ auf Seite 684 verwendet, aber Sie können mit dem Programm in diesem Kapitel Experimentieren, um einen ihrer eigenen Werte zu erhalten.

Das Beispiel für eine **Mandelbrot-Animation** speichert diese `COUNT` Bitmaps im lokalen Anwendungs Speicher. 50 Bitmaps erfordern mehr als 20 MB Speicherplatz auf Ihrem Gerät. Sie sollten also wissen, wie viel Speicher diese Bitmaps belegen und an einem beliebigen Punkt alle löschen möchten. Dies ist der Zweck dieser beiden Methoden am Ende der `MainPage` Klasse:

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

Sie können die Bitmaps im lokalen Speicher löschen, während das Programm die gleichen Bitmaps animiert, da Sie vom Programm im Arbeitsspeicher beibehalten werden. Wenn Sie das Programm das nächste Mal ausführen, müssen Sie jedoch die Bitmaps neu erstellen.

Die im lokalen Anwendungs Speicher gespeicherten Bitmaps enthalten den `center` Wert in ihren Dateinamen. Wenn Sie also die `center` Einstellung ändern, werden die vorhandenen Bitmaps nicht im Speicher ersetzt und belegen weiterhin Speicherplatz.

Im folgenden finden Sie die Methoden, die `MainPage` zum Erstellen der Dateinamen und eine `MakePixel` Methode zum Definieren eines Pixel Werts auf der Grundlage von Farbkomponenten verwenden:

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

Der `zoomLevel` Parameter für `FilePath` reicht von 0 bis zur `COUNT` Konstanten minus 1.

Der `MainPage` Konstruktor ruft die- `LoadAndStartAnimation` Methode auf:

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

Die- `LoadAndStartAnimation` Methode ist für den Zugriff auf den lokalen Anwendungs Speicher verantwortlich, um Bitmaps zu laden, die möglicherweise beim Ausführen des Programms zuvor erstellt wurden. Sie durchläuft `zoomLevel` die Werte zwischen 0 und `COUNT` . Wenn die Datei vorhanden ist, wird Sie in das `bitmaps` Array geladen. Andernfalls muss eine Bitmap für die speziellen `center` `zoomLevel` Werte und durch Aufrufen von erstellt werden `Mandelbrot.CalculateAsync` . Diese Methode ruft die Iterations Zähler für jedes Pixel ab, das von dieser Methode in Farben konvertiert wird:

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

Beachten Sie, dass das Programm diese Bitmaps nicht in der Fotobibliothek des Geräts, sondern im Speicher lokaler Anwendungen speichert. Die Bibliothek .NET Standard 2,0 ermöglicht die Verwendung der vertrauten `File.OpenRead` -Methode und der- `File.WriteAllBytes` Methode für diese Aufgabe.

Nachdem alle Bitmaps erstellt oder in den Arbeitsspeicher geladen wurden, startet die-Methode ein `Stopwatch` -Objekt und ruft auf `Device.StartTimer` . Die- `OnTimerTick` Methode wird alle 16 Millisekunden aufgerufen.

`OnTimerTick`berechnet einen `time` Wert in Millisekunden, der zwischen 0 und 6000 Mal `COUNT` liegt, wobei sechs Sekunden für die Anzeige der einzelnen Bitmap zugeordnet werden. Der `progress` Wert verwendet den `Math.Sin` Wert, um eine Sinus förmigem-Animation zu erstellen, die am Anfang des Zyklen langsamer ist und am Ende langsamer ist, wenn die Richtung umgekehrt wird.

Der `progress` Wert liegt zwischen 0 und `COUNT` . Dies bedeutet, dass der ganzzahlige Teil von `progress` ein Index in das `bitmaps` Array ist, während der Bruchteile von `progress` eine Zoomstufe für diese bestimmte Bitmap angibt. Diese Werte werden in den `bitmapIndex` Feldern und gespeichert `bitmapProgress` und werden von `Label` und `Slider` in der XAML-Datei angezeigt. Der `SKCanvasView` wird für die Aktualisierung der bitmapanzeige ungültig:

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

Schließlich berechnet der- `PaintSurface` Handler `SKCanvasView` ein Ziel Rechteck, um die Bitmap so groß wie möglich anzuzeigen, während das Seitenverhältnis beibehalten wird. Ein Quell Rechteck basiert auf dem `bitmapProgress` Wert. Der `fraction` hier berechnete Wert liegt zwischen 0 und `bitmapProgress` dem Wert 0, um die gesamte Bitmap anzuzeigen, bis 0,25, wenn `bitmapProgress` 1 ist, um die Hälfte der Breite und Höhe der Bitmap anzuzeigen, wodurch die Größe und die Höhe der Bitmap vergrößert werden:

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

Dies ist das Programm, das ausgeführt wird:

[![Mandelbrot-Animation](animating-images/MandelbrotAnimation.png "Mandelbrot-Animation")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF-Animation

Die Graphics Interchange Format (GIF)-Spezifikation enthält ein Feature, das es einer einzelnen GIF-Datei ermöglicht, mehrere sequenzielle Frames einer Szene zu enthalten, die nacheinander angezeigt werden können, häufig in einer-Schleife. Diese Dateien werden als _animierte GIFs_bezeichnet. Webbrowser können animierte GIFs wiedergeben, und skiasharp ermöglicht es einer Anwendung, die Frames aus einer animierten GIF-Datei zu extrahieren und nacheinander anzuzeigen.

Das [skiasharpformsdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel enthält eine animierte GIF-Ressource mit dem Namen **Newtons_cradle_animation_book_2.gif** , die von dämondeluxe erstellt und von der Seite mit der Seite für [Newton](https://en.wikipedia.org/wiki/Newton%27s_cradle) in Wikipedia heruntergeladen wurde. Die **animierte GIF** -Seite enthält eine XAML-Datei, die diese Informationen bereitstellt und eine instanziiert `SKCanvasView` :

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

Die Code-Behind-Datei wird nicht generalisiert, um eine animierte GIF-Datei wiederzugeben. Einige der verfügbaren Informationen werden ignoriert, insbesondere eine Wiederholungs Anzahl, und das animierte GIF wird einfach in einer Schleife wiedergegeben.

Die Verwendung von skissharp zum Extrahieren der Rahmen einer animierten GIF-Datei scheint nicht überall dokumentiert zu werden, sodass die Beschreibung des folgenden Codes ausführlicher als üblich ist:

Das Decodieren der animierten GIF-Datei erfolgt im Konstruktor der Seite und erfordert, dass das `Stream` Objekt, das auf die Bitmap verweist, verwendet wird, um ein Objekt zu erstellen, `SKManagedStream` und dann ein- [`SKCodec`](xref:SkiaSharp.SKCodec) Objekt. Die- [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount) Eigenschaft gibt die Anzahl der Frames an, aus denen die Animation besteht.

Diese Frames werden schließlich als einzelne Bitmaps gespeichert, sodass der Konstruktor verwendet, `FrameCount` um ein Array vom Typ sowie `SKBitmap` zwei `int` Arrays für die Dauer der einzelnen Frames und (zur Erleichterung der Animations Logik) der akkumulierten Dauer zuzuordnen.

Die- [`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo) Eigenschaft der- `SKCodec` Klasse ist ein Array von [`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo) Werten, eines für jeden Frame, aber das einzige, was dieses Programm von dieser Struktur annimmt, ist das [`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) des Frames in Millisekunden.

`SKCodec`definiert eine Eigenschaft [`Info`](xref:SkiaSharp.SKCodec.Info) mit dem Namen vom Typ [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) , aber dieser `SKImageInfo` Wert gibt (zumindest für dieses Bild) an, dass der Farbtyp ist `SKColorType.Index8` . das bedeutet, dass jedes Pixel ein Index in einen Farbtyp ist. Um das Zutun mit Farbtabellen zu vermeiden, verwendet das Programm die [`Width`](xref:SkiaSharp.SKImageInfo.Width) [`Height`](xref:SkiaSharp.SKImageInfo.Height) Informationen und aus dieser Struktur, um den eigenen voll `ImageInfo` Farbwert zu erstellen. Jede `SKBitmap` wird aus dieser erstellt.

Die- `GetPixels` Methode von `SKBitmap` gibt einen zurück, der auf `IntPtr` die Pixel Bits dieser Bitmap verweist. Diese Pixel Bits wurden noch nicht festgelegt. `IntPtr`An eine der [`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) Methoden von `SKCodec` . Diese Methode kopiert den Frame aus der GIF-Datei in den Speicherplatz, auf den von verwiesen wird `IntPtr` . Der [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions) Konstruktor gibt die Frame Nummer an:

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

Trotz des- `IntPtr` Werts `unsafe` ist kein Code erforderlich, da der `IntPtr` niemals in einen c#-Zeiger Wert konvertiert wird.

Nachdem jeder Frame extrahiert wurde, summiert der Konstruktor die Dauer aller Frames und initialisiert dann ein anderes Array mit der akkumulierten Dauer.

Der Rest der Code Behind-Datei ist für die Animation dediziert. Die `Device.StartTimer` -Methode wird verwendet, um einen Timer zu starten, und der `OnTimerTick` Rückruf verwendet ein- `Stopwatch` Objekt, um die verstrichene Zeit in Millisekunden zu bestimmen. Das Durchlaufen des akkumulierten laufzeitarrays reicht aus, um den aktuellen Frame zu finden:

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

Jedes Mal, wenn die `currentframe` Variable geändert wird, `SKCanvasView` wird der ungültig, und der neue Frame wird angezeigt:

[![Animiertes GIF](animating-images/AnimatedGif.png "Animiertes GIF")](animating-images/AnimatedGif-Large.png#lightbox)

Natürlich möchten Sie das Programm selbst ausführen, um die Animation zu sehen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Mandelbrot-Animation (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
