---
title: Verwenden von Webdiensten
description: Dieses Handbuch veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen gehören kommuniziert mit ASMX-Diensten, WCF-Dienste, REST-Dienste und Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a4c842ea7fd37ade9be0a9cb3e3ff7e50a6d1491
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="consuming-web-services"></a>Verwenden von Webdiensten

_Dieser Handbuch veranschaulicht, wie für die Kommunikation mit anderen Webdienste bereitstellen erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen gehören kommuniziert mit ASMX-Diensten, WCF-Dienste, REST-Dienste und Azure Mobile Apps.

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Verarbeiten eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET-Webdienste (ASMX) bieten die Möglichkeit zum Erstellen von Webdiensten, die Nachrichten über HTTP senden (SOAP, Simple Object Access Protocol) verwenden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll zum Erstellen von und Zugreifen auf Webdienste. Consumer von ASMX-Dienst müssen nicht alles Plattform, das Objektmodell oder Programmiersprache ab, die zum Implementieren des Diensts kennen. Sie müssen verstehen, wie SOAP-Nachrichten senden und empfangen. In diesem Artikel veranschaulicht, wie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung nutzen.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Nutzen einen Windows Communication Foundation (WCF)-Webdienst](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Es ermöglicht Entwicklern, sichere, zuverlässige, transaktive und interoperable verteilte Anwendungen zu erstellen. Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF dieselben Funktionen unterstützt, die ASMX-SOAP-Nachrichten über HTTP. In diesem Artikel veranschaulicht, wie einen WCF-SOAP-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) ist eine Architektur zum Erstellen von Webdiensten. REST-Anforderungen werden über HTTP angefordert mithilfe der gleichen HTTP-Verben, die Webbrowser verwenden Sie zum Abrufen von Webseiten und Daten an Server senden. Dieser Artikel veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung nutzen.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Verwenden einer mobilen Anwendung für Azure](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps können Sie zum Entwickeln von apps mit skalierbare Back-Ends mit Unterstützung für mobile-Authentifizierung, offline-Synchronisierung und Push-Benachrichtigungen in Azure App Service, gehostet. Dieser Artikel, die nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden gilt, werden die Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Instanz von Azure-Mobile-Apps erläutert.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
