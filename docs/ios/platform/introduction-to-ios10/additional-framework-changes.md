---
title: Zusätzliche iOS 10 Frameworks Änderungen
description: Dieser Artikel umfasst zusätzliche, kleinere Änderungen oder Erweiterungen von vorhandenen Frameworks für iOS 10.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 33852ef62bd00368ef6544d07e60dd6de4c3b7d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="additional-ios-10-frameworks-changes"></a>Zusätzliche iOS 10 Frameworks Änderungen

_Dieser Artikel umfasst zusätzliche, kleinere Änderungen oder Erweiterungen von vorhandenen Frameworks für iOS 10._

## <a name="av-foundation-framework-additions"></a>AV-Foundation Framework Ergänzungen

Das AVFoundation-Framework beinhaltet die folgenden Verbesserungen:

- In iOS-10, hat der Entwickler nicht mehr verschiedene implementieren [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) Verhaltensweisen auf der Grundlage von Inhaltstyps. Legen Sie einfach die `Rate` Eigenschaft und AVFoundation wird bestimmt, wann genügend Inhalt verfügbar ist, für die Wiedergabe ohne führte ist.
- Die neue [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) Klasse ersetzt die veraltete `AVCaptureStillImageOutput` -Klasse und stellt eine einheitliche Methode zum Verarbeiten von alle Workflows, die Fotos von anspruchsvollen Kontrolle und Überwachung von der Aufzeichnungsprozess und Unterstützung für neue Features wie Fotos Live und der UNFORMATIERTE Capture-Format.
- Die neue `AVPlayerLooper` Klasse erleichtert es, einen bestimmten Medientyp während der Wiedergabe einer Schleife.
- Die `AVAssetDownloadURLSession` Klasse ermöglicht das Herunterladen und spätere Wiedergabe des FairPlay verschlüsselte HLS-Datenströmen.
- Wird standardmäßig die [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) Klasse unterstützt Wide-Farbe, Breite Farbskala Erfassung automatisch, wenn Sie die Hardware der Geräte unterstützt wird. Finden Sie in der Apple- [iOS-Gerät Compatibility Referenz](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) Weitere Details.

## <a name="avkit-additions"></a>AVKit Ergänzungen

Das Framework AVKit enthält jetzt das neue `UpdatesNowPlayingInfoCenter` Eigenschaft, um anzugeben, wann die jetzt Playing Info-Center aktualisiert werden sollen.

## <a name="core-data-enhancements"></a>Core-Daten-Erweiterungen

iOS 10 umfasst die folgenden Erweiterungen für das Framework Core-Daten:

- Die [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) Objekte mit SQLite-Datenspeicher in die WAL Journal Modus Unterstützung der neuen Abfrage Generation Funktion, auf dem verwalteten Objekt Kontexten (MOC) auf bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen.
- Stamm [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) Objekte unterstützt gleichzeitige zugesteht und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) Klasse verwaltet einen Pool von SQLite-Datenspeichern.
- Mehrere neue Hilfsmethoden wurden hinzugefügt, um `NSManagedObject` einfacher zu Abrufvorgängen auszuführen und Unterklassen zu erstellen.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) und andere Ressourcen Core Daten.

Weitere Informationen finden Sie in der Apple- [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Core-Image-Erweiterungen

iOS 10 macht das Framework Core-Image die folgenden Erweiterungen:

- Entwickler kann jetzt Bilder in einem Farbraum außerhalb des Kontexts die Core-Image Arbeitsfarbraum verarbeiten, durch Konvertieren in und aus den Farbraum vor und nach der Verarbeitung.
- Für iOS-Geräte, die die A8 oder A9-CPUs verwenden, wird die UNFORMATIERTE Bildformat jetzt unterstützt. Core-Image bietet jetzt Unterstützung für die Decodierung RAW-Bilder aus der integrierten iSight Kamera oder aus einer 3rd Party Kamera. Verwenden der `FilterWithImageData` oder `FilterWithImageURL` Methoden die [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) Klasse, um UNFORMATIERTE Bilder zu verarbeiten.
- Wurden mehrere Rendering leistungsverbesserungen vorgenommen, um `UIImage` rendern (wenn es sich um eine von Core-Image Bild speichert gesicherte) in `UIImageView` Objekte. 
- `UIImage` Objekte mit Tags Wide-Farbskala rendert als Breite Farbskala Farbe im `UIImageView` Objekte auf iOS-Geräten, die Breite Farbskala unterstützen.
- Core-Image Kernelcode kann jetzt bestimmtes Pixel Ausgabeformate anfordern.
- Die `ImageWithExtent` Methode der [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) Klasse kann verwendet werden, um benutzerdefinierte Verarbeitung in der Filtervorgang einzufügen. Core-Image wird Ruft den angegebenen Rückruf zwischen filtern, wenn ein Bild für die Ausgabe zu verarbeiten oder anzuzeigen.

Darüber hinaus wurden die folgenden neuen Core-Image-Filter hinzugefügt:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Core während des Verschiebens Ergänzungen

Neu für iOS 10, enthält das Framework Core Motion Schrittzähler-Ereignisse, die eine app auf schnelle, Echtzeit des Benutzers anhalten und Fortsetzen der Verfolgung während der Ausführung Benachrichtigungen zu aktivieren. Verwenden der [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) für Vorder- oder Hintergrund Schrittzähler Ereignisse registrieren.

## <a name="foundation-enhancements"></a>Foundation-Erweiterungen

Foundation-Framework für iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) Klasse, um lokalisierte Messungen für die Anzeige für den Endbenutzer zu formatieren.
- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse, um das Datum und Uhrzeit Intervall-Berechnungen, z. B. Dauer, für den Vergleich von Intervallen und Tests für Intervall Schnittmengen vornehmen.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) -Klasse zum Konvertieren zwischen verschiedenen Einheiten des Measure (Maßeinheit) oder Berechnungen auf den Werten in anderen UOMs.

- Verwenden Sie die neue [NSUnit](https://developer.apple.com/reference/foundation/nsunit) und [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) Klassen zum Darstellen von bestimmten UOMs.
- Mehrere neue Eigenschaften hinzugefügt wurden die [NSLocal](https://developer.apple.com/reference/foundation/nslocale) Klasse, um lokale Daten und die verfügbaren Anzeigeformate abzurufen.

## <a name="gamekit-enhancements"></a>GameKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das GameKit-Framework in iOS 10 vorgenommen:

- Die **Game Center-App** als veraltet markiert und IOS entfernt wurde. Wenn die app GameKit, verwendet er _müssen_ Darstellung eine eigene Benutzeroberfläche zum Anzeigen von GameKit-Funktionen, z. B. usw. setzen. 
- Ein neues nur iCloud-Konto des Typs von implementiert wurde die [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) Klasse.
- Die neue [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) Klasse bietet eine allgemeine Lösung zum Verwalten von persistenten Datenspeicher Game Center. `GKGameSession` verwaltet eine Liste der Spieler und die app ist verantwortlich für die Implementierung wie und wann Teilnehmer Datum gespeichert, abgerufen oder zwischen den Beteiligten ausgetauscht. In vielen Fällen können Spiel Sitzungen vorhandenen aktivieren basierende Übereinstimmungen, Übereinstimmungen in Echtzeit oder persistenten Spiel speichern Methoden ersetzen.

## <a name="gameplaykit-enhancements"></a>GameplayKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das GameplayKit-Framework in iOS 10 vorgenommen:

- Verwenden Sie die neue [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) Klasse, um leistungsstarke, natürlichen aussehende Pfade bereitzustellen.
- Prozedurale Rauschen Generierung wurde hinzugefügt und kann verwendet werden, um realistische in natürlicher aussehende Texturen verbessern Kamera Bewegungen realistische hinzu, und Hilfe spielwelten zu generieren.
- Die für die effiziente Suche Spiels partitioniert räumliche Partitionierung verwendet.
- Eine neue Monte Carlo-Strategie ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) für eine vollständige möglich verschieben Berechnung hinzugefügt wurde.
- 3D-Unterstützung wurde auf vorhandenen Agent und Pfad suchen mit dem neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) Klassen.
- Die neue [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) und [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Klassen stellen Kombinieren von GameplayKit und SpriteKit einfacher als je zuvor.
- Eine neue Decision Tree-API wurde hinzugefügt ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) und [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) um das Spiel-Erstellung AI zu verbessern.

## <a name="healthkit-enhancements"></a>HealthKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das HealthKit-Framework in iOS 10 vorgenommen:

- Neue Metadaten-Schlüssel für Weather Typen wurden hinzugefügt (wie z. B. `HKWeatherConditionClear` und `HKWeatherConditionCloudy`) und Trainings-Typen (z. B. `HKWorkoutActivityTypeFlexibility` und `HKWorkoutActivityTypeWheelchairRunPace`) hinzugefügt wurden.
- Die neue `HKCDADocument` Klasse zum Darstellen einer klinischen Architektur (CDA) formatiert Dokument hinzugefügt wurde.
- Verwenden Sie die neue [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) Klasse an die `ActivityType` und `LocationType` von einer Herausforderung.
- Die neue [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) und die `WheelchairUse` Methode der [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) Klasse wurden für die Arbeit mit Rollstuhl hinzugefügt Zustandsdaten beziehen.

## <a name="homekit-enhancements"></a>HomeKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das HomeKit-Framework in iOS 10 vorgenommen:

- Neue Dienste und Merkmale wurden hinzugefügt.
- Einem iPad kann fungieren als HomeKit Hub Zubehörs Remotezugriff bereitzustellen, führen die Automation-Trigger aus, und aktivieren Sie die Benutzerberechtigungen freigegebenen konfiguriert werden.
- Unterstützung wurde für Zubehör, Kamera und Haustürklingel hinzugefügt.
- Mehr Kontext und Konfigurationen wurden für Zubehör bereitgestellt.

Finden Sie in unserem [Einführung in HomeKit](~/ios/platform/homekit.md) Dokumentation weitere Informationen.

## <a name="metal-enhancements"></a>-Metal-Erweiterungen

-Metal-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- 3D apps und Spiele können jetzt _Mosaik_ komplexe Szenen und Geometrie über die GPU effizientes Rendering.
- Geben Sie eine präzisere Kontrolle der Ressourcenzuordnung zum Optimieren der Leistung von Metall auf Grundlage der Ressource Heaps mit apps und Memoryless Rendern Ziele.
- Verwenden Sie zum Erstellen einer Sammlung stark optimiert, Material und hell Kombination aus Funktionen für eine Szene Spezialisierung-Funktion.

Weitere Informationen finden Sie unter der Apple- [Metall Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>ModelIO-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das ModelIO-Framework in iOS 10 vorgenommen:

 - Das Dateiformat USD wird jetzt unterstützt.
 - Signiert Abstand Feld Unterstützung, um hinzugefügt wurde die [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) Klasse.
 - Verwenden Sie die neue `MDLLightProbeIrradianceDataSource` Klasse, um bei der Platzierung Licht Prüfpunkt zu unterstützen.
 - Verwenden Sie die neue `MDLMaterialPropertyGraph` Klasse zur Laufzeit problemlos Unterstützung für Modelle ändert.

## <a name="photos-enhancements"></a>Fotos Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf die Fotos-Framework in iOS 10 vorgenommen:

- Verwenden der [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) und [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) Klassen nutzen die neuen Core-Image Prozessorfunktion Bearbeitungen ausführen.
- Bearbeiten von Live Foto ist jetzt verfügbar für apps, die das Framework Fotos unterstützen und Erweiterungen zum Bearbeiten von Fotos (für die Verwendung innerhalb der Fotos und Kamera apps).
- Verwenden Sie die neue [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) Klasse, um die Änderungen auf das Video und weiterhin Inhalt Live Fotos anzuwenden.

## <a name="replaykit-enhancements"></a>ReplayKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das ReplayKit-Framework in iOS 10 vorgenommen:

- Verwenden der [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) und [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) Klassen zur Unterstützung von Broadcasting aufgezeichnet Medien über 3rd Party Websites.
- Die Erweiterungen Broadcast-Benutzeroberfläche und die Übertragung hochladen müssen ReplayKit 3rd Party broadcast Dienste in der app zu unterstützen.

## <a name="scenekit-enhancements"></a>SceneKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das SceneKit-Framework in iOS 10 vorgenommen:

- Die [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) Klasse kann größer realistische mit HDR-Funktionen und Effekte bereitstellen. Verwenden Sie adaptive Offenlegung zum Erstellen von automatischen Effekte oder Vignettierung verwenden, Farbsäume und Farbe Einteilung, um das Spiel Fillmatic Effekte hinzuzufügen.
- SceneKit enthält jetzt ein neue physisch basierend rendern (PBR)-System für realistischere Ergebnisse bei der einfacheren Asset authoring.
- Verwenden Sie die neue [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) Schattierung modellieren Produkt eine Breite Palette von realistische Schattierung Auswirkungen bei nur drei grundlegende Eigenschaften (`Diffuse`, `Metalness` und `Roughness`).
- Da PBR Schattierung funktioniert am besten mit einer Umgebung basierende Beleuchtung, verwendet die `LightingEnvironment` eine gesamte Szene abbildbasierte Beleuchtung zuzuweisende Eigenschaft.
- Verwenden der `IESProfileURL` Eigenschaft zum Importieren von realen Leuchten, die Beleuchtung definieren anhand von realen Werte wie z. B. Intensität (in Lumen) und Farbentemperatur (in Grad Kelvin).
- PBR und HDR Kamera bieten bessere Ergebnisse als herkömmliche-Rendering-Techniken und folglich SceneKit jetzt führt alle Farbe Berechnungen in einer linearen Farbraum (mit Farbskala P3 auf geräteanzeigen Wide-Farbe).
- SceneKit jetzt Farbe alle Farben entspricht, indem Sie die Farbe Profilinformationen zu lesen.
- SceneKit interpretiert Komponente Farbwerte in einer linearen RGB-Farbspektrum für alle Shader-Typen.
- Beide lineare Speicherplatz Rendering-Farbe aus, und Wide-Color kann deaktiviert werden, indem Sie angeben der `SCNDisableLinearSpaceRendering` und `SCNDisableWideGamut` Schlüssel in der app `Info.plist`.
- Erstellen Sie beliebige Polygon Primaten (entweder aus Dateien geladen oder programmgesteuert generiert) mit dem neuen Geometry an [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) Klasse.
- Da SceneKit liest und für Farbe-Profilinformationen in Textur Bildern anpassen, verwenden Sie Asset Kataloge für alle Abbilder, um sicherzustellen, dass diese Informationen bereitgestellt wird.

## <a name="spritekit-enhancements"></a>SpriteKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das SpriteKit-Framework in iOS 10 vorgenommen:

- Benutzerdefinierte Shadern können Attribute bereitstellen (`SKAttribute`), kann von jedem Knoten, die den Shader durch Angabe eines Attributwerts verwendet separat konfiguriert werden (`SKAttributeValue`).
- Tilemaps unterstützt nun die Kachel "quadratisch, Sechseckige und isometrische" Formen 2D, 2,5 D und Durchführen eines Bildlaufs Seite Spiele, die mit der `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet` Klassen.
- Verwenden Sie die neue `SKWarpGeometry` Klasse gestreckt oder im Simulator beeinträchtigt werden [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) oder [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) rendern. Die neue [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) Klasse zum Animieren der Übergänge zwischen Warp Effekte verwendet werden kann.
- Die [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) -Klasse stellt mehrere neue Methoden, um eine präzisere steuern, wann und wie eine Szene gerendert wird.

## <a name="scrollview-enhancements"></a>ScrollView-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden dem ScrollView-Steuerelement in iOS 10.3 vorgenommen:

- `UIScrollView` enthalten nun die `IndexDisplayMode` Eigenschaft, um steuern, wie der Index angezeigt wird, während der Benutzer als Durchführen eines Bildlaufs ist eine `UIScrollViewIndexDisplayMode` von:
    - `Automatic` -Die Index Anzeige wird vom Betriebssystem gesteuert.
    - `AlwaysHidden` -Die Index Anzeige ist immer ausgeblendet.

Finden Sie unter der [iOSTenThree Beispiel](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) für die Verwendung.

## <a name="uikit-enhancements"></a>UIKit-Erweiterungen

Im folgenden aufgeführten Erweiterungen wurden auf das UIKit-Framework in iOS 10 vorgenommen:

- Die neue [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) -API bietet neue Optionen (z. B. Einschränkungen für Lebensdauer) und wird automatisch kompatible Inhaltstypen für allgemeine Klassentypen deklariert werden.
- Neue vollständig interaktiven, objektbasierte und parallelisierbar Animation-Unterstützung hinzugefügt wurde und mit Gesten verknüpft werden kann. Pleas Siehe Apple des [UIViewAnimating Protokollreferenz](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator-Klassenreferenz](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protokollreferenz](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters-Klassenreferenz](https://developer.apple.com/reference/uikit/uicubictimingparameters) und [UISpringTimingParameter-Klassenreferenz](https://developer.apple.com/reference/uikit/uispringtimingparameters) für Weitere Informationen.
- Die neue `UIPreviewInteraction` und `UIPreviewInteractionDelegate` kann die app Entwickler eine benutzerdefinierte Schnittstelle Peek / "und" Pop-Vorgänge bereit.
- Die neue `UIAccessibilityCustomRotor` Klasse ermöglicht der app, die benutzerdefinierte, kontextspezifische Funktionalität für hilfstechnologien wie Voice Over bereitstellen.
- Verwenden der `UIAccessibilityIsAssistiveTouchRunning` und `UIAccessibilityAssistiveTouchStatusDidChangeNotification` Symbole, um festzustellen, ob AssistiveTouch aktiviert ist.
- Verwenden der `UIAccessibilityHearingDevicePairedEar` und `UIAccessibilityHearingDevicePairedEarDidChangeNotification` Symbole zum Abrufen des Status eines beliebigen gekoppelt Konfiguration hörgeschädigt sind Hilfsmittel.
- Zur Unterstützung von dynamischen Typ in Bezeichnungen Textfelder und Textfelder verwenden Sie die neue `PreferredFontForTextStyle` Methode der `UIFont` Klasse.
- Entscheiden, ob ein Element, dessen Schriftart aktualisieren sollten bei der des Geräts `UIContentSizeCategory` ändert, verwenden die `AdjustsFontForContentSizeCategory` Eigenschaft der `UIContentSizeCategoryAdjusting` delegieren.
- Die `OpenURL` Methode der `UIApplication` Klasse asynchron aufgerufen wird, und unterstützt nun eine Abschlusshandler, die aufgerufen wird, nachdem die open-Aktion abgeschlossen wurde.
- Initiieren Sie die CloudKit freigeben und ändern Sie seine Eigenschaften mit dem neuen `UICloudSharingController` und `UICloudSharingControllerDelegate` Klassen.
- Nutzen von Prefetch Zellen zur Verbesserung der Bildlauf der `UICollectionViews` mit dem neuen `UICollectionViewDataSourcePrefetching` delegieren.
- Entwickler kann nun die Darstellung von das Signal für Leiste Registerkartenelemente (z. B. Text und Hintergrund) steuern.
- Das Steuerelement aktualisieren wird jetzt in allen Rollen anzeigen und Scroll Ansicht Unterklassen unterstützt (z. B. `UICollectionView`).

## <a name="webkit-enhancements"></a>WebKit-Erweiterungen

Die folgenden Verbesserungen wurden für den WebKit-Framework in iOS-10 vorgenommen:

- Peek / "und" pop-Unterstützung hinzugefügt wurde die `WKWebView` Klasse. Verwenden der `ShouldPreviewElement` Methode, um zu bestimmen, ob eine bestimmte Webansicht eine Vorschau angezeigt werden soll.


## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Was ist neu in iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
