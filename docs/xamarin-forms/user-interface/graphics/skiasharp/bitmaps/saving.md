---
title: Speichern von SkiaSharp-Bitmaps in Dateien
description: Untersuchen Sie die verschiedenen Dateiformaten von SkiaSharp für das Speichern von Bitmaps in der Fotobibliothek des Benutzers unterstützt wird.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 4414ce498bdf69e82269137c35af8f27b9e5f541
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649565"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>Speichern von SkiaSharp-Bitmaps in Dateien

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Nach eine SkiaSharp-Anwendung erstellt oder eine Bitmap geändert haben, kann die Anwendung die Bitmap auf Fotobibliothek des Benutzers speichern möchten:

![Speichern von Bitmaps](saving-images/SavingSample.png "Speichern von Bitmaps")

Diese Aufgabe umfasst zwei Schritte:

- Konvertieren die SkiaSharp-Bitmap, auf Daten in einem bestimmten Dateiformat, z. B. JPEG oder PNG.
- Das Ergebnis an die Fotobibliothek, die mithilfe von plattformspezifischem Code gespeichert.

## <a name="file-formats-and-codecs"></a>Dateiformaten und codecs

Die meisten heutigen beliebten Bitmap-Formate verwenden dateikomprimierung, Speicherplatz zu reduzieren. Die zwei allgemeine Kategorien von Komprimierungstechniken heißen _verlustbehaftete_ und _verlustfreie_. Diese Begriffe Geben Sie an, und zwar unabhängig davon, ob der Komprimierungsalgorithmus zu Datenverlust führt.

Das am häufigsten verwendete verlustbehaftete Format wurde von der Joint Photographic Experts Group entwickelt und JPEG aufgerufen wird. Der Komprimierungsalgorithmus JPEG analysiert das Image mit dem mathematischen Tool die _diskrete Kosinustransformation_, und versucht, Daten zu entfernen, die nicht beibehalten des Bilds Wiedergabetreue von entscheidender Bedeutung ist. Der Grad der Komprimierung kann gesteuert werden, mit der Einstellung in der Regel als bezeichnet _Qualität_. Einstellungen für höhere Qualität führen zu größeren Dateien.

Im Gegensatz dazu werden ein Algorithmus für verlustfreie Komprimierung das Bild für Wiederholung und Muster der Pixel, die auf eine Weise codiert werden können, die die Daten reduziert, aber führt nicht zum Verlust von Informationen analysiert. Die ursprüngliche Bitmap-Daten können vollständig aus der komprimierten Datei wiederhergestellt werden. Das primäre verlustfreie komprimierte Datei in verwendete Format ist heute Portable Network Graphics (PNG).

Im Allgemeinen wird die JPEG für Fotografien, verwendet, während PNG für Images verwendet wird, die manuell oder über einen Algorithmus generiert wurden. Alle verlustfreie Komprimierung-Algorithmus, der die Größe der Dateien verringert wird, muss unbedingt die Größe der anderen erhöhen. Glücklicherweise tritt auf, Anstieg der Größe in der Regel nur für Daten, die eine Vielzahl von Informationen für zufällige (oder scheinbar willkürlichen) enthält.

Die Komprimierung, die Algorithmen sind komplex genug, um zu garantieren, dass zwei, die Bedingungen beschrieben, die Komprimierung und dekomprimierung Prozesse:

- _Decodieren von_ &mdash; ein Bitmap-Dateiformat zu lesen und Dekomprimieren Sie sie
- _Codieren_ &mdash; die Bitmap zu komprimieren, und Schreiben in ein Bitmap-Dateiformat

Die [ `SKBitmap` ](xref:SkiaSharp.SKBitmap) -Klasse enthält mehrere Methoden, die mit dem Namen `Decode` erstellen, eine `SKBitmap` aus einer komprimierten Quelle. Alles, was erforderlich ist, ist ein Dateiname, Stream oder Bytearray angeben. Der Decoder kann bestimmen das Dateiformat und an die richtige interne Decodierung-Funktion übergeben.

Darüber hinaus die [ `SKCodec` ](xref:SkiaSharp.SKCodec) -Klasse verfügt über zwei Methoden namens `Create` erstellen kann ein `SKCodec` -Objekt aus einer komprimierten Quelldateien und damit eine Anwendung in die Decodierung komplizierter zu erhalten. (Die `SKCodec` ist in diesem Artikel dargestellt [ **Animieren von SkiaSharp-Bitmaps** ](animating.md#gif-animation) im Zusammenhang mit der Decodierung einer animierten GIF-Datei.)

Beim Codieren einer Bitmap sind weitere Informationen erforderlich: Der Encoder muss das jeweilige Dateiformat kennen, das von der Anwendung verwendet werden soll (JPEG oder PNG oder etwas anderes). Wenn ein verlustreiches Format gewünscht ist, muss die Codierung auch das gewünschte Maß an Qualität kennen.

Die `SKBitmap` Klasse definiert eine [ `Encode` ](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) Methode mit der folgenden Syntax:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

Diese Methode wird in Kürze ausführlicher beschrieben. Die codierten Bitmaps wird in einen nicht schreibgeschützten Stream geschrieben. (In der 'W' `SKWStream` steht für "beschreibbar".) Das zweite und dritte Argument angeben, das Dateiformat und (für verlustbehafteten Formaten) die gewünschte Qualität, die im Bereich von 0 bis 100.

Darüber hinaus die [ `SKImage` ](xref:SkiaSharp.SKImage) und [ `SKPixmap` ](xref:SkiaSharp.SKPixmap) Klassen auch definieren `Encode` Methoden, die etwas flexibler sind, und die durch das Verlagen. Können Sie problemlos erstellen eine `SKImage` -Objekt aus einer `SKBitmap` mithilfe der statischen [ `SKImage.FromBitmap` ](xref:SkiaSharp.SKImage.FromBitmap(SkiaSharp.SKBitmap)) Methode. Sie erhalten eine `SKPixmap` -Objekt aus einer `SKBitmap` -Objekt unter Verwendung der [ `PeekPixels` ](xref:SkiaSharp.SKBitmap.PeekPixels) Methode.

Eines der [ `Encode` ](xref:SkiaSharp.SKImage.Encode) definierten Methoden `SKImage` weist keine Parameter auf und speichert automatisch in ein PNG-Format. Diese parameterlosen Methode ist sehr einfach zu verwenden.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>Plattformspezifischen Code zum Speichern von Bitmap-Dateien

Wenn Sie Codieren einer `SKBitmap` -Objekt in eine bestimmte Datei zu formatieren, in der Regel werden Sie mit irgend einem Stream-Objekt oder ein Array von Daten ausgelassen werden. Einige der `Encode` Methoden (einschließlich der App, die keine Parameter definiert, indem `SKImage`) zurückgeben eine [ `SKData` ](xref:SkiaSharp.SKData) -Objekt, das in ein Array von Bytes, die mit konvertiert werden kann die [ `ToArray` ](xref:SkiaSharp.SKData.ToArray) Methode. Diese Daten müssen dann in einer Datei gespeichert werden.

Speichern in einer Datei im lokalen Anwendungsspeicher ist recht einfach, da es sich bei Verwendung von Standard `System.IO` Klassen und Methoden für diese Aufgabe. Dieses Verfahren wird veranschaulicht, in diesem Artikel [ **Animieren von SkiaSharp-Bitmaps** ](animating.md#bitmap-animation) in Verbindung mit einer Reihe von Bitmaps aus der Mandelbrot-Menge animieren.

Wenn Sie die Datei von anderen Anwendungen gemeinsam verwendet werden soll, muss es auf Fotobibliothek des Benutzers gespeichert werden. Diese Aufgabe ist erforderlich, plattformspezifischen Code und die Verwendung von der Xamarin.Forms [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

Die **SkiaSharpFormsDemo** -Projekt in der [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Anwendung definiert eine `IPhotoLibrary` Schnittstelle, die verwendet werden, mit der `DependencyService` Klasse. Dies definiert die Syntax einer `SavePhotoAsync` Methode:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

Diese Schnittstelle definiert auch die `PickPhotoAsync` -Methode, die zum Öffnen der Dateiauswahl plattformspezifische, für das Gerät die fotomediathek verwendet wird.

Für `SavePhotoAsync`, das erste Argument ist ein Array von Bytes, die enthält das Bitmuster, die bereits in einem bestimmten Dateiformat, z. B. JPEG oder PNG-Datei codiert. Es ist möglich, dass eine Anwendung sollen alle Bitmaps zu isolieren, die sie in einem bestimmten Ordner erstellt, der in der nächste Parameter, gefolgt vom Namen Datei angegeben ist. Die Methode gibt einen booleschen Wert, der angibt, Erfolg, oder nicht.

Den folgenden Abschnitten wird erläutert, wie `SavePhotoAsync` auf jeder Plattform implementiert wird.

### <a name="the-ios-implementation"></a>Die iOS-Implementierung

Die iOS-Implementierung von `SavePhotoAsync` verwendet die [ `SaveToPhotosAlbum` ](xref:UIKit.UIImage.SaveToPhotosAlbum*) -Methode der `UIImage`:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

Leider besteht keine Möglichkeit, einen Dateinamen oder einen Ordner für das Image angeben.

Die **"Info.plist"** -Datei in das iOS-Projekt erfordert einen Schlüssel, der angibt, dass der Fotobibliothek Bilder hinzugefügt:

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

Vorsicht! Der Schlüssel der Berechtigung für den Zugriff einfach auf der Fotobibliothek ist sehr ähnlich, jedoch nicht identisch:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Die Android-Implementierung

Die Android-Implementierung von `SavePhotoAsync` überprüft zuerst, ob die `folder` Argument `null` oder eine leere Zeichenfolge. Wenn dies der Fall ist, wird die Bitmap im Stammverzeichnis der Fotobibliothek gespeichert. Andernfalls wird der Ordner abgerufen, und wenn es nicht vorhanden ist, wird es erstellt:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Der Aufruf von `MediaScannerConnection.ScanFile` ist nicht zwingend erforderlich, wenn Sie das Programm sofort Überprüfen der Fotobibliothek testen sind, wird dadurch jedoch viel durch Aktualisieren der Ansicht der Katalog.

Die **"androidmanifest.xml"** Datei muss die folgende Berechtigung-Tag:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>Die UWP-Implementierung

Die UWP-Implementierung von `SavePhotoAsync` ist in der Struktur der Android-Implementierung sehr ähnlich:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Die **Funktionen** Teil der **"Package.appxmanifest"** Konfigurationsincludedatei **Bildbibliothek**.

## <a name="exploring-the-image-formats"></a>Untersuchen die Bildformate

Hier ist die [ `Encode` ](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) -Methode der `SKImage` erneut:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](xref:SkiaSharp.SKEncodedImageFormat) ist eine Enumeration mit Elementen, die in elf Bitmap-Dateiformate, verweisen, von die einige stattdessen unklar sind:

- `Astc` &mdash; Adaptive skalierbare Texturkomprimierung
- `Bmp` &mdash; Windows-Bitmap
- `Dng` &mdash; Digitalen Adobe-negativ
- `Gif` &mdash; Graphics Interchange Format
- `Ico` &mdash; Symbolbilder für Windows
- `Jpeg` &mdash; Joint Photographic Experts Group
- `Ktx` &mdash; Khronos Texturformat für OpenGL
- `Pkm` &mdash; Pokémon-Datei speichern
- `Png` &mdash; Portable Network Graphics
- `Wbmp` &mdash; Drahtlose Anwendung Protokoll Bitmap-Format (1 Bit pro Pixel)
- `Webp` &mdash; Google WebP-format

Wie Sie bald sehen werden, nur drei der folgenden Dateiformate (`Jpeg`, `Png`, und `Webp`) tatsächlich von SkiaSharp unterstützt werden.

Speichern einer `SKBitmap` Objekt mit dem Namen `bitmap` auf Fotobibliothek des Benutzers, benötigen Sie auch ein Mitglied der `SKEncodedImageFormat` Enumeration namens `imageFormat` und (für verlustbehafteten Formaten) eine ganze Zahl `quality` Variable. Sie können den folgenden Code zum Speichern dieser Bitmap in eine Datei mit dem Namen `filename` in die `folder` Ordner:

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

Die `SKManagedWStream` Klasse leitet sich von `SKWStream` (das steht für "ein schreibbarer Stream"). Die `Encode` Methode schreibt die codierten Bitmaps-Datei in den Stream. Die Kommentare im Code finden Sie eine fehlerprüfung auf, dass Sie möglicherweise durchführen müssen.

Die **-Dateiformate speichern** auf der Seite die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Anwendung ähnlichen Code verwendet, um Ihnen das Experimentieren mit eine Bitmap in verschiedenen Formaten speichern zu ermöglichen.

Die XAML-Datei enthält eine `SKCanvasView` , die eine Bitmap anzeigt, während der Rest der Seite, die alles enthält, muss die Anwendung zum Aufrufen der `Encode` -Methode der `SKBitmap`. Hat eine `Picker` für ein Mitglied der `SKEncodedImageFormat` -Enumeration ein `Slider` für das Qualitätsargument für verlustbehaftete Bitmapformate, zwei `Entry` Ansichten für einen Dateinamen und den Ordner ein, und ein `Button` zum Speichern der Datei.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save"
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei lädt eine Bitmapressource und verwendet die `SKCanvasView` angezeigt. Die bitmap nie geändert wird. Die `SelectedIndexChanged` Handler für die `Picker` ändert den Dateinamen mit der Erweiterung, die der Enumerationsmember identisch ist:

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

Die `Clicked` Handler für die `Button` alle die tatsächlichen funktioniert. Er erhält zwei Argumente auf `Encode` aus der `Picker` und `Slider`, und verwendet dann den bereits gezeigten Codeabschnitt zum Erstellen einer `SKManagedWStream` für die `Encode` Methode. Die beiden `Entry` Ansichten Dokumentationselement Ordner und Dateinamen für die `SavePhotoAsync` Methode.

Die meisten dieser Methode geht es um die Behandlung von Problemen oder Fehlern. Wenn `Encode` erstellt ein leeres Array, dies bedeutet, dass das bestimmten Dateiformat nicht unterstützt wird. Wenn `SavePhotoAsync` gibt `false`, und klicken Sie dann die Datei erfolgreich gespeichert wurde nicht.

Hier wird das Programm ausgeführt wird:

[![Speichern von Dateiformaten](saving-images/SaveFileFormats.png "-Dateiformate speichern")](saving-images/SaveFileFormats-Large.png#lightbox)

Dieser Screenshot zeigt, die nur drei Formate, die auf diesen Plattformen unterstützt werden:

- JPEG
- PNG
- WebP

Für alle anderen Formate die `Encode` Methode schreibt "nothing" in den Stream und das resultierende Byte-Array ist leer.

Die Bitmap, die die **Dateiformate speichern** Seite speichert ist 600-Pixel im Quadrat. Mit 4 Bytes pro Pixel ist, die insgesamt 1,440,000 Bytes im Arbeitsspeicher. Die folgende Tabelle zeigt die Dateigröße für verschiedene Kombinationen von Dateiformat und die Qualität:

|Format|Qualität|Größe|
|------|------:|---:|
| PNG | Nicht zutreffend | 492K |
| JPEG | 0 | 2,95 K |
|      | 50 | 22.1 K |
|      | 100 | 206K |
| WebP | 0 | 2.71K |
|      | 50 | 11.9 K |
|      | 100 | 101 KB |

Sie können experimentieren Sie mit verschiedenen Einstellungen von Quality und überprüfen Sie die Ergebnisse.

## <a name="saving-finger-paint-art"></a>Speichern von Finger-paint Kunst

Eine häufige Verwendung einer Bitmap beim Zeichnen von Programmen, funktioniert es, in dem wie einem so genannten ist eine _Schatten Bitmap_. Auf die Bitmap, die dann von der Anwendung angezeigt werden, werden alle die Zeichnung beibehalten. Die Bitmap ist auch praktisch zum Speichern der Zeichnung.

Die [ **Zeichnen mit Fingern in SkiaSharp** ](../paths/finger-paint.md) Artikel wurde veranschaulicht, wie zur Implementierung eines primitiven multitoucheingaben Programms nachverfolgen Toucheingabe zu verwenden. Die Anwendung unterstützt nur eine Farbe und nur eine Strichbreite bleiben jedoch erhalten sie die gesamte Zeichnen in einer Auflistung von `SKPath` Objekte.

Die **Fingerpaint mit speichern** auf der Seite die [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Beispiel auch behält das gesamte Zeichnen in einer Auflistung von `SKPath` Objekte aufweist, aber auch Rendert die Zeichnung auf eine Bitmap, die sie auf Ihre Fotobibliothek speichern können.

Ein Großteil dieses Programm ähnelt der ursprünglichen **Fingerpaint** Programm. Eine Verbesserung ist, dass die XAML-Datei nun Schaltflächen, die mit der Bezeichnung instanziiert **löschen** und **speichern**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei, verwaltet ein Feld vom Typ `SKBitmap` mit dem Namen `saveBitmap`. Diese Bitmap erstellt oder neu erstellt, der `PaintSurface` Handler auf, wenn die Größe der Anzeige auftreten von Änderungen. Wenn die Bitmap neu erstellt werden muss, werden den Inhalt der vorhandenen Bitmap auf die neue Bitmap kopiert, so dass alle Elemente beibehalten wird, unabhängig davon, wie die Anzeigeoberfläche Größe ändert:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

Die Zeichnung erreicht, indem die `PaintSurface` Handler tritt auf, ganz am Ende und besteht ausschließlich aus die Bitmap rendern.

Die Touch-Verarbeitung ist ähnlich dem früheren Programm. Das Programm verwaltet zwei Auflistungen, `inProgressPaths` und `completedPaths`, enthalten alle Elemente, die der Benutzer wurde seit dem letzten, die die Anzeige wurde gelöscht. Für die einzelnen Touch-Ereignisse die `OnTouchEffectAction` handleraufrufen `UpdateBitmap`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

Die `UpdateBitmap` Methode neuzeichnungen `saveBitmap` durch Erstellen eines neuen `SKCanvas`, löschen Sie ihn, und das anschließende Rendern alle Pfade in der Bitmap. Schließlich werden für ungültig zu erklären `canvasView` , damit auf die Anzeige die Bitmap gezeichnet werden kann.

Hier sind die Ereignishandler für die zwei Schaltflächen. Die **löschen** Schaltfläche löscht beide Sammlungen Pfad Updates `saveBitmap` (führt zu die Bitmap löschen), und erklärt die `SKCanvasView`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

Die **speichern** Schaltflächenhandler verwendet die vereinfachte [ `Encode` ](xref:SkiaSharp.SKImage.Encode) aus `SKImage`. Diese Methode codiert das PNG-Format verwenden. Die `SKImage` Objekt wird basierend auf erstellt `saveBitmap`, und die `SKData` Objekt enthält die codierte PNG-Datei.

Die `ToArray` -Methode der `SKData` Ruft ein Array von Bytes. Dies ist das übergeben wird die `SavePhotoAsync` -Methode, zusammen mit einer festen Ordnernamen und einen eindeutigen Dateinamen aus dem aktuellen Datum und Uhrzeit erstellt.

So sieht die Anwendung in Aktion:

[![Finger Paint speichern](saving-images/FingerPaintSave.png "Finger Paint speichern")](saving-images/FingerPaintSave-Large.png#lightbox)

Ein sehr ähnliches Verfahren wird verwendet, der [ **SpinPaint** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint) Beispiel. Dies ist auch ein multitoucheingaben Programm, mit dem Unterschied, dass der Benutzer auf einer rotierenden Festplatte zeichnet, die Sie dann die Entwürfe auf der anderen vier Quadranten reproduziert. Dreht sich die Farbe der Finger Paint Änderungen als Datenträger:

[![Starten von Paint](saving-images/SpinPaint.png "Paint starten")](saving-images/SpinPaint-Large.png#lightbox)

Die **speichern** Schaltfläche `SpinPaint` Klasse ist vergleichbar mit **Fingerpaint** , da sie das Image in einen festen Ordnernamen speichert (**SpainPaint**) und ein Dateinamen aus das Datum und Uhrzeit.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [SpinPaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint)
