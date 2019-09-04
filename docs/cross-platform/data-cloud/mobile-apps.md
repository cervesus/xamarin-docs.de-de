---
title: Microsoft Azure Mobile Apps
description: Dieses Dokument stellt Links zu Anleitungen dar, in denen beschrieben wird, wie eine mit Azure verbundene xamarin-App erstellt wird. Es wird erläutert, wie Sie die xamarin-Azure-Komponente, Benutzer und Pushbenachrichtigungen einsetzen.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: 093e2ad9cf18abecc4eb1b63690c016e8ed21a0c
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227775"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_Beispiele und Code Downloads für die Azure-Portal-Dokumentation._

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
as https://developer xamarin com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data  http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Diese Links gelten für die xamarin-Dokumentation, die auf der [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) -Website verfügbar ist.
Hinzufügen von Azure-Funktionen zu einer xamarin-app durch Herunterladen des [Azure Mobile-Clients](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Arbeiten mit der xamarin Azure-Komponente

Allgemeine Dokumentation zum [Arbeiten mit der xamarin-Client Bibliothek (Komponente)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) zum Ausführen verschiedener Aufgaben mit Azure Mobile Apps. Diese Seite enthält viele Beispielcode Ausschnitte, ohne die ausführlichen Erläuterungen und Beispiele in den einzelnen exemplarischen Vorgehensweisen, die unten aufgeführt sind.

## <a name="getting-started"></a>Erste Schritte

Dieser Artikel enthält Schritt-für-Schritt-Anleitungen, mit denen Sie Ihre erste xamarin-Azure-app in Betrieb nehmen können.
Darin wird das Erstellen einer neuen mobilen Azure-App im Portal und das anschließende herunterladen und Ausführen der vorkonfigurierten App behandelt.

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
- [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Einstieg in die Benutzer

Enthält vollständige Anweisungen zum Konfigurieren und Codieren eines Anmeldebildschirms mithilfe von Azure Mobile Services. Zu den unterstützten Authentifizierungs Anbietern zählen Microsoft, Google, Facebook und Twitter.

- [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
- [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autorisieren von Benutzern in Skripts

Beispielcode für JavaScript-Back-Ends

- [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Einstieg in Push

Vervollständigen Sie die Anweisungen zum Konfigurieren von Pushbenachrichtigungen auf Apple-und Google-Websites, und senden Sie dann eine Pushbenachrichtigung von Azure Mobile Services an ein Gerät.

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Beginnen Sie mit Notification Hubs

Vervollständigen Sie die Anweisungen zum Konfigurieren von Pushbenachrichtigungen auf der Apple-und Google-Website, konfigurieren Sie den Azure Notification Hub, und generieren Sie dann Pushbenachrichtigungen an Geräte.

- [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
- [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>Verwandte Links

- [GettingStarted (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [Getstartedwithdata (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [Getstartedwithusers (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [Getstartedwithpush (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [Notificationhubs (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure Mobile-Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Lernpfad für Azure Mobile Apps](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
