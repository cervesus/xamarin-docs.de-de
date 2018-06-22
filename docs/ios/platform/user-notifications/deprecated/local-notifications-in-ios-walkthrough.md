---
title: Exemplarische Vorgehensweise – mit lokalen Benachrichtigungen in Xamarin.iOS
description: In diesem Abschnitt werden wir durch Verwendung von lokalen Benachrichtigungen in einer Anwendung Xamarin.iOS geführt. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine Benachrichtigung, die Sie eine Warnung, wenn von der Anwendung empfangen eingeblendet wird.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: bb133d16f12249cbd31e4fce2b227162b4b28333
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776780"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Exemplarische Vorgehensweise – mit lokalen Benachrichtigungen in Xamarin.iOS

_In diesem Abschnitt werden wir durch Verwendung von lokalen Benachrichtigungen in einer Anwendung Xamarin.iOS geführt. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine Benachrichtigung, die Sie eine Warnung, wenn von der Anwendung empfangen eingeblendet wird._

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt beziehen sich auf iOS 9 und vorherigen, es ist noch hier zur Unterstützung von älterer iOS-Versionen. IOS 10 und höher, finden Sie unter der [Benachrichtigungsframeworks User Guide](~/ios/platform/user-notifications/index.md) für die Unterstützung von lokalen und Remote-Benachrichtigung auf einem iOS-Gerät.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Können Sie eine einfache Anwendung erstellen, die lokale Benachrichtigungen in Aktion angezeigt wird. Diese Anwendung wird ein einzelnes Optionsfeld verfügen. Wenn wir auf die Schaltfläche klicken, wird eine lokale Benachrichtigung erstellt. Nachdem die angegebene Zeitraum verstrichen ist, sehen wir die Benachrichtigung angezeigt werden.


1. Klicken Sie in Visual Studio für Mac erstellen Sie eine neue Ansicht iOS-Projektmappe, und rufen sie `Notifications`.
1. Öffnen der `Main.storyboard` Datei, und ziehen Sie eine Schaltfläche in der Ansicht. Benennen Sie die Schaltfläche **Schaltfläche**, und weisen Sie ihm den Titel **Benachrichtigung hinzufügen**. Sie können auch einige festlegen möchten [Einschränkungen](~/ios/user-interface/designer/designer-auto-layout.md) auf die Schaltfläche an diesem Punkt: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Einige Einschränkungen auf die Schaltfläche \"" festlegen")
1. Bearbeiten der `ViewController` Klasse, und fügen Sie den folgenden Ereignishandler für die ViewDidLoad-Methode:

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

    Mit diesem Code wird eine Benachrichtigung erstellt, die einen Sound verwendet, wird der Wert, der das Symbol "Signal auf 1 und dem Benutzer eine Warnung angezeigt.

1. Als Nächstes bearbeiten Sie die Datei `AppDelegate.cs`, zunächst den folgenden Code Hinzufügen der `FinishedLaunching` Methode. Wir haben überprüft, um festzustellen, ob das Gerät iOS 8 ausgeführt wird, wenn wir sind **erforderlichen** erfragen Sie die Berechtigung des Benutzers, um Benachrichtigungen zu erhalten:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. Noch im `AppDelegate.cs`, fügen Sie die folgende Methode aufgerufen wird, wenn eine Benachrichtigung empfangen wird:

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

1. Wir müssen, um den Fall abzudecken, in dem die Benachrichtigung aufgrund einer lokalen Benachrichtigung gestartet wurde. Bearbeiten Sie die Methode `FinishedLaunching` in der `AppDelegate` enthalten die folgenden Codeausschnitt:


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

1. Abschließend führen Sie die Anwendung an. Auf iOS werden 8 Sie aufgefordert, Benachrichtigungen ermöglichen. Klicken Sie auf **OK** , und klicken Sie dann auf die **Benachrichtigung hinzufügen** Schaltfläche. Nach einer kurzen Pause sollten Sie das Dialogfeld Warnung sehen, wie in den folgenden Screenshots dargestellt:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Bestätigen die Möglichkeit zum Senden von Benachrichtigungen") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "hinzufügen Benachrichtigung Schaltfläche") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "das Benachrichtigungsdialogfeld \"Warnung\"")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie der verschiedenen APIs zum Erstellen und Veröffentlichen von Benachrichtigungen in iOS verwenden. Es wird veranschaulicht, wie das Anwendungssymbol mit einem Badge Anwendung bestimmte Feedback zu geben, für dem Benutzer zu aktualisieren.


## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokale und Pushbenachrichtigungen Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
