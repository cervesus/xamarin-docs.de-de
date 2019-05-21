---
title: Bindbare Layouts in Xamarin.Forms
description: Bindbare Layouts ermöglichen die Layout-Klassen, um ihre Inhalte durch Bindung an eine Auflistung von Elementen, mit der Option, die die Darstellung der einzelnen Elemente mit einem DataTemplate festlegen zu generieren.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 28846e6e9590d2adf56114fce8bc6056c0112ac1
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970966"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Bindbare Layouts in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)

Bindbare Layouts können jede abgeleitete Layoutklasse der [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) Klasse, um den Inhalt durch Bindung an eine Auflistung von Elementen, mit der Option, die die Darstellung der einzelnen Elemente in festlegen generieren eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Bindbare Layouts werden bereitgestellt, durch die `BindableLayout` -Klasse, die die folgenden angefügten Eigenschaften aufweist:

- `ItemsSource` – Gibt die Auflistung der `IEnumerable` Elemente, die durch das Layout angezeigt werden.
- `ItemTemplate` – Gibt an, die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zuweisen jedes Element in die Auflistung von Elementen, die das Layout angezeigt.
- `ItemTemplateSelector` – Gibt an, die [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) , die verwendet werden, wählen Sie eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit.

Diese Eigenschaften angefügt werden können, um die [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout), [ `Grid` ](xref:Xamarin.Forms.Grid), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) , und [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , alle abgeleiteten Klassen der [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) Klasse.

> [!NOTE]
> Die `ItemTemplate` -Eigenschaft Vorrang bei der sowohl die `ItemTemplate` und `ItemTemplateSelector` Eigenschaften festgelegt werden.

Die `Layout<T>` -Klasse macht eine [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) Auflistung, die die untergeordneten Elemente eines Layouts hinzugefügt werden. Bei der `BinableLayout.ItemsSource` Eigenschaft ist auf eine Auflistung von Elementen festgelegt und an der eine [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-abgeleiteten Klasse, die jedes Element in der Auflistung hinzugefügt wird die `Layout<T>.Children` Auflistung für die Anzeige nach dem Layout. Die `Layout<T>`-abgeleiteten Klasse wird seine untergeordneten Ansichten aktualisiert, wenn die zugrunde liegende Auflistung ändert. Weitere Informationen zu den Zyklus der Xamarin.Forms-Layouts, finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Bindbare Layouts sollte nur verwendet werden, wenn die Auflistung von Elementen, die angezeigt werden, klein ist und Durchführen eines Bildlaufs und Auswahl ist nicht erforderlich. Beim Durchführen eines Bildlaufs bereitgestellt werden kann, durch das wrapping bindbare Layouts in einer [ `ScrollView` ](xref:Xamarin.Forms.ScrollView), dies wird nicht empfohlen, da bindbare Layouts UI-Virtualisierung fehlt. Wenn das Durchführen eines Bildlaufs erforderlich ist, eine bildlauffähige Ansicht, die Virtualisierung der Benutzeroberfläche, z. B. enthält [ `ListView` ](xref:Xamarin.Forms.ListView) oder [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), verwendet werden soll. Fehler beim beobachten von diese Empfehlung kann zu Leistungsproblemen führen.

> [!IMPORTANT]
>Es ist zwar technisch möglich ist, eine bindbare Layout einer beliebigen Klasse Layout anfügen, die von abgeleitet der [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) -Klasse, es ist nicht immer praktikabel zu diesem Zweck, insbesondere für die [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) , [ `Grid` ](xref:Xamarin.Forms.Grid), und [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) Klassen. Betrachten Sie beispielsweise das Szenario entwickelt, die zum Anzeigen einer Auflistung von Daten in einem [ `Grid` ](xref:Xamarin.Forms.Grid) mit bindbaren Layout, wobei jedes Element in der Auflistung ein Objekt ist, die mehrere Eigenschaften enthält. Jede Zeile in der `Grid` sollte angezeigt werden ein Objekt aus der Auflistung, in der jede Spalte in der `Grid` eine der Eigenschaften des Objekts anzeigen. Da die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) für das bindbare Layout nur ein einzelnes Objekt enthalten kann, ist es erforderlich, für dieses Objekt eine Layoutklasse, die mit mehreren Ansichten an, dass jede eine der Eigenschaften des Objekts in einer bestimmten angezeigtwerden`Grid` Spalte. Obwohl dieses Szenario mit bindbaren Layouts realisiert werden kann, führt dies zu einem übergeordneten Element `Grid` , enthält ein untergeordnetes Element `Grid` für jedes Element in die gebundene Auflistung, die die ist eine äußerst ineffiziente und problematische Verwendung der `Grid` Layout.

## <a name="populating-a-bindable-layout-with-data"></a>Eine bindbare Layout mit Daten auffüllen

Eine bindbare Layout mit Daten aufgefüllt wird, durch Festlegen der `ItemsSource` Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`, und fügen Sie diesen auf eine [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-abgeleitete Klasse:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Der entsprechende C#-Code ist:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Wenn die `BindableLayout.ItemsSource` angefügte Eigenschaft in einem Layout mit Ausrichtung festgelegt ist aber die `BindableLayout.ItemTemplate` angefügte Eigenschaft nicht festgelegt ist, jedes Element der `IEnumerable` Sammlung angezeigt, auf eine [ `Label` ](xref:Xamarin.Forms.Label) werden von der erstellt`BindableLayout` Klasse.

## <a name="defining-item-appearance"></a>Definieren der Darstellung des Elements

Die Darstellung der einzelnen Elemente im bindbare Layout definiert werden, indem Sie die Einstellung der `BindableLayout.ItemTemplate` angefügten Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

In diesem Beispiel wird jedes Element in der `TopFollowers` Sammlung angezeigt, auf eine `CircleImage` Sicht definiert, der [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

![Bindbare Layout mit einem DataTemplate](bindable-layouts-images/top-followers.png "bindbare Layout mit einer Datenvorlage")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Auswählen der Element-Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im bindbare Layout kann ausgewählt werden, zur Laufzeit basierend auf dem Elementwert, durch Festlegen der `BindableLayout.ItemTemplateSelector` angefügten Eigenschaft, um eine [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector):

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

Die [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) verwendete Anwendung wird im Beispiel im folgenden Beispiel gezeigt:

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

Die `TechItemTemplateSelector` -Klasse definiert `DefaultTemplate` und `XamarinFormsTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf verschiedene Datenvorlagen festgelegt sind. Die `OnSelectTemplate` Methode gibt die `XamarinFormsTemplate`, dem ein Element in Dunkelrot mit einem Kern angezeigt wird, angezeigt, wenn das Element "Xamarin.Forms" entspricht. Wenn das Element nicht gleich "Xamarin.Forms", ist die `OnSelectTemplate` Methode gibt die `DefaultTemplate`, wird angezeigt, die ein Element mit die Standardfarbe eine [ `Label` ](xref:Xamarin.Forms.Label):

![Bindbare Layout mit einem DataTemplateSelector](bindable-layouts-images/favorite-tech.png "bindbare Layout mit eine Datenvorlagenauswahl")

Weitere Informationen zu Daten Vorlage Selektoren, finden Sie unter [Erstellen einer Xamarin.Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [Bindbare Layout-Demo (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen einer Xamarin.Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
