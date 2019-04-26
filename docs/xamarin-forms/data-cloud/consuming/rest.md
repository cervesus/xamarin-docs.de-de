---
title: Verwenden eines RESTful-Web-Diensts
description: Die Integration von einem Webdienst in eine Anwendung ist ein gängiges Szenario. In diesem Artikel wird veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 1b25a4a1b65a1473bd122ae9cf7c1a6a72ff9ccc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61328209"
---
# <a name="consuming-a-restful-web-service"></a>Verwenden eines RESTful-Web-Diensts

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)

_Die Integration von einem Webdienst in eine Anwendung ist ein gängiges Szenario. In diesem Artikel wird veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung genutzt wird._

Representational State Transfer (REST) ist ein Architekturstil zum Erstellen von Webdiensten. REST-Anforderungen erfolgen über HTTP mit den gleichen HTTP-Verben, die Webbrowser zum Abrufen von Webseiten und zum Senden von Daten auf Servern verwenden. Die Verben sind:

- **ERSTE** – dieser Vorgang wird verwendet, um Daten aus dem Webdienst abzurufen.
- **POST** – dieser Vorgang wird verwendet, um ein neues Element der Daten auf den Webdienst zu erstellen.
- **PUT** – dieser Vorgang wird verwendet, um ein Element der Daten auf den Webdienst zu aktualisieren.
- **PATCH** – dieser Vorgang wird verwendet, um ein Element der Daten auf den Webdienst aktualisieren, indem Sie beschreiben einen Satz von Anweisungen dazu, wie das Element geändert werden soll. Dieses Verb ist nicht in der beispielanwendung verwendet.
- **Löschen Sie** – dieser Vorgang wird verwendet, um ein Element der Daten auf den Webdienst zu löschen.

Webdienst-APIs, die mit REST entsprechen RESTful-APIs aufgerufen werden, und mit definiert werden:

- Basis-URI.
- HTTP-Methoden, z. B. GET, POST, PUT, PATCH oder DELETE.
- Ein anderes Medium für die Daten, z. B. JavaScript Object Notation (JSON).

RESTful-Webdienste verwenden JSON-Nachrichten in der Regel um Daten an den Client zurückzugeben. JSON ist ein textbasiertes Datenaustauschformat, dass erzeugt compact Nutzlasten, sodass Anforderungen an die Netzwerkbandbreite, beim Senden von Daten eingeschränkt. Die beispielanwendung verwendet die open Source [newtonsoft-JSON.NET-Bibliothek](http://www.newtonsoft.com/json) zum Serialisieren und Deserialisieren von Nachrichten.

Die Einfachheit von REST hat dazu beigetragen, die primäre Methode für den Zugriff auf Web Services in mobilen Anwendungen zu vereinfachen.

Wenn die Anwendung ausgeführt wird, wird es auf einen lokal gehosteten REST-Dienst verbinden, wie im folgenden Screenshot gezeigt:

![](rest-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
>
>ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die **HTTPS** -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Nutzung des Webdiensts

Der REST-Dienst ist in ASP.NET Core geschrieben und bietet die folgenden Vorgänge:

|Vorgang|HTTP-Methode|Relativer URI|Parameter|
|--- |--- |--- |--- |
|Abrufen einer Liste von To-Do-Elementen|GET|/api/todoitems/|
|Erstellt ein neues to-do-Element|POST|/api/todoitems/|Eine JSON-formatierte TodoItem|
|Aktualisieren eines To-Do-Elements|PUT|/api/todoitems/|Eine JSON-formatierte TodoItem|
|Löschen eines To-Do-Elements|DELETE|/api/todoitems/{id}|

Die Mehrheit der URIs umfassen die `TodoItem` -ID im Pfad. So löschen Sie beispielsweise die `TodoItem` , deren ID `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, der Client sendet eine DELETE-Anforderung an `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Weitere Informationen zu Datenmodell, das in der beispielanwendung verwendet, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/walkthrough.md).

Wenn die Web-API-Framework eine Anforderung empfängt weitergeleitet die Anforderung an eine Aktion. Diese Aktionen sind einfach öffentliche Methoden in der `TodoItemsController` Klasse. Das Framework verwendet eine Routingtabelle, um zu bestimmen, welche Aktion als Reaktion auf eine Anforderung, rufen Sie die in der im folgenden Codebeispiel gezeigt wird:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Die Routingtabelle enthält eine routenvorlage, und das Web-API-Framework eine HTTP-Anforderung empfängt, versucht, den URI für die routenvorlage in der Routingtabelle überein. Wenn kein übereinstimmendes wird Route kann nicht gefunden werden, dass der Client Fehler 404 (nicht gefunden) empfängt. Wenn eine übereinstimmende Route gefunden wird, wählt Web-API-Controller und die Aktion wie folgt:

- Um den Controller zu suchen, Web-API hinzugefügt "Controller" der Wert des der *{Controller}* Variable.
- Um die Aktion zu suchen, Web-API die HTTP-Methode untersucht und untersucht die Controller-Aktionen, die mit der gleichen HTTP-Methode als ein Attribut ergänzt werden.
- Die *{Id}* die Platzhaltervariable einen Action-Parameter zugeordnet ist.

Der REST-Dienst verwendet die Standardauthentifizierung. Weitere Informationen finden Sie unter [authentifizieren einen RESTful Webdienst](~/xamarin-forms/data-cloud/authentication/rest.md). Weitere Informationen zum routing von ASP.NET Web-API finden Sie unter [Routing in ASP.NET Web-API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) auf der ASP.NET-Website. Weitere Informationen zu den REST-Dienst mithilfe von ASP.NET Core erstellen, finden Sie unter [Erstellen von Back-End-Diensten für Native Mobile Anwendungen](/aspnet/core/mobile/native-mobile-backend/).

Die `HttpClient` Klasse wird verwendet, um das Senden und Empfangen von Anforderungen über HTTP. Es stellt Funktionen bereit, für das HTTP-Anforderungen senden und Empfangen von HTTP-Antworten aus einem URI identifizierte Ressource. Jede Anforderung wird als asynchronen Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Async-Unterstützung (Übersicht)](~/cross-platform/platform/async.md).

Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht vom Webdienst empfangen werden, nachdem eine HTTP-Anforderung erfolgt ist. Es enthält Informationen über die Antwort, einschließlich der Statuscode, Header und alle Text. Die `HttpContent` Klasse stellt die HTTP-Text und die Inhaltsheader, z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann mit einer der gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`je nach dem Format der Daten.

### <a name="creating-the-httpclient-object"></a>Erstellen die HTTPClient-Objekt

Die `HttpClient` Instanz wird auf der Klassenebene deklariert, sodass für das Objekt befindet, solange die Anwendung auf HTTP-Anforderungen durchzuführen, wie im folgenden Codebeispiel wird gezeigt muss:

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

Die `HttpClient.GetAsync` Methode wird verwendet, um die GET-Anforderung an den Webdienst, der vom URI angegebenen senden und empfangen Sie dann die Antwort vom Webdienst, wie im folgenden Codebeispiel gezeigt:

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

Der REST-Dienst sendet einen HTTP-Statuscode in den `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Für diesen Vorgang den REST sendet Service HTTP-Statuscode 200 (OK), in der Antwort gibt an, dass die Anforderung erfolgreich war und die angeforderte Informationen in der Antwort ist.

Wenn der HTTP-Vorgang erfolgreich war, der Inhalt der Antwort gelesen wurde, für die Anzeige. Die `HttpResponseMessage.Content` Eigenschaft darstellt, den Inhalt der HTTP-Antwort, und die `HttpContent.ReadAsStringAsync` Methode schreibt den HTTP-Inhalt asynchron in eine Zeichenfolge. Dieser Inhalt wird dann von JSON in konvertiert eine `List` von `TodoItem` Instanzen.

### <a name="creating-data"></a>Erstellen von Daten

Die `HttpClient.PostAsync` Methode dient zum Senden der POST-Anforderung an den Webdienst, der durch den URI angegeben, und klicken Sie dann zum Empfangen der Antwort aus dem Webdienst, wie im folgenden Codebeispiel gezeigt:

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

Die `TodoItem` Instanz in eine JSON-Nutzlast zum Senden an den Webdienst konvertiert wird. Diese Nutzlast wird dann eingebettet, im Hauptteil der HTTP-Inhalt, die an den Webdienst gesendet wird, bevor die Anforderung wird mit der `PostAsync` Methode.

Der REST-Dienst sendet einen HTTP-Statuscode in den `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Für diesen Vorgang die häufige Antworten lauten:

- **201 (CREATED)** – die Anforderung führte zu einer neuen Ressource, die erstellt wird, bevor die Antwort gesendet wurde.
- **400 (Ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **409 (Konflikt)** – die Anforderung konnte nicht ausgeführt werden kann aufgrund eines Konflikts auf dem Server.

### <a name="updating-data"></a>Aktualisieren von Daten

Die `HttpClient.PutAsync` Methode wird verwendet, um die PUT-Anforderung an den Webdienst, der vom URI angegebenen senden und empfangen Sie dann die Antwort vom Webdienst, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await _client.PutAsync (uri, content);
  ...
}
```
Der Vorgang von der `PutAsync` Methode ist identisch mit der `PostAsync` -Methode, die zum Erstellen von Daten im Webdienst verwendet wird. Unterscheiden sich jedoch der möglichen Antworten vom Webdienst gesendet.

Der REST-Dienst sendet einen HTTP-Statuscode in den `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Für diesen Vorgang die häufige Antworten lauten:

- **204 (NO CONTENT)** – die Anforderung erfolgreich verarbeitet wurde und die Antwort ist absichtlich leer.
- **400 (Ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **404 (nicht gefunden)** : die angeforderte Ressource ist auf dem Server nicht vorhanden.

### <a name="deleting-data"></a>Löschen von Daten

Die `HttpClient.DeleteAsync` Methode wird verwendet, um die DELETE-Anforderung an den Webdienst, der vom URI angegebenen senden und empfangen Sie dann die Antwort vom Webdienst, wie im folgenden Codebeispiel gezeigt:

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

Der REST-Dienst sendet einen HTTP-Statuscode in den `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Für diesen Vorgang die häufige Antworten lauten:

- **204 (NO CONTENT)** – die Anforderung erfolgreich verarbeitet wurde und die Antwort ist absichtlich leer.
- **400 (Ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **404 (nicht gefunden)** : die angeforderte Ressource ist auf dem Server nicht vorhanden.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Back-End-Diensten für native mobile Anwendungen](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
