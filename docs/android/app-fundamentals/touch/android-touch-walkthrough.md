---
title: 'Exemplarische Vorgehensweise: Verwenden von Touchscreen in Android'
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: 78878e62a36e3f6dd6ca3c7fcfb6413da4f0e0f9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644126"
---
# <a name="walkthrough---using-touch-in-android"></a>Exemplarische Vorgehensweise: Verwenden von Touchscreen in Android

Wir sehen uns nun an, wie die Konzepte aus dem vorherigen Abschnitt in einer funktionierenden Anwendung verwendet werden. Wir erstellen eine Anwendung mit vier Aktivitäten. Bei der ersten Aktivität handelt es sich um ein Menü oder ein Schalttafel, das die anderen Aktivitäten startet, um die verschiedenen APIs zu veranschaulichen. Der folgende Screenshot zeigt die Hauptaktivität:

[![Screenshot mit der Touchscreen-Schaltfläche](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

In der ersten Aktivität (Fingereingabe Beispiel) wird gezeigt, wie Ereignishandler zum Berühren der Ansichten verwendet werden. Die Gesten Erkennungs Aktivität veranschaulicht, wie Ereignisse unterteilt `Android.View.Views` und behandelt werden. Außerdem wird gezeigt, wie Sie die gepingergesten behandeln können. Die dritte und letzte Aktivität, **benutzerdefinierte Geste**, zeigt, wie benutzerdefinierte Gesten verwendet werden. Um die Vorgehensweise und das Auffangen der Dinge zu vereinfachen, wird diese exemplarische Vorgehensweise in Abschnitte unterteilt, wobei sich jeder Abschnitt auf eine der Aktivitäten konzentriert.

## <a name="touch-sample-activity"></a>Sample-Beispiel Aktivität

-   Öffnen Sie den Projekt- **touchwalkthrough\_-Start**. Die **mainactivity** ist &ndash; so festgelegt, dass Sie das Fingerabdruck Verhalten in der Aktivität implementieren kann. Wenn Sie die Anwendung ausführen und auf Fingereingabe **Beispiel**klicken, sollte die folgende Aktivität gestartet werden:

    [![Screenshot: Aktivität mit angezeigter Berührungs Anzeige](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Nachdem Sie nun bestätigt haben, dass die Aktivität gestartet wird, öffnen Sie die Datei **TouchActivity.cs** , und fügen Sie `Touch` einen Handler für `ImageView`das-Ereignis von hinzu:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Fügen Sie als nächstes die folgende Methode zu **TouchActivity.cs**hinzu:

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
        {
            case MotionEventActions.Down:
            case MotionEventActions.Move:
            message = "Touch Begins";
            break;

            case MotionEventActions.Up:
            message = "Touch Ends";
            break;

            default:
            message = string.Empty;
            break;
        }

        _touchInfoTextView.Text = message;
    }
    ```

Beachten Sie im obigen Code, dass die `Move` -Aktion und die- `Down` Aktion als identisch behandelt werden. Der Grund hierfür ist, dass der Benutzer den Finger möglicherweise nicht aus `ImageView`dem abhebt, da er sich möglicherweise ändert oder der vom Benutzer ausgeübte Druck geändert wird. Diese Änderungs Typen generieren eine `Move` -Aktion.

Jedes Mal, wenn der Benutzer `ImageView`das berührt `Touch` , wird das-Ereignis ausgelöst, und der Handler zeigt den Beginn der Nachrichten **Berührung** auf dem Bildschirm an, wie im folgenden Screenshot gezeigt:

[![Screenshot der Aktivität mit Berührungs Beginn](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Solange der Benutzer die `ImageView` **berührt, wird** der Fingerabdruck in der `TextView`angezeigt. Wenn der Benutzer das `ImageView`nicht mehr berührt, wird der Nachrichten **Berührungs Ende** im `TextView`angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot der Aktivität mit Berührungs enden](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Gesten Erkennungs Aktivität

Nun können Sie die Gesten Erkennungs Aktivität implementieren. Diese Aktivität zeigt, wie Sie eine Ansicht um den Bildschirm ziehen und eine Möglichkeit zum Implementieren von "Verkleinern-zu-Zoom" veranschaulichen.

-   Fügen Sie der Anwendung eine neue-Aktivität `GestureRecognizer`mit dem Namen hinzu.
    Bearbeiten Sie den Code für diese Aktivität, sodass Sie dem folgenden Code ähnelt:

    ```csharp
    public class GestureRecognizerActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            View v = new GestureRecognizerView(this);
            SetContentView(v);
        }
    }
    ```

-   Fügen Sie dem Projekt eine neue Android-Ansicht hinzu, und `GestureRecognizerView`benennen Sie Sie. Fügen Sie dieser Klasse die folgenden Variablen hinzu:

    ```csharp
    private static readonly int InvalidPointerId = -1;

    private readonly Drawable _icon;
    private readonly ScaleGestureDetector _scaleDetector;

    private int _activePointerId = InvalidPointerId;
    private float _lastTouchX;
    private float _lastTouchY;
    private float _posX;
    private float _posY;
    private float _scaleFactor = 1.0f;
    ```

-   Fügen Sie den folgenden Konstruktor `GestureRecognizerView`hinzu. Dieser Konstruktor fügt der Aktivität `ImageView` einen hinzu. An diesem Punkt wird der Code noch nicht kompiliert &ndash; . Wir müssen die Klasse `MyScaleListener` erstellen, die beim Fixieren des Benutzers die `ImageView` Größe der ändert:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Um das Bild in unserer Aktivität zu zeichnen, müssen wir die `OnDraw` -Methode der Ansichts Klasse überschreiben, wie im folgenden Code Ausschnitt gezeigt. Mit diesem Code wird der `ImageView` an die von `_posX` und angegebene Position `_posY` verschoben, und die Größe des Bilds wird entsprechend dem Skalierungsfaktor angepasst:

    ```csharp
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        canvas.Save();
        canvas.Translate(_posX, _posY);
        canvas.Scale(_scaleFactor, _scaleFactor);
        _icon.Draw(canvas);
        canvas.Restore();
    }
    ```

-   Als nächstes müssen Sie die Instanzvariable `_scaleFactor` aktualisieren, wenn der Benutzer die `ImageView`pingt. Wir fügen eine Klasse mit dem `MyScaleListener`Namen hinzu. Diese Klasse lauscht auf die Skalierungs Ereignisse, die von Android ausgelöst werden, wenn der Benutzer die `ImageView`pingt.
    Fügen Sie die folgende innere Klasse `GestureRecognizerView`hinzu. Diese Klasse ist eine `ScaleGesture.SimpleOnScaleGestureListener`-Klasse. Diese Klasse ist eine Hilfsklasse, die Listener Unterklassen Unterklassen haben können, wenn Sie an einer Teilmenge von Gesten interessiert sind:

    ```csharp
    private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
    {
        private readonly GestureRecognizerView _view;

        public MyScaleListener(GestureRecognizerView view)
        {
            _view = view;
        }

        public override bool OnScale(ScaleGestureDetector detector)
        {
            _view._scaleFactor *= detector.ScaleFactor;

            // put a limit on how small or big the image can get.
            if (_view._scaleFactor > 5.0f)
            {
                _view._scaleFactor = 5.0f;
            }
            if (_view._scaleFactor < 0.1f)
            {
                _view._scaleFactor = 0.1f;
            }

            _view.Invalidate();
            return true;
        }
    }
    ```

-   Die nächste Methode, die in `GestureRecognizerView` überschrieben werden muss, ist. `OnTouchEvent` Der folgende Code listet die vollständige Implementierung dieser Methode auf. Hier ist viel Code vorhanden, deshalb dauert es eine Minute, und wir sehen uns an, was hier passiert. Als erstes skaliert diese Methode das Symbol, wenn dies erforderlich &ndash; ist, indem aufgerufen `_scaleDetector.OnTouchEvent`wird. Als nächstes versuchen wir herauszufinden, welche Aktion diese Methode genannt hat:

    - Wenn der Benutzer den Bildschirm mit berührt hat, notieren wir die X-und Y-Positionen und die ID des ersten Zeigers, der den Bildschirm berührt hat.

    - Wenn der Benutzer den Mauszeiger auf dem Bildschirm bewegt hat, können Sie herausfinden, wie weit der Benutzer den Mauszeiger bewegt hat.

    - Wenn der Benutzer seinen Finger vom Bildschirm entfernt hat, beenden wir die Nachverfolgung der Gesten.

    ```csharp
    public override bool OnTouchEvent(MotionEvent ev)
    {
        _scaleDetector.OnTouchEvent(ev);

        MotionEventActions action = ev.Action & MotionEventActions.Mask;
        int pointerIndex;

        switch (action)
        {
            case MotionEventActions.Down:
            _lastTouchX = ev.GetX();
            _lastTouchY = ev.GetY();
            _activePointerId = ev.GetPointerId(0);
            break;

            case MotionEventActions.Move:
            pointerIndex = ev.FindPointerIndex(_activePointerId);
            float x = ev.GetX(pointerIndex);
            float y = ev.GetY(pointerIndex);
            if (!_scaleDetector.IsInProgress)
            {
                // Only move the ScaleGestureDetector isn't already processing a gesture.
                float deltaX = x - _lastTouchX;
                float deltaY = y - _lastTouchY;
                _posX += deltaX;
                _posY += deltaY;
                Invalidate();
            }

            _lastTouchX = x;
            _lastTouchY = y;
            break;

            case MotionEventActions.Up:
            case MotionEventActions.Cancel:
            // We no longer need to keep track of the active pointer.
            _activePointerId = InvalidPointerId;
            break;

            case MotionEventActions.PointerUp:
            // check to make sure that the pointer that went up is for the gesture we're tracking.
            pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
            int pointerId = ev.GetPointerId(pointerIndex);
            if (pointerId == _activePointerId)
            {
                // This was our active pointer going up. Choose a new
                // action pointer and adjust accordingly
                int newPointerIndex = pointerIndex == 0 ? 1 : 0;
                _lastTouchX = ev.GetX(newPointerIndex);
                _lastTouchY = ev.GetY(newPointerIndex);
                _activePointerId = ev.GetPointerId(newPointerIndex);
            }
            break;

        }
        return true;
    }
    ```

-   Führen Sie nun die Anwendung aus, und starten Sie die Aktivität Gestenerkennung.
    Beim Start sollte der Bildschirm in etwa wie im folgenden Screenshot aussehen:

    [![Startbildschirm der Gestenerkennung mit Android-Symbol](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Berühren Sie nun das Symbol, und ziehen Sie es auf den Bildschirm. Versuchen Sie, die Stift Bewegung zu vergrößern. An einem beliebigen Punkt sieht der Bildschirm in etwa wie der folgende Screenshot aus:

    [![Symbol zum Verschieben von Gesten um den Bildschirm](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

An diesem Punkt sollten Sie sich selbst einen Pat-Rückruf für eine Android-Anwendung durch drücken. Nehmen Sie eine kurze Pause vor, und lassen Sie sich &ndash; mit benutzerdefinierten Gesten mit der dritten und abschließenden Aktivität in dieser exemplarischen Vorgehensweise fortfahren.

## <a name="custom-gesture-activity"></a>Benutzerdefinierte Gesten Aktivität

Der letzte Bildschirm in dieser exemplarischen Vorgehensweise verwendet benutzerdefinierte Gesten.

Im Rahmen dieser exemplarischen Vorgehensweise wurde die Gesten Bibliothek bereits mit dem Gesten Tool erstellt und dem Projekt in den Datei **Ressourcen/RAW/Gesten**hinzugefügt. Mit diesem kleinen Teil der Arbeit können Sie sich mit der abschließenden Aktivität in der exemplarischen Vorgehensweise anmelden.

-   Fügen Sie dem Projekt eine Layoutdatei mit dem Namen **Custom\_Gesten\_Layout. axml** mit folgendem Inhalt hinzu. Das Projekt enthält bereits alle Images im Ordner " **Resources** ":

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
        <ImageView
            android:src="@drawable/check_me"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="3"
            android:id="@+id/imageView1"
            android:layout_gravity="center_vertical" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    </LinearLayout>
    ```

-   Fügen Sie als nächstes dem Projekt eine neue Aktivität hinzu, `CustomGestureRecognizerActivity.cs`und benennen Sie Sie. Fügen Sie der-Klasse zwei Instanzvariablen hinzu, die in den folgenden zwei Codezeilen angezeigt werden:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Bearbeiten Sie `OnCreate` die-Methode der this-Aktivität, sodass Sie dem folgenden Code ähnelt. Wir nehmen uns eine Minute Zeit, um zu erläutern, was in diesem Code passiert. Als erstes instanziieren wir eine `GestureOverlayView` und legen diese als Stamm Ansicht der Aktivität fest.
    Außerdem wird dem `GesturePerformed` -Ereignis von `GestureOverlayView`ein Ereignishandler zugewiesen. Im nächsten Schritt wird die zuvor erstellte Layoutdatei vergrößert und als unter `GestureOverlayView`geordnete Ansicht des hinzugefügt. Der letzte Schritt besteht darin, die Variable `_gestureLibrary` zu initialisieren und die Gesten Datei aus den Anwendungs Ressourcen zu laden. Wenn die Gesten Datei aus irgendeinem Grund nicht geladen werden kann, gibt es nicht viel zu dieser Aktivität, sodass Sie heruntergefahren wird:

    ```csharp
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
        SetContentView(gestureOverlayView);
        gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

        View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
        _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
        gestureOverlayView.AddView(view);

        _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
        if (!_gestureLibrary.Load())
        {
            Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
            Finish();
        }
    }
    ```

-   Im letzten Schritt müssen wir die-Methode `GestureOverlayViewOnGesturePerformed` implementieren, wie im folgenden Code Ausschnitt gezeigt. Wenn eine Geste erkennt, wird diese Methode zurück aufgerufen. `GestureOverlayView` Als erstes versuchen wir, ein `IList<Prediction>` Objekt abzurufen, das mit der Geste identisch ist, indem aufgerufen `_gestureLibrary.Recognize()`wird. Wir verwenden ein Bit von LINQ, um die `Prediction` mit der höchsten Bewertung für die Geste zu erhalten.

    Wenn keine übereinstimmende Geste mit einer hohen ausreichenden Bewertung vorhanden ist, wird der Ereignishandler beendet, ohne etwas zu tun. Andernfalls überprüfen wir den Namen der Vorhersage und ändern das Bild, das auf der Grundlage des Namens der Geste angezeigt wird:

    ```csharp
    private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
    {
        IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
        orderby p.Score descending
        where p.Score > 1.0
        select p;
        Prediction prediction = predictions.FirstOrDefault();

        if (prediction == null)
        {
            Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
            return;
        }

        Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

        if (prediction.Name.StartsWith("checkmark"))
        {
            _imageView.SetImageResource(Resource.Drawable.checked_me);
        }
        else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
        {
            // Match one of our "erase" gestures
            _imageView.SetImageResource(Resource.Drawable.check_me);
        }
    }
    ```

-   Führen Sie die Anwendung aus, und starten Sie die benutzerdefinierte Gesten Erkennungs Aktivität. Dies sollte in etwa wie im folgenden Screenshot aussehen:

    [![Bildschirm Abbildung mit Bild überprüfen](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Zeichnen Sie nun ein Häkchensymbol auf dem Bildschirm, und die anzuzeigende Bitmap sollte in etwa wie in den folgenden Screenshots aussehen:

    [![Gezeichnetes häkches, Häkchensymbol erkannt](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Zeichnen Sie schließlich eine Scribble-Taste auf dem Bildschirm. Das Kontrollkästchen sollte wieder in das ursprüngliche Bild geändert werden, wie in den folgenden Screenshots gezeigt:

    [![Scribble auf dem Bildschirm, ursprüngliches Bild wird angezeigt](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Sie haben nun ein Verständnis für die Integration von toucheingaben und Gesten in eine Android-Anwendung mithilfe von xamarin. Android.


## <a name="related-links"></a>Verwandte Links

- [Android-Berührungs Start (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android-Berührungs Finale (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
