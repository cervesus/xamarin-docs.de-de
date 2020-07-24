---
title: Verwenden eines ASP.NET-Webdiensts (ASMX)
description: In diesem Artikel wird veranschaulicht, wie Sie einen ASMX-SOAP-Dienst aus einer- Xamarin.Forms Anwendung nutzen.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: aa600974cdf25f8f85d9152edc4a377334cc8c78
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936551"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>Verwenden eines ASP.NET-Webdiensts (ASMX)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX bietet die Möglichkeit, Webdienste zu erstellen, die Nachrichten mithilfe von SOAP (Simple Object Access Protocol) senden. SOAP ist ein plattformunabhängiges und sprachunabhängiges Protokoll zum entwickeln und Zugreifen auf Webdienste. Consumer eines ASMX-Dienstanbieter müssen nichts über die Plattform, das Objektmodell oder die Programmiersprache wissen, die zur Implementierung des Dienstanbieter verwendet werden. Sie müssen nur wissen, wie SOAP-Nachrichten gesendet und empfangen werden. In diesem Artikel wird veranschaulicht, wie Sie einen ASMX-SOAP-Dienst aus einer- Xamarin.Forms Anwendung nutzen._

Eine SOAP-Nachricht ist ein XML-Dokument, das die folgenden Elemente enthält:

- Ein root-Element mit dem Namen *Envelope* , das das XML-Dokument als SOAP-Nachricht identifiziert.
- Ein optionales *Header* -Element, das anwendungsspezifische Informationen wie z. b. Authentifizierungsdaten enthält. Wenn das *Header* -Element vorhanden ist, muss es das erste untergeordnete Element des *Envelope* -Elements sein.
- Ein erforderliches *Body* -Element, das die für den Empfänger vorgesehene SOAP-Nachricht enthält.
- Ein optionales *Fault* -Element, das zum Angeben von Fehlermeldungen verwendet wird. Wenn das *Fault* -Element vorhanden ist, muss es sich um ein untergeordnetes Element des *Body* -Elements handeln.

SOAP kann über viele Transportprotokolle, einschließlich http, SMTP, TCP und UDP, betrieben werden. Ein ASMX-Dienst kann jedoch nur über HTTP verwendet werden. Die xamarin-Plattform unterstützt standardmäßige SOAP 1,1-Implementierungen über HTTP und bietet Unterstützung für viele der standardmäßigen ASMX-Dienst Konfigurationen.

Dieses Beispiel enthält die mobilen Anwendungen, die auf physischen oder emulierten Geräten ausgeführt werden, sowie einen ASMX-Dienst, der Methoden zum herunter-, hinzufügen, bearbeiten und Löschen von Daten bereitstellt. Wenn die mobilen Anwendungen ausgeführt werden, stellen Sie eine Verbindung mit dem lokal gehosteten ASMX-Dienst her, wie im folgenden Screenshot zu sehen:

![Beispielanwendung](asmx-images/portal.png)

> [!NOTE]
> In ios 9 und höher erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird. Da ATS in apps, die für IOS 9 erstellt wurden, standardmäßig aktiviert ist, unterliegen alle Verbindungen den Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.
> Die Ate können deaktiviert werden, wenn es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Nutzen des Webdiensts

Der ASMX-Dienst bietet die folgenden Vorgänge:

|Vorgang|BESCHREIBUNG|Parameter|
|--- |--- |--- |
|Getgetdoitems|Abrufen einer Liste von To-Do-Elementen|
|"Kreatedoitem"|Neues to-do-Element erstellen|Ein serialisiertes XML-Element todoitem|
|Editto doitem|Aktualisieren eines To-Do-Elements|Ein serialisiertes XML-Element todoitem|
|Deleteto doitem|Löschen eines To-Do-Elements|Ein serialisiertes XML-Element todoitem|

Weitere Informationen zum Datenmodell, das in der Anwendung verwendet wird, finden Sie unter [Modellieren der Daten](~/xamarin-forms/data-cloud/web-services/introduction.md).

## <a name="create-the-todoservice-proxy"></a>Erstellen des Service Proxy-Proxys

Eine Proxy Klasse, die aufgerufen `TodoService` wird, erweitert `SoapHttpClientProtocol` und stellt Methoden für die Kommunikation mit dem ASMX-Dienst über HTTP bereit. Der Proxy wird generiert, indem jedem plattformspezifischen Projekt in Visual Studio 2019 oder Visual Studio 2017 ein Webverweis hinzugefügt wird. Der Webverweis generiert Methoden und Ereignisse für jede Aktion, die im WSDL-Dokument (Service-Web Services Description Language) definiert ist.

Beispielsweise führt die `GetTodoItems` Dienst Aktion zu einer `GetTodoItemsAsync` -Methode und einem- `GetTodoItemsCompleted` Ereignis im Proxy. Die generierte Methode hat einen void-Rückgabetyp und ruft die `GetTodoItems` Aktion für die übergeordnete `SoapHttpClientProtocol` Klasse auf. Wenn die aufgerufene Methode eine Antwort vom Dienst empfängt, wird das `GetTodoItemsCompleted` -Ereignis ausgelöst, und die Antwortdaten werden in der-Eigenschaft des Ereignisses bereitstellt `Result` .

## <a name="create-the-isoapservice-implementation"></a>Erstellen der isoapservice-Implementierung

Damit das freigegebene, plattformübergreifende Projekt mit dem Dienst zusammenarbeiten kann, wird im Beispiel die- `ISoapService` Schnittstelle definiert, die auf das [asynchrone Programmiermodell der Aufgabe in c#](/dotnet/csharp/programming-guide/concepts/async/)folgt. Jede Plattform implementiert den `ISoapService` , um den plattformspezifischen Proxy verfügbar zu machen. Im Beispiel werden- `TaskCompletionSource` Objekte verwendet, um den Proxy als asynchrone Aufgaben Schnittstelle verfügbar zu machen. Details zur Verwendung von `TaskCompletionSource` finden Sie in den Implementierungen der einzelnen Aktions Typen in den folgenden Abschnitten.

Das Beispiel `SoapService` :

1. Instanziiert `TodoService` als Instanz auf Klassenebene.
1. Erstellt eine Auflistung `Items` , die zum Speichern von Objekten aufgerufen wird. `TodoItem`
1. Gibt einen benutzerdefinierten Endpunkt für die optionale `Url` Eigenschaft auf dem`TodoService`

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

Die Beispielanwendung verwendet die- `TodoItem` Klasse, um Daten zu modellieren. Um ein `TodoItem` Element im Webdienst zu speichern, muss es zuerst in den vom Proxy generierten `TodoItem` Typ konvertiert werden. Dies erfolgt durch die- `ToASMXServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Mit dieser Methode wird eine neue `ASMService.TodoItem` -Instanz erstellt und jede Eigenschaft auf die identische-Eigenschaft der-Instanz festgelegt `TodoItem` .

Ebenso muss beim Abrufen von Daten aus dem-Webdienst aus dem vom Proxy generierten `TodoItem` Typ in eine-Instanz konvertiert werden `TodoItem` . Dies erfolgt mit der- `FromASMXServiceTodoItem` Methode, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode ruft die Daten aus dem vom Proxy generierten `TodoItem` Typ ab und legt Sie in der neu erstellten `TodoItem` Instanz fest.

### <a name="retrieve-data"></a>Abrufen von Daten

Die- `ISoapService` Schnittstelle erwartet, dass die- `RefreshDataAsync` Methode einen mit der-Element Auflistung zurückgibt `Task` . Die `TodoService.GetTodoItemsAsync` Methode gibt jedoch "void" zurück. Um das Schnittstellen Muster zu erfüllen, müssen Sie aufzurufen `GetTodoItemsAsync` , auf das Auslösen des `GetTodoItemsCompleted` Ereignisses warten und die Auflistung auffüllen. Auf diese Weise können Sie eine gültige Sammlung an die Benutzeroberfläche zurückgeben.

Im folgenden Beispiel wird ein neuer erstellt, der asynchrone `TaskCompletionSource` -Befehl in der `RefreshDataAsync` -Methode gestartet und die `Task` von bereitgestellten erwartet `TaskCompletionSource` . Wenn der `TodoService_GetTodoItemsCompleted` Ereignishandler aufgerufen wird, füllt er die Auflistung auf `Items` und aktualisiert Folgendes `TaskCompletionSource` :

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

Beim Erstellen oder Bearbeiten von Daten müssen Sie die- `ISoapService.SaveTodoItemAsync` Methode implementieren. Diese Methode erkennt, ob `TodoItem` ein neues oder aktualisiertes Element ist, und ruft die entsprechende-Methode für das- `todoService` Objekt auf. Die `CreateTodoItemCompleted` `EditTodoItemCompleted` Ereignishandler und sollten ebenfalls implementiert werden, damit Sie wissen, wann `todoService` eine Antwort vom ASMX-Dienst empfangen hat (diese können zu einem einzigen Handler kombiniert werden, da Sie denselben Vorgang ausführen). Im folgenden Beispiel werden die-Schnittstellen-und ereignishandlerimplementierungen sowie das-Objekt veranschaulicht, das `TaskCompletionSource` für den asynchronen Betrieb verwendet wird:

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

Das Löschen von Daten erfordert eine ähnliche Implementierung. Definieren Sie eine `TaskCompletionSource` , implementieren Sie einen Ereignishandler, und die- `ISoapService.DeleteTodoItemAsync` Methode:

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

- ["Doasmx" (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
