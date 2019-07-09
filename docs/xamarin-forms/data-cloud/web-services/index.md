---
title: Xamarin.Forms und Webdienste
description: Dieser Leitfaden erläutert, wie für die Kommunikation mit anderen Webdiensten zu erstellen, lesen, aktualisieren und löschen (CRUD)-Funktionen zu einer Xamarin.Forms-Anwendung. Zu den behandelten Themen umfassen kommuniziert mit ASMX-Dienste, die WCF-Dienste, die REST-Diensten.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 1cf2714191528c5619b4f877bcb43e80464c44d1
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659007"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Forms und Webdienste

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Dieser Artikel enthält eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die zeigt, wie Sie mit anderen Webdiensten kommunizieren. Zu den behandelten Themen gehören die Struktur der Anwendung, die Seiten, die ein Datenmodell, und das Aufrufen von Webdienstvorgängen.

## <a name="consume-an-aspnet-web-service-asmxxamarin-formsdata-cloudweb-servicesasmxmd"></a>[Verwenden eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET-Webdienste (ASMX) bieten die Möglichkeit, Webdienste zu erstellen, die Nachrichten über HTTP senden (SOAP, Simple Object Access Protocol) verwenden. SOAP ist ein Plattform- und sprachenunabhängiges Protokoll für die Erstellung und den Zugriff auf Webdienste. Consumer einen ASMX-Dienst müssen nicht alles über die Plattform, das Objektmodell oder zum Implementieren des Diensts verwendete Programmiersprache wissen. Sie müssen nur wissen, wie zum Senden und Empfangen von SOAP-Nachrichten. In diesem Artikel wird veranschaulicht, wie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung genutzt wird.

## <a name="consume-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudweb-serviceswcfmd"></a>[Nutzen Sie einen Windows Communication Foundation (WCF)-Webdienst](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF ist Microsofts einheitliches Framework zum Erstellen von dienstorientierten Anwendungen. Sie können Entwickler sichere, zuverlässige, transaktionsbasierte und interoperable verteilte Anwendungen erstellen. Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF die gleichen Funktionen unterstützt, die ASMX bereitstellt – SOAP-Nachrichten über HTTP. In diesem Artikel wird veranschaulicht, wie ein WCF-SOAP-Webdiensts aus einer Xamarin.Forms-Anwendung wird.

## <a name="consume-a-restful-web-servicexamarin-formsdata-cloudweb-servicesrestmd"></a>[Verwenden eines RESTful-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)

Representational State Transfer (REST) ist ein Architekturstil zum Erstellen von Webdiensten. REST-Anforderungen erfolgen über HTTP mit den gleichen HTTP-Verben, die Webbrowser zum Abrufen von Webseiten und zum Senden von Daten auf Servern verwenden. In diesem Artikel wird veranschaulicht, wie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung genutzt wird.
