---
title: Interaktive Benachrichtigungs Benutzeroberflächen in xamarin. IOS
description: Mit IOS 12 ist es möglich, interaktive Benutzeroberflächen für lokale und Remote Benachrichtigungen zu erstellen. In diesem Leitfaden wird beschrieben, wie diese Funktionen mit xamarin. IOS verwendet werden.
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: bc566cf3744b8d6ec05204153b7c731935f98b8a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652437"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Interaktive Benachrichtigungs Benutzeroberflächen in xamarin. IOS

[Erweiterungen für Benachrichtigungs Inhalte](~/ios/platform/user-notifications/advanced-user-notifications.md), die in ios 10 eingeführt wurden, ermöglichen das Erstellen benutzerdefinierter Benutzeroberflächen für Benachrichtigungen. Ab IOS 12 können Benachrichtigungs Benutzerschnittstellen interaktive Elemente wie Schaltflächen und Schieberegler enthalten.

## <a name="sample-app-redgreennotifications"></a>Beispiel-App: RedGreenNotifications

Die Beispiel-APP [redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) enthält eine Erweiterung für Benachrichtigungs Inhalte mit einer interaktiven Benutzeroberfläche.

Code Ausschnitte in dieser Anleitung stammen aus diesem Beispiel.

## <a name="notification-content-extension-infoplist-file"></a>Benachrichtigungs Inhalts Erweiterung Info. plist-Datei

In der Beispiel-app enthält die **Info. plist** -Datei im Projekt **redgreennotificationscontentextension** die folgende Konfiguration:

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

- Das `UNNotificationExtensionCategory` Array gibt den Typ der Benachrichtigungs Kategorien an, die von der Inhalts Erweiterung behandelt werden.
- Um interaktive Inhalte zu unterstützen, legt die Erweiterung für Benachrichtigungs `UNNotificationExtensionUserInteractionEnabled` Inhalte den `true`Schlüssel auf fest.
- Der `UNNotificationExtensionInitialContentSizeRatio` Schlüssel gibt das anfängliche height/width-Verhältnis für die Schnittstelle der Inhalts Erweiterung an.

## <a name="interactive-interface"></a>Interaktive Oberfläche

**Maininterface. Storyboard**, das die Schnittstelle für eine Erweiterung für Benachrichtigungs Inhalte definiert, ist ein Standard-Storyboard, das einen einzelnen Ansichts Controller enthält. In der Beispiel-App weist der Ansichts Controller den `NotificationViewController`Typ auf und enthält eine Bildansicht, drei Schaltflächen und einen Schieberegler. Das Storyboard verknüpft diese Steuerelemente mit Handlern, die in **NotificationViewController.cs**definiert sind:

- Mit dem Schaltflächen Handler **app starten** wird die `PerformNotificationDefaultAction` Aktions `ExtensionContext`Methode auf aufgerufen, die die APP startet:

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    In der APP kann das Benutzer Benachrichtigungs Center `Delegate` (in der Beispiel-APP `AppDelegate`) auf die Interaktion in der `DidReceiveNotificationResponse` -Methode reagieren:

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- Der Handler für das Verwerfen von `DismissNotificationContentExtension` **Benachrichtigungs** Schaltflächen Ruft auf auf `ExtensionContext`und schließt die Benachrichtigung:

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- Mit dem Handler zum **Entfernen von Benachrichtigungs** Schaltflächen wird die Benachrichtigung geschlossen und aus dem Benachrichtigungs Center entfernt:

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- Die Methode, die Wertänderungen am Schieberegler behandelt, aktualisiert das Alpha des Bilds, das in der Benutzeroberfläche der Benachrichtigung angezeigt wird:

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App – redgreenbenachrichtigungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Benutzer Benachrichtigungs Framework in xamarin. IOS](~/ios/platform/user-notifications/index.md)
- [Benutzer Benachrichtigungen (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Neues in Benutzer Benachrichtigungen (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Bewährte Methoden und Neuerungen bei Benutzer Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Umfassende Benachrichtigungen (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [Erstellen einer Remote Benachrichtigung (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
