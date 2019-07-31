---
title: Gruppierte Benachrichtigungen in xamarin. IOS
description: Mit IOS 12 ist es möglich, Benachrichtigungen im Benachrichtigungs Center oder auf dem Sperrbildschirm nach Anwendung oder Thread zu gruppieren. In diesem Dokument wird beschrieben, wie mit xamarin. IOS Thread-und unthreaded-Benachrichtigungen gesendet werden.
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 3edaabe287bc2b37d2ec5a759ada9f59441c6d3a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652575"
---
# <a name="grouped-notifications-in-xamarinios"></a>Gruppierte Benachrichtigungen in xamarin. IOS

Standardmäßig platziert IOS 12 alle Benachrichtigungen einer APP in einer Gruppe. Der Sperrbildschirm und das Benachrichtigungs Center zeigen diese Gruppe als Stapel mit der neuesten Benachrichtigung an. Benutzer können die Gruppe erweitern, um alle darin enthaltenen Benachrichtigungen anzuzeigen und die Gruppe als Ganzes zu verwerfen.

Apps können auch Benachrichtigungen nach Thread gruppieren, sodass Benutzer die gewünschten Informationen leichter finden und mit ihnen interagieren können.

## <a name="sample-app-groupednotifications"></a>Beispiel-App: Groupedbenachrichtigungen

Weitere Informationen zum Verwenden von gruppierten Benachrichtigungen mit xamarin. IOS finden Sie in der Beispiel-APP [groupednotification](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-groupednotifications) .

Diese Beispiel-App simuliert Konversationen mit verschiedenen Freunden, sendet eine Benachrichtigung für jede Nachricht und gruppiert Sie nach Thread. Außerdem wird veranschaulicht, wie unthread-Benachrichtigungen in einer Gruppe auf Anwendungsebene landen.

Code Ausschnitte in dieser Anleitung stammen aus dieser Beispiel-app.

## <a name="request-authorization-and-allow-foreground-notifications"></a>Autorisierung anfordern und Vordergrund Benachrichtigungen zulassen

Bevor eine APP lokale Benachrichtigungen senden kann, muss Sie eine entsprechende Berechtigung anfordern. In der Beispiel-APP [`AppDelegate`](xref:UIKit.UIApplicationDelegate)fordert die [`FinishedLaunching`](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary)) -Methode diese Berechtigung an:

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

Der [`Delegate`](xref:UserNotifications.UNUserNotificationCenter.Delegate) (oben festgelegte) für [`UNUserNotificationCenter`](xref:UserNotifications.UNUserNotificationCenter) einen entscheidet, ob eine Vordergrund-App eine eingehende Benachrichtigung anzeigen soll, indem der an [`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions}))weiter gegebene Abschluss Handler aufgerufen wird:

```csharp
[Export("userNotificationCenter:willPresentotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

Der [`UNNotificationPresentationOptions.Alert`](xref:UserNotifications.UNNotificationPresentationOptions) -Parameter gibt an, dass die APP die Warnung anzeigen soll, aber keinen Sound abspielen oder einen Badge aktualisieren soll.

## <a name="threaded-notifications"></a>Thread Benachrichtigungen

Tippen Sie wiederholt auf die Meldung der Beispiel-APP **mit der Alice** -Schaltfläche, damit Benachrichtigungen für eine Konversation mit dem Namen Alice gesendet werden.
Da die Benachrichtigungen dieser Konversation Teil desselben Threads sind, Gruppieren Sie den Sperrbildschirm und das Benachrichtigungs Center zusammen.

Um eine Konversation mit einem anderen Freund zu starten, tippen Sie auf die Schaltfläche **neuen Freund auswählen** . Benachrichtigungen für diese Konversation werden in einer separaten Gruppe angezeigt.

### <a name="threadidentifier"></a>Threadidentifier

Jedes Mal, wenn die Beispiel-APP einen neuen Thread startet, wird ein eindeutiger Thread Bezeichner erstellt:

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

Zum Senden einer Thread Benachrichtigung wird in der Beispiel-App Folgendes angezeigt:

- Überprüft, ob die APP über eine Autorisierung zum Senden einer Benachrichtigung verfügt.
- Erstellt eine[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
Objekt für den Inhalt der Benachrichtigung und legt seine[`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier)
an den oben erstellten Thread Bezeichner.
- Erstellt eine Anforderung und plant die Benachrichtigung:

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

Alle Benachrichtigungen aus derselben App mit dem gleichen Thread Bezeichner werden in derselben Benachrichtigungs Gruppe angezeigt.

> [!NOTE]
> Fügen Sie der JSON-Nutzlast der Benachrichtigung den `thread-id` Schlüssel hinzu, um einen Thread Bezeichner für eine Remote Benachrichtigung festzulegen. Weitere Informationen finden Sie in der Apple-Dokumentation zum [Erstellen eines Remote Benachrichtigungs](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) Dokuments.

### <a name="summaryargument"></a>SummaryArgument

`SummaryArgument`Gibt an, wie sich eine Benachrichtigung auf den Übersichts Text auswirkt, der in der unteren linken Ecke einer Benachrichtigungs Gruppe angezeigt wird, zu der die Benachrichtigung gehört. IOS aggregiert einen Zusammenfassungs Text aus Benachrichtigungen in derselben Gruppe, um eine allgemeine Übersichts Beschreibung zu erstellen.

In der Beispiel-APP wird der Autor der Nachricht als Zusammenfassungs Argument verwendet. Bei diesem Ansatz kann der Zusammenfassungs Text für eine Gruppe von sechs Benachrichtigungen mit Alice **5 weitere Benachrichtigungen von Alice und mir**sein.

## <a name="unthreaded-notifications"></a>Nicht Thread Benachrichtigungen

Jede Tap-Schaltfläche zur **Termin Erinnerung** der Beispiel-App sendet eine der verschiedenen Benachrichtigungen zur Termin Erinnerung. Da diese Erinnerungen nicht Thread abhängig sind, werden Sie auf dem Sperrbildschirm und im Benachrichtigungs Center in der Benachrichtigungs Gruppe auf Anwendungsebene angezeigt.

Um eine unthread Benachrichtigung zu senden, verwendet die- `ScheduleUnthreadedNotification` Methode der Beispiel-APP einen ähnlichen Code wie oben.
Es wird jedoch nicht `ThreadIdentifier` für das `UNMutableNotificationContent` Objekt festgelegt.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App – groupednotification](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-groupednotifications)
- [Benutzer Benachrichtigungs Framework in xamarin. IOS](~/ios/platform/user-notifications/index.md)
- [Neues in Benutzer Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Verwenden von gruppierten Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [Bewährte Methoden und Neuerungen bei Benutzer Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Erstellen einer Remote Benachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
