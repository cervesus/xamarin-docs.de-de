---
title: ''Xamarin.Essentials: OrientationSensor'' description: ms.assetid: author: ms.author: ms.date: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

Mit der Klasse **OrientationSensor** können Sie die Ausrichtung eines Gerätes im dreidimensionalen Raum überwachen.

> [!NOTE]
> Diese Klasse dient zum Bestimmen der Ausrichtung eines Gerätes im 3D-Raum. Wenn Sie feststellen müssen, ob sich die Videoanzeige des Geräts im Hoch- oder Querformat befindet, verwenden Sie die Eigenschaft `Orientation` des Objekts `ScreenMetrics`, das in der Klasse [`DeviceDisplay`](device-display.md) verfügbar ist.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-orientationsensor"></a>Verwenden von OrientationSensor

Fügen Sie Ihrem Projekt einen Xamarin.Essentials-Verweis hinzu:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` wird durch Aufrufen der Methode `Start` aktiviert, um Änderungen an der Ausrichtung des Geräts zu überwachen, und durch Aufrufen der Methode `Stop` deaktiviert. Änderungen werden über das `ReadingChanged`-Ereignis zurück gesendet. Eine Beispielanwendung:

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(object sender, OrientationSensorChangedEventArgs e)
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

`OrientationSensor`-Messwerte werden im [`Quaternion`](xref:System.Numerics.Quaternion)-Format zurückgegeben, das die Ausrichtung des Geräts basierend auf zwei 3D-Koordinatensystemen beschreibt:

Das Gerät (in der Regel ein Smartphone oder Tablet) verfügt über ein 3D-Koordinatensystem mit den folgenden Achsen:

- Die positive X-Achse zeigt im Hochformat nach rechts auf der Anzeige.
- Die positive Y-Achse zeigt im Hochformat auf dem Gerät nach oben.
- Die positive Z-Achse zeigt aus dem Bildschirm heraus.

Das 3D-Koordinatensystem der Erde hat die folgenden Achsen:

- Die positive X-Achse ist eine Tangente zur Erdoberfläche und zeigt nach Osten.
- Die positive Y-Achse ist eine Tangente zur Erdoberfläche und zeigt nach Norden.
- Die positive Z-Achse verläuft senkrecht zur Erdoberfläche und zeigt nach oben.

Die `Quaternion` beschreibt die Drehung des Koordinatensystems des Geräts relativ zum Erdkoordinatensystem.

Ein `Quaternion`-Wert ist sehr eng mit der Drehung um eine Achse verbunden. Wenn eine Drehachse der normierte Vektor (a<sub>x</sub>, a<sub>y</sub>, a<sub>z</sub>) ist und der Drehwinkel Θ entspricht, dann lauten die Komponenten (X, Y, Z, W) der Quaternion:

(a<sub>x</sub>·sin(Θ/2), a<sub>y</sub>·sin(Θ/2), a<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Dies sind rechtshändige Koordinatensysteme: Zeigt der Daumen der rechten Hand in die positive Richtung der Drehachse, zeigen die anderen Finger den positiven Drehsinn an.

Beispiele:

- Wenn das Gerät flach auf einem Tisch mit dem Bildschirm nach oben liegt und die Oberseite des Geräts (im Hochformat) nach Norden zeigt, werden die beiden Koordinatensysteme ausgerichtet. Der `Quaternion`-Wert stellt die Identitätsquaternion (0, 0, 0, 1) dar. Alle Rotationen lassen sich relativ zu dieser Position analysieren.

- Wenn das Gerät flach auf einem Tisch mit dem Bildschirm nach oben liegt und die Oberseite des Geräts (im Hochformat) nach Westen zeigt, entspricht der `Quaternion`-Wert (0, 0, 0,707, 0,707). Das Gerät wurde um 90 Grad um die Z-Achse der Erde gedreht.

- Wenn das Gerät aufrecht gehalten wird, sodass seine Oberseite (im Hochformat) zum Himmel und die Rückseite nach Norden zeigt, wurde das Gerät um 90 Grad um die X-Achse gedreht. Der `Quaternion`-Wert lautet 0,707, 0, 0, 0,707.

- Wenn das Gerät so positioniert ist, dass sein linker Rand auf einem Tisch liegt und die Oberseite nach Norden zeigt, wurde das Gerät um &ndash;90 Grad um die Y-Achse (bzw. 90 Grad um die negative Y-Achse) gedreht. Der `Quaternion`-Wert ist (0, –0,707, 0, 0,707).

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor-API-Dokumentation](xref:Xamarin.Essentials.OrientationSensor)
