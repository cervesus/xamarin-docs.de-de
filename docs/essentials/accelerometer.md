---
title: 'Xamarin.Essentials: Beschleunigungsmesser'
description: Die Klasse des Beschleunigungsmessers in Xamarin.Essentials können Sie die Sensor für beschleunigungsmessung des Geräts, womit die Beschleunigung des Geräts in drei dimensionalen Raum zu überwachen.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b5a24e214eb129b4d53b94586632791c8827447b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353840"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: Beschleunigungsmesser

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Beschleunigungsmesser** Klasse können Sie den Sensor für beschleunigungsmessung des Geräts überwachen, die die Beschleunigung des Geräts in drei dimensionalen Raum angibt.

## <a name="using-accelerometer"></a>Beschleunigungsmesser

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Der Beschleunigungsmesser funktioniert durch Aufrufen der `Start` und `Stop` Methoden, um die Änderungen auf die Beschleunigung überwacht werden soll. Alle Änderungen gesendet werden, durch die `ReadingChanged` Ereignis. Hier ist Beispiel für die Verwendung:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

Beschleunigungsmesser Messwerte werden in G. zurückgemeldet. Eine G ist eine Einheit der Gravitation erzwingen, üben nach der Erde Schwerpunkte Feld gleich (9.81 m/s ^ 2).

Das Koordinatensystem ist relativ zum Bildschirm des Telefons in der standardausrichtung definiert. Die Achsen werden nicht ausgetauscht werden, wenn Änderungen der bildschirmausrichtung des Geräts.

Die x-Achse ist horizontal und zeigt auf der rechten Seite, die y-Achse ist vertikal und zeigt nach oben und die Z-Achse zeigt in Richtung der Außenseite der Vorderseite des Bildschirms. In diesem System verwendet haben Koordinaten hinter dem Bildschirm negative Z-Werte.

Beispiele:

* Wenn das Gerät flach auf einem Tisch liegt, und auf der linken Seite an der rechten Seite mithilfe von Push übertragen wird, ist der Wert von X Beschleunigung positiv.

* Wenn das Gerät flach auf einem Tisch liegt, wird der Beschleunigungswert +1.00 G oder (+ 9.81 m/s ^ 2), entsprechen die Beschleunigung des Geräts (0 m/s ^ 2) minus beträgt (-9.81 m/s ^ 2) und normalisierten wie G.

* Wenn das Gerät flach auf einem Tisch liegt, und für den Himmel durch eine Beschleunigung von einer m/s mithilfe von Push übertragen wird ^ 2, der Beschleunigungswert entspricht einer 9.81 an, die Beschleunigung des Geräts entsprechen (+ m/s ^ 2) minus beträgt (-9.81 m/s ^ 2) und normalisierten in G.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Beschleunigungsmesser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Beschleunigungsmesser-API-Dokumentation](xref:Xamarin.Essentials.Accelerometer)
