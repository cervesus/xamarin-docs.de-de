---
title: DependencyService-Registrierung und -Auflösung in Xamarin.Forms
description: In diesem Artikel wird die Funktionsweise der DependencyService-Klasse von Xamarin.Forms für den Aufruf der nativen Plattformfunktionalität erläutert.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
ms.openlocfilehash: aab5f56594f9b9b81acb9c447eee238d151bd533
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832193"
---
# <a name="xamarinforms-dependencyservice-registration-and-resolution"></a>DependencyService-Registrierung und -Auflösung in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/DependencyService)

Wenn Sie [`DependencyService`](xref:Xamarin.Forms.DependencyService) von Xamarin.Forms verwenden, um die native Plattformfunktionalität aufzurufen, müssen Plattformimplementierungen mit `DependencyService` registriert und dann aus dem freigegebenen Code aufgelöst werden, um sie aufzurufen.

## <a name="register-platform-implementations"></a>Registrieren von Plattformimplementierungen

Plattformimplementierungen müssen mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse registriert werden, damit diese in Xamarin.Forms zur Laufzeit gefunden werden können.

Die Registrierung kann bei der [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)-Klasse oder über die [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methode erfolgen.

> [!IMPORTANT]
> Releasebuilds von UWP-Projekten, die nativen .NET-Kompilierung verwenden, sollten Plattformimplementierungen mit den [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methoden registrieren.

### <a name="registration-by-attribute"></a>Registrierung mithilfe eines Attributs

Mithilfe von [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) kann eine Plattformimplementierung mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse registriert werden. Das Attribut gibt an, dass der festgelegte Typ eine konkrete Implementierung der Schnittstelle bereitstellt.

Im folgenden Beispiel wird veranschaulicht, wie mithilfe von [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) die iOS-Implementierung der `IDeviceOrientationService`-Schnittstelle registriert wird:

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DeviceOrientationService))]
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

In diesem Beispiel wird `DeviceOrientationService` von [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse registriert. Dies führt dazu, dass der konkrete Typ mit der von ihm implementierten Schnittstelle registriert wird.

Gleichermaßen sollten die Implementierungen der `IDeviceOrientationService`-Schnittstelle auf anderen Plattformen mit der [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)-Klasse registriert werden.

> [!NOTE]
> Die Registrierung mit [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) erfolgt auf Namespaceebene.

### <a name="registration-by-method"></a>Registrierung mithilfe einer Methode

Mithilfe von [`DependencyService.Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methoden kann eine Plattformimplementierung mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse registriert werden.

Im folgenden Beispiel wird veranschaulicht, wie mithilfe einer [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methode die iOS-Implementierung der `IDeviceOrientationService`-Schnittstelle registriert wird:

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
                global::Xamarin.Forms.Forms.Init();
                LoadApplication(new App());
                DependencyService.Register<IDeviceOrientationService, DeviceOrientationService>();
                return base.FinishedLaunching(app, options);
        }
}
```

In diesem Beispiel wird mit der [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methode der konkrete Typ, `DeviceOrientationService`, für die `IDeviceOrientationService`-Schnittstelle registriert. Alternativ kann die [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methode überladen werden, um eine Plattformimplementierung mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse zu registrieren:

```csharp
DependencyService.Register<DeviceOrientationService>();
```

In diesem Beispiel wird `DeviceOrientationService` von der [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methode mit der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse registriert. Dies führt dazu, dass der konkrete Typ mit der von ihm implementierten Schnittstelle registriert wird.

Gleichermaßen können die Implementierungen der `IDeviceOrientationService`-Schnittstelle auf anderen Plattformen mit den [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methoden registriert werden.

> [!IMPORTANT]
> Die Registrierung mit den [`Register`](xref:Xamarin.Forms.DependencyService.Register*)-Methoden muss in Plattformprojekten erfolgen, bevor die von der Plattformimplementierung bereitgestellte Funktionalität aus dem freigegebenen Code aufgerufen wird.

## <a name="resolve-the-platform-implementations"></a>Auflösen der Plattformimplementierungen

Plattformimplementierungen müssen zunächst aufgelöst werden, bevor sie aufgerufen werden. Dies erfolgt in der Regel im freigegebenen Code mithilfe der [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode. Aber auch mit der [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)-Methode kann dies erreicht werden.

Standardmäßig löst [`DependencyService`](xref:Xamarin.Forms.DependencyService) nur Plattformimplementierungen auf, die parameterlose Konstruktoren besitzen. In Xamarin.Forms kann jedoch eine Methode für die Auflösung von Abhängigkeiten eingefügt werden, die für die Auflösung von Plattformimplementierungen einen Container für die Dependency Injection oder Factorymethoden verwendet. Dieser Ansatz kann zur Auflösung von Plattformimplementierungen verwendet werden, die über Konstruktoren mit Parametern verfügen. Weitere Informationen finden Sie unter [Auflösung von Abhängigkeiten in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

> [!IMPORTANT]
> Wenn eine Plattformimplementierung, die nicht mit [`DependencyService`](xref:Xamarin.Forms.DependencyService) registriert wurde, aufgerufen wird, tritt eine `NullReferenceException` auf.

### <a name="resolve-using-the-getlttgt-method"></a>Auflösen mit der Get&lt;T&gt;-Methode

Die [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode ruft zur Laufzeit die Plattformimplementierung der `T`-Schnittstelle ab und erstellt eine Instanz davon als Singleton. Diese Instanz bleibt während der gesamten Lebensdauer der Anwendung erhalten, und alle nachfolgenden Aufrufe zur Auflösung derselben Plattformimplementierung rufen dieselbe Instanz ab.

Im folgenden Code ist ein Beispiel für den Aufruf der [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode zur Auflösung der `IDeviceOrientationService`-Schnittstelle und den anschließenden Aufruf der `GetOrientation`-Methode dargestellt:

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Alternativ kann dieser Code auf eine einzige Zeile verkürzt werden:

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> Die [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode erstellt standardmäßig eine Instanz der Plattformimplementierung der `T`-Schnittstelle als Singleton. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Verwalten der Lebensdauer von aufgelösten Objekten](#manage-the-lifetime-of-resolved-objects).

### <a name="resolve-using-the-resolvelttgt-method"></a>Auflösen mit der Resolve&lt;T&gt;-Methode

Die [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)-Methode ruft zur Laufzeit die Plattformimplementierung der `T`-Schnittstelle mithilfe einer Abhängigkeitsauflösungsmethode ab, die in Xamarin.Forms mit der [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver)-Klasse eingefügt wurde. Wenn keine Abhängigkeitsauflösungsmethode in Xamarin.Forms eingefügt wurde, greift die `Resolve<T>`-Methode auf den Aufruf der [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode zurück, um die Plattformimplementierung abzurufen. Weitere Informationen zum Einfügen einer Abhängigkeitsauflösungsmethode in Xamarin.Forms finden Sie unter [Abhängigkeitsauflösung in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

Im folgenden Code ist ein Beispiel für den Aufruf der [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)-Methode zur Auflösung der `IDeviceOrientationService`-Schnittstelle und den anschließenden Aufruf der `GetOrientation`-Methode dargestellt:

```csharp
IDeviceOrientationService service = DependencyService.Resolve<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Alternativ kann dieser Code auf eine einzige Zeile verkürzt werden:

```csharp
DeviceOrientation orientation = DependencyService.Resolve<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> Wenn die [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)-Methode auf den Aufruf der [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*)-Methode zurückgreift, wird standardmäßig eine Instanz der Plattformimplementierung der `T`-Schnittstelle als Singleton erstellt. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Verwalten der Lebensdauer von aufgelösten Objekten](#manage-the-lifetime-of-resolved-objects).

## <a name="manage-the-lifetime-of-resolved-objects"></a>Verwalten der Lebensdauer von aufgelösten Objekten

Das Standardverhalten der [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse ist die Auflösung von Plattformimplementierungen als Singletons. Daher bleiben Plattformimplementierungen während der gesamten Lebensdauer einer Anwendung erhalten.

Dieses Verhalten wird mit dem optionalen Argument [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) in den Methoden [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) und [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) festgelegt. Die [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget)-Enumeration definiert zwei Elemente:

- `GlobalInstance`: Die Plattformimplementierung wird als Singleton zurückgegeben.
- `NewInstance`: Eine neue Instanz der Plattformimplementierung wird zurückgegeben. Die Verwaltung der Lebensdauer der Plattformimplementierungsinstanz wird dann von der Anwendung übernommen.

Die Methoden [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) und [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) setzen beide ihre optionalen Argumente auf [`DependencyFetchTarget.GlobalInstance`](xref:Xamarin.Forms.DependencyFetchTarget), sodass Plattformimplementierungen immer als Singletons auflöst werden. Dieses Verhalten kann geändert werden, um neue Instanzen von Plattformimplementierungen zu erstellen, indem [`DependencyFetchTarget.NewInstance`](xref:Xamarin.Forms.DependencyFetchTarget) als Argumente für die Methoden `Get<T>` und `Resolve<T>` angegeben werden:

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
```

In diesem Beispiel wird mit [`DependencyService`](xref:Xamarin.Forms.DependencyService) eine neue Instanz der Plattformimplementierung für die `ITextToSpeechService`-Schnittstelle erstellt. Durch alle nachfolgenden Aufrufe zum Auflösen von `ITextToSpeechService` werden ebenfalls neue Instanzen erstellt.

Wenn immer wieder eine neue Instanz einer Plattformimplementierung erstellt wird, hat dies zur Folge, dass die Verwaltung der Lebensdauer dieser Instanzen durch die Anwendung erfolgt. Daher wird empfohlen, wenn Sie ein in einer Plattformimplementierung definiertes Ereignis abonnieren, das Ereignis wieder abzubestellen, sobald die Plattformimplementierung nicht mehr benötigt wird. Darüber hinaus kann es für Plattformimplementierungen erforderlich sein, `IDisposable` zu implementieren und die entsprechenden Ressourcen in `Dispose`-Methoden zu bereinigen. In der Beispielanwendung wird dieses Szenario in der `TextToSpeechService`-Plattformimplementierungen veranschaulicht.

Wenn eine Plattformimplementierung, die `IDisposable` implementiert, von einer Anwendung nicht mehr benötigt wird, sollte diese die `Dispose`-Implementierung des Objekts aufrufen. Dies kann beispielsweise mit einer `using`-Anweisung erreicht werden:

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
using (service as IDisposable)
{
        await service.SpeakAsync("Hello world");
}
```

In diesem Beispiel verwirft die `using`-Anweisung nach dem Aufruf der `SpeakAsync`-Methode automatisch das Plattformimplementierungsobjekt. Dies führt dazu, dass die `Dispose`-Methode des Objekts aufgerufen wird, wodurch die erforderliche Bereinigung erfolgt.

Weitere Informationen zum Aufrufen der `Dispose`-Methode eines Objekts finden Sie unter [Verwenden von Objekten, die IDisposable implementieren](/dotnet/standard/garbage-collection/using-objects).

## <a name="related-links"></a>Verwandte Links

- [DependencyService-Demos (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/DependencyService)
- [Abhängigkeitsauflösung in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md)
