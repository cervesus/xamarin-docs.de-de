---
title: Xamarin. Forms CollectionView-Layout
description: Standardmäßig zeigt eine CollectionView ihre Elemente in einer vertikalen Liste an. Es können jedoch vertikale und horizontale Listen und Raster angegeben werden.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2019
ms.openlocfilehash: 0b64583f2bd17ddecac66778066406f81c7e7800
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517612"
---
# <a name="xamarinforms-collectionview-layout"></a>Xamarin. Forms CollectionView-Layout

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften, die das Layout Steuern:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)Gibt das zu [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout)verwendende Layout des Typs an.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)Gibt die zu [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)verwendende elementmeasure-Strategie vom Typ an.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Standardmäßig [`CollectionView`](xref:Xamarin.Forms.CollectionView) werden die Elemente in einer vertikalen Liste angezeigt. Allerdings können die folgenden Layouts verwendet werden:

- Vertikale Liste – eine einzelne Spaltenliste, die beim Hinzufügen neuer Elemente vertikal wächst.
- Horizontale Liste – eine einzelne Zeilen Liste, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.
- Vertikales Raster – ein mehrspaltige Raster, das beim Hinzufügen neuer Elemente vertikal wächst.
- Horizontales Raster – ein Raster mit mehreren Zeilen, das horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.

Diese Layouts können durch Festlegen der [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) -Eigenschaft auf die Klasse angegeben werden, die [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) von der-Klasse abgeleitet wird. Diese Klasse definiert die folgenden Eigenschaften:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)Gibt die Richtung [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)des Typs an, in der die [`CollectionView`](xref:Xamarin.Forms.CollectionView) erweitert werden, wenn Elemente hinzugefügt werden.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)Gibt an [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment), wie Andockpunkte mit Elementen ausgerichtet werden.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Gibt beim Scrollen [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)das Verhalten der Andockpunkte an, vom Typ.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können. Weitere Informationen zu Endpunkt Punkten finden Sie im Bild Lauf Handbuch zu [xamarin. Forms CollectionView](scrolling.md) unter [Snap Points](scrolling.md#snap-points) .

Die [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) -Enumeration definiert die folgenden Member:

- `Vertical`Gibt an, [`CollectionView`](xref:Xamarin.Forms.CollectionView) dass das vertikal erweitert wird, wenn Elemente hinzugefügt werden.
- `Horizontal`Gibt an, [`CollectionView`](xref:Xamarin.Forms.CollectionView) dass der horizontal erweitert wird, wenn Elemente hinzugefügt werden.

Die `LinearItemsLayout` -Klasse erbt von [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) der-Klasse und definiert `ItemSpacing` eine-Eigenschaft vom `double`Typ, die den leeren Leerraum um jedes Element darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer oder gleich 0 sein. Die `LinearItemsLayout` -Klasse definiert auch `Vertical` statische `Horizontal` Member und. Diese Member können zum Erstellen vertikaler oder horizontaler Listen verwendet werden. Alternativ kann ein `LinearItemsLayout` -Objekt erstellt werden, wobei ein [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) Enumerationsmember als Argument angegeben wird.

Die [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) Klasse erbt von der [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) -Klasse und definiert die folgenden Eigenschaften:

- `VerticalItemSpacing`vom Typ `double`, der den vertikalen leeren Leerraum um jedes Element darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer oder gleich 0 sein.
- `HorizontalItemSpacing`vom Typ `double`, der den horizontalen leeren Leerraum um jedes Element darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer oder gleich 0 sein.
- `Span`vom Typ `int`, der die Anzahl der Spalten oder Zeilen darstellt, die im Raster angezeigt werden sollen. Der Standardwert dieser Eigenschaft ist 1, und der Wert muss immer größer oder gleich 1 sein.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwendet die systemeigenen layoutengines, um das Layout auszuführen.

## <a name="vertical-list"></a>Vertikale Liste

In der Standard [`CollectionView`](xref:Xamarin.Forms.CollectionView) Einstellung werden die Elemente in einem vertikalen Listen Layout angezeigt. Daher ist es nicht erforderlich, die-Eigenschaft [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) für die Verwendung dieses Layouts festzulegen:

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

Aus Gründen der Vollständigkeit [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann jedoch festgelegt werden, dass die Elemente in einer vertikalen Liste angezeigt werden, [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) indem die `VerticalList`zugehörige-Eigenschaft auf festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

Dies kann auch erreicht werden, [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) indem die-Eigenschaft auf ein `LinearItemsLayout` -Objekt festgelegt wird `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) . dabei wird der- `Orientation` Enumerationsmember als Eigenschafts Wert angegeben:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

Dies führt zu einer einzelnen Spaltenliste, die beim Hinzufügen neuer Elemente vertikal wächst:

[![Screenshot eines Auflistungs Sicht-vertikal Listen Layouts unter IOS und Android](layout-images/vertical-list.png "Layout für die vertikale Auflistung von CollectionView")](layout-images/vertical-list-large.png#lightbox "Layout für die vertikale Auflistung von CollectionView")

## <a name="horizontal-list"></a>Horizontale Liste

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann seine Elemente in einer horizontalen Liste anzeigen, indem die [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) zugehörige `HorizontalList`-Eigenschaft auf festgelegt wird:

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

Alternativ kann dieses Layout auch erreicht werden, [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) indem die-Eigenschaft auf ein `LinearItemsLayout` -Objekt festgelegt `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) wird. dabei wird der `Orientation` -Enumerationsmember als Eigenschafts Wert angegeben:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

Dies führt zu einer einzelnen Zeilen Liste, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Screenshot eines horizontalen Listen Layouts der CollectionView unter IOS und Android](layout-images/horizontal-list.png "Layout der horizontalen Auflistung von CollectionView")](layout-images/horizontal-list-large.png#lightbox "Layout der horizontalen Auflistung von CollectionView")

## <a name="vertical-grid"></a>Vertikales Raster

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann seine Elemente in einem vertikalen Raster anzeigen, indem seine [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) -Eigenschaft auf [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) ein- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) Objekt festgelegt wird `Vertical`, dessen-Eigenschaft auf festgelegt ist:

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

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

Standardmäßig werden in einer [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) vertikalen Elemente in einer einzelnen Spalte angezeigt. In diesem Beispiel wird die `GridItemsLayout.Span` -Eigenschaft jedoch auf 2 festgelegt. Dies führt zu einem zwei spaltigen Raster, das vertikal wächst, wenn neue Elemente hinzugefügt werden:

[![Screenshot eines vertikalen auflistungsansichts Layouts unter IOS und Android](layout-images/vertical-grid.png "Layout für das vertikale Raster von CollectionView")](layout-images/vertical-grid-large.png#lightbox "Layout für das vertikale Raster von CollectionView")

## <a name="horizontal-grid"></a>Horizontales Raster

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann seine Elemente in einem horizontalen Raster anzeigen, indem seine [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) -Eigenschaft auf [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) ein-[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) Objekt festgelegt wird `Horizontal`, dessen-Eigenschaft auf festgelegt ist:

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

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

Standardmäßig zeigt ein Horizont [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) Ales Element Elemente in einer einzelnen Zeile an. In diesem Beispiel wird die `GridItemsLayout.Span` -Eigenschaft jedoch auf 4 festgelegt. Dies führt zu einem Raster mit vier Zeilen, das horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Screenshot eines horizontalen auflistungssicht-Raster Layouts unter IOS und Android](layout-images/horizontal-grid.png "Horizontales Raster Layout für CollectionView")](layout-images/horizontal-grid-large.png#lightbox "Horizontales Raster Layout für CollectionView")

## <a name="headers-and-footers"></a>Kopf- und Fußzeilen

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann eine Kopfzeile und eine Fußzeile darstellen, die mit den Elementen in der Liste scrollen. Die Kopf-und Fußzeilen können Zeichen folgen, Sichten oder [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekte sein.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften zum Angeben der Kopf-und Fußzeile:

- `Header`Gibt die Zeichen `object`Folge, die Bindung oder die Ansicht an, die am Anfang der Liste angezeigt wird, vom Typ.
- `HeaderTemplate`Gibt den vom [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)Typ an, `DataTemplate` der zum Formatieren von `Header`verwendet werden soll.
- `Footer``object`gibt die Zeichenfolge, die Bindung oder die Ansicht an, die am Ende der Liste angezeigt werden.
- `FooterTemplate`Gibt den vom [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)Typ an, `DataTemplate` der zum Formatieren von `Footer`verwendet werden soll.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Wenn ein Header einem Layout hinzugefügt wird, das von links nach rechts horizontal vergrößert wird, wird der Header links neben der Liste angezeigt. Wenn eine Fußzeile einem Layout hinzugefügt wird, das von links nach rechts horizontal vergrößert wird, wird der Fußzeile rechts neben der Liste angezeigt.

### <a name="display-strings-in-the-header-and-footer"></a>Anzeigen von Zeichen folgen in der Kopf-und Fußzeile

Die `Header` - `Footer` Eigenschaft und die-Eigenschaft `string` können auf-Werte festgelegt werden, wie im folgenden Beispiel gezeigt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    Header = "Monkeys",
    Footer = "2019"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Dieser Code führt zu den folgenden Screenshots, wobei die Kopfzeile im IOS-Bildschirmfoto und die Fußzeile im Android-Bildschirmfoto angezeigt wird:

[![Screenshot einer CollectionView-Zeichen folgen Kopfzeile und-Fußzeile unter IOS und Android](layout-images/header-footer-string.png "Zeichen folgen Kopfzeile und-Fußzeile der CollectionView")](layout-images/header-footer-string-large.png#lightbox "Zeichen folgen Kopfzeile und-Fußzeile der CollectionView")

### <a name="display-views-in-the-header-and-footer"></a>Anzeigen von Ansichten in der Kopf-und Fußzeile

Die `Header` Eigenschaften `Footer` und können jeweils auf eine Ansicht festgelegt werden. Dabei kann es sich um eine einzelne Ansicht oder eine Ansicht handeln, die mehrere untergeordnete Sichten enthält. Das folgende Beispiel zeigt die `Header` Eigenschaften `Footer` und, die jeweils auf [`StackLayout`](xref:Xamarin.Forms.StackLayout) ein-Objekt fest [`Label`](xref:Xamarin.Forms.Label) gelegt sind, das ein-Objekt enthält:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.Header>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Monkeys"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Header>
    <CollectionView.Footer>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Friends of Xamarin Monkey"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Footer>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    Header = new StackLayout
    {
        Children =
        {
            new Label { Text = "Monkeys", ... }
        }
    },
    Footer = new StackLayout
    {
        Children =
        {
            new Label { Text = "Friends of Xamarin Monkey", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Dieser Code führt zu den folgenden Screenshots, wobei die Kopfzeile im IOS-Bildschirmfoto und die Fußzeile im Android-Bildschirmfoto angezeigt wird:

[![Screenshot einer CollectionView-Kopfzeile und-Fußzeile unter Verwendung von Ansichten unter IOS und Android](layout-images/header-footer-view.png "Kopf-und Fußzeile der CollectionView-Ansicht")](layout-images/header-footer-view-large.png#lightbox "Kopf-und Fußzeile der CollectionView-Ansicht")

### <a name="display-a-templated-header-and-footer"></a>Anzeigen einer auf Vorlagen basierenden Kopfzeile und-Fußzeile

Die `HeaderTemplate` - `FooterTemplate` Eigenschaft und die-Eigenschaft [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) können auf Objekte festgelegt werden, die zum Formatieren der Kopf-und Fußzeile verwendet werden. In diesem Szenario müssen die `Header` - `Footer` Eigenschaft und die-Eigenschaft an die aktuelle Quelle gebunden werden, damit die Vorlagen angewendet werden, wie im folgenden Beispiel gezeigt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="{Binding .}"
                Footer="{Binding .}">
    <CollectionView.HeaderTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Monkeys"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.HeaderTemplate>
    <CollectionView.FooterTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Friends of Xamarin Monkey"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.FooterTemplate>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    HeaderTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    }),
    FooterTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    })
};
collectionView.SetBinding(ItemsView.HeaderProperty, ".");
collectionView.SetBinding(ItemsView.FooterProperty, ".");
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Dieser Code führt zu den folgenden Screenshots, wobei die Kopfzeile im IOS-Bildschirmfoto und die Fußzeile im Android-Bildschirmfoto angezeigt wird:

[![Screenshot einer CollectionView-Kopfzeile und-Fußzeile unter Verwendung von Vorlagen unter IOS und Android](layout-images/header-footer-template.png "CollectionView-Vorlagen Kopfzeile und-Fußzeile")](layout-images/header-footer-template-large.png#lightbox "CollectionView-Vorlagen Kopfzeile und-Fußzeile")

## <a name="item-spacing"></a>Element Abstand

Standardmäßig gibt es kein Leerzeichen zwischen jedem Element in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView). Dieses Verhalten kann geändert werden, indem Eigenschaften für das von verwendete Element Layout fest `CollectionView`gelegt werden.

Wenn eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) seine [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) -Eigenschaft auf ein `LinearItemsLayout` -Objekt festlegt `LinearItemsLayout.ItemSpacing` , kann die-Eigenschaft auf `double` einen Wert festgelegt werden, der den Leerraum zwischen Elementen darstellt:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> Die `LinearItemsLayout.ItemSpacing` -Eigenschaft verfügt über einen Satz von Validierungs Rückrufen, mit dem sichergestellt wird, dass der Wert der-Eigenschaft immer größer oder gleich 0 ist.

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Dieser Code ergibt eine vertikale einzelne Spaltenliste mit einem Abstand von 20 zwischen Elementen:

[![Screenshot einer CollectionView mit Element Abstand unter IOS und Android](layout-images/vertical-list-spacing.png "Element Abstand für CollectionView")](layout-images/vertical-list-spacing-large.png#lightbox "Element Abstand für CollectionView")

Wenn eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) seine-Eigenschaft auf ein [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) -Objekt festlegt `GridItemsLayout.VerticalItemSpacing` , `GridItemsLayout.HorizontalItemSpacing` können die-Eigenschaft und `double` die-Eigenschaft auf Werte festgelegt werden, die den leeren Bereich vertikal und horizontal zwischen Elementen darstellen:

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
> Für `GridItemsLayout.VerticalItemSpacing` die `GridItemsLayout.HorizontalItemSpacing` -Eigenschaft und die-Eigenschaft sind Validierungs Rückrufe festgelegt, die sicherstellen, dass die Werte der Eigenschaften immer größer oder gleich 0 sind.

Der entsprechende C#-Code lautet:

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

Dieser Code ergibt ein vertikales zweispaltige Raster mit einem vertikalen Abstand von 20 zwischen Elementen und einem horizontalen Abstand von 30 zwischen Elementen:

[![Screenshot einer CollectionView mit Element Abstand unter Android](layout-images/vertical-grid-spacing.png "Element Abstand für CollectionView")](layout-images/vertical-grid-spacing-large.png#lightbox "Element Abstand für CollectionView")

## <a name="item-sizing"></a>Elementgröße

Standardmäßig wird jedes Element in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) einzeln gemessen und verkleinert, vorausgesetzt, dass die Benutzeroberflächen Elemente [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) in der keine festgelegten Größen angeben. Dieses Verhalten, das geändert werden kann, wird durch den [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) -Eigenschafts Wert angegeben. Dieser Eigenschafts Wert kann auf einen der [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) Enumerationsmember festgelegt werden:

- `MeasureAllItems`– jedes Element wird einzeln gemessen. Dies ist der Standardwert.
- `MeasureFirstItem`– nur das erste Element wird gemessen, wobei alle nachfolgenden Elemente dieselbe Größe wie das erste Element erhalten.

> [!IMPORTANT]
> Die `MeasureFirstItem` Größen Anpassungsstrategie führt zu einer verbesserten Leistung, wenn Sie in Situationen verwendet wird, in denen die Elementgröße für alle Elemente einheitlich sein soll.

Im folgenden Codebeispiel wird das Festlegen [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) der-Eigenschaft veranschaulicht:

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>Dynamische Größe von Elementen

Elemente in einer [`CollectionView`](xref:Xamarin.Forms.CollectionView) können zur Laufzeit dynamisch angepasst werden, indem layoutbezogene Eigenschaften von Elementen in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)geändert werden. Im folgenden Codebeispiel werden z. b. [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) -Eigenschaft und [`Image`](xref:Xamarin.Forms.Image) die-Eigenschaft eines-Objekts geändert:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

Der `OnImageTapped` Ereignishandler wird als Reaktion auf ein [`Image`](xref:Xamarin.Forms.Image) Objekt ausgeführt, das abgetippt wird, und ändert die Abmessungen des Bilds, sodass es leichter angezeigt werden kann:

[![Screenshot einer CollectionView mit dynamischer Elementgröße unter IOS und Android](layout-images/runtime-resizing.png "Größe des dynamischen Elements für CollectionView")](layout-images/runtime-resizing-large.png#lightbox "Größe des dynamischen Elements für CollectionView")

## <a name="right-to-left-layout"></a>Layout von rechts nach links

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann seinen Inhalt in einer Richtung von rechts nach links durch Festlegen der- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft auf [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)festlegen. Allerdings sollte `FlowDirection` die-Eigenschaft idealerweise auf einem Seiten-oder Stamm Layout festgelegt werden, was bewirkt, dass alle Elemente auf der Seite bzw. im Stamm Layout auf die Fluss Richtung Antworten:

```xaml
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

Der Standard [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert für ein Element mit einem über [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)geordneten Element ist. [`CollectionView`](xref:Xamarin.Forms.CollectionView) Daher erbt der den `FlowDirection` -Eigenschafts Wert von [`StackLayout`](xref:Xamarin.Forms.StackLayout), der wiederum den `FlowDirection` -Eigenschafts Wert von erbt [`ContentPage`](xref:Xamarin.Forms.ContentPage). Dies ergibt das Layout von rechts nach links, das in den folgenden Screenshots angezeigt wird:

[![Screenshot eines Auflistungs Ansicht-Layouts von rechts nach Links mit vertikaler Liste unter IOS und Android](layout-images/vertical-list-rtl.png "CollectionView-Layout von rechts nach links vertikal auflisten")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView-Layout von rechts nach links vertikal auflisten")

Weitere Informationen zur Fluss Richtung finden Sie unter von [rechts nach links lokalisierte Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Lokalisierung von rechts nach links](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin. Forms CollectionView-Bildlauf](scrolling.md)
