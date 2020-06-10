---
Title: "Abhängigkeitsinjektion" Beschreibung: "in diesem Kapitel wird erläutert, wie der eshoponcontainers-Mobile App eine Abhängigkeitsinjektion verwendet, um konkrete Typen aus dem Code, der von diesen Typen abhängt, zu entkoppeln."
ms. Prod: xamarin ms. assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 11/04/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="dependency-injection"></a>Dependency Injection

In der Regel wird ein Klassenkonstruktor aufgerufen, wenn ein Objekt instanziiert wird. alle Werte, die das Objekt benötigt, werden als Argumente an den Konstruktor übergeben. Dies ist ein Beispiel für eine Abhängigkeitsinjektion, insbesondere als *Konstruktorinjektion*bezeichnet. Die Abhängigkeiten, die das Objekt benötigt, werden in den Konstruktor eingefügt.

Durch die Angabe von Abhängigkeiten als Schnittstellentypen ermöglicht die Abhängigkeitsinjektion das Entkoppeln der konkreten Typen aus dem Code, der von diesen Typen abhängig ist. Im Allgemeinen wird ein Container verwendet, der eine Liste von Registrierungen und Zuordnungen Zwischenschnitt stellen und abstrakten Typen sowie die konkreten Typen enthält, die diese Typen implementieren oder erweitern.

Es gibt auch andere Arten von Abhängigkeitsinjektion, wie z. b. *Einschleusung von Eigenschaften*und *Methodenaufrufe*, aber Sie sind seltener sichtbar. Daher konzentriert sich dieses Kapitel ausschließlich auf die Durchführung der Konstruktorinjektion mit einem Abhängigkeits Injection-Container.

## <a name="introduction-to-dependency-injection"></a>Einführung in die Abhängigkeitsinjektion

Die Abhängigkeitsinjektion ist eine spezialisierte Version des IOC-Musters (Inversion of Control), bei der das Problem, das invertiert wird, das Abrufen der erforderlichen Abhängigkeit ist. Bei der Abhängigkeitsinjektion ist eine andere Klasse für das Einfügen von Abhängigkeiten in ein Objekt zur Laufzeit verantwortlich. Das folgende Codebeispiel zeigt, wie die- `ProfileViewModel` Klasse bei der Verwendung von Abhängigkeitsinjektion strukturiert ist:

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

Der `ProfileViewModel` Konstruktor empfängt eine `IOrderService` Instanz als Argument, das von einer anderen Klasse eingefügt wurde. Die einzige Abhängigkeit in der- `ProfileViewModel` Klasse ist der Schnittstellentyp. Daher verfügt die `ProfileViewModel` -Klasse nicht über Kenntnisse über die-Klasse, die für die Instanziierung des Objekts zuständig ist `IOrderService` . Die Klasse, die für das Instanziieren des `IOrderService` -Objekts und das Einfügen in die-Klasse zuständig ist, `ProfileViewModel` wird als *Abhängigkeits Injection-Container*bezeichnet.

Abhängigkeits einschleusungs Container reduzieren die Kopplung zwischen Objekten, indem Sie eine Funktion zum Instanziieren von Klassen Instanzen und zum Verwalten Ihrer Lebensdauer basierend auf der Konfiguration des Containers bereitstellen. Während der Objekt Erstellung fügt der Container alle Abhängigkeiten ein, die das Objekt benötigt. Wenn diese Abhängigkeiten noch nicht erstellt wurden, werden die Abhängigkeiten vom Container erstellt und aufgelöst.

> [!NOTE]
> Die Abhängigkeitsinjektion kann auch manuell mithilfe von Factorys implementiert werden. Die Verwendung eines Containers bietet jedoch zusätzliche Funktionen wie die Verwaltung der Lebensdauer und die Registrierung über die Assemblyüberprüfung.

Die Verwendung eines Containers für die Abhängigkeitsinjektion bietet mehrere Vorteile:

- Mit einem Container entfällt die Notwendigkeit, dass eine Klasse ihre Abhängigkeiten findet und ihre Lebensdauer verwaltet.
- Ein Container ermöglicht die Zuordnung implementierter Abhängigkeiten, ohne dass sich dies auf die Klasse auswirkt.
- Ein Container vereinfacht die Prüfbarkeit, indem Abhängigkeiten ermöglicht werden.
- Ein Container steigert die Verwaltbarkeit, da neue Klassen problemlos der app hinzugefügt werden können.

Im Kontext einer- Xamarin.Forms app, die MVVM verwendet, wird in der Regel ein Abhängigkeits Injektions Container zum Registrieren und Auflösen von Ansichts Modellen und zum Registrieren von Diensten und zum Einfügen der Dienste in Ansichts Modelle verwendet.

Es sind viele abhängigkeiteneinschleusungs Container verfügbar, wobei der eshoponcontainers-Mobile App tinyioc zum Verwalten der Instanziierung von Ansichts Modell-und Dienst Klassen in der APP verwendet. Tinyioc wurde nach der Auswertung verschiedener Container ausgewählt und bietet im Vergleich zu den meisten bekannten Containern eine bessere Leistung auf mobilen Plattformen. Es vereinfacht das entwickeln lose gekoppelter apps und bietet alle Features, die häufig in Objekten für die Abhängigkeitsinjektion gefunden werden, einschließlich Methoden zum Registrieren von Typzuordnungen, zum Auflösen von Objekten, zum Verwalten von Objekt Lebensdauern und zum Einfügen abhängiger Objekte in Konstruktoren von Objekten, die aufgelöst werden. Weitere Informationen zu tinyioc finden Sie unter [tinyioc](https://github.com/grumpydev/TinyIoC/wiki) auf GitHub.com.

In tinyioc stellt der- `TinyIoCContainer` Typ den Container für die Abhängigkeitsinjektion bereit. In Abbildung 3-1 werden die Abhängigkeiten angezeigt, wenn dieser Container verwendet wird, der ein Objekt instanziiert `IOrderService` und in die- `ProfileViewModel` Klasse einfügt.

![](dependency-injection-images/dependencyinjection.png "Dependencies example when using dependency injection")

**Abbildung 3-1:** Abhängigkeiten bei der Verwendung von Abhängigkeitsinjektion

Zur Laufzeit muss der Container wissen, welche Implementierung der `IOrderService` Schnittstelle instanziiert werden soll, bevor er ein-Objekt instanziieren kann `ProfileViewModel` . Dies umfasst Folgendes:

- Der Container, der entscheidet, wie ein Objekt instanziiert werden soll, das die- `IOrderService` Schnittstelle implementiert. Dies wird als *Registrierung*bezeichnet.
- Der Container, der das Objekt instanziiert, das die `IOrderService` -Schnittstelle implementiert, und das- `ProfileViewModel` Objekt. Dies wird als *Auflösung*bezeichnet.

Schließlich wird die APP mit dem `ProfileViewModel` -Objekt fertiggestellt und ist für Garbage Collection verfügbar. An diesem Punkt sollte die Garbage Collector die Instanz verwerfen, `IOrderService` Wenn andere Klassen nicht dieselbe Instanz gemeinsam verwenden.

> [!TIP]
> Schreiben Sie Container agnostischen Code. Versuchen Sie immer, Container agnostischen Code zu schreiben, um die APP vom jeweiligen verwendeten Abhängigkeits Container zu entkoppeln.

## <a name="registration"></a>Registrierung

Bevor Abhängigkeiten in ein Objekt eingefügt werden können, müssen die Typen der Abhängigkeiten zuerst beim Container registriert werden. Beim Registrieren eines Typs wird in der Regel eine Schnittstelle und ein konkreter Typ, der die-Schnittstelle implementiert, dem Container übergeben.

Es gibt zwei Möglichkeiten, Typen und Objekte im Container über den Code zu registrieren:

- Registrieren Sie einen Typ oder eine Zuordnung beim Container. Wenn dies erforderlich ist, erstellt der Container eine Instanz des angegebenen Typs.
- Registriert ein vorhandenes Objekt im Container als Singleton. Wenn dies erforderlich ist, gibt der Container einen Verweis auf das vorhandene Objekt zurück.

> [!TIP]
> Container für die Abhängigkeitsinjektion sind nicht immer geeignet. Die Abhängigkeitsinjektion führt zu zusätzlicher Komplexität und Anforderungen, die für kleine Apps möglicherweise nicht geeignet oder nützlich sind. Wenn eine Klasse keine Abhängigkeiten hat oder keine Abhängigkeit für andere Typen ist, ist es möglicherweise nicht sinnvoll, Sie in den Container einzufügen. Wenn eine Klasse über einen einzelnen Satz von Abhängigkeiten verfügt, die für den Typ ganzzahlig sind und sich nie ändern, ist es möglicherweise nicht sinnvoll, Sie in den Container einzufügen.

Die Registrierung von Typen, die eine Abhängigkeitsinjektion erfordern, sollte in einer einzigen Methode in einer app ausgeführt werden, und diese Methode sollte früh im Lebenszyklus der app aufgerufen werden, um sicherzustellen, dass die APP die Abhängigkeiten zwischen den Klassen kennt. In der eshoponcontainers-Mobile App wird dies von der-Klasse durchgeführt `ViewModelLocator` , die das `TinyIoCContainer` -Objekt erstellt und die einzige Klasse in der APP ist, die einen Verweis auf dieses Objekt enthält. Das folgende Codebeispiel zeigt, wie der eshoponcontainers-Mobile App das- `TinyIoCContainer` Objekt in der- `ViewModelLocator` Klasse deklariert:

```csharp
private static TinyIoCContainer _container;
```

Typen werden im `ViewModelLocator` Konstruktor registriert. Dies wird erreicht, indem zuerst eine-Instanz erstellt wird `TinyIoCContainer` , die im folgenden Codebeispiel veranschaulicht wird:

```csharp
_container = new TinyIoCContainer();
```

Die Typen werden dann mit dem `TinyIoCContainer` -Objekt registriert, und im folgenden Codebeispiel wird die häufigste Form der Typregistrierung veranschaulicht:

```csharp
_container.Register<IRequestProvider, RequestProvider>();
```

Die `Register` hier gezeigte-Methode ordnet einen Schnittstellentyp einem konkreten Typ zu. Standardmäßig ist jede Schnittstellen Registrierung als Singleton konfiguriert, sodass jedes abhängige Objekt dieselbe freigegebene Instanz erhält. Daher ist nur eine einzelne `RequestProvider` Instanz im Container vorhanden, die von Objekten gemeinsam genutzt wird, die eine Injektion eines `IRequestProvider` über einen Konstruktor erfordern.

Konkrete Typen können auch ohne Zuordnung von einem Schnittstellentyp direkt registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
_container.Register<ProfileViewModel>();
```

Standardmäßig ist jede konkrete Klassen Registrierung als mehrere Instanzen konfiguriert, sodass jedes abhängige Objekt eine neue-Instanz empfängt. Wenn `ProfileViewModel` aufgelöst wird, wird eine neue-Instanz erstellt, und der Container fügt die erforderlichen Abhängigkeiten ein.

## <a name="resolution"></a>Lösung

Nachdem ein Typ registriert wurde, kann er aufgelöst oder als Abhängigkeit eingefügt werden. Wenn ein Typ aufgelöst wird und der Container eine neue Instanz erstellen muss, fügt er alle Abhängigkeiten in die Instanz ein.

Im Allgemeinen geschieht Folgendes: Wenn ein Typ aufgelöst wird, geschieht eines von drei Dingen:

1. Wenn der Typ nicht registriert wurde, löst der Container eine Ausnahme aus.
1. Wenn der Typ als Singleton registriert wurde, gibt der Container die Singleton-Instanz zurück. Wenn dies das erste Mal ist, dass der-Typ für aufgerufen wird, erstellt der Container ihn bei Bedarf und behält einen Verweis darauf bei.
1. Wenn der Typ nicht als Singleton registriert wurde, gibt der Container eine neue Instanz zurück und behält keinen Verweis darauf bei.

Das folgende Codebeispiel zeigt, wie der `RequestProvider` Typ, der zuvor bei tinyioc registriert wurde, aufgelöst werden kann:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

In diesem Beispiel wird tinyioc aufgefordert, den konkreten Typ für den `IRequestProvider` Typ zusammen mit allen Abhängigkeiten aufzulösen. In der Regel `Resolve` wird die-Methode aufgerufen, wenn eine Instanz eines bestimmten Typs erforderlich ist. Informationen zum Steuern der Lebensdauer von aufgelösten Objekten finden Sie unter [Verwalten der Lebensdauer von aufgelösten Objekten](#managing-the-lifetime-of-resolved-objects).

Das folgende Codebeispiel zeigt, wie der eshoponcontainers-Mobile App Ansichts Modelltypen und deren Abhängigkeiten instanziiert:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

In diesem Beispiel wird tinyioc aufgefordert, den Ansichts Modelltyp für ein angefordertes Ansichts Modell aufzulösen. Außerdem werden alle Abhängigkeiten vom Container aufgelöst. Beim Auflösen des `ProfileViewModel` Typs sind die aufzulösenden Abhängigkeiten ein `ISettingsService` -Objekt und ein- `IOrderService` Objekt. Da beim Registrieren der-Klasse und der-Klasse-Schnittstellen Registrierungen verwendet wurden `SettingsService` `OrderService` , gibt tinyioc Singleton-Instanzen für die-Klasse und die- `SettingsService` Klasse zurück `OrderService` und übergibt sie an den Konstruktor der- `ProfileViewModel` Klasse. Weitere Informationen darüber, wie die eshoponcontainers-Mobile App Ansichts Modelle erstellt und diese Ansichten zuordnet, finden Sie unter [Automatisches Erstellen eines Ansichts Modells mit einem Ansichts Modell-Locator](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator).

> [!NOTE]
> Das Registrieren und Auflösen von Typen mit einem Container wirkt sich negativ auf die Leistung aus, da der Container Reflektion zum Erstellen der einzelnen Typen verwendet, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der App rekonstruiert werden müssen. Wenn zahlreiche oder tiefe Abhängigkeiten vorhanden sind, können die Kosten für die Erstellung erheblich zunehmen.

## <a name="managing-the-lifetime-of-resolved-objects"></a>Verwalten der Lebensdauer von aufgelösten Objekten

Nachdem Sie einen Typ mit einer konkreten Klassen Registrierung registriert haben, besteht das Standardverhalten für tinyioc darin, bei jedem Auflösen des Typs eine neue Instanz des registrierten Typs zu erstellen, oder wenn der Abhängigkeits Mechanismus Instanzen in andere Klassen einfügt. In diesem Szenario enthält der Container keinen Verweis auf das aufgelöste Objekt. Wenn Sie jedoch einen Typ mithilfe der Schnittstellen Registrierung registrieren, besteht das Standardverhalten für tinyioc darin, die Lebensdauer des Objekts als Singleton zu verwalten. Daher verbleibt die Instanz im Gültigkeitsbereich, während sich der Container im Gültigkeitsbereich befindet, und wird verworfen, wenn der Container den Gültigkeitsbereich verlässt und eine Garbage Collection durchgeführt wird oder wenn der Container explizit vom Code gelöscht wird.

Das standardmäßige tinyioc-Registrierungs Verhalten kann mithilfe der Methoden "fließend" und "API" überschrieben werden `AsSingleton` `AsMultiInstance` . Die-Methode kann z. b. `AsSingleton` mit der-Methode verwendet werden `Register` , sodass der Container beim Aufrufen der-Methode eine Singleton-Instanz eines Typs erstellt oder zurückgibt `Resolve` . Im folgenden Codebeispiel wird gezeigt, wie tinyioc angewiesen wird, eine Singleton Instanz der-Klasse zu erstellen `LoginViewModel` :

```csharp
_container.Register<LoginViewModel>().AsSingleton();
```

Wenn der-Typ zum ersten Mal `LoginViewModel` aufgelöst wird, erstellt der Container ein neues `LoginViewModel` -Objekt und behält einen Verweis darauf bei. Bei allen nachfolgenden Auflösungen von `LoginViewModel` gibt der Container einen Verweis auf das Objekt zurück, `LoginViewModel` das zuvor erstellt wurde.

> [!NOTE]
> Typen, die als Singletons registriert sind, werden verworfen, wenn der Container verworfen wird.

## <a name="summary"></a>Zusammenfassung

Die Abhängigkeitsinjektion ermöglicht das Entkoppeln von konkreten Typen aus dem Code, der von diesen Typen abhängig ist. In der Regel wird ein Container verwendet, der eine Liste von Registrierungen und Zuordnungen Zwischenschnitt stellen und abstrakten Typen enthält, sowie die konkreten Typen, die diese Typen implementieren oder erweitern.

Tinyioc ist ein Lightweight-Container, der im Vergleich zu den meisten bekannten Containern eine bessere Leistung auf mobilen Plattformen bietet. Es vereinfacht das entwickeln lose gekoppelter apps und bietet alle Features, die häufig in Abhängigkeits einschleusungs Containern gefunden werden, einschließlich Methoden zum Registrieren von Typzuordnungen, Auflösen von Objekten, Verwalten der Objekt Lebensdauer und Einfügen abhängiger Objekte in Konstruktoren von Objekten, die Sie auflöst.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
