---
title: Die Drehungstransformation
description: In diesem Artikel untersucht die Effekten und Animationen, die mit der die Drehungstransformation SkiaSharp möglich und wird dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 399f19ba4ec1ed8494e8269fc4cd0682b466a31a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056554"
---
# <a name="the-rotate-transform"></a>Die Drehungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Erkunden Sie die Auswirkungen und Animationen, die mit der die Drehungstransformation SkiaSharp möglich_

Mit der Rotationstransformation befreien Sie sich SkiaSharp-Graphics-Objekten für die Einschränkung der Ausrichtung mit der horizontalen und vertikalen Achsen:

![](rotate-images/rotateexample.png "Text, um einen Mittelpunkt gedreht")

Zum Drehen eines grafischen Objekts, um den Punkt (0, 0), SkiaSharp unterstützt sowohl eine [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) Methode und eine [ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) Methode:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Ein Kreis von 360 Grad ist Twoπ Bogenmaß identisch, daher ist es einfach für die Konvertierung zwischen den zwei Einheiten. Verwenden Sie je nachdem, was praktisch ist. Alle trigonometrischen Funktionen in .NET [ `Math` ](xref:System.Math) -Klasse verwenden Einheiten im Bogenmaß.

Rotation ist zum Erhöhen der Winkel im Uhrzeigersinn. (Obwohl Drehung auf die kartesisches Koordinatensystem gegen den Uhrzeigersinn gemäß der Konvention ist, entspricht Drehung im Uhrzeigersinn um Y-Koordinaten erhöhen, wie SkiaSharp ausfällt.) Negativer Winkel und Winkel, die größer als 360 Grad zulässig sind.

Die Transformation Formeln für die Rotation sind komplexer als die für übersetzen und Skalierung. Für einen Winkel von α sind die Formeln für die Transformation:

X' = x•cos(α) – y•sin(α)   

y' = x•sin(α) + y•cos(α)

Die **grundlegende Drehen** Seite zeigt die `RotateDegrees` Methode. Die [ **BasicRotate.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Datei zeigt Text mit der zugehörigen Basislinie, die auf der Seite zentriert und wird auf der Grundlage einer `Slider` mit einem Bereich von –360 auf 360. Hier ist der relevante Teil der `PaintSurface` Handler:

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

Da die Drehung um die linke obere Ecke des Zeichenbereichs, für die meisten Winkel, die in diesem Programm festgelegt zentriert ist gedreht wird der Text außerhalb des Bildschirms:

[![](rotate-images/basicrotate-small.png "Dreifacher Screenshot der Seite drehen Sie grundlegende")](rotate-images/basicrotate-large.png#lightbox "dreifachen Screenshot der Seite für grundlegende drehen")

Sehr häufig sollten Sie ein zentraler Punkt angegebenen macht es irgendwann mit diesen Versionen von Drehen der [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) und [ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) Methoden:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

Die **zentriert Drehen** Seite entspricht der **grundlegende Drehen** mit dem Unterschied, dass die erweiterte Version des der `RotateDegrees` wird verwendet, um den Mittelpunkt der Drehung auf die gleiche Punktmenge verwendet, um den Text zu positionieren festgelegt:

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

Nun wird der Text gedreht, um den Punkt verwendet, um den Text, positionieren Sie, der horizontalen Mitte des Texts Baseline ist:

[![](rotate-images/centeredrotate-small.png "Dreifacher Screenshot der Seite zentriert Drehen")](rotate-images/centeredrotate-large.png#lightbox "dreifachen Screenshot der Seite zentriert drehen")

Wie bei der zentrierte Version von der `Scale` -Methode, die zentriert die `RotateDegrees` Aufruf ist eine Verknüpfung. Hier ist die Methode ein:

```csharp
RotateDegrees (degrees, px, py);
```

Dieser Aufruf ist äquivalent zu folgendem:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Erfahren Sie, dass Sie manchmal kombinieren können `Translate` Ruft mit `Rotate` aufrufen. Hier sind beispielsweise die `RotateDegrees` und `DrawText` Aufrufe in die **zentriert Drehen** Seite;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Die `RotateDegrees` Aufruf entspricht zwei `Translate` aufrufen und eine nicht-zentriert `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Die `DrawText` Aufruf zum Anzeigen von Text an einer bestimmten Stelle entspricht einer `Translate` für diesen Standort an, gefolgt von Aufrufen `DrawText` am Punkt (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Die beiden aufeinander folgenden `Translate` Aufrufe heben sich gegenseitig:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Vom Konzept her werden zwei Transformationen angewendet, in der Reihenfolge Gegensatz zu wie sie im Code angezeigt werden. Die `DrawText` Aufruf zeigt den Text in der oberen linken Ecke des Zeichenbereichs. Die `RotateDegrees` Aufruf wird dieser Text relativ zur oberen linken Ecke gedreht. Die `Translate` Aufruf verschiebt den Text in der Mitte der Canvas.

Es gibt in der Regel mehrere Möglichkeiten, Drehung und Übersetzung zu kombinieren. Die **Text gedreht** -Seite erstellt, die folgende Anzeige:

[![](rotate-images/rotatedtext-small.png "Dreifacher Screenshot der Seite Text gedreht")](rotate-images/rotatedtext-large.png#lightbox "dreifachen Screenshot der Seite Text gedreht")

Hier ist die `PaintSurface` Handler, der die [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) Klasse:

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

Die `xCenter` und `yCenter` Werte geben an der Mitte der Canvas. Die `yText` Wert ist ein bisschen von dem offset. Dieser Wert ist die Y-Koordinate, die erforderlich ist, um den Text zu positionieren, damit sie auf der Seite wirklich vertikal zentriert wird. Die `for` Schleife legt dann eine Rotation basierend auf der Mitte der Canvas. Die Rotation ist in Schritten von 30 Grad. Der Text gezeichnet wird, mit der `yText` Wert. Die Anzahl von Leerzeichen vor dem Wort "drehen" in der `text` Wert zum Herstellen die Verbindung zwischen diesen 12 Textzeichenfolgen anscheinend ein Dodecagon empirisch ermittelt wurde.

Eine Möglichkeit, diesen Code zu vereinfachen, ist den Winkel der Drehung um 30° jedes Mal nach der Schleife erhöht den `DrawText` aufrufen. Dadurch entfällt die Notwendigkeit für Aufrufe von `Save` und `Restore`. Beachten Sie, dass die `degrees` Variable wird nicht mehr innerhalb des Texts verwendet die `for` blockieren:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Es ist auch möglich, verwenden die einfache Form des `RotateDegrees` durch Voranstellen von der Schleife mit einem Aufruf von `Translate` , alles, was in der Mitte der Canvas zu verschieben:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Die geänderte `yText` Berechnung nicht mehr beinhaltet `yCenter`. Jetzt die `DrawText` Aufruf zentriert den Text vertikal am oberen Rand des Zeichenbereichs.

Da die Transformationen konzeptionell Gegensatz in Code Anzeige angewendet werden, ist es oft möglich zunächst mehr globale Transformationen, gefolgt von weitere lokalen Transformationen. Dies ist oft die einfachste Möglichkeit, Drehung und Übersetzung zu kombinieren.

Nehmen wir beispielsweise an, dass Sie ein grafisches Objekt, das dreht sich um seinen Mittelpunkt ähnlich wie ein Planet seine Achse drehen zeichnen möchten. Aber auch dieses Objekt, um die Mitte des Bildschirms ähnlich wie eine weltweit, drehen, um die Sonne drehen.

Dies ist möglich, positionieren das Objekt in der oberen linken Ecke des Zeichenbereichs, und klicken Sie dann mithilfe einer Animation es diese Ecke Drehung. Als Nächstes übersetzt das Objekt horizontal z. B. eine Orbitalschüttler. Wenden Sie nun eine zweite animierte Drehung auch um den Ursprung. Dadurch wird das Objekt, um die Ecke drehen. Übersetzen Sie jetzt in der Mitte des im Zeichenbereich.

Hier ist die `PaintSurface` Handler, der diese enthält transformieren Aufrufe in umgekehrter Reihenfolge:

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

Die `revolveDegrees` und `rotateDegrees` Felder animiert werden. Dieses Programm verwendet eine andere Animation-Technik, die basierend auf der Xamarin.Forms [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse. (Diese Klasse wird beschrieben, [Kapitel 22 von *Erstellen mobiler Apps mit Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) die `OnAppearing` Außerkraftsetzung erstellt zwei `Animation` Objekte mit Rückrufmethoden, und ruft dann `Commit` werden für die Dauer einer Animation:

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

Die erste `Animation` Objekt animiert wird `revolveDegrees` der von 0 bis 360 Grad mehr als 10 Sekunden. Das zweite Argument eine Animation `rotateDegrees` der von 0 bis 360 Grad alle 1 Sekunde und auch erklärt die Oberfläche zum Generieren von einem weiteren Aufruf von der `PaintSurface` Handler. Die `OnDisappearing` Außerkraftsetzung bricht diese zwei Animationen ab:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

Die **hässlichen Analoguhr** Programm (so genannt, weil eine attraktivere analoge Uhr in einem nachfolgenden Artikel beschrieben) verwendet Drehung aus, um die Markierungen Minute und Stunde der Uhr zu zeichnen und die Hände zu drehen. Die Anwendung zeichnet die Uhr mit einem beliebigen Koordinatensystem, die basierend auf ein Kreis, der an dem Punkt (0, 0) mit einem Radius von 100 zentriert ist. Übersetzung und Skalierung verwendet zu erweitern, und zentrieren des Kreises auf der Seite.

Die `Translate` und `Scale` Aufrufe gelten global für die Uhr, also zunächst auf kommunaler aufgerufen werden, nach der Initialisierung der der `SKPaint` Objekte:

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

Es gibt 60 Markierungen von zwei verschiedenen Größen, die rund um die Uhr in einem Kreis gezeichnet werden müssen. Die `DrawCircle` Aufruf zeichnet, Kreis, an dem Punkt (0, –90), das Bezug auf den Mittelpunkt der Uhr, 12:00 Uhr entspricht. Die `RotateDegrees` Aufruf erhöht den Drehwinkel in Schritten von 6 Grad nach jeder Teilstrich. Die `angle` Variable wird verwendet, nur um zu bestimmen, ob ein großer Kreis oder ein kleiner Kreis gezeichnet wird:

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

Zum Schluss die `PaintSurface` Handler Ruft die aktuelle Uhrzeit ab und berechnet Sie für die Stunde, Minute und Sekunde Hände Grad der Drehung. Jeder Runde wird in die 12:00 Uhr-Position gezeichnet, damit der Drehwinkel für Bezeichnungen, ist:

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

Die Uhr ist sicherlich funktionsfähig, obwohl die Hände recht einfach gehalten sind:

[![](rotate-images/uglyanalogclock-small.png "Screenshot der Seite hässlichen analogen Uhr Text dreifach")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

Eine noch attraktiver Uhr, finden Sie im Artikel [ **SVG-Pfaddaten in SkiaSharp**](../curves/path-data.md).

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
