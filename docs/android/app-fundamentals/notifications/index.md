---
title: Notifications in Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bc39faa37adae660a7751313d0d573237fadce94
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="notifications-in-xamarinandroid"></a>Notifications in Xamarin.Android


## <a name="overview"></a>Übersicht

In diesem Abschnitt wird erläutert, wie Benachrichtigungen in Xamarin.Android implementiert wird. Es beschreibt die verschiedenen Benutzeroberflächenelemente der Android-Benachrichtigung und behandelt die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt.


## <a name="sections"></a>Abschnitte

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Lokale Benachrichtigungen In Android](local-notifications.md)

In diesem Abschnitt wird erläutert, wie lokale Benachrichtigungen in Xamarin.Android implementiert wird. Es beschreibt die verschiedenen Benutzeroberflächenelemente der Android-Benachrichtigung, und diskutieren Sie die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Exemplarische Vorgehensweise – mit lokalen Benachrichtigungen in Xamarin.Android](local-notifications-walkthrough.md)  
 
In dieser exemplarischen Vorgehensweise wird beschrieben, wie lokale Benachrichtigungen in einer Anwendung Xamarin.Android verwenden. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine Benachrichtigung. Klickt der Benutzer auf die Benachrichtigung in öffnen Sie die Benachrichtigung wird er eine zweite Aktivität gestartet. 


## <a name="for-further-reading"></a>Weitere Informationen

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) ist ein Dienst, das messaging zwischen mobilen apps und serveranwendungen vereinfacht. Firebase Cloud Messaging kann verwendet werden, remote Benachrichtigungen (so genannte Pushbenachrichtigungen) zu implementieren in Xamarin.Android Anwendungen.

[Benachrichtigungen](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; Android-Entwickler dieses Thema ist die Dokumentation für Android-Benachrichtigungen. Er enthält ein Design Considerations-Abschnitt, der Ihnen hilft Ihre Benachrichtigungen so entwerfen, dass sie die Richtlinien der Android-Benutzeroberfläche entsprechen. Es bietet weitere Informationen über die Preserviing Navigation beim Starten einer Aktivität, und es wird erläutert, wie zur Anzeige des Fortschritts in einer Benachrichtigung und Steuerung Medienwiedergabe auf dem Sperrbildschirm. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; Android dieser Dienst ermöglicht es der app zu überwachen (und damit interagieren) alle Benachrichtigungen gesendet werden, auf dem Android-Gerät, nicht nur die Benachrichtigungen, die für Ihre app registriert wurde empfangen. Beachten Sie, dass der Benutzer explizit auf Ihre app zum Abhören von Benachrichtigungen auf dem Gerät können zustimmen muss.





## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Remote-Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
