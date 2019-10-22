---
title: Xamarin. Forms-Karten Pins
description: In diesem Artikel wird erläutert, wie Pins in einer xamarin. Forms-Karte erstellt werden.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/23/2019
ms.openlocfilehash: 76535f9c31a9dc138e132a3e582b986daf89bdb0
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697668"
---
# <a name="xamarinforms-map-pins"></a>Xamarin. Forms-Karten Pins

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Mit dem xamarin. Forms-`Maps` Steuerelement können Standorte mit `Pin` Objekten gekennzeichnet werden. Ein `Pin` ist ein Karten Marker, der ein Informationsfenster öffnet, wenn darauf geklickt oder getippt wird.

Die `Pin`-Klasse verfügt über die folgenden Eigenschaften:

- `Type` ist ein `PinType` Enumerationswert: generic, Place, savedpin oder SearchResult.
- `Position` ist eine `Position` Instanz, die den breiten-und Längengrad der PIN enthält.
- `Label` ist eine `string`, die in der Regel als PIN-Titel angezeigt wird.
- `Address` ist ein `string`, der im Info-Fenster angezeigt wird. Dabei kann es sich um beliebige `string` Inhalte, nicht nur um eine Adresse handeln.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die `Pin` das Ziel von Daten Bindungen sein kann. Weitere Informationen finden Sie unter [Erstellen von Pins mit Datenbindung](#create-pins-with-data-binding).

## <a name="create-map-pins"></a>Erstellen von Karten Pins

Eine `Pin` Instanz kann im Code erstellt und zu einer Zuordnung hinzugefügt werden:

```csharp
Pin pin1 = new Pin
{
    Type = PinType.Place,
    Position = new Position(47.6368678, -122.137305),
    Label = "Example Pin 1",
    Address = "Example custom details..."
};
map.Pins.Add(pin1);
```

> [!NOTE]
> Der Wert `PinType` wirkt sich darauf aus, wie Pins abhängig von der Plattform gerendert werden. Zum Anpassen des Erscheinungs Bilds einer PIN müssen Sie einen benutzerdefinierten Renderer erstellen. Weitere Informationen finden Sie unter [Anpassen einer Karten-PIN](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

## <a name="create-pins-with-data-binding"></a>Erstellen von Pins mit Datenbindung

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse macht die folgenden Eigenschaften verfügbar:

- `ItemsSource` – gibt die Sammlung der `IEnumerable` Elemente an, die angezeigt werden sollen.
- `ItemTemplate` – gibt die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, die auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden sollen.
- `ItemTemplateSelector` – gibt die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) an, die verwendet werden, um zur Laufzeit eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element auszuwählen.

> [!NOTE]
> Die `ItemTemplate`-Eigenschaft hat Vorrang, wenn die Eigenschaften `ItemTemplate` und `ItemTemplateSelector` festgelegt sind.

Eine [`Map`](xref:Xamarin.Forms.Maps.Map) kann mit Daten aufgefüllt werden, indem die Datenbindung verwendet wird, um die `ItemsSource`-Eigenschaft an eine `IEnumerable` Sammlung zu binden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

Die `ItemsSource`-Eigenschafts Daten werden an die `Locations`-Eigenschaft des verbundenen Ansichts Modells gebunden, das eine `ObservableCollection` von `Location` Objekten zurückgibt, bei der es sich um einen benutzerdefinierten Typ handelt. Jedes `Location` Objekt definiert `Address` und `Description` Eigenschaften vom Typ `string` und eine `Position` Eigenschaft vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position).

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung wird definiert, indem die `ItemTemplate`-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt wird, die ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekt enthält, das von Daten an die entsprechenden Eigenschaften gebunden wird.

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , die eine [`Pin`](xref:Xamarin.Forms.Maps.Pin) Auflistung mithilfe der Datenbindung anzeigt:

[![Screenshot der Karte mit Daten gebundenen Pins unter IOS und Android](map-images/pins-itemssource.png "Zuordnung mit Daten gebundenen Pins")](map-images/pins-itemssource-large.png#lightbox "Zuordnung mit Daten gebundenen Pins")

### <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `ItemTemplateSelector`-Eigenschaft auf einen [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)festgelegt wird:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

Das folgende Beispiel zeigt die `MapItemTemplateSelector`-Klasse:

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

Die `MapItemTemplateSelector`-Klasse definiert `DefaultTemplate` und `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate`-Methode gibt die `XamarinTemplate` zurück, die "xamarin" als Bezeichnung anzeigt, wenn ein `Pin` getippt wird, wenn das Element über eine Adresse verfügt, die "San Francisco" enthält. Wenn das Element nicht über eine Adresse verfügt, die "San Francisco" enthält, gibt die `OnSelectTemplate`-Methode die `DefaultTemplate` zurück.

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="pin-events"></a>PIN-Ereignisse

Die `Pin`-Klasse bietet zwei Ereignisse:

- `MarkerClicked` wird ausgelöst, wenn auf die PIN geklickt oder getippt wird.
- `InfoWindowClicked` wird ausgelöst, wenn auf das Info-Fenster geklickt oder getippt wird.

Ereignishandler können im Code an eine PIN angefügt werden:

```csharp
public class PinEvents: ContentPage
{
    public PinEvents()
    {
        // ...

        pin1.MarkerClicked += OnMarkerClickedAsync;
        pin1.InfoWindowClicked += OnInfoWindowClickedAsync;
    }

    async void OnMarkerClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
    }

    async void OnInfoWindowClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Zuordnen eines benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
