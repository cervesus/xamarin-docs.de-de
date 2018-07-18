---
title: Bitmap-Grundlagen in SkiaSharp
description: In diesem Artikel wird erläutert, wie Bitmaps in SkiaSharp aus verschiedenen Quellen laden und in Xamarin.Forms-Anwendungen anzuzeigen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: dec6fa1534f14836ae98677ad33e280ff510fb97
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995185"
---
# <a name="bitmap-basics-in-skiasharp"></a>Bitmap-Grundlagen in SkiaSharp

_Bitmaps aus verschiedenen Quellen laden und anzeigen._

Die Unterstützung von Bitmaps in SkiaSharp ist sehr umfangreich. Dieser Artikel behandelt nur die Grundlagen &mdash; Bitmaps zu laden und wie diese angezeigt:

![](bitmaps-images/bitmapssample.png "Die Anzeige von zwei bitmaps")

Eine SkiaSharp-Bitmap ist ein Objekt des Typs [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Es gibt viele Möglichkeiten, eine Bitmap zu erstellen, aber in diesem Artikel beschränkt sich auf die [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) -Methode, die aus der Bitmap lädt ein [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) -Objekt, das eine Bitmapdatei verweist. Es ist sinnvoll, verwenden Sie die [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) abgeleitete Klasse `SKStream` , da sie einen Konstruktor verfügt, die eine .NET akzeptiert [ `Stream` ](xref:System.IO.Stream) Objekt.

Die **grundlegende Bitmaps** auf der Seite die **SkiaSharpFormsDemos** Programm veranschaulicht, wie zum Laden von Bitmaps aus drei verschiedenen Quellen:

- Über das Internet
- Aus einer Ressource in die ausführbare Datei eingebettet
- Aus der Fotobibliothek des Benutzers

Drei `SKBitmap` -Objekten für diese drei Datenquellen, als Felder in definiert sind der [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) Klasse:

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

Um eine Bitmap, die basierend auf einer URL zu laden, können Sie die [ `WebRequest` ](xref:System.Net.WebRequest) Klasse, wie gezeigt in den folgenden Code, der ausgeführt wird, der `BasicBitmapsPage` Konstruktor. Hier die URL verweist auf einen Bereich auf der Xamarin-Website mit einige Beispiel-Bitmaps. Ein Paket auf der Website ermöglicht das Anfügen von einer Spezifikation für die größenanpassung der Bitmap in eine bestimmte Breite:

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

Wenn die Bitmap wurde erfolgreich heruntergeladen wurde, wird die Callback-Methode zum Übergeben der `BeginGetResponse` Methode ausgeführt wird. Die `EndGetResponse` Aufruf muss sich in einem `try` block für den Fall, dass ein Fehler aufgetreten ist. Die `Stream` -Objekts vom `GetResponseStream` ist nicht auf manchen Plattformen ist ausreichend, damit der Bitmapinhalt in kopiert werden eine `MemoryStream` Objekt. An diesem Punkt die `SKManagedStream` Objekt kann erstellt werden. Nun verweist auf die Bitmapdatei, die wahrscheinlichste Ursache, eine JPEG- oder PNG-Datei ist. Die `SKBitmap.Decode` -Methode decodiert die Bitmap-Datei und speichert die Ergebnisse in einem internen SkiaSharp-Format.

Die Rückrufmethode übergeben werden, um `BeginGetResponse` ausgeführt wird, nachdem der Konstruktor ausgeführt hat, dies bedeutet, dass die `SKCanvasView` muss für ungültig erklärt werden, um zu ermöglichen die `PaintSurface` Handler, der die Anzeige zu aktualisieren. Allerdings die `BeginGetResponse` Rückruf in einem zweiten Thread der Ausführung ausgeführt wird, daher ist es erforderlich, verwenden Sie `Device.BeginInvokeOnMainThread` zum Ausführen der `InvalidateSurface` -Methode in der UI-Thread.

## <a name="loading-a-bitmap-resource"></a>Beim Laden einer Bitmapressource

Im Hinblick auf Code ist der einfachste Ansatz für das Laden von Bitmaps eine Bitmap-Ressource direkt in der Anwendung einschließlich. Die **SkiaSharpFormsDemos** Programm enthält einen Ordner namens **Media** , enthält eine Bitmap-Datei mit dem Namen **monkey.png**. In der **Eigenschaften** Dialogfeld Geben Sie für diese Datei, eine solche Datei eine **Buildvorgang** von **eingebettete Ressource**!

Jede eingebettete Ressource verfügt über eine *Ressourcen-ID* , die aus den Projektnamen, den Ordner und dem Dateinamen, die durch Punkte aller angeschlossenen besteht: **SkiaSharpFormsDemos.Media.monkey.png**. Sie erhalten Zugriff auf diese Ressource durch Angeben dieser Ressource-ID als Argument an die [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) Methode der [ `Assembly` ](xref:System.Reflection.Assembly) Klasse:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Dies `Stream` -Objekt direkt konvertiert werden kann ein `SKManagedStream` Objekt.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Laden eine Bitmap aus der Fotobibliothek

Es ist auch möglich, für den Benutzer ein Foto aus Bildbibliothek des Geräts zu laden. Diese Funktion wird nicht von Xamarin.Forms selbst bereitgestellt. Dieser Auftrag erfordert einen Abhängigkeitsdienst, z. B. in diesem Artikel beschriebenen [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Die **IPicturePicker.cs** -Datei und die drei **PicturePickerImplementation.cs** Dateien aus diesem Artikel wurden die den verschiedenen Projekten kopiert die **SkiaSharpFormsDemos**Lösung, und der neue Namespacenamen gegeben. Darüber hinaus die Android **"mainactivity.cs"** wurde geändert, wie in diesem Artikel beschrieben, und das iOS-Projekt der Fotobibliothek mit nur zwei Zeilen im unteren Bereich der Zugriff auf eine Berechtigung erteilt wurde die **"Info.plist"**  Datei.

Die `BasicBitmapsPage` Konstruktor fügt ein `TapGestureRecognizer` auf die `SKCanvasView` Taps benachrichtigt werden sollen. Beim Empfang von einem tippen die `Tapped` Handler Ruft den Zugriff auf die Bild-Auswahl Abhängigkeitsdienst und ruft `GetImageStreamAsync`. Wenn eine `Stream` Objekt wird zurückgegeben, und klicken Sie dann die Inhalte werden kopiert, in einem `MemoryStream`, wie für andere Plattformen erforderlich. Der Rest des Codes ähnelt die beiden anderen Techniken:

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

Die `PaintSurface` Handler drei Bitmaps angezeigt werden soll. Der Handler wird davon ausgegangen, dass das Smartphone im Hochformat und im Zeichenbereich vertikal in drei gleich große Teile unterteilt.

Die erste Bitmap wird angezeigt, mit der einfachsten [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) Methode. Alles, was, die Sie angeben müssen, sind die X- und Y-Koordinaten der oberen linken Ecke der Bitmap positioniert werden soll:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Obwohl eine `SKPaint` Parameter definiert ist, müssen sie den Standardwert `null` und kann ignoriert werden. Die Pixel der Bitmap werden einfach an die Pixel der Anzeigeoberfläche mit einer Zuordnung übertragen.

Ein Programm erhalten die Pixeldimensionen der Bitmap mit den [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) und [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) Eigenschaften. Diese Eigenschaften ermöglichen das Programm zum Berechnen von Koordinaten, um die Bitmap in der Mitte des oberen Drittel der Leinwand positionieren:

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

Die anderen zwei Bitmaps werden angezeigt, mit einer Version von [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) mit einer `SKRect` Parameter:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Eine dritte Version des [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) verfügt über zwei `SKRect` Argumente für die Angabe einer rechteckigen Teilmenge der Bitmap für die Anzeige, aber diese Version wird nicht in diesem Artikel verwendet.

Hier ist der Code zum Anzeigen der Bitmap, die aus einer Bitmap der eingebetteten Ressource geladen:

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

Die Bitmap wird an die Abmessungen des Rechtecks gestreckt wird das Monkey-Objekt in diese Screenshots horizontal gestreckt wird:

[![](bitmaps-images/basicbitmaps-small.png "Einen dreifachen Screenshot der Seite für grundlegende Bitmaps")](bitmaps-images/basicbitmaps-large.png#lightbox "einen dreifachen Screenshot der Seite für grundlegende Bitmaps")

Das dritte Bild &mdash; Sie nur sehen, wenn Sie das Programm ausführen, und Laden ein Foto aus Ihren eigenen Bildbibliothek &mdash; wird auch innerhalb eines Rechtecks, jedoch des Rechtecks angezeigt werden Position und Größe angepasst, um die Bitmap-Seitenverhältnis beizubehalten. Diese Berechnung ist ein wenig komplizierter, da es erforderlich ist, berechnen einen Skalierungsfaktor basierend auf der Größe der Bitmap und das Zielrechteck und zentrieren das Rechteck in diesem Bereich:

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

Wenn noch keine Bitmap aus der Bildbibliothek geladen wurde und dann die `else` Block zeigt Text ein, den Benutzer auffordern, tippen Sie, auf dem Bildschirm.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Wählen ein Foto aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
