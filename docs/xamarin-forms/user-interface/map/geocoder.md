---
title: Xamarin.FormsKarten Geocodierung
description: In diesem Artikel wird erläutert, wie Sie mithilfe von die Geocodierung von Geocodierungsdaten und die rückgängig-Zuordnung Xamarin.Forms Maps Geocoder-Klasse.
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fe099235857f6bd0531539e3aa84e41bf59b50ba
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139866"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin.FormsKarten Geocodierung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Der- [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) Namespace stellt eine- [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) Klasse bereit, die zwischen Zeichen folgen Adressen und breiten-und Längenkoordinaten konvertiert, die in-Objekten gespeichert werden [`Position`](xref:Xamarin.Forms.Maps.Position) . Weitere Informationen zur [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur finden Sie unter [map Position und Distance](position-distance.md).

## <a name="geocode-an-address"></a>Eine Adresse Geocodieren

Eine Straßenadresse kann in breiten-und Längenkoordinaten geocodiert werden, indem eine [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Instanz erstellt und die- [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) Methode für die-Instanz aufgerufen wird `Geocoder` :

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

Die [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) -Methode nimmt ein `string` -Argument an, das die Adresse darstellt, und gibt asynchron eine Auflistung von- [`Position`](xref:Xamarin.Forms.Maps.Position) Objekten zurück, die die Adresse darstellen könnten.

## <a name="reverse-geocode-an-address"></a>Umgekehrte Geocodierung einer Adresse

Breiten-und Längenkoordinaten können durch das Erstellen einer [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Instanz und das Aufrufen der- [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) Methode für die-Instanz in eine Straße zurückgeschrieben werden `Geocoder` :

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

Die [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) -Methode nimmt ein [`Position`](xref:Xamarin.Forms.Maps.Position) Argument an, das aus breiten-und Längenkoordinaten besteht, und gibt asynchron eine Auflistung von Zeichen folgen zurück, die die Adressen in der Nähe der Position darstellen.

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.FormsKarten Position und-Entfernung](position-distance.md)
- [Geocoder-API](xref:Xamarin.Forms.Maps.Geocoder)
