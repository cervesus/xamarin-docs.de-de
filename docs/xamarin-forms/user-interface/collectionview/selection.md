---
title: Xamarin.Forms CollectionView-Auswahl
description: CollectionView Auswahl ist standardmäßig deaktiviert. Allerdings kann die Auswahl von einzelnen und mehreren aktiviert werden.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 1ffed60253889491636fa105dd444ced9c2bedf5
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048209"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin.Forms CollectionView-Auswahl

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` definiert die folgenden Eigenschaften, die Auswahl von Listenelementen zu steuern:

- `SelectionMode`, des Typs `SelectionMode`, den Auswahlmodus.
- `SelectedItem`, des Typs `object`, das ausgewählte Element in der Liste. Diese Eigenschaft verfügt über eine `null` Wert, wenn kein Element ausgewählt ist.
- `SelectedItems`, des Typs `IList<object>`, die Elemente in der Liste ausgewählt. Diese Eigenschaft ist schreibgeschützt und verfügt über eine `null` Wert, wenn keine Elemente ausgewählt sind.
- `SelectionChangedCommand`, des Typs `ICommand`, die ausgeführt wird, wenn das ausgewählte Element geändert wird.
- `SelectionChangedCommandParameter`, des Typs `object`, dies ist der Parameter, der an die `SelectionChangedCommand`.

Alle diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können.

In der Standardeinstellung `CollectionView` Auswahl ist deaktiviert. Allerdings kann dieses Verhalten geändert werden, indem die `SelectionMode` Eigenschaftswert eines der `SelectionMode` Enumerationsmember:

- `None` – Gibt an, dass Elemente nicht ausgewählt werden können. Dies ist der Standardwert.
- `Single` – Gibt an, dass ein einzelnes Element ausgewählt werden kann, mit dem ausgewählten Element hervorgehoben wird.
- `Multiple` – Gibt an, dass mehrere Elemente ausgewählt werden können, mit der ausgewählten Elemente werden hervorgehoben.

`CollectionView` definiert eine `SelectionChanged` Ereignis, wenn ausgelöst, wird, die `SelectedItem` eigenschaftsänderungen, entweder weil der Benutzer ein Element ausgewählt wird, oder in der Liste, wenn eine Anwendung die Eigenschaft festlegt. Darüber hinaus wird auch ausgelöst, wenn die `SelectedItems` eigenschaftenänderungen. Die `SelectionChangedEventArgs` Objekt an, die mit der `SelectionChanged` Ereignis verfügt über zwei Eigenschaften, die beide vom Typ `IReadOnlyList<object>`:

- `PreviousSelection` – die Liste der Elemente, die ausgewählt wurden, bevor Sie die Auswahl geändert wurde,.
- `CurrentSelection` – die Liste der Elemente, die ausgewählt werden, nachdem die Änderung der Auswahl.

## <a name="single-selection"></a>Einfachauswahl

Wenn die `SelectionMode` -Eigenschaftensatz auf `Single`, ein einzelnes Element in der `CollectionView` ausgewählt werden kann. Wenn ein Element ausgewählt ist, die `SelectedItem` Eigenschaft wird auf den Wert des ausgewählten Elements festgelegt werden. Wenn diese Eigenschaft geändert, die `SelectionChangedCommand` ausgeführt wird (mit dem Wert von der `SelectionChangedCommandParameter` übergeben wird, um die `ICommand`), und die `SelectionChanged` Ereignis wird ausgelöst.

Das folgende XAML-Beispiel zeigt eine `CollectionView` , die Auswahl Element Einmaliger reagieren können:

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

In diesem Codebeispiel wird die `OnCollectionViewSelectionChanged` -Ereignishandler ausgeführt wird bei der `SelectionChanged` Ereignis wird ausgelöst, mit dem Ereignishandler, der zuvor ausgewählten Elements und das aktuell ausgewählte Element abrufen:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Die `SelectionChanged` Ereignis kann ausgelöst werden, durch die Änderungen, die Folge der Änderung auftreten der `SelectionMode` Eigenschaft.

Die folgenden Screenshots zeigen einzelne Elementauswahl in einem `CollectionView`:

[![Screenshot von einer vertikalen Liste der CollectionView mit Einfachauswahl, unter iOS und Android](selection-images/single-selection.png "CollectionView vertikale Liste mit Einfachauswahl")](selection-images/single-selection-large.png#lightbox "CollectionView vertikale Liste mit einzelnen Auswahl")

## <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die `SelectionMode` -Eigenschaftensatz auf `Multiple`, mehrere Elemente der `CollectionView` ausgewählt werden kann. Wenn Elemente ausgewählt sind, die `SelectedItems` Eigenschaft wird auf die ausgewählten Elemente festgelegt werden. Wenn diese Eigenschaft geändert, die `SelectionChangedCommand` ausgeführt wird (mit dem Wert von der `SelectionChangedCommandParameter` übergeben wird, um die `ICommand`), und die `SelectionChanged` Ereignis wird ausgelöst.

Das folgende XAML-Beispiel zeigt eine `CollectionView` , die auf der Auswahl mehrerer Elemente reagieren kann:

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

In diesem Codebeispiel wird die `OnCollectionViewSelectionChanged` -Ereignishandler ausgeführt wird bei der `SelectionChanged` Ereignis wird ausgelöst, mit dem Ereignishandler, der zuvor ausgewählten Elementen und den derzeit ausgewählten Elementen abrufen:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> Die `SelectionChanged` Ereignis kann ausgelöst werden, durch die Änderungen, die Folge der Änderung auftreten der `SelectionMode` Eigenschaft.

Die folgenden Screenshots zeigen die Auswahl des mehrerer Elemente in einem `CollectionView`:

[![Screenshot von einer vertikalen Liste der CollectionView mit Mehrfachauswahl unter iOS und Android](selection-images/multiple-selection.png "CollectionView vertikale Liste mit Mehrfachauswahl")](selection-images/multiple-selection-large.png#lightbox "CollectionView vertikale Liste mit Mehrfachauswahl")

## <a name="single-pre-selection"></a>Einzelne vor der Auswahl

Wenn die `SelectionMode` -Eigenschaftensatz auf `Single`, ein einzelnes Element in der `CollectionView` durch Festlegen von vorab ausgewählt werden kann die `SelectedItem` Eigenschaft, um das Element. Das folgende XAML-Beispiel zeigt eine `CollectionView` , bei dem ein einzelnes Element bereits ausgewählt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
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
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

Die `SelectedItem` Eigenschaftendaten bindet an die `SelectedMonkey` Eigenschaft des verbundenen Ansichtsmodell, das vom Typ `Monkey`. Ein `TwoWay` Bindung wird verwendet, damit, auch wenn der Benutzer das ausgewählte Element den Wert der ändert die `SelectedMonkey` Eigenschaft wird festgelegt, die dem ausgewählten `Monkey` Objekt. Die `SelectedMonkey` Eigenschaft wird definiert, der `MonkeysViewModel` Klasse, und klicken Sie auf das vierte Element der festgelegt ist die `Monkeys` Auflistung:

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

Aus diesem Grund, wenn die `CollectionView` angezeigt wird, wird das vierte Element in der Liste vorab ausgewählt ist:

[![Screenshot einer CollectionView vertikale Liste mit einzelnen vor der Auswahl, unter iOS und Android](selection-images/single-pre-selection.png "CollectionView vertikale Liste mit einzelnen vor der Auswahl")](selection-images/single-pre-selection-large.png#lightbox "CollectionView vertikale Liste mit einzelnen vor der Auswahl")

## <a name="multiple-pre-selection"></a>Mehrere vor der Auswahl

Wenn die `SelectionMode` -Eigenschaftensatz auf `Multiple`, mehrere Elemente der `CollectionView` vorab ausgewählt werden kann. Das folgende XAML-Beispiel zeigt eine `CollectionView` wird, die vor der Auswahl mehrerer Elemente ermöglichen:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple">
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
```

Mehrere Elemente der `CollectionView` durch Hinzufügen der Benutzer zur vorab ausgewählt werden kann die `SelectedItems` Eigenschaft:

```csharp
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(1).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(3).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(4).FirstOrDefault());
```

> [!NOTE]
> Die `SelectedItems` Eigenschaft ist schreibgeschützt, und es ist daher nicht möglich, eine bidirektionale Datenbindung Vorauswahl von Elementen zu verwenden.

Aus diesem Grund, wenn die `CollectionView` angezeigt wird, der zweite viertes und fünfte Elemente in der Liste vorab ausgewählt sind:

[![Screenshot von einer vertikalen Liste der CollectionView mit mehreren vor der Auswahl, unter iOS und Android](selection-images/multiple-pre-selection.png "CollectionView vertikale Liste mit mehreren vor der Auswahl")](selection-images/multiple-pre-selection-large.png#lightbox "CollectionView vertikal Liste mit mehreren vor der Auswahl")

## <a name="change-selected-item-color"></a>Farbe des ausgewählten Elements ändern

`CollectionView` verfügt über eine `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, initiieren Sie eine visuelle Änderung an das ausgewählte Element in der `CollectionView`. Ein häufiger Anwendungsfall für diese `VisualState` besteht darin, ändern Sie die Hintergrundfarbe des ausgewählten Elements, das in den folgenden XAML-Beispiel gezeigt wird:

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

In diesem Beispiel die [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaftswert wird festgelegt, um `Grid` da das Stammelement der `ItemTemplate` ist eine [ `Grid` ](xref:Xamarin.Forms.Grid). Die `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) gibt an, dass, wenn ein Element in der `CollectionView` ausgewählt ist, die [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) des Elements wird festgelegt `LightSkyBlue`:

[![Screenshot von einer vertikalen Liste der CollectionView mit einer benutzerdefinierten Einfachauswahl Farbe an, unter iOS und Android](selection-images/single-selection-color.png "CollectionView vertikale Liste mit einer benutzerdefinierten Einfachauswahl Farbe") ] (selection-images/single-selection-color-large.png#lightbox " CollectionView vertikale Liste mit einer benutzerdefinierten Einfachauswahl Farbe")

Weitere Informationen zu visuellen Status finden Sie unter [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Auswahl deaktivieren

`CollectionView` Auswahl ist standardmäßig deaktiviert. Jedoch wenn eine `CollectionView` verfügt über Auswahl aktiviert ist, können deaktiviert werden, indem die `SelectionMode` Eigenschaft `None`:

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

Wenn die `SelectionMode` -Eigenschaftensatz auf `None`, Elemente in der `CollectionView` kann nicht ausgewählt werden, die `SelectedItem` Eigenschaft bleibt `null`, und die `SelectionChanged` Ereignis wird nicht ausgelöst.

> [!NOTE]
> Wenn ein Element ausgewählt wurde und die `SelectionMode` Eigenschaft geändert wird `Single` zu `None`, `SelectedItem` Eigenschaft auf festgelegt `null` und `SelectionChanged` Ereignis wird ausgelöst, mit einer leeren `CurrentSelection` Eigenschaft.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms visuellen Zustands-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
