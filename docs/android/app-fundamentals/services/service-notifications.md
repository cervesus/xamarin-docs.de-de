---
title: Dienstbenachrichtigungen
description: "Dieses Handbuch beschreibt, wie ein Android-Dienst lokale Benachrichtigungen verwenden kann, dem Versand von Informationen für einen Benutzer."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a5309b46b67a79225611aafb546b35e73d891b38
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="service-notifications"></a>Dienstbenachrichtigungen

_Dieses Handbuch beschreibt, wie ein Android-Dienst lokale Benachrichtigungen verwenden kann, dem Versand von Informationen für einen Benutzer._


## <a name="service-notifications-overview"></a>Übersicht über den Dienst Benachrichtigungen

Dienstbenachrichtigungen erlauben eine app zum Anzeigen von Informationen für den Benutzer an, auch wenn die Android-Anwendung nicht im Vordergrund ist. Es ist möglich, dass eine Benachrichtigung, um Aktionen für den Benutzer, z. B. das Anzeigen einer Aktivität aus einer Anwendung bereitgestellt werden. Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienst eine Benachrichtigung an einen Benutzer verteilen kann:

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

Diese Abbildung ist ein Beispiel der Benachrichtigung, die angezeigt wird:

[![Benachrichtigungssymbol in der Statusleiste angezeigt](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png)

Wenn Benutzer Folien, die Benachrichtigung-Bildschirm von oben nach unten, wird die vollständige Benachrichtigung angezeigt:

![Notication im Benachrichtigungsbereich angezeigt](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Aktualisieren eine Benachrichtigung

Um eine Benachrichtigung zu aktualisieren, wird der Dienst die Benachrichtigung wird über die gleiche benachrichtigungs-ID erneut veröffentlichen Android anzeigt oder die Benachrichtigung in der Statusleiste nach Bedarf aktualisieren.

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

Weitere Informationen zu Benachrichtigungen finden Sie in der [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md) Teil der [Android-Benachrichtigungen](~/android/app-fundamentals/notifications/index.md) Handbuch.


## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen in Android](~/android/app-fundamentals/notifications/local-notifications.md)
