---
title: SkiaSharp linearen Farbverlaufs
description: Erfahren Sie, wie Zeichnen von Linien oder Bereiche mit Farbverläufen, bestehend aus schrittweise eine Mischung aus zwei Farben füllen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 9551a3b8e093dbb49a55a3761543602c40e81023
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53053576"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp linearen Farbverlaufs

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Die [ `SKPaint` ](xref:SkiaSharp.SKPaint) -Klasse definiert eine [ `Color` ](xref:SkiaSharp.SKPaint.Color) -Eigenschaft, die zum Zeichnen von Linien oder Bereiche mit einer Volltonfarbe füllen verwendet wird. Sie können alternativ Zeichnen von Linien oder Bereiche mit _Farbverläufe_, die schrittweise Farbverlauf der Farben sind:

![Beispiel für linearen Farbverlauf](linear-gradient-images/LinearGradientSample.png "linearen Farbverlauf-Beispiel")

Die einfachste Form des Farbverlaufs ist eine _lineare_ Farbverlauf. Die Mischung von Farben in einer Zeile auftritt (bezeichnet das _Farbverlaufslinie_) von einem Punkt in eine andere. Zeilen, die senkrecht zur der Farbverlaufslinie sind aufweisen die gleiche Farbe. Sie erstellen einen linearen Farbverlauf mit einer der beiden statischen [ `SKShader.CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient*) Methoden. Der Unterschied zwischen den zwei Überladungen ist, dass eine eine Matrixtransformation enthält und das andere nicht. 

Diese Methoden geben ein Objekt des Typs zurück [ `SKShader` ](xref:SkiaSharp.SKShader) , die Sie festlegen, um die [ `Shader` ](xref:SkiaSharp.SKPaint.Shader) Eigenschaft `SKPaint`. Wenn die `Shader` -Eigenschaft ungleich Null ist, überschreibt es die `Color` Eigenschaft. Jede Zeile, die mit Strichen gezeichnet wird oder andere Bereiche, die mit diesem gefüllt ist `SKPaint` -Objekts basiert auf der Volltonfarbe entspricht, anstatt den Farbverlauf.

> [!NOTE]
> Die `Shader` Eigenschaft wird ignoriert, wenn Sie enthalten eine `SKPaint` -Objekt in ein `DrawBitmap` aufrufen. Können Sie die `Color` Eigenschaft `SKPaint` Grad der Transparenz für die Anzeige einer Bitmap festlegen (in diesem Artikel beschriebenen [SkiaSharp Anzeigen von Bitmaps](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)), kann jedoch nicht die `Shader` Eigenschaft für die Anzeige von eine Bitmap mit einem Farbverlauf Transparenz. Andere Methoden stehen zum Anzeigen von Bitmaps mit Farbverlauf Transparenz: Diese werden in den Artikeln beschrieben [SkiaSharp zirkuläre Farbverläufe](circular-gradients.md#radial-gradients-for-masking) und [SkiaSharp-Zusammensetzung und Blend-Modi](../blend-modes/porter-duff.md#gradient-transparency-and-transitions).

## <a name="corner-to-corner-gradients"></a>Ecke-zu-Ecke Farbverläufe

Erstreckt sich häufig ein linearer Farbverlauf von einer Ecke eines Rechtecks zu einem anderen. Wenn Sie der Startpunkt auf der linken oberen Ecke des Rechtecks ist, kann Farbverlaufs erweitern:

- vertikal auf der linken unteren Ecke.
- horizontal auf der oberen rechten Ecke
- auf der unteren rechten Ecke diagonal

Diagonale, lineare Farbverlauf wird veranschaulicht, in die erste Seite in der **SkiaSharp-Shader und andere Effekte** Teil der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel. Die **Ecke-zu-Ecke Farbverlauf** -Seite erstellt eine `SKCanvasView` in seinem Konstruktor. Die `PaintSurface` Ereignishandler erstellt ein `SKPaint` -Objekt in ein `using` Anweisung und definiert dann ein quadratisches 300 Pixel-Rechteck, der zentriert in der Canvas:

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    ···
    public CornerToCornerGradientPage ()
    {
        Title = "Corner-to-Corner Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ···
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Create 300-pixel square centered rectangle
            float x = (info.Width - 300) / 2;
            float y = (info.Height - 300) / 2;
            SKRect rect = new SKRect(x, y, x + 300, y + 300);

            // Create linear gradient from upper-left to lower-right
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { SKColors.Red, SKColors.Blue },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Repeat);

            // Draw the gradient on the rectangle
            canvas.DrawRect(rect, paint);
            ···
        }
    }
}
```

Die `Shader` Eigenschaft `SKPaint` erhält die `SKShader` Rückgabewert der statischen `SKShader.CreateLinearGradient` Methode. Die fünf Argumente sind wie folgt aus:

- Der Ausgangspunkt des Farbverlaufs entspricht, hier auf der linken oberen Ecke des Rechtecks festgelegt.
- Der Endpunkt des Farbverlaufs entspricht, hier auf der unteren rechten Ecke des Rechtecks festgelegt.
- Ein Array von zwei oder mehr Farben, die bei der Farbverlauf beitragen
- Ein Array von `float` Werte, der angibt, der relativen Position der Farben in der Farbverlaufslinie
- Ein Mitglied der [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) Enumeration, der angibt, das Verhalten des Farbverlaufs hinter das Ende der Farbverlaufslinie

Nachdem farbverlaufsobjekt erstellt wurde, die `DrawRect` Methode zeichnet die quadratische Rechteck von 300 Pixel mit den `SKPaint` -Objekt, das den Shader enthält. Hier wird die Anwendung unter iOS, Android und die universelle Windows-Plattform (UWP) ausgeführt:

[![Ecke-zu-Ecke Farbverlauf](linear-gradient-images/CornerToCornerGradient.png "Ecke-zu-Ecke Farbverlauf")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

Der Farbverlaufslinie wird durch die beiden Punkte, die als die ersten beiden Argumente definiert. Beachten Sie, dass diese Punkte relativ zu den _canvas_ und _nicht_ mit dem grafischen Objekt mit dem Verlauf angezeigt. Entlang der Farbverlaufslinie wechselt die Farbe allmählich von Rot in der oberen linken Ecke unter der unteren rechten Ecke Blau. Jede Zeile, die senkrecht zur der Farbverlaufslinie hat es sich um eine Konstante Farbe.

Das Array von `float` Werte als viertes Argument angegeben sind, mit dem Array von Farben eins zu eins entsprechen. Die Werte geben die relative Position entlang der Farbverlaufslinie, in denen diese Farben auftreten. Hier ist die 0 bedeutet, dass `Red` tritt am Anfang der Farbverlaufslinie, und 1 bedeutet, dass `Blue` tritt am Ende der Zeile. Die Zahlen müssen aufsteigend sein und müssen im Bereich von 0 bis 1. Wenn sie in diesem Bereich nicht sind, werden sie in diesem Bereich werden angepasst.

Die beiden Werte im Array können auf einen anderen Wert als 0 und 1 festgelegt werden. Testen Sie Folgendes:

```csharp
new float[] { 0.25f, 0.75f }
```

Jetzt ist die gesamte erste Quartal der Farbverlaufslinie reines Rot, und das letzte Quartal ist reine Blau. Die Kombination von Red Team und Blue ist auf der zentrale Teil der Farbverlaufslinie beschränkt.

Im Allgemeinen sollten Sie diese Positionswerte gleichermaßen von 0 auf 1 Leerzeichen. Wenn dies der Fall ist, können Sie einfach eine angeben `null` als das vierte Argument `CreateLinearGradient`.

Obwohl diese Farbverlauf zwischen beiden Ecken des Rechtecks quadratischen 300 Pixel definiert ist, ist es nicht auf dieses Rechteck ausfüllen beschränkt. Die **Ecke-zu-Ecke Farbverlauf** Seite enthält einen zusätzlichen Code, der auf Klicks reagiert oder Mausklicks auf der Seite. Die `drawBackground` Feld wird zwischen umgeschaltet `true` und `false` mit jeder tippen. Wenn der Wert ist `true`, und klicken Sie dann die `PaintSurface` Ereignishandler verwendet die gleichen `SKPaint` das Objekt, für den gesamten Zeichenbereich ausfüllen und zeichnet dann ein schwarzes Rechteck, der angibt, des kleinere Rechtecks: 

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    bool drawBackground;

    public CornerToCornerGradientPage ()
    {
        ···
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            drawBackground ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            ···
            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                canvas.DrawRect(info.Rect, paint);

                // Outline the smaller rectangle
                paint.Shader = null;
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                canvas.DrawRect(rect, paint);
            }
        }
    }
}
```

Hier ist, was Sie sehen werden, nach dem Tippen auf dem Bildschirm:

[![Farbverlauf vollständige Ecke-zu-Ecke](linear-gradient-images/CornerToCornerGradientFull.png "Farbverlauf vollständige Ecke-zu-Ecke")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

Beachten Sie, dass der Farbverlauf in dem gleichen Muster über die Punkte, die Definition der Farbverlaufslinie wiederholt. Diese Wiederholung tritt auf, weil das letzte Argument für `CreateLinearGradient` ist `SKShaderTileMode.Repeat`. (Sie müssen die anderen Optionen in Kürze sehen.)

Beachten Sie außerdem, dass die Punkte, die Sie mithilfe der Farbverlaufslinie angeben nicht eindeutig sind. Zeilen, die senkrecht zur der Farbverlaufslinie sind haben die gleiche Farbe, daher gibt es eine unendliche Anzahl von Farbverlauf Zeilen, die Sie für die gleiche Auswirkung angeben können. Beispielsweise können bei der ein Rechteck mit einem horizontalen Farbverlauf zu füllen, Sie angeben der oberen linken und rechten oberen Ecken oder den unteren linken und rechten unteren Ecken oder beliebigen zwei Punkten, die selbst und parallel zur diese Zeilen sind.

## <a name="interactively-experiment"></a>Interaktives experimentieren.

Sie können interaktiv mit einem linearen Farbverlauf mit experimentieren die **interaktive linearen Farbverlauf** Seite. Diese Seite verwendet die `InteractivePage` Klasse eingeführt, die in diesem Artikel [ **drei Möglichkeiten, einen Bogen zu zeichnen**](../../curves/arcs.md). `InteractivePage` Handles [ `TouchEffect` ](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) Ereignisse, um eine Auflistung von verwalten `TouchPoint` Objekte, die Sie mit den Fingern und mit der Maus verschieben können.

Fügt die XAML-Datei der `TouchEffect` an ein übergeordnetes Element von der `SKCanvasView` und umfasst auch eine `Picker` , ermöglicht Ihnen die Auswahl eines der drei Elemente von der [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) Enumeration:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.InteractiveLinearGradientPage"
                       Title="Interactive Linear Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid BackgroundColor="White"
              Grid.Row="0">
            <skiaforms:SKCanvasView x:Name="canvasView"
                                    PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</local:InteractivePage>
```

Der Konstruktor im Code-Behind-Datei erstellt zwei `TouchPoint` Objekte für die Start- und Endpunkt des linearen Farbverlaufs. Die `PaintSurface` Handler definiert ein Array von drei Farben (für einen Farbverlauf von Rot, Grün und Blau) und ruft die aktuelle `SKShaderTileMode` aus der `Picker`:

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    public InteractiveLinearGradientPage ()
    {
        InitializeComponent ();

        touchPoints = new TouchPoint[2];

        for (int i = 0; i < 2; i++)
        { 
            touchPoints[i] = new TouchPoint
            {
                Center = new SKPoint(100 + i * 200, 100 + i * 200)
            };
        }

        InitializeComponent();
        baseCanvasView = canvasView;
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKColor[] colors = { SKColors.Red, SKColors.Green, SKColors.Blue };
        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateLinearGradient(touchPoints[0].Center,
                                                         touchPoints[1].Center,
                                                         colors,
                                                         null,
                                                         tileMode);
            canvas.DrawRect(info.Rect, paint);
        }
        ···
    }
}
```

Die `PaintSurface` Ereignishandler erstellt die `SKShader` Objekt aus der alle diese Informationen aus, und verwendet, um die Farbe, die des gesamten Zeichenbereichs. Das Array von `float` Werte nastaven NA hodnotu `null`. Drei Farben gleich sein soll, würden Sie andernfalls diesen Parameter in ein Array mit den Werten 0, 0,5 und 1 festlegen.

Der größte Teil der `PaintSurface` Handler geht es um mehrere Objekte anzeigen: die Touch-Punkte als Gliederung Kreise, der Farbverlaufslinie und die Zeilen, die senkrecht zur der Farbverlauf Zeilen an die Touch-Punkte:

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Display the touch points here rather than by TouchPoint
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;

            foreach (TouchPoint touchPoint in touchPoints)
            {
                canvas.DrawCircle(touchPoint.Center, touchPoint.Radius, paint);
            }

            // Draw gradient line connecting touchpoints
            canvas.DrawLine(touchPoints[0].Center, touchPoints[1].Center, paint);

            // Draw lines perpendicular to the gradient line
            SKPoint vector = touchPoints[1].Center - touchPoints[0].Center;
            float length = (float)Math.Sqrt(Math.Pow(vector.X, 2) +
                                            Math.Pow(vector.Y, 2));
            vector.X /= length;
            vector.Y /= length;
            SKPoint rotate90 = new SKPoint(-vector.Y, vector.X);
            rotate90.X *= 200;
            rotate90.Y *= 200;

            canvas.DrawLine(touchPoints[0].Center, 
                            touchPoints[0].Center + rotate90, 
                            paint);

            canvas.DrawLine(touchPoints[0].Center,
                            touchPoints[0].Center - rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center + rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center - rotate90,
                            paint);
        }
    }
}
```

Das Herstellen einer Verbindung der beiden Touchpoints Farbverlaufslinie ist einfach zu zeichnen, aber die senkrecht Zeilen benötigen einige weitere Aufgaben. Der Farbverlaufslinie wird konvertiert in einen Vektor normalisiert, um eine Länge von einer Einheit haben und dann um 90 Grad gedreht. Diesem Vektor erhält anschließend eine Länge von 200 Pixel. Es wird zum Zeichnen von vier Zeilen, die aus der Touch-Punkte senkrecht zur der Farbverlaufslinie sein.

Die senkrecht Zeilen übereinstimmen, mit dem Anfang und Ende des Farbverlaufs. Was geschieht, über die Zeilen, hängt davon ab, der Einstellung von der `SKShaderTileMode` Enumeration:

[![Interaktive linearer Farbverlauf](linear-gradient-images/InteractiveLinearGradient.png "interaktive linearer Farbverlauf")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

Die drei Screenshots zeigen die Ergebnisse der die drei unterschiedlichen Werte von [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode). Der iOS-Screenshot zeigt `SKShaderTileMode.Clamp`, einfach erweitert die Farben der Rahmen des Farbverlaufs entspricht. Die `SKShaderTileMode.Repeat` -Option in der Android-Screenshot zeigt, wie das Verlaufsmuster wiederholt wird. Die `SKShaderTileMode.Mirror` -Option in der UWP-Screenshot wird auch das Muster wiederholt, das Muster ist jedoch umgekehrt jedes Mal, was keine Diskontinuitäten Farbe.

## <a name="gradients-on-gradients"></a>Farbverläufe in Farbverläufen

Die `SKShader` -Klasse definiert keine öffentlichen Eigenschaften oder Methoden, mit Ausnahme von `Dispose`. Die `SKShader` Objekte, die die statischen Methoden erstellt daher unveränderlich sind. Auch wenn Sie den gleichen Farbverlauf für zwei verschiedene Objekte verwenden, ist es wahrscheinlich ist, Sie sollten den Farbverlauf weichen geringfügig ab. Zu diesem Zweck müssen Sie zum Erstellen eines neuen `SKShader` Objekt.

Die **Text-Farbverlauf** Seite zeigt Text und einem Brackground, die beide mit einem ähnlichen Farbverlauf farblich dargestellt werden:

[![Text-Farbverlauf](linear-gradient-images/GradientText.png "Text-Farbverlauf")](linear-gradient-images/GradientText-Large.png#lightbox)

Der einzige Unterschied in der Farbverläufe sind die Start- und Endpunkt. Zum Anzeigen von Text verwendeten Farbverlaufs basiert auf zwei Punkten auf die Ecken des umschließenden Rechtecks für den Text. Für den Hintergrund basiert die beiden Punkte auf den gesamten Zeichenbereich. Hier ist der Code ein:

```csharp
public class GradientTextPage : ContentPage
{
    const string TEXT = "GRADIENT";

    public GradientTextPage ()
    {
        Title = "Gradient Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Create gradient for background
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                new SKPoint(info.Width, info.Height),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw background
            canvas.DrawRect(info.Rect, paint);

            // Set TextSize to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Shift textBounds by that amount
            textBounds.Offset(xText, yText);

            // Create gradient for text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(textBounds.Left, textBounds.Top),
                                new SKPoint(textBounds.Right, textBounds.Bottom),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Die `Shader` Eigenschaft der `SKPaint` Objekt zuerst einen Farbverlauf an, um den Hintergrund abzudecken Anzeige festgelegt ist. Die Farbverlauf-Punkte werden auf der oberen linken und rechten unteren Ecken der Leinwand festgelegt.

Der Code legt die `TextSize` Eigenschaft der `SKPaint` Objekts, sodass der Text bei 90 % der Breite des Zeichenbereichs angezeigt wird. Werden die textbegrenzungen berechnet `xText` und `yText` Werte, die zum Übergeben der `DrawText` Methode, um den Text zentrieren.

Allerdings verweist des Farbverlaufs für die zweite `CreateLinearGradient` Aufruf muss auf der linken oberen und unteren rechten Ecke des Texts relativ zur Leinwand verweisen, wenn er angezeigt wird. Dies erfolgt durch die Umstellung der `textBounds` Rechteck vom selben `xText` und `yText` Werte:

```csharp
textBounds.Offset(xText, yText);
```

Nachdem der oberen linken und rechten unteren Ecken des Rechtecks zum Festlegen der Start- und Endpunkt des Farbverlaufs verwendet werden können.

## <a name="animating-a-gradient"></a>Animieren eines Farbverlaufs

Es gibt mehrere Möglichkeiten, die einen Farbverlauf zu animieren. Ein Ansatz darin, die Start- und Endpunkt zu animieren. Die **Farbverlauf Animation** Seite verschiebt die beiden Punkte in einem Kreis, der zentriert wird im Zeichenbereich. Der Radius der diesen Vertrauenskreis aufgenommen wird, die Hälfte der Breite oder Höhe des Zeichenbereichs, welcher Wert kleiner ist. Die Start- und Endpunkt gegenüber auf diesen Vertrauenskreis aufgenommen und der Farbverlauf wird von weiß auf Schwarz festgelegt, mit einem `Mirror` nebeneinander:

[![Farbverlauf Animation](linear-gradient-images/GradientAnimation.png "Farbverlauf Animation")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

Der Konstruktor erstellt die `SKCanvasView`. Die `OnAppearing` und `OnDisappearing` Methoden verarbeiten die Animationslogik:

```csharp
public class GradientAnimationPage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    double angle;
    Stopwatch stopwatch = new Stopwatch();

    public GradientAnimationPage()
    {
        Title = "Gradient Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 3000;
        angle = 2 * Math.PI * (stopwatch.ElapsedMilliseconds % duration) / duration;
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Die `OnTimerTick` Methode berechnet einen `angle` -Wert, der von 0 zu 2π alle 3 Sekunden animiert wird. 

Hier ist eine Möglichkeit, um den Farbverlauf zwei Punkten zu berechnen. Ein `SKPoint` Wert mit dem Namen `vector` errechnet sich aus der Mitte der Canvas mit einem Punkt der Radius des Kreises zu erweitern. Die Richtung dieses Vektors basiert auf den Werten Sinus und Cosinus des Winkels. Die beiden umgekehrter Farbverlauf Punkte werden dann berechnet: ein Punkt wird berechnet, indem Sie diesem Vektor vom Mittelpunkt subtrahiert und weiterer Punkt wird berechnet, indem Sie den Mittelpunkt den Vektor hinzugefügt:

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
            int radius = Math.Min(info.Width, info.Height) / 2;
            SKPoint vector = new SKPoint((float)(radius * Math.Cos(angle)),
                                         (float)(radius * Math.Sin(angle)));

            paint.Shader = SKShader.CreateLinearGradient(
                                center - vector,
                                center + vector,
                                new SKColor[] { SKColors.White, SKColors.Black },
                                null,
                                SKShaderTileMode.Mirror);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Ein etwas anderer Ansatz ist weniger Code erforderlich. Dieser Ansatz nutzt die [ `SKShader.CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladung der Methode mit einer Matrixtransformation als letztes Argument. Dieser Ansatz ist die Version in der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel:

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                info.Width < info.Height ? new SKPoint(info.Width, 0) : 
                                                           new SKPoint(0, info.Height),
                                new SKColor[] { SKColors.White, SKColors.Black },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Mirror,
                                SKMatrix.MakeRotation((float)angle, info.Rect.MidX, info.Rect.MidY));

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Wenn die Breite des Zeichenbereichs ist kleiner als die Höhe, auf die beiden Farbverlauf Punkte festgelegt sind (0, 0) und (`info.Width`, 0). Die Drehungstransformation übergeben wird, als das letzte Argument für `CreateLinearGradient` effektiv rotiert diese beiden Punkte in der Mitte des Bildschirms.

Beachten Sie, dass, wenn der Winkel 0 ist, es keine Drehung gibt, und die zwei Farbverlauf der oberen linken und rechten oberen Ecken des Zeichenbereichs. Diese Punkte werden nicht die gleichen Farbverlauf Punkte berechnet, wie im vorherigen gezeigt `CreateLinearGradient` aufrufen. Diese Punkte sind jedoch _parallele_ auf der horizontalen Farbverlaufslinie bisects, die die Mitte der Canvas, und führen dazu, dass eine identische Farbverlauf.

**Rainbow Farbverlauf**

Die **Rainbow Farbverlauf** Seite zeichnet eine Rainbow aus der linken oberen Ecke des Zeichenbereichs, auf der unteren rechten Ecke. Aber diese Rainbow Farbverlauf nicht wie eine echte Rainbow. Direkt, anstatt gekrümmt ist, aber sie basiert auf acht HSL (Farbton-Sättigung-Helligkeit)-Farben, die durch Durchlaufen der Hue-Werte zwischen 0 und 360 bestimmt werden:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

Die Code Teil von der `PaintSurface` Handler unten. Der Handler beginnt mit dem Erstellen eines Pfads, das ein Polygon sechsseitiger definiert, die an der unteren rechten Ecke aus der linken oberen Ecke des Zeichenbereichs erweitert:

```csharp
public class RainbowGradientPage : ContentPage
{
    public RainbowGradientPage ()
    {
        Title = "Rainbow Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 2f;

            // Create path from upper-left to lower-right corner
            path.MoveTo(0, 0);
            path.LineTo(rainbowWidth / 2, 0);
            path.LineTo(info.Width, info.Height - rainbowWidth / 2);
            path.LineTo(info.Width, info.Height);
            path.LineTo(info.Width - rainbowWidth / 2, info.Height);
            path.LineTo(0, rainbowWidth / 2);
            path.Close();

            using (SKPaint paint = new SKPaint())
            {
                SKColor[] colors = new SKColor[8];

                for (int i = 0; i < colors.Length; i++)
                {
                    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
                }

                paint.Shader = SKShader.CreateLinearGradient(
                                    new SKPoint(0, rainbowWidth / 2), 
                                    new SKPoint(rainbowWidth / 2, 0),
                                    colors,
                                    null,
                                    SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

Die beiden Farbverlauf Punkte in der `CreateLinearGradient` Methode basieren auf zwei Punkte, die diesen Pfad zu definieren: beide Punkte werden in der Nähe der oberen linken Ecke. Die erste ist, auf den oberen Rand des Zeichenbereichs, und die zweite ist am linken Rand des Zeichenbereichs. Hier ist das Ergebnis:

[![Rainbow Farbverlauf fehlerhafte](linear-gradient-images/RainbowGradientFaulty.png "Rainbow Farbverlauf fehlerhaft")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

Dies ist eine interessante Image, aber es ist nicht sehr von der Absicht. Das Problem ist, dass bei einen linearen Farbverlauf zu erstellen, die Zeilen der Konstante Farbe senkrecht zur der Farbverlaufslinie sind. Der Farbverlaufslinie basiert auf dem, in dem in der Abbildung berührt die oberen und linken Rand, und diese Zeile in der Regel nicht senkrecht an den Rändern des in der Abbildung, die auf der unteren rechten Ecke erweitern. Dieser Ansatz funktioniert nur, wenn im Zeichenbereich quadratischen wurden.

Um eine ordnungsgemäße Rainbow Farbverlauf zu erstellen, muss der Farbverlaufslinie senkrecht an den Rand des den Regenbogen hervorgebracht. Dies ist eine komplexere Berechnung. Ein Vektor muss definiert werden, die parallel zu der langen Seite des in der Abbildung ist. Der Vektor ist um 90 Grad gedreht, sodass es senkrecht an dieser Seite ist. Es ist dann verlängert werden, um die Breite der Abbildung werden durch Multiplikation mit `rainbowWidth`. Die beiden Farbverlauf Punkte auf einen Punkt im Zweifelsfall in der Abbildung werden berechnet, und, sowie den Vektor verweisen. Hier ist der Code, der angezeigt wird der **Rainbow Farbverlauf** auf der Seite die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel:

```csharp
public class RainbowGradientPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPath path = new SKPath())
        {
            ···
            using (SKPaint paint = new SKPaint())
            {
                ···
                // Vector on lower-left edge, from top to bottom 
                SKPoint edgeVector = new SKPoint(info.Width - rainbowWidth / 2, info.Height) - 
                                     new SKPoint(0, rainbowWidth / 2);

                // Rotate 90 degrees counter-clockwise:
                SKPoint gradientVector = new SKPoint(edgeVector.Y, -edgeVector.X);

                // Normalize
                float length = (float)Math.Sqrt(Math.Pow(gradientVector.X, 2) +
                                                Math.Pow(gradientVector.Y, 2));
                gradientVector.X /= length;
                gradientVector.Y /= length;

                // Make it the width of the rainbow
                gradientVector.X *= rainbowWidth;
                gradientVector.Y *= rainbowWidth;

                // Calculate the two points
                SKPoint point1 = new SKPoint(0, rainbowWidth / 2);
                SKPoint point2 = point1 + gradientVector;

                paint.Shader = SKShader.CreateLinearGradient(point1,
                                                             point2,
                                                             colors,
                                                             null,
                                                             SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

Jetzt sind die Rainbow Farben in der Abbildung ausgerichtet:

[![Rainbow Farbverlauf](linear-gradient-images/RainbowGradient.png "Rainbow Farbverlauf")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**Infinity-Farben**

Ein Farbverlauf Rainbow wird auch verwendet, der **unendlich Farben** Seite. Auf dieser Seite zeichnet eine unendlich-Anmeldung mit einem Pfadobjekt, das in diesem Artikel beschriebenen [ **drei Typen von Bézierkurven**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs). Das Bild wird dann mit einem Farbverlauf animierte Rainbow gefärbt, der kontinuierlich auf das Bild führt ein Sweep.

Der Konstruktor erstellt die `SKPath` Objekt, das die Infinity-Zeichen beschreibt. Nachdem der Pfad erstellt wurde, kann der Konstruktor auch die rechteckigen Grenzen des Pfads abrufen. Klicken Sie dann einen Wert mit dem berechnet `gradientCycleLength`. Wenn ein Farbverlauf auf der oberen linken und rechten unteren Ecken ist die `pathBounds` Rechteck dies `gradientCycleLength` Wert ist die horizontale Gesamtbreite des Farbverlauf-Musters:

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    SKCanvasView canvasView;

    // Path information 
    SKPath infinityPath;
    SKRect pathBounds;
    float gradientCycleLength;

    // Gradient information
    SKColor[] colors = new SKColor[8];
    ···

    public InfinityColorsPage ()
    {
        Title = "Infinity Colors";

        // Create path for infinity sign
        infinityPath = new SKPath();
        infinityPath.MoveTo(0, 0);                                  // Center
        infinityPath.CubicTo(  50,  -50,   95, -100,  150, -100);   // To top of right loop
        infinityPath.CubicTo( 205, -100,  250,  -55,  250,    0);   // To far right of right loop
        infinityPath.CubicTo( 250,   55,  205,  100,  150,  100);   // To bottom of right loop
        infinityPath.CubicTo(  95,  100,   50,   50,    0,    0);   // Back to center  
        infinityPath.CubicTo( -50,  -50,  -95, -100, -150, -100);   // To top of left loop
        infinityPath.CubicTo(-205, -100, -250,  -55, -250,    0);   // To far left of left loop
        infinityPath.CubicTo(-250,   55, -205,  100, -150,  100);   // To bottom of left loop
        infinityPath.CubicTo( -95,  100, - 50,   50,    0,    0);   // Back to center
        infinityPath.Close();

        // Calculate path information 
        pathBounds = infinityPath.Bounds;
        gradientCycleLength = pathBounds.Width +
            pathBounds.Height * pathBounds.Height / pathBounds.Width;

        // Create SKColor array for gradient
        for (int i = 0; i < colors.Length; i++)
        {
            colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
        }

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Der Konstruktor erstellt auch die `colors` Array nach den Regenbogen hervorgebracht, und die `SKCanvasView` Objekt.

Der überschreibt die `OnAppearing` und `OnDisappearing` Methoden führen Sie den Aufwand für die Animation. Die `OnTimerTick` Methode erstellt eine Animation die `offset` Feld von 0 bis `gradientCycleLength` alle zwei Sekunden:

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    float offset;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 2;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        offset = (float)(gradientCycleLength * progress);
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Zum Schluss die `PaintSurface` Handler rendert die Infinity-Zeichen. Da der Pfad enthält, negative und positive Koordinaten Zusammenhang mit einen Mittelpunkt (0, 0), eine `Translate` Transformation im Zeichenbereich wird verwendet, um es in die Mitte verschoben. Die Verschiebungstransformation wird gefolgt von einem `Scale` Transformation, die einen Skalierungsfaktor, die die Vorzeichen unendlich so groß wie möglich macht angewendet, Speicherlaufwerke trotzdem 95 % der Breite und Höhe des Zeichenbereichs. 

Beachten Sie, dass die `STROKE_WIDTH` Konstante wird die Breite und Höhe des umschließenden Rechtecks Pfads hinzugefügt. Der Pfad wird Strichen gezeichnet werden mit einer Zeile mit diese Breite, damit die Größe des gerenderten unendlich, durch die halbe Breite auf allen vier Seiten erhöht wird:

```csharp
public class InfinityColorsPage : ContentPage
{
    const int STROKE_WIDTH = 50;
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transforms to shift path to center and scale to canvas size
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.95f * 
            Math.Min(info.Width / (pathBounds.Width + STROKE_WIDTH),
                     info.Height / (pathBounds.Height + STROKE_WIDTH)));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = STROKE_WIDTH;
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(pathBounds.Left, pathBounds.Top),
                                new SKPoint(pathBounds.Right, pathBounds.Bottom),
                                colors,
                                null,
                                SKShaderTileMode.Repeat,
                                SKMatrix.MakeTranslation(offset, 0));

            canvas.DrawPath(infinityPath, paint);
        }
    }
}
```

Sehen Sie sich die Punkte, die als die ersten beiden Argumente übergeben `SKShader.CreateLinearGradient`. Diese Punkte basieren auf den ursprünglichen Pfad umschließenden Rechtecks. Der erste Punkt ist (&ndash;250, &ndash;100) und die zweite (250, 100). Interne SkiaSharp, werden diese Punkte für die aktuelle Canvas-Transformation unterzogen, damit sie richtig mit den angezeigten unendlich auszurichten.

Ohne das letzte Argument für `CreateLinearGradient`, sehen Sie einen Rainbow Farbverlauf, der von oben links der Anmeldeseite unendlich nach unten rechts erstreckt. (Tatsächlich erweitert der Farbverlauf von der oberen linken Ecke auf der unteren rechten Ecke des umschließenden Rechtecks. Die gerenderte Infinity-Zeichen ist größer als das umschließende Rechteck um die Hälfte der `STROKE_WIDTH` Wert auf allen Seiten. Da der Gradient am Anfang und Ende Rot ist, und der Farbverlauf wird erstellt, mit `SKShaderTileMode.Repeat`, der Unterschied ist, nicht bemerkbar.)

Mit dieser letzten Argument `CreateLinearGradient`, das Verlaufsmuster kontinuierlich führt ein Sweep für das Image:

[![Infinity-Farben](linear-gradient-images/InfinityColors.png "unendlich Farben")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>Transparenz und Farbverläufen

Die Farben, die bei einem Farbverlauf beitragen können Transparenz integrieren. Anstatt ein Farbverlauf, der von einer Farbe in eine andere ausgeblendet wird, kann der Farbverlauf aus einer Farbe transparent eingeblendet. 

Sie können dieses Verfahren für einige interessante Effekte verwenden. Eines der klassische Beispiele zeigt eine graphische Objekt mit der Reflektion:

[![Reflektion Farbverlauf](linear-gradient-images/ReflectionGradient.png "Reflektion Farbverlauf")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Der Text, nach unten zeigende wird mit einem Farbverlauf Farbe gefüllt, die 50 % nach oben, um am unteren Rand vollständig transparent transparent ist. Diese Ebenen der Transparenz werden alpha-Werte von 0 x 80 und 0 zugeordnet.

Die `PaintSurface` Ereignishandler in der **Reflektion Farbverlauf** Seite skaliert die Größe des Texts auf 90 % der Breite des Zeichenbereichs. Klicken Sie dann berechnet `xText` und `yText` Werte zum Positionieren des Texts horizontal zentriert werden soll, aber waren für eine Baseline, die am vertikalen Mittelpunkt der Seite entspricht:

```csharp
public class ReflectionGradientPage : ContentPage
{
    const string TEXT = "Reflection";

    public ReflectionGradientPage ()
    {
        Title = "Reflection Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Set text color to blue
            paint.Color = SKColors.Blue;

            // Set text size to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to position text above center
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2;

            // Draw unreflected text
            canvas.DrawText(TEXT, xText, yText, paint);

            // Shift textBounds to match displayed text
            textBounds.Offset(xText, yText);

            // Use those offsets to create a gradient for the reflected text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, textBounds.Top),
                                new SKPoint(0, textBounds.Bottom),
                                new SKColor[] { paint.Color.WithAlpha(0),
                                                paint.Color.WithAlpha(0x80) },
                                null,
                                SKShaderTileMode.Clamp);

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Die `xText` und `yText` Werte sind die gleichen Werte für den reflektierten Text angezeigt werden soll die `DrawText` rufen Sie am unteren Rand der `PaintSurface` Handler. Kurz vor diesen Code, allerdings sehen Sie einen Aufruf der `Scale` -Methode der `SKCanvas`. Dies `Scale` Methode skaliert horizontal um 1 (die keine Aktion ausführt) aber vertikal um &ndash;1, was effektiv alles Kopf gekippt. Der Mittelpunkt der Drehung wird festgelegt, zu dem Punkt (0, `yText`), wobei `yText` vertikale Mitte des Zeichenbereichs, als ursprünglich berechnet wird `info.Height` geteilt durch 2.

Bedenken Sie, dass Skia Farbverlaufs verwendet, um grafische Objekte vor dem Canvas-Transformationen mit Farbe versehen. Nachdem der unreflected Text gezeichnet wird, die `textBounds` Rechteck wird verschoben, sodass es den angezeigten Text entspricht:

```csharp
textBounds.Offset(xText, yText);
```

Die `CreateLinearGradient` Aufruf einen Farbverlauf von der obersten Position dieses Rechtecks unten definiert. Der Gradient ist von einem vollständig transparent, Blau (`paint.Color.WithAlpha(0)`) eine 50 % transparente Blau (`paint.Color.WithAlpha(0x80)`). Die Leinwand Transformation spiegelt den Text verkehrt, damit die 50 % transparente blaue bei der Baseline beginnt und wird am oberen Rand des Texts transparent.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
