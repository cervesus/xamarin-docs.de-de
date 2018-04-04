---
title: Daten und Clouddienste
description: Xamarin.Forms-Anwendungen können Verarbeiten von Webdiensten implementiert, mithilfe einer Vielzahl von Technologien, und dieser Anleitung hierzu untersucht.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: f20042b9599f7b4dde699a125e63c5ce435f6bc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="data--cloud-services"></a>Daten und Clouddienste

_Xamarin.Forms-Anwendungen können Verarbeiten von Webdiensten implementiert, mithilfe einer Vielzahl von Technologien, und dieser Anleitung hierzu untersucht._

Eine Einführung zu Web plattformübergreifende Nutzung der Dienste auf der Xamarin-Plattform finden Sie unter [Einführung in Web Services](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Grundlegendes zum Beispiel](~/xamarin-forms/data-cloud/walkthrough.md)

Dieser Artikel bietet eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die für die Kommunikation mit anderen Webdiensten veranschaulicht. Zu den behandelten Themen gehören die Struktur der Anwendung, die Seiten, die Datenmodell, und Webdienstvorgänge aufrufen.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Verwenden von Webdiensten](~/xamarin-forms/data-cloud/consuming/index.md)

Dieses Handbuch veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen gehören, bei der Kommunikation mit [ASMX-Diensten](consuming/asmx.md), [WCF-Dienste](consuming/wcf.md), [REST-Dienste](consuming/rest.md), [Azure Mobile Apps](consuming/azure.md), und [ Amazon Web Services](consuming/aws.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Authentifizieren des Zugriffs auf Webdienste](~/xamarin-forms/data-cloud/authentication/index.md)

Dieses Handbuch wird erläutert, wie zum Integrieren von Authentifizierungsdienste in einer Xamarin.Forms-Anwendung von Benutzern ein Back-End-freigeben, während Sie den Zugriff auf ihre eigenen Daten nur eine aktiviert werden. Zu den behandelten Themen gehören [unter Verwendung der Standardauthentifizierung mit einem REST-Dienst](authentication/rest.md), [mithilfe der Komponente zur Xamarin.Auth zum Authentifizieren beim OAuth-Identitätsanbieter](authentication/oauth.md), und die integrierte Authentifizierung verwenden Mechanismen angebotenen [Azure Mobile Apps](authentication/azure.md), und [Amazon Web Services](authentication/aws.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Synchronisieren von Daten mit Webdiensten](sync/index.md)

In diesem Artikel wird erläutert, wie offline Sync-Funktionen zu einer Xamarin.Forms-Anwendung hinzugefügt. Offline Sync ermöglicht Benutzern, für die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Netzwerkverbindung vorhanden ist. Änderungen in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen durch den FTP-Dienst synchronisiert werden.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Senden von Pushbenachrichtigungen](push-notifications/index.md)

Dieser Artikel veranschaulicht, wie Pushbenachrichtigungen zu einer Xamarin.Forms-Anwendung hinzufügen. Azure Benachrichtigungshubs stellen eine skalierbare pushinfrastruktur für das Senden von Pushbenachrichtigungen mobile von beliebigen Back-Ends an beliebige mobile Plattformen die Komplexität von einem Back-End-müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme Geschäftslogikkomponenten bei.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Speichern von Dateien in der Cloud](storage/index.md)

In diesem Artikel wird veranschaulicht, wie Sie Xamarin.Forms verwenden, um Text und Binärdaten im Azure-Speicher zu speichern und zum Zugreifen auf die Daten. Azure-Speicher ist eine skalierbare Cloud-speicherlösung, die zum Speichern von unstrukturierte und strukturierte Daten verwendet werden kann.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Suchen von Daten in der Cloud](search/index.md)

Dieser Artikel veranschaulicht, wie der Microsoft Azure Search-Bibliothek zur Integration von Azure Search in einer Xamarin.Forms-Anwendung. Azure Search ist ein Cloud-Dienst, der Indizierung und Abfragefunktionen für hochgeladene Daten bereitstellt. Dies entfernt die Anforderungen an die Infrastruktur und die Suche Algorithmus Komplexitäten, die normalerweise bei der Implementierung von Suchfunktionen in einer Anwendung verknüpft sind.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Speichern von Daten in einer Dokumentdatenbank](cosmosdb/index.md)

Dieses Handbuch veranschaulicht, wie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure-Cosmos-DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren. Ein Azure-Cosmos-DB-dokumentdatenbank ist ein NoSQL-Datenbank, die mit geringer Latenz der Zugang zu JSON-Dokumente, bietet einen schnelle, hochverfügbare, skalierbare Datenbankdienst für Anwendungen, die eine nahtlose Skalierung und globale Replikation erforderlich sind.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Hinzufügen von Intelligenz mit Cognitive Services](cognitive-services/index.md)

Dieses Handbuch erläutert, wie einige der APIs für Cognitive Microsoft-Dienste in einer Xamarin.Forms-Anwendung verwendet wird. Cognitive Dienste sind eine Reihe von APIs, SDKs und Dienste, die für Entwickler ihre Anwendungen intelligenter machen, indem Sie zusätzliche Funktionen wie gesichtserkennung, Spracherkennung und Verständnis der Sprache verfügbar.
