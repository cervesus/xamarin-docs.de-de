---
title: Verwenden einer Azure Cosmos DB-dokumentdatenbank inXamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie die Azure Cosmos DB .NET Standard-Client Bibliothek verwenden, um eine Azure Cosmos DB Dokumentendatenbank in eine-Anwendung zu integrieren Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 47b35d394eab339a8e9a1f81880e6de4233f29b6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127085"
---
# <a name="consume-an-azure-cosmos-db-document-database-in-xamarinforms"></a>Verwenden einer Azure Cosmos DB-dokumentdatenbank inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)

_Eine Azure Cosmos DB Document-Datenbank ist eine nosql-Datenbank, die den Zugriff auf JSON-Dokumente mit geringer Latenz ermöglicht und einen schnellen, hoch verfügbaren, skalierbaren Datenbankdienst für Anwendungen bietet, die eine nahtlose Skalierung und globale Replikation erfordern In diesem Artikel wird erläutert, wie Sie die Azure Cosmos DB .NET Standard-Client Bibliothek verwenden, um eine Azure Cosmos DB Dokumentendatenbank in eine-Anwendung zu integrieren Xamarin.Forms ._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB-Video**

Ein Azure Cosmos DB Dokumenten-Daten Bankkonto kann mithilfe eines Azure-Abonnements bereitgestellt werden. Jedes Daten Bankkonto kann über NULL oder mehr Datenbanken verfügen. Eine dokumentdatenbank in Azure Cosmos DB ist ein logischer Container für Dokument Sammlungen und Benutzer.

Eine Azure Cosmos DB-dokumentdatenbank kann NULL oder mehr Dokument Sammlungen enthalten. Jede Dokument Sammlung kann eine andere Leistungsstufe aufweisen, sodass für häufig verwendete Auflistungen mehr Durchsatz und weniger Durchsatz für selten aufgerufene Sammlungen festgelegt werden kann.

Jede Dokument Sammlung besteht aus null oder mehr JSON-Dokumenten. Dokumente in einer Sammlung sind Schema frei und müssen daher nicht dieselbe Struktur oder dieselben Felder gemeinsam verwenden. Beim Hinzufügen von Dokumenten zu einer Dokument Auflistung werden diese von Cosmos DB automatisch indiziert und stehen zur Abfrage zur Verfügung.

Zu Entwicklungszwecken kann eine dokumentdatenbank auch über einen Emulator genutzt werden. Mithilfe des Emulators können Anwendungen lokal entwickelt und getestet werden, ohne ein Azure-Abonnement zu erstellen oder Kosten zu verursachen. Weitere Informationen zum Emulator finden Sie unter [lokale Entwicklung mit dem Azure Cosmos DB-Emulator](/azure/cosmos-db/local-emulator/).

Dieser Artikel und die begleitende Beispielanwendung veranschaulichen eine ToDo List-Anwendung, in der die Aufgaben in einer Azure Cosmos DB-dokumentdatenbank gespeichert werden. Weitere Informationen zur Beispielanwendung finden Sie Untergrund Legendes [zum Beispiel](~/xamarin-forms/data-cloud/web-services/introduction.md).

Weitere Informationen zu Azure Cosmos DB finden Sie in der [Azure Cosmos DB-Dokumentation](/azure/cosmos-db/).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

## <a name="setup"></a>Einrichten

Der Prozess zum Integrieren einer Azure Cosmos DB-dokumentdatenbank in eine- Xamarin.Forms Anwendung sieht wie folgt aus:

1. Erstellen Sie ein Cosmos DB Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Cosmos DB Kontos](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account).
1. Fügen Sie den Platt Form Projekten in der Projekt Mappe das nuget-Paket für die [Azure Cosmos DB .NET Standard-Client Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) hinzu Xamarin.Forms .
1. Fügen Sie `using` den `Microsoft.Azure.Documents` Klassen, die `Microsoft.Azure.Documents.Client` `Microsoft.Azure.Documents.Linq` auf das Cosmos DB Konto zugreifen, Direktiven für die Namespaces, und hinzu.

Nachdem Sie diese Schritte ausgeführt haben, können Sie die Azure Cosmos DB .NET Standard-Client Bibliothek zum Konfigurieren und Ausführen von Anforderungen für die dokumentdatenbank verwenden.

> [!NOTE]
> Die Azure Cosmos DB .NET Standard-Client Bibliothek kann nur in Platt Form Projekten und nicht in einem Projekt für eine portable Klassenbibliothek (Portable Class Library, PCL) installiert werden. Daher ist die Beispielanwendung ein Shared Access-Projekt (SAP), um Code Duplikate zu vermeiden. Allerdings kann die- `DependencyService` Klasse in einem PCL-Projekt verwendet werden, um Azure Cosmos DB .NET Standard Client Bibliotheks Code aufzurufen, der in plattformspezifischen Projekten enthalten ist.

## <a name="consuming-the-azure-cosmos-db-account"></a>Verwenden des Azure Cosmos DB Kontos

Der `DocumentClient` -Typ kapselt den Endpunkt, die Anmelde Informationen und die Verbindungsrichtlinie, die für den Zugriff auf das Azure Cosmos DB Konto verwendet werden, und wird zum Konfigurieren und Ausführen von Anforderungen für das Konto verwendet. Im folgenden Codebeispiel wird veranschaulicht, wie eine Instanz dieser Klasse erstellt wird:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Der Cosmos DB-URI und der Primärschlüssel müssen für den `DocumentClient` Konstruktor bereitgestellt werden. Diese können im Azure-Portal abgerufen werden. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem Azure Cosmos DB Konto](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Erstellen einer Datenbank

Eine dokumentdatenbank ist ein logischer Container für Dokument Sammlungen und-Benutzer und kann im Azure-Portal oder Programm gesteuert mithilfe der-Methode erstellt werden `DocumentClient.CreateDatabaseIfNotExistsAsync` :

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

Die- `CreateDatabaseIfNotExistsAsync` Methode gibt ein- `Database` Objekt als Argument an, wobei das- `Database` Objekt den Datenbanknamen als seine- `Id` Eigenschaft angibt. Die- `CreateDatabaseIfNotExistsAsync` Methode erstellt die Datenbank, wenn Sie nicht vorhanden ist, oder gibt die Datenbank zurück, wenn Sie bereits vorhanden ist. Die Beispielanwendung ignoriert jedoch alle Daten, die von der-Methode zurückgegeben werden `CreateDatabaseIfNotExistsAsync` .

> [!NOTE]
> Die `CreateDatabaseIfNotExistsAsync` -Methode gibt ein `Task<ResourceResponse<Database>>` -Objekt zurück, und der Statuscode der Antwort kann geprüft werden, um zu bestimmen, ob eine Datenbank erstellt wurde oder ob eine vorhandene Datenbank zurückgegeben wurde.

### <a name="creating-a-document-collection"></a>Erstellen einer Dokument Sammlung

Eine Dokument Sammlung ist ein Container für JSON-Dokumente und kann im Azure-Portal oder Programm gesteuert mithilfe der-Methode erstellt werden `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` :

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

Die `CreateDocumentCollectionIfNotExistsAsync` -Methode erfordert zwei obligatorische Argumente – einen als angegebenen Datenbanknamen `Uri` und ein- `DocumentCollection` Objekt. Das- `DocumentCollection` Objekt stellt eine Dokument Auflistung dar, deren Name mit der-Eigenschaft angegeben wird `Id` . Die- `CreateDocumentCollectionIfNotExistsAsync` Methode erstellt die Dokument Auflistung, wenn Sie nicht vorhanden ist, oder gibt die Dokument Auflistung zurück, wenn Sie bereits vorhanden ist. Die Beispielanwendung ignoriert jedoch alle Daten, die von der-Methode zurückgegeben werden `CreateDocumentCollectionIfNotExistsAsync` .

> [!NOTE]
> Die `CreateDocumentCollectionIfNotExistsAsync` -Methode gibt ein `Task<ResourceResponse<DocumentCollection>>` -Objekt zurück, und der Statuscode der Antwort kann geprüft werden, um zu bestimmen, ob eine Dokument Auflistung erstellt wurde oder ob eine vorhandene Dokument Auflistung zurückgegeben wurde.

Optional kann die- `CreateDocumentCollectionIfNotExistsAsync` Methode auch ein- `RequestOptions` Objekt angeben, das Optionen kapselt, die für Anforderungen angegeben werden können, die an das Cosmos DB Konto ausgegeben werden. Die `RequestOptions.OfferThroughput` -Eigenschaft wird zum Definieren der Leistungs Ebene der Dokument Auflistung und in der Beispielanwendung auf 400 Anforderungs Einheiten pro Sekunde festgelegt. Dieser Wert sollte vergrößert oder verringert werden, je nachdem, ob die Auflistung häufig oder nur selten verwendet wird.

> [!IMPORTANT]
> Beachten Sie, dass die- `CreateDocumentCollectionIfNotExistsAsync` Methode eine neue Sammlung mit reserviertem Durchsatz erstellt. Dies hat Auswirkungen auf die Preisgestaltung.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Abrufen von Dokumenten Sammlungs Dokumenten

Der Inhalt einer Dokument Sammlung kann abgerufen werden, indem eine Dokument Abfrage erstellt und ausgeführt wird. Eine Dokument Abfrage wird mit der- `DocumentClient.CreateDocumentQuery` Methode erstellt:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

Diese Abfrage Ruft asynchron alle Dokumente aus der angegebenen Auflistung ab und platziert die Dokumente zur Anzeige in einer Auflistung `List<TodoItem>` .

Die- `CreateDocumentQuery<T>` Methode gibt ein- `Uri` Argument an, das die Auflistung darstellt, die für Dokumente abgefragt werden soll. In diesem Beispiel ist die- `collectionLink` Variable ein Feld auf Klassenebene, das die `Uri` Dokument Auflistung darstellt, aus der Dokumente abgerufen werden sollen:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

Die `CreateDocumentQuery<T>` -Methode erstellt eine Abfrage, die synchron ausgeführt wird, und gibt ein- `IQueryable<T>` Objekt zurück. Allerdings konvertiert die- `AsDocumentQuery` Methode das- `IQueryable<T>` Objekt in ein- `IDocumentQuery<T>` Objekt, das asynchron ausgeführt werden kann. Die asynchrone Abfrage wird mit der- `IDocumentQuery<T>.ExecuteNextAsync` Methode ausgeführt, die die nächste Seite der Ergebnisse aus der Dokumentendatenbank abruft, wobei die- `IDocumentQuery<T>.HasMoreResults` Eigenschaft angibt, ob zusätzliche Ergebnisse von der Abfrage zurückgegeben werden müssen.

Dokumente können gefiltert werden, indem eine- `Where` Klausel in die Abfrage eingeschlossen wird, die ein Filter Prädikat auf die Abfrage für die Dokument Sammlung anwendet:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Diese Abfrage ruft alle Dokumente aus der Auflistung ab, deren- `Done` Eigenschaft gleich ist `false` .

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Einfügen eines Dokuments in eine Dokument Sammlung

Dokumente sind benutzerdefinierte JSON-Inhalte und können mit der-Methode in eine Dokument Sammlung eingefügt werden `DocumentClient.CreateDocumentAsync` :

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

Die `CreateDocumentAsync` -Methode gibt ein Argument an, das `Uri` die Auflistung darstellt, in die das Dokument eingefügt werden soll, und ein `object` Argument, das das einzufügende Dokument darstellt.

### <a name="replacing-a-document-in-a-document-collection"></a>Ersetzen eines Dokuments in einer Dokument Sammlung

Dokumente können mithilfe der-Methode in einer Dokument Sammlung ersetzt werden `DocumentClient.ReplaceDocumentAsync` :

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

Die `ReplaceDocumentAsync` -Methode gibt ein `Uri` -Argument an, das das Dokument in der Auflistung darstellt, das ersetzt werden soll, und ein `object` Argument, das die aktualisierten Dokument Daten darstellt.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Löschen eines Dokuments aus einer Dokument Sammlung

Ein Dokument kann mit der-Methode aus einer Dokument Sammlung gelöscht werden `DocumentClient.DeleteDocumentAsync` :

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

Die- `DeleteDocumentAsync` Methode gibt ein- `Uri` Argument an, das das Dokument in der Auflistung darstellt, das gelöscht werden soll.

### <a name="deleting-a-document-collection"></a>Löschen einer Dokument Sammlung

Eine Dokument Sammlung kann mit der-Methode aus einer Datenbank gelöscht werden `DocumentClient.DeleteDocumentCollectionAsync` :

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

Die- `DeleteDocumentCollectionAsync` Methode gibt ein- `Uri` Argument an, das die zu löschende Dokument Auflistung darstellt. Beachten Sie, dass beim Aufrufen dieser Methode auch die in der Auflistung gespeicherten Dokumente gelöscht werden.

### <a name="deleting-a-database"></a>Löschen einer Datenbank

Eine Datenbank kann mit der-Methode aus einem Cosmos DB-Daten Bankkonto gelöscht werden `DocumentClient.DeleteDatabaesAsync` :

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

Die- `DeleteDatabaseAsync` Methode gibt ein- `Uri` Argument an, das die zu löschende Datenbank darstellt. Beachten Sie, dass beim Aufrufen dieser Methode auch die in der Datenbank gespeicherten Dokument Sammlungen und die in den Dokument Auflistungen gespeicherten Dokumente gelöscht werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Azure Cosmos DB .NET Standard-Client Bibliothek verwendet wird, um eine Azure Cosmos DB Dokumentendatenbank in eine-Anwendung zu integrieren Xamarin.Forms . Eine Azure Cosmos DB Document-Datenbank ist eine nosql-Datenbank, die den Zugriff auf JSON-Dokumente mit geringer Latenz ermöglicht und einen schnellen, hoch verfügbaren, skalierbaren Datenbankdienst für Anwendungen bietet, die eine nahtlose Skalierung und globale Replikation erfordern

## <a name="related-links"></a>Verwandte Links

- [TODO-Azure Cosmos DB (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)
- [Dokumentation für Azure Cosmos DB](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard-Client Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB-API](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
