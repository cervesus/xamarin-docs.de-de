---
title: Verwaltung von Benachrichtigungen in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Xamarin.iOS zu verwenden, um neue Verwaltungsfunktionen für Notification in iOS-12 eingeführt nutzen.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 0157a685ac990c0626cd4d6001ef853c6a28b993
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035273"
---
# <a name="notification-management-in-xamarinios"></a>Verwaltung von Benachrichtigungen in Xamarin.iOS

Das Betriebssystem in iOS 12 können deep-Link in der Mitteilungszentrale und der Einstellungs-app-Bildschirm zur Eingabe einer app Benachrichtigung. Dieser Bildschirm sollten Benutzern aktivieren und aus verschiedenen Typen von Benachrichtigungen, die die app sendet.

## <a name="sample-app-redgreennotifications"></a>Beispiel-app: RedGreenNotifications

Um ein Beispiel für die Funktionsweise der Verwaltung der Benachrichtigung zu sehen, sehen Sie sich die [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) Beispiel-app.

Diese Beispiel-app sendet zwei Arten von Benachrichtigungen, – Rot, Grün – und bietet einen Bildschirm, der Benutzern ermöglicht, das ein-bzw. auszuschalten beide Arten.

Codeausschnitte in diesem Handbuch stammen aus dieser Beispiel-app.

## <a name="notification-management-screen"></a>Benachrichtigungs-Bildschirm

In der Beispiel-app `ManageNotificationsViewController` definiert eine Benutzeroberfläche, die unabhängig voneinander aktivieren und Deaktivieren der roten Benachrichtigungen und grünen Benachrichtigungen ermöglicht. Dies ist ein standard [`UIViewController`](xref:UIKit.UIViewController)
mit einem [ `UISwitch` ](xref:UIKit.UISwitch) für jeden Benachrichtigungstyp. Umschalten des Schalters für beide Arten von Benachrichtigungen wird gespeichert, in den Standardeinstellungen für Benutzer, die Einstellungen der Benutzer für diese Art von Benachrichtigung:

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> Das Verwaltungsfenster Benachrichtigung überprüft auch, und zwar unabhängig davon, ob der Benutzer die Benachrichtigungen für die app vollständig deaktiviert hat. Wenn dies der Fall ist, blendet sie die Optionen für die einzelnen Benachrichtigungstypen an. Das Verwaltungsfenster für die Benachrichtigung dazu:
>
> - Aufrufe [ `UNUserNotificationCenter.Current.GetNotificationSettingsAsync` ](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync) und untersucht die [ `AuthorizationStatus` ](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus) Eigenschaft.
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
- Implementieren der `OpenSettings` aus [ `IUNUserNotificationCenterDelegate` ](xref:UserNotifications.IUNUserNotificationCenterDelegate).

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
