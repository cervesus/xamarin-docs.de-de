---
title: Einführung in Webdienste
description: In dieser Anleitung wird veranschaulicht, wie verschiedene Webdienst Technologien genutzt werden. Zu den behandelten Themen gehören die Kommunikation mit Rest-Diensten, SOAP-Diensten und Windows Communication Foundation-Diensten.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 50302b0b9cf96d211c704ab9e68d1c61d11e807a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016572"
---
# <a name="introduction-to-web-services"></a>Einführung in Webdienste

_In dieser Anleitung wird veranschaulicht, wie verschiedene Webdienst Technologien genutzt werden. Zu den behandelten Themen gehören die Kommunikation mit Rest-Diensten, SOAP-Diensten und Windows Communication Foundation-Diensten._

Um ordnungsgemäß zu funktionieren, sind viele mobile Anwendungen von der Cloud abhängig, sodass das Integrieren von Webdiensten in Mobile Anwendungen ein gängiges Szenario ist. Die xamarin-Plattform unterstützt die Nutzung verschiedener Webdienst Technologien und umfasst integrierte und Drittanbieter Unterstützung für die Nutzung von Rest-, asmx-und Windows Communication Foundation (WCF)-Diensten.

Für Kunden, die xamarin. Forms verwenden, gibt es vollständige Beispiele für die Verwendung dieser Technologien in der Dokumentation zu [xamarin. Forms-Webdiensten](~/xamarin-forms/data-cloud/index.yml) .

> [!IMPORTANT]
> In ios 9 erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird.
> Da ATS in apps, die für IOS 9 erstellt wurden, standardmäßig aktiviert ist, unterliegen alle Verbindungen den Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.

Sie können sich entscheiden, ob es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="rest"></a>REST

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

Die Einfachheit von Rest hat dazu beigetragen, dass Sie die primäre Methode für den Zugriff auf Webdienste in mobilen Anwendungen ist.

## <a name="consuming-rest-services"></a>Verwenden von Rest-Diensten

Es gibt eine Reihe von Bibliotheken und Klassen, die verwendet werden können, um Rest-Dienste zu nutzen. in den folgenden Unterabschnitten werden diese erläutert. Weitere Informationen zur Nutzung eines REST-Diensts finden Sie unter verwenden [eines](~/xamarin-forms/data-cloud/web-services/rest.md)Rest-Diensts.

### <a name="httpclient"></a>HttpClient

Die [Microsoft HTTP-Client Bibliotheken](https://www.nuget.org/packages/Microsoft.Net.Http) stellen die `HttpClient`-Klasse bereit, die zum Senden und empfangen von Anforderungen über HTTP verwendet wird. Sie stellt Funktionen zum Senden von HTTP-Anforderungen und empfangen von HTTP-Antworten aus einer durch URI identifizierten Ressource bereit. Jede Anforderung wird als asynchroner Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie [unter async-Unterstützung: Übersicht](~/cross-platform/platform/async.md).

Die `HttpResponseMessage`-Klasse stellt eine HTTP-Antwortnachricht dar, die vom Webdienst empfangen wurde, nachdem eine HTTP-Anforderung durchgeführt wurde. Sie enthält Informationen über die Antwort, einschließlich Statuscode, Header und Text. Die `HttpContent`-Klasse stellt den HTTP-Textkörper und Inhalts Header dar, z. b. `Content-Type` und `Content-Encoding`. Der Inhalt kann je nach Format der Daten mit allen `ReadAs` Methoden wie `ReadAsStringAsync` und `ReadAsByteArrayAsync`gelesen werden.

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

Eine weitere Option zum Aufrufen von Webdiensten ist die [Dienst Stapel](https://www.servicestack.net/) Bibliothek. Der folgende Code zeigt beispielsweise, wie die `IServiceClient.GetAsync` Methode des Dienst Stapels verwendet wird, um eine Service Request auszugeben:

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

Rest-Webdienste verwenden normalerweise JSON-Nachrichten, um Daten an den Client zurückzugeben. JSON ist ein textbasiertes Datenaustauschformat, das kompakte Nutzlasten erzeugt, was zu geringeren Bandbreitenanforderungen beim Senden von Daten führt. In diesem Abschnitt werden Mechanismen für die Verwendung von Rest-Antworten in JSON und Plain-Old-XML (POX) untersucht.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System. JSON

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

### <a name="servicestacktext"></a>Servicestack. Text

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

ASMX bietet die Möglichkeit, Webdienste zu erstellen, die Nachrichten mithilfe von SOAP (Simple Object Access Protocol) senden. SOAP ist ein plattformunabhängiges und sprachunabhängiges Protokoll zum entwickeln und Zugreifen auf Webdienste. Consumer eines ASMX-Dienstanbieter müssen nichts über die Plattform, das Objektmodell oder die Programmiersprache wissen, die zur Implementierung des Dienstanbieter verwendet werden. Sie müssen nur wissen, wie SOAP-Nachrichten gesendet und empfangen werden.

Eine SOAP-Nachricht ist ein XML-Dokument, das die folgenden Elemente enthält:

- Ein root-Element mit dem Namen *Envelope* , das das XML-Dokument als SOAP-Nachricht identifiziert.
- Ein optionales *Header* -Element, das anwendungsspezifische Informationen wie z. b. Authentifizierungsdaten enthält. Wenn das *Header* -Element vorhanden ist, muss es das erste untergeordnete Element des *Envelope* -Elements sein.
- Ein erforderliches *Body* -Element, das die für den Empfänger vorgesehene SOAP-Nachricht enthält.
- Ein optionales *Fault* -Element, das zum Angeben von Fehlermeldungen verwendet wird. Wenn das *Fault* -Element vorhanden ist, muss es sich um ein untergeordnetes Element des *Body* -Elements handeln.

SOAP kann über viele Transportprotokolle, einschließlich http, SMTP, TCP und UDP, betrieben werden. Ein ASMX-Dienst kann jedoch nur über HTTP verwendet werden. Die xamarin-Plattform unterstützt standardmäßige SOAP 1,1-Implementierungen über HTTP und bietet Unterstützung für viele der standardmäßigen ASMX-Dienst Konfigurationen.

### <a name="generating-a-proxy"></a>Erstellen eines Proxys

Ein *Proxy* muss generiert werden, um einen ASMX-Dienst zu nutzen, der der Anwendung das Herstellen einer Verbindung mit dem Dienst ermöglicht. Der Proxy wird erstellt, indem Dienst Metadaten genutzt werden, die die Methoden und die zugehörige Dienst Konfiguration definieren. Diese Metadaten werden als Web Services Description Language-Dokument (WSDL) verfügbar gemacht, das vom Webdienst generiert wird. Der Proxy wird erstellt, indem Visual Studio für Mac oder Visual Studio verwendet wird, um den plattformspezifischen Projekten einen Webverweis für den Webdienst hinzuzufügen.

Die Webdienst-URL kann entweder eine gehostete Remote Quelle oder eine lokale Dateisystem Ressource sein, auf die über das Präfix `file:///` Pfad zugegriffen werden kann. Beispiel:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "The web service URL can either be a hosted remote source or local file system resource accessible via the file path prefix")](images/add-webreference-dialog.png#lightbox)

Dadurch wird der Proxy im Web-oder Dienst Verweis Ordner des Projekts generiert. Da es sich bei einem Proxy um generierten Code handelt, sollte er nicht geändert werden.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Manuelles Hinzufügen eines Proxys zu einem Projekt

Wenn Sie über einen vorhandenen Proxy verfügen, der mit kompatiblen Tools generiert wurde, kann diese Ausgabe genutzt werden, wenn Sie als Teil des Projekts enthalten ist. Verwenden Sie in Visual Studio für Mac die **Dateien hinzufügen...** Menüoption zum Hinzufügen des Proxys. Außerdem erfordert dies die explizite referenzierte Verwendung von " *System. Web. Services. dll* " mithilfe der **Add-Verweise...** Dialog.

### <a name="consuming-the-proxy"></a>Verwenden des Proxys

Die generierten Proxy Klassen bieten Methoden zum Verarbeiten des Webdiensts, der das APM-Entwurfsmuster (Asynchronous Programming Model) verwendet. In diesem Muster wird ein asynchroner Vorgang als zwei Methoden namens *BeginOperationName* und *EndOperationName*implementiert, die den asynchronen Vorgang starten und beenden.

Die *BeginOperationName* -Methode startet den asynchronen Vorgang und gibt ein Objekt zurück, das die `IAsyncResult`-Schnittstelle implementiert. Nach dem Aufrufen von *BeginOperationName*kann eine Anwendung die Ausführung von Anweisungen für den aufrufenden Thread fortsetzen, während der asynchrone Vorgang in einem Thread Pool Thread erfolgt.

Für jeden Aufrufen von *BeginOperationName*sollte die Anwendung auch *EndOperationName* aufrufen, um die Ergebnisse des Vorgangs zu erhalten. Der Rückgabewert von *EndOperationName* ist derselbe Typ, der von der synchronen Webdienst Methode zurückgegeben wird. Das folgende Codebeispiel veranschaulicht dieses Verhalten:

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

Der Task Parallel Library (TPL) kann den Prozess der Nutzung eines apm-Begin/End-Methoden Paars vereinfachen, indem die asynchronen Vorgänge im gleichen `Task` Objekt gekapselt werden. Diese Kapselung wird von mehreren über Ladungen der `Task.Factory.FromAsync`-Methode bereitgestellt. Diese Methode erstellt eine `Task`, die die `TodoService.EndGetTodoItems`-Methode ausführt, sobald die `TodoService.BeginGetTodoItems`-Methode abgeschlossen ist. der `null`-Parameter gibt an, dass keine Daten an den `BeginGetTodoItems` Delegaten übergeben werden. Schließlich gibt der Wert der `TaskCreationOptions`-Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM finden Sie unter [asynchrones Programmiermodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zum Verwenden eines ASMX-Diensts finden Sie unter verwenden [eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF ist das vereinheitlichte Framework von Microsoft zum entwickeln Dienst orientierter Anwendungen. Es ermöglicht Entwicklern das Erstellen sicherer, zuverlässiger, transaktiver und interoperable verteilter Anwendungen.

WCF beschreibt einen Dienst mit einer Vielzahl unterschiedlicher Verträge, einschließlich der folgenden:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt innerhalb einer Nachricht bilden.
- **Nachrichten Verträge** – Verfassen von Nachrichten aus vorhandenen Daten Verträgen.
- **Fehler Verträge** – zulassen, dass benutzerdefinierte SOAP-Fehler angegeben werden.
- **Dienstverträge** – geben Sie die Vorgänge an, die von Diensten unterstützt werden, sowie die für die Interaktion mit den einzelnen Vorgängen erforderlichen Außerdem geben Sie ein benutzerdefiniertes Fehler Verhalten an, das den Vorgängen in jedem Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET-Webdiensten (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF dieselben Funktionen unterstützt, die ASMX – SOAP-Nachrichten über HTTP bereitstellt.

> [!IMPORTANT]
> Die xamarin-Platt Form Unterstützung für WCF ist auf Text codierte SOAP-Nachrichten über HTTP/HTTPS mit der `BasicHttpBinding`-Klasse beschränkt. Außerdem erfordert die WCF-Unterstützung die Verwendung von Tools, die nur in einer Windows-Umgebung zur Generierung des Proxys verfügbar sind.

### <a name="generating-a-proxy"></a>Erstellen eines Proxys

Ein *Proxy* muss generiert werden, um einen WCF-Dienst zu nutzen, mit dem die Anwendung eine Verbindung mit dem Dienst herstellen kann. Der Proxy wird erstellt, indem Dienst Metadaten genutzt werden, die die Methoden und die zugehörige Dienst Konfiguration definieren. Diese Metadaten werden in Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, das vom Webdienst generiert wird. Der Proxy kann erstellt werden, indem der Microsoft WCF Web Service Reference Provider in Visual Studio 2017 verwendet wird, um einer .NET Standard Bibliothek einen Dienst Verweis für den Webdienst hinzuzufügen.

Eine Alternative zum Erstellen des Proxys mithilfe des Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist die Verwendung des Service Model Metadata Utility Tool (Svcutil. exe). Weitere Informationen finden Sie unter [Service Model Metadata Utility-Tool (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

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

Eine Bindung wird verwendet, um die Transport-, Codierungs-und Protokoll Details anzugeben, die für die Kommunikation zwischen Anwendungen und Diensten erforderlich sind. Der `BasicHttpBinding` gibt an, dass Text codierte SOAP-Nachrichten über das HTTP-Transportprotokoll gesendet werden. Durch das Angeben einer Endpunkt Adresse kann die Anwendung eine Verbindung mit verschiedenen Instanzen des WCF-Dienstanbieter herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

### <a name="consuming-the-proxy"></a>Verwenden des Proxys

Die generierten Proxy Klassen stellen Methoden zum Verarbeiten der Webdienste bereit, die das APM-Entwurfsmuster (Asynchronous Programming Model) verwenden. In diesem Muster wird ein asynchroner Vorgang als zwei Methoden namens *BeginOperationName* und *EndOperationName*implementiert, die den asynchronen Vorgang starten und beenden.

Die *BeginOperationName* -Methode startet den asynchronen Vorgang und gibt ein Objekt zurück, das die `IAsyncResult`-Schnittstelle implementiert. Nach dem Aufrufen von *BeginOperationName*kann eine Anwendung die Ausführung von Anweisungen für den aufrufenden Thread fortsetzen, während der asynchrone Vorgang in einem Thread Pool Thread erfolgt.

Für jeden Aufrufen von *BeginOperationName*sollte die Anwendung auch *EndOperationName* aufrufen, um die Ergebnisse des Vorgangs zu erhalten. Der Rückgabewert von *EndOperationName* ist derselbe Typ, der von der synchronen Webdienst Methode zurückgegeben wird. Das folgende Codebeispiel veranschaulicht dieses Verhalten:

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

Der Task Parallel Library (TPL) kann den Prozess der Nutzung eines apm-Begin/End-Methoden Paars vereinfachen, indem die asynchronen Vorgänge im gleichen `Task` Objekt gekapselt werden. Diese Kapselung wird von mehreren über Ladungen der `Task.Factory.FromAsync`-Methode bereitgestellt. Diese Methode erstellt eine `Task`, die die `TodoServiceClient.EndGetTodoItems`-Methode ausführt, sobald die `TodoServiceClient.BeginGetTodoItems`-Methode abgeschlossen ist. der `null`-Parameter gibt an, dass keine Daten an den `BeginGetTodoItems` Delegaten übergeben werden. Schließlich gibt der Wert der `TaskCreationOptions`-Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

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

## <a name="related-links"></a>Verwandte Links

- [Webdienste in xamarin. Forms](~/xamarin-forms/data-cloud/index.yml)
- [Service Model Metadata Utility-Tool (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
