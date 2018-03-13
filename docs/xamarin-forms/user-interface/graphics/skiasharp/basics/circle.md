---
title: Zeichnen eines Kreises einfache
description: "Grundlagen der SkiaSharp zeichnen, einschließlich explizite und paint"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 5b09621f1d3a24f8061e5cd6551dd85ce93e36e3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="drawing-a-simple-circle"></a>Zeichnen eines Kreises einfache

_Grundlagen der SkiaSharp zeichnen, einschließlich explizite und paint_

In diesem Artikel erläutert die Konzepte der Grafiken in SkiaSharp, einschließlich der Erstellung mit Xamarin.Forms ein `SKCanvasView` Objekt, das die Grafiken, die Behandlung von host der `PaintSurface` -Ereignisses und die Verwendung einer `SKPaint` Objekt, um Farb- und anderen Zeichnung anzugeben Attribute.

Die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) Programm enthält alle Beispielcode für diese Reihe von Artikeln SkiaSharp. Die erste Seite berechtigt ist **einfacher Kreis** und ruft die Seitenklasse [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Dieser Code veranschaulicht das Zeichnen eines Kreises in der Mitte der Seite mit einem Radius von 100 Pixel. Die Gliederung des Kreises ist rot, und das Innere des Kreises ist Blau.

![](circle-images/circleexample.png "Einem blauen Kreis rot umrandet")

Die [ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Page-Klasse abgeleitet `ContentPage` und enthält zwei `using` Direktiven für die SkiaSharp-Namespaces:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Der folgende Konstruktor der Klasse erstellt eine [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) Objekt, fügt einen Handler für das [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) Ereignisses, und legt die `SKCanvasView` Objekt als Inhalt der Seite:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

Die `SKCanvasView` belegt den gesamten Inhaltsbereichs der Seite. Sie können alternativ kombinieren einer `SKCanvasView` mit anderen Xamarin.Forms `View` ableitungen, wie Sie sehen in anderen Beispielen.

Die `PaintSurface` -Ereignishandler ist in dem sich alle Ihre Zeichnung sollen. Diese Methode wird in der Regel mehrere Male aufgerufen, während das Programm ausgeführt wird, damit alle Informationen, die dann zum erneuten Erstellen erforderlichen verwalten soll die Grafiken anzuzeigen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

Die [ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) -Objekt, das das Ereignis begleitet verfügt über zwei Eigenschaften:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) vom Typ [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) vom Typ [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

Die `SKImageInfo` Struktur enthält Informationen über die Zeichenoberfläche, vor allem, Breite und Höhe in Pixel. Die `SKSurface` -Objekt stellt die Zeichenoberfläche selbst dar. In diesem Programm wird die Zeichenoberfläche eine Videoanzeige aber in anderen Programmen ein `SKSurface` Objekt kann auch ein Bitmuster, mit denen Sie SkiaSharp Zeichnen auf darstellen.

Die wichtigste Eigenschaft der `SKSurface` ist [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) des Typs [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Diese Klasse ist eine Zeichnung Kontext, mit denen Sie die tatsächliche Zeichnung zu erstellen. Die `SKCanvas` -Objekt kapselt einen Grafikzustand, die Transformationen für Grafiken und Clipping umfasst.

Hier ist eine typische Anfang einer `PaintSurface` Ereignishandler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

Die [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Methode löscht den Zeichenbereich durch eine transparente Farbe. Eine Überladung können Sie eine Hintergrundfarbe für den Zeichenbereich angeben.

Das Ziel wird einen roten Kreis mit blau gefüllt gezeichnet werden soll. Da dieser bestimmten Grafik zwei unterschiedliche Farben enthält, muss der Auftrag in zwei Schritten erfolgen. Im ersten Schritt wird die Gliederung des Kreises gezeichnet werden soll. Um die Farbe und andere Merkmale der Zeile anzugeben, erstellen und initialisieren Sie ein [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) Objekt:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

Die [ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Eigenschaft gibt an, dass Sie möchten *Strich* einer Zeile (in diesem Fall die Gliederung des Kreises) statt *Füllung* das innere. Die drei Elemente von der [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) Enumeration lauten wie folgt:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Die Standardeinstellung ist `Fill`. Verwenden Sie die dritte Option, die Linie zu zeichnen, und füllen das innere mit der gleichen Farbe.

Legen Sie die [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) Eigenschaft auf einen Wert vom Typ [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Eine Möglichkeit zum Abrufen einer `SKColor` der Wert ist, konvertieren eine Xamarin.Forms `Color` -Wert an ein `SKColor` Wert mit der Erweiterungsmethode [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). Die [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) -Klasse in der `SkiaSharp.Views.Forms` Namespace enthält andere Methoden, die zwischen Werten von Xamarin.Forms und SkiaSharp konvertiert.

Die [ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Eigenschaft gibt die Stärke der Linie an. Hier ist er auf 25 Pixel festgelegt.

Sie verwenden, die `SKPaint` Objekt zum Zeichnen des Kreises:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Koordinaten werden relativ zur linken oberen Ecke der Anzeigeoberfläche angegeben. X-Koordinaten Erhöhung rechts bzw. Y-Koordinaten Erhöhung ausfällt. In der Diskussion über Grafiken die mathematische Schreibweise (X, y) wird häufig um einen Punkt zu kennzeichnen. Der Punkt (0, 0) wird von der linken oberen Ecke der Anzeigeoberfläche und wird häufig aufgerufen, die *Ursprung*.

Die ersten beiden Argumente der `DrawCircle` die X- und Y-Koordinaten für den Mittelpunkt des Kreises anzuzeigen. Diese werden halbe Breite und Höhe der Anzeigeoberfläche in der Mitte der Anzeigeoberfläche den Mittelpunkt des Kreises versetzt zugewiesen. Das dritte Argument gibt den Radius des Kreises, und das letzte Argument ist der `SKPaint` Objekt.

Um das Innere eines Kreises zu füllen, können Sie zwei Eigenschaften ändern die `SKPaint` Objekt, und rufen `DrawCircle` erneut aus. Dieser Code zeigt auch eine alternative Möglichkeit zum Abrufen einer `SKColor` Wert aus einer viele Felder die [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) Struktur:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Dieses Mal die `DrawCircle` Aufruf füllt den Kreis, der mithilfe der neuen Eigenschaften der `SKPaint` Objekt.

Hier wird das Programm auf iOS-, Android- und universellen Windows-Plattform ausgeführt wird:

[![](circle-images/simplecircle-small.png "Dreifacher Screenshot der Seite einfacher Kreis")](circle-images/simplecircle-large.png#lightbox "dreifacher Screenshot der Seite einfacher Kreis")

Wenn das Programm selbst ausführen, können Sie Aktivieren der Phone oder im Simulator seitlich ausgerichtet, um anzuzeigen, wie die Grafik neu gezeichnet wird. Jedes Mal neu gezeichnet wird, die Grafik muss die `PaintSurface` Ereignishandler erneut aufgerufen wird.

Ein `SKPaint` Objekt ist etwas mehr als eine Auflistung von Eigenschaften zu zeichnen. Diese Objekte sind sehr einfach. Sie können wiederverwenden `SKPaint` Objekte wie dieses Programm ist, oder Sie können mehrere erstellen `SKPaint` Objekte für verschiedene Kombinationen von Eigenschaften zu zeichnen. Sie erstellen und Initialisieren von diesen Objekten außerhalb von der `PaintSurface` -Ereignishandler, und Sie können diese speichern als Felder in der Page-Klasse.

Obwohl die Breite der Kontur des Kreises als 25 Pixel & #x 2014; angegeben wird oder ein Viertel des Radius der Kreis & #x 2014; Sie wird schlankere sein, und es gibt ein guter Grund für diesen: halbe Breite der Linie durch die blauen Kreis verdeckt wird. Die Argumente für die `DrawCircle` Methode definieren, die abstrakte geometrischen Koordinaten Umfang eines Kreises. Das blaue innere wird auf diese Dimension auf das nächstliegende Pixel angepasst, aber die Kontur 25 Pixel breiten Betrieb verläuft die geometrische Kreis & #x 2014; die Hälfte auf interne und die Hälfte an der Außenseite.

Im nächsten Beispiel in der [Integration mit Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) Artikel wird dies visuell veranschaulicht.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
