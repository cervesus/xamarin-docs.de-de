---
title: Grundlegende Animation
description: Erfahren Sie, wie Grafiken SkiaSharp animieren
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: d9eea30e1d9e55101975e59ba9d259fba909ca0f
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="basic-animation"></a>Grundlegende Animation

_Erfahren Sie, wie Grafiken SkiaSharp animieren_

Sie können Animationen SkiaSharp Grafiken in Xamarin.Forms verursacht die `PaintSurface` Methode, die sehr häufig aufgerufen werden jedes Mal, wenn die Grafiken etwas anders. Hier ist eine Animation, die weiter unten in diesem Artikel mit konzentrische Kreise, die scheinbar erweitern Sie in der Mitte dargestellt:

![](animation-images/animationexample.png "Mehrere konzentrische Kreise, die scheinbar aus dem Center erweitern")

Die **Pulsating Ellipse** auf der Seite der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm erstellt eine Animation der beiden Achsen einer Ellipse, damit er angezeigt wird, um pulsating werden, und Sie können auch steuern, die Rate der dieser Pulsation:


Die [ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) Datei instanziiert einen Xamarin.Forms `Slider` und ein `Label` den aktuellen Wert des Schiebereglers angezeigt. Dies ist eine allgemeine Möglichkeit zum Integrieren einer `SKCanvasView` mit anderen Ansichten Xamarin.Forms:

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

Die Code-Behind-Datei instanziiert einen `Stopwatch` Objekt als eine hohe Genauigkeit Uhr dienen. Die `OnAppearing` überschreiben legt die `pageIsActive` Feld `true` und ruft eine Methode mit dem Namen `AnimationLoop`. Die `OnDisappearing` Außerkraftsetzung festlegt, `pageIsActive` Feld `false`:

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

Die `AnimationLoop` Methode startet die `Stopwatch` und klicken Sie dann die Schleifen beim `pageIsActive` ist `true`. Dies ist im Wesentlichen eine "Endlosschleife", während auf der Seite "ist aktiv, aber es ist nicht dazu führen, dass das Programm nicht mehr reagiert, da die Schleife mit einem Aufruf von endet `Task.Delay` mit der `await` -Operator, der anderen Teile der Anwendung-Funktion kann. Das Argument für `Task.Delay` bewirkt, dass es nach 1 30./Sekunde abgeschlossen. Dadurch wird die Framerate der Animation definiert.

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

Die `while` Schleife beginnt, durch den Bezug einer Zykluszeit aus der `Slider`. Dies ist eine Zeit in Sekunden an, z. B. 5. Die zweite Anweisung berechnet einen Wert von `t` für *Zeit*. Für eine `cycleTime` 5 `t` erhöht, von 0 auf 1 alle 5 Sekunden. Das Argument für die `Math.Sin` -Funktion in der zweiten Anweisung liegt zwischen 0 und 2π alle 5 Sekunden. Die `Math.Sin` Funktion gibt einen Wert zwischen 0 und 1 zurück, die auf 0, und klicken Sie dann auf &ndash;1 und 0 für alle fünf Sekunden, aber mit Werten, die langsamer zu ändern, wenn der Wert in der Nähe von 1 oder – 1 ist. Der Wert 1 wird hinzugefügt, damit die Werte immer positive sind und klicken Sie dann es durch 2, geteilt wird damit die Werte reicht von ½ auf 1 fest, um ½ auf 0, um ½, jedoch langsamer, wenn der Wert ca. 1 und 0 ist. Diese befindet sich in der `scale` Feld, und die `SKCanvasView` ist ungültig.

Die `PaintSurface` Methode verwendet dieses `scale` Wert zum Berechnen der beiden Achsen der Ellipse:

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

Die Methode berechnet eine maximale basierend auf der Größe des Anzeigebereichs, und eine minimale Radius basierend auf den maximalen Radius. Die `scale` Wert wird animiert zwischen 0 und 1 und 0 (null) an, damit die Methode, die zum Berechnen verwendet ein `xRadius` und `yRadius` , die Bereiche zwischen `minRadius` und `maxRadius`. Diese Werte dienen zum Zeichnen und Ausfüllen einer Ellipse, die:

[![](animation-images/pulsatingellipse-small.png "Dreifacher Screenshot der Seite animiertes Ellipse")](animation-images/pulsatingellipse-large.png#lightbox "dreifacher Screenshot der Seite animiertes Ellipse")

Beachten Sie, dass die `SKPaint` -Objekt wird erstellt, einem `using` Block. Wie viele SkiaSharp Klassen `SKPaint` leitet sich von `SKObject`, die sich daraus ableitet `SKNativeObject`, implementiert die [ `IDisposable` ](https://developer.xamarin.com/api/type/System.IDisposable/) Schnittstelle. `SKPaint` überschreibt die `Dispose` Methode, um nicht verwaltete Ressourcen freizugeben.

 Einfügen `SKPaint` in einem `using` Block wird sichergestellt, dass `Dispose` aufgerufen wird, am Ende des Blocks diese nicht verwaltete Ressourcen freizugeben. Dies geschieht bei Verwendung von Arbeitsspeicher durch trotzdem die `SKPaint` Objekt wird freigegeben, durch den Garbage Collector von .NET, aber im Code der Animation, es wird empfohlen, bei der Freigabe von Speicher auf eine Weise transaktionsbesitzers proaktiv etwas.

 Eine bessere Lösung in diesem speziellen Fall wäre, erstellen Sie zwei `SKPaint` Objekte einmal, und speichern Sie sie als Felder.

Was ist die **erweitern Kreise** Animation verfügt. Die [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Klasse beginnt, durch die mehrere Felder, einschließlich der Definition einer `SKPaint` Objekt:

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

Dieses Programm verwendet einen anderen Ansatz zum Animation basierend auf den Xamarin.Forms `Device.StartTimer`. Die `t` Feld von 0 auf 1 animiert wird jede `cycleTime` Millisekunden:

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

Die `PaintSurface` Handler zeichnet 5 konzentrische Kreise mit animierten Radien. Wenn die `baseRadius` Variable wird berechnet als 100 ist, klicken Sie dann als `t` von 0 auf 1 fest, die Radien der fünf Kreise Erhöhung von 0, 100, 100, 200, 200, 300, 300, 400 und 400 und 500 animiert wird. Für die meisten Kreise der `strokeWidth` ist 50, aber für die erste Kreis, der `strokeWidth` von 0 bis 50 animiert. Für die meisten Kreise die Farbe ist Blau, aber für die letzten Kreis, die Farbe von Blau transparent animiert wird:

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

Das Ergebnis ist, dass das Bild bei Verwendung sieht `t` gleich 0, als wenn `t` gleich 1 ist, und die Kreise erscheinen, um den Vorgang fortzusetzen, erweitern eine unbegrenzte Zeitdauer aufbewahrt:

[![](animation-images/expandingcircles-small.png "Dreifacher Screenshot der Seite erweitern Kreise")](animation-images/expandingcircles-large.png#lightbox "dreifacher Screenshot der Seite Kreise erweitern")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
