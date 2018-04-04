---
title: Nutzen einen RESTful-Webdienst
description: Integrieren von einem Webdienst in einer Anwendung ist ein gängiges Szenario. Dieser Artikel veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung nutzen.
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 48b81c5beb1643501c69e5de1ea4f4197d587001
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-a-restful-web-service"></a>Nutzen einen RESTful-Webdienst

_Integrieren von einem Webdienst in einer Anwendung ist ein gängiges Szenario. Dieser Artikel veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung nutzen._

Representational State Transfer (REST) ist eine Architektur zum Erstellen von Webdiensten. REST-Anforderungen werden über HTTP angefordert mithilfe der gleichen HTTP-Verben, die Webbrowser verwenden Sie zum Abrufen von Webseiten und Daten an Server senden. Die Verben sind:

- **Abrufen** – dieser Vorgang wird verwendet, um Daten aus dem Webdienst abzurufen.
- **POST** – dieser Vorgang wird verwendet, um ein neues Element der Daten auf den Webdienst zu erstellen.
- **PUT** – dieser Vorgang wird verwendet, um ein Element der Daten auf den Webdienst zu aktualisieren.
- **Patch für** – dieser Vorgang zum Aktualisieren eines Datenelements auf den Webdienst beschreiben Sie einen Satz von Anweisungen zur wie das Element geändert werden sollte. Das Verb ist nicht in der beispielanwendung verwendet.
- **Löschen Sie** – dieser Vorgang wird verwendet, um ein Element der Daten auf den Webdienst zu löschen.

APIs, die Folgen REST-Webdienst RESTful-APIs aufgerufen werden, und mit definiert werden:

- Eine Basis-URI.
- HTTP-Methoden, z. B. GET, POST, PUT, PATCH oder DELETE.
- Einen Medientyp für die Daten, z. B. JavaScript Object Notation (JSON).

Rest-Webdienste verwenden JSON-Nachrichten in der Regel Daten an den Client zurückgegeben. JSON ist eine textbasierte Datenaustauschformat, dass erzeugt compact Nutzlasten, was zur Folge bandbreitenanforderungen, beim Senden von Daten reduziert. Die beispielanwendung verwendet, die open Source [NewtonSoft JSON.NET Bibliothek](http://www.newtonsoft.com/json) zum Serialisieren und Deserialisieren von Nachrichten.

Die Einfachheit von REST hat dazu beigetragen, erleichtern die Hauptmethode für den Zugriff auf Webdienste in mobilen Anwendungen.

Anweisungen zum Einrichten des REST-Diensts können in der Readme-Datei gefunden werden, die die beispielanwendung gehören. Jedoch wenn die beispielanwendung ausgeführt wird, wird es verbinden eine Xamarin-gehostete REST-Dienst, der nur-Lese Zugriff auf Daten bereitstellt wie im folgenden Screenshot gezeigt:

![](rest-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
>
>ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die **HTTPS** -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Den Webdienst

Der REST-Dienst mithilfe von ASP.NET Core geschrieben und stellt die folgenden Vorgänge:

|Vorgang|HTTP-Methode|Relativer URI|Parameter|
|--- |--- |--- |--- |
|Abrufen einer Liste von Aufgaben|GET|/api/todoitems/|
|Ein neues Aufgabenelement erstellen|BEREITSTELLEN|/api/todoitems/|Eine JSON-Format TodoItem|
|Aktualisieren einer Aufgabe|PUT|/api/todoitems/|Eine JSON-Format TodoItem|
|Löschen einer Aufgabe|DELETE|/api/todoitems/{id}|

Die Mehrheit der URIs umfassen die `TodoItem` ID im Pfad. So löschen Sie beispielsweise die `TodoItem` , deren ID ist `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, der Client sendet eine DELETE-Anforderung zu `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Weitere Informationen zu das Datenmodell in der beispielanwendung verwendet, finden Sie unter [die Daten modellieren,](~/xamarin-forms/data-cloud/walkthrough.md).

Wenn die Web-API-Framework eine Anforderung empfängt weitergeleitet die Anforderung einer Aktion. Diese Aktionen werden nur öffentliche Methoden in der `TodoItemsController` Klasse. Das Framework verwendet eine Routingtabelle, um zu bestimmen, welche Maßnahme als Reaktion auf eine Anforderung aufrufen, die im folgenden Codebeispiel gezeigt wird:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Die Routingtabelle enthält eine routenvorlage für die und die Web-API-Framework eine HTTP-Anforderung empfängt, versucht, den URI für die routenvorlage in der Routingtabelle überein. Wenn kein übereinstimmendes kann Route gefunden werden, dass der Client Fehler 404 (nicht gefunden) empfängt. Wenn eine übereinstimmende Route gefunden wird, wählt Web-API-Controller und Aktion wie folgt:

- Um den Controller zu suchen, Web-API hinzugefügt "Controller" der Wert der *{Controller}* Variable.
- Um die Aktion zu suchen, Web-API prüft die HTTP-Methode und untersucht die Controlleraktionen, die mit der gleichen HTTP-Methode als ein Attribut ergänzt werden.
- Die *{Id}* Platzhaltervariable an einen Aktionsparameter zugeordnet ist.

Der REST-Dienst verwendet die Standardauthentifizierung. Weitere Informationen finden Sie unter [einen RESTful-Webdienst authentifiziert](~/xamarin-forms/data-cloud/authentication/rest.md). Weitere Informationen zu ASP.NET Web API-routing finden Sie unter [Routing in ASP.NET Web API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) auf der ASP.NET-Website. Weitere Informationen zu den REST-Dienst mithilfe von ASP.NET Core erstellen, finden Sie unter [Erstellen von Back-End-Diensten für systemeigene Mobile Anwendungen](/aspnet/core/mobile/native-mobile-backend/).

> [!NOTE]
> Die beispielanwendung nutzt die Xamarin-gehostete REST-Dienst, der nur-Lese Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten die in der Anwendung verwendeten Daten nicht geändert. Eine hostfähige Version der REST-Dienst ist jedoch verfügbar, in der **TodoRESTService** Ordner, in dem zugehörigen [Beispielcode](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/).
> Wenn Sie den REST-Dienst selbst hosten, können Sie in der vollständigen erstellen, aktualisieren, lesen und löschen Sie den Zugriff auf die Daten.

Die `HttpClient` Klasse dient zum Senden und Empfangen von Anforderungen über HTTP. Sie bietet Funktionen zum Senden von HTTP-Anforderungen und Empfangen von HTTP-Antworten aus einem URI identifizierte Ressource. Jede Anforderung wird als asynchronen Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Überblick über den asynchronen](~/cross-platform/platform/async.md).

Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht vom Webdienst empfangen werden, nachdem eine HTTP-Anforderung gestellt wurde. Es enthält Informationen über die Antwort, einschließlich der Statuscode, Header und eine beliebige Stelle. Die `HttpContent` Klasse stellt die HTTP-Texts und der bereitgestellten Inhaltsheader wie z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann einer beliebigen, von gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`je nach dem Format der Daten.

### <a name="creating-the-httpclient-object"></a>Beim Erstellen des Objekts "HttpClient"

Die `HttpClient` Instanz wird auf der Klassenebene deklariert, sodass für das Objekt aktiv ist, solange die Anwendung HTTP-Anforderungen zu stellen, wie im folgenden Codebeispiel gezeigt muss:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

Die `HttpClient.MaxResponseContentBufferSize` Eigenschaft wird verwendet, um die maximale Anzahl von Bytes, die beim Lesen des Inhalts in der HTTP-Antwortnachricht gepuffert angeben. Die Standardgröße dieser Eigenschaft ist die maximale Größe einer ganzen Zahl. Aus diesem Grund wird die Eigenschaft festgelegt auf einen niedrigeren Wert als Schutzmechanismus, um die Menge der Daten zu beschränken, von denen die Anwendung als eine Antwort vom Webdienst akzeptiert.

### <a name="retrieving-data"></a>Abrufen von Daten

Die `HttpClient.GetAsync` Methode wird verwendet, um die GET-Anforderung an den Webdienst, die vom URI angegebenen senden und empfangen Sie dann die Antwort vom Webdienst, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Der REST-Dienst sendet einen Statuscode "HTTP" in der `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder nicht. Für diesen Vorgang den REST sendet Service HTTP-Statuscode 200 (OK), in der Antwort, die angibt, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort ist.

Wenn der HTTP-Vorgang erfolgreich war, der Inhalt der Antwort wird gelesen, für die Anzeige. Die `HttpResponseMessage.Content` Eigenschaft stellt den Inhalt der HTTP-Antwort dar und die `HttpContent.ReadAsStringAsync` Methode schreibt den HTTP-Inhalt asynchron in eine Zeichenfolge. Dieser Inhalt wird dann in JSON konvertiert eine `List` von `TodoItem` Instanzen.

### <a name="creating-data"></a>Erstellen von Daten

Die `HttpClient.PostAsync` Methode wird verwendet, um die POST-Anforderung an den Webdienst, der durch den URI angegebene senden und dann zum Empfangen der Antwort aus dem Webdienst, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

Die `TodoItem` Instanz in eine JSON-Nutzlast zum Senden an den Webdienst konvertiert wird. Diese Nutzlast wird dann eingebettet, im Hauptteil der HTTP-Inhalt, der an den Webdienst gesendet wird, bevor die Anforderung wird mit der `PostAsync` Methode.

Der REST-Dienst sendet einen Statuscode "HTTP" in der `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder nicht. Die allgemeine Antworten für diesen Vorgang sind:

- **201 (erstellt)** – die Anforderung führte zu einer neuen Ressource erstellt werden, bevor die Antwort gesendet wurde.
- **400 (Ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **409 (Konflikt)** – die Anforderung konnte nicht durchgeführt werden aufgrund eines Konflikts auf dem Server.

### <a name="updating-data"></a>Aktualisieren von Daten

Die `HttpClient.PutAsync` Methode wird verwendet, um die PUT-Anforderung an den Webdienst, die vom URI angegebenen senden und empfangen Sie dann die Antwort vom Webdienst, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
Der Vorgang von der `PutAsync` -Methode ist identisch mit der `PostAsync` -Methode, die zum Erstellen von Daten im Webdienst verwendet wird. Unterscheiden sich jedoch der möglichen Antworten vom Webdienst gesendet.

Der REST-Dienst sendet einen Statuscode "HTTP" in der `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder nicht. Die allgemeine Antworten für diesen Vorgang sind:

- **204 (kein Inhalt)** – die Anforderung erfolgreich verarbeitet wurde, und die Antwort wird absichtlich leer.
- **400 (Ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **404 (nicht gefunden)** – die angeforderte Ressource ist auf dem Server nicht vorhanden.

### <a name="deleting-data"></a>Löschen von Daten

Die `HttpClient.DeleteAsync` Methode wird verwendet, um die DELETE-Anforderung an den Webdienst, die vom URI angegebenen senden und empfangen Sie dann die Antwort vom Webdienst, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

Der REST-Dienst sendet einen Statuscode "HTTP" in der `HttpResponseMessage.IsSuccessStatusCode` -Eigenschaft, um anzugeben, ob die HTTP-Anforderung erfolgreich war oder nicht. Die allgemeine Antworten für diesen Vorgang sind:

- **204 (kein Inhalt)** – die Anforderung erfolgreich verarbeitet wurde, und die Antwort wird absichtlich leer.
- **400 (Ungültige Anforderung)** – die Anforderung wird vom Server nicht verstanden.
- **404 (nicht gefunden)** – die angeforderte Ressource ist auf dem Server nicht vorhanden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel untersucht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung mithilfe der `HttpClient` Klasse. Die Einfachheit von REST hat dazu beigetragen, erleichtern die Hauptmethode für den Zugriff auf Webdienste in mobilen Anwendungen.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Back-End-Diensten für native mobile Anwendungen](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
