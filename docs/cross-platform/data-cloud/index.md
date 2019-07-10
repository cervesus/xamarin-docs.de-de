---
title: Microsoft Azure und Xamarin
description: Dieses Dokument enthält Links zur Dokumentation zu verbundenen Diensten in Visual Studio für Mac, die Azure Mobile Apps, die Active Directory-Authentifizierung und die Web-API.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: dd211fecad0bff58cb9ff6c6a99ae6a15c60eb7b
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674991"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure und Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services-Funktionen sind einfach zu Xamarin-apps, einschließlich cloudspeicher für Daten und plattformübergreifende Pushbenachrichtigungen hinzufügen")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Weiterentwicklung 2016: Entwickeln von verbundenen Apps mit Azure und Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Verbundene Dienste in Visual Studio für Mac

Die neue [verbundene Dienste](connected-services.md) Feature von Visual Studio für Mac kann Entwickler schnell und einfach Azure-Funktionen für mobile Anwendungen aus der IDE hinzugefügt. Derzeit verfügbar für das Testen in der Alpha-Kanal.

## <a name="azure-app-services"></a>Azure App Services

Es ist eine Sammlung von [Azure Mobile Apps-Dokumentation](~/cross-platform/data-cloud/mobile-apps.md) führt, die Sie durch die Implementierung der [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin bietet auch eine Azure-Messaging-NuGet-Pakete für [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) und [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) , um erste Schritte mit Pushbenachrichtigungen über Plattformen hinweg zu implementieren.

Konfigurieren Sie Ihre apps auf die [Azure App Services-Portal](https://portal.azure.com/) auf Mobile Apps, Web-APIs, Speicher und vieles mehr zuzugreifen. Erfahren Sie mehr über [wie app-Dienste unterscheiden](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) und sehen Sie sich [diese Videos von Microsoft](https://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Active Directory-Authentifizierung

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) kann verwendet werden, um Benutzer anzumelden, in Xamarin-apps über die [Xamarin.Auth-Komponente](https://www.nuget.org/packages/Xamarin.Auth/).
Die apps können anschließend zusätzliche Dienste wie Office 365 zugreifen.

## <a name="webapi"></a>WebAPI

Microsoft Web-API macht eine REST-ähnliche-Schnittstelle, die leicht von Xamarin-Anwendungen genutzt werden kann.
Sie können leicht Hochfahren ein [Azure-Website](https://trywebsites.azurewebsites.net/) , und erstellen Sie eine Web-API-basierte Anwendung für die Verbindung mit Xamarin-apps.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Einführung in Webdienste](~/cross-platform/data-cloud/web-services/index.md)

In diesem Tutorial wird erläutert, wie Sie integrieren von REST, WCF und SOAP-web-Technologien mit mobile Xamarin-Anwendungen. Verschiedene dienstimplementierungen untersucht, ausgewertet wird, verfügbaren Tools und Bibliotheken zu integrieren und Beispiel-Muster für die Nutzung von Daten bietet. Zum Schluss wird eine grundlegende Übersicht über die zum Erstellen eines RESTful-Webdiensts für die Nutzung mit einer mobilen Xamarin-Anwendung.

## <a name="samples"></a>Proben

Zusätzlich zu den [Dokumentationsbeispiele](https://github.com/xamarin/mobile-samples/tree/master/Azure), die folgenden vollständigen Anwendungen veranschaulichen die verschiedenen Azure-Features, die in Xamarin-apps integriert:

- [Sport](https://github.com/xamarin/Sport) – benutzerfreundliche Sportliga-nachverfolgung-app, die Data-Speicher und Push-Benachrichtigungen verwendet.
- [Minuten](https://github.com/pierceboggan/Moments) – sofortige, die zum Teilen von Fotos verwendet Azure-Speicher für Bilder.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – Web-API für das Back-End verwendet.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) – Azure Mobile Apps.

- [Shopping](https://github.com/dotnet-architecture/eShopOnContainers) – Beispiel für die [Architektur Reihe](https://www.microsoft.com/net/learn/architecture) der e-Books.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure und IoT-Beispiel von der Build 2016.


## <a name="related-links"></a>Verwandte Links

- [Beispiel für eine Azure PCL (von @paulbatum) (Beispiel)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure-portal](https://azure.microsoft.com/)
- [Mobiler Client für Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
