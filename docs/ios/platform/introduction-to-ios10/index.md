---
title: Einführung in iOS 10
description: In diesem Artikel werden alle neuen und geänderten-APIs und in iOS 10 verfügbaren Funktionen für Entwickler von Xamarin.iOS eingeführt.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: c3bee0f15016394005a67e98cd8435e6d63b3ac6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30786466"
---
# <a name="introduction-to-ios-10"></a>Einführung in iOS 10

_In diesem Artikel werden alle neuen und geänderten-APIs und in iOS 10 verfügbaren Funktionen für Entwickler von Xamarin.iOS eingeführt._

## <a name="introducing-ios-10"></a>Einführung in iOS 10

Mit der neuen iOS 10 SDK an, Apple hat enthalten neue APIs und Dienste, die den Entwickler so erstellen Sie neue Kategorien von apps und Features zu aktivieren. Eine iOS-app kann jetzt erweitern, die Nachrichten "," Siri "," Telefon "und" Maps apps umfangreiche Funktionen einbeziehen, um der Endbenutzer bereitzustellen, die zuvor nicht verfügbar war.

Weitere Informationen zu iOS 10, finden Sie im Apple [iOS + Apps](https://developer.apple.com/ios/) Dokumentation.

## <a name="whats-new-in-ios-10"></a>Was ist neu in iOS 10

Apple hat mehrere neue APIs und Dienste in iOS 10 sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:


## <a name="adapting-to-the-true-tone-display"></a>Anpassen der Anzeige "true" Signalton an

"True" Ton anzeigen Apple-Technologie verwendet den ambient light Sensor in einem iOS-Gerät, um dynamisch anpassen, die Farbe und die Intensität der Anzeige die aktuellen Beleuchtung Bedingungen erfüllen. iOS 10 bietet die neuen [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) Schlüssel, der der app hinzugefügt werden kann `Info.plist` Datei, und steuert, wie "true" Signalton an die Schicht die Standardfarbe anwendet. 

Die folgenden Werte sind verfügbar:

- `UIWhitePointAdaptivityStyleStandard` **Standard** -standard White-Point-Adaptivität verwenden.
- `UIWhitePointAdaptivityStyleReading` -Zum Lesen mit Fokus apps verwendet.
- `UIWhitePointAdaptivityStyleGame` – Für apps Game Developers verwendet.
- `UIWhitePointAdaptivityStyleVideo` – Für apps Video Developers verwendet.
- `UIWhitePointAdaptivityStylePhoto` -Wird für apps Fotos mit Fokus, an der Farbtreue wichtiger als environmental weiß Point-Anpassungen.

<a name="app-extensions" />

## <a name="app-extensions"></a>App-Erweiterungen

Apple hat mehrere neue Erweiterungspunkte in iOS 10 zur Verfügung:

- Rufen Sie Verzeichnis
- Intents und Intents UI
- Mitteilungen
- Benachrichtigungsinhalt
- Notification Services
- Der Aufkleber mit Pack

3. Tastatur App-Erweiterungen von Drittanbietern verfügen darüber hinaus die folgenden Verbesserungen:

- Die neue `DocumentInputMode` Eigenschaft von der `UITextDocumentProxy` Klasse bestimmen die Eingabesprache eines Dokuments und ermöglichen die Erweiterung der Tastatur an die entsprechende Sprache aus.
- Die neue `HandleInputModeList` -Methode können die Tastatur-Erweiterung, die das System Tastatur Menü zur Auswahl als Antwort auf dem Globus Schlüssel abgerufen wird angezeigt.

Weitere Informationen finden Sie unter unsere [Einführung in die Erweiterungen](~/ios/platform/extensions.md), [App Nachrichtenintegration](~/ios/platform/message-app-integration/index.md), [Einführung in die proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md), [ Einführung in die SiriKit](~/ios/platform/sirikit/index.md), [Einführung in Benutzerbenachrichtigungen](~/ios/platform/user-notifications/index.md) und Apple [-App-Erweiterung Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>Suchen Sie App-Erweiterungen

Core Spotlight in iOS 10 bietet mehrere Erweiterungen wie z. B. App suchen:

- **Crowdsourced Deep-Link Beliebtheit (mit differenzielle Datenschutz)** -bietet eine Möglichkeit, die app mit Deep-Link-Inhalt in den Suchergebnissen angezeigter höher stufen.
- **Suchen in der App** -verwenden Sie die neue `CSSearchQuery` -Klasse, in der app Spotlight-Suche Möglichkeit ähnlich wie die e-Mail-, Nachrichten und Anmerkungen zu dieser apps arbeiten bereitzustellen.
- **Suchen Sie die Fortsetzung** – ermöglicht Benutzern das Starten einer Suche im Spotlight oder Safari, und öffnen Sie eine app und die Suche zu fortfahren.
- **Visualisierung von Überprüfungsergebnissen** -Apple [App Search-API-Überprüfungstools](https://search.developer.apple.com/appsearch-validation-tool) zeigt jetzt eine visuelle Darstellung von Markup und umfassende Verknüpfen einer Website aus, wenn Tests preforming.
- **Freigabe-App Image Message** -können Sie gängige in app-Images, die bereitgestellt werden, für die Freigabe in Nachrichten (über eine Nachrichten-App-Erweiterung) in der Spotlight-Suche angezeigt werden sollen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [App Suche Erweiterungen](~/ios/platform/search/app-search-enhancements.md) Handbuch.

## <a name="apple-pay-enhancements"></a>Apple Pay-Erweiterungen

Apple hat mehrere Erweiterungen Apple Pay in iOS 10 vorgenommen, mit denen Benutzer sicheren Zahlungen von Websites und über die Interaktion mit Siri und Zuordnungen vornehmen können.

Mit iOS-10 wurden mehrere neue APIs hinzugefügt, arbeiten mit iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-testumgebung.

Darüber hinaus wurde das PassKit-Framework zur Unterstützung von Apple Pay außerhalb von erweitert `UIKit` und -Aussteller an ihre Karten in ihren apps zu ermöglichen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Apple bezahlen Verbesserungen](~/ios/platform/apple-pay.md) Handbuch.

## <a name="alternate-app-icons"></a>Alternative App-Symbole

Apple hat mehrere Erweiterungen für iOS-10.3 hinzugefügt, mit denen eine app, deren Symbol verwalten können:

 - `ApplicationIconBadgeNumber` -Ruft ab oder legt das Signal für das Symbol "app" in der Springboard fest.
 - `SupportsAlternateIcons` -If `true` die app hat eine Alternative Gruppe von Symbolen.
 - `AlternateIconName` -Gibt den Namen des aktuell ausgewählten alternativen Symbols oder `null` Wenn das Symbol "primary" verwenden.
 - `SetAlternameIconName` -Verwenden Sie diese Methode, um das Symbol für die app des angegebenen alternativen Symbols zu wechseln.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [alternativen App-Symbole](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) Handbuch.


## <a name="introduction-to-callkit"></a>Einführung in CallKit

Die neue CallKit-API in iOS 10 bietet eine Möglichkeit für VOIP-apps für iPhone-Benutzeroberfläche integriert und bieten eine vertraute Oberfläche für den Endbenutzer auftreten. Mit dieser API können Benutzer anzeigen und interagieren mit VOIP-Aufrufe von des iOS-Geräts Sperrbildschirm und verwalten können Kontakte mit der Phone-app **Favoriten** und **zuletzt geöffnete** Ansichten.

Darüber hinaus bietet die CallKit-API die Fähigkeit zum Erstellen von App-Erweiterungen, die einem Namen (Anrufer-ID) eine zehnstellige Nummer zuordnen oder mitteilt, dass das System, wenn eine Zahl sein darf (aufrufen blockiert sind) blockiert können.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in Callkit](~/ios/platform/callkit.md) Handbuch.

## <a name="message-app-integration"></a>Integrieren von Apps

iOS 10 ermöglicht die Aufnahme des Nachrichten-App-Erweiterung in der Xamarin.iOS-Lösung, mit den der **Nachrichten** app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Zwei Arten von App-Erweiterung der Nachricht sind verfügbar:

- **Der Aufkleber mit Packs** -enthält eine Auflistung von Aufkleber, die der Benutzer eine Nachricht hinzufügen kann. Der Aufkleber mit Packs können erstellt werden, ohne Code schreiben zu müssen.
- **iMessage App** -kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-app für Aufkleber auswählen, Text eingeben, einschließlich Mediendateien (mit optionalen typkonvertierungen) und erstellen, bearbeiten und Senden von Nachrichten der Aktivität darstellen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [App Nachrichtenintegration](~/ios/platform/message-app-integration/index.md) Handbuch.

## <a name="news-publisher-enhancements"></a>News Verleger Erweiterungen

Mit iOS-10 wird Apple jedermann von wichtigen Zeitschriften und neue Organisationen Blogger und unabhängige Verleger zu registrieren und Product und Inhalte der Apple-News-App bereitstellen. Weitere Informationen finden Sie unter der Apple- [News Ressourcen](https://newsresources.apple.com/) Dokumentation.

## <a name="providing-haptic-feedback"></a>Haptic Übermitteln von Feedback

Auf dem iPhone 7 und iPhone umfasst 7 Plus Apple neue Haptics-Antworten, die zusätzliche Möglichkeiten für den Benutzer physisch initiiert bereitstellen. Verwenden Sie die neue praktisch Feedbackoptionen, die Aufmerksamkeit des Benutzers abrufen und ihre Aktionen zu vertiefen.

Mehrere integrierte Benutzeroberflächenelemente Ihr Feedback bereits haptic z. B. Bildlaufbereich, Switches und Schieberegler. iOS 10 fügt nun die Möglichkeit, mithilfe einer konkreten Unterklasse von Haptics programmgesteuert Auslösen der `UIFeedbackGenerator` Klasse.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Haptic Feedback bereitstellen](~/ios/user-interface/ios-ui/haptic-feedback.md) Handbuch.

## <a name="proactive-suggestions"></a>Proaktive Vorschläge

iOS 10 präsentiert neue Methoden für die steuernde Engagement an eine app nach Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer zur richtigen Zeit jeweils anzuzeigen. Ebenso wie iOS bereitgestellt 9 die Möglichkeit zum Hinzufügen von umfassenden Suche der App mit Spotlight, Übergabe und Vorschläge zur Siri mit iOS 10, die eine app Funktionen verfügbar gemacht werden können, die vom System in den folgenden Speicherorten aus, die dem Benutzer angezeigt werden können:

- Der App Switcher
- Dem Sperrbildschirm
- CarPlay
- Karten
- Siri Interaktionen
- QuickType-Vorschläge 

Eine app macht diese Funktion in das System für eine Sammlung von Technologien wie z. B. über [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), Web-Markup, Core Spotlight, MapKit und UIKit Media Player.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in die proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md) Handbuch.

## <a name="request-app-review"></a>Prüfung der änderungsanforderungen-App

Neu bei iOS 10.3, die `RequestReview()` -Methode ermöglicht es eine iOS-app an den Benutzer bitten, bewerten, oder Überprüfen sie. Während dieser Methode zu einem beliebigen Zeitpunkt aufgerufen werden kann, in denen es Sinn, in die Benutzeroberfläche macht, wird der Prüfvorgang gesteuert und vom App Store Richtlinien behandelt. Daher diese Methode möglicherweise eine Warnung u. u. nicht angezeigt und sollte nie in Reaktion auf eine Benutzeraktion, z. B. Tippen auf eine Schaltfläche aufgerufen werden.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [App Überprüfung anfordern](~/ios/platform/request-app-review.md) Handbuch.

## <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Erweiterungen

Apple hat mehrere Erweiterungen auf Sicherheit und Datenschutz für iOS 10, mit deren Hilfe wird den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten.

Folglich apps unter iOS 10 (oder höher) müssen ihre Absicht auf bestimmte Funktionen oder Benutzerinformationen zugreifen, indem Sie eine oder mehrere bestimmte Datenschutz-Schlüssel in eingeben statisch deklarieren ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Sicherheit und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) Handbuch.

## <a name="sirikit"></a>SiriKit

Neue iOS 10, SiriKit ermöglicht eine Xamarin.iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri auf einem iOS-Gerät zugänglich sind. Diese Funktion finden Sie im App-Erweiterung für eine oder mehrere mit dem neuen **Intents** und **Intents UI** Frameworks.

SiriKit unterstützt die folgenden Dienstdomänen an:

- Audio oder video aufrufen.
- Buchung eine fuhr an.
- Verwalten von Training.
- Messaging.
- Suchen Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung Siri im Zusammenhang mit einer der Dienste für die App-Erweiterung stellt, sendet SiriKit die Erweiterung ein **Absicht** -Objekt, das der Benutzer-Anforderung sowie alle unterstützungsdaten beschreiben. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Details wie die Erweiterung für die Anforderung verarbeiten kann.

Siri in der Regel alle Benutzerinteraktionen behandelt, die App-Erweiterung können jedoch die **Absicht UI** Framework ein umfassendes, benutzerdefinierte Benutzeroberfläche mit die app branding präsentieren und zusätzliche Informationen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in SiriKit](~/ios/platform/sirikit/index.md) Handbuch.

## <a name="speech-recognition"></a>Spracherkennung

iOS 10 umfasst eine neue Speech-API, die die app fortlaufende Spracherkennung unterstützen und transkribieren Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text ermöglicht.

Da Spracherkennung erforderlich, die Übertragung und die temporäre Speicherung von Daten auf der Apple-Servern, die app ist _müssen_ Erlaubnis des Benutzers auszuführenden Anerkennung durch Einschließen der `NSSpeechRecognitionUsageDescription` -Schlüssel in seine `Info.plist` Datei- und Aufrufen der `SFSpeechRecognizer.RequestAutorization` Methode.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in die Spracherkennung](~/ios/platform/speech.md) Handbuch.

## <a name="user-notifications"></a>Benutzerbenachrichtigungen

Neue für iOS-10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Mit diesem Framework, kann die app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen durch eine Reihe von Bedingungen, z. B. Speicherort oder die Uhrzeit angeben.

Darüber hinaus die app oder eine Erweiterung kann empfangen (und möglicherweise ändern) sowohl lokale als auch Benachrichtigungen, wie sie iOS-Gerät des Benutzers übermittelt werden.

Der neue Benutzer Benachrichtigung Benutzeroberflächen-Framework ermöglicht die app oder App-Erweiterung, um die Darstellung des sowohl lokale als auch Benachrichtigungen anpassen, wenn sie dem Benutzer angezeigt werden.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Benutzer Benachrichtigungen Framework](~/ios/platform/user-notifications/index.md) Handbuch.

## <a name="video-subscriber-account"></a>Video Abonnentenkonto

Neue kann für iOS 10, das Video Abonnentenkonto Framework-apps dieser Unterstützung authentifiziert streaming oder Video On Demand für die Authentifizierung bei ihren Kabel- oder Satelliten-TV-Anbieter mit einem Single-anmeldeerfahrung für den Endbenutzer.

## <a name="wide-color"></a>Breite Farbskala

iOS 10 erweitert die Unterstützung für erweiterte Schlüsselbereich Pixelformate und Breite Farbskala Farbräumen im gesamten System einschließlich Frameworks wie z. B. Grafiken Core und Core-Image, Metall AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter Dienstverweise durch dieses Verhalten im ganzen Grafikstapel gesamte bereitstellen.

Darüber hinaus [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) wurde so geändert, arbeiten Sie in der neuen erweiterten **sRGB** Farbraum, die zum Mischen von Farben in wide Farbumfänge ohne signifikante Leistungseinbußen zu erleichtern.

Apple bietet die folgenden bewährten Methoden bei der Arbeit mit breiten Farben:

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/) nun verwendet der sRGB Farbspektrum und wird nicht mehr Werte clamp der `0.0` zu `1.0` Bereich. Wenn die app vom vorherigen Clamp Verhalten basiert, müssen sie für iOS 10 nicht geändert werden.
- Die Umgebung wird für den Farbraum sRGB konfiguriert werden, beim Ausführen von benutzerdefinierten `UIView` auf einem iPad Pro zeichnen.
- Wenn die app benutzerdefinierte Rendering führt `UIImages`, verwenden Sie die neue [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) Klasse, um die Verwendung der erweiterten Bereich oder Standard-Bereich Formate anzugeben.
- Beim Verwenden einer Low-Level-API z. B. Core Grafiken oder Metall, das eine bildverarbeitung bereitzustellen, muss der Entwickler einer erweiterten Bereich Farbe Speicherplatz und Pixel-Format verwenden, 16-Bit-Gleitkommawerte unterstützt, werden kann. Bei Bedarf können müssen Entwickler manuell Komponente Farbwerte clamp.
- Core-Grafiken, Core-Image und Metall Leistung Shader alle neue Methoden für die Konvertierung zwischen den zwei Farbräumen bereitstellen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in die Breite Farbskala](~/ios/platform/wide-color.md) Handbuch.

## <a name="widget-enhancements"></a>Widget-Erweiterungen

Apple wurde eingeführt, mehrere Erweiterungen für das System Widget, um sicherzustellen, dass die Widgets auf eine im Hintergrund laufende hervorragend aussehen, die auf dem neuen iOS-Sperrbildschirm 10 vorhanden ist. Die [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) Eigenschaft ist veraltet und wurde durch die neue ersetzt [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) oder [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) Eigenschaften. Darüber hinaus enthalten Widgets jetzt eine [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, kann der Entwickler wird beschrieben, wie viel Inhalt verfügbar ist, und ermöglicht es dem Benutzer zum Erweitern und reduzieren den Inhalt.

Um mehr zu erfahren, finden Sie unter unsere [Such- und Startseite Bildschirm Widget Verbesserungen](~/ios/platform/search/widgets.md) Handbuch.

## <a name="additional-framework-changes"></a>Zusätzliche Framework-Änderungen

Zusätzlich zu den wichtigsten Framework Änderungen und Ergänzungen, die oben aufgeführten machte Apple viele zusätzliche kleinere Framework Änderungen in iOS-10.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [zusätzliche Framework Änderungen](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) Handbuch.

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs sind in iOS 10 veraltet:

- Die `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` und `CKFetchRecordChangesOperation` Klassen sind in CloudKit für iOS 10 veraltet. Verwenden der [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/), [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/) und [CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/) Klassen (die Unterstützung für die Freigabe-Datensatz) stattdessen.
- Mehrere [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) APIs (z. B. Zone basierende und abfragebasierte Abonnements) sind veraltet. Verwenden der [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/) und [CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) APIs stattdessen.
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) Symbole, die im Zusammenhang mit weit verbreitete Inhalte sind veraltet.
- `ADBannerView`, `ADInterstitialAd` und zugehörigen Symbole in der [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) Klasse sind veraltet.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) Symbole, die im Zusammenhang mit Gleitkommawerte sind veraltet.
- Die `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` und `UIUserNotificationSettings` Klassen von UIKit sind veraltet. Verwenden der [Benutzerbenachrichtigungen](#User-Notifications) Framework stattdessen.
- Die `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` und `DidReceiveRemoteNotification` WatchKit Methoden sind veraltet. Verwenden der `HandleActionForNotification` und `DidReceiveNotification` Methoden stattdessen.
- Die `DidReceiveLocalNotification` und `DidReceiveRemoteNotification` Methoden die [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) sind veraltet. Erstellen Sie eine Instanz des [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) , die die entsprechenden Methoden implementiert, und weisen Sie ihn der `Delegate` Eigenschaft von der [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) Objekt.
- Die **Game Center-App** als veraltet markiert und IOS entfernt wurde. Wenn die app GameKit, verwendet er _müssen_ Darstellung eine eigene Benutzeroberfläche zum Anzeigen von GameKit-Funktionen, z. B. usw. setzen.

Finden Sie in der Apple- [iOS 9.3 verwenden, um die API-Unterschiede iOS 10.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) Dokumentation für eine vollständige Liste der veralterungen.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Was ist neu in iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
