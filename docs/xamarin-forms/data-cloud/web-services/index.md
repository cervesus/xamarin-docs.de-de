---
title: Xamarin.Formsund Webdienste
description: In diesem Handbuch wird erläutert, wie Sie mit verschiedenen Webdiensten kommunizieren, um eine Funktion zum Erstellen, lesen, aktualisieren und löschen (CRUD) für eine-Anwendung bereitzustellen Xamarin.Forms . Die behandelten Themen umfassen die Kommunikation mit ASMX-Diensten, WCF-Diensten und Rest-Diensten.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5b2613b94d2c347d9bc6a94086f869b07ab8a55b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131884"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Formsund Webdienste

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

Dieser Artikel enthält eine exemplarische Vorgehensweise für die Xamarin.Forms Beispielanwendung, die die Kommunikation mit verschiedenen Webdiensten veranschaulicht. Zu den behandelten Themen gehören die Anatomie der Anwendung, die Seiten, das Datenmodell und das Aufrufen von Webdienst Vorgängen.

## <a name="consume-an-aspnet-web-service-asmx"></a>[Verwenden eines ASP.NET-Webdiensts (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.net Web Services (ASMX) bieten die Möglichkeit, Webdienste zu erstellen, die Nachrichten über HTTP mithilfe von SOAP (Simple Object Access Protocol) senden. SOAP ist ein plattformunabhängiges und sprachunabhängiges Protokoll zum entwickeln und Zugreifen auf Webdienste. Consumer eines ASMX-Dienstanbieter müssen nichts über die Plattform, das Objektmodell oder die Programmiersprache wissen, die zur Implementierung des Dienstanbieter verwendet werden. Sie müssen nur wissen, wie SOAP-Nachrichten gesendet und empfangen werden. In diesem Artikel wird veranschaulicht, wie Sie einen ASMX-Webdienst aus einer- Xamarin.Forms Anwendung nutzen.

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[Verwenden eines Windows Communication Foundation (WCF)-Webdiensts](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF ist das vereinheitlichte Framework von Microsoft zum entwickeln Dienst orientierter Anwendungen. Es ermöglicht Entwicklern das Erstellen sicherer, zuverlässiger, transaktiver und interoperable verteilter Anwendungen. Es gibt Unterschiede zwischen ASP.NET-Webdiensten (ASMX) und WCF, aber es ist wichtig zu verstehen, dass WCF dieselben Funktionen unterstützt, die ASMX – SOAP-Nachrichten über HTTP bereitstellt. In diesem Artikel wird veranschaulicht, wie ein WCF-SOAP-Dienst aus einer-Anwendung verwendet wird Xamarin.Forms .

## <a name="consume-a-restful-web-service"></a>[Nutzen eines Rest-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)

Der Representational State Transfer (Rest) ist ein Architekturstil zum Entwickeln von Webdiensten. Rest-Anforderungen werden über HTTP mit denselben HTTP-Verben hergestellt, die Webbrowser zum Abrufen von Webseiten und zum Senden von Daten an Server verwenden. In diesem Artikel wird veranschaulicht, wie Sie einen Rest-Webdienst aus einer- Xamarin.Forms Anwendung nutzen.
