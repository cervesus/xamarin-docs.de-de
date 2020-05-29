---
title: Xamarin.FormsKarten Polygone, Polylines und Kreise
description: In diesem Artikel wird erläutert, wie Polygone, Polylines und Kreise auf einer Xamarin.Forms Karten Instanz erstellt werden.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ca813f9f0f75aeaf4a2502faa7cb96d1fbead471
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138410"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin.FormsKarten Polygone und Polylines

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

`Polygon``Polyline` `Circle` die Elemente, und ermöglichen es Ihnen, bestimmte Bereiche auf einer Karte hervorzuheben. Eine `Polygon` ist eine vollständig eingeschlossene Form, die einen Strich und eine Füllfarbe aufweisen kann. Ein `Polyline` ist eine Zeile, die einen Bereich nicht vollständig umschließt. Ein `Circle` hebt einen kreisförmigen Bereich der Karte hervor:

[!["Screenshot eines Karten Polygons und Polylinien unter IOS und Android"](polygons-images/polygon-polyline.png "Polygon und Polylinie in einer Karte")](polygons-images/polygon-polyline-large.png#lightbox "Polygon und Polylinie in einer Karte") 
 [ !["Screenshot eines Karten Kreises unter IOS und Android"](polygons-images/circle.png "Kreis auf einer Karte")](polygons-images/circle-large.png#lightbox "Kreis auf einer Karte")

Die `Polygon` `Polyline` Klassen, und `Circle` werden von der- `MapElement` Klasse abgeleitet, die die folgenden bindbaren Eigenschaften verfügbar macht:

- `StrokeColor`ist ein- `Color` Objekt, das die Linien Farbe bestimmt.
- `StrokeWidth`ist ein- `float` Objekt, das die Linienbreite bestimmt.

Die- `Polygon` Klasse definiert eine zusätzliche bindbare Eigenschaft:

- `FillColor`ist ein `Color` -Objekt, das die Hintergrundfarbe des Polygons bestimmt.

Außerdem `Polygon` definieren sowohl die-Klasse als auch die- `Polyline` Klasse eine- `GeoPath` Eigenschaft, die eine Liste von-Objekten ist, die [`Position`](xref:Xamarin.Forms.Maps.Position) die Punkte der Form angeben.

Die- `Circle` Klasse definiert die folgenden bindbaren Eigenschaften:

- `Center`ist ein- [`Position`](xref:Xamarin.Forms.Maps.Position) Objekt, das den Mittelpunkt des Kreises in breiten-und Längengrad definiert.
- `Radius`ist ein- [`Distance`](xref:Xamarin.Forms.Maps.Distance) Objekt, das den Radius des Kreises in Meter, Kilometern oder km definiert.
- `FillColor`eine `Color` Eigenschaft, die die Farbe innerhalb des Kreis Umkreises bestimmt.

> [!NOTE]
> Wenn die `StrokeColor` Eigenschaft nicht angegeben wird, wird der Strich standardmäßig auf schwarz festgelegt. Wenn die- `FillColor` Eigenschaft nicht angegeben ist, wird die Füllung standardmäßig auf transparent festgelegt. Wenn keine Eigenschaft angegeben wird, hat die Form eine schwarze Kontur ohne Füllung.

## <a name="create-a-polygon"></a>Erstellen eines Polygons

Ein `Polygon` -Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der Auflistung der Zuordnung hinzugefügt wird `MapElements` . Dies kann in XAML folgendermaßen erfüllt werden:

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

Die-Eigenschaft und die-Eigenschaft `StrokeColor` `StrokeWidth` werden angegeben, um die Kontur des Polygons anzupassen. Der `FillColor` Eigenschafts Wert stimmt `StrokeColor` mit dem Eigenschafts Wert überein, verfügt jedoch über einen angegebenen Alpha Wert, um ihn transparent zu machen, sodass die zugrunde liegende Karte durch die Form sichtbar ist. Die- `GeoPath` Eigenschaft enthält eine Liste von- `Position` Objekten, die die geografischen Koordinaten der Polygon Punkte definieren. Ein- `Polygon` Objekt wird in der Zuordnung gerendert, sobald es der-Auflistung von hinzugefügt wurde `MapElements` `Map` .

> [!NOTE]
> Eine `Polygon` ist eine vollständig eingeschlossene Form. Der erste und der letzte Punkt werden automatisch verbunden, wenn keine Entsprechung gefunden wird.

## <a name="create-a-polyline"></a>Erstellen einer Polylinie

Ein `Polyline` -Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der Auflistung der Zuordnung hinzugefügt wird `MapElements` . Dies kann in XAML folgendermaßen erfüllt werden:

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

Die `StrokeColor` -Eigenschaft und die-Eigenschaft `StrokeWidth` werden angegeben, um die Zeile anzupassen. Die- `GeoPath` Eigenschaft enthält eine Liste von- `Position` Objekten, die die geografischen Koordinaten der polyzeiligen Punkte definieren. Ein- `Polyline` Objekt wird in der Zuordnung gerendert, sobald es der-Auflistung von hinzugefügt wurde `MapElements` `Map` .

## <a name="create-a-circle"></a>Erstellen eines Kreises

Ein `Circle` -Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der Auflistung der Zuordnung hinzugefügt wird `MapElements` . Dies kann in XAML folgendermaßen erfüllt werden:

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
    Center = new Position(37.79752, -122.40183),
    Radius = new Distance(250),
    StrokeColor = Color.FromHex("#88FF0000"),
    StrokeWidth = 8,
    FillColor = Color.FromHex("#88FFC0CB")
};

// Add the Circle to the map's MapElements collection
map.MapElements.Add(circle);
```

Der Speicherort der `Circle` auf der Karte wird durch den Wert der-Eigenschaft `Center` und der-Eigenschaft bestimmt `Radius` . Die- `Center` Eigenschaft definiert den Mittelpunkt des Kreises in breiten-und Längengrad, während die- `Radius` Eigenschaft den Radius des Kreises in Meter definiert. Die-Eigenschaft `StrokeColor` und die `StrokeWidth` -Eigenschaft werden angegeben, um die Kontur des Kreises anzupassen. Der- `FillColor` Eigenschafts Wert gibt die Farbe innerhalb des Kreis Umkreises an. Beide Farbwerte geben einen Alphakanal an, sodass die zugrunde liegende Karte über den Kreis sichtbar ist. Das- `Circle` Objekt wird in der Zuordnung gerendert, nachdem es der-Auflistung von hinzugefügt wurde `MapElements` `Map` .

> [!NOTE]
> Die `GeographyUtils` -Klasse verfügt über eine- `ToCircumferencePositions` Erweiterungsmethode, die ein `Circle` -Objekt (das die `Center` - `Radius` Eigenschaftswerte definiert) in eine Liste von-Objekten konvertiert, die `Position` die breiten-und Längenkoordinaten des Kreis Umkreises bilden.

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
