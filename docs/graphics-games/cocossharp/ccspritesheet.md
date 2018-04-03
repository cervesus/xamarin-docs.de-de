---
title: Verbessern der Framerate mit CCSpriteSheet
description: CCSpriteSheet bietet Funktionen zum Kombinieren von und verwenden viele Bilddateien in eine Textur. Anzahl der Textur verringern kann ein Spiel Ladezeiten und Framerate verbessert werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 7e2bb5b98b5c93fb625ce645692d8a3ccb3d143b
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>Verbessern der Framerate mit CCSpriteSheet

_CCSpriteSheet bietet Funktionen zum Kombinieren von und verwenden viele Bilddateien in eine Textur. Anzahl der Textur verringern kann ein Spiel Ladezeiten und Framerate verbessert werden._

Viele Spiele ist die Optimierung reibungslos, und Laden schnell auf mobile Hardware erforderlich. Die `CCSpriteSheet` Klasse kann dazu beitragen, viele CocosSharp Spiele angetroffen allgemeiner Leistungsprobleme zu behandeln. Diese Anleitung enthält häufig auftretende Leistungsprobleme und zum Behandeln sie mithilfe der `CCSpriteSheet` Klasse.


## <a name="what-is-a-sprite-sheet"></a>Was ist ein Blatt Sprite?

Ein *Sprite Blatt*, die können auch auf bezeichnet werden, als ein *Textur Atlas*, ein Bild in einer Datei mehrere Abbilder kombiniert wird. Dies kann die Leistung zur Laufzeit als auch Content Ladezeiten verbessern.

Die folgende Abbildung ist beispielsweise ein einfaches Sprite Blatt von drei separaten Images erstellt. Die einzelnen Bilder können beliebiger Größe sein, und das resultierende Sprite Blatt ist nicht vollständig ausgefüllt werden:

![](ccspritesheet-images/image1.png "Die einzelnen Bilder können beliebiger Größe sein, und das resultierende Sprite Blatt ist nicht vollständig ausgefüllt werden")


### <a name="render-states"></a>Rendern von Zuständen

Visueller CocosSharp-Objekte (z. B. `CCSprite`) vereinfachen Renderingcodes über herkömmlichen grafischen API Renderingcodes z. B. MonoGame oder OpenGL, die die Erstellung von Vertexpuffer erfordern (wie in der [zeichnen 3D-Grafiken mit Scheitelpunkten in MonoGame](~/graphics-games/monogame/3d/part2.md) Handbuch). Trotz seiner Einfachheit CocosSharp nicht beseitigen die Kosten für die Einstellung *Rendern Zustände*, sind die Anzahl der Male, dass Renderingcodes Texturen oder anderen Rendering-bezogene Status wechseln muss.

CocosSharp des interne Code rendert jedes visuelle Element nacheinander durch den Anfang der visuellen Struktur mit dem aktuellen durchlaufen `CCScene`. Betrachten Sie beispielsweise die folgenden Szene aus:

![](ccspritesheet-images/image2.png "Betrachten Sie diese Szene")

CocosSharp würde die vier Sterne nacheinander Rendern:

![](ccspritesheet-images/image3.png "CocosSharp würde die vier Sterne nacheinander rendern.")

Da jede `CCSprite` verwendet die gleiche Struktur CocosSharp kann alle vier Sterne zusammen zu gruppieren. Dieser Code erfordert nur eine Render Zustand Zuweisung (Zuweisung der Textur Stern) pro Frame. Dieses Szenario ist sehr effizient.

Verwenden Sie nur sehr wenige Spiele natürlich nur ein Bild. Die folgenden Szene führt eine Grafik Planeten:

![](ccspritesheet-images/image4.png "Diese Szene führt eine Planeten-Grafik")

Im Idealfall soll CocosSharp alle Sprites mit einem Bild zunächst (z. B. die Sterne), und klicken Sie dann auf die restliche die Sprites mit den anderen Bilds (Planeten) zeichnen:

![](ccspritesheet-images/image5.png "Im Idealfall sollten CocosSharp alle mit einem Bild zunächst wie z. B. die Sterne, klicken Sie dann den Rest mithilfe des anderen Images Planeten Sprites Sprites zeichnen")

Die Sortierung über zwei Zustände Rendern erfordert: eine auf dem ersten Stern, auf die erste Planeten.

Wenn alle `CCSprite` Instanzen sind untergeordnete Elemente des gleichen `CCNode`, dann CocosSharp die Reihenfolge Zeichnen-Befehl zum Rendern von Zustandsänderungen zu reduzieren optimieren. Wenn sich hingegen der `CCSprite` Instanzen werden so organisiert, dass CocosSharp der Optimierung kann (z. B. Wenn sie Teil der anderen Entität sind `CCNode` Instanzen), und klicken Sie dann die Reihenfolge möglicherweise nicht optimal. Das folgende Beispiel zeigt die schlechteste möglich Zeichnen Reihenfolge fünf Render-Status aus:

![](ccspritesheet-images/image6.png "Dadurch wird die schlechteste möglich Zeichnen Reihenfolge, wodurch fünf Render-Status")

Render-Status können schwierig sein, zu optimieren, da die Reihenfolge zeichnen die visuelle Struktur eines unterliegen muss `CCNode` Instanzen. Diese Struktur ist häufig so strukturiert, dass Sie leicht zur Bearbeitung (z. B. Entitäten, die mit ihren untergeordneten visuellen Elemente) werden, oder aufgrund der gewünschten visuelle Layout angeordnet werden, gemäß der Definition von Künstler.

Natürlich ist die ideale Situation an, die einen einzelnen Render Status, obwohl er besitzt mehrere Abbilder. CocosSharp Spiele können zu diesem Zweck alle Bilder in einer einzelnen Datei kombiniert, und klicken Sie dann, dass eine Datei laden (zusammen mit seiner zugehörigen **plist** Datei) in einem `CCSpriteSheet`. Mithilfe der `CCSpriteSheet` Klasse ist sogar noch wichtiger für Spiele, wofür eine große Anzahl von Images, oder bei denen sehr, komplexe Layouts. 

### <a name="load-times"></a>Laden.

Kombinieren mehrere Bilder in einer Datei verbessert auch ein Spiel Ladezeiten eine Reihe von Gründen:

 - Kombinieren mehrere Bilder in einer einzelnen Datei kann die gesamte Anzahl der Pixel, die über effiziente Packen verwendet, reduzieren.
 - Laden weniger Dateien bedeutet weniger Aufwand von pro Datei, z. B. das Analysieren von PNG-Headern
 - Laden weniger Dateien erfordert weniger Zeit, die wichtig für datenträgerbasierte Medien wie DVDs oder Festplatten eines Computers herkömmlichen ist seek

## <a name="using-ccspritesheet-in-code"></a>Verwenden von CCSpriteSheet in code

Zum Erstellen einer `CCSpriteSheet` Instanz, ein Bild und eine Datei, die die Bereiche des für jeden Frame zu verwendenden Images definiert, muss den Code angeben. Das Bild geladen werden kann, wie eine **PNG** oder **.xnb** Datei (bei Verwendung der [Content Pipeline](~/graphics-games/cocossharp/content-pipeline/index.md)). Die Datei definieren die Frames ist ein **plist** Datei, die manuell erstellt werden kann oder *TexturePacker* (die unten erläutert).

Die Beispiel-app, die [kann hier heruntergeladen werden](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), erstellt der `CCSpriteSheet` aus einer **PNG** und **plist** Datei mit dem folgenden Code:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Einmal geladen, die `CCSpriteSheet` enthält eine `List` von `CCSpriteFrame` Instanzen – jede Instanz einer Quellbilder verwendet, um das ganze Blatt erstellen entspricht. Im Fall von der **SpriteSheetDemo** -Projekt die `CCSpriteSheet` drei Bilder enthält. Die **plist** Datei in Visual Studio für Mac oder in einem Text-Editor, um festzustellen, welche Images verfügbar sind, überprüft werden kann. Wenn wir sehen den **plist** Datei in einem Texteditor, wie Sie die drei Frames (Abschnitte weggelassen wird sehen, um die Schlüsselnamen zu bezeichnen):


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

Wir können die `Find` Methode, um die Frames wie folgt nach Namen suchen (Code ausgelassen wird, um Sie hervorzuheben `CCSpriteFrame` Nutzung):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Seit der `CCSprite` Konstruktor dauert eine `CCSpriteFrame` Parameter der Code nie hat, untersuchen Sie die Details der `CCSpriteFrame`, z. B. welche Struktur verwendet oder die Region des Bilds in der master Sprite Blatt.


## <a name="creating-a-sprite-sheet-plist"></a>Erstellen eine Sprite Blatt plist

Die plist-Datei ist eine Xml-basierte-Datei, die erstellt und manuell bearbeitet werden kann. Auf ähnliche Weise kann Bildbearbeitungsprogramme verwendet werden, um mehrere Dateien in eine größere Datei kombinieren. Seit erstellen und Verwalten von Sprite Blätter sehr zeitaufwändig sein können, wird das Programm TexturePacker betrachten wir die Dateien im CocosSharp-Format exportieren können. TexturePacker bietet ein kostenloses und eine "Pro" Version und steht für Windows und Mac OS. Der Rest dieses Handbuchs kann mithilfe der kostenlosen Version ausgeführt werden. 

TexturePacker kann [heruntergeladen, die von der Website TexturePacker](https://www.codeandweb.com/texturepacker). Nach dem Öffnen werden TexturePacker ein geladenes Projekt keinen. Der erste Bildschirm können Sie Sprites hinzufügen, öffnen Sie zuletzt geöffnete Projekte (falls es sich bei anderen Projekten erstellt wurden), und wählen Sie das Format für das Blatt Sprite verwendet. CocosSharp verwendet das Datenformat Cocos2D:

![](ccspritesheet-images/image7.png "CocosSharp verwendet das Cocos2D-Datenformat")

Bilddateien (z. B. **PNG**) können TexturePacker durch Ziehen Sie Drag & Drop werden von Windows-Explorer unter Windows oder auf einem Mac Finder hinzugefügt werden TexturePacker wird die Sprite Blatt Vorschau automatisch aktualisiert, sobald eine Datei hinzugefügt wird:

![](ccspritesheet-images/image8.png "TexturePacker aktualisiert automatisch die Sprite Blatt Vorschau, sobald eine Datei hinzugefügt wird")

Um ein Blatt Sprite zu exportieren, klicken Sie auf die **veröffentlichen Sprite Blatt** Schaltfläche und wählen Sie einen Speicherort für das Sprite ein. TexturePacker speichert eine plist-Datei und einer Bilddatei.

Um die resultierenden Dateien zu verwenden, fügen Sie die PNG und der plist zu einem Projekt CocosSharp ein. Informationen zum Hinzufügen von Dateien zu Projekten CocosSharp finden Sie unter der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md). Nachdem die Dateien hinzugefügt wurden, können sie in geladen werden eine `CCSpriteSheet` wie im obigen Code weiter oben gezeigt wurde:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>Überlegungen zum Verwalten einer TexturePacker Sprite hinzu

Wie Spiele entwickelt werden, möglicherweise Künstler hinzufügen, entfernen oder Ändern von Grafiken. Jede Änderung erfordert eine aktualisierte Sprite Blatt. Die folgenden Aspekte können Sprite Blatt Wartung erleichtern:

 - Behalten Sie die ursprünglichen Dateien (Dateien verwendet, um die Blätter Sprite erstellen) in einem Ordner im Projekt, und stellen Sie sicher, dass sie zur Versionskontrolle hinzugefügt werden. Diese Dateien werden das Blatt Sprite neu zu erstellen, wenn eine Änderung vorgenommen wird benötigt.
 - Nicht Visual Studio für Mac/Visual Studio die ursprünglichen Dateien hinzufügen oder wenn sie hinzugefügt wurden, legen Sie die **Buildvorgang** auf **keine**. Wenn die Dateien hinzugefügt werden, und die plattformspezifischen haben **Buildvorgang**, und klicken Sie dann sie unnötig resultierende app-Datei werden vergrößert.
 - Erwägen Sie *smart-Ordner* in TexturePacker. Intelligente Ordner hinzufügen keine eigenständige Bilder automatisch auf das Arbeitsblatt Sprite. Diese Funktion kann viel Zeit beim Entwickeln von Spielen mit einer großen Anzahl von Bildern speichern. 

    ![](ccspritesheet-images/image9.png "Diese Funktion kann viel Zeit beim Entwickeln von Spielen mit einer großen Anzahl von Bildern speichern")
 - Achten Sie auf Sprite Blatt Textur Größen. Einige älterer Phone Hardware unterstützt keine Textur größer als 2048 x 2048. Ein 32-Bit-Abbild von 2048 x 2048 Bits verwendet außerdem fast 17 MB RAM – eine erhebliche Menge an Arbeitsspeicher.
 - TexturePacker enthält keine Ordner in Sprite Namen standardmäßig, sodass Namenskonflikte möglich sind. Es wird empfohlen, entscheiden Sie, ob der Ordner der Dateinamen oder nicht am Anfang der Entwicklung. Größere spielen, sollten i. Allg. von Ordnernamen um Konflikte zu vermeiden. Wenn Ordnerpfaden einschließen möchten, klicken Sie auf **Anzeigen erweiterter** in der **Daten** , und überprüfen Sie im Abschnitt **stellen Ordnername**. 

    ![](ccspritesheet-images/image10.png "Um Ordnerpfade einzuschließen, klicken Sie auf Anzeigen erweiterter im Datenabschnitt und stellen Ordnername überprüfen")

## <a name="summary"></a>Zusammenfassung

Diese Anleitung enthält Informationen zum Erstellen und Verwenden der `CCSpriteSheet` Klasse. Sie erfahren auch, wie Dateien erstellt, die geladen werden kann `CCSpriteSheet` Instanzen mit dem Programm TexturePacker.

## <a name="related-links"></a>Verwandte links

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Vollständige Demo (Beispiel)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
