---
title: Weitere Änderungen an IOS 10-Frameworks
description: In diesem Dokument werden kleinere Änderungen und Verbesserungen an vorhandenen Frameworks in ios 10 beschrieben. Außerdem wird erläutert, wie Sie diese Updates in xamarin. IOS verwenden können.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 620b89ba4682d65552fa5555c978b7eb5f437714
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290775"
---
# <a name="additional-ios-10-frameworks-changes"></a>Weitere Änderungen an IOS 10-Frameworks

_In diesem Artikel werden zusätzliche, kleinere Änderungen oder Verbesserungen an vorhandenen Frameworks für IOS 10 behandelt._

## <a name="av-foundation-framework-additions"></a>Erweiterungen des AV Foundation-Frameworks

Das AVFoundation-Framework umfasst die folgenden Verbesserungen:

- In ios 10 muss der Entwickler nicht mehr unterschiedliche [avplayeritem](xref:AVFoundation.AVPlayerItem) -Verhaltensweisen basierend auf dem Inhaltstyp implementieren. Legen Sie einfach `Rate` die-Eigenschaft fest, und AVFoundation bestimmt, wann genug Inhalt für die Wiedergabe verfügbar ist, ohne Sie zu warten.
- Die neue [avcapturephotooutput](xref:AVFoundation.AVCaptureFileOutput) -Klasse ersetzt die veraltete `AVCaptureStillImageOutput` Klasse und stellt eine einheitliche Methode für die Verarbeitung aller Bild Verarbeitungs Workflows bereit, indem Sie eine ausgereifte Kontrolle und Überwachung des Aufzeichnungsprozesses und der Unterstützung für neue Funktionen wie livefotos und das RAW-Erfassungs Format.
- Die neue `AVPlayerLooper` -Klasse vereinfacht die Schleife eines bestimmten Mediums während der Wiedergabe.
- Die `AVAssetDownloadURLSession` -Klasse ermöglicht das herunterladen und spätere Wiedergabe von Fairplay-verschlüsselten HLS-Datenströmen.
- Standardmäßig unterstützt die [avcapturesession](xref:AVFoundation.AVCaptureSession) -Klasse automatisch eine breit farbige und breit gefasste Erfassung, wenn Sie von der Gerätehardware unterstützt wird. Weitere Informationen finden Sie in der [Referenz zur IOS-Gerätekompatibilität](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) von Apple.

## <a name="avkit-additions"></a>Avkit-Ergänzungen

Das avkit-Framework enthält jetzt die `UpdatesNowPlayingInfoCenter` neue-Eigenschaft, die angibt, wann das jetzt wiedergegebene Info Center aktualisiert werden soll.

## <a name="core-data-enhancements"></a>Erweiterungen der Kern Daten

IOS 10 umfasst die folgenden Erweiterungen für das Core Data Framework:

- Die [nsmanagedobjectcontext](xref:CoreData.NSManagedObjectContext) -Objekte mit SQLite-Daten speichern im Wal-Journal Modus unterstützen die neue Abfrage Generierungs Funktion, bei der verwaltete Objekt Kontexte (Managed Object Context, MUC) an bestimmte Daten Bank Versionen angeheftet werden können, um zukünftige Abruf-und fehlerverarbeitungs Vorgänge durchzusetzen
- Stamm- [nsmanagedobjectcontext](xref:CoreData.NSManagedObjectContext) -Objekte unterstützen das gleichzeitige fehlschlagen und Abrufen ohne Serialisierung.
- Die [nspersistentstorecoordinator](xref:CoreData.NSPersistentStoreCoordinator) -Klasse verwaltet einen Pool mit SQLite-Daten speichern.
- Es wurden mehrere neue Hilfsmethoden hinzugefügt `NSManagedObject` , um die Ausführung von Abrufe und das Erstellen von Unterklassen zu vereinfachen.
- Verwenden Sie die allgemeine Ebene `NSPersistenceContainer` , um auf `NSPersistentStoreCoordinator`das, [nsmanagedobjectmodel](xref:CoreData.NSManagedObjectModel) und andere Kern Daten Konfigurations Ressourcen zu verweisen.

Weitere Informationen finden Sie in der [Core Data Framework-Referenz](https://developer.apple.com/reference/coredata)von Apple.

## <a name="core-image-enhancements"></a>Erweiterungen des Kern Images

IOS 10 nimmt die folgenden Verbesserungen am Core-Image-Framework vor:

- Der Entwickler kann nun Bilder in einem Farbraum außerhalb des Arbeits Farbraums des Kern Bild Kontexts verarbeiten, indem er vor und nach der Verarbeitung in den und aus dem Farb Raum umrechnet.
- Für IOS-Geräte, die A8-oder A9-CPUs verwenden, wird das Rohbild Format jetzt unterstützt. Das Core-Image bietet jetzt Unterstützung für das Decodieren von rohimages von der integrierten iSight-Kamera oder von einer Drittanbieter Kamera. Verwenden Sie `FilterWithImageData` die `FilterWithImageURL` -Methode oder die-Methode der [cifilter](xref:CoreImage.CIFilter) -Klasse zum Verarbeiten von Rohdaten.
- Es wurden mehrere renderingleistungserweiterungen für `UIImage` das Rendern von `UIImageView` Objekten (bei der Unterstützung durch Image Speicher-Kern Images) 
- `UIImage`-Objekte, die als Wide-Gamut gekennzeichnet sind, werden in `UIImageView` Objekten auf IOS-Geräten, die Breite Farben unterstützen, als Breite Farbe dargestellt.
- Kernbild-Kernelcode kann jetzt bestimmte Pixel Ausgabeformate anfordern.
- Die `ImageWithExtent` -Methode der [cifilter](xref:CoreImage.CIFilter) -Klasse kann verwendet werden, um eine benutzerdefinierte Verarbeitung in den Filter Vorgang einzufügen. Das Core-Image Ruft den gegebenen Rückruf zwischen Filtern auf, wenn ein Bild für die Ausgabe oder Anzeige verarbeitet wird.

Außerdem wurden die folgenden neuen Kern Bild Filter hinzugefügt:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Grundlegende Bewegungs Ergänzungen

Neu bei IOS 10, das Core Motion Framework umfasst Peer Domain-Ereignisse, die einer APP ermöglichen, schnelle Echt Zeit Benachrichtigungen zu empfangen, während Sie ausgeführt werden und die Nachverfolgung angehalten wird. Verwenden Sie [cmpedometer](xref:CoreMotion.CMPedometer) , um für Vordergrund-oder hintergrundpeer-Peer-Ereignisse zu registrieren.

## <a name="foundation-enhancements"></a>Foundation-Erweiterungen

Die folgenden Verbesserungen wurden an Foundation Framework für IOS 10 vorgenommen:

- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)-Klasse, um lokalisierte Messungen zu formatieren, die dem Endbenutzer angezeigt werden sollen.
- Verwenden Sie die neue [nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval) -Klasse, um Datums-und Zeitintervall Berechnungen zu erstellen, z. b. Dauer Zeiten, zum Vergleichen von Intervallen und zum Testen von Intervall interabschnitten.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)-Klasse, um zwischen verschiedenen Maßeinheiten (Unit of Measure, UOM) zu konvertieren oder Berechnungen für Werte in verschiedenen UOMS auszuführen.

- Verwenden Sie die neuen [nsunit](https://developer.apple.com/reference/foundation/nsunit) -und [nsdimension](https://developer.apple.com/reference/foundation/nsdimension) -Klassen, um bestimmte UOMS darzustellen.
- Der [nslocal](https://developer.apple.com/reference/foundation/nslocale) -Klasse wurden mehrere neue Eigenschaften hinzugefügt, um lokale Informationen und die verfügbaren Anzeige Formate abzurufen.

## <a name="gamekit-enhancements"></a>GameKit-Verbesserungen

Die folgenden Verbesserungen wurden an dem GameKit-Framework in ios 10 vorgenommen:

- Die **Game Center-App** wurde als veraltet markiert und von Ios entfernt. Wenn die APP GameKit verwendet, _muss_ Sie eine eigene Schnittstelle zum Anzeigen von GameKit-Features, z. b. Bestenlisten usw., enthalten. 
- Ein neuer icloud-Kontotyp wurde von der [gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) -Klasse implementiert.
- Die neue Klasse " [gkgamesession](https://developer.apple.com/reference/gamekit/gkgamesession) " stellt eine generalisierte Lösung für die Verwaltung von persistenten Daten speichern auf Game Center bereit. `GKGameSession`verwaltet eine Liste von Playern, und die APP ist dafür verantwortlich, wie und wann das Teilnehmer Datum gespeichert, abgerufen oder zwischen den Playern ausgetauscht wird. In vielen Fällen können Spielsitzungen vorhandene Turn-basierte Übereinstimmungen, Echt Zeit Übereinstimmungen oder persistente Spiel Speichermethoden ersetzen.

## <a name="gameplaykit-enhancements"></a>Gameplaykit-Erweiterungen

Die folgenden Verbesserungen wurden an dem gameplaykit-Framework in ios 10 vorgenommen:

- Verwenden Sie die neue Klasse " [gkmeshgraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) ", um Hochleistungs Pfade mit hoher Leistung bereitzustellen.
- Das Generieren von Prozeduren wurde hinzugefügt und kann verwendet werden, um den Realismus in Natur orientierten Texturen zu verbessern, den Bewegungsmöglichkeiten der Kamera zu verleihen und umfassende Spielwelten zu generieren.
- Verwenden Sie räumliche Partitionierung, um die spielworld-Daten für eine effiziente Suche zu partitionieren
- Für eine vollständige Verschiebungs Berechnung wurde ein neuer Monte Carlo-Stratege ("[gkmontecarlostrateg;](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)") hinzugefügt.
- die 3D-Unterstützung wurde vorhandenen Agent-und Pfad Suchverhalten mithilfe der neuen [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) -und [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) -Klassen hinzugefügt.
- Die neuen Klassen " [gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) " und " [gksknodecomponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) " vereinfachen das Kombinieren von gameplaykit und spritekit.
- Es wurde eine neue Decision Tree-API hinzugefügt ("[gkdecisiontree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) " und " [gkdecisionnode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)"), um die KI der Spiel Bildung zu verbessern.

## <a name="healthkit-enhancements"></a>Healthkit-Erweiterungen

Die folgenden Verbesserungen wurden am healthkit-Framework in ios 10 vorgenommen:

- Neue Metadatenschlüssel wurden für Wetter Typen ( `HKWeatherConditionClear` z. b. und `HKWeatherConditionCloudy`) hinzugefügt, und es `HKWorkoutActivityTypeFlexibility` wurden `HKWorkoutActivityTypeWheelchairRunPace`Trainings Typen (z. b. und) hinzugefügt.
- Die neue `HKCDADocument` -Klasse wurde hinzugefügt, um ein Dokument mit einer klinischen Dokument Architektur (CDA) darzustellen.
- Verwenden Sie die neue [hkworkoutconfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) `ActivityType` -Klasse, um `LocationType` und für ein Training anzugeben.
- Das neue [hkwheelchairuseobject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) und die `WheelchairUse` -Methode der [hkhealthstore](https://developer.apple.com/reference/healthkit/hkhealthstore) -Klasse wurden zum Arbeiten mit den im Rollstuhl bezogenen Integritäts Daten hinzugefügt.

## <a name="homekit-enhancements"></a>Homekit-Erweiterungen

Die folgenden Verbesserungen wurden an dem homekit-Framework in ios 10 vorgenommen:

- Es wurden neue Dienste und Eigenschaften hinzugefügt.
- Ein iPad kann so konfiguriert werden, dass es als homekit-Hub fungiert, um Remote Zubehör Zugriff zu ermöglichen, Automatisierungs Trigger auszuführen und freigegebene Benutzerberechtigungen zu aktivieren.
- Unterstützung für Kamera-und Türglocken Zubehör hinzugefügt.
- Es wurden weitere Kontext-und Konfigurationsoptionen für Zubehör bereitgestellt.

Weitere Informationen finden Sie [in der Einführung in die homekit](~/ios/platform/homekit.md) -Dokumentation.

## <a name="metal-enhancements"></a>Metal-Erweiterungen

Die folgenden Verbesserungen wurden am Metal-Framework in ios 10 vorgenommen:

- 3D-apps und-Spiele können jetzt _Mosaik verwendet werden_ , um komplexe Szenen und Geometrie effizient über die GPU zu erzeugen.
- Sorgen Sie für eine differenzierte Steuerung der Ressourcen Zuordnung, um die Leistung von Metal-basierten Apps mithilfe von Ressourcen Heaps und Speicher losen Renderingzielen zu optimieren.
- Verwenden Sie die Funktions Spezialisierung, um eine hochgradig optimierte Sammlung von Material-und Light-Kombinations Funktionen für eine Szene zu erstellen.

Weitere Informationen finden Sie im [Handbuch zur Metal-Programmierung](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)von Apple.

## <a name="modelio-enhancements"></a>Verbesserungen bei der Verbesserung

Die folgenden Verbesserungen wurden am "mudelio"-Framework in ios 10 vorgenommen:

- Das Dateiformat "USD" wird jetzt unterstützt.
- Die Unterstützung für unterstützte Entfernungs Felder wurde der [mdlvoxelarray](https://developer.apple.com/reference/modelio/mdlvoxelarray) -Klasse hinzugefügt.
- Verwenden Sie die `MDLLightProbeIrradianceDataSource` neue-Klasse, um bei der Platzierung von Licht zu helfen.
- Verwenden Sie die `MDLMaterialPropertyGraph` neue-Klasse, um Lauf Zeit Änderungen von Modellen problemlos zu unterstützen.

## <a name="photos-enhancements"></a>Fotos Erweiterungen

Die folgenden Verbesserungen wurden am Photos-Framework in ios 10 vorgenommen:

- Verwenden Sie die Klassen " [ciimageprocessorinput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) " und " [ciimageprocessoroutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) ", um das neue Feature "Core Image Processor" zum Ausführen von Änderungen zu nutzen.
- Die Live-Fotobearbeitung steht nun für Apps zur Verfügung, die das Foto Framework und die Bearbeitung von Fotos unterstützen (für die Verwendung in den Fotos und Kamera-Apps).
- Verwenden Sie die neue [phlivephorteeditingcontext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) -Klasse, um Änderungen auf das Video und den Inhalt von livefotos anzuwenden.

## <a name="replaykit-enhancements"></a>Replaykit-Erweiterungen

Die folgenden Verbesserungen wurden an dem replaykit-Framework in ios 10 vorgenommen:

- Verwenden Sie die Klassen [rpscreenrecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [rpbroadcastactivityviewcontroller](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) und [rpbroadcastcontroller](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) , um die Übertragung von aufgezeichneten Medien über Websites von Drittanbietern zu unterstützen.
- Die Broadcast-UI und die Broadcast Upload Erweiterungen sind erforderlich, um die Broadcast Dienste von replaykit-Drittanbietern in der APP zu unterstützen.

## <a name="scenekit-enhancements"></a>Scenekit-Erweiterungen

Die folgenden Verbesserungen wurden an dem scenekit-Framework in ios 10 vorgenommen:

- Mit den HDR-Features und-Effekten kann die [scncamera](xref:SceneKit.SCNCamera) -Klasse einen besseren Realismus bereitstellen. Verwenden Sie Adaptive verfügbar machung, um automatische Effekte zu erstellen, oder verwenden Sie die Werte für die Verwendung von "vignetup", "Color Fringing" und "Color", um
- Scenekit enthält jetzt ein neues physisch basiertes Rendering (PBR)-System für realistischere Ergebnisse mit einfacherer Erstellung von Medienobjekten.
- Verwenden Sie das neue [scnlightingmodelphysicallybased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) -Schattierungs Modell, um eine breite Palette realistischer Schattierungs Effekte zu erzeugen, während nur drei`Diffuse`Grund `Metalness` Legende `Roughness`Eigenschaften (und) benötigt werden.
- Da die PBR-Schattierung am besten mit der Umgebungs basierten Beleuchtung funktioniert `LightingEnvironment` , verwenden Sie die-Eigenschaft, um einer ganzen Szene Bildbasierte Beleuchtung zuzuweisen.
- Verwenden Sie `IESProfileURL` die-Eigenschaft, um einfache Beleuchtung zu importieren, die Beleuchtung auf der Grundlage realer Werte, wie Intensität (in Lumens) und Farbtemperatur (in Grad Kelvin) definieren.
- Sowohl PBR-als auch HDR-Kamera Features bieten bessere Ergebnisse als herkömmliche Renderingverfahren. Daher führt scenekit jetzt alle Farb Berechnungen in einem linearen Farbraum durch (mit P3-Farbpalette auf breit farbigen Geräte anzeigen).
- Scenekit ist nun mit allen Farben übereinstimmen, indem die Farbprofil Informationen gelesen werden.
- Scenekit interpretiert Farbkomponenten Werte in einem linearen RGB-Farbraum für alle shadertypen.
- Sowohl lineares Farb Raum Rendering als auch breit Farben können deaktiviert werden, indem `SCNDisableLinearSpaceRendering` die `SCNDisableWideGamut` `Info.plist`Schlüssel und in der der APP angegeben werden.
- Erstellen Sie beliebige Polygon-Prime (entweder aus Dateien geladen oder Programm gesteuert generiert), um die Geometrie mit der neuen [scngeometryprimitivetyetpolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) -Klasse anzugeben.
- Da scenekit Farbprofil Informationen in Textur Bildern liest und anpasst, verwenden Sie für alle Images Asset-Kataloge, um sicherzustellen, dass diese Informationen bereitgestellt werden.

## <a name="spritekit-enhancements"></a>Spritekit-Erweiterungen

Die folgenden Verbesserungen wurden an dem spritekit-Framework in ios 10 vorgenommen:

- Benutzerdefinierte Shader können Attribute (`SKAttribute`) bereitstellen, die separat von jedem Knoten konfiguriert werden können, der den Shader verwendet, indem`SKAttributeValue`ein Attribut Wert () bereitgestellt wird.
- Tilemaps unterstützt jetzt quadratische, hexagonale und isometrische Kachel Formen für 2D-, 2.5 d-und Side- `SKTileMapMode`Scroll- `SKTileGroupRule` Spiele `SKTileSet` mithilfe der Klassen, `SKTileGroup`und.
- Verwenden Sie die `SKWarpGeometry` neue-Klasse, um das Rendern von [skspritenode](xref:SpriteKit.SKSpriteNode) oder [schrägerectnode](xref:SpriteKit.SKEffectNode) zu erweitern oder zu verzerren. Die neue [skaction](xref:SpriteKit.SKAction) -Klasse kann verwendet werden, um Übergänge zwischen Warp-Effekten zu animieren.
- Die [skview](xref:SpriteKit.SKView) -Klasse stellt mehrere neue Methoden bereit, mit denen Sie präzise steuern können, wann und wie eine Szene gerendert wird.

## <a name="scrollview-enhancements"></a>ScrollView-Erweiterungen

Die folgenden Verbesserungen wurden an dem ScrollView-Steuerelement in ios 10,3 vorgenommen:

- `UIScrollView`Fügen Sie nun `IndexDisplayMode` die-Eigenschaft ein, um zu steuern, wie der Index angezeigt wird, `UIScrollViewIndexDisplayMode` während der Benutzer einen Bildlauf als durchführt:
  - `Automatic`-Die Index Anzeige wird vom Betriebssystem gesteuert.
  - `AlwaysHidden`-Die Index Anzeige ist immer ausgeblendet.

Informationen zur Verwendung finden Sie im [iostenthree-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree) .

## <a name="uikit-enhancements"></a>UIKit-Verbesserungen

Die folgenden Verbesserungen wurden an dem UIKit-Framework in ios 10 vorgenommen:

- Die neue [uipasteboard](xref:UIKit.UIPasteboard) -API bietet neue Optionen (z. b. Lebensdauer Einschränkungen) und deklariert automatisch kompatible Inhaltstypen für allgemeine Klassentypen.
- Neue, vollständig interaktive, objektbasierte und unter brechbare Animations Unterstützung wurde hinzugefügt und kann mit Gesten verknüpft werden. Weitere Informationen finden Sie in der [uiviewanimating-Protokoll Referenz](https://developer.apple.com/reference/uikit/uiviewanimating)von Apple, der [uiviewpropertyanimator-Klassenreferenz](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), der [uitimingcurrveprovider-Protokoll Referenz](https://developer.apple.com/reference/uikit/uitimingcurveprovider), der [uicubictimingparameters-Klassenreferenz](https://developer.apple.com/reference/uikit/uicubictimingparameters) und [ Uispringtimingparameter-Klassenreferenz](https://developer.apple.com/reference/uikit/uispringtimingparameters) für weitere Informationen.
- Der neue `UIPreviewInteraction` und `UIPreviewInteractionDelegate` ermöglicht der Entwickler-App, eine benutzerdefinierte Schnittstelle für Peek-und Pop-Vorgänge bereitzustellen.
- Die neue `UIAccessibilityCustomRotor` Klasse ermöglicht der APP, benutzerdefinierte kontextspezifische Funktionen für Hilfstechnologien wie Voice over bereitzustellen.
- Verwenden Sie `UIAccessibilityIsAssistiveTouchRunning` die `UIAccessibilityAssistiveTouchStatusDidChangeNotification` Symbole und, um zu bestimmen, ob assistivetouch aktiviert ist.
- Verwenden Sie `UIAccessibilityHearingDevicePairedEar` die `UIAccessibilityHearingDevicePairedEarDidChangeNotification` Symbole und, um den Status von gekoppelten MFI-Hörhilfen zu erhalten.
- Zur Unterstützung des dynamischen Typs in Bezeichnungen verwenden Textfelder und Textfelder die `PreferredFontForTextStyle` neue-Methode `UIFont` der-Klasse.
- Um zu entscheiden, ob ein Element seine Schriftart aktualisieren soll, wenn `UIContentSizeCategory` sich das Gerät ändert `AdjustsFontForContentSizeCategory` , verwenden Sie `UIContentSizeCategoryAdjusting` die-Eigenschaft des-Delegaten.
- Die `OpenURL` -Methode `UIApplication` der-Klasse wird asynchron aufgerufen und unterstützt jetzt einen Vervollständigungs Handler, der aufgerufen wird, nachdem die Öffnungs Aktion abgeschlossen wurde.
- Initiieren der cloudkit-Freigabe und Ändern der Eigenschaften mithilfe `UICloudSharingController` der `UICloudSharingControllerDelegate` neuen-Klasse und der-Klasse.
- Profitieren Sie von vorab abgerufenen Zellen, um die scrolldarstellung von `UICollectionViews` mit dem neuen `UICollectionViewDataSourcePrefetching` Delegaten zu verbessern.
- Der Entwickler kann nun die Darstellung des Badge für Tabulator Elemente (z. b. Text und Hintergrundfarbe) steuern.
- Das Aktualisierungs Steuerelement wird jetzt in allen Unterklassen der Bild Lauf Ansicht und der scrollansicht (z `UICollectionView`. b.) unterstützt.

## <a name="webkit-enhancements"></a>WebKit-Erweiterungen

Die folgenden Verbesserungen wurden an dem WebKit-Framework in ios 10 vorgenommen:

- Die Unterstützung von Peek und Pop wurde der `WKWebView` -Klasse hinzugefügt. Verwenden Sie `ShouldPreviewElement` die-Methode, um zu bestimmen, ob eine bestimmte Webansicht eine Vorschau anzeigen soll.


## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Neuerungen in ios 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
