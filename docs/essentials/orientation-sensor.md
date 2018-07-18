---
title: 'Xamarin.Essentials: OrientationSensor'
description: Die OrientationSensor-Klasse können Sie die Ausrichtung eines Geräts im dreidimensionalen Raum zu überwachen.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c01fa28e495eb3eceec62885060dce8f096c4086
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947389"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **OrientationSensor** Klasse können Sie die Ausrichtung eines Geräts in drei dimensionalen Raum zu überwachen.

> [!NOTE]
> Diese Klasse ist für die Ausrichtung eines Geräts im 3D-Raum zu bestimmen. Bei Bedarf, um festzustellen, ob das Gerät Bild des hoch-oder Querformat angezeigt wird, verwenden Sie die `Orientation` Eigenschaft der `ScreenMetrics` Objekt verfügbar der [ `DeviceDisplay` ](device-display.md) Klasse.

## <a name="using-orientationsensor"></a>Verwenden von OrientationSensor

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die `OrientationSensor` erfolgt durch Aufrufen der `Start` Methode zum Überwachen von Änderungen an des Geräts die Ausrichtung und deaktiviert durch Aufrufen der `Stop` Methode. Alle Änderungen gesendet werden, durch die `ReadingChanged` Ereignis. Hier ist eine Beispiel für die Verwendung:

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

`OrientationSensor` Messwerte werden in Form von zurückgemeldet eine [ `Quaternion` ](xref:System.Numerics.Quaternion) , beschreibt die Ausrichtung des Geräts basierend auf zwei 3D Koordinatensysteme:

Das Gerät (in der Regel auf einem Smartphone oder Tablet) besteht aus einem 3D-Koordinatensystem, mit dem folgenden Achsen:

- Die Positive X Achsenpunkte rechts neben der Anzeige im Hochformatmodus ausgeführt werden soll.
- Die positive y-Achse verweist auf den Anfang das Gerät im Hochformat.
- Die positive Z-Achse zeigt aus dem Bildschirm heraus.

Die 3D-Koordinatensystem der Erde hat die folgenden Achsen:

- Der positiven x-Achse Tangens auf die Oberfläche der Erde dar und zeigt "Osten".
- Es ist auch die positive y-Achse Tangens auf die Oberfläche der Erde und Norden zeigt.
- Die positive Z-Achse ist senkrecht zur Oberfläche der Erde und zeigt nach oben.

Die `Quaternion` wird beschrieben, die Drehung des Koordinatensystems des Geräts, relativ zur Erde-Koordinatensystem.

Ein `Quaternion` Wert ist sehr eng in die Rotation um eine Achse. Wenn eine Achse der Drehung der normalisierte Vektor ist (eine<sub>x</sub>,<sub>y</sub>, eine<sub>z</sub>), und der Winkel der Drehung Θ, und klicken Sie dann die (X, Y, Z, W) sind Komponenten der Quaternion:

(eine<sub>x</sub>·sin(Θ/2), eine<sub>y</sub>·sin(Θ/2), eine<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Hierbei handelt es sich um rechten Koordinatensysteme, sodass das Thumb-Steuerelement von der rechten Seite in positiver Richtung der Achse der Drehung gezeigt wird, die Kurve der Finger anzugeben, dass mit die Richtung der Drehung für positiven Winkel.

Beispiele:

* Wenn das Gerät flach auf eine Tabelle befindet, mit dem Bildschirm nach oben, am Rand des Geräts (im Hochformat) verweist (Norden), werden die zwei Koordinatensysteme ausgerichtet. Die `Quaternion` Wert darstellt, die Einheitsquaternion (0, 0, 0, 1). Alle Drehungen können relativ zu dieser Position analysiert werden.

* Wenn das Gerät flach auf eine Tabelle mit dem Bildschirm nach oben und am Anfang des Geräts (in-Hochformat) verweist, Westen, liegt die `Quaternion` Wert (0, 0, dem Wert 0,707 entspricht, dem Wert 0,707 entspricht). Das Gerät wurde um rund um die Z-Achse der Erde 90 Grad gedreht.

* Wenn das Gerät aufrecht befindet, damit im oberen Bereich (im Hochformat) auf den Himmel verweist und der Rückseite des Geräts nach Norden zeigt, wurde das Gerät um 90 Grad gedreht, um die x-Achse. Die `Quaternion` Wert (dem Wert 0,707 entspricht, 0, 0, dem Wert 0,707 entspricht).

* Wenn das Gerät befindet, am linke Rand für eine Tabelle, und im oberen Bereich zeigt (Norden), hat das Gerät gedreht wurden &ndash;um 90 Grad, um die y-Achse (oder um 90 Grad um die y-Achse negativ). Die `Quaternion` Wert ("0", "-0.707", "0", "dem Wert 0,707 entspricht").

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor-API-Dokumentation](xref:Xamarin.Essentials.OrientationSensor)
