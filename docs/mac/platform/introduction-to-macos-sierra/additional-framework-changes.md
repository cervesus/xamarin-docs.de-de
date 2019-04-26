---
title: Zusätzliche macOS Sierra Framework-Änderungen
description: Dieses Dokument beschreibt die kleinere Änderungen und Verbesserungen an vorhandenen Frameworks, die unter MacOS Sierra eingeführt. Er untersucht die Änderungen an das Framework Accelerate, AppKit, AVFoundation, Kerndaten, Core-Image, Foundation und vieles mehr.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6ed827c018931e5b79887dc355f136e2a84623d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61031626"
---
# <a name="additional-macos-sierra-framework-changes"></a>Zusätzliche macOS Sierra Framework-Änderungen

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Beschleunigen der Framework-Erweiterungen

Die folgende Verbesserungen wurden für MacOS Sierra Framework zu beschleunigen:

- Hinzugefügte Quadrature (ganzzahligen berechnen).
- Grundlegende Funktionen zum Erstellen von neuronaler Netzwerken hinzugefügt.
- Hinzugefügte geometrische prädikatfunktionen, um Dinge wie die Schnittmenge von zwei geometrischen Objekten getestet.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>AppKit-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das AppKit Framework für MacOS Sierra:

- Verschiedene Verbesserungen `NSCollectionView` wie z.B.:
    - **Reduzierbare Abschnitte** -ermöglicht dem Benutzer, einen Auflistungsansicht-Abschnitt in einer einzelnen horizontalen Zeile reduzieren.
    - **Floating-Header** -Kopf- und Fußzeilen können jetzt (in einem Flusslayout) abgedockt werden über die gleiche API wie [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) unter iOS.
    - **Bildlauffähige Hintergrund Ansichten** -Auflistung Ansichten Hintergrund kann jetzt festgelegt werden, zusammen mit den Inhalt einen Bildlauf durchführen.
- Der Layoutdurchlauf zurückgestellte Ansicht wurde erweitert und wurde optimiert.
- Die Drag & Drop-API enthält nun die neue `NSFilePromiseProvider` und `NSFilePromiseReceiver` Klassen zur Unterstützung ziehen flocking.
- Mehrere Konstruktoren der Einfachheit halber wurden an vorhandene Steuerelemente hinzugefügt:
    -  `NSButton` enthält neue Konstruktoren zum Erstellen von Schaltflächen, Kontrollkästchen und Optionsfelder.
    -  `NSTextField` enthält neue Konstruktoren zum Erstellen von umschließt und ohne Umbrüche Bezeichnungen, attributierte Bezeichnungen und bearbeitbare Textfelder.
    -  `NSSegmentedControl` enthält neue Konstruktoren zum Erstellen von segmentierte Steuerelemente aus einer Gruppe von Bezeichnungen oder Bilder.
    -  `NSSlider` enthält neue Konstruktoren für das horizontale lineare Schieberegler erstellen.
    -  `NSImageView` enthält neue Konstruktoren zum Erstellen von nicht-bearbeitbaren Bildansichten aus einem angegebenen `NSImage`.
-  Die neue `NSGridView` Automatisches Layout, die eine Auflistung von untergeordneten Ansichten in ein Raster mit Variablen Größe, Zeilen und Spalten, die dynamisch angezeigt oder ausgeblendet werden können hinzugefügt wurde.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das AVFoundation Framework für MacOS Sierra:

- In MacOS die app nicht mehr verfügt über verschiedene implementieren [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) Verhalten basierend auf Inhaltstyp. Legen Sie einfach die `Rate` -Eigenschaft und AVFoundation bestimmt Wenn genügend Inhalte ohne treten für die Wiedergabe verfügbar ist.
- Die neue `AVPlayerLooper` Klasse macht es einfacher, einen bestimmten Media während der Wiedergabe in einer Schleife.
- Die `AVAssetDownloadURLSession` Klasse ermöglicht das Herunterladen und spätere Wiedergabe von FairPlay verschlüsselte HLS-Datenströmen.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Core-Data-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um dem Kernframework für die für MacOS Sierra:

- Stamm [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte unterstützt gleichzeitige Fehlerbehandlung und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) Klasse verwaltet einen Pool mit SQLite-Datenspeicher.
- Die [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte mit SQLite-Datenspeicher in die WAL-Journal-Modus-Unterstützung der neuen Abfrage Generation Funktion, in denen verwaltete Objekt Kontexten (MOC) für bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen an.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und anderen Konfigurationsressourcen Core Data.
- Mehrere neue Hilfsmethoden hinzugefügt wurden `NSManagedObject` abrufen und Erstellen von Unterklassen zu vereinfachen.

Weitere Informationen finden Sie unter Apple [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Core-Image-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das Core-Image-Framework für MacOS Sierra:

- Die `ImageWithExtent` Methode der [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) Klasse kann verwendet werden, um die benutzerdefinierte Verarbeitung in der Filtervorgang einzufügen. Core-Image wird Ruft den angegebenen Rückruf zwischen filtern, wenn ein Bild für die Ausgabe verarbeitet oder angezeigt werden.
- Die app kann jetzt Bilder in einem Bereich der Farbe außerhalb des Kontexts der Core-Image arbeiten-Farbraum verarbeiten, durch die Konvertierung in und aus des Farbraums vor und nach der Verarbeitung.
- Die Core-Image-Kernel kann jetzt ein bestimmtes Pixel-Ausgabeformat anfordern.
- Die folgenden neuen Image-Filter wurden hinzugefügt: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` und `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das Foundation-Framework für MacOS Sierra:

- Verwenden der [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) API zur Darstellung, konvertieren und viele der am häufigsten verwendeten physische Einheiten wie z. B. anzeigen Masse, Länge, die Geschwindigkeit, Dauer und Temperatur.
- Verwenden der [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) Klasse zum Analysieren und Generieren von ISO 8601-formatierte Datumsangaben.
- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse um Datum und Uhrzeit-Intervall Berechnungen wie Dauer, für den Vergleich von Abständen, und für die Interval-Schnittpunkte zu testen.
- Verwenden der [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) Klasse, um die Elemente der den Namen einer Person aus einer Zeichenfolge zu analysieren.
- Verwenden Sie die neue [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) Klasse, um Metriken für eine URL mit dem Netzwerk der Sitzung abzurufen.

Weitere Informationen finden Sie unter Apple [Foundation Anmerkungen zu dieser Version für v10.12 für OS X und iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das GameKit Framework für MacOS Sierra:

- Die **Game Center-App** als veraltet markiert und aus MacOS entfernt wurde. Wenn die app GameKit, verwendet es _müssen_ eine eigene Benutzeroberfläche zum Anzeigen von GameKit Features wie Bestenlisten usw. vorhanden. 
- Ein neues nur iCloud-Konto des Typs wurde implementiert, durch die [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) Klasse.
- Die neue [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) Klasse stellt eine allgemeine Lösung zum Verwalten von persistenten Datenspeicher auf der Game Center bereit. `GKGameSession` verwaltet eine Liste der Spieler und die app verantwortlich ist die Implementierung wie und wann Teilnehmer Datum gespeichert wird, abgerufen oder zwischen den Beteiligten ausgetauscht. In vielen Fällen können Spiele Sitzungen vorhandenen Turn-basierte Übereinstimmungen, die in Echtzeit Übereinstimmungen oder persistenten Spiel speichern Methoden ersetzen.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das GamePlayKit Framework für MacOS Sierra:

- Prozedurale Rauschen Generation hinzugefügt wurde und dienen zum verbessern die wirklichkeitsgetreuen Bilder in Texturen naturgetreue Bewegungen der Kamera wirklichkeitsgetreuen Bilder hinzu, und helfen anspruchsvoller spielwelten zu generieren.
- Verwenden Sie die räumliche Partitionierung, um die Welt, die Daten für die effiziente Suche zu partitionieren.
- Eine neue Monte-Carlo-Strategist ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) für die vollständige möglich verschieben Berechnung hinzugefügt wurde.
- Eine neue Decision Tree-API wurde hinzugefügt ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) und [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) um das Spiel zum Erstellen von AI zu verbessern.
- 3D-Unterstützung hinzugefügt wurde vorhandenen Agent "und" Pfad Auffinden von Verhaltensweisen, die mit dem neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) Klassen.
- Verwenden Sie die neue [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) Klasse, um leistungsstarke, naturgetreue Pfade bereitzustellen.
- Die neue [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) und [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Klassen stellen GameplayKit und einfacher als je zuvor SpriteKit kombiniert.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Metal-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das Metal-Framework für MacOS Sierra:

- 3D-Apps und spielen können Sie jetzt _Mosaik_ komplexe Szenen und "Geometry" über die GPU effizientes Rendering.
- Verwenden Sie zum Erstellen einer hochgradig optimierten Sammlung von Material und Licht Kombination-Funktionen für eine Szene Funktion Spezialisierung.
- Geben Sie, dass eine präzisere Kontrolle ressourcenzuteilung zum Optimieren der Leistung aus Metall-apps mithilfe von Resource Heaps und Memoryless Rendern Ziele basierend.

Weitere Informationen finden Sie unter Apple [Metall Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Verbesserungen hinsichtlich des e/a-Framework

Die folgende Verbesserungen wurden vorgenommen, um die e/a-Frameworks für MacOS Sierra:

- Das US-Dollar-Dateiformat wird jetzt unterstützt.
- Verwenden Sie die neue `MDLMaterialPropertyGraph` Klasse zur Laufzeit problemlos Unterstützung für Modelle ändert.
- Signiert von Feld ' Abstand ' Unterstützung hinzugefügt wurde die [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) Klasse.
- Verwenden Sie die neue `MDLLightProbeIrradianceDataSource` Klasse zur Unterstützung bei der Platzierung Licht Prüfpunkt.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Fotos Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um die Fotos-Framework für MacOS Sierra:

- Foto bearbeiten ist jetzt verfügbar, für apps, die Unterstützung der Fotos-Framework und Erweiterungen zum Bearbeiten von Fotos (für die Verwendung innerhalb von Fotos und Kamera apps).
- Verwenden Sie die neue [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) Klasse, um die Änderungen auf das Video und weiterhin Inhalt Live Fotos gelten.
- Verwenden der [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) und [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) Klassen nutzen das neue Feature der Core-Image-Prozessor zu bearbeiten.
- Zur Unterstützung von Live Fotos, die [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) und [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) Klassen wurden in iOS, MacOS portiert.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das SceneKit-Framework für MacOS Sierra:

- Enthält jetzt ein neues physisch basierten rendern (PBR)-System für realistischer Ergebnisse bei der einfacheren Asset erstellen.
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Framework-Sicherheitserweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das Sicherheitsframework für MacOS Sierra:

- Die `SecKey` Schnittstelle wurde modernisiert und einheitlich für alle Plattformen (iOS, TvOS, WatchOS und Mac OS).

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um das SpriteKit-Framework für MacOS Sierra:

- Tilemaps unterstützen jetzt die Kachel "Square", "hexagonal" und "isometrischen" Formen für 2D, 2,5D und Seite elementscrolling Spiele, die mit dem die `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet` Klassen.
- Verwenden Sie die neue `SKWarpGeometry` Klasse gestreckt oder verzerrt [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) oder [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendern. Die neue [SKAction](https://developer.apple.com/reference/spritekit/skaction) Klasse kann verwendet werden, um die Übergänge zwischen Warp-Effekte zu animieren.
- Benutzerdefinierte Shadern können Attribute bereitstellen (`SKAttribute`), können von jedem Knoten, die den Shader verwendet wird, durch Angabe eines Attributwerts separat konfiguriert werden (`SKAttributeValue`).
- Die [SKView](https://developer.apple.com/reference/spritekit/skview) Klasse bietet mehrere neue Methoden, um eine präzise steuern, wann und wie eine Szene gerendert wird.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Neue Frameworks

Die folgenden Frameworks wurden in MacOS Sierra hinzugefügt:

- **Intents-Framework** -dieses Framework ermöglichen es der Anwendung zum Untersuchen von Aktivitäten (z. B. Speicherort oder der Benutzer Aktionen), und geeignete Maßnahmen, die anhand dieser Informationen.
- **SafariServices Framework** -dieses Framework kann die app für MacOS und iOS-app-Erweiterungen für Safari (z. B. Content Blocker) zu entwickeln.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://developer.xamarin.com/samples/mac/)
- [Neuerungen in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
