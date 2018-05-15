---
title: Xamarin.Essentials Kompass
description: Die Compass-Klasse können Sie das Gerät elektromagnetischen North Überschrift überwachen.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 39f141424ddd247458b9c8b35ae02ab29e2c2206
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials Kompass

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Kompass** Klasse können Sie das Gerät elektromagnetischen North Überschrift zu überwachen.

## <a name="using-compass"></a>Kompass verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionsweise der Kompass durch Aufrufen der `Start` und `Stop` Methoden, um Änderungen an der Kompass abhören. Alle Änderungen gesendet werden, wieder über die `ReadingChanged` Ereignis. Im Folgenden ein Beispiel:

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Temperatursensor Speed](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste** – die Sensordaten so schnell wie möglich (nicht garantiert zurückzugebenden UI-Thread) abrufen.
- **Spiel** – Rate für Spiele, die (nicht unbedingt UI-Thread zurückgeben) geeignet ist.
- **Normal** – Standardsatz für Bildschirm Ausrichtung wird geeignet ist.
- **UI** – Rate für die allgemeine Benutzeroberfläche geeignet ist.

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android bietet keine API zum Abrufen von Compass Überschrift. Wir nutzen die Beschleunigungsmesser und die Magnetometer vom Google empfohlen wird die Überschrift elektromagnetischen North berechnen. 

In seltenen Fällen sehen Sie möglicherweise inkonsistente Ergebnisse, da die Sensoren kalibriert werden, müssen die umfasst das Verschieben von Ihrem Geräts in einer Abbildung 8 Bewegung. Die beste Methode hierfür ist dies öffnen Google Maps, tippen auf den Punkt für Ihren Standort aus, und wählen **kalibrieren Kompass**.

Bedenken Sie, die mehrere Sensoren aus Ihrer app zur gleichen Zeit ausgeführt der Temperatursensor Speed anpassen können.

--------------

## <a name="api"></a>API

- [Kompass Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass-API-Dokumentation](xref:Xamarin.Essentials.Compass)
