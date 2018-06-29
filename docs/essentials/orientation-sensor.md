---
title: 'Xamarin.Essentials: OrientationSensor'
description: Die OrientationSensor-Klasse können Sie die Ausrichtung eines Geräts im dreidimensionalen Raum zu überwachen.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080505"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **OrientationSensor** Klasse können Sie die Ausrichtung eines Geräts in drei dimensionalen Raum zu überwachen.

> [!NOTE]
> Diese Klasse ist für die Ausrichtung eines Geräts im 3D-Bereich bestimmen. Wenn müssen Sie bestimmen, ob das Gerät Videoadapter des Anzeige im Hochformat oder Querformat Modus befindet, verwenden Sie die `Orientation` Eigenschaft des der `ScreenMetrics` Objekt von der [ `DeviceDisplay` ](device-display.md) Klasse.

## <a name="using-orientationsensor"></a>Verwenden von OrientationSensor

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die `OrientationSensor` aktiviert ist, durch Aufrufen der `Start` Methode zum Überwachen von Änderungen an des Geräts Ausrichtung und deaktiviert durch Aufrufen der `Stop` Methode. Alle Änderungen gesendet werden, wieder über die `ReadingChanged` Ereignis. Hier ist eine Beispiel für die Verwendung:

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` Messwerte werden in Form von gemeldet eine [ `Quaternion` ](xref:System.Numerics.Quaternion) , beschreibt die Ausrichtung des Geräts anhand von zwei 3D Koordinatensysteme:

Das Gerät (in der Regel auf einem Telefon oder Tablet) hat eine 3D Koordinatensystem mit den folgenden Achsen:

- Die Positive X Achsenpunkte rechts neben der Anzeige im Hochformat.
- Die positive y-Achse zeigt oben in der das Gerät im Hochformat.
- Die positive Z-Achse verweist, aus dem Bildschirm.

Der Erde dar, die 3D Koordinatensystem hat die folgenden Achsen:

- Der positiven x-Achse Tangens auf die Oberfläche der Erde dar und zeigt Osten.
- Die positive y-Achse ist auch auf die Oberfläche der Erde dar und zeigt Nord Tangenten.
- Die positive Z-Achse ist senkrecht zur Oberfläche der Erde dar und zeigt einrichten.

Die `Quaternion` wird beschrieben, die Drehung des Koordinatensystems an das Gerät relativ zu der Erde-Koordinatensystem.

Ein `Quaternion` Wert ist sehr eng miteinander verknüpft, mit der Drehung um eine Achse. Wenn eine Achse der Drehung der normalisierte Vektor ist (eine<sub>x</sub>, eine<sub>y</sub>,<sub>z</sub>), und den Drehwinkel für Bezeichnungen Θ, und klicken Sie dann die (X, Y, Z, W) sind Komponenten der Quaternion:

(eine<sub>x</sub>·sin(Θ/2), eine<sub>y</sub>·sin(Θ/2), eine<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Hierbei handelt es sich um rechten Koordinatensysteme, daher mit dem Ziehpunkt von rechten, die in positiver Richtung der Drehung Achse gezeigt wird, die Kurve der Finger die Richtung der Drehung für positive Winkel angeben.

Beispiele:

* Wenn das Gerät flach auf eine Tabelle mit dem Bildschirm nach oben, am oberen Rand der Gerät (im Hochformat) zeigt, liegt, werden zwei Koordinatensysteme ausgerichtet. Die `Quaternion` Wert darstellt, die identitätsquaternion (0, 0, 0, 1). Alle Drehungen können relativ zu dieser Position analysiert werden.

* Wenn das Gerät flach auf eine Tabelle mit dem Bildschirm nach oben, und dem oberen des Geräts (im Hochformat) zeigen west, liegt die `Quaternion` Wert (0, 0, dem Wert 0,707 entspricht, dem Wert 0,707 entspricht). Das Gerät wurde um die Z-Achse der Erde dar, um 90° gedreht.

* Wenn das Gerät aufrecht aufrecht erhalten wird, damit Anfang (im Hochformat) auf die Sky verweist und die Rückseite des Geräts Norden portalclient, wurde das Gerät um 90 Grad gedreht, um die x-Achse. Die `Quaternion` Wert (dem Wert 0,707 entspricht, 0, 0, dem Wert 0,707 entspricht).

* Wenn das Gerät positioniert ist, am linke Rand für eine Tabelle, und im oberen Bereich Norden, verweist das Gerät wurde gedreht &ndash;90 Grad, um die y-Achse (oder um 90 Grad, um die negative y-Achse). Die `Quaternion` Wert ist ("0", "-0.707", "0", "dem Wert 0,707 entspricht").

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Temperatursensor Speed](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste** – die Sensordaten so schnell wie möglich (nicht garantiert zurückzugebenden UI-Thread) abrufen.
- **Spiel** – Rate für Spiele, die (nicht unbedingt UI-Thread zurückgeben) geeignet ist.
- **Normal** – Standardsatz für Bildschirm Ausrichtung wird geeignet ist.
- **UI** – Rate für die allgemeine Benutzeroberfläche geeignet ist.

Wenn der Ereignishandler nicht unbedingt im UI-Thread ausgeführt, und verwenden, wenn der Ereignishandler benötigt Zugriff auf Elemente der Benutzeroberfläche, die [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) Methode, um diesen Code im UI-Thread auszuführen.

## <a name="api"></a>API

- [OrientationSensor-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API-Dokumentation](xref:Xamarin.Essentials.OrientationSensor)
