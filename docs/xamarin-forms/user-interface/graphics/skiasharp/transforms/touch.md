---
Title: "berühren von Bearbeitungen": "in diesem Artikel wird erläutert, wie Sie mithilfe von Matrix Transformationen Drag & amp; quot; Drag & amp; quot; Drag & amp; quot; Drag & Drop
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650 Author: davidbritch ms. Author: dabritch ms. Date: 09/14/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="touch-manipulations"></a>Manipulationen durch Toucheingaben

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden von Matrix Transformationen zum Implementieren von Drag & Drop, Pinching und Drehung von Finger Eingaben_

In Multitouch-Umgebungen, wie z. b. auf mobilen Geräten, verwenden Benutzer häufig ihre Finger zum Bearbeiten von Objekten auf dem Bildschirm. Gängige Gesten, wie z. b. ein Drag & Drop mit einem Finger, können Objekte verschieben und skalieren oder sogar drehen. Diese Gesten werden im Allgemeinen mithilfe von Transformations Matrizen implementiert, und in diesem Artikel wird erläutert, wie Sie das tun.

![](touch-images/touchmanipulationsexample.png "A bitmap subjected to translation, scaling, and rotation")

Alle hier gezeigten Beispiele verwenden den Xamarin.Forms in dem Artikel [**Aufrufen von Ereignissen aus Effekten**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)dargestellten Fingerabdruck Effekt.

## <a name="dragging-and-translation"></a>Ziehen und übersetzen

Eine der wichtigsten Anwendungen von Matrix Transformationen ist die Berührungs Verarbeitung. Mit einem einzelnen [`SKMatrix`](xref:SkiaSharp.SKMatrix) Wert kann eine Reihe von Berührungs Vorgängen konsolidiert werden. 

Beim Ziehen mit einem Finger wird der `SKMatrix` Wert übersetzt. Dies wird auf der Seite zum **ziehen der Bitmap** veranschaulicht. Die XAML-Datei instanziiert eine `SKCanvasView` in einer Xamarin.Forms `Grid` . Ein- `TouchEffect` Objekt wurde der-Auflistung `Effects` von hinzugefügt `Grid` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.BitmapDraggingPage"
             Title="Bitmap Dragging">
    
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Theoretisch `TouchEffect` kann das Objekt direkt der-Auflistung von hinzugefügt werden `Effects` `SKCanvasView` , aber dies funktioniert nicht auf allen Plattformen. Da die `SKCanvasView` gleiche Größe wie `Grid` in dieser Konfiguration hat, funktioniert das Anfügen an die `Grid` ebenfalls genauso.

Die Code-Behind-Datei lädt in den Konstruktor einer Bitmap-Ressource und zeigt Sie im- `PaintSurface` Handler an:

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    // Bitmap and matrix for display
    SKBitmap bitmap;
    SKMatrix matrix = SKMatrix.MakeIdentity();
    ···

    public BitmapDraggingPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, new SKPoint());
    }
}
```

Ohne weiteren Code `SKMatrix` ist der Wert immer die identifistruktur, und er hat keine Auswirkung auf die Anzeige der Bitmap. Das Ziel des `OnTouchEffectAction` in der XAML-Datei festgelegten Handlers besteht darin, den Matrix Wert so zu ändern, dass er Berührungs Manipulationen widerspiegelt.

Der `OnTouchEffectAction` Handler beginnt damit, den Xamarin.Forms `Point` Wert in einen skiasharp- `SKPoint` Wert umzuwandeln. Dies ist eine einfache Skalierung auf der Grundlage der `Width` -Eigenschaft und der- `Height` `SKCanvasView` Eigenschaft von (die geräteunabhängigen Einheiten sind) und der- `CanvasSize` Eigenschaft, die in Pixel Einheiten liegt:

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    ···
    // Touch information
    long touchId = -1;
    SKPoint previousPoint;
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
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point))
                {
                    touchId = args.Id;
                    previousPoint = point;
                }
                break;

            case TouchActionType.Moved:
                if (touchId == args.Id)
                {
                    // Adjust the matrix for the new position
                    matrix.TransX += point.X - previousPoint.X;
                    matrix.TransY += point.Y - previousPoint.Y;
                    previousPoint = point;
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = -1;
                break;
        }
    }
    ···
}
```

Wenn ein Finger den Bildschirm zum ersten Mal berührt, wird ein Ereignis vom Typ ausgelöst `TouchActionType.Pressed` . Die erste Aufgabe besteht darin zu bestimmen, ob der Finger die Bitmap berührt. Eine solche Aufgabe wird häufig als _Treffer Test_bezeichnet. In diesem Fall können Sie Treffer Tests durchgeführt werden, indem `SKRect` Sie einen Wert erstellen, der der Bitmap entspricht, indem Sie die Matrix Transformation mit anwenden `MapRect` und dann ermitteln, ob sich der Berührungspunkt innerhalb des transformierten Rechtecks befindet.

Wenn dies der Fall ist, wird das `touchId` Feld auf die Touch-ID festgelegt, und die Fingerposition wird gespeichert.

Für das- `TouchActionType.Moved` Ereignis werden die Übersetzungs Faktoren des `SKMatrix` Werts basierend auf der aktuellen Position des Fingers und der neuen Position des Fingers angepasst. Diese neue Position wird zum nächsten Mal gespeichert, und der `SKCanvasView` wird ungültig.

Wenn Sie mit diesem Programm experimentieren, beachten Sie, dass Sie nur die Bitmap ziehen können, wenn der Finger einen Bereich berührt, in dem die Bitmap angezeigt wird. Obwohl diese Einschränkung für dieses Programm nicht sehr wichtig ist, wird Sie bei der Bearbeitung mehrerer Bitmaps entscheidend.

## <a name="pinching-and-scaling"></a>Pinching und Skalierung

Was möchten Sie tun, wenn die Bitmap durch zwei Finger berührt wird? Wenn sich die beiden Finger parallel bewegen, soll die Bitmap wahrscheinlich mit den Fingern bewegt werden. Wenn die beiden Finger einen Pinsel-oder streckungs Vorgang ausführen, möchten Sie möglicherweise, dass die Bitmap gedreht (im nächsten Abschnitt erläutert) oder skaliert wird. Wenn eine Bitmap skaliert wird, ist es am sinnvollsten, wenn sich die beiden Finger an denselben Positionen in Bezug auf die Bitmap befinden und die Bitmap entsprechend skaliert werden soll.

Die Verarbeitung von zwei Fingern auf einmal scheint kompliziert zu sein, aber beachten Sie, dass der `TouchAction` Handler nur Informationen zu einem Finger gleichzeitig empfängt. Wenn die Bitmap von zwei Fingern bearbeitet wird, hat sich für jedes Ereignis ein Finger geändert, aber der andere hat sich nicht geändert. Im unten stehenden Code der **Bitmapskalierung** wird der Finger, der nicht geändert wurde, als _Pivotpunkt bezeichnet_ , da die Transformation relativ zu diesem Punkt ist.

Ein Unterschied zwischen diesem Programm und dem vorherigen Programm besteht darin, dass mehrere Berührungs-IDs gespeichert werden müssen. Zu diesem Zweck wird ein Wörterbuch verwendet, bei dem die Touch-ID der Wörterbuch Schlüssel und der Wörterbuch Wert die aktuelle Position dieses fingers ist:

```csharp
public partial class BitmapScalingPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
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
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point) && !touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Add(args.Id, point);
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger scale and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index of non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points involved in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Scaling factors are ratios of those
                        float scaleX = newVector.X / oldVector.X;
                        float scaleY = newVector.Y / oldVector.Y;

                        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
                            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
                        {
                            // If something bad hasn't happened, calculate a scale and translation matrix
                            SKMatrix scaleMatrix = 
                                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);

                            SKMatrix.PostConcat(ref matrix, scaleMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }
    ···
}
```

Die Behandlung der `Pressed` Aktion ist fast identisch mit dem vorherigen Programm, mit dem Unterschied, dass die ID und der Berührungspunkt dem Wörterbuch hinzugefügt werden. Die `Released` `Cancelled` Aktionen und entfernen den Wörterbucheintrag.

Die Behandlung für die `Moved` Aktion ist jedoch komplexer. Wenn nur ein Finger beteiligt ist, ist die Verarbeitung sehr ähnlich wie das vorherige Programm. Bei zwei oder mehr Fingern muss das Programm auch Informationen aus dem Wörterbuch abrufen, die den nicht verschiebenden Finger betreffen. Dies geschieht, indem die Wörterbuch Schlüssel in ein Array kopiert und anschließend der erste Schlüssel mit der ID des Fingers verglichen wird, der verschoben wird. Dadurch kann das Programm den Pivotpunkt abrufen, der dem nicht verschiebenden Finger entspricht.

Anschließend berechnet das Programm zwei Vektoren der neuen Fingerposition relativ zum Pivotpunkt und die alte Fingerposition relativ zum Pivotpunkt. Die Verhältnisse dieser Vektoren sind Skalierungsfaktoren. Da die Division durch 0 (null) eine Möglichkeit ist, müssen diese auf unbegrenzte Werte oder NaN-Werte (keine Zahl) geprüft werden. Wenn alles richtig ist, wird eine Skalierungs Transformation mit dem Wert verkettet, der `SKMatrix` als Feld gespeichert ist.

Wenn Sie auf dieser Seite experimentieren, werden Sie bemerken, dass Sie die Bitmap mit einem oder zwei Fingern ziehen oder mit zwei Fingern skalieren können. Die Skalierung ist _anisotrope_, was bedeutet, dass sich die Skalierung in horizontaler und vertikaler Richtung unterscheiden kann. Dadurch wird das Seitenverhältnis verzerrt, aber Sie können die Bitmap auch zum Erstellen eines Spiegel Bilds kippen. Möglicherweise stellen Sie auch fest, dass Sie die Bitmap auf eine Dimension mit 0 (null) verkleinern können, und Sie verschwindet. Im Produktionscode sollten Sie sich davor schützen.

## <a name="two-finger-rotation"></a>Zwei-Finger-Drehung

Auf der Seite **Bitmap** -Rotation können Sie zwei Finger für die Drehung oder die isotrope Skalierung verwenden. Die Bitmap behält immer das richtige Seitenverhältnis bei. Die Verwendung von zwei Fingern sowohl für die Drehung als auch für die anisotrope Skalierung funktioniert nicht sehr gut, da die Bewegung der Finger für beide Aufgaben sehr ähnlich ist.

Der erste große Unterschied in diesem Programm ist die Logik für den Treffer Test. Die vorherigen Programme verwendeten die- `Contains` Methode von `SKRect` , um zu bestimmen, ob sich der Berührungspunkt innerhalb des transformierten Rechtecks befindet, das der Bitmap entspricht. Beim Manipulieren der Bitmap wird die Bitmap jedoch möglicherweise gedreht und `SKRect` kann kein gedrehtes Rechteck ordnungsgemäß darstellen. Sie können befürchten, dass die Logik für die Treffer Tests in diesem Fall eine recht komplexe analytische Geometrie implementieren muss.

Es ist jedoch eine Verknüpfung verfügbar: bestimmen, ob ein Punkt innerhalb der Grenzen eines transformierten Rechtecks liegt, entspricht dem bestimmen, ob ein umgekehrter transformier ter Punkt innerhalb der Grenzen des nicht transformierten Rechtecks liegt. Dies ist eine wesentlich einfachere Berechnung, und die Logik kann die bequeme Methode weiterhin verwenden `Contains` :

```csharp
public partial class BitmapRotationPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
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
                if (!touchDictionary.ContainsKey(args.Id))
                {
                    // Invert the matrix
                    if (matrix.TryInvert(out SKMatrix inverseMatrix))
                    {
                        // Transform the point using the inverted matrix
                        SKPoint transformedPoint = inverseMatrix.MapPoint(point);

                        // Check if it's in the untransformed bitmap rectangle
                        SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);

                        if (rect.Contains(transformedPoint))
                        {
                            touchDictionary.Add(args.Id, point);
                        }
                    }
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger rotate, scale, and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Find angles from pivot point to touch points
                        float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                        float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                        // Calculate rotation matrix
                        float angle = newAngle - oldAngle;
                        SKMatrix touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                        // Effectively rotate the old vector
                        float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                        oldVector.X = magnitudeRatio * newVector.X;
                        oldVector.Y = magnitudeRatio * newVector.Y;

                        // Isotropic scaling!
                        float scale = Magnitude(newVector) / Magnitude(oldVector);

                        if (!float.IsNaN(scale) && !float.IsInfinity(scale))
                        {
                            SKMatrix.PostConcat(ref touchMatrix,
                                SKMatrix.MakeScale(scale, scale, pivotPoint.X, pivotPoint.Y));

                            SKMatrix.PostConcat(ref matrix, touchMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }

    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
    ···
}
```

Die Logik für das `Moved` Ereignis beginnt wie das vorherige Programm. Zwei Vektoren mit dem Namen `oldVector` und `newVector` werden basierend auf dem vorherigen und dem aktuellen Punkt des verschiebenden fingers und dem Pivotpunkt des nicht verschiebenden fingers berechnet. Anschließend werden Winkel dieser Vektoren festgelegt, und der Unterschied ist der Drehwinkel.

Die Skalierung kann auch einbezogen werden, sodass der alte Vektor auf der Grundlage des Drehwinkels gedreht wird. Die relative Größe der beiden Vektoren ist nun der Skalierungsfaktor. Beachten Sie, dass der gleiche `scale` Wert für die horizontale und vertikale Skalierung verwendet wird, sodass die Skalierung isotrope ist. Das `matrix` Feld wird durch die Rotations Matrix und eine Skalierungs Matrix angepasst.

Wenn Ihre Anwendung die Berührungs Verarbeitung für eine einzelne Bitmap (oder ein anderes Objekt) implementieren muss, können Sie den Code aus diesen drei Beispielen für Ihre eigene Anwendung anpassen. Wenn Sie jedoch die Berührungs Verarbeitung für mehrere Bitmaps implementieren müssen, sollten Sie diese Berührungs Vorgänge wahrscheinlich in anderen Klassen kapseln.

## <a name="encapsulating-the-touch-operations"></a>Kapseln der Berührungs Vorgänge

Die Seite für die **Berührungs Bearbeitung** veranschaulicht die Berührungs Bearbeitung einer einzelnen Bitmap, aber die Verwendung mehrerer anderer Dateien, die einen Großteil der oben gezeigten Logik Kapseln. Die erste dieser Dateien ist die- [`TouchManipulationMode`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) Enumeration, die die verschiedenen Typen von Berührungs Manipulationen angibt, die vom Code implementiert werden, den Sie sehen:

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

`PanOnly`ist ein Zieh Punkt mit einem Finger, der mit Translation implementiert wird. Alle nachfolgenden Optionen umfassen auch das Schwenken, beinhalten jedoch zwei Finger: `IsotropicScale` ist ein Vorgang, bei dem das Objekt in horizontaler und vertikaler Richtung gleichmäßig skaliert wird. `AnisotropicScale`ermöglicht eine ungleiche Skalierung.

Die `ScaleRotate` Option ist die zwei-Finger-Skalierung und-Rotation. Die Skalierung ist isotrope. Wie bereits erwähnt, ist die Implementierung der zwei-Finger-Drehung mit der anisotrope Skalierung problematisch, da die Fingerbewegungen im Wesentlichen identisch sind.

Mit der `ScaleDualRotate` -Option wird die Drehung mit einem Finger eingefügt. Wenn ein einzelner Finger das Objekt zieht, wird das gezogene Objekt zuerst um seine Mitte gedreht, sodass sich der Mittelpunkt des Objekts mit dem Zieh Vektor dreht.

Die Datei " [**touchmanipulationpage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) " enthält ein-Element mit den Membern der- `Picker` `TouchManipulationMode` Enumeration:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos.Transforms"
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:TouchManipulationMode}">
                    <x:Static Member="local:TouchManipulationMode.None" />
                    <x:Static Member="local:TouchManipulationMode.PanOnly" />
                    <x:Static Member="local:TouchManipulationMode.IsotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.AnisotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.ScaleRotate" />
                    <x:Static Member="local:TouchManipulationMode.ScaleDualRotate" />
                </x:Array>
            </Picker.ItemsSource>
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

Im unteren Bereich befindet sich eine `SKCanvasView` und eine `TouchEffect` , die an die einzelle angefügt ist `Grid` , die Sie einschließt.

Die [**TouchManipulationPage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) -Code Behind-Datei verfügt über ein `bitmap` Feld, ist aber nicht vom Typ `SKBitmap` . Der Typ ist `TouchManipulationBitmap` (eine Klasse, die Sie in Kürze sehen werden):

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

Der-Konstruktor instanziiert ein- `TouchManipulationBitmap` Objekt, wobei an den Konstruktor übergeben wird, der `SKBitmap` von einer eingebetteten Ressource abgerufen wurde. Der Konstruktor wird beendet, indem die-Eigenschaft der- `Mode` `TouchManager` Eigenschaft des- `TouchManipulationBitmap` Objekts auf einen Member der- `TouchManipulationMode` Enumeration festgelegt wird.

Der- `SelectedIndexChanged` Handler für `Picker` legt auch diese `Mode` Eigenschaft fest:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            bitmap.TouchManager.Mode = (TouchManipulationMode)picker.SelectedItem;
        }
    }
    ...
}
```

Der `TouchAction` Handler von, der `TouchEffect` in der XAML-Datei instanziiert wird, ruft zwei Methoden in mit dem `TouchManipulationBitmap` Namen `HitTest` und auf `ProcessTouchEvent` :

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

Wenn die `HitTest` Methode zurückgibt `true` &mdash; , dass ein Finger den Bildschirm innerhalb des Bereichs, der von der Bitmap besetzt ist, berührt hat, &mdash; wird der Auflistung die Touch-ID hinzugefügt `TouchIds` . Diese ID stellt die Abfolge von Touch-Ereignissen für diesen Finger dar, bis der Finger auf dem Bildschirm angezeigt wird. Wenn die Bitmap mehrere Finger berühren, enthält die Auflistung `touchIds` eine Touch-ID für jeden Finger.

Der `TouchAction` Handler ruft auch die- `ProcessTouchEvent` Klasse in auf `TouchManipulationBitmap` . An dieser Stelle treten einige (aber nicht alle) der echten Berührungs Verarbeitung auf.

Die- [`TouchManipulationBitmap`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Klasse ist eine Wrapper Klasse für `SKBitmap` , die Code zum Rendering der Bitmap-und Process-Berührungs Ereignisse enthält. Dies funktioniert in Verbindung mit mehr verallgemeinertem Code in einer `TouchManipulationManager` Klasse (die Sie in Kürze sehen werden).

Der `TouchManipulationBitmap` Konstruktor speichert den `SKBitmap` und instanziiert zwei Eigenschaften, die `TouchManager` -Eigenschaft vom Typ `TouchManipulationManager` und die- `Matrix` Eigenschaft des Typs `SKMatrix` :

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

Diese `Matrix` Eigenschaft ist die akkumulierte Transformation, die sich aus allen Berührungs Aktivitäten ergibt. Wie Sie sehen werden, wird jedes Berührungs Ereignis in eine Matrix aufgelöst, die dann mit dem Wert verkettet wird, der `SKMatrix` von der-Eigenschaft gespeichert wird `Matrix` .

Das- `TouchManipulationBitmap` Objekt zeichnet sich selbst in seiner- `Paint` Methode auf. Das-Argument ist ein- `SKCanvas` Objekt. `SKCanvas`Auf diese Weise wird möglicherweise bereits eine Transformation angewendet, sodass die- `Paint` Methode die der `Matrix` Bitmap zugeordnete-Eigenschaft der vorhandenen Transformation verkettet und den Zeichenbereich wiederherstellt, wenn der Vorgang abgeschlossen ist:

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

Die `HitTest` Methode gibt zurück, `true` Wenn der Benutzer den Bildschirm an einem Punkt innerhalb der Grenzen der Bitmap berührt. Dies verwendet die zuvor auf der Seite **Bitmap-Rotation** gezeigte Logik:

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

Die zweite öffentliche Methode in `TouchManipulationBitmap` ist `ProcessTouchEvent` . Wenn diese Methode aufgerufen wird, wurde bereits festgelegt, dass das Berührungs Ereignis zu dieser speziellen Bitmap gehört. Die-Methode verwaltet ein Wörterbuch von-Objekten, d. h. [`TouchManipulationInfo`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) den vorherigen Punkt und den neuen Punkt jedes Fingers:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Hier sehen Sie das Wörterbuch und die `ProcessTouchEvent` Methode selbst:

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

Im `Moved` -Ereignis und im- `Released` Ereignis ruft die-Methode auf `Manipulate` . Zu diesen Zeitpunkten `touchDictionary` enthält ein oder mehrere- `TouchManipulationInfo` Objekte. Wenn `touchDictionary` ein Element enthält, ist es wahrscheinlich, dass die `PreviousPoint` -und- `NewPoint` Werte ungleich sind und die Bewegung eines Fingers darstellen. Wenn die Bitmap durch mehrere Finger berührt wird, enthält das Wörterbuch mehrere Elemente, aber nur eines dieser Elemente hat unterschiedliche `PreviousPoint` -und- `NewPoint` Werte. Alle Rest-Werte haben die gleichen `PreviousPoint` `NewPoint` Werte und.

Dies ist wichtig: die `Manipulate` Methode kann davon ausgehen, dass Sie die Bewegung nur mit einem Finger verarbeitet. Zum Zeitpunkt dieses Aufrufs verschieben sich keine der anderen Finger, und wenn Sie tatsächlich bewegt werden (wie wahrscheinlich), werden diese Bewegungen in zukünftigen Aufrufen von verarbeitet `Manipulate` .

Die- `Manipulate` Methode kopiert das Wörterbuch zur einfacheren Handhabung zunächst in ein Array. Sie ignoriert etwas anderes als die ersten beiden Einträge. Wenn mehr als zwei Finger versuchen, die Bitmap zu manipulieren, werden die anderen ignoriert. `Manipulate`ist der abschließende Member von `TouchManipulationBitmap` :

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

Wenn die Bitmap mit einem Finger manipuliert wird, wird `Manipulate` die- `OneFingerManipulate` Methode des- `TouchManipulationManager` Objekts aufgerufen. Bei zwei Fingern wird aufgerufen `TwoFingerManipulate` . Die Argumente für diese Methoden sind identisch: das `prevPoint` -Argument und das- `newPoint` Argument stellen den Finger dar, der bewegt wird. Das- `pivotPoint` Argument unterscheidet sich jedoch für die beiden-Aufrufe:

Bei einer Bearbeitung mit einem Finger `pivotPoint` ist die Mitte der Bitmap. Dadurch wird die Rotation mit einem Finger ermöglicht. Bei der zwei-Finger-Bearbeitung gibt das-Ereignis die Bewegung von nur einem Finger an, sodass der `pivotPoint` der Finger ist, der nicht bewegt wird.

In beiden Fällen wird `TouchManipulationManager` ein-Wert zurückgegeben, der von `SKMatrix` der-Methode mit der aktuellen-Eigenschaft verkettet wird, die `Matrix` `TouchManipulationPage` zum Rendering der Bitmap verwendet.

`TouchManipulationManager`wird generalisiert und verwendet keine anderen Dateien mit Ausnahme von `TouchManipulationMode` . Sie können diese Klasse möglicherweise ohne Änderungen in ihren eigenen Anwendungen verwenden. Sie definiert eine einzelne Eigenschaft vom Typ `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```

Sie sollten die Option jedoch wahrscheinlich vermeiden `AnisotropicScale` . Mit dieser Option ist es sehr einfach, die Bitmap so zu bearbeiten, dass einer der Skalierungsfaktoren NULL wird. Dadurch wird die Bitmap nicht mehr von Sight, sondern niemals zurückgegeben. Wenn Sie wirklich eine anisotrope Skalierung benötigen, sollten Sie die Logik erweitern, um unerwünschte Ergebnisse zu vermeiden.

`TouchManipulationManager`verwendet Vektoren, aber da `SKVector` in skiasharp keine Struktur vorhanden ist, `SKPoint` wird stattdessen verwendet. `SKPoint`unterstützt den Subtraktions Operator, und das Ergebnis kann als Vektor behandelt werden. Die einzige Vektor spezifische Logik, die hinzugefügt werden musste, ist eine `Magnitude` Berechnung:

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

Wenn die Drehung ausgewählt wurde, wird die Drehung von der Methode mit einem Finger und zwei Fingern zuerst behandelt. Wenn eine Drehung erkannt wird, wird die Rotations Komponente tatsächlich entfernt. Was bleibt, wird als schwenken und Skalieren interpretiert.

Hier ist die- `OneFingerManipulate` Methode. Wenn die Drehung von einem Finger nicht aktiviert wurde, ist die Logik einfach &mdash; . Sie verwendet einfach den vorherigen Punkt und den neuen Punkt, um einen Vektor mit dem Namen zu erstellen `delta` , der exakt der Übersetzung entspricht. Bei aktivierter Einfinger Drehung verwendet die-Methode Winkel vom Pivotpunkt (der Mittelpunkt der Bitmap) bis zum vorherigen Punkt und neuen Punkt, um eine Rotations Matrix zu erstellen:

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

In der- `TwoFingerManipulate` Methode ist der Pivotpunkt die Position des Fingers, der in diesem speziellen Touch-Ereignis nicht bewegt wird. Die Drehung ähnelt der Drehung mit einem Finger, und dann wird der Vektor mit dem Namen `oldVector` (basierend auf dem vorherigen Punkt) für die Drehung angepasst. Die verbleibende Bewegung wird als Skalierung interpretiert:

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

Sie werden feststellen, dass in dieser Methode keine explizite Übersetzung vorliegt. Allerdings basieren sowohl die `MakeRotation` -Methode als auch die- `MakeScale` Methode auf dem Pivotpunkt, der eine implizite Übersetzung einschließt. Wenn Sie zwei Finger auf der Bitmap verwenden und diese in die gleiche Richtung ziehen, erhält `TouchManipulation` eine Reihe von Berührungs Ereignissen, die zwischen den beiden Fingern wechseln. Wenn sich jeder Finger in Relation zum anderen, Skalierungs-oder Rotations Ergebnis bewegt, wird er durch die Bewegung des anderen Fingers negiert, und das Ergebnis ist Übersetzung.

Der einzige verbleibende Teil der **Berührungs Bearbeitungs** Seite ist der `PaintSurface` Handler in der `TouchManipulationPage` Code Behind-Datei. Dadurch wird die- `Paint` Methode von aufgerufen `TouchManipulationBitmap` , die die Matrix anwendet, die die akkumulierte Berührungs Aktivität darstellt:

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

Der `PaintSurface` Handler schließt den Abschluss eines-Objekts ein, `MatrixDisplay` das die akkumulierte Berührungs Matrix anzeigt:

[![](touch-images/touchmanipulation-small.png "Triple screenshot of the Touch Manipulation page")](touch-images/touchmanipulation-large.png#lightbox "Triple screenshot of the Touch Manipulation page")

## <a name="manipulating-multiple-bitmaps"></a>Bearbeiten mehrerer Bitmaps

Einer der Vorteile beim Isolieren von Code für die Berührungs Verarbeitung in Klassen wie `TouchManipulationBitmap` und `TouchManipulationManager` ist die Möglichkeit, diese Klassen in einem Programm wiederzuverwenden, das es dem Benutzer ermöglicht, mehrere Bitmaps zu bearbeiten.

Die **bitmapansichts** Seite zeigt, wie dies geschieht. Anstatt ein Feld vom Typ zu definieren `TouchManipulationBitmap` , [`BitmapScatterPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) definiert die Klasse eine `List` von Bitmap-Objekten:

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

Der Konstruktor lädt alle Bitmaps, die als eingebettete Ressourcen verfügbar sind, und fügt diese der hinzu `bitmapCollection` . Beachten Sie, dass die- `Matrix` Eigenschaft für jedes-Objekt initialisiert wird `TouchManipulationBitmap` , sodass die oberen linken Ecken jeder Bitmap um 100 Pixel versetzt werden.

`BitmapScatterView`Auf der Seite müssen auch Berührungs Ereignisse für mehrere Bitmaps behandelt werden. Anstatt eine der Fingereingabe- `List` IDs von momentan bearbeiteten `TouchManipulationBitmap` Objekten zu definieren, erfordert dieses Programm ein Wörterbuch:

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

Beachten Sie, dass die `Pressed` Logik die `bitmapCollection` in umgekehrter Richtung durchläuft. Die Bitmaps überlappen sich häufig. Die Bitmaps, die später in der Auflistung angezeigt werden, werden auf den Bitmaps weiter oben in der Auflistung angezeigt. Wenn unter dem Finger, der auf dem Bildschirm drückt, mehrere Bitmaps vorhanden sind, muss der oberste Wert mit dem Finger auf dem Bildschirm manipuliert werden.

Beachten Sie außerdem, dass die `Pressed` Logik diese Bitmap an das Ende der Auflistung verschiebt, sodass Sie visuell an den oberen Rand des Stapels anderer Bitmaps verschoben wird.

Im `Moved` -Ereignis und im- `Released` Ereignis `TouchAction` ruft der Handler die- `ProcessingTouchEvent` Methode `TouchManipulationBitmap` wie im vorherigen Programm auf.

Schließlich ruft der `PaintSurface` Handler die- `Paint` Methode für jedes- `TouchManipulationBitmap` Objekt auf:

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

Der Code durchläuft die Auflistung und zeigt den Stapel von Bitmaps vom Anfang der Auflistung bis zum Ende an:

[![](touch-images/bitmapscatterview-small.png "Triple screenshot of the Bitmap Scatter View page")](touch-images/bitmapscatterview-large.png#lightbox "Triple screenshot of the Bitmap Scatter View page")

## <a name="single-finger-scaling"></a>Skalierung mit nur einem Finger

Ein Skalierungs Vorgang erfordert in der Regel eine Pinsel Bewegung mit zwei Fingern. Allerdings ist es möglich, die Skalierung mit einem einzelnen Finger zu implementieren, indem der Finger die Ecken einer Bitmap bewegt.

Dies wird auf der **Skalierungs Seite mit einer einzelnen fingerecke** veranschaulicht. Da in diesem Beispiel eine etwas andere Skalierungs Skala verwendet wird, als in der- `TouchManipulationManager` Klasse implementiert ist, wird diese Klasse oder die-Klasse nicht verwendet `TouchManipulationBitmap` . Stattdessen befindet sich die gesamte Berührungs Logik in der Code-Behind-Datei. Dies ist eine etwas einfachere Logik als üblich, da Sie jeweils nur einen Finger nachverfolgt und alle sekundären Finger ignoriert, die den Bildschirm berühren können.

Die Seite [**singlefingercornerscale. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) instanziiert die `SKCanvasView` -Klasse und erstellt ein- `TouchEffect` Objekt zum Nachverfolgen von Berührungs Ereignissen:

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

Die [**SingleFingerCornerScalePage.XAML.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) -Datei lädt eine Bitmap-Ressource aus dem **Medien** Verzeichnis und zeigt Sie mit einem `SKMatrix` als Feld definierten Objekt an:

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

Dieses `SKMatrix` Objekt wird durch die unten gezeigte Berührungs Logik geändert.

Der Rest der Code Behind-Datei ist der- `TouchEffect` Ereignishandler. Zunächst wird die aktuelle Position des Fingers in einen-Wert umgerechnet `SKPoint` . Für den `Pressed` Aktionstyp prüft der Handler, ob der Bildschirm von einem anderen Finger berührt wird und ob sich der Finger innerhalb der Grenzen der Bitmap befindet.

Der entscheidende Teil des Codes ist eine- `if` Anweisung, die zwei Aufrufe der-Methode beinhaltet `Math.Pow` . Diese mathematische Prüfung überprüft, ob die Fingerposition außerhalb einer Ellipse liegt, die die Bitmap füllt. Wenn dies der Fall ist, dann ist dies ein Skalierungs Vorgang. Der Finger befindet sich in der Nähe einer der Ecken der Bitmap, und es wird ein Pivotpunkt festgelegt, der die entgegengesetzte Ecke ist. Wenn sich der Finger innerhalb dieser Ellipse befindet, handelt es sich um einen regulären schwenkvorgang:

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

Der `Moved` Aktionstyp berechnet eine Matrix, die der Touch-Aktivität ab dem Zeitpunkt entspricht, zu dem der Finger auf den Bildschirm gedrückt hat. Sie verkettet diese Matrix mit der Matrix, die zu dem Zeitpunkt wirksam ist, zu dem der Finger zuerst auf die Bitmap gedrückt hat. Der Skalierungs Vorgang ist immer relativ zur Ecke, die gegen die Ecke steht, die der Finger berührt hat.

Bei kleinen oder unlangen Bitmaps kann eine innere Ellipse den größten Teil der Bitmap belegen und kleine Bereiche an den Ecken zum Skalieren der Bitmap belassen. Möglicherweise bevorzugen Sie einen etwas anderen Ansatz. in diesem Fall können Sie den gesamten `if` Block, der `isScaling` auf festlegt, `true` mit dem folgenden Code ersetzen:

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

Mit diesem Code wird der Bereich der Bitmap effektiv in eine innere Raute Form und vier Dreiecke an den Ecken aufgeteilt. Dadurch können viel größere Bereiche an den Ecken die Bitmap erfassen und skalieren.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Aufrufen von Ereignissen über Effekte](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
