---
title: Arbeiten mit Eigenschaften Listen in xamarin. IOS
description: In diesem Dokument wird der grafische und erweiterte Eigenschaften Listen-Editor (plist) Visual Studio für Mac für die Verwendung von "Info. plist" und "Berechtigungen. plist" eingeführt. Es veranschaulicht die Festlegung von Symbolen und Start Bildern für IOS-Anwendungen in Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 6c2b5869f647f65b932b6ec92f359f8a79402c8f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569295"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Arbeiten mit Eigenschaften Listen in xamarin. IOS

_In diesem Dokument wird der grafische und erweiterte Eigenschaften Listen-Editor (plist) Visual Studio für Mac für die Verwendung von "Info. plist" und "Berechtigungen. plist" eingeführt. Es veranschaulicht die Festlegung von Symbolen und Start Bildern für IOS-Anwendungen in Visual Studio für Mac._

Visual Studio für Mac bietet einen grafischen plist-Editor, der das Bearbeiten von App-Eigenschaften und-Funktionen vereinfacht. Visual Studio für Mac verfügt über zwei. plists- `Info.plist` zum Bearbeiten von App-Eigenschaften und Symbolen sowie `Entitlements.plist` zum Verwalten von App-Funktionen. In diesem Leitfaden wird die Info. plists vorgestellt, und Sie erhalten eine Übersicht über die Arbeit mit Ihnen in Visual Studio für Mac. Weitere Informationen zu "Berechtigungen. plist" finden Sie im Handbuch [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) .

## <a name="infoplist"></a>Info.plist

Die Informations Eigenschaften Liste ( `Info.plist` ) ist eine erforderliche IOS-Datei, die Informationen über die Konfiguration Ihrer Anwendung für das System bereitstellt. Der benutzerdefinierte `Info.plist` Editor von Visual Studio für Mac enthält drei Panels, die von Registerkarten unten links im Editor Fenster gesteuert werden:

 [![](property-lists-images/tabs.png "The Info.plist editor tabs at the bottom left of the editor window")](property-lists-images/tabs.png#lightbox)

Jeder Bereich steuert verschiedene Eigenschaften, wie unten beschrieben:

- **Anwendungs Panel** : eine grafische Benutzeroberfläche, um allgemeine Anwendungseigenschaften sowie Symbole und Start Bilder festzulegen. Hiermit werden Karten Integration und backerdmodi angegeben.
- **Advanced Panel** (erweiterter Bereich): im erweiterten Bereich können Unterstützte Dokumenttypen, utis und URL-Typen angegeben werden.
- **Quell Panel** : der Quellbereich steuert weniger allgemeine Eigenschaften sowie benutzerdefinierte Eigenschaften für die Anwendung.

In den nächsten drei Abschnitten werden die Features der einzelnen Panels ausführlicher untersucht.

## <a name="application-panel"></a>Anwendungs Panel

Visual Studio für Mac verfügt über eine grafische Benutzeroberfläche zum Bearbeiten allgemeiner `Info.plist` Einträge für eine Anwendung:

1. Anwendungseigenschaften
1. Unterstützte Gerätetypen
1. Unterstützung von Ausrichtungen für die einzelnen Gerätetypen
1. Stil und Farbe der Status Leiste
1. Symbole und Startbildschirme
1. Karten und hintergrundmodi

Diese werden in den nächsten Abschnitten ausführlicher beschrieben.

 <a name="iOS_Application_Target"></a>

### <a name="ios-application-target"></a>IOS-Anwendungs Ziel

Dieser Abschnitt enthält wichtige Informationen, die Ihre Anwendung beschreiben.
Der hier gespeicherte **Bezeichner** muss mit dem Bündel Bezeichner identisch sein, der in iTunes Connect (für App Store-Apps) und auch in der APP-ID-Liste des IOS-Bereitstellungs Portals und in den Entwicklungs-und Verteilungs Zertifikaten eingegeben wird

 [![](property-lists-images/image24.png "iOS Application Target")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Geräte Bereitstellung

 [![](property-lists-images/deployment.png "Device Deployment")](property-lists-images/deployment.png#lightbox)

Die Abschnitts Informationen zur Geräte **Bereitstellung** werden selektiv angezeigt, abhängig von der Auswahl in der Dropdown Liste " **Geräte** " im obigen Abschnitt " **Anwendungs Ziel** ". Die Dropdown-Seite der **Hauptschnittstelle** wird in Storyboard-gesteuerten Anwendungen auf **mainstoryboard** festgelegt. Wenn die Benutzeroberfläche vollständig im Code geschrieben ist, kann dies leer bleiben.

### <a name="supported-device-orientations"></a>Unterstützte Geräte Ausrichtungen

 **Unterstützte Geräte Ausrichtungen** steuern, wie die APP auf die Geräte Rotation antwortet. Es ist sehr üblich, dass iPhone/iPad- **Apps nur das**Hochformat oder alles, aber einen **aufwärts**unterstützen. Im Allgemeinen sollten alle iPad-Anwendungen außer Games alle Ausrichtungen unterstützen.

### <a name="status-bar-styles"></a>StatusBar-Stile

Der Abschnitt "Status leisten- **Stile** " ist eine grafische Benutzeroberfläche zum Bearbeiten der Anwendungen `UIStatusBarStyle` :

 [![](property-lists-images/status.png "Status Bar Styles")](property-lists-images/status.png#lightbox)

 <a name="Icons"></a>

### <a name="icons-launch-images-and-itunes-artwork"></a>Symbole, Start Bilder und iTunes-Grafiken

Informationen zum Verwenden von Symbolen, Bildern und Grafiken in der Datei "Info. plist" finden Sie im Handbuch [Working with Images (Arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md) ).

### <a name="maps-integration-and-background-modes"></a>Zuordnungen von Integrations-und hintergrundmodi

Die `Info.plist` enthält spezielle Abschnitte zum Angeben von Zuordnungs-und backerden-Modi. Wenn Sie die Optionen auswählen, die Sie unterstützen möchten, fügen Sie der Anwendung die erforderlichen Eigenschaften für Sie hinzu.

 [![](property-lists-images/maps.png "Maps Integration")](property-lists-images/maps.png#lightbox)

Weitere Informationen zum Arbeiten mit Maps finden Sie im Leitfaden zu xamarin [IOS Maps](~/ios/user-interface/controls/ios-maps/index.md) .

 [![](property-lists-images/bging.png "Background Modes")](property-lists-images/bging.png#lightbox)

Weitere Informationen zu den hintergrundmodi finden Sie im Leitfaden xamarin [Backgroundin IOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) .

## <a name="advanced-panel"></a>Erweiterter Bereich

Der Bereich erweitert steuert die Dokumenttypen und URL-Schemas, die von der Anwendung unterstützt werden.

 [![](property-lists-images/image34.png "Advanced Panel")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types"></a>

## <a name="document-types"></a>Dokumenttypen

Für Anwendungen, die das Öffnen bestimmter Dateitypen unterstützen, stellt IOS den `CFBundleDocumentTypes` Schlüssel bereit. Wenn unsere Anwendung bestimmte bekannte Dateitypen unterstützen soll (z. b. pdf), würden wir den PDF-Wert dem Schlüssel hinzufügen. Dieser Abschnitt bietet eine bequeme Möglichkeit, die Daten einzugeben, die im- `CFBundleDocumentTypes` Schlüssel in der Datei gespeichert werden `Info.plist` .

Ausführliche Informationen zum Konfigurieren dieser Werte finden Sie in der Dokumentation zum [Registrieren der von der APP unterstützten Dateitypen](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) .

## <a name="utis"></a>UTIs

Manchmal muss eine Anwendung das Öffnen eines benutzerdefinierten Dateityps unterstützen. Beispielsweise kann es sein, dass Sie Bilddateien mit einer benutzerdefinierten Erweiterung *. XAM*öffnen möchten. Um einen benutzerdefinierten Dateityp anzugeben, erstellen wir mithilfe des Schlüssels einen benutzerdefinierten Typ Bezeichner mit dem Typ "UTI-Universal" `UIExportedTypeDeclarations` . Der folgende Screenshot zeigt, wie Sie eine benutzerdefinierte UTI-Datei für die Erweiterung. XAM erstellen:

 [![](property-lists-images/uti.png "UTIs Editor")](property-lists-images/uti.png#lightbox)

Ebenso wie exportierte Typ-utis benutzerdefinierte utis angeben, die für Ihre APP spezifisch sind, geben die *importierten Typ-utis* ( `UIImportedTypeDeclarations` Key) benutzerdefinierte Typen an, die unterstützt werden, aber nicht Ihrer Anwendung

Weitere Informationen zur Verwendung von benutzerdefinierten utis finden Sie unter Apple- [Registrierungs Dateitypen, die von ihrer App unterstützt](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) werden.

## <a name="custom-urls"></a>Benutzerdefinierte URLs

Ein URL-Schema Name (auch als Protokoll bezeichnet) ist der erste Teil der URL. Beispielsweise `http://` sind und `https://` Allgemeine URL-Schemas. Sie haben die Möglichkeit, ein benutzerdefiniertes URL-Schema für Ihre Anwendung zu erstellen. Benutzerdefinierte URL-Schemas werden für die Kommunikation und das Senden von Daten mit anderen Anwendungen verwendet. Der folgende Screenshot veranschaulicht das Erstellen eines neuen benutzerdefinierten URL-Schemas namens `monkeys://` :

 [![](property-lists-images/url.png "Custom URLs")](property-lists-images/url.png#lightbox)

Weitere Informationen zum Implementieren von benutzerdefinierten URL-Schemas finden Sie im [Abschnitt Implementieren von benutzerdefinierten URL-Schemas](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html) in Apple in diesem Handbuch.

## <a name="source-panel"></a>Quell Panel

Die Registerkarte **Quelle** der `Info.plist` Datei ermöglicht das Hinzufügen oder Bearbeiten von benutzerdefinierten Werten. Visual Studio für Mac enthält eine Liste der gängigsten Eigenschaften:

 [![](property-lists-images/image31.png "Adding a new property from a dropdown")](property-lists-images/image31.png#lightbox)

Für bekannte Eigenschaften Visual Studio für Mac eine Liste gültiger Werte, wie im folgenden Screenshot veranschaulicht:

 [![](property-lists-images/image32.png "Select a value from a know value list")](property-lists-images/image32.png#lightbox)

Visual Studio für Mac erkennt auch den Eigenschaftentyp, wie hier gezeigt:

 [![](property-lists-images/image33.png "The available property types")](property-lists-images/image33.png#lightbox)

Weitere Informationen zu optionalen Eigenschaften finden Sie in den Links zu den [App-bezogenen Ressourcen](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) von Apple.

 <a name="Entitlements"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Sie mit den grafischen und erweiterten plist-Editoren gängige App-Konfigurationen bearbeiten sowie Symbole und Start Bilder angeben. Außerdem wurde das `Entitlements.plist` zum Hinzufügen und Verwalten von App-Funktionen eingeführt.

## <a name="related-links"></a>Verwandte Links

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [App-bezogene Ressourcen](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Registrieren der von Ihrer APP unterstützten Dateitypen](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Implementieren benutzerdefinierter URL-Schemas](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Referenz zum Asset Catalog-Format](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
