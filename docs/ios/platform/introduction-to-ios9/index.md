---
title: Einführung in iOS 9
description: In diesem Artikel werden alle neuen und geänderten-APIs und verfügbare Funktionen auf iOS 9 für Entwickler von Xamarin.iOS eingeführt.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 10ed9154b92e6f13dd71f83cf4fed47585dc795f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781271"
---
# <a name="introduction-to-ios-9"></a>Einführung in iOS 9

_In diesem Artikel werden alle neuen und geänderten-APIs und verfügbare Funktionen auf iOS 9 für Entwickler von Xamarin.iOS eingeführt._

![](images/ios9-sml.png "Das iOS 9-logo")

Apple hat mehrere neue APIs und Dienste in iOS 9 sowie zahlreiche Verbesserungen an vorhandenen Funktionen hinzugefügt.

## <a name="3d-touch"></a>3D Touch

Neue iOS 9 und dem iPhone 6 s und iPhone 6 s Plus, fügt 3D Touch vertrauliche Gesten Druck auf Ihre iOS-apps. Mit 3D kann Touch iPhone-app jetzt nicht nur sagen, dass der Benutzer des Geräts Bildschirm berührt es auch Sinn, wie viel Druck, die der Benutzer selbst ist und reagieren auf die verschiedenen arbeitsspeicherauslastung.

3D Touch bietet die folgenden Funktionen zur app:

- **Druck Empfindlichkeit** - Apps können jetzt wie starke messen oder berührt Licht der Benutzer den Bildschirm, und ergreifen Sie Vorteil dieser Daten. Beispielsweise kann eine zeichnen-app eine Linie dicker oder flacher basierend auf der Festplatte wie der Benutzer den Bildschirm berührt ausführen.
- **Peek and Pop** -Ihrer app können nun den Benutzer seine Daten interagieren, ohne außerhalb des aktuellen Kontexts navigieren kann. Durch Drücken von Festplatte auf dem Bildschirm, können sie *einsehen* mit dem Element, das sie interessiert (z. B. das Ausführen einer Vorschau für eine Nachricht). Durch Drücken von schwieriger, sie können *Pop* in das Element.
- **Schnelle Aktionen** -vorstellen von Schnellaktionen wie die Kontextmenüs, die per pop ausgelesen von Volltextkatalogen können sein, wenn ein Benutzer in einer desktop-app auf ein Element klickt. Verwenden von Schnellaktionen, können Sie allgemeine, schnell und einfach zu Verknüpfungen der Zugriff auf Funktionen in Ihrer app über das Symbol des Home-Bildschirm auf dem iOS-Gerät hinzufügen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Handbuch.

## <a name="app-transport-security"></a>App-Transportsicherheit

Neu für iOS 9, App-Transport-Sicherheit (ATS) erzwingt die sichere Verbindungen zwischen Internet-Ressourcen (z. B. die app-Back-End-Server) und Ihre app. ATS wird sichergestellt, dass die gesamte Internetkommunikation entsprechen, um die Verbindung secure bewährte Methoden um versehentlichen Offenlegung vertraulicher Informationen entweder direkt über Ihre app oder eine Bibliothek, die es benötigt, ist das verhindern.

Da ATS in apps für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit erstellt standardmäßig aktiviert ist. [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) oder [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) unterliegen werden ATS sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehl.

Um weitere Informationen zu ATS zu suchen, finden Sie unter unsere [App Transportsicherheit](~/ios/app-fundamentals/ats.md) Handbuch.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Multitasking für iPad

Mit iOS 9 verfügt über Apple Multitasking-Unterstützung für die Ausführung von zwei Web-apps zur gleichen Zeit auf bestimmte iPad Hardware hinzugefügt. Daher können Ihre apps Xamarin.iOS nicht mehr davon ausgehen, dass sie die Apps einem bestimmten Zeitpunkt ausgeführt werden oder dass sie Zugriff auf den gesamten Bildschirm oder Ressourcen des Geräts.

Multitasking für iPad wird über die folgenden Funktionen unterstützt:

- **Folie über** -ermöglicht es dem Benutzer eine zweite iOS-app vorübergehend in eine Folie out Bereich (entweder auf der linken oder rechten Seite des Bildschirms basierend auf Sprache Richtung) ausgeführt wird, die etwa 25 % der Haupt-app derzeit ausgeführt abdeckt. Schieben Sie über steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4.
- **Geteilte Ansicht** – der Benutzer kann auf unterstützten iPad Hardware (iPad iPad Mini-4 und iPad Pro nur per Funk 2) Wählen Sie eine zweite-app und führen Sie es Seite-an-Seite mit der aktuell ausgeführten app in einem geteilten Bildschirmmodus. Der Benutzer kann den Anteil der Hauptbildschirm steuern, die jede app belegt.
- **Bild in Abbildung** – für apps, die Wiedergabe Videoinhalten, das video kann nun in einem Fenster verschiebbares und in der Größe veränderbaren abgespielt werden, die über die andere apps, die derzeit auf dem iOS-Gerät ausgeführt: verschiebt. Der Benutzer hat die vollständige Kontrolle über die Größe und Position des Fensters. Bild im Bild sind nur ein iPhone Pro iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3 oder iPad Mini-4 verfügbar.

Um weitere Informationen über die neuen Funktionen von Multitasking von iOS 9 zu erhalten, finden Sie unter unsere [Multitasking für iPad](~/ios/platform/multitasking.md) Handbuch.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Neuer Kontakte und Kontakte Benutzeroberflächen-Frameworks

Mit der Einführung von iOS 9, Apple hat zwei neue Frameworks, veröffentlicht [Kontakte](https://developer.xamarin.com/api/namespace/Contacts/) und [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), die dem vorhandenen Adressbuch ersetzen und Adressbuch Benutzeroberflächen-Frameworks von iOS 8 und früher verwendeten.

Diese neue, eines objektorientierten Frameworks bieten Folgendes:

- **Kontakte** – Xamarin.iOS bietet Zugriff auf die Kontaktinformationen des Benutzers. Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für den Thread sicheren, nur-Lese Zugriff optimiert.
- **ContactsUI** – Xamarin.iOS Benutzeroberflächenelemente bietet anzeigen, bearbeiten, wählen und Kontakte auf iOS-Geräte erstellen.

Weitere Informationen finden Sie in unserer [Kontakte und Kontakte UI](~/ios/platform/contacts.md) Dokumentation.


## <a name="new-search-apis"></a>Neue Such-APIs

Suche wurde in iOS 9 hervorragende neue Möglichkeiten, um den Zugriff auf Informationen innerhalb der app Xamarin.iOS bereitstellen erweitert. Verwenden die neue Such-APIs, können Sie den Inhalt Ihrer app über die Spotlight und Safari Suchergebnissen Übergabe und Siri Erinnerungen und Vorschläge durchsuchbaren vornehmen. Dadurch werden Benutzer schnellen Zugriff auf Aktivitäten und Informationen, die umfassende innerhalb Ihrer app.

Darüber hinaus erleichtern die neue Such-APIs Suche in Ihrer app ohne die vorherige Implementierung Suchabfrage zu integrieren. Aus diesem Grund Ansprüche Apple an, dass es sich normalerweise um ein paar Stunden zu einer iOS 9-app-Inhalte universell mithilfe der App suchen zu lassen dauert.

Weitere Informationen finden Sie unter unsere [Suche Erweiterungen](~/ios/platform/search/index.md) Dokumentation.

## <a name="new-stack-view"></a>Neue Stapelansicht

Der Stapel Ansichtssteuerelement ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) nutzt die Funktionen von Automatisches Layout und Klassen Größe einen Stapel mit Unteransichten (horizontal oder vertikal) zu verwalten, die dynamisch auf dem iOS-Gerät Ausrichtung und der Bildschirmgröße Größe reagiert.

Mithilfe der Stapel Ansichtssteuerelement erforderlich, der Arbeitsaufwand, Layout, das eine Benutzeroberfläche stark reduziert wird. Das Layout der alle Unteransichten an eine Stapelansicht angefügt werden automatisch auf der Grundlage Entwickler definierten Eigenschaften wie z. B. Achse "," Verteilung "," Ausrichtung "und" Abstand verwaltet.

Weitere Informationen finden Sie unter unsere [Einführung in die Stapelansicht](~/ios/user-interface/controls/uistackview.md) Dokumentation.


## <a name="collection-view-changes"></a>Sammlungsänderungen-Sicht

In iOS 9, die Auflistungsansicht ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) unterstützt das Ziehen Sie jetzt neuanordnung von Elementen ausgegeben, durch Hinzufügen einer neuen Standard Geste Erkennung und mehrere neue unterstützender Methoden.

Verwenden diese neuen Methoden, können Sie problemlos Implementieren von Drag-zu-anordnen in der Auflistungsansicht und haben die Möglichkeit zum Anpassen der Darstellung der Elemente jeder Phase des Prozesses neu anordnen.

Um weitere Informationen zu den Änderungen Auflistungsansicht für iOS 9 zu erhalten, finden Sie unter unsere [Ansicht Sammlungsänderungen](~/ios/user-interface/controls/uicollectionview.md) Handbuch.

## <a name="game-enhancements"></a>Spiel Erweiterungen

Mit iOS 9 machte Apple mehrere technologische Verbesserungen an die Gaming-APIs, die zur Implementierung von Spiel Grafiken und Audio in Ihrer app Xamarin.iOS vereinfachen. Dazu gehören sowohl einfache Entwicklung über allgemeine Frameworks und nutzen die Leistungsfähigkeit von dem iOS-Gerät GPU für verbesserte Geschwindigkeit und Grafik Funktionen im Hinblick auf niedriger Ebene optimiert.

Dies schließt GameplayKit, ReplayKit, Modell-e/a, MetalKit und Metall Leistung Shader zusammen mit der neuen, verbesserten Funktionen von Metall, SceneKit und SpriteKit.

Weitere Informationen finden Sie unter unsere [Spiel Verbesserungen](~/ios/platform/gaming/index.md) Dokumentation.

## <a name="homekit-framework-changes"></a>HomeKit Framework ändert.

Die [HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) Framework, eingeführt in iOS 8, bietet die Möglichkeit zum Einrichten und steuern verschiedene HomeKit aktiviert Zubehör (z. B. automatisierte Leuchten, Türschlösser und Garage Tür Dosenöffner) aus einem Xamarin.iOS-app. Einfache Einrichtung und Konfiguration, sondern können HomeKit Zubehör über gesprochene Siri Befehle gesteuert werden.

In iOS 9 Apple erleichtert das Setup, erweitert die Typen von Zubehör unterstützt und weitere Zubehörs Aktivitäten (z. B. die Steuerung der Zubehör Remote über die iCloud) bereitgestellt.

Weitere Informationen finden Sie unter unsere [Einführung in HomeKit](~/ios/platform/homekit.md), [HomeKitIntro iOS-Beispielanwendung](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) und Apple [HomeKit](https://developer.apple.com/homekit/) Dokumentation.

## <a name="handoff-framework-changes"></a>Übergabe Framework-Änderungen

Übergabe (auch bekannt als Geschäftskontinuität) wurde von Apple in iOS 8 und OS X Yosemite (10.10) als eine Möglichkeit für den Benutzer zu eine Aktivität eines ihrer Geräte (IOS- oder Mac) beginnen und den Vorgang fortzusetzen, derselben Aktivität auf einem anderen Gerät (wie durch den Benutzer iClou identifiziert eingeführt. d Konto).

Übergabe wurde in den iOS 9 auch neue, unterstützt verbesserte Suchfunktionen erweitert. Weitere Informationen finden Sie unter unsere [Suche Erweiterungen](~/ios/platform/search/index.md) Dokumentation. Weitere Informationen finden Sie unter Übergabe, finden Sie unter unsere [Einführung in die Übergabe](~/ios/platform/handoff.md) Dokumentation.

## <a name="new-extension-points"></a>Neue Erweiterungspunkte

Apple iOS 8 eingeführt Erweiterungen – Bibliotheken, die vom Betriebssystem in standard Kontexten wie z. B. in der Mitteilungszentrale dargestellt werden, wenn der Benutzer die Tastatur anfordert oder wenn sie ein Foto bearbeiten.

Mit iOS 9, Apple Unterstützung durch die Bereitstellung mehrere neue Erweiterung _Erweiterungspunkte_ , definieren Sie Nutzungsrichtlinien und APIs bereit, die für die Arbeit in einem angegebenen Bereich wie folgt:

- **Neue Audio Einheit Erweiterungspunkt** – verwenden Sie diese Erweiterungspunkt, um audio Effekte, Musik Instrumente, sound Generatoren usw. für die Verwendung in anderen Audio Einheit Host-apps (z. B. GarageBand) bereitzustellen. Diese Erweiterungspunkt können auch die verkaufen _Audioeinheiten_ (audio-Plug-ins) auf dem App Store.
- **Neuen Index Wartung Erweiterungspunkt** – verwenden Sie diese Erweiterungspunkt zur Unterstützung von Neuindizieren des app-Daten, ohne dass eine app nächsten Starten.
- **Neue Netzwerk-Erweiterungspunkte** (erfordern spezielle Berechtigung von Apple):
    - **Anbieter-Proxy-App-Erweiterung** – diese Erweiterungspunkt verwenden, um eine benutzerdefinierte, transparent die clientseitige Netzwerkproxy zu implementieren.
    - **Datenanbieter Filter / Filter Anbieter Steuerelementerweiterung** -verwenden Sie diese Erweiterungspunkte, um dynamische Netzwerkinhalte, die Filterung auf dem Gerät implementieren.
    - **Paket Tunnel-Anbietererweiterung** – diese Erweiterungspunkt verwenden, um eine benutzerdefinierte VPN-Tunneling-Protokoll die clientseitige zu implementieren.
- **Neue Safari Erweiterungspunkte**:
    - **Inhalts-Erweiterung blockieren** – diese Erweiterungspunkt verwenden, um eine Liste der blockierten Inhalt definieren, die nicht angezeigt wird, wenn der Benutzer den Webbrowser ist.
    - **Freigegebene Links Erweiterung** – diese Erweiterungspunkt verwenden, um den Inhalt Ihrer app in Safari des freigegebenen Links anzeigen zu aktivieren.

Weitere Informationen finden Sie unter unsere [Einführung in die Erweiterungen](~/ios/platform/extensions.md) und Apple [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) Dokumentation.

## <a name="keychain-enhancements"></a>Schlüsselbund-Erweiterungen

In iOS 9 wurde Apple Schlüsselbund zum Bereitstellen von eines neuen Schlüssel Verschlüsselungstyp für die sichere Enclave und weitere Artikel Schutzoptionen wie folgt erweitert:

- Eine neue Touch ID-Einschränkung, die Schlüsselsammlung Elemente werden ungültig, wenn die Fingerabdruckdatenbank geändert wird.
- Neue Einschränkungen, die es ermöglichen, nur die Einträge für Access Control List mit Touch ID oder die Kennung erstellt.
- Eine neue authentifizierungskontext, der Ihnen ermöglicht, Authentifizierung, die getrennt von Aufrufen `SecItem` aufrufen.
- Zugriff auf die Steuerelementliste Entropie (mit der Option Anwendungskennwort) für die bereitgestellte app Schlüsselbund Element Verschlüsselung.
- Unterstützung für das Generieren und Verwenden von Schlüsseln in der sicheren Enclave (über die `kSecAttrTokenIDSecureEnclave` Attribut).

Weitere Informationen finden Sie unter unsere [Einführung in Touch ID](~/ios/platform/touchid.md) Dokumentation.


## <a name="right-to-left-language-support"></a>Unterstützung für rechts-nach-links-Sprachen

IOS 9 machte vorlegen gekippte Benutzeroberfläche einfacher als jemals durch die vollständige Unterstützung für rechts-nach-links-Sprachen Apple. Hierzu gehören folgende Elemente:

- Standard [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) Steuerelemente rechts-nach-links-basierend auf den iOS-Geräte Gebietsschema- und sprachunterstützung Einstellungen automatisch gekippt.
- Die [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) Klasse enthält Attribute, mit denen Sie definieren, wie eine bestimmte Ansicht angezeigt werden soll, gekippt rechts-nach-links.
- Die Möglichkeit, ein Bild programmgesteuert mithilfe von spiegeln die [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) Eigenschaft von der [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) Klasse.

Weitere Informationen finden Sie in der Apple- [Unterstützung von rechts nach links geschriebene Sprachen](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) Dokumentation.



## <a name="additional-framework-changes"></a>Zusätzliche Framework-Änderungen

Zusätzlich zu den wichtigen Änderungen, denen wir über behandelt wurden, machte Apple Änderungen und Verbesserungen für mehrere vorhandene Frameworks für iOS 9, u. a. folgende:

- AV-Foundation-Framework
- AVKit-Framework
- CloudKit Framework
- Foundation-Framework
- Handoff Framework
- HealthKit Framework
- HomeKit Framework
- Lokale Authentifizierung-Framework
- MapKit Framework
- PassKit Framework
- Safari Services Framework
- UIKit Framework

Weitere Informationen finden Sie unter unsere [zusätzliche iOS 9-Framework-Änderungen](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) Dokumentation.

## <a name="deprecated-apis-and-functions"></a>Nicht mehr unterstützte APIs und -Funktionen

Apple hat die folgenden APIs und Funktionen in iOS 9 veraltet:

- **Adresse Buch & Adressbuch UI** -diese APIs wurden durch die Kontakt- und wenden Sie sich an UI-Frameworks ersetzt. Weitere Informationen finden Sie in unserer [Kontakte und Kontakte UI](~/ios/platform/contacts.md) Dokumentation.
- **CBCentralManager** – der `RetrievePeripherals` und `RetrieveConnectedPeripherals` Methoden die `CBCentralManager` Klasse in iOS 9 entfernt wurden. Diese Methoden aufrufen führt dazu, dass eine app zum Absturz beim Verbindungsaufbau von Zubehör oder app-Start.
- **FetchAllChanges** – der `FetchAllChanges` von der `CKFetchRecordChangesOperation` Klasse wurde jedoch veraltet und wird in iOS 9 entfernt.
- **Media Player** -der Media-Player Framework in iOS 9 ist veraltet. Verwenden Sie stattdessen AVKit oder AV Foundation-APIs.

Eine vollständige Liste der spezifischen API veralterungen, finden Sie in der Apple [iOS 9.0 API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) Dokumentation.

## <a name="ios-9-sample-apps"></a>Beispiel-Apps für iOS 9

Wir haben einige [iOS 9-spezifische Beispiele](https://developer.xamarin.com/samples/ios/iOS9/) für den Einstieg:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Außerdem sehen Sie sich die iOS-Teile dieser Beispiele (Companion Mac OS X-Versionen stammen!):

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Einführung in 3D Touch](~/ios/platform/3d-touch.md)
- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Multitasking für iPad](~/ios/platform/multitasking.md)
- [Kontakte und Kontakte UI](~/ios/platform/contacts.md)
- [Neue Such-APIs](~/ios/platform/search/index.md)
- [Einführung in die Ansicht des Stapels](~/ios/user-interface/controls/uistackview.md)
- [Sammlungsänderungen-Sicht](~/ios/user-interface/controls/uicollectionview.md)
- [Gaming-Erweiterungen](~/ios/platform/gaming/index.md)
- [Einführung in HomeKit](~/ios/platform/homekit.md)
- [Einführung in die Übergabe](~/ios/platform/handoff.md)
- [Weitere iOS 9-Frameworkänderungen](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Problembehandlung](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualisieren Ihre apps Xamarin.iOS auf iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
