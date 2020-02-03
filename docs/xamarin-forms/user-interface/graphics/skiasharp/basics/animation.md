---
title: Grundlegende Animation in SkiaSharp
description: In diesem Artikel wird erläutert, wie zum Animieren von Grafiken SkiaSharp in Xamarin.Forms-Anwendungen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 80de16a0cf9b601ac3795085b638b9d62812f4d9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725550"
---
# <a name="basic-animation-in-skiasharp"></a>Grundlegende Animation in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie Ihre skiasharp-Grafiken animieren_

Sie können skiasharp-Grafiken in xamarin. Forms animieren, indem Sie bewirken, dass die `PaintSurface`-Methode regelmäßig aufgerufen wird. dabei wird jedes Mal die Grafik etwas anders gezeichnet. Hier ist eine Animation, die weiter unten in diesem Artikel mit konzentrischen Kreise, die scheinbar aus dem Center erweitert:

![](animation-images/animationexample.png "Several concentric circles seemingly expanding from the center")

Die Seite mit der **pulsierenden Ellipse** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programm animiert die zwei Achsen einer Ellipse, sodass Sie zu einem pulsierenden wird, und Sie können sogar die Rate der pulfizierung steuern. Die Datei [**pulsatingellipsepage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) instanziiert eine xamarin. Forms-`Slider` und eine `Label`, um den aktuellen Wert des Schiebereglers anzuzeigen. Dies ist eine gängige Methode, um eine `SKCanvasView` in andere xamarin. Forms-Sichten zu integrieren:

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

Die Code-Behind-Datei instanziiert ein `Stopwatch` Objekt, das als eine Uhr mit hoher Genauigkeit fungiert. Durch die `OnAppearing` Überschreibung wird das `pageIsActive` Feld auf `true` festgelegt und eine Methode mit dem Namen `AnimationLoop`aufgerufen. Durch die `OnDisappearing` Überschreibung wird das `pageIsActive` Feld auf `false`festgelegt:

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

Die `AnimationLoop`-Methode startet die `Stopwatch` und dann Schleifen, während `pageIsActive` `true`ist. Dabei handelt es sich im Wesentlichen um eine "unendliche Schleife", während die Seite aktiv ist, das Programm jedoch nicht reagiert, da die Schleife mit einem-`Task.Delay` mit dem `await` Operator endet, der andere Teile der Programmfunktion ermöglicht. Das Argument für `Task.Delay` bewirkt, dass es nach 1/30. Sekunde ausgeführt wird. Definiert die Framerate der Animation.

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

Die `while`-Schleife beginnt mit dem Abrufen einer Cycle-Zeit vom `Slider`. Dies ist eine Zeit in Sekunden, z. B. 5. Die zweite Anweisung berechnet den Wert `t` für die *Zeit*. Bei einem `cycleTime` von 5 steigt `t` alle 5 Sekunden von 0 auf 1. Das Argument für die `Math.Sin`-Funktion in der zweiten Anweisung reicht alle 5 Sekunden von 0 bis 2 aus. Die `Math.Sin`-Funktion gibt einen Wert zwischen 0 und 1 zurück zu 0 und dann alle 5 Sekunden auf &ndash;1 und 0 zurück, wobei sich die Werte langsamer ändern, wenn der Wert in der Nähe von 1 oder – 1 liegt. Der Wert 1 wird hinzugefügt, damit die Werte immer positive sind aus, und klicken Sie dann diese durch 2 geteilt wird, damit die Werte von ½, können Sie ½ auf ½, aber langsamer reichen, wenn der Wert, etwa 1 und 0 ist 0 1. Dies wird im Feld `scale` gespeichert, und die `SKCanvasView` wird für ungültig erklärt.

Die `PaintSurface`-Methode verwendet diesen `scale` Wert, um die zwei Achsen der Ellipse zu berechnen:

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

Die Methode berechnet eine maximale basierend auf der Größe des Anzeigebereichs, und eine minimale Radius basierend auf den maximalen Radius. Der `scale` Wert wird zwischen 0 und 1 und zurück zu 0 animiert, sodass die-Methode diesen verwendet, um eine `xRadius` zu berechnen und `yRadius`, die zwischen `minRadius` und `maxRadius`liegen. Diese Werte werden verwendet, und geben Sie eine Ellipse:

[![](animation-images/pulsatingellipse-small.png "Triple screenshot of the Pulsating Ellipse page")](animation-images/pulsatingellipse-large.png#lightbox "Triple screenshot of the Pulsating Ellipse page")

Beachten Sie, dass das `SKPaint` Objekt in einem `using` Block erstellt wird. Wie viele skiasharp-Klassen `SKPaint` von `SKObject`abgeleitet, das von `SKNativeObject`abgeleitet wird, das die [`IDisposable`](xref:System.IDisposable) Schnittstelle implementiert. `SKPaint` überschreibt die `Dispose`-Methode, um nicht verwaltete Ressourcen freizugeben.

 Wenn Sie `SKPaint` in einem `using`-Block platzieren, wird sichergestellt, dass `Dispose` am Ende des-Blocks aufgerufen wird, um diese nicht verwalteten Ressourcen freizugeben. Dies ist dennoch der Fall, wenn der vom `SKPaint` Objekt verwendete Arbeitsspeicher von der .NET-Garbage Collector freigegeben wird. im Animations Code empfiehlt es sich jedoch, den Speicher auf eine sicherere Weise freizugeben.

 Eine bessere Lösung in diesem speziellen Fall wäre das einmalige Erstellen von zwei `SKPaint` Objekten und das Speichern als Felder.

Das ist das, was die Animation für **erweiternde Kreise** bewirkt. Die [`ExpandingCirclesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Klasse beginnt mit dem Definieren mehrerer Felder, einschließlich eines `SKPaint` Objekts:

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

Dieses Programm verwendet eine andere Herangehensweise an die Animation auf der Grundlage der xamarin. Forms-`Device.StartTimer`-Methode. Das `t` Feld wird alle `cycleTime` Millisekunden zwischen 0 und 1 animiert:

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

Der `PaintSurface`-Handler zeichnet fünf konzentrische Kreise mit animierten Radii. Wenn die `baseRadius` Variable als 100 berechnet wird, wird die Radien der fünf Kreise, wenn `t` von 0 bis 1 animiert wird, von 0 auf 100, 100 bis 200, 200 auf 300, 300 auf 400 und 400 auf 500 vergrößert. Bei den meisten Kreisen ist der `strokeWidth` 50, aber für den ersten Kreis wird der `strokeWidth` von 0 bis 50 animiert. Für die meisten der Kreise die Farbe Blau ist, aber für den letzten Kreis, die Farbe von Blau transparent animiert wird. Beachten Sie das vierte Argument für den `SKColor`-Konstruktor, der die Deckkraft angibt:

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

Das Ergebnis ist, dass das Bild identisch ist, wenn `t` gleich 0 ist, als wenn `t` gleich 1 ist, und die Kreise werden scheinbar immer weiter erweitert:

[![](animation-images/expandingcircles-small.png "Triple screenshot of the Expanding Circles page")](animation-images/expandingcircles-large.png#lightbox "Triple screenshot of the Expanding Circles page")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
