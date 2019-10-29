---
title: Multitouch-Finger Verfolgung in xamarin. Android
description: In diesem Thema wird veranschaulicht, wie Berührungs Ereignisse von mehreren Fingern nachverfolgt werden.
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: f960c3cec90bd331f5a1433a869c7720b40c9680
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024272"
---
# <a name="multi-touch-finger-tracking"></a>Multitouch-Finger Verfolgung

_In diesem Thema wird veranschaulicht, wie Berührungs Ereignisse von mehreren Fingern nachverfolgt werden._

Es gibt Zeiten, in denen eine Multitouch-Anwendung einzelne Finger nachverfolgen muss, während Sie sich gleichzeitig auf dem Bildschirm bewegen. Eine typische Anwendung ist ein Finger Paint-Programm. Sie möchten, dass der Benutzer mit einem einzelnen Finger zeichnen kann, aber gleichzeitig mit mehreren Fingern zeichnen kann. Wenn das Programm mehrere Touch-Ereignisse verarbeitet, muss es unterscheiden, welche Ereignisse den einzelnen Fingern entsprechen. Android stellt zu diesem Zweck einen ID-Code bereit, aber das Abrufen und Verarbeiten des Codes kann etwas kompliziert sein.

Für alle Ereignisse, die einem bestimmten Finger zugeordnet sind, bleibt der ID-Code unverändert. Der ID-Code wird zugewiesen, wenn ein Finger den Bildschirm zum ersten Mal berührt und ungültig wird, nachdem der Finger auf dem Bildschirm angezeigt wird.
Diese ID-Codes sind im Allgemeinen sehr klein und werden von Android für spätere Berührungs Ereignisse wieder verwendet.

Fast immer verwaltet ein Programm, das einzelne Finger verfolgt, ein Wörterbuch für die Berührungs Überwachung. Der Wörterbuch Schlüssel ist der ID-Code, der einen bestimmten Finger identifiziert. Der Wörter Buchwert hängt von der Anwendung ab. Im [fingerpaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) -Programm wird jeder Finger Strich (von Touch zu Release) einem Objekt zugeordnet, das alle Informationen enthält, die erforderlich sind, um die mit diesem Finger gezeichnete Linie zu renderieren. Das Programm definiert für diesen Zweck eine kleine `FingerPaintPolyline` Klasse:

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

Jede Polylinie hat eine Farbe, eine Strichbreite und ein Android-Grafik [`Path`](xref:Android.Graphics.Path) Objekt, um beim Zeichnen mehrere Punkte der Linie zu sammeln und zu Renderern.

Der Rest des unten gezeigten Codes ist in einer `View` Ableitung mit dem Namen `FingerPaintCanvasView`enthalten. Diese Klasse verwaltet ein Wörterbuch von Objekten vom Typ `FingerPaintPolyline` während der Zeit, in der Sie aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Dieses Wörterbuch ermöglicht der Ansicht, schnell die `FingerPaintPolyline` Informationen zu erhalten, die einem bestimmten Finger zugeordnet sind.

Die `FingerPaintCanvasView`-Klasse verwaltet auch ein `List`-Objekt für die ausgefüllten Polylinien:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in derselben Reihenfolge, in der Sie gezeichnet wurden.

`FingerPaintCanvasView` überschreibt zwei durch `View`definierte Methoden: [`OnDraw`](xref:Android.Views.View.OnDraw*)
und [`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*).
In der `OnDraw` Überschreibung zeichnet die Ansicht die abgeschlossenen Polylinien und zeichnet dann die in Bearbeitung befindlichen Polylinien.

Die außer Kraft setzung der `OnTouchEvent` Methode beginnt mit dem Abrufen eines `pointerIndex` Werts aus der `ActionIndex`-Eigenschaft. Dieser `ActionIndex` Wert unterscheidet sich zwischen mehreren Fingern, ist aber nicht über mehrere Ereignisse hinweg konsistent. Aus diesem Grund verwenden Sie den `pointerIndex`, um den Zeiger `id` Wert aus der `GetPointerId`-Methode abzurufen. Diese ID *ist* in mehreren Ereignissen konsistent:

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

Beachten Sie, dass die-außer Kraft Setzung die `ActionMasked`-Eigenschaft in der `switch`-Anweisung anstelle der `Action`-Eigenschaft verwendet. Erläuterung:

Wenn Sie mit Multi-Touch arbeiten, hat die `Action`-Eigenschaft den Wert `MotionEventsAction.Down`, damit der erste Finger den Bildschirm berührt, und dann werden die Werte `Pointer2Down` und `Pointer3Down` als zweite und dritte Finger den Bildschirm berühren. Wenn der vierte und fünfte Fingerkontakt herstellen, hat die `Action`-Eigenschaft numerische Werte, die nicht einmal den Membern der `MotionEventsAction`-Enumeration entsprechen. Sie müssen die Werte der Bitflags in den Werten überprüfen, um zu interpretieren, was Sie bedeuten.

Ebenso hat die `Action`-Eigenschaft Werte `Pointer2Up` und `Pointer3Up` für das zweite und dritte Finger und `Up` für den ersten Finger, wenn die Finger die Verbindung mit dem Bildschirm verlassen.

Die `ActionMasked`-Eigenschaft benötigt eine geringere Anzahl von Werten, da Sie in Verbindung mit der `ActionIndex`-Eigenschaft verwendet werden soll, um zwischen mehreren Fingern zu unterscheiden. Wenn Finger den Bildschirm berühren, kann die-Eigenschaft nur `MotionEventActions.Down` für den ersten Finger und `PointerDown` für nachfolgende Finger gleich sein. Wenn die Finger den Bildschirm verlassen, sind `ActionMasked` Werte von `Pointer1Up` für die nachfolgenden Finger und `Up` für den ersten Finger.

Wenn Sie `ActionMasked`verwenden, unterscheidet der `ActionIndex` zwischen den nachfolgenden Fingern, um sich zu berühren und den Bildschirm zu verlassen, aber Sie müssen diesen Wert in der Regel nicht als Argument für andere Methoden im `MotionEvent`-Objekt verwenden. Für Multi-Touch ist eine der wichtigsten dieser Methoden `GetPointerId` im obigen Code aufgerufen. Diese Methode gibt einen Wert zurück, den Sie für einen Wörterbuch Schlüssel verwenden können, um bestimmte Ereignisse zu Fingern zuzuordnen.

Die `OnTouchEvent` Überschreibung im [fingerpaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) -Programm verarbeitet die `MotionEventActions.Down` und `PointerDown` Ereignisse identisch, indem ein neues `FingerPaintPolyline` Objekt erstellt und dem Wörterbuch hinzugefügt wird:

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

Beachten Sie, dass die `pointerIndex` auch verwendet wird, um die Position des Fingers in der Ansicht abzurufen. Alle Berührungs Informationen sind dem `pointerIndex` Wert zugeordnet. Mit der `id` werden Finger über mehrere Nachrichten eindeutig identifiziert, sodass der Wörterbucheintrag erstellt wird.

Ebenso behandelt die `OnTouchEvent` Überschreibung auch die `MotionEventActions.Up` und `Pointer1Up` identisch, indem die abgeschlossene Polylinie an die `completedPolylines` Auflistung übertragen wird, damit Sie während der `OnDraw` Überschreibung gezeichnet werden können. Der Code entfernt auch den `id` Eintrag aus dem Wörterbuch:

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

Nun für den kniffligen Teil.

Zwischen den Down-und up-Ereignissen gibt es im Allgemeinen viele `MotionEventActions.Move` Ereignisse. Diese werden in einem einzigen `OnTouchEvent`aufgerufen, und Sie müssen anders als die Ereignisse `Down` und `Up` behandelt werden. Der zuvor von der `ActionIndex`-Eigenschaft erhaltene `pointerIndex` Wert muss ignoriert werden. Stattdessen muss die-Methode mehrere `pointerIndex` Werte abrufen, indem Sie eine Schleife zwischen 0 und der `PointerCount`-Eigenschaft durchlaufen und dann eine `id` für jeden dieser `pointerIndex` Werte abrufen:

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

Dieser Verarbeitungstyp ermöglicht dem [fingerpaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) -Programm das Nachverfolgen einzelner Finger und das Zeichnen der Ergebnisse auf dem Bildschirm:

[![Beispiel Bildschirm Abbildung von fingerpaint example](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und voneinander unterscheiden können.

## <a name="related-links"></a>Verwandte Links

- [Entsprechendes xamarin IOS-Handbuch](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Fingerpaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
