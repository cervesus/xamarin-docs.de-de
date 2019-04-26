---
title: Benachrichtigungen in Xamarin.Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b98ee5afbd65d5cf32bc6e3151284678e248cf47
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61014250"
---
# <a name="notifications-in-xamarinandroid"></a>Benachrichtigungen in Xamarin.Android


## <a name="overview"></a>Übersicht

In diesem Abschnitt wird erläutert, wie Benachrichtigungen in Xamarin.Android implementiert wird. Es wird beschrieben, die von einer Android-benachrichtigungs UI-Elemente und behandelt die API des mit dem Erstellen und Anzeigen einer Benachrichtigung beteiligt.


## <a name="sections"></a>Abschnitte

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Lokale Benachrichtigungen unter Android](local-notifications.md)

In diesem Abschnitt wird erläutert, wie lokale Benachrichtigungen in Xamarin.Android implementiert wird. Es wird beschrieben, die UI-Elemente von einer Android-benachrichtigungs und diskutieren Sie die API des mit dem Erstellen und Anzeigen einer Benachrichtigung beteiligt. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in Xamarin.Android](local-notifications-walkthrough.md)  
 
In dieser exemplarischen Vorgehensweise wird beschrieben, wie lokale Benachrichtigungen in einer Xamarin.Android-Anwendung verwendet wird. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine Benachrichtigung. Klickt der Benutzer auf die Benachrichtigung in die Benachrichtigung wird es einer zweiten Aktivität gestartet. 


## <a name="for-further-reading"></a>Weitere Informationen

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) ist ein Dienst, der ermöglicht, Nachrichten zwischen mobile apps und Server-Anwendungen. Firebase Cloud Messaging dienen zum Implementieren von remotebenachrichtigungen (auch als "Pushbenachrichtigungen" bezeichnet) in Xamarin.Android-Anwendungen.

[Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; Android-Entwickler dieses Thema ist das umfassende Handbuch für die Android-Benachrichtigungen. Es umfasst einen Entwurf Überlegungen-Abschnitt, der Ihnen hilft Ihrer Benachrichtigungen so entwerfen, dass sie die Richtlinien der Android-Benutzeroberfläche entsprechen. Es bietet weitere Hintergrundinformationen zu Preserviing Navigation, beim Starten von einer Aktivitäts, und es wird erläutert, wie Status in einer Benachrichtigung und Steuerelement Wiedergabe von Medien auf dem Sperrbildschirm angezeigt werden soll. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; dieses Android-Dienst ermöglicht es Ihrer app zum Lauschen auf (und damit interagieren) alle Benachrichtigungen gesendet werden, auf dem Android-Gerät nicht nur die Benachrichtigungen, die auf Ihre app registriert wurde empfangen. Beachten Sie, dass der Benutzer die Berechtigungen für Ihre app, damit es zum Abhören von Benachrichtigungen auf dem Gerät können explizit gewähren muss.





## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Remotebenachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
