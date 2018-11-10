---
title: 'Xamarin.Essentials: Kompass'
description: In diesem Dokument wird die Klasse „Compass“ in Xamarin.Essentials beschrieben, mit der Sie die Ausrichtung des Geräts auf den magnetischen Norden überwachen können.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 51812f9b4f88d77bf553a26ef3a6802239e338e0
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675496"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Kompass

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der Klasse **Compass** können Sie die Ausrichtung des Geräts auf den magnetischen Norden überwachen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-compass"></a>Verwenden des Kompasses

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Kompassfunktion ruft die Methoden `Start` und `Stop` auf, um den Kompass auf Veränderungen zu überwachen. Änderungen werden über das `ReadingChanged`-Ereignis zurück gesendet. Im Folgenden ein Beispiel:

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

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der Plattformimplementierung

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android bietet keine API zum Abrufen der Kompassausrichtung. Zum Berechnen der magnetischen Nordrichtung werden der Beschleunigungsmesser und der Magnetometer verwendet, was von Google empfohlen wird.

In seltenen Fällen werden möglicherweise inkonsistente Ergebnisse angezeigt, weil die Sensoren kalibriert werden müssen. Dafür müssen Sie u.a. mit Ihrem Gerät eine Acht formen. Am besten öffnen Sie dazu Google Maps, tippen Sie auf den Punkt für Ihren Standort, und wählen Sie **Kompass kalibrieren** aus.

Denken Sie daran, dass sich das gleichzeitige Ausführen mehrerer Sensoren von Ihrer App auf die Geschwindigkeit des Sensors auswirken kann.

## <a name="low-pass-filter"></a>Tiefpassfilter

Je nachdem, wie die Android-Kompasswerte aktualisiert und berechnet werden, müssen die Werte möglicherweise geglättet werden. Hier kann ein _Tiefpassfilter_ angewendet werden, der aus den Sinus- und Kosinuswerten der Winkel einen Mittelwert bildet. Er wird durch Festlegen der Eigenschaft `ApplyLowPassFilter` in der Klasse `Compass` aktiviert:

```csharp
Compass.ApplyLowPassFilter = true;
```

Dies gilt nur für die Android-Plattform. Weitere Informationen erhalten Sie [hier](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>API

- [Compass-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass-API-Dokumentation](xref:Xamarin.Essentials.Compass)
