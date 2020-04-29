---
title: Xamarin. Forms-Karten Pins
description: In diesem Artikel wird erläutert, wie Pins in einer xamarin. Forms-Karte erstellt werden.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: 3df78a7c8eaf12306ade182f134f8d294d203af5
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517584"
---
# <a name="xamarinforms-map-pins"></a>Xamarin. Forms-Karten Pins

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Mit dem xamarin. [`Map`](xref:Xamarin.Forms.Maps.Map) Forms-Steuerelement können Standorte mit [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Objekten gekennzeichnet werden. Ein `Pin` ist ein Karten Marker, der ein Informationsfenster öffnet, wenn er getippt wird:

[![Screenshot einer Karten-PIN und des zugehörigen Informations Fensters unter IOS und Android](pins-images/pin-and-information-window.png "PIN mit Informationsfenster zuordnen")](pins-images/pin-and-information-window-large.png#lightbox "PIN mit Informationsfenster zuordnen")

Wenn der [`Pin`](xref:Xamarin.Forms.Maps.Pin) [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) Auflistung ein-Objekt hinzugefügt wird, wird die PIN in der Karte gerendert.

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse verfügt über die folgenden Eigenschaften:

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)vom Typ `string`, der in der Regel die Adresse für den PIN-Speicherort darstellt. Dabei kann es sich jedoch um `string` beliebige Inhalte, nicht nur um eine Adresse handeln.
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)vom Typ `string`, der in der Regel den PIN-Titel darstellt.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position), der den breiten-und Längengrad der PIN darstellt.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType), der den Typ der PIN darstellt.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. dies `Pin` bedeutet, dass ein als Ziel von Daten Bindungen dienen kann. Weitere Informationen zu Daten Bindungs `Pin` Objekten finden Sie unter [Anzeigen einer PIN](#display-a-pin-collection)-Auflistung.

Außerdem werden die- [`Pin`](xref:Xamarin.Forms.Maps.Pin) und- `MarkerClicked` Ereignisse `InfoWindowClicked` in der-Klasse definiert. Das `MarkerClicked` -Ereignis wird ausgelöst, wenn eine PIN abgetippt wird `InfoWindowClicked` , und das-Ereignis wird ausgelöst, wenn das Informationsfenster abgetippt wird. Das `PinClickedEventArgs` -Objekt, das beide Ereignisse begleitet, `HideInfoWindow` verfügt über eine einzelne `bool`Eigenschaft vom Typ.

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

Dieser XAML-Code [`Map`](xref:Xamarin.Forms.Maps.Map) erstellt ein-Objekt, das den Bereich anzeigt, [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) der durch das-Objekt angegeben wird. Das `MapSpan` -Objekt wird auf den breiten-und Längengrad zentriert, [`Position`](xref:Xamarin.Forms.Maps.Position) der durch ein-Objekt dargestellt wird, das 0,01 Längen-und Längengrad erweitert. Ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Objekt wird der [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) -Auflistung hinzugefügt und auf dem `Map` an der Position gezeichnet, die [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) durch die-Eigenschaft angegeben wird. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md). Informationen zum Übergeben von Argumenten in XAML an Objekte, die Standardkonstruktoren fehlen, finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

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
> Wenn die [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) -Eigenschaft nicht festgelegt wird, `ArgumentException` wird eine ausgelöst, [`Pin`](xref:Xamarin.Forms.Maps.Pin) wenn der einem [`Map`](xref:Xamarin.Forms.Maps.Map)hinzugefügt wird.

Dieser Beispielcode führt dazu, dass eine einzelne PIN auf einer Karte gerendert wird:

[![Screenshot einer Karten-PIN unter IOS und Android](pins-images/pin-only.png "PIN zuordnen")](pins-images/pin-only-large.png#lightbox "PIN zuordnen")

## <a name="interact-with-a-pin"></a>Interagieren mit einer PIN

Standardmäßig wird ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) angezeigtes Informationsfenster angezeigt:

[![Screenshot einer Karten-PIN und des zugehörigen Informations Fensters unter IOS und Android](pins-images/pin-and-information-window.png "PIN mit Informationsfenster zuordnen")](pins-images/pin-and-information-window-large.png#lightbox "PIN mit Informationsfenster zuordnen")

Wenn Sie an anderer Stelle auf der Karte tippen, wird das Informationsfenster geschlossen

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert `MarkerClicked` ein-Ereignis, das ausgelöst wird `Pin` , wenn ein getippt wird. Es ist nicht erforderlich, dieses Ereignis zu behandeln, um das Informationsfenster anzuzeigen. Stattdessen sollte dieses Ereignis behandelt werden, wenn eine Anforderung vorhanden ist, benachrichtigt zu werden, dass eine bestimmte Pin abgetippt wurde.

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert auch `InfoWindowClicked` ein-Ereignis, das ausgelöst wird, wenn ein Informationsfenster getippt wird. Dieses Ereignis sollte behandelt werden, wenn eine Anforderung vorhanden ist, benachrichtigt zu werden, dass ein bestimmtes Informationsfenster angetippt wurde.

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

Das `PinClickedEventArgs` -Objekt, das beide Ereignisse begleitet, `HideInfoWindow` verfügt über eine einzelne `bool`Eigenschaft vom Typ. Wenn diese Eigenschaft in einem Ereignis `true` Handler auf festgelegt ist, wird das Informationsfenster ausgeblendet.

## <a name="pin-types"></a>PIN-Typen

[`Pin`](xref:Xamarin.Forms.Maps.Pin)-Objekte enthalten [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) eine-Eigenschaft vom [`PinType`](xref:Xamarin.Forms.Maps.PinType)Typ, die den Typ der PIN darstellt. Die `PinType`-Enumeration definiert die folgenden Member:

- `Generic`stellt eine generische Pin dar.
- `Place`stellt eine PIN für einen Speicherort dar.
- `SavedPin`stellt eine PIN für einen gespeicherten Speicherort dar.
- `SearchResult`stellt eine PIN für ein Suchergebnis dar.

Wenn Sie jedoch die [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) -Eigenschaft auf [`PinType`](xref:Xamarin.Forms.Maps.PinType) einen beliebigen Member festlegen, wird die Darstellung der gerenderten PIN nicht geändert. Stattdessen müssen Sie einen benutzerdefinierten Renderer erstellen, um die PIN-Darstellung anzupassen. Weitere Informationen finden Sie unter [Anpassen einer Karten-PIN](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md).

## <a name="display-a-pin-collection"></a>Anzeigen einer PIN-Sammlung

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert die folgenden Eigenschaften:

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)vom Typ, `IEnumerable`der die Auflistung der `IEnumerable` anzuzeigenden Elemente angibt.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), der das angibt, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) das auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden soll.
- `ItemTemplateSelector`vom Typ [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector), der die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) angibt, die zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

> [!IMPORTANT]
> Die [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) -Eigenschaft hat Vorrang, wenn `ItemTemplate` die `ItemTemplateSelector` -Eigenschaft und die-Eigenschaft festgelegt sind.

Ein [`Map`](xref:Xamarin.Forms.Maps.Map) kann mit Pins aufgefüllt werden, indem die Datenbindung verwendet wird [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) , um die `IEnumerable` -Eigenschaft an eine-Auflistung zu binden:

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

Die [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) Eigenschaften Daten werden an die `Locations` -Eigenschaft des verbundenen ViewModel gebunden, das eine `ObservableCollection` von `Location` -Objekten zurückgibt, bei der es sich um einen benutzerdefinierten Typ handelt. Jedes `Location` -Objekt `Address` definiert `Description` die-Eigenschaft und `string`die-Eigenschaft `Position` vom Typ und eine [`Position`](xref:Xamarin.Forms.Maps.Position)-Eigenschaft vom Typ.

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung wird durch Festlegen der [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) -Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definiert, die [`Pin`](xref:Xamarin.Forms.Maps.Pin) ein-Objekt enthält, das von Daten an die entsprechenden Eigenschaften gebunden wird.

Die folgenden Screenshots zeigen, [`Map`](xref:Xamarin.Forms.Maps.Map) wie eine [`Pin`](xref:Xamarin.Forms.Maps.Pin) Auflistung mit Datenbindung angezeigt wird:

[![Screenshot der Karte mit Daten gebundenen Pins unter IOS und Android](pins-images/pins-itemsource.png "Zuordnung mit Daten gebundenen Pins")](pins-images/pins-itemsource-large.png#lightbox "Zuordnung mit Daten gebundenen Pins")

### <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `ItemTemplateSelector` -Eigenschaft auf festgelegt [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)wird:

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

Das folgende Beispiel zeigt die `MapItemTemplateSelector` -Klasse:

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

Die `MapItemTemplateSelector` -Klasse `DefaultTemplate` definiert `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) die Eigenschaften und, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` -Methode gibt `XamarinTemplate`den zurück, der "xamarin" als Bezeichnung anzeigt, `Pin` wenn ein getippt wird, wenn das Element über eine Adresse verfügt, die "San Francisco" enthält. Wenn das Element nicht über eine Adresse verfügt, die "San Francisco" enthält `OnSelectTemplate` , gibt die `DefaultTemplate`Methode zurück.

> [!NOTE]
> Ein Anwendungsfall für diese Funktion ist das Binden von Eigenschaften von unter [`Pin`](xref:Xamarin.Forms.Maps.Pin) geordneten Objekten an verschiedene Eigenschaften, basierend auf `Pin` dem Untertyp.

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Zuordnen eines benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Erstellen einer Xamarin.Forms-DataTemplateSelector-Klasse](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
