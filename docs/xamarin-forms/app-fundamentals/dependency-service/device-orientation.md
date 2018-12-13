---
title: Überprüfen der Geräteausrichtung
description: In diesem Artikel wird beschrieben, wie die Klasse „DependencyService“ von Xamarin.Forms verwendet wird, um über freigegebenen Code auf die Geräteausrichtung zuzugreifen.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 52b82033cbd6fe0e1a44f5729c815074852230bf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115418"
---
# <a name="checking-device-orientation"></a>Überprüfen der Geräteausrichtung

In diesem Artikel wird erläutert, wie Sie [`DependencyService`](xref:Xamarin.Forms.DependencyService) mithilfe der nativen APIs für jede Plattform für die Überprüfung der Geräteausrichtung über freigegebenen Code verwenden können. Diese exemplarische Vorgehensweise basiert auf dem vorhandenen `DeviceOrientation`-Plugin von Ali Özgür. Weitere Informationen finden Sie im [GitHub-Repository](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation).

- **[Creating the Interface (Erstellen der Schnittstelle):](#Creating_the_Interface)** Erfahren Sie, wie die Schnittstelle im freigegebenen Code erstellt wird.
- **[iOS-Implementierung:](#iOS_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für iOS implementiert wird.
- **[Android-Implementierung:](#Android_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für Android implementiert wird.
- **[UWP-Implementierung:](#WindowsImplementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für die Universelle Windows-Plattform implementiert wird.
- **[Implementieren in freigegebenem Code:](#Implementing_in_Shared_Code)** Erfahren Sie, wie Sie mit `DependencyService` die native Implementierung von freigegebenem Code aufrufen.

Die App, die `DependencyService` verwendet, hat folgende Struktur:

![](device-orientation-images/orientation-diagram.png "App-Struktur von DependencyService")

> [!NOTE]
> Es kann ermittelt werden, ob sich das Gerät im freigegebenen Code im Hoch- oder Querformat befindet. Dies wird unter [Geräteausrichtung](~/xamarin-forms/user-interface/layouts/device-orientation.md#Reacting_to_Changes_in_Orientation) veranschaulicht. Die in diesem Artikel beschriebene Methode verwendet native Funktionen, um weitere Informationen über die Ausrichtung abzurufen, einschließlich der Information, ob das Gerät nach unten zeigt.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle im freigegebenen Code, die die Funktion ausdrückt, die Sie implementieren möchten. In diesem Beispiel enthält die Schnittstelle eine einzelne Methode:

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

Das Programmieren dieser Schnittstelle im freigegebenen Code ermöglicht der Xamarin.Forms-App den Zugriff auf die APIs für die Geräteausrichtung auf jeder Plattform.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, müssen einen parameterlosen Konstruktor aufweisen, damit sie mit `DependencyService` verwendet werden können.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die Schnittstelle muss in jedem plattformspezifischen App-Projekt implementiert werden. Beachten Sie, dass die Klasse einen parameterlosen Konstruktor aufweist, sodass `DependencyService` neue Instanzen erstellen kann:

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

Fügen Sie schlussendlich das `[assembly]`-Attribut über der Klasse hinzu (außerhalb der Namespaces, die definiert wurden), einschließlich erforderlicher `using`-Anweisungen:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der Schnittstelle `IDeviceOrientation`. `DependencyService.Get<IDeviceOrientation>` kann also im freigegebenen Code verwendet werden, um eine Instanz davon zu erstellen.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Im folgenden Code wird die Methode `IDeviceOrientation` unter Android implementiert:

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

Fügen Sie das `[assembly]`-Attribut über der Klasse hinzu (außerhalb der Namespaces, die definiert wurden), einschließlich erforderlicher `using`-Anweisungen:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der Schnittstelle `IDeviceOrientaiton`. `DependencyService.Get<IDeviceOrientation>` kann also im freigegebenen Code verwendet werden, um eine Instanz davon zu erstellen.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>UWP-Implementierung

Der folgende Code implementiert die Schnittstelle `IDeviceOrientation` auf der Universellen Windows-Plattform:

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

Fügen Sie das `[assembly]`-Attribut über der Klasse hinzu (außerhalb der Namespaces, die definiert wurden), einschließlich erforderlicher `using`-Anweisungen:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der Schnittstelle `DeviceOrientationImplementation`. `DependencyService.Get<IDeviceOrientation>` kann also im freigegebenen Code verwendet werden, um eine Instanz davon zu erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementieren in freigegebenem Code

Nun können Sie freigegebenen Code schreiben und testen, der auf die Schnittstelle `IDeviceOrientation` zugreift. Diese einfache Seite enthält eine Schaltfläche, mit der der eigene Text basierend auf der Geräteausrichtung aktualisiert wird. Sie verwendet `DependencyService`, um eine Instanz der Schnittstelle `IDeviceOrientation` zu erhalten. Zur Laufzeit entspricht diese Instanz der plattformspezifische Implementierung, die über Vollzugriff auf das native SDK verfügt:

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

Durch das Ausführen dieser App unter iOS, Android, auf den Windows-Plattformen und durch Klicken der Schaltfläche wird der Text der Schaltfläche mit der Ausrichtung des Geräts aktualisiert.

![](device-orientation-images/orientation.png "Beispiel für Geräteausrichtung")


## <a name="related-links"></a>Verwandte Links

- [Verwenden von DependencyService (Beispiel)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (sample) (DependencyService (Beispiel))](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
