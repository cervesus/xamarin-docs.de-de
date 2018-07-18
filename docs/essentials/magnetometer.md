---
title: 'Xamarin.Essentials: Magnetometer'
description: Die Magnetometer-Klasse in Xamarin.Essentials können Sie der Sensor Magnetometer des Geräts zu überwachen, die Ausrichtung des Geräts, relativ zum Magnetfelds der Erde angibt.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 52790f78c2d78347a35f111b3c4db63900c24ec7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947360"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetometer

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Magnetometer** Klasse können Sie des Geräts Magnetometer Sensor zu überwachen, die Ausrichtung des Geräts, relativ zum Magnetfelds der Erde angibt.

## <a name="using-magnetometer"></a>Mithilfe der Magnetometer

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität der Magnetometer erfolgt durch Aufrufen der `Start` und `Stop` Methoden zum Lauschen auf Änderungen an der Magnetometer. Alle Änderungen gesendet werden, durch die `ReadingChanged` Ereignis. Hier ist Beispiel für die Verwendung:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

Alle Daten werden zurückgegeben, in die Einheit Mikrotesla verwendet.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Magnetometer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Magnetometer-API-Dokumentation](xref:Xamarin.Essentials.Magnetometer)
