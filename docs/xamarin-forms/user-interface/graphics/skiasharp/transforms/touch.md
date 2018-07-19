---
title: Manipulationen durch toucheingaben
description: In diesem Artikel wird erläutert, wie mit Matrixtransformationen Touch ziehen, berührpunkte und Drehung implementiert, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: 2de5b9a3a6bf0d36330212a52ba5c7278b970efc
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130906"
---
# <a name="touch-manipulations"></a>Manipulationen durch toucheingaben

_Verwenden von Matrixtransformationen zum Implementieren der Touch ziehen, berührpunkte und Drehung_

In Umgebungen wie z. B. die auf mobilen Geräten Multitouch verwenden Benutzer häufig die Finger zum Bearbeiten von Objekten auf dem Bildschirm. Allgemeine Gesten wie ein Ziehvorgang nur einem Finger und eine zwei-Finger-Prise können verschieben und skaliert Objekte oder sogar drehen können. Für diese Gesten werden in der Regel mithilfe von Transform-Matrizen implementiert, und in diesem Artikel erfahren Sie, wie das geht.

![](touch-images/touchmanipulationsexample.png "Eine Bitmap, bei denen sich Übersetzung, Skalierung und Drehung")

## <a name="manipulating-one-bitmap"></a>Bearbeiten eine Bitmap

Die **Touch-Bearbeitung** Touch-Manipulationen an einer einzelnen Bitmap veranschaulicht.
In diesem Beispiel verwendet die Nachverfolgen von Touch-Auswirkungen, die in diesem Artikel vorgestellten [Aufrufen von Ereignissen von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Mehrere andere Dateien bieten Unterstützung für die **Touch-Bearbeitung** Seite. Die erste ist die [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) Enumeration, der die verschiedenen Typen von Touch-Bearbeitung implementiert durch den Code, wird es, angibt:

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

`PanOnly` ist ein Ziehvorgang von nur einem Finger, die bei der Übersetzung implementiert wird. Alle nachfolgenden Optionen auch schwenken aber umfassen zwei Finger: `IsotropicScale` ist ein Pinch-Vorgang, der das Objekt, das Skalieren von Daten gleichmäßig in der horizontalen und vertikalen Richtungen ergibt. `AnisotropicScale` ermöglicht das Skalieren von ungleich.

Die `ScaleRotate` Option ist für zwei-Finger-Skalierung und Drehung. Skalierung ist kugelstrahler. Implementieren zwei-Finger-Rotation mit anisotrope Skalierung ist problematisch, da die Finger-Bewegungen im Wesentlichen identisch sind.

Die `ScaleDualRotate` Option wird nur einem Finger Drehung hinzugefügt. Wenn ein einzelner Finger das Objekt zieht, wird zuerst das gezogene Objekt um seinen Mittelpunkt gedreht, sodass der Mittelpunkt des Objekts mit dem Ziehen Initialisierungsvektor.

Die [ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) -Datei enthält eine `Picker` mit den Elementen der `TouchManipulationMode` Enumeration:

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

Unten ist ein `SKCanvasView` und ein `TouchEffect` angefügt, die in einer Zelle befindliches `Grid` , das es einschließt.

Die [ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) Code-Behind-Datei hat eine `bitmap` Feld, aber es ist nicht vom Typ `SKBitmap`. Der Typ ist `TouchManipulationBitmap` (eine Klasse, die Sie werden sehen gleich):

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
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Der Konstruktor instanziiert ein `TouchManipulationBitmap` Objekts auf und übergibt an den Konstruktor einer `SKBitmap` aus einer eingebetteten Ressource abgerufen. Der Konstruktor wird abgeschlossen, indem Sie festlegen der `Mode` Eigenschaft der `TouchManager` Eigenschaft der `TouchManipulationBitmap` Objekt, das ein Mitglied der `TouchManipulationMode` Enumeration.

Die `SelectedIndexChanged` Handler für die `Picker` setzt auch dies `Mode` Eigenschaft:

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

Die `TouchAction` Handler, der die `TouchEffect` instanziiert in der XAML-Datei ruft zwei Methoden in `TouchManipulationBitmap` mit dem Namen `HitTest` und `ProcessTouchEvent`:

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

Wenn die `HitTest` Methodenrückgabe `true` &mdash; bedeutet, dass ein Finger den Bildschirm innerhalb des Bereichs, der von der Bitmap berührt hat &mdash; und klicken Sie dann die Touch-ID hinzugefügt wird die `TouchIds` Auflistung. Diese ID stellt die Reihenfolge der Berührungsereignisse für Fingers dar, bis der Finger vom Bildschirm hebt. Wenn mehrere Finger Tippen Sie auf die Bitmap, die `touchIds` Auflistung enthält eine Touch-ID für jeden Finger.

Die `TouchAction` Handler ruft auch die `ProcessTouchEvent` -Klasse im `TouchManipulationBitmap`. Hier kommt einige (aber nicht alle) an die echten Multitouch-Verarbeitung.

Die [ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) -Klasse ist eine Wrapperklasse für `SKBitmap` , die Code für die Bitmap Rendern und Verarbeiten von touchereignissen enthält. Es funktioniert in Verbindung mit mehr generalisierten Code in einem `TouchManipulationManager` -Klasse (die Sie gleich sehen werden).

Die `TouchManipulationBitmap` Konstruktor speichert die `SKBitmap` und zwei Eigenschaften, instanziiert die `TouchManager` Eigenschaft vom Typ `TouchManipulationManager` und `Matrix` Eigenschaft vom Typ `SKMatrix`:

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

Dies `Matrix` -Eigenschaft ist die kumulierte Transformation, die durch die Touch-Aktivität. Sie sehen, wird jedes touchereignis aufgelöst, in einer Matrix, die dann verkettet wird, mit der `SKMatrix` von gespeicherten Wert der `Matrix` Eigenschaft.

Die `TouchManipulationBitmap` Objekt zeichnet sich selbst in die `Paint` Methode. Das Argument ist ein `SKCanvas` Objekt. Dies `SKCanvas` möglicherweise bereits eine Transformation angewendet wird, sodass der `Paint` Methode verkettet die `Matrix` Eigenschaft der Bitmap für die vorhandene Transformation zugeordnet, und im Zeichenbereich wird wiederhergestellt, wenn er abgeschlossen wurde:

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

Die `HitTest` Methodenrückgabe `true` , wenn der Benutzer den Bildschirm zu einem Zeitpunkt innerhalb der Grenzen der Bitmap berührt. Wie der Benutzer die Bitmap bearbeitet, wird die Bitmap gedreht werden kann, oder sogar (über eine Kombination aus anisotrope skalierungs- und drehungsvorgängen) werden in der Form eines Parallelogramms. Können Sie Angst haben, die die `HitTest` -Methode muss in diesem Fall recht komplexen analytische Geometrie zu implementieren.

Es ist jedoch eine Verknüpfung zur Verfügung:

Bestimmen, ob ein Punkt innerhalb der Grenzen eines transformierten Rechtecks liegt ist identisch mit bestimmen, ob ein umgekehrter transformierte Punkt innerhalb der Grenzen des Rechtecks untransformierten liegt. Das ist eine viel einfachere Berechnung aus, und sie können die komfortable `Contains` definierte Methode `SKRect`:

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

Die zweite öffentliche Methode in `TouchManipulationBitmap` ist `ProcessTouchEvent`. Wenn diese Methode aufgerufen wird, wurde es bereits eingerichtet, dass das touchereignis in dieser bestimmten Bitmap gehört. Die Methode verwaltet ein Dictionary [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) -Objekte, die einfach die vorherigen Punkt und dem neuen von jedem Finger ist:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Hier ist das Wörterbuch und die `ProcessTouchEvent` Methode selbst:

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

In der `Moved` und `Released` Ereignisse, die Methodenaufrufe `Manipulate`. Derzeit wird die `touchDictionary` enthält ein oder mehrere `TouchManipulationInfo` Objekte. Wenn die `touchDictionary` enthält mindestens ein Element, ist es wahrscheinlich, die die `PreviousPoint` und `NewPoint` Werte ungleich sind, und stellen die Bewegung des Fingers. Wenn mehrere Finger berühren sich die Bitmap, das Wörterbuch mehr als ein Element enthält, aber nur eines dieser Elemente andere gelten `PreviousPoint` und `NewPoint` Werte. Die restlichen haben gleich `PreviousPoint` und `NewPoint` Werte.

Dies ist wichtig: die `Manipulate` Methode kann davon ausgehen, dass die Verschiebung von nur einem Finger verarbeitet wird. Zum Zeitpunkt des Aufrufs keine anderen Finger verschieben sind, und wenn sie wirklich (wie wahrscheinlich ist) verschieben, werden dieser Verschiebungen in zukünftigen Aufrufen von verarbeitet `Manipulate`.

Die `Manipulate` Methode kopiert zunächst das Wörterbuch in ein Array der Einfachheit halber. Etwas anderes als die ersten beiden Einträge werden ignoriert. Wenn mehr als zwei Finger versuchen, die Bearbeitung der Bitmap, werden die anderen ignoriert. `Manipulate` ist das letzte Mitglied `TouchManipulationBitmap`:

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

Wenn ein Finger Bitmap bearbeitet `Manipulate` Aufrufe der `OneFingerManipulate` Methode der `TouchManipulationManager` Objekt. Für zwei Finger ruft `TwoFingerManipulate`. Die Argumente für diese Methoden sind identisch: die `prevPoint` und `newPoint` Argumente darstellen, den Finger, die verschoben werden. Aber die `pivotPoint` Argument ist für die beiden Aufrufe unterschiedlich:

Für die Bearbeitung von nur einem Finger den `pivotPoint` steht im Mittelpunkt der Bitmap. Dies ist auf nur einem Finger Rotation zu ermöglichen. Für die Bearbeitung von zwei-Finger gibt das Ereignis an das Verschieben von nur einem Finger, sodass die `pivotPoint` ist der Finger, die nicht verschoben wird.

In beiden Fällen `TouchManipulationManager` gibt ein `SKMatrix` -Wert, der die Methode mit dem aktuellen verkettet `Matrix` Eigenschaft, die `TouchManipulationPage` verwendet, um die Bitmap zu rendern.

`TouchManipulationManager` generalisiert werden, und verwendet keine anderen Dateien mit Ausnahme von `TouchManipulationMode`. Sie können diese Klasse ohne Änderungen in Ihren eigenen Anwendungen verwenden können. Definiert eine einzelne Eigenschaft vom Typ `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Aber Sie sollten vermeiden Sie die `AnisotropicScale` Option. Es ist sehr einfach, mit diese Option, um die Bitmap zu bearbeiten, sodass eine Skalierung Faktoren auf 0 (null) ist. Dadurch wird die Bitmap, die nicht mehr in der Sicht, nicht mehr angezeigt. Wenn Sie die anisotrope Skalierung wirklich benötigen, sollten Sie zur Verbesserung der der Logik, um unerwünschte Ergebnisse zu vermeiden.

`TouchManipulationManager` verwendet von Vektoren, aber da gibt es keine `SKVector` Struktur in SkiaSharp, `SKPoint` wird stattdessen verwendet. `SKPoint` unterstützt, die der Subtraktionsoperator, und das Ergebnis als Vektor behandelt werden kann. Der Vektor-spezifische-Programmlogik, die hinzugefügt werden müssen, ist eine `Magnitude` Berechnung:

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

Wenn Drehung ausgewählt wurde, behandeln Sie zunächst beide die Manipulation von nur einem Finger und zwei-Finger-Methoden die Drehung. Wenn Drehung erkannt wird, wird dann effektiv die drehungskomponente entfernt werden. Was übrig bleibt, wird als Schwenken und Skalierung interpretiert.

Hier ist die `OneFingerManipulate` Methode. Wenn die Drehung mit nur einem Finger nicht aktiviert wurde, und klicken Sie dann die Logik ist einfach &mdash; sie einfach die vorherigen Punkt und den neuen Punkt zum Erstellen eines Vektors mit dem Namen verwendet `delta` , die genau auf die Übersetzung entspricht. Mit einer Drehung von nur einem Finger aktiviert verwendet die Methode Winkel aus der Dreh-und Angelpunkt (in der Mitte der Bitmap) auf den vorherigen Punkt und den neuen Punkt um eine Rotationsmatrix zu erstellen.

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

In der `TwoFingerManipulate` -Methode der Dreh-und Angelpunkt ist die Position des Fingers, die in diesem bestimmten Touch-Ereignis nicht bewegt wird. Die Drehung ist die Rotation von nur einem Finger sehr ähnlich, und klicken Sie dann für der Vektor mit dem Namen `oldVector` (basierend auf vorherigen Punkt), die für die Rotation angepasst wird. Die verbleibende Bewegung wird als die Skalierung interpretiert:

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

Sie werden feststellen, dass keine explizite Übersetzung in dieser Methode vorhanden ist. Jedoch sowohl die `MakeRotation` und `MakeScale` Methoden basieren auf der Dreh-und Angelpunkt sowie implizite Übersetzung enthält. Wenn Sie zwei Finger auf die Bitmap und ziehen sie in der gleichen Richtung verwenden `TouchManipulation` erhalten Sie eine Reihe von touchereignissen, die abwechselnd zwischen den beiden Fingern. Wie jedes fingerbewegung relativ zu anderen, Skalierung oder Drehung Ergebnissen, jedoch wird es von der andere Finger verschieben negiert, und das Ergebnis ist die Übersetzung.

Der einzige verbleibende Teil der **Touch-Bearbeitung** Seite ist die `PaintSurface` Ereignishandler in der `TouchManipulationPage` Code-Behind-Datei. Dadurch wird die `Paint` -Methode der der `TouchManipulationBitmap`, die gilt, dass die Transformationsmatrix zur Darstellung der kumulierten Touch-Aktivität:

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

Die `PaintSurface` Handler schließt mit der Anzeige einer `MatrixDisplay` Objekt zeigt die kumulierte Touch-Matrix:

[![](touch-images/touchmanipulation-small.png "Dreifacher Screenshot der Seite für Touch-Bearbeitung")](touch-images/touchmanipulation-large.png#lightbox "dreifachen Screenshot der Seite für Touch-Bearbeitung")

## <a name="manipulating-multiple-bitmaps"></a>Bearbeiten mehrerer Bitmaps

Einer der Vorteile der Isolation von Touch-die Verarbeitung von Code in Klassen wie z. B. `TouchManipulationBitmap` und `TouchManipulationManager` ist die Möglichkeit, diese Klassen in einem Programm wieder verwenden, die dem Benutzer ermöglicht, mehrere Bitmaps zu bearbeiten.

Die **Bitmap Punktdiagramm Ansicht** Seite erläutert, wie dies vonstatten geht. Anstatt das Definieren eines Felds vom Typ `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) -Klasse definiert eine `List` der Bitmap-Objekte:

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
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
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

Der Konstruktor in allen Bitmaps verfügbar als eingebettete Ressourcen lädt und hinzugefügt der `bitmapCollection`. Beachten Sie, dass die `Matrix` -Eigenschaft wird auf jedem initialisiert `TouchManipulationBitmap` Objekt, sodass jede Bitmap die linke obere Ecken von 100 Pixel ausgeglichen werden.

Die `BitmapScatterView` Seite muss außerdem touchereignisse für mehrere Bitmaps zu behandeln. Anstatt definieren eine `List` an Multitouch-IDs der aktuell bearbeitet `TouchManipulationBitmap` Objekten, die dieses Programm erfordert ein Wörterbuch:

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

Beachten Sie, dass die `Pressed` Logik durchläuft die `bitmapCollection` in umgekehrter Reihenfolge. Die Bitmaps überlappen häufig einander. Die Bitmaps weiter unten in der Auflistung liegen visuell über die Bitmaps weiter oben in der Auflistung. Treten mehrere Bitmaps unter dem Finger, der auf dem Bildschirm klickt, muss der obersten der sein, die von den Fingers bearbeitet wird.

Beachten Sie auch, dass die `Pressed` Logik wird diese Bitmap an das Ende der Auflistung verschoben, sodass visuell am Anfang eine Ansammlung von anderen Bitmaps verschoben wird.

In der `Moved` und `Released` Ereignisse, die `TouchAction` Ereignishandler ruft die `ProcessingTouchEvent` -Methode in der `TouchManipulationBitmap` ebenso wie das Programm weiter oben.

Zum Schluss die `PaintSurface` Ereignishandler ruft die `Paint` Methode `TouchManipulationBitmap` Objekt:

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

Der Code durchläuft die Auflistung und zeigt eine Ansammlung von Bitmaps vom Anfang der Auflistung am Ende:

[![](touch-images/bitmapscatterview-small.png "Dreifacher Screenshot der Seite für Bitmap Punktdiagramm Ansicht")](touch-images/bitmapscatterview-large.png#lightbox "dreifachen Screenshot der Seite für Bitmap Punktdiagramm anzeigen")

## <a name="single-finger-scaling"></a>Skalierung von einem Finger

Eine Skalierung erfordert im Allgemeinen eine zusammendrückbewegung mit zwei Fingern. Allerdings ist es möglich, mit einem einzelnen Finger skalieren, indem Sie den Finger verschieben Sie die Ecken einer Bitmap zu implementieren.

Dies wird veranschaulicht, der **Finger Ecke Skala** Seite. Da in diesem Beispiel eine etwas andere Art der Skalierung verwendet wird, dass, die es in implementiert die `TouchManipulationManager` -Klasse, wird diese Klasse nicht verwendet oder die `TouchManipulationBitmap` Klasse. Stattdessen wird die gesamte Touch Logik in der CodeBehind-Datei. Dies ist etwas einfacher Logik als üblich, weil es nur mit einem Finger gleichzeitig verfolgt und ignoriert einfach alle sekundären Finger den Bildschirm berührt werden können.

Die [ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) Seite instanziiert die `SKCanvasView` -Klasse und erstellt eine `TouchEffect` Objekt zum Nachverfolgen von Touch-Ereignissen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Die [ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) Datei lädt eine Bitmapressource aus der **Media** Verzeichnis und zeigt sie mithilfe einer `SKMatrix` -Objekt als definiert eine Feld:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

Dies `SKMatrix` Objekt von der Touch-Logik, die unten gezeigten geändert wird.

Der Rest des Code-Behind-Datei ist die `TouchEffect` -Ereignishandler. Es beginnt mit der die aktuelle Position des Fingers zum Konvertieren einer `SKPoint` Wert. Für die `Pressed` Aktionstyp, der Handler überprüft, dass keine anderen Finger den Bildschirm berührt und dass der Finger innerhalb der Grenzen der Bitmap.

Der entscheidende Teil des Codes ist ein `if` -Anweisung, die beiden Aufrufe von der `Math.Pow` Methode. Diese mathematischen überprüft, ob der Finger Speicherort außerhalb einer Ellipse, die die Bitmap ausfüllt. Wenn dies der Fall ist, handelt es sich um eine Skalierung ausgeführt. Der Finger in der Nähe von einer der Ecken der Bitmap ist, und eine Art Drehmoment wird bestimmt, dass der gegenüberliegenden Ecke ist. Wenn der Finger innerhalb dieser Ellipse ist, ist es ein regulärer schwenkansicht Vorgang:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

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
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

Die `Moved` Aktionstyp berechnet eine Matrix, die für die Touch-Aktivität, ab dem Zeitpunkt der Finger-Bildschirms bis zu diesem Zeitpunkt gedrückt. Diese Matrix mit der Matrix werden wirksam zum Zeitpunkt der Finger Bitmap gedrückt verkettet. Skalierungsvorgangs bezieht sich immer auf die Ecke Gegensatz zu, die der Finger berührt.

Für kleine oder längliche Bitmaps kann eine innere Ellipse belegen die meisten der Bitmap und sehr kleine Bereiche an den Ecken die Bitmap skalieren lassen. Möglicherweise bevorzugen Sie einen etwas anderen Ansatz, in diesem Fall können Sie diese gesamte `if` Block, der festlegt `isScaling` zu `true` durch den folgenden Code:

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Dieser Code teilt effektiv den Bereich der Bitmap in einem inneren Raute und vier Dreiecke, an den Ecken. Dies ermöglicht eine viel größere Bereiche an den Ecken ziehen, und Skalieren die Bitmap.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Aufrufen von Ereignissen von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
