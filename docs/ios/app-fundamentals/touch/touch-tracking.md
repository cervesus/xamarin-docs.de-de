---
title: Multitouch-Finger Verfolgung in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie einzelne Finger in Multitouch-Gesten in einer xamarin. IOS-App nachverfolgen können. Das Beispiel ist ein Beispiel für eine Fingerabdruck-App.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: b1ba548135cedd951d7f0a349f273b29182839d1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928678"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Multitouch-Finger Verfolgung in xamarin. IOS

_In diesem Dokument wird veranschaulicht, wie Berührungs Ereignisse von mehreren Fingern nachverfolgt werden._

Es gibt Zeiten, in denen eine Multitouch-Anwendung einzelne Finger nachverfolgen muss, während Sie sich gleichzeitig auf dem Bildschirm bewegen. Eine typische Anwendung ist ein Finger Paint-Programm. Sie möchten, dass der Benutzer mit einem einzelnen Finger zeichnen kann, aber gleichzeitig mit mehreren Fingern zeichnen kann. Wenn das Programm mehrere Berührungs Ereignisse verarbeitet, muss es zwischen diesen Fingern unterschieden werden.

Wenn ein Finger den Bildschirm zum ersten Mal berührt, erstellt IOS ein- [`UITouch`](xref:UIKit.UITouch) Objekt für diesen Finger. Dieses Objekt bleibt unverändert, wenn der Finger auf dem Bildschirm angezeigt wird, und dann wird der Bildschirm auf dem Bildschirm angezeigt, an dem das Objekt verworfen wird. Zum Nachverfolgen von Fingern sollte ein Programm vermeiden, dieses `UITouch` Objekt direkt zu speichern. Stattdessen kann die- [`Handle`](xref:Foundation.NSObject.Handle) Eigenschaft des-Typs verwendet werden `IntPtr` , um diese Objekte eindeutig zu identifizieren `UITouch` .

Fast immer verwaltet ein Programm, das einzelne Finger verfolgt, ein Wörterbuch für die Berührungs Überwachung. Bei einem IOS-Programm ist der Wörterbuch Schlüssel der `Handle` Wert, der einen bestimmten Finger identifiziert. Der Wörter Buchwert hängt von der Anwendung ab. Im [fingerpaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint) -Programm wird jeder Finger Strich (von Touch zu Release) einem Objekt zugeordnet, das alle Informationen enthält, die erforderlich sind, um die mit diesem Finger gezeichnete Linie zu renderieren. Das Programm definiert `FingerPaintPolyline` zu diesem Zweck eine kleine Klasse:

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

Jede Polylinie hat eine Farbe, eine Strichbreite und ein IOS-Grafik [`CGPath`](xref:CoreGraphics.CGPath) Objekt, um beim Zeichnen mehrere Punkte der Linie zu sammeln und zu Renderern.

Der Rest des unten gezeigten Codes ist in einer Ableitung mit dem `UIView` Namen enthalten `FingerPaintCanvasView` . Diese Klasse verwaltet ein Wörterbuch von Objekten des Typs `FingerPaintPolyline` , wenn Sie aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Dieses Wörterbuch ermöglicht der Ansicht, die `FingerPaintPolyline` jedem Finger zugeordneten Informationen schnell basierend auf der- `Handle` Eigenschaft des-Objekts abzurufen `UITouch` .

Die- `FingerPaintCanvasView` Klasse verwaltet auch ein- `List` Objekt für die ausgefüllten Polylinien:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in derselben Reihenfolge, in der Sie gezeichnet wurden.

`FingerPaintCanvasView`Überschreibt fünf durch definierte Methoden `View` :

- [`TouchesBegan`](xref:UIKit.UIResponder.TouchesBegan(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesMoved`](xref:UIKit.UIResponder.TouchesMoved(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesEnded`](xref:UIKit.UIResponder.TouchesEnded(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesCancelled`](xref:UIKit.UIResponder.TouchesCancelled(Foundation.NSSet,UIKit.UIEvent))
- [`Draw`](xref:UIKit.UIView.Draw(CoreGraphics.CGRect))

Die verschiedenen `Touches` über schreibungen sammeln die Punkte, aus denen die Polylinien bestehen.

Der [ `Draw` ]-überschreiben zeichnet die abgeschlossenen Polylinien und anschließend die in Bearbeitung befindlichen Polylinien:

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

Jede der außer Kraft setzungen `Touches` meldet möglicherweise die Aktionen mehrerer Finger, die durch ein oder mehrere-Objekte angegeben werden, `UITouch` die im- `touches` Argument der-Methode gespeichert sind. Die `TouchesBegan` außer Kraft setzungen-Schleife durch diese Objekte. Für jedes- `UITouch` Objekt erstellt und initialisiert die-Methode ein neues- `FingerPaintPolyline` Objekt, einschließlich der Speicherung der ursprünglichen Position des Fingers, der aus der-Methode abgerufen wurde `LocationInView` . Dieses `FingerPaintPolyline` Objekt wird dem Wörterbuch hinzugefügt, `InProgressPolylines` indem die- `Handle` Eigenschaft des- `UITouch` Objekts als Wörterbuch Schlüssel verwendet wird:

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

Die Methode wird beendet, indem aufgerufen wird `SetNeedsDisplay` , um einen Aufruf der außer Kraft Setzung zu generieren `Draw` und den Bildschirm zu aktualisieren.

Wenn der Finger oder die Finger auf dem Bildschirm bewegt werden, `View` Ruft mehrere Aufrufe der `TouchesMoved` außer Kraft Setzung ab. Diese Überschreibung durchläuft die `UITouch` im `touches` -Argument gespeicherten-Objekte und fügt die aktuelle Position des Fingers dem Grafik Pfad hinzu:

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

Die-Auflistung `touches` enthält nur die- `UITouch` Objekte für die Finger, die seit dem letzten-Rückruf von oder verschoben wurden `TouchesBegan` `TouchesMoved` . Wenn Sie jemals Objekte benötigen, die `UITouch` sich derzeit in Kontakt mit dem Bildschirm befinden, sind diese Informationen über die *all* - `AllTouches` Eigenschaft des- `UIEvent` Arguments für die-Methode verfügbar.

Die `TouchesEnded` außer Kraft Setzung umfasst zwei Aufträge. Er muss dem Grafik Pfad den letzten Punkt hinzufügen und das `FingerPaintPolyline` Objekt aus dem `inProgressPolylines` Wörterbuch in die Liste übertragen `completedPolylines` :

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

Die `TouchesCancelled` außer Kraft setzung wird durch einfaches verwerfen des- `FingerPaintPolyline` Objekts im Wörterbuch behandelt:

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

Diese Verarbeitung ermöglicht es dem [fingerpaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint) -Programm, einzelne Finger zu verfolgen und die Ergebnisse auf dem Bildschirm zu zeichnen:

[![Nachverfolgen von einzelnen Fingern und Zeichnen der Ergebnisse auf dem Bildschirm](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und voneinander unterscheiden können.

## <a name="related-links"></a>Ähnliche Themen

- [Entsprechendes xamarin Android-Handbuch](~/android/app-fundamentals/touch/touch-tracking.md)
- [Fingerpaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
