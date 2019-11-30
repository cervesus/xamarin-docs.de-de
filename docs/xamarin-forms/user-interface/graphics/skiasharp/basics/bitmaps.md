---
title: Grundlagen der Bitmap in skiasharp
description: In diesem Artikel wird erläutert, wie Bitmaps in skiasharp aus verschiedenen Quellen geladen und in xamarin. Forms-Anwendungen angezeigt werden. Dies wird anhand von Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 4aa73e1cba3f970396e5a52679372a160f47f7af
ms.sourcegitcommit: 3bf02ecac9a8855779e07eb1e7e27abb9fc38934
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2019
ms.locfileid: "74658267"
---
# <a name="bitmap-basics-in-skiasharp"></a>Grundlagen der Bitmap in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Laden Sie Bitmaps aus verschiedenen Quellen, und zeigen Sie Sie an._

Die Unterstützung von Bitmaps in skiasharp ist recht umfangreich. In diesem Artikel werden nur die Grundlagen &mdash; das Laden von Bitmaps und deren Anzeige behandelt:

![](bitmaps-images/basicbitmaps-small.png "The display of two bitmaps")

Eine weitaus tiefere Untersuchung von Bitmaps finden Sie im Abschnitt [skiasharp-Bitmaps](../bitmaps/index.md).

Eine skiasharp-Bitmap ist ein Objekt vom Typ [`SKBitmap`](xref:SkiaSharp.SKBitmap). Es gibt viele Möglichkeiten, eine Bitmap zu erstellen, aber dieser Artikel beschränkt sich auf die [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) -Methode, die die Bitmap aus einem .net `Stream`-Objekt lädt.

Die Seite **grundlegende Bitmaps** im **skiasharpformsdemos** -Programm veranschaulicht, wie Bitmaps aus drei verschiedenen Quellen geladen werden:

- Über das Internet
- Aus einer Ressource, die in die ausführbare Datei eingebettet ist
- Aus der Fotobibliothek des Benutzers

Drei `SKBitmap` Objekte für diese drei Quellen sind als Felder in der [`BasicBitmapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) -Klasse definiert:

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

## <a name="loading-a-bitmap-from-the-web"></a>Laden einer Bitmap aus dem Web

Zum Laden einer Bitmap, die auf einer URL basiert, können Sie die [`HttpClient`](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) -Klasse verwenden. Sie sollten nur eine Instanz von `HttpClient` instanziieren und wieder verwenden. speichern Sie Sie daher als Feld:

```csharp
HttpClient httpClient = new HttpClient();
```

Wenn Sie `HttpClient` mit IOS-und Android-Anwendungen verwenden, möchten Sie die Projekteigenschaften wie in den Dokumenten auf **[Transport Layer Security (TLS) 1,2](~/cross-platform/app-fundamentals/transport-layer-security.md)** beschrieben festlegen.

Da es am einfachsten ist, den `await`-Operator mit `HttpClient`zu verwenden, kann der Code nicht im `BasicBitmapsPage`-Konstruktor ausgeführt werden. Stattdessen ist Sie Teil der `OnAppearing` Überschreibung. Die URL hier verweist auf einen Bereich auf der xamarin-Website mit einigen Beispiel Bitmaps. Ein Paket auf der Website ermöglicht das Anfügen einer Spezifikation zum Ändern der Größe der Bitmap an eine bestimmte Breite:

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

            webBitmap = SKBitmap.Decode(memStream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Das Android-Betriebssystem löst eine Ausnahme aus, wenn die `Stream`, die von `GetStreamAsync` in der `SKBitmap.Decode`-Methode zurückgegeben wird, verwendet wird, da es einen langwierigen Vorgang für einen Haupt Thread ausführt. Aus diesem Grund wird der Inhalt der Bitmapdatei mithilfe `CopyToAsync`in ein `MemoryStream` Objekt kopiert.

Die statische `SKBitmap.Decode` Methode ist für das Decodieren von Bitmapdateien verantwortlich. Sie funktioniert mit den Bitmapformaten JPEG, PNG und GIF und speichert die Ergebnisse in einem internen skiasharp-Format. An diesem Punkt muss die `SKCanvasView` für ungültig erklärt werden, damit der `PaintSurface` Handler die Anzeige aktualisieren kann.

## <a name="loading-a-bitmap-resource"></a>Laden einer Bitmap-Ressource

Im Hinblick auf Code ist der einfachste Ansatz zum Laden von Bitmaps, eine Bitmap-Ressource direkt in Ihre Anwendung einzubeziehen. Das **skiasharpformsdemos** -Programm enthält einen Ordner mit dem Namen **Medien** , der mehrere Bitmapdateien enthält, einschließlich einer mit dem Namen " **Monkey. png**". Für Bitmaps, die als Programmressourcen gespeichert werden, müssen Sie das Dialogfeld **Eigenschaften** verwenden, um der Datei eine **Buildaktion** der **eingebetteten Ressource**zu übergeben.

Jede eingebettete Ressource verfügt über eine *Ressourcen-ID* , die aus dem Projektnamen, dem Ordner und dem Dateinamen besteht, die alle durch Zeiträume verbunden sind: **skiasharpformsdemos. Media. Monkey. png**. Sie können auf diese Ressource zugreifen, indem Sie die Ressourcen-ID als Argument für die [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) -Methode der [`Assembly`](xref:System.Reflection.Assembly) -Klasse angeben:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

Dieses `Stream` Objekt kann direkt an die `SKBitmap.Decode`-Methode übermittelt werden.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Laden einer Bitmap aus der Fotobibliothek

Es ist auch möglich, dass der Benutzer ein Foto aus der Bildbibliothek des Geräts lädt. Diese Funktion wird nicht von xamarin. Forms selbst bereitgestellt. Der Auftrag erfordert einen Abhängigkeits Dienst, z. b. den, der im Artikel [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)beschrieben wird.

Die **IPhotoLibrary.cs** -Datei im **skiasharpformsdemos** -Projekt und die drei **PhotoLibrary.cs** -Dateien in den Platt Form Projekten wurden aus diesem Artikel angepasst. Außerdem wurde die Android-Datei **MainActivity.cs** wie im Artikel beschrieben geändert, und dem IOS-Projekt wurde die Berechtigung zum Zugriff auf die Fotobibliothek mit zwei Zeilen am Ende der Datei **Info. plist** erteilt.

Der `BasicBitmapsPage`-Konstruktor fügt dem `SKCanvasView` eine `TapGestureRecognizer` hinzu, um über TAPS benachrichtigt zu werden. Beim Empfang einer Tap-Methode erhält der `Tapped` Handler Zugriff auf den Abhängigkeits Dienst der Bildauswahl und ruft `PickPhotoAsync`auf. Wenn ein `Stream` Objekt zurückgegeben wird, wird es an die `SKBitmap.Decode`-Methode weitergegeben:

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

Beachten Sie, dass der `Tapped` Handler auch die `InvalidateSurface`-Methode des `SKCanvasView`-Objekts aufruft. Dadurch wird ein neuer-aufrufbefehl an den `PaintSurface`-Handler generiert.

## <a name="displaying-the-bitmaps"></a>Anzeigen der Bitmaps

Der `PaintSurface` Handler muss drei Bitmaps anzeigen. Der Handler geht davon aus, dass sich das Telefon im Hochformat befindet, und teilt den Zeichenbereich vertikal in drei gleiche Teile auf.

Die erste Bitmap wird mit der einfachsten [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) -Methode angezeigt. Sie müssen lediglich die X-und Y-Koordinaten angeben, in denen die linke obere Ecke der Bitmap positioniert werden soll:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Obwohl ein `SKPaint` Parameter definiert ist, verfügt er über den Standardwert `null`, und Sie können ihn ignorieren. Die Pixel der Bitmap werden einfach an die Pixel der Anzeige Oberfläche mit einer eins-zu-Eins-Zuordnung übertragen. Im nächsten Abschnitt über [**skiasharp-Transparenz**](transparency.md)wird eine Anwendung für dieses `SKPaint` Argument angezeigt.

Ein Programm kann die Pixel Dimensionen einer Bitmap mit den Eigenschaften [`Width`](xref:SkiaSharp.SKBitmap.Width) und [`Height`](xref:SkiaSharp.SKBitmap.Height) abrufen. Diese Eigenschaften ermöglichen es dem Programm, Koordinaten zu berechnen, um die Bitmap in der Mitte des oberen dritten Bereichs der Canvas zu positionieren:

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

Die anderen beiden Bitmaps werden mit einer Version von [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) mit einem `SKRect`-Parameter angezeigt:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Eine dritte Version von [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) verfügt über zwei `SKRect` Argumente zum Angeben einer rechteckigen Teilmenge der Bitmap, die angezeigt werden soll. diese Version wird in diesem Artikel jedoch nicht verwendet.

Hier ist der Code zum Anzeigen der Bitmap, die aus einer eingebetteten Ressourcen Bitmap geladen wurde:

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

Die Bitmap wird auf die Abmessungen des Rechtecks gestreckt, weshalb der Affe horizontal in den folgenden Screenshots gestreckt wird:

[![](bitmaps-images/basicbitmaps-small.png "A triple screenshot of the Basic Bitmaps page")](bitmaps-images/basicbitmaps-large.png#lightbox "A triple screenshot of the Basic Bitmaps page")

Das dritte Bild &mdash; das Sie nur sehen können, wenn Sie das Programm ausführen und ein Foto aus ihrer eigenen Bildbibliothek laden &mdash; wird auch innerhalb eines Rechtecks angezeigt, aber die Position und Größe des Rechtecks werden angepasst, um das Seitenverhältnis der Bitmap beizubehalten. Diese Berechnung ist etwas komplizierter, da Sie einen Skalierungsfaktor basierend auf der Größe der Bitmap und des Ziel Rechtecks berechnen muss und das Rechteck in diesem Bereich zentriert:

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

Wenn noch keine Bitmap aus der Bildbibliothek geladen wurde, wird im `else`-Block ein Text angezeigt, der den Benutzer auffordert, auf den Bildschirm zu tippen.

Sie können Bitmaps mit unterschiedlichen Grad an Transparenz anzeigen, und im nächsten Artikel über [**skiasharp-Transparenz**](transparency.md) wird beschrieben, wie.

## <a name="related-links"></a>Verwandte Themen

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
