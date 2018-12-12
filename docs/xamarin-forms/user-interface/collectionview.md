---
title: Xamarin.Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 46ab8169b31624e3862cf14f477bcd6cf8c8f3a3
ms.sourcegitcommit: 408b78dd6eded4696469e316af7922a5991f2211
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53246307"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![Vorschau](~/media/shared/preview.png)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` ist eine Ansicht für die Darstellung von Listen mit Daten anderes Layout-Spezifikationen verwenden. Es zielt darauf ab, die eine flexiblere, bereitstellen und leistungsfähige Alternative zu `ListView`. Während der `CollectionView` und `ListView` APIs sind ähnlich, es gibt einige wichtige Unterschiede:

- `CollectionView` verfügt über kein Konzept für die Zellen aus. Datenvorlagen werden stattdessen verwendet, um die Darstellung der einzelnen Elemente der Daten in der Liste zu definieren.
- `CollectionView` reduziert die API-Oberfläche des `ListView`. Viele Eigenschaften und Ereignisse von `ListView` sind nicht im `CollectionView`.
- `CollectionView` verfügt über ein flexibles Layout-Modell, die Daten vertikal oder horizontal in einer Liste oder einem Raster angezeigt werden kann.

Ein `CollectionView` wird durch Festlegen von verbraucht seine `ItemsSource` Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`, und die zugehörige `ItemTemplate` Eigenschaft, um eine `DataTemplate` , die Darstellung jeder Liste-Element definiert. Darüber hinaus die `ItemsLayout` Eigenschaft muss festgelegt werden, um eine `ItemsLayout` -Klasse, um die Spezifikation für das Layout für die Liste zu definieren.

> [!IMPORTANT]
> Die `CollectionView` befindet sich derzeit um eine frühe Vorschau und verfügt nicht über viele der geplanten Funktionen. Darüber hinaus wird die API ändern, wie die Implementierung abgeschlossen ist.

`CollectionView` ist in Xamarin.Forms 4.0-pre1 verfügbar. Allerdings es ist derzeit experimentell und kann nur verwendet werden, durch die folgende Zeile des Codes zum Hinzufügen Ihrer `AppDelegate` Klasse unter iOS, oder um Ihre `MainActivity` Klasse, die unter Android werden vor dem Aufruf `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

## <a name="populating-a-collectionview-with-data"></a>Eine CollectionView mit Daten auffüllen

Ein `CollectionView` mit Daten aufgefüllt wird, durch Festlegen seiner `ItemSource` Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`. Elemente können in XAML hinzugefügt werden, durch die Initialisierung der `ItemsSource` Eigenschaft aus einem Array von Elementen:

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
> Beachten Sie, dass die `x:Array` -Element ist ein `Type` Attribut, der angibt, der des Typs der Elemente im Array.

Darüber hinaus eine `CollectionView` mit Daten gefüllt werden kann, um mithilfe der Datenbindung binden die `ItemsSource` Eigenschaft, um eine `IEnumerable` Auflistung. In XAML erfolgt dies über die `Binding` Markuperweiterung:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

In diesem Beispiel die `ItemsSource` Eigenschaftendaten bindet an die `Monkeys` Eigenschaft des verbundenen Ansichtsmodell, wodurch eine `IList<Monkey>` Auflistung. Das folgende Codebeispiel zeigt die `Monkey` -Klasse, die vier Eigenschaften enthält:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

## <a name="providing-a-view-for-the-empty-state"></a>Bietet eine Darstellung für den leeren Zustand

Wenn die `CollectionView` verfügt nicht über alle Daten angezeigt werden, kann eine entsprechende Meldung an den Benutzer angezeigt werden. Dies geschieht durch Festlegen der `CollectionView.EmptyView` Eigenschaft, um eine `object` , wird angezeigt, wenn die Auflistung von angegebenen die `ItemSource` -Eigenschaft ist leer:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}">
    <CollectionView.EmptyView>
        <Label Text="No items to display" />
    </CollectionView.EmptyView>
</CollectionView>
```

Die `EmptyView` Eigenschaft kann festgelegt werden, um eine `string`, eine Bindung oder eine Sicht, und interaktiven Inhalte enthalten kann, falls erforderlich.

Darüber hinaus die `EmptyViewTemplate` Eigenschaft kann festgelegt werden, um eine `DataTemplate` wird, wird für das Formatieren der `EmptyView`.

## <a name="defining-list-item-appearance"></a>Definieren der Darstellung von Listen-Element

Die Darstellung der Daten für jedes Element in der `CollectionView` definiert werden, indem Sie die Einstellung der `CollectionView.ItemTemplate` Eigenschaft, um eine `DataTemplate`:

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

In diesem Beispiel die `DataTemplate` enthält eine `Grid` mit einer `Image`, und zwei `Label` Instanzen, die, dass alle Eigenschaften der Bindung der `Monkey` Objekt.

Weitere Informationen zu Datenvorlagen finden Sie unter [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="specifying-a-layout"></a>Festlegen eines Layouts

Standardmäßig eine `CollectionView` werden die Elemente in einer vertikalen Liste angezeigt. Allerdings kann eine der folgenden layoutspezifikationen verwendet werden:

- Vertikale Liste – eine Liste der einzelnen Spalten, die vertikal vergrößert werden, wenn neue Elemente hinzugefügt werden.
- Horizontale Liste – eine einzelne Zeile, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.
- Vertikales Raster ein Raster mit mehreren Spalten, die vertikal vergrößert werden, wenn neue Elemente hinzugefügt werden.
- Horizontale Raster – ein mehrzeiliges-Raster, das horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.

Diese Layouts können angegeben werden, indem die `CollectionView.ItemsLayout` Eigenschaft, um eine `ItemsLayout` -Klasse, mit der `ListItemsLayout` -Klasse, die ein Layout, in der Liste bereitstellt und die `GridItemsLayout` -Klasse, die ein Rasterlayout bereitstellt.

Die `ItemsLayout` -Klasse definiert die `Orientation` -Eigenschaft vom Typ `ItemsLayoutOrientation`. Die `ItemsLayoutOrientation` -Enumeration definiert `Vertical` und `Horizontal` Elementwerte.

Die `ListItemsLayout` Klasse erbt von der `ItemsLayout` Klasse, und definiert statische `VerticalList` und `HorizontalList` Member. Diese Member können verwendet werden, auf die vertikale oder horizontale Listen, erstellen. Sie können auch eine `ListItemsLayout` erstellt werden kann, Angeben einer `ItemsLayoutOrientation` -Enumerationsmember als Argument.

Die `GridItemsLayout` Klasse erbt von der `ItemsLayout` Klasse, und definiert eine `Span` -Eigenschaft vom Typ `int`, die die Anzahl von Spalten oder Zeilen im Raster angezeigt. Der Standardwert der `Span` -Eigenschaft ist 1, und sein Wert muss immer größer als oder gleich 1.

### <a name="vertical-list"></a>Vertikale Liste

Standardmäßig eine `CollectionView` werden die Elemente in einem Layout mit vertikalen Liste angezeigt. Aus diesem Grund ist es nicht erforderlich, legen Sie die `ItemsLayout` Eigenschaft, um dieses Layout verwenden:

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

Jedoch aus Gründen der Vollständigkeit eine `CollectionView` kann festgelegt werden, um seine Elemente in einer vertikalen Liste anzuzeigen, durch Festlegen seiner `ItemsLayout` an die statische `ListItemsLayout.VerticalList` Member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

Alternativ dazu können auch ist dies durch Festlegen der `ItemsLayout` Eigenschaft mit einer Instanz von der `ListItemsLayout` -Klasse und gibt die `Vertical` `ItemsLayoutOrientation` -Enumerationsmember als Argument:

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

Dadurch wird eine Liste mit einzelnen Spalten, die vertikal vergrößert werden, wenn neue Elemente hinzugefügt werden:

[![Vertikale Listenlayout CollectionView](collectionview-images/vertical-list.png "CollectionView vertikale Listenlayout")](collectionview-images/vertical-list-large.png#lightbox)

### <a name="horizontal-list"></a>Horizontale Liste

Ein `CollectionView` können die Elemente in einer horizontalen Liste anzeigen, durch Festlegen der `ItemsLayout` Eigenschaft, um die statische `ListItemsLayout.HorizontalList` Member:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.HorizontalList}">
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

Alternativ dazu können auch ist dies durch Festlegen der `ItemsLayout` Eigenschaft mit einer Instanz von der `ListItemsLayout` -Klasse und gibt die `Horizontal` `ItemsLayoutOrientation` -Enumerationsmember als Argument:

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

Dies führt zu einer einzelnen Zeile Liste aus, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Horizontale Listenlayout CollectionView](collectionview-images/horizontal-list.png "CollectionView horizontale Listenlayout")](collectionview-images/horizontal-list-large.png#lightbox)

### <a name="vertical-grid"></a>Vertikales Raster

Ein `CollectionView` können die Elemente in einem vertikalen Raster anzeigen, indem Festlegen seiner `ItemsLayout` Eigenschaft, um eine `GridItemsLayout` Instanz, deren `Orientation` -Eigenschaftensatz auf `Vertical`:

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

Standardmäßig wird ein vertikales `GridItemsLayout` werden Elemente in einer einzelnen Spalte angezeigt. Allerdings in diesem Beispiel wird die `GridItemsLayout.Span` Eigenschaft auf 2. Dadurch wird in einem zweispaltigen Raster der vertikal wächst, wenn neue Elemente hinzugefügt werden:

[![Vertikale Rasterlayout CollectionView](collectionview-images/vertical-grid.png "CollectionView vertikale Rasterlayout")](collectionview-images/vertical-grid-large.png#lightbox)

### <a name="horizontal-grid"></a>Horizontales Raster

Ein `CollectionView` können die Elemente in einem horizontale Raster anzeigen, durch Festlegen seiner `ItemsLayout` Eigenschaft, um eine `GridItemsLayout` Instanz, deren `Orientation` -Eigenschaftensatz auf `Horizontal`:

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

Standardmäßig werden in einer horizontalen `GridItemsLayout` werden Elemente in einer einzelnen Zeile angezeigt. Allerdings in diesem Beispiel wird die `GridItemsLayout.Span` Eigenschaft 4. Dadurch wird in einem Raster vier Zeilen, die horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Horizontale Rasterlayout CollectionView](collectionview-images/horizontal-grid.png "CollectionView horizontale Rasterlayout")](collectionview-images/horizontal-grid-large.png#lightbox)

## <a name="scrolling"></a>Durchführen eines Bildlaufs

`CollectionView` Scrollen Sie angeforderte Elemente können in der Ansicht mit den `ScrollTo` Methode. Diese Methode führt einen Bildlauf durch das Element am angegebenen Index in der Ansicht:

```csharp
_collectionView.ScrollTo(12);
```

Sie können auch kann eine Überladung dieser Methode verwendet werden, das angegebene Element in der Ansicht einen Bildlauf durchführen:

```csharp
var viewModel = BindingContext as MonkeysViewModel;
var monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
_collectionView.ScrollTo(monkey);
```

Beide Überladungen können zusätzliche Argumente angegeben werden, dass anzugeben, dass die Gruppe, die, der das Element im befindet, die genaue Position des Elements, nachdem das Scrollen wurde abgeschlossen (Start, zentriert, end) und ob das Scrollen zu animieren.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
