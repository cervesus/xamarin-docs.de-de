---
title: Nicht Affine Transformationen
description: Erstellen Sie mit der dritten Spalte der Transformationsmatrix Perspektive und Konikwinkel Effekte
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 3fda8524b824042aa4aba07853da2801baf47027
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="non-affine-transforms"></a>Nicht Affine Transformationen

_Erstellen Sie mit der dritten Spalte der Transformationsmatrix Perspektive und Konikwinkel Effekte_

Übersetzung, Skalierung, Drehung und Neigung, werden alle als klassifiziert *affine* transformiert. Affine Transformationen beibehalten parallele Linien. Wenn zwei Zeilen vor der Transformation parallel sind, bleiben sie nach der Transformation parallel. Rechtecke werden immer in Parallelograms transformiert.

SkiaSharp ist allerdings auch nicht affinen Transformationen, die über die Fähigkeit zum Transformieren ein Rechteck in jeder konvexe Quadrat kann:

![](non-affine-images/nonaffinetransformexample.png "Eine Bitmap, die in eine konvexe Quadrat transformiert")

Eine konvexe Quadrat ist eine Abbildung vier Seiten, deren innere Winkel immer kleiner als 180 Grad und Seiten, die einander nicht schneiden nicht.

Nicht affinen transformiert Ergebnis, wenn die dritte Zeile der Transformationsmatrix Werte als 0, 0 und 1 festgelegt ist. Die vollständige `SKMatrix` Multiplikation ist:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Die resultierenden Transform-Formeln sind:

X "= ScaleX·x + SkewX·y + VerschX

y "= SkewY·x + ScaleY·y + VerschY

Z' = Persp0·x + Persp1·y + Persp2

Die zentrale Regel der mit einer 3 x 3-Matrix für Transformationen, die zweidimensionalen ist, dass alles auf der Ebene bleibt, wobei Z 1 entspricht. Es sei denn, `Persp0` und `Persp1` sind 0 (null) und `Persp2` gleich 1 ist, die Transformation die Z-Koordinaten, Ebene verschoben wurde.

Um dies zu einer zweidimensionalen Transformation wiederherzustellen, müssen die Koordinaten zurück in diese Ebene verschoben werden. Ein weiterer Schritt ist erforderlich. Das "X" ", y", und Z 'müssen Werte durch z geteilt":

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

Diese werden als bezeichnet *homogene Koordinaten* und sie wurden entwickelt, indem Mathematiker August Ferdinand Möbius, wesentlich besser bekannten für seine topologische Schwierigkeit bei den Möbius Streifen.

Wenn Z "0", "die Division Ergebnisse in unendlich Koordinaten ist. Tatsächlich war es eines Möbiuss Beweggründe für die Entwicklung von homogene Koordinaten die Möglichkeit, die mit einer endlichen Anzahl unendliche Werte darstellen.

Beim Anzeigen von Grafiken, sollten Sie jedoch vermieden, Rendern etwas mit den Koordinaten, die auf unbegrenzte Werte umwandeln. Diese Koordinaten wird nicht gerendert werden. Alles, was in diese Koordinaten werden sehr große und wahrscheinlich nicht visuell kohärente.

In dieser Formel möchten nicht die Z-Wert "0 (null):

Z' = Persp0·x + Persp1·y + Persp2

Die `Persp2` Zelle kann 0 (null) oder nicht 0 (null) sein. Wenn `Persp2` ist 0 (null), und dann Z "ist NULL für den Punkt (0, 0), und, das ist normalerweise nicht wünschenswert, da dieser Punkt häufig in zweidimensionaler Grafiken ist. Wenn `Persp2` ist ungleich 0 (null), und es kein Datenverlust der Verzicht auf, entsteht Wenn `Persp2` ist auf 1 fest. Wenn Sie feststellen, dass z. B. `Persp2` sollte 5, und Sie einfach teilen können alle Zellen in der Matrix durch 5, wodurch `Persp2` gleich 1 und das Ergebnis wird gleich sein.

Aus diesen Gründen `Persp2` ist häufig festen bei 1, die den gleichen Wert in die Identitätsmatrix ist.

Im allgemeinen `Persp0` und `Persp1` sind kleine Zahlen. Nehmen wir beispielsweise an, Sie beginnen mit einer Identitätsmatrix, sondern Satz `Persp0` auf 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Die Transformation-Formeln sind:

X "= X / (0.01·x + 1)

y "= y / (0.01·x + 1)

Jetzt verwenden Sie diese Transformation zum Rendern einer 100 Pixel Rechteck am ursprünglichen Speicherort positioniert. Hier ist wie die vier Ecken transformiert werden:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Wenn x ist, 100, und klicken Sie dann auf der Z "Nenner ist 2, damit die x- und y-Koordinaten sind effektiv halbiert. Rechts neben dem Feld wird kürzer als der linken Seite:

![](non-affine-images/nonaffinetransform.png "Ein Feld vorhanden ist, die eine nicht affinen Transformation")

Die `Persp` Teil dieser Zelle Namen bezieht sich auf "Perspektive", da die Verkürzung vorgeschlagen, dass das Feld jetzt mit der rechten Seite von der Viewer geneigt ist.

Die **Testperspektive** Seite ermöglicht Ihnen das Experimentieren mit Werten von `Persp0` und `Pers1` um einen Eindruck darüber erhalten, für deren Funktionsweise. Sinnvolle Werte diese Matrixzellen so klein sind, die die `Slider` in universellen Windows-Plattform kann nicht ordnungsgemäß behandelt werden. Um das Problem universelle Windows-Plattform, die beiden Rechnung zu tragen `Slider` Elemente in der [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) Bereich von – 1, 1 initialisiert werden müssen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
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
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Die Ereignishandler für die Schieberegler in der [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) Code-Behind-Datei die Werte mit 100 so aufteilen, dass sie im Bereich zwischen –0.01 und 0,01 US-Dollar. Darüber hinaus werden der Konstruktor in eine Bitmap geladen:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
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

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

Die `PaintSurface` Ereignishandler berechnet eine `SKMatrix` -Wert namens `perspectiveMatrix` basierend auf den Werten von diesen beiden Schiebereglern durch 100 dividiert. Dies ist die Kombination mit zwei Transformationen, die das Zentrum des dieser Transformation in der Mitte der Bitmap gestellt übersetzen:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Hier sind einige Beispielbilder:

[![](non-affine-images/testperspective-small.png "Dreifacher Screenshot der Seite Testperspektive")](non-affine-images/testperspective-large.png "dreifacher Screenshot der Seite Testperspektive")

Wie Sie die Schieberegler experimentieren, finden Sie, dass die Werte über 0.0066 oder unter –0.0066 dazu führen, dass das Bild, das es aufgeteilter und inkohärente werden. Die Bitmap, die transformiert werden ist 300 Pixel Quadrat. Es ist relativ zu ihren Mittelpunkt transformiert, damit die Koordinaten der Bitmap für die im Bereich von –150 auf 150. Bedenken Sie, dass der Wert von Z "ist:

Z' = Persp0·x + Persp1·y + 1

Wenn `Persp0` oder `Persp1` ist größer als 0.0066 oder unten –0.0066, besteht immer einige Bitmap, die in ein Z-Koordinate "der Wert 0 (null). Die Division durch 0 (null) darstellt, und das Rendering wird ein Chaos. Wenn Sie nicht affine Transformationen zu verwenden, möchten Sie vermeiden Rendern macht nichts mit den Koordinaten, die dazu führen, Division durch 0 (null dass).

Im Allgemeinen, Sie wird nicht festlegen `Persp0` und `Persp1` isoliert. Es ist auch häufig zum Festlegen von anderen Zellen in der Matrix, um bestimmte Typen von nicht-affine Transformationen zu erzielen.

Eine solche nicht affinen "Transformieren" ist ein *Konikwinkel Transformation*. Diese Art von nicht affinen Transformation behält die gesamten Dimensionen eines Rechtecks jedoch nimmt einseitige:

![](non-affine-images/tapertransform.png "Ein Feld vorhanden ist, die eine Konikwinkel Transformation")

Die [ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Klasse führt eine generalisierte Berechnung einer nicht affinen Transformation basierend auf diesen Parametern:

- die rechteckigen Größe des Bilds transformiert werden
- eine Enumeration, die die Seite des Rechtecks gibt an, die reduziert,
- eine andere Enumeration, der angibt, wie es nimmt und
- das Ausmaß von der Spitz zulaufend.

Hier ist der Code ein:

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

Diese Klasse wird verwendet, der **Konikwinkel transformieren** Seite. Die XAML-Datei instanziiert zwei `Picker` Elemente die Aufzählungswerte auswählen und eine `Slider` für die Auswahl des Konikwinkel Bruch. Die [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Handler kombiniert die Konikwinkel Transformation mit zwei übersetzen Transformationen, um die Transformation relativ zur linken oberen Ecke der Bitmap zu machen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

Hier einige Beispiele:

[![](non-affine-images/tapertransform-small.png "Dreifacher Screenshot der Seite Konikwinkel transformieren")](non-affine-images/tapertransform-large.png "dreifacher Screenshot der Seite Konikwinkel transformieren")

Ein weiterer Typ von generalisierte nicht affine Transformationen sind 3D-Drehung in das nächste Dokument veranschaulicht wird, [3D Drehungen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Die Transformation nicht affinen kann ein Rechteck in jeder konvexe Quadrat transformiert werden. Dies wird veranschaulicht, durch die **nicht affinen Matrix anzeigen** Seite. Es ist sehr ähnlich der **Affine Matrix anzeigen** Seite aus der [Matrix transformiert](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) Artikel mit dem Unterschied, dass es sich um eine vierte verfügt `TouchPoint` Objekt zum Bearbeiten der vierten Ecke der Bitmap:

[![](non-affine-images/shownonaffinematrix-small.png "Dreifacher Screenshot der Seite nicht affinen Matrix anzeigen")](non-affine-images/shownonaffinematrix-large.png "Triple Screenshot, der die Seite mit nicht affinen Matrix anzeigen")

Solange Sie versuchen nicht, einen innere Winkel von einer der Ecken der Bitmap größer als 180 Grad oder beiden Seiten, die einander nicht schneiden, berechnet das Programm erfolgreich die Transformation, die mit dieser Methode aus der [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) Klasse:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
{
    // Scale transform
    SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

    // Affine transform
    SKMatrix A = new SKMatrix
    {
        ScaleX = ptUR.X - ptUL.X,
        SkewY = ptUR.Y - ptUL.Y,
        SkewX = ptLL.X - ptUL.X,
        ScaleY = ptLL.Y - ptUL.Y,
        TransX = ptUL.X,
        TransY = ptUL.Y,
        Persp2 = 1
    };

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

Zur Vereinfachung der Berechnung ruft diese Methode die gesamte Transformation als Produkt aus drei Transformationen, die die symbolisiert hier mit Pfeilen wie dieser Transformationen für die vier Ecken der Bitmap geändert werden:

(0, 0) → (0, 0) → (0, 0) → (x 0, y0) (oben links)

(0, H) → (0, 1) → (0, 1) → (X1 y1) (unten links)

(W, 0) → (1, 0) → (1, 0) → (x 2 y2) (rechts)

(B, H) → (1, 1) → (a, b) → (x 3 y3) (unten rechts)

Die endgültige Koordinaten auf der rechten Seite werden die vier Punkte, die vier Berührungspunkte zugeordnet. Hierbei handelt es sich um die endgültige Koordinaten der Ecken der Bitmap.

W und H, stellen Sie die Breite und Höhe der Bitmap dar. Die erste Transformation (`S`) einfach das Bitmuster in ein Quadrat 1 Pixel skaliert. Die zweite Transformation wird die Transformation nicht affinen `N`, und der dritte ist der affine Transformation `A`. Diese affine Transformation basiert auf drei Punkte, damit hat ebenso wie die zuvor affine [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) Methode und nicht mit die vierte Zeile umfassen die (a, b) zeigen.

Die `a` und `b` Werte berechnet werden, damit die dritte Transformation affin ist. Der Code Ruft die Umkehrung der affine Transformation ab und verwendet, die dann zum Zuordnen der unteren rechten Ecke. Dies ist der Punkt (a, b).

Eine weitere Verwendungsmöglichkeit nicht affine Transformationen ist dreidimensionale Grafiken zu imitieren. Im nächsten Artikel [3D Drehungen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) angezeigt wie eine zweidimensionale Grafik im 3D-Raum drehen.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
