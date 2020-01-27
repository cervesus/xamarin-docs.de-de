---
title: Einführung in Webdienste
description: In dieser Anleitung wird veranschaulicht, wie verschiedene Webdienst Technologien genutzt werden. Zu den behandelten Themen gehören die Kommunikation mit Rest-Diensten, SOAP-Diensten und Windows Communication Foundation-Diensten.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: ebd7cad9ef33a44dbc7aa469bb4e866bdfea2e61
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724781"
---
# <a name="introduction-to-web-services"></a>Einführung in Webdienste

_In dieser Anleitung wird veranschaulicht, wie verschiedene Webdienst Technologien genutzt werden. Zu den behandelten Themen gehören die Kommunikation mit Rest-Diensten, SOAP-Diensten und Windows Communication Foundation-Diensten._

Um ordnungsgemäß zu funktionieren, sind viele mobile Anwendungen von der Cloud abhängig, sodass das Integrieren von Webdiensten in Mobile Anwendungen ein gängiges Szenario ist. Die xamarin-Plattform unterstützt die Nutzung verschiedener Webdienst Technologien und umfasst integrierte und Drittanbieter Unterstützung für die Nutzung von Rest-, asmx-und Windows Communication Foundation (WCF)-Diensten.

Für Kunden, die xamarin. Forms verwenden, gibt es vollständige Beispiele für die Verwendung dieser Technologien in der Dokumentation zu [xamarin. Forms-Webdiensten](~/xamarin-forms/data-cloud/index.yml) .

> [!IMPORTANT]
> In ios 9 erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird.
> Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.

Sie können sich entscheiden, ob es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="rest"></a>REST

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

Die Einfachheit von REST hat dazu beigetragen, die primäre Methode für den Zugriff auf Web Services in mobilen Anwendungen zu vereinfachen.

## <a name="consuming-rest-services"></a>Verwenden von Rest-Diensten

Es gibt eine Reihe von Bibliotheken und Klassen, die verwendet werden können, um Rest-Dienste zu nutzen. in den folgenden Unterabschnitten werden diese erläutert. Weitere Informationen zur Nutzung eines REST-Diensts finden Sie unter verwenden [eines](~/xamarin-forms/data-cloud/web-services/rest.md)Rest-Diensts.

### <a name="httpclient"></a>HttpClient

Die [Microsoft HTTP-Client Bibliotheken](https://www.nuget.org/packages/Microsoft.Net.Http) stellen die `HttpClient`-Klasse bereit, die zum Senden und empfangen von Anforderungen über HTTP verwendet wird. Sie stellt Funktionen zum Senden von HTTP-Anforderungen und empfangen von HTTP-Antworten aus einer durch URI identifizierten Ressource bereit. Jede Anforderung wird als asynchronen Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Async-Unterstützung (Übersicht)](~/cross-platform/platform/async.md).

Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht vom Webdienst empfangen werden, nachdem eine HTTP-Anforderung erfolgt ist. Sie enthält Informationen über die Antwort, einschließlich Statuscode, Header und Text. Die `HttpContent` Klasse stellt die HTTP-Text und die Inhaltsheader, z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann mit einer der gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`je nach dem Format der Daten.

Weitere Informationen zum `HttpClient`-Klasse finden Sie unter [Erstellen des httpclient-Objekts](~/xamarin-forms/data-cloud/web-services/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Das Aufrufen von Webdiensten mit `HTTPWebRequest` umfasst Folgendes:

- Erstellen der Anforderungs Instanz für einen bestimmten URI.
- Festlegen verschiedener http-Eigenschaften für die Anforderungs Instanz.
- Abrufen eines `HttpWebResponse` aus der Anforderung.
- Lesen von Daten aus der Antwort.

Mit dem folgenden Code werden z. b. Daten aus der US-amerikanischen National Library of Medicine Web Service abgerufen:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

Im obigen Beispiel wird eine `HttpWebRequest` erstellt, die als JSON formatierte Daten zurückgibt. Die Daten werden in einem `HttpWebResponse`zurückgegeben, aus dem ein `StreamReader` abgerufen werden kann, um die Daten zu lesen.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Ein weiterer Ansatz für die Verwendung von Rest-Diensten ist die Verwendung der [restsharp](http://restsharp.org/) -Bibliothek. Restsharp kapselt HTTP-Anforderungen, einschließlich der Unterstützung für das Abrufen von Ergebnissen als unformatierten Zeichen folgen Inhalt C# oder als deserialisiertes Objekt. Der folgende Code sendet z. b. eine Anforderung an die US-amerikanische National Library of Medicine Web Service und ruft die Ergebnisse als JSON-formatierte Zeichenfolge ab:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` ist eine Methode, die die unformatierte JSON-Zeichenfolge aus der `RestSharp.RestResponse.Content`-Eigenschaft übernimmt C# und in ein-Objekt konvertiert. Die Deserialisierung von Daten, die von Webdiensten zurückgegeben werden, wird später in diesem Artikel erläutert.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Zusätzlich zu den Klassen, die in der Mono-Basisklassen Bibliothek (BCL) verfügbar sind, wie z. C# b. `HttpWebRequest`, und Bibliotheken von Drittanbietern, z. b. restsharp, sind plattformspezifische Klassen auch für die Nutzung von Webdiensten verfügbar. In ios können z. b. die Klassen `NSUrlConnection` und `NSMutableUrlRequest` verwendet werden.

Im folgenden Codebeispiel wird gezeigt, wie die US-amerikanische National Library of Medicine Web Service mithilfe von IOS-Klassen aufgerufen wird:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

Im Allgemeinen sollten plattformspezifische Klassen für die Verwendung von Webdiensten auf Szenarien beschränkt werden, in denen nativer C#Code zu portiert wird. Wenn möglich, sollte der Webdienst Zugriffs Code portabel sein, damit er plattformübergreifend freigegeben werden kann.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Eine weitere Option zum Aufrufen von Webdiensten ist die [Dienst Stapel](https://servicestack.net) Bibliothek. Der folgende Code zeigt beispielsweise, wie die `IServiceClient.GetAsync` Methode des Dienst Stapels verwendet wird, um eine Service Request auszugeben:

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> Tools wie servicestack und restsharp erleichtern das Aufrufen und Nutzen von Rest-Diensten. manchmal ist es jedoch nicht trivial, XML oder JSON zu verwenden, das nicht den standardserialisierungskonventionen von _DataContract_ entspricht. Rufen Sie ggf. die Anforderung auf, und behandeln Sie die geeignete Serialisierung explizit mithilfe der nachstehend beschriebenen servicestack. Text-Bibliothek.

<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Beanspruchen von Rest-Daten

RESTful-Webdienste verwenden JSON-Nachrichten in der Regel um Daten an den Client zurückzugeben. JSON ist ein textbasiertes Datenaustauschformat, das kompakte Nutzlasten erzeugt, was zu geringeren Bandbreitenanforderungen beim Senden von Daten führt. In diesem Abschnitt werden Mechanismen für die Verwendung von Rest-Antworten in JSON und Plain-Old-XML (POX) untersucht.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Die xamarin-Plattform bietet standardmäßig Unterstützung für JSON. Mithilfe eines `JsonObject`können Ergebnisse wie im folgenden Codebeispiel gezeigt abgerufen werden:

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Es ist jedoch wichtig zu wissen, dass die `System.Json` Tools die gesamten Daten in den Arbeitsspeicher laden.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

Die [newtonsoft JSON.Net Library](https://www.newtonsoft.com/json) ist eine häufig verwendete Bibliothek zum Serialisieren und Deserialisieren von JSON-Nachrichten. Im folgenden Codebeispiel wird gezeigt, wie JSON.NET verwendet wird, um eine JSON-Nachricht in C# ein-Objekt zu deserialisieren:

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack.Text

Servicestack. Text ist eine JSON-serialisierungsbibliothek, die für die Arbeit mit der servicestack-Bibliothek konzipiert ist. Im folgenden Codebeispiel wird gezeigt, wie JSON mithilfe eines `ServiceStack.Text.JsonObject`analysiert wird:

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

Bei der Verwendung eines XML-basierten Rest-Webdiensts können LINQ to XML verwendet werden, um den XML-Code zu analysieren und ein C# -Objekt wie im folgenden Codebeispiel gezeigt aufzufüllen:

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>ASP.NET-Webdienst (ASMX)

ASMX bietet die Möglichkeit, Webdienste zu erstellen, die Nachrichten mithilfe von SOAP (Simple Object Access Protocol) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten.

Eine SOAP-Nachricht ist ein XML-Dokument mit den folgenden Elementen:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Eine optionale *Header* -Element, das anwendungsspezifische Informationen z. B. Authentifizierungsdaten zu bewegen enthält. Wenn die *Header* Element vorhanden ist. es muss sich auf das erste untergeordnete Element des der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht den Empfänger enthält.
- Eine optionale *Fehler* -Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* -Element vorhanden ist, muss es ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, einschließlich HTTP, SMTP, TCP und UDP-ausgeführt werden. Allerdings kann ein ASMX-Dienst nur über HTTP verwendet werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies schließt die Unterstützung vieler den Standardkonfigurationen der ASMX-Dienst.

### <a name="generating-a-proxy"></a>Erstellen eines Proxys

Ein *Proxy* muss generiert werden, um einen ASMX-Dienst zu nutzen, der der Anwendung das Herstellen einer Verbindung mit dem Dienst ermöglicht. Der Proxy wird vom nutzenden Dienst-Metadaten erstellt, die die Methoden und zugeordnete Dienstkonfiguration definiert. Diese Metadaten werden als Web Services Description Language-Dokument (WSDL) verfügbar gemacht, das vom Webdienst generiert wird. Der Proxy wird erstellt, indem Visual Studio für Mac oder Visual Studio verwendet wird, um den plattformspezifischen Projekten einen Webverweis für den Webdienst hinzuzufügen.

Die Webdienst-URL kann entweder eine gehostete Remote Quelle oder eine lokale Dateisystem Ressource sein, auf die über das Präfix `file:///` Pfad zugegriffen werden kann. Beispiel:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "The web service URL can either be a hosted remote source or local file system resource accessible via the file path prefix")](images/add-webreference-dialog.png#lightbox)

Dadurch wird der Proxy im Web-oder Dienst Verweis Ordner des Projekts generiert. Da es sich bei einem Proxy um generierten Code handelt, sollte er nicht geändert werden.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Manuelles Hinzufügen eines Proxys zu einem Projekt

Wenn Sie über einen vorhandenen Proxy verfügen, der mit kompatiblen Tools generiert wurde, kann diese Ausgabe genutzt werden, wenn Sie als Teil des Projekts enthalten ist. Verwenden Sie in Visual Studio für Mac die **Dateien hinzufügen...** Menüoption zum Hinzufügen des Proxys. Außerdem erfordert dies die explizite referenzierte Verwendung von " *System. Web. Services. dll* " mithilfe der **Add-Verweise...** hinzu.

### <a name="consuming-the-proxy"></a>Verwenden des Proxys

Die generierten Proxyklassen bieten Methoden für die Nutzung des Webdiensts, die das asynchrone Programmiermodell (APM)-Entwurfsmuster zu verwenden. In diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Das folgende Codebeispiel veranschaulicht dieses Verhalten:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `Task.Factory.FromAsync` Methode. Diese Methode erstellt eine `Task`, die die `TodoService.EndGetTodoItems`-Methode ausführt, sobald die `TodoService.BeginGetTodoItems`-Methode abgeschlossen ist. der `null`-Parameter gibt an, dass keine Daten an den `BeginGetTodoItems` Delegaten übergeben werden. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM finden Sie unter [asynchrones Programmiermodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zum Verwenden eines ASMX-Diensts finden Sie unter verwenden [eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF ist das vereinheitlichte Framework von Microsoft zum entwickeln Dienst orientierter Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen.

WCF wird einen Dienst mit einer Vielzahl von verschiedene Verträge, die im folgenden beschrieben:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt in einer Nachricht zu bilden.
- **Nachrichtenverträge** – Erstellen von Nachrichten von vorhandene Datenverträge.
- **Fehler Verträge** – ermöglichen von benutzerdefinierten SOAP-Fehlern angegeben werden.
- **-Dienstverträge** : Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem Verhalten benutzerdefinierter Fehler, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen, die ASMX bereitgestellt werden: SOAP-Nachrichten über HTTP unterstützt.

> [!IMPORTANT]
> Die xamarin-Platt Form Unterstützung für WCF ist auf Text codierte SOAP-Nachrichten über HTTP/HTTPS mit der `BasicHttpBinding`-Klasse beschränkt. Unterstützung der WCF erfordert außerdem die Verwendung von Tools, die nur verfügbar, in einer Windows-Umgebung in den Proxy zu generieren.

### <a name="generating-a-proxy"></a>Erstellen eines Proxys

Ein *Proxy* muss generiert werden, um einen WCF-Dienst nutzen, die der Anwendung ermöglicht, mit dem Dienst herstellen. Der Proxy wird erstellt, indem verarbeitende Dienstmetadaten, die die Methoden und zugeordnete Dienstkonfiguration zu definieren. Diese Metadaten werden in die Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die vom Webdienst generiert wird. Der Proxy kann erstellt werden, indem der Microsoft WCF Web Service Reference Provider in Visual Studio 2017 verwendet wird, um einer .NET Standard Bibliothek einen Dienst Verweis für den Webdienst hinzuzufügen.

Eine Alternative zum Erstellen des Proxys verwenden den Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Konfigurieren des Proxys

Beim Konfigurieren des generierten Proxys werden in der Regel zwei Konfigurations Argumente (abhängig von SOAP 1.1/ASMX oder WCF) während der Initialisierung übernommen: die `EndpointAddress` und/oder die zugehörigen Bindungs Informationen, wie im folgenden Beispiel gezeigt:

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Eine Bindung wird verwendet, an den Transport, Codierung und Protokolldetails erforderlich für Anwendungen und Dienste miteinander kommunizieren. Die `BasicHttpBinding` gibt an, dass textcodierten SOAP-Nachrichten über das HTTP-Transport-Protokoll gesendet werden. Angeben einer Endpunktadresse kann die Anwendung mit verschiedenen Instanzen von den WCF-Dienst herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

### <a name="consuming-the-proxy"></a>Verwenden des Proxys

Die generierten Proxyklassen bieten Methoden für die Nutzung der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. Bei diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Das folgende Codebeispiel veranschaulicht dieses Verhalten:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `Task.Factory.FromAsync` Methode. Diese Methode erstellt eine `Task`, die die `TodoServiceClient.EndGetTodoItems`-Methode ausführt, sobald die `TodoServiceClient.BeginGetTodoItems`-Methode abgeschlossen ist. der `null`-Parameter gibt an, dass keine Daten an den `BeginGetTodoItems` Delegaten übergeben werden. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM finden Sie unter [asynchrones Programmiermodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zum Verarbeiten eines WCF-Diensts finden Sie unter verwenden [eines Windows Communication Foundation (WCF)-Webdiensts](~/xamarin-forms/data-cloud/web-services/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Verwenden der Transport Sicherheit

WCF-Dienste können die Sicherheit auf Transport Ebene nutzen, um das Abfangen von Nachrichten zu schützen Die xamarin-Plattform unterstützt Bindungen, die die Sicherheit auf Transport Ebene mithilfe von SSL verwenden. Es gibt jedoch Fälle, in denen der Stapel das Zertifikat möglicherweise validieren muss, was zu einem unerwarteten Verhalten führt. Die Validierung kann überschrieben werden, indem ein `ServerCertificateValidationCallback` Delegaten registriert wird, bevor der Dienst aufgerufen wird, wie im folgenden Codebeispiel gezeigt:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Dies behält die transportverschlüsselung bei und ignoriert dabei die serverseitige Zertifikats Überprüfung. Bei dieser Vorgehensweise werden jedoch die dem Zertifikat zugeordneten Vertrauens Probleme effektiv ignoriert und sind möglicherweise nicht geeignet. Weitere Informationen finden Sie unter [respektvolle Verwendung vertrauenswürdiger](https://www.mono-project.com/UsingTrustedRootsRespectfully) Stämme auf [Mono-Project.com](https://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>Verwenden der Client Anmelde Informationen-Sicherheit

WCF-Dienste erfordern möglicherweise auch, dass sich die Dienst Clients mithilfe von Anmelde Informationen authentifizieren. Die xamarin-Plattform unterstützt das WS-Security-Protokoll nicht, das es Clients ermöglicht, Anmelde Informationen innerhalb des SOAP-Nachrichten Umschlags zu senden. Die xamarin-Plattform unterstützt jedoch die Möglichkeit, http-Anmelde Informationen für die Standard Authentifizierung an den Server zu senden, indem die entsprechenden `ClientCredentialType`angegeben werden:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Anschließend können die Anmelde Informationen für die Standard Authentifizierung angegeben werden:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

Wenn Sie im obigen Beispiel die Meldung "Out of Trampolines of type 0" erhalten, können Sie die Anzahl der basolines vom Typ 0 erhöhen, indem Sie dem Build das `–aot “trampolines={number of trampolines}”` Argument hinzufügen. Weitere Informationen finden Sie unter [Problembehandlung](~/ios/troubleshooting/troubleshooting.md#trampolines).

Weitere Informationen zur http-Standard Authentifizierung, obwohl im Kontext eines Rest-Webdiensts, finden Sie unter [Authentifizieren eines Rest-Webdiensts](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="related-links"></a>Verwandte Themen

- [Webdienste in xamarin. Forms](~/xamarin-forms/data-cloud/index.yml)
- [Service Model Metadata Utility-Tool (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
