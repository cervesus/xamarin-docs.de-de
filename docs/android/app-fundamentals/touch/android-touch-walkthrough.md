---
title: 'Exemplarische Vorgehensweise: Verwenden von Toucheingaben in Android'
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: c4192f22ebd0ad1cde27745f5439c2d18a268ed3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111024"
---
# <a name="walkthrough---using-touch-in-android"></a>Exemplarische Vorgehensweise: Verwenden von Toucheingaben in Android

Lassen Sie uns finden Sie unter die Konzepte aus dem vorherigen Abschnitt in einer Anwendung verwenden. Wir erstellen eine Anwendung mit vier Aktivitäten. Ein Menü oder eine Übersicht, die die anderen Aktivitäten zur Veranschaulichung der verschiedenen APIs gestartet wird, wird die erste Aktivität sein. Der folgende Screenshot zeigt die Hauptaktivität:

[![Beispielscreenshot mit Touch Me-Schaltfläche](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Die erste Aktivität, die Touch-Beispiel zeigen, wie von Ereignishandlern für die Ansichten zu berühren. Die Aktivität Stiftbewegungs-Erkennung wird gezeigt, wie das Erstellen einer Unterklasse `Android.View.Views` und Ereignisse behandeln, als auch zeigen, wie Sie die Pinch-Gesten behandelt. Der dritte und letzte Aktivität **benutzerdefinierte Geste**, wird zeigen die Verwendung benutzerdefinierter stiftbewegungen. Um leichter zu folgen und absorb, werden wir in dieser exemplarischen Vorgehensweise in Abschnitte mit jedem Abschnitt mit dem Schwerpunkt auf eine der Aktivitäten aufzuteilen.

## <a name="touch-sample-activity"></a>Touch-Beispielaktivität

-   Öffnen Sie das Projekt **TouchWalkthrough\_starten**. Die **MainActivity** startklar zu &ndash; ist dafür verantwortlich, die das Verhalten der Toucheingabe in der Aktivität zu implementieren. Wenn Sie die Anwendung auszuführen, und klicken Sie auf **Touch Beispiel**, sollte die folgende Aktivität starten:

    [![Screenshot der Aktivität mit Touch beginnt angezeigt](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Nun, da wir bestätigt haben, dass die Aktivität wird gestartet, öffnen Sie die Datei **TouchActivity.cs** und Hinzufügen eines ereignishandlers für das `Touch` Ereignis die `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Fügen Sie die folgende Methode **TouchActivity.cs**:

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

Im obigen Code Beachten Sie, dass wir behandeln die `Move` und `Down` Aktion als identisch. Dies ist da, obwohl der Benutzer nicht den Finger heben Sie möglicherweise aus der `ImageView`, es verschieben kann, oder der vom Benutzer üben Druck kann sich ändern. Diese Art von Änderungen generiert eine `Move` Aktion.

Jedes Mal, wenn die Workflows der Benutzer die `ImageView`, `Touch` Ereignis wird ausgelöst, und unser Handler wird die Meldung angezeigt **Touch beginnt** auf dem Bildschirm, wie im folgenden Screenshot gezeigt:

[![Screenshot der Aktivität mit Touch beginnt](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Solange der Benutzer berührt die `ImageView`, **Touch beginnt** erscheint in der `TextView`. Wenn der Benutzer ist nicht mehr berührt die `ImageView`, die Nachricht **Touch endet** erscheint in der `TextView`, wie im folgenden Screenshot gezeigt:

[![Screenshot der Aktivität mit Touch endet](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Geste Erkennung-Aktivität

Jetzt können die Aktivität Stiftbewegungs-Erkennung zu implementieren. Diese Aktivität wird eine Ansicht auf dem Bildschirm ziehen und veranschaulicht eine Möglichkeit zum Implementieren der Pinch-Finger-Zoom zu veranschaulichen.

-   Fügen Sie eine neue Aktivität für die Anwendung `GestureRecognizer`.
    Bearbeiten Sie den Code für diese Aktivität aus, sodass sie den folgenden Code ähnelt:

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

-   Fügen Sie eine neue Android zeigen Sie auf das Projekt, und nennen Sie sie `GestureRecognizerView`. Fügen Sie die folgenden Variablen für diese Klasse ein:

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

-   Fügen Sie den folgenden Konstruktor hinzu `GestureRecognizerView`. Dieser Konstruktor wird hinzugefügt. eine `ImageView` unserer Aktivität. An dieser Stelle den Code noch nicht kompiliert werden &ndash; müssen wir die Klasse erstellen `MyScaleListener` soll, die zum Ändern der Größe der `ImageView` Wenn der Benutzer pinches es:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Um das Bild in unserer Aktivität zu zeichnen, müssen wir überschreiben die `OnDraw` -Methode der Klasse Ansicht wie im folgenden Codeausschnitt gezeigt. Dieser Code geht die `ImageView` an die Position, die anhand des `_posX` und `_posY` auch als die Bildgröße gemäß den Skalierungsfaktor:

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

-   Als Nächstes müssen wir die Instanzvariable aktualisieren `_scaleFactor` pinches als der Benutzer die `ImageView`. Fügen wir eine Klasse namens `MyScaleListener`. Diese Klasse wird lauscht, für die skalierungsereignisse, die von Android ausgelöst werden, wenn der Benutzer pinches der `ImageView`.
    Fügen Sie die folgende interne Klasse `GestureRecognizerView`. Diese Klasse ist eine `ScaleGesture.SimpleOnScaleGestureListener`. Diese Klasse ist eine Hilfsklasse, dass der Listener als Unterklasse verwenden können, wenn Sie eine Teilmenge der Bewegungen interessiert sind:

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

-   Die nächste Methode, die wir in außer Kraft setzen müssen `GestureRecognizerView` ist `OnTouchEvent`. Der folgende Code Listet die vollständige Implementierung dieser Methode. Es ist viel Code hier also ermöglicht, eine Minute dauern, und sehen, was hier passiert. Diese Methode wird als Erstes wird das Symbol "bei Bedarf skalieren &ndash; Dies erfolgt durch Aufrufen von `_scaleDetector.OnTouchEvent`. Als Nächstes versuchen wir herauszufinden, welche Aktion diese Methode aufgerufen:

    - Wenn der Benutzer den Bildschirm mit berührt, zeichnen wir die X- und Y-Position und die ID der der erste Zeiger, der den Bildschirm berührt.

    - Wenn der Benutzer die Touch auf dem Bildschirm verschoben, ermitteln wir, wie weit der Benutzer den Zeiger verschoben.

    - Wenn der Benutzer seinen Finger außerhalb des Bildschirms transformiert wurde, wird dann beenden wir die Gesten nachverfolgen.

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

-   Nun führen Sie die Anwendung, und starten Sie die Aktivität Stiftbewegungs-Erkennung.
    Zunächst sollte der Bildschirm etwa wie im folgenden Screenshot aussehen:

    [![Stiftbewegungs-Erkennung Startbildschirm mit Android-Symbol](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Jetzt Tippen Sie auf das Symbol, und ziehen Sie es auf dem Bildschirm. Versuchen Sie es der Pinch-Finger-Zoom-Bewegung. Irgendwann kann es sich bei Ihr Bildschirm etwa wie im folgenden Screenshot aussehen:

    [![Bewegungen Verschiebesymbol auf dem Bildschirm](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

An diesem Punkt sollten Sie erteilen selbst ein persönliches Zugriffstoken auf der Rückseite: Sie haben die Pinch-Finger-Zoom einfach in eine Android-Anwendung implementiert! Nehmen Sie eine kurze Pause, und können auf die dritte und letzte Aktivität in dieser exemplarischen Vorgehensweise fortfahren &ndash; mithilfe von benutzerdefinierten stiftbewegungen.

## <a name="custom-gesture-activity"></a>Benutzerdefinierte Aktion-Aktivität

Der letzten Seite in dieser exemplarischen Vorgehensweise werden die benutzerdefinierte stiftbewegungen verwenden.

Im Rahmen dieser exemplarischen Vorgehensweise, die Gesten-Bibliothek bereits mit Gesten-Tool erstellt haben und wurde dem Projekt in der Datei hinzugefügt, **Ressourcen/raw/Gesten**. Mit diesem wenig Organisationsarbeit aus dem Weg können mit der letzten Aktivität in der exemplarischen Vorgehensweise erhalten in ein.

-   Fügen Sie die Layoutdatei **benutzerdefinierte\_Geste\_layout.axml** auf das Projekt mit dem folgenden Inhalt. Das Projekt enthält bereits alle Images in die **Ressourcen** Ordner:

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

-   Als Nächstes fügen Sie dem Projekt eine neue Aktivität, und nennen Sie sie `CustomGestureRecognizerActivity.cs`. Fügen Sie zwei Instanzvariablen wie gezeigt in den folgenden zwei Zeilen Code der Klasse hinzu:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Bearbeiten der `OnCreate` Methode dieser Aktivität so, dass die It den folgenden Code ähnelt. Können eine Minute dauern zu erklären, was in diesem Code geht. Das erste, was wir tun wird instanziiert ein `GestureOverlayView` und festlegen, wie die Stammansicht der Aktivität.
    Weisen wir auch einen Ereignishandler an das `GesturePerformed` Ereignis `GestureOverlayView`. Als Nächstes wir die Layoutdatei, die zuvor erstellte Vergrößerung und hinzufügen, die als eine untergeordnete Ansicht von der `GestureOverlayView`. Der letzte Schritt ist, um die Variable initialisieren `_gestureLibrary` und die Gesten-Datei aus den Anwendungsressourcen zu laden. Wenn die Gesten-Datei aus irgendeinem Grund nicht geladen werden kann, ist gibt es nicht, die diese Aktivität tun kann, damit er heruntergefahren wird:

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

-   Abschließend wir die Methode implementieren müssen `GestureOverlayViewOnGesturePerformed` wie im folgenden Codeausschnitt gezeigt. Wenn die `GestureOverlayView` eine stiftbewegung, erkennt er einen Rückruf an diese Methode. Das erste, was wir versuchen, Sie erhalten eine `IList<Prediction>` Objekten, die die Bewegung durch Aufrufen von entsprechen `_gestureLibrary.Recognize()`. Wir verwenden ein wenig LINQ zum Abrufen der `Prediction` , bei dem der höchsten Bewertung für die Bewegung.

    Wenn es kein passendes wurde Gestenhandler mit einer hohen genügend Bewertung der Ereignishandler und beendet dann ohne Benutzereingriff. Andernfalls wir überprüfen Sie den Namen der Vorhersage, und ändern das Bild angezeigt wird, basierend auf den Namen der Bewegung:

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

-   Führen Sie die Anwendung, und starten Sie die benutzerdefinierte Stiftbewegungs-Erkennung-Aktivität. Es sollte etwa wie im folgenden Screenshot aussehen:

    [![Screenshot mit mir Image überprüfen](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Zeichnen Sie nun ein Häkchen auf dem Bildschirm, und die angezeigten Bitmap sollte in etwa wie in den nächsten Screenshots aussehen:

    [![Gezeichneten Häkchen, Häkchen wird erkannt.](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Zeichnen Sie ein Gekritzel auf dem Bildschirm. Das Kontrollkästchen sollte wieder auf das ursprüngliche Image ändern, wie im folgenden Screenshots gezeigt:

    [![Scribble auf dem Bildschirm, das ursprüngliche Bild wird angezeigt.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Sie haben nun einen Überblick über die Integration von Berührung und Gesten in einer Android-Anwendung mithilfe von Xamarin.Android.


## <a name="related-links"></a>Verwandte Links

- [Android Touch starten (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch endgültige (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
