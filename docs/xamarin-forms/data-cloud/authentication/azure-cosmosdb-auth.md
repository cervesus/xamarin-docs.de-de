---
title: Authentifizieren von Benutzern mit einem Azure Cosmos DB-Dokumentdatenbank und Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Zugriffssteuerung mit Azure Cosmos DB partitionierte Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e55b3b9b0f204992de684ba09f3d9ff2552ce00
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832363"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Authentifizieren von Benutzern mit einem Azure Cosmos DB-Dokumentdatenbank und Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)

_Azure Cosmos DB-Dokumentdatenbanken unterstützen partitionierte Sammlungen enthalten, die mehrere Server und Partitionen unterstützt unbegrenzten Speicher und Durchsatz umfassen können. In diesem Artikel wird erläutert, wie Zugriffssteuerung mit partitionierten Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann._

## <a name="overview"></a>Übersicht

Ein Partitionsschlüssel angegeben werden, wenn Sie eine partitionierte Sammlung erstellen und Dokumente mit demselben Partitionsschlüssel werden in der gleichen Partition gespeichert. Aus diesem Grund führt angeben der Identität des Benutzers als Partitionsschlüssel in einer partitionierten Sammlung, die nur die Dokumente für diesen Benutzer gespeichert werden. Dadurch wird auch sichergestellt, dass die Azure Cosmos DB-dokumentdatenbank, als die Anzahl von Benutzern skaliert wird und Elementen erhöhen.

Zugriff erteilt werden, damit eine beliebige Sammlung, und das SQL-API-Zugriffssteuerungsmodell definiert zwei Typen von zugriffskonstrukten:

- **Hauptschlüssel** vollen administrativen Zugriff auf alle Ressourcen in einem Cosmos DB-Konto aktivieren und werden erstellt, wenn ein Cosmos DB-Konto erstellt wird.
- **Ressourcentoken** erfasst die Beziehung zwischen dem Benutzer eine Datenbank und die Berechtigung, die der Benutzer für eine bestimmte Cosmos DB-Ressource, z. B. eine Auflistung oder ein Dokument verfügt.

Cosmos DB-Konto besteht die Gefahr von böswilliger oder fahrlässiger Nutzung wird geöffnet, wenn Sie einen Hauptschlüssel verfügbar zu machen. Azure Cosmos DB-Ressourcentoken bieten jedoch einen sicheren Mechanismus für das Zulassen von Clients das Lesen, schreiben und Löschen bestimmter Ressourcen in einem Azure Cosmos DB-Konto gemäß den gewährten Berechtigungen.

Ein typischer Ansatz zum anfordern, ist die generieren und Bereitstellen von Ressourcentoken an einer mobilen Anwendung zur Verwendung von Resource token Broker. Das folgende Diagramm zeigt einen allgemeinen Überblick darüber, wie die beispielanwendung eine ressourcentokenbroker zum Verwalten des Zugriffs auf die Dokumentendaten für die Datenbank verwendet:

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Prozess der Dokument-Datenbank-Authentifizierung")

Die ressourcentokenbroker ist ein Mid-Tier-Web-API, gehosteter Dienst in Azure App Service, der den Hauptschlüssel des Cosmos DB-Konto besitzt. Die beispielanwendung verwendet die ressourcentokenbroker zum Verwalten des Zugriffs auf die Dokumentendaten für die Datenbank wie folgt:

1. Bei der Anmeldung kontaktiert die Xamarin.Forms-Anwendung für Azure App Service zum Initiieren einer Authentifizierungsablauf.
1. Azure App Service führt einen OAuth-authentifizierungsdatenfluss mit Facebook. Nach Ablauf der Authentifizierung abgeschlossen ist, empfängt die Xamarin.Forms-Anwendung ein Zugriffstoken an.
1. Die Xamarin.Forms-Anwendung verwendet das Zugriffstoken ein, das von der ressourcentokenbroker anfordern.
1. Die ressourcentokenbroker verwendet das Zugriffstoken, um die Identität des Benutzers von Facebook angefordert. Die Identität des Benutzers wird einen Ressourcentoken von Cosmos DB, anfordern, die verwendet wird, gewähren Lese-/Schreibzugriff zu partitionierten Sammlung des authentifizierten Benutzers verwendet.
1. Die Xamarin.Forms-Anwendung verwendet das Ressourcentoken, um direkten Zugriff auf Cosmos DB-Ressourcen mit den Berechtigungen, die durch das Ressourcentoken definiert.

> [!NOTE]
> Wenn das Ressourcentoken abläuft, erhalten nachfolgende-dokumentanforderungen für die Datenbank eine Ausnahme für 401 nicht autorisiert. An diesem Punkt sollte Xamarin.Forms-Anwendungen die Identität erneut herzustellen, und fordern ein neues Ressourcentoken.

Weitere Informationen zum Partitionieren von Cosmos DB finden Sie unter [partitionieren und skalieren in Azure Cosmos DB](/azure/cosmos-db/partition-data/). Weitere Informationen zur Zugriffssteuerung für Cosmos DB finden Sie unter [Sichern des Zugriffs auf Cosmos DB-Daten](/azure/cosmos-db/secure-access-to-data/) und [Zugriffssteuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Setup

Der Prozess für die Integration der ressourcentokenbroker in einer Xamarin.Forms-Anwendung lautet wie folgt aus:

1. Erstellen Sie eine Cosmos DB-Konto an, die Zugriffssteuerung verwendet werden. Weitere Informationen finden Sie unter [Cosmos DB-Konfiguration](#cosmosdb_configuration).
1. Erstellen Sie einen Azure App Service zum Hosten der ressourcentokenbroker. Weitere Informationen finden Sie unter [Azure App Service-Konfigurationseinstellungen](#app_service_configuration).
1. Erstellen einer Facebook-app für die Authentifizierung an. Weitere Informationen finden Sie unter [Facebook-App Configuration](#facebook_configuration).
1. Konfigurieren von Azure App Service für die einfache Authentifizierung mit Facebook. Weitere Informationen finden Sie unter [Konfiguration von Azure App Service-Authentifizierung](#app_service_authentication_configuration).
1. Konfigurieren Sie die Xamarin.Forms-beispielanwendung für die Kommunikation mit Azure App Service und Cosmos DB. Weitere Informationen finden Sie unter [Xamarin.Forms-Anwendungskonfiguration](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB-Konfiguration

Der Prozess zum Erstellen eines Cosmos DB-Kontos, das Zugriffssteuerung verwendet werden, lautet wie folgt:

1. Erstellen Sie ein Cosmos DB-Konto an. Weitere Informationen finden Sie unter [erstellen Sie ein Azure Cosmos DB-Konto](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. In Cosmos DB-Kontos, erstellen Sie eine neue Sammlung mit dem Namen `UserItems`, einen Partitionsschlüssel angeben `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service-Konfiguration

Der Prozess zum Hosten der ressourcentokenbroker in Azure App Service lautet wie folgt aus:

1. Erstellen Sie eine neue App Service-Web-app im Azure-Portal. Weitere Informationen finden Sie unter [erstellen Sie eine Web-app in App Service-Umgebung](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Klicken Sie im Azure-Portal öffnen Sie das Blatt "App-Einstellungen" für die Web-app, und fügen Sie die folgenden Einstellungen:
    - `accountUrl` – der Wert sollte die URL des Cosmos DB-Konto über das Blatt "Schlüssel" Cosmos DB-Konto sein.
    - `accountKey` – der Wert sollte der Cosmos DB-Hauptschlüssel (Primär oder sekundär) aus dem Blatt "Schlüssel" Cosmos DB-Konto sein.
    - `databaseId` – der Wert sollte der Name der Cosmos DB-Datenbank sein.
    - `collectionId` – der Wert muss der Name der Cosmos DB-Sammlung (in diesem Fall `UserItems`).
    - `hostUrl` – der Wert sollte die URL der Web-app aus dem Blatt "Übersicht" der App Service-Konto sein.

    Der folgende Screenshot veranschaulicht diese Konfiguration:

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service-Web-App-Einstellungen")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service-Web-App-Einstellungen")

1. Veröffentlichen Sie die Resource token Broker-Lösung in die Azure App Service-Web-app an.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook-App-Konfiguration

Der Prozess zum Erstellen einer Facebook-app für die Authentifizierung lautet wie folgt aus:

1. Erstellen einer Facebook-app. Weitere Informationen finden Sie unter [registrieren und Konfigurieren einer App](https://developers.facebook.com/docs/apps/register) im Facebook Developer Center.
1. Die app das Facebook-Anmeldung Produkt hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen von Facebook-Anmeldung für Ihre App oder Website](https://developers.facebook.com/docs/facebook-login) im Facebook Developer Center.
1. Konfigurieren Sie die Facebook-Anmeldung wie folgt:
   - Aktivieren Sie die Client-OAuth-Anmeldung.
   - Aktivieren Sie die Web-OAuth-Anmeldung.
   - Legen Sie die gültige OAuth-umleitungs-URI an den URI der App Service-Web-app mit `/.auth/login/facebook/callback` angefügt.

  Der folgende Screenshot veranschaulicht diese Konfiguration:

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook-Anmeldung OAuth-Einstellungen")

Weitere Informationen finden Sie unter [Registrieren Ihrer Anwendung für Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Konfiguration für Azure App Service-Authentifizierung

Der Prozess für die Konfiguration einfach App Service-Authentifizierung lautet wie folgt aus:

1. Navigieren Sie im Azure-Portal zu der App Service-Web-app.
1. Öffnen Sie im Azure-Portal, die Authentifizierung / Autorisierung auf dem Blatt, und führen Sie die folgende Konfiguration:
    - App Service-Authentifizierung sollte aktiviert sein.
    - Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Facebook**.

    Der folgende Screenshot veranschaulicht diese Konfiguration:

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service-Web-App-Authentifizierungseinstellungen")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service-Web-App-Authentifizierungseinstellungen")

Die App Service-Web-app sollte auch für die Kommunikation mit der Facebook-app So aktivieren Sie den Authentifizierungsablauf konfiguriert werden. Dies kann erreicht werden, von der Facebook-Identitätsanbieter auswählen und Eingeben der **App-ID** und **App-Geheimnis** Werte aus den Facebook-app-Einstellungen für das Facebook Developer Center. Weitere Informationen finden Sie unter [Hinzufügen von Facebook-Informationen für Ihre Anwendung](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Konfiguration der Xamarin.Forms-Anwendung

Der Prozess zum Konfigurieren der Xamarin.Forms-Beispiel-Anwendung lautet wie folgt aus:

1. Öffnen Sie die Xamarin.Forms-Projektmappe.
1. Open `Constants.cs` und aktualisieren Sie die Werte der folgenden Konstanten:
    - `EndpointUri` – der Wert sollte die URL des Cosmos DB-Konto über das Blatt "Schlüssel" Cosmos DB-Konto sein.
    - `DatabaseName` – der Wert sollte der Name der dokumentdatenbank sein.
    - `CollectionName` – der Wert sollte der Name der Datenbank-Dokumentsammlung sein (in diesem Fall `UserItems`).
    - `ResourceTokenBrokerUrl` – der Wert sollte die URL der Resource token Broker Web-app aus dem Blatt "Übersicht" der App Service-Konto sein.

## <a name="initiating-login"></a>Anmeldung wird initiiert.

Die beispielanwendung initiiert den Anmeldeprozess mithilfe von Xamarin.Auth zum Umleiten von eines Browsers auf eine URL des Identitätsanbieters, wie im folgenden Beispielcode gezeigt:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Dies bewirkt, dass ein OAuth-authentifizierungsflusses initiiert werden, zwischen Azure App Service und Facebook, das die Facebook-Anmeldeseite anzeigt:

![](azure-cosmosdb-auth-images/login.png "Facebook-Anmeldung")

Die Anmeldung abgebrochen werden kann, durch Drücken der **Abbrechen** auf iOS oder durch Drücken der Schaltfläche die **zurück** Schaltfläche unter Android wird in diesem Fall der Benutzer nicht authentifiziert bleibt und Identity Provider User Interface vom Bildschirm entfernt.

Weitere Informationen zu Xamarin.Auth, finden Sie unter [Authentifizieren von Benutzern mit einem Identitätsanbieter](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Abrufen eines Ressourcentokens

Nach erfolgreicher Authentifizierung kann der `WebRedirectAuthenticator.Completed` -Ereignis ausgelöst wird. Das folgende Codebeispiel veranschaulicht die Behandlung dieses Ereignisses:

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

Das Ergebnis einer erfolgreichen Authentifizierung ist ein Zugriffstoken, das verfügbar ist `AuthenticatorCompletedEventArgs.Account` Eigenschaft. Das Zugriffstoken extrahiert und in eine GET-Anforderung an des Resource token Brokers verwendet `resourcetoken` API.

Die `resourcetoken` -API verwendet das Zugriffstoken, die Identität des Benutzers aus Facebook, anfordern, die wiederum verwendet wird, um ein Ressourcentoken von Cosmos DB anzufordern. Wenn ein Dokument gültige Berechtigung für den Benutzer in der dokumentdatenbank bereits vorhanden ist, er wird abgerufen, und ein JSON-Dokument mit dem Ressourcentoken wird an die Xamarin.Forms-Anwendung zurückgegeben. Wenn ein Dokument gültige Berechtigung für den Benutzer nicht vorhanden ist, einen Benutzer und Berechtigungen in der dokumentdatenbank erstellt wird und das Ressourcentoken wird aus dem Dokument Berechtigungen extrahiert und an die Xamarin.Forms-Anwendung in einem JSON-Dokument zurückgegeben.

> [!NOTE]
> Ein Datenbankbenutzer Dokument ist eine Ressource, die eine dokumentdatenbank zugeordnet, und jede Datenbank kann NULL oder mehr Benutzer enthalten. Eine Dokument-Database-Berechtigung ist eine Ressource, die einem Datenbankbenutzer Dokument zugeordnet, und jeder Benutzer kann NULL oder mehr Berechtigungen enthalten. Eine Berechtigungsressource ermöglicht den Zugriff auf ein Sicherheitstoken, die der Benutzer benötigt, wenn Sie versuchen, Zugriff auf eine Ressource, z. B. ein Dokument.

Wenn die `resourcetoken` API erfolgreich abgeschlossen wurde, sendet er HTTP-Statuscode 200 (OK) in der Antwort zusammen mit einem JSON-Dokument mit dem Ressourcentoken. Die folgenden JSON-Daten zeigt eine typische erfolgreiche Antwortnachricht:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

Die `WebRedirectAuthenticator.Completed` Ereignishandler liest die Antwort auf die `resourcetoken` API und extrahiert das Ressourcentoken und die Benutzer-Id. Das Ressourcentoken wird dann als Argument für das Übergeben der `DocumentClient` -Konstruktor, der kapselt den Endpunkt, Anmeldeinformationen und verwendet, um den Zugriff auf Cosmos DB-Verbindungsrichtlinie und dient zum Konfigurieren und Ausführen von Anforderungen an Cosmos DB. Das Ressourcentoken wird mit jeder Anforderung direkt Zugriff auf eine Ressource gesendet, und gibt an, dass Lese-/Schreibzugriff in der authentifizierten Benutzer partitionierte Sammlung gewährt wird.

## <a name="retrieving-documents"></a>Abrufen von Dokumenten

Abrufen von Dokumenten, die nur für den authentifizierten Benutzer gehören kann erreicht werden, durch das Erstellen einer Dokumentabfrage, die ab, der die Benutzer Id als Partitionsschlüssel und wird im folgenden Codebeispiel wird:

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

Die Abfrage asynchron Ruft alle Dokumente, die für den authentifizierten Benutzer aus der angegebenen Auflistung gehören, und platziert sie in einem `List<TodoItem>` Auflistung für die Anzeige.

Die `CreateDocumentQuery<T>` Methode gibt eine `Uri` Argument, das der Auflistung darstellt, die für Dokumente abgefragt werden sollen und ein `FeedOptions` Objekt. Die `FeedOptions` Objekt gibt an, dass eine unbegrenzte Anzahl von Elementen, die von der Abfrage und die Benutzer Id als Partitionsschlüssel zurückgegeben werden kann. Dadurch wird sichergestellt, dass nur Dokumente in einer partitionierten Sammlung des Benutzers im Resultset zurückgegeben werden.

> [!NOTE]
> Beachten Sie, dass die Berechtigung Dokumente, die vom Resource token Broker erstellt werden, in der gleichen Dokumentsammlung als der von der Xamarin.Forms-Anwendung erstellten Dokumente gespeichert werden. Die Dokumentabfrage aus diesem Grund enthält ein `Where` -Klausel, die eine Filter-Prädikat für die Abfrage für die Dokumentsammlung gilt. Diese Klausel stellt sicher, dass es sich bei Berechtigung Dokumente aus der Auflistung Dokument zurückgegeben werden nicht.

Weitere Informationen zum Abrufen von Dokumenten in einer Dokumentsammlung finden Sie unter [Dokument Datenbanksammlungs-Dokumenten abrufen](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query).

## <a name="inserting-documents"></a>Einfügen von Dokumenten

Vor dem Einfügen eines Dokuments in einer Dokumentsammlung der `TodoItem.UserId` Eigenschaft sollte mit dem Wert, der verwendet wird, als Partitionsschlüssel, aktualisiert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Dadurch wird sichergestellt, dass in partitionierte Sammlung, die der Benutzer das Dokument eingefügt wird.

Weitere Informationen zu einem Dokument in einer Dokumentsammlung eingefügt, finden Sie unter [Einfügen von einem Dokument in einer Dokumentsammlung](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document).

## <a name="deleting-documents"></a>Das Löschen von Dokumenten

Wert des partitionsschlüssels muss angegeben werden, beim Löschen eines Dokuments aus einer partitionierten Sammlung, wie im folgenden Codebeispiel veranschaulicht:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Dadurch wird sichergestellt, dass Cosmos DB weiß die partitionierte Auflistung So löschen Sie das Dokument aus.

Weitere Informationen zum Löschen eines Dokuments in einer Dokumentsammlung finden Sie unter [Löschen eines Dokuments in einer Dokumentsammlung](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Zugriffssteuerung mit partitionierten Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Dokument-Datenbank-Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann. Angeben der Identität des Benutzers als Partitionsschlüssel wird sichergestellt, dass eine partitionierte Sammlung nur Dokumente für diesen Benutzer gespeichert werden kann.


## <a name="related-links"></a>Verwandte Links

- [TODO Azure Cosmos DB-Authentifizierung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Verwenden einer Azure Cosmos DB-Dokumentdatenbank](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Sichern des Zugriffs auf Azure Cosmos DB-Daten](/azure/cosmos-db/secure-access-to-data/)
- [Zugriffssteuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Partitionieren und skalieren in Azure Cosmos DB](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB-Clientbibliothek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB-API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
