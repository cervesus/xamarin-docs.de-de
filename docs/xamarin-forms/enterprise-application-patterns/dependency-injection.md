---
title: Dependency Injection
description: In diesem Kapitel wird erläutert, wie der eshoponcontainers-Mobile App eine Abhängigkeitsinjektion verwendet, um konkrete Typen aus dem Code zu entkoppeln, der von diesen Typen abhängt.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6cbcd6612323acc8619004d56fff82461e005e9e
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529134"
---
# <a name="dependency-injection"></a>Dependency Injection

In der Regel wird ein Klassenkonstruktor aufgerufen, wenn ein Objekt instanziiert wird. alle Werte, die das Objekt benötigt, werden als Argumente an den Konstruktor übergeben. Dies ist ein Beispiel für eine Abhängigkeitsinjektion, insbesondere als *Konstruktorinjektion*bezeichnet. Die Abhängigkeiten, die das Objekt benötigt, werden in den Konstruktor eingefügt.

Durch die Angabe von Abhängigkeiten als Schnittstellentypen ermöglicht die Abhängigkeitsinjektion das Entkoppeln der konkreten Typen aus dem Code, der von diesen Typen abhängig ist. Im Allgemeinen wird ein Container verwendet, der eine Liste von Registrierungen und Zuordnungen Zwischenschnitt stellen und abstrakten Typen sowie die konkreten Typen enthält, die diese Typen implementieren oder erweitern.

Es gibt auch andere Arten von Abhängigkeitsinjektion, wie z. b. Einschleusung von *Eigenschaften*und *Methodenaufrufe*, aber Sie sind seltener sichtbar. Daher konzentriert sich dieses Kapitel ausschließlich auf die Durchführung der Konstruktorinjektion mit einem Abhängigkeits Injection-Container.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Einführung in die Abhängigkeitsinjektion

Die Abhängigkeitsinjektion ist eine spezialisierte Version des IOC-Musters (Inversion of Control), bei der das Problem, das invertiert wird, das Abrufen der erforderlichen Abhängigkeit ist. Bei der Abhängigkeitsinjektion ist eine andere Klasse für das Einfügen von Abhängigkeiten in ein Objekt zur Laufzeit verantwortlich. Das folgende Codebeispiel zeigt, wie `ProfileViewModel` die-Klasse bei der Verwendung von Abhängigkeitsinjektion strukturiert ist:

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

Der `ProfileViewModel` Konstruktor empfängt eine `IOrderService` Instanz als Argument, das von einer anderen Klasse eingefügt wurde. Die einzige Abhängigkeit in der `ProfileViewModel` -Klasse ist der Schnittstellentyp. Daher verfügt die `ProfileViewModel` -Klasse nicht über Kenntnisse über die-Klasse, die für die Instanziierung `IOrderService` des Objekts zuständig ist. Die Klasse, die für das Instanziieren des `IOrderService` -Objekts und das Einfügen in die `ProfileViewModel` -Klasse zuständig ist, wird als *Abhängigkeits Injection-Container*bezeichnet.

Abhängigkeits einschleusungs Container reduzieren die Kopplung zwischen Objekten, indem Sie eine Funktion zum Instanziieren von Klassen Instanzen und zum Verwalten Ihrer Lebensdauer basierend auf der Konfiguration des Containers bereitstellen. Während der Objekt Erstellung fügt der Container alle Abhängigkeiten ein, die das Objekt benötigt. Wenn diese Abhängigkeiten noch nicht erstellt wurden, werden die Abhängigkeiten vom Container erstellt und aufgelöst.

> [!NOTE]
> Die Abhängigkeitsinjektion kann auch manuell mithilfe von Factorys implementiert werden. Die Verwendung eines Containers bietet jedoch zusätzliche Funktionen wie die Verwaltung der Lebensdauer und die Registrierung über die Assemblyüberprüfung.

Die Verwendung eines Containers für die Abhängigkeitsinjektion bietet mehrere Vorteile:

- Mit einem Container entfällt die Notwendigkeit, dass eine Klasse ihre Abhängigkeiten findet und ihre Lebensdauer verwaltet.
- Ein Container ermöglicht die Zuordnung implementierter Abhängigkeiten, ohne dass sich dies auf die Klasse auswirkt.
- Ein Container vereinfacht die Prüfbarkeit, indem Abhängigkeiten ermöglicht werden.
- Ein Container steigert die Verwaltbarkeit, da neue Klassen problemlos der app hinzugefügt werden können.

Im Kontext einer xamarin. Forms-APP, die MVVM verwendet, wird in der Regel ein Container für die Abhängigkeitsinjektion verwendet, um Ansichts Modelle zu registrieren und aufzulösen sowie um Dienste zu registrieren und in Ansichts Modelle einzuschleusen.

Es sind viele Container für die Abhängigkeitsinjektion verfügbar, wobei die eshoponcontainers-Mobile App verwenden von autofac zum Verwalten der Instanziierung von Ansichts Modell-und Dienst Klassen in der app. Autofac vereinfacht das entwickeln lose gekoppelter apps und bietet alle Features, die häufig in Abhängigkeits einschleusungs Containern gefunden werden, einschließlich Methoden zum Registrieren von Typzuordnungen und Objektinstanzen, zum Auflösen von Objekten, zum Verwalten von Objekt Lebensdauern und einfügen. abhängige Objekte in Konstruktoren von Objekten, die aufgelöst werden. Weitere Informationen zu autofac finden Sie unter [autofac](http://autofac.readthedocs.io/en/latest/index.html) auf readthedocs.IO.

In autofac stellt die `IContainer` -Schnittstelle den Container für die Abhängigkeitsinjektion bereit. In Abbildung 3-1 werden die Abhängigkeiten angezeigt, wenn dieser Container verwendet wird, der `IOrderService` ein Objekt instanziiert und in `ProfileViewModel` die-Klasse einfügt.

![](dependency-injection-images/dependencyinjection.png "Abhängigkeits Beispiel bei Verwendung von Abhängigkeitsinjektion")

**Abbildung 3-1:** Abhängigkeiten bei der Verwendung von Abhängigkeitsinjektion

Zur Laufzeit muss der Container wissen, welche Implementierung der `IOrderService` Schnittstelle instanziiert werden soll, bevor er ein `ProfileViewModel` -Objekt instanziieren kann. Dies umfasst Folgendes:

- Der Container, der entscheidet, wie ein Objekt instanziiert werden `IOrderService` soll, das die-Schnittstelle implementiert. Dies wird als *Registrierung*bezeichnet.
- Der Container, der das Objekt instanziiert, `IOrderService` das die-Schnitt `ProfileViewModel` Stelle implementiert, und das-Objekt. Dies wird als *Auflösung*bezeichnet.

Schließlich wird die APP mit dem `ProfileViewModel` -Objekt fertiggestellt und ist für Garbage Collection verfügbar. An diesem Punkt sollte die Garbage Collector die `IOrderService` Instanz verwerfen, wenn andere Klassen nicht dieselbe Instanz gemeinsam verwenden.

> [!TIP]
> Schreiben Sie Container agnostischen Code. Versuchen Sie immer, Container agnostischen Code zu schreiben, um die APP vom jeweiligen verwendeten Abhängigkeits Container zu entkoppeln.

## <a name="registration"></a>Registrierung

Bevor Abhängigkeiten in ein Objekt eingefügt werden können, müssen die Typen der Abhängigkeiten zuerst beim Container registriert werden. Beim Registrieren eines Typs wird in der Regel eine Schnittstelle und ein konkreter Typ, der die-Schnittstelle implementiert, dem Container übergeben.

Es gibt zwei Möglichkeiten, Typen und Objekte im Container über den Code zu registrieren:

- Registrieren Sie einen Typ oder eine Zuordnung beim Container. Wenn dies erforderlich ist, erstellt der Container eine Instanz des angegebenen Typs.
- Registriert ein vorhandenes Objekt im Container als Singleton. Wenn dies erforderlich ist, gibt der Container einen Verweis auf das vorhandene Objekt zurück.

> [!TIP]
> Container für die Abhängigkeitsinjektion sind nicht immer geeignet. Die Abhängigkeitsinjektion führt zu zusätzlicher Komplexität und Anforderungen, die für kleine Apps möglicherweise nicht geeignet oder nützlich sind. Wenn eine Klasse keine Abhängigkeiten hat oder keine Abhängigkeit für andere Typen ist, ist es möglicherweise nicht sinnvoll, Sie in den Container einzufügen. Wenn eine Klasse über einen einzelnen Satz von Abhängigkeiten verfügt, die für den Typ ganzzahlig sind und sich nie ändern, ist es möglicherweise nicht sinnvoll, Sie in den Container einzufügen.

Die Registrierung von Typen, die eine Abhängigkeitsinjektion erfordern, sollte in einer einzigen Methode in einer app ausgeführt werden, und diese Methode sollte früh im Lebenszyklus der app aufgerufen werden, um sicherzustellen, dass die APP die Abhängigkeiten zwischen den Klassen kennt. In der eshoponcontainers-Mobile App wird dies von der `ViewModelLocator` -Klasse durchgeführt, `IContainer` die das-Objekt erstellt und die einzige Klasse in der APP ist, die einen Verweis auf dieses Objekt enthält. Das folgende Codebeispiel zeigt, wie der eshoponcontainers-Mobile App `IContainer` das-Objekt `ViewModelLocator` in der-Klasse deklariert:

```csharp
private static IContainer _container;
```

Typen und Instanzen werden in der `RegisterDependencies` -Methode in der `ViewModelLocator` -Klasse registriert. Dies wird erreicht, indem zuerst eine `ContainerBuilder` -Instanz erstellt wird, die im folgenden Codebeispiel veranschaulicht wird:

```csharp
var builder = new ContainerBuilder();
```

Typen und Instanzen werden dann beim `ContainerBuilder` -Objekt registriert, und im folgenden Codebeispiel wird die häufigste Form der Typregistrierung veranschaulicht:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

Die `RegisterType` hier gezeigte-Methode ordnet einen Schnittstellentyp einem konkreten Typ zu. Er weist den Container an, ein `RequestProvider` -Objekt zu instanziieren, wenn es ein-Objekt instanziiert, das eine Injektion `IRequestProvider` eines über einen Konstruktor erfordert.

Konkrete Typen können auch ohne Zuordnung von einem Schnittstellentyp direkt registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
builder.RegisterType<ProfileViewModel>();
```

Wenn der `ProfileViewModel` Typ aufgelöst ist, fügt der Container die erforderlichen Abhängigkeiten ein.

Autofac ermöglicht außerdem die Instanzregistrierung, bei der der Container für die Verwaltung eines Verweises auf eine Singleton Instanz eines Typs zuständig ist. Beispielsweise wird im folgenden Codebeispiel veranschaulicht, wie die eshoponcontainers-Mobile App den konkreten Typ registrieren, der `ProfileViewModel` verwendet werden soll `IOrderService` , wenn eine-Instanz eine Instanz benötigt:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

Die `RegisterType` hier gezeigte-Methode ordnet einen Schnittstellentyp einem konkreten Typ zu. Die `SingleInstance` -Methode konfiguriert die Registrierung so, dass jedes abhängige Objekt dieselbe freigegebene Instanz erhält. Daher ist nur eine einzelne `OrderService` Instanz im Container vorhanden, die von Objekten gemeinsam genutzt wird, die eine Injektion `IOrderService` eines über einen Konstruktor erfordern.

Die Instanzregistrierung kann auch mit der `RegisterInstance` -Methode durchgeführt werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

Die `RegisterInstance` hier gezeigte-Methode erstellt eine `OrderMockService` neue-Instanz und registriert Sie beim Container. Daher ist nur eine einzelne `OrderMockService` Instanz im Container vorhanden, die von Objekten gemeinsam genutzt wird, die eine Injektion `IOrderService` eines über einen Konstruktor erfordern.

Nach der Registrierung von Typ und Instanz `IContainer` muss das Objekt erstellt werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
_container = builder.Build();
```

Durch Aufrufen `Build` der-Methode `ContainerBuilder` für die-Instanz wird ein neuer Container für die Abhängigkeitsinjektion erstellt, der die vorgenommenen Registrierungen enthält.

> [!TIP]
> Angenommen, `IContainer` eine ist unveränderlich. Obwohl autofac eine `Update` Methode zum Aktualisieren von Registrierungen in einem vorhandenen Container bereitstellt, sollte das Aufrufen dieser Methode nach Möglichkeit vermieden werden. Es besteht das Risiko, einen Container zu ändern, nachdem er erstellt wurde. Dies gilt insbesondere, wenn der Container verwendet wurde. Weitere Informationen finden Sie unter [betrachten eines Containers als unveränderlich](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) auf readthedocs.IO.

<a name="resolution" />

## <a name="resolution"></a>Auflösung

Nachdem ein Typ registriert wurde, kann er aufgelöst oder als Abhängigkeit eingefügt werden. Wenn ein Typ aufgelöst wird und der Container eine neue Instanz erstellen muss, fügt er alle Abhängigkeiten in die Instanz ein.

Im Allgemeinen geschieht Folgendes: Wenn ein Typ aufgelöst wird, geschieht eines von drei Dingen:

1. Wenn der Typ nicht registriert wurde, löst der Container eine Ausnahme aus.
1. Wenn der Typ als Singleton registriert wurde, gibt der Container die Singleton-Instanz zurück. Wenn dies das erste Mal ist, dass der-Typ für aufgerufen wird, erstellt der Container ihn bei Bedarf und behält einen Verweis darauf bei.
1. Wenn der Typ nicht als Singleton registriert wurde, gibt der Container eine neue Instanz zurück und behält keinen Verweis darauf bei.

Das folgende Codebeispiel zeigt, wie `RequestProvider` der Typ, der zuvor bei autofac registriert wurde, aufgelöst werden kann:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

In diesem Beispiel wird autofac aufgefordert, den konkreten Typ `IRequestProvider` des Typs zusammen mit allen Abhängigkeiten aufzulösen. In der Regel `Resolve` wird die-Methode aufgerufen, wenn eine Instanz eines bestimmten Typs erforderlich ist. Informationen zum Steuern der Lebensdauer von aufgelösten Objekten finden Sie unter [Verwalten der Lebensdauer von aufgelösten Objekten](#managing_the_lifetime_of_resolved_objects).

Das folgende Codebeispiel zeigt, wie der eshoponcontainers-Mobile App Ansichts Modelltypen und deren Abhängigkeiten instanziiert:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

In diesem Beispiel wird autofac aufgefordert, den Ansichts Modelltyp für ein angefordertes Ansichts Modell aufzulösen. Außerdem werden alle Abhängigkeiten vom Container aufgelöst. Beim Auflösen des `ProfileViewModel` Typs ist die aufzulösende Abhängigkeit ein `IOrderService` Objekt. Daher erstellt autofac zuerst ein `OrderService` -Objekt und übergibt es dann an den Konstruktor `ProfileViewModel` der-Klasse. Weitere Informationen darüber, wie die eshoponcontainers-Mobile App Ansichts Modelle erstellt und diese Ansichten zuordnet, finden Sie unter [Automatisches Erstellen eines Ansichts Modells mit einem Ansichts Modell-Locator](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> Das Registrieren und Auflösen von Typen mit einem Container wirkt sich negativ auf die Leistung aus, da der Container Reflektion zum Erstellen der einzelnen Typen verwendet, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der App rekonstruiert werden müssen. Wenn zahlreiche oder tiefe Abhängigkeiten vorhanden sind, können die Kosten für die Erstellung erheblich zunehmen.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>Verwalten der Lebensdauer von aufgelösten Objekten

Nach der Registrierung eines Typs besteht das Standardverhalten von autofac darin, eine neue Instanz des registrierten Typs jedes Mal zu erstellen, wenn der Typ aufgelöst wird, oder wenn der Abhängigkeits Mechanismus Instanzen in andere Klassen einfügt. In diesem Szenario enthält der Container keinen Verweis auf das aufgelöste Objekt. Wenn Sie jedoch eine Instanz registrieren, besteht das Standardverhalten von autofac darin, die Lebensdauer des Objekts als Singleton zu verwalten. Daher verbleibt die Instanz im Gültigkeitsbereich, während sich der Container im Gültigkeitsbereich befindet, und wird verworfen, wenn der Container den Gültigkeitsbereich verlässt und eine Garbage Collection durchgeführt wird oder wenn der Container explizit vom Code gelöscht wird.

Ein autofac-Instanzbereich kann verwendet werden, um das Singleton-Verhalten für ein Objekt anzugeben, das von autofac aus einem registrierten Typ erstellt wird. Autofac-Instanzbereiche verwalten die Objekt Lebensdauer, die vom Container instanziiert wird. Der standardinstanzbereich `RegisterType` für die- `InstancePerDependency` Methode ist der Gültigkeitsbereich. Der `SingleInstance` -Bereich kann jedoch mit der `RegisterType` -Methode verwendet werden, sodass der Container beim Aufrufen der `Resolve` -Methode eine Singleton Instanz eines Typs erstellt oder zurückgibt. Im folgenden Codebeispiel wird gezeigt, wie autofac angewiesen wird, eine Singleton Instanz der `NavigationService` -Klasse zu erstellen:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Wenn die `INavigationService` Schnittstelle zum ersten Mal aufgelöst wird, erstellt der Container ein `NavigationService` neues-Objekt und verwaltet einen Verweis darauf. Bei allen nachfolgenden Auflösungen der `INavigationService` -Schnittstelle gibt der Container einen Verweis auf `NavigationService` das Objekt zurück, das zuvor erstellt wurde.

> [!NOTE]
> Der Bereich SingleInstance entfernt erstellte Objekte, wenn der Container verworfen wird.

Autofac umfasst zusätzliche Instanzbereiche. Weitere Informationen finden Sie unter [Instanzbereich](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) auf readthedocs.IO.

## <a name="summary"></a>Zusammenfassung

Die Abhängigkeitsinjektion ermöglicht das Entkoppeln von konkreten Typen aus dem Code, der von diesen Typen abhängig ist. In der Regel wird ein Container verwendet, der eine Liste von Registrierungen und Zuordnungen Zwischenschnitt stellen und abstrakten Typen enthält, sowie die konkreten Typen, die diese Typen implementieren oder erweitern.

Autofac vereinfacht das entwickeln lose gekoppelter apps und bietet alle Features, die häufig in Abhängigkeits einschleusungs Containern gefunden werden, einschließlich Methoden zum Registrieren von Typzuordnungen und Objektinstanzen, zum Auflösen von Objekten, zum Verwalten von Objekt Lebensdauern und einfügen. abhängige Objekte in Konstruktoren von Objekten, die Sie auflöst.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
