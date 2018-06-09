---
title: Verwenden ein Azure-Cosmos-DB-Dokumentdatenbank
description: In diesem Artikel wird erläutert, wie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure-Cosmos-DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren.
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e797eaad98f6fac66876aaebecd7ae53ad9dbab
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242504"
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Verwenden ein Azure-Cosmos-DB-Dokumentdatenbank

_Ein Azure-Cosmos-DB-dokumentdatenbank ist ein NoSQL-Datenbank, die mit geringer Latenz der Zugang zu JSON-Dokumente, bietet einen schnelle, hochverfügbare, skalierbare Datenbankdienst für Anwendungen, die eine nahtlose Skalierung und globale Replikation erforderlich sind. In diesem Artikel wird erläutert, wie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure-Cosmos-DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure-Cosmos-DB, von [Xamarin University](https://university.xamarin.com/)**

Ein Azure-Cosmos-DB-Dokument-Datenbankkonto kann mit einem Azure-Abonnement bereitgestellt werden. Jede Datenbankkonto kann keine oder mehrere Datenbanken verfügen. Eine dokumentdatenbank in Azure-Cosmos-DB ist ein logischer Container für dokumentauflistungen und Benutzer.

Ein Azure-Cosmos-DB-dokumentdatenbank kann NULL oder mehr dokumentauflistungen enthalten. Jede Auflistung Dokument kann eine unterschiedlichen Leistungsstufe ermöglicht einen höheren Durchsatz für häufig verwendete Auflistungen angegeben werden und weniger Durchsatz für Sammlungen selten haben.

Jede Auflistung Dokument besteht aus null oder mehr JSON-Dokumente. Dokumente in einer Auflistung sind schemafreier und müssen daher nicht die gleiche Struktur oder Felder freigeben. Dokumente eine Dokument-Auflistung hinzugefügt werden, indiziert Cosmos-DB automatisch und stehen abgefragt werden.

Zu Entwicklungszwecken kann eine dokumentdatenbank auch durch einen Emulator genutzt werden. Verwenden den Emulator, können lokal, ohne ein Azure-Abonnement erstellen oder zu allen Kosten anfallen Anwendungen entwickelt und getestet werden. Weitere Informationen zu den Emulator, finden Sie unter [lokal entwickeln, mit dem Azure-Cosmos-DB-Emulator](/azure/cosmos-db/local-emulator/).

In diesem Artikel und zugehörigen beispielanwendung veranschaulicht eine Todo List-Anwendung, wo die Aufgaben in einer Azure-Cosmos-DB-Dokument-Datenbank gespeichert werden. Weitere Informationen über die beispielanwendung finden Sie unter [Verständnis des Beispiels](~/xamarin-forms/data-cloud/walkthrough.md).

Weitere Informationen zu Azure-Cosmos-Datenbank finden Sie unter der [Azure Cosmos-DB-Dokumentation](/azure/cosmos-db/).

## <a name="setup"></a>Setup

Der Prozess für die Integration einer Azure-Cosmos-DB-Dokument-Datenbank in einer Xamarin.Forms-Anwendung lautet wie folgt:

1. Erstellen Sie ein Konto Cosmos-DB. Weitere Informationen finden Sie unter [erstellen Sie ein Azure-Cosmos-DB-Konto](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Hinzufügen der [Azure Cosmos DB .NET Standard-Clientbibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) NuGet-Paket mit den plattformprojekten in Xamarin.Forms-Projektmappe.
1. Hinzufügen `using` Direktiven für die `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, und `Microsoft.Azure.Documents.Linq` Namespaces, Klassen, die das Cosmos-DB-Konto zugreift.

Nach dem Ausführen dieser Schritte, kann die Azure Cosmos DB .NET Standard-Clientbibliothek zum Konfigurieren und Ausführen von Abfragen der dokumentdatenbank verwendet werden.

> [!NOTE]
> Die Azure Cosmos DB .NET Standard-Clientbibliothek kann nur in Projekten für die Plattform und nicht in ein Projekt für die Portable Klassenbibliothek (PCL) installiert werden. Daher ist die beispielanwendung eine Shared Access Projekt (SAP) um codeverdoppelungen zu vermeiden. Allerdings die `DependencyService` -Klasse kann in einer PCL-Projekt verwendet werden, zum Aufrufen von Azure Cosmos DB .NET Client Library Standardcode in plattformspezifischen Projekte enthalten sind.

## <a name="consuming-the-azure-cosmos-db-account"></a>Nutzen das Azure-Cosmos-DB-Konto

Die `DocumentClient` Typ kapselt den Endpunkt, Anmeldeinformationen und Verbindungsrichtlinie verwendet, um das Azure-Cosmos-DB-Konto zugreifen und dient zum Konfigurieren und Ausführen von Anforderungen für das Konto. Im folgenden Codebeispiel wird veranschaulicht, wie eine Instanz dieser Klasse erstellt wird:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Der Cosmos-DB-Uri und den Primärschlüssel können um müssen angegeben werden die `DocumentClient` Konstruktor. Diese können über das Azure Portal abgerufen werden. Weitere Informationen finden Sie unter [mit einem Azure-Cosmos-DB-Konto verbinden](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Erstellen einer Datenbank

Eine dokumentdatenbank ist ein logischer Container für dokumentauflistungen und Benutzer und kann in der Azure-Verwaltungsportal erstellt oder programmgesteuert mit der `DocumentClient.CreateDatabaseIfNotExistsAsync` Methode:

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

Die `CreateDatabaseIfNotExistsAsync` Methode gibt ein `Database` -Objekt als Argument, mit der `Database` -Objekt, mit den Datenbanknamen als seine `Id` Eigenschaft. Die `CreateDatabaseIfNotExistsAsync` Methode wird die Datenbank erstellt, wenn er nicht vorhanden, oder gibt die Datenbank zurück, wenn sie bereits vorhanden ist. Die beispielanwendung ignoriert jedoch keine Daten zurückgegeben werden, indem Sie die `CreateDatabaseIfNotExistsAsync` Methode.

> [!NOTE]
> Die `CreateDatabaseIfNotExistsAsync` Methode gibt ein `Task<ResourceResponse<Database>>` Objekt und den Statuscode der Antwort können überprüft werden, um zu bestimmen, ob eine Datenbank erstellt wurde, oder es wurde eine vorhandene Datenbank zurückgegeben.

### <a name="creating-a-document-collection"></a>Erstellen eine Dokument-Auflistung

Eine Dokument-Auflistung ist ein Container für JSON-Dokumente, und kann in der Azure-Verwaltungsportal erstellt oder programmgesteuert mit der `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` Methode:

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

Die `CreateDocumentCollectionIfNotExistsAsync` Methode erfordert zwei obligatorische Argumente – ein Datenbankname angegeben als eine `Uri`, und ein `DocumentCollection` Objekt. Die `DocumentCollection` Objekt darstellt, eine Dokument-Auflistung, deren Name angegeben ist, mit, der `Id` Eigenschaft. Die `CreateDocumentCollectionIfNotExistsAsync` Methode die Dokument-Auflistung erstellt, wenn er nicht vorhanden, oder gibt die Dokument-Auflistung zurück, wenn sie bereits vorhanden ist. Die beispielanwendung ignoriert jedoch keine Daten zurückgegeben werden, indem Sie die `CreateDocumentCollectionIfNotExistsAsync` Methode.

> [!NOTE]
> Die `CreateDocumentCollectionIfNotExistsAsync` Methode gibt ein `Task<ResourceResponse<DocumentCollection>>` Objekt und den Statuscode der Antwort können überprüft werden, um zu bestimmen, ob eine Dokument-Auflistung erstellt wurde, oder eine vorhandene Dokumentsammlung wurde zurückgegeben.

Optional, die `CreateDocumentCollectionIfNotExistsAsync` -Methode angeben kann auch ein `RequestOptions` -Objekt, das Optionen kapselt, die für Anforderungen, die dem Cosmos-DB-Konto angegeben werden können. Die `RequestOptions.OfferThroughput` Eigenschaft wird verwendet, um die Leistungsstufe der Auflistung Dokument definieren und in der Beispiel-Anwendung auf 400 anforderungseinheiten pro Sekunde festgelegt ist. Dieser Wert sollte erhöht oder verringert werden, je nachdem, ob die Auflistung häufig oder nur selten zugegriffen wird.

> [!IMPORTANT]
> Beachten Sie, dass die `CreateDocumentCollectionIfNotExistsAsync` Methode erstellt eine neue Sammlung mit einem reservierten Durchsatz, besitzt Auswirkungen auf die Preisgestaltung.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Abrufen von Dokument Auflistung Dokumente

Der Inhalt einer Auflistung Dokument können durch Erstellen und Ausführen einer Abfrage abgerufen werden. Eine Dokumentabfrage wird erstellt, mit der `DocumentClient.CreateDocumentQuery` Methode:

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

Diese Abfrage asynchron alle Dokumente aus der angegebenen Auflistung abgerufen und platziert die Dokumente in einem `List<TodoItem>` Auflistung für die Anzeige.

Die `CreateDocumentQuery<T>` Methode gibt ein `Uri` Argument, das die Auflistung darstellt, die für Dokumente abgefragt werden soll. In diesem Beispiel wird die `collectionLink` Variable ist ein Feld auf Klassenebene, der angibt, die `Uri` , Dokument-Auflistung, um Dokumente aus abzurufen darstellt:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

Die `CreateDocumentQuery<T>` Methode erstellt eine Abfrage, die synchron ausgeführt wird, und gibt ein `IQueryable<T>` Objekt. Allerdings die `AsDocumentQuery` -Methode konvertiert die `IQueryable<T>` -Objekt an eine `IDocumentQuery<T>` Objekt, das asynchron ausgeführt werden kann. Die asynchrone Abfrage wird ausgeführt, mit der `IDocumentQuery<T>.ExecuteNextAsync` -Methode, die mit der nächste Seite mit Ergebnissen aus der dokumentdatenbank abruft der `IDocumentQuery<T>.HasMoreResults` Eigenschaft, die angibt, ob zusätzliche Ergebnisse aus der Abfrage zurückgegeben werden.

Dokumente können möglicherweise gefiltert Serverseite dazu eine `Where` -Klausel der Abfrage, die ein Filterprädikat auf die Abfrage für die Auflistung Dokument gilt:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Diese Abfrage ruft alle Dokumente aus der Auflistung zurück, deren `Done` -Eigenschaft gleich `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Ein Dokument in eine Auflistung Dokument eingefügt.

Dokumente werden benutzerdefinierte JSON-Inhalt und eingefügt werden können, in eine Dokument-Auflistung mit den `DocumentClient.CreateDocumentAsync` Methode:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

Die `CreateDocumentAsync` Methode gibt ein `Uri` Argument, das die Auflistung darstellt, in das Dokument eingefügt werden soll, und ein `object` Argument, das das Dokument eingefügt werden.

### <a name="replacing-a-document-in-a-document-collection"></a>Ersetzen eines Dokuments in einer Auflistung Dokument

Dokumente können ersetzt werden, in die Auflistung ein Dokument mit der `DocumentClient.ReplaceDocumentAsync` Methode:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

Die `ReplaceDocumentAsync` Methode gibt ein `Uri` Argument, das das Dokument in der Auflistung, die ersetzt werden soll darstellt, und ein `object` Argument, das die aktualisierten Daten darstellt.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Löschen ein Dokument aus einer Auflistung Dokument

Ein Dokument kann gelöscht werden, aus einer Auflistung Dokument mit der `DocumentClient.DeleteDocumentAsync` Methode:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

Die `DeleteDocumentAsync` Methode gibt ein `Uri` Argument, das das Dokument in der Auflistung darstellt, die gelöscht werden soll.

### <a name="deleting-a-document-collection"></a>Löschen eine Dokument-Auflistung

Eine Dokument-Auflistung kann gelöscht werden, aus einer Datenbank mit der `DocumentClient.DeleteDocumentCollectionAsync` Methode:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

Die `DeleteDocumentCollectionAsync` Methode gibt ein `Uri` Argument, das die zu löschende Auflistung Dokument darstellt. Beachten Sie, dass die Dokumente in der Auflistung gespeicherten Aufrufen dieser Methode auch gelöscht werden.

### <a name="deleting-a-database"></a>Löschen einer Datenbank

Eine Datenbank kann gelöscht werden, aus einem Cosmos-DB-Datenbankkonto mit der `DocumentClient.DeleteDatabaesAsync` Methode:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

Die `DeleteDatabaseAsync` Methode gibt ein `Uri` Argument, das die zu löschende Datenbank darstellt. Beachten Sie, dass Aufrufen dieser Methode auch dokumentauflistungen, die in der Datenbank gespeichert und in der dokumentauflistungen gespeicherten Dokumente gelöscht werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure-Cosmos-DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren. Ein Azure-Cosmos-DB-dokumentdatenbank ist ein NoSQL-Datenbank, die mit geringer Latenz der Zugang zu JSON-Dokumente, bietet einen schnelle, hochverfügbare, skalierbare Datenbankdienst für Anwendungen, die eine nahtlose Skalierung und globale Replikation erforderlich sind.


## <a name="related-links"></a>Verwandte Links

- [TODO-Azure-Cosmos-Datenbank (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Azure-Cosmos-DB-Dokumentation](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard-Clientbibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure-Cosmos-DB-API](https://docs.microsoft.com/en-us/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
