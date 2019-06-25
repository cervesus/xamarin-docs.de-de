---
title: Verwenden von Webdiensten
description: Diese Anleitung veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen umfassen die Kommunikation mit ASMX-Dienste, WCF-Dienste, REST-Dienste und Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 163b82590e95a1587836502883cd5f9f2afbe19c
ms.sourcegitcommit: 6e04246207aa743820029e8c217a43cfdd24f991
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2019
ms.locfileid: "67352113"
---
# <a name="consuming-web-services"></a>Verwenden von Webdiensten

_Diese Anleitung veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen umfassen die Kommunikation mit ASMX-Dienste, WCF-Dienste, REST-Dienste und Azure Mobile Apps._

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Verwenden eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET-Webdienste (ASMX) bieten die Möglichkeit, Webdienste zu erstellen, die Nachrichten über HTTP senden (SOAP, Simple Object Access Protocol) verwenden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten. In diesem Artikel wird veranschaulicht, wie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung genutzt wird.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Verwenden einen Windows Communication Foundation (WCF)-Webdienst](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen. Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen unterstützt, die ASMX bereitstellt – SOAP-Nachrichten über HTTP. In diesem Artikel wird veranschaulicht, wie ein WCF-SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung wird.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Verwenden eines RESTful-Web-Diensts](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) ist ein Architekturstil zum Erstellen von Webdiensten. REST-Anforderungen erfolgen über HTTP mit den gleichen HTTP-Verben, die Webbrowser zum Abrufen von Webseiten und zum Senden von Daten auf Servern verwenden. In diesem Artikel wird veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung genutzt wird.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Verwenden einer Azure-Mobile-App](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps können Sie zum Entwickeln von apps mit skalierbaren Back-Ends in Azure App Service gehostet wird, mit Unterstützung für mobile Authentifizierung, offlinesynchronisierung und Pushbenachrichtigungen. In diesem Artikel, der nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden, wird erläutert, wie zum Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Azure Mobile Apps-Instanz gespeichert werden.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
