---
title: Kritische Warnungen in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie kritische Warnungen mit xamarin. IOS verwendet werden. Bei kritischen Warnungen, die mit IOS 12 eingeführt werden, handelt es sich um störende Benachrichtigungen, die unabhängig davon, ob nicht gestört ist, oder der Ruftonschalter deaktiviert ist.
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: 54a214215f77b66f6a4b134dcb8d27b26c44fb6c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291295"
---
# <a name="critical-alerts-in-xamarinios"></a>Kritische Warnungen in xamarin. IOS

Mit IOS 12 können apps kritische Warnungen senden. Kritische Warnungen spielen einen Sound, unabhängig davon, ob nicht stören aktiviert ist oder der Ruftonschalter deaktiviert ist. Diese Benachrichtigungen sind störend und sollten nur verwendet werden, wenn Benutzer sofortige Maßnahmen ergreifen müssen.

## <a name="custom-critical-alert-entitlement"></a>Berechtigung für benutzerdefinierte kritische Warnungen

Um kritische Warnungen in Ihrer APP anzuzeigen, fordern Sie zuerst [eine benutzerdefinierte Warnung für kritische Benachrichtigungs Benachrichtigungen](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/) von Apple an.

Nachdem Sie diese Berechtigung von Apple erhalten haben, und befolgen Sie die zugehörigen Anweisungen zum Konfigurieren Ihrer APP für deren Verwendung, fügen Sie die benutzerdefinierte [Berechtigung](~/ios/deploy-test/provisioning/entitlements.md) zu den **Berechtigungen. plist** -Datei (en) Ihrer APP hinzu. Konfigurieren Sie dann Ihre **IOS-Bundle-Signierungs** Optionen so, dass beim Signieren der APP sowohl auf dem **Simulator als auch** auf dem Gerät die Berechtigung ""

## <a name="request-authorization"></a>Autorisierung anfordern

Die Benachrichtigungs Autorisierungs Anforderung einer APP fordert den Benutzer auf, Benachrichtigungen zu einer APP zuzulassen oder zu unterbinden. Wenn die Benachrichtigungs Autorisierungs Anforderung zum Senden kritischer Warnungen aufgefordert wird, bietet die APP dem Benutzer auch die Möglichkeit, kritische Warnungen zu abonnieren.

Der folgende Code fordert die Berechtigung zum Senden kritischer Warnungen und Standard Benachrichtigungen und-Sounds an, indem er die entsprechende[`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
Werte für [`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*):

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

Um eine lokale kritische Warnung zu senden, erstellen Sie eine[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
und legen Sie `Sound` die-Eigenschaft auf Folgendes fest:

- `UNNotificationSound.DefaultCriticalSound`, bei dem der standardmäßige kritische Benachrichtigungs Sound verwendet wird.
- `UNNotificationSound.GetCriticalSound`ermöglicht es Ihnen, einen benutzerdefinierten Sound anzugeben, der mit Ihrer APP und einem Volume gebündelt ist.

Erstellen Sie dann eine `UNNotificationRequest` aus dem Benachrichtigungs Inhalt, und fügen Sie Sie dem Benachrichtigungs Center hinzu:

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
> Kritische Warnungen werden nicht übermittelt, wenn Sie für Ihre APP nicht aktiviert sind. Zusammen mit der Eingabeaufforderung, die angezeigt wird, wenn eine APP zum Senden kritischer Warnungen zum ersten Mal eine Berechtigung anfordert, kann ein Benutzer kritische Warnungen auch im **Benachrichtigungs** Abschnitt Ihrer APP in der IOS- **Einstellungs** -App aktivieren bzw. deaktivieren.

## <a name="remote-critical-alerts"></a>Kritische Remote Warnungen

Informationen zu kritischen Remote Warnungen finden Sie [in den Neuerungen in der Benutzer Benachrichtigungs](https://developer.apple.com/videos/play/wwdc2018/710/) Sitzung von WWDC 2018 und im Dokument zum [Erstellen einer Remote Benachrichtigung](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) .

## <a name="related-links"></a>Verwandte Links

- [Benutzer Benachrichtigungs Framework in xamarin. IOS](~/ios/platform/user-notifications/index.md)
- [Benutzer Benachrichtigungen (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neues in Benutzer Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuerungen bei Benutzer Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Erstellen einer Remote Benachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
