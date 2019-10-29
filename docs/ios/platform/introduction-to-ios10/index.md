---
title: Einführung in iOS 10
description: In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in ios 10 für xamarin. IOS-Entwickler zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: ce262faf2d79e6a2cc969df582446fdc2ec29bde
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032228"
---
# <a name="introduction-to-ios-10"></a>Einführung in iOS 10

Mit dem neuen IOS 10 SDK enthält Apple neue APIs und Dienste, mit denen Entwickler neue Kategorien von apps und Features erstellen können. Eine IOS-App kann jetzt die Nachrichten-, Siri-, Phone-und Maps-apps erweitern, um den Endbenutzern, die zuvor nicht verfügbar waren, umfassende, ansprechende Funktionen bereitzustellen

Weitere Informationen zu IOS 10 finden Sie in der Dokumentation zu [IOS und apps](https://developer.apple.com/ios/) von Apple.

## <a name="whats-new-in-ios-10"></a>Neuerungen in ios 10

Apple hat mehrere neue APIs und Dienste in ios 10 sowie viele Verbesserungen an vorhandenen Features hinzugefügt, einschließlich:

## <a name="adapting-to-the-true-tone-display"></a>Anpassen an die echte Ton Anzeige

Die echte Tone-Anzeige Technologie von Apple verwendet den Umgebungslichtsensor in einem IOS-Gerät, um die Farbe und Intensität der Anzeige dynamisch anzupassen, sodass Sie den aktuellen Beleuchtungsbedingungen entspricht. IOS 10 bietet den neuen [uiwhitepointadaptivitystyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) -Schlüssel, der der `Info.plist` Datei der app hinzugefügt werden kann, und steuert, wie true Tone die Standard Farbverschiebung anwendet. 

Die folgenden Werte sind verfügbar:

- Standard **mäßig** verwenden Sie die standardmäßige-weiß Punkt-Adaptivität. `UIWhitePointAdaptivityStyleStandard`
- `UIWhitePointAdaptivityStyleReading`, die für Lese orientierte Apps verwendet werden.
- `UIWhitePointAdaptivityStyleGame` für Spiele orientierte Apps verwendet.
- `UIWhitePointAdaptivityStyleVideo`, die für Video orientierte Apps verwendet werden.
- `UIWhitePointAdaptivityStylePhoto`, die für fotografische Apps verwendet werden, bei denen die Farb Treue wichtiger ist als Umgebungs-weiß Punkt Anpassungen.

## <a name="app-extensions"></a>App-Erweiterungen

Apple hat mehrere neue APP-Erweiterungs Punkte in ios 10 bereitgestellt:

- Callverzeichnis
- Intents und Intents-Benutzeroberfläche
- Meldungen
- Benachrichtigungs Inhalt
- Notification Services
- Aufkleber Pack

Außerdem haben Erweiterungen von Drittanbieter-Tastatur-Apps die folgenden Erweiterungen:

- Die neue `DocumentInputMode`-Eigenschaft der `UITextDocumentProxy`-Klasse kann die Eingabe Sprache eines Dokuments bestimmen und die Ausrichtung der Tastatur Erweiterung an dieser Sprache ermöglichen.
- Die neue `HandleInputModeList`-Methode ermöglicht der Tastatur Erweiterung, das Tastaturauswahl Menü des Systems als Reaktion auf die getippt Kugel Taste anzuzeigen.

Weitere Informationen finden Sie in unserer [Einführung in Extensions](~/ios/platform/extensions.md), [Message App Integration](~/ios/platform/message-app-integration/index.md), [Introduction to proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md), [Introduction to Sirikit](~/ios/platform/sirikit/index.md), [Introduction to User Benachrichtigungen](~/ios/platform/user-notifications/index.md) und Apple [ Programmier Handbuch](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)für die APP-Erweiterung.

## <a name="app-search-enhancements"></a>Verbesserungen bei der App-Suche

Kernspotlight in ios 10 bietet verschiedene Verbesserungen bei der APP-Suche, wie z.b.:

- **Crowdsource Deep-Link-Beliebtheit (mit differenziellen Datenschutz)** : bietet eine Möglichkeit zum herauf Stufen von Deep-verknüpften App-Inhalten in den Suchergebnissen.
- **In-App-Suche** : Verwenden Sie die neue `CSSearchQuery`-Klasse, um eine in-App-Spotlight-Suchfunktion bereitzustellen, ähnlich der Funktionsweise von Mail-, Nachrichten-und Notizen
- **Such Fortsetzung** : ermöglicht einem Benutzer das Starten einer Suche in Spotlight oder Safari, das Öffnen einer APP und das Fortsetzen der Suche.
- **Visualisierung der Überprüfungs Ergebnisse** : das [Validierungs Tool der App Search-API](https://search.developer.apple.com/appsearch-validation-tool) von Apple zeigt nun eine visuelle Darstellung des Markups einer Website und die Deep-Verknüpfung bei der Vorbildung von Tests an.
- **Nachrichten-APP-Image Freigabe** : ermöglicht beliebte in-App-Images, die für die Freigabe in Nachrichten (über eine Nachrichten-APP-Erweiterung) bereitgestellt werden, um Sie in Spotlight

Weitere Informationen finden Sie im Leitfaden zur [App-Suche-Erweiterungen](~/ios/platform/search/app-search-enhancements.md) .

## <a name="apple-pay-enhancements"></a>Apple Pay Erweiterungen

Apple hat mehrere Verbesserungen an Apple Pay in ios 10 vorgenommen, mit denen der Benutzer sichere Zahlungen von Websites und durch Interaktion mit Siri und Maps durchführen kann.

Mit IOS 10 wurden mehrere neue APIs hinzugefügt, die mit IOS und watchos funktionieren, um dynamische Zahlungs Netzwerke und eine neue Sandkasten Testumgebung zu unterstützen.

Außerdem wurde das passkit-Framework erweitert, um Apple Pay außerhalb `UIKit` zu unterstützen, und damit Kartenaussteller Ihre Karten in ihren apps präsentieren können.

Weitere Informationen finden Sie im Leitfaden zur [Apple Pay Erweiterungen](~/ios/platform/apple-pay.md) .

## <a name="alternate-app-icons"></a>Alternative App-Symbole

Apple hat eine Reihe von Erweiterungen zu IOS 10,3 hinzugefügt, die es einer APP ermöglichen, Ihr Symbol zu verwalten:

- `ApplicationIconBadgeNumber`: Ruft das Badge des App-Symbols im Springboard ab oder legt es fest.
- `SupportsAlternateIcons`: Wenn `true` der APP ein alternativer Satz von Symbolen.
- `AlternateIconName`: gibt den Namen des alternativen Symbols zurück, das derzeit ausgewählt ist, oder `null`, wenn das primäre Symbol verwendet wird.
- `SetAlternameIconName`: Verwenden Sie diese Methode, um das Symbol der APP auf das angegebene Alternative Symbol zu wechseln.

Weitere Informationen finden Sie in unserem Leitfaden für [Alternative App-Symbole](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) .

## <a name="introduction-to-callkit"></a>Einführung in callkit

Die neue callkit-API in ios 10 bietet eine Möglichkeit für VoIP-Apps, mit der iPhone-Benutzeroberfläche zu integrieren und eine vertraute Schnittstelle und Benutzeroberfläche für den Endbenutzer bereitzustellen. Mit dieser API können Benutzer VoIP-Anrufe über den Sperrbildschirm des IOS-Geräts anzeigen und mit ihnen interagieren und Kontakte mithilfe der **Favoriten** **-und Wiederholungs** Ansichten der Phone-App verwalten.

Außerdem bietet die callkit-API die Möglichkeit, App-Erweiterungen zu erstellen, die eine Telefonnummer mit einem Namen (Aufrufer-ID) verknüpfen oder dem System mitteilen können, wenn eine Zahl blockiert werden soll (Aufruf Sperre).

Weitere Informationen finden Sie im Leitfaden [Einführung in callkit](~/ios/platform/callkit.md) .

## <a name="message-app-integration"></a>Integration von Nachrichten-Apps

IOS 10 ermöglicht die Einbindung einer Erweiterung für die Erweiterung von Nachrichten in die xamarin. IOS-Projekt Mappe, die in die **Nachrichten** -app integriert wird und dem Benutzer neue Funktionen präsentiert. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Es sind zwei Typen von Message App-Erweiterungen verfügbar:

- **Aufkleber Packs** : enthält eine Auflistung von Aufkleber, die der Benutzer einer Nachricht hinzufügen kann. Aufkleber Packs können erstellt werden, ohne Code zu schreiben.
- **IMESS Age-App** : kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-APP zum Auswählen von Stickern, zum Eingeben von Text, einschließlich Mediendateien (mit optionalen Typkonvertierungen) und zum Erstellen, bearbeiten und Senden von Interaktions Nachrichten darstellen.

Weitere Informationen finden Sie im Leitfaden für die [Message App-Integration](~/ios/platform/message-app-integration/index.md) .

## <a name="news-publisher-enhancements"></a>Erweiterungen für News Publisher

Mit IOS 10 ermöglicht Apple allen Benutzern von großen Zeit Gebern und neuen Organisationen die Registrierung und das Produkt sowie die Bereitstellung von Inhalten für die Apple News-App. Weitere Informationen finden Sie in der Dokumentation zu den [News-Ressourcen](https://newsresources.apple.com/) von Apple.

## <a name="providing-haptic-feedback"></a>Übermitteln von haptischem Feedback

Auf iPhone 7 und iPhone 7 plus enthält Apple neue, willkürliche Antworten, die zusätzliche Möglichkeiten zur physischen Einbindung des Benutzers bereitstellen. Verwenden Sie die neuen Optionen für "taktiles Feedback", um die Aufmerksamkeit des Benutzers zu erhalten und seine Aktionen zu verstärken

Mehrere integrierte Benutzeroberflächen Elemente bieten bereits haptisches Feedback wie z. b. Picker, Switches und Schieberegler. IOS 10 bietet nun die Möglichkeit, die Haptik mithilfe einer konkreten Unterklasse der `UIFeedbackGenerator` Klasse Programm gesteuert zu initiieren.

Weitere Informationen finden Sie im Leitfaden zur [Bereitstellung von willkürlichen Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md) .

## <a name="proactive-suggestions"></a>Proaktive Vorschläge

IOS 10 bietet neue Möglichkeiten, um die Einbindung in eine APP zu fördern, indem es dem System ermöglicht, den Benutzern proaktiv nützliche Informationen zu den richtigen Zeiten zu präsentieren. Ebenso wie IOS 9 die Möglichkeit bot, der App mithilfe von Spotlight-, Handoff-und Siri-Vorschlägen Deep Search hinzuzufügen, kann eine APP mit IOS 10 Funktionen verfügbar machen, die dem Benutzer vom System aus den folgenden Speicherorten angezeigt werden können:

- Der APP-Switcher
- Der Sperrbildschirm
- Carplay
- Karten
- Siri-Interaktionen
- Quick Type-Vorschläge

Eine APP macht diese Funktionalität für das System mithilfe einer Sammlung von Technologien verfügbar, wie z. b. [nsuseractivity](xref:Foundation.NSUserActivity), Web Markup, Core Spotlight, MapKit, Media Player und UIKit.

Weitere Informationen finden Sie im Leitfaden [Einführung in proaktives vorschlagen](~/ios/platform/search/proactive-suggestions.md) .

## <a name="request-app-review"></a>Anfordern der App-Prüfung

Neu bei IOS 10,3, mit der `RequestReview()`-Methode kann eine IOS-App den Benutzer auffordern, ihn zu bewerten oder zu überprüfen. Diese Methode kann zwar an jeder Stelle aufgerufen werden, an der Sie in der Benutzerfunktion sinnvoll ist, der Überprüfungsprozess wird jedoch von der App Store-Richtlinie gesteuert und behandelt. Folglich kann diese Methode eine Warnung anzeigen und nicht als Antwort auf eine Benutzeraktion aufgerufen werden, z. b. das Tippen auf eine Schaltfläche.

Weitere Informationen finden Sie in unserem Handbuch zum [anfordern von App-Überprüfungen](~/ios/platform/request-app-review.md) .

## <a name="security-and-privacy-enhancements"></a>Erweiterungen für Sicherheit und Datenschutz

Apple hat mehrere Verbesserungen hinsichtlich Sicherheit und Datenschutz in ios 10 vorgenommen, die dem Entwickler dabei helfen, die Sicherheit Ihrer apps zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten.

Folglich müssen apps, die unter IOS 10 (oder höher) ausgeführt werden, ihre Absicht, auf bestimmte Features oder Benutzerinformationen zuzugreifen, statisch deklarieren, indem Sie einen oder mehrere Datenschutz spezifische Schlüssel in den `Info.plist` Dateien eingeben, die dem Benutzer erklären, warum die APP Zugriff erhalten möchte.

Weitere Informationen finden Sie im Leitfaden zu den [Sicherheits-und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) .

## <a name="sirikit"></a>SiriKit

Mit dem neuen IOS 10 ermöglicht das Sirikit einer xamarin. IOS-APP, Dienste bereitzustellen, auf die der Benutzer mithilfe von Siri auf einem IOS-Gerät zugreifen kann. Diese Funktionalität wird in einer oder mehreren App-Erweiterungen mithilfe der neuen **Intents** und **Intents-Benutzer** Oberflächen-Frameworks bereitgestellt.

Das Sirikit unterstützt die folgenden Dienst Domänen:

- Aufrufen von Audiodaten oder Videos.
- Die Reservierung einer Fahrt.
- Verwalten von-Workouts.
- Übermittlung.
- Fotos werden gesucht.
- Senden oder empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri an einen der Dienste der APP-Erweiterung sendet, sendet Sirikit der Erweiterung ein **Intent** -Objekt, das die Anforderung des Benutzers zusammen mit allen unterstützenden Daten beschreibt. Die APP-Erweiterung generiert dann das entsprechende **Antwort** Objekt für die angegebene **Absicht**und erläutert, wie die Erweiterung die Anforderung verarbeiten kann.

Obwohl Siri in der Regel die gesamte Benutzerinteraktion verarbeitet, kann die APP-Erweiterung das **Intent UI** -Framework verwenden, um eine umfangreiche, benutzerdefinierte Benutzeroberfläche mit dem Branding der APP und zusätzlichen Informationen zu präsentieren.

Weitere Informationen finden Sie im Leitfaden [Einführung in den Sirikit](~/ios/platform/sirikit/index.md) .

## <a name="speech-recognition"></a>Spracherkennung

IOS 10 enthält eine neue Sprach-API, die es der App ermöglicht, fortlaufende sprach Erkennungs-und transkrinungs Sprache (aus Live-oder aufgezeichneten Audiodatenströmen) in Text zu unterstützen.

Da die Spracherkennung die Übertragung und temporäre Speicherung von Daten auf den Servern von Apple erfordert, _muss_ die APP die Berechtigung des Benutzers zum Durchführen der Erkennung anfordern, indem der `NSSpeechRecognitionUsageDescription` Schlüssel in seine `Info.plist` Datei eingeschlossen und die `SFSpeechRecognizer.RequestAutorization` aufgerufen wird. anzuwenden.

Weitere Informationen finden Sie in unserem Leitfaden [Einführung in die Spracherkennung](~/ios/platform/speech.md) .

## <a name="user-notifications"></a>Benutzer Benachrichtigungen

Das Benutzer Benachrichtigungs Framework ist neu bei IOS 10 und ermöglicht die Übermittlung und Handhabung von lokalen Benachrichtigungen und Remote Benachrichtigungen. Mithilfe dieses Frameworks kann die APP-oder App-Erweiterung die Übermittlung lokaler Benachrichtigungen planen, indem Sie einen Satz von Bedingungen angibt, wie z. b. den Standort oder die Uhrzeit.

Darüber hinaus kann die APP oder Erweiterung lokale Benachrichtigungen und Remote Benachrichtigungen empfangen (und potenziell ändern), wenn Sie an das IOS-Gerät des Benutzers übermittelt werden.

Das neue UI-Framework für Benutzer Benachrichtigungen ermöglicht der APP-oder App-Erweiterung, die Darstellung von lokalen Benachrichtigungen und Remote Benachrichtigungen anzupassen, wenn Sie dem Benutzer angezeigt werden.

Weitere Informationen finden Sie im Handbuch zum [Benutzer Benachrichtigungen-Framework](~/ios/platform/user-notifications/index.md) .

## <a name="video-subscriber-account"></a>Video Abonnenten Konto

Neu für IOS 10: mit dem Framework für die Video Abonnenten Konten können apps, die authentifizierte Streaming-oder Video on-Demand-Funktionen unterstützen, bei Ihrem Kabel-oder Satelliten-TV-Anbieter authentifiziert werden, indem ein einmaliger Anmeldevorgang für den Endbenutzer durchgesetzt wird.

## <a name="wide-color"></a>Breite Farbskala

IOS 10 erweitert die Unterstützung für erweiterte Pixel Formate und große Farbbereiche im gesamten System, einschließlich Frameworks wie Kern Grafiken, Core-Bild, Metal und AVFoundation. Die Unterstützung für Geräte mit breit Farbanzeige wird weiter vereinfacht, indem dieses Verhalten im gesamten Grafik Stapel bereitgestellt wird.

Außerdem wurde [UIKit](xref:UIKit) so geändert, dass Sie in den neuen erweiterten **sRGB** -colorspace funktioniert, sodass Farben in Wide Color-Gamuts ohne erheblichen Leistungsverlust einfacher gemischt werden können.

Apple bietet bei der Arbeit mit breiten Farben die folgenden bewährten Methoden:

- [Uicolor](xref:UIKit.UIColor) verwendet nun den sRGB-Farbraum und gibt keine Werte mehr an die `0.0` an `1.0` Bereich an. Wenn die APP auf dem vorherigen Verhalten der Klammer basiert, muss Sie für IOS 10 geändert werden.
- Die Zeichnungs Umgebung wird für den sRGB-Farbraum beim Ausführen benutzerdefinierter `UIView` zeichnen auf einem iPad pro konfiguriert.
- Wenn die APP ein benutzerdefiniertes Rendering von `UIImages`ausführt, verwenden Sie die neue [uigraphicsimagerender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) -Klasse, um die Verwendung der Formate für erweiterte Bereiche oder Standard Bereiche anzugeben.
- Wenn Sie eine API auf niedriger Ebene, wie z. b. Kern Grafiken oder Metal, zum Bereitstellen der Bildverarbeitung verwenden, sollte der Entwickler einen erweiterten Bereichs Farbraum und ein Pixel Format verwenden, der 16-Bit-Gleit Komma Werte unterstützt. Bei Bedarf muss der Entwickler die Farbkomponenten Werte manuell einspannen.
- Haupt Grafiken, Kern Bild-und Metal-leistungshader bieten neue Methoden zum Wechseln zwischen den beiden Farbräumen.

Weitere Informationen finden Sie im Leitfaden [Einführung in Wide Color](~/ios/platform/wide-color.md) .

## <a name="widget-enhancements"></a>Widgeerweiterungen

Apple hat mehrere Verbesserungen am Widget-System eingeführt, um sicherzustellen, dass die Widgets in jedem Hintergrund, der auf dem neuen IOS 10-Sperrbildschirm vorhanden ist, hervorragend aussehen. Die [notificationcentervibrancyeffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) -Eigenschaft wurde als veraltet markiert und durch die neuen [widgetprimaryvibrancyeffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) -oder [widgetsecondaryvibrancyeffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) -Eigenschaften ersetzt. Außerdem enthalten Widgets nun eine [ncwidgetdisplaymode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, mit der Entwickler beschreiben können, wie viel Inhalt verfügbar ist und der Benutzer den Inhalt erweitern und reduzieren kann.

Weitere Informationen finden Sie im Handbuch zum [Durchsuchen und Verbesserungen des Startbildschirms](~/ios/platform/search/widgets.md) .

## <a name="additional-framework-changes"></a>Weitere frameworkänderungen

Zusätzlich zu den oben aufgeführten wichtigen frameworkänderungen und Ergänzungen hat Apple in ios 10 viele zusätzliche Änderungen am geringfügigen Framework vorgenommen.

Weitere Informationen finden Sie in unserem Handbuch zu [zusätzlichen Framework-Änderungen](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) .

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs wurden in ios 10 als veraltet markiert:

- Die Klassen `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` und `CKFetchRecordChangesOperation` wurden in cloudkit für IOS 10 als veraltet markiert. Verwenden Sie stattdessen die Klassen [ckdiscoveralluseridentitiesoperation](xref:CloudKit.CKDiscoverUserIdentitiesOperation), [ckuseridentity](xref:CloudKit.CKUserIdentity) und [ckfetchrecordzonechangesoperation](xref:CloudKit.CKFetchRecordZoneChangesOperation) (die die Daten Satz Freigabe unterstützen).
- Mehrere [ckabonnement](https://developer.apple.com/reference/cloudkit/cksubscription) -APIs (z. b. zonenbasierte und Abfrage basierte Abonnements) sind veraltet. Verwenden Sie stattdessen die [ckrecordzonesub-](xref:CloudKit.CKRecordZoneSubscription) und [ckqueryabonnement-](xref:CloudKit.CKQuerySubscription) APIs.
- [Nspersistentstorecoordinator](xref:CoreData.NSPersistentStoreCoordinator) -Symbole in Bezug auf den universellen Inhalt wurden als veraltet markiert.
- `ADBannerView`, `ADInterstitialAd` und verwandte Symbole in der [UIViewController](xref:UIKit.UIViewController) -Klasse sind veraltet.
- [Skuniform](https://developer.apple.com/reference/spritekit/skuniform) -Symbole im Zusammenhang mit Gleit Komma Werten sind veraltet.
- Die `UILocalNotification`-, `UIMutableUserNotificationAction`-, `UIMutableUserNotificationCategory`-, `UIUserNotificationAction`-, `UIUserNotificationCategory`-und `UIUserNotificationSettings`-Klassen von UIKit sind veraltet. Verwenden Sie stattdessen das [Benutzer Benachrichtigungs](#user-notifications) Framework.
- Die `HandleActionForLocalNotification`-, `HandleActionForRemoteNotification`-, `DidReceiveLocalNotification`-und `DidReceiveRemoteNotification` watchkit-Methode sind veraltet. Verwenden Sie stattdessen die Methoden `HandleActionForNotification` und `DidReceiveNotification`.
- Die Methoden `DidReceiveLocalNotification` und `DidReceiveRemoteNotification` von [wkextensiondelegaten](https://developer.apple.com/reference/watchkit/wkextensiondelegate) sind veraltet. Erstellen Sie eine Instanz von [unusernotificationcenterdelegat](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) , die die entsprechenden Methoden implementiert, und weisen Sie Sie der `Delegate`-Eigenschaft des [unusernotificationcenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) -Objekts zu.
- Die **Game Center-App** wurde als veraltet markiert und von Ios entfernt. Wenn die APP GameKit verwendet, _muss_ Sie eine eigene Schnittstelle zum Anzeigen von GameKit-Features, z. b. Bestenlisten usw., enthalten.

Eine komplette Liste der veralteten Anwendungen finden [Sie in der Dokumentation zu den IOS 9,3 to Ios 10,0 API-unterschieden](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) von Apple.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
