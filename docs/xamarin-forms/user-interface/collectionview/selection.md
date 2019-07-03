---
title: Xamarin.Forms CollectionView-Auswahl
description: CollectionView Auswahl ist standardmäßig deaktiviert. Allerdings kann die Auswahl von einzelnen und mehreren aktiviert werden.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: dc01cf6bea9fe614cbfb53dcc4417ffb0e602c6f
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512757"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin.Forms CollectionView-Auswahl

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert die folgenden Eigenschaften, die Auswahl von Listenelementen zu steuern:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), des Typs [ `SelectionMode` ](xref:Xamarin.Forms.SelectionMode), den Auswahlmodus.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem), des Typs `object`, das ausgewählte Element in der Liste. Diese Eigenschaft verfügt über einen Standardmodus für die Bindung der `TwoWay`, und verfügt über eine `null` Wert, wenn kein Element ausgewählt ist.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), des Typs `IList<object>`, die Elemente in der Liste ausgewählt. Diese Eigenschaft verfügt über einen Standardmodus für die Bindung der `OneWay`, und verfügt über eine `null` Wert, wenn keine Elemente ausgewählt sind.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand), des Typs `ICommand`, die ausgeführt wird, wenn das ausgewählte Element geändert wird.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter), des Typs `object`, dies ist der Parameter, der an die `SelectionChangedCommand`.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

In der Standardeinstellung [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) Auswahl ist deaktiviert. Allerdings kann dieses Verhalten geändert werden, indem die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaftswert eines der [ `SelectionMode` ](xref:Xamarin.Forms.SelectionMode) Enumerationsmember:

- `None` – Gibt an, dass Elemente nicht ausgewählt werden können. Dies ist der Standardwert.
- `Single` – Gibt an, dass ein einzelnes Element ausgewählt werden kann, mit dem ausgewählten Element hervorgehoben wird.
- `Multiple` – Gibt an, dass mehrere Elemente ausgewählt werden können, mit der ausgewählten Elemente werden hervorgehoben.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert eine [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis, wenn ausgelöst, wird, die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) eigenschaftsänderungen, entweder weil der Benutzer ein Element ausgewählt wird, oder in der Liste, wenn eine Anwendung die Eigenschaft festlegt. Darüber hinaus wird auch ausgelöst, wenn die [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) eigenschaftenänderungen. Die [ `SelectionChangedEventArgs` ](xref:Xamarin.Forms.SelectionChangedEventArgs) Objekt an, die mit der `SelectionChanged` Ereignis verfügt über zwei Eigenschaften, die beide vom Typ `IReadOnlyList<object>`:

- `PreviousSelection` – die Liste der Elemente, die ausgewählt wurden, bevor Sie die Auswahl geändert wurde,.
- `CurrentSelection` – die Liste der Elemente, die ausgewählt werden, nachdem die Änderung der Auswahl.

## <a name="single-selection"></a>Einfachauswahl

Wenn die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaftensatz auf `Single`, ein einzelnes Element in der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) ausgewählt werden kann. Wenn ein Element ausgewählt ist, die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft wird auf den Wert des ausgewählten Elements festgelegt werden. Wenn diese Eigenschaft geändert, die [ `SelectionChangedCommand` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) ausgeführt wird (mit dem Wert von der [ `SelectionChangedCommandParameter` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) übergeben wird, um die `ICommand`), und die [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) -Ereignis ausgelöst wird.

Das folgende XAML-Beispiel zeigt eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) , die Auswahl Element Einmaliger reagieren können:

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

In diesem Codebeispiel wird die `OnCollectionViewSelectionChanged` -Ereignishandler ausgeführt wird bei der [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird ausgelöst, mit dem Ereignishandler, der zuvor ausgewählten Elements und das aktuell ausgewählte Element abrufen:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Die [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann ausgelöst werden, durch die Änderungen, die Folge der Änderung auftreten der [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft.

Die folgenden Screenshots zeigen einzelne Elementauswahl in einem [ `CollectionView` ](xref:Xamarin.Forms.CollectionView):

[![Screenshot von einer vertikalen Liste der CollectionView mit Einfachauswahl, unter iOS und Android](selection-images/single-selection.png "CollectionView vertikale Liste mit Einfachauswahl")](selection-images/single-selection-large.png#lightbox "CollectionView vertikale Liste mit einzelnen Auswahl")

## <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaftensatz auf `Multiple`, mehrere Elemente der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) ausgewählt werden kann. Wenn Elemente ausgewählt sind, die [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft wird auf die ausgewählten Elemente festgelegt werden. Wenn diese Eigenschaft geändert, die [ `SelectionChangedCommand` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) ausgeführt wird (mit dem Wert von der [ `SelectionChangedCommandParameter` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) übergeben wird, um die `ICommand`), und die [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) -Ereignis ausgelöst wird.

Das folgende XAML-Beispiel zeigt eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) , die auf der Auswahl mehrerer Elemente reagieren kann:

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

In diesem Codebeispiel wird die `OnCollectionViewSelectionChanged` -Ereignishandler ausgeführt wird bei der [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird ausgelöst, mit dem Ereignishandler, der zuvor ausgewählten Elementen und den derzeit ausgewählten Elementen abrufen:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> Die [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann ausgelöst werden, durch die Änderungen, die Folge der Änderung auftreten der [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft.

Die folgenden Screenshots zeigen die Auswahl des mehrerer Elemente in einem [ `CollectionView` ](xref:Xamarin.Forms.CollectionView):

[![Screenshot von einer vertikalen Liste der CollectionView mit Mehrfachauswahl unter iOS und Android](selection-images/multiple-selection.png "CollectionView vertikale Liste mit Mehrfachauswahl")](selection-images/multiple-selection-large.png#lightbox "CollectionView vertikale Liste mit Mehrfachauswahl")

## <a name="single-pre-selection"></a>Einzelne vor der Auswahl

Wenn die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaftensatz auf `Single`, ein einzelnes Element in der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) durch Festlegen von vorab ausgewählt werden kann die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft, die das Element. Das folgende XAML-Beispiel zeigt eine `CollectionView` , bei dem ein einzelnes Element bereits ausgewählt:

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
> Die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft verfügt über einen Standardmodus für die Bindung der `TwoWay`.

Die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaftendaten bindet an die `SelectedMonkey` Eigenschaft des verbundenen Ansichtsmodell, das vom Typ `Monkey`. Standardmäßig eine `TwoWay` Bindung wird verwendet, daher ändert sich, auch wenn der Benutzer das ausgewählte Element den Wert des der `SelectedMonkey` Eigenschaft wird festgelegt, die dem ausgewählten `Monkey` Objekt. Die `SelectedMonkey` Eigenschaft wird definiert, der `MonkeysViewModel` Klasse, und klicken Sie auf das vierte Element der festgelegt ist die `Monkeys` Auflistung:

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

Aus diesem Grund, wenn die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) angezeigt wird, wird das vierte Element in der Liste vorab ausgewählt ist:

[![Screenshot einer CollectionView vertikale Liste mit einzelnen vor der Auswahl, unter iOS und Android](selection-images/single-pre-selection.png "CollectionView vertikale Liste mit einzelnen vor der Auswahl")](selection-images/single-pre-selection-large.png#lightbox "CollectionView vertikale Liste mit einzelnen vor der Auswahl")

## <a name="multiple-pre-selection"></a>Mehrere vor der Auswahl

Wenn die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaftensatz auf `Multiple`, mehrere Elemente der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) vorab ausgewählt werden kann. Das folgende XAML-Beispiel zeigt eine `CollectionView` wird, die vor der Auswahl mehrerer Elemente ermöglichen:

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
> Die [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft verfügt über einen Standardmodus für die Bindung der `OneWay`.

Die [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaftendaten bindet an die `SelectedMonkeys` Eigenschaft des verbundenen Ansichtsmodell, das vom Typ `ObservableCollection<object>`. Die `SelectedMonkeys` Eigenschaft wird definiert, der `MonkeysViewModel` Klasse, und auf der zweiten, vierte und fünfte festgelegt ist Elemente in der `Monkeys` Auflistung:

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

Aus diesem Grund, wenn die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) angezeigt wird, der zweite viertes und fünfte Elemente in der Liste vorab ausgewählt sind:

[![Screenshot von einer vertikalen Liste der CollectionView mit mehreren vor der Auswahl, unter iOS und Android](selection-images/multiple-pre-selection.png "CollectionView vertikale Liste mit mehreren vor der Auswahl")](selection-images/multiple-pre-selection-large.png#lightbox "CollectionView vertikal Liste mit mehreren vor der Auswahl")

## <a name="clearing-selections"></a>Auswahl löschen

Die [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) und [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaften können gelöscht werden, durch Festlegen von Optionen oder die Objekte, die sie an, zu `null`.

## <a name="change-selected-item-color"></a>Farbe des ausgewählten Elements ändern

[`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über eine `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, initiieren Sie eine visuelle Änderung an das ausgewählte Element in der `CollectionView`. Ein häufiger Anwendungsfall für diese `VisualState` besteht darin, ändern Sie die Hintergrundfarbe des ausgewählten Elements, das in den folgenden XAML-Beispiel gezeigt wird:

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
> Die [ `Style` ](xref:Xamarin.Forms.Style) , enthält die `Selected` `VisualState` benötigen eine [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaftswert, der den Typ des Stammelements des ist der [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), festgelegt wird, als die `ItemTemplate` -Eigenschaftswert.

In diesem Beispiel die [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaftswert wird festgelegt, um `Grid` da das Stammelement der [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) ist eine [ `Grid` ](xref:Xamarin.Forms.Grid). Die `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) gibt an, dass, wenn ein Element in der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) ausgewählt ist, die [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) des Elements wird auf festgelegtwerden`LightSkyBlue`:

[![Screenshot von einer vertikalen Liste der CollectionView mit einer benutzerdefinierten Einfachauswahl Farbe an, unter iOS und Android](selection-images/single-selection-color.png "CollectionView vertikale Liste mit einer benutzerdefinierten Einfachauswahl Farbe") ] (selection-images/single-selection-color-large.png#lightbox " CollectionView vertikale Liste mit einer benutzerdefinierten Einfachauswahl Farbe")

Weitere Informationen zu visuellen Status finden Sie unter [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Auswahl deaktivieren

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl ist standardmäßig deaktiviert. Jedoch wenn eine `CollectionView` verfügt über Auswahl aktiviert ist, können deaktiviert werden, indem die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft `None`:

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

Wenn der [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) -Eigenschaftensatz auf `None`, Elemente in der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) kann nicht ausgewählt werden, der [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft bleiben `null`, und die [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird nicht ausgelöst.

> [!NOTE]
> Wenn ein Element ausgewählt wurde und die [ `SelectionMode` ](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft geändert wird `Single` zu `None`, [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft auf festgelegt `null` und [ `SelectionChanged` ](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird ausgelöst, mit einer leeren `CurrentSelection` Eigenschaft.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms visuellen Zustands-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
