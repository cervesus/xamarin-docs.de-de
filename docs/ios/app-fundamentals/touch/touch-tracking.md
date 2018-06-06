---
title: Multi-Touch-Finger in Xamarin.iOS nachverfolgen
description: Dieses Dokument beschreibt, wie einzelne Finger in Multitouch-Gesten in einer app Xamarin.iOS nachverfolgt wird. Es basiert auf einem finger-painting app-Beispiel.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 85dbd3c158408026f4ecef5fb2b01c265747140e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784402"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Multi-Touch-Finger in Xamarin.iOS nachverfolgen

_Dieses Dokument veranschaulicht, wie zum Nachverfolgen von touchereignissen aus mehreren Finger_

Es gibt Situationen, wenn eine Multi-Touch-Anwendung muss einzelne Finger zu verfolgen, während sie gleichzeitig auf dem Bildschirm zu verschieben. Eine typische Anwendung ist ein Finger-paint-Programm. Die Benutzer können mit einem einzelnen Finger gezeichnet werden soll, sondern auch gleichzeitig mit mehreren Fingern gezeichnet werden soll. Wie Ihre Anwendung mehrere Berührungsereignisse verarbeitet, muss es diese Finger unterscheiden.

Wenn ein Finger den Bildschirm berührt, iOS erstellt eine [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) Objekt für diese den Finger wieder an. Dieses Objekt bleibt gleich, wie Sie der Finger auf dem Bildschirm bewegt, und hebt dann aus dem Bildschirm, an welcher, den Stelle das Objekt freigegeben wurde. Zum Nachverfolgen Finger, ein Programm sollten durch die Speicherung `UITouch` -Objekts direkt. Verwenden sie stattdessen die [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) Eigenschaft vom Typ `IntPtr` zur eindeutigen Identifizierung dieser `UITouch` Objekte.

Ein Programm, das die einzelnen Finger nachverfolgt verwaltet fast immer ein Wörterbuch zum Nachverfolgen von Touch. Bei einem iOS-Programm, der Wörterbuchschlüssel ist der `Handle` Wert, der einen bestimmten Finger identifiziert. Der Wörterbuchwert hängt von der Anwendung ab. In der [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) Programm, jeden Finger Strich (von Touch freigeben) bezieht sich auf ein Objekt, alle Informationen, die erforderlich sind enthält, um mit diesem Finger gezeichnete Linie zu rendern. Das Programm definiert eine kleine `FingerPaintPolyline` Klasse für diesen Zweck:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Jede Polylinie hat eine Farbe, eine Strichbreite und ein iOS-Grafiken [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) Objekt ansammeln und Rendern von mehreren Punkten der Zeile, wie gezeichnet wird.


Alle übrigen der unten gezeigten Code befindet sich einem `UIView` Ableitung, die mit dem Namen `FingerPaintCanvasView`. Dass die Klasse ein Wörterbuch von Objekten des Typs verwaltet `FingerPaintPolyline` während der Zeit, die sie aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Dieses Wörterbuch kann die Ansicht, um schnell ermitteln der `FingerPaintPolyline` jeden Finger zugeordnete Informationen basierend auf der `Handle` Eigenschaft von der `UITouch` Objekt.

Die `FingerPaintCanvasView` Klasse verwaltet auch eine `List` Objekt für die Polylinien, die abgeschlossen wurden:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in der gleichen Reihenfolge an, dass sie gezeichnet wurden.

`FingerPaintCanvasView` überschreibt die fünf Methoden, die durch definiert `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Die verschiedenen `Touches` Außerkraftsetzungen kumuliert werden, die Punkte, die die Polylinien bilden.

Der [`Draw`] Außerkraftsetzung zeichnet abgeschlossenen Polylinien, und klicken Sie dann die Polylinien in Bearbeitung:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Jede der `Touches` Außerkraftsetzungen meldet möglicherweise die Aktionen von mehreren Fingern, angegeben durch eine oder mehrere `UITouch` in gespeicherten Objekte die `touches` Argument an die Methode. Die `TouchesBegan` überschreibt, durchlaufen diese Objekte. Für jede `UITouch` -Objekts können die Methode erstellt und initialisiert eine neue `FingerPaintPolyline` Objekts, z. B. das Speichern von den ursprünglichen Speicherort mit dem Finger abgerufenes die `LocationInView` Methode. Dies `FingerPaintPolyline` Objekt hinzugefügt wird die `InProgressPolylines` Wörterbuch mit den `Handle` Eigenschaft von der `UITouch` Objekt als Wörterbuchschlüssel:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

Die Methode abgeschlossen ist, durch den Aufruf `SetNeedsDisplay` zum Generieren von eines Aufrufs der `Draw` außer Kraft setzen und den Bildschirm zu aktualisieren.

Bewegen die Finger oder Finger auf dem Bildschirm die `View` ruft mehreren Aufrufen an die `TouchesMoved` außer Kraft setzen. Diese Außerkraftsetzung wird auf ähnliche Weise durchläuft die `UITouch` in gespeicherten Objekte die `touches` Argument und der Grafikpfad die aktuelle Position des Fingers hinzugefügt:

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

Die `touches` Auflistung enthält nur die `UITouch` Objekte für die Finger aufeinander, die seit dem letzten Aufruf von verschoben haben `TouchesBegan` oder `TouchesMoved`. Wenn Sie müssen `UITouch` entsprechende Objekte *alle* Finger derzeit mit dem Bildschirm verbunden, diese Informationen finden Sie über die `AllTouches` Eigenschaft von der `UIEvent` Argument an die Methode.

Die `TouchesEnded` Überschreibung hat zwei Aufträge. Es muss den letzten Punkt hinzufügen, der Grafikpfad und die Übertragung der `FingerPaintPolyline` -Objekt aus der `inProgressPolylines` Wörterbuch, das die `completedPolylines` Liste:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

Die `TouchesCancelled` Überschreibung erfolgt durch die einfach Aufgeben der `FingerPaintPolyline` Objekt aus dem Wörterbuch:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

Diese Verarbeitung zu, ermöglicht die [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) Programm zum Nachverfolgen von einzelnen Finger und zeichnen die Ergebnisse auf dem Bildschirm:

[![](touch-tracking-images/image01.png "Nachverfolgen von einzelnen Finger und zeichnen die Ergebnisse auf dem Bildschirm")](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und diese zu unterscheiden.



## <a name="related-links"></a>Verwandte Links

- [Entsprechende Xamarin Android-Handbuch](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
