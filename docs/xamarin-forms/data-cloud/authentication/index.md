---
title: Xamarin.Forms-Web-Service-Authentifizierung
description: Dieses Handbuch wird erläutert, wie Authentifizierungsdienste in einer Xamarin.Forms-Anwendung, damit Benutzer auf eine Back-End zu verwenden, während Sie nur den Zugriff auf ihre eigenen Daten können zu integrieren.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: a36dfba7aa07de1633ca9620674ddb4887902ba9
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650423"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.Forms-Web-Service-Authentifizierung

## <a name="authenticate-a-restful-web-servicerestmd"></a>[Authentifizieren eines RESTful-Webdiensts](rest.md)

HTTP unterstützt die Verwendung mehrere Authentifizierungsmechanismen zum Steuern des Zugriffs auf Ressourcen. Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. In diesem Artikel erläutert die Standardauthentifizierung zu verwenden, um den Zugriff auf RESTful-Web-Service-Ressourcen zu schützen.

## <a name="authenticate-users-with-an-identity-provideroauthmd"></a>[Authentifizieren von Benutzern mit einem Identitätsanbieter](oauth.md)

Xamarin.Auth ist ein Plattform-SDK für die Authentifizierung von Benutzern, und speichern ihre Konten an. Es enthält die OAuth-Authentifikatoren, die Unterstützung bieten, für die Nutzung von Identitätsanbietern wie Google, Microsoft, Facebook und Twitter. In diesem Artikel wird erläutert, wie Xamarin.Auth zum Verwalten des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung verwendet wird.

## <a name="authenticate-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Authentifizieren von Benutzern mit Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird erläutert, wie Microsoft Authentication Library (MSAL) und Azure Active Directory B2C verwenden, um die Verwaltung von Kundenidentitäten in einer Xamarin.Forms-Anwendung zu integrieren.

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[Authentifizieren von Benutzern mit einem Azure Cosmos DB-Dokumentdatenbank und Xamarin.Forms](azure-cosmosdb-auth.md)

Azure Cosmos DB-Dokumentdatenbanken unterstützen partitionierte Sammlungen enthalten, die mehrere Server und Partitionen unterstützt unbegrenzten Speicher und Durchsatz umfassen können. In diesem Artikel wird erläutert, wie Zugriffssteuerung mit partitionierten Sammlungen kombiniert, damit ein Benutzer nur ihre eigenen Dokumente in einer Xamarin.Forms-Anwendung zugreifen kann.
