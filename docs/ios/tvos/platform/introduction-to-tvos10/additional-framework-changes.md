---
title: Zusätzliche tvos. außerdem wurden 10 Frameworks Änderungen
description: Dieses Dokument beschreibt kleinere Änderungen und Verbesserungen an vorhandenen Frameworks in iOS 10 vorgenommen wurden. Es wird erläutert, Updates für AVFoundation, AVKit, Core Daten, Core Grafiken, Foundation, GameKit, GameplayKit und vieles mehr.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 326fb6a23048ba3d3e1d33f42c8da2fff8544c25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788874"
---
# <a name="additional-tvos-10-frameworks-changes"></a>Zusätzliche tvos. außerdem wurden 10 Frameworks Änderungen

Zusätzlich zu den wichtigsten Änderungen zu tvos. außerdem wurden machte Apple Änderungen und Verbesserungen für mehrere vorhandene Frameworks in tvos. außerdem wurden 10.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation Framework Ergänzungen

Das AVFoundation-Framework beinhaltet die folgenden Verbesserungen:

 - In tvos. außerdem wurden 10, die app nicht mehr implementiert verschiedene [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) Verhaltensweisen auf der Grundlage von Inhaltstyps. Legen Sie einfach die `Rate` Eigenschaft und AVFoundation wird bestimmt, wann genügend Inhalt verfügbar ist, für die Wiedergabe ohne führte ist.
 - Die neue `AVPlayerLooper` Klasse erleichtert es, einen bestimmten Medientyp während der Wiedergabe einer Schleife.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit Framework-Erweiterungen

Das AVKit-Framework beinhaltet die folgenden Verbesserungen:

 - Die app verfügt jetzt über die Steuerung des Verhaltens wird ausgelassen, der die [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) damit eine Geste wird übersprungen zum nächsten Element in der Wiedergabeliste oder erweiterte innerhalb des aktuellen Elements verschoben werden kann.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Core-Daten-Erweiterungen

tvos. außerdem wurden 10 umfasst die folgenden Erweiterungen für das Framework Core-Daten:

 - Stamm [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte unterstützt gleichzeitige zugesteht und ohne Serialisierung abrufen.
 - Die [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) Klasse verwaltet einen Pool von SQLite-Datenspeichern.
 - Die [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte mit SQLite-Datenspeicher in die WAL Journal Modus Unterstützung der neuen Abfrage Generation Funktion, auf dem verwalteten Objekt Kontexten (MOC) auf bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen.
 - Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und andere Ressourcen Core Daten.
 - Mehrere neue Hilfsmethoden wurden hinzugefügt, um `NSManagedObject` einfacher zu Abrufvorgängen auszuführen und Unterklassen zu erstellen.

Weitere Informationen finden Sie in der Apple- [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Core-Grafiken-Erweiterungen

tvos. außerdem wurden 10 umfasst die folgenden Verbesserungen Framework Graphics Core:

 - Die neue [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) Klasse kann verwendet werden, um eine Reihe von Farbe Konvertierungen ausführen.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Core-Image-Erweiterungen

tvos. außerdem wurden 10 macht das Framework Core-Image die folgenden Erweiterungen:

 - Die `ImageWithExtent` Methode der [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) Klasse kann verwendet werden, um benutzerdefinierte Verarbeitung in der Filtervorgang einzufügen. Core-Image wird Ruft den angegebenen Rückruf zwischen filtern, wenn ein Bild für die Ausgabe zu verarbeiten oder anzuzeigen.
 - Die app kann jetzt Bilder in einem Farbraum außerhalb des Kontexts die Core-Image Arbeitsfarbraum verarbeiten, durch Konvertieren in und aus den Farbraum vor und nach der Verarbeitung.
 - Wurden mehrere Rendering leistungsverbesserungen vorgenommen, um `UIImage` rendern (wenn es sich um eine von Core-Image Bild speichert gesicherte) in `UIImageView` Objekte. 
 - `UIImage` Objekte mit Tags Wide-Farbskala rendert als Breite Farbskala Farbe im `UIImageView` Objekte auf iOS-Geräten, die Breite Farbskala unterstützen.
 - Core-Image Kernelcode kann jetzt bestimmtes Pixel Ausgabeformate anfordern.

Darüber hinaus wurden die folgenden neuen Core-Image-Filter hinzugefügt:

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation-Erweiterungen

Foundation-Framework für tvos. außerdem wurden 10 wurden die folgenden Verbesserungen vorgenommen:

 - Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse, um das Datum und Uhrzeit Intervall-Berechnungen, z. B. Dauer, für den Vergleich von Intervallen und Tests für Intervall Schnittmengen vornehmen.
 - Mehrere neue Eigenschaften hinzugefügt wurden die [NSLocal](https://developer.apple.com/reference/foundation/nslocale) Klasse, um lokale Daten und die verfügbaren Anzeigeformate abzurufen.
 - Verwenden Sie die neue [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) -Klasse zum Konvertieren zwischen verschiedenen Einheiten des Measure (Maßeinheit) oder Berechnungen auf den Werten in anderen UOMs.
 - Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) Klasse, um lokalisierte Messungen für die Anzeige für den Endbenutzer zu formatieren.
 - Verwenden Sie die neue [NSUnit](https://developer.apple.com/reference/foundation/nsunit) und [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) Klassen zum Darstellen von bestimmten UOMs.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das GameKit-Framework in tvos. außerdem wurden 10 vorgenommen:

 - Ein neues nur iCloud-Konto des Typs von implementiert wurde die [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) Klasse.
 - Die neue [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) Klasse bietet eine allgemeine Lösung zum Verwalten von persistenten Datenspeicher Game Center. `GKGameSession` verwaltet eine Liste der Spieler und die app ist verantwortlich Formular implementieren wie und wann Teilnehmer Datum gespeichert wird, abgerufen oder zwischen den Beteiligten ausgetauscht. In vielen Fällen können Spiel Sitzungen vorhandenen aktivieren basierende Übereinstimmungen, Übereinstimmungen in Echtzeit oder persistenten Spiel speichern Methoden ersetzen.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das GameplayKit-Framework in tvos. außerdem wurden 10 vorgenommen:

 - Prozedurale Rauschen Generierung wurde hinzugefügt und kann verwendet werden, um realistische in natürlicher aussehende Texturen verbessern Kamera Bewegungen realistische hinzu, und Hilfe spielwelten zu generieren.
 - Die für die effiziente Suche Spiels partitioniert räumliche Partitionierung verwendet.
 - Eine neue Monte Carlo-Strategie ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) für eine vollständige möglich verschieben Berechnung hinzugefügt wurde.
 - Eine neue Decision Tree-API wurde hinzugefügt ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) und [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) um das Spiel-Erstellung AI zu verbessern.
 - 3D-Unterstützung wurde auf vorhandenen Agent und Pfad suchen mit dem neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) Klassen.
 - Verwenden Sie die neue [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) Klasse, um leistungsstarke, natürlichen aussehende Pfade bereitzustellen.
 - Die neue [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) und [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Klassen stellen Kombinieren von GameplayKit und SpriteKit einfacher als je zuvor.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>-Metal-Erweiterungen

-Metal-Framework in tvos. außerdem wurden 10 wurden die folgenden Verbesserungen vorgenommen:

 - 3D apps und Spiele können jetzt _Mosaik_ komplexe Szenen und Geometrie über die GPU effizientes Rendering.
 - Verwenden Sie zum Erstellen einer Sammlung stark optimiert, Material und hell Kombination aus Funktionen für eine Szene Spezialisierung-Funktion.
 - Geben Sie eine präzisere Kontrolle der Ressourcenzuordnung zum Optimieren der Leistung von Metall auf Grundlage der Ressource Heaps mit apps und Memoryless Rendern Ziele.

Weitere Informationen finden Sie unter der Apple- [Metall Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>-Metal-Shader Leistungsverbesserungen

Im folgenden aufgeführten Erweiterungen wurden auf das Metall Leistung Shader-Framework in tvos. außerdem wurden 10 vorgenommen:

 - Viele neue Kernels wurde um Metall Leistung Shader-Framework, damit die app-gekapselt, datenparallele Berechnungen z. B. Farbe Speicherplatz Konvertierungen und neural Network-Operationen nutzen kann.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das ModelIO-Framework in tvos. außerdem wurden 10 vorgenommen:

 - Das Dateiformat USD wird jetzt unterstützt.
 - Verwenden Sie die neue `MDLMaterialPropertyGraph` Klasse zur Laufzeit problemlos Unterstützung für Modelle ändert.
 - Signiert Abstand Feld Unterstützung, um hinzugefügt wurde die [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) Klasse.
 - Verwenden Sie die neue `MDLLightProbeIrradianceDataSource` Klasse, um bei der Platzierung Licht Prüfpunkt zu unterstützen.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das SceneKit-Framework in tvos. außerdem wurden 10 vorgenommen:

 - SceneKit enthält jetzt ein neue physisch basierend rendern (PBR)-System für realistischere Ergebnisse bei der einfacheren Asset authoring.
 - Verwenden Sie die neue [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) Schattierung modellieren Produkt eine Breite Palette von realistische Schattierung Auswirkungen bei nur drei grundlegende Eigenschaften (`Diffuse`, `Metalness` und `Roughness`).
 - Seit PBR Schattierung funktioniert am besten mit einer Umgebung basierende Beleuchtung, verwenden die `LightingEnvironment` Eigenschaft abbildbasierte Beleuchtung zum gesamten Szene tan zuweisen.
 - Verwenden der `IESProfileURL` Eigenschaft zum Importieren von realen Leuchten, die Beleuchtung Basis auf realen Werte wie z. B. Intensität (in Lumen) definieren und die Farbe für Temperatur (in Grad Kelvin).
 - Die [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) Klasse kann größer realistische mit HDR-Funktionen und Effekte bereitstellen. Verwenden Sie adaptive Offenlegung zum Erstellen von automatischen Effekte oder Vignettierung verwenden, Farbsäume und Farbe Einteilung, um das Spiel filmatic Effekte hinzuzufügen.
 - PBR und HDR Kamera bieten bessere Ergebnisse als herkömmliche-Rendering-Techniken und folglich SceneKit jetzt führt alle Farbe Berechnungen in einer linearen Farbraum (mit Farbskala P3 auf geräteanzeigen Wide-Farbe).
 - SceneKit jetzt Farbe alle Farben entspricht, indem Sie die Farbe Profilinformationen zu lesen.
 - SceneKit interpretiert Komponente Farbwerte in einer linearen RGB-Farbspektrum für alle Shader-Typen.
 - Da SceneKit liest und für Farbe-Profilinformationen in Textur Bildern anpassen, verwenden Sie Asset Kataloge für alle Abbilder, um sicherzustellen, dass diese Informationen bereitgestellt wird.
 - Beide lineare Speicherplatz Rendering-Farbe aus, und Wide-Color kann deaktiviert werden, indem Sie angeben der `SCNDisableLinearSpaceRendering` und `SCNDisableWideGamut` Schlüssel in der app `Info.plist`.
 - Erstellen Sie beliebige Polygon Primaten (entweder aus Dateien geladen oder programmgesteuert generiert) mit dem neuen Geometry an [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) Klasse.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das SpriteKit-Framework in tvos. außerdem wurden 10 vorgenommen:

 - Tilemaps unterstützt nun die Kachel "quadratisch, Sechseckige und isometrische" Formen 2D, 2,5 D und Durchführen eines Bildlaufs Seite Spiele, die mit der `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet` Klassen.
 - Verwenden Sie die neue `SKWarpGeometry` Klasse gestreckt oder im Simulator beeinträchtigt werden [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) oder [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendern. Die neue [SKAction](https://developer.apple.com/reference/spritekit/skaction) Klasse zum Animieren der Übergänge zwischen Warp Effekte verwendet werden kann.
 - Benutzerdefinierte Shadern können Attribute bereitstellen (`SKAttribute`), kann von jedem Knoten, die den Shader durch Angabe eines Attributwerts verwendet separat konfiguriert werden (`SKAttributeValue`).
 - Die [SKView](https://developer.apple.com/reference/spritekit/skview) -Klasse stellt mehrere neue Methoden, um eine präzisere steuern, wann und wie eine Szene gerendert wird.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das UIKit-Framework in tvos. außerdem wurden 10 vorgenommen:

 - Der Fokus API wurde verbessert, um den Schwerpunkt Element nicht anzeigen, darüber hinaus unterstützen `UIViews`. Elemente, die Fokus unterstützen _müssen_ implementieren die `IUIFocusItem` Schnittstelle.
 - Die neue `UIGraphicsRender` Klasse stellt eine objektorientierte Methode zum Erstellen von Bitmaps oder PDF-Dateien aus UIKit rendern oder Core-Grafiken und ersetzt die veraltete `UIGraphicsBeginImageContext` Methode.
 - Die `UIUserInterfaceStyle` Klasse wurde hinzugefügt, um zu bestimmen, welche User Interface Design (dunkle oder helle) derzeit aktiv ist.
 - Neue vollständig interaktiven, objektbasierte und parallelisierbar Animation-Unterstützung hinzugefügt wurde und van mit Gesten verknüpft werden. Pleas Siehe Apple des [UIViewAnimating Protokollreferenz](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator-Klassenreferenz](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protokollreferenz](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters-Klassenreferenz](https://developer.apple.com/reference/uikit/uicubictimingparameters) und [UISpringTimingParameter-Klassenreferenz](https://developer.apple.com/reference/uikit/uispringtimingparameters) für Weitere Informationen.
 - Die neue `UIPreviewInteraction` und `UIPreviewInteractionDelegate` kann die app app Peek / "und" Pop-Vorgänge eine benutzerdefinierte Schnittstelle bereit.
 - Die neue `UIAccessibilityCustomRotor` Klasse ermöglicht der app, die benutzerdefinierte, kontextspezifische Funktionalität für hilfstechnologien wie Voice Over bereitstellen.
 - Verwenden der `UIAccessibilityIsAssistiveTouchRunning` und `UIAccessibilityAssistiveTouchStatusDidChangeNotification` Symbole, um festzustellen, ob AssistiveTouch aktiviert ist.
 - Verwenden der `UIAccessibilityHearingDevicePairedEar` und `UIAccessibilityHearingDevicePairedEarDidChangeNotification` Symbole zum Abrufen des Status eines beliebigen gekoppelt Konfiguration hörgeschädigt sind Hilfsmittel.
 - Die neue [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) -API bietet neue Optionen (z. B. Einschränkungen für Lebensdauer) und wird automatisch kompatible Inhaltstypen für allgemeine Klassentypen deklariert werden.
 - Zur Unterstützung von dynamischen Typ in Bezeichnungen Textfelder und Textfelder verwenden Sie die neue `PreferredFontForTextStyle` Methode der `UIFont` Klasse.
 - Entscheiden, ob ein Element Schriftart aktualisieren sollten bei der die Geräte `UIContentSizeCategory` ändert, verwenden die `AdjustsFontForContentSizeCategory` Eigenschaft von der `UIContentSizeCategoryAdjusting` delegieren.
 - Die Darstellung von das Signal für Leiste Registerkartenelemente z. B. Text-und Hintergrundfarbe kann von die app nun steuern.
 - Die Aktualisierungssteuerung jetzt unterstützt, in allen Scroll und Scroll Ansicht Unterklassen (z. B. `UICollectionView`).
 - Die `OpenURL` Methode der `UIApplication` Klasse heißt asynchron unterstützt jetzt ein Abschluss-Handler, der aufgerufen wird, nachdem die offene abgeschlossen wurde.
 - Initiieren Sie die CloudKit freigeben und ändern Sie seine Eigenschaften mit dem neuen `UICloudSharingController` und `UICloudSharingControllerDelegate` Klassen.
 - Nutzen von Prefetch Zellen zur Verbesserung der Bildlauf der `UICollectionViews` mit dem neuen `UICollectionViewDataSourcePrefetching` delegieren.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Was ist neu in tvos. außerdem wurden 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
