---
title: Tippen Sie auf Manipulationen
description: Matrixtransformationen verwenden, um Touch ziehen, Pinch und Drehung implementieren
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 16e9423c84e591e15a703b4d5bb204a8b642bb40
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="touch-manipulations"></a>Tippen Sie auf Manipulationen

_Matrixtransformationen verwenden, um Touch ziehen, Pinch und Drehung implementieren_

In Multi-Touch-Umgebungen, wie z. B. die auf mobilen Geräten verwenden die Benutzer häufig ihre Finger um Objekte auf dem Bildschirm zu bearbeiten. Allgemeine Gesten wie einem Finger ziehen und eine zwei-Finger-Prise können verschieben und Objekten skalieren, oder sogar zu drehen. Diese Aktionen werden in der Regel mithilfe von Transformation Matrizen implementiert, und diesem Artikel erfahren Sie, wie das geht.

![](touch-images/touchmanipulationsexample.png "Eine Bitmap, die vorhanden ist, die Übersetzung, Skalierung und Drehung")

## <a name="manipulating-one-bitmap"></a>Bearbeiten einer Bitmap

Die **berühren Manipulation** Touch Manipulationen an eine einzelne Bitmap veranschaulicht.
Dieses Beispiel verwendet die Touch-Überwachung Effekte, die im Artikel dargestellt [Aufrufen Ereignisse von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Mehrere andere Dateien bieten Unterstützung für die **berühren Manipulation** Seite. Die erste ist die [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) -Enumeration, die gibt die verschiedenen Typen von Touch Manipulation implementiert durch den Code, der Sie angezeigt werden müssen:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` wird einem Finger ziehen, die bei der Übersetzung implementiert wird. Die nachfolgenden Optionen auch gehören Schwenken erfordern jedoch zwei Fingern fest: `IsotropicScale` ist ein zwei-Finger-Vorgang, der das Objekt in horizontaler und vertikaler Richtung gleichermaßen Skalierung ausgibt. `AnisotropicScale` ermöglicht das ungleich skalieren.

Die `ScaleRotate` Option ist für zwei-Finger-Skalierung und Drehung. Skalierung ist ein kugelstrahler. Implementieren zwei Finger Drehung mit anisotrope Skalierung ist problematisch, da die Finger-Bewegungen, die im Wesentlichen identisch sind.

Die `ScaleDualRotate` Option Fügt einem Finger Drehung. Wenn Sie von ein einzelnen Finger das Objekt gezogen, wird das gezogene Objekt, damit die Mitte des Objekts mit dem Ziehen Initialisierungsvektor ausgerichtet zuerst um ihren Mittelpunkt gedreht.

Die [ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) -Datei enthält eine `Picker` mit den Elementen der `TouchManipulationMode` Enumeration:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>

        <Grid BackgroundColor="White"
              Grid.Row="1">

            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

Unten ist ein `SKCanvasView` und eine `TouchEffect` angefügt, die in einer Zelle befindliches `Grid` , das er einschließt.

Die [ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) Code-Behind-Datei hat eine `bitmap` Feld, aber es ist nicht vom Typ `SKBitmap`. Der Typ ist `TouchManipulationBitmap` (eine Klasse wird in Kürze angezeigt):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Der Konstruktor instanziiert einen `TouchManipulationBitmap` Objekt, das an den Konstruktor übergeben ein `SKBitmap` aus einer eingebetteten Ressource abgerufen. Der Konstruktor wird beendet, indem Sie festlegen der `Mode` Eigenschaft von der `TouchManager` Eigenschaft der `TouchManipulationBitmap` Objekt, das Mitglied der der `TouchManipulationMode` Enumeration.

Die `SelectedIndexChanged` Handler für das `Picker` setzt dies auch `Mode` Eigenschaft:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

Die `TouchAction` Handler, der die `TouchEffect` instanziiert in der XAML-Datei ruft zwei Methoden im `TouchManipulationBitmap` mit dem Namen `HitTest` und `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Wenn die `HitTest` -Methode zurückkehrt `true` & #x 2014; bedeutet, dass ein Finger den Bildschirm innerhalb des Bereichs, der von der Bitmap & #x 2014; belegt berührt werden hat, und klicken Sie dann die Touch ID wurde die `TouchIds` Auflistung. Diese ID stellt die Reihenfolge der Berührungsereignisse für diese Finger dar, bis Sie den Finger vom Bildschirm hebt. Wenn mehrere Finger die Bitmap, tippen Sie dann die `touchIds` Auflistung enthält eine Touch ID für jeden Finger.

Die `TouchAction` Handler ruft auch die `ProcessTouchEvent` in Klasse `TouchManipulationBitmap`. Dies ist, wenn einige (aber nicht alle) von der tatsächlichen Fingereingabe Verarbeitung erfolgt.

Die [ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Klasse ist eine Wrapperklasse für `SKBitmap` enthält Code zum Rendern der Bitmap und Touch-Ereignisse verarbeiten. Funktioniert in Kombination mit mehr generalisiert Code in eine `TouchManipulationManager` -Klasse (die Sie in Kürze sehen).

Die `TouchManipulationBitmap` Konstruktor speichert die `SKBitmap` und instanziiert zwei Eigenschaften, die `TouchManager` Eigenschaft vom Typ `TouchManipulationManager` und `Matrix` Eigenschaft vom Typ `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

Dies `Matrix` Eigenschaft ist die kumulierte Transformation, die sich aus der Touch-Aktivität ergeben. Wie Sie sehen, wird jedes Touch-Ereignis aufgelöst, in einer Matrix, die dann verkettet ist, mit der `SKMatrix` vom gespeicherten Wert der `Matrix` Eigenschaft.

Die `TouchManipulationBitmap` Objekt selbst zeichnet seine `Paint` Methode. Das Argument ist ein `SKCanvas` Objekt. Dies `SKCanvas` möglicherweise bereits eine Transformation, die darauf angewendet, sodass die `Paint` Methode verkettet die `Matrix` Eigenschaft der Bitmap für die vorhandene Transformation zugeordnet und im Zeichenbereich wird wiederhergestellt, wenn er abgeschlossen ist:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

Die `HitTest` -Methode zurückkehrt `true` , wenn der Benutzer den Bildschirm zu einem Zeitpunkt innerhalb der Grenzen der Bitmap berührt. Wie der Benutzer die Bitmap bearbeitet, wird die Bitmap gedreht werden kann, oder selbst (über eine Kombination aus anisotrope Skalierung und Drehung) werden in der Form eines Parallelogramms. Sie können jedoch, die die `HitTest` Methode muss sehr komplex analytisch Geometry in diesem Fall implementieren.

Allerdings ist eine Verknüpfung verfügbar:

Bestimmen, ob ein Punkt innerhalb der Grenzen eines Rechtecks, das transformierte liegt entspricht die bestimmen, ob ein umgekehrter transformierter Punkt innerhalb der Grenzen des Rechtecks BoundingBox liegt. Ist viel einfacher Berechnungen und sie können die komfortable `Contains` definierte Methode `SKRect`:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

Die zweite öffentliche Methode in `TouchManipulationBitmap` ist `ProcessTouchEvent`. Wenn diese Methode aufgerufen wird, wurde es, dass das Touch-Ereignis für dieses bestimmte Bitmap gehört bereits eingerichtet. Die Methode verwaltet ein Wörterbuch mit [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) Objekte, die einfach am vorherigen Punkt und den neuen Punkt der einzelnen Finger ist:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Hier wird das Wörterbuch und die `ProcessTouchEvent` Methode selbst:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

In der `Moved` und `Released` Ereignisse, die Methodenaufrufe `Manipulate`. Zu diesen Zeitpunkten der `touchDictionary` enthält ein oder mehrere `TouchManipulationInfo` Objekte. Wenn die `touchDictionary` enthält mindestens ein Element, es ist wahrscheinlich, die die `PreviousPoint` und `NewPoint` Werte ungleich sind, und die Verschiebung mit einem Finger darstellen. Wenn mehrere Finger berühren sich die Bitmap, das Wörterbuch mehr als ein Element enthält, aber nur eines dieser Elemente verfügt über verschiedene `PreviousPoint` und `NewPoint` Werte. Alle übrigen haben gleich `PreviousPoint` und `NewPoint` Werte.

Dies ist wichtig: die `Manipulate` Methode kann davon ausgehen, dass er die Verschiebung der nur ein Finger verarbeitet wird. Zum Zeitpunkt dieses Aufrufs werden keine der anderen Finger verschieben, und wenn sie eigentlich (wie wahrscheinlich) verschieben,-Bewegungen, die verarbeitet werden in späteren Aufrufen `Manipulate`.

Die `Manipulate` Methode zuerst das Wörterbuch in ein Array der Einfachheit halber kopiert. Etwas anderes als die ersten beiden Einträge ignoriert. Wenn mehr als zwei Finger versuchen, die Bearbeitung der Bitmap, werden die anderen ignoriert. `Manipulate` das letzte Element der `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Wenn ein Finger Bitmap bearbeiten ist `Manipulate` Aufrufe der `OneFingerManipulate` Methode der `TouchManipulationManager` Objekt. Für zwei Finger ruft `TwoFingerManipulate`. Die Argumente für diese Methoden sind identisch: die `prevPoint` und `newPoint` Argumente darstellen, den Finger wieder an, die verschoben werden. Aber die `pivotPoint` Argument wird für die beiden Aufrufe unterschiedlich:

Für die Bearbeitung von einem Finger den `pivotPoint` steht im Mittelpunkt der Bitmap. Dies ist ein Finger Drehung zu ermöglichen. Für die Bearbeitung von zwei-Finger das Ereignis die Verschiebung der nur ein Finger gibt, damit die `pivotPoint` wird von den Finger wieder an, die nicht verschoben wird.

In beiden Fällen `TouchManipulationManager` gibt eine `SKMatrix` -Wert, der die Methode mit dem aktuellen verkettet `Matrix` Eigenschaft, die `TouchManipulationPage` verwendet, um das Bitmuster zu rendern.

`TouchManipulationManager` generalisiert ist und keine anderen Dateien außer verwendet `TouchManipulationMode`. Sie können diese Klasse ohne Änderung in den eigenen Anwendungen verwenden können. Er definiert eine einzelne Eigenschaft vom Typ `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Aber Sie müssen wahrscheinlich vermeiden möchten die `AnisotropicScale` Option. Es ist sehr einfach, mit diese Option, um die Bearbeitung der Bitmap, sodass einer der Faktoren, die Skalierung auf 0 (null) ist. Stellt die Bitmap aus der Sicht, nie zurückzugebenden nicht mehr angezeigt. Wenn Sie die anisotrope Skalierung tatsächlich benötigen, sollten Sie verbessern die Logik, um unerwünschte Ergebnisse zu vermeiden.

`TouchManipulationManager` binärencoder Vektoren, aber da es ist keine `SKVector` Struktur in SkiaSharp, `SKPoint` wird stattdessen verwendet. `SKPoint` unterstützt, die der Subtraktionsoperator und das Ergebnis als ein Vektor behandelt werden kann. Vektor-spezifische-Logik, die erforderlich sind, hinzugefügt werden wird eine `Magnitude` Berechnung:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Wenn Drehung ausgewählt wurde, behandeln Sie zunächst die einem Finger und zwei-Finger textmanipulationsmethoden die Drehung. Wenn Drehung erkannt wird, wird die rotationskomponente effektiv entfernt. Was bleibt, wird als Schwenken und Skalierung interpretiert.

So sieht die `OneFingerManipulate` Methode. Wenn ein Finger Rotation nicht aktiviert wurde, ist die Logik einfache & #x 2014. einfach verwendet die vorherigen Punkt und den neuen Punkt So erstellen Sie einen Vektor mit dem Namen `delta` , entspricht genau Übersetzung. Mit einem Finger Drehung aktiviert verwendet die Methode Winkel aus der Dreh-und Angelpunkt (in der Mitte der Bitmap) mit dem vorherigen Punkt und den neuen Punkt erstellt eine Rotationsmatrix.

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

In der `TwoFingerManipulate` -Methode, der Dreh-und Angelpunkt ist die Position des Fingers, die in diesem bestimmten Touch-Ereignis nicht bewegt wird. Die Rotation ist vergleichbar mit einem Finger Drehung, und klicken Sie dann der Vektor mit dem Namen `oldVector` (basierend auf den vorherigen Zustand) ist für die Drehung angepasst. Die verbleibenden Bewegung wird als Skalierung interpretiert:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Sie werden bemerken, dass keine explizite Übersetzung in dieser Methode vorhanden ist. Jedoch sowohl die `MakeRotation` und `MakeScale` Methoden basieren auf der Dreh-und Angelpunkt sowie implizite Übersetzung enthält. Bei Verwendung von zwei Fingern auf die Bitmap und ziehen sie in der gleichen Richtung `TouchManipulation` erhalten Sie eine Reihe von touchereignissen Wechsel zwischen den zwei Fingern fest. Wie jedes Finger bewegt, relativ zu den anderen, Skalierung oder Drehung Ergebnissen, aber es wird durch den andere Finger Bewegung negiert, und das Ergebnis ist Übersetzung.

Der einzige verbleibende Teil der **berühren Manipulation** Seite ist die `PaintSurface` Ereignishandler in der `TouchManipulationPage` Code-Behind-Datei. Aufruft der `Paint` Methode der `TouchManipulationBitmap`, welche gilt die Matrix, die akkumulierten Touch-Aktivität darstellt:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

Die `PaintSurface` Handler endet, durch Anzeigen einer `MatrixDisplay` Objekt zeigt die kumulierte Touch Matrix:

[![](touch-images/touchmanipulation-small.png "Dreifacher Screenshot der Seite berühren Manipulation")](touch-images/touchmanipulation-large.png#lightbox "dreifacher Screenshot der Seite berühren Manipulation")

## <a name="manipulating-multiple-bitmaps"></a>Bearbeiten von mehreren Bitmaps

Einer der Vorteile der Isolation von Touch-die Verarbeitung von Code in Klassen wie z. B. `TouchManipulationBitmap` und `TouchManipulationManager` ist die Möglichkeit, diese Klassen in einem Programm wiederverwenden, die dem Benutzer ermöglicht, mehrere Bitmaps zu bearbeiten.

Die **Bitmap Punktdiagramm Ansicht** Seite veranschaulicht, wie dies funktioniert. Anstatt das Definieren eines Felds vom Typ `TouchManipulationBitmap`, die [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) Klasse definiert ein `List` Bitmap-Objekte:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Der Konstruktor in allen Bitmaps verfügbar als eingebettete Ressourcen lädt und hinzugefügt der `bitmapCollection`. Beachten Sie, dass die `Matrix` -Eigenschaft wird auf jedem initialisiert `TouchManipulationBitmap` Objekt, damit die linken oberen Ecken jede Bitmap um 100 Pixel versetzt werden.

Die `BitmapScatterView` Seite auch Berührungsereignisse für mehrere Bitmaps behandeln muss. Anstatt definieren eine `List` Fingereingabe IDs der derzeit bearbeitet `TouchManipulationBitmap` Objekte aufweist, dieses Programm erfordert ein Wörterbuch:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Beachten Sie, dass der `Pressed` Logik durchläuft die `bitmapCollection` in umgekehrter Reihenfolge. Die Bitmaps überlappen häufig einander. Die Bitmaps weiter unten in der Auflistung liegen visuell zusätzlich zu Bitmaps weiter oben in der Auflistung. Wenn es mehrere Bitmaps unter den Finger wieder an, der auf dem Bildschirm drückt sind, muss das oberste Element der, die von diesem Finger bearbeitet wird.

Beachten Sie auch, dass die `Pressed` Logik wird dieser Bitmap an das Ende der Auflistung verschoben, sodass visuell an den Anfang der eine Ansammlung von anderen Bitmaps verschoben.

In der `Moved` und `Released` Ereignisse, die `TouchAction` Ereignishandler ruft die `ProcessingTouchEvent` Methode in `TouchManipulationBitmap` ebenso wie die ältere Anwendung.

Schließlich die `PaintSurface` Ereignishandler ruft die `Paint` Methode jedes `TouchManipulationBitmap` Objekt:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Der Code durchläuft die Auflistung und zeigt die eine Ansammlung von Bitmaps vom Anfang der auflistungs am Ende an:

[![](touch-images/bitmapscatterview-small.png "Dreifacher Screenshot, der die Bitmap Punktdiagramm Ansichtsseite")](touch-images/bitmapscatterview-large.png#lightbox "dreifacher Screenshot, der die Bitmap Punktdiagramm Ansichtsseite")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Aufrufen von Ereignissen von Effekten](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
