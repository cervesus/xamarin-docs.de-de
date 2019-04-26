---
title: Die Scherungstransformation
description: In diesem Artikel wird erläutert, wie die Scherungstransformation Geneigter Grafikobjekte in SkiaSharp erstellen kann, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: a0c65ab2d319e39b236799cd453874142206b467
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61356197"
---
# <a name="the-skew-transform"></a>Die Scherungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Erfahren Sie, wie die Scherungstransformation Geneigter Grafikobjekte in SkiaSharp erstellen können_

In SkiaSharp neigt die Scherungstransformation grafische Objekte, z. B. die Schattenkopie in dieser Abbildung:

![](skew-images/skewexample.png "Ein Beispiel für eine Zerren von aus dem Programm Schattentext neigen")

Die ungleichmäßigkeit wird ein Rechteck zu einem Parallelogramm, aber eine verfälschte Ellipse ist immer noch eine Ellipse.

Obwohl Xamarin.Forms Eigenschaften für die Übersetzung, Skalierung und Drehungen definiert wird, ist keine entsprechende Eigenschaft in Xamarin.Forms für datenschiefe.

Die [ `Skew` ](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single)) -Methode der `SKCanvas` akzeptiert zwei Argumente für die horizontale Neigung und vertikal Neigen:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Ein zweites [ `Skew` ](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint)) Methode kombiniert diese Argumente in einem einzelnen `SKPoint` Wert:

```csharp
public void Skew (SKPoint skew)
```

Es ist jedoch unwahrscheinlich, dass Sie eine dieser beiden Methoden in Isolation verwenden möchten.

Die **neigen experimentieren** Seite können Sie experimentieren Sie mit der datenschiefe Werten zwischen – 10 und 10. Eine Zeichenfolge befindet sich in der oberen linken Ecke der Seite mit datenschiefe abgerufen, die aus zwei Werten `Slider` Elemente. Hier ist die `PaintSurface` Ereignishandler in der [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) Klasse:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Werte von der `xSkew` Argument shift unteren Rand der Text für positive Werte rechts oder Links für negative Werte. Werte der `ySkew` für positive Werte oder negative Werte für die rechts neben dem Text nach unten zu verschieben:

[![](skew-images/skewexperiment-small.png "Dreifacher Screenshot der Seite neigen Experiment")](skew-images/skewexperiment-large.png#lightbox "dreifachen Screenshot der Seite Experiment neigen")

Wenn die `xSkew` Wert ist der negative Wert von der `ySkew` Wert, der das Ergebnis ist die Rotation, aber ein wenig als auch für skaliert gibt an, die UWP-Anzeige.

Die Transformation Formeln lauten wie folgt aus:

X' = X + xSkew – y

y' = ySkew – X + y

Z. B. für ein positives Ergebnis `xSkew` Wert, der die transformierten `x'` erhöht sich der Wert als `y` erhöht. Das ist Was bewirkt, dass die Neigung.

Wenn ein Dreieck 200 Pixel breit und 100 Pixel hoch, die mit der linken oberen Ecke, an dem Punkt (0, 0) positioniert ist und gerendert wird, mit einem `xSkew` Wert von 1,5, die folgenden Parallelogramm Ergebnisse:

![](skew-images/skeweffect.png "Die Auswirkungen der datenschiefe Transformation auf ein Rechteck")

Die Koordinaten des unteren Rands `y` -Werte von 100, daher ist es um 150 Pixel nach rechts verschoben.

Für nicht-NULL-Werte der `xSkew` oder `ySkew`, nur der Punkt (0, 0) gleich bleibt. Diesem Punkt kann die Mitte des eine Zerren von betrachtet werden. Wenn Sie den Mittelpunkt der Verzerrung um etwas anderes benötigen (die normalerweise der Fall ist), besteht keine `Skew` Methode, die enthält. Sie müssen explizit kombinieren `Translate` Ruft mit der `Skew` aufrufen. Zentrieren der Verzerrung an `px` und `py`, stellen Sie die folgenden Aufrufe:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Die zusammengesetzten Transformierung-Formeln sind:

X' = X + xSkew – (j – Py)

y' = ySkew – (X – px) + y

Wenn `ySkew` ist 0 (null), und klicken Sie dann die `px` Wert wird nicht verwendet. Der Wert ist irrelevant, und ebenso für `ySkew` und `py`.

Sie können mehr als einen Winkel von neigen, z. B. den Winkel α in diesem Diagramm datenschiefe angeben vertraut:

![](skew-images/skewangleeffect.png "Die Auswirkungen der datenschiefe Transformation auf ein Rechteck mit einem neigen Winkel angegeben.")

Das Verhältnis der Verlagerung der vertikalen 100 Pixel 150 Pixel ist der Tangens von diesen Winkel, in diesem Beispiel 56.3 Grad an.

Der XAML-Datei, der die **neigen Winkel Experiment** Seite ähnelt der **Neigungswinkel** Seite, außer dass die `Slider` Elemente, die von –90 Grad und 90 Grad liegen. Die [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Code-Behind-Datei zentriert den Text auf der Seite, und verwendet `Translate` Mitte Zerren von in die Mitte der Seite festlegen. Eine kurze `SkewDegrees` Methode am unteren Rand der Code konvertiert Winkel um Werte zu neigen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Ein Winkel positive oder negative um 90 Grad erreicht, der Tangens nähert, unendlich, aber Winkel bis zu etwa 80 Grad oder so verwendet werden können:

[![](skew-images/skewangleexperiment-small.png "Dreifacher Screenshot der Seite neigen Winkel Experiment")](skew-images/skewangleexperiment-large.png#lightbox "dreifachen Screenshot der Seite neigen Winkel Experiment")

Eine kleine negative horizontale Neigung kann schräg oder kursiv formatierter Text, imitieren, die als die **schräg Text** Seite erläutert. Die [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Klasse zeigt, wie dies funktioniert:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Die `TextAlign` Eigenschaft `SKPaint` nastaven NA hodnotu `Center`. Ohne Transformationen die `DrawText` rufen Sie mit der Koordinaten der (0, 0) würde, positionieren Sie den Text an der horizontalen Mitte des der Basislinie in der oberen linken Ecke. Die `SkewDegrees` neigt den Text horizontal 20 Grad relativ zur Baseline. Die `Translate` Aufruf wird die horizontale Mitte des Texts Baseline auf die Mitte der Canvas:

[![](skew-images/obliquetext-small.png "Dreifacher Screenshot der Seite schräg Text")](skew-images/obliquetext-large.png#lightbox "dreifachen Screenshot der Seite schräg Text")

Die **Schattentext neigen** Seite veranschaulicht, wie eine Kombination aus einer 45-Grad-datenschiefe und die vertikale Skala einen Textschatten vornehmen, die außerhalb des Texts kippt. Hier ist der relevante Teil der `PaintSurface` Handler:

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Der Schatten ist angezeigten zuerst, und klicken Sie dann den Text an:

[![](skew-images/skewshadowtext1-small.png "Dreifacher Screenshot der Seite Schattentext neigen")](skew-images/skewshadowtext1-large.png#lightbox "dreifachen Screenshot der Seite Schattentext neigen")

Die vertikale Koordinate, die an die `DrawText` Methode gibt die Position des Texts relativ zur Baseline. Dies ist die gleiche vertikale Koordinate für den Mittelpunkt der Verzerrung verwendet. Dieses Verfahren funktioniert nicht, wenn die Textzeichenfolge Unterlängen enthält. Ersetzen z. B. das Wort "Merkwürdiges", "Schatten" aus, und hier ist das Ergebnis:

[![](skew-images/skewshadowtext2-small.png "Dreifacher Screenshot der Seite Schattentext neigen ein alternatives Wort mit Unterlängen")](skew-images/skewshadowtext2-large.png#lightbox "dreifachen Screenshot der Seite Schattentext neigen ein alternatives Wort mit Unterlängen")

Die Schattenkopie und der Text immer noch unter der Baseline, ist jedoch der Effekt wird nur falsch. Um dies zu beheben, müssen Sie die textbegrenzungen abrufen:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

Die `Translate` Aufrufe müssen durch die Höhe der der Unterlängen angepasst werden:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Jetzt erweitert der Schatten zwischen dem unteren Rand dieser Unterlängen ein:

[![](skew-images/skewshadowtext3-small.png "Dreifacher Screenshot der Seite Schattentext neigen mit Anpassungen für Unterlängen")](skew-images/skewshadowtext3-large.png#lightbox "dreifachen Screenshot der Seite Schattentext neigen mit Anpassungen für Unterlängen bestimmt")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
