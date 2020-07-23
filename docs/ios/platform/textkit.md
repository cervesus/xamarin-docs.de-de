---
title: Textkit in xamarin. IOS
description: In diesem Dokument wird die Verwendung von textkit in xamarin. IOS beschrieben. Textkit bietet leistungsstarke Funktionen für das Layout und Rendering von Text.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 91db8ad0008afa29c732429c3304c24f4ab030a6
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935381"
---
# <a name="textkit-in-xamarinios"></a>Textkit in xamarin. IOS

Textkit ist eine neue API, die leistungsstarke Funktionen für das Layout und Rendering von Text Funktionen bietet. Es baut auf dem grundlegenden Text Framework auf niedriger Ebene auf, ist aber viel einfacher zu verwenden als Kerntext.

Um die Funktionen von textkit für Standard Steuerelemente verfügbar zu machen, wurden mehrere IOS-Text Steuerelemente für die Verwendung von textkit neu implementiert, einschließlich:

- UITextView
- UITextField
- UILabel

## <a name="architecture"></a>Aufbau

Textkit bietet eine geschichtete Architektur, die den Text Speicher vom Layout und der Anzeige trennt, einschließlich der folgenden Klassen:

- `NSTextContainer`– Stellt das Koordinatensystem und die Geometrie zur Verfügung, die zum Layouttext verwendet werden.
- `NSLayoutManager`– Legt Text fest, indem Text in Glyphen verwandelt wird.
- `NSTextStorage`– Enthält die Textdaten und verarbeitet Updates von Batch Texteigenschaften. Alle Batch Aktualisierungen werden dem LayoutManager für die tatsächliche Verarbeitung der Änderungen übergeben, z. b. Neuberechnen des Layouts und Neuzeichnen des Texts.

Diese drei Klassen werden auf eine Ansicht angewendet, die Text rendert. Die integrierten Text Behandlungs Sichten, wie z. b. `UITextView` , `UITextField` und, `UILabel` haben Sie bereits festgelegt, aber Sie können Sie auch erstellen und auf jede beliebige Instanz anwenden `UIView` .

Diese Architektur wird in der folgenden Abbildung veranschaulicht:

 ![Diese Abbildung veranschaulicht die textkit-Architektur.](textkit-images/textkitarch.png)

## <a name="text-storage-and-attributes"></a>Text Speicher und Attribute

Die- `NSTextStorage` Klasse enthält den Text, der von einer Ansicht angezeigt wird. Außerdem werden alle Änderungen an den Text, z. b. Änderungen an Zeichen oder deren Attributen, an den Layout-Manager für die Anzeige übermittelt. `NSTextStorage`erbt von der `MSMutableAttributed` Zeichenfolge, sodass Änderungen an Textattributen in Batches zwischen `BeginEditing` -und-aufrufen angegeben werden können `EndEditing` .

Der folgende Code Ausschnitt gibt z. b. eine Änderung an Vordergrund-bzw. Hintergrundfarben an und richtet sich an bestimmte Bereiche:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Nachdem `EndEditing` aufgerufen wurde, werden die Änderungen an den Layout-Manager gesendet, der wiederum alle notwendigen Layout-und Renderingerweiterungen für den Text ausführt, der in der Ansicht angezeigt werden soll.

## <a name="layout-with-exclusion-path"></a>Layout mit Ausschluss Pfad

Textkit unterstützt auch das Layout und ermöglicht komplexe Szenarien, wie z. b. mehrspaltigen Text und fließenden Text um angegebene Pfade, die als *Ausschluss Pfade*bezeichnet werden. Ausschluss Pfade werden auf den Text Container angewendet, der die Geometrie des Textlayouts ändert, wodurch der Text um die angegebenen Pfade herum fließt.

Zum Hinzufügen eines Ausschluss Pfades muss die- `ExclusionPaths` Eigenschaft für den Layout-Manager festgelegt werden. Das Festlegen dieser Eigenschaft bewirkt, dass der LayoutManager das Text Layout für ungültig erklärt und den Text um den Ausschluss Pfad umgeht.

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

Dieser Code fügt Unterstützung für das Zeichnen in der Textansicht mithilfe von Kern Grafiken hinzu. Da die `UITextView` -Klasse jetzt erstellt wurde, um textkit für das Text Rendering und Layout zu verwenden, kann Sie alle Funktionen von textkit verwenden, z. b. das Festlegen von Ausschluss Pfaden.

> [!IMPORTANT]
> Diese Beispiel Unterklassen `UITextView` , um die Unterstützung für Finger Eingaben hinzuzufügen. Die Unterklassen sind `UITextView` nicht erforderlich, um die Funktionen von textkit zu erhalten.

Nachdem der Benutzer die Textansicht gezeichnet hat, wird der gezeichnete `CGPath` auf eine-Instanz angewendet, `UIBezierPath` indem die-Eigenschaft festgelegt wird `UIBezierPath.CGPath` :

```csharp
bezierPath.CGPath = exclusionPath;
```

Durch Aktualisieren der folgenden Codezeile wird das textlayoutupdate um den Pfad verschoben:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Der folgende Screenshot veranschaulicht, wie sich das Textlayout um den gezeichneten Pfad ändert:

<!-- ![This screenshot illustrates how the text layout changes to flow around the drawn path](textkit-images/exclusionpath1.png)-->
![Dieser Screenshot veranschaulicht, wie sich das Textlayout um den gezeichneten Pfad ändert.](textkit-images/exclusionpath2.png)

Beachten Sie, dass die-Eigenschaft des Layout-Managers `AllowsNonContiguousLayout` in diesem Fall auf false festgelegt ist. Dadurch wird das Layout für alle Fälle neu berechnet, in denen sich der Text ändert. Wenn diese Einstellung auf "true" festgelegt wird, kann die Leistung beeinträchtigt werden, indem eine Aktualisierung des vollständigen Layouts vermieden wird, insbesondere bei großen Dokumenten. `AllowsNonContiguousLayout`Wenn Sie jedoch auf true festlegen, wird verhindert, dass der Ausschluss Pfad das Layout in einigen Fällen aktualisiert, z. b. Wenn Text zur Laufzeit ohne nachfolgende Wagen Rücklauf Eingabe eingegeben wird, bevor der Pfad festgelegt wird.

## <a name="related-links"></a>Ähnliche Themen

- [Einführung zu IOS 7 (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
