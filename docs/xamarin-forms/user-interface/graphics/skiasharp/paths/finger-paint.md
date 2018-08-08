---
title: Zeichnen mit Fingern in SkiaSharp
description: In diesem Artikel wird erläutert, wie Sie mit den Fingern zeichnen auf der Leinwand SkiaSharp in Xamarin.Forms-Anwendung, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: b0f28cd3e8a928a6da3169dee96ec089178a64e2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615820"
---
# <a name="finger-painting-in-skiasharp"></a>Zeichnen mit Fingern in SkiaSharp

_Verwenden Sie die Finger, um auf der Leinwand zeichnen._

Ein `SKPath` Objekt ständig aktualisiert und angezeigt werden kann. Mit diesem Feature können einen Pfad zu den für interaktive Funktionen zum Zeichnen, z. B. in einem multitoucheingaben Programm verwendet werden.

![](finger-paint-images/fingerpaintsample.png "Eine Übung in Zeichnen mit Fingern")

Die Touch-Unterstützung in Xamarin.Forms lässt sich nicht auf einzelner Finger verfolgt, auf dem Bildschirm, damit ein Nachverfolgen von Touch-Effekt Xamarin.Forms entwickelt wurde, um zusätzliche Multitouch-Unterstützung bieten. Dieser Effekt wird in diesem Artikel beschrieben [ **Aufrufen von Ereignissen von Auswirkungen**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Das Beispielprogramm [ **Auswirkung Nachverfolgen von Touch-Demos** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) enthält zwei Seiten, die SkiaSharp, einschließlich von multitoucheingaben Programm verwenden.

Die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Lösung umfasst dieses Nachverfolgen von Touch-Ereignis. .NET Standard Library-Projekt enthält die `TouchEffect` -Klasse, die `TouchActionType` Enumeration, die `TouchActionEventHandler` zu delegieren, und die `TouchActionEventArgs` Klasse. Die Plattform-Projekte enthalten eine `TouchEffect` -Klasse für die Plattform, die iOS-Projekt enthält außerdem eine `TouchRecognizer` Klasse.

Die **Fingerpaint** auf der Seite **SkiaSharpFormsDemos** ist eine vereinfachte Implementierung der Zeichnen mit Fingern. Damit Farbe ausgewählt werden können oder nicht Strichbreite, hat keine Möglichkeit, deaktivieren Sie im Zeichenbereich und Sie können Ihre Grafik natürlich nicht speichern.

Die [ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) Datei fügt die `SKCanvasView` in einer einzelnen Zelle `Grid` und fügt die `TouchEffect` mit `Grid`:

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

Die [ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) Code-Behind-Datei definiert zwei Auflistungen für die Speicherung von der `SKPath` Objekte als auch eines `SKPaint` Objekt zum Rendern dieser Pfade:

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

Wie der Name der `inProgressPaths` Wörterbuch gespeichert, die Pfade, die derzeit von einem oder mehreren Fingern gezeichnet werden. Die Wörterbuchschlüssel ist die Touch-ID, die die Touch-Ereignissen begleitet. Die `completedPaths` Feld ist eine Auflistung der Pfade, die bei einem Finger, die den Pfad, der transformiert werden auf dem Bildschirm gezeichnet. abgeschlossen wurden.

Die `TouchAction` Handler verwaltet diesen beiden Sammlungen. Wenn ein Finger den Bildschirm, zuerst berührt ein neues `SKPath` hinzugefügt `inProgressPaths`. Wie Fingers verschoben wird, werden zusätzliche Punkte zum Pfad hinzugefügt. Wenn der Finger veröffentlicht wird, wird der Pfad zum Übertragen der `completedPaths` Auflistung. Sie können gleichzeitig mit mehreren Fingern zeichnen. Nach jeder Änderung an einer der Pfade oder Sammlungen die `SKCanvasView` ungültig wird:

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

Die Punkte, die zugehörigen der Nachverfolgen von Touch-Ereignissen sind Xamarin.Forms-Koordinaten. Diese müssen SkiaSharp-Koordinaten, die Pixel konvertiert werden. Dies ist der Zweck der `ConvertToPixel` Methode.

Die `PaintSurface` Handler rendert dann einfach beide Auflistungen von Pfaden. Die zuvor abgeschlossenen Pfade, die unter den Pfaden in Bearbeitung angezeigt werden:

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

Ihr Finger Originalwerke werden nur durch Ihr Talent beschränkt:

[![](finger-paint-images/fingerpaint-small.png "Dreifacher Screenshot der Seite Fingerpaint")](finger-paint-images/fingerpaint-large.png#lightbox "dreifachen Screenshot der Seite Fingerpaint")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Nachverfolgen von Touch-Wirkung-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Aufrufen von Ereignissen von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
