---
title: Matrixtransformationen
description: Eingehendere Untersuchung SkiaSharp Transformationen mit vielseitigen Transformationsmatrix
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 9d5e65abe675ded48e9239f2cd10ceed4a7c3a52
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="matrix-transforms"></a>Matrixtransformationen

_Eingehendere Untersuchung SkiaSharp Transformationen mit vielseitigen Transformationsmatrix_

Die Transformationen, die angewendet werden, um die `SKCanvas` in einer einzelnen Instanz von Objekt konsolidiert die [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) Struktur. Dies ist eine standardmäßige 3 x 3-Transformationsmatrix in allen modernen 2D Grafiksystemen vergleichbar.

Wie Sie gesehen haben, können Sie Transformationen in SkiaSharp ohne Kenntnis über die Transformation, Matrix, aber die Transformationsmatrix ist wichtig, hinsichtlich der theoretische und es ist entscheidend, wenn mithilfe von Transformationen zum Ändern von Pfaden oder für die Behandlung von komplexen Fingereingabe, beide die in diesem Artikel und der nächsten veranschaulicht werden.

![](matrix-images/matrixtransformexample.png "Eine Bitmap, die vorhanden ist, die eine affine Transformation")

Die aktuelle Transformationsmatrix angewendet die `SKCanvas` steht jedoch jederzeit durch den Zugriff auf den nur-Lese- [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) Eigenschaft. Sie können festlegen, eine neue Transformation Matrix mit der [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) Methode, und Sie können diese Transformationsmatrix Standardwerte wiederherstellen durch Aufrufen von [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Der einzige andere `SKCanvas` Member, die direkt mit dem Zeichenbereich Matrixtransformation ist [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) die zwei Matrizen durch Multiplizieren diese miteinander verkettet.

Die Standard-Transformationsmatrix um die Identitätsmatrix handelt und 1 in die Diagonale Zellen und 0 ansonsten besteht aus:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Sie können eine Identitätsmatrix mithilfe der statischen erstellen [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) Methode:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

Die `SKMatrix` Standardkonstruktor führt *nicht* eine Identitätsmatrix zurück. Es gibt eine Matrix mit alle Zellen auf 0 (null) festgelegt. Verwenden Sie nicht die `SKMatrix` Konstruktor, wenn Sie solche Zellen ein manuell festlegen möchten.

Wenn SkiaSharp ein Grafikobjekt gerendert wird, wird jeder Punkt (X, y) effektiv zu einer 1 3-Matrix mit 1 in der dritten Spalte konvertiert:

<pre>
| x  y  1 |
</pre>

Diese 1 3-Matrix stellt einen dreidimensionalen Punkt dar, mit die Z-Koordinate, die auf 1 festgelegt. Es gibt mathematische Gründe, die (weiter unten erläutert), warum eine zweidimensionale Matrixtransformation benötigt arbeiten in drei Dimensionen. Sie können diese 1 3-Matrix Darstellung eines Punkts in einem Koordinatensystem 3D, jedoch immer in der 2D-Ebene vorstellen, wobei Z 1 entspricht.

Dieser 1 3-Matrix mit der Transformationsmatrix multipliziert wird, und das Ergebnis ist der Punkt im Zeichenbereich gerendert:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Verwenden die standardmäßigen Matrixmultiplikation, lauten wie folgt die konvertierte Punkte:

x' = x

y' = y

z' = 1

Dies ist die Standard-Transformation.

Wenn die `Translate` Methode aufgerufen wird die `SKCanvas` -Objekt, der `tx` und `ty` Argumente für die `Translate` Methode werden die ersten beiden Zellen in der dritten Zeile der Transformationsmatrix:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Die Multiplikation ist jetzt wie folgt aus:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Hier sind die Transformation Formeln:

x' = x + tx

y "= y +" ty "

Skalierungsfaktoren haben den Standardwert 1. Beim Aufrufen der `Scale` Methode auf einem neuen `SKCanvas` Objekts, das sich ergebende Transformationsmatrix enthält die `sx` und `sy` Argumente in den diagonalen Zellen:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Die Transformation Formeln lauten wie folgt:

x' = sx · x

y' = sy · y

Nach dem Aufruf der Transformationsmatrix `Skew` die zwei Argumente in die Matrixzellen angrenzend an die Skalierungsfaktoren enthält:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Die Transformation-Formeln sind:

X "= X + xSkew · y

y "= ySkew · X + y

Für einen Aufruf `RotateDegrees` oder `RotateRadians` für α-Winkel der Transformationsmatrix lautet wie folgt:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Hier sind die Transformation Formeln:

x' = cos(α) · x - sin(α) · y

y "= sin(α) · X - cos(α) · y

Wenn α 0 Grad ist, um die Identitätsmatrix handelt. Wenn α 180 Grad ist, wird die Transformationsmatrix wie folgt:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Eine Drehung um 180 Grad ist gleichbedeutend mit der ein Objekt horizontal kippen und vertikal, die auch durch Festlegen von – 1 Skalierungsfaktoren erreicht wird.

Alle diese Typen von Transformationen, die als klassifiziert sind *affine* transformiert. Affine Transformationen umfassen nie die dritte Spalte der Matrix, die auf die Standardwerte von 0, 0 und 1 bleibt. Der Artikel [nicht Affine Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) nicht affine Transformationen erläutert.

## <a name="matrix-multiplication"></a>Matrixmultiplikation

Ein großer Vorteil bei der Verwendung der Transformationsmatrix kann also zusammengesetzte Transformationen von Matrixmultiplikation, abgerufen werden können, die häufig in der Dokumentation SkiaSharp als bezeichnet *Verkettung*. Viele der Methoden zugehörige Umwandeln in `SKCanvas` finden Sie unter "Pre-Verkettung" oder "Pre-Concat." Dies bezieht sich Reihenfolge der Multiplikation, also wichtig, da Matrixmultiplikation nicht kommutativ.

Angenommen, die Dokumentation für die [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Methode besagt, dass die It "Pre-Concats die aktuelle Matrix mit die angegebene Verschiebung" während der Dokumentation für die [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) Methode besagt, dass die It "Die aktuelle Matrix mit dem angegebenen Maßstab Pre-Concats."

Dies bedeutet, dass die Transformation, die durch Aufruf der Methode angegeben, der Multiplikator (der linke Operand ist) und die aktuelle Transformationsmatrix der Multiplikand (den rechten Operand).

Nehmen wir an, die `Translate` wird aufgerufen, gefolgt von `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

Die `Scale` Transformation multipliziert die `Translate` für zusammengesetzte Transformationsmatrix Transformieren:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` kann aufgerufen werden, bevor `Translate` wie folgt:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

In diesem Fall wird die Reihenfolge der Multiplikation umgekehrt, und die Skalierungsfaktoren effektiv auf die Übersetzung Faktoren angewendet werden:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

So sieht die `Scale` Methode mit einem Dreh-und Angelpunkt:

```csharp
canvas.Scale(sx, sy, px, py);
```

Dies entspricht den folgenden Aufrufen der übersetzen und Skalierung:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Die drei Transformation Matrizen werden in umgekehrter Reihenfolge multipliziert, aus wie die Methoden im Code angezeigt werden:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>Die SKMatrix-Struktur

Die `SKMatrix` Arbeitsmappenstruktur neun Lese-/Schreibeigenschaften des Typs `float` , die neun Zellen der Transformationsmatrix entspricht:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` definiert auch eine Eigenschaft namens [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) vom Typ `float[]`. Diese Eigenschaft kann verwendet werden, festlegen oder Abrufen der neun Werte gewünschtes in der Reihenfolge `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, und `Persp2`.

Die `Persp0`, `Persp1`, und `Persp2` Zellen werden in diesem Artikel erläutert [nicht Affine Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Wenn diese Zellen Standardwerte von 0, 0 und 1 haben, wird die Transformation mit einem-Koordinate Zeitpunkt wie folgt multipliziert:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

X "= ScaleX · X + SkewX · y + VerschX

y "= SkewX · X + ScaleY · y + VerschY

z' = 1

Dies ist die vollständige zweidimensionalen affine Transformation. Affine Transformation behält parallele Linien, was bedeutet, dass ein Rechteck nie in etwas anderes als ein Parallelogramm transformiert wird.

Die `SKMatrix` Struktur definiert mehrere statische Methoden zum Erstellen `SKMatrix` Werte. Diese alle return `SKMatrix` Werte:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) mit einem Dreh-und Angelpunkt
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) für einen Winkel im Bogenmaß
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) für einen Winkel im Bogenmaß (Radiant) mit einem Dreh-und Angelpunkt
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) mit einem Dreh-und Angelpunkt
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` auch definiert mehrere statische Methoden, die Verketten von zwei Matrizen, d. h. sie multiplizieren. Diese Methoden werden mit dem Namen `Concat`, `PostConcat`, und `PreConcat`, und es gibt zwei Versionen der einzelnen. Diese Methoden verfügen über keine Rückgabewerte; stattdessen die Ereignisse verweisen auf vorhandene `SKMatrix` Werte über `ref` Argumente. Im folgenden Beispiel `A`, `B`, und `R` (für "Ergebnis") werden alle `SKMatrix` Werte.

Die beiden `Concat` Methoden werden aufgerufen, wie folgt:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Diese führen die folgenden Multiplikation:

R = B × A

Die anderen Methoden werden nur zwei Parameter aufweisen. Der erste Parameter geändert, und bei der Rückgabe aus dem Aufruf der Methode ist, enthält das Produkt von zwei Matrizen. Die beiden `PostConcat` Methoden werden aufgerufen, wie folgt:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Diese Aufrufe führen Sie den folgenden Vorgang:

A = A × B

Die beiden `PreConcat` Methoden ähneln:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Diese Aufrufe führen Sie den folgenden Vorgang:

A = B × A

Die Versionen der Methodenaufrufen mit allen `ref` Argumente beim Aufrufen der zugrunde liegenden Implementierungen etwas effizienter sind, jedoch ist es möglicherweise verwirrend Personen Ihren Code lesen und vorausgesetzt, dass etwas mit einem `ref` Argument ist durch die Methode geändert. Darüber hinaus ist es häufig sinnvoll, ein Argument zu übergeben, das Ergebnis eines ist die `Make` Methoden, zum Beispiel:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Dadurch wird die folgende Matrix erstellt:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Dies ist die Skalierungstransformation die übersetzen-Transformation multipliziert. In diesem speziellen Fall die `SKMatrix` Struktur verfügt über eine Tastenkombination mit einer Methode namens [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Dies ist eine der mehrmals aus, wenn ist es sicher ist, verwenden Sie, die `SKMatrix` Konstruktor. Die `SetScaleTranslate` Methode legt alle neun Zellen der Matrix. Außerdem ist es sicher ist, verwenden Sie die `SKMatrix` Konstruktor mit der statischen `Rotate` und `RotateDegrees` Methoden:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Führen Sie diese Methoden *nicht* eine Rotationstransformation auf eine vorhandene Transformation zu verketten. Die Methoden legen Sie alle Zellen der Matrix. Sie sind funktionell identisch zum der `MakeRotation` und `MakeRotationDegrees` Methoden mit dem Unterschied, dass sie instanziieren Sie nicht die `SKMatrix` Wert.

Angenommen, Sie haben ein `SKPath` -Objekt, das Sie anzeigen möchten, aber Sie verwenden möchten, dass sie eine etwas andere Ausrichtung oder einem anderen Mittelpunkt aufweist. Sie können alle Koordinaten des Pfades durch Aufrufen der [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) Methode `SKPath` mit einer `SKMatrix` Argument. Die **Pfad transformieren** Seite veranschaulicht dies. Die [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) -Klasse Verweise der `HendecagramPath` Objekt in einem Feld verwendet jedoch ihren Konstruktor anzuwendende Transformation an, dass der Pfad:

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

Die `HendecagramPath` Objekt verfügt über eine Center unter (0, 0), und die elf Punkte des Sterns nach außen aus dieser Center von 100 Einheiten in allen Richtungen erweitern. Dies bedeutet, dass der Pfad der positive und negative Koordinaten enthält. Die **Pfad transformieren** Seite mit einem Stern dreimal so groß, und alle positive Koordinaten bevorzugt. Darüber hinaus soll nicht einem Punkt des Sterns gerade verweisen. Er möchte stattdessen für einen Punkt des Sterns gerade nach unten zeigen. (Da Sterns elf Elemente aufweist, kann er sowohl nicht aufweisen.) Dies erfordert, dass den Stern 360 Grad drehen 22 unterteilt.

Der Konstruktor erstellt ein `SKMatrix` Objekt aus drei separaten Transformationen, die mit der `PostConcat` Methode mit dem folgenden Muster ein, wobei A, B und C Instanzen von werden `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Dies ist eine Reihe von aufeinander folgenden Multiplikationen, daher ist das Ergebnis wie folgt:

EINE × B × C

Aufeinander folgende Multiplikationen helfen zu verstehen, was bewirkt, dass jede Transformation. Die Skalierungstransformation vergrößert die Pfadkoordinaten mit einem Faktor von 3, damit die Koordinaten im Bereich von –300 auf 300. Die Rotationstransformation dreht den Stern, um dessen Ursprung. Die Translationstransformation verschoben dann von 300 Pixel rechts und nach unten, d. h. alle Koordinaten, die positive werden.

Es gibt andere Sequenzen, die die gleiche Matrix zu erzeugen. Hier ist eine andere aus:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Dies den Pfad, um ihren Mittelpunkt zuerst dreht, und klicken Sie dann übersetzt 100 Pixel rechts und nach unten, sodass alle die Koordinaten sind positiv. Sterns wird dann relativ zur neuen oberen linken Ecke, vergrößert die den Punkt (0, 0) ist.

Die `PaintSurface` Handler kann einfach Rendern dieses Pfads:

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Es wird in der oberen linken Ecke des Zeichenbereichs angezeigt:

[![](matrix-images/pathtransform-small.png "Dreifacher Screenshot der Seite Pfad transformieren")](matrix-images/pathtransform-large.png#lightbox "dreifacher Screenshot der Seite Pfad transformieren")

Der Konstruktor Vertrieb dieses Programms gilt die Matrix für den Pfad mit dem folgenden Aufruf:

```csharp
transformedPath.Transform(matrix);
```

Der Pfad ist *nicht* dieser Matrix als Eigenschaft beibehalten. Stattdessen gilt es die Transformation für alle die Koordinaten des Pfads an. Wenn `Transform` heißt erneut, die Transformation angewendet wird erneut aus, und die einzige Möglichkeit, Sie können zurückgehen, durch Anwenden von einem anderen Matrix, die die Transformation wird rückgängig gemacht. Glücklicherweise der `SKMatrix` Struktur definiert eine [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix@/) Methode, die der Matrix abruft, kehrt eine angegebene Matrix:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Die Methode wird aufgerufen `TryInverse` da nicht alle Matrizen invertierbar ist, sondern eine Matrix nicht invertierbar ist unwahrscheinlich, dass für eine Transformation von Grafiken verwendet werden soll.

Sie können auch eine Matrixtransformation Anwenden einer `SKPoint` -Wert, der ein Array von Punkten, eine `SKRect`, oder auch nur eine einzelne Zahl innerhalb des Programms. Die `SKMatrix` Struktur unterstützt diese Vorgänge mit einer Auflistung von Methoden, die mit dem Wort beginnen `Map`, wie diese:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Wenn Sie diese letzte Methode verwenden, beachten Sie, dass die `SKRect` Struktur ist nicht in der Lage, eine gedrehte Rechteck darstellt. Die Methode ist nur sinnvoll für ein `SKMatrix` Wert, der Übersetzung darstellt, und skalieren.

### <a name="interactive-experimentation"></a>Interaktive Experimente

Eine Möglichkeit, ein Gefühl für die affine Transformation ist interaktiv drei Ecken einer Bitmap auf dem Bildschirm verschieben und sehen, welche Transformation führt. Dies ist das Konzept hinter der **Affine Matrix anzeigen** Seite. Diese Seite erfordert zwei weitere Klassen, die auch in anderen Demonstrationen verwendet werden:

Die [ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs) Klasse zeigt einen transparentes Kreis, der auf dem Bildschirm gezogen werden kann. `TouchPoint` erfordert, dass ein `SKCanvasView` oder ein Element, das ein übergeordnetes Element ist ein `SKCanvasView` haben die [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs) angefügt. Legen Sie die `Capture`-Eigenschaft auf `true` fest. In der `TouchAction` Ereignishandler, d. h. das Programm muss rufen die `ProcessTouchEvent` Methode in `TouchPoint` für jede `TouchPoint` Instanz. Gibt die Methode `true` , wenn das Touch-Ereignis im Verschieben Touch Point geführt hat. Darüber hinaus die `PaintSurface` Handler aufrufen muss die `Paint` Methode in den einzelnen `TouchPoint` Instanz, die an sie übergibt die `SKCanvas` Objekt.

`TouchPoint` Zeigt eine allgemeine Möglichkeit, dass ein visuelles Element SkiaSharp in einer separaten Klasse gekapselt werden. Die Klasse kann Eigenschaften für die Angabe der Merkmale des visuellen Elements definieren, und eine Methode mit dem Namen `Paint` mit einem `SKCanvas` Argument gerendert werden kann.

Die `Center` Eigenschaft `TouchPoint` gibt den Speicherort des Objekts. Diese Eigenschaft kann festgelegt werden, um den Speicherort zu initialisieren. die Eigenschaft geändert wird, wenn der Benutzer auf den Kreis, um im Zeichenbereich zieht.

Die **Affine Matrix-Seite anzeigen** erfordert außerdem die [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs) Klasse. Diese Klasse zeigt die Zellen einer `SKMatrix` Objekt. Sie verfügt über zwei öffentliche Methoden: `Measure` die Dimensionen der gerenderten Matrix abrufen und `Paint` angezeigt. Die Klasse enthält eine `MatrixPaint` Eigenschaft vom Typ `SKPaint` , die für eine andere Schriftgröße oder Farbe ersetzt werden.

Die [ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) Datei instanziiert den `SKCanvasView` und fügt eine `TouchEffect`. Die [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) Code-Behind-Datei erstellt drei `TouchPoint` Objekte und dann Positionen für drei Ecken einer Bitmap, die aus dem eingebetteten geladen wird Ressource:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Eine affine Matrix wird eindeutig durch drei Punkte definiert werden. Die drei `TouchPoint` Objekten entsprechen, in der oberen linken, oberen rechten und unteren linken Ecken der Bitmap. Da eine affine Matrix nur ein Rechteck in einem Parallelogramm transformiert fähig ist, wird der vierte Punkt von den anderen drei impliziert. Der Konstruktor endet mit einem Aufruf von `ComputeMatrix`, berechnet, die Zellen einer `SKMatrix` Objekt aus diesen drei Punkten.

Die `TouchAction` Ereignishandler ruft die `ProcessTouchEvent` Methode der einzelnen `TouchPoint`. Die `scale` Wert mit Xamarin.Forms Koordinaten in Pixel konvertiert:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Wenn alle `TouchPoint` verschoben wurde, und klicken Sie dann die Methode aufruft `ComputeMatrix` erneut und erklärt die Oberfläche ungültig.

Die `ComputeMatrix` Methode bestimmt die Matrix impliziert durch die drei Punkte. Wird aufgerufen, die Matrix `A` Transformationen ein einem Pixel quadratische Rechteck in einem Parallelogramm basierend auf den drei Punkten dagegen die Skalierungstransformation aufgerufen `S` Bitmap ein Quadrat Rechteck mit einem Pixel skaliert. Die zusammengesetzte Matrix ist `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
    {
        // Scale transform
        SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

        // Affine transform
        SKMatrix A = new SKMatrix
        {
            ScaleX = ptUR.X - ptUL.X,
            SkewY = ptUR.Y - ptUL.Y,
            SkewX = ptLL.X - ptUL.X,
            ScaleY = ptLL.Y - ptUL.Y,
            TransX = ptUL.X,
            TransY = ptUL.Y,
            Persp2 = 1
        };

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Schließlich die `PaintSurface` Methode rendert das Bitmuster, die basierend auf dieser Matrix zeigt die Matrix am unteren Rand des Bildschirms und rendert die Berührungspunkte drei Ecken der Bitmap:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

Der iOS-Bildschirm unten zeigt das Bitmuster an, wenn die Seite erstmals geladen wird, während die beiden anderen Bildschirme nach einigen Manipulation anzeigen:

[![](matrix-images/showaffinematrix-small.png "Dreifacher Screenshot der Seite Affine Matrix anzeigen")](matrix-images/showaffinematrix-large.png#lightbox "dreifacher Screenshot der Seite Affine Matrix anzeigen")

Obwohl es scheint, als ob die Berührungspunkte die Ecken der Bitmap ziehen, die nur eine Illusion ist. Die Matrix aus den Berührungspunkte berechnet transformiert das Bitmuster, damit die Ecken mit die Berührungspunkte übereinstimmen.

Es ist besser für Benutzer zu verschieben, ändern Sie die Größe und drehen Bitmaps nicht durch Ziehen der Ecken, aber mit ein oder zwei Fingern direkt für das Objekt zu ziehen, zusammendrücken, und drehen. Dieser Vorgang wird im nächsten Artikel beschrieben [berühren Manipulation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>Der Grund für die 3 x 3-Matrix

Möglicherweise erwartet, dass eine zweidimensionale Grafiken-System nur eine von 2 x 2-Transformationsmatrix erfordern würden:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Dies funktioniert für Skalierung, Drehung und sogar neigen, aber es kann keine die grundlegendste Transformationen, die Übersetzung ist.

Das Problem besteht darin, dass 2 x 2-Matrix darstellt eine *lineare* in zwei Dimensionen zu transformieren. Eine lineare Transformation behält einige grundlegende arithmetischen Operationen, aber zu den Auswirkungen ist, eine lineare Transformation nie den Punkt (0, 0) ändert. Eine lineare Transformation ist Übersetzung nicht möglich.

In drei Dimensionen sieht eine lineare Transformationsmatrix:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Die Zelle mit der Bezeichnung `SkewXY` bedeutet, die der Wert der X-Koordinate, die basierend auf der Y-Werte neigt; die Zelle `SkewXZ` bedeutet, dass der Wert wird die X-Koordinate, die basierend auf Werten Z; geneigt und Werte auf ähnliche Weise für die anderen verzerren `Skew` Zellen.

Es ist möglich, diese 3D-Transformation Matrix auf einer zweidimensionalen Ebene durch Festlegen von beschränken `SkewZX` und `SkewZY` auf 0 (null) und `ScaleZ` auf 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Wenn zweidimensionalen Grafiken vollständig auf die Ebene in einem 3D-Bereich gezeichnet werden, wobei Z 1 entspricht, sieht die Multiplikation von Transformation:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Alles, was verbleibt auf dem zweidimensionalen Ebene, bei denen gleich Z 1, aber die `SkewXZ` und `SkewYZ` Zellen werden effektiv zweidimensionalen Übersetzung Faktoren.

Dies ist eine dreidimensionale lineare Transformation wie als zweidimensionales nicht linearen Transformation dient. (Entsprechend, Transformationen in der 3D-Grafik basiert auf den eine 4 x 4-Matrix.)

Die `SKMatrix` in SkiaSharp Struktur definiert die Eigenschaften für die dritte Zeile:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Nicht-NULL-Werte der `Persp0` und `Persp1` dazu führen, Transformationen, die Objekte aus dem zweidimensionalen Ebene verschieben, wobei Z 1 entspricht. Was geschieht, wenn diese Objekte zurück in diese Ebene verschoben werden im Artikel behandelt wird, auf [nicht Affine Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
