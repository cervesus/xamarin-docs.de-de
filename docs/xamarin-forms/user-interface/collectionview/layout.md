---
title: Xamarin.Forms CollectionView Layout
description: Standardmäßig wird ein CollectionView seine Elemente in einer vertikalen Liste angezeigt. Allerdings können vertikale und horizontale Listen und Raster angegeben werden.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2019
ms.openlocfilehash: 786cea04718022847bba2ecffed8f377dd49bd8b
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512816"
---
# <a name="xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView Layout

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert die folgenden Eigenschaften, die Layout steuern:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout), des Typs [ `IItemsLayout` ](xref:Xamarin.Forms.IItemsLayout), gibt das Layout verwendet werden.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy), des Typs [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy), gibt die Element-Measure-Strategie, die verwendet werden.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können.

Standardmäßig eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) werden die Elemente in einer vertikalen Liste angezeigt. Allerdings können die folgenden Layouts verwendet werden:

- Vertikale Liste – eine Liste der einzelnen Spalten, die vertikal vergrößert werden, wenn neue Elemente hinzugefügt werden.
- Horizontale Liste – eine einzelne Zeile, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.
- Vertikales Raster ein Raster mit mehreren Spalten, die vertikal vergrößert werden, wenn neue Elemente hinzugefügt werden.
- Horizontale Raster – ein mehrzeiliges-Raster, das horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.

Diese Layouts können angegeben werden, indem die [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um die Klasse leitet sich von der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) Klasse. Diese Klasse definiert die folgenden Eigenschaften:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation), des Typs [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation), gibt die Richtung, in dem die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) erweitert wird, wenn Elemente hinzugefügt werden.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), des Typs [ `SnapPointsAlignment` ](xref:Xamarin.Forms.SnapPointsAlignment), gibt an, wie Elemente Ausrichtungspunkte ausgerichtet sind.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType), des Typs [ `SnapPointsType` ](xref:Xamarin.Forms.SnapPointsType), gibt das Verhalten der Ausrichtungspunkte an, beim Durchführen eines Bildlaufs.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können. Weitere Informationen zu Ausrichtungspunkte, finden Sie unter [Snap-Points](scrolling.md#snap-points) in die [Xamarin.Forms CollectionView Bildlauf](scrolling.md) Guide.

Die [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) Enumeration definiert die folgenden Elemente:

- `Vertical` Gibt an, dass die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) wird vertikal erweitert, wenn Elemente hinzugefügt werden.
- `Horizontal` Gibt an, dass die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) wird horizontal erweitert, wenn Elemente hinzugefügt werden.

Die [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) Klasse erbt von der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) Klasse, und definiert eine `ItemSpacing` -Eigenschaft vom Typ `double`, das den Leerraum für die einzelnen Elemente darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer als oder gleich 0. Die `ListItemsLayout` Klasse definiert auch statische `Vertical` und `Horizontal` Member. Diese Member können verwendet werden, auf die vertikale oder horizontale Listen, erstellen. Sie können auch eine `ListItemsLayout` Objekt kann erstellt werden, angeben eine [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) -Enumerationsmember als Argument.

Die [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) Klasse erbt von der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) Klasse, und definiert die folgenden Eigenschaften:

- `VerticalItemSpacing`, des Typs `double`, der den vertikalen Platz für die einzelnen Elemente darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer als oder gleich 0.
- `HorizontalItemSpacing`, des Typs `double`, das den horizontalen Leerraum für die einzelnen Elemente darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer als oder gleich 0.
- `Span`, des Typs `int`, die die Anzahl von Spalten oder Zeilen im Raster angezeigt. Der Standardwert dieser Eigenschaft ist 1, und der Wert muss immer größer als oder gleich 1.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) verwendet die systemeigenen Layout-Engines für das Layout an.

## <a name="vertical-list"></a>Vertikale Liste

In der Standardeinstellung [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) werden die Elemente in einem Layout mit vertikalen Liste angezeigt. Aus diesem Grund ist es nicht erforderlich, legen Sie die [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um dieses Layout verwenden:

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
</CollectionView>
```

Aus Gründen der Vollständigkeit, jedoch eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) kann festgelegt werden, um seine Elemente in einer vertikalen Liste anzuzeigen, durch Festlegen der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um `VerticalList`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

Auch dies kann auch erreicht werden durch Festlegen der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft auf ein Objekt der [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) -Klasse und gibt die `Vertical` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) -Enumerationsmember als Argument:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.Vertical
};
```

Dadurch wird eine Liste mit einzelnen Spalten, die vertikal vergrößert werden, wenn neue Elemente hinzugefügt werden:

[![Screenshot der vertikalen Liste ein CollectionView Layout, unter iOS und Android](layout-images/vertical-list.png "CollectionView vertikale Listenlayout")](layout-images/vertical-list-large.png#lightbox "CollectionView vertikale Listenlayout")

## <a name="horizontal-list"></a>Horizontale Liste

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können die Elemente in einer horizontalen Liste anzeigen, durch Festlegen seiner [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft `HorizontalList`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalList">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Alternativ dazu können auch ist dies durch Festlegen der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um eine [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) Objekt angeben der `Horizontal` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) -Enumerationsmember als Argument:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Horizontal</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.Horizontal
};
```

Dies führt zu einer einzelnen Zeile Liste aus, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Screenshot, der eine horizontale CollectionView Listenlayout unter iOS und Android](layout-images/horizontal-list.png "CollectionView horizontale Listenlayout")](layout-images/horizontal-list-large.png#lightbox "CollectionView horizontale Listenlayout")

## <a name="vertical-grid"></a>Vertikales Raster

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können die Elemente in einem vertikalen Raster anzeigen, indem Festlegen seiner [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um eine [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) Objekt, dessen [ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) -Eigenschaftensatz auf `Vertical`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

Standardmäßig wird ein vertikales [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) werden Elemente in einer einzelnen Spalte angezeigt. Allerdings in diesem Beispiel wird die `GridItemsLayout.Span` Eigenschaft auf 2. Dadurch wird in einem zweispaltigen Raster der vertikal wächst, wenn neue Elemente hinzugefügt werden:

[![Screenshot des einem vertikalen CollectionView Rasterlayout unter iOS und Android](layout-images/vertical-grid.png "CollectionView vertikale Rasterlayout")](layout-images/vertical-grid-large.png#lightbox "CollectionView vertikale Rasterlayout")

## <a name="horizontal-grid"></a>Horizontales Raster

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können die Elemente in einem horizontale Raster anzeigen, durch Festlegen der [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um eine [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) Objekt, dessen[ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) Eigenschaft auf festgelegt ist `Horizontal`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

Standardmäßig werden in einer horizontalen [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) werden Elemente in einer einzelnen Zeile angezeigt. Allerdings in diesem Beispiel wird die `GridItemsLayout.Span` Eigenschaft 4. Dadurch wird in einem Raster vier Zeilen, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Screenshot des einem horizontalen CollectionView Rasterlayout unter iOS und Android](layout-images/horizontal-grid.png "CollectionView horizontale Rasterlayout")](layout-images/horizontal-grid-large.png#lightbox "CollectionView horizontale Rasterlayout")

## <a name="item-spacing"></a>Abstand des Elements

Standardmäßig führt jedes Element einem [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) verfügt nicht über einen leeren Bereich, um es herum. Dieses Verhalten kann geändert werden, durch Festlegen von Eigenschaften zum Layout Elemente ein, die die `CollectionView`.

Wenn eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) legt diese fest seine [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um eine [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) -Objekt, der `ListItemsLayout.ItemSpacing` Eigenschaft kann festgelegt werden, um eine `double` -Wert, der freie Speicherplatz für die einzelnen Elemente darstellt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout ItemSpacing="20">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> Die `ListItemsLayout.ItemSpacing` Eigenschaft verfügt über einen validierungssatz Rückruf, der sicherstellt, dass der Wert der Eigenschaft immer größer als oder gleich 0 ist.

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Dieser Code führt in einer Liste vertikale einzelne Spalte, die einen Abstand von 20 für die einzelnen Elemente aufweist:

[![Screenshot des eine CollectionView mit Abstand des Elements unter iOS und Android](layout-images/vertical-list-spacing.png "CollectionView Element Abstand")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView Element Abstand")

Wenn eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) legt diese fest seine [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) Eigenschaft, um eine [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) -Objekt, der `GridItemsLayout.VerticalItemSpacing` und `GridItemsLayout.HorizontalItemSpacing` Eigenschaften können jeweils einen Legen Sie auf `double` Werte, die den freie Speicherplatz für die einzelnen Elemente vertikal und horizontal darstellen:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2"
                        VerticalItemSpacing="20"
                        HorizontalItemSpacing="30" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> Die `GridItemsLayout.VerticalItemSpacing` und `GridItemsLayout.HorizontalItemSpacing` Eigenschaften haben überprüfungsrückrufe festgelegt, die Stellen Sie sicher, dass die Werte der Eigenschaften immer größer als oder gleich 0,.

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
    {
        VerticalItemSpacing = 20,
        HorizontalItemSpacing = 30
    }
};
```

Dieser Code führt in einem vertikalen zweispaltigen Raster, das einen vertikalen Abstand von 20 für die einzelnen Elemente und einen horizontalen Abstand von 30 für die einzelnen Elemente aufweist:

[![Screenshot des eine CollectionView mit Abstand des Elements unter iOS und Android](layout-images/vertical-grid-spacing.png "CollectionView Element Abstand")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView Element Abstand")

## <a name="item-sizing"></a>Element-größenanpassung

In der Standardeinstellung jedem Element im eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) jeweils gemessen und die Größe, bereitgestellt, die Elemente der Benutzeroberfläche in der [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) feste Größen geben nicht an. Dieses Verhalten, das geändert werden kann, wird angegeben, indem die [ `CollectionView.ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) -Eigenschaftswert. Wert dieser Eigenschaft kann festgelegt werden, um eines der [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy) Enumerationsmember:

- `MeasureAllItems` – jedes Element einzeln gemessen wird. Dies ist der Standardwert.
- `MeasureFirstItem` – nur das erste Element wird gemessen, mit allen nachfolgenden Elementen wird die gleiche Größe wie das erste Element angegeben.

> [!IMPORTANT]
> Die `MeasureFirstItem` Strategie für die größenanpassung führt zu höhere Leistung bei der Verwendung in Situationen, in denen die Größe des Artikels über alle Elemente hinweg einheitlich sein soll.

Das folgende Codebeispiel zeigt die Einstellung der [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) Eigenschaft:

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>Dynamische Ändern der Größe von Elementen

Elemente in einer [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) geändert werden kann dynamisch zur Laufzeit ändern Layout speicherbezogenen Eigenschaften der Elemente innerhalb der [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Der folgende code z. B. Beispiel für Änderungen der [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) und [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) Eigenschaften eine [ `Image` ](xref:Xamarin.Forms.Image) Objekt:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

Die `OnImageTapped` Ereignishandler wird ausgeführt, als Reaktion auf eine [ `Image` ](xref:Xamarin.Forms.Image) Objekt getippt wird, und ändert sich die Abmessungen des Bilds, damit sie leichter angezeigt wird:

[![Screenshot des eine CollectionView mit dynamischen Elements größenanpassung unter iOS und Android](layout-images/runtime-resizing.png "CollectionView dynamische Element größenanpassung")](layout-images/runtime-resizing-large.png#lightbox "CollectionView dynamische Element zur größenanpassung")

## <a name="right-to-left-layout"></a>Rechts-nach-links-layout

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können das Layout des Inhalts in einer flussrichtung von rechts-nach-links durch Festlegen seiner [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft). Allerdings die `FlowDirection` -Eigenschaft sollte im Idealfall eine Seiten- oder Root-Layout, wodurch alle Elemente innerhalb der Seite oder Root-Layout, festgelegt werden, um auf die Richtung des Inhaltsflusses zu reagieren:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

Der Standardwert [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) für ein mit einem übergeordneten Element Element [ `MatchParent` ](xref:Xamarin.Forms.FlowDirection.MatchParent). Aus diesem Grund die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) erbt die `FlowDirection` Eigenschaftswert aus der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wiederum erbt die `FlowDirection` Eigenschaftswert aus der [ `ContentPage`](xref:Xamarin.Forms.ContentPage). Dadurch wird der rechts-nach-links-Layout, die in den folgenden Screenshots gezeigt:

[![Screenshot des einem CollectionView vertikalen Liste der rechts-nach-links-Layout mit Ausrichtung auf IOS- und Android](layout-images/vertical-list-rtl.png "CollectionView vertikalen Liste der rechts-nach-links-Layout")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView rechts-nach-links-vertikal Layout der Liste")

Weitere Informationen zu Richtung des Inhaltsflusses, finden Sie unter [rechts-nach-links-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Rechts-nach-links-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.Forms-CollectionView Durchführen eines Bildlaufs](scrolling.md)
