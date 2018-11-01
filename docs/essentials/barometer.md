---
title: 'Xamarin.Essentials: Barometer'
description: Mit der Barometer-Klasse in Xamarin.Essentials können Sie den Barometersensor des Geräts überwachen, der den Druck misst.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 08/16/2018
ms.openlocfilehash: 0217b89bc3f45347acecbdb3b8cdccfeed751744
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50130297"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: Barometer

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der Klasse **Barometer** können Sie den Barometersensor des Geräts überwachen, der den Druck misst.

## <a name="using-barometer"></a>Verwenden des Barometers

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Barometer-Funktionalität ruft die Methoden `Start` und `Stop` auf, um auf die Änderungen der Barometerdruck-Messwerte in Kilopascal zu lauschen. Änderungen werden über das `ReadingChanged`-Ereignis zurück gesendet. Sie können sie z.B. wie folgt verwenden:

```csharp

public class BarometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public BarometerTest()
    {
        // Register for reading changes.
        Barometer.ReadingChanged += Barometer_ReadingChanged;
    }

    void Barometer_ReadingChanged(object sender, BarometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Pressure
        Console.WriteLine($"Reading: Pressure: {data.Pressure} kilopascals");
    }

    public void ToggleBarometer()
    {
        try
        {
            if (Barometer.IsMonitoring)
              Barometer.Stop();
            else
              Barometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der Plattformimplementierung

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine plattformspezifischen Implementierungsangaben.

# <a name="iostabios"></a>[iOS](#tab/ios)

Diese API verwendet [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) zum Überwachen von Druckänderungen. Diese Hardwarefunktion wurde iPhone 6 und neueren Geräten hinzugefügt. `FeatureNotSupportedException` wird auf Geräten ausgelöst, die Altimeter nicht unterstützen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine plattformspezifischen Implementierungsangaben.

-----

## <a name="api"></a>API

- [Barometer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Barometer-API-Dokumentation](xref:Xamarin.Essentials.Barometer)
