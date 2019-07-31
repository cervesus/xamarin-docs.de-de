---
title: WatchOS-Benachrichtigungen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit WatchOS-Benachrichtigungen in Xamarin. Es wird erläutert, erstellen Notification-Controller, Generieren von Benachrichtigungen und Benachrichtigungen zu testen.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: a3273b4bed13c3982b9d9b4df874e4ad2ee30e3f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645918"
---
# <a name="watchos-notifications-in-xamarin"></a>WatchOS-Benachrichtigungen in Xamarin

Watch-apps können Benachrichtigungen empfangen, wenn diese von die enthaltenden iOS-app unterstützt. Integrierten Benachrichtigungen zu behandeln, damit Sie nicht der Fall ist *müssen* die unten beschriebene, jedoch zusätzliche Benachrichtigung-Unterstützung hinzufügen. Wenn Sie Benachrichtigungsverhalten anpassen möchten, und Darstellung dann lesen Sie weiter.

Finden Sie in der [iOS-Benachrichtigungen](~/ios/platform/user-notifications/deprecated/index.md) Doc für Weitere Informationen zum Hinzufügen von Unterstützung der iOS-App in der Projektmappe.

## <a name="creating-notification-controllers"></a>Erstellen Notification-Controller

Auf dem Storyboard haben Benachrichtigungen Controller eine besondere Art von segue ausgelöst werden. Wenn Sie ein neues ziehen **Benachrichtigungscontrollers-Schnittstelle** auf ein Storyboard verfügen sie automatisch über einem Segue angefügt:

![](notifications-images/notification-storyboard1.png "Eine neue Benachrichtigung Schnittstellencontrollers mit einem Segue angefügt")

Wenn die Benachrichtigung segue ausgewählt ist können Sie seine Eigenschaften bearbeiten:

![](notifications-images/notification-storyboard2.png "Die Benachrichtigung segue ausgewählten")

Nachdem Sie den Controller angepasst haben, kann er wie im folgenden Beispiel aus der WatchKitCatalog aussehen:

![](notifications-images/notifications-segue.png "Die Eigenschaften der Benachrichtigungen")


Es gibt zwei Arten von Benachrichtigungen:

- **Kurze aussehen** -nicht bildlauffähige statische Ansicht, die vom System definierte.

- **Lange aussehen** – ScrollBar, anpassbare Ansicht, die von Ihnen festgelegten! Eine einfachere, statischen und eine komplexere dynamische Version können angegeben werden.

### <a name="short-look-notification-controller"></a>Sehen Sie kurze Benachrichtigungscontrollers aus

Die kurze aussehen Benutzeroberfläche besteht aus nur das Symbol der app, app-Namen und die Benachrichtigung Titelzeichenfolge.

Wenn der Benutzer die Benachrichtigung nicht ignoriert wird, wechselt das System automatisch eine Benachrichtigung lang aussehen, die Informationen bereitstellt.


### <a name="long-look-notification-controller"></a>Sehen Sie lange Benachrichtigungscontrollers aus

Das Betriebssystem entscheidet, ob die statische oder dynamische Ansicht basierend auf einer Reihe von Faktoren ab. Müssen Sie eine statische Schnittstelle bereitzustellen und können optional auch eine dynamische Schnittstelle für Benachrichtigungen enthalten.

#### <a name="static"></a>Statisch

Die statische Ansicht sollte einfach und schnell in angezeigt.

![](notifications-images/notification-static.png "Die statische Ansicht")

#### <a name="dynamic"></a>Dynamic

Die dynamische Ansicht kann weitere Daten anzeigen und Bereitstellen von interaktiver gestalten.

![](notifications-images/notification-dynamic.png "Die dynamische Ansicht")


## <a name="generating-notifications"></a>Generieren von Benachrichtigungen

Benachrichtigungen können von einem remote-Server stammen ([Apple Push Notifications Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), oder APNS) oder lokal in der iOS-app generiert werden.

Finden Sie in der [iOS Exemplarische Vorgehensweise zu Benachrichtigungen](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) für verdeutlicht, wie Sie lokale Benachrichtigungen zu generieren und die [WatchNotifications Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications) ein funktionsfähiges Beispiel.

Lokale Benachrichtigungen benötigen die `AlertTitle` legen Sie auf der Apple Watch - angezeigt werden die `AlertTitle` Zeichenfolge wird in der Schnittstelle aussehen kurz angezeigt. Sowohl die `AlertTitle` und `AlertBody` werden angezeigt, in der Benachrichtigungsliste enthalten; und die `AlertBody` wird angezeigt, in der Long-Wert-suchen-Schnittstelle.

Dieser Screenshot zeigt die `AlertTitle` in der Benachrichtigungsliste angezeigt wird und die `AlertBody` angezeigt, in der Long-Wert-suchen-Schnittstelle (mithilfe der [Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)):

![](notifications-images/watch-notificationslist-sml.png "Dieser Screenshot zeigt die angezeigt wird, in der Benachrichtigungsliste enthalten %alerttitle") ![](notifications-images/watch-notificationcontroller-sml.png "der AlertBody angezeigt, in der Long-Wert-suchen-Schnittstelle")

## <a name="testing-notifications"></a>Testen von Benachrichtigungen

Benachrichtigungen (lokal und remote) können nur ordnungsgemäß getestet werden auf einem Gerät, aber sie simuliert werden können, mit einem **JSON** -Datei in den iOS-Simulator.

### <a name="testing-on-apple-watch"></a>Tests auf der Apple Watch

Wenn Sie Benachrichtigungen auf einem Apple Watch zu testen, beachten Sie, dass [Apple Dokumentation](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) sagt dazu Folgendes:

> Wenn eine der Benachrichtigungen für Ihre app lokal oder remote auf einem iPhone des Benutzers eingeht, entscheidet iOS angibt, ob diese Benachrichtigung auf dem iPhone oder auf der Apple Watch angezeigt.

Dies ist die Tatsache alluding, iOS entscheidet, ob eine Benachrichtigung auf dem iPhone oder auf der Apple Watch angezeigt wird. Die Benachrichtigung ist wahrscheinlich auf dem iPhone angezeigt werden, wenn das gekoppelte iPhone aktiv ist, wenn eine Benachrichtigung empfangen wird, und *nicht* weitergeleitet, auf der Apple Watch.

Um sicherzustellen, dass die Benachrichtigung auf der Apple Watch angezeigt wird, deaktivieren Sie die iPhone-Bildschirm (den Netzschalter einmal drücken), oder lassen Sie ihn in den Ruhezustand versetzt. Wenn die gekoppelte Apple Watch befindet sich im Bereich, mit Strom versorgt und ist auf die Finger abgenutzt wird, die Benachrichtigung gibt es weitergeleitet werden und auf der Apple Watch (zusammen mit einem feine) angezeigt werden.

### <a name="testing-on-the-ios-simulator"></a>Auf dem iOS-Simulator testen

Sie *müssen* Geben Sie eine Test-JSON-Nutzlast aus, wenn der Benachrichtigungsmodus im iOS-Simulator testen. Legen Sie den Pfad der **benutzerdefinierte Ausführungsargumente** Fenster in Visual Studio für Mac.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac werden zusätzliche Optionen angezeigt, wenn eine Watch-Erweiterung festgelegt ist die **Startprojekt**.
Mit der rechten Maustaste auf das Projekt der Watch-Erweiterung, und wählen Sie **ausführen mit > benutzerdefinierte Parameter...** :
    
[![](notifications-images/runwith-customparams-sml.png "Mit benutzerdefinierten Eigenschaften")](notifications-images/runwith-customparams.png#lightbox)
    
Daraufhin wird die **Ausführungsargumente** Fenster enthält einen **WatchKit** Registerkarte. Wählen Sie **Benachrichtigung** , und geben Sie eine JSON-Nutzlast, drücken Sie dann die **Execute** auf die Watch-app im Simulator zu starten:
    
[![](notifications-images/runwith-execargs-sml.png "Wählen Sie Notification Payload-Standard")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Zum Festlegen von der benachrichtigungsnutzlast Test in Visual Studio mit der rechten Maustaste auf der Watch-Erweiterung zum Bearbeiten der **Projekteigenschaften**. Wechseln Sie zu der **Debuggen** aus, und wählen Sie eine JSON-Datei von Benachrichtigungen aus der Liste (es wird automatisch Listet alle JSON-Dateien im Projekt enthalten ist).
    
[![](notifications-images/runwith-execargs-sml-vs.png "Wählen Sie eine JSON-Datei von Benachrichtigungen")](notifications-images/runwith-execargs-vs.png#lightbox)

Wenn die Watch-Erweiterung ist die **Startprojekt**, Visual Studio zeigt zusätzliche Optionen, wie unten dargestellt. Wählen Sie eine der der **Benachrichtigung** Optionen die Watch-app zu starten, in **Benachrichtigung** Modus (mit der JSON-Datei, die im Eigenschaftenfenster ausgewählt):
    
![](notifications-images/runwith-vs.png "Klicken Sie im Menü des Geräts")

-----

Standardcontroller Benachrichtigung sieht folgendermaßen aus, auf dem Simulator mit der Standard-Nutzlast JSON-Datei zu testen:

![](notifications-images/notification-debug-sml.png "Eine Beispiel-Benachrichtigung")

Es ist auch möglich, mit der [Befehlszeile](~/ios/watchos/troubleshooting.md#command_line) iOS-Simulator zu starten.

### <a name="example-notification-payload"></a>Beispiel-Benachrichtigungsnutzlast

In der [Watch Kit Katalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) Beispiel gibt es ist eine JSON-Beispieldatei Nutzlast **NotificationPayload.json** (siehe unten).

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

- [WatchNotifications (lokale Benachrichtigungen) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)
- [WatchKitCatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple Watch-Kit-Benachrichtigungen-Dokumentation](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
