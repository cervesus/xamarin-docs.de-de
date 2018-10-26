---
title: Überprüfen der Geräteausrichtung
description: In diesem Artikel wird erläutert, wie Sie mit, dass die Klasse Xamarin.Forms DependencyService geräteausrichtung über freigegebenen Code zuzugreifen.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 52b82033cbd6fe0e1a44f5729c815074852230bf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115418"
---
# <a name="checking-device-orientation"></a>Überprüfen der Geräteausrichtung

Dieser Artikel führt Sie mit [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) zum Überprüfen der geräteausrichtung von freigegebenem Code, die mithilfe der systemeigenen APIs auf jeder Plattform. Diese exemplarische Vorgehensweise basiert auf dem vorhandenen `DeviceOrientation` -Plug-in von Ali Özgür. Finden Sie unter den [GitHub-Repository](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) für Weitere Informationen.

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; verstehen, wie Sie die Schnittstelle wird in freigegebenem Code erstellt.
- **[iOS-Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in nativem Code zu implementieren.
- **[UWP-Implementierung](#WindowsImplementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[In freigegebenem Code implementieren](#Implementing_in_Shared_Code)**  &ndash; erfahren Sie, wie Sie mit `DependencyService` , die native Implementierung von freigegebenem Code aufzurufen.

Die Anwendung mit `DependencyService` wird die folgende Struktur aufweisen:

![](device-orientation-images/orientation-diagram.png "DependencyService Anwendungsstruktur")

> [!NOTE]
> Es ist möglich, erkennen, ob das Gerät im Hoch-oder Querformat in freigegebenem Code ist ein, wie in [Geräteausrichtung](~/xamarin-forms/user-interface/layouts/device-orientation.md#Reacting_to_Changes_in_Orientation). Die Methode, die in diesem Artikel beschriebenen verwendet native Funktionen um weitere Informationen zu Ausrichtung, z. B., ob das Gerät, finden Sie verkehrt herum ist zu erhalten.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle im freigegebenen Code, der die Funktionalität ausdrückt, die Sie implementieren möchten. In diesem Beispiel enthält die Schnittstelle für eine einzelne Methode:

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

Mit der Programmierung für diese Schnittstelle in den freigegebenen Code lässt sich auf die Xamarin.Forms-app auf der geräteausrichtung APIs auf jeder Plattform aus.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, müssen einen parameterlosen Konstruktor für die Arbeit mit der `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die Schnittstelle muss in jedem Anwendungsprojekt plattformspezifischen-implementiert werden. Beachten Sie, dass die Klasse über einen parameterlosen Konstruktor hat, damit die `DependencyService` neue Instanzen erstellen können:

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

Fügen Sie abschließend Folgendes `[assembly]` Attribut oberhalb der Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der `IDeviceOrientation` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<IDeviceOrientation>` kann in den freigegebenen Code verwendet werden, um eine Instanz davon erstellen.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Der folgende code implementiert `IDeviceOrientation` unter Android:

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

Fügen Sie Folgendes `[assembly]` Attribut oberhalb der Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der `IDeviceOrientaiton` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<IDeviceOrientation>` kann verwendet werden, der freigegebene Code kann eine Instanz davon erstellen.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementierung der universellen Windows-Plattform

Der folgende code implementiert die `IDeviceOrientation` Schnittstelle für die universelle Windows-Plattform:

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

Hinzufügen der `[assembly]` Attribut oberhalb der Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Dieses Attribut registriert die Klasse als eine Implementierung der `DeviceOrientationImplementation` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<IDeviceOrientation>` kann verwendet werden, der freigegebene Code kann eine Instanz davon erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>In freigegebenem Code implementieren

Nun können wir schreiben und Testen freigegebenen Code, der greift auf die `IDeviceOrientation` Schnittstelle. Diese einfache Seite enthält eine Schaltfläche, die einen eigenen Text basierend auf der geräteausrichtung aktualisiert. Er verwendet die `DependencyService` zum Abrufen einer Instanz von der `IDeviceOrientation` Schnittstelle &ndash; zur Laufzeit werden diese Instanz die plattformspezifischen Implementierungen, die uneingeschränkten Zugriff auf das systemeigene SDK verfügt:

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

Diese Anwendung unter iOS, Android oder den Windows-Plattformen ausgeführt und auf die Schaltfläche führt der Text der Schaltfläche Aktualisieren mit der Ausrichtung des Geräts.

![](device-orientation-images/orientation.png "Ausrichtung-Gerätebeispiel")


## <a name="related-links"></a>Verwandte Links

- [Verwenden von DependencyService (Beispiel)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
