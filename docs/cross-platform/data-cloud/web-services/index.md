---
title: Einführung in Webdienste
description: Diese Anleitung veranschaulicht, wie andere webdiensttechnologien genutzt wird. Die behandelten Themen umfassen die Kommunikation mit REST-Diensten, SOAP-Dienste und Windows Communication Foundation-Dienste.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: afebe7f491855844e18bf054d665cf8d54e8f353
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61183876"
---
# <a name="introduction-to-web-services"></a>Einführung in Webdienste

_Diese Anleitung veranschaulicht, wie andere webdiensttechnologien genutzt wird. Die behandelten Themen umfassen die Kommunikation mit REST-Diensten, SOAP-Dienste und Windows Communication Foundation-Dienste._

Um korrekt zu funktionieren, zahlreichen mobile Anwendungen sind abhängig von der Cloud, und so Web Services in mobilen Anwendungen integrieren, ist ein gängiges Szenario. Die Xamarin-Plattform unterstützt verschiedene webdiensttechnologien Verbrauch, und enthält integrierte und Drittanbieter-Unterstützung für die Nutzung von RESTful-basierten, ASMX und Windows Communication Foundation (WCF)-Dienste.

Für Kunden, die mit Xamarin.Forms, es gibt vollständige Beispiele, die mit einer dieser Technologien in der [Xamarin.Forms-Webdienste](~/xamarin-forms/data-cloud/index.md) Dokumentation.

> [!IMPORTANT]
> In iOS 9 setzt App Transport Security (ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen.
> Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.

Sie können opt-Out-of ATS ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

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

## <a name="consuming-rest-services"></a>Nutzen von REST-Diensten

Es gibt eine Reihe von Bibliotheken und Klassen, die zum Verarbeiten von REST-Diensten verwendet werden können, und in den folgenden Unterabschnitten erläutern. Weitere Informationen zur Nutzung eines REST-Diensts finden Sie unter [verwenden eine RESTful-Webdiensts](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

Die [Microsoft HTTP Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http) bietet die `HttpClient` -Klasse, die zum Senden und Empfangen von Anforderungen über HTTP verwendet wird. Es stellt Funktionen für HTTP-Anforderungen senden und Empfangen von HTTP-Antworten aus einer URI identifizierte Ressource bereit. Jede Anforderung wird als asynchronen Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Async-Unterstützung (Übersicht)](~/cross-platform/platform/async.md).

Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht vom Webdienst empfangen werden, nachdem eine HTTP-Anforderung erfolgt ist. Es enthält Informationen über die Antwort, einschließlich Statuscode, Header und Text. Die `HttpContent` Klasse stellt die HTTP-Text und die Inhaltsheader, z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann mit einer der gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`je nach dem Format der Daten.

Weitere Informationen zu den `HttpClient` Klasse, finden Sie unter [Erstellen des Objekts "HttpClient"](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Aufrufen von Webdiensten mit `HTTPWebRequest` umfasst:

-  Erstellen die Anforderungsinstanz für einen bestimmten URI ein.
-  Verschiedene HTTP-Eigenschaften festgelegt für die Anforderungsinstanz.
-  Abrufen einer `HttpWebResponse` aus der Anforderung.
-  Lesen von Daten aus der Antwort.

Der folgende Code ruft z. B. Daten aus den USA National-Bibliothek von Medizin-Webdienst:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
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

Das obige Beispiel erstellt eine `HttpWebRequest` als JSON formatierte Daten zurückgegeben. Die Daten werden zurückgegeben, eine `HttpWebResponse`, von dem eine `StreamReader` zum Lesen der Daten abgerufen werden kann.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Ein weiteres Verfahren zum Nutzen von REST-Diensten wird mithilfe der [RestSharp](http://restsharp.org/) Bibliothek. RestSharp kapselt die HTTP-Anforderungen, einschließlich der Unterstützung für das Abrufen von Ergebnissen, die entweder als unformatierte Zeichenfolgeninhalt oder eine deserialisierte C# Objekt. Der folgende Code sendet beispielsweise eine Anforderung in den USA Nationale Webdienst für die Bibliothek der Medizin und ruft die Ergebnisse als JSON-formatierte Zeichenfolge:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` ist eine Methode, mit denen die unformatierte JSON-Zeichenfolge aus gelangen die `RestSharp.RestResponse.Content` Eigenschaft und konvertieren Sie ihn in eine C# Objekt. Deserialisierung von Daten, die von Webdiensten zurückgegeben wird weiter unten in diesem Artikel erläutert.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Zusätzlich zu den Klassen zur Verfügung, in der Mono-Basis-Klassenbibliothek (BCL), wie z. B. `HttpWebRequest`, und von Drittanbietern C# Bibliotheken, z. B. RestSharp, plattformspezifischen Klassen sind auch für die Nutzung von Webdiensten verfügbar. In iOS z. B. die `NSUrlConnection` und `NSMutableUrlRequest` Klassen können verwendet werden.

Im folgenden Codebeispiel wird veranschaulicht, wie zum Aufrufen der USA National-Bibliothek von Medizin-Webdienst mithilfe von iOS-Klassen:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
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

Im Allgemeinen sollte plattformspezifischen Klassen für die Nutzung von Webdiensten auf Szenarien, in dem systemeigenen Code ist, der auf portiert, beschränkt C#. Nach Möglichkeit sollten Webdienstcode Zugriff portabel sein, damit es auf gemeinsam genutzten plattformübergreifenden sein kann.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Eine weitere Möglichkeit zum Aufrufen von Webdiensten ist die [Dienststapel](http://www.servicestack.net/) Bibliothek. Der folgende Code zeigt z. B. wie Sie mit der Dienststapel `IServiceClient.GetAsync` Methode, um eine dienstanforderung ausgeben:

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
> Während Tools wie ServiceStack und RestSharp erleichtern zum Aufrufen und Nutzen von-Dienste Rest, manchmal ist es nicht trivialen nutzen XML oder JSON, die nicht dem Standard entspricht _DataContract_ Serialisierung Konventionen. Falls erforderlich, rufen Sie die Anforderung, und Behandeln der entsprechenden Serialisierungs explizit mithilfe der ServiceStack.Text-Bibliothek, die weiter unten erläutert.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Nutzen von RESTful-Daten

RESTful-Webdienste verwenden JSON-Nachrichten in der Regel um Daten an den Client zurückzugeben. JSON ist eine textbasierte, Datenaustauschformat, das erstellt, Nutzlasten und komprimiert die Anforderungen an die geringere Netzwerkbandbreite beim Senden von Daten führt. In diesem Abschnitt werden die Mechanismen für die Nutzung von RESTful-Antworten im JSON-"und" Plain-Old-XML (POX) untersucht werden.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Die Xamarin-Plattform wird mit Unterstützung für JSON standardmäßig geliefert. Mithilfe einer `JsonObject`, Ergebnisse abgerufen werden können, wie im folgenden Codebeispiel gezeigt:

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

Es ist jedoch unbedingt beachtet werden, die die `System.Json` Tools während des gesamten Entwicklungsprozesses der Daten in den Speicher geladen.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

Die [newtonsoft-JSON.NET-Bibliothek](http://www.newtonsoft.com/json) ist eine weit verbreitete Bibliothek zum Serialisieren und Deserialisieren von JSON-Nachrichten. Im folgenden Codebeispiel wird veranschaulicht, wie mit JSON.NET zum Deserialisieren einer JSON-Nachricht in eine C# Objekt:

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

ServiceStack.Text ist eine JSON-Serialisierungsbibliothek, die mit der Bibliothek ServiceStack funktionieren. Im folgenden Codebeispiel wird veranschaulicht, wie beim Analysieren von JSON-mit einem `ServiceStack.Text.JsonObject`:

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

Bei Nutzung eines XML-basierte REST-Webdiensts, LINQ to XML kann verwendet werden, um den XML-Code analysiert, und füllen Sie ein C# Inline, Objekt, wie im folgenden Codebeispiel gezeigt:

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

ASMX ermöglicht das Erstellen von Webdiensten, die Nachrichten, die mithilfe der einfachen Objekt Zugriff Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten.

Eine SOAP-Nachricht ist ein XML-Dokument mit den folgenden Elementen:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Eine optionale *Header* -Element, das anwendungsspezifische Informationen z. B. Authentifizierungsdaten zu bewegen enthält. Wenn die *Header* Element vorhanden ist. es muss sich auf das erste untergeordnete Element des der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht den Empfänger enthält.
- Eine optionale *Fehler* -Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* -Element vorhanden ist, muss es ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, einschließlich HTTP, SMTP, TCP und UDP-ausgeführt werden. Allerdings kann ein ASMX-Dienst nur über HTTP verwendet werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies schließt die Unterstützung vieler den Standardkonfigurationen der ASMX-Dienst.

### <a name="generating-a-proxy"></a>Generieren eines Proxys

Ein *Proxy* muss generiert werden, um einen ASMX-Dienst nutzen, die der Anwendung ermöglicht, mit dem Dienst herstellen. Der Proxy wird vom nutzenden Dienst-Metadaten erstellt, die die Methoden und zugeordnete Dienstkonfiguration definiert. Diese Metadaten werden als Web Services Description Language (WSDL)-Dokument verfügbar gemacht, die vom Webdienst generiert wird. Der Proxy wird mithilfe von Visual Studio für Mac oder Visual Studio zum Hinzufügen eines Webverweises für den Webdienst zu den plattformspezifischen Projekten erstellt.

Die Webdienst-URL kann entweder sein, einen gehosteten Remotequelle oder eine lokale Datei System-Ressource über die `file:///` Pfadpräfix, z.B.:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "Die Webdienst-URL kann entweder eine gehostete Remotequelle oder lokalen Dateisystemressource über das Pfadpräfix sein.")](images/add-webreference-dialog.png#lightbox)

Dadurch wird den Proxy generiert, im Ordner "Web" oder "Dienstverweise" des Projekts. Da ein Proxy generiert, wird Code sollte nicht geändert werden.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Einen Proxy hinzugefügt manuell zu einem Projekt

Wenn Sie einen vorhandenen Proxy, der mit kompatiblen-Tools generiert wurde haben, kann diese Ausgabe genutzt werden, wenn als Teil des Projekts enthalten. Verwenden Sie in Visual Studio für Mac die **Dateien hinzufügen...** Menüoption, um den Proxy hinzuzufügen. Darüber hinaus dafür *System.Web.Services.dll* verwiesen werden explizit mit der **Verweise hinzufügen...** Dialogfeld.

### <a name="consuming-the-proxy"></a>Verwenden den Proxy

Die generierten Proxyklassen bieten Methoden für die Nutzung des Webdiensts, die das asynchrone Programmiermodell (APM)-Entwurfsmuster zu verwenden. In diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Das folgende Codebeispiel zeigt ein Beispiel hierfür:

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

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `Task.Factory.FromAsync` Methode. Diese Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndGetTodoItems` -Methode einmal die `TodoService.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben wird, werden die `BeginGetTodoItems` delegieren. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM, finden Sie unter [asynchronen Programmiermodells](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zu einen ASMX-Dienst zu nutzen, finden Sie unter [nutzen eine ASP.NET Web Service (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen.

WCF wird einen Dienst mit einer Vielzahl von verschiedene Verträge, die im folgenden beschrieben:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt in einer Nachricht zu bilden.
- **Nachrichtenverträge** – Erstellen von Nachrichten von vorhandene Datenverträge.
- **Fehler Verträge** – ermöglichen von benutzerdefinierten SOAP-Fehlern angegeben werden.
- **-Dienstverträge** : Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem Verhalten benutzerdefinierter Fehler, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen, die ASMX bereitgestellt werden: SOAP-Nachrichten über HTTP unterstützt.

> [!IMPORTANT]
> Die Xamarin-Plattform-Unterstützung für WCF ist gegenüber der Verwendung von HTTP/HTTPS-textcodierten SOAP-Nachrichten auf der `BasicHttpBinding` Klasse. Unterstützung der WCF erfordert außerdem die Verwendung von Tools, die nur verfügbar, in einer Windows-Umgebung in den Proxy zu generieren.

### <a name="generating-a-proxy"></a>Generieren eines Proxys

Ein *Proxy* muss generiert werden, um einen WCF-Dienst nutzen, die der Anwendung ermöglicht, mit dem Dienst herstellen. Der Proxy wird erstellt, indem verarbeitende Dienstmetadaten, die die Methoden und zugeordnete Dienstkonfiguration zu definieren. Diese Metadaten werden in die Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die vom Webdienst generiert wird. Hinzufügen ein Dienstverweises für den Webdienst in einer .NET Standard-Bibliothek mit den Microsoft WCF Web Service Reference Provider in Visual Studio 2017, kann der Proxy erstellt werden.

Eine Alternative zum Erstellen des Proxys verwenden den Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Konfigurieren des Anwendungsproxys

Konfigurieren den generierten Proxy gelangen Sie in der Regel zwei konfigurationsargumente (je nach SOAP 1.1/ASMX oder WCF) während der Initialisierung: der `EndpointAddress` und/oder die zugehörigen Bindungsinformationen, wie im folgenden Beispiel gezeigt:

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

### <a name="consuming-the-proxy"></a>Verwenden den Proxy

Die generierten Proxyklassen bieten Methoden für die Nutzung der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. Bei diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Das folgende Codebeispiel zeigt ein Beispiel hierfür:

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

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `Task.Factory.FromAsync` Methode. Diese Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndGetTodoItems` -Methode einmal die `TodoServiceClient.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben wird, werden die `BeginGetTodoItems` delegieren. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM, finden Sie unter [asynchronen Programmiermodells](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zum Verarbeiten eines WCF-Diensts, finden Sie unter [Nutzung eines Windows Communication Foundation (WCF)-Webdiensts](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Unter Berücksichtigung der Transportsicherheit

WCF-Dienste kann Sicherheit auf Transportebene zum Schutz vor Abfangen von Nachrichten einsetzen. Die Xamarin-Plattform unterstützt Bindungen, die Sicherheit der Verwendung von SSL auf Transportebene verwenden. Allerdings gibt es möglicherweise Fälle, in denen der Stapel möglicherweise, zum Überprüfen des Zertifikats, was zu unerwarteten Verhalten führt. Die Überprüfung kann überschrieben werden, durch die Registrierung einer `ServerCertificateValidationCallback` Delegaten vor dem Aufrufen von des Diensts, wie im folgenden Codebeispiel veranschaulicht:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Dadurch wird die Verschlüsselung auf Transportebene und ignoriert die zertifikatsüberprüfung für serverseitige beibehalten. Dieser Ansatz wird jedoch effektiv ignoriert die Trust-Probleme, die mit dem Zertifikat verknüpft ist, und der eventuell nicht möglich. Weitere Informationen finden Sie unter [mithilfe von vertrauenswürdigen Stämme mit freundlichen Grüßen](https://www.mono-project.com/UsingTrustedRootsRespectfully) auf [Mono-project.com](https://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>Verwenden die Client-Anmeldeinformationen-Sicherheit

WCF-Dienste können auch die Dienstclients für die Authentifizierung mit Anmeldeinformationen erfordern. Die Xamarin-Plattform unterstützt nicht das Protokoll WS-Security, Clients Anmeldeinformationen innerhalb des SOAP-Nachrichtenumschlags senden können. Die Xamarin-Plattform unterstützt jedoch die Möglichkeit zum Senden von HTTP-Standardauthentifizierung-Anmeldeinformationen an den Server durch Angabe der entsprechenden `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Anschließend können standardauthentifizierungs-Anmeldeinformationen angegeben werden:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

Im obigen Beispiel, wenn Sie die Nachricht erhalten "Ran aus Trampolines vom Typ 0" Sie können die Anzahl erhöhen des Typ 0 Trampolines durch Hinzufügen der `–aot “trampolines={number of trampolines}”` Argument für den Build. Weitere Informationen finden Sie unter [Problembehandlung](~/ios/troubleshooting/troubleshooting.md#trampolines).

Weitere Informationen zu HTTP-Standardauthentifizierung zwar im Kontext eines REST-Webdiensts finden Sie [authentifizieren einen RESTful Webdienst](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="related-links"></a>Verwandte Links

- [Webdienste in Xamarin.Forms](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
