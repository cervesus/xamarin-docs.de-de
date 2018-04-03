---
title: 3D Drehungen
description: Verwenden Sie nicht affine Transformationen 2D Objekte im 3D-Raum drehen.
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: a7d76bfad6a34bcd43b304a6ea4a8f9210e3550c
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="3d-rotations"></a>3D Drehungen

_Verwenden Sie nicht affine Transformationen 2D Objekte im 3D-Raum drehen._

Eine häufige Anwendung der nicht affine Transformationen simuliert die Rotation eines Objekts in einem 3D-Bereich 2D:

![](3d-rotation-images/3drotationsexample.png "Eine Textzeichenfolge, die in einem 3D-Bereich gedreht")

Sein Job umfasst die Arbeit mit dreidimensionalen Drehungen und Ableiten einer nicht affinen `SKMatrix` Transformation, die diese 3D Drehungen ausführt.

Es ist schwierig, diese entwickeln `SKMatrix` Transformation arbeiten ausschließlich innerhalb von zwei Dimensionen. Der Auftrag wird viel einfacher, wenn eine 4 x 4-Matrix in 3D-Grafiken verwendet diese 3 x 3-Matrix abgeleitet ist. SkiaSharp enthält die [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) Klasse für diesen Zweck, aber einige Kenntnisse zu 3D-Grafiken ist erforderlich für das Verständnis von 3D Drehungen und die von 4 x 4-Transformationsmatrix.

Ein dreidimensionales Koordinatensystem Fügt einer dritten Achse z konzeptionell aufgerufen, die Z-Achse wird im rechten Winkel auf dem Bildschirm. Koordinaten in 3D-Bereich mit drei Zahlen angegeben werden: (X, y, Z). In der 3D Koordinatensystem verwendet in diesem Artikel ansteigenden Werten von X nach rechts und zunehmenden Y-Werte ausfallen, wie zwei Dimensionen. Zunehmende positive Z-Werte stammen aus dem Bildschirm. Der Ursprung ist der oberen linken Ecke, wie 2D Grafiken. Sie können als XY-Ebene mit der Z-Achse im rechten Winkel auf dieser Ebene des Bildschirms vorstellen.

Dadurch wird eine linke Koordinatensystem aufgerufen. Wenn Sie die Zeigefinger für Ihre linken in Richtung der positive X Koordinaten (rechts zeigen) und dem mittleren Finger in die Richtung des zu erhöhenden Y-Koordinaten (unten), klicken Sie dann Ihre Thumb verweist in der Richtung des zu erhöhenden Z-Koordinaten – erweitern aus der Bildschirm.

In der 3D-Grafik basieren die Transformationen auf eine 4 x 4-Matrix. So sieht die 4 x 4-Identitätsmatrix aus:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

Bei der Arbeit mit einer 4 x 4-Matrix, empfiehlt es sich um die Zellen mit den Zeilen- und Zahlen zu identifizieren:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Allerdings die SkiaSharp `Matrix44` Klasse ist ein wenig anders. Die einzige Möglichkeit zum Festlegen oder Abrufen der Werte der einzelnen Zellen `SKMatrix44` mithilfe der [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) Indexer. Die Zeilen- und Spaltenindizes sind nullbasiert, statt einsbasierte, und die Zeilen und Spalten ausgetauscht werden. Die Zelle M14 in der obigen Abbildung erfolgt unter Verwendung des Indexers `[3, 0]` in einem `SKMatrix44` Objekt.

In einem System 3D-Grafiken wird ein 3D Punkt (X, y, Z) in eine 1 x 4-Matrix für Multiplikation mit der 4 x 4-Transformationsmatrix konvertiert:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

2D entspricht, erfolgen in drei Dimensionen transformiert, 3D Transformationen wird angenommen, dass in vier Dimensionen stattfinden. Die vierte Dimension wird als W bezeichnet und wird davon ausgegangen, dass die 3D-Bereich innerhalb der 4D Platz vorhanden sein, in denen W-Koordinaten gleich 1 sind. Die Transformation Formeln lauten wie folgt:

X "= M11·x + M21·y M31·z + M41

y "= M12·x + M22·y M32·z + M42

Z' = M13·x + M23·y M33·z + M43

w "= M14·x + M24·y M34·z + M44

Es ist offensichtlich, von der Transformation Formeln, die Zellen `M11`, `M22`, `M33` sind Skalierungsfaktoren in X-, Y- und Z-Richtung und `M41`, `M42`, und `M43` sind Übersetzung Faktoren für die X-, Y- und Z Erfahren Sie, wie.

Diese Koordinaten zurück in die 3D-Bereich konvertieren, bei denen W 1, das "X gleich" ", y", und Z "werden Koordinaten alle durch w dividiert":

x" = x' / w'

y" = y' / w'

z" = z' / w'

w" = w' / w' = 1

Dieser Division durch w "Perspektive im 3D-Bereich bereitstellt. Wenn w "gleich 1 ist, und klicken Sie dann keine Perspektive auftritt.

Drehungen in 3D-Bereich können sehr komplex sein, aber die einfachsten Drehungen getroffenen, um die X-, Y- und Z-Achse. Eine Drehung des Winkels α, um die x-Achse ist dieser Matrix:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Werte von X bleiben unverändert, wenn diese Transformation unterzogen. Die Drehung um die y-Achse bleibt unverändert Y-Werte:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Die Drehung um die Z-Achse ist im 2D Grafiken identisch:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Die Richtung der Drehung wird durch die Eignung des Koordinatensystems impliziert. Dies ist ein Linkshändig System, wenn Sie den Ziehpunkt der Richtung ansteigenden Werten für eine bestimmte Achse der linken zeigen – rechts für die Drehung um die x-Achse, Bild-ab für die Drehung um die y-Achse, und Sie für die Drehung um die Z-Achse – klicken Sie dann die Kurve der Handelsversion die anderen Finger gibt die Richtung der Drehung für positive Winkel an.

`SKMatrix44` hat die statische generalisiert [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) und [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) Methoden, mit denen Sie auf die Achse angeben, die die Drehung erfolgt:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Legen Sie die ersten drei Argumente für die Drehung um die x-Achse 1, 0, 0. Legen Sie für die Drehung um die y-Achse sie auf 0, 1, 0, und legen Sie sie für die Drehung um die Z-Achse, auf 0, 0, 1.

Die vierte Spalte der 4 x 4 ist für die Perspektive. Die `SKMatrix44` verfügt über keine Methoden zum Erstellen von Perspektive Transformationen, aber Sie können selbst erstellen, eine mit dem folgenden Code:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Der Grund für die Argumentnamen `depth` wird in Kürze offensichtlich sein. Dieser Code wird die Matrix erstellt:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Führen Sie die Transformation Formeln in der folgenden Berechnung w ":

w "– Z = / Tiefe + 1

Dies dient zum X- und Y-Koordinaten zu reduzieren, wenn Z-Werte sind kleiner als 0 (null) (konzeptionell hinter der XY-Ebene) sowie zum Erhöhen von X und Y-Koordinaten für positive Werte von Z. Wenn die Z-Koordinate gleich `depth`, klicken Sie dann w "0 (null), und die Koordinaten werden unendlich. Dreidimensionale Grafiksystemen um eine Kamera Metapher erstellt werden und die `depth` hier Wert repräsentiert die Entfernung der Kamera auf den Ursprung des Koordinatensystems. Wenn ein Objekt einen Z, d. h. koordinieren hat `depth` Einheiten vom Ursprung, ist grundsätzlich berühren Sicht der Kamera und unendlich ist.

Beachten Sie, dass Sie wahrscheinlich dadurch verwenden `perspectiveMatrix` Wert in Kombination mit Matrizen Drehung. Verfügt ein Grafikobjekt rotierenden X- oder Y-Koordinaten, die größer als `depth`, die Drehung dieses Objekts in einem 3D-Bereich ist wahrscheinlich größer als Z-Koordinaten hinzuzuziehen `depth`. Dies muss vermieden werden. Beim Erstellen `perspectiveMatrix` festlegen möchten `depth` auf einen Wert, der groß genug für alle Koordinaten im Grafikobjekt unabhängig davon, wie es gedreht wird. Dadurch wird sichergestellt, dass nie eine Division durch 0 (null) vorhanden ist.

Kombinieren von 3D Drehungen und Perspektive erfordert Multiplikation 4 x 4-Matrizen ergibt. Zu diesem Zweck `SKMatrix44` Verkettung Methoden definiert. Wenn `A` und `B` sind `SKMatrix44` Objekte aufweist, der folgende Code eine gleich festgelegt, bis eine × B:

```csharp
A.PostConcat(B);
```

Wenn eine von 4 x 4-Transformationsmatrix in einer 2D-Grafiksystem verwendet wird, wird er auf 2D Objekte angewendet. Diese Objekte sind Flatfile und werden als Z-Koordinaten von 0 (null) aufweisen. Die Transformation Multiplikation ist etwas einfacher als die Transformation, die weiter oben dargestellten:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Dieser Wert 0 für die Z-Ergebnissen in Transformation Formeln, die keine Zellen in der dritten Zeile der Matrix beinhalten:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

Z' = M13·x + M23·y + M43

w "= M14·x + M24·y + M44

Darüber hinaus Z' Koordinate wird ebenfalls hier nicht relevant. Wenn ein 3D-Objekt in einer 2D-Grafiksystem angezeigt wird, ist es ein zweidimensionales Objekt durch Ignorieren der Z-Koordinatenwerten reduziert. Die Transformation-Formeln sind tatsächlich nur diese beiden:

x" = x' / w'

y" = y' / w'

Dies bedeutet, dass die dritte Zeile *und* dritte Spalte die 4 x 4-Matrix kann ignoriert werden.

Jedoch so, weshalb ist ist sogar erforderlich 4 x 4-Matrix ursprünglich?

Obwohl der dritten Zeile und die dritte Spalte die 4 x 4 für zweidimensionale Transformationen, die dritte Zeile und Spalte irrelevant sind *führen* spielen eine Rolle vor, die bei verschiedenen `SKMatrix44` Werte miteinander multipliziert werden. Nehmen wir beispielsweise an, dass Sie Drehung um die y-Achse mit der perspektivische Transformation multipliziert:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

Innerhalb des Produkts, die die Zelle `M14` enthält jetzt einen Wert für die Perspektive. Wenn Sie diese Matrix 2D Objekte anwenden möchten, werden der dritten Zeile und Spalte entfernt, um sie in eine 3 x 3-Matrix konvertieren:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Jetzt kann verwendet werden, um einen 2D-Skalierungsvorgang Punkt zu transformieren:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Die Transformation-Formeln sind:

x' = cos(α)·x

y' = y

Z' = (sin (α) / Tiefe) ·x + 1

Teilen Sie alles, was nun durch Z ":

X"= cos (α) ·x / ((sin (α) / Tiefe) ·x + 1)

y"= y / ((sin (α) / Tiefe) ·x + 1)

Wenn 2D Objekte mit einem positiven Winkel um die y-Achse, und klicken Sie dann positive gedreht werden laufen X-Werte im Hintergrund beim Negative X-Werte in den Vordergrund stammen. Die X-Werte scheinen, wie die Koordinaten, die am die y-Achse auf der y-Achse (die den Kosinus-Wert unterliegt) näher werden kleinere oder größere, wie sie von der Viewer verschieben oder näher an den Viewer.

Bei Verwendung `SKMatrix44`, alle 3D-Drehung und Perspektive Vorgänge auszuführen, durch Multiplizieren der verschiedenen `SKMatrix44` Werte. Und dann eine zweidimensionale 3 x 3-Matrix aus den 4 x 4 extrahiert werden können Matrix mit der [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) Eigenschaft von der `SKMatrix44` Klasse. Diese Eigenschaft gibt ein vertrautes `SKMatrix` Wert.

Die **Drehung 3D** Seite können Sie die 3D-Drehung experimentieren. Die [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) Datei instanziiert vier Schieberegler, um die Drehung um die X-, Y- und Z-Achse festlegen, und klicken Sie zum Festlegen eines Tiefenwerts:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Beachten Sie, dass die `depthSlider` wird initialisiert, indem eine `Minimum` Wert 250. Dies bedeutet, dass die 2D Objekt, das hier rotierenden X- und Y-Koordinaten, die in einen Kreis, der durch einen Radius 250 Pixel, um den Ursprung definierten eingeschränkt hat. Drehung dieses Objekts in einem 3D-Bereich führt immer zu Koordinatenwerte weniger als 250.

Die [ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) Code-Behind-Datei lädt in eine Bitmap, das 300 Pixel quadratische ist:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Falls die 3D-Transformation auf diese Bitmap zentriert, koordiniert dann X- und Y zwischen –150 und 150, während die Ecken 212 Pixel aus der Mitte sind, damit alles innerhalb der 250 Pixel Radius ist.

Die `PaintSurface` Ereignishandler erstellt `SKMatrix44` Objekte basierend auf den Schieberegler und multipliziert zusammen mit `PostConcat`. Die `SKMatrix` extrahiert wurde von der letzten `SKMatrix44` Objekt umgeben von übersetzen Transformationen zum Zentrieren Sie der Rotation in der Mitte des Bildschirms:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
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

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

Wenn Sie mit dem vierten Schieberegler experimentieren, sehen Sie sich, dass die verschiedenen Tiefe-Einstellungen nicht das Objekt von der Viewer verschieben, aber stattdessen das Ausmaß der Perspektive Effekte Alter:

[![](3d-rotation-images/rotation3d-small.png "Dreifacher Screenshot der Drehung 3D Seite")](3d-rotation-images/rotation3d-large.png#lightbox "dreifacher Screenshot der Drehung 3D Seite")

Die **animiert Drehung 3D** verwendet auch `SKMatrix44` eine Textzeichenfolge in einem 3D-Bereich animiert. Die `textPaint` -Objekts festgelegt, wie ein Feld im Konstruktor verwendet wird, um die Grenzen des Texts zu bestimmen:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

Die `OnAppearing` Außerkraftsetzung definiert drei Xamarin.Forms `Animation` Objekte zum Animieren der `xRotationDegrees`, `yRotationDegrees`, und `zRotationDegrees` Felder mit unterschiedlichen Raten. Beachten Sie, dass die Zeiträume dieser Animationen festgelegt ist, um Zahlen Primzahl – 5 Sekunden, 7 Sekunden und 11 Sekunden – damit die gesamte Kombination nur jede 385 Sekunden oder mehr als 10 Minuten wiederholt:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Wie das vorherige Programm die `PaintCanvas` Ereignishandler erstellt `SKMatrix44` miteinander multipliziert und Werte für die Drehung und Perspektive:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Diese 3D-Drehung umgeben mit mehreren 2D Transformationen Mittelpunkts der Drehung um die Mitte des Bildschirms verschoben, und die Größe der Textzeichenfolge skalieren, sodass sie die gleiche Breite wie der Bildschirm ist:

[![](3d-rotation-images/animatedrotation3d-small.png "Dreifacher Screenshot der Drehung animiert 3D Seite")](3d-rotation-images/animatedrotation3d-large.png#lightbox "dreifacher Screenshot der Drehung animiert 3D Seite")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
