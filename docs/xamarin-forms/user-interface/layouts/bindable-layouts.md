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
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68647908"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Bindbare Layouts in xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Bindbare Layouts ermöglichen es allen layoutklassen, die von der [`Layout<T>`](xref:Xamarin.Forms.Layout`1) -Klasse abgeleitet sind, den Inhalt durch Binden an eine Auflistung von Elementen zu generieren. die Möglichkeit, die Darstellung der einzelnen Elemente mit einem [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festzulegen. Bindbare Layouts werden von der `BindableLayout`-Klasse bereitgestellt, die die folgenden angefügten Eigenschaften verfügbar macht:

- `ItemsSource` – gibt die Sammlung der `IEnumerable` Elemente an, die vom Layout angezeigt werden sollen.
- `ItemTemplate` – gibt die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, die auf jedes Element in der Auflistung von Elementen angewendet werden soll, die vom Layout angezeigt werden.
- `ItemTemplateSelector` – gibt die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) an, die verwendet werden, um zur Laufzeit eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element auszuwählen.

Diese Eigenschaften können an die Klassen [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`FlexLayout`](xref:Xamarin.Forms.FlexLayout), [`Grid`](xref:Xamarin.Forms.Grid), [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)und [`StackLayout`](xref:Xamarin.Forms.StackLayout) angefügt werden, die alle von der [1](xref:Xamarin.Forms.Layout`1) -Klasse abgeleitet werden.

> [!NOTE]
> Die `ItemTemplate`-Eigenschaft hat Vorrang, wenn die Eigenschaften `ItemTemplate` und `ItemTemplateSelector` festgelegt sind.

Die `Layout<T>`-Klasse macht eine [`Children`](xref:Xamarin.Forms.Layout`1.Children) Auflistung verfügbar, der die untergeordneten Elemente eines Layouts hinzugefügt werden. Wenn die `BinableLayout.ItemsSource`-Eigenschaft auf eine Auflistung von Elementen festgelegt ist und einer [`Layout<T>`](xref:Xamarin.Forms.Layout`1)abgeleiteten Klasse angefügt ist, wird jedes Element in der Auflistung der `Layout<T>.Children` Auflistung hinzugefügt, um es durch das Layout anzuzeigen. Die von `Layout<T>` abgeleitete Klasse aktualisiert dann die untergeordneten Sichten, wenn sich die zugrunde liegende Auflistung ändert. Weitere Informationen zum xamarin. Forms-layoutcycle finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Bindbare Layouts sollten nur verwendet werden, wenn die Auflistung von Elementen, die angezeigt werden sollen, klein ist und ein Bildlauf und eine Auswahl nicht erforderlich sind. Wenn Sie einen Bildlauf durchführen können, indem Sie ein bindbare Layout in einem [`ScrollView`](xref:Xamarin.Forms.ScrollView)umgestalten, wird dies nicht empfohlen, da bei bindbaren Layouts die UI-Virtualisierung fehlt. Wenn ein Bildlauf erforderlich ist, sollte eine Bild lauffähige Ansicht verwendet werden, die die UI-Virtualisierung wie [`ListView`](xref:Xamarin.Forms.ListView) oder [`CollectionView`](xref:Xamarin.Forms.CollectionView)enthält. Wenn Sie diese Empfehlung nicht beachten, kann dies zu Leistungsproblemen führen.

> [!IMPORTANT]
>Obwohl es technisch möglich ist, ein bindbares Layout an eine beliebige Layoutklasse anzufügen, die von der [`Layout<T>`](xref:Xamarin.Forms.Layout`1) -Klasse abgeleitet ist, ist dies nicht immer praktisch, insbesondere für die Klassen [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), [`Grid`](xref:Xamarin.Forms.Grid)und [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . Angenommen, Sie möchten eine Auflistung von Daten in einem [`Grid`](xref:Xamarin.Forms.Grid) mithilfe eines bindbaren Layouts anzeigen, wobei jedes Element in der Auflistung ein Objekt mit mehreren Eigenschaften ist. Jede Zeile im `Grid` sollte ein Objekt aus der Auflistung anzeigen, wobei jede Spalte im `Grid` eine der Eigenschaften des Objekts anzeigt. Da die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für das bindbare Layout nur ein einzelnes Objekt enthalten kann, ist es erforderlich, dass dieses Objekt eine Layoutklasse mit mehreren Ansichten ist, die jeweils eine der Eigenschaften des Objekts in einer bestimmten `Grid` Spalte anzeigen. Obwohl dieses Szenario mit bindbaren Layouts realisiert werden kann, führt es zu einer übergeordneten `Grid`, die für jedes Element in der gebundenen Auflistung eine untergeordnete `Grid` enthält. Dies ist eine hochgradig ineffiziente und problematische Verwendung des `Grid` Layouts.

## <a name="populating-a-bindable-layout-with-data"></a>Auffüllen eines bindbaren Layouts mit Daten

Ein bindbares Layout wird mit Daten aufgefüllt, indem die `ItemsSource`-Eigenschaft auf eine beliebige Auflistung festgelegt wird, die `IEnumerable` implementiert und an eine [`Layout<T>`](xref:Xamarin.Forms.Layout`1)abgeleitete Klasse anfügt:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Der entsprechende C#-Code lautet:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Wenn die `BindableLayout.ItemsSource` angefügte-Eigenschaft für ein Layout festgelegt ist, aber die angefügte Eigenschaft `BindableLayout.ItemTemplate` nicht festgelegt ist, wird jedes Element in der `IEnumerable` Auflistung von einem [`Label`](xref:Xamarin.Forms.Label) angezeigt, das von der `BindableLayout`-Klasse erstellt wird.

## <a name="defining-item-appearance"></a>Definieren der Element Darstellung

Die Darstellung der einzelnen Elemente im Bindungs fähigen Layout kann definiert werden, indem die `BindableLayout.ItemTemplate` angefügte Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird:

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

In diesem Beispiel wird jedes Element in der `TopFollowers` Auflistung von einer `CircleImage` Ansicht angezeigt, die in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)definiert ist:

![Bindable Layout mit einem DataTemplate](bindable-layouts-images/top-followers.png "Bindable Layout mit einer Daten Vorlage")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im bindbaren Layout kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `BindableLayout.ItemTemplateSelector` angefügte Eigenschaft auf eine [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)festgelegt wird:

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

Die in der Beispielanwendung verwendeten [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) werden im folgenden Beispiel gezeigt:

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

Die `TechItemTemplateSelector`-Klasse definiert `DefaultTemplate` und `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate`-Methode gibt das `XamarinFormsTemplate` zurück, das ein Element in einem dunklen rot mit einem Herz anzeigt, wenn das Element gleich "xamarin. Forms" ist. Wenn das Element nicht gleich "xamarin. Forms" ist, gibt die `OnSelectTemplate` Methode den `DefaultTemplate` zurück, der ein Element mit der Standardfarbe eines [`Label`](xref:Xamarin.Forms.Label)anzeigt:

![Bindable Layout mit DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Bindable Layout mit Datenvorlagen Auswahl")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [Demo zu bindbaren Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
