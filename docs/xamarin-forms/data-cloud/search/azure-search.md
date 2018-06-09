---
title: Suchen nach Daten mit Azure Search
description: Dieser Artikel veranschaulicht, wie der Microsoft Azure Search-Bibliothek zur Integration von Azure Search in einer Xamarin.Forms-Anwendung.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: bb1ebec25d747f1188f39e9c9032145bcdc3cb97
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242127"
---
# <a name="searching-data-with-azure-search"></a>Suchen nach Daten mit Azure Search

_Azure Search ist ein Cloud-Dienst, der Indizierung und Abfragefunktionen für hochgeladene Daten bereitstellt. Dies entfernt die Anforderungen an die Infrastruktur und die Suche Algorithmus Komplexitäten, die normalerweise bei der Implementierung von Suchfunktionen in einer Anwendung verknüpft sind. Dieser Artikel veranschaulicht, wie der Microsoft Azure Search-Bibliothek zur Integration von Azure Search in einer Xamarin.Forms-Anwendung._

## <a name="overview"></a>Übersicht

Daten werden in Azure Search als Indizes und Dokumente gespeichert. Ein *Index* ist ein Speicher für Daten, die vom Azure-Suchdienst gesucht werden können, und gleicht konzeptionell in einer Datenbanktabelle. Ein *Dokument* ist eine zentrale Bereitstellungseinheit durchsuchbare Daten in einem Index, und gleicht konzeptionell Zeile in einer Datenbank. Beim Hochladen von Dokumenten und Übermitteln von Suchabfragen für Azure Search, Anforderungen an einen bestimmten Index in der Search-Dienst vorgenommen.

Jede Anforderung an Azure Search muss den Namen des Diensts und eine API-Schlüssel enthalten. Es gibt zwei Arten von API-Schlüssel:

- *Administratorschlüssel* Vollzugriff auf alle Vorgänge gewähren. Dies schließt das Verwalten des Diensts, erstellen und Löschen von Indizes und Datenquellen.
- *Abfragen von Schlüsseln* nur-Lese Zugriff auf Indizes und Dokumente gewähren, und sollte von Anwendungen, die suchanforderungen ausgeben verwendet werden.

Die am häufigsten verwendete Anforderungen für Azure Search ist zum Ausführen einer Abfrage. Es gibt zwei Arten von Abfragen, die übermittelt werden können:

- Ein *Suche* Abfrage sucht nach ein oder mehrere Elemente in der alle durchsuchbaren Felder in einem Index. Suchabfragen werden über die vereinfachte Syntax oder die Abfragesyntax Lucene erstellt. Weitere Informationen finden Sie unter [einfache Abfragesyntax in Azure Search](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), und [Lucene-Abfragesyntax in Azure Search](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- Ein *Filter* Abfrage ergibt einen booleschen Ausdruck über alle filterbaren Feldern in einem Index. Filterabfragen werden mit einer Teilmenge der OData-Filtersprache erstellt. Weitere Informationen finden Sie unter [OData-Ausdruckssyntax für Azure Search](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Suchabfragen und Filterabfragen können separat oder zusammen verwendet werden. Bei kombinierter Verwendung die Filterabfrage für den gesamten Index zuerst angewendet wird, und klicken Sie dann die Suchabfrage ausgeführt wird, auf dem die Ergebnisse der Filterabfrage.

Azure Search unterstützt auch abrufen Vorschläge basierend auf Suchen. Weitere Informationen finden Sie unter [Vorschlag Abfragen](#suggestions).

## <a name="setup"></a>Setup

Der Prozess für die Integration von Azure Search in einer Xamarin.Forms-Anwendung lautet wie folgt:

1. Erstellen Sie einen Azure Search-Dienst. Weitere Informationen finden Sie unter [Azure Search-Dienst über das Azure-Portal erstellen](/azure/search/search-create-service-portal/).
1. Entfernen Sie Silverlight als Zielframework aus Xamarin.Forms-Projektmappe Portable Klassenbibliothek (PCL). Dies kann erfolgen, indem Sie das PCL-Profil in alle Profile, die plattformübergreifende Entwicklung unterstützt, unterstützt jedoch keine Silverlight, z. B. 151-Profil oder 92 ändern.
1. Hinzufügen der [Microsoft Azure Search-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Search) NuGet-Paket zum PCL-Projekt in der Xamarin.Forms-Projektmappe.

Nach dem Ausführen dieser Schritte, kann der Microsoft Search-Library-API verwendet werden, zum Suchen von Indizes und Datenquellen verwalten, hochladen und Verwalten von Dokumenten und Abfragen ausführen.

## <a name="creating-the-azure-search-index"></a>Erstellen die Azure Search-Index

Einem IndexSchema muss definiert werden, die die Struktur der Daten zu durchsuchenden zugeordnet ist. Dies kann abgeschlossenen im Azure-Verwaltungsportal oder programmgesteuert mit der `SearchServiceClient` Klasse. Diese Klasse verwaltet die Verbindungen mit Azure Search und zum Erstellen eines Indexes verwendet werden kann. Im folgenden Codebeispiel wird veranschaulicht, wie eine Instanz dieser Klasse erstellt wird:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

Die `SearchServiceClient` Konstruktorüberladung akzeptiert ein Dienstname und eine `SearchCredentials` -Objekt als Argumente, mit der `SearchCredentials` Wrapperobjekt der *Administratorschlüssel* für den Azure-Suchdienst. Die *Administratorschlüssel* ist erforderlich, um einen Index zu erstellen.

> [!NOTE]
>  Ein einzelnes `SearchServiceClient` Instanz sollte in einer Anwendung verwendet werden, um zu vermeiden, öffnen zu viele Verbindungen mit Azure Search.

Ein Index definiert wurde, durch die `Index` -Objekts, wie im folgenden Codebeispiel veranschaulicht:

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

Die `Index.Name` Eigenschaft sollte festgelegt werden, auf den Namen des Indexes, und die `Index.Fields` Eigenschaft sollte festgelegt werden, um ein Array von `Field` Objekte. Jede `Field` Instanz gibt einen Namen, einen Typ und alle Eigenschaften, die angeben, wie das Feld verwendet wird. Zu diesen Eigenschaften zählen:

- `IsKey` – Gibt an, ob das Feld der Schlüssel des Indexes ist. Nur ein Feld im Index vom Typ `DataType.String`, muss als Schlüsselfeld festgelegt werden.
- `IsFacetable` – Gibt an, ob es möglich ist, führen Sie die facettennavigation auf dieses Feld. Der Standardwert ist `false`.
- `IsFilterable` – Gibt an, ob das Feld in Filterabfragen verwendet werden kann. Der Standardwert ist `false`.
- `IsRetrievable` – Gibt an, ob das Feld in den Suchergebnissen angezeigter abgerufen werden kann. Der Standardwert ist `true`.
- `IsSearchable` – Gibt an, ob das Feld in Volltextsuchen enthalten ist. Der Standardwert ist `false`.
- `IsSortable` – Gibt an, ob das Feld in verwendet werden kann `OrderBy` Ausdrücke. Der Standardwert ist `false`.

> [!NOTE]
> Ändern einen Index aus, nachdem sie bereitgestellt wurde umfasst das Neuerstellen und erneutes Laden der Daten.

Ein `Index` Objekt kann optional angeben, ein `Suggesters` Eigenschaft, die die Felder im Index verwendet werden, zur Unterstützung einer AutoVervollständigen oder Vorschlag Suchabfragen definiert. Die `Suggesters` Eigenschaft sollte festgelegt werden, um ein Array von `Suggester` -Objekten, die Felder, die verwendet werden definieren, um die Suche Vorschlag Ergebnisse erstellen.

Nach dem Erstellen der `Index` -Objekt, der Index wird erstellt, durch den Aufruf `Indexes.Create` auf die `SearchServiceClient` Instanz.

> [!NOTE]
> Beim Erstellen eines Indexes aus einer Anwendung reaktionsfähig bleiben muss, verwenden die `Indexes.CreateAsync` Methode.

Weitere Informationen finden Sie unter [erstellen Sie einen Azure Search-Index mit dem .NET SDK](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Löschen die Azure Search-Index

Ein Index kann gelöscht werden, durch den Aufruf `Indexes.Delete` auf die `SearchServiceClient` Instanz:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Hochladen von Daten in der Azure Search-Index

Nach dem Definieren des Indexes, können Daten mithilfe einer der beiden Modelle hochgeladen werden:

- **Pullmodell** : Daten in regelmäßigen Abständen aus dem Azure-Cosmos-Datenbank, Azure SQL-Datenbank und Azure Blob-Speicher aufgenommen werden, oder SQL Server gehostet auf einem virtuellen Azure-Computer.
- **Pushmodell** – Daten programmgesteuert auf den Index gesendet werden. Dies ist das Modell in diesem Artikel übernommen.

Ein `SearchIndexClient` Instanz muss zum Importieren von Daten in den Index erstellt werden. Dies geschieht durch Aufrufen der `SearchServiceClient.Indexes.GetClient` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Daten für den Import in den Index als verpackt ist ein `IndexBatch` -Objekt, das eine Auflistung von kapselt `IndexAction` Objekte. Jede `IndexAction` Instanz enthält, ein Dokument und eine Eigenschaft, die Azure Search mitteilt, welche Aktion Sie für das Dokument ausgeführt. Im Codebeispiel weiter oben wird die `IndexAction.Upload` Aktion angegeben wird, dies führt im Dokument in den Index eingefügt werden, wenn sie neu ist oder ersetzt, falls sie bereits vorhanden ist. Die `IndexBatch` Objekt wird dann gesendet, die dem Index durch Aufrufen der `Documents.Index` Methode für die `SearchIndexClient` Objekt. Informationen zu anderen Indizierung Aktionen finden Sie unter [entscheiden, welche Aktion Sie Volltextindizierung verwenden](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use).

> [!NOTE]
> Nur 1000 Dokumente können in einer einzelnen Anforderung für die Volltextindizierung einbezogen werden.

Beachten Sie, dass im oben genannten Beispiel den `monkeyList` Sammlung wird erstellt, als ein anonymes Objekt aus einer Auflistung von `Monkey` Objekte. Dies erstellt Daten für die `id` ein, und löst die Zuordnung der Pascal-Schreibweise `Monkey` Eigenschaftennamen in Camel-Case suchen, Index-Feldnamen. Alternativ können Sie diese Zuordnung kann auch vorgenommen werden durch Hinzufügen der `[SerializePropertyNamesAsCamelCase]` -Attribut auf die `Monkey` Klasse.

Weitere Informationen finden Sie unter [Hochladen von Daten in Azure Search .NET SDK mithilfe](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Abfragen des Azure Search-Indexes

Ein `SearchIndexClient` Instanz erstellt werden muss, um einen Index abzufragen. Wenn eine Anwendung Abfragen ausführt, ist es ratsam, befolgen das Prinzip der geringsten Rechte, und erstellen Sie eine `SearchIndexClient` direkt, übergeben die *Abfrageschlüssel* als Argument. Dadurch wird sichergestellt, dass Benutzer schreibgeschützten Zugriff auf Indizes und Dokumente. Dieser Ansatz wird in das folgende Codebeispiel veranschaulicht:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

Die `SearchIndexClient` Konstruktorüberladung akzeptiert ein Dienstname, den Indexnamen, und ein `SearchCredentials` -Objekt als Argumente, mit der `SearchCredentials` Wrapperobjekt der *Abfrageschlüssel* für den Azure-Suchdienst.

### <a name="search-queries"></a>Suchabfragen

Der Index kann abgefragt werden, durch Aufrufen der `Documents.SearchAsync` Methode für die `SearchIndexClient` Instanz, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

Die `SearchAsync` -Methode akzeptiert ein Argument der Search-Text und ein optionales `SearchParameters` -Objekt, das zum Verfeinern der Abfrage verwendet werden kann. Eine Suchabfrage als das Argument der Search-Text angegeben ist, während eine Filterabfrage kann, durch Festlegen angegeben werden der `Filter` Eigenschaft von der `SearchParameters` Argument. Im folgenden Codebeispiel wird veranschaulicht, dass beide Typen Abfragen:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Diese Filterabfrage für den gesamten Index angewendet wird, und entfernt Sie Dokumente aus den Ergebnissen, in denen die `location` Feld ist nicht gleich China und Vietnam nicht gleich. Die Suchabfrage erfolgt nach Filterung wird auf den Ergebnissen der Filterabfrage.

> [!NOTE]
> Übergeben Sie zum Filtern von ohne suchen `*` als das Argument der Search-Text.

Die `SearchAsync` Methode gibt ein `DocumentSearchResult` Objekt, das die Ergebnisse der Abfrage enthält. Dieses Objekt wird aufgelistet, mit jedem `Document` -Objekt erstellt wird, als ein `Monkey` Objekt hinzugefügt, und wählen Sie die `Monkeys` `ObservableCollection` für die Anzeige. Die folgenden Screenshots zeigen suchabfrageergebnissen aus Azure Search zurückgegeben:

![](azure-search-images/search.png "Suchergebnisse")

Weitere Informationen zum Suchen und Filtern finden Sie unter [Ihren Azure Search-Index mit dem SDK für .NET Abfragen](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Vorschlag Abfragen

Azure Search ermöglicht Vorschläge enthalten, die angefordert werden basierend auf einer Suchabfrage durch Aufrufen der `Documents.SuggestAsync` Methode für die `SearchIndexClient` Instanz. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

Die `SuggestAsync` Methode akzeptiert eine Suche Text-Argument, der Name des suggesters verwenden (die im Index definiert ist), und eine optionale `SuggestParameters` -Objekt, das zum Verfeinern der Abfrage verwendet werden kann. Die `SuggestParameters` Instanz legt die folgenden Eigenschaften fest:

- `UseFuzzyMatching` – Bei der Einstellung `true`, Azure Search findet Vorschläge, auch wenn es eine Zeichen anders ist oder fehlende in den Suchtext ein.
- `HighlightPreTag` – Das Tag aus den Vorschlag Treffer vorangestellt ist.
- `HighlightPostTag` – Das Tag, das Vorschlag Treffer angefügt wird.
- `MinimumCoverage` – stellt der Prozentsatz des Indexes, die von einer vorschlagsabfrage für die Abfrage abgedeckt werden müssen gemeldet Erfolg. Der Standardwert ist 80.
- `Top` – die Anzahl der abzurufenden Vorschläge. Es muss eine ganze Zahl zwischen 1 und 100 mit einem Standardwert von 5 sein.

Der Gesamteffekt besteht darin, dass die obersten 10 Ergebnisse aus dem Index mit zurückgegeben werden, erreichen von Hervorhebung und die Ergebnisse werden Dokumente, die implizit enthalten, die auf ähnliche Weise geschrieben Suchbegriffe enthalten.

Die `SuggestAsync` Methode gibt ein `DocumentSuggestResult` Objekt, das die Ergebnisse der Abfrage enthält. Dieses Objekt wird aufgelistet, mit jedem `Document` -Objekt erstellt wird, als ein `Monkey` Objekt hinzugefügt, und wählen Sie die `Monkeys` `ObservableCollection` für die Anzeige. Die folgenden Screenshots zeigen die Vorschlag Ergebnisse aus Azure Search zurückgegeben:

![](azure-search-images/suggest.png "Vorschlag-Ergebnisse")

Beachten Sie, dass in der beispielanwendung, die `SuggestAsync` Methode wird nur aufgerufen, wenn der Benutzer einen Suchbegriff eingeben. Allerdings kann es auch verwendet werden um AutoVervollständigen Suchabfragen zu unterstützen, durch das Ausführen auf jede Keypress.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie der Microsoft Azure Search-Bibliothek verwenden, um Azure Search in einer Xamarin.Forms-Anwendung zu integrieren. Azure Search ist ein Cloud-Dienst, der Indizierung und Abfragefunktionen für hochgeladene Daten bereitstellt. Dies entfernt die Anforderungen an die Infrastruktur und die Suche Algorithmus Komplexitäten, die normalerweise bei der Implementierung von Suchfunktionen in einer Anwendung verknüpft sind.


## <a name="related-links"></a>Verwandte Links

- [Azure-Suchdienst (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Azure Search-Dokumentation](/azure/search/)
- [Bibliothek für Microsoft Azure suchen](https://www.nuget.org/packages/Microsoft.Azure.Search/)
