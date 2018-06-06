---
title: Zusätzliche MacOS Sierra Framework-Änderungen
description: Dieses Dokument beschreibt die geringfügigen Änderungen und Erweiterungen für vorhandene Frameworks, die in MacOS Sierra eingeführt. Er überprüft die Änderungen an das Framework beschleunigen, AppKit, AVFoundation, Core Daten, Core-Image, Foundation und vieles mehr.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3cfa2e9bcb0be4d65462914215045c9c7f01da5b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792592"
---
# <a name="additional-macos-sierra-framework-changes"></a>Zusätzliche MacOS Sierra Framework-Änderungen

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Framework-Erweiterungen zu beschleunigen

Die folgende Verbesserungen beschleunigen Framework für MacOS Sierra vorgenommen wurden:

- Hinzugefügte Quadratur (integrale Analysis).
- Grundlegende Funktionen zum Erstellen von neuronalen Netzwerken hinzugefügt.
- Hinzugefügte geometrische prädikatfunktionen, um z. B. die Schnittmenge zweier geometrische Objekte zu testen.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>AppKit Framework-Erweiterungen

Die folgende Verbesserungen AppKit Framework für MacOS Sierra vorgenommen wurden:

- Mehrere Erweiterungen für `NSCollectionView` wie z. B.:
    - **Reduzierbare Abschnitte** -ermöglicht es dem Benutzer um einen Abschnitt Auflistungsansicht in einer einzelnen horizontalen Reihe zu reduzieren.
    - **Gleitkommawert Header** -Kopf- und Fußzeilen können jetzt (in einem Flusslayout) umfließt verwenden die gleiche API wie [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) in iOS.
    - **Bildlauffähige Hintergrund Ansichten** -Auflistung Ansichten Hintergrund kann jetzt festgelegt werden, zusammen mit den Inhalt einen Bildlauf durchführen.
- Die verzögerte Ansicht Layoutdurchlauf wurde erweitert und wurde optimiert.
- Die Drag-and-Drop-API umfasst jetzt die neue `NSFilePromiseProvider` und `NSFilePromiseReceiver` Klassen zur Unterstützung ziehen flocking.
- Mehrere halber Konstruktoren wurden an vorhandene Steuerelemente hinzugefügt:
    -  `NSButton` enthält neue Konstruktoren zum Erstellen von Schaltflächen, Kontrollkästchen und Optionsfelder.
    -  `NSTextField` enthält neue Konstruktoren zum Erstellen von umschließen und nicht-Wrapping Bezeichnungen, attributierte Bezeichnungen und bearbeitbare Textfelder.
    -  `NSSegmentedControl` enthält neue Konstruktoren für das Erstellen von segmentierte Steuerelemente aus einer Gruppe von Bezeichnungen oder Bilder.
    -  `NSSlider` enthält neue Konstruktoren für das horizontale lineare Schieberegler erstellen.
    -  `NSImageView` enthält neue Konstruktoren zum Erstellen von nicht bearbeitbaren Image-Sichten aus einer bestimmten `NSImage`.
-  Die neue `NSGridView` wurde hinzugefügt, um Automatisches Layout, die eine Auflistung von untergeordneten Ansichten in ein Raster mit variabler Größe, Zeilen und Spalten, die dynamisch angezeigt oder ausgeblendet werden können.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation Framework-Erweiterungen

Die folgende Verbesserungen AVFoundation Framework für MacOS Sierra vorgenommen wurden:

- In MacOS, die app nicht mehr enthält verschiedene implementieren [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) Verhaltensweisen auf der Grundlage von Inhaltstyps. Legen Sie einfach die `Rate` Eigenschaft und AVFoundation wird bestimmt, wann genügend Inhalt verfügbar ist, für die Wiedergabe ohne führte ist.
- Die neue `AVPlayerLooper` Klasse erleichtert es, einen bestimmten Medientyp während der Wiedergabe einer Schleife.
- Die `AVAssetDownloadURLSession` Klasse ermöglicht das Herunterladen und spätere Wiedergabe des FairPlay verschlüsselte HLS-Datenströmen.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Core Data-Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um die Core-Data-Framework für MacOS Sierra:

- Stamm [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte unterstützt gleichzeitige zugesteht und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) Klasse verwaltet einen Pool von SQLite-Datenspeichern.
- Die [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte mit SQLite-Datenspeicher in die WAL Journal Modus Unterstützung der neuen Abfrage Generation Funktion, auf dem verwalteten Objekt Kontexten (MOC) auf bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und andere Ressourcen Core Daten.
- Mehrere neue Hilfsmethoden wurden hinzugefügt, um `NSManagedObject` einfacher zu Abrufvorgängen auszuführen und Unterklassen zu erstellen.

Weitere Informationen finden Sie in der Apple- [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Core-Image-Framework-Erweiterungen

Die folgende Verbesserungen in der Core-Image-Framework für MacOS Sierra vorgenommen wurden:

- Die `ImageWithExtent` Methode der [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) Klasse kann verwendet werden, um benutzerdefinierte Verarbeitung in der Filtervorgang einzufügen. Core-Image wird Ruft den angegebenen Rückruf zwischen filtern, wenn ein Bild für die Ausgabe zu verarbeiten oder anzuzeigen.
- Die app kann jetzt Bilder in einem Farbraum außerhalb des Kontexts die Core-Image Arbeitsfarbraum verarbeiten, durch Konvertieren in und aus den Farbraum vor und nach der Verarbeitung.
- Der Kernel Core-Image kann jetzt ein bestimmtes Pixel Ausgabeformat anfordern.
- Die folgenden neuen Image Filter wurden hinzugefügt: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` und `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation-Framework-Erweiterungen

Die folgende Verbesserungen an der Foundation-Framework für MacOS Sierra vorgenommen wurden:

- Verwenden der [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) Masse, Länge, Geschwindigkeit, Dauer und Temperatur API zum darstellen, konvertieren und viele der am häufigsten verwendeten physischen Einheiten wie z. B. anzeigen.
- Verwenden der [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) Klasse für das Analysieren und Generieren von ISO 8601-formatierte Datumsangaben.
- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse, um das Datum und Uhrzeit Intervall-Berechnungen, z. B. Dauer, für den Vergleich von Intervallen und Tests für Intervall Schnittmengen vornehmen.
- Verwenden der [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) Klasse, die Elemente der den Namen einer Person aus einer Zeichenfolge zu analysieren.
- Verwenden Sie die neue [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) Klasse zum Abrufen von Metriken für einen URL an networking-Sitzung.

Weitere Informationen finden Sie in der Apple- [Foundation Anmerkungen zu dieser Version für OS X-v10.12 und iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit Framework-Erweiterungen

Die folgende Verbesserungen GameKit Framework für MacOS Sierra vorgenommen wurden:

- Die **Game Center-App** veraltet und aus MacOS entfernt wurde. Wenn die app GameKit, verwendet er _müssen_ Darstellung eine eigene Benutzeroberfläche zum Anzeigen von GameKit-Funktionen, z. B. usw. setzen. 
- Ein neues nur iCloud-Konto des Typs von implementiert wurde die [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) Klasse.
- Die neue [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) Klasse bietet eine allgemeine Lösung zum Verwalten von persistenten Datenspeicher Game Center. `GKGameSession` verwaltet eine Liste der Spieler und die app ist verantwortlich Formular implementieren wie und wann Teilnehmer Datum gespeichert wird, abgerufen oder zwischen den Beteiligten ausgetauscht. In vielen Fällen können Spiel Sitzungen vorhandenen aktivieren basierende Übereinstimmungen, Übereinstimmungen in Echtzeit oder persistenten Spiel speichern Methoden ersetzen.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit Framework-Erweiterungen

Die folgende Verbesserungen GamePlayKit Framework für MacOS Sierra vorgenommen wurden:

- Prozedurale Rauschen Generierung wurde hinzugefügt und kann verwendet werden, um realistische in natürlicher aussehende Texturen verbessern Kamera Bewegungen realistische hinzu, und Hilfe spielwelten zu generieren.
- Die für die effiziente Suche Spiels partitioniert räumliche Partitionierung verwendet.
- Eine neue Monte Carlo-Strategie ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) für eine vollständige möglich verschieben Berechnung hinzugefügt wurde.
- Eine neue Decision Tree-API wurde hinzugefügt ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) und [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) um das Spiel-Erstellung AI zu verbessern.
- 3D-Unterstützung wurde auf vorhandenen Agent und Pfad suchen mit dem neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) Klassen.
- Verwenden Sie die neue [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) Klasse, um leistungsstarke, natürlichen aussehende Pfade bereitzustellen.
- Die neue [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) und [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Klassen stellen Kombinieren von GameplayKit und SpriteKit einfacher als je zuvor.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>-Metal-Framework-Erweiterungen

Die folgende Verbesserungen Metal-Framework für MacOS Sierra vorgenommen wurden:

- 3D apps und Spiele können jetzt _Mosaik_ komplexe Szenen und Geometrie über die GPU effizientes Rendering.
- Verwenden Sie zum Erstellen einer Sammlung stark optimiert, Material und hell Kombination aus Funktionen für eine Szene Spezialisierung-Funktion.
- Geben Sie eine präzisere Kontrolle der Ressourcenzuordnung zum Optimieren der Leistung von Metall auf Grundlage der Ressource Heaps mit apps und Memoryless Rendern Ziele.

Weitere Informationen finden Sie unter der Apple- [Metall Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Verbesserungen hinsichtlich des e/a-Framework

Die folgende Verbesserungen wurden vorgenommen, um das Modell e/a-Framework für MacOS Sierra:

- Das Dateiformat USD wird jetzt unterstützt.
- Verwenden Sie die neue `MDLMaterialPropertyGraph` Klasse zur Laufzeit problemlos Unterstützung für Modelle ändert.
- Signiert Abstand Feld Unterstützung, um hinzugefügt wurde die [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) Klasse.
- Verwenden Sie die neue `MDLLightProbeIrradianceDataSource` Klasse, um bei der Platzierung Licht Prüfpunkt zu unterstützen.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Fotos Framework-Erweiterungen

Die folgende Verbesserungen wurden vorgenommen, um die Fotos-Framework für MacOS Sierra:

- Bearbeiten von Live Foto ist jetzt verfügbar für apps, die das Framework Fotos unterstützen und Erweiterungen zum Bearbeiten von Fotos (für die Verwendung innerhalb der Fotos und Kamera apps).
- Verwenden Sie die neue [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) Klasse, um die Änderungen auf das Video und weiterhin Inhalt Live Fotos anzuwenden.
- Verwenden der [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) und [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) Klassen nutzen die neuen Core-Image Prozessorfunktion Bearbeitungen ausführen.
- Zur Unterstützung von Live Fotos der [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) und [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) Klassen wurden iOS, Mac OS portiert.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit Framework-Erweiterungen

Die folgende Verbesserungen SceneKit Framework für MacOS Sierra vorgenommen wurden:

- Enthält jetzt neues System physisch basierend rendern (PBR) für realistischere Ergebnisse bei der einfacheren Asset authoring aus.
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Framework-Sicherheitserweiterungen

Die folgende Verbesserungen an der-Sicherheitsframework für MacOS Sierra vorgenommen wurden:

- Die `SecKey` Schnittstelle modernisiert und einheitlich für alle Plattformen (iOS, tvos. außerdem wurden, WatchOS und MacOS) wurde.

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit Framework-Erweiterungen

Die folgende Verbesserungen SpriteKit Framework für MacOS Sierra vorgenommen wurden:

- Tilemaps unterstützt nun die Kachel "quadratisch, Sechseckige und isometrische" Formen 2D, 2,5 D und Durchführen eines Bildlaufs Seite Spiele, die mit der `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet` Klassen.
- Verwenden Sie die neue `SKWarpGeometry` Klasse gestreckt oder im Simulator beeinträchtigt werden [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) oder [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendern. Die neue [SKAction](https://developer.apple.com/reference/spritekit/skaction) Klasse zum Animieren der Übergänge zwischen Warp Effekte verwendet werden kann.
- Benutzerdefinierte Shadern können Attribute bereitstellen (`SKAttribute`), kann von jedem Knoten, die den Shader durch Angabe eines Attributwerts verwendet separat konfiguriert werden (`SKAttributeValue`).
- Die [SKView](https://developer.apple.com/reference/spritekit/skview) -Klasse stellt mehrere neue Methoden, um eine präzisere steuern, wann und wie eine Szene gerendert wird.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Neue Frameworks

Die folgenden Frameworks wurde MacOS Sierra hinzugefügt:

- **Intents Framework** -dieses Framework kann die app Untersuchen von Aktivitäten (z. B. Speicherort oder Benutzers Aktionen), und die Aktion, die auf Basis dieser Informationen.
- **SafariServices Framework** -dieses Framework kann die app zu app-Erweiterungen für Safari (z. B. Content Blocker) für MacOS und iOS entwickeln.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://developer.xamarin.com/samples/mac/)
- [Was ist neu in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
