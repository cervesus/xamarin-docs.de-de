---
title: Die Skalierungstransformation
description: Verkaufsrabatte Artikel untersucht die Skalierungstransformation SkiaSharp, für die Skalierung von Objekten in verschiedenen Größen und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 2e9259bed6ad0ae5a926cb75ea74c1f379897220
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649287"
---
# <a name="the-scale-transform"></a>Die Skalierungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Ermitteln Sie die Skalierungstransformation SkiaSharp, für die Skalierung von Objekten in verschiedenen Größen_

Wie in der Sie gesehen haben [ **das Übersetzen transformieren** ](translate.md) Artikel die Verschiebungstransformation kann ein grafisches Objekts von einem Speicherort in einen anderen verschieben. Im Gegensatz dazu ändert die Skalierungstransformation die Größe des grafischen Objekts:

![](scale-images/scaleexample.png "Eine hohe Word Größe skaliert")

Die Skalierungstransformation bewirkt, dass oft auch Grafikkoordinaten verschieben, wenn sie größere vorgenommen werden.

Sie gesehen haben zwei Transformation Formeln, die beschreiben, die Auswirkungen der Übersetzung Faktor `dx` und `dy`:

X' = X + Dx

y' = y + dy

Skalierungsfaktoren des `sx` und `sy` multiplikative additiv sind:

X' = Sx – x

y' = Sy – y

Die Standardwerte der übersetzen Faktoren sind 0; die Standardwerte für die Skalierungsfaktoren sind 1.

Die `SKCanvas` Klasse definiert vier `Scale` Methoden. Die erste [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single)) Methode ist für Fälle, in denen Sie dieselbe horizontale und vertikale Skalierung möchten berücksichtigen:

```csharp
public void Scale (Single s)
```

Dies bezeichnet man als *kugelstrahler* Skalierung &mdash; Skalierung, dasselbe in beide Richtungen. Kugelstrahler Skalierung wird das Seitenverhältnis des Objekts beibehalten.

Die zweite [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) Methode können Sie unterschiedliche Werte für die horizontale und vertikale Skalierung angeben:

```csharp
public void Scale (Single sx, Single sy)
```

Dies führt zu *anisotrope* skalieren.
Die dritte [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) Methode kombiniert die beiden Skalierungsfaktoren in einem einzelnen `SKPoint` Wert:

```csharp
public void Scale (SKPoint size)
```

Der vierte `Scale` Methode wird in Kürze beschrieben.

Die **einfache Skalierung** Seite zeigt die `Scale` Methode. Die [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) -Datei enthält zwei `Slider` Elemente, mit denen Sie wählen die horizontale und vertikale Skalierungsfaktoren zwischen 0 und 10. Die [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) Code-Behind-Datei verwendet diese Werte aufrufen `Scale` vor der Anzeige eines abgerundeten Rechtecks mit Strichen dargestellt, mit einer gestrichelten Linie und Text in der linken oberen Ecke passt die Größe die Ecke des Zeichenbereichs:

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

Vielleicht Fragen Sie sich Folgendes: Wie wirken sich die Skalierungsfaktoren auf den von der `MeasureText` -Methode `SKPaint`von zurückgegebenen Wert aus? Die Antwort lautet: Nein. `Scale` ist eine Methode `SKCanvas`. Es hat keine Auswirkungen auf Alles was Sie tun, mit einem `SKPaint` Objekt, bis Sie das Objekt verwenden, um etwas auf der Leinwand zu rendern.

Wie Sie alle Elemente, die nach dem sehen können die `Scale` erhöht sich proportional aufrufen:

[![](scale-images/basicscale-small.png "Dreifacher Screenshot der Seite für einfache Skalierung")](scale-images/basicscale-large.png#lightbox "dreifachen Screenshot der Seite für einfache Skalierung")

Der Text die Breite der gestrichelten Linie, die Länge der Bindestriche in dieser Zeile, die runden der Ecken und der 10-Pixel-Rand zwischen dem linken und oberen Rand der Leinwand und abgerundeten Rechtecks unterliegen alle die gleiche Skalierungsfaktoren.

> [!IMPORTANT]
> Die universelle Windows-Plattform anisotropicly skalierten Text nicht ordnungsgemäß gerendert.

Anisotrope Skalierung bewirkt, dass Sie die Strichbreite für Zeilen unterschiedlich sind, die mit der horizontalen und vertikalen Achsen ausgerichtet ist. (Dies ist auch auf der ersten Abbildung auf dieser Seite deutlich zu sehen.) Wenn Sie nicht möchten, dass die Strichbreite von die Skalierungsfaktoren betroffen sind, legen Sie ihn auf 0 und es ist immer einem Pixel Breite unabhängig von der `Scale` festlegen.

Skalierung ist relativ zu der oberen linken Ecke des Zeichenbereichs. Dies ist möglicherweise genau erwünscht, aber möglicherweise nicht. Angenommen Sie, Sie Text und Rechteck, die an anderer Stelle auf der Leinwand positionieren möchten, und es relativ zu seinen Mittelpunkt skaliert werden soll. In diesem Fall können Sie die vierte Version von der [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) -Methode, die umfasst zwei zusätzliche Parameter, um den Mittelpunkt der Skalierung anzugeben:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

Die `px` und `py` Parameter definieren, einen Punkt, der manchmal auch aufgerufen wird, die *Skalierung Center* jedoch in der SkiaSharp Dokumentation genannt wird eine *Pivotpunkts*. Dies ist ein Punkt relativ zu der oberen linken Ecke des Zeichenbereichs, die nicht von der Skalierung betroffen ist. Gesamte Skalierung tritt auf, Bezug auf dieses Zentrum.

Die [ **zentriert Skalierung** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) Seite zeigt, wie dies funktioniert. Die `PaintSurface` Handler ist vergleichbar mit der **einfache Skalierung** programmieren, außer dass die `margin` Wert wird berechnet, den Text horizontal zentriert. Dies bedeutet, dass die Anwendung am besten im Hochformat funktioniert:

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

Befindet sich der linke obere Ecke des abgerundeten Rechtecks `margin` Pixel vom linken Rand der Leinwand und `margin` Pixel vom oberen Rand. Die beiden letzten Argumente für die `Scale` Methode werden auf diese Werte sowie die Breite und Höhe des Texts, handelt es sich auch die Breite und Höhe des abgerundeten Rechtecks festgelegt. Dies bedeutet, dass alle Skalierung Bezug auf den Mittelpunkt dieses Rechtecks:

[![](scale-images/centeredscale-small.png "Dreifacher Screenshot der Seite zentriert Skalierung")](scale-images/centeredscale-large.png#lightbox "dreifachen Screenshot der Seite zentriert skalieren")

Die `Slider` Elemente in diesem Programm haben einen Bereich von &ndash;10 bis 10. Wie Sie sehen können, dazu führen, dass negative Werte, der vertikale Skalierung (z. B. in der Mitte auf der Android Bildschirm "") Objekte, um die horizontale Achse kippen, die den Mittelpunkt der Skalierung durchlaufen. Negative Werte, der die horizontale Skalierung (z. B. in der UWP-Bildschirm, auf der rechten Seite) dazu führen, dass Objekte, um die vertikale Achse kippen, die den Mittelpunkt der Skalierung durchlaufen.

Die Version der [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) -Methode mit pivotpunkte ist eine Abkürzung für eine Reihe von drei `Translate` und `Scale` aufrufen. Möglicherweise möchten Sie sehen, wie dies durch Ersetzen funktioniert die `Scale` -Methode in der die **zentriert Skalierung** Seite mit den folgenden:

```csharp
canvas.Translate(-px, -py);
```

Hierbei handelt es sich um die negative, der die Punktkoordinaten Pivot.

Führen Sie nun das Programm erneut aus. Sie sehen, dass das Rechteck und der Text, dass sich das Center in der oberen linken Ecke des Zeichenbereichs verschoben werden. Sie können es kaum sehen. Der Schieberegler nicht natürlich verwendet werden, da Sie nun die Anwendung überhaupt nicht skaliert werden kann.

Fügen Sie nun die grundlegende `Scale` aufrufen (ohne eine Skalierung Center) *vor* , `Translate` aufrufen:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Wenn Sie sich mit vertraut ist dieser Übung in anderen Grafikprogrammierung Systeme Sie vielleicht denken, dass dies falsch ist, aber nicht. Skia behandelt die Transformation von aufeinander folgenden Aufrufen etwas anders aus, was Sie möglicherweise kennen.

Mit den nachfolgenden `Scale` und `Translate` Aufrufe die Mitte des abgerundeten Rechtecks befindet sich noch in der oberen linken Ecke, aber Sie können es jetzt skalieren, relativ zu der oberen linken Ecke des Zeichenbereichs, handelt es sich auch die Mitte des abgerundeten Rechtecks.

Nun davor `Scale` aufzurufen, fügen Sie ein weiteres `Translate` mit den Werten zentrieren aufrufen:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Dadurch wird das Ergebnis der skalierte zurück auf die ursprüngliche Position verschoben. Diese drei Aufrufe entsprechen:

```csharp
canvas.Scale(sx, sy, px, py);
```

Die einzelnen Transformationen sind noch verstärkt, damit die gesamte Transformation Formel lautet:

 X' = Sx – (X – px) + px

 y' = Sy – (j – Py) + Py

Beachten Sie, dass die Standardwerte der `sx` und `sy` 1 sind. Es ist einfach, sich selbst davon überzeugen, dass der Dreh-und Angelpunkt (px, Py) nicht durch diese Formeln transformiert wird. Es verbleibt am gleichen Speicherort relativ zur Leinwand.

Wenn Sie kombinieren `Translate` und `Scale` Aufrufe, die Reihenfolge wichtig ist. Wenn die `Translate` kommt nach der `Scale`, die Übersetzung Faktoren sind effektiv skaliert, indem die Skalierungsfaktoren. Wenn die `Translate` kommt vor dem `Scale`, die Übersetzung Faktoren nicht skaliert. Dieser Vorgang wird etwas klarer (obgleich mehr mathematische) bei der Betreff der Transformation-Matrizen eingeführt wird.

Die `SKPath` -Klasse definiert eine schreibgeschützte [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds) Eigenschaft, die zurückgibt ein `SKRect` definieren den Umfang der die Koordinaten im Pfad. Z. B., wenn die `Bounds` Eigenschaft wird abgerufen, von dem zuvor erstellten Hendecagram-Pfad der `Left` und `Top` Eigenschaften des Rechtecks werden – etwa 100 der `Right` und `Bottom` Eigenschaften sind ungefähr 100 an, und die `Width` und `Height` Eigenschaften sind ungefähr 200. (Die meisten der tatsächlichen Werte sind ein wenig kleiner, da nur die obersten Punkts mit die horizontalen oder vertikalen Achsen ist jedoch die Punkte der Sterne werden durch einen Kreis mit einem Radius von 100 definiert.)

Die Verfügbarkeit dieser Informationen wird impliziert, dass es möglich, Skalierung und Übersetzen der Faktoren, die für die Skalierung eines Pfads zu die Größe des Zeichenbereichs geeignet sein sollte. Die [ **anisotrope Skalierung** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) Seite wird dies veranschaulicht, mit dem Stern 11 zwischengespeichert. Ein *anisotrope* Skalierung bedeutet, dass es ungleich in horizontaler und vertikaler Richtung, was bedeutet, dass das Sternsymbol das ursprüngliche Seitenverhältnis beibehalten wird nicht. Hier ist der entsprechende Code in die `PaintSurface` Handler:

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

Die `pathBounds` Rechteck am oberen Rand dieser Code abgerufen, und klicken Sie dann später verwendet werden, mit der Breite und Höhe des Zeichenbereichs in der `Scale` aufrufen. Dass der Aufruf selbst skaliert werden kann, die Koordinaten des Pfads, nach dem Rendern durch die `DrawPath` Aufruf jedoch das Sternsymbol wird in der Ecke oben rechts im Zeichenbereich zentriert. Nach unten und nach links verschoben werden muss. Dies ist die Aufgabe der `Translate` aufrufen. Diese beiden Eigenschaften des `pathBounds` sind ungefähr – 100, also die Übersetzung Faktoren etwa 100. Da die `Translate` Aufruf ist nach der `Scale` aufrufen, diese Werte werden durch die Skalierungsfaktoren effektiv skaliert, damit sie die Mitte des Sterns in der Mitte der Canvas zu verschieben:

[![](scale-images/anisotropicscaling-small.png "Dreifacher Screenshot der Seite anisotrope Skalierung")](scale-images/anisotropicscaling-large.png#lightbox "dreifachen Screenshot der Seite anisotrope Skalierung")

Eine andere Möglichkeit, die `Scale` Aufrufe von und `Translate` zu überprüfen, besteht darin, die Auswirkung in umgekehrter Reihenfolge zu bestimmen: Der `Translate` -Befehl verschiebt den Pfad so, dass er vollständig sichtbar und in der oberen linken Ecke des Zeichen Bereichs ausgerichtet wird. Die `Scale` Methode dann vornimmt, Stern größere relativ zur oberen linken Ecke.

Tatsächlich wird es angezeigt, dass das Sternchen ein bisschen größer als die Leinwand ist. Das Problem ist die Strichbreite. Die `Bounds` Eigenschaft `SKPath` gibt an, die Dimensionen der Koordinaten codiert, in dem Pfad, und was die Anwendung verwendet, um sie zu skalieren. Wenn der Pfad mit einer Strichbreite von bestimmten gerendert wird, ist der gerenderte Pfad größer als der Leinwand.

Zur Behebung dieses Problems müssen Sie dafür zu kompensieren. Ein einfacher Ansatz an diesem Programm ist beispielsweise die folgende Anweisung direkt vor hinzufügen der `Scale` aufrufen:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Dadurch erhöht sich die `pathBounds` Rechteck von 1,5 Einheiten auf allen vier Seiten. Dies ist eine vernünftige Lösung nur, wenn der Join Stroke gerundet wird. Eine gehrungsverbindung kann nicht länger, und es ist schwierig, berechnen.

Sie können auch eine ähnliche Technik, mit dem Text, als die **anisotrope Text** Seite erläutert. Hier ist der relevante Teil der `PaintSurface` -Handler aus der [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) Klasse:

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

[![](scale-images/anisotropictext-small.png "Dreifacher Screenshot der Seite anisotrope Test")](scale-images/anisotropictext-large.png#lightbox "dreifachen Screenshot der Seite anisotrope Test")

Wenn Sie das Seitenverhältnis des grafischen Objekte beibehalten müssen, sollten Sie kugelstrahler Skalierung verwenden. Die **Kugelstrahler Skalierung** Seite zeigt dies für den Stern 11 zwischengespeichert. Im Prinzip werden die Schritte zum Anzeigen eines grafischen Objekts in der Mitte der Seite mit der kugelstrahler Skalierung:

- Der Mittelpunkt des grafischen Objekts in die linke obere Ecke zu übersetzen.
- Skalieren Sie das Objekt basierend auf das Minimum der die horizontalen und vertikalen Seitengrößen geteilt durch die Objekt-Dimensionen.
- Übersetzen Sie den Mittelpunkt des skalierten Objekts in die Mitte der Seite.

Die [ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) führt folgende Schritte aus in umgekehrter Reihenfolge vor den Stern angezeigt:

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

Der Code zeigt auch das Sternsymbol 10 weitere Male, berücksichtigen jedes Mal, verringern die Skalierung von 10 % und progressiv die Farbe von Rot zu Blau zu ändern:

[![](scale-images/isotropicscaling-small.png "Dreifacher Screenshot der Seite Kugelstrahler Skalierung")](scale-images/isotropicscaling-large.png#lightbox "dreifachen Screenshot der Seite Kugelstrahler Skalierung")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
