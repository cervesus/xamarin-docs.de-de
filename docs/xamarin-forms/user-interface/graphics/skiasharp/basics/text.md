---
title: Die Integration von Text und Grafiken
description: In diesem Artikel wird erläutert, wie zum Bestimmen der Größe von gerenderten Text-Zeichenfolge an die Text mit Grafiken von SkiaSharp in Xamarin.Forms-Anwendungen zu integrieren, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: be1524029ada79896f83517c3b439f2ad0e2c6d9
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615160"
---
# <a name="integrating-text-and-graphics"></a>Die Integration von Text und Grafiken

_Erfahren Sie, wie die Größe der gerenderten Text-Zeichenfolge, die Integration von Text in SkiaSharp-Grafiken zu bestimmen._

In diesem Artikel veranschaulicht, wie Text zu messen, möglicherweise den Text, der eine bestimmte Größe skalieren und integrieren den Text in andere Grafik:

![](text-images/textandgraphicsexample.png "Text, umgeben von Rechtecken")

Die SkiaSharp `Canvas` Klasse enthält außerdem Methoden zum Zeichnen eines Rechtecks ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) und ein Rechteck mit abgerundeten Ecken ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Diese Methoden erfordern das Rechteck definiert werden, als ein `SKRect` Wert.

Die **Text mit einem Frame versehen** Seite konzentriert sich auf der Seite und umschließt es mit einem Rahmen besteht aus zwei Rechtecke mit abgerundeten eine kurze Textzeichenfolge. Die [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Klasse zeigt, wie dies funktioniert.

In SkiaSharp verwenden Sie die `SKPaint` Klasse, um Text und Schriftarten Festlegen von Attributen, aber Sie können es auch die gerenderte Größe des Texts abrufen. Der Anfang des folgenden `PaintSurface` -Ereignishandler ruft zwei verschiedene `MeasureText` Methoden. Die erste [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) Aufruf verfügt über eine einfache `string` Argument und gibt die Breite des Texts in Pixel auf der aktuellen Schriftartattribute Grundlage. Das Programm berechnet dann einen neuen `TextSize` Eigenschaft der `SKPaint` -Objekt auf Grundlage dieser gerenderte Breite der aktuellen `TextSize` -Eigenschaft und die Breite des Anzeigebereichs. Dies dient als festlegen `TextSize` , damit der Text die Zeichenfolge, die bei 90 % der Breite des Bildschirms gerendert werden:

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

Die zweite [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) Aufruf hat einen `SKRect` -Argument, sodass sie sowohl eine Breite und Höhe des gerenderten Texts abruft. Die `Height` -Eigenschaft dieses `SKRect` Wert hängt das Vorhandensein von Großbuchstaben Oberlängen und Unterlängen in der Textzeichenfolge. Verschiedene `Height` Werte werden für die Text-Zeichenfolgen "Mom", "Cat" und "Dog", z. B. gemeldet.

Die `Left` und `Top` Eigenschaften der `SKRect` Struktur die Koordinaten der oberen linken Ecke des gerenderten Texts anzugeben, wenn der Text angezeigt wird, indem eine `DrawText` rufen Sie mit X und Y-Position 0. Beispielsweise, wenn dieses Programm auf einem iPhone 7-Simulator ausgeführt wird `TextSize` erhält den Wert 90.6254 als Ergebnis der Berechnung, die nach dem ersten Aufruf an `MeasureText`. Die `SKRect` Wert aus der zweite Aufruf von `MeasureText` hat die folgenden Eigenschaftenwerte fest:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

Denken Sie daran, dass die X- und Y Sie koordiniert übergeben die `DrawText` Methode angeben, der linken Seite des Texts an der Baseline. Die `Top` Wert gibt an, dass der Text 68 Pixel oberhalb dieser Baseline und (subtrahieren 68 It von 88) erweitert 20 Pixel unterhalb der Basislinie. Die `Left` Wert 6 angibt, dass der Text, 6 Pixel nach rechts von der X-Wert in beginnt der `DrawText` aufrufen. Dadurch können normale zeichenzwischenraum. Wenn der Text gut verpackt in der oberen linken Ecke der Anzeige angezeigt werden sollen, übergeben Sie diese negativen Ausdruck `Left` und `Top` Werte, wie die X- und Y-Koordinaten `DrawText`, in diesem Beispiel &ndash;6 und 68.

Die `SKRect` Struktur definiert verschiedene nützliche Eigenschaften und Methoden, von denen einige werden im weiteren Verlauf verwendet die `PaintSurface` Handler. Die `MidX` und `MidY` Werte geben an, die Koordinaten für den Mittelpunkt des Rechtecks. (Im Beispiel iPhone 7 werden diese Werte 338.4107 und &ndash;24.) Der folgende Code verwendet diese Werte für die Berechnung der Koordinaten der einfachste zentrieren Text auf dem:

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

Die `PaintSurface` Handler wird mit zwei Aufrufe abgeschlossen `DrawRoundRect`, beide Argumente müssen `SKRect`. Dies `SKRect` Wert ähnelt sicherlich die `SKRect` Wert aus der `MeasureText` -Methode, aber es kann nicht identisch sein. Zunächst muss ein bisschen größer sein, sodass abgerundeten Rechtecks nicht über die Ränder des Texts zu zeichnen, und zweitens im Raum verschoben werden sollen muss, damit die `Left` und `Top` Werte entsprechen, auf der linken oberen Ecke, in denen das Rechteck ist, positioniert. Diese zwei Aufträge werden erreicht, indem `Offset` und `Inflate` definierten Methoden `SKRect`:

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

Danach ist der Rest der Methode einfach. Erstellt einen anderen `SKPaint` Objekt für die Rahmen und ruft `DrawRoundRect` zweimal. Der zweite Aufruf verwendet ein Rechteck vergrößert werden, um einen anderen 10 Pixel. Der erste Aufruf gibt einen Eckradius von 20 Pixel; die zweite hat einen Eckradius von 30 Pixel, damit sie scheinen parallel verlaufen:

 [![](text-images/framedtext-small.png "Dreifacher Screenshot der Seite mit einem Frame versehen Text")](text-images/framedtext-large.png#lightbox "dreifachen Screenshot der Seite mit einem Frame versehen von Text")

Sie können Ihr Telefon oder den Simulator auf die Seite, um den Text und Rahmen vergrößert werden, finden Sie unter aktivieren.

Wenn Sie nur Text auf dem Bildschirm zu zentrieren müssen, können Sie dies tun, etwa ohne messen den Text durch Festlegen der `TextAlign` Eigenschaft `SKPaint` zu `SKTextAlign.Center`. Die X-Koordinate, die Sie, in angeben der `DrawText` zeigt dann an, wo die horizontale Mitte des Texts positioniert ist. Wenn Sie den Mittelpunkt des Bildschirms, um übergeben die `DrawText` -Methode, der Text wird horizontal zentriert und *fast* vertikal zentriert werden, da die Baseline vertikal zentriert ausgerichtet werden.

Text selbst kann sehr viel wie eine grafische Option behandelt werden. Eine einfache Möglichkeit ist die Gliederung von Textzeichen anstelle der normalen gefüllte Anzeige angezeigt:

[![](text-images/outlinedtext-small.png "Screenshot der Seite Text mit Kontur dreifach")](text-images/outlinedtext-large.png#lightbox "Tripleresolutionimage-Screenshot der Seite Text mit Kontur")

Dies erfolgt durch einfaches Ändern der normalen `Style` Eigenschaft der `SKPaint` Objekt von der Standardeinstellung von `SKPaintStyle.Fill` zu `SKPaintStyle.Stroke` und eine Strichbreite angeben. Die `PaintSurface` Handler, der die **von Text mit Kontur** Seite zeigt, wie dies funktioniert:

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

 In den letzten Artikeln mehrere Kreisen Sie haben Sie nun gesehen, wie SkiaSharp verwenden Sie zum Zeichnen von Text, Ellipsen und Rechtecke mit abgerundeten. Im nächsten Schritt [SkiaSharp-Linien und-Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) in der erfahren Sie, wie zum Zeichnen von miteinander verbundenen Linien in einem Grafikpfad.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
