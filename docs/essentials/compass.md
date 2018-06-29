---
title: 'Xamarin.Essentials: Kompass'
description: Dieses Dokument beschreibt die Compass-Klasse in Xamarin.Essentials, der Sie das Gerät elektromagnetischen North Überschrift überwachen können.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 63818014a9b3bdbef479055cbbcfbf8d348080fc
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080498"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Kompass

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

Wenn der Ereignishandler nicht unbedingt im UI-Thread ausgeführt, und verwenden, wenn der Ereignishandler benötigt Zugriff auf Elemente der Benutzeroberfläche, die [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) Methode, um diesen Code im UI-Thread auszuführen.

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android bietet keine API zum Abrufen von Compass Überschrift. Wir nutzen die Beschleunigungsmesser und die Magnetometer vom Google empfohlen wird die Überschrift elektromagnetischen North berechnen. 

In seltenen Fällen sehen Sie möglicherweise inkonsistente Ergebnisse, da die Sensoren kalibriert werden, müssen die umfasst das Verschieben von Ihrem Geräts in einer Abbildung 8 Bewegung. Die beste Methode hierfür ist dies öffnen Google Maps, tippen auf den Punkt für Ihren Standort aus, und wählen **kalibrieren Kompass**.

Bedenken Sie, die mehrere Sensoren aus Ihrer app zur gleichen Zeit ausgeführt der Temperatursensor Speed anpassen können.

--------------

## <a name="api"></a>API

- [Kompass Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass-API-Dokumentation](xref:Xamarin.Essentials.Compass)
