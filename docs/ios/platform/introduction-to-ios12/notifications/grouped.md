---
title: Gruppierte Benachrichtigungen in Xamarin.iOS
description: In iOS 12 ist es möglich, Gruppe Benachrichtigungen im Notification Center oder dem Sperrbildschirm von Anwendung oder vom Thread. Dieses Dokument beschreibt, wie Threads gesendet und nicht verketteten Benachrichtigungen mit Xamarin.iOS.
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 278986b29e629995a202f474242670f5524c45ff
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111523"
---
# <a name="grouped-notifications-in-xamarinios"></a>Gruppierte Benachrichtigungen in Xamarin.iOS

Standardmäßig platziert iOS 12 alle von der app-Benachrichtigungen in einer Gruppe. Der Sperrbildschirm und Notification Center können Sie dieser Gruppe als einen Stapel mit der letzten Benachrichtigung im Vordergrund anzeigen. Benutzer können die Gruppe "" finden alle Benachrichtigungen, die es enthält, und schließen die Gruppe als Ganzes erweitern.

Apps können auch Benachrichtigungen für Gruppe von Thread, erleichtert es dem Benutzer suchen, und interagieren mit der bestimmte Informationen, die, der Sie interessiert sind.

## <a name="sample-app-groupednotifications"></a>Beispiel-app: GroupedNotifications

Informationen zum gruppierte Benachrichtigungen mit Xamarin.iOS verwenden, sehen Sie sich die [GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications) Beispiel-app.

Diese Beispiel-app simuliert Konversationen mit verschiedenen Freunde senden einer Benachrichtigung für jede Nachricht, und gruppieren sie nach Thread. Darüber hinaus wird veranschaulicht, wie nicht verketteten Flächen, die Benachrichtigungen in einer Gruppe auf Anwendungsebene.

Codeausschnitte in diesem Handbuch stammen aus dieser Beispiel-app.

## <a name="request-authorization-and-allow-foreground-notifications"></a>Autorisierung anfordern und Foreground-Benachrichtigungen zulassen

Um eine app lokale Benachrichtigungen senden zu kann, müssen sie die Berechtigung anfordern. In der Beispiel-app [ `AppDelegate` ](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/), [ `FinishedLaunching` ](https://developer.xamarin.com/api/member/UIKit.UIApplicationDelegate.FinishedLaunching/p/UIKit.UIApplication/Foundation.NSDictionary/) Methode fordert diese Berechtigung:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    center.RequestAuthorization(UNAuthorizationOptions.Alert, (bool success, NSError error) =>
    {
        // Set the Delegate regardless of success; users can modify their notification
        // preferences at any time in the Settings app.
        center.Delegate = this;
    });
    return true;
}
```

Die [ `Delegate` ](https://developer.xamarin.com/api/property/UserNotifications.UNUserNotificationCenter.Delegate/) (Satz oben) für eine [ `UNUserNotificationCenter` ](https://developer.xamarin.com/api/type/UserNotifications.UNUserNotificationCenter/) entscheidet, ob eine eingehende Benachrichtigung durch den Aufruf des abschlusshandlers ,dieaneineVordergrund-appangezeigtwerdensollodernicht[`WillPresentNotification`](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification/p/UserNotifications.IUNUserNotificationCenterDelegate/UserNotifications.UNUserNotificationCenter/UserNotifications.UNNotification/System.Action%7BUserNotifications.UNNotificationPresentationOptions%7D/):

```csharp
[Export("userNotificationCenter:willPresentotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

Die [ `UNNotificationPresentationOptions.Alert` ](https://developer.xamarin.com/api/type/UserNotifications.UNNotificationPresentationOptions/) Parameter gibt an, dass die app soll Warnung anzeigen, aber keines Sounds wiedergeben oder einen Badge zu aktualisieren.

## <a name="threaded-notifications"></a>Singlethread-Benachrichtigungen

Tippen Sie auf der Beispiel-app **Nachricht mit Alice** Schaltfläche wiederholt, um Benachrichtigungen für eine Konversation mit einem Freund namens Alice zu senden.
Da diese Konversation, die Benachrichtigungen im gleichen Thread gehören, Gruppieren der Sperrbildschirm und Mitteilungszentrale zu.

Um eine Konversation mit einem anderen Freund zu starten, tippen Sie auf die **wählen Sie einen neuen Freund** Schaltfläche. Benachrichtigungen für diese Konversation, die in eine separate Gruppe angezeigt werden.

### <a name="threadidentifier"></a>ThreadIdentifier

Jedes Mal die Beispiel-app einen neuen Thread und startet, erstellt es eine eindeutige Thread-ID:

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

Um eine Singlethread-Benachrichtigung, die Beispiel-app zu senden:

- Überprüft, ob die app die Autorisierung zum Senden einer Benachrichtigung.
- Erstellt ein [`UNMutableNotificationContent`](https://developer.xamarin.com/api/type/UserNotifications.UNMutableNotificationContent/)
-Objekt für die Benachrichtigung, den Inhalt, und legt sie fest seine [`ThreadIdentifier`](https://developer.xamarin.com/api/property/UserNotifications.UNMutableNotificationContent.ThreadIdentifier/)
der Thread-ID oben erstellt haben.
- Erstellt eine Anforderung, und plant die Benachrichtigung:

```csharp
async partial void ScheduleThreadedNotification(UIButton sender)
{
    var center = UNUserNotificationCenter.Current;

    UNNotificationSettings settings = await center.GetNotificationSettingsAsync();
    if (settings.AuthorizationStatus != UNAuthorizationStatus.Authorized)
    {
        return;
    }

    string author =  // ...
    string message = // ...

    var content = new UNMutableNotificationContent()
    {
        ThreadIdentifier = threadId,
        Title = author,
        Body = message,
        SummaryArgument = author
    };

    var request = UNNotificationRequest.FromIdentifier(
        Guid.NewGuid().ToString(),
        content,
        UNTimeIntervalNotificationTrigger.CreateTrigger(1, false)
    );

    center.AddNotificationRequest(request, null);

    // ...
}
```

Alle Benachrichtigungen von der gleichen app mit derselben Thread-ID werden in der gleichen Benachrichtigungsgruppe angezeigt.

> [!NOTE]
> Um eine Thread-ID für eine remotebenachrichtigung festzulegen, fügen die `thread-id` um die JSON-Nutzlast der Benachrichtigung. Finden Sie unter Apple [generieren eine Remotebenachrichtigung](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) Dokument Weitere Informationen.

### <a name="summaryargument"></a>SummaryArgument

`SummaryArgument` Gibt an, wie eine Benachrichtigung den Zusammenfassungstext auswirken, der auf der linken unteren Ecke der einer Benachrichtigungsgruppe angezeigt wird, der die Benachrichtigung angehört. iOS aggregiert Zusammenfassungstext von Benachrichtigungen in der gleichen Gruppe um eine zusammenfassende Beschreibung zu erstellen.

Die Beispiel-app wird der Nachricht Autor als Zusammenfassung Argument verwendet. Mit diesem Ansatz ist für eine Gruppe von sechs Benachrichtigungen mit Alice der Zusammenfassungstext möglicherweise **5 weitere Benachrichtigungen von Alice und mich**.

## <a name="unthreaded-notifications"></a>Nicht verketteten Benachrichtigungen

Jede der Beispiel-app durch Tippen auf **Erinnerung Termin** Schaltfläche sendet eine der verschiedenen Termin Erinnerung Benachrichtigungen. Da diese Erinnerungen nicht verkettet werden, werden sie in der Benachrichtigungsgruppe auf Anwendungsebene auf dem Sperrbildschirm angezeigt und in der Mitteilungszentrale angezeigt.

Nicht verketteten Senden einer benachrichtigungs, die Beispiel-app die `ScheduleUnthreadedNotification` Methode verwendet ähnlichen Code wie oben beschrieben.
Allerdings ist es nicht festgelegt die `ThreadIdentifier` auf die `UNMutableNotificationContent` Objekt.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications)
- [Framework für Benutzerbenachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Neuerungen in Benutzerbenachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Verwenden von gruppierten Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [Bewährte Methoden und Neuigkeiten in Benutzerbenachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generieren eine Remotebenachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
