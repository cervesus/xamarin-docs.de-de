---
title: Die Scherungstransformation
description: In diesem Artikel wird erläutert, wie die Schiefe Transformation in skiasharp gekippte grafische Objekte erstellen kann, und veranschaulicht dies mit Beispielcode.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 207b16f062a5c2137ac5fc3c21775d2486fda57d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135862"
---
# <a name="the-skew-transform"></a>Die Scherungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Sehen Sie, wie die Schiefe Transformation in skiasharp gekippte grafische Objekte erstellen kann_

In skiasharp gibt die Schiefe-Transformation grafische Objekte, wie z. b. den Schatten in diesem Bild, aus:

![](skew-images/skewexample.png "An example of skewing from the Skew Shadow Text program")

Die Schiefe wandelt ein Rechteck in ein Parallelogramm um, aber eine Schiefe Ellipse ist nach wie vor eine Ellipse.

Obwohl Xamarin.Forms Eigenschaften für Übersetzung, Skalierung und Rotation definiert, gibt es keine entsprechende Eigenschaft in Xamarin.Forms für die Schiefe.

Die [`Skew`](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single)) -Methode von `SKCanvas` akzeptiert zwei Argumente für horizontale Schiefe und vertikale Schiefe:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Eine zweite [`Skew`](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint)) Methode kombiniert diese Argumente in einem einzelnen `SKPoint` Wert:

```csharp
public void Skew (SKPoint skew)
```

Es ist jedoch unwahrscheinlich, dass Sie eine dieser beiden Methoden isoliert verwenden.

Auf der Seite " **schiefe Experiment** " können Sie mit Schiefe Werten experimentieren, die zwischen – 10 und 10 liegen. Eine Text Zeichenfolge wird in der oberen linken Ecke der Seite positioniert, wobei Schiefe-Werte aus zwei `Slider` Elementen abgerufen werden. Dies ist der `PaintSurface` Handler in der- [`SkewExperimentPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) Klasse:

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

Werte des `xSkew` Arguments bewegen den unteren Rand des Texts nach rechts für positive Werte oder bei negativen Werten nach links. Werte für `ySkew` die Verschiebung der rechten Seite des Texts nach unten für positive Werte oder nach oben für negative Werte:

[![](skew-images/skewexperiment-small.png "Triple screenshot of the Skew Experiment page")](skew-images/skewexperiment-large.png#lightbox "Triple screenshot of the Skew Experiment page")

Wenn der `xSkew` Wert der negative Wert des `ySkew` Werts ist, ist das Ergebnis Drehung, aber auch etwas skaliert.

Die transformationsformeln lauten wie folgt:

x ' = x + xschräw = Teenie

y ' = yschiew = x + y

Bei einem positiven `xSkew` Wert wird der transformierte Wert beispielsweise vergrößert `x'` `y` . Das ist das, was die Neigung bewirkt.

Wenn ein Dreieck 200 Pixel breit und 100 Pixel hoch mit der oberen linken Ecke am Punkt (0,0) positioniert und mit dem Wert 1,5 gerendert wird `xSkew` , ergibt sich Folgendes Parallelogramm:

![](skew-images/skeweffect.png "The effect of the skew transform on a rectangle")

Die Koordinaten des unteren Rands haben die `y` Werte 100, sodass Sie um 150 Pixel nach rechts verschoben werden.

Bei Werten ungleich NULL von `xSkew` oder `ySkew` bleibt nur der Punkt (0, 0) unverändert. Dieser Punkt kann als Mittelpunkt der Neigung betrachtet werden. Wenn Sie den Mittelpunkt der Neigung als etwas anderes benötigen (was in der Regel der Fall ist), gibt es keine `Skew` Methode, die diese bereitstellt. Aufrufe müssen explizit `Translate` mit dem-Aufruf kombiniert werden `Skew` . Um die Neigung unter und zu zentrieren `px` `py` , führen Sie die folgenden Aufrufe aus:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Die zusammengesetzten transformationsformeln lauten wie folgt:

x ' = x + xschräw = (y – py)

y ' = yschiew = (x – px) + y

Wenn `ySkew` 0 (null) ist, `px` wird der Wert nicht verwendet. Der-Wert ist irrelevant und ähnlich für `ySkew` und `py` .

Möglicherweise ist es besser, die Neigung als Neigungswinkel anzugeben, wie z. b. den Winkel "α" in diesem Diagramm:

![](skew-images/skewangleeffect.png "The effect of the skew transform on a rectangle with a skewing angle indicated")

Das Verhältnis zwischen der 150-Pixel-Verschiebung und der vertikalen 100 Pixel ist der Tangens dieses Winkels, in diesem Beispiel 56,3 Grad.

Die XAML-Datei der **Neigungswinkel-Experiment** Seite ähnelt der **Neigungswinkel** Seite, mit dem Unterschied, dass die `Slider` Elemente zwischen – 90 Grad und 90 Grad liegen. Die [`SkewAngleExperiment`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Code-Behind-Datei zentriert den Text auf der Seite und verwendet `Translate` , um einen Mittelpunkt der Neigung auf den Mittelpunkt der Seite festzulegen. Eine kurze `SkewDegrees` Methode am Ende des Codes konvertiert Winkel in schiefe Werte:

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

Wenn ein Winkel positive oder negative 90 Grad erreicht, nähert sich der Tangens unendlich, aber Winkel bis zu etwa 80 Grad oder können verwendet werden:

[![](skew-images/skewangleexperiment-small.png "Triple screenshot of the Skew Angle Experiment page")](skew-images/skewangleexperiment-large.png#lightbox "Triple screenshot of the Skew Angle Experiment page")

Eine kleine negative horizontale Schiefe kann schrägen oder kursiv formatiert werden, wie die **schräge Textseite** zeigt. Die [`ObliqueTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Klasse zeigt, wie Sie ausgeführt wird:

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

Die- `TextAlign` Eigenschaft von `SKPaint` ist auf festgelegt `Center` . Ohne Transformationen würde der-Befehl `DrawText` mit Koordinaten von (0,0) den Text mit der horizontalen Mitte der Baseline in der oberen linken Ecke positionieren. Die vergrenzt `SkewDegrees` den Text horizontal um 20 Grad relativ zur Baseline. Der `Translate` -Befehl verschiebt den horizontalen Mittelpunkt der Baseline des Texts in den Mittelpunkt der Canvas:

[![](skew-images/obliquetext-small.png "Triple screenshot of the Oblique Text page")](skew-images/obliquetext-large.png#lightbox "Triple screenshot of the Oblique Text page")

Die **schiefe schattentextseite** zeigt, wie eine Kombination aus einer 45-Grad-Schiefe und vertikaler Skala verwendet wird, um einen Text Schatten zu erstellen, der aus dem Text entfernt wird. Hier ist der relevante Teil des `PaintSurface` Handlers:

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

Zuerst wird der Schatten und dann der Text angezeigt:

[![](skew-images/skewshadowtext1-small.png "Triple screenshot of the Skew Shadow Text page")](skew-images/skewshadowtext1-large.png#lightbox "Triple screenshot of the Skew Shadow Text page")

Die an die Methode über gegebene vertikale Koordinate `DrawText` gibt die Position des Texts relativ zur Baseline an. Dies ist dieselbe vertikale Koordinate, die für den Mittelpunkt der Neigung verwendet wird. Diese Technik funktioniert nicht, wenn die Text Zeichenfolge Nachfolger Zeichen enthält. Ersetzen Sie z. b. das Wort "Quirky" für "Shadow", und hier ist das Ergebnis:

[![](skew-images/skewshadowtext2-small.png "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")](skew-images/skewshadowtext2-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")

Der Schatten und der Text sind immer noch an der Baseline ausgerichtet, aber der Effekt sieht nur falsch aus. Um dieses Problem zu beheben, müssen Sie die Text Begrenzungen abrufen:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

Die `Translate` Aufrufe müssen durch die Höhe der nachfolgenden untergeordneten Elemente angepasst werden:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Nun reicht der Schatten vom unteren Rand der nachfolgenden untergeordneten Elemente:

[![](skew-images/skewshadowtext3-small.png "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")](skew-images/skewshadowtext3-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
