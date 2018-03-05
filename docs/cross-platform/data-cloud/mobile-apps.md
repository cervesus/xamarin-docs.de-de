---
title: Microsoft Azure Mobile Apps
description: "Downloads für das Azure Portal-Dokumentation, Beispiele und Code."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fb3c26b7d090ca42328c61192c794dec1544d1d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_Downloads für das Azure Portal-Dokumentation, Beispiele und Code._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Diese Links werden für Xamarin-Dokumentation auf der [Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) Website.
Hinzufügen von Azure-Funktionen in einer Xamarin-app ist so einfach wie das Herunterladen der [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Arbeiten mit der Xamarin-Azure-Komponente

Allgemeiner Natur [arbeiten mit der Xamarin-Clientbibliothek (Komponente)](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) Ausführen verschiedener Aufgaben mit Azure Mobile Apps. Diese Seite enthält viele Beispielcodeausschnitten, ohne eine ausführliche Beschreibung und Beispiele, die in jedem der unten aufgeführten exemplarischen Vorgehensweise Artikel verfügbar.

## <a name="getting-started"></a>Erste Schritte

Dieser Artikel enthält schrittweise Anleitungen zum Abrufen von der ersten Xamarin Azure-app einrichten und ausführen.
Es umfasst erstellen eine neue Azure-Mobile-App in das Portal und herunterladen und Ausführen der vorkonfiguriert, dass app.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## <a name="validate-modify-and-augment-data-in-scripts"></a>Überprüfen Sie, ändern und Erweitern von Daten in Skripts

Veranschaulicht, wie Azure Mobile Services-Datentabellen, um die serverseitige Validierung und andere Funktionen zu implementieren: serverseitige Skripts hinzuzufügen.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## <a name="add-paging-to-data"></a>Hinzufügen von Paging zu Daten

Ein kurzes Beispiel große Mengen von Daten mithilfe von Skip() und Take() Paging.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## <a name="get-started-with-users"></a>Erste Schritte mit Benutzern

Stellt ausführliche Anweisungen zum Konfigurieren und codieren Anmeldebildschirm mit Azure Mobile Services bereit. Unterstützten Authentifizierungsanbieter gehören Microsoft, Google, Facebook und Twitter.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autorisieren von Benutzern in Skripts

Beispielcode für Javascript-Back-Ends

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Erste Schritte mit Pushbenachrichtigungen

Führen Sie die Anweisungen zum Konfigurieren von Pushbenachrichtigungen für Apple und Google-Websites, und anschließend eine Pushbenachrichtigung von Azure Mobile Services auf einem Gerät zu senden.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## <a name="get-started-with-notification-hubs"></a>Erste Schritte mit Notification Hubs

Führen Sie die Anweisungen zum Konfigurieren von Pushbenachrichtigungen für Apple und Google-Websites, Konfigurieren der Azure-Benachrichtigungs-Hub und anschließend generieren Pushbenachrichtigungen für Geräte.

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## <a name="related-links"></a>Verwandte Links

- [Erste Schritte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure PCL-Beispiel (durch @paulbatum) (Beispiel)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure Mobile-Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Lernpfad für Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)
