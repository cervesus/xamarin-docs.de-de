---
title: Überprüfen der Geräteausrichtung
description: In diesem Artikel wird erläutert, wie für die Klasse Xamarin.Forms DependencyService geräteausrichtung von freigegebenem Code den Zugriff auf.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: e21531b7f39d3876d91eea8fa6cb9e409a9deffa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240055"
---
# <a name="checking-device-orientation"></a>Überprüfen der Geräteausrichtung

Dieser Artikel führt Sie verwenden [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) der geräteausrichtung von freigegebenem Code mit den systemeigenen APIs auf jeder Plattform zu überprüfen. Diese exemplarische Vorgehensweise basiert auf dem vorhandenen `DeviceOrientation` Plug-in mit Ali Özgür. Finden Sie unter der [GitHub-Repository](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) für Weitere Informationen.

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; verstehen, wie die Schnittstelle, die im freigegebenen Code erstellt wird.
- **[iOS Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in systemeigenem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in systemeigenem Code zu implementieren.
- **[Uwp-Implementierung](#WindowsImplementation)**  &ndash; erfahren Sie, wie die Schnittstelle in systemeigenem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[Implementieren im freigegebenen Code](#Implementing_in_Shared_Code)**  &ndash; erfahren, wie `DependencyService` in die systemeigene Implementierung von freigegebenem Code aufrufen.

Die Anwendung mit `DependencyService` hat die folgende Struktur:

![](device-orientation-images/orientation-diagram.png "DependencyService Anwendungsstruktur")

> [!NOTE]
> Es ist möglich, erkennen, ob das Gerät im Hochformat oder Querformat im freigegebenen Code befindet, wie gezeigt in [Device Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). In diesem Artikel beschriebene Methode verwendet systemeigene Funktionen, um erhalten weitere Informationen zur Ausrichtung, z. B. ob das Gerät stehend angezeigt wird.

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

Mit der Programmierung für diese Schnittstelle im freigegebenen Code können Xamarin.Forms-app auf der geräteausrichtung APIs auf jeder Plattform zugreifen.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, benötigen einen parameterlosen Konstruktor zur Bearbeitung der `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

In jedem Anwendungsprojekt plattformspezifischen muss die Schnittstelle implementiert werden. Beachten Sie, dass die Klasse über einen parameterlosen Konstruktor verfügt, damit die `DependencyService` neue Instanzen erstellen können:

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

Fügen Sie schließlich diese `[assembly]` Attribut über die Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Dieses Attribut wird die Klasse als eine Implementierung von registriert die `IDeviceOrientation` -Schnittstelle, dies bedeutet, dass `DependencyService.Get<IDeviceOrientation>` im freigegebenen Code zum Erstellen einer Instanz des Zertifikats verwendet werden können.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Der folgende code implementiert `IDeviceOrientation` auf Android-Geräten:

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

Fügen Sie der `[assembly]` Attribut über die Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Dieses Attribut wird die Klasse als eine Implementierung von registriert die `IDeviceOrientaiton` -Schnittstelle, dies bedeutet, dass `DependencyService.Get<IDeviceOrientation>` kann verwendet werden, der gemeinsam verwendeten Code kann eine Instanz davon erstellen.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Universelle Windows-Plattform-Implementierung

Der folgende code implementiert die `IDeviceOrientation` Schnittstelle auf die universelle Windows-Plattform:

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

Hinzufügen der `[assembly]` Attribut über die Klasse (und die außerhalb der Namespaces, die definiert wurden), einschließlich der erforderlichen `using` Anweisungen:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Dieses Attribut wird die Klasse als eine Implementierung von registriert die `DeviceOrientationImplementation` -Schnittstelle, dies bedeutet, dass `DependencyService.Get<IDeviceOrientation>` kann verwendet werden, der gemeinsam verwendeten Code kann eine Instanz davon erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementieren im freigegebenen Code

Wir können jetzt schreiben und Testen von freigegebenen Code, der greift auf die `IDeviceOrientation` Schnittstelle. Diese einfache Seite enthält eine Schaltfläche, die einen eigenen Text basierend auf der geräteausrichtung aktualisiert. Er verwendet die `DependencyService` zum Abrufen einer Instanz von der `IDeviceOrientation` Schnittstelle &ndash; zur Laufzeit werden diese Instanz die Clientplattform-spezifische Implementierung, die uneingeschränkten Zugriff auf die systemeigene SDK verfügt:

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

Diese Anwendung auf iOS, Android oder Windows-Plattformen ausgeführt, und drücken die Schaltfläche "" führt zu der der Text der Schaltfläche Aktualisieren mit der Ausrichtung des Geräts.

![](device-orientation-images/orientation.png "Gerät Ausrichtung-Beispiel")


## <a name="related-links"></a>Verwandte Links

- [Verwenden DependencyService (Beispiel)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
