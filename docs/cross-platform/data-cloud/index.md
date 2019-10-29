---
title: Microsoft Azure und xamarin
description: Dieses Dokument ist mit der Dokumentation zu verbundene Dienste in Visual Studio für Mac, Azure Mobile Apps, Active Directory Authentifizierung und WebAPI verknüpft.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 273a1a8fec4cf40893ff94fef4b1394065a8547b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016614"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure und xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Entwickeln 2016: entwickeln verbundener apps mit Azure und xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Verbundene Dienste in Visual Studio für Mac

Das neue [verbundene Dienste](connected-services.md) Feature von Visual Studio für Mac unterstützt Entwickler bei der schnellen und einfachen Hinzufügen von Azure-Funktionen zu mobilen Anwendungen innerhalb der IDE. Derzeit zum Testen im Alpha Kanal verfügbar.

## <a name="azure-app-services"></a>Azure-App Dienste

Es gibt eine Sammlung von [Azure Mobile Apps-Dokumentation](~/cross-platform/data-cloud/mobile-apps.md) , die Sie durch den Prozess der Implementierung von [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)führt.
Xamarin bietet auch ein Azure Messaging-nuget-Paket für [IOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) und [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) , um die plattformübergreifende Implementierung von Pushbenachrichtigungen zu unterstützen.

Konfigurieren Sie Ihre apps im [Azure-App Services-Portal](https://portal.azure.com/) , um auf Mobile Apps, Web-APIs, Speicher und vieles mehr zuzugreifen. Erfahren Sie, [wie sich app Services unterscheiden](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) , und sehen Sie sich [Diese Videos von Microsoft](https://azure.microsoft.com/campaigns/azure-march-announcement/)an.

## <a name="active-directory-authentication"></a>Active Directory Authentifizierung

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) können zum Anmelden von Benutzern in xamarin-Apps über die [xamarin. auth-Komponente](https://www.nuget.org/packages/Xamarin.Auth/)verwendet werden.
Die apps können dann auf weitere Dienste wie Office 365 zugreifen.

## <a name="webapi"></a>WebAPI

Die Web-API von Microsoft macht eine Rest-ähnliche Schnittstelle verfügbar, die problemlos von xamarin-Anwendungen genutzt werden kann.
Sie können problemlos eine [Azure-Website](https://trywebsites.azurewebsites.net/) einrichten und eine WebAPI-basierte App zum Herstellen einer Verbindung mit xamarin-Apps erstellen.

### <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)

In diesem Tutorial wird erläutert, wie Sie Rest-, WCF-und SOAP-Webdienst Technologien mit mobilen xamarin-Anwendungen integrieren. Es werden verschiedene Dienst Implementierungen untersucht, verfügbare Tools und Bibliotheken für die Integration ausgewertet und Beispiel Muster zum Verarbeiten von Dienst Daten bereitgestellt. Schließlich bietet es einen grundlegenden Überblick über die Erstellung eines Rest-Webdiensts für die Verwendung mit einer mobilen xamarin-Anwendung.

## <a name="samples"></a>Proben

Zusätzlich zu den [Dokumentations Beispielen](https://github.com/xamarin/mobile-samples/tree/master/Azure)veranschaulichen die folgenden vollständigen Anwendungen verschiedene Azure-Features, die in xamarin-Apps integriert sind:

- [Sport](https://github.com/xamarin/Sport) – freundliche Sport-Liga-nach Verfolgungs-APP, die Datenspeicher & Pushbenachrichtigungen verwendet.
- [Augenblicke](https://github.com/pierceboggan/Moments) – sofortige Freigabe von Fotos, die Azure Storage für Bilder verwendet.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – verwendet die Web-API für das Back-End.
- [Myshoppe](https://github.com/jamesmontemagno/MyShoppe) : Azure-Mobile Apps.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – Beispiel für die [Architektur Reihe](https://www.microsoft.com/net/learn/architecture) von eBooks.
- [Mydriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IOT-Beispiel von Build 2016.

## <a name="related-links"></a>Verwandte Links

- [Azure PCL-Beispiel (durch @paulbatum) (Beispiel)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure-Portal](https://azure.microsoft.com/)
- [Mobile Client für xamarin (nuget)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
