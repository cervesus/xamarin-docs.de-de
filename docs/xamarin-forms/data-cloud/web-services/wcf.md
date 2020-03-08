---
title: Verwenden eines Windows Communication Foundation (WCF)-Webdiensts
description: In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: 28cb1573262b63cc2b0ccad9f468fe36c682718d
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915391"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Verwenden eines Windows Communication Foundation (WCF)-Webdiensts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF ist das vereinheitlichte Framework von Microsoft zum entwickeln Dienst orientierter Anwendungen. Es ermöglicht Entwicklern das Erstellen sicherer, zuverlässiger, transaktiver und interoperable verteilter Anwendungen. In diesem Artikel wird veranschaulicht, wie ein WCF Simple Object Access Protocol (SOAP)-Dienst aus einer xamarin. Forms-Anwendung verwendet wird._

WCF beschreibt einen Dienst mit einer Vielzahl verschiedener Verträge, einschließlich:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt innerhalb einer Nachricht bilden.
- **Nachrichten Verträge** – Verfassen von Nachrichten aus vorhandenen Daten Verträgen.
- **Fehler Verträge** – zulassen, dass benutzerdefinierte SOAP-Fehler angegeben werden.
- **Dienstverträge** – geben Sie die Vorgänge an, die von Diensten unterstützt werden, sowie die für die Interaktion mit den einzelnen Vorgängen erforderlichen Sie geben außerdem Verhalten benutzerdefinierter Fehler, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET-Webdiensten (ASMX) und WCF, WCF unterstützt jedoch dieselben Funktionen wie ASMX – SOAP-Nachrichten über http. Weitere Informationen zur Nutzung eines ASMX-Diensts finden Sie unter Verwenden von [ASP.NET-Webdiensten (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Die xamarin-Platt Form Unterstützung für WCF ist auf Text codierte SOAP-Nachrichten über HTTP/HTTPS mit der `BasicHttpBinding`-Klasse beschränkt.
>
> WCF-Unterstützung erfordert die Verwendung von Tools, die nur in einer Windows-Umgebung verfügbar sind, um den Proxy zu generieren und den todowcfservice zu hosten Das entwickeln und Testen der IOS-App erfordert die Bereitstellung von "dedowcfservice" auf einem Windows-Computer oder als Azure-Webdienst.
>
> Xamarin Forms Native apps Teilen Code in der Regel mit einer .NET Standard-Klassenbibliothek. Allerdings unterstützt .net Core derzeit WCF nicht, daher muss das freigegebene Projekt eine ältere portable-Klassenbibliothek sein. Weitere Informationen zur WCF-Unterstützung in .net Core finden Sie unter [auswählen zwischen .net Core und .NET Framework für Server-apps](/dotnet/standard/choosing-core-framework-server).

Die Beispiel Anwendungslösung umfasst einen WCF-Dienst, der lokal ausgeführt werden kann, und ist im folgenden Screenshot dargestellt:

![](wcf-images/portal.png "Sample Application")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
>
> ATS kann deaktiviert werden, wenn es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Nutzen des Webdiensts

Der WCF-Dienst bietet die folgenden Vorgänge:

|Vorgang|BESCHREIBUNG|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Erstellt ein neues to-do-Element|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zum Datenmodell, das in der Anwendung verwendet wird, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

Ein *Proxy* muss generiert werden, um einen WCF-Dienst zu nutzen, mit dem die Anwendung eine Verbindung mit dem Dienst herstellen kann. Der Proxy wird erstellt, indem verarbeitende Dienstmetadaten, die die Methoden und zugeordnete Dienstkonfiguration zu definieren. Diese Metadaten werden in die Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die vom Webdienst generiert wird. Hinzufügen ein Dienstverweises für den Webdienst in einer .NET Standard-Bibliothek mit den Microsoft WCF Web Service Reference Provider in Visual Studio 2017, kann der Proxy erstellt werden. Eine Alternative zum Erstellen des Proxys verwenden den Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [Service Model Metadata Utility-Tool (Svcutil. exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Die generierten Proxyklassen bieten Methoden für die Nutzung der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. In diesem Muster wird ein asynchroner Vorgang als zwei Methoden namens *BeginOperationName* und *EndOperationName*implementiert, die den asynchronen Vorgang starten und beenden.

Die *BeginOperationName* -Methode startet den asynchronen Vorgang und gibt ein Objekt zurück, das die `IAsyncResult`-Schnittstelle implementiert. Nach dem Aufrufen von *BeginOperationName*kann eine Anwendung die Ausführung von Anweisungen für den aufrufenden Thread fortsetzen, während der asynchrone Vorgang in einem Thread Pool Thread erfolgt.

Für jeden Aufrufen von *BeginOperationName*sollte die Anwendung auch *EndOperationName* aufrufen, um die Ergebnisse des Vorgangs zu erhalten. Der Rückgabewert von *EndOperationName* ist derselbe Typ, der von der synchronen Webdienst Methode zurückgegeben wird. Die `EndGetTodoItems`-Methode gibt z. b. eine Auflistung von `TodoItem`-Instanzen zurück. Die *EndOperationName* -Methode enthält auch einen `IAsyncResult`-Parameter, der auf die Instanz festgelegt werden soll, die durch den entsprechenden Aufrufen der *BeginOperationName* -Methode zurückgegeben wird.

Der Task Parallel Library (TPL) kann den Prozess der Nutzung eines apm-Begin/End-Methoden Paars vereinfachen, indem die asynchronen Vorgänge im gleichen `Task` Objekt gekapselt werden. Diese Kapselung wird von mehreren über Ladungen der `TaskFactory.FromAsync`-Methode bereitgestellt.

Weitere Informationen zu APM finden Sie unter [asynchrones Programmiermodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="create-the-todoserviceclient-object"></a>Erstellen des Objekts "dedoserviceclient"

Die generierte Proxy Klasse stellt die `TodoServiceClient`-Klasse bereit, die für die Kommunikation mit dem WCF-Dienst über HTTP verwendet wird. Es stellt Funktionen bereit, für das Aufrufen von Webdienstmethoden, wie asynchrone Vorgänge aus einem URI-Dienstinstanz identifiziert. Weitere Informationen zu asynchronen Vorgängen finden Sie [unter async-Unterstützung: Übersicht](~/cross-platform/platform/async.md).

Die `TodoServiceClient` Instanz wird auf Klassenebene deklariert, sodass das Objekt so lange verwendet, wie die Anwendung den WCF-Dienst nutzen muss, wie im folgenden Codebeispiel gezeigt:

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

Die `TodoServiceClient` Instanz wird mit Bindungs Informationen und einer Endpunkt Adresse konfiguriert. Eine Bindung wird verwendet, an den Transport, Codierung und Protokolldetails erforderlich für Anwendungen und Dienste miteinander kommunizieren. Der `BasicHttpBinding` gibt an, dass Text codierte SOAP-Nachrichten über das HTTP-Transportprotokoll gesendet werden. Angeben einer Endpunktadresse kann die Anwendung mit verschiedenen Instanzen von den WCF-Dienst herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

Weitere Informationen zum Konfigurieren des Dienst Verweises finden Sie unter [Konfigurieren des Dienst Verweises](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Erstellen von Datenübertragungs Objekten

Die Beispielanwendung verwendet die `TodoItem`-Klasse, um Daten zu modellieren. Um ein `TodoItem` Element im Webdienst zu speichern, muss es zuerst in den `TodoItem` Typ generierten Proxy konvertiert werden. Dies erfolgt durch die `ToWCFServiceTodoItem`-Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode erstellt einfach eine neue `TodoWCFService.TodoItem`-Instanz und legt jede Eigenschaft auf die identische Eigenschaft von der `TodoItem` Instanz fest.

Ebenso muss beim Abrufen von Daten aus dem-Webdienst aus dem Proxy, der `TodoItem`-Typ generiert wurde, in eine `TodoItem`-Instanz konvertiert werden. Dies wird mit der `FromWCFServiceTodoItem`-Methode erreicht, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode ruft einfach die Daten aus dem Proxy ab, der `TodoItem` Typ generiert wurde, und legt ihn in der neu erstellten `TodoItem` Instanz fest.

### <a name="retrieve-data"></a>Abrufen von Daten

Die Methoden `TodoServiceClient.BeginGetTodoItems` und `TodoServiceClient.EndGetTodoItems` werden verwendet, um den `GetTodoItems` Vorgang aufzurufen, der vom Webdienst bereitgestellt wird. Diese asynchronen Methoden werden in einem `Task` Objekt gekapselt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync`-Methode erstellt eine `Task`, die die `TodoServiceClient.EndGetTodoItems`-Methode ausführt, sobald die `TodoServiceClient.BeginGetTodoItems`-Methode abgeschlossen ist. der `null`-Parameter gibt an, dass keine Daten an den `BeginGetTodoItems` Delegaten übergeben werden. Schließlich gibt der Wert der `TaskCreationOptions`-Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die `TodoServiceClient.EndGetTodoItems`-Methode gibt eine `ObservableCollection` von `TodoWCFService.TodoItem`-Instanzen zurück, die dann zur Anzeige in eine `List` von `TodoItem` Instanzen konvertiert werden.

### <a name="create-data"></a>Erstellen von Daten

Die Methoden `TodoServiceClient.BeginCreateTodoItem` und `TodoServiceClient.EndCreateTodoItem` werden verwendet, um den `CreateTodoItem` Vorgang aufzurufen, der vom Webdienst bereitgestellt wird. Diese asynchronen Methoden werden in einem `Task` Objekt gekapselt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync`-Methode erstellt eine `Task`, die die `TodoServiceClient.EndCreateTodoItem`-Methode ausführt, sobald die `TodoServiceClient.BeginCreateTodoItem`-Methode abgeschlossen ist, wobei der `todoItem`-Parameter die Daten ist, die an den `BeginCreateTodoItem` Delegaten übergeben werden, um die vom Webdienst zu erstellenden `TodoItem` anzugeben. Schließlich gibt der Wert der `TaskCreationOptions`-Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der-Webdienst löst eine `FaultException` aus, wenn er die `TodoItem`nicht erstellen kann, die von der Anwendung verarbeitet wird.

### <a name="update-data"></a>Aktualisieren von Daten

Die Methoden `TodoServiceClient.BeginEditTodoItem` und `TodoServiceClient.EndEditTodoItem` werden verwendet, um den `EditTodoItem` Vorgang aufzurufen, der vom Webdienst bereitgestellt wird. Diese asynchronen Methoden werden in einem `Task` Objekt gekapselt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync`-Methode erstellt eine `Task`, die die `TodoServiceClient.EndEditTodoItem`-Methode ausführt, sobald die `TodoServiceClient.BeginCreateTodoItem`-Methode abgeschlossen ist. der `todoItem` Parameter ist die Daten, die an den `BeginEditTodoItem` Delegaten übergeben werden, um die `TodoItem` anzugeben, die vom Webdienst aktualisiert werden soll. Schließlich gibt der Wert der `TaskCreationOptions`-Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der Webdienst löst eine `FaultException` aus, wenn er die `TodoItem`nicht finden oder aktualisieren kann, die von der Anwendung verarbeitet wird.

### <a name="delete-data"></a>Löschen von Daten

Die Methoden `TodoServiceClient.BeginDeleteTodoItem` und `TodoServiceClient.EndDeleteTodoItem` werden verwendet, um den `DeleteTodoItem` Vorgang aufzurufen, der vom Webdienst bereitgestellt wird. Diese asynchronen Methoden werden in einem `Task` Objekt gekapselt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync`-Methode erstellt eine `Task`, die die `TodoServiceClient.EndDeleteTodoItem`-Methode ausführt, sobald die `TodoServiceClient.BeginDeleteTodoItem`-Methode abgeschlossen ist. der `id` Parameter ist die Daten, die an den `BeginDeleteTodoItem` Delegaten übergeben werden, um die `TodoItem` anzugeben, die vom Webdienst gelöscht werden sollen. Schließlich gibt der Wert der `TaskCreationOptions`-Enumeration an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der Webdienst löst eine `FaultException` aus, wenn er die `TodoItem`nicht finden oder löschen kann, die von der Anwendung behandelt wird.

## <a name="configure-remote-access-to-iis-express"></a>Konfigurieren des Remote Zugriffs auf IIS Express
In Visual Studio 2017 oder Visual Studio 2019 sollten Sie in der Lage sein, die UWP-Anwendung auf einem PC ohne zusätzliche Konfiguration zu testen. Das Testen von Android-und IOS-Clients erfordert möglicherweise die zusätzlichen Schritte in diesem Abschnitt. Weitere Informationen finden [Sie unter Herstellen einer Verbindung mit lokalen Webdiensten von IOS-Simulatoren und Android-Emulatoren](~/cross-platform/deploy-test/connect-to-local-web-services.md) .

Standardmäßig reagieren IIS Express nur auf Anforderungen an `localhost`. Remote Geräte (z. b. ein Android-Gerät, ein iPhone oder sogar ein Simulator) haben keinen Zugriff auf Ihren lokalen WCF-Dienst. Sie müssen Ihre Windows 10-Arbeitsstations-IP-Adresse im lokalen Netzwerk kennen. Nehmen Sie für dieses Beispiel an, dass die IP-Adresse Ihrer Arbeitsstation `192.168.1.143`ist. In den folgenden Schritten wird erläutert, wie Sie Windows 10 konfigurieren und IIS Express, um Remote Verbindungen zu akzeptieren und von einem physischen oder virtuellen Gerät aus eine Verbindung mit dem Dienst herzustellen:

1. **Fügen Sie der Windows-Firewall eine Ausnahme hinzu**. Sie müssen über die Windows-Firewall einen Port öffnen, der von Anwendungen in Ihrem Subnetz verwendet werden kann, um mit dem WCF-Dienst zu kommunizieren. Erstellen Sie eine eingehende Regel zum Öffnen von Port 49393 in der Firewall. Führen Sie an einer administrativen Eingabeaufforderung den folgenden Befehl aus:

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Konfigurieren Sie IIS Express, um Remote Verbindungen zu akzeptieren**. Sie können IIS Express konfigurieren, indem Sie die Konfigurationsdatei für IIS Express unter **[Projektmappenverzeichnis]\.vs\config\applicationhost.config**bearbeiten. Suchen Sie das `site`-Element mit dem Namen `TodoWCFService`. Es sollte in etwa wie folgt aussehen:

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

    Zum Öffnen von Port 49393 für den externen Datenverkehr und den Android-Emulator müssen zwei `binding` Elemente hinzugefügt werden. Die Bindung verwendet ein `[IP address]:[port]:[hostname]` Format, das angibt, wie IIS Express auf Anforderungen antwortet. Externe Anforderungen verfügen über Hostnamen, die als `binding`angegeben werden müssen. Fügen Sie dem `bindings`-Element den folgenden XML-Code hinzu, und ersetzen Sie die IP-Adresse durch ihre eigene IP-Adresse:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    Nachdem Sie die Änderungen vorgenommen haben, sollte das `bindings` Element wie folgt aussehen:

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

    Nachdem Sie diese Schritte ausgeführt haben, sollten Sie in der Lage sein, den todowcfservice auszuführen und eine Verbindung von anderen Geräten in Ihrem Subnetz herzustellen. Sie können dies testen, indem Sie Ihre Anwendung ausführen und `http://localhost:49393/TodoService.svc`besuchen. Wenn beim Aufrufen dieser URL ein Fehler wegen einer ungültigen **Anforderung** auftritt, ist der `bindings` in der IIS Express Konfiguration möglicherweise falsch (die Anforderung erreicht IIS Express, wird jedoch zurückgewiesen). Wenn Sie einen anderen Fehler erhalten, kann es sein, dass Ihre Anwendung nicht ausgeführt wird oder die Firewall nicht ordnungsgemäß konfiguriert ist.

    Deaktivieren Sie die Option " **Bearbeiten und Fortfahren** " in den **Projekteigenschaften > web->-Debuggern**, damit IIS Express den Dienst weiterhin ausführen und bedienen können.

1. **Anpassen der Endpunkt Geräte, die für den Zugriff auf den Dienst verwendet**werden. Dieser Schritt umfasst das Konfigurieren der Client Anwendung, die auf einem physischen oder emulierten Gerät ausgeführt wird, um auf den WCF-Dienst zuzugreifen.

    Der Android-Emulator verwendet einen internen Proxy, der verhindert, dass der Emulator direkt auf die `localhost` Adresse des Host Computers zugreift. Stattdessen wird die Adresse `10.0.2.2` auf dem Emulator über einen internen Proxy an `localhost` auf dem Host Computer weitergeleitet. Diese Proxy Anforderungen verfügen über `127.0.0.1` als Hostnamen im Anforderungs Header. aus diesem Grund haben Sie die IIS Express Bindung für diesen Hostnamen in den obigen Schritten erstellt.

    Der IOS-Simulator wird auf einem Mac-buildhost ausgeführt, auch wenn Sie den [remoten IOS-Simulator für Windows](~/tools/ios-simulator/index.md)verwenden. Bei Netzwerk Anforderungen aus dem Simulator wird die IP-Adresse Ihrer Arbeitsstation im lokalen Netzwerk als Hostname angezeigt (in diesem Beispiel ist Sie `192.168.1.143`, aber die tatsächliche IP-Adresse ist wahrscheinlich anders). Aus diesem Grund haben Sie in den obigen Schritten die IIS Express Bindung für diesen Hostnamen erstellt.

    Stellen Sie sicher, dass die `SoapUrl`-Eigenschaft in der **Constants.cs** -Datei im Projekt "Projekt CF" (portabel) Werte hat, die für Ihr Netzwerk korrekt sind:

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
- [Vorgehensweise: Erstellen eines Windows Communication Foundation Clients](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Service Model Metadata Utility-Tool (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
