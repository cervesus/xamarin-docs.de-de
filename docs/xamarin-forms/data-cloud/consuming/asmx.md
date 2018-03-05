---
title: Verarbeiten eines ASP.NET-Webdiensts (ASMX)
description: "ASMX bietet die Möglichkeit, Webdienste erstellen, die Nachrichten, die mit einfachen Objekt Access Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll zum Erstellen von und Zugreifen auf Webdienste. Consumer von ASMX-Dienst müssen nicht alles Plattform, das Objektmodell oder Programmiersprache ab, die zum Implementieren des Diensts kennen. Sie müssen verstehen, wie SOAP-Nachrichten senden und empfangen. In diesem Artikel veranschaulicht, wie einen ASMX-SOAP-Dienst aus einer Xamarin.Forms-Anwendung zu nutzen."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 3362744d0d201ef82c846c80b0e1a87426953c85
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Verarbeiten eines ASP.NET-Webdiensts (ASMX)

_ASMX bietet die Möglichkeit, Webdienste erstellen, die Nachrichten, die mit einfachen Objekt Access Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll zum Erstellen von und Zugreifen auf Webdienste. Consumer von ASMX-Dienst müssen nicht alles Plattform, das Objektmodell oder Programmiersprache ab, die zum Implementieren des Diensts kennen. Sie müssen verstehen, wie SOAP-Nachrichten senden und empfangen. In diesem Artikel veranschaulicht, wie einen ASMX-SOAP-Dienst aus einer Xamarin.Forms-Anwendung zu nutzen._

Eine SOAP-Nachricht ist ein XML-Dokument enthält die folgenden Elemente:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Ein optionales *Header* Element, das anwendungsspezifische Informationen wie z. B. Authentifizierungsdaten enthält. Wenn die *Header* -Element vorhanden ist muss er das erste untergeordnete Element von der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht, die der Empfänger enthält.
- Ein optionales *Fehler* Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* Element vorhanden ist, muss er ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, z. B. HTTP, SMTP, TCP und UDP ausgeführt werden. Ein ASMX-Dienst kann jedoch nur über HTTP ausgeführt werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies umfasst Unterstützung für viele den Standardkonfigurationen der ASMX-Dienst.

Anweisungen zum Einrichten des ASMX-Diensts können in der Readme-Datei gefunden werden, die die beispielanwendung gehören. Jedoch wenn die beispielanwendung ausgeführt wird, wird es Verbinden mit einem Xamarin-gehostete ASMX-Dienst, der nur-Lese Zugriff auf Daten bereitstellt wie im folgenden Screenshot gezeigt:

![](asmx-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Den Webdienst

Der ASMX-Dienst stellt die folgenden Vorgänge:

<table>
  <thead>
    <tr>
      <th>Vorgang</th>
      <th>Beschreibung</th>
      <th>Parameter</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GetTodoItems</td>
      <td>Abrufen einer Liste von Aufgaben</td>
      <td></td>
    </tr>
    <tr>
      <td>CreateTodoItem</td>
      <td>Ein neues Aufgabenelement erstellen</td>
      <td>Ein XML-serialisierten <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>EditTodoItem</td>
      <td>Aktualisieren einer Aufgabe</td>
      <td>Ein XML-serialisierten <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>DeleteTodoItem</td>
      <td>Löschen einer Aufgabe</td>
      <td>Ein XML-serialisierten <code>TodoItem</code></td>
    </tr>
  </tbody>
</table>

Weitere Informationen zu dem in der Anwendung verwendeten Datenmodell finden Sie unter [die Daten modellieren,](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> **Hinweis**: die beispielanwendung nutzt den Xamarin-gehostete ASMX-Dienst, der nur-Lese Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten die in der Anwendung verwendeten Daten nicht geändert. Eine hostfähige Version von der ASMX-Dienst ist jedoch verfügbar, in der **TodoASMXService** Ordner in der zugehörigen beispielanwendung. Dieser hostfähigen Version der ASMX-Dienst ermöglicht vollständige erstellen, aktualisieren, lesen und löschen den Zugriff auf die Daten.

Ein *Proxy* muss generiert werden, um der ASMX-Dienst zu nutzen, die die Anwendung für die Verbindung mit dem Dienst ermöglicht. Der Proxy wird vom verbrauchende Dienstmetadaten erstellt, die die Methoden und die zugehörigen Dienstkonfiguration definiert. Diese Metadaten wird in Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die durch den Webdienst generiert wird. Der Proxy wird durch das Hinzufügen eines Webverweises für den Webdienst zu den plattformspezifischen Projekten erstellt.

Die generierte Proxyklassen bieten Methoden für den Webdienst, die das asynchrone Programmiermodell (APM)-Entwurfsmuster verwenden. In diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang, und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread stattfindet.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* um die Ergebnisse des Vorgangs abzurufen. Der Rückgabewert der *EndOperationName* ist der gleiche Typ von synchronen Webdienstmethode zurückgegeben. Z. B. die `EndGetTodoItems` -Methode gibt eine Auflistung von `TodoItem` Instanzen. Die *EndOperationName* -Methode enthält auch eine `IAsyncResult` Parameter, um die vom entsprechenden Aufruf zurückgegebene Instanz festgelegt werden sollte, die *BeginOperationName* Methode.

Die Task Parallel Library (TPL) kann vereinfachen das nutzen ein APM begin/End-Methodenpaar von kapseln die asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung von mehrere Überladungen der dient den `TaskFactory.FromAsync` Methode.

Weitere Informationen zu APM finden Sie unter [asynchronen Programmierungsmodell](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="creating-the-todoservice-object"></a>Beim Erstellen des Objekts TodoService

Die generierte Proxyklasse bietet die `TodoService` -Klasse, die Kommunikation mit der ASMX-Dienst über HTTP verwendet wird. Sie bietet Funktionen zum Aufrufen von Methoden des Webdiensts als asynchrone Vorgänge aus einem URI Dienstinstanz identifiziert. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Überblick über den asynchronen](~/cross-platform/platform/async.md).

Die `TodoService` Instanz wird auf der Klassenebene deklariert, sodass für das Objekt aktiv ist, solange die Anwendung die ASMX-Dienst zu nutzen, wie im folgenden Codebeispiel gezeigt muss:

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

Die `TodoService` Konstruktor akzeptiert einen optionalen Zeichenfolgenwert-Parameter, der die URL der ASMX-Dienstinstanz angibt. Dadurch kann die Anwendung für die Verbindung zu verschiedenen Instanzen der ASMX-Dienst, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

### <a name="creating-data-transfer-objects"></a>Erstellen von Datenobjekten Übertragung

Die beispielanwendung verwendet die `TodoItem` Klasse für die Modelldaten. Zum Speichern einer `TodoItem` Element im Webdienst Sie zunächst in den generierten Proxy konvertiert werden müssen `TodoItem` Typ. Dies wird erreicht, indem die `ToASMXServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Diese Methode erstellt einfach eine neue `ASMService.TodoItem` Serverinstanz aus, und legt jede Eigenschaft der identische Eigenschaft aus der `TodoItem` Instanz.

Auf ähnliche Weise beim Abrufen von Daten aus dem Webdienst, sie muss konvertiert werden aus den generierten Proxy `TodoItem` -Typ in einen `TodoItem` Instanz. Dies wird erreicht, mit der `FromASMXServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
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

Die `TodoService.BeginGetTodoItems` und `TodoService.EndGetTodoItems` Methoden zum Aufrufen der `GetTodoItems` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

Der `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausführt, der `TodoService.EndGetTodoItems` -Methode einmal die `TodoService.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben werden die `BeginGetTodoItems` delegieren. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die `TodoService.EndGetTodoItems` Methode gibt ein Array von `ASMXService.TodoItem` -Instanzen, die dann in konvertiert wird eine `List` von `TodoItem` Instanzen für die Anzeige.

### <a name="creating-data"></a>Erstellen von Daten

Die `TodoService.BeginCreateTodoItem` und `TodoService.EndCreateTodoItem` Methoden zum Aufrufen der `CreateTodoItem` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Die `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndCreateTodoItem` -Methode einmal die `TodoService.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die übergeben werden die `BeginCreateTodoItem` Delegat, der die angeben`TodoItem` durch den Webdienst erstellt werden soll. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Löst die Web-Dienst eine `SoapException` Ausfall zum Erstellen der `TodoItem`, der von der Anwendung verarbeitet wird.

### <a name="updating-data"></a>Aktualisieren von Daten

Die `TodoService.BeginEditTodoItem` und `TodoService.EndEditTodoItem` Methoden zum Aufrufen der `EditTodoItem` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Die `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndEditTodoItem` -Methode einmal die `TodoService.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die übergeben werden die `BeginEditTodoItem` Delegat, der die angeben`TodoItem` durch den Webdienst aktualisiert werden. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die Web-Dienst löst eine `SoapException` Ausfall suchen oder aktualisiert die `TodoItem`, wird und der von der Anwendung verarbeitet.

### <a name="deleting-data"></a>Löschen von Daten

Die `TodoService.BeginDeleteTodoItem` und `TodoService.EndDeleteTodoItem` Methoden zum Aufrufen der `DeleteTodoItem` Vorgang vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` -Objekts, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndDeleteTodoItem` -Methode einmal die `TodoService.BeginDeleteTodoItem` Methode ausgeführt wird, mit der `id` Parameter werden die Daten, die übergeben werden die `BeginDeleteTodoItem` Delegat, der die angeben`TodoItem` durch den Webdienst gelöscht werden soll. Schließlich den Wert, der die `TaskCreationOptions` Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Löst die Web-Dienst eine `SoapException` Ausfall suchen und Löschen der `TodoItem`, der von der Anwendung verarbeitet wird.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung nutzen. ASMX bietet die Möglichkeit, Webdienste, die Nachrichten über HTTP senden mit SOAP zu erstellen. Consumer von ASMX-Dienst müssen nicht alles Plattform, das Objektmodell oder Programmiersprache ab, die zum Implementieren des Diensts kennen. Sie müssen verstehen, wie SOAP-Nachrichten senden und empfangen.


## <a name="related-links"></a>Verwandte Links

- [TodoASMX (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
