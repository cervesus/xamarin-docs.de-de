---
title: "Einführung in watchOS"
description: "Übersicht über die WatchOS Projektmappenstruktur und Einschränkungen"
ms.topic: article
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2276b67fc29f2752e4b178168a12e6e980b788d0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-watchos"></a>Einführung in watchOS

> [!NOTE]
> **Hinweis:** sehen Sie sich die [Einführung in WatchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) eine Übersicht über die neuesten Funktionen.

## <a name="about-watchos"></a>Informationen zu watchOS

Eine WatchOS app-Lösung beinhaltet 3-Projekte:

- **Überwachen Sie die Erweiterung** – ein Projekt, das den Code für die Watch-app enthält.
- **Überwachen der App** – enthält die Benutzeroberfläche Storyboard und Ressourcen.
- **iOS-App übergeordneten** – diese app ist eine normale iPhone-app. Der Watch-app und die Erweiterung sind in der iPhone-app für die Übermittlung an den Benutzer überwachen gebündelt.

Der Code in der Erweiterung ausgeführt wird, auf dem iPhone WatchOS 1-apps – der Apple Watch ist tatsächlich eine externe anzeigen. WatchOS 2 und 3-apps werden auf der Apple Watch vollständig ausgeführt. Dieser Unterschied wird im folgenden Diagramm dargestellt:

[ ![](intro-to-watchos-images/arch-sml.png "Der Unterschied zwischen WatchOS 1 und WatchOS 2 (und höher) ist in diesem Diagramm angezeigt.")](intro-to-watchos-images/arch.png#lightbox)

Unabhängig davon, welche Version von WatchOS gerichtet ist wird in Visual Studio für Mac Lösung eine vollständige Lösung etwa wie folgt aussehen:

[![](intro-to-watchos-images/projectstructure-sml.png "Die Lösung mit Leerstellen auffüllen")](intro-to-watchos-images/projectstructure.png#lightbox)

Die *übergeordnete App* in einer WatchOS Lösung ist eine reguläre iOS-app. Dies ist das einzige Projekt in der Projektmappe, die sichtbar ist **auf dem Telefon**. Anwendungsfälle für diese app würde Lernprogramme, Verwaltungsbildschirme und mittlerer Ebene filtern, Cacheing usw. enthalten. Allerdings ist es möglich, dass der Benutzer zum Installieren und Ausführen der Watch-app/Erweiterungs ohne **jemals** müssen die übergeordnete app geöffnet haben, also wenn Sie für die einmalige Initialisierung oder Verwaltung, Ausführung die übergeordneten Anwendung müssen so programmieren Sie Ihre Überwachung App/Erweiterung, den Benutzer, die.

Obwohl die übergeordnete app Watch-app und die Erweiterung der übermittelt, sie in anderen Sandkästen ausgeführt werden.

Auf WatchOS 1 können sie Daten über ein freigegebenes app-Gruppe oder die statische Funktion freigeben `WKInterfaceController.OpenParentApplication`, durch die ausgelöst wird die `UIApplicationDelegate.HandleWatchKitExtensionRequest` Methode in der übergeordneten app `AppDelegate` (finden Sie unter [arbeiten mit der übergeordneten app](~/ios/watchos/app-fundamentals/parent-app.md)).

Auf das Überwachungsfenster Konnektivität-Framework verwendet wird, um die Kommunikation mit der app übergeordneten WatchOS 2 oder höher verwenden die `WCSession` Klasse.

## <a name="application-lifecycle"></a>Anwendungslebenszyklus

In der Watch-Erweiterung eine Unterklasse von der `WKInterfaceController` Klasse wird für jede Szene Storyboard erstellt.

Diese `WKInterfaceController` Klassen sind analog zu den `UIViewController` Objekte in der iOS-Programmierung haben aber nicht das gleiche Maß Zugriff auf die Ansicht.
Sie können nicht für die Instanz, dynamisch Steuerelemente hinzuzufügen oder strukturieren Sie Ihre Benutzeroberfläche.
Sie können, jedoch ausblenden und Steuerelemente anzeigen und mit einigen Steuerelementen ändern ihre Größe, Transparenz und Optionen für die Darstellung.

Der Lebenszyklus einer `WKInterfaceController` Objekt umfasst die folgenden Aufrufe:

- [Aktiv](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : Sie sollten die meisten Ihrer Initialisierung in dieser Methode ausführen.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : aufgerufen, kurz bevor die Watch-App für die Benutzer angezeigt wird. Verwenden Sie diese Methode zum letzten Zeitpunkt Initialisierung ausführen, starten Animationen usw. ein.
- An diesem Punkt Watch-App angezeigt wird und die Erweiterung beginnt reagieren auf Benutzer, die Eingabe, und aktualisieren die Watch-App-Anzeige pro Ihrer Anwendungslogik.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) nach der Watch-App wurde vom Benutzer geschlossen wurde, wird diese Methode aufgerufen. Nach dem Beenden dieser Methode können nicht Steuerelemente der Benutzeroberfläche geändert werden, bis das nächste Mal `WillActivate` aufgerufen wird. Diese Methode wird auch aufgerufen werden, wenn die Verbindung mit dem iPhone unterbrochen wird.
- Nachdem die Erweiterung deaktiviert wurde, ist es für Ihr Programm nicht möglich. Ausstehende asynchrone Funktionen **nicht** aufgerufen werden. Überwachen-Kit-Erweiterungen sind nicht im Hintergrund Verarbeitungsmodi verwenden. Wenn das Programm wird vom Benutzer erneut aktiviert, aber die app wurde nicht vom Betriebssystem beendet, wird die erste Methode, die aufgerufen werden `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Application Lifecycle (Übersicht)")

## <a name="types-of-user-interface"></a>Typen der Benutzeroberfläche

Es gibt drei Arten von Interaktion, die der Benutzer mit der Watch-app haben kann.
Alle programmiert werden, mit benutzerdefinierten untergeordneten Klassen von `WKInterfaceController`, sodass die obige Lebenszyklus Sequenz universell gilt (Benachrichtigungen mit untergeordneten Klassen von programmiert werden `WKUserNotificationController`, die selbst ist eine Unterklasse von `WKInterfaceController`):

### <a name="normal-interaction"></a>Normale Aktivität

Die Mehrzahl der Interaktion der Watch-app-Erweiterung werden mit untergeordneten Klassen von `WKInterfaceController` , die Sie schreiben, um die Szenen in Ihre Watch-app entsprechen **Interface.storyboard**. Dieser Vorgang wird ausführlich beschrieben die [Installation](~/ios/watchos/get-started/installation.md) und [Einstieg](~/ios/watchos/get-started/index.md) Artikel.
Die folgende Abbildung zeigt einen Teil der [überwachen Kit Katalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Storyboard Beispiel. Für jede Szene wurde hier gezeigt, besteht eine entsprechende benutzerdefinierte `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`usw.) in das Erweiterungsprojekt.

![](intro-to-watchos-images/scenes.png "Beispiele für die normale Aktivität")

### <a name="notifications"></a>Benachrichtigungen

[Benachrichtigungen](~/ios/watchos/platform/notifications.md) sind ein wichtiger Anwendungsfall für die Apple Watch. Sowohl lokale als auch Benachrichtigungen werden unterstützt. Interaktion mit Benachrichtigungen erfolgt in zwei Phasen, kurzen und langen aussehen aufgerufen.

Kurze sieht werden kurz angezeigt und zeigt das Symbol "Watch-app", den Namen und den Titel (wie angegeben mit `WKInterfaceController.SetTitle`).

Lange suchen kombiniert eine vom System vorgegebene **Bildtrennlinie** Bereichs- und Dismiss-Schaltfläche mit dem benutzerdefinierten Storyboard-basierten Inhalt.

`WKUserNotificationInterfaceController` Erweitert `WKInterfaceController` mit den Methoden `DidReceiveLocalNotification` und `DidReceiveRemoteNotification`.
Überschreiben Sie diese Methoden, um auf Notification-Ereignisse zu reagieren.

Weitere Informationen zum Design des Notification, finden Sie in der [Apple Watch-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Beispiel-Benachrichtigungen")

## <a name="screen-sizes"></a>Bildschirmgrößen

Der Apple Watch hat zwei Gesicht Größen: 38mm und 42mm, mit einem 5:4 Seitenverhältnis und einem Retina-Display. Ihre nutzbare Größen sind:

- 38 mm: 136 x 170 logische Pixel (272 x 340 physischen Pixel)
- 42 mm: 156 x 195 logische Pixel (312 x 390 physischen Pixel).

Verwendung `WKInterfaceDevice.ScreenBounds` um zu bestimmen, auf welche Anzeige Watch-App ausgeführt wird.

Im Allgemeinen ist es einfacher, zum Entwickeln von Entwurfs Text und das Layout mit stärker eingeschränkten 38mm angezeigt, und klicken Sie dann vertikales Skalieren.
Wenn Sie mit der größeren Umgebung beginnen, kann die Skalierung nach unten zu problematischen überlappen oder Abschneiden von Text führen.

Erfahren Sie mehr über [arbeiten mit Bildschirmgrößen](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>Einschränkungen von watchOS

Es gibt einige Einschränkungen von WatchOS beim Entwickeln von apps WatchOS geachtet werden:

- Apple Watch-Geräte nur über begrenzte Speicher – Bedenken Sie den verfügbaren Speicherplatz vor dem Herunterladen großer Dateien (z. b. Audio- oder Movie-Dateien).

- Viele WatchOS [Steuerelemente](~/ios/watchos/user-interface/index.md) analoge in UIKit, aber verschiedenen Klassen (`WKInterfaceButton` statt `UIButton`, `WKInterfaceSwitch` für `UISwitch`usw.) und haben eine begrenzte Anzahl von Methoden, die im Vergleich zu ihren UIKit Entsprechungen. Darüber hinaus WatchOS verfügt über einige Steuerelemente wie z. B. `WKInterfaceDate` (für die Anzeige von Datum und Uhrzeit), UIKit verfügt nicht über.

  - Sie können nicht weiterleiten, Benachrichtigungen, die nur die Überwachung oder das iPhone nur (welche Art von Steuerelement der Benutzer über das routing wurde nicht von Apple angekündigt wurde).

Einige andere bekannten Einschränkungen / häufig gestellte Fragen:

- Apple lässt sich nicht auf Flächen 3rd Party benutzerdefinierte Überwachung aus.

- Die APIs, mit die die Überwachung iTunes auf dem verbundenen Telefon steuern können, die privat sind.


## <a name="further-reading"></a>Weiterführende Themen

Sehen Sie sich die Dokumentation von Apple an:

* [Entwickeln für Überwachung Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Sehen Sie sich Kit Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Apple Watch Human Interface-Richtlinien](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>Verwandte Links

- [WatchOS 3 Katalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchOS 1 Katalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Setup und Installation](~/ios/watchos/get-started/installation.md)
- [Erste Watch-App-video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Überwachen-Kit-Handbuch des Apple entwickeln.](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple WatchKit Tipps](https://developer.apple.com/watchkit/tips/)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
