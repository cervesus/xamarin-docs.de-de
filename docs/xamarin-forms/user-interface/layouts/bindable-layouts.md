---
title: Bindbare Layouts in xamarin. Forms
description: Bindbare Layouts ermöglichen layoutklassen das Generieren ihres Inhalts durch Binden an eine Auflistung von Elementen, mit der Option, die Darstellung der einzelnen Elemente mit einem DataTemplate festzulegen.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: a824c892d21df9264b772bed09a4aef893f3b949
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647908"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Bindbare Layouts in xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Bindbare Layouts ermöglichen es allen layoutklassen, die [`Layout<T>`](xref:Xamarin.Forms.Layout`1) von der-Klasse abgeleitet sind, den Inhalt durch Binden an eine Auflistung von Elementen zu generieren. die Möglichkeit, die Darstellung der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)einzelnen Elemente mit einem festzulegen. Bindbare Layouts werden von der `BindableLayout` -Klasse bereitgestellt, die die folgenden angefügten Eigenschaften verfügbar macht:

- `ItemsSource`– Gibt die Sammlung von `IEnumerable` Elementen an, die vom Layout angezeigt werden sollen.
- `ItemTemplate`– Gibt das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, das auf jedes Element in der Auflistung von Elementen angewendet werden soll, die vom Layout angezeigt werden.
- `ItemTemplateSelector`– Gibt das [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) an, das zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

Diese [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Eigenschaften können an die- [`Grid`](xref:Xamarin.Forms.Grid), [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)-,-,- [`StackLayout`](xref:Xamarin.Forms.StackLayout) und-Klassen angefügt werden, [`Layout<T>`](xref:Xamarin.Forms.Layout`1) die alle von der-Klasse abgeleitet werden.

> [!NOTE]
> Die `ItemTemplate` -Eigenschaft hat Vorrang, wenn `ItemTemplate` die `ItemTemplateSelector` -Eigenschaft und die-Eigenschaft festgelegt sind.

Die `Layout<T>` -Klasse macht [`Children`](xref:Xamarin.Forms.Layout`1.Children) eine Auflistung verfügbar, der die untergeordneten Elemente eines Layouts hinzugefügt werden. Wenn die `BinableLayout.ItemsSource` Eigenschaft auf eine Auflistung von Elementen festgelegt und an eine [`Layout<T>`](xref:Xamarin.Forms.Layout`1)von abgeleitete Klasse angefügt wird, wird jedes Element in der Auflistung `Layout<T>.Children` der Auflistung für die Anzeige durch das Layout hinzugefügt. Die `Layout<T>`von abgeleitete Klasse aktualisiert dann die untergeordneten Sichten, wenn sich die zugrunde liegende Auflistung ändert. Weitere Informationen zum xamarin. Forms-layoutcycle finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Bindbare Layouts sollten nur verwendet werden, wenn die Auflistung von Elementen, die angezeigt werden sollen, klein ist und ein Bildlauf und eine Auswahl nicht erforderlich sind. Wenn Sie einen Bildlauf durchführen können, indem Sie ein bindbare Layout in einem [`ScrollView`](xref:Xamarin.Forms.ScrollView)umgestalten, wird dies nicht empfohlen, da es bei bindbaren Layouts keine UI-Virtualisierung Wenn ein Bildlauf erforderlich ist, sollte eine Bild lauffähige Sicht verwendet [`ListView`](xref:Xamarin.Forms.ListView) werden [`CollectionView`](xref:Xamarin.Forms.CollectionView), die die UI-Virtualisierung wie oder enthält. Wenn Sie diese Empfehlung nicht beachten, kann dies zu Leistungsproblemen führen.

> [!IMPORTANT]
>Obwohl es technisch möglich ist, ein bindbares Layout an eine beliebige Layoutklasse anzufügen, [`Layout<T>`](xref:Xamarin.Forms.Layout`1) die von der-Klasse abgeleitet ist, ist dies nicht immer praktisch, [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)insbesondere [`Grid`](xref:Xamarin.Forms.Grid)für die [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Klassen, und. Angenommen, Sie möchten eine Auflistung von Daten in einem [`Grid`](xref:Xamarin.Forms.Grid) mithilfe eines bindbaren Layouts anzeigen, wobei jedes Element in der Auflistung ein Objekt mit mehreren Eigenschaften ist. Jede Zeile in der `Grid` sollte ein Objekt aus der Auflistung anzeigen, wobei jede Spalte in der `Grid` eine der Eigenschaften des Objekts anzeigt. Da der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für das bindbare Layout nur ein einzelnes Objekt enthalten kann, ist es erforderlich, dass dieses Objekt eine Layoutklasse mit mehreren Ansichten ist, die jeweils eine der Eigenschaften des Objekts in einer bestimmten `Grid` Spalte anzeigen. Obwohl dieses Szenario mit bindbaren Layouts realisiert werden kann, führt es zu einem `Grid` `Grid` übergeordneten Element, das ein untergeordnetes Element für jedes Element in der gebundenen Auflistung enthält. Dies ist eine hochgradig `Grid` ineffiziente und problematische Verwendung des Layouts.

## <a name="populating-a-bindable-layout-with-data"></a>Auffüllen eines bindbaren Layouts mit Daten

Ein bindbares Layout wird mit Daten aufgefüllt, indem die `ItemsSource` zugehörige-Eigenschaft auf eine `IEnumerable`beliebige Auflistung festgelegt wird, [`Layout<T>`](xref:Xamarin.Forms.Layout`1)die implementiert, und an eine von abgeleitete Klasse angehängt wird:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Der entsprechende C#-Code ist:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Wenn die `BindableLayout.ItemsSource` angefügte-Eigenschaft für ein Layout festgelegt ist `BindableLayout.ItemTemplate` , die angefügte-Eigenschaft jedoch nicht fest `IEnumerable` gelegt ist, wird jedes Element [`Label`](xref:Xamarin.Forms.Label) in der Auflistung von einem `BindableLayout` angezeigt, das von der-Klasse erstellt wird.

## <a name="defining-item-appearance"></a>Definieren der Element Darstellung

Die Darstellung der einzelnen Elemente im bindbaren Layout kann definiert werden `BindableLayout.ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), indem die angefügte-Eigenschaft auf festgelegt wird:

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

Der entsprechende C#-Code ist:

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

In diesem Beispiel wird jedes Element in der `TopFollowers` Auflistung von einer `CircleImage` Ansicht angezeigt, die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)in definiert ist:

![Bindable Layout mit einem DataTemplate](bindable-layouts-images/top-followers.png "Bindable Layout mit einer Daten Vorlage")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im bindbaren Layout kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `BindableLayout.ItemTemplateSelector` angefügte-Eigenschaft [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)auf festgelegt wird:

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

Der entsprechende C#-Code ist:

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

Die `TechItemTemplateSelector` -Klasse `DefaultTemplate` definiert `XamarinFormsTemplate` die Eigenschaften und [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` -Methode gibt `XamarinFormsTemplate`den zurück, der ein Element in einem dunkelroten mit einem Herz daneben anzeigt, wenn das Element gleich "xamarin. Forms" ist. Wenn das Element nicht gleich "xamarin. Forms" ist, gibt `OnSelectTemplate` die Methode den `DefaultTemplate`zurück, der ein Element mit der Standardfarbe eines [`Label`](xref:Xamarin.Forms.Label)anzeigt:

![Bindable Layout mit DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Bindable Layout mit Datenvorlagen Auswahl")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [Demo zu bindbaren Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
