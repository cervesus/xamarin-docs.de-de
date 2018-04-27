---
title: Pixel und geräteunabhängigen Einheiten
description: Untersuchen Sie die Unterschiede zwischen SkiaSharp und Xamarin.Forms-Koordinaten
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 95f782fd4670782217d8ce4bc055341747a71170
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="pixels-and-device-independent-units"></a>Pixel und geräteunabhängigen Einheiten

_Untersuchen Sie die Unterschiede zwischen SkiaSharp und Xamarin.Forms-Koordinaten_

In diesem Artikel wird erklärt, die Unterschiede in das Koordinatensystem, die bei der SkiaSharp und Xamarin.Forms verwendet werden. Sie können Informationen zum Konvertieren zwischen zwei Koordinatensysteme und zeichnen Sie Grafiken, die einen bestimmten Bereich füllen auch abrufen:

![](pixels-images/screenfillexample.png "Eine Ellipse, die den Bildschirm ausfüllt.")

Wenn Sie in Xamarin.Forms für eine Weile programmiert haben, müssen Sie einen Eindruck darüber für Größen und Xamarin.Forms Koordinaten möglicherweise. Die Kreise in den beiden vorherigen Artikeln erscheinen möglicherweise etwas klein ist, um Sie.

Diese Kreise *sind* im Vergleich mit Xamarin.Forms-Größen kleine. Standardmäßig zeichnet SkiaSharp in Pixel, während Xamarin.Forms Größen und Koordinaten auf eine geräteunabhängige Einheit, die durch die die zugrunde liegende Plattform beruht. (Weitere Informationen zu Xamarin.Forms-Koordinatensystem verwendbaren im [Kapitel 5. Umgang mit Größen](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) des Buchs *Erstellen mobiler Apps mit Xamarin.Forms*.)

Die Seite in der [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm berechtigt **Oberfläche Größe** SkiaSharp Textausgabe verwendet, um die Größe der Anzeigeoberfläche aus drei verschiedenen Quellen anzuzeigen:

- Die normale Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) und [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) Eigenschaften der `SKCanvasView` Objekt.
- Die [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Eigenschaft von der `SKCanvasView` Objekt.
- Die [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Eigenschaft der `SKImageInfo` Wert, der mit konsistent ist die `Width` und `Height` Eigenschaften, die in den beiden vorherigen Seiten verwendet.

Die [ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Klasse veranschaulicht, wie diese Werte anzuzeigen. Der Konstruktor speichert die `SKCanvasView` Objekt als ein Feld, damit es in zugegriffen werden kann die `PaintSurface` Ereignishandler:

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

`SKCanvas` umfasst sechs verschiedenen `DrawText` Methoden, aber dies [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) Methode ist die einfachste Form:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Geben Sie die Textzeichenfolge, die X- und Y-Koordinaten, in dem der Text ist, um zu beginnen, und ein `SKPaint` Objekt. Gibt an, die X-Koordinate, an der linken Seite des Texts positioniert wird, aber achten: die Y-Koordinate gibt die Position von der *Baseline* des Texts. Wenn Sie jemals von Hand auf unterstrichenen Papier geschrieben haben, ist die Baseline, die Zeile, auf welche Zeichen Sit und unterhalb der Unterlängen bestimmt (z. B. die auf den Buchstaben g, p, Q und y) abgeleitet werden.

Die `SKPaint` Objekts können Sie die Farbe des Texts, der Schriftfamilie sowie die Textgröße anzugeben. Wird standardmäßig die [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) Eigenschaft kann einen Wert von 12, was sehr kleine Text auf Geräten wie Smartphones mit hoher Auflösung zur Folge. In anderen Wert als die einfachsten Anwendungen kann es auch einige Informationen über die Größe des Texts erforderlich, die Sie anzeigen. Die `SKPaint` Klasse definiert ein [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) Eigenschaft sowie einige weitere [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) Methoden, aber weniger praktische Anforderungen der [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) Eigenschaft stellt einen empfohlenen Wert für Abstand aufeinander folgenden Textzeilen.

Die folgenden `PaintSurface` Ereignishandler erstellt ein `SKPaint` -Objekt für eine `TextSize` von 40 Pixel, die gewünschte Höhe des Texts von der obersten Position des Oberlängen an das Ende Unterlängen bestimmt wird. Die `FontSpacing` -Wert, der `SKPaint` Objekt zurückgibt, ist ein bisschen größer als, zu 47 Pixel.

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

Die Methode beginnt die erste Zeile des Texts mit einem X-Koordinate der 20 (für einen kleinen Rand auf der linken Seite) und eine Y-Koordinate der `fontSpacing`, also etwas mehr als für die Anzeige der vollständigen Höhe der ersten Zeile des Texts am oberen Rand der Anzeigeoberfläche erforderlich ist. Nach jedem Anruf `DrawText`, ein oder zwei Schritten ist die Y-Koordinate vergrößert `fontSpacing`.

Hier wird das Programm auf allen drei Plattformen ausgeführt:

[![](pixels-images/surfacesize-small.png "Dreifacher Screenshot der Seite Oberfläche Größe")](pixels-images/surfacesize-large.png#lightbox "dreifacher Screenshot der Seite Oberfläche Größe")

Wie Sie sehen können, die `CanvasSize` Eigenschaft von der `SKCanvasView` und die `Size` Eigenschaft der `SKImageInfo` Wert, der in den Pixelmaßen reporting konsistent sind. Die `Height` und `Width` Eigenschaften der `SKCanvasView` Xamarin.Forms-Eigenschaften, und melden die Größe der Sicht in dem geräteunabhängigen Einheiten, die von der Plattform definiert.

Die iOS 7-Simulator auf der linken Seite 2 Pixel pro geräteunabhängige Einheit und das Android Nexus 5 in der Mitte 3 Pixel pro Einheit. Deshalb ist die weiter oben dargestellten einfache Kreis hat unterschiedliche Größen auf verschiedenen Plattformen.

Falls Sie lieber vollständig in geräteunabhängigen Einheiten arbeiten möchten, Sie können dazu durch Festlegen der `IgnorePixelScaling` Eigenschaft von der `SKCanvasView` auf `true`. Allerdings können Sie die Ergebnisse nicht gefallen. SkiaSharp rendert die Grafiken auf eine kleinere Geräteoberfläche mit einer Pixelgröße gleich der Größe der Ansicht in geräteunabhängigen Einheiten. (Z. B. SkiaSharp würde eine Anzeigeoberfläche des 360 x 512 Pixel auf die Nexus 5 verwenden.) Klicken Sie dann wird das Bild oben Größe, was zu spürbaren Bitmap Jaggies skaliert.

Eine bessere Lösung werden, um die gleichen bildauflösung zu gewährleisten, Ihre eigenen einfache Funktionen zum Konvertieren zwischen zwei Koordinatensysteme zu schreiben.

Zusätzlich zu den `DrawCircle` Methode `SKCanvas` definiert außerdem zwei `DrawOval` Methoden, die eine Ellipse, die gezeichnet werden soll. Eine Ellipse wird durch zwei Radien statt eines einzelnen Radius definiert. Diese werden als bezeichnet den *wichtigen Radius* und *kleinere Radius*. Die `DrawOval` Methode zeichnet eine Ellipse mit zwei Radien, die parallel zu den X- und y-Achsen. Mit Transformationen oder die Verwendung eines Pfads Grafiken (um Sie später behandelt werden), kann diese Einschränkung umgehen, aber [dies `DrawOval` Methode](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) benennt das Argument zwei Radien `rx` und `ry` , um anzugeben, dass sie parallel zur sind die X- und y-Achsen:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Ist es möglich, eine Ellipse, die gezeichnet werden soll, die die Anzeigeoberfläche füllt? Die **Ellipse füllen** Seite wird veranschaulicht, wie. Die `PaintSurface` -Ereignishandler in der [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) Klasse subtrahiert halbe Breite der Kontur aus der `xRadius` und `yRadius` Werte für die gesamte Ellipse passen und die zugehörige innerhalb der Anzeigeoberfläche beschreiben:

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

Hier ist er auf den drei Plattformen ausgeführt:

[![](pixels-images/ellipsefill-small.png "Dreifacher Screenshot der Seite Oberfläche Größe")](pixels-images/ellipsefill-large.png#lightbox "dreifacher Screenshot der Seite Oberfläche Größe")

Die [andere `DrawOval` Methode](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) verfügt über eine [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) Argument, das ein Rechteck im Hinblick auf die X- und Y-Koordinaten der oberen linken Ecke und unteren rechten Ecke definiert ist. Die Ellipse füllt dieses Rechteck, was darauf hinweist, dass es möglich eventuell, verwenden Sie ihn in die **Ellipse füllen** Seite wie folgt:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Allerdings schneidet, die alle die Ränder der Kontur der Ellipse auf vier Seiten ab. Müssen Sie alle Anpassen der `SKRect` Konstruktorargumente basierend auf den `strokeWidth` dieses Werk rechten vornehmen:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
