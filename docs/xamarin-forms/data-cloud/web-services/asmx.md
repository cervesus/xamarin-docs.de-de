---
title: Verwenden eines ASP.NET-Webdiensts (ASMX)
description: In diesem Artikel wird veranschaulicht, wie einem ASMX SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 124cffaf827ebeb8109f3cd4ac4dfd2701e43f9c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656657"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>Verwenden eines ASP.NET-Webdiensts (ASMX)

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX ermöglicht das Erstellen von Webdiensten, die Nachrichten, die mithilfe der einfachen Objekt Zugriff Protocol (SOAP) senden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten. In diesem Artikel wird veranschaulicht, wie einem ASMX SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung._

Eine SOAP-Nachricht ist ein XML-Dokument mit den folgenden Elementen:

- Ein Stammelement mit dem Namen *Umschlag* , die das XML-Dokument als SOAP-Nachricht identifiziert.
- Eine optionale *Header* -Element, das anwendungsspezifische Informationen z. B. Authentifizierungsdaten zu bewegen enthält. Wenn die *Header* Element vorhanden ist. es muss sich auf das erste untergeordnete Element des der *Umschlag* Element.
- Ein erforderliches *Text* Element, das die SOAP-Nachricht den Empfänger enthält.
- Eine optionale *Fehler* -Element, das verwendet wird, um Fehlermeldungen anzuzeigen. Wenn die *Fehler* -Element vorhanden ist, muss es ein untergeordnetes Element von der *Text* Element.

SOAP kann über viele Transportprotokolle verwenden, einschließlich HTTP, SMTP, TCP und UDP-ausgeführt werden. Allerdings kann ein ASMX-Dienst nur über HTTP verwendet werden. Die Xamarin-Plattform unterstützt SOAP 1.1-standardimplementierungen über HTTP, und dies schließt die Unterstützung vieler den Standardkonfigurationen der ASMX-Dienst.

Dieses Beispiel enthält die mobilen Anwendungen, die auf physischen oder emulierten Geräten ausgeführt werden, sowie einen ASMX-Dienst, der Methoden zum herunter-, hinzufügen, bearbeiten und Löschen von Daten bereitstellt. Wenn die mobilen Anwendungen ausgeführt werden, stellen Sie eine Verbindung mit dem lokal gehosteten ASMX-Dienst her, wie im folgenden Screenshot zu sehen:

![](asmx-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Verwenden des Webdiensts

Der ASMX-Dienst bietet die folgenden Vorgänge:

|Vorgang|Beschreibung|Parameter|
|--- |--- |--- |
|GetTodoItems|Abrufen einer Liste von To-Do-Elementen|
|CreateTodoItem|Erstellt ein neues to-do-Element|Eine mithilfe von XML serialisierte TodoItem|
|EditTodoItem|Aktualisieren eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|
|DeleteTodoItem|Löschen eines To-Do-Elements|Eine mithilfe von XML serialisierte TodoItem|

Weitere Informationen zu das Datenmodell in der Anwendung verwendet, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

## <a name="create-the-todoservice-proxy"></a>Erstellen des Service Proxy-Proxys

Eine Proxy Klasse, die `TodoService`aufgerufen wird `SoapHttpClientProtocol` , erweitert und stellt Methoden für die Kommunikation mit dem ASMX-Dienst über HTTP bereit. Der Proxy wird generiert, indem jedem plattformspezifischen Projekt in Visual Studio 2019 oder Visual Studio 2017 ein Webverweis hinzugefügt wird. Der Webverweis generiert Methoden und Ereignisse für jede Aktion, die im WSDL-Dokument (Service-Web Services Description Language) definiert ist.

Beispielsweise führt die `GetTodoItems` Dienst Aktion zu einer `GetTodoItemsAsync` -Methode und einem `GetTodoItemsCompleted` -Ereignis im Proxy. Die generierte Methode hat einen void-Rückgabetyp `GetTodoItems` und ruft die Aktion `SoapHttpClientProtocol` für die übergeordnete Klasse auf. Wenn die aufgerufene Methode eine Antwort vom Dienst empfängt, wird das `GetTodoItemsCompleted` -Ereignis ausgelöst, und die Antwortdaten werden in der- `Result` Eigenschaft des Ereignisses bereitstellt.

## <a name="create-the-isoapservice-implementation"></a>Erstellen der isoapservice-Implementierung

Damit das freigegebene, plattformübergreifende Projekt mit dem Dienst zusammenarbeiten kann, wird im Beispiel `ISoapService` die-Schnittstelle definiert, die dem [asynchronen Programmier C#Modell der Aufgabe in ](/dotnet/csharp/programming-guide/concepts/async/)folgt. Jede Plattform implementiert den `ISoapService` , um den plattformspezifischen Proxy verfügbar zu machen. Im Beispiel werden `TaskCompletionSource` -Objekte verwendet, um den Proxy als asynchrone Aufgaben Schnittstelle verfügbar zu machen. Details zur Verwendung `TaskCompletionSource` von finden Sie in den Implementierungen der einzelnen Aktions Typen in den folgenden Abschnitten.

Das Beispiel `SoapService`:

1. Instanziiert `TodoService` als Instanz auf Klassenebene.
1. Erstellt eine Auflistung, `Items` die zum `TodoItem` Speichern von Objekten aufgerufen wird.
1. Gibt einen benutzerdefinierten Endpunkt für die `Url` optionale Eigenschaft auf dem`TodoService`

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

### <a name="create-data-transfer-objects"></a>Erstellen von Datenübertragungs Objekten

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

Mit dieser Methode wird eine `ASMService.TodoItem` neue-Instanz erstellt und jede Eigenschaft auf die identische-Eigenschaft `TodoItem` der-Instanz festgelegt.

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

Diese Methode ruft die Daten aus dem vom Proxy generierten `TodoItem` Typ ab und legt Sie in der neu `TodoItem` erstellten Instanz fest.

### <a name="retrieve-data"></a>Abrufen von Daten

Die `ISoapService` -Schnittstelle `RefreshDataAsync` erwartet, dass die `Task` -Methode einen mit der-Element Auflistung zurückgibt. Die `TodoService.GetTodoItemsAsync` Methode gibt jedoch "void" zurück. Um das Schnittstellen Muster zu erfüllen, müssen Sie `GetTodoItemsAsync`aufzurufen, auf `GetTodoItemsCompleted` das Auslösen des Ereignisses warten und die Auflistung auffüllen. Auf diese Weise können Sie eine gültige Sammlung an die Benutzeroberfläche zurückgeben.

Im folgenden Beispiel wird ein neuer `TaskCompletionSource`erstellt, der asynchrone-Befehl `RefreshDataAsync` in der-Methode gestartet `Task` und die von `TaskCompletionSource`bereitgestellten erwartet. Wenn der `TodoService_GetTodoItemsCompleted` Ereignishandler aufgerufen wird, füllt er die `Items` Auflistung auf und aktualisiert `TaskCompletionSource`Folgendes:

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

Weitere Informationen finden Sie unter [asynchrones Programmiermodell](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) und [TPL und herkömmliche .NET Framework asynchrone Programmierung](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming).

### <a name="create-or-edit-data"></a>Erstellen oder Bearbeiten von Daten

Beim Erstellen oder Bearbeiten von Daten müssen Sie die `ISoapService.SaveTodoItemAsync` -Methode implementieren. Diese Methode erkennt, ob `TodoItem` ein neues oder aktualisiertes Element ist, und ruft die entsprechende- `todoService` Methode für das-Objekt auf. Die `CreateTodoItemCompleted` Ereignis `EditTodoItemCompleted` Handler und sollten ebenfalls implementiert werden, `todoService` damit Sie wissen, wann eine Antwort vom ASMX-Dienst empfangen hat (diese können zu einem einzigen Handler kombiniert werden, da Sie denselben Vorgang ausführen). Im folgenden Beispiel werden die-Schnittstellen-und ereignishandlerimplementierungen `TaskCompletionSource` sowie das-Objekt veranschaulicht, das für den asynchronen Betrieb verwendet wird:

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

Das Löschen von Daten erfordert eine ähnliche Implementierung. Definieren Sie `TaskCompletionSource`eine, implementieren Sie einen Ereignishandler, `ISoapService.DeleteTodoItemAsync` und die-Methode:

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

Zum Testen physischer oder emulierten Geräte mit einem lokal gehosteten Dienst müssen benutzerdefinierte IIS-Konfiguration, Endpunkt Adressen und Firewallregeln vorhanden sein. Weitere Informationen zum Einrichten Ihrer Umgebung für Tests finden Sie unter [Konfigurieren des Remote Zugriffs auf IIS Express](wcf.md#configure-remote-access-to-iis-express). Der einzige Unterschied zwischen dem Testen von WCF und ASMX ist die Portnummer von "dedoservice".

## <a name="related-links"></a>Verwandte Links

- [TodoASMX (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
