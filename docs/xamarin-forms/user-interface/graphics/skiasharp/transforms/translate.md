---
title: ''
description: In diesem Artikel wird beschrieben, wie Sie die Transformation "übersetzen" verwenden, um skiasharp-Grafiken in Anwendungen zu verschieben Xamarin.Forms , und dies mit Beispielcode veranschaulicht.
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0eb3b4a6b37d59363984c9248cc39de91a6819e0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138254"
---
# <a name="the-translate-transform"></a>Die Verschiebungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie mit der Transform-Transformation skiasharp-Grafiken verschieben._

Der einfachste Typ der Transformation in skiasharp ist die Translation *-oder* *Translation* -Transformation. Diese Transformation verschiebt grafische Objekte in horizontaler und vertikaler Richtung. In gewisser Hinsicht ist die Übersetzung die unnötigste Transformation, da Sie in der Regel die gleichen Effekte erzielen können, indem Sie einfach die Koordinaten ändern, die Sie in der Zeichnungs Funktion verwenden. Wenn jedoch ein Pfad gerendert wird, werden alle Koordinaten im Pfad gekapselt, sodass es sehr viel einfacher ist, eine Translation-Transformation anzuwenden, um den gesamten Pfad zu verschieben.

Die Übersetzung ist auch für Animationen und einfache Texteffekte nützlich:

![](translate-images/translateexample.png "Text shadow, engraving, and embossing with translation")

Die [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) -Methode in `SKCanvas` verfügt über zwei Parameter, die bewirken, dass nachträglich gezeichnete Grafik Objekte horizontal und vertikal verschoben werden:

```csharp
public void Translate (Single dx, Single dy)
```

Diese Argumente können negativ sein. Eine zweite [`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) Methode kombiniert die beiden Übersetzungs Werte in einem einzelnen `SKPoint` Wert:

```csharp
public void Translate (SKPoint point)
```

Die **Kumulierte Übersetzungs** Seite des [**skiasharpforms**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel Programms veranschaulicht, dass mehrere Aufrufe der `Translate` Methode kumulativ sind. Die- [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Klasse zeigt 20 Versionen desselben Rechtecks an, wobei jeder Offset vom vorherigen Rechteck genau genug ist, sodass Sie sich entlang der Diagonal Strecken. Dies ist der `PaintSurface` Ereignishandler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Die aufeinander folgenden Rechtecke werden auf der Seite angezeigt:

[![](translate-images/accumulatedtranslate-small.png "Triple screenshot of the Accumulated Translate page")](translate-images/accumulatedtranslate-large.png#lightbox "Triple screenshot of the Accumulated Translate page")

Wenn die akkumulierten Übersetzungs Faktoren `dx` und lauten `dy` und der von Ihnen in einer Zeichnungs Funktion angegebene Punkt ( `x` ,) ist `y` , wird das grafische Objekt an der Stelle ( `x'` ,) gerendert `y'` , wobei:

x ' = x + DX

y ' = y + dy

Diese werden als *transformationsformeln* für die Übersetzung bezeichnet. Die Standardwerte von `dx` und `dy` für einen neuen `SKCanvas` sind 0.

Häufig wird die Translation-Transformation für Schatteneffekte und ähnliche Techniken verwendet, wie die Seite " **Text Effekte übersetzen** " veranschaulicht. Hier ist der relevante Teil des `PaintSurface` Handlers in der- [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) Klasse:

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

In jedem der drei Beispiele `Translate` wird aufgerufen, um den Text anzuzeigen, um ihn von der Position zu versetzen, die von den `x` Variablen und angegeben wird `y` . Der Text wird in einer anderen Farbe ohne Übersetzungseffekt angezeigt:

[![](translate-images/translatetexteffects-small.png "Triple screenshot of the Translate Text Effects page")](translate-images/translatetexteffects-large.png#lightbox "Triple screenshot of the Translate Text Effects page")

Jedes der drei Beispiele zeigt eine andere Methode zum neinieren des `Translate` Aufrufens:

Im ersten Beispiel wird einfach erneut aufgerufen, aber es werden `Translate` negative Werte angezeigt. Da die `Translate` Aufrufe kumulativ sind, stellt diese Sequenz von Aufrufen einfach die gesamte Übersetzung in die Standardwerte 0 (null) wieder her.

Im zweiten Beispiel wird aufgerufen [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) . Dies bewirkt, dass alle Transformationen auf ihren Standardzustand zurückgesetzt werden.

Im dritten Beispiel wird der Zustand des `SKCanvas` -Objekts mit einem-Rückruf gespeichert, [`Save`](xref:SkiaSharp.SKCanvas.Save) und anschließend wird der Zustand mit einem-Befehl wieder hergestellt [`Restore`](xref:SkiaSharp.SKCanvas.Restore) . Dies ist die vielseitigste Methode zum Bearbeiten von Transformationen für eine Reihe von Zeichnungs Vorgängen. Diese `Save` und `Restore` Aufrufe funktionieren wie ein Stapel: Sie können `Save` mehrmals aufrufen und dann `Restore` in umgekehrter Reihenfolge aufrufen, um zu den vorherigen Zuständen zurückzukehren. Die `Save` -Methode gibt eine ganze Zahl zurück, und Sie können diese [`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*) Ganzzahl an übergeben, um effektiv mehrmals aufzurufen `Restore` . Die- [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount) Eigenschaft gibt die Anzahl der Zustände zurück, die derzeit auf dem Stapel gespeichert werden.

Sie können auch die- [`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore) Klasse verwenden, um den Canvas-Zustand wiederherzustellen. Der Konstruktor dieser Klasse soll in einer-Anweisung aufgerufen werden `using` . der Canvas-Status wird am Ende des-Blocks automatisch wieder hergestellt `using` .

Allerdings müssen Sie sich keine Gedanken über Transformationen machen, die von einem der Aufrufe des `PaintSurface` Handlers zum nächsten übertragen werden. Jeder neue-Befehl von stellt ein neues- `PaintSurface` `SKCanvas` Objekt mit Standard Transformationen bereit.

Eine weitere häufige Verwendung der `Translate` Transformation ist das Rendern eines visuellen Objekts, das ursprünglich mit Koordinaten erstellt wurde, die für das Zeichnen geeignet sind. Beispielsweise können Sie Koordinaten für eine analoge Uhr mit einem Mittelpunkt am Punkt (0,0) angeben. Sie können dann Transformationen verwenden, um die Uhr anzuzeigen, an der Sie es wünschen. Diese Technik wird auf der Seite [**qudecagram Array**] veranschaulicht. Die [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs) Klasse beginnt mit dem Erstellen eines- `SKPath` Objekts für einen 11-pointierten Stern. Das `HendecagramPath` Objekt ist als Public, static und Read-Only definiert, damit von anderen Demonstrations Programmen darauf zugegriffen werden kann. Sie wird in einem statischen Konstruktor erstellt:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Wenn die Mitte des Sterns der Punkt (0,0) ist, befinden sich alle Punkte des Sterns in einem Kreis, der diesen Punkt umgibt. Jeder Punkt ist eine Kombination aus Sinus-und Kosinus-Werten eines Winkels, der sich um 5/11sekunden um 360 Grad vergrößert. (Es ist auch möglich, einen 11-pointierten Stern zu erstellen, indem der Winkel um 2/11., 3/11sekunden oder 4/11. des Kreises vergrößert wird.) Der RADIUS dieses Kreises wird auf 100 festgelegt.

Wenn dieser Pfad ohne Transformationen gerendert wird, wird das Zentrum in der oberen linken Ecke von positioniert `SKCanvas` , und nur ein Quartal davon ist sichtbar. Der `PaintSurface` -Handler von `HendecagramPage` verwendet stattdessen, `Translate` um den Zeichenbereich mit mehreren Kopien des Sterns zu Kacheln, die jeweils nach dem Zufallsprinzip gefärbt werden:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

Das Ergebnis lautet wie folgt:

[![](translate-images/hendecagramarray-small.png "Triple screenshot of the Hendecagram Array page")](translate-images/hendecagramarray-large.png#lightbox "Triple screenshot of the Hendecagram Array page")

Animationen umfassen häufig Transformationen. Auf der Seite " **qudecagram Animation** " wird der 11-Spitze-Stern in einem Kreis bewegt. Die [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) -Klasse beginnt mit einigen Feldern und über schreibungen der `OnAppearing` -Methode und der- `OnDisappearing` Methode, um einen Timer zu starten und anzuhalten Xamarin.Forms :

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

Das `angle` Feld wird alle 5 Sekunden zwischen 0 Grad und 360 Grad animiert. Der `PaintSurface` Handler verwendet die `angle` -Eigenschaft auf zweierlei Weise: um den Farbton der Farbe in der `SKColor.FromHsl` Methode anzugeben, und als Argument für die-Methode und die-Methode, `Math.Sin` `Math.Cos` um die Position des Sterns zu bestimmen:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

Der `PaintSurface` Handler Ruft die `Translate` Methode zweimal auf, um zuerst in den Mittelpunkt des Zeichen Bereichs zu übersetzen und dann in den Umfang eines Kreises zu konvertieren (0,0). Der Radius des Kreises wird so groß wie möglich festgelegt, während er den Stern weiterhin innerhalb der Begrenzungen der Seite hält:

[![](translate-images/hendecagramanimation-small.png "Triple screenshot of the Hendecagram Animation page")](translate-images/hendecagramanimation-large.png#lightbox "Triple screenshot of the Hendecagram Animation page")

Beachten Sie, dass der Stern die gleiche Ausrichtung beibehält, wie er sich um den Mittelpunkt der Seite dreht. Es dreht sich überhaupt nicht. Das ist ein Auftrag für eine Transformation zum drehen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
