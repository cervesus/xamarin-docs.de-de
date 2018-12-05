---
title: 'Xamarin.Essentials: Karten'
description: Mit der Maps-Klasse in Xamarin.Essentials kann eine Anwendung installierte Kartenanwendung für einen bestimmten Standort oder eine bestimmte Ortsmarkierung öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: fb4cbc2fd334d574abc57a3359fa346bc6795408
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674773"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: Karten

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der **Maps**-Klasse kann eine Anwendung installierte Kartenanwendung für einen bestimmten Standort oder eine bestimmte Ortsmarkierung öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-maps"></a>Verwenden von Karten

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Maps-Funktionalität ruft die `OpenAsync`-Methode mit `Location` oder `Placemark` zum Öffnen des optionalen `MapsLaunchOptions`-Elements auf.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

Beim Öffnen mit einem `Placemark`-Element werden die folgenden Informationen benötigt:

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>Erweiterungsmethoden

Wenn Sie bereits einen Verweis auf ein `Location`- oder `Placemark`-Element haben, können Sie die integrierte Erweiterungsmethode `OpenMapsAsync` mit dem optionalen `MapsLaunchOptions`-Element verwenden:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="directions-mode"></a>Modus „Wegbeschreibung“

Wenn Sie `OpenMapsAsync` ohne `MapsLaunchOptions` aufrufen, startet die Karte an der angegebenen Position. Optional können Sie eine Navigationsroute ausgehend von der aktuellen Position des Geräts berechnen lassen. Dafür muss `MapDirectionsMode` für `MapsLaunchOptions` festgelegt werden.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { MapDirectionsMode = MapDirectionsMode.Driving };

        await Maps.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `MapDirectionsMode` unterstützt Radfahren, Fahren und Gehen.

# <a name="iostabios"></a>[iOS](#tab/ios)

- `MapDirectionsMode` unterstützt Fahren, öffentliche Verkehrsmittel und Gehen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `MapDirectionsMode` unterstützt Fahren, öffentliche Verkehrsmittel und Gehen.

--------------

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android verwendet das URI-Schema `geo:`, um die Kartenanwendung auf dem Gerät zu starten. Möglicherweise wird der Benutzer aufgefordert, eine vorhandene App auszuwählen, die dieses URI-Schema unterstützt.  Xamarin.Essentials wurde mit Google Maps getestet, das dieses Schema unterstützt.

# <a name="iostabios"></a>[iOS](#tab/ios)

Keine plattformspezifischen Implementierungsangaben.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine plattformspezifischen Implementierungsangaben.

--------------

## <a name="api"></a>API

- [Maps-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Maps-API-Dokumentation](xref:Xamarin.Essentials.Maps)
