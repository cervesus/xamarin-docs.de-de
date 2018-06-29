---
title: 'Xamarin.Essentials: Geocodierung'
description: Die Geocodierung-Klasse in Xamarin.Essentials bietet APIs zu beiden "Geocode" eine Placemark in eine mit Feldern fester Breite Koordinaten und umgekehrt "Geocode" Koordinaten auf einer Placemark.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 063adba82d96e7fcc64d7ec49a0c0133e1cef8ef
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080325"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: Geocodierung

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Geocodierung** Klasse bietet APIs zu "Geocode" eine Placemark in eine mit Feldern fester Breite Koordinaten und "Geocode" Coordincates zu einem Placemark umkehren.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Geocodierung** Funktionen die folgenden spezifischen Plattform-Setup ist erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Ein Bing Maps-API-Schlüssel ist erforderlich, Geocoding Funcationality verwenden. Registrieren Sie sich für eine kostenlose [Bing Maps](https://www.bingmapsportal.com/) Konto. Klicken Sie unter **Mein Konto > Meine Schlüssel** einen neuen Schlüssel erstellen und füllen Sie Informationen basierend auf Ihren Anwendungstyp (sein sollten **öffentlichen Windows-App (universelle Windows-Plattform, 8.x und früher)** für uwp-apps).

In einer frühen Phase Ihrer Anwendung zu überdenken, bevor das Aufrufen einer **Geocodierung** Methoden legen Sie den API-Schlüssel:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Verwenden von Geocodierung

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Erste [Speicherort](xref:Xamarin.Essentials.Location) Koordinaten für eine Adresse:

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
    // Handle exception that may have occured in geocoding
}
```

Die Höhe ist nicht immer verfügbar. Wenn sie nicht verfügbar ist, ist die `Altitude` Eigenschaft möglicherweise `null` oder der Wert kann 0 (null) sein. Wenn die Höhe verfügbar ist, ist der Wert in Metern über über Sea-Ebene. 

Erste [Placemarks](xref:Xamarin.Essentials.Placemark) für einen vorhandenen Satz von Koordinaten:

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

Die [ `Location` ](xref:Xamarin.Essentials.Location) und [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) Klassen definieren Methoden, um den Abstand zwischen zwei Standorten zu berechnen. Finden Sie im Artikel [ **Xamarin.Essentials: Geolocation** ](geolocation.md#calculate-distance) ein Beispiel.

## <a name="api"></a>API

- [Geocodierung-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Geocodierung API-Dokumentation](xref:Xamarin.Essentials.Geocoding)
