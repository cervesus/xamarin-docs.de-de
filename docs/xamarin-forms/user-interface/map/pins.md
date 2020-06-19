---
title: Xamarin.FormsKarten Pins
description: In diesem Artikel wird erläutert, wie Pins auf einer Karte erstellt werden Xamarin.Forms .
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e22888291a430863b8e45ee21d359a5acec750f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138436"
---
# <a name="xamarinforms-map-pins"></a>Xamarin.FormsKarten Pins

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Mit dem- Xamarin.Forms [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement können Standorte mit-Objekten gekennzeichnet werden [`Pin`](xref:Xamarin.Forms.Maps.Pin) . Ein `Pin` ist ein Karten Marker, der ein Informationsfenster öffnet, wenn er getippt wird:

[![Screenshot einer Karten-PIN und des zugehörigen Informations Fensters unter IOS und Android](pins-images/pin-and-information-window.png "PIN mit Informationsfenster zuordnen")](pins-images/pin-and-information-window-large.png#lightbox "PIN mit Informationsfenster zuordnen")

Wenn der Auflistung ein- [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekt hinzugefügt wird [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) , wird die PIN in der Karte gerendert.

Die- [`Pin`](xref:Xamarin.Forms.Maps.Pin) Klasse verfügt über die folgenden Eigenschaften:

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)vom Typ `string` , der in der Regel die Adresse für den PIN-Speicherort darstellt. Dabei kann es sich jedoch um beliebige `string` Inhalte, nicht nur um eine Adresse handeln.
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)vom Typ `string` , der in der Regel den PIN-Titel darstellt.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position) , der den breiten-und Längengrad der PIN darstellt.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType) , der den Typ der PIN darstellt.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass ein als `Pin` Ziel von Daten Bindungen dienen kann. Weitere Informationen zu Daten Bindungs `Pin` Objekten finden Sie unter [Anzeigen einer PIN](#display-a-pin-collection)-Auflistung.

Außerdem werden die [`Pin`](xref:Xamarin.Forms.Maps.Pin) `MarkerClicked` -und-Ereignisse in der-Klasse definiert `InfoWindowClicked` . Das `MarkerClicked` -Ereignis wird ausgelöst, wenn eine PIN abgetippt wird, und das- `InfoWindowClicked` Ereignis wird ausgelöst, wenn das Informationsfenster abgetippt wird. Das- `PinClickedEventArgs` Objekt, das beide Ereignisse begleitet, verfügt über eine einzelne `HideInfoWindow` Eigenschaft vom Typ `bool` .

## <a name="display-a-pin"></a>PIN anzeigen

Ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) kann einem [`Map`](xref:Xamarin.Forms.Maps.Map) in XAML hinzugefügt werden:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False">
         <x:Arguments>
             <maps:MapSpan>
                 <x:Arguments>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     <x:Double>0.01</x:Double>
                     <x:Double>0.01</x:Double>
                 </x:Arguments>
             </maps:MapSpan>
         </x:Arguments>
         <maps:Map.Pins>
             <maps:Pin Label="Santa Cruz"
                       Address="The city with a boardwalk"
                       Type="Place">
                 <maps:Pin.Position>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                 </maps:Pin.Position>
             </maps:Pin>
         </maps:Map.Pins>
     </maps:Map>
</ContentPage>
```

Dieser XAML-Code erstellt ein- [`Map`](xref:Xamarin.Forms.Maps.Map) Objekt, das den Bereich anzeigt, der durch das-Objekt angegeben wird [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . Das- `MapSpan` Objekt wird auf den breiten-und Längengrad zentriert, der durch ein-Objekt dargestellt wird [`Position`](xref:Xamarin.Forms.Maps.Position) , das 0,01 Längen-und Längengrad erweitert. Ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Objekt wird der-Auflistung hinzugefügt [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) und auf dem `Map` an der Position gezeichnet, die durch die-Eigenschaft angegeben wird [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) . Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md). Informationen zum Übergeben von Argumenten in XAML an Objekte, die Standardkonstruktoren fehlen, finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

Der entsprechende C#-Code lautet:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
Pin pin = new Pin
{
  Label = "Santa Cruz",
  Address = "The city with a boardwalk",
  Type = PinType.Place,
  Position = new Position(36.9628066, -122.0194722)
};
map.Pins.Add(pin);
```

> [!WARNING]
> Wenn die-Eigenschaft nicht festgelegt [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) wird, wird eine ausgelöst, `ArgumentException` Wenn der [`Pin`](xref:Xamarin.Forms.Maps.Pin) einem hinzugefügt wird [`Map`](xref:Xamarin.Forms.Maps.Map) .

Dieser Beispielcode führt dazu, dass eine einzelne PIN auf einer Karte gerendert wird:

[![Screenshot einer Karten-PIN unter IOS und Android](pins-images/pin-only.png "PIN zuordnen")](pins-images/pin-only-large.png#lightbox "PIN zuordnen")

## <a name="interact-with-a-pin"></a>Interagieren mit einer PIN

Standardmäßig [`Pin`](xref:Xamarin.Forms.Maps.Pin) wird ein angezeigtes Informationsfenster angezeigt:

[![Screenshot einer Karten-PIN und des zugehörigen Informations Fensters unter IOS und Android](pins-images/pin-and-information-window.png "PIN mit Informationsfenster zuordnen")](pins-images/pin-and-information-window-large.png#lightbox "PIN mit Informationsfenster zuordnen")

Wenn Sie an anderer Stelle auf der Karte tippen, wird das Informationsfenster geschlossen

Die- [`Pin`](xref:Xamarin.Forms.Maps.Pin) Klasse definiert ein- `MarkerClicked` Ereignis, das ausgelöst wird, wenn ein `Pin` getippt wird. Es ist nicht erforderlich, dieses Ereignis zu behandeln, um das Informationsfenster anzuzeigen. Stattdessen sollte dieses Ereignis behandelt werden, wenn eine Anforderung vorhanden ist, benachrichtigt zu werden, dass eine bestimmte Pin abgetippt wurde.

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert auch ein `InfoWindowClicked` -Ereignis, das ausgelöst wird, wenn ein Informationsfenster getippt wird. Dieses Ereignis sollte behandelt werden, wenn eine Anforderung vorhanden ist, benachrichtigt zu werden, dass ein bestimmtes Informationsfenster angetippt wurde.

Der folgende Code zeigt ein Beispiel für die Behandlung dieser Ereignisse:

```csharp
using Xamarin.Forms.Maps;
// ...
Pin boardwalkPin = new Pin
{
    Position = new Position(36.9641949, -122.0177232),
    Label = "Boardwalk",
    Address = "Santa Cruz",
    Type = PinType.Place
};
boardwalkPin.MarkerClicked += async (s, args) =>
{
    args.HideInfoWindow = true;
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
};

Pin wharfPin = new Pin
{
    Position = new Position(36.9571571, -122.0173544),
    Label = "Wharf",
    Address = "Santa Cruz",
    Type = PinType.Place
};
wharfPin.InfoWindowClicked += async (s, args) =>
{
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
};
```

Das- `PinClickedEventArgs` Objekt, das beide Ereignisse begleitet, verfügt über eine einzelne `HideInfoWindow` Eigenschaft vom Typ `bool` . Wenn diese Eigenschaft in einem Ereignishandler auf festgelegt ist `true` , wird das Informationsfenster ausgeblendet.

## <a name="pin-types"></a>PIN-Typen

[`Pin`](xref:Xamarin.Forms.Maps.Pin)-Objekte enthalten eine- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) Eigenschaft vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType) , die den Typ der PIN darstellt. Die `PinType`-Enumeration definiert die folgenden Member:

- `Generic`stellt eine generische Pin dar.
- `Place`stellt eine PIN für einen Speicherort dar.
- `SavedPin`stellt eine PIN für einen gespeicherten Speicherort dar.
- `SearchResult`stellt eine PIN für ein Suchergebnis dar.

Wenn Sie jedoch die- [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) Eigenschaft auf einen beliebigen Member festlegen, [`PinType`](xref:Xamarin.Forms.Maps.PinType) wird die Darstellung der gerenderten PIN nicht geändert. Stattdessen müssen Sie einen benutzerdefinierten Renderer erstellen, um die PIN-Darstellung anzupassen. Weitere Informationen finden Sie unter [Anpassen einer Karten-PIN](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md).

## <a name="display-a-pin-collection"></a>Anzeigen einer PIN-Sammlung

Die- [`Map`](xref:Xamarin.Forms.Maps.Map) Klasse definiert die folgenden Eigenschaften:

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)vom Typ `IEnumerable` , der die Auflistung der `IEnumerable` anzuzeigenden Elemente angibt.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der das angibt, das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden soll.
- `ItemTemplateSelector`vom Typ [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , der die angibt, die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

> [!IMPORTANT]
> Die [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) -Eigenschaft hat Vorrang, wenn die `ItemTemplate` -Eigenschaft und die-Eigenschaft `ItemTemplateSelector` festgelegt sind.

Ein [`Map`](xref:Xamarin.Forms.Maps.Map) kann mit Pins aufgefüllt werden, indem die Datenbindung verwendet wird, um die- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) Eigenschaft an eine-Auflistung zu binden `IEnumerable` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
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

Die [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) Eigenschaften Daten werden an die- `Locations` Eigenschaft des verbundenen ViewModel gebunden, das eine `ObservableCollection` von-Objekten zurückgibt, bei der es sich um `Location` einen benutzerdefinierten Typ handelt. Jedes `Location` -Objekt definiert die `Address` -Eigenschaft und die- `Description` Eigenschaft vom Typ `string` und eine- `Position` Eigenschaft vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position) .

Die Darstellung der einzelnen Elemente in der Auflistung `IEnumerable` wird durch Festlegen der- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) Eigenschaft auf eine definiert, die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ein- [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekt enthält, das von Daten an die entsprechenden Eigenschaften gebunden wird.

Die folgenden Screenshots zeigen, wie eine Auflistung [`Map`](xref:Xamarin.Forms.Maps.Map) [`Pin`](xref:Xamarin.Forms.Maps.Pin) mit Datenbindung angezeigt wird:

[![Screenshot der Karte mit Daten gebundenen Pins unter IOS und Android](pins-images/pins-itemsource.png "Zuordnung mit Daten gebundenen Pins")](pins-images/pins-itemsource-large.png#lightbox "Zuordnung mit Daten gebundenen Pins")

### <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente in der Auflistung `IEnumerable` kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die-Eigenschaft auf festgelegt wird `ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) :

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
                    <!-- Change the property values, or the properties that are bound to. -->
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

Das folgende Beispiel zeigt die- `MapItemTemplateSelector` Klasse:

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

Die `MapItemTemplateSelector` -Klasse definiert die `DefaultTemplate` `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften und, die auf unterschiedliche Datenvorlagen festgelegt sind. Die- `OnSelectTemplate` Methode gibt den zurück `XamarinTemplate` , der "xamarin" als Bezeichnung anzeigt, wenn ein `Pin` getippt wird, wenn das Element über eine Adresse verfügt, die "San Francisco" enthält. Wenn das Element nicht über eine Adresse verfügt, die "San Francisco" enthält, `OnSelectTemplate` gibt die Methode zurück `DefaultTemplate` .

> [!NOTE]
> Ein Anwendungsfall für diese Funktion ist das Binden von Eigenschaften von untergeordneten [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekten an verschiedene Eigenschaften, basierend auf dem `Pin` Untertyp.

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Zuordnen eines benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Erstellen eines Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
