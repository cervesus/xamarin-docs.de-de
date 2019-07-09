---
title: Nutzen Sie einen Windows Communication Foundation (WCF)-Webdienst
description: In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: c79dd6430d387d75acfa010e7f5ad01829f8a6f0
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658937"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Nutzen Sie einen Windows Communication Foundation (WCF)-Webdienst

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen. In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird._

WCF beschreibt einen Dienst mit einer Vielzahl von verschiedene Verträge, einschließlich:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt in einer Nachricht zu bilden.
- **Nachrichtenverträge** – Erstellen von Nachrichten von vorhandene Datenverträge.
- **Fehler Verträge** – ermöglichen von benutzerdefinierten SOAP-Fehlern angegeben werden.
- **-Dienstverträge** : Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem Verhalten benutzerdefinierter Fehler, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber WCF unterstützt die gleichen Funktionen, die ASMX bereitgestellt werden: SOAP-Nachrichten über HTTP. Weitere Informationen zu einen ASMX-Dienst zu nutzen, finden Sie unter [Nutzen der ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Die Xamarin-Plattform-Unterstützung für WCF ist gegenüber der Verwendung von HTTP/HTTPS-textcodierten SOAP-Nachrichten auf der `BasicHttpBinding` Klasse.
>
> Unterstützung der WCF erfordert die Verwendung von Tools, die nur in einer Windows-Umgebung zum Generieren des Proxys, und hosten die TodoWCFService verfügbar. Bereitstellen der TodoWCFService auf einem Windows-Computer oder als Azure-Webdienst benötigen erstellen und testen die iOS-app.
>
> Native Xamarin Forms-apps Freigeben von Code in der Regel mit einer .NET Standard-Klassenbibliothek. Allerdings unterstützt .NET Core derzeit WCF nicht damit das freigegebene Projekt eine ältere Portable Klassenbibliothek sein muss. Weitere Informationen zur Unterstützung von WCF in .NET Core finden Sie unter [Wahl zwischen .NET Core und .NET Framework für Server-apps](/dotnet/standard/choosing-core-framework-server).

Die beispiellösung für die Anwendung enthält einen WCF-Dienst kann lokal ausgeführt werden und wird im folgenden Screenshot dargestellt:

![](wcf-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
>
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Nutzen des Webdiensts

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

### <a name="create-the-todoserviceclient-object"></a>Erstellen Sie das TodoServiceClient-Objekt

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

### <a name="create-data-transfer-objects"></a>Erstellen von Objekten für die Datenübertragung

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

## <a name="configure-remote-access-to-iis-express"></a>Konfigurieren des Remotezugriffs für IIS Express
In Visual Studio 2017 oder Visual Studio-2019 sollten Sie die UWP-Anwendung auf einem Computer ohne zusätzliche Konfiguration testen können. Testen von Android und iOS-Clients möglicherweise die zusätzlichen Schritte in diesem Abschnitt. Finden Sie unter [Herstellen einer Verbindung mit lokalen Webdiensten von iOS-Simulatoren und Android-Emulatoren](~/cross-platform/deploy-test/connect-to-local-web-services.md) für Weitere Informationen.

Standardmäßig IIS Express wird nur dann Antworten auf Anforderungen zum `localhost`. Remote-Geräte (z. B. ein Android-Gerät, um ein iPhone oder sogar einen Simulator) werden nicht auf den lokalen WCF-Dienst zugreifen. Sie müssen Ihre Windows 10-Arbeitsstation IP-Adresse im lokalen Netzwerk kennen. In diesem Beispiel wird davon ausgegangen, dass es sich bei Ihrer Arbeitsstation auf die IP-Adresse `192.168.1.143`. Die folgenden Schritte erläutern, wie Sie Windows 10 und IIS Express, um die Annahme von Remoteverbindungen, und Verbinden mit dem Dienst von einem physischen oder virtuellen Gerät zu konfigurieren:

1. **Fügen Sie eine Ausnahme für die Windows-Firewall**. Sie müssen einen Port über die Windows-Firewall öffnen, die Anwendungen im Subnetz für die Kommunikation mit den WCF-Dienst verwenden können. Erstellen Sie eine eingehende Regel Port 49393 in der Firewall öffnen. Führen Sie an einer administrativen Eingabeaufforderung folgenden Befehl ein:
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Konfigurieren Sie IIS Express, beim Annehmen von Remoteverbindungen**. Sie können die IIS Express konfigurieren, durch Bearbeiten der Konfigurationsdatei für IIS Express unter **[Projektmappenverzeichnis]\.vs\config\applicationhost.config**. Suchen der `site` Element mit dem Namen `TodoWCFService`. Es sollte ähnlich der folgenden XML-Code aussehen:

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

    Sie müssen zwei hinzufügen `binding` Elemente öffnen Sie port 49393 externen Datenverkehr und Android-Emulator. Die Bindung verwendet ein `[IP address]:[port]:[hostname]` Format, der angibt, wie IIS Express auf Anforderungen reagiert. Externe Anforderungen müssen den Hostnamen, die als angegeben werden müssen eine `binding`. Hinzufügen der folgenden XML-Code der `bindings` -Element, und Ersetzen Sie die IP-Adresse durch Ihre eigenen IP-Adresse:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    Nach dem Ändern der `bindings` -Element sollte wie folgt aussehen:

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
    >Standardmäßig akzeptiert IIS Express nicht Verbindungen von externen Quellen aus Sicherheitsgründen. Zum Aktivieren von Verbindungen von Remotegeräten müssen Sie IIS Express mit Administratorberechtigungen ausführen. Die einfachste Möglichkeit hierzu ist die Ausführung von Visual Studio 2017 mit Administratorberechtigungen. Dadurch wird IIS Express mit Administratorberechtigungen gestartet, bei der Ausführung der TodoWCFService.

    Mit diesen Schritten, der abgeschlossen ist sollten Sie in der Lage, führen Sie die TodoWCFService und Herstellen einer Verbindung von anderen Geräten im Subnetz. Sie können dies testen, indem Sie Ausführung Ihrer Anwendung und das Aufrufen von `http://localhost:49393/TodoService.svc`. Wenn Sie erhalten eine **Bad Request** Fehler beim Zugriff auf diese URL und Ihre `bindings` ist möglicherweise nicht korrekt in der IIS Express-Konfiguration (die IIS Express erreicht die Anforderung aber zurückgewiesen). Wenn Sie einen anderen Fehler erhalten, kann es sein, dass Ihre Anwendung nicht ausgeführt wird oder die Firewall ist nicht ordnungsgemäß konfiguriert.

    Um IIS Express ausgeführt wird und der Bereitstellung des Diensts zu ermöglichen, deaktivieren Sie die **bearbeiten und Fortfahren** option **Projekteigenschaften > Web > Debugger**.

1. **Anpassen des Endpunkts, die Geräte verwenden, um Zugriff auf den Dienst**. Dieser Schritt umfasst die Konfiguration der Clientanwendung, auf einem physischen oder emulierten Gerät, auf den WCF-Dienst zuzugreifen.

    Android-Emulator verwendet eine interne Proxyserver, der verhindert, dass den Emulator direkten Zugriff auf dem Hostcomputer `localhost` Adresse. Stattdessen die Adresse `10.0.2.2` auf dem Emulator geleitet wird `localhost` auf dem Hostcomputer über einen internen Proxy. Diese Proxyanforderungen müssen `127.0.0.1` als Hostname im Anforderungsheader ist Sie die IIS Express-Bindung für diesen Hostnamen in den vorherigen Schritten erstellt haben.

    Die iOS-Simulator ausgeführt, auf einem Mac wird-buildhost, selbst bei Verwendung der [remoten iOS-Simulator für Windows](~/tools/ios-simulator/index.md). Netzwerkanforderungen des Simulators müssen Ihre Arbeitsstation IP-Adresse im lokalen Netzwerk als Hostnamen (in diesem Beispiel hat `192.168.1.143`, aber die tatsächliche IP-Adresse werden Sie wahrscheinlich andere). Daher ist Sie die IIS Express-Bindung für diesen Hostnamen in den vorherigen Schritten erstellt haben.

    Stellen Sie sicher die `SoapUrl` -Eigenschaft in der **"Constants.cs"** Datei im Projekt TodoWCF (portabel) Werte aufweisen, die für Ihr Netzwerk korrekt sind:

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

    Nachdem Sie konfiguriert haben die **"Constants.cs"** mit den entsprechenden-Endpunkten, Sie muss die Verbindung mit der TodoWCFService aus physischen oder virtuellen Geräte in Ihrer Windows 10-Arbeitsstation ausgeführt wird.

## <a name="related-links"></a>Verwandte Links

- [TodoWCF (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [Vorgehensweise: Erstellen eines Windows Communication Foundation-Clients](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
