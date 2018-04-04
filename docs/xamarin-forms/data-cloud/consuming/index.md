---
title: Verwenden von Webdiensten
description: Dieses Handbuch veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen gehören kommuniziert mit ASMX-Dienste, WCF Services REST-Dienste, Azure Mobile Apps und Amazon Web Services.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 530b57c009a1f76d3756d7315856f74b6cda2f66
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-web-services"></a>Verwenden von Webdiensten

_Dieses Handbuch veranschaulicht, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen gehören kommuniziert mit ASMX-Dienste, WCF Services REST-Dienste, Azure Mobile Apps und Amazon Web Services._

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Verarbeiten eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET-Webdienste (ASMX) bieten die Möglichkeit zum Erstellen von Webdiensten, die Nachrichten über HTTP senden (SOAP, Simple Object Access Protocol) verwenden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll zum Erstellen von und Zugreifen auf Webdienste. Consumer von ASMX-Dienst müssen nicht alles Plattform, das Objektmodell oder Programmiersprache ab, die zum Implementieren des Diensts kennen. Sie müssen verstehen, wie SOAP-Nachrichten senden und empfangen. In diesem Artikel veranschaulicht, wie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung nutzen.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Nutzen einen Windows Communication Foundation (WCF)-Webdienst](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Es ermöglicht Entwicklern, sichere, zuverlässige, transaktive und interoperable verteilte Anwendungen zu erstellen. Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF dieselben Funktionen unterstützt, die ASMX-SOAP-Nachrichten über HTTP. In diesem Artikel veranschaulicht, wie einen WCF-SOAP-Dienst aus einer Xamarin.Forms-Anwendung genutzt wird.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) ist eine Architektur zum Erstellen von Webdiensten. REST-Anforderungen werden über HTTP angefordert mithilfe der gleichen HTTP-Verben, die Webbrowser verwenden Sie zum Abrufen von Webseiten und Daten an Server senden. Dieser Artikel veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung nutzen.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Verwenden einer mobilen Anwendung für Azure](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps können Sie zum Entwickeln von apps mit skalierbare Back-Ends mit Unterstützung für mobile-Authentifizierung, offline-Synchronisierung und Push-Benachrichtigungen in Azure App Service, gehostet. Dieser Artikel, die nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden gilt, werden die Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Instanz von Azure-Mobile-Apps erläutert.

## <a name="consuming-an-amazon-simpledb-servicexamarin-formsdata-cloudconsumingawsmd"></a>[Verarbeiten einer Amazon SimpleDB-Diensts](~/xamarin-forms/data-cloud/consuming/aws.md)

Amazon SimpleDB ist ein Webdienst, der bietet die Möglichkeit zum Speichern und Abfragen von Daten in der Amazon-Cloud. In diesem Artikel wird erläutert, wie der AWS-SDK für .NET abzufragen, zu erstellen und zu ersetzen, und Löschen von Daten in einem Dienst SimpleDB gespeichert wird.


## <a name="related-links"></a>Verwandte Links

- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
