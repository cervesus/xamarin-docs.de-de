---
title: Xamarin. Forms CollectionView-Auswahl
description: Standardmäßig ist die CollectionView-Auswahl deaktiviert. Die Auswahl von einzeln und mehrfach kann jedoch aktiviert werden.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f1a3e8bb8959588e64339f70268370440f356be9
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738974"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin. Forms CollectionView-Auswahl

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften, die die Elementauswahl Steuern:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), vom Typ [`SelectionMode`](xref:Xamarin.Forms.SelectionMode), der Auswahlmodus.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem), vom Typ `object`, das ausgewählte Element in der Liste. Diese Eigenschaft verfügt über einen Standard Bindungs Modus `TwoWay`von und verfügt über `null` einen Wert, wenn kein Element ausgewählt ist.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), vom Typ `IList<object>`, die in der Liste ausgewählten Elemente. Diese Eigenschaft verfügt über einen Standard Bindungs Modus `OneWay`von und verfügt über `null` einen Wert, wenn keine Elemente ausgewählt sind.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)vom Typ `ICommand`, der ausgeführt wird, wenn das ausgewählte Element geändert wird.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)vom Typ `object`, der dem-Parameter entspricht, der an den `SelectionChangedCommand`übergeben wird.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

Die [`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl ist standardmäßig deaktiviert. Dieses Verhalten kann jedoch geändert werden, indem der [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschafts Wert auf einen [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) der-Enumerationsmember festgelegt wird:

- `None`– Gibt an, dass Elemente nicht ausgewählt werden können. Dies ist der Standardwert.
- `Single`– Gibt an, dass ein einzelnes Element ausgewählt werden kann, wobei das ausgewählte Element hervorgehoben wird.
- `Multiple`– Gibt an, dass mehrere Elemente ausgewählt werden können, wobei die ausgewählten Elemente hervorgehoben werden.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert ein [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) -Ereignis, das ausgelöst wird [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) , wenn die-Eigenschaft geändert wird. Dies liegt daran, dass der Benutzer ein Element aus der Liste auswählt oder wenn eine Anwendung die-Eigenschaft festlegt. Außerdem wird dieses Ereignis auch ausgelöst, wenn sich die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaft ändert. Das [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) Objekt, das das `SelectionChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, `IReadOnlyList<object>`beide vom Typ:

- `PreviousSelection`– die Liste der Elemente, die ausgewählt wurden, bevor die Auswahl geändert wurde.
- `CurrentSelection`– die Liste der Elemente, die ausgewählt werden, nachdem die Auswahl geändert wurde.

## <a name="single-selection"></a>Einfache Auswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Single`festgelegt ist, kann ein einzelnes [`CollectionView`](xref:Xamarin.Forms.CollectionView) Element in der ausgewählt werden. Wenn ein Element ausgewählt wird, wird [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) die-Eigenschaft auf den Wert des ausgewählten Elements festgelegt. Wenn diese Eigenschaft geändert wird, [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) wird der ausgeführt (mit dem Wert [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) von, der an das `ICommand`-Objekt übergebenen [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) wird), und das-Ereignis wird ausgelöst.

Das folgende XAML-Beispiel zeigt [`CollectionView`](xref:Xamarin.Forms.CollectionView) ein, das auf die Auswahl einzelner Elemente reagieren kann:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

In diesem Codebeispiel wird der `OnCollectionViewSelectionChanged` Ereignishandler ausgeführt, wenn das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis ausgelöst wird, wobei der Ereignishandler das zuvor ausgewählte Element und das aktuell ausgewählte Element abruft:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann durch Änderungen ausgelöst werden, die durch Ändern der [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auftreten.

Die folgenden Screenshots zeigen die Auswahl einzelner Elemente in [`CollectionView`](xref:Xamarin.Forms.CollectionView)einem:

[ ![Screenshot einer vertikalen CollectionView-Liste mit einzelner Auswahl, unter IOS und Android](selection-images/single-selection.png "CollectionView vertikal Liste mit einzelner Auswahl") ] (selection-images/single-selection-large.png#lightbox "Vertikale CollectionView-Liste mit einzelner Auswahl")

## <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Multiple`festgelegt ist, können mehrere [`CollectionView`](xref:Xamarin.Forms.CollectionView) Elemente in der ausgewählt werden. Wenn Elemente ausgewählt werden, wird [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) die-Eigenschaft auf die ausgewählten Elemente festgelegt. Wenn diese Eigenschaft geändert wird, [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) wird der ausgeführt (mit dem Wert [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) von, der an das `ICommand`-Objekt übergebenen [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) wird), und das-Ereignis wird ausgelöst.

Das folgende XAML-Beispiel zeigt [`CollectionView`](xref:Xamarin.Forms.CollectionView) ein, das auf die Auswahl mehrerer Elemente reagieren kann:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

In diesem Codebeispiel wird der `OnCollectionViewSelectionChanged` Ereignishandler ausgeführt, wenn das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis ausgelöst wird, wobei der Ereignishandler die zuvor ausgewählten Elemente und die aktuell ausgewählten Elemente abruft:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> Das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann durch Änderungen ausgelöst werden, die durch Ändern der [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auftreten.

Die folgenden Screenshots zeigen die Auswahl mehrerer Elemente in [`CollectionView`](xref:Xamarin.Forms.CollectionView)einem:

[ ![Screenshot einer vertikalen CollectionView-Liste mit Mehrfachauswahl unter IOS und Android](selection-images/multiple-selection.png "CollectionView vertikal Liste mit Mehrfachauswahl") ] (selection-images/multiple-selection-large.png#lightbox "Vertikale Auflistung von CollectionView mit Mehrfachauswahl")

## <a name="single-pre-selection"></a>Einzelne vorab Auswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Single`festgelegt ist, [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann ein einzelnes Element im vorab ausgewählt werden, indem die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft auf das-Element festgelegt wird. Das folgende XAML-Beispiel zeigt `CollectionView` ein-Element, das ein einzelnes Element vorab auswählt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> Die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft verfügt über einen Standard Bindungs `TwoWay`Modus von.

Die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaftendaten bindet an die `SelectedMonkey` Eigenschaft des verbundenen Ansichtsmodell, das vom Typ `Monkey`. Standardmäßig wird eine `TwoWay` Bindung verwendet, damit der Wert `SelectedMonkey` der Eigenschaft auf das ausgewählte `Monkey` Objekt festgelegt wird, wenn der Benutzer das ausgewählte Element ändert. Die `SelectedMonkey` -Eigenschaft ist in der `MonkeysViewModel` -Klasse definiert, und wird auf `Monkeys` das vierte Element der Auflistung festgelegt:

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

Wenn [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigt wird, ist das vierte Element in der Liste daher vorab ausgewählt:

[ ![Screenshot einer vertikalen CollectionView-Liste mit einzelner Vorauswahl, in der vertikalen Liste der IOS-und Android-](selection-images/single-pre-selection.png "CollectionView mit einer einzelnen vorab Auswahl") ] (selection-images/single-pre-selection-large.png#lightbox "Vertikale CollectionView-Liste mit einzelner Vorauswahl")

## <a name="multiple-pre-selection"></a>Mehrfache vorab Auswahl

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `Multiple`festgelegt ist, können mehrere [`CollectionView`](xref:Xamarin.Forms.CollectionView) Elemente in der vorab ausgewählt werden. Das folgende XAML-Beispiel zeigt `CollectionView` einen, der die vorab Auswahl mehrerer Elemente ermöglicht:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> Die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) -Eigenschaft verfügt über einen Standard Bindungs `OneWay`Modus von.

Die [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaftendaten bindet an die `SelectedMonkeys` Eigenschaft des verbundenen Ansichtsmodell, das vom Typ `ObservableCollection<object>`. Die `SelectedMonkeys` -Eigenschaft ist in der `MonkeysViewModel` -Klasse definiert, und wird auf das zweite, vierte und fünfte Element in der `Monkeys` Auflistung festgelegt:

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

Wenn [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigt wird, sind die zweiten, vierten und fünften Elemente in der Liste vorab ausgewählt:

[ ![Screenshot einer vertikalen CollectionView-Liste mit mehreren vorauswahlrechten in IOS und Android](selection-images/multiple-pre-selection.png "CollectionView vertikale Liste mit mehreren vorab Auswahl") ] (selection-images/multiple-pre-selection-large.png#lightbox "Vertikale Auflistung von CollectionView mit mehreren vorab Auswahl")

## <a name="clearing-selections"></a>Löschen der Auswahl

Die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) - [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft und die-Eigenschaft können durch Festlegen der-Eigenschaft oder der-Objekte, `null`an die Sie gebunden werden, gelöscht werden.

## <a name="change-selected-item-color"></a>Farbe für ausgewähltes Element ändern

[`CollectionView`](xref:Xamarin.Forms.CollectionView)verfügt über `Selected` eine [`VisualState`](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, um eine visuelle Änderung am ausgewählten Element in `CollectionView`der zu initiieren. Ein häufiger Anwendungsfall hierfür besteht `VisualState` darin, die Hintergrundfarbe des ausgewählten Elements zu ändern. Dies wird im folgenden XAML-Beispiel gezeigt:

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
> Der [`Style`](xref:Xamarin.Forms.Style) , der die `Selected` `VisualState` enthält, muss einen [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschafts [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)Wert aufweisen, der den Typ des Stamm Elements des enthält, der als `ItemTemplate` Eigenschafts Wert festgelegt wird.

In diesem Beispiel wird der [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschafts Wert auf `Grid` festgelegt, da [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) das Stamm Element von [`Grid`](xref:Xamarin.Forms.Grid)ein ist. Der `Selected` [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) `LightSkyBlue`gibt an, dass bei Auswahl eines Elements in der der des Elements auf festgelegt wird: [`VisualState`](xref:Xamarin.Forms.VisualState)

[ ![Screenshot einer vertikalen CollectionView-Liste mit einer benutzerdefinierten Auswahl Farbe, in der vertikalen Liste der IOS-und Android-](selection-images/single-selection-color.png "CollectionView mit einer benutzerdefinierten Auswahl Farbe") ] (selection-images/single-selection-color-large.png#lightbox "Auflistungs Ansicht (vertikal) mit einer benutzerdefinierten Auswahl Farbe")

Weitere Informationen zu visuellen Zuständen finden Sie unter [xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Auswahl deaktivieren

[`CollectionView`](xref:Xamarin.Forms.CollectionView)die Auswahl ist standardmäßig deaktiviert. Wenn für eine `CollectionView` Auswahl aktiviert ist, kann Sie jedoch deaktiviert werden, indem die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft `None`auf festgelegt wird:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

Wenn die [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaft auf `None`festgelegt ist, können [`CollectionView`](xref:Xamarin.Forms.CollectionView) Elemente in der nicht ausgewählt [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) werden, die `null`-Eigenschaft bleibt [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) erhalten, und das-Ereignis wird nicht ausgelöst.

> [!NOTE]
> Wenn ein Element ausgewählt wurde [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) und die-Eigenschaft von `Single` in `None`geändert wird, wird [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) die-Eigenschaft auf `null` festgelegt, [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) und das Ereignis wird mit einer leeren `CurrentSelection` -Eigenschaft ausgelöst. .

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
