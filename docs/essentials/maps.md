---
title: Xamarin.Essentials-Zuordnungen
description: Die Maps-Klasse in Xamarin.Essentials ermöglicht eine Anwendung, die installierten kartensatz-Anwendung zu einem bestimmten Ort oder Placemark zu öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353946"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: Zuordnungen

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **ordnet** Klasse ermöglicht einer Anwendung, die installierten kartensatz-Anwendung zu einem bestimmten Ort oder Placemark zu öffnen.

## <a name="using-maps"></a>Verwenden von Zuordnungen

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität von Maps erfolgt durch Aufrufen der `OpenAsync` -Methode mit der `Location` oder `Placemark` mit optionalen öffnen `MapsLaunchOptions`.

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

Beim Öffnen von mit einer `Placemark` ist erforderlich, die folgende Informationen:

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

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

Wenn Sie noch einen Verweis auf eine `Location` oder `Placemark` können Sie die integrierten Erweiterungsmethode `OpenMapsAsync` mit optionalen `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` wird nicht unterstützt und hat keine Auswirkungen.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` wird unterstützt, um den Standardmodus für die Richtung festzulegen, wenn die Maps-Anwendung geöffnet wird.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `MapDirectionsMode` wird nicht unterstützt und hat keine Auswirkungen.

--------------

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android verwendet die `geo:` Uri-Schema, das die Maps-Anwendung auf dem Gerät zu starten. Dies kann der Benutzer aufgefordert, eine vorhandene app auswählen, die diese Uri-Schema unterstützt.  Xamarin.Essentials ist mit Google Maps getestet, die dieses Schema unterstützt.

# <a name="iostabios"></a>[iOS](#tab/ios)

Keine Plattform bestimmte Implementierungsdetails.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Plattform bestimmte Implementierungsdetails.

--------------

## <a name="api"></a>API

- [Maps-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Maps-API-Dokumentation](xref:Xamarin.Essentials.Maps)
