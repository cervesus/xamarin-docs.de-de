---
title: Einführung in iOS 9
description: Dieser Artikel enthält alle neuen und geänderten APIs und Funktionen von iOS 9 für Xamarin.iOS-Entwickler.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 2f9cc72dcbe506d22c8a986bcf59ddaa6355f043
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103249"
---
# <a name="introduction-to-ios-9"></a>Einführung in iOS 9

_Dieser Artikel enthält alle neuen und geänderten APIs und Funktionen von iOS 9 für Xamarin.iOS-Entwickler._

![](images/ios9-sml.png "Das iOS 9-logo")

Apple hat verschiedene neue APIs und Dienste in iOS 9 sowie zahlreiche Verbesserungen an vorhandenen Funktionen hinzugefügt.

## <a name="3d-touch"></a>3D Touch

Noch nicht mit iOS 9 und das iPhone 6 s und iPhone 6 s Plus, 3D Touch hinzugefügt Druck vertrauliche Gesten Ihre iOS-apps. Mit 3D kann Touch, eine iPhone-app jetzt nicht nur sagen, dass der Benutzer den Bildschirm des Geräts berührt, es auch wie viel der Benutzer selbst ist Druck erkennen und reagieren auf die verschiedenen arbeitsspeicherauslastung.

3D Touch bietet die folgenden Funktionen zu Ihrer app:

- **Vertraulichkeit Druck** : Apps können jetzt wie schwer messen oder Licht der Benutzer berührt den Bildschirm "und" Take-Vorteil mit diesen Informationen. Eine app zeichnen kann z. B. eine Linie dicker oder flacher abhängig, wie sehr die Benutzer den Bildschirm berührt vornehmen.
- **Einsehen und Pop** -Ihrer-app können nun den Benutzer, die mit den Daten interagieren, ohne aus ihrem aktuellen Kontext Navigieren zu müssen. Durch Drücken auf dem Bildschirm schwierig, sie können *einsehen* mit dem Element, das sie interessieren (z. B. das Anzeigen einer Vorschau einer Nachricht). Durch Drücken von schwieriger, sie können *Pop* in das Element.
- **Schnelle Aktionen** -vorstellen von Schnellaktionen wie die Kontextmenüs, die per pop ausgelesen oben können sein, wenn ein Benutzer auf ein Element in einer desktop-app mit der rechten Maustaste. Verwenden von Schnellaktionen, können Sie allgemeine, schnell und einfach zu Verknüpfungen der Zugriff auf Funktionen in Ihrer app über das Symbol des Home-Bildschirm auf iOS-Geräten hinzufügen.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Guide.

## <a name="app-transport-security"></a>App-Transportsicherheit

Neue IOS 9, App Transport Security (ATS) erzwingt die sichere Verbindungen zwischen Ressourcen für Internet (z. B. die app Back-End-Server) und Ihrer app. ATS wird sichergestellt, dass die gesamte Internetkommunikation entsprechen, um die Verbindung secure best Practices und verhindert versehentliche Offenlegung vertraulicher Informationen, die entweder direkt über Ihre app oder eine Bibliothek, die es verwendet.

Da ATS, wird standardmäßig in apps aktiviert ist, die für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) oder [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) wird vorgenommen werden. ATS-sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehlschlagen.

Um mehr über ATS erfahren möchten, informieren Sie sich unsere [App Transport Security](~/ios/app-fundamentals/ats.md) Guide.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Multitasking für iPad

Mit iOS 9 hat Apple Multitasking-Unterstützung für die Ausführung von zwei apps gleichzeitig auf bestimmte iPad Hardware hinzugefügt. Daher können Ihrer Xamarin.iOS-apps nicht mehr davon ausgehen, dass sie die einzige app, die einem bestimmten Zeitpunkt ausgeführt werden, oder, dass sie Zugriff auf die gesamte Anzeige oder die Ressourcen des Geräts haben.

Multitasking für iPad wird über die folgenden Funktionen unterstützt:

- **Folie über** -ermöglicht dem Benutzer, die vorübergehend eine zweite iOS-app in eine Folie, Bereich (entweder auf den rechten oder linken Seite des Bildschirms basierend auf Sprache Richtung) ausgeführt, die CA. 25 % der Haupt-app ausgeführten abdeckt. Schieben Sie über steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4.
- **Geteilte Ansicht** – der Benutzer kann auf unterstützten iPad-Hardware (iPad Air 2, iPad Mini-4 und iPad nur Pro), wählen Sie eine zweite app und Seite-an-Seite mit der aktuell ausgeführten app in einem geteilten Bildschirms-Modus ausführen. Der Benutzer kann es sich um den Prozentsatz des Hauptbildschirms steuern, die jeder app belegt.
- **Bild in Abbildung** : für apps, die Wiedergabe von Videoinhalten, das video kann jetzt in einem verschiebbaren und in der Größe veränderbaren Fenster wiedergegeben werden, die über die anderen apps, die derzeit auf dem iOS-Gerät ausgeführten gleitet. Der Benutzer hat vollständige Kontrolle über die Größe und Position des Fensters. Bild in Abbildung steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4.

Um mehr über die neuen Multitasking-Funktionen von iOS 9 zu suchen, finden Sie unserem [Multitasking für iPad](~/ios/platform/multitasking.md) Guide.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Neue Kontakte und Kontakte-Benutzeroberflächen-Frameworks

Apple hat mit der Einführung von iOS 9 sind zwei neue Frameworks vorgestellt, [Kontakte](https://developer.xamarin.com/api/namespace/Contacts/) und [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), das Ersetzen der vorhandenen Adressbuch und Adressbuch Benutzeroberflächen-Frameworks von iOS 8 und früher verwendeten.

Diese neue, objektorientierte Frameworks bieten Folgendes:

- **Kontakte** – Xamarin.iOS bietet Zugriff auf die Kontaktinformationen des Benutzers. Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für den Thread sicher, schreibgeschützten Zugriff optimiert.
- **ContactsUI** – bietet der Xamarin.iOS-UI-Elemente angezeigt werden, bearbeiten, wählen und Kontakte auf iOS-Geräten erstellen.

Weitere Informationen finden Sie unserem [Kontakte und Kontakte UI](~/ios/platform/contacts.md) Dokumentation.


## <a name="new-search-apis"></a>Neue Suche-APIs

Suche wurde erweitert, unter iOS 9, um hervorragende neue Möglichkeiten, den Zugriff auf Informationen innerhalb Ihrer Xamarin.iOS-app bereitzustellen. Mithilfe der neuen Suche-APIs können Sie den Inhalt Ihrer app durchsuchbare durch Spotlight und Safari Suchergebnissen Übergabeanimationen und Siri-Erinnerungen und Vorschläge machen. Dadurch können Benutzer die schnellen Zugriff in Ihrer app detaillierte Informationen zu Aktivitäten.

Darüber hinaus erleichtern den neuen Such-APIs auf die Suche in Ihrer app ohne die vorherige Implementierung Suchfunktionen integrieren. Aus diesem Grund Ansprüche Apple an, dass in der Regel ein paar Stunden dauert nach einer iOS 9-app-Inhalte global gesucht werden mithilfe von App-Suche.

Weitere Informationen finden Sie unserem [Verbesserungen bei der Suche](~/ios/platform/search/index.md) Dokumentation.

## <a name="new-stack-view"></a>Neue Stapelansicht

Die Stack-Steuerelement ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) nutzt die Leistungsfähigkeit von Automatisches Layout und Größenklassen einen Stapel von Unteransichten (entweder horizontal oder vertikal) zu verwalten, die auf dem iOS-Gerät-Ausrichtung und der Bildschirmgröße Größe dynamisch reagiert.

Mit Stapelansicht Steuerelement erforderlich, der Umfang der Arbeit zu Layout, die eine Benutzeroberfläche erheblich reduziert wird. Das Layout der alle Unteransichten, die an eine Stapelansicht angefügt werden automatisch basierend auf definierten Developer-Eigenschaften, z. B. Achse "," Verteilungspunkt "," Ausrichtung "und" Abstand verwaltet.

Weitere Informationen finden Sie unserem [Einführung in die Stapelansicht](~/ios/user-interface/controls/uistackview.md) Dokumentation.


## <a name="collection-view-changes"></a>Änderungen der Auflistung anzeigen

In iOS 9, die Auflistungsansicht ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) unterstützt das Ziehen Sie jetzt neuanordnung von Elementen, die standardmäßig durch Hinzufügen einer neuen Standard stiftbewegungs-Erkennung und mehrere neue unterstützender Methoden.

Verwenden diese neuen Methoden, können Sie einfache Implementierung von Drag-zu-neuanordnung in der Ansicht der Auflistung und haben die Möglichkeit zum Anpassen der Darstellung der Elemente jeder Phase des Prozesses Neuordnung von Elementen.

Um weitere Informationen zu den Änderungen der Auflistungsansicht für iOS 9 zu suchen, finden Sie unsere [Ansicht Sammlungsänderungen](~/ios/user-interface/controls/uicollectionview.md) Handbuch.

## <a name="game-enhancements"></a>Spiele-Verbesserungen

Mit iOS 9 hat Apple mehrere technologische Verbesserungen auf die Gaming-APIs vorgenommen, die Grafiken und Audio in Ihrer Xamarin.iOS-app implementieren zu vereinfachen. Dazu gehören sowohl einfache Entwicklung über die allgemeine Frameworks und der leitungsfähigkeit von dem iOS-Gerät GPU für verbesserte Geschwindigkeit und Grafik-Fähigkeiten im Hinblick auf niedriger Ebene.

Dies schließt GameplayKit "," ReplayKit "," Model e/a-"," MetalKit "und"-Metal-Leistung Shader zusammen mit neuen, erweiterten Funktionen von Metall, SceneKit und SpriteKit.

Weitere Informationen finden Sie unserem [Spiel Verbesserungen](~/ios/platform/gaming/index.md) Dokumentation.

## <a name="homekit-framework-changes"></a>HomeKit-Frameworkänderungen

Die [HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) , iOS 8 eingeführtes Framework bietet die Möglichkeit zum Einrichten und verschiedene aktiviert HomeKit-Zubehör (z.B. automatisierte Beleuchtung, Türschlösser und Türöffnern für Garagen) aus einer Xamarin.iOS-app zu steuern. Einfache Einrichtung und Konfiguration, sondern können HomeKit-Zubehör gesprochenen Siri-Befehle gesteuert werden.

In iOS 9 hat Apple vereinfacht das Setup, erweitert die Typen von Zubehör unterstützt und weitere Zubehör Interaktionen (z. B. das Steuern von Zubehör Remote über die iCloud) bereitgestellt.

Weitere Informationen finden Sie unserem [Einführung in HomeKit](~/ios/platform/homekit.md), [HomeKitIntro iOS-Beispielanwendung](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) und Apple [HomeKit](https://developer.apple.com/homekit/) Dokumentation.

## <a name="handoff-framework-changes"></a>Übergabe-Frameworkänderungen

Übergabe (auch bekannt als Geschäftskontinuität) wurde von Apple iOS 8 und OS X Yosemite (10.10) als eine Möglichkeit für den Benutzer an eine Aktivität eines ihrer Geräte (IOS- oder Mac) beginnen, und weiterhin diese gleichen Aktivität auf einem anderen Geräte (wie anhand des Benutzers iClou identifiziert eingeführt. (d Konto).

Übergabe wurde in den iOS 9, um neue, unterstützen auch erweiterte Suchfunktionen erweitert. Weitere Informationen finden Sie unserem [Verbesserungen bei der Suche](~/ios/platform/search/index.md) Dokumentation. Weitere Informationen zur Verwendung von Übergabeanimationen finden Sie unserem [Einführung in die Übergabe](~/ios/platform/handoff.md) Dokumentation.

## <a name="new-extension-points"></a>Neue Erweiterungspunkte

In iOS 8 hat Apple Erweiterungen eingeführt – Bibliotheken, die vom Betriebssystem in standard Kontexten wie z. B. in der Mitteilungszentrale angezeigt werden, wenn der Benutzer die Tastatur anfordert oder wenn sie ein Foto bearbeiten.

Mit iOS 9, Apple Unterstützung durch die Bereitstellung mehrere neue erweitert _Erweiterungspunkte_ , definieren Sie Nutzungsrichtlinien und bieten APIs für die Arbeit in einem bestimmten Bereich wie folgt:

- **Neuen Audio Unit-Erweiterungspunkt** – mit diesem Erweiterungspunkt zum Bereitstellen von Audioeffekten, Musikinstrumente, sound Generatoren usw. für die Verwendung in anderen Audio Unit-Host-apps (z. B. GarageBand) verwenden. Mit diesem Erweiterungspunkt können Sie verkaufen auch _Audioeinheiten_ (audio-Plug-ins) auf dem App Store.
- **Neuen Index Wartung Erweiterungspunkt** – verwenden Sie diesen Erweiterungspunkt zur Unterstützung der neuindizierung von app-Daten, ohne dass eine app erneut starten.
- **Neue Netzwerk-Erweiterungspunkte** (erfordern spezielle Berechtigung von Apple):
    - **App-Proxy-Anbietererweiterung** – mit diesem Erweiterungspunkt zum Implementieren von benutzerdefinierten transparentes Netzwerk von der clientseitigen Proxy verwenden.
    - **Filtern von Datenanbieter / Filter-Steuerelement-Anbietererweiterung** -verwenden Sie diese Erweiterungspunkte, um dynamische Netzwerkinhalte Filterung auf dem Gerät zu implementieren.
    - **Paket-Tunnel-Anbietererweiterung** – mit diesem Erweiterungspunkt Implementierung ein benutzerdefiniertes VPN-Tunneling-Protokoll der clientseitigen verwenden.
- **Neue Erweiterungspunkte für Safari**:
    - **Erweiterung blockiert Inhalte** – mit diesem Erweiterungspunkt verwenden, um eine Liste der blockierten Inhalte zu definieren, die nicht angezeigt wird, wenn der Benutzer im Internet durchsucht.
    - **Freigegebene Links Erweiterung** – verwenden Sie diesen Erweiterungspunkt zum Anzeigen von Inhalt von Safari Shared Links Ihrer app zu aktivieren.

Weitere Informationen finden Sie unserem [Einführung in die Erweiterungen](~/ios/platform/extensions.md) und Apple [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) Dokumentation.

## <a name="keychain-enhancements"></a>Keychain-Verbesserungen

In iOS 9 wurde verbessert, Apple die Keychain aus, um einen neuen Schlüssel Verschlüsselungstyp für die sichere Enclave und weitere Elementoptionen Schutz wie folgt bereitstellen:

- Eine neue Touch ID-Einschränkung, die Schlüsselbund-Objekte ungültig, wenn die Fingerabdruck-Datenbank geändert wird.
- Neue Beschränkungen, die es ermöglichen, nur die Einträge der Zugriffssteuerungsliste mit Touch ID oder der Kennung erstellen.
- Eine neue authentifizierungskontext, der Ihnen ermöglicht, das Aufrufen der Authentifizierung getrennt von `SecItem` aufrufen.
- Zugriff auf Access Control List Entropie (mithilfe der Option für die Anwendung) für die Verschlüsselung der app bereitgestellten Keychain-Element.
- Unterstützung für das Generieren und Verwenden von Schlüsseln innerhalb der sicheren Enclave (über die `kSecAttrTokenIDSecureEnclave` Attribut).

Weitere Informationen finden Sie unserem [Einführung in die Touch ID](~/ios/platform/touchid.md) Dokumentation.


## <a name="right-to-left-language-support"></a>Rechts-nach-links-sprachunterstützung

In iOS 9 hat Apple präsentieren einer gespiegelten Benutzeroberfläche einfacher als je zuvor durch die vollständige Unterstützung für rechts-nach-links-Sprachen. Hierzu gehören folgende Elemente:

- Standard [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) Steuerelemente werden automatisch kippt, rechts-nach-links-basierend auf den iOS-Geräte-Gebietsschema und die Sprache-Einstellungen.
- Die [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) Klasse enthält Attribute, mit denen Sie definieren, wie eine bestimmte Ansicht angezeigt werden soll, gekippt rechts-nach-links.
- Die Möglichkeit, programmgesteuert kippen Sie ein Bild mithilfe der [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) Eigenschaft der [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) Klasse.

Weitere Informationen finden Sie unter Apple [unterstützen rechts-nach-links-Sprachen](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) Dokumentation.



## <a name="additional-framework-changes"></a>Zusätzliche-Frameworkänderungen

Zusätzlich zu den wichtigsten Änderungen, denen wir oben behandelt wurden, hat Apple Änderungen und Verbesserungen an mehrere vorhandene Frameworks für iOS 9, einschließlich der folgenden:

- AV-Foundation-Framework
- AVKit-Framework
- CloudKit-Framework
- Foundation-Framework
- Übergabe-Framework
- HealthKit-Framework
- HomeKit-Framework
- Lokale Authentifizierungs-Frameworks
- MapKit-Framework
- PassKit-Framework
- Safari-Services-Framework
- UIKit-Framework

Weitere Informationen finden Sie unserem [Weitere iOS 9-Frameworkänderungen](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) Dokumentation.

## <a name="deprecated-apis-and-functions"></a>Veraltete APIs und Funktionen

Apple hat die folgenden APIs und Funktionen in iOS 9 als veraltet markiert:

- **Adresse Buch und die Benutzeroberfläche der Adressbuch** -diese APIs wurden durch die wenden Sie sich an, und wenden Sie sich an UI-Frameworks ersetzt. Weitere Informationen finden Sie unserem [Kontakte und Kontakte UI](~/ios/platform/contacts.md) Dokumentation.
- **CBCentralManager** : die `RetrievePeripherals` und `RetrieveConnectedPeripherals` Methoden der `CBCentralManager` Klasse in iOS 9 entfernt wurden. Aufrufen dieser Methoden führt dazu, dass eine app zu einem Absturz, wenn Zubehör Kopplung oder beim Start der app.
- **FetchAllChanges** : die `FetchAllChanges` von der `CKFetchRecordChangesOperation` -Klasse wurde jedoch veraltet und wird in iOS 9 entfernt.
- **Media Player** : der Media Player-Framework in iOS 9 ist veraltet. Verwenden Sie stattdessen AVKit oder AV-Foundation-APIs.

Eine vollständige Liste der veralteten für bestimmte API, finden Sie unter Apple [iOS 9.0-API-Unterschiede](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) Dokumentation.

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

Testen Sie auch die iOS-Teile dieser Beispiele (Companion Mac OS X-Versionen verfügbar!) ein:

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Einführung in 3D Touch](~/ios/platform/3d-touch.md)
- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Multitasking für iPad](~/ios/platform/multitasking.md)
- [Kontakte und UI-Kontakte](~/ios/platform/contacts.md)
- [Neue Suche-APIs](~/ios/platform/search/index.md)
- [Einführung in die Aufrufliste anzeigen](~/ios/user-interface/controls/uistackview.md)
- [Änderungen der Auflistung anzeigen](~/ios/user-interface/controls/uicollectionview.md)
- [Gaming-Verbesserungen](~/ios/platform/gaming/index.md)
- [Einführung in HomeKit](~/ios/platform/homekit.md)
- [Einführung in die Übergabe](~/ios/platform/handoff.md)
- [Weitere iOS 9-Frameworkänderungen](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Problembehandlung](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualisieren Ihre Xamarin.iOS-apps auf iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
