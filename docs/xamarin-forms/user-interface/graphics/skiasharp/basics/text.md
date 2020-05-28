---
title: ''
description: In diesem Artikel wird erläutert, wie Sie die Größe der gerenderten Text Zeichenfolge für die Integration von Text mit skiasharp-Grafiken in Xamarin.Forms Anwendungen ermitteln und dies mit Beispielcode veranschaulichen.
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ee97ee2aae11e4e54a0d25e80ffd7bce301fa2f3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137682"
---
# <a name="integrating-text-and-graphics"></a>Integrieren von Text und Grafiken

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Siehe Ermitteln der Größe der gerenderten Text Zeichenfolge zum Integrieren von Text in skiasharp-Grafiken_

In diesem Artikel wird veranschaulicht, wie Sie Text messen, den Text auf eine bestimmte Größe skalieren und Text in andere Grafiken integrieren:

![](text-images/textandgraphicsexample.png "Text surrounded by rectangles")

Dieses Bild enthält auch ein abgerundetes Rechteck. Die skiasharp `Canvas` -Klasse enthält [`DrawRect`](xref:SkiaSharp.SKCanvas.DrawRect*) Methoden zum Zeichnen eines Rechtecks und [`DrawRoundRect`](xref:SkiaSharp.SKCanvas.DrawRoundRect*) von Methoden zum Zeichnen eines Rechtecks mit abgerundeten Ecken. Diese Methoden ermöglichen es, das Rechteck als- `SKRect` Wert oder auf andere Weise zu definieren.

Auf der Seite mit dem **Text** wird eine kurze Text Zeichenfolge auf der Seite zentriert und mit einem Frame umgeben, der aus einem Paar abgerundeter Rechtecke besteht. Die [`FramedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) -Klasse zeigt, wie Sie ausgeführt wird.

In skiasharp verwenden Sie die `SKPaint` -Klasse, um Text-und Schriftart Attribute festzulegen, Sie können Sie jedoch auch zum Abrufen der gerenderten Textgröße verwenden. Der Anfang des folgenden `PaintSurface` Ereignis Handlers ruft zwei verschiedene `MeasureText` Methoden auf. Der erste [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) -Befehl verfügt über ein einfaches `string` Argument und gibt die Pixel Breite des Texts basierend auf den aktuellen Schriftart Attributen zurück. Das Programm berechnet dann `TextSize` `SKPaint` basierend auf der gerenderten Breite, der aktuellen `TextSize` Eigenschaft und der Breite des Anzeige Bereichs eine neue Eigenschaft des-Objekts. Diese Berechnung soll so festgelegt `TextSize` werden, dass die Text Zeichenfolge bei 90% der Bildschirmbreite gerendert wird:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Der zweite [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String,SkiaSharp.SKRect@)) Aufrufs hat ein `SKRect` Argument, sodass er sowohl eine Breite als auch eine Höhe des gerenderten Texts erhält. Die- `Height` Eigenschaft dieses `SKRect` Werts hängt davon ab, ob Großbuchstaben, Vorgänger und Nachfolger in der Text Zeichenfolge vorhanden sind. Für `Height` die Text Zeichenfolgen "MOM", "Cat" und "Dog" werden z. b. unterschiedliche Werte gemeldet.

Die `Left` -Eigenschaft und die-Eigenschaft `Top` der- `SKRect` Struktur geben die Koordinaten der linken oberen Ecke des gerenderten Texts an, wenn der Text durch einen `DrawText` -Befehl mit X-und Y-Positionen 0 angezeigt wird. Wenn dieses Programm z. b. auf einem iPhone 7-Simulator ausgeführt wird, `TextSize` wird der Wert 90,6254 als Ergebnis der Berechnung nach dem ersten-Befehl zugewiesen `MeasureText` . Der `SKRect` Wert, der aus dem zweiten-Befehl abgerufen `MeasureText` wurde, verfügt über die folgenden Eigenschaftswerte:

- `Left`= 6
- `Top` = &ndash;68
- `Width`= 664,8214
- `Height`= 88;

Beachten Sie, dass die X-und Y-Koordinaten, die Sie an die-Methode übergeben, `DrawText` die linke Seite des Texts an der Baseline angeben. Der `Top` Wert gibt an, dass der Text 68 Pixel oberhalb dieser Baseline und (Subtrahieren von 68 von 88) 20 Pixel unterhalb der Baseline erweitert. Der `Left` Wert 6 gibt an, dass der Text in dem-Befehl sechs Pixel rechts vom X-Wert beginnt `DrawText` . Dies ermöglicht normale Zeichen übergreifende Abstände. Wenn Sie den Text in der oberen linken Ecke der Anzeige bündig anzeigen möchten, übergeben Sie die negativen `Left` `Top` Werte dieser Werte und als X-und Y-Koordinaten von `DrawText` , in diesem Beispiel &ndash; 6 und 68.

Die `SKRect` -Struktur definiert mehrere praktische Eigenschaften und Methoden, von denen einige im Rest des `PaintSurface` Handlers verwendet werden. Der `MidX` - `MidY` Wert und der-Wert geben die Koordinaten der Mitte des Rechtecks an. (Im iPhone 7-Beispiel lauten diese Werte 338,4107 und &ndash; 24.) Der folgende Code verwendet diese Werte für die einfachste Berechnung von Koordinaten zum Zentrieren von Text auf der Anzeige:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

Die `SKImageInfo` Info-Struktur definiert auch eine [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) Eigenschaft vom Typ `SKRect` , sodass Sie auch diese berechnen können `xText` `yText` :

```csharp
float xText = info.Rect.MidX - textBounds.MidX;
float yText = info.Rect.MidY - textBounds.MidY;
```

Der- `PaintSurface` Handler schließt mit zwei Aufrufen `DrawRoundRect` von ab, die beide Argumente von erfordern `SKRect` . Dieser `SKRect` Wert basiert auf dem `SKRect` Wert, der von der- `MeasureText` Methode abgerufen wurde, kann jedoch nicht identisch sein. Zuerst muss Sie etwas größer sein, damit das abgerundete Rechteck nicht über Kanten des Texts gezeichnet wird. Zweitens muss Sie in den Raum versetzt werden, damit die `Left` Werte und `Top` der oberen linken Ecke entsprechen, in der das Rechteck positioniert werden soll. Diese beiden Aufträge werden durch die [`Offset`](xref:SkiaSharp.SKRect.Offset*) -und-Methoden durchgeführt, die [`Inflate`](xref:SkiaSharp.SKRect.Inflate*) durch definiert werden `SKRect` :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

Danach ist der Rest der Methode geradlinig. Es erstellt ein weiteres `SKPaint` -Objekt für die Rahmen und ruft `DrawRoundRect` zweimal auf. Der zweite-Befehl verwendet ein Rechteck, das durch eine andere 10 Pixel aufgebraucht ist. Der erste-Befehl gibt einen Eckradius von 20 Pixeln an. Die zweite hat einen Eckradius von 30 Pixeln, sodass Sie parallel erscheinen:

 [![](text-images/framedtext-small.png "Triple screenshot of the Framed Text page")](text-images/framedtext-large.png#lightbox "Triple screenshot of the Framed Text page")

Sie können das Telefon oder den Simulator seitwärts drehen, um den Text und die Rahmen Vergrößerung anzuzeigen.

Wenn Sie nur Text auf dem Bildschirm zentrieren müssen, können Sie diesen Vorgang ungefähr durchführen, ohne den Text zu messen. Legen Sie stattdessen die- [`TextAlign`](xref:SkiaSharp.SKPaint.TextAlign) Eigenschaft von `SKPaint` auf den-Enumerationsmember fest [`SKTextAlign.Center`](xref:SkiaSharp.SKTextAlign) . Die X-Koordinate, die Sie in der-Methode angeben, `DrawText` gibt dann an, wo die horizontale Mitte des Texts positioniert ist. Wenn Sie den Mittelpunkt des Bildschirms an die- `DrawText` Methode übergeben, wird der Text horizontal zentriert und *beinahe* vertikal zentriert, da die Baseline vertikal zentriert wird.

Text kann ähnlich wie jedes andere grafische Objekt behandelt werden. Eine einfache Option besteht darin, die Gliederung der Textzeichen anzuzeigen:

[![](text-images/outlinedtext-small.png "Triple screen shot of the Outlined Text page")](text-images/outlinedtext-large.png#lightbox "Triple screenshot of the Outlined Text page")

Dies wird einfach durch Ändern der normalen- `Style` Eigenschaft des `SKPaint` -Objekts von der Standardeinstellung `SKPaintStyle.Fill` in `SKPaintStyle.Stroke` und durch Angeben einer Strichbreite erreicht. Der `PaintSurface` Handler der **beschriebenen Textseite** zeigt, wie er ausgeführt wird:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Ein weiteres gängiges grafisches Objekt ist die Bitmap. Dabei handelt es sich um ein umfangreiches Thema, das ausführlich im Abschnitt [**skiasharp-Bitmaps**](../bitmaps/index.md)behandelt wird, aber der nächste Artikel, [**Bitmap-Grundlagen in skiasharp**](bitmaps.md), bietet eine kurze Einführung.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
