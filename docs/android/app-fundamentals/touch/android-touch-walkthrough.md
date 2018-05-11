---
title: Exemplarische Vorgehensweise – mit Touch in Android
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: 625ba800ce498f80c0344c67e26bd79360de4002
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="walkthrough---using-touch-in-android"></a>Exemplarische Vorgehensweise – mit Touch in Android

Lassen Sie uns finden Sie unter der Konzepte aus dem vorherigen Abschnitt in einer Anwendung verwenden. Es wird eine Anwendung mit vier Aktivitäten erstellt. Die erste Aktivität wird ein Menü oder eine Übersicht, die die anderen Aktivitäten zur Veranschaulichung der verschiedenen APIs gestartet wird. Der folgende Screenshot zeigt die Hauptaktivität:

[![Beispiel-Screenshot mit Touch Me-Schaltfläche](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Die erste Aktivität, Touch-Beispiel zeigt wie Sie Ereignishandler für die Ansichten berühren. Die Bewegung Erkennungsmodul-Aktivität wird gezeigt, wie Unterklasse `Android.View.Views` und Ereignisse behandeln, als auch gezeigt, wie zwei-Finger-Gesten zu behandeln. Der dritte und letzte Aktivität **benutzerdefinierte Geste**, wird zeigen die Verwendung benutzerdefinierter Gesten. Um leichter zu verfolgen und aufnehmen, müssen wir in dieser exemplarischen Vorgehensweise in Abschnitte mit jeder Abschnitt, wobei schwerpunktmäßig auf eine der Aktivitäten zusammensetzen.

## <a name="touch-sample-activity"></a>Touch-Beispielaktivität

-   Öffnen Sie das Projekt **TouchWalkthrough\_starten**. Die **Activity\_main** als gehen alle festgelegt ist &ndash; es ist Aufgabe der Touch-Verhalten der Aktivität implementiert. Wenn Sie die Anwendung auszuführen, und klicken Sie auf **berühren Beispiel**, sollte die folgende Aktivität gestartet:

    [![Screenshot der Aktivität mit Touch beginnt angezeigt](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Nun, da wir bestätigt haben, dass die Aktivität wird gestartet, öffnen Sie die Datei **TouchActivity.cs** und fügen Sie einen Handler für das `Touch` -Ereignis für die `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Fügen Sie die folgende Methode hinzu **TouchActivity.cs**:

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

Beachten Sie, dass wir behandeln im obigen Code die `Move` und `Down` Aktion als identisch. Dafür ist, dass, obwohl der Benutzer nicht ihre möglicherweise deaktiviert Fingers die `ImageView`, möglicherweise verschieben oder die Auslastung des vom Benutzer ausgeübt kann geändert werden. Diese Art von Änderungen generiert eine `Move` Aktion.

Jedes Mal, wenn der Benutzer Fingereingaben der `ImageView`, `Touch` Ereignis wird ausgelöst, und unsere Ereignishandler zeigt an **berühren beginnt** auf dem Bildschirm, wie im folgenden Screenshot gezeigt:

[![Screenshot der Aktivität mit Touch beginnt](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Solange der Benutzer betrifft die `ImageView`, **berühren beginnt** angezeigt werden die `TextView`. Wenn der Benutzer ist nicht mehr durch berühren der `ImageView`, die Nachricht **berühren endet** angezeigt werden die `TextView`, wie im folgenden Screenshot gezeigt:

[![Screenshot der Aktivität mit Touch endet](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Geste Erkennungsmodul-Aktivität

Nun wollen wir uns die Bewegung Erkennungsmodul Aktivität implementiert wird. Diese Aktivität zeigen, wie eine Ansicht auf dem Bildschirm ziehen und es wird eine Möglichkeit, implementieren zwei-Finger-Zoom veranschaulicht.

-   Hinzufügen eine neue Aktivität der Anwendung namens `GestureRecognizer`.
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

-   Hinzufügen einer neuen Android anzeigen, um das Projekt, und nennen Sie sie `GestureRecognizerView`. Fügen Sie diese Klasse die folgenden Variablen hinzu:

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

-   Fügen Sie den folgenden Konstruktor hinzu `GestureRecognizerView`. Dieser Konstruktor wird hinzugefügt. eine `ImageView` an unsere Aktivität. An diesem Punkt im Code noch nicht kompiliert &ndash; müssen wir die Klasse erstellen `MyScaleListener` unterstützt, die beim Ändern der Größe der `ImageView` Wenn der Benutzer pinches es:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Zum Zeichnen des Bilds für unsere Aktivität müssen wir überschreiben die `OnDraw` Methode der Ansichtsklasse wie im folgenden Codeausschnitt gezeigt. Dieser Code geht die `ImageView` an der angegebenen Position `_posX` und `_posY` sowie Größe wie das Bild entsprechend der Skalierungsfaktor:

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

-   Als Nächstes müssen wir die Instanzvariable aktualisieren `_scaleFactor` pinches als der Benutzer die `ImageView`. Es wird eine Klasse mit dem Namen hinzufügen `MyScaleListener`. Diese Klasse wird für die Skalierung-Ereignisse, die von Android ausgelöst werden, wenn der Benutzer pinches überwachen die `ImageView`.
    Fügen Sie die folgenden interne Klasse zu `GestureRecognizerView`. Diese Klasse ist eine `ScaleGesture.SimpleOnScaleGestureListener`. Diese Klasse ist eine Hilfsklasse, die Listener Unterklasse können, wenn Sie eine Teilmenge der Gesten interessiert sind:

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

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   Die next-Methode, die wir in überschreiben, müssen `GestureRecognizerView` ist `OnTouchEvent`. Der folgende Code enthält die vollständige Implementierung dieser Methode. Es ist viel Code hier also ermöglicht, nehmen Sie sich, und sehen Sie, was passiert hier. Diese Methode wird als Erstes wird das Symbol "bei Bedarf skalieren &ndash; Dies erfolgt durch Aufrufen von `_scaleDetector.OnTouchEvent`. Als Nächstes verwenden wir herausfinden, welche Aktion diese Methode aufgerufen:

    - Wenn der Benutzer den Bildschirm mit berührt, zeichnen wir die X- und Y-Positionen und die ID des ersten Zeigers, der den Bildschirm berührt.

    - Wenn der Benutzer die Fingereingabe auf dem Bildschirm verschoben, Abbildung wir, wie weit der Benutzer den Zeiger verschoben.

    - Wenn der Benutzer seine Finger außerhalb des Bildschirms aufgehoben wurde, wird es beendet Gesten nachverfolgen.

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

-   Jetzt führen Sie die Anwendung, und starten Sie die Bewegung Erkennungsmodul-Aktivität.
    Zunächst sollte der Bildschirm der folgende Screenshot aussehen:

    [![Geste Erkennungsmodul Startbildschirm mit Android-Symbol](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Jetzt Tippen Sie auf das Symbol, und ziehen Sie es auf dem Bildschirm. Wiederholen Sie dann die zwei-Finger-Zoom-Aktion. Irgendwann kann Ihren Bildschirm im folgenden Screenshot aussehen:

    [![Gesten Symbol "verschieben" auf dem Bildschirm](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

An diesem Punkt sollten Sie erteilen selbst ein Pat auf der Rückseite: Sie haben zwei-Finger-Zoom nur in einer Android-Anwendung implementiert! Nehmen Sie eine kurze Unterbrechung vorliegt und Sie können auf die dritte und letzte Aktivität in dieser exemplarischen Vorgehensweise zu verschieben, auf &ndash; mithilfe von benutzerdefinierten Gesten.

## <a name="custom-gesture-activity"></a>Benutzerdefinierte Gestenhandler-Aktivität

Im letzten Bildschirm in dieser exemplarischen Vorgehensweise werden die benutzerdefinierte Gesten verwenden.

Im Rahmen dieser exemplarischen Vorgehensweise, die Bibliothek Gesten bereits mit Gestenhandler-Tool erstellt haben und wurde hinzugefügt, um das Projekt in der Datei **Ressourcen/raw/Gesten**. Mit dieser geringe Wartungsabläufe aus dem Weg können auf die letzte Aktivität in der exemplarischen Vorgehensweise erhalten.

-   Fügen Sie eine Layoutdatei mit dem Namen **benutzerdefinierte\_Geste\_layout.axml** auf das Projekt mit dem folgenden Inhalt. Das Projekt enthält bereits alle Bilder der **Ressourcen** Ordner:

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

-   Als Nächstes fügen Sie dem Projekt eine neue Aktivität hinzu, und nennen Sie sie `CustomGestureRecognizerActivity.cs`. Fügen Sie zwei Nachrichteninstanzvariablen der Klasse hinzu, wie in den folgenden zwei Codezeilen angezeigt:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Bearbeiten der `OnCreate` Methode, die dieser Aktivität so, dass die It über den folgenden Code ähnelt. Ermöglicht, nehmen Sie sich zur Erläuterung, was in diesem Code. Führen Sie es als Erstes wird Instanziieren einer `GestureOverlayView` und festlegen, wie die Stammansicht der Aktivität.
    Wir weisen auch einen Ereignishandler an das `GesturePerformed` -Ereignis `GestureOverlayView`. Als Nächstes es vergrößern die Layoutdatei, die zuvor erstellt wurde, und hinzufügen, die als eine untergeordnete Ansicht von der `GestureOverlayView`. Der letzte Schritt besteht, initialisieren Sie die Variable `_gestureLibrary` und Laden Sie die Datei Gesten aus den Anwendungsressourcen. Wenn die Datei Gesten aus irgendeinem Grund nicht geladen werden kann, ist es nicht viel, was diese Aktivität tun kann, damit er heruntergefahren wird:

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

-   Im letzten Schritt wir die Methode implementieren müssen `GestureOverlayViewOnGesturePerformed` wie im folgenden Codeausschnitt gezeigt. Wenn die `GestureOverlayView` eine Geste, erkennt er einen Rückruf an diese Methode. Wir versuchen, erhalten zuerst ein `IList<Prediction>` Objekte, die durch Aufrufen der Aktion entsprechen `_gestureLibrary.Recognize()`. Verwenden wir ein Bit von LINQ zum Abrufen der `Prediction` , besitzt die höchste Bewertung für die Aktion.

    Wenn es kein übereinstimmender wurde Gestenhandler mit hoch genug Ergebnis, und der Ereignishandler beendet, ohne jegliche. Andernfalls überprüfen Sie den Namen der Vorhersage und ändern das Bild angezeigt wird, basierend auf den Namen der Bewegung:

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

-   Führen Sie die Anwendung, und die benutzerdefinierte Aktion Erkennungsmodul Aktivität gestartet. Es sollte etwa wie das folgende Bildschirmfoto aussehen:

    [![Screenshot mit mir Image überprüfen](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Zeichnen Sie nun ein Häkchen auf dem Bildschirm, und die Bitmap angezeigt wird, sollte etwa wie in der nächsten Screenshots aussehen:

    [![Gezeichneten Häkchen, Häkchen wird erkannt.](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Zeichnen Sie schließlich eine Scribble auf dem Bildschirm. Das Kontrollkästchen sollte wieder auf das ursprüngliche Image ändern, wie im folgenden Screenshots gezeigt:

    [![Scribble auf dem Bildschirm, ursprungsabbild wird angezeigt.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Sie haben jetzt einen Überblick über die Vorgehensweise beim Integrieren von Touch- und Gesten in einer Android-Anwendung mit Xamarin.Android.


## <a name="related-links"></a>Verwandte Links

- [Android berühren starten (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android berühren endgültigen (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
