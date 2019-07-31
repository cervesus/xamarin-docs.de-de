---
title: Manipulationen durch Toucheingaben
description: In diesem Artikel wird erläutert, wie mit Matrixtransformationen Touch ziehen, berührpunkte und Drehung implementiert, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: davidbritch
ms.author: dabritch
ms.date: 09/14/2018
ms.openlocfilehash: 407fe78618c5e5fcd8732d9ff3cea50561ca78f3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655547"
---
# <a name="touch-manipulations"></a>Manipulationen durch Toucheingaben

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden von Matrixtransformationen zum Implementieren der Touch ziehen, berührpunkte und Drehung_

In Umgebungen wie z. B. die auf mobilen Geräten Multitouch verwenden Benutzer häufig die Finger zum Bearbeiten von Objekten auf dem Bildschirm. Allgemeine Gesten wie ein Ziehvorgang nur einem Finger und eine zwei-Finger-Prise können verschieben und skaliert Objekte oder sogar drehen können. Für diese Gesten werden in der Regel mithilfe von Transform-Matrizen implementiert, und in diesem Artikel erfahren Sie, wie das geht.

![](touch-images/touchmanipulationsexample.png "Eine Bitmap, bei denen sich Übersetzung, Skalierung und Drehung")

Die hier gezeigten Beispiele verwenden die Xamarin.Forms Nachverfolgen von Touch-Auswirkungen, die in diesem Artikel vorgestellten [ **Aufrufen von Ereignissen von Auswirkungen**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

## <a name="dragging-and-translation"></a>Ziehen und Übersetzung

Eine der wichtigsten Anwendungen von Matrixtransformationen ist die Touch-Verarbeitung. Ein einzelnes [ `SKMatrix` ](xref:SkiaSharp.SKMatrix) Wert kann eine Reihe von Touch-Vorgängen konsolidiert werden. 

Für das Ziehen von einem Finger, die `SKMatrix` Wert führt die Übersetzung. Dies wird veranschaulicht, der **Bitmap ziehen** Seite. Die XAML-Datei instanziiert ein `SKCanvasView` in einer Xamarin.Forms `Grid`. Ein `TouchEffect` Objekt wurde hinzugefügt die `Effects` -Auflistung dieses `Grid`:

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

In der Theorie der `TouchEffect` Objekt kann direkt hinzugefügt werden die `Effects` Auflistung von der `SKCanvasView`, aber auf allen Plattformen funktioniert nicht. Da die `SKCanvasView` ist die gleiche Größe wie die `Grid` in dieser Konfiguration anfügen, damit die `Grid` genauso gut funktioniert.

Die Code-Behind-Datei wird in einer Bitmap-Ressource in seinem Konstruktor geladen, und zeigt sie im der `PaintSurface` Handler:

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

Ohne weiteren Code der `SKMatrix` Wert ist immer der Matrix identifizieren, und mussten keine Auswirkungen auf die Anzeige der Bitmap. Das Ziel der `OnTouchEffectAction` Handler, der in der XAML-Datei festgelegt ist, den Wert "Matrix" Touch Manipulationen entsprechend ändern.

Die `OnTouchEffectAction` Handler beginnt, durch die Konvertierung der Xamarin.Forms `Point` Wert in eine SkiaSharp `SKPoint` Wert. Dies ist einfach der Skalierung basierend auf den `Width` und `Height` Eigenschaften `SKCanvasView` (die sind geräteunabhängige Einheiten) und die `CanvasSize` Eigenschaft, die in Pixel:

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

Wenn ein Finger den Bildschirm, ein Ereignis vom Typ zuerst berührt `TouchActionType.Pressed` ausgelöst wird. Die erste Aufgabe besteht darin, zu bestimmen, ob die Bitmap der Finger berührt. Eine solche Aufgabe wird häufig aufgerufen _Treffertests_. In diesem Fall Treffertests geschieht durch Erstellen einer `SKRect` Wert, der die Bitmap, die durch die Matrixtransformation mit `MapRect`, und klicken Sie dann feststellen, ob es sich bei der Touch-Punkt innerhalb des Rechtecks transformiert wird.

Wenn dies der Fall ist die `touchId` Feld der Touch-ID gesetzt ist, und die Finger Position gespeichert ist.

Für die `TouchActionType.Moved` Ereignis, die Übersetzung Faktoren von der `SKMatrix` Wert auf der aktuellen Position des Fingers, und die neue Position des Fingers basierend angepasst werden. Neue Position für das nächste Mal über gespeichert ist und die `SKCanvasView` für ungültig erklärt.

Wie Sie mit dem Programm zu experimentieren, beachten Sie, dass Sie nur die Bitmap ziehen können, wenn Ihrem Finger berührt, einen Bereich, in denen die Bitmap angezeigt wird. Obwohl diese Einschränkung nicht für dieses Programm wichtig ist, wird es entscheidend, wenn mehrere Bitmaps zu bearbeiten.

## <a name="pinching-and-scaling"></a>Berührpunkte und Skalierung

Was möchten Sie auftreten, wenn zwei Fingern Tippen Sie auf die Bitmap? Wenn zwei Finger gleichzeitig verschieben, möchten Sie wahrscheinlich die Bitmap um zusammen mit den Fingern zu verschieben. Wenn zwei Finger führen Sie ein oder stretch-Vorgangs, sollten Sie die Bitmap gedreht (um im nächsten Abschnitt erläutert) oder skaliert werden soll. Wenn eine Bitmap skaliert wird, ist es die meisten sinnvoll, die für zwei Finger in Bezug auf die Bitmap der gleichen Position bleibt und für die Bitmap entsprechend skaliert werden.

Behandeln zwei Finger gleichzeitig scheint kompliziert, aber denken Sie daran, die die `TouchAction` Handler empfängt nur Informationen zu einem Finger gleichzeitig. Wenn zwei Finger Bitmap bearbeiten, klicken Sie dann für jedes Ereignis, einen Finger Position geändert hat, jedoch das andere nicht geändert wurde. In der **Bitmapskalierung** folgenden Seitencode, der Finger, der Position nicht geändert wird aufgerufen, die _Pivot_ verweisen, da die Transformation relativ zu diesem Punkt ist.

Ein Unterschied zwischen diesem Programm und den oben stehenden Programms ist, dass mehrere die Fingereingabe, die IDs gespeichert werden müssen. In denen die Touch-ID ist der Wörterbuchschlüssel und der Wörterbuchwert ist der aktuellen Position des Fingers wird zu diesem Zweck ein Wörterbuch verwendet:

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

Die Behandlung von der `Pressed` Aktion ist fast identisch mit dem vorherigen, außer dass die ID Programmieren und Kontaktpunkte zum Wörterbuch hinzugefügt werden. Die `Released` und `Cancelled` Aktionen entfernen Wörterbucheintrags.

Die Handhabung der `Moved` Aktion ist jedoch komplexer. Wenn nur mit einem Finger beteiligt sind, und klicken Sie dann die Verarbeitung sehr ähnlich wie die oben stehenden Programms ist. Für zwei oder mehr Finger muss das Programm auch abrufen, Informationen aus dem Wörterbuch, das im Zusammenhang mit dem Finger, der nicht verschoben wird. Hierzu kopieren die Wörterbuchschlüssel in ein Array, und klicken Sie dann verglichen wird des ersten Schlüssels, mit der ID des Fingers verschoben wird. Können die Anwendung zum Abrufen der Dreh-und Angelpunkt für den Finger, der nicht verschoben wird.

Als Nächstes wird berechnet, die zwei Vektoren, die neue Finger Position relativ zum Pivotpunkt und die alte Finger Position relativ zum Pivotpunkt. Das Verhältnis der diese Vektoren sind Faktoren skaliert. Da die Division durch 0 (null) Möglichkeit ist, müssen diese auf unendlich oder NaN (keine Zahl) Werte überprüft werden. Wenn alles funktioniert gut, wird eine Skalierung Transformation verkettet, mit der `SKMatrix` Wert als Feld gespeichert.

Beim Experimentieren mit dieser Seite werden Sie feststellen, dass Sie die Bitmap mit einem oder zwei Fingern ziehen oder Skalieren mit zwei Fingern. Die Skalierung ist _anisotrope_, was bedeutet, dass die Skalierung in horizontaler und vertikaler Richtung unterscheiden kann. Dies verzerrt das Seitenverhältnis wird beibehalten, aber auch können Sie so kippen Sie die Bitmap um ein Spiegelbild zu machen. Sie können auch ermitteln, können Sie die Bitmap zu einer Dimension von 0 (null) verkleinern, und es wird nicht mehr angezeigt. Im Produktionscode sollten Sie dies zu vermeiden.

## <a name="two-finger-rotation"></a>Zwei-Finger-Drehung

Die **Bitmap Drehen** Seite können Sie zwei Finger für Drehung oder kugelstrahler Skalierung verwenden. Die Bitmap behält immer die richtige Seitenverhältnisse beizubehalten. Mit zwei Fingern für Drehung und Skalierung von anisotrope funktioniert sehr gut nicht, da das Verschieben der Finger für beide Aufgaben sehr ähnlich ist.

Der erste große Unterschied in diesem Programm ist die Treffertest-Logik. Die vorherigen Programme verwendet, die `Contains` -Methode der `SKRect` zu bestimmen, ob die Touch-Punkt innerhalb des Rechtecks transformiert wird, das die Bitmap entspricht. Aber wie der Benutzer die Bitmap bearbeitet, die Bitmap gedreht, und `SKRect` ein gedrehtes Rechteck nicht ordnungsgemäß darstellen. Sie können Angst haben, dass die Treffertests Logik in diesem Fall recht komplexen analytische Geometrie zu implementieren muss.

Es ist jedoch eine Verknüpfung verfügbar: Die Bestimmung, ob ein Punkt innerhalb der Grenzen eines transformierten Rechtecks liegt, entspricht dem bestimmen, ob ein umgekehrter transformierter Punkt innerhalb der Grenzen des nicht transformierten Rechtecks liegt. Dies ist eine viel einfachere Berechnung und die Logik kann weiterhin die komfortable `Contains` Methode:

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

Die Logik für die `Moved` Ereignis wie das vorherige Programm beginnt. Zwei Vektoren, die mit dem Namen `oldVector` und `newVector` werden für die vorherige und dem aktuellen Punkt des Fingers verschieben und der Dreh-und Angelpunkt des Fingers unbewegten berechnet. Aber dann Winkel der diese Vektoren bestimmt sind und der Unterschied ist der Winkel der Drehung.

Skalierung kann auch beteiligt sein, damit der alte Vektor basierend auf den Drehwinkel für Bezeichnungen gedreht wird. Die relative Größe der beiden Vektoren ist jetzt der Skalierungsfaktor. Beachten Sie, dass die gleichen `scale` Wert wird verwendet, für die horizontale und vertikale Skalierung so, dass die Skalierung kugelstrahler ist. Die `matrix` Feld wird angepasst, indem Sie sowohl die Rotationsmatrix und eine Skalierung Matrix.

Wenn Ihre Anwendung zum Implementieren von Touch muss können für eine einzelne Bitmap (oder ein anderes Objekt) verarbeiten, Sie den Code aus diesen drei Beispielen für Ihre eigene Anwendung anpassen. Aber wenn Sie Touch-Verarbeitung für mehrere Bitmaps implementieren müssen, sollten Sie wahrscheinlich diese kapseln Operationen in anderen Klassen touch.

## <a name="encapsulating-the-touch-operations"></a>Kapselt die Touch-Vorgänge

Die **Touch-Bearbeitung** Seite erläutert, die Touch-Bearbeitung einer einzelnen Bitmap, aber mit anderen Dateien, die viele der oben gezeigten Logik zu kapseln. Die erste dieser Dateien ist die [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) Enumeration, der die verschiedenen Typen von Touch-Bearbeitung implementiert durch den Code, wird es, angibt:

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

Die `ScaleRotate` Option ist für zwei-Finger-Skalierung und Drehung. Skalierung ist kugelstrahler. Wie bereits erwähnt, ist zwei-Finger-Rotation mit anisotrope Skalierung implementieren problematisch, da die Finger-Bewegungen im Wesentlichen identisch sind.

Die `ScaleDualRotate` Option wird nur einem Finger Drehung hinzugefügt. Wenn ein einzelner Finger das Objekt zieht, wird zuerst das gezogene Objekt um seinen Mittelpunkt gedreht, sodass der Mittelpunkt des Objekts mit dem Ziehen Initialisierungsvektor.

Die [ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) -Datei enthält eine `Picker` mit den Elementen der `TouchManipulationMode` Enumeration:

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
            bitmap.TouchManager.Mode = (TouchManipulationMode)picker.SelectedItem;
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

Die `HitTest` Methodenrückgabe `true` , wenn der Benutzer den Bildschirm zu einem Zeitpunkt innerhalb der Grenzen der Bitmap berührt. Dabei wird die Logik, die zuvor gezeigt verwendet die **Drehung der Bitmap** Seite:

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

Das ist wichtig: Die `Manipulate` Methode kann davon ausgehen, dass die Bewegung nur mit einem Finger verarbeitet wird. Zum Zeitpunkt des Aufrufs keine anderen Finger verschieben sind, und wenn sie wirklich (wie wahrscheinlich ist) verschieben, werden dieser Verschiebungen in zukünftigen Aufrufen von verarbeitet `Manipulate`.

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

Dies wird veranschaulicht, der **Finger Ecke Skala** Seite. Da in diesem Beispiel, eine etwas andere Art verwendet von als Skalierung implementiert, die der `TouchManipulationManager` -Klasse, wird diese Klasse nicht verwendet oder die `TouchManipulationBitmap` Klasse. Stattdessen wird die gesamte Touch Logik in der CodeBehind-Datei. Dies ist etwas einfacher Logik als üblich, weil es nur mit einem Finger gleichzeitig verfolgt und ignoriert einfach alle sekundären Finger den Bildschirm berührt werden können.

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

Für kleine oder längliche Bitmaps kann eine innere Ellipse belegen die meisten der Bitmap und kleine Bereiche an den Ecken die Bitmap skalieren lassen. Möglicherweise bevorzugen Sie einen etwas anderen Ansatz, in diesem Fall können Sie diese gesamte `if` Block, der festlegt `isScaling` zu `true` durch den folgenden Code:

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

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Aufrufen von Ereignissen von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
