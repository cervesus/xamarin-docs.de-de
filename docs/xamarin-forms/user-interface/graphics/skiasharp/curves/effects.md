---
title: Pfad Effekte
description: Ermitteln der verschiedenen Pfad Auswirkungen, mit die Pfade für Kontur zuweisen und ausfüllen verwendet werden können
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 4097aea4079555b26b586db5ec63fa261d5e7946
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="path-effects"></a>Pfad Effekte

_Ermitteln der verschiedenen Pfad Auswirkungen, mit die Pfade für Kontur zuweisen und ausfüllen verwendet werden können_

Ein *Pfad Auswirkungen* ist eine Instanz der [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) -Klasse, die mit einer der acht statischen erstellt wird `Create` Methoden. Die `SKPathEffect` Objekt legen Sie dann auf die [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) Eigenschaft ein `SKPaint` Objekt für eine Vielzahl von interessante Effekte Kontur eine Zeile mit einem kleinen replizierten Pfad zuweisen:

![](effects-images/patheffectsample.png "Das verknüpfte Kette-Beispiel")

Pfad Effekte können Sie:

- Zeichnen Sie eine Zeile mit Punkte und Bindestriche enthalten
- Zeichnen Sie eine Zeile mit jeder gefüllte Pfad
- Füllen Sie einen Bereich mit schraffierte Linien
- Füllen Sie einen Bereich mit einer unterteilten Pfad
- Stellen Sie spitze Ecken gerundet
- Hinzufügen von zufälligen "jitter" Linien und Kurven

Darüber hinaus können Sie zwei oder mehr Pfad Effekte kombinieren.

In diesem Artikel auch veranschaulicht, wie die `GetFillPath` Methode `SKPaint` auf einen Pfad in einen anderen Pfad zu konvertieren, indem Sie Eigenschaften anwenden `SKPaint`, einschließlich `StrokeWidth` und `PathEffect`. Dies führt dazu, dass einige interessanten Techniken, wie das Abrufen von einem Pfad, der einen Überblick über einen anderen Pfad ist. `GetFillPath` ist auch hilfreich, im Zusammenhang mit Pfad Auswirkungen.

## <a name="dots-and-dashes"></a>Punkte und Bindestriche enthalten

Die Verwendung der [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) Methode wurde im Artikel beschriebenen [ **Punkte und Bindestriche**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Das erste Argument der Methode ist ein Array, eine gerade Anzahl von zwei oder mehr Werte, die abwechselnd Längen der Striche und Längen von Lücken zwischen der Bindestriche enthält:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Diese Werte sind *nicht* relativ zu die Konturbreite. Wenn die Strichbreite 10 ist, und Sie eine Linie quadratische Striche und Lücken quadratische besteht möchten, z. B. Festlegen der `intervals` array in {10, 10}. Die `phase` Argument gibt an, wo innerhalb des Strichmusters Zeile beginnt. Wenn Sie die Zeile mit dem die quadratische Lücke begonnen werden soll, legen Sie in diesem Beispiel `phase` auf 10.

Die Enden der betroffen sind der `StrokeCap` Eigenschaft `SKPaint`. Für Breite Strich Breiten, es ist üblich, zum Festlegen dieser Eigenschaft `SKStrokeCap.Round` gerundet wird, am Ende der Bindestriche. In diesem Fall werden die Werte in der `intervals` Array stimmen *nicht* enthalten die zusätzliche Länge durch die Rundung Dies bedeutet, dass ein zirkulärer Punkt erfordert die Angabe einer Breite von 0 (null). Verwenden Sie für eine Strichbreite von 10, zum Erstellen einer Zeile mit zirkuläre Punkte und Lücken zwischen den Punkten von der gleichen Durchmesser ein `intervals` Array von {0, 20}.

Die **gepunktet Text animiert** Seite ähnelt der **beschriebenen Text** Seite, die im Artikel beschriebenen [ **Integrieren von Text und Grafiken** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) in Es zeigt beschriebenen Textzeichen durch Festlegen der `Style` Eigenschaft von der `SKPaint` -Objekt `SKPaintStyle.Stroke`. Darüber hinaus **gepunktet Text animiert** verwendet `SKPathEffect.CreateDash` erteilen kann dies eine punktierte Darstellung und das Programm auch eine Animation der `phase` Argument die `SKPathEffect.CreateDash` Methode, um die Punkte scheinen Reisen den Textqualifizierer eingeschlossen, Zeichen. So sieht die Seite im Querformat:

[![](effects-images/animateddottedtext-small.png "Dreifacher Screenshot der Seite gepunktet Text animiert")](effects-images/animateddottedtext-large.png#lightbox "dreifacher Screenshot der Seite animiert gepunktet Text")

Die [ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasse zunächst werden einige Konstanten definieren, und überschreibt auch die `OnAppearing` und `OnDisappearing` Methoden für die Animation:

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

Die `PaintSurface` Handler beginnt mit der Erstellung einer `SKPaint` Objekt, um den Text anzuzeigen. Die `TextSize` Eigenschaft wird basierend auf der Breite des Bildschirms angepasst:

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
            SKRect textBounds;
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

Gegen Ende der Methode die `SKPathEffect.CreateDash` -Methode wird aufgerufen, wobei die `dashArray` definiert ist als ein Feld aus, und die animierte `phase` Wert. Die `SKPathEffect` Instanz wird festgelegt, um die `PathEffect` Eigenschaft der `SKPaint` Objekt, um den Text anzuzeigen.

Sie können alternativ Festlegen der `SKPathEffect` -Objekt an die `SKPaint` Objekt vor dem Messen des Texts und zentrieren sie auf der Seite. In diesem Fall jedoch der animierten Punkte und Bindestriche enthalten dazu führen, dass einige Variante in der Größe des gerenderten Texts und der Text etwas Vibrieren tendenziell. (Probieren Sie es.)

Sie werden bemerken, dass als die Textzeichen animierten Punkte Kreis vorhanden ein bestimmten Punkt im jede geschlossene Kurve ist, in dem die Punkte in und aus Vorhandensein pop scheinen. Dies ist, in dem der Pfad, der definiert, die Gliederung Zeichen beginnt und endet. Wenn die Pfadlänge kein ganzzahliges Vielfaches der Länge der Strichmuster (in diesem Fall 20 Pixel) ist, kann nur ein Teil des Musters am Ende des Pfads entsprechen.

Es ist möglich, passen Sie die Länge des Strichmuster an die Länge des Pfads an, aber, die erfordert, bestimmen die Länge des Pfads, eine Technik, die in einem zukünftigen Artikel behandelt.

Die **Punkt / Bindestrich Umwandlung** Programm erstellt eine Animation des eigentlichen Musters des Bindestrich, sodass Bindestriche scheint Teilen in Punkten, wieder zum Formular Bindestriche kombinieren:

[![](effects-images/dotdashmorph-small.png "Dreifacher Screenshot der Seite Punkt Dash Umwandlung")](effects-images/dotdashmorph-large.png#lightbox "dreifacher Screenshot der Seite Punkt-Dash-Umwandlung")

Die [ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) -Klasse überschreibt die `OnAppearing` und `OnDisappearing` Methoden ebenso wie das vorherige Programm hat, aber die Klasse definiert die `SKPaint` Objekt als Feld:

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

Die `PaintSurface` Handler erstellt einen elliptischen Pfad basierend auf der Größe der Seite und einen langen Codeabschnitt, der festlegt führt die `dashArray` und `phase` Variablen. Wie die animierte Variable `t` liegt zwischen 0 und 1, der `if` Blöcke zusammensetzen von diesem Zeitpunkt in vier Quartale und in jedem dieser Quartale `tsub` auch reicht von 0 auf 1. Ganz am Ende des Programms erstellt die `SKPathEffect` und legt es auf die `SKPaint` -Objekts zum Zeichnen.

## <a name="from-path-to-path"></a>Vom Pfad zum Pfad

Die [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Methode `SKPaint` Wandelt einen Pfad in einen anderen anhand der Einstellungen in der `SKPaint` Objekt. Um anzuzeigen, wie dies funktioniert, ersetzen die `canvas.DrawPath` in der vorherigen Programm mit den folgenden Code aufrufen:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

In diesem neuen Code die `GetFillPath` aufrufen, konvertiert der `ellipsePath` (Dies ist nur ein Oval) in `newPath`, die dann mit dem angezeigt wird `newPaint`. Die `newPaint` -Objekts wird mit allen Standardeinstellungen Eigenschaft mit dem Unterschied, dass die `Style` festgelegt wird basierend auf den booleschen Rückgabewert aus `GetFillPath`.

Die visuellen Elemente unterscheiden sich nur die Farbe, die festgelegt wird, in `ellipsePaint` , aber nicht `newPaint`. Anstatt die einfache Ellipse definiert, die `ellipsePath`, `newPath` enthält zahlreiche Pfad Konturen, die definieren, die Reihe von Punkte und Bindestriche enthalten. Dies ist das Ergebnis des Anwendens von verschiedenen Eigenschaften der `ellipsePaint` – `StrokeWidth`, `StrokeCap`, und `PathEffect` – auf `ellipsePath` und das Übertragen des resultierenden Pfads in `newPath`. Die `GetFillPath` Methodenrückgabe ein boolescher Wert, der angibt, und zwar unabhängig davon, ob der Zielpfad ist gefüllt werden soll; in diesem Beispiel ist des Rückgabewerts `true` zum Ausfüllen des Pfads.

Ändern Sie die `Style` festlegen in `newPaint` auf `SKPaintStyle.Stroke` und sehen Sie die einzelnen Pfad Konturen Linie als Trennzeichen ein Pixelbreite beschrieben.

## <a name="stroking-with-a-path"></a>Mit einem Pfad Kontur zuweisen

Die [ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Methode gleicht konzeptionell `SKPathEffect.CreateDash` mit dem Unterschied, dass Sie ein Muster der Striche und Lücken, anstatt einen Pfad angeben. Dieser Pfad wird mehrere Male auf Strich der Zeile oder Kurve repliziert.

Die Syntax lautet:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Achten Sie: Es wird eine Überladung der `Create1DPath` , die durch eine Enumeration-Argument des Typs definiert ist `SkPath1DPathEffect` mit einem kleingeschriebenen "k". Dieser Name ist ein Fehler, und daher, dass die Enumeration und Methode als veraltet markiert sind, aber es ist sehr einfach, für die als veraltet markierte Methode, die Teil des Codes werden und ist es schwierig, was genau finden Sie unter ist falsch.

Im Allgemeinen gilt: der Pfad, den Sie übergeben `Create1DPath` werden kleine und zentriert, um den Punkt (0, 0). Die `advance` Parameter gibt den Abstand zwischen den Rechenzentren Pfad an, wie der Pfad auf die Zeile repliziert wird. Normalerweise legen Sie dieses Argument auf die ungefähre Breite der den Pfad fest. Die `phase` Argument spielt die gleiche Rolle als es führt Sie in der `CreateDash` Methode.

Die [ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) hat drei Member:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

Die `Translate` Member führt dazu, dass den Pfad zu der in der gleichen Ausrichtung zu bleiben, da es entlang einer Linie oder eines Kurve repliziert wird. Für `Rotate`, des Pfads gedreht wird basierend auf die ausgewählte Tangente der Kurve. Der Pfad weist die normale Ausrichtung für horizontale Linien. `Morph` ähnelt dem `Rotate` mit dem Unterschied, dass der Pfad selbst auch gekrümmt, entsprechend der Krümmung der Linie gezeichnet wird.

Die **1 D Pfad Auswirkungen** veranschaulicht diese drei Optionen. Die [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) -Datei definiert eine Auswahl mit drei Elementen, die drei Member der Enumeration entspricht:

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
            <Picker.Items>
                <x:String>Translate</x:String>
                <x:String>Rotate</x:String>
                <x:String>Morph</x:String>
            </Picker.Items>
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

Die [ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) Code-Behind-Datei definiert drei `SKPathEffect` Objekte als Felder. Diese werden alle erstellt mit `SKPathEffect.Create1DPath` mit `SKPath` mit erstellte Objekte `SKPath.ParseSvgPathData`. Der erste ist ein einfaches Feld, das zweite ist eine Rautenform und der dritte ist ein Rechteck. Diese werden verwendet, die Stile für drei wirksam zu veranschaulichen:

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

            switch (effectStylePicker.Items[effectStylePicker.SelectedIndex])
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

Die `PaintSurface` Ereignishandler erstellt eine Bézier-Kurve, die Schleifen, um sich selbst und greift auf die Auswahl, um zu ermitteln, welche `PathEffect` sollte verwendet werden, um es mit Strichen zu zeichnen. Die drei Optionen – `Translate`, `Rotate`, und `Morph` – werden von links nach rechts angezeigt:

[![](effects-images/1dpatheffect-small.png "Dreifacher Screenshot der Seite Pfad Auswirkungen 1D")](effects-images/1dpatheffect-large.png#lightbox "dreifacher Screenshot der Seite Pfad Auswirkungen 1D")

Die im angegebenen Pfad die `SKPathEffect.Create1DPath` Methode immer gefüllt wird. Die im angegebenen Pfad die `DrawPath` Methode schraffiert ist immer, wenn die `SKPaint` Objekt hat seine `PathEffect` -Eigenschaft auf einen 1D Pfad Effekt festgelegt. Beachten Sie, dass die `pathPaint` Objekt hat keine `Style` Einstellung, die normalerweise standardmäßig `Fill`, jedoch der Pfad unabhängig davon schraffiert ist.

Das Feld verwendet wird, der `Translate` Beispiel beträgt 20 Pixel, die quadratische, und die `advance` -Argument 24 festgelegt wird. Dieser Unterschied bewirkt, dass eine Lücke zwischen den Feldern, wenn die Zeile ungefähr horizontalen oder vertikalen ist, aber die Felder etwas überlappen, wenn die Zeile diagonalen ist, da die diagonale des Felds 28,3 Pixel ist. 

Die Rautenform in die `Rotate` Beispiel ist auch 20 Pixel breit. Die `advance` auf 20 festgelegt ist, damit weiterhin die Punkte berühren, wie die Raute zusammen mit der Krümmung der Linie gedreht wird.

Das Rechteck in die `Morph` Beispiel ist 50 Pixel breit und eine `advance` von 55 festlegen, um eine kleine zeitliche Lücke zwischen den Rechtecken zu machen, wie sie auf der Bézier-Kurve verbogen ist.

Wenn die `advance` Arguments ist kleiner als die Größe des Pfads, und dann die replizierten Pfade können sich überschneiden. Dies kann dazu führen, dass einige interessante Effekte werden. Die **verknüpften Kette** Seite zeigt eine Reihe von überlappende Kreise, die anscheinend mit eine Kette verknüpfte ähneln denen in der unterschiedliche Form von einer Oberleitung hängt:

[![](effects-images/linkedchain-small.png "Dreifacher Screenshot der Seite verknüpften Kette")](effects-images/linkedchain-large.png#lightbox "dreifacher Screenshot der Seite verknüpften Kette")

Sehr nahe suchen, und Sie sehen, dass dies tatsächlich Kreise sind nicht. Jeder Link in der Kette ist zwei Bögen, Größe, und also sie für die Verbindung mit benachbarten Links scheint positioniert.

Eine Kette oder die Kabel uniform Gewichtung Verteilung hängt in Form einer Oberleitung. Ein Arch erstellt in Form einer invertierten Oberleitung profitiert von der eine gleichmäßige Verteilung der Druck von der die Gewichtung des ein Arch. Der Oberleitung hat eine scheinbar einfache mathematische Beschreibung:

y = eine · COSH(x / a)

Die *Cosh* ist die hyperbolische Kosinus-Funktion. Für *x* gleich 0, *Cosh* 0 (null) und *y* gleich *eine*. Dies ist das Zentrum des der Oberleitung. Wie die *Kosinus* Funktion *Cosh* gilt als *sogar*, was bedeutet, dass *cosh(–x)* gleich *cosh(x)*, und die Werte für die Erhöhung positiv oder negativ Argumente zu erhöhen. Diese Werte beschrieben, die Kurven, die die Seiten des der Oberleitung bilden.

Suchen den richtigen Wert der *eine* an der Oberleitung auf die Dimensionen der Seite "den Anschluss" ist keine direkte Berechnung. Wenn *w* und *h* sind die Breite und Höhe eines Rechtecks, den optimalen Wert für *eine* erfüllt die folgende Gleichung:

cosh(w / 2 / a) = 1 + h / a

Die folgende Methode in der [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) Klasse enthält, auf Gleichheit durch einen Verweis auf die zwei Ausdrücke auf der linken Seite und rechts neben dem Gleichheitszeichen als `left` und `right`. Bei kleinen Werten für *eine*, `left` ist größer als `right`; bei großen Werten für *eine*, `left` ist kleiner als `right`. Die `while` Schleife eingeschränkt wird, auf einen optimalen Wert von *eine*:

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

Die `SKPath` -Objekt für die Links im Konstruktor der Klasse und die resultierenden erstellt wird `SKPathEffect` Objekt legen Sie dann auf die `PathEffect` Eigenschaft von der `SKPaint` -Objekt, das als ein Feld gespeichert wird:

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

Die Hauptaufgabe des der `PaintSurface` Handler ist, einen Pfad für die Oberleitung selbst erstellen. Nach der Bestimmung des optimalen *eine* und speichern ihn in die `optA` -Variable verwenden, es muss außerdem einen Offset vom oberen Rand des Fensters zu berechnen. Anschließend können sie eine Auflistung von kumuliert werden `SKPoint` Werte für die Oberleitung umwandeln, die in einem Pfad, und zeichnen Sie den Pfad für das zuvor erstellte `SKPaint` Objekt:

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

Dieses Programm definiert, den im verwendete Pfad `Create1DPath` haben seine (0, 0) in der Mitte zeigen. Dies scheint sinnvoll da der (0, 0) des Pfads ausgerichtet ist die Zeile oder die Kurve, die es hinzugefügt wird. Sie können jedoch eine nicht zentriert (0, 0) für einige Spezialeffekte zeigen.

Die **Förderband** Seite erstellt einen Pfad, der aussieht wie ein längliche Förderband transportiert die mit einem gekrümmten nach oben und unten wird dies auf die Dimensionen des Fensters angepasst. Dass der Pfad mit einem einfachen schraffiert ist `SKPaint` 20 Pixel breit und Grau, und klicken Sie dann erneut mit einem anderen gezeichnet `SKPaint` -Objekt mit einem `SKPathEffect` Objekt auf einen Pfad ähnelt ein wenig Bucket verweisen:

[![](effects-images/conveyorbelt-small.png "Dreifacher Screenshot der Seite "Fließband"")](effects-images/conveyorbelt-large.png#lightbox "dreifacher Screenshot der Seite "Fließband"")

Der (0, 0) des Pfads Bucket ist das Handle, das dies der Fall bei der `phase` Argument animiert wird, scheinen die Buckets um Förderband, vielleicht scooping oben Wasser am unteren und Dump-Sicherungen es aus der oben drehen.

Die [ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Klasse implementiert, überschreibt der Animation dem `OnAppearing` und `OnDisappearing` Methoden. Der Pfad für den Bucket wird im Konstruktor der Seite definiert:

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

Der Bucket Erstellung Code wird mit zwei Transformationen, die den Bucket etwas größeres stellen, und schalten Sie ihn seitwärts abgeschlossen. Diese Transformationen anwenden ist einfacher als die Koordinaten für die im vorherigen Code anpassen. 

Die `PaintSurface` Handler beginnt, indem Sie einen Pfad für die Förderband selbst definieren. Dies ist einfach ein paar Zeilen und zwei Semikolons Kreise, die mit 20 Pixel breit dunkelgrauen Linie gezeichnet werden:

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

Die Logik für das Zeichnen der Förderband funktioniert nicht im Querformat.

Die Buckets sollte ungefähr 200 Pixel auf Förderband auseinander angeordnet. Förderband ist jedoch wahrscheinlich kein Vielfaches von 200 Pixel long, d. h. als die `phase` Argument `SKPathEffect.Create1DPath` ist animierenden werden Buckets in und aus dem Vorhandensein angezeigt. 

Aus diesem Grund die Anwendung zuerst berechnet einen Wert namens `length` also die Länge des Förderband. Weil gerade Linien und Semikolon Kreise Förderband besteht, ist dies eine einfache Weise berechnen. Als Nächstes wird die Anzahl der Buckets berechnet, indem die Division `length` von 200. Dies wird auf die nächste Ganzzahl gerundet und aufgeteilt, dass Zahl, klicken Sie dann ist `length`. Das Ergebnis ist ein Abstand für eine ganze Zahl der Buckets an. Die `phase` Argument ist nur ein Bruchteil der.

## <a name="from-path-to-path-again"></a>Vom Pfad zum Pfad erneut

Am unteren Rand der `DrawSurface` Ereignishandler in **Förderband**, kommentieren Sie Sie aus der `canvas.DrawPath` aufrufen, und Ersetzen Sie ihn durch den folgenden Code:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Wie im vorherigen Beispiel der `GetFillPath`, sehen Sie, dass die Ergebnisse mit Ausnahme der Farbe identisch sind. Nach der Ausführung `GetFillPath`die `newPath` -Objekt enthält mehrere Kopien des Pfads Bucket, jeweils positioniert, in der gleichen erkennen, dass die Animation zum Zeitpunkt des Aufrufs positioniert.

## <a name="hatching-an-area"></a>Schraffurmuster, einen Bereich

Die [ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Methode füllt einen Bereich mit parallelen Linien, die häufig aufgerufen *Schraffieren Zeilen*. Die Methode hat die folgende Syntax:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

Die `width` -Argument gibt die Strichbreite Schraffur Zeilen. Die `matrix` Parameter ist eine Kombination von skalierungs- und optionale Drehung. Der Skalierungsfaktor gibt das Skia verwendet wird, um Speicherplatz, der die Zeilen Schraffur Pixel-Inkrement an. Die Trennung zwischen den Zeilen ist der Skalierungsfaktor minus der `width` Argument. Wenn der Skalierungsfaktor kleiner als oder gleich ist der `width` Wert, zwischen den Zeilen Schraffur wird kein Speicherplatz vorhanden sein, und der Bereich erscheint gefüllt werden soll. Geben Sie den Wert für die horizontale und vertikale Skalierung. 

Standardmäßig sind schraffierte Linien horizontal. Wenn die `matrix` Parameter enthält, Drehung, Schraffur Zeilen werden im Uhrzeigersinn gedreht.

Die **Schraffur füllen** Seite zeigt die Auswirkung dieser Pfad. Die [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Klasse definiert drei Pfad Effekte als Felder, das erste für horizontale schraffierte Linien mit einer Breite von 3 Pixel mit einer Skalierung Faktor, der angibt, deren Abstand voneinander 6 Pixel. Die Trennung zwischen den Zeilen ist daher 3 Pixel. Das zweite Pfad wird für 24 Pixel auseinander (also die Trennung ist 18 Pixel), die gesamte Fläche vertikal schraffierte Linien mit einer Breite von 6 Pixel und die dritte für diagonale schraffierte 12 Pixel breit gleichmäßigen Abständen 36 Pixel auseinander Zeilen. 

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
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Beachten Sie die Matrix `Multiply` Methode. Da die horizontalen und vertikalen Skalierungsfaktoren identisch sind, ist die Reihenfolge, in der die Skalierung und Drehung Matrizen multipliziert sind, unerheblich.

Die `PaintSurface` Handler verwendet diese drei Pfad Effekte mit drei verschiedenen Farben in Kombination mit `fillPaint` ein abgerundetes Rechteck, das die Seite angepasst zu füllen. Die `Style` festgelegte Eigenschaft `fillPaint` ignoriert wird, wenn die `SKPaint` Objekt enthält einen Pfad Effekt aus erstellt `SKPathEffect.Create2DLine`, Bereich gefüllt wird, unabhängig davon:

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

Wenn Sie sorgfältig die Ergebnisse betrachten, sehen Sie sich, dass das rote und blaue schraffierte Linien genau auf des abgerundeten Rechtecks beschränkt werden nicht. (Dies ist anscheinend ein Merkmal des zugrunde liegenden Codes Skia.) Wenn dies ungenügend ist, wird ein alternativer Ansatz für die diagonale schraffierte Linien in grün angezeigt: des abgerundeten Rechtecks verwendet wird, als einen Freistellungspfad und schraffurfarbe Zeilen auf die gesamte Seite gezeichnet werden.

Die `PaintSurface` Handler endet mit einem Aufruf des abgerundeten Rechtecks einfach zu zeichnen, sodass Ihnen die Abweichung mit dem roten und blauen schraffierte Linien angezeigt:

[![](effects-images/hatchfill-small.png "Dreifacher Screenshot der Seite Schraffur füllen")](effects-images/hatchfill-large.png#lightbox "dreifacher Screenshot der Seite Schraffur ausfüllen")

Die Android Bildschirm nicht tatsächlich, aussehen: die Skalierung des Screenshots der schmale rote Linien und thin Leerzeichen in scheinbar breiter rote Linien konsolidiert und breiter Leerzeichen verursacht hat.

## <a name="filling-with-a-path"></a>Füllen mit einem Pfad

Die [ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Bereich faktisch Kacheln können Sie einen Bereich mit einem Pfad zu füllen, die horizontal oder vertikal repliziert wird:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

Die `SKMatrix` Skalierungsfaktoren anzugeben, die horizontale und vertikale Zwischenräume des replizierten Pfads. Jedoch können nicht gedreht werden die mit diesem Pfad `matrix` Argument; Sie des Pfads gedreht wird, drehen Sie den Pfad selbst mithilfe der `Transform` definierte Methode `SKPath`. 

Der Pfad des replizierten wird normalerweise mit der linken und oberen Rand der Benutzeranmeldebildschirm anstelle ausgefüllten ausgerichtet. Sie können dieses Verhalten überschreiben, durch die Bereitstellung der Übersetzung Faktoren zwischen 0 und der Skalierungsfaktoren horizontalen und vertikalen Offsets von der linken und oberen Seiten angeben.

Die **Pfad Kachel auszufüllen** Seite zeigt die Auswirkung dieser Pfad. Der Pfad zum Anordnen des Bereichs wird definiert als Feld in der [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) Klasse. Horizontalen und vertikalen Koordinaten Bereich von – 40 bis 40, was bedeutet, dass dieser Pfad 80 Pixel quadratische ist: 

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

In der `PaintSurface` Handler, der `SKPathEffect.Create2DPath` Aufrufe legt die horizontale und vertikale Zwischenräume 64, die dazu führen, dass die quadratischen 80-Pixel-Kacheln überlappen. Glücklicherweise ähnelt der Pfad ein Puzzle-Stück, Vernetzung ordentlich mit angrenzenden Kacheln:

[![](effects-images/pathtilefill-small.png "Dreifacher Screenshot der Seite Pfad Kachel auszufüllen")](effects-images/pathtilefill-large.png#lightbox "dreifacher Screenshot der Seite Pfad Kachel auszufüllen.")

Die Skalierung von der ursprünglichen Screenshot bewirkt, dass einige Verzerrung, insbesondere auf dem Android-Bildschirm.

Beachten Sie, dass diese Kacheln immer die gesamte angezeigt und werden nie abgeschnitten. Außer ist auf dem Bildschirm Windows 10 Mobile nicht selbst offensichtlich, dass der Bereich, das ausgefüllt wurde ein abgerundetes Rechteck ist. Wenn Sie diese Kacheln zu einem bestimmten Bereich abschneiden möchten, verwenden Sie einen Freistellungspfad.

Versuchen Sie die `Style` Eigenschaft der `SKPaint` -Objekt `Stroke`, und sehen Sie die einzelnen Kacheln beschriebenen statt gefüllt.

## <a name="rounding-sharp-corners"></a>Runden spitze Ecken

Die **Siebeneck gerundet** Programm angezeigt, der [ **drei Möglichkeiten, einen Bogen gezeichnet** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) Artikel verwendet einen Tangenten Bogen, um die Punkte einer Abbildung 7 doppelseitige Kurve. Die **eine andere gerundet Siebeneck** Seite zeigt einen viel einfacheren Ansatz, der einen Pfad Effekt erstellt, aus der [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) Methode:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Obwohl das einzelne Argument heißt `radius` müssen Sie es auf die Hälfte der gewünschten Eckradius festlegen. (Dies ist ein Merkmal des zugrunde liegenden Codes Skia.)

So sieht die `PaintSurface` Ereignishandler in der [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) Klasse:

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

Verwenden Sie diesen Effekt mit Verlauf oder füllen, die basierend auf der `Style` Eigenschaft von der `SKPaint` Objekt. Hier ist es auf allen drei Plattformen:

[![](effects-images/anotherroundedheptagon-small.png "Dreifacher Screenshot der Seite eine andere gerundet Siebeneck")](effects-images/anotherroundedheptagon-large.png#lightbox "dreifacher Screenshot der Seite "einen anderen gerundet Siebeneck"")

Sie sehen, dass diese abgerundeten Siebeneck an das frühere Programm identisch ist. Wenn Sie überzeugt Weitere, die die Eckradius ist tatsächlich 100 anstatt 50 angegeben wird, der `SKPathEffect.CreateCorner` Aufruf, Sie können kommentieren Sie die letzte Anweisung im Programm und ausführliche Informationen finden Sie eine 100-Radius-Kreis überlagert, in der Ecke klicken.

## <a name="random-jitter"></a>Zufällige Jitter

In einigen Fällen fehlerlos geraden Computer Grafiken sind nicht sehr, was Sie möchten, und ein wenig hinsichtlich Ihrer Zufälligkeit erwünscht ist. In diesem Fall sollten Sie versuchen die [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) Methode:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Dieser Pfad Effekt können für Kontur zuweisen oder füllen. Zeilen werden in verbundene Segmente getrennt – die geschätzte Zeitdauer gemäß `segLength` – und in verschiedene Richtungen erweitern. Das Ausmaß der Abweichung vom die ursprüngliche Zeile wird angegeben, indem `deviation`. 

Das letzte Argument handelt es sich um einen Ausgangswert verwendet, um die pseudozufällige Sequenz verwendet, um die Auswirkungen zu generieren. Der Effekt Jitter sieht für verschiedene Ausgangswerte etwas anders. Das Argument hat den Standardwert 0 (null), was bedeutet, dass die Auswirkungen identisch ist, wenn Sie das Programm ausführen. Wenn Sie unterschiedliche Jitter möchten, wenn der Bildschirm aktualisiert wird, können Sie den Ausgangswert festlegen, auf die `Millisecond` Eigenschaft ein `DataTime.Now` Wert (z. B.).


Die **Jitter experimentieren** Seite ermöglicht Ihnen das Experimentieren mit unterschiedlichen Werten im Verlauf eines Rechtecks:

[![](effects-images/jitterexperiment-small.png "Screenshot der Seite Jitter Experiment dreifach")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Das Programm ist Straightfoward. Die [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) Datei instanziiert zwei `Slider` Elemente und ein `SKCanvasView`:

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

Die `PaintSurface` Ereignishandler in der [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) Code-Behind-Datei wird aufgerufen, wenn eine `Slider` -Wert ändert. Sie ruft `SKPathEffect.CreateDiscrete` mit den beiden `Slider` Werte dann verwendet, um ein Rechteck mit Strichen zu zeichnen:

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

Dieser Effekt können zum Ausfüllen auch diese zufälligen abweichungen in diesem Fall die Kontur des gefärbten Bereich unterliegt. Die **Jitter Text** Seite veranschaulicht die Verwendung dieser Effekt Pfad anzuzeigende Text. Hauptteil des Codes in der `PaintSurface` Handler die [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) Klasse dient zum Ändern der Größe und zentrieren Text:

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
        SKRect textBounds;
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Hier wird es im Querformat auf allen drei Plattformen ausgeführt:

[![](effects-images/jittertext-small.png "Screenshot der Seite Text Jitter dreifach")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Gliedern von Pfad

Sie haben bereits gesehen, zwei wenige Beispiele für die [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Methode `SKPaint`, auch vorhanden ist ein [überladen](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Nur die ersten beiden Argumente erforderlich sind. Die Methode greift auf den Pfad verweist die `src` Argument, ändert die Pfaddaten, die basierend auf der Stricheigenschaften in der `SKPaint` Objekt (einschließlich der `PathEffect` Eigenschaft), und schreibt dann die Ergebnisse in die `dst` Pfad. Die `resScale` Parameter ermöglicht, reduzieren die Genauigkeit, um einen kleineren Zielpfad zu erstellen und die `cullRect` Argument Konturen außerhalb eines Rechtecks vermeiden kann.

Eine grundlegende Verwendung dieser Methode beinhaltet gar keine Auswirkungen Pfad. Wenn die `SKPaint` Objekt hat seine `Style` -Eigenschaftensatz auf `SKPaintStyle.Stroke`, an und führt *nicht* haben seine `PathEffect` festgelegt ist, `GetFillPath` erstellt einen Pfad ein, steht ein *Gliederung*von den Quellpfad, als wäre es durch die Paint-Eigenschaften gezeichnet wurde, hatte.

Z. B. wenn die `src` Pfad ist ein einfacher Kreis RADIUS 500 Personen durch, und die `SKPaint` Objekt gibt an, eine Strichbreite von 100, und klicken Sie dann die `dst` Pfad wird zwei konzentrische Kreise, eine mit einem Radius von 450 und der andere mit einem Radius von 550. Die Methode wird aufgerufen `GetFillPath` , da dies füllen `dst` Pfad entspricht dem Verlauf der `src` Pfad. Aber Sie können auch mit Strichen zu zeichnen die `dst` Pfad zu dem Pfad-Pfade finden Sie unter.

Die **Tippen Sie auf Gliederung der Pfad** wird dies veranschaulicht. Die `SKCanvasView` und `TapGestureRecognizer` instanziiert werden, der [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) Datei. Die [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) Code-Behind-Datei definiert drei `SKPaint` Objekte als Felder, zwei für Verlauf mit Strichen zu zeichnen Breiten von 100 20 und die dritte für ausfüllen:

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

Wenn der Bildschirm nicht abgerufen wurde, die `PaintSurface` Handler verwendet die `blueFill` und `redThickStroke` Zeichnen von Objekten zum Rendern eines kreisförmigen Pfads:

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

Der Kreis gefüllt und gezeichnet werden, wie zu erwarten:

[![](effects-images/taptooutlinethepathnormal-small.png "Dreifacher Screenshot der normalen tippen, Gliederung Pfad Seite")](effects-images/taptooutlinethepathnormal-large.png#lightbox "dreifacher Screenshot der normalen tippen, Gliederung der Path-Seite")

Tippen Sie auf dem Bildschirm `outlineThePath` auf festgelegt ist `true`, und die `PaintSurface` Ereignishandler erstellt ein neuer `SKPath` -Objekt und verwendet diesen als der Zielpfad in einem Aufruf von `GetFillPath` auf die `redThickStroke` Paint-Objekt. Dieser Zielpfad dann gefüllt und mit Gestrichelt `redThinStroke`, wodurch Folgendes:

[![](effects-images/taptooutlinethepathoutlined-small.png "Dreifacher Screenshot der grundsätzliche tippen, Gliederung Pfad Seite")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "dreifacher Screenshot der grundsätzliche tippen, Gliederung der Path-Seite")

Die zwei roten Kreise zeigen deutlich, dass der ursprüngliche Pfad für zirkuläre in zwei zirkuläre Konturen konvertiert wurde.

Diese Methode kann sehr hilfreich bei der Entwicklung zu verwendende für Pfade werden der `SKPathEffect.Create1DPath` Methode. Die angegebenen Pfade in diesen Methoden werden immer gefüllt, wenn die Pfade repliziert werden. Wenn Sie nicht möchten, dass den gesamten Pfad gefüllt werden soll, müssen Sie sorgfältig die Umrisse definieren.

Beispielsweise ist in der **verknüpften Kette** Beispiel definiert, die Links mit einer Reihe von vier Bögen, jedes Paar von denen wurden basierend auf zwei Radien kann den Bereich des Pfads gefüllt werden soll. Es ist möglich, ersetzen Sie den Code in der `LinkedChainPage` Klasse dafür etwas anders.

Zunächst sollten Sie neu zu definieren, die `linkRadius` Konstanten:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

Die `linkPath` ist jetzt nur zwei Bögen basierend auf dieser einzelnen Radius, mit der gewünschten Winkel starten und sweep Winkel:

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

Die `outlinePath` Objekt wird dann die Empfänger der Gliederung des `linkPath` Wenn es mit den Eigenschaften, die im angegebenen schraffiert ist `strokePaint`. 

Ein weiteres Beispiel, die auf diese Weise kommt weiter für den Pfad in verwendet eine `SKPathEffect.Create2DPath` Methoden. 

## <a name="combining-path-effects"></a>Kombination von Pfad Effekte

Die zwei letzten statischen Erstellungsmethoden von `SKPathEffect` sind [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) und [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Diese beiden Methoden kombinieren von zwei Pfad Effekte um einen zusammengesetzten Pfad erstellen. Die `CreateSum` Methode erstellt einen Pfad Effekt, der ähnlich wie die beiden Pfad Effekte separat angewendet während `CreateCompose` gilt ein Pfad Effekt (die `inner`) und wendet dann die `outer` mit.

Sie haben bereits gesehen, wie die `GetFillPath` Methode `SKPaint` können einen Pfad in einen anderen Pfad auf der Grundlage konvertieren `SKPaint` Eigenschaften (einschließlich `PathEffect`) Es ist also darf keine *zu* unverständliche, wie ein `SKPaint`Objekt kann diesen Vorgang ausführen, zweimal mit zwei Pfad Effekten angegeben, der `CreateSum` oder `CreateCompose` Methoden.

Eine offensichtliche Verwendung von `CreateSum` besteht im Definieren einer `SKPaint` Objekt, das füllt eines Pfads mit einem Pfad und den Pfad mit einem anderen Pfad Effekt Konturen. Dies wird dargestellt, der **Katzen im Frame** Beispiel, das ein Array von Katzen innerhalb eines Rahmens mit gewellte Kanten anzeigt:

[![](effects-images/catsinframe-small.png "Dreifacher Screenshot der Seite Katzen im Frame")](effects-images/catsinframe-large.png#lightbox "dreifacher Screenshot der Seite Katzen im Frame")

Die [ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Klasse beginnt, indem Sie mehrere Felder definieren. Sie möglicherweise erkennen, dass das erste Feld aus der [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) -Klasse aus den [ **SVG-Pfaddaten** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) Artikel. Der zweite Pfad basiert auf einer Zeile und Bogen nach dem Muster des Ausbuchten des Frames:

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

Die `catPath` verwendet werden, der `SKPathEffect.Create2DPath` Methode Wenn die `SKPaint` Objekt `Style` -Eigenschaftensatz auf `Stroke`. Jedoch, wenn die `catPath` verwendet wird, direkt an diesem Programm, klicken Sie dann den gesamten Anfang der Cat ausgefüllt, und Whisker nicht selbst sichtbar. (Probieren Sie es.) Es ist erforderlich, die Gliederung des Pfades zu erhalten und verwenden die Gliederung in der `SKPathEffect.Create2DPath` Methode.

Der Konstruktor übernimmt diese Aufgabe. Zuerst angewendet wird, zwei Transformationen `catPath` zum Verschieben der (0, 0) in die Mitte zeigen und Größe Herunterskalieren. `GetFillPath` Ruft alle Gliederungen der Konturen in `outlinedCatPath`, und dieses Objekt wird verwendet, der `SKPathEffect.Create2DPath` aufrufen. Die Skalierung Faktoren, die der `SKMatrix` Wert sind etwas größer als die horizontale und vertikale Größe des der Katze in bieten einen kleinen Puffer zwischen der Kacheln verläuft, während die Übersetzung Faktoren waren abgeleitet in einem gewissen empirisch so, dass eine vollständige Cat in angezeigt wird der linken oberen Ecke des Frames:

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

Der Konstruktor ruft dann `SKPathEffect.Create1DPath` für den gewellte Frame. Beachten Sie, dass die Breite des Pfads 100 Pixel beträgt, sondern der vorabversandmitteilung 75 Pixel, damit der replizierten Pfad überlappt wird, um den Rahmen aus. Der Konstruktor ruft die letzte Anweisung `SKPathEffect.CreateSum` die Effekte von zwei Pfaden kombinieren, und legen das Ergebnis auf der `SKPaint` Objekt.

Alle diese Aufgaben kann den `PaintSurface` Handler ganz einfach sein. Es muss nur definieren ein Rechteck, und zeichnen sie mit `framePaint`:

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

Die Algorithmen hinter die Pfad-Auswirkungen führen immer Kontur zuweisen oder füllen, die angezeigt werden, zum gesamten Pfad dadurch einige visuelle Elemente außerhalb des Rechtecks angezeigt werden können. Die `ClipRect` vor dem Aufrufen der `DrawRect` Aufruf gestattet die visuellen Elemente erheblich cleaner sein. (Probieren Sie es ohne Clipping.)

Es ist üblich, verwenden Sie `SKPathEffect.CreateCompose` einen anderen Pfad Effekt einige Jitter hinzu. Sie sicherlich selbst experimentieren können allerdings so sieht ein etwas anderes Beispiel:

Die **Strichlinien Schraffur** füllt eine Ellipse, die mit Schraffur Zeilen, die gestrichelte sind. Die meisten Aufgaben in der [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) Klasse erfolgt direkt in die Felddefinitionen. Diese Felder Definieren eines Effekts Dash und eine Schraffur wirksam. Sie definiert sind, als `static` , da sie dann in referenziert werden ein `SKPathEffect.CreateCompose` rufen Sie in der `SKPaint` Definition:

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
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Die `PaintSurface` Handler müssen nur die standard-Overhead plus zwei aufrufen enthalten `DrawOval`:

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

Als Sie bereits ermittelt haben, die schraffierte Linien sind nicht genau mit dem inneren Bereich beschränkt, und in diesem Beispiel immer beginnen auf der linken Seite mit einem ganzen Bindestrich:

[![](effects-images/dashedhatchlines-small.png "Dreifacher Screenshot der Seite gestrichelte Schraffur Linien")](effects-images/dashedhatchlines-large.png#lightbox "dreifacher Screenshot der Seite gestrichelte schraffierte Linien")

Nun, da Sie Auswirkungen Pfad zwischen einfachen Punkte und Bindestriche enthalten und ungewöhnliche Kombinationen gesehen haben, verwenden Sie Ihre Vorstellungskraft hinzugefügt, und finden Sie unter erstellen können.



## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
