---
title: Xamarin.Forms CollectionView EmptyView
description: In CollectionView kann eine leere Ansicht angegeben werden, dass, Feedback an den Benutzer bereitstellt, wenn keine Daten für die Anzeige verfügbar sind. Die leere Sicht kann es sich um eine Zeichenfolge, eine Sicht oder mehrere Ansichten sein.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 78e9ddcb1d9dd91dadea94016b206867ac9508e6
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048197"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin.Forms CollectionView EmptyView

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` definiert die folgenden Eigenschaften, die mit dem Benutzer Feedback, wenn keine Daten angezeigt werden können:

- `EmptyView`, des Typs `object`, die Zeichenfolge, die Bindung oder die Ansicht, die angezeigt wird, wenn die `ItemsSource` -Eigenschaft ist `null`, oder wenn die Auflistung, die vom angegebenen die `ItemsSource` -Eigenschaft ist `null` oder leer. Der Standardwert ist `null`.
- `EmptyViewTemplate`, des Typs [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), die Vorlage, mit dem angegebenen format `EmptyView`. Der Standardwert ist `null`.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können.

Die wichtigsten Verwendungsszenarien für die Einstellung der `EmptyView` Eigenschaft werden bei einem Filtervorgang auf Feedback von Benutzern angezeigt ein `CollectionView` ergibt keine Daten und Anzeigen von Benutzerfeedback, während die Daten von einem Webdienst abgerufen wird.

> [!NOTE]
> Die `EmptyView` Eigenschaft kann festgelegt werden, um einer Sicht, interaktiven Inhalt enthält, falls erforderlich.

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Eine Zeichenfolge angezeigt, wenn Daten nicht verfügbar ist.

Die `EmptyView` Eigenschaft kann festgelegt werden, in eine Zeichenfolge, die angezeigt wird, wenn die `ItemsSource` -Eigenschaft ist `null`, oder wenn die Auflistung, die vom angegebenen die `ItemsSource` -Eigenschaft ist `null` oder leer sein. Der folgende XAML zeigt ein Beispiel für dieses Szenario:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

Das Ergebnis ist, dass, da das datengebundene Auflistung `null`, legen Sie die Zeichenfolge als das `EmptyView` -Eigenschaftswert angezeigt wird:

[![Screenshot von einer vertikalen Liste der CollectionView mit einer leeren Textansicht, für iOS und Android](emptyview-images/null-itemssource.png "CollectionView vertikale Liste mit leeren Textansicht")](emptyview-images/null-itemssource-large.png#lightbox "CollectionView vertikale Liste mit Text leer ansehen")

## <a name="display-views-when-data-is-unavailable"></a>Ansichten angezeigt, wenn Daten nicht verfügbar ist.

Die `EmptyView` Eigenschaft kann festgelegt werden, um eine Ansicht, die angezeigt wird, wenn die `ItemsSource` -Eigenschaft ist `null`, oder wenn die Auflistung, die vom angegebenen die `ItemsSource` -Eigenschaft ist `null` oder leer sein. Dies kann sein, eine einzelne Ansicht oder eine Sicht, die mehrere untergeordnete Ansichten enthält. Das folgende XAML-Beispiel zeigt die `EmptyView` -Eigenschaft auf eine Ansicht, die mehrere untergeordnete Ansichten enthält:

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

Der entsprechende C#-Code ist:

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

Bei der [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) führt die `FilterCommand`, der Auflistung, die angezeigt wird der `CollectionView` wird gefiltert, für der gesuchten Begriff als gespeichert der [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) Eigenschaft. Wenn der Filtervorgang keine Daten, ergibt die [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) als Festlegen der `EmptyView` -Eigenschaftswert angezeigt wird:

[![Screenshot einer CollectionView vertikale Liste mit benutzerdefinierten leere Sicht, für iOS und Android](emptyview-images/filter-multiple-views.png "CollectionView vertikale Liste mit benutzerdefinierten leere Ansicht")](emptyview-images/filter-multiple-views-large.png#lightbox "CollectionView vertikale Liste mit benutzerdefinierten leere Ansicht")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Einen auf Vorlagen basierenden, benutzerdefinierten Typ angezeigt wird, wenn Daten nicht verfügbar ist.

Die `EmptyView` Eigenschaft kann festgelegt werden, um einen benutzerdefinierten Typ, dessen Vorlage ist angezeigt wird, wenn die `ItemsSource` -Eigenschaft ist `null`, oder wenn die Auflistung, die vom angegebenen der `ItemsSource` -Eigenschaft ist `null` oder leer sein. Die `EmptyViewTemplate` Eigenschaft kann festgelegt werden, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , definiert die Darstellung der `EmptyView`. Der folgende XAML zeigt ein Beispiel für dieses Szenario:

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

Der entsprechende C#-Code ist:

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

Die `FilterData` Typ definiert ein `Filter` -Eigenschaft und einem entsprechenden [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty):

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

Die `EmptyView` -Eigenschaftensatz auf eine `FilterData` -Objekt, und die `Filter` Eigenschaftendaten bindet an die [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) Eigenschaft. Bei der [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) führt die `FilterCommand`, der Auflistung, die angezeigt wird der `CollectionView` wird gefiltert, für der gesuchten Begriff als gespeichert der `Filter` Eigenschaft. Wenn der Filtervorgang keine Daten ergibt die [ `Label` ](xref:Xamarin.Forms.Label) in definiert die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), festgelegt ist, als die `EmptyViewTemplate` Eigenschaftswert angezeigt wird:

[![Screenshot von einer vertikalen Liste der CollectionView mit einer Vorlage leere Ansicht unter iOS und Android](emptyview-images/emptyviewtemplate.png "CollectionView vertikale Liste mit einer ansichtsvorlage der leeren")](emptyview-images/emptyviewtemplate-large.png#lightbox "CollectionView vertikale Liste mit Vorlage für leere Ansicht")

> [!NOTE]
> Wenn einen auf Vorlagen basierenden, benutzerdefinierten Typ angezeigt werden soll, wenn Daten nicht verfügbar ist, sind die `EmptyViewTemplate` Eigenschaft kann festgelegt werden, um eine Ansicht, die mehrere untergeordnete Ansichten enthält.

## <a name="choose-an-emptyview-at-runtime"></a>Wählen Sie eine EmptyView zur Laufzeit

Sichten, die als angezeigt werden ein `EmptyView` Wenn Daten nicht verfügbar ist, kann als definiert werden [ `ContentView` ](xref:Xamarin.Forms.ContentView) Objekte in einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Die `EmptyView` können dann Eigenschaftensatz für eine bestimmte `ContentView`basierend auf Geschäftslogik zur Laufzeit. Im folgende XAML-Beispiel zeigt ein Beispiel für dieses Szenario:

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

Dieses XAML definiert zwei [ `ContentView` ](xref:Xamarin.Forms.ContentView) Objekte in der auf Seitenebene [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), mit der [ `Switch` ](xref:Xamarin.Forms.Switch) -Objekt, das `ContentView` Objekt wird als festgelegt werden die `EmptyView` -Eigenschaftswert. Wenn die [ `Switch` ](xref:Xamarin.Forms.Switch) ein-/ausgeschaltet ist, die `OnEmptyViewSwitchToggled` -Ereignishandler ausgeführt wird die `ToggleEmptyView` Methode:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

Die `ToggleEmptyView` Methode wird die `EmptyView` Eigenschaft der `collectionView` Objekt, das eine der beiden [ `ContentView` ](xref:Xamarin.Forms.ContentView) im gespeicherten Objekte die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)basierend auf den Wert des der [ `Switch.IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Eigenschaft. Bei der [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) führt die `FilterCommand`, der Auflistung, die angezeigt wird der `CollectionView` wird gefiltert, für der gesuchten Begriff als gespeichert der [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) Eigenschaft. Wenn der Filtervorgang keine Daten, ergibt die `ContentView` Objekt festgelegt werden, als die `EmptyView` Eigenschaft wird angezeigt:

[![Screenshot von einer vertikalen Liste der CollectionView mit vertauscht leere Ansichten unter iOS und Android](emptyview-images/swap.png "CollectionView vertikale Liste mit vertauscht leere Ansichten")](emptyview-images/swap-large.png#lightbox "CollectionView vertikale Liste mit Vertauscht die leere Ansichten")

Weitere Informationen zu Ressourcenwörterbüchern, finden Sie unter [Xamarin.Forms Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Wählen Sie eine EmptyViewTemplate zur Laufzeit

Die Darstellung der `EmptyView` kann ausgewählt werden, zur Laufzeit basierend auf den Wert festlegen die `CollectionView.EmptyViewTemplate` Eigenschaft, um eine [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) Objekt:

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

Der entsprechende C#-Code ist:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Die `EmptyView` -Eigenschaftensatz auf die [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) -Eigenschaft, und die `EmptyViewTemplate` -Eigenschaftensatz auf eine `SearchTermDataTemplateSelector` Objekt.

Bei der [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) führt die `FilterCommand`, der Auflistung, die angezeigt wird der `CollectionView` wird gefiltert, für der gesuchten Begriff als gespeichert der [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) Eigenschaft. Wenn der Filtervorgang keine Daten ergibt die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) vom ausgewählten der `SearchTermDataTemplateSelector` Objekt festgelegt ist, als die `EmptyViewTemplate` Eigenschaft angezeigt.

Das folgende Beispiel zeigt die `SearchTermDataTemplateSelector` Klasse:

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

Die `SearchTermTemplateSelector` -Klasse definiert `DefaultTemplate` und `OtherTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf verschiedene Datenvorlagen festgelegt sind. Die `OnSelectTemplate` außer Kraft setzen gibt `DefaultTemplate`, die in einer Meldung für den Benutzer bei die Abfrage nicht gleich "Xamarin" ist. Wenn die Suchabfrage "Xamarin" entspricht der `OnSelectTemplate` außer Kraft setzen gibt `OtherTemplate`, die eine einfache Nachricht dem Benutzer werden angezeigt:

[![Screenshot einer CollectionView Runtime leere Ansicht Vorlage Auswahl unter iOS und Android](emptyview-images/datatemplateselector.png "Vorlagenauswahl der Common Language Runtime-leere Ansicht in einem CollectionView")](emptyview-images/datatemplateselector-large.png#lightbox "runtimevorlage-leere Ansicht Auswahl in einem CollectionView")

Weitere Informationen zu Daten Vorlage Selektoren, finden Sie unter [Erstellen einer Xamarin.Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms-Ressourcenwörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Erstellen einer Xamarin.Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
