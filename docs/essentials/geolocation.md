---
title: 'Xamarin.Essentials: Geolocation'
description: Dieses Dokument beschreibt die Geolocation-Klasse in Xamarin.Essentials, die APIs zum Abrufen der aktuellen Geolocation-Koordinaten des Geräts bereitstellt.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848750"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: Geolocation

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Geolocation** Klasse stellt APIs zum Abrufen der aktuellen Geolocation-Koordinaten des Geräts bereit.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Geolocation** Funktionalität, die die folgende plattformspezifischen-Einrichtung ist erforderlich:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Grob und Speicherort mit ausreichend Berechtigungen sind erforderlich und müssen in der Android-Projekt konfiguriert werden. Darüber hinaus verwendet, wenn Ihre app auf Android 5.0 (API Level 21) ausgerichtet ist, oder höher verwenden, Sie deklarieren, die müssen Ihre app Hardware-Features in der Manifestdatei. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **"AssemblyInfo.cs"** -Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Oder Aktualisieren des Android-Manifests:

Öffnen der **"androidmanifest.xml"** -Datei unter dem **Eigenschaften** Ordner und fügen Sie Folgendes in der die **manifest** Knoten:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Oder mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** finden Sie die **erforderliche Berechtigungen:** Bereichs- und überprüfen Sie die **ACCESS_COARSE_LOCATION** und **ACCESS_FINE_LOCATION**Berechtigungen. Dadurch wird automatisch aktualisiert. die **"androidmanifest.xml"** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ihrer app **"Info.plist"** darf die `NSLocationWhenInUseUsageDescription` Schlüssel, um auf den Gerätestandort zuzugreifen.

Öffnen Sie die Plist-Editor, und fügen die **Datenschutz – Location beim In Use Usage Description** -Eigenschaft, und geben Sie einen Wert aus, um den Benutzer anzuzeigen.

Oder bearbeiten Sie die Datei manuell, und fügen Sie Folgendes hinzu:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Sie müssen festlegen, die `Location` über die Berechtigung für die Anwendung. Dies ist möglich durch Öffnen der **"Package.appxmanifest"** und Selecing der **Funktionen** Registerkarte und Überprüfung **Speicherort**.

-----

## <a name="using-geolocation"></a>Verwenden von Geolocation

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Geoloation API fordert den Benutzer für Berechtigungen, die bei Bedarf auch.

Sie erhalten den letzten bekannten [Speicherort](xref:Xamarin.Essentials.Location) des Geräts durch Aufrufen der `GetLastKnownLocationAsync` Methode. Dies ist häufig dann einmal eine vollständige Abfrage schneller, aber ungenauer sein kann.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

Die Höhe nicht immer verfügbar ist. Wenn sie nicht verfügbar ist, die `Altitude` Eigenschaft möglicherweise `null` oder der Wert 0 (null) sein. Wenn die Höhe verfügbar ist, ist der Wert, in Metern über über Sea-Ebene. 

Des aktuellen Geräts Abfragen [Speicherort](xref:Xamarin.Essentials.Location) Koordinaten, die `GetLocationAsync` kann verwendet werden. Es wird empfohlen, eine vollständige übergeben `GeolocationRequest` und `CancellationToken` da möglicherweise einige Zeit dauert, um den Standort eines Geräts zu erhalten.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>GeoLocation-Genauigkeit

In der folgende Tabelle sind die Genauigkeit pro Plattform aufgeführt:

### <a name="lowest"></a>Niedrigste

| Plattform | Abstand (in Meter) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000: 5000 |

### <a name="low"></a>Niedrig

| Plattform | Abstand (in Meter) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 - 3000 |

### <a name="medium-default"></a>Mittel (Standard)

| Plattform | Abstand (in Meter) |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30 bis 500 |

### <a name="high"></a>Hoch

| Plattform | Abstand (in Meter) |
| --- | --- |
| Android | 0 – 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>Am besten

| Plattform | Abstand (in Meter) |
| --- | --- |
| Android | 0 – 100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Abstand zwischen zwei Standorten

Die [ `Location` ](xref:Xamarin.Essentials.Location) und [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) Klassen definieren `CalculateDistance` Methoden, die Ihnen ermöglichen, der Berechnung der Entfernung zwischen zwei geografisch getrennten Standorten. Die berechnete Abstand nimmt keine Straßen oder andere Pfade berücksichtigt und lediglich die kürzeste Entfernung zwischen den beiden Punkten auf der Oberfläche der Erde, auch bekannt als ist die _großkreisentfernung_ oder gelegentlich, die Abstand "als die Luftlinie-Dateien."

Im Folgenden ein Beispiel:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

Die `Location` Konstruktor verfügt die Breiten- und Längengrad Argumente in dieser Reihenfolge. Positive Breitengradwerte sind nördlich vom Äquator und positive längengradwerte östlich vom Nullmeridian. Verwenden Sie das letzte Argument für `CalculateDistance` Meilen oder Kilometer angeben. Die `Location` -Klasse definiert außerdem `KilometersToMiles` und `MilesToKilometers` Methoden zum Konvertieren zwischen den zwei Einheiten.

## <a name="api"></a>API

- [GeoLocation-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [GeoLocation-API-Dokumentation](xref:Xamarin.Essentials.Geolocation)
