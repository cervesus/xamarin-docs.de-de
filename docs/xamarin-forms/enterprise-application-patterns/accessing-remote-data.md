---
title: Zugreifen auf Remotedaten
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: f1e15d31f11c6845ad61882996f01fb16e80ed95
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="accessing-remote-data"></a>Zugreifen auf Remotedaten

Viele moderne webbasierte Lösungen stellen Gebrauch von Webdiensten, gehostet von Webservern, um Funktionen für remote-Client-Anwendungen bereitzustellen. Die Vorgänge, die einen Webdienst verfügbar macht, bilden eine Web-API.

Client-apps muss die Web-API nutzen, ohne zu wissen, wie die Daten oder Vorgänge, die die API verfügbar macht implementiert werden. Dies erfordert, dass die API durch common-Standards hält sich in, mit denen ein Clientdienst App- und zustimmen, auf welche Datenformate zu verwenden sind und die Struktur der Daten, die zwischen Clientanwendungen und dem Webdienst ausgetauscht werden können.

## <a name="introduction-to-representational-state-transfer"></a>Einführung in die Representational State Transfer

Representational State Transfer (REST) ist eine Architektur für das Erstellen verteilter Systeme, die basierend auf Hypermedia. Ein wichtiger Vorteil von REST-Modells ist, dass er auf offenen Standards basierend wurde und nicht die Implementierung für das Modell oder die Client-apps, die sie auf eine bestimmte Implementierung Zugriff zu binden. Aus diesem Grund ein REST-Webdienst mithilfe von Microsoft ASP.NET Core MVC implementiert werden kann, und Client-apps können mithilfe jeder Sprache und das Toolset, das Generieren von HTTP-Anforderungen und Analysieren der HTTP-Antworten entwickeln.

Die REST-Modell verwendet ein Navigation Schema zur Darstellung von Objekten und-Diensten über ein Netzwerk, die als Ressourcen bezeichnet. Systeme, die in der Regel REST implementieren verwenden das HTTP-Protokoll zum Übertragen von Anforderungen auf diese Ressourcen zuzugreifen. In solchen Systemen übermittelt eine Client-app eine Anforderung in Form von einem URI, der eine Ressource identifiziert, und eine HTTP-Methode (z. B. GET, POST, PUT oder DELETE), die den Vorgang, der für diese Ressource ausgeführt werden. Der Text der HTTP-Anforderung enthält alle Daten, die zum Ausführen des Vorgangs erforderlich.

> [!NOTE]
> REST definiert eine zustandslose Request-Modell. Aus diesem Grund werden die HTTP-Anforderungen unabhängig sein und können in beliebiger Reihenfolge auftreten.

Die Antwort von einer anderen Anforderung macht mithilfe der standardmäßigen HTTP-Statuscodes. Beispielsweise sollte eine Anforderung, die gültige Daten gibt den HTTP-Antwortcode 200 (OK), enthalten, während eine Anforderung, die nicht gefunden, oder Löschen einer angegebenen Ressource eine Antwort zurückgeben soll, die den HTTP-Statuscode 404 (Nichtgefunden) enthält.

Eine RESTful-Web-API macht eine Reihe von verbundenen Ressourcen, und stellt die Core-Vorgänge, mit die eine app, bearbeiten diese Ressourcen und problemlos zwischen ihnen navigieren können. Aus diesem Grund sind die URIs, die eine typische RESTful-Web-API bilden serverworkflow die Daten, die sie offenlegt und bereitgestellten Funktionen für die Verwendung von HTTP, das diese Daten verarbeitet werden.

Die Daten enthalten, indem eine Client-app in einer HTTP-Anforderung und die entsprechenden Antwortnachrichten aus dem Webserver konnte in einer Vielzahl von Formaten, die als Medientypen angezeigt. Wenn eine Clientanwendung eine Anforderung, die Daten im Text einer Nachricht zurückgibt sendet, können sie angeben, die Medientypen, kann er im verarbeiten, der `Accept` -Header der Anforderung. Wenn der Webserver dieser Medientyp unterstützt, können Sie mit einer Antwort, die enthält Antworten der `Content-Type` Header, der das Format der Daten im Text der Nachricht angibt. Es ist dann die Zuständigkeit für die Client-app, um die Antwortnachricht zu analysieren und die Ergebnisse im Nachrichtentext richtig interpretieren.

Weitere Informationen zu REST finden Sie unter [API-Entwurf](/azure/architecture/best-practices/api-design/) und [-API-Implementierung](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Nutzen die Rest-APIs

Die mobile eShopOnContainers-app verwendet die Model-View-ViewModel (MVVM)-Muster und die betroffenen Modellelemente, der die Muster darstellen, die die Domänenentitäten in der app verwendet. Der Controller und des Repositorys Klassen in der Anwendung der eShopOnContainers Verweis akzeptieren und viele dieser Modellobjekte zurückzugeben. Aus diesem Grund werden sie als datenübertragungsobjekte (DTOs) verwendet, die alle Daten enthalten, die zwischen der mobilen Anwendung und die Datenvolumes Microservices übergeben wird. Der wichtigste Vorteil der Verwendung von DTOs zum Übergeben von Daten an und Empfangen von Daten von einem Webdienst wird von mehr Daten übertragen werden in einem einzigen remote-Aufruf, die app die Anzahl von Remoteaufrufen vermeiden, die vorgenommen werden müssen.

### <a name="making-web-requests"></a>Ausführen von Webanforderungen

Der eShopOnContainers mobile app verwendet die `HttpClient` Klasse, um Anfragen über HTTP mit JSON als Medientyp verwendet wird. Diese Klasse bietet Funktionen für das HTTP-Anforderungen asynchron senden und Empfangen von HTTP-Antworten aus einem URI identifizierte Ressource. Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht, die über eine REST-API empfangen wird, nachdem eine HTTP-Anforderung gestellt wurde. Es enthält Informationen über die Antwort, einschließlich der Statuscode, Header und eine beliebige Stelle. Die `HttpContent` Klasse stellt die HTTP-Texts und der bereitgestellten Inhaltsheader wie z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann einer beliebigen, von gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`, je nachdem, auf das Format der Daten.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Eine GET-Anforderung ausgeben

Die `CatalogService` Klasse dient zum Abrufen von Daten aus dem Katalog Microservice zu verwalten. In der `RegisterDependencies` Methode in der `ViewModelLocator` -Klasse, die `CatalogService` Klasse wird als eine Zuordnung für registriert die `ICatalogService` Typ mit dem Autofac abhängigkeitseinschleusungscontainer. Klicken Sie dann, wenn eine Instanz von der `CatalogViewModel` Klasse wird erstellt, dessen Konstruktor akzeptiert ein `ICatalogService` eingeben, die Autofac aufgelöst wird, Zurückgeben von einer Instanz von der `CatalogService` Klasse. Weitere Informationen zu Abhängigkeitsinjektion, finden Sie unter [Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Abbildung 10 – 1 zeigt die Interaktion von Klassen, die Katalogdaten aus dem Katalog Microservice für die Anzeige von Lesen der `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Abrufen von Daten aus dem Katalog Microservice")](accessing-remote-data-images/catalogdata-large.png#lightbox "Abrufen von Daten aus dem Katalog Microservice")

**Abbildung 10 – 1**: Abrufen von Daten aus dem Katalog Microservice

Wenn die `CatalogView` navigiert wird, die `OnInitialize` Methode in der `CatalogViewModel` -Klasse aufgerufen wird. Diese Methode ruft Katalogdaten aus dem Katalog Microservice ab, wie im folgenden Codebeispiel gezeigt:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Diese Methode ruft die `GetCatalogAsync` Methode der `CatalogService` -Instanz, die in eingeschleust wurde die `CatalogViewModel` von Autofac. Das folgende Codebeispiel zeigt die `GetCatalogAsync` Methode:

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

Diese Methode erstellt den URI, der die Ressource identifiziert, die Anforderung gesendet werden wird, und verwendet die `RequestProvider` Klasse, um die GET HTTP-Methode für die Ressource vor der Rückgabe der Ergebnisse in Aufrufen der `CatalogViewModel`. Die `RequestProvider` Klasse enthält Funktionen, die eine Anforderung in Form eines URIs sendet, der eine Ressource, die eine HTTP-Methode identifiziert, die den Vorgang, der für diese Ressource ausgeführt werden und ein Text, der keine Daten enthält, die zum Ausführen des Vorgangs erforderlich sind. Informationen darüber, wie die `RequestProvider` Klasse eingefügt, in der `CatalogService class`, finden Sie unter [Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Das folgende Codebeispiel zeigt die `GetAsync` Methode in der `RequestProvider` Klasse:

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Diese Methode ruft die `CreateHttpClient` -Methode, die eine Instanz zurückgegeben der `HttpClient` -Klasse mit dem entsprechenden Header. Er sendet dann eine asynchrone GET-Anforderung an die Ressource identifiziert durch den URI mit der Antwort in gespeichert werden die `HttpResponseMessage` Instanz. Die `HandleResponse` Methode wird dann aufgerufen, die löst eine Ausnahme aus, wenn die Antwort auf eine erfolgreiche HTTP-Statuscode enthalten nicht. Und dann die Antwort als Zeichenfolge, Konvertierung von JSON zu lesen ist eine `CatalogRoot` -Objekt und zurückgegeben werden, um die `CatalogService`.

Die `CreateHttpClient` Methode wird im folgenden Codebeispiel gezeigt:

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

Diese Methode erstellt eine neue Instanz der dem `HttpClient` -Klasse und legt die `Accept` Header, der alle Anforderungen von der `HttpClient` -Instanz, auf `application/json`, gibt an, dass er davon ausgeht, den Inhalt der Antworten auf die mit JSON formatiert werden. Klicken Sie dann, wenn ein Zugriffstoken übergeben wurde, als Argument an die `CreateHttpClient` -Methode, wird Sie hinzugefügt der `Authorization` Header, der alle Anforderungen, die von der `HttpClient` Instanz, die die Zeichenfolge mit dem Präfix `Bearer`. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Wenn die `GetAsync` Methode in der `RequestProvider` -Klasse ruft `HttpClient.GetAsync`, die `Items` Methode in der `CatalogController` Klasse im Projekt Catalog.API wird aufgerufen, die im folgenden Codebeispiel dargestellt ist:

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

Diese Methode ruft die Katalogdaten aus der SQL-Datenbank mithilfe von EntityFramework ab und wird als eine Antwortnachricht, die eine erfolgreiche HTTP-Statuscode zurückgegeben, und eine Auflistung von JSON formatiert `CatalogItem` Instanzen.

#### <a name="making-a-post-request"></a>Eine POST-Anforderung

Die `BasketService` Klasse wird verwendet, um den Datenabruf zu verwalten und den Aktualisierungsvorgang mit dem Warenkorb Microservice. In der `RegisterDependencies` Methode in der `ViewModelLocator` -Klasse, die `BasketService` Klasse wird als eine Zuordnung für registriert die `IBasketService` Typ mit dem Autofac abhängigkeitseinschleusungscontainer. Klicken Sie dann, wenn eine Instanz von der `BasketViewModel` Klasse wird erstellt, dessen Konstruktor akzeptiert ein `IBasketService` eingeben, die Autofac aufgelöst wird, Zurückgeben von einer Instanz von der `BasketService `Klasse. Weitere Informationen zu Abhängigkeitsinjektion, finden Sie unter [Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Abbildung 10 – 2 zeigt die Interaktion von Klassen, die die Warenkorb angezeigten Daten durch Senden der `BasketView`, um die Warenkorb-Microservice.

[![](accessing-remote-data-images/basketdata.png "Senden von Daten an die Warenkorb-Microservice")](accessing-remote-data-images/basketdata-large.png#lightbox "Senden von Daten an die Warenkorb-Microservice")

**Abbildung 10 – 2**: Senden von Daten an die Warenkorb-Microservice

Wenn ein Element, in den Warenkorb hinzugefügt wird der `ReCalculateTotalAsync` Methode in der `BasketViewModel` -Klasse aufgerufen wird. Diese Methode aktualisiert den Gesamtwert der Elemente im Warenkorb und sendet die Warenkorb-Daten an die Warenkorb-Microservice, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

Diese Methode ruft die `UpdateBasketAsync` Methode der `BasketService` -Instanz, die in eingeschleust wurde die `BasketViewModel` von Autofac. Die folgende Methode zeigt den `UpdateBasketAsync` Methode:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Diese Methode erstellt den URI, der die Ressource identifiziert, die Anforderung gesendet werden wird, und verwendet die `RequestProvider` Klasse, um die HTTP POST-Methode für die Ressource vor der Rückgabe der Ergebnisse in Aufrufen der `BasketViewModel`. Ein Zugriffstoken abgerufenes IdentityServer während des Authentifizierungsprozesses ist erforderlich, um Anforderungen an die Warenkorb-Microservice zu autorisieren. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Das folgende Codebeispiel zeigt einen von der `PostAsync` Methoden in der `RequestProvider` Klasse:

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Diese Methode ruft die `CreateHttpClient` -Methode, die eine Instanz zurückgegeben der `HttpClient` -Klasse mit dem entsprechenden Header. Eine asynchrone POST-Anforderung auf die Ressource identifiziert durch den URI mit den serialisierten Warenkorb-Daten im JSON-Format und die Antwort, die in gespeicherten gesendet werden übermittelt die `HttpResponseMessage` Instanz. Die `HandleResponse` Methode wird dann aufgerufen, die löst eine Ausnahme aus, wenn die Antwort auf eine erfolgreiche HTTP-Statuscode enthalten nicht. Klicken Sie dann die Antwort als Zeichenfolge, Konvertierung von JSON zu lesen ist eine `CustomerBasket` -Objekt und zurückgegeben werden, um die `BasketService`. Weitere Informationen zu den `CreateHttpClient` -Methode finden Sie unter [machen eine GET-Anforderung](#making_a_get_request).

Wenn die `PostAsync` Methode in der `RequestProvider` -Klasse ruft `HttpClient.PostAsync`, die `Post` Methode in der `BasketController` Klasse im Projekt Basket.API wird aufgerufen, die im folgenden Codebeispiel dargestellt ist:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Diese Methode verwendet eine Instanz der `RedisBasketRepository` Klasse, um die Warenkorb-Daten in den Redis-Cache beizubehalten und das Arrayobjekt zurückgibt, wie eine Antwortnachricht, die einen Erfolg Statuscode "HTTP" und eine JSON umfasst formatiert `CustomerBasket` Instanz.

#### <a name="making-a-delete-request"></a>Ausführen einer DELETE-Anforderung

Abbildung 10 – 3 zeigt die Interaktionen von Klassen, die Warenkorb-Daten aus dem Warenkorb Microservice für Löschen der `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Löschen des Daten aus dem Warenkorb microservice")

**Abbildung 10 – 3**: Löschen von Daten aus dem Warenkorb Microservice

Beim Aufrufen des Auscheckvorgangs der `CheckoutAsync` Methode in der `CheckoutViewModel` -Klasse aufgerufen wird. Diese Methode erstellt eine neue Bestellung vor dem Löschen der Warenkorb wie im folgenden Codebeispiel gezeigt:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Diese Methode ruft die `ClearBasketAsync` Methode der `BasketService` -Instanz, die in eingeschleust wurde die `CheckoutViewModel` von Autofac. Die folgende Methode zeigt den `ClearBasketAsync` Methode:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Diese Methode erstellt den URI, der die Ressource identifiziert, die die Anforderung gesendet werden, und verwendet die `RequestProvider` Klasse, die DELETE HTTP-Methode für die Ressource aufzurufen. Ein Zugriffstoken abgerufenes IdentityServer während des Authentifizierungsprozesses ist erforderlich, um Anforderungen an die Warenkorb-Microservice zu autorisieren. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Das folgende Codebeispiel zeigt die `DeleteAsync` Methode in der `RequestProvider` Klasse:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Diese Methode ruft die `CreateHttpClient` -Methode, die eine Instanz zurückgegeben der `HttpClient` -Klasse mit dem entsprechenden Header. Danach wird eine asynchrone DELETE-Anforderung an die vom URI angegebene Ressource übermittelt. Weitere Informationen zu den `CreateHttpClient` -Methode finden Sie unter [machen eine GET-Anforderung](#making_a_get_request).

Wenn die `DeleteAsync` Methode in der `RequestProvider` -Klasse ruft `HttpClient.DeleteAsync`, die `Delete` Methode in der `BasketController` Klasse im Projekt Basket.API wird aufgerufen, die im folgenden Codebeispiel dargestellt ist:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Diese Methode verwendet eine Instanz der `RedisBasketRepository` Klasse, um die Warenkorb-Daten aus dem Redis-Cache zu löschen.

## <a name="caching-data"></a>Zwischenspeichern von Daten

Die Leistung einer App kann verbessert werden, durch das Zwischenspeichern von häufig verwendete Daten schnell Speicher, der zeichenfolgenspeicherdateien befindet die app. Wenn der schnelle Speicher befindet sich näher an die app als die ursprüngliche Quelle, caching kann die Antwort erheblich verbessern Zeitüberschreitung beim Abrufen von Daten.

Die am häufigsten verwendete Form des Zwischenspeicherns ist Through zwischenspeichern, in denen eine Anwendung Daten abruft, durch Verweisen auf den Cache. Wenn die Daten im Cache enthalten ist, hat er aus dem Datenspeicher abgerufen und dem Cache hinzugefügt. Apps können Through zwischenspeichern, wenn der Cache-Aside-Muster implementieren. Dieses Muster wird bestimmt, ob das Element zurzeit im Cache befindet. Wenn das Element im Cache nicht ist, aus dem Datenspeicher gelesene und dem Cache hinzugefügt. Weitere Informationen finden Sie unter der [Cache-Aside](/azure/architecture/patterns/cache-aside/) Muster.

> [!TIP]
> Zwischenspeichern von Daten, die häufig gelesen wird und sich nur selten ändert. Diese Daten können hinzugefügt werden, in den Cache bei Bedarf das erste Mal, dass sie von einer app abgerufen wird. Dies bedeutet, dass die app benötigt, um die Daten nur einmal aus dem Datenspeicher abzurufen, den Zugriff mit dem Cache erfüllt werden kann.

Verteilte Anwendungen, sollten z. B. die eShopOnContainers Anwendung verweisen eine oder beide der folgenden Caches bereitstellen:

-   Einem geteilten Datencache, die durch mehrere Prozesse oder Computer zugegriffen werden kann.
-   Eine private Cache, in dem Daten lokal auf dem Gerät, das Ausführen der app aufrechterhalten wird.

Die eShopOnContainers-mobile-app verwendet einen privaten Cache, in dem Daten lokal auf dem Gerät befinden, die eine Instanz der app ausgeführt wird. Informationen zum Cache wird von der Anwendung der eShopOnContainers Referenz finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

> [!TIP]
> Vorstellen des Caches als vorübergehender Datenspeicher, der zu einem beliebigen Zeitpunkt verschwinden konnte. Stellen Sie sicher, dass die Daten in den ursprünglichen Datenspeicher als auch für den Cache verwaltet werden. Die Wahrscheinlichkeit, dass Daten verloren gehen werden dann minimiert, wenn der Cache nicht mehr verfügbar ist.

### <a name="managing-data-expiration"></a>Verwalten von Data-Ablauf

Es ist zu erwarten, dass zwischengespeicherte Daten immer mit den ursprünglichen Daten konsistent werden alleine nicht durchführbar. Daten in den ursprünglichen Datenspeicher möglicherweise ändern, nachdem er wird die zwischengespeicherten Daten zwischengespeichert wurde veralten verursacht. Aus diesem Grund sollten apps eine Strategie umzusetzen, die wird sichergestellt, dass die Daten im Cache so aktuell wie möglich, jedoch können auch erkennen und behandeln Situationen, die auftreten, wenn die Daten im Cache veralten, hat. Die meisten Cachemechanismus Aktivieren des Cache konfiguriert werden, um die Daten ablaufen, und verringern Sie daher den Zeitraum für den Daten veraltet sein könnten.

> [!TIP]
> Legen Sie eine Standardablaufzeit Zeit bei der Konfiguration eines Caches. Viele Caches implementieren Ablauf, die Daten ungültig und aus dem Cache entfernt, wenn er für einen angegebenen Zeitraum nicht zugegriffen werden kann. Allerdings muss darauf geachtet werden bei der Auswahl des Ablaufdatums. Wenn sie nicht zu kurz festgelegt ist, Daten zu schnell läuft, und die Vorteile des Zwischenspeicherns werden reduziert. Wenn sie zu lang ist, die Daten Risiken veraltet sind nicht festgelegt ist. Daher sollte die Ablaufzeit das Muster für den Zugriff auf apps übereinstimmen, die die Daten verwenden.

Wenn zwischengespeicherte Daten abgelaufen ist, es aus dem Cache entfernt werden sollte, und die app abrufen muss die Daten aus den ursprünglichen Daten speichern, und platzieren Sie es wieder in den Cache.

Es ist auch möglich, dass ein Cache Auffüllen kann, wenn die Daten für einen zu langen Zeitraum bleiben zugelassen werden. Aus diesem Grund Anforderungen zum Hinzufügen neuer Elemente in den Cache möglicherweise erforderlich, um einige Elemente in der genannten entfernen *Entfernung*. In der Regel entfernen Sie Cachingdienste Least-recently-used Masterdaten. Es gibt jedoch andere entfernungsrichtlinien, einschließlich der zuletzt verwendete und First-in-First-Out. Weitere Informationen finden Sie unter [Caching Guidance](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Zwischenspeichern von Bildern

Die mobile app eShopOnContainers nutzt remote Produktbilder, die zwischengespeichert werden, profitieren. Diese Bilder angezeigt werden, indem die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) -Steuerelement, und die `CachedImage` mithilfe der [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) Bibliothek.

Der Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) -Steuerelement unterstützt das Zwischenspeichern von heruntergeladenen Bildern. Caching ist standardmäßig aktiviert, und speichern Sie das Abbild wird für 24 Stunden lokal. Darüber hinaus kann die Ablaufzeit konfiguriert werden, mit der [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) Eigenschaft. Weitere Informationen finden Sie unter [heruntergeladen Image Caching](~/xamarin-forms/user-interface/images.md#Image_Caching).

Der FFImageLoading `CachedImage` Steuerelement ist ein Ersatz für die Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Steuerelement zusätzliche Eigenschaften, die zusätzliche Funktionalität zu aktivieren. Unter anderem diese Funktionalität bietet das Steuerelement konfigurierbare zwischenspeichern, während Fehler unterstützen, und Laden von Bild-Platzhalter. Im folgenden Codebeispiel wird veranschaulicht, wie die eShopOnContainers mobile app verwendet die `CachedImage` steuern, der `ProductTemplate`, welche ist die vom verwendete Datenvorlage die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) steuern, der `CatalogView`:

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

Die `CachedImage` -Steuerelement legt die `LoadingPlaceholder` und `ErrorPlaceholder` Eigenschaften plattformspezifischen Bilder. Die `LoadingPlaceholder` Eigenschaft gibt an, das Bild, während das angegebene Bild angezeigt werden die `Source` Eigenschaft wird abgerufen, und die `ErrorPlaceholder` Eigenschaft gibt das Bild, das angezeigt werden, wenn ein Fehler auftritt, wenn versucht wird, zum Abrufen des Images an. gemäß der `Source` Eigenschaft.

Wie der Name schon sagt, den `CachedImage` Steuerelement zwischenspeichert remote Bilder auf dem Gerät für die durch den Wert der angegebenen Zeit die `CacheDuration` Eigenschaft. Wenn der Wert dieser Eigenschaft nicht explizit festgelegt ist, wird der Standardwert von 30 Tagen angewendet.

## <a name="increasing-resilience"></a>Erhöhen der Flexibilität

Alle apps, die mit remote-Dienste und-Ressourcen kommunizieren müssen empfindlich gegenüber vorübergehenden Fehlern werden. Vorübergehende Fehler zählen der vorübergehende Verlust der Netzwerkverbindungen an Dienste, die vorübergehende nichtverfügbarkeit eines Diensts oder Timeouts, die auftreten, wenn ein Dienst ausgelastet ist. Diese Fehler werden häufig automatisch behoben, und wenn die Aktion, nach einer geeigneten Verzögerung wiederholt wird wird wahrscheinlich erfolgreich ist.

Vorübergehende Fehler können einen großen Einfluss auf die wahrgenommene Qualität einer App haben, auch wenn es unter allen vorhersehbaren Umständen gründlich getestet wurde. Um sicherzustellen, dass eine app, die Kommunikation mit Remotedienste zuverlässig ausgeführt wird, müssen sie alle der folgenden Aufgaben ausführen können:

-   Erkennen Sie Fehler, wenn sie auftreten, und bestimmen Sie, ob der Fehler nur vorübergehend aufzutreten wahrscheinlich sind.
-   Wiederholen Sie den Vorgang aus, wenn es feststellt, dass der Fehler wahrscheinlich vorübergehend sein und behalten Sie den Überblick über der Anzahl der Häufigkeit, mit die der Vorgang wiederholt wurde.
-   Verwenden Sie eine geeignete wiederholungsstrategie, die die Anzahl der Wiederholungen, die Verzögerung zwischen jeder Versuch, sowie die Aktionen werden nach einem fehlerhaftem Versuch angibt.

Diese Behandlung vorübergehender Fehler kann durch wrapping alle Zugriffsversuche auf einen Remotedienst in Code, der die Wiederholung (Muster) implementiert erreicht werden.

### <a name="retry-pattern"></a>Wiederholen Sie die Muster

Wenn eine app einen Fehler festgestellt wird, wenn er versucht, eine Anforderung an einen Remotedienst zu senden, können Fehler in einem der folgenden Arten behandelt werden:

-   Wiederholen den Vorgang. Die app konnte die fehlerhafte Anforderung sofort erneut ausführen.
-   Wiederholen den Vorgang nach einer Verzögerung. Die app, die für einen geeigneten Zeitspanne vor der Wiederholung der Anfrage warten soll.
-   Den Vorgang wird abgebrochen. Die Anwendung sollte den Vorgang abzubrechen und eine Ausnahme gemeldet.

Die wiederholungsstrategie sollte entsprechend die geschäftsanforderungen der app optimiert werden. Beispielsweise ist es wichtig, optimieren die Anzahl der Wiederholungsversuche und das Wiederholungsintervall, um den Vorgang. Wenn der Vorgang ein Eingreifen des Benutzers gehört, muss das Wiederholungsintervall kurzen und nur wenige Wiederholungen wurden versucht zu vermeiden, dass Benutzer, die auf eine Antwort wartet. Wenn der Vorgang Teil eines lang ausgeführte Workflows, wobei ist abbrechen oder erneuten Starten des Workflows teuer oder zeitaufwändig, ist es sinnvoll, mehr zwischen den einzelnen versuchen warten und mehrere Male wiederholen.

> [!NOTE]
> Eine aggressive wiederholungsstrategie mit minimaler Verzögerung zwischen versuchen und eine große Anzahl von Wiederholungen konnte einen Remotedienst beeinträchtigt werden, der zeichenfolgenspeicherdateien erreicht bzw. fast Kapazität ausgeführt wird. Darüber hinaus kann solche eine wiederholungsversuchstrategie auch auswirken die Reaktionsfähigkeit der app, wenn er fortlaufend versucht, einen fehlgeschlagenen Vorgang auszuführen.

Wenn eine Anforderung immer noch nach einer Anzahl von Wiederholungen fehlschlägt, ist es besser für die app aus, um zu verhindern, dass weitere Anforderungen, die auf die gleiche Ressource und einen Fehler zu melden. Nach einem festgelegten Zeitraum kann die Anwendung stellen Sie dann eine oder mehrere Anforderungen auf die Ressource, um festzustellen, ob sie erfolgreich sind. Weitere Informationen finden Sie unter [einen Trennschalter](#circuit_breaker_pattern).

> [!TIP]
> Implementieren Sie niemals eine endlose Wiederholungsmechanismus. Verwenden Sie eine endliche Anzahl von Wiederholungen, oder implementieren Sie die [Trennschalter](/azure/architecture/patterns/circuit-breaker/) Muster, um eine Wiederherstellung möglich.

Die mobilen Anwendung für eShopOnContainers implementiert des Wiederholungsmusters, das beim Bereitstellen von RESTful-webanforderungen nicht aktuell. Allerdings die `CachedImage` -Steuerelement, die [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) -Bibliothek unterstützt die Behandlung vorübergehender Fehler durch eine Wiederholung Bild laden. Laden von Images ein Fehler auftritt, erfolgt die weiter versucht werden. Die Anzahl der Versuche gemäß der `RetryCount` Eigenschaft und Wiederholungsversuche erfolgt nach einer Verzögerung, die gemäß der `RetryDelay` Eigenschaft. Wenn diese Eigenschaftswerte werden nicht explizit festgelegt, deren Standard Werte gelten – 3 für die `RetryCount` Eigenschaft und 250ms für die `RetryDelay` Eigenschaft. Weitere Informationen zu den `CachedImage` steuern, finden Sie unter [Zwischenspeichern Bilder](#caching_images).

Die eShopOnContainers Verweis Anwendung wird die Wiederholung (Muster) implementiert. Weitere Informationen einschließlich einer Erläuterung zum Kombinieren des Wiederholungsmusters, das mit der `HttpClient` Klasse, finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

Weitere Informationen zu den Wiederholung (Muster), finden Sie unter der [wiederholen](/azure/architecture/patterns/retry/) Muster.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Einen Trennschalter

In einigen Situationen können Fehler aufgrund von erwarteten Ereignisse auftreten, die diesen Fehler beheben länger dauern. Diese Fehler können und vollständiger Fehler eines Diensts aus einem teilweise Verlust der Verbindung liegen. In diesen Situationen ist es nicht sinnvoll, damit eine app einen Vorgang zu wiederholen, der wahrscheinlich nicht erfolgreich ausgeführt werden, und stattdessen sollte annehmen, dass der Vorgang fehlgeschlagen ist und dieser Fehler entsprechend behandelt wird.

Einen Trennschalter kann verhindern, dass eine app immer wieder versuchen, einen Vorgang auszuführen, der wahrscheinlich ein Fehler auftritt, als auch die app zu ermitteln, ob der Fehler behoben wurde, ist.

> [!NOTE]
> Der Zweck der einen Trennschalter unterscheidet sich von der Wiederholung (Muster). Die Wiederholung (Muster) kann eine app Wiederholung eines Vorgangs in der Annahme berechnet, die es erfolgreich ausgeführt werden muss. Einen Trennschalter verhindert, dass eine app Ausführen eines Vorgangs, das vermutlich fehlschlägt.

Ein Trennschalter fungiert als Proxy für Vorgänge, die für das fehlschlagen. Der Proxy muss überwachen die Anzahl der letzte Fehler, die aufgetreten sind und mithilfe dieser Informationen entscheiden, ob der Vorgang, um den Vorgang fortzusetzen, oder eine Ausnahme sofort zurückgegeben zugelassen.

Die mobilen Anwendung für eShopOnContainers implementiert einen Trennschalter nicht aktuell. Wird jedoch die eShopOnContainers. Weitere Informationen finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

> [!TIP]
> Kombinieren Sie die Wiederholung und Trennschalter-Muster. Eine app kann die Wiederholung und Trennschalter Muster mithilfe des Wiederholungsmusters, das zum Aufrufen eines Vorgangs über ein Trennschalter kombinieren. Allerdings sollte die Wiederholungslogik für alle Ausnahmen, die von der Trennschalter zurückgegeben werden und Wiederholungsversuche verwerfen, wenn der Trennschalter gibt an, dass ein Problem nicht vorübergehend ist.

Weitere Informationen über einen Trennschalter finden Sie unter der [Trennschalter](/azure/architecture/patterns/circuit-breaker/) Muster.

## <a name="summary"></a>Zusammenfassung

Viele moderne webbasierte Lösungen stellen Gebrauch von Webdiensten, gehostet von Webservern, um Funktionen für remote-Client-Anwendungen bereitzustellen. Die Vorgänge, die einen Webdienst verfügbar macht, bilden eine Web-API und Clientanwendungen sollten in der Lage, die Web-API nutzen, ohne zu wissen, wie die Daten oder Vorgänge, die die API verfügbar macht implementiert werden.

Die Leistung einer App kann verbessert werden, durch das Zwischenspeichern von häufig verwendete Daten schnell Speicher, der zeichenfolgenspeicherdateien befindet die app. Apps können Through zwischenspeichern, wenn der Cache-Aside-Muster implementieren. Dieses Muster wird bestimmt, ob das Element zurzeit im Cache befindet. Wenn das Element im Cache nicht ist, aus dem Datenspeicher gelesene und dem Cache hinzugefügt.

Bei der Kommunikation mit Web-APIs muss apps empfindlich gegenüber vorübergehenden Fehlern. Vorübergehende Fehler zählen der vorübergehende Verlust der Netzwerkverbindungen an Dienste, die vorübergehende nichtverfügbarkeit eines Diensts oder Timeouts, die auftreten, wenn ein Dienst ausgelastet ist. Diese Fehler werden häufig automatisch behoben, und falls die Aktion nach einer geeigneten Verzögerung wiederholt wird, dann ist es wahrscheinlich erfolgreich ausgeführt werden kann. Aus diesem Grund sollten apps zu umschließen, alle Zugriffsversuche auf eine Web-API im Code, der einen vorübergehenden Fehler Mechanismus zur Behandlung von implementiert.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
