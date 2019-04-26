---
title: Pixel und geräteunabhängige Einheiten
description: In diesem Artikel untersucht die Unterschiede zwischen SkiaSharp und Xamarin.Forms-Koordinaten und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
ms.openlocfilehash: c1e4a76a70dcac3414d384469f25bad7908ae77f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019949"
---
# <a name="pixels-and-device-independent-units"></a>Pixel und geräteunabhängige Einheiten

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Entdecken Sie die Unterschiede zwischen SkiaSharp und Xamarin.Forms-Koordinaten_

In diesem Artikel erläutert die Unterschiede bei der das Koordinatensystem, die bei der SkiaSharp und Xamarin.Forms verwendet. Sie erhalten Informationen zum Konvertieren zwischen den zwei Koordinatensysteme und die auch Grafiken, die einen bestimmten Bereich zu füllen:

![](pixels-images/screenfillexample.png "Eine Ellipse, die den Bildschirm ausfüllt.")

Wenn Sie in Xamarin.Forms seit einer Weile programmiert haben, können Sie ein Gefühl für die Xamarin.Forms-Koordinaten und Größen liegen. Die Kreise in den beiden vorherigen Artikeln scheinen ein wenig klein, um Sie.

Diese Kreise *sind* im Vergleich zu Xamarin.Forms-Größen kleine. Standardmäßig zeichnet SkiaSharp in Einheiten von Pixeln während Xamarin.Forms Koordinaten und Größen geräteunabhängige Einheit hergestellt, indem die zugrunde liegende Plattform Inhaltsmenge abgeleitet. (Weitere Informationen zu der Xamarin.Forms-Koordinatensystem befinden sich [Kapitel 5. Umgang mit Größen](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) des Buchs *Erstellen mobiler Apps mit Xamarin.Forms*.)

Die Seite in der [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm berechtigt **Oberfläche Größe** SkiaSharp Textausgabe verwendet, um die Größe der Anzeigeoberfläche aus drei verschiedenen Quellen anzuzeigen:

- Der normale Xamarin.Forms [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) und [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) Eigenschaften der `SKCanvasView` Objekt.
- Die [ `CanvasSize` ](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize) Eigenschaft der `SKCanvasView` Objekt.
- Die [ `Size` ](xref:SkiaSharp.SKImageInfo.Size) Eigenschaft der `SKImageInfo` -Wert, der entspricht der `Width` und `Height` Eigenschaften, die in den vorherigen zwei Seiten verwendet.

Die [ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Klasse veranschaulicht, wie diese Werte angezeigt. Der Konstruktor speichert die `SKCanvasView` Objekt wie ein Feld, damit es in zugegriffen werden kann die `PaintSurface` -Ereignishandler:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` enthält sechs verschiedene `DrawText` Methoden, aber dies [ `DrawText` ](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint)) Methode ist die einfachste:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Sie geben die Zeichenfolge, die die X- und Y-Koordinaten, in dem der Text ist, um zu beginnen, und ein `SKPaint` Objekt. Die X-Koordinate gibt an, wo der linken Seite des Texts positioniert ist, aber Achten Sie auf: Gibt an, die Y-Koordinate der Position des der *Baseline* des Texts. Wenn Sie jemals von Hand auf unterstrichenen Papier geschrieben haben, ist die Baseline der Zeile, auf welche Zeichen-Konto, und klicken Sie unterhalb der Unterlängen (z. B. auf den Buchstaben g, p, Q und y) abgeleitet werden.

Die `SKPaint` Objekt können Sie die Farbe des Texts, die die Schriftfamilie und die Textgröße anzugeben. In der Standardeinstellung die [ `TextSize` ](xref:SkiaSharp.SKPaint.TextSize) Eigenschaft hat den Wert 12, was in kleinen Text auf Geräten wie Smartphones mit hoher Auflösung führt. Im Prinzip die einfachsten Anwendungen benötigen Sie auch einige Informationen über die Größe des Texts, die Sie anzeigen. Die `SKPaint` -Klasse definiert eine [ `FontMetrics` ](xref:SkiaSharp.SKPaint.FontMetrics) Eigenschaft und mehrere [ `MeasureText` ](xref:SkiaSharp.SKPaint.MeasureText(System.String)) Methoden, aber weniger originelle Anforderungen, die [ `FontSpacing` ](xref:SkiaSharp.SKPaint.FontSpacing) Eigenschaft stellt einen empfohlenen Wert für Abstand aufeinander folgende Textzeilen.

Die folgenden `PaintSurface` Ereignishandler erstellt ein `SKPaint` Objekt für eine `TextSize` von 40 Pixel, die gewünschte Höhe des Texts zwischen dem oberen Rand der Oberlängen am unteren Rand der Unterlängen ist. Die `FontSpacing` -Wert, der `SKPaint` -Objekt zurückgibt, ist ein bisschen größer als die zu 47 Pixel.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Die Methode beginnt die erste Zeile des Texts mit einem X-Koordinate der 20 (für einen kleinen Rand auf der linken Seite) und eine Y-Koordinate der `fontSpacing`, dies ist ein wenig mehr als die zum Anzeigen der Gesamthöhe der ersten Zeile des Texts am oberen Rand der Anzeigeoberfläche erforderlich sind. Nach jedem Anruf `DrawText`, ein oder zwei Schritten erhöht die Y-Koordinate `fontSpacing`.

Hier wird das Programm ausgeführt wird:

[![](pixels-images/surfacesize-small.png "Dreifacher Screenshot der Seite für Surface-Größe")](pixels-images/surfacesize-large.png#lightbox "dreifachen Screenshot der Seite für Surface-Größe")

Wie Sie sehen können, die `CanvasSize` Eigenschaft der `SKCanvasView` und `Size` Eigenschaft der `SKImageInfo` Wert in die Pixeldimensionen reporting konsistent sind. Die `Height` und `Width` Eigenschaften der `SKCanvasView` Xamarin.Forms-Eigenschaften sind und die Größe der Ansicht in der geräteunabhängige Einheiten, die von der Plattform definiert.

Sieben iOS-Simulator auf der linken Seite verfügt über zwei Pixel pro geräteunabhängige Einheit, und die Android Nexus 5 in der Mitte hat drei Pixel pro Einheit. Deshalb die einfache Kreis oben gezeigten hat verschiedene Größen auf verschiedenen Plattformen.

Wenn Sie vollständig in geräteunabhängigen Einheiten arbeiten möchten, Sie können dazu durch Festlegen der `IgnorePixelScaling` Eigenschaft der `SKCanvasView` zu `true`. Allerdings können Sie die Ergebnisse nicht gefallen. SkiaSharp rendert die Grafiken auf eine kleinere Geräteoberfläche, mit einer Pixelgröße gleich der Größe der Ansicht in geräteunabhängigen Einheiten. (Z. B. SkiaSharp würde eine Anzeigeoberfläche der 360 x 512 Pixel auf Nexus 5 verwenden.) Klicken Sie dann skaliert dieses Abbild, Größe, was zu spürbaren Bitmap Jaggies.

Um die gleiche Auflösung des Bilds zu gewährleisten, ist eine bessere Lösung, Ihre eigenen einfache Funktionen für die Konvertierung zwischen den zwei Koordinatensysteme zu schreiben.

Zusätzlich zu den `DrawCircle` Methode `SKCanvas` definiert außerdem zwei `DrawOval` Methoden, die eine Ellipse zu zeichnen. Eine Ellipse wird durch zwei Radien statt einem einzigen Radius definiert. Diese werden als bezeichnet die *wichtigen Radius* und *kleinere Radius*. Die `DrawOval` Methode zeichnet eine Ellipse mit der zwei Radien, die parallel zu den X- und Y-Achsen. (Wenn Sie möchten eine Ellipse mit Achsen gezeichnet werden soll, die nicht parallel zu den X- und y-Achsen sind, können Sie einen drehungsübergang wie im folgenden Artikel beschrieben [ **der Rotate-Transformation** ](../transforms/rotate.md) oder ein Graphics-Pfad, wie unter der Artikel [ **drei Möglichkeiten, einen Bogen zu zeichnen**](../curves/arcs.md)). Diese Überladung von der [ `DrawOval` ](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) Methode benennt die Parameter zwei Radien `rx` und `ry` , um anzugeben, dass sie parallel zu den X- und y-Achsen sind:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Ist es möglich, eine Ellipse zeichnen, die die Anzeigeoberfläche füllt? Die **Ellipse ausgefüllt** Seite erläutert, wie. Die `PaintSurface` -Ereignishandler in der [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) Klasse subtrahiert halbe Strichbreite über den `xRadius` und `yRadius` Werte für die gesamte Ellipse passen und die zugehörige Beschreiben Sie in der Oberfläche angezeigt:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Hier wird ausgeführt:

[![](pixels-images/ellipsefill-small.png "Dreifacher Screenshot der Seite für Surface-Größe")](pixels-images/ellipsefill-large.png#lightbox "dreifachen Screenshot der Seite für Surface-Größe")

Die andere [ `DrawOval` ](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint)) Methode verfügt über eine [ `SKRect` ](xref:SkiaSharp.SKRect) -Argument, das ein Rechteck, das in Bezug auf die X- und Y-Koordinaten der oberen linken Ecke und unteren rechten Ecke definiert ist. Das Oval füllt, Rechteck, was darauf hinweist, dass es möglich eventuell, verwenden sie in der **Ellipse ausgefüllt** Seite wie folgt:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Allerdings schneidet, die alle Ränder die Kontur der Ellipse auf den vier Seiten ab. Müssen Sie alle Anpassen der `SKRect` Konstruktorargumente auf Grundlage der `strokeWidth` damit dies funktioniert rechten:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
