---
title: Interaktive Benachrichtigung von Benutzeroberflächen in Xamarin.iOS
description: In iOS 12 ist es möglich, interaktive Benutzeroberflächen für lokale und remote-Benachrichtigungen zu erstellen. Dieses Handbuch beschreibt, wie diese Funktionen mit Xamarin.iOS verwendet wird.
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: e6dc2f14b36c9d6f67f1df5ad3d118fa423e0d4d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034912"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Interaktive Benachrichtigung von Benutzeroberflächen in Xamarin.iOS

[Notification-Content-Erweiterungen](~/ios/platform/user-notifications/advanced-user-notifications.md), in iOS 10 eingeführt wurde, ermöglichen das Erstellen benutzerdefinierter Benutzeroberflächen für Benachrichtigungen. Beginnend mit iOS 12 können Benutzeroberflächen für die Benachrichtigung interaktiven Elemente wie Schaltflächen und Regler enthalten.

## <a name="sample-app-redgreennotifications"></a>Beispiel-App: RedGreenNotifications

Die [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) Beispiel-app enthält eine Erweiterung für benachrichtigungsinhalte mit einer interaktiven Benutzeroberfläche.

Codeausschnitte in diesem Handbuch ist in diesem Beispiel stammen.

## <a name="notification-content-extension-infoplist-file"></a>Benachrichtigungsdatei inhaltserweiterung-Datei "Info.plist"

In der Beispiel-app die **"Info.plist"** Datei die **RedGreenNotificationsContentExtension** -Projekt enthält die folgende Konfiguration:

```xml
<!-- ... -->
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>UNNotificationExtensionCategory</key>
        <array>
            <string>red-category</string>
            <string>green-category</string>
        </array>
        <key>UNNotificationExtensionUserInteractionEnabled</key>
        <true/>
        <key>UNNotificationExtensionDefaultContentHidden</key>
        <true/>
        <key>UNNotificationExtensionInitialContentSizeRatio</key>
        <real>0.6</real>
    </dict>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.usernotifications.content-extension</string>
    <key></key>
    <true/>
</dict>
<!-- ... -->
```

Beachten Sie die folgenden Features:

- Die `UNNotificationExtensionCategory` Array gibt den Typ der Benachrichtigungskategorien die inhaltserweiterung-Handles.
- Um interaktive Inhalte zu unterstützen, die Erweiterung für benachrichtigungsinhalte legt die `UNNotificationExtensionUserInteractionEnabled` um `true`.
- Die `UNNotificationExtensionInitialContentSizeRatio` Schlüssel gibt das anfängliche Höhe/Breite-Verhältnis für die inhaltserweiterung-Schnittstelle.

## <a name="interactive-interface"></a>Interaktive Schnittstelle

**MainInterface.storyboard**, definiert die Schnittstelle für eine Erweiterung für benachrichtigungsinhalte, ist ein standard-Storyboard mit einem einzelnen ansichtscontroller. In der Beispiel-app, der der ansichtscontroller ist vom Typ `NotificationViewController`, und ein Bild anzeigen, drei Schaltflächen und ein Schieberegler enthält. Das Storyboard ordnet diese Steuerelemente im definierten Handler **NotificationViewController.cs**:

- Die **App starten** Schaltfläche Ereignishandler ruft die `PerformNotificationDefaultAction` Aktionsmethode auf `ExtensionContext`, die die app gestartet:

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    In der app, die Benutzer mitteilungszentrale `Delegate` (in der Beispiel-app, die `AppDelegate`) kann dann Antworten, die Interaktion in den `DidReceiveNotificationResponse` Methode:

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- Die **Benachrichtigung verwerfen** Schaltfläche Ereignishandler ruft `DismissNotificationContentExtension` auf `ExtensionContext`, die die Benachrichtigung geschlossen:

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- Die **Benachrichtigung entfernen** Schaltflächenhandler schließt die Benachrichtigung und entfernt es aus der Mitteilungszentrale:

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- Den Alphawert des Bilds angezeigt, in die benachrichtigungs Schnittstelle, aktualisiert die Methode, die Änderungen auf dem Schieberegler behandelt:

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Framework für Benutzerbenachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- ["Usernotifications" (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neuerungen in Benutzerbenachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuigkeiten in Benutzerbenachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Rich-Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [Generieren eine Remotebenachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
