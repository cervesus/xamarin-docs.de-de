---
title: Benachrichtigungen
ms.topic: article
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0f1d968dcee0cb9b6cd0cee8fa60be4f4dbb2833
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="notifications"></a>Benachrichtigungen

Sehen Sie sich, dass apps Benachrichtigungen empfangen können, wenn die enthaltende iOS-app unterstützt. Besteht der integrierten Benachrichtigung zu behandeln, damit Sie nicht *müssen* der unten beschriebene jedoch zusätzliche Benachrichtigung-Unterstützung hinzuzufügen, wenn Sie Benachrichtigungsverhalten anpassen möchten, und Darstellung lesen Sie dann auf.

Finden Sie in der [iOS Benachrichtigungen](~/ios/platform/user-notifications/deprecated/index.md) Doc für Weitere Informationen zum Hinzufügen von Unterstützung der Benachrichtigung zur iOS-app in der Projektmappe.

## <a name="creating-notification-controllers"></a>Erstellen von Notification-Controller

Für das Storyboard verfügen Benachrichtigungen Controller eine besondere Art von segue ausgelöst werden. Beim Ziehen Sie ein neues **Benachrichtigung Schnittstellencontroller** auf ein Storyboard wird es automatisch ein Segue angefügt haben:

![](notifications-images/notification-storyboard1.png "Eine neue Benachrichtigung Schnittstelle Controller mit einem angefügten segue")

Wenn die Benachrichtigung segue ausgewählt ist können Sie seine Eigenschaften bearbeiten:

![](notifications-images/notification-storyboard2.png "Die Benachrichtigung segue ausgewählten")

Sie können dieses Beispiels über die WatchKitCatalog formuliert werden, nachdem Sie den Controller angepasst haben:

![](notifications-images/notifications-segue.png "Die Benachrichtigungseigenschaften")


Es gibt zwei Arten der Benachrichtigung:

- **Kurzer Blick** -nicht scrollfähige statische Sicht, die vom System definiert.

- **Long-Wert-Darstellung** - Bildläufe anpassbare anzeigen, die von Ihnen definierten! Eine einfachere, statische und eine komplexere dynamische Version können angegeben werden.

### <a name="short-look-notification-controller"></a>Kurzer Blick Benachrichtigung Controller

Die Benutzeroberfläche kurzer Blick besteht das app-Symbol, Anwendungsnamen und Titelzeichenfolge Benachrichtigung ab.

Wenn der Benutzer die Benachrichtigung nicht ignoriert wird, schaltet das System automatisch eine Benachrichtigung Long-Wert-Darstellung, die Informationen bereitstellt.


### <a name="long-look-notification-controller"></a>Long-Wert-Darstellung Benachrichtigung Controller

Das Betriebssystem entscheidet, ob die statische oder dynamische basierend auf einer Reihe von Faktoren anzuzeigen. Sie müssen eine statische Schnittstelle bereitzustellen und können optional auch eine dynamische Schnittstelle für Benachrichtigungen einbeziehen.

#### <a name="static"></a>Statisch

Die statische Sicht sollte einfach und schnell angezeigt werden.

![](notifications-images/notification-static.png "Die statische Ansicht")

#### <a name="dynamic"></a>Dynamic

Die dynamische Ansicht weitere Daten anzeigen und weitere Interaktivität bereitzustellen.

![](notifications-images/notification-dynamic.png "Die dynamische Ansicht")


## <a name="generating-notifications"></a>Generieren von Benachrichtigungen

Benachrichtigungen können von einem Remoteserver stammen ([Apple Push Notifications Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), oder APNS) oder lokal in der iOS-app generiert werden.

Finden Sie in der [iOS Benachrichtigungen Exemplarische Vorgehensweise](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) ein Beispiel zum lokale Benachrichtigungen zu generieren und die [WatchNotifications Beispiel](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) ein funktionsfähiges Beispiel.

Lokale Benachrichtigungen benötigen die `AlertTitle` legen Sie auf der Apple Watch - angezeigt werden die `AlertTitle` Zeichenfolge wird in der Schnittstelle kurzer Blick angezeigt. Sowohl die `AlertTitle` und `AlertBody` in der Benachrichtigungsliste angezeigt werden und die `AlertBody` in der Long-suchen-Benutzeroberfläche angezeigt wird.

Diese bildschirmabbildung zeigt die `AlertTitle` in der Benachrichtigungsliste angezeigt wird und die `AlertBody` angezeigt, in der Long-suchen-Schnittstelle (mit der [Beispielcode](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)):

![](notifications-images/watch-notificationslist-sml.png "Diese bildschirmabbildung zeigt die AlertTitle angezeigt wird, in der Benachrichtigungsliste") ![ ] (notifications-images/watch-notificationcontroller-sml.png "der AlertBody angezeigt, in der Long-suchen-Schnittstelle")

## <a name="testing-notifications"></a>Testen von Benachrichtigungen

Benachrichtigungen (lokal und remote) können nur ordnungsgemäß getestet werden auf einem Gerät, aber sie simuliert werden können, mithilfe einer **JSON** Datei im iOS-Simulator.

### <a name="testing-on-apple-watch"></a>Auf der Apple Watch-Tests

Beachten Sie beim Testen von Benachrichtigungen in einer Apple Watch die [Apple Dokumentation](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) Folgendes:

> Wenn eine der Benachrichtigungen für Ihre app lokal oder remote auf einem iPhone des Benutzers eingeht, entscheidet iOS, ob dieser Benachrichtigung auf dem iPhone oder auf der Apple Watch anzeigen.

Dies ist die Tatsache alluding, iOS entscheidet, ob eine Benachrichtigung auf dem iPhone oder auf der Apple Watch angezeigt wird. Gepaarte iPhone aktiv ist, wenn eine Benachrichtigung empfangen wird, die Benachrichtigung ist wahrscheinlich auf dem iPhone angezeigt werden und *nicht* an der Apple Watch weitergeleitet.

Um sicherzustellen, dass die Benachrichtigung auf der Apple Watch angezeigt wird, deaktivieren Sie die iPhone-Bildschirm (drücken einmal den Netzschalter), oder lassen Sie ihn in den Energiesparmodus wechseln. Wenn der gepaarten Watch liegt im Bereich, mit Strom versorgt und wird auf Ihrem Handgelenk abgenutzt wird, die Benachrichtigung wird es weitergeleitet werden und auf der Apple Watch (beiliegen eine geringfügige) angezeigt werden.

### <a name="testing-on-the-ios-simulator"></a>Testen von IOS-Simulator

Sie *müssen* bieten eine JSON-Nutzlast ein Test beim Testen von Benachrichtigungsmodus im iOS-Simulator. Legen Sie den Pfad der **benutzerdefinierte Ausführungsargumente** Fenster in Visual Studio für Mac.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Visual Studio für Mac werden zusätzliche Optionen angezeigt, wenn eine Watch-Erweiterung, als festgelegt ist die **Startprojekt**.
Mit der rechten Maustaste auf das Erweiterungsprojekt überwachen, und wählen Sie **ausführen mit > benutzerdefinierte Parameter...** :
    
[![](notifications-images/runwith-customparams-sml.png "Mit der benutzerdefinierten Eigenschaften")](notifications-images/runwith-customparams.png)
    
Daraufhin wird die **Ausführungsargumente** Fenster enthält ein **WatchKit** Registerkarte. Wählen Sie **Benachrichtigung** , und geben Sie eine JSON-Nutzlast, drücken Sie dann die **Execute** Watch-app im Simulator zu starten:
    
[![](notifications-images/runwith-execargs-sml.png "Wählen Sie die Nutzlast Standard Benachrichtigung")](notifications-images/runwith-execargs.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Festzulegende die benachrichtigungsnutzlast Test in Visual Studio mit der rechten Maustaste auf die Watch-Erweiterung zum Bearbeiten der **Projekteigenschaften**. Wechseln Sie zu der **Debuggen** Abschnitt, und wählen Sie eine Benachrichtigungen JSON-Datei aus der Liste (es wird automatisch Listet alle JSON-Dateien, die im Projekt enthaltenen).
    
[![](notifications-images/runwith-execargs-sml-vs.png "Wählen Sie eine JSON-Datei von Benachrichtigungen")](notifications-images/runwith-execargs-vs.png)

Wenn die Watch-Erweiterung ist die **Startprojekt**, Visual Studio zeigt zusätzliche Optionen, wie unten dargestellt. Wählen Sie eines der **Benachrichtigung** Optionen so starten Sie die Watch-app in **Benachrichtigung** Modus (mit der JSON-Datei, die im Eigenschaftenfenster ausgewählt):
    
![](notifications-images/runwith-vs.png "Klicken Sie im Menü des Geräts")

-----

Der Standard-Benachrichtigung-Controller sieht wie folgt testen im Simulator mit der Standardeinstellung Nutzlast JSON-Datei:

![](notifications-images/notification-debug-sml.png "Eine Beispiel-Benachrichtigung")

Es ist auch möglich, verwenden Sie die [Befehlszeile](~/ios/watchos/troubleshooting.md#command_line) um iOS-Simulator zu starten.

### <a name="example-notification-payload"></a>Beispiel-Benachrichtigungsnutzlast

In der [überwachen Kit Katalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) Beispiel vorhanden ist, eine Nutzlast JSON-Beispieldatei **NotificationPayload.json** (siehe unten).

```csharp
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>Verwandte Links

- [WatchNotifications (lokale Benachrichtigungen) (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple Watch Kit Benachrichtigungen docs](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
