---
title: Kritische Warnungen in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie wichtige Warnungen mit Xamarin.iOS verwendet wird. Kritische Warnungen, die mit iOS-12 eingeführt sind störende Benachrichtigungen, die einen Sound wiedergeben, die unabhängig davon, ob nicht stören sich befindet, oder der ruftonschalter ist deaktiviert.
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 4f847a86f3f92bcf7168c2e104471e1ca052969c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131455"
---
# <a name="critical-alerts-in-xamarinios"></a>Kritische Warnungen in Xamarin.iOS

In iOS 12 können apps, kritische Warnungen senden. Kritische Warnungen Wiedergeben eines Sounds unabhängig davon, ob nicht stören aktiviert ist, oder der ruftonschalter ist deaktiviert. Diese Benachrichtigungen führen zu einer Unterbrechung und sollte nur verwendet werden, wenn der Benutzer sofort Maßnahmen ergreifen müssen.

## <a name="custom-critical-alert-entitlement"></a>Benutzerdefinierte Berechtigung für kritische Warnungen

Kritische Warnungen in Ihrer app zuerst anzeigen [fordern Sie eine benutzerdefinierte kritische Benachrichtigungen Berechtigung](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/) von Apple.

Nachdem diese Berechtigung von Apple erhalten und alle zugeordneten Anweisungen zum Konfigurieren Ihrer app verwenden, fügen die benutzerdefinierte [Berechtigung](~/ios/deploy-test/provisioning/entitlements.md) zu Ihrer app **"Entitlements.plist"** -Dateien. Konfigurieren Sie dann Ihre **iOS Bundle-Signierung** zu verwendenden Optionen **"Entitlements.plist"** beim Signieren der app nach Simulator und Gerät.

## <a name="request-authorization"></a>Autorisierung anfordern

Der app-Benachrichtigung-autorisierungsanforderung fordert den Benutzer zu erlauben oder verbieten der app-Benachrichtigungen. Wenn die benachrichtigungsanforderung für die Autorisierung für die Berechtigung zum Senden von kritischen Warnungen aufgefordert werden, erhalten die app auch dem Benutzer Gelegenheit, kritische Warnungen aktivieren.

Der folgende Code fordert die Berechtigung zum kritische Warnungen und standard-Benachrichtigungen und Sounds zu senden, indem Sie den entsprechenden übergeben [`UNAuthorizationOptions`](https://developer.xamarin.com/api/type/UserNotifications.UNAuthorizationOptions/)
Werte [ `RequestAuthorization` ](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenter.RequestAuthorization/):

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.CriticalAlert;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

## <a name="local-critical-alerts"></a>Lokale kritische Warnungen

Um eine lokale kritische Warnung senden, erstellen eine [`UNMutableNotificationContent`](https://developer.xamarin.com/api/type/UserNotifications.UNMutableNotificationContent/)
und legen Sie dessen `Sound` Eigenschaft entweder:

- `UNNotificationSound.DefaultCriticalSound`, der die Benachrichtigung über wichtige Standardsound verwendet.
- `UNNotificationSound.GetCriticalSound`, können Sie angeben, ein benutzerdefiniertes klingen, die mit Ihrer app und ein Volume gebündelt ist.

Erstellen Sie dann eine `UNNotificationRequest` aus der Benachrichtigung Inhalt und die mitteilungszentrale hinzuzufügen:

```csharp
var content = new UNMutableNotificationContent()
{
    Title = "Critical alert title",
    Body = "Text of the critical alert",
    CategoryIdentifier = "my-critical-alert-category",
    // Sound = UNNotificationSound.DefaultCriticalSound
    Sound = UNNotificationSound.GetCriticalSound("my_critical_sound.m4a", 1.0f)
};

var request = UNNotificationRequest.FromIdentifier(
    Guid.NewGuid().ToString(),
    content,
    UNTimeIntervalNotificationTrigger.CreateTrigger(3, false)
);

var center = UNUserNotificationCenter.Current;
center.AddNotificationRequest(request, null);
```

> [!IMPORTANT]
> Kritische Warnungen werden nicht übermittelt werden, wenn sie für Ihre app nicht aktiviert sind. Zusammen mit der Eingabeaufforderung ein, der das erste angezeigt wird. Mal, wenn eine app fordert die Berechtigung zum Senden von kritischen Warnungen, Benutzer kann auch aktivieren oder deaktivieren Sie kritische Warnungen in Ihrer app **Benachrichtigungen** Abschnitt des iOS **Einstellungen**app.

## <a name="remote-critical-alerts"></a>Remote kritische Warnungen

Informationen zu remote kritische Warnungen, finden Sie unter den [neuerungen neue In Benutzerbenachrichtigungen](https://developer.apple.com/videos/play/wwdc2018/710/) -Sitzung von WWDC 2018 und [generieren eine Remotebenachrichtigung](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) Dokument.

## <a name="related-links"></a>Verwandte Links

- [Framework für Benutzerbenachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- ["Usernotifications" (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neuerungen in Benutzerbenachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuigkeiten in Benutzerbenachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Generieren eine Remotebenachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
