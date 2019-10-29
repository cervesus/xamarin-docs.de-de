---
title: Textkit in xamarin. IOS
description: In diesem Dokument wird die Verwendung von textkit in xamarin. IOS beschrieben. Textkit bietet leistungsstarke Funktionen für das Layout und Rendering von Text.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 6c1dee464de1f7ba708b1f7d60affc1616e71ee9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031421"
---
# <a name="textkit-in-xamarinios"></a>Textkit in xamarin. IOS

Textkit ist eine neue API, die leistungsstarke Funktionen für das Layout und Rendering von Text Funktionen bietet. Es baut auf dem grundlegenden Text Framework auf niedriger Ebene auf, ist aber viel einfacher zu verwenden als Kerntext.

Um die Funktionen von textkit für Standard Steuerelemente verfügbar zu machen, wurden mehrere IOS-Text Steuerelemente für die Verwendung von textkit neu implementiert, einschließlich:

- UITextView
- UITextField
- UILabel

## <a name="architecture"></a>Architektur

Textkit bietet eine geschichtete Architektur, die den Text Speicher vom Layout und der Anzeige trennt, einschließlich der folgenden Klassen:

- `NSTextContainer` – stellt das Koordinatensystem und die Geometrie bereit, die zum Layouttext verwendet werden.
- `NSLayoutManager` – legt Text fest, indem Text in Glyphen verwandelt wird.
- `NSTextStorage` – enthält die Textdaten und verarbeitet die Aktualisierungen von Batch Texteigenschaften. Alle Batch Aktualisierungen werden dem LayoutManager für die tatsächliche Verarbeitung der Änderungen übergeben, z. b. Neuberechnen des Layouts und Neuzeichnen des Texts.

Diese drei Klassen werden auf eine Ansicht angewendet, die Text rendert. Die integrierten Text Behandlungs Sichten, wie z. b. `UITextView`, `UITextField`und `UILabel`, sind bereits festgelegt, aber Sie können Sie auch für jede `UIView` Instanz erstellen und anwenden.

Diese Architektur wird in der folgenden Abbildung veranschaulicht:

 ![](textkit-images/textkitarch.png "This figure illustrates the TextKit architecture")

## <a name="text-storage-and-attributes"></a>Text Speicher und Attribute

Die `NSTextStorage`-Klasse enthält den Text, der von einer Ansicht angezeigt wird. Außerdem werden alle Änderungen an den Text, z. b. Änderungen an Zeichen oder deren Attributen, an den Layout-Manager für die Anzeige übermittelt. `NSTextStorage` erbt von `MSMutableAttributed` Zeichenfolge, sodass Änderungen an Textattributen in Batches zwischen `BeginEditing` und `EndEditing` aufrufen festgelegt werden können.

Der folgende Code Ausschnitt gibt z. b. eine Änderung an Vordergrund-bzw. Hintergrundfarben an und richtet sich an bestimmte Bereiche:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Nachdem `EndEditing` aufgerufen wurde, werden die Änderungen an den Layout-Manager gesendet, der wiederum alle notwendigen Layout-und renderingberechnungen durchführt, damit der Text in der Ansicht angezeigt wird.

## <a name="layout-with-exclusion-path"></a>Layout mit Ausschluss Pfad

Textkit unterstützt auch das Layout und ermöglicht komplexe Szenarien, wie z. b. mehrspaltigen Text und fließenden Text um angegebene Pfade, die als *Ausschluss Pfade*bezeichnet werden. Ausschluss Pfade werden auf den Text Container angewendet, der die Geometrie des Textlayouts ändert, wodurch der Text um die angegebenen Pfade herum fließt.

Zum Hinzufügen eines Ausschluss Pfades muss die `ExclusionPaths`-Eigenschaft im LayoutManager festgelegt werden. Das Festlegen dieser Eigenschaft bewirkt, dass der LayoutManager das Text Layout für ungültig erklärt und den Text um den Ausschluss Pfad umgeht.

### <a name="exclusion-based-on-a-cgpath"></a>Ausschluss basierend auf einem cgpath

Beachten Sie die folgende `UITextView` Unterklassen Implementierung:

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

Dieser Code fügt Unterstützung für das Zeichnen in der Textansicht mithilfe von Kern Grafiken hinzu. Da die `UITextView`-Klasse jetzt erstellt wurde, um textkit für das Text Rendering und Layout zu verwenden, kann Sie alle Funktionen von textkit verwenden, z. b. das Festlegen von Ausschluss Pfaden.

> [!IMPORTANT]
> Diese Beispiel Unterklassen `UITextView`, die Unterstützung für Finger Eingaben hinzuzufügen. Das Unterklassen `UITextView` ist nicht erforderlich, um die Funktionen von textkit zu erhalten.

Nachdem der Benutzer die Textansicht gezeichnet hat, wird der gezeichnete `CGPath` auf eine `UIBezierPath` Instanz angewendet, indem die `UIBezierPath.CGPath`-Eigenschaft festgelegt wird:

```csharp
bezierPath.CGPath = exclusionPath;
```

Durch Aktualisieren der folgenden Codezeile wird das textlayoutupdate um den Pfad verschoben:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Der folgende Screenshot veranschaulicht, wie sich das Textlayout um den gezeichneten Pfad ändert:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")-->
![](textkit-images/exclusionpath2.png "This screenshot illustrates how the text layout changes to flow around the drawn path")

Beachten Sie, dass die `AllowsNonContiguousLayout`-Eigenschaft des Layout-Managers in diesem Fall auf false festgelegt ist. Dadurch wird das Layout für alle Fälle neu berechnet, in denen sich der Text ändert. Wenn diese Einstellung auf "true" festgelegt wird, kann die Leistung beeinträchtigt werden, indem eine Aktualisierung des vollständigen Layouts vermieden wird, insbesondere bei großen Dokumenten. Wenn Sie jedoch `AllowsNonContiguousLayout` auf "true" festlegen, wird verhindert, dass der Ausschluss Pfad das Layout in einigen Fällen aktualisiert, z. b. Wenn Text zur Laufzeit ohne einen nachfolgenden Wagen Rücklauf eingegeben wird, bevor der Pfad festgelegt wird.

## <a name="related-links"></a>Verwandte Links

- [Einführung zu IOS 7 (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
