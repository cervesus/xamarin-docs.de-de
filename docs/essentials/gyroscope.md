---
title: 'Xamarin.Essentials: Gyroskop'
description: Die Gyroskop-Klasse in Xamarin.Essentials können Sie die Sensor, der das Gerät Gyroskop überwachen, die Drehung um drei Primärachsen des Geräts misst.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2f2961c6cb78293891e186e7e0f749a7aa2fb8fc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783014"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: Gyroskop

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Gyroskop** -Klasse können Sie das Gerät Gyroskop Sensor überwachen, die der Drehung um drei Primärachsen des Geräts ist.

## <a name="using-gyroscope"></a>Verwenden von Gyroskop

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionsweise der Gyroskop durch Aufrufen der `Start` und `Stop` Methoden, um Änderungen an der Gyroskop abhören. Alle Änderungen gesendet werden, wieder über die `ReadingChanged` Ereignis. So sieht Verwendung des Beispiels aus:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(GyroscopeChangedEventArgs e)
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Temperatursensor Speed](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste** – die Sensordaten so schnell wie möglich (nicht garantiert zurückzugebenden UI-Thread) abrufen.
- **Spiel** – Rate für Spiele, die (nicht unbedingt UI-Thread zurückgeben) geeignet ist.
- **Normal** – Standardsatz für Bildschirm Ausrichtung wird geeignet ist.
- **UI** – Rate für die allgemeine Benutzeroberfläche geeignet ist.

## <a name="api"></a>API

- [Gyroskop-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Gyroskop API-Dokumentation](xref:Xamarin.Essentials.Gyroscope)
