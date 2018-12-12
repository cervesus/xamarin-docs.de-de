---
title: 'Xamarin.Essentials: Magnetometer'
description: Mit der Klasse „Magnetometer“ in Xamarin.Essentials können Sie den Magnetometersensor des Geräts überwachen, der die Ausrichtung des Geräts relativ zum Magnetfeld der Erde angibt.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: bfc4916c40b47b715357692308d6b5dfa9db57bf
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898715"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetometer

Mit der Klasse **Magnetometer** können Sie den Magnetometersensor des Geräts überwachen, der die Ausrichtung des Geräts relativ zum Magnetfeld der Erde angibt.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-magnetometer"></a>Verwenden des Magnetometers

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Magnetometer-Funktion ruft die Methoden `Start` und `Stop` auf, um das Magnetometer Veränderungen zu überwachen. Änderungen werden über das `ReadingChanged`-Ereignis zurück gesendet. Sie können sie z.B. wie folgt verwenden:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometer_ReadingChanged(object sender, MagnetometerChangedEventArgs e)
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

Alle Daten werden in der Einheit µT (Mikrotesla) zurückgegeben.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Magnetometer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Magnetometer-API-Dokumentation](xref:Xamarin.Essentials.Magnetometer)
