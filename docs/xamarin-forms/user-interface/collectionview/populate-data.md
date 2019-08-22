---
title: Xamarin. Forms CollectionView-Daten
description: Eine CollectionView wird mit Daten aufgefüllt, indem die ItemsSource-Eigenschaft auf eine beliebige Auflistung festgelegt wird, die IEnumerable implementiert.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2019
ms.openlocfilehash: 6942baed6af2a2e9b2c713a8fe08cf4c8ed4416b
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888550"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin. Forms CollectionView-Daten

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften, die die anzuzeigenden Daten und ihre Darstellung definieren:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)Gibt die Auflistung `IEnumerable`der anzuzeigenden Elemente an und hat den Standard `null`Wert.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)Gibt die Vorlage [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)an, die auf jedes Element in der Auflistung der anzuzeigenden Elemente angewendet werden soll.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert eine `ItemsUpdatingScrollMode` Eigenschaft, die das Scrollverhalten von darstellt `CollectionView` , wenn der neue Elemente hinzugefügt werden. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Steuern der Scrollposition, wenn neue Elemente hinzugefügt werden](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann Daten auch inkrementell laden, wenn der Benutzer einen Bildlauf ausführt Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](#load-data-incrementally).

## <a name="populate-a-collectionview-with-data"></a>Auffüllen einer CollectionView mit Daten

Ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung `IEnumerable`festgelegt wird, die implementiert. Elemente können in XAML hinzugefügt werden, indem die `ItemsSource` -Eigenschaft aus einem Zeichen folgen Array initialisiert wird:

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
> Wenn die [`CollectionView`](xref:Xamarin.Forms.CollectionView) beim Hinzufügen, entfernen oder Ändern von Elementen in der zugrunde liegenden Auflistung aktualisiert werden muss, sollte die zugrunde liegende Auflistung eine `IEnumerable` -Auflistung sein, die Benachrichtigungen über Eigenschafts `ObservableCollection`Änderungen sendet, z. b.

Standardmäßig [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt Elemente in einer vertikalen Liste an, wie in den folgenden Screenshots gezeigt:

[ ![Screenshot von CollectionView mit Textelementen, auf IOS-und Android-](populate-data-images/text.png "Textelementen in einer CollectionView") ] (populate-data-images/text-large.png#lightbox "Text Elemente in einer CollectionView")

Weitere Informationen zum Ändern des [`CollectionView`](xref:Xamarin.Forms.CollectionView) Layouts finden Sie unter [Angeben eines Layouts](layout.md). Informationen dazu, wie die Darstellung der einzelnen Elemente in der `CollectionView`definiert wird, finden Sie unter Definieren der [Element](#define-item-appearance)Darstellung.

### <a name="data-binding"></a>Datenbindung

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann mit Daten aufgefüllt werden, indem die-Eigenschaft mithilfe der [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Datenbindung an `IEnumerable` eine-Auflistung gebunden wird. In XAML wird dies mit der `Binding` Markup Erweiterung erreicht:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

In diesem Beispiel werden die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaften Daten an die `Monkeys` -Eigenschaft des verbundenen ViewModel gebunden.

> [!NOTE]
> Kompilierte Bindungen können aktiviert werden, um die Daten bindungsleistung in xamarin. Forms-Anwendungen zu verbessern. Weitere Informationen finden Sie unter [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Element Darstellung definieren

Die Darstellung der einzelnen Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann definiert werden, indem die [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird:

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

Die in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angegebenen Elemente definieren die Darstellung der einzelnen Elemente in der Liste. Im Beispiel wird das Layout in der `DataTemplate` von einem [`Grid`](xref:Xamarin.Forms.Grid)verwaltet. Die `Grid` enthält ein [`Image`](xref:Xamarin.Forms.Image) -Objekt und zwei [`Label`](xref:Xamarin.Forms.Label) -Objekte `Monkey` , die alle an Eigenschaften der-Klasse gebunden sind:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die folgenden Screenshots zeigen das Ergebnis der Vorlagen Darstellung der einzelnen Elemente in der Liste:

[ ![Screenshot von CollectionView, in dem jedes Element auf Vorlagen basiert, auf IOS-und Android-]Vorlagen(populate-data-images/datatemplate.png "Elementen in einer CollectionView") ] (populate-data-images/datatemplate-large.png#lightbox "Auf Vorlagen basierende Elemente in einer CollectionView")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) -Objekt festgelegt wird:

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

Die [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft ist auf ein `MonkeyDataTemplateSelector` -Objekt festgelegt. Das folgende Beispiel zeigt die `MonkeyDataTemplateSelector` -Klasse:

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

Die `MonkeyDataTemplateSelector` -Klasse `AmericanMonkey` definiert `OtherMonkey` die Eigenschaften und [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` außer Kraft Setzung `AmericanMonkey` gibt die Vorlage zurück, die den affennamen und den Speicherort in Teal anzeigt, wenn der affenname "America" enthält. Wenn der affenname nicht "America" enthält, `OnSelectTemplate` gibt die über `OtherMonkey` Schreibung die Vorlage zurück, die den affennamen und den Speicherort in Silber anzeigt:

[ ![Screenshot der Auswahl von "CollectionView-Lauf Zeitelement Vorlage" unter IOS-und Android-Runtime-](populate-data-images/datatemplateselector.png "Element Vorlagen Auswahl in einer CollectionView") ] (populate-data-images/datatemplateselector-large.png#lightbox "Auswahl der Lauf Zeitelement Vorlage in einer CollectionView")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> Wenn Sie [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwenden, legen Sie [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) das Stamm Element der Objekte nie auf `ViewCell`einen fest. Dies führt dazu, dass eine Ausnahme ausgelöst wird `CollectionView` , da kein Konzept von Zellen aufweist.

## <a name="load-data-incrementally"></a>Inkrementelles Laden von Daten

[`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt inkrementelles Laden von Daten, wenn Benutzer durch Elemente scrollen. Dies ermöglicht Szenarios, wie z. b. das asynchrone Laden einer Datenseite aus einem Webdienst, während der Benutzer einen Bildlauf ausführt. Außerdem ist der Punkt, an dem mehr Daten geladen werden, so konfigurierbar, dass Benutzern kein leerer Bereich angezeigt wird oder der Bildlauf angehalten wird.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert die folgenden Eigenschaften zum Steuern des inkrementellen Ladens von Daten:

- `RemainingItemsThreshold`, vom Typ `int`, der Schwellenwert für Elemente, die noch nicht in der Liste sichtbar `RemainingItemsThresholdReached` sind, in der das Ereignis ausgelöst wird.
- `RemainingItemsThresholdReachedCommand`vom Typ `ICommand`, der ausgeführt wird, `RemainingItemsThreshold` wenn erreicht wird.
- `RemainingItemsThresholdReachedCommandParameter` vom Typ `object`: der Parameter, der an `RemainingItemsThresholdReachedCommand` übergeben wird.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert auch ein `RemainingItemsThresholdReached` -Ereignis, das ausgelöst wird `CollectionView` , wenn ein scrollt, `RemainingItemsThreshold` das so weit genug ist, dass Elemente nicht mehr angezeigt werden. Dieses Ereignis kann behandelt werden, um weitere Elemente zu laden. Wenn das `RemainingItemsThresholdReached` Ereignis ausgelöst wird `RemainingItemsThresholdReachedCommand` , wird außerdem ausgeführt, sodass inkrementelles Laden von Daten in einem ViewModel erfolgen kann.

Der Standardwert `RemainingItemsThreshold` der-Eigenschaft ist-1 und gibt an, dass `RemainingItemsThresholdReached` das Ereignis nie ausgelöst wird. Wenn der-Eigenschafts Wert 0 ist `RemainingItemsThresholdReached` , wird das-Ereignis ausgelöst, wenn das letzte [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Element in der angezeigt wird. Bei Werten, die größer als 0 `RemainingItemsThresholdReached` sind, wird das-Ereignis `ItemsSource` ausgelöst, wenn das die Anzahl der Elemente enthält, für die noch kein Rollup durchgeführt wurde.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)überprüft die `RemainingItemsThreshold` -Eigenschaft, sodass Ihr Wert immer größer oder gleich-1 ist.

Das folgende XAML-Beispiel zeigt [`CollectionView`](xref:Xamarin.Forms.CollectionView) einen, der Daten inkrementell lädt:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

In diesem Codebeispiel wird das `RemainingItemsThresholdReached` -Ereignis ausgelöst, wenn für fünf Elemente noch kein Rollup durchgeführt wurde und der Ereignis `OnCollectionViewRemainingItemsThresholdReached` Handler in der Antwort ausgeführt wird:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Daten können auch inkrementell geladen werden, `RemainingItemsThresholdReachedCommand` indem der `ICommand` an eine Implementierung in ViewModel gebunden wird.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
