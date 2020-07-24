---
title: 'Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in xamarin. IOS'
description: In diesem Abschnitt wird erläutert, wie lokale Benachrichtigungen in einer xamarin. IOS-Anwendung verwendet werden. Es veranschaulicht die Grundlagen der Erstellung und Veröffentlichung einer Benachrichtigung, mit der eine Warnung angezeigt wird, wenn Sie von der APP empfangen wird.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: b0a45207ba036f73c2d1066ea292a02ebcc45064
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934627"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in xamarin. IOS

_In diesem Abschnitt wird erläutert, wie lokale Benachrichtigungen in einer xamarin. IOS-Anwendung verwendet werden. Es veranschaulicht die Grundlagen der Erstellung und Veröffentlichung einer Benachrichtigung, mit der eine Warnung angezeigt wird, wenn Sie von der APP empfangen wird._

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt beziehen sich auf IOS 9 und ältere Versionen, die Sie für ältere IOS-Versionen unterstützen. Informationen zu IOS 10 und höher finden Sie im [Handbuch zum Benutzer Benachrichtigungs Framework](~/ios/platform/user-notifications/index.md) , das sowohl lokale als auch Remote Benachrichtigungen auf einem IOS-Gerät unterstützt.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Erstellen Sie eine einfache Anwendung, in der lokale Benachrichtigungen in Aktion angezeigt werden. Diese Anwendung verfügt über eine einzelne Schaltfläche. Wenn Sie auf die Schaltfläche klicken, wird eine lokale Benachrichtigung erstellt. Nachdem der angegebene Zeitraum abgelaufen ist, wird die Benachrichtigung angezeigt.

1. Erstellen Sie in Visual Studio für Mac eine neue Einzelansicht-IOS-Lösung, und rufen Sie Sie auf `Notifications` .
1. Öffnen Sie die `Main.storyboard` Datei, und ziehen Sie eine Schaltfläche auf die Ansicht. Benennen Sie die Schaltflächen **Schaltfläche**, und geben Sie Ihr den Titel **Benachrichtigung hinzufügen**. Sie können auch einige [Einschränkungen](~/ios/user-interface/designer/designer-auto-layout.md) für die Schaltfläche an diesem Punkt festlegen: 

    ![Festlegen einiger Einschränkungen auf der Schaltfläche](local-notifications-in-ios-walkthrough-images/image3.png)
1. Bearbeiten `ViewController` Sie die-Klasse, und fügen Sie der viewDidLoad-Methode den folgenden Ereignishandler hinzu:

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    Mit diesem Code wird eine Benachrichtigung erstellt, die einen Sound verwendet, den Wert des Symbol Badge auf 1 festlegt und eine Warnung für den Benutzer anzeigt.

1. Als nächstes Bearbeiten Sie die Datei `AppDelegate.cs` , und fügen Sie zuerst der-Methode den folgenden Code hinzu `FinishedLaunching` . Wir haben geprüft, ob auf dem Gerät IOS 8 ausgeführt wird. wenn dies der Fall ist **, müssen wir** die Berechtigung des Benutzers anfordern, Benachrichtigungen zu empfangen:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
        var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
            UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
        );

        application.RegisterUserNotificationSettings (notificationSettings);
    }
    ```

1. `AppDelegate.cs`Fügen Sie in noch die folgende Methode hinzu, die aufgerufen wird, wenn eine Benachrichtigung empfangen wird:

    ```csharp
    public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
    {
        // show an alert
        UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
    ```

1. Der Fall, in dem die Benachrichtigung aufgrund einer lokalen Benachrichtigung gestartet wurde, muss behandelt werden. Bearbeiten Sie die-Methode `FinishedLaunching` in der `AppDelegate` , um den folgenden Code Ausschnitt einzuschließen:

    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
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
    }
    ```

1. Führen Sie schließlich die Anwendung aus. Unter IOS 8 werden Sie aufgefordert, Benachrichtigungen zuzulassen. Klicken Sie auf **OK** und dann auf die Schaltfläche **Benachrichtigung hinzufügen** . Nach einer kurzen Pause sollte das Dialogfeld "Warnung" angezeigt werden, wie in den folgenden Screenshots zu sehen:

    ![Bestätigung der Möglichkeit zum Senden von Benachrichtigungen über die ](local-notifications-in-ios-walkthrough-images/image0.png) ![ Schaltfläche Benachrichtigung hinzufügen ](local-notifications-in-ios-walkthrough-images/image1.png) ![ im Dialogfeld Benachrichtigungs Warnung](local-notifications-in-ios-walkthrough-images/image2.png)

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie die verschiedenen APIs zum Erstellen und Veröffentlichen von Benachrichtigungen in ios verwendet werden. Außerdem wurde veranschaulicht, wie Sie das Anwendungssymbol mit einem Badge aktualisieren, um dem Benutzer einige anwendungsspezifische Feedback zu geben.

## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [Programmier Handbuch für lokale und Pushbenachrichtigungen](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
