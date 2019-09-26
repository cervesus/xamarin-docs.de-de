---
title: Einführung in Xamarin.Forms-DependencyService
description: In diesem Artikel wird die Funktionsweise der DependencyService-Klasse von Xamarin.Forms für den Aufruf der nativen Plattformfunktionalität erläutert.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
ms.openlocfilehash: b27b4b0c3c5662c6cc1c2c151dd9ebe1523da3a4
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71198515"
---
# <a name="xamarinforms-dependencyservice-introduction"></a>Einführung in Xamarin.Forms-DependencyService

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

Die [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse ist ein Dienst-Locator, der es Xamarin.Forms-Anwendungen ermöglicht, native Plattformfunktionen aus freigegebenem Code aufzurufen.

Zum Aufrufen der nativen Plattformfunktionalität wird [`DependencyService`](xref:Xamarin.Forms.DependencyService) wie folgt verwendet:

1. Erstellen Sie eine Schnittstelle für die native Plattformfunktionalität in freigegebenem Code. Weitere Informationen finden Sie unter [Erstellen einer Schnittstelle](#create-an-interface).
1. Implementieren Sie die Schnittstelle in den erforderlichen Plattformprojekten. Weitere Informationen finden Sie unter [Implementieren der Schnittstelle in den einzelnen Plattformen](#implement-the-interface-on-each-platform).
1. Registrieren Sie die Plattformimplementierungen mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse. Dadurch können die Plattformimplementierungen zur Laufzeit in Xamarin.Forms gefunden werden. Weitere Informationen finden Sie unter [Registrieren der Plattformimplementierungen](#register-the-platform-implementations).
1. Lösen Sie die Plattformimplementierungen aus freigegebenem Code auf, und rufen Sie diese auf. Weitere Informationen finden Sie unter [Auflösen der Plattformimplementierungen](#resolve-the-platform-implementations).

Im folgenden Diagramm wird gezeigt, wie die nativen Plattformfunktionen in einer Xamarin.Forms-Anwendung aufgerufen werden:

![Übersicht über die Dienstidentifizierung mit der DependencyService-Klasse von Xamarin.Forms](introduction-images/dependency-service.png "DependencyService-Dienstidentifizierung")

## <a name="create-an-interface"></a>Erstellen einer Schnittstelle

Der erste Schritt, um die native Plattformfunktionalität aus freigegebenem Code aufrufen zu können, besteht darin, eine Schnittstelle zu erstellen, die die API für die Interaktion mit der nativen Plattformfunktionalität definiert. Diese Schnittstelle sollte in Ihr Projekt mit freigegebenem Code platziert werden.

Im folgenden Beispiel sehen Sie eine Schnittstelle für eine API, mit der die Ausrichtung eines Geräts abgerufen werden kann:

```csharp
public interface IDeviceOrientationService
{
    DeviceOrientation GetOrientation();
}
```

## <a name="implement-the-interface-on-each-platform"></a>Implementieren der Schnittstelle in den einzelnen Plattformen

Nach dem Erstellen der Schnittstelle, die die API für die Interaktion mit der nativen Plattformfunktionalität definiert, muss die Schnittstelle in den einzelnen Plattformprojekten implementiert werden.

### <a name="ios"></a>iOS

Im folgenden Codebeispiel wird die Implementierung der `IDeviceOrientationService`-Schnittstelle unter iOS veranschaulicht:

```csharp
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            UIInterfaceOrientation orientation = UIApplication.SharedApplication.StatusBarOrientation;

            bool isPortrait = orientation == UIInterfaceOrientation.Portrait ||
                orientation == UIInterfaceOrientation.PortraitUpsideDown;
            return isPortrait ? DeviceOrientation.Portrait : DeviceOrientation.Landscape;
        }
    }
}
```

### <a name="android"></a>Android

Im folgenden Codebeispiel wird die Implementierung der `IDeviceOrientationService`-Schnittstelle unter Android veranschaulicht:

```csharp
namespace DependencyServiceDemos.Droid
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            SurfaceOrientation orientation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = orientation == SurfaceOrientation.Rotation90 ||
                orientation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Im folgenden Codebeispiel wird die Implementierung der `IDeviceOrientationService`-Schnittstelle für die Universelle Windows-Plattform (UWP) veranschaulicht:

```csharp
namespace DependencyServiceDemos.UWP
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ApplicationViewOrientation orientation = ApplicationView.GetForCurrentView().Orientation;
            return orientation == ApplicationViewOrientation.Landscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

## <a name="register-the-platform-implementations"></a>Registrieren der Plattformimplementierungen

Nach dem Implementieren der Schnittstelle in den einzelnen Plattformprojekten müssen die Plattformimplementierungen mit [`DependencyService`](xref:Xamarin.Forms.DependencyService) registriert werden, damit diese in Xamarin.Forms zur Laufzeit gefunden werden können. Dies wird typischerweise mit [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) durchgeführt, was bedeutet, dass der angegebene Typ eine Implementierung der Schnittstelle bereitstellt.

Im folgenden Beispiel wird veranschaulicht, wie mithilfe von [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) die iOS-Implementierung der `IDeviceOrientationService`-Schnittstelle registriert wird:

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DependencyServiceDemos.iOS.DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

In diesem Beispiel wird `DeviceOrientationService` von [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse registriert. Gleichermaßen sollten die Implementierungen der `IDeviceOrientationService`-Schnittstelle auf anderen Plattformen mit der [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)-Klasse registriert werden.

Weitere Informationen zum Registrieren von Plattformimplementierungen mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse finden Sie unter [DependencyService-Registrierung und -Auflösung in Xamarin.Forms](registration-and-resolution.md).

## <a name="resolve-the-platform-implementations"></a>Auflösen der Plattformimplementierungen

Nach der Registrierung von Plattformimplementierungen bei [`DependencyService`](xref:Xamarin.Forms.DependencyService) müssen die Implementierungen vor dem Aufruf aufgelöst werden. Dies erfolgt in der Regel im freigegebenen Code mithilfe der [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode.

Im folgenden Code ist ein Beispiel für den Aufruf der [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode zur Auflösung der `IDeviceOrientationService`-Schnittstelle und den anschließenden Aufruf der `GetOrientation`-Methode dargestellt:

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Alternativ kann dieser Code auf eine einzige Zeile verkürzt werden:

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

Weitere Informationen zum Auflösen von Plattformimplementierungen bei der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse finden Sie unter [DependencyService-Registrierung und -Auflösung in Xamarin.Forms](registration-and-resolution.md).

## <a name="related-links"></a>Verwandte Links

- [DependencyService-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [DependencyService-Registrierung und -Auflösung in Xamarin.Forms](registration-and-resolution.md)
