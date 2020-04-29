---
title: Xamarin. Forms-Zuordnungs Position und-Entfernung
description: Der xamarin. Forms. Maps-Namespace enthält eine Positions Struktur, die in der Regel verwendet wird, wenn eine Karte und Ihre Pins positioniert werden, sowie eine Entfernungs Struktur, die optional beim Positionieren einer Karte verwendet werden kann.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: 567e1b5620378f0c1b64d17c56c8fb451f18abc3
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517446"
---
# <a name="xamarinforms-map-position-and-distance"></a>Xamarin. Forms-Zuordnungs Position und-Entfernung

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Der [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) -Namespace enthält [`Position`](xref:Xamarin.Forms.Maps.Position) eine Struktur, die in der Regel verwendet wird, wenn eine Karte und Ihre [`Distance`](xref:Xamarin.Forms.Maps.Distance) Pins positioniert werden, und eine Struktur, die optional beim Positionieren einer Karte verwendet werden kann.

## <a name="position"></a>Position

Die [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur kapselt eine Position, die als Längen-und Längengrad Werte gespeichert wird. Diese Struktur definiert zwei schreibgeschützte Eigenschaften:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)vom Typ `double`, der den Breitengrad der Position in Dezimalgraden darstellt.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)vom Typ `double`, der den Längengrad der Position in Dezimalgrad darstellt.

[`Position`](xref:Xamarin.Forms.Maps.Position)-Objekte werden mit dem `Position` -Konstruktor erstellt, der breiten-und Längengrad Argumente `double` erfordert, die als Werte angegeben werden:

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

Beim Erstellen eines `Position` -Objekts wird der Breitengrad zwischen-90,0 und 90,0 geklammert, und der Längen Wert wird zwischen-180,0 und 180,0 geklammert.

> [!NOTE]
> Die `GeographyUtils` -Klasse verfügt `ToRadians` über eine-Erweiterungsmethode `double` , die einen Wert von Grad in Bogenmaß `ToDegrees` konvertiert, und eine Erweiterungs `double` Methode, die einen Wert von Bogenmaß in Grad konvertiert.

## <a name="distance"></a>Distance

Die [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur kapselt einen als `double` Wert gespeicherten Abstand, der den Abstand in Meter darstellt. Diese Struktur definiert drei schreibgeschützte Eigenschaften:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)vom Typ `double`, der den Abstand in Kilometern darstellt, die vom überspannt werden `Distance`.
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)vom Typ `double`, der den Abstand in Meter darstellt, der `Distance`vom überspannt wird.
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)vom Typ `double`, der den Abstand in Meilen darstellt, der von der `Distance`überspannt wird.

[`Distance`](xref:Xamarin.Forms.Maps.Distance)-Objekte können mit dem `Distance` -Konstruktor erstellt werden, für den ein Meter Argument erforderlich `double`ist, das als angegeben ist:

```csharp
Distance distance = new Distance(1450.5);
```

Alternativ können [`Distance`](xref:Xamarin.Forms.Maps.Distance) - [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*)Objekte mit den Methoden [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*),, [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*)und `BetweenPositions` erstellt werden:

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
