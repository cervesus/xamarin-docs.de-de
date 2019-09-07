---
title: Daten mit Azure Search und xamarin. Forms suchen
description: In diesem Artikel veranschaulicht, wie die Bibliothek für Microsoft Azure Search, um Azure Search in einer Xamarin.Forms-Anwendung zu integrieren.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: ea2c733a9c85662b9286f8e8631b601248dc11de
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770838"
---
# <a name="search-data-with-azure-search-and-xamarinforms"></a>Daten mit Azure Search und xamarin. Forms suchen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Azure Search ist ein Clouddienst zur indizierungs- und Abfragefunktionen für hochgeladene Daten. Dies entfernt die Anforderungen an die Infrastruktur und die Suche Algorithmus Komplexitäten, die normalerweise bei der Implementierung von Suchfunktionen in einer Anwendung verknüpft ist. In diesem Artikel veranschaulicht, wie die Bibliothek für Microsoft Azure Search, um Azure Search in einer Xamarin.Forms-Anwendung zu integrieren._

## <a name="overview"></a>Übersicht

Daten werden in Azure Search als Indizes und Dokumente gespeichert. Ein *Index* ist ein Speicher für Daten, die von Azure Search-Diensts durchsucht werden können, und gleicht konzeptionell einer Datenbanktabelle. Ein *Dokument* ist eine Einheit mit durchsuchbaren Daten in einem Index, und gleicht konzeptionell einer Datenbankzeile. Beim Hochladen von Dokumenten und Suchabfragen an Azure Search übermitteln, Anforderungen an einen bestimmten Index in der Search-Dienst vorgenommen.

Jede Anforderung an Azure Search muss es sich um den Namen des Diensts, und einen API-Schlüssel enthalten. Es gibt zwei Arten von API-Schlüssel:

- *Admin-Schlüssel* Vollzugriff auf alle Vorgänge gewähren. Dies schließt das Verwalten des Dienstes, erstellen und Löschen von Indizes und Datenquellen.
- *Abfrageschlüssel* Lesezugriff auf Indizes und Dokumente zu gewähren, und sollte von Anwendungen, die Suchanfragen ausgeben verwendet werden.

Die am häufigsten verwendete Anforderung an Azure Search ist zum Ausführen einer Abfrage. Es gibt zwei Arten von Abfragen, die übermittelt werden können:

- Ein *Suche* Abfrage sucht nach ein oder mehrere Elemente in allen durchsuchbaren Feldern in einem Index. Suchabfragen werden über die vereinfachte Syntax oder die Lucene-Abfragesyntax erstellt. Weitere Informationen finden Sie unter [einfache Abfragesyntax in Azure Search](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), und [Lucene-Abfragesyntax in Azure Search](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- Ein *Filter* -Abfrage wertet einen booleschen Ausdruck über alle filterbare Felder in einem Index. Filtern Sie Abfragen werden mit einer Teilmenge der OData-Filtersprache erstellt. Weitere Informationen finden Sie unter [OData-Ausdruckssyntax für Azure Search](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Suchabfragen und Filtern Sie Abfragen können separat oder zusammen verwendet werden. Bei gemeinsamer Verwendung die Filterabfrage für den gesamten Index zuerst angewendet wird, und klicken Sie dann die Suchabfrage für die Ergebnisse der Filterabfrage ausgeführt wird.

Azure Search unterstützt auch beim Abrufen von Vorschlägen, die basierend auf der Sucheingabe. Weitere Informationen finden Sie unter [Vorschlag Abfragen](#suggestions).

## <a name="setup"></a>Setup

Der Prozess zur Integration von Azure Search in einer Xamarin.Forms-Anwendung lautet wie folgt aus:

1. Erstellen eines Azure Search-Diensts an. Weitere Informationen finden Sie unter [erstellen Sie einen Azure Search-Dienst über das Azure-Portal](/azure/search/search-create-service-portal/).
1. Entfernen Sie Silverlight als Zielframework aus der Xamarin.Forms-Projektmappe Portable Klassenbibliothek (PCL). Dies kann erfolgen, indem Sie das PCL-Profil in alle Profile, die plattformübergreifende Entwicklung unterstützt, unterstützt jedoch keine Silverlight, z. B. Profil 151 oder 92 ändern.
1. Hinzufügen der [Bibliothek für Microsoft Azure Search](https://www.nuget.org/packages/Microsoft.Azure.Search) NuGet-Paket zum PCL-Projekt in der Xamarin.Forms-Projektmappe.

Nach der Durchführung dieser Schritte kann der Microsoft Search-Library-API zum Verwalten von Search-Indizes und Datenquellen, hochladen und Verwalten von Dokumenten und Ausführen von Abfragen verwendet werden.

## <a name="creating-the-azure-search-index"></a>Erstellen von Azure Search-Index

Einem IndexSchema muss definiert werden, die die Struktur der Daten, die durchsucht werden zugeordnet. Dies kann sein, im Azure-Portal durchgeführt oder programmgesteuert mithilfe der `SearchServiceClient` Klasse. Diese Klasse verwaltet die Verbindungen mit Azure Search und kann zum Erstellen eines Indexes verwendet werden. Im folgenden Codebeispiel wird veranschaulicht, wie Sie eine Instanz dieser Klasse zu erstellen:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

Die `SearchServiceClient` Konstruktorüberladung einen Search-Dienstnamen entgegennimmt und ein `SearchCredentials` Objekt als Argumente, mit der `SearchCredentials` Objekt Wrapping der *Admin-Schlüssel* für den Azure Search-Dienst. Die *Administratorschlüssel* ist erforderlich, um einen Index zu erstellen.

> [!NOTE]
> Ein einzelnes `SearchServiceClient` Instanz sollte in einer Anwendung verwendet werden, um zu vermeiden, öffnen zu viele Verbindungen mit Azure Search.

Ein Index definiert wurde, indem die `Index` -Objekts, wie im folgenden Codebeispiel gezeigt:

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

Die `Index.Name` -Eigenschaft sollte auf den Namen des Index festgelegt werden und die `Index.Fields` Eigenschaft muss festgelegt werden, auf ein Array von `Field` Objekte. Jede `Field` Instanz gibt einen Namen, einen Typ und Eigenschaften, die angeben, wie das Feld verwendet wird. Zu diesen Eigenschaften zählen:

- `IsKey` – Gibt an, ob das Feld der Schlüssel des Indexes ist. Nur ein Feld im Index vom Typ `DataType.String`, als das Schlüsselfeld festgelegt werden muss.
- `IsFacetable` – Gibt an, ob es möglich ist, facettennavigation für dieses Feld ausführen. Der Standardwert ist `false`.
- `IsFilterable` – Gibt an, ob das Feld in Filterabfragen verwendet werden kann. Der Standardwert ist `false`.
- `IsRetrievable` – Gibt an, ob das Feld in Suchergebnissen abgerufen werden kann. Der Standardwert ist `true`.
- `IsSearchable` – Gibt an, ob das Feld in der Volltextsuche enthalten ist. Der Standardwert ist `false`.
- `IsSortable` – Gibt an, ob in das Feld verwendet werden kann `OrderBy` Ausdrücke. Der Standardwert ist `false`.

> [!NOTE]
> Nachdem sie bereitgestellt wurde, wird durch das Ändern eines Indexes beinhaltet, neu zu erstellen und das erneute Laden der Daten.

Ein `Index` Objekt kann optional angeben, ein `Suggesters` Eigenschaft, die die Felder im Index, an die verwendet werden, um die automatische Vervollständigung zu unterstützen oder Vorschlag Suchabfragen definiert. Die `Suggesters` Eigenschaft muss festgelegt werden, auf ein Array von `Suggester` -Objekten, die Felder zu, die verwendet werden definieren, um die Suche Vorschlag Ergebnisse erstellen.

Nach dem Erstellen der `Index` Objekt ist, wird der Index erstellt, durch den Aufruf `Indexes.Create` auf die `SearchServiceClient` Instanz.

> [!NOTE]
> Beim Erstellen eines Index aus einer Anwendung, die reaktionsfähig bleiben muss, verwenden die `Indexes.CreateAsync` Methode.

Weitere Informationen finden Sie unter [erstellen Sie einen Azure Search-Index mit dem .NET SDK](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Azure Search-Index löschen

Ein Index kann gelöscht werden, durch den Aufruf `Indexes.Delete` auf die `SearchServiceClient` Instanz:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Hochladen von Daten in Azure Search-Index

Nachdem der Index definiert haben, können Daten mithilfe eines von zwei Modellen hochgeladen werden:

- **Pullmodell** – Daten werden in regelmäßigen Abständen von Azure Cosmos DB, Azure SQL-Datenbank, Azure Blob Storage erfasst werden sollen, oder SQL Server, die auf einem virtuellen Azure-Computer gehostet.
- **Pushmodell** – Daten programmgesteuert auf den Index gesendet werden. Dies ist das Modell in diesem Artikel übernommen.

Ein `SearchIndexClient` Instanz muss zum Importieren von Daten in den Index erstellt werden. Dies kann erreicht werden, durch den Aufruf der `SearchServiceClient.Indexes.GetClient` -Methode, wie im folgenden Codebeispiel gezeigt:

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

Daten für den Import in den Index als gepackt ein `IndexBatch` -Objekt, das eine Auflistung von kapselt `IndexAction` Objekte. Jede `IndexAction` Instanz enthält, ein Dokument, und eine Eigenschaft, die Azure Search mitteilt, welche Aktion Sie für das Dokument ausgeführt. Im obigen Codebeispiel wird die `IndexAction.Upload` Aktion wird angegeben, wodurch das Dokument in den Index eingefügt wird, wenn es neu ist, ist oder ersetzt werden, wenn sie bereits vorhanden ist. Die `IndexBatch` Objekt wird dann gesendet, die dem Index durch Aufrufen der `Documents.Index` Methode für die `SearchIndexClient` Objekt. Weitere Informationen zu anderen Indizierung Aktionen finden Sie unter [entscheiden, welche indizierungsaktion verwendet](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use).

> [!NOTE]
> Nur 1000 Dokumente können in einer einzigen indizierungsanforderung enthalten sein.

Beachten Sie, dass im obigen Codebeispiel wird die `monkeyList` Sammlung wird erstellt, als ein anonymes Objekt aus einer Auflistung von `Monkey` Objekte. Dadurch, dass Daten für die `id` ein, und löst die Zuordnung der Pascal-Schreibweise `Monkey` Eigenschaftsnamen in Camel-Case suchen, Index-Feldnamen. Sie können auch diese Zuordnung kann auch erreicht werden durch das Hinzufügen der `[SerializePropertyNamesAsCamelCase]` -Attribut auf die `Monkey` Klasse.

Weitere Informationen finden Sie unter [Hochladen von Daten in Azure Search mit dem .NET SDK](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Abfragen von Azure Search-Index

Ein `SearchIndexClient` Instanz muss erstellt werden, um einen Index Abfragen. Wenn eine Anwendung Abfragen ausführt, ist es ratsam, befolgen das Prinzip der geringsten Rechte, und erstellen Sie eine `SearchIndexClient` direkt, übergeben die *Abfrageschlüssel* als Argument. Dadurch wird sichergestellt, dass Benutzer Lesezugriff auf Indizes und Dokumente. Dieser Ansatz wird im folgenden Codebeispiel veranschaulicht:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

Die `SearchIndexClient` Überladung des Konstruktors verwendet wird, einem Search-Dienstnamen, der Indexname und ein `SearchCredentials` Objekt als Argumente, mit der `SearchCredentials` Objekt Wrapping der *Abfrageschlüssel* für den Azure Search-Dienst.

### <a name="search-queries"></a>Suchabfragen

Der Index kann abgefragt werden, durch den Aufruf der `Documents.SearchAsync` Methode für die `SearchIndexClient` -Instanz, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `SearchAsync` Methode akzeptiert ein Argument der Search-Text und einem optionalen `SearchParameters` -Objekt, das zum weiteren Verfeinerung der Abfrage verwendet werden kann. Eine Suchabfrage wird als das Argument der Search-Text angegeben, während eine Filter-Abfrage kann, durch Festlegen angegeben werden der `Filter` Eigenschaft der `SearchParameters` Argument. Im folgenden Codebeispiel wird veranschaulicht, dass beide Typen Abfragen:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Diese Filterabfrage für den gesamten Index angewendet wird und Dokumenten aus den Ergebnissen entfernt, in denen die `location` Feld ist nicht in China gleich und ungleich Vietnam. Nach dem Filtern, wird die Suchabfrage für die Ergebnisse der Filterabfrage ausgeführt.

> [!NOTE]
> Um ohne Suche zu filtern, übergeben Sie `*` als das Argument der Search-Text.

Die `SearchAsync` Methode gibt eine `DocumentSearchResult` Objekt, das die Ergebnisse der Abfrage enthält. Dieses Objekt wird aufgelistet, mit jedem `Document` Objekt erstellt wird, als eine `Monkey` Objekt hinzugefügt, und wählen Sie die `Monkeys` `ObservableCollection` für die Anzeige. Die folgenden Screenshots zeigen Ergebnisse der Suchabfrage von Azure Search zurückgegebenen:

![](azure-search-images/search.png "Suchergebnisse")

Weitere Informationen suchen und filtern, finden Sie unter [Abfragen Ihrer Azure Search-Index mit dem .NET SDK](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Vorschlag Abfragen

Azure Search können angefordert werden Vorschläge basierend auf einer Suchabfrage, durch den Aufruf der `Documents.SuggestAsync` Methode für die `SearchIndexClient` Instanz. Dies wird im folgenden Codebeispiel veranschaulicht:

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

Die `SuggestAsync` Methode akzeptiert ein Search-Text-Argument, der Name des Vorschlags verwenden (die im Index definiert ist), und eine optionale `SuggestParameters` -Objekt, das zum weiteren Verfeinerung der Abfrage verwendet werden kann. Die `SuggestParameters` Instanz legt die folgenden Eigenschaften fest:

- `UseFuzzyMatching` – Bei Festlegung auf `true`, Azure Search findet Vorschläge, auch wenn es eine Zeichen anders ist oder fehlende im Suchtext ein.
- `HighlightPreTag` – Das Tag, das Vorschläge Treffern vorangestellt ist.
- `HighlightPostTag` – Das Tag, das Vorschläge Treffer angefügt wird.
- `MinimumCoverage` – stellt der Prozentsatz des Indexes, der von einer vorschlagsabfrage für die Abfrage abgedeckt werden muss, gemeldet Erfolg. Der Standardwert ist 80.
- `Top` – die Anzahl der abzurufenden Vorschläge. Es muss eine ganze Zahl zwischen 1 und 100 mit einem Standardwert von 5 sein.

Der Gesamteffekt besteht, dass die ersten 10 Ergebnisse aus dem Index mit zurückgegeben werden, Treffermarkierung und die Ergebnisse werden Dokumente, die enthalten, die auf ähnliche Weise geschrieben Suchbegriffe enthalten.

Die `SuggestAsync` Methode gibt eine `DocumentSuggestResult` Objekt, das die Ergebnisse der Abfrage enthält. Dieses Objekt wird aufgelistet, mit jedem `Document` Objekt erstellt wird, als eine `Monkey` Objekt hinzugefügt, und wählen Sie die `Monkeys` `ObservableCollection` für die Anzeige. Die folgenden Screenshots zeigen die von Azure Search zurückgegebenen Ergebnisse Vorschlag:

![](azure-search-images/suggest.png "Vorschlag Ergebnisse")

Beachten Sie, dass in der beispielanwendung, die `SuggestAsync` Methode wird nur aufgerufen, wenn der Benutzer einen Suchbegriff eingeben. Allerdings können sie auch verwendet werden, automatische Vervollständigung von Suchabfragen zu unterstützen, indem Sie auf jeden Tastendruck ausführen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie die Microsoft Azure Search-Bibliothek verwenden, um Azure Search in einer Xamarin.Forms-Anwendung zu integrieren. Azure Search ist ein Clouddienst zur indizierungs- und Abfragefunktionen für hochgeladene Daten. Dies entfernt die Anforderungen an die Infrastruktur und die Suche Algorithmus Komplexitäten, die normalerweise bei der Implementierung von Suchfunktionen in einer Anwendung verknüpft ist.

## <a name="related-links"></a>Verwandte Links

- [Azure Search (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Azure Search-Dokumentation](/azure/search/)
- [Microsoft Azure Search-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Search/)
