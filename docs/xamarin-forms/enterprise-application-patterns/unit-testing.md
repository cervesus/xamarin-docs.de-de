---
title: Unittests
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 57201a32f5ffc0ae962f6db851a25a737e1cb17d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="unit-testing"></a>Unittests

Mobile apps haben eindeutige Probleme, denen Desktop- und webbasierte Anwendungen keine Gedanken. Mobile Benutzer von den Geräten, die sie durch die Netzwerkkonnektivität, indem Sie die Verfügbarkeit von Diensten und einen Bereich von anderen Faktoren verwenden, unterscheidet sich. Aus diesem Grund sollte mobiler apps getestet werden, wie sie in der wirklichen Welt verwendet werden sollen, um die Qualität, Zuverlässigkeit und Leistung zu verbessern. Es gibt viele Arten von Tests, die für eine app, einschließlich Komponententests, Integrationstests zu legen, und testen, mit Komponententests wird die am häufigsten verwendete Form des Testens Benutzeroberfläche ausgeführt werden soll.

Ein Komponententest nimmt eine kleine Einheit der Anwendung, in der Regel eine Methode, isoliert es aus der Rest des Codes und stellt sicher, dass es sich erwartungsgemäß verhält. Das Ziel besteht darin, dass jede Funktionseinheit wie erwartet, sodass Fehler in der app verteilt nicht überprüfen. Erkennen eines Fehlers, wo es auftritt, ist jedoch effizienter, die Auswirkungen eines Fehlers indirekt zu einem sekundären Zeitpunkt des Fehlers prüfen.

UnitTests hat die stärksten Auswirkungen auf die Qualität des Codes auf, wenn es sich um ein wesentlicher Bestandteil der Software-Entwicklungsworkflow ist. Als eine Methode geschrieben wurde, sollte Komponententests geschrieben werden können, die dem Verhalten der Methode als Reaktion auf Standard, Grenze und falsche Fälle der Eingabedaten und die Überprüfung alle expliziten oder impliziten Annahmen durch den Code überprüfen. Alternativ werden mit testgesteuerte Entwicklung Komponententests vor dem Code geschrieben. In diesem Szenario dienen Komponententests als Entwurfsdokumentation und als funktionale Spezifikationen der Funktionen.

> [!NOTE]
> Komponententests sind sehr gut für Regression – d. h. Funktionen, die Arbeit verwendet, jedoch wurde durch eine fehlerhafte Update gestört wurde.

In der Regel verwenden Sie Komponententests anordnen Act assert Muster:

-   Die *anordnen* Teil der Komponententestmethode Objekte initialisiert und legt den Wert der Daten, die zu testende Methode übergeben werden.
-   Die *fungieren* Abschnitt Ruft die Methode im Test mit erforderlichen Argumenten.
-   Die *assert* Abschnitt wird überprüft, ob die Aktion an, der die zu testende Methode wie erwartet.

Dieses Muster wird sichergestellt, dass Komponententests lesbar und konsistent.

## <a name="dependency-injection-and-unit-testing"></a>Abhängigkeiteneinschleusung und Komponententests

Einer der Beweggründe für die Übernahme einer lose verknüpften Architektur ist, dass sie Komponententests vereinfacht. Einer der Typen mit Autofac registriert ist die `OrderService` Klasse. Das folgende Codebeispiel zeigt einen Überblick über diese Klasse:

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

Die `OrderDetailViewModel` -Klasse verfügt über eine Abhängigkeit auf der `IOrderService` geben, der der Container aufgelöst wird, wenn er instanziiert einen `OrderDetailViewModel` Objekt. Allerdings anstatt erstellen ein `OrderService` Objekt Komponententest der `OrderDetailViewModel` -Klasse, ersetzen Sie stattdessen die `OrderService` Objekt mit einem Mock für die Tests. Abbildung 10 – 1 zeigt diese Beziehung.

![](unit-testing-images/unittesting.png "Klassen, die die IOrderService-Schnittstelle implementieren")

**Abbildung 10 – 1:** Klassen, die die IOrderService-Schnittstelle implementieren

Dieser Ansatz ermöglicht die `OrderService` Objekt übergeben werden die `OrderDetailViewModel` -Klasse zur Laufzeit und im Interesse der Prüfbarkeit, sie können die `OrderMockService` Klasse übergeben werden der `OrderDetailViewModel` Klasse zum Zeitpunkt der Tests. Der Hauptvorteil dieses Ansatzes ist, dass sie Komponententests ausgeführt werden, ohne dass unhandlich Ressourcen wie Webdienste oder Datenbanken ermöglicht.

## <a name="testing-mvvm-applications"></a>Testen von Anwendungen mit MVVM

Testen von Modellen und Modelle anzeigen von MVVM Anwendungen ist identisch mit Tests andere Klassen bilden, und die gleichen Tools und Techniken – z. B. Komponententests und imitieren, können verwendet werden. Es gibt jedoch einige Muster, die typisch für Modell sind und die Ansicht Modellklassen, die von bestimmten Einheit Testverfahren profitieren können.

> [!TIP]
> Testen Sie ein Ziel mit jeder Komponententest. Seien Sie nicht möchten, stellen eine Einheit Übung mehr als einen Aspekt des Verhaltens der Einheit testen. Auf diese Weise führt zu Tests, die schwer zu lesen und zu aktualisieren. Sie können auch zu Verwirrung führen, beim Interpretieren eines Fehlers.

Verwendet der eShopOnContainers-Verwaltungsrichtlinien für mobile Apps [xUnit](https://xunit.github.io/) Komponententests, ausführen, die zwei verschiedene Arten von Komponententests unterstützt:

-   Fakten sind Tests, die immer "true" werden die invariante Bedingungen zu testen.
-   Theorien sind Tests, die nur "true" für einen bestimmten Satz von Daten sind.

Mit der mobilen app eShopOnContainers enthaltenen Komponententests sind Fakt Tests, und daher wird jede Komponententestmethode ergänzt, mit der `[Fact]` Attribut.

> [!NOTE]
> durch einen Test Runner werden Tests xUnit ausgeführt. Führen Sie zum Ausführen von Test Runner das eShopOnContainers.TestRunner-Projekt für die erforderliche Plattform aus.

### <a name="testing-asynchronous-functionality"></a>Testen der asynchronen Funktionalität

Beim Implementieren des Steuerelementmusters MVVM aufrufvorgängen Modelle anzeigen in der Regel auf Dienste, häufig asynchron. Tests für Code, der in der Regel diese Vorgänge ruft werden Mocks als Ersatz für welche Dienste tatsächlich verwenden. Im folgenden Codebeispiel wird veranschaulicht, asynchrone Funktionen durch das Übergeben von eines simulierten Diensts an einem Ansichtsmodell getestet:

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

Diese Komponententests überprüft, ob die `Order` Eigenschaft von der `OrderDetailViewModel` Instanz weist einen Wert nach der `InitializeAsync` Methode wurde aufgerufen. Die `InitializeAsync` Methode wird aufgerufen, wenn das Ansichtsmodell entsprechende Ansicht zu der navigiert wird. Weitere Informationen über die Navigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Wenn die `OrderDetailViewModel` Instanz erstellt, die es erwartet ein `OrderService` Instanz als Argument angegeben werden. Allerdings die `OrderService` Ruft Daten von einem Webdienst ab. Aus diesem Grund eine `OrderMockService` -Instanz, die eine Pseudoversion wird von der `OrderService` Klasse, die als Argument angegeben ist die `OrderDetailViewModel` Konstruktor. Nachdem des Ansichtsmodells `InitializeAsync` Methode aufgerufen wird, ruft die `IOrderService` Vorgänge simulierte Daten werden abgerufen, statt mit einem Webdienst kommuniziert.

### <a name="testing-inotifypropertychanged-implementations"></a>Testen der INotifyPropertyChanged-Implementierungen

Implementieren der `INotifyPropertyChanged` Schnittstelle ermöglicht es, Ansichten, auf die Änderungen zu reagieren, die aus Sicht stammen und Modelle. Diese Änderungen sind nicht auf Daten in Steuerelementen beschränkt – sie werden auch verwendet, um die Sicht, z. B. Ansichtszustände-Modell zu steuern, Animationen gestartet werden soll oder Steuerelemente zu deaktivierenden verursachen.

Eigenschaften, die direkt von den Komponententest aktualisiert werden können, können durch das Anfügen an eines ereignishandlers getestet werden die `PropertyChanged` Ereignis und überprüfen, ob das Ereignis ausgelöst wird, nachdem Sie einen neuen Wert für die Eigenschaft festgelegt haben. Im folgenden Codebeispiel wird veranschaulicht, einen solchen Test:

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

Komponententest ruft die `InitializeAsync` Methode der `OrderViewModel` Klasse, wodurch seine `Order` Eigenschaft aktualisiert werden. Der Komponententest übergeben wird, bereitgestellt, die die `PropertyChanged` Ereignis wird ausgelöst, für die `Order` Eigenschaft.

### <a name="testing-message-based-communication"></a>Testen die meldungsbasierte Kommunikation

Ansicht modelliert, bei denen die [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse, um die Kommunikation zwischen lose verknüpfte Klassen kann Einheit getestet werden durch Abonnieren der Nachricht gesendet werden, indem der Code unter dem Test, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

Diese Komponententests überprüft, ob die `CatalogViewModel` veröffentlicht die `AddProduct` Nachricht als Antwort auf seine `AddCatalogItemCommand` ausgeführt wird. Da die [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse Multicastnachricht Abonnements unterstützt, die der Komponententest abonnieren kann die `AddProduct` Nachricht, und führen Sie einen Rückrufdelegaten als Antwort auf abgerufen wird. Legt dieser Rückrufdelegat, angegeben als Lambda-Ausdruck, ein `boolean` Feld, das von verwendet wird, die `Assert` Anweisung, um das Verhalten des Tests überprüfen.

### <a name="testing-exception-handling"></a>Testen die Behandlung von Ausnahmen

Komponententests können geschrieben werden auch dieser Überprüfung an, der für ungültige Aktionen oder Eingaben, bestimmte Ausnahmen ausgelöst werden wie im folgenden Codebeispiel gezeigt:

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

Komponententest löst eine Ausnahme aus, da die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement verfügt nicht über ein Ereignis namens `OnItemTapped`. Die `Assert.Throws<T>` Methode ist eine generische Methode, in dem `T` ist der Typ der erwarteten Ausnahme. Das Argument zu übergeben, um die `Assert.Throws<T>` Methode ist ein Lambda-Ausdruck, der die Ausnahme ausgelöst wird. Aus diesem Grund wird der Komponententest übergeben, vorausgesetzt, dass der Lambda-Ausdruck löst einen `ArgumentException`.

>💡 **Tipp**: vermeiden Sie das Schreiben von Komponententests, die Meldungs-Ausnahme zu untersuchen. Ausnahme Meldungs-Zeit ändern können, und daher Komponententests, die abhängig von deren Vorhandensein als anfälligen betrachtet werden.

### <a name="testing-validation"></a>Testen der Gültigkeitsüberprüfung

Es gibt zwei Aspekte für das Testen von der Implementierung der Validierung: testen möchten, ob alle Validierungsregeln ordnungsgemäß implementiert werden, und testen, die die `ValidatableObject<T>` Klasse erwartungsgemäß.

Validierungslogik ist in der Regel einfach zu testen, da sie in der Regel ein eigenständiger Prozess ist, in dem die Ausgabe für die Eingabe hängt ab. Es darf Tests auf den Ergebnissen des Aufrufs der `Validate` Methode für jede Eigenschaft, die mindestens eine zugeordnete Validierungsregel aufweist, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

Diese Komponententests überprüft, dass die Überprüfung ist, wenn erfolgreich die beiden `ValidatableObject<T>` Eigenschaften in der `MockViewModel` Instanz sowohl über Daten verfügen.

Und überprüfen, dass die Überprüfung ist erfolgreich, Komponententests für die Überprüfung sollte auch Überprüfen der Werte von der `Value`, `IsValid`, und `Errors` -Eigenschaft jedes `ValidatableObject<T>` Instanz, um sicherzustellen, dass die Klasse wie erwartet verhält. Im folgenden Codebeispiel wird veranschaulicht, einen Komponententest, der dies tut:

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

Diese Komponententests überprüft, dass die Überprüfung schlägt fehl, wenn die `Surname` Eigenschaft von der `MockViewModel` verfügt nicht über alle Daten, und die `Value`, `IsValid`, und `Errors` -Eigenschaft jedes `ValidatableObject<T>` Instanz sind richtig festgelegt.

## <a name="summary"></a>Zusammenfassung

Ein Komponententest nimmt eine kleine Einheit der Anwendung, in der Regel eine Methode, isoliert es aus der Rest des Codes und stellt sicher, dass es sich erwartungsgemäß verhält. Das Ziel besteht darin, dass jede Funktionseinheit wie erwartet, sodass Fehler in der app verteilt nicht überprüfen.

Das Verhalten eines Objekts getesteten kann durch Ersetzen der abhängigen Objekte mit Pseudoobjekten, mit die das Verhalten der abhängigen Objekte simuliert isoliert werden. Dadurch können die Komponententests ausgeführt werden, ohne dass unhandlich Ressourcen wie Webdienste oder Datenbanken.

Testen von Modellen und Modelle anzeigen von MVVM Anwendungen ist identisch mit Tests andere Klassen bilden, und die gleichen Tools und Techniken können verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
