---
title: Xamarin. Forms CollectionView-Daten
description: Eine CollectionView wird mit Daten aufgefüllt, indem die ItemsSource-Eigenschaft auf eine beliebige Auflistung festgelegt wird, die IEnumerable implementiert.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 9442f7878d9290946fabb7bfc5dee77a828228c7
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305930"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin. Forms CollectionView-Daten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) enthält die folgenden Eigenschaften, die die anzuzeigenden Daten und ihre Darstellung definieren:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)vom Typ `IEnumerable`gibt die Sammlung von Elementen an, die angezeigt werden sollen, und hat den Standardwert `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)gibt die Vorlage an, die auf jedes Element in der Auflistung der anzuzeigenden Elemente angewendet werden soll.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert eine `ItemsUpdatingScrollMode` Eigenschaft, die das Scrollverhalten der `CollectionView` darstellt, wenn ihr neue Elemente hinzugefügt werden. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Steuern der Scrollposition, wenn neue Elemente hinzugefügt werden](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können Daten auch inkrementell laden, wenn der Benutzer einen Bildlauf ausführt. Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](#load-data-incrementally).

## <a name="populate-a-collectionview-with-data"></a>Auffüllen einer CollectionView mit Daten

Ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung festgelegt wird, die `IEnumerable`implementiert. Elemente können in XAML hinzugefügt werden, indem die `ItemsSource`-Eigenschaft aus einem Zeichen folgen Array initialisiert wird:

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> Bedenken Sie, dass Sie für das Element `x:Array` ein `Type`-Attribute benötigen, das die Elementtypen im Array angibt.

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!WARNING]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) löst eine Ausnahme aus, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) vom UI-Thread aktualisiert wird.

Standardmäßig zeigt [`CollectionView`](xref:Xamarin.Forms.CollectionView) Elemente in einer vertikalen Liste an, wie in den folgenden Screenshots gezeigt:

[![Screenshot von CollectionView mit Textelementen unter IOS und Android](populate-data-images/text.png "Text Elemente in einer CollectionView")](populate-data-images/text-large.png#lightbox "Text Elemente in einer CollectionView")

> [!IMPORTANT]
> Wenn die [`CollectionView`](xref:Xamarin.Forms.CollectionView) beim Hinzufügen, entfernen oder Ändern von Elementen in der zugrunde liegenden Auflistung aktualisiert werden muss, sollte die zugrunde liegende Auflistung eine `IEnumerable` Auflistung sein, die Benachrichtigungen über Eigenschafts Änderungen sendet, z. b. `ObservableCollection`.

Informationen zum Ändern des [`CollectionView`](xref:Xamarin.Forms.CollectionView) Layouts finden Sie unter [xamarin. Forms CollectionView Layout](layout.md). Informationen dazu, wie Sie die Darstellung der einzelnen Elemente im `CollectionView`definieren, finden Sie unter [Definieren der Element](#define-item-appearance)Darstellung.

### <a name="data-binding"></a>Datenbindung

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können mit Daten aufgefüllt werden, indem die Datenbindung verwendet wird, um die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft an eine `IEnumerable` Auflistung zu binden. In XAML wird dies mit der `Binding` Markup Erweiterung erreicht:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

In diesem Beispiel werden die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschafts Daten an die `Monkeys`-Eigenschaft des verbundenen ViewModel gebunden.

> [!NOTE]
> Kompilierte Bindungen können aktiviert werden, um die Daten bindungsleistung in xamarin. Forms-Anwendungen zu verbessern. Weitere Informationen finden Sie unter [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Element Darstellung definieren

Die Darstellung der einzelnen Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann definiert werden, indem die [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

Die im [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angegebenen Elemente definieren die Darstellung der einzelnen Elemente in der Liste. Im Beispiel wird das Layout innerhalb des `DataTemplate` von einem [`Grid`](xref:Xamarin.Forms.Grid)verwaltet. Die `Grid` enthält ein [`Image`](xref:Xamarin.Forms.Image) -Objekt und zwei [`Label`](xref:Xamarin.Forms.Label) -Objekte, die alle an Eigenschaften der `Monkey`-Klasse gebunden sind:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die folgenden Screenshots zeigen das Ergebnis der Vorlagen Darstellung der einzelnen Elemente in der Liste:

[![Screenshot von CollectionView, in dem jedes Element auf IOS und Android Vorlagen basiert ist](populate-data-images/datatemplate.png "Auf Vorlagen basierende Elemente in einer CollectionView")](populate-data-images/datatemplate-large.png#lightbox "Auf Vorlagen basierende Elemente in einer CollectionView")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die Eigenschaft [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) auf ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) Objekt festgelegt wird:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Die [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft wird auf ein `MonkeyDataTemplateSelector`-Objekt festgelegt. Das folgende Beispiel zeigt die `MonkeyDataTemplateSelector`-Klasse:

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

Die `MonkeyDataTemplateSelector`-Klasse definiert `AmericanMonkey` und `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` Überschreibung gibt die `AmericanMonkey` Vorlage zurück, die den affennamen und den Speicherort in Teal anzeigt, wenn der affenname "America" enthält. Wenn der affenname nicht "America" enthält, gibt die `OnSelectTemplate` Überschreibung die `OtherMonkey` Vorlage zurück, die den affennamen und den Speicherort in Silber anzeigt:

[![Screenshot der Auswahl von "CollectionView-Lauf Zeitelement Vorlage" unter IOS und Android](populate-data-images/datatemplateselector.png "Auswahl der Lauf Zeitelement Vorlage in einer CollectionView")](populate-data-images/datatemplateselector-large.png#lightbox "Auswahl der Lauf Zeitelement Vorlage in einer CollectionView")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> Wenn Sie [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwenden, legen Sie das Stamm Element ihrer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) -Objekte nie auf einen `ViewCell`fest. Dies führt dazu, dass eine Ausnahme ausgelöst wird, da `CollectionView` über kein Konzept von Zellen verfügt.

## <a name="context-menus"></a>Kontextmenüs

[`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt Kontextmenüs für Datenelemente über die `SwipeView`, wodurch das Kontextmenü mit einer Schwenkbewegung angezeigt wird. Der `SwipeView` ist ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente für das Inhalts Element bereitstellt. Aus diesem Grund werden Kontextmenüs für eine `CollectionView` implementiert, indem ein `SwipeView` erstellt wird, das den Inhalt definiert, den der `SwipeView` umschließt, sowie die Kontextmenü Elemente, die durch die Schwenkbewegung angezeigt werden. Dies wird erreicht, indem die `SwipeView` als Stamm Ansicht in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt wird, die die Darstellung der einzelnen Datenelemente im `CollectionView`definiert:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <SwipeView>
                <SwipeView.LeftItems>
                    <SwipeItems>
                        <SwipeItem Text="Favorite"
                                   IconImageSource="favorite.png"
                                   BackgroundColor="LightGreen"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.FavoriteCommand}"
                                   CommandParameter="{Binding}" />
                        <SwipeItem Text="Delete"
                                   IconImageSource="delete.png"
                                   BackgroundColor="LightPink"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.DeleteCommand}"
                                   CommandParameter="{Binding}" />
                    </SwipeItems>
                </SwipeView.LeftItems>
                <Grid BackgroundColor="White"
                      Padding="10">
                    <!-- Define item appearance -->
                </Grid>
            </SwipeView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    // Define item appearance
    Grid grid = new Grid { Padding = 10, BackgroundColor = Color.White };
    // ...

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: collectionView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: collectionView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.LeftItems = new SwipeItems { favoriteSwipeItem, deleteSwipeItem };
    swipeView.Content = grid;    
    return swipeView;
});
```

In diesem Beispiel ist der `SwipeView` Inhalt eine [`Grid`](xref:Xamarin.Forms.Grid) , die die Darstellung der einzelnen Elemente im [`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert. Die wischen-Elemente werden verwendet, um Aktionen für den `SwipeView` Inhalt auszuführen. Sie werden angezeigt, wenn das Steuerelement von der linken Seite aus angezeigt wird:

[![Screenshot der Kontextmenü Elemente von "CollectionView" unter IOS und Android](populate-data-images/swipeview.png "CollectionView mit Kontextmenü Elementen von swipeer View")](populate-data-images/swipeview-large.png#lightbox "CollectionView mit Kontextmenü Elementen von swipeer View")

`SwipeView` unterstützt vier verschiedene Streifen Richtungen, wobei die Richtung des direktionalen `SwipeItems` der Auflistung, der die `SwipeItems` Objekte hinzugefügt werden, die Richtung definiert wird. Standardmäßig wird ein Schwenk Element ausgeführt, wenn es vom Benutzer getippt wird. Außerdem werden nach dem Ausführen eines Schwenk Elements die Schwenk Elemente ausgeblendet, und der `SwipeView` Inhalt wird erneut angezeigt. Diese Verhaltensweisen können jedoch geändert werden.

Weitere Informationen zum `SwipeView` Steuerelement finden Sie unter [xamarin. Forms swipeer View](~/xamarin-forms/user-interface/swipeview.md).

## <a name="pull-to-refresh"></a>Aktualisieren durch Ziehen

[`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt die Funktion zum Aktualisieren von Pull-Funktionen über die `RefreshView`, wodurch die Aktualisierung der Daten durch Abrufen der Liste der Elemente ermöglicht wird. Der `RefreshView` ist ein Container Steuerelement, das dem untergeordneten Pullvorgang zur Aktualisierungs Funktion bereitstellt, vorausgesetzt, das untergeordnete Element unterstützt scrollbaren Inhalt. Der Pull-Vorgang zum Aktualisieren wird daher für eine `CollectionView` implementiert, indem Sie als untergeordnetes Element eines `RefreshView`festgelegt wird:

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CollectionView ItemsSource="{Binding Animals}">
        ...
    </CollectionView>
</RefreshView>
```

Der entsprechende C#-Code lautet:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = collectionView;
// ...
```

Wenn der Benutzer eine Aktualisierung initiiert, wird die von der `Command`-Eigenschaft definierte `ICommand` ausgeführt, wodurch die angezeigten Elemente aktualisiert werden. Eine Aktualisierungs Visualisierung wird angezeigt, während die Aktualisierung durchgeführt wird, die aus einem animierten Fortschritts Kreis besteht:

[![Bildschirm Abbildung von Pull-to-Refresh in CollectionView unter IOS und Android](populate-data-images/pull-to-refresh.png "Sammlungsansicht: Pull-to-refresh")](populate-data-images/pull-to-refresh-large.png#lightbox "Sammlungsansicht: Pull-to-refresh")

Der Wert der `RefreshView.IsRefreshing`-Eigenschaft gibt den aktuellen Status des `RefreshView`an. Wenn eine Aktualisierung durch den Benutzer ausgelöst wird, wird diese Eigenschaft automatisch zu `true`übergehen. Nachdem die Aktualisierung abgeschlossen ist, sollten Sie die-Eigenschaft auf `false`zurücksetzen.

Weitere Informationen zu `RefreshView`finden Sie unter [xamarin. Forms erfrischendes View](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Inkrementelles Laden von Daten

[`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt das inkrementelle Laden von Daten, wenn Benutzer durch Elemente scrollen. Dies ermöglicht Szenarios, wie z. b. das asynchrone Laden einer Datenseite aus einem Webdienst, während der Benutzer einen Bildlauf ausführt. Außerdem ist der Punkt, an dem mehr Daten geladen werden, so konfigurierbar, dass Benutzern kein leerer Bereich angezeigt wird oder der Bildlauf angehalten wird.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert die folgenden Eigenschaften, um das inkrementelle Laden von Daten zu steuern:

- `RemainingItemsThreshold`vom Typ `int`den Schwellenwert für Elemente, die noch nicht in der Liste angezeigt werden, bei der das `RemainingItemsThresholdReached` Ereignis ausgelöst wird.
- `RemainingItemsThresholdReachedCommand`vom Typ `ICommand`, der ausgeführt wird, wenn die `RemainingItemsThreshold` erreicht wird.
- `RemainingItemsThresholdReachedCommandParameter` vom Typ `object`: der Parameter, der an `RemainingItemsThresholdReachedCommand` übergeben wird.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert auch ein `RemainingItemsThresholdReached` Ereignis, das ausgelöst wird, wenn der `CollectionView` einen Rollup hat, dass `RemainingItemsThreshold` Elemente nicht angezeigt wurden. Dieses Ereignis kann behandelt werden, um weitere Elemente zu laden. Wenn das `RemainingItemsThresholdReached`-Ereignis ausgelöst wird, wird außerdem der `RemainingItemsThresholdReachedCommand` ausgeführt, sodass inkrementelles Laden von Daten in einem ViewModel erfolgen kann.

Der Standardwert der `RemainingItemsThreshold`-Eigenschaft ist-1, was darauf hinweist, dass das `RemainingItemsThresholdReached` Ereignis nie ausgelöst wird. Wenn der-Eigenschafts Wert 0 ist, wird das `RemainingItemsThresholdReached` Ereignis ausgelöst, wenn das letzte Element in der [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) angezeigt wird. Bei Werten, die größer als 0 sind, wird das `RemainingItemsThresholdReached` Ereignis ausgelöst, wenn die `ItemsSource` die Anzahl der Elemente enthält, für die noch kein Rollup durchgeführt wurde.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) überprüft die `RemainingItemsThreshold`-Eigenschaft, sodass Ihr Wert immer größer oder gleich-1 ist.

Das folgende XAML-Beispiel zeigt eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) , mit der Daten inkrementell geladen werden:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

In diesem Codebeispiel wird das `RemainingItemsThresholdReached`-Ereignis ausgelöst, wenn für fünf Elemente noch kein Rollup durchgeführt wurde, und in der Antwort wird der `OnCollectionViewRemainingItemsThresholdReached`-Ereignishandler ausgeführt:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Daten können auch inkrementell geladen werden, indem die `RemainingItemsThresholdReachedCommand` an eine `ICommand` Implementierung in ViewModel gebunden wird.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. Forms-Ansicht "erfrischend"](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin. Forms swipeer View](~/xamarin-forms/user-interface/swipeview.md)
- [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
