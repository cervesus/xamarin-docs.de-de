---
title: Xamarin. Forms Karten Polygone, Polylines und Kreise
description: In diesem Artikel wird erläutert, wie Polygone, Polylines und Kreise in einer xamarin. Forms-Karten Instanz erstellt werden.
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: e1edbc4d7376023c9d3051b0518c8dc7368e63a7
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517341"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin. Forms-Karten Polygone und Polylines

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

`Polygon`die `Polyline`Elemente, `Circle` und ermöglichen es Ihnen, bestimmte Bereiche auf einer Karte hervorzuheben. Eine `Polygon` ist eine vollständig eingeschlossene Form, die einen Strich und eine Füllfarbe aufweisen kann. Ein `Polyline` ist eine Zeile, die einen Bereich nicht vollständig umschließt. Ein `Circle` hebt einen kreisförmigen Bereich der Karte hervor:

[!["Screenshot eines Karten Polygons und Polylinien unter IOS und Android"](polygons-images/polygon-polyline.png "Polygon und Polylinie in einer Karte")](polygons-images/polygon-polyline-large.png#lightbox "Polygon und Polylinie in einer Karte")
"[![Screenshot eines Karten Kreises unter IOS und Android"](polygons-images/circle.png "Kreis auf einer Karte")](polygons-images/circle-large.png#lightbox "Kreis auf einer Karte")

Die `Polygon`Klassen `Polyline`, und `Circle` werden von der `MapElement` -Klasse abgeleitet, die die folgenden bindbaren Eigenschaften verfügbar macht:

- `StrokeColor`ist ein `Color` -Objekt, das die Linien Farbe bestimmt.
- `StrokeWidth`ist ein `float` -Objekt, das die Linienbreite bestimmt.

Die `Polygon` -Klasse definiert eine zusätzliche bindbare Eigenschaft:

- `FillColor`ist ein `Color` -Objekt, das die Hintergrundfarbe des Polygons bestimmt.

Außerdem definieren sowohl die `Polygon` - `Polyline` Klasse als auch die `GeoPath` -Klasse eine-Eigenschaft, die [`Position`](xref:Xamarin.Forms.Maps.Position) eine Liste von-Objekten ist, die die Punkte der Form angeben.

Die `Circle` -Klasse definiert die folgenden bindbaren Eigenschaften:

- `Center`ist ein [`Position`](xref:Xamarin.Forms.Maps.Position) -Objekt, das den Mittelpunkt des Kreises in breiten-und Längengrad definiert.
- `Radius`ist ein [`Distance`](xref:Xamarin.Forms.Maps.Distance) -Objekt, das den Radius des Kreises in Meter, Kilometern oder km definiert.
- `FillColor`eine `Color` Eigenschaft, die die Farbe innerhalb des Kreis Umkreises bestimmt.

> [!NOTE]
> Wenn die `StrokeColor` Eigenschaft nicht angegeben wird, wird der Strich standardmäßig auf schwarz festgelegt. Wenn die `FillColor` -Eigenschaft nicht angegeben ist, wird die Füllung standardmäßig auf transparent festgelegt. Wenn keine Eigenschaft angegeben wird, hat die Form eine schwarze Kontur ohne Füllung.

## <a name="create-a-polygon"></a>Erstellen eines Polygons

Ein `Polygon` -Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der Auflistung der `MapElements` Zuordnung hinzugefügt wird. Dies kann in XAML folgendermaßen erfüllt werden:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polygon StrokeColor="#FF9900"
                           StrokeWidth="8"
                           FillColor="#88FF9900">
                 <maps:Polygon.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6368678</x:Double>
                             <x:Double>-122.137305</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polygon.Geopath>
             </maps:Polygon>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};

// instantiate a polygon
Polygon polygon = new Polygon
{
    StrokeWidth = 8,
    StrokeColor = Color.FromHex("#1BA1E2"),
    FillColor = Color.FromHex("#881BA1E2"),
    Geopath =
    {
        new Position(47.6368678, -122.137305),
        new Position(47.6368894, -122.134655),
        new Position(47.6359424, -122.134655),
        new Position(47.6359496, -122.1325521),
        new Position(47.6424124, -122.1325199),
        new Position(47.642463,  -122.1338932),
        new Position(47.6406414, -122.1344833),
        new Position(47.6384943, -122.1361248),
        new Position(47.6372943, -122.1376912)
    }
};

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

Die `StrokeColor` - `StrokeWidth` Eigenschaft und die-Eigenschaft werden angegeben, um die Kontur des Polygons anzupassen. Der `FillColor` Eigenschafts Wert stimmt `StrokeColor` mit dem Eigenschafts Wert überein, verfügt jedoch über einen angegebenen Alpha Wert, um ihn transparent zu machen, sodass die zugrunde liegende Karte durch die Form sichtbar ist. Die `GeoPath` -Eigenschaft enthält eine Liste `Position` von-Objekten, die die geografischen Koordinaten der Polygon Punkte definieren. Ein `Polygon` -Objekt wird in der Zuordnung gerendert, sobald es der `MapElements` -Auflistung von `Map`hinzugefügt wurde.

> [!NOTE]
> Eine `Polygon` ist eine vollständig eingeschlossene Form. Der erste und der letzte Punkt werden automatisch verbunden, wenn keine Entsprechung gefunden wird.

## <a name="create-a-polyline"></a>Erstellen einer Polylinie

Ein `Polyline` -Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der Auflistung der `MapElements` Zuordnung hinzugefügt wird. Dies kann in XAML folgendermaßen erfüllt werden:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polyline StrokeColor="Blue"
                            StrokeWidth="12">
                 <maps:Polyline.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6381401</x:Double>
                             <x:Double>-122.1317367</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polyline.Geopath>
             </maps:Polyline>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
// instantiate a polyline
Polyline polyline = new Polyline
{
    StrokeColor = Color.Blue,
    StrokeWidth = 12,
    Geopath =
    {
        new Position(47.6381401, -122.1317367),
        new Position(47.6381473, -122.1350841),
        new Position(47.6382847, -122.1353094),
        new Position(47.6384582, -122.1354703),
        new Position(47.6401136, -122.1360819),
        new Position(47.6403883, -122.1364681),
        new Position(47.6407426, -122.1377019),
        new Position(47.6412558, -122.1404056),
        new Position(47.6414148, -122.1418647),
        new Position(47.6414654, -122.1432702)
    }
};

// add the polyline to the map's MapElements collection
map.MapElements.Add(polyline);
```

Die `StrokeColor` - `StrokeWidth` Eigenschaft und die-Eigenschaft werden angegeben, um die Zeile anzupassen. Die `GeoPath` -Eigenschaft enthält eine Liste `Position` von-Objekten, die die geografischen Koordinaten der polyzeiligen Punkte definieren. Ein `Polyline` -Objekt wird in der Zuordnung gerendert, sobald es der `MapElements` -Auflistung von `Map`hinzugefügt wurde.

## <a name="create-a-circle"></a>Erstellen eines Kreises

Ein `Circle` -Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der Auflistung der `MapElements` Zuordnung hinzugefügt wird. Dies kann in XAML folgendermaßen erfüllt werden:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
      <maps:Map>
          <maps:Map.MapElements>
              <maps:Circle StrokeColor="#88FF0000"
                           StrokeWidth="8"
                           FillColor="#88FFC0CB">
                  <maps:Circle.Center>
                      <maps:Position>
                          <x:Arguments>
                              <x:Double>37.79752</x:Double>
                              <x:Double>-122.40183</x:Double>
                          </x:Arguments>
                      </maps:Position>
                  </maps:Circle.Center>
                  <maps:Circle.Radius>
                      <maps:Distance>
                          <x:Arguments>
                              <x:Double>250</x:Double>
                          </x:Arguments>
                      </maps:Distance>
                  </maps:Circle.Radius>
              </maps:Circle>             
          </maps:Map.MapElements>
          ...
      </maps:Map>
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map();

// Instantiate a Circle
Circle circle = new Circle
{
    Center = new Position(37.79752, -122.40183);,
    Radius = new Distance(250),
    StrokeColor = Color.FromHex("#88FF0000"),
    StrokeWidth = 8,
    FillColor = Color.FromHex("#88FFC0CB")
};

// Add the Circle to the map's MapElements collection
map.MapElements.Add(circle);
```

Der Speicherort der `Circle` auf der Karte wird durch den Wert der `Center` -Eigenschaft und `Radius` der-Eigenschaft bestimmt. Die `Center` -Eigenschaft definiert den Mittelpunkt des Kreises in breiten-und Längengrad, während `Radius` die-Eigenschaft den Radius des Kreises in Meter definiert. Die `StrokeColor` - `StrokeWidth` Eigenschaft und die-Eigenschaft werden angegeben, um die Kontur des Kreises anzupassen. Der `FillColor` -Eigenschafts Wert gibt die Farbe innerhalb des Kreis Umkreises an. Beide Farbwerte geben einen Alphakanal an, sodass die zugrunde liegende Karte über den Kreis sichtbar ist. Das `Circle` -Objekt wird in der Zuordnung gerendert, nachdem es der `MapElements` -Auflistung von `Map`hinzugefügt wurde.

> [!NOTE]
> Die `GeographyUtils` -Klasse verfügt `ToCircumferencePositions` über eine-Erweiterungsmethode `Circle` , die ein- `Center` Objekt `Radius` (das die-Eigenschaftswerte `Position` definiert) in eine Liste von-Objekten konvertiert, die die breiten-und Längenkoordinaten des Kreis Umkreises bilden.

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
