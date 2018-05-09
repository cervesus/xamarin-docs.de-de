---
title: Xamarin.Essentials Geolocation
description: Die Geolocation-Klasse bietet APIs zum Abrufen der aktuellen Geolocation-Koordinaten des Geräts.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 51e63245cd6959f650a0aa078cc4632bc825de5b
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials Geocodierung

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Geolocation** Klasse bietet APIs zum Abrufen der aktuellen Geolocation-Koordinaten des Geräts.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Geolocation** Funktionen die folgenden spezifischen Plattform-Setup ist erforderlich.

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

ODER Android-Manifest aktualisieren:

Öffnen der **AndroidManifest.xml** Datei unter dem **Eigenschaften** Ordner und fügen Sie die folgenden innerhalb eines der **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** Suchen der **die erforderlichen Berechtigungen verfügen:** Bereichs- und überprüfen Sie die **ACCESS_COARSE_LOCATION** und **ACCESS_FINE_LOCATION**Berechtigungen. Dadurch wird automatisch aktualisiert die **AndroidManifest.xml** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ihre app ist erforderlich, um den Schlüssel verfügen über eine Ihrer **"Info.plist"** für NSLocationWhenInUseUsageDescription Zugriff auf den Standort eines Geräts.

Die Plist-Editor öffnen, und fügen die **Datenschutz - Speicherort bei In Verwendung Verwendungsbeschreibung** Eigenschaft und füllen Sie einen Wert aus, um den Benutzer anzuzeigen.

ODER bearbeiten Sie die Datei manuell, und fügen Sie Folgendes hinzu:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Sie müssen festlegen, die `Location` über die Berechtigung für die Anwendung. Dies kann geschehen, indem öffnen die **"Package.appxmanifest"** und Selecing der **Funktionen** Registerkarte und Überprüfung **Speicherort**.

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
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Des aktuellen Geräts Abfragen [Speicherort](xref:Xamarin.Essentials.Location) Koordinaten der `GetLocationAsync` verwendet werden können. Es wird empfohlen, eine vollständige übergeben `GeolocationRequest` und `CancellationToken` seit möglicherweise einige Zeit dauert, um den Standort eines Geräts abzurufen.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Die folgende Tabelle führt die Genauigkeit plattformspezifischen

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

## <a name="api"></a>API

- [GeoLocation-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/Geolocation)
- [GeoLocation-API-Dokumentation](xref:Xamarin.Essentials.Geolocation)
