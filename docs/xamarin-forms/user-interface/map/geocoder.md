---
title: Xamarin. Forms-Karten Geocodierung
description: In diesem Artikel wird erläutert, wie Sie Geocode Map-Daten mithilfe der xamarin. Forms. Maps Geocoder-Klasse Geocodieren und umkehren.
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
ms.openlocfilehash: 9a20618fea0091979c2ea862f417dccec565b218
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425603"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin. Forms-Karten Geocodierung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Der [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) -Namespace stellt eine [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) Klasse bereit, die zwischen Zeichen folgen Adressen und breiten-und Längenkoordinaten konvertiert, die in [`Position`](xref:Xamarin.Forms.Maps.Position) Objekten gespeichert werden. Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

## <a name="geocode-an-address"></a>Eine Adresse Geocodieren

Eine Straße kann in breiten-und Längenkoordinaten geocodiert werden, indem eine [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) Instanz erstellt und die [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) -Methode für die `Geocoder` Instanz aufgerufen wird:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

Die [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) -Methode nimmt ein `string` Argument an, das die Adresse darstellt, und gibt asynchron eine Auflistung von [`Position`](xref:Xamarin.Forms.Maps.Position) Objekten zurück, die die Adresse darstellen könnten.

## <a name="reverse-geocode-an-address"></a>Umgekehrte Geocodierung einer Adresse

Breiten-und Längenkoordinaten können durch das Erstellen einer [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Instanz und Aufrufen der [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) -Methode für die `Geocoder` Instanz in eine Straße zurückgeschrieben werden:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

Die [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) -Methode nimmt ein [`Position`](xref:Xamarin.Forms.Maps.Position) Argument an, das aus breiten-und Längenkoordinaten besteht, und gibt asynchron eine Auflistung von Zeichen folgen zurück, die die Adressen in der Nähe der Position darstellen.

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin. Forms-Zuordnungs Position und-Entfernung](position-distance.md)
- [Geocoder-API](xref:Xamarin.Forms.Maps.Geocoder)
