---
title: Verwaltung von Benachrichtigungen in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Xamarin.iOS zu verwenden, um neue Verwaltungsfunktionen für Notification in iOS-12 eingeführt nutzen.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: a7a3bb8f720f1c6a2370a2510659693bb28ea09b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111517"
---
# <a name="notification-management-in-xamarinios"></a>Verwaltung von Benachrichtigungen in Xamarin.iOS

Das Betriebssystem in iOS 12 können deep-Link in der Mitteilungszentrale und der Einstellungs-app-Bildschirm zur Eingabe einer app Benachrichtigung. Dieser Bildschirm sollten Benutzern aktivieren und aus verschiedenen Typen von Benachrichtigungen, die die app sendet.

## <a name="sample-app-redgreennotifications"></a>Beispiel-app: RedGreenNotifications

Um ein Beispiel für die Funktionsweise der Verwaltung der Benachrichtigung zu sehen, sehen Sie sich die [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) Beispiel-app.

Diese Beispiel-app sendet zwei Arten von Benachrichtigungen, – Rot, Grün – und bietet einen Bildschirm, der Benutzern ermöglicht, das ein-bzw. auszuschalten beide Arten.

Codeausschnitte in diesem Handbuch stammen aus dieser Beispiel-app.

## <a name="notification-management-screen"></a>Benachrichtigungs-Bildschirm

In der Beispiel-app `ManageNotificationsViewController` definiert eine Benutzeroberfläche, die unabhängig voneinander aktivieren und Deaktivieren der roten Benachrichtigungen und grünen Benachrichtigungen ermöglicht. Dies ist ein standard [`UIViewController`](https://developer.xamarin.com/api/type/UIKit.UIViewController/)
mit einem [ `UISwitch` ](https://developer.xamarin.com/api/type/UIKit.UISwitch/) für jeden Benachrichtigungstyp. Umschalten des Schalters für beide Arten von Benachrichtigungen wird gespeichert, in den Standardeinstellungen für Benutzer, die Einstellungen der Benutzer für diese Art von Benachrichtigung:

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> Das Verwaltungsfenster Benachrichtigung überprüft auch, und zwar unabhängig davon, ob der Benutzer die Benachrichtigungen für die app vollständig deaktiviert hat. Wenn dies der Fall ist, blendet sie die Optionen für die einzelnen Benachrichtigungstypen an. Das Verwaltungsfenster für die Benachrichtigung dazu:
>
> - Aufrufe [ `UNUserNotificationCenter.Current.GetNotificationSettingsAsync` ](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync()/) und untersucht die [ `AuthorizationStatus` ](https://developer.xamarin.com/api/property/UserNotifications.UNNotificationSettings.AuthorizationStatus/) Eigenschaft.
> - Blendet die Optionen für die einzelnen Benachrichtigungstypen an, wenn Benachrichtigungen für die app vollständig deaktiviert wurden.
> - Erneut prüft, ob Benachrichtigungen jedes Mal deaktiviert wurden, um die Anwendung in den Vordergrund verschoben wird, da der Benutzer aktivieren/Benachrichtigungen in iOS-Einstellungen zu einem beliebigen Zeitpunkt deaktivieren kann.

Der Beispiel-app `ViewController` -Klasse, die der Benachrichtigungen, überprüfen Sie die Einstellung des Benutzers sendet, bevor senden eine lokale Benachrichtigung, um sicherzustellen, dass die Benachrichtigung vom Typ der Benutzer ist tatsächlich empfangen möchte:

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>Deep-link

iOS deep-Links zu einer app Benachrichtigung-Bildschirm von Mitteilungszentrale und der app-Benachrichtigungseinstellungen in der Einstellungs-app. Um dies zu ermöglichen, müssen eine app aus:

- Um anzugeben, dass ein Benachrichtigung-Bildschirm durch Übergabe verfügbar ist `UNAuthorizationOptions.ProvidesAppNotificationSettings` auf die app Benachrichtigung-autorisierungsanforderung.
- Implementieren der `OpenSettings` aus [ `IUNUserNotificationCenterDelegate` ](https://developer.xamarin.com/api/type/UserNotifications.IUNUserNotificationCenterDelegate/).

### <a name="authorization-request"></a>Autorisierungsanforderung

Das Betriebssystem an, dass ein Benachrichtigung-Bildschirm ist verfügbar, eine app sollte übergeben die `UNAuthorizationOptions.ProvidesAppNotificationSettings` (zusammen mit anderen Benachrichtigung Übermittlungsoptionen muss) die Möglichkeit, die `RequestAuthorization` Methode für die `UNUserNotificationCenter`.

In der Beispiel-app beispielsweise `AppDelegate`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.ProvidesAppNotificationSettings | UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
```

### <a name="opensettings-method"></a>OpenSettings-Methode

Die `OpenSettings` Methode wird aufgerufen, die vom System deep-Link zu einer app Benachrichtigung-Bildschirm, sollte den Benutzer direkt auf diesem Bildschirm navigieren.

In der Beispiel-app führt diese Methode der Segue an die `ManageNotificationsViewController` bei Bedarf:

```csharp
[Export("userNotificationCenter:openSettingsForNotification:")]
public void OpenSettings(UNUserNotificationCenter center, UNNotification notification)
{
    var navigationController = Window.RootViewController as UINavigationController;
    if (navigationController != null)
    {
        var currentViewController = navigationController.VisibleViewController;
        if (currentViewController is ViewController)
        {
            currentViewController.PerformSegue(ManageNotificationsViewController.ShowManageNotificationsSegue, this);
        }

    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Framework für Benutzerbenachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- ["Usernotifications" (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neuerungen in Benutzerbenachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuigkeiten in Benutzerbenachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generieren eine Remotebenachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
