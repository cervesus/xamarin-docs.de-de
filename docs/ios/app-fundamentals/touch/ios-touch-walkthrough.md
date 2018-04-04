---
title: Exemplarische Vorgehensweise – Touch mit iOS
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 58066ef0071c8105658f0d766e8f038b2bd3ddf2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough--using-touch-in-ios"></a>Exemplarische Vorgehensweise – Touch mit iOS

Diese exemplarische Vorgehensweise veranschaulicht das Schreiben von Code, der auf verschiedene Arten von touchereignissen reagiert. Jedes Beispiel ist in einem separaten Fenster enthalten:

- [Touch-Beispiele](#Touch_Samples) – Berührungsereignisse reagieren.
- [Erkennung Beispiele Gestenhandler](#Gesture_Recognizer_Samples) – wie integrierte Geste Erkennung zu verwenden.
- [Benutzerdefinierte Geste Erkennungsmodul Beispiel](#Custom_Gesture_Recognizer) – zum Erstellen einer benutzerdefinierten Gestenhandler-Erkennung.

Jeder Abschnitt enthält Anweisungen, um den Code von Grund auf neu zu schreiben.
Die [starten Beispielcode](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) enthält bereits einen vollständige Menüs "" und "Storyboard-Bildschirm:

 [![](ios-touch-walkthrough-images/image3.png "Das Beispiel enthält Menü Bildschirm")](ios-touch-walkthrough-images/image3.png#lightbox)

Gehen Sie folgendermaßen vor, um das Storyboard Code hinzu, und erfahren Sie mehr über die verschiedenen Typen von touchereignissen verfügbar im iOS. Öffnen Sie alternativ die [fertig gestellten Beispiel](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) alles anzeigen.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Touch-Beispiele

In diesem Beispiel werden einige der Touch-APIs beispielmedienobjekt. Führen Sie diese Schritte aus, um den Code zum Implementieren von touchereignissen hinzufügen:


1. Öffnen Sie das Projekt **Touch_Start**. Zuerst führen Sie das Projekt, um sicherzustellen, dass alles ist zulässig und Toucheingabe der **berühren Beispiele** Schaltfläche. Einen Bildschirm ähnlich der folgenden sollte angezeigt werden (auch wenn keine der Schaltflächen funktioniert):
    
    [![](ios-touch-walkthrough-images/image4.png "Beispiel-app ausführen, arbeitsfreie-Schaltflächen")](ios-touch-walkthrough-images/image4.png#lightbox)


1. Bearbeiten Sie die Datei **TouchViewController.cs** und fügen Sie die folgenden zwei Variablen auf die Klasse `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. Implementieren der `TouchesBegan` Methode, wie im folgenden Code gezeigt:

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
    
    Diese Methode funktioniert durch Suchen nach einem `UITouch` Objekt, und führen Sie eine Aktion, die basierend auf einem aufgetreten ist, falls vorhanden:

    * _In TouchImage_ – zeigt den Text `Touches Began` in eine Bezeichnung und ändern Sie das Bild.
    * _In DoubleTouchImage_ – ändern Sie das Bild angezeigt, wenn die Bewegung Doppeltippen wurde.
    * _In DragImage_ – legen Sie ein Flag gibt an, dass die Fingereingabe gestartet wurde. Die Methode `TouchesMoved` verwenden Sie dieses Flag wird ermittelt, ob `DragImage` auf dem Bildschirm verschoben werden soll, oder nicht, wie wir im nächsten Schritt sehen muss.

    Der obige Code nur befasst sich mit einzelnen Fingereingaben, es ist noch keine Verhalten, wenn der Benutzer ihre Finger auf dem Bildschirm verschoben wird. Um auf datenverschiebung zu reagieren, implementieren `TouchesMoved` wie im folgenden Code gezeigt:

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

    Diese Methode ruft eine `UITouch` -Objekt, und klicken Sie dann prüft, in einem aufgetreten ist. Wenn in einem aufgetreten ist `TouchImage`, klicken Sie dann den Text Fingereingaben verschoben wird auf dem Bildschirm angezeigt. 

    Wenn `touchStartedInside` ist "true", dann bekannt ist, dass der Benutzer ihre Finger für hat `DragImage` und wird verschoben wird. Der Code geht dann `DragImage` während der Benutzer ihre Finger auf dem Bildschirm bewegt.

1. Wir müssen die Groß-/Kleinschreibung zu behandeln, wenn der Benutzer sein eigenes Finger außerhalb des Bildschirms hebt oder iOS bricht Touch-Ereignis ab. Dazu implementieren wir `TouchesEnded` und `TouchesCancelled` wie unten dargestellt:

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
    
    Beide Methoden setzt die `touchStartedInside` Flag auf "false". `TouchesEnded` Außerdem zeigt `TouchesEnded` auf dem Bildschirm.

1. An dieser Stelle ist der Bildschirm berühren Beispiele abgeschlossen. Beachten Sie, wie der Bildschirm ändert, während Sie die Bilder interagieren wie im folgenden Screenshot gezeigt:
        
    [![](ios-touch-walkthrough-images/image4.png "Bildschirm der ersten app")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "Dem Bildschirm, nachdem der Benutzer eine Schaltfläche bewegt.")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>Geste Erkennungsmodul-Beispiele

Die [vorherigen Abschnitt](#Touch_Samples) veranschaulicht, wie ein Objekt auf dem Bildschirm ziehen mithilfe der Berührungsereignisse.
In diesem Abschnitt werden die Berührungsereignisse beseitigt werden und veranschaulichen, wie die folgenden Geste Merkmale:

-  Die `UIPanGestureRecognizer` für ein Bild auf dem Bildschirm ziehen.
-  Die `UITapGestureRecognizer` So reagieren Sie auf doppelte Taps auf dem Bildschirm.

Wenn das Ausführen der [starten Beispielcode](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) , und klicken Sie auf die **Geste Erkennungsmodul Beispiele** Schaltfläche, sollte den folgenden Bildschirm angezeigt:

 [![](ios-touch-walkthrough-images/image6.png "Dieser Bildschirm zeigt durch Klicken auf die Schaltfläche mit den Gestenhandler Erkennungsmodul Beispiele")](ios-touch-walkthrough-images/image6.png#lightbox)

Führen Sie folgende Schritte Prüfer Gestenhandler zu implementieren:


1. Bearbeiten Sie die Datei **GestureViewController.cs** und fügen Sie die folgenden Instanzvariable hinzu:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Wir benötigen diese Instanzvariable zum Nachverfolgen der vorherigen Speicherort des Bilds.
Die Pan-Geste-Erkennung verwenden die `originalImageFrame` Wert Methodenpaar erforderlich, um das Bild auf dem Bildschirm neu gezeichnet werden.

1. Fügen Sie die folgende Methode mit dem Controller aus:

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

    Dieser Code instanziiert einen `UIPanGestureRecognizer` -Instanz und eine Ansicht hinzugefügt.
Beachten Sie, dass wir die Bewegung in Form der Methode ein Ziel zuweisen `HandleDrag` – diese Methode wird im nächsten Schritt bereitgestellt.

1. Um HandleDrag zu implementieren, fügen Sie den folgenden Code an den Controller aus:

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

    Der obige Code wird zuerst überprüfen Sie den Zustand der Gestenhandler-Erkennung und verschieben Sie das Bild auf dem Bildschirm. Mit diesem Code werden kann der Controller jetzt unterstützen das ein Bild auf dem Bildschirm ziehen.


1. Hinzufügen einer `UITapGestureRecognizer` ändert, die das Bild in DoubleTouchImage angezeigt wird. Fügen Sie die folgende Methode, die `GestureViewController` Controller:

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

    Dieser Code ist vergleichbar mit der Code für die `UIPanGestureRecognizer` statt mithilfe eines Delegaten für ein Ziel-Code wir verwenden jedoch ein `Action`. 

1. Im letzten Schritt erforderlich ist, ändern `ViewDidLoad` so, dass sie die Methoden aufruft, wir gerade hinzugefügt. Ändern Sie ViewDidLoad, sodass sie den folgenden Code ähnelt:

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

    Beachten Sie auch, dass wir den Wert der initialisieren `originalImageFrame`.


1. Führen Sie die Anwendung, und die beiden Images interagieren.
Der folgende Screenshot ist ein Beispiel für diese Aktivitäten:
    
    [![](ios-touch-walkthrough-images/image7.png "Diese bildschirmabbildung zeigt eine Drag-Aktivität")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Benutzerdefinierte Gestenhandler-Erkennung

In diesem Abschnitt werden wir die Konzepte in vorherigen Abschnitten zum Erstellen einer benutzerdefinierten Geste Erkennung anwenden. Die benutzerdefinierte Aktion Erkennung wird Unterklassen `UIGestureRecognizer`, und wird dann erkennen, wenn der Benutzer einen "V" zeichnet auf dem Bildschirm eine Bitmap zu wechseln. Der folgende Screenshot ist ein Beispiel dieses Bildschirms:

 [![](ios-touch-walkthrough-images/image8.png "Die app erkennt, wenn der Benutzer einen "V" auf dem Bildschirm zeichnet.")](ios-touch-walkthrough-images/image8.png#lightbox)

Führen Sie die Schritte zum Erstellen einer benutzerdefinierten Geste Erkennung:


1. Fügen Sie eine neue Klasse, um das Projekt mit dem Namen `CheckmarkGestureRecognizer`, und stellen sie den folgenden Code ähneln:

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

    Reset-Methode wird aufgerufen, wenn die `State` eigenschaftsänderungen entweder `Recognized` oder `Ended`. Dies ist die Zeit, legen Sie in der benutzerdefinierten Aktion Erkennung internen Status zurücksetzen.
Jetzt kann die Klasse starten neue beim nächsten des Benutzers mit der Anwendung Interaktion, und werden erneut versucht wird, erkennen die Aktion bereit.



1. Nun, dass wir eine Erkennung für die benutzerdefinierte Aktion definiert haben (`CheckmarkGestureRecognizer`) bearbeiten Sie die **CustomGestureViewController.cs** Datei, und fügen Sie die folgenden zwei Variablen hinzu:

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Zum Instanziieren und unsere Geste Erkennung konfiguriert haben, fügen Sie die folgende Methode mit dem Controller aus:

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

1. Bearbeiten Sie `ViewDidLoad` , damit er ruft `WireUpCheckmarkGestureRecognizer`, wie im folgenden Codeausschnitt gezeigt:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Führen Sie die Anwendung, und versuchen Sie, zeichnen einen "V" auf dem Bildschirm. Ändern, wird das Bild angezeigt werden sollte angezeigt werden, wie in den folgenden Screenshots dargestellt:
    
    [![](ios-touch-walkthrough-images/image9.png "Die Schaltfläche "ausgecheckt"")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "Die Schaltfläche "deaktiviert"")](ios-touch-walkthrough-images/image10.png#lightbox)



Der oben genannten drei Abschnitte wird unterschiedlich reagieren Berührungsereignisse in iOS: Berührungsereignisse, integrierte Geste Merkmale mit oder mit einer benutzerdefinierten Gestenhandler-Erkennung.



## <a name="related-links"></a>Verwandte Links

- [iOS berühren starten (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS endgültigen berühren (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
