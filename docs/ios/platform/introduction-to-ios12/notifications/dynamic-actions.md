---
title: Dynamische Notification-Aktionsschaltflächen in Xamarin.iOS
description: In iOS 12 eine Erweiterung für benachrichtigungsinhalte hinzufügen, entfernen und aktualisieren die Aktionsschaltflächen zur zusammen mit der eine Benachrichtigung angezeigt. In diesem Dokument wird beschrieben, wie dynamische Benachrichtigung Aktionsschaltflächen mit Xamarin.iOS verwendet wird.
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 5611d673ecc7af896fd3a9e566e184e408b6b367
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870091"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Dynamische Notification-Aktionsschaltflächen in Xamarin.iOS

In iOS 12 können Benachrichtigungen dynamisch hinzufügen, entfernen und aktualisieren die Schaltflächen für die zugeordnete Aktion. Eine solche Anpassung ermöglicht Benutzern bei Aktionen bereitstellen, die direkt an die benachrichtigungs Inhalten und der Interaktion des Benutzers, mit der Anwendung relevant sind.

## <a name="sample-app-redgreennotifications"></a>Beispiel-app: RedGreenNotifications

Die Codeausschnitte in diesem Handbuch stammen aus der [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) Beispiel-app, die veranschaulicht, wie Sie Xamarin.iOS zum Arbeiten mit Notification-Aktionsschaltflächen in iOS 12 zu verwenden.

Diese Beispiel-app sendet zwei Arten von lokalen Benachrichtigungen: Rot und Grün.
Nach der, dass die app eine Benachrichtigung senden, verwenden von 3D Touch auf die individuellen Benutzeroberfläche anzeigen. Anschließend verwenden Sie die benachrichtigungs Aktionsschaltflächen, um das Bild zu drehen, die angezeigt. Wie das Bild gedreht wird, eine **zurücksetzen Drehung** Schaltfläche angezeigt wird und nicht mehr angezeigt wird, nach Bedarf.

Codeausschnitte in diesem Handbuch stammen aus dieser Beispiel-app.

## <a name="default-action-buttons"></a>Standard-Aktionsschaltflächen

Eine Benachrichtigung für die Kategorie bestimmt die Standardschaltflächen für die Aktion an.

Erstellen Sie und registrieren Sie Notification-Kategorien aus, während eine Anwendung startet.
Z. B. in der [Beispiel-app](#sample-app-redgreennotifications), `FinishedLaunching` -Methode der `AppDelegate` bewirkt Folgendes:

- Definiert eine Kategorie für rote Benachrichtigungen und eine für Grün Benachrichtigungen
- Registriert diese Kategorien durch Aufrufen der [`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
Methode `UNUserNotificationCenter`
- Fügt eine einzelnen [`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
jeder Kategorie

Der folgende Code zeigt, wie dies funktioniert:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional | UNAuthorizationOptions.ProvidesAppNotificationSettings;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
        var rotateTwentyDegreesAction = UNNotificationAction.FromIdentifier("rotate-twenty-degrees-action", "Rotate 20°", UNNotificationActionOptions.None);

        var redCategory = UNNotificationCategory.FromIdentifier(
            "red-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var greenCategory = UNNotificationCategory.FromIdentifier(
            "green-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var set = new NSSet<UNNotificationCategory>(redCategory, greenCategory);
        center.SetNotificationCategories(set);
    });
    // ...
}
```

Basierend auf diesem Code wird eine Benachrichtigung, deren [`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
ist "Rot-Kategorie" oder "Green-Kategorie" werden standardmäßig Anzeigen einer **20 Grad drehen** Aktionsschaltfläche.

## <a name="in-app-handling-of-notification-action-buttons"></a>Behandlung von in-app-Benachrichtigung Aktionsschaltflächen

`UNUserNotificationCenter` verfügt über eine `Delegate` Eigenschaft vom Typ [ `IUNUserNotificationCenterDelegate` ](xref:UserNotifications.IUNUserNotificationCenterDelegate).

In der Beispiel-app `AppDelegate` richtet sich selbsttätig als Benutzer Notification Center des Delegaten in `FinishedLaunching`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = // ...
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        center.Delegate = this;
        // ...
```

Klicken Sie dann `AppDelegate` implementiert [`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
zum Behandeln der Aktionsschaltfläche tippt:

```csharp
[Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
{
    if (response.IsDefaultAction)
    {
        Console.WriteLine("ACTION: Default");
    }
    if (response.IsDismissAction)
    {
        Console.WriteLine("ACTION: Dismiss");
    }
    else
    {
        Console.WriteLine($"ACTION: {response.ActionIdentifier}");
    }

    completionHandler();
        }
```

Diese Implementierung der `DidReceiveNotificationResponse` behandelt nicht der benachrichtigungs **20 Grad drehen** Aktionsschaltfläche. Stattdessen verarbeitet die die Erweiterung für benachrichtigungsinhalte Taps auf diese Schaltfläche. Im nächste Abschnitt weiter erläutert Benachrichtigung Aktion Button-Verarbeitung.

## <a name="action-buttons-in-the-notification-content-extension"></a>Aktionsschaltflächen in der Erweiterung für benachrichtigungsinhalte

Eine Erweiterung für benachrichtigungsinhalte enthält einen ansichtscontroller, der die benutzerdefinierte Schnittstelle für eine Benachrichtigung definiert.

Diesem ansichtscontroller können die `GetNotificationActions` und `SetNotificationActions` Methoden für die [`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
Zugreifen auf und ändern die benachrichtigungs Aktionsschaltflächen-Eigenschaft.

In der Beispiel-app ändert die Erweiterung für benachrichtigungsinhalte des ansichtscontrollers die Aktionsschaltflächen, nur, wenn auf ein Tippen auf einen bereits vorhandenen Aktionsschaltfläche reagiert.

> [!NOTE]
> Eine Benachrichtigung inhaltserweiterung kann dann Antworten, eine Aktion Knopfdruck in der des ansichtscontrollers [ `DidReceiveNotificationResponse` ](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*) Methode, die als Teil des deklarierten [IUNNotificationContentExtension](xref:UserNotificationsUI.IUNNotificationContentExtension).
>
> Obwohl es sich um einen Namen mit teilt die `DidReceiveNotificationResponse` Methode [oben beschriebenen](#in-app-handling-of-notification-action-buttons), dies ist eine andere Methode.
>
> Nach Abschluss eine Erweiterung für benachrichtigungsinhalte Verarbeitung berühren einer Schaltfläche können sie, ob die hauptanwendung, behandeln dieses gleiche fingertipp auf eine Schaltfläche zu informieren. Zu diesem Zweck müssen sie einen passender Wert von übergeben [UNNotificationContentExtensionResponseOption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption) zu seinem Abschluss-Ereignishandler:
>
> - `Dismiss` Gibt an, dass der Benachrichtigungsschnittstelle geschlossen werden soll, und die Haupt-app, nicht unbedingt die fingertipp auf eine Schaltfläche zu behandeln.
> - `DismissAndForwardAction` Gibt an, dass der Benachrichtigungsschnittstelle geschlossen werden soll, und tippen Sie auf die Schaltfläche auch in die Haupt-app verarbeiten soll.
> - `DoNotDismiss` Gibt an, dass die Schnittstelle für Benachrichtigungen nicht geschlossen werden soll, und die Haupt-app, nicht unbedingt die fingertipp auf eine Schaltfläche zu behandeln.

Die inhaltserweiterung `DidReceiveNotificationResponse` Methode bestimmt, welche Aktionsschaltfläche angetippt wurde, wird das Bild in die benachrichtigungs Schnittstelle, und zeigt, oder blendet Sie aus einer **zurücksetzen** Aktionsschaltfläche:

```csharp
[Export("didReceiveNotificationResponse:completionHandler:")]
public void DidReceiveNotificationResponse(UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
{
    var rotationAction = ExtensionContext.GetNotificationActions()[0];

    if (response.ActionIdentifier == "rotate-twenty-degrees-action")
    {
        rotationButtonTaps += 1;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        // 9 rotations * 20 degrees = 180 degrees. No reason to
        // show the reset rotation button when the image is half
        // or fully rotated.
        if (rotationButtonTaps % 9 == 0)
        {
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
        }
        else if (rotationButtonTaps % 9 == 1)
        {
            var resetRotationAction = UNNotificationAction.FromIdentifier("reset-rotation-action", "Reset rotation", UNNotificationActionOptions.None);
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction, resetRotationAction });
        }
    }

    if (response.ActionIdentifier == "reset-rotation-action")
    {
        rotationButtonTaps = 0;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
    }

    completionHandler(UNNotificationContentExtensionResponseOption.DoNotDismiss);
}
```

In diesem Fall die Methode übergibt `UNNotificationContentExtensionResponseOption.DoNotDismiss` zu seinem Abschluss-Ereignishandler. Dies ist das bedeutet, dass die benachrichtigungs Benutzeroberfläche geöffnet bleiben.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Framework für Benutzerbenachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Deklarieren Sie Ihre Aktionen erfordernde Benachrichtigungstypen](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- ["Usernotifications" (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neuerungen in Benutzerbenachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuigkeiten in Benutzerbenachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generieren eine Remotebenachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
