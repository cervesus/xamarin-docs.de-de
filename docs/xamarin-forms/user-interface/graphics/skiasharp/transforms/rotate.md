---
title: Die Rotationstransformation
description: Untersuchen Sie die Auswirkung und Animationen, die mit der SkiaSharp Rotationstransformation möglich
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 177437ef016a25849e7c34d0a26270ce14173b7d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="the-rotate-transform"></a>Die Rotationstransformation

_Untersuchen Sie die Auswirkung und Animationen, die mit der SkiaSharp Rotationstransformation möglich_

Mit der Rotationstransformation unterbrechen SkiaSharp Grafikobjekten freien der Einschränkung der Ausrichtung mit der horizontalen und vertikalen Achsen:

![](rotate-images/rotateexample.png "Um eine zentrale gedrehter Text")

Für das Drehen von einem Objekt um einen Punkt (0, 0), SkiaSharp unterstützt sowohl eine [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) Methode und eine [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) Methode:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Ein Kreis von 360 Grad entspricht; 2π Radianten, daher ist es einfach zum Konvertieren zwischen zwei Einheiten. Verwenden Sie, je nachdem, was praktisch ist. Alle trigonometrischen Funktionen in der statischen [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) Klasse verwenden Einheiten im Bogenmaß.

Rotation ist für die Erhöhung der Winkel im Uhrzeigersinn. (Obwohl Drehung auf die kartesischen Koordinatensystem gegen den Uhrzeigersinn gemäß der Konvention ist, ist Drehung im Uhrzeigersinn um konsistent mit Y-Koordinaten, die laufende nach unten zu erhöhen.) Negative Winkel und Winkel größer als 360 Grad zulässig sind.

Die Transformation Formeln für die Drehung sind komplexer als die Skalierung zu übersetzen. Ein Winkel von α sind die Transformation Formeln:

x' = x•cos(α) – y•sin(α)   

y "= x•sin(α) + y•cos(α)

Die **grundlegende Drehen** veranschaulicht die `RotateDegrees` Methode. Die [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Datei zeigt Text mit der zugehörigen Basislinie auf der Seite zentriert und dreht auf der Grundlage einer `Slider` mit einem Bereich von –360 auf 360. Hier wird der relevante Teil der `PaintSurface` Ereignishandler:

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

Der Text wird außerhalb des Bildschirms gedreht, da Drehung im erübrigt die linke obere Ecke des Zeichenbereichs, für die meisten Winkel an diesem Programm festgelegt wird:

[![](rotate-images/basicrotate-small.png "Dreifacher Screenshot der Seite grundlegende Drehen")](rotate-images/basicrotate-large.png#lightbox "dreifacher Screenshot der Seite grundlegende drehen")

Sehr häufig sollten Sie ein Element einer angegebenen Dreh-und Angelpunkt mit diesen Versionen von zentraler Drehen der [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) und [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) Methoden:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

Die **zentriert Drehen** Seite ist ebenso wie die **grundlegende Drehen** mit dem Unterschied, dass die erweiterte Version des der `RotateDegrees` dient zum Festlegen des Mittelpunkts der Drehung mit demselben Zeitpunkt verwendet, um den Text einzufügen:

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

Jetzt wird der Text gedreht, um den Punkt verwendet, um den Text einzufügen, der die horizontale Mitte der Basislinien-der Text ist:

[![](rotate-images/centeredrotate-small.png "Dreifacher Screenshot der Seite zentriert Drehen")](rotate-images/centeredrotate-large.png#lightbox "dreifacher Screenshot der Seite zentriert drehen")

Wie bei der zentriert Version von den `Scale` -Methode, die zentriert Version von den `RotateDegrees` Aufruf ist eine Verknüpfung:

```csharp
RotateDegrees (degrees, px, py);
```

Dieser Ausdruck ist äquivalent zu Folgendem:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Erfahren Sie, dass Sie manchmal kombinieren können `Translate` Aufrufe mit `Rotate` aufrufen. Hier sind z. B. die `RotateDegrees` und `DrawText` ruft in der **zentriert Drehen** Seite;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Die `RotateDegrees` Aufruf ist gleichbedeutend mit zwei `Translate` aufrufen und eine nicht zentriert `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Die `DrawText` Aufruf zum Anzeigen von Text an einer bestimmten Stelle entspricht einem `Translate` für diesen Speicherort, gefolgt von Aufrufen `DrawText` an dem Punkt (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Die beiden aufeinander folgenden `Translate` Aufrufe gegenseitig aufheben:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Im Prinzip werden die beiden Transformationen angewendet, in der Reihenfolge statt wie im Code angezeigt werden. Die `DrawText` Aufruf wird der Text in der oberen linken Ecke des Zeichenbereichs. Die `RotateDegrees` Aufruf dreht Text relativ zur linken oberen Ecke. Die `Translate` Aufruf verschiebt den Text in der Mitte des Zeichenbereichs.

Es gibt in der Regel mehrere Möglichkeiten, Drehung und Übersetzung zu kombinieren. Die **Text gedreht** -Seite erstellt der folgende angezeigt:

[![](rotate-images/rotatedtext-small.png "Dreifacher Screenshot der Seite Text gedreht")](rotate-images/rotatedtext-large.png#lightbox "dreifacher Screenshot der Seite Text gedreht")

So sieht die `PaintSurface` Handler, der die [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) Klasse:

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

Die `xCenter` und `yCenter` Werte geben die Mitte des Zeichenbereichs. Die `yText` Wert ist ein wenig von dem offset. Hiermit wird die Y-Koordinate, die auf den Text zu positionieren, sodass er tatsächlich vertikal auf der Seite zentriert ist erforderlich. Die `for` Schleife legt dann eine Rotation der Schwerpunkt auf die Mitte des Zeichenbereichs. Die Rotation ist in Schritten von 30 Grad. Der Text gezeichnet wird, mithilfe der `yText` Wert. Die Anzahl von Leerzeichen vor dem Wort "drehen" in der `text` Wert wurde zum Herstellen die Verbindung zwischen diesen 12 Textzeichenfolgen zu einer Dodecagon zu sein scheinen empirisch bestimmt.

Eine Möglichkeit, diesen Code vereinfachen ist den Winkel der Drehung um 30 Grad jedes Mal nach der Schleife erhöht den `DrawText` aufrufen. Hierdurch entfällt die Notwendigkeit für Aufrufe von `Save` und `Restore`. Beachten Sie, dass die `degrees` Variable wird nicht mehr im Coderumpf des verwendet die `for` blockieren:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Es ist auch möglich, verwenden Sie die einfache Form einer `RotateDegrees` durch die Schleife mit einem Aufruf von voran `Translate` um alles in der Mitte des Zeichenbereichs verschieben:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Das geänderte `yText` Berechnung enthält, nicht mehr `yCenter`. Jetzt die `DrawText` Aufruf zentriert den Text vertikal am oberen Rand des Zeichenbereichs.

Da die Transformationen konzeptionell angewendet werden, statt wie im Code angezeigt werden, ist es oft möglich zunächst eher global und Transformationen, gefolgt von weitere lokalen Transformationen. Dies ist häufig die einfachste Möglichkeit, Drehung und Übersetzung zu kombinieren.

Nehmen Sie z. B. an, dass Sie ein Grafikobjekt, die dreht zeichnen, um ihren Mittelpunkt ähnlich wie ein Planet auf der Achse drehen möchten. Aber auch dieses Objekts, um die Mitte des Bildschirms ähnlich wie ein Planet drehen, um die Sonne drehen.

Dies ist möglich, indem positionieren das Objekt in der oberen linken Ecke des Zeichenbereichs, und klicken Sie dann eine Animation es dieser Ecke Rotationsachse. Als Nächstes übersetzt das Objekt wie einen Orbitalschüttler Radius horizontal. Jetzt Anwenden einer zweiten animierten Rotation, auch um den Ursprung aus. Dadurch wird das Objekt, um die Ecke drehen. Jetzt in der Mitte des Zeichenbereichs übersetzt.

So sieht die `PaintSurface` Handler, der diese enthält Aufrufe in umgekehrter Reihenfolge Transformieren:

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

Die `revolveDegrees` und `rotateDegrees` Felder animiert werden. Das Programm erstellt mithilfe eine anderen Animationstechnik, die basierend auf den Xamarin.Forms `Animation` Klasse. (Diese Klasse ist in der beschriebenen [Kapitel 22 *Erstellen mobiler Apps mit Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) der `OnAppearing` Außerkraftsetzung erstellt zwei `Animation` Objekte mit Rückrufmethoden, und ruft dann `Commit` unter diesen Betriebssystemen für die Dauer einer Animation:

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

Die erste `Animation` Objekt animiert `revolveDegrees` von 0 bis 360 Grad mehr als 10 Sekunden. Das zweite Argument eine Animation `rotateDegrees` von 0 bis 360 Grad alle 1 Sekunde und auch erklärt die Oberfläche zum Generieren von einem weiteren Aufruf von der `PaintSurface` Handler. Die `OnDisappearing` Außerkraftsetzung bricht diese zwei Animationen ab:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

Die **problematischen Analoguhr** Programm (so genannt, weil es sich bei eine analoge Uhr gleichgesetzt attraktivere in einem Artikel weiter unten beschrieben werden) verwendet Drehung aus, um die Minute und Stunde Markierungen der Uhr zu zeichnen und die Hände drehen. Das Programm zeichnet die Uhr mithilfe einer beliebigen Koordinatensystems basierend auf einem Kreis, der an dem Punkt (0, 0) mit einem Radius von 100 zentriert ist. Übersetzung und Skalierung verwendet zum Erweitern und zentrieren Kreises auf der Seite.

Die `Translate` und `Scale` Aufrufe gelten global für die Uhr, daher sind diejenigen aus, die aufgerufen werden, nach der Initialisierung des ersten der `SKPaint` Objekte:

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

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

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

Schließlich die `PaintSurface` Handler Ruft die aktuelle Uhrzeit und Drehung Grad für die Stunde, Minute und zweite Hände berechnet. So, dass der Drehwinkel für Bezeichnungen relativ zum, die jeder Runde 12:00 Uhr-Position gezeichnet:

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

Die Uhr ist sicherlich funktionsfähig, obwohl die Hände stattdessen einfach gehalten sind:

[![](rotate-images/uglyanalogclock-small.png "Screenshot der Seite mit den problematischen analogen Uhr Text dreifach")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
