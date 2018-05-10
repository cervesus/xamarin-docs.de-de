---
title: Xamarin.Essentials Magnetometer
description: Die Magnetometer-Klasse können Sie das Gerät Magnetometer Sensor überwachen die Ausrichtung des Geräts relativ zu der Erde elektromagnetischen Feld angibt.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bb9bd656c809b05c49a27f7b3dab2a64ff7b7e94
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials Magnetometer

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Magnetometer** -Klasse können Sie das Gerät Magnetometer Sensor überwachen, die Ausrichtung des Geräts relativ zu der Erde elektromagnetischen Feld angibt.

## <a name="using-magnetometer"></a>Magnetometer verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionsweise der Magnetometer durch Aufrufen der `Start` und `Stop` Methoden, um Änderungen an der Magnetometer abhören. Alle Änderungen gesendet werden, wieder über die `ReadingChanged` Ereignis. So sieht Verwendung des Beispiels aus:

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

Alle Daten werden zurückgegeben, in der Einheit Mikrotesla verwendet.

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Temperatursensor Speed](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste** – die Sensordaten so schnell wie möglich (nicht garantiert zurückzugebenden UI-Thread) abrufen.
- **Spiel** – Rate für Spiele, die (nicht unbedingt UI-Thread zurückgeben) geeignet ist.
- **Normal** – Standardsatz für Bildschirm Ausrichtung wird geeignet ist.
- **UI** – Rate für die allgemeine Benutzeroberfläche geeignet ist.

## <a name="api"></a>API

- [Magnetometer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [Magnetometer API-Dokumentation](xref:Xamarin.Essentials.Magnetometer)
