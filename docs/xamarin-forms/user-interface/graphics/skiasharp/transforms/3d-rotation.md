---
title: 3D Drehungen in SkiaSharp
description: In diesem Artikel wird erläutert, wie nicht affine Transformationen verwenden, um 2D-Objekte im 3D-Raum zu drehen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
ms.openlocfilehash: 7ac9ec458f16357ef50e23c459a9b0e1f79bdd97
ms.sourcegitcommit: c6ff24b524d025d7e87b7b9c25f04c740dd93497
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56240369"
---
# <a name="3d-rotations-in-skiasharp"></a>3D Drehungen in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Verwenden Sie nicht affine Transformationen, um 2D-Objekte im 3D-Raum zu drehen._

Eine übliche Anwendung der nicht affine Transformationen ist die Rotation eines 2D-Objekts im 3D-Raum simulieren:

![](3d-rotation-images/3drotationsexample.png "Eine Textzeichenfolge, die in einem 3D-Bereich gedreht")

Sein Job umfasst die Arbeit mit dreidimensionalen Drehungen und eine nicht affine ableiten `SKMatrix` Transformation, die diese 3D Drehungen ausführt.

Es ist schwierig, diese Entwicklung `SKMatrix` Transformation arbeiten ausschließlich innerhalb von zwei Dimensionen. Der Auftrag wird wesentlich einfacher, wenn eine 4 x 4-Matrix in 3D-Grafiken verwendet diese 3-Mal-3-Matrix abgeleitet wird. SkiaSharp enthält die [ `SKMatrix44` ](xref:SkiaSharp.SKMatrix44) Klasse für diesen Zweck, aber die Erfahrung mit 3D-Grafiken ist erforderlich, um zu verstehen, 3D Drehungen und die der 4 x 4-Transformationsmatrix.

Ein dreidimensionales Koordinatensystem Fügt einer dritten Achse namens z vom Konzept her, die Z-Achse wird im rechten Winkel auf dem Bildschirm. Koordinieren von Punkten im 3D-Raum mit drei Zahlen angegeben werden: (X, y, Z). In den 3D Koordinatensystem, die in diesem Artikel ansteigenden Werten von X verwendet werden, auf der rechten Seite und zunehmenden Y-Werte ausfällt, wie zwei Dimensionen. Positive Z Werte stammen aus dem Bildschirm heraus. Der Ursprung ist der linke obere Ecke, wie die 2D-Grafiken. Sie können den Bildschirm als XY-Ebene mit der Z-Achse rechtwinklig zu dieser Ebene vorstellen.

Dies ist ein linken Koordinatensystem bezeichnet. Wenn Sie die Zeigefinger für Ihre linken in Richtung der positive X-Koordinaten (nach rechts zeigen) und dem mittleren Finger in Richtung der Erhöhung der Y-Koordinaten (zentral Herunterskalieren), klicken Sie dann Ihre Thumb-Steuerelement zeigt in Richtung der Z-Koordinaten erhöhen – Erweitern von sich aus der Bildschirm.

In 3D-Grafiken basieren die Transformationen in einer 4 x 4-Matrix. So sieht die Identitätsmatrix 4 x 4-aus:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

Bei der Arbeit mit einer 4 x 4-Matrix, ist es sinnvoll, die die Zellen mit ihren Zeilen- und Zahlen zu identifizieren:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Allerdings die SkiaSharp `Matrix44` Klasse ist ein wenig anders. Die einzige Möglichkeit zum Festlegen oder Abrufen der Werte der einzelnen Zellen `SKMatrix44` ist die Verwendung der [ `Item` ](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32)) Indexer. Die Zeilen- und Spaltenindizes sind nullbasierte statt 1-basierte, und die Zeilen und Spalten vertauscht sind. Die Zelle M14 in der obigen Abbildung erfolgt unter Verwendung des Indexers `[3, 0]` in einem `SKMatrix44` Objekt.

In einem System 3D-Grafiken wird ein 3D-Punkts (X, y, Z) um eine 1 x 4-Matrix für die Multiplikation mit der der 4 x 4-Transformationsmatrix konvertiert:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Entspricht bis 2D, statt in drei Dimensionen umwandelt, 3D-Transformationen befinden sich in vier Dimensionen stattfinden. Der vierten Dimension wird als W bezeichnet, und die 3D-Raum wird davon ausgegangen, dass innerhalb des Bereichs 4D vorhanden sein, in denen W-Koordinaten gleich 1 sind. Die Transformation Formeln lauten wie folgt aus:

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

Es ergibt sich aus der Transformation-Formeln, die die Zellen `M11`, `M22`, `M33` Faktoren in X, Y und Z-Richtung, skalieren und `M41`, `M42`, und `M43` Übersetzung Faktoren in Bezug auf das X, Y und Z werden Erfahren Sie, wie.

Diese Koordinaten zurück in die 3D-Raum konvertieren, in denen W ist 1, das "X" gleich ", y', Z' Koordinaten alle durch w unterteilt sind":

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

Dieser Division von w "zeigt die Perspektiven im 3D-Raum. Wenn w' gleich 1 ist, wird keine Perspektive zu durchgeführt.

Drehungen im 3D-Raum können sehr komplex sein, aber die einfachsten Rotationen stammen, um die X-, Y- und Z-Achsen. Eine Drehung um Winkel α, um die x-Achse ist dieser Matrix:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Werte von X bleibt bei dieser Transformation unterzogen. Drehung um die y-Achse bleibt unverändert Y-Werte:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Drehung um die Z-Achse ist dasselbe wie die 2D-Grafiken:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Die Richtung der Drehung wird durch die rechts-oder Linkshändigkeit des Koordinatensystems impliziert. Dies ist ein Linkshänder System, wenn Sie das Thumb-Steuerelement von der linken auf ansteigenden Werten für eine bestimmte Achse verweisen – auf der rechten Seite für die Rotation um die x-Achse, Bild-ab für Drehung um die y-Achse, und klicken Sie auf die Sie für die Rotation um die Z-Achse – klicken Sie dann die Kurve der "yo" die anderen Finger gibt die Richtung der Drehung für positiven Winkel an.

`SKMatrix44` hat die statische generalisiert [ `CreateRotation` ](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single)) und [ `CreateRotationDegrees` ](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single)) Methoden, mit denen Sie auf der Achse angeben, die die Drehung erfolgt:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Legen Sie die ersten drei Argumente für die Rotation um die x-Achse 1, 0, 0. Legen sie für die Rotation um die y-Achse auf 0, 1, 0, und legen Sie sie für die Drehung um die Z-Achse, auf 0, 0, 1.

Die vierte Spalte der 4 x 4 ist für die Perspektive. Die `SKMatrix44` verfügt über keine Methoden zum Erstellen von Perspektive Transformationen, aber Sie können eine selbst mithilfe des folgenden Codes erstellen:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Der Grund für den Argumentnamen `depth` werden in Kürze offensichtlich. Dieser Code wird die Matrix erstellt:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Führen Sie die Transformation Formeln in der folgenden Berechnung von w ":

`w' = –z / depth + 1`

Dies dient, X und Y-Koordinaten zu reduzieren, wenn Z-Werte sind kleiner als 0 (null) (im Prinzip hinter der XY-Ebene) und die X- und Y-Koordinaten für positive Z-Werte zu erhöhen. Wenn die Z-Koordinate gleich `depth`, klicken Sie dann w "ist 0 (null) und Koordinaten unendlich. Dreidimensionale Grafiksysteme werden erstellt, um eine Kamera Metapher, und die `depth` Wert repräsentiert die Entfernung der Kamera, vom Ursprung des Koordinatensystems. Verfügt ein grafisches Objekt einen Z, koordinieren `depth` Einheiten vom Ursprung der Fokus der Kamera ist im Prinzip berühren und wird unendlich groß.

Beachten Sie, dass Sie wahrscheinlich dieses verwenden `perspectiveMatrix` Wert in Verbindung mit einer Drehung Matrizen. Verfügt ein Graphics-Objekt, das gedreht X- oder Y-Koordinaten, die größer als `depth`, dann ist die Drehung des Objekts im 3D-Raum wahrscheinlich größer als Z-Koordinaten umfassen `depth`. Dies muss vermieden werden. Beim Erstellen von `perspectiveMatrix` festlegen möchten `depth` auf einen Wert, der groß genug für alle Koordinaten in das Graphics-Objekt, unabhängig davon, wie es gedreht wird. Dadurch wird sichergestellt, dass keiner Division durch 0 (null) vorhanden ist.

Kombinieren von 3D Drehungen und Perspektive erfordert 4 x 4-Matrizen miteinander multipliziert. Zu diesem Zweck `SKMatrix44` Verkettung Methoden definiert. Wenn `A` und `B` sind `SKMatrix44` Objekte aufweist, und klicken Sie dann der folgende Code eine legt, bis eine × B: fest

```csharp
A.PostConcat(B);
```

Wenn eine 4 x 4-Transformationsmatrix, die in einem System von 2D-Grafiken verwendet wird, wird es auf 2D-angewendet. Diese Objekte sind flach und befinden sich die Z-Koordinaten von 0 (null) haben. Die Transformation Multiplikation ist etwas einfacher als die Transformation, die weiter oben gezeigt:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Der Wert von 0 für die Z-Ergebnissen in Transformation Formeln, die alle Zellen in der dritten Zeile der Matrix nicht beinhalten:

X' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

Z' = M13·x + M23·y + M43

w' = M14·x + M24·y + M44

Darüber hinaus das Z' Koordinate ist auch hier nicht relevant. Wenn ein 3D-Objekt in einem System 2D-Grafiken angezeigt wird, wird der Wert der Z-Koordinatenwerten zu ignorieren, ein zweidimensionales Objekt reduziert. Die Transformation-Formeln sind eigentlich nur diese beiden:

`x" = x' / w'`

`y" = y' / w'`

Dies bedeutet, dass die dritte Zeile *und* dritten Spalte der 4 x 4-Matrix kann ignoriert werden.

Aber so, warum ist ist überhaupt benötigt 4 x 4-Matrix im ersten Schritt?

Obwohl der dritten Zeile und dritten Spalte der dem 4 x 4-irrelevant für zweidimensionale Transformationen, der dritten Zeile und Spalte sind *führen* spielen eine Rolle vor, die bei verschiedenen `SKMatrix44` Werte miteinander multipliziert. Nehmen wir beispielsweise an, dass Sie die Rotation um die y-Achse, mit der Transformation für die Perspektive multiplizieren:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

Das Produkt der Zelle `M14` enthält nun einen Wert für die Perspektive. Wenn Sie diese Matrix auf 2D Objekte anwenden möchten, werden der dritten Zeile und Spalte entfernt, um es in eine 3 x 3-Matrix zu konvertieren:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Sie können jetzt verwendet werden, einen 2D Punkt zu transformieren:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Die Transformation-Formeln sind:

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

Teilen Sie alles, was jetzt durch Z ":

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

Wenn mit einer positiven Winkel um die y-Achse, und klicken Sie dann positive 2D-gedreht werden rücken X-Werte in den Hintergrund und Negative X-Werte, die in den Vordergrund gesetzt werden. Die X-Werte scheinen, näher auf die y-Achse (die den Kosinus-Wert unterliegt) als Koordinaten, die am der Y-Achse dargestellt werden, kleiner oder größer ist, da sie das. der Viewer wechseln oder den Betrachter näher.

Bei Verwendung `SKMatrix44`, führen Sie alle 3D-Drehung und Perspektive Operationen durch Multiplikation der verschiedenen `SKMatrix44` Werte. Und Sie eine zweidimensionale 3 x 3-Matrix, aus dem 4 x 4 extrahieren können-Matrix mit der [ `Matrix` ](xref:SkiaSharp.SKMatrix44.Matrix) Eigenschaft der `SKMatrix44` Klasse. Diese Eigenschaft gibt einen bekannten `SKMatrix` Wert.

Die **Drehung 3D** Seite können Sie mit der 3D-Drehung experimentieren. Die [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) Datei instanziiert vier Schieberegler Drehung um die X-, Y- und Z-Achsen festgelegt, und klicken Sie zum Festlegen eines Tiefenwerts:

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

Beachten Sie, dass die `depthSlider` wird initialisiert, indem eine `Minimum` maximal 250. Dies bedeutet, dass der hier rotierenden 2D-Objekts X und Y-Koordinaten, die auf ein Kreis, der von einem Radius 250 Pixel, um den Ursprung definiert beschränkt ist. Drehung des Objekts im 3D-Raum führt immer zu Koordinatenwerte weniger als 250.

Die [ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) Code-Behind-Datei geladen wird, in eine Bitmap, das 300 Pixel im Quadrat ist:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Falls die Transformation 3D auf diese Bitmap zentriert, koordiniert dann X- und Y zwischen –150 und 150, während die Ecken 212 Pixel aus dem Center sind, also alles innerhalb der Radius 250 Pixel.

Die `PaintSurface` Ereignishandler erstellt `SKMatrix44` Objekte basierend auf den Schieberegler, und zusammen mit multipliziert `PostConcat`. Die `SKMatrix` extrahiert, die von der letzten `SKMatrix44` Objekt umgeben von Übersetzungstransformationen um zentrieren die Drehung in der Mitte des Bildschirms:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Wenn Sie mit dem vierten Schieberegler experimentieren, sehen Sie sich, dass die Einstellungen für die verschiedenen Tiefe nicht das Objekt weiter vom Betrachter weglenken verschieben, aber stattdessen das Ausmaß der Perspektive Auswirkungen Alter:

[![](3d-rotation-images/rotation3d-small.png "Dreifacher Screenshot der Seite der Drehung-3D")](3d-rotation-images/rotation3d-large.png#lightbox "dreifachen Screenshot der Seite der Drehung-3D")

Die **animiert Drehung 3D** verwendet auch `SKMatrix44` um eine Zeichenfolge im 3D-Raum zu animieren. Die `textPaint` Objekt festgelegt, wie ein Feld im Konstruktor verwendet wird, um die Grenzen des Texts zu bestimmen:

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

Die `OnAppearing` Außerkraftsetzung definiert drei Xamarin.Forms `Animation` zu animierenden Objekte die `xRotationDegrees`, `yRotationDegrees`, und `zRotationDegrees` Felder mit unterschiedlichen Raten. Beachten Sie, die die Zeiträume diese Animationen festgelegt werden, um eine Primzahl handelt Zahlen (5 Sekunden, 7 Sekunden und 11 Sekunden), damit die gesamte Kombination nur jede 385 Sekunden oder mehr als 10 Minuten wiederholt:

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

Wie in der oben stehenden Programms die `PaintCanvas` Ereignishandler erstellt `SKMatrix44` Werte für die Drehung und Perspektive, und diese miteinander multipliziert werden:

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

Diese 3D-Drehung umgeben mit mehreren 2D-Transformationen, um den Mittelpunkt der Drehung in der Mitte des Bildschirms zu verschieben, und klicken Sie auf die Größe der Textzeichenfolge zu skalieren, sodass sie die gleiche Breite wie der Bildschirm ist:

[![](3d-rotation-images/animatedrotation3d-small.png "Dreifacher Screenshot der Seite der Drehung animiert-3D")](3d-rotation-images/animatedrotation3d-large.png#lightbox "dreifachen Screenshot der Seite der Drehung animiert-3D")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
