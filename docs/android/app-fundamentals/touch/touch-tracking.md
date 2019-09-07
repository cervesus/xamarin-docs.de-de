---
title: Multitouch-Finger Verfolgung in xamarin. Android
description: In diesem Thema wird veranschaulicht, wie Berührungs Ereignisse von mehreren Fingern nachverfolgt werden.
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 6dd3bf848d38f0211dcda100994f6f7ec8831fce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754693"
---
# <a name="multi-touch-finger-tracking"></a>Multitouch-Finger Verfolgung

_In diesem Thema wird veranschaulicht, wie Berührungs Ereignisse von mehreren Fingern nachverfolgt werden._

Es gibt Zeiten, in denen eine Multitouch-Anwendung einzelne Finger nachverfolgen muss, während Sie sich gleichzeitig auf dem Bildschirm bewegen. Eine typische Anwendung ist ein Finger Paint-Programm. Sie möchten, dass der Benutzer mit einem einzelnen Finger zeichnen kann, aber gleichzeitig mit mehreren Fingern zeichnen kann. Wenn das Programm mehrere Touch-Ereignisse verarbeitet, muss es unterscheiden, welche Ereignisse den einzelnen Fingern entsprechen. Android stellt zu diesem Zweck einen ID-Code bereit, aber das Abrufen und Verarbeiten des Codes kann etwas kompliziert sein.

Für alle Ereignisse, die einem bestimmten Finger zugeordnet sind, bleibt der ID-Code unverändert. Der ID-Code wird zugewiesen, wenn ein Finger den Bildschirm zum ersten Mal berührt und ungültig wird, nachdem der Finger auf dem Bildschirm angezeigt wird.
Diese ID-Codes sind im Allgemeinen sehr klein und werden von Android für spätere Berührungs Ereignisse wieder verwendet.

Fast immer verwaltet ein Programm, das einzelne Finger verfolgt, ein Wörterbuch für die Berührungs Überwachung. Der Wörterbuch Schlüssel ist der ID-Code, der einen bestimmten Finger identifiziert. Der Wörter Buchwert hängt von der Anwendung ab. Im [fingerpaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) -Programm wird jeder Finger Strich (von Touch zu Release) einem Objekt zugeordnet, das alle Informationen enthält, die erforderlich sind, um die mit diesem Finger gezeichnete Linie zu renderieren. Das Programm definiert zu diesem `FingerPaintPolyline` Zweck eine kleine Klasse:

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

Jede Polylinie weist eine Farbe, eine Strichbreite und ein Android-Grafik [`Path`](xref:Android.Graphics.Path) Objekt auf, um mehrere Punkte der Linie zu sammeln und zu Renderern, während Sie gezeichnet werden.

Der Rest des unten gezeigten Codes ist in einer Ableitung mit `View` dem Namen `FingerPaintCanvasView`enthalten. Diese Klasse verwaltet ein Wörterbuch von Objekten des `FingerPaintPolyline` Typs, wenn Sie aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Dieses Wörterbuch ermöglicht der Ansicht, schnell die `FingerPaintPolyline` einem bestimmten Finger zugeordneten Informationen zu erhalten.

Die `FingerPaintCanvasView` -Klasse verwaltet auch `List` ein-Objekt für die ausgefüllten Polylinien:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in derselben Reihenfolge, in der Sie gezeichnet wurden.

`FingerPaintCanvasView`überschreibt zwei durch `View`definierte Methoden:[`OnDraw`](xref:Android.Views.View.OnDraw*)
und [`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*).
Bei der `OnDraw` außer Kraft Setzung zeichnet die Ansicht die abgeschlossenen Polylinien und zeichnet dann die in Bearbeitung befindlichen Polylinien.

Die außer Kraft `OnTouchEvent` setzung der-Methode beginnt mit `pointerIndex` dem Abrufen eines `ActionIndex` Werts aus der-Eigenschaft. Dieser `ActionIndex` Wert unterscheidet sich zwischen mehreren Fingern, ist aber nicht über mehrere Ereignisse hinweg konsistent. Aus diesem Grund verwenden Sie den `pointerIndex` , um den Zeiger `id` Wert aus der `GetPointerId` Methode abzurufen. Diese ID *ist* in mehreren Ereignissen konsistent:

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

Beachten Sie, dass die- `ActionMasked` außer Kraft Setzung `switch` anstelle der `Action` -Eigenschaft die-Eigenschaft in der-Anweisung verwendet. Erläuterung:

Wenn Sie sich mit Multi-Touch beschäftigen, hat `Action` die-Eigenschaft den `MotionEventsAction.Down` Wert, damit der erste Finger den Bildschirm berührt, und dann werden `Pointer2Down` die `Pointer3Down` Werte von und als zweite und dritte Finger auf den Bildschirm übergegangen. Wenn der vierte und fünfte Fingerkontakt herstellen, weist `Action` die-Eigenschaft numerische Werte auf, die nicht einmal den Membern `MotionEventsAction` der-Enumeration entsprechen. Sie müssen die Werte der Bitflags in den Werten überprüfen, um zu interpretieren, was Sie bedeuten.

Ebenso `Action` hat die `Pointer2Up` -Eigenschaft den Wert und `Pointer3Up` für das zweite und dritte Finger und `Up` für den ersten Finger, wenn die Finger die Verbindung mit dem Bildschirm verlassen.

Die `ActionMasked` -Eigenschaft benötigt eine geringere Anzahl von Werten, da Sie in Verbindung mit der `ActionIndex` -Eigenschaft verwendet werden soll, um zwischen mehreren Fingern zu unterscheiden. Wenn Finger den Bildschirm berühren, kann die-Eigenschaft `MotionEventActions.Down` nur für den ersten Finger `PointerDown` und für nachfolgende Finger gleich sein. Wenn die Finger den Bildschirm verlassen `ActionMasked` , enthält die `Pointer1Up` Werte für die nachfolgenden `Up` Finger und für den ersten Finger.

Wenn verwendet `ActionMasked` wird,`MotionEvent` unter scheidetunterdennachfolgendenFingern,umsichzuberührenunddenBildschirmzuverlassen,aberSiemüssendiesenWertinderRegelnichtalsArgumentfürandereMethodenim`ActionIndex` -Objekt verwenden. Für Multi-Touch wird `GetPointerId` eine der wichtigsten dieser Methoden im obigen Code aufgerufen. Diese Methode gibt einen Wert zurück, den Sie für einen Wörterbuch Schlüssel verwenden können, um bestimmte Ereignisse zu Fingern zuzuordnen.

Die `OnTouchEvent` außer Kraft Setzung [im fingerpaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) -Programm `MotionEventActions.Down` verarbeitet das-Ereignis und das- `PointerDown` Ereignis `FingerPaintPolyline` , indem ein neues-Objekt erstellt und dem Wörterbuch hinzugefügt wird:

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

Beachten Sie, `pointerIndex` dass der auch zum Abrufen der Position des Fingers in der Ansicht verwendet wird. Alle Berührungs Informationen sind dem `pointerIndex` Wert zugeordnet. Der `id` identifiziert Finger in mehreren Nachrichten eindeutig, sodass der Wörterbucheintrag erstellt wird.

Entsprechend `OnTouchEvent` `Pointer1Up` `completedPolylines` `OnDraw` behandelt die Überschreibung auch undidentischdurchübertragenderabgeschlossenenPolylinieandieAuflistung,sodassSiewährendderÜberschreibunggezeichnetwerdenkönnen.`MotionEventActions.Up` Der Code entfernt auch den `id` Eintrag aus dem Wörterbuch:

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

Zwischen den Down-und up-Ereignissen gibt es im `MotionEventActions.Move` allgemeinen viele Ereignisse. Diese werden in einem einzelnen-Rückruf `OnTouchEvent`gebündelt, und Sie müssen anders behandelt werden als das-Ereignis und das `Down` - `Up` Ereignis. Der `pointerIndex` zuvor aus der `ActionIndex` -Eigenschaft erhaltene Wert muss ignoriert werden. Stattdessen muss die `pointerIndex` -Methode mehrere Werte abrufen, indem Sie eine Schleife zwischen 0 und der `PointerCount` -Eigenschaft durchlaufen und dann für jeden `pointerIndex` dieser Werte einen `id` abrufen:

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

[![Beispiel eines Screenshots aus dem fingerpaint-Beispiel](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und voneinander unterscheiden können.

## <a name="related-links"></a>Verwandte Links

- [Entsprechendes xamarin IOS-Handbuch](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Fingerpaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
