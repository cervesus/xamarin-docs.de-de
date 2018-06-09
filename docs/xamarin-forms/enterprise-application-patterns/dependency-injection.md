---
title: Dependency Injection
description: In diesem Kapitel wird erläutert, wie die mobilen Anwendung für eShopOnContainers Abhängigkeitsinjektion verwendet, um konkrete Typen aus dem Code zu entkoppeln, von denen diese Typen abhängt.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fb225349b9ffb1c950486a817897b3c26c6ffbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242569"
---
# <a name="dependency-injection"></a>Dependency Injection

In der Regel ein Klassenkonstruktor wird aufgerufen, wenn ein Objekt zu instanziieren, und alle Werte, die das Objekt muss als Argumente an den Konstruktor übergeben werden. Dies ist ein Beispiel für abhängigkeiteneinschleusung und ausdrücklich genannt wird *Konstruktoreinfügung*. Das Objekt muss die erforderlichen Abhängigkeiten werden an den Konstruktor eingefügt.

Durch Angeben von Abhängigkeiten als Schnittstellentypen, ermöglicht die Abhängigkeitsinjektion Entkopplung der konkrete Typen aus dem Code, von der diese Typen abhängt. Im Allgemeinen verwendet ein Container, der eine Liste der Registrierungen und Zuordnungen zwischen Schnittstellen und abstrakte Typen enthält, und die konkrete Typen, die implementieren oder erweitern diese Typen.

Es gibt auch andere Arten von Abhängigkeitsinjektion, z. B. *Setter eigenschafteneinschleusung*, und *methodenaufrufeinschleusung*, aber sie sind seltener sichtbar. Aus diesem Grund wird in diesem Kapitel konzentrieren, ausschließlich auf die Konstruktoreinfügung mit einen abhängigkeitseinschleusungscontainer ausführen.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Einführung in die Abhängigkeitsinjektion

Abhängigkeitsinjektion ist eine spezielle Version des Musters Inversion of Control (IoC), in dem der Prozess des Abrufens der erforderlichen Abhängigkeit ist der Aufgabe wird umgekehrt. Mit Abhängigkeitsinjektion ist eine andere Klasse Räumen für Abhängigkeiten in ein Objekt zur Laufzeit verantwortlich. Im folgenden Codebeispiel wird veranschaulicht wie die `ProfileViewModel` Klasse sind bei Verwendung der abhängigkeitseinfügung strukturiert:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

Die `ProfileViewModel` Konstruktor empfängt eine `IOrderService` -Instanz als Argument, von einer anderen Klasse eingefügt. Der einzige Abhängigkeit der `ProfileViewModel` Klasse befindet sich auf den Schnittstellentyp. Aus diesem Grund die `ProfileViewModel` -Klasse hat keine Kenntnis von der Klasse, die verantwortlich für die Instanziierung der `IOrderService` Objekt. Die Klasse, die zum Instanziieren zuständig ist die `IOrderService` -Objekt und das Einfügen in die `ProfileViewModel` Klasse, wird als bezeichnet den *abhängigkeitseinschleusungscontainer*.

Dependency Injection-Containern reduzieren die Kopplung zwischen Objekten durch eine Klasseninstanzen zu instanziieren und verwalten ihre Lebensdauer auf Basis der Konfiguration des Containers ermöglicht. Während der Erstellung Objekte fügt der Container keine Abhängigkeiten, die das Objekt benötigt hinein. Wenn diese Abhängigkeiten noch nicht erstellt wurden, wird der Container erstellt und deren Abhängigkeiten zuerst aufgelöst.

> [!NOTE]
> Abhängigkeitsinjektion kann auch manuell über die Factorys implementiert werden. Verwenden einen Container bietet jedoch zusätzliche Funktionen wie z. B. die Verwaltung der Objektlebensdauer, und die Registrierung über Scan Assembly.

Es gibt mehrere Vorteile gegenüber der Verwendung eines abhängigkeitseinschleusungscontainer:

-   Ein Container beseitigt die Notwendigkeit für eine Klasse, um seine Abhängigkeiten suchen und Verwalten ihrer Lebensdauer.
-   Ein Container ermöglicht die Zuordnung von implementierten Abhängigkeiten ohne Auswirkungen auf die Klasse.
-   Ein Container ermöglicht Prüfbarkeit indem Abhängigkeiten für die Mocks erstellt werden.
-   Ein Container erhöht die Verwaltbarkeit können Sie neue Klassen in der app einfach hinzugefügt werden.

Im Rahmen einer Xamarin.Forms-app, die MVVM verwendet, wird ein abhängigkeitseinschleusungscontainer in der Regel für das Registrieren und Auflösen von Modelle anzeigen, und klicken Sie zum Registrieren von Diensten und Räumen sie in der Modelle anzeigen verwendet werden.

Viele Dependency Injection-Containern stehen zur Verfügung, mit der eShopOnContainers mobilen app Autofac zum Verwalten der Instanziierung des Ansichtsmodell und Klassen in der app service verwenden. Autofac erleichtert das Erstellen von lose verbundenen apps und bietet alle Funktionen, die häufig in Dependency Injection-Containern, einschließlich der Methoden zum Registrieren die Typmappings und Objekt-Instanzen gefunden Objekte zu beheben, verwalten die Objektlebensdauer und einfügen abhängige Objekte in den Konstruktoren von Objekten, die sie aufgelöst wird. Weitere Informationen zu Autofac, finden Sie unter [Autofac](http://autofac.readthedocs.io/en/latest/index.html) auf readthedocs.io.

In Autofac die `IContainer` Schnittstelle stellt die abhängigkeitseinschleusungscontainer bereit. Abbildung 3 – 1 zeigt die Abhängigkeiten, bei Verwendung von diesem Container die instanziiert ein `IOrderService` -Objekt und fügt ihn in die `ProfileViewModel` Klasse.

![](dependency-injection-images/dependencyinjection.png "Beispiel für die Abhängigkeiten bei Verwendung der abhängigkeitseinfügung")

**Abbildung 3 – 1:** Abhängigkeiten bei Verwendung der abhängigkeitseinfügung

Zur Laufzeit muss der Container wissen, welche Implementierung von der `IOrderService` Schnittstelle, die es sollte zu instanziieren, bevor es instanziieren kann ein `ProfileViewModel` Objekt. Dies umfasst:

-   Der Container entscheiden, wie ein Objekt nicht instanziieren, die implementiert die `IOrderService` Schnittstelle. Dies bezeichnet man *Registrierung*.
-   Der Container Instanziieren des Objekts, das implementiert die `IOrderService` -Schnittstelle, und die `ProfileViewModel` Objekt. Dies bezeichnet man *Auflösung*.

Schließlich wird die app wird fertig gestellt mithilfe der `ProfileViewModel` -Objekt, und es zur Verfügung gestellt, für die Garbagecollection. Zu diesem Zeitpunkt sollten der Garbage Collector von Freigeben der `IOrderService` Instanz, wenn andere Klassen nicht dieselbe Instanz gemeinsam verwenden.

> [!TIP]
> Unabhängigkeit von der Container-Code zu schreiben. Versuchen Sie immer so schreiben Sie Code Unabhängigkeit von der Container, um die app aus dem bestimmten Abhängigkeit Container verwendeten zu entkoppeln.

## <a name="registration"></a>Registrierung

Bevor Abhängigkeiten in ein Objekt eingefügt werden können, müssen die Typen der Abhängigkeiten zunächst mit dem Container registriert werden. Registrieren einen Typ in der Regel umfasst, übergeben dem Container, eine Schnittstelle und einen konkreten Typ, der die Schnittstelle implementiert.

Es gibt zwei Methoden zum Registrieren von Typen und Objekte in den Container über Code ein:

-   Registrieren Sie einen Typ oder eine Zuordnung mit dem Container ein. Wenn erforderlich, wird der Container eine Instanz des angegebenen Typs erstellen.
-   Registrieren Sie ein vorhandenes Objekt im Container als Singleton. Wenn erforderlich, wird der Container einen Verweis auf das vorhandene Objekt zurückgeben.

> [!TIP]
> Dependency Injection-Containern sind nicht immer geeignet ist. Abhängigkeitsinjektion führt zusätzliche Komplexität und Anforderungen, die nicht entsprechenden oder zumindest nützlich für kleine apps möglicherweise ein. Wenn eine Klasse keine Abhängigkeiten haben, oder keine Abhängigkeit für andere Typen, kann es nicht sinnvoll sein, in den Container eingefügt werden soll. Darüber hinaus besitzt eine Klasse ein einzelnen Satz von Abhängigkeiten, die in den Typ ganzzahligen sind und sich nie ändert, es möglicherweise nicht sinnvoll, in den Container eingefügt werden soll.

Die Registrierung von Typen, die Abhängigkeitsinjektion erfordern, sollten in einer einzelnen Methode in einer app ausgeführt werden, und diese Methode aufgerufen werden soll, einem frühen Zeitpunkt im Lebenszyklus der app, um sicherzustellen, dass die Anwendung die Abhängigkeiten zwischen den Klassen beachtet. In der mobilen Anwendung für eShopOnContainers erfolgt dies durch die `ViewModelLocator` Klasse, welche Builds den `IContainer` -Objekt und ist die einzige Klasse in der app, die einen Verweis auf dieses Objekt enthält. Im folgenden Codebeispiel wird veranschaulicht, wie die mobilen Anwendung für eShopOnContainers deklariert die `IContainer` Objekt in der `ViewModelLocator` Klasse:

```csharp
private static IContainer _container;
```

Typen und Instanzen registriert sind, der `RegisterDependencies` Methode in der `ViewModelLocator` Klasse. Dies wird erreicht, indem Sie zunächst eine `ContainerBuilder` -Instanz, die im folgenden Codebeispiel gezeigt:

```csharp
var builder = new ContainerBuilder();
```

Typen und Instanzen registriert werden, klicken Sie dann die `ContainerBuilder` -Objekt, und im folgenden Codebeispiel wird veranschaulicht, die am häufigsten verwendete Form des Typs Registrierung:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

Die `RegisterType` Methode, die hier gezeigten ordnet einen Schnittstellentyp in einen konkreten Typ. Sie erfahren, dass den Container zum Instanziieren einer `RequestProvider` -Objekt, wenn ein Objekt instanziiert wird, die eine Einfügung des erfordert eine `IRequestProvider` über einen Konstruktor.

Konkrete Typen können auch direkt ohne eine Zuordnung von einem Schnittstellentyp registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
builder.RegisterType<ProfileViewModel>();
```

Wenn die `ProfileViewModel` Typ behoben wurde, der Container werden die erforderlichen Abhängigkeiten einfügen.

Autofac kann auch Registrierung der Instanz, in dem der Container zuständig für die Verwaltung der eines Verweis auf eine Singletoninstanz eines Typs ist. Z. B. im folgenden Codebeispiel wird veranschaulicht, wie die eShopOnContainers mobile-Anwendung verwendet den konkreten Typ registriert eine `ProfileViewModel` -Instanz erfordert eine `IOrderService` Instanz:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

Die `RegisterType` Methode, die hier gezeigten ordnet einen Schnittstellentyp in einen konkreten Typ. Die `SingleInstance` Methode konfiguriert die Registrierung, sodass alle abhängigen Objekte die gleiche freigegebene Instanz empfängt. Daher nur einen einzigen `OrderService` Instanz ist vorhanden, in dem Container, der von Objekten gemeinsam verwendet wird, die eine Einfügung des erfordern eine `IOrderService` über einen Konstruktor.

Registrierung der Instanz kann auch ausgeführt werden, mit der `RegisterInstance` -Methode, die im folgenden Codebeispiel gezeigt:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

Die `RegisterInstance` hier gezeigten Methode erstellt ein neues `OrderMockService` -Instanz und registriert ihn mit dem Container. Daher nur einen einzigen `OrderMockService` Instanz vorhanden ist, in dem Container, der von Objekten gemeinsam verwendet wird, die eine Einfügung des erfordern eine `IOrderService` über einen Konstruktor.

Befolgen die Registrierung von Typ und die Instanz, die `IContainer` Objekt muss erstellt werden, die im folgenden Codebeispiel wird veranschaulicht:

```csharp
_container = builder.Build();
```

Aufrufen der `Build` Methode für die `ContainerBuilder` Instanz erstellt einen neuen abhängigkeitseinschleusungscontainer, die die Registrierungen enthält, die vorgenommen wurden.

>💡 **Tipp**: sollten Sie eine `IContainer` als unveränderlich. Autofac bietet eine `Update` Methode zum Aktualisieren von Registrierungen in einen vorhandenen Container, der beim Aufrufen dieser Methode sollten möglichst vermieden werden. Es sind Risiken für einen Container nach der sie erstellt wurde, wird geändert, insbesondere, wenn der Container verwendet wurde. Weitere Informationen finden Sie unter [sollten Sie einen Container als unveränderlich](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) auf readthedocs.io.

<a name="resolution" />

## <a name="resolution"></a>Auflösung

Wenn ein Typ registriert wurde, kann es gelöst oder als Abhängigkeit eingefügt. Wenn ein Typ aufgelöst wird, und erstellen Sie eine neue Instanz der Container muss, schleust alle Abhängigkeiten in der Instanz.

In der Regel, wenn ein Typ aufgelöst wird, geschieht eines von drei Dingen:

1.  Wenn der Typ wurde nicht registriert wurde, wird der Container eine Ausnahme ausgelöst.
1.  Wenn der Typ als Singleton registriert wurde, gibt der Container die Singleton-Instanz zurück. Ist dies das erste Mal für der Typ aufgerufen wird, wird der Container wird bei Bedarf erstellt und verwaltet einen Verweis darauf.
1.  Wenn der Typ als Singleton registriert wurden noch nicht, wird der Container gibt eine neue Instanz und einen Verweis darauf nicht verwalten.

Im folgenden Codebeispiel wird veranschaulicht wie die `RequestProvider` Typ, der zuvor bei Autofac registriert wurde aufgelöst werden kann:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

In diesem Beispiel wird Autofac aufgefordert, zum Auflösen des konkreten Typs für die `IRequestProvider` Typs führen, sowie eventuelle weitere Abhängigkeiten. In der Regel die `Resolve` Methode wird aufgerufen, wenn eine Instanz eines bestimmten Typs erforderlich ist. Informationen zum Steuern der Lebensdauer der Objekte aufgelöst, finden Sie unter [Verwalten der Lebensdauer des aufgelöst Objekte](#managing_the_lifetime_of_resolved_objects).

Im folgenden Codebeispiel wird veranschaulicht, wie die mobilen Anwendung für eShopOnContainers Modelltypen anzeigen und die zugehörigen Abhängigkeiten instanziiert:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

In diesem Beispiel Autofac wird aufgefordert, den Modelltyp Ansicht für ein Modell für die angeforderte Ansicht zu beheben, und der Container auch alle Abhängigkeiten aufgelöst wird. Beim Auflösen der `ProfileViewModel` handelt, die Abhängigkeit zu beheben ist ein `IOrderService` Objekt. Aus diesem Grund Autofac zuerst erstellt ein `OrderService` -Objekt und übergibt ihn dann an den Konstruktor des der `ProfileViewModel` Klasse. Weitere Informationen, wie die mobilen Anwendung für eShopOnContainers Ansicht erstellt modelliert und ordnet sie Sichten, finden Sie unter [erstellen automatisch mit einer Ansicht Modell Locator einem Ansichtsmodell](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> Registrieren und Auflösen von Typen mit einem Container verfügt über eine Leistungseinbußen aufgrund des Containers Verwendung von Reflektion für jeden Typ erstellt, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der app wiederhergestellt wird, sind. Wenn viele oder Tiefe Abhängigkeiten vorhanden sind, kann die Kosten der Erstellung erheblich erhöhen.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>Verwalten der Lebensdauer der Objekte aufgelöst

Nach dem Registrieren eines Typs, ist das Standardverhalten für Autofac, erstellen Sie eine neue Instanz des registrierten Typs jedes Mal den Typ aufgelöst werden, oder wenn mit der Abhängigkeit Mechanismus Instanzen injiziert, in anderen Klassen. In diesem Szenario enthalten nicht der Container einen Verweis auf das aufgelöste Objekt. Allerdings wird beim Registrieren einer Instanz das Standardverhalten für Autofac zum Verwalten der Lebensdauer des Objekts als Singleton. Aus diesem Grund werden die Instanz im Gültigkeitsbereich bleibt, während der Container im Gültigkeitsbereich befindet, und wird verworfen, wenn der Container den Gültigkeitsbereich verlässt und Garbage Collection durchgeführt oder wenn der Code explizit den Container frei.

Ein Autofac Instanz Bereich kann verwendet werden, um das Laufzeitverhalten von Singleton für ein Objekt anzugeben, die Autofac aus einem registrierten Typ erstellt. Autofac Instanz Bereiche verwalten, die Objektlebensdauer, die vom Container instanziiert wird. Der Standardbereich für die Instanz für die `RegisterType` Methode ist die `InstancePerDependency` Bereich. Allerdings die `SingleInstance` Bereich verwendet werden kann, mit der `RegisterType` -Methode so, dass der Container erstellt oder eine Singletoninstanz eines Typs, beim Aufrufen zurückgibt der `Resolve` Methode. Im folgenden Codebeispiel wird veranschaulicht, wie Autofac dazu angewiesen wird, erstellen Sie eine Singletoninstanz, der die `NavigationService` Klasse:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Bei der erstmaligen der `INavigationService` Schnittstelle wird aufgelöst, der Container erstellt ein neues `NavigationService` Objekt, und behält einen Verweis darauf. Auf alle nachfolgenden Lösungen von der `INavigationService` -Schnittstelle, die Container gibt einen Verweis auf die `NavigationService` -Objekt, das zuvor erstellt wurde.

> [!NOTE]
> Bereich SingleInstance verwirft erstellten Objekte an, wenn der Container freigegeben wird.

Autofac enthält zusätzliche Instanz Bereiche. Weitere Informationen finden Sie unter [Instanz Bereich](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) auf readthedocs.io.

## <a name="summary"></a>Zusammenfassung

Abhängigkeitsinjektion ermöglicht die Entkopplung der konkrete Typen aus dem Code, von der diese Typen abhängt. In der Regel verwendet ein Container, der eine Liste der Registrierungen und Zuordnungen zwischen Schnittstellen und abstrakte Typen enthält, und die konkrete Typen, die implementieren oder erweitern diese Typen.

Autofac erleichtert das Erstellen von lose verbundenen apps und bietet alle Funktionen, die häufig in Dependency Injection-Containern, einschließlich der Methoden zum Registrieren die Typmappings und Objekt-Instanzen gefunden Objekte zu beheben, verwalten die Objektlebensdauer und einfügen abhängige Objekte in Konstruktoren der Objekte, auf denen, die Sie aufgelöst wird.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
