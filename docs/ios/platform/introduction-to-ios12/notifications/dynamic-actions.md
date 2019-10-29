---
title: Schaltflächen für dynamische Benachrichtigungs Aktionen in xamarin. IOS
description: Mit IOS 12 kann eine Erweiterung für Benachrichtigungs Inhalte die Aktions Schaltflächen hinzufügen, entfernen und aktualisieren, die neben einer Benachrichtigung angezeigt werden. In diesem Dokument wird beschrieben, wie die Schaltflächen für dynamische Benachrichtigungs Aktionen mit xamarin. IOS verwendet werden.
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: cb38d222cecd1a6c5bb65b0fb376888770dd0e49
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031958"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Schaltflächen für dynamische Benachrichtigungs Aktionen in xamarin. IOS

In IOS 12 können Benachrichtigungen ihre zugeordneten Aktions Schaltflächen dynamisch hinzufügen, entfernen und aktualisieren. Diese Anpassung ermöglicht Benutzern das Bereitstellen von Aktionen, die für den Inhalt der Benachrichtigung direkt relevant sind, sowie die Interaktion des Benutzers.

## <a name="sample-app-redgreennotifications"></a>Beispiel-App: redgreenbenachrichtigungen

Die Code Ausschnitte in dieser Anleitung stammen aus der [redgreennotification](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) -Beispiel-APP, die die Verwendung von xamarin. IOS zum Arbeiten mit Benachrichtigungs Aktions Schaltflächen in IOS 12 veranschaulicht.

Diese Beispiel-App sendet zwei Arten von lokalen Benachrichtigungen: rot und grün.
Nachdem die APP eine Benachrichtigung gesendet hat, verwenden Sie den 3D-Fingerabdruck, um die benutzerdefinierte Benutzeroberfläche anzuzeigen. Verwenden Sie dann die Aktions Schaltflächen der Benachrichtigung, um das Bild zu drehen, das angezeigt wird. Wenn das Bild rotiert, wird die Schaltfläche **Drehung zurücksetzen** angezeigt und bei Bedarf ausgeblendet.

Code Ausschnitte in dieser Anleitung stammen aus dieser Beispiel-app.

## <a name="default-action-buttons"></a>Standard Aktions Schaltflächen

Die Standard Aktions Schaltflächen werden von einer Benachrichtigungs Kategorie bestimmt.

Erstellen und registrieren Sie Benachrichtigungs Kategorien, während eine Anwendung gestartet wird.
In der Beispiel- [App](#sample-app-redgreennotifications)führt die `FinishedLaunching`-Methode von `AppDelegate` die folgenden Schritte aus:

- Definiert eine Kategorie für rote Benachrichtigungen und eine andere für grüne Benachrichtigungen.
- Registriert diese Kategorien durch Aufrufen des [`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
Methode der `UNUserNotificationCenter`
- Fügt eine einzelne [`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
zu jeder Kategorie

Der folgende Beispielcode zeigt, wie dies funktioniert:

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

Basierend auf diesem Code, jede Benachrichtigung, deren [`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
der Wert "Red-Category" oder "Green-Category" zeigt standardmäßig die Schaltfläche " **20 ° drehen** " an.

## <a name="in-app-handling-of-notification-action-buttons"></a>In-App-Behandlung von Schaltflächen für Benachrichtigungs Aktionen

`UNUserNotificationCenter` verfügt über eine `Delegate`-Eigenschaft vom Typ [`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate).

In der Beispiel-APP wird `AppDelegate` sich selbst als Delegat des Benutzer Benachrichtigungs Centers in `FinishedLaunching`festlegen:

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

`AppDelegate` implementiert dann [`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
So verarbeiten Sie Aktions Schaltflächen:

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

Diese Implementierung von `DidReceiveNotificationResponse` behandelt nicht die Aktions Schaltfläche **20 ° drehen** der Benachrichtigung. Stattdessen behandelt die Inhalts Erweiterung der Benachrichtigung auf dieser Schaltfläche tippen. Im nächsten Abschnitt wird die Behandlung von Schaltflächen für Benachrichtigungs Aktionen behandelt.

## <a name="action-buttons-in-the-notification-content-extension"></a>Aktions Schaltflächen in der Erweiterung für Benachrichtigungs Inhalte

Eine Erweiterung für Benachrichtigungs Inhalte enthält einen Ansichts Controller, der die benutzerdefinierte Schnittstelle für eine Benachrichtigung definiert.

Dieser Ansichts Controller kann die Methoden `GetNotificationActions` und `SetNotificationActions` für seine [`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
Eigenschaft für den Zugriff auf und die Änderung der Aktions Schaltflächen der Benachrichtigung.

In der Beispiel-app ändert der Ansichts Controller der Benachrichtigungs Inhalts Erweiterung die Aktions Schaltflächen nur bei der Reaktion auf eine bereits vorhandene Aktions Schaltfläche.

> [!NOTE]
> Eine Erweiterung für Benachrichtigungs Inhalte kann auf eine Aktions Schaltfläche in der [`DidReceiveNotificationResponse`](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*) -Methode des Ansichts Controllers Antworten, die als Teil von [iunnotificationcontentextension](xref:UserNotificationsUI.IUNNotificationContentExtension)deklariert ist.
>
> Obwohl Sie einen Namen mit der [oben beschriebenen](#in-app-handling-of-notification-action-buttons)`DidReceiveNotificationResponse` Methode freigibt, ist dies eine andere Methode.
>
> Nachdem eine Erweiterung für Benachrichtigungs Inhalte die Verarbeitung einer Schaltflächen Abzweigung abgeschlossen hat, kann Sie auswählen, ob die Hauptanwendung die gleiche Schaltflächen Abzweigung verarbeiten soll. Hierfür muss ein entsprechender Wert von [unnotificationcontentextensionresponanoption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption) an den Abschluss Handler übergeben werden:
>
> - `Dismiss` gibt an, dass die Benachrichtigungs Schnittstelle verworfen werden soll, und dass die Haupt-APP die Schaltflächen Abzweigung nicht verarbeiten muss.
> - `DismissAndForwardAction` gibt an, dass die Benachrichtigungs Schnittstelle verworfen werden soll, und dass die Haupt-APP auch die Schaltflächen Abzweigung verarbeiten soll.
> - `DoNotDismiss` gibt an, dass die Benachrichtigungs Schnittstelle nicht verworfen werden sollte und dass die Haupt-APP die Schaltflächen Abzweigung nicht verarbeiten muss.

Die `DidReceiveNotificationResponse`-Methode der Inhalts Erweiterung bestimmt, welche Aktions Schaltfläche angetippt wurde, dreht das Bild in der Benutzeroberfläche der Benachrichtigung und zeigt eine Schaltfläche zum **Zurücksetzen** der Aktion an

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

In diesem Fall übergibt die-Methode `UNNotificationContentExtensionResponseOption.DoNotDismiss` an ihren Abschluss Handler. Dies bedeutet, dass die Benutzeroberfläche der Benachrichtigung geöffnet bleibt.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App – redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Benutzer Benachrichtigungs Framework in xamarin. IOS](~/ios/platform/user-notifications/index.md)
- [Deklarieren von Aktions fähigen Benachrichtigungs Typen](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [Benutzer Benachrichtigungen (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neues in Benutzer Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuerungen bei Benutzer Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Erstellen einer Remote Benachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
