---
title: 'Xamarin.Essentials: Gyroskop'
description: Die Gyroscope-Klasse in Xamarin.Essentials können Sie der Sensor Gyroskop des Geräts zu überwachen, die Drehung um das Gerät die drei primären Achsen misst.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f1e1199ae32158889ec569eb5f7e9742f37d45d4
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353625"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: Gyroskop

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Gyroskop** Klasse können Sie das Gerät die Gyroskop Sensor Überwachung die Drehung um das Gerät die drei primären Achsen ist.

## <a name="using-gyroscope"></a>Verwenden von Gyroskop

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität Gyroskop erfolgt durch Aufrufen der `Start` und `Stop` Methoden zum Lauschen auf Änderungen an der Gyroskop. Alle Änderungen gesendet werden, durch die `ReadingChanged` Ereignis. Hier ist Beispiel für die Verwendung:

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

- [Gyroskop-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Gyroskop-API-Dokumentation](xref:Xamarin.Essentials.Gyroscope)
