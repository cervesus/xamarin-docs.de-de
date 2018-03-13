---
title: Integrieren von Text und Grafiken
description: "Informationen Sie zur Bestimmung der Größe des gerenderten Textzeichenfolge SkiaSharp Grafiken Text integriert"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1cb6b6fcd8a9d02910842eb3eba966fce281d977
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="integrating-text-and-graphics"></a>Integrieren von Text und Grafiken

_Informationen Sie zur Bestimmung der Größe des gerenderten Textzeichenfolge SkiaSharp Grafiken Text integriert_

In diesem Artikel werden die messen Text, möglicherweise den Text an einer bestimmten Größe skaliert und integrieren den Text in andere Grafik veranschaulicht:

![](text-images/textandgraphicsexample.png "Text, umgeben von Rechtecken")

Die SkiaSharp `Canvas` Klasse enthält auch Methoden zum Zeichnen eines Rechtecks ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) und ein Rechteck mit abgerundeten Ecken ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Diese Methoden erfordern das Rechteck definiert werden, als ein `SKRect` Wert.

Die **Text eingebunden** Seite zentriert wird eine kurze Textzeichenfolge, die auf der Seite und mit einem Frame besteht aus einem Paar von abgerundeten Rechtecken umgeben. Die [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Klasse zeigt, wie dies funktioniert.

In SkiaSharp verwenden Sie die `SKPaint` Klasse, um Text und Schriftart-Attribute festlegen, aber Sie können auch sie um die gerenderte Größe des Texts zu erhalten. Der Anfang des folgenden `PaintSurface` Ereignishandler ruft zwei unterschiedliche `MeasureText` Methoden. Die erste [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) Aufruf weist ein einfaches `string` Argument und gibt die Breite des Texts in Pixel auf der aktuellen Schriftartattribute Basis. Die Anwendung berechnet dann einen neuen `TextSize` Eigenschaft von der `SKPaint` -Objekt auf Grundlage dieser gerenderten Breite, die aktuelle `TextSize` Eigenschaft und die Breite des Anzeigebereichs. Dies dient als festzulegende `TextSize` , damit der Text die Zeichenfolge, die bei 90 % der Breite des Bildschirms gerendert werden:

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
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Die zweite [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) Aufruf weist ein `SKRect` Argument, damit sie sowohl eine Breite und Höhe des gerenderten Texts abruft. Die `Height` -Eigenschaft dieser `SKRect` Wert hängt das Vorhandensein von Großbuchstaben, Oberlängen und Unterlängen in der Textzeichenfolge. Verschiedene `Height` Werte für die Text-Zeichenfolgen "Mom", "Cat" und "Dog", z. B. gemeldet werden.

Die `Left` und `Top` Eigenschaften der `SKRect` Struktur die Koordinaten der oberen linken Ecke des gerenderten Texts anzugeben, wenn der Text angezeigt wird, indem eine `DrawText` telefonisch mit X- und Y-Positionen von 0. Beispielsweise, wenn dieses Programm wird ausgeführt auf einem iPhone 7-Simulator `TextSize` der 90.6254-Wert als Ergebnis der Berechnung, die nach dem ersten Aufruf von zugewiesen `MeasureText`. Die `SKRect` der zweite Aufruf von abgerufenen Wert `MeasureText` verfügt über die folgenden Eigenschaftswerte:

- `Left` = 6
- `Top` = &#x2013;68
- `Width` = 664.8214
- `Height` = 88;

Denken Sie daran, dass die X- und Y koordiniert übergeben, um die `DrawText` Methode angeben, der linken Seite des Texts an der Basislinie. Die `Top` Wert gibt an, dass der Text 68 Pixel oberhalb dieser Basislinie und (68 It aus 88 Subtraktion) erweitert 20 Pixel unterhalb der Basislinie. Die `Left` Wert 6 gibt an, dass der Text rechts neben der X-Wert in 6 Pixel beginnt die `DrawText` aufrufen. Dies ermöglicht normalen zeichenzwischenraum. Wenn der Text in der oberen linken Ecke der Anzeige problemlos angezeigt werden sollen, übergeben Sie die negative dieser `Left` und `Top` Werte wie die X- und Y-Koordinaten `DrawText`, klicken Sie im folgenden Beispiel & #x 2013, 6 und 68.

Die `SKRect` Struktur definiert mehrere praktisch verwendeten Eigenschaften und Methoden, von denen einige sind im weiteren Verlauf der `PaintSurface` Handler. Die `MidX` und `MidY` Werte geben die Koordinaten für den Mittelpunkt des Rechtecks. (Im Beispiel iPhone 7 werden diese Werte 338.4107 und & #x 2013; 24.) Der folgende Code verwendet diese Werte bei der Berechnung des einfachste von Koordinaten zur Mitte der Text auf dem:

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

Die `PaintSurface` Handler mit beiden Aufrufe von endet `DrawRoundRect`, beide Argumente erfordern `SKRect`. Dies `SKRect` Wert ist sicherlich ähnelt der `SKRect` abgerufenen Wert der `MeasureText` -Methode, aber sie können nicht identisch sein. Zuerst müssen sie etwas größer sein, sodass keine der abgerundeten Rechtecks über Ränder des Texts zeichnen, und zweitens im Raum verschoben werden sollen muss, damit die `Left` und `Top` der oberen linken Ecke, in denen das Rechteck ist, werden, die Werte entsprechen positioniert. Diese zwei Aufträge werden erreicht, indem `Offset` und `Inflate` definierten Methoden `SKRect`:

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

Danach wird der Rest der Methode selbsterklärend. Erstellt eine andere `SKPaint` Objekt für den Rahmen und ruft `DrawRoundRect` zweimal. Der zweite Aufruf verwendet, ein Rechteck, das von einem anderen 10 Pixel vergrößert wird. Der erste Aufruf gibt einen Eckradius von 20 Pixel; die zweite hat einen Eckradius 30 Pixel, damit sie parallel zu sein scheinen:

 [![](text-images/framedtext-small.png "Dreifacher Screenshot der Seite Text eingebunden")](text-images/framedtext-large.png#lightbox "dreifacher Screenshot der Seite Text eingebunden")

Aktivieren Sie Ihr Smartphone oder seitwärts Simulator, um den Text und die Frame-Größe anzuzeigen.

Wenn Sie nur einige Text auf dem Bildschirm zentriert müssen, können Sie dies tun, etwa ohne messen den Text durch Festlegen der `TextAlign` Eigenschaft `SKPaint` auf `SKTextAlign.Center`. Die X-Koordinate, die Sie, in angeben der `DrawText` zeigt dann an, wo die horizontale Mitte des Texts positioniert ist. Wenn Sie den Mittelpunkt des Bildschirms zum Übergeben der `DrawText` -Methode, der Text horizontal mittig und *fast* vertikal zentriert, da die Basislinie vertikal zentriert werden soll.

Text selbst kann ähnlich wie eine grafische Option behandelt werden. Eine einfache Möglichkeit ist zum Anzeigen der Gliederung von Textzeichen anstelle der normalen gefüllte Anzeige:

[![](text-images/outlinedtext-small.png "Screenshot der Seite beschriebenen Text dreifach")](text-images/outlinedtext-large.png#lightbox "dreifach Screenshot der Seite beschriebenen Text")

Dies erfolgt durch einfaches Ändern von normalen `Style` Eigenschaft der `SKPaint` Objekt von der Standardeinstellung von `SKPaintStyle.Fill` zu `SKPaintStyle.Stroke` verwenden und eine Strichbreite angeben. Die `PaintSurface` Handler, der die **beschriebenen Text** Seite zeigt, wie dies funktioniert:

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
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 In den vergangenen mehreren Artikeln Kreise Sie haben Sie nun gesehen, wie SkiaSharp verwenden, Zeichnen von Text, Ellipsen und Rechtecke gerundet. Im nächsten Schritt [SkiaSharp Linien und Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) in der erfahren Sie, wie in einem Grafikpfad miteinander verbundenen Linien gezeichnet werden.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
