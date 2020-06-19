---
title: Xamarin.FormsKarten Position und-Entfernung
description: Die Xamarin.Forms . Der Maps-Namespace enthält eine Positions Struktur, die in der Regel verwendet wird, wenn eine Karte und Ihre Pins positioniert werden, sowie eine Entfernungs Struktur, die optional beim Positionieren einer Karte verwendet werden kann.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2b1613789029d59e46a6d0431bfa9da1a53082e8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138397"
---
# <a name="xamarinforms-map-position-and-distance"></a>Xamarin.FormsKarten Position und-Entfernung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Der [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) -Namespace enthält eine [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur, die in der Regel verwendet wird, wenn eine Karte und Ihre Pins positioniert werden, und eine [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur, die optional beim Positionieren einer Karte verwendet werden kann.

## <a name="position"></a>Position

Die [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur kapselt eine Position, die als Längen-und Längengrad Werte gespeichert wird. Diese Struktur definiert zwei schreibgeschützte Eigenschaften:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)vom Typ `double` , der den Breitengrad der Position in Dezimalgraden darstellt.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)vom Typ `double` , der den Längengrad der Position in Dezimalgrad darstellt.

[`Position`](xref:Xamarin.Forms.Maps.Position)-Objekte werden mit dem- `Position` Konstruktor erstellt, der breiten-und Längengrad Argumente erfordert, die als Werte angegeben werden `double` :

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

Beim Erstellen eines `Position` -Objekts wird der Breitengrad zwischen-90,0 und 90,0 geklammert, und der Längen Wert wird zwischen-180,0 und 180,0 geklammert.

> [!NOTE]
> Die `GeographyUtils` -Klasse verfügt über eine `ToRadians` -Erweiterungsmethode, die einen `double` Wert von Grad in Bogenmaß konvertiert, und eine `ToDegrees` Erweiterungsmethode, die einen `double` Wert von Bogenmaß in Grad konvertiert.

## <a name="distance"></a>Distance

Die [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur kapselt einen als Wert gespeicherten Abstand `double` , der den Abstand in Meter darstellt. Diese Struktur definiert drei schreibgeschützte Eigenschaften:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)vom Typ `double` , der den Abstand in Kilometern darstellt, die vom überspannt werden `Distance` .
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)vom Typ `double` , der den Abstand in Meter darstellt, der vom überspannt wird `Distance` .
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)vom Typ `double` , der den Abstand in Meilen darstellt, der von der überspannt wird `Distance` .

[`Distance`](xref:Xamarin.Forms.Maps.Distance)-Objekte können mit dem- `Distance` Konstruktor erstellt werden, für den ein Meter Argument erforderlich ist, das als angegeben ist `double` :

```csharp
Distance distance = new Distance(1450.5);
```

Alternativ [`Distance`](xref:Xamarin.Forms.Maps.Distance) können-Objekte mit den [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*) [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*) Methoden,, [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) und `BetweenPositions` erstellt werden:

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
