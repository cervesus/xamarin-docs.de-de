---
title: Bitmap-Grundlagen in SkiaSharp
description: In diesem Artikel wird erläutert, wie Bitmaps in SkiaSharp aus verschiedenen Quellen laden und in Xamarin.Forms-Anwendungen anzuzeigen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: e1e21fe121fba30755efbabe302ed0f22149e7e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61157395"
---
# <a name="bitmap-basics-in-skiasharp"></a>Bitmap-Grundlagen in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Bitmaps aus verschiedenen Quellen laden und anzeigen._

Die Unterstützung von Bitmaps in SkiaSharp ist sehr umfangreich. Dieser Artikel behandelt nur die Grundlagen &mdash; Bitmaps zu laden und wie diese angezeigt:

![](bitmaps-images/bitmapssample.png "Die Anzeige von zwei bitmaps")

Eine viel detailliertere Untersuchung der Bitmaps finden Sie im Abschnitt [SkiaSharp Bitmaps](../bitmaps/index.md).

Eine SkiaSharp-Bitmap ist ein Objekt des Typs [ `SKBitmap` ](xref:SkiaSharp.SKBitmap). Es gibt viele Möglichkeiten, eine Bitmap zu erstellen, aber in diesem Artikel beschränkt sich auf die [ `SKBitmap.Decode` ](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) -Methode, die die Bitmap in einer .NET-Konsolenanwendung lädt `Stream` Objekt.

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

Um eine Bitmap, die basierend auf einer URL zu laden, können Sie die [ `HttpClient` ](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) Klasse. Sie sollten nur eine Instanz instanziieren `HttpClient` und wiederverwenden, also als ein Feld speichern:

```csharp
HttpClient httpClient = new HttpClient();
```

Bei Verwendung `HttpClient` mit iOS und Android-Anwendungen, sollten Sie Projekteigenschaften festlegen, wie beschrieben in den Dokumenten auf  **[Transport Layer Security (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Da es am einfachsten, Sie zu verwenden, ist die `await` -Operator mit `HttpClient`, der Code kann nicht ausgeführt werden, der `BasicBitmapsPage` Konstruktor. Stattdessen ist er Teil der `OnAppearing` außer Kraft setzen. Hier die URL verweist auf einen Bereich auf der Xamarin-Website mit einige Beispiel-Bitmaps. Ein Paket auf der Website ermöglicht das Anfügen von einer Spezifikation für die größenanpassung der Bitmap in eine bestimmte Breite:


```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    // Load web bitmap.
    string url = "https://developer.xamarin.com/demo/IMG_3256.JPG?width=480";

    try
    {
        using (Stream stream = await httpClient.GetStreamAsync(url))
        using (MemoryStream memStream = new MemoryStream())
        {
            await stream.CopyToAsync(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            webBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Android-Betriebssysteme löst eine Ausnahme aus, wenn Sie verwenden die `Stream` Merry `GetStreamAsync` in die `SKBitmap.Decode` Methode da es einen langwierigen Vorgang in einem Hauptthread ausgeführt wird. Aus diesem Grund werden in den Inhalt der Bitmapdatei kopiert eine `MemoryStream` -Objekt unter Verwendung der `CopyToAsync`.

Die statische `SKBitmap.Decode` Methode ist verantwortlich für das Bitmap-Dateien decodieren. Es funktioniert mit JPEG, PNG oder GIF Bitmapformate, und speichert die Ergebnisse in einem internen SkiaSharp-Format. An diesem Punkt die `SKCanvasView` muss für ungültig erklärt werden, damit kann die `PaintSurface` Handler, der die Anzeige zu aktualisieren. 

## <a name="loading-a-bitmap-resource"></a>Beim Laden einer Bitmapressource

Im Hinblick auf Code ist der einfachste Ansatz für das Laden von Bitmaps eine Bitmap-Ressource direkt in der Anwendung einschließlich. Die **SkiaSharpFormsDemos** Programm enthält einen Ordner namens **Media** mit mehreren Dateien, einschließlich eine mit dem Namen "bitmap" **monkey.png**. Für Bitmaps, die als Programmressourcen gespeichert werden, müssen Sie verwenden die **Eigenschaften** Dialogfeld Geben Sie die Datei eine **Buildvorgang** von **eingebettete Ressource**!

Jede eingebettete Ressource verfügt über eine *Ressourcen-ID* , der den Namen des Projekts, den Ordner und dem Dateinamen, die durch Punkte aller angeschlossenen aus: **SkiaSharpFormsDemos.Media.monkey.png**. Sie erhalten Zugriff auf diese Ressource durch Angeben dieser Ressource-ID als Argument an die [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) Methode der [ `Assembly` ](xref:System.Reflection.Assembly) Klasse:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

Dies `Stream` Objekt übergeben werden, direkt an die `SKBitmap.Decode` Methode.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Laden eine Bitmap aus der Fotobibliothek

Es ist auch möglich, für den Benutzer ein Foto aus Bildbibliothek des Geräts zu laden. Diese Funktion wird nicht von Xamarin.Forms selbst bereitgestellt. Dieser Auftrag erfordert einen Abhängigkeitsdienst, z. B. in diesem Artikel beschriebenen [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Die **IPhotoLibrary.cs** Datei die **SkiaSharpFormsDemos** -Projekt und die drei **PhotoLibrary.cs** Dateien in die Plattformprojekte wurden aus diesem Artikel. Darüber hinaus die Android **"mainactivity.cs"** wurde geändert, wie in diesem Artikel beschrieben, und das iOS-Projekt der Fotobibliothek mit nur zwei Zeilen im unteren Bereich der Zugriff auf eine Berechtigung erteilt wurde die **"Info.plist"**  Datei.

Die `BasicBitmapsPage` Konstruktor fügt ein `TapGestureRecognizer` auf die `SKCanvasView` Taps benachrichtigt werden sollen. Beim Empfang von einem tippen die `Tapped` Handler Ruft den Zugriff auf die Bild-Auswahl Abhängigkeitsdienst und ruft `PickPhotoAsync`. Wenn eine `Stream` Objekt wird zurückgegeben, und klicken Sie dann an sie übergeben die `SKBitmap.Decode` Methode:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();

    using (Stream stream = await photoLibrary.PickPhotoAsync())
    {
        if (stream != null)
        {
            libraryBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Beachten Sie, dass die `Tapped` Handler ruft auch die `InvalidateSurface` Methode der `SKCanvasView` Objekt. Dadurch wird einen neuen Aufruf an die `PaintSurface` Handler.

## <a name="displaying-the-bitmaps"></a>Anzeigen von Bitmaps

Die `PaintSurface` Handler drei Bitmaps angezeigt werden soll. Der Handler wird davon ausgegangen, dass das Smartphone im Hochformat und im Zeichenbereich vertikal in drei gleich große Teile unterteilt.

Die erste Bitmap wird angezeigt, mit der einfachsten [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) Methode. Alles, was, die Sie angeben müssen, sind die X- und Y-Koordinaten der oberen linken Ecke der Bitmap positioniert werden soll:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Obwohl eine `SKPaint` Parameter definiert ist, müssen sie den Standardwert `null` und kann ignoriert werden. Die Pixel der Bitmap werden einfach an die Pixel der Anzeigeoberfläche mit einer Zuordnung übertragen. Sehen Sie eine Anwendung für diese `SKPaint` Argument im nächsten Abschnitt auf [ **SkiaSharp Transparenz**](transparency.md).

Ein Programm erhalten die Pixeldimensionen der Bitmap mit den [ `Width` ](xref:SkiaSharp.SKBitmap.Width) und [ `Height` ](xref:SkiaSharp.SKBitmap.Height) Eigenschaften. Diese Eigenschaften ermöglichen das Programm zum Berechnen von Koordinaten, um die Bitmap in der Mitte des oberen Drittel der Leinwand positionieren:

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

Die anderen zwei Bitmaps werden angezeigt, mit einer Version von [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) mit einer `SKRect` Parameter:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Eine dritte Version des [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) verfügt über zwei `SKRect` Argumente für die Angabe einer rechteckigen Teilmenge der Bitmap für die Anzeige, aber diese Version wird nicht in diesem Artikel verwendet.

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

Sie können Bitmaps mit mehrere verschiedene Stufen von Transparenz und dem nächsten Artikel zum Anzeigen [ **SkiaSharp Transparenz** ](transparency.md) wird beschrieben, wie.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Wählen ein Foto aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
