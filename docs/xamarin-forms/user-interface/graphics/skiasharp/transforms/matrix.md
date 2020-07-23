---
title: Matrix Transformationen in skiasharp
description: In diesem Artikel werden die skiasharp-Transformationen mit der vielseitigen Transformationsmatrix vertieft und mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 020319761ba1274495b7595a0d18435f98a5f990
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937175"
---
# <a name="matrix-transforms-in-skiasharp"></a>Matrix Transformationen in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Tiefere Einblicke in skiasharp-Transformationen mit der vielseitigen Transformationsmatrix_

Alle Transformationen, die auf das- `SKCanvas` Objekt angewendet werden, werden in einer einzelnen Instanz der- [`SKMatrix`](xref:SkiaSharp.SKMatrix) Struktur konsolidiert. Dabei handelt es sich um eine standardmäßige 3-bis 3-Transformationsmatrix, die mit denen in allen modernen 2D-Grafiksystemen vergleichbar ist.

Wie Sie gesehen haben, können Sie Transformationen in skiasharp verwenden, ohne die Transformationsmatrix zu kennen, aber die Transformationsmatrix ist aus theoretischer Sicht wichtig, und es ist entscheidend, wenn Sie Transformationen zum Ändern von Pfaden oder zur Verarbeitung komplexer Berührungs Eingaben verwenden, die beide in diesem Artikel und in der nächsten erläutert werden.

![Eine Bitmap, die einer affinen Transformation unterliegt.](matrix-images/matrixtransformexample.png)

Die aktuelle Transformationsmatrix, die auf das angewendet `SKCanvas` wird, ist jederzeit verfügbar, indem Sie auf die schreibgeschützte [`TotalMatrix`](xref:SkiaSharp.SKCanvas.TotalMatrix) Eigenschaft zugreifen. Sie können mithilfe der-Methode eine neue Transformationsmatrix festlegen [`SetMatrix`](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix)) , und Sie können diese Transformationsmatrix in Standardwerte wiederherstellen, indem Sie aufrufen [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) .

Der einzige andere `SKCanvas` Member, der direkt mit der Matrix Transformation des Canvas funktioniert, besteht darin [`Concat`](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@)) , dass zwei Matrizen verkettet werden, indem Sie miteinander multipliziert werden.

Die Standard Transformationsmatrix ist die Identitätsmatrix und besteht aus 1 in den diagonalen Zellen und 0 (null).

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Sie können eine Identitätsmatrix mit der statischen- [`SKMatrix.MakeIdentity`](xref:SkiaSharp.SKMatrix.MakeIdentity) Methode erstellen:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

Der `SKMatrix` Standardkonstruktor gibt *keine* Identitätsmatrix zurück. Sie gibt eine Matrix zurück, bei der alle Zellen auf 0 (null) festgelegt sind. Verwenden Sie den- `SKMatrix` Konstruktor nicht, es sei denn, Sie planen, diese Zellen manuell festzulegen.

Wenn skiasharp ein grafisches Objekt rendert, wird jeder Punkt (x, y) effektiv in eine 1-x-3-Matrix mit einem 1 in der dritten Spalte konvertiert:

<pre>
| x  y  1 |
</pre>

Diese 1-x-3-Matrix stellt einen dreidimensionalen Punkt dar, bei dem die Z-Koordinate auf 1 festgelegt ist. Es gibt mathematische Gründe (später erläutert), warum eine zweidimensionale Matrix Transformation in drei Dimensionen funktionieren muss. Sie können sich diese 1-by-3-Matrix als einen Punkt in einem 3D-Koordinatensystem vorstellen, aber immer auf der 2D-Ebene, wobei Z gleich 1 ist.

Diese 1-mal-3-Matrix wird dann mit der Transformationsmatrix multipliziert, und das Ergebnis ist der Punkt, der auf der Canvas gerendert wird:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Bei Verwendung der Standard Matrix Multiplikation lauten die konvertierten Punkte wie folgt:

`x' = x`

`y' = y`

`z' = 1`

Dies ist die Standard Transformation.

Wenn die `Translate` -Methode für das `SKCanvas` -Objekt aufgerufen wird, `tx` werden das-Argument und das-Argument der- `ty` `Translate` Methode zu den ersten beiden Zellen in der dritten Zeile der Transformationsmatrix:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Die Multiplikation ist jetzt wie folgt:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Hier sind die transformationsformeln:

`x' = x + tx`

`y' = y + ty`

Skalierungsfaktoren haben den Standardwert 1. Wenn Sie die `Scale` -Methode für ein neues `SKCanvas` -Objekt aufzurufen, enthält die resultierende Transformationsmatrix das `sx` -Argument und das- `sy` Argument in den diagonalen Zellen:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Die transformationsformeln lauten wie folgt:

`x' = sx · x`

`y' = sy · y`

Die Transformationsmatrix nach dem Aufruf `Skew` von enthält die beiden Argumente in den Matrixzellen neben den Skalierungsfaktoren:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Die transformationsformeln lauten wie folgt:

`x' = x + xSkew · y`

`y' = ySkew · x + y`

Wenn Sie `RotateDegrees` oder `RotateRadians` für einen Winkel von "α" abrufen, ist die Transformationsmatrix wie folgt:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Hier sind die transformationsformeln:

`x' = cos(α) · x - sin(α) · y`

`y' = sin(α) · x - cos(α) · y`

Wenn "α" 0 Grad ist, handelt es sich um die Identitätsmatrix. Wenn "α" 180 Grad ist, sieht die Transformationsmatrix wie folgt aus:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Eine 180-Grad-Drehung entspricht dem horizontalen und vertikalen Kippen eines Objekts. Dies wird auch durch Festlegen der Skalierungsfaktoren – 1 erreicht.

Alle diese Arten von Transformationen werden als *affine* Transformationen klassifiziert. Affine Transformationen enthalten niemals die dritte Spalte der Matrix, die mit den Standardwerten 0, 0 und 1 verbleibt. Der Artikel [**nicht affine Transformationen**](non-affine.md) erläutert nicht affine Transformationen.

## <a name="matrix-multiplication"></a>Matrix Multiplikation

Ein bedeutender Vorteil bei der Verwendung der Transformationsmatrix besteht darin, dass zusammengesetzte Transformationen durch die Matrix Multiplikation abgerufen werden können. Dies wird häufig in der skiasharp-Dokumentation als *Verkettung*bezeichnet. Viele der Transformations bezogenen Methoden in `SKCanvas` verweisen auf "vorverkettung" oder "vorverkettung". Dies bezieht sich auf die Reihenfolge der Multiplikation, was wichtig ist, da die Matrix Multiplikation nicht commutativ ist.

Die Dokumentation für die-Methode besagt z. b., [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) dass Sie die aktuelle Matrix mit der angegebenen Übersetzung vorverketten kann, während die Dokumentation für die- [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) Methode besagt, dass Sie die aktuelle Matrix mit der angegebenen Skala vorab verketten soll.

Dies bedeutet, dass die durch den Methoden Aufrufwert angegebene Transformation der Multiplikator (der linke Operand) und die aktuelle Transformationsmatrix der Multiplikand (der rechte Operand) ist.

Nehmen wir `Translate` an, dass aufgerufen wird, gefolgt von `Scale` :

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

Die `Scale` Transformation wird mit der `Translate` Transformation für die zusammengesetzte Transformationsmatrix multipliziert:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale`kann vor wie folgt aufgerufen werden `Translate` :

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

In diesem Fall wird die Reihenfolge der Multiplikation rückgängig gemacht, und die Skalierungsfaktoren werden effektiv auf die Übersetzungs Faktoren angewendet:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Hier ist die- `Scale` Methode mit einem Pivotpunkt:

```csharp
canvas.Scale(sx, sy, px, py);
```

Dies entspricht den folgenden Übersetzungs-und Skalierungs aufrufen:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Die drei Transformations Matrizen werden in umgekehrter Reihenfolge mit der Art und Weise multipliziert, wie die Methoden im Code erscheinen:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>Die skmatrix-Struktur

Die `SKMatrix` Struktur definiert neun Lese-/Schreibeigenschaften vom Typ `float` , die den neun Zellen der Transformationsmatrix entsprechen:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix`definiert auch eine Eigenschaft mit dem Namen [`Values`](xref:SkiaSharp.SKMatrix.Values) vom Typ `float[]` . Diese Eigenschaft kann verwendet werden, um die neun Werte in einem Satz in der Reihenfolge `ScaleX` , `SkewX` , `TransX` , `SkewY` , `ScaleY` , `TransY` , `Persp0` , `Persp1` und `Persp2` festzulegen oder abzurufen.

Die `Persp0` `Persp1` Zellen, und `Persp2` werden im Artikel [**nicht affine Transformationen**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)erläutert. Wenn diese Zellen die Standardwerte 0, 0 und 1 aufweisen, wird die Transformation mit einem Koordinatenpunkt wie dem folgenden multipliziert:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

`x' = ScaleX · x + SkewX · y + TransX`

`y' = SkewX · x + ScaleY · y + TransY`

`z' = 1`

Dies ist die gesamte zweidimensionale affine Transformation. Die affine Transformation behält parallele Zeilen bei, was bedeutet, dass ein Rechteck nie in etwas anderes als ein Parallelogramm transformiert wird.

Die- `SKMatrix` Struktur definiert mehrere statische Methoden, um Werte zu erstellen `SKMatrix` . Alle Rückgabe `SKMatrix` Werte:

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single))mit einem Pivotpunkt
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single))für einen Winkel im Bogenmaße
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single))ein Winkel im Bogenmaße mit einem Pivotpunkt
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single))mit einem Pivotpunkt
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix`definiert auch mehrere statische Methoden, die zwei Matrizen verketten, was bedeutet, Sie zu multiplizieren. Diese Methoden heißen [`Concat`](xref:SkiaSharp.SKMatrix.Concat*) , [`PostConcat`](xref:SkiaSharp.SKMatrix.PostConcat*) und [`PreConcat`](xref:SkiaSharp.SKMatrix.PreConcat*) , und es gibt jeweils zwei Versionen. Diese Methoden haben keine Rückgabewerte. Stattdessen verweisen Sie auf vorhandene `SKMatrix` Werte durch `ref` Argumente. Im folgenden Beispiel `A` `B` sind, und `R` (für "result") alle `SKMatrix` Werte.

Die beiden `Concat` Methoden werden wie folgt aufgerufen:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Folgende Multiplikation wird durchgeführt:

`R = B × A`

Die anderen Methoden verfügen nur über zwei Parameter. Der erste Parameter wird geändert, und bei der Rückgabe des Methoden Aufrufes enthält das Produkt der beiden Matrizen. Die beiden `PostConcat` Methoden werden wie folgt aufgerufen:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Diese Aufrufe führen den folgenden Vorgang aus:

`A = A × B`

Die beiden `PreConcat` Methoden ähneln einander:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Diese Aufrufe führen den folgenden Vorgang aus:

`A = B × A`

Die Versionen dieser Methoden mit allen `ref` Argumenten sind beim Aufrufen der zugrunde liegenden Implementierungen etwas effizienter, aber es kann verwirrend sein, dass jemand Ihren Code liest und davon ausgeht, dass alles mit einem `ref` Argument von der-Methode geändert wird. Außerdem ist es häufig sinnvoll, ein Argument zu übergeben, das sich aus einer der Methoden ergibt `Make` , z. b.:

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

Dies ist die Skalierungs Transformation, multipliziert mit der Translation-Transformation. In diesem speziellen Fall stellt die- `SKMatrix` Struktur eine Verknüpfung mit einer Methode mit dem Namen bereit [`SetScaleTranslate`](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)) :

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Dies ist eine der wenigen Male, wenn es sicher ist, den `SKMatrix` Konstruktor zu verwenden. Die- `SetScaleTranslate` Methode legt alle neun Zellen der Matrix fest. Es ist auch sicher, den `SKMatrix` Konstruktor mit den statischen `Rotate` Methoden und zu verwenden `RotateDegrees` :

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Diese Methoden verketten *keine* Transformation zum Drehen zu einer vorhandenen Transformation. Die-Methoden legen alle Zellen der Matrix fest. Sie sind funktionell identisch mit der `MakeRotation` -Methode und der- `MakeRotationDegrees` Methode, außer dass Sie den Wert nicht instanziieren `SKMatrix` .

Angenommen, Sie verfügen über ein `SKPath` Objekt, das Sie anzeigen möchten, aber Sie bevorzugen eine etwas andere Ausrichtung oder einen anderen Mittelpunkt. Sie können alle Koordinaten dieses Pfades ändern, indem Sie die- [`Transform`](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix)) Methode von `SKPath` mit einem- `SKMatrix` Argument aufrufen. Diese Vorgehensweise wird auf der Seite **Pfad Transformation** veranschaulicht. Die- [`PathTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Klasse verweist auf das- `HendecagramPath` Objekt in einem-Feld, verwendet jedoch ihren Konstruktor, um eine Transformation auf diesen Pfad anzuwenden:

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

Das `HendecagramPath` -Objekt verfügt über eine Mitte bei (0,0), und die 11 Punkte des Sterns werden von diesem Mittelpunkt um 100 Einheiten in allen Richtungen erweitert. Dies bedeutet, dass der Pfad sowohl positive als auch negative Koordinaten hat. Die Seite **Pfad Transformation** bevorzugt mit einem Stern dreimal so groß und mit allen positiven Koordinaten. Darüber hinaus soll ein Punkt des Sterns nicht auf einen geraden Punkt zeigen. Stattdessen wird ein Punkt des Sterns angezeigt, um auf den Punkt zu zeigen. (Da der Stern 11 Punkte hat, kann er nicht beides aufweisen.) Dies erfordert, dass der Stern um 360 Grad dividiert durch 22 gedreht wird.

Der-Konstruktor erstellt ein- `SKMatrix` Objekt aus drei separaten Transformationen mithilfe der- `PostConcat` Methode mit dem folgenden Muster, wobei A, B und C Instanzen von sind `SKMatrix` :

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Dabei handelt es sich um eine Reihe aufeinander folgender Multiplikationen, sodass das Ergebnis wie folgt aussieht:

`A × B × C`

Die aufeinander folgenden Multiplikationen helfen dabei, zu verstehen, was jede Transformation bewirkt. Die Skalierungs Transformation erhöht die Größe der Pfad Koordinaten um den Faktor 3, sodass die Koordinaten zwischen – 300 und 300 liegen. Die Transformation zum drehen dreht den Stern um seinen Ursprung. Die Translation-Transformation verschiebt Sie dann um 300 Pixel nach rechts und unten, sodass alle Koordinaten positiv werden.

Es gibt weitere Sequenzen, die dieselbe Matrix ergeben. Dies ist ein weiterer:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Dadurch wird der Pfad zuerst um seinen Mittelpunkt gedreht und dann 100 Pixel nach rechts und nach unten übersetzt, sodass alle Koordinaten positiv sind. Der Stern wird dann relativ zu seiner neuen linken oberen Ecke, dem Punkt (0, 0), um die Größe erweitert.

Der `PaintSurface` Handler kann einfach diesen Pfad Rendering:

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

Sie wird in der oberen linken Ecke der Canvas angezeigt:

[![Dreifacher Screenshot der Seite "Pfad Transformation"](matrix-images/pathtransform-small.png)](matrix-images/pathtransform-large.png#lightbox "Dreifacher Screenshot der Seite "Pfad Transformation"")

Der Konstruktor dieses Programms wendet die Matrix mit dem folgenden-Befehl auf den Pfad an:

```csharp
transformedPath.Transform(matrix);
```

Der Pfad behält diese Matrix *nicht* als Eigenschaft bei. Stattdessen wird die Transformation auf alle Koordinaten des Pfads angewendet. Wenn `Transform` erneut aufgerufen wird, wird die Transformation erneut angewendet, und Sie können zurückgehen, indem Sie eine andere Matrix anwenden, die die Transformation rückgängig macht. Glücklicherweise definiert die `SKMatrix` Struktur eine Methode, die die Matrix abruft, die [`TryInvert`](xref:SkiaSharp.SKMatrix.TryInvert*) eine bestimmte Matrix umkehrt:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Die-Methode wird aufgerufen `TryInverse` , weil nicht alle Matrizen invertierbar sind, aber für eine Grafik Transformation ist die Wahrscheinlichkeit, dass eine nicht Invertier Bare Matrix nicht verwendet werden kann.

Sie können auch eine Matrix Transformation auf einen- `SKPoint` Wert, ein Array von Punkten, eine `SKRect` oder sogar nur eine einzelne Zahl innerhalb des Programms anwenden. Die- `SKMatrix` Struktur unterstützt diese Vorgänge mit einer Auflistung von Methoden, die mit dem Wort beginnen, z. b. `Map` :

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Wenn Sie diese letzte Methode verwenden, denken Sie daran, dass die `SKRect` Struktur kein gedrehtes Rechteck darstellen kann. Die-Methode ist nur für einen-Wert sinnvoll, der `SKMatrix` Übersetzung und Skalierung darstellt.

## <a name="interactive-experimentation"></a>Interaktives experimentieren

Eine Möglichkeit, ein Gefühl für die affine Transformation zu erhalten, besteht darin, die drei Ecken einer Bitmap auf dem Bildschirm interaktiv zu verschieben und die Transformations Ergebnisse anzuzeigen. Dies ist die Idee hinter der Seite **affine Matrix anzeigen** . Diese Seite erfordert zwei weitere Klassen, die auch in anderen Demos verwendet werden:

Die- [`TouchPoint`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) Klasse zeigt einen durchlässigen Kreis an, der um den Bildschirm gezogen werden kann. `TouchPoint`erfordert, dass ein- `SKCanvasView` Element oder ein-Element, das ein übergeordnetes Element von ist, `SKCanvasView` den [`TouchEffect`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) angefügten aufweisen Setzen Sie die `Capture`-Eigenschaft auf `true`. Im- `TouchAction` Ereignishandler muss das Programm die- `ProcessTouchEvent` Methode in `TouchPoint` für jede Instanz von abrufen `TouchPoint` . Die Methode gibt zurück, `true` Wenn das Berührungs Ereignis zum Verschieben des Berührungs Punkts geführt hat. Außerdem muss der `PaintSurface` Handler die- `Paint` Methode in jeder Instanz aufzurufen `TouchPoint` und ihm das- `SKCanvas` Objekt übergeben.

`TouchPoint`veranschaulicht eine gängige Methode, mit der ein skiasharp-visuelles Element in einer separaten Klasse gekapselt werden kann. Die Klasse kann Eigenschaften zum Angeben der Merkmale des visuellen Elements definieren, und eine Methode `Paint` mit dem Namen mit einem `SKCanvas` Argument kann Sie darstellen.

Die- `Center` Eigenschaft von `TouchPoint` gibt den Speicherort des-Objekts an. Diese Eigenschaft kann festgelegt werden, um den Speicherort zu initialisieren. die-Eigenschaft ändert sich, wenn der Benutzer den Kreis um den Zeichenbereich zieht.

Die **Seite affine Matrix anzeigen** erfordert auch die- [`MatrixDisplay`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) Klasse. Diese Klasse zeigt die Zellen eines `SKMatrix` Objekts an. Sie verfügt über zwei öffentliche Methoden: `Measure` zum Abrufen der Dimensionen der gerenderten Matrix und `Paint` zum anzeigen. Die-Klasse enthält eine `MatrixPaint` Eigenschaft vom Typ `SKPaint` , die für eine andere Schriftgröße oder-Farbe ersetzt werden kann.

Die Datei [**showaffinematrixpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) instanziiert `SKCanvasView` und fügt einen an `TouchEffect` . Mit der [**ShowAffineMatrixPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) -Code Behind-Datei werden drei `TouchPoint` Objekte erstellt und dann auf Positionen festgelegt, die drei Ecken einer Bitmap entsprechen, die von einer eingebetteten Ressource geladen werden:

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

Eine affine Matrix wird durch drei Punkte eindeutig definiert. Die drei `TouchPoint` -Objekte entsprechen der oberen linken, oberen rechten und unteren linken Ecke der Bitmap. Da eine affine Matrix nur in der Lage ist, ein Rechteck in ein Parallelogramm umzuwandeln, wird der vierte Punkt von den anderen drei Punkten impliziert. Der Konstruktor endet mit einem-Befehl `ComputeMatrix` , der die Zellen eines `SKMatrix` Objekts aus diesen drei Punkten berechnet.

Der- `TouchAction` Handler Ruft die- `ProcessTouchEvent` Methode jeder-Methode auf `TouchPoint` . Der `scale` Wert konvertiert von Xamarin.Forms Koordinaten in Pixel:

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

Wenn eine `TouchPoint` verschoben wurde, ruft die-Methode `ComputeMatrix` erneut auf und macht die-Oberfläche ungültig.

Die- `ComputeMatrix` Methode bestimmt die Matrix, die von diesen drei Punkten impliziert wird. Mit der aufgerufenen Matrix `A` wird ein quadratisches quadratisches Rechteck in ein Parallelogramm transformiert, das auf den drei Punkten basiert, während die Skalierungs Transformation mit dem Namen `S` die Bitmap auf ein quadratisches Quadrat Rechteck skaliert. Die zusammengesetzte Matrix ist `S` × `A` :

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

Zum Schluss `PaintSurface` rendert die-Methode die Bitmap auf der Grundlage dieser Matrix, zeigt die Matrix unten auf dem Bildschirm an und rendert die Berührungspunkte in den drei Ecken der Bitmap:

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

Der unten stehende IOS-Bildschirm zeigt die Bitmap an, wenn die Seite zum ersten Mal geladen wird, während die beiden anderen Bildschirme Sie nach einer gewissen Bearbeitung anzeigen:

[![Dreifacher Screenshot der Seite "affine Matrix anzeigen"](matrix-images/showaffinematrix-small.png)](matrix-images/showaffinematrix-large.png#lightbox "Dreifacher Screenshot der Seite "affine Matrix anzeigen"")

Obwohl es so aussieht, als ob die Berührungspunkte die Ecken der Bitmap ziehen, ist das nur eine Illusion. Die von den Berührungspunkten berechnete Matrix wandelt die Bitmap um, sodass die Ecken mit den Berührungspunkten übereinstimmen.

Es ist natürlicher, dass Benutzer Bitmaps verschieben, ihre Größe ändern und rotieren, nicht indem Sie die Ecken ziehen, sondern mit einem oder zwei Fingern direkt auf das-Objekt ziehen, ein-und verkleinern und drehen. Dies wird im nächsten Artikel zur [**Berührungs Bearbeitung**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)behandelt.

## <a name="the-reason-for-the-3-by-3-matrix"></a>Der Grund für die 3 x 3-Matrix.

Es wird möglicherweise erwartet, dass ein zweidimensionales Grafiksystem nur eine 2 x 2-Transformationsmatrix erfordert:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Dies funktioniert bei der Skalierung, Drehung und sogar Neigung, aber es ist nicht in der Lage, die grundlegendsten Transformationen, d. h. Übersetzung, zu unterstützen.

Das Problem besteht darin, dass die 2 x 2-Matrix eine *lineare* Transformation in zwei Dimensionen darstellt. Eine lineare Transformation bewahrt einige grundlegende arithmetische Operationen auf, aber eine der Implikationen ist, dass eine lineare Transformation nie den Punkt (0,0) ändert. Eine lineare Transformation macht eine Übersetzung unmöglich.

In drei Dimensionen sieht eine lineare Transformationsmatrix wie folgt aus:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Die Zelle mit der Bezeichnung `SkewXY` bedeutet, dass der Wert die x-Koordinate auf der Grundlage von Y-Werten verzerrt. die Zelle `SkewXZ` bedeutet, dass der Wert die x-Koordinate auf der Grundlage der Werte von Z verzerrt, und die Werte neigen gleichermaßen für die anderen `Skew` Zellen.

Es ist möglich, diese 3D-Transformationsmatrix auf eine zweidimensionale Ebene zu beschränken `SkewZX` , indem und `SkewZY` auf 0 und `ScaleZ` auf 1 festgelegt werden:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Wenn die zweidimensionalen Grafiken vollständig auf der Ebene in 3D-Raum gezeichnet werden, wobei Z auf 1 festgelegt ist, sieht die Transformation für Transformationen wie folgt aus:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Alles bleibt auf der zweidimensionalen Ebene, wobei Z gleich 1 ist, aber `SkewXZ` die `SkewYZ` Zellen und werden effektiv zu zweidimensionalen Übersetzungs Faktoren.

Auf diese Weise fungiert eine dreidimensionale lineare Transformation als zweidimensionale, nichtlineare Transformation. (Analog dazu basieren Transformationen in 3D-Grafiken auf einer 4 x 4-Matrix.)

Die `SKMatrix` Struktur in skiasharp definiert Eigenschaften für die dritte Zeile:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Werte ungleich NULL von `Persp0` und `Persp1` führen zu Transformationen, die Objekte aus der zweidimensionalen Ebene verschieben, wobei Z gleich 1 ist. Was geschieht, wenn diese Objekte zurück in diese Ebene verschoben werden, wird in dem Artikel zu [**nicht affinen Transformationen**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)behandelt.

## <a name="related-links"></a>Ähnliche Themen

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
