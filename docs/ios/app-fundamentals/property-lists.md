---
title: Arbeiten mit Eigenschaftenlisten in Xamarin.iOS
description: In diesem Dokument werden die Visual Studio für Mac grafischen und der erweiterten Eigenschaft Eigenschaftenliste (plist)-Editor für die Arbeit mit "Info.plist" und "Entitlements.plist". Es veranschaulicht Einstellung Symbole und startbilder für iOS-Anwendungen in Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b102f268fd457ca42f3d64b5766a2b3824e3849d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242328"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Arbeiten mit Eigenschaftenlisten in Xamarin.iOS

_In diesem Dokument werden die Visual Studio für Mac grafischen und der erweiterten Eigenschaft Eigenschaftenliste (plist)-Editor für die Arbeit mit "Info.plist" und "Entitlements.plist". Es veranschaulicht Einstellung Symbole und startbilder für iOS-Anwendungen in Visual Studio für Mac._

Visual Studio für Mac verfügt über eine grafische plist-Editor, der erleichtert die Bearbeitung von app-Eigenschaften und Funktionen, die einfacher. Visual Studio für Mac verfügt über zwei .plists - `Info.plist` zum Bearbeiten von app-Eigenschaften und Symbole, und `Entitlements.plist` für die Verwaltung von app-Funktionen. Dieses Handbuch führt der Info.plists und bietet einen Überblick über die Arbeit mit ihnen in Visual Studio für Mac. Weitere Informationen zu "Entitlements.plist", finden Sie unter den [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Guide.

## <a name="infoplist"></a>Info.plist

Die Eigenschaftenliste Informationen ( `Info.plist`) ist eine erforderliche iOS-Datei, die Informationen der Konfiguration Ihrer Anwendung auf das System bereitstellt. Visual Studio für Mac die benutzerdefinierte `Info.plist` -Editor-Funktionen, die drei Bereiche, gesteuert durch die Registerkarten am unteren Rand im Editor-Fenster links:

 [![](property-lists-images/tabs.png "Die Datei \"Info.plist\"-Editor-Registerkarten unten links im Editor-Fenster")](property-lists-images/tabs.png#lightbox)

Jeder Bereich steuert verschiedene Eigenschaften, wie im folgenden erläutert:

-  **Panel aplikace** -eine grafische Oberfläche zum Festlegen der allgemeinen Eigenschaften der Anwendung als auch Symbole, und starten Sie Bilder; kartenintegration und hintergrundverarbeitung Modi angeben.
-  **Erweiterte Bereich** -im erweiterten Bereich ist der Ort zum Angeben von unterstützten Dokumenttypen UTIs, und die URL-Typen.
-  **Source-Bereich** -Bereich "Quelle" steuert, weniger häufig Eigenschaften als auch für benutzerdefinierte Eigenschaften für die Anwendung.


In den nächsten drei Abschnitten ermitteln Sie die Funktionen der jeder Bereich im Detail.

## <a name="application-panel"></a>Panel aplikace

Visual Studio für Mac bietet eine grafische Benutzeroberfläche zum Bearbeiten von allgemeinen `Info.plist` Einträge für eine Anwendung:

1.  Anwendungseigenschaften
1.  Unterstützter Gerätetypen
1.  Bündelbezeichners für jeden Gerätetyp
1.  Stil und Farbe der Statusleiste
1.  Symbole und Start-Bildschirme
1.  Karten und Hintergrundmodi


Diese werden in den nächsten Abschnitten ausführlicher beschrieben.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS Application Target

Dieser Abschnitt enthält wichtige Informationen, die Ihre Anwendung beschreibt.
Die **Bezeichner** gespeicherten übereinstimmen hier die Bündel-ID, die in iTunes Connect (für die App-Store-apps) und auch in der iOS Provisioning Portal App-IDs aus Entwicklungs-als auch verteilungszertifikate eingegeben werden.

 [![](property-lists-images/image24.png "iOS Application Target")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Gerätebereitstellung

 [![](property-lists-images/deployment.png "Gerätebereitstellung")](property-lists-images/deployment.png#lightbox)

Das Gerät **Bereitstellung** Abschnitten werden selektiv angezeigt, abhängig von der Auswahl in der **Geräte** Dropdownliste in der **Application Target** weiter oben. Die **Hauptschnittstelle** Dropdownliste nastaven NA hodnotu **MainStoryboard** im Storyboard-Anwendungen. Wenn die Benutzeroberfläche vollständig in Code geschrieben ist, und klicken Sie dann diese leer gelassen werden kann.

### <a name="supported-device-orientations"></a>Unterstützte Geräteausrichtungen

 **Unterstützte Geräteausrichtungen** steuert, wie die app auf geräterotation reagiert. Es kommt sehr häufig für iPhone/iPad-apps unterstützen nur **Hochformat**, oder alles, was aber **finden Sie verkehrt herum**. Alle iPad-Anwendungen mit Ausnahme der Spiele unterstützen in der Regel sollten alle Ausrichtungen.

### <a name="status-bar-styles"></a>Statusleiste-Stile

Die **Status Leiste Stile** Abschnitt ist eine grafische Benutzeroberfläche zum Bearbeiten einer Anwendung `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Statusleiste-Stile")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Symbole, Startbilder und iTunes-Grafik

Informationen zur Verwendung der Symbole, Bilder und Vorlagen in der Datei "Info.plist" befinden sich die [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Guide.




### <a name="maps-integration-and-background-modes"></a>Kartenintegration und Hintergrundmodi

Die `Info.plist` enthält spezielle Abschnitte kartenintegration und hintergrundverarbeitung Modi angeben. Wählen die Optionen, die Sie unterstützen möchten werden die erforderlichen Eigenschaften Ihrer Anwendung hinzugefügt werden.

 [![](property-lists-images/maps.png "Kartenintegration")](property-lists-images/maps.png#lightbox)

Weitere Informationen zum Arbeiten mit Karten finden Sie in der Xamarin [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) Guide.

 [![](property-lists-images/bging.png "Hintergrundmodi")](property-lists-images/bging.png#lightbox)

Weitere Informationen zu Background Modes, finden Sie in der Xamarin [Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) Guide.

## <a name="advanced-panel"></a>Erweiterte Bereich

Der erweiterte Bereich steuert die Dokumenttypen und URL-Schemas, die die Anwendung unterstützt.

 [![](property-lists-images/image34.png "Erweiterte Bereich")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Dokumenttypen

Für Anwendungen, die Unterstützung für das Öffnen bestimmter Dateien iOS bietet die `CFBundleDocumentTypes` Schlüssel. Wenn wir unsere Anwendung bestimmte bekannte Dateitypen – z. B. PDF-Dateien – unterstützen soll würden wir den PDF-Wert des Schlüssels hinzufügen. Dieser Abschnitt enthält eine einfache Möglichkeit, die Daten einzugeben, die in gespeichert werden, die `CFBundleDocumentTypes` -Schlüssel in der `Info.plist` Datei.

Finden Sie in der Dokumentation unter [die Typen Ihrer App unterstützt Registrierung](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) ausführliche Anleitungen, um diese Werte zu konfigurieren.

## <a name="utis"></a>UTIs

Zuweilen muss eine Anwendung, öffnen einen benutzerdefinierten Dateityp unterstützt. Wir möchten z. B. Bilddateien mit der eine benutzerdefinierte Erweiterung öffnen *.xam*. Um einen benutzerdefinierten Dateityp anzugeben, erstellen wir eine benutzerdefinierte UTI - universellen Typ-ID - mithilfe der `UIExportedTypeDeclarations` Schlüssel. Der folgende Screenshot zeigt, wie Sie eine benutzerdefinierte UTI für die Erweiterung .xam zu erstellen:

 [![](property-lists-images/uti.png "UTIs-Editor")](property-lists-images/uti.png#lightbox)

Geben Sie nur als exportierter Typ-UTIs benutzerdefinierte UTIs, die spezifisch für Ihre app, die *importierte Typ-UTIs* ( `UIImportedTypeDeclarations` Schlüssel) angeben von benutzerdefinierten Typen unterstützt, aber nicht im Besitz Ihrer Anwendung.

Weitere Informationen zur Verwendung von benutzerdefinierten UTIs finden Sie im Apple- [Registrieren von Typen Ihrer App unterstützt](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) Guide.

## <a name="custom-urls"></a>Benutzerdefinierte URLs

Ein URL-Schema-Name (auch als "Protokoll" bezeichnet) ist der erste Teil der URL. Z. B. `http://` und `https://` sind allgemeine URL-Schemas. Sie haben die Möglichkeit zum Erstellen eines benutzerdefinierten URL-Schemas für Ihre Anwendung. Benutzerdefinierte URL-Schemas dienen zum kommunizieren und Senden von Daten mit anderen Anwendungen hin und her. Der folgende Screenshot veranschaulicht die Erstellung eines neuen benutzerdefinierten URL-Schemas, die aufgerufen `monkeys://`:

 [![](property-lists-images/url.png "Benutzerdefinierte URLs")](property-lists-images/url.png#lightbox)



Weitere Informationen zum Implementieren von benutzerdefinierten URL-Schemas finden Sie im Apple- [Implementieren von benutzerdefinierten URL-Schemas im Abschnitt dieses Handbuchs](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Bereich "Quelle"

Die **Quelle** Registerkarte die `Info.plist` -Datei können Sie benutzerdefinierte Werte hinzugefügt oder bearbeitet werden soll. Visual Studio für Mac bietet eine Liste der am häufigsten verwendeten Eigenschaften:

 [![](property-lists-images/image31.png "Hinzufügen einer neuen Eigenschaft in einer Dropdownliste")](property-lists-images/image31.png#lightbox)

Für bekannte Eigenschaften Visual Studio für Mac wird eine Liste der gültigen Werte wie im folgenden Screenshot dargestellt:

 [![](property-lists-images/image32.png "Wählen Sie einen Wert aus einer Liste der bekannten Wert")](property-lists-images/image32.png#lightbox)

Visual Studio für Mac erkennt auch den Typ der Eigenschaft, wie gezeigt:

 [![](property-lists-images/image33.png "Die verfügbaren Eigenschaftentypen")](property-lists-images/image33.png#lightbox)

Überprüfen Sie die Apple [verwandte App-Ressourcen](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) Links, um weitere Informationen zu optionalen Eigenschaften.

 <a name="Entitlements" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, mit den grafischen und erweiterte plist-Editoren zum Bearbeiten von auch häufig verwendete app-Konfigurationen, geben Sie die Symbole und startbilder. Sie außerdem die `Entitlements.plist` für das Hinzufügen und Verwalten von app-Funktionen.


## <a name="related-links"></a>Verwandte Links

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [App-Ressourcen](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Registrieren die Datei Typen Ihrer App unterstützt](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Implementieren von benutzerdefinierten URL-Schemas](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Referenz zu Überwachungsprotokollformaten Asset-Katalog](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
