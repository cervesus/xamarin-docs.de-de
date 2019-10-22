---
title: Xamarin. Forms CollectionView emptyview
description: In CollectionView kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 9302b9a353173b0445d1d696849b4956134e555d
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696893"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin. Forms CollectionView emptyview

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert die folgenden Eigenschaften, die verwendet werden können, um Benutzer Feedback bereitzustellen, wenn keine anzuzeigenden Daten vorhanden sind:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)vom Typ `object`, der Zeichenfolge, der Bindung oder der Ansicht, die angezeigt wird, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft `null` ist, oder wenn die von der `ItemsSource`-Eigenschaft angegebene Auflistung `null` oder leer ist. Der Standardwert ist `null`sein.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)die Vorlage, die zum Formatieren der angegebenen `EmptyView` verwendet werden soll. Der Standardwert ist `null`sein.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Die wichtigsten Verwendungs Szenarien für das Festlegen der [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft sind das Anzeigen von Benutzer Feedback, wenn ein Filter Vorgang für eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) keine Daten zurückgibt und das Benutzer Feedback anzeigt, während Daten von einem Webdienst abgerufen werden.

> [!NOTE]
> Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf eine Ansicht festgelegt werden, die bei Bedarf interaktiven Inhalt enthält.

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Zeigt eine Zeichenfolge an, wenn Daten nicht verfügbar sind.

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf eine Zeichenfolge festgelegt werden, die angezeigt wird, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft `null` ist, oder wenn die von der `ItemsSource`-Eigenschaft angegebene Auflistung `null` oder leer ist. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

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

Das Ergebnis ist, dass die als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschafts Wert festgelegte Zeichenfolge angezeigt wird, da die Daten gebundene Auflistung `null` ist:

[![Screenshot einer vertikalen CollectionView-Liste mit einer leeren Textansicht unter IOS und Android](emptyview-images/null-itemssource.png "Vertikale Auflistung von CollectionView mit leerer Textansicht")](emptyview-images/null-itemssource-large.png#lightbox "Vertikale Auflistung von CollectionView mit leerer Textansicht")

## <a name="display-views-when-data-is-unavailable"></a>Anzeigen von Sichten, wenn Daten nicht verfügbar sind

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf eine Ansicht festgelegt werden, die angezeigt wird, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft `null` ist, oder wenn die von der `ItemsSource`-Eigenschaft angegebene Auflistung `null` oder leer ist. Dabei kann es sich um eine einzelne Ansicht oder eine Ansicht handeln, die mehrere untergeordnete Sichten enthält. Das folgende XAML-Beispiel zeigt die `EmptyView`-Eigenschaft, die auf eine Sicht festgelegt ist, die mehrere untergeordnete Sichten enthält:

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

Wenn das [`SearchBar`](xref:Xamarin.Forms.SearchBar) die `FilterCommand` ausführt, wird die vom [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigte Auflistung nach dem in der [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gespeicherten Suchbegriff gefiltert. Wenn beim Filter Vorgang keine Daten angezeigt werden, wird der [`StackLayout`](xref:Xamarin.Forms.StackLayout) als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschafts Wert festgelegt:

[![Screenshot einer vertikalen CollectionView-Liste mit benutzerdefinierter leerer Ansicht unter IOS und Android](emptyview-images/filter-multiple-views.png "Vertikale Auflistung von CollectionView mit benutzerdefinierter leerer Ansicht")](emptyview-images/filter-multiple-views-large.png#lightbox "Vertikale Auflistung von CollectionView mit benutzerdefinierter leerer Ansicht")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Anzeigen eines benutzerdefinierten Typs mit Vorlagen, wenn Daten nicht verfügbar sind

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf einen benutzerdefinierten Typ festgelegt werden, dessen Vorlage angezeigt wird, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft `null` ist, oder wenn die von der `ItemsSource`-Eigenschaft angegebene Auflistung `null` oder leer ist. Die [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft kann auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt werden, die die Darstellung des `EmptyView` definiert. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

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

Der `FilterData` Typ definiert eine `Filter` Eigenschaft und eine entsprechende [`BindableProperty`](xref:Xamarin.Forms.BindableProperty):

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

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft wird auf ein `FilterData` Objekt festgelegt, und die `Filter`-Eigenschaften Daten werden an die [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gebunden. Wenn das [`SearchBar`](xref:Xamarin.Forms.SearchBar) die `FilterCommand` ausführt, wird die vom [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigte Auflistung nach dem in der `Filter`-Eigenschaft gespeicherten Suchbegriff gefiltert. Wenn der Filter Vorgang keine Daten ergibt, wird der [`Label`](xref:Xamarin.Forms.Label) , der in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)definiert ist, die als [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschafts Wert festgelegt ist, angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit einer leeren Ansichts Vorlage unter IOS und Android](emptyview-images/emptyviewtemplate.png "Vertikale Auflistung von CollectionView mit leerer Ansichts Vorlage")](emptyview-images/emptyviewtemplate-large.png#lightbox "Vertikale Auflistung von CollectionView mit leerer Ansichts Vorlage")

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Typ mit Vorlagen anzeigen, wenn Daten nicht verfügbar sind, kann die [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft auf eine Ansicht festgelegt werden, die mehrere untergeordnete Sichten enthält.

## <a name="choose-an-emptyview-at-runtime"></a>Emptyview zur Laufzeit auswählen

Sichten, die als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) angezeigt werden, wenn Daten nicht verfügbar sind, können als [`ContentView`](xref:Xamarin.Forms.ContentView) Objekte in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)definiert werden. Die `EmptyView`-Eigenschaft kann dann zur Laufzeit auf einen bestimmten `ContentView` festgelegt werden, der auf einer Geschäftslogik basiert. Das folgende XAML-Beispiel zeigt ein Beispiel für dieses Szenario:

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

Dieser XAML-Code definiert zwei [`ContentView`](xref:Xamarin.Forms.ContentView) Objekte in der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Seitenebene, wobei das [`Switch`](xref:Xamarin.Forms.Switch) Objekt steuert, welches `ContentView` Objekt als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschafts Wert festgelegt wird. Wenn die [`Switch`](xref:Xamarin.Forms.Switch) ein-/ausgeschaltet wird, führt der `OnEmptyViewSwitchToggled`-Ereignishandler die `ToggleEmptyView`-Methode aus:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

Die `ToggleEmptyView`-Methode legt die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft des `collectionView`-Objekts basierend auf dem Wert der [`ResourceDictionary`](xref:Xamarin.Forms.Switch.IsToggled) -Eigenschaft auf eines der beiden [`ContentView`](xref:Xamarin.Forms.ContentView) -Objekte fest, die im [`Switch.IsToggled`](xref:Xamarin.Forms.ResourceDictionary)gespeichert sind. Wenn das [`SearchBar`](xref:Xamarin.Forms.SearchBar) die `FilterCommand` ausführt, wird die vom [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigte Auflistung nach dem in der [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gespeicherten Suchbegriff gefiltert. Wenn der Filter Vorgang keine Daten ergibt, wird das `ContentView` Objekt, das als `EmptyView`-Eigenschaft festgelegt ist, angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit vertauschten leeren Ansichten unter IOS und Android](emptyview-images/swap.png "Vertikale CollectionView-Liste mit vertauschten leeren Ansichten")](emptyview-images/swap-large.png#lightbox "Vertikale CollectionView-Liste mit vertauschten leeren Ansichten")

Weitere Informationen zu Ressourcen Wörterbüchern finden Sie unter [xamarin. Forms-Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Emptyviewtemplate zur Laufzeit auswählen

Die Darstellung des [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) kann zur Laufzeit basierend auf seinem Wert ausgewählt werden, indem die [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft auf ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) Objekt festgelegt wird:

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

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft wird auf die [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft festgelegt, und die [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft wird auf ein `SearchTermDataTemplateSelector` Objekt festgelegt.

Wenn das [`SearchBar`](xref:Xamarin.Forms.SearchBar) die `FilterCommand` ausführt, wird die vom [`CollectionView`](xref:Xamarin.Forms.CollectionView) angezeigte Auflistung nach dem in der [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gespeicherten Suchbegriff gefiltert. Wenn der Filter Vorgang keine Daten ergibt, wird der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der vom `SearchTermDataTemplateSelector`-Objekt ausgewählt wird, als [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft festgelegt und angezeigt.

Das folgende Beispiel zeigt die `SearchTermDataTemplateSelector`-Klasse:

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

Die `SearchTermTemplateSelector`-Klasse definiert `DefaultTemplate` und `OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf unterschiedliche Datenvorlagen festgelegt sind. Das `OnSelectTemplate` Überschreibungs Zeichen gibt `DefaultTemplate` zurück, das eine Meldung für den Benutzer anzeigt, wenn die Suchabfrage nicht gleich "xamarin" ist. Wenn die Suchabfrage gleich "xamarin" ist, gibt die `OnSelectTemplate` Überschreibung `OtherTemplate` zurück, die dem Benutzer eine grundlegende Meldung anzeigt:

[![Screenshot der Vorlage für eine leere Ansichts Vorlage der CollectionView-Laufzeit unter IOS und Android](emptyview-images/datatemplateselector.png "Vorlage für leere Lauf Zeitansicht in einer CollectionView")](emptyview-images/datatemplateselector-large.png#lightbox "Vorlage für leere Lauf Zeitansicht in einer CollectionView")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. Forms-Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
