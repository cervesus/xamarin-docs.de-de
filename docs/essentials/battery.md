---
title: 'Xamarin.Essentials: Akku'
description: In diesem Dokument wird die Klasse „Battery“ in Xamarin.Essentials beschrieben, mit der Sie die Akkuinformationen des Geräts überprüfen und auf Änderungen überwachen können.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 01/22/2019
ms.custom: video
ms.openlocfilehash: d5408894a9eda6b782f1f790ed8f1d0bb138a2f3
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120158"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: Akku

Mit der **Battery**-Klasse können Sie Informationen über den Akkustand und über Änderungen des Geräts sowie den Energiesparstatus des Geräts überwachen. Dieser gibt an, ob sich das Gerät im Energiesparmodus befindet. Anwendungen sollten Hintergrundverarbeitung vermeiden, wenn der Energiesparmodus des Geräts aktiviert ist.

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

Alternativ können Sie mit der rechten Maustaste auf das Android-Projekt klicken und die Eigenschaften des Projekts öffnen. Suchen Sie unter **Android-Manifest** den Bereich **Erforderliche Berechtigungen:** , und aktivieren Sie die Berechtigung **Battery** (Akku). Dadurch wird die Datei **AndroidManifest.xml** automatisch aktualisiert.

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
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or 1.0 when on AC or no battery.

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
        Battery.BatteryInfoChanged += Battery_BatteryInfoChanged;
    }

    void Battery_BatteryInfoChanged(object sender, BatteryInfoChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

Für Geräte, die mit Akku betrieben werden, lässt sich der Energiesparmodus aktivieren. Manchmal werden Geräte automatisch in diesen Modus umgeschaltet, z.B. wenn der Akku unter 20 % fällt. Das Betriebssystem reagiert auf den Energiesparmodus, indem Aktivitäten reduziert werden, die den Akku stark beanspruchen. Anwendungen können unterstützend wirken, indem sie Hintergrundverarbeitung oder andere Hochleistungsaktivitäten im Energiesparmodus vermeiden.

Sie können auch den aktuellen Energiesparstatus des Geräts über die statische Eigenschaft `Battery.EnergySaverStatus` ermitteln:

```csharp
// Get energy saver status
var status = Battery.EnergySaverStatus;
```

Diese Eigenschaft gibt ein Mitglied der `EnergySaverStatus`-Enumeration zurück: `On`, `Off` oder `Unknown`. Wenn die Eigenschaft `On` zurückgibt, sollte die Anwendung Hintergrundverarbeitung oder andere Aktivitäten vermeiden, die ggf. viel Energie verbrauchen.

Außerdem sollte die Anwendung einen Ereignishandler installieren. Die **Battery**-Klasse zeigt ein Ereignis an, das ausgelöst wird, wenn sich der Energiesparstatus ändert:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Battery.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Wenn sich der Energiesparstatus auf `On` ändert, sollte die Anwendung die Hintergrundverarbeitung deaktivieren. Wenn der Status auf `Unknown` oder `Off` wechselt, kann die Anwendung die Hintergrundverarbeitung wieder aufnehmen.


## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine Plattformunterschiede.

# <a name="iostabios"></a>[iOS](#tab/ios)

- Gerät muss zum Testen von APIs verwendet werden. 
- Gibt nur `AC` oder `Battery` für `PowerSource` zurück.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- Gibt nur `AC` oder `Battery` für `PowerSource` zurück.

-----

## <a name="api"></a>API

- [Battery-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Battery-API-Dokumentation](xref:Xamarin.Essentials.Battery)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Battery-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
