---
title: SkiaSharp-Effekten Pfad
description: Dieser Artikel erläutert die unterschiedlichen SkiaSharp-Pfad-Effekte, mit denen Pfade für die Kontur zuweisen, und füllen Sie verwendet werden soll, und dies mit Beispielcode wird veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: davidbritch
ms.author: dabritch
ms.date: 07/29/2017
ms.openlocfilehash: e0af5188dd34e76b419b4cd5bf8d604fb059b7d3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642759"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp-Effekten Pfad

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Ermitteln Sie die verschiedenen pfadeffekte, mit die Pfade für die Kontur zuweisen, und füllen Sie verwendet werden können_

Ein *Pfad Auswirkungen* ist eine Instanz der [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) -Klasse, die mit einem der acht statischen Erstellungsmethoden, die von der Klasse definiert wurde. Die `SKPathEffect` Objekt legen Sie dann auf die [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) Eigenschaft eine [ `SKPaint` ](xref:SkiaSharp.SKPaint) Objekt für eine Vielzahl von interessante Effekte, z. B. Kontur Zuweisen einer Zeile mit einem kleinen replizierte Pfad :

![](effects-images/patheffectsample.png "Das verknüpfte Kette-Beispiel")

Pfadeffekte können Sie:

- Zeichnen Sie eine Zeile mit Punkte und Bindestriche enthalten
- Zeichnen Sie eine Zeile mit einen ausgefüllten Pfad
- Geben Sie einen Bereich mit schraffurlinien
- Geben Sie einen Bereich mit einem gekachelten Pfad
- Stellen Sie spitze Ecken gerundet
- Hinzufügen von zufälligen "jitter", Linien und Kurven

Darüber hinaus können Sie zwei oder mehr pfadeffekte kombinieren.

In diesem Artikel auch veranschaulicht, wie die [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath*) -Methode der `SKPaint` auf einen Pfad in einen anderen Pfad zu konvertieren, indem Sie die Anwendung von Eigenschaften des `SKPaint`, einschließlich `StrokeWidth` und `PathEffect`. Dies führt dazu, dass einige interessante Techniken, wie das Abrufen von einem Pfad, der einen Überblick über einen anderen Pfad ist. `GetFillPath` ist auch im Zusammenhang mit pfadeffekte hilfreich.

## <a name="dots-and-dashes"></a>Punkte und Striche

Die Verwendung der [ `PathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) Methode wurde in diesem Artikel beschriebenen [ **Punkte und Gedankenstriche**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Das erste Argument der Methode ist ein Array, das eine gerade Anzahl von zwei oder mehr Werte, die abwechselnd Längen der Striche und Längen der Lücken zwischen der Bindestriche enthält:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Diese Werte sind *nicht* Bezug auf die Strichbreite. Wenn die Strichbreite 10 ist, und eine Linie quadratischen Striche und quadratische Lücken bestehen möchten, z. B. Legen Sie die `intervals` array an {10, 10}. Die `phase` Argument gibt an, innerhalb des Strichmusters, an dem die Zeile beginnt. Wenn die Zeile mit dem die quadratischen Differenz begonnen werden soll, legen Sie in diesem Beispiel `phase` auf 10.

Die Enden der betroffen sind der `StrokeCap` Eigenschaft `SKPaint`. Für große Stroke Breiten, es kommt sehr häufig zum Festlegen dieser Eigenschaft auf `SKStrokeCap.Round` die Enden der gerundet. In diesem Fall die Werte in der `intervals` Array *nicht* enthalten die zusätzliche Länge durch die Rundung. Dies bedeutet, dass es sich bei ein zirkulärer Punkt erfordert die Angabe einer Breite von 0 (null). Verwenden Sie für eine Strichbreite von 10, um eine Zeile mit zirkuläre Punkte und Lücken zwischen den Rasterpunkten angibt, der den gleichen Durchmesser zu erstellen, eine `intervals` Array von {0, 20}.

Die **gepunktet Text animiert** Seite ähnelt der **von Text mit Kontur** Seite, die in diesem Artikel beschriebenen [ **die Integration von Text und Grafiken** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) in Es zeigt Textzeichen beschrieben, durch Festlegen der `Style` Eigenschaft der `SKPaint` -Objekt `SKPaintStyle.Stroke`. Darüber hinaus **gepunktet Text animiert** verwendet `SKPathEffect.CreateDash` gewähren, die diese sich einer gepunkteten Darstellung und das Programm animiert ebenfalls die `phase` Argument der `SKPathEffect.CreateDash` Methode, um die Punkte, um den Text zu folgen scheint, Zeichen. Hier ist die Seite im Querformat ein:

[![](effects-images/animateddottedtext-small.png "Dreifacher Screenshot der Seite gepunktet Text animiert")](effects-images/animateddottedtext-large.png#lightbox "dreifachen Screenshot der Seite animiert gepunktet Text")

Die [ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasse beginnt mit dem Definieren einiger Konstanten und überschreibt auch die `OnAppearing` und `OnDisappearing` Methoden für die Animation:

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

Die `PaintSurface` Handler beginnt mit der Erstellung einer `SKPaint` Objekt, um den Text anzuzeigen. Die `TextSize` -Eigenschaft wird basierend auf der Breite des Bildschirms angepasst:

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

Am Ende der Methode die `SKPathEffect.CreateDash` Methode wird aufgerufen, mit der `dashArray` , definiert ist, als ein Feld aus, und die animierte `phase` Wert. Die `SKPathEffect` Instanz wird festgelegt, um die `PathEffect` Eigenschaft der `SKPaint` Objekt, um den Text anzuzeigen.

Alternativ können Sie festlegen der `SKPathEffect` -Objekt an die `SKPaint` Objekt vor dem gemessen wird des Texts, und es auf der Seite zu zentrieren. In diesem Fall jedoch die animierte Punkte und Bindestriche enthalten dazu führen, dass einige Variante in der Größe des gerenderten Texts und der Text ist ein wenig Vibrieren. (Probieren Sie es!)

Sie werden bemerken, dass als den Kreis der animierten Punkte rund um die Textzeichen, ein bestimmten Punkt im jede geschlossene Kurve ist, in denen die Punkte scheint zu ein-und ausgehenden vorhanden ist. Dies ist, in dem der Pfad, der definiert, die Gliederung Zeichen beginnt und endet. Wenn die Länge des Pfads nicht ganzzahliges Vielfaches der die Länge des Strichmusters (in diesem Fall 20 Pixel) ist, kann nur einen Teil dieses Muster am Ende des Pfads entsprechen.

Es ist möglich, passen Sie die Länge des Strichmusters die Länge des Pfads anpassen, aber, die erfordert, bestimmen die Länge des Pfads, eine Technik, die in diesem Artikel abgedeckt werden [ **Pfadinformationen und-Enumeration** ](information.md).

Die **Punkt / Dash Morph** Programm erstellt eine Animation des Strichmusters selbst aus, sodass Bindestriche scheint in Punkte, unterteilen, die erneut zu Formular Bindestrichen kombinieren:

[![](effects-images/dotdashmorph-small.png "Dreifacher Screenshot der Seite Punkt Dash Morph")](effects-images/dotdashmorph-large.png#lightbox "dreifachen Screenshot der Seite Punkt Dash Morph")

Die [ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) -Klasse überschreibt die `OnAppearing` und `OnDisappearing` Methoden wie die oben stehenden Programms hat, aber die Klasse definiert die `SKPaint` Objekt als Feld:

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

Die `PaintSurface` Handler erstellt einen elliptischen Pfads basierend auf der Größe der Seite und einen langen Codeabschnitt, der festlegt führt die `dashArray` und `phase` Variablen. Wie die animierte Variable `t` liegt zwischen 0 und 1, die `if` Blöcke Aufteilen von diesem Zeitpunkt in vier Quartalen, und klicken Sie in jedem dieser Quartale `tsub` auch liegt zwischen 0 und 1. Am Ende, das Programm erstellt die `SKPathEffect` und legt es auf die `SKPaint` Objekt für das Zeichnen.

## <a name="from-path-to-path"></a>Vom Pfad zum Pfad

Die [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single)) -Methode der `SKPaint` wandelt Sie einen Pfad in ein anderes auf Grundlage der Einstellungen in der `SKPaint` Objekt. Um anzuzeigen, wie dies funktioniert, ersetzen die `canvas.DrawPath` rufen Sie in der oben stehenden Programms durch den folgenden Code:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

In diesem neuen Code wird die `GetFillPath` aufrufen konvertiert die `ellipsePath` (Dies ist nur eine Ellipse) in `newPath`, die wird dann angezeigt, mit `newPaint`. Die `newPaint` -Objekt wird erstellt, mit allen Standardeinstellungen-Eigenschaft mit dem Unterschied, dass die `Style` -Eigenschaftensatz basierend auf der boolesche Rückgabewert aus `GetFillPath`.

Die visuellen Elemente sind identisch, außer die Farbe, die festgelegt wird, im `ellipsePaint` , nicht jedoch `newPaint`. Anstatt die einfache Ellipse definiert, die `ellipsePath`, `newPath` enthält zahlreiche Pfad Konturen, die definieren, die Reihe von Punkte und Bindestriche enthalten. Dies ist das Ergebnis des Anwendens von verschiedenen Eigenschaften der `ellipsePaint` (insbesondere `StrokeWidth`, `StrokeCap`, und `PathEffect`) zu `ellipsePath` und die den resultierenden Pfad angegebene `newPath`. Die `GetFillPath` Methodenrückgabe ein boolescher Wert, der angibt, ob der angegebene Zielpfad ist gefüllt werden soll; in diesem Beispiel ist der Rückgabewert ist `true` für das Füllen des Pfads.

Versuchen Sie es ändern der `Style` festlegen in `newPaint` zu `SKPaintStyle.Stroke` sehen Sie die einzelnen Pfadkonturen, die durch eine Linie ein-Pixel-Width beschrieben.

## <a name="stroking-with-a-path"></a>Kontur mit einem Pfad zuweisen

Die [ `SKPathEffect.Create1DPath` ](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle)) Methode ist konzeptionell identisch mit `SKPathEffect.CreateDash` mit dem Unterschied, dass Sie einen Pfad und nicht als ein Muster von Strichen und Lücken angeben. Dieser Pfad wird mehrere Male auf Strich die Linie oder Kurve repliziert.

Die Syntax lautet:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

Im Allgemeinen gilt: der Pfad, der Sie übergeben `Create1DPath` werden kleine und zentriert, um den Punkt (0, 0). Die `advance` Parameter gibt den Abstand zwischen den Rechenzentren des Pfads an, wie Sie der Pfad in der Zeile repliziert werden. Sie haben dieses Argument in der Regel auf die ungefähre Breite der den Pfad festlegen. Die `phase` Argument spielt die gleiche Rolle wie führt Sie in der `CreateDash` Methode.

Die [ `SKPath1DPathEffectStyle` ](xref:SkiaSharp.SKPath1DPathEffectStyle) weist drei Member auf:

- `Translate`
- `Rotate`
- `Morph`

Die `Translate` Member führt dazu, dass den Pfad, in der gleichen Ausrichtung zu bleiben, während sie auf ein Linien- oder Kurvensegmente repliziert werden. Für `Rotate`, des Pfads gedreht wird basierend auf einer Tangente der Kurve. Der Pfad weist die normalen Ausrichtung für horizontale Linien an. `Morph` ist vergleichbar mit `Rotate` abgesehen davon, dass Sie der Pfad selbst auch gekrümmt ist entsprechend die Krümmung der Zeile wird mit Strichen gezeichnet wird.

Die **1 D Pfad Auswirkungen** Seite veranschaulicht diese drei Optionen. Die [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) -Datei definiert eine Auswahl, die drei Elemente entsprechen den drei Membern der Enumeration enthält:

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

Die [ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) Code-Behind-Datei definiert drei `SKPathEffect` Objekte als Felder. Diese werden alle mit erstellt `SKPathEffect.Create1DPath` mit `SKPath` mit erstellte Objekte `SKPath.ParseSvgPathData`. Die erste ist ein einfaches Feld, das zweite ist eine Raute, und die dritte ist ein Rechteck. Diese werden verwendet, um die Auswirkungen von drei Stile zu veranschaulichen:

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

Die `PaintSurface` Ereignishandler erstellt eine, die Schleife durchläuft, um sich selbst, und greift auf die Auswahl, um zu ermitteln, welche Bézierkurve `PathEffect` sollte verwendet werden, um es zu zeichnen. Die drei Optionen – `Translate`, `Rotate`, und `Morph` – werden von links nach rechts angezeigt:

[![](effects-images/1dpatheffect-small.png "Dreifacher Screenshot der Seite Pfad Auswirkungen 1D")](effects-images/1dpatheffect-large.png#lightbox "dreifachen Screenshot der Seite Pfad Auswirkungen 1D")

Die im angegebenen Pfad die `SKPathEffect.Create1DPath` Methode wird immer ausgefüllt. Den im angegebenen Pfad die `DrawPath` Methode schraffiert ist immer, wenn die `SKPaint` Objekt verfügt über seine `PathEffect` -Eigenschaft auf einen 1D Pfad-Effekt. Beachten Sie, dass die `pathPaint` Objekt hat keine `Style` Einstellung, die normalerweise standardmäßig `Fill`, aber der Pfad ist unabhängig davon, ob mit Strichen gezeichnet.

Das Feld in verwendet die `Translate` Beispiel ist 20 Pixel im Quadrat, und die `advance` Argument auf 24 festgelegt ist. Dieser Unterschied führt dazu, dass eine Lücke zwischen den Feldern, wenn die Zeile ungefähr horizontal oder vertikal ist, aber die Felder ein wenig bei die Zeile diagonale ist, da die diagonale des Felds 28,3 Pixel überlappt.

Die Raute, in der `Rotate` Beispiel ist auch 20 Pixel breit. Die `advance` auf 20 festgelegt ist, damit weiterhin die Punkte berühren, wie die Raute zusammen mit der Krümmung der Linie gedreht wird.

Der rechteckige Form in die `Morph` Beispiel ist 50 Pixel breit und eine `advance` von 55 festlegen, um eine kleine zeitliche Lücke zwischen den Rechtecken zu machen, wie sie rund um die Bézierkurve verbogen ist.

Wenn die `advance` -Argument ist kleiner als die Größe des Pfads, und klicken Sie dann die replizierten Pfade können sich überschneiden. Dies kann einige interessante Auswirkungen führen. Die **verknüpfte Kette** Seite zeigt eine Reihe von überlappende Kreise, die eine verknüpfte Kette, ähneln die hängt in der bestimmten Form von einem Oberleitung scheinen:

[![](effects-images/linkedchain-small.png "Dreifacher Screenshot der Seite verknüpften Kette")](effects-images/linkedchain-large.png#lightbox "dreifachen Screenshot der Seite verknüpften Kette")

Suchen Sie sehr nahe, und sehen Sie, dass dies tatsächlich Kreise sind nicht. Jeder Link in der Kette ist zwei Bögen, Größe und positioniert werden, damit sie für die Verbindung mit benachbarten Links scheinen.

Eine Kette oder Kabel des uniform gewichtsverteilung hängt in Form einer Oberleitung. Ein Arch erstellt in Form einer invertierten Oberleitung profitiert von der eine gleichmäßige Verteilung der Druck von das Gewicht ein Arch. Der Oberleitung verfügt über eine scheinbar einfache mathematische Beschreibung:

`y = a · cosh(x / a)`

Die *Cosh* ist die hyperbolische Kosinus-Funktion. Für *x* gleich 0 (null) *Cosh* ist 0 (null) und *y* gleich *eine*. Das ist der Mitte des der Oberleitung. Wie die *Kosinus* -Funktion *Cosh* gilt als *sogar*, was bedeutet, dass *cosh(–x)* gleich *cosh(x)* , und erhöhen Sie die Werte für die Steigerung der positiver oder negativer Arguments. Diese Werte beschreiben die Kurven, die die Seiten der Oberleitung bilden.

Suchen den richtigen Wert von *eine* Anpassen der Oberleitung mit den Abmessungen des Telefons-Seite ist keine direkte Berechnung. Wenn *w* und *h* sind die Breite und Höhe eines Rechtecks, das den optimalen Wert für *eine* erfüllt die folgende Gleichung:

`cosh(w / 2 / a) = 1 + h / a`

Die folgende Methode in der [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) Klasse enthält, auf Gleichheit durch einen Verweis auf die beiden Ausdrücke auf der linken Seite und rechts neben dem Gleichheitszeichen als `left` und `right`. Bei kleinen Werten für *eine*, `left` ist größer als `right`; bei großen Werten für *eine*, `left` ist kleiner als `right`. Die `while` Schleife führt zu einer Einschränkung einer optimalen Wert, der *eine*:

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

Die `SKPath` -Objekt für die Links in den Konstruktor der Klasse erstellt, und die resultierenden ist `SKPathEffect` Objekt legen Sie dann auf die `PathEffect` Eigenschaft der `SKPaint` -Objekt, das als Feld gespeichert wird:

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

Die wichtigste Aufgabe der `PaintSurface` Handler ist, um einen Pfad für die Oberleitung selbst zu erstellen. Nach der Bestimmung des optimalen *eine* und speichern ihn in das `optA` Variable muss auch eine Abweichung von den oberen Rand des Fensters zu berechnen. Anschließend können sie eine Auflistung von kumuliert `SKPoint` Werte für die Oberleitung aktivieren, die zu einem Pfad, und zeichnen Sie den Pfad mit dem zuvor erstellten `SKPaint` Objekt:

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

Dieses Programm definiert, den im verwendete Pfad `Create1DPath` haben die (0, 0) in der Mitte zeigen. Dies erscheint einleuchtend da der (0, 0) des Pfads der Zeile oder die Kurve, die es Verzieren ist ausgerichtet ist. Sie können jedoch einen nicht-zentriert (0, 0) zeigen Sie für einige Spezialeffekte zu erzeugen.

Die **Fließband** Seite erstellt, einen Pfad, der ähnlich wie ein längliche Fließband mit einem gekrümmten oben und unten, auf die Dimensionen des Fensters, dessen Größe geändert wird. Dieser Pfad wird mit einem einfachen Gestrichelt `SKPaint` -Objekt 20 Pixel breit und Grau, und klicken Sie dann erneut mit einem anderen Gestrichelt `SKPaint` Objekt mit einer `SKPathEffect` Objekt, das auf einen Pfad, der ähnlich wie einen wenig Bucket:

[![](effects-images/conveyorbelt-small.png "Dreifacher Screenshot der Seite Fließband")](effects-images/conveyorbelt-large.png#lightbox "dreifachen Screenshot der Seite \"Fließband\"")

Der (0, 0) des Pfads Bucket ist der Handle, das dies der Fall bei der `phase` Argument animiert wird, scheinen die Buckets um Fließband, vielleicht scooping einrichten Wasser am unteren Rand, und Sichern sie sich am Anfang drehen.

Die [ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) -Klasse implementiert die Animation mit Außerkraftsetzungen für die `OnAppearing` und `OnDisappearing` Methoden. Der Pfad für den Bucket wird im Konstruktor der Seite definiert:

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

Der Code für die Datenbankerstellung Bucket wird mit zwei Transformationen, die den Bucket, der ein wenig erhöhen und aktivieren sie anschließend auf die Seite abgeschlossen. Diese Transformationen anwenden ist einfacher, alle Koordinaten im vorherigen Code anpassen.

Die `PaintSurface` Handler beginnt, durch einen Pfad für die Fließband selbst definieren. Dies ist lediglich ein paar Codezeilen und einem Paar von durch Kreise, die mit einer 20 Pixel breit dunkelgrauen Linie gezeichnet werden:

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

Die Logik für das Zeichnen der Fließband funktioniert nicht im Querformat.

Die Buckets sollte ungefähr 200 Pixel auseinander liegen, auf das Band verteilt werden. Förderband ist jedoch wahrscheinlich kein Vielfaches von 200 Pixel lang sein, was bedeutet, dass die `phase` Argument `SKPathEffect.Create1DPath` ist animiert ist, werden Buckets in und aus dem Vorhandensein angezeigt.

Aus diesem Grund die Anwendung zuerst berechnet einen Wert, der mit dem Namen `length` , die Länge des Förderband. Da die Fließband von geraden Linien und durch Kreise besteht, ist dies eine einfache Berechnung. Als Nächstes wird die Anzahl von Buckets berechnet, indem die Division `length` von 200. Dies wird auf die nächste Ganzzahl gerundet und unterteilt, dass die Zahl ist, klicken Sie dann `length`. Das Ergebnis ist ein Abstand für eine ganzzahlige Anzahl von Buckets an. Die `phase` Argument ist einfach ein Bruchteil.

## <a name="from-path-to-path-again"></a>Vom Pfad zum Pfad erneut

Am unteren Rand der `DrawSurface` Ereignishandler in **Fließband**, kommentieren Sie die `canvas.DrawPath` aufrufen, und Ersetzen Sie ihn durch den folgenden Code:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Wie im vorherigen Beispiel der `GetFillPath`, sehen Sie, dass die Ergebnisse mit Ausnahme der Farbe gleich sind. Nach der Ausführung `GetFillPath`, `newPath` Objekt enthält mehrere Kopien der Bucket-Pfad, jeweils positioniert, in der gleichen erkennen, dass die Animation zum Zeitpunkt des Aufrufs positioniert.

## <a name="hatching-an-area"></a>Schraffurmuster eines Bereichs

Die [ `SKPathEffect.Create2DLines` ](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix)) Methode füllt einen Bereich mit parallelen Linien, die häufig aufgerufen *Schraffieren Zeilen*. Die Methode hat die folgende Syntax:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

Die `width` -Argument gibt die Strichbreite der Positionen Schraffierung an. Die `matrix` Parameter ist eine Kombination von Skalierung und optionalen Drehung. Der Skalierungsfaktor gibt das Pixel-Inkrement, das Skia verwendet wird, um die Positionen Schraffierung an Speicherplatz an. Die Trennung zwischen den Zeilen ist der Skalierungsfaktor minus der `width` Argument. Wenn der Skalierungsfaktor kleiner als oder gleich der `width` Wert, es werden kein Leerzeichen zwischen den schraffurlinien und der Bereich erscheint gefüllt werden soll. Geben Sie den Wert für die horizontale und vertikale Skalierung.

Standardmäßig sind schraffurlinien horizontal. Wenn die `matrix` Parameter enthält, Drehung, die schraffurlinien werden im Uhrzeigersinn gedreht.

Die **Schraffur füllen** Seite zeigt dieser Pfad-Effekt. Die [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) -Klasse definiert drei pfadeffekte als Felder, die erste für horizontale schraffurlinien mit einer Breite von 3 Pixel mit einer Skalierung Faktor, der angibt, die sie verteilt 6 Pixel auseinander liegen. Die Trennung zwischen den Zeilen ist daher drei Pixel. Die zweite Pfad wird für die vertikale schraffurlinien mit einer Breite von sechs Pixel Abstand voneinander entfernt (also die Trennung ist 18 Pixel), 24 Pixel und das dritte hingegen ist für die diagonale schraffierte 12 Pixel breit Abstand 36 Pixel zwischen Zeilen.

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

Beachten Sie, dass die Matrix `Multiply` Methode. Da die horizontalen und vertikalen Skalierungsfaktoren identisch sind, spielt die Reihenfolge, in der die Skalierung und Drehung Matrizen multipliziert werden, keine Rolle.

Die `PaintSurface` Handler verwendet diesen drei pfadeffekte mit drei verschiedenen Farben in Kombination mit `fillPaint` , geben Sie ein abgerundetes Rechteck, das die Größe die Seite angepasst. Die `Style` -Eigenschaftensatz am `fillPaint` ignoriert wird, wenn die `SKPaint` Objekt enthält einen Pfad Effekt aus erstellt `SKPathEffect.Create2DLine`, ist unabhängig davon, ob der Bereich gefüllt:

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

Wenn Sie die Ergebnisse sorgfältig betrachten, sehen Sie sich, dass die rote und blaue schraffurlinien genau auf die abgerundetes Rechteck beschränkt sind nicht. (Dies ist offensichtlich ein Merkmal des zugrunde liegenden Codes Skia.) Wenn dies nicht zufriedenstellend ist, wird ein alternativer Ansatz für die diagonal Schraffurlinien in grün angezeigt: Das abgerundete Rechteck wird als clippingpfad verwendet, und die Schraffurlinien werden auf der gesamten Seite gezeichnet.

Die `PaintSurface` Handler endet mit einem Aufruf von einfach die abgerundete Rechteck zu zeichnen, damit Sie die Abweichung mit dem roten und blauen schraffurlinien sehen können:

[![](effects-images/hatchfill-small.png "Dreifacher Screenshot der Seite ausfüllen Schraffur")](effects-images/hatchfill-large.png#lightbox "dreifachen Screenshot der Seite ausfüllen Schraffur")

Der Android-Bildschirm sieht nicht wirklich wie folgt aus: Die Skalierung des Screenshots hat bewirkt, dass die schmalen roten Linien und die dünnen Leerzeichen in scheinbar breitere rote und größere Leerräume konsolidiert wurden.

## <a name="filling-with-a-path"></a>Füllen mit einem Pfad

Die [ `SKPathEffect.Create2DPath` ](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath)) Bereich können Sie einen Bereich mit einem Pfad zu füllen, die horizontal und vertikal, repliziert wird faktisch Kacheln:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

Die `SKMatrix` Skalierungsfaktoren anzugeben, den horizontalen und vertikalen Abstand des replizierten Pfads. Jedoch können nicht gedreht werden den Pfad, der mit diesem `matrix` Argument; Wunsch des Pfads gedreht wird drehen Sie den Pfad selbst mithilfe der `Transform` definierte Methode `SKPath`.

Der Pfad des replizierten wird normalerweise die linke und obere Rand des Bildschirms und nicht als der auszufüllende Bereich ausgerichtet. Sie können dieses Verhalten überschreiben, durch die Bereitstellung der Übersetzung Faktoren zwischen 0 und die Skalierungsfaktoren horizontale und vertikale abweichungen von den linken und oberen Seiten angeben.

Die **Pfad Kachel auszufüllen** Seite zeigt dieser Pfad-Effekt. Der Pfad zum Unterteilen des Bereichs wird definiert als Feld in der [ `PathTileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) Klasse. Die horizontalen und vertikalen Koordinaten Bereich von – 40 bis 40, was bedeutet, dass dieser Pfad 80 Pixel im Quadrat ist:

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

In der `PaintSurface` Handler auf, die `SKPathEffect.Create2DPath` Aufrufe legt den horizontalen und vertikalen Abstand auf 64, die dazu führen, dass die 80-Pixel-quadratischen Kacheln überlappen. Glücklicherweise ähnelt der Pfad einer Puzzleteil, das "meshing" gut mit angrenzenden Kacheln:

[![](effects-images/pathtilefill-small.png "Dreifacher Screenshot der Seite Pfad Kachel auszufüllen")](effects-images/pathtilefill-large.png#lightbox "dreifachen Screenshot der Seite Pfad Kachel auszufüllen.")

Die Skalierung von der ursprünglichen Screenshot führt dazu, dass einige Verzerrung, insbesondere auf die Android-Bildschirm.

Beachten Sie, dass diese Kacheln immer die gesamte angezeigt und werden nicht abgeschnitten. Klicken Sie auf den ersten beiden Screenshots ist es nicht auch offensichtlich, dass der auszufüllende Bereich ein abgerundetes Rechteck ist. Wenn diese Kacheln zu einem bestimmten Bereich abgeschnitten werden sollen, verwenden Sie einen Freistellungspfad an.

Versuchen Sie die `Style` Eigenschaft der `SKPaint` -Objekt `Stroke`, sehen Sie die einzelnen Kacheln gefüllt, anstatt beschrieben.

Es ist auch möglich, einen Bereich mit einer gekachelten Bitmap zu füllen, wie in diesem Artikel gezeigt [ **SkiaSharp Bitmap Tiling**](../effects/shaders/bitmap-tiling.md).

## <a name="rounding-sharp-corners"></a>Runden von Ecken

Die **gerundet Siebeneck** Programm angezeigt wird, der [ **drei Möglichkeiten, einen Bogen zu zeichnen** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) Artikel verwendet einen Tangenten Bogen, um die Punkte einer Figur Gleichseitiges-Kurve. Die **eine andere gerundet Siebeneck** Seite werden viel einfacher, die einen Pfad Effekt aus erstellten verwendet die [ `SKPathEffect.CreateCorner` ](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single)) Methode:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Obwohl das einzelne Argument dem Namen `radius`, müssen Sie es auf die Hälfte der gewünschten Eckradius festlegen. (Dies ist ein Merkmal des zugrunde liegenden Codes Skia.)

Hier ist die `PaintSurface` Ereignishandler in der [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) Klasse:

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

Verwenden Sie diesen Effekt mit Kontur zuweisen oder füllen, die basierend auf der `Style` Eigenschaft der `SKPaint` Objekt. Hier wird ausgeführt:

[![](effects-images/anotherroundedheptagon-small.png "Dreifacher Screenshot der Seite eine andere gerundet Siebeneck")](effects-images/anotherroundedheptagon-large.png#lightbox "dreifachen Screenshot der Seite eine andere gerundet Siebeneck")

Sie sehen, dass diese abgerundeten Siebeneck an das Programm früher identisch ist. Bei Bedarf weitere zu überzeugen, die der Eckradius tatsächlich 100 anstatt die 50 angegeben wird, der `SKPathEffect.CreateCorner` aufrufen, Sie können die auskommentierung aufheben die abschließende Anweisung in die Anwendung und ein 100-Radius-Kreis in der Ecke angezeigt.

## <a name="random-jitter"></a>Zufällige Jitter

Mitunter die Arbeitsdomänencontroller geraden Zeilen der Computergrafiken sind nicht sehr, was Sie möchten, und ein wenig Zufälligkeit erwünscht ist. In diesem Fall sollten Sie versuchen die [ `SKPathEffect.CreateDiscrete` ](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32)) Methode:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Sie können diesbezüglich Pfad verwenden, für die Kontur zuweisen, oder füllen. Zeilen werden in verbundene Segmente unterteilt, deren ungefähre Länge angegeben ist, von `segLength` –, und erweitern Sie in verschiedene Richtungen. Das Ausmaß der Abweichung von der ursprünglichen Zeile wird angegeben, indem `deviation`.

Das letzte Argument ist ein Ausgangswert verwendet, um die pseudozufällige Sequenz, die verwendet werden, für den Effekt zu generieren. Die Auswirkungen der Jitter sieht ein wenig anders für verschiedene Ausgangswerte. Das Argument hat den Standardwert 0 (null), was bedeutet, dass die Auswirkungen identisch ist, wenn Sie das Programm ausführen. Wenn Sie unterschiedliche Jitter möchten, wenn der Bildschirm aktualisiert wird, können Sie den Ausgangswert festlegen, auf die `Millisecond` Eigenschaft eine `DataTime.Now` Wert (z. B.).


Die **Jitter experimentieren** Seite ermöglicht Ihnen das Experimentieren mit verschiedenen Werten in der Kontur Zuweisen eines Rechtecks:

[![](effects-images/jitterexperiment-small.png "Screenshot der Seite Jitter Experiment dreifach")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Die Anwendung ist einfach. Die [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) Datei instanziiert zwei `Slider` Elemente und ein `SKCanvasView`:

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

Die `PaintSurface` Ereignishandler in der [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) Code-Behind-Datei wird aufgerufen, wenn eine `Slider` -Wert ändert. Ruft `SKPathEffect.CreateDiscrete` mit den beiden `Slider` Werte und verwendet, um ein Rechteck zu zeichnen:

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

Sie können diesen Effekt verwenden, für das ausfüllen, diese zufällige abweichungen in diesem Fall die Kontur des ausgefüllten Bereichs unterliegt. Die **Jitter Text** Seite veranschaulicht die Verwendung von diesbezüglich Pfad anzuzeigende Text. Hauptteil des Codes in der `PaintSurface` Handler, der die [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) Klasse geht es um die Größe und den Text zentrieren:

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

Hier wird es im Querformatmodus ausgeführt:

[![](effects-images/jittertext-small.png "Screenshot der Seite Text Jitter dreifach")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Pfad zu gliedern

Sie haben bereits gesehen, zwei kleine Beispiele für die [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath*) -Methode der `SKPaint`, die vorhanden ist, zwei Versionen:

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

Nur die ersten beiden Argumente erforderlich sind. Die Methode greift auf den Pfad, der auf die verwiesen wird durch die `src` Argument ändert die Pfaddaten, die basierend auf den Stricheigenschaften in der `SKPaint` Objekt (einschließlich der `PathEffect` Eigenschaft), und schreibt dann die Ergebnisse in der `dst` Pfad. Die `resScale` Parameter ermöglicht, verringern Sie die Genauigkeit, um einen kleineren Zielpfad an, zu erstellen und die `cullRect` Argument Konturen außerhalb eines Rechtecks kann ausgeschlossen werden.

Eine grundlegende Verwendung dieser Methode beinhaltet keine Pfad Effekte: Wenn für `SKPaint` das `Style` `SKPaintStyle.Stroke` `PathEffect` -Objektdie-Eigenschaftauffestgelegtistundnichtüberdiefestgelegte-Eigenschaftverfügt,wirdvoneinPfaderstellt,dereinenUmrissdesQuellPfadsdarstellt,alsob`GetFillPath` er vom Paint-Eigenschaften.

Z. B. wenn die `src` Pfad ist ein einfacher Kreis RADIUS 500, und die `SKPaint` Objekt gibt an, eine Strichbreite von 100, und klicken Sie dann die `dst` Pfad wird der beiden konzentrischen Kreise, eine mit einem Radius von 450 und der andere mit einem Radius von 550. Die Methode wird aufgerufen, `GetFillPath` , da dies ausfüllen `dst` Pfad entspricht dem Verlauf der `src` Pfad. Aber Sie können auch zeichnen die `dst` Pfades darauf, den Pfad beschreibt.

Die **Tippen Sie auf, um den Umriss des Pfads** veranschaulicht dies. Die `SKCanvasView` und `TapGestureRecognizer` instanziiert werden, der [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) Datei. Die [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) Code-Behind-Datei definiert drei `SKPaint` Objekte als Felder, zwei für die Kontur mit zuweisen zeichnen Breite von 100 20 und die dritte für den ausfüllen:

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

Wenn der Bildschirm nicht abgerufen wurde, die `PaintSurface` Ereignishandler verwendet die `blueFill` und `redThickStroke` Zeichnen von Objekten auf einen kreisförmigen Pfad zu rendern:

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

Der Kreis gefüllt und mit Strichen dargestellt, wie Sie erwarten:

[![](effects-images/taptooutlinethepathnormal-small.png "Dreifacher Screenshot der Seite für normale Tippen Sie auf die Umrisspfad")](effects-images/taptooutlinethepathnormal-large.png#lightbox "dreifachen Screenshot der normalen Tippen Sie auf die Umrisspfad-Seite")

Tippen Sie auf dem Bildschirm `outlineThePath` nastaven NA hodnotu `true`, und die `PaintSurface` Ereignishandler erstellt eine neue `SKPath` -Objekt und verwendet diesen als der angegebene Zielpfad in einem Aufruf von `GetFillPath` auf die `redThickStroke` Paint-Objekt. Klicken Sie dann diese Zielpfad gefüllt und mit Kontur in `redThinStroke`, sodass in der folgenden:

[![](effects-images/taptooutlinethepathoutlined-small.png "Dreifacher Screenshot Seitenrand Tippen Sie auf die Umrisspfad Kontur")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "dreifachen Screenshot Seitenrand Tippen Sie auf die Umrisspfad Kontur")

Die beiden roten Kreise zeigen deutlich, dass der ursprüngliche Pfad für zirkuläre in zwei zirkuläre Konturen konvertiert wurde.

Diese Methode kann sehr nützlich bei der Entwicklung von Pfaden für die Verwendung der `SKPathEffect.Create1DPath` Methode. Die angegebenen Pfade in diesen Methoden werden immer gefüllt, wenn die Pfade repliziert werden. Wenn Sie nicht möchten, dass den gesamten Pfad gefüllt werden soll, müssen Sie sorgfältig die Umrisse definieren.

Z. B. in der **verknüpfte Kette** Beispiel definiert, die Links mit einer Reihe von vier Bögen, jedes Paar der basieren auf den zwei Radien, sich den Bereich des Pfads gefüllt werden soll. Es ist möglich, ersetzen Sie den Code in die `LinkedChainPage` Klasse dafür ein wenig anders.

Zunächst sollten Sie neu definieren die `linkRadius` Konstanten:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

Die `linkPath` ist jetzt nur zwei Bögen basierend auf dieser einzelnen Radius, mit den gewünschten Winkel starten und beim Aufräumen Winkeln bearbeitete:

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

Die `outlinePath` Objekt kann dann die Empfänger der Kontur der `linkPath` Wenn es mit den Eigenschaften, die im angegebenen schraffiert ist `strokePaint`.

Ein weiteres Beispiel, die mit diesem Verfahren kommt als Nächstes für den Pfad in einer Methode verwendet.

## <a name="combining-path-effects"></a>Kombinieren von Pfadeffekte

Die beiden letzten statische Erstellungsmethoden der `SKPathEffect` sind [ `SKPathEffect.CreateSum` ](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) und [ `SKPathEffect.CreateCompose` ](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Beide Methoden kombinieren, zwei pfadeffekte um einen zusammengesetzten Pfad-Effekt zu erstellen. Die `CreateSum` Methode erstellt einen Pfad ab, der ähnlich wie die beiden pfadeffekte separat angewendet wird während `CreateCompose` einen Pfad Effekt angewendet (die `inner`) und wendet dann die `outer` mit.

Sie haben bereits gesehen, wie die `GetFillPath` -Methode der `SKPaint` können konvertieren Sie einen Pfad, an einen anderen Pfad basierend auf `SKPaint` Eigenschaften (einschließlich `PathEffect`), damit es sein, darf nicht *zu* mysteriöse, wie ein `SKPaint`Objekt kann diesen Vorgang ausführen, zweimal mit zwei Pfad Effekten, die im angegebenen die `CreateSum` oder `CreateCompose` Methoden.

Eine offensichtliche Verwendung `CreateSum` besteht darin, definieren eine `SKPaint` Objekt, das einen Pfad mit einem Pfad Effekt füllt und den Pfad mit einem anderen Pfad wirksam die Striche. Dies wird veranschaulicht, der **Katzen im Frame** Beispiel, das ein Array von Katzen innerhalb eines Rahmens mit gewellte Kanten angezeigt:

[![](effects-images/catsinframe-small.png "Dreifacher Screenshot der Seite für Katzen im Frame")](effects-images/catsinframe-large.png#lightbox "dreifachen Screenshot der Seite für Katzen im Frame")

Die [ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Klasse beginnt, indem Sie mehrere Felder definieren. Sie kennen das erste Feld aus der [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) -Klasse aus der [ **SVG-Pfaddaten** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) Artikel. Der zweite Pfad basiert auf einer Zeile und einen Bogen konvertiert nach dem Muster Ausbuchten des Frames:

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

Die `catPath` kann verwendet werden, der `SKPathEffect.Create2DPath` Methode Wenn die `SKPaint` Objekt `Style` -Eigenschaftensatz auf `Stroke`. Aber wenn die `catPath` wird verwendet, direkt in diesem Programm, klicken Sie dann den gesamten Anfang das Cat gefüllt, und Whisker nicht selbst sichtbar. (Probieren Sie es!) Es ist erforderlich, erhalten, dass der Pfad den Umriss, und Verwenden von dieser Gliederung in den `SKPathEffect.Create2DPath` Methode.

Der Konstruktor führt diese Aufgabe aus. Sie zuerst angewendet wird, zwei Transformationen `catPath` zum Verschieben der (0, 0) zeigen Sie auf die Mitte und Größe Herunterskalieren. `GetFillPath` Ruft alle Gliederungen der Konturen in `outlinedCatPath`, und dieses Objekt wird verwendet, der `SKPathEffect.Create2DPath` aufrufen. Die Skalierung Faktoren die `SKMatrix` Wert sind geringfügig größer sein als die horizontale und vertikale Größe des der Katze zu wenig Puffer zwischen der Kacheln verläuft, während der Übersetzung Faktoren sind abgeleitete etwas empirisch, damit eine vollständige Katze in angezeigt wird der linke obere Ecke des Frames:

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

Anschließend ruft der Konstruktor `SKPathEffect.Create1DPath` für den gewellte Frame. Beachten Sie, dass die Breite des Pfads 100 Pixel beträgt, aber der vorabversandmitteilung 75 Pixel, damit der replizierte Pfad um den Frame überlappt wird. Der Konstruktor ruft die letzte Anweisung `SKPathEffect.CreateSum` Kombinieren der zwei pfadeffekte und das Ergebnis auf die `SKPaint` Objekt.

All diese Arbeit ermöglicht die `PaintSurface` Handler, der ziemlich einfach sein. Es muss nur zum Definieren eines Rechtecks, und ziehen Sie es mit `framePaint`:

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

Die bewährten Algorithmen hinter der pfadeffekte führen immer den gesamten Pfad zum Kontur zuweisen oder zu füllen, die angezeigt werden, wodurch einige visuelle Elemente außerhalb des Rechtecks angezeigt werden kann. Die `ClipRect` vor dem Aufrufen der `DrawRect` einrichtungsaufrufs können die Visualisierungen deutlich viel klarer sein. (Probieren Sie es ohne Clipping!)

Es ist üblich, verwenden Sie `SKPathEffect.CreateCompose` etwas Jitter auftreten, einen anderen Pfad Effekt hinzufügen. Sie können sicherlich selbst experimentieren, aber hier ist ein etwas anderes Beispiel:

Die **gestrichelte Schraffurlinien** füllt eine Ellipse mit schraffurlinien, die gestrichelt werden. Die meisten Aufgaben in der [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) Klasse erfolgt direkt in den Felddefinitionen. Diese Felder definieren Sie einen Effekt Dash und einen Effekt Schraffur. Sie definiert sind, als `static` , da diese dann in referenziert werden ein `SKPathEffect.CreateCompose` rufen Sie in der `SKPaint` Definition:

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

Die `PaintSurface` Handler muss nur der standard-Aufwand sowie einen Aufruf von enthalten `DrawOval`:

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

Wenn Sie bereits festgestellt haben die, die schraffurlinien nicht genau mit dem inneren Bereich beschränkt sind und in diesem Beispiel immer beginnen auf der linken Seite mit einem ganzen Bindestrich:

[![](effects-images/dashedhatchlines-small.png "Dreifacher Screenshot der Seite Schraffurlinien Gestrichelt")](effects-images/dashedhatchlines-large.png#lightbox "dreifachen Screenshot der Seite Schraffurlinien gestrichelt")

Nun, da Sie pfadeffekte, reichen von einfachen Punkte und Striche seltsame Kombinationen aus gesehen haben, verwenden Sie Ihre Vorstellungskraft, und sehen Sie, was Sie erstellen können.



## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
