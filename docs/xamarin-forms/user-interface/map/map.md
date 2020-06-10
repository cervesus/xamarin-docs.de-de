---
Title: " Xamarin.Forms Kartensteuerelement" Beschreibung: "das Karten Steuerelement ist eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung. "
ms. Prod: xamarin ms. assetid: 22c99029-0b16-43a6-BF 58-26b48c4aed38 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/29/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-map-control"></a>Xamarin.FormsKartensteuerelement

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Das [`Map`](xref:Xamarin.Forms.Maps.Map) -Steuerelement ist eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung:

[![Screenshot der Karten Steuerung unter IOS und Android](map-images/map-default.png "Kartensteuerelement")](map-images/map-default-large.png#lightbox "Kartensteuerelement")

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert die folgenden Eigenschaften, die die Darstellung und das Verhalten von Karten Steuern:

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)gibt mit dem Typ an `bool` , ob die Karte den aktuellen Speicherort des Benutzers anzeigt.
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)vom Typ `IEnumerable` , der die Auflistung der `IEnumerable` anzuzeigenden Elemente angibt.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der das angibt, das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden soll.
- `ItemTemplateSelector`vom Typ [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , der die angibt, die [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)bestimmt, ob für die Zuordnung ein Bildlauf zulässig ist, vom Typ `bool` .
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)bestimmt vom Typ `bool` , ob die Zuordnung vergrößert werden darf.
- `MapElements`stellt vom Typ `IList<MapElement>` die Liste der Elemente in der Zuordnung dar, z. b. Polygone und Polylines.
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)gibt den Anzeige Stil der Karte an.
- `MoveToLastRegionOnLayoutChange`, vom Typ `bool` , steuert, ob der angezeigte Kartenbereich von seinem aktuellen Bereich in den zuvor festgelegten Bereich verschoben wird, wenn eine Layoutänderung auftritt.
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)stellt vom Typ `IList<Pin>` die Liste der Pins in der Karte dar.
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)gibt den aktuell angezeigten Bereich der Zuordnung vom Typ zurück.

Diese Eigenschaften, mit Ausnahme der `MapElements` `Pins` Eigenschaften, und `VisibleRegion` , werden von-Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können.

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert auch ein `MapClicked` -Ereignis, das ausgelöst wird, wenn die Zuordnung abgetippt wird. Das `MapClickedEventArgs` Objekt, das das Ereignis begleitet, verfügt über eine einzelne Eigenschaft namens `Position` vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position) . Wenn das-Ereignis ausgelöst wird, `Position` wird die-Eigenschaft auf den Zuordnungs Speicherort festgelegt, der abgetippt wurde. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Informationen zu den [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) Eigenschaften, [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) und `ItemTemplateSelector` finden Sie unter [Anzeigen einer PIN](pins.md#display-a-pin-collection)-Auflistung.

## <a name="display-a-map"></a>Karte anzeigen

Ein [`Map`](xref:Xamarin.Forms.Maps.Map) kann angezeigt werden, indem es einem Layout oder einer Seite hinzugefügt wird:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> Eine zusätzliche `xmlns` Namespace Definition ist erforderlich, um auf zu verweisen Xamarin.Forms . Ordnet Steuerelemente zu. Im vorherigen Beispiel wird auf den `Xamarin.Forms.Maps` Namespace über das- `maps` Schlüsselwort verwiesen.

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

In diesem Beispiel wird der [`Map`](xref:Xamarin.Forms.Maps.Map) Standardkonstruktor aufgerufen, der die Karte in Rom zentriert:

[![Screenshot der Karten Steuerung mit dem Standard Speicherort unter IOS und Android](map-images/map-default.png "Karten Steuerelement mit Standard Speicherort")](map-images/map-default-large.png#lightbox "Karten Steuerelement mit Standard Speicherort")

Alternativ kann ein- [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Argument an einen- [`Map`](xref:Xamarin.Forms.Maps.Map) Konstruktor übergeben werden, um den Mittelpunkt und die Zoomstufe der Karte beim Laden festzulegen. Weitere Informationen finden Sie unter [Anzeigen eines bestimmten Speicher Orts in einer Karte](#display-a-specific-location-on-a-map).

## <a name="map-types"></a>Kartentypen

Die- [`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType) Eigenschaft kann auf einen- [`MapType`](xref:Xamarin.Forms.Maps.MapType) Enumerationsmember festgelegt werden, um den Anzeige Stil der Karte zu definieren. Die `MapType`-Enumeration definiert die folgenden Member:

- `Street`Gibt an, dass eine Straßenkarte angezeigt wird.
- `Satellite`Gibt an, dass eine Karte angezeigt wird, die Satellitenbilder enthält.
- `Hybrid`Gibt an, dass eine Karte angezeigt wird, die Straßen-und Satellitendaten kombiniert.

Standardmäßig wird eine [`Map`](xref:Xamarin.Forms.Maps.Map) Straßenkarte angezeigt, wenn die- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) Eigenschaft nicht definiert ist. Alternativ kann die- `MapType` Eigenschaft auf einen der-Enumerationsmember festgelegt werden [`MapType`](xref:Xamarin.Forms.Maps.MapType) :

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

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , wenn die- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) Eigenschaft auf festgelegt ist `Street` :

[![Screenshot der Karten Steuerung mit dem Straßen Kartentyp unter IOS und Android](map-images/maptype-street.png "Karten Steuerelement mit dem "Straße" der Straße")](map-images/maptype-street-large.png#lightbox "Karten Steuerelement mit dem Typ "Street Map"")

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , wenn die- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) Eigenschaft auf festgelegt ist `Satellite` :

[![Screenshot der Karten Steuerung mit dem Satellitentyp unter IOS und Android](map-images/maptype-satellite.png "Karten Steuerelement mit dem satellitenmaptype")](map-images/maptype-satellite-large.png#lightbox "Karten Steuerelement mit dem satellitenkartentyp")

Die folgenden Screenshots zeigen eine [`Map`](xref:Xamarin.Forms.Maps.Map) , wenn die- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) Eigenschaft auf festgelegt ist `Hybrid` :

[![Screenshot der Karten Steuerung mit dem Hybriden Kartentyp unter IOS und Android](map-images/maptype-hybrid.png "Karten Steuerelement mit dem Hybriden maptype")](map-images/maptype-hybrid-large.png#lightbox "Karten Steuerelement mit dem Hybrid Kartentyp")

## <a name="display-a-specific-location-on-a-map"></a>Anzeigen einer bestimmten Position auf einer Karte

Der Bereich einer Karte, die beim Laden einer Karte angezeigt werden soll, kann durch Übergeben eines- [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Arguments an den-Konstruktor festgelegt werden [`Map`](xref:Xamarin.Forms.Maps.Map) :

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

In diesem Beispiel wird ein- [`Map`](xref:Xamarin.Forms.Maps.Map) Objekt erstellt, das den Bereich anzeigt, der durch das-Objekt angegeben wird [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . Das `MapSpan` -Objekt wird auf den breiten-und Längengrad zentriert, der durch ein [`Position`](xref:Xamarin.Forms.Maps.Position) -Objekt dargestellt wird, und erstreckt sich über 0,01 Breitengrad und 0,01 Längengrad. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md). Weitere Informationen zum Übergeben von Argumenten in XAML finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

Das Ergebnis ist, dass beim Anzeigen der Zuordnung eine bestimmte Position zentriert ist und eine bestimmte Anzahl von breiten-und Längengraden umfasst:

[![Screenshot der Karten Steuerung mit dem angegebenen Speicherort unter IOS und Android](map-images/map-region.png "Karten Steuerelement mit einem angegebenen Speicherort")](map-images/map-region-large.png#lightbox "Karten Steuerelement mit einem angegebenen Speicherort")

## <a name="create-a-mapspan-object"></a>Erstellen eines mapspan-Objekts

Es gibt eine Reihe von Ansätzen zum Erstellen von- [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Objekten. Ein gängiger Ansatz besteht darin, die erforderlichen Argumente für den `MapSpan` Konstruktor bereitzustellen. Dabei handelt es sich um einen breiten-und Längengrad, der durch ein [`Position`](xref:Xamarin.Forms.Maps.Position) -Objekt dargestellt wird, und um-Werte, die `double` den Grad des breiten-und Längen Grads darstellen `MapSpan` . Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Alternativ gibt es drei Methoden in der- [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) Klasse, die neue `MapSpan` Objekte zurückgeben:

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)Gibt einen `MapSpan` mit dem gleichen Wert `LongitudeDegrees` wie die-Klasseninstanz der Methode und einen durch das `north` -Argument und das-Argument definierten Radius zurück `south` .
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)Gibt einen zurück `MapSpan` , der durch seine [`Position`](xref:Xamarin.Forms.Maps.Position) -und-Argumente definiert wird [`Distance`](xref:Xamarin.Forms.Maps.Distance) .
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)gibt eine `MapSpan` mit derselben Mitte wie die Klasseninstanz der Methode zurück, wobei jedoch ein RADIUS mit dem Argument multipliziert ist `double` .

Weitere Informationen zur [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Nachdem ein [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) erstellt wurde, können Sie auf die folgenden Eigenschaften zugreifen, um Daten darüber abzurufen:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center), der die [`Position`](xref:Xamarin.Forms.Maps.Position) im geografischen Zentrum der darstellt `MapSpan` .
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees), der den Grad des breiten Grads darstellt, der von der überspannt wird `MapSpan` .
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees), der den Grad der Länge darstellt, der von der überspannt wird `MapSpan` .
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius), der den `MapSpan` RADIUS darstellt.

## <a name="move-the-map"></a>Verschieben der Karte

Die- [`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) Methode kann aufgerufen werden, um die Position und die Zoomstufe einer Karte zu ändern. Diese Methode akzeptiert ein [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Argument, das den Bereich der anzuzeigenden Karte und deren Zoomstufe definiert.

Der folgende Code zeigt ein Beispiel für das Verschieben des angezeigten Bereichs auf einer Karte:

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>Zoomen der Karte

Der Zoomfaktor eines [`Map`](xref:Xamarin.Forms.Maps.Map) kann geändert werden, ohne den Speicherort zu ändern. Dies kann mithilfe der kartenbenutzer Oberfläche oder Programm gesteuert durch Aufrufen der- [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) Methode mit einem-Argument erreicht werden, [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) das den aktuellen Speicherort als [`Position`](xref:Xamarin.Forms.Maps.Position) Argument verwendet:

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

In diesem Beispiel wird die [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode mit einem [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) -Argument aufgerufen, das den aktuellen Speicherort der Karte, über die [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) -Eigenschaft und die Zoomstufe als Grad an breiten-und Längengrad angibt. Das Gesamtergebnis ist, dass die Zoomstufe der Karte geändert wird, der Speicherort jedoch nicht. Ein alternativer Ansatz für die Implementierung von Zoom auf einer Karte ist die Verwendung der- [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) Methode, um den Zoomfaktor zu steuern.

> [!IMPORTANT]
> Wenn Sie eine Zuordnung Zoomen, ob über die kartenbenutzer Oberfläche oder Programm gesteuert, ist es erforderlich, dass die- [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) Eigenschaft ist `true` . Weitere Informationen zu dieser Eigenschaft finden Sie unter [Zoom deaktivieren](#disable-zoom).

## <a name="customize-map-behavior"></a>Anpassen des Zuordnungs Verhaltens

Das Verhalten eines [`Map`](xref:Xamarin.Forms.Maps.Map) kann angepasst werden, indem einige seiner Eigenschaften festgelegt werden und das-Ereignis behandelt wird `MapClicked` .

> [!NOTE]
> Weitere Anpassungs Verhalten können durch Erstellen eines benutzerdefinierten Renderers für Zuordnungen erreicht werden. Weitere Informationen finden Sie unter [Anpassen einer Xamarin.Forms Karte](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md).

### <a name="disable-scroll"></a>Scrollvorgang deaktivieren

Die- [`Map`](xref:Xamarin.Forms.Maps.Map) Klasse definiert eine [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) Eigenschaft vom Typ `bool` . Standardmäßig ist diese Eigenschaft `true` , was angibt, dass die Zuordnung zum Scrollen zugelassen ist. Wenn diese Eigenschaft auf festgelegt ist `false` , wird die Zuordnung nicht durchlaufen. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

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

Die- [`Map`](xref:Xamarin.Forms.Maps.Map) Klasse definiert eine [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) Eigenschaft vom Typ `bool` . Standardmäßig ist diese Eigenschaft `true` , wodurch angegeben wird, dass Zoom auf der Karte ausgeführt werden kann. Wenn diese Eigenschaft auf festgelegt ist `false` , kann die Zuordnung nicht vergrößert werden. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

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

Die- [`Map`](xref:Xamarin.Forms.Maps.Map) Klasse definiert eine [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) Eigenschaft vom Typ `bool` . Standardmäßig ist diese Eigenschaft `false` . Dies bedeutet, dass die Zuordnung nicht den aktuellen Speicherort des Benutzers anzeigt. Wenn diese Eigenschaft auf festgelegt ist `true` , zeigt die Karte den aktuellen Speicherort des Benutzers an. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

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

Die- [`Map`](xref:Xamarin.Forms.Maps.Map) Klasse definiert eine `MoveToLastRegionOnLayoutChange` Eigenschaft vom Typ `bool` . Standardmäßig ist diese Eigenschaft. dies `true` bedeutet, dass der angezeigte Zuordnungs Bereich beim Auftreten einer Layoutänderung von seinem aktuellen Bereich in den zuvor festgelegten Bereich wechselt, z. b. bei der Geräte Rotation. Wenn diese Eigenschaft auf festgelegt ist `false` , bleibt der angezeigte Kartenbereich zentriert, wenn eine Layoutänderung auftritt. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

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

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse definiert ein `MapClicked` -Ereignis, das ausgelöst wird, wenn die Karte abgetippt wird. Das `MapClickedEventArgs` Objekt, das das Ereignis begleitet, verfügt über eine einzelne Eigenschaft namens `Position` vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position) . Wenn das-Ereignis ausgelöst wird, `Position` wird die-Eigenschaft auf den Zuordnungs Speicherort festgelegt, der abgetippt wurde. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

Das folgende Codebeispiel zeigt einen Ereignishandler für das- `MapClicked` Ereignis:

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

In diesem Beispiel gibt der `OnMapClicked` -Ereignishandler die breiten-und Längengrade aus, die den angegeppten Zuordnungs Speicherort darstellen. Der Ereignishandler kann wie folgt mit dem-Ereignis registriert werden `MapClicked` :

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
- [Anpassen einer Xamarin.Forms Karte](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
