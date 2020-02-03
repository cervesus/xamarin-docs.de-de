---
title: Die Skalierungstransformation
description: Verkaufsrabatte Artikel untersucht die Skalierungstransformation SkiaSharp, für die Skalierung von Objekten in verschiedenen Größen und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 7dc7a2faabef86aa63b52739edda696efcb54aba
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724243"
---
# <a name="the-scale-transform"></a>Die Skalierungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Entdecken Sie die skiasharp-Skalierungs Transformation zum Skalieren von Objekten auf verschiedene Größen._

Wie Sie im Artikel [**übersetzen der Transformation**](translate.md) gesehen haben, kann die Translation-Transformation ein grafisches Objekt von einem Speicherort zu einem anderen verschieben. Im Gegensatz dazu ändert die Skalierungstransformation die Größe des grafischen Objekts:

![](scale-images/scaleexample.png "A tall word scaled in size")

Die Skalierungstransformation bewirkt, dass oft auch Grafikkoordinaten verschieben, wenn sie größere vorgenommen werden.

Früher haben Sie zwei transformationsformeln gesehen, die die Auswirkungen der Übersetzungs Faktoren `dx` und `dy`beschreiben:

X' = X + Dx

y' = y + dy

Skalierungsfaktoren von `sx` und `sy` sind eher multiplikativ als Additiv:

X' = Sx – x

y' = Sy – y

Die Standardwerte der übersetzen Faktoren sind 0; die Standardwerte für die Skalierungsfaktoren sind 1.

Die `SKCanvas`-Klasse definiert vier `Scale`-Methoden. Bei der ersten [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) Methode handelt es sich um Fälle, in denen Sie denselben horizontalen und vertikalen Skalierungsfaktor wünschen:

```csharp
public void Scale (Single s)
```

Dies wird als *isotrope* Skalierung &mdash; Skalierung bezeichnet, die in beide Richtungen gleich ist. Kugelstrahler Skalierung wird das Seitenverhältnis des Objekts beibehalten.

Mit dem zweiten [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) -Methode können Sie unterschiedliche Werte für die horizontale und vertikale Skalierung angeben:

```csharp
public void Scale (Single sx, Single sy)
```

Dies führt zu einer *anisotrope* Skalierung.
Die dritte [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) -Methode kombiniert die beiden Skalierungsfaktoren in einem einzelnen `SKPoint` Wert:

```csharp
public void Scale (SKPoint size)
```

Die vierte `Scale` Methode wird in Kürze beschrieben.

Die **grundlegende Skalierungs** Seite veranschaulicht die `Scale`-Methode. Die Datei " [**basicscalepage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) " enthält zwei `Slider` Elemente, mit denen Sie horizontale und vertikale Skalierungsfaktoren zwischen 0 und 10 auswählen können. Die [**BasicScalePage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) -Code Behind-Datei verwendet diese Werte, um `Scale` aufzurufen, bevor ein abgerundetes Rechteck mit einer gestrichelten Linie und einem Text in der oberen linken Ecke der Canvas angezeigt wird:

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

Sie Fragen sich vielleicht: Wie wirken sich die Skalierungsfaktoren auf den Wert aus, der von der `MeasureText` Methode `SKPaint`zurückgegeben wird? Die Antwort lautet: überhaupt nicht. `Scale` ist eine Methode `SKCanvas`. Es wirkt sich nicht auf Elemente aus, die Sie mit einem `SKPaint` Objekt ausführen, bis Sie dieses Objekt verwenden, um etwas auf der Canvas zu erzeugen.

Wie Sie sehen, nimmt alles nach dem `Scale`-aufrufbedarf proportional zu:

[![](scale-images/basicscale-small.png "Triple screenshot of the Basic Scale page")](scale-images/basicscale-large.png#lightbox "Triple screenshot of the Basic Scale page")

Der Text die Breite der gestrichelten Linie, die Länge der Bindestriche in dieser Zeile, die runden der Ecken und der 10-Pixel-Rand zwischen dem linken und oberen Rand der Leinwand und abgerundeten Rechtecks unterliegen alle die gleiche Skalierungsfaktoren.

> [!IMPORTANT]
> Die universelle Windows-Plattform anisotropicly skalierten Text nicht ordnungsgemäß gerendert.

Anisotrope Skalierung bewirkt, dass Sie die Strichbreite für Zeilen unterschiedlich sind, die mit der horizontalen und vertikalen Achsen ausgerichtet ist. (Dies ist auch in der ersten Abbildung auf dieser Seite ersichtlich.) Wenn Sie nicht möchten, dass die Strichbreite von den Skalierungsfaktoren betroffen ist, legen Sie Sie auf 0 fest, und es ist unabhängig von der `Scale` Einstellung immer ein Pixel breit.

Skalierung ist relativ zu der oberen linken Ecke des Zeichenbereichs. Dies ist möglicherweise genau erwünscht, aber möglicherweise nicht. Angenommen Sie, Sie Text und Rechteck, die an anderer Stelle auf der Leinwand positionieren möchten, und es relativ zu seinen Mittelpunkt skaliert werden soll. In diesem Fall können Sie die vierte Version der [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) -Methode verwenden, die zwei zusätzliche Parameter zum Angeben des Mittelpunkts der Skalierung umfasst:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

Mit den Parametern `px` und `py` wird ein Punkt definiert, der manchmal als zentrales *Zentrum* bezeichnet wird, in der skiasharp-Dokumentation aber als *Pivotpunkt*bezeichnet wird. Dies ist ein Punkt relativ zu der oberen linken Ecke des Zeichenbereichs, die nicht von der Skalierung betroffen ist. Gesamte Skalierung tritt auf, Bezug auf dieses Zentrum.

Die Seite " [**zentrierte Skalierung**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) " zeigt, wie dies funktioniert. Der `PaintSurface`-Handler ähnelt dem **grundlegenden Skalierungs** Programm, mit dem Unterschied, dass der `margin` Wert berechnet wird, um den Text horizontal zentrieren, was impliziert, dass das Programm am besten im Hochformat funktioniert:

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

Die linke obere Ecke des gerundeten Rechtecks wird `margin` Pixel links vom Zeichenbereich positioniert und `margin` Pixel vom oberen Rand. Die letzten zwei Argumente der `Scale`-Methode werden auf diese Werte festgelegt, zuzüglich der Breite und Höhe des Texts, bei der es sich auch um die Breite und Höhe des gerundeten Rechtecks handelt. Dies bedeutet, dass alle Skalierung Bezug auf den Mittelpunkt dieses Rechtecks:

[![](scale-images/centeredscale-small.png "Triple screenshot of the Centered Scale page")](scale-images/centeredscale-large.png#lightbox "Triple screenshot of the Centered Scale page")

Die `Slider` Elemente in diesem Programm haben einen Bereich von &ndash;10 bis 10. Wie Sie sehen können, dazu führen, dass negative Werte, der vertikale Skalierung (z. B. in der Mitte auf der Android Bildschirm "") Objekte, um die horizontale Achse kippen, die den Mittelpunkt der Skalierung durchlaufen. Negative Werte, der die horizontale Skalierung (z. B. in der UWP-Bildschirm, auf der rechten Seite) dazu führen, dass Objekte, um die vertikale Achse kippen, die den Mittelpunkt der Skalierung durchlaufen.

Die Version der [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) -Methode mit pivotpunkten ist eine Verknüpfung für eine Reihe von drei `Translate`-und `Scale` aufrufen. Möglicherweise möchten Sie sehen, wie dies funktioniert, indem Sie die `Scale`-Methode auf der **zentrierten Skalierungs** Seite durch Folgendes ersetzen:

```csharp
canvas.Translate(-px, -py);
```

Hierbei handelt es sich um die negative, der die Punktkoordinaten Pivot.

Führen Sie nun das Programm erneut aus. Sie sehen, dass das Rechteck und der Text, dass sich das Center in der oberen linken Ecke des Zeichenbereichs verschoben werden. Sie können es kaum sehen. Der Schieberegler nicht natürlich verwendet werden, da Sie nun die Anwendung überhaupt nicht skaliert werden kann.

Fügen Sie nun den grundlegenden `Scale`-aufrufsbefehl (ohne Skalierungs Center) *vor* dem `Translate` aufgerufen wird:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Wenn Sie sich mit vertraut ist dieser Übung in anderen Grafikprogrammierung Systeme Sie vielleicht denken, dass dies falsch ist, aber nicht. Skia behandelt die Transformation von aufeinander folgenden Aufrufen etwas anders aus, was Sie möglicherweise kennen.

Mit den aufeinander folgenden `Scale`-und `Translate` aufrufen befindet sich der Mittelpunkt des gerundeten Rechtecks immer noch in der oberen linken Ecke, aber Sie können es jetzt in Relation zur oberen linken Ecke des Zeichen Bereichs skalieren. Dies ist auch die Mitte des gerundeten Rechtecks.

Fügen Sie nun vor diesem `Scale` einen weiteren `Translate`-aufrufswert mit den zentrieren-Werten hinzu:

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

Beachten Sie, dass die Standardwerte `sx` und `sy` 1 lauten. Es ist einfach, sich selbst davon überzeugen, dass der Dreh-und Angelpunkt (px, Py) nicht durch diese Formeln transformiert wird. Es verbleibt am gleichen Speicherort relativ zur Leinwand.

Wenn Sie `Translate` und `Scale` Aufrufe kombinieren, ist die Reihenfolge wichtig. Wenn die `Translate` nach der `Scale`erfolgt, werden die Übersetzungs Faktoren effektiv durch die Skalierungsfaktoren skaliert. Wenn die `Translate` vor dem `Scale`steht, werden die Übersetzungs Faktoren nicht skaliert. Dieser Vorgang wird etwas klarer (obgleich mehr mathematische) bei der Betreff der Transformation-Matrizen eingeführt wird.

Die `SKPath` Klasse definiert eine schreibgeschützte [`Bounds`](xref:SkiaSharp.SKPath.Bounds) Eigenschaft, die einen `SKRect` zurückgibt, der den Umfang der Koordinaten im Pfad definiert. Wenn die `Bounds`-Eigenschaft beispielsweise aus dem zuvor erstellten Pfad "hendecagram" abgerufen wird, sind die Eigenschaften `Left` und `Top` des Rechtecks ungefähr – 100, die Eigenschaften `Right` und `Bottom` sind ungefähr 100, und die Eigenschaften `Width` und `Height` sind ungefähr 200. (Die meisten der tatsächlichen Werte sind ein wenig kleiner, da nur die obersten Punkts mit die horizontalen oder vertikalen Achsen ist jedoch die Punkte der Sterne werden durch einen Kreis mit einem Radius von 100 definiert.)

Die Verfügbarkeit dieser Informationen wird impliziert, dass es möglich, Skalierung und Übersetzen der Faktoren, die für die Skalierung eines Pfads zu die Größe des Zeichenbereichs geeignet sein sollte. Die Seite [**anisotrope Skalierung**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) veranschaulicht dies mit dem 11-pointierten Stern. Eine *anisotrope* Skala bedeutet, dass Sie in horizontaler und vertikaler Richtung ungleich ist, was bedeutet, dass der Stern das ursprüngliche Seitenverhältnis nicht beibehält. Dies ist der relevante Code im `PaintSurface` Handlers:

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

Das `pathBounds` Rechteck wird oben in diesem Code abgerufen und später mit der Breite und Höhe des Zeichen Bereichs im `Scale` aufgerufen. Durch diesen eigenständigen Aufrufe werden die Koordinaten des Pfads skaliert, wenn er durch den `DrawPath`-Befehl gerendert wird, aber der Stern wird in der oberen rechten Ecke der Canvas zentriert. Nach unten und nach links verschoben werden muss. Dies ist die Aufgabe des `Translate`-Aufrufes. Diese beiden `pathBounds` Eigenschaften sind ungefähr – 100, sodass die Übersetzungs Faktoren ungefähr 100 betragen. Da der `Translate`-Aufrufe nach dem `Scale`-Befehl erfolgt, werden diese Werte effektiv durch die Skalierungsfaktoren skaliert, sodass Sie die Mitte des Sterns in den Mittelpunkt der Canvas verschieben:

[![](scale-images/anisotropicscaling-small.png "Triple screenshot of the Anisotropic Scaling page")](scale-images/anisotropicscaling-large.png#lightbox "Triple screenshot of the Anisotropic Scaling page")

Eine andere Möglichkeit für die `Scale` und `Translate` Aufrufe besteht darin, die Auswirkung in umgekehrter Reihenfolge zu bestimmen: der `Translate` Aufruf verschiebt den Pfad so, dass er vollständig sichtbar ist, aber in der oberen linken Ecke der Canvas ausgerichtet ist. Durch die `Scale`-Methode wird dieser Stern dann relativ zur oberen linken Ecke vergrößert.

Tatsächlich wird es angezeigt, dass das Sternchen ein bisschen größer als die Leinwand ist. Das Problem ist die Strichbreite. Die `Bounds`-Eigenschaft von `SKPath` gibt die Dimensionen der Koordinaten an, die im Pfad codiert sind, und das ist das, was das Programm zum Skalieren verwendet. Wenn der Pfad mit einer Strichbreite von bestimmten gerendert wird, ist der gerenderte Pfad größer als der Leinwand.

Zur Behebung dieses Problems müssen Sie dafür zu kompensieren. Ein einfacher Ansatz in diesem Programm besteht darin, direkt vor dem `Scale`-Befehl die folgende Anweisung hinzuzufügen:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Dadurch wird das `pathBounds` Rechteck um 1,5 Einheiten auf allen vier Seiten vergrößert. Dies ist eine vernünftige Lösung nur, wenn der Join Stroke gerundet wird. Eine gehrungsverbindung kann nicht länger, und es ist schwierig, berechnen.

Sie können auch eine ähnliche Technik mit Text verwenden, wie die Seite " **anisotrope Text** " veranschaulicht. Hier ist der relevante Teil des `PaintSurface` Handlers aus der [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) -Klasse:

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

Es ist eine ähnliche Logik, und der Text wird auf Grundlage des Text Begrenzungen-Rechtecks, das von `MeasureText` zurückgegeben wird, auf die Größe der Seite erweitert (was etwas größer ist als der tatsächliche Text):

[![](scale-images/anisotropictext-small.png "Triple screenshot of the Anisotropic Test page")](scale-images/anisotropictext-large.png#lightbox "Triple screenshot of the Anisotropic Test page")

Wenn Sie das Seitenverhältnis des grafischen Objekte beibehalten müssen, sollten Sie kugelstrahler Skalierung verwenden. Die Seite für die **isotrope Skalierung** zeigt dies für den 11-pointierten Stern. Im Prinzip werden die Schritte zum Anzeigen eines grafischen Objekts in der Mitte der Seite mit der kugelstrahler Skalierung:

- Der Mittelpunkt des grafischen Objekts in die linke obere Ecke zu übersetzen.
- Skalieren Sie das Objekt basierend auf das Minimum der die horizontalen und vertikalen Seitengrößen geteilt durch die Objekt-Dimensionen.
- Übersetzen Sie den Mittelpunkt des skalierten Objekts in die Mitte der Seite.

Die [`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) führt diese Schritte in umgekehrter Reihenfolge aus, bevor der Stern angezeigt wird:

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

[![](scale-images/isotropicscaling-small.png "Triple screenshot of the Isotropic Scaling page")](scale-images/isotropicscaling-large.png#lightbox "Triple screenshot of the Isotropic Scaling page")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
