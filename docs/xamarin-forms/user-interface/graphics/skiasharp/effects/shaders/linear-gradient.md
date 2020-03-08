---
title: SkiaSharp linearen Farbverlaufs
description: Erfahren Sie, wie Zeichnen von Linien oder Bereiche mit Farbverläufen, bestehend aus schrittweise eine Mischung aus zwei Farben füllen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 290e533e54b2ee150b94d9fb6b0f5119324f9cf0
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916378"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp linearen Farbverlaufs

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die [`SKPaint`](xref:SkiaSharp.SKPaint) -Klasse definiert eine [`Color`](xref:SkiaSharp.SKPaint.Color) Eigenschaft, die verwendet wird, um Linien zu zeichnen oder Bereiche mit einer voll Tonfarbe auszufüllen. Sie können auch Linien oder Bereiche mit _Farbverläufen_, bei denen es sich um schrittweise Farben Farben handelt, in einem Strich zeichnen:

![Beispiel für linearen Farbverlauf](linear-gradient-images/LinearGradientSample.png "Beispiel für linearen Farbverlauf")

Die grundlegendste Art des Farbverlaufs ist ein _linearer_ Farbverlauf. Die Blend der Farben tritt in einer Zeile (die als _Farbverlaufslinie_bezeichnet wird) von einem Punkt zu einem anderen auf. Zeilen, die senkrecht zur der Farbverlaufslinie sind aufweisen die gleiche Farbe. Sie erstellen einen linearen Farbverlauf mit einer der beiden statischen [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) Methoden. Der Unterschied zwischen den zwei Überladungen ist, dass eine eine Matrixtransformation enthält und das andere nicht. 

Diese Methoden geben ein Objekt vom Typ [`SKShader`](xref:SkiaSharp.SKShader) zurück, das Sie auf die [`Shader`](xref:SkiaSharp.SKPaint.Shader) -Eigenschaft von `SKPaint`festgelegt haben. Wenn die `Shader`-Eigenschaft nicht NULL ist, überschreibt Sie die `Color`-Eigenschaft. Jede Zeile, die mit Strichen ist, oder ein beliebiger Bereich, der mit diesem `SKPaint` Objekt gefüllt wird, basiert auf dem Farbverlauf und nicht auf der voll Tonfarbe.

> [!NOTE]
> Die `Shader`-Eigenschaft wird ignoriert, wenn Sie ein `SKPaint` Objekt in einen `DrawBitmap`-Befehl einschließen. Mit der `Color`-Eigenschaft von `SKPaint` können Sie eine Transparenz Ebene für die Anzeige einer Bitmap festlegen (wie im Artikel [Anzeigen von skiasharp-Bitmaps](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)beschrieben), aber Sie können die `Shader`-Eigenschaft nicht verwenden, um eine Bitmap mit einer Farbverlaufs Transparenz anzuzeigen. Es stehen andere Techniken zum Anzeigen von Bitmaps mit Verlaufs Übersichten zur Verfügung: Diese werden in den Artikeln [skiasharp-zirkuläre Farbverläufe](circular-gradients.md#radial-gradients-for-masking) und [skiasharp Zusammensetzung-und Blend-Modi](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)beschrieben.

## <a name="corner-to-corner-gradients"></a>Ecke-zu-Ecke Farbverläufe

Erstreckt sich häufig ein linearer Farbverlauf von einer Ecke eines Rechtecks zu einem anderen. Wenn Sie der Startpunkt auf der linken oberen Ecke des Rechtecks ist, kann Farbverlaufs erweitern:

- vertikal auf der linken unteren Ecke.
- horizontal auf der oberen rechten Ecke
- auf der unteren rechten Ecke diagonal

Der lineare lineare Farbverlauf wird auf der ersten Seite im Abschnitt **skiasharp-Shader und andere Effekte** des [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiels veranschaulicht. Die Seite " **Corner-to-Corner-Farbverlauf** " erstellt eine `SKCanvasView` im Konstruktor. Der `PaintSurface` Handler erstellt ein `SKPaint`-Objekt in einer `using`-Anweisung und definiert dann ein in der Canvas zentriertes 300-Pixel-quadratisches Rechteck:

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

Der `Shader`-Eigenschaft von `SKPaint` wird der `SKShader` Rückgabewert aus der statischen `SKShader.CreateLinearGradient`-Methode zugewiesen. Die fünf Argumente sind wie folgt aus:

- Der Ausgangspunkt des Farbverlaufs entspricht, hier auf der linken oberen Ecke des Rechtecks festgelegt.
- Der Endpunkt des Farbverlaufs entspricht, hier auf der unteren rechten Ecke des Rechtecks festgelegt.
- Ein Array von zwei oder mehr Farben, die bei der Farbverlauf beitragen
- Ein Array von `float` Werten, das die relative Position der Farben in der Farbverlaufslinie angibt.
- Ein Member der [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) Enumeration, die angibt, wie sich der Farbverlauf über die Enden der Farbverlaufslinie hinaus verhält.

Nachdem das Gradient-Objekt erstellt wurde, zeichnet die `DrawRect`-Methode das quadratische Rechteck 300 Pixel mithilfe des `SKPaint` Objekts, das den Shader einschließt. Hier wird die Anwendung unter iOS, Android und die universelle Windows-Plattform (UWP) ausgeführt:

[![Farbverlauf (Ecke und Ecke)](linear-gradient-images/CornerToCornerGradient.png "Farbverlauf (Ecke und Ecke)")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

Der Farbverlaufslinie wird durch die beiden Punkte, die als die ersten beiden Argumente definiert. Beachten Sie, dass diese Punkte _relativ zum Zeichenbereich und_ _nicht_ zum grafischen Objekt sind, das mit dem Farbverlauf angezeigt wird. Entlang der Farbverlaufslinie wechselt die Farbe allmählich von Rot in der oberen linken Ecke unter der unteren rechten Ecke Blau. Jede Zeile, die senkrecht zur der Farbverlaufslinie hat es sich um eine Konstante Farbe.

Das Array der `float` Werte, die als viertes Argument angegeben sind, hat eine eins-zu-eins-Entsprechung mit dem Array von Farben. Die Werte geben die relative Position entlang der Farbverlaufslinie, in denen diese Farben auftreten. Der Wert 0 bedeutet, dass `Red` am Anfang der Farbverlaufslinie auftritt, und 1 bedeutet, dass `Blue` am Ende der Zeile auftritt. Die Zahlen müssen aufsteigend sein und müssen im Bereich von 0 bis 1. Wenn sie in diesem Bereich nicht sind, werden sie in diesem Bereich werden angepasst.

Die beiden Werte im Array können auf einen anderen Wert als 0 und 1 festgelegt werden. Versuchen Sie Folgendes:

```csharp
new float[] { 0.25f, 0.75f }
```

Jetzt ist die gesamte erste Quartal der Farbverlaufslinie reines Rot, und das letzte Quartal ist reine Blau. Die Kombination von Red Team und Blue ist auf der zentrale Teil der Farbverlaufslinie beschränkt.

Im Allgemeinen sollten Sie diese Positionswerte gleichermaßen von 0 auf 1 Leerzeichen. Wenn dies der Fall ist, können Sie einfach `null` als viertes Argument bereitstellen, um `CreateLinearGradient`.

Obwohl diese Farbverlauf zwischen beiden Ecken des Rechtecks quadratischen 300 Pixel definiert ist, ist es nicht auf dieses Rechteck ausfüllen beschränkt. Die **Farbverlaufs Seite "Corner-to-Corner** " enthält zusätzlichen Code, der auf tippen oder Mausklicks auf der Seite antwortet. Das `drawBackground` Feld wird zwischen `true` und `false` bei jedem tippen ein-und ausgeschaltet. Wenn der Wert `true`ist, verwendet der `PaintSurface` Handler dasselbe `SKPaint` Objekt, um den gesamten Zeichenbereich auszufüllen, und zeichnet dann ein schwarzes Rechteck, das das kleinere Rechteck angibt: 

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

[![Farbverlauf von Ecke zu Ecke voll](linear-gradient-images/CornerToCornerGradientFull.png "Farbverlauf von Ecke zu Ecke voll")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

Beachten Sie, dass der Farbverlauf in dem gleichen Muster über die Punkte, die Definition der Farbverlaufslinie wiederholt. Diese Wiederholung tritt auf, weil das letzte Argument für `CreateLinearGradient` `SKShaderTileMode.Repeat`ist. (Sie müssen die anderen Optionen in Kürze sehen.)

Beachten Sie außerdem, dass die Punkte, die Sie mithilfe der Farbverlaufslinie angeben nicht eindeutig sind. Zeilen, die senkrecht zur der Farbverlaufslinie sind haben die gleiche Farbe, daher gibt es eine unendliche Anzahl von Farbverlauf Zeilen, die Sie für die gleiche Auswirkung angeben können. Beispielsweise können bei der ein Rechteck mit einem horizontalen Farbverlauf zu füllen, Sie angeben der oberen linken und rechten oberen Ecken oder den unteren linken und rechten unteren Ecken oder beliebigen zwei Punkten, die selbst und parallel zur diese Zeilen sind.

## <a name="interactively-experiment"></a>Interaktives experimentieren.

Sie können interaktiv mit linearen Farbverläufen mit der interaktiven Seite für den **linearen Farbverlauf** experimentieren. Auf dieser Seite wird die `InteractivePage`-Klasse verwendet, die im Artikel [**drei Methoden zum Zeichnen eines Bogens**](../../curves/arcs.md)vorgestellt wurde. `InteractivePage` behandelt [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) Ereignisse, um eine Sammlung von `TouchPoint` Objekten zu verwalten, die Sie mit den Fingern oder der Maus bewegen können.

Die XAML-Datei fügt die `TouchEffect` an ein übergeordnetes Element des `SKCanvasView` an und enthält außerdem eine `Picker`, die Ihnen ermöglicht, eines der drei Member der [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) -Enumeration auszuwählen:

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

Der Konstruktor in der Code-Behind-Datei erstellt zwei `TouchPoint`-Objekten für die Start-und Endpunkte des linearen Farbverlaufs. Der `PaintSurface`-Handler definiert ein Array aus drei Farben (für einen Farbverlauf von rot zu grün bis blau) und ruft die aktuelle `SKShaderTileMode` aus dem `Picker`ab:

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

Der `PaintSurface` Handler erstellt das `SKShader`-Objekt aus allen diesen Informationen und verwendet es, um den gesamten Zeichenbereich zu färben. Das Array von `float` Werten ist auf `null`festgelegt. Drei Farben gleich sein soll, würden Sie andernfalls diesen Parameter in ein Array mit den Werten 0, 0,5 und 1 festlegen.

Der Großteil des `PaintSurface` Handlers ist für das Anzeigen von mehreren Objekten vorgesehen: die Berührungspunkte als Gliederungs Kreise, die Farbverlaufslinie und die Linien, die senkrecht zu den Farbverlaufs Linien an den Berührungspunkten stehen:

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

Die senkrecht Zeilen übereinstimmen, mit dem Anfang und Ende des Farbverlaufs. Was über diese Zeilen hinausgeht, hängt von der Einstellung der `SKShaderTileMode`-Enumeration ab:

[![Interaktiver linearer Farbverlauf](linear-gradient-images/InteractiveLinearGradient.png "Interaktiver linearer Farbverlauf")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

Die drei Screenshots zeigen die Ergebnisse der drei verschiedenen Werte [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode). Der IOS-Screenshot zeigt `SKShaderTileMode.Clamp`an, der nur die Farben auf den Rand des Farbverlaufs erweitert. Die Option `SKShaderTileMode.Repeat` im Android-Bildschirmfoto zeigt, wie das Farbverlaufs Muster wiederholt wird. Die Option `SKShaderTileMode.Mirror` im UWP-Screenshot wiederholt auch das Muster. das Muster wird jedoch jedes Mal umgekehrt, was zu keinen Farb Diskontinuitäten führt.

## <a name="gradients-on-gradients"></a>Farbverläufe in Farbverläufen

Die `SKShader`-Klasse definiert keine öffentlichen Eigenschaften oder Methoden mit Ausnahme von `Dispose`. Die `SKShader` Objekte, die von den statischen Methoden erstellt wurden, sind daher unveränderlich. Auch wenn Sie den gleichen Farbverlauf für zwei verschiedene Objekte verwenden, ist es wahrscheinlich ist, Sie sollten den Farbverlauf weichen geringfügig ab. Zu diesem Zweck müssen Sie ein neues `SKShader` Objekt erstellen.

Auf der **Textseite Farbverlauf** werden Text und ein Hintergrund angezeigt, die beide mit ähnlichen Farbverläufen gefärbt sind:

[![Farbverlaufs Text](linear-gradient-images/GradientText.png "Farbverlaufs Text")](linear-gradient-images/GradientText-Large.png#lightbox)

Der einzige Unterschied in der Farbverläufe sind die Start- und Endpunkt. Zum Anzeigen von Text verwendeten Farbverlaufs basiert auf zwei Punkten auf die Ecken des umschließenden Rechtecks für den Text. Für den Hintergrund basiert die beiden Punkte auf den gesamten Zeichenbereich. Der Code lautet wie folgt:

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

Die `Shader`-Eigenschaft des `SKPaint` Objekts wird zuerst festgelegt, um einen Farbverlauf zum Abdecken des Hintergrunds anzuzeigen. Die Farbverlauf-Punkte werden auf der oberen linken und rechten unteren Ecken der Leinwand festgelegt.

Im Code wird die `TextSize`-Eigenschaft des `SKPaint`-Objekts festgelegt, sodass der Text bei 90% der Breite des Zeichen Bereichs angezeigt wird. Die Text Begrenzungen werden verwendet, um `xText` und `yText` Werte zu berechnen, die an die `DrawText`-Methode übergeben werden, um den Text zu zentrieren.

Die Farbverlaufs Punkte für den zweiten `CreateLinearGradient`-Aufrufwert müssen jedoch in der oberen linken und unteren rechten Ecke des Texts relativ zum Zeichenbereich liegen, wenn er angezeigt wird. Dies wird erreicht, indem das `textBounds` Rechteck um denselben `xText` und `yText` Werte verschoben wird:

```csharp
textBounds.Offset(xText, yText);
```

Nachdem der oberen linken und rechten unteren Ecken des Rechtecks zum Festlegen der Start- und Endpunkt des Farbverlaufs verwendet werden können.

## <a name="animating-a-gradient"></a>Animieren eines Farbverlaufs

Es gibt mehrere Möglichkeiten, die einen Farbverlauf zu animieren. Ein Ansatz darin, die Start- und Endpunkt zu animieren. Die Seite für die **Verlaufs Animation** verschiebt die zwei Punkte in einem Kreis, der auf den Zeichenbereich zentriert ist. Der Radius der diesen Vertrauenskreis aufgenommen wird, die Hälfte der Breite oder Höhe des Zeichenbereichs, welcher Wert kleiner ist. Die Start-und Endpunkte sind in diesem Kreis gegenseitig zueinander, und der Farbverlauf wechselt von weiß zu schwarz mit einem `Mirror` Kachel Modus:

[![Farbverlaufs Animation](linear-gradient-images/GradientAnimation.png "Farbverlaufs Animation")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

Der-Konstruktor erstellt die `SKCanvasView`. Die Methoden `OnAppearing` und `OnDisappearing` behandeln die Animations Logik:

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

Mit der `OnTimerTick`-Methode wird ein `angle` Wert berechnet, der alle drei Sekunden zwischen 0 und 2 (null) animiert wird. 

Hier ist eine Möglichkeit, um den Farbverlauf zwei Punkten zu berechnen. Ein `SKPoint` Wert mit dem Namen `vector` wird so berechnet, dass er von der Mitte des Zeichen Bereichs auf einen Punkt auf dem Radius des Kreises erweitert wird. Die Richtung dieses Vektors basiert auf den Werten Sinus und Cosinus des Winkels. Die beiden umgekehrter Farbverlauf Punkte werden dann berechnet: ein Punkt wird berechnet, indem Sie diesem Vektor vom Mittelpunkt subtrahiert und weiterer Punkt wird berechnet, indem Sie den Mittelpunkt den Vektor hinzugefügt:

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

Ein etwas anderer Ansatz ist weniger Code erforderlich. Bei diesem Ansatz wird die [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladungs Methode mit einer Matrix Transformation als letztes Argument verwendet. Diese Vorgehensweise ist die Version im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel:

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

Wenn die Breite des Zeichen Bereichs kleiner ist als die Höhe, werden die zwei Farbverlaufs Punkte auf (0,0) und (`info.Width`, 0) festgelegt. Die Drehung der Drehung, die als letztes Argument an `CreateLinearGradient` übertragen wird, dreht diese beiden Punkte praktisch um den Mittelpunkt des Bildschirms.

Beachten Sie, dass, wenn der Winkel 0 ist, es keine Drehung gibt, und die zwei Farbverlauf der oberen linken und rechten oberen Ecken des Zeichenbereichs. Diese Punkte sind nicht die gleichen gradientenpunkte, die berechnet wurden, wie im vorherigen `CreateLinearGradient`-Aufrufe gezeigt. Diese Punkte sind jedoch _parallel_ zur horizontalen Farbverlaufslinie, die den Mittelpunkt der Canvas schneidet, und führen zu einem identischen Farbverlauf.

**Regenbogen Verlauf**

Die Seite " **Regenbogen Verlauf** " zeichnet einen Regenbogen Bereich von der oberen linken Ecke des Zeichen Bereichs in der unteren rechten Ecke. Aber diese Rainbow Farbverlauf nicht wie eine echte Rainbow. Direkt, anstatt gekrümmt ist, aber sie basiert auf acht HSL (Farbton-Sättigung-Helligkeit)-Farben, die durch Durchlaufen der Hue-Werte zwischen 0 und 360 bestimmt werden:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

Dieser Code ist Teil des unten gezeigten `PaintSurface` Handlers. Der Handler beginnt mit dem Erstellen eines Pfads, das ein Polygon sechsseitiger definiert, die an der unteren rechten Ecke aus der linken oberen Ecke des Zeichenbereichs erweitert:

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

Die zwei Farbverlaufs Punkte in der `CreateLinearGradient`-Methode basieren auf zwei der Punkte, die diesen Pfad definieren: beide Punkte befinden sich in der Nähe der oberen linken Ecke. Die erste ist, auf den oberen Rand des Zeichenbereichs, und die zweite ist am linken Rand des Zeichenbereichs. Das Ergebnis lautet wie folgt:

[![Fehler beim Regenbogen Verlauf](linear-gradient-images/RainbowGradientFaulty.png "Fehler beim Regenbogen Verlauf")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

Dies ist eine interessante Image, aber es ist nicht sehr von der Absicht. Das Problem ist, dass bei einen linearen Farbverlauf zu erstellen, die Zeilen der Konstante Farbe senkrecht zur der Farbverlaufslinie sind. Der Farbverlaufslinie basiert auf dem, in dem in der Abbildung berührt die oberen und linken Rand, und diese Zeile in der Regel nicht senkrecht an den Rändern des in der Abbildung, die auf der unteren rechten Ecke erweitern. Dieser Ansatz funktioniert nur, wenn im Zeichenbereich quadratischen wurden.

Um eine ordnungsgemäße Rainbow Farbverlauf zu erstellen, muss der Farbverlaufslinie senkrecht an den Rand des den Regenbogen hervorgebracht. Dies ist eine komplexere Berechnung. Ein Vektor muss definiert werden, die parallel zu der langen Seite des in der Abbildung ist. Der Vektor ist um 90 Grad gedreht, sodass es senkrecht an dieser Seite ist. Anschließend wird die Breite der Abbildung durch Multiplizieren mit `rainbowWidth`verlängert. Die beiden Farbverlauf Punkte auf einen Punkt im Zweifelsfall in der Abbildung werden berechnet, und, sowie den Vektor verweisen. Dies ist der Code, der auf der Seite " **Regenbogen Verlauf** " im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel angezeigt wird:

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

[![Regenbogen Verlauf](linear-gradient-images/RainbowGradient.png "Regenbogen Verlauf")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**Unendlich Farben**

Ein Regenbogen Farbverlauf wird auch auf der Seite **unendlich Farben** verwendet. Diese Seite zeichnet ein Unendlichkeitszeichen mithilfe eines Pfad Objekts, das im Artikel [**drei Typen von Bézier-Kurven**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)beschrieben wird. Das Bild wird dann mit einem Farbverlauf animierte Rainbow gefärbt, der kontinuierlich auf das Bild führt ein Sweep.

Der-Konstruktor erstellt das `SKPath`-Objekt, das das Unendlichkeitszeichen beschreibt. Nachdem der Pfad erstellt wurde, kann der Konstruktor auch die rechteckigen Grenzen des Pfads abrufen. Anschließend wird ein Wert mit dem Namen `gradientCycleLength`berechnet. Wenn ein Farbverlauf auf der oberen linken und der unteren rechten Ecke des `pathBounds` Rechtecks basiert, ist dieser `gradientCycleLength` Wert die horizontale Gesamtbreite des Farbverlaufs Musters:

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

Der Konstruktor erstellt auch das `colors` Array für den Regenbogen Wert und das `SKCanvasView`-Objekt.

Durch außer Kraft setzungen der Methoden `OnAppearing` und `OnDisappearing` wird der Aufwand für die Animation verursacht. Mit der `OnTimerTick`-Methode wird das `offset` Feld von 0 bis `gradientCycleLength` alle zwei Sekunden animiert:

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

Zum Schluss rendert der `PaintSurface` Handler das Unendlichkeitszeichen. Da der Pfad negative und positive Koordinaten im Zusammenhang mit einem Mittelpunkt von (0,0) enthält, wird eine `Translate` Transformation im Zeichenbereich verwendet, um Sie in den Mittelpunkt zu verschieben. Auf die Transform-Transformation folgt eine `Scale` Transformation, die einen Skalierungsfaktor anwendet, der das Unendlichkeitszeichen so groß wie möglich macht, während gleichzeitig innerhalb von 95% der Breite und Höhe der Canvas bleibt. 

Beachten Sie, dass die `STROKE_WIDTH` Konstante der Breite und Höhe des umgebenden Rechtecks für den Pfad hinzugefügt wird. Der Pfad wird Strichen gezeichnet werden mit einer Zeile mit diese Breite, damit die Größe des gerenderten unendlich, durch die halbe Breite auf allen vier Seiten erhöht wird:

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

Sehen Sie sich die Punkte an, die als die ersten beiden Argumente von `SKShader.CreateLinearGradient`werden. Diese Punkte basieren auf den ursprünglichen Pfad umschließenden Rechtecks. Der erste Punkt ist (&ndash;250, &ndash;100) und der zweite Punkt (250, 100). Interne SkiaSharp, werden diese Punkte für die aktuelle Canvas-Transformation unterzogen, damit sie richtig mit den angezeigten unendlich auszurichten.

Ohne das letzte Argument für `CreateLinearGradient`wird ein Regenbogen Farbverlauf angezeigt, der von der linken oberen Ecke des unendlichen Zeichens nach unten rechts reicht. (Tatsächlich erweitert der Farbverlauf von der oberen linken Ecke auf der unteren rechten Ecke des umschließenden Rechtecks. Das gerenderte Unendlichkeitszeichen ist größer als das umgebende Rechteck um die Hälfte des `STROKE_WIDTH` Werts auf allen Seiten. Da der Farbverlauf am Anfang und am Ende rot ist und der Farbverlauf mit `SKShaderTileMode.Repeat`erstellt wird, ist der Unterschied nicht erkennbar.)

Mit dem letzten Argument für `CreateLinearGradient`wird das Farbverlaufs Muster fortlaufend über das Bild hinweg durchgängig:

[![Unendlich Farben](linear-gradient-images/InfinityColors.png "Unendlich Farben")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>Transparenz und Farbverläufen

Die Farben, die bei einem Farbverlauf beitragen können Transparenz integrieren. Anstatt ein Farbverlauf, der von einer Farbe in eine andere ausgeblendet wird, kann der Farbverlauf aus einer Farbe transparent eingeblendet. 

Sie können dieses Verfahren für einige interessante Effekte verwenden. Eines der klassische Beispiele zeigt eine graphische Objekt mit der Reflektion:

[![Reflektionstyp](linear-gradient-images/ReflectionGradient.png "Reflektionstyp")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Der Text, nach unten zeigende wird mit einem Farbverlauf Farbe gefüllt, die 50 % nach oben, um am unteren Rand vollständig transparent transparent ist. Diese Ebenen der Transparenz werden alpha-Werte von 0 x 80 und 0 zugeordnet.

Der `PaintSurface` Handler auf der Seite **Reflektionstyp** skaliert die Größe des Texts auf 90% der Breite des Zeichen Bereichs. Anschließend werden `xText`-und `yText` Werte berechnet, um den Text horizontal zentriert zu positionieren, aber auf einer Baseline zu positionieren, die der vertikalen Mitte der Seite entspricht:

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

Diese `xText` und `yText` Werte sind die gleichen Werte, die verwendet werden, um den reflektierten Text im `DrawText`-aufrufen am unteren Rand des `PaintSurface` Handlers anzuzeigen. Direkt vor diesem Code sehen Sie jedoch einen aufzurufenden `Scale` Methode `SKCanvas`. Diese `Scale` Methode wird horizontal um 1 skaliert (was nichts bewirkt), aber vertikal durch &ndash;1, wodurch alle Elemente in der Mitte nach oben gedreht werden. Der Mittelpunkt der Drehung wird auf den Punkt (0, `yText`) festgelegt, wobei `yText` die vertikale Mitte der Canvas ist, die ursprünglich als `info.Height` dividiert durch 2 berechnet wurde.

Bedenken Sie, dass Skia Farbverlaufs verwendet, um grafische Objekte vor dem Canvas-Transformationen mit Farbe versehen. Nachdem der nicht reflektierte Text gezeichnet wurde, wird das `textBounds` Rechteck so verschoben, dass es dem angezeigten Text entspricht:

```csharp
textBounds.Offset(xText, yText);
```

Der `CreateLinearGradient`-Befehl definiert einen Farbverlauf vom oberen Rand des Rechtecks bis zum unteren Rand. Der Farbverlauf liegt zwischen einem vollständig transparenten blauen (`paint.Color.WithAlpha(0)`) und einem transparenten (`paint.Color.WithAlpha(0x80)`) 50%-blau (). Die Leinwand Transformation spiegelt den Text verkehrt, damit die 50 % transparente blaue bei der Baseline beginnt und wird am oberen Rand des Texts transparent.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
