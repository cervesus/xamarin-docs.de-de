---
title: "Exemplarische Vorgehensweise – mit CoreGraphics und CoreAnimation"
description: "Dieser Artikel veranschaulicht Schritt für Schritt zum Erstellen einer Anwendung, die Core-Grafiken und Core Animation verwendet. Es wird gezeigt, wie auf dem Bildschirm als Antwort auf Benutzer Touch gezeichnet werden soll und wie ein Bild entlang eines Pfads animiert."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: c62601ff446c114e97e9d4c2ded3727d08220095
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="drawing-and-animating-along-a-path"></a>Zeichnen und animieren entlang eines Pfads

In dieser exemplarischen Vorgehensweise werden wir einen Pfad mit Graphics Core als Antwort auf Fingereingaben zeichnen. Anschließend werden wir fügen eine `CALayer` mit einem Bild, das wir entlang des Pfads animiert werden soll.

Der folgende Screenshot zeigt die fertigen Anwendung:

![](graphics-animation-walkthrough-images/00-final-app.png "Der fertigen Anwendung")

Vor dem Beginn des Downloads der *GraphicsDemo* Beispiel, in dem diese Anleitung begleitet. Kann heruntergeladen werden [hier](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) und befindet sich innerhalb der **GraphicsWalkthrough** Directory starten Sie das Projekt mit dem Namen **GraphicsDemo_starter** durch Doppelklicken auf, und Öffnen Sie die `DemoView` Klasse.

## <a name="drawing-a-path"></a>Zeichnen eines Pfads


1. In `DemoView` Hinzufügen einer `CGPath` Variable auf die Klasse und in den Konstruktor zu instanziieren. Deklarieren Sie auch zwei `CGPoint` Variablen `initialPoint` und `latestPoint`, dass wir zum Erfassen des Berührungspunkt, aus der erstellen wir den Pfad, verwenden:
    
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

3. Als Nächstes überschreiben `TouchesBegan` und `TouchesMoved,` und fügen Sie die folgenden Implementierungen, um der ersten Fingereingabe und jeder nachfolgende Touch Punkt zu erfassen:

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

    `SetNeedsDisplay` wird jedes Mal Fingereingaben, in der Reihenfolge für verschieben aufgerufen `Draw` auf die nächste Ausführung Lauf aufgerufen werden.

4. Wir fügen Zeilen an den Pfad in der `Draw` -Methode und verwendet eine rote gestrichelte Linie zu zeichnen. [Implementieren `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) mit den folgenden Code:

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

Wenn wir die Anwendung jetzt ausführen, können wir die zum Zeichnen auf dem Bildschirm berühren wie im folgenden Screenshot gezeigt:

![](graphics-animation-walkthrough-images/01-path.png "Zeichnen auf dem Bildschirm")

## <a name="animating-along-a-path"></a>Animieren entlang eines Pfads

Nun, da wir den Code, damit Benutzer auf den Pfad zeichnen können implementiert haben, fügen wir den Code, um eine Ebene entlang des Pfads gezeichneten animieren hinzu.

1. Fügen Sie zunächst eine [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) Variable auf die Klasse und erstellen Sie ihn im Konstruktor:

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

2. Als Nächstes fügen wir die Ebene als eine Unterebene Ebene für die Ansicht, wenn der Benutzer von ihren Finger vom Bildschirm anhebt. Klicken Sie dann, wir erstellen eine Keyframe-Animation unter Verwendung des Pfads Animieren der Ebene `Position`.

    Um dies zu erreichen, wir überschreiben müssen, die `TouchesEnded` und fügen Sie den folgenden Code hinzu:

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

3. Führen Sie die Anwendung jetzt und nach dem Zeichnen, eine Ebene mit einem Bild wird hinzugefügt und reist entlang des gezeichneten Pfads:

![](graphics-animation-walkthrough-images/00-final-app.png "Eine Ebene mit einem Bild wird hinzugefügt und verläuft entlang des Pfads gezeichneten")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel durchlaufen wir ein Beispiel, das Grafiken und Animationen Konzepte miteinander verbunden. Zunächst hat wir gezeigt, wie Grafiken Core verwenden, um einen Pfad zu zeichnen, einem `UIView` als Antwort auf Benutzer Touch. Klicken Sie dann wir wurde gezeigt, wie Core Animation verwenden, um ein Bild entlang dieses Pfads zu treffen.


## <a name="related-links"></a>Verwandte Links

- [Core-Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core-Grafiken](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Core Animation Rezepte](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
