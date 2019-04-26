---
title: TextKit in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie TextKit in Xamarin.iOS verwendet wird. TextKit bietet leistungsstarke Text Layout- und Renderingaufgaben-Funktionen.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: f08e37d17cc32e45232d54cc4a51bb48d7ec94b1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61184514"
---
# <a name="textkit-in-xamarinios"></a>TextKit in Xamarin.iOS

TextKit ist eine neue API, die leistungsstarke Text Layout- und Renderingaufgaben Features bietet. Es basiert auf der Low-Level-Kern-Text-Framework und ist viel einfacher zu verwenden als Core Text.

Um die Funktionen von TextKit für standard-Steuerelemente verfügbar zu machen, mehrere iOS Text-Steuerelemente neu implementiert wurden mit TextKit, einschließlich:

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>Architektur

TextKit bietet eine mehrschichtigen Architektur, die den Text-Speicher aus dem Layout und die Anzeige, einschließlich der folgenden Klassen trennt:

-  `NSTextContainer` – Stellt das Koordinatensystem und die Geometrie an, die zum Layout von Text verwendet wird.
-  `NSLayoutManager` : Ordnet Text durch Aktivieren von Text in Symbole. 
-  `NSTextStorage` – Enthält die Textdaten, als auch Batchaktualisierungen, Text-Eigenschaft behandelt. Alle BatchUpdates werden auf der Layout-Manager für die eigentliche Verarbeitung der Änderungen, z. B. das Layout neu zu berechnen und Neuzeichnen des Texts übergeben.


Diese drei Klassen werden auf einer Sicht angewendet, die Text rendert. Die integrierten Textverarbeitung Ansichten, z. B. `UITextView`, `UITextField`, und `UILabel` bereits haben sie festgelegt, aber Sie können das Erstellen und auf alle anwenden `UIView` -Instanz.

Die folgende Abbildung veranschaulicht diese Architektur:

 ![](textkit-images/textkitarch.png "Die folgende Abbildung zeigt die TextKit-Architektur")

## <a name="text-storage-and-attributes"></a>Textspeicher und Attribute

Die `NSTextStorage` -Klasse enthält den Text, der von einer Ansicht angezeigt wird. Es kommuniziert auch alle Änderungen, die Text - beispielsweise Änderungen an Zeichen oder deren Attributen – auf der Layout-Manager für die Anzeige. `NSTextStorage` erbt von `MSMutableAttributed` Zeichenfolge ist, sodass Änderungen an Text-Attribute in Batches zwischen angegeben werden `BeginEditing` und `EndEditing` aufrufen.

Beispielsweise der folgende Codeausschnitt gibt an, eine Änderung in den Vordergrund und Hintergrundfarben, und bestimmte Bereiche abzielt:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Nach dem `EndEditing` wird aufgerufen, werden die Änderungen an der Layout-Manager, die alle erforderlichen Layout und Rendering Berechnungen für den Text, der in der Ansicht angezeigt werden wiederum durchführt gesendet.

## <a name="layout-with-exclusion-path"></a>Layout mit Ausschlusspfad

TextKit unterstützt Layout auch für komplexe Szenarien ermöglicht, wie z. B. mit mehreren Spalten und fließende Text angegebenen Pfade namens *pfadausschluss*. Beim pfadausschluss. werden in den Textcontainer und angewendet, die bestimmt, die Geometrie das Textlayout, sodass des Texts, um die angegebenen Pfade fließen.

Ein Ausschlusspfad hinzufügen ist erforderlich, das die `ExclusionPaths` Eigenschaft auf der Layout-Manager. Durch Festlegen dieser Eigenschaft bewirkt, dass die Layout-Manager für ungültig erklärt das Textlayout, und Text der Ausschlusspfad fließen.

### <a name="exclusion-based-on-a-cgpath"></a>Basierend auf einer CGPath Ausschluss

Beachten Sie Folgendes `UITextView` Unterklasse-Implementierung:

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

Dieser Code fügt Unterstützung für das Zeichnen in der Textansicht, die wichtigste Grafik verwenden. Da die `UITextView` Klasse ist jetzt erstellt, um die TextKit für ihren Textrendering und Layout zu verwenden, es können alle Features der TextKit, z. B. das Festlegen von pfadausschluss.

> [!IMPORTANT]
> Dieses Beispiel Unterklassen `UITextView` zeichnen Unterstützung Aussehen verleihen. Erstellen von Unterklassen für `UITextView` ist nicht notwendig, die Funktionen der TextKit zu erhalten.



Nachdem der Benutzer in der Textansicht, die gezeichnet zeichnet `CGPath` gilt für eine `UIBezierPath` Instanz durch Festlegen der `UIBezierPath.CGPath` Eigenschaft:

```csharp
bezierPath.CGPath = exclusionPath;
```

Aktualisieren die folgende Codezeile macht das Textlayout rund um den Pfad zu aktualisieren:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Der folgende Screenshot veranschaulicht, wie das Textlayout ändert, um rund um den gezeichneten Pfad fließen zu lassen:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "In diesem Screenshot veranschaulicht, wie das Textlayout ändert um rund um den gezeichneten Pfad fließen zu lassen.")

Beachten Sie, dass die Layout-Manager `AllowsNonContiguousLayout` Eigenschaft in diesem Fall auf "false" festgelegt ist. Dies bewirkt, dass das Layout in allen Fällen neu berechnet werden, in dem der Text ändert. Festlegung auf "true" kann Leistung profitieren, indem Sie eine vollständige Layout-Aktualisierung, vor allem bei großen Dokumenten zu vermeiden. Festlegen von jedoch `AllowsNonContiguousLayout` auf "true" verhindert die Ausschlusspfad Aktualisieren des Layouts in einigen Fällen – z.B. wenn Text zur Laufzeit eingegeben wird, ohne eine nachfolgende Wagenrücklauf vor dem Pfad, der festgelegt wird.


## <a name="related-links"></a>Verwandte Links

- [Einführung in iOS 7 (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
