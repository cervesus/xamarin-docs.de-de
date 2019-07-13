---
title: Zusätzliche TvOS 10 Frameworks Änderungen
description: Dieses Dokument beschreibt die kleinere Änderungen und Verbesserungen bei den vorhandenen Frameworks unter iOS 10. Es wird erläutert, Updates für AVFoundation AVKit, Kerndaten, Core Graphics, Foundation, GameKit, GameplayKit und vieles mehr.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: ab6236198d0a5826fc613d1f3839bafdb980d235
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865647"
---
# <a name="additional-tvos-10-frameworks-changes"></a>Zusätzliche TvOS 10 Frameworks Änderungen

Zusätzlich zu den wichtigsten Änderungen, tvos hat Apple Änderungen und Verbesserungen an mehrere vorhandene Frameworks in TvOS 10 gemacht.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation Framework-Erweiterungen

Das Framework AVFoundation umfasst folgende Erweiterungen:

- In TvOS 10 und die app nicht mehr implementiert verschiedene [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) Verhalten basierend auf Inhaltstyp. Legen Sie einfach die `Rate` -Eigenschaft und AVFoundation bestimmt Wenn genügend Inhalte ohne treten für die Wiedergabe verfügbar ist.
- Die neue `AVPlayerLooper` Klasse macht es einfacher, einen bestimmten Media während der Wiedergabe in einer Schleife.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit Framework-Erweiterungen

Das Framework AVKit umfasst folgende Erweiterungen:

- Die app verfügt jetzt über die Steuerung des Verhaltens übersprungen wird, der die [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) damit eine Geste für eine übersprungen wird zum nächsten Element in der Wiedergabeliste oder erweiterte innerhalb des aktuellen Elements verschoben werden kann.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Core-Data-Erweiterungen

TvOS 10 umfasst folgende Erweiterungen für die Kerndaten-Framework:

- Stamm [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte unterstützt gleichzeitige Fehlerbehandlung und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) Klasse verwaltet einen Pool mit SQLite-Datenspeicher.
- Die [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte mit SQLite-Datenspeicher in die WAL-Journal-Modus-Unterstützung der neuen Abfrage Generation Funktion, in denen verwaltete Objekt Kontexten (MOC) für bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen an.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und anderen Konfigurationsressourcen Core Data.
- Mehrere neue Hilfsmethoden hinzugefügt wurden `NSManagedObject` abrufen und Erstellen von Unterklassen zu vereinfachen.

Weitere Informationen finden Sie unter Apple [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Wichtigste Grafik-Erweiterungen

TvOS 10 umfasst folgende Erweiterungen auf dem Kernframework für die Grafik:

- Die neue [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) Klasse kann verwendet werden, um eine Reihe von Farbe Konvertierungen durchführen.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Core-Image-Verbesserungen

TvOS 10 macht das Framework Core-Image die folgenden Erweiterungen:

- Die `ImageWithExtent` Methode der [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) Klasse kann verwendet werden, um die benutzerdefinierte Verarbeitung in der Filtervorgang einzufügen. Core-Image wird Ruft den angegebenen Rückruf zwischen filtern, wenn ein Bild für die Ausgabe verarbeitet oder angezeigt werden.
- Die app kann jetzt Bilder in einem Bereich der Farbe außerhalb des Kontexts der Core-Image arbeiten-Farbraum verarbeiten, durch die Konvertierung in und aus des Farbraums vor und nach der Verarbeitung.
- Verschiedene Verbesserungen der Rendering-Leistung wurden vorgenommen, um `UIImage` rendern (wenn von Core-Image-Image-Speicher unterstützt wird) in `UIImageView` Objekte. 
- `UIImage` Objekte mit Tags Wide-Farbskala rendert als Breitzeichen-Farbskala Farbe im `UIImageView` Objekte auf iOS-Geräten, die die Breite Farbskala unterstützen.
- Core-Image-Kernelcode kann jetzt bestimmte Ausgabe Pixelformate anfordern.

Darüber hinaus wurden die folgenden neuen Core-Image-Filter hinzugefügt:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation-Verbesserungen

Der Foundation-Framework für TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse um Datum und Uhrzeit-Intervall Berechnungen wie Dauer, für den Vergleich von Abständen, und für die Interval-Schnittpunkte zu testen.
- Mehrere neue Eigenschaften hinzugefügt wurden die [NSLocal](https://developer.apple.com/reference/foundation/nslocale) Klasse, um lokale Informationen und die verfügbaren Anzeigeformate abzurufen.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) -Klasse zum Konvertieren zwischen verschiedenen Einheiten von Measures (Maßeinheit), oder führen Berechnungen auf den Werten in verschiedenen UOMs.
- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) Klasse, um lokalisierte Messungen für die Anzeige für den Endbenutzer zu formatieren.
- Verwenden Sie die neue [NSUnit](https://developer.apple.com/reference/foundation/nsunit) und [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) Klassen zum Darstellen von bestimmten UOMs.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit-Verbesserungen

Das Framework GameKit in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Ein neues nur iCloud-Konto des Typs wurde implementiert, durch die [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) Klasse.
- Die neue [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) Klasse stellt eine allgemeine Lösung zum Verwalten von persistenten Datenspeicher auf der Game Center bereit. `GKGameSession` verwaltet eine Liste der Spieler und die app verantwortlich ist die Implementierung wie und wann Teilnehmer Datum gespeichert wird, abgerufen oder zwischen den Beteiligten ausgetauscht. In vielen Fällen können Spiele Sitzungen vorhandenen Turn-basierte Übereinstimmungen, die in Echtzeit Übereinstimmungen oder persistenten Spiel speichern Methoden ersetzen.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit-Verbesserungen

Das Framework GameplayKit in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Prozedurale Rauschen Generation hinzugefügt wurde und dienen zum verbessern die wirklichkeitsgetreuen Bilder in Texturen naturgetreue Bewegungen der Kamera wirklichkeitsgetreuen Bilder hinzu, und helfen anspruchsvoller spielwelten zu generieren.
- Verwenden Sie die räumliche Partitionierung, um die Welt, die Daten für die effiziente Suche zu partitionieren.
- Eine neue Monte-Carlo-Strategist ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) für die vollständige möglich verschieben Berechnung hinzugefügt wurde.
- Eine neue Decision Tree-API wurde hinzugefügt ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) und [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) um das Spiel zum Erstellen von AI zu verbessern.
- 3D-Unterstützung hinzugefügt wurde vorhandenen Agent "und" Pfad Auffinden von Verhaltensweisen, die mit dem neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) Klassen.
- Verwenden Sie die neue [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) Klasse, um leistungsstarke, naturgetreue Pfade bereitzustellen.
- Die neue [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) und [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Klassen stellen GameplayKit und einfacher als je zuvor SpriteKit kombiniert.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Metal-Verbesserungen

Das-Metal-Framework in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- 3D-Apps und spielen können Sie jetzt _Mosaik_ komplexe Szenen und "Geometry" über die GPU effizientes Rendering.
- Verwenden Sie zum Erstellen einer hochgradig optimierten Sammlung von Material und Licht Kombination-Funktionen für eine Szene Funktion Spezialisierung.
- Geben Sie, dass eine präzisere Kontrolle ressourcenzuteilung zum Optimieren der Leistung aus Metall-apps mithilfe von Resource Heaps und Memoryless Rendern Ziele basierend.

Weitere Informationen finden Sie unter Apple [Metall Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Metal-Shader-Leistungsverbesserungen

Das Metal-Performance-Shader-Framework in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Viele der neuen Kernel wurden hinzugefügt, das Metal-Performance-Shader-Framework, damit die app-gekapselt, die datenparallele Berechnungen wie etwa farbraumkonvertierungen, die Farbe und neural Network-Vorgängen nutzen kann.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO-Verbesserungen

Das Framework ModelIO in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Das US-Dollar-Dateiformat wird jetzt unterstützt.
- Verwenden Sie die neue `MDLMaterialPropertyGraph` Klasse zur Laufzeit problemlos Unterstützung für Modelle ändert.
- Signiert von Feld ' Abstand ' Unterstützung hinzugefügt wurde die [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) Klasse.
- Verwenden Sie die neue `MDLLightProbeIrradianceDataSource` Klasse zur Unterstützung bei der Platzierung Licht Prüfpunkt.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit-Verbesserungen

Das SceneKit-Framework in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- SceneKit enthält jetzt ein neues physisch basierten rendern (PBR)-System für realistischer Ergebnisse bei der einfacheren Asset erstellen.
- Verwenden Sie die neue [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) Schattierung modellieren Produkt eine Vielzahl von realistische Schattierungseffekte erforderlich ist nur für die drei grundlegende Eigenschaften (`Diffuse`, `Metalness` und `Roughness`).
- Seit PBR funktioniert am besten mit Beleuchtung umgebungsbasierte Schattierung, verwenden Sie die `LightingEnvironment` Eigenschaft abbildbasierte Beleuchtung auf die gesamte Szene tan zuweisen.
- Verwenden der `IESProfileURL` Eigenschaft zum echten lichtinstallationen importieren, die Beleuchtung, die Basis für reale-Werten, z. B. Intensität (in Lumen) definieren und die Farbe, die Temperatur (in Grad Kelvin).
- Die [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) Klasse kann mehr Realismus verleihen bereitstellen, mithilfe von HDR-Funktionen und Effekte. Verwenden Sie adaptive Offenlegung, zum Erstellen von automatischen Effekte oder Vignettierung verwenden, Farbsäume und farbliche Abstufung, um das Spiel filmatic Effekte hinzuzufügen.
- Sowohl für PBR HDR-Kamera-Features bieten bessere Ergebnisse als herkömmliche-Rendering-Techniken und daher SceneKit jetzt führt alle Farbe Berechnungen in einer linearen Farbraum (Wide-Color geräteanzeigen unter Verwendung Farbskala P3).
- SceneKit nun Farbe alle Farben entspricht, indem die Farbinformationen für das Profil zu lesen.
- SceneKit interpretiert Farbkomponentenwerte in einer linearen RGB-Farbspektrum für alle Arten von Shader.
- Da SceneKit liest und für die Color-Profilinformationen in der Textur Bilder anpassen, verwenden Sie Asset-Katalogs für alle Bilder, um sicherzustellen, dass diese Informationen bereitgestellt werden.
- Sowohl lineare als Farbe, die Leerzeichen-Rendering und Wide-Color kann deaktiviert werden, durch Angabe der `SCNDisableLinearSpaceRendering` und `SCNDisableWideGamut` Schlüssel in der app `Info.plist`.
- Erstellen Sie beliebige Polygon Primaten gemeinsam (entweder aus Dateien geladen oder programmgesteuert generiert) mit dem neuen Geometrie an [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) Klasse.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit-Verbesserungen

Das SpriteKit-Framework in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Tilemaps unterstützen jetzt die Kachel "Square", "hexagonal" und "isometrischen" Formen für 2D, 2,5D und Seite elementscrolling Spiele, die mit dem die `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet` Klassen.
- Verwenden Sie die neue `SKWarpGeometry` Klasse gestreckt oder verzerrt [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) oder [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendern. Die neue [SKAction](https://developer.apple.com/reference/spritekit/skaction) Klasse kann verwendet werden, um die Übergänge zwischen Warp-Effekte zu animieren.
- Benutzerdefinierte Shadern können Attribute bereitstellen (`SKAttribute`), können von jedem Knoten, die den Shader verwendet wird, durch Angabe eines Attributwerts separat konfiguriert werden (`SKAttributeValue`).
- Die [SKView](https://developer.apple.com/reference/spritekit/skview) Klasse bietet mehrere neue Methoden, um eine präzise steuern, wann und wie eine Szene gerendert wird.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit-Verbesserungen

Das UIKit-Framework in TvOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Der Fokus-API-wurde verbessert, um den Fokus von nicht-View-Element darüber hinaus zu unterstützen `UIViews`. Elemente, die Fokus unterstützen _müssen_ implementieren die `IUIFocusItem` Schnittstelle.
- Die neue `UIGraphicsRender` -Klasse stellt eine objektorientierte Methode zum Erstellen von Bitmaps oder PDF-Dokumente aus UIKit-Rendering oder Core-Grafiken und ersetzt die veraltete `UIGraphicsBeginImageContext` Methode.
- Die `UIUserInterfaceStyle` -Klasse wurde hinzugefügt, um zu bestimmen, welche Design der Benutzeroberfläche (dunkle oder helle) derzeit aktiv ist.
- Neue Unterstützung für hochgradig interaktive, objektbasierten, unterbrechbar Animation hinzugefügt wurde und auf Gesten van verknüpft werden. Pleas finden Sie unter Apple die [UIViewAnimating-Protokollreferenz](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator Class Reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider-Protokollreferenz](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters Class Reference](https://developer.apple.com/reference/uikit/uicubictimingparameters) und [UISpringTimingParameter Class Reference](https://developer.apple.com/reference/uikit/uispringtimingparameters) für Weitere Informationen.
- Die neue `UIPreviewInteraction` und `UIPreviewInteractionDelegate` ermöglichen es der Anwendung eine benutzerdefinierte Schnittstelle für Peek "und" Pop-Vorgänge bereitstellen.
- Die neue `UIAccessibilityCustomRotor` Klasse ermöglicht der app, benutzerdefinierte, Kontext-spezifische Funktionalität für hilfstechnologien wie etwa Voice Over bereitzustellen.
- Verwenden der `UIAccessibilityIsAssistiveTouchRunning` und `UIAccessibilityAssistiveTouchStatusDidChangeNotification` Symbole verwenden, um zu bestimmen, ob AssistiveTouch aktiviert ist.
- Verwenden der `UIAccessibilityHearingDevicePairedEar` und `UIAccessibilityHearingDevicePairedEarDidChangeNotification` Symbole zum Abrufen des Status aller gekoppelten MFi hören Hilfsmittel.
- Die neue [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) -API bietet neue Optionen (z. B. Einschränkungen für Lebensdauer) und wird automatisch kompatible Inhaltstypen für allgemeine Klassentypen deklariert.
- Zur Unterstützung von Typ "Dynamic" in Bezeichnungen Textfelder und Text-Felder verwenden Sie die neue `PreferredFontForTextStyle` Methode der `UIFont` Klasse.
- Entscheiden, ob ein Element Schriftart aktualisieren sollten bei der die Geräte `UIContentSizeCategory` ändert, verwenden die `AdjustsFontForContentSizeCategory` Eigenschaft der `UIContentSizeCategoryAdjusting` delegieren.
- Die app kann jetzt die Darstellung für den Badge für Elemente wie z. B. Text und Hintergrundfarbe Registerkarte steuern.
- Das Steuerelement aktualisiert jetzt unterstützt, in allen scrollansicht und scrollansicht Unterklassen (z. B. `UICollectionView`).
- Die `OpenURL` Methode der `UIApplication` Klasse heißt asynchron unterstützt jetzt einen Abschlusshandler, die aufgerufen wird, nachdem das Öffnen abgeschlossen ist.
- Freigeben von CloudKit zu initiieren und ändern Sie seine Eigenschaften, die mit dem neuen `UICloudSharingController` und `UICloudSharingControllerDelegate` Klassen.
- Nutzen Sie vorab abgerufene Zellen der Bildlauf bieten Verbesserungen `UICollectionViews` mit dem neuen `UICollectionViewDataSourcePrefetching` delegieren.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Neuerungen in TvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
