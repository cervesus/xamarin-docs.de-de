---
title: Nicht affine Transformationen
description: In diesem Artikel wird erläutert, wie Sie Perspektiven und tapro-Effekte mit der dritten Spalte der Transformationsmatrix erstellen und dies mit Beispielcode veranschaulichen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 91a639b2d3c2f6a8437a09a70808dc6d793ba76b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131754"
---
# <a name="non-affine-transforms"></a>Nicht affine Transformationen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erstellen von Perspektiven und tapro-Effekten mit der dritten Spalte der Transformationsmatrix_

Übersetzung, Skalierung, Drehung und Neigung werden alle als *affine* Transformationen klassifiziert. Affine Transformationen bewahren parallele Zeilen auf. Wenn zwei Zeilen vor der Transformation parallel sind, bleiben Sie nach der Transformation parallel. Rechtecke werden immer zu parallelograms transformiert.

Skiasharp kann jedoch auch nicht affine Transformationen bieten, die die Möglichkeit haben, ein Rechteck in eine beliebige, Viereck Weise zu transformieren:

![](non-affine-images/nonaffinetransformexample.png "A bitmap transformed into a convex quadrilateral")

Ein "intervexes vierlateral" ist eine vierseitige Abbildung mit inneren Winkeln, die immer kleiner als 180 Grad und Seiten sind, die sich nicht gegenseitig überschreiten.

Nicht affine Transformationen ergeben sich, wenn die dritte Zeile der Transformationsmatrix auf andere Werte als 0, 0 und 1 festgelegt ist. Die vollständige `SKMatrix` Multiplikation lautet:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Die resultierenden transformationsformeln lauten:

x ' = ScaleX · x + schief, y + TransX

y ' = schrägen · x + ScaleY = y + transy

z ' = Persp0 · x + Persp1 = y + Persp2

Die grundlegende Regel der Verwendung einer 3-x-3-Matrix für zweidimensionale Transformationen besteht darin, dass alles auf der Ebene bleibt, wobei Z gleich 1 ist. Wenn `Persp0` und `Persp1` 0 sind und `Persp2` 1 ist, hat die Transformation die Z-Koordinaten von dieser Ebene verschoben.

Um dies in einer zweidimensionalen Transformation wiederherzustellen, müssen die Koordinaten zurück auf diese Ebene verschoben werden. Ein weiterer Schritt ist erforderlich. Die x-, y-und z-Werte müssen durch z "dividiert werden:

x "= x"/z "

y "= y"/z "

z "= z"/z "= 1

Diese werden als *homogene Koordinaten* bezeichnet und vom Mathematiker August Ferdinand Möbius entwickelt, die für die topologische oddität, den Möbius-Strip, viel besser bekannt ist.

Wenn z ' 0 ist, führt die Division zu unendlichen Koordinaten. Tatsächlich war eine der Gründe für die Entwicklung homogener Koordinaten die Möglichkeit, unendliche Werte mit begrenzten Zahlen darzustellen.

Wenn Sie Grafiken anzeigen, möchten Sie jedoch vermeiden, dass Sie etwas mit Koordinaten rendern, die in unendliche Werte transformiert werden. Diese Koordinaten werden nicht gerendert. Alles in der Nähe dieser Koordinaten ist sehr groß und wahrscheinlich nicht visuell kohärent.

In dieser Gleichung möchten Sie nicht, dass der Wert von z "NULL wird:

z ' = Persp0 · x + Persp1 = y + Persp2

Folglich haben diese Werte einige praktische Einschränkungen: 

Die `Persp2` Zelle kann entweder NULL oder ungleich 0 (null) sein. Wenn `Persp2` 0 (null) ist, dann ist z ' 0 (null) für den Punkt (0, 0), und das ist normalerweise nicht wünschenswert, da dieser Punkt in zweidimensionalen Grafiken sehr häufig vorkommt. Wenn ungleich `Persp2` 0 (null) ist, gibt es keinen Verlust der Generalität, wenn `Persp2` bei 1 korrigiert wird. Wenn Sie z. b. feststellen, dass `Persp2` 5 sein soll, können Sie einfach alle Zellen in der Matrix um 5 aufteilen, was den Wert `Persp2` 1 ergibt und das Ergebnis gleich ist.

Aus diesen Gründen `Persp2` wird häufig bei 1 festgeschrieben. Dies ist der gleiche Wert in der Identitätsmatrix.

Im allgemeinen `Persp0` `Persp1` sind und kleine Zahlen. Nehmen wir beispielsweise an, Sie beginnen mit einer Identitätsmatrix, legen aber `Persp0` auf 0,01 fest:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Die transformationsformeln lauten wie folgt:

x ' = x/(0,01 · x + 1)

y ' = y/(0,01 · x + 1)

Verwenden Sie diese Transformation nun, um eine auf dem Ursprung positionierte 100-Pixel-Quadratische Box zu rendern. Im folgenden wird erläutert, wie die vier Ecken transformiert werden:

(0,0) → (0,0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Wenn x 100 ist, dann ist der z-Nenner 2, sodass die x-und y-Koordinaten tatsächlich halbiert werden. Die Rechte Seite des Felds wird kürzer als die linke Seite:

![](non-affine-images/nonaffinetransform.png "A box subjected to a non-affine transform")

Der `Persp` Teil dieser Zellen Namen bezieht sich auf "Perspective", da die Vorhersage darauf hinweist, dass das Feld nun mit der rechten Seite des Viewers gekippt wird.

Auf der Seite **Test Perspektive** können Sie mit Werten von `Persp0` und experimentieren `Pers1` , um ein Gefühl für die Funktionsweise zu erhalten. Angemessene Werte dieser Matrixzellen sind so klein, dass die `Slider` im universelle Windows-Plattform Sie nicht ordnungsgemäß verarbeiten können. Um dem UWP-Problem Rechnung zu tragen, müssen die beiden `Slider` Elemente in der Datei " [**testperspective. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) " initialisiert werden, um zwischen – 1 und 1 zu reichen:

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

Die Ereignishandler für die Schieberegler in der [`TestPerspectivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) Code-Behind-Datei dividieren die Werte durch 100, sodass Sie zwischen – 0,01 und 0,01 liegen. Außerdem lädt der Konstruktor in eine Bitmap:

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

Der- `PaintSurface` Handler berechnet einen `SKMatrix` Wert mit dem Namen `perspectiveMatrix` auf der Grundlage der Werte dieser beiden Schieberegler dividiert durch 100. Dies wird mit zwei Translation-Transformationen kombiniert, die den Mittelpunkt dieser Transformation in der Mitte der Bitmap platzieren:

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

Im folgenden finden Sie einige Beispiel Bilder:

[![](non-affine-images/testperspective-small.png "Triple screenshot of the Test Perspective page")](non-affine-images/testperspective-large.png#lightbox "Triple screenshot of the Test Perspective page")

Wenn Sie mit den Schiebereglern experimentieren, werden Sie feststellen, dass die Werte über 0,0066 oder niedriger – 0,0066 bewirken, dass das Bild plötzlich Bruch und inkohärent wird. Die zu transformierende Bitmap ist 300-Pixel-Quadrat. Es wird relativ zu seiner Mitte transformiert, sodass die Koordinaten des Bitmap von – 150 bis 150 liegen. Erinnern Sie sich daran, dass der Wert von z ' ist:

z ' = Persp0 · x + Persp1 = y + 1

Wenn `Persp0` oder `Persp1` größer als 0,0066 oder kleiner als – 0,0066 ist, gibt es immer eine Koordinate der Bitmap, die zu einem z-Wert von 0 (null) führt. Dies bewirkt eine Division durch 0 (null), und das Rendering wird zu einem Chaos. Wenn Sie nicht-affine Transformationen verwenden, möchten Sie vermeiden, dass Elemente mit Koordinaten gerendert werden, die eine Division durch Null verursachen.

Im Allgemeinen werden Sie nicht `Persp0` und isoliert festgelegt `Persp1` . Es ist auch häufig notwendig, andere Zellen in der Matrix festzulegen, um bestimmte Typen von nicht affinen Transformationen zu erzielen.

Eine solche nicht affinen Transformation ist eine *tapro-Transformation*. Dieser Typ einer nicht affinen Transformation behält die Gesamt Dimensionen eines Rechtecks bei, aber eine Seite:

![](non-affine-images/tapertransform.png "A box subjected to a taper transform")

Die- [`TaperTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Klasse führt eine verallgemeinerte Berechnung einer nicht affinen Transformation basierend auf diesen Parametern aus:

- die rechteckige Größe des transformierten Bilds.
- eine Enumeration, die die Seite des Rechtecks angibt, das durch tattel
- eine weitere Enumeration, die angibt, wie Sie taert und
- der Umfang der durch taperung.

Der Code lautet wie folgt:

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

Diese Klasse wird auf der Seite " **tapro Transform** " verwendet. Die XAML-Datei instanziiert zwei `Picker` Elemente, um die Enumerationswerte auszuwählen, und eine `Slider` zum Auswählen des tapro-Bruchteils. Der [`PaintSurface`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Handler kombiniert die tapro-Transformation mit zwei Translation-Transformationen, um die Transformation relativ zur linken oberen Ecke der Bitmap vorzunehmen:

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

Im Folgenden finden Sie einige Beispiele:

[![](non-affine-images/tapertransform-small.png "Triple screenshot of the Taper Transform page")](non-affine-images/tapertransform-large.png#lightbox "Triple screenshot of the Taper Transform page")

Ein anderer Typ generalisierter nicht affininer Transformationen ist die 3D-Drehung, die im nächsten Artikel [**3D-Drehungen**](3d-rotation.md)veranschaulicht wird.

Die nicht affine Transformation kann ein Rechteck in eine beliebige, Viereck-und Viereck Umwandlung umwandeln. Dies wird auf der Seite **nicht-affine Matrix anzeigen** veranschaulicht. Sie ähnelt der Seite **affine Matrix anzeigen** im Artikel [**Matrix Transformationen**](matrix.md) , mit der Ausnahme, dass Sie über ein viertes `TouchPoint` Objekt verfügt, um die vierte Ecke der Bitmap zu bearbeiten:

[![](non-affine-images/shownonaffinematrix-small.png "Triple screenshot of the Show Non-Affine Matrix page")](non-affine-images/shownonaffinematrix-large.png#lightbox "Triple screenshot of the Show Non-Affine Matrix page")

Solange Sie nicht versuchen, einen inneren Winkel von einer der Ecken der Bitmap größer als 180 Grad zu machen, oder zwei Seiten nebeneinander machen, berechnet das Programm die Transformation erfolgreich mithilfe dieser Methode aus der- [`ShowNonAffineMatrixPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) Klasse:

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

Zur Erleichterung der Berechnung erhält diese Methode die gesamte Transformation als Produkt von drei separaten Transformationen, die hier mit Pfeilen symbolisiert werden, die zeigen, wie diese Transformationen die vier Ecken der Bitmap ändern:

(0,0) → (0,0) → (0,0) → (X0, Y0) (oben links)

(0, H) → (0,0) → (0,0) → (x1, Y1) (unten links)

(W, 0) → (1, 0) → (1, 0) → (x2, Y2) (oben rechts)

(W, H) → (1, 1) → (a, b) → (X3, Y3) (unten rechts)

Die letzten Koordinaten auf der rechten Seite sind die vier Punkte, die den vier Berührungspunkten zugeordnet sind. Dabei handelt es sich um die endgültigen Koordinaten der Ecken der Bitmap.

W und H stellen die Breite und Höhe der Bitmap dar. Mit der ersten Transformation wird `S` die Bitmap einfach auf ein 1-Pixel-Quadrat skaliert. Die zweite Transformation ist die nicht affine Transformation `N` , die dritte ist die affine Transformation `A` . Diese affine Transformation basiert auf drei Punkten, Sie ist also genau wie die frühere affine- [`ComputeMatrix`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) Methode und umfasst nicht die vierte Zeile mit dem (a, b) Punkt.

Der `a` -Wert und der- `b` Wert werden berechnet, sodass die dritte Transformation affin ist. Der Code Ruft die Umkehrung der affinen Transformation ab und verwendet diese dann zum Zuordnen der unteren rechten Ecke. Das ist der Punkt (a, b).

Eine weitere Verwendung von nicht affinen Transformationen ist die imitiert dreidimensionaler Grafiken. Im nächsten Artikel [**3D-Drehungen**](3d-rotation.md) sehen Sie, wie eine zweidimensionale Grafik im 3D-Raum gedreht wird.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
