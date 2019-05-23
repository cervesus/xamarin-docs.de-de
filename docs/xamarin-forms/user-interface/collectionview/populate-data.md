---
title: Xamarin.Forms CollectionView Daten
description: Eine CollectionView wird mit Daten aufgefüllt durch Festlegen der ItemsSource-Eigenschaft auf eine beliebige Sammlung, die IEnumerable implementiert.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: d2729250c0f991564ae70ddf6a15b40425ed6c46
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005268"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.Forms CollectionView Daten

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert die folgenden Eigenschaften, die definieren, die Daten angezeigt werden, und sein aussehen:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), des Typs `IEnumerable`, gibt die Auflistung von Elementen, die angezeigt werden, und hat den Standardwert `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate), des Typs [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), gibt an, die Vorlage für jedes Element in der Auflistung der Elemente, die angezeigt werden.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können.

## <a name="populate-a-collectionview-with-data"></a>Eine CollectionView mit Daten auffüllen

Ein [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) mit Daten aufgefüllt wird, durch Festlegen seiner [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`. Elemente können in XAML hinzugefügt werden, durch die Initialisierung der `ItemsSource` Eigenschaft aus einem Array von Zeichenfolgen:

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

Der entsprechende C#-Code ist:

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

> [!IMPORTANT]
> Wenn die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) ist erforderlich, um aktualisiert, wenn Elemente hinzugefügt, entfernt oder in der zugrunde liegenden Auflistung geändert werden, ist die zugrunde liegende Auflistung muss ein `IEnumerable` -Auflistung, die Eigenschaft sendet änderungsbenachrichtigungen, z. B. `ObservableCollection`.

In der Standardeinstellung [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) Zeigt Elemente in einer vertikalen Liste an, wie in den folgenden Screenshots gezeigt:

[![Screenshot der CollectionView, Textelemente, in iOS- und Android enthält](populate-data-images/text.png "Textelemente in einem CollectionView")](populate-data-images/text-large.png#lightbox "Textelemente in einem CollectionView")

Informationen zum Ändern der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) Layout finden Sie unter [Geben Sie ein Layout](layout.md). Informationen dazu, wie die Darstellung der einzelnen Elemente im Definieren der `CollectionView`, finden Sie unter [Element Darstellung definieren](#define-item-appearance).

### <a name="data-binding"></a>Datenbindung

[`CollectionView`](xref:Xamarin.Forms.CollectionView) mit Daten gefüllt werden kann, um mithilfe der Datenbindung binden die [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft, um eine `IEnumerable` Auflistung. In XAML, erfolgt dies über die `Binding` Markuperweiterung:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

In diesem Beispiel die [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaftendaten bindet an die `Monkeys` -Eigenschaft des Ansichtsmodells verbunden.

> [!NOTE]
> Kompilierte Bindungen können aktiviert werden, um die Bindung datenleistung in Xamarin.Forms-Anwendungen zu verbessern. Weitere Informationen finden Sie unter [kompiliert Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Definieren der Darstellung des Elements

Die Darstellung jedes Elements in der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) definiert werden, indem Sie die Einstellung der [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

Der entsprechende C#-Code ist:

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

Der im angegebenen Elemente den [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definieren Sie die Darstellung der einzelnen Elemente in der Liste. Im Beispiel des Layouts innerhalb der `DataTemplate` verwaltet wird, indem eine [ `Grid` ](xref:Xamarin.Forms.Grid). Die `Grid` enthält ein [ `Image` ](xref:Xamarin.Forms.Image) -Objekt, und zwei [ `Label` ](xref:Xamarin.Forms.Label) -Objekten, alle Eigenschaften der Bindung der `Monkey` Klasse:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die folgenden Screenshots zeigen dem Ergebnis der Vorlagen jedes Element in der Liste:

[![Screenshot der CollectionView, wobei jedes Element auf Vorlagen basierenden unter iOS und Android ist](populate-data-images/datatemplate.png "auf Vorlagen basierenden Elemente in einem CollectionView")](populate-data-images/datatemplate-large.png#lightbox "auf Vorlagen basierenden Elemente in einem CollectionView")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Wählen Sie die Element-Darstellung zur Laufzeit

Die Darstellung jedes Elements in der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) kann ausgewählt werden, zur Laufzeit basierend auf dem Elementwert, durch Festlegen der [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) Eigenschaft, um eine [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)Objekt:

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

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Die [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaftensatz auf eine `MonkeyDataTemplateSelector` Objekt. Das folgende Beispiel zeigt die `MonkeyDataTemplateSelector` Klasse:

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

Die `MonkeyDataTemplateSelector` -Klasse definiert `AmericanMonkey` und `OtherMonkey` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf verschiedene Datenvorlagen festgelegt sind. Die `OnSelectTemplate` überschreiben, gibt die `AmericanMonkey` Vorlage, die das Monkey-Namen und den Speicherort in Blaugrün, anzeigt, wenn der Name des Monkey "Amerika" enthält. Wenn der Name des Monkey "Amerika", enthalten nicht die `OnSelectTemplate` überschreiben, gibt die `OtherMonkey` Vorlage, die das Monkey-Namen und den Speicherort im Silver anzeigt:

[![Screenshot der CollectionView Runtime Element Vorlagenauswahl, unter iOS und Android](populate-data-images/datatemplateselector.png "Runtime Vorlage Elementauswahl in einem CollectionView")](populate-data-images/datatemplateselector-large.png#lightbox "Runtime Elementauswahl Vorlage in eine CollectionView")

Weitere Informationen zu Daten Vorlage Selektoren, finden Sie unter [Erstellen einer Xamarin.Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen einer Xamarin.Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
