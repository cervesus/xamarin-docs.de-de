---
title: Einführung in die WatchOS 3
description: In diesem Artikel werden alle neuen und geänderten-APIs und in WatchOS 3 verfügbaren Funktionen für Entwickler von Xamarin eingeführt.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: 506e3795538ceffc77301a608c504fc6ec2045a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos-3"></a>Einführung in die WatchOS 3

_In diesem Artikel werden alle neuen und geänderten-APIs und in WatchOS 3 verfügbaren Funktionen für Entwickler von Xamarin eingeführt._

Dieses Dokument befasst sich mit die folgenden Themen:

- [Was ist neu in WatchOS 3](#Whats-New-in-watchOS-3)
    - [Apple bezahlen Verbesserungen](#Apple-Pay-Enhancements) fügt Unterstützung für in der app-Zahlungen auf der Apple Watch.
    - [Hintergrundaufgaben](#Background-Tasks) Geben Sie der app die Möglichkeit, die Informationen im Hintergrund aktualisieren, damit er bereit ist, wenn der Benutzer benötigt.
    - [Komplikationen Verbesserungen](#Complications-Enhancements) für WatchOS 3, die neuen Features für die apps bieten vorgenommen wurden.
    - [Neu verfügbaren Frameworks](#Newly-Available-Frameworks) wurden für die apps WatchOS verfügbar gemacht.
    - [Proaktive Vorschläge](#Proactive-Suggestions) ermöglicht der app, um proaktiv Informationen für den Benutzer anzuzeigen.
    * Mehrere [Sicherheit und Datenschutz Erweiterungen](#Security-and-Privacy-Enhancements) an WatchOS 3 vorgenommen wurden.
    - [Momentaufnahmen und Andocken](#Snapshots-and-Dock) bieten dem Benutzer schnellen Zugriff auf die app WatchOS apps.
    - [Benutzerbenachrichtigungen](#User-Notifications) liefert sowohl lokale als auch Benachrichtigungen für den Benutzer.
    * Mehrere [überwachen Konnektivität Framework Verbesserungen](#Watch-Connectivity-Framework-Enhancements) in WatchOS 3 vorgenommen wurden.
    * Mehrere [WatchKit Framework Verbesserungen](#WatchKit-Framework-Enhancements) in WatchOS 3 vorgenommen wurden.
    - [Trainings-App-Erweiterungen](#Workout-App-Enhancements) bietet neue Möglichkeit, den Trainings beziehen, Apple Watch-apps.
- [Zusätzliche Framework Änderungen](#Additional-Framework-Changes) während des gesamten WatchOS 3 vorgenommen wurden.
- [Veraltete APIs](#Deprecated-APIs) in WatchOS 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Was ist neu in WatchOS 3

Apple hat mehrere neue APIs und Dienste in WatchOS 3 sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay-Erweiterungen

In WatchOS 3 wurde das Framework PassKit erweitert, um Unterstützung für sicheren, in der app Zahlungen (von sowohl physischen waren und Dienstleistungen) für die auf der Apple Watch-apps zu ermöglichen.

Verwenden Sie die neue [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) Klassen zum darstellen und reagieren auf eine Schnittstelle, in denen der Benutzer Zahlung Anforderungen autorisieren kann.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Apple bezahlen Verbesserungen](~/ios/watchos/platform/apple-pay.md) Handbuch.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Hintergrundaufgaben

WatchOS 3 führt mehrere Hintergrundaufgaben, die app nicht verwenden kann, die Informationen sicherstellen, dass er den Inhalt verfügt, die der Benutzer muss, bevor sie es öffnen aktualisieren.

Die folgende neue Hintergrundaufgaben sind verfügbar:

- **App-Aktualisierung im Hintergrund** – der [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) Aufgabe ermöglicht der app, dessen Status im Hintergrund aktualisiert werden. Dazu gehören i. d. r. eine andere Aufgabe einschließlich des Herunterladens neuen Inhalts aus dem Internet mit einer [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Im Hintergrund aktualisieren Momentaufnahme** – der [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) Aufgabe ermöglicht der app, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor das System eine Momentaufnahme darstellt, die zum Auffüllen des Docks verwendet werden.
- **Im Hintergrund Überwachungsfenster Konnektivität** – der [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) Task wird beim Empfang von Hintergrunddaten im mit gepaarten iPhone für die app gestartet.
- **URL-Sitzung im Hintergrund** – der [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) Vorgang wird für die app gestartet, wenn ein hintergrundübertragung erfordert eine Autorisierung oder abgeschlossen ist (erfolgreich oder fehlerhaft).

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) Handbuch.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Komplikationen-Erweiterungen

Komplikationen sind kleine visuelle Elemente, die nützliche Informationen auf einen Blick zu bieten. Je nach dem Zifferblatt Watch ausgewählt ist hat der Benutzer die Möglichkeit, einem Überwachungsfenster Gesicht mit mindestens Komplikation anzupassen.

WatchOS 3 kann die app die Möglichkeit, eine oder mehrere Komplikation für die Watch-app erstellen, sodass der Benutzer die Informationen auf einen Blick aus einem Überwachungsfenster Gesicht zugreifen kann.

Darüber hinaus bieten Komplikationen die folgenden Vorteile:

- Der Benutzer kann die app schnell durch Tippen auf der Komplikation direkt aus einer Schriftart für die Überwachung zu starten.
- Mit einem Komplikationen die app auf die Überwachungsfenster Gesicht Ursachen im System, halten die app in einem Ready-Start-Zustand, in denen versucht wird, starten Sie die Anwendung im Hintergrund, bewahren Sie ihn an Arbeitsspeicher und es ausreichend zusätzliche Zeit aktualisiert.
- Komplikationen garantiert mindestens 50 Push-Updates pro Tag.
- Wenn die app Komplikationen enthält, wird es in der Apple Watch Gesicht Gallery vertreten sein (finden Sie in der Apple- [Komplikationen hinzufügen, an den Katalog](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) Dokumentation weitere Informationen).

In WatchOS 3, das Framework ClockKit enthält nun mehrere neue Vorlagen für sehr große Komplikationen wie [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) und [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Darüber hinaus um lokalisierbare Text zu erstellen, verwenden neue Methoden der [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) Klasse.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [schnelle Interaktion Techniken für WatchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) Handbuch.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Neu verfügbare Frameworks

WatchOS 3 enthält mehrere vorhandene Apple-Frameworks, die nicht wie zuvor verfügbar waren:

- **SceneKit** -Verwendung SceneKit 3D einschließen in die Watch-app-Benutzeroberfläche, die meisten Funktionen, die auf anderen Plattformen wie Beleuchtung, Schattierung Animation, physikalische verfügbar und Partikelsysteme einschließlich modelliert. 3D räumliche Audio, benutzerdefinierte Metall oder OpenGL-Shader, Core-Image-Filter und Materialien physisch basierende werden nicht unterstützt.
- **SpriteKit** -Verwendung SpriteKit Rendern und animieren Sprites in die app Watch-app-Benutzeroberfläche, einschließlich der Großteil der Funktionen, die auf anderen Plattformen wie Aktionen, physikalische, Beleuchtung und Partikel-Systeme verfügbar. 3D räumliche Audio, Videowiedergabe und Core-Image-Filter werden nicht unterstützt.
- **AVFoundation** : Informationen zum Verwalten und abspielen.
- **CloudKit** – Informationen zum Verschieben von Daten zwischen die Watch-app und iCloud-Container.
- **Haupt-Audio** – Informationen zum Verwalten von Datentypen für Audiodatenströme, komplexe Puffer und Time-Werten darstellt.
- **GameKit** – Informationen zum Erstellen von sozialer spielen.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Proaktive Vorschläge

WatchOS 3 ermöglicht der app, Informationen zum Benutzer in proaktiv präsentieren Kontexten angegeben. Zur Unterstützung dieser Funktion die [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) enthält jetzt die `MapItem` -Eigenschaft, die die app, Informationen zum Speicherort für die spätere Verwendung durch andere apps bereitstellen kann.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in die proaktive Vorschläge](~/ios/watchos/platform/proactive-suggestions.md) Handbuch.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Erweiterungen

Apple hat mehrere Erweiterungen auf Sicherheit und Datenschutz in WatchOS 3, mit deren Hilfe wird den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten.

Apps, die unter WatchOS 3 (oder höher) müssen daher statisch deklarieren ihre Absicht auf bestimmte Funktionen oder Benutzerinformationen zugreifen, indem die Eingabe eine oder mehrere bestimmte Datenschutz-Schlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

Da diese Änderungen mit iOS 10 WatchOS 3 freigegeben hat, finden Sie unter unserer iOS 10 [Sicherheit und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) Weitere Informationen.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Momentaufnahmen und Andocken

In WatchOS 3 fügte Apple Docks, in denen Benutzer ihre bevorzugten apps anheften und schnell darauf zugreifen können. Wenn der Benutzer auf der Seite auf der Apple Watch drückt, wird ein Katalog von angeheftete app Momentaufnahmen angezeigt. Der Benutzer kann Wischen Sie nach links oder rechts an die gewünschte app suchen, und tippen Sie dann die app zum Ersetzen der Momentaufnahme mit der ausgeführten app-Benutzeroberfläche starten.

Das System wird in regelmäßigen Abständen Momentaufnahmen der Benutzeroberfläche der Anwendung akzeptiert und diese Momentaufnahmen zum Auffüllen von Dokumente verwendet wird. WatchOS ermöglicht es der app auf dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor diese Momentaufnahme erstellt wird.

Weitere Informationen finden Sie unter unsere [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) Handbuch und Apple [WKSnapshotRefreshBackgroundTask Verweis](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Benutzerbenachrichtigungen

In WatchOS 3 eingeführte Benutzerbenachrichtigung-Framework unterstützt die Übermittlung von lokalen und Remotecomputern Benachrichtigungen auf der Apple Watch. Verwenden Sie dieses Framework zum Planen von Benachrichtigungen auf Grundlage von bestimmten Bedingungen wie z. B. Zeit des Tages oder Speicherort und zum Empfangen und Verarbeiten von Benachrichtigungen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [schnelle Interaktion Techniken für WatchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) Handbuch.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Überwachen Sie die Konnektivität Framework-Erweiterungen

Die neue `HasContentPending` Eigenschaft von der [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) Klasse gibt an, dass die Sitzung Daten im Hintergrund erhalten hat, die verarbeitet werden muss. Und die `RemainingComplicationUserInfoTransfers` Eigenschaft gibt die verbleibenden Zeitpunkte für die iOS-app kann ihre WatchOS Komplikation aktualisieren.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) Handbuch.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit Framework-Erweiterungen

WatchOS 3 enthält verschiedene Verbesserungen der WatchKit-Framework, einschließlich der folgenden:

- Die app erhalten den Status der digitalen Kronenlänge mit dem neuen [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) Klasse und Updates zu erhalten, wenn der Benutzer die Crown mit dreht die [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) Klasse.
- Die [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) -Klasse enthält nun die `ApplicationState` Methode und [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) Konstante, die die app zum Nachverfolgen des Laufzeitstatus der app verwenden können. `WKExtension` Außerdem bietet zwei neue Methoden, die zum Planen von Hintergrundaufgaben verwendet werden können.
- Die [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) enthält jetzt das neue `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` und `HandleBackgroundTasks` Methoden, um Änderungen an den Status der Anwendung überwachen und Behandeln von Hintergrund Task aktualisiert.
- Ein neues [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) Klasse geben Sie die folgenden Typen von Geste Recognition an den Watch-apps hinzugefügt wurde: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer ](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) und [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Die neue [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) Klasse stellt eine Schnittstelle für alle HomeKit IP-Kamera angefügt.
- Die neue [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) Klasse ermöglicht der app, einen Film "Poster", die von der ausgeführten Film ersetzt wird, wenn der Benutzer es tippt anzuzeigen.
- Die neue [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) Klasse ermöglicht der app, eine Apple Pay-Schaltfläche in der Benutzeroberfläche bereit, die eine Zahlung-Anforderung, wenn getippt initiiert wird.
- Die neue [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) Klasse verfügt über eine Oberfläche zum Anzeigen von SceneKit themawechsel auf der Apple Watch.
- Die neue [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) Klasse verfügt über eine Oberfläche zum Anzeigen von SpriteKit themawechsel auf der Apple Watch.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [schnelle Interaktion Techniken für WatchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) Handbuch.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Trainings-App-Erweiterungen

Neue WatchOS 3, Trainings bezüglich apps haben die Möglichkeit, auf der Apple Watch im Hintergrund ausgeführt. Um dieses Feature aktivieren (und erhalten Zugriff auf Daten HealthKit), muss die app enthalten die `WKBackgroundModes` -Schlüssel in der `Info.plist` Datei mit dem Wert `workout-processing`.

Darüber hinaus hat der Entwickler jetzt die Möglichkeit zum Starten der WatchOS Trainings-app aus der iOS-app-Version auf dem iPhone gepaart.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Trainings App Verbesserungen](~/ios/watchos/platform/workout-apps.md) Handbuch.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Zusätzliche Framework-Änderungen

Zusätzlich zu den wichtigsten Framework Änderungen und Ergänzungen, die oben aufgeführten verfügt über Apple viele zusätzliche kleinere Framework WatchOS 3 geändert.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [zusätzliche Framework Änderungen](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) Handbuch.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs wurden im WatchOS 3 als veraltet markiert:

- Die `UILocalNotification` UIKit Klasse ist veraltet und sollte mit dem Framework Benutzerbenachrichtigung ersetzt werden.

Finden Sie in der Apple- [WatchOS 2.2, WatchOS 3.0-API-Unterschiede](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) Dokumentation für eine vollständige Liste der veralterungen und Änderungen.


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
- [Was ist neu in WatchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
