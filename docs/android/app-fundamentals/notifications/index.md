---
title: Benachrichtigungen in xamarin. Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 5d91904e35a658b03d4602567e5a123cafd6926c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025533"
---
# <a name="notifications-in-xamarinandroid"></a>Benachrichtigungen in xamarin. Android

In diesem Abschnitt wird erläutert, wie Benachrichtigungen in xamarin. Android implementiert werden. Darin werden die verschiedenen Benutzeroberflächen Elemente einer Android-Benachrichtigung beschrieben. Außerdem wird erläutert, welche API zum Erstellen und Anzeigen einer Benachrichtigung beteiligt ist.

## <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Lokale Benachrichtigungen in Android](local-notifications.md)

In diesem Abschnitt wird erläutert, wie lokale Benachrichtigungen in xamarin. Android implementiert werden. Es beschreibt die verschiedenen Benutzeroberflächen Elemente einer Android-Benachrichtigung und erläutert die APIs, die beim Erstellen und Anzeigen einer Benachrichtigung beteiligt sind.

## <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in xamarin. Android](local-notifications-walkthrough.md)  

In dieser exemplarischen Vorgehensweise wird erläutert, wie lokale Benachrichtigungen in einer xamarin. Android-Anwendung verwendet werden. Es veranschaulicht die Grundlagen der Erstellung und Veröffentlichung einer Benachrichtigung. Wenn der Benutzer in der Benachrichtigungsleiste auf die Benachrichtigung klickt, startet er eine zweite Aktivität. 

## <a name="further-reading"></a>Weiterführende Themen

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (SCM) ist ein Dienst, der das Messaging zwischen mobilen apps und Server Anwendungen ermöglicht. Firebase Cloud Messaging kann zum Implementieren von Remote Benachrichtigungen (auch als Pushbenachrichtigungen bezeichnet) in xamarin. Android-Anwendungen verwendet werden.

[Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; dieses Android-Entwickler Thema ist das definitive Handbuch für Android-Benachrichtigungen. Es enthält einen Abschnitt Überlegungen zum Entwurf, der Ihnen hilft, Ihre Benachrichtigungen so zu entwerfen, dass Sie den Richtlinien der Android-Benutzeroberfläche entsprechen. Sie bietet weitere Hintergrundinformationen zur vorab servitätsnavigation beim Starten einer Aktivität und erläutert, wie der Fortschritt in einer Benachrichtigung angezeigt und die Medienwiedergabe auf dem Sperrbildschirm gesteuert wird.

[Notificationlistenerservice](xref:Android.Service.Notification.NotificationListenerService) &ndash; dieser Android-Dienst ermöglicht es Ihrer APP, alle auf dem Android-Gerät veröffentlichten Benachrichtigungen zu lauschen (und damit zu interagieren), nicht nur die Benachrichtigungen, die Ihre APP für den Empfang registriert hat.
Beachten Sie, dass der Benutzer die Berechtigung für Ihre APP explizit erteilen muss, damit er auf dem Gerät auf Benachrichtigungen lauschen kann.

## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Remote Benachrichtigungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/remotenotifications)
