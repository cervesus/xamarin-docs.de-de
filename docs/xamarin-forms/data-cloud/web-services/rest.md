---
title: ''
description: Das Integrieren eines Webdiensts in eine Anwendung ist ein gängiges Szenario. In diesem Artikel wird veranschaulicht, wie Sie einen Rest-Webdienst aus einer- Xamarin.Forms Anwendung nutzen.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ecfcede22e96a4a91f5367dae49b0d837ca2416f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139164"
---
# <a name="consume-a-restful-web-service"></a>Nutzen eines Rest-Webdiensts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_Das Integrieren eines Webdiensts in eine Anwendung ist ein gängiges Szenario. In diesem Artikel wird veranschaulicht, wie Sie einen Rest-Webdienst aus einer- Xamarin.Forms Anwendung nutzen._

Der Representational State Transfer (Rest) ist ein Architekturstil zum Entwickeln von Webdiensten. Rest-Anforderungen werden über HTTP mit denselben HTTP-Verben hergestellt, die Webbrowser zum Abrufen von Webseiten und zum Senden von Daten an Server verwenden. Die Verben sind:

- **Get** – dieser Vorgang wird verwendet, um Daten aus dem Webdienst abzurufen.
- **Post** – dieser Vorgang wird verwendet, um ein neues Datenelement für den Webdienst zu erstellen.
- **Put** – dieser Vorgang wird verwendet, um ein Datenelement für den Webdienst zu aktualisieren.
- **Patch** – dieser Vorgang wird verwendet, um ein Element der Daten im Webdienst zu aktualisieren, indem eine Reihe von Anweisungen zum Ändern des Elements beschrieben werden. Dieses Verb wird in der Beispielanwendung nicht verwendet.
- **Delete** – dieser Vorgang wird verwendet, um ein Datenelement für den Webdienst zu löschen.

Webdienst-APIs, die Rest einhalten, werden als Rest-APIs bezeichnet und mithilfe von definiert:

- Ein Basis-URI.
- HTTP-Methoden wie Get, Post, Put, Patch oder DELETE.
- Ein Medientyp für die Daten, z. b. JavaScript Object Notation (JSON).

Rest-Webdienste verwenden normalerweise JSON-Nachrichten, um Daten an den Client zurückzugeben. JSON ist ein textbasiertes Datenaustauschformat, das kompakte Nutzlasten erzeugt, was zu geringeren Bandbreitenanforderungen beim Senden von Daten führt. Die Beispielanwendung verwendet die Open Source- [Bibliothek "newtonsoft JSON.net](https://www.newtonsoft.com/json) ", um Nachrichten zu serialisieren und zu deserialisieren.

Die Einfachheit von Rest hat dazu beigetragen, dass Sie die primäre Methode für den Zugriff auf Webdienste in mobilen Anwendungen ist.

Wenn die Beispielanwendung ausgeführt wird, wird eine Verbindung mit einem lokal gehosteten Rest-Dienst hergestellt, wie im folgenden Screenshot zu sehen:

![](rest-images/portal.png "Sample Application")

> [!NOTE]
> In ios 9 und höher erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird. Da ATS in apps, die für IOS 9 erstellt wurden, standardmäßig aktiviert ist, unterliegen alle Verbindungen den Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.
>
>Es kann deaktiviert werden, wenn es nicht möglich ist, das **https** -Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Verwenden des Webdiensts

Der Rest-Dienst wird mit ASP.net Core geschrieben und bietet die folgenden Vorgänge:

|Vorgang|HTTP-Methode|Relativer URI|Parameter|
|--- |--- |--- |--- |
|Abrufen einer Liste von To-Do-Elementen|GET|/api/todoitems/|
|Neues to-do-Element erstellen|POST|/api/todoitems/|Ein JSON-formatiertes "$ doitem"|
|Aktualisieren eines To-Do-Elements|PUT|/api/todoitems/|Ein JSON-formatiertes "$ doitem"|
|Löschen eines To-Do-Elements|Delete|/api/todoitems/{id}|

Die meisten URIs enthalten die `TodoItem` ID im Pfad. Um z. b. das zu löschen `TodoItem` , dessen ID ist `6bb8a868-dba1-4f1a-93b7-24ebce87e243` , sendet der Client eine DELETE-Anforderung an `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243` . Weitere Informationen zum Datenmodell, das in der Beispielanwendung verwendet wird, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

Wenn das Web-API-Framework eine Anforderung empfängt, leitet es die Anforderung an eine Aktion weiter. Diese Aktionen sind einfach öffentliche Methoden in der- `TodoItemsController` Klasse. Das Framework verwendet eine Routing Tabelle, um zu bestimmen, welche Aktion als Reaktion auf eine Anforderung aufgerufen werden soll. Dies wird im folgenden Codebeispiel gezeigt:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Die Routing Tabelle enthält eine Routen Vorlage, und wenn das Web-API-Framework eine HTTP-Anforderung empfängt, versucht es, den URI mit der Routen Vorlage in der Routing Tabelle abzugleichen. Wenn keine übereinstimmende Route gefunden wird, empfängt der Client den Fehler 404 (nicht gefunden). Wenn eine übereinstimmende Route gefunden wird, wählt die Web-API den Controller und die Aktion wie folgt aus:

- Um den Controller zu finden, fügt die Web-API "controller" dem Wert der *{Controller}* -Variablen hinzu.
- Um die Aktion zu finden, untersucht die Web-API die HTTP-Methode und überprüft Controller Aktionen, die mit derselben HTTP-Methode wie ein Attribut ergänzt werden.
- Die Platzhalter Variable *{ID}* ist einem Aktionsparameter zugeordnet.

Der Rest-Dienst verwendet die Standard Authentifizierung. Weitere Informationen finden Sie unter [Authentifizieren eines Rest-Webdiensts](~/xamarin-forms/data-cloud/authentication/rest.md). Weitere Informationen zum ASP.net-Web-API Routing finden Sie unter [Routing in ASP.net-Web-API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) auf der ASP.NET-Website. Weitere Informationen zum Erstellen des Rest-Diensts mit ASP.net Core finden Sie unter Erstellen von Back-End- [Diensten für Native Mobile Anwendungen](/aspnet/core/mobile/native-mobile-backend/).

Die `HttpClient` -Klasse wird verwendet, um Anforderungen über HTTP zu senden und zu empfangen. Es stellt Funktionen zum Senden von HTTP-Anforderungen und empfangen von HTTP-Antworten aus einer vom URI identifizierten Ressource bereit. Jede Anforderung wird als asynchroner Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie [unter async-Unterstützung: Übersicht](~/cross-platform/platform/async.md).

Die- `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht dar, die vom Webdienst empfangen wurde, nachdem eine HTTP-Anforderung durchgeführt wurde. Sie enthält Informationen über die Antwort, einschließlich Statuscode, Header und beliebiger Text. Die `HttpContent` -Klasse stellt den HTTP-Text und Inhalts Header dar, z `Content-Type` `Content-Encoding` . b. und. Der Inhalt kann mit einer beliebigen Methode gelesen werden `ReadAs` , z. b `ReadAsStringAsync` `ReadAsByteArrayAsync` . und, je nach Format der Daten.

### <a name="creating-the-httpclient-object"></a>Erstellen des httpclient-Objekts

Die `HttpClient` -Instanz wird auf Klassenebene deklariert, damit das-Objekt so lange wie die Anwendung HTTP-Anforderungen ausführen muss, wie im folgenden Codebeispiel gezeigt:

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    _client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>Abrufen von Daten

Die `HttpClient.GetAsync` -Methode wird verwendet, um die Get-Anforderung an den Webdienst zu senden, der durch den URI angegeben wird. Anschließend wird die Antwort vom Webdienst empfangen, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  var response = await _client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Der Rest-Dienst sendet einen HTTP-Statuscode in der- `HttpResponseMessage.IsSuccessStatusCode` Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Für diesen Vorgang sendet der Rest-Dienst den HTTP-Statuscode 200 (OK) in der Antwort, der angibt, dass die Anforderung erfolgreich war und die angeforderten Informationen in der Antwort angezeigt werden.

Wenn der http-Vorgang erfolgreich war, wird der Inhalt der Antwort für die Anzeige gelesen. Die `HttpResponseMessage.Content` -Eigenschaft stellt den Inhalt der HTTP-Antwort dar, und die- `HttpContent.ReadAsStringAsync` Methode schreibt den HTTP-Inhalt asynchron in eine Zeichenfolge. Dieser Inhalt wird dann von JSON in eine `List` von- `TodoItem` Instanzen konvertiert.

### <a name="creating-data"></a>Erstellen von Daten

Die `HttpClient.PostAsync` -Methode wird verwendet, um die Post-Anforderung an den durch den URI angegebenen Webdienst zu senden und dann die Antwort vom Webdienst zu empfangen, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await _client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");

  }
  ...
}
```

Die `TodoItem` Instanz wird in eine JSON-Nutzlast konvertiert, um Sie an den Webdienst zu senden. Diese Nutzlast wird dann in den Text des http-Inhalts eingebettet, der an den Webdienst gesendet wird, bevor die Anforderung mit der- `PostAsync` Methode erfolgt.

Der Rest-Dienst sendet einen HTTP-Statuscode in der- `HttpResponseMessage.IsSuccessStatusCode` Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Die häufigsten Antworten für diesen Vorgang lauten:

- **201 (erstellt)** – die Anforderung hat dazu geführt, dass eine neue Ressource erstellt wurde, bevor die Antwort gesendet wurde.
- **400 (ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **409 (Konflikt)** – die Anforderung konnte aufgrund eines Konflikts auf dem Server nicht ausgeführt werden.

### <a name="updating-data"></a>Aktualisieren von Daten

Die `HttpClient.PutAsync` -Methode wird verwendet, um die PUT-Anforderung an den Webdienst zu senden, der durch den URI angegeben wird. Anschließend wird die Antwort vom Webdienst empfangen, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await _client.PutAsync (uri, content);
  ...
}
```

Der Vorgang der- `PutAsync` Methode ist identisch mit der `PostAsync` -Methode, die zum Erstellen von Daten im Webdienst verwendet wird. Die möglichen Antworten, die vom Webdienst gesendet werden, unterscheiden sich jedoch.

Der Rest-Dienst sendet einen HTTP-Statuscode in der- `HttpResponseMessage.IsSuccessStatusCode` Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Die häufigsten Antworten für diesen Vorgang lauten:

- **204 (kein Inhalt)** – die Anforderung wurde erfolgreich verarbeitet, und die Antwort ist absichtlich leer.
- **400 (ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **404 (nicht gefunden)** – die angeforderte Ressource ist auf dem Server nicht vorhanden.

### <a name="deleting-data"></a>Löschen von Daten

Die `HttpClient.DeleteAsync` -Methode wird verwendet, um die DELETE-Anforderung an den Webdienst zu senden, der durch den URI angegeben wird. Anschließend wird die Antwort vom Webdienst empfangen, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  var response = await _client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

Der Rest-Dienst sendet einen HTTP-Statuscode in der- `HttpResponseMessage.IsSuccessStatusCode` Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Die häufigsten Antworten für diesen Vorgang lauten:

- **204 (kein Inhalt)** – die Anforderung wurde erfolgreich verarbeitet, und die Antwort ist absichtlich leer.
- **400 (ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **404 (nicht gefunden)** – die angeforderte Ressource ist auf dem Server nicht vorhanden.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Back-End-Diensten für Native Mobile Anwendungen](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
