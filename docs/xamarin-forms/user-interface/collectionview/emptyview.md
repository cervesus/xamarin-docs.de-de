---
title: Xamarin. Forms CollectionView emptyview
description: In CollectionView kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: c6a2a53f267a7f6764ec441944193e8c5ecd9189
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "68739191"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin. Forms CollectionView emptyview

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften, die verwendet werden können, um Benutzer Feedback bereitzustellen, wenn keine anzuzeigenden Daten vorhanden sind:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView), vom Typ `object`, der Zeichenfolge, der Bindung oder der Ansicht, die angezeigt wird [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) , wenn `null`die-Eigenschaft ist, oder, wenn `ItemsSource` die von `null` der-Eigenschaft angegebene Auflistung oder leer ist. Der Standardwert ist `null`sein.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate), vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), die Vorlage, die zum Formatieren des `EmptyView`angegebenen verwendet werden soll. Der Standardwert ist `null`sein.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Die wichtigsten Verwendungs Szenarien für das Festlegen [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) der-Eigenschaft sind das Anzeigen von Benutzer Feedback, wenn [`CollectionView`](xref:Xamarin.Forms.CollectionView) ein Filter Vorgang für ein-Objekt keine Daten ergibt und das Benutzer Feedback anzeigt, während Daten von einem Webdienst abgerufen werden.

> [!NOTE]
> Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf eine Ansicht festgelegt werden, die bei Bedarf interaktiven Inhalt enthält.

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Zeigt eine Zeichenfolge an, wenn Daten nicht verfügbar sind.

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf eine Zeichenfolge festgelegt werden, die angezeigt wird [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) , wenn `null`die-Eigenschaft ist, oder wenn die `ItemsSource` von der `null` -Eigenschaft angegebene Auflistung oder leer ist. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

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

Das Ergebnis ist, dass die Zeichenfolge, die `null` [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) als Eigenschafts Wert festgelegt ist, angezeigt wird, da die Daten gebundene Auflistung ist:

[![Screenshot einer vertikalen CollectionView-Liste mit einer leeren Textansicht unter IOS und Android](emptyview-images/null-itemssource.png "Vertikale Auflistung von CollectionView mit leerer Textansicht")](emptyview-images/null-itemssource-large.png#lightbox "Vertikale Auflistung von CollectionView mit leerer Textansicht")

## <a name="display-views-when-data-is-unavailable"></a>Anzeigen von Sichten, wenn Daten nicht verfügbar sind

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf eine Ansicht festgelegt werden, die angezeigt wird, [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) wenn die `null`-Eigenschaft ist, oder wenn die von `ItemsSource` der- `null` Eigenschaft angegebene Auflistung oder leer ist. Dabei kann es sich um eine einzelne Ansicht oder eine Ansicht handeln, die mehrere untergeordnete Sichten enthält. Das folgende XAML-Beispiel zeigt `EmptyView` , wie die-Eigenschaft auf eine Ansicht festgelegt ist, die mehrere untergeordnete Sichten enthält:

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

Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausführt, wird die von der angezeigte Auflistung nach dem in der [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gespeicherten Suchbegriff gefiltert. `FilterCommand` Wenn der Filter Vorgang keine Daten ergibt, wird [`StackLayout`](xref:Xamarin.Forms.StackLayout) der Satz [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) als Eigenschafts Wert angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit benutzerdefinierter leerer Ansicht unter IOS und Android](emptyview-images/filter-multiple-views.png "Vertikale Auflistung von CollectionView mit benutzerdefinierter leerer Ansicht")](emptyview-images/filter-multiple-views-large.png#lightbox "Vertikale Auflistung von CollectionView mit benutzerdefinierter leerer Ansicht")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Anzeigen eines benutzerdefinierten Typs mit Vorlagen, wenn Daten nicht verfügbar sind

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft kann auf einen benutzerdefinierten Typ festgelegt werden, dessen Vorlage angezeigt [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) wird, `null`wenn die-Eigenschaft ist, oder wenn `ItemsSource` die von `null` der-Eigenschaft angegebene Auflistung oder leer ist. Die [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft kann auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt werden, der die Darstellung `EmptyView`des definiert. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

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

Der `FilterData` -Typ definiert `Filter` eine-Eigenschaft und eine [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)entsprechende:

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

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft ist auf ein `FilterData` -Objekt festgelegt `Filter` , und die Eigenschaften Daten [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) werden an die-Eigenschaft gebunden. Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausführt, wird die von der angezeigte Auflistung nach dem in der `Filter` -Eigenschaft gespeicherten Suchbegriff gefiltert. `FilterCommand` Wenn der Filter Vorgang keine Daten ergibt, wird [`Label`](xref:Xamarin.Forms.Label) die in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)definierte, die als [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Eigenschafts Wert festgelegt ist, angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit einer leeren Ansichts Vorlage unter IOS und Android](emptyview-images/emptyviewtemplate.png "Vertikale Auflistung von CollectionView mit leerer Ansichts Vorlage")](emptyview-images/emptyviewtemplate-large.png#lightbox "Vertikale Auflistung von CollectionView mit leerer Ansichts Vorlage")

> [!NOTE]
> Wenn ein benutzerdefinierter Typ mit Vorlagen angezeigt wird, wenn Daten nicht [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) verfügbar sind, kann die-Eigenschaft auf eine Ansicht festgelegt werden, die mehrere untergeordnete Sichten enthält.

## <a name="choose-an-emptyview-at-runtime"></a>Emptyview zur Laufzeit auswählen

Sichten, die als [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) angezeigt werden, wenn Daten nicht verfügbar sind, können in einer [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)als-Objekte definiert werden. Die `EmptyView` -Eigenschaft kann dann zur Laufzeit auf einen `ContentView`bestimmten festgelegt werden, der auf einer Geschäftslogik basiert. Das folgende XAML-Beispiel zeigt ein Beispiel für dieses Szenario:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
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

Dieser XAML-Code [`ContentView`](xref:Xamarin.Forms.ContentView) definiert zwei Objekte auf Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), wobei das [`Switch`](xref:Xamarin.Forms.Switch) Objekt [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) steuert, `ContentView` welches Objekt als Eigenschafts Wert festgelegt wird. Wenn das [`Switch`](xref:Xamarin.Forms.Switch) -Objekt ein-/ausgeschaltet `OnEmptyViewSwitchToggled` wird, führt der `ToggleEmptyView` Ereignishandler die-Methode aus:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

Die `ToggleEmptyView` -Methode legt [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) die-Eigenschaft `collectionView` des-Objekts basierend auf dem [`ContentView`](xref:Xamarin.Forms.ContentView) Wert der- [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) Eigenschaft [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf eines der beiden-Objekte fest, die im gespeichert sind. Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausführt, wird die von der angezeigte Auflistung nach dem in der [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gespeicherten Suchbegriff gefiltert. `FilterCommand` Wenn der Filter Vorgang keine Daten ergibt, wird `ContentView` das `EmptyView` als Eigenschaft festgelegte Objekt angezeigt:

[![Screenshot einer vertikalen CollectionView-Liste mit vertauschten leeren Ansichten unter IOS und Android](emptyview-images/swap.png "Vertikale CollectionView-Liste mit vertauschten leeren Ansichten")](emptyview-images/swap-large.png#lightbox "Vertikale CollectionView-Liste mit vertauschten leeren Ansichten")

Weitere Informationen zu Ressourcen Wörterbüchern finden Sie unter [xamarin. Forms-Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Emptyviewtemplate zur Laufzeit auswählen

Die Darstellung des [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) kann zur Laufzeit basierend auf seinem Wert ausgewählt werden, indem die [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) -Eigenschaft auf ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) -Objekt festgelegt wird:

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

Die [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) -Eigenschaft wird auf die [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft festgelegt [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) , und die-Eigenschaft `SearchTermDataTemplateSelector` wird auf ein-Objekt festgelegt.

Wenn die [`SearchBar`](xref:Xamarin.Forms.SearchBar) [`CollectionView`](xref:Xamarin.Forms.CollectionView) ausführt, wird die von der angezeigte Auflistung nach dem in der [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft gespeicherten Suchbegriff gefiltert. `FilterCommand` Wenn beim Filter Vorgang keine Daten angezeigt werden, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) wird der, `SearchTermDataTemplateSelector` der vom-Objekt ausgewählt [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) wird, als die-Eigenschaft festgelegt und angezeigt.

Das folgende Beispiel zeigt die `SearchTermDataTemplateSelector` -Klasse:

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

Die `SearchTermTemplateSelector` -Klasse `DefaultTemplate` definiert `OtherTemplate` die Eigenschaften und [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` außer Kraft `DefaultTemplate`Setzung gibt zurück, wodurch dem Benutzer eine Meldung angezeigt wird, wenn die Suchabfrage nicht gleich "xamarin" ist. Wenn die Suchabfrage gleich "xamarin" ist, gibt die `OnSelectTemplate` Überschreibung `OtherTemplate`zurück, wodurch dem Benutzer eine grundlegende Meldung angezeigt wird:

[![Screenshot der Vorlage für eine leere Ansichts Vorlage der CollectionView-Laufzeit unter IOS und Android](emptyview-images/datatemplateselector.png "Vorlage für leere Lauf Zeitansicht in einer CollectionView")](emptyview-images/datatemplateselector-large.png#lightbox "Vorlage für leere Lauf Zeitansicht in einer CollectionView")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. Forms-Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
