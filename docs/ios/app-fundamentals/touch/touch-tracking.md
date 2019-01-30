---
title: Multitouch-Finger, die Nachrichtennachverfolgung in Xamarin.iOS
description: Dieses Dokument beschreibt, wie einzelner Finger in die Multitouch-Gesten in einer Xamarin.iOS-app nachverfolgt wird. Es basiert auf einer multitoucheingaben Beispiel-app.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 09e895714cb4bbe241e4e14facaaee52079d55d9
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233185"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Multitouch-Finger, die Nachrichtennachverfolgung in Xamarin.iOS

_In diesem Dokument wird veranschaulicht, wie zum Nachverfolgen von Touch-Ereignissen aus mehreren Fingern_

Es gibt Situationen, wenn eine Multi-Touch-Anwendung muss einzelner Finger zu verfolgen, während sie gleichzeitig auf dem Bildschirm zu verschieben. Eine typische Anwendung ist ein Programm Finger-paint. Sie möchten die Benutzer mit einem einzelnen Finger gezeichnet werden soll, sondern auch gleichzeitig mit mehreren Fingern zeichnen kann. Während das Programm mehrere touchereignisse verarbeitet, muss diese Finger zu unterscheiden.

Wenn ein Finger den Bildschirm zuerst berührt, iOS erstellt eine [ `UITouch` ](xref:UIKit.UITouch) Objekt für diese Finger. Dieses Objekt bleibt unverändert, da der Finger auf dem Bildschirm bewegt, und hebt dann wählen Sie im Fenster an diesem Punkt das Objekt verworfen wird. Um zu verfolgen Finger, ein Programm sollten durch die Speicherung `UITouch` direkt. Sie können stattdessen die [ `Handle` ](xref:Foundation.NSObject.Handle) Eigenschaft vom Typ `IntPtr` zum eindeutigen Identifizieren dieser `UITouch` Objekte.

Ein Programm, das einzelner Finger verfolgt wird fast immer ein Wörterbuch zum Nachverfolgen von Touch verwaltet. Bei einem iOS-Programm, der Wörterbuchschlüssel ist der `Handle` Wert, der einen bestimmten Finger identifiziert. Der Wörterbuchwert hängt von der Anwendung ab. In der [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) Programm Strichfarbe Finger (von Toucheingabe freigeben) bezieht sich auf ein Objekt, das alle Informationen, die zum Rendern mit Fingers gezeichneten Linie enthält. Das Programm definiert ein kleines `FingerPaintPolyline` -Klasse für diesen Zweck:

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

Jeder Polylinie verfügt über eine Farbe, eine Strichbreite und ein iOS-Grafiken [ `CGPath` ](xref:CoreGraphics.CGPath) Objekt, das Sammeln und mehrere Punkte der Linie zu rendern, wie er gerade gezeichnet wird.


Die restlichen den folgenden Code ist Bestandteil einer `UIView` -Ableitung namens `FingerPaintCanvasView`. Dass die Klasse ein Wörterbuch von Objekten des Typs verwaltet `FingerPaintPolyline` während des Zeitraums, der diese aktiv von einem oder mehreren Fingern gezeichnet werden:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Dieses Wörterbuch kann die Ansicht schnell Abrufen der `FingerPaintPolyline` jeden Finger zugeordneten Informationen auf der Grundlage der `Handle` Eigenschaft der `UITouch` Objekt.

Die `FingerPaintCanvasView` Klasse verwaltet auch eine `List` -Objekt für die Polylinien, die abgeschlossen wurden:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die Objekte in diesem `List` befinden sich in der gleichen Reihenfolge an, dass sie gezeichnet wurden.

`FingerPaintCanvasView` fünf definierte Methoden überschreibt `View`:

- [`TouchesBegan`](xref:UIKit.UIResponder.TouchesBegan(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesMoved`](xref:UIKit.UIResponder.TouchesMoved(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesEnded`](xref:UIKit.UIResponder.TouchesEnded(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesCancelled`](xref:UIKit.UIResponder.TouchesCancelled(Foundation.NSSet,UIKit.UIEvent))
- [`Draw`](xref:UIKit.UIView.Draw(CoreGraphics.CGRect))

Die verschiedenen `Touches` , überschreibungen, sammeln Sie die Punkte, die die Polylinien bilden.

Die [`Draw`] außer Kraft setzen zeichnet die abgeschlossenen Polylinien, und klicken Sie dann die Polylinien in Bearbeitung:

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

Jede der `Touches` Außerkraftsetzungen meldet möglicherweise die Aktionen mehrerer Finger, die von einer oder mehreren angegebenen `UITouch` im gespeicherten Objekte die `touches` Argument der Methode. Die `TouchesBegan` Außerkraftsetzungen, durchlaufen diese Objekte. Für jede `UITouch` -Objekts die Methode erstellt und initialisiert eine neue `FingerPaintPolyline` -Objekts, einschließlich, speichern die ursprüngliche Position des Fingers abgerufen, die von der `LocationInView` Methode. Dies `FingerPaintPolyline` Objekt hinzugefügt wird die `InProgressPolylines` Wörterbuch mit den `Handle` Eigenschaft der `UITouch` -Objekt als ein Dictionary-Schlüssel:

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

Die Methode endet mit dem Aufruf von `SetNeedsDisplay` zum Generieren von eines Aufrufs der `Draw` außer Kraft setzen und den Bildschirm zu aktualisieren.

Während die Finger oder der Finger auf dem Bildschirm bewegen der `View` ruft mehrere Aufrufe an die `TouchesMoved` außer Kraft setzen. Diese Außerkraftsetzung wird auf ähnliche Weise durchläuft die `UITouch` im gespeicherten Objekte die `touches` Argument und fügt der aktuellen Position des Fingers der Grafikpfad hinzu:

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

Die `touches` Auflistung enthält nur die `UITouch` Objekte für die Finger, die seit dem letzten Aufruf von verschoben wurden `TouchesBegan` oder `TouchesMoved`. Sollten Sie jemals `UITouch` entsprechende Objekte *alle* die Finger den Bildschirm berühren derzeit, diese Information steht über die `AllTouches` Eigenschaft der `UIEvent` Argument der Methode.

Die `TouchesEnded` Überschreibung hat zwei Aufträge. Sie müssen die Grafikpfad und die Übertragung den letzten Punkt hinzufügen der `FingerPaintPolyline` -Objekt aus der `inProgressPolylines` Wörterbuch, das die `completedPolylines` Liste:

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

Die `TouchesCancelled` Überschreibung erfolgt, indem Sie einfach Abbrechen der `FingerPaintPolyline` Objekt aus dem Wörterbuch:

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

Diese Verarbeitung vollständig zu entfernen und ermöglicht die [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) Programm einzelner Finger nachverfolgen und die Ergebnisse auf dem Bildschirm zeichnen:

[![](touch-tracking-images/image01.png "Einzelner Finger verfolgt, und zeichnen die Ergebnisse auf dem Bildschirm")](touch-tracking-images/image01.png#lightbox)

Sie haben nun gesehen, wie Sie einzelne Finger auf dem Bildschirm nachverfolgen und diese zu unterscheiden.



## <a name="related-links"></a>Verwandte Links

- [Entsprechende Xamarin Android-Leitfaden](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
