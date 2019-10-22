---
title: Xamarin. Forms-Karten Polygone und Polylines
description: In diesem Artikel wird erläutert, wie Polygone und Polylinien in einer xamarin. Forms-Karten Instanz erstellt werden.
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/20/2019
ms.openlocfilehash: b45a7af917e9147f519056ee5a9e5d06da51113a
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697662"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin. Forms-Karten Polygone und Polylines

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[![ "Polygon-und polyliniendemo unter IOS und Android"](polygons-images/polygon-app-cropped.png)](polygons-images/polygon-app.png#lightbox)

die Elemente `Polygon` und `Polyline` ermöglichen es Ihnen, bestimmte Bereiche auf einer Karte hervorzuheben. Ein `Polygon` ist eine vollständig eingeschlossene Form, die einen Strich und eine Füllfarbe aufweisen kann. Ein `Polyline` ist eine Zeile, die einen Bereich nicht vollständig umschließt.

> [!NOTE]
> Beispiele für `Polygon` und `Polyline` finden Sie unter dem **polygonspage** im Beispiel Projekt.

Die Klassen `Polygon` und `Polyline` werden von `MapElement` abgeleitet, das die folgenden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Eigenschaften verfügbar macht:

- `StrokeColor` ist eine `Color` Eigenschaft, die die Linien Farbe bestimmt.
- `StrokeWidth` ist eine `float` Eigenschaft, die die Linienbreite bestimmt.
- `Geopath` wird sowohl für `Polygon` als auch für `Polyline` definiert, und ist eine Liste von [`Position`](xref:Xamarin.Forms.Maps.Position) Objekten, die die Punkte der Form angeben.

Die `Polygon`-Klasse definiert eine zusätzliche Eigenschaft:

- `FillColor` ist eine `Color` Eigenschaft, die die Hintergrundfarbe des Polygons bestimmt.

> [!NOTE]
> Wenn die `StrokeColor`-Eigenschaft nicht angegeben ist, wird der Strich standardmäßig auf schwarz festgelegt. Wenn die `FillColor`-Eigenschaft nicht angegeben ist, wird für die Füllung standardmäßig transparent verwendet. Wenn keine Eigenschaft angegeben wird, hat die Form eine schwarze Kontur ohne Füllung.

## <a name="create-a-polygon"></a>Erstellen eines Polygons

Ein `Polygon`-Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der `MapElements` Auflistung der Karte hinzugefügt wird:

```csharp
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
}

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

Eine `Polygon` kann auch in XAML erstellt werden:

```xaml
<maps:Map x:Name="map">
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
```

Die Eigenschaften `StrokeColor` und `StrokeWidth` werden angegeben, um die Gliederung des Polygons anzupassen. Der `FillColor`-Eigenschafts Wert entspricht dem `StrokeColor`-Eigenschafts Wert, hat aber einen Alpha-Wert angegeben, um ihn transparent zu machen, sodass die zugrunde liegende Karte durch die Form sichtbar ist. Die `GeoPath`-Eigenschaft enthält eine Liste von `Position` Objekten, die die geografischen Koordinaten der Polygon Punkte definieren. Ein `Polygon`-Objekt wird in der Zuordnung gerendert, nachdem es der `MapElements` Auflistung der `Map` hinzugefügt wurde.

> [!NOTE]
> Eine `Polygon` ist eine vollständig eingeschlossene Form. Der erste und der letzte Punkt werden automatisch verbunden, wenn keine Entsprechung gefunden wird.

## <a name="create-a-polyline"></a>Erstellen einer Polylinie

Ein `Polyline`-Objekt kann einer Zuordnung hinzugefügt werden, indem es instanziiert und der `MapElements` Auflistung der Karte hinzugefügt wird:

```csharp
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

Eine `Polyline` kann auch in XAML erstellt werden:

```xaml
<maps:Map x:Name="map">
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
```

Die Eigenschaften `StrokeColor` und `StrokeWidth` werden angegeben, um die Zeile anzupassen. Die `GeoPath`-Eigenschaft enthält eine Liste von `Position` Objekten, die die geografischen Koordinaten der polyzeiligen Punkte definieren. Ein `Polyline`-Objekt wird in der Zuordnung gerendert, nachdem es der `MapElements` Auflistung der `Map` hinzugefügt wurde.

## <a name="related-links"></a>Verwandte Links

- [Sample Map-Projekt](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin. Forms-Karte](~/xamarin-forms/user-interface/map/index.md)
