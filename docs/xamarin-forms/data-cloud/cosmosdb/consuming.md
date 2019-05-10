---
title: Nutzen eine Azure Cosmos DB-Dokumentdatenbank
description: In diesem Artikel wird erläutert, wie Sie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure Cosmos DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren.
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6b1453164533e2905e78407f33d79a178c7f1ae8
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978270"
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Nutzen eine Azure Cosmos DB-Dokumentdatenbank

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)

_Eine Azure Cosmos DB-dokumentdatenbank ist eine NoSQL-Datenbank, die Zugriff mit geringer Latenz für JSON-Dokumenten, bietet einen schnelle, hoch verfügbare, skalierbare Datenbankdienst für Anwendungen, die eine nahtlose Skalierung und globale Replikation erfordern bereitstellt. In diesem Artikel wird erläutert, wie Sie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure Cosmos DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB-video**

Ein Azure Cosmos DB-Dokument-Datenbankkonto kann über ein Azure-Abonnement bereitgestellt werden. Jedes Datenbankkonto kann keine oder mehrere Datenbanken haben. Eine dokumentdatenbank in Azure Cosmos DB ist ein logischer Container für Dokumentsammlungen und Benutzer.

Eine Azure Cosmos DB-dokumentdatenbank kann NULL oder mehr Dokumentsammlungen enthalten. Jedes Dokument können eine andere Leistungsstufe auswähle, sodass mehr Durchsatz für Sammlungen angegeben werden und weniger Durchsatz für selten genutzte Sammlungen haben.

Jede Dokumentsammlung besteht aus null oder mehr JSON-Dokumente. Dokumente in einer Sammlung sind schemafreie, und Sie müssen also nicht die gleiche Struktur oder Felder freigeben. Eine Dokumentsammlung Dokumente hinzugefügt werden, Cosmos DB indiziert automatisch und abgefragt werden, verfügbar.

Für Entwicklungszwecke kann auch eine dokumentdatenbank, über einen Emulator genutzt werden. Mit dem Emulator können können lokal, ohne ein Azure-Abonnement erstellen oder keine Kosten anfallen Anwendungen entwickelt und getestet werden. Weitere Informationen zum Emulator finden Sie unter [lokale Entwicklung mit Azure Cosmos DB-Emulator](/azure/cosmos-db/local-emulator/).

In diesem Artikel und die zugehörige beispielanwendung zeigt eine Todolist-Anwendung, in dem die Aufgaben in einer Azure Cosmos DB-dokumentdatenbank gespeichert sind. Weitere Informationen zu der beispielanwendung, finden Sie unter [Grundlegendes zum Beispiel](~/xamarin-forms/data-cloud/walkthrough.md).

Weitere Informationen zu Azure Cosmos DB finden Sie unter den [Dokumentation für Azure Cosmos DB](/azure/cosmos-db/).

## <a name="setup"></a>Setup

Der Prozess für die Integration einer Azure Cosmos DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung lautet wie folgt aus:

1. Erstellen Sie ein Cosmos DB-Konto an. Weitere Informationen finden Sie unter [erstellen Sie ein Azure Cosmos DB-Konto](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account).
1. Hinzufügen der [Azure Cosmos DB .NET Standard-Clientbibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) NuGet-Paket auf den Platform-Projekten in der Xamarin.Forms-Lösung.
1. Hinzufügen `using` Direktiven für die `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, und `Microsoft.Azure.Documents.Linq` Namespaces, Klassen, die Cosmos DB-Konto zugegriffen werden.

Nach der Durchführung dieser Schritte kann die Azure Cosmos DB .NET Standard-Clientbibliothek zum Konfigurieren und Ausführen von Abfragen der dokumentdatenbank verwendet werden.

> [!NOTE]
> Die Azure Cosmos DB .NET Standard-Clientbibliothek kann nur in Projekten-Plattform, und nicht in ein Projekt für die Portable Klassenbibliothek (PCL) installiert werden. Aus diesem Grund ist die beispielanwendung eine Shared Access Projekt (SAP) um codeverdoppelungen zu vermeiden. Allerdings die `DependencyService` Klasse kann in ein PCL-Projekt verwendet werden, zum Aufrufen von Azure Cosmos DB .NET Standard Client Library-Code in plattformspezifischen Projekten enthalten sind.

## <a name="consuming-the-azure-cosmos-db-account"></a>Nutzen Azure Cosmos DB-Kontos

Die `DocumentClient` Typ kapselt, der Endpunkt, Anmeldeinformationen und Verbindungsrichtlinie, die für den Zugriff auf Azure Cosmos DB-Konto, und dient zum Konfigurieren und Ausführen von Anforderungen für das Konto. Im folgenden Codebeispiel wird veranschaulicht, wie Sie eine Instanz dieser Klasse zu erstellen:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Die Cosmos DB-Uri und Primärschlüssel müssen angegeben werden, die `DocumentClient` Konstruktor. Diese können über das Azure-Portal abgerufen werden. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einer Azure Cosmos DB-Datenbankkonto](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Erstellen einer Datenbank

Eine dokumentdatenbank ist ein logischer Container für Dokumentsammlungen und Benutzer, und kann erstellten im Azure-Portal oder programmgesteuert mithilfe der `DocumentClient.CreateDatabaseIfNotExistsAsync` Methode:

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

Die `CreateDatabaseIfNotExistsAsync` Methode gibt eine `Database` Objekt als Argument, mit der `Database` -Objekt, mit den Datenbanknamen als seine `Id` Eigenschaft. Die `CreateDatabaseIfNotExistsAsync` Methode erstellt die Datenbank aus, falls es nicht vorhanden ist, oder gibt die Datenbank zurück, wenn sie bereits vorhanden ist. Die beispielanwendung ignoriert jedoch alle von zurückgegebenen Daten die `CreateDatabaseIfNotExistsAsync` Methode.

> [!NOTE]
> Die `CreateDatabaseIfNotExistsAsync` Methode gibt eine `Task<ResourceResponse<Database>>` Objekt und der Statuscode der Antwort kann überprüft werden, um zu bestimmen, ob eine Datenbank erstellt wurde, oder es wurde eine vorhandene Datenbank zurückgegeben.

### <a name="creating-a-document-collection"></a>Erstellen eine Dokumentsammlung

Eine Dokumentsammlung ist ein Container für JSON-Dokumente, und kann im Azure-Portal erstellt oder programmgesteuert mithilfe der `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` Methode:

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

Die `CreateDocumentCollectionIfNotExistsAsync` Methode erfordert zwei obligatorische Argumente: ein Datenbankname angegeben als ein `Uri`, und ein `DocumentCollection` Objekt. Die `DocumentCollection` Objekt darstellt, eine Dokument-Auflistung, deren Name angegeben ist, mit, der `Id` Eigenschaft. Die `CreateDocumentCollectionIfNotExistsAsync` Methode erstellt die Dokumentsammlung an, falls es nicht vorhanden ist, oder gibt die Dokument-Auflistung zurück, wenn sie bereits vorhanden ist. Die beispielanwendung ignoriert jedoch alle von zurückgegebenen Daten die `CreateDocumentCollectionIfNotExistsAsync` Methode.

> [!NOTE]
> Die `CreateDocumentCollectionIfNotExistsAsync` Methode gibt eine `Task<ResourceResponse<DocumentCollection>>` Objekt und der Statuscode der Antwort kann überprüft werden, um zu bestimmen, ob ein Dokument erstellt wurde, oder eine vorhandenen Dokumentsammlung zurückgegeben wurde.

Optional die `CreateDocumentCollectionIfNotExistsAsync` -Methode angeben kann auch eine `RequestOptions` -Objekt, das Optionen kapselt, die für Anforderungen, die dem Cosmos DB-Konto angegeben werden können. Die `RequestOptions.OfferThroughput` Eigenschaft wird verwendet, um die Leistungsstufe der Dokumentsammlung zu definieren, und die Anwendung, in dem Beispiel in 400 anforderungseinheiten pro Sekunde festgelegt ist. Dieser Wert sollte erhöht oder verringert werden, je nachdem, ob die Auflistung häufig oder selten zugegriffen wird.

> [!IMPORTANT]
> Beachten Sie, dass die `CreateDocumentCollectionIfNotExistsAsync` Methode erstellt eine neue Sammlung mit einem reservierten Durchsatz, die Auswirkungen auf die Preise hat.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Abrufen von Dokument Datenbanksammlungs-Dokumenten

Der Inhalt einer Auflistung Dokument können durch Erstellen und Ausführen einer Dokumentabfrage abgerufen werden. Eine Dokumentabfrage wird erstellt, mit der `DocumentClient.CreateDocumentQuery` Methode:

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

Diese Abfrage asynchron Ruft alle Dokumente aus der angegebenen Auflistung ab und fügt die Dokumente in einem `List<TodoItem>` Auflistung für die Anzeige.

Die `CreateDocumentQuery<T>` Methode gibt eine `Uri` Argument, das der Auflistung darstellt, die für Dokumente abgefragt werden sollen. In diesem Beispiel die `collectionLink` Variable ist ein Feld auf Klassenebene, der angibt, die `Uri` , die die Dokumentsammlung zum Abrufen von Dokumenten aus darstellt:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

Die `CreateDocumentQuery<T>` Methode erstellt eine Abfrage, die synchron ausgeführt wird, und gibt eine `IQueryable<T>` Objekt. Allerdings die `AsDocumentQuery` Methode konvertiert die `IQueryable<T>` -Objekt an eine `IDocumentQuery<T>` Objekt, das asynchron ausgeführt werden kann. Die asynchrone Abfrage wird ausgeführt, mit der `IDocumentQuery<T>.ExecuteNextAsync` -Methode, die Ruft die nächste Seite mit Ergebnissen aus der dokumentdatenbank, mit der `IDocumentQuery<T>.HasMoreResults` Eigenschaft, der angibt, ob zusätzliche Ergebnisse der Abfrage zurückgegeben werden.

Dokumente können werden gefiltert, serverseitige dazu eine `Where` Klausel in der Abfrage, die eine Filter-Prädikat für die Abfrage für die Dokumentsammlung gilt:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Diese Abfrage ruft alle Dokumente aus der Auflistung zurück, deren `Done` Eigenschaft `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Einfügen von einem Dokument in einer Dokumentsammlung

Dokumente können in einer Dokumentsammlung mit eingefügt werden und werden benutzerdefinierte JSON-Inhalt der `DocumentClient.CreateDocumentAsync` Methode:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

Die `CreateDocumentAsync` Methode gibt eine `Uri` Argument, das die Auflistung darstellt, das Dokument eingefügt werden soll, und ein `object` Argument, das das Dokument eingefügt werden soll darstellt.

### <a name="replacing-a-document-in-a-document-collection"></a>Ersetzen eines Dokuments in einer Dokumentsammlung

Dokumente können in einer Dokumentsammlung mit ersetzt werden, die `DocumentClient.ReplaceDocumentAsync` Methode:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

Die `ReplaceDocumentAsync` Methode gibt eine `Uri` Argument, das das Dokument in der Auflistung, die ersetzt werden soll darstellt, und ein `object` Argument, das aktualisierte Dokumentdaten darstellt.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Löschen eines Dokuments in einer Dokumentsammlung

Ein Dokument kann gelöscht werden, aus einer Auflistung Dokument mit den `DocumentClient.DeleteDocumentAsync` Methode:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

Die `DeleteDocumentAsync` Methode gibt eine `Uri` Argument, das das Dokument in der Auflistung darstellt, die gelöscht werden soll.

### <a name="deleting-a-document-collection"></a>Löschen einer Dokumentsammlung

Eine Dokument-Auflistung kann gelöscht werden, aus einer Datenbank mit der `DocumentClient.DeleteDocumentCollectionAsync` Methode:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

Die `DeleteDocumentCollectionAsync` Methode gibt eine `Uri` Argument, das die Dokumentsammlung zu löschenden darstellt. Beachten Sie, dass das Aufrufen dieser Methode in der Auflistung gespeicherten Dokumente auch gelöscht werden.

### <a name="deleting-a-database"></a>Löschen einer Datenbank

Eine Datenbank kann gelöscht werden, aus einem Cosmos DB-Datenbankkonto mit der `DocumentClient.DeleteDatabaesAsync` Methode:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

Die `DeleteDatabaseAsync` Methode gibt eine `Uri` Argument, das die zu löschende Datenbank darstellt. Beachten Sie, dass das Aufrufen dieser Methode auch die Dokumentsammlungen, die in der Datenbank gespeichert und in den dokumentauflistungen gespeicherten Dokumente gelöscht werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure Cosmos DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren. Eine Azure Cosmos DB-dokumentdatenbank ist eine NoSQL-Datenbank, die Zugriff mit geringer Latenz für JSON-Dokumenten, bietet einen schnelle, hoch verfügbare, skalierbare Datenbankdienst für Anwendungen, die eine nahtlose Skalierung und globale Replikation erfordern bereitstellt.


## <a name="related-links"></a>Verwandte Links

- [TODO Azure Cosmos DB (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Dokumentation für Azure Cosmos DB](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard-Clientbibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB-API](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
