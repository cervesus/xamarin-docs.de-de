---
title: Zeichnen mit Fingern in SkiaSharp
description: In diesem Artikel wird erläutert, wie Sie mit den Fingern zeichnen auf der Leinwand SkiaSharp in Xamarin.Forms-Anwendung, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
ms.openlocfilehash: d5cf0927c64732d6d0a44204db9509fae77f0d1d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724073"
---
# <a name="finger-painting-in-skiasharp"></a>Zeichnen mit Fingern in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden Sie Ihre Finger, um die Zeichenfläche zu zeichnen._

Ein `SKPath`-Objekt kann ständig aktualisiert und angezeigt werden. Mit diesem Feature können einen Pfad zu den für interaktive Funktionen zum Zeichnen, z. B. in einem multitoucheingaben Programm verwendet werden.

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

Die Touch-Unterstützung in Xamarin.Forms lässt sich nicht auf einzelner Finger verfolgt, auf dem Bildschirm, damit ein Nachverfolgen von Touch-Effekt Xamarin.Forms entwickelt wurde, um zusätzliche Multitouch-Unterstützung bieten. Dieser Effekt wird im Artikel [**Aufrufen von Ereignissen aus Effekten**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)beschrieben. Die Demos der Beispielprogramm- [**Touch-nach Verfolgungs Effekte**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) enthalten zwei Seiten, die skiasharp verwenden, einschließlich eines fingerzeichnungs Programms.

Die Projekt Mappe [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) schließt dieses Ereignis für die Berührungs Nachverfolgung ein. Das .NET Standard Bibliotheksprojekt enthält die `TouchEffect` Klasse, die `TouchActionType`-Enumeration, den `TouchActionEventHandler` Delegaten und die `TouchActionEventArgs`-Klasse. Jedes der Platt Form Projekte enthält eine `TouchEffect` Klasse für diese Plattform. Das IOS-Projekt enthält auch eine `TouchRecognizer`-Klasse.

Die **fingerpaint** -Seite in **skiasharpformsdemos** ist eine vereinfachte Implementierung der Finger Zeichnung. Damit Farbe ausgewählt werden können oder nicht Strichbreite, hat keine Möglichkeit, deaktivieren Sie im Zeichenbereich und Sie können Ihre Grafik natürlich nicht speichern.

Mit der Datei " [**fingerpaintpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml) " werden die `SKCanvasView` in einer eindimensionalen `Grid` abgelegt und die `TouchEffect` an dieses `Grid`angefügt:

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

Das direkte Anfügen des `TouchEffect` an die `SKCanvasView` funktioniert nicht auf allen Plattformen.

Die Code-Behind-Datei [**FingerPaintPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml.cs) definiert zwei Auflistungen zum Speichern der `SKPath` Objekte sowie ein `SKPaint` Objekt zum Rendern dieser Pfade:

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

Wie der Name bereits vermuten lässt, speichert das `inProgressPaths` Wörterbuch die Pfade, die gerade von einem oder mehreren Fingern gezeichnet werden. Die Wörterbuchschlüssel ist die Touch-ID, die die Touch-Ereignissen begleitet. Das `completedPaths` Feld ist eine Auflistung von Pfaden, die abgeschlossen wurden, wenn ein Finger, der den Pfad gezeichnet hat, vom Bildschirm entfernt wurde.

Der `TouchAction` Handler verwaltet diese beiden Auflistungen. Wenn ein Finger den Bildschirm zum ersten Mal berührt, wird `inProgressPaths`ein neuer `SKPath` hinzugefügt. Wie Fingers verschoben wird, werden zusätzliche Punkte zum Pfad hinzugefügt. Wenn der Finger losgelassen wird, wird der Pfad an die `completedPaths` Sammlung übertragen. Sie können gleichzeitig mit mehreren Fingern zeichnen. Nach jeder Änderung an einem der Pfade oder Auflistungen wird der `SKCanvasView` für ungültig erklärt:

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

Der `PaintSurface` Handler rendert dann einfach beide Auflistungen von Pfaden. Die zuvor abgeschlossenen Pfade, die unter den Pfaden in Bearbeitung angezeigt werden:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
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

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

Sie haben jetzt gesehen, wie zum Zeichnen von Linien und Kurven, die mithilfe von Parametrischer Formeln definiert werden. Ein späterer Abschnitt zu [**skiasharp-Kurven und-Pfaden**](../curves/index.md) behandelt die verschiedenen Arten von Kurven, die `SKPath` unterstützt. Eine sinnvolle Voraussetzung ist jedoch eine Untersuchung von [**skiasharp-Transformationen**](../transforms/index.md).

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Demos zu Fingerabdruck Effekten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [Aufrufen von Ereignissen aus Effekten](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
