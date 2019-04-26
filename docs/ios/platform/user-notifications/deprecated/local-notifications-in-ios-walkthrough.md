---
title: 'Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in Xamarin.iOS'
description: In diesem Abschnitt begleiten wir erläutert, wie Sie lokale Benachrichtigungen in einer Xamarin.iOS-Anwendung verwenden. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine Benachrichtigung, die Sie eine Warnung, wenn von der app empfangen angezeigt wird.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 7589784563906d60fc8026feac9ea16362463bfa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085106"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in Xamarin.iOS

_In diesem Abschnitt begleiten wir erläutert, wie Sie lokale Benachrichtigungen in einer Xamarin.iOS-Anwendung verwenden. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine Benachrichtigung, die Sie eine Warnung, wenn von der app empfangen angezeigt wird._

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt beziehen sich auf iOS 9 und vorherigen, es wurde keine hier zur Unterstützung von älterer iOS-Versionen. IOS 10 und höher, finden Sie unter den [Handbuch für die Benutzer-Notification-Framework](~/ios/platform/user-notifications/index.md) für die Unterstützung von sowohl lokale als auch Remote-Benachrichtigung auf iOS-Geräten.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Lassen Sie eine einfache Anwendung erstellen, die anzeigt, lokale Benachrichtigungen in Aktion. Diese Anwendung wird eine einzelne Schaltfläche sein. Wenn wir auf die Schaltfläche klicken, wird eine lokale Benachrichtigung erstellt. Nachdem das angegebene Zeitraum verstrichen ist, sehen wir die Benachrichtigung angezeigt werden.


1. In Visual Studio für Mac erstellen eine neue Einzelansicht-iOS-Projektmappe, und nennen sie `Notifications`.
1. Öffnen der `Main.storyboard` Datei, und ziehen Sie eine Schaltfläche in der Ansicht. Benennen Sie die Schaltfläche **Schaltfläche**, und geben sie den Titel **Benachrichtigung hinzufügen**. Sie können auch einige festlegen möchten [Einschränkungen](~/ios/user-interface/designer/designer-auto-layout.md) auf die Schaltfläche an diesem Punkt: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Einige Einschränkungen auf die Schaltfläche festlegen")
1. Bearbeiten der `ViewController` Klasse und die ViewDidLoad-Methode den folgenden Ereignishandler hinzu:

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

    Dieser Code wird eine Benachrichtigung erstellt, die einen Sound verwendet, wird der Wert für den Badge Symbol auf 1 und dem Benutzer eine Warnung angezeigt.

1. Als Nächstes bearbeiten Sie die Datei `AppDelegate.cs`, zunächst den folgenden Code zum Hinzufügen der `FinishedLaunching` Methode. Wir haben überprüft, um festzustellen, ob das Gerät mit iOS 8 ausgeführt wird, wenn Sie also werden **erforderlichen** Fragen für die Berechtigung des Benutzers, Benachrichtigungen zu empfangen:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. Nach wie vor in `AppDelegate.cs`, fügen Sie die folgende Methode aufgerufen wird, wenn eine Benachrichtigung empfangen wird:

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

1. Wir müssen, um den Fall abzudecken, in dem die Benachrichtigung aufgrund eine lokale Benachrichtigung gestartet wurde. Bearbeiten Sie die Methode `FinishedLaunching` in die `AppDelegate` sollen den folgenden Codeausschnitt:


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

1. Führen Sie schließlich die Anwendung ein. Unter iOS werden 8 Sie werden aufgefordert, um Benachrichtigungen zu ermöglichen. Klicken Sie auf **OK** , und klicken Sie dann auf die **-hinzufügungsbenachrichtigung** Schaltfläche. Nach einer kurzen Pause sehen Sie das Dialogfeld "Warnung", wie in den folgenden Screenshots gezeigt:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Bestätigen die Möglichkeit zum Senden von Benachrichtigungen") ![](local-notifications-in-ios-walkthrough-images/image1.png "der hinzufügen-Notification-Schaltfläche") ![](local-notifications-in-ios-walkthrough-images/image2.png "im Warndialogfeld Benachrichtigung")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie der verschiedenen APIs verwenden Sie zum Erstellen und Veröffentlichen von Benachrichtigungen unter iOS. Es wurde auch veranschaulicht, wie das Symbol der Anwendung mit einen Badge Anwendung bestimmte Feedback zu geben, an dem Benutzer zu aktualisieren.


## <a name="related-links"></a>Verwandte Links

- [Lokale Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokalen und Push-Benachrichtigungen-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
