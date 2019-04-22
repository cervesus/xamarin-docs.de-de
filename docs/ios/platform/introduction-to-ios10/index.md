---
title: Einführung in iOS 10
description: Dieser Artikel enthält alle neuen und geänderten APIs und Funktionen verfügbar sind, unter iOS 10 für Xamarin.iOS-Entwickler.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: b018fe343a7d46f1323119b03a22cc3831a02d9f
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870247"
---
# <a name="introduction-to-ios-10"></a>Einführung in iOS 10

_Dieser Artikel enthält alle neuen und geänderten APIs und Funktionen verfügbar sind, unter iOS 10 für Xamarin.iOS-Entwickler._

## <a name="introducing-ios-10"></a>Einführung in iOS 10

Mit dem neuen iOS hat 10 SDK, Apple enthalten neue APIs und Dienste, die den Entwickler zum Erstellen von neuer Kategorien von apps und Features zu ermöglichen. Eine iOS-app kann nun erweitern, der Meldungen "," Siri "," Telefon "und" Karten-apps, um komplexe und beeindruckende Funktionalität für den Endbenutzer bereitzustellen, die zuvor nicht verfügbar waren.

Weitere Informationen zu iOS 10, finden Sie unter Apple [iOS + Apps](https://developer.apple.com/ios/) Dokumentation.

## <a name="whats-new-in-ios-10"></a>Was ist neu unter iOS 10

Apple hat verschiedene neue APIs und Dienste unter iOS 10 sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:


## <a name="adapting-to-the-true-tone-display"></a>Anpassen der Anzeige von "true" Ton

"True" Ton Anzeigen von Apple-Technologie der ambient-lichtsensor in einem iOS-Gerät verwendet, um dynamisch anpassen, die Farbe und Intensität der Anzeige entsprechend der aktuellen Lichtverhältnissen verwendet wird. iOS 10 bietet die neuen [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) Schlüssel, die der app hinzugefügt werden können `Info.plist` Datei, und steuert, wie die Standardfarbe Schicht auf "true" Ton angewendet wird. 

Die folgenden Werte sind verfügbar:

- `UIWhitePointAdaptivityStyleStandard` **Standard** -verwenden Sie die standard-Weißpunkt Adaptivität.
- `UIWhitePointAdaptivityStyleReading` : Wird für das Lesen ausgerichteten apps verwendet.
- `UIWhitePointAdaptivityStyleGame` : Wird für apps mit Fokus auf Spielen verwendet.
- `UIWhitePointAdaptivityStyleVideo` : Wird für Video ausgerichtete apps verwendet.
- `UIWhitePointAdaptivityStylePhoto` – Verwendet für Fotografie ausgerichtete apps Farbtreue wichtiger als die Umwelt Weißpunkt Anpassungen.

<a name="app-extensions" />

## <a name="app-extensions"></a>App-Erweiterungen

Apple hat mehrere neue Erweiterungspunkte unter iOS 10 bereitgestellt werden:

- Rufen Sie Verzeichnis
- Intents und Intents UI
- Meldungen
- Benachrichtigungsinhalt
- Notification Services
- Aufkleber Pack

3. Tastatur App-Erweiterungen von Drittanbietern verfügen darüber hinaus die folgenden Verbesserungen:

- Die neue `DocumentInputMode` Eigenschaft der `UITextDocumentProxy` Klasse können Sie bestimmen die Eingabesprache eines Dokuments und ermöglichen die Tastatur-Erweiterung, die dieser Sprache entsprechen.
- Die neue `HandleInputModeList` -Methode können Sie die Tastatur Erweiterung Menü zur Auswahl des Systems Tastatur als Reaktion auf der ganzen Welt Schlüssel getippt wird angezeigt.

Weitere Informationen finden Sie unter unserem [Einführung in die Erweiterungen](~/ios/platform/extensions.md), [App Nachrichtenintegration](~/ios/platform/message-app-integration/index.md), [Einführung in die proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md), [ Einführung in SiriKit](~/ios/platform/sirikit/index.md), [Einführung in Benutzerbenachrichtigungen](~/ios/platform/user-notifications/index.md) und Apple [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>Verbesserungen bei der App-Suche

Core Spotlight IOS 10 bietet mehrere Erweiterungen wie z. B. zum App-Suche an:

- **Per Crowdsourcing gesammelter Kommentare Deep-Link Beliebtheit (mit differenziellen Privacy)** -bietet eine Möglichkeit zur Förderung der Deep-Link-app-Inhalte in den Suchergebnissen angezeigt.
- **In-App-Suche** -verwenden Sie die neue `CSSearchQuery` Klasse zu in-app-Spotlight-Suche Möglichkeit, die ähnlich wie die E-Mail, Nachrichten und Anmerkungen zu dieser apps arbeiten.
- **Suchen Sie die Fortsetzung** – ermöglicht Benutzern das Starten einer Suche im Blickpunkt oder Safari, und öffnen Sie eine app, und diese Suche fortsetzen.
- **Visualisierung von Überprüfungsergebnissen** -Apple [App-Suche-API-Überprüfungstool](https://search.developer.apple.com/appsearch-validation-tool) zeigt nun eine visuelle Darstellung von Markup und Deep linking einer Website aus, bei der Herstellung von Tests.
- **Nachrichten-App Teilen von Bildern** -beliebten in-app-Images bereitgestellt werden, für die Freigabe in Nachrichten (über eine Nachrichten-App-Erweiterung), bei der Spotlight-Suche angezeigt werden können.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Verbesserungen bei der Suche von App](~/ios/platform/search/app-search-enhancements.md) Handbuch.

## <a name="apple-pay-enhancements"></a>Apple Pay-Verbesserungen

Apple hat mehrere Erweiterungen zu Apple Pay in iOS 10 vorgenommen, die der Benutzer auf sichere Zahlungen von Websites und durch die Interaktion mit Siri und Zuordnungen haben.

Mit iOS 10 wurden mehrere neue APIs mit iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-Test-Umgebung hinzugefügt.

Darüber hinaus wurde das PassKit-Framework zur Unterstützung von Apple Pay außerhalb von erweitert `UIKit` und belastungsrisiken präsentieren Sie ihre Karten in ihren apps zu ermöglichen.

Um mehr zu erfahren, informieren Sie sich unsere [Apple Zahlen Verbesserungen](~/ios/platform/apple-pay.md) Guide.

## <a name="alternate-app-icons"></a>Alternative App-Symbole

Apple hat mehrere Erweiterungen auf iOS 10.3 hinzugefügt, mit denen eine app zum Verwalten von das entsprechende Symbols:

 - `ApplicationIconBadgeNumber` -Ruft ab oder legt den Badge, der das app-Symbol in der Springboard-Reihe.
 - `SupportsAlternateIcons` -If `true` die app verfügt über eine Alternative Gruppe von Symbolen.
 - `AlternateIconName` -Gibt den Namen des aktuell ausgewählten alternativen Symbols zurück oder `null` Wenn das primäre Symbol verwenden.
 - `SetAlternameIconName` – Verwenden Sie diese Methode, um das Symbol der app in der angegebenen alternativen Symbol wechseln.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [alternative App-Symbole](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) Guide.


## <a name="introduction-to-callkit"></a>Einführung in die CallKit

Die neue CallKit-API unter iOS 10 bietet eine Möglichkeit für VOIP-apps Sie mit der iPhone-Benutzeroberfläche und integrieren und bieten eine vertraute Schnittstelle für den Endbenutzer auftreten. Mit dieser API können Benutzer können und VOIP-Anrufe über des iOS-Geräts der Sperrbildschirm interagieren Kontakte anzeigen und Verwalten der Phone-app mit **Favoriten** und **zuletzt** Ansichten.

Darüber hinaus bietet der CallKit-API die Fähigkeit zum Erstellen von App-Erweiterungen, die eine Telefonnummer einen Namen (Anrufer-ID) zuordnen kann, und erkennen Sie, dass das System, wenn eine Zahl sein muss (aufgerufen durch blockieren) blockiert.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Callkit – Einführung](~/ios/platform/callkit.md) Guide.

## <a name="message-app-integration"></a>Integration von Nachrichten-Apps

iOS 10 ermöglicht die Einbeziehung einer Nachrichten-App-Erweiterung in der Xamarin.iOS-Projektmappe, die integriert werden, die **Nachrichten** -app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Es stehen zwei Arten von Nachrichten-App-Erweiterung:

- **Aufkleber Packs** -enthält eine Auflistung von Aufkleber, die der Benutzer eine Nachricht hinzugefügt werden kann. Aufkleber Packs erstellt werden, ohne Code schreiben zu müssen.
- **iMessage App** -kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-app für Aufkleber auswählen, Text eingeben, einschließlich Media-Dateien (mit optionalen typkonvertierungen) und erstellen, bearbeiten und Senden von Nachrichten von Interaktion darstellen.

Um mehr zu erfahren, informieren Sie sich unsere [Message-App-Integration](~/ios/platform/message-app-integration/index.md) Guide.

## <a name="news-publisher-enhancements"></a>Verbesserungen der News-Verleger

Mit iOS 10 wird die Apple zulassen von wichtigen Zeitschriften und neue Unternehmen Blogger und unabhängiger Herausgeber zu registrieren und Produkt und Übermitteln von Inhalten an Apple News-app. Weitere Informationen finden Sie unter Apple [News Ressourcen](https://newsresources.apple.com/) Dokumentation.

## <a name="providing-haptic-feedback"></a>Übermitteln von haptischem Feedback

Auf dem iPhone 7 und das iPhone hat 7 und Apple enthalten neue Haptics-Antworten, die weitere Möglichkeiten, die sich physisch in Verbindung setzen den Benutzer bereitstellen. Verwenden Sie die neue praktisch Feedbackoptionen, die Aufmerksamkeit des Benutzers zu erhalten, und vertiefen ihre Aktionen.

Einige integrierte Elemente der Benutzeroberfläche bieten bereits Übermitteln von haptischem Feedback wie z. B. Dateiöffnungs-, Switches und Schieberegler. iOS 10 bietet nun die Möglichkeit, programmgesteuert mithilfe einer konkreten Unterklasse von Haptics Auslösen der `UIFeedbackGenerator` Klasse.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Übermitteln von Haptischem Feedback bereitstellen](~/ios/user-interface/ios-ui/haptic-feedback.md) Guide.

## <a name="proactive-suggestions"></a>Proaktive Vorschläge

iOS 10 stellt neue Methoden für die treibende Engagement zu einer app durch den Wechsel des Systems proaktiv hilfreiche Informationen automatisch an den Benutzer zur richtigen Zeit jeweils vorhanden. Wie iOS bereitgestellten 9 die Möglichkeit zum Hinzufügen von detaillierten Suche der App mit Spotlight, Übergabeanimationen und Vorschläge für Siri mit iOS 10-app Funktionalität verfügbar gemacht werden können, die für dem Benutzer durch das System in den folgenden Speicherorten angezeigt werden kann:

- Der App-Switcher
- Der Sperrbildschirm
- CarPlay
- Karten
- Siri-Interaktionen
- QuickType Vorschläge 

Eine app macht diese Funktion in das System für eine Sammlung von Technologien wie z. B. über [NSUserActivity](xref:Foundation.NSUserActivity), Markup im Web, Core Spotlight, MapKit, Media Player und UIKit.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in die proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md) Guide.

## <a name="request-app-review"></a>Anfordern der App-Prüfung

Noch nicht mit iOS 10.3, die `RequestReview()` Methode ermöglicht eine iOS-app, um den Benutzer zu bewerten, oder Überprüfen sie. Während dieser Methode zu einem beliebigen Zeitpunkt aufgerufen werden kann, wo es sinnvoll, auf der Benutzeroberfläche ist, ist der Überprüfungsprozess gesteuert und vom App Store-Richtlinie verarbeitet. Daher diese Methode kann eine Warnung kann nicht angezeigt und sollte nie aufgerufen werden, als Reaktion auf eine Benutzeraktion, z. B. auf eine Schaltfläche tippen.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [App Review anfordern](~/ios/platform/request-app-review.md) Guide.

## <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Verbesserungen

Apple hat mehrere Verbesserungen vorgenommen, sowohl Sicherheit und Datenschutz in iOS 10, die den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten, die helfen.

Apps, die unter iOS 10 (oder höher) müssen daher statisch deklarieren ihrer Absicht, bestimmte Features oder Benutzerinformationen zugreifen, indem Sie die Eingabe eine oder mehrere bestimmte Datenschutzschlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Verbesserungen Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md) Guide.

## <a name="sirikit"></a>SiriKit

Neue iOS 10, SiriKit kann eine Xamarin.iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri auf einem iOS-Gerät zugänglich sind. Diese Funktion finden Sie im App-Erweiterung für eine oder mehrere mit dem neuen **Intents** und **Intents UI** Frameworks.

SiriKit unterstützt die folgenden Service-Domänen:

- Audio- oder Videoanruf.
- Buchung einer Tour an.
- Verwalten von fitnessaktivitäten aus.
- Messaging.
- Durchsuchen von Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri im Zusammenhang mit einer der Dienste für die App-Erweiterung stellt, sendet SiriKit die Erweiterung einer **Absicht** Objekt, das die Anforderung des Benutzers sowie alle unterstützungsdaten beschreibt. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Informationen, wie die Erweiterung für die Anforderung verarbeiten kann.

Siri in der Regel alle Benutzerinteraktionen behandelt, die App-Erweiterung können jedoch die **beabsichtigte Benutzeroberfläche** Framework, um eine umfassende, benutzerdefinierte Benutzeroberfläche mit dem branding die app zu präsentieren und zusätzliche Informationen.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in SiriKit](~/ios/platform/sirikit/index.md) Guide.

## <a name="speech-recognition"></a>Spracherkennung

iOS 10 bietet es sich um eine neue Spracherkennungs-API, die die app fortlaufende Spracherkennung unterstützen und transkribieren Speech (von Livedaten oder aufgezeichnete Audiodatenströme) in Text ermöglicht.

Da Spracherkennung die Übertragung und die temporäre Speicherung von Daten auf Apple Servern, die app muss _müssen_ fordern Sie die Berechtigung des Benutzers zum Ausführen der Erkennung durch Einschließen der `NSSpeechRecognitionUsageDescription` -Schlüssel in der `Info.plist` Datei- und Aufrufen der `SFSpeechRecognizer.RequestAutorization` Methode.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in die Spracherkennung](~/ios/platform/speech.md) Handbuch.

## <a name="user-notifications"></a>Benutzerbenachrichtigungen

Noch nicht mit iOS 10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Durch die Verwendung dieses Frameworks kann der app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen eine Reihe von Bedingungen wie z. B. Speicherort oder Uhrzeit angeben.

Darüber hinaus die app oder die Erweiterung kann empfangen (und potenziell ändern) sowohl lokale als auch Benachrichtigungen, wie sie iOS-Gerät des Benutzers übermittelt werden.

Das neue Benachrichtigung User Interface, UI-Framework ermöglicht der app oder App-Erweiterung, die Darstellung von lokalen und Remotecomputern Benachrichtigungen anpassen, wenn sie dem Benutzer angezeigt werden.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Framework für Benutzerbenachrichtigungen](~/ios/platform/user-notifications/index.md) Guide.

## <a name="video-subscriber-account"></a>Video-Abonnentenkonto

Neue kann für iOS 10, das Video-Abonnentenkonto Framework apps diese Unterstützung authentifiziert streaming oder Video On Demand, um bei ihrem Kabel- oder Satelliten TV-Anbieter, die mit einer einzelnen-Anmeldung für den Endbenutzer zu authentifizieren.

## <a name="wide-color"></a>Breite Farbskala

iOS 10 erweitert die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System einschließlich Frameworks wie Core Graphics "," Core-Image ","-Metal-Computern "und" AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

Darüber hinaus [UIKit](xref:UIKit) wurde geändert, arbeiten Sie in der neuen erweiterten **sRGB** Farbraum, erleichtert Ihnen die zum Mischen von Farben in der Breite Farbskala Farbumfänge ohne signifikante Leistungseinbußen zur Folge.

Apple bietet die folgenden bewährten Methoden bei der Arbeit mit breiten Farben:

- [UIColor](xref:UIKit.UIColor) nun clamp-verwendet der sRGB-Farbraum und nicht mehr Werte für die `0.0` zu `1.0` Bereich. Wenn die app vom vorherigen clamps-Verhalten basiert, müssen sie für iOS 10 geändert werden.
- Die Umgebung wird für den sRGB-Farbraum konfiguriert werden, beim Ausführen von benutzerdefinierten `UIView` auf einem iPad Pro zeichnen.
- Wenn die Anwendung benutzerdefinierte Rendering von führt `UIImages`, verwenden Sie die neue [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) Klasse, um die Verwendung der Formate erweiterte-Range- oder Standard-Bereich anzugeben.
- Eine Low-Level-API, z. B. die wichtigste Grafik oder Metall mit bildverarbeitung bereitzustellen, muss der Entwickler eine erweiterte Palette Farbe Speicherplatz und Pixel-Format verwenden, 16-Bit-Gleitkommawerte unterstützt, werden kann. Ggf. müssen der Entwickler clamp-Farbkomponentenwerte manuell.
- Wichtigste Grafik "," Core-Image "und"-Metal-Leistung Shader neue Methoden bieten für die Konvertierung zwischen den zwei Farbräume ".

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in die Breite Farbskala](~/ios/platform/wide-color.md) Guide.

## <a name="widget-enhancements"></a>Widget-Verbesserungen

Apple wurden mehrere Erweiterungen für das Widget-System, um sicherzustellen, dass die Widgets auf eine im Hintergrund laufende suchen, die auf dem neuen iOS 10 Sperrbildschirm vorhanden ist. Die [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) Eigenschaft ist veraltet und wurde durch die neue ersetzt [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) oder [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) Eigenschaften. Darüber hinaus enthalten Widgets jetzt eine [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, kann der Entwickler beschrieben, wie viel Inhalt verfügbar ist, und ermöglicht dem Benutzer zum Erweitern und reduzieren den Inhalt.

Um mehr zu erfahren, informieren Sie sich unsere [Such- und Startseite Bildschirm Widget Verbesserungen](~/ios/platform/search/widgets.md) Guide.

## <a name="additional-framework-changes"></a>Zusätzliche-Frameworkänderungen

Zusätzlich zu den wichtigsten Framework-Änderungen und Ergänzungen, die oben aufgeführten hat Apple viele weitere kleinere frameworkänderungen unter iOS 10 vorgenommen.

Um mehr zu erfahren, informieren Sie sich unsere [zusätzliche-Frameworkänderungen](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) Guide.

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs verfügen unter iOS 10 als veraltet markiert:

- Die `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` und `CKFetchRecordChangesOperation` Klassen wurden in CloudKit für iOS 10 eingestellt. Verwenden der [CKDiscoverAllUserIdentitiesOperation](xref:CloudKit.CKDiscoverUserIdentitiesOperation), [CKUserIdentity](xref:CloudKit.CKUserIdentity) und [CKFetchRecordZoneChangesOperation](xref:CloudKit.CKFetchRecordZoneChangesOperation) Klassen (die Unterstützung von Datensatz freigeben) stattdessen.
- Mehrere [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) APIs (z. B. Zone und Abfrage-basierten Abonnements) sind veraltet. Verwenden der [CKRecordZoneSubscription](xref:CloudKit.CKRecordZoneSubscription) und [CKQuerySubscription](xref:CloudKit.CKQuerySubscription) APIs stattdessen.
- [NSPersistentStoreCoordnator](xref:CoreData.NSPersistentStoreCoordinator) Symbole, die im Zusammenhang mit der weit verbreitete Inhalte sind veraltet.
- `ADBannerView`, `ADInterstitialAd` und zugehörigen Symbole in der [UIViewController](xref:UIKit.UIViewController) Klasse sind veraltet.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) Symbole, die im Zusammenhang mit Gleitkommawerte sind veraltet.
- Die `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` und `UIUserNotificationSettings` Klassen von UIKit sind veraltet. Verwenden der [Benutzerbenachrichtigungen](#user-notifications) Framework stattdessen.
- Die `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` und `DidReceiveRemoteNotification` WatchKit-Methoden sind veraltet. Verwenden der `HandleActionForNotification` und `DidReceiveNotification` Methoden stattdessen.
- Die `DidReceiveLocalNotification` und `DidReceiveRemoteNotification` Methoden der [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) sind veraltet. Erstellen Sie eine Instanz des ["unusernotificationcenterdelegate"](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) , die die entsprechenden Methoden implementiert, und weisen sie Sie der `Delegate` Eigenschaft der [mit dem UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) Objekt.
- Die **Game Center-App** als veraltet markiert und aus iOS entfernt wurde. Wenn die app GameKit, verwendet es _müssen_ eine eigene Benutzeroberfläche zum Anzeigen von GameKit Features wie Bestenlisten usw. vorhanden.

Finden Sie unter Apple [iOS 9.3 verwenden, um die API-Unterschiede iOS 10.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) Dokumentation für eine vollständige Liste der veralteten.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Neuigkeiten in iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
