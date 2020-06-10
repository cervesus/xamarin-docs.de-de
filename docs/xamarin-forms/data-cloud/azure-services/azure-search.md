---
Title: "Durchsuchen von Daten mit Azure Search und Xamarin.Forms " Beschreibung: "in diesem Artikel wird veranschaulicht, wie Sie die Microsoft Azure Search-Bibliothek verwenden, um Azure Search in eine-Anwendung zu integrieren Xamarin.Forms ."
ms. Prod: xamarin ms. assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 12/05/2016 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="search-data-with-azure-search-and-xamarinforms"></a>Durchsuchen von Daten mit Azure Search undXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Azure Search ist ein clouddienst, der Indizierungs-und Abfragefunktionen für hochgeladene Daten bereitstellt. Dadurch werden die Komplexitäten in Bezug auf die Infrastruktur und Suchalgorithmen entfernt In diesem Artikel wird veranschaulicht, wie Sie die Microsoft Azure Search-Bibliothek verwenden, um Azure Search in eine-Anwendung zu integrieren Xamarin.Forms ._

## <a name="overview"></a>Übersicht

Daten werden in Azure Search als Indizes und Dokumente gespeichert. Ein *Index* ist ein Datenspeicher, der vom Azure Search-Dienst durchsucht werden kann, und ist konzeptionell ähnlich wie eine Datenbanktabelle. Ein *Dokument* ist eine einzelne Einheit von durchsuchbaren Daten in einem Index und ist konzeptionell ähnlich wie eine Datenbankzeile. Beim Hochladen von Dokumenten und Übermitteln von Such Abfragen an Azure Search werden Anforderungen an einen bestimmten Index im Suchdienst gestellt.

Jede an Azure Search vorgenommene Anforderung muss den Namen des diensdienstanbieter und einen API-Schlüssel enthalten. Es gibt zwei Arten von API-Schlüsseln:

- *Administrator Schlüssel* gewähren vollständige Rechte für alle Vorgänge. Dies umfasst das Verwalten des dienstanens, das Erstellen und Löschen von Indizes und Datenquellen.
- Mit *Abfrage Schlüsseln* wird Schreib geschützter Zugriff auf Indizes und Dokumente gewährt. Sie sollten auch von Anwendungen verwendet werden, die Suchanforderungen ausgeben.

Die häufigste Anforderung an Azure Search ist die Ausführung einer Abfrage. Es gibt zwei Arten von Abfragen, die übermittelt werden können:

- Eine *Such* Abfrage sucht nach einem oder mehreren Elementen in allen durchsuchbaren Feldern in einem Index. Such Abfragen werden mithilfe der vereinfachten Syntax oder der Lucene-Abfrage Syntax erstellt. Weitere Informationen finden Sie unter [einfache Abfrage Syntax in Azure Search](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/)und [Lucene-Abfrage Syntax in Azure Search](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- Eine *Filter* Abfrage wertet einen booleschen Ausdruck für alle Filter baren Felder in einem Index aus. Filter Abfragen werden mithilfe einer Teilmenge der odata-Filter Sprache erstellt. Weitere Informationen finden Sie unter [odata-Ausdrucks Syntax für Azure Search](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Such Abfragen und Filter Abfragen können separat oder in kombinieren verwendet werden. Bei der gemeinsamen Verwendung wird die Filter Abfrage zuerst auf den gesamten Index angewendet, und dann wird die Suchabfrage für die Ergebnisse der Filter Abfrage durchgeführt.

Azure Search unterstützt auch das Abrufen von Vorschlägen basierend auf der Sucheingabe. Weitere Informationen finden Sie unter [Vorschlags Abfragen](#suggestion-queries).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

## <a name="setup"></a>Einrichten

Der Prozess für die Integration von Azure Search in eine- Xamarin.Forms Anwendung sieht wie folgt aus:

1. Erstellen Sie einen Azure Search-Dienst. Weitere Informationen finden Sie unter [Erstellen eines Azure Search Dienstanbieter mithilfe des Azure-Portals](/azure/search/search-create-service-portal/).
1. Entfernen Sie Silverlight als Ziel Framework aus der Projekt Mappe Xamarin.Forms portable Klassenbibliothek (PCL). Dies kann erreicht werden, indem Sie das PCL-Profil in ein beliebiges Profil ändern, das die plattformübergreifende Entwicklung unterstützt, aber Silverlight nicht unterstützt, z. b. Profil 151 oder Profil 92.
1. Fügen Sie das nuget-Paket für die [Microsoft Azure Suchbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Search) dem PCL-Projekt in der Projekt Mappe hinzu Xamarin.Forms .

Nachdem Sie diese Schritte ausgeführt haben, kann die Microsoft Search-Bibliotheks-API zum Verwalten von Such Indizes und Datenquellen, zum Hochladen und Verwalten von Dokumenten und zum Ausführen von Abfragen verwendet werden.

## <a name="creating-the-azure-search-index"></a>Erstellen eines Azure Search-Index

Es muss ein Index Schema definiert werden, das der Struktur der zu durchsuchenden Daten entspricht. Dies kann im Azure-Portal oder Programm gesteuert mithilfe der-Klasse erfolgen `SearchServiceClient` . Diese Klasse verwaltet Verbindungen mit Azure Search und kann verwendet werden, um einen Index zu erstellen. Im folgenden Codebeispiel wird veranschaulicht, wie eine Instanz dieser Klasse erstellt wird:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

Die `SearchServiceClient` Konstruktorüberladung übernimmt einen Suchdienst Namen und ein- `SearchCredentials` Objekt als Argumente, wobei das- `SearchCredentials` Objekt den *Administrator Schlüssel* für den Azure Search-Dienst umwickelt. Der *Administrator Schlüssel* ist erforderlich, um einen Index zu erstellen.

> [!NOTE]
> Eine einzelne `SearchServiceClient` Instanz sollte in einer Anwendung verwendet werden, um zu vermeiden, dass zu viele Verbindungen mit Azure Search geöffnet werden.

Ein Index wird durch das- `Index` Objekt definiert, wie im folgenden Codebeispiel gezeigt:

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

Die `Index.Name` -Eigenschaft sollte auf den Namen des Indexes festgelegt werden, und die- `Index.Fields` Eigenschaft sollte auf ein Array von-Objekten festgelegt werden `Field` . Jede `Field` Instanz gibt einen Namen, einen Typ und alle Eigenschaften an, die angeben, wie das Feld verwendet wird. Zu diesen Eigenschaften zählen folgende:

- `IsKey`– Gibt an, ob das Feld der Schlüssel des Indexes ist. Nur ein Feld im Index vom Typ `DataType.String` muss als Schlüsselfeld festgelegt werden.
- `IsFacetable`– Gibt an, ob es möglich ist, die Facetten Navigation für dieses Feld auszuführen. Der Standardwert ist `false`.
- `IsFilterable`– Gibt an, ob das Feld in Filter Abfragen verwendet werden kann. Der Standardwert ist `false`.
- `IsRetrievable`– Gibt an, ob das Feld in den Suchergebnissen abgerufen werden kann. Der Standardwert ist `true`.
- `IsSearchable`– Gibt an, ob das Feld in voll Text suchen eingeschlossen ist. Der Standardwert ist `false`.
- `IsSortable`– Gibt an, ob das Feld in Ausdrücken verwendet werden kann `OrderBy` . Der Standardwert ist `false`.

> [!NOTE]
> Wenn Sie einen Index nach seiner Bereitstellung ändern, werden die Daten neu erstellt und erneut geladen.

Ein `Index` Objekt kann optional eine `Suggesters` Eigenschaft angeben, die die Felder im Index definiert, die zur Unterstützung von Auto vervollständigen-oder Such Vorschlags Abfragen verwendet werden sollen. Die- `Suggesters` Eigenschaft sollte auf ein Array von-Objekten festgelegt werden `Suggester` , die die Felder definieren, die zum Erstellen der Such Vorschlags Ergebnisse verwendet werden.

Nachdem das- `Index` Objekt erstellt wurde, wird der Index durch Aufrufen von `Indexes.Create` für die- `SearchServiceClient` Instanz erstellt.

> [!NOTE]
> Verwenden Sie die-Methode, wenn Sie einen Index aus einer Anwendung erstellen, die weiterhin reaktionsfähig sein muss `Indexes.CreateAsync` .

Weitere Informationen finden Sie unter [Erstellen eines Azure Search Indexes mit dem .NET SDK](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Löschen des Azure Search Indexes

Ein Index kann durch Aufrufen von `Indexes.Delete` für die- `SearchServiceClient` Instanz gelöscht werden:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Hochladen von Daten in den Azure Search Index

Nachdem Sie den Index definiert haben, können Daten mithilfe eines von zwei Modellen in ihn hochgeladen werden:

- **Pull-Modell** – Daten werden in regelmäßigen Abständen aus Azure Cosmos DB, Azure SQL-Datenbank, Azure BLOB Storage oder SQL Server erfasst, die auf einem virtuellen Azure-Computer gehostet werden.
- **Push-Modell** – Daten werden Programm gesteuert an den Index gesendet. Dies ist das in diesem Artikel angenommene Modell.

Eine- `SearchIndexClient` Instanz muss erstellt werden, um Daten in den Index zu importieren. Dies können Sie erreichen, indem Sie die- `SearchServiceClient.Indexes.GetClient` Methode aufrufen, wie im folgenden Codebeispiel gezeigt:

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

Daten, die in den Index importiert werden sollen, werden als- `IndexBatch` Objekt verpackt, das eine Auflistung von-Objekten kapselt `IndexAction` . Jede `IndexAction` Instanz enthält ein Dokument und eine Eigenschaft, die angibt, Azure Search welche Aktion für das Dokument ausgeführt werden soll. Im obigen Codebeispiel wird die- `IndexAction.Upload` Aktion angegeben, die dazu führt, dass das Dokument in den Index eingefügt wird, wenn es neu ist, oder ersetzt wird, wenn es bereits vorhanden ist. Das- `IndexBatch` Objekt wird dann durch Aufrufen der- `Documents.Index` Methode für das-Objekt an den Index gesendet `SearchIndexClient` . Informationen zu anderen Indizierungs Aktionen finden [Sie unter entscheiden, welche Indizierungs Aktion verwendet werden soll](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use).

> [!NOTE]
> Nur 1000 Dokumente können in einer einzelnen Indizierungs Anforderung enthalten sein.

Beachten Sie, dass im obigen Codebeispiel die-Auflistung `monkeyList` als anonymes Objekt aus einer Auflistung von- `Monkey` Objekten erstellt wird. Dadurch werden die Daten für das `id` Feld erstellt, und die Zuordnung von Pascal-Case- `Monkey` Eigenschaftsnamen wird in die Feldnamen der Feldnamen für den Camel-Fall Alternativ kann diese Zuordnung auch durch Hinzufügen des- `[SerializePropertyNamesAsCamelCase]` Attributs zur- `Monkey` Klasse erreicht werden.

Weitere Informationen finden Sie unter [Hochladen von Daten in Azure Search mithilfe des .NET SDK](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Abfragen des Azure Search Indexes

Eine- `SearchIndexClient` Instanz muss erstellt werden, um einen Index abzufragen. Wenn eine Anwendung Abfragen ausführt, empfiehlt es sich, dem Prinzip der geringsten Berechtigung zu folgen und einen `SearchIndexClient` direkt zu erstellen, wobei der *Abfrage Schlüssel* als Argument übergeben wird. Dadurch wird sichergestellt, dass Benutzer schreibgeschützten Zugriff auf Indizes und Dokumente haben. Diese Vorgehensweise wird im folgenden Codebeispiel veranschaulicht:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

Die `SearchIndexClient` Konstruktorüberladung übernimmt einen Suchdienst Namen, einen Indexnamen und ein- `SearchCredentials` Objekt als Argumente, wobei das- `SearchCredentials` Objekt den *Abfrage Schlüssel* für den Azure Search-Dienst umfasst.

### <a name="search-queries"></a>Suchabfragen

Der Index kann durch Aufrufen der- `Documents.SearchAsync` Methode für die-Instanz abgefragt werden `SearchIndexClient` , wie im folgenden Codebeispiel gezeigt:

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

Die `SearchAsync` -Methode nimmt ein Suchtext-Argument und ein optionales- `SearchParameters` Objekt an, das verwendet werden kann, um die Abfrage weiter zu verfeinern. Eine Suchabfrage wird als Suchtext Argument angegeben, während eine Filter Abfrage durch Festlegen der-Eigenschaft des-Arguments angegeben werden kann `Filter` `SearchParameters` . Im folgenden Codebeispiel werden beide Abfrage Typen veranschaulicht:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Diese Filter Abfrage wird auf den gesamten Index angewendet und entfernt Dokumente aus den Ergebnissen, bei denen das `location` Feld nicht gleich China und nicht gleich Vietnam ist. Nach dem Filtern wird die Suchabfrage für die Ergebnisse der Filter Abfrage durchgeführt.

> [!NOTE]
> Wenn Sie ohne Suche filtern möchten, übergeben Sie `*` als Suchtext-Argument.

Die- `SearchAsync` Methode gibt ein- `DocumentSearchResult` Objekt zurück, das die Abfrageergebnisse enthält. Dieses Objekt wird aufgelistet, wobei jedes `Document` Objekt als `Monkey` -Objekt erstellt und der für die Anzeige hinzugefügt wird `Monkeys` `ObservableCollection` . Die folgenden Screenshots zeigen Ergebnisse der Suchabfrage, die von Azure Search zurückgegeben werden:

![](azure-search-images/search.png "Search Results")

Weitere Informationen zum Suchen und Filtern finden Sie unter [Abfragen des Azure Search Indexes mit dem .NET SDK](/azure/search/search-query-dotnet/).

### <a name="suggestion-queries"></a>Vorschlags Abfragen

Azure Search ermöglicht das Anfordern von Vorschlägen basierend auf einer Suchabfrage, indem die- `Documents.SuggestAsync` Methode für die-Instanz aufgerufen wird `SearchIndexClient` . Dies wird im folgenden Codebeispiel veranschaulicht:

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

Die `SuggestAsync` -Methode verwendet ein Suchtext-Argument, den Namen des zu verwendenden Vorschlags Bilds (das im Index definiert ist) und ein optionales `SuggestParameters` Objekt, das verwendet werden kann, um die Abfrage weiter zu verfeinern. Die- `SuggestParameters` Instanz legt die folgenden Eigenschaften fest:

- `UseFuzzyMatching`– Wenn der Wert auf festgelegt `true` ist, findet Azure Search auch dann Vorschläge, wenn im Suchtext ein ersetzendes oder fehlendes Zeichen vorhanden ist.
- `HighlightPreTag`– das Tag, dem Vorschläge vorangestellt wird.
- `HighlightPostTag`– das Tag, das an Vorschlags Treffer angefügt wird.
- `MinimumCoverage`– stellt den Prozentsatz des Indexes dar, der von einer Vorschlags Abfrage abgedeckt werden muss, damit die Abfrage als erfolgreich gemeldet wird. Der Standardwert ist 80.
- `Top`– die Anzahl der abzurufenden Vorschläge. Der Wert muss eine ganze Zahl zwischen 1 und 100 sein. der Standardwert ist 5.

Der Gesamteffekt besteht darin, dass die ersten 10 Ergebnisse aus dem Index mit der Treffer Markierung zurückgegeben werden, und die Ergebnisse enthalten Dokumente, die ähnlich geschriebene Suchbegriffe enthalten.

Die- `SuggestAsync` Methode gibt ein- `DocumentSuggestResult` Objekt zurück, das die Abfrageergebnisse enthält. Dieses Objekt wird aufgelistet, wobei jedes `Document` Objekt als `Monkey` -Objekt erstellt und der für die Anzeige hinzugefügt wird `Monkeys` `ObservableCollection` . Die folgenden Screenshots zeigen die Vorschlags Ergebnisse, die von Azure Search zurückgegeben werden:

![](azure-search-images/suggest.png "Suggestion Results")

Beachten Sie, dass die-Methode in der Beispielanwendung `SuggestAsync` nur aufgerufen wird, wenn der Benutzer das Einfügen eines Suchbegriffs abgeschlossen hat. Sie kann jedoch auch verwendet werden, um Such Abfragen mit automatischer Vervollständigung zu unterstützen, indem bei jedem KeyPress ausgeführt wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Sie die Microsoft Azure Search-Bibliothek verwenden, um Azure Search in eine-Anwendung zu integrieren Xamarin.Forms . Azure Search ist ein clouddienst, der Indizierungs-und Abfragefunktionen für hochgeladene Daten bereitstellt. Dadurch werden die Komplexitäten in Bezug auf die Infrastruktur und Suchalgorithmen entfernt

## <a name="related-links"></a>Verwandte Links

- [Azure Search (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Dokumentation zu Azure Search](/azure/search/)
- [Microsoft Azure Search Library](https://www.nuget.org/packages/Microsoft.Azure.Search/)
