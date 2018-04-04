---
title: Multi-Touch-Finger-Überwachung
description: In diesem Thema wird veranschaulicht, wie zum Nachverfolgen von touchereignissen aus mehreren Finger
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9c0206de17e0c60803252328ff0398cee0997dbb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="multi-touch-finger-tracking"></a>Multi-Touch-Finger-Überwachung

_In diesem Thema wird veranschaulicht, wie zum Nachverfolgen von touchereignissen aus mehreren Finger_

Es gibt Situationen, wenn eine Multi-Touch-Anwendung muss einzelne Finger zu verfolgen, während sie gleichzeitig auf dem Bildschirm zu verschieben. Eine typische Anwendung ist ein Finger-paint-Programm. Die Benutzer können mit einem einzelnen Finger gezeichnet werden soll, sondern auch gleichzeitig mit mehreren Fingern gezeichnet werden soll. Wie Ihre Anwendung mehrere Berührungsereignisse verarbeitet, muss er zu unterscheiden, welche Ereignisse jeden Finger entsprechen. Android liefert einen ID-Code für diesen Zweck, aber abrufen und diesen Code behandeln können ein bisschen schwierig.

Für alle Ereignisse im Zusammenhang mit einem bestimmten Finger bleibt die ID-Code. Die ID-Code wird zugewiesen, wenn ein Finger zuerst den Bildschirm berührt und wird ungültig, nachdem Sie den Finger vom Bildschirm hebt.
Diese ID-Codes sind im Allgemeinen sehr klein ganze Zahlen und Android für spätere Berührungsereignisse wiederverwendet.

Ein Programm, das die einzelnen Finger nachverfolgt verwaltet fast immer ein Wörterbuch zum Nachverfolgen von Touch. Die Wörterbuchschlüssel ist die ID-Code, der einen bestimmten Finger identifiziert. Der Wörterbuchwert hängt von der Anwendung ab. In der [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) Programm, jeden Finger Strich (von Touch freigeben) bezieht sich auf ein Objekt, alle Informationen, die erforderlich sind enthält, um mit diesem Finger gezeichnete Linie zu rendern. Das Programm definiert eine kleine `FingerPaintPolyline` Klasse für diesen Zweck:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Jede Polylinie hat eine Farbe, Strichbreite und eine Android Grafiken [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) Objekt ansammeln und Rendern von mehreren Punkten der Zeile, wie gezeichnet wird.

Im weiteren Verlauf des unten angezeigten Codes befindet sich einem `View` Ableitung, die mit dem Namen `FingerPaintCanvasView`. Dass die Klasse ein Wörterbuch von Objekten des Typs verwaltet `FingerPaintPolyline` während der Zeit, die sie aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Dieses Wörterbuch kann die Ansicht, um schnell ermitteln der `FingerPaintPolyline` Informationen, die einem bestimmten Finger zugeordnet.

Die `FingerPaintCanvasView` Klasse verwaltet auch eine `List` Objekt für die Polylinien, die abgeschlossen wurden:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in der gleichen Reihenfolge an, dass sie gezeichnet wurden.

`FingerPaintCanvasView` zwei Methoden, die durch definierten überschreibt `View`: [ `OnDraw` ](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/) und [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
In seiner `OnDraw` "Override", "der Ansicht zeichnet die abgeschlossenen Polylinien, und klicken Sie dann zeichnet die Polylinien in Bearbeitung.

Überschreiben von der `OnTouchEvent` Methode beginnt mit der abrufen eine `pointerIndex` Wert aus der `ActionIndex` Eigenschaft. Dies `ActionIndex` Wert unterscheidet zwischen mehreren Finger, aber es ist nicht über mehrere Ereignisse konsistent. Aus diesem Grund verwenden Sie die `pointerIndex` zum Abrufen des Zeigers `id` Wert aus der `GetPointerId` Methode. Diese ID *ist* Konsistenz im gesamten mehrere Ereignisse:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Beachten Sie, die die Außerkraftsetzung verwendet die `ActionMasked` Eigenschaft in der `switch` Anweisung statt über das `Action` Eigenschaft. Erläuterung:

Beim Umgang mit Multi-Touch, sind die `Action` Eigenschaft hat den Wert der `MotionEventsAction.Down` für Zeigefinger, berühren Sie den Bildschirm und Werte der `Pointer2Down` und `Pointer3Down` als das zweite und dritte Finger auch den Bildschirm berühren. Wie die vierte und fünfte Finger Stellen wenden Sie sich an, die `Action` Eigenschaft hat numerische Werte, die auch Mitglieder der entsprechen nicht den `MotionEventsAction` Enumeration! Sie müssten die Werte von Bitflags in den Werten zu interpretieren, ihre Bedeutung zu untersuchen.

Auf ähnliche Weise wie die Finger aufeinander Kontakt mit dem Bildschirm lassen die `Action` Eigenschaft enthält die Werte von `Pointer2Up` und `Pointer3Up` für den zweiten und dritten Finger und `Up` für Zeigefinger.

Die `ActionMasked` -Eigenschaft übernimmt eine geringere Anzahl der Werte, da es in Verbindung mit verwendet werden soll, hat die `ActionIndex` Eigenschaft, um die Unterscheidung zwischen mehreren Fingern fest. Wenn ein Finger den Bildschirm berühren, wird die Eigenschaft kann nur gleich `MotionEventActions.Down` für Zeigefinger und `PointerDown` für nachfolgende Fingern fest. Wie lassen Sie den Finger den Bildschirm `ActionMasked` enthält die Werte von `Pointer1Up` für nachfolgende Finger und `Up` für Zeigefinger.

Bei Verwendung `ActionMasked`, `ActionIndex` unterscheidet zwischen nachfolgende Finger Touch und lassen Sie den Bildschirm jedoch in der Regel verwenden Sie diesen Wert außer als Argument an andere Methoden im müssen nicht die `MotionEvent` Objekt. Für Multi-Touch, zu den wichtigsten dieser Methoden ist `GetPointerId` im obigen Code aufgerufen. Dass die Methode einen Wert zurückgibt können Sie für eine Wörterbuchschlüssel verwenden, um bestimmte Ereignisse zu Finger zuzuordnen.

Die `OnTouchEvent` außer Kraft setzen, der [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) Programmvorgänge der `MotionEventActions.Down` und `PointerDown` Ereignisse identisch durch Erstellen eines neuen `FingerPaintPolyline` -Objekt und dem Wörterbuch hinzugefügt:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Beachten Sie, dass die `pointerIndex` auch verwendet, um die Position des Fingers innerhalb der Ansicht abzurufen. Alle Touch Informationen zugeordnet ist die `pointerIndex` Wert. Die `id` Finger über mehrere Nachrichten eindeutig identifiziert, sodass verwendeten Wörterbucheintrags zu erstellen.

Auf ähnliche Weise die `OnTouchEvent` überschreiben auch Handles die `MotionEventActions.Up` und `Pointer1Up` identisch, indem Sie die abgeschlossene Polylinie zum Übertragen der `completedPolylines` Sammlung, damit sie während der gezeichnet werden können die `OnDraw` außer Kraft setzen. Der Code entfernt auch den `id` Eintrag aus dem Wörterbuch:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Jetzt für die schwierige Teil ist.

Zwischen den nach unten und Ereignisse sind im Allgemeinen gibt es viele `MotionEventActions.Move` Ereignisse. Diese werden in einem einzigen Aufruf gebündelt `OnTouchEvent`, und sie müssen über anders behandelt werden die `Down` und `Up` Ereignisse. Die `pointerIndex` zuvor abgerufenen Wert der `ActionIndex` Eigenschaft muss ignoriert werden. Stattdessen Abrufen der Methode muss mehrere `pointerIndex` Werte von Schleifen zwischen 0 und der `PointerCount` -Eigenschaft, und rufen Sie dann eine `id` für jede dieser `pointerIndex` Werte:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

Diese Art der Verarbeitung ermöglicht die [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) Programm zum Nachverfolgen von einzelnen Finger und zeichnen die Ergebnisse auf dem Bildschirm:

[![Beispiel-Screenshot FingerPaint-Beispiel](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und diese zu unterscheiden.


## <a name="related-links"></a>Verwandte Links

- [Entsprechende Xamarin iOS-Handbuch](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
