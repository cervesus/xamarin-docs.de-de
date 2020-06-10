---
title: Einführung in iOS 9
description: In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in ios 9 für xamarin. IOS-Entwickler zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: de4b6e8b95eed33e7fb38baf51a0da73cef313c0
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574131"
---
# <a name="introduction-to-ios-9"></a>Einführung in iOS 9

_In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in ios 9 für xamarin. IOS-Entwickler zur Verfügung stehen._

![](images/ios9-sml.png "The iOS 9 logo")

Apple hat mehrere neue APIs und Dienste in ios 9 zusammen mit vielen Verbesserungen an vorhandenen Features hinzugefügt.

## <a name="3d-touch"></a>3D Touch

Neu bei IOS 9 und der iPhone 6S und iPhone 6S Plus ist eine 3D-Toucheingabe, die ihren IOS-apps druckempfindliche Gesten hinzufügt. Mit der 3D-Toucheingabe kann eine iPhone-APP jetzt nicht nur erkennen, dass der Benutzer den Bildschirm des Geräts berührt, sondern auch festzustellen, wie viel Druck der Benutzer ausübt und auf die unterschiedlichen Druck Ebenen antwortet.

der 3D-Fingerabdruck bietet die folgenden Features für Ihre APP:

- Unterscheidung nach **Druck** : Apps können nun Messen, wie schwer oder hell der Benutzer den Bildschirm berührt und diese Informationen nutzt. Beispielsweise kann eine Zeichnungs-App abhängig von der Art und Weise, in der der Benutzer den Bildschirm berührt, zu einem dickeren oder dünneren.
- **Peek und Pop** : Ihre APP kann jetzt den Benutzer mit seinen Daten interagieren, ohne aus dem aktuellen Kontext navigieren zu müssen. Wenn Sie auf dem Bildschirm hart drücken, *können Sie sich das* gewünschte Element ansehen (z. b. eine Vorschau der Nachricht). Durch das Drücken von "Harder" können *Sie in das* Element eingeblendet werden.
- **Schnelle Aktionen** : Stellen Sie sich schnell Aktionen wie die Kontextmenüs vor, die angezeigt werden können, wenn ein Benutzer mit der rechten Maustaste auf ein Element in einer Desktop-App klickt. Mithilfe von schnellen Aktionen können Sie in der APP über das Startbildschirm Symbol auf dem IOS-Gerät allgemeine, schnelle und leicht zu verwendbare Verknüpfungen zu Funktionen in Ihrer APP hinzufügen.

Weitere Informationen finden Sie im Artikel [Einführung in die 3D-](~/ios/platform/3d-touch.md) Eingabe Anleitung.

## <a name="app-transport-security"></a>App-Transportsicherheit

Neu in ios 9: die APP-Transport Sicherheit (app Transport Security, ATS) erzwingt sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und ihrer app. Mithilfe von ATS wird sichergestellt, dass die gesamte Internetkommunikation den bewährten Methoden der sicheren Verbindung entspricht. Dadurch wird die versehentliche Offenlegung vertraulicher Informationen entweder direkt über Ihre APP oder eine von ihr verwendete Bibliothek verhindert.

Da ATS in apps, die für IOS 9 und OS X 10,11 (El Capitan) erstellt werden, standardmäßig aktiviert ist, gelten für alle Verbindungen, die [NSURLConnection](xref:Foundation.NSUrlConnection), [cfurl](xref:CoreFoundation.CFUrl) oder [nsurlsession](xref:Foundation.NSUrlSession) verwenden, Sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, schlagen Sie mit einer Ausnahme fehl.

Weitere Informationen zu ATS finden Sie in unserem Handbuch zur [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md) .

<a name="multitasking"></a>

## <a name="multitasking-for-ipad"></a>Multitasking für iPad

Mit IOS 9 hat Apple Multitasking-Unterstützung für die gleich malige Ausführung von zwei apps auf einer bestimmten iPad-Hardware hinzugefügt. Folglich können Ihre xamarin. IOS-apps nicht mehr davon ausgehen, dass es sich um die einzige APP handelt, die zu einem beliebigen Zeitpunkt ausgeführt wird oder ob Sie Zugriff auf den vollständigen Bildschirm oder die Ressourcen des Geräts haben.

Multitasking für iPad wird durch die folgenden Features unterstützt:

- **Folie over** : ermöglicht es dem Benutzer, eine zweite IOS-App temporär in einem Schiebe Bereich auszuführen (entweder auf der rechten oder linken Seite des Bildschirms basierend auf der Sprache), der ungefähr 25% der derzeit ausgelaufenden Haupt-App abdeckt. Die Folie ist nur auf iPad pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 oder iPad Mini 4 verfügbar.
- **Geteilte Ansicht** : auf der unterstützten iPad-Hardware (iPad Air 2, iPad Mini 4 und iPad pro) kann der Benutzer eine zweite App auswählen und parallel mit der aktuell aktiven app in einem Split Screen-Modus ausführen. Der Benutzer kann den Prozentsatz des Hauptbildschirms steuern, den jede APP einnimmt.
- **Bild im Bild** : für apps, die Videoinhalte wiedergeben, kann das Video nun in einem Fenster wiedergegeben werden, das sich in einem Fenster befindet und in der Größe geändert werden kann Der Benutzer hat die vollständige Kontrolle über die Größe und Position dieses Fensters. Bild im Bild ist nur auf iPad pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 oder iPad Mini 4 verfügbar.

Weitere Informationen zu den neuen Multitasking-Funktionen von IOS 9 finden Sie in unserem Handbuch [zum Multitasking für iPad](~/ios/platform/multitasking.md) .

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Neue Benutzeroberflächen-Frameworks und Kontakte

Mit der Einführung von IOS 9 hat Apple zwei neue Frameworks ( [Kontakte](xref:Contacts) und [contactsui](xref:ContactsUI)) veröffentlicht, die das vorhandene Adressbuch und die Benutzeroberflächen-Frameworks der Adressbücher ersetzen, die von IOS 8 und früheren Versionen verwendet werden.

Diese neuen, objektorientierten Frameworks stellen Folgendes bereit:

- **Kontakte** – bietet xamarin. IOS Zugriff auf die Kontaktinformationen des Benutzers. Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für Thread sicheren, schreibgeschützten Zugriff optimiert.
- **Contactsui** – bietet xamarin. IOS-Benutzeroberflächen Elemente zum Anzeigen, bearbeiten, auswählen und Erstellen von Kontakten auf IOS-Geräten.

Weitere Informationen finden Sie in unserer Benutzeroberflächen Dokumentation zu [Kontakten und Kontakten](~/ios/platform/contacts.md) .

## <a name="new-search-apis"></a>Neue Such-APIs

Die Suche wurde in ios 9 erweitert und bietet hervorragend neue Möglichkeiten für den Zugriff auf Informationen in ihrer xamarin. IOS-app. Mithilfe der neuen Such-APIs können Sie den Inhalt Ihrer APP über Spotlight-und Safari-Suchergebnisse, Übergabe und Siri-Erinnerungen und Vorschläge durchsuchbar machen. Dadurch können Benutzer schnell auf Aktivitäten und Informationen innerhalb ihrer App zugreifen.

Außerdem vereinfachen die neuen Such-APIs das Integrieren der Suche in Ihre APP, ohne dass eine vorherige Such Implementierung möglich ist. Daher beansprucht Apple in der Regel einige Stunden, bis der Inhalt einer IOS 9-APP über die APP-Suche universell durchsuchbar ist.

Weitere Informationen finden Sie in der Dokumentation zu den [Such Erweiterungen](~/ios/platform/search/index.md) .

## <a name="new-stack-view"></a>Neue Stapel Ansicht

Das Stapel Ansicht-Steuerelement ([uistackview](xref:UIKit.UIStackView) nutzt die Leistungsfähigkeit der automatischen Layout-und Größenklassen, um einen Stapel von unter Sichten (horizontal oder vertikal) zu verwalten, der dynamisch auf die Ausrichtung und Bildschirmgröße des IOS-Geräts antwortet.

Mithilfe des Stapel Ansicht-Steuer Elements wird die Menge an Arbeit, die für das Layout einer Benutzeroberfläche erforderlich ist, erheblich reduziert. Das Layout aller untergeordneten Sichten, die an eine Stapel Ansicht angefügt sind, wird automatisch auf der Grundlage von vom Entwickler definierten Eigenschaften wie Achse, Verteilung, Ausrichtung und Abstand verwaltet.

Weitere Informationen finden Sie [in der Einführung in die Stack View](~/ios/user-interface/controls/uistackview.md) -Dokumentation.

## <a name="collection-view-changes"></a>Änderungen der Sammlungsansicht

In ios 9 unterstützt die Auflistungs Ansicht ("[uicollectionview](xref:UIKit.UICollectionView) " jetzt die Neuanordnung von Elementen nach oben, indem eine neue Standard Gestenerkennung und mehrere neue unterstützende Methoden hinzugefügt werden.

Mithilfe dieser neuen Methoden können Sie Drag-to-Order in der Auflistungs Ansicht problemlos implementieren und die Darstellung der Elemente in jeder Phase des Neuanordnungs Vorgangs anpassen.

Weitere Informationen zu den Änderungen der Sammlungsansicht für IOS 9 finden Sie in unserem Handbuch zum [Ändern der Sammlungsansicht](~/ios/user-interface/controls/uicollectionview.md) .

## <a name="game-enhancements"></a>Spiele Erweiterungen

Mit IOS 9 hat Apple einige technologische Verbesserungen an den Gaming-APIs vorgenommen, die die Implementierung von Spielgrafiken und Audiodaten in ihrer xamarin. IOS-App vereinfachen. Diese umfassen sowohl die einfache Entwicklung durch allgemeine Frameworks als auch die Leistungsfähigkeit der GPU des IOS-Geräts, um die Geschwindigkeit und Grafikfunktionen mit Low-Level-Erweiterungen zu verbessern.

Dies umfasst gameplaykit, replaykit, Model I/O, Metal Kit und Metal Performance Shaders sowie neue, erweiterte Features von Metal, scenekit und spritekit.

Weitere Informationen finden Sie in unserer Dokumentation zur [Spiel Verbesserung](~/ios/platform/gaming/index.md) .

## <a name="homekit-framework-changes"></a>Änderungen am homekit-Framework

Das [homekit](xref:HomeKit) -Framework, das in ios 8 eingeführt wurde, bietet die Möglichkeit zum Einrichten und Steuern verschiedener homekit-fähiger Zubehör (z. b. automatisierter Beleuchtung, Tür Sperren und Türöffnungs Türen) aus einer xamarin. IOS-app. Homekit-Zubehör kann nicht nur einfach einzurichten und zu konfigurieren, sondern auch über gesprochene Siri-Befehle gesteuert werden.

In ios 9 hat Apple das Setup vereinfacht, die unterstützten Zubehör Typen erweitert und mehr Zubehör Interaktionen bereitgestellt (z. b. die Remote Steuerung von Zubehör über icloud).

Weitere Informationen finden [Sie in unserer Einführung in homekit](~/ios/platform/homekit.md), [homekitintro IOS-Beispiel-App](https://docs.microsoft.com/samples/xamarin/ios-samples/homekit-homekitintro) und Dokumentation zu [homekit](https://developer.apple.com/homekit/) von Apple.

## <a name="handoff-framework-changes"></a>Übergabe von frameworkänderungen

Die Übergabe (auch als Kontinuität bezeichnet) wurde von Apple in ios 8 und OS X Yosemite (10,10) eingeführt, sodass der Benutzer eine Aktivität auf einem Ihrer Geräte (IOS oder Mac) starten und dieselbe Aktivität auf einem anderen Gerät (wie durch das icloud-Konto des Benutzers identifiziert) fortsetzen kann.

Die Übergabe von Handoff wurde in ios 9 erweitert, um auch neue, erweiterte Suchfunktionen zu unterstützen. Weitere Informationen finden Sie in der Dokumentation zu den [Such Erweiterungen](~/ios/platform/search/index.md) . Weitere Informationen zur Verwendung von Handoff finden Sie in unserer [Einführung in die Übergabe](~/ios/platform/handoff.md) Dokumentation.

## <a name="new-extension-points"></a>Neue Erweiterungspunkte

In ios 8 hat Apple Erweiterungen – Bibliotheken eingeführt, die vom Betriebssystem in Standard Kontexten, z. b. innerhalb des Benachrichtigungs Centers, wenn der Benutzer eine Tastatur anfordert, oder beim Bearbeiten eines Fotos angezeigt werden.

Mit IOS 9 erweitert Apple die Erweiterungs Unterstützung durch die Bereitstellung mehrerer neuer _Erweiterungs Punkte_ , die Verwendungs Richtlinien definieren und APIs für die Arbeit innerhalb eines bestimmten Bereichs bereitstellen, wie im folgenden dargestellt:

- **Neuer AudioUnit-Erweiterungs Punkt** – verwenden Sie diesen Erweiterungs Punkt, um Audioeffekte, Musikinstrumente, Soundgeneratoren usw. zur Verwendung in anderen AudioUnit-Host-Apps (z. b. GarageBand) bereitzustellen. Mit diesem Erweiterungs Punkt können Sie auch _Audioeinheiten_ (Audioplug-ins) im App Store verkaufen.
- **Neuer Index Wartungs Erweiterungs Punkt** – verwenden Sie diesen Erweiterungs Punkt zur Unterstützung der Neuindizierung von App-Daten, ohne dass ein App-Neustart erforderlich ist.
- **Neue Netzwerk Erweiterungs Punkte** (diese erfordern eine spezielle Berechtigung von Apple):
  - Anwendungs **Proxy-Anbieter Erweiterung** – verwenden Sie diesen Erweiterungs Punkt zum Implementieren eines benutzerdefinierten transparenten Client seitigen Netzwerk Proxys.
  - **Filter Datenanbieter-/filtersteuerungsanbietererweiterung** : Verwenden Sie diese Erweiterungs Punkte, um die dynamische Netzwerk Inhalts Filterung auf dem Gerät zu implementieren.
  - **Paket Tunnel Anbieter-Erweiterung** – verwenden Sie diesen Erweiterungs Punkt, um ein benutzerdefiniertes VPN-Tunneling-Protokoll auf Clientseite zu implementieren.
- **Neue Safari-Erweiterungs Punkte**:
  - **Erweiterung für Inhalts Blockierung** – verwenden Sie diesen Erweiterungs Punkt, um eine Liste der blockierten Inhalte zu definieren, die nicht angezeigt werden, wenn der Benutzer das Internet durchsucht.
  - **Erweiterung für freigegebene Links** – verwenden Sie diesen Erweiterungs Punkt, um die Anzeige des Inhalts ihrer app in den freigegebenen Links von Safari zu aktivieren.

Weitere Informationen finden Sie [in unserer Einführung in Erweiterungen](~/ios/platform/extensions.md) und in der Dokumentation zum [Programmier Handbuch für App](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) -Erweiterungen von Apple.

## <a name="keychain-enhancements"></a>Keychain-Erweiterungen

In ios 9 hat Apple die Keychain erweitert, um einen neuen Verschlüsselungs Schlüsseltyp für die Secure Enclave und weitere Element Schutz Optionen wie folgt bereitzustellen:

- Eine neue Berührungs-ID-Einschränkung, die Keychain-Elemente für ungültig erklärt, wenn die Fingerabdruckdatenbank geändert wird.
- Neue Einschränkungen, die das Erstellen von Access Control Listeneinträgen mit der Fingereingabe-ID oder Kennung ermöglichen.
- Ein neuer Authentifizierungs Kontext, mit dem Sie die Authentifizierung unabhängig von den aufrufen aufrufen können `SecItem` .
- Access Control Listen Entropie (mit der Option "Anwendungs Kennwort") für die von der APP bereitgestellte Schlüsselbund Element-Verschlüsselung.
- Unterstützung für das Erstellen und Verwenden von Schlüsseln innerhalb der sicheren Enclave (über das- `kSecAttrTokenIDSecureEnclave` Attribut).

Weitere Informationen finden Sie unter Fingereingabe [-ID und Face ID in xamarin. IOS](~/ios/platform/touch-id-face-id.md).

## <a name="right-to-left-language-support"></a>Sprachunterstützung von rechts nach links

In ios 9 hat Apple die Darstellung einer geflickten Benutzeroberfläche einfacher als je zuvor durch Bereitstellen der vollständigen Unterstützung von rechts-nach-links-Sprachen bereitgestellt. Dazu gehören:

- Standard mäßige [UIKit](xref:UIKit) -Steuerelemente werden basierend auf den Gebiets Schema-und Spracheinstellungen der IOS-Geräte automatisch von rechts nach links gekippt.
- Die [UIView](xref:UIKit.UIView) -Klasse stellt Attribute bereit, mit denen Sie definieren können, wie eine bestimmte Ansicht angezeigt werden soll, wenn Sie von rechts nach links gekippt werden.
- Die Möglichkeit zum programmgesteuerten Kippen eines Bilds mithilfe der [flipsforrighttoleftlayoutdirection](xref:UIKit.UIImage.FlipsForRightToLeftLayoutDirection) -Eigenschaft der [uiimage](xref:UIKit.UIImage) -Klasse.

Weitere Informationen finden Sie in der Dokumentation von Apple, die von [rechts nach Links unterstützt](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) wird.

## <a name="additional-framework-changes"></a>Weitere frameworkänderungen

Zusätzlich zu den wichtigsten Änderungen, die wir oben beschrieben haben, hat Apple Änderungen und Verbesserungen an mehreren vorhandenen Frameworks für IOS 9 vorgenommen, einschließlich der folgenden:

- AV Foundation-Framework
- Avkit-Framework
- Cloudkit-Framework
- Foundation-Framework
- Handoff-Framework
- Healthkit-Framework
- Homekit-Framework
- Lokales Authentifizierungs Framework
- MapKit-Framework
- Passkit-Framework
- Safari Services-Framework
- UIKit-Framework

Weitere Informationen finden Sie in unserer zusätzlichen Dokumentation zu [IOS 9 Framework-Änderungen](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) .

## <a name="deprecated-apis-and-functions"></a>Veraltete APIs und Funktionen

Apple hat die folgenden APIs und Funktionen in ios 9 als veraltet markiert:

- **Adressbuch & Address Book UI** : Diese APIs wurden durch die Benutzeroberflächen-Frameworks Contact und Contact ersetzt. Weitere Informationen finden Sie in unserer Benutzeroberflächen Dokumentation zu [Kontakten und Kontakten](~/ios/platform/contacts.md) .
- **Cbcentralmanager** : die- `RetrievePeripherals` Methode und die- `RetrieveConnectedPeripherals` Methode der- `CBCentralManager` Klasse wurden in ios 9 entfernt. Wenn Sie diese Methoden aufrufen, stürzt eine App ab, wenn ein Zubehör oder ein App-Start paarweise gekoppelt wird.
- **Fetchallchanges** -der `FetchAllChanges` der `CKFetchRecordChangesOperation` Klasse wurde abgewertet und wird in ios 9 entfernt.
- **Media Player** -das Media Player Framework ist in ios 9 veraltet. Verwenden Sie stattdessen avkit oder die AV Foundation-APIs.

Eine umfassende Liste der spezifischen API-Abgrenzungen finden Sie in der Dokumentation zu den [IOS 9,0-API-Distributionen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) von Apple.

## <a name="ios-9-sample-apps"></a>IOS 9-Beispiel-apps

Wir haben einige [IOS 9-spezifische Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9) für den Einstieg:

- [Astrolayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [Metalperformanceshadershelloworld](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-metalperformanceshadershelloworld)
- [Musik Bewegung](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-musicmotion)
- [Photoprogress](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-photoprogress)
- [Abbild Katalogisierung](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-seguecatalog)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- ["Stickycorners"](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Sehen Sie sich auch die IOS-Abschnitte dieser Beispiele an (Begleit Mac OS X Versionen kommen!):

- [Agentscatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [Metalkitessentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Einführung in 3D-Fingereingabe](~/ios/platform/3d-touch.md)
- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Multitasking für iPad](~/ios/platform/multitasking.md)
- [Benutzeroberfläche von Kontakten und Kontakten](~/ios/platform/contacts.md)
- [Neue Such-APIs](~/ios/platform/search/index.md)
- [Einführung in die Stapel Ansicht](~/ios/user-interface/controls/uistackview.md)
- [Änderungen der Sammlungsansicht](~/ios/user-interface/controls/uicollectionview.md)
- [Spiele Erweiterungen](~/ios/platform/gaming/index.md)
- [Einführung in homekit](~/ios/platform/homekit.md)
- [Einführung in die Übergabe](~/ios/platform/handoff.md)
- [Weitere iOS 9-Frameworkänderungen](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Problembehandlung](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualisieren Ihrer xamarin. IOS-apps auf iOS9 (Video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
