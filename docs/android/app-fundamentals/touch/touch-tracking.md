---
title: Multitouch-Finger in Xamarin.Android nachverfolgen
description: In diesem Thema wird veranschaulicht, wie zum Nachverfolgen von Touch-Ereignissen aus mehreren Fingern
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 34a9d2d9b8acb05a1b978a70e85038507032faaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61011784"
---
# <a name="multi-touch-finger-tracking"></a>Multitouch-Finger-nachverfolgung

_In diesem Thema wird veranschaulicht, wie zum Nachverfolgen von Touch-Ereignissen aus mehreren Fingern_

Es gibt Situationen, wenn eine Multi-Touch-Anwendung muss einzelner Finger zu verfolgen, während sie gleichzeitig auf dem Bildschirm zu verschieben. Eine typische Anwendung ist ein Programm Finger-paint. Sie möchten die Benutzer mit einem einzelnen Finger gezeichnet werden soll, sondern auch gleichzeitig mit mehreren Fingern zeichnen kann. Während das Programm mehrere touchereignisse verarbeitet, muss er zu unterscheiden, welche Ereignisse, die jeden Finger entsprechen. Android bietet einen ID-Code für diesen Zweck, aber abrufen und Verarbeiten dieser Code können etwas schwierig sein.

Für alle Ereignisse mit einem bestimmten Finger verknüpft ist bleibt die ID-Code unverändert. Die ID-Code wird zugewiesen, wenn ein Finger zuerst den Bildschirm berührt, und wird ungültig, nachdem Sie den Finger vom Bildschirm hebt.
Diese ID-Codes werden im Allgemeinen sehr klein Ganzzahlen und Android für späteren Berührungsereignisse wiederverwendet.

Ein Programm, das einzelner Finger verfolgt wird fast immer ein Wörterbuch zum Nachverfolgen von Touch verwaltet. Die Wörterbuchschlüssel ist die ID-Code, der einen bestimmten Finger identifiziert. Der Wörterbuchwert hängt von der Anwendung ab. In der [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) Programm Strichfarbe Finger (von Toucheingabe freigeben) bezieht sich auf ein Objekt, das alle Informationen, die zum Rendern mit Fingers gezeichneten Linie enthält. Das Programm definiert ein kleines `FingerPaintPolyline` -Klasse für diesen Zweck:

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

Jeder Polylinie verfügt über eine Farbe, eine Strichbreite und eine Android-Grafiken [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) Objekt, das Sammeln und mehrere Punkte der Linie zu rendern, wie er gerade gezeichnet wird.

Der Rest von den folgenden Code ist Bestandteil einer `View` -Ableitung namens `FingerPaintCanvasView`. Dass die Klasse ein Wörterbuch von Objekten des Typs verwaltet `FingerPaintPolyline` während des Zeitraums, der diese aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Dieses Wörterbuch kann die Ansicht schnell Abrufen der `FingerPaintPolyline` Informationen, die einem bestimmten Finger zugeordnet.

Die `FingerPaintCanvasView` Klasse verwaltet auch eine `List` -Objekt für die Polylinien, die abgeschlossen wurden:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in der gleichen Reihenfolge an, dass sie gezeichnet wurden.

`FingerPaintCanvasView` überschreibt zwei Methoden definiert, indem `View`: [`OnDraw`](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/)
und [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
In der `OnDraw` "Override", "der Ansicht die abgeschlossenen Polylinien zeichnet und zeichnet dann die Polylinien in Bearbeitung.

Überschreiben der `OnTouchEvent` Methode beginnt mit dem Abrufen von einer `pointerIndex` Wert aus der `ActionIndex` Eigenschaft. Dies `ActionIndex` Wert unterscheidet sich zwischen mehreren Fingern, aber es ist nicht über mehrere Ereignisse hinweg konsistent. Aus diesem Grund verwenden Sie die `pointerIndex` zum Abrufen des Zeigers `id` Wert aus der `GetPointerId` Methode. Diese ID *ist* Konsistenz im gesamten mehrere Ereignisse:

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

Beachten Sie, die die Außerkraftsetzung verwendet die `ActionMasked` -Eigenschaft in der `switch` Anweisung anstelle der `Action` Eigenschaft. Erläuterung:

Im Zusammenhang mit Multi-Touch, sind die `Action` Eigenschaft hat den Wert `MotionEventsAction.Down` für den ersten Finger auf dem Bildschirm, und klicken Sie dann Werte der touch `Pointer2Down` und `Pointer3Down` als die zweite und dritte Finger auch den Bildschirm berühren. Die vierte und fünfte Finger Stellen wenden Sie sich an, die `Action` Eigenschaft verfügt über numerische Werte, die Mitglieder selbst entsprechen nicht, den `MotionEventsAction` Enumeration! Sie müssten die Werte von Bitflags, die Werte zum Interpretieren, zu deren Bedeutung zu untersuchen.

Auf ähnliche Weise wie die Finger Kontakt mit dem Bildschirm, lassen Sie die `Action` Eigenschaft enthält die Werte von `Pointer2Up` und `Pointer3Up` für die zweite und dritte Finger und `Up` für den ersten Finger.

Die `ActionMasked` -Eigenschaft übernimmt eine geringere Anzahl von Werten, da es dafür vorgesehen ist, die in Verbindung mit verwendet werden die `ActionIndex` Eigenschaft unterscheiden mehrerer Finger. Wenn der Finger den Bildschirm berühren, wird die Eigenschaft kann nur gleich `MotionEventActions.Down` für den ersten Finger und `PointerDown` für nachfolgende Finger. Wie lassen Sie die Finger den Bildschirm `ActionMasked` enthält die Werte von `Pointer1Up` für die nachfolgenden Finger und `Up` für den ersten Finger.

Bei Verwendung `ActionMasked`, `ActionIndex` unterscheidet zwischen nachfolgenden Finger Touch zu lassen, auf der Bildschirm, aber Sie in der Regel verwenden Sie diesen Wert außer als Argument an andere Methoden im müssen nicht die `MotionEvent` Objekt. Für Multi-Touch, eine der wichtigsten dieser Methoden ist `GetPointerId` im obigen Code aufgerufen. Dass die Methode einen Wert zurückgibt können für ein Dictionary-Schlüssel Sie bestimmte Ereignisse Finger zuordnen.

Die `OnTouchEvent` außer Kraft setzen, der [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programmprozesse der `MotionEventActions.Down` und `PointerDown` Ereignisse identisch durch Erstellen eines neuen `FingerPaintPolyline` -Objekts und zum Wörterbuch hinzufügen:

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

Beachten Sie, dass die `pointerIndex` wird auch verwendet, um die Position des Fingers in der Ansicht zu erhalten. Alle Touch Informationen zugeordnet ist die `pointerIndex` Wert. Die `id` Finger über mehrere Nachrichten, eindeutig identifiziert, sodass verwendete des Wörterbucheintrags zu erstellen.

Auf ähnliche Weise die `OnTouchEvent` überschreiben auch die Handles der `MotionEventActions.Up` und `Pointer1Up` identisch, indem Sie das abgeschlossene Polyline-Objekt zum Übertragen der `completedPolylines` Sammlung, damit sie während der gezeichnet werden, können die `OnDraw` außer Kraft setzen. Entfernt der Code auch die `id` Eintrag aus dem Wörterbuch:

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

Jetzt für die der schwierige Teil.

Zwischen den nach unten, und Ereignisse, in der Regel stehen viele `MotionEventActions.Move` Ereignisse. Diese werden in einem einzigen Aufruf gebündelt `OnTouchEvent`, und sie müssen über anders behandelt werden die `Down` und `Up` Ereignisse. Die `pointerIndex` zuvor abgerufenen Wert der `ActionIndex` Eigenschaft muss ignoriert werden. Stattdessen muss die Methode mehrere abrufen `pointerIndex` Werte von Schleifen zwischen 0 und die `PointerCount` -Eigenschaft, und rufen Sie dann eine `id` für jede dieser `pointerIndex` Werte:

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

Diese Art der Verarbeitung ermöglicht die [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) Programm einzelner Finger nachverfolgen und die Ergebnisse auf dem Bildschirm zeichnen:

[![Beispielscreenshot FingerPaint-Beispiel](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und diese zu unterscheiden.


## <a name="related-links"></a>Verwandte Links

- [Entsprechende Xamarin iOS-Entwicklerhandbuch](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
