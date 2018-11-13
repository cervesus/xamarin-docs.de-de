---
title: Aufrufen von Ereignissen von Auswirkungen
description: Auswirkungen definieren und ein Ereignis signalisiert Änderungen in der zugrunde liegenden systemeigenen Ansicht aufrufen. In diesem Artikel wird gezeigt, wie Low-Level Multitouch Finger, die nachverfolgung zu implementieren, und zum Generieren von Ereignissen, die Touch-Aktivitäten hinweisen.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 0a5e2c1a7a7807da91fd98e617467ea251a25bc0
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527403"
---
# <a name="invoking-events-from-effects"></a>Aufrufen von Ereignissen von Auswirkungen

_Auswirkungen definieren und ein Ereignis signalisiert Änderungen in der zugrunde liegenden systemeigenen Ansicht aufrufen. In diesem Artikel wird gezeigt, wie Low-Level Multitouch Finger, die nachverfolgung zu implementieren, und zum Generieren von Ereignissen, die Touch-Aktivitäten hinweisen._

Die Auswirkungen, die in diesem Artikel beschriebenen ermöglicht den Zugriff auf Low-Level-Touch-Ereignissen. Diese Ereignisse auf niedriger Ebene sind nicht verfügbar, durch die vorhandene `GestureRecognizer` Klassen, sind aber wichtig, einige Arten von Anwendungen. Z. B. eine finger-paint anwendungsanforderungen einzelner Finger nachverfolgen, wie sie auf dem Bildschirm wechseln. Musik Tastatur Taps erkennen muss, und es auf die einzelnen Schlüssel sowie einen Finger, die von einem Schlüssel in eine andere in einem Glissando gleitend frei.

Ein Effekt ist ideal für Multitouch Finger nachverfolgen, da es zu einem Xamarin.Forms-Element zugeordnet werden kann.

## <a name="platform-touch-events"></a>Touch-Plattformereignisse

Tippen Sie Aktivität auf iOS-, Android- und universelle Windows-Plattform, die alle eine Low-Level-API enthalten, die Anwendungen erkennen können. Berührungsereignisse für diese Plattformen, die alle drei grundlegende Arten von unterscheiden:

- *Gedrückt*, wenn ein Finger den Bildschirm berührt.
- *Verschoben*, wenn ein Finger berührt den Bildschirm bewegt wird.
- *Veröffentlicht*, wenn der Finger vom Bildschirm veröffentlicht wird

In einer Umgebung mit Mehrfingereingabe können mehrere Finger den Bildschirm zur gleichen Zeit berühren. Die verschiedenen Plattformen gehören eine ID (ID), mit denen Anwendungen unterscheiden mehrerer Finger.

Bei iOS der `UIView` -Klasse definiert drei überschreibbare Methoden, `TouchesBegan`, `TouchesMoved`, und `TouchesEnded` für diese drei grundlegende Ereignisse. Der Artikel [Nachverfolgen von Multi-Touch Finger](~/ios/app-fundamentals/touch/touch-tracking.md) beschreibt, wie Sie diese Methoden verwenden. Allerdings ein iOS-Programm muss nicht von einer Klasse überschrieben, die von abgeleitet `UIView` dieser Methoden. IOS `UIGestureRecognizer` definiert auch dieselben drei Methoden, und Sie können eine Instanz einer Klasse, die von abgeleitet Anfügen `UIGestureRecognizer` auf `UIView` Objekt.

In Android die `View` -Klasse definiert eine überschreibbare Methode, die mit dem Namen `OnTouchEvent` alle Touch-Aktivitäten verarbeitet. Der Typ der Aktivität Touch wird definiert, von Enumerationsmembern `Down`, `PointerDown`, `Move`, `Up`, und `PointerUp` in diesem Artikel beschriebenen [Nachverfolgen von Multi-Touch Finger](~/android/app-fundamentals/touch/touch-tracking.md). Die Android `View` definiert auch ein Ereignis namens `Touch` , die einen Ereignishandler an einem beliebigen angefügt werden können `View` Objekt.

In der universellen Windows-Plattform (UWP), die `UIElement` Klasse definiert die Ereignisse, die mit dem Namen `PointerPressed`, `PointerMoved`, und `PointerReleased`. Diese werden in diesem Artikel beschrieben [MSDN-Artikel behandeln Zeiger Eingabe](/windows/uwp/input-and-devices/handle-pointer-input/) und der API-Dokumentation für die [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) Klasse.

Die `Pointer` -API in die universelle Windows-Plattform dient zur Vereinheitlichung der Maus, Touch und Stifteingabe. Aus diesem Grund die `PointerMoved` Ereignis wird immer dann aufgerufen, wenn die Maus über ein Element bewegt wird, selbst wenn eine Maustaste nicht gedrückt wird. Die `PointerRoutedEventArgs` -Objekt, das diesen Ereignissen begleitet, besitzt eine Eigenschaft namens `Pointer` , die besitzt eine Eigenschaft namens `IsInContact` der gibt an, wenn eine Maustaste gedrückt wird oder ein Finger den Bildschirm berühren.

Die UWP definiert darüber hinaus zwei weitere Ereignisse, die mit dem Namen `PointerEntered` und `PointerExited`. Diese Fehlermeldungen geben an, wenn ein Maus- oder fingerbewegung von einem Element in einen anderen verschoben wird. Betrachten Sie beispielsweise zwei benachbarte Elemente, die mit dem Namen A und b. Beide Elemente, die Handler für die zeigerereignisse installiert haben. Wenn ein Finger auf ein, drückt die `PointerPressed` -Ereignis aufgerufen wird. Wenn der Finger bewegt, ruft ein `PointerMoved` Ereignisse. Wenn der Finger von A nach B verschoben wird, ruft ein eine `PointerExited` Ereignis und B ruft eine `PointerEntered` Ereignis. Wenn der Finger dann aufgehoben wird, ruft B eine `PointerReleased` Ereignis.

Die IOS- und Android-Plattformen unterscheiden sich von der UWP: die Ansicht, die den Aufruf zuerst ruft `TouchesBegan` oder `OnTouchEvent` Wenn ein Finger berührt die Ansicht wird fortgesetzt, um die Touch-Aktivität zu erhalten, selbst wenn der fingerbewegung mit anderen Ansichten. Die UWP kann sich ähnlich Verhalten, wenn die Anwendung die Zeiger aufzeichnet: In der `PointerEntered` Ereignishandler, die die Element-Aufrufe `CapturePointer` und ruft anschließend alle Touch-Aktivitäten aus dieser Finger.

Die UWP-Vorgehensweise als sehr nützlich für einige Arten von Anwendungen, z. B. eine Musik-Tastatur. Jeder Schlüssel kann für diesen Schlüssel die touchereignisse behandeln, und erkennen, wenn ein Finger in eine andere mithilfe von einem Schlüssel geschoben hat die `PointerEntered` und `PointerExited` Ereignisse.

Aus diesem Grund implementiert die in diesem Artikel beschriebenen Nachverfolgen von Touch-Auswirkungen den UWP-Ansatz.

## <a name="the-touch-tracking-effect-api"></a>Die Nachverfolgen von Touch-Wirkung-API

Die [ **Touch nachverfolgung Auswirkungen Demos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) Beispiel enthält die Klassen (sowie eine Enumeration), die die Low-Level-Touch-nachverfolgung zu implementieren. Diese Typen gehören zum Namespace `TouchTracking` und beginnen Sie mit dem Wort `Touch`. Die **TouchTrackingEffectDemos** .NET Standard Library-Projekt enthält die `TouchActionType` Enumeration für den Typ des Touch-Ereignissen:

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

Alle Plattformen umfassen auch ein Ereignis, das zeigt an, dass das touchereignis abgebrochen wurde.

Die `TouchEffect` Klasse in der .NET Standardbibliothek leitet sich von `RoutingEffect` und definiert ein Ereignis namens `TouchAction` und eine Methode namens `OnTouchAction` aufruft, die die `TouchAction` Ereignis:

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

Beachten Sie auch die `Capture` Eigenschaft. Um touchereignisse zu erfassen, muss eine Anwendung diese Eigenschaft festlegen, um `true` vor einem `Pressed` Ereignis. Andernfalls Verhalten sich die touchereignisse, wie in der universellen Windows-Plattform zu erhalten.

Die `TouchActionEventArgs` Klasse in der .NET Standard-Bibliothek enthält alle Informationen, die mit jedem Ereignis:

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

Eine Anwendung kann mithilfe der `Id` -Eigenschaft für einzelner Finger verfolgt. Beachten Sie, dass die `IsInContact` Eigenschaft. Diese Eigenschaft ist immer `true` für `Pressed` Ereignisse und `false` für `Released` Ereignisse. Es ist auch immer `true` für `Moved` Ereignisse unter iOS und Android. Die `IsInContact` Eigenschaft möglicherweise `false` für `Moved` Ereignisse für die universelle Windows Plattform aus, wenn das Programm auf dem Desktop ausgeführt wird und der Mauszeiger bewegt, ohne eine Schaltfläche wird geklickt.

Können Sie die `TouchEffect` Klasse in Ihren eigenen Anwendungen, einschließlich der Datei im .NET Standard Library-Projekt der Projektmappe, und durch das Hinzufügen einer Instanz der `Effects` Auflistung eines beliebigen Elements von Xamarin.Forms. Fügen Sie einen Handler, der die `TouchAction` Ereignis, um die Touch-Ereignissen zu erhalten.

Verwendung von `TouchEffect` in Ihrer eigenen Anwendung können außerdem benötigen Sie die plattformimplementierungen, die in enthaltenen **TouchTrackingEffectDemos** Lösung.

## <a name="the-touch-tracking-effect-implementations"></a>Die Auswirkungen der Nachverfolgen von Touch-Implementierungen

IOS-, Android- und UWP Implementierungen der `TouchEffect` werden unten mit die einfachste Implementierung (UWP) und endet mit der iOS-Implementierung, da es mehr strukturell komplexer als das andere ist.

### <a name="the-uwp-implementation"></a>Die UWP-Implementierung

Die UWP-Implementierung von `TouchEffect` ist die einfachste. Wie üblich, die Klasse abgeleitet `PlatformEffect` und enthält zwei Attribute der Assembly:

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

Die `OnAttached` Außerkraftsetzung speichert einige Informationen als Felder und fügt einen Handler für alle zeigerereignisse:

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

Die `OnPointerPressed` Handler Ruft den Effekt-Ereignis durch Aufrufen der `onTouchAction` -Feld in der `CommonHandler` Methode:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed` Außerdem überprüft den Wert des der `Capture` Eigenschaft in der Klasse "Auswirkung" in der .NET Standard-Bibliothek und ruft `CapturePointer` ist dies `true`.

 Die UWP-Ereignishandler sind sogar noch einfacher:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Die Android-Implementierung

Die Android- und iOS-Implementierungen sind unbedingt komplexer, da sie implementieren, müssen die `Exited` und `Entered` Ereignisse aus, wenn ein Finger von einem Element in einen anderen verschiebt. Beide Implementierungen sind ähnlich strukturiert.

Die Android `TouchEffect` Klasse installiert einen Handler für die `Touch` Ereignis:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

Die Klasse definiert außerdem zwei statische Wörterbüchern:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

Die `viewDictionary` ruft Sie einen neuen Eintrag jedes Mal die `OnAttached` Außerkraftsetzung aufgerufen wird:

```csharp
viewDictionary.Add(view, this);
```

Der Eintrag entfernt wird, aus dem Wörterbuch in `OnDetached`. Jede Instanz des `TouchEffect` bezieht sich auf eine bestimmte Ansicht, die die Auswirkungen zugeordnet ist. Das statische Wörterbuch kann jede `TouchEffect` Instanz durchlaufen, bis alle anderen Ansichten und die entsprechenden `TouchEffect` Instanzen. Dies ist erforderlich, um zu ermöglichen, übertragen die Ereignisse von einer Sicht in eine andere.

Android weist einen ID-Code auf Fingereingabeereignisse, der eine Anwendung zum Nachverfolgen von einzelner Finger kann. Die `idToEffectDictionary` ordnet diesen ID-Code mit einem `TouchEffect` Instanz. Dieses Wörterbuch ein Element hinzugefügt wird bei der `Touch` Handler für das Drücken einer Finger aufgerufen wird:

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

Das Element wird entfernt, von der `idToEffectDictionary` wann der Finger vom Bildschirm veröffentlicht wird. Die `FireEvent` Methode sammelt einfach alle Informationen, die zum Aufrufen der `OnTouchAction` Methode:

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

Alle anderen Touch-Typen werden auf zwei unterschiedliche Arten verarbeitet: Wenn die `Capture` Eigenschaft `true`, das touchereignis ist eine ziemlich einfache Übersetzung in die `TouchEffect` Informationen. Es ruft komplizierter, wenn `Capture` ist `false` da Berührungsereignissen möglicherweise von einer Sicht in eine andere verschoben werden müssen. Dies ist die Verantwortung für die `CheckForBoundaryHop` -Methode, die während eines Move-Ereignisse aufgerufen wird. Diese Methode verwendet beide statischen Wörterbüchern. Es durchläuft die `viewDictionary` ermitteln Sie die Ansicht, die derzeit der Finger berührt, und es verwendet `idToEffectDictionary` zum Speichern der aktuellen `TouchEffect` -Instanz (und daher die aktuelle Ansicht) mit einer bestimmten ID verknüpft ist:

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

Wenn eine in Änderung der `idToEffectDictionary`, potenziell Ruft die Methode `FireEvent` für `Exited` und `Entered` Übertragung von einer Sicht in eine andere. Allerdings der Finger möglicherweise wurden verschoben zu einem Bereich belegt wird, indem Sie eine Ansicht ohne eine angefügte `TouchEffect`, oder aus diesem Bereich an eine Ansicht mit dem Effekt angefügt.

Beachten Sie, dass die `try` und `catch` blockieren, wenn die Sicht zugegriffen wird. Auf einer Seite, die mit dem navigiert wird, und klicken Sie dann zurück zur Startseite, navigiert die `OnDetached` Methode nicht aufgerufen, und Elemente verbleiben im der `viewDictionary` aber Android betrachtet werden verworfen.

### <a name="the-ios-implementation"></a>Der iOS-Implementierung

Die iOS-Implementierung ist ähnlich, außer dass die Android-Implementierung der iOS `TouchEffect` Instanziieren der Klasse muss eine Ableitung von `UIGestureRecognizer`. Dies ist eine Klasse im iOS-Projekt mit dem Namen `TouchRecognizer`. Diese Klasse verwaltet zwei statische Wörterbücher, die speichern `TouchRecognizer` Instanzen:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Die Struktur dieser Anteil `TouchRecognizer` Klasse ist vergleichbar mit dem Android `TouchEffect` Klasse.

## <a name="putting-the-touch-effect-to-work"></a>Platzieren den Touch-Effekt für die Arbeit

Die [ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) Programm enthält fünf Seiten, die den Nachverfolgen von Touch-Effekt für allgemeine Aufgaben zu testen.

Die **BoxView ziehen** Seite können Sie die hinzuzufügenden `BoxView` Elementen, die eine `AbsoluteLayout` und ziehen sie dann auf dem Bildschirm. Die [XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) instanziiert zwei `Button` Ansichten für das Hinzufügen von `BoxView` Elementen, die die `AbsoluteLayout` und das Löschen der `AbsoluteLayout`.

Die Methode in der [Code-Behind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) , die Fügt ein neues `BoxView` auf die `AbsoluteLayout` fügt auch eine `TouchEffect` -Objekt die `BoxView` und fügt einen Ereignishandler für den Effekt:

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

Die `TouchAction` Ereignishandler verarbeitet alle Touch-Ereignisse für alle der `BoxView` Elemente, sondern soll einige Vorsicht walten lassen,: Es sind keine zwei Finger auf einem einzelnen zugelassen `BoxView` daran, dass das Programm nur implementiert ziehen, und würden zwei Finger dass sich gegenseitig beeinträchtigen. Aus diesem Grund definiert die Seite eine eingebettete Klasse für jeden Finger, die derzeit nachverfolgten:

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

Die `dragDictionary` enthält einen Eintrag für jede `BoxView` gerade gezogen werden.

Die `Pressed` Touch-Aktion Fügt ein Element dieses Wörterbuch und die `Released` Aktion entfernt. Die `Pressed` Logik muss überprüfen, wenn es bereits ein Element im Wörterbuch für diese ist `BoxView`. Wenn dies der Fall ist, die `BoxView` ist bereits gezogen wird, und das neue Ereignis ist eine zweite finger auf, die gleiche `BoxView`. Für die `Moved` und `Released` Aktionen der Ereignishandler muss überprüfen, ob das Wörterbuch einen Eintrag für diesen hat `BoxView` und die Berührung `Id` Eigenschaft, die gezogen `BoxView` entspricht, der im Wörterbucheintrag:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

Die `Pressed` Logik legt die `Capture` Eigenschaft der `TouchEffect` -Objekt `true`. Dies wirkt sich das Übermitteln von allen nachfolgenden Ereignissen für diese Finger an denselben Ereignishandler aus.

Die `Moved` Logik wechselt die `BoxView` durch Ändern der `LayoutBounds` angefügte Eigenschaft. Die `Location` Eigenschaft der Ereignisargumente ist immer relativ zum die `BoxView` , das gezogen wird, und, wenn die `BoxView` mit konstanter Geschwindigkeit, gezogen wird die `Location` Eigenschaften der aufeinander folgende Ereignisse werden ungefähr gleich sein. Wenn ein Finger drückt z. B. die `BoxView` in die Mitte zu der `Pressed` Aktion speichert eine `PressPoint` Eigenschaft (50, 50), für nachfolgende Ereignisse gleich bleibt. Wenn die `BoxView` diagonal gezogen wird, mit konstanter Geschwindigkeit, die nachfolgenden `Location` Eigenschaften während des der `Moved` Werte handeln (55, 55) in diesem Fall die `Moved` Logik die horizontale und vertikale Position von der 5hinzugefügt`BoxView`. Auf diese Weise verschoben der `BoxView` , damit seine Mitte wieder ist direkt unter dem Finger.

Sie können mehrere verschieben `BoxView` Elemente gleichzeitig mit anderen Finger.

[![](touch-tracking-images/boxviewdragging-small.png "Dreifacher Screenshot der Seite BoxView ziehen")](touch-tracking-images/boxviewdragging-large.png#lightbox "dreifachen Screenshot der Seite BoxView ziehen.")

### <a name="subclassing-the-view"></a>Erstellen von Unterklassen für die Ansicht

Häufig ist es einfacher für ein Xamarin.Forms-Element, um eine eigene touchereignisse zu behandeln. Die **ziehbare BoxView ziehen** Seite funktioniert genauso wie die **BoxView ziehen** Seite, jedoch die Elemente, die der Benutzer zieht Instanzen sind eine [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) abgeleitete Klasse `BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

Der Konstruktor erstellt und fügt die `TouchEffect`, und legt die `Capture` Eigenschaft, wenn das Objekt zuerst instanziiert wird. Kein Wörterbuch ist erforderlich, da die Klasse selbst speichert `isBeingDragged`, `pressPoint`, und `touchId` Werte jeder Finger zugeordnet. Die `Moved` Behandlung ändert die `TranslationX` und `TranslationY` Eigenschaften so die Logik funktioniert auch wenn das übergeordnete Element des der `DraggableBoxView` ist keiner `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>Integrieren von SkiaSharp

Die nächsten beiden Demos erfordern, Grafiken und SkiaSharp für diesen Zweck verwendet. Möglicherweise möchten Sie Informationen zu [mithilfe von SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) , bevor Sie diese Beispiele studieren. Die ersten beiden Artikel ("SkiaSharp-Zeichnungen Basics" und "SkiaSharp Zeilen und Paths") behandelt alle Elemente, die Sie hier benötigen.

Die **Ellipse zeichnen** Seite können Sie eine Ellipse zeichnen sich durch Wischen Ihren Finger auf dem Bildschirm. Abhängig davon, wie Sie Ihren Finger verschieben, können Sie die Ellipse aus der linken oberen Ecke auf der rechten unteren Ecke oder aus einer beliebigen anderen Ecke zur gegenüberliegenden Ecke zeichnen. Die Ellipse, die mit einem zufällig ausgewählten Farbe und Deckkraft gezeichnet wird.

[![](touch-tracking-images/ellipsedrawing-small.png "Dreifacher Screenshot der Seite Ellipse zeichnen")](touch-tracking-images/ellipsedrawing-large.png#lightbox "dreifachen Screenshot der Seite Ellipse zeichnen")

Wenn Sie dann eine Ellipse der Fingereingabe, können Sie es an eine andere Position ziehen. Dies erfordert ein Verfahren, bekannt als "Treffertests," Suche nach dem grafischen Objekt zu einem bestimmten Zeitpunkt dazu. Die Auslassungspunkte SkiaSharp sind nicht Xamarin.Forms-Elemente, damit sie ihre eigenen ausführen können nicht `TouchEffect` verarbeiten. Die `TouchEffect` müssen gelten für die gesamte `SKCanvasView` Objekt.

Die [EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) Datei instanziiert den `SKCanvasView` in einer einzelnen Zelle `Grid`. Die `TouchEffect` Objekt angefügt ist, die `Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Unter Android und der universellen Windows-Plattform die `TouchEffect` kann direkt angefügt werden, die `SKCanvasView`, aber unter iOS, das nicht funktioniert. Beachten Sie, dass die `Capture` -Eigenschaftensatz auf `true`.

Jede Ellipse, die SkiaSharp rendert, wird durch ein Objekt des Typs dargestellt `EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

Die `StartPoint` und `EndPoint` Eigenschaften werden verwendet, wenn das Programm Fingereingaben; verarbeitet die `Rectangle` Eigenschaft wird verwendet, das die Ellipse gezeichnet. Die `LastFingerLocation` Eigenschaft ins Spiel, wenn die Ellipse gezogen wird, und die `IsInEllipse` Methode unterstützt die Internetsuche Treffertests. Gibt die Methode zurück `true` ist der Punkt innerhalb der Ellipse.

Die [Code-Behind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) drei Auflistungen verwaltet:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

Die `draggingFigure` Wörterbuch enthält eine Teilmenge der `completedFigures` Auflistung. Die SkiaSharp `PaintSurface` -Ereignishandler wird einfach die Objekte in diesen rendert die `completedFigures` und `inProgressFigures` Sammlungen:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

Der schwierigste Teil der Touch-Verarbeitung ist der `Pressed` behandeln. Hier kommt der Treffertest ausgeführt wird, aber wenn der Code unter dem Finger des Benutzers eine Ellipse erkennt, diese Ellipse nur gezogen werden kann, wenn es derzeit nicht von einem anderen Finger gezogen wird. Liegt keine Ellipse unter dem Finger des Benutzers, beginnt der Code den Prozess der eine neue Ellipse Zeichnen:

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

Das andere SkiaSharp-Beispiel ist die **Fingerpaint** Seite. Sie können eine Strichfarbe und die Strichbreite auswählen, aus zwei `Picker` anzeigt, und klicken Sie dann mit einem oder mehreren Fingern zeichnen:

[![](touch-tracking-images/fingerpaint-small.png "Dreifacher Screenshot der Seite Fingerpaint")](touch-tracking-images/fingerpaint-large.png#lightbox "dreifachen Screenshot der Seite Fingerpaint")

Dieses Beispiel erfordert auch eine separate Klasse für jede Zeile, die auf dem Bildschirm gezeichnet darstellen:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

Ein `SKPath` Objekt wird verwendet, um jede Zeile zu rendern. Die [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) Datei verwaltet zwei Sammlungen dieser Objekte, für die Polylinien, die gerade gezeichnet wird und eine für die abgeschlossenen Polylinien:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die `Pressed` Verarbeitung erstellt ein neues `FingerPaintPolyline`, Aufrufe `MoveTo` für das Pfadobjekt zum Speichern von des ersten Punkt, und fügt das Objekt mit der `inProgressPolylines` Wörterbuch. Die `Moved` Aufrufe verarbeiten `LineTo` für das Pfadobjekt, mit der neuen Position der Finger, und die `Released` Verarbeitung überträgt die abgeschlossene Polylinie aus `inProgressPolylines` zu `completedPolylines`. Die tatsächliche SkiaSharp Zeichencode ist wieder recht einfach:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>Nachverfolgen von Touch des Ansicht-zu-Ansicht

Alle in die vorherigen Beispielen festgelegt haben die `Capture` Eigenschaft der `TouchEffect` zu `true`, wenn entweder die `TouchEffect` erstellt wurde oder wenn die `Pressed` Ereignis aufgetreten ist. Dadurch wird sichergestellt, dass das gleiche Element empfängt alle Ereignissen im Zusammenhang mit dem Finger, der zunächst die Sicht gedrückt. Im letzten Beispiel *nicht* festgelegt `Capture` zu `true`. Dies bewirkt, dass unterschiedliche Verhalten, wenn ein Finger den Bildschirm berühren von einem Element in einen anderen verschoben wird. Das Element, das aus der fingerbewegung empfängt ein Ereignis mit einer `Type` -Eigenschaft auf festgelegt `TouchActionType.Exited` und das zweite Element empfängt ein Ereignis mit einer `Type` -Einstellung `TouchActionType.Entered`.

Diese Art der Touch-Verarbeitung ist sehr nützlich für eine Musik-Tastatur. Ein Schlüssel sollten in der Lage zu erkennen, wann es, sondern auch gedrückt wird, wenn ein Finger von einem Schlüssel zu einem anderen verschoben.

Die **automatische Tastatur** Seite definiert kleine [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) und [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) abgeleitete Klassen [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs), die sich daraus ableitet `BoxView`.

Die `Key` Klasse ist in einem tatsächlichen Musikprogramm verwendet werden. Es definiert, dass öffentliche Eigenschaften namens `IsPressed` und `KeyNumber`, die auf den Tastencode hergestellt, indem der MIDI-Standard festgelegt werden soll. Die `Key` Klasse definiert auch ein Ereignis namens `StatusChanged`, wird aufgerufen, wenn die `IsPressed` eigenschaftenänderungen.

Mehrere Finger sind für jeden Schlüssel zulässig. Aus diesem Grund die `Key` -Klasse verwaltet eine `List` von Touch-ID-Nummern der alle Finger berühren derzeit auf diesem Schlüssel:

```csharp
List<long> ids = new List<long>();
```

Die `TouchAction` Ereignishandler fügt eine ID für die die `ids` Liste für beide eine `Pressed` -Ereignis des Typs und ein `Entered` -Typ, sondern nur, wenn die `IsInContact` -Eigenschaft ist `true` für die `Entered` Ereignis. Die ID wird entfernt, von der `List` für eine `Released` oder `Exited` Ereignis:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

Die `AddToList` und `RemoveFromList` als auch überprüfen, ob die `List` zwischen leer und nicht leer ist, geändert hat und wenn dies der Fall ist, ruft der `StatusChanged` Ereignis.

Die verschiedenen `WhiteKey` und `BlackKey` Elemente werden in der Seite angeordnet [XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), sieht die am besten, wenn das Telefon in einer im Querformat gehalten wird:

[![](touch-tracking-images/silentkeyboard-small.png "Dreifacher Screenshot der Seite Automatische Tastatur")](touch-tracking-images/silentkeyboard-large.png#lightbox "dreifachen Screenshot der Seite Automatische Tastatur")

Wenn Sie Ihren Finger über die Tasten sweeps, sehen Sie durch die geringfügigen Änderungen in Farbe, dass die touchereignisse von einem Schlüssel zu einem anderen übertragen werden.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat veranschaulicht, wie zum Aufrufen von Ereignissen eines Effekts, und Schreiben und verwenden Sie einen Effekt, der auf niedriger Ebene Multitouch-Verarbeitung implementiert.


## <a name="related-links"></a>Verwandte Links

- [Nachverfolgen von Multitouch Finger in iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Multitouch-Finger, die Nachrichtennachverfolgung in Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [Nachverfolgen von Touch-Auswirkungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
