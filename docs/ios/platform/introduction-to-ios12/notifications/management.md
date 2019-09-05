---
title: Benachrichtigungs Verwaltung in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie xamarin. IOS verwenden, um die neuen Benachrichtigungs Verwaltungs Features zu nutzen, die in IOS 12 eingeführt wurden.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: b6c6baad2cbd923bde4dab3766040b5df4351787
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282119"
---
# <a name="notification-management-in-xamarinios"></a>Benachrichtigungs Verwaltung in xamarin. IOS

In IOS 12 kann das Betriebssystem einen Deep-Link zwischen dem Benachrichtigungs Center und der App "Einstellungen" mit dem Benachrichtigungs Verwaltungs Bildschirm einer App herstellen. Auf diesem Bildschirm können Benutzer die verschiedenen Benachrichtigungs Typen, die von der APP gesendet werden, abonnieren.

## <a name="sample-app-redgreennotifications"></a>Beispiel-App: RedGreenNotifications

Ein Beispiel für die Funktionsweise der Benachrichtigungs Verwaltung finden Sie in der Beispiel-APP [redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) .

Diese Beispiel-App sendet zwei Arten von Benachrichtigungen – rot und grün – und bietet einen Bildschirm, der es Benutzern ermöglicht, beide Arten zu abonnieren.

Code Ausschnitte in dieser Anleitung stammen aus dieser Beispiel-app.

## <a name="notification-management-screen"></a>Benachrichtigungs Verwaltungs Bildschirm

In der Beispiel-APP `ManageNotificationsViewController` definiert eine Benutzeroberfläche, die Benutzern das unabhängige aktivieren und Deaktivieren von roten Benachrichtigungen und grünen Benachrichtigungen ermöglicht. Es ist ein Standard[`UIViewController`](xref:UIKit.UIViewController)
, der [`UISwitch`](xref:UIKit.UISwitch) einen für jeden Benachrichtigungstyp enthält. Ein-und Ausschalten des Schalters für beide Arten von Benachrichtigungen in den Benutzer Standardwerten für diesen Benachrichtigungstyp:

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> Der Bildschirm Benachrichtigungs Verwaltung prüft auch, ob der Benutzer vollständig deaktivierte Benachrichtigungen für die APP hat. Wenn dies der Fall ist, werden die UMSCHALT Flächen für die einzelnen Benachrichtigungs Typen ausgeblendet. Gehen Sie hierzu wie folgt vor:
>
> - Ruft [`UNUserNotificationCenter.Current.GetNotificationSettingsAsync`](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync) [die`AuthorizationStatus`](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus) -Eigenschaft auf und überprüft sie.
> - Blendet die UMSCHALT Flächen für die einzelnen Benachrichtigungs Typen aus, wenn Benachrichtigungen für die APP vollständig deaktiviert wurden.
> - Prüft, ob Benachrichtigungen bei jedem Verschieben der Anwendung in den Vordergrund deaktiviert wurden, da der Benutzer jederzeit Benachrichtigungen in den IOS-Einstellungen aktivieren/deaktivieren kann.

Mit der-Klasse `ViewController` der Beispiel-APP, die die Benachrichtigungen sendet, wird die Benutzereinstellung vor dem Senden einer lokalen Benachrichtigung überprüft, um sicherzustellen, dass es sich bei der Benachrichtigung um einen Typ handelt, den der Benutzer tatsächlich erhalten möchte:

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>Deep-Link

IOS Deep Links zum Bildschirm für die Benachrichtigungs Verwaltung einer App aus dem Benachrichtigungs Center und die Benachrichtigungseinstellungen der app in der App "Einstellungen". Um dies zu vereinfachen, muss eine APP folgende Aktionen ausführen:

- Geben Sie an, dass ein Benachrichtigungs Verwaltungs Bildschirm `UNAuthorizationOptions.ProvidesAppNotificationSettings` verfügbar ist, indem Sie die Benachrichtigungs Autorisierungs Anforderung der APP übergeben.
- Implementieren Sie `OpenSettings` die- [`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate)Methode aus.

### <a name="authorization-request"></a>Autorisierungs Anforderung

Um dem Betriebssystem mitzuteilen, dass ein Benachrichtigungs Verwaltungs Bildschirm verfügbar ist, sollte eine APP die `UNAuthorizationOptions.ProvidesAppNotificationSettings` -Option (zusammen mit allen anderen erforderlichen Benachrichtigungs Zustellungs Optionen) `RequestAuthorization` an die- `UNUserNotificationCenter`Methode in der übergeben.

Beispielsweise in der Beispiel- `AppDelegate`App:

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

### <a name="opensettings-method"></a>Opensettings-Methode

Die `OpenSettings` Methode, die vom System zum Deep-Link zum Benachrichtigungs Verwaltungs Bildschirm einer APP aufgerufen wird, sollte den Benutzer direkt zu diesem Bildschirm navigieren.

In der Beispiel-App führt diese Methode ggf. den- `ManageNotificationsViewController` Abschnitt für die-Methode aus:

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

- [Beispiel-App – redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Benutzer Benachrichtigungs Framework in xamarin. IOS](~/ios/platform/user-notifications/index.md)
- [Benutzer Benachrichtigungen (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neues in Benutzer Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuerungen bei Benutzer Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Erstellen einer Remote Benachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
