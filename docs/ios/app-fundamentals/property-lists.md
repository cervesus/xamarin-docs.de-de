---
title: Arbeiten mit Eigenschaftenlisten
description: "Dieses Dokument stellt Visual Studio für Mac Computer grafischen und der erweiterten Eigenschaft Eigenschaftenliste (plist)-Editor für die Arbeit mit der Datei \"Info.plist\" und Entitlements.plist bereit. Es wird veranschaulicht, Festlegen von Symbolen und starten-Images für iOS-Anwendungen von innerhalb von Visual Studio für Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0d7b4c5a539470a3544d0117251f40fd6bd37f2b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-property-lists"></a>Arbeiten mit Eigenschaftenlisten

_Dieses Dokument stellt Visual Studio für Mac Computer grafischen und der erweiterten Eigenschaft Eigenschaftenliste (plist)-Editor für die Arbeit mit der Datei "Info.plist" und Entitlements.plist bereit. Es wird veranschaulicht, Festlegen von Symbolen und starten-Images für iOS-Anwendungen von innerhalb von Visual Studio für Mac._

Visual Studio für Mac verfügt über eine grafische plist-Editor, der erleichtert die Bearbeitung von app-Eigenschaften und Funktionen, die einfacher. Visual Studio für Mac verfügt über zwei .plists - `Info.plist` für app-Eigenschaften und Symbole, bearbeiten und `Entitlements.plist` für die Verwaltung von app-Funktionen. Dieses Handbuch stellt die Info.plists und bietet einen Überblick über die Arbeit mit ihnen in Visual Studio für Mac. Informationen zu Entitlements.plist, finden Sie unter der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Handbuch.

## <a name="infoplist"></a>Info.plist

Die Eigenschaftenliste Informationen ( `Info.plist`) ist eine erforderliche iOS-Datei, die Informationen zur Anwendungskonfiguration an das System bereitstellt. Visual Studio für Mac des benutzerdefinierten `Info.plist` Features im Editor links im Editor-Fenster auf drei Bereiche, die durch die Registerkarten am unteren gesteuert:

 [ ![](property-lists-images/tabs.png "Die Datei "Info.plist"-Editor-Registerkarten unten links im Editor-Fenster")](property-lists-images/tabs.png)

Jeder Bereich steuert verschiedene Eigenschaften, wie im folgenden erläutert:

-  **Feld "Anwendung"** -eine grafische Benutzeroberfläche zum legen Sie allgemeinen Eigenschaften der Anwendung als auch Symbole und starten Sie Bilder, geben Sie Zuordnungen Integration auch backgrounding Modi.
-  **Erweiterte Bereich** -im erweiterten Bereich bietet die Möglichkeit zum Angeben von unterstützten Dokumenttypen UTIs, und die URL-Typen.
-  **Datenquelle Bereich** -Bereich Quelle steuert ungewöhnlich Eigenschaften als auch benutzerdefinierte Eigenschaften für die Anwendung.


In den nächsten drei Abschnitten untersuchen Sie die Funktionen der einzelnen Bereichen im Detail.

## <a name="application-panel"></a>Feld "Anwendung"

Visual Studio für Mac bietet eine grafische Benutzeroberfläche zum Bearbeiten von allgemeinen `Info.plist` Einträge für eine Anwendung:

1.  Anwendungseigenschaften
1.  Unterstützten Gerätetypen
1.  Unterstützung Ausrichtungen für jeden Gerätetyp
1.  Art und Farbe der Statusleiste
1.  Symbole und Start Bildschirme
1.  Karten und Hintergrundmodi


Diese werden in den nächsten Abschnitten ausführlicher beschrieben.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS-Anwendung-Ziel

Dieser Abschnitt enthält wichtige Informationen, die die Anwendung beschreibt.
Die **Bezeichner** gespeicherten übereinstimmen hier die Paket-ID, die in iTunes Connect (für App-Store-apps) und auch in der Liste der iOS-Portal-App-IDs Bereitstellung und die Entwicklung und die Verteilung von Zertifikaten eingegeben werden.

 [ ![](property-lists-images/image24.png "iOS-Anwendung-Ziel")](property-lists-images/image24.png)

### <a name="device-deployment"></a>Gerätebereitstellung

 [ ![](property-lists-images/deployment.png "Gerätebereitstellung")](property-lists-images/deployment.png)

Das Gerät **Bereitstellung** Info Abschnitte werden selektiv angezeigt, abhängig von der Auswahl in der **Geräte** Dropdownliste in der **Anwendung Ziel** obigen Abschnitt. Die **Hauptbenutzeroberfläche** Dropdown-Menü festgelegt ist, um **MainStoryboard** in Storyboard datengesteuerten Anwendungen. Wenn die Benutzeroberfläche vollständig in Code geschrieben ist, und dies kann leer gelassen werden.

### <a name="supported-device-orientations"></a>Unterstützte Geräte Ausrichtungen

 **Unterstützte Geräte Ausrichtungen** steuert, wie die app auf geräterotation reagiert. Es ist üblich für iPhone/iPad-apps unterstützen nur **Hochformat**, oder alles, was aber **stehend**. Alle iPad-Anwendungen mit Ausnahme von Spiele sollte im Allgemeinen alle Ausrichtungen unterstützen.

### <a name="status-bar-styles"></a>Statusleiste-Stile

Die **Status Leiste Stile** Abschnitt ist eine grafische Benutzeroberfläche zum Bearbeiten einer Anwendungsverzeichnis `UIStatusBarStyle`:

 [ ![](property-lists-images/status.png "Statusleiste-Stile")](property-lists-images/status.png)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Symbole, Bilder zu starten und iTunes Bildmaterial

Informationen zum Verwenden von Symbole, Bilder und Grafiken in Ihrer Datei "Info.plist" finden Sie der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Handbuch.




### <a name="maps-integration-and-background-modes"></a>Maps-Integration und Hintergrundmodi

Die `Info.plist` enthält spezielle Abschnitte für die Maps-Integration und backgrounding Modi angeben. Auswählen der Optionen, die Sie unterstützen möchten wird die Anwendung für Sie die erforderlichen Eigenschaften hinzugefügt.

 [ ![](property-lists-images/maps.png "Maps-Integration")](property-lists-images/maps.png)

Weitere Informationen zum Arbeiten mit Karten finden Sie in der Xamarin [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) Handbuch.

 [ ![](property-lists-images/bging.png "Hintergrundmodi")](property-lists-images/bging.png)

Weitere Informationen zu Hintergrundmodi, finden Sie in der Xamarin [Backgrounding in iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) Handbuch.

## <a name="advanced-panel"></a>Erweiterte Bedienfeld

Im erweiterten Bereich steuert die Dokumenttypen und URL-Schemas, die von der Anwendung unterstützt.

 [ ![](property-lists-images/image34.png "Erweiterte Bedienfeld")](property-lists-images/image34.png)

 <a name="Document_Types" />


## <a name="document-types"></a>Dokumenttypen

Für Anwendungen, die unterstützt bestimmte Typen von Dateien zu öffnen, iOS bietet die `CFBundleDocumentTypes` Schlüssel. Wenn wir unsere Anwendung bestimmte bekannte Dateitypen – z. B. PDF - unterstützen soll, würden wir den PDF-Wert auf den Schlüssel hinzufügen. Dieser Abschnitt enthält eine einfache Möglichkeit, die Daten einzugeben, die in gespeichert werden, die `CFBundleDocumentTypes` -Schlüssel in der `Info.plist` Datei.

Finden Sie in der Dokumentation auf [Registrieren der Datei Typen Ihrer App unterstützt](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) Weitere Informationen zum Konfigurieren dieser Werte.

## <a name="utis"></a>UTIs

In einigen Fällen muss eine Anwendung unterstützen, öffnen einen benutzerdefinierten Dateityp. Wir möchten z. B. mit einer benutzerdefinierten Erweiterung Bilddateien öffnen *.xam*. Um einen benutzerdefinierten Dateityp angeben, wir Erstellen einer benutzerdefinierten UTI - Universal Typbezeichner - mit den `UIExportedTypeDeclarations` Schlüssel. Der folgende Screenshot zeigt, wie eine benutzerdefinierte UTI für die Erweiterung .xam erstellt wird:

 [ ![](property-lists-images/uti.png "UTIs-Editor")](property-lists-images/uti.png)

Nur als exportierte UTIs Geben Sie die benutzerdefinierte UTIs spezifisch für Ihre app die *importiert Typ UTIs* ( `UIImportedTypeDeclarations` Schlüssel) benutzerdefinierte Typen unterstützt, aber nicht im Besitz der Anwendungsstatus angeben.

Weitere Informationen finden Sie unter benutzerdefinierte UTIs, finden Sie in der Apple- [registrieren Datei Typen Ihrer App unterstützt](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) Handbuch.

## <a name="custom-urls"></a>Benutzerdefinierte URLs

Ein URL-Schema-Name (auch als "Protokoll" bezeichnet) ist der erste Teil der URL. Beispielsweise `http://` und `https://` sind allgemeine URL-Schemas. Sie haben die Möglichkeit zum Erstellen eines benutzerdefinierten URL-Schemas für Ihre Anwendung. Benutzerdefinierte URL-Schemas werden verwendet, um die Kommunikation und Senden von Daten mit anderen Anwendungen hin und her. Der folgende Screenshot veranschaulicht die Erstellung eines neuen benutzerdefinierten URL-Schemas aufgerufen `monkeys://`:

 [ ![](property-lists-images/url.png "Benutzerdefinierte URLs")](property-lists-images/url.png)



Weitere Informationen zum Implementieren von benutzerdefinierten URL-Schemas finden Sie in der Apple- [implementieren benutzerdefinierte URL-Schemas im Abschnitt dieses Handbuchs](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Source-Bereich

Die **Quelle** auf der Registerkarte die `Info.plist` Datei ermöglicht, benutzerdefinierte Werte hinzugefügt oder bearbeitet werden soll. Visual Studio für Mac enthält eine Liste der am häufigsten verwendeten Eigenschaften:

 [ ![](property-lists-images/image31.png "Hinzufügen einer neuen Eigenschaft in einer Dropdownliste")](property-lists-images/image31.png)

Für bekannte Eigenschaften Visual Studio für Mac wird eine Liste der gültigen Werte wie der folgende Screenshot veranschaulicht:

 [ ![](property-lists-images/image32.png "Wählen Sie einen Wert aus einer Liste der bekannten Wert")](property-lists-images/image32.png)

Visual Studio für Mac erkennt auch den Eigenschaftentyp wie gezeigt:

 [ ![](property-lists-images/image33.png "Die verfügbaren Eigenschaftentypen")](property-lists-images/image33.png)

Überprüfen Sie die Apple [App Verwandte Ressourcen](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) Links für Weitere Informationen zu optionalen Eigenschaften.

 <a name="Entitlements" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird veranschaulicht, mit den grafischen als auch erweiterte plist-Editoren bearbeiten sowie allgemeine app-Konfigurationen, geben Sie die Symbole und Bilder zu starten. Sie außerdem die `Entitlements.plist` zum Hinzufügen und Verwalten von app-Funktionen.


## <a name="related-links"></a>Verwandte Links

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [App-Ressourcen](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Registrieren die Datei Typen Ihrer App unterstützt](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Implementieren benutzerdefinierte URL-Schemas](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Informationen zu Asset-Kataloge](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
