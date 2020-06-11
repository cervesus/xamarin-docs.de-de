---
Title: " Xamarin.Forms CollectionView Selection" Description: "die CollectionView-Auswahl ist standardmäßig deaktiviert. Die Auswahl von "einfach" und "Mehrfachauswahl" kann jedoch aktiviert werden.
ms. Prod: xamarin ms. assetid: 423d91c7-1E58-4735-9e80-58b11cdfd953 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 05/06/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview-selection"></a>Xamarin.FormsCollectionView-Auswahl

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften, die die Elementauswahl Steuern:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), vom Typ [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) , der Auswahlmodus.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem), vom Typ `object` , das ausgewählte Element in der Liste. Diese Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` und verfügt über einen `null` Wert, wenn kein Element ausgewählt ist.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), vom Typ `IList<object>` , die in der Liste ausgewählten Elemente. Diese Eigenschaft verfügt über einen Standard Bindungs Modus von `OneWay` und verfügt über einen `null` Wert, wenn keine Elemente ausgewählt sind.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)vom Typ `ICommand` , der ausgeführt wird, wenn das ausgewählte Element geändert wird.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)vom Typ `object` , der dem-Parameter entspricht, der an den übergeben wird `SelectionChangedCommand` .

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

Die [`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl ist standardmäßig deaktiviert. Dieses Verhalten kann jedoch geändert werden, indem der- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschafts Wert auf einen der-Enumerationsmember festgelegt wird [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) :

- `None`– Gibt an, dass Elemente nicht ausgewählt werden können. Dies ist der Standardwert.
- `Single`– Gibt an, dass ein einzelnes Element ausgewählt werden kann, wobei das ausgewählte Element hervorgehoben wird.
- `Multiple`– Gibt an, dass mehrere Elemente ausgewählt werden können, wobei die ausgewählten Elemente hervorgehoben werden.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert ein- [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis, das ausgelöst wird, wenn die- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft geändert wird. Dies liegt daran, dass der Benutzer ein Element aus der Liste auswählt oder wenn eine Anwendung die-Eigenschaft festlegt. Außerdem wird dieses Ereignis auch ausgelöst, wenn sich die- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft ändert. Das [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) Objekt, das das `SelectionChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, beide vom Typ `IReadOnlyList<object>` :

- `PreviousSelection`– die Liste der Elemente, die ausgewählt wurden, bevor die Auswahl geändert wurde.
- `CurrentSelection`– die Liste der Elemente, die ausgewählt werden, nachdem die Auswahl geändert wurde.

Außerdem [`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über eine- `UpdateSelectedItems` Methode, die die- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft mit einer Liste ausgewählter Elemente aktualisiert und nur eine einzige Änderungs Benachrichtigung auslöst.

## <a name="single-selection"></a>Einzelauswahl

Wenn die- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft auf festgelegt ist `Single` , kann ein einzelnes Element in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausgewählt werden. Wenn ein Element ausgewählt wird, wird die- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft auf den Wert des ausgewählten Elements festgelegt. Wenn diese Eigenschaft geändert wird, [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) wird der ausgeführt (mit dem Wert von, der [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) an das-Objekt übergebenen wird `ICommand` ), und das-Ereignis wird ausgelöst [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) .

Das folgende XAML-Beispiel zeigt ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) , das auf die Auswahl einzelner Elemente reagieren kann:

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
> Das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann durch Änderungen ausgelöst werden, die durch Ändern der-Eigenschaft auftreten [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) .

Die folgenden Screenshots zeigen die Auswahl einzelner Elemente in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) :

[![Screenshot einer vertikalen CollectionView-Liste mit einmaliger Auswahl unter IOS und Android](selection-images/single-selection.png "Vertikale CollectionView-Liste mit einzelner Auswahl")](selection-images/single-selection-large.png#lightbox "Vertikale CollectionView-Liste mit einzelner Auswahl")

## <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft auf festgelegt ist `Multiple` , können mehrere Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausgewählt werden. Wenn Elemente ausgewählt werden, wird die- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft auf die ausgewählten Elemente festgelegt. Wenn diese Eigenschaft geändert wird, [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) wird der ausgeführt (mit dem Wert von, der [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) an das-Objekt übergebenen wird `ICommand` ), und das-Ereignis wird ausgelöst [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) .

Das folgende XAML-Beispiel zeigt ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) , das auf die Auswahl mehrerer Elemente reagieren kann:

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
> Das [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis kann durch Änderungen ausgelöst werden, die durch Ändern der-Eigenschaft auftreten [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) .

Die folgenden Screenshots zeigen die Auswahl mehrerer Elemente in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) :

[![Screenshot einer vertikalen CollectionView-Liste mit Mehrfachauswahl unter IOS und Android](selection-images/multiple-selection.png "Vertikale Auflistung von CollectionView mit Mehrfachauswahl")](selection-images/multiple-selection-large.png#lightbox "Vertikale Auflistung von CollectionView mit Mehrfachauswahl")

## <a name="single-pre-selection"></a>Einzelne vorab Auswahl

Wenn die- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft auf festgelegt ist `Single` , kann ein einzelnes Element im [`CollectionView`](xref:Xamarin.Forms.CollectionView) vorab ausgewählt werden, indem die- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft auf das-Element festgelegt wird. Das folgende XAML-Beispiel zeigt ein `CollectionView` -Element, das ein einzelnes Element vorab auswählt:

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
> Die- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` .

Die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) Eigenschaften Daten werden an die- `SelectedMonkey` Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ ist `Monkey` . Standardmäßig wird eine `TwoWay` Bindung verwendet, damit der Wert der `SelectedMonkey` Eigenschaft auf das ausgewählte Objekt festgelegt wird, wenn der Benutzer das ausgewählte Element ändert `Monkey` . Die `SelectedMonkey` -Eigenschaft ist in der `MonkeysViewModel` -Klasse definiert, und wird auf das vierte Element der Auflistung festgelegt `Monkeys` :

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

[![Screenshot einer vertikalen CollectionView-Liste mit einer einzelnen vorab Auswahl unter IOS und Android](selection-images/single-pre-selection.png "Vertikale CollectionView-Liste mit einzelner Vorauswahl")](selection-images/single-pre-selection-large.png#lightbox "Vertikale CollectionView-Liste mit einzelner Vorauswahl")

## <a name="multiple-pre-selection"></a>Mehrfache vorab Auswahl

Wenn die- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft auf festgelegt ist `Multiple` , können mehrere Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) vorab ausgewählt werden. Das folgende XAML-Beispiel zeigt einen `CollectionView` , der die vorab Auswahl mehrerer Elemente ermöglicht:

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
> Die- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaft verfügt über einen Standard Bindungs Modus von `OneWay` .

Die [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) Eigenschaften Daten werden an die- `SelectedMonkeys` Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ ist `ObservableCollection<object>` . Die `SelectedMonkeys` -Eigenschaft ist in der `MonkeysViewModel` -Klasse definiert, und wird auf das zweite, vierte und fünfte Element in der Auflistung festgelegt `Monkeys` :

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

[![Screenshot einer vertikalen CollectionView-Liste mit mehreren vorab Auswahl unter IOS und Android](selection-images/multiple-pre-selection.png "Vertikale Auflistung von CollectionView mit mehreren vorab Auswahl")](selection-images/multiple-pre-selection-large.png#lightbox "Vertikale Auflistung von CollectionView mit mehreren vorab Auswahl")

## <a name="clear-selections"></a>Auswahl löschen

Die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft und die-Eigenschaft [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) können durch Festlegen der-Eigenschaft oder der-Objekte, an die Sie gebunden werden, gelöscht werden `null` .

## <a name="change-selected-item-color"></a>Farbe für ausgewähltes Element ändern

[`CollectionView`](xref:Xamarin.Forms.CollectionView)verfügt über eine `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, um eine visuelle Änderung am ausgewählten Element in der zu initiieren `CollectionView` . Ein häufiger Anwendungsfall hierfür besteht darin, `VisualState` die Hintergrundfarbe des ausgewählten Elements zu ändern. Dies wird im folgenden XAML-Beispiel gezeigt:

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
> Der [`Style`](xref:Xamarin.Forms.Style) , der die enthält, `Selected` `VisualState` muss einen [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschafts Wert aufweisen, der den Typ des Stamm Elements des enthält [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der als `ItemTemplate` Eigenschafts Wert festgelegt wird.

In diesem Beispiel wird der- [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschafts Wert auf festgelegt, `Grid` da das Stamm Element von [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) ein ist [`Grid`](xref:Xamarin.Forms.Grid) . Der gibt an, `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) dass bei Auswahl eines Elements in der der [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) des Elements auf festgelegt wird `LightSkyBlue` :

[![Screenshot einer vertikalen CollectionView-Liste mit einer benutzerdefinierten Auswahl Farbe unter IOS und Android](selection-images/single-selection-color.png "Auflistungs Ansicht (vertikal) mit einer benutzerdefinierten Auswahl Farbe")](selection-images/single-selection-color-large.png#lightbox "Auflistungs Ansicht (vertikal) mit einer benutzerdefinierten Auswahl Farbe")

Weitere Informationen zu visuellen Zuständen finden Sie unter [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Auswahl deaktivieren

[`CollectionView`](xref:Xamarin.Forms.CollectionView)die Auswahl ist standardmäßig deaktiviert. Wenn `CollectionView` für eine Auswahl aktiviert ist, kann Sie jedoch deaktiviert werden, indem die-Eigenschaft auf festgelegt wird [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `None` :

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

Wenn die- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft auf festgelegt ist `None` , können Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) nicht ausgewählt werden, die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft bleibt erhalten `null` , und das- [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird nicht ausgelöst.

> [!NOTE]
> Wenn ein Element ausgewählt wurde und die- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Eigenschaft von in geändert `Single` wird `None` , wird die [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) -Eigenschaft auf festgelegt, `null` und das- [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) Ereignis wird mit einer leeren- `CurrentSelection` Eigenschaft ausgelöst.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
