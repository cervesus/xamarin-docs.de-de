---
title: watchos-Benachrichtigungen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit watchos-Benachrichtigungen in xamarin arbeiten. Es wird erläutert, wie Benachrichtigungs Controller erstellt, Benachrichtigungen erstellt und Benachrichtigungen getestet werden.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 6be46d31ac2c16d02749519907d650588dbbcbe6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028225"
---
# <a name="watchos-notifications-in-xamarin"></a>watchos-Benachrichtigungen in xamarin

Watch-Apps können Benachrichtigungen empfangen, wenn Sie von der enthaltenen IOS-App unterstützt werden. Es gibt eine integrierte Benachrichtigungs Behandlung, sodass Sie die unten beschriebene zusätzliche Benachrichtigungs Unterstützung nicht hinzufügen *müssen* . Wenn Sie jedoch das Benachrichtigungs Verhalten und die Darstellung anpassen möchten, lesen Sie weiter.

Weitere Informationen zum Hinzufügen von Benachrichtigungs Unterstützung zur IOS-app in Ihrer Lösung finden Sie im [IOS-Benachrichtigungs](~/ios/platform/user-notifications/deprecated/index.md) Dokument.

## <a name="creating-notification-controllers"></a>Benachrichtigungs Controller werden erstellt

Auf den Storyboard-Benachrichtigungs Controllern gibt es eine besondere Art von "*", die diese auslöst. Wenn Sie einen neuen **Benachrichtigungs Schnittstellen Controller** auf ein Storyboard ziehen, wird ihm automatisch ein "*

![](notifications-images/notification-storyboard1.png "A new Notification Interface Controller with a segue attached")

Wenn der Benachrichtigungs-"*" ausgewählt ist, können Sie die zugehörigen Eigenschaften bearbeiten:

![](notifications-images/notification-storyboard2.png "The notification segue selected")

Nachdem Sie den Controller angepasst haben, kann er wie in diesem Beispiel aus dem watchkitcatalog aussehen:

![](notifications-images/notifications-segue.png "The Notification Properties")

Es gibt zwei Arten von Benachrichtigungen:

- Nicht **Bild** lauffähierbare statische Ansicht, die vom System definiert wird.

- Mit **langer Bild** lauffähigen, anpassbare Ansicht, die von Ihnen definiert wurde. Eine einfachere, statische Version und eine komplexere dynamische Version können angegeben werden.

### <a name="short-look-notification-controller"></a>Kurzbild-Benachrichtigungs Controller

Die kurzbild-Benutzeroberfläche besteht nur aus dem App-Symbol, dem APP-Namen und der Benachrichtigungs Titel Zeichenfolge.

Wenn der Benutzer die Benachrichtigung nicht ignoriert, wechselt das System automatisch zu einer lang Zeit Benachrichtigung, die weitere Informationen enthält.

### <a name="long-look-notification-controller"></a>Benachrichtigungs Controller mit langer Ausführungszeit

Das Betriebssystem entscheidet, ob die statische oder dynamische Ansicht basierend auf einer Reihe von Faktoren angezeigt werden soll. Sie müssen eine statische Schnittstelle bereitstellen, und optional können Sie auch eine dynamische Schnittstelle für Benachrichtigungen einschließen.

#### <a name="static"></a>Static

Die statische Ansicht sollte einfach und schnell angezeigt werden.

![](notifications-images/notification-static.png "The static view")

#### <a name="dynamic"></a>Dynamic

Die dynamische Ansicht kann mehr Daten anzeigen und mehr Interaktivität bereitstellen.

![](notifications-images/notification-dynamic.png "The dynamic view")

## <a name="generating-notifications"></a>Erstellen von Benachrichtigungen

Benachrichtigungen können von einem Remote Server ([Apple Push Notification Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), APNs) stammen oder lokal in der IOS-App generiert werden.

In der exemplarischen Vorgehensweise zu [IOS-Benachrichtigungen](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) finden Sie ein Beispiel für das Generieren von lokalen Benachrichtigungen und das [watchnotification](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications) -Beispiel für ein funktionierendes Beispiel.

Für lokale Benachrichtigungen muss die `AlertTitle` auf der Apple Watch angezeigt werden. die `AlertTitle` Zeichenfolge wird in der kurzbild Schnittstelle angezeigt. Die `AlertTitle` und `AlertBody` werden in der Benachrichtigungsliste angezeigt. und der `AlertBody` wird in der Schnittstelle mit langer Ausführungszeit angezeigt.

Dieser Screenshot zeigt die `AlertTitle`, die in der Benachrichtigungsliste angezeigt werden, und die `AlertBody`, die in der Schnittstelle für langes sehen angezeigt werden (mithilfe des [Beispielcodes](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)):

![](notifications-images/watch-notificationslist-sml.png "Dieser Screenshot zeigt, wie alerttitle in der Benachrichtigungsliste angezeigt wird.") ![](notifications-images/watch-notificationcontroller-sml.png "Der alertbody, der in der Schnittstelle mit langer Anzeige angezeigt wird.")

## <a name="testing-notifications"></a>Testen von Benachrichtigungen

Benachrichtigungen (sowohl lokal als auch Remote) können nur ordnungsgemäß auf einem Gerät getestet werden, Sie können jedoch mithilfe einer **JSON** -Datei im IOS-Simulator simuliert werden.

### <a name="testing-on-apple-watch"></a>Testen auf Apple Watch

Beachten Sie beim Testen von Benachrichtigungen auf einem Apple Watch, dass in der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) Folgendes angezeigt wird:

> Wenn eine der lokalen oder Remote Benachrichtigungen der APP auf dem iPhone des Benutzers eingeht, entscheidet IOS, ob diese Benachrichtigung auf dem iPhone oder in der Apple Watch angezeigt werden soll.

Dies bezieht sich auf die Tatsache, dass IOS entscheidet, ob eine Benachrichtigung auf dem iPhone oder auf der Uhr angezeigt wird. Wenn das gekoppelte iPhone aktiv ist, wenn eine Benachrichtigung empfangen wird, wird die Benachrichtigung wahrscheinlich auf dem iPhone angezeigt und *nicht* an die Überwachung weitergeleitet.

Um sicherzustellen, dass die Benachrichtigung auf der Überwachung angezeigt wird, schalten Sie den iPhone-Bildschirm aus (drücken Sie einmal die Taste), oder wechseln Sie zum Standbymodus. Wenn die gekoppelte Überwachung in Reichweite ist und auf dem Handgelenk beansprucht wird, wird die Benachrichtigung an die Hand geleitet und auf der Uhr angezeigt (mit einem geringfügigen).

### <a name="testing-on-the-ios-simulator"></a>Testen des IOS-Simulators

Sie *müssen* eine Test-JSON-Nutzlast angeben, wenn Sie im IOS-Simulator den Benachrichtigungs Modus testen. Legen Sie den Pfad im Fenster **benutzerdefinierte Ausführungs Argumente** in Visual Studio für Mac fest.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac zeigt zusätzliche Optionen an, wenn eine Watch-Erweiterung als **Startprojekt**festgelegt wird.
Klicken Sie mit der rechten Maustaste auf das Überwachungs Erweiterungsprojekt, und wählen Sie **mit > benutzerdefinierte Parameter ausführen...** aus:

[![](notifications-images/runwith-customparams-sml.png "Running with Custom Properties")](notifications-images/runwith-customparams.png#lightbox)

Dadurch wird das Fenster **Ausführungs Argumente** geöffnet, das eine **watchkit** -Registerkarte enthält. Wählen Sie **Benachrichtigung** aus, und geben Sie eine JSON-Nutzlast an, und drücken Sie dann **Execute** , um die Watch-App im Simulator

[![](notifications-images/runwith-execargs-sml.png "Select Notification Payload Default")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um die Test Benachrichtigungs Nutzlast in Visual Studio festzulegen, klicken Sie mit der rechten Maustaste auf die Überwachungs Erweiterung, um die **Projekteigenschaften**zu bearbeiten. Wechseln Sie zum Abschnitt " **Debuggen** ", und wählen Sie eine JSON-Benachrichtigungs Datei aus der Liste aus. (es werden automatisch alle im Projekt enthaltenen JSON-Dateien aufgelistet.)

[![](notifications-images/runwith-execargs-sml-vs.png "Select a notifications JSON file")](notifications-images/runwith-execargs-vs.png#lightbox)

Wenn die Watch-Erweiterung das **Startprojekt**ist, zeigt Visual Studio zusätzliche Optionen an, wie unten gezeigt. Wählen Sie eine der **Benachrichtigungs** Optionen aus, um die Watch-App im **Benachrichtigungs** Modus zu starten (mithilfe der im Eigenschaften Fenster ausgewählten JSON-Datei):

![](notifications-images/runwith-vs.png "The Device menu")

-----

Der standardmäßige Benachrichtigungs Controller sieht wie folgt aus, wenn Sie den Simulator mit der standardmäßigen Nutzlast-JSON-Datei testen:

![](notifications-images/notification-debug-sml.png "An example notification")

Es ist auch möglich, den IOS-Simulator mit der [Befehlszeile](~/ios/watchos/troubleshooting.md#command_line) zu starten.

### <a name="example-notification-payload"></a>Beispiel für Benachrichtigungs Nutzlast

Im [Watch Kit-Katalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) Beispiel finden Sie ein Beispiel für eine JSON-Nutz Last Datei **notificationpayload. JSON** (unten aufgeführt).

```json
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

- [Watchbenachrichtigungen (lokale Benachrichtigungen) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)
- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Benachrichtigungs Dokumentation für das Apple Watch Kit](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
