---
title: Zugreifen auf Remotedaten
description: In diesem Kapitel wird erläutert, wie die "eshoponcontainers" mobile app aus der Microservices in Containern auf Daten zugreift.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a4c58139b0ddbaaedf5769eeac6585bac4c013e4
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832114"
---
# <a name="accessing-remote-data"></a>Zugreifen auf Remotedaten

Viele moderne webbasierte Lösungen stellen Verwendung von Webdiensten, Webservern gehostete Webdienste, um Funktionen für remote-Client-Anwendungen bereitzustellen. Die Vorgänge, die einen Webdienst verfügbar macht, bilden eine Web-API.

Client-apps sollte die Web-API nutzen, ohne zu wissen, wie die Daten oder Vorgänge, die die-API implementiert werden können. Dies erfordert, dass die API allgemeine Standards befolgt, mit denen einen Client-app und Web-Dienst zuzustimmen Datenformate zu verwenden und die Struktur der Daten, die zwischen Client-apps und dem Webdienst ausgetauscht werden.

## <a name="introduction-to-representational-state-transfer"></a>Einführung in Representational State Transfer

Representational State Transfer (REST) ist ein Architekturstil zum Erstellen verteilter Systeme, die auf Hypermedia basieren. Ein wichtiger Vorteil des REST-Modells ist, dass es auf offenen Standards basierende hat und nicht die Implementierung für das Modell oder die Client-apps, die es den Zugriff auf eine bestimmte Implementierung gebunden. Aus diesem Grund könnte ein REST-Webdienst mit Microsoft ASP.NET Core MVC implementiert werden, und Client-apps können Entwickeln mit allen Sprachen und Toolsets, die HTTP-Anforderungen generieren und HTTP-Antworten analysieren kann.

REST-Modell verwendet ein Navigationsschema zur Darstellung von Objekten und-Diensten über ein Netzwerk, die als Ressourcen bezeichnet. Systeme, die in der Regel implementieren Sie REST verwenden das HTTP-Protokoll zum Übertragen von Anforderungen auf diese Ressourcen zugreifen. In solchen Systemen sendet eine Client-app eine Anforderung in Form von ein URI, der eine Ressource kennzeichnet, und eine HTTP-Methode (z. B. GET, POST, PUT oder DELETE), die den Vorgang, für die Ressource ausgeführt werden angibt. Der Text der HTTP-Anforderung enthält alle Daten, die zum Ausführen des Vorgangs erforderlich.

> [!NOTE]
> REST definiert ein Zustandsloses Anforderungsmodell. Aus diesem Grund werden die HTTP-Anforderungen müssen unabhängig sein und können in beliebiger Reihenfolge auftreten.

Die Antwort von einem REST-Anforderung ist der standard-HTTP-Statuscodes verwendet. Beispielsweise sollte eine Anforderung, die gültige Daten zurückgibt HTTP-Antwortcode 200 (OK), enthalten, während eine Anforderung, die nicht finden oder löschen eine angegebene Ressource auf eine Antwort zurückgeben sollte, die HTTP-Statuscode 404 (nicht gefunden) enthält.

Eine RESTful-Web-API macht eine Reihe von verbundenen Ressourcen verfügbar, und stellt die Core-Vorgänge, mit denen eine app zum Bearbeiten dieser Ressourcen und problemlos zwischen diesen navigieren. Aus diesem Grund sind die URIs, die eine typische RESTful-Web-API bilden auf die Daten, die sie verfügbar macht, und verwenden die Funktionen von HTTP zum Verarbeiten von Daten ausgerichtet.

Eine Client-app in einer HTTP-Anforderung und die entsprechenden Antwortnachrichten vom Webserver, enthaltenen Daten können in einer Vielzahl von Formaten, die als Medientypen angezeigt. Wenn eine Clientanwendung eine Anforderung, die Daten im Text einer Nachricht zurückgibt sendet, können sie angeben, die Medientypen, die sie verarbeitet werden kann die `Accept` -Header der Anforderung. Wenn der Webserver diesen Medientyp unterstützt, können die Antworten mit einer Antwort, umfasst die `Content-Type` Header, der das Format der Daten im Text der Nachricht angibt. Es ist dann die Verantwortung für die Client-app zu analysieren, die Response-Nachricht die Ergebnisse im Nachrichtentext richtig interpretieren.

Weitere Informationen zu REST finden Sie unter [API-Design](/azure/architecture/best-practices/api-design/) und [-API-Implementierung](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Nutzen von RESTful-APIs

Die eShopOnContainers-mobile-app verwendet das Model-View-ViewModel (MVVM) Muster und die Modellelemente, der das Muster dar, die die Domänenentitäten in der app verwendet. Die Controller und Repository-Klassen in der referenzanwendung "eshoponcontainers" akzeptieren und viele dieser Modellobjekte zurück. Aus diesem Grund werden sie als datenübertragungsobjekte (DTOs) verwendet, die alle Daten enthalten, die zwischen der mobilen app und die Microservices in Containern übergeben wird. Der Hauptvorteil der Verwendung von DTOs zum Übergeben von Daten an und Empfangen von Daten von einem Webdienst ist, dass mehr Daten übertragen werden in einem einzelnen Remoteaufruf, die app die Anzahl von Remoteaufrufen reduzieren kann, die vorgenommen werden müssen.

### <a name="making-web-requests"></a>Ausführen von Webanforderungen

Die eShopOnContainers-mobile app verwendet die `HttpClient` Klasse, um Anfragen über HTTP mit JSON als Medientyp verwendet wird. Diese Klasse bietet Funktionen für die asynchrone HTTP-Anforderungen senden und Empfangen von HTTP-Antworten aus einem URI identifizierte Ressource. Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht, die von einer REST-API empfangen werden, nachdem eine HTTP-Anforderung erfolgt ist. Es enthält Informationen über die Antwort, einschließlich der Statuscode, Header und alle Text. Die `HttpContent` Klasse stellt die HTTP-Text und die Inhaltsheader, z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann mit einer der gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`, je nachdem, auf das Format der Daten.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Eine GET-Anforderung

Die `CatalogService` Klasse dient zum Abrufen von Daten aus dem katalogmicroservice zu verwalten. In der `RegisterDependencies` -Methode in der die `ViewModelLocator` -Klasse, die `CatalogService` Klasse wird als eine Typzuordnung für registriert die `ICatalogService` Typ mit dem Container für Abhängigkeitsinjektion Autofac. Klicken Sie dann, wenn eine Instanz von der `CatalogViewModel` Klasse wird erstellt, die ihr Konstruktor nimmt eine `ICatalogService` eingeben, die Autofac aufgelöst wird, Zurückgeben von einer Instanz von der `CatalogService` Klasse. Weitere Informationen zur Abhängigkeitsinjektion finden Sie unter [Einführung in Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Abbildung 10-1 zeigt die Interaktion von Klassen, die gelesen werden der katalogmicroservice für die Anzeige von Daten im Katalog die `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Abrufen von Daten aus dem katalogmicroservice")](accessing-remote-data-images/catalogdata-large.png#lightbox "Abrufen von Daten aus dem katalogmicroservice")

**Abbildung 10-1**: Abrufen von Daten aus dem katalogmicroservice

Wenn die `CatalogView` navigiert wird, die `OnInitialize` -Methode in der die `CatalogViewModel` -Klasse aufgerufen wird. Diese Methode ruft Katalogdaten aus den katalogmicroservice ab, wie im folgenden Codebeispiel gezeigt:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Diese Methode ruft die `GetCatalogAsync` Methode der `CatalogService` -Instanz, die in eingefügt wurde die `CatalogViewModel` von Autofac. Die `GetCatalogAsync`-Methode wird in folgendem Codebeispiel veranschaulicht:

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

Diese Methode den URI, der die Ressource identifiziert, an die Anforderung, erstellt und verwendet die `RequestProvider` Klasse, um die HTTP GET-Methode für diese Ressource aufzurufen, vor der Rückgabe der Ergebnisse an die `CatalogViewModel`. Die `RequestProvider` -Klasse enthält Funktionen, die eine Anforderung in Form eines URI sendet, die eine Ressource, die eine HTTP-Methode identifiziert, der den Vorgang für diese Ressource auszuführende angibt und ein Text ein, die keine Daten enthält, die zum Ausführen des Vorgangs erforderlich sind. Informationen darüber, wie die `RequestProvider` Klasse injiziert den `CatalogService class`, finden Sie unter [Einführung in Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Das folgende Codebeispiel zeigt die `GetAsync` -Methode in der die `RequestProvider` Klasse:

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Diese Methode ruft die `CreateHttpClient` -Methode, die eine Instanz der zurückgibt der `HttpClient` Klasse mit den entsprechenden Header. Es sendet dann eine asynchrone GET-Anforderung an die Ressource, die durch den URI identifiziert wird, mit der Antwort, die in gespeichert werden die `HttpResponseMessage` Instanz. Die `HandleResponse` Methode dann aufgerufen werden kann, die eine Ausnahme auslöst, wenn die Antwort auf Erfolg HTTP-Statuscode enthalten nicht. Und dann die Antwort als Zeichenfolge aus einer JSON-Code für konvertierte gelesen wird eine `CatalogRoot` Objekt aus, und zurückgegeben werden, um die `CatalogService`.

Die `CreateHttpClient` Methode ist im folgenden Codebeispiel gezeigt:

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

Diese Methode erstellt eine neue Instanz der der `HttpClient` Klasse, wobei die `Accept` Header, der alle Anforderungen, die von der `HttpClient` -Instanz, auf `application/json`, was bedeutet, dass es erwartet, dass den Inhalt der jede Antwort auf die mit JSON formatiert werden. Klicken Sie dann, wenn ein Zugriffstoken als Argument übergeben wurde die `CreateHttpClient` -Methode, die sie hinzugefügt wird, auf die `Authorization` Header, der alle Anforderungen von der `HttpClient` Instanz, mit der Zeichenfolge das Präfix `Bearer`. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Wenn die `GetAsync` -Methode in der die `RequestProvider` -Klasse ruft `HttpClient.GetAsync`, wird die `Items` -Methode in der die `CatalogController` Klasse im Catalog.API-Projekt aufgerufen, die im folgenden Codebeispiel dargestellt ist:

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

Diese Methode ruft die Katalogdaten aus der SQL-Datenbank mithilfe von EntityFramework. ab und gibt ihn als eine Response-Nachricht, die eine erfolgreiche HTTP-Statuscode enthält, und eine Sammlung von JSON-formatierte `CatalogItem` Instanzen.

#### <a name="making-a-post-request"></a>Eine POST-Anforderung

Die `BasketService` Klasse wird verwendet, um das Abrufen von Daten zu verwalten und den Aktualisierungsvorgang mit Warenkorb-Microservice. In der `RegisterDependencies` -Methode in der die `ViewModelLocator` -Klasse, die `BasketService` Klasse wird als eine Typzuordnung für registriert die `IBasketService` Typ mit dem Container für Abhängigkeitsinjektion Autofac. Klicken Sie dann, wenn eine Instanz von der `BasketViewModel` Klasse wird erstellt, die ihr Konstruktor nimmt eine `IBasketService` eingeben, die Autofac aufgelöst wird, Zurückgeben von einer Instanz von der `BasketService` Klasse. Weitere Informationen zur Abhängigkeitsinjektion finden Sie unter [Einführung in Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Abbildung 10-2 zeigt die Interaktion von Klassen, die die Warenkorb angezeigten Daten durch Senden der `BasketView`, mit dem warenkorbmicroservice.

[![](accessing-remote-data-images/basketdata.png "Senden von Daten mit dem warenkorbmicroservice")](accessing-remote-data-images/basketdata-large.png#lightbox "Senden von Daten mit dem warenkorbmicroservice")

**Abbildung 10-2**: Senden von Daten mit dem warenkorbmicroservice

Wenn ein Element, im Warenkorb abgelegt hinzugefügt wird, der `ReCalculateTotalAsync` -Methode in der die `BasketViewModel` -Klasse aufgerufen wird. Diese Methode aktualisiert den Gesamtwert der Artikel im Warenkorb, und sendet die Warenkorb-Daten an die Warenkorb-Microservice, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

Diese Methode ruft die `UpdateBasketAsync` Methode der `BasketService` -Instanz, die in eingefügt wurde die `BasketViewModel` von Autofac. Die folgende Methode zeigt die `UpdateBasketAsync` Methode:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Diese Methode den URI, der die Ressource identifiziert, an die Anforderung, erstellt und verwendet die `RequestProvider` Klasse, um die HTTP POST-Methode für diese Ressource aufzurufen, vor der Rückgabe der Ergebnisse an die `BasketViewModel`. Ein Zugriffstoken aus Identity Server während des Authentifizierungsprozesses ist erforderlich, um Anforderungen mit dem warenkorbmicroservice zu autorisieren. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Im folgenden Codebeispiel wird veranschaulicht, eines der `PostAsync` Methoden in der `RequestProvider` Klasse:

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Diese Methode ruft die `CreateHttpClient` -Methode, die eine Instanz der zurückgibt der `HttpClient` Klasse mit den entsprechenden Header. Es sendet dann eine asynchrone POST-Anforderung an die Ressource durch den URI mit den serialisierten Warenkorb-Daten gesendet werden, in der JSON-Format und die Antwort, die in gespeichert werden die `HttpResponseMessage` Instanz. Die `HandleResponse` Methode dann aufgerufen werden kann, die eine Ausnahme auslöst, wenn die Antwort auf Erfolg HTTP-Statuscode enthalten nicht. Klicken Sie dann die Antwort wird als eine Zeichenfolge, die aus einer JSON-Code für konvertierte gelesen eine `CustomerBasket` Objekt aus, und zurückgegeben werden, um die `BasketService`. Weitere Informationen zu den `CreateHttpClient` -Methode finden Sie unter [vornehmen, eine GET-Anforderung](#making_a_get_request).

Wenn die `PostAsync` -Methode in der die `RequestProvider` -Klasse ruft `HttpClient.PostAsync`, `Post` -Methode in der die `BasketController` Klasse im Projekt "Basket.API" wird aufgerufen, die im folgenden Codebeispiel dargestellt ist:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Diese Methode wird eine Instanz der dem `RedisBasketRepository` Klasse, um die Warenkorb-Daten auf der Redis-Cache beizubehalten, und gibt zurück, wie eine Response-Nachricht, die eine erfolgreiche HTTP-Statuscode und eine JSON-Code enthält formatiert `CustomerBasket` Instanz.

#### <a name="making-a-delete-request"></a>Eine DELETE-Anforderung

Abbildung 10-3 zeigt die Interaktionen von Klassen, die Warenkorb-Daten aus der warenkorbmicroservice Löschen der `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Löschen von Daten aus dem warenkorbmicroservice")

**Abbildung 10-3**: Löschen von Daten aus dem warenkorbmicroservice

Beim Aufrufen des Auscheckvorgangs der `CheckoutAsync` -Methode in der die `CheckoutViewModel` -Klasse aufgerufen wird. Diese Methode erstellt einen neuen Auftrag vor dem Löschen der Einkaufswagen, wie im folgenden Codebeispiel gezeigt:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Diese Methode ruft die `ClearBasketAsync` Methode der `BasketService` -Instanz, die in eingefügt wurde die `CheckoutViewModel` von Autofac. Die folgende Methode zeigt die `ClearBasketAsync` Methode:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Diese Methode erstellt den URI, der die Ressource identifiziert, die an die Anforderung gesendet wird, und verwendet die `RequestProvider` Klasse, um die DELETE HTTP-Methode für die Ressource aufzurufen. Ein Zugriffstoken aus Identity Server während des Authentifizierungsprozesses ist erforderlich, um Anforderungen mit dem warenkorbmicroservice zu autorisieren. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Das folgende Codebeispiel zeigt die `DeleteAsync` -Methode in der die `RequestProvider` Klasse:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Diese Methode ruft die `CreateHttpClient` -Methode, die eine Instanz der zurückgibt der `HttpClient` Klasse mit den entsprechenden Header. Er sendet dann eine asynchrone DELETE-Anforderung an die durch den URI identifizierte Ressource. Weitere Informationen zu den `CreateHttpClient` -Methode finden Sie unter [vornehmen, eine GET-Anforderung](#making_a_get_request).

Wenn die `DeleteAsync` -Methode in der die `RequestProvider` -Klasse ruft `HttpClient.DeleteAsync`, `Delete` -Methode in der die `BasketController` Klasse im Projekt "Basket.API" wird aufgerufen, die im folgenden Codebeispiel dargestellt ist:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Diese Methode wird eine Instanz der dem `RedisBasketRepository` Klasse, um die Warenkorb-Daten aus dem Redis-Cache zu löschen.

## <a name="caching-data"></a>Zwischenspeichern von Daten

Die Leistung einer App kann verbessert werden, durch das Zwischenspeichern von häufig verwendete Daten in einen schnellen Speicher, der Nähe befindet der app. Wenn der schnellere Speicher ist näher an die app als die ursprüngliche Datenquelle befindet, Zwischenspeichern kann die Antwort erheblich verbessern Timeout beim Abrufen von Daten.

Die häufigste Form des Cachings ist Read-through-caching, in denen eine app Daten von Verweisen auf den Cache abruft. Wenn die Daten im Cache nicht ist, wurde es aus dem Datenspeicher abgerufen und dem Cache hinzugefügt. Apps können die Read-through-caching mit dem Cache-Aside-Muster implementieren. Dieses Muster wird bestimmt, ob das Element derzeit im Cache vorhanden ist. Wenn das Element nicht im Cache befinden, aus dem Datenspeicher gelesene und dem Cache hinzugefügt. Weitere Informationen finden Sie unter den [Cache-Aside](/azure/architecture/patterns/cache-aside/) Muster.

> [!TIP]
> Zwischenspeichern von Daten, die häufig gelesen und selten. Diese Daten können hinzugefügt werden, in den Cache bei Bedarf beim ersten, dass sie von einer app abgerufen werden. Dies bedeutet, dass die app benötigt, um die Daten nur einmal aus dem Datenspeicher abzurufen, und der nachfolgende Zugriff mit dem Cache erfüllt werden kann.

Verteilte Anwendungen sollten wie z. B. die eShopOnContainers-Anwendung verweisen eine oder beide der folgenden Caches bereitstellen:

-   Einen freigegebenen Cache, die von mehreren Prozessen oder Computern zugegriffen werden kann.
-   Eines privaten Caches, in dem Daten lokal auf dem Gerät, das Ausführen der app gespeichert wird.

Die eShopOnContainers-mobile-app verwendet einen privaten Cache, in dem Daten lokal auf dem Gerät befindet, die eine Instanz der app ausgeführt wird. Weitere Informationen zu den Cache, die von der referenzanwendung "eshoponcontainers" verwendet, finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

> [!TIP]
> Stellen Sie sich den Cache als vorübergehende Datenspeicher, der jederzeit entfernt werden konnte. Stellen Sie sicher, dass Daten in den ursprünglichen Datenspeicher als auch für den Cache erhalten bleibt. Die Wahrscheinlichkeit eines Datenverlusts werden dann minimiert werden, wenn der Cache nicht verfügbar ist.

### <a name="managing-data-expiration"></a>Verwalten von Ablauf von Daten

Es ist nicht erwartet, dass zwischengespeicherte Daten immer mit den ursprünglichen Daten konsistent ist. Daten in den ursprünglichen Datenspeicher möglicherweise ändern, nachdem er ist verursacht die zwischengespeicherten Daten zwischengespeichert wurde veralten. Aus diesem Grund sollten apps implementieren eine Strategie, um sicherzustellen, dass die Daten im Cache so aktuell wie möglich, jedoch können auch erkennen und Behandeln von Situationen, in denen auftreten, wenn die Daten im Cache veraltet sind. Die meisten Zwischenspeicherungsmechanismen aktivieren den Cache konfiguriert werden, um die Daten ablaufen, und daher reduzieren den Zeitraum für den Daten möglicherweise nicht mehr aktuell.

> [!TIP]
> Legen Sie eine Standardablaufdauer von Zeit, wenn Sie einen Cache zu konfigurieren. Viele Caches implementieren Ablauf, der Daten ungültig gemacht und entfernt sie aus dem Cache ein, wenn es für einen bestimmten Zeitraum nicht zugegriffen werden kann. Allerdings muss darauf geachtet werden bei der Auswahl der Ablaufzeitraum fest. Wenn sie nicht zu kurz festgelegt ist, Daten zu schnell abläuft und die Vorteile des Zwischenspeicherns werden reduziert. Wenn sie zu lang, Risiken Daten veraltet sind nicht festgelegt ist. Aus diesem Grund sollte die Ablaufzeit das Muster des Zugriffs für apps übereinstimmen, die die Daten zu verwenden.

Zwischengespeicherte Daten laufen ab, es sollte entfernt werden, aus dem Cache und die app abrufen muss die Daten aus der ursprünglichen Daten speichern, und platzieren Sie es wieder in den Cache.

Es ist auch möglich, dass ein Cache ansammeln kann, wenn die Daten bleiben für einen zu langen Zeitraum zugelassen werden. Aus diesem Grund Anforderungen zum Hinzufügen neuer Elemente in den Cache erforderlich sein, entfernen Sie einige Elemente in einem Prozess namens *Entfernung*. Cachingdienste entfernen in der Regel Daten für die Least-recently-used. Es gibt jedoch andere entfernungsrichtlinien, einschließlich der vor kurzem verwendet und First-in-First-Out. Weitere Informationen finden Sie unter [zum Caching](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Zwischenspeichern von Bildern

Die "eshoponcontainers" mobile app nutzt remote produktabbildungen, die aus der Zwischenspeicherung profitieren. Diese Images werden angezeigt, indem die [ `Image` ](xref:Xamarin.Forms.Image) -Steuerelement, und die `CachedImage` Kontrolle durch die [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) Bibliothek.

Die Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) -Steuerelement unterstützt das Zwischenspeichern der heruntergeladenen Bilder. Caching ist standardmäßig aktiviert, und speichert das Bild lokal für 24 Stunden. Darüber hinaus kann die Ablaufzeit konfiguriert werden, mit der [ `CacheValidity` ](xref:Xamarin.Forms.UriImageSource.CacheValidity) Eigenschaft. Weitere Informationen finden Sie unter [heruntergeladene Bild zwischenspeichern](~/xamarin-forms/user-interface/images.md#downloaded-image-caching).

Die FFImageLoading `CachedImage` Steuerelement ist ein Ersatz für die Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) -Steuerelement zusätzliche Eigenschaften, die zusätzliche Funktionalität zu aktivieren. Für diese Funktion bietet die Steuerung, konfigurierbare zwischenspeichern, während Fehler unterstützt, und laden die Image-Platzhalter. Im folgenden Codebeispiel wird veranschaulicht, wie die mobile app "eshoponcontainers" verwendet die `CachedImage` steuern, der `ProductTemplate`, die die Datenvorlage ein, die die [ `ListView` ](xref:Xamarin.Forms.ListView) steuern, der `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

Die `CachedImage` -Steuerelement legt die `LoadingPlaceholder` und `ErrorPlaceholder` Eigenschaften, die plattformspezifische Images. Die `LoadingPlaceholder` Eigenschaft gibt an, das Bild, während das angegebene Bild angezeigt werden die `Source` Eigenschaft wird abgerufen, und die `ErrorPlaceholder` Eigenschaft gibt an, das Bild angezeigt werden, wenn ein Fehler auftritt, wenn versucht wird, um das Bild abzurufen gemäß der `Source` Eigenschaft.

Wie der Name schon sagt, den `CachedImage` Steuerelement speichert remote-Images auf dem Gerät für die durch den Wert der angegebenen Zeit der `CacheDuration` Eigenschaft. Wenn der Wert dieser Eigenschaft nicht explizit festgelegt ist, wird der Standardwert von 30 Tagen angewendet.

## <a name="increasing-resilience"></a>Erhöhen der Resilienz

Alle apps, die mit Remotediensten und Ressourcen kommunizieren müssen gegenüber vorübergehenden Fehlern empfindlich sein. Vorübergehende Fehler umschließen den vorübergehenden Verlust der Netzwerkkonnektivität mit Diensten, die vorübergehende nichtverfügbarkeit eines Diensts oder Timeouts, die auftreten, wenn ein Dienst ausgelastet ist. Diese Fehler werden häufig automatisch behoben, und wenn die Aktion nach einer angemessenen Verzögerung wiederholt wird, die wahrscheinlich erfolgreich ausgeführt werden kann.

Vorübergehende Fehler haben eine große Auswirkung auf die wahrgenommene Qualität einer App muss, auch wenn es unter allen vorhersehbaren Umständen gründlich getestet wurde. Um sicherzustellen, dass eine app, die mit Remotediensten kommuniziert zuverlässig funktioniert, muss es möglich, alle der folgenden sein:

-   Erkennen Sie Fehler, wenn diese auftreten, und bestimmen, ob der Fehler wahrscheinlich vorübergehend sind.
-   Wiederholen Sie den Vorgang aus, wenn es feststellt, dass der Fehler wahrscheinlich vorübergehend und verfolgen Sie die Anzahl der Wiederholungsversuche für der Vorgang.
-   Verwenden Sie eine geeignete wiederholungsstrategie, die angibt, die Anzahl der Wiederholungsversuche ausführt, wird die Verzögerung zwischen jedem Versuch und die Aktionen nach einem fehlgeschlagenen Versuch durchzuführen.

Diese Behandlung vorübergehender Fehler kann durch Umschließen alle Zugriffsversuche auf einen Remotedienst in Code, der das Wiederholungsmuster implementiert erreicht werden.

### <a name="retry-pattern"></a>Muster "Wiederholung"

Wenn eine app einen Fehler erkennt, wenn er versucht, eine Anforderung an einen Remotedienst senden, können Fehler in einem der folgenden Arten behandelt werden:

-   Der Vorgang wiederholt wird. Die app konnte die fehlgeschlagene Anforderung sofort wiederholen.
-   Der Vorgang nach einer Verzögerung wiederholt wird. Die app sollte eine geeignete Menge an Zeit, bevor ein Wiederholungsversuch der Anforderung warten.
-   Den Vorgang wird abgebrochen. Die Anwendung sollte den Vorgang abbrechen und eine Ausnahme melden.

Die wiederholungsstrategie sollte optimiert werden, um die geschäftlichen Anforderungen der app entsprechen. Beispielsweise ist es wichtig, optimieren die Anzahl der Wiederholungsversuche und das Wiederholungsintervall aufzurufen, um den Vorgang. Wenn der Vorgang Teil einer Benutzerinteraktion ist, sollte das Wiederholungsintervall kurz und nur wenige Wiederholungsversuche zu vermeiden, dass der Benutzer auf eine Antwort warten. Wenn der Vorgang Teil eines lang andauernden Workflows, wobei ist abbrechen oder Neustarten des Workflows teuer oder zeitaufwändig, empfiehlt es zwischen den versuchen länger zu warten und weitere Male zu wiederholen.

> [!NOTE]
> Eine aggressive wiederholungsstrategie mit minimaler Verzögerung zwischen versuchen und eine große Anzahl von Wiederholungen kann einen Remotedienst beeinträchtigt werden, der nahe oder an Kapazität ausgeführt wird. Darüber hinaus beeinträchtigt diese eine wiederholungsstrategie die Reaktionsfähigkeit der app, wenn ständig versucht wird, einen fehlgeschlagenen Vorgang auszuführen.

Wenn eine Anforderung immer noch nach einer Anzahl von Wiederholungen fehlschlägt, ist es besser für die app aus, um weitere Anforderungen an die gleiche Ressource zu verhindern und einen Fehler melden. Nach einer festgelegten Dauer, kann die app stellen Sie dann eine oder mehrere Anforderungen an die Ressource, um festzustellen, ob diese erfolgreich ausgeführt werden. Weitere Informationen finden Sie unter [Trennschalter-Muster](#circuit_breaker_pattern).

> [!TIP]
> Implementieren Sie niemals einen endlosen Wiederholungsmechanismus. Verwenden Sie eine endliche Anzahl von Wiederholungen oder implementieren Sie die [Circuit-Breaker](/azure/architecture/patterns/circuit-breaker/) Muster, um einen Dienst für die Wiederherstellung zu ermöglichen.

Die mobile app von eShopOnContainers implementiert des Wiederholungsmusters, das bei RESTful-Web-Anforderungen nicht aktuell. Allerdings die `CachedImage` Kontrolle, durch die [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) Bibliothek unterstützt die Behandlung vorübergehender Fehler durch eine Wiederholung Bild laden. Wenn Sie laden ein Fehler auftritt, weitere Versuche erfolgen. Die Anzahl der Versuche wird angegeben, durch die `RetryCount` -Eigenschaft und Wiederholungen erfolgt nach einer Verzögerung, die gemäß der `RetryDelay` Eigenschaft. Wenn diese Eigenschaftswerte werden nicht explizit festgelegt, ihren standardmäßigen Werte gelten – 3 für die `RetryCount` -Eigenschaft und 250ms für die `RetryDelay` Eigenschaft. Weitere Informationen zu den `CachedImage` steuern, finden Sie unter [Zwischenspeichern von Bildern](#caching_images).

Die referenzanwendung "eshoponcontainers" wird das Wiederholungsmuster implementiert. Weitere Informationen, einschließlich einer Erläuterung der kombinieren des Wiederholungsmusters, das mit der `HttpClient` Klasse, finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

Weitere Informationen zu des Wiederholungsmusters, finden Sie unter den [wiederholen](/azure/architecture/patterns/retry/) Muster.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Trennschalter-Muster

In einigen Fällen können Fehler aufgrund von erwarteten Ereignisse auftreten, die länger dauern, bis zu beheben. Diese Fehler reichen von einem teilverlust der Konnektivität bis zum vollständigen Ausfall eines Diensts. In diesen Situationen ist es sinnlos ist, dass eine app einen Vorgang zu wiederholen, der wahrscheinlich nicht erfolgreich ausgeführt werden, und stattdessen sollte akzeptieren, dass der Vorgang fehlgeschlagen ist, und diesen Fehler entsprechend behandeln.

Trennschalter-Musters kann eine app wiederholt versucht einen Vorgang auszuführen, die wahrscheinlich nicht ausgeführt wird als auch die app aus, um festzustellen, ob der Fehler behoben wurde, verhindern.

> [!NOTE]
> Der Zweck des Trennschalter-Musters unterscheidet sich von der Wiederholung (Muster). Das Wiederholungsmuster kann es sich um eine app einen Vorgang in der Annahme zu wiederholen, die er erfolgreich ausgeführt wird. Das Trennschalter-Muster wird verhindert, dass eine app Ausführen eines Vorgangs, das vermutlich fehlschlägt.

Ein Trennschalter fungiert als Proxy für Vorgänge, die möglicherweise Fehler auftreten. Der Proxy sollte Überwachen der Anzahl der letzten Fehler, die aufgetreten sind, und anhand dieser Informationen entscheiden, können Sie den Vorgang, um den Vorgang fortzusetzen, oder eine Ausnahme sofort zurückgegeben.

Die eShopOnContainers-mobile-app implementiert derzeit nicht das Circuit-Breaker-Muster. Allerdings ist die "eshoponcontainers". Weitere Informationen finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

> [!TIP]
> Kombinieren Sie die Wiederholung und Circuit-Breaker-Muster. Eine app kann die Wiederholung und Circuit-Breaker-Muster kombinieren, mithilfe des Wiederholungsmusters, einen Vorgang über einen Trennschalter aufzurufen. Die Wiederholungslogik sollte jedoch alle Ausnahmen, die durch den Trennschalter zurückgegeben werden, und Wiederholungsversuche Abbrechen, wenn der Trennschalter anzeigt, dass es sich bei ein Fehler nicht vorübergehend ist.

Weitere Informationen über das Circuit-Breaker-Muster finden Sie unter den [Circuit-Breaker](/azure/architecture/patterns/circuit-breaker/) Muster.

## <a name="summary"></a>Zusammenfassung

Viele moderne webbasierte Lösungen stellen Verwendung von Webdiensten, Webservern gehostete Webdienste, um Funktionen für remote-Client-Anwendungen bereitzustellen. Die Vorgänge, die einen Webdienst verfügbar macht, bilden eine Web-API und Client-apps muss die Web-API nutzen, ohne zu wissen, wie die Daten oder Vorgänge, die die-API implementiert werden können.

Die Leistung einer App kann verbessert werden, durch das Zwischenspeichern von häufig verwendete Daten in einen schnellen Speicher, der Nähe befindet der app. Apps können die Read-through-caching mit dem Cache-Aside-Muster implementieren. Dieses Muster wird bestimmt, ob das Element derzeit im Cache vorhanden ist. Wenn das Element nicht im Cache befinden, aus dem Datenspeicher gelesene und dem Cache hinzugefügt.

Bei der Kommunikation mit Web-APIs müssen apps gegenüber vorübergehenden Fehlern empfindlich sein. Vorübergehende Fehler umschließen den vorübergehenden Verlust der Netzwerkkonnektivität mit Diensten, die vorübergehende nichtverfügbarkeit eines Diensts oder Timeouts, die auftreten, wenn ein Dienst ausgelastet ist. Diese Fehler werden häufig automatisch behoben, und falls die Aktion nach einer angemessenen Verzögerung wiederholt wird, dann ist es wahrscheinlich erfolgreich ausgeführt werden kann. Aus diesem Grund sollten apps zu umschließen, alle Zugriffsversuche auf eine Web-API im Code, der einen Mechanismus zur Behandlung vorübergehender implementiert.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
