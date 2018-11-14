---
title: 'Xamarin.Essentials: Geocodierung'
description: Die Klasse „Geocoding“ in Xamarin.Essentials stellt APIs bereit, um sowohl eine Ortsmarkierung (Placemark) mit Positionskoordinaten zu geocodieren als auch Geocode-Koordinaten in eine Ortsmarkierung umzuwandeln.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3202d4ace85c68042425ca5f23641ca7a76a6c7e
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528389"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: Geocodierung

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **Geocoding** stellt APIs bereit, um sowohl eine Ortsmarkierung (Placemark) mit Positionskoordinaten zu geocodieren als auch Geocode-Koordinaten in eine Ortsmarkierung umzuwandeln.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Für den Zugriff auf die **Geocodierungsfunktion** ist die folgende plattformspezifische Einrichtung erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Es ist kein zusätzliches Setup erforderlich.

# <a name="iostabios"></a>[iOS](#tab/ios)

Es ist kein zusätzliches Setup erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ein API-Schlüssel von Bing Maps ist erforderlich, um die Geocodierungsfunktion zu nutzen. Registrieren Sie sich für ein kostenloses [Bing Maps](https://www.bingmapsportal.com/)-Konto. Erstellen Sie unter **Mein Konto > Meine Schlüssel** einen neuen Schlüssel, und füllen Sie die Informationen basierend auf Ihrem Anwendungstyp aus (sollte **Public Windows App (UWP, 8.x und früher)** für UWP-Apps sein).

Legen Sie früh im Leben Ihrer Anwendung – noch vor dem Aufrufen von **Geocodierungs**-Methoden – den API-Schlüssel fest:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Verwenden der Geocodierung

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

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

Abrufen von [Placemarks](xref:Xamarin.Essentials.Placemark) (Ortsmarkierungen) für einen vorhandenen Koordinatensatz:

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

Die Klassen [`Location`](xref:Xamarin.Essentials.Location) und [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) definieren Methoden, mit denen Sie den Abstand zwischen zwei Standorten berechnen können. Im Artikel [**Xamarin.Essentials: Geolocation**](geolocation.md#calculate-distance) finden Sie ein Beispiel hierfür.

## <a name="api"></a>API

- [Geocoding-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Geocoding-API-Dokumentation](xref:Xamarin.Essentials.Geocoding)
