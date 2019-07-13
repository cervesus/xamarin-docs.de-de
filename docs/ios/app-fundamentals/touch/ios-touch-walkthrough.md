---
title: 'Exemplarische Vorgehensweise: Verwenden von Toucheingaben in Xamarin.iOS'
description: Dieses Dokument beschreibt, wie von toucheingaben in Xamarin.iOS-Anwendungen, die Erörterung von Beispiel-Touch-Interaktionen, Geste Erkennungen und Erkennungen von benutzerdefinierten Gesten behandelt wird.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: abcd89a6c4547680df0b96d235531312d3b21c52
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864335"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>Exemplarische Vorgehensweise: Verwenden von Toucheingaben in Xamarin.iOS

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie Code schreiben, die auf verschiedene Arten von Fingereingabe-Ereignissen reagiert. Jedes Beispiel befindet sich in einem separaten Fenster:

- [Tippen Sie auf Beispiele](#Touch_Samples) – wie Berührungsereignisse reagiert.
- [Erkennung Beispiele Geste](#Gesture_Recognizer_Samples) – Gewusst wie: Verwenden von integrierten Geste Erkennungen.
- [Beispiel für die benutzerdefinierte Stiftbewegungs-Erkennung](#Custom_Gesture_Recognizer) – wie Sie eine benutzerdefinierte stiftbewegungs-Erkennung zu erstellen.

Jeder Abschnitt enthält Anweisungen, um den Code von Grund auf neu zu schreiben.
Die [ab Beispielcode](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) enthält bereits einen vollständigen Storyboard und Menü Bildschirm:

 [![](ios-touch-walkthrough-images/image3.png "Das Beispiel enthält die Menü-Bildschirm")](ios-touch-walkthrough-images/image3.png#lightbox)

Führen Sie die Anweisungen unten, um das Storyboard Code hinzugefügt, und erfahren Sie mehr über die verschiedenen Typen von Touch-Ereignissen, die in iOS verfügbar. Öffnen Sie alternativ die [fertig gestellten Beispiel](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) alle arbeiten.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Touch-Beispiele

In diesem Beispiel werden einige der Multitouch-APIs veranschaulicht. Um die Implementierung von Touch-Ereignissen erforderlichen Code hinzufügen, gehen Sie wie folgt vor:


1. Öffnen Sie das Projekt **Touch_Start**. Führen Sie das Projekt zuerst, um sicherzustellen, dass alles in Ordnung ist und Toucheingabe der **Touch Beispiele** Schaltfläche. Einen Bildschirm ähnlich dem folgenden sollte angezeigt werden (obwohl keine der Schaltflächen funktionieren):
    
    [![](ios-touch-walkthrough-images/image4.png "Führen Sie mit der arbeitsfreien Schaltflächen-Beispiel-app")](ios-touch-walkthrough-images/image4.png#lightbox)


1. Bearbeiten Sie die Datei **TouchViewController.cs** und fügen Sie der Klasse die folgenden zwei Instanzvariablen `TouchViewController`:

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
    
    Diese Methode kann durch Prüfen auf eine `UITouch` Objekt aus, und führen Sie eine Aktion, die basierend auf, wo die Berührung aufgetreten ist, sofern vorhanden:

    * _In TouchImage_ – zeigt den Text `Touches Began` in eine Bezeichnung und ändern Sie das Image.
    * _In DoubleTouchImage_ – das Bild angezeigt, wenn die Bewegung Doppeltippen wurde.
    * _Klicken Sie im DragImage_ – ein Flag setzen, der angibt, dass die Berührung gestartet wurde. Die Methode `TouchesMoved` verwenden Sie dieses Flag wird bestimmt, ob `DragImage` auf dem Bildschirm verschoben werden soll, oder nicht, wie wir im nächsten Schritt aufgezeigt wird.

    Der obige Code befasst sich nur mit einzelnen Workflows, die noch kein Verhalten vorhanden ist, wenn der Benutzer den Finger auf dem Bildschirm verschoben wird. Um auf datenverschiebung zu reagieren, implementieren `TouchesMoved` wie im folgenden Code gezeigt:

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

    Diese Methode ruft eine `UITouch` -Objekt, und klicken Sie dann prüft, wo die Berührung aufgetreten ist. Wenn die Fingereingabe in aufgetreten ist `TouchImage`, klicken Sie dann den Text, die Workflows verschoben wird auf dem Bildschirm angezeigt. 

    Wenn `touchStartedInside` true ist, wissen wir, dass der Benutzer den Finger auf `DragImage` und wird verschoben wird. Der Code geht `DragImage` während der Benutzer den Finger auf dem Bildschirm bewegt.

1. Wir müssen die Situation zu behandeln, wenn der Benutzer seine Finger außerhalb des Bildschirms hebt oder iOS bricht ab, das touchereignis. Dazu implementieren wir `TouchesEnded` und `TouchesCancelled` wie unten dargestellt:

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

1. An diesem Punkt ist der Bildschirm berühren Beispiele abgeschlossen. Beachten Sie, wie der Bildschirm ändert, während Sie mit jedem der Images, interagieren wie im folgenden Screenshot gezeigt:
        
    [![](ios-touch-walkthrough-images/image4.png "Bildschirm der starten-app")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "Der Bildschirm, nachdem der Benutzer eine Schaltfläche gezogen hat.")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

## <a name="gesture-recognizer-samples"></a>Beispiele für die Erkennung von Bewegung

Die [vorherigen Abschnitt](#Touch_Samples) veranschaulicht, wie ein Objekt auf dem Bildschirm zu ziehen, mithilfe von touchereignissen.
In diesem Abschnitt werden die touchereignisse beseitigt werden und gezeigt, wie die folgende Aktion Erkennungen:

-  Die `UIPanGestureRecognizer` für das ein Bild auf dem Bildschirm ziehen.
-  Die `UITapGestureRecognizer` auf doppelte Tippen auf dem Bildschirm zu reagieren.

Wenn das Ausführen der [ab Beispielcode](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) und klicken Sie auf die **Geste Erkennung Beispiele** Schaltfläche, sollte den folgenden Bildschirm angezeigt:

 [![](ios-touch-walkthrough-images/image6.png "Durch Klicken auf die Schaltfläche mit den Gesten Erkennung Beispiele zeigt dieser Bildschirm")](ios-touch-walkthrough-images/image6.png#lightbox)

Um die Bewegung Erkennungen implementieren, gehen Sie wie folgt vor:


1. Bearbeiten Sie die Datei **GestureViewController.cs** und fügen Sie die folgenden Instanzvariable hinzu:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Wir benötigen diese Instanzvariable zum Nachverfolgen der vorherigen Position des Bilds.
Die Schwenken stiftbewegungs-Erkennung verwendet die `originalImageFrame` Wert, der den Offset erforderlich, um das Bild neu gezeichnet werden auf dem Bildschirm zu berechnen.

1. Fügen Sie die folgende Methode hinzu, mit dem Controller:

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

    Dieser Code instanziiert ein `UIPanGestureRecognizer` -Instanz und fügt es an eine Ansicht hinzu.
Beachten Sie, dass wir die Aktion in der Form der Methode ein Ziel zuweisen `HandleDrag` – diese Methode wird im nächsten Schritt bereitgestellt.

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

    Der obige Code wird Überprüfen Sie zunächst den Status der stiftbewegungs-Erkennung, und klicken Sie dann das Bild auf dem Bildschirm verschieben. Mit diesem Code werden kann der Controller jetzt unterstützen, das ein Bild auf dem Bildschirm ziehen.


1. Hinzufügen einer `UITapGestureRecognizer` das ändert sich das Bild in DoubleTouchImage angezeigt wird. Fügen Sie die folgende Methode der `GestureViewController` Controller:

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

    Dieser Code ähnelt der Code für die `UIPanGestureRecognizer` statt mit einem Delegaten für ein Ziel, die wir verwenden jedoch ein `Action`. 

1. Abschließend wir müssen wird ändern `ViewDidLoad` , damit sie die Methoden aufruft, wir gerade hinzugefügt haben. Ändern Sie ViewDidLoad, damit sie den folgenden Code ähnelt:

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


1. Führen Sie die Anwendung, und interagieren Sie mit den beiden Bildern.
Im folgende Screenshot ist ein Beispiel für diese Interaktionen:
    
    [![](ios-touch-walkthrough-images/image7.png "Dieser Screenshot zeigt eine Drag-Interaktion")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Benutzerdefinierte Stiftbewegungs-Erkennung

In diesem Abschnitt werden wir die Begriffe in den vorherigen Abschnitten zum Erstellen einer benutzerdefinierten stiftbewegungs-Erkennung gelten. Die benutzerdefinierte stiftbewegungs-Erkennung wird Unterklassen `UIGestureRecognizer`, und wird dann erkennen, wenn der Benutzer eine "V" zeichnet auf dem Bildschirm eine Bitmap zu wechseln. Im folgende Screenshot ist ein Beispiel für diesen Bildschirm:

 [![](ios-touch-walkthrough-images/image8.png "Die app erkennt, wenn der Benutzer eine \"V\" auf dem Bildschirm zeichnet.")](ios-touch-walkthrough-images/image8.png#lightbox)

Um eine benutzerdefinierte stiftbewegungs-Erkennung zu erstellen, gehen Sie wie folgt vor:


1. Fügen Sie eine neue Klasse zum-Projekt namens `CheckmarkGestureRecognizer`, und legen Sie ihn in den folgenden Code aussehen:

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

    Die Reset-Methode wird aufgerufen, wenn die `State` eigenschaftsänderungen entweder `Recognized` oder `Ended`. Dies ist die Zeit, in der benutzerdefinierten stiftbewegungs-Erkennung festlegen internen Zustand zurückzusetzen.
Die Klasse kann jetzt von vorn zu beginnen, die Interaktion des Benutzers mit der Anwendung beim nächsten und möchten, wiederholen Sie dann die Geste erkannt werden.



1. Nun, da wir eine benutzerdefinierte stiftbewegungs-Erkennung definiert haben (`CheckmarkGestureRecognizer`) bearbeiten Sie die **CustomGestureViewController.cs** Datei, und fügen Sie die folgenden zwei Instanzvariablen:

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Zum Instanziieren und unsere stiftbewegungs-Erkennung konfiguriert haben, fügen Sie die folgende Methode mit dem Controller aus:

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

1. Bearbeiten Sie `ViewDidLoad` , damit sie ruft `WireUpCheckmarkGestureRecognizer`, wie im folgenden Codeausschnitt gezeigt:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Führen Sie die Anwendung, und versuchen Sie eine "V" auf dem Bildschirm zu zeichnen. Angezeigt, dass die Änderung das Bild wird angezeigt, wie in den folgenden Screenshots gezeigt:
    
    [![](ios-touch-walkthrough-images/image9.png "Die Schaltfläche \"aktiviert\"")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "Die Schaltfläche \"deaktiviert\"")](ios-touch-walkthrough-images/image10.png#lightbox)



Die oben genannten drei Abschnitten veranschaulicht verschiedene Möglichkeiten auf berührungen reagieren Berührungsereignisse in iOS: mithilfe von Touch-Ereignissen, integrierte Geste Erkennungen, oder mit einer benutzerdefinierten stiftbewegungs-Erkennung.



## <a name="related-links"></a>Verwandte Links

- [iOS-Touch (Beispiel) starten](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS letzte Touch (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
