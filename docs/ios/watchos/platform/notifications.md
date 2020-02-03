---
title: WatchOS-Benachrichtigungen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit WatchOS-Benachrichtigungen in Xamarin. Es wird erläutert, erstellen Notification-Controller, Generieren von Benachrichtigungen und Benachrichtigungen zu testen.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 85a55967446da5cf89e8ce19dadf88d0de16d80a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725069"
---
# <a name="watchos-notifications-in-xamarin"></a>WatchOS-Benachrichtigungen in Xamarin

Watch-apps können Benachrichtigungen empfangen, wenn diese von die enthaltenden iOS-app unterstützt. Es gibt eine integrierte Benachrichtigungs Behandlung, sodass Sie die unten beschriebene zusätzliche Benachrichtigungs Unterstützung nicht hinzufügen *müssen* . Wenn Sie jedoch das Benachrichtigungs Verhalten und die Darstellung anpassen möchten, lesen Sie weiter.

Weitere Informationen zum Hinzufügen von Benachrichtigungs Unterstützung zur IOS-app in Ihrer Lösung finden Sie im [IOS-Benachrichtigungs](~/ios/platform/user-notifications/deprecated/index.md) Dokument.

## <a name="creating-notification-controllers"></a>Erstellen Notification-Controller

Auf dem Storyboard haben Benachrichtigungen Controller eine besondere Art von segue ausgelöst werden. Wenn Sie einen neuen **Benachrichtigungs Schnittstellen Controller** auf ein Storyboard ziehen, wird ihm automatisch ein "*

![](notifications-images/notification-storyboard1.png "A new Notification Interface Controller with a segue attached")

Wenn die Benachrichtigung segue ausgewählt ist können Sie seine Eigenschaften bearbeiten:

![](notifications-images/notification-storyboard2.png "The notification segue selected")

Nachdem Sie den Controller angepasst haben, kann er wie im folgenden Beispiel aus der WatchKitCatalog aussehen:

![](notifications-images/notifications-segue.png "The Notification Properties")

Es gibt zwei Arten von Benachrichtigungen:

- Nicht **Bild** lauffähierbare statische Ansicht, die vom System definiert wird.

- Mit **langer Bild** lauffähigen, anpassbare Ansicht, die von Ihnen definiert wurde. Eine einfachere, statischen und eine komplexere dynamische Version können angegeben werden.

### <a name="short-look-notification-controller"></a>Sehen Sie kurze Benachrichtigungscontrollers aus

Die kurze aussehen Benutzeroberfläche besteht aus nur das Symbol der app, app-Namen und die Benachrichtigung Titelzeichenfolge.

Wenn der Benutzer die Benachrichtigung nicht ignoriert wird, wechselt das System automatisch eine Benachrichtigung lang aussehen, die Informationen bereitstellt.

### <a name="long-look-notification-controller"></a>Sehen Sie lange Benachrichtigungscontrollers aus

Das Betriebssystem entscheidet, ob die statische oder dynamische Ansicht basierend auf einer Reihe von Faktoren ab. Müssen Sie eine statische Schnittstelle bereitzustellen und können optional auch eine dynamische Schnittstelle für Benachrichtigungen enthalten.

#### <a name="static"></a>statischen

Die statische Ansicht sollte einfach und schnell in angezeigt.

![](notifications-images/notification-static.png "The static view")

#### <a name="dynamic"></a>Dynamisch

Die dynamische Ansicht kann weitere Daten anzeigen und Bereitstellen von interaktiver gestalten.

![](notifications-images/notification-dynamic.png "The dynamic view")

## <a name="generating-notifications"></a>Generieren von Benachrichtigungen

Benachrichtigungen können von einem Remote Server stammen oder lokal in der IOS-App generiert werden.

Ein Beispiel für das Generieren von lokalen Benachrichtigungen finden Sie in der exemplarischen Vorgehensweise zu [IOS-Benachrichtigungen](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) .

Für lokale Benachrichtigungen muss die `AlertTitle` auf der Apple Watch angezeigt werden. die `AlertTitle` Zeichenfolge wird in der kurzbild Schnittstelle angezeigt. Die `AlertTitle` und `AlertBody` werden in der Benachrichtigungsliste angezeigt. und der `AlertBody` wird in der Schnittstelle mit langer Ausführungszeit angezeigt.

Dieser Screenshot zeigt die `AlertTitle`, die in der Benachrichtigungsliste angezeigt werden, und die `AlertBody`, die in der Schnittstelle für langes sehen angezeigt werden:

![](notifications-images/watch-notificationslist-sml.png "Dieser Screenshot zeigt, wie alerttitle in der Benachrichtigungsliste angezeigt wird.")![](notifications-images/watch-notificationcontroller-sml.png "Der alertbody, der in der Schnittstelle mit langer Anzeige angezeigt wird.")

## <a name="testing-notifications"></a>Testen von Benachrichtigungen

Benachrichtigungen (sowohl lokal als auch Remote) können nur ordnungsgemäß auf einem Gerät getestet werden, Sie können jedoch mithilfe einer **JSON** -Datei im IOS-Simulator simuliert werden.

### <a name="testing-on-apple-watch"></a>Tests auf der Apple Watch

Beachten Sie beim Testen von Benachrichtigungen auf einem Apple Watch, dass in der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) Folgendes angezeigt wird:

> Wenn eine der Benachrichtigungen für Ihre app lokal oder remote auf einem iPhone des Benutzers eingeht, entscheidet iOS angibt, ob diese Benachrichtigung auf dem iPhone oder auf der Apple Watch angezeigt.

Dies ist die Tatsache alluding, iOS entscheidet, ob eine Benachrichtigung auf dem iPhone oder auf der Apple Watch angezeigt wird. Wenn das gekoppelte iPhone aktiv ist, wenn eine Benachrichtigung empfangen wird, wird die Benachrichtigung wahrscheinlich auf dem iPhone angezeigt und *nicht* an die Überwachung weitergeleitet.

Um sicherzustellen, dass die Benachrichtigung auf der Apple Watch angezeigt wird, deaktivieren Sie die iPhone-Bildschirm (den Netzschalter einmal drücken), oder lassen Sie ihn in den Ruhezustand versetzt. Wenn die gekoppelte Apple Watch befindet sich im Bereich, mit Strom versorgt und ist auf die Finger abgenutzt wird, die Benachrichtigung gibt es weitergeleitet werden und auf der Apple Watch (zusammen mit einem feine) angezeigt werden.

### <a name="testing-on-the-ios-simulator"></a>Auf dem iOS-Simulator testen

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

Standardcontroller Benachrichtigung sieht folgendermaßen aus, auf dem Simulator mit der Standard-Nutzlast JSON-Datei zu testen:

![](notifications-images/notification-debug-sml.png "An example notification")

Es ist auch möglich, den IOS-Simulator mit der [Befehlszeile](~/ios/watchos/troubleshooting.md#command_line) zu starten.

### <a name="example-notification-payload"></a>Beispiel-Benachrichtigungsnutzlast

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

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Benachrichtigungs Dokumentation für das Apple Watch Kit](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
