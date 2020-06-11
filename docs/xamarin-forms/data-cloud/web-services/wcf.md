---
Title: "Verwenden eines Windows Communication Foundation (WCF)-Webdiensts" Beschreibung: "in diesem Artikel wird veranschaulicht, wie ein WCF Simple Object Access Protocol (SOAP)-Dienst aus einer-Anwendung verwendet wird Xamarin.Forms ."
ms. Prod: xamarin ms. assetid: 5696ff04-ef21-4b7a-8c8b-26de28b5c0ad ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 03/28/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Verwenden eines Windows Communication Foundation (WCF)-Webdiensts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF ist das vereinheitlichte Framework von Microsoft zum entwickeln Dienst orientierter Anwendungen. Es ermöglicht Entwicklern das Erstellen sicherer, zuverlässiger, transaktiver und interoperable verteilter Anwendungen. In diesem Artikel wird veranschaulicht, wie ein WCF Simple Object Access Protocol (SOAP)-Dienst aus einer-Anwendung verwendet wird Xamarin.Forms ._

WCF beschreibt einen Dienst mit einer Vielzahl verschiedener Verträge, einschließlich:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt innerhalb einer Nachricht bilden.
- **Nachrichten Verträge** – Verfassen von Nachrichten aus vorhandenen Daten Verträgen.
- **Fehler Verträge** – zulassen, dass benutzerdefinierte SOAP-Fehler angegeben werden.
- **Dienstverträge** – geben Sie die Vorgänge an, die von Diensten unterstützt werden, sowie die für die Interaktion mit den einzelnen Vorgängen erforderlichen Außerdem geben Sie ein benutzerdefiniertes Fehler Verhalten an, das den Vorgängen in jedem Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET-Webdiensten (ASMX) und WCF, WCF unterstützt jedoch dieselben Funktionen wie ASMX – SOAP-Nachrichten über http. Weitere Informationen zur Nutzung eines ASMX-Diensts finden Sie unter Verwenden von [ASP.NET-Webdiensten (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Die xamarin-Platt Form Unterstützung für WCF ist mithilfe der-Klasse auf Text codierte SOAP-Nachrichten über HTTP/HTTPS beschränkt `BasicHttpBinding` .
>
> WCF-Unterstützung erfordert die Verwendung von Tools, die nur in einer Windows-Umgebung verfügbar sind, um den Proxy zu generieren und den todowcfservice zu hosten Das entwickeln und Testen der IOS-App erfordert die Bereitstellung von "dedowcfservice" auf einem Windows-Computer oder als Azure-Webdienst.
>
> Xamarin Forms Native apps Teilen Code in der Regel mit einer .NET Standard-Klassenbibliothek. Allerdings unterstützt .net Core derzeit WCF nicht, daher muss das freigegebene Projekt eine ältere portable-Klassenbibliothek sein. Weitere Informationen zur WCF-Unterstützung in .net Core finden Sie unter [auswählen zwischen .net Core und .NET Framework für Server-apps](/dotnet/standard/choosing-core-framework-server).

Die Beispiel Anwendungslösung umfasst einen WCF-Dienst, der lokal ausgeführt werden kann, und ist im folgenden Screenshot dargestellt:

![](wcf-images/portal.png "Sample Application")

> [!NOTE]
> In ios 9 und höher erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird. Da ATS in apps, die für IOS 9 erstellt wurden, standardmäßig aktiviert ist, unterliegen alle Verbindungen den Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.
>
> Die Ate können deaktiviert werden, wenn es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Nutzen des Webdiensts

Der WCF-Dienst stellt die folgenden Vorgänge bereit:

|Vorgang|BESCHREIBUNG|Parameter|
|--- |--- |--- |
|Getgetdoitems|Abrufen einer Liste von To-Do-Elementen|
|"Kreatedoitem"|Neues to-do-Element erstellen|Ein serialisiertes XML-Element todoitem|
|Editto doitem|Aktualisieren eines To-Do-Elements|Ein serialisiertes XML-Element todoitem|
|Deleteto doitem|Löschen eines To-Do-Elements|Ein serialisiertes XML-Element todoitem|

Weitere Informationen zum Datenmodell, das in der Anwendung verwendet wird, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

Ein *Proxy* muss generiert werden, um einen WCF-Dienst zu nutzen, mit dem die Anwendung eine Verbindung mit dem Dienst herstellen kann. Der Proxy wird erstellt, indem Dienst Metadaten genutzt werden, die die Methoden und die zugehörige Dienst Konfiguration definieren. Diese Metadaten werden in Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, das vom Webdienst generiert wird. Der Proxy kann erstellt werden, indem der Microsoft WCF Web Service Reference Provider in Visual Studio 2017 verwendet wird, um einer .NET Standard Bibliothek einen Dienst Verweis für den Webdienst hinzuzufügen. Eine Alternative zum Erstellen des Proxys mithilfe des Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist die Verwendung des Service Model Metadata Utility Tool (svcutil.exe). Weitere Informationen finden Sie unter [Service Model Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Die generierten Proxy Klassen stellen Methoden zum Verarbeiten der Webdienste bereit, die das APM-Entwurfsmuster (Asynchronous Programming Model) verwenden. In diesem Muster wird ein asynchroner Vorgang als zwei Methoden namens *BeginOperationName* und *EndOperationName*implementiert, die den asynchronen Vorgang starten und beenden.

Die *BeginOperationName* -Methode startet den asynchronen Vorgang und gibt ein Objekt zurück, das die- `IAsyncResult` Schnittstelle implementiert. Nach dem Aufrufen von *BeginOperationName*kann eine Anwendung die Ausführung von Anweisungen für den aufrufenden Thread fortsetzen, während der asynchrone Vorgang in einem Thread Pool Thread erfolgt.

Für jeden Aufrufen von *BeginOperationName*sollte die Anwendung auch *EndOperationName* aufrufen, um die Ergebnisse des Vorgangs zu erhalten. Der Rückgabewert von *EndOperationName* ist derselbe Typ, der von der synchronen Webdienst Methode zurückgegeben wird. Beispielsweise gibt die- `EndGetTodoItems` Methode eine Auflistung von- `TodoItem` Instanzen zurück. Die *EndOperationName* -Methode enthält auch einen- `IAsyncResult` Parameter, der auf die-Instanz festgelegt werden soll, die durch den entsprechenden Aufrufen der *BeginOperationName* -Methode zurückgegeben wird.

Der Task Parallel Library (TPL) kann den Prozess der Nutzung eines apm-Begin/End-Methoden Paars vereinfachen, indem die asynchronen Vorgänge in demselben Objekt gekapselt werden `Task` . Diese Kapselung wird von mehreren über Ladungen der- `TaskFactory.FromAsync` Methode bereitgestellt.

Weitere Informationen zu APM finden Sie unter [asynchrones Programmiermodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="create-the-todoserviceclient-object"></a>Erstellen des Objekts "dedoserviceclient"

Die generierte Proxy Klasse stellt die- `TodoServiceClient` Klasse bereit, die für die Kommunikation mit dem WCF-Dienst über HTTP verwendet wird. Sie stellt Funktionen zum Aufrufen von Webdienst Methoden als asynchrone Vorgänge aus einer vom URI identifizierten Dienst Instanz bereit. Weitere Informationen zu asynchronen Vorgängen finden Sie [unter async-Unterstützung: Übersicht](~/cross-platform/platform/async.md).

Die `TodoServiceClient` -Instanz wird auf Klassenebene deklariert, sodass das-Objekt so lange verwendet wird, wie die Anwendung den WCF-Dienst nutzen muss, wie im folgenden Codebeispiel gezeigt:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

Die `TodoServiceClient` Instanz wird mit Bindungs Informationen und einer Endpunkt Adresse konfiguriert. Eine Bindung wird verwendet, um die Transport-, Codierungs-und Protokoll Details anzugeben, die für die Kommunikation zwischen Anwendungen und Diensten erforderlich sind. Der `BasicHttpBinding` gibt an, dass Text codierte SOAP-Nachrichten über das HTTP-Transportprotokoll gesendet werden. Durch das Angeben einer Endpunkt Adresse kann die Anwendung eine Verbindung mit verschiedenen Instanzen des WCF-Dienstanbieter herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

Weitere Informationen zum Konfigurieren des Dienst Verweises finden Sie unter [Konfigurieren des Dienst Verweises](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Erstellen von Datenübertragungs Objekten

Die Beispielanwendung verwendet die- `TodoItem` Klasse, um Daten zu modellieren. Um ein `TodoItem` Element im Webdienst zu speichern, muss es zuerst in den vom Proxy generierten `TodoItem` Typ konvertiert werden. Dies erfolgt durch die- `ToWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Diese Methode erstellt einfach eine neue `TodoWCFService.TodoItem` -Instanz und legt jede Eigenschaft auf die identische-Eigenschaft der- `TodoItem` Instanz fest.

Ebenso muss beim Abrufen von Daten aus dem-Webdienst aus dem vom Proxy generierten `TodoItem` Typ in eine-Instanz konvertiert werden `TodoItem` . Dies erfolgt mit der- `FromWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Mit dieser Methode werden lediglich die Daten aus dem vom Proxy generierten `TodoItem` Typ abgerufen und in der neu erstellten `TodoItem` Instanz festgelegt.

### <a name="retrieve-data"></a>Abrufen von Daten

Mit der `TodoServiceClient.BeginGetTodoItems` -Methode und der- `TodoServiceClient.EndGetTodoItems` Methode wird der `GetTodoItems` vom Webdienst bereitgestellte Vorgang aufgerufen. Diese asynchronen Methoden werden in einem-Objekt gekapselt `Task` , wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

Die- `Task.Factory.FromAsync` Methode erstellt eine `Task` , die die-Methode ausführt `TodoServiceClient.EndGetTodoItems` , nachdem die- `TodoServiceClient.BeginGetTodoItems` Methode abgeschlossen wurde. der- `null` Parameter gibt an, dass keine Daten an den Delegaten übergeben werden `BeginGetTodoItems` . Schließlich gibt der Wert der- `TaskCreationOptions` Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die- `TodoServiceClient.EndGetTodoItems` Methode gibt eine `ObservableCollection` von-Instanzen zurück `TodoWCFService.TodoItem` , die dann zur Anzeige in eine-Instanz konvertiert wird `List` `TodoItem` .

### <a name="create-data"></a>Erstellen von Daten

Mit der `TodoServiceClient.BeginCreateTodoItem` -Methode und der- `TodoServiceClient.EndCreateTodoItem` Methode wird der `CreateTodoItem` vom Webdienst bereitgestellte Vorgang aufgerufen. Diese asynchronen Methoden werden in einem-Objekt gekapselt `Task` , wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Die- `Task.Factory.FromAsync` Methode erstellt eine `Task` , die die-Methode ausführt `TodoServiceClient.EndCreateTodoItem` , nachdem die- `TodoServiceClient.BeginCreateTodoItem` Methode abgeschlossen wurde, wobei der-Parameter die Daten ist, die an den-Delegaten `todoItem` übergeben werden, `BeginCreateTodoItem` um die `TodoItem` vom Webdienst zu erstellende anzugeben. Schließlich gibt der Wert der- `TaskCreationOptions` Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der-Webdienst löst einen `FaultException` aus, wenn er die nicht erstellen `TodoItem` kann, die von der Anwendung verarbeitet wird.

### <a name="update-data"></a>Aktualisieren von Daten

Mit der `TodoServiceClient.BeginEditTodoItem` -Methode und der- `TodoServiceClient.EndEditTodoItem` Methode wird der `EditTodoItem` vom Webdienst bereitgestellte Vorgang aufgerufen. Diese asynchronen Methoden werden in einem-Objekt gekapselt `Task` , wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Die- `Task.Factory.FromAsync` Methode erstellt eine `Task` , die die-Methode ausführt `TodoServiceClient.EndEditTodoItem` , nachdem die- `TodoServiceClient.BeginCreateTodoItem` Methode abgeschlossen wurde, wobei der-Parameter die Daten ist, die an den-Delegaten übergeben werden, `todoItem` `BeginEditTodoItem` um die `TodoItem` vom Webdienst zu Aktualisier Ende anzugeben Schließlich gibt der Wert der- `TaskCreationOptions` Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der-Webdienst löst einen `FaultException` aus, wenn er das nicht finden oder aktualisieren `TodoItem` kann, das von der Anwendung behandelt wird.

### <a name="delete-data"></a>Löschen von Daten

Mit der `TodoServiceClient.BeginDeleteTodoItem` -Methode und der- `TodoServiceClient.EndDeleteTodoItem` Methode wird der `DeleteTodoItem` vom Webdienst bereitgestellte Vorgang aufgerufen. Diese asynchronen Methoden werden in einem-Objekt gekapselt `Task` , wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

Die- `Task.Factory.FromAsync` Methode erstellt eine `Task` , die die-Methode ausführt `TodoServiceClient.EndDeleteTodoItem` , nachdem die- `TodoServiceClient.BeginDeleteTodoItem` Methode abgeschlossen wurde, wobei der-Parameter die Daten ist, die an den-Delegaten übergeben werden, `id` `BeginDeleteTodoItem` um die `TodoItem` vom Webdienst zu löschende anzugeben. Schließlich gibt der Wert der- `TaskCreationOptions` Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der-Webdienst löst einen `FaultException` aus, wenn er das nicht finden oder Löschen `TodoItem` kann, das von der Anwendung behandelt wird.

## <a name="configure-remote-access-to-iis-express"></a>Konfigurieren des Remote Zugriffs auf IIS Express
In Visual Studio 2017 oder Visual Studio 2019 sollten Sie in der Lage sein, die UWP-Anwendung auf einem PC ohne zusätzliche Konfiguration zu testen. Das Testen von Android-und IOS-Clients erfordert möglicherweise die zusätzlichen Schritte in diesem Abschnitt. Weitere Informationen finden [Sie unter Herstellen einer Verbindung mit lokalen Webdiensten von IOS-Simulatoren und Android-Emulatoren](~/cross-platform/deploy-test/connect-to-local-web-services.md) .

Standardmäßig antwortet IIS Express nur auf Anforderungen von `localhost` . Remote Geräte (z. b. ein Android-Gerät, ein iPhone oder sogar ein Simulator) haben keinen Zugriff auf Ihren lokalen WCF-Dienst. Sie müssen Ihre Windows 10-Arbeitsstations-IP-Adresse im lokalen Netzwerk kennen. Nehmen Sie für dieses Beispiel an, dass die Arbeitsstation über die IP-Adresse verfügt `192.168.1.143` . In den folgenden Schritten wird erläutert, wie Sie Windows 10 konfigurieren und IIS Express, um Remote Verbindungen zu akzeptieren und von einem physischen oder virtuellen Gerät aus eine Verbindung mit dem Dienst herzustellen:

1. **Fügen Sie der Windows-Firewall eine Ausnahme hinzu**. Sie müssen über die Windows-Firewall einen Port öffnen, der von Anwendungen in Ihrem Subnetz verwendet werden kann, um mit dem WCF-Dienst zu kommunizieren. Erstellen Sie eine eingehende Regel zum Öffnen von Port 49393 in der Firewall. Führen Sie an einer administrativen Eingabeaufforderung den folgenden Befehl aus:

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Konfigurieren Sie IIS Express, um Remote Verbindungen zu akzeptieren**. Sie können IIS Express konfigurieren, indem Sie die Konfigurationsdatei für IIS Express unter **[Projektmappenverzeichnis] \.vs\config\applicationhost.config**bearbeiten. Suchen Sie nach dem- `site` Element mit dem Namen `TodoWCFService` . Es sollte in etwa wie folgt aussehen:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
        </bindings>
    </site>
    ```

    Sie müssen zwei Elemente hinzufügen `binding` , um Port 49393 für den externen Datenverkehr und den Android-Emulator zu öffnen. Die Bindung verwendet ein `[IP address]:[port]:[hostname]` Format, das angibt, wie IIS Express auf Anforderungen antwortet. Externe Anforderungen verfügen über Hostnamen, die als angegeben werden müssen `binding` . Fügen Sie dem-Element das folgende XML hinzu `bindings` , und ersetzen Sie dabei die IP-Adresse durch ihre eigene IP-Adresse:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    Nachdem Sie die Änderungen vorgenommen haben, sollte das- `bindings` Element wie folgt aussehen:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
            <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
            <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
        </bindings>
    </site>
    ```

    >[!IMPORTANT]
    >Standardmäßig werden von IIS Express aus Sicherheitsgründen keine Verbindungen von externen Quellen akzeptiert. Zum Aktivieren von Verbindungen von Remote Geräten müssen Sie IIS Express mit Administrator Berechtigungen ausführen. Die einfachste Möglichkeit hierzu ist das Ausführen von Visual Studio 2017 mit Administrator Berechtigungen. Dadurch wird IIS Express mit Administrator Berechtigungen gestartet, wenn der-Dienst ausgeführt wird.

    Nachdem Sie diese Schritte ausgeführt haben, sollten Sie in der Lage sein, den todowcfservice auszuführen und eine Verbindung von anderen Geräten in Ihrem Subnetz herzustellen. Sie können dies testen, indem Sie Ihre Anwendung ausführen und besuchen `http://localhost:49393/TodoService.svc` . Wenn beim Besuch dieser URL ein Fehler wegen einer ungültigen **Anforderung** auftritt, ist Ihr `bindings` in der IIS Express Konfiguration möglicherweise falsch (die Anforderung geht IIS Express, wird jedoch zurückgewiesen). Wenn Sie einen anderen Fehler erhalten, kann es sein, dass Ihre Anwendung nicht ausgeführt wird oder die Firewall nicht ordnungsgemäß konfiguriert ist.

    Deaktivieren Sie die Option " **Bearbeiten und Fortfahren** " in den **Projekteigenschaften > Web->-Debuggern**, damit IIS Express den Dienst weiterhin ausführen und bedienen können.

1. **Anpassen der Endpunkt Geräte, die für den Zugriff auf den Dienst verwendet**werden. Dieser Schritt umfasst das Konfigurieren der Client Anwendung, die auf einem physischen oder emulierten Gerät ausgeführt wird, um auf den WCF-Dienst zuzugreifen.

    Der Android-Emulator verwendet einen internen Proxy, der verhindert, dass der Emulator direkt auf die Adresse des Host Computers zugreift `localhost` . Stattdessen wird die Adresse des `10.0.2.2` Emulators über `localhost` einen internen Proxy an den Host Computer weitergeleitet. Diese Proxy Anforderungen verfügen über den `127.0.0.1` Hostnamen im Anforderungs Header, weshalb Sie in den obigen Schritten die IIS Express Bindung für diesen Hostnamen erstellt haben.

    Der IOS-Simulator wird auf einem Mac-buildhost ausgeführt, auch wenn Sie den [remoten IOS-Simulator für Windows](~/tools/ios-simulator/index.md)verwenden. Bei Netzwerk Anforderungen aus dem Simulator wird die IP-Adresse Ihrer Arbeitsstation im lokalen Netzwerk als Hostname angezeigt (in diesem Beispiel ist dies `192.168.1.143` , ihre tatsächliche IP-Adresse ist jedoch wahrscheinlich anders). Aus diesem Grund haben Sie in den obigen Schritten die IIS Express Bindung für diesen Hostnamen erstellt.

    Stellen `SoapUrl` Sie sicher, dass die-Eigenschaft in der **Constants.cs** -Datei im Projekt "Projekt CF" (portabel) Werte hat, die für Ihr Netzwerk korrekt sind:

    ```csharp
    public static string SoapUrl
    {
        get
        {
            var defaultUrl = "http://localhost:49393/TodoService.svc";

            if (Device.RuntimePlatform == Device.Android)
            {
                defaultUrl = "http://10.0.2.2:49393/TodoService.svc";
            }
            else if (Device.RuntimePlatform == Device.iOS)
            {
                defaultUrl = "http://192.168.1.143:49393/TodoService.svc";
            }

            return defaultUrl;
        }
    }
    ```

    Nachdem Sie die **Constants.cs** mit den entsprechenden Endpunkten konfiguriert haben, sollten Sie in der Lage sein, von physischen oder virtuellen Geräten aus eine Verbindung mit dem auf Ihrer Windows 10-Arbeitsstation ausgelaufenden Dienst von Windows 10 herzustellen.

## <a name="related-links"></a>Verwandte Links

- ["Windows CF" (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [Gewusst wie: Erstellen eines Windows Communication Foundation-Clients](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Service Model Metadata Utility-Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
