---
title: Xamarin. Forms-Karten Pins
description: In diesem Artikel wird erläutert, wie Pins in einer xamarin. Forms-Karte erstellt werden.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: 197c48a7a3486d7161d351a6b06101daaa389256
ms.sourcegitcommit: 2cc0796902123df137611b855a55b754ca3c6d73
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74556162"
---
# <a name="xamarinforms-map-pins"></a>Xamarin. Forms-Karten Pins

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Mit dem xamarin. Forms- [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement können Standorte mit [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekten gekennzeichnet werden. Ein `Pin` ist ein Zuordnungs Marker, der ein Informationsfenster öffnet, wenn er getippt wird:

[![Screenshot einer Karten-PIN und des zugehörigen Informations Fensters unter IOS und Android](pins-images/pin-and-information-window.png "PIN mit Informationsfenster zuordnen")](pins-images/pin-and-information-window-large.png#lightbox "PIN mit Informationsfenster zuordnen")

Wenn ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekt der [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) Auflistung hinzugefügt wird, wird die PIN auf der Karte gerendert.

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse verfügt über die folgenden Eigenschaften:

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)vom Typ `string`, der in der Regel die Adresse für den PIN-Speicherort darstellt. Dabei kann es sich jedoch um beliebige `string` Inhalte, nicht nur um eine Adresse handeln.
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)vom Typ `string`, der in der Regel den PIN-Titel darstellt.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position), der den breiten-und Längengrad der PIN darstellt.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType), der den Typ der PIN darstellt.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass eine `Pin` das Ziel von Daten Bindungen sein kann. Weitere Informationen zur Datenbindung `Pin` Objekten finden Sie unter [Anzeigen einer PIN](#display-a-pin-collection)-Auflistung.

Außerdem definiert die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse `MarkerClicked` und `InfoWindowClicked`-Ereignisse. Das `MarkerClicked` Ereignis wird ausgelöst, wenn eine PIN getippt wird, und das `InfoWindowClicked` Ereignis wird ausgelöst, wenn das Informationsfenster getippt wird. Das `PinClickedEventArgs` Objekt, das beide Ereignisse begleitet, verfügt über eine einzelne `HideInfoWindow` Eigenschaft vom Typ `bool`.

## <a name="display-a-pin"></a>PIN anzeigen

Eine [`Pin`](xref:Xamarin.Forms.Maps.Pin) kann einer [`Map`](xref:Xamarin.Forms.Maps.Map) in XAML hinzugefügt werden:

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

Dieser XAML-Code erstellt ein [`Map`](xref:Xamarin.Forms.Maps.Map) Objekt, das den Bereich anzeigt, der durch das [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Objekt angegeben wird. Das `MapSpan` Objekt wird auf den breiten-und Längengrad zentriert, der durch ein [`Position`](xref:Xamarin.Forms.Maps.Position) -Objekt dargestellt wird, das 0,01 Längen-und Längengrad erweitert. Ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Objekt wird der [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) Auflistung hinzugefügt und auf dem `Map` an dem Speicherort gezeichnet, der durch seine [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) -Eigenschaft angegeben wird. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md). Informationen zum Übergeben von Argumenten in XAML an Objekte, die Standardkonstruktoren fehlen, finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

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
> Wenn Sie die [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) -Eigenschaft nicht festlegen, führt dies dazu, dass eine `ArgumentException` ausgelöst wird, wenn der [`Pin`](xref:Xamarin.Forms.Maps.Pin) einer [`Map`](xref:Xamarin.Forms.Maps.Map)hinzugefügt wird.

Dieser Beispielcode führt dazu, dass eine einzelne PIN auf einer Karte gerendert wird:

[![Screenshot einer Karten-PIN unter IOS und Android](pins-images/pin-only.png "PIN zuordnen")](pins-images/pin-only-large.png#lightbox "PIN zuordnen")

## <a name="interact-with-a-pin"></a>Interagieren mit einer PIN

Wenn ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) abgetippt wird, wird standardmäßig das Fenster Informationen angezeigt:

[![Screenshot einer Karten-PIN und des zugehörigen Informations Fensters unter IOS und Android](pins-images/pin-and-information-window.png "PIN mit Informationsfenster zuordnen")](pins-images/pin-and-information-window-large.png#lightbox "PIN mit Informationsfenster zuordnen")

Wenn Sie an anderer Stelle auf der Karte tippen, wird das Informationsfenster geschlossen

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert ein `MarkerClicked` Ereignis, das ausgelöst wird, wenn ein `Pin` getippt wird. Es ist nicht erforderlich, dieses Ereignis zu behandeln, um das Informationsfenster anzuzeigen. Stattdessen sollte dieses Ereignis behandelt werden, wenn eine Anforderung vorhanden ist, benachrichtigt zu werden, dass eine bestimmte Pin abgetippt wurde.

Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert auch ein `InfoWindowClicked` Ereignis, das ausgelöst wird, wenn ein Informationsfenster getippt wird. Dieses Ereignis sollte behandelt werden, wenn eine Anforderung vorhanden ist, benachrichtigt zu werden, dass ein bestimmtes Informationsfenster angetippt wurde.

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

Das `PinClickedEventArgs` Objekt, das beide Ereignisse begleitet, verfügt über eine einzelne `HideInfoWindow` Eigenschaft vom Typ `bool`. Wenn diese Eigenschaft auf `true` innerhalb eines Ereignis Handlers festgelegt ist, wird das Informationsfenster ausgeblendet.

## <a name="pin-types"></a>PIN-Typen

[`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekte enthalten eine [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) Eigenschaft vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType), die den Typ der PIN darstellt. Die `PinType`-Enumeration definiert die folgenden Member:

- `Generic`stellt eine generische Pin dar.
- `Place`stellt eine PIN für einen Speicherort dar.
- `SavedPin`stellt eine PIN für einen gespeicherten Speicherort dar.
- `SearchResult`stellt eine PIN für ein Suchergebnis dar.

Wenn die [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) -Eigenschaft jedoch auf [`PinType`](xref:Xamarin.Forms.Maps.PinType) Member festgelegt wird, wird die Darstellung der gerenderten PIN nicht geändert. Stattdessen müssen Sie einen benutzerdefinierten Renderer erstellen, um die PIN-Darstellung anzupassen. Weitere Informationen finden Sie unter [Anpassen einer Karten-PIN](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

## <a name="display-a-pin-collection"></a>Anzeigen einer PIN-Auflistung

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert die folgenden Eigenschaften:

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)vom Typ `IEnumerable`, der die Auflistung der `IEnumerable` Elemente angibt, die angezeigt werden sollen.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), der die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angibt, die auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden sollen.
- `ItemTemplateSelector`vom Typ [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector), der die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) angibt, die zum Auswählen einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

> [!IMPORTANT]
> Die [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) -Eigenschaft hat Vorrang, wenn die Eigenschaften `ItemTemplate` und `ItemTemplateSelector` festgelegt sind.

Eine [`Map`](xref:Xamarin.Forms.Maps.Map) kann mit Pins aufgefüllt werden, indem die Datenbindung verwendet wird, um die [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) Eigenschaft an eine `IEnumerable` Sammlung zu binden:

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

Die [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) -Eigenschafts Daten werden an die `Locations`-Eigenschaft des verbundenen ViewModel gebunden, das eine `ObservableCollection` von `Location` Objekten zurückgibt, bei der es sich um einen benutzerdefinierten Typ handelt. Jedes `Location` Objekt definiert `Address` und `Description` Eigenschaften vom Typ `string`und eine `Position` Eigenschaft vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position).

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung wird definiert, indem die [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) -Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt wird, die ein [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekt enthält, das von Daten an die entsprechenden Eigenschaften gebunden wird.

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , die eine [`Pin`](xref:Xamarin.Forms.Maps.Pin) Auflistung mithilfe der Datenbindung anzeigt:

[![Screenshot der Karte mit Daten gebundenen Pins unter IOS und Android](pins-images/pins-itemsource.png "Zuordnung mit Daten gebundenen Pins")](pins-images/pins-itemsource-large.png#lightbox "Zuordnung mit Daten gebundenen Pins")

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

Die `MapItemTemplateSelector`-Klasse definiert `DefaultTemplate` und `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate`-Methode gibt die `XamarinTemplate`zurück, die "xamarin" als Bezeichnung anzeigt, wenn ein `Pin` getippt wird, wenn das Element über eine Adresse verfügt, die "San Francisco" enthält. Wenn das Element nicht über eine Adresse verfügt, die "San Francisco" enthält, gibt die `OnSelectTemplate`-Methode die `DefaultTemplate`zurück.

> [!NOTE]
> Ein Anwendungsfall für diese Funktion ist das Binden von Eigenschaften von untergeordneten [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekten an verschiedene Eigenschaften, basierend auf dem `Pin` Untertyp.

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Zuordnen eines benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
