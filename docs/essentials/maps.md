---
title: 'Xamarin.Essentials: Map'
description: Mit der Map-Klasse in Xamarin.Essentials kann eine Anwendung die installierte Kartenanwendung für einen bestimmten Standort oder eine bestimmte Ortsmarkierung öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: c0875534d88ea5b66b3072c35b9d38894fe98934
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870026"
---
# <a name="xamarinessentials-map"></a>Xamarin.Essentials: Zuordnung

Mit der **Map**-Klasse kann eine Anwendung die installierte Kartenanwendung für einen bestimmten Standort oder eine bestimmte Ortsmarkierung öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-map"></a>Verwenden von Map

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Map-Funktionalität ruft die `OpenAsync`-Methode mit `Location` oder `Placemark` zum Öffnen des optionalen `MapLaunchOptions` auf.

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(location, options);
    }
}
```

Beim Öffnen mit einem `Placemark`-Element werden die folgenden Informationen benötigt:

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapTest
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
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>Erweiterungsmethoden

Wenn Sie bereits einen Verweis auf ein `Location`- oder `Placemark`-Element haben, können Sie die integrierte Erweiterungsmethode `OpenMapAsync` mit dem optionalen `MapLaunchOptions`-Element verwenden:

```csharp
public class MapTest
{
    public async Task OpenPlacemarkOnMap(Placemark placemark)
    {
        await placemark.OpenMapAsync();
    }
}
```

## <a name="directions-mode"></a>Modus „Wegbeschreibung“

Wenn Sie `OpenMapAsync` ohne `MapLaunchOptions` aufrufen, startet die Karte an der angegebenen Position. Optional können Sie eine Navigationsroute ausgehend von der aktuellen Position des Geräts berechnen lassen. Dafür muss `NavigationMode` für `MapLaunchOptions` festgelegt werden.

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { NavigationMode = NavigationMode.Driving };

        await Map.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `NavigationMode` unterstützt Radfahren, Fahren und Gehen.

# <a name="iostabios"></a>[iOS](#tab/ios)

- `NavigationMode` unterstützt Fahren, öffentliche Verkehrsmittel und Gehen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `NavigationMode` unterstützt Fahren, öffentliche Verkehrsmittel und Gehen.

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

- [Kartenquellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Map)
- [Karten- API-Dokumentation](xref:Xamarin.Essentials.Map)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Maps-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
