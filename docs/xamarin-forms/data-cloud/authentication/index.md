---
title: Authentifizieren des Zugriffs auf Webdienste
description: Dieses Handbuch wird erläutert, wie zum Integrieren von Authentifizierungsdienste in einer Xamarin.Forms-Anwendung von Benutzern ein Back-End-freigeben, während Sie den Zugriff auf ihre eigenen Daten nur eine aktiviert werden. Zu den behandelten Themen gehören unter Verwendung der Standardauthentifizierung mit einem REST-Dienst, mithilfe der Komponente zur Xamarin.Auth zum Authentifizieren beim OAuth-Identitätsanbieter, und mithilfe der integrierten Authentifizierungsmechanismen von unterschiedlichen Anbietern angeboten.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: df0e188efd2791b03a63c31b715ed1da77079230
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-access-to-web-services"></a>Authentifizieren des Zugriffs auf Webdienste

_Dieses Handbuch wird erläutert, wie zum Integrieren von Authentifizierungsdienste in einer Xamarin.Forms-Anwendung von Benutzern ein Back-End-freigeben, während Sie den Zugriff auf ihre eigenen Daten nur eine aktiviert werden. Zu den behandelten Themen gehören unter Verwendung der Standardauthentifizierung mit einem REST-Dienst, mithilfe der Komponente zur Xamarin.Auth zum Authentifizieren beim OAuth-Identitätsanbieter, und mithilfe der integrierten Authentifizierungsmechanismen von unterschiedlichen Anbietern angeboten._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[Authentifizieren von RESTful-Webdienst](rest.md)

HTTP unterstützt die Verwendung von mehreren Authentifizierungsmechanismen zum Steuern des Zugriffs auf Ressourcen. Standardauthentifizierung bietet Zugriff auf Ressourcen, um nur die Clients, die über die richtigen Anmeldeinformationen verfügen. In diesem Artikel veranschaulicht die Standardauthentifizierung verwenden, um den Zugriff auf Ressourcen von RESTful-Web-Dienst zu schützen.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Authentifizieren von Benutzern mit einem Identitätsanbieter](oauth.md)

Xamarin.Auth ist eine plattformübergreifende-SDK für die Authentifizierung von Benutzern und speichern ihre Konten. Es umfasst OAuth-Authentifikatoren, die Unterstützung für die Nutzung der Identitätsanbieter z. B. Google, Microsoft, Facebook und Twitter. In diesem Artikel erläutert die Xamarin.Auth verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung zu verwalten.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Authentifizieren von Benutzern mit Azure Mobile Apps](azure.md)

Azure Mobile Apps verwenden eine Vielzahl von externen Identitätsanbieter zum Authentifizieren und Autorisieren von Benutzern zu unterstützen. Berechtigungen können klicken Sie dann auf Tabellen festgelegt werden, um Zugriff auf nur durch authentifizierte Benutzer zu beschränken. In diesem Artikel wird erläutert, wie Azure Mobile Apps zu verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung verwalten.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Authentifizieren von Benutzern mit Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen. Dieser Artikel veranschaulicht, wie Microsoft Authentication Library (MSAL) und Azure Active Directory B2C Consumer identitätsverwaltung in einer Xamarin.Forms-Anwendung integriert.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Integrieren von Azure Active Directory B2C in Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Azure Active Directory B2C kann zum Verwalten der Authentifizierung für Azure-Mobile-Apps verwendet werden. Bei diesem Ansatz die Verwaltungsoberfläche für die Identität ist vollständig in der Cloud definiert und kann ohne Änderung des Codes für die mobile Anwendung geändert werden. Dieser Artikel veranschaulicht, wie Azure Active Directory B2C-Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms bereitstellen.

## <a name="authenticating-users-with-an-amazon-simpledb-serviceawsmd"></a>[Authentifizieren von Benutzern mit einer Amazon SimpleDB-Dienst](aws.md)

Amazon SimpleDB bietet keine eigenen ressourcenbasierte Berechtigungssystem. Authentifizierung bei einem Identitätsanbieter kann stattdessen verwendet werden, um sicherzustellen, dass Benutzer nur Zugriff auf ihre eigenen Daten in der Domäne SimpleDB haben. In diesem Artikel wird erläutert, wie Benutzer den Zugriff auf ihre eigenen Daten SimpleDB zu beschränken.


## <a name="related-links"></a>Verwandte Links

- [Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
