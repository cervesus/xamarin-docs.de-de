---
title: 'Xamarin.Essentials: Geolocation'
description: In diesem Dokument wird die Klasse „Geolocation“ in Xamarin.Essentials beschrieben, die APIs zum Abrufen der Koordinaten des aktuellen geografischen Standorts des Geräts bereitstellt.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 533620fbfca9f2a7a235fe65e038b6dd89aa95a9
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674899"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: Geolocation

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **Geolocation** stellt APIs zum Abrufen der Koordinaten des aktuellen geografischen Standorts des Geräts bereit.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Der Zugriff auf die **Geolocation**-Funktionalität setzt die folgende plattformspezifische Einrichtung voraus:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Berechtigungen „Access Coarse Location“ (Standortzugriff (grob)) und „Access Fine Location“ (Standortzugriff (fein)) sind erforderlich und müssen im Android-Projekt konfiguriert werden. Außerdem müssen Sie, wenn Ihre App auf Android 5.0 (API-Ebene 21) oder höher ausgerichtet ist, deklarieren, dass Ihre App die Hardware-Features in der Manifestdatei verwendet. Das Hinzufügen erfolgt folgendermaßen:

Öffnen Sie die Datei **AssemblyInfo.cs** im Ordner **Eigenschaften** und fügen Sie Folgendes hinzu:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Alternativ können Sie das Android-Manifest aktualisieren:

Öffnen Sie die Datei **AndroidManifest.xml** im Ordner **Eigenschaften**, und fügen Sie Folgendes im Knoten **Manifest** hinzu:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Alternativ können Sie mit der rechten Maustaste auf das Android-Projekt klicken und die Eigenschaften des Projekts öffnen. Suchen Sie im **Android-Manifest** nach dem Bereich **Erforderliche Berechtigungen:**, und aktivieren Sie die Berechtigung **ACCESS_COARSE_LOCATION** (Standortzugriff (grob)) und **ACCESS_FINE_LOCATION** (Standortzugriff (fein)). Dadurch wird die Datei **AndroidManifest.xml** automatisch aktualisiert.

# <a name="iostabios"></a>[iOS](#tab/ios)

Die Datei **Info.plist** Ihrer App muss den `NSLocationWhenInUseUsageDescription`-Schlüssel enthalten, damit Sie auf den Gerätestandort zugreifen können.

Öffnen Sie den Plist-Editor, fügen Sie die Eigenschaft **Datenschutz – Beschreibung der Nutzung der Ortserkennung** hinzu, und geben Sie einen Wert ein, um den Benutzer anzuzeigen.

Oder bearbeiten Sie die Datei manuell, und fügen Sie Folgendes hinzu:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Sie müssen die Berechtigung `Location` für die Anwendung festlegen. Öffnen Sie dazu das Dokument **Package.appxmanifest**, wählen Sie die Registerkarte **Funktionen** aus, und aktivieren Sie **Standort**.

-----

## <a name="using-geolocation"></a>Verwenden der Geolocation

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Geoloation-API fordert außerdem den Benutzer bei Bedarf zur Eingabe von Berechtigungen auf.

Durch Aufrufen der `GetLastKnownLocationAsync`-Methode können Sie den letzten bekannten [Standort](xref:Xamarin.Essentials.Location) des Geräts abrufen. Dies ist häufig schneller als eine vollständige Abfrage, kann aber auch ungenauer sein.

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

Die Höhe ist nicht immer verfügbar. Wenn sie nicht verfügbar ist, ist die Eigenschaft `Altitude` möglicherweise `null`, oder der Wert ist 0 (null). Wenn die Höhe verfügbar ist, entspricht der Wert der Höhe über dem Meeresspiegel in Metern. 

Mit `GetLocationAsync` können Sie die Koordinaten des aktuellen [Standorts](xref:Xamarin.Essentials.Location) des Geräts abrufen. Es wird empfohlen, `GeolocationRequest` und `CancellationToken` vollständig zu übergeben, da es möglicherweise einige Zeit dauert, den Standort des Geräts abzurufen.

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

## <a name="geolocation-accuracy"></a>Genauigkeit der Geolocation

In der folgenden Tabelle ist die Genauigkeit bei den einzelnen Plattformen aufgeführt:

### <a name="lowest"></a>Sehr niedrig

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000-5000 |

### <a name="low"></a>Niedrig

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300-3000 |

### <a name="medium-default"></a>Mittel (Standard)

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 100-500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>Hoch

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 0-100 |
| iOS | 10 |
| UWP | <=10 |

### <a name="best"></a>Sehr hoch

| Plattform | Abstand (in Metern) |
| --- | --- |
| Android | 0-100 |
| iOS | ~0 |
| UWP | <=10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Abstand zwischen zwei Standorten

Die Klassen [`Location`](xref:Xamarin.Essentials.Location) und [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) definieren `CalculateDistance`-Methoden, mit denen Sie den Abstand zwischen zwei geografischen Standorten berechnen können. Bei der Berechnung dieses Abstands werden weder Straßen noch andere Wege berücksichtigt. Es handelt sich lediglich um die kürzeste Entfernung zwischen den beiden Punkten auf der Erdoberfläche, auch als _orthodromer Abstand_ oder Luftlinie bezeichnet.

Im Folgenden ein Beispiel:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

Der `Location`-Konstruktor verfügt über Argumente für Breiten- und Längengrad in dieser Reihenfolge. Positive Breitengradwerte befinden sich nördlich vom Äquator und positive Längengradwerte östlich vom Nullmeridian. Verwenden Sie das letzte Argument für `CalculateDistance`, um Meilen oder Kilometer anzugeben. Die Klasse `Location` definiert außerdem die Methoden `KilometersToMiles` und `MilesToKilometers` zum Konvertieren der beiden Einheiten.

## <a name="api"></a>API

- [Geolocation-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Geolacation-API-Dokumentation](xref:Xamarin.Essentials.Geolocation)
