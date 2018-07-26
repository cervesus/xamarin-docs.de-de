---
title: Verwenden von Core Grafiken und Core-Animationen in Xamarin.iOS
description: Dieser Artikel veranschaulicht Schritt für Schritt zum Erstellen einer Anwendung, die wichtigste Grafik und Core Animation verwendet. Es zeigt, wie auf dem Bildschirm als Reaktion auf die Toucheingabe Benutzers gezeichnet sowie wie Sie ein Bild, das entlang eines Pfads zu animieren.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cecfd7f3a9678f298af3ed547aa7b50a18238729
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242003"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Verwenden von Core Grafiken und Core-Animationen in Xamarin.iOS

In dieser exemplarischen Vorgehensweise werden wir einen Pfad mit Core Graphics als Reaktion auf die Toucheingabe zu zeichnen. Anschließend fügen wir eine `CALayer` mit einem Bild, das wir entlang des Pfads animiert werden soll.

Der folgende Screenshot zeigt die fertige Anwendung:

![](graphics-animation-walkthrough-images/00-final-app.png "Die fertige Anwendung")

Download zunächst die *GraphicsDemo* -Beispiel, das diesem Handbuch beigefügt ist. Es kann heruntergeladen werden [hier](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) und befindet sich im die **GraphicsWalkthrough** Verzeichnis starten Sie das Projekt mit dem Namen **GraphicsDemo_starter** durch Doppelklicken auf diese, und Öffnen Sie die `DemoView` Klasse.

## <a name="drawing-a-path"></a>Zeichnen eines Pfads


1. In `DemoView` Hinzufügen einer `CGPath` -Variable auf die Klasse, und instanziiere es im Konstruktor. Auch deklariert zwei `CGPoint` Variablen, `initialPoint` und `latestPoint`, die wir von Touch-Punkts zu erfassen mithilfe, von dem wir den Pfad zu erstellen:
    
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

2. Fügen Sie die folgenden using-Anweisungen hinzu:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Als Nächstes überschreiben `TouchesBegan` und `TouchesMoved,` und fügen Sie die folgenden Implementierungen zum Erfassen von anfänglichen Touch-Punkts und jedes nachfolgende Touch-Punkts:

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

    `SetNeedsDisplay` wird werden jedes Mal aufgerufen, Workflows zu, in der Reihenfolge für verschieben `Draw` im nächsten Durchlauf der Ausführung der Schleife aufgerufen werden.

4. Wir fügen Zeilen an den Pfad in der `Draw` -Methode und verwendet eine rote gestrichelte Linie zeichnen mit. [Implementieren `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) mit den folgenden Code:

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

![](graphics-animation-walkthrough-images/01-path.png "Auf dem Bildschirm zeichnen")

## <a name="animating-along-a-path"></a>Animieren von entlang eines Pfads

Nun, da wir den Code, damit Benutzer, die zum Zeichnen des Pfads implementiert haben, fügen Sie den Code, um eine Ebene entlang des gezeichneten Pfads zu animieren.

1. Fügen Sie zunächst eine [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) -Variable auf die Klasse und erstellen Sie ihn im Konstruktor:

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

2. Als Nächstes fügen wir die Ebene als eine Ebene der Ebene von der Ansicht, wenn der Benutzer Sie ihren Finger vom Bildschirm abhebt. Anschließend erstellen wir eine Keyframe-Animation, die unter Verwendung des Pfads, Animieren der Ebene des `Position`.

    Um dies zu erreichen, außer Kraft setzen wir müssen, die `TouchesEnded` und fügen Sie den folgenden Code hinzu:

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

3. Führen Sie die Anwendung jetzt und nach dem Zeichnen, eine Ebene mit einem Bild wird hinzugefügt und wird dahin weitergeleitet entlang des gezeichneten Pfads:

![](graphics-animation-walkthrough-images/00-final-app.png "Eine Ebene mit einem Bild wird hinzugefügt und wird dahin weitergeleitet entlang des gezeichneten Pfads")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel durchlaufen wir ein Beispiel, das Grafiken und Animationen Konzepte miteinander verbunden. Zunächst wir gezeigt, wie mit Core Graphics zeichnen Sie einen Pfad eine `UIView` als Reaktion auf die Toucheingabe des Benutzers. Klicken Sie dann wurde gezeigt, wie das Core Animation nutzen, um ein Bild, das entlang dieses Pfads zu erstellen.


## <a name="related-links"></a>Verwandte Links

- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Core Animation Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
