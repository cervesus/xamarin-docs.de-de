---
title: Weitere iOS 10-Frameworks Änderungen
description: In diesem Dokument wird beschrieben, kleinere Änderungen und Verbesserungen bei den vorhandenen Frameworks unter iOS 10 und erläutert, wie Sie diese Updates in Xamarin.iOS verwendet.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: a7b029aad69e65192d48d969dba2b9bb9a0d7a50
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364296"
---
# <a name="additional-ios-10-frameworks-changes"></a>Weitere iOS 10-Frameworks Änderungen

_Dieser Artikel enthält zusätzliche, kleinere Änderungen oder Verbesserungen an vorhandenen Frameworks für iOS 10._

## <a name="av-foundation-framework-additions"></a>AV-Foundation-Framework-Erweiterungen

Das Framework AVFoundation umfasst folgende Erweiterungen:

- In iOS 10, der Entwickler nicht mehr hat verschiedene implementieren [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) Verhalten basierend auf Inhaltstyp. Legen Sie einfach die `Rate` -Eigenschaft und AVFoundation bestimmt Wenn genügend Inhalte ohne treten für die Wiedergabe verfügbar ist.
- Die neue [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) Klasse ersetzt die veraltete `AVCaptureStillImageOutput` -Klasse und stellt eine einheitliche Methode zum Verarbeiten von allen Fotografie Workflows anspruchsvolle Kontrolle und Überwachung von der Capture-Prozess und Unterstützung für neue Features wie Live Fotos und das RAW-Capture-Format.
- Die neue `AVPlayerLooper` Klasse macht es einfacher, einen bestimmten Media während der Wiedergabe in einer Schleife.
- Die `AVAssetDownloadURLSession` Klasse ermöglicht das Herunterladen und spätere Wiedergabe von FairPlay verschlüsselte HLS-Datenströmen.
- In der Standardeinstellung die [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) Klasse automatisch Wide-Color "," Wide-Farbskala Erfassung unterstützt, wenn die Hardware der Geräte unterstützt. Finden Sie unter Apple [iOS Device Compatibility Referenz](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) Weitere Details.

## <a name="avkit-additions"></a>AVKit-Erweiterungen

Das Framework AVKit enthält jetzt den neuen `UpdatesNowPlayingInfoCenter` Eigenschaft, um anzugeben, wenn die jetzt spielen Info-Center aktualisiert werden sollen.

## <a name="core-data-enhancements"></a>Core-Data-Erweiterungen

iOS 10 umfasst folgende Erweiterungen für die Kerndaten-Framework:

- Die [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) Objekte mit SQLite-Datenspeicher in die WAL-Journal-Modus-Unterstützung der neuen Abfrage Generation Funktion, in denen verwaltete Objekt Kontexten (MOC) für bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen an.
- Stamm [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) -Objekte unterstützt gleichzeitige Fehlerbehandlung und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) Klasse verwaltet einen Pool mit SQLite-Datenspeicher.
- Mehrere neue Hilfsmethoden hinzugefügt wurden `NSManagedObject` abrufen und Erstellen von Unterklassen zu vereinfachen.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) und anderen Konfigurationsressourcen Core Data.

Weitere Informationen finden Sie unter Apple [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Core-Image-Verbesserungen

iOS 10 macht das Framework Core-Image die folgenden Erweiterungen:

- Entwickler kann jetzt Bilder in einem Bereich der Farbe außerhalb des Kontexts der Core-Image arbeiten-Farbraum verarbeiten, durch die Konvertierung in und aus des Farbraums vor und nach der Verarbeitung.
- Für iOS-Geräte, die A8 oder A9-CPUs verwenden, wird das RAW-Image-Format wird jetzt unterstützt. Core-Image bietet nun Unterstützung für das Decodieren der RAW-Bilder entweder der integrierten iSight Kamera oder von einer 3rd Party Kamera. Verwenden der `FilterWithImageData` oder `FilterWithImageURL` Methoden der [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) -Klasse zum Verarbeiten von RAW-Bilder.
- Verschiedene Verbesserungen der Rendering-Leistung wurden vorgenommen, um `UIImage` rendern (wenn von Core-Image-Image-Speicher unterstützt wird) in `UIImageView` Objekte. 
- `UIImage` Objekte mit Tags Wide-Farbskala rendert als Breitzeichen-Farbskala Farbe im `UIImageView` Objekte auf iOS-Geräten, die die Breite Farbskala unterstützen.
- Core-Image-Kernelcode kann jetzt bestimmte Ausgabe Pixelformate anfordern.
- Die `ImageWithExtent` Methode der [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) Klasse kann verwendet werden, um die benutzerdefinierte Verarbeitung in der Filtervorgang einzufügen. Core-Image wird Ruft den angegebenen Rückruf zwischen filtern, wenn ein Bild für die Ausgabe verarbeitet oder angezeigt werden.

Darüber hinaus wurden die folgenden neuen Core-Image-Filter hinzugefügt:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Core-Motion-Erweiterungen

Neue IOS 10, enthält die Motion-Core-Framework Schrittzähler-Ereignisse, die eine app zum Empfangen von in Echtzeit Benachrichtigungen über den Benutzer, Anhalten und Fortsetzen der nachverfolgung während der Ausführung schneller, aktivieren. Verwenden der [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) für Vorder- oder Hintergrund Schrittzähler Ereignisse registrieren.

## <a name="foundation-enhancements"></a>Foundation-Verbesserungen

Der Foundation-Framework für iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) Klasse, um lokalisierte Messungen für die Anzeige für den Endbenutzer zu formatieren.
- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse um Datum und Uhrzeit-Intervall Berechnungen wie Dauer, für den Vergleich von Abständen, und für die Interval-Schnittpunkte zu testen.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) -Klasse zum Konvertieren zwischen verschiedenen Einheiten von Measures (Maßeinheit), oder führen Berechnungen auf den Werten in verschiedenen UOMs.

- Verwenden Sie die neue [NSUnit](https://developer.apple.com/reference/foundation/nsunit) und [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) Klassen zum Darstellen von bestimmten UOMs.
- Mehrere neue Eigenschaften hinzugefügt wurden die [NSLocal](https://developer.apple.com/reference/foundation/nslocale) Klasse, um lokale Informationen und die verfügbaren Anzeigeformate abzurufen.

## <a name="gamekit-enhancements"></a>GameKit-Verbesserungen

Das Framework GameKit unter iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Die **Game Center-App** als veraltet markiert und aus iOS entfernt wurde. Wenn die app GameKit, verwendet es _müssen_ eine eigene Benutzeroberfläche zum Anzeigen von GameKit Features wie Bestenlisten usw. vorhanden. 
- Ein neues nur iCloud-Konto des Typs wurde implementiert, durch die [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) Klasse.
- Die neue [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) Klasse stellt eine allgemeine Lösung zum Verwalten von persistenten Datenspeicher auf der Game Center bereit. `GKGameSession` verwaltet eine Liste der Spieler und die app ist verantwortlich für die Implementierung wie und wann Teilnehmer Datum gespeichert, abgerufen oder zwischen den Beteiligten ausgetauscht. In vielen Fällen können Spiele Sitzungen vorhandenen Turn-basierte Übereinstimmungen, die in Echtzeit Übereinstimmungen oder persistenten Spiel speichern Methoden ersetzen.

## <a name="gameplaykit-enhancements"></a>GameplayKit-Verbesserungen

Das Framework GameplayKit unter iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Verwenden Sie die neue [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) Klasse, um leistungsstarke, naturgetreue Pfade bereitzustellen.
- Prozedurale Rauschen Generation hinzugefügt wurde und dienen zum verbessern die wirklichkeitsgetreuen Bilder in Texturen naturgetreue Bewegungen der Kamera wirklichkeitsgetreuen Bilder hinzu, und helfen anspruchsvoller spielwelten zu generieren.
- Verwenden Sie die räumliche Partitionierung, um die Welt, die Daten für die effiziente Suche zu partitionieren.
- Eine neue Monte-Carlo-Strategist ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) für die vollständige möglich verschieben Berechnung hinzugefügt wurde.
- 3D-Unterstützung hinzugefügt wurde vorhandenen Agent "und" Pfad Auffinden von Verhaltensweisen, die mit dem neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) Klassen.
- Die neue [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) und [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Klassen stellen GameplayKit und einfacher als je zuvor SpriteKit kombiniert.
- Eine neue Decision Tree-API wurde hinzugefügt ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) und [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) um das Spiel zum Erstellen von AI zu verbessern.

## <a name="healthkit-enhancements"></a>HealthKit-Verbesserungen

Das Framework "HealthKit" unter iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Neue Metadatenschlüssel für das Wetter-Typen hinzugefügt wurden (z. B. `HKWeatherConditionClear` und `HKWeatherConditionCloudy`) und Trainings-Typen (z. B. `HKWorkoutActivityTypeFlexibility` und `HKWorkoutActivityTypeWheelchairRunPace`) wurden hinzugefügt.
- Die neue `HKCDADocument` Klasse zur Darstellung einer Clinical Architektur (CDA) formatiert Dokument hinzugefügt wurde.
- Verwenden Sie die neue [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) Klasse an die `ActivityType` und `LocationType` von einer Herausforderung.
- Die neue [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) und `WheelchairUse` -Methode der der [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) Klasse für die Arbeit mit Rollstuhl hinzugefügt wurden integritätsdaten beziehen.

## <a name="homekit-enhancements"></a>HomeKit-Verbesserungen

Das HomeKit-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Neue Dienste und Eigenschaften wurden hinzugefügt.
- Einem iPad kann konfiguriert werden wie ein HomeKit-Hub zum Bereitstellen des Remotezugriffs Zubehör, Ausführen von Automation-Trigger aus, und Aktivieren von Benutzerberechtigungen freigegeben.
- Unterstützung wurde für Zubehör, Kamera und türklingel hinzugefügt.
- Mehr Kontext und Konfigurationen wurden für Accessories bereitgestellt.

Informieren Sie sich unsere [Einführung in HomeKit](~/ios/platform/homekit.md) Dokumentation zu informieren.

## <a name="metal-enhancements"></a>Metal-Verbesserungen

Das-Metal-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- 3D-Apps und spielen können Sie jetzt _Mosaik_ komplexe Szenen und "Geometry" über die GPU effizientes Rendering.
- Geben Sie, dass eine präzisere Kontrolle ressourcenzuteilung zum Optimieren der Leistung aus Metall-apps mithilfe von Resource Heaps und Memoryless Rendern Ziele basierend.
- Verwenden Sie zum Erstellen einer hochgradig optimierten Sammlung von Material und Licht Kombination-Funktionen für eine Szene Funktion Spezialisierung.

Weitere Informationen finden Sie unter Apple [Metall Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>ModelIO-Verbesserungen

Das Framework ModelIO unter iOS 10 wurden die folgenden Verbesserungen vorgenommen:

 - Das US-Dollar-Dateiformat wird jetzt unterstützt.
 - Signiert von Feld ' Abstand ' Unterstützung hinzugefügt wurde die [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) Klasse.
 - Verwenden Sie die neue `MDLLightProbeIrradianceDataSource` Klasse zur Unterstützung bei der Platzierung Licht Prüfpunkt.
 - Verwenden Sie die neue `MDLMaterialPropertyGraph` Klasse zur Laufzeit problemlos Unterstützung für Modelle ändert.

## <a name="photos-enhancements"></a>Verbesserungen von Fotos

Das Framework Fotos in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Verwenden der [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) und [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) Klassen nutzen das neue Feature der Core-Image-Prozessor zu bearbeiten.
- Foto bearbeiten ist jetzt verfügbar, für apps, die Unterstützung der Fotos-Framework und Erweiterungen zum Bearbeiten von Fotos (für die Verwendung innerhalb von Fotos und Kamera apps).
- Verwenden Sie die neue [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) Klasse, um die Änderungen auf das Video und weiterhin Inhalt Live Fotos gelten.

## <a name="replaykit-enhancements"></a>ReplayKit-Verbesserungen

Das Framework ReplayKit unter iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Verwenden der [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) und [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) Klassen zur Unterstützung von Übertragungen von aufgezeichneten Medien durch 3rd Party Websites.
- Die Broadcast-Benutzeroberfläche und Broadcast Upload-Erweiterungen sind erforderlich, um per broadcast ReplayKit 3rd Party-Dienste in der app zu unterstützen.

## <a name="scenekit-enhancements"></a>SceneKit-Verbesserungen

Das SceneKit-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Die [SCNCamera](xref:SceneKit.SCNCamera) Klasse kann mehr Realismus verleihen bereitstellen, mithilfe von HDR-Funktionen und Effekte. Verwenden Sie adaptive Offenlegung, zum Erstellen von automatischen Effekte oder Vignettierung verwenden, Farbsäume und farbliche Abstufung, um das Spiel Fillmatic Effekte hinzuzufügen.
- SceneKit enthält jetzt ein neues physisch basierten rendern (PBR)-System für realistischer Ergebnisse bei der einfacheren Asset erstellen.
- Verwenden Sie die neue [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) Schattierung modellieren Produkt eine Vielzahl von realistische Schattierungseffekte erforderlich ist nur für die drei grundlegende Eigenschaften (`Diffuse`, `Metalness` und `Roughness`).
- Seit PBR funktioniert am besten mit Beleuchtung umgebungsbasierte Schattierung, verwenden Sie die `LightingEnvironment` Eigenschaft, um eine gesamte Szene abbildbasierte Beleuchtung zuweisen.
- Verwenden der `IESProfileURL` -Eigenschaft realen lichtinstallationen importieren, die Beleuchtung definieren basierend auf realen-Werte, z. B. Intensität (in Lumen) und Farbtemperatur (in Grad Kelvin).
- Sowohl für PBR HDR-Kamera-Features bieten bessere Ergebnisse als herkömmliche-Rendering-Techniken und daher SceneKit jetzt führt alle Farbe Berechnungen in einer linearen Farbraum (Wide-Color geräteanzeigen unter Verwendung Farbskala P3).
- SceneKit nun Farbe alle Farben entspricht, indem die Farbinformationen für das Profil zu lesen.
- SceneKit interpretiert Farbkomponentenwerte in einer linearen RGB-Farbspektrum für alle Arten von Shader.
- Sowohl lineare als Farbe, die Leerzeichen-Rendering und Wide-Color kann deaktiviert werden, durch Angabe der `SCNDisableLinearSpaceRendering` und `SCNDisableWideGamut` Schlüssel in der app `Info.plist`.
- Erstellen Sie beliebige Polygon Primaten gemeinsam (entweder aus Dateien geladen oder programmgesteuert generiert) mit dem neuen Geometrie an [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) Klasse.
- Da SceneKit liest und für die Color-Profilinformationen in der Textur Bilder anpassen, verwenden Sie Asset-Katalogs für alle Bilder, um sicherzustellen, dass diese Informationen bereitgestellt werden.

## <a name="spritekit-enhancements"></a>SpriteKit-Verbesserungen

Das SpriteKit-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Benutzerdefinierte Shadern können Attribute bereitstellen (`SKAttribute`), können von jedem Knoten, die den Shader verwendet wird, durch Angabe eines Attributwerts separat konfiguriert werden (`SKAttributeValue`).
- Tilemaps unterstützen jetzt die Kachel "Square", "hexagonal" und "isometrischen" Formen für 2D, 2,5D und Seite elementscrolling Spiele, die mit dem die `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` und `SKTileSet` Klassen.
- Verwenden Sie die neue `SKWarpGeometry` Klasse gestreckt oder verzerrt [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) oder [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) rendern. Die neue [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) Klasse kann verwendet werden, um die Übergänge zwischen Warp-Effekte zu animieren.
- Die [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) Klasse bietet mehrere neue Methoden, um eine präzise steuern, wann und wie eine Szene gerendert wird.

## <a name="scrollview-enhancements"></a>ScrollView-Verbesserungen

Die folgenden Verbesserungen wurden dem ScrollView-Steuerelement in iOS 10.3 vorgenommen:

- `UIScrollView` umfassen jetzt die `IndexDisplayMode` Eigenschaft, um steuern, wie der Index angezeigt wird, während der Benutzer als Durchführen eines Bildlaufs ist eine `UIScrollViewIndexDisplayMode` von:
    - `Automatic` -Die Index-Anzeige wird vom Betriebssystem gesteuert.
    - `AlwaysHidden` -Die Index-Anzeige ist immer ausgeblendet.

Finden Sie unter den [iOSTenThree Beispiel](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) für die Nutzung.

## <a name="uikit-enhancements"></a>UIKit-Verbesserungen

Das UIKit-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Die neue [UIPasteboard](xref:UIKit.UIPasteboard) -API bietet neue Optionen (z. B. Einschränkungen für Lebensdauer) und wird automatisch kompatible Inhaltstypen für allgemeine Klassentypen deklariert.
- Neue Unterstützung für hochgradig interaktive, objektbasierten, unterbrechbar Animation hinzugefügt wurde und auf Gesten verknüpft werden kann. Informieren Sie sich von Apple [UIViewAnimating-Protokollreferenz](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator Class Reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider-Protokollreferenz](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters Class Reference](https://developer.apple.com/reference/uikit/uicubictimingparameters) und [UISpringTimingParameter Class Reference](https://developer.apple.com/reference/uikit/uispringtimingparameters) für Weitere Informationen.
- Die neue `UIPreviewInteraction` und `UIPreviewInteractionDelegate` können die Entwickler-app eine benutzerdefinierte Schnittstelle für Peek "und" Pop-Vorgänge bereitstellen.
- Die neue `UIAccessibilityCustomRotor` Klasse ermöglicht der app, benutzerdefinierte, Kontext-spezifische Funktionalität für hilfstechnologien wie etwa Voice Over bereitzustellen.
- Verwenden der `UIAccessibilityIsAssistiveTouchRunning` und `UIAccessibilityAssistiveTouchStatusDidChangeNotification` Symbole verwenden, um zu bestimmen, ob AssistiveTouch aktiviert ist.
- Verwenden der `UIAccessibilityHearingDevicePairedEar` und `UIAccessibilityHearingDevicePairedEarDidChangeNotification` Symbole zum Abrufen des Status aller gekoppelten MFi hören Hilfsmittel.
- Zur Unterstützung von Typ "Dynamic" in Bezeichnungen Textfelder und Text-Felder verwenden Sie die neue `PreferredFontForTextStyle` Methode der `UIFont` Klasse.
- Entscheiden, ob ein Element, dessen Schriftart aktualisieren sollten beim des Geräts `UIContentSizeCategory` ändert, verwenden die `AdjustsFontForContentSizeCategory` Eigenschaft der `UIContentSizeCategoryAdjusting` delegieren.
- Die `OpenURL` Methode der `UIApplication` Klasse asynchron aufgerufen wird, und unterstützt jetzt einen Abschlusshandler, die aufgerufen wird, nach dem Abschluss der Aktion geöffneten.
- Freigeben von CloudKit zu initiieren und ändern Sie seine Eigenschaften, die mit dem neuen `UICloudSharingController` und `UICloudSharingControllerDelegate` Klassen.
- Nutzen Sie vorab abgerufene Zellen der Bildlauf bieten Verbesserungen `UICollectionViews` mit dem neuen `UICollectionViewDataSourcePrefetching` delegieren.
- Entwickler kann jetzt steuern, die Darstellung für den Badge für Registerkartenelemente (z. B. die Farbe für Text und Hintergrund).
- Das Aktualisierungssteuerelement wird jetzt in allen scrollansicht und führen Sie einen Bildlauf Ansicht Unterklassen unterstützt (z. B. `UICollectionView`).

## <a name="webkit-enhancements"></a>WebKit-Verbesserungen

Das WebKit-Framework in iOS 10 wurden die folgenden Verbesserungen vorgenommen:

- Peek "und" pop-Unterstützung hinzugefügt wurde die `WKWebView` Klasse. Verwenden der `ShouldPreviewElement` Methode, um zu bestimmen, ob eine angegebene Webansicht eine Vorschau angezeigt werden soll.


## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Neuigkeiten in iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
