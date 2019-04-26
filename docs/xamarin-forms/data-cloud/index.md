---
title: Daten und Clouddienste
description: Xamarin.Forms-Anwendungen können Webdienste implementiert, mithilfe einer Vielzahl von Technologien nutzen, und dieser Anleitung dazu untersucht.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327505"
---
# <a name="data--cloud-services"></a>Daten und Clouddienste

_Xamarin.Forms-Anwendungen können Webdienste implementiert, mithilfe einer Vielzahl von Technologien nutzen, und dieser Anleitung dazu untersucht._

Eine Einführung in plattformübergreifende webdienstnutzung auf der Xamarin-Plattform, finden Sie unter [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Grundlegendes zum Beispiel](~/xamarin-forms/data-cloud/walkthrough.md)

Dieser Artikel enthält eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die zeigt, wie Sie mit anderen Webdiensten kommunizieren. Zu den behandelten Themen gehören die Struktur der Anwendung, die Seiten, die ein Datenmodell, und das Aufrufen von Webdienstvorgängen.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Verwenden von Webdiensten](~/xamarin-forms/data-cloud/consuming/index.md)

Diese Anleitung veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Behandelten Themen gehören die Kommunikation mit [ASMX-Dienste](consuming/asmx.md), [WCF-Dienste](consuming/wcf.md), [REST-Diensten](consuming/rest.md), und [Azure Mobile Apps](consuming/azure.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Authentifizieren des Zugriffs auf Webdienste](~/xamarin-forms/data-cloud/authentication/index.md)

Dieses Handbuch wird erläutert, wie Authentifizierungsdienste in einer Xamarin.Forms-Anwendung, damit Benutzer auf eine Back-End zu verwenden, während Sie nur den Zugriff auf ihre eigenen Daten können zu integrieren. Zu den behandelten Themen gehören [unter Verwendung der Standardauthentifizierung mit einem REST-Dienst](authentication/rest.md), [mit, dass die Xamarin.Auth-Komponente OAuth-Identitätsanbieter authentifizieren](authentication/oauth.md), und verwenden Sie die integrierte Authentifizierung Mechanismen von angebotenen [Azure Mobile Apps](authentication/azure.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Synchronisieren von Daten mit Webdiensten](sync/index.md)

In diesem Artikel wird erläutert, Hinzufügen von offlinesynchronisierung zu einer Xamarin.Forms-Anwendung. Die offlinesynchronisierung ermöglicht Benutzern die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn es keine Netzwerkverbindung. Änderungen werden in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen mit dem Webdienst synchronisiert werden.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Senden von Pushbenachrichtigungen](push-notifications/index.md)

Dieser Artikel beschreibt das Hinzufügen von Pushbenachrichtigungen zu einer Xamarin.Forms-Anwendung. Azure Notification Hubs bietet eine skalierbare pushinfrastruktur für mobile Pushbenachrichtigungen von jedem Back-End an beliebige mobile Plattformen, sodass Sie die Komplexität eines Back-Ends müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme senden.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Speichern von Dateien in der Cloud](storage/index.md)

In diesem Artikel wird veranschaulicht, wie Xamarin.Forms zum Speichern von Text- und Binärdaten im Azure-Speicher zu verwenden, und die Daten zugreifen. Azure Storage ist ein skalierbarer Cloud-speicherlösung, die zum Speichern von unstrukturierten und strukturierten Daten verwendet werden kann.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Suchen von Daten in der Cloud](search/index.md)

In diesem Artikel veranschaulicht, wie die Bibliothek für Microsoft Azure Search, um Azure Search in einer Xamarin.Forms-Anwendung zu integrieren. Azure Search ist ein Clouddienst zur indizierungs- und Abfragefunktionen für hochgeladene Daten. Dies entfernt die Anforderungen an die Infrastruktur und die Suche Algorithmus Komplexitäten, die normalerweise bei der Implementierung von Suchfunktionen in einer Anwendung verknüpft ist.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Speichern von Daten in einer Dokumentdatenbank](cosmosdb/index.md)

Diese Anleitung veranschaulicht, wie Sie die Azure Cosmos DB .NET Standard-Clientbibliothek verwenden, um ein Azure Cosmos DB-dokumentdatenbank in einer Xamarin.Forms-Anwendung zu integrieren. Eine Azure Cosmos DB-dokumentdatenbank ist eine NoSQL-Datenbank, die Zugriff mit geringer Latenz für JSON-Dokumenten, bietet einen schnelle, hoch verfügbare, skalierbare Datenbankdienst für Anwendungen, die eine nahtlose Skalierung und globale Replikation erfordern bereitstellt.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Hinzufügen von Intelligenz mit Cognitive Services](cognitive-services/index.md)

Dieses Handbuch wird erläutert, wie einige der Microsoft Cognitive Services-APIs in einer Xamarin.Forms-Anwendung verwenden. Cognitive Services umfasst einen Satz von APIs, SDKs und Diensten für Entwickler, um ihre Anwendungen intelligenter zu machen, durch das Hinzufügen von Features wie gesichtserkennung, Spracherkennung und sprachverständnis.
