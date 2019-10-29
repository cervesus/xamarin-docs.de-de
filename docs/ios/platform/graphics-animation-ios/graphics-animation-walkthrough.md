---
title: Verwenden von Kern Grafiken und Kern Animationen in xamarin. IOS
description: In diesem Artikel wird Schritt für Schritt erläutert, wie Sie eine Anwendung erstellen, die Core-Grafiken und die Kern Animation verwendet. Es zeigt, wie Sie auf dem Bildschirm als Reaktion auf Benutzereingaben zeichnen und ein Bild animieren, um einen Pfad zu besuchen.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: b35e88cfdc0bce321068951f1617885c90331c83
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032442"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Verwenden von Kern Grafiken und Kern Animationen in xamarin. IOS

In dieser exemplarischen Vorgehensweise zeichnen wir mithilfe von Kern Grafiken einen Pfad als Reaktion auf die Fingereingabe. Anschließend fügen wir eine `CALayer` mit einem Bild hinzu, das wir entlang des Pfads animieren werden.

Der folgende Screenshot zeigt die abgeschlossene Anwendung:

![](graphics-animation-walkthrough-images/00-final-app.png "The completed application")

Bevor wir beginnen, laden wir das *graphicsdemo* -Beispiel herunter, das diese Anleitung begleitet. Sie können [hier](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation) heruntergeladen werden und befinden sich im Verzeichnis **graphicswalkthrough** . Starten Sie das Projekt mit dem Namen **GraphicsDemo_starter** , indem Sie darauf doppelklicken, und öffnen Sie die `DemoView`-Klasse.

## <a name="drawing-a-path"></a>Zeichnen eines Pfads

1. Fügen Sie in `DemoView` der-Klasse eine `CGPath` Variable hinzu, und instanziieren Sie Sie im Konstruktor. Deklarieren Sie auch zwei `CGPoint` Variablen `initialPoint` und `latestPoint`, die wir verwenden werden, um den Berührungspunkt zu erfassen, von dem aus wir den Pfad erstellen:

    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;

        public DemoView ()
        {
            BackgroundColor = UIColor.White;

            path = new CGPath ();
        }
    }
    ```

2. Fügen Sie die folgenden using-Direktiven hinzu:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Überschreiben Sie als nächstes `TouchesBegan` und `TouchesMoved,`, und fügen Sie die folgenden Implementierungen hinzu, um den ersten Berührungspunkt und alle nachfolgenden Berührungspunkte zu erfassen:

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){

        base.TouchesBegan (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt){

        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` wird jedes Mal aufgerufen, wenn die Bewegung berührt wird, damit `Draw` bei der nächsten Durchlauf Schleife aufgerufen wird.

4. Wir fügen dem Pfad in der `Draw`-Methode Zeilen hinzu und verwenden eine rote, gestrichelte Linie, mit der gezeichnet wird. [Implementieren Sie `Draw`](~/ios/platform/graphics-animation-ios/core-graphics.md) mit dem unten gezeigten Code:

    ```csharp
    public override void Draw (CGRect rect){

        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){

                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();

                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }

                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });

                //add geometry to graphics context and draw it
                g.AddPath (path);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Wenn wir die Anwendung jetzt ausführen, können wir uns mit dem Zeichnen auf dem Bildschirm berühren, wie im folgenden Screenshot zu sehen:

![](graphics-animation-walkthrough-images/01-path.png "Drawing on the screen")

## <a name="animating-along-a-path"></a>Animieren entlang eines Pfades

Nun, da wir den Code implementiert haben, um Benutzern das Zeichnen des Pfads zu ermöglichen, fügen wir den Code hinzu, um eine Ebene entlang des gezeichneten Pfades zu animieren.

1. Fügen Sie zunächst der-Klasse eine [`CALayer`](~/ios/platform/graphics-animation-ios/core-animation.md) Variable hinzu, und erstellen Sie Sie im Konstruktor:

    ```csharp
    public class DemoView : UIView
        {
            …

            CALayer layer;

            public DemoView (){
                …

                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. Als Nächstes fügen wir die Ebene als Unterschicht der Ebene der Ansicht hinzu, wenn der Benutzer den Finger vom Bildschirm abhebt. Anschließend erstellen wir eine Keyframe-Animation mit dem Pfad und animieren die `Position`der Ebene.

    Um dies zu erreichen, müssen wir den `TouchesEnded` überschreiben und den folgenden Code hinzufügen:

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Führen Sie die Anwendung jetzt aus, und nach dem zeichnen wird eine Ebene mit einem Bild hinzugefügt und entlang des gezeichneten Pfades angezeigt:

![](graphics-animation-walkthrough-images/00-final-app.png "A layer with an image is added and travels along the drawn path")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir ein Beispiel durchlaufen, in dem Grafiken und Animations Konzepte miteinander verknüpft wurden. Zuerst haben wir gezeigt, wie Kern Grafiken verwendet werden, um einen Pfad in einer `UIView` als Reaktion auf die Benutzer Berührung zu zeichnen. Anschließend haben wir gezeigt, wie Sie mit der Core-Animation ein Bild an diesen Pfad weitergeben können.

## <a name="related-links"></a>Verwandte Links

- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grundlegende Animations Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
