---
title: 'Xamarin.Essentials: Kompass'
description: Dieses Dokument beschreibt die Compass-Klasse in Xamarin.Essentials, der Sie die Überschrift für das Gerät die elektromagnetischen Norden überwachen können.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947412"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Kompass

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Compass** Klasse können Sie das Gerät die elektromagnetischen Norden Überschrift zu überwachen.

## <a name="using-compass"></a>Mithilfe der Kompass

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Compass-Funktionalität kann durch Aufrufen der `Start` und `Stop` Methoden zum Lauschen auf Änderungen an der Kompass. Alle Änderungen gesendet werden, durch die `ReadingChanged` Ereignis. Im Folgenden ein Beispiel:

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android bietet keine API zum Abrufen von die Compass-Überschrift. Wir verwenden den Beschleunigungsmesser und Magnetometer zum Berechnen der der elektromagnetischen Norden Überschrift, das Empfehlung von Google ist. 

In seltenen Fällen sehen Sie möglicherweise inkonsistente Ergebnisse, weil die Sensoren kalibriert werden, müssen die umfasst das Verschieben von Ihrem Geräts in einer Abbildung 8 Bewegung. Die beste Möglichkeit dafür Dies ist, öffnen Sie Google Maps, tippen auf den Punkt für Ihren Standort aus, und wählen Sie **Calibrate Compass**.

Denken Sie daran, das Ausführen von mehreren Sensoren aus Ihrer app zur gleichen Zeit die Geschwindigkeit der Sensor anpassen kann.

--------------

## <a name="api"></a>API

- [Die Compass-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass-API-Dokumentation](xref:Xamarin.Essentials.Compass)
