---
title: Xamarin.Formulare und Webdienste
description: In diesem Handbuch wird erläutert, wie Sie mit verschiedenen Webdiensten kommunizieren, um CRUD-Funktionen (Create, Read, Update, Delete) für eine Xamarin.Forms-Anwendung bereitzustellen. Zu den behandelten Themen gehören die Kommunikation mit ASMX-Diensten, WCF-Dienste und REST-Dienste.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 799a0a97f7c1a212b10dc62f8b3e7bd2cf2060c8
ms.sourcegitcommit: bf3b5925018e1bd6a00505e9f37500761b66809d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395458"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Formulare und Webdienste

## <a name="introduction"></a>[Einführung](introduction.md)

Dieser Artikel enthält eine exemplarische Vorgehensweise der Xamarin.Forms-Beispielanwendung, die veranschaulicht, wie sie mit verschiedenen Webdiensten kommunizieren. Zu den behandelten Themen gehören die Anatomie der Anwendung, die Seiten, das Datenmodell und das Aufrufen von Webdienstvorgängen.

## <a name="consume-an-aspnet-web-service-asmx"></a>[Verwenden eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web Services (ASMX) bieten die Möglichkeit, Webdienste zu erstellen, die Nachrichten über HTTP mit Simple Object Access Protocol (SOAP) senden. SOAP ist ein plattformunabhängiges und sprachunabhängiges Protokoll zum Erstellen und Zugreifen auf Webdienste. Benutzer eines ASMX-Dienstes müssen nichts über die Plattform, das Objektmodell oder die Programmiersprache wissen, die zum Implementieren des Dienstes verwendet wird. Sie müssen nur verstehen, wie SOAP-Nachrichten gesendet und empfangen werden. In diesem Artikel wird veranschaulicht, wie Sie einen ASMX-Webdienst aus einer Xamarin.Forms-Anwendung nutzen.

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[Verwenden eines Windows Communication Foundation (WCF)-Webdienstes](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF ist Microsofts einheitliches Framework zum Erstellen serviceorientierter Anwendungen. Es ermöglicht Entwicklern, sichere, zuverlässige, transacted und interoperable verteilte Anwendungen zu erstellen. Es gibt Unterschiede zwischen ASP.NET Web Services (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF dieselben Funktionen unterstützt, die ASMX bietet – SOAP-Nachrichten über HTTP. In diesem Artikel wird veranschaulicht, wie Sie einen WCF SOAP-Dienst aus einer Xamarin.Forms-Anwendung verwenden.

## <a name="consume-a-restful-web-service"></a>[Verwenden eines RESTful-Webdienstes](~/xamarin-forms/data-cloud/web-services/rest.md)

Representational State Transfer (REST) ist ein architekturaler Stil zum Erstellen von Webdiensten. REST-Anforderungen werden über HTTP mit denselben HTTP-Verben gestellt, die Webbrowser zum Abrufen von Webseiten und zum Senden von Daten an Server verwenden. In diesem Artikel wird veranschaulicht, wie Sie einen RESTful-Webdienst aus einer Xamarin.Forms-Anwendung verwenden.
