---
title: Benachrichtigungen in xamarin. IOS
description: In diesem Abschnitt wird gezeigt, wie lokale Benachrichtigungen in xamarin. IOS implementiert werden. Darin werden die verschiedenen Benutzeroberflächen Elemente einer IOS-Benachrichtigung erläutert, und es wird erläutert, welche API zum Erstellen und Anzeigen einer Benachrichtigung beteiligt ist.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: eb8956e44a3148d79fff59d728cb63818b9bab60
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292751"
---
# <a name="notifications-in-xamarinios"></a>Benachrichtigungen in xamarin. IOS

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt beziehen sich auf IOS 9 und früher. Informationen zu IOS 10 und höher finden Sie im [Handbuch zum Benutzer Benachrichtigungs Framework](~/ios/platform/user-notifications/index.md).

IOS bietet drei Möglichkeiten, um dem Benutzer mitzuteilen, dass eine Benachrichtigung empfangen wurde:

- **Sound oder Vibration** : IOS kann einen Sound abspielen, um Benutzer zu benachrichtigen. Wenn der Sound deaktiviert ist, kann das Gerät so konfiguriert werden, dass es vibrieren kann.
- **Warnungen** : auf dem Bildschirm kann ein Dialogfeld mit Informationen zur Benachrichtigung angezeigt werden.
- **Abzeichen** : Wenn eine Benachrichtigung veröffentlicht wird, kann eine Zahl auf dem Anwendungssymbol angezeigt (badelt) werden.

IOS bietet auch ein *Benachrichtigungs Center* , in dem alle lokalen und Remote Benachrichtigungen für den Benutzer angezeigt werden. Benutzer können darauf zugreifen, indem Sie vom oberen Bildschirmrand nach unten:

![Das Benachrichtigungs Center](local-notifications-in-ios-images/image13.png "Das Benachrichtigungs Center")

## <a name="creating-local-notifications-in-ios"></a>Erstellen von lokalen Benachrichtigungen in ios

mit IOS ist es recht einfach, lokale Benachrichtigungen zu erstellen und zu verarbeiten.
Zuerst erfordert IOS 8, dass Anwendungen die Berechtigung des Benutzers zum Anzeigen von Benachrichtigungen anfordern. Fügen Sie der APP den folgenden Code hinzu, bevor Sie versuchen, eine lokale Benachrichtigung zu senden. die [angefügte Stichprobe](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications) platziert Sie in der **finishedstart** -Methode des **appdelegaten**.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![Bestätigung der Möglichkeit zum Senden einer lokalen Benachrichtigung](local-notifications-in-ios-images/image0-sml.png "Bestätigung der Möglichkeit zum Senden einer lokalen Benachrichtigung")](local-notifications-in-ios-images/image0.png#lightbox)

Um eine lokale Benachrichtigung zu planen, erstellen `UILocalNotification` Sie ein-Objekt `FireDate`, legen Sie fest, und `ScheduleLocalNotification` planen Sie es `UIApplication.SharedApplication` über die-Methode für das-Objekt. Der folgende Code Ausschnitt zeigt, wie Sie eine Benachrichtigung planen, die in der Zukunft eine Minute auslösen wird, und eine Warnung mit einer Meldung anzeigen:

```csharp
UILocalNotification notification = new UILocalNotification();
notification.FireDate = NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Der folgende Screenshot zeigt, wie diese Warnung aussieht:

[![](local-notifications-in-ios-images/image2-sml.png "Beispiel für eine Warnung")](local-notifications-in-ios-images/image2.png#lightbox)

Beachten Sie, dass nichts angezeigt wird, wenn der Benutzer Benachrichtigungen *nicht zulässt* .

Wenn Sie ein Badge auf das Anwendungssymbol mit einer Zahl anwenden möchten, können Sie es wie im folgenden Zeilen Code gezeigt festlegen:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

Wenn Sie einen Sound mit dem Symbol abspielen, legen Sie die Eigenschaft "Sound Name" für die Benachrichtigung fest, wie im folgenden Code Ausschnitt gezeigt:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Wenn der Benachrichtigungs Sound länger als 30 Sekunden ist, übernimmt IOS stattdessen den Standard Sound.

> [!IMPORTANT]
> Der IOS-Simulator weist einen Fehler auf, mit dem die delegatbenachrichtigung zweimal ausgelöst wird. Dieses Problem sollte nicht auftreten, wenn die Anwendung auf einem Gerät ausgeführt wird.

## <a name="handling-notifications"></a>Behandeln von Benachrichtigungen

IOS-Anwendungen verarbeiten Remote Benachrichtigungen und lokale Benachrichtigungen fast genauso. Wenn eine Anwendung ausgeführt wird, wird `ReceivedLocalNotification` die-Methode `ReceivedRemoteNotification` oder die- `AppDelegate` Methode der-Klasse aufgerufen, und die Benachrichtigungs Informationen werden als Parameter übergeben.

Eine Anwendung kann eine Benachrichtigung auf unterschiedliche Weise verarbeiten. Beispielsweise zeigt die Anwendung möglicherweise nur eine Warnung an, die Benutzer an ein Ereignis erinnert. Die Benachrichtigung kann auch verwendet werden, um dem Benutzer eine Warnung anzuzeigen, dass ein Prozess abgeschlossen wurde, z. b. das Synchronisieren von Dateien mit einem Server.

Der folgende Code zeigt, wie Sie eine lokale Benachrichtigung behandeln und eine Warnung anzeigen und die Badge-Nummer auf Null zurücksetzen:

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

Wenn die Anwendung nicht ausgeführt wird, übernimmt IOS den Sound und/oder aktualisiert das Symbol Badge nach Bedarf. Wenn der Benutzer die Anwendung startet, die der Warnung zugeordnet ist, wird die Anwendung gestartet, `FinishedLaunching` und die-Methode im App-Delegaten wird aufgerufen, und die Benachrichtigungs Informationen werden `launchOptions` über den-Parameter übergeben. Wenn das Options Wörterbuch den `UIApplication.LaunchOptionsLocalNotificationKey`Schlüssel enthält, `AppDelegate` weiß das, dass die Anwendung von einer lokalen Benachrichtigung aus gestartet wurde. Der folgende Code Ausschnitt veranschaulicht diesen Prozess:

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

Bei einer Remote Benachrichtigung verfügt `launchOptions` über einen `LaunchOptionsRemoteNotificationKey` mit einem zugeordneten `NSDictionary` , der die Remote Benachrichtigungs Nutzlast enthält. Sie können die Benachrichtigungs Nutzlast über die `alert`Schlüssel `badge`, und `sound` extrahieren. Der folgende Code Ausschnitt zeigt, wie Sie Remote Benachrichtigungen erhalten:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt wurde gezeigt, wie eine Benachrichtigung in xamarin. IOS erstellt und veröffentlicht wird. Es zeigt, wie eine Anwendung möglicherweise auf Benachrichtigungen reagiert, `ReceivedLocalNotification` indem Sie die `ReceivedRemoteNotification` -Methode oder `AppDelegate`die-Methode in der überschreibt.

## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [Lokale und Pushbenachrichtigungen für Entwickler](https://developer.apple.com/notifications/)
- [Programmier Handbuch für lokale und Pushbenachrichtigungen](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
