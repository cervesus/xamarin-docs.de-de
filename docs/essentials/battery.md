---
title: 'Xamarin.Essentials: Akku'
description: Dieses Dokument beschreibt die Batterie-Klasse in Xamarin.Essentials, dem Sie die Informationen des Geräts Akku und überwachen Sie Änderungen überprüfen können.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6b87625b3305d0a9ec40593d8b3fe29eb551bbf4
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514309"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: Akku

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Akku** Klasse können Sie Akkuinformationen und überwachen Sie Änderungen des Geräts zu überprüfen.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Akku** Funktionalität die folgende Plattform Einrichtung erforderlich ist.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die `Battery` -Berechtigung ist erforderlich und muss in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **"AssemblyInfo.cs"** -Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **"androidmanifest.xml"** -Datei unter dem **Eigenschaften** Ordner und fügen Sie Folgendes in der die **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** finden Sie die **erforderliche Berechtigungen:** Bereichs- und überprüfen Sie die **Akku** Berechtigung. Dadurch wird automatisch aktualisiert. die **"androidmanifest.xml"** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-battery"></a>Mit einem Akku betrieben

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Überprüfen Sie die aktuellen Akkuinformationen:

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

Ein Ereignis wird ausgelöst, wenn der Akku Eigenschaften ändern:

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(object sender, BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine Plattformunterschiede.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Gerät muss verwendet werden, um APIs zu testen. 
* Gibt nur `AC` oder `Battery` für `PowerSource`.
* Nicht möglich, Vibration abzubrechen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Gibt nur `AC` oder `Battery` für `PowerSource`.

-----

## <a name="api"></a>API

- [Akku-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Akku-API-Dokumentation](xref:Xamarin.Essentials.Battery)
