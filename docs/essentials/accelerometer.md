---
title: 'Xamarin.Essentials: Beschleunigungsmesser'
description: Die Beschleunigungsmesser-Klasse in Xamarin.Essentials können Sie das Gerät Beschleunigungsmesser Sensor, überwachen, womit die Beschleunigung des Geräts in drei dimensionalen Raum.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8229a372659e7918457a9d2f358b871e1a3f5978
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080490"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: Beschleunigungsmesser

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Beschleunigungsmesser** Klasse können Sie das Gerät Beschleunigungsmesser Sensor zu überwachen, womit die Beschleunigung des Geräts in drei dimensionalen Raum.

## <a name="using-accelerometer"></a>Beschleunigungsmesser verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionsweise der Beschleunigungsmesser durch Aufrufen der `Start` und `Stop` Methoden, um Änderungen an die Beschleunigung abhören. Alle Änderungen gesendet werden, wieder über die `ReadingChanged` Ereignis. So sieht Verwendung des Beispiels aus:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
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

Beschleunigungsmesser Messwerte werden in G. zurückgemeldet. Eine G ist eine Einheit der Gravitation erzwingen, die von der Erde Schwerpunkte Feld ausgeübt gleich (9.81 m/s ^ 2).

Das Koordinatensystem ist relativ zu dem Bildschirm der Telefonnummer in der standardausrichtung definiert. Die Achsen werden nicht ausgetauscht werden, wenn das Gerät bildschirmausrichtung ändert.

Die x-Achse ist horizontal und zeigt auf das Recht, die y-Achse ist vertikal nach oben und die Z-Achse auf der Außenseite der Vorderseite des Bildschirms verweist. In diesem System haben Koordinaten hinter dem Bildschirm negative Z-Werte.

Beispiele:

* Wenn das Gerät flach auf eine Tabelle liegt und auf der linken Seite nach rechts verschoben wird, ist der Wert von X Acceleration positiv.

* Wenn das Gerät flach auf eine Tabelle liegt, ist der Beschleunigungswert +1.00 G oder (+ 9.81 m/s ^ 2), die die Beschleunigung des Geräts entsprechen (0, m/s ^ 2) minus der "Force" Schwerpunkt (-9.81 m/s ^ 2) und der normalisierten wie G.

* Wenn das Gerät flach auf eine Tabelle befindet, und wird in Richtung der Sky mit einer Beschleunigung von m/s abgelegt ^ 2, die Beschleunigungswert entspricht einer + 9.81 an, die eine Beschleunigung des Geräts entsprechen (+ m/s ^ 2) minus der "Force" Schwerpunkt (-9.81 m/s ^ 2) und der normalisierten in G. 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Temperatursensor Speed](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste** – die Sensordaten so schnell wie möglich (nicht garantiert zurückzugebenden UI-Thread) abrufen.
- **Spiel** – Rate für Spiele, die (nicht unbedingt UI-Thread zurückgeben) geeignet ist.
- **Normal** – Standardsatz für Bildschirm Ausrichtung wird geeignet ist.
- **UI** – Rate für die allgemeine Benutzeroberfläche geeignet ist.

Wenn der Ereignishandler nicht unbedingt im UI-Thread ausgeführt, und verwenden, wenn der Ereignishandler benötigt Zugriff auf Elemente der Benutzeroberfläche, die [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) Methode, um diesen Code im UI-Thread auszuführen.

## <a name="api"></a>API

- [Beschleunigungsmesser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Beschleunigungsmesser API-Dokumentation](xref:Xamarin.Essentials.Accelerometer)
