---
title: Finger Paint-Ereignisse
description: Verwenden Sie die Finger im Zeichenbereich Paint-Ereignis.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: 95c023d702d165b7a8a0ba392b2f87af58bfae07
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="finger-painting"></a>Finger Paint-Ereignisse

_Verwenden Sie die Finger im Zeichenbereich Paint-Ereignis._

Ein `SKPath` Objekt ständig aktualisiert und angezeigt werden kann. Dieses Feature ermöglicht es sich um einen Pfad zu den für interaktive zeichnen, z. B. in einem finger-painting-Programm verwendet werden.

![](finger-paint-images/fingerpaintsample.png "Übung Finger Paint-Ereignisse")

Touch-Unterstützung in Xamarin.Forms lässt nicht zu einzelnen Finger auf dem Bildschirm nachverfolgen, damit Sie eine Xamarin.Forms-Touch-Überwachung Auswirkung entwickelt wurde, um zusätzliche Touch-Unterstützung bereitzustellen. Dieser Effekt wird im Artikel beschriebenen [ **Aufrufen Ereignisse von Auswirkungen**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Das Beispielprogramm [ **Touch-Tracking Auswirkungen Demos** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) enthält zwei Seiten, die SkiaSharp, einschließlich finger-painting Programm verwenden.

Die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Lösung umfasst dieses Touch-Tracking-Ereignis. Das Portable Class Library-Projekt enthält die `TouchEffect` -Klasse, die `TouchActionType` -Enumeration, die `TouchActionEventHandler` zu delegieren, und die `TouchActionEventArgs` Klasse. Plattformprojekte enthalten eine `TouchEffect` für diese Plattform-Klasse; das iOS-Projekt enthält außerdem eine `TouchRecognizer` Klasse.

Die **Finger Paint** auf der Seite **SkiaSharpFormsDemos** ist eine vereinfachte Finger zeichnen-Implementierung. Nicht zulassen Farbe auswählen oder Breite mit Strichen zu zeichnen, es wurde keine Möglichkeit zum Löschen des Zeichenbereichs, und Sie können nicht natürlich Bildmaterial speichern.

Die [ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) Datei setzt die `SKCanvasView` in einer einzelnen Zelle `Grid` und fügt die `TouchEffect` , `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Anfügen der `TouchEffect` direkt an die `SKCanvasView` funktioniert nicht unter allen Plattformen.

Die [ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) Code-Behind-Datei definiert zwei Auflistungen zum Speichern von der `SKPath` Objekte, sowie einen `SKPaint` Objekt für das Rendern von diese Pfade:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Wie der Name andeutet den `inProgressPaths` Wörterbuch speichert die Pfaden, die derzeit von einem oder mehreren Fingern gezeichnet werden. Die Wörterbuchschlüssel ist die Touch ID, die mit der Berührungsereignisse. Die `completedPaths` Feld ist eine Auflistung von Pfaden, die bei einem Finger, zeichnen den Pfad transformiert aus dem Bildschirm abgeschlossen wurden.

Die `TouchAction` Handler verwaltet diese zwei Auflistungen. Wenn ein Finger den Bildschirm berührt ein neues `SKPath` hinzugefügt `inProgressPaths`. Fingers bewegen, werden zusätzliche Punkte zum Pfad hinzugefügt. Nach der Finger freigegeben wird, wird der Pfad zum Übertragen der `completedPaths` Auflistung. Sie können gleichzeitig mit mehreren Fingern zeichnen. Nach jeder Änderung an einem der Pfade oder Sammlungen die `SKCanvasView` ist ungültig:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Die begleitende Touch-Nachverfolgungsereignisse Punkte sind Xamarin.Forms Koordinaten. Diese müssen SkiaSharp Koordinaten, die Pixel konvertiert werden. Das ist der Zweck der `ConvertToPixel` Methode.

Die `PaintSurface` Handler rendert dann einfach beide Auflistungen von Pfaden. Die zuvor abgeschlossenen Pfade, die unterhalb der Pfade in Bearbeitung angezeigt werden:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Ihre Fingermalereien sind nur durch Ihre Talent beschränkt:

[![](finger-paint-images/fingerpaint-small.png "Dreifacher Screenshot der Seite Finger Paint")](finger-paint-images/fingerpaint-large.png#lightbox "dreifacher Screenshot der Seite Finger Paint")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Touch-Tracking Auswirkungen Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Aufrufen von Ereignissen von Effekten](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
