---
title: Verarbeiten mehrerer Auflösungen in CocosSharp
description: Dieses Handbuch zeigt das Arbeiten mit CocosSharp um Spiele zu entwickeln, die auf Geräten von unterschiedlichen Auflösungen korrekt angezeigt.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 6803dc2668b89ee2d037da8b34e202191dd5465d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307744"
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Verarbeiten mehrerer Auflösungen in CocosSharp

_Dieses Handbuch zeigt das Arbeiten mit CocosSharp um Spiele zu entwickeln, die auf Geräten von unterschiedlichen Auflösungen korrekt angezeigt._

CocosSharp stellt Methoden für die Standardisierung der Objekt-Dimensionen in einem Spiel ungeachtet der physischen Pixel auf der Anzeige des Geräts bereit. In der Vergangenheit konnten für PCs und Spielekonsolen entwickelte Spiele eine einzelne Lösung als Ziel. Moderne Spiele – und insbesondere Spiele für Mobilgeräte – müssen erstellt werden, um eine Vielzahl von Geräten zu ermöglichen. Dieses Handbuch veranschaulicht CocosSharp standardisieren Spiele, unabhängig von der Auflösung Merkmale der physischen Anzeige.

Das Standardverhalten für die Auflösung von CocosSharp ist physische Pixel im Spiel Koordinaten entsprechend. Die folgende Tabelle zeigt, wie verschiedene Geräte ein Hintergrund Umgebung Sprite mit Breite und Höhe des 368 x 240 Pixel gerendert werden kann. Die erste Zeile ist technisch gesehen nicht um ein echtes Gerät, jedoch stattdessen das erwartete Rendering des Sprite, unabhängig von der Auflösung für dieses Gerät:


| **Gerät** | **Bildschirmauflösung** | **Beispielscreenshot** |
|--- | --- |--- |
|Gewünschte Anzeige|368 x 240 Pixel (mit schwarz gefüllten Balken für Seitenverhältnis)| ![368 x 240 Pixel (mit schwarz gefüllten Balken für Seitenverhältnis)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920x1080](resolutions-images/image3.png) |

In diesem Dokument wird beschrieben, wie mit CocosSharp zum Beheben des Problems in der Tabelle oben gezeigt wird. Wie Sie von jedem Gerät zu rendern, wie in der ersten Zeile – unabhängig von der Auflösung des Telefonbildschirms angezeigt, also wird behandelt.


## <a name="working-with-setdesignresolutionsize"></a>Arbeiten mit SetDesignResolutionSize

Die `CCScene` Klasse wird in der Regel als Stammcontainer für alle visuellen Objekte verwendet, bietet jedoch auch eine statische Methode namens `SetDesignResolutionSize` für die Standardgröße für alle Szenen angeben. Anders ausgedrückt: `SetDesignResolutionSize` Methode ermöglicht Entwicklern, um Spiele zu entwickeln, der auf einer Vielzahl von Hardware-Auflösung ordnungsgemäß angezeigt wird. Die CocosSharp-Projektvorlagen verwenden diese Methode, um die Standardgröße für das Projekt mit 1024 x 768, festzulegen, wie im folgenden Code gezeigt wird:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

Sie können ändern, die `desiredWidth` und `desiredHeight` Variablen aus, um die Anzeige entsprechend die gewünschte Auflösung Ihres Spiels ändern. Der obige Code könnte beispielsweise wie folgt geändert werden, zum Anzeigen von Hintergrund 368 x 240 Pixel als Vollbild:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` ermöglicht es, wie im Spielfenster angezeigt auf die gewünschte Auflösung passt. Die folgenden Abschnitte zeigen, wie eine 500 x 500-Bild angezeigt wird, mit anderen `CCSceneResolutonPolicy` Werte, die an die `SetDesignResolutionSize` Methode. Die folgenden Werte werden bereitgestellt, durch die `CCSceneResolutionPolicy` Enumeration:

 - `ShowAll` : Zeigt die gesamte angeforderte Auflösung Seitenverhältnis beizubehalten.
 - `ExactFit` : Zeigt die gesamte angefordert behoben, aber keine Seitenverhältnis wird beibehalten.
 - `FixedWidth` : Zeigt die gesamte angeforderte Breite Seitenverhältnis beizubehalten.
 - `FixedHeight` : Zeigt die gesamte angeforderte Höhe, das Seitenverhältnis beizubehalten.
 - `NoBorder` – Zeigt an, entweder die gesamte Höhe oder die gesamte Breite Beibehaltung des Seitenverhältnisses. Wenn Sie das Seitenverhältnis des Geräts das Seitenverhältnis der gewünschten Lösung, die größer ist, wird die gesamte Breite angezeigt. Wenn das Seitenverhältnis des Geräts kleiner als das Seitenverhältnis beibehalten, der die gewünschte Auflösung ist, wird die gesamte Höhe angezeigt.
 - `Custom` – Die Anzeige hängt die `Viewport` Eigenschaft des aktuellen `CCScene`.

Alle Screenshots werden mit iPhone 4 s Auflösung (960 x 640) im Querformat erstellt und dieses Bild verwenden:

![](resolutions-images/image4.png "Alle Screenshots iPhone 4 s einer Auflösung von 960 x 640 im Querformat werden erstellt, und dieses Bild verwenden")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` Gibt an, dass die Auflösung des gesamte Spiele auf dem Bildschirm angezeigt werden, aber möglicherweise angezeigt *Letterbox-Effekt* (schwarze Balken), um unterschiedliche Seitenverhältnisse anpassen. Diese Richtlinie wird häufig verwendet, wie es wird sichergestellt, dass die gesamte game-Ansicht ohne jede Verzerrung auf dem Bildschirm angezeigt wird.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterbox-Effekt wird angezeigt, auf die Links und rechts des Bilds für das physische Seitenverhältnis beibehalten wird breiter als die gewünschte Auflösung zu berücksichtigen:

![](resolutions-images/image5.png "Letterbox-Effekt wird sichtbar, links und rechts neben dem Bild für das physische Seitenverhältnis breiter als die gewünschte Auflösung wird berücksichtigt")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` Gibt an, dass die Auflösung des gesamte Spiele mit ohne Letterbox-Effekt auf dem Bildschirm sichtbar ist. Sichtbare Bereich möglicherweise verzerrt (Seitenverhältnis möglicherweise nicht beibehalten werden) gemäß dem Seitenverhältnis der Hardware.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Ohne Letterbox-Effekt wird sichtbar, aber die game-Ansicht ist verzerrt, da die Auflösung für dieses Gerät rechteckig ist:

![](resolutions-images/image6.png "Ohne Letterbox-Effekt wird sichtbar, aber die game-Ansicht ist verzerrt, da die Auflösung für dieses Gerät rechteckig ist.")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Gibt an, dass die Breite der Sicht entspricht den Breitenwert, der an übergebene `SetDesignResolutionSize`, aber die Höhe angezeigt werden kann, unterliegt das Seitenverhältnis des physischen Geräts. Der Wert für die Höhe, die an `SetDesignResolutionSize` wird ignoriert, da er zur Laufzeit basierend auf dem physischen Gerät Seitenverhältnis berechnet wird. Dies bedeutet, dass die berechnete Höhe kleiner ist als die gewünschte Höhe (wodurch Teile der game-Ansicht wird außerhalb des Bildschirms) oder die berechnete Höhe möglicherweise größer als die gewünschte Höhe (wodurch mehr von der game-Ansicht angezeigt wird) werden kann. Da dadurch weitere des Spiels angezeigt werden kann, kann dann es den Anschein Letterbox-Effekt aufgetreten ist; Allerdings wird der zusätzliche Speicherplatz nicht unbedingt, black, wenn visuelle Objekt es angezeigt wird. 

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

Das iPhone 4 s hat ein Seitenverhältnis von 3:2, der die berechnete Höhe auf etwa 333 Einheiten sind:

![](resolutions-images/image7.png "Das iPhone 4 s hat ein Seitenverhältnis von 3:2, der die berechnete Höhe auf etwa 333 Einheiten sind")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Im Prinzip `FixedHeight` verhält sich ähnlich wie `FixedWidth` – das Spiel wird der Wert für die Höhe, die an unterliegen `SetDesignResolutionSize,` , aber die Breite zur Laufzeit basierend auf der physischen Auflösung berechnet. Deaktivieren wie oben erwähnt, diese bedeutet, dass die angezeigten Breite kleiner oder größer als die gewünschte Breite, die Teil der Spiele, wodurch Bildschirm oder mehrere des Spiels angezeigt wird, bzw.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Da der folgende Screenshot mit der app im Querformat erstellt wurde, ist die berechnete Breite größer als 500 – insbesondere 750. Diese Richtlinie wird den X-Wert von 0 linksbündig ausgerichtet, damit die zusätzliche Auflösung auf der rechten Seite des Bildschirms angezeigt werden.

![](resolutions-images/image8.png "Diese Richtlinie bleibt den Wert der 0 X linksbündig ausgerichtet, damit die zusätzliche Auflösung auf der rechten Seite des Bildschirms angezeigt werden")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` versucht, die Anwendung ohne Letterbox-Effekt unter Beibehaltung der ursprünglichen Seitenverhältnisses (ohne Verzerrung) anzuzeigen. Wenn die angeforderte Auflösung Seitenverhältnis des Geräts physischen Seitenverhältnis beibehalten übereinstimmt, wird ohne Clipping ausgeführt. Wenn Seitenverhältnisse nicht übereinstimmen, erfolgt klicken Sie dann clipping.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Der folgende Screenshot zeigt die oberen und unteren Teile der Anzeige zugeschnitten, während alle 500 Pixel, der die Breite der Anzeige angezeigt werden:

![](resolutions-images/image9.png "Dieser Screenshot zeigt die oberen und unteren Teile der Anzeige zugeschnitten, während alle 500 Pixel, der die Breite der Anzeige angezeigt werden")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` ermöglicht es einzelnen `CCScene` an einen eigenen benutzerdefinierten Viewport relativ zu der im angegebenen Auflösung `SetDesignResolutionSize`.

Szenen definieren ihre `Viewport` Eigenschaft durch das Erstellen einer `CCViewport`, denen wiederum wird erstellt, mit einer `CCRect`. Weitere Informationen zu `CCViewport` und `CCRect` finden Sie unter den [CocosSharp-API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/). Im Zusammenhang mit der Erstellung einer `CCViewport` der `CCRect` Geben Sie die Parameter des Konstruktors (in Reihenfolge):

 - X – der Betrag für die horizontal den Viewport sind, entfernt, wobei jede Einheit eine gesamte Bildschirmbreite darstellt. Verwenden Sie beispielsweise einen Wert von .5f-Ergebnissen in der Szene, die nach rechts verschoben, um die Hälfte der Bildschirmbreite.
 - y – der Betrag, um vertikal den Viewport sind, entfernt, wobei jede Einheit einen ganzen Bildschirm angezeigte Höhe darstellt werden soll. Verwenden Sie beispielsweise einen Wert von .5f-Ergebnissen in der Szene, um die Hälfte der Bildschirmhöhe nach unten verschoben.
 - Width – der horizontalen Teil, der die Szene einnehmen sollte. Verwenden Sie beispielsweise den Wert 1 / 3. 0f führt in der Szene horizontal belegt ein Drittel des Bildschirms.
 - Height – der vertikalen Teil, der die Szene einnehmen sollte. Verwenden Sie beispielsweise den Wert 1 / 3. 0f führt in der Szene vertikal belegt ein Drittel des Bildschirms.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

Der obige Code hat folgende Auswirkungen:

![](resolutions-images/image10.png "Der obige Code führt in diesem screenshot")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

Die `DefaultTexelToContentSizeRatio` vereinfacht die Verwendung von Texturen mit höherer Auflösung auf Geräten Bildschirme mit höherer Auflösung. Diese Eigenschaft ermöglicht insbesondere Spiele mit höherer Auflösung Ressourcen ohne so ändern Sie die Größe oder Position von visuellen Elementen. 

Beispielsweise kann ein Spiel ein Medienobjekt Art für ein Spiel Zeichen handelt es sich 200 Pixel hoch und dem Spiel Bildschirm enthalten (nach den Angaben von `SetDesignResolutionSize`) möglicherweise 400 Pixeln. In diesem Fall wird das Zeichen Hälfte des Bildschirms einnimmt. Wenn ein 400 Pixel hoch Medienobjekt für das Zeichen für Geräte mit höherer Auflösung verwendet wurden, würde das Zeichen die gesamte Höhe der Anzeige belegen. Wenn die Absicht ist, die dem Zeichen, um höhere Auflösung Geräte nutzen die gleiche Größe behalten, ist zusätzlicher Code erforderlich, das Sprite Zeichen auf die Hälfte der Höhe des Bildschirms zu beschränken.

Das oben angeführte Beispiel stellt ein häufiges Problem bei der Entwicklung von Spielen – größere Ressourcen hinzufügen, ohne dass die Entwickler führen Sie für jedes visuelle Element nach geräteauflösung Größenänderung dar. `DefaultTexelToContentSizeRatio` wird eine globale Eigenschaft, die zum Ändern der Größe von visuellen Elementen verwendet wird. Dieser Wert wird alle visuellen Elemente eines bestimmten Typs von der zugewiesene Wert.

Diese Eigenschaft ist vorhanden, in den folgenden Klassen: 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp von Visual Studio für Mac Vorlagen Satz `CCSprite.DefaultTexelToContentSizeRatio` entsprechend die Breite des Geräts als Beispiel dafür, wie sie verwendet werden kann. Der folgende Code befindet sich im `CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```


### <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio-Beispiel

Um finden Sie unter wie `DefaultTexelToContentSizeRatio` wirkt sich auf die Größe der Visualisierung Elemente, sollten Sie den oben aufgeführten Code:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Die in der folgenden Abbildung, die mit einer Textur 500 x 500 führt:

![](resolutions-images/image5.png "Das führt in diesem Bild mit einer 500 x 500-Struktur")

Das iPad Retina, die auf einer physischen Auflösung von 2048 x 1536 ausgeführt wird. Dies bedeutet, dass das Spiel wie oben dargestellt 500 Pixel über 1536 physische Pixel gestreckt wird. Spiele profitieren von dieser zusätzlichen Auflösung durch das Erstellen, was in der Regel als bezeichnet *hd* Assets – Ressourcen, die auf Bildschirme mit höherer Auflösung ausgeführt werden soll. Dieses Spiel könnte z. B. eine hd-Version dieser Textur, die an die Größe 1000 x 1000 verwenden. Allerdings mit dem größeren Image würde die `CCSprite` müssen eine Breite und Höhe von 1000 Einheiten. Da unser Spiel immer 500 Einheiten angezeigt wird (aufgrund der `SetDesignResolutioSize` aufrufen), unsere 1000 x 1000 Sprite wäre zu groß ist (nur im unteren linken Bereich des Zertifikats angezeigt):

![](resolutions-images/image11.png "Da das Spiel immer 500 Einheiten aufgrund des Aufrufs SetDesignResolutioSize angezeigt wird, wäre das Sprite 1000 x 1000 zu groß, dass nur im unteren linken Bereich des Zertifikats wird angezeigt.")

Wir können festlegen, `CCSprite.DefaultTexelToContentSizeRatio` wird zweimal so breit und hoch ist, als die originaltextur 500 x 500-1000 x 1000 Textur berücksichtigen:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Beim Ausführen des Spiels wird die Textur 1000 x 1000 jetzt vollständig sichtbar sein:

![](resolutions-images/image12.png "Jetzt beim Ausführen des Spiels wird die Textur 1000 x 1000 vollständig sichtbar sein")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio details

Die `DefaultTexelToContentSizeRatio` Eigenschaft `static,` was bedeutet, dass alle Sprites in der Anwendung gemeinsam den gleichen Wert. Der typische Ansatz für Spiele mit Ressourcen, die für verschiedene Auflösungen vorgenommen ist, einen vollständigen Satz von Ressourcen für jede Kategorie Lösung enthalten. Standardmäßig CocosSharp von Visual Studio für Mac-Vorlagen bereitstellen **%ld** und **hd** Ordner für Ressourcen, die nützlich für Spiele, die zwei Sätze von Texturen zu unterstützen. Ein Beispielordner mit Inhalt mit Inhalt aussehen kann:

![](resolutions-images/image13.png "Wie kann ein Beispielordner mit Inhalt mit Inhalt aus.")

Beachten Sie, dass die Standardvorlage auch Code zum Hinzufügen von bedingt auf der Anwendung enthält `ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```

Dies bedeutet, dass die Anwendung sucht Dateien in Pfaden nach, ob es in hoher Auflösung oder regulären Auflösung-Modus ausgeführt wird. Z. B. wenn die **hd** und **%ld** Ordner enthalten, eine Datei namens **background.png** und klicken Sie dann der folgende Code führen würde, und wählen die richtige Datei für die Auflösung:


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird beschrieben, wie Spiele zu erstellen, die unabhängig von der Auflösung für dieses Gerät richtig angezeigt wird. Es werden Beispiele für die Verwendung verschiedenen `CCSceneResolutionPolicy` Werte für das Ändern der Größe des Spiels nach der Auflösung für dieses Gerät. Es enthält auch ein Beispiel dafür, wie `DefaultTexelToContentSizeRatio` können verwendet werden, um mehrere Sätze von Inhalt zu ermöglichen, ohne dass visuelle Elemente einzeln geändert werden kann.

## <a name="related-links"></a>Verwandte Links

- [CocosSharp-Einführung](~/graphics-games/cocossharp/index.md)
- [CocosSharp-API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/)
