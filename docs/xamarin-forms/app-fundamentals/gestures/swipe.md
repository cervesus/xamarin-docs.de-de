---
title: Hinzufügen einer streifbewegung stiftbewegungs-Erkennung
description: In diesem Artikel wird erläutert, wie Sie eine streifbewegung auf eine Ansicht zu erkennen.
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: 95e95d8849824cd2dc31c2019627cc5adbbefeec
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131524"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>Hinzufügen einer streifbewegung stiftbewegungs-Erkennung

_Eine streifbewegung tritt auf, wenn ein Finger auf dem Bildschirm in eine Richtung horizontal oder vertikal verschoben wird, und häufig verwendet, um die Navigation durch Inhalte zu initiieren. Die Codebeispiele in diesem Artikel stammen aus der [Streifbewegung](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/) Beispiel._

Vornehmen einer [ `View` ](xref:Xamarin.Forms.View) erkennen Sie eine streifbewegung, erstellen Sie eine [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) Instanz, legen die [ `Direction` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) Eigenschaft, um eine [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) Enumerationswert (`Left`, `Right`, `Up`, oder `Down`) und optional die [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) -Eigenschaft, das Handle der [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) -Ereignis, und fügen Sie der neuen stiftbewegungs-Erkennung, um die [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) Auflistung für die Sicht. Das folgende Codebeispiel zeigt eine `SwipeGestureRecognizer` angefügt, um eine [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Hier ist das Äquivalent C# Code:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Die [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) -Klasse enthält auch eine [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) Eigenschaft, die optional, um festgelegt werden eine `uint` Wert, der die minimale Wischen Entfernung darstellt, der für erzielt werden muss ein streichen Sie nach, um erkannt zu werden, in geräteunabhängigen Einheiten. Der Standardwert dieser Eigenschaft ist 100, was bedeutet, dass alle Kundenkarte, die weniger als 100 geräteunabhängige Einheiten werden ignoriert.

## <a name="recognizing-the-swipe-direction"></a>Erkennt die Richtung Wischen

In den obigen Beispielen die [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) -Eigenschaftensatz auf einzelne einen Wert aus der [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) Enumeration. Es ist jedoch auch möglich, diese Eigenschaft festzulegen, um mehrere Werte aus der `SwipeDirection` Enumeration, sodass die [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) Ereignis wird ausgelöst, als Reaktion auf ein Wischen nach in beide Richtungen. Die Einschränkung ist jedoch ein einzelnes [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) Kundenkarte, die auftreten, können nur auf der gleichen Achse erkennen. Aus diesem Grund für die Kundenkarte, die auf der horizontalen Achse auftreten, erkannt werden können, durch Festlegen der `Direction` Eigenschaft `Left` und `Right`:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

Auf ähnliche Weise für die Kundenkarte, die auf der vertikalen Achse auftreten, erkannt werden können, durch Festlegen der [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) Eigenschaft `Up` und `Down`:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

Sie können auch eine [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) für jede Wischen Richtung erstellt werden kann, um die Kundenkarte in jede Richtung zu erkennen:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Right" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Up" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Down" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Hier ist das Äquivalent C# Code:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;
var rightSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Right };
rightSwipeGesture.Swiped += OnSwiped;
var upSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up };
upSwipeGesture.Swiped += OnSwiped;
var downSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Down };
downSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
boxView.GestureRecognizers.Add(rightSwipeGesture);
boxView.GestureRecognizers.Add(upSwipeGesture);
boxView.GestureRecognizers.Add(downSwipeGesture);
```

> [!NOTE]
> In den obigen Beispielen, der denselben Ereignishandler reagiert auf die [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) Auslösen von Ereignissen. Jedoch jede [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) Instanz kann bei Bedarf einen anderer Ereignishandler verwenden.

## <a name="responding-to-the-swipe"></a>Reagieren auf das Wischen

Ein Ereignishandler für die [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) Ereignis wird im folgenden Beispiel gezeigt:

```csharp
void OnSwiped(object sender, SwipedEventArgs e)
{
    switch (e.Direction)
    {
        case SwipeDirection.Left:
            // Handle the swipe
            break;
        case SwipeDirection.Right:
            // Handle the swipe
            break;
        case SwipeDirection.Up:
            // Handle the swipe
            break;
        case SwipeDirection.Down:
            // Handle the swipe
            break;
    }
}
```

Die [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs) untersucht werden kann, um zu bestimmen, die Richtung des Streifen, benutzerdefinierte Logik, die auf das Wischen nach Bedarf reagieren. Die Richtung des Streifen abgerufen werden kann, aus der [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) Eigenschaft der Ereignisargumente, die auf einen der Werte festgelegt werden, wird die [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) Enumeration. Darüber hinaus die Ereignisargumente auch haben eine [ `Parameter` ](xref:Xamarin.Forms.SwipedEventArgs.Parameter) -Eigenschaft, die auf den Wert der festgelegt werden, die [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) -Eigenschaft, wenn definiert.

## <a name="using-commands"></a>Mithilfe von Befehlen

Die [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) -Klasse enthält auch [ `Command` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) und [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) Eigenschaften. Diese Eigenschaften werden in der Regel in Anwendungen verwendet, die die Model-View-ViewModel (MVVM)-Muster verwenden. Die `Command` -Eigenschaft definiert die `ICommand` aufgerufen werden, wenn eine streifbewegung erkannt wird, mit der `CommandParameter` Eigenschaft definieren ein Objekt übergeben werden soll die `ICommand.` im folgenden Codebeispiel wird veranschaulicht, wie zum Binden der `Command` Eigenschaft, um eine `ICommand` im Ansichtsmodell, deren Instanz, wie die Seite festgelegt wird, definiert [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext):

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Der entsprechende XAML-Code ist:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` ist eine Eigenschaft vom Typ `ICommand` in der Ansicht-Modell-Instanz, die als die Seite "festgelegt ist definiert [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Wenn eine streifbewegung erkannt wird, die `Execute` Methode der `SwipeCommand` Objekt ausgeführt wird. Das Argument für die `Execute` Methode ist der Wert der [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) Eigenschaft. Weitere Informationen zu Befehlen finden Sie unter [die Befehlsschnittstelle](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="creating-a-swipe-container"></a>Erstellen eines Containers Wischen

Die `SwipeContainer` -Klasse, die im folgenden Codebeispiel gezeigt wird, ist eine verallgemeinerte Wischen-Erkennung-Klasse, die umschließt eine [ `View` ](xref:Xamarin.Forms.View) Wischen gestenerkennung ausführen:

```csharp
public class SwipeContainer : ContentView
{
    public event EventHandler<SwipedEventArgs> Swipe;

    public SwipeContainer()
    {
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Left));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Right));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Up));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Down));
    }

    SwipeGestureRecognizer GetSwipeGestureRecognizer(SwipeDirection direction)
    {
        var swipe = new SwipeGestureRecognizer { Direction = direction };
        swipe.Swiped += (sender, e) => Swipe?.Invoke(this, e);
        return swipe;
    }
}
```

Die `SwipeContainer` -Klasse erstellt [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) von Objekten für alle vier Wischen Richtungen, und fügt `Swipe` -Ereignishandler. Rufen Sie diese Ereignishandler die `Swipe` Ereignisses definiert, durch die `SwipeContainer`.

Das folgende Beispiel zeigt für die XAML-Code der `SwipeContainer` Klasse umschließen einer [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

Das folgende Codebeispiel zeigt die `SwipeContainer` dient als Wrapper für eine [ `BoxView` ](xref:Xamarin.Forms.BoxView) in eine C# Seite:

```csharp
public class SwipeContainerPageCS : ContentPage
{
    public SwipeContainerPageCS()
    {
        var boxView = new BoxView { Color = Color.Teal, ... };
        var swipeContainer = new SwipeContainer { Content = boxView, ... };
        swipeContainer.Swipe += (sender, e) =>
        {
          // Handle the swipe
        };

        Content = new StackLayout
        {
            Children = { swipeContainer }
        };
    }
}
```

Wenn der [ `BoxView` ](xref:Xamarin.Forms.BoxView) empfängt eine streifbewegung der [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) Ereignis in der [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) ausgelöst wird. Dies erfolgt durch die `SwipeContainer` -Klasse, die eine eigene löst `Swipe` Ereignis. Dies `Swipe` Ereignis erfolgt auf der Seite. Die [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs) können dann untersucht werden, um zu bestimmen, die Richtung des Streifen, benutzerdefinierte Logik, die auf das Wischen nach Bedarf reagieren.

## <a name="related-links"></a>Verwandte Links

- [Die Streifbewegung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
