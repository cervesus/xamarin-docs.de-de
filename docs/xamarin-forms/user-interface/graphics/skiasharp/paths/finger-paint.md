---
title: Finger zeichnen in skiasharp
description: In diesem Artikel wird erläutert, wie Sie mit Ihren Fingern in der skiasharp-Canvas in einer Xamarin.Forms -Anwendung zeichnen und dies mit Beispielcode veranschaulichen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 61ae651a2402204f69f642235d74d8d641b47988
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139021"
---
# <a name="finger-painting-in-skiasharp"></a>Finger zeichnen in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden Sie Ihre Finger, um die Zeichenfläche zu zeichnen._

Ein `SKPath` Objekt kann ständig aktualisiert und angezeigt werden. Diese Funktion ermöglicht die Verwendung eines Pfads für das interaktive zeichnen, z. b. in einem fingerzeichnungs Programm.

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

Die touchunterstützung in Xamarin.Forms ermöglicht nicht das Nachverfolgen von einzelnen Fingern auf dem Bildschirm, sodass ein Fingerabdruck der Xamarin.Forms touchverfolgung entwickelt wurde, um zusätzliche touchunterstützung bereitzustellen. Dieser Effekt wird im Artikel [**Aufrufen von Ereignissen aus Effekten**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)beschrieben. Die Demos der Beispielprogramm- [**Touch-nach Verfolgungs Effekte**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) enthalten zwei Seiten, die skiasharp verwenden, einschließlich eines fingerzeichnungs Programms.

Die Projekt Mappe [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) schließt dieses Ereignis für die Berührungs Nachverfolgung ein. Das .NET Standard Bibliotheksprojekt enthält die `TouchEffect` -Klasse, die `TouchActionType` -Enumeration, den-Delegaten `TouchActionEventHandler` und die- `TouchActionEventArgs` Klasse. Jedes der Platt Form Projekte enthält eine `TouchEffect` Klasse für diese Plattform. das IOS-Projekt enthält auch eine- `TouchRecognizer` Klasse.

Die **fingerpaint** -Seite in **skiasharpformsdemos** ist eine vereinfachte Implementierung der Finger Zeichnung. Die Auswahl von Farbe oder Strichbreite ist nicht zulässig. es ist nicht möglich, den Zeichenbereich zu löschen, und natürlich können Sie die Grafik nicht speichern.

Mit der Datei " [**fingerpaintpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml) " wird der `SKCanvasView` in eine einzelne Zelle eingefügt `Grid` und an die angefügt `TouchEffect` `Grid` :

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

Das direkte anfügen `TouchEffect` von an den funktioniert `SKCanvasView` nicht auf allen Plattformen.

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

Wie der Name vermuten lässt, `inProgressPaths` speichert das Wörterbuch die Pfade, die gerade von einem oder mehreren Fingern gezeichnet werden. Der Wörterbuch Schlüssel ist die Berührungs-ID, die die Berührungs Ereignisse begleitet. Das `completedPaths` Feld ist eine Auflistung von Pfaden, die beendet wurden, wenn ein Finger, der den Pfad gezeichnet hat, vom Bildschirm entfernt wurde.

Der `TouchAction` Handler verwaltet diese beiden Auflistungen. Wenn ein Finger den Bildschirm zum ersten Mal berührt, wird eine neue `SKPath` hinzugefügt `inProgressPaths` . Wenn dieser Finger bewegt wird, werden dem Pfad zusätzliche Punkte hinzugefügt. Wenn der Finger losgelassen wird, wird der Pfad an die Sammlung übertragen `completedPaths` . Sie können gleichzeitig mit mehreren Fingern zeichnen. Nach jeder Änderung an einem der Pfade oder Sammlungen `SKCanvasView` wird der ungültig:

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

Die Punkte, die die Berührungs Verfolgungs Ereignisse begleiten, sind Xamarin.Forms Koordinaten. diese müssen in skiasharp-Koordinaten konvertiert werden, bei denen es sich um Pixel handelt. Das ist der Zweck der- `ConvertToPixel` Methode.

Der `PaintSurface` Handler rendert dann einfach beide Auflistungen von Pfaden. Die vorherigen abgeschlossenen Pfade werden unterhalb der in Bearbeitung befindlichen Pfade angezeigt:

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

Ihre Finger Bilder sind nur durch ihr Talent eingeschränkt:

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

Sie haben nun gesehen, wie Sie Linien zeichnen und Kurven mithilfe von parametrischen Gleichungen definieren. Ein späterer Abschnitt zu [**skiasharp-Kurven und-Pfaden**](../curves/index.md) behandelt die verschiedenen Arten von Kurven, die von `SKPath` unterstützt werden. Eine sinnvolle Voraussetzung ist jedoch eine Untersuchung von [**skiasharp-Transformationen**](../transforms/index.md).

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Demos zu Fingerabdruck Effekten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [Aufrufen von Ereignissen über Effekte](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
