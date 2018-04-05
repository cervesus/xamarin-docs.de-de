---
title: Bitmap-Grundlagen
description: Laden Sie Bitmaps aus verschiedenen Quellen und angezeigt werden können.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: 688c6218f9ac66e3dfd6cd157e43f9b639e124c6
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="bitmap-basics"></a>Bitmap-Grundlagen

_Laden Sie Bitmaps aus verschiedenen Quellen und angezeigt werden können._

Die Unterstützung von Bitmaps in SkiaSharp ist sehr umfangreich. Dieser Artikel umfasst nur die Grundlagen &mdash; wie Bitmaps geladen und wie diese angezeigt:

![](bitmaps-images/bitmapssample.png "Die Anzeige von zwei bitmaps")

Eine Bitmap SkiaSharp ist ein Objekt des Typs [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Es gibt viele Möglichkeiten, eine Bitmap zu erstellen, aber in diesem Artikel schränkt selbst, um die [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) -Methode, die aus der Bitmap lädt ein [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) -Objekt, das eine Bitmapdatei verweist. Es ist sinnvoll, verwenden Sie die [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) von abgeleitete Klasse `SKStream` , da es einen Konstruktor verfügt, das eine .NET-Klasse akzeptiert [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) Objekt.

Die **grundlegende Bitmaps** auf der Seite der **SkiaSharpFormsDemos** Programm veranschaulicht, wie Bitmaps aus drei verschiedenen Quellen zu laden:

- Über das Internet
- Aus einer Ressource in die ausführbare Datei eingebettet
- Aus der Benutzer Foto-Bibliothek

Drei `SKBitmap` -Objekte für diese drei Datenquellen, als Felder in definiert sind der [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) Klasse:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Laden eine Bitmap aus dem Web

Um eine Bitmap, die basierend auf einer URL zu laden, können Sie die [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Klasse, entsprechend dem folgenden Code ausgeführt wird, der `BasicBitmapsPage` Konstruktor. Hier die URL verweist auf einen Bereich auf der Xamarin-Website mit einige Beispiel-Bitmaps. Ein Paket auf der Website ermöglicht das Anfügen einer Spezifikation zu einer bestimmten Breite der Bitmap Größe:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Wenn die Bitmap wurde erfolgreich heruntergeladen wurde, wird die Rückrufmethode an übergeben der `BeginGetResponse` Methode ausgeführt wird. Die `EndGetResponse` Aufruf angehören soll eine `try` blockieren, für den Fall, dass ein Fehler aufgetreten ist. Die `Stream` abgerufenes Objekt `GetResponseStream` ist nicht ausreichend auf manchen Plattformen, sodass in der Bitmapinhalt kopiert wird ein `MemoryStream` Objekt. An diesem Punkt der `SKManagedStream` Objekt kann erstellt werden. Nun verweist auf die Bitmap-Datei, die wahrscheinlich eine JPEG oder PNG-Datei ist. Die `SKBitmap.Decode` Methode die Bitmapdatei decodiert und speichert die Ergebnisse in ein internes SkiaSharp-Format.

An die Rückrufmethode übergeben `BeginGetResponse` ausgeführt wird, nachdem der Konstruktor ausgeführt hat, dies bedeutet, dass die `SKCanvasView` für ungültig erklärt werden, um zulassen muss die `PaintSurface` Handler, um die Anzeige zu aktualisieren. Allerdings die `BeginGetResponse` Rückruf in einem sekundären Thread der Ausführung ausgeführt wird, daher ist es notwendig, verwenden `Device.BeginInvokeOnMainThread` zum Ausführen der `InvalidateSurface` -Methode in der Benutzeroberflächenthread.

## <a name="loading-a-bitmap-resource"></a>Beim Laden einer Bitmapressource

Im Hinblick auf Code ist der einfachste Ansatz zum Laden von Bitmaps eine Bitmapressource direkt in der Anwendung einschließlich. Die **SkiaSharpFormsDemos** Programm enthält einen Ordner namens **Medien** , enthält eine Bitmap-Datei mit dem Namen **monkey.png**. In der **Eigenschaften** Dialogfeld für diese Datei müssen Sie eine solche Datei geben eine **Buildvorgang** von **eingebettete Ressource**!

Jede eingebettete Ressource weist einen *Ressourcen-ID* , die den Namen des Projekts, des Ordners und der Dateiname, der allen verbundenen durch Punkte besteht: **SkiaSharpFormsDemos.Media.monkey.png**. Erhalten Sie Zugriff auf diese Ressource durch Angeben dieser Ressource-ID als ein Argument an die [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) Methode der [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) Klasse:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Dies `Stream` Objekt direkt konvertiert werden kann ein `SKManagedStream` Objekt.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Laden eine Bitmap aus der Fotobibliothek

Es ist auch möglich, für den Benutzer beim Laden eines Fotos aus der Bildbibliothek des Geräts. Diese Funktion wird nicht von Xamarin.Forms selbst bereitgestellt. Der Auftrag dies erfordert eine Abhängigkeitsdienst, wie die in diesem Artikel beschriebene [Kommissionieren eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Die **IPicturePicker.cs** -Datei und die drei **PicturePickerImplementation.cs** Dateien aus diesem Artikel wurden in den verschiedenen Projekten kopiert die **SkiaSharpFormsDemos**Lösung, und der neue Namespacenamen gegeben. Darüber hinaus die Android **MainActivity.cs** Datei wie im Artikel beschrieben geändert wurde, und das iOS-Projekt über die Berechtigung zum Zugriff auf die Fotobibliothek mit zwei Zeilen im unteren Teil der zugewiesen wurde die **"Info.plist"**  Datei.

Die `BasicBitmapsPage` Konstruktor fügt eine `TapGestureRecognizer` auf die `SKCanvasView` Taps benachrichtigt zu werden. Nach dem Empfang der ein tippen der `Tapped` Handler erhält Zugriff auf das Bild Datumsauswahl Abhängigkeitsdienst und ruft `GetImageStreamAsync`. Wenn eine `Stream` Objekt wird zurückgegeben, und klicken Sie dann die Inhalte werden kopiert, in einem `MemoryStream`, wie für andere Plattformen erforderlich. Der Rest des Codes ist vergleichbar mit zwei anderen Verfahren:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Beachten Sie, dass die `Tapped` Ereignishandler ruft die `InvalidateSurface` Methode der `SKCanvasView` Objekt. Dadurch wird einen neuen Aufruf an die `PaintSurface` Handler.

## <a name="displaying-the-bitmaps"></a>Anzeigen von Bitmaps

Die `PaintSurface` muss Handler drei Bitmaps anzuzeigen. Der Handler wird davon ausgegangen, dass das Telefon im Hochformat ist und im Zeichenbereich vertikal in drei gleich große Teile unterteilt.

Die erste Bitmap wird angezeigt, mit der einfachsten [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) Methode. Alles, was, die Sie angeben müssen, sind die X- und Y-Koordinaten der linken oberen Ecke der Bitmap, in denen positioniert wird:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Obwohl ein `SKPaint` Parameter definiert ist, er hat den Standardwert `null` und kann ignoriert werden. Die Pixel der Bitmap werden einfach in Pixel der Anzeigeoberfläche mit einer Zuordnung übertragen.

Ein Programm erhalten die Pixel-Dimensionen einer Bitmap mit den [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) und [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) Eigenschaften. Diese Eigenschaften ermöglichen Koordinaten zum Positionieren der Bitmap in der Mitte des oben in der dritten des Zeichenbereichs zu berechnen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Die anderen zwei Bitmaps werden angezeigt, mit einer Version von [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) mit einem `SKRect` Parameter:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Eine dritte Version von [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) verfügt über zwei `SKRect` Argumente für das Angeben einer rechteckigen Teilmenge der Bitmap für die Anzeige, aber diese Version wird nicht in diesem Artikel verwendet.

Hier ist der Code zum Anzeigen der Bitmap, die aus einer eingebetteten Ressourcenbitmap geladen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

Die Bitmap gestreckt wird mit den Abmessungen des Rechtecks aus diesem Grund die Affe horizontal in diese Screenshots verzerrt wird:

[![](bitmaps-images/basicbitmaps-small.png "Dreifacher Screenshot von der Seite "grundlegende Bitmaps"")](bitmaps-images/basicbitmaps-large.png#lightbox "triple Screenshot der Seite grundlegende Bitmaps")

Das dritte Bild &mdash; dem Sie nur anzeigen können, wenn Sie das Programm auszuführen, und Laden ein Foto aus Ihren eigenen Bildbibliothek &mdash; wird ebenfalls innerhalb eines Rechtecks, aber des Rechtecks angezeigt Position und Größe angepasst werden, um die Bitmap Seitenverhältnis beibehalten. In dieser Berechnung ist etwas komplizierter, da es erfordert die Berechnung von einem Skalierungsfaktor basierend auf der Größe der Bitmap und Zielrechtecks und zentrieren das Rechteck in diesem Bereich:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Wenn noch keine Bitmap aus der Bildbibliothek geladen wurde und dann die `else` Block zeigt Text ein, den Benutzer auffordern, den Bildschirm tippen.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Kommissionieren eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
