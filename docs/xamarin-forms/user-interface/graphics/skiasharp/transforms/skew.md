---
title: Die Schiefsymmetrische Transformation
description: Finden Sie unter wie die Neigung Transformation Geneigter Grafikobjekte in SkiaSharp erstellen können
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: c8913dcb5dbe9664f1186b1acf46f09cb8da74ed
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="the-skew-transform"></a>Die Schiefsymmetrische Transformation

_Finden Sie unter wie die Neigung Transformation Geneigter Grafikobjekte in SkiaSharp erstellen können_

In SkiaSharp neigt die Neigung Transformation grafische Objekte, z. B. die Schattenkopie in dieser Abbildung:

![](skew-images/skewexample.png "Ein Beispiel des Programms verzerren Schattentext neigen")

Die Neigung transformiert Rechtecke in Parallelograms, aber eine verfälschte Ellipse ist immer noch eine Ellipse.

Obwohl Xamarin.Forms Eigenschaften für die Übersetzung, Skalierung und Drehungen definiert, ist keine entsprechende Eigenschaft in Xamarin.Forms für Neigung.

Die [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Methode `SKCanvas` akzeptiert zwei Argumente für Neigung horizontalen und vertikalen verzerren:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Ein zweites [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) -Methode kombiniert diese Argumente in einer einzelnen `SKPoint` Wert:

```csharp
public void Skew (SKPoint skew)
```

Es ist jedoch unwahrscheinlich, dass Sie eine dieser beiden Methoden in Isolation verwenden.

Die **verzerren experimentieren** Seite können Sie experimentieren mit Neigung Werte zwischen – 10 und 10. Eine Textzeichenfolge mit Zeitversatz abgerufene aus zwei Werte in der oberen linken Ecke der Seite positioniert ist `Slider` Elemente. So sieht die `PaintSurface` Ereignishandler in der [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) Klasse:

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

Werte von der `xSkew` Argument verschieben Sie den unteren Rand der Text für positive Werte nach rechts oder Links für negative Werte. Werte der `ySkew` für positive Werte oder für negative Werte rechts des Texts nach unten zu verschieben:

[![](skew-images/skewexperiment-small.png "Dreifacher Screenshot der Seite verzerren Experiment")](skew-images/skewexperiment-large.png#lightbox "dreifacher Screenshot der Seite Experiment neigen")

Wenn `xSkew` der negative Wert von `ySkew`, das Ergebnis ist die Rotation, aber auch skalierte etwas wie die Anzeige für die universelle Windows-Plattform gibt an.

Die Transformation Formeln lauten wie folgt:

X "= X + xSkew · y

y "= ySkew · X + y

Z. B. für eine Positive `xSkew` den transformierten Wert `x'` Wert erhöht als `y` erhöht. Was bewirkt, dass die Neigung ist.

Wenn ein Dreieck 200 Pixel breit und 100 Pixel hoch, die mit der linken oberen Ecke an dem Punkt (0, 0) positioniert ist, und wird beim Rendering ein `xSkew` Wert von 1,5 Parallelogramm folgende Ergebnisse:

![](skew-images/skeweffect.png "Die Auswirkung der Zeitversatz Transformation auf ein Rechteck")

Die Koordinaten des unteren Rands haben `y` -Werte von 100, daher ist es um 150 Pixel nach rechts verschoben.

Für Werte ungleich Null des `xSkew` oder `ySkew`, nur der Punkt (0, 0) gleich bleibt. Diesem Punkt kann das Zentrum des neigen angesehen werden. Wenn Sie das Zentrum des neigen um etwas anderes sein (Dies ist normalerweise die Groß-/Kleinschreibung) möchten, besteht keine `Skew` Methode, der bereitstellt. Sie müssen explizit kombinieren `Translate` Aufrufe mit der `Skew` aufrufen. Zur Mitte der neigen am `px` und `py`, stellen Sie die folgenden Aufrufe:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Die zusammengesetzte Transformation-Formeln sind:

X "= X + xSkew · (y – Py)

y "= ySkew · (X – px) + y

Wenn `ySkew` 0 (null), und geben Sie nur einen Wert ungleich null der `xSkew`, klicken Sie dann `px` Wert wird nicht verwendet. Der Wert ist irrelevant, und ebenso für `ySkew` und `py`.

Sie können nachrichtenorientierten angeben als Winkel der Neigungswinkel, z. B. den Winkel α in diesem Diagramm Zeitversatz unsicher sind:

![](skew-images/skewangleeffect.png "Die Auswirkung der Zeitversatz Transformation auf ein Rechteck mit einem neigen Winkel angegeben.")

Das Verhältnis der Schicht 150 Pixel zur vertikalen 100 Pixel ist der Tangens von diesen Winkel, in diesem Beispiel 56.3 Grad.

Der XAML-Datei von der **verzerren Winkel Experiment** Seite ähnelt der **Neigungswinkel** Seite, außer dass die `Slider` Elemente im Bereich zwischen-90 und 90 Grad. Die [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Code-Behind-Datei der Text auf der Seite zentriert und verwendet `Translate` Mittelpunkt neigen in die Mitte der Seite festlegen. Ein kurzer `SkewDegrees` Methode am unteren Rand der Code konvertiert Winkel zu neigen Werte:

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

Ein Winkel positive oder negative 90 Grad erreicht, der Tangens Ansätze unendlich, aber Winkel bis zu ungefähr 80 Grad oder eine ähnliche verwendet werden können:

[![](skew-images/skewangleexperiment-small.png "Dreifacher Screenshot der Seite verzerren Winkel Experiment")](skew-images/skewangleexperiment-large.png#lightbox "dreifacher Screenshot der Seite verzerren Winkel Experiment")

Eine kleine negative horizontale Neigung kann schräg oder kursiv Text imitieren, die als die **schräg Text** veranschaulicht. Die [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Klasse zeigt, wie dies funktioniert:

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

Die `TextAlign` Eigenschaft `SKPaint` festgelegt ist, um `Center`. Ohne Transformationen die `DrawText` telefonisch mit Koordinaten (0, 0) würde, positionieren Sie den Text horizontal mittig an der Basislinie in der oberen linken Ecke. Die `SkewDegrees` neigt den Text horizontal 20 Grad relativ zu der Basislinie. Die `Translate` Aufruf verschiebt die horizontale Mitte der Basislinien-der Text in der Mitte des Zeichenbereichs:

[![](skew-images/obliquetext-small.png "Dreifacher Screenshot der Seite schräg Text")](skew-images/obliquetext-large.png#lightbox "dreifacher Screenshot der Seite schräg Text")

Die **verzerren Schattentext** Seite veranschaulicht, wie eine Kombination aus einem 45-Grad-Neigung und vertikale Skalierung einer Textschatten vornehmen möchten, die den Text kippt. Hier wird der relevante Teil der `PaintSurface` Ereignishandler:

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

Der Schatten ist angezeigten zuerst und anschließend auf den Text an:

[![](skew-images/skewshadowtext1-small.png "Dreifacher Screenshot der Seite verzerren Schattentext")](skew-images/skewshadowtext1-large.png#lightbox "dreifacher Screenshot der Seite verzerren Schattentext")

Die vertikale Koordinate übergeben, um die `DrawText` Methode gibt die Position des Texts relativ zu der Basislinie. Dies ist die gleiche vertikale Koordinate für das Zentrum des neigen verwendet. Diese Technik kann nicht ausgeführt werden, wenn die Textzeichenfolge Unterlängen enthält. Zum Beispiel das Einsetzen des Worts "sonderbaren", "Schatten" und hier das Ergebnis:

[![](skew-images/skewshadowtext2-small.png "Dreifacher Screenshot der Seite verzerren Schattentext ein alternatives Wort mit Unterlängen")](skew-images/skewshadowtext2-large.png#lightbox "dreifacher Screenshot der Seite verzerren Schattentext ein alternatives Wort mit Unterlängen")

Die Schattenkopien und den Text werden weiterhin an der Grundlinie ausgerichtet, aber die Auswirkungen nur falschen sucht. Um dies zu beheben, müssen Sie die Textgrenzen zu erhalten:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

Die `Translate` Aufrufe durch die Höhe der Unterlängen angepasst werden müssen:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Jetzt erweitert der Schatten vom unteren Rand dieser Unterlängen bestimmt:

[![](skew-images/skewshadowtext3-small.png "Dreifacher Screenshot der Seite verzerren Schattentext Korrekturen für Unterlängen")](skew-images/skewshadowtext3-large.png#lightbox "dreifacher Screenshot der Seite verzerren Schattentext Korrekturen für Unterlängen bestimmt")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
