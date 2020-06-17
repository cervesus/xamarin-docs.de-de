---
Title: " Xamarin.Forms carouselview-Interaktion" Beschreibung: "der Zugriff auf das aktuell angezeigte Element in einer" carouselview "ist über die Eigenschaften" "und" Position "möglich."
ms. Prod: xamarin ms. assetid: 854d97e5-D119-4be2-AE7C-bd428792c992 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 02/11/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-carouselview-interaction"></a>Xamarin.FormsCarouselview-Interaktion

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert die folgenden Eigenschaften, die die Benutzerinteraktion steuern:

- `CurrentItem`, vom Typ `object` , das aktuelle Element, das angezeigt wird. Diese Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` und verfügt über einen `null` Wert, wenn keine anzuzeigenden Daten vorhanden sind.
- `CurrentItemChangedCommand`vom Typ `ICommand` , der ausgeführt wird, wenn sich das aktuelle Element ändert.
- `CurrentItemChangedCommandParameter` vom Typ `object`: der Parameter, der an `CurrentItemChangedCommand` übergeben wird.
- `IsBounceEnabled`vom Typ `bool` , der angibt, ob das `CarouselView` an einer Inhalts Grenze springt. Standardwert: `true`.
- `IsSwipeEnabled`vom Typ `bool` , der bestimmt, ob das angezeigte Element durch eine Schwenkbewegung geändert wird. Standardwert: `true`.
- `Position`, vom Typ `int` , der Index des aktuellen Elements in der zugrunde liegenden Auflistung. Diese Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` und verfügt über einen Wert von 0, wenn keine anzuzeigenden Daten vorhanden sind.
- `PositionChangedCommand`vom Typ `ICommand` , der ausgeführt wird, wenn sich die Position ändert.
- `PositionChangedCommandParameter` vom Typ `object`: der Parameter, der an `PositionChangedCommand` übergeben wird.
- `VisibleViews`vom Typ `ObservableCollection<View>` , bei dem es sich um eine schreibgeschützte Eigenschaft handelt, die die-Objekte für die Elemente enthält, die zurzeit sichtbar sind.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert ein `CurrentItemChanged` -Ereignis, das ausgelöst wird, wenn die- `CurrentItem` Eigenschaft geändert wird, entweder aufgrund eines Benutzer Bildlaufs oder wenn eine Anwendung die-Eigenschaft festlegt. Das `CurrentItemChangedEventArgs` Objekt, das das `CurrentItemChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, beide vom Typ `object` :

- `PreviousItem`– das vorherige Element, nachdem die-Eigenschaft geändert wurde.
- `CurrentItem`– das aktuelle Element, nachdem die-Eigenschaft geändert wurde.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert auch ein `PositionChanged` -Ereignis, das ausgelöst wird, wenn die- `Position` Eigenschaft geändert wird, entweder aufgrund eines Benutzer Bildlaufs oder wenn eine Anwendung die-Eigenschaft festlegt. Das `PositionChangedEventArgs` Objekt, das das `PositionChanged` Ereignis begleitet, verfügt über zwei Eigenschaften, beide vom Typ `int` :

- `PreviousPosition`– die vorherige Position nach der Eigenschafts Änderung.
- `CurrentPosition`– die aktuelle Position nach der Eigenschafts Änderung.

## <a name="respond-to-the-current-item-changing"></a>Reagieren auf das aktuelle Element ändern

Wenn sich das aktuell angezeigte Element ändert, wird die- `CurrentItem` Eigenschaft auf den Wert des Elements festgelegt. Wenn diese Eigenschaft geändert wird, `CurrentItemChangedCommand` wird der mit dem Wert von ausgeführt, der an das-Objekt über `CurrentItemChangedCommandParameter` mittelt wird `ICommand` . `Position`Anschließend wird die-Eigenschaft aktualisiert, und das-Ereignis wird ausgelöst `CurrentItemChanged` .

> [!IMPORTANT]
> Die- `Position` Eigenschaft ändert sich, wenn sich die `CurrentItem` Eigenschaft ändert. Dies führt dazu `PositionChangedCommand` , dass der ausgeführt wird und das `PositionChanged` Ereignis ausgelöst wird.

### <a name="event"></a>event

Das folgende XAML-Beispiel zeigt einen [`CarouselView`](xref:Xamarin.Forms.CarouselView) , der einen Ereignishandler verwendet, um auf das aktuelle Element zu reagieren, das sich ändert:

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

In diesem Beispiel wird der `OnCurrentItemChanged` Ereignishandler ausgeführt, wenn das `CurrentItemChanged` Ereignis ausgelöst wird:

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

In diesem Beispiel macht der `OnCurrentItemChanged` Ereignishandler die vorherigen und aktuellen Elemente verfügbar:

[![Screenshot einer carouselview mit vorherigen und aktuellen Elementen unter IOS und Android](interaction-images/current-item-events.png "Carouselview mit aktuellen und vorherigen Elementen")](interaction-images/current-item-events-large.png#lightbox "Carouselview mit aktuellen und vorherigen Elementen")

### <a name="command"></a>Get-Help

Das folgende XAML-Beispiel zeigt einen [`CarouselView`](xref:Xamarin.Forms.CarouselView) , der einen Befehl verwendet, um auf das aktuelle Element zu reagieren, das sich ändert:

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

In diesem Beispiel wird die- `CurrentItemChangedCommand` Eigenschaft an die- `ItemChangedCommand` Eigenschaft gebunden, wobei der `CurrentItem` Eigenschafts Wert als Argument übergeben wird. Der `ItemChangedCommand` kann dann wie erforderlich auf das aktuelle Element reagieren, das geändert wird:

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

In diesem Beispiel aktualisiert die `ItemChangedCommand` Objekte, die die vorherigen und aktuellen Elemente speichern.

## <a name="respond-to-the-position-changing"></a>Reagieren auf die Änderung der Position

Wenn sich das aktuell angezeigte Element ändert, wird die- `Position` Eigenschaft auf den Index des aktuellen Elements in der zugrunde liegenden Auflistung festgelegt. Wenn diese Eigenschaft geändert wird, `PositionChangedCommand` wird der mit dem Wert von ausgeführt, der an das-Objekt über `PositionChangedCommandParameter` mittelt wird `ICommand` . Das `PositionChanged` Ereignis wird ausgelöst. Wenn die `Position` Eigenschaft Programm gesteuert geändert wurde, wird der [`CarouselView`](xref:Xamarin.Forms.CarouselView) auf das Element gescrollt, das dem Wert entspricht `Position` .

> [!NOTE]
> Wenn Sie die- `Position` Eigenschaft auf 0 festlegen, wird das erste Element in der zugrunde liegenden Auflistung angezeigt.

### <a name="event"></a>event

Das folgende XAML-Beispiel zeigt einen [`CarouselView`](xref:Xamarin.Forms.CarouselView) , der einen Ereignishandler verwendet, um auf die Eigenschaft zu reagieren, die `Position` geändert wird:

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

In diesem Beispiel wird der `OnPositionChanged` Ereignishandler ausgeführt, wenn das `PositionChanged` Ereignis ausgelöst wird:

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

In diesem Beispiel macht der `OnCurrentItemChanged` Ereignishandler die vorherigen und aktuellen Positionen verfügbar:

[![Screenshot einer carouselview mit früheren und aktuellen Positionen unter IOS und Android](interaction-images/current-position-events.png "Carouselview mit aktuellen und vorherigen Positionen")](interaction-images/current-position-events-large.png#lightbox "Carouselview mit aktuellen und vorherigen Positionen")

### <a name="command"></a>Get-Help

Das folgende XAML-Beispiel zeigt einen [`CarouselView`](xref:Xamarin.Forms.CarouselView) , der einen Befehl verwendet, um auf die Eigenschaft zu reagieren, die `Position` geändert wird:

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

In diesem Beispiel wird die- `PositionChangedCommand` Eigenschaft an die- `PositionChangedCommand` Eigenschaft gebunden, wobei der `Position` Eigenschafts Wert als Argument übergeben wird. Der `PositionChangedCommand` kann dann wie erforderlich auf die ändernde Position reagieren:

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

In diesem Beispiel aktualisiert die `PositionChangedCommand` Objekte, die die vorherige und die aktuelle Position speichern.

## <a name="preset-the-current-item"></a>Aktuelles Element voreingestellt

Das aktuelle Element in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann Programm gesteuert festgelegt werden, indem die- `CurrentItem` Eigenschaft auf das-Element festgelegt wird. Das folgende XAML-Beispiel zeigt einen `CarouselView` , der das aktuelle Element vorab auswählt:

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
> Die- `CurrentItem` Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` .

Die `CarouselView.CurrentItem` Eigenschaften Daten werden an die- `CurrentItem` Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ ist `Monkey` . Standardmäßig wird eine `TwoWay` Bindung verwendet, damit der Wert der- `CurrentItem` Eigenschaft auf das aktuelle-Objekt festgelegt wird, wenn der Benutzer das aktuelle Element ändert `Monkey` . Die- `CurrentItem` Eigenschaft ist in der- `MonkeysViewModel` Klasse definiert:

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

In diesem Beispiel wird die- `CurrentItem` Eigenschaft auf das vierte Element in der-Auflistung festgelegt `Monkeys` :

[![Screenshot einer carouselview mit voreingestellten Element unter IOS und Android](interaction-images/preset-item.png "Carouselview mit voreingestellten Element")](interaction-images/preset-item-large.png#lightbox "Carouselview mit voreingestellten Element")

## <a name="preset-the-position"></a>Position der Voreinstellung

Das angezeigte Element [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann Programm gesteuert festgelegt werden, indem die- `Position` Eigenschaft auf den Index des Elements in der zugrunde liegenden Auflistung festgelegt wird. Das folgende XAML-Beispiel zeigt `CarouselView` , wie das angezeigte Element festgelegt wird:

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
> Die- `Position` Eigenschaft verfügt über einen Standard Bindungs Modus von `TwoWay` .

Die `CarouselView.Position` Eigenschaften Daten werden an die- `Position` Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ ist `int` . Standardmäßig wird eine `TwoWay` Bindung verwendet, damit der [`CarouselView`](xref:Xamarin.Forms.CarouselView) Wert der `Position` Eigenschaft auf den Index des angezeigten Elements festgelegt wird, wenn der Benutzer durch den scrollt. Die- `Position` Eigenschaft ist in der- `MonkeysViewModel` Klasse definiert:

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

In diesem Beispiel wird die- `Position` Eigenschaft auf das vierte Element in der-Auflistung festgelegt `Monkeys` :

[![Screenshot einer carouselview mit vordefinierter Position unter IOS und Android](interaction-images/preset-position.png "Carouselview mit vordefinierter Position")](interaction-images/preset-position-large.png#lightbox "Carouselview mit vordefinierter Position")

## <a name="define-visual-states"></a>Definieren von visuellen Zuständen

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert vier visuelle Zustände:

- `CurrentItem`stellt den visuellen Zustand für das aktuell angezeigte Element dar.
- `PreviousItem`stellt den visuellen Zustand für das zuvor angezeigte Element dar.
- `NextItem`stellt den visuellen Zustand für das nächste Element dar.
- `DefaultItem`stellt den visuellen Zustand für die restlichen Elemente dar.

Diese visuellen Zustände können verwendet werden, um visuelle Änderungen an den Elementen zu initiieren, die von angezeigt werden [`CarouselView`](xref:Xamarin.Forms.CarouselView) .

Das folgende XAML-Beispiel zeigt, wie die `CurrentItem` `PreviousItem` visuellen Zustände,, und definiert werden `NextItem` `DefaultItem` :

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="CurrentItem">
                            <VisualState.Setters>
                                <Setter Property="Scale"
                                        Value="1.1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="PreviousItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="NextItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="DefaultItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.25" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <!-- Item template content -->
                <Frame HasShadow="true">
                    ...
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

In diesem Beispiel gibt der `CurrentItem` visuelle Zustand an, dass die-Eigenschaft des aktuellen Elements, das von angezeigt [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird, [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) von seinem Standardwert von 1 in 1,1 geändert wird. Die `PreviousItem` `NextItem` visuellen Zustände und geben an, dass die Elemente, die das aktuelle Element umgeben, mit dem [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) Wert 0,5 angezeigt werden. Der `DefaultItem` visuelle Zustand gibt an, dass die restlichen Elemente, die von angezeigt werden, `CarouselView` mit dem `Opacity` Wert 0,25 angezeigt werden.

> [!NOTE]
> Alternativ können die visuellen Zustände in einer definiert werden [`Style`](xref:Xamarin.Forms.Style) , die über einen [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschafts Wert verfügt, der der Typ des Stamm Elements von [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ist, das als `ItemTemplate` Eigenschafts Wert festgelegt wird.

Die folgenden Screenshots zeigen die `CurrentItem` `PreviousItem` `NextItem` visuellen Zustände, und:

[![Screenshot einer carouselview mit visuellen Zuständen unter IOS und Android](interaction-images/visual-states.png "Visuelle Zustände von carouselview")](interaction-images/visual-states-large.png#lightbox "Visuelle Zustände von carouselview")

Weitere Informationen zu visuellen Zuständen finden Sie unter [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="clear-the-current-item"></a>Aktuelles Element löschen

Die- `CurrentItem` Eigenschaft kann gelöscht werden, indem Sie oder das Objekt, an das Sie gebunden wird, auf festgelegt wird `null` .

## <a name="disable-bounce"></a>Springen deaktivieren

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) springt Elemente an Inhalts Grenzen. Dies kann deaktiviert werden, indem die-Eigenschaft auf festgelegt wird `IsBounceEnabled` `false` .

## <a name="disable-swipe-interaction"></a>Deaktivieren der wischen-Interaktion

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) ermöglicht es Benutzern, mithilfe einer Schwenkbewegung durch Elemente zu navigieren. Diese Schwenk Interaktion kann deaktiviert werden, indem die- `IsSwipeEnabled` Eigenschaft auf festgelegt wird `false` .

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
