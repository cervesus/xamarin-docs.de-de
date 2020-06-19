---
title: Zugreifen auf Remotedaten
description: In diesem Kapitel wird erläutert, wie der eshoponcontainers-Mobile App auf Daten aus den containerisierten microservices zugreift.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cef3c2369bb4aee81a52ddd27d6ad732d7544dfa
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573845"
---
# <a name="accessing-remote-data"></a>Zugreifen auf Remotedaten

Viele moderne webbasierte Lösungen nutzen Webdienste, die von Webservern gehostet werden, um Funktionen für Remote Client Anwendungen bereitzustellen. Die von einem Webdienst verfügbar gemachten Vorgänge bilden eine Web-API.

Client-apps sollten in der Lage sein, die Web-API zu verwenden, ohne zu wissen, wie die von der API verfügbar gemachten Daten oder Vorgänge implementiert werden. Dies erfordert, dass die API gängige Standards befolgt, mit denen eine Client-App und ein Webdienst die zu verwendenden Datenformate und die Struktur der Daten, die zwischen Client-apps und dem Webdienst ausgetauscht werden, zustimmen können.

## <a name="introduction-to-representational-state-transfer"></a>Einführung in Representational State Transfer

Der Representational State Transfer (Rest) ist ein Architekturstil zum entwickeln verteilter Systeme auf der Grundlage von Hypermedia. Ein primärer Vorteil des Rest-Modells besteht darin, dass es auf offenen Standards basiert und die Implementierung des Modells oder der Client-apps, die darauf zugreifen, nicht an eine bestimmte Implementierung bindet. Aus diesem Grund könnte ein Rest-Webdienst mit Microsoft ASP.net Core MVC implementiert werden, und Client-Apps können unter Verwendung beliebiger Sprachen und Toolsets entwickelt werden, die HTTP-Anforderungen generieren und HTTP-Antworten analysieren können.

Das Rest-Modell verwendet ein Navigations Schema zur Darstellung von Objekten und Diensten über ein Netzwerk (als Ressourcen bezeichnet). Systeme, die Rest implementieren, verwenden normalerweise das HTTP-Protokoll zum Übertragen von Anforderungen für den Zugriff auf diese Ressourcen In solchen Systemen übermittelt eine Client-App eine Anforderung in Form eines URI, der eine Ressource identifiziert, und eine HTTP-Methode (z. b. Get, Post, Put oder DELETE), die den Vorgang angibt, der für diese Ressource ausgeführt werden soll. Der Text der HTTP-Anforderung enthält alle Daten, die erforderlich sind, um den Vorgang auszuführen.

> [!NOTE]
> Rest definiert ein Zustands Loses Anforderungsmodell. Daher müssen HTTP-Anforderungen unabhängig sein und können in beliebiger Reihenfolge auftreten.

Die Antwort von einer Rest-Anforderung verwendet die standardmäßigen HTTP-Statuscodes. Beispielsweise sollte eine Anforderung, die gültige Daten zurückgibt, den HTTP-Antwortcode 200 (OK) enthalten, während eine Anforderung, die eine bestimmte Ressource nicht finden oder löschen kann, eine Antwort zurückgeben sollte, die den HTTP-Statuscode 404 (Nicht gefunden) enthält.

Eine Rest-Web-API macht eine Gruppe verbundener Ressourcen verfügbar und bietet die wichtigsten Vorgänge, mit denen eine APP diese Ressourcen bearbeiten und problemlos zwischen ihnen navigieren kann. Aus diesem Grund sind die URIs, die eine typische Rest-Web-API bilden, auf die Daten ausgerichtet, die Sie verfügbar macht, und die von http bereitgestellten Funktionen zum Verarbeiten dieser Daten verwenden.

Die Daten, die von einer Client-app in einer HTTP-Anforderung und den entsprechenden Antwort Nachrichten vom Webserver eingeschlossen werden, können in einer Vielzahl von Formaten dargestellt werden, die als Medientypen bezeichnet werden. Wenn eine Client-App eine Anforderung sendet, die Daten im Nachrichtentext zurückgibt, kann Sie die Medientypen angeben, die Sie im `Accept` Header der Anforderung verarbeiten kann. Wenn der Webserver diesen Medientyp unterstützt, kann er mit einer Antwort Antworten, die den-Header enthält, `Content-Type` der das Format der Daten im Text der Nachricht angibt. Es ist dann die Aufgabe der Client-App, die Antwortnachricht zu analysieren und die Ergebnisse im Nachrichtentext entsprechend zu interpretieren.

Weitere Informationen zu Rest finden Sie unter [API-Entwurf](/azure/architecture/best-practices/api-design/) und [API-Implementierung](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Nutzen von Rest-APIs

Der eshoponcontainers-Mobile App verwendet das Model-View-ViewModel (MVVM)-Muster, und die Modellelemente des Musters stellen die Domänen Entitäten dar, die in der APP verwendet werden. Die Controller-und Repository-Klassen in der eshoponcontainers-Verweis Anwendung akzeptieren und geben viele dieser Modell Objekte zurück. Aus diesem Grund werden Sie als Datenübertragungs Objekte (DTOs) verwendet, die alle Daten enthalten, die zwischen dem Mobile App und den containerisierten microservices übergeben werden. Der Hauptvorteil der Verwendung von DTOs zum Übergeben von Daten an und zum Empfangen von Daten von einem Webdienst besteht darin, dass die APP die Anzahl der Remote Aufrufe verringern kann, die durchgeführt werden müssen, indem Sie mehr Daten in einem einzelnen Remote Aufruf übermittelt.

### <a name="making-web-requests"></a>Ausführen von Webanforderungen

Der eshoponcontainers-Mobile App verwendet die- `HttpClient` Klasse, um Anforderungen über HTTP zu senden, wobei JSON als Medientyp verwendet wird. Diese Klasse stellt Funktionen zum asynchronen Senden von HTTP-Anforderungen und empfangen von HTTP-Antworten aus einer vom URI identifizierten Ressource bereit. Die- `HttpResponseMessage` Klasse stellt eine von einer Rest-API empfangene http-Antwortnachricht dar, nachdem eine HTTP-Anforderung durchgeführt wurde. Sie enthält Informationen über die Antwort, einschließlich Statuscode, Header und beliebiger Text. Die `HttpContent` -Klasse stellt den HTTP-Text und Inhalts Header dar, z `Content-Type` `Content-Encoding` . b. und. Der Inhalt kann mit einer beliebigen `ReadAs` Methode wie und gelesen werden `ReadAsStringAsync` `ReadAsByteArrayAsync` , abhängig vom Format der Daten.

#### <a name="making-a-get-request"></a>Erstellen einer GET-Anforderung

Die- `CatalogService` Klasse wird verwendet, um den Datenabruf Vorgang aus dem Catalog-Mikro Dienst zu verwalten. In der- `RegisterDependencies` Methode in der- `ViewModelLocator` Klasse `CatalogService` wird die-Klasse als Typzuordnung für den- `ICatalogService` Typ mit dem Container für die Abhängigkeitsinjektion von autofac registriert. Wenn dann eine Instanz der- `CatalogViewModel` Klasse erstellt wird, akzeptiert der Konstruktor einen- `ICatalogService` Typ, den autofac auflöst, und gibt eine Instanz der- `CatalogService` Klasse zurück. Weitere Informationen zur Abhängigkeitsinjektion finden [Sie unter Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection).

In Abbildung 10-1 wird die Interaktion von Klassen veranschaulicht, die Katalogdaten aus dem Katalog-mikrodienst zum Anzeigen von lesen `CatalogView` .

[![](accessing-remote-data-images/catalogdata.png "Retrieving data from the catalog microservice")](accessing-remote-data-images/catalogdata-large.png#lightbox "Retrieving data from the catalog microservice")

**Abbildung 10-1**: Abrufen von Daten aus dem Katalog-mikroservice

Wenn `CatalogView` auf die navigiert wird, wird die- `OnInitialize` Methode in der- `CatalogViewModel` Klasse aufgerufen. Diese Methode ruft Katalogdaten aus dem Katalog-mikrodienst ab, wie im folgenden Codebeispiel gezeigt:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Diese Methode ruft die- `GetCatalogAsync` Methode der- `CatalogService` Instanz auf, die `CatalogViewModel` von autofac in den eingefügt wurde. Die `GetCatalogAsync`-Methode wird in folgendem Codebeispiel veranschaulicht:

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

Diese Methode erstellt den URI, der die Ressource identifiziert, an die die Anforderung gesendet wird, und verwendet die- `RequestProvider` Klasse, um die Get HTTP-Methode für die Ressource aufzurufen, bevor die Ergebnisse an zurückgegeben werden `CatalogViewModel` . Die- `RequestProvider` Klasse enthält Funktionen, die eine Anforderung in Form eines URI übermittelt, der eine Ressource identifiziert, eine HTTP-Methode, die den für diese Ressource auszuführenden Vorgang angibt, und einen Text, der alle Daten enthält, die zum Ausführen des Vorgangs erforderlich sind. Informationen dazu, wie die- `RequestProvider` Klasse in eingefügt wird `CatalogService class` , finden [Sie unter Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection).

Das folgende Codebeispiel zeigt die- `GetAsync` Methode in der- `RequestProvider` Klasse:

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

Diese Methode ruft die- `CreateHttpClient` Methode auf, die eine Instanz der-Klasse zurückgibt, für `HttpClient` die die entsprechenden Header festgelegt sind. Anschließend sendet er eine asynchrone Get-Anforderung an die Ressource, die durch den URI identifiziert wird, wobei die Antwort in der-Instanz gespeichert wird `HttpResponseMessage` . `HandleResponse`Anschließend wird die-Methode aufgerufen, die eine Ausnahme auslöst, wenn die Antwort keinen HTTP-Erfolgsstatus Code enthält. Anschließend wird die Antwort als Zeichenfolge gelesen, von JSON in ein `CatalogRoot` -Objekt konvertiert und an das-Objekt zurückgegeben `CatalogService` .

Die- `CreateHttpClient` Methode wird im folgenden Codebeispiel gezeigt:

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

Diese Methode erstellt eine neue Instanz der `HttpClient` -Klasse und legt den- `Accept` Header von Anforderungen, die von der- `HttpClient` Instanz gestellt werden, auf fest. Dies bedeutet, `application/json` dass erwartet wird, dass der Inhalt einer beliebigen Antwort mithilfe von JSON formatiert wird. Wenn ein Zugriffs Token als Argument an die-Methode weitergegeben wurde `CreateHttpClient` , wird es dann dem- `Authorization` Header aller von der-Instanz vorgenommenen Anforderungen hinzugefügt, `HttpClient` die der-Zeichenfolge vorangestellt sind `Bearer` . Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Wenn die- `GetAsync` Methode in der- `RequestProvider` Klasse aufruft `HttpClient.GetAsync` , wird die- `Items` Methode in der- `CatalogController` Klasse im Catalog. API-Projekt aufgerufen, was im folgenden Codebeispiel gezeigt wird:

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

Diese Methode ruft die Katalogdaten mithilfe von EntityFramework aus der SQL-Datenbank ab und gibt Sie als Antwortnachricht zurück, die den HTTP-Statuscode Success und eine Auflistung von JSON-formatierten `CatalogItem` Instanzen enthält.

#### <a name="making-a-post-request"></a>Senden einer POST-Anforderung

Die `BasketService` -Klasse wird verwendet, um den Datenabruf und den Update Prozess mit dem Warenkorb-microservice zu verwalten. In der- `RegisterDependencies` Methode in der- `ViewModelLocator` Klasse `BasketService` wird die-Klasse als Typzuordnung für den- `IBasketService` Typ mit dem Container für die Abhängigkeitsinjektion von autofac registriert. Wenn dann eine Instanz der- `BasketViewModel` Klasse erstellt wird, akzeptiert der Konstruktor einen- `IBasketService` Typ, den autofac auflöst, und gibt eine Instanz der- `BasketService` Klasse zurück. Weitere Informationen zur Abhängigkeitsinjektion finden [Sie unter Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection).

In Abbildung 10-2 wird die Interaktion von Klassen gezeigt, die die von der angezeigten Korb Daten `BasketView` an den Warenkorb-microservice senden.

[![](accessing-remote-data-images/basketdata.png "Sending data to the basket microservice")](accessing-remote-data-images/basketdata-large.png#lightbox "Sending data to the basket microservice")

**Abbildung 10-2**: Senden von Daten an den Warenkorb-microservice

Wenn ein Element dem Warenkorb hinzugefügt wird, wird die- `ReCalculateTotalAsync` Methode in der- `BasketViewModel` Klasse aufgerufen. Diese Methode aktualisiert den Gesamtwert der Elemente im Warenkorb und sendet die Warenkorb-Daten an den Warenkorb-microservice, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode ruft die- `UpdateBasketAsync` Methode der- `BasketService` Instanz auf, die `BasketViewModel` von autofac in den eingefügt wurde. Die folgende Methode zeigt die- `UpdateBasketAsync` Methode:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Diese Methode erstellt den URI, der die Ressource identifiziert, an die die Anforderung gesendet wird, und verwendet die- `RequestProvider` Klasse, um die Post http-Methode für die Ressource aufzurufen, bevor die Ergebnisse an zurückgegeben werden `BasketViewModel` . Beachten Sie, dass ein Zugriffs Token, das von identityserver während des Authentifizierungsprozesses abgerufen wird, erforderlich ist, um Anforderungen an den Warenkorb-microservice zu autorisieren. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Das folgende Codebeispiel zeigt eine der `PostAsync` Methoden in der- `RequestProvider` Klasse:

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

Diese Methode ruft die- `CreateHttpClient` Methode auf, die eine Instanz der-Klasse zurückgibt, für `HttpClient` die die entsprechenden Header festgelegt sind. Anschließend sendet er eine asynchrone Post-Anforderung an die Ressource, die durch den URI identifiziert wird, wobei die serialisierten Warenkorb-Daten im JSON-Format gesendet werden und die Antwort in der-Instanz gespeichert wird `HttpResponseMessage` . `HandleResponse`Anschließend wird die-Methode aufgerufen, die eine Ausnahme auslöst, wenn die Antwort keinen HTTP-Erfolgsstatus Code enthält. Anschließend wird die Antwort als Zeichenfolge gelesen, von JSON in ein `CustomerBasket` -Objekt konvertiert und an das-Objekt zurückgegeben `BasketService` . Weitere Informationen zur- `CreateHttpClient` Methode finden Sie unter [Erstellen einer GET-Anforderung](#making-a-get-request).

Wenn die- `PostAsync` Methode in der- `RequestProvider` Klasse aufruft `HttpClient.PostAsync` , wird die- `Post` Methode in der- `BasketController` Klasse im Basket. API-Projekt aufgerufen, was im folgenden Codebeispiel gezeigt wird:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Diese Methode verwendet eine Instanz der- `RedisBasketRepository` Klasse, um die Warenkorb-Daten im redis Cache beizubehalten, und gibt Sie als Antwortnachricht zurück, die den HTTP-Statuscode Success und eine JSON-formatierte `CustomerBasket` Instanz enthält.

#### <a name="making-a-delete-request"></a>Erstellen einer Löschanforderung

Abbildung 10-3 zeigt die Interaktionen von Klassen, die waren Korb Daten aus dem Warenkorb-microservice für das Löschen `CheckoutView` .

![](accessing-remote-data-images/checkoutdata.png "Deleteing data from the basket microservice")

**Abbildung 10-3**: Löschen von Daten aus dem Warenkorb-microservice

Wenn der Checkout-Prozess aufgerufen wird, `CheckoutAsync` wird die-Methode in der- `CheckoutViewModel` Klasse aufgerufen. Diese Methode erstellt vor dem Löschen des Warenkorbs eine neue Bestellung, wie im folgenden Codebeispiel gezeigt:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Diese Methode ruft die- `ClearBasketAsync` Methode der- `BasketService` Instanz auf, die `CheckoutViewModel` von autofac in den eingefügt wurde. Die folgende Methode zeigt die- `ClearBasketAsync` Methode:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Diese Methode erstellt den URI, der die Ressource identifiziert, an die die Anforderung gesendet wird, und verwendet die- `RequestProvider` Klasse, um die Delete HTTP-Methode für die Ressource aufzurufen. Beachten Sie, dass ein Zugriffs Token, das von identityserver während des Authentifizierungsprozesses abgerufen wird, erforderlich ist, um Anforderungen an den Warenkorb-microservice zu autorisieren. Weitere Informationen zur Autorisierung finden Sie unter [Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Das folgende Codebeispiel zeigt die- `DeleteAsync` Methode in der- `RequestProvider` Klasse:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Diese Methode ruft die- `CreateHttpClient` Methode auf, die eine Instanz der-Klasse zurückgibt, für `HttpClient` die die entsprechenden Header festgelegt sind. Anschließend sendet er eine asynchrone DELETE-Anforderung an die Ressource, die durch den URI identifiziert wird. Weitere Informationen zur- `CreateHttpClient` Methode finden Sie unter [Erstellen einer GET-Anforderung](#making-a-get-request).

Wenn die- `DeleteAsync` Methode in der- `RequestProvider` Klasse aufruft `HttpClient.DeleteAsync` , wird die- `Delete` Methode in der- `BasketController` Klasse im Basket. API-Projekt aufgerufen, was im folgenden Codebeispiel gezeigt wird:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Diese Methode verwendet eine Instanz der- `RedisBasketRepository` Klasse, um die Warenkorb-Daten aus dem redis Cache zu löschen.

## <a name="caching-data"></a>Zwischenspeichern von Daten

Die Leistung einer APP kann durch das Zwischenspeichern von Daten, auf die häufig zugegriffen wird, in der Nähe der app in fast Storage verbessert werden. Wenn sich der schnelle Speicher näher an der APP befindet als die ursprüngliche Quelle, kann das Caching die Reaktionszeiten beim Abrufen von Daten erheblich verbessern.

Die häufigste Form der Zwischenspeicherung ist das Zwischenspeichern von Lesevorgängen, bei dem eine APP Daten durch Verweise auf den Cache abruft. Wenn die Daten nicht im Cache enthalten sind, werden sie aus dem Datenspeicher abgerufen und dem Cache hinzugefügt. Apps können das Zwischenspeichern mit Zwischenspeicherung mit dem Cache--Muster implementieren. Dieses Muster bestimmt, ob sich das Element derzeit im Cache befindet. Wenn sich das Element nicht im Cache befindet, wird es aus dem Datenspeicher gelesen und dem Cache hinzugefügt. Weitere Informationen finden Sie unter [Cache-off-](/azure/architecture/patterns/cache-aside/) Muster.

> [!TIP]
> Zwischenspeichern von Daten, die häufig gelesen werden und die sich nur selten ändern. Diese Daten können nach Bedarf dem Cache hinzugefügt werden, wenn Sie zum ersten Mal von einer APP abgerufen werden. Dies bedeutet, dass die APP die Daten nur einmal aus dem Datenspeicher abrufen muss und dass der nachfolgende Zugriff über den Cache erfüllt werden kann.

Verteilte Anwendungen, wie z. b. die eshoponcontainers-Referenz Anwendung, sollten einen oder beide der folgenden Caches bereitstellen:

- Ein frei gegebener Cache, auf den von mehreren Prozessen oder Computern aus zugegriffen werden kann.
- Ein privater Cache, in dem Daten lokal auf dem Gerät gespeichert werden, auf dem die app ausgeführt wird.

Der Mobile App eshoponcontainers verwendet einen privaten Cache, bei dem Daten lokal auf dem Gerät gespeichert werden, auf dem eine Instanz der app ausgeführt wird. Informationen zu dem von der eshoponcontainers-Referenz Anwendung verwendeten Cache finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

> [!TIP]
> Stellen Sie sich den Cache als vorübergehenden Datenspeicher vor, der jederzeit verschwinden könnte. Stellen Sie sicher, dass die Daten im ursprünglichen Datenspeicher und im Cache beibehalten werden. Die Wahrscheinlichkeit, dass Daten verloren gehen, wird dann minimiert, wenn der Cache nicht mehr verfügbar ist.

### <a name="managing-data-expiration"></a>Verwalten des Daten Ablaufs

Es ist nicht zu erwarten, dass zwischengespeicherte Daten immer mit den ursprünglichen Daten konsistent sind. Die Daten im ursprünglichen Datenspeicher können sich ändern, nachdem Sie zwischengespeichert wurden, wodurch die zwischengespeicherten Daten veraltet werden. Daher sollten apps eine Strategie implementieren, mit der sichergestellt werden kann, dass die Daten im Cache so aktuell wie möglich sind. Sie können jedoch auch Situationen erkennen und behandeln, die auftreten, wenn die Daten im Cache veraltet sind. Bei den meisten zwischen Speicherungs Mechanismen kann der Cache so konfiguriert werden, dass Daten ablaufen, sodass der Zeitraum, in dem die Daten möglicherweise veraltet sind, verringert wird.

> [!TIP]
> Legen Sie beim Konfigurieren eines Caches eine Standard Ablaufzeit fest. Viele Caches implementieren den Ablauf, wodurch Daten ungültig gemacht und aus dem Cache entfernt werden, wenn für einen bestimmten Zeitraum nicht darauf zugegriffen wird. Sie müssen jedoch sorgfältig vorgehen, wenn Sie den Ablauf Zeitraum auswählen. Wenn Sie zu kurz gemacht wird, laufen die Daten zu schnell ab, und die Vorteile der Zwischenspeicherung werden verringert. Wenn der Vorgang zu lange dauert, ist das Risiko von Daten gefährdet. Daher sollte die Ablaufzeit dem Zugriffsmuster für apps entsprechen, die die Daten verwenden.

Wenn zwischengespeicherte Daten ablaufen, sollten Sie aus dem Cache entfernt werden, und die APP muss die Daten aus dem ursprünglichen Datenspeicher abrufen und wieder im Cache platzieren.

Es ist auch möglich, dass ein Cache aufgefüllt wird, wenn Daten für einen zu langen Zeitraum beibehalten werden. Daher sind Anforderungen zum Hinzufügen neuer Elemente zum Cache möglicherweise erforderlich, um einige Elemente in einem Prozess zu entfernen, der als Entfernungs Vorgang bezeichnet *wird.* Cache Dienste entfernen in der Regel Daten auf der Grundlage der zuletzt verwendeten Daten. Es gibt jedoch noch weitere Entfernungs Richtlinien, einschließlich der zuletzt verwendeten, der zuletzt verwendeten und der First-in-First-out. Weitere Informationen finden Sie unter [Leitfaden](/azure/architecture/best-practices/caching/)zum Zwischenspeichern.

### <a name="caching-images"></a>Zwischenspeichern von Bildern

Die eshoponcontainers-Mobile App verbraucht Remote Produkt Images, die von der Zwischenspeicherung profitieren. Diese Bilder werden vom [`Image`](xref:Xamarin.Forms.Image) -Steuerelement und vom-Steuerelement angezeigt, das `CachedImage` von der [ffimageloading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) -Bibliothek bereitgestellt wird.

Das Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) Steuerelement unterstützt das Zwischenspeichern von heruntergeladenen Bildern Caching ist standardmäßig aktiviert und speichert das Image für 24 Stunden lokal. Außerdem kann die Ablaufzeit mit der-Eigenschaft konfiguriert werden [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) . Weitere Informationen finden Sie unter [Herunterladen von Bild Caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching).

Das Steuerelement von ffimageloading `CachedImage` ist ein Ersatz für das- Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) Steuerelement, das zusätzliche Eigenschaften bereitstellt, die zusätzliche Funktionalität ermöglichen. Unter dieser Funktionalität bietet das Steuerelement konfigurierbare Zwischenspeicherung und unterstützt gleichzeitig Fehler und lade Platzhalter. Im folgenden Codebeispiel wird veranschaulicht, wie der eshoponcontainers-Mobile App das- `CachedImage` Steuerelement in verwendet `ProductTemplate` , bei dem es sich um die Daten Vorlage handelt, die vom-Steuerelement im verwendet wird [`ListView`](xref:Xamarin.Forms.ListView) `CatalogView`

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

Das `CachedImage` -Steuerelement legt die `LoadingPlaceholder` `ErrorPlaceholder` Eigenschaften und auf plattformspezifische Bilder fest. Die `LoadingPlaceholder` -Eigenschaft gibt das Bild an, das angezeigt werden soll, wenn das von der-Eigenschaft angegebene Bild `Source` abgerufen wird, und die- `ErrorPlaceholder` Eigenschaft gibt das Bild an, das angezeigt werden soll, wenn beim Versuch, das von der-Eigenschaft angegebene Bild abzurufen, ein Fehler auftritt `Source`

Wie der Name schon sagt, `CachedImage` speichert das-Steuerelement Remote Abbilder auf dem Gerät für die Zeit, die durch den Wert der-Eigenschaft angegeben wird `CacheDuration` . Wenn dieser Eigenschafts Wert nicht explizit festgelegt wird, wird der Standardwert von 30 Tagen angewendet.

## <a name="increasing-resilience"></a>Erhöhen der Resilienz

Alle apps, die mit Remote Diensten und Ressourcen kommunizieren, müssen bei vorübergehenden Fehlern empfindlich sein. Vorübergehende Fehler umfassen den vorübergehenden Verlust der Netzwerk Konnektivität mit Diensten, die vorübergehende Nichtverfügbarkeit eines Diensts oder Timeouts, die auftreten, wenn ein Dienst ausgelastet ist. Diese Fehler werden häufig selbst korrigiert, und wenn die Aktion nach einer angemessenen Verzögerung wiederholt wird, ist es wahrscheinlich, dass Sie erfolgreich ausgeführt wird.

Vorübergehende Fehler können enorme Auswirkungen auf die wahrgenommene Qualität einer APP haben, auch wenn Sie unter allen vorhersehbaren Umständen gründlich getestet wurde. Um sicherzustellen, dass eine APP, die mit Remote Diensten kommuniziert, zuverlässig funktioniert, muss Sie in der Lage sein, Folgendes durchzuführen:

- Erkennen Sie Fehler, wenn Sie auftreten, und stellen Sie fest, ob die Fehler wahrscheinlich vorübergehend sind.
- Wiederholen Sie den Vorgang, wenn er feststellt, dass der Fehler wahrscheinlich vorübergehend ist, und verfolgen Sie, wie oft der Vorgang erneut versucht wurde.
- Verwenden Sie eine geeignete Wiederholungs Strategie, die die Anzahl der Wiederholungen, die Verzögerung zwischen den einzelnen versuchen und die nach einem fehlgeschlagenen Versuch auszuführenden Aktionen angibt.

Diese Behandlung vorübergehender Fehler kann erreicht werden, indem alle Versuche, auf einen Remote Dienst zuzugreifen, in Code, der das Wiederholungsmuster implementiert, eingebunden werden.

### <a name="retry-pattern"></a>Wiederholungsmuster

Wenn eine APP einen Fehler erkennt, wenn versucht wird, eine Anforderung an einen Remote Dienst zu senden, kann Sie den Fehler auf eine der folgenden Arten behandeln:

- Der Vorgang wird wiederholt. Die APP kann die fehlgeschlagene Anforderung sofort wiederholen.
- Wiederholen Sie den Vorgang nach einer Verzögerung. Die APP sollte nach einem angemessenen Zeitraum warten, bis die Anforderung erneut versucht wird.
- Der Vorgang wird abgebrochen. Die Anwendung sollte den Vorgang abbrechen und eine Ausnahme melden.

Die Wiederholungs Strategie sollte so angepasst werden, dass Sie den geschäftlichen Anforderungen der APP entspricht. Beispielsweise ist es wichtig, die Anzahl der Wiederholungs Versuche und das Wiederholungsintervall für den Vorgang zu optimieren, der versucht wird. Wenn der Vorgang Teil einer Benutzerinteraktion ist, sollte das Wiederholungsintervall kurz sein, und es sollten nur wenige Wiederholungs Versuche unternommen werden, um zu vermeiden, dass Benutzer auf eine Antwort warten. Wenn der Vorgang Teil eines Workflows mit langer Laufzeit ist, bei dem das Abbrechen oder Neustarten des Workflows aufwendig oder zeitaufwändig ist, ist es angebracht, länger zwischen den versuchen zu warten und mehrmals zu versuchen.

> [!NOTE]
> Eine aggressive Wiederholungs Strategie mit minimaler Verzögerung zwischen versuchen und einer großen Anzahl von Wiederholungen könnte einen Remote Dienst beeinträchtigen, der in der Nähe der Kapazität oder der Kapazität ausgeführt wird. Außerdem könnte eine solche Wiederholungs Strategie auch die Reaktionsfähigkeit der APP beeinflussen, wenn Sie ständig versucht, einen fehlgeschlagenen Vorgang auszuführen.

Wenn eine Anforderung nach einer Reihe von Wiederholungen weiterhin fehlschlägt, ist es für die APP besser, weitere Anforderungen an dieselbe Ressource zu verhindern und einen Fehler zu melden. Anschließend kann die APP nach einem bestimmten Zeitraum eine oder mehrere Anforderungen an die Ressource senden, um festzustellen, ob Sie erfolgreich sind. Weitere Informationen finden Sie unter [Circuit Breaker Pattern](#circuit-breaker-pattern) (Schutzschaltermuster).

> [!TIP]
> Implementieren Sie niemals einen endlosen Wiederholungsmechanismus. Verwenden Sie eine begrenzte Anzahl von Wiederholungen, oder implementieren Sie das Trenn [Schalter](/azure/architecture/patterns/circuit-breaker/) -Muster, damit ein Dienst wieder hergestellt werden kann.

Die eshoponcontainers-Mobile App implementiert das Wiederholungsmuster derzeit nicht, wenn Sie Rest-Webanforderungen vornehmen. Das- `CachedImage` Steuerelement, das von der [ffimageloading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) -Bibliothek bereitgestellt wird, unterstützt jedoch die Behandlung vorübergehender Fehler, indem das Laden von Bildern Wenn beim Laden des Bilds ein Fehler auftritt, werden weitere Versuche unternommen. Die Anzahl der Versuche wird durch die `RetryCount` -Eigenschaft angegeben, und Wiederholungen erfolgen nach einer Verzögerung, die durch die-Eigenschaft angegeben wird `RetryDelay` . Wenn diese Eigenschaftswerte nicht explizit festgelegt sind, werden die Standardwerte für die `RetryCount` -Eigenschaft auf – 3 und für die-Eigenschaft auf 250 MS angewendet `RetryDelay` . Weitere Informationen zum- `CachedImage` Steuerelement finden Sie unter Zwischenspeichern von [Bildern](#caching-images).

Die eshoponcontainers-Verweis Anwendung implementiert das Wiederholungsmuster. Weitere Informationen, einschließlich einer Erläuterung zum Kombinieren des Wiederholungs Musters mit der- `HttpClient` Klasse, finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

Weitere Informationen zum Wiederholungsmuster finden Sie unter dem [Wiederholungs](/azure/architecture/patterns/retry/) Muster.

### <a name="circuit-breaker-pattern"></a>"Circuit-Breaker"-Muster

In einigen Situationen können Fehler aufgrund von erwarteten Ereignissen auftreten, deren Behebung länger dauert. Diese Fehler können von einem partiellen Verlust der Konnektivität bis hin zum vollständigen Ausfall eines Diensts reichen. In diesen Fällen ist es nicht sinnlos, dass eine APP einen Vorgang wiederholt, der wahrscheinlich nicht erfolgreich ist. Stattdessen sollte angenommen werden, dass der Vorgang fehlgeschlagen ist, und diesen Fehler entsprechend behandeln.

Das Trennschalter-Muster kann verhindern, dass eine APP wiederholt versucht, einen Vorgang auszuführen, bei dem der Fehler wahrscheinlich fehlschlägt, während die APP gleichzeitig erkennt, ob der Fehler behoben wurde.

> [!NOTE]
> Der Zweck des Trennschalter Musters unterscheidet sich von dem Wiederholungsmuster. Das Wiederholungsmuster ermöglicht einer APP, in der Annahme, dass Sie erfolgreich ausgeführt wird, einen Vorgang zu wiederholen. Das Trennschalter-Muster verhindert, dass eine APP einen Vorgang ausführt, der vermutlich fehlschlägt.

Ein Trennschalter fungiert als Proxy für Vorgänge, bei denen möglicherweise Fehler auftreten. Der Proxy sollte die Anzahl der zuletzt aufgetretenen Fehler überwachen und anhand dieser Informationen entscheiden, ob der Vorgang fortgesetzt werden darf, oder eine Ausnahme sofort zurückgeben.

Der Mobile App "eshoponcontainers" implementiert derzeit nicht das Trennschalter-Muster. Eshoponcontainers hingegen. Weitere Informationen finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

> [!TIP]
> Kombinieren Sie die Muster für Wiederholung und Trennschalter. Eine APP kann die Wiederholungs-und Trennschalter Muster mithilfe des Wiederholungs Musters kombinieren, um einen Vorgang über einen Trennschalter aufzurufen. Die Wiederholungslogik sollte jedoch auf Ausnahmen reagieren, die vom Trennschalter zurückgegeben werden, und Wiederholungsversuche abbrechen, wenn der Trennschalter anzeigt, dass ein Fehler nicht vorübergehend ist.

Weitere Informationen zum Trennschalter-Muster finden Sie unter dem Trenn [Schalter](/azure/architecture/patterns/circuit-breaker/) -Muster.

## <a name="summary"></a>Zusammenfassung

Viele moderne webbasierte Lösungen nutzen Webdienste, die von Webservern gehostet werden, um Funktionen für Remote Client Anwendungen bereitzustellen. Die von einem Webdienst verfügbar gemachten Vorgänge bilden eine Web-API, und Client-apps sollten in der Lage sein, die Web-API zu verwenden, ohne zu wissen, wie die von der API verfügbar gemachten Daten oder Vorgänge implementiert werden.

Die Leistung einer APP kann durch das Zwischenspeichern von Daten, auf die häufig zugegriffen wird, in der Nähe der app in fast Storage verbessert werden. Apps können das Zwischenspeichern mit Zwischenspeicherung mit dem Cache--Muster implementieren. Dieses Muster bestimmt, ob sich das Element derzeit im Cache befindet. Wenn sich das Element nicht im Cache befindet, wird es aus dem Datenspeicher gelesen und dem Cache hinzugefügt.

Bei der Kommunikation mit Web-APIs müssen Apps bei vorübergehenden Fehlern empfindlich sein. Vorübergehende Fehler umfassen den vorübergehenden Verlust der Netzwerk Konnektivität mit Diensten, die vorübergehende Nichtverfügbarkeit eines Diensts oder Timeouts, die auftreten, wenn ein Dienst ausgelastet ist. Diese Fehler werden häufig selbst korrigiert, und wenn die Aktion nach einer angemessenen Verzögerung wiederholt wird, ist es wahrscheinlich, dass Sie erfolgreich ausgeführt wird. Daher sollten apps alle Versuche, auf eine Web-API zuzugreifen, in Code einschließen, der einen Mechanismus zur Behandlung vorübergehender Fehler implementiert.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
