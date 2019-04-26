---
title: Verbessern der Framerate mit CCSpriteSheet
description: CCSpriteSheet bietet Funktionalität für viele Bilddateien in einer Textur verwendet und kombiniert. Anzahl der Textur verringern kann Ladezeiten des Spiels und der Bildrate verbessern.
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: eab6153653a8c8df2068aaaf879d84d35473c541
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61245689"
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>Verbessern der Framerate mit CCSpriteSheet

_CCSpriteSheet bietet Funktionalität für viele Bilddateien in einer Textur verwendet und kombiniert. Anzahl der Textur verringern kann Ladezeiten des Spiels und der Bildrate verbessern._

Viele Spiele erfordern anstrengungen zur speicheroptimierung reibungslos und schnell für mobile Hardware zu laden. Die `CCSpriteSheet` Klasse kann dazu beitragen, viele allgemeiner Leistungsprobleme, die von CocosSharp Spielen auftreten zu behandeln. Dieser Leitfaden behandelt allgemeine Leistungsprobleme und wie sie mithilfe der `CCSpriteSheet` Klasse.


## <a name="what-is-a-sprite-sheet"></a>Was ist ein Sprite-Sheet?

Ein *Sprite-Sheet*, dies kann auch sein als bezeichnet ein *Strukturatlas*, ist ein Image der kombiniert mehrere Abbilder in einer Datei. Dies kann die Leistung zur Laufzeit als auch Inhalt Ladezeiten verbessern.

Das folgende Bild ist beispielsweise ein einfaches Sprite-Sheet, das durch drei separate Abbilder erstellt. Die einzelnen Bilder können eine beliebige Größe, und das resultierende Sprite-Sheet ist nicht vollständig ausgefüllt werden muss:

![](ccspritesheet-images/image1.png "Die einzelnen Bilder können beliebige Größe aufweisen, und das resultierende Sprite-Sheet ist nicht vollständig ausgefüllt werden muss")


### <a name="render-states"></a>Rendern von Zuständen

Visuelle CocosSharp-Objekte (z. B. `CCSprite`) vereinfachen Sie Code zum Rendern über herkömmliche grafische-API-Rendering-Code wie z. B. MonoGame oder OpenGL verwendet, die die Erstellung von Vertex-Puffer erfordern (wie in der [Zeichnen von 3D-Grafiken mit Vertices in MonoGame](~/graphics-games/monogame/3d/part2.md) Handbuch). Trotz seiner Einfachheit CocosSharp beseitigt nicht die Kosten für die Einstellung *Rendern Zustände*, welche sind die Anzahl der Male, der Renderingcode Texturen oder anderen Zuständen Rendering-bezogene wechseln muss.

CocosSharp interne Code rendert jedes visuelle Element nacheinander durch Durchlaufen der visuellen Struktur ab, mit dem aktuellen `CCScene`. Betrachten Sie beispielsweise die folgende Szene:

![](ccspritesheet-images/image2.png "Betrachten Sie diese Szene")

CocosSharp nacheinander die vier Sterne gerendert:

![](ccspritesheet-images/image3.png "CocosSharp gerendert nacheinander die vier Sterne")

Da jede `CCSprite` verwendet die gleiche Textur, CocosSharp kann die Gruppierung alle vier Sterne. Dieser Code erfordert nur eine Render Zustand Zuweisung (Zuweisung der Stern Textur) pro Frame. Dieses Szenario ist sehr effizient.

Verwenden Sie nur sehr wenige Spiele natürlich nur ein Bild. Die folgende Szene führt eine weltweit Grafik:

![](ccspritesheet-images/image4.png "Diese Szene führt eine weltweit-Grafik")

Im Idealfall sollten alle Sprites, die mit einem Bild zunächst (z. B. die Sterne), und klicken Sie dann auf den Rest der Sprites mit dem anderen Image (weltweit) CocosSharp gezogen werden:

![](ccspritesheet-images/image5.png "CocosSharp sollten idealerweise alle Sprites, die ein Abbild zuerst verwenden, z. B. die Sterne, klicken Sie dann den Rest der Sprites mit dem anderen Image weltweit zeichnen.")

Die Reihenfolge über zwei Zustände Rendern erfordert: eine auf der ersten Stern, auf dem ersten Planeten.

Wenn alle `CCSprite` Instanzen sind untergeordnete Elemente des gleichen `CCNode`, und klicken Sie dann CocosSharp die Reihenfolge der Zeichnen-Befehl optimiert, Änderungen am Ansichtszustand des Rendering zu reduzieren. IF, andererseits, die `CCSprite` Instanzen sind so organisiert, dass CocosSharp die Darstellung zu optimieren kann (z. B. Wenn sie Teil der anderen Entität sind `CCNode` Instanzen), und klicken Sie dann die Reihenfolge möglicherweise nicht optimal. Das folgende Beispiel zeigt die schlechteste mögliche Zeichnen-Reihenfolge, führt fünf Render-Status:

![](ccspritesheet-images/image6.png "Dadurch wird die schlechteste mögliche Draw-Reihenfolge, führt fünf Render-Status")

Render-Status können schwierig sein, da die Draw-Reihenfolge bei der die visuelle Struktur der standardeinhaltung zu optimieren `CCNode` Instanzen. Diese Struktur ist häufig so strukturiert, dass Sie einfach zu (z. B. Entitäten, die mit ihrer visuellen untergeordneten Elemente) werden, oder aufgrund der gewünschten visuellen Layout angeordnet werden, gemäß der Künstler.

Natürlich ist die ideale Situation, um einen einzelnen Renderingzustand trotz mehrerer Abbilder zu erhalten. CocosSharp-Spiele können dies erreichen, indem alle Abbilder in einer einzelnen Datei kombiniert, und klicken Sie dann, dass eine Datei laden (sowie die zugehörigen **plist** Datei) in einem `CCSpriteSheet`. Mithilfe der `CCSpriteSheet` Klasse ist sogar noch wichtiger ist, für Spiele, die eine große Anzahl von Bildern haben, oder bei denen sehr, kompliziert Layouts. 

### <a name="load-times"></a>Laden.

Kombinieren mehrerer Abbilder in einer Datei verbessert auch die des Spiels in den Ladezeiten für eine Reihe von Ursachen haben:

 - Kombinieren mehrerer Abbilder in einer einzelnen Datei kann die Anzahl der Pixel, die über das effiziente Packen verwendet, reduzieren.
 - Laden weniger Dateien bedeutet weniger Aufwand pro Datei, z. B. das Analysieren von PNG-Headern
 - Laden weniger Dateien erfordert weniger Zeit, die wichtig für datenträgerbasierte Medien wie DVDs und herkömmlichen Computerfestplatten ist suchen

## <a name="using-ccspritesheet-in-code"></a>Verwenden CCSpriteSheet in code

Zum Erstellen einer `CCSpriteSheet` -Instanz ein Bild und eine Datei, die definiert, die Bereiche des Bilds, die für jeden Frame verwendet, muss den Code angeben. Das Bild geladen werden kann, als eine **PNG** oder **.xnb** Datei (bei Verwendung der [Pipeline für Bildinhalte](~/graphics-games/cocossharp/content-pipeline/index.md)). Die Datei definieren die Frames ist eine **plist** Datei, die manuell erstellt werden kann oder *TexturePacker* (die wir im folgenden beschrieben werden).

Der Beispiel-app die [kann hier heruntergeladen werden](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), erstellt die `CCSpriteSheet` aus einem **PNG** und **plist** Datei mit dem folgenden Code:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Einmal geladen, die `CCSpriteSheet` enthält eine `List` von `CCSpriteFrame` Instanzen: jede Instanz entsprechen, der die Source-Images verwendet, um das ganze Blatt erstellen. Im Fall von der **SpriteSheetDemo** -Projekt die `CCSpriteSheet` enthält drei Bilder. Die **plist** Datei in Visual Studio für Mac oder in einem Text-Editor, um festzustellen, welche Images verfügbar sind, überprüft werden kann. Wenn wir hier die **plist** -Datei in einem Text-Editor, wie Sie die drei Frames (Abschnitte ausgelassen sehen, um die Schlüsselnamen hervorheben):


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

Wir können die `Find` Methode, um die Frames nach Namen suchen, wie folgt (Code ausgelassen wird, um hervorzuheben `CCSpriteFrame` Verwendung):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Da die `CCSprite` Konstruktor dauert eine `CCSpriteFrame` Parameter der Code nie hat, untersuchen Sie die Details des der `CCSpriteFrame`, z. B. welche Textur verwendet bzw. in der Region des Bilds in der master Sprite-Sheet.


## <a name="creating-a-sprite-sheet-plist"></a>Erstellen ein Sprite-Sheet plist

Die plist-Datei ist eine Xml-Datei, die erstellt und manuell bearbeitet werden kann. Auf ähnliche Weise kann Bildbearbeitungsprogramme verwendet werden, um mehrere Dateien in eine größere Datei kombinieren. Seit dem Erstellen und warten, dass die Sprite-Sheets sehr zeitaufwändig sein können, untersuchen das Programm TexturePacker wir die Dateien im CocosSharp-Format exportiert werden können. TexturePacker bietet ein kostenloses und einer "Pro"-Version, und steht für Windows und Mac OS. Der Rest dieses Handbuchs kann mithilfe der kostenlosen Version folgen. 

TexturePacker möglich [heruntergeladen, die von der Website TexturePacker](https://www.codeandweb.com/texturepacker). Beim Öffnen, muss die texturepacker erlaubt kein Projekt geladen. Die ab Bildschirm können Sie zum Öffnen zuletzt geöffnete Projekte (falls es sich bei anderen Projekten erstellt wurden), füge Sprites hinzu und wählen Sie das Format für das Sprite-Sheet verwenden. CocosSharp wird das Datenformat Cocos2D verwendet:

![](ccspritesheet-images/image7.png "CocosSharp verwendet das Cocos2D-Datenformat")

Bilddateien (z. B. **PNG**) können hinzugefügt werden TexturePacker ziehen Sie Drag & Drop sie aus dem Windows-Explorer unter Windows oder Finder auf Mac. TexturePacker wird automatisch die Sprite-Sheet-Vorschau aktualisiert, wenn eine Datei hinzugefügt wird:

![](ccspritesheet-images/image8.png "TexturePacker aktualisiert automatisch die Sprite-Sheet-Vorschau, sobald eine Datei hinzugefügt wird")

Um ein Sprite-Sheet zu exportieren, klicken Sie auf die **veröffentlichen Sprite-Sheet** Schaltfläche, und wählen Sie einen Speicherort für das Sprite-Sheet. Texturepacker erlaubt, wird eine plist-Datei und eine Image-Datei gespeichert.

Um die resultierenden Dateien zu verwenden, fügen Sie sowohl PNG-als auch plist, eines CocosSharp-Projekts ein. Informationen zum Hinzufügen von Dateien zu CocosSharp-Projekten finden Sie in der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md). Nachdem die Dateien hinzugefügt wurden, können sie in geladen werden eine `CCSpriteSheet` wie zuvor im Code oben gezeigt wurde:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>Überlegungen zum Verwalten einer TexturePacker Sprite-sheet

Wie Spiele entwickelt werden, können Künstler hinzufügen, entfernen oder Ändern von Grafiken. Jede Änderung erfordert eine aktualisierte Sprite-Sheet. Die folgenden Überlegungen können Sprite-Sheet-Wartung zu erleichtern:

 - Behalten Sie die ursprünglichen Dateien (die Dateien verwendet, um die Sprite-Sheets erstellen) in einem Ordner in Ihrem Projekt ein, und stellen Sie sicher, dass sie zur Versionskontrolle hinzugefügt werden. Diese Dateien sind erforderlich werden, um das Sprite-Sheet neu zu erstellen, wenn eine Änderung vorgenommen wird.
 - Nicht Visual Studio für Mac/Visual Studio die ursprünglichen Dateien hinzufügen, oder wenn sie hinzugefügt wurden, legen Sie die **Buildvorgang** zu **keine**. Wenn die Dateien werden hinzugefügt, und die plattformspezifischen haben **Buildvorgang**, und klicken Sie dann sie unnötigerweise die resultierende app Datei vergrößert werden.
 - Erwägen Sie die Verwendung *intelligente Ordner* in texturepacker erlaubt. Intelligente Ordner hinzufügen enthaltenen Bilder automatisch auf das Sprite-Sheet. Dieses Feature spart viel Zeit bei der Entwicklung von Spiele mit einer großen Anzahl von Bildern. 

    ![](ccspritesheet-images/image9.png "Dieses Feature spart viel Zeit bei der Entwicklung von Spiele mit einer großen Anzahl von Bildern")
 - Achten Sie auf die Sprite-Sheet-Textur-Größen. Einige ältere Phone-Hardware unterstützt keine Textur-Größen, die größer als 2048 x 2048. Ein 32-Bit-Image von 2048 x 2048 verwendet außerdem fast 17 MB RAM – eine erhebliche Menge an Arbeitsspeicher.
 - TexturePacker enthält keine Ordner in Sprite-Namen in der Standardeinstellung damit Konflikte bei Befehlsnamen möglich sind. Es wird empfohlen, um zu entscheiden, ob der Ordner der Dateinamen oder nicht am Anfang der Entwicklung. Größere Spiele sollten Ordnernamen verwenden, um Konflikte zu vermeiden. Wenn Ordnerpfade einschließen möchten, klicken Sie auf **Anzeigen erweiterter** in die **Daten** Abschnitt, und überprüfen Sie **voranstellen Ordnername**. 

    ![](ccspritesheet-images/image10.png "Ordnerpfade enthalten soll, klicken Sie auf Erweiterte im Datenabschnitt anzeigen und überprüfen Sie den Ordnernamen voranstellen")

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wird beschrieben, wie zum Erstellen und Verwenden der `CCSpriteSheet` Klasse. Sie erfahren auch, wie Dateien erstellt, die geladen werden kann `CCSpriteSheet` Instanzen mit dem Programm texturepacker erlaubt.

## <a name="related-links"></a>Verwandte Links

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Vollständige Demo (Beispiel)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
