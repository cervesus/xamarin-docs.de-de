---
title: Xamarin. Forms carouselview-Interaktion
description: Auf das aktuell angezeigte Element in einer "carouselview" kann über die Eigenschaften "Eigenschaften" und "Position" zugegriffen werden.
ms.prod: xamarin
ms.assetid: 854D97E5-D119-4BE2-AE7C-BD428792C992
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: d020cf809369596076d67b68bdd6b3319e629f1e
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697818"
---
# <a name="xamarinforms-carouselview-interaction"></a>Xamarin. Forms carouselview-Interaktion

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert die folgenden Eigenschaften, die die Benutzerinteraktion steuern:

- `CurrentItem` vom Typ `object` das aktuelle Element, das angezeigt wird. Diese Eigenschaft verfügt über einen Standard Bindungs Modus `TwoWay` und verfügt über einen `null` Wert, wenn keine anzuzeigenden Daten vorhanden sind.
- `CurrentItemChangedCommand` vom Typ `ICommand`, der ausgeführt wird, wenn das aktuelle Element geändert wird.
- `CurrentItemChangedCommandParameter` vom Typ `object`: der Parameter, der an `CurrentItemChangedCommand` übergeben wird.
- `IsBounceEnabled` vom Typ `bool`, der angibt, ob der `CarouselView` an einer Inhalts Grenze springt. Der Standardwert ist `true`sein.
- `IsSwipeEnabled` vom Typ `bool`, der bestimmt, ob das angezeigte Element durch eine Schwenkbewegung geändert wird. Der Standardwert ist `true`sein.
- `Position` vom Typ `int` den Index des aktuellen Elements in der zugrunde liegenden Auflistung. Diese Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` und hat den Wert 0, wenn keine anzuzeigenden Daten vorhanden sind.
- `PositionChangedCommand` vom Typ `ICommand`, der ausgeführt wird, wenn sich die Position ändert.
- `PositionChangedCommandParameter` vom Typ `object`: der Parameter, der an `PositionChangedCommand` übergeben wird.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert ein `CurrentItemChanged` Ereignis, das ausgelöst wird, wenn sich die `CurrentItem`-Eigenschaft ändert, entweder aufgrund eines Benutzer Bildlaufs oder wenn eine Anwendung die-Eigenschaft festlegt. Das `CurrentItemChangedEventArgs` Objekt, das das `CurrentItemChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, beide vom Typ `object`:

- `PreviousItem` – das vorherige Element, nachdem die-Eigenschaft geändert wurde.
- `CurrentItem` – das aktuelle Element, nachdem die-Eigenschaft geändert wurde.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert auch ein `PositionChanged` Ereignis, das ausgelöst wird, wenn sich die `Position`-Eigenschaft ändert, entweder aufgrund eines Benutzer Bildlaufs oder wenn eine Anwendung die-Eigenschaft festlegt. Das `PositionChangedEventArgs` Objekt, das das `PositionChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, beide vom Typ `int`:

- `PreviousPosition` – die vorherige Position, nachdem die-Eigenschaft geändert wurde.
- `CurrentPosition` – die aktuelle Position nach der Eigenschafts Änderung.

## <a name="respond-to-the-current-item-changing"></a>Reagieren auf das aktuelle Element ändern

Wenn sich das aktuell angezeigte Element ändert, wird die `CurrentItem`-Eigenschaft auf den Wert des Elements festgelegt. Wenn diese Eigenschaft geändert wird, wird die `CurrentItemChangedCommand` mit dem Wert des `CurrentItemChangedCommandParameter` ausgeführt, der an die `ICommand` übergebenen wird. Die `Position`-Eigenschaft wird dann aktualisiert, und das `CurrentItemChanged`-Ereignis wird ausgelöst.

> [!IMPORTANT]
> Die `Position`-Eigenschaft ändert sich, wenn die `CurrentItem`-Eigenschaft geändert wird. Dies führt dazu, dass die `PositionChangedCommand` ausgeführt wird und das `PositionChanged` Ereignis ausgelöst wird.

### <a name="event"></a>event

Das folgende XAML-Beispiel zeigt eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) , die einen Ereignishandler verwendet, um auf das aktuelle Element zu reagieren, das sich ändert:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChanged="OnCurrentItemChanged">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.CurrentItemChanged += OnCurrentItemChanged;
```

In diesem Beispiel wird der `OnCurrentItemChanged` Ereignishandler ausgeführt, wenn das `CurrentItemChanged`-Ereignis ausgelöst wird, wobei der Ereignishandler die vorherigen und aktuellen Elemente verfügbar macht:

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

### <a name="command"></a>Befehl

Das folgende XAML-Beispiel zeigt eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) , die einen Befehl verwendet, um auf das aktuelle Element zu reagieren, das sich ändert:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChangedCommand="{Binding ItemChangedCommand}"
              CurrentItemChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=CurrentItem}">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandProperty, "ItemChangedCommand");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandParameterProperty, new Binding("CurrentItem", source: RelativeBindingSource.Self));
```

In diesem Beispiel wird die `CurrentItemChangedCommand`-Eigenschaft an die `ItemChangedCommand`-Eigenschaft gebunden, wobei der `CurrentItem`-Eigenschafts Wert als Argument übergeben wird. Der `ItemChangedCommand` kann dann wie erforderlich auf das aktuelle Element reagieren, das geändert wird:

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

In diesem Beispiel aktualisiert der `ItemChangedCommand` Objekte, die die vorherigen und aktuellen Elemente speichern.

## <a name="respond-to-the-position-changing"></a>Reagieren auf die Änderung der Position

Wenn sich das aktuell angezeigte Element ändert, wird die `Position`-Eigenschaft auf den Index des aktuellen Elements in der zugrunde liegenden Auflistung festgelegt. Wenn diese Eigenschaft geändert wird, wird die `PositionChangedCommand` mit dem Wert des `PositionChangedCommandParameter` ausgeführt, der an die `ICommand` übergebenen wird. Das `PositionChanged` Ereignis wird ausgelöst. Wenn die `Position`-Eigenschaft Programm gesteuert geändert wurde, wird der [`CarouselView`](xref:Xamarin.Forms.CarouselView) auf das Element, das dem `Position` Wert entspricht, gescrollt.

> [!NOTE]
> Wenn Sie die `Position`-Eigenschaft auf 0 festlegen, wird das erste Element in der zugrunde liegenden Auflistung angezeigt.

### <a name="event"></a>event

Das folgende XAML-Beispiel zeigt eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) , die einen Ereignishandler verwendet, um auf die `Position` Eigenschaft zu reagieren, die geändert wird:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"              
              PositionChanged="OnPositionChanged">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.PositionChanged += OnPositionChanged;
```

In diesem Beispiel wird der `OnPositionChanged` Ereignishandler ausgeführt, wenn das `PositionChanged`-Ereignis ausgelöst wird, wobei der Ereignishandler die vorherige und die aktuelle Position verfügbar macht:

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

### <a name="command"></a>Befehl

Das folgende XAML-Beispiel zeigt eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) , die einen Befehl verwendet, um auf die `Position` Eigenschaften Änderung zu reagieren:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PositionChangedCommand="{Binding PositionChangedCommand}"
              PositionChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=Position}">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionChangedCommandProperty, "PositionChangedCommand");
carouselView.SetBinding(CarouselView.PositionChangedCommandParameterProperty, new Binding("Position", source: RelativeBindingSource.Self));
```

In diesem Beispiel wird die `PositionChangedCommand`-Eigenschaft an die `PositionChangedCommand`-Eigenschaft gebunden, wobei der `Position`-Eigenschafts Wert als Argument übergeben wird. Der `PositionChangedCommand` kann dann wie erforderlich auf die ändernde Position reagieren:

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

In diesem Beispiel aktualisiert der `PositionChangedCommand` Objekte, die die vorherige und die aktuelle Position speichern.

## <a name="preset-the-current-item"></a>Aktuelles Element voreingestellt

Das aktuelle Element in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann Programm gesteuert festgelegt werden, indem die `CurrentItem`-Eigenschaft auf das Element festgelegt wird. Das folgende XAML-Beispiel zeigt eine `CarouselView`, die das aktuelle Element vorab auswählt:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItem="{Binding CurrentItem}">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemProperty, "CurrentItem");
```

> [!NOTE]
> Die `CurrentItem`-Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay`.

Die `CarouselView.CurrentItem`-Eigenschaften Daten werden an die `CurrentItem`-Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ `Monkey` ist. Standardmäßig wird eine `TwoWay` Bindung verwendet. wenn der Benutzer das aktuelle Element ändert, wird der Wert der `CurrentItem`-Eigenschaft standardmäßig auf das aktuelle `Monkey`-Objekt festgelegt. Die `CurrentItem`-Eigenschaft wird in der `MonkeysViewModel`-Klasse definiert und auf das vierte Element in der `Monkeys`-Auflistung festgelegt:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    public Monkey CurrentItem { get; set; }

    public MonkeysViewModel()
    {
        // ...
        CurrentItem = Monkeys.Skip(3).FirstOrDefault();
        OnPropertyChanged("CurrentItem");
    }
}
```

## <a name="preset-the-position"></a>Position der Voreinstellung

Das angezeigte Element [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann Programm gesteuert festgelegt werden, indem die `Position`-Eigenschaft auf den Index des Elements in der zugrunde liegenden Auflistung festgelegt wird. Das folgende XAML-Beispiel zeigt eine `CarouselView`, mit der das angezeigte Element festgelegt wird:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              Position="{Binding Position}">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionProperty, "Position");
```

> [!NOTE]
> Die `Position`-Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay`.

Die `CarouselView.Position`-Eigenschaften Daten werden an die `Position`-Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ `int` ist. Standardmäßig wird eine `TwoWay` Bindung verwendet, sodass der Wert der `Position`-Eigenschaft auf den Index des angezeigten Elements festgelegt wird, wenn der Benutzer einen Bildlauf durch die [`CarouselView`](xref:Xamarin.Forms.CarouselView)durchführt. Die `Position`-Eigenschaft wird in der `MonkeysViewModel`-Klasse definiert und auf das vierte Element in der `Monkeys`-Auflistung festgelegt:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public int Position { get; set; }

    public MonkeysViewModel()
    {
        // ...
        Position = 3;
        OnPropertyChanged("Position");
    }
}
```

## <a name="clear-the-current-item"></a>Aktuelles Element löschen

Die `CurrentItem`-Eigenschaft kann gelöscht werden, indem Sie Sie oder das Objekt, an das Sie gebunden wird, an `null` festlegen.

## <a name="disable-bounce"></a>Springen deaktivieren

Standardmäßig springt [`CarouselView`](xref:Xamarin.Forms.CarouselView) Elemente an Inhalts Grenzen. Dies kann deaktiviert werden, indem Sie die `IsBounceEnabled`-Eigenschaft auf `false` festlegen.

## <a name="disable-swipe-interaction"></a>Deaktivieren der wischen-Interaktion

Standardmäßig ermöglicht [`CarouselView`](xref:Xamarin.Forms.CarouselView) Benutzern das Navigieren durch Elemente mithilfe einer Schwenkbewegung. Diese wischen-Interaktion kann deaktiviert werden, indem Sie die `IsSwipeEnabled`-Eigenschaft auf `false` festlegen.

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)
