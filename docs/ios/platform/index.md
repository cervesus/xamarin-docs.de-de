---
title: Übersicht über die iOS-Plattform-features
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, die Funktionen, die in verschiedenen Versionen von iOS und andere Features der iOS-Plattform beschrieben werden.
ms.prod: xamarin
ms.assetid: 9F6A27E5-8A87-ADE2-D1EF-5684E7B8C999
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/25/2018
ms.openlocfilehash: 6703e922a628504e0afdcf56533d74741131581a
ms.sourcegitcommit: a6ba6ed086bcde4f52fb05f83c59c68e8aa5e436
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540156"
---
# <a name="ios-platform-features-overview"></a>Übersicht über die iOS-Plattform-features

Diese Seite listet die aktuelle iOS-releases sowie besonders hervorgehoben werden einige der Apple-Frameworks können Sie mit Xamarin.iOS zugreifen.

## <a name="ios-releases"></a>iOS-Versionen

|  |  |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Einführung in iOS-13 (Vorschau)](~/ios/platform/ios13/index.md) | Dieses Dokument beschreibt die Xamarin.iOS-13 (Vorschau).|
| [Einführung in iOS 12](~/ios/platform/introduction-to-ios12/index.md) | Dieses Dokument beschreibt die iOS-12-Features zur Verwendung beim Erstellen von Xamarin.iOS-Anwendungen.|
| [Einführung in iOS 11](~/ios/platform/introduction-to-ios11/index.md) | Dieses Dokument beschreibt die neuen und aktualisierten Funktionen in iOS 11 und Xcode 9, wie ARKit, Core ML, Core NFC, Drag und Drop, MapKit, PDFKit, SiriKit und maschinelles sehen. Es enthält links zu Leitfäden, in denen beschrieben, wie diese Funktionen mit Xamarin.iOS verwendet werden. |
| [Einführung in iOS 10](~/ios/platform/introduction-to-ios10/index.md) | iOS 10 umfasst verschiedene neue APIs und Dienste, die Sie zum Entwickeln von apps mit neuen Features und Funktionen zu ermöglichen. Mit iOS 10 haben die apps neue Fähigkeiten wie das Erweitern von Karten "," Nachrichten "," Telefon "und" Siri. In diesem Abschnitt wird gezeigt, wie diese Features in einer Xamarin.iOS-app nutzen. |
| [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)   | Dieser Abschnitt definiert die Änderungen in iOS 9, bei der Aktualisierung von iOS 8 und wie Sie diese Funktionen in einer Xamarin.iOS-app verwenden. |
| [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)         | iOS 8, die eine große Anzahl von Änderungen für das Betriebssystem aus iOS 7 erstellt wird. Hier zeigen wir diese Kategorien und deren Verwendung. |
| [Einführung in iOS 7](~/ios/platform/introduction-to-ios7/index.md)   | Über die wichtigsten neuen APIs, die in iOS 7 eingeführt wurden einschließlich der Ansichtscontroller geht über, Verbesserungen bei der UIView-Animationen, UIKit Dynamics und Text-Kit. |
| [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)   | Erläuterungen zu den Funktionen, die in iOS 6, einschließlich der Auflistungsansichten, Kit übergeben, Ereignis-Kit und das Social-Framework. |

## <a name="apple-payiosplatformapple-paymd"></a>[Apple Pay](~/ios/platform/apple-pay.md)

Apple Pay, wurde zusammen mit iOS 8 eingeführt, Benutzern das bezahlen physischer Güter wie Futter, Unterhaltung und Mitgliedschaften über ihre iOS-Geräte aktivieren. Es ist verfügbar, auf dem iPhone 6 und iPhone 6 Plus, und Sie können auch mit der Apple Watch-Store-Einkäufe kombiniert werden. Wenn auf einem iPhone verwendet wird, verwendet er Touch ID als eine Möglichkeit, um zu bestätigen und Transaktionen eines Benutzers Kredit- oder Debitkarte zu autorisieren.

## <a name="callkitiosplatformcallkitmd"></a>[CallKit](~/ios/platform/callkit.md)

Die neue CallKit-API unter iOS 10 bietet eine Möglichkeit für VOIP-apps Sie mit der iPhone-Benutzeroberfläche und integrieren und bieten eine vertraute Schnittstelle für den Endbenutzer auftreten. Mit dieser API Benutzer können und VOIP-Anrufe über des iOS-Geräts der Sperrbildschirm interagieren Kontakte anzeigen und Verwalten der Phone-app mit **Favoriten** und **zuletzt** Ansichten.

## <a name="contacts-and-contactsuiiosplatformcontactsmd"></a>[Kontakte und ContactsUI](~/ios/platform/contacts.md)

Apple hat mit der Einführung von iOS 9 sind zwei neue Frameworks vorgestellt, `Contacts` und `ContactsUI`, ersetzen Sie das vorhandene Adressbuch und Adressbuch Benutzeroberflächen-Frameworks von iOS 8 und frühere Versionen verwendet.

## <a name="document-pickeriosplatformdocument-pickermd"></a>[Dokumentauswahl](~/ios/platform/document-picker.md)

Die Dokumentauswahl können Dokumente apps gemeinsam genutzt werden. Diese Dokumente können in iCloud oder in eine andere app-Verzeichnis gespeichert werden. Dokumente werden gemeinsam genutzt, über den Satz von [Dokument-Anbietererweiterungen](~/ios/platform/extensions.md) der Benutzer hat auf ihrem Gerät installiert.

## <a name="eventkitiosplatformeventkitmd"></a>[EventKit](~/ios/platform/eventkit.md)

iOS enthält zwei kalenderbezogenen Anwendungen integrierten: die Kalender-Anwendung, und die Erinnerungen-Anwendung. Es ist einfach genug, um zu verstehen, wie der Kalenderanwendung Kalenderdaten verwaltet, aber die Erinnerungen-Anwendung ist weniger offensichtlich. Erinnerungen Datumsangaben fällig, wenn sie abgeschlossen sind, werden diese in Form von zugeordnet haben usw. Daher iOS alle Kalenderdaten, speichert, ob sie Termine im Kalender oder Erinnerungen an einem Ort, dem Namen der *Kalenderdatenbank*.

## <a name="ios-extensionsiosplatformextensionsmd"></a>[iOS-Erweiterungen](~/ios/platform/extensions.md)

Erweiterungen in iOS 8 eingeführten sind speziell `UIViewControllers` , sind präsentiert von iOS in standard-Kontexten z. B. wie in der **Mitteilungszentrale**, wie spezielle selbstdefinierten Typen, die vom Benutzer angefordert wird, ausführen Eingabe oder eine andere Kontexte wie bearbeiten ein Foto, in dem die Erweiterung Spezialeffekt Filter bereitstellen kann.

## <a name="graphics-and-animation-in-iosiosplatformgraphics-animation-iosindexmd"></a>[Grafiken und Animationen in iOS](~/ios/platform/graphics-animation-ios/index.md)

Grafiken und Animationen in iOS behandelt grundlegende Graphics-Konzepte in iOS z. B. CoreImage, Core Graphics und Core-Animation.

## <a name="handoffiosplatformhandoffmd"></a>[Handoff](~/ios/platform/handoff.md)

Apple führte Übergabe in iOS 8 und OS X Yosemite (10.10), geben Sie einen allgemeinen Mechanismus für den Benutzer zum Übertragen von Aktivitäten, die Schritte auf einem ihrer Geräte, auf ein anderes Gerät mit der gleichen app oder einer anderen app, die von der gleichen Aktivität unterstützt.

## <a name="healthkitiosplatformhealthkitmd"></a>[HealthKit](~/ios/platform/healthkit.md)

Integrität Kit bietet einen sicheren Datenspeicher für integritätsbezogene Informationen des Benutzers. Integrität Kit apps können mit expliziten Berechtigungen des Benutzers, lesen und Schreiben in diese Datenspeicher und Benachrichtigungen erhalten, wenn die relevante Daten, die hinzugefügt werden. Apps können die Darstellung der Daten oder des Benutzers kann von Apple bereitgestellten Integrität app verwenden, um ein Dashboard für alle ihre Daten anzeigen.

## <a name="homekitiosplatformhomekitmd"></a>[HomeKit](~/ios/platform/homekit.md)

Apple führte HomeKit IOS 8, um ein gemeinsames Framework zum Ermitteln und die Kommunikation mit heimautomatisierungsgeräte in Wohnung des Benutzers bereitzustellen. HomeKit bietet es sich um eine gemeinsame Plattform für die Konfiguration von Geräten und Einrichten von Aktionen zu steuern.

## <a name="in-app-purchasingiosplatformin-app-purchasingindexmd"></a>[In-app-Käufe](~/ios/platform/in-app-purchasing/index.md)

iOS-Anwendungen können verkaufen, digitalen Produkten oder Diensten, die mithilfe von StoreKit – eine Reihe von APIs unter iOS, die die Kommunikation mit Apple Server bereitgestellt, Finanztransaktionen, mit dem Benutzer über ihre Apple-ID durchzuführen. Die StoreKit-APIs sind in erster Linie mit Produktinformationen abrufen und Durchführen von Transaktionen befassen: Es gibt keine Benutzeroberfläche-Komponente. Anwendungen, die in-app-Käufe implementieren, müssen ihre eigene Benutzeroberfläche erstellen und verfolgen gekauften Artikel mit benutzerdefiniertem Code, dem Benutzer die erforderlichen Produkte oder Dienste anbieten.

## <a name="ios-gaming-apisiosplatformgamingindexmd"></a>[iOS-gaming-APIs](~/ios/platform/gaming/index.md)

Apple hat mehrere technologische Verbesserungen an der Gaming-APIs unter iOS 9 vorgenommen, die Grafiken und Audio in einer Xamarin.iOS-app implementieren zu vereinfachen. Dazu gehören sowohl einfache Entwicklung über die allgemeine Frameworks und der leitungsfähigkeit von dem iOS-Gerät GPU für verbesserte Geschwindigkeit und grafischen Möglichkeiten.

## <a name="message-app-integrationiosplatformmessage-app-integrationindexmd"></a>[Integration von Apps](~/ios/platform/message-app-integration/index.md)

Neue IOS 10, integriert eine Nachrichten-App-Erweiterung der **Nachrichten** -app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="multitasking-for-ipadiosplatformmultitaskingmd"></a>[Multitasking für iPad](~/ios/platform/multitasking.md)

iOS 9 fügt Multitasking-Unterstützung für die Ausführung von zwei apps gleichzeitig auf bestimmte iPad-Hardware. Multitasking für iPad wird über die folgenden Funktionen unterstützt: Folie über "," geteilte Ansicht "und" Picture in Picture ".

## <a name="passkitiosplatformpasskitmd"></a>[PassKit](~/ios/platform/passkit.md)

Passbook ist eine app für iPhones und iPod mit iOS 6 berührt. Er speichert und Anzeigen von Barcodes und anderen Informationen zum Verknüpfen von Kundentransaktionen auf seinem Telefon mit der realen Welt. Übergeben von Händlern generiert und gesendet, um den Kunden per e-Mail, URLs oder aus einer iOS-app für die Händler. Passbook speichert und organisiert werden alle übergibt, die sich auf einem Smartphone und Pass-Erinnerungen angezeigt, auf dem Sperrbildschirm, je nach Datum/Uhrzeit oder dem Standort des Geräts.

Dieses Dokument führt Passbook, Xamarin.iOS, mit der-API übergeben von Kit und erläutert, wie Pass auf dem Server zu implementieren.

## <a name="photokitiosplatformphotokitmd"></a>[PhotoKit](~/ios/platform/photokit.md)

Foto-Kit ist ein neues Framework, das ermöglicht es Anwendungen, die systembilderbibliothek Abfragen und die Erstellung benutzerdefinierter Benutzeroberflächen zum Anzeigen und ändern seinen Inhalt. Sie enthält eine Reihe von Klassen, die Images und Videoassets sowie Sammlungen von Ressourcen wie z. B. Alben und Ordner darstellen.

## <a name="request-app-reviewiosplatformrequest-app-reviewmd"></a>[App Review anfordern](~/ios/platform/request-app-review.md)

Noch nicht mit iOS 10.3, die `RequestReview()` Methode ermöglicht eine iOS-app, um den Benutzer zu bewerten, oder Überprüfen sie. Wenn diese Methode in einer Protokollversand-app aufgerufen wird, die der Benutzer aus dem App Store installiert wurde, wird mit iOS 10 behandelt die gesamte Bewertung und Überprüfungsprozess für den Entwickler. Da dieser Prozess mit App-Store-Richtlinie gesteuert wird, wird eine Warnung kann oder möglicherweise nicht angezeigt.

## <a name="search-apisiosplatformsearchindexmd"></a>[Such-APIs](~/ios/platform/search/index.md)

Suche wurde erweitert, unter iOS 9, um hervorragende neue Möglichkeiten zum Zugriff auf Informationen und Funktionen in einer Xamarin.iOS-app bereitzustellen. Verwenden den neuen App-Suche-APIs, app-Inhalte durch Spotlight und Safari Suchergebnissen Übergabeanimationen und Siri-Erinnerungen und Vorschläge durchsuchbaren erfolgt. Dadurch können Benutzer schnell die Aktivitäten und Informationen, die Tiefe in Ihrer app zugreifen.

## <a name="sirikitiosplatformsirikitindexmd"></a>[SiriKit](~/ios/platform/sirikit/index.md)

Neue iOS 10, SiriKit kann eine iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät mithilfe von App-Erweiterungen und neuen zugänglich sind **Intents** und **Intents UI** Frameworks.

## <a name="social-frameworkiosplatformsocial-frameworkmd"></a>[Soziale framework](~/ios/platform/social-framework.md)

Das soziale Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich _Twitter_ und _Facebook_, als auch _SinaWeibo_ für Benutzer in China.

## <a name="speech-recognitioniosplatformspeechmd"></a>[Spracherkennung](~/ios/platform/speech.md)

iOS 10 bietet es sich um eine neue Spracherkennungs-API, die die app fortlaufende Spracherkennung unterstützen und transkribieren Speech (von Livedaten oder aufgezeichnete Audiodatenströme) in Text ermöglicht.

## <a name="textkitiosplatformtextkitmd"></a>[TextKit](~/ios/platform/textkit.md)

Text-Kit ist eine neue API, die leistungsstarke Text Layout- und Renderingaufgaben Features bietet. Es basiert auf niedriger Ebene Text des Core-Framework und ist viel einfacher zu verwenden als Core Text.

## <a name="3d-touchiosplatform3d-touchmd"></a>[3D Touch](~/ios/platform/3d-touch.md)

In diesem Artikel bieten und Einführung in die Verwendung der neuen 3D Touch-APIs zum Druck vertrauliche Gesten Ihrer Xamarin.iOS-apps hinzufügen, die auf dem neuen iPhone 6s und iPhone 6 s ausgeführt werden sowie Geräte.

## <a name="touch-idiosplatformtouchidmd"></a>[Touch-ID](~/ios/platform/touchid.md)

Touch ID wurde in iOS 7 als Mittel zum Authentifizieren des Benutzers, um eine Kennung ähnlichen eingeführt. Es war jedoch auf die entsperrung des Geräts, verwenden die App-Store, mithilfe von iTunes und iCloud-Schlüsselbund nur Authentifizierung beschränkt.

## <a name="user-notificationsiosplatformuser-notificationsindexmd"></a>[Benutzerbenachrichtigungen](~/ios/platform/user-notifications/index.md)

Noch nicht mit iOS 10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Durch die Verwendung dieses Frameworks kann der app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen eine Reihe von Bedingungen wie z. B. Speicherort oder Uhrzeit angeben.

## <a name="wide-coloriosplatformwide-colormd"></a>[Breite Farbskala](~/ios/platform/wide-color.md)

iOS 10 und MacOS Sierra verbessert die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System einschließlich Frameworks wie Core Graphics "," Core-Image ","-Metal-Computern "und" AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

## <a name="binding-objective-cbinding-objective-cindexmd"></a>[Binden von Objective-C](binding-objective-c/index.md)

Bei der Arbeit an iOS können Fällen auftreten, in dem Sie ein Objective-C-Bibliotheken von Drittanbietern nutzen möchten. In diesen Fällen können Sie zum Erstellen einer C#-Bindung, die nativen Objective-C-Bibliotheken MonoTouchs-Bindungsprojekte verwenden. Das Projekt verwendet die gleichen Tools, die wir verwenden, um die iOS-APIs zu bringen, C#. Dieses Dokument beschreibt das Binden von Objective-C-APIs.

## <a name="referencing-native-librariesnative-interopmd"></a>[Verweisen auf systemeigene Bibliotheken](native-interop.md)

Xamarin.iOS unterstützt das Verknüpfen mit nativen C++-Bibliotheken und Objective-C-Bibliotheken. Dieses Dokument erläutert, wie Sie Ihre systemeigenen C-Bibliotheken mit Ihrem Xamarin.iOS-Projekt verknüpfen.

## <a name="embedded-frameworksembedded-frameworksmd"></a>[Eingebettete Frameworks](embedded-frameworks.md)

Erläutert, wie Objective-C-Benutzer-Frameworks in Xamarin.iOS-apps einbetten.
