---
title: Authentifizieren von Benutzern mit einer Amazon SimpleDB-Dienst
description: Amazon SimpleDB bietet keine eigenen ressourcenbasierte Berechtigungssystem. Authentifizierung bei einem Identitätsanbieter kann stattdessen verwendet werden, um sicherzustellen, dass Benutzer nur Zugriff auf ihre eigenen Daten in der Domäne SimpleDB haben. In diesem Artikel wird erläutert, wie Benutzer den Zugriff auf ihre eigenen Daten SimpleDB zu beschränken.
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 592e957e0c64e7189d6f01f1ba0f23da074c4bec
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/12/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Authentifizieren von Benutzern mit einer Amazon SimpleDB-Dienst

_Amazon SimpleDB bietet keine eigenen ressourcenbasierte Berechtigungssystem. Authentifizierung bei einem Identitätsanbieter kann stattdessen verwendet werden, um sicherzustellen, dass Benutzer nur Zugriff auf ihre eigenen Daten in der Domäne SimpleDB haben. In diesem Artikel wird erläutert, wie Benutzer den Zugriff auf ihre eigenen Daten SimpleDB zu beschränken._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) wird in der beispielanwendung verwendet, um die Benutzer-Authentifizierungsprozess verwalten, und lagern die Kontodetails für Benutzer auf dem Gerät. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit einem Identitätsanbieter](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>Ein authentifizierter Benutzer den Zugriff auf SimpleDB Domänendaten

Die beispielanwendung verwendet die `TodoItem` Klasse für die Modelldaten. Zum Speichern einer `TodoItem` Instanz in einem SimpleDB-Dienst zuerst in konvertiert werden muss ein `List` von `ReplaceableAttribute` Objekte. Weitere Informationen finden Sie unter [SimpleDB erstellen Objekte](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

Um sicherzustellen, dass Benutzer nur ihre eigenen Daten in der Domäne SimpleDB können zugreifen die `ToSimpleDBReplaceableAttributes` Methode speichert ein zusätzliches Attribut für eine `TodoItem` Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

Dieses Attribut stellt sicher, dass jedes Element in der Domäne SimpleDB gespeicherten eine zugeordnete e-Mail-Adresse für einen Benutzer dient zur eindeutigen Identifizierung den Benutzer, dem die Daten gehört. Wenn der Inhalt einer Domäne durch Aufrufen abgerufen werden die `AmazonSimpleDBClient.SelectAsync` -Methode der Abfrageausdruck wird sichergestellt, dass nur Elemente für den authentifizierten Benutzer abgerufen werden, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

Die `SelectAsync` Methode gibt eine Antwort mit einer Auflistung von Elementen und die zugehörigen Attribute, die mit den Abfrageausdruck übereinstimmen. Der Abfrageausdruck wird sichergestellt, dass nur Elemente, die e-Mail-Adresse des Benutzers entsprechen abgerufen werden sollen. Weitere Informationen zu Abfrageausdrücken, finden Sie unter [wählen Sie zum Erstellen von Amazon SimpleDB Abfragen](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) des Amazon-Website.

> [!NOTE]
> Achten Sie darauf, dass die zitieren Regeln befolgt, beim Erstellen des Abfrageausdrucks. Weitere Informationen finden Sie unter [zitieren Regeln auswählen](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) des Amazon-Website.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie Benutzer den Zugriff auf ihre eigenen SimpleDB Daten einschränken. Amazon SimpleDB bietet keine eigenen ressourcenbasierte Berechtigungssystem. Authentifizierung bei einem Identitätsanbieter kann stattdessen verwendet werden, um sicherzustellen, dass Benutzer nur ihre eigenen Daten in der Domäne SimpleDB können zugreifen.


## <a name="related-links"></a>Verwandte Links

- [TodoAWSAuth (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Verarbeiten einer Amazon SimpleDB-Diensts](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Authentifizieren von Benutzern mit einem Identitätsanbieter](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito Identität](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Developer Documentation](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK für .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
