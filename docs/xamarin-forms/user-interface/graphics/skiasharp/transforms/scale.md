---
title: Die Skalierungstransformation
description: In diesem Artikel wird die skiasharp-Skalierungs Transformation zum Skalieren von Objekten auf verschiedene Größen untersucht und mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bdf33f499bf43d99436cef815c03d35b27866b80
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140178"
---
# <a name="the-scale-transform"></a>Die Skalierungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Entdecken Sie die skiasharp-Skalierungs Transformation zum Skalieren von Objekten auf verschiedene Größen._

Wie Sie im Artikel [**übersetzen der Transformation**](translate.md) gesehen haben, kann die Translation-Transformation ein grafisches Objekt von einem Speicherort zu einem anderen verschieben. Im Gegensatz dazu ändert die Skalierungs Transformation die Größe des grafischen Objekts:

![](scale-images/scaleexample.png "A tall word scaled in size")

Die Skalierungs Transformation bewirkt auch häufig, dass Grafik Koordinaten verschoben werden, wenn Sie größer werden.

Früher haben Sie zwei transformationsformeln gesehen, die die Auswirkungen der Übersetzungs Faktoren von `dx` und beschreiben `dy` :

x ' = x + DX

y ' = y + dy

Die Skalierungsfaktoren von `sx` und `sy` sind multiplikativ und nicht additiv:

x ' = SX = Stuben

y ' = Sy = Teenie

Die Standardwerte der Übersetzungs Faktoren sind 0. die Standardwerte der Skalierungsfaktoren sind 1.

Die- `SKCanvas` Klasse definiert vier `Scale` Methoden. Bei der ersten [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) Methode handelt es sich um Fälle, in denen Sie denselben horizontalen und vertikalen Skalierungsfaktor wünschen:

```csharp
public void Scale (Single s)
```

Dies wird als *isotrope* Skalierung bezeichnet &mdash; , die in beide Richtungen gleich ist. Bei der isotrope Skalierung wird das Seitenverhältnis des Objekts beibehalten.

Mit der zweiten [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) Methode können Sie unterschiedliche Werte für die horizontale und vertikale Skalierung angeben:

```csharp
public void Scale (Single sx, Single sy)
```

Dies führt zu einer *anisotrope* Skalierung.
Die dritte [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) Methode kombiniert die beiden Skalierungsfaktoren in einem einzelnen `SKPoint` Wert:

```csharp
public void Scale (SKPoint size)
```

Die vierte `Scale` Methode wird in Kürze beschrieben.

Die **grundlegende Skalierungs** Seite veranschaulicht die- `Scale` Methode. Die Datei " [**basicscalepage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) " enthält zwei `Slider` Elemente, mit denen Sie horizontale und vertikale Skalierungsfaktoren zwischen 0 und 10 auswählen können. Die [**BasicScalePage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) -Code Behind-Datei verwendet diese Werte, um aufzurufen, `Scale` bevor ein abgerundetes Rechteck mit einer gestrichelten Linie und einem Text in der oberen linken Ecke der Canvas angezeigt wird:

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

Sie Fragen sich vielleicht: Wie wirken sich die Skalierungsfaktoren auf den von der-Methode zurückgegebenen Wert aus `MeasureText` `SKPaint` ? Die Antwort lautet: überhaupt nicht. `Scale`ist eine Methode von `SKCanvas` . Es wirkt sich nicht auf Elemente aus, die Sie mit einem `SKPaint` Objekt ausführen, bis Sie dieses Objekt verwenden, um etwas auf der Canvas zu erzeugen.

Wie Sie sehen, nimmt alles, was nach dem-aufzurufen gezeichnet wird, `Scale` proportional zu:

[![](scale-images/basicscale-small.png "Triple screenshot of the Basic Scale page")](scale-images/basicscale-large.png#lightbox "Triple screenshot of the Basic Scale page")

Der Text, die Breite der gestrichelten Linie, die Länge der Bindestriche in dieser Zeile, die Rundung der Ecken und der 10-Pixel-Rand zwischen dem linken und oberen Rand des Canvas und das abgerundete Rechteck unterliegen den gleichen Skalierungsfaktoren.

> [!IMPORTANT]
> Mit dem universelle Windows-Plattform wird der nicht ordnungsgemäß skalierte Text nicht ordnungsgemäß dargestellt.

Die anisotrope Skalierung bewirkt, dass die Strichbreite für Zeilen, die an den horizontalen und vertikalen Achsen ausgerichtet sind, unterschiedlich wird (Dies ist auch in der ersten Abbildung auf dieser Seite ersichtlich.) Wenn Sie nicht möchten, dass die Strichbreite von den Skalierungsfaktoren betroffen ist, legen Sie den Wert auf 0 fest, und der Wert ist immer ein Pixel breit, unabhängig von der- `Scale` Einstellung.

Die Skalierung ist relativ zur oberen linken Ecke der Canvas. Dies ist möglicherweise genau das, was Sie möchten, aber es ist möglicherweise nicht der Fall. Angenommen, Sie möchten den Text und das Rechteck an einer anderen Stelle im Zeichenbereich positionieren und ihn relativ zum Mittelpunkt skalieren. In diesem Fall können Sie die vierte Version der- [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) Methode verwenden, die zwei zusätzliche Parameter zum Angeben des Mittelpunkts der Skalierung umfasst:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

Der `px` -Parameter und der- `py` Parameter definieren einen Punkt, der manchmal als *Skalierungs Center* bezeichnet wird, in der skiasharp-Dokumentation aber als Pivotpunkt bezeichnet wird. *pivot point* Dies ist ein Punkt relativ zur oberen linken Ecke des Zeichen Bereichs, der nicht von der Skalierung betroffen ist. Die gesamte Skalierung erfolgt relativ zu diesem Mittelpunkt.

Die Seite " [**zentrierte Skalierung**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) " zeigt, wie dies funktioniert. Der- `PaintSurface` Handler ähnelt dem **einfachen Skalierungs** Programm, mit dem Unterschied, dass der- `margin` Wert so berechnet wird, dass der Text horizontal zentriert wird, was impliziert, dass das Programm am besten im Hochformat funktioniert:

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

In der oberen linken Ecke des gerundeten Rechtecks werden `margin` Pixel von der linken Seite des Zeichen Bereichs und `margin` Pixel vom oberen Rand positioniert. Die letzten zwei Argumente der- `Scale` Methode werden auf diese Werte festgelegt, zuzüglich der Breite und Höhe des Texts, bei der es sich auch um die Breite und Höhe des gerundeten Rechtecks handelt. Dies bedeutet, dass die gesamte Skalierung relativ zum Mittelpunkt dieses Rechtecks ist:

[![](scale-images/centeredscale-small.png "Triple screenshot of the Centered Scale page")](scale-images/centeredscale-large.png#lightbox "Triple screenshot of the Centered Scale page")

Die `Slider` Elemente in diesem Programm haben einen Bereich von &ndash; 10 bis 10. Wie Sie sehen können, bewirken negative Werte vertikaler Skalierung (z. b. auf dem Android-Bildschirm in der Mitte), dass Objekte um die horizontale Achse kippen, die den Mittelpunkt der Skalierung durchläuft. Negative Werte der horizontalen Skalierung (z. b. im UWP-Bildschirm auf der rechten Seite) bewirken, dass Objekte um die vertikale Achse kippen, die den Mittelpunkt der Skalierung durchläuft.

Die Version der [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) -Methode mit Pivot-Punkten ist eine Verknüpfung für eine Reihe von drei `Translate` -und- `Scale` aufrufen. Möglicherweise möchten Sie sehen, wie dies funktioniert, indem Sie die- `Scale` Methode auf der **zentrierten Skalierungs** Seite durch Folgendes ersetzen:

```csharp
canvas.Translate(-px, -py);
```

Dies sind die negativen Werte der pivotpunktkoordinaten.

Führen Sie nun das Programm erneut aus. Sie werden feststellen, dass das Rechteck und der Text so verschoben werden, dass sich das Zentrum in der oberen linken Ecke des Zeichen Bereichs befindet. Sie können es kaum sehen. Die Schieberegler funktionieren natürlich nicht, da das Programm jetzt nicht mehr skaliert wird.

Fügen Sie den grundlegenden-Befehl `Scale` (ohne Skalierungs Center) *vor* diesem-Befehl hinzu `Translate` :

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Wenn Sie mit dieser Übung in anderen Grafik Programmierungs Systemen vertraut sind, denken Sie vielleicht, dass es falsch ist, aber das ist nicht der Fall. Skia verarbeitet aufeinander folgende Transformations Aufrufe etwas anderes als das, was Sie möglicherweise bereits kennen.

Mit den aufeinander folgenden `Scale` -und `Translate` -aufrufen befindet sich der Mittelpunkt des gerundeten Rechtecks immer noch in der oberen linken Ecke, aber Sie können es jetzt in Relation zur oberen linken Ecke des Zeichen Bereichs skalieren. Dies ist auch die Mitte des gerundeten Rechtecks.

Fügen Sie nun vor diesem-Befehl `Scale` einen weiteren-Befehl `Translate` mit den zentrieren-Werten hinzu:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Dadurch wird das skalierte Ergebnis an die ursprüngliche Position zurück verschoben. Diese drei Aufrufe sind äquivalent zu:

```csharp
canvas.Scale(sx, sy, px, py);
```

Die einzelnen Transformationen werden zusammengefügt, sodass die Formel für die Gesamt Transformation wie folgt lautet:

 x ' = SX = (x – px) + px

 y ' = Sy = (y – py) + py

Beachten Sie, dass die Standardwerte von `sx` und `sy` 1 sind. Es ist einfach zu überzeugen, dass der Pivot-Punkt (px, PY) nicht durch diese Formeln transformiert wird. Sie verbleibt an derselben Position relativ zum Zeichenbereich.

Wenn Sie kombinieren `Translate` und `Scale` aufrufen, ist die Reihenfolge wichtig. Wenn das `Translate` nach dem `Scale` steht, werden die Übersetzungs Faktoren effektiv durch die Skalierungsfaktoren skaliert. Wenn der `Translate` vor dem `Scale` steht, werden die Übersetzungs Faktoren nicht skaliert. Dieser Prozess wird bei der Einführung des Subjekts von Transformations Matrizen zu einem besseren besseren Zeitpunkt (falls auch mehr mathematisch).

Die- `SKPath` Klasse definiert eine schreibgeschützte [`Bounds`](xref:SkiaSharp.SKPath.Bounds) Eigenschaft, die einen zurückgibt, `SKRect` der den Umfang der Koordinaten im Pfad definiert. Wenn z. b. die `Bounds` -Eigenschaft aus dem zuvor erstellten Pfad "hendecagram" abgerufen wird, `Left` sind die-Eigenschaft und die-Eigenschaft `Top` des Rechtecks ungefähr – 100, die-Eigenschaft und die-Eigenschaft `Right` `Bottom` sind ungefähr 100, und die `Width` -Eigenschaft 200 und die- `Height` Eigenschaft (Die meisten der tatsächlichen Werte sind etwas weniger, da die Punkte der Sterne durch einen Kreis mit dem RADIUS 100 definiert werden, aber nur der obere Punkt ist parallel zur horizontalen oder vertikalen Achse.)

Die Verfügbarkeit dieser Informationen impliziert, dass es möglich sein sollte, Skalierungs-und Übersetzungs Faktoren abzuleiten, die für die Skalierung eines Pfads auf die Größe des Canvas geeignet sind. Die Seite [**anisotrope Skalierung**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) veranschaulicht dies mit dem 11-pointierten Stern. Eine *anisotrope* Skala bedeutet, dass Sie in horizontaler und vertikaler Richtung ungleich ist, was bedeutet, dass der Stern das ursprüngliche Seitenverhältnis nicht beibehält. Im folgenden ist der relevante Code im- `PaintSurface` Handler aufgeführt:

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

Das `pathBounds` Rechteck wird in der Nähe des oberen Bereichs dieses Codes abgerufen und später mit der Breite und Höhe des Zeichen Bereichs im-Befehl verwendet `Scale` . Durch diesen eigenständigen Aufrufe werden die Koordinaten des Pfads skaliert, wenn er durch den-Befehl gerendert wird, `DrawPath` aber der Stern wird in der oberen rechten Ecke der Canvas zentriert. Es muss nach unten und nach links verschoben werden. Dies ist die Aufgabe des `Translate` Aufrufes. Diese beiden Eigenschaften von `pathBounds` sind ungefähr – 100, sodass die Übersetzungs Faktoren ungefähr 100 betragen. Da der `Translate` -Rückruf nach dem `Scale` -Befehl erfolgt, werden diese Werte effektiv durch die Skalierungsfaktoren skaliert, sodass Sie den Mittelpunkt des Stern in den Mittelpunkt der Canvas verschieben:

[![](scale-images/anisotropicscaling-small.png "Triple screenshot of the Anisotropic Scaling page")](scale-images/anisotropicscaling-large.png#lightbox "Triple screenshot of the Anisotropic Scaling page")

Eine andere Möglichkeit `Scale` , die-und- `Translate` Aufrufe zu überprüfen, besteht darin, die Auswirkung in umgekehrter Reihenfolge zu bestimmen: der `Translate` Aufruf verschiebt den Pfad so, dass er vollständig sichtbar ist, aber in der oberen linken Ecke der Canvas ausgerichtet ist. `Scale`Mit der-Methode wird dieser Stern dann relativ zur oberen linken Ecke vergrößert.

Es scheint, dass der Stern ein wenig größer als der Zeichenbereich ist. Das Problem ist die Strichbreite. Die `Bounds` -Eigenschaft von `SKPath` gibt die Dimensionen der Koordinaten an, die im Pfad codiert sind, und das ist das, was das Programm zum Skalieren verwendet. Wenn der Pfad mit einer bestimmten Strichbreite gerendert wird, ist der gerenderte Pfad größer als der Zeichenbereich.

Um dieses Problem zu beheben, müssen Sie dies kompensieren. Ein einfacher Ansatz in diesem Programm besteht darin, die folgende Anweisung direkt vor dem-Befehl hinzuzufügen `Scale` :

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Dadurch wird das `pathBounds` Rechteck um 1,5 Einheiten auf allen vier Seiten vergrößert. Dies ist nur eine sinnvolle Lösung, wenn die Stroke JOIN-Anweisung gerundet wird. Eine Gehrungs JOIN-Anweisung kann länger sein und ist schwierig zu berechnen.

Sie können auch eine ähnliche Technik mit Text verwenden, wie die Seite " **anisotrope Text** " veranschaulicht. Hier ist der relevante Teil des `PaintSurface` Handlers aus der- [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) Klasse:

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

Es handelt sich um eine ähnliche Logik, und der Text wird auf der Grundlage des Text Begrenzungen-Rechtecks, das von zurückgegeben wird, auf die Größe der Seite erweitert `MeasureText` :

[![](scale-images/anisotropictext-small.png "Triple screenshot of the Anisotropic Test page")](scale-images/anisotropictext-large.png#lightbox "Triple screenshot of the Anisotropic Test page")

Wenn Sie das Seitenverhältnis der grafischen Objekte beibehalten müssen, sollten Sie die isotrope Skalierung verwenden. Die Seite für die **isotrope Skalierung** zeigt dies für den 11-pointierten Stern. Konzeptionell sind die Schritte zum Anzeigen eines grafischen Objekts in der Mitte der Seite mit der isotropischen Skalierung:

- Übersetzt den Mittelpunkt des grafischen Objekts in die linke obere Ecke.
- Skalieren Sie das Objekt basierend auf dem minimalen der horizontalen und vertikalen Seiten Dimensionen dividiert durch die grafischen Objekt Abmessungen.
- Übersetzt den Mittelpunkt des skalierten Objekts in den Mittelpunkt der Seite.

[`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs)Führt diese Schritte in umgekehrter Reihenfolge aus, bevor der Stern angezeigt wird:

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

Der Code zeigt auch den Stern 10 mehrmals an, wobei der Skalierungsfaktor um 10% verringert und die Farbe von rot in blau geändert wird:

[![](scale-images/isotropicscaling-small.png "Triple screenshot of the Isotropic Scaling page")](scale-images/isotropicscaling-large.png#lightbox "Triple screenshot of the Isotropic Scaling page")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
