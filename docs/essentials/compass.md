---
title: 'Xamarin.Essentials: Kompass'
description: Dieses Dokument beschreibt die Compass-Klasse in Xamarin.Essentials, der Sie die Überschrift für das Gerät die elektromagnetischen Norden überwachen können.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c3fe98c384a87bdc08ce94e7537d1a6343767561
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353882"
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
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
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
            // Some other exception has occurred
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

## <a name="low-pass-filter"></a>Tiefpassfilter

Aufgrund der Verwendung der Android Kompass Werte werden aktualisiert und berechnet wird, gibt es möglicherweise zum Glätten der Werte. Ein _niedrig übergeben Filter_ angewendet werden können, wird der Durchschnittswert Sinus und Kosinus der Winkel und können durch Festlegen von eingeschaltet werden die `ApplyLowPassFilter` Eigenschaft für die `Compass` Klasse:

```csharp
Compass.ApplyLowPassFilter = true;
```

Dies wird nur auf die Android-Plattform angewendet. Weitere Informationen erhalten [hier](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>API

- [Die Compass-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass-API-Dokumentation](xref:Xamarin.Essentials.Compass)
