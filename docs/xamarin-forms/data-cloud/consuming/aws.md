---
title: Verarbeiten einer Amazon SimpleDB-Diensts
description: "Amazon SimpleDB ist ein Webdienst, der bietet die Möglichkeit zum Speichern und Abfragen von Daten in der Amazon-Cloud. Dieser Artikel beschreibt, wie der AWS-SDK für .NET verwenden, um Abfragen, erstellen, ersetzen und Löschen von Daten in einem Dienst SimpleDB."
ms.topic: article
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 590e39deb7972df9e45064bb1a96e533a1fc9856
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Verarbeiten einer Amazon SimpleDB-Diensts

_Amazon SimpleDB ist ein Webdienst, der bietet die Möglichkeit zum Speichern und Abfragen von Daten in der Amazon-Cloud. Dieser Artikel beschreibt, wie der AWS-SDK für .NET verwenden, um Abfragen, erstellen, ersetzen und Löschen von Daten in einem Dienst SimpleDB._

SimpleDB-Dienste verwenden eine Anforderung und Antwort Modell vertraut, um Consumern des REST-Dienste. Vorgänge werden für den Dienst SimpleDB aufgerufen, durch Senden einer Anforderung, die Daten enthalten kann. Nach der Verarbeitung der Anforderungs, gibt der SimpleDB-Dienst eine Antwort, die keine Ergebnisse enthält. Ein SimpleDB-Dienst muss programmgesteuert erstellt und kann nicht erstellt werden, über die [AWS-Konsole](https://aws.amazon.com). Allerdings muss ein AWS-Konto erstellen und den Zugriff auf alle Amazon-Webdienst.

In einem Dienst SimpleDB werden Daten in Domänen organisiert, in dem Daten platziert werden können, und Abfragen für die Daten ausführen. Domänen bestehen aus Elementen, die vom Attribut-Wert-Paaren beschrieben werden. Domänen können ähnlich wie Tabellen, mit Attributen wird ähnlich wie Spalten und Zeilen ähnlich wird Elemente betrachtet werden. Weitere Informationen zu den SimpleDB-Datenmodell, finden Sie unter [Datenmodell](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) des Amazon-Website.

Anweisungen zum Einrichten der erforderlichen Amazon-Dienste können in der Readme-Datei gefunden werden, die die beispielanwendung gehören. Wenn die beispielanwendung ausgeführt wird, verbunden mit einer Amazon Cognito Identität Pool zum Autorisieren des Zugriffs auf den Dienst SimpleDB wie im folgenden Screenshot gezeigt:

![](aws-images/portal.png "Beispielanwendung")

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consuming-a-simpledb-service"></a>Verarbeiten einer SimpleDB-Diensts

Amazon Cognito Identität ermöglicht AWS-Dienste, z. B. SimpleDB, von einer Anwendung ohne eine feste Programmierung AWS-Anmeldeinformationen in der Anwendung aufgerufen werden. Stattdessen wird ein eindeutige Identität Pool erstellt, der [Amazon Cognito Konsole](https://console.aws.amazon.com/cognito/home). Der Identity-Pool enthält Identitäten, die Rollen verwenden, um Ressourcen, z. B. SimpleDB, anzugeben, die die Identität zugreifen kann.

Die [AWS SDK für .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) bietet die `CognitoAWSCredentials` und `AmazonSimpleDBClient` Klassen, die von einer Xamarin.Forms-Anwendung verwendet werden auf den Dienst SimpleDB zuzugreifen wie im folgenden Codebeispiel gezeigt:

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

Eine neue Instanz der dem `CognitoAWSCredentials` Klasse wird erstellt, indem Sie die eindeutige Identität Pool-Id und die Region des Kontos Cognito Identität bereitstellen. Zum Zeitpunkt der Verfassung ist Cognito Identität nur in den Regionen USEast1 und EUWest1 verfügbar. Es kann jedoch mit Amazon Dienste außerhalb dieser Regionen kommunizieren.

Wenn der `AmazonSimpleDBClient` Instanz erstellt, die `CognitoAWSCredentials` Instanz muss angegeben werden, zusammen mit einer `AmazonSimpleDBConfig` -Instanz, die die geografischen Bereich angibt, in dem sich der Dienst SimpleDB befindet. Die `CognitoAWSCredentials` Instanz wird sichergestellt, dass der SimpleDB-Dienst, der Zugriff auf eine der AWS-Konto in der Identität Pool, erstellt wurde, und vermeidet die Notwendigkeit zum Einbetten einer AWS-Zugriffsschlüssel und den geheimen Schlüssel in der Anwendung zugeordnet.

Eine SimpleDB-Service-Domäne erstellt wird, durch Aufrufen der `AmazonSimpleDBClient.CreateDomainAsync` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

Die `CreateDomainAsync` Methode erfordert eine `CreateDomainRequest` Instanz als Parameter. Die `CreateDomainRequest` -Instanz initialisiert die `DomainName` Eigenschaft auf den Wert, der zur Kennzeichnung der Domäne verwendet werden. Um die Domäne zu erstellen, muss dieser Wert unter dem der AWS-Konto zugeordneten Domänen eindeutig sein. Hingegen die Domäne nicht erstellt werden und keine Fehlerantwort gesendet wird, um anzugeben. Alle Vorgänge mit dem Domänennamen erfolgt dann anhand der vorhandenen Domäne geben, statt einer neu erstellten Domäne.

### <a name="creating-simpledb-objects"></a>Erstellen von Objekten SimpleDB

Die beispielanwendung verwendet die `TodoItem` Klasse für die Modelldaten. Zum Speichern einer `TodoItem` Instanz in einem SimpleDB-Dienst zuerst in konvertiert werden muss ein `List` von `ReplaceableAttribute` Objekte. Dies wird erreicht, indem die `ToSimpleDBReplaceableAttributes` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

Diese Methode erstellt eine `List` der neue `ReplaceableAttribute` Instanzen, mit der `List` , die ein einzelnes darstellt `TodoItem` Instanz. Jede `ReplaceableAttribute` Instanz stellt eine einzelne Eigenschaft von der `TodoItem` Instanz. Weitere Informationen zu den `ReplaceableAttribute` Klasse, finden Sie unter [ReplaceableAttribute Klasse](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) des Amazon-Website.

Auf ähnliche Weise beim Abrufen von Daten aus dem Dienst SimpleDB er muss von konvertiert werden ein `List` von `Attribute` auf Instanzen einer `TodoItem` Instanz. Dies wird erreicht, mit der `FromSimpleDBAttributes` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

Diese Methode ruft jede einfach `Attribute` -Instanz von der `List` und wird in das neu erstellte `TodoItem` Instanz.

Weitere Informationen zu den `Attribute` Klasse, finden Sie unter [Attributklasse](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) des Amazon-Website.

### <a name="querying-data"></a>Abfrage von Daten

Der Inhalt einer Domäne können abgerufen werden, durch Aufrufen der `AmazonSimpleDBClient.SelectAsync` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

Die `SelectAsync` Methode akzeptiert eine `SelectRequest` Instanz als Parameter, der angibt, eine `Select` Abfrageausdruck in seinem `SelectExpression` Eigenschaft. Das Format des Abfrageausdrucks ist ähnlich wie das Format der SQL-standard `SELECT` Anweisung. Weitere Informationen zu den Abfrageausdruck, finden Sie unter [wählen Sie zum Erstellen von Amazon SimpleDB Abfragen](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) des Amazon-Website.

> [!NOTE]
> **Hinweis**: Achten Sie darauf, dass Sie die zitieren Regeln befolgt, beim Erstellen des Abfrageausdrucks. Weitere Informationen finden Sie unter [zitieren Regeln auswählen](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) des Amazon-Website.

Die `SelectAsync` Methode gibt eine Antwort mit einer Auflistung von Elementen und die zugehörigen Attribute, die mit den Abfrageausdruck übereinstimmen. Diese Auflistung wird dann zum konvertiert eine `List` von `TodoItem` Instanzen für die Anzeige.

### <a name="creating-and-replacing-data"></a>Erstellen und Austauschen von Daten

Die `AmazonSimpleDBClient.PutAttributesAsync` Methode dient zum Erstellen, und Ersetzen Sie die Daten in die Domäne SimpleDB, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

Die `PutAttributesAsync` Methode akzeptiert eine `PutAttributesRequest` Instanz als Parameter. Die `PutAttributesRequest` Instanz gibt die Attribut-Wert-Paaren, die als neues Element erstellt oder in ein vorhandenes Element ersetzt werden. Die `List` von `ReplaceableAttribute` Instanzen wird erstellt, indem die `ToSimpleDBReplaceableAttributes` Methode. Diese Methode setzt auch die `Replace` -Eigenschaft jedes `ReplaceableAttribute` auf `true`. Dadurch wird den neue Attributwert, um einem vorhandenen Attributwert zu ersetzen, wenn Daten ersetzt wird. Allerdings führt versucht, die Attributwerte ersetzen, die nicht vorhanden sind, keiner Fehlerantwort.

Der Wert, der die `PutAttributesRequest.ItemName` Eigenschaft steuert, ob Sie ein neues Element mit der Domäne hinzugefügt wird, oder gibt an, ob ein vorhandenes Element ersetzt werden. Wenn die Anwendung ein neues Element erstellt, legt der `TodoItem.ID` Eigenschaft, um ein neues `Guid`. Dadurch wird sichergestellt, dass jedes `TodoItem` Instanz besitzt einen eindeutigen Bezeichner. Aus diesem Grund werden, wenn die `PutAttributesRequest.ItemName` Eigenschaft auf einen Wert, der nicht vorhanden ist, in der Domäne festgelegt ist, der Dienst SimpleDB erstellt ein neues Element mit dem angegebenen Attribut-Wert-Paaren. Wenn die `PutAttributesRequest.ItemName` Eigenschaft auf einen Wert, der bereits in der Domäne festgelegt ist, der Dienst SimpleDB aktualisiert das Element mit dem angegebenen Attribut-Wert-Paaren.

### <a name="deleting-data"></a>Löschen von Daten

Die `AmazonSimpleDBClient.DeleteAttributesAsync` Methode dient zum Löschen von Daten aus der Domäne SimpleDB, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

Die `DeleteAttributesAsync` Methode akzeptiert eine `DeleteAttributesRequest` Instanz als Parameter.  Die `DeleteAttributesRequest` Instanz gibt die Attribute, die aus dem Element werden sollen, gelöscht mit der `List` von `Attribute` Instanzen gelöscht werden sollen, die von erstellt wird die `ToSimpleDBAttributes` Methode. Das Element wird gelöscht, vorausgesetzt, dass alle Attribute des Elements gelöscht werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie der AWS-SDK für .NET verwenden, um Abfragen, erstellen und zu ersetzen und Löschen von Daten in einem Dienst SimpleDB gespeichert wird. Dieses SDK bietet die `CognitoAWSCredentials` und `AmazonSimpleDBClient` Klassen, die von einer Xamarin.Forms-Anwendung für den Dienstzugriff SimpleDB verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [TodoAWS (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Amazon Web Services SDK-Xamarin-Entwicklerhandbuch](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Entwicklerdokumentation](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [AmazonSimpleDBClient Class](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [Amazon Web Services SDK für .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
