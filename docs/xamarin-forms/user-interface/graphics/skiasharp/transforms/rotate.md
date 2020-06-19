---
title: Die Drehungstransformation
description: In diesem Artikel werden die Auswirkungen und Animationen erläutert, die mit der skiasharp-Transformation zum Drehen möglich sind. Dies wird mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 520c4c3b61049bf17c2c964523714db196da6839
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84132183"
---
# <a name="the-rotate-transform"></a>Die Drehungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Untersuchen der möglichen Auswirkungen und Animationen mit der skiasharp-Transformation zum Drehen_

Mit der Transformation zum drehen werden skiasharp-Grafik Objekte von der Einschränkungs Einschränkung mit den horizontalen und vertikalen Achsen freigegeben:

![](rotate-images/rotateexample.png "Text rotated around a center")

Zum Drehen eines grafischen Objekts um den Punkt (0, 0) unterstützt skiasharp sowohl eine [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) -Methode als auch eine- [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) Methode:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Ein Kreis von 360 Grad ist identisch mit dem Two-Bogenmaß, sodass er problemlos zwischen den beiden Einheiten konvertiert werden kann. Verwenden Sie das, was praktisch ist. Alle in der .NET-Klasse in der .net- [`Math`](xref:System.Math) Klasse ermittelter Funktionen verwenden die Einheiten des Radials.

Drehung ist im Uhrzeigersinn für steigende Winkel. (Obwohl die Drehung im kartesischen Koordinatensystem nach Konvention gegen den Uhrzeigersinn steht, ist die Drehung im Uhrzeigersinn mit Y-Koordinaten konsistent, die wie in skiasharp zunehmen.) Negative Winkel und Winkel, die größer als 360 Grad sind, sind zulässig.

Die transformationsformeln für die Drehung sind komplexer als die für die Übersetzung und Skalierung. Für einen Winkel von "α" lauten die transformationsformeln wie folgt:

x ' = x • cos (α) – y • sin (α)   

y ' = x • sin (α) + y • cos (α)

Auf der Seite **grundlegende Rotation** wird die- `RotateDegrees` Methode veranschaulicht. Die Datei [**BasicRotate.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) zeigt Text an, dessen Baseline auf der Seite zentriert ist, und rotiert Sie auf der Grundlage eines `Slider` mit einem Bereich von – 360 bis 360. Hier ist der relevante Teil des `PaintSurface` Handlers:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Da die Drehung um die linke obere Ecke der Canvas zentriert ist, wird der Text für die meisten Winkel, die in diesem Programm festgelegt sind, aus dem Bildschirm gedreht:

[![](rotate-images/basicrotate-small.png "Triple screenshot of the Basic Rotate page")](rotate-images/basicrotate-large.png#lightbox "Triple screenshot of the Basic Rotate page")

Sehr häufig sollten Sie mit den folgenden Versionen der [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) -und-Methoden etwas drehen, das sich um einen angegebenen Pivotpunkt dreht [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) :

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

Die **zentrierte Seite Drehung** ist genau wie die **einfache** Rotation, mit der Ausnahme, dass die erweiterte Version von `RotateDegrees` verwendet wird, um den Mittelpunkt der Drehung auf denselben Punkt festzulegen, der zum Positionieren des Texts verwendet wird:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Nun dreht sich der Text um den Punkt, der zum Positionieren des Texts verwendet wird. Dies ist die horizontale Mitte der Baseline des Texts:

[![](rotate-images/centeredrotate-small.png "Triple screenshot of the Centered Rotate page")](rotate-images/centeredrotate-large.png#lightbox "Triple screenshot of the Centered Rotate page")

Wie bei der zentrierten Version der- `Scale` Methode ist die zentrierte Version des `RotateDegrees` Aufrufes eine Verknüpfung. Hier ist die-Methode:

```csharp
RotateDegrees (degrees, px, py);
```

Dieser Befehl entspricht Folgendem:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Sie werden feststellen, dass Sie manchmal `Translate` Aufrufe mit aufrufen kombinieren können `Rotate` . Dies sind z `RotateDegrees` . b. die-und- `DrawText` Aufrufe auf der Seite mit der **Drehung**

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Der `RotateDegrees` Aufruf entspricht zwei `Translate` aufrufen und einer nicht zentrierten `RotateDegrees` :

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Der-Befehl zum `DrawText` Anzeigen von Text an einer bestimmten Position entspricht einem- `Translate` Aufrufsort, gefolgt von `DrawText` am Punkt (0,0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Die beiden aufeinander folgenden `Translate` Aufrufe brechen einander ab:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Konzeptionell werden die beiden Transformationen in der Reihenfolge angewendet, in der Sie im Code angezeigt werden. Der-Befehl `DrawText` zeigt den Text in der oberen linken Ecke des Zeichen Bereichs an. Der-Befehl `RotateDegrees` dreht den Text relativ zur linken oberen Ecke. Anschließend verschiebt der-Befehl `Translate` den Text in den Mittelpunkt der Canvas.

Es gibt in der Regel mehrere Möglichkeiten, Drehung und Übersetzung zu kombinieren. Die **gedrehte Textseite** erstellt folgende Anzeige:

[![](rotate-images/rotatedtext-small.png "Triple screenshot of the Rotated Text page")](rotate-images/rotatedtext-large.png#lightbox "Triple screenshot of the Rotated Text page")

Hier ist der `PaintSurface` Handler der- [`RotatedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) Klasse:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

Der `xCenter` -Wert und der- `yCenter` Wert geben die Mitte des Zeichen Bereichs an. Der `yText` Wert ist ein wenig Offset. Dieser Wert ist die Y-Koordinate, die erforderlich ist, um den Text so zu positionieren, dass er tatsächlich vertikal auf der Seite zentriert ist. Die `for` Schleife legt dann eine Drehung auf der Grundlage der Mitte der Canvas fest. Die Drehung erfolgt in Inkrementen von 30 Grad. Der Text wird mit dem- `yText` Wert gezeichnet. Die Anzahl der Leerzeichen vor dem Wort "drehen" im `text` Wert wurde empirisch festgelegt, um die Verbindung zwischen diesen 12 Text Zeichenfolgen als dodecseck zu gestalten.

Eine Möglichkeit, diesen Code zu vereinfachen, besteht darin, den Drehungs Winkel jedes Mal nach dem Aufruf um 30 Grad zu erhöhen `DrawText` . Dadurch entfällt die Notwendigkeit von Aufrufen von `Save` und `Restore` . Beachten Sie, dass die `degrees` Variable nicht mehr im Hauptteil des Blocks verwendet wird `for` :

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Es ist auch möglich, die einfache Form von zu verwenden, `RotateDegrees` indem der Schleife ein-Befehl vorangestellt wird, `Translate` um alles in den Mittelpunkt der Canvas zu verschieben:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Die geänderte `yText` Berechnung umfasst nicht mehr `yCenter` . Nun zentriert der-Befehl `DrawText` den Text vertikal am oberen Rand der Canvas.

Da die Transformationen konzeptionell angewendet werden, wenn Sie im Code angezeigt werden, ist es oft möglich, mit weiteren globalen Transformationen, gefolgt von weiteren lokalen Transformationen, zu beginnen. Dies ist oft die einfachste Möglichkeit, Drehung und Übersetzung zu kombinieren.

Nehmen wir beispielsweise an, Sie möchten ein grafisches Objekt zeichnen, das um seine Mitte dreht, ähnlich wie ein Planet, der sich auf der Achse dreht. Sie möchten aber auch, dass sich dieses Objekt um den Mittelpunkt des Bildschirms dreht, ähnlich wie ein Planet, der sich um die Sonne dreht.

Hierzu können Sie das Objekt in der oberen linken Ecke der Canvas positionieren und dann eine Animation verwenden, um Sie um diese Ecke zu drehen. Übersetzen Sie das Objekt dann horizontal wie einen Orbital Radius. Wenden Sie nun eine zweite animierte Drehung an, auch um den Ursprung. Dadurch dreht sich das Objekt um die Ecke. Übersetzt nun in den Mittelpunkt der Canvas.

Hier ist der `PaintSurface` Handler, der diese Transformations Aufrufe in umgekehrter Reihenfolge enthält:

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

Die `revolveDegrees` `rotateDegrees` Felder und werden animiert. Dieses Programm verwendet eine andere Animationstechnik, die auf der- Xamarin.Forms [`Animation`](xref:Xamarin.Forms.Animation) Klasse basiert. (Diese Klasse wird in [Kapitel 22 der *Erstellung von Mobile Apps mit Xamarin.Forms * ](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)beschrieben) mit der Überschreibung werden `OnAppearing` zwei `Animation` Objekte mit Rückruf Methoden erstellt und dann `Commit` für eine Animations Dauer aufgerufen:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

Das erste-Objekt erstellt eine `Animation` Animation `revolveDegrees` zwischen 0 Grad und 360 Grad über 10 Sekunden. Die zweite Animation führt `rotateDegrees` Alle 1 Sekunde zwischen 0 Grad und 360 Grad aus und erklärt auch die Oberfläche für die Generierung eines weiteren aufrufsaufruftlers für ungültig `PaintSurface` . Durch die Überschreibung werden `OnDisappearing` diese beiden Animationen abgebrochen:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

Das **hässliche Programm der analogen Uhr** (so aufgerufen, weil eine attraktivere analoge Uhr in einem späteren Artikel beschrieben wird) verwendet die Drehung zum Zeichnen der Minuten-und Stundenmarkierungen der Uhr und zum Drehen der Hände. Das Programm zeichnet die Uhr mithilfe eines beliebigen Koordinatensystems auf Grundlage eines Kreises, der am Punkt (0,0) mit einem Radius von 100 zentriert ist. Er verwendet Übersetzung und Skalierung, um diesen Kreis auf der Seite zu erweitern und zu zentrieren.

Die `Translate` `Scale` Aufrufe und gelten global für die Uhr, sodass Sie die ersten sind, die nach der Initialisierung der Objekte aufgerufen werden sollen `SKPaint` :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}
```

Es gibt 60 Markierungen von zwei unterschiedlichen Größen, die in einem Kreis um die Uhr gezeichnet werden müssen. Der-Befehl `DrawCircle` zeichnet diesen Kreis an dem Punkt (0, – 90), der relativ zur Mitte der Uhr 12:00 entspricht. Der-Befehl `RotateDegrees` erhöht den Drehungs Winkel um 6 Grad nach jedem Teil Strich. Die- `angle` Variable wird nur verwendet, um zu bestimmen, ob ein großer Kreis oder ein kleiner Kreis gezeichnet wird:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Schließlich ruft der `PaintSurface` Handler die aktuelle Uhrzeit ab und berechnet die Drehzeiten für die Stunde, Minute und die zweite Hand. Jede Hand wird an der 12:00-Position gezeichnet, sodass der Drehungs Winkel relativ zum folgenden ist:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

Die Uhr ist sicherlich funktionsfähig, obwohl die Hände Recht grob sind:

[![](rotate-images/uglyanalogclock-small.png "Triple screenshot of the Ugly Analog Clock Text page")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

Eine attraktivere Uhr finden Sie im Artikel [**SVG Path Data in skiasharp (in skiasharp**](../curves/path-data.md)).

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
