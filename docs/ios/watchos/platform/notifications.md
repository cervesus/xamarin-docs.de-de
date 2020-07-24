---
title: watchos-Benachrichtigungen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit watchos-Benachrichtigungen in xamarin arbeiten. Es wird erläutert, wie Benachrichtigungs Controller erstellt, Benachrichtigungen erstellt und Benachrichtigungen getestet werden.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 0358b2b422e4cc69faa15187ee24d72c7d02ca38
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937929"
---
# <a name="watchos-notifications-in-xamarin"></a>watchos-Benachrichtigungen in xamarin

Watch-Apps können Benachrichtigungen empfangen, wenn Sie von der enthaltenen IOS-App unterstützt werden. Es gibt eine integrierte Benachrichtigungs Behandlung, sodass Sie die unten beschriebene zusätzliche Benachrichtigungs Unterstützung nicht hinzufügen *müssen* . Wenn Sie jedoch das Benachrichtigungs Verhalten und die Darstellung anpassen möchten, lesen Sie weiter.

Weitere Informationen zum Hinzufügen von Benachrichtigungs Unterstützung zur IOS-app in Ihrer Lösung finden Sie im [IOS-Benachrichtigungs](~/ios/platform/user-notifications/deprecated/index.md) Dokument.

## <a name="creating-notification-controllers"></a>Benachrichtigungs Controller werden erstellt

Auf den Storyboard-Benachrichtigungs Controllern gibt es eine besondere Art von "*", die diese auslöst. Wenn Sie einen neuen **Benachrichtigungs Schnittstellen Controller** auf ein Storyboard ziehen, wird ihm automatisch ein "*

![Ein neuer Benachrichtigungs Schnittstellen Controller mit angefügtem "*".](notifications-images/notification-storyboard1.png)

Wenn der Benachrichtigungs-"*" ausgewählt ist, können Sie die zugehörigen Eigenschaften bearbeiten:

![Der ausgewählte Benachrichtigungs-*](notifications-images/notification-storyboard2.png)

Nachdem Sie den Controller angepasst haben, kann er wie in diesem Beispiel aus dem watchkitcatalog aussehen:

![Benachrichtigungs Eigenschaften](notifications-images/notifications-segue.png)

Es gibt zwei Arten von Benachrichtigungen:

- Nicht **Bild** lauffähierbare statische Ansicht, die vom System definiert wird.

- Mit **langer Bild** lauffähigen, anpassbare Ansicht, die von Ihnen definiert wurde. Eine einfachere, statische Version und eine komplexere dynamische Version können angegeben werden.

### <a name="short-look-notification-controller"></a>Kurzbild-Benachrichtigungs Controller

Die kurzbild-Benutzeroberfläche besteht nur aus dem App-Symbol, dem APP-Namen und der Benachrichtigungs Titel Zeichenfolge.

Wenn der Benutzer die Benachrichtigung nicht ignoriert, wechselt das System automatisch zu einer lang Zeit Benachrichtigung, die weitere Informationen enthält.

### <a name="long-look-notification-controller"></a>Benachrichtigungs Controller mit langer Ausführungszeit

Das Betriebssystem entscheidet, ob die statische oder dynamische Ansicht basierend auf einer Reihe von Faktoren angezeigt werden soll. Sie müssen eine statische Schnittstelle bereitstellen, und optional können Sie auch eine dynamische Schnittstelle für Benachrichtigungen einschließen.

#### <a name="static"></a>statischen

Die statische Ansicht sollte einfach und schnell angezeigt werden.

![Die statische Ansicht](notifications-images/notification-static.png)

#### <a name="dynamic"></a>Dynamisch

Die dynamische Ansicht kann mehr Daten anzeigen und mehr Interaktivität bereitstellen.

![Die dynamische Ansicht](notifications-images/notification-dynamic.png)

## <a name="generating-notifications"></a>Erstellen von Benachrichtigungen

Benachrichtigungen können von einem Remote Server stammen oder lokal in der IOS-App generiert werden.

Ein Beispiel für das Generieren von lokalen Benachrichtigungen finden Sie in der exemplarischen Vorgehensweise zu [IOS-Benachrichtigungen](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) .

Lokale Benachrichtigungen müssen den `AlertTitle` Satz aufweisen, der auf dem Apple Watch angezeigt wird `AlertTitle` . die Zeichenfolge wird in der kurzbild Schnittstelle angezeigt. Sowohl `AlertTitle` als auch `AlertBody` werden in der Benachrichtigungsliste angezeigt, und der `AlertBody` wird in der Schnittstelle mit langer Anzeige angezeigt.

Dieser Screenshot zeigt `AlertTitle` , welche in der Benachrichtigungsliste angezeigt wird, und die `AlertBody` in der Schnittstelle mit langer Anzeige angezeigt wird:

![Dieser Screenshot zeigt, wie alerttitle in der Benachrichtigungsliste angezeigt wird.](notifications-images/watch-notificationslist-sml.png) ![Der alertbody, der in der Schnittstelle mit langer Anzeige angezeigt wird.](notifications-images/watch-notificationcontroller-sml.png)

## <a name="testing-notifications"></a>Testen von Benachrichtigungen

Benachrichtigungen (sowohl lokal als auch Remote) können nur ordnungsgemäß auf einem Gerät getestet werden, Sie können jedoch mithilfe einer **JSON** -Datei im IOS-Simulator simuliert werden.

### <a name="testing-on-apple-watch"></a>Testen auf Apple Watch

Beachten Sie beim Testen von Benachrichtigungen auf einem Apple Watch, dass in der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) Folgendes angezeigt wird:

> Wenn eine der lokalen oder Remote Benachrichtigungen der APP auf dem iPhone des Benutzers eingeht, entscheidet IOS, ob diese Benachrichtigung auf dem iPhone oder in der Apple Watch angezeigt werden soll.

Dies bezieht sich auf die Tatsache, dass IOS entscheidet, ob eine Benachrichtigung auf dem iPhone oder auf der Uhr angezeigt wird. Wenn das gekoppelte iPhone aktiv ist, wenn eine Benachrichtigung empfangen wird, wird die Benachrichtigung wahrscheinlich auf dem iPhone angezeigt und *nicht* an die Überwachung weitergeleitet.

Um sicherzustellen, dass die Benachrichtigung auf der Überwachung angezeigt wird, schalten Sie den iPhone-Bildschirm aus (drücken Sie einmal die Taste), oder wechseln Sie zum Standbymodus. Wenn die gekoppelte Überwachung in Reichweite ist und auf dem Handgelenk beansprucht wird, wird die Benachrichtigung an die Hand geleitet und auf der Uhr angezeigt (mit einem geringfügigen).

### <a name="testing-on-the-ios-simulator"></a>Testen des IOS-Simulators

Sie *müssen* eine Test-JSON-Nutzlast angeben, wenn Sie im IOS-Simulator den Benachrichtigungs Modus testen. Legen Sie den Pfad im Fenster **benutzerdefinierte Ausführungs Argumente** in Visual Studio für Mac fest.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac zeigt zusätzliche Optionen an, wenn eine Watch-Erweiterung als **Startprojekt**festgelegt wird.
Klicken Sie mit der rechten Maustaste auf das Überwachungs Erweiterungsprojekt, und wählen Sie **mit > benutzerdefinierte Parameter ausführen...** aus:

[![Ausführen mit benutzerdefinierten Eigenschaften](notifications-images/runwith-customparams-sml.png)](notifications-images/runwith-customparams.png#lightbox)

Dadurch wird das Fenster **Ausführungs Argumente** geöffnet, das eine **watchkit** -Registerkarte enthält. Wählen Sie **Benachrichtigung** aus, und geben Sie eine JSON-Nutzlast an, und drücken Sie dann **Execute** , um die Watch-App im Simulator

[![Standardwert für Benachrichtigungs Nutzlast auswählen](notifications-images/runwith-execargs-sml.png)](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Um die Test Benachrichtigungs Nutzlast in Visual Studio festzulegen, klicken Sie mit der rechten Maustaste auf die Überwachungs Erweiterung, um die **Projekteigenschaften**zu bearbeiten. Wechseln Sie zum Abschnitt " **Debuggen** ", und wählen Sie eine JSON-Benachrichtigungs Datei aus der Liste aus. (es werden automatisch alle im Projekt enthaltenen JSON-Dateien aufgelistet.)

[![JSON-Datei für Benachrichtigungen auswählen](notifications-images/runwith-execargs-sml-vs.png)](notifications-images/runwith-execargs-vs.png#lightbox)

Wenn die Watch-Erweiterung das **Startprojekt**ist, zeigt Visual Studio zusätzliche Optionen an, wie unten gezeigt. Wählen Sie eine der **Benachrichtigungs** Optionen aus, um die Watch-App im **Benachrichtigungs** Modus zu starten (mithilfe der im Eigenschaften Fenster ausgewählten JSON-Datei):

![Das Geräte Menü](notifications-images/runwith-vs.png)

-----

Der standardmäßige Benachrichtigungs Controller sieht wie folgt aus, wenn Sie den Simulator mit der standardmäßigen Nutzlast-JSON-Datei testen:

![Beispiel Benachrichtigung](notifications-images/notification-debug-sml.png)

Es ist auch möglich, den IOS-Simulator mit der [Befehlszeile](~/ios/watchos/troubleshooting.md#command_line) zu starten.

### <a name="example-notification-payload"></a>Beispiel für Benachrichtigungs Nutzlast

Im [Watch Kit-Katalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) Beispiel finden Sie ein Beispiel für eine JSON-Nutz Last Datei **NotificationPayload.js** (siehe unten).

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

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Benachrichtigungs Dokumentation für das Apple Watch Kit](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
