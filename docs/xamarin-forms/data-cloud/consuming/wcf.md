---
title: Nutzen einen Windows Communication Foundation (WCF)-Webdienst
description: WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Es ermöglicht Entwicklern, sichere, zuverlässige, transaktive und interoperable verteilte Anwendungen zu erstellen. Dieser Artikel veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst über eine Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 23cdc1871511fa75ba2686213d135822ca0fb971
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Nutzen einen Windows Communication Foundation (WCF)-Webdienst

_WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Es ermöglicht Entwicklern, sichere, zuverlässige, transaktive und interoperable verteilte Anwendungen zu erstellen. Dieser Artikel veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst über eine Xamarin.Forms-Anwendung genutzt wird._

WCF beschreibt einen Dienst mit einer Vielzahl von verschiedene Verträge, darunter die folgenden:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt auf eine Nachricht zu bilden.
- **Nachrichtenverträge** – Verfassen Sie Nachrichten von vorhandene Datenverträge.
- **Verträge Fault** – benutzerdefinierte SOAP-Fehler angegeben werden können.
- **-Dienstverträgen** – Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem alle benutzerdefinierten Fehler Verhalten, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen, die ASMX bereitstellt – SOAP-Nachrichten über HTTP unterstützt. Weitere Informationen zum Verarbeiten einer ASMX-Diensts finden Sie unter [nutzen ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

Im Allgemeinen unterstützt die Xamarin-Plattform die gleiche clientseitige Teilmenge von WCF, die mit der Silverlight-Laufzeit bereitgestellt wird. Dies schließt die am häufigsten verwendeten Codierungs- und Protokolldetails Implementierungen von WCF-Text-codierte SOAP-Nachrichten über den HTTP-transport-Protokoll der `BasicHttpBinding` Klasse. WCF-Unterstützung erfordert außerdem die Verwendung von Tools nur in einer Windows-Umgebung für den Proxy zu generieren.

Anweisungen zum Einrichten des WCF-Diensts können in der Readme-Datei gefunden werden, die die beispielanwendung gehören. Jedoch beim Ausführen der beispielanwendung werden es verbunden, um eine Xamarin-gehosteten WCF-Dienst, der nur-Lese Zugriff auf Daten bereitstellt wie im folgenden Screenshot gezeigt:

![](wcf-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Den Webdienst

Der WCF-Dienst stellt die folgenden Vorgänge:

|Vorgang|Beschreibung|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Ein neues Aufgabenelement erstellen|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zu dem in der Anwendung verwendeten Datenmodell finden Sie unter [die Daten modellieren,](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Die beispielanwendung nutzt die Xamarin-gehosteten WCF-Dienst, der nur-Lese Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten die in der Anwendung verwendeten Daten nicht geändert. Eine hostfähige Version von der ASMX-Dienst ist jedoch verfügbar, in der **TodoWCFService** Ordner in der zugehörigen beispielanwendung. Diese hostfähigen Version von den WCF-Dienst ermöglicht vollständige erstellen, aktualisieren, lesen und löschen den Zugriff auf die Daten.

Ein *Proxy* muss generiert werden, um einen WCF-Dienst zu nutzen, die die Anwendung für die Verbindung mit dem Dienst ermöglicht. Der Proxy erstellt wird, nutzen Dienstmetadaten, die die Methoden und die zugehörigen Dienstkonfiguration zu definieren. Diese Metadaten wird in Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die durch den Webdienst generiert wird. Der Proxy kann mithilfe von Microsoft WCF Web Service-Reference-Anbieter in Visual Studio 2017 zum Hinzufügen eines Dienstverweises für den Webdienst in einer standardmäßigen .NET Bibliothek erstellt werden. Eine Alternative zum Erstellen des Proxys, die mit dem Microsoft WCF Web Verweis Dienstanbieter in Visual Studio 2017 wird das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Die generierte Proxyklassen enthalten Methoden zum Verarbeiten der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. Bei diesem Muster wird ein asynchroner Vorgang als zwei Methoden namens implementiert *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang, und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread stattfindet.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* um die Ergebnisse des Vorgangs abzurufen. Der Rückgabewert der *EndOperationName* ist der gleiche Typ von synchronen Webdienstmethode zurückgegeben. Z. B. die `EndGetTodoItems` -Methode gibt eine Auflistung von `TodoItem` Instanzen. Die *EndOperationName* -Methode enthält auch eine `IAsyncResult` Parameter, um die vom entsprechenden Aufruf zurückgegebene Instanz festgelegt werden sollte, die *BeginOperationName* Methode.

Die Task Parallel Library (TPL) kann vereinfachen das nutzen ein APM begin/End-Methodenpaar von kapseln die asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung von mehrere Überladungen der dient den `TaskFactory.FromAsync` Methode.

Weitere Informationen zu APM finden Sie unter [asynchronen Programmierungsmodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="creating-the-todoserviceclient-object"></a>Beim Erstellen des Objekts TodoServiceClient

Die generierte Proxyklasse bietet die `TodoServiceClient` -Klasse, die Kommunikation mit dem WCF-Dienst über HTTP verwendet wird. Sie bietet Funktionen zum Aufrufen von Methoden des Webdiensts als asynchrone Vorgänge aus einem URI Dienstinstanz identifiziert. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Überblick über den asynchronen](~/cross-platform/platform/async.md).

Die `TodoServiceClient` Instanz wird auf der Klassenebene deklariert, sodass für das Objekt aktiv ist, solange die Anwendung den WCF-Dienst nutzen, wie im folgenden Codebeispiel gezeigt muss:

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

Die `TodoServiceClient` -Instanz mit einer Bindung Informationen und eine Endpunktadresse konfiguriert ist. Eine Bindung an die Transport-, Codierungs- und Protokolldetails erforderlich für Anwendungen und Dienste zur Kommunikation mit anderen verwendet. Die `BasicHttpBinding` gibt an, dass der Text-codierte SOAP-Nachrichten über das HTTP-Transportprotokoll gesendet werden. Angeben einer Endpunktadresse ermöglicht der Anwendung eine Verbindung zu verschiedenen Instanzen der den WCF-Dienst herstellen, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

Weitere Informationen zu den Dienstverweis konfigurieren, finden Sie unter [den Dienstverweis konfigurieren](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="creating-data-transfer-objects"></a>Erstellen von Datenobjekten Übertragung

Die beispielanwendung verwendet die `TodoItem` Klasse für die Modelldaten. Zum Speichern einer `TodoItem` Element im Webdienst Sie zunächst in den generierten Proxy konvertiert werden müssen `TodoItem` Typ. Dies wird erreicht, indem die `ToWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Diese Methode erstellt einfach eine neue `TodoWCFService.TodoItem` Serverinstanz aus, und legt jede Eigenschaft der identische Eigenschaft aus der `TodoItem` Instanz.

Auf ähnliche Weise beim Abrufen von Daten aus dem Webdienst, sie muss konvertiert werden aus den generierten Proxy `TodoItem` -Typ in einen `TodoItem` Instanz. Dies wird erreicht, mit der `FromWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Diese Methode ruft einfach die Daten aus den generierten Proxy `TodoItem` geben und wird in das neu erstellte `TodoItem` Instanz.

### <a name="retrieving-data"></a>Abrufen von Daten

Die `TodoServiceClient.BeginGetTodoItems` und `TodoServiceClient.EndGetTodoItems` Methoden zum Aufrufen der `GetTodoItems` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

Der `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausführt, der `TodoServiceClient.EndGetTodoItems` -Methode einmal die `TodoServiceClient.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben werden die `BeginGetTodoItems` delegieren. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die `TodoServiceClient.EndGetTodoItems` Methode gibt ein `ObservableCollection` von `TodoWCFService.TodoItem` -Instanzen, die dann in konvertiert wird eine `List` von `TodoItem` Instanzen für die Anzeige.

### <a name="creating-data"></a>Erstellen von Daten

Die `TodoServiceClient.BeginCreateTodoItem` und `TodoServiceClient.EndCreateTodoItem` Methoden zum Aufrufen der `CreateTodoItem` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndCreateTodoItem` -Methode einmal die `TodoServiceClient.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die übergeben werden die `BeginCreateTodoItem` Delegat, der die angeben`TodoItem` durch den Webdienst erstellt werden soll. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Löst die Web-Dienst eine `FaultException` Ausfall zum Erstellen der `TodoItem`, der von der Anwendung verarbeitet wird.

### <a name="updating-data"></a>Aktualisieren von Daten

Die `TodoServiceClient.BeginEditTodoItem` und `TodoServiceClient.EndEditTodoItem` Methoden zum Aufrufen der `EditTodoItem` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndEditTodoItem` -Methode einmal die `TodoServiceClient.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die übergeben werden die `BeginEditTodoItem` Delegat, der die angeben`TodoItem` durch den Webdienst aktualisiert werden. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die Web-Dienst löst eine `FaultException` Ausfall suchen oder aktualisiert die `TodoItem`, wird und der von der Anwendung verarbeitet.

### <a name="deleting-data"></a>Löschen von Daten

Die `TodoServiceClient.BeginDeleteTodoItem` und `TodoServiceClient.EndDeleteTodoItem` Methoden zum Aufrufen der `DeleteTodoItem` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndDeleteTodoItem` -Methode einmal die `TodoServiceClient.BeginDeleteTodoItem` Methode ausgeführt wird, mit der `id` Parameter werden die Daten, die übergeben werden die `BeginDeleteTodoItem` Delegat, der die angeben`TodoItem` durch den Webdienst gelöscht werden soll. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Löst die Web-Dienst eine `FaultException` Ausfall suchen und Löschen der `TodoItem`, der von der Anwendung verarbeitet wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie einen WCF-SOAP-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird. Im Allgemeinen unterstützt die Xamarin-Plattform die gleiche clientseitige Teilmenge von WCF, die mit der Silverlight-Laufzeit bereitgestellt wird. Dies schließt die am häufigsten verwendeten Codierungs- und Protokolldetails Implementierungen von WCF-Text-codierte SOAP-Nachrichten über den HTTP-transport-Protokoll der `BasicHttpBinding` Klasse. WCF-Unterstützung erfordert außerdem die Verwendung von Tools nur in einer Windows-Umgebung für den Proxy zu generieren.


## <a name="related-links"></a>Verwandte Links

- [TodoWCF (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
