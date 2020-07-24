---
title: 3D-Drehungen in skiasharp
description: In diesem Artikel wird erläutert, wie Sie nicht-affine Transformationen verwenden, um 2D-Objekte im 3D-Raum zu drehen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 855fde62483dcbc6f8769e7a8eb66d84aadfe1da
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934978"
---
# <a name="3d-rotations-in-skiasharp"></a>3D-Drehungen in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden Sie nicht-affine Transformationen, um 2D-Objekte im 3D-Raum zu drehen._

Eine gängige Anwendung von nicht affinen Transformationen ist das Simulieren der Drehung eines 2D-Objekts im 3D-Raum:

![Eine Text Zeichenfolge, gedreht in 3D](3d-rotation-images/3drotationsexample.png)

Dieser Auftrag umfasst das Arbeiten mit dreidimensionalen Drehungen und das anschließende Ableiten einer nicht affinen `SKMatrix` Transformation, die diese 3D-Rotationen ausführt.

Es ist schwierig, diese Transformation zu entwickeln, die `SKMatrix` ausschließlich innerhalb von zwei Dimensionen funktioniert. Der Auftrag wird sehr viel einfacher, wenn diese 3-x-3-Matrix von einer 4 x 4-Matrix abgeleitet ist, die in 3D-Grafiken verwendet wird. Skiasharp enthält die- [`SKMatrix44`](xref:SkiaSharp.SKMatrix44) Klasse für diesen Zweck, aber es ist ein gewisser Hintergrund in 3D-Grafiken erforderlich, um 3D-Drehungen und die 4-x-4-Transformationsmatrix zu verstehen.

Ein dreidimensionales Koordinatensystem fügt eine dritte Achse mit dem Namen Z. konzeptionell hinzu. die Z-Achse befindet sich in der rechten Ecke des Bildschirms. Koordinaten Punkte im 3D-Raum werden mit drei Zahlen angegeben: (x, y, z). In dem 3D-Koordinatensystem, das in diesem Artikel verwendet wird, sind die zunehmenden Werte von X auf der rechten Seite, und die zunehmenden Werte von Y werden genau wie in zwei Dimensionen nach unten durchlaufen. Das Erhöhen der positiven Z-Werte wird auf dem Bildschirm angezeigt. Der Ursprung ist die linke obere Ecke, ebenso wie in 2D-Grafiken. Sie können sich den Bildschirm als XY-Ebene mit der Z-Achse in der rechten Ecke auf dieser Ebene vorstellen.

Dies wird als linkes Koordinatensystem bezeichnet. Wenn Sie für den Vordergrund auf die linke Seite in der Richtung der positiven X-Koordinaten (rechts) und für den mittleren Finger in der Richtung der Erhöhung der Y-Koordinaten (unten) zeigen, wird der Ziehpunkt in die Richtung der Erhöhung der Z-Koordinaten angezeigt – das Erweitern vom Bildschirm.

In 3D-Grafiken basieren Transformationen auf einer 4 x 4-Matrix. Hier sehen Sie die 4-bis 4-Identitätsmatrix:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

Beim Arbeiten mit einer 4 x 4-Matrix ist es praktisch, die Zellen mit ihren Zeilen-und Spalten Nummern zu identifizieren:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Die skiasharp- `Matrix44` Klasse ist jedoch etwas anders. Die einzige Möglichkeit, einzelne Zellwerte in festzulegen oder zu erhalten, `SKMatrix44` ist die Verwendung des [`Item`](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32)) Indexers. Die Zeilen-und Spalten Indizes sind NULL basiert, und die Zeilen und Spalten werden ausgetauscht. Der Zugriff auf die Zelle M14 im obigen Diagramm erfolgt mithilfe des Indexers `[3, 0]` in einem- `SKMatrix44` Objekt.

In einem 3D-Grafiksystem wird ein 3D-Punkt (x, y, z) in eine 1-x-4-Matrix konvertiert, um mit der 4-mal-4-Transformationsmatrix multipliziert zu werden:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Analog zu 2D-Transformationen, die in drei Dimensionen stattfinden, wird angenommen, dass 3D-Transformationen in vier Dimensionen stattfinden. Die vierte Dimension wird als W bezeichnet, und es wird davon ausgegangen, dass der 3D-Raum im 4D-Raum vorhanden ist, in dem w-Koordinaten gleich 1 sind. Die transformationsformeln lauten wie folgt:

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

Es ist offensichtlich in den transformationsformeln, dass es sich bei den Zellen `M11` `M22` `M33` um Skalierungsfaktoren in der x-, y-und z-Richtung handelt, und `M41` , `M42` und `M43` sind Übersetzungs Faktoren in der x-, y-und z-Richtung.

Um diese Koordinaten wieder in den 3D-Raum zu konvertieren, bei dem W 1 entspricht, werden die Koordinaten x, y und z "durch w" dividiert:

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

Diese Division durch w ' bietet eine Perspektive im 3D-Raum. Wenn w ' 1 ist, dann wird keine Perspektive angezeigt.

Rotationen im 3D-Raum können sehr komplex sein, aber die einfachsten Drehungen sind die der X-, Y-und Z-Achse. Eine Drehung des Winkels von der X-Achse um die X-Achse ist die folgende Matrix:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Werte von X bleiben unverändert, wenn diese Transformation unterzogen wird. Bei Drehung um die y-Achse bleiben die Werte von "y" unverändert:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Die Drehung um die Z-Achse ist die gleiche wie in 2D-Grafiken:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Die Richtung der Drehung wird durch die häntigkeit des Koordinatensystems impliziert. Dabei handelt es sich um ein nach links übergebender System. Wenn Sie also auf den Ziehpunkt der linken Seite zeigen, um die Werte für eine bestimmte Achse zu vergrößern – rechts für die Drehung um die X-Achse, nach unten für die Drehung um die Y-Achse, und für die Drehung um die Z-Achse.

`SKMatrix44`verfügt über generalisierte statische [`CreateRotation`](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single)) [`CreateRotationDegrees`](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single)) Methoden und, mit denen Sie die Achse angeben können, um die die Drehung erfolgt:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Legen Sie für die Drehung um die X-Achse die ersten drei Argumente auf 1, 0, 0 fest. Legen Sie für die Drehung um die Y-Achse diese auf 0, 1, 0 und für die Drehung um die Z-Achse fest, und legen Sie Sie auf 0, 0, 1 fest.

Die vierte Spalte von 4 x 4 ist die Perspektive. Der `SKMatrix44` verfügt über keine Methoden zum Erstellen von perspektivischen Transformationen, Sie können jedoch selbst einen erstellen, indem Sie den folgenden Code verwenden:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Der Grund für den Argument Namen `depth` wird in Kürze ersichtlich. Mit diesem Code wird die Matrix erstellt:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Die transformationsformeln führen zu der folgenden Berechnung von w ':

`w' = –z / depth + 1`

Dadurch werden x-und y-Koordinaten reduziert, wenn die Werte von z (konzeptionell hinter der XY-Ebene) kleiner als 0 (null) sind und die x-und y-Koordinaten für positive Werte von z erhöhen. Wenn die Z-Koordinate gleich `depth` ist, dann ist w "0 (null), und Koordinaten werden unendlich. Dreidimensionale Grafik Systeme werden um eine Kamera Metapher erstellt, und der hier dargestellte `depth` Wert stellt den Abstand der Kamera vom Ursprung des Koordinatensystems dar. Wenn ein grafisches Objekt eine Z-Koordinate hat, die `depth` Einheiten vom Ursprung ist, wird das Bild der Kamera konzeptionell berührt und ist unendlich groß.

Denken Sie daran, dass Sie diesen `perspectiveMatrix` Wert wahrscheinlich in Kombination mit Rotations Matrizen verwenden werden. Wenn ein zu rotierender Grafik Objekt über X-oder Y-Koordinaten größer als ist `depth` , muss die Drehung dieses Objekts im 3D-Raum wahrscheinlich Z-Koordinaten mit mehr als einschließen `depth` . Dies muss vermieden werden. Beim Erstellen `perspectiveMatrix` möchten Sie `depth` auf einen Wert festlegen, der für alle Koordinaten im Grafik Objekt ausreichend groß ist, unabhängig davon, wie er gedreht wird. Dadurch wird sichergestellt, dass nie eine Division durch 0 (null) erfolgt.

Die Kombination von 3D-Drehungen und-Perspektiven erfordert die Multiplikation von 4 x 4 Matrizen. Zu diesem Zweck `SKMatrix44` definiert Verkettungs Methoden. Wenn `A` und `B` - `SKMatrix44` Objekte sind, legt der folgende Code einen-Wert fest, der gleich einem × B ist:

```csharp
A.PostConcat(B);
```

Wenn in einem 2D-Grafiksystem eine 4 x 4-Transformationsmatrix verwendet wird, wird Sie auf 2D-Objekte angewendet. Diese Objekte sind flach, und es wird angenommen, dass Z-Koordinaten NULL sind. Die Transformation für Transformationen ist etwas einfacher als die zuvor gezeigte Transformation:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Der Wert 0 für z führt zu transformationsformeln, die keine Zellen in der dritten Zeile der Matrix einschließen:

x ' = M11 = x + M21 · y + M41

y ' = M12 = x + M22 = y + M42

z ' = M13 · x + M23 = y + M43

w ' = M14 · x + M24 = y + M44

Außerdem ist die z-Koordinate hier ebenfalls irrelevant. Wenn ein 3D-Objekt in einem 2D-Grafiksystem angezeigt wird, wird es mit einem zweidimensionalen Objekt reduziert, indem die Z-Koordinaten Werte ignoriert werden. Die transformationsformeln sind tatsächlich nur die folgenden:

`x" = x' / w'`

`y" = y' / w'`

Dies bedeutet, dass die dritte Zeile *und* die dritte Spalte der 4 x 4-Matrix ignoriert werden können.

Wenn dies der Fall ist, warum ist die 4-mal-4-Matrix überhaupt notwendig?

Obwohl die dritte Zeile und die dritte Spalte von 4 bis 4 für zweidimensionale Transformationen irrelevant sind *, spielen die* dritte Zeile und Spalte eine Rolle vor, wenn verschiedene `SKMatrix44` Werte multipliziert werden. Nehmen Sie beispielsweise an, dass Sie die Drehung um die Y-Achse mit der Perspektiven Transformation multiplizieren:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

Im Produkt enthält die Zelle `M14` nun einen perspektivischen Wert. Wenn Sie diese Matrix auf 2D-Objekte anwenden möchten, werden die dritte Zeile und Spalte entfernt, um Sie in eine 3 x 3-Matrix zu konvertieren:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Nun kann es verwendet werden, um einen 2D-Punkt zu transformieren:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Die transformationsformeln lauten wie folgt:

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

Teilen Sie nun alles durch z ":

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

Wenn 2D-Objekte mit einem positiven Winkel um die Y-Achse gedreht werden, werden positive x-Werte an den Hintergrund zurückgegeben, während negative x-Werte in den Vordergrund stehen. Die X-Werte scheinen näher an der y-Achse (die durch den Kosinus-Wert geregelt ist) zu wechseln, da die Koordinaten, die am weitesten oben von der y-Achse liegen, kleiner oder größer werden, wenn Sie sich vom Viewer oder näher an den Viewer bewegen.

Wenn Sie verwenden `SKMatrix44` , führen Sie alle 3D-Rotations-und perspektivischen Vorgänge durch Multiplizieren verschiedener `SKMatrix44` Werte aus Anschließend können Sie mithilfe der-Eigenschaft der-Klasse eine zweidimensionale 3-x-3-Matrix aus der 4-x-4-Matrix extrahieren [`Matrix`](xref:SkiaSharp.SKMatrix44.Matrix) `SKMatrix44` . Diese Eigenschaft gibt einen vertrauten `SKMatrix` Wert zurück.

Auf der Seite **Drehung 3D** können Sie mit 3D-Drehung experimentieren. Die Datei [**Rotation3DPage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) instanziiert vier Schieberegler zum Festlegen der Drehung um die X-, Y-und Z-Achse und zum Festlegen eines tiefen Werts:

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

Beachten Sie, dass `depthSlider` mit dem Wert 250 initialisiert wird `Minimum` . Dies bedeutet, dass das 2D-Objekt, das hier gedreht wird, über X-und Y-Koordinaten verfügt, die auf einen Kreis beschränkt sind, der durch einen 250-Pixel-Radius um Jede Drehung dieses Objekts im 3D-Raum führt immer zu Koordinaten Werten, die kleiner als 250 sind.

Die [**Rotation3DPage.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) -Code-Behind-Datei wird in eine Bitmap geladen, die ein Quadrat von 300 Pixeln ist:

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

Wenn die 3D-Transformation auf dieser Bitmap zentriert ist, reichen die X-und Y-Koordinaten zwischen – 150 und 150, während die Ecken 212 Pixel von der Mitte sind, sodass alles innerhalb des Radius von 250 Pixel liegt.

Der `PaintSurface` -Handler erstellt `SKMatrix44` -Objekte auf der Grundlage der Schieberegler und multipliziert Sie mithilfe von `PostConcat` . Der `SKMatrix` aus dem abschließenden Objekt extrahierte Wert `SKMatrix44` wird von Translation-Transformationen umgeben, um die Drehung in der Mitte des Bildschirms zu zentrieren:

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

Wenn Sie mit dem vierten Schieberegler experimentieren, werden Sie feststellen, dass die unterschiedlichen tiefeneinstellungen das Objekt nicht weiter vom Viewer verschieben, sondern stattdessen den Umfang des perspektivischen Effekts ändern:

[![Dreifacher Screenshot der Drehung 3D-Seite](3d-rotation-images/rotation3d-small.png)](3d-rotation-images/rotation3d-large.png#lightbox "Dreifacher Screenshot der Drehung 3D-Seite")

Die **animierte Drehung 3D** verwendet auch `SKMatrix44` , um eine Text Zeichenfolge im 3D-Raum zu animieren. Das `textPaint` Objekt, das als Feld festgelegt wird, wird im Konstruktor verwendet, um die Begrenzungen des Texts zu bestimmen:

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

Die `OnAppearing` außer Kraft Setzung definiert drei Xamarin.Forms `Animation` Objekte, um die `xRotationDegrees` `yRotationDegrees` Felder, und `zRotationDegrees` zu unterschiedlichen Raten zu animieren. Beachten Sie, dass die Zeiträume dieser Animationen auf Primzahlen (5 Sekunden, 7 Sekunden und 11 Sekunden) festgelegt werden, sodass die Gesamtkombination nur alle 385 Sekunden oder mehr als 10 Minuten wiederholt wird:

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

Wie im vorherigen Programm `PaintCanvas` erstellt der Handler `SKMatrix44` Werte für Drehung und Perspektive und multipliziert Sie:

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

Diese 3D-Drehung ist mit mehreren 2D-Transformationen umgeben, um den Mittelpunkt der Drehung in den Mittelpunkt des Bildschirms zu verschieben und die Größe der Text Zeichenfolge so zu skalieren, dass Sie die gleiche Breite wie der Bildschirm hat:

[![Dreifacher Screenshot der 3D-Seite für animierte Drehung](3d-rotation-images/animatedrotation3d-small.png)](3d-rotation-images/animatedrotation3d-large.png#lightbox "Dreifacher Screenshot der 3D-Seite für animierte Drehung")

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
