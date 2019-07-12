---
title: Dependency Injection
description: In diesem Kapitel wird erläutert, wie die mobile app "eshoponcontainers" Dependency Injection um konkrete Typen aus dem Code zu entkoppeln, von denen diese Typen abhängt.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 97b95ccb3e756f02c945adc63b9e173a9f9e0226
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832685"
---
# <a name="dependency-injection"></a>Dependency Injection

In der Regel ein Klassenkonstruktor wird immer dann aufgerufen, wenn ein Objekt zu instanziieren, und alle Werte, die das Objekt muss als Argumente an den Konstruktor übergeben werden. Dies ist ein Beispiel für Abhängigkeitsinjektion und insbesondere ist bekannt als *Konstruktorinjektion*. Die Abhängigkeiten des Objekts benötigten werden an den Konstruktor eingefügt.

Durch Angeben von Abhängigkeiten als Schnittstellentypen, können Abhängigkeitsinjektion Entkopplung der konkreten Typen, aus dem Code, von der diese Typen abhängt. Er verwendet im Allgemeinen ein Container, der eine Liste der Registrierungen und Zuordnungen zwischen Schnittstellen und abstrakten Typen enthält, und die konkrete Typen, die implementieren oder erweitern diese Typen.

Es gibt auch andere Arten von Abhängigkeitsinjektion, z. B. *Eigenschaft-Setter-Injektion*, und *Methodenaufruf-Injektionen*, aber sie sind weniger häufig sichtbar. Aus diesem Grund in diesem Kapitel konzentriert sich ausschließlich auf Konstruktorinjektion mit DI-Containern ausführen.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Einführung in Abhängigkeitsinjektion

Abhängigkeitsinjektion ist eine spezielle Version des Musters Inversion of Control (IoC) zu, in dem der Prozess des Abrufens der erforderlichen Abhängigkeit ist der Aufgabe, die umgekehrt werden. Über Dependency Injection ist eine weitere Klasse für die Injektion von Abhängigkeiten in ein Objekt zur Laufzeit verantwortlich ist. Das folgende Codebeispiel zeigt die `ProfileViewModel` Klasse ist so strukturiert, wenn die Abhängigkeitsinjektion verwenden:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

Die `ProfileViewModel` Konstruktor empfängt eine `IOrderService` Instanz als Argument von einer anderen Klasse eingefügt. Die einzige Abhängigkeit in der `ProfileViewModel` Klasse, die auf dem Schnittstellentyp ist. Aus diesem Grund die `ProfileViewModel` -Klasse hat keine Kenntnis der Klasse, die verantwortlich für die Instanziierung ist die `IOrderService` Objekt. Die Klasse, die verantwortlich für die Instanziierung ist die `IOrderService` Objekt aus, und das Einfügen in die `ProfileViewModel` Klasse, wird als bezeichnet die *Dependency Injection-Container*.

Dependency Injection-Containern verringern Sie die Kopplung zwischen Objekten durch eine Funktion zum Instanziieren von Klasseninstanzen und verwalten ihre Lebensdauer, die auf Basis der Konfiguration des Containers bereitstellen. Während der Erstellung Objekte fügt der Container um Abhängigkeiten, die das Objekt erfordert hinein. Wenn diese Abhängigkeiten noch nicht erstellt wurden, wird der Container erstellt und ihre Abhängigkeiten zuerst aufgelöst.

> [!NOTE]
> Abhängigkeitsinjektion kann auch manuell mithilfe von Factorys implementiert werden. Verwenden einen Container bietet jedoch zusätzliche Funktionen wie z. B. die Verwaltung der Lebensdauer und die Registrierung über die Assembly, die Überprüfung.

Es gibt mehrere Vorteile gegenüber der Verwendung von Dependency Injection-Container:

-   Ein Container besteht keine Notwendigkeit für eine Klasse zum Suchen von Abhängigkeiten und verwalten ihre Lebensdauer.
-   Ein Container ermöglicht die Zuordnung von implementierten Abhängigkeiten ohne Auswirkungen auf die Klasse.
-   Ein Container ermöglicht Prüfbarkeit indem Abhängigkeiten für die Mocks erstellt werden.
-   Ein Container erhöht, dass Verwaltbarkeit können neue Klassen in den app ganz einfach hinzugefügt werden.

Im Rahmen einer Xamarin.Forms-app, die MVVM verwendet, wird ein Container für Abhängigkeitsinjektion in der Regel für das Registrieren und Auflösen von anzeigemodelle, und klicken Sie zum Registrieren von Diensten und sie dann in Ansichtsmodelle einzuschleusen verwendet werden.

Viele Dependency Injection-Container stehen zur Verfügung, mit der mobilen eShopOnContainers-app, die Verwendung von Autofac zum Verwalten der Instanziierung des Ansichtsmodells und zum Verarbeiten von Klassen in der app. Autofac erleichtert das Erstellen von lose verbundenen apps und bietet alle Features finden Sie häufig in Dependency Injection-Containern, einschließlich Methoden zum Registrieren von Typmappings und Objektinstanzen, Objekte zu finden, Verwalten der Lebensdauer von Objekten und einfügen abhängige Objekte in den Konstruktoren von Objekten, die sie aufgelöst wird. Weitere Informationen zu Autofac, finden Sie unter [Autofac](http://autofac.readthedocs.io/en/latest/index.html) auf readthedocs.io.

In Autofac die `IContainer` Schnittstelle stellt den Container für Abhängigkeitsinjektion bereit. Abbildung 3-1 zeigt die Abhängigkeiten auf, wenn Sie diesen Container verwenden, die instanziiert ein `IOrderService` -Objekt und fügt ihn in das `ProfileViewModel` Klasse.

![](dependency-injection-images/dependencyinjection.png "Beispiel für die Abhängigkeiten bei Verwendung der Abhängigkeitsinjektion")

**Abbildung 3-1:** Abhängigkeiten, die bei Verwendung der Abhängigkeitsinjektion

Zur Laufzeit muss der Container, die Implementierung von der `IOrderService` Schnittstelle, die es sollte zu instanziieren, bevor sie instanziieren kann eine `ProfileViewModel` Objekt. Dies umfasst:

-   Der Container, die Entscheidung, wie ein Objekt zu instanziieren, die implementiert die `IOrderService` Schnittstelle. Dies bezeichnet man als *Registrierung*.
-   Der Container mit dem Instanziieren des Objekts, das implementiert die `IOrderService` -Schnittstelle, und die `ProfileViewModel` Objekt. Dies bezeichnet man als *Auflösung*.

Schließlich wird die app mithilfe von abgeschlossen der `ProfileViewModel` -Objekt, und es für die Garbagecollection zur Verfügung stehen. An diesem Punkt muss der Garbage Collector von dispose der `IOrderService` Instanz, wenn Sie andere Klassen nicht dieselbe Instanz gemeinsam verwenden.

> [!TIP]
> Schreiben Sie Unabhängigkeit von der Container-Code. Versuchen Sie immer Unabhängigkeit von der Container-Code zum Entkoppeln der app aus dem bestimmten verwendeten Container schreiben.

## <a name="registration"></a>Registrierung

Bevor die Abhängigkeiten in ein Objekt eingefügt werden können, müssen die Typen der Abhängigkeiten zunächst mit dem Container registriert werden. Registrieren einen Typ in der Regel umfasst, übergeben dem Container, eine Schnittstelle und einen konkreten Typ, der die Schnittstelle implementiert.

Es gibt zwei Möglichkeiten zum Registrieren von Typen und Objekten in den Container mithilfe von Code aus:

-   Registrieren Sie einen Typ oder eine Zuordnung mit dem Container ein. Wenn erforderlich, wird der Container eine Instanz des angegebenen Typs erstellt.
-   Registrieren Sie ein vorhandenes Objekt im Container als Singleton. Wenn erforderlich, wird der Container einen Verweis auf das vorhandene Objekt zurückgeben.

> [!TIP]
> Dependency Injection-Containern sind nicht immer geeignet ist. Abhängigkeitsinjektion führt zusätzliche Komplexität und die Anforderungen, die entsprechenden oder nützlich für kleine apps möglicherweise nicht. Wenn eine Klasse keine Abhängigkeiten besitzt oder nicht für andere Typen abhängig ist, kann es nicht sinnvoll sein, es in den Container zu platzieren. Darüber hinaus, wenn eine Klasse hat ein einzelnen Satz von Abhängigkeiten, die für den Typ sind und sich nie ändert, es nicht sinnvoll, es in den Container zu platzieren.

Die Registrierung von Typen, die Abhängigkeitsinjektion benötigen, die in einer einzelnen Methode in einer app ausgeführt werden soll, und diese Methode aufgerufen werden soll, früh im Lebenszyklus der app, um sicherzustellen, dass die app die Abhängigkeiten zwischen den Klassen bekannt ist. In der mobilen app von eShopOnContainers erfolgt dies durch die `ViewModelLocator` Klasse, welche Builds den `IContainer` -Objekt und ist die einzige Klasse in der app, die einen Verweis auf dieses Objekt enthält. Im folgenden Codebeispiel wird veranschaulicht, wie die "eshoponcontainers" mobile app deklariert die `IContainer` -Objekt in der `ViewModelLocator` Klasse:

```csharp
private static IContainer _container;
```

Typen und Instanzen registriert sind, der `RegisterDependencies` -Methode in der die `ViewModelLocator` Klasse. Dies wird erreicht, indem Sie zunächst eine `ContainerBuilder` -Instanz, die im folgenden Codebeispiel gezeigt wird:

```csharp
var builder = new ContainerBuilder();
```

Typen und Instanzen registriert werden, klicken Sie dann die `ContainerBuilder` -Objekt, und im folgenden Codebeispiel wird veranschaulicht, die am häufigsten verwendete Form typregistrierung:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

Die `RegisterType` Methode, die hier gezeigten ordnet einen Schnittstellentyp in einen konkreten Typ. Es teilt dem Container zum Instanziieren einer `RequestProvider` -Objekt, wenn es sich um ein Objekt instanziiert, die eine Einfügung von erfordert einen `IRequestProvider` über einen Konstruktor.

Konkrete Typen können auch direkt ohne eine Zuordnung von einem Schnittstellentyp registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
builder.RegisterType<ProfileViewModel>();
```

Wenn die `ProfileViewModel` Typ aufgelöst wurde, der Container werden die erforderlichen Abhängigkeiten injizieren.

Autofac ermöglicht auch Registrierung der Instanz, wo ist der Container für die Verwaltung der eines Verweis auf eine Singleton-Instanz eines Typs zuständig. Z. B. im folgenden Codebeispiel wird veranschaulicht, wie den konkreten Typ, um beim Verwenden von die eShopOnContainers-mobile-app registriert eine `ProfileViewModel` -Instanz benötigt einen `IOrderService` Instanz:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

Die `RegisterType` Methode, die hier gezeigten ordnet einen Schnittstellentyp in einen konkreten Typ. Die `SingleInstance` Methode konfiguriert die Registrierung, sodass alle abhängigen Objekte die gleiche freigegebene Instanz empfängt. Aus diesem Grund nur eine einzige `OrderService` Instanz befindet sich im Container dar, die von Objekten gemeinsam verwendet wird, die eine Einfügung von erfordern eine `IOrderService` über einen Konstruktor.

Registrierung der Instanz kann auch ausgeführt werden, mit der `RegisterInstance` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

Die `RegisterInstance` hier gezeigten Methode erstellt ein neues `OrderMockService` Instanz, und es mit dem Container registriert werden. Aus diesem Grund nur eine einzige `OrderMockService` Instanz vorhanden ist, im Container dar, die von Objekten gemeinsam verwendet wird, die eine Einfügung von erfordern eine `IOrderService` über einen Konstruktor.

Nach Typ und die Instanz-Registrierung, die `IContainer` Objekt muss erstellt werden, die im folgenden Codebeispiel wird veranschaulicht:

```csharp
_container = builder.Build();
```

Aufrufen der `Build` Methode für die `ContainerBuilder` Instanz erstellt einen neuen Dependency Injection-Container, der die Registrierungen enthält, die vorgenommen wurden.

> [!TIP]
> Erwägen Sie eine `IContainer` als unveränderlich. Zwar Autofac bietet ein `Update` -Methode zum Aktualisieren von Registrierungen in einem vorhandenen Container, die das Aufrufen dieser Methode sollte möglichst vermieden werden. Es gibt und die Risiken für einen Container nach der sie erstellt wurde, geändert, insbesondere dann, wenn der Container verwendet wurde. Weitere Informationen finden Sie unter [sollten Sie einen Container als unveränderlich](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) auf readthedocs.io.

<a name="resolution" />

## <a name="resolution"></a>Auflösung

Nachdem Sie ein Typ registriert wurde, kann es gelöst oder als Abhängigkeit eingefügt. Wenn ein Typ aufgelöst wird und der Container eine neue Instanz erstellen muss, fügt es alle Abhängigkeiten in der Instanz.

In der Regel, wenn ein Typ aufgelöst wird, geschieht eines von drei Dingen:

1.  Wenn der Typ noch nicht registriert wurde, wird der Container eine Ausnahme ausgelöst.
1.  Der Container zurückgegeben, wenn der Typ als Singleton registriert wurde, die Singleton-Instanz. Ist dies beim ersten, die für der Typ aufgerufen wird, wird der Container bei Bedarf erstellt und verwaltet einen Verweis darauf.
1.  Wenn der Typ als Singleton registriert wurde noch nicht, wird der Container gibt eine neue Instanz, und keinen Verweis darauf behalten.

Das folgende Codebeispiel zeigt die `RequestProvider` Typ, der zuvor bei Autofac registriert wurde aufgelöst werden kann:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

In diesem Beispiel wird Autofac zum Auflösen von des konkreten Typs für aufgefordert die `IRequestProvider` Typs führen, sowie alle Abhängigkeiten. In der Regel die `Resolve` Methode wird aufgerufen, wenn eine Instanz eines bestimmten Typs erforderlich ist. Informationen zur Steuerung der Lebensdauer von aufgelösten Objekte finden Sie unter [verwalten die Lebensdauer des aufgelöst Objekte](#managing_the_lifetime_of_resolved_objects).

Im folgenden Codebeispiel wird veranschaulicht, wie die mobile app "eshoponcontainers" View Model-Typen sowie deren Abhängigkeiten instanziiert:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

In diesem Beispiel wird Autofac zum Auflösen der ansichtsmodelltyp für ein Modell für die angeforderte Ansicht aufgefordert, und der Container werden auch alle Abhängigkeiten aufgelöst. Beim Auflösen der `ProfileViewModel` Typ die Abhängigkeit aufgelöst wird ein `IOrderService` Objekt. Folglich Autofac zuerst erstellt ein `OrderService` Objekt und übergibt diese an den Konstruktor der `ProfileViewModel` Klasse. Weitere Informationen, wie die "eshoponcontainers" mobile app Ansicht erstellt Modelle und ordnet diese zu Ansichten finden Sie [automatisch ein Ansichtsmodell mit einem View Model-Locator erstellen](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> Registrieren und Auflösen von Typen mit einem Container verfügt über die Leistung aufgrund des Containers mithilfe der Reflektion für jeden Typ erstellen, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der app wiederhergestellt wird, sind. Wenn viele oder deep-Abhängigkeiten vorhanden sind, kann die Kosten für die Erstellung erheblich erhöhen.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>Verwalten der Lebensdauer der aufgelösten Objekte

Nach der Registrierung eines Typs ist das Standardverhalten für Autofac, erstellen Sie eine neue Instanz des registrierten Typs jedes Mal den Typ behoben ist, oder wenn der Dependency-Mechanismus Instanzen in anderen Klassen einfügt. In diesem Szenario enthalten nicht der Container einen Verweis auf das aufgelöste Objekt. Allerdings ist bei der Registrierung einer Instanz das Standardverhalten für Autofac zum Verwalten der Lebensdauer des Objekts als Singleton. Aus diesem Grund bleibt die Instanz im Bereich, während der Container im Gültigkeitsbereich befindet, und freigegeben, wenn der Container den Gültigkeitsbereich verlässt und Garbage Collection durchgeführt, oder wenn es sich bei Code explizit den Container entfernt.

Ein Bereich des Autofac-Instanz kann verwendet werden, um das Laufzeitverhalten von Singleton für ein Objekt anzugeben, die Autofac aus einem registrierten Typ erstellt. Bereiche von Autofac-Instanz Verwalten der Lebensdauer von Objekten vom Container instanziiert. Der Standardbereich für die Instanz für die `RegisterType` Methode ist die `InstancePerDependency` Bereich. Allerdings der `SingleInstance` Bereich kann verwendet werden, mit der `RegisterType` -Methode, damit der Container erstellt oder eine Singleton-Instanz eines Typs, beim Aufrufen zurückgibt der `Resolve` Methode. Im folgenden Codebeispiel wird veranschaulicht, wie Autofac dazu angewiesen wird, erstellen Sie eine Singletoninstanz, der die `NavigationService` Klasse:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Beim ersten Aufruf von der `INavigationService` Schnittstelle wird aufgelöst, der Container erstellt ein neues `NavigationService` Objekt aus, und behält einen Verweis darauf. Für alle nachfolgenden Lösungen von der `INavigationService` -Schnittstelle, die der Container gibt einen Verweis auf die `NavigationService` -Objekt, das zuvor erstellt wurde.

> [!NOTE]
> Bereich SingleInstance verwirft erstellten Objekte aus, wenn der Container gelöscht wird.

Autofac enthält zusätzliche Instanz Bereiche. Weitere Informationen finden Sie unter [Instanz Bereich](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) auf readthedocs.io.

## <a name="summary"></a>Zusammenfassung

Abhängigkeitsinjektion ermöglicht die Entkopplung der konkrete Typen aus dem Code, von der diese Typen abhängt. In der Regel verwendet ein Container, der eine Liste der Registrierungen und Zuordnungen zwischen Schnittstellen und abstrakten Typen enthält, und die konkrete Typen, die implementieren oder erweitern diese Typen.

Autofac erleichtert das Erstellen von lose verbundenen apps und bietet alle Features finden Sie häufig in Dependency Injection-Containern, einschließlich Methoden zum Registrieren von Typmappings und Objektinstanzen, Objekte zu finden, Verwalten der Lebensdauer von Objekten und einfügen abhängige Objekte in den Konstruktoren der Objekte, die sie aufgelöst wird.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
