---
title: Verwenden eines ASP.NET-Webdiensts (ASMX)
description: In diesem Artikel wird veranschaulicht, wie einem ASMX SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 73e7d13188c4c2ea1be39a237c3dc5cfd7fd8a7d
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659057"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>Verwenden eines ASP.NET-Webdiensts (ASMX)

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)

_ASMX ermöglicht das Erstellen von Webdiensten, die Nachrichten, die mithilfe der einfachen Objekt Zugriff Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten. In diesem Artikel wird veranschaulicht, wie einem ASMX SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung._

Eine SOAP-Nachricht ist ein XML-Dokument mit den folgenden Elementen:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Eine optionale *Header* -Element, das anwendungsspezifische Informationen z. B. Authentifizierungsdaten zu bewegen enthält. Wenn die *Header* Element vorhanden ist. es muss sich auf das erste untergeordnete Element des der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht den Empfänger enthält.
- Eine optionale *Fehler* -Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* -Element vorhanden ist, muss es ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, einschließlich HTTP, SMTP, TCP und UDP-ausgeführt werden. Allerdings kann ein ASMX-Dienst nur über HTTP verwendet werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies schließt die Unterstützung vieler den Standardkonfigurationen der ASMX-Dienst.

In diesem Beispiel enthält die mobilen Anwendungen, die auf physischen oder emulierte Geräte ausgeführt und einen ASMX-Dienst, der Methoden zum Abrufen, hinzufügen, bearbeiten und Löschen von Daten bereitstellt. Bei die mobilen Anwendungen ausgeführt werden, verbinden sie mit der ASMX-Dienst lokal gehostet, wie im folgenden Screenshot gezeigt:

![](asmx-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Nutzen des Webdiensts

Der ASMX-Dienst bietet die folgenden Vorgänge:

|Vorgang|Beschreibung|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Erstellt ein neues to-do-Element|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zu das Datenmodell in der Anwendung verwendet, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

## <a name="create-the-todoservice-proxy"></a>Erstellen des Proxys TodoService

Eine Proxyklasse, die Namen `TodoService`, erweitert `SoapHttpClientProtocol` und bietet Methoden für die Kommunikation mit der ASMX-Dienst über HTTP. Der Proxy wird generiert, durch das Hinzufügen von jedes plattformspezifische Projekt eines Webverweis in Visual Studio-2019 oder Visual Studio 2017. Der Webverweis wird generiert, Methoden und Ereignisse für jede Aktion in des Dokuments des Diensts Web Services Description Language (WSDL) definiert.

Z. B. die `GetTodoItems` Service-Aktion führt zu einer `GetTodoItemsAsync` Methode und eine `GetTodoItemsCompleted` Ereignis in den Proxy. Die generierte Methode verfügt über einen void-Rückgabetyp und ruft die `GetTodoItems` Aktion für das übergeordnete `SoapHttpClientProtocol` Klasse. Wenn die aufgerufene Methode eine Antwort vom Dienst empfängt, löst die `GetTodoItemsCompleted` Ereignis sowie die Antwortdaten innerhalb des Ereignisses `Result` Eigenschaft.

## <a name="create-the-isoapservice-implementation"></a>Erstellen Sie die ISoapService-Implementierung

Um das Projekt für freigegebenen, plattformübergreifende arbeiten Sie mit dem Dienst zu aktivieren, das Beispiel definiert die `ISoapService` Schnittstelle, die folgt die [asynchrone Task-Programmiermodell in C# ](/dotnet/csharp/programming-guide/concepts/async/). Die einzelnen Plattformen implementieren die `ISoapService` , die plattformspezifische Proxy verfügbar zu machen. Das Beispiel verwendet `TaskCompletionSource` Objekte, um den Proxy als asynchrone Task-Schnittstelle verfügbar zu machen. Informationen zur Verwendung von `TaskCompletionSource` befinden sich in den Implementierungen der einzelnen Aktion in den folgenden Abschnitten.

Das Beispiel `SoapService`:

1. Instanziiert die `TodoService` als eine Instanz auf Klassenebene
1. Erstellt eine Sammlung namens `Items` zum Speichern von `TodoItem` Objekte
1. Gibt an, einen benutzerdefinierten Endpunkt für das optionale `Url` Eigenschaft für die `TodoService`

```csharp
public class SoapService : ISoapService
{
    ASMXService.TodoService todoService;
    public List<TodoItem> Items { get; private set; } = new List<TodoItem>();

    public SoapService ()
    {
        todoService = new ASMXService.TodoService ();
        todoService.Url = Constants.SoapUrl;
        ...
    }
}
```

### <a name="create-data-transfer-objects"></a>Erstellen von Objekten für die Datenübertragung

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

Diese Methode erstellt ein neues `ASMService.TodoItem` Instanz, und legt jede Eigenschaft, die identische Eigenschaft aus der `TodoItem` Instanz.

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

Diese Methode ruft die Daten ab, aus den generierten Proxy `TodoItem` geben und wird in der neu erstellten `TodoItem` Instanz.

### <a name="retrieve-data"></a>Abrufen von Daten

Die `ISoapService` Schnittstelle erwartet, dass die `RefreshDataAsync` -Methode zur Rückgabe einer `Task` mit der Auflistung. Allerdings die `TodoService.GetTodoItemsAsync` Methode "void" zurück. Um das Muster "Dienstschnittstelle" erfüllen zu können, müssen Sie aufrufen `GetTodoItemsAsync`, warten, bis die `GetTodoItemsCompleted` Ereignis ausgelöst und das Füllen der Auflistung. Dadurch können Sie eine gültige Auflistung an der Benutzeroberfläche zurückgegeben.

Das folgende Beispiel erstellt ein neues `TaskCompletionSource`, beginnt den asynchronen Aufruf in die `RefreshDataAsync` -Methode, und wartet auf die `Task` gebotenen die `TaskCompletionSource`. Bei der `TodoService_GetTodoItemsCompleted` -Ereignishandler wird aufgerufen, füllt es die `Items` Auflistung und Updates der `TaskCompletionSource`:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> getRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.GetTodoItemsCompleted += TodoService_GetTodoItemsCompleted;
    }

    public async Task<List<TodoItem>> RefreshDataAsync()
    {
        getRequestComplete = new TaskCompletionSource<bool>();
        todoService.GetTodoItemsAsync();
        await getRequestComplete.Task;
        return Items;
    }

    private void TodoService_GetTodoItemsCompleted(object sender, ASMXService.GetTodoItemsCompletedEventArgs e)
    {
        try
        {
            getRequestComplete = getRequestComplete ?? new TaskCompletionSource<bool>();

            Items = new List<TodoItem>();
            foreach (var item in e.Result)
            {
                Items.Add(FromASMXServiceTodoItem(item));
            }
            getRequestComplete?.TrySetResult(true);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(@"\t\tERROR {0}", ex.Message);
        }
    }

    ...
}
```

Weitere Informationen finden Sie unter [asynchronen Programmiermodells](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming).

### <a name="create-or-edit-data"></a>Erstellen oder Bearbeiten von Daten

Wenn Sie Sie erstellen oder Bearbeiten von Daten, müssen Sie implementieren die `ISoapService.SaveTodoItemAsync` Methode. Diese Methode erkennt, ob die `TodoItem` ist eine neue oder aktualisierte-Element, und ruft die entsprechende Methode für die `todoService` Objekt. Die `CreateTodoItemCompleted` und `EditTodoItemCompleted` Ereignishandler sollte auch implementiert werden, damit Sie, wann wissen die `todoService` hat eine Antwort von der ASMX-Dienst (diese können kombiniert werden in einem einzelnen Handler, da der gleiche Vorgang ausgeführt). Das folgende Beispiel zeigt die Schnittstelle und Ereignis-Handler-Implementierungen, als auch die `TaskCompletionSource` Objekt verwendet, um asynchron ausgeführt werden:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> saveRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.CreateTodoItemCompleted += TodoService_SaveTodoItemCompleted;
        todoService.EditTodoItemCompleted += TodoService_SaveTodoItemCompleted;
    }

    public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
    {
        try
        {
            var todoItem = ToASMXServiceTodoItem(item);
            saveRequestComplete = new TaskCompletionSource<bool>();
            if (isNewItem)
            {
                todoService.CreateTodoItemAsync(todoItem);
            }
            else
            {
                todoService.EditTodoItemAsync(todoItem);
            }
            await saveRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_SaveTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        saveRequestComplete?.TrySetResult(true);
    }

    ...
}
```

### <a name="delete-data"></a>Löschen von Daten

Löschen von Daten erfordert etwas Ähnliches implementiert. Definieren einer `TaskCompletionSource`, implementieren Sie einen Ereignishandler, und die `ISoapService.DeleteTodoItemAsync` Methode:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> deleteRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.DeleteTodoItemCompleted += TodoService_DeleteTodoItemCompleted;
    }

    public async Task DeleteTodoItemAsync (string id)
    {
        try
        {
            deleteRequestComplete = new TaskCompletionSource<bool>();
            todoService.DeleteTodoItemAsync(id);
            await deleteRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_DeleteTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        deleteRequestComplete?.TrySetResult(true);
    }

    ...
}
```

## <a name="test-the-web-service"></a>Testen des Webdiensts

Testen die physische oder emulierte Geräte mit einem lokal gehosteten Dienst ist erforderlich, benutzerdefinierte IIS-Konfiguration, Endpunktadressen und Firewall-Regeln vorhanden sein. Weitere Informationen zum Einrichten der Umgebung zum Testen finden Sie unter den [Konfigurieren des Remotezugriffs für IIS Express](wcf.md#configure-remote-access-to-iis-express). Der einzige Unterschied zwischen dem Testen von WCF und ASMX ist die Portnummer der TodoService.

## <a name="related-links"></a>Verwandte Links

- [TodoASMX (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
