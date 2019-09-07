---
title: 'Exemplarische Vorgehensweise: Verwenden von "berühren" in xamarin. IOS'
description: In diesem Dokument wird beschrieben, wie Sie die Toucheingabe in xamarin. IOS-Anwendungen behandeln und Beispiel-touchinteraktionen, Gesten erkenungen und benutzerdefinierte Gesten erkenungen erörtern.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 022602c50386017b178672e20e3e352345feec0b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767206"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>Exemplarische Vorgehensweise: Verwenden von "berühren" in xamarin. IOS

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie Code schreiben, der auf verschiedene Arten von Berührungs Ereignissen antwortet. Jedes Beispiel ist in einem separaten Bildschirm enthalten:

- [Berührungs Beispiele](#Touch_Samples) – Antworten auf Berührungs Ereignisse.
- [Beispiele für Gestenerkennung](#Gesture_Recognizer_Samples) – Verwenden integrierter Gesten Erkennungs Tools.
- [Beispiel für eine benutzerdefinierte Gestenerkennung](#Custom_Gesture_Recognizer) – Gewusst wie: Erstellen einer benutzerdefinierten Gestenerkennung.

Jeder Abschnitt enthält Anweisungen, um den Code von Grund auf neu zu schreiben.
Der [Start Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start) enthält bereits ein vollständiges Storyboard und einen Menü Bildschirm:

 [![](ios-touch-walkthrough-images/image3.png "Der Menü Bildschirm \"Sample includes\"")](ios-touch-walkthrough-images/image3.png#lightbox)

Befolgen Sie die nachfolgenden Anweisungen, um dem Storyboard Code hinzuzufügen, und erfahren Sie mehr über die verschiedenen Typen von Berührungs Ereignissen, die in ios verfügbar sind. Öffnen Sie alternativ das [fertige Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final) , um alles zu sehen, was funktioniert.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Berührungs Beispiele

In diesem Beispiel veranschaulichen wir einige der Berührungs-APIs. Führen Sie die folgenden Schritte aus, um den Code hinzuzufügen, der zum Implementieren von Berührungs Ereignissen

1. Öffnen Sie das Projekt **Touch_Start**. Führen Sie zuerst das Projekt aus, um sicherzustellen, dass alles okay ist, und tippen Sie **auf die Schalt** Fläche mit den Es sollte ein Bildschirm angezeigt werden, der dem folgenden ähnelt (obwohl keine der Schaltflächen funktioniert):

    [![](ios-touch-walkthrough-images/image4.png "Beispiel-App mit nicht funktionierenden Schaltflächen ausführen")](ios-touch-walkthrough-images/image4.png#lightbox)

1. Bearbeiten Sie die Datei **TouchViewController.cs** , und fügen Sie der-Klasse `TouchViewController`die folgenden beiden Instanzvariablen hinzu:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```

1. Implementieren Sie `TouchesBegan` die-Methode, wie im folgenden Code gezeigt:

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    
    Diese Methode überprüft, ob ein- `UITouch` Objekt vorhanden ist. ist dies der Fall, führen Sie eine Aktion aus, die auf dem Speicherort der Fingereingabe basiert:

    - In _touchimage_ – zeigen Sie den `Touches Began` Text in einer Bezeichnung an, und ändern Sie das Bild.
    - _In doubletouchimage_ – ändern Sie das Bild, das angezeigt wird, wenn es sich um eine doppelte Abzweigung handelt.
    - Legen Sie _in DragImage_ – ein Flag fest, das angibt, dass der Fingerabdruck gestartet wurde. Die- `TouchesMoved` Methode verwendet dieses Flag, um zu `DragImage` bestimmen, ob auf dem Bildschirm verschoben werden soll oder nicht, wie im nächsten Schritt zu sehen ist.

    Der obige Code befasst sich nur mit einzelnen Berührungen, es gibt immer noch kein Verhalten, wenn der Benutzer seinen Finger auf dem Bildschirm verschiebt. Um auf Bewegung zu reagieren, `TouchesMoved` implementieren Sie, wie im folgenden Code gezeigt:

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Mit dieser Methode wird `UITouch` ein-Objekt abgerufen und dann überprüft, ob die Fingereingabe aufgetreten ist. Wenn die Fingereingabe in `TouchImage`aufgetreten ist, wird der Text berührt verschoben auf dem Bildschirm angezeigt. 

    Wenn `touchStartedInside` true ist, wissen wir, dass der Benutzer über `DragImage` seinen Finger verfügt und ihn bewegt. Der Code wird verschoben `DragImage` , wenn der Benutzer den Finger um den Bildschirm bewegt.

1. Wir müssen die Groß-/Kleinschreibung behandeln, wenn der Benutzer seinen Finger vom Bildschirm abhebt oder das Touch-Ereignis von IOS abgebrochen wird. Hierfür werden und `TouchesCancelled` wie unten dargestellt `TouchesEnded` implementiert:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```

    Beide Methoden setzen das `touchStartedInside` Flag auf false zurück. `TouchesEnded`wird auch auf `TouchesEnded` dem Bildschirm angezeigt.

1. An diesem Punkt ist der Bildschirm mit den touchbeispielen fertiggestellt. Beachten Sie, wie sich der Bildschirm ändert, während Sie mit jedem der Bilder interagieren, wie im folgenden Screenshot zu sehen:

    [![](ios-touch-walkthrough-images/image4.png "Der Bildschirm zum Starten der APP")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "Der Bildschirm, nachdem der Benutzer eine Schaltfläche gezogen hat")](ios-touch-walkthrough-images/image5.png#lightbox)

<a name="Gesture_Recognizer_Samples" />

## <a name="gesture-recognizer-samples"></a>Gesten Erkennungs Beispiele

Im [vorherigen Abschnitt](#Touch_Samples) wurde gezeigt, wie Sie ein Objekt mithilfe von touchereignissen auf den Bildschirm ziehen.
In diesem Abschnitt werden die Berührungs Ereignisse beseitigt und gezeigt, wie die folgenden Gesten Erkennungs Tools verwendet werden:

- Der `UIPanGestureRecognizer` zum Ziehen eines Bilds auf dem Bildschirm.
- Der `UITapGestureRecognizer` , der auf doppelte Seiten auf dem Bildschirm antwortet.

Wenn Sie den Code für das [Start Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start) ausführen und auf die Schaltfläche " **Gesten Erkennungs Beispiele** " klicken, sollte der folgende Bildschirm angezeigt werden:

 [![](ios-touch-walkthrough-images/image6.png "Klicken auf die Schaltfläche \"Gesten Erkennungs Beispiele\" zeigt diesen Bildschirm an")](ios-touch-walkthrough-images/image6.png#lightbox)

Führen Sie die folgenden Schritte aus, um Gesten Erkennungs Tools zu implementieren:

1. Bearbeiten Sie die Datei **GestureViewController.cs** , und fügen Sie die folgende Instanzvariable hinzu:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Wir benötigen diese Instanzvariable, um den vorherigen Speicherort des Bilds nachzuverfolgen.
Die Pan-Gestenerkennung verwendet den `originalImageFrame` -Wert, um den Offset zu berechnen, der zum Neuzeichnen des Bilds auf dem Bildschirm erforderlich ist.

1. Fügen Sie dem Controller die folgende Methode hinzu:

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Dieser Code instanziiert eine `UIPanGestureRecognizer` -Instanz und fügt Sie einer Ansicht hinzu.
Beachten Sie, dass wir der Geste in Form der Methode `HandleDrag` ein Ziel zuweisen – diese Methode wird im nächsten Schritt bereitgestellt.

1. Fügen Sie zum Implementieren von "dragdrag" dem Controller den folgenden Code hinzu:

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Der obige Code überprüft zuerst den Status der Gestenerkennung und verschiebt dann das Bild um den Bildschirm. Wenn dieser Code vorhanden ist, kann der Controller jetzt das Ziehen eines Bilds auf dem Bildschirm unterstützen.

1. Fügen Sie `UITapGestureRecognizer` einen hinzu, der das Bild ändert, das in doubletouchimage angezeigt wird. Fügen Sie dem `GestureViewController` Controller die folgende Methode hinzu:

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Dieser Code ähnelt dem Code für das `UIPanGestureRecognizer` , aber anstelle eines Delegaten für ein Ziel, das wir `Action`verwenden. 

1. Als letztes müssen wir ändern `ViewDidLoad` , sodass die soeben hinzugefügten Methoden aufgerufen werden. Ändern Sie viewDidLoad so, dass es dem folgenden Code ähnelt:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Beachten Sie auch, dass der Wert von `originalImageFrame`initialisiert wird.

1. Führen Sie die Anwendung aus, und interagieren Sie mit den beiden Bildern.
Der folgende Screenshot zeigt ein Beispiel für diese Interaktionen:
    
    [![](ios-touch-walkthrough-images/image7.png "Dieser Screenshot zeigt eine Drag-Interaktion.")](ios-touch-walkthrough-images/image7.png#lightbox)

<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Benutzerdefinierte Gestenerkennung

In diesem Abschnitt werden die Konzepte aus den vorherigen Abschnitten zum Erstellen einer benutzerdefinierten Gestenerkennung angewendet. Die benutzerdefinierte Gestenerkennung führt Unterklassen `UIGestureRecognizer`aus und erkennt, wenn der Benutzer ein "V" auf dem Bildschirm zeichnet, und schaltet eine Bitmap um. Der folgende Screenshot zeigt ein Beispiel für diesen Bildschirm:

 [![](ios-touch-walkthrough-images/image8.png "Die APP erkennt, wenn der Benutzer ein \"V\" auf dem Bildschirm zeichnet.")](ios-touch-walkthrough-images/image8.png#lightbox)

Führen Sie die folgenden Schritte aus, um eine benutzerdefinierte Gestenerkennung zu erstellen:

1. Fügen Sie dem Projekt eine neue Klasse mit `CheckmarkGestureRecognizer`dem Namen hinzu, und sehen Sie, wie der folgende Code aussieht:

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Die Reset-Methode wird aufgerufen, `State` wenn die-Eigenschaft `Recognized` entweder `Ended`in oder geändert wird. Dies ist der Zeitpunkt, an dem alle internen Status in der benutzerdefinierten Gestenerkennung zurückgesetzt werden.
Nun kann die Klasse bei der nächsten Interaktion des Benutzers mit der Anwendung neu gestartet werden, und Sie können versuchen, die Geste zu erkennen.

1. Nachdem Sie nun eine benutzerdefinierte Gestenerkennung (`CheckmarkGestureRecognizer`) definiert haben, bearbeiten Sie die Datei **CustomGestureViewController.cs** , und fügen Sie die folgenden beiden Instanzvariablen hinzu:

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Fügen Sie dem Controller die folgende Methode hinzu, um die Gestenerkennung zu instanziieren und zu konfigurieren:

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Bearbeiten `ViewDidLoad` Sie, sodass aufgerufen `WireUpCheckmarkGestureRecognizer`wird, wie im folgenden Code Ausschnitt gezeigt:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Führen Sie die Anwendung aus, und versuchen Sie, ein "V" auf dem Bildschirm zu zeichnen. Es sollte angezeigt werden, dass das Bild geändert wird, wie in den folgenden Screenshots zu sehen:
    
    [![](ios-touch-walkthrough-images/image9.png "Die Schaltfläche ist aktiviert")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "Die Schaltfläche ist deaktiviert")](ios-touch-walkthrough-images/image10.png#lightbox)

In den obigen drei Abschnitten wurden verschiedene Möglichkeiten zur Reaktion auf touchereignisse in ios aufgezeigt: Verwenden von touchereignissen, integrierten Gesten Erkennungs Programmen oder mit einer benutzerdefinierten Gestenerkennung.

## <a name="related-links"></a>Verwandte Links

- [IOS-Berührungs Start (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start)
- [IOS-Berührungs Finale (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
