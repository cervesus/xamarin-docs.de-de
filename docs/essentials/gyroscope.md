---
title: 'Xamarin.Essentials: Gyroskop'
description: Mit der Klasse „Gyroscope“ in Xamarin.Essentials können Sie den Gyroskopsensor des Geräts überwachen, der die Drehung um die drei Hauptachsen des Geräts misst.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1d42658160855e260e0d159c58a1f95e7a8c7d4c
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674727"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: Gyroskop

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der Klasse **Gyroscope** können Sie den Gyroskopsensor des Geräts überwachen, der die Drehung um die drei Hauptachsen des Geräts verfolgt.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-gyroscope"></a>Verwenden des Gyroskops

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Gyroskopfunktion ruft die Methoden `Start` und `Stop` auf, um das Gyroskop auf Veränderungen zu überwachen. Änderungen werden über das `ReadingChanged`-Ereignis zurück gesendet. Sie können sie z.B. wie folgt verwenden:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
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

## <a name="api"></a>API

- [Gyroscope-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Gyroscope-API-Dokumentation](xref:Xamarin.Essentials.Gyroscope)
