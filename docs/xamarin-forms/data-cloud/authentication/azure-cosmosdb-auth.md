---
Title: "Authentifizieren von Benutzern mit einer Azure Cosmos DB-dokumentdatenbank und Xamarin.Forms " Beschreibung: in diesem Artikel wird erläutert, wie Sie die Zugriffs Steuerung mit Azure Cosmos DB partitionierten Sammlungen kombinieren, sodass ein Benutzer nur auf seine eigenen Dokumente in einer-Anwendung zugreifen kann Xamarin.Forms . "
ms. Prod: xamarin ms. assetid: 11ed4a4c-0F 05-40b2-ab06-5a0f 2188ef3d ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 06/16/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Authentifizieren von Benutzern mit einer Azure Cosmos DB-dokumentdatenbank undXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB Dokumenten Datenbanken unterstützen partitionierte Sammlungen, die mehrere Server und Partitionen umfassen können und gleichzeitig unbegrenzten Speicher und Durchsatz unterstützen. In diesem Artikel wird erläutert, wie Sie die Zugriffs Steuerung mit partitionierten Sammlungen kombinieren, sodass ein Benutzer nur auf seine eigenen Dokumente in einer-Anwendung zugreifen kann Xamarin.Forms ._

## <a name="overview"></a>Übersicht

Beim Erstellen einer partitionierten Sammlung muss ein Partitions Schlüssel angegeben werden, und Dokumente mit demselben Partitions Schlüssel werden in derselben Partition gespeichert. Daher führt das Angeben der Identität des Benutzers als Partitions Schlüssel zu einer partitionierten Sammlung, in der nur Dokumente für diesen Benutzer gespeichert werden. Dadurch wird auch sichergestellt, dass die Azure Cosmos DB-dokumentdatenbank bei einer Erhöhung der Anzahl von Benutzern und Elementen skaliert wird.

Jeder Sammlung muss Zugriff gewährt werden, und das SQL-API-Zugriffs Steuerungsmodell definiert zwei Typen von zugriffskonstrukten:

- **Hauptschlüssel** ermöglichen vollständigen Administrator Zugriff auf alle Ressourcen innerhalb eines Cosmos DB Kontos und werden erstellt, wenn ein Cosmos DB Konto erstellt wird.
- **Ressourcen Token** erfassen die Beziehung zwischen dem Benutzer einer Datenbank und der Berechtigung, über die der Benutzer für eine bestimmte Cosmos DB Ressource verfügt, z. b. eine Sammlung oder ein Dokument.

Wenn Sie einen Hauptschlüssel verfügbar machen, öffnet sich ein Cosmos DB Konto, um die Möglichkeit einer schädlichen oder fahrlässigen Verwendung zu Azure Cosmos DB-Ressourcen Token stellen jedoch einen sicheren Mechanismus dar, mit dem Clients bestimmte Ressourcen in einem Azure Cosmos DB Konto gemäß den gewährten Berechtigungen lesen, schreiben und löschen können.

Ein typischer Ansatz zum anfordern, erstellen und bereitzustellen von Ressourcen Token an eine mobile Anwendung ist die Verwendung eines ressourcentokenbrokers. Das folgende Diagramm zeigt eine allgemeine Übersicht darüber, wie die Beispielanwendung einen ressourcentokenbroker verwendet, um den Zugriff auf die Dokument Datenbankdaten zu verwalten:

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

Beim ressourcentokenbroker handelt es sich um einen in Azure App Service gehosteten Web-API-Dienst der mittleren Ebene, der über den Hauptschlüssel des Cosmos DB Kontos verfügt. Die Beispielanwendung verwendet den ressourcentokenbroker, um den Zugriff auf die Dokumenten Datenbankdaten wie folgt zu verwalten:

1. Bei der Anmeldung Xamarin.Forms kontaktiert die Anwendung Azure App Service, um einen Authentifizierungs Fluss zu initiieren.
1. Azure App Service führt einen OAuth-Authentifizierungs Fluss mit Facebook aus. Nachdem der Authentifizierungs Ablauf abgeschlossen ist, Xamarin.Forms empfängt die Anwendung ein Zugriffs Token.
1. Die Xamarin.Forms Anwendung verwendet das Zugriffs Token, um ein Ressourcen Token vom ressourcentokenbroker anzufordern.
1. Der ressourcentokenbroker verwendet das Zugriffs Token, um die Identität des Benutzers von Facebook anzufordern. Die Identität des Benutzers wird dann verwendet, um ein Ressourcen Token von Cosmos DB anzufordern, das verwendet wird, um Lese-/Schreibzugriff auf die partitionierte Sammlung des authentifizierten Benutzers zu gewähren.
1. Die Xamarin.Forms Anwendung verwendet das Ressourcen Token für den direkten Zugriff auf Cosmos DB Ressourcen mit den Berechtigungen, die durch das Ressourcen Token definiert werden.

> [!NOTE]
> Wenn das Ressourcen Token abläuft, erhalten nachfolgende Dokument Datenbankanforderungen die Ausnahme 401 nicht autorisiert. An diesem Punkt Xamarin.Forms sollten die Anwendungen die Identität erneut einrichten und ein neues Ressourcen Token anfordern.

Weitere Informationen zur Cosmos DB Partitionierung finden [Sie unter Partitionieren und Skalieren in Azure Cosmos DB](/azure/cosmos-db/partition-data/). Weitere Informationen zur Cosmos DB Access Control finden Sie unter [Sichern des Zugriffs auf Cosmos DB Daten](/azure/cosmos-db/secure-access-to-data/) und [Zugriffs Steuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Einrichten

Der Vorgang zum Integrieren des ressourcentokenbrokers in eine- Xamarin.Forms Anwendung lautet wie folgt:

1. Erstellen Sie ein Cosmos DB Konto, das die Zugriffs Steuerung verwendet. Weitere Informationen finden Sie unter [Azure Cosmos DB-Konfiguration](#azure-cosmos-db-configuration).
1. Erstellen Sie eine Azure App Service zum Hosten des ressourcentokenbrokers. Weitere Informationen finden Sie unter [Azure App Service-Konfiguration](#azure-app-service-configuration).
1. Erstellen Sie eine Facebook-App zum Durchführen der Authentifizierung. Weitere Informationen finden Sie unter [Facebook-App-Konfiguration](#facebook-app-configuration).
1. Konfigurieren Sie die Azure App Service, um eine einfache Authentifizierung mit Facebook auszuführen. Weitere Informationen finden Sie unter [Azure App Service-Authentifizierungs Konfiguration](#azure-app-service-authentication-configuration).
1. Konfigurieren Xamarin.Forms Sie die Beispielanwendung für die Kommunikation mit Azure App Service und Cosmos DB. Weitere Informationen finden Sie unter [ Xamarin.Forms Anwendungskonfiguration](#xamarinforms-application-configuration).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB Konfiguration

Der Prozess zum Erstellen eines Cosmos DB Kontos, für das die Zugriffs Steuerung verwendet wird, lautet wie folgt:

1. Erstellen Sie ein Cosmos DB Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Cosmos DB Kontos](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Erstellen Sie im Cosmos DB-Konto eine neue Sammlung mit dem Namen, und geben Sie dabei den `UserItems` Partitions Schlüssel an `/userid` .

### <a name="azure-app-service-configuration"></a>Azure App Service Konfiguration

Der Vorgang zum Hosting des ressourcentokenbrokers in Azure App Service lautet wie folgt:

1. Erstellen Sie im Azure-Portal eine neue APP Service Web-App. Weitere Informationen finden Sie unter [Erstellen einer Web-App in einem App Service-Umgebung](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Öffnen Sie im Azure-Portal das Blatt mit den App-Einstellungen für die Web-App, und fügen Sie die folgenden Einstellungen hinzu:
    - `accountUrl`– der Wert muss die Cosmos DB Konto-URL auf dem Blatt "Schlüssel" des Cosmos DB Kontos sein.
    - `accountKey`– der Wert sollte der Cosmos DB Haupt Schlüssels (primär oder sekundär) auf dem Blatt "Schlüssel" des Cosmos DB Kontos sein.
    - `databaseId`– der Wert sollte der Name der Cosmos DB Datenbank sein.
    - `collectionId`– der Wert sollte der Name der Cosmos DB Auflistung sein (in diesem Fall `UserItems` ).
    - `hostUrl`– der Wert sollte die URL der Web-App auf dem Übersichtsblatt des App Service Kontos sein.

    Der folgende Screenshot veranschaulicht diese Konfiguration:

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. Veröffentlichen Sie die ressourcentokenbroker-Lösung in der Azure App Service-Web-App.

### <a name="facebook-app-configuration"></a>Facebook-App-Konfiguration

Der Prozess zum Erstellen einer Facebook-App zum Durchführen der Authentifizierung lautet wie folgt:

1. Erstellen Sie eine Facebook-App. Weitere Informationen finden Sie unter [registrieren und Konfigurieren einer APP](https://developers.facebook.com/docs/apps/register) im Facebook Developer Center.
1. Fügen Sie der APP das Facebook-Anmelde Produkt hinzu. Weitere Informationen finden Sie unter [Hinzufügen einer Facebook-Anmeldung zu Ihrer APP oder Website](https://developers.facebook.com/docs/facebook-login) im Facebook Developer Center.
1. Konfigurieren Sie die Facebook-Anmeldung wie folgt:
   - Aktivieren Sie die OAuth-Client Anmeldung.
   - Aktivieren Sie die Web-OAuth-Anmeldung.
   - Legen Sie den gültigen OAuth-Umleitungs-URI auf den URI der APP Service Web-App mit angefügter fest `/.auth/login/facebook/callback` .

  Der folgende Screenshot veranschaulicht diese Konfiguration:

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

Weitere Informationen finden Sie unter [Registrieren Ihrer Anwendung bei Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

### <a name="azure-app-service-authentication-configuration"></a>Konfiguration der Azure App Service Authentifizierung

Der Prozess für die Konfiguration App Service einfachen Authentifizierung lautet wie folgt:

1. Navigieren Sie im Azure-Portal zur APP Service Web-App.
1. Öffnen Sie das Blatt Authentifizierung/Autorisierung im Azure-Portal, und führen Sie die folgende Konfiguration aus:
    - App Service Authentifizierung sollte aktiviert sein.
    - Die Aktion, die ausgeführt wird, wenn eine Anforderung nicht authentifiziert ist, sollte für die **Anmeldung mit Facebook**festgelegt sein.

    Der folgende Screenshot veranschaulicht diese Konfiguration:

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

Außerdem sollte die APP Service Web-App für die Kommunikation mit der Facebook-App konfiguriert werden, um den Authentifizierungs Ablauf zu aktivieren. Wählen Sie hierzu den Facebook-Identitäts Anbieter aus, und geben Sie die Werte für **App-ID** und **App-Geheimnis** aus den Facebook-App-Einstellungen im Facebook Developer Center ein. Weitere Informationen finden Sie [unter Hinzufügen von Facebook-Informationen zu Ihrer Anwendung](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

### <a name="xamarinforms-application-configuration"></a>Xamarin.FormsAnwendungskonfiguration

Der Vorgang zum Konfigurieren der Xamarin.Forms Beispielanwendung lautet wie folgt:

1. Öffnen Sie die Projekt Mappe Xamarin.Forms .
1. Öffnen Sie, `Constants.cs` und aktualisieren Sie die Werte der folgenden Konstanten:
    - `EndpointUri`– der Wert muss die Cosmos DB Konto-URL auf dem Blatt "Schlüssel" des Cosmos DB Kontos sein.
    - `DatabaseName`– der Wert sollte der Name der Dokumentendatenbank sein.
    - `CollectionName`– der Wert sollte der Name der Dokumenten Datenbanksammlung sein (in diesem Fall `UserItems` ).
    - `ResourceTokenBrokerUrl`– der Wert sollte die URL der ressourcentokenbroker-Web-App auf dem Übersichtsblatt des App Service Kontos sein.

## <a name="initiating-login"></a>Anmelde Name wird initiiert

Die Beispielanwendung initiiert den Anmeldevorgang, indem ein Browser an eine Identitäts Anbieter-URL umgeleitet wird, wie im folgenden Beispielcode gezeigt:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Dies bewirkt, dass ein OAuth-Authentifizierungs Fluss zwischen Azure App Service und Facebook initiiert wird, der die Facebook-Anmeldeseite anzeigt:

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

Der Anmelde Name kann durch Drücken der Schaltfläche " **Abbrechen** " unter IOS oder durch Drücken der Schaltfläche " **zurück** " unter Android abgebrochen werden. in diesem Fall bleibt der Benutzer nicht authentifiziert, und die Benutzeroberfläche des Identitäts Anbieters wird vom Bildschirm entfernt.

## <a name="obtaining-a-resource-token"></a>Abrufen eines Ressourcen Tokens

Nach erfolgreicher Authentifizierung wird das- `WebRedirectAuthenticator.Completed` Ereignis ausgelöst. Das folgende Codebeispiel veranschaulicht die Behandlung dieses Ereignisses:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Das Ergebnis einer erfolgreichen Authentifizierung ist ein Zugriffs Token, das die verfügbare `AuthenticatorCompletedEventArgs.Account` Eigenschaft ist. Das Zugriffs Token wird extrahiert und in einer GET-Anforderung an die API des ressourcentokenbrokers verwendet `resourcetoken` .

Die `resourcetoken` API verwendet das Zugriffs Token, um die Identität des Benutzers von Facebook anzufordern, der wiederum verwendet wird, um ein Ressourcen Token von Cosmos DB anzufordern. Wenn für den Benutzer bereits ein gültiges Berechtigungs Dokument in der dokumentdatenbank vorhanden ist, wird es abgerufen, und es wird ein JSON-Dokument mit dem Ressourcen Token an die Anwendung zurückgegeben Xamarin.Forms . Wenn für den Benutzer kein gültiges Berechtigungs Dokument vorhanden ist, wird ein Benutzer und eine Berechtigung in der dokumentdatenbank erstellt, und das Ressourcen Token wird aus dem Berechtigungs Dokument extrahiert und Xamarin.Forms in einem JSON-Dokument an die Anwendung zurückgegeben.

> [!NOTE]
> Ein Dokument Datenbankbenutzer ist eine Ressource, die einer dokumentdatenbank zugeordnet ist, und jede Datenbank enthält möglicherweise 0 (null) oder mehr Benutzer. Bei einer Dokument Daten Bank Berechtigung handelt es sich um eine Ressource, die einem Dokument Datenbankbenutzer zugeordnet ist, und jeder Benutzer kann NULL oder mehr Berechtigungen enthalten. Eine Berechtigungs Ressource ermöglicht den Zugriff auf ein Sicherheits Token, das der Benutzer für den Zugriff auf eine Ressource, z. b. ein Dokument, benötigt.

Wenn die `resourcetoken` API erfolgreich abgeschlossen wird, sendet Sie in der Antwort den HTTP-Statuscode 200 (OK) zusammen mit einem JSON-Dokument, das das Ressourcen Token enthält. Die folgenden JSON-Daten zeigen eine typische erfolgreiche Antwortnachricht:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

Der `WebRedirectAuthenticator.Completed` Ereignishandler liest die Antwort aus der `resourcetoken` API und extrahiert das Ressourcen Token und die Benutzer-ID. Das Ressourcen Token wird dann als Argument an den- `DocumentClient` Konstruktor übergeben, der den Endpunkt, die Anmelde Informationen und die Verbindungsrichtlinie kapselt, die für den Zugriff auf Cosmos DB verwendet werden, und wird zum Konfigurieren und Ausführen von Anforderungen für Cosmos DB verwendet. Das Ressourcen Token wird mit jeder Anforderung gesendet, um direkt auf eine Ressource zuzugreifen, und gibt an, dass der Lese-/Schreibzugriff auf die partitionierte Sammlung der authentifizierten Benutzer erteilt wird.

## <a name="retrieving-documents"></a>Abrufen von Dokumenten

Zum Abrufen von Dokumenten, die nur zum authentifizierten Benutzer gehören, kann eine Dokument Abfrage erstellt werden, die die Benutzer-ID als Partitions Schlüssel enthält. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Die Abfrage Ruft asynchron alle Dokumente, die zu dem authentifizierten Benutzer gehören, aus der angegebenen Auflistung ab und platziert Sie zur Anzeige in einer Auflistung `List<TodoItem>` .

Die `CreateDocumentQuery<T>` -Methode gibt ein `Uri` -Argument an, das die Auflistung darstellt, die für Dokumente abgefragt werden soll, und ein- `FeedOptions` Objekt. Das `FeedOptions` -Objekt gibt an, dass eine unbegrenzte Anzahl von Elementen von der Abfrage zurückgegeben werden kann, und die ID des Benutzers als Partitions Schlüssel. Dadurch wird sichergestellt, dass nur Dokumente in der partitionierten Sammlung des Benutzers im Ergebnis zurückgegeben werden.

> [!NOTE]
> Beachten Sie, dass Berechtigungs Dokumente, die vom ressourcentokenbroker erstellt werden, in derselben Dokument Sammlung wie die von der Anwendung erstellten Dokumente gespeichert werden Xamarin.Forms . Daher enthält die Dokument Abfrage eine- `Where` Klausel, die ein Filter Prädikat auf die Abfrage für die Dokument Sammlung anwendet. Diese Klausel stellt sicher, dass Berechtigungs Dokumente nicht von der Dokument Auflistung zurückgegeben werden.

Weitere Informationen zum Abrufen von Dokumenten aus einer Dokument Auflistung finden Sie unter [Abrufen von Dokumenten Sammlungs Dokumenten](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#retrieving-document-collection-documents).

## <a name="inserting-documents"></a>Einfügen von Dokumenten

Vor dem Einfügen eines Dokuments in eine Dokument Auflistung sollte die- `TodoItem.UserId` Eigenschaft mit dem Wert aktualisiert werden, der als Partitions Schlüssel verwendet wird, wie im folgenden Codebeispiel gezeigt:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Dadurch wird sichergestellt, dass das Dokument in die partitionierte Sammlung des Benutzers eingefügt wird.

Weitere Informationen zum Einfügen eines Dokuments in eine Dokument Auflistung finden Sie unter [Einfügen eines Dokuments in eine Dokument Sammlung](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting-a-document-into-a-document-collection).

## <a name="deleting-documents"></a>Löschen von Dokumenten

Der Partitions Schlüsselwert muss angegeben werden, wenn ein Dokument aus einer partitionierten Sammlung gelöscht wird, wie im folgenden Codebeispiel gezeigt:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Dadurch wird sichergestellt, dass Cosmos DB weiß, von welcher partitionierten Sammlung das Dokument gelöscht werden soll.

Weitere Informationen zum Löschen eines Dokuments aus einer Dokument Auflistung finden Sie unter [Löschen eines Dokuments aus einer Dokument Sammlung](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting-a-document-from-a-document-collection).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie die Zugriffs Steuerung mit partitionierten Sammlungen kombinieren, sodass ein Benutzer nur auf seine eigenen Dokument Daten Bankdokumente in einer-Anwendung zugreifen kann Xamarin.Forms . Durch die Angabe der Identität des Benutzers als Partitions Schlüssel wird sichergestellt, dass eine partitionierte Sammlung nur Dokumente für diesen Benutzer speichern kann.

## <a name="related-links"></a>Verwandte Links

- [TODO Azure Cosmos DB auth (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Verwenden einer Azure Cosmos DB-Dokumentdatenbank](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Sichern des Zugriffs auf Azure Cosmos DB-Daten](/azure/cosmos-db/secure-access-to-data/)
- [Zugriffs Steuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Partitionieren und Skalieren in Azure Cosmos DB](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB-Client Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB-API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
