---
title: Nicht affine Transformationen
description: In diesem Artikel wird erläutert, wie mit der dritten Spalte der Transformationsmatrix Perspektive und Konikwinkel Effekte zu erstellen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
ms.openlocfilehash: eb7057d40e6ff0c48c6dc1b5dc38af2eb92de2e0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772778"
---
# <a name="non-affine-transforms"></a>Nicht affine Transformationen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erstellen Sie mit der dritten Spalte der Transformationsmatrix Perspektive und Konikwinkel Effekte_

Übersetzung, Skalierung, Drehung und neigen, werden alle als klassifiziert *affine* transformiert. Affine Transformationen beibehalten parallele Linien. Wenn zwei Zeilen vor der Transformation parallel sind, bleiben sie nach der Transformation parallel. Rechtecke werden immer in Parallelograms transformiert.

SkiaSharp ist jedoch auch nicht affine Transformationen, die die Möglichkeit, ein Rechteck in jeder konvexe Viereck transformiert haben kann:

![](non-affine-images/nonaffinetransformexample.png "Eine Bitmap, die in eine konvexe Viereck transformiert")

Eine konvexe Viereck ist eine Abbildung vier Seiten, deren innere Winkel als 180 Grad und Seiten, die einander nicht schneiden nicht immer kleiner.

Nicht affine Transformationen Ergebnis, wenn die dritte Zeile der Transformationsmatrix, die auf andere Werte als 0, 0 und 1 festgelegt ist. Die vollständige `SKMatrix` Multiplikation ist:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Die resultierende Transformation-Formeln sind:

X' = ScaleX·x + SkewX·y + VerschX

y' = SkewY·x + ScaleY·y + VerschY

Z' = Persp0·x + Persp1·y + Persp2

Die zentrale Regel verwenden Sie eine 3 x 3-Matrix für Transformationen, die zweidimensionalen ist, dass alles, was in der Ebene bleibt, wobei Z 1 entspricht. Es sei denn, `Persp0` und `Persp1` sind 0 (null) und `Persp2` gleich 1 ist, die Transformation die Z-Koordinaten aus Ebene verschoben wurde.

Um dies zu einer zweidimensionalen Transformation wiederherzustellen, müssen die Koordinaten zurück auf diese Ebene verschoben werden. Ein weiterer Schritt ist erforderlich. Das "X" ", y", und Z ", müssen Werte z unterteilt werden":

x" = x' / z'

y"y =" / Z "

z" = z' / z' = 1

Diese werden als bezeichnet *homogene Koordinaten* und sie wurden entwickelt, von Mathematiker August Ferdinand Möbius, wesentlich besser bekannt für seine topologische Schwierigkeit bei, den Möbius Streifen.

Wenn Z "ist 0, die Ergebnisse der Division in Koordinaten der unendlich. Tatsächlich war der Möbiuss Motivation für die Entwicklung von homogene Koordinaten die Möglichkeit, unbegrenzte Werte mit finite-Nummern darstellen.

Beim Anzeigen von Grafiken, möchten jedoch zu vermeiden, rendering mit Koordinaten, die transformiert, unendliche Werte. Diese Koordinaten wird nicht gerendert werden. Alles, was ausgetreten ist, diese Koordinaten werden sehr große und visuell wahrscheinlich nicht kohärent.

In dieser Gleichung, Sie möchten nicht den Wert des Z' 0 (null):

Z' = Persp0·x + Persp1·y + Persp2

Daher die folgenden Werte haben einige praktischen Einschränkungen: 

Die `Persp2` Zelle kann entweder 0 (null) oder nicht 0 (null) sein. Wenn `Persp2` ist 0 (null), und dann Z ", das ist in der Regel nicht wünschenswert, da diese häufig in zweidimensionalen Grafiken ist und 0 (null) für den Punkt (0, 0). Wenn `Persp2` ist nicht gleich NULL ist, dann es ohne Datenverlust an Allgemeingültigkeit, ist Wenn `Persp2` ist auf 1 fest. Wenn Sie feststellen, dass z. B. `Persp2` sollte 5, und klicken Sie dann Sie einfach verteilen aller Zellen in der Matrix von 5, wodurch `Persp2` gleich 1, und das Ergebnis ist identisch sein.

Aus diesen Gründen `Persp2` ist häufig behoben bei 1, die den gleichen Wert in die Identitätsmatrix ist.

Im allgemeinen `Persp0` und `Persp1` sind kleine Zahlen. Nehmen wir beispielsweise an, Sie beginnen mit einer Identitätsmatrix jedoch einen Satz `Persp0` auf 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Die Transformation-Formeln sind:

X' = X / (0.01·x + 1)

y' = y / (0.01·x + 1)

Verwenden Sie nun diese Transformation, um ein quadratisches 100-Pixel-Feld, positioniert am ursprünglichen Speicherort zu rendern. Hier ist, wie die vier Ecken transformiert werden:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Wenn x ist, 100, dann ist das Z' Nenner ist 2, damit die x- und y-Koordinaten halbiert werden. Rechts neben dem Feld ist kürzer als der linken Seite:

![](non-affine-images/nonaffinetransform.png "Ein Feld, bei denen sich eine nicht affine Transformation")

Die `Persp` Teil dieser Zelle Namen bezieht sich "Perspektive" auf, da es sich bei der Verkürzung zeigt an, dass das Feld jetzt mit der rechten Seite weiter von der Viewer geneigt ist.

Die **Testperspektive** Seite ermöglicht Ihnen das Experimentieren mit den Werten `Persp0` und `Pers1` um ein Gefühl für die Funktionsweise zu erhalten. Angemessenen Werte für diese Matrixzellen sind so klein, die die `Slider` in die universelle Windows-Plattform kann nicht ordnungsgemäß behandelt werden. Um das Problem der UWP, die beiden aufzunehmen `Slider` Elemente in der [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) müssen Bereich von – 1, 1 initialisiert werden:

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

Die Ereignishandler für den Schieberegler in der [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) Code-Behind-Datei die Werte von 100 so aufteilen, dass zwischen –0.01 und 0,01 reichen. Darüber hinaus lädt der Konstruktor in einer Bitmap aus:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Die `PaintSurface` Ereignishandler berechnet eine `SKMatrix` Wert mit dem Namen `perspectiveMatrix` basierend auf den Werten von diesen beiden Schiebereglern durch 100 dividiert. Diese wird mit kombiniert zwei Übersetzungstransformationen, die den Mittelpunkt der Transformation in den Mittelpunkt der Bitmap eingefügt:

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

Hier sind einige Abbildungen von Beispielen aus:

[![](non-affine-images/testperspective-small.png "Dreifacher Screenshot der Seite Testperspektive")](non-affine-images/testperspective-large.png#lightbox "dreifachen Screenshot der Seite Testperspektive")

Wie Sie mit den Schiebereglern experimentieren möchten, finden Sie, dass die Werte über 0.0066 hinaus oder darunter –0.0066 dazu führen, dass das Bild, das es aufgeteilter und inkohärente werden. Die Bitmap, die transformiert werden, ist 300-Pixel im Quadrat. Es wird relativ zu seinen Mittelpunkt, transformiert, damit die Koordinaten der Bitmap aus –150 auf 150 liegen. Bedenken Sie, dass der Wert des Z "ist:

Z' = Persp0·x + Persp1·y + 1

Wenn `Persp0` oder `Persp1` ist größer als 0.0066 oder unten –0.0066, dann gibt es immer einige Bitmap, die führt einen Z-Koordinate ' Wert 0 (null). Wodurch der Division durch 0 (null), und das Rendering wird ein Chaos. Bei Verwendung nicht affine Transformationen, vermeiden Sie rendern alles mit Koordinaten, die dazu führen, Division durch 0 (null dass) werden soll.

Im Allgemeinen, Sie wird nicht festgelegt `Persp0` und `Persp1` isoliert. Es ist auch häufig zum Festlegen von anderen Zellen in der Matrix, um bestimmte Arten von nicht affine Transformationen zu erzielen.

Eine solche nicht affine Transformation ist eine *Konikwinkel Transformation*. Diese Art von nicht affine Transformation behält die gesamten Dimensionen des Rechtecks, jedoch nimmt eine Seite:

![](non-affine-images/tapertransform.png "Ein Feld, bei denen sich eine Konikwinkel Transformation")

Die [ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Klasse führt eine generalisierte Berechnung eine nicht affine Transformation, die basierend auf diesen Parametern:

- die rechteckigen Größe des Bilds transformiert werden
- eine Enumeration, die die Seite des Rechtecks angibt, die reduziert,
- eine andere Enumeration, der angibt, wie sie reduziert, und
- der Umfang der der Spitz zulaufend.

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

Diese Klasse wird verwendet, der **Konikwinkel transformieren** Seite. Die XAML-Datei instanziiert zwei `Picker` Enumerationswerte auszuwählenden Elemente und ein `Slider` für die Auswahl des Konikwinkel Anteil. Die [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Handler kombiniert die Konikwinkel Transformation mit zwei Übersetzungstransformationen hinzu, die Transformation in Bezug auf die linke obere Ecke der Bitmap stellen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedItem;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedItem;
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

[![](non-affine-images/tapertransform-small.png "Dreifacher Screenshot der Seite Konikwinkel transformieren")](non-affine-images/tapertransform-large.png#lightbox "dreifachen Screenshot der Seite Konikwinkel transformieren")

Ein weiterer generalisierten nicht affine Transformationen 3D-Drehung, die im nächsten Artikel gezeigt wird, ist [ **3D Drehungen**](3d-rotation.md).

Nicht affine Transformation kann ein Rechteck in jeder konvexe Viereck umwandeln. Dies wird veranschaulicht, durch die **nicht Affine Matrix anzeigen** Seite. Es ist sehr ähnlich der **Affine Matrix anzeigen** Seite die [ **Matrix transformiert** ](matrix.md) Artikel mit dem Unterschied, dass es sich um eine vierte verfügt `TouchPoint` Objekt, das das vierte bearbeiten die Ecke der Bitmap:

[![](non-affine-images/shownonaffinematrix-small.png "Dreifacher Screenshot der Seite nicht Affine Matrix anzeigen")](non-affine-images/shownonaffinematrix-large.png#lightbox "dreifachen Screenshot der Seite nicht Affine Matrix anzeigen")

Solange Sie nicht versuchen, einen innere Winkel von einer der Ecken der Bitmap größer als 180 Grad stellen, oder stellen zwei Seiten, die einander nicht schneiden werden, die Anwendung erfolgreich berechnet die Transformation, die mit dieser Methode aus der [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) Klasse:

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

Vereinfachen der Berechnung ruft diese Methode die gesamte Transformation ab, als ein Produkt von drei separaten Transformationen, die hier dadurch mit Pfeilen, die zeigt symbolisiert werden, wie diese Transformationen auf die vier Ecken der Bitmap ändern:

(0, 0) → (0, 0) → (0, 0) → (x 0, y0) (oben links)

(0, H) → (0, 1) → (0, 1) → (X1, y1) (unten links)

(B, 0) → (1, 0) → (1, 0) → (x 2 y2) (rechts)

("B", "H") → (1, 1) → (a, b) → (x 3 y3) (unten rechts)

Die endgültige Koordinaten auf der rechten Seite werden die vier Punkte, die die vier Berührungspunkte zugeordnet. Hierbei handelt es sich um die endgültige Koordinaten der Ecken der Bitmap.

B und H stellen dar, die Breite und Höhe der Bitmap. Die erste Transformation `S` zu einem Quadrat 1-Pixel-Bitmap einfach skaliert werden kann. Die zweite Transformation ist das nicht affine Transformation `N`, und das dritte hingegen ist die Transformation affin `A`. Diese affine Transformation basiert auf drei Punkte, damit hat ebenso wie die zuvor affine [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) Methode und nicht mit die vierte Zeile umfassen die (a, b) zeigen.

Die `a` und `b` Werte berechnet werden, sodass die dritte Transformation affin ist. Der Code Ruft die Umkehrung der affine Transformation und anschließend anhand die unteren rechten Ecke zuordnen. Dies ist der Punkt (a, b).

Eine andere Verwendung von nicht affine Transformationen werden dreidimensionale Grafiken zu imitieren. Im nächsten Artikel [ **3D Drehungen** ](3d-rotation.md) veranschaulicht die zweidimensionale Grafik im 3D-Raum zu drehen.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
