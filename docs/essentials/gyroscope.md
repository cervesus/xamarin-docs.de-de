---
title: "Xamarin.Essentials: Gyroscope" description: "Mit der Klasse „Gyroscope“ in Xamarin.Essentials können Sie den Gyroskopsensor des Geräts überwachen, der die Drehung um die drei Hauptachsen des Geräts misst."
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1 author: jamesmontemagno ms.author: jamont ms.date: 11/04/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: Gyroskop

Mit der Klasse **Gyroscope** können Sie den Gyroskopsensor des Geräts überwachen, der die Drehung um die drei Hauptachsen des Geräts verfolgt.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-gyroscope"></a>Verwenden des Gyroskops

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Gyroskopfunktion ruft die Methoden `Start` und `Stop` auf, um das Gyroskop auf Veränderungen zu überwachen. Änderungen werden über das `ReadingChanged`-Ereignis zurückgesendet. Sie können sie z.B. wie folgt verwenden:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z reported in rad/s
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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Gyroscope-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Gyroscope-API-Dokumentation](xref:Xamarin.Essentials.Gyroscope)
