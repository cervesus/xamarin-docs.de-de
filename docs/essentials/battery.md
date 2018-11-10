---
title: 'Xamarin.Essentials: Akku'
description: In diesem Dokument wird die Klasse „Battery“ in Xamarin.Essentials beschrieben, mit der Sie die Akkuinformationen des Geräts überprüfen und auf Änderungen überwachen können.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6a14c939064538a405a1fe64061e0bb2e903fedd
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675431"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: Akku

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der Klasse **Battery** können Sie die Akkuinformationen des Geräts überprüfen und auf Änderungen überwachen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Für den Zugriff auf die **Akku**-Funktion ist die folgende plattformspezifische Einrichtung erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Berechtigung `Battery` ist obligatorisch und muss im Android-Projekt konfiguriert werden. Das Hinzufügen erfolgt folgendermaßen:

Öffnen Sie die Datei **AssemblyInfo.cs** im Ordner **Eigenschaften** und fügen Sie Folgendes hinzu:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

Alternativ können Sie das Android-Manifest aktualisieren:

Öffnen Sie die Datei **AndroidManifest.xml** im Ordner **Eigenschaften**, und fügen Sie Folgendes im Knoten **Manifest** hinzu.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Alternativ können Sie mit der rechten Maustaste auf das Android-Projekt klicken und die Eigenschaften des Projekts öffnen. Suchen Sie unter **Android-Manifest** den Bereich **Erforderliche Berechtigungen:**, und aktivieren Sie die Berechtigung **Battery** (Akku). Dadurch wird die Datei **AndroidManifest.xml** automatisch aktualisiert.

# <a name="iostabios"></a>[iOS](#tab/ios)

Es ist kein zusätzliches Setup erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Es ist kein zusätzliches Setup erforderlich.

-----

## <a name="using-battery"></a>Verwenden der Akku-Funktion

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Überprüfen der aktuellen Akkuinformationen:

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

Jedes Mal, wenn sich die Eigenschaften des Akkus ändern, wird ein Ereignis ausgelöst:

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

* Gerät muss zum Testen von APIs verwendet werden. 
* Gibt nur `AC` oder `Battery` für `PowerSource` zurück.
* Es ist nicht möglich, die Vibration abzubrechen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Gibt nur `AC` oder `Battery` für `PowerSource` zurück.

-----

## <a name="api"></a>API

- [Battery-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Battery-API-Dokumentation](xref:Xamarin.Essentials.Battery)
