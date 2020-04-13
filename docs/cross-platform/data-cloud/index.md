---
title: Microsoft Azure und Xamarin
description: Dieses Dokument enthält Links zu dokumentation en Connected Services in Visual Studio for Mac, Azure Mobile Apps, Active Directory Authentication und WebAPI.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 2b6dfeb0de0fac59556280609dbf870c23a9298b
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388524"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure und Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Evolve 2016: Entwickeln von verbundenen Apps mithilfe von Azure und Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Verbundene Dienste in Visual Studio für Mac

Die neue [Funktion "Verbundene Dienste"](connected-services.md) von Visual Studio für Mac hilft Entwicklern, schnell und einfach Azure-Funktionen für mobile Anwendungen innerhalb der IDE hinzuzufügen. Derzeit zum Testen im Alpha-Kanal verfügbar.

## <a name="azure-app-services"></a>Azure App Services

Es gibt eine Sammlung von [Azure Mobile Apps-Dokumentationen,](~/cross-platform/data-cloud/mobile-apps.md) die Sie durch den Prozess der Implementierung des [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)führt.
Xamarin bietet auch Azure Messaging NuGet-Pakete für [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) und [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) an, um Pushbenachrichtigungen plattformübergreifend zu implementieren.

Konfigurieren Sie Ihre Apps im [Azure App Services-Portal](https://portal.azure.com/) für den Zugriff auf Mobile Apps, Web-APIs, Speicher und vieles mehr. Erfahren [Sie, wie sich App-Dienste unterscheiden,](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) und sehen Sie sich dies in [diesen Videos von Microsoft an.](https://azure.microsoft.com/campaigns/azure-march-announcement/)

## <a name="active-directory-authentication"></a>Active Directory-Authentifizierungs-

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) kann verwendet werden, um Benutzer bei Xamarin-Apps anzumelden. Die Apps können dann auf zusätzliche Dienste wie Office 365 zugreifen.

## <a name="webapi"></a>WebAPI

Die Web-API von Microsoft macht eine REST-ähnliche Schnittstelle verfügbar, die von Xamarin-Anwendungen problemlos genutzt werden kann.
Sie können eine [Azure-Website](https://trywebsites.azurewebsites.net/) einfach neu erstellen und eine WebAPI-basierte App erstellen, um eine Verbindung mit Xamarin-Apps herzustellen.

### <a name="introduction-to-web-services"></a>[Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)

In diesem Tutorial wird die Integration von REST-, WCF- und SOAP-Webdiensttechnologien in mobile Xamarin-Anwendungen vorgestellt. Es untersucht verschiedene Dienstimplementierungen, wertet verfügbare Tools und Bibliotheken aus, um sie zu integrieren, und stellt Beispielmuster für die Verwendung von Dienstdaten bereit. Schließlich bietet es einen grundlegenden Überblick über das Erstellen eines RESTful-Webdienstes für den Verbrauch mit einer mobilen Xamarin-Anwendung.

## <a name="samples"></a>Beispiele

Zusätzlich zu den [Dokumentationsbeispielen](https://github.com/xamarin/mobile-samples/tree/master/Azure)zeigen die folgenden vollständigen Anwendungen verschiedene Azure-Funktionen, die in Xamarin-Apps integriert sind:

- [Sport](https://github.com/xamarin/Sport) – freundliche Sport-Liga-Tracking-App, die Datenspeicherung & Push-Benachrichtigungen verwendet.
- [Momente](https://github.com/pierceboggan/Moments) – sofortige Fotofreigabe, die Azure Storage für Bilder verwendet.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – verwendet Web-API für das Back-End.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) - Azure Mobile Apps.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – Beispiel für die [Architektur-Reihe](https://www.microsoft.com/net/learn/architecture) von E-Books.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IoT-Beispiel aus Build 2016.

## <a name="related-links"></a>Verwandte Links

- [Azure PCL-Beispiel @paulbatum(nach ) (Beispiel)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portal](https://azure.microsoft.com/)
- [Mobile Client für Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
