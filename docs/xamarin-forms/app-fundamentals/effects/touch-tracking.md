---
title: Aufrufen von Ereignissen über Effekte
description: Effekte können Ereignisse definieren und auslösen, die Änderungen in der zugrunde liegenden nativen Ansicht signalisieren. In diesem Artikel wird beschrieben, wie Sie eine umfassende Multitouchverfolgung implementieren und Ereignisse generieren, die eine Touchaktivität signalisieren.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 892bffa4027a1a61d6c22cc26d1556fb007432d8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136980"
---
# <a name="invoking-events-from-effects"></a>Aufrufen von Ereignissen über Effekte

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)

_Effekte können Ereignisse definieren und auslösen, die Änderungen in der zugrunde liegenden nativen Ansicht signalisieren. In diesem Artikel wird beschrieben, wie Sie eine umfassende Multitouchverfolgung implementieren und Ereignisse generieren, die eine Touchaktivität signalisieren._

Der in diesem Artikel beschriebene Effekt ermöglicht den Zugriff auf umfassende Touchereignisse. Diese umfassenden Ereignisse sind zwar nicht über die bereits vorhandenen `GestureRecognizer`-Klassen verfügbar, aber sie sind für einige Arten von Anwendungen sehr bedeutend. Beispielsweise muss eine Anwendung, bei der mit den Fingern gemalt wird, einzelne Fingerbewegungen auf dem Bildschirm nachverfolgen können. Eine Musiktastatur muss erkennen können, wenn einzelne Tasten angetippt und wieder losgelassen werden bzw. wenn ein Finger über die Tasten gleitet (Glissando).

Effekte eignen sich besonders gut für die Multitouchverfolgung, da sie an jedes beliebige Xamarin.Forms-Element angefügt werden können.

## <a name="platform-touch-events"></a>Touchereignisse auf den verschiedenen Plattformen

Sowohl iOS als auch Android und die Universelle Windows-Plattform (UWP) beinhalten eine umfassende API, mit der Anwendungen Touchaktivitäten erkennen können. Diese Plattformen unterscheiden alle zwischen drei grundlegenden Arten von Touchereignissen:

- *Gedrückt*, wenn ein Finger den Bildschirm berührt
- *Moved* (Bewegt), wenn sich ein Finger auf dem Bildschirm bewegt
- *Losgelassen*, wenn die Berührung durch den Finger beendet wird

In einer Multitouchumgebung können mehrere Finger gleichzeitig den Bildschirm berühren. Die verschiedenen Plattformen umfassen eine Identifikationsnummer (ID), die die Anwendungen verwenden können, um zwischen mehreren Fingern zu unterscheiden.

Unter iOS definiert die `UIView`-Klasse drei überschreibbare Methoden, die den drei grundlegenden Ereignissen entsprechen: `TouchesBegan`, `TouchesMoved` und `TouchesEnded`. Im Artikel [Multi-Touch Finger Tracking (Multitouchverfolgung)](~/ios/app-fundamentals/touch/touch-tracking.md) wird erläutert, wie diese Methoden verwendet werden können. Allerdings müssen iOS-Programme Klassen, die von `UIView` abgeleitet werden, nicht überschreiben, um diese Methoden verwenden zu können. Der iOS-`UIGestureRecognizer` definiert dieselben drei Methoden, und Sie können eine Instanz einer Klasse, die von `UIGestureRecognizer` abgeleitet wird, an jedes beliebige `UIView`-Objekt anfügen.

Unter Android definiert die `View`-Klasse eine überschreibbare Methode mit dem Namen `OnTouchEvent`, um sämtliche Touchaktivitäten zu verarbeiten. Der Typ der Touchaktivität wird wie im Artikel [Multi-Touch Finger Tracking (Multitouchverfolgung)](~/android/app-fundamentals/touch/touch-tracking.md) von den Enumerationsmembern `Down`, `PointerDown`, `Move`, `Up` und `PointerUp` definiert. Außerdem definiert die Android-`View` ein Ereignis mit dem Namen `Touch`, durch das ein Ereignishandler an jedes beliebige `View`-Objekt angefügt werden kann.

Auf der UWP definiert die `UIElement`-Klasse die folgenden Ereignisse: `PointerPressed`, `PointerMoved` und `PointerReleased`. Diese Ereignisse werden im MSDN-Artikel [Handle Pointer Input (Zeigereingabe)](/windows/uwp/input-and-devices/handle-pointer-input/) und in der API-Dokumentation für die [`UIElement`](/uwp/api/windows.ui.xaml.uielement/)-Klasse beschrieben.

Die `Pointer`-API auf der UWP soll Maus-, Touch- und Stifteingaben vereinheitlichen. Aus diesem Grund wird das Ereignis `PointerMoved` ausgelöst, wenn die Maus zwar über ein Element bewegt, aber keine Taste gedrückt wird. Das `PointerRoutedEventArgs`-Objekt, das im Zusammenhang mit diesen Ereignissen verwendet wird, umfasst eine Eigenschaft mit dem Namen `Pointer`, die wiederum eine Eigenschaft mit dem Namen `IsInContact` beinhaltet, die angibt, ob eine Maustaste gedrückt wird oder ein Finger den Bildschirm berührt.

Zudem definiert die UWP zwei weitere Ereignisse: `PointerEntered` und `PointerExited`. Diese Ereignisse geben an, ob sich eine Maus oder ein Finger von einem Element zu einem anderen bewegt. Betrachten Sie z. B. die beiden angrenzenden Elemente A und B. Beide Elemente verfügen über installierte Handler für die Zeigerereignisse. Wenn mit einem Finger auf A gedrückt wird, wir das Ereignis `PointerPressed` ausgelöst. Wenn sich der Finger bewegt, löst A `PointerMoved`-Ereignisse aus. Wenn sich der Finger von A nach B bewegt, löst A ein `PointerExited`-Ereignis und B ein `PointerEntered`-Ereignis aus. Wenn der Finger den Bildschirm dann nicht mehr berührt, löst B ein `PointerReleased`-Ereignis aus.

Die iOS- und Android-Plattformen unterscheiden sich von der UWP darin, dass die Ansicht, die als erstes den Aufruf von `TouchesBegan` oder `OnTouchEvent` abruft, wenn ein Finger die Ansicht berührt, auch weiter alle Touchaktivitäten erhält, wenn der Finger sich zu anderen Ansichten bewegt. Die UWP kann ein ähnliches Verhalten aufweisen, wenn die Anwendung den Zeiger erfasst. Dann ruft das Element zuerst im Ereignishandler `PointerEntered``CapturePointer` auf und ruft anschließend alle Touchaktivitäten ab, die durch diesen Finger ausgelöst werden.

Für einige Anwendungstypen wie die Musiktastatur ist der UWP-Ansatz sehr nützlich. Jede Taste kann Touchereignisse verarbeiten und anhand der Ereignisse `PointerEntered` und `PointerExited` erkennen, wenn ein Finger von einer Taste zu einer anderen gleitet.

Aus diesem Grund implementiert der in diesem Artikel beschriebene Touchverfolgungseffekt den UWP-Ansatz.

## <a name="the-touch-tracking-effect-api"></a>Die API für den Touchverfolgungseffekt

Die [**Touch Tracking Effect Demos (Demos zum Touchverfolgungseffekt)** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) umfassen die Klassen (und eine Enumeration), die die umfassende Touchverfolgung implementieren. Diese Typen gehören zum Namespace `TouchTracking` und beginnen mit dem Wort `Touch`. Das **TouchTrackingEffectDemos**-Projekt für eine .NET Standard-Bibliothek umfasst die `TouchActionType`-Enumeration für den Touchereignistyp:

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

Alle Plattformen umfassen außerdem ein Ereignis, das angibt, dass das Touchereignis abgebrochen wurde.

Die `TouchEffect`-Klasse in der .NET Standard-Bibliothek wird von `RoutingEffect` abgeleitet und definiert ein Ereignis mit dem Namen `TouchAction` sowie eine Methode mit dem Namen `OnTouchAction`, die das Ereignis `TouchAction` auslöst:

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

Beachten Sie außerdem die `Capture`-Eigenschaft. Damit Touchereignisse erfasst werden können, muss die Anwendung diese Eigenschaft auf `true` festlegen, bevor ein `Pressed`-Ereignis ausgelöst wird. Andernfalls weisen die Touchereignisse dasselbe Verhalten auf wie auf der Universellen Windows-Plattform.

Die `TouchActionEventArgs`-Klasse in der .NET Standard-Bibliothek enthält alle Informationen im Zusammenhang mit den einzelnen Ereignissen:

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

Die Anwendung kann mithilfe der `Id`-Eigenschaft einzelne Finger nachverfolgen. Beachten Sie die `IsInContact`-Eigenschaft. Diese ist für `Pressed`-Ereignisse immer auf `true` und für `Released`-Ereignisse immer auf `false` festgelegt. Genauso sind `Moved`-Ereignisse unter iOS und Android immer auf `true` festgelegt. Die `IsInContact`-Eigenschaft wird möglicherweise für `Moved`-Ereignisse auf der Universellen Windows-Plattform auf `false` festgelegt, wenn das Programm auf dem Desktop ausgeführt wird und sich der Mauszeiger bewegt, aber keine Taste gedrückt wird.

Sie können die `TouchEffect`-Klasse in Ihren eigenen Anwendungen verwenden, indem Sie die Datei in das .NET Standard-Bibliotheksprojekt in der Projektmappe einfügen und der `Effects`-Collection eines beliebigen Xamarin.Forms-Elements eine Instanz hinzufügen. Fügen Sie einen Handler an das `TouchAction`-Ereignis an, um die Touchereignisse abzurufen.

Damit Sie den `TouchEffect` in Ihrer eigenen Anwendung verwenden können, benötigen Sie die Plattformimplementierungen, die in der Projektmappe **TouchTrackingEffectDemos** enthalten sind.

## <a name="the-touch-tracking-effect-implementations"></a>Implementierungen des Touchverfolgungseffekts

Nachfolgend wird die Implementierung des `TouchEffect` für iOS, Android und die UWP beschrieben. Zunächst erhalten Sie Informationen über die einfachste Implementierung (auf der UWP), und als Letztes wird die Implementierung unter iOS beschrieben, da deren Struktur am komplexesten ist.

### <a name="the-uwp-implementation"></a>Die UWP-Implementierung

Am einfachsten ist die Implementierung von `TouchEffect` auf der UWP. In der Regel wird die Klasse von `PlatformEffect` abgeleitet und umfasst zwei Assemblyattribute:

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

Die Überschreibung `OnAttached` speichert einige Informationen als Felder und fügt allen Zeigerereignissen Handler an:

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

Der `OnPointerPressed`-Handler löst das Effektereignis aus, indem er das `onTouchAction`-Feld in der `CommonHandler`-Methode aufruft:

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

Außerdem überprüft `OnPointerPressed` den Wert der `Capture`-Eigenschaft in der Effektklasse der .NET Standard-Bibliothek und ruft den `CapturePointer` auf, wenn er auf `true` festgelegt ist.

 Die Verwendung der anderen UWP-Ereignishandler ist sogar noch einfacher:

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

Die Implementierung ist unter Android und iOS zwangsläufig komplexer, weil die Ereignisse `Exited` und `Entered` implementiert werden müssen, wenn sich ein Finger vom einen zum anderen Element bewegt. Beide Implementierungen sind ähnlich aufgebaut.

Die Android-Klasse `TouchEffect` installiert einen Handler für das Ereignis `Touch`:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

Außerdem definiert die Klasse zwei statische Wörterbücher:

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

Dem `viewDictionary` wird bei jedem Aufruf der Überschreibung `OnAttached` ein neuer Eintrag hinzugefügt:

```csharp
viewDictionary.Add(view, this);
```

In `OnDetached` wird der Eintrag aus dem Wörterbuch entfernt. Jede Instanz von `TouchEffect` wird einer bestimmten Ansicht zugeordnet, an die der Effekt angefügt ist. Mithilfe des statischen Wörterbuchs können alle `TouchEffect`-Instanzen alle anderen Ansichten und die zugehörigen `TouchEffect`-Instanzen durchlaufen. Dies ist notwendig, damit die Ereignisse von einer Ansicht auf eine andere übertragen werden können.

Android weist Touchereignissen einen ID-Code zu, über den Anwendungen einzelne Finger verfolgen können. Das `idToEffectDictionary` weist diesen ID-Code einer `TouchEffect`-Instanz zu. Dem Wörterbuch wird ein Element hinzugefügt, wenn der `Touch`-Handler durch den Druck eines Fingers aufgerufen wird:

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

Das Element wird aus dem `idToEffectDictionary` entfernt, wenn der Finger den Bildschirm nicht mehr berührt. Die `FireEvent`-Methode fasst nur alle Informationen zusammen, die zum Aufrufen der `OnTouchAction`-Methode benötigt wird:

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

Alle anderen Touchtypen werden auf zwei verschiedene Weisen verarbeitet: Wenn die `Capture`-Eigenschaft auf `true` festgelegt ist, ist das Touchereignis eine recht einfache Übersetzung der `TouchEffect`-Informationen. Komplizierter wird es, wenn die `Capture`-Eigenschaft auf `false` festgelegt ist, weil dann die Touchereignisse möglicherweise von einer Ansicht auf eine andere verschoben werden müssen. Für diesen Vorgang ist die `CheckForBoundaryHop`-Methode zuständig, die bei Ereignissen ausgelöst werden, bei denen Elemente verschoben werden. Sie verwendet beide statischen Wörterbücher. Sie durchläuft das `viewDictionary`, um die Ansicht zu ermitteln, die der Finger gerade berührt, und verwendet `idToEffectDictionary`, um die aktuelle `TouchEffect`-Instanz (und daher die aktuelle Ansicht) zu speichern, die einer bestimmten ID zugeordnet ist:

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

Wenn am `idToEffectDictionary` eine Änderung vorgenommen wird, ruft die Methode möglicherweise `FireEvent` für `Exited` und `Entered` auf, um die Daten einer Ansicht zu einer anderen zu übertragen. Es kann allerdings sein, dass der Finger in einen Bereich bewegt wurde, der von einer anderen Ansicht besetzt ist, der kein `TouchEffect` angefügt ist, oder dass er von diesem Bereich zu einer Ansicht bewegt wurde, der der Effekt angefügt ist.

Beachten Sie die Blöcke `try` und `catch`, wenn auf die Ansicht zugegriffen wird. Auf einer Seite, auf der erst zu diesen Blöcken und dann zurück zur Startseite navigiert wird, wird die `OnDetached`-Methode nicht aufgerufen, und die Elemente verbleiben im `viewDictionary`, obwohl Android sie als gelöscht ansieht.

### <a name="the-ios-implementation"></a>Die iOS-Implementierung

Die iOS-Implementierung ähnelt der Implementierung unter Android, allerdings muss die iOS-Klasse `TouchEffect` eine Ableitung von `UIGestureRecognizer` instanziieren. Es handelt sich dabei um eine Klasse im iOS-Projekt mit dem Namen `TouchRecognizer`. Sie verwaltet zwei statische Wörterbücher, in denen `TouchRecognizer`-Instanzen gespeichert sind:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Die `TouchRecognizer`-Klasse ähnelt der Android-Klasse `TouchEffect` weitestgehend.

> [!IMPORTANT]
> Bei vielen der Ansichten in `UIKit` ist der Toucheffekt nicht standardmäßig aktiviert. Der Toucheffekt kann aktiviert werden, indem `view.UserInteractionEnabled = true;` zur Überschreibung `OnAttached` in der `TouchEffect`-Klasse im iOS-Projekt hinzugefügt wird. Dies sollte nach dem Abrufen der `UIView` geschehen, die dem Element entspricht, dem der Effekt angefügt wird.

## <a name="putting-the-touch-effect-to-work"></a>Einsetzen des Toucheffekts

Das [**TouchTrackingEffectDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)-Programm enthält fünf Seiten, die den Touchverfolgungseffekt auf häufig auftretende Tasks untersuchen.

Über die Seite **BoxView Dragging** (BoxView ziehen) können Sie `BoxView`-Elemente zu einem `AbsoluteLayout` hinzufügen und anschließend an eine andere Position auf dem Bildschirm ziehen. Die [XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BoxViewDraggingPage.xaml) instanziiert zwei `Button`-Ansichten zum Hinzufügen von `BoxView`-Elementen zum `AbsoluteLayout` und zum Löschen des `AbsoluteLayout`.

Die Methode in der [CodeBehind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BoxViewDraggingPage.xaml.cs), die eine neue `BoxView` zum `AbsoluteLayout` hinzufügt, fügt auch ein `TouchEffect`-Objekt zur `BoxView` hinzu und fügt einen Ereignishandler an den Effekt an:

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

Der Ereignishandler `TouchAction` verarbeitet zwar alle Touchereignisse für alle `BoxView`-Elemente, muss dabei aber vorsichtig sein: Er kann nicht zwei Finger auf einer `BoxView` zulassen, da das Programm nur Ziehbewegungen implementiert und die beiden Finger einander beeinträchtigen würden. Aus diesem Grund definiert die Seite eine eingebettete Klasse für jeden Finger, der gerade verfolgt wird:

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

Das `dragDictionary` enthält einen Eintrag für jede `BoxView`, die gerade an eine andere Stelle gezogen wird.

Die `Pressed`-Touchaktion fügt diesem Wörterbuch ein Element hinzu, das von der `Released`-Aktion wieder entfernt wird. Die `Pressed`-Logik muss überprüfen, ob für diese `BoxView` bereits ein Element im Wörterbuch enthalten ist. Wenn dies der Fall ist, wird die `BoxView` bereits an einen anderen Punkt gezogen, und das neue Ereignis wird durch einen zweiten Finger auf dieser `BoxView` ausgelöst. Für die Aktionen `Moved` und `Released` muss der Ereignishandler überprüfen, ob es für diese `BoxView` einen Eintrag im Wörterbuch gibt und ob die Toucheigenschaft `Id` für diese an einen anderen Punkt gezogene `BoxView` mit der im Wörterbucheintrag übereinstimmt:

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

Die `Pressed`-Logik legt die `Capture`-Eigenschaft des `TouchEffect`-Objekts auf `true` fest. Dadurch werden alle Folgeereignisse für diesen Finger an diesen Ereignishandler übermittelt.

Die `Moved`-Logik verschiebt die `BoxView`, indem sie die angefügte `LayoutBounds`-Eigenschaft ändert. Die `Location`-Eigenschaft des Ereignisarguments steht immer in Verbindung mit der `BoxView`, die an einen anderen Punkt gezogen wird, und wenn der Ziehvorgang für die `BoxView` bei konstanter Geschwindigkeit ausgeführt wird, sind die `Location`-Eigenschaften der Folgeereignisse in etwa gleich. Wenn z. B. ein Finger auf die Mitte der `BoxView` drückt, speichert die `Pressed`-Aktion eine `PressPoint`-Eigenschaft von (50, 50), die sich für die Folgeereignisse nicht ändert. Wenn die `BoxView` bei gleichbleibender Geschwindigkeit diagonal an eine andere Stelle gezogen wird, weisen die nachfolgenden `Location`-Eigenschaften während der `Moved`-Aktion möglicherweise Werte von (55, 55) auf. Wenn dies der Fall ist, fügt die `Moved`-Logik zu der horizontalen und der vertikalen Position der `BoxView` den Wert 5 hinzu. Dadurch wird die `BoxView` so verschoben, dass sich ihre Mitte wieder genau unter dem Finger befindet.

Sie können mehrere `BoxView`-Elemente gleichzeitig mit mehreren Fingern verschieben.

[![](touch-tracking-images/boxviewdragging-small.png "Triple screenshot of the BoxView Dragging page")](touch-tracking-images/boxviewdragging-large.png#lightbox "Triple screenshot of the BoxView Dragging page")

### <a name="subclassing-the-view"></a>Erstellen von Unterklassen für die Ansicht

Häufig ist es für Xamarin.Forms-Elemente einfacher, wenn sie ihre eigenen Touchereignisse verarbeiten. Die Seite **Draggable BoxView Dragging** (Ziehbare BoxView ziehen) funktioniert zwar genauso wie die Seite **BoxView Dragging** (BoxView ziehen), aber die Elemente, die der Benutzer an eine andere Position zieht, sind Instanzen einer [`DraggableBoxView`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/DraggableBoxView.cs)-Klasse, die von `BoxView` abgeleitet wird:

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

Der Konstruktor erstellt das `TouchEffect`, fügt dieses an und legt die `Capture`-Eigenschaft fest, wenn das Objekt zum ersten Mal instanziiert wird. Es ist kein Wörterbuch erforderlich, weil die Klasse selbst die `isBeingDragged`-, `pressPoint`- und `touchId`-Werte speichert, die jedem Finger zugeordnet werden können. Die Behandlung von `Moved` ändert die Eigenschaften `TranslationX` und `TranslationY`, damit die Logik auch funktioniert, wenn das übergeordnete Element der `DraggableBoxView` kein `AbsoluteLayout` ist.

### <a name="integrating-with-skiasharp"></a>Integration in SkiaSharp

Für die nächsten beiden Demos benötigen Sie Grafiken. Dafür wird hier SkiaSharp verwendet. Bevor Sie sich diese Beispiele ansehen, sollten Sie sich über die [Verwendungsweise von SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) informieren. In den ersten beiden Artikeln („SkiaSharp Drawing Basics“ (Zeichnungsgrundlagen für SkiaSharp) und „SkiaSharp Lines and Paths“ („Zeilen und Pfade mit SkiaSharp“) finden Sie alle Informationen, die Sie für die hier erläuterten Beispiele benötigen.

Über die Seite **Ellipse Drawing** (Ellipse zeichnen) können Sie eine Ellipse zeichnen, indem Sie mit Ihrem Finger über den Bildschirm wischen. Je nachdem, wie Sie Ihren Finger bewegen, können Sie die Ellipse von oben links nach unten rechts oder von einer beliebigen Ecke bis hin zur gegenüberliegende Ecke zeichnen. Die Farbe und Deckkraft der Ellipse können beliebig ausgewählt werden.

[![](touch-tracking-images/ellipsedrawing-small.png "Triple screenshot of the Ellipse Drawing page")](touch-tracking-images/ellipsedrawing-large.png#lightbox "Triple screenshot of the Ellipse Drawing page")

Wenn Sie eine der Ellipsen berühren, können Sie sie an einen anderen Ort ziehen. Diese Technik wird als „hit-testing“ (Treffertest) bezeichnet und umfasst die Suche nach einem grafischen Objekt an einem bestimmten Punkt. Da es sich bei den SkiaSharp-Ellipsen nicht um Xamarin.Forms-Elemente handelt, können diese den `TouchEffect` nicht selbst verarbeiten. Der `TouchEffect` muss für das gesamte `SKCanvasView`-Objekt gelten.

Die [EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/EllipseDrawingPage.xaml)-Datei instanziiert die `SKCanvasView` in einem `Grid` mit einer Zelle. Das `TouchEffect`-Objekt wird an dieses `Grid` angefügt:

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

Unter Android und auf der UWP kann der `TouchEffect` direkt an die `SKCanvasView` angefügt werden. Unter iOS ist dies hingegen nicht möglich. Beachten Sie, dass die `Capture`-Eigenschaft auf `true` festgelegt ist.

Jede Ellipse, die SkiaSharp rendert, wird von einem Objekt vom Typ `EllipseDrawingFigure` dargestellt:

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

Die Eigenschaften `StartPoint` und `EndPoint` werden verwendet, wenn das Programm eine Toucheingabe verarbeitet. Die `Rectangle`-Eigenschaft wird zum Zeichnen der Ellipse verwendet. Die `LastFingerLocation`-Eigenschaft kommt ins Spiel, wenn die Ellipse an eine andere Position gezogen wird und die `IsInEllipse`-Methode beim Treffertest hilft. Die Methode gibt `true` zurück, wenn sich der Punkt innerhalb der Ellipse befindet.

Die [CodeBehind-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/EllipseDrawingPage.xaml.cs) verwaltet drei Collections:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

Das `draggingFigure`-Wörterbuch enthält eine Teilmenge der `completedFigures`-Collection. Der `PaintSurface`-Ereignishandler für SkiaSharp rendert nur die Objekte in den Collections `completedFigures` und `inProgressFigures`:

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

Der komplizierteste Teil der Touchverarbeitung ist die `Pressed`-Verarbeitung. In diesem Zusammenhang wird zwar der Treffertest durchgeführt, aber wenn der Code eine Ellipse unter dem Finger des Benutzers erkennt, kann diese nur an eine andere Position gezogen werden, wenn sie nicht gleichzeitig mit einem anderen Finger verschoben wird. Wenn sich keine Ellipse unter dem Finger des Benutzers befindet, beginnt der Code mit dem Zeichnen einer neuen Ellipse:

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

Das andere SkiaSharp-Beispiel stellt die Seite **Finger Paint** (Mit den Fingern malen) dar. Sie können über die beiden `Picker`-Ansichten eine Strichfarbe und -stärke auswählen und dann mit mindestens zwei Fingern zeichnen:

[![](touch-tracking-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](touch-tracking-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

Dieses Beispiel erfordert außerdem eine separate Klasse, um die einzelnen Linien darzustellen, die auf den Bildschirm gemalt werden:

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

Zum Rendern der einzelnen Zeilen wird ein `SKPath`-Objekt verwendet. Die [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/FingerPaintPage.xaml.cs)-Datei verwaltet zwei Collections dieser Objekte: eine für die Polylinien die gerade gezeichnet werden und eine für bereits fertige Polylinien:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Bei der Verarbeitung von `Pressed` wird eine neue `FingerPaintPolyline` erstellt, `MoveTo` auf dem Pfadobjekt aufgerufen, damit der Anfangspunkt gespeichert wird, und das Objekt zum `inProgressPolylines`-Wörterbuch hinzugefügt. Bei der Verarbeitung von `Moved` wird `LineTo` mit der neuen Position des Fingers für das Pfadobjekt aufgerufen, und bei der Verarbeitung von `Released` wird die fertige Polylinie von `inProgressPolylines` an `completedPolylines` weitergeleitet. Wieder ist der eigentliche Zeichnungscode für SkiaSharp relativ einfach:

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

### <a name="tracking-view-to-view-touch"></a>Verfolgen der View-to-View-Touchbewegung

In allen Beispielen wurde bisher entweder bei der Erstellung des `TouchEffect` oder beim Auftreten des `Pressed`-Ereignisses die `Capture`-Eigenschaft des `TouchEffect` auf `true` festgelegt. Dadurch wird sichergestellt, dass dasselbe Element alle Ereignisse empfängt, die dem Finger zugeordnet werden können, der die Ansicht zuerst berührt hat. Im letzten Beispiel ist dies allerdings *nicht* der Fall, und `Capture` wird nicht auf `true` festgelegt. Dadurch wird ein anderes Verhalten ausgelöst, wenn sich ein Finger, der den Bildschirm berührt, von einem Element zum anderen bewegt. Das Ausgangselement, von dem sich der Finger wegbewegt, empfängt ein Ereignis mit einer `Type`-Eigenschaft, die auf `TouchActionType.Exited` festgelegt ist, und das zweite Element empfängt ein Ereignis mit einer `Type`-Einstellung von `TouchActionType.Entered`.

Diese Art von Touchverarbeitung ist für Musiktastaturen besonders nützlich. Eine Taste sollte sowohl erkennen können, wenn sie gedrückt wird, als auch merken, wenn ein Finger von einer Taste zu einer anderen gleitet.

Die Seite **Silent Keyboard** (Lautlose Tastatur) definiert kleine [`WhiteKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/WhiteKey.cs)- und [`BlackKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BlackKey.cs)-Klassen, die von der [`Key`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/Key.cs) abgeleitet werden, die wiederum von der `BoxView` abgeleitet wird.

Die `Key`-Klasse kann in einem echten Musikprogramm verwendet werden. Sie definiert die öffentlichen Eigenschaften mit den Namen `IsPressed` und `KeyNumber`, die auf den Tastencode festgelegt werden sollen, die von dem MIDI-Standard festgelegt sind. Außerdem definiert die `Key`-Klasse ein Ereignis mit dem Namen `StatusChanged`, das ausgelöst wird, wenn sich die `IsPressed`-Eigenschaft ändert.

Es können mehrere Finger auf den einzelnen Tasten positioniert werden. Aus diesem Grund verwaltet die `Key`-Klasse eine `List` mit ID-Nummern für Touchvorgänge aller Finger, die zum jeweiligen Zeitpunkt die Taste berühren:

```csharp
List<long> ids = new List<long>();
```

Der Ereignishandler `TouchAction` fügt eine ID an die `ids`-Liste für sowohl einen `Pressed`-Ereignishandler als auch einen `Entered`-Typ hinzu. Dies ist jedoch nur der Fall, wenn die `IsInContact`-Eigenschaft für das `Entered`-Ereignis auf `true` festgelegt ist. Die ID wird für ein `Released`- oder `Exited`-Ereignis aus der `List` entfernt:

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

Die Methoden `AddToList` und `RemoveFromList` überprüfen, ob sich die `List` von „leer“ in „nicht leer“ geändert hat, und wenn dies der Fall ist, rufen sie das `StatusChanged`-Ereignis auf.

Die verschiedenen `WhiteKey`- und `BlackKey`-Elemente werden in der [XAML-Datei](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/SilentKeyboardPage.xaml) der Seite angeordnet, die am besten aussieht, wenn das Smartphone im Modus „Querformat“ verwendet wird:

[![](touch-tracking-images/silentkeyboard-small.png "Triple screenshot of the Silent Keyboard page")](touch-tracking-images/silentkeyboard-large.png#lightbox "Triple screenshot of the Silent Keyboard page")

Wenn Sie Ihren Finger über die Tasten gleiten lassen, sehen Sie anhand der minimalen Farbänderungen, dass die Touchereignisse von einer Taste zur nächsten weitergeleitet werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie Ereignisse in einem Effekt auslösen können und wie Sie einen Effekt schreiben und verwenden können, der eine umfassende Multitouchverarbeitung implementiert.

## <a name="related-links"></a>Verwandte Links

- [Multi-Touch Finger Tracking in iOS (Multitouchverfolgung unter iOS)](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Multi-Touch Finger Tracking in Android (Multitouchverfolgung unter Android)](~/android/app-fundamentals/touch/touch-tracking.md)
- [Touch Tracking Effect (sample) (Touchverfolgungseffekt (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
