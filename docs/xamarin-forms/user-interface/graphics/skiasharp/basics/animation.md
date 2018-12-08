---
title: Grundlegende Animation in SkiaSharp
description: In diesem Artikel wird erläutert, wie zum Animieren von Grafiken SkiaSharp in Xamarin.Forms-Anwendungen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 8a533dd48acf698667044d600338555b6c00a0ae
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057626"
---
# <a name="basic-animation-in-skiasharp"></a>Grundlegende Animation in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Erfahren Sie, wie zum Animieren von SkiaSharp-Grafiken_

Sie können Grafiken von SkiaSharp in Xamarin.Forms animieren, verliert, indem die `PaintSurface` in regelmäßigen Abständen aufzurufende Methode jedes Mal, wenn die Grafik ein wenig anders zu zeichnen. Hier ist eine Animation, die weiter unten in diesem Artikel mit konzentrischen Kreise, die scheinbar aus dem Center erweitert:

![](animation-images/animationexample.png "Mehrere konzentrische Kreise scheinbar aus dem Center erweitern")

Die **Pulsating Ellipse** auf der Seite die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm erstellt eine Animation der beiden Achsen einer Ellipse, damit er angezeigt wird, um pulsating werden, und Sie können auch steuern, die Rate der dieser Pulsation. Die [ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) Datei instanziiert ein Xamarin.Forms `Slider` und `Label` den aktuellen Wert des Schiebereglers angezeigt. Dies ist eine allgemeine Möglichkeit zum Integrieren einer `SKCanvasView` mit anderen Xamarin.Forms-Ansichten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei instanziiert ein `Stopwatch` Objekt, das als eine hohe Genauigkeit Uhr dienen. Die `OnAppearing` außer Kraft setzen Legt die `pageIsActive` Feld `true` und ruft eine Methode namens `AnimationLoop`. Die `OnDisappearing` Außerkraftsetzung festlegt, `pageIsActive` Feld `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

Die `AnimationLoop` Methode startet die `Stopwatch` und klicken Sie dann die Schleifen während `pageIsActive` ist `true`. Dies ist im Grunde eine "Endlosschleife", während die Seite aktiv ist, führt jedoch nicht das Programm nicht mehr reagiert, da die Schleife wird, mit einem Aufruf von abgeschlossen `Task.Delay` mit der `await` -Operator, der anderen Teile der Programmfunktion kann. Das Argument für `Task.Delay` bewirkt, dass er nach 1 30./Sekunde abgeschlossen. Definiert die Framerate der Animation.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

Die `while` Schleife beginnt, mit dem Abrufen einer Zykluszeit aus der `Slider`. Dies ist eine Zeit in Sekunden, z. B. 5. Die zweite Anweisung berechnet einen Wert von `t` für *Zeit*. Für eine `cycleTime` 5 `t` erhöht, von 0 auf 1 alle 5 Sekunden. Das Argument für die `Math.Sin` -Funktion in der zweiten Anweisung liegt zwischen 0 und 2π alle 5 Sekunden. Die `Math.Sin` Funktion gibt einen Wert zwischen 0 und 1 zurück, die auf 0, und klicken Sie dann auf &ndash;1 und 0 5 Sekunden, aber mit Werten, die langsamer ändern, wenn der Wert in der Nähe von 1 oder – 1 ist. Der Wert 1 wird hinzugefügt, damit die Werte immer positive sind aus, und klicken Sie dann diese durch 2 geteilt wird, damit die Werte von ½, können Sie ½ auf ½, aber langsamer reichen, wenn der Wert, etwa 1 und 0 ist 0 1. Dies befindet sich in der `scale` Feld, und die `SKCanvasView` für ungültig erklärt.

Die `PaintSurface` Methode verwendet diesen `scale` Wert, der Berechnung der beiden Achsen der Ellipse:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Die Methode berechnet eine maximale basierend auf der Größe des Anzeigebereichs, und eine minimale Radius basierend auf den maximalen Radius. Die `scale` Wert animiert ist zwischen 0 und 1 und 0 (null) zurück, daher die Methode, die zum Berechnen verwendet einer `xRadius` und `yRadius` liegt, die im Bereich zwischen `minRadius` und `maxRadius`. Diese Werte werden verwendet, und geben Sie eine Ellipse:

[![](animation-images/pulsatingellipse-small.png "Dreifacher Screenshot der Seite animiertes Ellipse")](animation-images/pulsatingellipse-large.png#lightbox "dreifachen Screenshot der Seite animiertes Ellipse")

Beachten Sie, dass die `SKPaint` -Objekt wird erstellt, einem `using` Block. Wie viele Klassen von SkiaSharp `SKPaint` leitet sich von `SKObject`, die sich daraus ableitet `SKNativeObject`, implementiert die [ `IDisposable` ](xref:System.IDisposable) Schnittstelle. `SKPaint` überschreibt die `Dispose` Methode, um nicht verwaltete Ressourcen freizugeben.

 Einfügen von `SKPaint` in einem `using` Block stellt sicher, dass `Dispose` aufgerufen wird, am Ende des Blocks auf diesen nicht verwalteten Ressourcen frei. Dies geschieht bei Verwendung von Arbeitsspeicher durch trotzdem die `SKPaint` Objekt wird freigegeben, vom .NET Garbage Collector, aber im Code der Animation, es empfiehlt sich, bei der Freigabe von Arbeitsspeicher auf eine Weise transaktionsbesitzers proaktiv zu sein.

 In diesem Fall eine bessere Lösung wäre zwei `SKPaint` Objekte einmal, und speichern sie als Felder.

Was ist die **erweitern Kreise** Animation ist. Die [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Klasse beginnt, durch die Definition von mehreren Feldern, einschließlich einer `SKPaint` Objekt:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Dieses Programm verwendet einen anderen Ansatz für die Animation basierend auf der Xamarin.Forms `Device.StartTimer` Methode. Die `t` Feld wird von 0 auf 1 animiert jeder `cycleTime` Millisekunden:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
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

Die `PaintSurface` Handler zeichnet fünf konzentrische Kreise mit animierte Radien. Wenn die `baseRadius` Variable wird berechnet als 100 ist, klicken Sie dann als `t` wird von 0 auf 1 fest, die Radien der fünf Kreise Erhöhung von 0, 100, 100 und 200, 200, 300, 300, 400 und zwischen 400 und 500 animiert. Für die meisten der Kreise die `strokeWidth` ist 50, aber für die erste Kreis, der `strokeWidth` von 0 bis 50 animiert. Für die meisten der Kreise die Farbe Blau ist, aber für den letzten Kreis, die Farbe von Blau transparent animiert wird. Beachten Sie, dass das vierte Argument der `SKColor` Konstruktor, der angibt, die Deckkraft:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

Das Ergebnis ist, dass das Bild bei Verwendung sieht `t` gleich 0, als wenn `t` gleich 1 ist, und die Kreise scheinen weiterhin unbegrenzt erweitern:

[![](animation-images/expandingcircles-small.png "Dreifacher Screenshot der Seite erweitert Kreise")](animation-images/expandingcircles-large.png#lightbox "dreifachen Screenshot der Seite Kreise erweitern")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
