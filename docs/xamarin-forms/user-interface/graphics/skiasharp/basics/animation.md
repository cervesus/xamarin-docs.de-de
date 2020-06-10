---
Title: "grundlegende Animation in skiasharp" Description: "in diesem Artikel wird erläutert, wie Sie Ihre skiasharp-Grafiken in Xamarin.Forms Anwendungen animieren, und dies wird mit Beispielcode veranschaulicht."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 31c96fd6-07e4-4473-a551-24753a5118c3 Author: davidbritch ms. Author: dabritch ms. Date: 03/10/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="basic-animation-in-skiasharp"></a>Grundlegende Animation in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie, wie Sie Ihre skiasharp-Grafiken animieren_

Sie können skiasharp-Grafiken in animieren Xamarin.Forms , indem Sie die- `PaintSurface` Methode in regelmäßigen Abständen aufrufen. Im folgenden finden Sie eine Animation, die später in diesem Artikel mit konzentrischen Kreisen gezeigt wird, die sich scheinbar von der Mitte aus erweitern:

![](animation-images/animationexample.png "Several concentric circles seemingly expanding from the center")

Die Seite mit der **pulsierenden Ellipse** im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programm animiert die zwei Achsen einer Ellipse, sodass Sie zu einem pulsierenden wird, und Sie können sogar die Rate der pulfizierung steuern. Die Datei [**pulsatingellipsepage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) instanziiert ein Xamarin.Forms `Slider` -und ein-Zeichen `Label` , um den aktuellen Wert des Schiebereglers anzuzeigen. Dies ist eine gängige Methode, um eine in `SKCanvasView` andere Xamarin.Forms Ansichten zu integrieren:

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

Die Code-Behind-Datei instanziiert ein- `Stopwatch` Objekt, das als zeitintensive Uhr fungiert. Die `OnAppearing` Überschreibung legt das `pageIsActive` -Feld auf fest `true` und ruft eine Methode mit dem Namen auf `AnimationLoop` . Durch das `OnDisappearing` Überschreiben wird dieses Feld auf Folgendes festgelegt `pageIsActive` `false` :

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

Die `AnimationLoop` -Methode startet die `Stopwatch` und dann Schleifen, während gleich `pageIsActive` ist `true` . Dabei handelt es sich im Grunde um eine "unendliche Schleife", während die Seite aktiv ist, aber nicht, da die Schleife mit einem-Operator mit `Task.Delay` dem- `await` Operator endet, der andere Teile der Programmfunktion ermöglicht. Das-Argument von bewirkt, dass `Task.Delay` es nach 1/30. Sekunde ausgeführt wird. Dadurch wird die Framerate der Animation definiert.

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

Die `while` Schleife beginnt mit dem Abrufen einer Cycle-Zeit aus `Slider` . Dies ist eine Zeit in Sekunden, z. b. 5. Die zweite Anweisung berechnet den Wert `t` für *time*. Bei einem `cycleTime` von 5 `t` erhöht sich alle 5 Sekunden von 0 auf 1. Das Argument für die `Math.Sin` Funktion in der zweiten Anweisung reicht alle 5 Sekunden von 0 bis 2 aus. Die `Math.Sin` -Funktion gibt einen Wert von 0 bis 1 zurück zu 0 und dann &ndash; alle 5 Sekunden auf 1 und 0 zurück, wobei sich die Werte langsamer ändern, wenn der Wert in der Nähe von 1 oder – 1 liegt. Der Wert 1 wird hinzugefügt, sodass die Werte immer positiv sind, und dann wird Sie durch 2 dividiert, sodass die Werte zwischen 1/2 und 1 bis 1/2 bis 0 bis 1/2 liegen und langsamer sind, wenn der Wert ungefähr 1 und 0 beträgt. Dies wird im `scale` -Feld gespeichert, und der `SKCanvasView` wird ungültig.

Die- `PaintSurface` Methode verwendet diesen `scale` Wert, um die zwei Achsen der Ellipse zu berechnen:

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

Die-Methode berechnet einen maximalen Radius basierend auf der Größe des Anzeige Bereichs und einem minimalen Radius basierend auf dem maximalen Radius. Der `scale` Wert wird zwischen 0 und 1 und zurück zu 0 animiert, sodass die-Methode verwendet, um einen zu berechnen, der `xRadius` `yRadius` zwischen `minRadius` und liegt `maxRadius` . Diese Werte werden verwendet, um eine Ellipse zu zeichnen und auszufüllen:

[![](animation-images/pulsatingellipse-small.png "Triple screenshot of the Pulsating Ellipse page")](animation-images/pulsatingellipse-large.png#lightbox "Triple screenshot of the Pulsating Ellipse page")

Beachten Sie, dass das- `SKPaint` Objekt in einem-Block erstellt wird `using` . Wie viele skiasharp-Klassen werden `SKPaint` von abgeleitet `SKObject` , das von abgeleitet `SKNativeObject` wird und die- [`IDisposable`](xref:System.IDisposable) Schnittstelle implementiert. `SKPaint`überschreibt die- `Dispose` Methode, um nicht verwaltete Ressourcen freizugeben.

 Durch `SKPaint` das Einfügen in einen- `using` Block wird sichergestellt, dass `Dispose` am Ende des-Blocks aufgerufen wird, um diese nicht verwalteten Ressourcen freizugeben. Dies ist dennoch der Fall, wenn der vom Objekt verwendete Arbeitsspeicher `SKPaint` durch den .NET-Garbage Collector freigegeben wird, aber im Animations Code ist es am besten, den Speicher auf eine sicherere Weise freizugeben.

 Eine bessere Lösung in diesem speziellen Fall besteht darin, zwei `SKPaint` Objekte einmal zu erstellen und Sie als Felder zu speichern.

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

Dieses Programm verwendet einen anderen Ansatz für die Animation basierend auf der- Xamarin.Forms `Device.StartTimer` Methode. Das `t` Feld wird alle Millisekunden zwischen 0 und 1 animiert `cycleTime` :

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

Der `PaintSurface` Handler zeichnet fünf konzentrische Kreise mit animierten Radii. Wenn die `baseRadius` Variable als 100 berechnet wird, wird die Zahl der fünf Kreise, wie Sie `t` zwischen 0 und 1 animiert wird, von 0 auf 100, 100 bis 200, 200 auf 300, 300 auf 400 und 400 auf 500 vergrößert. Bei den meisten der Kreise `strokeWidth` ist 50, aber für den ersten Kreis wird die `strokeWidth` Animation zwischen 0 und 50 durchlaufen. In den meisten Kreisen ist die Farbe blau, aber im letzten Kreis wird die Farbe von blau zu transparent animiert. Beachten Sie das vierte Argument des `SKColor` Konstruktors, der die Deckkraft angibt:

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

Das Ergebnis ist, dass das Bild identisch ist, wenn `t` gleich 0 ist `t` , wenn gleich 1 ist, und die Kreise auch immer weiter erweitert werden sollen:

[![](animation-images/expandingcircles-small.png "Triple screenshot of the Expanding Circles page")](animation-images/expandingcircles-large.png#lightbox "Triple screenshot of the Expanding Circles page")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
