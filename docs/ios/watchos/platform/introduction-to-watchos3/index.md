---
title: Einführung in watchOS 3
description: Dieser Artikel enthält alle neuen und geänderten APIs und Features, die in WatchOS 3 verfügbar ist, für die Xamarin-Entwickler.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/07/2017
ms.openlocfilehash: 0428a0df157e359ab34a6a71dbba31bdeb6962fa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224061"
---
# <a name="introduction-to-watchos-3"></a>Einführung in watchOS 3

_Dieser Artikel enthält alle neuen und geänderten APIs und Features, die in WatchOS 3 verfügbar ist, für die Xamarin-Entwickler._

In diesem Dokument werden die folgenden Themen behandelt:

- [Was ist neu in WatchOS 3](#Whats-New-in-watchOS-3)
    - [Apple-Zahlung Verbesserungen](#Apple-Pay-Enhancements) fügt Unterstützung für in-app-Zahlungen in der Apple Watch.
    - [Hintergrundaufgaben](#Background-Tasks) Geben Sie die app die Möglichkeit, aktualisieren die Informationen im Hintergrund aus, damit er bereit ist, wenn der Benutzer benötigt.
    - [Verbesserungen der Komplikationen](#Complications-Enhancements) für WatchOS 3, die neuen Features für die apps bieten vorgenommen wurden.
    - [Neu verfügbaren Frameworks](#Newly-Available-Frameworks) haben, die für die WatchOS-apps verfügbar gemacht.
    - [Proaktive Vorschläge](#Proactive-Suggestions) ermöglicht der app, proaktiv Informationen für den Benutzer anzuzeigen.
    * Mehrere [Verbesserungen Sicherheit und Datenschutz](#Security-and-Privacy-Enhancements) watchos 3 vorgenommen wurden.
    - [Momentaufnahmen und Andocken](#Snapshots-and-Dock) bieten dem Benutzer schnell Zugriff auf die app WatchOS-apps.
    - [Benutzerbenachrichtigungen](#User-Notifications) bietet sowohl lokale als auch Benachrichtigungen für den Benutzer.
    * Mehrere [sehen Sie sich Framework Konnektivitätsverbesserungen](#Watch-Connectivity-Framework-Enhancements) in WatchOS 3 vorgenommen wurden.
    * Mehrere [WatchKit-Framework-Erweiterungen](#WatchKit-Framework-Enhancements) in WatchOS 3 vorgenommen wurden.
    - [Trainings-App-Erweiterungen](#Workout-App-Enhancements) bietet neue Möglichkeiten der Trainings beziehen, Apple Watch-apps.
- [Zusätzliche-Frameworkänderungen](#Additional-Framework-Changes) in WatchOS 3 vorgenommen wurden.
- [Veraltete APIs](#Deprecated-APIs) in WatchOS 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Was ist neu in WatchOS 3

Apple hat verschiedene neue APIs und Dienste in WatchOS 3 sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay-Verbesserungen

In WatchOS 3 wurde das PassKit-Framework erweitert, um Unterstützung für sichere, in-app-Zahlungen (von sowohl physischen waren und Dienstleistungen) für die auf der Apple Watch-apps zu ermöglichen.

Verwenden Sie die neue [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) Klassen zum Darstellen von und reagieren auf eine Schnittstelle, in denen der Benutzer Anforderungen von Payment autorisieren kann.

Um mehr zu erfahren, informieren Sie sich unsere [Apple Zahlen Verbesserungen](~/ios/watchos/platform/apple-pay.md) Guide.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Hintergrundaufgaben

WatchOS 3 führt mehrere Aufgaben im Hintergrund, die eine app verwenden können, aktualisieren Sie die zugehörigen Informationen stellt sicher, dass sie den Inhalt, die der Benutzer muss, bevor sie es öffnen.

Die folgenden neuen Hintergrundaufgaben sind verfügbar:

- **App-Aktualisierung im Hintergrund** : die [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) Task ermöglicht der app, die den Zustand im Hintergrund zu aktualisieren. Dazu gehören in der Regel eine andere Aufgabe wie das Herunterladen neuen Inhalts aus dem Internet mit einer [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Aktualisieren Sie die Momentaufnahme im Hintergrund** : die [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) Task ermöglicht der app, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor das System eine Momentaufnahme darstellt, die zum Auffüllen der Dock verwendet werden.
- **Watch-Konnektivität im Hintergrund** : die [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) Aufgabe wird für die app gestartet, wenn der Empfang von Hintergrunddaten aus dem gekoppelten iPhone.
- **URL-Sitzung im Hintergrund** : die [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) Aufgabe wird für die app gestartet, wenn eine hintergrundübertragung erfordert eine Autorisierung oder abgeschlossen ist (erfolgreich oder Fehler).

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) Guide.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Komplikationen-Verbesserungen

Komplikationen sind kleine visuelle Elemente, die nützliche Informationen auf einen Blick zu bieten. Je nach dem Zifferblatt Ihrer Apple Watch ausgewählt haben hat der Benutzer die Möglichkeit, eine Zifferblatt Ihrer Apple Watch mit ein oder mehrere Komplikation anzupassen.

WatchOS 3 erhält die app die Möglichkeit, eine oder mehrere Komplikation für die Watch-app erstellen, damit der Benutzer die Informationen auf einen Blick auf eine Zifferblatt zugreifen kann.

Darüber hinaus bieten Komplikationen, die folgenden Vorteile:

- Der Benutzer kann die app schnell durch Tippen auf der Komplikation direkt aus einer Zifferblatt Ihrer Apple Watch starten.
- Müssen eine der schwierigkeiten bei der Art der app auf die Überwachungsfenster Gesicht Ursachen des Systems lassen Sie die app sofort zu starten, in denen versucht wird, starten Sie die app im Hintergrund, bewahren Sie es im Arbeitsspeicher und gibt es zusätzliche Zeit, um zu aktualisieren.
- Komplikationen garantiert mindestens 50 Push Aktualisierungen pro Tag.
- Wenn die app Komplikationen enthält, wird sie in der Apple Watch-Face-Galerie enthielt (finden Sie unter Apple [Komplikationen im Katalog hinzufügen](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) finden Sie Dokumentation).

In WatchOS 3, das Framework ClockKit enthält nun mehrere neue Vorlagen für sehr große schwierigkeiten wie [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) und [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Verwenden Sie darüber hinaus neue Methoden zum Erstellen lokalisierbaren Texts die [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) Klasse.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [schnelle Interaktionstechniken für WatchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) Guide.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Neu verfügbare Frameworks

WatchOS 3 umfasst mehrere vorhandene Apple-Frameworks, die zuvor nicht verfügbar, z. B. wurden:

- **SceneKit** -mithilfe von SceneKit 3D sollen in der Watch-app-Benutzeroberfläche, einschließlich der meisten Funktionen, die auf anderen Plattformen verfügbar, wie die Beleuchtung, Schattierung, Animation, die Physik und-Partikelsysteme modelliert. Räumliche 3D, Audio, benutzerdefinierten Metall oder OpenGL-Shadern, Core Bildfilter und physisch basierende Materialien werden nicht unterstützt.
- **Ein SpriteKit** -mithilfe von SpriteKit Rendern und Animieren von Sprites in der app Watch-app-Benutzeroberfläche, einschließlich der Großteil der Features, die auf anderen Plattformen wie Aktionen, die Physik, Beleuchtung und Partikel Systeme verfügbar. Räumliche 3D, Audio, Videowiedergabe und Core-Image-Filter werden nicht unterstützt.
- **AVFoundation** : Informationen zum Verwalten und Audiowiedergabe.
- **CloudKit** – zum Verschieben von Daten zwischen die Watch-app und iCloud-Container.
- **Core-Audio** – zum Verwalten von Datentypen für Audiostreams, komplexe Puffer und Time-Werten darstellt.
- **GameKit** – soziale Spiele zu erstellen.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Proaktive Vorschläge

WatchOS 3 ermöglicht der app, proaktiv Informationen für den Benutzer in präsentieren Kontexten angegeben. Zur Unterstützung dieser Funktion, die [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) enthält jetzt die `MapItem` -Eigenschaft, die der app, die Informationen zum Speicherort für die spätere Verwendung von anderen apps bereitstellen kann.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in die proaktive Vorschläge](~/ios/watchos/platform/proactive-suggestions.md) Guide.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Verbesserungen

Apple hat verschiedene Verbesserungen vorgenommen, um sowohl Sicherheit und Datenschutz für WatchOS 3, die den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten, die helfen.

Apps für WatchOS 3 (oder höher) müssen daher statisch deklarieren ihrer Absicht, bestimmte Features oder Benutzerinformationen zugreifen, indem Sie die Eingabe eine oder mehrere bestimmte Datenschutzschlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

Da diese Änderungen mit iOS 10 WatchOS 3 freigegeben hat, finden Sie unserem iOS 10 [Verbesserungen Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md) Anleitung finden Sie weitere Informationen.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Momentaufnahmen und Andocken

In WatchOS 3 hat Apple das Dock hinzugefügt, in denen Benutzer heften Sie ihre bevorzugten apps und schnell darauf zugreifen können. Wenn der Benutzer auf der Seite auf der Apple Watch-drückt, wird ein Katalog von angehefteten app Momentaufnahmen angezeigt. Der Benutzer kann streichen Sie nach links oder rechts an die gewünschte app zu finden, und tippen Sie dann die app, um die Datei und Ersetzen Sie dabei die Momentaufnahme mit der ausgeführten app-Benutzeroberfläche zu starten.

Das System wird in regelmäßigen Abständen erstellt Momentaufnahmen die Benutzeroberfläche der app und verwenden diese Momentaufnahmen, um die Dokumentation zu füllen. WatchOS erhält die app die Möglichkeit, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor diese Momentaufnahme erstellt wird.

Weitere Informationen finden Sie unsere [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) Handbuch und Apple [WKSnapshotRefreshBackgroundTask Verweis](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Benutzerbenachrichtigungen

Die Benutzerbenachrichtigung-Framework, die in WatchOS 3 eingeführt unterstützt die Übermittlung von Benachrichtigungen von sowohl lokale als auch auf der Apple Watch. Verwenden Sie dieses Framework Benachrichtigungen basierend auf bestimmten Bedingungen wie z. B. der Tageszeit oder Speicherort planen und zu empfangen und Verarbeiten von Benachrichtigungen an.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [schnelle Interaktionstechniken für WatchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) Guide.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Sehen Sie sich Framework Konnektivitätsverbesserungen

Die neue `HasContentPending` Eigenschaft der [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) -Ereignisklasse zeigt an, dass die Sitzung, Daten im Hintergrund erhalten hat, die verarbeitet werden muss. Und die `RemainingComplicationUserInfoTransfers` Eigenschaft gibt die verbleibenden Zeitpunkte für die iOS-app können Sie die WatchOS-Komplikation aktualisieren.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) Guide.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit-Framework-Erweiterungen

WatchOS 3 enthält verschiedene Verbesserungen für das WatchKit-Framework, einschließlich der folgenden:

- Die app kann den Status der digitalen Crown abrufen mithilfe der neuen [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) Klasse und Updates erhalten, wenn der Benutzer die Crown mit dreht das [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) Klasse.
- Die [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) -Klasse enthält nun die `ApplicationState` Methode und [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) -Konstante, die die app verwenden können, um den Laufzeitstatus der app zu verfolgen. `WKExtension` Außerdem bietet zwei neue Methoden, die zum Planen von Aufgaben im Hintergrund verwendet werden können.
- Die [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) enthält jetzt den neuen `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` und `HandleBackgroundTasks` Methoden, um Änderungen an den Status der Anwendung überwachen und Behandeln von Updates für die Hintergrundfarbe.
- Ein neues [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) -Klasse wurde hinzugefügt, um die folgenden Typen von gestenerkennung für die Watch-apps bereitzustellen: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) and [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Die neue [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) Klasse stellt eine Schnittstelle für alle HomeKit IP-Kamera angefügt.
- Die neue [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) Klasse ermöglicht der app, einen Film "Poster", die durch den ausgeführten Film ersetzt wird, wenn der Benutzer, es tippt anzuzeigen.
- Die neue [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) Klasse ermöglicht der app, die eine Schaltfläche "Apple Pay" in der Benutzeroberfläche vorhanden, die eine Zahlungsaufforderung beim Tippen auf initiieren.
- Die neue [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) -Klasse stellt eine Schnittstelle zum Anzeigen von einer SceneKit-Szene in der Apple Watch.
- Die neue [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) -Klasse stellt eine Schnittstelle zum Anzeigen von einer SpriteKit-Szene in der Apple Watch.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [schnelle Interaktionstechniken für WatchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) Guide.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Trainings-App-Erweiterungen

Neue WatchOS 3 Trainings im Zusammenhang mit apps haben die Möglichkeit, im Hintergrund ausgeführt werden soll, klicken Sie auf der Apple Watch. Um dieses Feature aktivieren (und erhalten Zugriff auf Daten von HealthKit), muss die app enthalten die `WKBackgroundModes` -Schlüssel in der `Info.plist` Datei mit dem Wert `workout-processing`.

Darüber hinaus kann der Entwickler jetzt zum Starten der WatchOS-Trainings-app aus dem iOS-app-Version auf dem gekoppelten iPhone.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Trainings-App-Erweiterungen](~/ios/watchos/platform/workout-apps.md) Guide.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Zusätzliche-Frameworkänderungen

Zusätzlich zu den wichtigsten Framework-Änderungen und Ergänzungen, die oben aufgeführten hat Apple viele weitere kleinere Framework WatchOS 3 geändert.

Um mehr zu erfahren, informieren Sie sich unsere [zusätzliche-Frameworkänderungen](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) Guide.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs wurden in WatchOS 3 veraltet:

- Die `UILocalNotification` UIKit Klasse ist veraltet und sollte mit dem Framework für die Benutzerbenachrichtigung ersetzt werden.

Finden Sie unter Apple [WatchOS 2.2 auf WatchOS-3.0-API-Unterschiede](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) Dokumentation für eine vollständige Liste der veralteten und Änderungen.


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
- [Neuerungen in WatchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
