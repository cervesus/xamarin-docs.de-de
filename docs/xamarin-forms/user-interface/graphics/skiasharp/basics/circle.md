---
title: Zeichnen eines einfachen Kreises in SkiaSharp
description: Dieser Artikel erläutert die Grundlagen von SkiaSharp-Zeichnung, einschließlich Leinwände und Paint, in Xamarin.Forms-Anwendungen, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e06a1310fad01da11c8d8b115df504cc19426344
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615222"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>Zeichnen eines einfachen Kreises in SkiaSharp

_Erlernen der Grundlagen von SkiaSharp-Zeichnungen, einschließlich Leinwände und paint_

In diesem Artikel erläutert die Konzepte der Zeichnen von Grafiken in Xamarin.Forms mithilfe von SkiaSharp, einschließlich der Erstellung einer `SKCanvasView` Objekt zum Hosten der Grafik, Verarbeitung der `PaintSurface` -Ereignis, und Verwenden einer `SKPaint` Objekt, das Angeben von Farbe und andere Funktionen zum Zeichnen Attribute.

Die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm enthält den Beispielcode für diese Reihe von SkiaSharp-Artikeln. Die erste Seite berechtigt ist **einfachen Kreises** und ruft die Seitenklasse [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Dieser Code zeigt, wie Sie das Zeichnen eines Kreises in der Mitte der Seite mit einem Radius von 100 Pixel. Die Gliederung des Kreises ist rot, und das Innere des Kreises ist Blau.

![](circle-images/circleexample.png "Ein blauer Kreis rot umrandet")

Die [ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Page-Klasse leitet sich von `ContentPage` und enthält zwei `using` Direktiven für die SkiaSharp-Namespaces:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Der folgende Konstruktor der Klasse erstellt eine [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) Objekt, fügt einen Handler für die [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) Ereignis und legt die `SKCanvasView` Objekt als Inhalt der Seite:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

Die `SKCanvasView` belegt den gesamten Bereich der Seite Inhalt. Sie können auch kombinieren, einem `SKCanvasView` mit anderen Xamarin.Forms `View` ableitungen, wie Sie sehen in anderen Beispielen.

Die `PaintSurface` -Ereignishandler ist, in dem sich alle Ihre Zeichnung sollen. Diese Methode wird in der Regel mehrere Male aufgerufen, während das Programm ausgeführt wird, damit sie alle Informationen zum neu erstellen verwalten soll der Grafiken anzeigen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

Die [ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) -Objekt, das das Ereignis verfügt über zwei Eigenschaften:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) Der Typ [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) Der Typ [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

Die `SKImageInfo` Struktur enthält Informationen über die Zeichenoberfläche, vor allem ist dies Breite und Höhe in Pixel. Die `SKSurface` Objekt darstellt, die Zeichenoberfläche selbst. In diesem Programm ist die Zeichenoberfläche ein video anzeigen, aber in anderen Programmen eine `SKSurface` Objekt kann auch repräsentieren eine Bitmap, mit denen Sie SkiaSharp gezeichnet werden soll.

Die wichtigste Eigenschaft von `SKSurface` ist [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) des Typs [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Diese Klasse ist eine Grafik Zeichnungskontext, die Sie verwenden, um die eigentlichen Zeichenvorgang führen. Die `SKCanvas` -Objekt kapselt einen Grafikzustand, einschließlich grafiktransformationen und Clipping.

Hier ist eine typische Anfang eine `PaintSurface` -Ereignishandler:

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

Die [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Methode löscht den Zeichenbereich durch eine transparente Farbe. Eine Überladung können Sie eine Hintergrundfarbe für die Canvas angegeben.

Das Ziel hierbei ist einen roten Kreis mit blau gefüllt zu zeichnen. Da dieser bestimmten Grafik zwei unterschiedliche Farben enthält, muss der Auftrag in zwei Schritten erfolgen. Der erste Schritt ist die Gliederung des Kreises zu zeichnen. Um die Farbe und andere Merkmal der Zeile anzugeben, erstellt und initialisiert ein [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) Objekt:

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

Die [ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Eigenschaft gibt an, dass Sie möchten *Stroke* einer Zeile (in diesem Fall die Gliederung des Kreises) statt *Füllung* inneren. Die drei Elemente von der [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) Enumeration lauten wie folgt:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Die Standardeinstellung ist `Fill`. Verwenden Sie die dritte Option, um die Linie zu zeichnen, und füllen das innere mit der gleichen Farbe.

Legen Sie die [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) Eigenschaft auf einen Wert vom Typ [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Eine Möglichkeit zum Abrufen einer `SKColor` Wert ist das Konvertieren einer Xamarin.Forms `Color` -Wert in ein `SKColor` Wert mithilfe der Erweiterungsmethode [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). Die [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) -Klasse in der `SkiaSharp.Views.Forms` Namespace enthält andere Methoden, die zwischen der Xamarin.Forms-Werte und SkiaSharp-Werte zu konvertieren.

Die [ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Eigenschaft gibt die Stärke der Linie an. Hier wird es auf 25 Pixel festgelegt.

Sie verwenden, die `SKPaint` Objekt, das den Kreis gezeichnet werden soll:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Koordinaten werden relativ zur linken oberen Ecke der Anzeigeoberfläche angegeben. X-Koordinaten Erhöhung auf der rechten Seite und Y-Koordinaten Erhöhung ausfällt. In der Diskussion über Grafiken häufig die mathematische Notation (X, y) verwendet, um einen Punkt anzugeben. Der Punkt (0, 0) der oberen linken Ecke der Anzeigeoberfläche und häufig aufgerufen wird die *Ursprung*.

Die ersten beiden Argumente der `DrawCircle` die X- und Y-Koordinaten für den Mittelpunkt des Kreises anzuzeigen. Diese werden halbe Breite und Höhe der Anzeigeoberfläche platzieren Sie den Mittelpunkt des Kreises in der Mitte der Anzeigeoberfläche zugewiesen. Das dritte Argument gibt den Radius des Kreises, und das letzte Argument ist der `SKPaint` Objekt.

Um das Innere des Kreises zu füllen, können Sie zwei Eigenschaften des Ändern der `SKPaint` Objekt, und rufen `DrawCircle` erneut aus. Dieser Code zeigt außerdem eine alternative Möglichkeit zum Abrufen einer `SKColor` Wert eines der vielen Felder von der [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) Struktur:

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

So sieht das Programm ausgeführt wird, unter iOS, Android und die universelle Windows-Plattform aus:

[![](circle-images/simplecircle-small.png "Dreifacher Screenshot der Seite einfachen Kreises")](circle-images/simplecircle-large.png#lightbox "dreifachen Screenshot der Seite einfachen Kreises")

Wenn das Programm selbst ausführen, können Sie aktivieren das Telefon oder den Simulator auf die Seite, um festzustellen, wie die Abbildung neu gezeichnet wird. Jedes Mal neu gezeichnet wird, die Grafik muss die `PaintSurface` -Ereignishandler erneut aufgerufen wird.

Ein `SKPaint` Objekt ist nicht viel mehr als eine Auflistung von Grafiken, die Eigenschaften zu zeichnen. Diese Objekte sind sehr einfach. Sie können wiederverwenden `SKPaint` Objekte wie dieses Programm ist, oder Sie können mehrere erstellen `SKPaint` Objekte für verschiedene Kombinationen von Eigenschaften zu zeichnen. Sie erstellen und initialisieren Sie diese Objekte außerhalb des dem `PaintSurface` -Ereignishandler, und Sie können sie speichern als Felder in der Page-Klasse.

Obwohl die Breite der Umriss des Kreises als 25 Pixeln angegeben wird &mdash; oder ein Viertel der den Radius des Kreises &mdash; anscheinend schlankere werden, und es gibt ein guter Grund dafür: halbe Breite der Zeile durch den blauen Kreis verdeckt ist. Die Argumente für die `DrawCircle` Methode der abstrakten geometrischen Koordinaten eines Kreises zu definieren. Das blaue innere ist auf diese Dimension auf den nächsten Pixel groß, aber die Gliederung 25 Pixel breiten überspannt geometrische Kreises &mdash; Hälfte auf der Innenseite und die andere Hälfte außerhalb.

Im folgenden Beispiel in der [Integrieren von Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) Artikel veranschaulicht dies visuell.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
