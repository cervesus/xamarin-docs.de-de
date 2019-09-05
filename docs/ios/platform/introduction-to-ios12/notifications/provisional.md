---
title: Vorläufige Benachrichtigungen in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie xamarin. IOS verwenden, um mit provisorischen Benachrichtigungen zu arbeiten. In IOS 12 eingeführte vorläufige Benachrichtigungen ermöglichen Anwendungen das Senden von stillen Benachrichtigungen ohne explizite Benutzerberechtigungen.
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: d321e8061d3091abeaa3cff6a6af9172c981cb60
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291202"
---
# <a name="provisional-notifications-in-xamarinios"></a>Vorläufige Benachrichtigungen in xamarin. IOS

Durch vorläufige Benachrichtigungen können apps Benachrichtigungen übermitteln, ohne dass eine vorherige Zustimmung des Benutzers besteht. Diese Benachrichtigungen werden nur in der Benachrichtigungs zentrale angezeigt, sodass Benutzer Sie in der Vorschau anzeigen können, bevor Sie sich für die fortgesetzte Bereitstellung entscheiden.

Im Benachrichtigungs Center können Benutzer angeben, dass eine APP die Bereitstellung von provisorischen Benachrichtigungen beenden, Sie weiterhin vorläufig bereitstellt oder mit der Bereitstellung der Lösungen beginnen soll.

## <a name="sample-app-redgreennotifications"></a>Beispiel-App: RedGreenNotifications

Sehen Sie sich die Beispiel-APP [redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) an, mit der vorläufige Benachrichtigungen gesendet werden.

## <a name="sending-provisional-notifications"></a>Senden von provisorischen Benachrichtigungen

Um vorläufige Benachrichtigungen zu senden, `UNAuthorizationOptions.Provisional` geben Sie als Option für das[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)
Methode von `UNUserNotificationCenter`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

Wenn der Benutzer vorläufige Benachrichtigungen auf eine bedeutende Übermittlung herauf stuft `UNAuthorizationOptions` , bestimmen die `RequestAuthorization` an übergebenen Werte die neuen Einstellungen für die Benachrichtigungs Übermittlung ( `UNAuthorizationOptions.Alert` im `UNAuthorizationOptions.Sound`obigen Code und).

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App – redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Benutzer Benachrichtigungs Framework in xamarin. IOS](~/ios/platform/user-notifications/index.md)
- [Benutzer Benachrichtigungen (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neues in Benutzer Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuerungen bei Benutzer Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Erstellen einer Remote Benachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
