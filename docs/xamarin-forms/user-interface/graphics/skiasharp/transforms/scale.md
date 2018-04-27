---
title: Die Skalierungstransformation für die
description: Ermitteln Sie die SkiaSharp Skalierungstransformation für Objekte in verschiedenen Größen skalieren
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: b4a36e15bd5db72ef113748282175c6d31a95966
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="the-scale-transform"></a>Die Skalierungstransformation für die

_Ermitteln Sie die SkiaSharp Skalierungstransformation für Objekte in verschiedenen Größen skalieren_

Wie in der Sie gesehen haben [der übersetzen-Transformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) Artikel, die übersetzen-Transformation kann ein Objekt von einem Speicherort in einen anderen verschieben. Im Gegensatz dazu ändert die Skalierungstransformation die Größe des grafischen Objekts:

![](scale-images/scaleexample.png "Ein hoch Wort Größe skaliert")

Die Skalierungstransformation wird auch häufig Grafikkoordinaten zu verschieben, wenn sie größere bereitgestellt werden.

Zuvor haben Sie gesehen zwei Transformation Formeln, die beschreiben, die Auswirkungen der Übersetzung Authentifizierungsstufen `dx` und `dy`:

X "= X + Dx

y "= y + dy

Skalierungsfaktoren des `sx` und `sy` sind und nicht Additive multiplikative:

X "Sx · = x

y "Sy · = y

Die Standardwerte der übersetzen Faktoren sind 0; die Standardwerte für die Skalierungsfaktoren steht "1".

Die `SKCanvas` Klasse definiert vier `Scale` Methoden. Die erste [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) Methode ist für Fälle, in denen gewünschten dieselbe horizontale und vertikale Skalierung berücksichtigen:

```csharp
public void Scale (Single s)
```

Dies bezeichnet man *kugelstrahler* Skalierung &mdash; Skalierung also dasselbe in beide Richtungen. Kugelstrahler Skalierung wird das Objekt Seitenverhältnis beibehalten.

Die zweite [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) Methode können Sie verschiedene Werte für die horizontale und vertikale Skalierung angeben:

```csharp
public void Scale (Single sx, Single sy)
```

Dies führt zu *anisotrope* Skalierung.
Das dritte [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) -Methode kombiniert die beiden Skalierungsfaktoren in einer einzelnen `SKPoint` Wert:

```csharp
public void Scale (SKPoint size)
```

Das vierte `Scale` Methode wird in Kürze beschrieben werden.

Die **grundlegende Skalierung** veranschaulicht die `Scale` Methode. Die [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) XAML-Datei enthält zwei `Slider` Elemente, mit denen Sie wählen die horizontale und vertikale Skalierungsfaktoren zwischen 0 und 10. Die [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) Code-Behind-Datei verwendet diese Werte aufrufen `Scale` bevor durch eine gestrichelte Linie gezeichnet und angepasst Text in der linken oberen Ecke ein abgerundetes Rechteck anzeigen die Ecke des Zeichenbereichs:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Sie Fragen sich vielleicht: wie wirken sich die Skalierungsfaktoren auf den Rückgabewert aus dem `MeasureText` Methode `SKPaint`? Die Antwort lautet: gar nicht. `Scale` ist eine Methode `SKCanvas`. Es hat keine Auswirkungen auf alle Elemente mit vornehmen ein `SKPaint` Objekt, bis Sie dieses Objekt verwenden, um ein Element im Zeichenbereich zu rendern.

Wie Sie, alle Elemente, die nach dem sehen die `Scale` erhöht sich proportional aufrufen:

[![](scale-images/basicscale-small.png "Dreifacher Screenshot der Seite grundlegende Skalierung")](scale-images/basicscale-large.png#lightbox "dreifacher Screenshot der Seite grundlegende Skalierung")

Der Text die Breite der gestrichelte Linie, die Länge der Bindestriche in dieser Zeile, die Rundung von den Ecken und der 10-Pixel-Rand zwischen dem linken und oberen Randes des Zeichenbereichs und des abgerundeten Rechtecks unterliegen alle den gleichen Skalierungsfaktoren.

> [!IMPORTANT]
> Die universelle Windows-Plattform wird nicht ordnungsgemäß anisotropicly skalierten Text gerendert.

Anisotrope Skalierung Ursachen Strichbreite anderen Zeilen werden mit der horizontalen und vertikalen Achsen ausgerichtet wird. (Dies ist auch aus das erste Bild auf dieser Seite.) Wenn Sie nicht möchten, dass die Strichbreite von Skalierung Faktoren beeinflusst werden, legen Sie es auf 0 und es ist immer einem Pixel Breite unabhängig von der `Scale` Einstellung.

Skalierung ist relativ zu der oberen linken Ecke des Zeichenbereichs. Dies ist möglicherweise genau erwünscht, aber möglicherweise nicht. Angenommen, Sie möchten zum Positionieren von Text und Rechteck, die an anderer Stelle im Zeichenbereich, und Sie möchten, um relativ zu ihren Mittelpunkt zu skalieren. In diesem Fall können Sie die vierte Version der [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) -Methode, die umfasst zwei zusätzliche Parameter, um den Mittelpunkt der Skalierung anzugeben:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

Die `px` und `py` Parameter definiert keinen Punkt, die manchmal aufgerufen wird die *Skalierung Center* jedoch in der SkiaSharp Dokumentation genannt wird eine *Pivotieren Punkt*. Dies ist ein Punkt relativ zur linken oberen Ecke neben dem Zeichenbereich angezeigt, die von der Skalierung nicht betroffen ist. Alle Skalierung erfolgt relativ zu diesem Center.

Die [ **zentriert Skalierung** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) Seite zeigt, wie dies funktioniert. Die `PaintSurface` Handler ähnelt der **grundlegende Skalierung** programmieren, außer dass die `margin` den Text Horizontal zentrieren Wert berechnet wird, was bedeutet, dass das Programm am besten im Hochformat funktioniert:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Der oberen linken Ecke des abgerundeten Rechtecks positioniert ist `margin` Pixel vom linken Rand des Zeichenbereichs und `margin` Pixel vom oberen Rand. Die beiden letzten Argumente an die `Scale` Methode werden auf diese Werte sowie die Breite und Höhe des Texts, der die Breite und Höhe des abgerundeten Rechtecks auch ist festgelegt. Dies bedeutet, dass alle Skalierung relativ zum Mittelpunkt des dieses Rechteck ist:

[![](scale-images/centeredscale-small.png "Dreifacher Screenshot der Seite zentriert Skalierung")](scale-images/centeredscale-large.png#lightbox "dreifacher Screenshot der Seite zentriert Skalierung")

Die `Slider` Elemente an diesem Programm haben einen Bereich von &ndash;10 bis 10. Wie Sie sehen können, dazu führen, dass negative Werte vertikale Skalierung (z. B. Bildschirm auf das Android-Geräten in der Mitte) Objekte aus, um die horizontale Achse zu blättern, die den Mittelpunkt der Skalierung durchlaufen. Negative Werte der horizontalen Skalierung (z. B. wie die uwp-Bildschirm auf der rechten Seite) dazu führen, dass Objekte, um die vertikale Achse zu blättern, die den Mittelpunkt der Skalierung durchlaufen.

Dieser vierte Version der `Scale` Methode ist tatsächlich eine Verknüpfung. Möglicherweise möchten Sie sehen, wie dies durch Ersetzen funktioniert die `Scale` Methode in diesem Code durch Folgendes:

```csharp
canvas.Translate(-px, -py);
```

Hierbei handelt es sich um die negative der Pivot-Punktkoordinaten.

Führen Sie nun das Programm erneut aus. Sie sehen, dass das Rechteck und Text, dass sich im Center in der oberen linken Ecke des Zeichenbereichs verschoben werden. Sie können es kaum sehen. Der Schieberegler funktioniert nicht natürlich, da nun die Anwendung überhaupt skalieren nicht.

Fügen Sie jetzt die grundlegenden `Scale` aufrufen (ohne eine Skalierung Center) *vor* , `Translate` aufrufen:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Wenn Sie sich mit vertraut ist dieser Übung in anderen Grafikprogrammierung Systeme Sie vielleicht denken, dass falsche ist jedoch nicht. Skia behandelt Transformation aufeinander folgende Aufrufe etwas anders aus, was Sie mit vertraut sein könnte.

Mit den nachfolgenden `Scale` und `Translate` Aufrufe, die Mitte des abgerundeten Rechtecks ist, weiterhin in der oberen linken Ecke, aber Sie können es jetzt skalieren, relativ zur linken oberen Ecke des Zeichenbereichs, der auch das Zentrum des abgerundeten Rechtecks ist.

Jetzt, bevor der `Scale` Aufruf Hinzufügen einer anderen `Translate` mit den Werten zentrieren aufrufen:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Dadurch wird das skalierte Ergebnis wieder auf die ursprüngliche Position verschoben. Diese drei Aufrufe entsprechen:

```csharp
canvas.Scale(sx, sy, px, py);
```

Die einzelnen Transformationen sind verschärft, damit die Formel für den gesamten Transformation ist:

 X "Sx · = (X – px) + px

 y "Sy · = (y – Py) + Py

Beachten Sie, dass die Standardwerte der `sx` und `sy` steht "1". Es ist einfach, sich davon überzeugen, dass Pivotpunkt (px, Py) nicht durch diese Formeln transformiert wird. Er verbleibt am gleichen Speicherort relativ zu den Zeichenbereich.

Wenn Sie kombinieren `Translate` und `Scale` Aufrufe, die Reihenfolge wichtig ist. Wenn die `Translate` kommt nach der `Scale`, die Übersetzung Faktoren effektiv mit der Skalierungsfaktoren skaliert werden. Wenn die `Translate` kommt vor der `Scale`, die Übersetzung Faktoren werden nicht skaliert. Dieser Vorgang wird etwas klarer (obwohl mathematisches) bei der Betreff der Transformation Matrizen eingeführt wird.

Die `SKPath` Klasse definiert ein schreibgeschütztes [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) Eigenschaft, die zurückgibt ein `SKRect` definieren das Ausmaß der Koordinaten im Pfad. Z. B., wenn die `Bounds` Eigenschaft abgerufen wird, aus der zuvor erstellte Hendecagram-Pfad der `Left` und `Top` Eigenschaften des Rechtecks sind – ca. 100 die `Right` und `Bottom` Eigenschaften sind ungefähr 100, und die `Width` und `Height` Eigenschaften sind ungefähr 200. (Die meisten die tatsächlichen Werte sind eine etwas kleiner, da die Punkte der Sterne durch einen Kreis mit einem Radius von 100 definiert sind, aber nur über der obere Punkt die horizontalen oder vertikalen Achsen parallel ist.)

Die Verfügbarkeit dieser Informationen impliziert, dass es möglich, Skalierung und übersetzen Faktoren für einen Pfad auf die Größe des Zeichenbereichs Skalierung geeignet sein sollte. Die [ **anisotrope Skalierung** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) Seite wird dies veranschaulicht, mit dem Stern 11 gezeigt wird. Ein *anisotrope* Skalierung bedeutet, dass es in die horizontal und vertikal um ungleich was bedeutet, dass die Stern das ursprüngliche Seitenverhältnis beibehalten wird nicht. Der relevante Code sieht der `PaintSurface` Ereignishandler:

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Die `pathBounds` Rechteck am oberen Rand dieser Code abgerufen und dann später verwendet werden, mit der Breite und Höhe des Zeichenbereichs in der `Scale` aufrufen. Dass der Aufruf selbst die Koordinaten des Pfads skaliert werden, wenn er vom gerendert wird die `DrawPath` Aufruf, der den Stern werden in der oberen rechten Ecke des Zeichenbereichs zentriert. Es muss nach unten und nach links verschoben werden sollen. Dies ist die Aufgabe von der `Translate` aufrufen. Diese zwei Eigenschaften des `pathBounds` – ca. 100 sind, damit die Übersetzung Faktoren ca. 100 sind. Da die `Translate` Aufruf ist nach der `Scale` aufrufen, diese Werte werden durch die Skalierungsfaktoren effektiv skaliert, damit sie die Mitte des Sterns in der Mitte des Zeichenbereichs verschieben:

[![](scale-images/anisotropicscaling-small.png "Dreifacher Screenshot der Seite anisotrope Skalierung")](scale-images/anisotropicscaling-large.png#lightbox "dreifacher Screenshot der Seite anisotrope Skalierung")

Eine weitere Möglichkeit, Sie können überlegen, der `Scale` und `Translate` Aufrufe ist, um die Auswirkung in umgekehrter Reihenfolge zu bestimmen: die `Translate` Aufruf verlagert den Pfad an, weshalb es vollständig sichtbar ist, aber in der oberen linken Ecke des Zeichenbereichs ausgerichtet ist. Die `Scale` Methode setzt diese Stern größere relativ zur linken oberen Ecke.

Tatsächlich wird es, dass die Stern etwas größer als im Zeichenbereich angezeigt. Das Problem ist die Konturbreite. Die `Bounds` Eigenschaft `SKPath` gibt an, die Dimensionen der Koordinaten codiert, in dem Pfad, und was die Anwendung verwendet, um sie zu skalieren. Wenn der Pfad mit einer bestimmten Strichbreite gerendert wird, ist der gerenderte Pfad größer als im Zeichenbereich.

Zum Beheben dieses Problems müssen Sie für diese compensate. Ein einfacher Ansatz an diesem Programm ist die folgende Anweisung unmittelbar vor dem Hinzufügen der `Scale` aufrufen:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Dadurch erhöht sich die `pathBounds` Rechteck von 1,5 Einheiten auf allen vier Seiten. Dies ist eine angemessene Lösung nur, wenn der Join Strich gerundet wird. Ein Join Spitz kann nicht länger und sind schwer zu berechnen.

Sie können auch eine ähnliche Technik mit Text, als die **anisotrope Text** veranschaulicht. Hier wird der relevante Teil der `PaintSurface` Handler aus der [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) Klasse:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Ähnliche Logik ist, und der Text wird erweitert, auf die Größe der Seite basierend auf Grenzen textrechtecks Merry `MeasureText` (Dies ist ein bisschen größer als der tatsächliche Text):

[![](scale-images/anisotropictext-small.png "Dreifacher Screenshot der anisotrope Testseite")](scale-images/anisotropictext-large.png#lightbox "dreifacher Screenshot der anisotrope Testseite")

Wenn Sie das Seitenverhältnis des grafische Objekte beibehalten müssen, müssen Sie kugelstrahler Skalierung verwenden möchten. Die **Kugelstrahler Skalierung** Seite zeigt dies für den Stern 11 gezeigt wird. Im Prinzip sind die Schritten zum Anzeigen von einem Objekt in der Mitte der Seite mit kugelstrahler Skalierung:

- Übersetzt das Zentrum des dem Objekt auf der linken oberen Ecke.
- Das Objekt, das basierend auf der minimale Wert der horizontalen und vertikalen Seitengrößen dividiert durch die Objekt-Dimensionen zu skalieren.
- Die skalierte Objekte in die Mitte der Seite zu übersetzen.

Die [ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) führt folgende Schritte aus in umgekehrter Reihenfolge vor dem Sternchen anzeigen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Der Code zeigt auch die Stern zehn weitere Male, jedes Mal, verringern die Skalierung Faktor von 10 % und die Farbe progressiv von Rot in Blau zu ändern:

[![](scale-images/isotropicscaling-small.png "Dreifacher Screenshot der Seite Skalierung Kugelstrahler")](scale-images/isotropicscaling-large.png#lightbox "dreifacher Screenshot der Seite Kugelstrahler Skalierung")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
