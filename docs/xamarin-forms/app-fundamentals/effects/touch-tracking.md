---
title: Aufrufen von Ereignissen von Effekten
description: Ein Effekt definieren und ein Ereignis, die gewartet werden Änderungen in der zugrunde liegenden systemeigenen Ansicht aufrufen. In diesem Artikel zeigt, wie auf niedriger Ebene Multitouch Finger nachverfolgen implementiert und wie Ereignisse generiert werden, die Touch-Aktivität zu signalisieren.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: e363cae4dd72a25e4768395410d4e56a8db30eba
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="invoking-events-from-effects"></a>Aufrufen von Ereignissen von Effekten

_Ein Effekt definieren und ein Ereignis, die gewartet werden Änderungen in der zugrunde liegenden systemeigenen Ansicht aufrufen. In diesem Artikel zeigt, wie auf niedriger Ebene Multitouch Finger nachverfolgen implementiert und wie Ereignisse generiert werden, die Touch-Aktivität zu signalisieren._

Die Auswirkungen, die in diesem Artikel beschriebenen ermöglicht den Zugriff auf Low-Level Berührungsereignisse. Diese Ereignisse auf niedriger Ebene sind nicht verfügbar, durch die vorhandenen `GestureRecognizer` Klassen, doch diese sind entscheidend für einige Arten von Anwendungen. Z. B. eine Anwendung benötigt, um die einzelnen Finger verfolgen, während sie auf dem Bildschirm finger-paint. Musik Tastatur muss Taps erkennen und auf einzelne Schlüssel als auch von einem Schlüssel in eine andere in einer Glissando gleitend sorgen frei.

Ein Effekt ist ideal für Multi-Touch-Finger nachverfolgen, da er auf jedes Element Xamarin.Forms angefügt werden kann.

## <a name="platform-touch-events"></a>Touch-Plattformereignisse

Touch-Aktivität iOS-, Android und universelle Windows-Plattform, die alle eine Low-Level-API, die Clientanwendungen ermöglicht enthalten, zu erkennen. Diese Plattformen, mit denen, die alle drei grundlegende Arten von unterscheiden, touch-Ereignisse:

- *Gedrückt*, wenn ein Finger den Bildschirm berührt.
- *Verschoben*, wenn ein Finger den Bildschirm berühren bewegt wird.
- *Freigegeben*, wenn sich der Finger vom Bildschirm freigegeben wird

In einer Multi-Touch-Umgebung können mehrere Finger den Bildschirm zur gleichen Zeit berühren. Die verschiedenen Plattformen enthalten eine Identifikationsnummer (ID), die Anwendungen verwenden können, um zwischen mehreren Finger zu unterscheiden.

In iOS die `UIView` Klasse definiert drei überschreibbare Methoden, `TouchesBegan`, `TouchesMoved`, und `TouchesEnded` , diese drei grundlegende Ereignisse entspricht. Der Artikel [Multitouch Finger nachverfolgen](~/ios/app-fundamentals/touch/touch-tracking.md) wird beschrieben, wie diese Methoden verwenden. Allerdings ein iOS-Programm muss nicht von einer Klasse überschrieben, die abgeleitet `UIView` dieser Methoden verwenden. IOS `UIGestureRecognizer` definiert auch dieselben drei Methoden, und Sie können eine Instanz einer Klasse, die abgeleitet Anfügen `UIGestureRecognizer` an ein beliebiges `UIView` Objekt.

In der Android-Geräten die `View` Klasse definiert eine überschreibbare Methode mit dem Namen `OnTouchEvent` zum Verarbeiten der Touch-Aktivität. Der Typ der Aktivität Touch wird definiert, von Enumerationsmembern `Down`, `PointerDown`, `Move`, `Up`, und `PointerUp` im Artikel beschriebenen [Multitouch Finger nachverfolgen](~/android/app-fundamentals/touch/touch-tracking.md). Die Android `View` definiert auch ein Ereignis namens `Touch` , mit dessen Hilfe eines ereignishandlers eine anzufügenden `View` Objekt.

In der universelle Windows-Plattform (UWP), die `UIElement` Klasse definiert die Ereignisse, die mit dem Namen `PointerPressed`, `PointerMoved`, und `PointerReleased`. Diese werden im Artikel beschrieben [behandeln Zeiger Eingabe Artikel auf MSDN](/windows/uwp/input-and-devices/handle-pointer-input/) und der API-Dokumentation für die [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) Klasse.

Die `Pointer` -API in universellen Windows-Plattform zu vereinheitlichen der Maus, Fingereingabe und Stifteingabe dient. Aus diesem Grund die `PointerMoved` Ereignis wird aufgerufen, wenn die Maus über ein Element bewegt wird, selbst wenn eine Maustaste nicht gedrückt wird. Die `PointerRoutedEventArgs` -Objekt, das diesen Ereignissen begleitet verfügt über eine Eigenschaft namens `Pointer` , die eine Eigenschaft namens hat `IsInContact` gibt an, wenn eine Maustaste gedrückt wird oder ein Finger mit dem Bildschirm verbunden ist.

Die universelle Windows-Plattform definiert außerdem zwei weitere Ereignisse, die mit dem Namen `PointerEntered` und `PointerExited`. Diese Werte zeigen, wenn eine Maus oder ein Finger von einem Element in eine andere verschoben wird. Angenommen, haben Sie zwei benachbarte Elemente, die mit dem Namen als auch b Beide Elemente, die Handler für die Ereignisse Zeiger installiert haben. Wenn ein Finger auf A drückt die `PointerPressed` Ereignis aufgerufen wird. Bewegen Sie der Finger, ruft ein `PointerMoved` Ereignisse. Ein wird aufgerufen, wenn sich der Finger von A zu B verschoben werden, eine `PointerExited` Ereignis und B ruft eine `PointerEntered` Ereignis. Klicken Sie dann der Finger freigegeben, B ruft eine `PointerReleased` Ereignis.

Die IOS- und Android-Plattformen unterscheiden sich von UWP: die Ansicht, die den Aufruf von zuerst abruft `TouchesBegan` oder `OnTouchEvent` , wenn ein Finger berührt weiterhin die Sicht alle die Touch-Aktivität, selbst wenn erhalten, wenn den Finger mit anderen Ansichten bewegt. UWP kann auf ähnliche Weise Verhalten, wenn die Anwendung die Zeiger aufzeichnet: In der `PointerEntered` -Ereignishandler, die Element-Aufrufe `CapturePointer` und ruft anschließend alle Touch-Aktivität aus, den Finger wieder an.

Die uwp-Ansatz wird bewiesen, dass für einige Arten von Anwendungen, z. B. auf einer Tastatur Musik sehr nützlich sein. Jeder Schlüssel kann die Berührungsereignisse für diesen Schlüssel zu behandeln und erkennen, wenn ein Finger in eine andere mithilfe von einem Schlüssel verschoben wurde die `PointerEntered` und `PointerExited` Ereignisse.

Aus diesem Grund implementiert der in diesem Artikel beschriebenen Touch-Tracking-Effekt den uwp-Ansatz.

## <a name="the-touch-tracking-effect-api"></a>Die Touch-Tracking Effekt-API

Die [ **berühren nachverfolgen Auswirkungen Demos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) Beispiel enthält die Klassen (und einer Enumeration), die die Low-Level Touch-Tracking zu implementieren. Diese Typen gehören zum Namespace `TouchTracking` und beginnen mit dem Wort `Touch`. Die **TouchTrackingEffectDemos** .NET Standard-Bibliotheksprojekt enthält die `TouchActionType` Enumeration für den Typ der Berührungsereignisse:

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

Alle Plattformen umfassen auch ein Ereignis, das zeigt an, dass das Touch-Ereignis abgebrochen wurde.

Die `TouchEffect` Klasse in der Standardbibliothek .NET leitet sich von `RoutingEffect` und definiert ein Ereignis mit dem Namen `TouchAction` und eine Methode namens `OnTouchAction` aufruft, die die `TouchAction` Ereignis:

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

Beachten Sie außerdem die `Capture` Eigenschaft. Um Touch-Ereignisse zu erfassen, muss eine Anwendung diese Eigenschaft festlegen, um `true` vor einem `Pressed` Ereignis. Ansonsten Verhalten sich die Berührungsereignisse, wie die in universellen Windows-Plattform zu erhalten.

Die `TouchActionEventArgs` Klasse in der Standardbibliothek .NET enthält alle Informationen, die zu jedem Ereignis gehören:

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

Eine Anwendung kann mithilfe der `Id` Eigenschaft zum Nachverfolgen von einzelnen Fingern fest. Beachten Sie, dass die `IsInContact` Eigenschaft. Diese Eigenschaft ist immer `true` für `Pressed` Ereignisse und `false` für `Released` Ereignisse. Es ist auch immer `true` für `Moved` Ereignisse auf IOS- und Android. Die `IsInContact` Eigenschaft möglicherweise `false` für `Moved` Ereignisse auf die universelle Windows-Plattform, wenn das Programm auf dem Desktop ausgeführt wird und der Mauszeiger bewegt, ohne eine Schaltfläche wird gedrückt.

Können Sie die `TouchEffect` Klasse in Ihren eigenen Anwendungen dazu die Datei in der Projektmappe .NET Standard-Bibliotheksprojekt, und durch Hinzufügen einer Instanz, um die `Effects` Auflistung eines beliebigen Elements Xamarin.Forms. Fügen Sie einen Handler, der `TouchAction` Ereignis, um die Berührungsereignisse zu erhalten.

Mit `TouchEffect` in Ihrer eigenen Anwendung, Sie benötigen auch das Plattform-Implementierungen in enthalten **TouchTrackingEffectDemos** Lösung.

## <a name="the-touch-tracking-effect-implementations"></a>Die Implementierungen der Effekt Touch-Überwachung

Die iOS-, Android- und uwp-Implementierungen von der `TouchEffect` werden unten mit der einfachsten Implementierung (UWP) beginnt und endet mit der iOS-Implementierung mehr strukturell komplex als die anderen Optionen beschrieben.

### <a name="the-uwp-implementation"></a>Die uwp-Implementierung

Die uwp-Implementierung von `TouchEffect` ist die am einfachsten. Wie üblich die Klasse abgeleitet `PlatformEffect` und enthält zwei Attribute der Assembly:

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

Die `OnAttached` Außerkraftsetzung speichert einige Informationen als Felder und fügt einen Handler für alle Zeiger-Ereignisse:

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

Die `OnPointerPressed` Ereignishandler ruft die Auswirkungen Ereignis durch Aufrufen der `onTouchAction` -Feld in der `CommonHandler` Methode:

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

`OnPointerPressed` überprüft auch den Wert der `Capture` Eigenschaften der Klasse Effekt in der Standardbibliothek .NET und Aufrufe `CapturePointer` wird jedoch `true`.

 Die UWP-Ereignishandler werden jetzt noch einfacher:

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

Android und iOS-Implementierungen sind zwangsläufig komplexer, da sie implementieren müssen die `Exited` und `Entered` Ereignisse aus, wenn ein Finger von einem Element in eine andere verschoben. Beide Implementierungen sind ähnlich strukturiert.

Die Android `TouchEffect` Klasse installiert wird, einen Handler für das `Touch` Ereignis:

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

Die `viewDictionary` Ruft einen neuen Eintrag jedes Mal die `OnAttached` Außerkraftsetzung aufgerufen wird:

```csharp
viewDictionary.Add(view, this);
```

Der Eintrag wird entfernt, aus dem Wörterbuch in `OnDetached`. Jede Instanz des `TouchEffect` bezieht sich auf eine bestimmte Ansicht, die der Effekt angefügt ist. Das statische Wörterbuch kann jede `TouchEffect` Instanz zu durchlaufen, bis die andere Sichten und die entsprechenden `TouchEffect` Instanzen. Dies ist erforderlich, damit für die Ereignisse von einer Sicht in eine andere übertragen kann.

Android weist einen ID-Code um touch-Ereignisse, der eine Anwendung zum Nachverfolgen von einzelnen Finger ermöglicht. Die `idToEffectDictionary` ordnet diese ID-Code mit einem `TouchEffect` Instanz. Dieses Wörterbuch ein Element hinzugefügt wird bei der `Touch` Handler für das Drücken einer Finger aufgerufen wird:

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

Entfernt das Element aus der `idToEffectDictionary` wann der Finger vom Bildschirm freigegeben wird. Die `FireEvent` Methode sammelt einfach alle Informationen, die erforderlich sind, rufen Sie die `OnTouchAction` Methode:

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

Alle anderen Touch-Typen werden auf zwei unterschiedliche Arten verarbeitet: Wenn die `Capture` Eigenschaft `true`, das Touch-Ereignis ist eine relativ einfache Übersetzung in die `TouchEffect` Informationen. Er ruft komplizierter, wenn `Capture` ist `false` , da die Berührungsereignisse möglicherweise von einer Sicht in eine andere verschoben werden müssen. Dies ist die Zuständigkeit für die `CheckForBoundaryHop` Methode, die während der Verschiebung Ereignisse aufgerufen wird. Diese Methode verwendet beide statischen Wörterbüchern. Es durchläuft die `viewDictionary` ermitteln Sie die Ansicht, die der Finger ist derzeit berühren und verwendet `idToEffectDictionary` zum Speichern von aktuellen `TouchEffect` Instanz (und daher die aktuelle Ansicht) einer bestimmten ID zugeordnet:

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

Wenn eine Änderung stattgefunden hat die `idToEffectionDictionary`, potenziell Methodenaufrufe `FireEvent` für `Exited` und `Entered` -Übertragung von einer Sicht in eine andere. Allerdings der Finger möglicherweise wurden verschoben zu einem Bereich von einer Sicht ohne eine angefügte belegt `TouchEffect`, oder aus diesem Bereich einer Ansicht mit dem Effekt angefügt.

Beachten Sie, dass die `try` und `catch` blockieren, wenn die Sicht zugegriffen wird. Auf einer Seite, die navigiert wird, und dann zurück zur Startseite, navigiert die `OnDetached` -Methode nicht aufgerufen wird und Elemente verbleiben im die `viewDictionary` aber Android betrachtet werden verworfen.

### <a name="the-ios-implementation"></a>Die iOS-Implementierung

Die iOS-Implementierung ist die Android-Implementierung mit der Ausnahme, die mit dem iOS `TouchEffect` Instanziieren der Klasse muss eine Ableitung von `UIGestureRecognizer`. Dies ist eine Klasse in der iOS-Projekt mit dem Namen `TouchRecognizer`. Diese Klasse verwaltet zwei statische Wörterbücher, in denen gespeichert `TouchRecognizer` Instanzen:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Die Struktur dieser Anteil `TouchRecognizer` -Klasse ähnelt der Android `TouchEffect` Klasse.

## <a name="putting-the-touch-effect-to-work"></a>Wenn Sie die Fingereingabe Auswirkungen auf Arbeit

Die [ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) Programm enthält fünf Seiten, die den Touch-Tracking-Effekt für allgemeine Aufgaben zu testen.

Die **BoxView ziehen** Seite ermöglicht Ihnen das Hinzufügen `BoxView` Elemente, die eine `AbsoluteLayout` und ziehen Sie sie auf dem Bildschirm. Die [XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) instanziiert zwei `Button` Ansichten für das Hinzufügen von `BoxView` Elemente, die die `AbsoluteLayout` und das Löschen der `AbsoluteLayout`.

Die Methode in der [Code-Behind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) , addiert ein neues `BoxView` auf die `AbsoluteLayout` fügt auch eine `TouchEffect` -Objekt an die `BoxView` und fügt einen Ereignishandler für den Effekt:

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

Die `TouchAction` Ereignishandler verarbeitet die Berührungsereignisse für alle der `BoxView` Elemente, aber sie muss einige Vorsicht walten lassen: Es darf keine zwei Fingern auf einem einzelnen zulassen `BoxView` daran, dass das Programm nur implementiert ziehen und würden zwei Finger behindern Sie einander. Aus diesem Grund definiert die Seite eine eingebettete Klasse für jeden Finger derzeit nachverfolgt wird:

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

Die `Pressed` Touch Aktion dieses Wörterbuch ein Element hinzufügt und die `Released` Aktion entfernt. Die `Pressed` Logik muss überprüfen, ob es bereits ein Element im Wörterbuch für diese ist `BoxView`. Wenn dies der Fall ist, die `BoxView` ist bereits gezogen werden und das neue Ereignis ist eine zweite finger auf, die gleiche `BoxView`. Für die `Moved` und `Released` Aktionen der Ereignishandler muss überprüfen, ob das Wörterbuch einen Eintrag für diesen hat `BoxView` und einem `Id` -Eigenschaft für, das gezogen `BoxView` in Wörterbucheintrags entspricht:

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

Die `Pressed` Logik legt die `Capture` Eigenschaft von der `TouchEffect` -Objekt `true`. Dies entspricht der alle nachfolgenden Ereignisse für diese Finger an denselben Ereignishandler übermittelt.

Die `Moved` Logik wechselt die `BoxView` durch Ändern der `LayoutBounds` -Eigenschaft. Die `Location` -Eigenschaft der Ereignisargumente ist immer relativ zum der `BoxView` gezogen wird, und, wenn die `BoxView` mit einer Konstante Rate gezogen wird die `Location` Eigenschaften der aufeinander folgenden Ereignisse werden ungefähr gleich. Z. B. wenn ein Finger drückt die `BoxView` in der Mitte der `Pressed` Aktion speichert eine `PressPoint` Eigenschaft (50, 50), die unverändert auf nachfolgende Ereignisse. Wenn die `BoxView` diagonal mit einer Konstante, die nachfolgenden Rate gezogen wird `Location` Eigenschaften während der `Moved` Werte handeln (55, 55), in diesem Fall die `Moved` Logik die horizontale und vertikale Position des der 5hinzugefügt`BoxView`. Auf diese Weise verschoben der `BoxView` , damit ihren Mittelpunkt wieder ist direkt unter dem Finger.

Sie können mehrere verschieben `BoxView` Elemente, die gleichzeitig mit anderen Fingern fest.

[![](touch-tracking-images/boxviewdragging-small.png "Dreifacher Screenshot der Seite BoxView ziehen")](touch-tracking-images/boxviewdragging-large.png#lightbox "dreifacher Screenshot der Seite BoxView ziehen")

### <a name="subclassing-the-view"></a>Erstellen von Unterklassen für die Sicht

Häufig ist es einfacher, für ein Element Xamarin.Forms, um eine eigene Touch-Ereignisse zu behandeln. Die **Draggable BoxView ziehen** Seite funktioniert genauso wie die **BoxView ziehen** Seite, jedoch die Elemente, die der Benutzer zieht Instanzen sind eine [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) von abgeleitete Klasse `BoxView`:

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

Der Konstruktor erstellt und fügt die `TouchEffect`, und legt die `Capture` Eigenschaft, wenn dieses Objekt zuerst instanziiert wird. Kein Wörterbuch ist erforderlich, da die Klasse selbst speichert `isBeingDragged`, `pressPoint`, und `touchId` jeden Finger zugeordnete Werte. Die `Moved` Behandlung ändert die `TranslationX` und `TranslationY` Eigenschaften, sodass die Logik ausgeführt wird auch dann, wenn das übergeordnete Element des der `DraggableBoxView` kein `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>Integrieren von SkiaSharp

Die nächsten beiden Demos erfordern, Grafiken und können die SkiaSharp für diesen Zweck. Möglicherweise möchten Sie weitere Informationen zu [verwenden SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) , bevor Sie diese Beispiele zu untersuchen. Die ersten beiden Artikel ("SkiaSharp Zeichnung Grundlagen zu" und "SkiaSharp Linien und Pfade") werden alle Elemente, die Sie hier müssen behandelt.

Die **Ellipse Zeichnung** Seite können Sie eine Ellipse, die durch ein Lesegerät Finger auf dem Bildschirm gezeichnet werden soll. Je nachdem, wie Sie den Finger verschieben, können Sie die Ellipse, die von der linken oberen Ecke auf der unteren rechten Ecke oder von einer beliebigen anderen Ecke in die entgegengesetzte Ecke zeichnen. Die Ellipse wird mit einem zufälligen Farbe und Deckkraft gezeichnet.

[![](touch-tracking-images/ellipsedrawing-small.png "Dreifacher Screenshot der Seite Ellipse Zeichnung")](touch-tracking-images/ellipsedrawing-large.png#lightbox "dreifacher Screenshot der Seite Ellipse Zeichnung")

Wenn Sie eine Schaltfläche mit den Auslassungszeichen klicken Sie dann berühren können Sie es an eine andere Position ziehen. Dies erfordert eine Technik, bekannt als "Treffertests," der auch für das Objekt zu einem bestimmten Zeitpunkt zu suchen. Schaltfläche mit die Auslassungszeichen SkiaSharp sind nicht Xamarin.Forms-Elemente, deshalb können sie ihre eigenen ausführen `TouchEffect` verarbeiten. Die `TouchEffect` müssen gelten für die gesamte `SKCanvasView` Objekt.

Die [EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) Datei instanziiert den `SKCanvasView` in einer einzelnen Zelle `Grid`. Die `TouchEffect` -Objekt an, die angefügt wird `Grid`:

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

In Android und universellen Windows-Plattform die `TouchEffect` können direkt angefügt werden, die `SKCanvasView`, jedoch unter iOS, das nicht funktioniert. Beachten Sie, dass die `Capture` -Eigenschaftensatz auf `true`.

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

Die `StartPoint` und `EndPoint` Eigenschaften werden verwendet, wenn das Programm Fingereingabe; verarbeitet der `Rectangle` Eigenschaft wird zum Zeichnen der Ellipse. Die `LastFingerLocation` Eigenschaft kommt es bei die Ellipse gezogen wird, und die `IsInEllipse` Methode unterstützt die Internetsuche Treffertests. Die Methode gibt `true` ist der Punkt innerhalb der Ellipse.

Die [Code-Behind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) drei Auflistungen verwaltet:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

Die `draggingFigure` Wörterbuch enthält eine Teilmenge der `completedFigures` Auflistung. Die SkiaSharp `PaintSurface` Ereignishandler einfach die Objekte in diesen rendert die `completedFigures` und `inProgressFigures` Sammlungen:

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

Der schwierigste Teil der Touch-Verarbeitung ist der `Pressed` behandeln. Dies ist, wenn der Treffertest ausgeführt wird, aber wenn der Code eine Ellipse, die unter den Finger wieder an den Benutzer erkennt, diese Ellipse nur gezogen werden kann, wenn es derzeit nicht von einem anderen Finger gezogen wird. Ist keine Ellipse unter den Finger wieder an den Benutzer, beginnt der Code den Prozess der Zeichnen einer neue Ellipse:

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

Das andere SkiaSharp-Beispiel ist die **Finger Paint** Seite. Sie können eine Konturfarbe und Strichbreite auswählen, aus zwei `Picker` anzeigt, und zeichnen Sie dann mit einem oder mehreren Fingern:

[![](touch-tracking-images/fingerpaint-small.png "Dreifacher Screenshot der Seite Finger Paint")](touch-tracking-images/fingerpaint-large.png#lightbox "dreifacher Screenshot der Seite Finger Paint")

Dieses Beispiel benötigen Sie auch eine separate Klasse zum Darstellen von jeder Zeile auf dem Bildschirm gezeichnet:

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

Ein `SKPath` Objekt wird verwendet, um jede Zeile zu rendern. Die [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) Datei verwaltet zwei Sammlungen dieser Objekte, für die aktuell gezeichnete Polylinien und ein anderes für die abgeschlossenen Polylinien:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Die `Pressed` Verarbeitung erstellt ein neues `FingerPaintPolyline`, Aufrufe `MoveTo` auf die Pfadobjekt, das den ersten Punkt gespeichert und fügt dieses Objekt an die `inProgressPolylines` Wörterbuch. Die `Moved` Verarbeitung Aufrufe `LineTo` auf das Pfadobjekt mit der neuen Finger Position und die `Released` Verarbeitung überträgt die abgeschlossene Polylinie aus `inProgressPolylines` auf `completedPolylines`. Wiederum ist die tatsächliche SkiaSharp Zeichencode relativ einfach:

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

### <a name="tracking-view-to-view-touch"></a>Nachverfolgen von Touch Ansicht

Die vorherigen Beispielen festgelegt haben die `Capture` Eigenschaft von der `TouchEffect` auf `true`, entweder bei der `TouchEffect` erstellt wurde oder wenn die `Pressed` Ereignis aufgetreten ist. Dadurch wird sichergestellt, dass das gleiche Element empfängt alle Ereignisse im Zusammenhang mit den Finger wieder an, der die Sicht zuerst gedrückt. Das letzte Beispiel ruft *nicht* festgelegt `Capture` auf `true`. Dies bewirkt, dass anderes Verhalten, wenn ein Finger mit dem Bildschirm verbunden von einem Element in einen anderen bewegt wird. Das Element, das der Finger verliert empfängt ein Ereignis mit einer `Type` -Eigenschaftensatz auf `TouchActionType.Exited` und das zweite Element empfängt ein Ereignis mit einer `Type` Festlegen von `TouchActionType.Entered`.

Diese Art der Verarbeitung von Touch ist sehr nützlich für Musik Tastatur. Ein Schlüssel sollten erkennen, wenn er, sondern auch gedrückt wird, wenn ein Finger von einem Schlüssel zu einem anderen Folien sein.

Die **automatische Tastatur** Seite definiert kleine [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) und [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) abgeleitete Klassen [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs), die sich daraus ableitet `BoxView`.

Die `Key` Klasse ist jetzt in einem tatsächlichen Musikprogramm verwendet werden. Öffentliche Eigenschaften namens definiert `IsPressed` und `KeyNumber`, die auf den Tastencode, durch die der MIDI-Standard festgelegt werden soll. Die `Key` -Klasse definiert außerdem ein Ereignis namens `StatusChanged`, welche wird aufgerufen, wenn die `IsPressed` -Eigenschaft ändert.

Mehrere Finger sind für jeden Schlüssel zulässig. Aus diesem Grund die `Key` -Klasse verwaltet eine `List` von Touch ID-Nummern aller derzeit berühren diesen Schlüssel Finger:

```csharp
List<long> ids = new List<long>();
```

Der `TouchAction` -Ereignishandler fügt eine ID, die `ids` Liste für beide eine `Pressed` -Ereignis des Typs und ein `Entered` aufweisen, aber nur, wenn der `IsInContact` Eigenschaft ist `true` für die `Entered` Ereignis. Die ID wird daraus der `List` für eine `Released` oder `Exited` Ereignis:

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

Die `AddToList` und `RemoveFromList` beide Methoden überprüft, ob die `List` zwischen leer und nicht leer ist, geändert wurde und wenn dies der Fall ist, ruft der `StatusChanged` Ereignis.

Die verschiedenen `WhiteKey` und `BlackKey` Elemente werden auf der Seite angeordnet [XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), dem sieht am besten des Telefons in einer im Querformat aktiviert wird:

[![](touch-tracking-images/silentkeyboard-small.png "Dreifacher Screenshot der Seite Automatische Tastatur")](touch-tracking-images/silentkeyboard-large.png#lightbox "dreifacher Screenshot der Seite Automatische Tastatur")

Wenn Sie über die Schlüssel den Finger sweep, wird durch die geringfügigen Änderungen in Farbe angezeigt, dass die Touch-Ereignisse von einem Schlüssel zu einem anderen übertragen werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Ereignisse in einen Effekt aufgerufen und zum Schreiben und verwenden einen Effekt, der auf niedriger Ebene Multitouch-Verarbeitung implementiert.


## <a name="related-links"></a>Verwandte Links

- [Nachverfolgen von Multi-Touch Finger in iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Multi-Touch-Finger in Android nachverfolgen](~/android/app-fundamentals/touch/touch-tracking.md)
- [Touch-Tracking wirksam (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
