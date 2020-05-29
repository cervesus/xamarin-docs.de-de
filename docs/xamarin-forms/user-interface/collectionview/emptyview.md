---
title: Xamarin.FormsCollectionView emptyview
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d35e39e55d66452e47c7a3e3faf86a7a7d6adaca
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136489"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin.FormsCollectionView emptyview

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften, die verwendet werden können, um Benutzer Feedback bereitzustellen, wenn keine anzuzeigenden Daten vorhanden sind:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView), vom Typ `object` , der Zeichenfolge, der Bindung oder der Ansicht, die angezeigt wird, wenn die- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft ist `null` , oder, wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Standardwert: `null`.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate), vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die Vorlage, die zum Formatieren des angegebenen verwendet werden soll `EmptyView` . Standardwert: `null`.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Die wichtigsten Verwendungs Szenarien für das Festlegen der- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschaft sind das Anzeigen von Benutzer Feedback, wenn ein Filter Vorgang für ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) -Objekt keine Daten ergibt und das Benutzer Feedback anzeigt, während Daten von einem Webdienst abgerufen werden.

> [!NOTE]
> Die- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschaft kann auf eine Ansicht festgelegt werden, die bei Bedarf interaktiven Inhalt enthält.

Weitere Informationen zu Datenvorlagen finden Sie unter [ Xamarin.Forms Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Zeigt eine Zeichenfolge an, wenn Daten nicht verfügbar sind.

Die- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschaft kann auf eine Zeichenfolge festgelegt werden, die angezeigt wird, wenn die- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

Das Ergebnis ist, dass `null` die Zeichenfolge, die als Eigenschafts Wert festgelegt ist, angezeigt wird, da die Daten gebundene Auflistung ist [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) :

[![Screenshot einer vertikalen CollectionView-Liste mit einer leeren Textansicht unter IOS und Android](emptyview-images/null-itemssource.png "Vertikale Auflistung von CollectionView mit leerer Textansicht")](emptyview-images/null-itemssource-large.png#lightbox "Vertikale Auflistung von CollectionView mit leerer Textansicht")

## <a name="display-views-when-data-is-unavailable"></a>Anzeigen von Sichten, wenn Daten nicht verfügbar sind

Die- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschaft kann auf eine Ansicht festgelegt werden, die angezeigt wird, wenn die- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Dabei kann es sich um eine einzelne Ansicht oder eine Ansicht handeln, die mehrere untergeordnete Sichten enthält. Das folgende XAML-Beispiel zeigt, wie die- `EmptyView` Eigenschaft auf eine Ansicht festgelegt ist, die mehrere untergeordnete Sichten enthält:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

Der entsprechende C#-Code lautet:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) ausführt `FilterCommand` , wird die von der angezeigte Auflistung [`CollectionView`](xref:Xamarin.Forms.CollectionView) nach dem in der-Eigenschaft gespeicherten Suchbegriff gefiltert [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . Wenn der Filter Vorgang keine Daten ergibt, [`StackLayout`](xref:Xamarin.Forms.StackLayout) wird der Satz als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschafts Wert angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit benutzerdefinierter leerer Ansicht unter IOS und Android](emptyview-images/filter-multiple-views.png "Vertikale Auflistung von CollectionView mit benutzerdefinierter leerer Ansicht")](emptyview-images/filter-multiple-views-large.png#lightbox "Vertikale Auflistung von CollectionView mit benutzerdefinierter leerer Ansicht")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Anzeigen eines benutzerdefinierten Typs mit Vorlagen, wenn Daten nicht verfügbar sind

Die- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschaft kann auf einen benutzerdefinierten Typ festgelegt werden, dessen Vorlage angezeigt wird, wenn die- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Die- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Eigenschaft kann auf einen festgelegt werden [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der die Darstellung des definiert `EmptyView` . Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

Der entsprechende C#-Code lautet:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

Der `FilterData` -Typ definiert eine `Filter` -Eigenschaft und eine entsprechende [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) :

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft ist auf ein `FilterData` -Objekt festgelegt, und die `Filter` Eigenschaften Daten werden an die- [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) Eigenschaft gebunden. Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) ausführt `FilterCommand` , wird die von der angezeigte Auflistung [`CollectionView`](xref:Xamarin.Forms.CollectionView) nach dem in der-Eigenschaft gespeicherten Suchbegriff gefiltert `Filter` . Wenn der Filter Vorgang keine Daten ergibt, [`Label`](xref:Xamarin.Forms.Label) wird die in der definierte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die als [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Eigenschafts Wert festgelegt ist, angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit einer leeren Ansichts Vorlage unter IOS und Android](emptyview-images/emptyviewtemplate.png "Vertikale Auflistung von CollectionView mit leerer Ansichts Vorlage")](emptyview-images/emptyviewtemplate-large.png#lightbox "Vertikale Auflistung von CollectionView mit leerer Ansichts Vorlage")

> [!NOTE]
> Wenn ein benutzerdefinierter Typ mit Vorlagen angezeigt wird, wenn Daten nicht verfügbar sind, kann die- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Eigenschaft auf eine Ansicht festgelegt werden, die mehrere untergeordnete Sichten enthält.

## <a name="choose-an-emptyview-at-runtime"></a>Emptyview zur Laufzeit auswählen

Sichten, die als angezeigt werden [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) , wenn Daten nicht verfügbar sind, können in einer als-Objekte definiert werden [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Die- `EmptyView` Eigenschaft kann dann zur Laufzeit auf einen bestimmten festgelegt werden `ContentView` , der auf einer Geschäftslogik basiert. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

Dieser XAML-Code definiert zwei [`ContentView`](xref:Xamarin.Forms.ContentView) Objekte auf Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , wobei das [`Switch`](xref:Xamarin.Forms.Switch) Objekt steuert, welches `ContentView` Objekt als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschafts Wert festgelegt wird. Wenn das-Objekt ein-/ausgeschaltet [`Switch`](xref:Xamarin.Forms.Switch) wird, `OnEmptyViewSwitchToggled` führt der Ereignishandler die- `ToggleEmptyView` Methode aus:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

Die- `ToggleEmptyView` Methode legt die- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Eigenschaft des- `collectionView` Objekts basierend auf dem Wert der-Eigenschaft auf eines der beiden- [`ContentView`](xref:Xamarin.Forms.ContentView) Objekte fest, die im gespeichert [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) sind [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) . Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) ausführt `FilterCommand` , wird die von der angezeigte Auflistung [`CollectionView`](xref:Xamarin.Forms.CollectionView) nach dem in der-Eigenschaft gespeicherten Suchbegriff gefiltert [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . Wenn der Filter Vorgang keine Daten ergibt, `ContentView` wird das als Eigenschaft festgelegte Objekt `EmptyView` angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit vertauschten leeren Ansichten unter IOS und Android](emptyview-images/swap.png "Vertikale CollectionView-Liste mit vertauschten leeren Ansichten")](emptyview-images/swap-large.png#lightbox "Vertikale CollectionView-Liste mit vertauschten leeren Ansichten")

Weitere Informationen zu Ressourcen Wörterbüchern finden Sie unter [ Xamarin.Forms Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Emptyviewtemplate zur Laufzeit auswählen

Die Darstellung des [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) kann zur Laufzeit basierend auf seinem Wert ausgewählt werden, indem die- [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Eigenschaft auf ein-Objekt festgelegt wird [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <CollectionView ItemsSource="{Binding Monkeys}"
                        EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                        EmptyViewTemplate="{StaticResource SearchSelector}" />
    </StackLayout>
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft wird auf die [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft festgelegt, und die- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Eigenschaft wird auf ein-Objekt festgelegt `SearchTermDataTemplateSelector` .

Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) ausführt `FilterCommand` , wird die von der angezeigte Auflistung [`CollectionView`](xref:Xamarin.Forms.CollectionView) nach dem in der-Eigenschaft gespeicherten Suchbegriff gefiltert [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . Wenn beim Filter Vorgang keine Daten angezeigt werden, wird der, der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) vom- `SearchTermDataTemplateSelector` Objekt ausgewählt wird, als die [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft festgelegt und angezeigt.

Das folgende Beispiel zeigt die- `SearchTermDataTemplateSelector` Klasse:

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

Die `SearchTermTemplateSelector` -Klasse definiert die `DefaultTemplate` `OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften und, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` außer Kraft Setzung gibt zurück `DefaultTemplate` , wodurch dem Benutzer eine Meldung angezeigt wird, wenn die Suchabfrage nicht gleich "xamarin" ist. Wenn die Suchabfrage gleich "xamarin" ist, gibt die `OnSelectTemplate` Überschreibung zurück `OtherTemplate` , wodurch dem Benutzer eine grundlegende Meldung angezeigt wird:

[![Screenshot der Vorlage für eine leere Ansichts Vorlage der CollectionView-Laufzeit unter IOS und Android](emptyview-images/datatemplateselector.png "Vorlage für leere Lauf Zeitansicht in einer CollectionView")](emptyview-images/datatemplateselector-large.png#lightbox "Vorlage für leere Lauf Zeitansicht in einer CollectionView")

Weitere Informationen zu Datenvorlagen Selektoren finden Sie unter [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.FormsDatenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.FormsRessourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Erstellen eines Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
