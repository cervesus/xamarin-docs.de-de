---
title: Xamarin.Essentials Geocodierung
description: Die Geocodierung-Klasse bietet APIs zu "Geocode" eine Placemark in eine mit Feldern fester Breite Koordinaten und "Geocode" Coordincates zu einem Placemark umkehren.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1b9bb3232c5b9eacde5ca90c9decf02138e3069a
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials Geocodierung

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Geocodierung** Klasse bietet APIs zu "Geocode" eine Placemark in eine mit Feldern fester Breite Koordinaten und "Geocode" Coordincates zu einem Placemark umkehren.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Geocodierung** Funktionen die folgenden spezifischen Plattform-Setup ist erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Ein Bing Maps-API-Schlüssel ist erforderlich, Geocoding Funcationality verwenden. Registrieren Sie sich für eine kostenlose [Bing Maps](https://www.bingmapsportal.com/) Konto. Klicken Sie unter **Mein Konto > Meine Schlüssel** einen neuen Schlüssel erstellen und füllen Sie Informationen basierend auf Ihren Anwendungstyp.

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
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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
    // Handle exception that may have occured in geocoding
}
```

## <a name="api"></a>API

- [Geocodierung-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/Geocoding)
- [Geocodierung API-Dokumentation](xref:Xamarin.Essentials.Geocoding)
