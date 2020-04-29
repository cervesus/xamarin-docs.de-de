---
title: Bindbare Layouts in xamarin. Forms
description: Bindbare Layouts ermöglichen layoutklassen das Generieren ihres Inhalts durch Binden an eine Auflistung von Elementen, mit der Option, die Darstellung der einzelnen Elemente mit einem DataTemplate festzulegen.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/09/2020
ms.openlocfilehash: d084d910786c24a4b0333ecbc3e893cfe144d404
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517255"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Bindbare Layouts in xamarin. Forms

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Bindbare Layouts ermöglichen es allen layoutklassen, die [`Layout<T>`](xref:Xamarin.Forms.Layout`1) von der-Klasse abgeleitet sind, den Inhalt durch Binden an eine Auflistung von Elementen zu generieren. die Möglichkeit, die Darstellung der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)einzelnen Elemente mit einem festzulegen. Bindbare Layouts werden von der `BindableLayout` -Klasse bereitgestellt, die die folgenden angefügten Eigenschaften verfügbar macht:

- `ItemsSource`– Gibt die Sammlung von `IEnumerable` Elementen an, die vom Layout angezeigt werden sollen.
- `ItemTemplate`– Gibt das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, das auf jedes Element in der Auflistung von Elementen angewendet werden soll, die vom Layout angezeigt werden.
- `ItemTemplateSelector`– Gibt das [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) an, das zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

> [!NOTE]
> Die `ItemTemplate` -Eigenschaft hat Vorrang, wenn `ItemTemplate` die `ItemTemplateSelector` -Eigenschaft und die-Eigenschaft festgelegt sind.

Außerdem macht die `BindableLayout` -Klasse die folgenden bindbaren Eigenschaften verfügbar:

- `EmptyView`– Gibt die `string` -oder-Ansicht an, die angezeigt `ItemsSource` wird, `null`wenn die-Eigenschaft ist, oder wenn `ItemsSource` die von `null` der-Eigenschaft angegebene Auflistung oder leer ist. Standardwert: `null`.
- `EmptyViewTemplate`– Gibt den [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, der angezeigt wird, `ItemsSource` wenn die `null`-Eigenschaft ist, oder wenn die von `ItemsSource` der- `null` Eigenschaft angegebene Auflistung oder leer ist. Standardwert: `null`.

> [!NOTE]
> Die `EmptyViewTemplate` -Eigenschaft hat Vorrang, wenn `EmptyView` die `EmptyViewTemplate` -Eigenschaft und die-Eigenschaft festgelegt sind.

Alle diese Eigenschaften [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)können an die-, [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`Grid`](xref:Xamarin.Forms.Grid) [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)-,-,-und [`StackLayout`](xref:Xamarin.Forms.StackLayout) -Klassen angefügt werden, die [`Layout<T>`](xref:Xamarin.Forms.Layout`1) alle von der-Klasse abgeleitet sind.

Die `Layout<T>` -Klasse macht [`Children`](xref:Xamarin.Forms.Layout`1.Children) eine Auflistung verfügbar, der die untergeordneten Elemente eines Layouts hinzugefügt werden. Wenn die `BinableLayout.ItemsSource` Eigenschaft auf eine Auflistung von Elementen festgelegt und an eine [`Layout<T>`](xref:Xamarin.Forms.Layout`1)von abgeleitete Klasse angefügt wird, wird jedes Element in der Auflistung `Layout<T>.Children` der Auflistung für die Anzeige durch das Layout hinzugefügt. Die `Layout<T>`von abgeleitete Klasse aktualisiert dann die untergeordneten Sichten, wenn sich die zugrunde liegende Auflistung ändert. Weitere Informationen zum xamarin. Forms-layoutcycle finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Bindbare Layouts sollten nur verwendet werden, wenn die Auflistung von Elementen, die angezeigt werden sollen, klein ist und ein Bildlauf und eine Auswahl nicht erforderlich sind. Wenn Sie einen Bildlauf durchführen können, indem Sie ein bindbare Layout in einem [`ScrollView`](xref:Xamarin.Forms.ScrollView)umgestalten, wird dies nicht empfohlen, da es bei bindbaren Layouts keine UI-Virtualisierung Wenn ein Bildlauf erforderlich ist, sollte eine Bild lauffähige Sicht verwendet [`ListView`](xref:Xamarin.Forms.ListView) werden [`CollectionView`](xref:Xamarin.Forms.CollectionView), die die UI-Virtualisierung wie oder enthält. Wenn Sie diese Empfehlung nicht beachten, kann dies zu Leistungsproblemen führen.

> [!IMPORTANT]
> Obwohl es technisch möglich ist, ein bindbares Layout an eine beliebige Layoutklasse anzufügen, [`Layout<T>`](xref:Xamarin.Forms.Layout`1) die von der-Klasse abgeleitet ist, ist dies nicht immer praktisch, [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)insbesondere [`Grid`](xref:Xamarin.Forms.Grid)für die [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Klassen, und. Angenommen, Sie möchten eine Auflistung von Daten in einem [`Grid`](xref:Xamarin.Forms.Grid) mithilfe eines bindbaren Layouts anzeigen, wobei jedes Element in der Auflistung ein Objekt mit mehreren Eigenschaften ist. Jede Zeile in der `Grid` sollte ein Objekt aus der Auflistung anzeigen, wobei jede Spalte in der `Grid` eine der Eigenschaften des Objekts anzeigt. Da der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für das bindbare Layout nur ein einzelnes Objekt enthalten kann, ist es erforderlich, dass dieses Objekt eine Layoutklasse mit mehreren Ansichten ist, die jeweils eine der Eigenschaften des Objekts in einer bestimmten `Grid` Spalte anzeigen. Obwohl dieses Szenario mit bindbaren Layouts realisiert werden kann, führt es zu einem `Grid` `Grid` übergeordneten Element, das ein untergeordnetes Element für jedes Element in der gebundenen Auflistung enthält. Dies ist eine hochgradig `Grid` ineffiziente und problematische Verwendung des Layouts.

## <a name="populate-a-bindable-layout-with-data"></a>Auffüllen eines bindbaren Layouts mit Daten

Ein bindbares Layout wird mit Daten aufgefüllt, indem die `ItemsSource` zugehörige-Eigenschaft auf eine `IEnumerable`beliebige Auflistung festgelegt wird, [`Layout<T>`](xref:Xamarin.Forms.Layout`1)die implementiert, und an eine von abgeleitete Klasse angehängt wird:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Der entsprechende C#-Code lautet:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Wenn die `BindableLayout.ItemsSource` angefügte-Eigenschaft für ein Layout festgelegt ist `BindableLayout.ItemTemplate` , die angefügte-Eigenschaft jedoch nicht fest `IEnumerable` gelegt ist, wird jedes Element [`Label`](xref:Xamarin.Forms.Label) in der Auflistung von einem `BindableLayout` angezeigt, das von der-Klasse erstellt wird.

## <a name="define-item-appearance"></a>Element Darstellung definieren

Die Darstellung der einzelnen Elemente im bindbaren Layout kann definiert werden, indem die `BindableLayout.ItemTemplate` angefügte-Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

Der entsprechende C#-Code lautet:

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

In diesem Beispiel wird jedes Element in der `TopFollowers` Auflistung von einer `CircleImage` Ansicht angezeigt, die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)in definiert ist:

![Bindable Layout mit einem DataTemplate](bindable-layouts-images/top-followers.png "Bindable Layout mit einer Daten Vorlage")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im bindbaren Layout kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `BindableLayout.ItemTemplateSelector` angefügte-Eigenschaft auf festgelegt [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)wird:

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

Der entsprechende C#-Code lautet:

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

Das [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) in der Beispielanwendung verwendete ist im folgenden Beispiel dargestellt:

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

Die `TechItemTemplateSelector` -Klasse `DefaultTemplate` definiert `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) die Eigenschaften und, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` -Methode gibt `XamarinFormsTemplate`den zurück, der ein Element in einem dunkelroten mit einem Herz daneben anzeigt, wenn das Element gleich "xamarin. Forms" ist. Wenn das Element nicht gleich "xamarin. Forms" ist, gibt `OnSelectTemplate` die Methode den `DefaultTemplate`zurück, der ein Element mit der Standardfarbe eines [`Label`](xref:Xamarin.Forms.Label)anzeigt:

![Bindable Layout mit DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Bindable Layout mit Datenvorlagen Auswahl")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Zeigt eine Zeichenfolge an, wenn Daten nicht verfügbar sind.

Die `EmptyView` -Eigenschaft kann auf eine Zeichenfolge festgelegt werden, die von einem [`Label`](xref:Xamarin.Forms.Label) angezeigt wird `ItemsSource` , wenn `null`die-Eigenschaft ist, oder wenn die `ItemsSource` von der `null` -Eigenschaft angegebene Auflistung den Wert oder leer ist. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

Das Ergebnis ist, dass bei der Daten gebundenen Auflistung `null`die Zeichenfolge, die als `EmptyView` Eigenschafts Wert festgelegt ist, angezeigt wird:

[![Screenshot einer bindbaren layoutzeichenfolge leere Ansicht unter IOS und Android](bindable-layouts-images/emptyview-string.png "Bindbare layoutzeichenfolge leere Ansicht")](bindable-layouts-images/emptyview-string-large.png#lightbox "Bindbare layoutzeichenfolge leere Ansicht")

## <a name="display-views-when-data-is-unavailable"></a>Anzeigen von Sichten, wenn Daten nicht verfügbar sind

Die `EmptyView` -Eigenschaft kann auf eine Ansicht festgelegt werden, die angezeigt wird, `ItemsSource` wenn die `null`-Eigenschaft ist, oder wenn die von `ItemsSource` der- `null` Eigenschaft angegebene Auflistung oder leer ist. Dabei kann es sich um eine einzelne Ansicht oder eine Ansicht handeln, die mehrere untergeordnete Sichten enthält. Das folgende XAML-Beispiel zeigt `EmptyView` , wie die-Eigenschaft auf eine Ansicht festgelegt ist, die mehrere untergeordnete Sichten enthält:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyView>
        <StackLayout>
            <Label Text="None."
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
            <Label Text="Try harder and return later?"
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
        </StackLayout>
    </BindableLayout.EmptyView>
    ...
</StackLayout>
```

Das Ergebnis ist, dass bei der Daten gebundenen `null`Auflistung die [`StackLayout`](xref:Xamarin.Forms.StackLayout) und ihre untergeordneten Sichten angezeigt werden.

[![Screenshot einer leeren Ansicht mit bindbarem Layout mit mehreren Ansichten unter IOS und Android](bindable-layouts-images/emptyview-views.png "Leere Ansicht des bindbaren Layouts")](bindable-layouts-images/emptyview-views-large.png#lightbox "Leere Ansicht des bindbaren Layouts")

Ebenso kann die `EmptyViewTemplate` auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt werden, der angezeigt wird, wenn die `ItemsSource` -Eigenschaft `null`ist, oder wenn die von der `ItemsSource` -Eigenschaft angegebene `null` Auflistung oder leer ist. Die `DataTemplate` kann eine einzelne Ansicht oder eine Sicht enthalten, die mehrere untergeordnete Sichten enthält. Außerdem `BindingContext` `EmptyViewTemplate` wird der von der von geerbt `BindingContext` `BindableLayout`. Das folgende XAML-Beispiel zeigt `EmptyViewTemplate` , wie die- `DataTemplate` Eigenschaft auf festgelegt ist, die eine einzelne Ansicht enthält:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyViewTemplate>
        <DataTemplate>
            <Label Text="{Binding Source={x:Reference usernameLabel}, Path=Text, StringFormat='{0} has no achievements.'}" />
        </DataTemplate>
    </BindableLayout.EmptyViewTemplate>
    ...
</StackLayout>
```

Das Ergebnis ist, dass die `null` [`Label`](xref:Xamarin.Forms.Label) in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angezeigt wird, wenn die Daten gebundene Auflistung ist:

[![Screenshot einer leeren Ansichts Vorlage für das bindbare Layout unter IOS und Android](bindable-layouts-images/emptyviewtemplate.png "Vorlage für leere Vorlage mit bindbarem Layout")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "Vorlage für leere Vorlage mit bindbarem Layout")

> [!NOTE]
> Die `EmptyViewTemplate` -Eigenschaft kann nicht über einen [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)festgelegt werden.

## <a name="choose-an-emptyview-at-runtime"></a>Emptyview zur Laufzeit auswählen

Sichten, die als angezeigt werden, `EmptyView` wenn Daten nicht verfügbar sind, können in einer [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)als-Objekte definiert werden. Die `EmptyView` -Eigenschaft kann dann zur Laufzeit auf einen `ContentView`bestimmten festgelegt werden, der auf einer Geschäftslogik basiert. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...    
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No achievements."
                       FontSize="14" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="None."
                       FontAttributes="Italic"
                       FontSize="14" />
                <Label Text="Try harder and return later?"
                       FontAttributes="Italic"
                       FontSize="14" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <Switch Toggled="OnEmptyViewSwitchToggled" />

        <StackLayout x:Name="stackLayout"
                     BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
            ...
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Der XAML-Code [`ContentView`](xref:Xamarin.Forms.ContentView) definiert zwei Objekte auf Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), wobei das [`Switch`](xref:Xamarin.Forms.Switch) Objekt steuert, `ContentView` welches Objekt als `EmptyView` Eigenschafts Wert festgelegt wird. Wenn das `Switch` -Objekt ein-/ausgeschaltet `OnEmptyViewSwitchToggled` wird, führt der `ToggleEmptyView` Ereignishandler die-Methode aus:

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

Die `ToggleEmptyView` -Methode legt `EmptyView` die-Eigenschaft `stackLayout` des-Objekts basierend auf dem [`ContentView`](xref:Xamarin.Forms.ContentView) Wert der- [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) Eigenschaft [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf eines der beiden-Objekte fest, die im gespeichert sind. Wenn die Daten gebundene Auflistung ist `null`, wird das Objekt `ContentView` , das als- `EmptyView` Eigenschaft festgelegt ist, angezeigt:

[![Screenshot der Auswahl leerer Ansichten zur Laufzeit unter IOS und Android](bindable-layouts-images/emptyview-runtime.png "Bindable Layout leere Ansicht Lauf Zeitauswahl")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "Bindable Layout leere Ansicht Lauf Zeitauswahl")

## <a name="related-links"></a>Verwandte Links

- [Demo zu bindbaren Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen einer Xamarin.Forms-DataTemplateSelector-Klasse](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
