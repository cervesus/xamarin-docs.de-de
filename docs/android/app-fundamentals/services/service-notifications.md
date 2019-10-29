---
title: Dienstbenachrichtigungen
description: In diesem Handbuch wird erläutert, wie ein Android-Dienst lokale Benachrichtigungen verwenden kann, um Informationen an einen Benutzer zu senden.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: b02785863f89ef6a273c52c09f45a99c17cb6242
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024531"
---
# <a name="service-notifications"></a>Dienstbenachrichtigungen

_In diesem Handbuch wird erläutert, wie ein Android-Dienst lokale Benachrichtigungen verwenden kann, um Informationen an einen Benutzer zu senden._

## <a name="service-notifications-overview"></a>Übersicht über Dienst Benachrichtigungen

Dienst Benachrichtigungen ermöglichen es einer APP, Informationen für den Benutzer anzuzeigen, auch wenn sich die Android-Anwendung nicht im Vordergrund befindet. Es ist möglich, dass eine Benachrichtigung Aktionen für den Benutzer bereitstellt, z. b. das Anzeigen einer Aktivität aus einer Anwendung. Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienst eine Benachrichtigung an einen Benutzer senden kann:

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

Dieser Screenshot zeigt ein Beispiel für die Benachrichtigung, die angezeigt wird:

[![Benachrichtigungssymbol, das in der Statusleiste angezeigt wird](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Wenn der Benutzer den Benachrichtigungs Bildschirm von oben nach unten bewegt, wird die vollständige Benachrichtigung angezeigt:

![Benachrichtigung in Benachrichtigungsleiste](service-notifications-images/02-fullnotification.png)

## <a name="updating-a-notification"></a>Aktualisieren einer Benachrichtigung

Zum Aktualisieren einer Benachrichtigung wird die Benachrichtigung vom Dienst mit der gleichen Benachrichtigungs-ID erneut veröffentlicht. Android wird die Benachrichtigung in der Statusleiste nach Bedarf anzeigen oder aktualisieren.

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

Weitere Informationen zu Benachrichtigungen finden Sie im [Android-Benachrichtigungs](~/android/app-fundamentals/notifications/index.md) Handbuch im Abschnitt [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md) .

## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen in Android](~/android/app-fundamentals/notifications/local-notifications.md)
