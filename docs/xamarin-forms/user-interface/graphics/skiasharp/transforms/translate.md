---
title: Die Verschiebungstransformation
description: In diesem Artikel wird untersucht, wie Sie mit, dass die Verschiebungstransformation um Grafiken von SkiaSharp in Xamarin.Forms-Anwendungen zu verschieben, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: a51a441aeacf265093b82ddb65237887b0a30719
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382455"
---
# <a name="the-translate-transform"></a>Die Verschiebungstransformation

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Erfahren Sie, wie die Verschiebungstransformation zu verwenden, um das Verschieben von SkiaSharp-Grafiken_

Die einfachste Art der Transformation im SkiaSharp ist die *übersetzen* oder *Übersetzung* transformieren. Mit dieser Transformation wird die grafische Objekte in der horizontalen und vertikalen Richtungen verlagert. In gewisser Hinsicht ist Übersetzung die am häufigsten unnötige Transformation, da Sie in der Regel denselben Effekt erreichen können, durch einfaches Ändern der Koordinaten, die Sie in der Zeichnen-Funktion verwenden. Beim Rendern von eines Pfads werden jedoch alle Koordinaten im Pfad, gekapselt daher ist es viel einfacher, Anwenden von TranslateTransform, um den vollständigen Pfad zu verschieben.

Übersetzung ist auch nützlich, Animation und Effekte für einfachen Text:

![](translate-images/translateexample.png "Textschatten, Eingravieren und bei der Übersetzung Prägen")

Die [ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) -Methode in der `SKCanvas` verfügt über zwei Parameter, die dazu führen, dass nachfolgend gezeichneten Grafikobjekten, die horizontal und vertikal verschoben werden sollen:

```csharp
public void Translate (Single dx, Single dy)
```

Diese Argumente können negativ sein. Ein zweites [ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) Methode kombiniert die beiden Übersetzungswerte in einer einzelnen `SKPoint` Wert:

```csharp
public void Translate (SKPoint point)
```

Der **übersetzen gesammelt** auf der Seite der [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispielprogramm zeigt, dass mehrere Aufrufe der, wie die `Translate` Methode sind kumulativ. Die [ `AccumulatedTranslatePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Klasse zeigt 20 Versionen des gleichen Rechtecks, jede Abweichung von der des vorherigen Rechtecks ausreichend, damit sie entlang der diagonalen gestreckt. Hier ist die `PaintSurface` -Ereignishandler:

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

Die nachfolgenden Rechtecke, die unten auf der Seite angewendet wurden:

[![](translate-images/accumulatedtranslate-small.png "Dreifacher Screenshot der Seite übersetzt kumuliert")](translate-images/accumulatedtranslate-large.png#lightbox "dreifachen Screenshot der Seite übersetzt gesammelt")

Wenn die akkumulierten Übersetzung Faktoren sind `dx` und `dy`, und der Angabe in einer zeichnen-Funktion (`x`, `y`), und klicken Sie dann die grafischen Objekts, an dem Punkt gerendert wird (`x'`, `y'`), wobei:

X' = X + Dx

y' = y + dy

Diese werden als bezeichnet die *transformieren Formeln* für die Übersetzung. Die Standardwerte der `dx` und `dy` für einen neuen `SKCanvas` 0 sind.

Es ist üblich, verwenden die Verschiebungstransformation für Schatteneffekte und ähnliche Verfahren, als die **Texteffekte übersetzen** Seite erläutert. Hier ist der relevante Teil der `PaintSurface` Ereignishandler in der [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) Klasse:

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

In jedem der drei Beispiele `Translate` wird aufgerufen, für die Anzeige von den Text, um es von den vom angegebenen Speicherort versetzt die `x` und `y` Variablen. Dann wird der Text in einer anderen Farbe ohne Übersetzung Auswirkungen erneut angezeigt:

[![](translate-images/translatetexteffects-small.png "Dreifacher Screenshot der Seite für Texteffekte übersetzen")](translate-images/translatetexteffects-large.png#lightbox "dreifachen Screenshot der Seite für Texteffekte übersetzen")

Jedes der drei Beispiele zeigt, dass eine andere Art der negieren die `Translate` aufrufen:

Ruft das erste Beispiel einfach `Translate` wieder, aber mit negativen Werten. Da die `Translate` Aufrufe sind kumulativ, das dieser Sequenz von Aufrufen wird die Gesamtkosten für die Übersetzung einfach auf die Standardwerte 0 (null).

Im zweiten Beispiel wird [ `ResetMatrix` ](xref:SkiaSharp.SKCanvas.ResetMatrix). Dies bewirkt, dass alle Transformationen, die für die zurückzugebenden auf ihren Standardzustand zurückgesetzt.

Das dritte Beispiel speichert den Zustand der `SKCanvas` Objekt mit einem Aufruf von [ `Save` ](xref:SkiaSharp.SKCanvas.Save) und dann wird der Status mit einem Aufruf von [ `Restore` ](xref:SkiaSharp.SKCanvas.Restore). Dies ist der vielseitigste Möglichkeit zum Bearbeiten von Transformationen für eine Reihe von Operationen zu zeichnen. Diese `Save` und `Restore` Ruft die Funktion wie ein Stapel: Rufen Sie `Save` mehrere Male aus, und rufen Sie dann `Restore` in umgekehrte Sequenz, die in vorherigen Zustand zurück. Die `Save` Methode gibt eine ganze Zahl zurück, und Sie können diese ganze Zahl, und übergeben [ `RestoreToCount` ](xref:SkiaSharp.SKCanvas.RestoreToCount*) effektiv Aufrufen `Restore` mehrmals. Die [ `SaveCount` ](xref:SkiaSharp.SKCanvas.SaveCount) Eigenschaft gibt die Anzahl der Zustände, die derzeit auf dem Stapel gespeichert.

Sie können auch die [ `SKAutoCanvasRestore` ](xref:SkiaSharp.SKAutoCanvasRestore) -Klasse für den Wiederherstellungsstatus der Canvas. Richtet sich an der Konstruktor dieser Klasse aufgerufen werden, eine `using` -Anweisung, die Canvas Zustand wird automatisch wiederhergestellt, am Ende der `using` Block. 

Jedoch keinen kümmern, Transformationen, die von einem Aufruf von übertragen die `PaintSurface` Handler zur nächsten. Jeder neue Aufruf `PaintSurface` bietet eine neue `SKCanvas` Objekt mit der standardmäßigen Transformationen.

Häufig verwendet die `Translate` Transformation ist für das Rendern eines visuellen Objekts, das ursprünglich erstellt wurde mithilfe von Koordinaten, die zum Zeichnen sind. Beispielsweise empfiehlt es sich Koordinaten für einer analogen Uhr mit einem Rechenzentrum, an dem Punkt (0, 0) an. Sie können dann Transformationen verwenden, um die Uhr anzuzeigen an die gewünschte. Dieses Verfahren wird veranschaulicht, der [**Hendecagram Array**] Seite. Die [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Klasse beginnt mit der Erstellung einer `SKPath` -Objekt für einen Stern 11 zwischengespeichert. Die `HendecagramPath` Objekt ist als öffentliche, statische und schreibgeschützte definiert, sodass es von anderen Programmen Demo zugegriffen werden kann. Es wird in einem statischen Konstruktor erstellt:

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

Wenn die Mitte des Sterns den Punkt (0, 0) ist, werden alle Punkte des Sterns auf einem Kreis, um diesen Punkt. Jeder Punkt ist eine Kombination von Werten von Sinus und Kosinus eines Winkels, der um 5/11ths von 360 Grad erhöht. (Es ist auch möglich, einen Stern 11 Spitzen zu erstellen, durch das Erhöhen der Neigungswinkel 2/11., 3/11ths oder 4/11. des Kreises.) Der Radius des Kreises wird als 100 festgelegt.

Wenn dieser Pfad ohne Transformationen gerendert wird, wird das Center auf der linken oberen Ecke des positioniert die `SKCanvas`, und nur ein Viertel des Zertifikats ist sichtbar. Die `PaintSurface` Handler `HendecagramPage` verwendet stattdessen `Translate` um die Kachel im Zeichenbereichs aus, wenn mehrere Kopien des Sterns jeweils nach dem Zufallsprinzip zugewiesen:

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

Hier ist das Ergebnis:

[![](translate-images/hendecagramarray-small.png "Dreifacher Screenshot der Seite Hendecagram Array")](translate-images/hendecagramarray-large.png#lightbox "dreifachen Screenshot der Seite Hendecagram-Array")

Animationen umfassen häufig Transformationen. Die **Hendecagram Animation** Seite verschiebt den Stern 11 Spitzen in einem Kreis. Die [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Klasse beginnt mit einigen Feldern und der überschreibt die `OnAppearing` und `OnDisappearing` Methoden zum Starten und Beenden einen Timer Xamarin.Forms:

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

Die `angle` Feld animiert von 0 Grad und 360 Grad alle 5 Sekunden. Die `PaintSurface` Ereignishandler verwendet die `angle` Eigenschaft gibt es zwei Möglichkeiten: an den Farbton der Farbe im der `SKColor.FromHsl` -Methode, und als Argument an die `Math.Sin` und `Math.Cos` Methoden, um den Speicherort des Sterns steuern:

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

Die `PaintSurface` Ereignishandler ruft die `Translate` Methode zweimal, um in der Mitte der Canvas, übersetzt und dann in den Umfang eines Kreises bei zentraler Punkt übersetzt (0, 0). Der Radius des Kreises ist so groß wie möglich sein und dennoch die Sicherheit des Sterns innerhalb der Grenzen der Seite festgelegt:

[![](translate-images/hendecagramanimation-small.png "Dreifacher Screenshot der Seite Hendecagram Animation")](translate-images/hendecagramanimation-large.png#lightbox "dreifachen Screenshot der Seite Hendecagram-Animation")

Beachten Sie, dass das Sternsymbol dieselbe Ausrichtung verwaltet, wie sie in der Mitte der Seite basiert. Es werden nicht alle drehen. Dies ist ein Auftrag für eine Rotationstransformation.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
