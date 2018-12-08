---
title: Verwenden einen Windows Communication Foundation (WCF)-Webdienst
description: In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 7e8acc6e8aaf8b8e0e8cec7d5d0f3e28cf60073a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055594"
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Verwenden einen Windows Communication Foundation (WCF)-Webdienst

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen. In diesem Artikel wird veranschaulicht, wie einen WCF SOAP Simple Object Access Protocol ()-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird._

WCF wird einen Dienst mit einer Vielzahl von verschiedene Verträge, die im folgenden beschrieben:

- **Datenverträge** – definieren Sie die Datenstrukturen, die die Grundlage für den Inhalt in einer Nachricht zu bilden.
- **Nachrichtenverträge** – Erstellen von Nachrichten von vorhandene Datenverträge.
- **Fehler Verträge** – ermöglichen von benutzerdefinierten SOAP-Fehlern angegeben werden.
- **-Dienstverträge** : Geben Sie die Vorgänge, die Dienste unterstützen und die Nachrichten, die für die Interaktion mit jedem Vorgang erforderlich sind. Sie geben außerdem Verhalten benutzerdefinierter Fehler, die Vorgänge für jeden Dienst zugeordnet werden kann.

Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen, die ASMX bereitgestellt werden: SOAP-Nachrichten über HTTP unterstützt. Weitere Informationen zu einen ASMX-Dienst zu nutzen, finden Sie unter [Nutzung der ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

Die Xamarin-Plattform unterstützt in der Regel die gleiche Client-Side Teilmenge von WCF, die in der Silverlight-Laufzeit enthalten ist. Dies schließt die am häufigsten verwendeten Codierungs- und Protokolldetails Implementierungen von WCF – textcodierten SOAP-Nachrichten über die HTTP-transport-Protokoll der `BasicHttpBinding` Klasse. Unterstützung der WCF erfordert außerdem die Verwendung von Tools, die nur verfügbar, in einer Windows-Umgebung in den Proxy zu generieren.

Anweisungen zum Einrichten des WCF-Diensts können in der Readme-Datei gefunden werden, die mit der beispielanwendung. Jedoch beim Ausführen der beispielanwendung wird es verbinden zu einem Xamarin-gehosteten WCF-Dienst, der nur-Lese Zugriff auf Daten bereitstellt wie im folgenden Screenshot gezeigt:

![](wcf-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Nutzung des Webdiensts

Der WCF-Dienst bietet die folgenden Vorgänge:

|Vorgang|Beschreibung|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Erstellt ein neues to-do-Element|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zu das Datenmodell in der Anwendung verwendet, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Die beispielanwendung nutzt den Xamarin-gehosteten WCF-Dienst, der schreibgeschützten Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten nicht geändert werden die Daten in der Anwendung genutzt. Eine hostfähigen Version von der ASMX-Dienst ist jedoch verfügbar, in der **TodoWCFService** Ordner in der zugehörigen beispielanwendung. Diese hostfähigen Version von den WCF-Dienst ermöglicht vollständige erstellen, aktualisieren, lesen und löschen den Zugriff auf die Daten.

Ein *Proxy* muss generiert werden, um einen WCF-Dienst nutzen, die der Anwendung ermöglicht, mit dem Dienst herstellen. Der Proxy wird erstellt, indem verarbeitende Dienstmetadaten, die die Methoden und zugeordnete Dienstkonfiguration zu definieren. Diese Metadaten werden in die Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die vom Webdienst generiert wird. Hinzufügen ein Dienstverweises für den Webdienst in einer .NET Standard-Bibliothek mit den Microsoft WCF Web Service Reference Provider in Visual Studio 2017, kann der Proxy erstellt werden. Eine Alternative zum Erstellen des Proxys verwenden den Microsoft WCF Web Service Reference Provider in Visual Studio 2017 ist das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Die generierten Proxyklassen bieten Methoden für die Nutzung der Webdienste, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. Bei diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Z. B. die `EndGetTodoItems` Methode gibt eine Auflistung von `TodoItem` Instanzen. Die *EndOperationName* Methode schließt auch eine `IAsyncResult` Parameter, um die vom entsprechenden Aufruf zurückgegebene Instanz festgelegt werden, sollte, die *BeginOperationName* Methode.

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `TaskFactory.FromAsync` Methode.

Weitere Informationen zu APM finden Sie unter [asynchronen Programmiermodells](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="creating-the-todoserviceclient-object"></a>Erstellen des Objekts TodoServiceClient

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

### <a name="creating-data-transfer-objects"></a>Erstellen von Data Transfer Objects

Die beispielanwendung verwendet die `TodoItem` Klasse, um die Daten des Modells. Zum Speichern einer `TodoItem` Element in den Webdienst, sie zuerst in den generierten Proxy konvertiert werden müssen `TodoItem` Typ. Dies erfolgt durch die `ToWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode erstellt einfach ein neues `TodoWCFService.TodoItem` Instanz, und legt jede Eigenschaft, die identische Eigenschaft aus der `TodoItem` Instanz.

Auf ähnliche Weise, wenn Daten aus dem Webdienst abgerufen werden, muss er sein konvertiert aus den generierten Proxy `TodoItem` Typ in einen `TodoItem` Instanz. Dies erfolgt mit der `FromWCFServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode ruft einfach die Daten ab, aus den generierten Proxy `TodoItem` geben und wird in der neu erstellten `TodoItem` Instanz.

### <a name="retrieving-data"></a>Abrufen von Daten

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

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoServiceClient.EndGetTodoItems` -Methode einmal die `TodoServiceClient.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben wird, werden die `BeginGetTodoItems` delegieren. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die `TodoServiceClient.EndGetTodoItems` Methode gibt ein `ObservableCollection` von `TodoWCFService.TodoItem` -Instanzen, die anschließend in konvertiert wird eine `List` von `TodoItem` Instanzen für die Anzeige.

### <a name="creating-data"></a>Erstellen von Daten

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

### <a name="updating-data"></a>Aktualisieren von Daten

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

### <a name="deleting-data"></a>Löschen von Daten

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

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie ein WCF-SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung. Die Xamarin-Plattform unterstützt in der Regel die gleiche Client-Side Teilmenge von WCF, die in der Silverlight-Laufzeit enthalten ist. Dies schließt die am häufigsten verwendeten Codierungs- und Protokolldetails Implementierungen von WCF – textcodierten SOAP-Nachrichten über die HTTP-transport-Protokoll der `BasicHttpBinding` Klasse. Unterstützung der WCF erfordert außerdem die Verwendung von Tools, die nur verfügbar, in einer Windows-Umgebung in den Proxy zu generieren.


## <a name="related-links"></a>Verwandte Links

- [TodoWCF (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
