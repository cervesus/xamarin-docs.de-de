---
title: Hinzufügen einer Gestenerkennung für Wischbewegungen
description: In diesem Artikel wird erläutert, wie eine Wischbewegung auf einer Ansicht erkannt werden kann.
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: ae9b5eb5b768b50ddcbc199040074de855f220de
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649458"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>Hinzufügen einer Gestenerkennung für Wischbewegungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)

_Eine Wischbewegung tritt auf, wenn ein Finger in horizontaler oder vertikaler Richtung über den Bildschirm bewegt wird. Diese Bewegung wird oft genutzt, um durch Inhalte zu navigieren. Die Codebeispiele in diesem Artikel stammen aus dem Beispiel [SwipeGesture](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)._

Damit eine [`View`](xref:Xamarin.Forms.View)-Klasse eine Wischbewegung erkennt, erstellen Sie eine [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Instanz, legen die [`Direction`](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction)-Eigenschaft auf einen [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection)-Enumerationswert (`Left`, `Right`, `Up` oder `Down`) fest, legen optional die [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold)-Eigenschaft fest, bearbeiten das [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)-Ereignis und fügen die neue Gestenerkennung für Wischbewegungen zur [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers)-Collection auf der Ansicht hinzu. Das folgende Codebeispiel zeigt eine `SwipeGestureRecognizer`-Klasse, die an eine [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse angefügt ist:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Hier der entsprechende C#-Code:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Die [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Klasse enthält auch eine [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold)-Eigenschaft, die optional auf einen `uint`-Wert festgelegt werden kann, der in geräteunabhängigen Einheiten die minimale Strecke angibt, die gewischt werden muss, damit eine Bewegung erkannt wird. Der Standardwert dieser Eigenschaft beträgt 100 und bedeutet, dass alle Wischbewegungen mit einer Länge von unter 100 geräteunabhängigen Einheiten ignoriert werden.

## <a name="recognizing-the-swipe-direction"></a>Erkennen der Wischrichtung

In den obigen Beispielen wird die [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction)-Eigenschaft auf einen einzelnen Wert aus der [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection)-Enumeration festgelegt. Es ist jedoch auch möglich, diese Eigenschaft auf mehrere Werte aus der `SwipeDirection`-Enumeration festzulegen, sodass das [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)-Ereignis als Antwort auf eine Wischbewegung in mehrere Richtungen ausgelöst wird. Dies wird jedoch dadurch eingeschränkt, dass eine einzelne [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Klasse nur Wischbewegungen erkennen kann, die auf derselben Achse vorkommen. Deshalb können Wischbewegungen auf der horizontalen Achse erkannt werden, indem die `Direction`-Eigenschaft auf `Left` und `Right` festgelegt wird:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

Auf ähnliche Weise können Wischbewegungen auf der vertikalen Achse erkannt werden, indem die [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction)-Eigenschaft auf `Up` und `Down` festgelegt wird:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

Alternativ kann eine [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Klasse für jede Wischrichtung erstellt werden, damit Wischbewegungen in alle Richtungen erkannt werden können:

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

Hier der entsprechende C#-Code:

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
> In den obigen Beispielen reagiert derselbe Ereignishandler auf die Auslösung des [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)-Ereignisses. Falls erforderlich kann jedoch jede [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Instanz einen anderen Ereignishandler verwenden.

## <a name="responding-to-the-swipe"></a>Reagieren auf die Wischbewegung

Ein Ereignishandler für das [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)-Ereignis wird im folgenden Beispiel gezeigt:

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

Die [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs)-Klasse kann untersucht werden, um die Richtung der Wischbewegung zu bestimmen, wobei benutzerdefinierte Logik nach Bedarf auf die Wischbewegung reagiert. Die Richtung der Wischbewegung kann aus der [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction)-Eigenschaft der Ereignisargumente abgerufen werden, die auf einen der Werte aus der [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection)-Enumeration festgelegt wird. Darüber hinaus verfügen die Ereignisargumente auch über eine [`Parameter`](xref:Xamarin.Forms.SwipedEventArgs.Parameter)-Eigenschaft, die auf den Wert der [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter)-Eigenschaft festgelegt wird, sofern definiert.

## <a name="using-commands"></a>Verwenden von Befehlen

Die [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Klasse enthält auch [`Command`](xref:Xamarin.Forms.SwipeGestureRecognizer.Command)- und [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter)-Eigenschaften. Diese Eigenschaften werden in der Regel in Anwendungen mit Model View ViewModel (MVVM)-Muster verwendet. Die `Command`-Eigenschaft definiert den `ICommand`, der aufgerufen werden soll, wenn eine Wischbewegung erkannt wurde. Die `CommandParameter`-Eigenschaft definiert dabei ein Objekt, das an den `ICommand.` übergeben werden soll. Im folgenden Codebeispiel wird gezeigt, wie die `Command`-Eigenschaft an einen `ICommand` gebunden wird, der im Ansichtsmodell definiert und dessen Instanz als [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite festgelegt wurde:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Der entsprechende XAML-Code lautet wie folgt:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` ist eine Eigenschaft vom Typ `ICommand`, der in der Ansichtsmodellinstanz definiert und als [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite festgelegt wurde. Wenn eine Wischbewegung erkannt wurde, wird die `Execute`-Methode des `SwipeCommand`-Objekts ausgeführt. Das Argument an die `Execute`-Methode ist der Wert der [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter)-Eigenschaft. Weitere Informationen zu Befehlen finden Sie unter [The Xamarin.Forms Command Interface (Die Xamarin.Forms-Befehlsschnittstelle)](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="creating-a-swipe-container"></a>Erstellen eines Wischcontainers

Die `SwipeContainer`-Klasse, die im folgenden Codebeispiel gezeigt wird, ist eine verallgemeinerte Klasse zur Erkennung von Wischbewegungen, die eine [`View`](xref:Xamarin.Forms.View)-Klasse umschließen kann, um die Erkennung von Wischbewegungen durchzuführen:

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

Die `SwipeContainer`-Klasse erstellt [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Objekte für alle vier Wischrichtungen und fügt `Swipe`-Ereignishandler an. Diese Ereignishandler rufen das `Swipe`-Ereignis auf, das durch den `SwipeContainer` definiert wird.

Das folgende XAML-Codebeispiel zeigt die Klasse `SwipeContainer`, die ein [`BoxView`](xref:Xamarin.Forms.BoxView)-Element umschließt:

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

Das folgende Codebeispiel veranschaulicht, wie die Klasse `SwipeContainer` ein [`BoxView`](xref:Xamarin.Forms.BoxView)-Element auf einer C#-Seite umschließt:

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

Wenn die [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse eine Wischbewegung empfängt, wird das [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)-Ereignis in der [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Klasse ausgelöst. Dies erfolgt durch die `SwipeContainer`-Klasse, die ihr eigenes `Swipe`-Ereignis auslöst. Dieses `Swipe`-Ereignis wird auf der Seite behandelt. Die [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs)-Klasse kann dann untersucht werden, um die Richtung der Wischbewegung zu bestimmen, wobei benutzerdefinierte Logik nach Bedarf auf die Wischbewegung reagiert.

## <a name="related-links"></a>Verwandte Links

- [SwipeGesture (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
