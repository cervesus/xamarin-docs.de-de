---
title: Weitere tvos 10-Frameworks-Änderungen
description: In diesem Dokument werden kleinere Änderungen und Verbesserungen an vorhandenen Frameworks in ios 10 beschrieben. Es werden Updates zu AVFoundation, avkit, Core Data, Core graphics, Foundation, GameKit, gameplaykit und mehr erläutert.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 230da58bba68b9411b67baacd53b534ae832510d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657435"
---
# <a name="additional-tvos-10-frameworks-changes"></a>Weitere tvos 10-Frameworks-Änderungen

Zusätzlich zu den wichtigsten Änderungen an tvos hat Apple Änderungen und Verbesserungen an mehreren vorhandenen Frameworks in tvos 10 vorgenommen.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>Erweiterungen des AVFoundation-Frameworks

Das AVFoundation-Framework umfasst die folgenden Verbesserungen:

- In tvos 10 implementiert die APP nicht mehr unterschiedliche [avplayeritem](https://developer.apple.com/reference/avfoundation/avplayeritem) -Verhaltensweisen basierend auf dem Inhaltstyp. Legen Sie einfach `Rate` die-Eigenschaft fest, und AVFoundation bestimmt, wann genug Inhalt für die Wiedergabe verfügbar ist, ohne Sie zu warten.
- Die neue `AVPlayerLooper` -Klasse vereinfacht die Schleife eines bestimmten Mediums während der Wiedergabe.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>Erweiterungen des avkit-Frameworks

Das avkit-Framework umfasst die folgenden Verbesserungen:

- Die APP verfügt jetzt über die Kontrolle über das übersprungen Verhalten von [avplayerviewcontroller](https://developer.apple.com/reference/avkit/avplayerviewcontroller) , sodass eine übersprselungbewegung zum nächsten Element in der Wiedergabeliste wechseln oder innerhalb des aktuellen Elements fortschreiten kann.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Erweiterungen der Kern Daten

tvos 10 umfasst die folgenden Erweiterungen für das Core Data Framework:

- Stamm- [nsmanagedobjectcontext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte unterstützen das gleichzeitige fehlschlagen und Abrufen ohne Serialisierung.
- Die [nspersistentstorecoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) -Klasse verwaltet einen Pool mit SQLite-Daten speichern.
- Die [nsmanagedobjectcontext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte mit SQLite-Daten speichern im Wal-Journal Modus unterstützen die neue Abfrage Generierungs Funktion, bei der verwaltete Objekt Kontexte (Managed Object Context, MUC) an bestimmte Daten Bank Versionen angeheftet werden können, um zukünftige Abruf-und fehlerverarbeitungs Vorgänge durchzusetzen
- Verwenden Sie die allgemeine Ebene `NSPersistenceContainer` , um auf `NSPersistentStoreCoordinator`das, [nsmanagedobjectmodel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und andere Kern Daten Konfigurations Ressourcen zu verweisen.
- Es wurden mehrere neue Hilfsmethoden hinzugefügt `NSManagedObject` , um die Ausführung von Abrufe und das Erstellen von Unterklassen zu vereinfachen.

Weitere Informationen finden Sie in der [Core Data Framework-Referenz](https://developer.apple.com/reference/coredata)von Apple.

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Erweiterungen der Kern Grafik

tvos 10 umfasst die folgenden Verbesserungen am Kern Grafik Framework:

- Die neue [cgcolorconverterref](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) -Klasse kann verwendet werden, um eine Reihe von Farb Konvertierungen auszuführen.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Erweiterungen des Kern Images

tvos 10 nimmt die folgenden Verbesserungen am Core-Image-Framework vor:

- Die `ImageWithExtent` -Methode der [cifilter](https://developer.apple.com/reference/coreimage/cifilter) -Klasse kann verwendet werden, um eine benutzerdefinierte Verarbeitung in den Filter Vorgang einzufügen. Das Core-Image Ruft den gegebenen Rückruf zwischen Filtern auf, wenn ein Bild für die Ausgabe oder Anzeige verarbeitet wird.
- Die APP kann jetzt Bilder in einem Farbraum außerhalb des Arbeits Farbraums des Kern Bild Kontexts verarbeiten, indem Sie vor und nach der Verarbeitung in den und aus dem Farbraum in-und auslagern.
- Es wurden mehrere renderingleistungserweiterungen für `UIImage` das Rendern von `UIImageView` Objekten (bei der Unterstützung durch Image Speicher-Kern Images) 
- `UIImage`-Objekte, die als Wide-Gamut gekennzeichnet sind, werden in `UIImageView` Objekten auf IOS-Geräten, die Breite Farben unterstützen, als Breite Farbe dargestellt.
- Kernbild-Kernelcode kann jetzt bestimmte Pixel Ausgabeformate anfordern.

Außerdem wurden die folgenden neuen Kern Bild Filter hinzugefügt:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation-Erweiterungen

Die folgenden Verbesserungen wurden an Foundation Framework für tvos 10 vorgenommen:

- Verwenden Sie die neue [nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval) -Klasse, um Datums-und Zeitintervall Berechnungen zu erstellen, z. b. Dauer Zeiten, zum Vergleichen von Intervallen und zum Testen von Intervall interabschnitten.
- Der [nslocal](https://developer.apple.com/reference/foundation/nslocale) -Klasse wurden mehrere neue Eigenschaften hinzugefügt, um lokale Informationen und die verfügbaren Anzeige Formate abzurufen.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)-Klasse, um zwischen verschiedenen Maßeinheiten (Unit of Measure, UOM) zu konvertieren oder Berechnungen für Werte in verschiedenen UOMS auszuführen.
- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)-Klasse, um lokalisierte Messungen zu formatieren, die dem Endbenutzer angezeigt werden sollen.
- Verwenden Sie die neuen [nsunit](https://developer.apple.com/reference/foundation/nsunit) -und [nsdimension](https://developer.apple.com/reference/foundation/nsdimension) -Klassen, um bestimmte UOMS darzustellen.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit-Verbesserungen

Die folgenden Verbesserungen wurden an dem GameKit-Framework in tvos 10 vorgenommen:

- Ein neuer icloud-Kontotyp wurde von der [gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) -Klasse implementiert.
- Die neue Klasse " [gkgamesession](https://developer.apple.com/reference/gamekit/gkgamesession) " stellt eine generalisierte Lösung für die Verwaltung von persistenten Daten speichern auf Game Center bereit. `GKGameSession`verwaltet eine Liste von Playern, und die APP ist dafür verantwortlich, wie und wann das Teilnehmer Datum gespeichert, abgerufen oder zwischen den Playern ausgetauscht wird. In vielen Fällen können Spielsitzungen vorhandene Turn-basierte Übereinstimmungen, Echt Zeit Übereinstimmungen oder persistente Spiel Speichermethoden ersetzen.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>Gameplaykit-Erweiterungen

Die folgenden Verbesserungen wurden an dem gameplaykit-Framework in tvos 10 vorgenommen:

- Das Generieren von Prozeduren wurde hinzugefügt und kann verwendet werden, um den Realismus in Natur orientierten Texturen zu verbessern, den Bewegungsmöglichkeiten der Kamera zu verleihen und umfassende Spielwelten zu generieren.
- Verwenden Sie räumliche Partitionierung, um die spielworld-Daten für eine effiziente Suche zu partitionieren
- Für eine vollständige Verschiebungs Berechnung wurde ein neuer Monte Carlo-Stratege ("[gkmontecarlostrateg;](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)") hinzugefügt.
- Es wurde eine neue Decision Tree-API hinzugefügt ("[gkdecisiontree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) " und " [gkdecisionnode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)"), um die KI der Spiel Bildung zu verbessern.
- die 3D-Unterstützung wurde vorhandenen Agent-und Pfad Suchverhalten mithilfe der neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) -und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) -Klassen hinzugefügt.
- Verwenden Sie die neue Klasse " [gkmeshgraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) ", um Hochleistungs Pfade mit hoher Leistung bereitzustellen.
- Die neuen Klassen " [gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) " und " [gksknodecomponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) " vereinfachen das Kombinieren von gameplaykit und spritekit.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Metal-Erweiterungen

Die folgenden Verbesserungen wurden am Metal-Framework in tvos 10 vorgenommen:

- 3D-apps und-Spiele können jetzt _Mosaik verwendet werden_ , um komplexe Szenen und Geometrie effizient über die GPU zu erzeugen.
- Verwenden Sie die Funktions Spezialisierung, um eine hochgradig optimierte Sammlung von Material-und Light-Kombinations Funktionen für eine Szene zu erstellen.
- Sorgen Sie für eine differenzierte Steuerung der Ressourcen Zuordnung, um die Leistung von Metal-basierten Apps mithilfe von Ressourcen Heaps und Speicher losen Renderingzielen zu optimieren.

Weitere Informationen finden Sie im [Handbuch zur Metal-Programmierung](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)von Apple.

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Verbesserungen der Metal Performance Shaders

Die folgenden Verbesserungen wurden am Metal Performance Shaders-Framework in tvos 10 vorgenommen:

- Dem Framework "Metal Performance Shaders" wurden viele neue Kernel hinzugefügt, damit die APP hochoptimierte, Daten parallele Berechnungen, wie z. b. Farb Raum Konvertierungen und neuronale Netzwerk Vorgänge, nutzen kann.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>Verbesserungen bei der Verbesserung

Die folgenden Verbesserungen wurden am "mudelio"-Framework in tvos 10 vorgenommen:

- Das Dateiformat "USD" wird jetzt unterstützt.
- Verwenden Sie die `MDLMaterialPropertyGraph` neue-Klasse, um Lauf Zeit Änderungen von Modellen problemlos zu unterstützen.
- Die Unterstützung für unterstützte Entfernungs Felder wurde der [mdlvoxelarray](https://developer.apple.com/reference/modelio/mdlvoxelarray) -Klasse hinzugefügt.
- Verwenden Sie die `MDLLightProbeIrradianceDataSource` neue-Klasse, um bei der Platzierung von Licht zu helfen.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>Scenekit-Erweiterungen

Die folgenden Verbesserungen wurden an dem scenekit-Framework in tvos 10 vorgenommen:

- Scenekit enthält jetzt ein neues physisch basiertes Rendering (PBR)-System für realistischere Ergebnisse mit einfacherer Erstellung von Medienobjekten.
- Verwenden Sie das neue [scnlightingmodelphysicallybased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) -Schattierungs Modell, um eine breite Palette realistischer Schattierungs Effekte zu erzeugen, während nur drei`Diffuse`Grund `Metalness` Legende `Roughness`Eigenschaften (und) benötigt werden.
- Da die PBR-Schattierung am besten mit der Umgebungs basierten Beleuchtung funktioniert `LightingEnvironment` , verwenden Sie die-Eigenschaft, um die bildbasierte Beleuchtung der Tan gesamten Szene zuzuweisen.
- Verwenden Sie `IESProfileURL` die-Eigenschaft, um einfache Beleuchtungsanlagen zu importieren, die Beleuchtungs Basis auf realen Werten wie Intensität (in Lumens) und Farbtemperatur (in Grad Kelvin) definieren.
- Mit den HDR-Features und-Effekten kann die [scncamera](https://developer.apple.com/reference/scenekit/scncamera) -Klasse einen besseren Realismus bereitstellen. Verwenden Sie Adaptive verfügbar machung, um automatische Effekte zu erstellen, oder verwenden Sie die Werte für die Durchsetzung von Farben, die Farbränder und die farbeinstufung, um dem Spiel filtersche
- Sowohl PBR-als auch HDR-Kamera Features bieten bessere Ergebnisse als herkömmliche Renderingverfahren. Daher führt scenekit jetzt alle Farb Berechnungen in einem linearen Farbraum durch (mit P3-Farbpalette auf breit farbigen Geräte anzeigen).
- Scenekit ist nun mit allen Farben übereinstimmen, indem die Farbprofil Informationen gelesen werden.
- Scenekit interpretiert Farbkomponenten Werte in einem linearen RGB-Farbraum für alle shadertypen.
- Da scenekit Farbprofil Informationen in Textur Bildern liest und anpasst, verwenden Sie für alle Images Asset-Kataloge, um sicherzustellen, dass diese Informationen bereitgestellt werden.
- Sowohl lineares Farb Raum Rendering als auch breit Farben können deaktiviert werden, indem `SCNDisableLinearSpaceRendering` die `SCNDisableWideGamut` `Info.plist`Schlüssel und in der der APP angegeben werden.
- Erstellen Sie beliebige Polygon-Prime (entweder aus Dateien geladen oder Programm gesteuert generiert), um die Geometrie mit der neuen [scngeometryprimitivetyetpolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) -Klasse anzugeben.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>Spritekit-Erweiterungen

Die folgenden Verbesserungen wurden am spritekit-Framework in tvos 10 vorgenommen:

- Tilemaps unterstützt jetzt quadratische, hexagonale und isometrische Kachel Formen für 2D-, 2.5 d-und Side- `SKTileMapMode`Scroll- `SKTileGroupRule` Spiele `SKTileSet` mithilfe der Klassen, `SKTileGroup`und.
- Verwenden Sie die `SKWarpGeometry` neue-Klasse, um das Rendern von [skspritenode](https://developer.apple.com/reference/spritekit/skspritenode) oder [schrägerectnode](https://developer.apple.com/reference/spritekit/skeffectnode) zu erweitern oder zu verzerren. Die neue [skaction](https://developer.apple.com/reference/spritekit/skaction) -Klasse kann verwendet werden, um Übergänge zwischen Warp-Effekten zu animieren.
- Benutzerdefinierte Shader können Attribute (`SKAttribute`) bereitstellen, die separat von jedem Knoten konfiguriert werden können, der den Shader verwendet, indem`SKAttributeValue`ein Attribut Wert () bereitgestellt wird.
- Die [skview](https://developer.apple.com/reference/spritekit/skview) -Klasse stellt mehrere neue Methoden bereit, mit denen Sie präzise steuern können, wann und wie eine Szene gerendert wird.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit-Verbesserungen

Die folgenden Verbesserungen wurden an dem UIKit-Framework in tvos 10 vorgenommen:

- Die Fokus-API wurde erweitert, um den Fokus des nicht Sicht Elements zusätzlich zu zu `UIViews`unterstützen. Elemente, die den Fokus unterstützen `IUIFocusItem` , müssen die Schnittstelle implementieren.
- Die neue `UIGraphicsRender` -Klasse stellt eine objektorientierte Methode zum Erstellen von Bitmaps oder PDF-Code aus dem UIKit-Rendering oder Kern Grafiken `UIGraphicsBeginImageContext` bereit und ersetzt die veraltete-Methode.
- Die `UIUserInterfaceStyle` -Klasse wurde hinzugefügt, um zu bestimmen, welches Design der Benutzeroberfläche (dunkel oder hell) derzeit aktiv ist.
- Neue, vollständig interaktive, objektbasierte und unter brechbare Animations Unterstützung wurde hinzugefügt, und van ist mit Gesten verknüpft. Weitere Informationen finden Sie in der [uiviewanimating-Protokoll Referenz](https://developer.apple.com/reference/uikit/uiviewanimating)von Apple, der [uiviewpropertyanimator-Klassenreferenz](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), der uitimingcurrveprovider- [Protokoll Referenz](https://developer.apple.com/reference/uikit/uitimingcurveprovider), der [uicubictimingparameters-Klassenreferenz](https://developer.apple.com/reference/uikit/uicubictimingparameters) und [ Uispringtimingparameter-Klassenreferenz](https://developer.apple.com/reference/uikit/uispringtimingparameters) für weitere Informationen.
- Der neue `UIPreviewInteraction` und `UIPreviewInteractionDelegate` ermöglicht der APP, eine benutzerdefinierte Schnittstelle für Peek-und Pop-Vorgänge bereitzustellen.
- Die neue `UIAccessibilityCustomRotor` Klasse ermöglicht der APP, benutzerdefinierte kontextspezifische Funktionen für Hilfstechnologien wie Voice over bereitzustellen.
- Verwenden Sie `UIAccessibilityIsAssistiveTouchRunning` die `UIAccessibilityAssistiveTouchStatusDidChangeNotification` Symbole und, um zu bestimmen, ob assistivetouch aktiviert ist.
- Verwenden Sie `UIAccessibilityHearingDevicePairedEar` die `UIAccessibilityHearingDevicePairedEarDidChangeNotification` Symbole und, um den Status von gekoppelten MFI-Hörhilfen zu erhalten.
- Die neue [uipasteboard](https://developer.apple.com/reference/uikit/uipasteboard) -API bietet neue Optionen (z. b. Lebensdauer Einschränkungen) und deklariert automatisch kompatible Inhaltstypen für allgemeine Klassentypen.
- Zur Unterstützung des dynamischen Typs in Bezeichnungen verwenden Textfelder und Textfelder die `PreferredFontForTextStyle` neue-Methode `UIFont` der-Klasse.
- Um zu entscheiden, ob ein Element seine Schriftart aktualisieren soll, `UIContentSizeCategory` wenn sich die Geräte `AdjustsFontForContentSizeCategory` ändern, verwenden `UIContentSizeCategoryAdjusting` Sie die-Eigenschaft des-Delegaten.
- Die APP kann jetzt die Darstellung des Badge für Tabulator Elemente wie Text und Hintergrundfarbe steuern.
- Das Aktualisierungs Steuerelement in wird jetzt in allen Unterklassen der Bild Lauf Ansicht und der `UICollectionView`scrollansicht (z. b.) unterstützt
- Die `OpenURL` -Methode `UIApplication` der-Klasse wird asynchron aufgerufen und unterstützt jetzt einen Vervollständigungs Handler, der nach Abschluss des öffnenden aufgerufen wird.
- Initiieren der cloudkit-Freigabe und Ändern der Eigenschaften mithilfe `UICloudSharingController` der `UICloudSharingControllerDelegate` neuen-Klasse und der-Klasse.
- Profitieren Sie von vorab abgerufenen Zellen, um die scrolldarstellung von `UICollectionViews` mit dem neuen `UICollectionViewDataSourcePrefetching` Delegaten zu verbessern.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Neues in tvos 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
