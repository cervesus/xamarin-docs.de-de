---
title: 'Xamarin.Essentials: Geocodierung'
description: Die Geocoding-Klasse in Xamarin.Essentials stellt APIs bereit, um sowohl eine Ortsmarkierung mit Positionskoordinaten zu geocodieren als auch Geocode-Koordinaten in eine Ortsmarkierung umzuwandeln.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/28/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 66383e441435b5f4bdb48224c9ab602f28072b3d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802338"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: Geocodierung

Die Klasse **Geocoding** stellt APIs bereit, um sowohl eine Ortsmarkierung (Placemark) mit Positionskoordinaten zu geocodieren als auch Geocode-Koordinaten in eine Ortsmarkierung umzuwandeln.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Für den Zugriff auf die **Geocodierungsfunktion** ist die folgende plattformspezifische Einrichtung erforderlich.

# <a name="android"></a>[Android](#tab/android)

Es ist kein zusätzliches Setup erforderlich.

# <a name="ios"></a>[iOS](#tab/ios)

Es ist kein zusätzliches Setup erforderlich.

# <a name="uwp"></a>[UWP](#tab/uwp)

Ein API-Schlüssel von Bing Maps ist erforderlich, um die Geocodierungsfunktion zu nutzen. Registrieren Sie sich für ein kostenloses [Bing Maps](https://www.bingmapsportal.com/)-Konto. Erstellen Sie unter **Mein Konto > Meine Schlüssel** einen neuen Schlüssel, und füllen Sie die Informationen basierend auf Ihrem Anwendungstyp aus (sollte **Public Windows App (UWP, 8.x und früher)** für UWP-Apps sein).

Legen Sie früh im Lebenszyklus Ihrer Anwendung noch vor dem Aufrufen von **Geocodierungsmethoden** den API-Schlüssel fest (nur auf UWP verfügbar):

```csharp
Platform.MapServiceToken = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Verwenden der Geocodierung

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Abrufen von [Standort](xref:Xamarin.Essentials.Location)koordinaten für eine Adresse:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

Die Höhe ist nicht immer verfügbar. Wenn sie nicht verfügbar ist, ist die Eigenschaft `Altitude` möglicherweise `null`, oder der Wert ist 0 (null). Ist die Höhe verfügbar, ist der Wert in Metern über Normalhöhennull angegeben.

## <a name="using-reverse-geocoding"></a>Verwenden von Reverse-Geocodierung

Bei der Reverse-Geocodierung werden [Placemarks](xref:Xamarin.Essentials.Placemark) (Ortsmarkierungen) für einen vorhandenen Satz von Koordinaten abgerufen:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>Abstand zwischen zwei Standorten

Die Klassen [`Location`](xref:Xamarin.Essentials.Location) und [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) definieren Methoden, mit denen Sie den Abstand zwischen zwei Standorten berechnen können. Ein Beispiel finden Sie im Artikel [ **Xamarin.Essentials: GeoLocation**](geolocation.md#calculate-distance).

## <a name="api"></a>API

- [Geocoding-Quellcode](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Geocoding)
- [Geocoding-API-Dokumentation](xref:Xamarin.Essentials.Geocoding)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Geocoding-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
