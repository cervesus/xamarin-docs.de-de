---
title: Pfad Effekte in skiasharp
description: In diesem Artikel werden die verschiedenen skiasharp-Pfad Effekte erläutert, die die Verwendung von Pfaden für das streicheln und Auffüllen ermöglichen und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: davidbritch
ms.author: dabritch
ms.date: 07/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f3a5a581ffb4ca2acf1d4209b8b7a744f0daa5eb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128047"
---
# <a name="path-effects-in-skiasharp"></a>Pfad Effekte in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Entdecken Sie die verschiedenen Pfad Effekte, die die Verwendung von Pfaden zum übernehmen und Auffüllen ermöglichen._

Ein *Pfad Effekt* ist eine Instanz der- [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) Klasse, die mit einer von acht statischen Erstellungs Methoden erstellt wird, die von der-Klasse definiert werden. Das- `SKPathEffect` Objekt wird dann auf die- [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) Eigenschaft eines [`SKPaint`](xref:SkiaSharp.SKPaint) Objekts für eine Vielzahl interessanter Effekte festgelegt, z. b. eine Linie mit einem kleinen replizierten Pfad:

![Beispiel für eine verknüpfte Kette](effects-images/patheffectsample.png)

Mit Pfad Effekten können Sie folgende Aktionen ausführen:

- Zeichnen einer Linie mit Punkten und Bindestrichen
- Einen Strich mit einem beliebigen ausgefüllten Pfad zeichnen
- Bereich mit schraffurzeilen ausfüllen
- Einen Bereich mit einem gekachelten Pfad ausfüllen
- Erstellen von spitzen Ecken gerundet
- Hinzufügen von zufälligem "Jitter" zu Linien und Kurven

Außerdem können Sie zwei oder mehr Pfad Effekte kombinieren.

In diesem Artikel wird auch veranschaulicht, wie Sie mithilfe der [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) -Methode von `SKPaint` einen Pfad in einen anderen Pfad konvertieren, indem Sie die Eigenschaften von `SKPaint` , einschließlich und, anwenden `StrokeWidth` `PathEffect` . Dies führt zu einigen interessanten Techniken, z. b. beim Abrufen eines Pfades, bei dem es sich um einen anderen Pfad handelt. `GetFillPath`ist auch hilfreich bei der Verbindung mit den Pfad Effekten.

## <a name="dots-and-dashes"></a>Punkte und Striche

Die Verwendung der- [`PathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) Methode wurde im Artikel [**Punkte und Bindestriche**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)beschrieben. Das erste Argument der-Methode ist ein Array, das eine gerade Anzahl von zwei oder Mehrwerten enthält, die zwischen Längen von Bindestrichen und Längen von Lücken zwischen den Bindestrichen wechseln:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Diese Werte sind *nicht* relativ zur Strichbreite. Wenn z. b. die Strichbreite 10 beträgt und Sie eine Linie aus quadratischen Bindestrichen und quadratischen Lücken wünschen, legen Sie das `intervals` Array auf {10, 10} fest. Das-Argument gibt an, an welcher Stelle `phase` innerhalb des Strich Musters die Zeile beginnt. Wenn die Zeile in diesem Beispiel mit der quadratischen Lücke beginnen soll, legen Sie `phase` auf 10 fest.

Die Enden der Bindestriche werden von der- `StrokeCap` Eigenschaft von beeinflusst `SKPaint` . Bei breiten Strichen ist es sehr üblich, diese Eigenschaft auf festzulegen, `SKStrokeCap.Round` um die Enden der Bindestriche zu runden. In diesem Fall enthalten die Werte im `intervals` array *nicht* die zusätzliche Länge, die sich aus der Rundung ergibt. Dies bedeutet, dass ein Zirkel Punkt die Angabe einer Breite von 0 (null) erfordert. Um eine Strichbreite von 10 zu erstellen, verwenden Sie ein `intervals` Array von {0, 20}, um eine Linie mit Zirkel Punkten und Lücken zwischen den Punkten desselben Durchmessers zu erstellen.

Die animierte Textseite für **gepunktete** Text ähnelt der im Artikel [**integrieren von Text und Grafiken**](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) beschriebenen **Textseite** , in der die Textzeichen durch Festlegen der- `Style` Eigenschaft des- `SKPaint` Objekts auf dargestellt werden `SKPaintStyle.Stroke` . Außerdem verwendet der **animierte gepunktete Text** , `SKPathEffect.CreateDash` um diesem Umriss eine gepunktete Darstellung zu geben. das Programm animiert auch das- `phase` Argument der- `SKPathEffect.CreateDash` Methode, damit die Punkte zu den Text Zeichen bewegt werden. Hier befindet sich die Seite im Querformat:

[![Dreifacher Screenshot der animierten gepunkteten Textseite](effects-images/animateddottedtext-small.png)](effects-images/animateddottedtext-large.png#lightbox)

Die [`AnimatedDottedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasse beginnt mit dem definieren einiger Konstanten und überschreibt außerdem die `OnAppearing` -Methode und die- `OnDisappearing` Methode für die Animation:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
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

Der `PaintSurface` Handler beginnt mit dem Erstellen eines- `SKPaint` Objekts, um den Text anzuzeigen. Die `TextSize` Eigenschaft wird basierend auf der Breite des Bildschirms angepasst:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds = new SKRect();
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

Am Ende der Methode `SKPathEffect.CreateDash` wird die-Methode mit dem aufgerufen, der `dashArray` als Feld definiert ist, und dem animierten `phase` Wert. Die- `SKPathEffect` Instanz wird auf die- `PathEffect` Eigenschaft des-Objekts festgelegt `SKPaint` , um den Text anzuzeigen.

Alternativ können Sie das `SKPathEffect` -Objekt auf das- `SKPaint` Objekt festlegen, bevor Sie den Text messen und auf der Seite zentrieren. In diesem Fall bewirken die animierten Punkte und Bindestriche eine gewisse Variation in der Größe des gerenderten Texts, und der Text wird tendenziell etwas vibrieren. (Testen!)

Sie werden auch feststellen, dass es bei den animierten Punkten um die Textzeichen zu einer bestimmten Stelle in jeder geschlossenen Kurve kommt, in der die Punkte erscheinen und nicht vorhanden sind. An dieser Stelle beginnt und endet der Pfad, der die Zeichen Gliederung definiert. Wenn die Pfadlänge kein ganzzahliges Vielfaches der Länge des Bindestrich Musters ist (in diesem Fall 20 Pixel), kann nur ein Teil dieses Musters am Ende des Pfads angepasst werden.

Es ist möglich, die Länge des Bindestrich Musters an die Länge des Pfads anzupassen. Dies erfordert jedoch, dass die Länge des Pfads bestimmt wird. Dies ist eine Technik, die in den Artikel [**Pfadinformationen und-Enumeration**](information.md)behandelt wird.

Mit dem Programm " **Dot/Dash Morph** " wird das Dash-Muster selbst animiert, sodass Gedanken Striche in Punkte aufgeteilt werden, die wiederum zu Bindestrichen führen:

[![Dreifacher Screenshot der Punkt-Dash-Morph-Seite](effects-images/dotdashmorph-small.png)](effects-images/dotdashmorph-large.png#lightbox)

Die [`DotDashMorphPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) -Klasse überschreibt die `OnAppearing` -Methode und die- `OnDisappearing` Methode wie das vorherige Programm, aber die-Klasse definiert das- `SKPaint` Objekt als-Feld:

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

Der `PaintSurface` Handler erstellt einen elliptischen Pfad basierend auf der Größe der Seite und führt einen langen Code Abschnitt aus, der die `dashArray` Variablen und festlegt `phase` . Da die animierte Variable `t` zwischen 0 und 1 liegt, `if` brechen die Blöcke diese Zeit in vier Quartale ein, und in jedem dieser Quartale liegt der Bereich `tsub` ebenfalls zwischen 0 und 1. Am Ende erstellt das Programm den `SKPathEffect` und legt ihn auf das- `SKPaint` Objekt für das Zeichnen fest.

## <a name="from-path-to-path"></a>Von Pfad zu Pfad

Die- [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single)) Methode von `SKPaint` wandelt einen Pfad auf der Grundlage der Einstellungen im-Objekt in einen anderen um `SKPaint` . Um zu sehen, wie dies funktioniert, ersetzen Sie den-Befehl `canvas.DrawPath` im vorherigen Programm durch den folgenden Code:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

In diesem neuen Code konvertiert der-Befehl `GetFillPath` den `ellipsePath` (der nur ein Oval ist) in `newPath` , der dann mit angezeigt wird `newPaint` . Das- `newPaint` Objekt wird mit allen Standard Eigenschafts Einstellungen erstellt, mit dem Unterschied, dass die- `Style` Eigenschaft basierend auf dem booleschen Rückgabewert von festgelegt wird `GetFillPath` .

Die visuellen Elemente sind identisch mit Ausnahme der Farbe, die in festgelegt ist, `ellipsePaint` aber nicht `newPaint` . Statt der einfachen Ellipse, die in definiert `ellipsePath` ist, `newPath` enthält zahlreiche Pfad Kontur, die die Reihen der Punkte und Bindestriche definieren. Dies ist das Ergebnis der Anwendung verschiedener Eigenschaften von `ellipsePaint` (insbesondere, `StrokeWidth` , `StrokeCap` und `PathEffect` ) auf `ellipsePath` und das Platzieren des resultierenden Pfades in `newPath` . Die- `GetFillPath` Methode gibt einen booleschen Wert zurück, der angibt, ob der Zielpfad ausgefüllt werden soll. in diesem Beispiel dient der Rückgabewert `true` zum Auffüllen des Pfads.

Versuchen `Style` Sie, die Einstellung in in `newPaint` zu ändern, `SKPaintStyle.Stroke` und Sie sehen, dass die einzelnen Pfad Kontur mit einer Zeile mit einer Breite von einer Pixel Breite dargestellt werden.

## <a name="stroking-with-a-path"></a>Über einen Pfad

Die- [`SKPathEffect.Create1DPath`](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle)) Methode ähnelt konzeptionell, mit dem Unterschied `SKPathEffect.CreateDash` , dass Sie anstelle eines Musters von Bindestrichen und Lücken einen Pfad angeben. Dieser Pfad wird mehrmals repliziert, um die Linie oder die Kurve zu zeichnen.

Die Syntax ist:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

Im Allgemeinen ist der Pfad, den Sie an übergeben, `Create1DPath` klein und dreht sich um den Punkt (0,0). Der- `advance` Parameter gibt den Abstand zwischen den Centern des Pfads an, wenn der Pfad in der Zeile repliziert wird. Normalerweise legen Sie dieses Argument auf die ungefähre Breite des Pfads fest. Das- `phase` Argument spielt hier dieselbe Rolle wie in der- `CreateDash` Methode.

Der [`SKPath1DPathEffectStyle`](xref:SkiaSharp.SKPath1DPathEffectStyle) hat drei Mitglieder:

- `Translate`
- `Rotate`
- `Morph`

Der- `Translate` Member bewirkt, dass der Pfad in derselben Richtung verbleibt, in der er entlang einer Linie oder Kurve repliziert wird. Für `Rotate` wird der Pfad auf der Grundlage eines Tangens der Kurve gedreht. Der Pfad hat seine normale Ausrichtung für horizontale Linien. `Morph`ähnelt, mit dem Unterschied `Rotate` , dass der Pfad selbst auch gekrümmt ist, um mit der Krümmung der gezeichneten Linie übereinzustimmen.

Auf der Seite **1D-Pfad Effekt** werden diese drei Optionen veranschaulicht. Die Datei " [**onedimensionalpatheffectpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) " definiert eine Auswahl, die drei Elemente enthält, die den drei Membern der-Enumeration entsprechen:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Translate</x:String>
                    <x:String>Rotate</x:String>
                    <x:String>Morph</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

Die Code-Behind-Datei [**OneDimensionalPathEffectPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) definiert drei `SKPathEffect` Objekte als Felder. Diese werden alle mithilfe `SKPathEffect.Create1DPath` von mit `SKPath` erstellten Objekten erstellt `SKPath.ParseSvgPathData` . Das erste ist ein einfaches Feld, das zweite ist eine Rautenform, das dritte ist ein Rechteck. Diese werden verwendet, um die drei Effekt Stile zu veranschaulichen:

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch ((string)effectStylePicker.SelectedItem))
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

Der `PaintSurface` Handler erstellt eine Bézier-Kurve, die sich selbst durchläuft, und greift auf die Auswahl zu, um zu bestimmen, welcher `PathEffect` zum Zeichnen verwendet werden soll. Die drei Optionen – `Translate` , `Rotate` und `Morph` – werden von links nach rechts angezeigt:

[![Dreifacher Screenshot der Seite mit den 1-d-Pfad Effekten](effects-images/1dpatheffect-small.png)](effects-images/1dpatheffect-large.png#lightbox)

Der in der-Methode angegebene Pfad `SKPathEffect.Create1DPath` wird immer ausgefüllt. Der in der-Methode angegebene Pfad `DrawPath` wird immer mit Strichen gezeichnet, wenn die- `SKPaint` Eigenschaft des `PathEffect` -Objekts auf einen 1D-Pfad Effekt festgelegt ist. Beachten Sie, dass das- `pathPaint` Objekt keine `Style` Einstellung aufweist, die normalerweise standardmäßig ist `Fill` , aber der Pfad ist unabhängig davon gezeichnet.

Das im Beispiel verwendete Feld `Translate` ist 20 Pixel quadratisch, und das- `advance` Argument wird auf 24 festgelegt. Dieser Unterschied bewirkt eine Lücke zwischen den Feldern, wenn die Linie ungefähr horizontal oder vertikal ist, aber die Felder überlappen ein wenig, wenn die Linie diagonal ist, da die Diagonale des Felds 28,3 Pixel ist.

Die Rautenform im `Rotate` Beispiel ist ebenfalls 20 Pixel breit. Der `advance` wird auf 20 festgelegt, sodass die Punkte sich weiterhin berühren, wenn der Diamant mit der Krümmung der Linie gedreht wird.

Die Form des Rechtecks im `Morph` Beispiel ist 50 Pixel breit und hat die `advance` Einstellung 55, um zwischen den Rechtecke eine kleine Lücke zu schaffen, wenn Sie sich um die Bézier-Kurve bewegen.

Wenn das `advance` Argument kleiner als die Größe des Pfads ist, können sich die replizierten Pfade überlappen. Dies kann zu einigen interessanten Auswirkungen führen. Die Seite **verknüpfte Kette** zeigt eine Reihe von überlappenden Kreisen an, die einer verknüpften Kette ähneln, die in der unterschiedlichen Form einer Kategorie angezeigt wird:

[![Dreifacher Screenshot der Seite "verknüpfte Kette"](effects-images/linkedchain-small.png)](effects-images/linkedchain-large.png#lightbox)

Sehen Sie sehr nah aus, und Sie sehen, dass es sich nicht um Kreise handelt. Jeder Link in der Kette besteht aus zwei Arcs, deren Größen und positioniert sind, sodass Sie mit benachbarten Links verbunden werden.

Eine Kette oder ein Kabel mit einheitlicher Gewichtungs Verteilung ist in Form eines katary nicht mehr verbleibt. Ein in Form einer invertierter Darstellung erstellter Arch profitiert von einer gleich hohen Verteilung des Drucks von der Gewichtung eines Bogens. Die-Kategorie hat eine scheinbar einfache mathematische Beschreibung:

`y = a · cosh(x / a)`

Das *cosh* ist die hyperbolische Kosinus-Funktion. Für *x* gleich 0 (null) ist *cosh* NULL und *y* gleich *a*. Das ist der Mittelpunkt der Kategorie. Wie die *Kosinus* -Funktion wird auch *cosh* als *gerade*bezeichnet, d. h., Cosh *(– x)* entspricht *cosh (x)*, und die Werte erhöhen sich, um positive oder negative Argumente zu erhöhen. Diese Werte beschreiben die Kurven, die die Seiten der Kategorie bilden.

Es ist keine direkte Berechnung, dass der richtige Wert von *a* ist, um den Speicher für die Dimensionen der Telefon Seite anzupassen. Wenn *w* und *h* die Breite und Höhe eines Rechtecks sind, entspricht der optimale Wert *von der folgenden* Gleichung:

`cosh(w / 2 / a) = 1 + h / a`

Die folgende Methode in der [`LinkedChainPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) -Klasse schließt diese Gleichheit ein, indem Sie sich auf der linken und rechten Seite des Gleichheitszeichens als und auf die beiden Ausdrücke bezieht `left` `right` . Bei kleinen *Werten eines* `left` ist größer als `right` . bei großen Werten eines *a* `left` ist kleiner als `right` . Die- `while` Schleife schränkt den optimalen Wert eines *ein*:

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

Das `SKPath` -Objekt für die Verknüpfungen wird im Konstruktor der Klasse erstellt, und das resultierende `SKPathEffect` Objekt wird dann auf die-Eigenschaft des-Objekts festgelegt, `PathEffect` `SKPaint` das als Feld gespeichert wird:

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

Die Hauptaufgabe des `PaintSurface` Handlers besteht darin, einen Pfad für den Eintrag selbst zu erstellen. Nachdem Sie die optimale *a* festgelegt und in der `optA` Variablen gespeichert haben, muss Sie auch einen Offset vom oberen Rand des Fensters berechnen. Anschließend kann er eine `SKPoint` Auflistung von Werten für den Eintrag sammeln, ihn in einen Pfad umwandeln und den Pfad mit dem zuvor erstellten `SKPaint` Objekt zeichnen:

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

Dieses Programm definiert den Pfad, der in verwendet wird `Create1DPath` , um seinen (0,0) Punkt in der Mitte zu haben. Dies ist sinnvoll, da der (0,0) Punkt des Pfads an der Linie oder Kurve ausgerichtet ist, die er hat. Sie können jedoch einen nicht zentrierten (0,0) Punkt für einige besondere Effekte verwenden.

Auf der Seite " **Fließband** " wird ein Pfad mit einem Oblong-Kreuz Band mit einem gekrümmten oberen und unteren Rand erstellt, der den Abmessungen des Fensters entspricht. Dieser Pfad ist mit einem einfachen `SKPaint` Objekt, das 20 Pixel breit und grau gefärbt ist, und dann wieder mit einem anderen Objekt mit einem Objekt, das auf einen Pfad verweist, der `SKPaint` `SKPathEffect` einem kleinen Bucket ähnelt:

[![Dreifacher Screenshot der Seite "Fließband"](effects-images/conveyorbelt-small.png)](effects-images/conveyorbelt-large.png#lightbox)

Der (0,0) Punkt des bucketpfads ist das handle. Wenn also das `phase` Argument animiert ist, scheinen die bucketbänder um das Band herum herum gedreht zu werden. vielleicht werden Sie im unteren Bereich auf Wasser und im oberen Bereich ausgecheckt.

Die [`ConveyorBeltPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) -Klasse implementiert Animationen mit über schreibungen der `OnAppearing` -Methode und der- `OnDisappearing` Methode. Der Pfad für den Bucket wird im Konstruktor der Seite definiert:

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

Der Bucket-Erstellungs Code wird mit zwei Transformationen abgeschlossen, die den Bucket etwas vergrößern und ihn seitwärts drehen. Das Anwenden dieser Transformationen war einfacher als das Anpassen aller Koordinaten im vorherigen Code.

Der `PaintSurface` Handler definiert zunächst einen Pfad für das-Transportband. Dabei handelt es sich einfach um ein Zeilen paar und ein paar von semikreisen, die mit einer dunkelgrauen Linie mit 20 Pixel Breite gezeichnet werden:

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect =
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase,
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

Die Logik zum Zeichnen des Kreuzbandes funktioniert nicht im Querformat.

Die bucketbereiche müssen ungefähr 200 Pixel voneinander entfernt sein. Allerdings ist das Förderband wahrscheinlich nicht ein Vielfaches von 200 Pixeln, d. h., da das `phase` -Argument von `SKPathEffect.Create1DPath` animiert wird, werden die bucketzeichen in das vorhanden sein und das vorhanden sein.

Aus diesem Grund berechnet das Programm zuerst einen Wert `length` mit dem Namen, der die Länge des Kreuzbandes ist. Da das Kreuz Band aus geraden Linien und semikreisen besteht, ist dies eine einfache Berechnung. Im nächsten Schritt wird die Anzahl der Bucket durch die Division `length` durch 200 berechnet. Diese wird auf die nächste ganze Zahl gerundet, und diese Zahl wird dann in aufgeteilt `length` . Das Ergebnis ist ein Abstand für eine ganzzahlige Anzahl von Buchern. Das- `phase` Argument ist einfach ein Bruchteil davon.

## <a name="from-path-to-path-again"></a>Erneut von Pfad zu Pfad

Kommentieren Sie im unteren Bereich des `DrawSurface` Handlers den-Befehl aus **Conveyor Belt**, `canvas.DrawPath` und ersetzen Sie ihn durch den folgenden Code:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Wie im vorherigen Beispiel von `GetFillPath` sehen Sie, dass die Ergebnisse mit Ausnahme der Farbe identisch sind. Nach `GetFillPath` der Ausführung `newPath` von enthält das-Objekt mehrere Kopien des Bucket-Pfads, die jeweils an derselben Stelle positioniert sind, an der die Animation Sie zum Zeitpunkt des Aufrufens positioniert hat.

## <a name="hatching-an-area"></a>Schraffur eines Bereichs

Die- [`SKPathEffect.Create2DLines`](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix)) Methode füllt einen Bereich mit parallelen Linien, häufig als *Schraffurlinien*bezeichnet. Die-Methode weist die folgende Syntax auf:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

Das- `width` Argument gibt die Strichbreite der schraffurzeilen an. Der `matrix` -Parameter ist eine Kombination aus Skalierung und optionaler Drehung. Der Skalierungsfaktor gibt das Pixel Inkrement an, das Skia zum Leerraum der Schraffurlinien verwendet. Die Trennung zwischen den Zeilen ist der Skalierungsfaktor abzüglich des `width` Arguments. Wenn der Skalierungsfaktor kleiner als oder gleich dem `width` Wert ist, gibt es zwischen den Schraffurlinien keinen Leerraum, und der Bereich wird scheinbar ausgefüllt. Geben Sie den gleichen Wert für die horizontale und vertikale Skalierung an.

Standardmäßig sind die Schraffurlinien horizontal. Wenn der- `matrix` Parameter Drehung enthält, werden die Schraffurlinien im Uhrzeigersinn gedreht.

Die Füll Seite für die **Schraffur** zeigt diesen Pfad Effekt an. Die- [`HatchFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Klasse definiert drei Pfad Effekte als Felder, die erste für horizontale Schraffurlinien mit einer Breite von 3 Pixeln mit einem Skalierungsfaktor, der angibt, dass Sie 6 Pixel voneinander entfernt sind. Die Trennung zwischen den Zeilen besteht daher aus drei Pixeln. Der zweite Pfad Effekt bezieht sich auf vertikale Schraffurlinien mit einer Breite von sechs Pixel, die 24 Pixel voneinander entfernt sind (sodass die Trennung 18 Pixel beträgt), und die dritte ist für Diagonal schraffurzeilen mit einer Breite von 12 Pixel Breite von 36 Pixel.

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6,
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12,
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Beachten Sie die Matrix `Multiply` Methode. Da die horizontalen und vertikalen Skalierungsfaktoren identisch sind, ist die Reihenfolge, in der die Skalierungs-und Rotations Matrizen multipliziert werden, unerheblich.

Der `PaintSurface` Handler verwendet diese drei Pfad Effekte mit drei unterschiedlichen Farben in Kombination mit `fillPaint` , um ein abgerundetes Rechteck auf die Seite anzupassen. Die- `Style` Eigenschaft, die für festgelegt `fillPaint` ist, wird ignoriert; wenn das- `SKPaint` Objekt einen Pfad Effekt enthält `SKPathEffect.Create2DLine` , der von erstellt wurde, wird der Bereich unabhängig von

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

Wenn Sie die Ergebnisse sorgfältig betrachten, werden Sie feststellen, dass die roten und blauen Schraffurlinien nicht exakt auf das abgerundete Rechteck beschränkt sind. (Dies ist offensichtlich ein Merkmal des zugrunde liegenden Skia-Codes.) Wenn dies nicht zufriedenstellend ist, wird ein alternativer Ansatz für die diagonal Schraffurlinien in grün angezeigt: das abgerundete Rechteck wird als clippingpfad verwendet, und die Schraffurlinien werden auf der gesamten Seite gezeichnet.

Der `PaintSurface` Handler endet mit einem-Befehl, um einfach das abgerundete Rechteck zu zeichnen, sodass Sie die Diskrepanz mit den roten und blauen schraffurzeilen sehen können:

[![Dreifacher Screenshot der Füllseite für die Schraffur](effects-images/hatchfill-small.png)](effects-images/hatchfill-large.png#lightbox)

Der Android-Bildschirm sieht nicht so aus: die Skalierung des Screenshots hat bewirkt, dass die schlanken roten Linien und die dünnen Leerzeichen in scheinbar größere rote Linien und größere Leerräume konsolidiert werden.

## <a name="filling-with-a-path"></a>Ausfüllen mit einem Pfad

[`SKPathEffect.Create2DPath`](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath))Mit dem können Sie einen Bereich mit einem Pfad ausfüllen, der horizontal und vertikal repliziert wird, wobei der Bereich tatsächlich gezittet wird:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

Die `SKMatrix` Skalierungsfaktoren geben den horizontalen und vertikalen Abstand des replizierten Pfads an. Sie können den Pfad jedoch nicht mithilfe dieses `matrix` Arguments drehen. Wenn Sie möchten, dass der Pfad gedreht wird, drehen Sie den Pfad selbst mithilfe der Methode, die `Transform` von definiert wird `SKPath` .

Der replizierte Pfad wird normalerweise am linken und oberen Rand des Bildschirms ausgerichtet, nicht an dem Bereich, der ausgefüllt wird. Sie können dieses Verhalten überschreiben, indem Sie Übersetzungs Faktoren zwischen 0 und den Skalierungsfaktoren festlegen, um horizontale und vertikale Offsets von der linken und der oberen Seite anzugeben.

Die Füll Seite für die **Pfad Kachel** zeigt diesen Pfad Effekt. Der zum Durchsuchen des Bereichs verwendete Pfad wird als Feld in der- [`PathTileFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) Klasse definiert. Der horizontale und vertikale Koordinaten Bereich liegt zwischen – 40 und 40. Dies bedeutet, dass dieser Pfad 80 Pixel quadratisch ist:

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " +
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " +
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50),
                    100, 100, paint);
            }
        }
    }
}
```

Im `PaintSurface` -Handler legt der `SKPathEffect.Create2DPath` -Aufruf den horizontalen und vertikalen Abstand auf 64 fest, damit die Quadrat Kacheln der 80 Pixel überlappend sind. Glücklicherweise ähnelt der Pfad einem Rätsel, der sich gut mit benachbarten Kacheln vergrenzt:

[![Dreifacher Screenshot der Seite mit der Pfad Kachel Füllung](effects-images/pathtilefill-small.png)](effects-images/pathtilefill-large.png#lightbox)

Die Skalierung auf der Grundlage des ursprünglichen Screenshots führt zu einer gewissen Verzerrung, insbesondere auf dem Android-Bildschirm.

Beachten Sie, dass diese Kacheln immer als Ganzes angezeigt werden und nie abgeschnitten werden. Auf den ersten beiden Screenshots ist nicht einmal ersichtlich, dass der Bereich, der ausgefüllt wird, ein abgerundetes Rechteck ist. Wenn Sie diese Kacheln in einem bestimmten Bereich abschneiden möchten, verwenden Sie einen clippingpfad.

Legen Sie die `Style` -Eigenschaft des- `SKPaint` Objekts auf fest `Stroke` , und Sie sehen, dass die einzelnen Kacheln anstatt ausgefüllt angezeigt werden.

Es ist auch möglich, einen Bereich mit einer gekachelten Bitmap auszufüllen, wie im Artikel [**skiasharp-Bitmap-tiult**](../effects/shaders/bitmap-tiling.md)gezeigt.

## <a name="rounding-sharp-corners"></a>Runden von spitzen Ecken

Das **abgerundete heptagon** -Programm, das auf die [**drei Arten zum Zeichnen eines Bogen**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) Artikels gezeigt wurde, verwendete einen Tangenten Bogen, um die Punkte einer siebenseitigen Abbildung zu Kurven. Die **andere abgerundete heptagon** -Seite zeigt einen viel einfacheren Ansatz, bei dem ein von der-Methode erstellter Pfad Effekt verwendet wird [`SKPathEffect.CreateCorner`](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single)) :

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Obwohl das einzelne Argument den Namen `radius` hat, müssen Sie es auf die Hälfte des gewünschten Eckradius festlegen. (Dies ist ein Merkmal des zugrunde liegenden Skia-Codes.)

Hier ist der `PaintSurface` Handler in der- [`AnotherRoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) Klasse:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

Sie können diesen Effekt verwenden, wenn Sie auf Grundlage der-Eigenschaft des-Objekts ein-oder Auffüllen `Style` `SKPaint` . Hier wird ausgeführt:

[![Dreifacher Screenshot der weiteren gerundeten heptagon-Seite](effects-images/anotherroundedheptagon-small.png)](effects-images/anotherroundedheptagon-large.png#lightbox)

Sie werden feststellen, dass dieses abgerundete heptagon mit dem früheren Programm identisch ist. Wenn Sie mehr überzeugen müssen, dass der Eckradius tatsächlich 100 und nicht im 50 im-Befehl angegeben ist `SKPathEffect.CreateCorner` , können Sie die Auskommentierung der abschließenden Anweisung im Programm aufheben und einen 100-RADIUS-Kreis Rand in der Ecke sehen.

## <a name="random-jitter"></a>Zufälliger Jitter

Manchmal sind die fehlerfreien geraden Linien der Computergrafiken nicht ganz so, wie Sie möchten, und es ist ein wenig Zufälligkeit erwünscht. In diesem Fall sollten Sie die- [`SKPathEffect.CreateDiscrete`](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32)) Methode ausprobieren:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Sie können diesen Pfad Effekt entweder zum übertragen oder Ausfüllen verwenden. Zeilen werden in verbundene Segmente aufgeteilt – die ungefähre Länge, die von – angegeben `segLength` und in verschiedene Richtungen erweitert wird. Der Umfang der Abweichung von der ursprünglichen Zeile wird durch angegeben `deviation` .

Das abschließende Argument ist ein Ausgangswert, der zum Generieren der für den Effekt verwendeten Pseudo Zufallssequenz verwendet wird. Der Jitter-Effekt sieht für verschiedene Kerne etwas anders aus. Das-Argument hat den Standardwert 0 (null), was bedeutet, dass der Effekt immer gleich ist, wenn Sie das Programm ausführen. Wenn Sie unterschiedliche Jitter wünschen, wenn der Bildschirm neu gezeichnet wird, können Sie den Ausgangswert auf die- `Millisecond` Eigenschaft eines- `DataTime.Now` Werts (z. b.) festlegen.

Die **Jitter-Experiment** Seite ermöglicht Ihnen das Experimentieren mit unterschiedlichen Werten beim Durchstreifen eines Rechtecks:

[![Dreifacher Screenshot der jitterexperiment-Seite](effects-images/jitterexperiment-small.png)](effects-images/jitterexperiment-large.png#lightbox)

Das Programm ist einfach. Die [**jitterexperimentpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) -Datei instanziiert zwei `Slider` -Elemente und ein-Element `SKCanvasView` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Der- `PaintSurface` Handler in der [**JitterExperimentPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) -Code Behind-Datei wird immer dann aufgerufen, wenn sich ein `Slider` Wert ändert. Er ruft `SKPathEffect.CreateDiscrete` die Verwendung der beiden `Slider` Werte auf und verwendet diese, um ein Rechteck zu zeichnen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Sie können diese Wirkung auch zum Auffüllen verwenden. in diesem Fall unterliegt der Umriss des gefüllten Bereichs diesen zufälligen Abweichungen. Auf der **Textseite Jitter** wird die Verwendung dieses Pfad Effekts zum Anzeigen von Text veranschaulicht. Der größte Teil des Codes im `PaintSurface` -Handler der [`JitterTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) -Klasse ist für die Größenanpassung und das Zentrieren des Texts vorgesehen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Hier wird Sie im Querformat ausgeführt:

[![Dreifacher Screenshot der jittertext-Seite](effects-images/jittertext-small.png)](effects-images/jittertext-large.png#lightbox)

## <a name="path-outlining"></a>Pfad Gliederung

Sie haben bereits zwei wenige Beispiele für die- [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) Methode von gesehen `SKPaint` , die zwei Versionen enthält:

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

Nur die ersten beiden Argumente sind erforderlich. Die-Methode greift auf den Pfad zu, auf den durch das- `src` Argument verwiesen wird, ändert die Pfaddaten auf der Grundlage der Stroke-Eigenschaften im `SKPaint` -Objekt (einschließlich der- `PathEffect` Eigenschaft) und schreibt dann die Ergebnisse in den `dst` Pfad. Der `resScale` -Parameter ermöglicht das Verringern der Genauigkeit, um einen kleineren Zielpfad zu erstellen, und das- `cullRect` Argument kann Kontur außerhalb eines Rechtecks eliminieren.

Eine grundlegende Verwendung dieser Methode beinhaltet keine Pfad Effekte: Wenn die `SKPaint` `Style` -Eigenschaft des-Objekts auf festgelegt ist `SKPaintStyle.Stroke` und *nicht* über Ihren `PathEffect` Satz verfügt, dann `GetFillPath` erstellt einen Pfad, der einen *Umriss* des Quell Pfads darstellt, als wäre er von den Paint-Eigenschaften gezeichnet worden.

Wenn der Pfad z. b. `src` ein einfacher Kreis von RADIUS 500 ist und das- `SKPaint` Objekt eine Strichbreite von 100 angibt, dann `dst` wird der Pfad zwei konzentrische Kreise, eine mit dem RADIUS 450 und die andere mit einem Radius von 550. Die-Methode wird aufgerufen, `GetFillPath` da das Ausfüllen dieses `dst` Pfades dem `src` Pfad entspricht. Sie können den Pfad jedoch auch mit einem Strich zeichnen `dst` , um die Pfad Gliederungen anzuzeigen.

Dies wird durch **tippen zum Gliedern des Pfads** veranschaulicht. `SKCanvasView`Und `TapGestureRecognizer` werden in der Datei " [**tapdeoutlinederpathpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) " instanziiert. Die Code-Behind-Datei [**TapToOutlineThePathPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) definiert drei `SKPaint` Objekte als Felder, zwei für das überragt mit der Strichbreite 100 und 20 und die dritte zum Ausfüllen:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

Wenn der Bildschirm nicht abgetippt wurde, `PaintSurface` verwendet der Handler das `blueFill` -Objekt und das `redThickStroke` Paint-Objekt, um einen Zirkel Pfad zu erzeugen:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) -
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

Der Kreis wird wie erwartet ausgefüllt und gestreichelt:

[![Dreifacher Screenshot der normalen Abzweigung zum Gliedern der Seite "Pfad"](effects-images/taptooutlinethepathnormal-small.png)](effects-images/taptooutlinethepathnormal-large.png#lightbox)

Wenn Sie auf den Bildschirm tippen, `outlineThePath` wird auf festgelegt `true` , und der- `PaintSurface` Handler erstellt ein neues `SKPath` -Objekt und verwendet dieses als Zielpfad in einem-Rückruf für `GetFillPath` das `redThickStroke` Paint-Objekt. Der Zielpfad wird dann ausgefüllt und mit Strichen `redThinStroke` , was Folgendes zur Folge hat:

[![Dreifacher Screenshot der beschriebenen Abzweigung zum Gliedern der Seite "Pfad"](effects-images/taptooutlinethepathoutlined-small.png)](effects-images/taptooutlinethepathoutlined-large.png#lightbox)

Die beiden roten Kreise geben eindeutig an, dass der ursprüngliche zirkuläre Pfad in zwei zirkuläre Konturen konvertiert wurde.

Diese Methode kann bei der Entwicklung von Pfaden, die für die-Methode verwendet werden, sehr nützlich sein `SKPathEffect.Create1DPath` . Die Pfade, die Sie in diesen Methoden angeben, werden immer ausgefüllt, wenn die Pfade repliziert werden. Wenn Sie nicht möchten, dass der gesamte Pfad ausgefüllt wird, müssen Sie die umrissen sorgfältig definieren.

Beispielsweise wurden im Beispiel für eine **verknüpfte Kette** die Verknüpfungen mit einer Reihe von vier Arcs definiert, von denen jedes Paar auf zwei Radien basiert, um den Bereich des zu füllenden Pfads darzustellen. Es ist möglich, den Code in der- `LinkedChainPage` Klasse so zu ersetzen, dass er etwas anders ist.

Zuerst möchten Sie die Konstante neu definieren `linkRadius` :

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

Der `linkPath` ist jetzt nur zwei Bögen, die auf diesem einzelnen RADIUS basieren, mit den gewünschten Start Winkeln und den Pfeil Winkeln:

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

Das- `outlinePath` Objekt ist dann der Empfänger der Kontur von, `linkPath` Wenn es mit den in angegebenen Eigenschaften gezeichnet wird `strokePaint` .

Ein weiteres Beispiel für die Verwendung dieser Technik wird als nächstes für den in einer Methode verwendeten Pfad angezeigt.

## <a name="combining-path-effects"></a>Kombinieren von Pfad Effekten

Die zwei abschließenden statischen Erstellungs Methoden von `SKPathEffect` sind [`SKPathEffect.CreateSum`](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) und [`SKPathEffect.CreateCompose`](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) :

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Beide Methoden kombinieren zwei Pfad Effekte, um einen zusammengesetzten Pfad Effekt zu erstellen. Mit der- `CreateSum` Methode wird ein Pfad Effekt erstellt, der den beiden angewendeten Pfad Effekten ähnlich ist, während `CreateCompose` einen Pfad Effekt ( `inner` ) anwendet und dann `outer` auf diese Anwendung anwendet.

Sie haben bereits gesehen, wie die- `GetFillPath` Methode von `SKPaint` einen Pfad in einen anderen Pfad konvertieren kann, der auf `SKPaint` Eigenschaften (einschließlich `PathEffect` ) basiert. Daher sollte es nicht *allzu* Geheimnis sein, wie ein `SKPaint` Objekt diesen Vorgang zweimal mit den beiden in der-Methode oder der-Methode angegebenen Pfad Effekten ausführen kann `CreateSum` `CreateCompose` .

Eine offensichtliche Verwendung von `CreateSum` besteht darin, ein `SKPaint` Objekt zu definieren, das einen Pfad mit einem Pfad Effekt füllt und den Pfad mit einem anderen Pfad Effekt streift. Dies wird im Beispiel " **Katzen in Frame** " veranschaulicht, das ein Array von Katzen innerhalb eines Frames mit Bildkanten anzeigt:

[![Dreifacher Screenshot der Katzen in Frame-Seite](effects-images/catsinframe-small.png)](effects-images/catsinframe-large.png#lightbox)

Die [`CatsInFramePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Klasse beginnt mit der Definition mehrerer Felder. Möglicherweise erkennen Sie das erste Feld der- [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) Klasse im Artikel " [**SVG Path Data**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) ". Der zweite Pfad basiert auf einer Linie und einem Bogen für das Scallop-Muster des Frames:

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath =
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath`Kann in der-Methode verwendet werden `SKPathEffect.Create2DPath` , wenn die- `SKPaint` Objekt `Style` Eigenschaft auf festgelegt ist `Stroke` . Wenn jedoch `catPath` direkt in diesem Programm verwendet wird, wird der gesamte Anfang der Cat gefüllt, und die whiskus sind nicht einmal sichtbar. (Testen!) Es ist erforderlich, den Umriss dieses Pfades zu erhalten und diesen Umriss in der- `SKPathEffect.Create2DPath` Methode zu verwenden.

Der Konstruktor führt diesen Auftrag aus. Zuerst werden zwei Transformationen auf angewendet, um `catPath` den (0,0) Punkt in den Mittelpunkt zu verschieben und ihn in der Größe zu skalieren. `GetFillPath`Ruft alle Konturen der Kontur in ab `outlinedCatPath` , und das Objekt wird im-Befehl verwendet `SKPathEffect.Create2DPath` . Die Skalierungsfaktoren im `SKMatrix` Wert sind etwas größer als die horizontale und vertikale Größe des Cat, um einen kleinen Puffer zwischen den Kacheln bereitzustellen, während die Übersetzungs Faktoren etwas empirisch abgeleitet wurden, sodass eine vollständige CAT in der oberen linken Ecke des Frames sichtbar ist:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect =
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

Der Konstruktor ruft dann `SKPathEffect.Create1DPath` für den aufgerufenen Frame auf. Beachten Sie, dass die Breite des Pfads 100 Pixel beträgt, der Fortschritt jedoch 75 Pixel beträgt, sodass der replizierte Pfad um den Frame überlappt wird. Die letzte Anweisung des Konstruktors ruft `SKPathEffect.CreateSum` auf, um die beiden Pfad Effekte zu kombinieren und das Ergebnis auf das-Objekt festzulegen `SKPaint` .

All diese Arbeit ermöglicht es `PaintSurface` , dass der Handler recht einfach ist. Es muss nur ein Rechteck definiert und mit gezeichnet werden `framePaint` :

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

Die Algorithmen hinter den Pfad Effekten bewirken immer, dass der gesamte Pfad zum Durchsuchen oder Ausfüllen von angezeigt wird, was dazu führen kann, dass einige visuelle Elemente außerhalb des Rechtecks angezeigt werden. Der- `ClipRect` Aufrufe vor dem-Befehl `DrawRect` ermöglicht es, dass die visuellen Elemente erheblich bereinigt werden. (Probieren Sie es ohne Clipping aus!)

Es wird häufig verwendet, `SKPathEffect.CreateCompose` um einem anderen Pfad Effekt eine Jitter hinzuzufügen. Sie können natürlich selbst experimentieren, aber hier ist ein etwas anderes Beispiel:

Die **gestrichelten Schraffurlinien** füllen eine Ellipse mit gestrichelten Schraffurlinien aus. Die meisten Aufgaben in der- [`DashedHatchLinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) Klasse werden direkt in den Feld Definitionen ausgeführt. Diese Felder definieren einen Bindestrich Effekt und einen schraffureffekt. Sie sind so definiert, `static` dass in einem-Befehl in der-Definition auf Sie verwiesen wird `SKPathEffect.CreateCompose` `SKPaint` :

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect =
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60),
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Der `PaintSurface` Handler muss nur den Standard Mehraufwand und einen-Rückruf enthalten `DrawOval` :

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2,
                        0.45f * info.Width, 0.45f * info.Height,
                        paint);
    }
    ...
}
```

Wie Sie bereits entdeckt haben, sind die Schraffurlinien nicht genau auf das Innere des Bereichs beschränkt, und in diesem Beispiel beginnen Sie immer auf der linken Seite mit einem ganzen Bindestrich:

[![Dreifacher Screenshot der schraffurschraffurseite](effects-images/dashedhatchlines-small.png)](effects-images/dashedhatchlines-large.png#lightbox)

Nun, da Sie die Auswirkungen von Pfaden auf einfache Punkte und Bindestriche auf seltsame Kombinationen haben, können Sie Ihre Phantasie nutzen und sehen, was Sie erstellen können.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
