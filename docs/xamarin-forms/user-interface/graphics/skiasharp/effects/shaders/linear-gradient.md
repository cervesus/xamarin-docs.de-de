---
Title: "The skiasharp Linear Gradient" Description: "Discover line to Stroke Lines or Fill Areas with Gradienten, bestehend aus einer schrittweisen Mischung aus zwei Farben."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: 20a2a8c4-feb7-478d-bf57-c92e26117b6a Author: davidbritch ms. Author: dabritch ms. Date: 08/23/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-skiasharp-linear-gradient"></a>Der lineare skiasharp-Farbverlauf

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die- [`SKPaint`](xref:SkiaSharp.SKPaint) Klasse definiert eine- [`Color`](xref:SkiaSharp.SKPaint.Color) Eigenschaft, die zum Zeichnen von Linien oder Ausfüllen von Bereichen mit einer voll Tonfarbe verwendet wird. Sie können auch Linien oder Bereiche mit _Farbverläufen_, bei denen es sich um schrittweise Farben Farben handelt, in einem Strich zeichnen:

![Beispiel für linearen Farbverlauf](linear-gradient-images/LinearGradientSample.png "Beispiel für linearen Farbverlauf")

Die grundlegendste Art des Farbverlaufs ist ein _linearer_ Farbverlauf. Die Blend der Farben tritt in einer Zeile (die als _Farbverlaufslinie_bezeichnet wird) von einem Punkt zu einem anderen auf. Zeilen, die auf der Farbverlaufslinie senkrecht sind, haben die gleiche Farbe. Sie erstellen einen linearen Farbverlauf mit einer der beiden statischen [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) Methoden. Der Unterschied zwischen den zwei über Ladungen besteht darin, dass eine Matrix Transformation enthält und die andere nicht. 

Diese Methoden geben ein Objekt vom Typ zurück [`SKShader`](xref:SkiaSharp.SKShader) , das Sie auf die-Eigenschaft von festgelegt haben [`Shader`](xref:SkiaSharp.SKPaint.Shader) `SKPaint` . Wenn die `Shader` Eigenschaft nicht NULL ist, wird die-Eigenschaft überschrieben `Color` . Jede Zeile, die mit Strichen ist, oder ein beliebiger Bereich, der mit diesem Objekt gefüllt wird, `SKPaint` basiert auf dem Farbverlauf und nicht auf der voll Tonfarbe.

> [!NOTE]
> Die- `Shader` Eigenschaft wird ignoriert, wenn Sie ein `SKPaint` Objekt in einen- `DrawBitmap` Rückruf einschließen. Mithilfe der- `Color` Eigenschaft von können Sie `SKPaint` eine Transparenz Ebene für die Anzeige einer Bitmap festlegen (wie im Artikel [Anzeigen von skiasharp-Bitmaps](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)beschrieben), aber Sie können die-Eigenschaft nicht verwenden, `Shader` um eine Bitmap mit einer Farbverlaufs Transparenz anzuzeigen. Es stehen andere Techniken zum Anzeigen von Bitmaps mit Verlaufs Übersichten zur Verfügung: Diese werden in den Artikeln [skiasharp-zirkuläre Farbverläufe](circular-gradients.md#radial-gradients-for-masking) und [skiasharp Zusammensetzung-und Blend-Modi](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)beschrieben.

## <a name="corner-to-corner-gradients"></a>Ecken-zu-Ecke-Farbverläufe

Häufig wird ein linearer Farbverlauf von einer Ecke eines Rechtecks zu einem anderen erweitert. Wenn der Startpunkt die linke obere Ecke des Rechtecks ist, kann der Farbverlauf Folgendes erweitern:

- vertikal in der unteren linken Ecke
- horizontal zur oberen rechten Ecke
- Diagonal in der unteren rechten Ecke

Der lineare lineare Farbverlauf wird auf der ersten Seite im Abschnitt **skiasharp-Shader und andere Effekte** des [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiels veranschaulicht. Auf der Seite " **Corner-to-Corner-Farbverlauf** " wird ein-Element `SKCanvasView` im Konstruktor erstellt. Der `PaintSurface` Handler erstellt `SKPaint` in einer `using` -Anweisung ein-Objekt und definiert dann ein quadratisches 300-Pixel-Rechteck, das sich in der Canvas zentriert:

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

Der- `Shader` Eigenschaft von `SKPaint` wird der `SKShader` Rückgabewert der statischen- `SKShader.CreateLinearGradient` Methode zugewiesen. Die fünf Argumente lauten wie folgt:

- Der Startpunkt des Farbverlaufs, hier auf die obere linke Ecke des Rechtecks festgelegt.
- Der Endpunkt des Farbverlaufs, hier auf die untere rechte Ecke des Rechtecks festgelegt.
- Ein Array aus mindestens zwei Farben, die zum Farbverlauf beitragen.
- Ein Array von- `float` Werten, das die relative Position der Farben in der Farbverlaufslinie angibt.
- Ein Member der- [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) Enumeration, der angibt, wie sich der Farbverlauf hinter den Enden der Farbverlaufslinie verhält.

Nachdem das Gradient-Objekt erstellt wurde, `DrawRect` zeichnet die Methode das quadratische Rechteck 300 Pixel mithilfe des `SKPaint` Objekts, das den Shader einschließt. Hier wird Sie unter IOS, Android und der universelle Windows-Plattform (UWP) ausgeführt:

[![Farbverlauf (Ecke und Ecke)](linear-gradient-images/CornerToCornerGradient.png "Farbverlauf (Ecke und Ecke)")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

Die Farbverlaufslinie wird von den beiden Punkten definiert, die als die ersten beiden Argumente angegeben sind. Beachten Sie, dass diese Punkte _relativ zum Zeichenbereich und_ _nicht_ zum grafischen Objekt sind, das mit dem Farbverlauf angezeigt wird. Entlang der Farbverlaufslinie wechselt die Farbe allmählich von rot oben links zu blau unten rechts. Jede Zeile, die in der Farbverlaufslinie senkrecht ist, weist eine Konstante Farbe auf.

Das Array von `float` Werten, die als viertes Argument angegeben sind, hat eine eins-zu-eins-Entsprechung mit dem Array von Farben. Die Werte geben die relative Position entlang der Farbverlaufslinie an, in der diese Farben vorkommen. Hier bedeutet das 0, dass `Red` am Anfang der Farbverlaufslinie auftritt, und 1 bedeutet, dass `Blue` am Ende der Zeile auftritt. Die Zahlen müssen aufsteigend sein und müssen im Bereich zwischen 0 und 1 liegen. Wenn Sie sich nicht in diesem Bereich befinden, werden Sie so angepasst, dass Sie in diesem Bereich liegen.

Die beiden Werte im-Array können auf einen anderen Wert als 0 und 1 festgelegt werden. Versuchen Sie Folgendes:

```csharp
new float[] { 0.25f, 0.75f }
```

Nun ist das gesamte erste Quartal der Farbverlaufslinie rein rot, und das letzte Quartal ist rein blau. Die Mischung aus rot und blau ist auf die zentrale Hälfte der Farbverlaufslinie beschränkt.

Im Allgemeinen sollten Sie diese Positionswerte gleichmäßig zwischen 0 und 1 platzieren. Wenn dies der Fall ist, können Sie einfach `null` als viertes Argument für angeben `CreateLinearGradient` .

Obwohl dieser Farbverlauf zwischen zwei Ecken des quadratischen Rechtecks 300 Pixel definiert ist, ist es nicht darauf beschränkt, dieses Rechteck zu füllen. Die **Farbverlaufs Seite "Corner-to-Corner** " enthält zusätzlichen Code, der auf tippen oder Mausklicks auf der Seite antwortet. Das `drawBackground` Feld wird zwischen `true` und mit jedem tippen ein-und ausgeschaltet `false` . Wenn der Wert ist `true` , verwendet der `PaintSurface` Handler das gleiche `SKPaint` -Objekt, um den gesamten Zeichenbereich auszufüllen, und zeichnet dann ein schwarzes Rechteck, das das kleinere Rechteck angibt: 

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

Nach dem Tippen auf den Bildschirm sehen Sie Folgendes:

[![Farbverlauf von Ecke zu Ecke voll](linear-gradient-images/CornerToCornerGradientFull.png "Farbverlauf von Ecke zu Ecke voll")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

Beachten Sie, dass sich der Farbverlauf im gleichen Muster wiederholt, als die Punkte, die die Verlaufs Linie definieren. Diese Wiederholung tritt auf, weil das letzte Argument für `CreateLinearGradient` ist `SKShaderTileMode.Repeat` . (In Kürze werden die anderen Optionen angezeigt.)

Beachten Sie auch, dass die Punkte, mit denen Sie die Farbverlaufslinie angeben, nicht eindeutig sind. Zeilen, die in der Farbverlaufslinie senkrecht sind, haben die gleiche Farbe, daher gibt es eine unendliche Anzahl von Farbverlaufs Linien, die Sie für denselben Effekt angeben können. Wenn Sie z. b. ein Rechteck mit einem horizontalen Farbverlauf füllen, können Sie die oberen linken und oberen rechten Ecken oder die unteren linken und unteren rechten Ecken oder alle zwei Punkte angeben, die auch mit und parallel zu diesen Zeilen sind.

## <a name="interactively-experiment"></a>Interaktiv experimentieren

Sie können interaktiv mit linearen Farbverläufen mit der interaktiven Seite für den **linearen Farbverlauf** experimentieren. Diese Seite verwendet die-Klasse, die `InteractivePage` im Artikel [**drei Methoden zum Zeichnen eines Bogens**](../../curves/arcs.md)vorgestellt wurde. `InteractivePage` behandelt [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) Ereignisse, um eine Auflistung von Objekten zu verwalten `TouchPoint` , die Sie mit den Fingern oder der Maus bewegen können.

Die XAML-Datei fügt das `TouchEffect` an ein übergeordnetes Element von an `SKCanvasView` und enthält außerdem eine, mit der `Picker` Sie eines der drei Member der- [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) Enumeration auswählen können:

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

Der Konstruktor in der Code-Behind-Datei erstellt zwei `TouchPoint` -Objekte für den Start-und Endpunkt des linearen Farbverlaufs. Der `PaintSurface` Handler definiert ein Array aus drei Farben (für einen Farbverlauf von rot zu grün bis blau) und ruft den aktuellen `SKShaderTileMode` von der ab `Picker` :

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

Der `PaintSurface` Handler erstellt das `SKShader` Objekt aus allen diesen Informationen und verwendet es, um den gesamten Zeichenbereich zu färben. Das Array von- `float` Werten wird auf festgelegt `null` . Wenn Sie andernfalls drei Farben gleichzeitig angeben möchten, legen Sie diesen Parameter auf ein Array mit den Werten 0, 0,5 und 1 fest.

Der Großteil des `PaintSurface` Handlers ist für das Anzeigen mehrerer Objekte vorgesehen: die Berührungspunkte als Gliederungs Kreise, die Farbverlaufslinie und die Linien, die senkrecht zu den Farbverlaufs Linien an den Berührungspunkten stehen:

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

Die Farbverlaufslinie, die die beiden Touchpoints verbindet, ist leicht zu zeichnen, aber die senkrechten Linien erfordern etwas mehr Arbeit. Die Farbverlaufslinie wird in einen Vektor konvertiert, normalisiert, um eine Länge von einer Einheit zu haben, und wird dann um 90 Grad gedreht. Dieser Vektor erhält dann eine Länge von 200 Pixeln. Es wird verwendet, um vier Linien zu zeichnen, die sich von den Berührungspunkten bis zu der Farbverlaufslinie erstrecken.

Die senkrechten Zeilen stimmen mit dem Anfang und dem Ende des Farbverlaufs überein. Was über diese Zeilen hinausgeht, hängt von der Einstellung der- `SKShaderTileMode` Enumeration ab:

[![Interaktiver linearer Farbverlauf](linear-gradient-images/InteractiveLinearGradient.png "Interaktiver linearer Farbverlauf")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

Die drei Screenshots zeigen die Ergebnisse der drei verschiedenen Werte von [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) . Der IOS-Screenshot zeigt `SKShaderTileMode.Clamp` , dass nur die Farben am Rand des Farbverlaufs erweitert werden. Die- `SKShaderTileMode.Repeat` Option im Android-Bildschirmfoto zeigt, wie das Farbverlaufs Muster wiederholt wird. Die- `SKShaderTileMode.Mirror` Option im UWP-Screenshot wiederholt auch das-Muster, das Muster wird jedoch jedes Mal umgekehrt, was zu keinen Farb Diskontinuitäten führt.

## <a name="gradients-on-gradients"></a>Farbverläufe in Farbverläufen

Die- `SKShader` Klasse definiert keine öffentlichen Eigenschaften oder Methoden mit Ausnahme von `Dispose` . Die `SKShader` Objekte, die von den statischen Methoden erstellt wurden, sind daher unveränderlich. Auch wenn Sie den gleichen Farbverlauf für zwei verschiedene Objekte verwenden, ist es wahrscheinlich, dass Sie den Farbverlauf leicht verändern möchten. Zu diesem Zweck müssen Sie ein neues- `SKShader` Objekt erstellen.

Auf der **Textseite Farbverlauf** werden Text und ein Hintergrund angezeigt, die beide mit ähnlichen Farbverläufen gefärbt sind:

[![Farbverlaufs Text](linear-gradient-images/GradientText.png "Farbverlaufs Text")](linear-gradient-images/GradientText-Large.png#lightbox)

Die einzigen Unterschiede in den Farbverläufen sind Start-und Endpunkte. Der zum Anzeigen von Text verwendete Farbverlauf basiert auf zwei Punkten in den Ecken des umgebenden Rechtecks für den Text. Für den Hintergrund basieren die beiden Punkte auf dem gesamten Zeichenbereich. Der Code lautet wie folgt:

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

Die- `Shader` Eigenschaft des- `SKPaint` Objekts wird zuerst festgelegt, um einen Farbverlauf zum Abdecken des Hintergrunds anzuzeigen. Die Farbverlaufs Punkte werden auf die oberen linken und unteren rechten Ecke des Zeichen Bereichs festgelegt.

Der Code legt die- `TextSize` Eigenschaft des- `SKPaint` Objekts fest, sodass der Text bei 90% der Breite des Zeichen Bereichs angezeigt wird. Die Text Begrenzungen werden verwendet, um Werte und Werte zu berechnen, die an `xText` `yText` die Methode übergeben werden, `DrawText` um den Text zu zentrieren.

Die Farbverlaufs Punkte für den zweiten `CreateLinearGradient` -Befehl müssen jedoch auf die linke obere und untere rechte Ecke des Texts relativ zum Zeichenbereich verweisen, wenn er angezeigt wird. Dies wird erreicht, indem das `textBounds` Rechteck um denselben `xText` -Wert und einen-Wert verschoben wird `yText` :

```csharp
textBounds.Offset(xText, yText);
```

Nun können die oberen linken und unteren rechten Ecke des Rechtecks verwendet werden, um die Anfangs-und Endpunkte des Farbverlaufs festzulegen.

## <a name="animating-a-gradient"></a>Animieren eines Farbverlaufs

Es gibt mehrere Möglichkeiten, einen Farbverlauf zu animieren. Ein Ansatz besteht darin, die Start-und Endpunkte zu animieren. Die Seite für die **Verlaufs Animation** verschiebt die zwei Punkte in einem Kreis, der auf den Zeichenbereich zentriert ist. Der RADIUS dieses Kreises entspricht der Hälfte der Breite oder Höhe des Canvas, je nachdem, welcher Wert kleiner ist. Die Start-und Endpunkte sind in diesem Kreis gegenseitig zueinander, und der Farbverlauf wechselt von weiß zu schwarz mit einem `Mirror` Kachel Modus:

[![Farbverlaufs Animation](linear-gradient-images/GradientAnimation.png "Farbverlaufs Animation")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

Der-Konstruktor erstellt `SKCanvasView` . Die `OnAppearing` - `OnDisappearing` Methode und die-Methode behandeln die Animations Logik:

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

Die- `OnTimerTick` Methode berechnet einen- `angle` Wert, der alle drei Sekunden zwischen 0 und 2 Zeichen animiert wird. 

Hier ist eine Möglichkeit, die zwei Farbverlaufs Punkte zu berechnen. Ein- `SKPoint` Wert `vector` mit dem Namen wird so berechnet, dass er von der Mitte des Zeichen Bereichs bis zu einem Punkt auf dem Radius des Kreises erweitert wird. Die Richtung dieses Vektors basiert auf dem Sinus-und Kosinus-Wert des Winkels. Anschließend werden die beiden umgekehrten Farbverlaufs Punkte berechnet: ein Punkt wird berechnet, indem der Vektor vom Mittelpunkt subtrahieren wird, und ein anderer Punkt wird berechnet, indem der Vektor dem Mittelpunkt hinzugefügt wird:

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

Ein etwas anderer Ansatz erfordert weniger Code. Bei diesem Ansatz wird die [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) Überladungs Methode mit einer Matrix Transformation als letztes Argument verwendet. Diese Vorgehensweise ist die Version im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel:

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

Wenn die Breite des Zeichen Bereichs kleiner ist als die Höhe, werden die zwei Farbverlaufs Punkte auf (0,0) und ( `info.Width` , 0) festgelegt. Die Drehungs Transformation, die als letztes Argument übertragen wird, um `CreateLinearGradient` diese beiden Punkte in der Mitte des Bildschirms zu drehen.

Beachten Sie Folgendes: Wenn der Winkel 0 ist, gibt es keine Drehung, und die zwei Farbverlaufs Punkte sind die oberen linken und oberen rechten Ecke der Canvas. Diese Punkte sind nicht die gleichen Farbverlaufs Punkte, wie im vorherigen-Befehl gezeigt `CreateLinearGradient` . Diese Punkte sind jedoch _parallel_ zur horizontalen Farbverlaufslinie, die den Mittelpunkt der Canvas schneidet, und führen zu einem identischen Farbverlauf.

**Regenbogen Verlauf**

Die Seite " **Regenbogen Verlauf** " zeichnet einen Regenbogen Bereich von der oberen linken Ecke des Zeichen Bereichs in der unteren rechten Ecke. Dieser Regenbogen Farbverlauf ist jedoch nicht mit einem echten Regenbogen Verlauf vergleichbar. Es ist nicht gekrümmte, sondern basiert auf acht Farben (Hue-Sättigung-luminosity), die durch das Durchlaufen der Hue-Werte zwischen 0 und 360 bestimmt werden:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

Dieser Code ist Teil des `PaintSurface` unten gezeigten Handlers. Der Handler erstellt zunächst einen Pfad, der ein sechsseitiges Polygon definiert, das sich von der oberen linken Ecke des Zeichen Bereichs bis zur unteren rechten Ecke erstreckt:

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

Die zwei Farbverlaufs Punkte in der `CreateLinearGradient` Methode basieren auf zwei der Punkte, die diesen Pfad definieren: beide Punkte befinden sich in der Nähe der oberen linken Ecke. Der erste befindet sich am oberen Rand des Zeichen Bereichs, und der zweite befindet sich am linken Rand der Canvas. So sieht das Ergebnis aus:

[![Fehler beim Regenbogen Verlauf](linear-gradient-images/RainbowGradientFaulty.png "Fehler beim Regenbogen Verlauf")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

Dabei handelt es sich um ein interessantes Bild, aber es ist nicht wirklich beabsichtigt. Das Problem besteht darin, dass beim Erstellen eines linearen Farbverlaufs die Linien der Konstanten Farbe senkrecht zur Farbverlaufslinie sind. Die Farbverlaufslinie basiert auf den Punkten, an denen die Figur die obere und linke Seite berührt, und diese Linie ist im Allgemeinen nicht senkrecht zu den Rändern der Abbildung, die sich auf die untere rechte Ecke ausdehnen. Dieser Ansatz funktioniert nur, wenn die Canvas quadratisch war.

Um einen korrekten Regenbogen Farbverlauf zu erstellen, muss die Farbverlaufslinie senkrecht zum Rand des Regenbogen Bereichs liegen. Dabei handelt es sich um eine detailliertere Berechnung. Es muss ein Vektor definiert werden, der parallel zur langen Seite der Abbildung ist. Der Vektor wird um 90 Grad gedreht, sodass er senkrecht zu dieser Seite ist. Anschließend wird Sie durch Multiplizieren von mit der Breite der Abbildung verlängert `rainbowWidth` . Die zwei Farbverlaufs Punkte werden basierend auf einem Punkt auf der Seite der Abbildung und diesem Punkt plus dem Vektor berechnet. Dies ist der Code, der auf der Seite " **Regenbogen Verlauf** " im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel angezeigt wird:

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

Nun werden die Regenbogenfarben an der Abbildung ausgerichtet:

[![Regenbogen Verlauf](linear-gradient-images/RainbowGradient.png "Regenbogen Verlauf")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**Unendlich Farben**

Ein Regenbogen Farbverlauf wird auch auf der Seite **unendlich Farben** verwendet. Diese Seite zeichnet ein Unendlichkeitszeichen mithilfe eines Pfad Objekts, das im Artikel [**drei Typen von Bézier-Kurven**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)beschrieben wird. Das Bild wird dann mit einem animierten Regenbogen Farbverlauf gefärbt, der fortlaufend über das Bild läuft.

Der-Konstruktor erstellt das-Objekt, das `SKPath` das Unendlichkeitszeichen beschreibt. Nachdem der Pfad erstellt wurde, kann der Konstruktor auch die rechteckigen Begrenzungen des Pfads abrufen. Anschließend wird ein Wert mit dem Namen berechnet `gradientCycleLength` . Wenn ein Farbverlauf auf der oberen linken und unteren rechten Ecke des `pathBounds` Rechtecks basiert, ist dieser `gradientCycleLength` Wert die horizontale Gesamtbreite des Farbverlaufs Musters:

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

Der Konstruktor erstellt auch das `colors` Array für den Regenbogen und das- `SKCanvasView` Objekt.

Über schreibungen der `OnAppearing` -Methode und der- `OnDisappearing` Methode führen den Aufwand für die Animation aus. Die- `OnTimerTick` Methode animiert das `offset` Feld von 0 bis `gradientCycleLength` alle zwei Sekunden:

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

Zum Schluss `PaintSurface` rendert der Handler das Unendlichkeitszeichen. Da der Pfad negative und positive Koordinaten für einen Mittelpunkt von (0,0) enthält, `Translate` wird eine Transformation auf der Canvas verwendet, um Sie in den Mittelpunkt zu verschieben. Auf die Transform-Transformation folgt eine `Scale` Transformation, die einen Skalierungsfaktor anwendet, der das Unendlichkeitszeichen so groß wie möglich macht, während gleichzeitig innerhalb von 95% der Breite und Höhe der Canvas bleibt. 

Beachten Sie, dass die `STROKE_WIDTH` Konstante der Breite und Höhe des umgebenden Rechtecks für den Pfad hinzugefügt wird. Der Pfad wird mit einer Zeile dieser Breite gezeichnet, sodass die Größe der gerenderten unendlich Größe um die Hälfte der Breite auf allen vier Seiten angehoben wird:

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

Sehen Sie sich die Punkte an, die als die ersten beiden Argumente von vergangen sind `SKShader.CreateLinearGradient` . Diese Punkte basieren auf dem ursprünglichen Pfad des umgebenden Rechtecks. Der erste Punkt ist ( &ndash; 250, &ndash; 100), und der zweite ist (250, 100). Intern in skiasharp, werden diese Punkte der aktuellen Canvas-Transformation unterzogen, sodass Sie ordnungsgemäß mit dem angezeigten unendlichen Zeichen übereinstimmen.

Ohne das letzte Argument für `CreateLinearGradient` sehen Sie einen Regenbogen Farbverlauf, der von der linken oberen Ecke des unendlichen Zeichens nach unten rechts reicht. (Tatsächlich erstreckt sich der Farbverlauf von der oberen linken Ecke bis zur unteren rechten Ecke des umgebenden Rechtecks. Das gerenderte Unendlichkeitszeichen ist größer als das umgebende Rechteck um die Hälfte des `STROKE_WIDTH` Werts auf allen Seiten. Da der Farbverlauf am Anfang und am Ende rot ist und der Farbverlauf mit erstellt wird `SKShaderTileMode.Repeat` , ist der Unterschied nicht erkennbar.)

Mit dem letzten Argument für `CreateLinearGradient` durch geht das Farbverlaufs Muster fortlaufend über das Bild:

[![Unendlich Farben](linear-gradient-images/InfinityColors.png "Unendlich Farben")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>Transparenz und Farbverläufe

Die Farben, die zu einem Farbverlauf beitragen, können Transparenz einschließen. Anstelle eines Farbverlaufs, der von einer Farbe in eine andere ausgeblendet wird, kann der Farbverlauf von einer Farbe in eine transparente Farbe einblenden. 

Diese Technik kann für einige interessante Effekte verwendet werden. Eines der klassischen Beispiele zeigt ein grafisches Objekt mit der Reflektion:

[![Reflektionstyp](linear-gradient-images/ReflectionGradient.png "Reflektionstyp")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Der Text, der sich auf der Vorderseite befindet, wird mit einem Farbverlauf gefärbt, der im oberen Bereich 50% transparent ist, um unten vollständig transparent zu sein. Diese Transparenz Ebene ist mit den Alpha Werten 0x80 und 0 verknüpft.

Der `PaintSurface` Handler auf der Seite **reflektiongradienten** skaliert die Größe des Texts auf 90% der Breite des Zeichen Bereichs. Anschließend `xText` `yText` werden die Werte und berechnet, um den Text horizontal zentriert zu positionieren, aber auf einer Baseline, die der vertikalen Mitte der Seite entspricht:

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

Bei diesen Werten `xText` `yText` handelt es sich um dieselben Werte, mit denen der reflektierte Text im-Befehl `DrawText` am Ende des `PaintSurface` Handlers angezeigt wird. Direkt vor diesem Code wird jedoch ein `Scale` aufrufungsmethode von angezeigt `SKCanvas` . Diese `Scale` Methode wird horizontal um 1 skaliert (was nichts bewirkt), aber vertikal durch &ndash; 1 Der Mittelpunkt der Drehung wird auf den Punkt (0, `yText` ) festgelegt, wobei `yText` die vertikale Mitte des Zeichen Bereichs ist, der ursprünglich als `info.Height` dividiert durch 2 berechnet wurde.

Beachten Sie, dass Skia den Farbverlauf verwendet, um grafische Objekte vor den Canvas-Transformationen zu färben. Nachdem der nicht reflektierte Text gezeichnet wurde, `textBounds` wird das Rechteck so verschoben, dass es dem angezeigten Text entspricht:

```csharp
textBounds.Offset(xText, yText);
```

Der-Befehl `CreateLinearGradient` definiert einen Farbverlauf vom oberen Rand des Rechtecks bis zum unteren Rand. Der Farbverlauf wird von einem vollständig transparenten blau ( `paint.Color.WithAlpha(0)` ) bis zu einem transparenten blauen 50% ()-Wert dargestellt `paint.Color.WithAlpha(0x80)` . Die Canvas-Transformation dreht den Text nach oben, sodass der 50-prozentige transparente blaue Bereich bei der Baseline beginnt und am oberen Rand des Texts transparent wird.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
