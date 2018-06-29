---
title: 'Xamarin.Essentials: Geolocation'
description: Dieses Dokument beschreibt die Geolocation-Klasse in Xamarin.Essentials, die APIs zum Abrufen der aktuellen Geolocation-Koordinaten des Geräts bereitstellt.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080286"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: Geolocation

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Geolocation** Klasse bietet APIs zum Abrufen der aktuellen Geolocation-Koordinaten des Geräts.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Geolocation** Funktionalität, die folgenden plattformspezifischen Setup erforderlich ist:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Grob und gut Speicherort Berechtigungen sind erforderlich und müssen in der Android-Projekt konfiguriert werden. Darüber hinaus verwendet, wenn die app Android 5.0.x (API-Ebene 21 abzielt) oder höher verwenden, Sie deklarieren, die müssen Ihre app Hardware-Features in der Manifestdatei. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **AssemblyInfo.cs** Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Oder aktualisieren Sie die Android-Manifest:

Öffnen der **AndroidManifest.xml** Datei unter dem **Eigenschaften** Ordner und fügen Sie die folgenden innerhalb eines der **manifest** Knoten:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Oder mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** Suchen der **die erforderlichen Berechtigungen verfügen:** Bereichs- und überprüfen Sie die **ACCESS_COARSE_LOCATION** und **ACCESS_FINE_LOCATION**Berechtigungen. Dadurch wird automatisch aktualisiert die **AndroidManifest.xml** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ihre app **"Info.plist"** darf die `NSLocationWhenInUseUsageDescription` Schlüssel, um den Standort eines Geräts zuzugreifen.

Die Plist-Editor öffnen, und fügen die **Datenschutz - Speicherort bei In Verwendung Verwendungsbeschreibung** Eigenschaft und füllen Sie einen Wert aus, um den Benutzer anzuzeigen.

Oder bearbeiten Sie die Datei manuell, und fügen Sie Folgendes hinzu:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Sie müssen festlegen, die `Location` über die Berechtigung für die Anwendung. Dies kann geschehen, indem Sie öffnen die **"Package.appxmanifest"** und Selecing der **Funktionen** Registerkarte und Überprüfung **Speicherort**.

-----

## <a name="using-geolocation"></a>Verwenden die Geolocation

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Geoloation-API wird auch der Benutzer Berechtigungen bei Bedarf aufgefordert.

Sie können die letzten bekannten abrufen [Speicherort](xref:Xamarin.Essentials.Location) des Geräts durch Aufrufen der `GetLastKnownLocationAsync` Methode. Dies ist oft schneller klicken Sie dann auf diese Weise einer vollständigen Abfrage, aber ungenauer sein kann.

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

Die Höhe ist nicht immer verfügbar. Wenn sie nicht verfügbar ist, ist die `Altitude` Eigenschaft möglicherweise `null` oder der Wert kann 0 (null) sein. Wenn die Höhe verfügbar ist, ist der Wert in Metern über über Sea-Ebene. 

Des aktuellen Geräts Abfragen [Speicherort](xref:Xamarin.Essentials.Location) Koordinaten, die `GetLocationAsync` verwendet werden können. Es wird empfohlen, eine vollständige übergeben `GeolocationRequest` und `CancellationToken` seit möglicherweise einige Zeit dauert, um den Standort eines Geräts abzurufen.

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

In der folgenden Tabelle sind die Genauigkeit pro Plattform aufgeführt:

### <a name="lowest"></a>Niedrigste

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 - 5000 |

### <a name="low"></a>Niedrig

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 - 3000 |

### <a name="medium-default"></a>Mittel (Standard)

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>Hoch

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>Am besten

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Abstand zwischen zwei Standorten

Die [ `Location` ](xref:Xamarin.Essentials.Location) und [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) Klassen definieren `CalculateDistance` Methoden, die Ihnen ermöglichen, den Abstand zwischen zwei geografisch getrennten Standorten zu berechnen. Dieser Abstand wird nicht berücksichtigt Straßen oder andere Pfade zur Verfügung und wird lediglich die kürzeste Entfernung zwischen den beiden Punkten entlang der Oberfläche der Erde dar, auch bezeichnet als berechnet die _gute Kreis Abstand_ oder gelegentlich, die Abstand "als Code-Crow."

Im Folgenden ein Beispiel:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

Die `Location` Konstruktor Breiten- und Längengrad Argumente in dieser Reihenfolge aufweist. Positive Breitengradwerte sind nördlich vom Äquator und Werte einen vom Nullmeridian positive Längengrad. Verwenden Sie das letzte Argument für `CalculateDistance` Meilen oder Kilometer angeben. Die `Location` -Klasse definiert außerdem `KilometersToMiles` und `MilesToKilometers` Methoden für die Konvertierung zwischen den zwei Einheiten.

## <a name="api"></a>API

- [GeoLocation-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [GeoLocation-API-Dokumentation](xref:Xamarin.Essentials.Geolocation)
