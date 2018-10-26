---
title: Dienstbenachrichtigungen
description: Dieser Leitfaden erläutert, wie ein Android-Dienst lokale Benachrichtigungen verwenden dürfen, um Informationen zu einem Benutzer zu senden.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d56f67254a9eae334fa8ac3f08d3ef270800c309
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103247"
---
# <a name="service-notifications"></a>Dienstbenachrichtigungen

_Dieser Leitfaden erläutert, wie ein Android-Dienst lokale Benachrichtigungen verwenden dürfen, um Informationen zu einem Benutzer zu senden._


## <a name="service-notifications-overview"></a>Übersicht über Service-Benachrichtigungen

Dienstbenachrichtigungen können eine app zum Anzeigen von Informationen für den Benutzer, auch wenn die Android-Anwendung nicht im Vordergrund ist. Es ist möglich, eine Benachrichtigung-Aktionen für den Benutzer, z. B. das Anzeigen einer Aktivität aus einer Anwendung bieten. Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienst eine Benachrichtigung zu einem Benutzer verteilen kann:

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

In diesem Screenshot ist ein Beispiel für die Benachrichtigung, die angezeigt wird:

[![Benachrichtigungssymbol angezeigt, in der Statusleiste](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Wenn die Benutzer-Folien, die Benachrichtigung-Bildschirm von oben nach unten, wird die vollständige Benachrichtigung angezeigt:

![Benachrichtigung, die im Benachrichtigungsbereich angezeigt](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Aktualisieren eine Benachrichtigung

Um eine Benachrichtigung zu aktualisieren, wird der Dienst die Benachrichtigung wird über die gleiche benachrichtigungs-ID erneut veröffentlichen Android anzeigt oder die Benachrichtigung in der Statusleiste nach Bedarf zu aktualisieren.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Weitere Informationen zu Benachrichtigungen finden Sie in der [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md) Teil der [Android-Benachrichtigungen](~/android/app-fundamentals/notifications/index.md) Guide.


## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen unter Android](~/android/app-fundamentals/notifications/local-notifications.md)
