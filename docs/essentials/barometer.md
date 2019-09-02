---
title: 'Xamarin.Essentials: Barometer'
description: Mit der Barometer-Klasse in Xamarin.Essentials können Sie den Barometersensor des Geräts überwachen, der den Druck misst.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 5a496fc06732be1cf104cfbaffa8ff4b68c8f564
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976439"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: Barometer

Mit der Klasse **Barometer** können Sie den Barometersensor des Geräts überwachen, der den Druck misst.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-barometer"></a>Verwenden des Barometers

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Barometerfunktionalität ruft die Methoden `Start` und `Stop` auf, um auf die Änderungen der Druckmesswerte des Barometers in Hektopascal zu lauschen. Änderungen werden über das `ReadingChanged`-Ereignis zurück gesendet. Sie können sie z.B. wie folgt verwenden:

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
        Console.WriteLine($"Reading: Pressure: {data.PressureInHectopascals} hectopascals");
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

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine plattformspezifischen Implementierungsangaben.

# <a name="iostabios"></a>[iOS](#tab/ios)

Diese API verwendet [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) zum Überwachen von Druckänderungen. Diese Hardwarefunktion wurde iPhone 6 und neueren Geräten hinzugefügt. `FeatureNotSupportedException` wird auf Geräten ausgelöst, die Altimeter nicht unterstützen.

`SensorSpeed` wird aufgrund der fehlenden Unterstützung unter iOS nicht verwendet.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine plattformspezifischen Implementierungsangaben.

-----

## <a name="api"></a>API

- [Barometer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Barometer-API-Dokumentation](xref:Xamarin.Essentials.Barometer)
