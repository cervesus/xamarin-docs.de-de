---
title: Matrixtransformationen in SkiaSharp
description: In diesem Artikel dringt tiefer in SkiaSharp-Transformationen mit der vielseitige Transformationsmatrix, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2017
ms.openlocfilehash: 07b6a13a8bba1e30db1d69e49aa87420bbbdf601
ms.sourcegitcommit: a635312ffec816ba357a92b66c8c5221c8d9044c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "39615430"
---
# <a name="matrix-transforms-in-skiasharp"></a>Matrixtransformationen in SkiaSharp

_Dringen Sie tiefer in SkiaSharp-Transformationen mit der vielseitige Transformationsmatrix_

Aller Transformationen angewendet werden, um die `SKCanvas` Objekt werden in einer einzelnen Instanz von konsolidiert die [ `SKMatrix` ](xref:SkiaSharp.SKMatrix) Struktur. Dies ist eine standard-3-Mal-3-Transformationsmatrix ähnlich denen in allen modernen 2D-Grafiken-Systemen.

Wie Sie gesehen haben, können Sie Transformationen in SkiaSharp ohne zu wissen über die Transformation, Matrix, aber die Transformationsmatrix unbedingt aus theoretischer Sicht aus, und es ist entscheidend, Verwendung von Transformationen zum Ändern von Pfaden oder für die Behandlung komplexer toucheingaben, beide die in diesem Artikel und der nächsten veranschaulicht werden.

![](matrix-images/matrixtransformexample.png "Eine Bitmap, bei denen sich eine affine Transformation")

Die aktuellen Transformationsmatrix, die angewendet werden, um die `SKCanvas` steht jederzeit durch den Zugriff auf den schreibgeschützten [ `TotalMatrix` ](xref:SkiaSharp.SKCanvas.TotalMatrix) Eigenschaft. Sie können festlegen, eine neue Transformation Matrix mit der [ `SetMatrix` ](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix)) Methode, und Sie können diese Transformationsmatrix Standardwerte wiederherstellen durch Aufrufen von [ `ResetMatrix` ](xref:SkiaSharp.SKCanvas.ResetMatrix).

Der einzige andere `SKCanvas` Member, die direkt mit der Leinwand Matrixtransformation [ `Concat` ](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@)) die zwei Matrizen durch Multiplikation miteinander verkettet.

Die Transformationsmatrix der Standardwert ist die Identitätsmatrix und 1 in der Diagonale Zellen und 0 alle anderen Elemente besteht aus:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Sie können eine Identitätsmatrix mit der statischen erstellen [ `SKMatrix.MakeIdentity` ](xref:SkiaSharp.SKMatrix.MakeIdentity) Methode:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

Die `SKMatrix` Standardkonstruktor führt *nicht* eine Identitätsmatrix zurück. Es gibt eine Matrix mit der alle Zellen auf NULL festgelegt. Verwenden Sie nicht die `SKMatrix` Konstruktor, wenn Sie solche Zellen ein manuell festlegen möchten.

Wenn SkiaSharp ein grafisches Objekts gerendert wird, wird jeder Punkt (X, y) effektiv zu einer 1-von-3-Matrix mit 1 in der dritten Spalte konvertiert:

<pre>
| x  y  1 |
</pre>

Die 1-von-3-Matrix stellt einen dreidimensionalen Punkt dar, mit die Z-Koordinate, die auf 1 festgelegt. Warum eine zweidimensionale Matrixtransformation erfordert, arbeiten mit drei Dimensionen sind mathematische Ursachen (weiter unten behandelt). Sie können die 1-von-3-Matrix Darstellung eines Punkts in einem 3D-Koordinatensystem, jedoch immer in der 2D-Ebene vorstellen, in der Z gleich 1 ist.

Die 1-von-3-Matrix wird dann mit die Transformationsmatrix multipliziert, und das Ergebnis ist der Punkt im Zeichenbereich gerendert:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Verwenden die standardmäßigen Matrixmultiplikation, sind die konvertierte Punkte wie folgt:

x' = x

y' = y

z' = 1

Dies ist die Standard-Transformation.

Wenn die `Translate` Methode wird aufgerufen, auf die `SKCanvas` -Objekt, der `tx` und `ty` Argumente, die die `Translate` Methode werden die ersten beiden Zellen in der dritten Zeile der Transformationsmatrix:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Die Multiplikation sieht jetzt wie folgt aus:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Hier sind die Formeln für die Transformation:

X' = X + tx

y' = y + Ty

Skalierungsfaktoren haben den Standardwert 1. Beim Aufrufen der `Scale` Methode auf einem neuen `SKCanvas` Objekts, die sich ergebende Transformationsmatrix enthält die `sx` und `sy` Argumente in der diagonalen Zellen:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Die Transformation Formeln lauten wie folgt aus:

X' = Sx – x

y' = Sy – y

Die Transformationsmatrix, die nach dem Aufruf `Skew` enthält die zwei Argumente in den neben der Skalierungsfaktoren Matrixzellen:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Die Transformation-Formeln sind:

X' = X + xSkew – y

y' = ySkew – X + y

Für einen Aufruf von `RotateDegrees` oder `RotateRadians` für ein Winkel von α Transformationsmatrix lautet wie folgt:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Hier sind die Formeln für die Transformation:

X' = cos(α) – X - sin(α) – y

y' = sin(α) – X - cos(α) – y

Wenn α 0 Grad ist, ist es die Identitätsmatrix. Wenn α um 180 Grad ist, lautet die Transformationsmatrix wie folgt:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Eine Drehung um 180 Grad entspricht Kippen eines Objekts, horizontal und vertikal die wird auch durch die Einstellung Skalierungsfaktoren hexadezimalentsprechung für – 1 erreicht.

Alle diese Arten von Transformationen sind als klassifiziert *affine* transformiert. Affine Transformationen beinhalten nicht die dritte Spalte in der Matrix, die die Standardwerte von 0, 0 und 1 bleibt. Der Artikel [ **nicht Affine Transformationen** ](non-affine.md) nicht affine Transformationen beschreibt.

## <a name="matrix-multiplication"></a>Matrizenmultiplikation

Ist ein deutlicher Vorteil bei der Verwendung der Transformationsmatrix, die zusammengesetzte Transformationen von Matrizenmultiplikation, die häufig in der Dokumentation SkiaSharp als bezeichnet wird, abgerufen werden können *Verkettung*. Viele der Transformation-bezogene Methoden in `SKCanvas` finden Sie unter "vor Verkettung" oder "Pre-Concat." Dies bezieht sich Sequenznummern Multiplikation, was wichtig ist, da die Matrixmultiplikation nicht kommutativ ist.

Z. B. die Dokumentation für die [ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) Methode besagt, dass die It "Die aktuelle Matrix mit die angegebene Verschiebung, Pre-Concats" während der Dokumentation für die [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) Methode besagt, dass die It "Die aktuelle Matrix mit dem angegebenen Maßstab Pre-Concats."

Dies bedeutet, dass die Transformation, die durch Aufruf der Methode angegeben, der Multiplikator (der linke Operand ist) und der aktuellen Transformationsmatrix der Multiplikand (den rechten Operand).

Nehmen wir an, die `Translate` wird aufgerufen, gefolgt von `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

Die `Scale` Transformation multipliziert wird die `Translate` für die Matrix der zusammengesetzten Transformierung Transformieren:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` kann aufgerufen werden, bevor `Translate` wie folgt aus:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

In diesem Fall wird die Reihenfolge der Multiplikation umgekehrt, und die Skalierungsfaktoren effektiv auf die Faktoren Übersetzung angewendet werden:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Hier ist die `Scale` -Methode mit einem Pivotpunkt fest:

```csharp
canvas.Scale(sx, sy, px, py);
```

Dies entspricht den folgenden übersetzen und die Skalierung aufrufen:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Die drei Matrizen Transformation multipliziert in umgekehrter Reihenfolge aus, wie die Methoden im Code angezeigt werden:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>Die SKMatrix-Struktur

Die `SKMatrix` Struktur definiert die neun Lese-/Schreibeigenschaften des Typs `float` , die neun Zellen aus, der die Transformationsmatrix entspricht:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` definiert auch eine Eigenschaft namens [ `Values` ](xref:SkiaSharp.SKMatrix.Values) des Typs `float[]`. Diese Eigenschaft kann verwendet werden, um festzulegen, oder rufen Sie neun Werte auf einmal zurückholen in der Reihenfolge `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, und `Persp2`.

Die `Persp0`, `Persp1`, und `Persp2` Zellen werden in diesem Artikel erläuterten [ **nicht Affine Transformationen**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Wenn diese Zellen Standardwert 0, 0 und 1 aufweisen, und klicken Sie dann einen Koordinatenpunkt wie folgt die Transformation multipliziert wird:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

X' = ScaleX – X + SkewX – y + VerschX

y' = SkewX – X + ScaleY – y + VerschY

z' = 1

Dies ist die vollständige zweidimensionalen affine Transformation. Die affine Transformation behält die parallele Linien, was bedeutet, dass ein Rechteck nie in etwas anderes als ein Parallelogramm transformiert wird.

Die `SKMatrix` Struktur definiert mehrere statische Methoden zum Erstellen `SKMatrix` Werte. Diese geben `SKMatrix` Werte:

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single)) mit einem Pivotpunkt
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single)) für einen Winkel im Bogenmaß im Bereich
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single)) für einen Winkel im Bogenmaß zurück, mit einem Pivotpunkt
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single)) mit einem Pivotpunkt
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix` auch definiert mehrere statische Methoden, die Verketten von zwei Matrizen, was bedeutet, dass sie multiplizieren. Diese Methoden werden mit dem Namen [ `Concat` ](xref:SkiaSharp.SKMatrix.Concat*), [ `PostConcat` ](xref:SkiaSharp.SKMatrix.PostConcat*), und [ `PreConcat` ](xref:SkiaSharp.SKMatrix.PreConcat*), und es gibt zwei Versionen der einzelnen. Diese Methoden verfügen über keine Rückgabe von Werten; verweisen Sie stattdessen diese auf vorhandene `SKMatrix` Werte über `ref` Argumente. Im folgenden Beispiel `A`, `B`, und `R` (für "Result") werden alle `SKMatrix` Werte.

Die beiden `Concat` Methoden werden aufgerufen, wie folgt:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Diese führen die folgende Multiplikation:

R = B × EIN

Die anderen Methoden werden nur zwei Parameter aufweisen. Der erste Parameter geändert, und bei der Rückgabe aus dem Aufruf der Methode ist das Produkt von zwei Matrizen enthält. Die beiden `PostConcat` Methoden werden aufgerufen, wie folgt:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Diese Aufrufe ausführen den folgenden Vorgang:

A = A × B

Die beiden `PreConcat` Methoden ähneln:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Diese Aufrufe ausführen den folgenden Vorgang:

A = B × EIN

Die Versionen dieser Methoden mit allen `ref` Argumente sind etwas effizienter, in der zugrunde liegende Implementierungen aufrufen, aber es kann unübersichtlich werden, eine Person Ihren Code lesen und vorausgesetzt, dass alles, was eine `ref` Argument wird geändert, indem die Methode. Darüber hinaus ist es häufig sinnvoll, ein Argument zu übergeben, das Ergebnis eines ist die `Make` Methoden, z.B.:

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

Dies ist die Skalierungstransformation die Verschiebungstransformation multipliziert. In diesem Fall die `SKMatrix` Struktur verfügt über eine Tastenkombination mit einer Methode namens [ `SetScaleTranslate` ](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Dies ist eine von der einige Male, wird er sicher ist, verwenden, die `SKMatrix` Konstruktor. Die `SetScaleTranslate` Methode legt alle neun Zellen der Matrix. Es ist auch sicher ist, verwenden die `SKMatrix` Konstruktor mit der statischen `Rotate` und `RotateDegrees` Methoden:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Diese Methoden führen *nicht* verketten Sie eine Rotationstransformation auf eine vorhandene Transformation. Die Methoden legen Sie alle Zellen der Matrix. Sind sie funktional identisch mit der `MakeRotation` und `MakeRotationDegrees` Methoden mit dem Unterschied, dass sie instanziieren, nicht die `SKMatrix` Wert.

Angenommen, Sie haben eine `SKPath` -Objekt, das Sie anzeigen möchten, aber Sie verwenden möchten, dass es eine etwas andere Ausrichtung oder einen anderen Mittelpunkt. Sie können alle Koordinaten dieses Pfads durch den Aufruf der [ `Transform` ](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix)) -Methode der `SKPath` mit einer `SKMatrix` Argument. Die **Pfad transformieren** Seite veranschaulicht dies. Die [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Klasse verweisen die `HendecagramPath` Objekt in ein Feld verwendet ihren Konstruktor jedoch eine Transformation auf diesen Pfad angewendet:

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

Die `HendecagramPath` Objekt verfügt über einen Mittelpunkt (0, 0), und erweitern Sie die 11 Punkte des Sterns nach außen aus, die von Center von 100 Einheiten in alle Richtungen. Dies bedeutet, dass der Pfad mit positive und negative Koordinaten enthält. Die **Pfad transformieren** Seite mit einem Stern dreimal so groß, und alle positive Koordinaten bevorzugt. Darüber hinaus soll er einen Punkt von den Stern, um die gerade nach oben zeigen nicht mehr an. Er möchte stattdessen für einen Punkt des Sterns direkt nach unten zeigen. (Da das Sternsymbol 11 Punkte verfügt, kann nicht es sowohl haben.) Dies erfordert, dass das Sternsymbol 360 Grad drehen von 22 unterteilt.

Der Konstruktor erstellt ein `SKMatrix` Objekt aus drei separaten Transformationen, die mit der `PostConcat` -Methode mit dem folgenden Muster ein, wobei A, B und C Instanzen von werden `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Dies ist eine Reihe von aufeinander folgenden Multiplikationen, daher ist das Ergebnis wie folgt:

EINE × B-× C

Die aufeinander folgenden Multiplikation helfen zu verstehen, was bewirkt, dass jede Transformation. Die Skalierungstransformation vergrößert die Pfadkoordinaten um den Faktor 3, damit die Koordinaten von –300 und 300 liegen. Die Drehungstransformation dreht das Sternsymbol, um dessen Ursprung. Die Verschiebungstransformation verschiebt sie anschließend, 300 Pixel, rechts und nach unten, sodass alle die Koordinaten werden positive.

Es gibt andere Sequenzen, die die gleiche Matrix zu erzeugen. Hier ist ein anderes:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Den Pfad, um seinen Mittelpunkt zuerst Rotation, und übersetzt sie anschließend 100 Pixel nach rechts und nach unten, also die Koordinaten sind positiv. Der Stern ist dann relativ zur oberen linken Ecke neue, vergrößert Dies ist der Punkt (0, 0).

Die `PaintSurface` Handler kann einfach diesen Pfad Rendern:

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

[![](matrix-images/pathtransform-small.png "Dreifacher Screenshot der Seite Pfad transformieren")](matrix-images/pathtransform-large.png#lightbox "dreifachen Screenshot der Seite Pfad transformieren")

Der Konstruktor des Programms gilt die Matrix für den Pfad mit dem folgenden Aufruf:

```csharp
transformedPath.Transform(matrix);
```

Der Pfad ist *nicht* dieser Matrix als Eigenschaft beibehalten werden sollen. Stattdessen wird es auf alle von den Koordinaten des Pfads der Transformation angewendet. Wenn `Transform` heißt in diesem Fall die Transformation erneut angewendet wird, und die einzige Möglichkeit, die Sie zurückkehren durch Anwenden von einer anderen Matrix, die die Transformation rückgängig gemacht wird. Glücklicherweise die `SKMatrix` Struktur definiert eine [ `TryInvert` ](xref:SkiaSharp.SKMatrix.TryInvert*) Methode, die der Matrix abruft, kehrt eine angegebene Matrix:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Die Methode wird aufgerufen, `TryInverse` da nicht alle Matrizen rückgängig gemacht werden kann, aber eine Matrix nicht invertierbar ist wahrscheinlich nicht für eine Grafik-Transformation verwendet werden.

Sie können auch eine Matrixtransformation Anwenden einer `SKPoint` Wert, der ein Array von Punkten, eine `SKRect`, oder auch nur eine einzelne Zahl innerhalb des Programms. Die `SKMatrix` Struktur unterstützt diese Vorgänge mit einer Auflistung von Methoden, die mit dem Wort beginnen `Map`, wie diese:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Wenn Sie mit dieser letzten Methode verwenden, beachten Sie, dass die `SKRect` Struktur ist nicht in der Lage, eine gedrehtes Rechteck darstellt. Die Methode ist nur sinnvoll für ein `SKMatrix` Wert, der Übersetzung darstellt, und Skalieren von Daten.

## <a name="interactive-experimentation"></a>Interaktives experimentieren

Eine Möglichkeit, ein Gefühl für die Transformation affin ist interaktiv drei Ecken einer Bitmap auf dem Bildschirm verschoben, und sehen, welche Transformation führt. Dies ist die Idee hinter der **Affine Matrix anzeigen** Seite. Diese Seite erfordert zwei weitere Klassen, die auch in anderen Demos verwendet werden:

Die [ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) Klasse zeigt einen lichtdurchlässiger Kreis, der auf dem Bildschirm gezogen werden kann. `TouchPoint` erfordert, dass ein `SKCanvasView` oder ein Element, das ein übergeordnetes Element ist ein `SKCanvasView` haben die [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) angefügt. Legen Sie die `Capture` -Eigenschaft auf `true`fest. In der `TouchAction` -Ereignishandler die Anwendung muss Aufrufen der `ProcessTouchEvent` -Methode in der `TouchPoint` für jede `TouchPoint` Instanz. Gibt die Methode zurück `true` , wenn das Verschieben von Touch-Punkt das touchereignis geführt haben. Darüber hinaus die `PaintSurface` Handler aufrufen muss die `Paint` -Methode in jeder `TouchPoint` Instanz, die an sie übergibt den `SKCanvas` Objekt.

`TouchPoint` Zeigt eine allgemeine Möglichkeit, dass ein visuelles SkiaSharp in einer separaten Klasse gekapselt werden kann. Die Klasse kann Eigenschaften für die Angabe der Merkmale des visuellen Elements definieren und eine Methode mit dem Namen `Paint` mit einer `SKCanvas` Argument rendern kann.

Die `Center` Eigenschaft `TouchPoint` gibt den Speicherort des Objekts. Diese Eigenschaft kann festgelegt werden, um den Speicherort zu initialisieren. die Eigenschaft geändert wird, wenn der Benutzer den Kreis um im Zeichenbereich zieht.

Die **Affine Matrix-Seite anzeigen** erfordert außerdem die [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) Klasse. Diese Klasse zeigt die Zellen einer `SKMatrix` Objekt. Sie verfügt über zwei öffentliche Methoden: `Measure` die Dimensionen der gerenderten Matrix, abrufen und `Paint` angezeigt. Die Klasse enthält eine `MatrixPaint` Eigenschaft vom Typ `SKPaint` , die für eine andere Schriftart, Größe oder Farbe ersetzt werden.

Die [ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) Datei instanziiert den `SKCanvasView` und fügt eine `TouchEffect`. Die [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) Code-Behind-Datei erstellt drei `TouchPoint` Objekte aus, und sie dann auf Positionen für drei Ecken einer Bitmap, die sie über ein eingebettetes lädt festgelegt Ressource:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Eine Matrix affine ist eindeutig durch drei Punkte definiert werden. Die drei `TouchPoint` Objekten entsprechen, in der oberen linken, oberen rechten und unteren linken Ecken der Bitmap. Da es sich bei eine affine Matrix nur ein Rechteck in einem Parallelogramm transformiert werden kann, wird der vierten Stelle von den anderen drei impliziert. Der Konstruktor endet mit einem Aufruf von `ComputeMatrix`, berechnet die Zellen einer `SKMatrix` Objekt aus diesen drei Punkten.

Die `TouchAction` Ereignishandler ruft die `ProcessTouchEvent` Methode `TouchPoint`. Die `scale` Wert wird von Xamarin.Forms-Koordinaten in Pixel konvertiert:

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

Ggf. `TouchPoint` verschoben wurde, und klicken Sie dann die Methode ruft `ComputeMatrix` erneut und erklärt die Oberfläche ungültig.

Die `ComputeMatrix` Methode bestimmt die Matrix, die durch die drei Punkte impliziert. Wird aufgerufen, die Matrix `A` Transformationen, die ein quadratisches ein-Pixel-Rechteck in einem Parallelogramm basierend auf den drei Punkten, zwar die Skalierungstransformation aufgerufen `S` die Bitmap zu einem ein-Pixel-Quadrat-Rechteck skaliert werden kann. Die zusammengesetzte Matrix ist `S` × `A`:

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

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Zum Schluss die `PaintSurface` Methode rendert die Bitmap, die basierend auf dieser Matrix zeigt die Matrix am unteren Rand des Bildschirms und die Touch-Punkte an den drei Ecken der Bitmap gerendert:

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

IOS-Bildschirms unten zeigt das Bitmuster an, wenn die Seite zuerst geladen wird, während es nach dem einige Änderungen von die beiden anderen Bildschirmen angezeigt:

[![](matrix-images/showaffinematrix-small.png "Dreifacher Screenshot der Seite Affine Matrix anzeigen")](matrix-images/showaffinematrix-large.png#lightbox "dreifachen Screenshot der Seite Affine Matrix anzeigen")

Obwohl es scheint, als ob Touch-Punkt die Bitmap, das die Ecken ziehen, die nur eine Illusion. Die Matrix aus den Touch-Punkte berechnet transformiert die Bitmap so, dass die Ecken mit Touch-Punkt übereinstimmen.

Es ist für Benutzer zu verschieben, skalieren und Rotieren Bitmaps nicht durch Ziehen der Ecken natürlicher, aber mit einem oder zwei Fingern direkt auf das Objekt, das ziehen, verkleinern und drehen. Dies wird im nächsten Artikel behandelt [ **Touch-Bearbeitung**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

## <a name="the-reason-for-the-3-by-3-matrix"></a>Der Grund für die 3-Mal-3-Matrix

Möglicherweise erwartet, dass ein zweidimensionaler Grafik-System nur eine 2 x 2-Transformationsmatrix erforderlich wäre:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Dies funktioniert für Skalierung, Drehung und sogar neigen, aber es kann keine die grundlegendste von Transformationen, die Übersetzung ist.

Das Problem besteht darin, dass der 2 x 2-Matrix darstellt. ein *lineare* in zwei Dimensionen zu transformieren. Eine lineare Umformung werden einige grundlegenden arithmetischen Operationen beibehalten, aber die Auswirkungen auf die ist, dass den Punkt (0, 0) in eine lineare Umformung nie geändert wird. Eine lineare Umformung-Übersetzung ist nicht möglich.

In 3D sieht eine lineare Transformationsmatrix folgendermaßen aus:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Die Zelle, die mit der Bezeichnung `SkewXY` bedeutet, die der Wert der X-Koordinate, die basierend auf der Y-Werte neigt; die Zelle `SkewXZ` bedeutet, dass der Wert die X-Koordinate, die basierend auf Werten Z neigt und Werte Analog dazu, für die anderen neigen `Skew` Zellen.

Es ist möglich, durch Festlegen dieses 3D Transformationsmatrix auf einer zweidimensionalen Ebene zu beschränken `SkewZX` und `SkewZY` auf 0 (null) und `ScaleZ` auf 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Wenn die zweidimensionalen Grafiken vollständig auf die Ebene im 3D-Raum gezeichnet werden, in der Z gleich 1 ist, sieht die Multiplikation Transformation folgendermaßen aus:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Alles, was verbleibt der zweidimensionalen Raum, in denen gleich Z 1, aber die `SkewXZ` und `SkewYZ` Zellen werden effektiv zweidimensionalen Übersetzung Faktoren.

Dies ist wie eine dreidimensionale lineare Umformung als eine zweidimensionale nichtlineare Umwandlung dient. (Entsprechend Transformationen in 3D-Grafiken eine 4 x 4-Matrix basiert auf.)

Die `SKMatrix` Struktur in SkiaSharp definiert Eigenschaften für die dritte Zeile:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Ungleich NULL-Werte der `Persp0` und `Persp1` führen Transformationen, die Objekte aus dem zweidimensionalen Raum verschoben werden, wobei Z 1 entspricht. Was geschieht, wenn die Objekte wieder auf diese Ebene verschoben werden in diesem Artikel behandelt wird, auf [ **nicht Affine Transformationen**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
