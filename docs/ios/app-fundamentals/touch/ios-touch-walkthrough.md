---
title: 'Exemplarische Vorgehensweise: Verwenden von "berühren" in xamarin. IOS'
description: In diesem Dokument wird beschrieben, wie Sie die Toucheingabe in xamarin. IOS-Anwendungen behandeln und Beispiel-touchinteraktionen, Gesten erkenungen und benutzerdefinierte Gesten erkenungen erörtern.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 16e35d67f8fce33c8a0b21ddcd07df14fedf179b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725203"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>Exemplarische Vorgehensweise: Verwenden von "berühren" in xamarin. IOS

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie Code schreiben, der auf verschiedene Arten von Berührungs Ereignissen antwortet. Jedes Beispiel ist in einem separaten Bildschirm enthalten:

- [Berührungs Beispiele](#Touch_Samples) – Antworten auf Berührungs Ereignisse.
- [Beispiele für Gestenerkennung](#Gesture_Recognizer_Samples) – Verwenden integrierter Gesten Erkennungs Tools.
- [Beispiel für eine benutzerdefinierte Gestenerkennung](#Custom_Gesture_Recognizer) – Gewusst wie: Erstellen einer benutzerdefinierten Gestenerkennung.

Jeder Abschnitt enthält Anweisungen, um den Code von Grund auf neu zu schreiben.

Befolgen Sie die nachfolgenden Anweisungen, um dem Storyboard Code hinzuzufügen, und erfahren Sie mehr über die verschiedenen Typen von Berührungs Ereignissen, die in ios verfügbar sind. Öffnen Sie alternativ das [fertige Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final) , um alles zu sehen, was funktioniert.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Berührungs Beispiele

In diesem Beispiel veranschaulichen wir einige der Berührungs-APIs. Führen Sie die folgenden Schritte aus, um den Code hinzuzufügen, der zum Implementieren von Berührungs Ereignissen

1. Öffnen Sie das Projekt **Touch_Start**. Führen Sie zuerst das Projekt aus, um sicherzustellen, dass alles okay ist, und tippen Sie **auf die Schalt** Fläche mit den Es sollte ein Bildschirm angezeigt werden, der dem folgenden ähnelt (obwohl keine der Schaltflächen funktioniert):

    [![](ios-touch-walkthrough-images/image4.png "Sample app run with non-working buttons")](ios-touch-walkthrough-images/image4.png#lightbox)

1. Bearbeiten Sie die Datei **TouchViewController.cs** , und fügen Sie die folgenden beiden Instanzvariablen der-Klasse `TouchViewController`hinzu:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```

1. Implementieren Sie die `TouchesBegan`-Methode, wie im folgenden Code gezeigt:

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

    Diese Methode prüft, ob ein `UITouch` Objekt vorhanden ist. ist dies der Fall, führen Sie eine Aktion aus, die auf dem Speicherort der Fingereingabe basiert:

    - In _touchimage_ – zeigen Sie den Text `Touches Began` in einer Bezeichnung an, und ändern Sie das Bild.
    - _In doubletouchimage_ – ändern Sie das Bild, das angezeigt wird, wenn es sich um eine doppelte Abzweigung handelt.
    - Legen Sie _in DragImage_ – ein Flag fest, das angibt, dass der Fingerabdruck gestartet wurde. Die-Methode `TouchesMoved` verwendet dieses Flag, um zu bestimmen, ob `DragImage` auf dem Bildschirm verschoben werden soll, wie im nächsten Schritt zu sehen ist.

    Der obige Code befasst sich nur mit einzelnen Berührungen, es gibt immer noch kein Verhalten, wenn der Benutzer seinen Finger auf dem Bildschirm verschiebt. Implementieren Sie `TouchesMoved` wie im folgenden Code gezeigt, um auf eine Bewegung zu reagieren:

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

    Diese Methode ruft ein `UITouch` Objekt ab und überprüft dann, ob die Fingereingabe aufgetreten ist. Wenn die Fingereingabe in `TouchImage`aufgetreten ist, wird der Text berührt verschoben auf dem Bildschirm angezeigt.

    Wenn `touchStartedInside` true ist, wissen wir, dass der Benutzer seinen Finger auf `DragImage` hat und ihn bewegt. Der Code wird `DragImage` verschoben, wenn der Benutzer den Finger auf den Bildschirm verschiebt.

1. Wir müssen die Groß-/Kleinschreibung behandeln, wenn der Benutzer seinen Finger vom Bildschirm abhebt oder das Touch-Ereignis von IOS abgebrochen wird. Hierfür implementieren wir `TouchesEnded` und `TouchesCancelled`, wie unten dargestellt:

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

    Bei beiden Methoden wird das `touchStartedInside`-Flag auf false zurückgesetzt. `TouchesEnded` wird auch `TouchesEnded` auf dem Bildschirm angezeigt.

1. An diesem Punkt ist der Bildschirm mit den touchbeispielen fertiggestellt. Beachten Sie, wie sich der Bildschirm ändert, während Sie mit jedem der Bilder interagieren, wie im folgenden Screenshot zu sehen:

    [![](ios-touch-walkthrough-images/image4.png "The starting app screen")](ios-touch-walkthrough-images/image4.png#lightbox)

    [![](ios-touch-walkthrough-images/image5.png "The screen after the user drags a button")](ios-touch-walkthrough-images/image5.png#lightbox)

<a name="Gesture_Recognizer_Samples" />

## <a name="gesture-recognizer-samples"></a>Gesten Erkennungs Beispiele

Im [vorherigen Abschnitt](#Touch_Samples) wurde gezeigt, wie Sie ein Objekt mithilfe von touchereignissen auf den Bildschirm ziehen.
In diesem Abschnitt werden die Berührungs Ereignisse beseitigt und gezeigt, wie die folgenden Gesten Erkennungs Tools verwendet werden:

- Der `UIPanGestureRecognizer` zum Ziehen eines Bilds auf dem Bildschirm.
- Der `UITapGestureRecognizer`, um auf doppelte Abzweigungen auf dem Bildschirm zu antworten.

Führen Sie die folgenden Schritte aus, um Gesten Erkennungs Tools zu implementieren:

1. Bearbeiten Sie die Datei **GestureViewController.cs** , und fügen Sie die folgende Instanzvariable hinzu:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Wir benötigen diese Instanzvariable, um den vorherigen Speicherort des Bilds nachzuverfolgen.
Die Pan-Gestenerkennung verwendet den `originalImageFrame` Wert, um den Offset zu berechnen, der zum Neuzeichnen des Bilds auf dem Bildschirm erforderlich ist.

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

    Dieser Code instanziiert eine `UIPanGestureRecognizer`-Instanz und fügt Sie einer Ansicht hinzu.
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

1. Fügen Sie eine `UITapGestureRecognizer` hinzu, mit der das in doubletouchimage angezeigte Bild geändert wird. Fügen Sie dem `GestureViewController` Controller die folgende Methode hinzu:

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

    Dieser Code ähnelt dem Code für den `UIPanGestureRecognizer`, aber anstelle eines Delegaten für ein Ziel verwenden wir eine `Action`.

1. Als letztes müssen Sie `ViewDidLoad` ändern, sodass die soeben hinzugefügten Methoden aufgerufen werden. Ändern Sie viewDidLoad so, dass es dem folgenden Code ähnelt:

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

    Beachten Sie auch, dass wir den Wert `originalImageFrame`initialisieren.

1. Führen Sie die Anwendung aus, und interagieren Sie mit den beiden Bildern.
Der folgende Screenshot zeigt ein Beispiel für diese Interaktionen:

    [![](ios-touch-walkthrough-images/image7.png "This screenshot shows a drag interaction")](ios-touch-walkthrough-images/image7.png#lightbox)

<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Benutzerdefinierte Gestenerkennung

In diesem Abschnitt werden die Konzepte aus den vorherigen Abschnitten zum Erstellen einer benutzerdefinierten Gestenerkennung angewendet. Die benutzerdefinierte Gestenerkennung führt `UIGestureRecognizer`Unterklassen aus und erkennt, wenn der Benutzer ein "V" auf dem Bildschirm zeichnet, und schaltet eine Bitmap um. Der folgende Screenshot zeigt ein Beispiel für diesen Bildschirm:

 [![](ios-touch-walkthrough-images/image8.png "The app will recognize when the user draws a `V` on the screen")](ios-touch-walkthrough-images/image8.png#lightbox)

Führen Sie die folgenden Schritte aus, um eine benutzerdefinierte Gestenerkennung zu erstellen:

1. Fügen Sie dem Projekt eine neue Klasse mit dem Namen `CheckmarkGestureRecognizer`hinzu, und machen Sie Sie wie im folgenden Code aussehen:

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

    Die Reset-Methode wird aufgerufen, wenn die `State`-Eigenschaft entweder in `Recognized` oder `Ended`geändert wird. Dies ist der Zeitpunkt, an dem alle internen Status in der benutzerdefinierten Gestenerkennung zurückgesetzt werden.
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

1. Bearbeiten Sie `ViewDidLoad`, sodass `WireUpCheckmarkGestureRecognizer`aufgerufen wird, wie im folgenden Code Ausschnitt gezeigt:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Führen Sie die Anwendung aus, und versuchen Sie, ein "V" auf dem Bildschirm zu zeichnen. Es sollte angezeigt werden, dass das Bild geändert wird, wie in den folgenden Screenshots zu sehen:

    [![](ios-touch-walkthrough-images/image9.png "The button checked")](ios-touch-walkthrough-images/image9.png#lightbox)

    [![](ios-touch-walkthrough-images/image10.png "The button unchecked")](ios-touch-walkthrough-images/image10.png#lightbox)

In den obigen drei Abschnitten wurden verschiedene Möglichkeiten zur Reaktion auf touchereignisse in ios aufgezeigt: Verwenden von touchereignissen, integrierten Gesten Erkennungs Programmen oder mit einer benutzerdefinierten Gestenerkennung.

## <a name="related-links"></a>Verwandte Links

- [IOS-Berührungs Finale (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
