---
title: TextKit
description: "Der Text-Kit-API bietet leistungsfähige Text Layout und Rendering-Funktionen in Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 6b065c4b695edc3c88c5aed8c407c53fa6bbc578
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="text-kit"></a>Text-Kit

Text-Kit ist eine neue API, die leistungsstarke Text Layout und Rendering Funktionen bietet. Basiert auf niedriger Ebene Framework Core-Text, aber ist sehr viel einfacher zu verwenden als Core Text.

Damit die Funktionen des Text-Kit für Standardsteuerelemente verfügbar, wurden mehrere iOS Textsteuerelemente erneut implementiert, Text-Kit, einschließlich verwenden:

-  UITextView
-  UITextField
-  UILabel


## <a name="architecture"></a>Architektur

Text-Kit bietet eine mehrstufige Architektur, die eine den Text-Speicher aus dem Layout und die Anzeige Trennung, einschließlich der folgenden Klassen:

-  `NSTextContainer` – Stellt das Koordinatensystem und die Geometrie an, die zum Layout von Text verwendet wird.
-  `NSLayoutManager` – Angeordnet Text ein, durch das Aktivieren von Text in Glyphen. 
-  `NSTextStorage` – Enthält die Textdaten sowie BatchUpdates Text-Eigenschaft behandelt. Alle BatchUpdates werden an der Layout-Manager für die tatsächliche Verarbeitung der Änderungen, z. B. das Layout Neuberechnen und durch das Neuzeichnen erhalten des Texts übergeben.


Diese drei Klassen werden auf einer Sicht angewendet, die Text gerendert wird. Die integrierte Text Behandlung von Ansichten, z. B. `UITextView`, `UITextField`, und `UILabel` bereits festgelegt sind, aber Sie können das Erstellen und gelten sie für jede `UIView` -Instanz.

Die folgende Abbildung veranschaulicht diese Architektur:

 ![](textkit-images/textkitarch.png "Diese Abbildung zeigt die Text-Kit-Architektur")

## <a name="text-storage-and-attributes"></a>Text-Speicher und Attribute

Die `NSTextStorage` Klasse enthält den Text, der von einer Ansicht angezeigt wird. Es kommuniziert auch Änderungen in den Text - beispielsweise Änderungen an Zeichen oder deren Attributen – dem Layout-Manager für die Anzeige. `NSTextStorage` erbt von `MSMutableAttributed` Zeichenfolge ist, dass Änderungen Textattribute in Batches zwischen angegeben werden `BeginEditing` und `EndEditing` aufrufen.

Z. B. der folgende Codeausschnitt gibt eine Änderung in den Vordergrund und Hintergrund Farben versehen, und bestimmte Bereiche abzielen:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Nach dem `EndEditing` wird aufgerufen, werden die Änderungen an der Layout-Manager, die wiederum durchführt, die alle erforderlichen Layout und Rendering Berechnungen für den Text, der in der Ansicht angezeigt werden gesendet.

## <a name="layout-with-exclusion-path"></a>Layout mit Ausschlusspfad

Text-Kit auch Layout unterstützt und können für komplexe Szenarios wie z. B. mit mehreren Spalten und fließende Text angegebenen Pfade aufgerufen *Ausschluss Pfade*. Ausschluss-Pfade werden für des Textcontainers angewendet, die die Geometrie des Textlayouts, verursacht des Texts, um die angegebenen Pfade fließen ändert.

Das Hinzufügen von ein Ausschlusspfad Einstellung erfordert die `ExclusionPaths` Eigenschaft für den Layout-Manager. Durch Festlegen dieser Eigenschaft führt dazu, dass den Layout-Manager für ungültig erklärt das Textlayout und der Text der Ausschlusspfad fließen.

### <a name="exclusion-based-on-a-cgpath"></a>Ausschluss basierend auf einer CGPath

Beachten Sie Folgendes `UITextView` Unterklasse Implementierung:

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Dieser Code fügt Unterstützung für das Zeichnen auf Textansicht mit Graphics Core. Da die `UITextView` Klasse ist jetzt erstellt, um Text-Kit für die Textrendering und Layout verwenden, können sie alle Funktionen des Text-Kits, z. B. das Festlegen der Ausschluss Pfade verwenden.

> [!IMPORTANT]
>   Hinweis: Diese Beispiel-Unterklassen `UITextView` hinzuzufügende Touch-Unterstützung zu zeichnen. Erstellen von Unterklassen für `UITextView` ist nicht erforderlich, die Funktionen des Text-Kits zu erhalten.



Nachdem der Benutzer auf die Textansicht der gezeichneten zeichnet `CGPath` wird angewendet, um eine `UIBezierPath` Instanz durch Festlegen der `UIBezierPath.CGPath` Eigenschaft:

```csharp
bezierPath.CGPath = exclusionPath;
```

Aktualisieren die folgende Codezeile macht das Textlayout rund um den Pfad zu aktualisieren:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Der folgende Screenshot veranschaulicht, wie das Textlayout ändert um rund um den gezeichneten Pfad auszuführen:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "Diesem Screenshot veranschaulicht, wie das Textlayout ändert um rund um den gezeichneten Pfad fließen zu lassen.")

Beachten Sie, dass des Layout-Managers `AllowsNonContiguousLayout` Eigenschaft in diesem Fall auf "false" festgelegt ist. Dies bewirkt, dass das Layout in allen Fällen neu berechnet werden, in dem der Text ändert. Diese Einstellung auf "true" festgelegt möglicherweise Leistung profitieren, indem Sie eine vollständige Layout-Aktualisierung, insbesondere bei großen Dokumenten zu vermeiden. Allerdings festlegen `AllowsNonContiguousLayout` auf "true" würde verhindern, dass der Ausschlusspfad aktualisieren das Layout unter bestimmten Umständen – z. B. wenn Text zur Laufzeit eingegeben wird, ohne nachfolgende Wagenrücklauf vor dem Pfad, der festgelegt wird.


## <a name="related-links"></a>Verwandte Links

- [Einführung in die iOS 7 (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 Übersicht über die Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
