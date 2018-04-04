---
title: Der Translate-Transformation
description: Erfahren Sie, wie die übersetzen-Transformation verwenden, um SkiaSharp Grafiken zu verschieben
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 573848186a8f389ac18e22ea4c3b7d4fe1503449
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="the-translate-transform"></a>Der Translate-Transformation

_Erfahren Sie, wie die übersetzen-Transformation verwenden, um SkiaSharp Grafiken zu verschieben_

Die einfachste Art von Transformation im SkiaSharp ist die *übersetzen* oder *Übersetzung* transformieren. Diese Transformation verlagert Grafikobjekte in horizontaler und vertikaler Richtung. In gewisser Weise ist Übersetzung die am häufigsten unnötige Transformation, da Sie in der Regel denselben Effekt erreichen können, indem Sie einfach ändern die Koordinaten, die Sie in der Zeichnen-Funktion verwenden. Beim Rendern von eines Pfads werden jedoch alle Koordinaten im Pfad, gekapselt daher ist es viel einfacher Anwenden einer übersetzen-Transformation, um den vollständigen Pfad zu verschieben.

Übersetzung ist auch nützlich für Animation und einfache Texteffekte:

![](translate-images/translateexample.png "Textschatten, Eingravieren und Übersetzung Reliefs")

Die [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Methode in `SKCanvas` hat zwei Parameter, die dazu führen, dass nachfolgend gezeichneten Grafikobjekten horizontal und vertikal verschoben werden sollen:

```csharp
public void Translate (Single dx, Single dy)
```

Diese Argumente können negativ sein. Ein zweites [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) -Methode kombiniert die beiden Übersetzungswerte in einer einzelnen `SKPoint` Wert:

```csharp
public void Translate (SKPoint point)
```

Die **übersetzen kumuliert** auf der Seite der [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispielprogramm zeigt, dass mehrere Aufrufe der der `Translate` Methode sind kumulativ. Die [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Klasse zeigt 20 Versionen des gleichen Rechtecks, jeweils offset aus dem vorherigen Rechteck gerade genug, damit sie entlang der diagonalen zu Strecken. So sieht die `PaintSurface` Ereignishandler:

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

Durch Einfügung aufeinander folgenden Rechtecke nach unten:

[![](translate-images/accumulatedtranslate-small.png "Dreifacher Screenshot der Seite kumuliert übersetzen")](translate-images/accumulatedtranslate-large.png#lightbox "dreifacher Screenshot der Seite kumuliert übersetzen")

Wenn die akkumulierten Übersetzung Faktoren sind `dx` und `dy`, und der Angabe in einer Zeichnung-Funktion (`x`, `y`), und klicken Sie dann das Objekt, an dem Punkt gerendert wird (`x'`, `y'`), wobei:

x' = x + dx

y "= y + dy

Diese werden als bezeichnet den *transformieren Formeln* für die Übersetzung. Die Standardwerte der `dx` und `dy` für einen neuen `SKCanvas` 0 sind.

Es ist üblich, verwenden die übersetzen-Transformation für Schatteneffekten und ähnliche Verfahren als der **übersetzen Texteffekte** veranschaulicht. Hier wird der relevante Teil der `PaintSurface` Ereignishandler in der [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) Klasse:

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

In jedem der drei Beispiele `Translate` wird aufgerufen, für die Anzeige von den Text, um ihn von der vom angegebenen Speicherort versetzt die `x` und `y` Variablen. Dann wird der Text in eine andere Farbe hat keine Auswirkungen Übersetzung erneut angezeigt:

[![](translate-images/translatetexteffects-small.png "Dreifacher Screenshot der Seite Texteffekte übersetzen")](translate-images/translatetexteffects-large.png#lightbox "dreifacher Screenshot der Seite Texteffekte übersetzen")

Jedes der drei Beispiele zeigt eine andere Art der negieren der `Translate` aufrufen:

Ruft das erste Beispiel einfach `Translate` ebenfalls jedoch mit negativen Werten. Da die `Translate` Aufrufe sind kumulativ, diese Abfolge an Aufrufen stellt die Gesamtkosten für die Übersetzung einfach auf die Standardwerte von 0 (null).

Im zweiten Beispiel wird [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). Dies bewirkt, dass alle Transformationen auf ihren Standardstatus zurück.

Im dritte Beispiel speichert den Zustand der von der `SKCanvas` Objekt mit einem Aufruf von [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) und wiederhergestellt werden den Status mit einem Aufruf von [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Dies ist sehr vielseitig wie Umwandlungen für eine Reihe von Zeichenvorgänge bearbeiten. Diese `Save` und `Restore` Funktionsaufrufe wie ein Stapel: Sie erreichen `Save` mehrere Zeit, und rufen Sie dann `Restore` reverse Sequenz, die in vorherigen Zustand zurück. Die `Save` Methode gibt eine ganze Zahl zurück, und Sie können diese ganze Zahl, und übergeben [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) effektiv Aufrufen `Restore` mehrere Male. Die [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Eigenschaft gibt die Anzahl der Status, die derzeit auf dem Stapel gespeichert.

Sie haben jedoch keinen Gedanken Transformationen über zwischen zwei Aufrufen der Durchführung der `PaintSurface` Handler zur nächsten. Jeder neue Aufruf `PaintSurface` bietet einen aktuellen `SKCanvas` Objekt mit Standard-Transformationen.

Eine andere häufige Verwendung von der `Translate` "Transformieren" ist für einem visuellen Objekt zu rendern, die ursprünglich erstellt wurde mithilfe von Koordinaten, die zum Zeichnen praktische sind. Sie möchten z. B. Koordinaten für einer analogen Uhr mit einem Rechenzentrum, an dem Punkt (0, 0) angeben. Sie können dann Transformationen verwenden, um diese anzuzeigen möchten. Dies wird dargestellt, der [**Hendecagram Array**] Seite. Die [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Klasse beginnt mit der Erstellung einer `SKPath` Objekt für einen Stern 11 gezeigt wird. Die `HendecagramPath` Objekt ist definiert als öffentliche, statische und schreibgeschützt, damit er von anderen Programmen Demo zugegriffen werden kann. Es wird in einem statischen Konstruktor erstellt:

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

Wenn die Mitte des Sterns den Punkt (0, 0) ist, werden alle Punkte des Sterns auf einem Kreis, der diesen Punkt umgibt. Jeder Punkt ist eine Kombination von Werten von Sinus- und Kosinuswert eines Winkels, der um 5/11ths von 360 Grad erhöht. (Es ist auch möglich, einen Stern 11 Spitzen zu erstellen, durch Erhöhen des Winkels von 2/11., 3/11ths oder 4/11. des Kreises.) Der Radius des Kreises wird als 100 festgelegt.

Wenn dieser Pfad ohne Transformationen gerendert wird, werden die Center in der linken oberen Ecke der positioniert das `SKCanvas`, und nur ein Viertel werden angezeigt. Die `PaintSurface` Handler `HendecagramPage` verwendet stattdessen `Translate` um im Zeichenbereich mit mehreren Kopien des Sterns Kachel, jeweils nach dem Zufallsprinzip gefärbt:

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

[![](translate-images/hendecagramarray-small.png "Dreifacher Screenshot der Seite Hendecagram Array")](translate-images/hendecagramarray-large.png#lightbox "dreifacher Screenshot der Seite Hendecagram Array")

Animationen umfassen häufig Transformationen. Die **Hendecagram Animation** Seite 11 Spitzen Sterns über in einem Kreis bewegt. Die [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Klasse beginnt mit einigen Feldern und der überschreibt die `OnAppearing` und `OnDisappearing` Methoden zum Starten und Beenden einen Xamarin.Forms-Zeitgeber:

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

Die `angle` Feld animiert zwischen 0 und 360 Grad alle 5 Sekunden. Die `PaintSurface` Handler verwendet die `angle` Eigenschaft auf zwei Arten: an den Farbton der Farbe in der `SKColor.FromHsl` -Methode, und als Argument an die `Math.Sin` und `Math.Cos` Methoden, um zu steuern, den Speicherort des Sterns:

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

Die `PaintSurface` Ereignishandler ruft die `Translate` aufzurufende Methode zweimal, zuerst in der Mitte des Zeichenbereichs, übersetzt und dann in der Umfang eines Kreises bei zentraler übersetzt (0, 0). So groß wie möglich werden gleichzeitig die Stern innerhalb der Grenzen der Seite wird auf der Radius des Kreises festgelegt:

[![](translate-images/hendecagramanimation-small.png "Dreifacher Screenshot der Seite Hendecagram Animation")](translate-images/hendecagramanimation-large.png#lightbox "dreifacher Screenshot der Seite Hendecagram Animation")

Beachten Sie, dass die Stern dieselbe Ausrichtung beibehält, die es dreht, um die Mitte der Seite. Es ist nicht auf allen drehen. Dies ist ein Auftrag für eine Rotationstransformation.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
