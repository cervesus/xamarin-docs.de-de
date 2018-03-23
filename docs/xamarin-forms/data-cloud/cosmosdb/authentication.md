---
title: Authentifizieren von Benutzern mit einer Azure-Cosmos-DB-Dokument-Datenbank
description: Azure DB Cosmos-Dokument-Datenbanken unterstützen partitionierte Sammlungen enthalten, die mehrere Server und Partitionen, und unterstützt eine unbegrenzte Speicherdauer und Durchsatz erstrecken können. In diesem Artikel wird erläutert, wie Zugriffssteuerung mit partitionierte Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann.
ms.topic: article
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 8de64d6489b4022e43bcf694f3b13d6f7eaaecbd
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Authentifizieren von Benutzern mit einer Azure-Cosmos-DB-Dokument-Datenbank

_Azure DB Cosmos-Dokument-Datenbanken unterstützen partitionierte Sammlungen enthalten, die mehrere Server und Partitionen, und unterstützt eine unbegrenzte Speicherdauer und Durchsatz erstrecken können. In diesem Artikel wird erläutert, wie Zugriffssteuerung mit partitionierte Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann._

## <a name="overview"></a>Übersicht

Partitionsschlüssel muss angegeben werden, wenn eine partitionierte Sammlung zu erstellen, und die Dokumente mit den gleichen Partitionsschlüssel werden in der gleichen Partition gespeichert werden. Aus diesem Grund führt angeben der Identität des Benutzers als Partitionsschlüssel in eine partitionierte Sammlung, die nur für diesen Benutzer Dokumente gespeichert werden. Außerdem sichergestellt werden, dass die Azure-Cosmos-DB-dokumentdatenbank, als die Anzahl der Benutzer skaliert wird und Elemente erhöhen.

Jede Auflistung muss Zugriff gewährt werden, und das SQL-API-Zugriffssteuerungsmodell definiert zwei Typen von Access-Konstrukte:

- **Hauptschlüssel** vollen administrativen Zugriff auf alle Ressourcen in einem Cosmos-DB-Konto zu aktivieren, und erstellt, wenn ein Cosmos-DB-Konto erstellt wird.
- **Ressourcentoken** die Beziehung zwischen dem Benutzer einer Datenbank und die Berechtigung, die der Benutzer für eine bestimmte Cosmos-DB-Ressource, z. B. eine Auflistung oder ein Dokument verfügt zu erfassen.

Verfügbarmachen eines Hauptschlüssels öffnet ein Cosmos-DB-Konto, um die Möglichkeit, dass böswillige oder eine nachlässige verwenden. Azure-Cosmos-DB Ressourcentoken bieten jedoch einen sicheren Mechanismus für das Zulassen von Clients zu lesen, schreiben und Löschen bestimmte Ressourcen in einem Azure-Cosmos-DB-Konto entsprechend der erteilten Berechtigungen.

Ein typischer Ansatz angefordert wird, ist generieren und Übermitteln von Ressourcentoken einer mobilen Anwendung auf einen Ressource token Broker verwenden. Das folgende Diagramm zeigt einen allgemeinen Überblick darüber, wie die beispielanwendung einen Ressource-token-Broker, zum Verwalten des Zugriffs auf die Datenbank Dokumentdaten verwendet:

![](authentication-images/documentdb-authentication.png "Datenbank-Authentifizierungsprozess Dokument")

Die Ressource token Broker ist ein Mid-Tier-Web-API, gehosteter Dienst in Azure App Service, der den Hauptschlüssel der Cosmos-DB-Konto besitzt. Die beispielanwendung werden mit dem Ressourcen-token-Broker Verwalten des Zugriffs auf die Datenbankdaten wie folgt:

1. Bei der Anmeldung kontaktiert die Anwendung Xamarin.Forms Azure App Service um eine Authentifizierungsablauf zu initiieren.
1. Azure App Service führt einen OAuth-authentifizierungsdatenfluss mit Facebook. Nach Abschluss der Authentifizierungsablauf empfängt die Xamarin.Forms-Anwendung ein Zugriffstoken an.
1. Die Xamarin.Forms-Anwendung verwendet das Zugriffstoken, um ein Ressourcentoken von token Broker Ressource anzufordern.
1. Die Ressource token Broker verwendet das Zugriffstoken, um die Identität des Benutzers von Facebook anzufordern. Die Identität des Benutzers wird dann einen Ressourcentoken von Cosmos-DB anfordern, die verwendet wird, gewähren Lese-/Schreibzugriff für partitionierte Sammlung des authentifizierten Benutzers verwendet.
1. Die Xamarin.Forms-Anwendung verwendet das Ressourcentoken direkt Cosmos-DB-Zugriff auf Ressourcen mit den Berechtigungen, die durch das Ressourcentoken definiert.

> [!NOTE]
> Wenn das Ressourcentoken abläuft, erhalten nachfolgende-dokumentanforderungen für die Datenbank eine Ausnahme für 401 nicht autorisiert. An diesem Punkt sollte Xamarin.Forms Anwendungen stellen Sie die Identität wieder her, und Sie ein neues Ressourcentoken anfordern.

Weitere Informationen zum Partitionieren von Cosmos-DB finden Sie unter [partitionieren und Skalierung in Azure-Cosmos-DB](/azure/cosmos-db/partition-data/). Weitere Informationen zur Cosmos-DB-Zugriffssteuerung finden Sie unter [Sichern des Zugriffs auf Daten Cosmos-DB](/azure/cosmos-db/secure-access-to-data/) und [Zugriffssteuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Setup

Der Prozess für die Integration von der Ressource token Broker in einer Xamarin.Forms-Anwendung lautet wie folgt:

1. Erstellen Sie ein Cosmos-DB-Konto, die Zugriffssteuerung verwendet wird. Weitere Informationen finden Sie unter [Cosmos-DB-Konfiguration](#cosmosdb_configuration).
1. Erstellen Sie ein Azure App Service zum Hosten des Resource-token-Brokers. Weitere Informationen finden Sie unter [Azure App Service-Konfiguration](#app_service_configuration).
1. Erstellen einer Facebook-app für die Authentifizierung an. Weitere Informationen finden Sie unter [Facebook-App Configuration](#facebook_configuration).
1. Konfigurieren Sie die Azure App Service für die einfache Authentifizierung mit Facebook. Weitere Informationen finden Sie unter [Azure App Service-Authentifizierungskonfiguration](#app_service_authentication_configuration).
1. Konfigurieren Sie das Xamarin.Forms-beispielanwendung für die Kommunikation mit Azure App Service und Cosmos-DB. Weitere Informationen finden Sie unter [Xamarin.Forms Anwendungskonfiguration](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure-Cosmos-DB-Konfiguration

Der Prozess zum Erstellen einer Cosmos-DB-Kontos, der Zugriffssteuerung verwendet wird, lautet wie folgt:

1. Erstellen Sie ein Konto Cosmos-DB. Weitere Informationen finden Sie unter [erstellen Sie ein Azure-Cosmos-DB-Konto](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Erstellen Sie eine neue Sammlung mit dem Namen im Konto Cosmos-DB `UserItems`, Partitionsschlüssel angeben `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service-Konfiguration

Der Prozess zum Hosten von in Azure App Service Broker token Ressource lautet wie folgt:

1. Erstellen Sie eine neue App Service-Web-app in Azure-Portal. Weitere Informationen finden Sie unter [erstellen Sie eine Web-app im App Service-Umgebung](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Klicken Sie im Azure-Portal öffnen Sie Blatts "App-Einstellungen", für die Web-app, und fügen Sie die folgenden Einstellungen:
    - `accountUrl` – der Wert sollte die Cosmos-DB-Konto-URL aus dem Blatt "Schlüssel" des Kontos Cosmos-DB sein.
    - `accountKey` – der Wert der Cosmos-DB-Hauptschlüssel (Primär oder sekundär) aus dem Blatt "Schlüssel" des Kontos Cosmos-DB werden sollte.
    - `databaseId` – der Wert sollte den Namen der Cosmos-DB-Datenbank sein.
    - `collectionId` – der Wert muss der Name der Cosmos-DB-Auflistung (in diesem Fall `UserItems`).
    - `hostUrl` – der Wert sollte die URL der Web-app aus dem Blatt "Übersicht" des App-Dienstkontos sein.

    Der folgende Screenshot zeigt diese Konfiguration:

    [![](authentication-images/azure-web-app-settings.png "App Service-Web-App-Einstellungen")](authentication-images/azure-web-app-settings-large.png#lightbox "App Service-Web-App-Einstellungen")

1. Veröffentlichen Sie die Ressource token Broker-Lösung in Azure App Service-Web-app.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook-App-Konfiguration

Der Prozess zum Erstellen einer Facebook-app für die Authentifizierung ist wie folgt:

1. Erstellen einer Facebook-app. Weitere Informationen finden Sie unter [registrieren und konfigurieren Sie eine App](https://developers.facebook.com/docs/apps/register) im Facebook-Developer Center.
1. Fügen Sie das Produkt Facebook-Anmeldung zur app hinzu. Weitere Informationen finden Sie unter [Facebook-Anmeldung hinzufügen, um Ihre App oder Website](https://developers.facebook.com/docs/facebook-login) im Facebook-Developer Center.
1. Konfigurieren Sie Facebook-Anmeldung wie folgt:
  - Aktivieren Sie die Client-OAuth-Anmeldung.
  - Web-OAuth-Anmeldung zu aktivieren.
  - Legen Sie den gültigen OAuth-umleitungs-URI, an den URI der App Service-Web-app mit `/.auth/login/facebook/callback` angefügt.

  Der folgende Screenshot zeigt diese Konfiguration:

  ![](authentication-images/facebook-oauth-settings.png "Facebook-Anmeldung OAuth-Einstellungen")

Weitere Informationen finden Sie unter [registrieren Sie Ihre Anwendung mit Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure App Service-Authentifizierungskonfiguration

Der Prozess zum Konfigurieren der App Service Einfache Authentifizierung lautet wie folgt:

1. Navigieren Sie zu der App Service-Web-app, im Azure-Portal.
1. Öffnen Sie im Azure-Portal die Authentifizierung / Autorisierung Blatt, und führen Sie die folgende Konfiguration:
  - App Service-Authentifizierung sollte aktiviert sein.
  - Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **Anmeldung mit Facebook**.

  Der folgende Screenshot zeigt diese Konfiguration:

  [![](authentication-images/app-service-authentication-settings.png "App Service Web-App-Authentifizierungseinstellungen")](authentication-images/app-service-authentication-settings-large.png#lightbox "App Service Web-App-Authentifizierungseinstellungen")

Die App Service-Web-app sollte auch für die Kommunikation mit Facebook-app So aktivieren Sie den Authentifizierungsablauf konfiguriert werden. Dies kann durch Auswählen des Facebook-Identitätsanbieters und Eingeben der **App-ID** und **App-Geheimnis** Werte aus den Facebook-app-Einstellungen im Facebook-Developer Center. Weitere Informationen finden Sie unter [Hinzufügen von Facebook-Informationen für Ihre Anwendung](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms-Anwendungskonfiguration

Der Prozess zur Konfiguration der beispielanwendung Xamarin.Forms lautet wie folgt:

1. Öffnen Sie das Xamarin.Forms-Projektmappe.
1. Open `Constants.cs` und aktualisieren Sie die Werte der folgenden Konstanten:
  - `EndpointUri` – der Wert sollte die Cosmos-DB-Konto-URL aus dem Blatt "Schlüssel" des Kontos Cosmos-DB sein.
  - `DatabaseName` – der Wert sollte den Namen der dokumentdatenbank sein.
  - `CollectionName` – der Wert sollte den Namen der Sammlung der Dokument-Datenbank sein (in diesem Fall `UserItems`).
  - `ResourceTokenBrokerUrl` – der Wert sollte die URL der Ressource token Broker Web-app aus dem Blatt "Übersicht" des App-Dienstkontos sein.

## <a name="initiating-login"></a>Initiieren die Anmeldung

Die beispielanwendung initiiert den Anmeldevorgang mithilfe von Xamarin.Auth zum Umleiten eines Browsers zu einer URL des Identitätsanbieters, wie im folgenden Beispielcode gezeigt:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Dies bewirkt, dass einen OAuth-authentifizierungsdatenfluss zwischen Azure App Service und Facebook, das die Facebook-Anmeldeseite anzeigt eingeleitet werden:

![](authentication-images/login.png "Facebook-Anmeldung")

Die Anmeldung kann abgebrochen werden, durch Drücken der **"Abbrechen"** Schaltfläche auf iOS oder durch Drücken der **wieder** Schaltfläche auf Android-Geräten wird in diesem Fall der Benutzer nicht authentifizierte bleibt und die Benutzeroberfläche für Identität entfernt auf dem Bildschirm.

Weitere Informationen zu Xamarin.Auth, finden Sie unter [Authentifizieren von Benutzern mit einem Identitätsanbieter](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Einen Ressourcentoken abrufen

Nach erfolgreicher Authentifizierung hat der `WebRedirectAuthenticator.Completed` Ereignis ausgelöst wird. Das folgende Codebeispiel veranschaulicht die Behandlung dieses Ereignisses:

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

Das Ergebnis einer erfolgreichen Authentifizierung ist ein Zugriffstoken, die verfügbar ist `AuthenticatorCompletedEventArgs.Account` Eigenschaft. Das Zugriffstoken wird extrahiert und in eine GET-Anforderung an des Ressource token Brokers verwendet `resourcetoken` API.

Die `resourcetoken` -API verwendet das Zugriffstoken, um die Identität des Benutzers von Facebook, anzufordern, die wiederum verwendet wird, um ein Ressourcentoken von Cosmos-DB anzufordern. Wenn ein Dokument gültige Berechtigung für den Benutzer in der dokumentdatenbank bereits vorhanden ist, sie abgerufen wird, und ein JSON-Dokument mit dem Ressourcentoken wird an die Xamarin.Forms-Anwendung zurückgegeben. Wenn ein Dokument gültige Berechtigung für den Benutzer nicht vorhanden ist, einen Benutzer und Berechtigungen in der dokumentdatenbank erstellt wird und das Ressourcentoken aus dem Dokument Berechtigung extrahiert und an die Xamarin.Forms-Anwendung in einem JSON-Dokument zurückgegeben wird.

> [!NOTE]
> Ein Datenbankbenutzer Dokument ist eine Ressource, die eine dokumentdatenbank zugeordnet, und jede Datenbank kann NULL oder mehr Benutzer enthalten. Eine Dokument-Database-Berechtigung ist eine Ressource, die einen Datenbankbenutzer Dokument zugeordnet, und jeder Benutzer kann NULL oder mehr Berechtigungen enthalten. Eine Berechtigungsressource ermöglicht den Zugriff auf ein Sicherheitstoken, die der Benutzer benötigt, bei dem Versuch, eine Ressource, z. B. ein Dokument zuzugreifen.

Wenn die `resourcetoken` API erfolgreich abgeschlossen wurde, sendet er HTTP-Statuscode 200 (OK) in der Antwort zusammen mit einem JSON-Dokument, das Ressourcentoken enthält. Die folgenden JSON-Daten enthalten eine typische erfolgreiche Antwortnachricht:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

Die `WebRedirectAuthenticator.Completed` Ereignishandler liest die Antwort aus dem `resourcetoken` API und extrahiert das Ressourcentoken und die Benutzer-Id. Das Ressourcentoken wird dann als Argument übergeben der `DocumentClient` -Konstruktor, der kapselt den Endpunkt, Anmeldeinformationen und Verbindungsrichtlinie Cosmos-DB den Zugriff auf verwendet und dient zum Konfigurieren und Ausführen von Anforderungen für Cosmos-DB. Das Ressourcentoken wird mit jeder Anforderung direkt Zugriff auf eine Ressource gesendet, und gibt an, dass Schreib-Lese-Zugriff auf den authentifizierten Benutzern partitionierte Sammlung gewährt wird.

## <a name="retrieving-documents"></a>Abrufen von Dokumenten

Abrufen von Dokumenten, die nur für den authentifizierten Benutzer gehören kann erreicht werden, erstellen Sie eine Dokumentabfrage, die die Id des Benutzers als Partitionsschlüssel enthält, und wird veranschaulicht, im folgenden Codebeispiel wird:

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

Die Abfrage asynchron werden die Dokumente, die dem authentifizierten Benutzer aus der angegebenen Auflistung gehören alle abgerufen und platziert sie in einem `List<TodoItem>` Auflistung für die Anzeige.

Die `CreateDocumentQuery<T>` Methode gibt ein `Uri` Argument, das die Auflistung darstellt, die für Dokumente abgefragt werden soll und ein `FeedOptions` Objekt. Die `FeedOptions` Objekt gibt an, dass eine unbegrenzte Anzahl von Elementen, die von der Abfrage und die Id des Benutzers als Partitionsschlüssel zurückgegeben werden kann. Dadurch wird sichergestellt, dass nur Dokumente in partitionierte Sammlung des Benutzers im Resultset zurückgegeben werden.

> [!NOTE]
> Beachten Sie, dass die Berechtigung Dokumente, die vom Broker token Ressource erstellt werden, in der gleichen Dokument Auflistung als der von der Anwendung Xamarin.Forms erstellten Dokumente gespeichert werden. Aus diesem Grund die Dokumentabfrage enthält eine `Where` -Klausel, die ein Filterprädikat auf die Abfrage für die Auflistung Dokument gilt. Diese Klausel wird sichergestellt, dass die Berechtigung Dokumente aus der Auflistung Dokument zurückgegeben werden nicht.

Weitere Informationen zum Abrufen von Dokumenten aus einer Auflistung Dokument finden Sie unter [abrufen Dokument Auflistung Dokumente](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query).

## <a name="inserting-documents"></a>Einfügen von Dokumenten

Vor dem Einfügen eines Dokuments in einer Auflistung Dokument die `TodoItem.UserId` Eigenschaft sollte mit dem Wert, der verwendet wird, als Partitionsschlüssel, aktualisiert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Dadurch wird sichergestellt, dass das Dokument in die partitionierte Sammlung des Benutzers eingefügt wird.

Weitere Informationen zum Einfügen von einem Dokument in eine Dokument-Auflistung finden Sie unter [Einfügen eines Dokuments in einer Auflistung Dokument](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document).

## <a name="deleting-documents"></a>Löschen von Dokumenten

Der Wert für den Partitionsschlüssel muss angegeben werden, wenn durch das Löschen eines Dokuments aus einer partitionierten Auflistung als in das folgende Codebeispiel veranschaulicht:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Dadurch sichergestellt, dass Cosmos-DB kennt die partitioniert Auflistung zum Löschen des Dokuments aus.

Weitere Informationen zum Löschen eines Dokuments aus einer Auflistung Dokument finden Sie unter [löschen ein Dokument aus einer Auflistung Dokument](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie Zugriffssteuerung mit partitionierte Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Datenbank-Dokument-Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann. Angeben der Identität des Benutzers als Partitionsschlüssel wird sichergestellt, dass eine partitionierte Sammlung nur für diesen Benutzer Dokumente speichern kann.


## <a name="related-links"></a>Verwandte Links

- [TODO-Azure-Cosmos-DB-Auth (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Verwenden einer Azure Cosmos DB-Dokumentdatenbank](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Sichern des Zugriffs auf Azure-Cosmos-DB-Daten](/azure/cosmos-db/secure-access-to-data/)
- [Zugriffssteuerung in der SQL-API](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Partitionieren und Skalierung in Azure-Cosmos-DB](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
