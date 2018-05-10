---
title: Einführung in Webdienste
description: Dieses Handbuch veranschaulicht, wie verschiedene Web Service-Technologien zu nutzen. Zu den behandelten Themen gehören bei der Kommunikation mit REST-Dienste, die SOAP-Dienste und die Windows Communication Foundation-Diensten.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 5c07443519391192f7bdb62cc19ff8b1f87b2558
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-web-services"></a>Einführung in Webdienste

_Dieses Handbuch veranschaulicht, wie verschiedene Web Service-Technologien zu nutzen. Zu den behandelten Themen gehören bei der Kommunikation mit REST-Dienste, die SOAP-Dienste und die Windows Communication Foundation-Diensten._

Um korrekt zu funktionieren, sind viele mobile Anwendungen in der Cloud abhängig, und so die Integration von Web Services in mobilen Anwendungen ist ein gängiges Szenario. Die Xamarin-Plattform unterstützt den Verbrauch von anderen Web Service Technologien zum Einsatz und enthält integrierte und Drittanbieter-Unterstützung für die Nutzung RESTful, ASMX- und Windows Communication Foundation (WCF)-Diensten.

In diesem Artikel werden die folgenden Themen erläutert:

- [REST-Dienste](#rest)
- [ASP.NET-Webdienste (ASMX)](#asmx)
- [WCF-Dienste](#wcf)

Für Kunden, indem Sie xamarin.Forms verwenden, stehen die vollständige Beispiele, die mithilfe dieser Technologien in der [Xamarin.Forms Webdienste](~/xamarin-forms/data-cloud/index.md) Dokumentation.

> [!IMPORTANT]
> In iOS 9 erzwingt App Transport Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern.
> Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.

Sie können opt-Out-of ATS ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).



<a name="rest" />

## <a name="rest"></a>REST

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

Die Einfachheit von REST hat dazu beigetragen, erleichtern die Hauptmethode für den Zugriff auf Webdienste in mobilen Anwendungen.

## <a name="consuming-rest-services"></a>Nutzung von REST-Diensten

Es gibt eine Reihe von Bibliotheken und Klassen, die zum Verarbeiten von REST-Diensten verwendet werden können, und den folgenden Unterabschnitten werden diese. Weitere Informationen zu REST-Dienst nutzen, finden Sie unter [RESTful Webdiensten](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

Die [Microsoft HTTP Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http) bietet die `HttpClient` -Klasse, die zum Senden und Empfangen von Anforderungen über HTTP verwendet wird. Sie bietet Funktionen für die sendende HTTP-Anforderungen und Empfangen von HTTP-Antworten aus einer URI identifizierte Ressource. Jede Anforderung wird als asynchronen Vorgang gesendet. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Überblick über den asynchronen](~/cross-platform/platform/async.md).

Die `HttpResponseMessage` Klasse stellt eine HTTP-Antwortnachricht vom Webdienst empfangen werden, nachdem eine HTTP-Anforderung gestellt wurde. Es enthält Informationen über die Antwort, einschließlich der Statuscode, Header und Text. Die `HttpContent` Klasse stellt die HTTP-Texts und der bereitgestellten Inhaltsheader wie z. B. `Content-Type` und `Content-Encoding`. Der Inhalt kann einer beliebigen, von gelesen werden die `ReadAs` Methoden, wie z. B. `ReadAsStringAsync` und `ReadAsByteArrayAsync`je nach dem Format der Daten.

Weitere Informationen zu den `HttpClient` Klasse, finden Sie unter [beim Erstellen des Objekts "HttpClient"](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Aufrufen von Webdiensten mit `HTTPWebRequest` umfasst:

-  Erstellen die Anforderungsinstanz für einen bestimmten URI.
-  Verschiedene HTTP-Eigenschaften festlegen für die Anforderungsinstanz.
-  Abrufen einer `HttpWebResponse` aus der Anforderung.
-  Lesen von Daten aus der Antwort.

Der folgende Code ruft z. B. Daten aus den USA National Bibliothek der Humanmedizin-Webdienst:

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

Im obigen Beispiel erstellt eine `HttpWebRequest` , die als JSON formatierte Daten zurück. Die Daten werden zurückgegeben, eine `HttpWebResponse`, aus dem ein `StreamReader` zum Lesen der Daten abgerufen werden kann.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Ein weiteres Verfahren zum Nutzen von REST-Diensten wird mithilfe der [RestSharp](http://restsharp.org/) Bibliothek. RestSharp kapselt HTTP-Anforderungen, einschließlich der Unterstützung für das Abrufen von Ergebnissen als Inhalt für unformatierte Zeichenfolge oder als deserialisierte c#-Objekt. Der folgende Code sendet beispielsweise eine Anforderung in den USA Nationale Bibliothek der Humanmedizin-Webdienst und ruft die Ergebnisse als JSON-formatierte Zeichenfolge:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` ist eine Methode, mit denen die unformatierte JSON-Zeichenfolge aus gelangen die `RestSharp.RestResponse.Content` Eigenschaft und konvertieren Sie ihn in ein c#-Objekt. Deserialisierung von Daten von Webdiensten zurückgegeben wird weiter unten in diesem Artikel erläutert.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Zusätzlich zu den Klassen, die in der Mono / Base verfügbar: Klassenbibliothek (BCL), wie z. B. `HttpWebRequest`, und von Drittanbietern C#-Bibliotheken, z. B. RestSharp, Clientplattform-spezifische Klassen sind außerdem zur Nutzung von Webdiensten zur Verfügung. In iOS, beispielsweise die `NSUrlConnection` und `NSMutableUrlRequest` Klassen verwendet werden können.

Im folgenden Codebeispiel wird veranschaulicht, wie die USA aufrufen National Bibliothek der Humanmedizin-Webdienst mithilfe von iOS-Klassen:

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

Im Allgemeinen sollten Clientplattform-spezifische Klassen für die Nutzung von Webdiensten auf Szenarien beschränkt werden, in dem systemeigenen Code in c# portiert wird. Nach Möglichkeit sollten Webdienstcode Zugriff portabel sein, damit es über Plattformen hinweg gemeinsam genutzten sein kann.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Eine weitere Option zum Aufrufen von Webdiensten ist die [Dienststapel](http://www.servicestack.net/) Bibliothek. Der folgende Code zeigt z. B. zum Verwenden des Dienst-Stapel `IServiceClient.GetAsync` Methode, um eine Serviceanfrage ausgeben:

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
> Während Tools wie z. B. ServiceStack und RestSharp erleichtern aufrufen und nutzen Dienste bewegen, manchmal ist es nicht triviale nutzen, XML oder JSON, der nicht dem Standard entspricht _DataContract_ Serialisierung Konventionen. Falls erforderlich, rufen Sie die Anforderung, und behandeln Sie die entsprechenden Serialisierung explizit mithilfe der ServiceStack.Text-Bibliothek, die weiter unten erläutert.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Nutzen von RESTful-Daten

Rest-Webdienste verwenden JSON-Nachrichten in der Regel Daten an den Client zurückgegeben. JSON ist eine textbasierte, Datenaustauschformat, das erzeugt Nutzlasten und komprimieren vortäuschen reduzierter Bandbreitenbedarf beim Senden von Daten. In diesem Abschnitt werden die Mechanismen für die Nutzung von RESTful-Antworten in JSON und Plain-Old-XML (POX) untersucht werden.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Im Lieferumfang von der Xamarin-Plattform sind Unterstützung für JSON ausgegeben. Mithilfe einer `JsonObject`, Ergebnisse abgerufen werden können, wie im folgenden Codebeispiel gezeigt:

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

Allerdings ist es wichtig, sich bewusst sein, die die `System.Json` Tools die Gesamtheit der Daten in den Speicher geladen.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

Die [NewtonSoft JSON.NET Bibliothek](http://www.newtonsoft.com/json) ist eine weit verbreitete Bibliothek zum Serialisieren und Deserialisieren von JSON-Nachrichten. Im folgenden Codebeispiel wird gezeigt, wie JSON.NET zum Deserialisieren einer JSON-Nachricht in eine c#-Objekt verwendet wird:

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

ServiceStack.Text ist eine JSON-Serialisierung-Bibliothek dienen zum Arbeiten mit der ServiceStack-Bibliothek. Im folgenden Codebeispiel wird veranschaulicht, wie beim Analysieren von JSON mit einem `ServiceStack.Text.JsonObject`:

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

Im Falle einen XML-basierte REST-Webdienst in Anspruch, kann LINQ to XML zum Analysieren der XML-Codes, und füllen Sie eine C#-Inline-Objekt, verwendet werden, wie im folgenden Codebeispiel gezeigt:

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

## <a name="aspnet-web-service-asmx"></a>ASP.NET Web Service (ASMX)

ASMX bietet die Möglichkeit, Webdienste erstellen, die Nachrichten, die mit einfachen Objekt Access Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll zum Erstellen von und Zugreifen auf Webdienste. Consumer von ASMX-Dienst müssen nicht alles Plattform, das Objektmodell oder Programmiersprache ab, die zum Implementieren des Diensts kennen. Sie müssen verstehen, wie SOAP-Nachrichten senden und empfangen.

Eine SOAP-Nachricht ist ein XML-Dokument enthält die folgenden Elemente:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Ein optionales *Header* Element, das anwendungsspezifische Informationen wie z. B. Authentifizierungsdaten enthält. Wenn die *Header* -Element vorhanden ist muss er das erste untergeordnete Element von der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht, die der Empfänger enthält.
- Ein optionales *Fehler* Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* Element vorhanden ist, muss er ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, z. B. HTTP, SMTP, TCP und UDP ausgeführt werden. Ein ASMX-Dienst kann jedoch nur über HTTP ausgeführt werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies umfasst Unterstützung für viele den Standardkonfigurationen der ASMX-Dienst.

### <a name="generating-a-proxy"></a>Generieren eines Proxys

Ein *Proxy* muss generiert werden, um einen ASMX-Dienst zu nutzen, die die Anwendung für die Verbindung mit dem Dienst ermöglicht. Der Proxy wird vom verbrauchende Dienstmetadaten erstellt, die die Methoden und die zugehörigen Dienstkonfiguration definiert. Diese Metadaten wird als ein Dokument Web Services Description Language (WSDL) verfügbar gemacht, die durch den Webdienst generiert wird. Der Proxy wird erstellt, mithilfe von Visual Studio für Mac oder Visual Studio einen Webverweis für den Webdienst zu den plattformspezifischen Projekten hinzufügen.

Die Webdienst-URL oder sein. eine gehostete Remotequelle Systemressource für lokale Datei zugegriffen werden kann, über die `file:///` Pfadpräfix, z. B.:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "Die Webdienst-URL kann entweder eine gehostete Remotequelle oder Remotedatei Systemressource-Zugriff über das Pfadpräfix sein.")](images/add-webreference-dialog.png#lightbox)

Dadurch wird den Proxy im Web oder Verweise des Projekts generiert. Seit dem Generieren eines Proxys Code sollte nicht geändert werden.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Manuelles Hinzufügen eines Proxys zu einem Projekt

Wenn Sie einen vorhandenen Proxy, der über kompatible Tools generiert wurde haben, kann diese Ausgabe genutzt werden, wenn als Teil des Projekts enthalten. In Visual Studio für Mac verwenden die **Dateien hinzufügen...** Menüoption, um den Proxy hinzuzufügen. Darüber hinaus erfordert dies *System.Web.Services.dll* verwiesen wird, explizit mit der **Verweise hinzufügen...** Dialogfeld.

### <a name="consuming-the-proxy"></a>Nutzen den Proxy

Die generierte Proxyklassen bieten Methoden für den Webdienst, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. In diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang, und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread stattfindet.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* um die Ergebnisse des Vorgangs abzurufen. Der Rückgabewert der *EndOperationName* ist der gleiche Typ von synchronen Webdienstmethode zurückgegeben. Das folgende Codebeispiel zeigt ein Beispiel dafür:

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

Die Task Parallel Library (TPL) kann vereinfachen das nutzen ein APM begin/End-Methodenpaar von kapseln die asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung von mehrere Überladungen der dient den `Task.Factory.FromAsync` Methode. Diese Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndGetTodoItems` -Methode einmal die `TodoService.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben werden die `BeginGetTodoItems` delegieren. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM, finden Sie unter [asynchronen Programmierungsmodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zum Verarbeiten einer ASMX-Diensts finden Sie unter [Nutzen einer ASP.NET Web Service (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Es ermöglicht Entwicklern, sichere, zuverlässige, transaktive und interoperable verteilte Anwendungen zu erstellen.

WCF beschreibt einen Dienst mit einer Vielzahl von verschiedene Verträge, darunter die folgenden:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt auf eine Nachricht zu bilden.
- **Nachrichtenverträge** – Verfassen Sie Nachrichten von vorhandene Datenverträge.
- **Verträge Fault** – benutzerdefinierte SOAP-Fehler angegeben werden können.
- **-Dienstverträgen** – Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem alle benutzerdefinierten Fehler Verhalten, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen, die ASMX bereitstellt – SOAP-Nachrichten über HTTP unterstützt.

Im Allgemeinen unterstützt die Xamarin-Plattform die gleiche clientseitige Teilmenge von WCF, die mit der Silverlight-Laufzeit bereitgestellt wird. Dies schließt die am häufigsten verwendeten Codierungs- und Protokolldetails Implementierungen von WCF-Text-codierte SOAP-Nachrichten über den HTTP-transport-Protokoll der `BasicHttpBinding` Klasse. WCF-Unterstützung erfordert außerdem die Verwendung von Tools nur in einer Windows-Umgebung für den Proxy zu generieren.

Weitere Informationen zur Verwendung von der Xamarin-Plattform für die Verarbeitung eines WCF-web-Dienst mit der `BasicHttpBinding` Klasse, finden Sie unter [Exemplarische Vorgehensweise – arbeiten mit WCF](walkthrough-working-with-wcf.md).

### <a name="generating-a-proxy"></a>Generieren eines Proxys

Ein *Proxy* muss generiert werden, um einen WCF-Dienst zu nutzen, die die Anwendung für die Verbindung mit dem Dienst ermöglicht. Der Proxy erstellt wird, nutzen Dienstmetadaten, die die Methoden und die zugehörigen Dienstkonfiguration zu definieren. Diese Metadaten wird in Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die durch den Webdienst generiert wird. Der Proxy kann mithilfe von Microsoft WCF Web Service-Reference-Anbieter in Visual Studio 2017 zum Hinzufügen eines Dienstverweises für den Webdienst eine Standardbibliothek des .NET erstellt werden.

Eine Alternative zum Erstellen des Proxys, die mit dem Microsoft WCF Web Verweis Dienstanbieter in Visual Studio 2017 wird das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Konfigurieren des Proxys

Konfigurieren den generierten Proxy wird im Allgemeinen nehmen zwei Konfiguration Argumente (je nach SOAP 1.1/ASMX oder WCF) während der Initialisierung: der `EndpointAddress` und/oder die zugehörigen Bindungsinformationen, wie im folgenden Beispiel gezeigt:

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

Eine Bindung an die Transport-, Codierungs- und Protokolldetails erforderlich für Anwendungen und Dienste zur Kommunikation mit anderen verwendet. Die `BasicHttpBinding` gibt an, dass der Text-codierte SOAP-Nachrichten über das HTTP-Transportprotokoll gesendet werden. Angeben einer Endpunktadresse ermöglicht der Anwendung eine Verbindung zu verschiedenen Instanzen der den WCF-Dienst herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

### <a name="consuming-the-proxy"></a>Nutzen den Proxy

Die generierte Proxyklassen enthalten Methoden zum Verarbeiten der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. Bei diesem Muster wird ein asynchroner Vorgang als zwei Methoden namens implementiert *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang, und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread stattfindet.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* um die Ergebnisse des Vorgangs abzurufen. Der Rückgabewert der *EndOperationName* ist der gleiche Typ von synchronen Webdienstmethode zurückgegeben. Das folgende Codebeispiel zeigt ein Beispiel dafür:

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

Die Task Parallel Library (TPL) kann vereinfachen das nutzen ein APM begin/End-Methodenpaar von kapseln die asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung von mehrere Überladungen der dient den `Task.Factory.FromAsync` Methode. Diese Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndGetTodoItems` -Methode einmal die `TodoServiceClient.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben werden die `BeginGetTodoItems` delegieren. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Weitere Informationen zu APM, finden Sie unter [asynchronen Programmierungsmodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

Weitere Informationen zum Verarbeiten einer WCF-Diensts finden Sie unter [Nutzung eines Windows Communication Foundation (WCF)-Webdiensts](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Mithilfe der Transportsicherheit

WCF-Dienste kann Sicherheit auf Transportebene zum Schutz vor Abfangen von Nachrichten verwenden. Die Xamarin-Plattform unterstützt Bindungen, die Sicherheit der Verwendung von SSL auf Transportebene verwenden. Aber es gibt möglicherweise Fälle, in denen der Stapel möglicherweise Überprüfung des Zertifikats, was zu einem unerwarteten Verhalten führt. Die Überprüfung kann überschrieben werden, durch die Registrierung einer `ServerCertificateValidationCallback` Delegaten vor dem Aufrufen des Diensts, wie im folgenden Codebeispiel gezeigt:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Damit wird die Transport-Verschlüsselung beim wird ignoriert, die serverseitige zertifikatsüberprüfung. Dieser Ansatz wird allerdings effektiv ignoriert die Vertrauensstellung Sicherheitsproblemen im Zusammenhang mit dem Zertifikat, und der möglicherweise nicht geeignet. Weitere Informationen finden Sie unter [verwenden vertrauenswürdiger Stämme mit freundlichen Grüßen](http://www.mono-project.com/UsingTrustedRootsRespectfully) auf [Mono-project.com](http://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>Mithilfe der Client Anmeldeinformationen-Sicherheit

WCF-Dienste erfordern u. u. zudem die Dienstclients zur Authentifizierung mithilfe von Anmeldeinformationen. Das WS-Security-Protokoll, unterstützt die Clients zum Senden von Anmeldeinformationen innerhalb des SOAP-Nachrichtenumschlags ermöglicht die Xamarin-Plattform nicht. Die Xamarin-Plattform unterstützt jedoch die Möglichkeit zum Senden von HTTP-Standardauthentifizierung Anmeldeinformationen an den Server durch Angabe der entsprechenden `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Anschließend können die Anmeldeinformationen für Standardauthentifizierung angegeben werden:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

Im obigen Beispiel, wenn Sie die Nachricht erhalten "Ran Out Trampolines vom Typ 0" Sie erhöht die Anzahl von Typ 0 Trampolines durch Hinzufügen der `–aot “trampolines={number of trampolines}”` Argument für den Build. Weitere Informationen finden Sie unter [Problembehandlung](~/ios/troubleshooting/troubleshooting.md#trampolines).

Weitere Informationen zu HTTP-Standardauthentifizierung, obwohl im Kontext eines REST-Webdiensts, finden Sie [eine RESTful-Webdienst authentifiziert](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wurde veranschaulicht, wie verschiedene Web Service-Technologien zu nutzen. Zu den behandelten Themen gehören bei der Kommunikation mit REST-Dienste, die SOAP-Dienste und die Windows Communication Foundation-Diensten.

## <a name="related-links"></a>Verwandte Links

- [WebServices-Beispiel](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Webdienste in Xamarin.Forms](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
