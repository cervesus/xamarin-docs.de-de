---
title: Authentifizieren von Benutzern mit einer Azure Cosmos DB-Dokumentdatenbank und Xamarin.Forms
description: In diesem Artikel wird erläutert, wie die Zugriffssteuerung mit partitionierten Azure Cosmos DB-Sammlungen kombiniert wird, sodass ein Benutzer nur in einer Xamarin.Forms-Anwendung auf seine eigenen Dokumente zugreifen kann.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e79e647d64103b6d257de7233f488899bcaff40
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388602"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Authentifizieren von Benutzern mit einer Azure Cosmos DB-Dokumentdatenbank und Xamarin.Forms

[![Beispiel](~/media/shared/download.png) herunterladen Download des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB-Dokumentdatenbanken unterstützen partitionierte Sammlungen, die mehrere Server und Partitionen umfassen können, während unbegrenzter Speicher und Durchsatz unterstützt werden. In diesem Artikel wird erläutert, wie Die Zugriffssteuerung mit partitionierten Auflistungen kombiniert wird, sodass ein Benutzer nur in einer Xamarin.Forms-Anwendung auf seine eigenen Dokumente zugreifen kann._

## <a name="overview"></a>Übersicht

Beim Erstellen einer partitionierten Auflistung muss ein Partitionsschlüssel angegeben werden, und Dokumente mit demselben Partitionsschlüssel werden in derselben Partition gespeichert. Daher führt die Angabe der Identität des Benutzers als Partitionsschlüssel zu einer partitionierten Auflistung, die nur Dokumente für diesen Benutzer speichert. Dadurch wird auch sichergestellt, dass die Azure Cosmos DB-Dokumentdatenbank skaliert wird, wenn die Anzahl der Benutzer und Elemente zunimmt.

Der Zugriff muss für jede Auflistung gewährt werden, und das SQL-API-Zugriffssteuerungsmodell definiert zwei Typen von Zugriffskonstrukten:

- **Hauptschlüssel** ermöglichen den vollständigen administrativen Zugriff auf alle Ressourcen innerhalb eines Cosmos DB-Kontos und werden erstellt, wenn ein Cosmos DB-Konto erstellt wird.
- **Ressourcentoken** erfassen die Beziehung zwischen dem Benutzer einer Datenbank und der Berechtigung, die der Benutzer für eine bestimmte Cosmos DB-Ressource, z. B. eine Auflistung oder ein Dokument, hat.

Wenn Sie einen Hauptschlüssel freilegen, wird ein Cosmos DB-Konto für die Möglichkeit einer böswilligen oder fahrlässigen Verwendung geöffnet. Azure Cosmos DB-Ressourcentoken bieten jedoch einen sicheren Mechanismus, mit dem Clients bestimmte Ressourcen in einem Azure Cosmos DB-Konto gemäß den erteilten Berechtigungen lesen, schreiben und löschen können.

Ein typischer Ansatz zum Anfordern, Generieren und Bereitstellen von Ressourcentoken an eine mobile Anwendung besteht in der Verwendung eines Ressourcentokenbrokers. Das folgende Diagramm zeigt eine Übersicht auf hoher Ebene darüber, wie die Beispielanwendung einen Ressourcentokenbroker verwendet, um den Zugriff auf die Dokumentdatenbankdaten zu verwalten:

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

Der Ressourcentokenbroker ist ein Web-API-Dienst der mittleren Ebene, der in Azure App Service gehostet wird und über den Hauptschlüssel des Cosmos DB-Kontos verfügt. Die Beispielanwendung verwendet den Ressourcentokenbroker, um den Zugriff auf die Dokumentdatenbankdaten wie folgt zu verwalten:

1. Bei der Anmeldung kontaktiert die Xamarin.Forms-Anwendung Azure App Service, um einen Authentifizierungsfluss zu initiieren.
1. Azure App Service führt einen OAuth-Authentifizierungsfluss mit Facebook durch. Nach Abschluss des Authentifizierungsflusses erhält die Xamarin.Forms-Anwendung ein Zugriffstoken.
1. Die Xamarin.Forms-Anwendung verwendet das Zugriffstoken, um ein Ressourcentoken vom Ressourcentokenbroker anzufordern.
1. Der Ressourcentokenbroker verwendet das Zugriffstoken, um die Identität des Benutzers von Facebook anzufordern. Die Identität des Benutzers wird dann verwendet, um ein Ressourcentoken von Cosmos DB anzufordern, das verwendet wird, um Lese-/Schreibzugriff auf die partitionierte Auflistung des authentifizierten Benutzers zu gewähren.
1. Die Xamarin.Forms-Anwendung verwendet das Ressourcentoken, um direkt auf Cosmos DB-Ressourcen mit den vom Ressourcentoken definierten Berechtigungen zuzugreifen.

> [!NOTE]
> Wenn das Ressourcentoken abläuft, erhalten nachfolgende Dokumentdatenbankanforderungen eine nicht autorisierte Ausnahme von 401. An diesem Punkt sollten Xamarin.Forms-Anwendungen die Identität neu einrichten und ein neues Ressourcentoken anfordern.

Weitere Informationen zur Cosmos DB-Partitionierung finden Sie [unter Partitionierung und Skalierung in Azure Cosmos DB](/azure/cosmos-db/partition-data/). Weitere Informationen zur Cosmos DB-Zugriffssteuerung finden Sie unter [Sichern des Zugriffs auf Cosmos DB-Daten](/azure/cosmos-db/secure-access-to-data/) und der [Zugriffssteuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Einrichten

Der Prozess für die Integration des Ressourcentokenbrokers in eine Xamarin.Forms-Anwendung ist wie folgt:

1. Erstellen Sie ein Cosmos DB-Konto, das die Zugriffssteuerung verwendet. Weitere Informationen finden Sie unter [Cosmos DB Configuration](#cosmosdb_configuration).
1. Erstellen Sie einen Azure App Service, um den Ressourcentokenbroker zu hosten. Weitere Informationen finden Sie unter [Azure App Service Configuration](#app_service_configuration).
1. Erstellen Sie eine Facebook-App, um die Authentifizierung durchzuführen. Weitere Informationen finden Sie unter [Facebook App Configuration](#facebook_configuration).
1. Konfigurieren Sie den Azure App Service so, dass er eine einfache Authentifizierung mit Facebook durchführt. Weitere Informationen finden Sie unter [Azure App Service Authentication Configuration](#app_service_authentication_configuration).
1. Konfigurieren Sie die Xamarin.Forms-Beispielanwendung für die Kommunikation mit Azure App Service und Cosmos DB. Weitere Informationen finden Sie unter [Xamarin.Forms Application Configuration](#forms_application_configuration).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)haben, erstellen Sie ein [kostenloses Konto,](https://aka.ms/azfree-docs-mobileapps) bevor Sie beginnen.

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB-Konfiguration

Der Vorgang zum Erstellen eines Cosmos DB-Kontos, das die Zugriffssteuerung verwendet, ist wie folgt:

1. Erstellen Sie ein Cosmos DB-Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Cosmos DB-Kontos](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Erstellen Sie im Cosmos DB-Konto `UserItems`eine neue Auflistung `/userid`mit dem Namen , die einen Partitionsschlüssel von angibt.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service-Konfiguration

Der Prozess zum Hosten des Ressourcentokenbrokers in Azure App Service ist wie folgt:

1. Erstellen Sie im Azure-Portal eine neue App Service-Web-App. Weitere Informationen finden Sie unter [Erstellen einer Web-App in einer App Service-Umgebung](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Öffnen Sie im Azure-Portal das Blatt App-Einstellungen für die Web-App, und fügen Sie die folgenden Einstellungen hinzu:
    - `accountUrl`– Der Wert sollte die Cosmos DB-Konto-URL vom Keys-Blatt des Cosmos DB-Kontos sein.
    - `accountKey`– Der Wert sollte der Cosmos DB-Masterschlüssel (primär oder sekundär) vom Blatt Keys des Cosmos DB-Kontos sein.
    - `databaseId`– Der Wert sollte der Name der Cosmos DB-Datenbank sein.
    - `collectionId`– Der Wert sollte der Name der Cosmos DB-Auflistung sein (in diesem Fall). `UserItems`
    - `hostUrl`– Der Wert sollte die URL der Web-App über das Blatt Übersicht des App Service-Kontos sein.

    Der folgende Screenshot veranschaulicht diese Konfiguration:

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. Veröffentlichen Sie die Ressourcentokenbrokerlösung in der Azure App Service-Web-App.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook-App-Konfiguration

Der Vorgang zum Erstellen einer Facebook-App zum Durchführen der Authentifizierung ist wie folgt:

1. Erstellen Sie eine Facebook-App. Weitere Informationen finden Sie unter [Registrieren und Konfigurieren einer App](https://developers.facebook.com/docs/apps/register) im Facebook Developer Center.
1. Fügen Sie das Facebook-Login-Produkt zur App hinzu. Weitere Informationen finden Sie unter Hinzufügen von [Facebook-Anmeldungzu deiner App oder Website](https://developers.facebook.com/docs/facebook-login) im Facebook Developer Center.
1. Konfigurieren Sie Facebook Login wie folgt:
   - Aktivieren Sie client oAuth Login.
   - Aktivieren Sie Die Web OAuth-Anmeldung.
   - Legen Sie den GÜLTIGEN OAuth-Umleitungs-URI auf `/.auth/login/facebook/callback` den URI der App Service-Web-App mit Anhängen fest.

  Der folgende Screenshot veranschaulicht diese Konfiguration:

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

Weitere Informationen finden Sie unter [Registrieren Ihrer Anwendung bei Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure App Service-Authentifizierungskonfiguration

Der Prozess zum Konfigurieren der einfachen Authentifizierung von App Service ist wie folgt:

1. Navigieren Sie im Azure-Portal zur App Service-Web-App.
1. Öffnen Sie im Azure-Portal das Blatt Authentifizierung/Autorisierung, und führen Sie die folgende Konfiguration aus:
    - Die App Service-Authentifizierung sollte aktiviert sein.
    - Die Aktion, die ausgeführt werden soll, wenn eine Anforderung nicht authentifiziert wird, sollte auf **Anmelden bei Facebook**festgelegt werden.

    Der folgende Screenshot veranschaulicht diese Konfiguration:

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

Die App Service-Web-App sollte auch für die Kommunikation mit der Facebook-App konfiguriert werden, um den Authentifizierungsfluss zu aktivieren. Dies kann erreicht werden, indem Sie den Facebook-Identitätsanbieter auswählen und die **App-ID-** und **App-Geheimwerte** aus den Einstellungen der Facebook-App im Facebook Developer Center eingeben. Weitere Informationen finden Sie unter Hinzufügen von [Facebook-Informationen zu Ihrer Anwendung](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms Anwendungskonfiguration

Der Prozess zum Konfigurieren der Xamarin.Forms-Beispielanwendung ist wie folgt:

1. Öffnen Sie die Xamarin.Forms-Lösung.
1. Öffnen `Constants.cs` und aktualisieren Sie die Werte der folgenden Konstanten:
    - `EndpointUri`– Der Wert sollte die Cosmos DB-Konto-URL vom Keys-Blatt des Cosmos DB-Kontos sein.
    - `DatabaseName`– Der Wert sollte der Name der Dokumentdatenbank sein.
    - `CollectionName`– Der Wert sollte der Name der Dokumentendatenbanksammlung `UserItems`sein (in diesem Fall).
    - `ResourceTokenBrokerUrl`– Der Wert sollte die URL der Ressourcentoken-Broker-Web-App über das Blatt Übersicht des App Service-Kontos sein.

## <a name="initiating-login"></a>Initiieren der Anmeldung

Die Beispielanwendung initiiert den Anmeldeprozess, indem sie einen Browser an eine Identitätsanbieter-URL umleitet, wie im folgenden Beispielcode gezeigt:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Dadurch wird ein OAuth-Authentifizierungsfluss zwischen Azure App Service und Facebook initiiert, der die Facebook-Anmeldeseite anzeigt:

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

Die Anmeldung kann durch Drücken der **Schaltfläche Abbrechen** unter iOS oder durch Drücken der **Zurück-Taste** auf Android abgebrochen werden, in diesem Fall bleibt der Benutzer nicht authentifiziert und die Benutzeroberfläche des Identitätsanbieters wird vom Bildschirm entfernt.

## <a name="obtaining-a-resource-token"></a>Abrufen eines Ressourcentokens

Nach erfolgreicher Authentifizierung wird das `WebRedirectAuthenticator.Completed` Ereignis ausgelöst. Das folgende Codebeispiel veranschaulicht die Behandlung dieses Ereignisses:

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

Das Ergebnis einer erfolgreichen Authentifizierung ist ein `AuthenticatorCompletedEventArgs.Account` Zugriffstoken, das die verfügbare Eigenschaft ist. Das Zugriffstoken wird extrahiert und in einer GET-Anforderung `resourcetoken` an die API des Ressourcentokenbrokers verwendet.

Die `resourcetoken` API verwendet das Zugriffstoken, um die Identität des Benutzers von Facebook anzufordern, das wiederum verwendet wird, um ein Ressourcentoken von Cosmos DB anzufordern. Wenn bereits ein gültiges Berechtigungsdokument für den Benutzer in der Dokumentdatenbank vorhanden ist, wird es abgerufen, und ein JSON-Dokument, das das Ressourcentoken enthält, wird an die Anwendung Xamarin.Forms zurückgegeben. Wenn kein gültiges Berechtigungsdokument für den Benutzer vorhanden ist, werden ein Benutzer und eine Berechtigung in der Dokumentdatenbank erstellt, und das Ressourcentoken wird aus dem Berechtigungsdokument extrahiert und in einem JSON-Dokument an die Anwendung Xamarin.Forms zurückgegeben.

> [!NOTE]
> Ein Dokumentdatenbankbenutzer ist eine Ressource, die einer Dokumentdatenbank zugeordnet ist, und jede Datenbank kann null oder mehr Benutzer enthalten. Eine Dokumentdatenbankberechtigung ist eine Ressource, die einem Dokumentdatenbankbenutzer zugeordnet ist, und jeder Benutzer kann null oder mehr Berechtigungen enthalten. Eine Berechtigungsressource bietet Zugriff auf ein Sicherheitstoken, das der Benutzer benötigt, wenn er versucht, auf eine Ressource wie ein Dokument zuzugreifen.

Wenn `resourcetoken` die API erfolgreich abgeschlossen wird, sendet sie den HTTP-Statuscode 200 (OK) in der Antwort zusammen mit einem JSON-Dokument, das das Ressourcentoken enthält. Die folgenden JSON-Daten zeigen eine typische erfolgreiche Antwortmeldung:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

Der `WebRedirectAuthenticator.Completed` Ereignishandler liest die `resourcetoken` Antwort aus der API und extrahiert das Ressourcentoken und die Benutzer-ID. Das Ressourcentoken wird dann als `DocumentClient` Argument an den Konstruktor übergeben, der den Endpunkt, die Anmeldeinformationen und die Verbindungsrichtlinie für den Zugriff auf Cosmos DB kapselt und zum Konfigurieren und Ausführen von Anforderungen für Cosmos DB verwendet wird. Das Ressourcentoken wird mit jeder Anforderung gesendet, um direkt auf eine Ressource zuzugreifen, und gibt an, dass Lese-/Schreibzugriff auf die partitionierte Auflistung der authentifizierten Benutzer gewährt wird.

## <a name="retrieving-documents"></a>Abrufen von Dokumenten

Das Abrufen von Dokumenten, die nur dem authentifizierten Benutzer gehören, kann durch Erstellen einer Dokumentabfrage erreicht werden, die die ID des Benutzers als Partitionsschlüssel enthält, und wird im folgenden Codebeispiel gezeigt:

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

Die Abfrage ruft alle Dokumente, die dem authentifizierten Benutzer gehören, asynchron aus der angegebenen Auflistung ab und platziert sie zur Anzeige in einer `List<TodoItem>` Auflistung.

Die `CreateDocumentQuery<T>` Methode gibt `Uri` ein Argument an, das die Auflistung darstellt, die für Dokumente abgefragt werden soll, und ein `FeedOptions` Objekt. Das `FeedOptions` Objekt gibt an, dass eine unbegrenzte Anzahl von Elementen von der Abfrage zurückgegeben werden kann, und die ID des Benutzers als Partitionsschlüssel. Dadurch wird sichergestellt, dass nur Dokumente in der partitionierten Auflistung des Benutzers im Ergebnis zurückgegeben werden.

> [!NOTE]
> Beachten Sie, dass Berechtigungsdokumente, die vom Ressourcentokenbroker erstellt werden, in derselben Dokumentsammlung gespeichert werden wie die von der Anwendung Xamarin.Forms erstellten Dokumente. Daher enthält die Dokumentabfrage eine `Where` Klausel, die ein Filterprädikat auf die Abfrage für die Dokumentsammlung anwendet. Diese Klausel stellt sicher, dass Berechtigungsdokumente nicht aus der Dokumentsammlung zurückgegeben werden.

Weitere Informationen zum Abrufen von Dokumenten aus einer Dokumentsammlung finden Sie unter [Abrufen von Dokumentsammlungsdokumenten](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query).

## <a name="inserting-documents"></a>Einfügen von Dokumenten

Bevor Sie ein Dokument in eine `TodoItem.UserId` Dokumentsammlung einfügen, sollte die Eigenschaft mit dem Wert aktualisiert werden, der als Partitionsschlüssel verwendet wird, wie im folgenden Codebeispiel gezeigt:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Dadurch wird sichergestellt, dass das Dokument in die partitionierte Auflistung des Benutzers eingefügt wird.

Weitere Informationen zum Einfügen eines Dokuments in eine Dokumentsammlung finden Sie unter [Einfügen eines Dokuments in eine Dokumentsammlung](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document).

## <a name="deleting-documents"></a>Löschen von Dokumenten

Der Partitionsschlüsselwert muss beim Löschen eines Dokuments aus einer partitionierten Auflistung angegeben werden, wie im folgenden Codebeispiel gezeigt:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Dadurch wird sichergestellt, dass Cosmos DB weiß, aus welcher partitionierten Auflistung das Dokument gelöscht werden soll.

Weitere Informationen zum Löschen eines Dokuments aus einer Dokumentsammlung finden Sie unter [Löschen eines Dokuments aus einer Dokumentsammlung](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Die Zugriffssteuerung mit partitionierten Auflistungen kombiniert wird, sodass ein Benutzer nur auf eigene Dokumentdatenbankdokumente in einer Xamarin.Forms-Anwendung zugreifen kann. Durch das Angeben der Identität des Benutzers als Partitionsschlüssel wird sichergestellt, dass eine partitionierte Auflistung nur Dokumente für diesen Benutzer speichern kann.

## <a name="related-links"></a>Verwandte Links

- [Todo Azure Cosmos DB Auth (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Verwenden einer Azure Cosmos DB-Dokumentdatenbank](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Sichern des Zugriffs auf Azure Cosmos DB-Daten](/azure/cosmos-db/secure-access-to-data/)
- [Zugriffssteuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Partitionieren und Skalieren in Azure Cosmos DB](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB-Clientbibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB-API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
