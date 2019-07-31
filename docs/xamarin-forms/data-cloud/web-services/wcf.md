---
title: Verwenden eines Windows Communication Foundation (WCF)-Webdiensts
description: In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: d170e37b8bf4ce880f9d8f48d30defb42ee6bba2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648010"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Verwenden eines Windows Communication Foundation (WCF)-Webdiensts

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen. In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird._

WCF beschreibt einen Dienst mit einer Vielzahl verschiedener Verträge, einschließlich:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt in einer Nachricht zu bilden.
- **Nachrichtenverträge** – Erstellen von Nachrichten von vorhandene Datenverträge.
- **Fehler Verträge** – ermöglichen von benutzerdefinierten SOAP-Fehlern angegeben werden.
- **-Dienstverträge** : Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem Verhalten benutzerdefinierter Fehler, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET-Webdiensten (ASMX) und WCF, WCF unterstützt jedoch dieselben Funktionen wie ASMX – SOAP-Nachrichten über http. Weitere Informationen zur Nutzung eines ASMX-Diensts finden Sie unter Verwenden von [ASP.NET-Webdiensten (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Die xamarin-Platt Form Unterstützung für WCF ist mithilfe der `BasicHttpBinding` -Klasse auf Text codierte SOAP-Nachrichten über HTTP/HTTPS beschränkt.
>
> WCF-Unterstützung erfordert die Verwendung von Tools, die nur in einer Windows-Umgebung verfügbar sind, um den Proxy zu generieren und den todowcfservice zu hosten Das entwickeln und Testen der IOS-App erfordert die Bereitstellung von "dedowcfservice" auf einem Windows-Computer oder als Azure-Webdienst.
>
> Xamarin Forms Native apps Teilen Code in der Regel mit einer .NET Standard-Klassenbibliothek. Allerdings unterstützt .net Core derzeit WCF nicht, daher muss das freigegebene Projekt eine ältere portable-Klassenbibliothek sein. Weitere Informationen zur WCF-Unterstützung in .net Core finden Sie unter [auswählen zwischen .net Core und .NET Framework für Server-apps](/dotnet/standard/choosing-core-framework-server).

Die Beispiel Anwendungslösung umfasst einen WCF-Dienst, der lokal ausgeführt werden kann, und ist im folgenden Screenshot dargestellt:

![](wcf-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
>
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Verwenden des Webdiensts

Der WCF-Dienst bietet die folgenden Vorgänge:

|Vorgang|Beschreibung|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Erstellt ein neues to-do-Element|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zu das Datenmodell in der Anwendung verwendet, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

Ein *Proxy* muss generiert werden, um einen WCF-Dienst nutzen, die der Anwendung ermöglicht, mit dem Dienst herstellen. Der Proxy wird erstellt, indem verarbeitende Dienstmetadaten, die die Methoden und zugeordnete Dienstkonfiguration zu definieren. Diese Metadaten werden in die Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die vom Webdienst generiert wird. Hinzufügen ein Dienstverweises für den Webdienst in einer .NET Standard-Bibliothek mit den Microsoft WCF Web Service Reference Provider in Visual Studio 2017, kann der Proxy erstellt werden. Eine Alternative zum Erstellen des Proxys verwenden den Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Die generierten Proxyklassen bieten Methoden für die Nutzung der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. Bei diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Z. B. die `EndGetTodoItems` Methode gibt eine Auflistung von `TodoItem` Instanzen. Die *EndOperationName* Methode schließt auch eine `IAsyncResult` Parameter, um die vom entsprechenden Aufruf zurückgegebene Instanz festgelegt werden, sollte, die *BeginOperationName* Methode.

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `TaskFactory.FromAsync` Methode.

Weitere Informationen zu APM finden Sie unter [asynchronen Programmiermodells](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="create-the-todoserviceclient-object"></a>Erstellen des Objekts "dedoserviceclient"

Die generierte Proxyklasse enthält die `TodoServiceClient` -Klasse, die Kommunikation mit dem WCF-Dienst über HTTP verwendet wird. Es stellt Funktionen bereit, für das Aufrufen von Webdienstmethoden, wie asynchrone Vorgänge aus einem URI-Dienstinstanz identifiziert. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Async-Unterstützung (Übersicht)](~/cross-platform/platform/async.md).

Die `TodoServiceClient` Instanz wird auf der Klassenebene deklariert, so, dass für das Objekt befindet, solange die Anwendung den WCF-Dienst nutzen, wie im folgenden Codebeispiel gezeigt muss:

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

Die `TodoServiceClient` -Instanz mit einer Bindung, Informationen und eine Endpunktadresse konfiguriert ist. Eine Bindung wird verwendet, an den Transport, Codierung und Protokolldetails erforderlich für Anwendungen und Dienste miteinander kommunizieren. Die `BasicHttpBinding` gibt an, dass textcodierten SOAP-Nachrichten über das HTTP-Transport-Protokoll gesendet werden. Angeben einer Endpunktadresse kann die Anwendung mit verschiedenen Instanzen von den WCF-Dienst herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

Weitere Informationen zu den Dienstverweis konfigurieren, finden Sie unter [konfigurieren den Dienstverweis](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Erstellen von Datenübertragungs Objekten

Die beispielanwendung verwendet die `TodoItem` Klasse, um die Daten des Modells. Zum Speichern einer `TodoItem` Element in den Webdienst, sie zuerst in den generierten Proxy konvertiert werden müssen `TodoItem` Typ. Dies erfolgt durch die `ToWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode erstellt einfach ein neues `TodoWCFService.TodoItem` Instanz, und legt jede Eigenschaft, die identische Eigenschaft aus der `TodoItem` Instanz.

Auf ähnliche Weise, wenn Daten aus dem Webdienst abgerufen werden, muss er sein konvertiert aus den generierten Proxy `TodoItem` Typ in einen `TodoItem` Instanz. Dies erfolgt mit der `FromWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode ruft einfach die Daten ab, aus den generierten Proxy `TodoItem` geben und wird in der neu erstellten `TodoItem` Instanz.

### <a name="retrieve-data"></a>Abrufen von Daten

Die `TodoServiceClient.BeginGetTodoItems` und `TodoServiceClient.EndGetTodoItems` Methoden zum Aufrufen der `GetTodoItems` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndGetTodoItems` -Methode einmal die `TodoServiceClient.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben wird, werden die `BeginGetTodoItems` delegieren. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die `TodoServiceClient.EndGetTodoItems` Methode gibt ein `ObservableCollection` von `TodoWCFService.TodoItem` -Instanzen, die anschließend in konvertiert wird eine `List` von `TodoItem` Instanzen für die Anzeige.

### <a name="create-data"></a>Erstellen von Daten

Die `TodoServiceClient.BeginCreateTodoItem` und `TodoServiceClient.EndCreateTodoItem` Methoden zum Aufrufen der `CreateTodoItem` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndCreateTodoItem` -Methode einmal die `TodoServiceClient.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die in übergeben werden die `BeginCreateTodoItem` Delegat, um die `TodoItem` vom Webdienst erstellt werden. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Löst die Web-Dienst eine `FaultException` Ausfall zum Erstellen der `TodoItem`, die von der Anwendung erfolgt.

### <a name="update-data"></a>Aktualisieren von Daten

Die `TodoServiceClient.BeginEditTodoItem` und `TodoServiceClient.EndEditTodoItem` Methoden zum Aufrufen der `EditTodoItem` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndEditTodoItem` -Methode einmal die `TodoServiceClient.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die in übergeben werden die `BeginEditTodoItem` Delegat, um die `TodoItem` vom Webdienst aktualisiert werden. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der Web-Dienst löst eine `FaultException` bei einem Fehler zu suchen, oder Aktualisieren der `TodoItem`, die von der Anwendung erfolgt.

### <a name="delete-data"></a>Löschen von Daten

Die `TodoServiceClient.BeginDeleteTodoItem` und `TodoServiceClient.EndDeleteTodoItem` Methoden zum Aufrufen der `DeleteTodoItem` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndDeleteTodoItem` -Methode einmal die `TodoServiceClient.BeginDeleteTodoItem` Methode ausgeführt wird, mit der `id` Parameter werden die Daten, die in übergeben werden die `BeginDeleteTodoItem` Delegat, um die `TodoItem` vom Webdienst gelöscht werden soll. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der Web-Dienst löst eine `FaultException` bei einem Fehler zu suchen, oder Löschen der `TodoItem`, die von der Anwendung erfolgt.

## <a name="configure-remote-access-to-iis-express"></a>Konfigurieren des Remote Zugriffs auf IIS Express
In Visual Studio 2017 oder Visual Studio 2019 sollten Sie in der Lage sein, die UWP-Anwendung auf einem PC ohne zusätzliche Konfiguration zu testen. Das Testen von Android-und IOS-Clients erfordert möglicherweise die zusätzlichen Schritte in diesem Abschnitt. Weitere Informationen finden [Sie unter Herstellen einer Verbindung mit lokalen Webdiensten von IOS-Simulatoren und Android-Emulatoren](~/cross-platform/deploy-test/connect-to-local-web-services.md) .

Standardmäßig antwortet IIS Express nur auf Anforderungen `localhost`von. Remote Geräte (z. b. ein Android-Gerät, ein iPhone oder sogar ein Simulator) haben keinen Zugriff auf Ihren lokalen WCF-Dienst. Sie müssen Ihre Windows 10-Arbeitsstations-IP-Adresse im lokalen Netzwerk kennen. Nehmen Sie für dieses Beispiel an, dass die Arbeitsstation über die IP- `192.168.1.143`Adresse verfügt. In den folgenden Schritten wird erläutert, wie Sie Windows 10 konfigurieren und IIS Express, um Remote Verbindungen zu akzeptieren und von einem physischen oder virtuellen Gerät aus eine Verbindung mit dem Dienst herzustellen:

1. **Fügen Sie der Windows-Firewall eine Ausnahme hinzu**. Sie müssen über die Windows-Firewall einen Port öffnen, der von Anwendungen in Ihrem Subnetz verwendet werden kann, um mit dem WCF-Dienst zu kommunizieren. Erstellen Sie eine eingehende Regel zum Öffnen von Port 49393 in der Firewall. Führen Sie an einer administrativen Eingabeaufforderung den folgenden Befehl aus:
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Konfigurieren Sie IIS Express, um Remote Verbindungen zu akzeptieren**. Sie können IIS Express konfigurieren, indem Sie die Konfigurationsdatei für IIS Express unter [Projektmappenverzeichnis **]\.vs\config\applicationhost.config**bearbeiten. Suchen Sie `site` nach dem-Element `TodoWCFService`mit dem Namen. Es sollte in etwa wie folgt aussehen:

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

    Sie müssen zwei `binding` Elemente hinzufügen, um Port 49393 für den externen Datenverkehr und den Android-Emulator zu öffnen. Die Bindung verwendet ein `[IP address]:[port]:[hostname]` Format, das angibt, wie IIS Express auf Anforderungen antwortet. Externe Anforderungen verfügen über Hostnamen, die als `binding`angegeben werden müssen. Fügen Sie dem- `bindings` Element das folgende XML hinzu, und ersetzen Sie dabei die IP-Adresse durch ihre eigene IP-Adresse:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    Nachdem Sie die Änderungen `bindings` vorgenommen haben, sollte das-Element wie folgt aussehen:

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

    Nachdem Sie diese Schritte ausgeführt haben, sollten Sie in der Lage sein, den todowcfservice auszuführen und eine Verbindung von anderen Geräten in Ihrem Subnetz herzustellen. Sie können dies testen, indem Sie Ihre Anwendung ausführen `http://localhost:49393/TodoService.svc`und besuchen. Wenn beim Besuch dieser URL ein Fehler wegen einer ungültigen **Anforderung** auftritt `bindings` , ist Ihr in der IIS Express Konfiguration möglicherweise falsch (die Anforderung geht IIS Express, wird jedoch zurückgewiesen). Wenn Sie einen anderen Fehler erhalten, kann es sein, dass Ihre Anwendung nicht ausgeführt wird oder die Firewall nicht ordnungsgemäß konfiguriert ist.

    Deaktivieren Sie die Option " **Bearbeiten und Fortfahren** " in den **Projekteigenschaften > web->-Debuggern**, damit IIS Express den Dienst weiterhin ausführen und bedienen können.

1. **Anpassen der Endpunkt Geräte, die für den Zugriff auf den Dienst verwendet**werden. Dieser Schritt umfasst das Konfigurieren der Client Anwendung, die auf einem physischen oder emulierten Gerät ausgeführt wird, um auf den WCF-Dienst zuzugreifen.

    Der Android-Emulator verwendet einen internen Proxy, der verhindert, dass der Emulator direkt auf die `localhost` Adresse des Host Computers zugreift. Stattdessen wird die Adresse `10.0.2.2` des Emulators über einen internen `localhost` Proxy an den Host Computer weitergeleitet. Diese Proxy Anforderungen verfügen über den `127.0.0.1` Hostnamen im Anforderungs Header, weshalb Sie in den obigen Schritten die IIS Express Bindung für diesen Hostnamen erstellt haben.

    Der IOS-Simulator wird auf einem Mac-buildhost ausgeführt, auch wenn Sie den [remoten IOS-Simulator für Windows](~/tools/ios-simulator/index.md)verwenden. Bei Netzwerk Anforderungen aus dem Simulator wird die IP-Adresse Ihrer Arbeitsstation im lokalen Netzwerk als Hostname angezeigt (in diesem `192.168.1.143`Beispiel ist dies, ihre tatsächliche IP-Adresse ist jedoch wahrscheinlich anders). Aus diesem Grund haben Sie in den obigen Schritten die IIS Express Bindung für diesen Hostnamen erstellt.

    Stellen Sie `SoapUrl` sicher, dass die-Eigenschaft in der **Constants.cs** -Datei im Projekt "Projekt CF" (portabel) Werte hat, die für Ihr Netzwerk korrekt sind:

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

- [TodoWCF (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [Vorgehensweise: Erstellen eines Windows Communication Foundation Clients](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Service Model Metadata Utility-Tool (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
