---
title: Unit-Testing Unternehmens-Apps
description: In diesem Kapitel wird erläutert, wie Komponententests in der eShopOnContainers-mobile-app ausgeführt wird.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d83cdce7076eac5a022863b583ecb01346ae440a
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831087"
---
# <a name="unit-testing-enterprise-apps"></a>Unit-Testing Unternehmens-Apps

Mobile apps haben eindeutige Probleme, Desktop- und webbasierten Anwendungen nicht kümmern. Mobile Benutzer von den Geräten, die sie durch die Netzwerkverbindung getrennt wird, indem Sie die Verfügbarkeit von Diensten und einen Bereich von anderen Faktoren verwenden, unterscheiden sich. Aus diesem Grund sollte mobiler apps getestet werden, wie sie in der Praxis verwendet werden werden, um die Qualität, Zuverlässigkeit und Leistung zu verbessern. Es gibt viele Arten von Tests, die für eine app, einschließlich Komponententests, Integrationstests und testen, mit Komponententests werden das am häufigsten verwendete Form des Testens der Benutzeroberfläche ausgeführt werden soll.

Ein Komponententest nimmt eine kleine Einheit der app, in der Regel eine Methode, wird es vom Rest des Codes isoliert und stellt sicher, dass sie sich erwartungsgemäß verhält. Ziel ist es, überprüfen Sie, dass jede Einheit der Funktionalität führt erwartungsgemäß funktioniert, sodass Fehler in der gesamten app weitergegeben werden, nicht. Erkennen eines Fehlers, in dem er auftritt, ist jedoch effizienter, beobachten die Auswirkungen eines Fehlers indirekt an einen sekundären Point of Failure.

Komponententests hat die stärksten Auswirkungen auf die Qualität des Codes auf, wenn es sich um ein wesentlicher Bestandteil der Software-Entwicklungsworkflow ist. Sobald eine Methode geschrieben wurde, sollte Komponententests geschrieben werden, dass dem Verhalten der Methode als Reaktion auf Standard, Grenzen und falsche Anfragen von Eingabedaten, und die Überprüfung alle expliziten oder impliziten Annahmen durch den Code überprüfen, ob. Sie können auch mit testgesteuerte Entwicklung, Komponententests vor dem Code wurden erstellt. In diesem Szenario werden Komponententests als Entwurfsdokumentation und als funktionale Spezifikationen der fungieren.

> [!NOTE]
> Komponententests sind sehr effektiv für Regression – d. h. Funktionen, die zum Arbeiten verwendet, jedoch wurde von einem fehlerhaften Update gestört wurde.

In der Regel verwenden Sie Komponententests Arrange-Act-assert-Muster:

-   Die *anordnen* Teil der Komponententestmethode Objekte initialisiert und legt den Wert der Daten, die zu testende Methode übergeben werden.
-   Die *fungieren* Abschnitt Ruft die Methode unter testbedingungen mit erforderlichen Argumenten.
-   Die *assert* Abschnitt stellt sicher, dass die Aktion an, der die zu testende Methode wie erwartet verhält.

Mit diesem Muster wird sichergestellt, dass Komponententests lesbar und konsistent.

## <a name="dependency-injection-and-unit-testing"></a>Dependency Injection und Komponententests

Einer der Beweggründe für die Einführung einer lose gekoppelten Architektur ist, dass es sich um Komponententests ermöglicht. Einer der Typen mit Autofac registriert ist die `OrderService` Klasse. Das folgende Codebeispiel zeigt einen Überblick über diese Klasse:

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

Die `OrderDetailViewModel` -Klasse verfügt über eine Abhängigkeit auf der `IOrderService` geben, der der Container aufgelöst wird, wenn es instanziiert einen `OrderDetailViewModel` Objekt. Jedoch statt erstellen ein `OrderService` Objekt um einen Komponententest durchführen der `OrderDetailViewModel` -Klasse, stattdessen ersetzen die `OrderService` Objekt mit der ein Mock im Rahmen der Tests. Abbildung 10-1 zeigt diese Beziehung.

![](unit-testing-images/unittesting.png "Klassen, die die IOrderService-Schnittstelle implementieren")

**Abbildung 10-1:** Klassen, die die IOrderService-Schnittstelle implementieren

Dieser Ansatz ermöglicht es der `OrderService` Objekt, das Übergeben der `OrderDetailViewModel` Klasse zur Laufzeit, und klicken Sie im Interesse der testbarkeit, sie können die `OrderMockService` Klasse übergeben die `OrderDetailViewModel` Klasse während der Testphase. Der Hauptvorteil dieses Ansatzes ist, dass Komponententests ohne umständliche Ressourcen wie Web-Dienste oder Datenbanken ausgeführt werden können.

## <a name="testing-mvvm-applications"></a>Testen von MVVM-Anwendungen

Testen der Modelle und Ansichtsmodelle von MVVM-Anwendungen ist identisch mit dem Testen andere Klassen bilden, und die gleichen Tools und Techniken – z. B. die Komponententests und Mocks, können verwendet werden. Es gibt jedoch einige Muster, die typisch für Modell und ViewModel-Klassen, die von bestimmten Einheit Testverfahren profitieren können.

> [!TIP]
> Testen Sie eins mit jeder Komponententest. Seien Sie nicht versucht, eine Übung mehr als einen Aspekt des Verhaltens für die Einheit des test-Einheit zu machen. Auf diese Weise führt Tests, die schwer zu lesen und zu aktualisieren. Es kann auch zu Verwirrung führen, wenn einen Fehler.

Die eShopOnContainers-mobile app verwendet [xUnit](https://xunit.github.io/) zum Ausführen von Komponententests, die zwei verschiedene Arten von Komponententests unterstützt:

-   Fakten sind Tests, die immer "true", sind der invariante Bedingungen zu testen.
-   Theorien sind Tests, die nur für einen bestimmten Satz von Daten "true" sind.

Mit der mobilen app von eShopOnContainers enthaltenen Komponententests sind Tatsache Tests aus, und daher wird jede Komponententestmethode ergänzt, mit der `[Fact]` Attribut.

> [!NOTE]
> xUnit-Tests werden von einem Test Runner ausgeführt. Führen Sie zum Ausführen von Test Runner die eShopOnContainers.TestRunner-Projekt für die erforderliche Plattform ein.

### <a name="testing-asynchronous-functionality"></a>Testen der asynchronen Funktionalität

Wenn Sie das MVVM-Muster zu implementieren, rufen Sie anzeigemodelle in der Regel Vorgänge für Dienste, häufig asynchron. Tests für Code, der diese Vorgänge, in der Regel aufruft verwenden Sie Mocks als Ersatz für welche Dienste tatsächlich. Im folgenden Codebeispiel wird veranschaulicht, Testen von asynchronen Funktionalität durch ein mock-Dienst in einem View Model übergeben:

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

Testet dieser Komponententest überprüft, ob die `Order` Eigenschaft der `OrderDetailViewModel` Instanz hat einen Wert, nachdem die `InitializeAsync` Methode wurde aufgerufen. Die `InitializeAsync` Methode wird aufgerufen, wenn die entsprechende Ansicht des Ansichtsmodells zu dem navigiert wird. Weitere Informationen zur Navigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Wenn die `OrderDetailViewModel` Instanz erstellt, es erwartet, dass ein `OrderService` -Instanz als Argument angegeben werden. Allerdings die `OrderService` Ruft Daten aus einem Webdienst ab. Aus diesem Grund eine `OrderMockService` -Instanz, die eine Pseudoversion wird von der `OrderService` Klasse, die als Argument angegeben ist die `OrderDetailViewModel` Konstruktor. Dann, wenn des Ansichtsmodells `InitializeAsync` Methode wird aufgerufen, ruft die `IOrderService` Vorgänge simulierte Daten ist nicht mit einem Webdienst kommuniziert, sondern abgerufen.

### <a name="testing-inotifypropertychanged-implementations"></a>Testen von Implementierungen von "INotifyPropertyChanged"

Implementieren der `INotifyPropertyChanged` Schnittstelle ermöglicht es, Ansichten, um auf Änderungen reagieren, die aus der Sicht stammen Ansichtsmodelle und Modelle. Diese Änderungen sind nicht auf Daten in Steuerelementen beschränkt – sie werden auch verwendet, die Ansicht, z. B. Ansichtszustände-Modell zu steuern, die dazu führen, dass Animationen gestartet werden oder Steuerelemente deaktiviert werden soll.

Eigenschaften, die direkt vom Komponententest aktualisiert werden können, können durch Anhängen eines ereignishandlers zu getestet werden die `PropertyChanged` Ereignis- und überprüft, ob das Ereignis ausgelöst wird, nachdem Sie einen neuen Wert für die Eigenschaft festgelegt haben. Das folgende Codebeispiel zeigt einen solchen Test:

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

Testet dieser Komponententest ruft die `InitializeAsync` Methode der `OrderViewModel` Klasse, wodurch die `Order` zu aktualisierenden Eigenschaft. Der Komponententest wird bestanden, bereitgestellt, die die `PropertyChanged` Ereignis wird ausgelöst, für die `Order` Eigenschaft.

### <a name="testing-message-based-communication"></a>Testen die nachrichtenbasierte Kommunikation

Anzeigemodelle, verwenden die [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse für die Kommunikation zwischen lose gekoppelten Klassen kann Komponententests getestet werden durch Abonnieren der Nachricht gesendet werden, indem der zu testenden Code, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

Diese Komponententests überprüft, ob die `CatalogViewModel` veröffentlicht die `AddProduct` Nachricht als Antwort auf seine `AddCatalogItemCommand` ausgeführt wird. Da die [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse Multicastnachricht-Abonnements unterstützt, kann der Komponententest Abonnieren der `AddProduct` Nachricht, und führen Sie einen Callback-Delegaten als Reaktion auf erhalten. Callback-Delegaten, angegeben als ein Lambda-Ausdruck legt eine `boolean` Feld, das verwendet wird die `Assert` Anweisung, um das Verhalten des Tests zu überprüfen.

### <a name="testing-exception-handling"></a>Testen der Behandlung von Ausnahmen

Komponententests können auch geschrieben werden, überprüfen Sie, die für ungültige Aktionen oder Eingaben, bestimmte Ausnahmen ausgelöst werden wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

Testet dieser Komponententest wird eine Ausnahme ausgelöst, da die [ `ListView` ](xref:Xamarin.Forms.ListView) -Steuerelement verfügt nicht über ein Ereignis namens `OnItemTapped`. Die `Assert.Throws<T>` Methode ist eine generische Methode, in denen `T` ist der Typ der erwarteten Ausnahme. Das Argument zu übergeben, um die `Assert.Throws<T>` Methode ist ein Lambda-Ausdruck, der die Ausnahme ausgelöst wird. Aus diesem Grund der Komponententest wird bestanden, vorausgesetzt, dass der Lambda-Ausdruck löst einen `ArgumentException`.

> [!TIP]
> Vermeiden Sie das Schreiben von Komponententests, die Zeichenfolgen für Ausnahme zu untersuchen. Zeichenfolgen für Ausnahme im Laufe der Zeit ändern können, und Komponententests, die auf ihr Vorhandensein beruhen daher gelten als anfälligen.

### <a name="testing-validation"></a>Testen der Validierung

Es gibt zwei Aspekte zum Testen der Implementierung der Validierung: testen, ob alle Validierungsregeln ordnungsgemäß implementiert werden, und testen, die die `ValidatableObject<T>` Klasse wie erwartet ausgeführt wird.

Validierungslogik ist in der Regel einfach zu testen, da sie in der Regel einen eigenständigen Prozess ist, bei der Eingabe die Ausgabe abhängig ist. Vorhanden sein soll Tests die Ergebnisse eines Aufrufs der `Validate` Methode für jede Eigenschaft, die mindestens eine zugehörige Überprüfungsregel, wie im folgenden Codebeispiel gezeigt:

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

Diese Komponententests überprüft, dass die Überprüfung erfolgreich ist, wenn die beiden `ValidatableObject<T>` Eigenschaften in der `MockViewModel` Instanz sowohl über Daten verfügen.

Und überprüfen, dass die Überprüfung erfolgreich ist, Komponententests für die Validierung hinaus sollten Sie überprüfen die Werte der `Value`, `IsValid`, und `Errors` Eigenschaft der einzelnen `ValidatableObject<T>` Instanz, um sicherzustellen, dass die Klasse wie erwartet ausgeführt wird. Im folgenden Codebeispiel wird veranschaulicht, einen Komponententest, der ihn durchführt:

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

Diese Komponententests überprüft, dass die Überprüfung schlägt fehl, wenn die `Surname` Eigenschaft der `MockViewModel` verfügt nicht über alle Daten, und die `Value`, `IsValid`, und `Errors` Eigenschaft der einzelnen `ValidatableObject<T>` Instanz korrekt eingestellt sind.

## <a name="summary"></a>Zusammenfassung

Ein Komponententest nimmt eine kleine Einheit der app, in der Regel eine Methode, wird es vom Rest des Codes isoliert und stellt sicher, dass sie sich erwartungsgemäß verhält. Ziel ist es, überprüfen Sie, dass jede Einheit der Funktionalität führt erwartungsgemäß funktioniert, sodass Fehler in der gesamten app weitergegeben werden, nicht.

Durch Ersetzen von abhängigen Objekte mit Pseudoobjekten, die das Verhalten der abhängige Objekte zu simulieren, kann das Verhalten eines Objekts im Test isoliert werden. Dadurch können die Komponententests ausgeführt werden, ohne umständliche Ressourcen wie Web-Dienste oder Datenbanken.

Testen der Modelle und Ansichtsmodelle von MVVM-Anwendungen ist identisch mit dem Testen andere Klassen bilden, und die gleichen Tools und Techniken können verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
