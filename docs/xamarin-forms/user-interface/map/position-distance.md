---
title: Xamarin. Forms-Zuordnungs Position und-Entfernung
description: Der xamarin. Forms. Maps-Namespace enthält eine Positions Struktur, die in der Regel verwendet wird, wenn eine Karte und Ihre Pins positioniert werden, sowie eine Entfernungs Struktur, die optional beim Positionieren einer Karte verwendet werden kann.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
ms.openlocfilehash: a68eab7bb3e6da764f5a35af4461d6af2be50ebe
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426282"
---
# <a name="xamarinforms-map-position-and-distance"></a>Xamarin. Forms-Zuordnungs Position und-Entfernung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Der [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) -Namespace enthält eine [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur, die in der Regel verwendet wird, wenn eine Karte und Ihre Pins positioniert werden, sowie eine [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur, die optional beim Positionieren einer Karte verwendet werden kann.

## <a name="position"></a>Position

Die [`Position`](xref:Xamarin.Forms.Maps.Position) -Struktur kapselt eine Position, die als Längen-und Längengrad Werte gespeichert wird. Diese Struktur definiert zwei schreibgeschützte Eigenschaften:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)vom Typ `double`, der den Breitengrad der Position in Dezimalgraden darstellt.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)vom Typ `double`, der den Längengrad der Position in Dezimalgrad darstellt.

[`Position`](xref:Xamarin.Forms.Maps.Position) Objekte werden mit dem `Position`-Konstruktor erstellt, der breiten-und Längengrad Argumente erfordert, die als `double` Werte angegeben werden:

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

> [!NOTE]
> Beim Erstellen eines `Position` Objekts wird der Breitengrad zwischen-90,0 und 90,0 und der Längen Wert zwischen-180,0 und 180,0 geklammert.

## <a name="distance"></a>Flüge

Die [`Distance`](xref:Xamarin.Forms.Maps.Distance) -Struktur kapselt einen als `double` Wert gespeicherten Abstand, der den Abstand in Meter darstellt. Diese Struktur definiert drei schreibgeschützte Eigenschaften:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)vom Typ `double`, der den Abstand in Kilometern darstellt, der vom `Distance`überspannt wird.
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)vom Typ `double`, der den Abstand in Meter darstellt, der vom `Distance`überspannt wird.
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)vom Typ `double`, der den Abstand in Meilen darstellt, der vom `Distance`überspannt wird.

[`Distance`](xref:Xamarin.Forms.Maps.Distance) Objekte können mit dem `Distance`-Konstruktor erstellt werden, für den ein "Meter"-Argument erforderlich ist, das als `double`angegeben ist:

```csharp
Distance distance = new Distance(1450.5);
```

Alternativ können [`Distance`](xref:Xamarin.Forms.Maps.Distance) Objekte mit den Methoden [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*), [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*)und [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) Factory erstellt werden:

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
```

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
