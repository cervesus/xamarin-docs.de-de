---
title: Übersicht über IOS-Platt Form Features
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, in denen die in verschiedenen Versionen von IOS eingeführten Features und andere IOS-Platt Form Features beschrieben werden.
ms.prod: xamarin
ms.assetid: 9F6A27E5-8A87-ADE2-D1EF-5684E7B8C999
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: bcf293b29d6ddca10ab60ae061491b60f1e30520
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77495637"
---
# <a name="ios-platform-features-overview"></a>Übersicht über IOS-Platt Form Features

Auf dieser Seite werden die aktuellen IOS-Releases aufgeführt und einige der Apple-Frameworks hervorgehoben, auf die Sie mit xamarin. IOS zugreifen können.

## <a name="ios-releases"></a>IOS-Releases

|  |  |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Einführung in ios 13](~/ios/platform/ios13/index.md) | In diesem Dokument wird xamarin. IOS 13 beschrieben.|
| [Einführung in iOS 12](~/ios/platform/introduction-to-ios12/index.md) | In diesem Dokument werden die IOS 12-Funktionen beschrieben, die zum Entwickeln von xamarin. IOS-Anwendungen verfügbar sind.|
| [Einführung in iOS 11](~/ios/platform/introduction-to-ios11/index.md) | In diesem Dokument werden die neuen und aktualisierten Features in ios 11 und Xcode 9 beschrieben, z. b. Arkit, Core ml, Core NFC, Drag & Drop, MapKit, PDFKit, Sirikit und Vision. Es enthält Links zu Anleitungen, in denen beschrieben wird, wie diese Funktionen mit xamarin. IOS verwendet werden. |
| [Einführung in iOS 10](~/ios/platform/introduction-to-ios10/index.md) | IOS 10 umfasst mehrere neue APIs und Dienste, mit denen Sie Apps mit neuen Features und Funktionen entwickeln können. IOS 10 verfügt über neue Funktionen, wie z. b. das Erweitern von Karten, Nachrichten, Telefon und Siri. In diesem Abschnitt wird erläutert, wie Sie diese Features in einer xamarin. IOS-App nutzen können. |
| [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)   | In diesem Abschnitt werden die in ios 9 vorgenommenen Änderungen beim Upgrade von IOS 8 und die Verwendung dieser Features in einer xamarin. IOS-App definiert. |
| [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)         | IOS 8 hat eine große Anzahl von Änderungen am Betriebssystem von IOS 7 vorgenommen. Hier zeigen wir Ihnen, was Sie sind und wie Sie verwendet werden. |
| [Einführung in iOS 7](~/ios/platform/introduction-to-ios7/index.md)   | Informationen zu den wichtigsten neuen APIs, die in ios 7 eingeführt wurden, u.a. zum Anzeigen von Controller Übergängen, Erweiterungen von UIView-Animationen, UIKit Dynamics und textkit. |
| [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)   | Erläuterungen zu den in ios 6 eingeführten Features, einschließlich Auflistungs Ansichten, Pass-Kit, ereigniskit und das Social Framework. |

## <a name="apple-pay"></a>[Apple Pay](~/ios/platform/apple-pay.md)

Apple Pay wurde zusammen mit IOS 8 eingeführt, sodass Benutzer physische waren wie Nahrungsmittel, Unterhaltung und Mitgliedschaften über Ihre IOS-Geräte bezahlen können. Es ist auf iPhone 6 und iPhone 6 Plus verfügbar und kann auch mit dem Apple Watch für Einkäufe im Store gekoppelt werden. Bei Verwendung auf einem iPhone wird die Berührungs-ID verwendet, um Transaktionen für die Kredit-oder Debitkarte eines Benutzers zu bestätigen und zu autorisieren.

## <a name="callkit"></a>[CallKit](~/ios/platform/callkit.md)

Die neue callkit-API in ios 10 bietet eine Möglichkeit für VoIP-Apps, mit der iPhone-Benutzeroberfläche zu integrieren und eine vertraute Schnittstelle und Benutzeroberfläche für den Endbenutzer bereitzustellen. Mit dieser API können Benutzer VoIP-Anrufe über den Sperrbildschirm des IOS-Geräts anzeigen und mit ihnen interagieren und Kontakte mithilfe der **Favoriten** **-und Wiederholungs** Ansichten der Phone-App verwalten.

## <a name="contacts-and-contactsui"></a>[Kontakte und ContactsUI](~/ios/platform/contacts.md)

Mit der Einführung von IOS 9 hat Apple zwei neue Frameworks, `Contacts` und `ContactsUI`freigegeben, die das vorhandene Adressbuch und die Benutzeroberflächen-Frameworks der Adressbücher ersetzen, die von IOS 8 und früher verwendet werden.

## <a name="document-picker"></a>[Dokumentauswahl](~/ios/platform/document-picker.md)

Die Dokument Auswahl ermöglicht die gemeinsame Nutzung von Dokumenten zwischen apps. Diese Dokumente können in icloud oder in einem anderen App-Verzeichnis gespeichert werden. Dokumente werden über den Satz von [Dokument Anbieter Erweiterungen](~/ios/platform/extensions.md) freigegeben, die der Benutzer auf seinem Gerät installiert hat.

## <a name="eventkit"></a>[EventKit](~/ios/platform/eventkit.md)

IOS verfügt über zwei integrierte Kalender bezogene Anwendungen: die Kalenderanwendung und die Erinnerungs Anwendung. Es ist einfach genug, um zu verstehen, wie Kalenderdaten von der Kalenderanwendung verwaltet werden, aber die Erinnerungs Anwendung ist weniger offensichtlich. Erinnerungen können tatsächlich Datumsangaben zugeordnet werden, wenn Sie fällig sind, wenn Sie abgeschlossen sind usw. Daher speichert IOS alle Kalenderdaten, unabhängig davon, ob es sich um Kalenderereignisse oder Erinnerungen handelt, an einem Ort, der als *Calendar Database*bezeichnet wird.

## <a name="ios-extensions"></a>[IOS-Erweiterungen](~/ios/platform/extensions.md)

Erweiterungen, die in ios 8 eingeführt werden, sind spezialisierte `UIViewControllers`, die von IOS in Standard Kontexten wie z. b. innerhalb des **Benachrichtigungs Centers**präsentiert werden, als benutzerdefinierte Tastaturtypen, die vom Benutzer angefordert werden, um spezialisierte Eingaben oder andere Kontexte wie das Bearbeiten eines Fotos auszuführen, bei dem die Erweiterung spezielle Effektfilter bereitstellen kann.

## <a name="graphics-and-animation-in-ios"></a>[Grafiken und Animationen in ios](~/ios/platform/graphics-animation-ios/index.md)

Grafiken und Animationen in ios decken grundlegende Grafik Konzepte in ios ab, wie z. b. CoreImage, Core-Grafiken und Core-Animationen.

## <a name="handoff"></a>[Handoff](~/ios/platform/handoff.md)

Apple hat die Übergabe in ios 8 und OS X Yosemite (10,10) eingeführt, um dem Benutzer einen allgemeinen Mechanismus zum Übertragen von Aktivitäten zu ermöglichen, die auf einem seiner Geräte gestartet werden, auf ein anderes Gerät, auf dem dieselbe APP oder eine andere app ausgeführt wird, die dieselbe Aktivität unterstützt.

## <a name="healthkit"></a>[HealthKit](~/ios/platform/healthkit.md)

Health Kit bietet einen sicheren Datenspeicher für die Integritäts bezogenen Informationen des Benutzers. Health Kit-Apps können mit der expliziten Berechtigung des Benutzers Lese-und Schreibvorgänge für diesen Datenspeicher durch nehmen und Benachrichtigungen erhalten, wenn relevante Daten hinzugefügt werden. Apps können die Daten darstellen, oder Benutzer können die von Apple bereitgestellte Integritäts-App verwenden, um ein Dashboard aller Ihrer Daten anzuzeigen.

## <a name="homekit"></a>[HomeKit](~/ios/platform/homekit.md)

Apple hat homekit in ios 8 eingeführt, um ein gemeinsames Framework zum Ermitteln und kommunizieren mit Home Automation-Geräten in der Startseite eines Benutzers bereitzustellen. Homekit bietet eine gängige Plattform zum Konfigurieren von Geräten und Einrichten von Aktionen zur Steuerung.

## <a name="in-app-purchasing"></a>[In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)

IOS-Anwendungen können digitale Produkte oder Dienste mit storekit verkaufen – eine Reihe von APIs, die von IOS bereitgestellt werden, die mit Apple-Servern kommunizieren, um Finanztransaktionen mit dem Benutzer über Ihre Apple-ID auszuführen. Die storekit-APIs betreffen in erster Linie das Abrufen von Produktinformationen und das Ausführen von Transaktionen – es ist keine Benutzeroberflächen Komponente vorhanden. Anwendungen, die in-App-Käufe implementieren, müssen eine eigene Benutzeroberfläche erstellen und erworbene Elemente mit benutzerdefiniertem Code nachverfolgen, um dem Benutzer die erforderlichen Produkte oder Dienste bereitzustellen.

## <a name="ios-gaming-apis"></a>[IOS-Gaming-APIs](~/ios/platform/gaming/index.md)

Apple hat einige technologische Verbesserungen an den Gaming-APIs in ios 9 vorgenommen, die die Implementierung von Spielgrafiken und Audiodaten in einer xamarin. IOS-App vereinfachen. Diese umfassen sowohl die einfache Entwicklung durch allgemeine Frameworks als auch die Leistungsfähigkeit der GPU des IOS-Geräts, um die Geschwindigkeit und Grafikfunktionen zu verbessern.

## <a name="message-app-integration"></a>[Integration der Nachrichten-APP](~/ios/platform/message-app-integration/index.md)

Neu bei IOS 10. eine Erweiterung der Nachrichten-APP wird in die **Nachrichten** -app integriert und stellt dem Benutzer neue Funktionen zur Anwendung. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="multitasking-for-ipad"></a>[Multitasking für iPad](~/ios/platform/multitasking.md)

IOS 9 fügt eine Multitasking-Unterstützung für die gleich malige Ausführung von zwei apps auf einer bestimmten iPad-Hardware hinzu. Multitasking für iPad wird über die folgenden Funktionen unterstützt: Folie über, geteilte Ansicht & Bild im Bild.

## <a name="passkit"></a>[PassKit](~/ios/platform/passkit.md)

Das Passbook ist eine APP für iPhones und iPod Touch mit IOS 6. Sie speichert und zeigt Barcodes und andere Informationen an, um Kundentransaktionen auf Ihrem Telefon mit der "realen Welt" zu verknüpfen. Durch Führungen werden von Händlern generiert und per e-Mail, URLs oder innerhalb der eigenen IOS-App eines Händlers an den Kunden gesendet. Das Passbook speichert und organisiert alle Durchgänge auf einem Telefon und zeigt abhängig vom Datum/der Uhrzeit oder dem Speicherort des Geräts Pass Erinnerungen auf dem Sperrbildschirm an.

In diesem Dokument wird eine Passbook mithilfe der Pass-Kit-API mit xamarin. IOS eingeführt, und es wird erläutert, wie Sie Pass-Through auf Ihrem Server implementieren.

## <a name="photokit"></a>[PhotoKit](~/ios/platform/photokit.md)

Photo Kit ist ein neues Framework, das es Anwendungen ermöglicht, die System Image Bibliothek abzufragen und benutzerdefinierte Benutzeroberflächen zu erstellen, um deren Inhalte anzuzeigen und zu ändern. Sie enthält eine Reihe von Klassen, die Bild-und Video Medienobjekte sowie Sammlungen von Assets wie z. b. Alben und Ordner darstellen.

## <a name="request-app-review"></a>[App-Überprüfung anfordern](~/ios/platform/request-app-review.md)

Neu bei IOS 10,3, mit der `RequestReview()`-Methode kann eine IOS-App den Benutzer auffordern, ihn zu bewerten oder zu überprüfen. Wenn diese Methode in einer Versand-app aufgerufen wird, die der Benutzer aus dem App Store installiert hat, übernimmt IOS 10 den gesamten Bewertungs-und Überprüfungsprozess für den Entwickler. Da dieser Prozess von der App Store-Richtlinie gesteuert wird, wird möglicherweise eine Warnung angezeigt oder nicht angezeigt.

## <a name="search-apis"></a>[Such-APIs](~/ios/platform/search/index.md)

Die Suche wurde in ios 9 erweitert und bietet hervorragend neue Möglichkeiten für den Zugriff auf Informationen und Features in einer xamarin. IOS-app. Mithilfe der neuen App-Such-APIs können App-Inhalte durch Spotlight-und Safari-Suchergebnisse, Übergabe und Siri-Erinnerungen und Vorschläge durchsucht werden. Dadurch können Benutzer schnell auf Aktivitäten und Informationen innerhalb ihrer App zugreifen.

## <a name="sirikit"></a>[SiriKit](~/ios/platform/sirikit/index.md)

Mit dem neuen IOS 10 ermöglicht das Sirikit einer IOS-APP, Dienste bereitzustellen, auf die der Benutzer mithilfe von Siri und der Maps-APP auf einem IOS-Gerät mithilfe von App-Erweiterungen und den neuen **Intents** und **Intents-Benutzer** Oberflächen-Frameworks zugreifen kann.

## <a name="social-framework"></a>[Social Framework](~/ios/platform/social-framework.md)

Das Social Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich _Twitter_ und _Facebook_, sowie _sinaweibo_ für Benutzer in China.

## <a name="speech-recognition"></a>[Spracherkennung](~/ios/platform/speech.md)

IOS 10 enthält eine neue Sprach-API, die es der App ermöglicht, fortlaufende sprach Erkennungs-und transkrinungs Sprache (aus Live-oder aufgezeichneten Audiodatenströmen) in Text zu unterstützen.

## <a name="textkit"></a>[TextKit](~/ios/platform/textkit.md)

Das textkit ist eine neue API, die leistungsstarke Layout-und Renderingfeatures von Text bietet. Es baut auf dem Low-Level-Kern Text Framework auf, ist aber viel einfacher zu verwenden als Kerntext.

## <a name="3d-touch"></a>[3D Touch](~/ios/platform/3d-touch.md)

Dieser Artikel bietet eine Einführung in die Verwendung der neuen 3D-touchapis, um Ihren xamarin. IOS-apps, die auf den neuen iPhone 6S-und iPhone 6S Plus-Geräten ausgeführt werden, druckempfindliche Gesten hinzuzufügen.

## <a name="touch-id-and-face-id-with-xamarinios"></a>[Fingereingabe-ID und Face ID mit xamarin. IOS](~/ios/platform/touch-id-face-id.md)

Die Fingereingabe-ID und die Gesichts-ID sind biometrische Authentifizierungssysteme seit IOS 8. In diesem Artikel und Beispiel wird beschrieben, wie Sie die Fingereingabe-ID und die Face ID mit xamarin. IOS verwenden.

## <a name="user-notifications"></a>[Benutzer Benachrichtigungen](~/ios/platform/user-notifications/index.md)

Das Benutzer Benachrichtigungs Framework ist neu bei IOS 10 und ermöglicht die Übermittlung und Handhabung von lokalen Benachrichtigungen und Remote Benachrichtigungen. Mithilfe dieses Frameworks kann die APP-oder App-Erweiterung die Übermittlung lokaler Benachrichtigungen planen, indem Sie einen Satz von Bedingungen angibt, wie z. b. den Standort oder die Uhrzeit.

## <a name="wide-color"></a>[Breite Farbskala](~/ios/platform/wide-color.md)

IOS 10 und macOS Sierra verbessern die Unterstützung für erweiterte Pixel Formate und große Farbbereiche im gesamten System, einschließlich Frameworks wie Kern Grafiken, Core-Bild, Metal und AVFoundation. Die Unterstützung für Geräte mit breit Farbanzeige wird weiter vereinfacht, indem dieses Verhalten im gesamten Grafik Stapel bereitgestellt wird.

## <a name="binding-objective-c"></a>[Binden von Objective-C](binding-objective-c/index.md)

Bei der Arbeit mit IOS kann es vorkommen, dass Sie eine Ziel-C-Bibliothek eines Drittanbieters verwenden möchten. In diesen Fällen können Sie die Bindungs Projekte von MonoTouch verwenden, um eine C# Bindung mit den nativen Ziel-C-Bibliotheken zu erstellen. Das Projekt verwendet die gleichen Tools, mit denen die IOS-APIs verwendet werden C#. In diesem Dokument wird beschrieben, wie Sie die Ziel-C-APIs binden.

## <a name="bind-ios-swift-libraries"></a>[Binden von IOS SWIFT-Bibliotheken](binding-swift/index.md)

In diesem Dokument wird beschrieben, C# wie Bindungen für SWIFT-Code erstellt werden, sodass Native Bibliotheken und cocoapods in einer xamarin. IOS-Anwendung verwendet werden können.

## <a name="referencing-native-libraries"></a>[Verweisen auf Native Bibliotheken](native-interop.md)

Xamarin. IOS unterstützt das Verknüpfen mit nativen c-Bibliotheken und Ziel-C-Bibliotheken. In diesem Dokument wird erläutert, wie Sie Ihre nativen C-Bibliotheken mit Ihrem xamarin. IOS-Projekt verknüpfen.

## <a name="embedded-frameworks"></a>[Eingebettete Frameworks](embedded-frameworks.md)

Erläutert, wie Sie Ziel-C-Benutzer-Frameworks in xamarin. IOS-apps einbetten.
