---
title: Pixel und geräteunabhängige Einheiten
description: In diesem Artikel werden die Unterschiede zwischen skiasharp-Koordinaten und- Xamarin.Forms Koordinaten erläutert und mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c1ae9e05b6671d45d8df485a89cfc0dea86632d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937305"
---
# <a name="pixels-and-device-independent-units"></a>Pixel und geräteunabhängige Einheiten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Untersuchen der Unterschiede zwischen skiasharp-Koordinaten und- Xamarin.Forms Koordinaten_

In diesem Artikel werden die Unterschiede im Koordinatensystem erläutert, das in skiasharp und verwendet wird Xamarin.Forms . Sie können Informationen abrufen, um zwischen den beiden Koordinatensystemen zu konvertieren und auch Grafiken zu zeichnen, die einen bestimmten Bereich ausfüllen:

![Ein Oval, das den Bildschirm füllt](pixels-images/screenfillexample.png)

Wenn Sie schon Xamarin.Forms seit einer Weile programmieren, haben Sie möglicherweise ein Gefühl für Xamarin.Forms Koordinaten und Größen. Die in den beiden vorherigen Artikeln gezeichneten Kreise scheinen etwas klein zu sein.

Diese Kreise *sind* im Vergleich zu den Xamarin.Forms Größen gering. Standardmäßig zeichnet skiasharp Einheiten von Pixeln, während Xamarin.Forms Koordinaten und Größen auf einer geräteunabhängigen Einheit basieren, die von der zugrunde liegenden Plattform festgelegt wird. (Weitere Informationen zum Xamarin.Forms Koordinatensystem finden Sie in [Kapitel 5. Umgang mit den Größen](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) des Buchs, das *Mobile Apps Xamarin.Forms mit erstellt *.)

Die Seite im Programm " [**schrägsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) " mit der Bezeichnung " **Surface size** " verwendet die Textausgabe "skiasharp", um die Größe der Anzeige Oberfläche aus drei verschiedenen Quellen anzuzeigen:

- Die normalen Xamarin.Forms [`Width`](xref:Xamarin.Forms.VisualElement.Width) -und- [`Height`](xref:Xamarin.Forms.VisualElement.Height) Eigenschaften des- `SKCanvasView` Objekts.
- Die- [`CanvasSize`](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize) Eigenschaft des- `SKCanvasView` Objekts.
- Die [`Size`](xref:SkiaSharp.SKImageInfo.Size) -Eigenschaft des `SKImageInfo` -Werts, die mit den Eigenschaften und übereinstimmt, die `Width` `Height` auf den beiden vorherigen Seiten verwendet werden.

Die- [`SurfaceSizePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Klasse zeigt, wie diese Werte angezeigt werden. Der-Konstruktor speichert das- `SKCanvasView` Objekt als-Feld, sodass im-Ereignishandler darauf zugegriffen werden kann `PaintSurface` :

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

`SKCanvas`enthält sechs verschiedene `DrawText` Methoden, aber diese [`DrawText`](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint)) Methode ist die einfachste Methode:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Sie geben die Text Zeichenfolge, die X-und Y-Koordinaten, an denen der Text beginnen soll, und ein- `SKPaint` Objekt an. Die X-Koordinate gibt an, wo die linke Seite des Texts positioniert ist, aber beachten Sie Folgendes: die Y-Koordinate gibt die Position der *Baseline* des Texts an. Wenn Sie jemals ein Whitepaper geschrieben haben, ist die Baseline die Zeile, in der sich die Zeichen befinden und unter welchen Nachfolger (z. b. die Buchstaben g, p, q und y) liegen.

Das `SKPaint` -Objekt ermöglicht es Ihnen, die Farbe des Texts, die Schriftfamilie und die Textgröße anzugeben. Standardmäßig hat die- [`TextSize`](xref:SkiaSharp.SKPaint.TextSize) Eigenschaft den Wert 12, was zu einem kleinen Text auf Geräten mit hoher Auflösung (z. b. Smartphones) führt. In allem, aber in den einfachsten Anwendungen, benötigen Sie auch einige Informationen über die Größe des Texts, den Sie anzeigen. Die `SKPaint` -Klasse definiert eine [`FontMetrics`](xref:SkiaSharp.SKPaint.FontMetrics) Eigenschaft und mehrere [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) Methoden, aber für weniger ausgefallene Anforderungen stellt die- [`FontSpacing`](xref:SkiaSharp.SKPaint.FontSpacing) Eigenschaft einen empfohlenen Wert für aufeinander folgende Textzeilen bereit.

Der folgende `PaintSurface` Handler erstellt ein- `SKPaint` Objekt für einen `TextSize` von 40 Pixeln, d. h. die gewünschte vertikale Höhe des Texts vom oberen Rand des aufsteiders bis zum unteren Rand der untergeordneten Elemente. Der `FontSpacing` Wert, den das `SKPaint` Objekt zurückgibt, ist etwas größer als das, ungefähr 47 Pixel.

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

Die-Methode beginnt die erste Textzeile mit einer X-Koordinate von 20 (für einen kleinen Rand auf der linken Seite) und eine Y-Koordinate von `fontSpacing` . Dies ist ein wenig mehr als das, was erforderlich ist, um die vollständige Höhe der ersten Textzeile am oberen Rand der Anzeige Oberfläche anzuzeigen. Nach jedem-Rückruf `DrawText` wird die Y-Koordinate um ein oder zwei Inkremente erhöht `fontSpacing` .

Dies ist das Programm, das ausgeführt wird:

[![Dreifacher Screenshot der Seite "Surface size"](pixels-images/surfacesize-small.png)](pixels-images/surfacesize-large.png#lightbox "Dreifacher Screenshot der Seite "Surface size"")

Wie Sie sehen, sind die `CanvasSize` -Eigenschaft der `SKCanvasView` -Eigenschaft und der- `Size` Eigenschaft des- `SKImageInfo` Werts konsistent, wenn die Pixel Dimensionen gemeldet werden. Die `Height` -Eigenschaft und die-Eigenschaft `Width` des `SKCanvasView` sind Xamarin.Forms Eigenschaften und melden die Größe der Ansicht in den geräteunabhängigen Einheiten, die von der Plattform definiert werden.

Der IOS-Simulator 7 auf der linken Seite verfügt über zwei Pixel pro Geräte unabhängiger Einheit, und das Android Nexus 5 in der Mitte verfügt über drei Pixel pro Einheit. Daher hat der zuvor gezeigte einfache Kreis unterschiedliche Größen auf verschiedenen Plattformen.

Wenn Sie lieber vollständig in geräteunabhängigen Einheiten arbeiten möchten, können Sie dies tun, indem Sie die- `IgnorePixelScaling` Eigenschaft von `SKCanvasView` auf festlegen `true` . Die Ergebnisse werden jedoch möglicherweise nicht angezeigt. Skiasharp rendert die Grafiken auf einer kleineren Geräteoberfläche mit einer Pixelgröße, die der Größe der Ansicht in geräteunabhängigen Einheiten entspricht. (Skiasharp würde z. b. eine Anzeige Oberfläche von 360 x 512 Pixel auf dem Nexus 5 verwenden.) Anschließend wird dieses Bild zentral hochskaliert, was zu merkbaren Bitmap-Jaggies führt.

Um die gleiche Bildauflösung aufrechtzuerhalten, empfiehlt es sich, eigene einfache Funktionen zu schreiben, um zwischen den beiden Koordinatensystemen zu konvertieren.

Zusätzlich zur- `DrawCircle` Methode `SKCanvas` definiert auch zwei Methoden, `DrawOval` die eine Ellipse zeichnen. Eine Ellipse wird durch zwei Radien anstelle eines einzelnen RADIUS definiert. Diese werden als *Haupt* -und *neben*Version bezeichnet. Die `DrawOval` -Methode zeichnet eine Ellipse mit den beiden Radien parallel zur X-und Y-Achse. (Wenn Sie eine Ellipse mit Achsen zeichnen müssen, die sich nicht parallel zur X-und Y-Achse befinden, können Sie eine Rotations Transformation verwenden, wie im Artikel [**Drehen der Transformation**](../transforms/rotate.md) oder eines Grafik Pfades erläutert, wie im Artikel [**drei Methoden zum Zeichnen eines Bogens**](../curves/arcs.md)erläutert). Diese Überladung der [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) -Methode benennt die beiden Radien `rx` -Parameter und `ry` zeigt an, dass Sie parallel zur X-und Y-Achse sind:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Ist es möglich, eine Ellipse zu zeichnen, die die Anzeige Oberfläche füllt? Die Auslassungs Seite für **Ellipse** veranschaulicht, wie Sie. Der `PaintSurface` -Ereignishandler in der [**EllipseFillPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) -Klasse subtrahiert die Hälfte der Strichbreite von den `xRadius` `yRadius` Werten und an die gesamte Ellipse und deren Kontur innerhalb der Anzeige Oberfläche:

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

[![Dreifacher Screenshot der Seite "Surface size"](pixels-images/ellipsefill-small.png)](pixels-images/ellipsefill-large.png#lightbox "Dreifacher Screenshot der Seite "Surface size"")

Die andere [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint)) Methode verfügt über ein [`SKRect`](xref:SkiaSharp.SKRect) Argument, bei dem es sich um ein Rechteck handelt, das in Bezug auf die X-und Y-Koordinaten der linken oberen Ecke und der unteren rechten Ecke definiert ist. Das Oval füllt dieses Rechteck aus. Dies deutet darauf hin, dass es möglicherweise möglich ist, es auf der **ausfüllseite der Ellipse** wie folgt zu verwenden:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Allerdings werden alle Ränder der Gliederung der Ellipse auf den vier Seiten abgeschnitten. Sie müssen alle `SKRect` Konstruktorargumente auf der Grundlage von anpassen `strokeWidth` , damit diese Aufgabe ordnungsgemäß funktioniert:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
