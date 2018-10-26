---
title: Einführung in watchOS
description: Dieses Dokument enthält eine Übersicht über WatchOS beschreiben den Lebenszyklus der Anwendung, Benutzer Schnittstellentypen, Bildschirmgrößen, Einschränkungen und mehr.
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: f000b75963eb7d517a124edd6f51a69b0f6ec93c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113195"
---
# <a name="introduction-to-watchos"></a>Einführung in watchOS

> [!NOTE]
> Sehen Sie sich die [Einführung in WatchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) eine Übersicht über die neuesten Funktionen.

## <a name="about-watchos"></a>Informationen zu watchOS

Eine WatchOS-app-Lösung verfügt über 3-Projekte:

- **Sehen Sie sich Erweiterung** – ein Projekt, das den Code für die Watch-app enthält.
- **Sehen Sie sich App** – enthält die Benutzeroberfläche Storyboard und Ressourcen.
- **iOS-App mit übergeordneten** : Diese app ist eine normale iPhone-app. Der Watch-app und die Erweiterung werden in der iPhone-app für die Übermittlung an des Benutzers ansehen gebündelt.

Klicken Sie in WatchOS 1-apps der Code in der Erweiterung ausgeführt wird, auf dem iPhone, – der Apple Watch ist effektiv einen externen Monitor aus. WatchOS 2 und 3-apps, die vollständig auf der Apple Watch ausgeführt werden. Dieser Unterschied wird im folgenden Diagramm dargestellt:

[ ![](intro-to-watchos-images/arch-sml.png "Der Unterschied zwischen WatchOS 1 und WatchOS 2 (und höher) wird in diesem Diagramm angezeigt.")](intro-to-watchos-images/arch.png#lightbox)

Unabhängig davon, welche WatchOS-Version ausgerichtet ist wird in Visual Studio für Mac Lösungspad eine vollständige Lösung wie folgt aussehen:

[![](intro-to-watchos-images/projectstructure-sml.png "Das Pad \"Projektmappe\"")](intro-to-watchos-images/projectstructure.png#lightbox)

Die *übergeordnete App* in einer WatchOS-Lösung ist eine reguläre iOS-app. Dies ist das einzige Projekt in der Projektmappe, die sichtbar ist **auf dem Telefon**. Anwendungsfälle für diese app würde es sich um Tutorials, Verwaltungsbildschirme und mittlerer Ebene filtern, Cacheing usw. enthalten. Es ist jedoch möglich, dass der Benutzer zum Installieren und Ausführen der app/der Watch-Erweiterungs ohne **jemals** müssen die übergeordnete app geöffnet haben, also, wenn Sie für die einmalige Initialisierung oder Verwaltung, Ausführung die übergeordneten Anwendung müssen auf Ihrer Apple Watch-Programm App-/ Erweiterung den Benutzer mitzuteilen.

Obwohl die übergeordnete app der Watch-app und die Erweiterung bietet, die Ausführung in unterschiedliche Sandboxes.

Auf WatchOS 1 können sie Daten mithilfe einer freigegebenen app-Gruppe oder über die statische Funktion freigeben `WKInterfaceController.OpenParentApplication`, die löst die `UIApplicationDelegate.HandleWatchKitExtensionRequest` -Methode in Ihrer übergeordneten app `AppDelegate` (finden Sie unter [arbeiten mit der übergeordneten app](~/ios/watchos/app-fundamentals/parent-app.md)).

Auf WatchOS 2 oder höher der Watch-Connectivity-Framework verwendet wird, um die Kommunikation mit dem übergeordneten-app, mit der `WCSession` Klasse.

## <a name="application-lifecycle"></a>Anwendungslebenszyklus

In der Watch-Erweiterung eine Unterklasse von der `WKInterfaceController` Klasse wird für jede Storyboard-Szene erstellt.

Diese `WKInterfaceController` Klassen sind analog zu den `UIViewController` Objekte in der iOS-Programmierung besitzen jedoch nicht der gleichen Ebene des Zugriffs auf die Ansicht.
Beispielsweise können nicht Sie dynamisch Steuerelemente hinzufügen, oder strukturieren Sie die Benutzeroberfläche.
Sie können, jedoch ausblenden und Anzeigen von Steuerelementen und, mit einige Steuerelemente, ändern ihre Größe, Transparenz und Optionen für die Darstellung.

Der Lebenszyklus einer `WKInterfaceController` Objekt umfasst die folgenden Aufrufe:

- [Aktiv](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : Sie sollten die meisten der Initialisierung in dieser Methode ausführen.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : wird aufgerufen, kurz bevor der Watch-App, die dem Benutzer angezeigt wird. Verwenden Sie diese Methode zum letzten Moment Initialisierung auszuführen, starten, Animationen usw. ein.
- An diesem Punkt der Watch-App angezeigt wird und die Erweiterung startet, reagieren auf Benutzer, die Eingabe, und aktualisieren die Anwendungslogik die Watch-App anzeigen.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) nach der Watch-App vom Benutzer verworfen wurde, diese Methode wird aufgerufen. Nach der Rückgabe dieser Methode nicht Steuerelemente der Benutzeroberfläche geändert werden, bis das nächste Mal `WillActivate` aufgerufen wird. Diese Methode wird auch aufgerufen werden, wenn die Verbindung mit dem iPhone, fehlerhaft ist.
- Nachdem die Erweiterung deaktiviert wurde, ist es für Ihr Programm nicht zugänglich. Ausstehende asynchrone Funktionen **nicht** aufgerufen werden. Sehen Sie sich, dass die Modi für die hintergrundverarbeitung Kit Erweiterungen nicht verwenden. Wenn das Programm wird durch den Benutzer erneut aktiviert, aber die app wurde nicht vom Betriebssystem beendet, wird die erste aufgerufene Methode `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Übersicht über den Anwendungslebenszyklus")

## <a name="types-of-user-interface"></a>Typen der Benutzeroberfläche

Es gibt drei Arten von Interaktion der Benutzer mit Ihrer app überwachen kann.
Alle werden so programmiert, verwenden benutzerdefinierte untergeordnete Klassen von `WKInterfaceController`, sodass die zuvor erläuterten Lebenszyklus Sequenz universell angewendet wird (Benachrichtigungen werden so programmiert, mit der untergeordnete Klassen von `WKUserNotificationController`, die selbst ist eine Unterklasse von `WKInterfaceController`):

### <a name="normal-interaction"></a>Normale Interaktion

Die Mehrheit der Interaktion der Watch-app-Erweiterung werden mit untergeordneten Klassen von `WKInterfaceController` , die Sie schreiben, um die Szenen in der Watch-app entsprechen **Interface.storyboard**. Dieser Vorgang wird ausführlich beschrieben die [Installation](~/ios/watchos/get-started/installation.md) und [Einstieg](~/ios/watchos/get-started/index.md) Artikel.
Die folgende Abbildung zeigt einen Teil der [Watch Kit Katalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) des Beispiels Storyboard. Für jede Szene hier gezeigt, besteht eine entsprechende benutzerdefinierte `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`usw.) in das Erweiterungsprojekt.

![](intro-to-watchos-images/scenes.png "Beispiele für die Interaktion mit normalen")

### <a name="notifications"></a>Benachrichtigungen

[Benachrichtigungen](~/ios/watchos/platform/notifications.md) sind eine wichtige Anwendungsfälle für die Apple Watch. Sowohl lokale als auch Benachrichtigungen werden unterstützt. Interaktion mit Benachrichtigungen erfolgt in zwei Phasen, kurze und lange aussehen aufgerufen.

Kurze sieht kurz angezeigt werden, und das Watch-app-Symbol, den Namen und den Titel anzeigen (gemäß der Angabe `WKInterfaceController.SetTitle`).

Kombiniert die lange suchen, eine vom System bereitgestellte **Bildtrennlinie** Bereichs- und Schaltfläche "verwerfen" mit benutzerdefinierten Storyboard-basierten Inhalt.

`WKUserNotificationInterfaceController` Erweitert `WKInterfaceController` mit den Methoden `DidReceiveLocalNotification` und `DidReceiveRemoteNotification`.
Überschreiben Sie diese Methoden, um auf Benachrichtigung über Ereignisse zu reagieren.

Weitere Informationen zum Entwurf der Benutzeroberfläche der Benachrichtigung, finden Sie in der [Apple Watch Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Beispiel-Benachrichtigungen")

## <a name="screen-sizes"></a>Bildschirmgrößen

Der Apple Watch-verfügt über zwei Gesicht Größen: 38mm "und" 42mm, mit einem Seitenverhältnis von 5:4 und ein Retina-Display. Die Größen verwendet werden:

- 38 mm: 136 x 170 logischen Pixeln (272 340 x physische Pixel)
- 42 mm: 156 x 195 logischen Pixeln (312 x 390 physische Pixel).

Verwendung `WKInterfaceDevice.ScreenBounds` um zu bestimmen, auf welche die Watch-App ausgeführt wird.

Im Allgemeinen ist es einfacher, entwickeln Ihren Entwurf Text und Layout, mit der stärker eingeschränkten 38mm-Anzeige, und klicken Sie dann zentral hochskalieren.
Wenn Sie mit der größeren Umgebung starten, kann zentral Herunterskalieren zu hässlichen überlappungen oder Abschneiden von Text führen.

Erfahren Sie mehr über [arbeiten mit Bildschirmen](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>Einschränkungen für watchOS

Es gibt einige Einschränkungen der WatchOS beim Entwickeln von apps für WatchOS berücksichtigen:

- Apple Watch-Geräte nur über begrenzte Storage – berücksichtigen Sie den verfügbaren Speicherplatz vor dem Herunterladen großer Dateien (z. b. Audio- oder Movie-Dateien).

- Viele WatchOS [Steuerelemente](~/ios/watchos/user-interface/index.md) analoge in UIKit, jedoch verschiedenen Klassen (`WKInterfaceButton` statt `UIButton`, `WKInterfaceSwitch` für `UISwitch`usw.) und haben eine begrenzte Anzahl von Methoden, die im Vergleich zu ihren UIKit Entsprechungen. Darüber hinaus WatchOS verfügt über einige Steuerelemente wie z. B. `WKInterfaceDate` (für die Anzeige von Datum und Uhrzeit), UIKit verfügt nicht über.

  - Sie können nicht weiterleiten, Benachrichtigungen, um nur die Überwachung oder das iPhone nur (welche Art von Steuerelement der Benutzer über das routing wurde nicht von Apple angekündigt wurde).

Einige andere bekannten Einschränkungen / häufig gestellte Fragen:

- Apple lässt sich nicht auf benutzerdefinierte 3rd-Party-watchfaces aus.

- Die APIs, mit die die Apple Watch iTunes auf dem verbundenen Telefon steuern können, sind privat.


## <a name="further-reading"></a>Weiterführende Themen

Testen Sie die Dokumentation von Apple aus:

* [Entwickeln für die Watch-Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Sehen Sie sich Kit-Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Richtlinien für die interaktive Benutzeroberfläche von Apple Watch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>Verwandte Links

- [WatchOS 3 Catalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchOS 1 Catalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Setup und Installation](~/ios/watchos/get-started/installation.md)
- [Erste Watch-App-video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Sehen Sie sich Kit Anleitung des Apple entwickeln.](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple WatchKit-Tipps](https://developer.apple.com/watchkit/tips/)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
