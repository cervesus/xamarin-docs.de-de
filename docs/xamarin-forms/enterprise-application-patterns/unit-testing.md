---
title: Komponententests für Unternehmens-apps
description: In diesem Kapitel wird erläutert, wie Unittests in den eshoponcontainers-Mobile App durchgeführt werden.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0fb63c650e73bce5a08b204f942f0c19583e4899
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770687"
---
# <a name="unit-testing-enterprise-apps"></a>Komponententests für Unternehmens-apps

Mobile Apps haben eindeutige Probleme, die Desktop-und webbasierten Anwendungen nicht berücksichtigen müssen. Mobile Benutzer unterscheiden sich von den Geräten, die Sie verwenden, durch Netzwerk Konnektivität, durch die Verfügbarkeit von Diensten und einen Bereich anderer Faktoren. Daher sollten Mobile Apps getestet werden, da Sie in der realen Welt verwendet werden, um ihre Qualität, Zuverlässigkeit und Leistung zu verbessern. Es gibt viele Arten von Tests, die für eine app ausgeführt werden sollten, einschließlich Komponententests, Integrationstests und Tests der Benutzeroberfläche, wobei Komponententests die gängigste Art von Tests sind.

Bei einem Komponenten Test wird eine kleine Einheit der APP benötigt, in der Regel eine Methode, die von dem Rest des Codes isoliert und überprüft wird, ob Sie sich erwartungsgemäß verhält. Ziel ist es, zu überprüfen, ob jede Funktionseinheit erwartungsgemäß funktioniert, damit Fehler nicht in der gesamten App verbreitet werden. Es ist effizienter, einen Fehler zu erkennen, bei dem der Fehler auftritt, um die Auswirkungen eines Fehlers indirekt an einem sekundären Zeitpunkt des Fehlers zu beobachten.

Komponententests haben die größte Auswirkung auf die Codequalität, wenn Sie ein integraler Bestandteil des Software Entwicklungs Workflows sind. Sobald eine Methode geschrieben wurde, müssen Komponententests geschrieben werden, die das Verhalten der Methode als Reaktion auf Standard-, Begrenzungs-und falsche Fälle von Eingabedaten überprüfen und alle expliziten oder impliziten Annahmen des Codes überprüfen. Alternativ dazu werden Komponententests bei der Test orientierten Entwicklung vor dem Code geschrieben. In diesem Szenario fungieren Komponententests sowohl als Entwurfsdokumentation als auch als funktionale Spezifikationen.

> [!NOTE]
> Komponententests sind bei Regression sehr effektiv – d. h. Funktionen, die verwendet wurden, um jedoch durch ein fehlerhaftes Update gestört zu werden.

Komponententests verwenden normalerweise das Muster "Arrange-Act-Assert":

- Der *Anordnungs* Abschnitt der Komponenten Testmethode initialisiert Objekte und legt den Wert der Daten fest, die an die zu testende Methode weitergegeben werden.
- Der *Act* -Abschnitt Ruft die zu testende Methode mit den erforderlichen Argumenten auf.
- Der *Assert* -Abschnitt überprüft, ob sich die Aktion der zu testenden Methode wie erwartet verhält.

Wenn Sie dieses Muster befolgen, wird sichergestellt, dass Komponententests lesbar und konsistent sind.

## <a name="dependency-injection-and-unit-testing"></a>Abhängigkeitsinjektion und Unittests

Einer der Beweggründe für die Einführung einer lose gekoppelten Architektur besteht darin, dass Komponententests vereinfacht werden. Einer der Typen, die bei autofac registriert sind `OrderService` , ist die-Klasse. Das folgende Codebeispiel zeigt eine Gliederung dieser Klasse:

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

Die `OrderDetailViewModel` -Klasse hat eine Abhängigkeit von `IOrderService` dem Typ, den der Container auflöst, wenn er ein `OrderDetailViewModel` -Objekt instanziiert. Anstatt jedoch ein `OrderService` -Objekt für den Komponenten Test der `OrderDetailViewModel` -Klasse zu erstellen, ersetzen Sie `OrderService` stattdessen das-Objekt durch ein Mock zum Zweck der Tests. In Abbildung 10-1 wird diese Beziehung veranschaulicht.

![](unit-testing-images/unittesting.png "Klassen, die die IOrderService-Schnittstelle implementieren")

**Abbildung 10-1:** Klassen, die die IOrderService-Schnittstelle implementieren

Bei diesem Ansatz kann `OrderService` das Objekt zur Laufzeit an die `OrderDetailViewModel` -Klasse übermittelt werden, und im Interesse der Testability kann die `OrderMockService` Klasse zur Testzeit an die `OrderDetailViewModel` -Klasse übermittelt werden. Der Hauptvorteil dieses Ansatzes besteht darin, dass Komponententests ausgeführt werden können, ohne dass dafür Ressourcen wie z. b. Webdienste oder Datenbanken erforderlich sind.

## <a name="testing-mvvm-applications"></a>Testen von MVVM-Anwendungen

Das Testen von Modellen und Anzeigen von Modellen von MVVM-Anwendungen ist identisch mit dem Testen von anderen Klassen, und die gleichen Tools und Techniken – wie z. b. Komponententests und das simulieren können verwendet werden. Es gibt jedoch einige Muster, die typisch für das Modellieren und Anzeigen von Modellklassen sind, die von speziellen Verfahren für Unittests profitieren können.

> [!TIP]
> Testen Sie eine Aufgabe mit jedem Komponenten Test. Seien Sie nicht in der Versuchung, einen Komponenten Test mehr als einen Aspekt des Verhaltens der Einheit durchführen zu lassen. Dies führt zu Tests, die schwer zu lesen und zu aktualisieren sind. Dies kann auch zu Verwirrung führen, wenn ein Fehler interpretiert wird.

Der eshoponcontainers-Mobile App verwendet [xUnit](https://xunit.github.io/) zum Ausführen von Komponententests, die zwei verschiedene Arten von Komponententests unterstützen:

- Fakten sind Tests, bei denen es sich immer um true handelt, die invariante Bedingungen testen.
- Bei den Theorien handelt es sich um Tests, die nur für einen bestimmten Datensatz zutreffen.

Die Komponententests, die in den eshoponcontainers-Mobile App enthalten sind, sind Fakten Tests, sodass jede Komponenten Testmethode `[Fact]` mit dem-Attribut versehen wird.

> [!NOTE]
> xUnit-Tests werden von einem Test Runner ausgeführt. Um den Test Runner auszuführen, führen Sie das Projekt eshoponcontainers. testranunner für die erforderliche Plattform aus.

### <a name="testing-asynchronous-functionality"></a>Testen der asynchronen Funktionalität

Wenn Sie das MVVM-Muster implementieren, rufen Ansichts Modelle normalerweise Vorgänge für Dienste auf, häufig asynchron. Tests für Code, in dem diese Vorgänge aufgerufen werden, werden in der Regel als Ersatz für die eigentlichen Dienste verwendet. Im folgenden Codebeispiel wird veranschaulicht, wie asynchrone Funktionen getestet werden, indem ein Mock-Dienst an ein Ansichts Modell übergeben wird:

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

Dieser Komponenten Test überprüft, ob `Order` die-Eigenschaft `OrderDetailViewModel` der-Instanz einen Wert hat, `InitializeAsync` nachdem die-Methode aufgerufen wurde. Die `InitializeAsync` -Methode wird aufgerufen, wenn die entsprechende Ansicht des Ansichts Modells navigiert wird. Weitere Informationen zur Navigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Wenn die `OrderDetailViewModel` Instanz erstellt wird, wird erwartet, `OrderService` dass eine Instanz als Argument angegeben wird. `OrderService` Ruft jedoch Daten von einem Webdienst ab. Daher wird eine `OrderMockService` -Instanz, bei der es sich um eine `OrderService` Pseudo Version der-Klasse handelt, als Argument `OrderDetailViewModel` für den-Konstruktor angegeben. Wenn dann die-Methode des Ansichts `InitializeAsync` Modells aufgerufen wird, `IOrderService` die Vorgänge aufruft, werden Mock-Daten abgerufen, anstatt mit einem Webdienst zu kommunizieren.

### <a name="testing-inotifypropertychanged-implementations"></a>Testen von INotifyPropertyChanged-Implementierungen

Durch das `INotifyPropertyChanged` Implementieren der-Schnittstelle können Sichten auf Änderungen reagieren, die aus Sicht Modellen und Modellen stammen. Diese Änderungen sind nicht auf Daten beschränkt, die in Steuerelementen angezeigt werden – Sie werden auch verwendet, um die Ansicht zu steuern, z. b. Ansichts Modell Zustände, die bewirken, dass Animationen gestartet oder Steuerelemente deaktiviert werden.

Eigenschaften, die direkt durch den Komponenten Test aktualisiert werden können, können getestet werden, indem ein Ereignishandler `PropertyChanged` an das-Ereignis angefügt wird und überprüft wird, ob das Ereignis ausgelöst wird, nachdem ein neuer Wert für die-Eigenschaft festgelegt wurde. Das folgende Codebeispiel zeigt einen solchen Test:

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

Dieser Komponenten Test Ruft die `InitializeAsync` -Methode `OrderViewModel` der- `Order` Klasse auf, die bewirkt, dass die-Eigenschaft aktualisiert wird. Der Komponenten Test wird bestanden, vorausgesetzt, `PropertyChanged` das-Ereignis wird für `Order` die-Eigenschaft ausgelöst.

### <a name="testing-message-based-communication"></a>Testen der Nachrichten basierten Kommunikation

Ansichts Modelle, die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) die-Klasse für die Kommunikation zwischen lose gekoppelten Klassen verwenden, können durch Abonnieren der Nachricht, die von dem getesteten Code gesendet wird, Komponententests durchführen, wie im folgenden Codebeispiel gezeigt:

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

Dieser Komponenten Test überprüft, ob `CatalogViewModel` die `AddProduct` Nachricht als Antwort auf Ihre `AddCatalogItemCommand` ausgeführte veröffentlicht. Da die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) -Klasse Multicast Nachrichten Abonnements unterstützt, kann der-Komponenten Test `AddProduct` die Nachricht abonnieren und als Antwort auf den Empfang einen Rückruf Delegaten ausführen. Dieser Rückruf Delegat, der als Lambda-Ausdruck angegeben wird `boolean` , legt ein Feld fest, `Assert` das von der-Anweisung verwendet wird, um das Verhalten des Tests zu überprüfen.

### <a name="testing-exception-handling"></a>Testen der Ausnahmebehandlung

Komponententests können auch geschrieben werden, um zu überprüfen, ob bestimmte Ausnahmen für ungültige Aktionen oder Eingaben ausgelöst werden, wie im folgenden Codebeispiel gezeigt:

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

Dieser Komponenten Test löst eine Ausnahme aus, da das [`ListView`](xref:Xamarin.Forms.ListView) Steuerelement nicht über ein Ereignis mit `OnItemTapped`dem Namen verfügt. Bei `Assert.Throws<T>` der-Methode handelt es sich `T` um eine generische Methode, bei der der Typ der erwarteten Ausnahme ist. Das an die `Assert.Throws<T>` -Methode übergeben Argument ist ein Lambda-Ausdruck, der die Ausnahme auslöst. Daher wird der Komponenten Test durchlaufen, wenn der Lambda-Ausdruck eine `ArgumentException`auslöst.

> [!TIP]
> Schreiben Sie keine Komponententests, die Ausnahme Meldungs Zeichenfolgen untersuchen Ausnahme Meldungs Zeichenfolgen können sich im Laufe der Zeit ändern, sodass Komponententests, die auf ihrer Anwesenheit basieren, als fehleranfällig angesehen werden.

### <a name="testing-validation"></a>Testen der Validierung

Es gibt zwei Aspekte beim Testen der Überprüfungs Implementierung: das Testen, ob alle Validierungsregeln ordnungsgemäß implementiert sind, und `ValidatableObject<T>` das Testen, dass die Klasse erwartungsgemäß funktioniert.

Validierungs Logik ist in der Regel einfach zu testen, da es sich in der Regel um einen eigenständigen Prozess handelt, bei dem die Ausgabe von der Eingabe abhängig ist. Es sollten Tests zu den Ergebnissen des `Validate` Aufrufs der-Methode für jede Eigenschaft vorhanden sein, die mindestens eine zugeordnete Validierungs Regel aufweist, wie im folgenden Codebeispiel gezeigt:

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

Dieser Komponenten Test prüft, ob die Überprüfung erfolgreich `ValidatableObject<T>` ist, wenn `MockViewModel` die beiden Eigenschaften in der Instanz Daten aufweisen.

Wenn Sie überprüfen möchten, ob die Überprüfung erfolgreich ist, sollten Sie auch die Werte `Value`der `IsValid`Eigenschaften, `Errors` und der einzelnen `ValidatableObject<T>` Instanzen überprüfen, um zu überprüfen, ob die Klasse erwartungsgemäß funktioniert. Im folgenden Codebeispiel wird ein Komponenten Test veranschaulicht, der Folgendes bewirkt:

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

Dieser Komponenten Test überprüft, ob die Validierung fehl `Surname` schlägt, wenn `MockViewModel` die-Eigenschaft von keine Daten enthält und `IsValid`die- `Errors` Eigenschaft, die `ValidatableObject<T>` -Eigenschaft und die `Value`-Eigenschaft der einzelnen Instanzen ordnungsgemäß festgelegt sind.

## <a name="summary"></a>Zusammenfassung

Bei einem Komponenten Test wird eine kleine Einheit der APP benötigt, in der Regel eine Methode, die von dem Rest des Codes isoliert und überprüft wird, ob Sie sich erwartungsgemäß verhält. Ziel ist es, zu überprüfen, ob jede Funktionseinheit erwartungsgemäß funktioniert, damit Fehler nicht in der gesamten App verbreitet werden.

Das Verhalten eines zu testenden Objekts kann isoliert werden, indem abhängige Objekte durch Mock-Objekte ersetzt werden, die das Verhalten der abhängigen Objekte simulieren. Dadurch wird die Ausführung von Komponententests ermöglicht, ohne dass Ressourcen wie z. b. Webdienste oder Datenbanken erforderlich sind.

Das Testen von Modellen und Anzeigen von Modellen von MVVM-Anwendungen ist identisch mit dem Testen anderer Klassen, und es können dieselben Tools und Techniken verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
