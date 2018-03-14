---
title: Benachrichtigungen in Xamarin.iOS
description: "In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in Xamarin.iOS implementiert wird. Es wird erläutert, die verschiedenen Benutzeroberflächenelemente einer iOS-Benachrichtigung und diskutieren Sie die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 2a8ae55f9cc3e2dd4818dec96a35017c76cc9623
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="notifications-in-xamarinios"></a>Benachrichtigungen in Xamarin.iOS

_In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in Xamarin.iOS implementiert wird. Es wird erläutert, die verschiedenen Benutzeroberflächenelemente einer iOS-Benachrichtigung und diskutieren Sie die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt._

> [!IMPORTANT]
> **Hinweis:** die Informationen in diesem Abschnitt beziehen sich auf iOS 9 und vorherigen, es ist noch hier zur Unterstützung von älterer iOS-Versionen. IOS 10 und höher, finden Sie unter der [Benachrichtigungsframeworks User Guide](~/ios/platform/user-notifications/index.md) für die Unterstützung von lokalen und Remote-Benachrichtigung auf einem iOS-Gerät.

iOS verfügt über drei Möglichkeiten, um zu verdeutlichen, dass eine Benachrichtigung empfangen wurde:

-  **Sound oder Vibrationen** -iOS können Benutzer benachrichtigt werden, einen Sound wiedergeben. Wenn der Sound deaktiviert ist, kann das Gerät Vibrieren konfiguriert werden.
-  **Warnungen** -es ist möglich, ein Dialogfeld angezeigt, auf dem Bildschirm mit Informationen zur Benachrichtigung.
-  **Signale** – Wenn eine Benachrichtigung veröffentlicht wird, eine Zahl (Telefonliste), auf das Anwendungssymbol angezeigt werden kann.


iOS bietet auch eine *Mitteilungszentrale* , werden alle Benachrichtigungen, sowohl lokale als auch remote, für den Benutzer anzeigen. Benutzer können dies Streifen Sie vom oberen Rand des Bildschirms zugreifen:

 ![](local-notifications-in-ios-images/image13.png "Die Mitteilungszentrale")

## <a name="creating-local-notifications-in-ios"></a>Erstellen lokale Benachrichtigungen in iOS

iOS erleichtert recht einfach zu erstellen und Verarbeiten von lokalen Benachrichtigungen.
Zunächst erfordert iOS 8 Anwendungen für die Benutzer die Berechtigung zum Anzeigen von Benachrichtigungen um Unterstützung bitten. Fügen Sie den folgenden Code in Ihrer app, bevor Sie versuchen, eine lokale Benachrichtigung senden – die angefügte Beispiel platziert es in der **AppDelegate**des **FinishedLaunching** Methode.

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "Bestätigen die Möglichkeit, eine lokale Benachrichtigung senden")](local-notifications-in-ios-images/image0.png#lightbox)

Um eine lokale Benachrichtigung zu planen, Sie erstellen, eine `UILocalNotification` Objekt, das Festlegen der `FireDate`, und Planen Sie ihn über die `ScheduleLocalNotification` Methode auf die `UIApplication.SharedApplication` Objekt. Der folgende Codeausschnitt zeigt, wie eine Benachrichtigung zu planen, die ausgelöst werden in der Zukunft eine Minute, und eine Warnung mit einer Meldung angezeigt:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Der folgende Screenshot zeigen, wie diese Warnung aussieht:

  [![](local-notifications-in-ios-images/image2-sml.png "Eine Beispiel-Warnung")](local-notifications-in-ios-images/image2.png#lightbox)

Beachten Sie, dass, wenn der Benutzer auf *nicht zulassen* Benachrichtigungen wird nichts angezeigt.

Wenn Sie ein Signal auf das Anwendungssymbol mit einer Zahl anwenden möchten, können Sie es wie im folgenden Code der Zeile dargestellt festlegen:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

In der Reihenfolge wiedergeben legen ein Sounds mit dem Symbol, Sie die SoundName Eigenschaft auf die Benachrichtigung wie im folgenden Codeausschnitt gezeigt:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Pro Apple Human Richtlinien für die Benutzeroberfläche Wenn eine Benachrichtigung über einen Sound spielt er auch i. d. durch ein Signal oder eine Warnung, um die Hilfe des Benutzers die Anwendung zu identifizieren, die die Warnung ausgelöst. Wenn der Sound länger als 30 Sekunden ist, wird iOS standardmäßig sound stattdessen spielen auch.

> [!IMPORTANT]
> **Hinweis**: Es ist ein Fehler in der iOS-Simulator, die die Delegat Benachrichtigung zweimal ausgelöst werden. Dieses Problem sollte nicht auftreten, wenn die Anwendung auf einem Gerät ausführen.

## <a name="handling-notifications"></a>Handlingbenachrichtigungen

iOS-Anwendungen zu Remote- und lokale Benachrichtigungen in fast genau die gleiche Weise behandeln. Wenn eine Anwendung ausgeführt wird, die `ReceivedLocalNotification` Methode oder die `ReceivedRemoteNotification` Methode für die Klasse AppDelegate wird aufgerufen, und die Benachrichtigungsinformationen wird als Parameter übergeben werden.

Eine Anwendung kann eine Benachrichtigung auf unterschiedliche Weise behandeln. Die Anwendung kann z. B. nur eine Warnung aus, um Benutzer über ein Ereignis zu erinnern angezeigt. Oder die Benachrichtigung kann verwendet werden, um eine Warnung für den Benutzer anzuzeigen, die ein Prozess, z. B. synchronisiert Dateien auf einem Server abgeschlossen ist.

Der folgende Code zeigt, wie eine lokale Benachrichtigung behandeln und eine Warnung anzeigen, und die Badge-Anzahl auf 0 (null) zurückgesetzt wird:

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

Wenn die Anwendung nicht ausgeführt wird, werden iOS der Sound wiedergeben, und/oder aktualisieren das Symbol "Signal gegebenenfalls. Wenn der Benutzer die Anwendung mit der Warnung startet, wird beim Starten der Anwendung und die FinishedLaunching-Methode in der app-Delegat wird aufgerufen, und die Benachrichtigungsinformationen wird über die Options-Parameter übergeben werden. Wenn die Optionen Wörterbuch der Schlüssel enthält `UIApplication.LaunchOptionsLocalNotificationKey`, und klicken Sie dann die AppDelegate weiß, dass die Anwendung über eine lokale Benachrichtigung gestartet wurde. Der folgende Codeausschnitt veranschaulicht diesen Prozess:

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

Wird jedoch eine entfernte Benachrichtigung Wörterbuch Optionen stattdessen enthält eine `LaunchOptionsRemoteNotificationKey`, und der Ergebniswert für diesen Schlüssel wird ein `NSDictionary` Objekt mit der remote-Benachrichtigung-Nutzlast. Sie können die benachrichtigungsnutzlast über die Warnung, infoanzeigerupdates und sound Schlüssel extrahieren. Der folgende Codeausschnitt zeigt, wie remote Benachrichtigungen abgerufen wird:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt wurde gezeigt, wie erstellen und veröffentlichen eine Benachrichtigung in Xamarin.iOS. Es zeigen, wie eine Anwendung durch Überschreiben von Benachrichtigungen reagieren kann die `ReceivedLocalNotification` Methode oder die `ReceivedRemoteNotification` Methode in der `AppDelegate`.


## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokale und Pushbenachrichtigungen für Entwickler](https://developer.apple.com/notifications/)
- [Lokale und Push Notification-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
