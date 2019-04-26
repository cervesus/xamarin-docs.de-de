---
title: Benachrichtigungen in Xamarin.iOS
description: In diesem Abschnitt veranschaulicht das Implementieren von lokalen Benachrichtigungen in Xamarin.iOS. Es wird erläutert, die UI-Elemente von einer iOS-Benachrichtigung und diskutieren Sie die API des mit dem Erstellen und Anzeigen einer Benachrichtigung beteiligt.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/13/2018
ms.openlocfilehash: f31490a683adfb46c609f14adf08b68de0abc375
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082985"
---
# <a name="notifications-in-xamarinios"></a>Benachrichtigungen in Xamarin.iOS

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt bezieht sich auf iOS 9 und vor. IOS 10 und höher, finden Sie unter den [Benachrichtigung Framework-Benutzerhandbuch](~/ios/platform/user-notifications/index.md).

iOS enthält drei Möglichkeiten, um zu verdeutlichen, dass eine Benachrichtigung empfangen wurde:

- **Aussprache oder Vibration** -iOS kann Wiedergeben eines Sounds aus, um Benutzer zu informieren. Wenn der Sound deaktiviert ist, kann das Gerät für die Vibrieren konfiguriert werden.
- **Warnungen** – es ist möglich, ein Dialogfeld angezeigt, auf dem Bildschirm mit Informationen zur Benachrichtigung.
- **Badges** – Wenn eine Benachrichtigung veröffentlicht wird, eine Zahl (Stellen Sie ihn) auf das Symbol der Anwendung angezeigt werden kann.

iOS bietet auch eine *Mitteilungszentrale* werden, die alle Benachrichtigungen, lokalen und Remotecomputern, für den Benutzer angezeigt. Benutzer können dies durch Wischen vom oberen Bildschirmrand nach unten zugreifen:

![Die Mitteilungszentrale](local-notifications-in-ios-images/image13.png "Notification Center")

## <a name="creating-local-notifications-in-ios"></a>Erstellen von lokalen Benachrichtigungen unter iOS

iOS ist es recht einfach zu erstellen und Behandeln von lokalen Benachrichtigungen.
Zunächst erfordert iOS 8-Anwendungen des Benutzers nötige Berechtigung zum Anzeigen von Benachrichtigungen zu erhalten. Fügen Sie den folgenden Code zu Ihrer app, bevor Sie versuchen, eine lokale Benachrichtigung zu senden: die [angefügte Beispiel](https://developer.xamarin.com/samples/monotouch/LocalNotifications/) platziert es in der **AppDelegate**des **FinishedLaunching** Methode.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![Bestätigen die Möglichkeit, eine lokale Benachrichtigung senden](local-notifications-in-ios-images/image0-sml.png "bestätigen die Möglichkeit, eine lokale Benachrichtigung senden")](local-notifications-in-ios-images/image0.png#lightbox)

Um eine lokale Benachrichtigung zu planen, erstellen eine `UILocalNotification` -Objekts die `FireDate`, und Planen Sie ihn über die `ScheduleLocalNotification` Methode für die `UIApplication.SharedApplication` Objekt. Der folgende Codeausschnitt zeigt, wie eine Benachrichtigung zu planen, die in der Zukunft eine Minute ausgelöst, und eine Warnung mit einer Meldung angezeigt wird:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Der folgende Screenshot zeigt, wie diese Warnung aussieht:

[![](local-notifications-in-ios-images/image2-sml.png "Ein Beispiel für Warnung")](local-notifications-in-ios-images/image2.png#lightbox)

Beachten Sie, dass, wenn der Benutzer auf *nicht zu, dass* Benachrichtigungen wird nichts angezeigt wird.

Wenn Sie das Symbol der Anwendung mit einer Zahl einen Badge zuweisen möchten, können Sie es wie in der folgenden Codezeile dargestellt festlegen:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

In der Reihenfolge wiedergeben legen ein Sounds mit dem Symbol, Sie die SoundName Eigenschaft auf die Benachrichtigung wie im folgenden Codeausschnitt gezeigt:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Wenn der Sound Benachrichtigung länger als 30 Sekunden ist, werden iOS standardmäßig sound stattdessen wiedergegeben.

> [!IMPORTANT]
> Es ist ein Fehler im iOS-Simulator, der zwei Mal die Delegat-Benachrichtigung ausgelöst werden. Dieses Problem sollte nicht auftreten, wenn die Anwendung auf einem Gerät ausgeführt wird.

## <a name="handling-notifications"></a>Behandeln von Benachrichtigungen

iOS-Anwendungen verarbeiten Remote- und lokale Benachrichtigungen in fast genau die gleiche Art und Weise. Wenn eine Anwendung ausgeführt wird, die `ReceivedLocalNotification` Methode oder der `ReceivedRemoteNotification` Methode für die `AppDelegate` Klasse hat den Namen und die Benachrichtigungsinformationen wird als Parameter übergeben werden.

Eine Anwendung kann eine Benachrichtigung auf unterschiedliche Weise verarbeiten. Z. B. möglicherweise die Anwendung nur eine Warnung, um Benutzer über ein Ereignis zu erinnern angezeigt. Oder die Benachrichtigung kann verwendet werden, um eine Warnung, die dem Benutzer angezeigt wird, die ein Prozess, wie z. B. die Synchronisierung von Dateien auf einem Server abgeschlossen ist.

Der folgende Code zeigt, wie Sie eine lokale Benachrichtigung verarbeitet und eine Warnung angezeigt, und die Badge-Anzahl auf 0 (null) zurückgesetzt wird:

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

Wenn die Anwendung nicht ausgeführt wird, wird iOS der Klang wiedergegeben und/oder den Badge "Symbol" nach Bedarf zu aktualisieren. Die Anwendung wird gestartet, wenn der Benutzer die Verbindung mit der Anwendung gestartet wird, und die `FinishedLaunching` Methode für den app-Delegaten aufgerufen, und die Benachrichtigungsinformationen übergeben wird über die `launchOptions` Parameter. Wenn das Wörterbuch für die Optionen für den Schlüssel enthält `UIApplication.LaunchOptionsLocalNotificationKey`, und klicken Sie dann die `AppDelegate` weiß, dass die Anwendung über eine lokale Benachrichtigung gestartet wurde. Der folgende Codeausschnitt zeigt diesen Vorgang:

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

Für eine remote-Benachrichtigung `launchOptions` müssen eine `LaunchOptionsRemoteNotificationKey` mit einer zugeordneten `NSDictionary` , die die remote benachrichtigungsnutzlast enthält. Sie können die Nutzlast der Benachrichtigung über Extrahieren der `alert`, `badge`, und `sound` Schlüssel. Der folgende Codeausschnitt zeigt, wie um remotebenachrichtigungen zu erhalten:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt wurde gezeigt, wie erstellen und veröffentlichen eine Benachrichtigung in Xamarin.iOS. Sie zeigen, wie eine Anwendung durch das Überschreiben von Benachrichtigungen reagieren kann die `ReceivedLocalNotification` Methode oder der `ReceivedRemoteNotification` -Methode in der die `AppDelegate`.

## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokale und Pushbenachrichtigungen für Entwickler](https://developer.apple.com/notifications/)
- [Lokalen und Push Notification Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
