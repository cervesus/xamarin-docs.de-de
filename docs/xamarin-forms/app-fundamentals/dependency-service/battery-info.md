---
title: Überprüfen des Akkustatus
description: In diesem Artikel wird erläutert, wie Sie mit der DependencyService-Klasse von Xamarin.Forms für jede Plattform nativ auf Informationen zum Akkustatus zugreifen können.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 2c9fae211e6a88944c94ca265409865b3252a10a
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740927"
---
# <a name="checking-battery-status"></a>Überprüfen des Akkustatus

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/)

Dieser Artikel führt Sie durch die Erstellung einer Anwendung, die den Akkustatus überprüft. Dieser Artikel basiert auf dem Akku-Plug-In von James Montemagno. Weitere Informationen finden Sie im [GitHub-Repository](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Da Xamarin.Forms über keine Funktion für die Überprüfung des aktuellen Akkustatus verfügt, muss diese Anwendung [`DependencyService`](xref:Xamarin.Forms.DependencyService) verwenden, um native APIs nutzen zu können.  Dieser Artikel behandelt die folgenden Schritte bei der Verwendung von `DependencyService`:

- **[Erstellen der Schnittstelle:](#Creating_the_Interface)** Erfahren Sie, wie die Schnittstelle im freigegebenen Code erstellt wird.
- **[iOS-Implementierung:](#iOS_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für iOS implementiert wird.
- **[Android-Implementierung:](#Android_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für Android implementiert wird.
- **[UWP-Implementierung:](#UWPImplementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für die Universelle Windows-Plattform (UWP) implementiert wird.
- **[Implementierung in freigegebenem Code:](#Implementing_in_Shared_Code)** Erfahren Sie, wie Sie mit `DependencyService` die native Implementierung aus freigegebenem Code aufrufen.

Abschließend hat die Anwendung, die `DependencyService` verwendet, folgende Struktur:

![](battery-info-images/battery-diagram.png "Anwendungsstruktur von DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle in freigegebenem Code, der die gewünschte Funktionalität ausdrückt. Für eine Anwendung, die den Akkustatus überprüft, sind der Prozentsatz der verbleibenden Akkukapazität, die Angabe, ob das Gerät aufgeladen wird oder nicht, und die Art der Stromversorgung des Geräts die relevanten Informationen:

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

Die Kodierung mit dieser Schnittstelle im freigegebenen Code ermöglicht der Xamarin.Forms-App den Zugriff auf die APIs der Energieverwaltung auf jeder Plattform.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, müssen einen parameterlosen Konstruktor aufweisen, damit sie mit `DependencyService` funktionieren können. Konstruktoren können nicht durch Schnittstellen definiert werden.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die Schnittstelle `IBattery` muss in jedem plattformspezifischen Anwendungsprojekt implementiert werden. Bei der iOS-Implementierung werden die nativen [`UIDevice`](xref:UIKit.UIDevice)-APIs für den Zugriff auf Akkuinformationen verwendet. Beachten Sie, dass folgende Klasse einen parameterlosen Konstruktor aufweist, sodass `DependencyService` neue Instanzen erstellen kann:

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

Fügen Sie schlussendlich das `[assembly]`-Attribut über der Klasse hinzu (außerhalb der Namespaces, die definiert wurden), einschließlich erforderlicher `using`-Anweisungen:

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

Dieses Attribut registriert die Klasse als eine Implementierung der Schnittstelle `IBattery`, d. h., dass `DependencyService.Get<IBattery>` im freigegebenen Code verwendet werden kann, um eine Instanz davon zu erstellen:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Bei der Android-Implementierung wird die [`Android.OS.BatteryManager`](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/)-API verwendet. Diese Implementierung ist komplexer als die iOS-Version. Es sind Überprüfungen zur Behandlung fehlender Berechtigungen für den Akku erforderlich:

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

Fügen Sie dieses `[assembly]`-Attribut über der Klasse hinzu (außerhalb der Namespaces, die definiert wurden), einschließlich erforderlicher `using`-Anweisungen:

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

Dieses Attribut registriert die Klasse als eine Implementierung der Schnittstelle `IBattery`, d. h. dass `DependencyService.Get<IBattery>` im freigegebenen Code verwendet werden kann, um eine Instanz davon zu erstellen.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>UWP-Implementierung

Bei der UWP-Implementierung werden die `Windows.Devices.Power`-APIs zum Abrufen von Informationen zum Akkustatus verwendet:

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

Das Attribut `[assembly]` über der Namespacedeklaration registriert die Klasse als eine Implementierung der Schnittstelle `IBattery`, d. h., dass `DependencyService.Get<IBattery>` im freigegebenen Code verwendet werden kann, um eine Instanz davon zu erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementierung in freigegebenem Code

Nachdem die Schnittstelle nun für jede Plattform implementiert wurde, kann die freigegebene Anwendung geschrieben werden, um diese nutzen zu können. Die Anwendung besteht aus einer Seite mit einer Schaltfläche, über die der Text mit dem aktuellen Akkustatus aktualisiert wird, wenn sie angeklickt wird. Sie verwendet `DependencyService`, um eine Instanz der Schnittstelle `IBattery` abzurufen. Zur Laufzeit ist diese Instanz die plattformspezifische Implementierung, die über Vollzugriff auf das native SDK verfügt.

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

Durch das Ausführen dieser Anwendung unter iOS, Android oder auf der UWP und durch Klicken auf die Schaltfläche wird der Text der Schaltfläche mit dem aktuellen Energiestatus des Geräts aktualisiert.

![](battery-info-images/battery.png "Beispiel für den Akkustatus")


## <a name="related-links"></a>Verwandte Links

- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/)
- [Using DependencyService (Verwenden von DependencyService (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
