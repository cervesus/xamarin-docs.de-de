---
title: Zusätzliche macOS Sierra Framework-Änderungen
description: In diesem Dokument werden kleinere Änderungen und Erweiterungen vorhandener Frameworks beschrieben, die in macOS Sierra eingeführt wurden. Sie untersucht Änderungen am Beschleunigung-Framework, AppKit, AVFoundation, Core Data, Core Image, Foundation und mehr.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 44468d3f2d323065161c290f2df8e6f0e89d3def
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724987"
---
# <a name="additional-macos-sierra-framework-changes"></a>Zusätzliche macOS Sierra Framework-Änderungen

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Beschleunigung von Framework-Erweiterungen

Die folgenden Verbesserungen wurden am Beschleunigung-Framework für macOS Sierra vorgenommen:

- Die Quadrature (ganzzahliger Kalkül) wurde hinzugefügt.
- Grundlegende Funktionen zum Erstellen von neuronalen Netzwerken hinzugefügt
- Es wurden geometrische Prädikat Funktionen hinzugefügt, um auf Dinge wie die Schnittmenge zweier geometrischer Objekte zu testen.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Erweiterungen des AppKit-Frameworks

Die folgenden Verbesserungen wurden am AppKit-Framework für macOS Sierra vorgenommen:

- Es gibt mehrere Verbesserungen an `NSCollectionView` beispielsweise:
  - Reduzier **Bare Abschnitte** : ermöglicht dem Benutzer das Reduzieren eines Auflistungs Ansichts Abschnitts in eine einzelne horizontale Zeile.
  - Gleit **Komma Kopfzeilen** : Kopf-und Fußzeilen können nun mit derselben API wie [uicollectionview](https://developer.apple.com/reference/uikit/uicollectionview) in ios (in einem Fluss Layout) übertragen werden.
  - Bild lauffähigen **Hintergrund Ansichten** : ein Auflistungs Ansichten-Hintergrund kann jetzt so festgelegt werden, dass er mit dem Inhalt
- Der verzögerte Ansichts Layoutdurchlauf wurde optimiert und erweitert.
- Die Drag & amp; Drop-API umfasst nun die neuen `NSFilePromiseProvider`-und `NSFilePromiseReceiver` Klassen, um das ziehen zu unterstützen.
- Vorhandenen Steuerelementen wurden mehrere bequeme Konstruktoren hinzugefügt:
  - `NSButton` enthält neue Konstruktoren zum Erstellen von pushschaltflächen, Kontrollkästchen und Options Feldern.
  - `NSTextField` enthält neue Konstruktoren zum Erstellen von Wrapping-und nichtwrapping Bezeichnungen, attributierten Bezeichnungen und bearbeitbaren Textfeldern.
  - `NSSegmentedControl` enthält neue Konstruktoren zum Erstellen von segmentierten Steuerelementen aus einer Gruppe von Bezeichnungen oder Bildern.
  - `NSSlider` enthält neue Konstruktoren zum Erstellen horizontaler linearer Schieberegler.
  - `NSImageView` enthält neue Konstruktoren zum Erstellen von nicht bearbeitbaren Bildansichten von einem bestimmten `NSImage`.
- Die neue `NSGridView` wurde hinzugefügt, um eine Auflistung von unter Sichten automatisch in einem Raster mit Zeilen und Spalten variabler Größen und Spalten zu erstellen, die dynamisch ausgeblendet oder angezeigt werden können.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>Erweiterungen des AVFoundation-Frameworks

Die folgenden Verbesserungen wurden am AVFoundation-Framework für macOS Sierra vorgenommen:

- In macOS muss die APP nicht mehr unterschiedliche [avplayeritem](https://developer.apple.com/reference/avfoundation/avplayeritem) -Verhaltensweisen basierend auf dem Inhaltstyp implementieren. Legen Sie einfach die `Rate`-Eigenschaft fest, und AVFoundation bestimmt, wann genug Inhalt für die Wiedergabe ohne Stalling verfügbar ist.
- Die neue `AVPlayerLooper`-Klasse vereinfacht die Schleife eines bestimmten Mediums während der Wiedergabe.
- Die `AVAssetDownloadURLSession`-Klasse ermöglicht das herunterladen und spätere Wiedergabe von Fairplay-verschlüsselten HLS-Datenströmen.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Erweiterungen des Kern-Data Frameworks

Die folgenden Verbesserungen wurden am Core Data Framework für macOS Sierra vorgenommen:

- Stamm- [nsmanagedobjectcontext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte unterstützen das gleichzeitige fehlschlagen und Abrufen ohne Serialisierung.
- Die [nspersistentstorecoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) -Klasse verwaltet einen Pool mit SQLite-Daten speichern.
- Die [nsmanagedobjectcontext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte mit SQLite-Daten speichern im Wal-Journal Modus unterstützen die neue Abfrage Generierungs Funktion, bei der verwaltete Objekt Kontexte (Managed Object Context, MUC) an bestimmte Daten Bank Versionen angeheftet werden können, um zukünftige Abruf-und fehlerverarbeitungs Vorgänge durchzusetzen
- Verwenden der `NSPersistenceContainer` auf hoher Ebene, um auf die `NSPersistentStoreCoordinator`, [nsmanagedobjectmodel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und andere Kern Daten Konfigurations Ressourcen zu verweisen.
- Es wurden mehrere neue Hilfsmethoden hinzugefügt, die `NSManagedObject` die das Ausführen von Abruf Vorgängen und das Erstellen von Unterklassen vereinfachen.

Weitere Informationen finden Sie in der [Core Data Framework-Referenz](https://developer.apple.com/reference/coredata)von Apple.

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Erweiterungen des Kern Image-Frameworks

Die folgenden Verbesserungen wurden an dem Core-Image Framework für macOS Sierra vorgenommen:

- Die `ImageWithExtent`-Methode der [cifilter](https://developer.apple.com/reference/coreimage/cifilter) -Klasse kann verwendet werden, um eine benutzerdefinierte Verarbeitung in den Filter Vorgang einzufügen. Das Core-Image Ruft den gegebenen Rückruf zwischen Filtern auf, wenn ein Bild für die Ausgabe oder Anzeige verarbeitet wird.
- Die APP kann jetzt Bilder in einem Farbraum außerhalb des Arbeits Farbraums des Kern Bild Kontexts verarbeiten, indem Sie vor und nach der Verarbeitung in den und aus dem Farbraum in-und auslagern.
- Der kernbild-Kernel kann nun ein bestimmtes Pixel Ausgabeformat anfordern.
- Die folgenden neuen Bild Filter wurden hinzugefügt: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` und `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation Framework-Erweiterungen

Die folgenden Verbesserungen wurden an Foundation Framework für macOS Sierra vorgenommen:

- Verwenden Sie die [nsdimensiontions](https://developer.apple.com/reference/foundation/nsdimension) -API, um viele der gängigsten physischen Einheiten wie z. b. Masse, Länge, Geschwindigkeit, Dauer und Temperatur darzustellen, zu wandeln und anzuzeigen.
- Verwenden Sie die [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) -Klasse zum Auswerten und Erstellen von ISO 8601-formatierten Datumsangaben.
- Verwenden Sie die neue [nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval) -Klasse, um Datums-und Zeitintervall Berechnungen zu erstellen, z. b. Dauer Zeiten, zum Vergleichen von Intervallen und zum Testen von Intervall interabschnitten.
- Verwenden Sie die [nspersonnamecomponentsformatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) -Klasse, um die Elemente des Namens einer Person aus einer Zeichenfolge zu analysieren.
- Verwenden Sie die neue [nsurlsessiontaskmetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) -Klasse zum Abrufen von Metriken für eine URL-Netzwerksitzung.

Weitere Informationen finden Sie in den [Anmerkungen zu dieser Version von Apple für OS X v 10.12 und IOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>Verbesserungen bei GameKit

Die folgenden Verbesserungen wurden am GameKit-Framework für macOS Sierra vorgenommen:

- Die **Game Center-App** wurde als veraltet markiert und aus macOS entfernt. Wenn die APP GameKit verwendet, _muss_ Sie eine eigene Schnittstelle zum Anzeigen von GameKit-Features, z. b. Bestenlisten usw., enthalten.
- Ein neuer icloud-Kontotyp wurde von der [gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) -Klasse implementiert.
- Die neue Klasse " [gkgamesession](https://developer.apple.com/reference/gamekit/gkgamesession) " stellt eine generalisierte Lösung für die Verwaltung von persistenten Daten speichern auf Game Center bereit. `GKGameSession` verwaltet eine Liste von Playern, und die APP ist dafür verantwortlich, wie und wann das Teilnehmer Datum gespeichert, abgerufen oder zwischen den Playern ausgetauscht wird. In vielen Fällen können Spielsitzungen vorhandene Turn-basierte Übereinstimmungen, Echt Zeit Übereinstimmungen oder persistente Spiel Speichermethoden ersetzen.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>Erweiterungen des gameplaykit-Frameworks

Die folgenden Verbesserungen wurden am gameplaykit-Framework für macOS Sierra vorgenommen:

- Das Generieren von Prozeduren wurde hinzugefügt und kann verwendet werden, um den Realismus in Natur orientierten Texturen zu verbessern, den Bewegungsmöglichkeiten der Kamera zu verleihen und umfassende Spielwelten zu generieren.
- Verwenden Sie räumliche Partitionierung, um die spielworld-Daten für eine effiziente Suche zu partitionieren
- Für eine vollständige Verschiebungs Berechnung wurde ein neuer Monte Carlo-Stratege ("[gkmontecarlostrateg;](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)") hinzugefügt.
- Es wurde eine neue Decision Tree-API hinzugefügt ("[gkdecisiontree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) " und " [gkdecisionnode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)"), um die KI der Spiel Bildung zu verbessern.
- die 3D-Unterstützung wurde vorhandenen Agent-und Pfad Suchverhalten mithilfe der neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) -und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) -Klassen hinzugefügt.
- Verwenden Sie die neue Klasse " [gkmeshgraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) ", um Hochleistungs Pfade mit hoher Leistung bereitzustellen.
- Die neuen Klassen " [gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) " und " [gksknodecomponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) " vereinfachen das Kombinieren von gameplaykit und spritekit.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Erweiterungen von Metal Framework

Die folgenden Verbesserungen wurden am Metal-Framework für macOS Sierra vorgenommen:

- 3D-apps und-Spiele können jetzt _Mosaik verwendet werden_ , um komplexe Szenen und Geometrie effizient über die GPU zu erzeugen.
- Verwenden Sie die Funktions Spezialisierung, um eine hochgradig optimierte Sammlung von Material-und Light-Kombinations Funktionen für eine Szene zu erstellen.
- Sorgen Sie für eine differenzierte Steuerung der Ressourcen Zuordnung, um die Leistung von Metal-basierten Apps mithilfe von Ressourcen Heaps und Speicher losen Renderingzielen zu optimieren.

Weitere Informationen finden Sie im [Handbuch zur Metal-Programmierung](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)von Apple.

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Erweiterungen des Modell-e/a-Frameworks

Die folgenden Verbesserungen wurden am Modell-e/a-Framework für macOS Sierra vorgenommen:

- Das Dateiformat "USD" wird jetzt unterstützt.
- Verwenden Sie die neue `MDLMaterialPropertyGraph`-Klasse, um Lauf Zeit Änderungen von Modellen problemlos zu unterstützen.
- Die Unterstützung für unterstützte Entfernungs Felder wurde der [mdlvoxelarray](https://developer.apple.com/reference/modelio/mdlvoxelarray) -Klasse hinzugefügt.
- Verwenden Sie die neue `MDLLightProbeIrradianceDataSource`-Klasse, um die Platzierung von lichttests zu unterstützen.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Erweiterungen für Fotos-Framework

Die folgenden Verbesserungen wurden am Foto-Framework für macOS Sierra vorgenommen:

- Die Live-Fotobearbeitung steht nun für Apps zur Verfügung, die das Foto Framework und die Bearbeitung von Fotos unterstützen (für die Verwendung in den Fotos und Kamera-Apps).
- Verwenden Sie die neue [phlivephorteeditingcontext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) -Klasse, um Änderungen auf das Video und den Inhalt von livefotos anzuwenden.
- Verwenden Sie die Klassen " [ciimageprocessorinput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) " und " [ciimageprocessoroutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) ", um das neue Feature "Core Image Processor" zum Ausführen von Änderungen zu nutzen.
- Zur Unterstützung von livefotos wurden die Klassen [phlivephorto](https://developer.apple.com/reference/photos/phlivephoto) und [phlivephuanview](https://developer.apple.com/reference/photosui/phlivephotoview) von IOS zu macOS portiert.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>Erweiterungen des scenekit-Frameworks

Die folgenden Verbesserungen wurden am scenekit-Framework für macOS Sierra vorgenommen:

- Enthält nun ein neues physisch basiertes Rendering (PBR)-System für realistischere Ergebnisse mit einfacherer Erstellung von Medienobjekten.
- Verwenden Sie das neue [scnlightingmodelphysicallybased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) -Schattierungs Modell, um eine breite Palette realistischer Schattierungs Effekte zu erzeugen, während nur drei grundlegende Eigenschaften (`Diffuse`, `Metalness` und `Roughness`) benötigt werden.
- Da die PBR-Schattierung am besten mit der Umgebungs basierten Beleuchtung funktioniert, verwenden Sie die `LightingEnvironment`-Eigenschaft, um eine Bildbasierte Beleuchtung der gesamten Szene zuzuweisen.
- Verwenden Sie die `IESProfileURL`-Eigenschaft, um einfache Beleuchtungs Vorrichtungen zu importieren, die Beleuchtungs Basis auf realen Werten, wie Intensität (in Lumens) und Farbtemperatur (in Grad Kelvin) definieren.
- Mit den HDR-Features und-Effekten kann die [scncamera](https://developer.apple.com/reference/scenekit/scncamera) -Klasse einen besseren Realismus bereitstellen. Verwenden Sie Adaptive verfügbar machung, um automatische Effekte zu erstellen, oder verwenden Sie die Werte für die Durchsetzung von Farben, die Farbränder und die farbeinstufung, um dem Spiel filtersche
- Sowohl PBR-als auch HDR-Kamera Features bieten bessere Ergebnisse als herkömmliche Renderingverfahren. Daher führt scenekit jetzt alle Farb Berechnungen in einem linearen Farbraum durch (mit P3-Farbpalette auf breit farbigen Geräte anzeigen).
- Scenekit ist nun mit allen Farben übereinstimmen, indem die Farbprofil Informationen gelesen werden.
- Scenekit interpretiert Farbkomponenten Werte in einem linearen RGB-Farbraum für alle shadertypen.
- Da scenekit Farbprofil Informationen in Textur Bildern liest und anpasst, verwenden Sie für alle Images Asset-Kataloge, um sicherzustellen, dass diese Informationen bereitgestellt werden.
- Sowohl lineares Farb Raum Rendering als auch breit Farben können deaktiviert werden, indem die `SCNDisableLinearSpaceRendering` und `SCNDisableWideGamut` Schlüssel im `Info.plist`der APP angegeben werden.
- Erstellen Sie beliebige Polygon-Prime (entweder aus Dateien geladen oder Programm gesteuert generiert), um die Geometrie mit der neuen [scngeometryprimitivetyetpolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon?language=objc) -Klasse anzugeben.

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Erweiterungen des Sicherheits Frameworks

Die folgenden Verbesserungen wurden am Sicherheits Framework für macOS Sierra vorgenommen:

- Die `SecKey`-Schnittstelle wurde auf allen Plattformen (Ios, tvos, watchos und macOS) modernisiert und vereinheitlicht.

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>Erweiterungen des spritekit-Frameworks

Die folgenden Verbesserungen wurden am spritekit-Framework für macOS Sierra vorgenommen:

- Tilemaps unterstützt jetzt quadratische, hexagonale und isometrische Kachel Formen für 2D-, 2.5 d-und Side-Scroll-Spiele mithilfe der Klassen `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet`.
- Verwenden Sie die neue `SKWarpGeometry`-Klasse, um das Rendern von [skspritenode](https://developer.apple.com/reference/spritekit/skspritenode) oder [schrägerectnode](https://developer.apple.com/reference/spritekit/skeffectnode) zu erweitern oder zu verzerren. Die neue [skaction](https://developer.apple.com/reference/spritekit/skaction) -Klasse kann verwendet werden, um Übergänge zwischen Warp-Effekten zu animieren.
- Benutzerdefinierte Shader können Attribute (`SKAttribute`) bereitstellen, die separat von jedem Knoten konfiguriert werden können, der den Shader verwendet, indem ein Attribut Wert (`SKAttributeValue`) bereitgestellt wird.
- Die [skview](https://developer.apple.com/reference/spritekit/skview) -Klasse stellt mehrere neue Methoden bereit, mit denen Sie präzise steuern können, wann und wie eine Szene gerendert wird.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Neue Frameworks

Die folgenden Frameworks wurden macOS Sierra hinzugefügt:

- **Intents Framework** : Dieses Framework ermöglicht der APP, Interaktionen (z. b. Speicherort oder Benutzeraktionen) zu überprüfen und auf der Grundlage dieser Informationen Maßnahmen zu ergreifen.
- **Safariservices-Framework** : Dieses Framework ermöglicht der APP, App-Erweiterungen für Safari (z. b. Inhalts Blockierer) für macOS und IOS zu entwickeln.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Neuerungen in OS X 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
