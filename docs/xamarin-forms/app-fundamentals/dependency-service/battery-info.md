---
title: Überprüfen des Akkustatus
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms DependencyService-Klasse, die Zugriff auf Akkuinformationen für jede Plattform systemintern verwendet wird.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: cbb4a01ac2c6d933fe40a0b3c2571d1fe3ce75c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998393"
---
# <a name="checking-battery-status"></a>Überprüfen des Akkustatus

Dieser Artikel führt durch die Erstellung einer Anwendung, die Akkustatus überprüft. Dieser Artikel basiert auf den Akku-Plug-in von James Montemagno. Weitere Informationen finden Sie unter den [GitHub-Repository](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Da Xamarin.Forms nicht für die Überprüfung des aktuellen Status des Akkus enthalten ist, müssen diese Anwendung mit [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) systemeigene APIs nutzen.  Dieser Artikel behandelt die folgenden Schritte aus, für die Verwendung von `DependencyService`:

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; zu verstehen, wie die Schnittstelle in freigegebenem Code erstellt wird.
- **[iOS-Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in nativem Code zu implementieren.
- **[Implementierung der universellen Windows-Plattform](#UWPImplementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[In freigegebenem Code implementieren](#Implementing_in_Shared_Code)**  &ndash; erfahren Sie, wie Sie mit `DependencyService` , die native Implementierung von freigegebenem Code aufzurufen.

Abschließend wird die Anwendung mit `DependencyService` wird die folgende Struktur aufweisen:

![](battery-info-images/battery-diagram.png "DependencyService Anwendungsstruktur")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle in freigegebenem Code, der die gewünschte Funktionalität ausdrückt. Im Falle eines Akkus, überprüfen die Anwendung ist die relevante Informationen der Prozentsatz der die verbleibende Akkukapazität, ob das Gerät aufgeladen oder nicht, und wie das Gerät mit Strom versorgt wird:

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

Mit der Programmierung für diese Schnittstelle in den freigegebenen Code lässt sich auf die Xamarin.Forms-app auf die Power-Verwaltungs-APIs auf jeder Plattform aus.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, müssen einen parameterlosen Konstruktor für die Arbeit mit der `DependencyService`. Von Schnittstellen können keine Konstruktoren definiert werden.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die `IBattery` -Schnittstelle muss in jedem Anwendungsprojekt plattformspezifischen-implementiert werden. Die iOS-Implementierung verwendet das systemeigene [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) APIs, um den Zugriff auf Akkuinformationen. Beachten Sie, dass die folgende Klasse über einen parameterlosen Konstruktor hat, damit die `DependencyService` neue Instanzen erstellen können:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

Fügen Sie abschließend Folgendes `[assembly]` Attribut oberhalb der Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der `IBattery` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<IBattery>` in freigegebenem Code verwendet werden können, um eine Instanz davon erstellen:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Die Android-Implementierung verwendet die [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API. Diese Implementierung ist komplexer als der iOS-Version, die überprüft, behandeln fehlende Akku Berechtigungen erfordern:

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

Fügen Sie Folgendes `[assembly]` Attribut oberhalb der Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der `IBattery` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<IBattery>` kann verwendet werden, der freigegebene Code kann eine Instanz davon erstellen.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementierung der universellen Windows-Plattform

Die UWP-Implementierung verwendet die `Windows.Devices.Power` APIs zum Abrufen von Informationen zum Akku:

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

Die `[assembly]` Attribut über der Namespacedeklaration registriert die Klasse als eine Implementierung der `IBattery` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<IBattery>` in freigegebenem Code verwendet werden können, um eine Instanz davon erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>In freigegebenem Code implementieren

Nun, da die Schnittstelle für jede Plattform implementiert wurde, kann die freigegebene Anwendung geschrieben werden, zu nutzen. Die Anwendung besteht aus einer Seite mit einer Schaltfläche tippen, die bei Aktualisierungen auf den Text mit dem aktuellen Status des Akkus. Er verwendet den `DependencyService` zum Abrufen einer Instanz von der `IBattery` Schnittstelle. Zur Laufzeit werden diese Instanz die plattformspezifischen Implementierungen, die uneingeschränkten Zugriff auf das systemeigene SDK verfügt.

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

Ausführen dieser Anwendung unter iOS, führt Android oder UWP und auf die Schaltfläche den Text der Schaltfläche entsprechend den aktuellen Status des Geräts aktualisieren.

![](battery-info-images/battery.png "Akku-Status-Beispiel")


## <a name="related-links"></a>Verwandte Links

- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/DependencyService)
- [Verwenden von DependencyService (Beispiel)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
