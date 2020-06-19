---
title: Speichern von skiasharp-Bitmaps in Dateien
description: Erkunden Sie die verschiedenen Dateiformate, die von skiasharp für das Speichern von Bitmaps in der Fotobibliothek des Benutzers unterstützt werden.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01f4fcf1953658af44d2a8996913860a3b605abf
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138657"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>Speichern von skiasharp-Bitmaps in Dateien

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Nachdem eine skiasharp-Anwendung eine Bitmap erstellt oder geändert hat, kann die Anwendung die Bitmap in der Fotobibliothek des Benutzers speichern:

![Speichern von Bitmaps](saving-images/SavingSample.png "Speichern von Bitmaps")

Diese Aufgabe umfasst zwei Schritte:

- Die skiasharp-Bitmap wird in Daten in einem bestimmten Dateiformat, z. b. JPEG oder PNG, umgerechnet.
- Speichern des Ergebnisses in der Fotobibliothek mithilfe von Platt Form spezifischem Code.

## <a name="file-formats-and-codecs"></a>Dateiformate und Codecs

In den meisten gängigen Bitmapdatei-Formaten werden Komprimierungen verwendet, um den Speicherplatz zu reduzieren. Die zwei allgemeinen Kategorien von Komprimierungs Techniken werden als _Verlust_ und _Verlust_frei bezeichnet. Diese Begriffe geben an, ob der Komprimierungs Algorithmus zum Verlust von Daten führt.

Das beliebteste Verlust Format wurde vom Joint Photographic Experts Group entwickelt und wird als JPEG bezeichnet. Der JPEG-Komprimierungs Algorithmus analysiert das Bild mit einem mathematischen Tool, das als _diskrete Kosinus-Transformation_bezeichnet wird, und versucht, Daten zu entfernen, die für die Beibehaltung der visuellen Darstellung des Bilds nicht entscheidend sind. Der Grad der Komprimierung kann mit einer Einstellung gesteuert werden, die in der Regel als _Qualität_bezeichnet wird. Höhere Qualitätseinstellungen führen zu größeren Dateien.

Im Gegensatz dazu analysiert ein verlustfreier Komprimierungs Algorithmus das Bild auf Wiederholung und Muster von Pixeln, die auf eine Weise codiert werden können, die die Daten verringert, aber nicht zum Verlust von Informationen führt. Die ursprünglichen Bitmapdaten können vollständig aus der komprimierten Datei wieder hergestellt werden. Das primäre, in Gebrauch Bare, nicht mehr komprimierte Dateiformat ist Portable Network Graphics (PNG).

Im Allgemeinen wird JPEG für Fotos verwendet, während PNG für Images verwendet wird, die manuell oder algorithmisch generiert wurden. Jeder Verluste lose Komprimierungs Algorithmus, der die Größe einiger Dateien reduziert, muss notwendigerweise die Größe anderer erhöhen. Glücklicherweise tritt diese Größen Zunahme in der Regel nur bei Daten auf, die eine Menge zufälliger (oder scheinbar zufälliger) Informationen enthalten.

Die Komprimierungs Algorithmen sind komplex genug, um zwei Begriffe zu rechtfertigen, die die Komprimierungs-und Dekomprimierungs Prozesse beschreiben:

- _decodieren_ &mdash; ein Bitmapdateiformat lesen und Dekomprimieren
- _Codieren_ &mdash; Komprimieren der Bitmap und Schreiben in ein Bitmapdateiformat

Die- [`SKBitmap`](xref:SkiaSharp.SKBitmap) Klasse enthält mehrere Methoden mit dem Namen `Decode` , die `SKBitmap` aus einer komprimierten Quelle erstellen. Alles, was erforderlich ist, besteht darin, einen Dateinamen, einen Stream oder ein Bytearray bereitzustellen. Der Decoder kann das Dateiformat ermitteln und an die richtige interne Decodierungs Funktion übergeben.

Außerdem verfügt die- [`SKCodec`](xref:SkiaSharp.SKCodec) Klasse über zwei Methoden mit dem Namen `Create` , mit denen ein `SKCodec` Objekt aus einer komprimierten Quelle erstellt werden kann und eine Anwendung mehr an dem Decodierungs Prozess beteiligt werden kann. (Die- `SKCodec` Klasse wird im Artikel [**Animieren von skiasharp-Bitmaps**](animating.md#gif-animation) im Zusammenhang mit der Decodierung einer animierten GIF-Datei gezeigt.)

Beim Codieren einer Bitmap sind weitere Informationen erforderlich: der Encoder muss das jeweilige Dateiformat kennen, das von der Anwendung verwendet werden soll (JPEG oder PNG oder etwas anderes). Wenn ein verlustfreies Format gewünscht ist, muss die Codierung auch die gewünschte Qualität kennen.

Die- `SKBitmap` Klasse definiert eine [`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) Methode mit der folgenden Syntax:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

Diese Methode wird in Kürze ausführlicher beschrieben. Die codierte Bitmap wird in einen beschreibbaren Stream geschrieben. (Das "W" in `SKWStream` steht für "beschreibbare".) Das zweite und das dritte Argument geben das Dateiformat und (für verlustfreie Formate) die gewünschte Qualität im Bereich von 0 bis 100 an.

Außerdem [`SKImage`](xref:SkiaSharp.SKImage) definieren die Klassen und [`SKPixmap`](xref:SkiaSharp.SKPixmap) auch `Encode` Methoden, die etwas vielseitiger sind und die Sie bevorzugen. Mithilfe der statischen-Methode können Sie problemlos ein `SKImage` Objekt aus einem- `SKBitmap` Objekt erstellen [`SKImage.FromBitmap`](xref:SkiaSharp.SKImage.FromBitmap(SkiaSharp.SKBitmap)) . Mithilfe der-Methode können Sie ein- `SKPixmap` Objekt von einem- `SKBitmap` Objekt abrufen [`PeekPixels`](xref:SkiaSharp.SKBitmap.PeekPixels) .

Eine der [`Encode`](xref:SkiaSharp.SKImage.Encode) durch definierten Methoden `SKImage` hat keine Parameter und speichert automatisch in einem PNG-Format. Diese Parameter lose Methode ist sehr einfach zu verwenden.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>Plattformspezifischer Code zum Speichern von Bitmapdateien

Wenn Sie ein `SKBitmap` Objekt in ein bestimmtes Dateiformat codieren, bleiben Sie in der Regel mit einem Stream-Objekt einer bestimmten Sortierung oder einem Array von Daten. Einige der- `Encode` Methoden (einschließlich der ohne durch definierten Parameter `SKImage` ) geben ein- [`SKData`](xref:SkiaSharp.SKData) Objekt zurück, das mithilfe der-Methode in ein Bytearray konvertiert werden kann [`ToArray`](xref:SkiaSharp.SKData.ToArray) . Diese Daten müssen dann in einer Datei gespeichert werden.

Das Speichern in einer Datei im lokalen Anwendungs Speicher ist recht einfach, da Sie Standard `System.IO` Klassen und-Methoden für diese Aufgabe verwenden können. Diese Vorgehensweise wird im Artikel [**Animieren von skiasharp-Bitmaps**](animating.md#bitmap-animation) in Verbindung mit der Animation einer Reihe von Bitmaps der Mandelbrot-Menge veranschaulicht.

Wenn Sie möchten, dass die Datei von anderen Anwendungen gemeinsam genutzt wird, muss Sie in der Fotobibliothek des Benutzers gespeichert werden. Diese Aufgabe erfordert plattformspezifischen Code und die Verwendung von Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

Das **skiasharpformsdemo** -Projekt in der [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Anwendung definiert eine `IPhotoLibrary` Schnittstelle, die mit der-Klasse verwendet wird `DependencyService` . Dadurch wird die Syntax einer `SavePhotoAsync` Methode definiert:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

Diese Schnittstelle definiert auch die- `PickPhotoAsync` Methode, die verwendet wird, um die plattformspezifische Dateiauswahl für die Fotobibliothek des Geräts zu öffnen.

Für `SavePhotoAsync` ist das erste Argument ein Bytearray, das die Bitmap enthält, die bereits in ein bestimmtes Dateiformat (z. b. JPEG oder PNG) codiert wurde. Möglicherweise möchte eine Anwendung alle von ihr erstellten Bitmaps in einem bestimmten Ordner isolieren, der im nächsten Parameter angegeben wird, gefolgt vom Dateinamen. Die Methode gibt einen booleschen Wert zurück, der einen Erfolg angibt.

In den folgenden Abschnitten wird erläutert, wie `SavePhotoAsync` auf jeder Plattform implementiert wird.

### <a name="the-ios-implementation"></a>Die IOS-Implementierung

Die IOS-Implementierung von `SavePhotoAsync` verwendet die- [`SaveToPhotosAlbum`](xref:UIKit.UIImage.SaveToPhotosAlbum*) Methode von `UIImage` :

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

Leider gibt es keine Möglichkeit, einen Dateinamen oder Ordner für das Image anzugeben.

Für die **Info. plist** -Datei im IOS-Projekt ist ein Schlüssel erforderlich, der angibt, dass der Fotobibliothek Bilder hinzugefügt werden:

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

Vorsicht! Der Berechtigungsschlüssel für den einfachen Zugriff auf die Fotobibliothek ist sehr ähnlich, aber nicht identisch:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Die Android-Implementierung

Die Android-Implementierung von `SavePhotoAsync` prüft zunächst, ob das- `folder` Argument `null` oder eine leere Zeichenfolge ist. Wenn dies der Fall ist, wird die Bitmap im Stammverzeichnis der Fotobibliothek gespeichert. Andernfalls wird der Ordner abgerufen, und wenn er nicht vorhanden ist, wird er erstellt:

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

Der-Befehl `MediaScannerConnection.ScanFile` ist nicht unbedingt erforderlich. Wenn Sie das Programm jedoch durch sofortige Überprüfung der Fotobibliothek testen, ist es sehr hilfreich, indem Sie die Bibliothekskatalog Ansicht aktualisieren.

Die **AndroidManifest.xml** Datei erfordert das folgende Berechtigungs Kennzeichen:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>Die UWP-Implementierung

Die UWP-Implementierung von `SavePhotoAsync` ähnelt in der Struktur der Android-Implementierung:

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

Der Bereich " **Funktionen** " der Datei " **Package. appxmanifest** " erfordert eine **Bilderbibliothek**.

## <a name="exploring-the-image-formats"></a>Untersuchen der Bildformate

Hier ist die- [`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) Methode von `SKImage` wieder:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](xref:SkiaSharp.SKEncodedImageFormat)ist eine Enumeration mit Membern, die auf elf Bitmapdateiformate verweisen, von denen einige recht unüberschaubar sind:

- `Astc`&mdash;Adaptive skalierbare Textur Komprimierung
- `Bmp`&mdash;Windows-Bitmap
- `Dng`&mdash;Adobe Digital Negative
- `Gif`&mdash;Graphics Interchange Format
- `Ico`&mdash;Windows-Symbolbilder
- `Jpeg`&mdash;Joint Photographic Experts Group
- `Ktx`&mdash;Khronos-Textur Format für OpenGL
- `Pkm`&mdash;Pokémon-Speicherdatei
- `Png`&mdash;Portable Network Graphics
- `Wbmp`&mdash;Bitmap-Format des drahtlos Anwendungs Protokolls (1 Bit pro Pixel)
- `Webp`&mdash;Google Webp-Format

Wie Sie in Kürze sehen werden, werden nur drei dieser Dateiformate ( `Jpeg` , `Png` und `Webp` ) von skiasharp unterstützt.

`SKBitmap` `bitmap` Wenn Sie ein Objekt mit dem Namen in der Fotobibliothek des Benutzers speichern möchten, benötigen Sie auch einen Member der- `SKEncodedImageFormat` Enumeration mit dem Namen `imageFormat` und (bei verlustfreien Formaten) eine ganzzahlige `quality` Variable. Sie können den folgenden Code verwenden, um die Bitmap in einer Datei mit dem Namen `filename` im Ordner zu speichern `folder` :

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

Die- `SKManagedWStream` Klasse wird von abgeleitet `SKWStream` (was "Beschreib barer Stream" steht). Die- `Encode` Methode schreibt die codierte Bitmapdatei in diesen Stream. Die Kommentare in diesem Code verweisen auf eine Fehlerüberprüfung, die Sie möglicherweise ausführen müssen.

Die Seite **Dateiformate speichern** in der [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Anwendung verwendet ähnlichen Code, damit Sie mit dem Speichern einer Bitmap in den verschiedenen Formaten experimentieren können.

Die XAML-Datei enthält ein `SKCanvasView` , das eine Bitmap anzeigt, während der Rest der Seite alle Elemente enthält, die die Anwendung benötigt, um die-Methode aufzurufen `Encode` `SKBitmap` . Sie verfügt über einen `Picker` für einen Member der- `SKEncodedImageFormat` Enumeration, einen `Slider` für das Quality-Argument für verlustfreie Bitmapformate, zwei `Entry` Ansichten für einen Dateinamen und einen Ordnernamen und ein `Button` zum Speichern der Datei.

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

Die Code-Behind-Datei lädt eine Bitmap-Ressource und verwendet das `SKCanvasView` , um Sie anzuzeigen. Diese Bitmap ändert sich nie. Der- `SelectedIndexChanged` Handler für den `Picker` ändert den Dateinamen mit einer Erweiterung, die mit dem-Enumerationsmember identisch ist:

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

Der- `Clicked` Handler für die `Button` erledigt alle wirklichen Aufgaben. Es werden zwei Argumente für `Encode` aus dem `Picker` und abgerufen `Slider` , und anschließend wird der zuvor gezeigte Code verwendet, um einen `SKManagedWStream` für die-Methode zu erstellen `Encode` . In den beiden `Entry` Ansichten werden Ordner-und Dateinamen für die-Methode bereitstellen `SavePhotoAsync` .

Die meisten dieser Methoden sind für die Behandlung von Problemen oder Fehlern vorgesehen. Wenn `Encode` ein leeres Array erstellt, bedeutet dies, dass das jeweilige Dateiformat nicht unterstützt wird. Wenn `SavePhotoAsync` zurückgibt `false` , wurde die Datei nicht erfolgreich gespeichert.

Das Programm wird ausgeführt:

[![Speichern von Dateiformaten](saving-images/SaveFileFormats.png "Speichern von Dateiformaten")](saving-images/SaveFileFormats-Large.png#lightbox)

Dieser Screenshot zeigt die einzigen drei Formate, die auf diesen Plattformen unterstützt werden:

- JPEG
- PNG
- Webp

Für alle anderen Formate `Encode` schreibt die-Methode nichts in den Stream, und das resultierende Bytearray ist leer.

Die Bitmap, die die Seite " **Dateiformate speichern** " speichert, ist 600-Pixel-Quadrat. Bei 4 Bytes pro Pixel beträgt der Arbeitsspeicher insgesamt 1.440.000 Bytes. In der folgenden Tabelle wird die Dateigröße für verschiedene Kombinationen aus Dateiformat und Qualität angezeigt:

|Format|Qualität|Size|
|------|------:|---:|
| PNG | – | 492k |
| JPEG | 0 | 2,95 KB |
|      | 50 | 22,1 k |
|      | 100 | 206k |
| Webp | 0 | 2,71 KB |
|      | 50 | 11.9 KB |
|      | 100 | 101 KB |

Sie können mit verschiedenen Qualitätseinstellungen experimentieren und die Ergebnisse untersuchen.

## <a name="saving-finger-paint-art"></a>Speichern von fingerfarb Grafiken

Eine häufige Verwendung einer Bitmap ist in Zeichnungsprogrammen, wo Sie als eine _Schatten Bitmap_bezeichnet werden. Die gesamte Zeichnung wird in der Bitmap beibehalten, die dann vom Programm angezeigt wird. Die Bitmap ist auch für das Speichern der Zeichnung praktisch.

Im Artikel " [**Finger zeichnen in skiasharp**](../paths/finger-paint.md) " wurde veranschaulicht, wie die Touch-Nachverfolgung verwendet wird, um ein Primitives fingerzeichnungs Programm zu implementieren. Das Programm hat nur eine Farbe und nur eine Strichbreite unterstützt, aber die gesamte Zeichnung wurde in einer Auflistung von `SKPath` Objekten beibehalten.

Die Seite **fingerpaint mit Save** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel behält auch die gesamte Zeichnung in einer Auflistung von- `SKPath` Objekten bei. Sie rendert aber auch die Zeichnung in einer Bitmap, die Sie in der Fotobibliothek speichern kann.

Ein Großteil dieses Programms ähnelt dem ursprünglichen **fingerpaint** -Programm. Eine Erweiterung besteht darin, dass die XAML-Datei nun Schaltflächen mit der Bezeichnung **Clear** und **Save**instanziiert:

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

Die Code-Behind-Datei verwaltet ein Feld vom Typ mit dem `SKBitmap` Namen `saveBitmap` . Diese Bitmap wird im Handler erstellt oder neu erstellt, `PaintSurface` Wenn sich die Größe der Anzeige Oberfläche ändert. Wenn die Bitmap neu erstellt werden muss, wird der Inhalt der vorhandenen Bitmap in die neue Bitmap kopiert, damit alles unabhängig davon, wie sich die Größe der Anzeige Oberfläche ändert, beibehalten wird:

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

Die vom Handler durchgeführte Zeichnung `PaintSurface` erfolgt ganz am Ende und besteht lediglich aus dem Rendern der Bitmap.

Die Berührungs Verarbeitung ähnelt dem früheren Programm. Das Programm verwaltet zwei Auflistungen, `inProgressPaths` und `completedPaths` , die alles enthalten, was der Benutzer seit dem letzten Löschen der Anzeige gezeichnet hat. Für jedes Berührungs Ereignis ruft der `OnTouchEffectAction` Handler Folgendes auf `UpdateBitmap` :

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

Die `UpdateBitmap` Methode wird `saveBitmap` durch Erstellen eines neuen `SKCanvas` , löschen und anschließendes Rendern aller Pfade auf der Bitmap neu gezeichnet. Dadurch wird der Vorgang beendet `canvasView` , sodass die Bitmap auf der Anzeige gezeichnet werden kann.

Im folgenden finden Sie die Handler für die beiden Schaltflächen. Mit der Schaltfläche " **Löschen** " werden sowohl Pfad Auflistungen, Updates `saveBitmap` (die das Löschen der Bitmap ergeben) gelöscht, als auch ungültig `SKCanvasView` :

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

Der Handler für die **Save** -Schaltfläche verwendet die vereinfachte [`Encode`](xref:SkiaSharp.SKImage.Encode) Methode aus `SKImage` . Diese Methode codiert mit dem PNG-Format. Das `SKImage` -Objekt wird auf Grundlage von erstellt `saveBitmap` , und das- `SKData` Objekt enthält die codierte PNG-Datei.

Die- `ToArray` Methode von `SKData` erhält ein Bytearray. Dies wird an die- `SavePhotoAsync` Methode, zusammen mit einem Namen fester Ordner und einem eindeutigen Dateinamen weitergeleitet, der aus dem aktuellen Datum und der aktuellen Uhrzeit erstellt wurde.

Hier sehen Sie das Programm bei der Ausführung:

[![Finger Farben speichern](saving-images/FingerPaintSave.png "Finger Farben speichern")](saving-images/FingerPaintSave-Large.png#lightbox)

Im [**spinpaint**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint) -Beispiel wird eine sehr ähnliche Technik verwendet. Dies ist auch ein fingerzeichnungs Programm, mit dem Unterschied, dass der Benutzer einen drehenden Datenträger zeichnet, der dann die Entwürfe auf den anderen vier Quadranten wiederherstellt. Die Farbe der Fingerfarbe ändert sich, wenn der Datenträger sich dreht:

[![Drehfarbe](saving-images/SpinPaint.png "Drehfarbe")](saving-images/SpinPaint-Large.png#lightbox)

Die Schaltfläche **Speichern** der- `SpinPaint` Klasse ähnelt **Finger Paint** darin, dass Sie das Bild in einen Namen mit festem Ordner (**spainpaint**) und einen Dateinamen speichert, der aus dem Datum und der Uhrzeit erstellt wurde.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Spinpaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint)
