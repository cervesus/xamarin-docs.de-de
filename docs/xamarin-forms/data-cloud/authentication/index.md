---
title: Authentifizieren des Zugriffs auf Webdienste
description: Dieses Handbuch wird erläutert, wie Authentifizierungsdienste in einer Xamarin.Forms-Anwendung, damit Benutzer auf eine Back-End zu verwenden, während Sie nur den Zugriff auf ihre eigenen Daten können zu integrieren.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: d598a9b3de31ea6823530f911c3544bf3cebb37f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331181"
---
# <a name="authenticating-access-to-web-services"></a>Authentifizieren des Zugriffs auf Webdienste

_Dieses Handbuch wird erläutert, wie Authentifizierungsdienste in einer Xamarin.Forms-Anwendung, damit Benutzer auf eine Back-End zu verwenden, während Sie nur den Zugriff auf ihre eigenen Daten können zu integrieren. Behandelten Themen gehören die unter Verwendung der Standardauthentifizierung mit einem REST-Dienst, mit die Xamarin.Auth-Komponente zum OAuth-Identitätsanbieter authentifizieren, und die integrierten Authentifizierungsmechanismen mit Angeboten von anderen Anbietern._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[Die Authentifizierung eines RESTful-Web-Diensts](rest.md)

HTTP unterstützt die Verwendung mehrere Authentifizierungsmechanismen zum Steuern des Zugriffs auf Ressourcen. Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. In diesem Artikel veranschaulicht die Standardauthentifizierung zu verwenden, um den Zugriff auf RESTful-Web-Service-Ressourcen zu schützen.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Authentifizieren von Benutzern mit einem Identitätsanbieter](oauth.md)

Xamarin.Auth ist ein Plattform-SDK für die Authentifizierung von Benutzern, und speichern ihre Konten an. Es enthält die OAuth-Authentifikatoren, die Unterstützung bieten, für die Nutzung von Identitätsanbietern wie Google, Microsoft, Facebook und Twitter. In diesem Artikel wird erläutert, wie Xamarin.Auth zum Verwalten des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung verwendet wird.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Authentifizieren von Benutzern mit Azure Mobile Apps](azure.md)

Mit Azure Mobile Apps verwenden eine Vielzahl externer Identitätsanbieter zum Authentifizieren und Autorisieren von Benutzern der Anwendung zu unterstützen. Klicken Sie dann können Berechtigungen für Tabellen festgelegt werden, um Zugriff auf nur authentifizierte Benutzer beschränken. In diesem Artikel wird erläutert, wie Sie Azure Mobile Apps zu verwenden, um die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung ermöglichen.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Authentifizieren von Benutzern mit Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird veranschaulicht, wie Microsoft Authentication Library (MSAL) und Azure Active Directory B2C verwenden, um die Verwaltung von Kundenidentitäten in einer Xamarin.Forms-Anwendung zu integrieren.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Integrieren von Azure Active Directory B2C in Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Azure Active Directory B2C kann verwendet werden, um den Workflow der Authentifizierung für Azure Mobile Apps zu verwalten. Bei diesem Ansatz die Verwaltungsoberfläche für die Identität ist in der Cloud vollständig definiert und geändert werden kann, ohne Ihren mobilen Anwendungscode ändern zu müssen. In diesem Artikel wird veranschaulicht, wie Sie Azure Active Directory B2C zum Bereitstellen von Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms zu verwenden.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
