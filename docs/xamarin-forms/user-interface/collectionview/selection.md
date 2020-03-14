---
title: Xamarin. Forms CollectionView-Auswahl
description: Standardmäßig ist die CollectionView-Auswahl deaktiviert. Die Auswahl von einzeln und mehrfach kann jedoch aktiviert werden.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a4cc237ef738edeccf66f1a91a010e4831c1c72f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305912"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin. Forms CollectionView-Auswahl

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert die folgenden Eigenschaften, die die Elementauswahl Steuern:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)vom Typ [`SelectionMode`](xref:Xamarin.Forms.SelectionMode)den Auswahlmodus.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)vom Typ `object`das ausgewählte Element in der Liste. Diese Eigenschaft verfügt über einen Standard Bindungs Modus `TwoWay`und verfügt über einen `null` Wert, wenn kein Element ausgewählt ist.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)vom Typ `IList<object>`die in der Liste ausgewählten Elemente. Diese Eigenschaft verfügt über einen Standard Bindungs Modus `OneWay`und verfügt über einen `null` Wert, wenn keine Elemente ausgewählt sind.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)vom Typ `ICommand`, der ausgeführt wird, wenn das ausgewählte Element geändert wird.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)vom Typ `object`, bei dem es sich um den Parameter handelt, der an die `SelectionChangedCommand`übergeben wird.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

Standardmäßig ist [`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl deaktiviert. Dieses Verhalten kann jedoch geändert werden, indem der [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschafts Wert auf einen der [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) Enumerationsmember festgelegt wird:

- `None` – gibt an, dass Elemente nicht ausgewählt werden können. Dies ist der Standardwert.
- `Single` – gibt an, dass ein einzelnes Element ausgewählt werden kann, wobei das ausgewählte Element hervorgehoben wird.
- `Multiple` – gibt an, dass mehrere Elemente ausgewählt werden können, wobei die ausgewählten Elemente hervorgehoben werden.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert ein [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis, das ausgelöst wird, wenn sich die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft ändert. Dies liegt daran, dass der Benutzer ein Element aus der Liste auswählt oder wenn eine Anwendung die-Eigenschaft festlegt. Außerdem wird dieses Ereignis auch dann ausgelöst, wenn sich die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaft ändert. Das [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) Objekt, das das `SelectionChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, beide vom Typ `IReadOnlyList<object>`:

- `PreviousSelection` – die Liste der Elemente, die ausgewählt wurden, bevor die Auswahl geändert wurde.
- `CurrentSelection` – die Liste der Elemente, die ausgewählt werden, nachdem die Auswahl geändert wurde.

Außerdem verfügt [`CollectionView`](xref:Xamarin.Forms.CollectionView) über eine `UpdateSelectedItems` Methode, die die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaft mit einer Liste ausgewählter Elemente aktualisiert und nur eine einzige Änderungs Benachrichtigung auslöst.

## <a name="single-selection"></a>Einfache Auswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Single`festgelegt ist, kann ein einzelnes Element im [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausgewählt werden. Wenn ein Element ausgewählt wird, wird die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft auf den Wert des ausgewählten Elements festgelegt. Wenn diese Eigenschaft geändert wird, wird der [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) ausgeführt (mit dem Wert des [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) an den `ICommand`übergebenen), und das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird ausgelöst.

Das folgende XAML-Beispiel zeigt eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) , die auf die Auswahl einzelner Elemente reagieren kann:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

In diesem Codebeispiel wird der `OnCollectionViewSelectionChanged` Ereignishandler ausgeführt, wenn das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) -Ereignis ausgelöst wird, wobei der Ereignishandler das zuvor ausgewählte Element und das aktuell ausgewählte Element abruft:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann durch Änderungen ausgelöst werden, die aufgrund einer Änderung der [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auftreten.

Die folgenden Screenshots zeigen die Auswahl einzelner Elemente in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView):

[![Screenshot einer vertikalen CollectionView-Liste mit einmaliger Auswahl unter IOS und Android](selection-images/single-selection.png "Vertikale CollectionView-Liste mit einzelner Auswahl")](selection-images/single-selection-large.png#lightbox "Vertikale CollectionView-Liste mit einzelner Auswahl")

## <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Multiple`festgelegt ist, können mehrere Elemente im [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausgewählt werden. Wenn Items ausgewählt ist, wird die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaft auf die ausgewählten Elemente festgelegt. Wenn diese Eigenschaft geändert wird, wird der [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) ausgeführt (mit dem Wert des [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) an den `ICommand`übergebenen), und das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird ausgelöst.

Das folgende XAML-Beispiel zeigt eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) , die auf die Auswahl mehrerer Elemente reagieren kann:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

In diesem Codebeispiel wird der `OnCollectionViewSelectionChanged` Ereignishandler ausgeführt, wenn das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) -Ereignis ausgelöst wird, wobei der Ereignishandler die zuvor ausgewählten Elemente und die aktuell ausgewählten Elemente abruft:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> Das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann durch Änderungen ausgelöst werden, die aufgrund einer Änderung der [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auftreten.

Die folgenden Screenshots zeigen die Auswahl mehrerer Elemente in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView):

[![Screenshot einer vertikalen CollectionView-Liste mit Mehrfachauswahl unter IOS und Android](selection-images/multiple-selection.png "Vertikale Auflistung von CollectionView mit Mehrfachauswahl")](selection-images/multiple-selection-large.png#lightbox "Vertikale Auflistung von CollectionView mit Mehrfachauswahl")

## <a name="single-pre-selection"></a>Einzelne vorab Auswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Single`festgelegt ist, kann ein einzelnes Element im [`CollectionView`](xref:Xamarin.Forms.CollectionView) vorab ausgewählt werden, indem die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft auf das Element festgelegt wird. Das folgende XAML-Beispiel zeigt eine `CollectionView`, die ein einzelnes Element vorab auswählt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> Die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay`.

Die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaften Daten werden an die `SelectedMonkey`-Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ `Monkey`ist. Standardmäßig wird eine `TwoWay` Bindung verwendet, sodass der Wert der Eigenschaft `SelectedMonkey` auf das ausgewählte `Monkey` Objekt festgelegt wird, wenn der Benutzer das ausgewählte Element ändert. Die `SelectedMonkey`-Eigenschaft wird in der `MonkeysViewModel`-Klasse definiert und auf das vierte Element der `Monkeys`-Auflistung festgelegt:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

Wenn das [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigt wird, ist das vierte Element in der Liste daher vorab ausgewählt:

[![Screenshot einer vertikalen CollectionView-Liste mit einer einzelnen vorab Auswahl unter IOS und Android](selection-images/single-pre-selection.png "Vertikale CollectionView-Liste mit einzelner Vorauswahl")](selection-images/single-pre-selection-large.png#lightbox "Vertikale CollectionView-Liste mit einzelner Vorauswahl")

## <a name="multiple-pre-selection"></a>Mehrfache vorab Auswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Multiple`festgelegt ist, können mehrere Elemente im [`CollectionView`](xref:Xamarin.Forms.CollectionView) vorab ausgewählt werden. Das folgende XAML-Beispiel zeigt eine `CollectionView`, die die vorab Auswahl mehrerer Elemente ermöglicht:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> Die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaft verfügt über einen Standard Bindungs Modus von `OneWay`.

Die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaften Daten werden an die `SelectedMonkeys`-Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ `ObservableCollection<object>`ist. Die `SelectedMonkeys`-Eigenschaft wird in der `MonkeysViewModel`-Klasse definiert und auf das zweite, vierte und fünfte Element in der `Monkeys` Collection festgelegt:

```csharp
namespace CollectionViewDemos.ViewModels
{
    public class MonkeysViewModel : INotifyPropertyChanged
    {
        ...
        ObservableCollection<object> selectedMonkeys;
        public ObservableCollection<object> SelectedMonkeys
        {
            get
            {
                return selectedMonkeys;
            }
            set
            {
                if (selectedMonkeys != value)
                {
                    selectedMonkeys = value;
                }
            }
        }

        public MonkeysViewModel()
        {
            ...
            SelectedMonkeys = new ObservableCollection<object>()
            {
                Monkeys[1], Monkeys[3], Monkeys[4]
            };
        }
        ...
    }
}
```

Wenn das [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigt wird, sind die zweiten, vierten und fünften Elemente in der Liste daher vorab ausgewählt:

[![Screenshot einer vertikalen CollectionView-Liste mit mehreren vorab Auswahl unter IOS und Android](selection-images/multiple-pre-selection.png "Vertikale Auflistung von CollectionView mit mehreren vorab Auswahl")](selection-images/multiple-pre-selection-large.png#lightbox "Vertikale Auflistung von CollectionView mit mehreren vorab Auswahl")

## <a name="clear-selections"></a>Auswahl löschen

Die Eigenschaften [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) und [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) können gelöscht werden, indem Sie oder die Objekte, an die Sie gebunden werden, an `null`festgelegt werden.

## <a name="change-selected-item-color"></a>Farbe für ausgewähltes Element ändern

[`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über eine `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, um eine visuelle Änderung am ausgewählten Element im `CollectionView`zu initiieren. Ein häufiger Anwendungsfall für diese `VisualState` besteht darin, die Hintergrundfarbe des ausgewählten Elements zu ändern. Dies wird im folgenden XAML-Beispiel gezeigt:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> Der [`Style`](xref:Xamarin.Forms.Style) , der die `Selected` `VisualState` enthält, muss einen [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschafts Wert aufweisen, der der Typ des Stamm Elements des [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)ist, das als `ItemTemplate`-Eigenschafts Wert festgelegt wird.

In diesem Beispiel wird der [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschafts Wert auf `Grid` festgelegt, da das Stamm Element des [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) ein [`Grid`](xref:Xamarin.Forms.Grid)ist. Die `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass die [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) des Elements auf `LightSkyBlue`festgelegt wird, wenn ein Element im [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausgewählt wird:

[![Screenshot einer vertikalen CollectionView-Liste mit einer benutzerdefinierten Auswahl Farbe unter IOS und Android](selection-images/single-selection-color.png "Auflistungs Ansicht (vertikal) mit einer benutzerdefinierten Auswahl Farbe")](selection-images/single-selection-color-large.png#lightbox "Auflistungs Ansicht (vertikal) mit einer benutzerdefinierten Auswahl Farbe")

Weitere Informationen zu visuellen Zuständen finden Sie unter [xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Auswahl deaktivieren

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl ist standardmäßig deaktiviert. Wenn für einen `CollectionView` die Auswahl jedoch aktiviert ist, kann er durch Festlegen der Eigenschaft [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) auf `None`deaktiviert werden:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `None`festgelegt ist, können Elemente im [`CollectionView`](xref:Xamarin.Forms.CollectionView) nicht ausgewählt werden, die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft bleibt `null`, und das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird nicht ausgelöst.

> [!NOTE]
> Wenn ein Element ausgewählt wurde und die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft von `Single` in `None`geändert wird, wird die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft auf `null` festgelegt, und das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird mit einer leeren `CurrentSelection`-Eigenschaft ausgelöst.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
