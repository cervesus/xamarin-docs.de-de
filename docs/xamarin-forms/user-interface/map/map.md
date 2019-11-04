---
title: Xamarin. Forms-Kartensteuerelement
description: Beim Karten Steuerelement handelt es sich um eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung.
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 1cfda90360557af1160d421f18807f8b534967a8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426348"
---
# <a name="xamarinforms-map-control"></a>Xamarin. Forms-Kartensteuerelement

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Das [`Map`](xref:Xamarin.Forms.Maps.Map) -Steuerelement ist eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung:

[![Screenshot der Karten Steuerung unter IOS und Android](map-images/map-default.png "Karten Steuerelement")](map-images/map-default-large.png#lightbox "Karten Steuerelement")

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert die folgenden Eigenschaften, die die Darstellung und das Verhalten von Karten Steuern:

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)vom Typ `bool`gibt an, ob die Karte den aktuellen Speicherort des Benutzers anzeigt.
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)vom Typ `IEnumerable`, der die Auflistung der `IEnumerable` Elemente angibt, die angezeigt werden sollen.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), der die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angibt, die auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden sollen.
- `ItemTemplateSelector`vom Typ [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector), der die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) angibt, die zum Auswählen einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)vom Typ `bool`bestimmt, ob für die Zuordnung ein Bildlauf zulässig ist.
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)vom Typ `bool`bestimmt, ob die Zuordnung vergrößert werden darf.
- `MapElements`vom Typ `IList<MapElement>`stellt die Liste der Elemente in der Karte dar, z. b. Polygone und Polylines.
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)vom Typ [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)gibt den Anzeige Stil der Karte an.
- `MoveToLastRegionOnLayoutChange`vom Typ `bool`steuert, ob der angezeigte Kartenbereich von seinem aktuellen Bereich in den zuvor festgelegten Bereich verschoben wird, wenn eine Layoutänderung auftritt.
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)vom Typ `IList<Pin>`stellt die Liste der Pins in der Karte dar.
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)vom Typ [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)gibt den aktuell angezeigten Bereich der Karte zurück.

Diese Eigenschaften werden mit Ausnahme der Eigenschaften `MapElements`, `Pins`und `VisibleRegion` von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können.

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert auch ein `MapClicked` Ereignis, das ausgelöst wird, wenn die Zuordnung abgetippt wird. Das `MapClickedEventArgs` Objekt, das das Ereignis begleitet, verfügt über eine einzelne Eigenschaft mit dem Namen `Position`vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position). Wenn das Ereignis ausgelöst wird, wird die `Position`-Eigenschaft auf den Zuordnungs Speicherort festgelegt, der abgetippt wurde. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Informationen zu den Eigenschaften [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource), [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)und `ItemTemplateSelector` finden Sie unter [Anzeigen einer PIN](pins.md#display-a-pin-collection)-Auflistung.

## <a name="display-a-map"></a>Karte anzeigen

Eine [`Map`](xref:Xamarin.Forms.Maps.Map) kann angezeigt werden, indem Sie zu einem Layout oder einer Seite hinzugefügt wird:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> Eine zusätzliche `xmlns`-Namespace Definition ist erforderlich, um auf die xamarin. Forms. Maps-Steuerelemente zu verweisen. Im vorherigen Beispiel wird auf den `Xamarin.Forms.Maps` Namespace über das `maps`-Schlüsselwort verwiesen.

Der entsprechende C#-Code lautet:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Maps;

namespace WorkingWithMaps
{
    public class MapTypesPageCode : ContentPage
    {
        public MapTypesPageCode()
        {
            Map map = new Map();
            Content = map;
        }
    }
}
```

In diesem Beispiel wird der Standardkonstruktor [`Map`](xref:Xamarin.Forms.Maps.Map) aufgerufen, der die Karte in Rom zentriert:

[![Screenshot der Karten Steuerung mit dem Standard Speicherort unter IOS und Android](map-images/map-default.png "Karten Steuerelement mit Standard Speicherort")](map-images/map-default-large.png#lightbox "Karten Steuerelement mit Standard Speicherort")

Alternativ kann ein [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Argument an einen [`Map`](xref:Xamarin.Forms.Maps.Map) -Konstruktor übergeben werden, um den Mittelpunkt und die Zoomstufe der Karte beim Laden festzulegen. Weitere Informationen finden Sie unter [Anzeigen eines bestimmten Speicher Orts in einer Karte](#display-a-specific-location-on-a-map).

## <a name="map-types"></a>Kartentypen

Die [`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType) -Eigenschaft kann auf einen [`MapType`](xref:Xamarin.Forms.Maps.MapType) Enumerationsmember festgelegt werden, um den Anzeige Stil der Karte zu definieren. Die `MapType`-Enumeration definiert die folgenden Member:

- `Street` gibt an, dass eine Straßenkarte angezeigt wird.
- `Satellite` gibt an, dass eine Karte angezeigt wird, die Satellitenbilder enthält.
- `Hybrid` gibt an, dass eine Karte angezeigt wird, die Straßen-und Satellitendaten kombiniert.

Standardmäßig wird in einem [`Map`](xref:Xamarin.Forms.Maps.Map) eine Straßenkarte angezeigt, wenn die [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) -Eigenschaft nicht definiert ist. Alternativ kann die `MapType`-Eigenschaft auf einen der [`MapType`](xref:Xamarin.Forms.Maps.MapType) Enumerationsmember festgelegt werden:

```xaml
<maps:Map MapType="Satellite" />
```

Der entsprechende C#-Code lautet:

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , wenn die [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) -Eigenschaft auf `Street`festgelegt ist:

[![Screenshot der Karten Steuerung mit dem Straßen Kartentyp unter IOS und Android](map-images/maptype-street.png "Karten Steuerelement mit dem "Straße" der Straße")](map-images/maptype-street-large.png#lightbox "Map control with the street map type")

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , wenn die [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) -Eigenschaft auf `Satellite`festgelegt ist:

[![Screenshot der Karten Steuerung mit dem Satellitentyp unter IOS und Android](map-images/maptype-satellite.png "Karten Steuerelement mit dem satellitenmaptype")](map-images/maptype-satellite-large.png#lightbox "Map control with the satellite map type")

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , wenn die [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) -Eigenschaft auf `Hybrid`festgelegt ist:

[![Screenshot der Karten Steuerung mit dem Hybriden Kartentyp unter IOS und Android](map-images/maptype-hybrid.png "Karten Steuerelement mit dem Hybriden maptype")](map-images/maptype-hybrid-large.png#lightbox "Map control with the hybrid map type")

## <a name="display-a-specific-location-on-a-map"></a>Anzeigen einer bestimmten Position auf einer Karte

Der Bereich einer Karte, die beim Laden einer Karte angezeigt werden soll, kann durch Übergeben eines [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Arguments an den [`Map`](xref:Xamarin.Forms.Maps.Map) -Konstruktor festgelegt werden:

```xaml
<maps:Map>
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
</maps:Map>
```

Der entsprechende C#-Code lautet:

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

In diesem Beispiel wird ein [`Map`](xref:Xamarin.Forms.Maps.Map) Objekt erstellt, das den Bereich anzeigt, der durch das [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Objekt angegeben wird. Das `MapSpan` Objekt wird auf dem breiten-und Längengrad zentriert, der durch ein [`Position`](xref:Xamarin.Forms.Maps.Position) Objekt dargestellt wird, und umfasst 0,01 breiten-und 0,01 Längengrad. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md). Weitere Informationen zum Übergeben von Argumenten in XAML finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

Das Ergebnis ist, dass beim Anzeigen der Zuordnung eine bestimmte Position zentriert ist und eine bestimmte Anzahl von breiten-und Längengraden umfasst:

[![Screenshot der Karten Steuerung mit dem angegebenen Speicherort unter IOS und Android](map-images/map-region.png "Karten Steuerelement mit einem angegebenen Speicherort")](map-images/map-region-large.png#lightbox "Karten Steuerelement mit einem angegebenen Speicherort")

## <a name="create-a-mapspan-object"></a>Erstellen eines mapspan-Objekts

Es gibt eine Reihe von Vorgehensweisen zum Erstellen von [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Objekten. Ein gängiger Ansatz besteht darin, die erforderlichen Argumente für den `MapSpan`-Konstruktor bereitzustellen. Dabei handelt es sich um einen breiten-und Längengrad, der durch ein [`Position`](xref:Xamarin.Forms.Maps.Position) Objekt dargestellt wird, sowie `double` Werte, die den Grad des breiten-und Längen Grads darstellen, die vom `MapSpan`überspannt werden. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Alternativ gibt es drei Methoden in der [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Klasse, die neue `MapSpan` Objekte zurückgeben:

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*) gibt eine `MapSpan` zurück, die dieselbe `LongitudeDegrees` wie die Klasseninstanz der Methode und einen durch ihre `north` und `south` Argumente definierten Radius hat.
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*) gibt ein `MapSpan` zurück, das durch seine [`Position`](xref:Xamarin.Forms.Maps.Position) und [`Distance`](xref:Xamarin.Forms.Maps.Distance) Argumente definiert wird.
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) gibt eine `MapSpan` mit derselben Mitte wie die Klasseninstanz der Methode zurück, wobei jedoch ein RADIUS mit dem `double` Argument multipliziert ist.

Weitere Informationen zur [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Nachdem ein [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) erstellt wurde, können Sie auf die folgenden Eigenschaften zugreifen, um Daten darüber abzurufen:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center), das die [`Position`](xref:Xamarin.Forms.Maps.Position) im geografischen Zentrum der `MapSpan`darstellt.
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees), das die Breitengrade darstellt, die vom `MapSpan`überspannt werden.
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees), der den Grad der Länge darstellt, der durch die `MapSpan`überspannt wird.
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius), der den `MapSpan` Radius darstellt.

## <a name="move-the-map"></a>Verschieben der Karte

Die [`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode kann aufgerufen werden, um die Position und die Zoomstufe einer Karte zu ändern. Diese Methode akzeptiert ein [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Argument, das den Bereich der anzuzeigenden Karte und deren Zoomstufe definiert.

Der folgende Code zeigt ein Beispiel für das Verschieben des angezeigten Bereichs auf einer Karte:

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>Vergrößern der Karte

Der Zoomfaktor einer [`Map`](xref:Xamarin.Forms.Maps.Map) kann geändert werden, ohne den Speicherort zu ändern. Dies kann mithilfe der kartenbenutzer Oberfläche oder Programm gesteuert durch Aufrufen der [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode mit einem [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Argument erreicht werden, das den aktuellen Speicherort als [`Position`](xref:Xamarin.Forms.Maps.Position) Argument verwendet:

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

In diesem Beispiel wird die [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode mit einem [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Argument aufgerufen, das den aktuellen Speicherort der Karte, über die [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) -Eigenschaft und die Zoomstufe als Grad an breiten-und Längengrad angibt. Das Gesamtergebnis ist, dass die Zoomstufe der Karte geändert wird, der Speicherort jedoch nicht. Ein alternativer Ansatz für die Implementierung von Zoom auf einer Karte ist die Verwendung der [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) -Methode, um den Zoomfaktor zu steuern.

> [!IMPORTANT]
> Wenn Sie eine Zuordnung Zoomen, ob über die kartenbenutzer Oberfläche oder Programm gesteuert, ist es erforderlich, dass die [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) -Eigenschaft `true`ist. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Zoom deaktivieren](#disable-zoom).

## <a name="customize-map-behavior"></a>Anpassen des Zuordnungs Verhaltens

Das Verhalten einer [`Map`](xref:Xamarin.Forms.Maps.Map) kann angepasst werden, indem einige der zugehörigen Eigenschaften festgelegt werden und das `MapClicked` Ereignis verarbeitet wird.

> [!NOTE]
> Ein zusätzliches Zuordnungs Verhalten kann durch Erstellen eines benutzerdefinierten Renderers für Zuordnungen erreicht werden. Weitere Informationen finden Sie unter [Anpassen einer xamarin. Forms-Karte](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

### <a name="disable-scroll"></a>Scrollvorgang deaktivieren

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert eine [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) Eigenschaft vom Typ `bool`. Standardmäßig ist diese Eigenschaft `true`, was darauf hinweist, dass der Zuordnung ein Bildlauf zulässig ist. Wenn diese Eigenschaft auf `false`festgelegt ist, wird die Zuordnung nicht durchlaufen. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

```xaml
<maps:Map HasScrollEnabled="false" />
```

Der entsprechende C#-Code lautet:

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>Zoom deaktivieren

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert eine [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) Eigenschaft vom Typ `bool`. Standardmäßig ist diese Eigenschaft `true`, was darauf hinweist, dass Zoom auf der Karte ausgeführt werden kann. Wenn diese Eigenschaft auf `false`festgelegt ist, kann die Zuordnung nicht vergrößert werden. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

```xaml
<maps:Map HasZoomEnabled="false" />
```

Der entsprechende C#-Code lautet:

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>Speicherort des Benutzers anzeigen

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert eine [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) Eigenschaft vom Typ `bool`. Standardmäßig ist diese Eigenschaft `false`. Dies bedeutet, dass die Zuordnung nicht den aktuellen Speicherort des Benutzers anzeigt. Wenn diese Eigenschaft auf `true`festgelegt ist, zeigt die Karte den aktuellen Speicherort des Benutzers an. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

```xaml
<maps:Map IsShowingUser="true" />
```

Der entsprechende C#-Code lautet:

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> Unter IOS, Android und der universelle Windows-Plattform muss für den Zugriff auf den Speicherort des Benutzers Standort Berechtigungen für die Anwendung erteilt worden sein. Weitere Informationen finden Sie unter [Platt Form Konfiguration](setup.md#platform-configuration).

### <a name="maintain-map-region-on-layout-change"></a>Kartenbereich bei Layoutänderung beibehalten

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert eine `MoveToLastRegionOnLayoutChange` Eigenschaft vom Typ `bool`. Standardmäßig ist diese Eigenschaft `true`. Dies bedeutet, dass der angezeigte Kartenbereich von seinem aktuellen Bereich in den zuvor festgelegten Bereich wechselt, wenn eine Layoutänderung auftritt, z. b. bei der Geräte Rotation. Wenn diese Eigenschaft auf `false` festgelegt ist, bleibt der angezeigte Kartenbereich zentriert, wenn eine Layoutänderung auftritt. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

Der entsprechende C#-Code lautet:

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>Karten Klicks

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert ein `MapClicked` Ereignis, das ausgelöst wird, wenn die Karte abgetippt wird. Das `MapClickedEventArgs` Objekt, das das Ereignis begleitet, verfügt über eine einzelne Eigenschaft mit dem Namen `Position`vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position). Wenn das Ereignis ausgelöst wird, wird die `Position`-Eigenschaft auf den Zuordnungs Speicherort festgelegt, der abgetippt wurde. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Das folgende Codebeispiel zeigt einen Ereignishandler für das `MapClicked`-Ereignis:

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

In diesem Beispiel gibt der `OnMapClicked`-Ereignishandler den breiten-und Längengrad aus, der den gezapften Zuordnungs Speicherort darstellt. Der Ereignishandler kann wie folgt beim `MapClicked`-Ereignis registriert werden:

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

Der entsprechende C#-Code lautet:

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Karten Position und-Entfernung](position-distance.md)
- [Anpassen einer xamarin. Forms-Karte](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
