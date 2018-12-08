---
title: Verwenden eines ASP.NET-Webdiensts (ASMX)
description: In diesem Artikel wird veranschaulicht, wie einem ASMX SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1fa2fee36619a2a443d84b861b2473a1f616ed0e
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055140"
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Verwenden eines ASP.NET-Webdiensts (ASMX)

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)

_ASMX ermöglicht das Erstellen von Webdiensten, die Nachrichten, die mithilfe der einfachen Objekt Zugriff Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten. In diesem Artikel wird veranschaulicht, wie einem ASMX SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung._

Eine SOAP-Nachricht ist ein XML-Dokument mit den folgenden Elementen:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Eine optionale *Header* -Element, das anwendungsspezifische Informationen z. B. Authentifizierungsdaten zu bewegen enthält. Wenn die *Header* Element vorhanden ist. es muss sich auf das erste untergeordnete Element des der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht den Empfänger enthält.
- Eine optionale *Fehler* -Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* -Element vorhanden ist, muss es ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, einschließlich HTTP, SMTP, TCP und UDP-ausgeführt werden. Allerdings kann ein ASMX-Dienst nur über HTTP verwendet werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies schließt die Unterstützung vieler den Standardkonfigurationen der ASMX-Dienst.

Anweisungen zum Einrichten des ASMX-Diensts können in der Readme-Datei gefunden werden, die mit der beispielanwendung. Jedoch, wenn die beispielanwendung ausgeführt wird, wird sie verbinden ein Xamarin-gehosteten ASMX-Dienst, der nur-Lese Zugriff auf Daten bereitstellt, wie im folgenden Screenshot gezeigt:

![](asmx-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Nutzung des Webdiensts

Der ASMX-Dienst bietet die folgenden Vorgänge:

|Vorgang|Beschreibung|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Erstellt ein neues to-do-Element|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zu das Datenmodell in der Anwendung verwendet, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Die beispielanwendung nutzt den Xamarin-gehosteten ASMX-Dienst, der nur-Lese-Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten nicht geändert werden die Daten in der Anwendung genutzt. Eine hostfähigen Version von der ASMX-Dienst ist jedoch verfügbar, in der **TodoASMXService** Ordner in der zugehörigen beispielanwendung. Diese hostfähigen Version von der ASMX-Dienst ermöglicht vollständige erstellen, aktualisieren, lesen und löschen den Zugriff auf die Daten.

Ein *Proxy* muss generiert werden, um die ASMX-Dienst nutzen, die der Anwendung ermöglicht, mit dem Dienst herstellen. Der Proxy wird vom nutzenden Dienst-Metadaten erstellt, die die Methoden und zugeordnete Dienstkonfiguration definiert. Diese Metadaten werden in die Form eines Web Services Description Language (WSDL)-Dokuments verfügbar gemacht, die vom Webdienst generiert wird. Der Proxy wird durch das Hinzufügen eines Webverweises für den Webdienst zu den plattformspezifischen Projekten erstellt.

Die generierten Proxyklassen bieten Methoden für die Nutzung des Webdiensts, die das asynchrone Programmiermodell (APM)-Entwurfsmuster zu verwenden. In diesem Muster wird ein asynchroner Vorgang implementiert, als zwei Methoden namens *BeginOperationName* und *EndOperationName*, das Starten und beenden Sie den asynchronen Vorgang.

Die *BeginOperationName* Methode startet den asynchronen Vorgang und gibt ein Objekt, implementiert die `IAsyncResult` Schnittstelle. Nach dem Aufruf *BeginOperationName*, eine Anwendung kann weiterhin Ausführen von Anweisungen für den aufrufenden Thread, während der asynchrone Vorgang in einem Threadpoolthread ausgeführt wird.

Für jeden Aufruf von *BeginOperationName*, die Anwendung sollte auch aufrufen *EndOperationName* zum Abrufen der Ergebnisse des Vorgangs. Der Rückgabewert von *EndOperationName* ist der gleiche Typ zurückgegeben, die von der synchronen Webdienstmethode. Z. B. die `EndGetTodoItems` Methode gibt eine Auflistung von `TodoItem` Instanzen. Die *EndOperationName* Methode schließt auch eine `IAsyncResult` Parameter, um die vom entsprechenden Aufruf zurückgegebene Instanz festgelegt werden, sollte, die *BeginOperationName* Methode.

Die Task Parallel Library (TPL) vereinfachen kann ein APM begin/End-Methodenpaar zu nutzen, durch die Kapselung der asynchronen Vorgänge in der gleichen `Task` Objekt. Diese Kapselung durch mehrere Überladungen der dient den `TaskFactory.FromAsync` Methode.

Weitere Informationen zu APM finden Sie unter [asynchronen Programmiermodells](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) auf MSDN.

### <a name="creating-the-todoservice-object"></a>Erstellen des Objekts TodoService

Die generierte Proxyklasse enthält die `TodoService` -Klasse, die Kommunikation mit dem ASMX-Dienst über HTTP verwendet wird. Es stellt Funktionen bereit, für das Aufrufen von Webdienstmethoden, wie asynchrone Vorgänge aus einem URI-Dienstinstanz identifiziert. Weitere Informationen zu asynchronen Vorgängen finden Sie unter [Async-Unterstützung (Übersicht)](~/cross-platform/platform/async.md).

Die `TodoService` Instanz wird auf der Klassenebene deklariert, sodass für das Objekt befindet, solange die Anwendung die ASMX-Dienst nutzen, wie im folgenden Codebeispiel wird gezeigt muss:

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

Die `TodoService` Konstruktor akzeptiert einen optionale Zeichenfolge-Parameter, der die URL der ASMX-Dienstinstanz angibt. Dadurch kann es sich um die Anwendung für verschiedene Instanzen der ASMX-Dienst die Verbindung, vorausgesetzt, dass mehrere veröffentlichte Instanzen vorhanden sind.

### <a name="creating-data-transfer-objects"></a>Erstellen von Data Transfer Objects

Die beispielanwendung verwendet die `TodoItem` Klasse, um die Daten des Modells. Zum Speichern einer `TodoItem` Element in den Webdienst, sie zuerst in den generierten Proxy konvertiert werden müssen `TodoItem` Typ. Dies erfolgt durch die `ToASMXServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode erstellt einfach ein neues `ASMService.TodoItem` Instanz, und legt jede Eigenschaft, die identische Eigenschaft aus der `TodoItem` Instanz.

Auf ähnliche Weise, wenn Daten aus dem Webdienst abgerufen werden, muss er sein konvertiert aus den generierten Proxy `TodoItem` Typ in einen `TodoItem` Instanz. Dies erfolgt mit der `FromASMXServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode ruft einfach die Daten ab, aus den generierten Proxy `TodoItem` geben und wird in der neu erstellten `TodoItem` Instanz.

### <a name="retrieving-data"></a>Abrufen von Daten

Die `TodoService.BeginGetTodoItems` und `TodoService.EndGetTodoItems` Methoden zum Aufrufen der `GetTodoItems` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndGetTodoItems` -Methode einmal die `TodoService.BeginGetTodoItems` Methode ausgeführt wird, mit der `null` Parameter, der angibt, dass keine Daten an übergeben wird, werden die `BeginGetTodoItems` delegieren. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Die `TodoService.EndGetTodoItems` Methode gibt ein Array von `ASMXService.TodoItem` -Instanzen, die anschließend in konvertiert wird eine `List` von `TodoItem` Instanzen für die Anzeige.

### <a name="creating-data"></a>Erstellen von Daten

Die `TodoService.BeginCreateTodoItem` und `TodoService.EndCreateTodoItem` Methoden zum Aufrufen der `CreateTodoItem` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndCreateTodoItem` -Methode einmal die `TodoService.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die in übergeben werden die `BeginCreateTodoItem` Delegat, um die `TodoItem` vom Webdienst erstellt werden. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Löst die Web-Dienst eine `SoapException` Ausfall zum Erstellen der `TodoItem`, die von der Anwendung erfolgt.

### <a name="updating-data"></a>Aktualisieren von Daten

Die `TodoService.BeginEditTodoItem` und `TodoService.EndEditTodoItem` Methoden zum Aufrufen der `EditTodoItem` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndEditTodoItem` -Methode einmal die `TodoService.BeginCreateTodoItem` Methode ausgeführt wird, mit der `todoItem` Parameter werden die Daten, die in übergeben werden die `BeginEditTodoItem` Delegat, um die `TodoItem` vom Webdienst aktualisiert werden. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der Web-Dienst löst eine `SoapException` bei einem Fehler zu suchen, oder Aktualisieren der `TodoItem`, die von der Anwendung erfolgt.

### <a name="deleting-data"></a>Löschen von Daten

Die `TodoService.BeginDeleteTodoItem` und `TodoService.EndDeleteTodoItem` Methoden zum Aufrufen der `DeleteTodoItem` Operation, die vom Webdienst bereitgestellt. Diese asynchronen Methoden in gekapselt sind eine `Task` Objekt, wie im folgenden Codebeispiel gezeigt:

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

Die `Task.Factory.FromAsync` -Methode erstellt eine `Task` , ausgeführt wird die `TodoService.EndDeleteTodoItem` -Methode einmal die `TodoService.BeginDeleteTodoItem` Methode ausgeführt wird, mit der `id` Parameter werden die Daten, die in übergeben werden die `BeginDeleteTodoItem` Delegat, um die `TodoItem` vom Webdienst gelöscht werden soll. Zum Schluss den Wert, der die `TaskCreationOptions` -Enumeration gibt an, dass das Standardverhalten für die Erstellung und Ausführung von Aufgaben verwendet werden soll.

Der Web-Dienst löst eine `SoapException` bei einem Fehler zu suchen, oder Löschen der `TodoItem`, die von der Anwendung erfolgt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung zu nutzen. ASMX ermöglicht das Erstellen von Webdiensten, die für das Senden von Nachrichten über HTTP, SOAP verwenden. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten.


## <a name="related-links"></a>Verwandte Links

- [TodoASMX (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
