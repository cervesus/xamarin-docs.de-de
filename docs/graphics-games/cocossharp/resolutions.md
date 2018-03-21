---
title: "Behandlung von mehrere Auflösungen in CocosSharp"
description: "Dieses Handbuch veranschaulicht das Arbeiten mit CocosSharp Spiele entwickeln, die auf Geräten von unterschiedlichen Auflösungen ordnungsgemäß angezeigt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 9b76376bdbcf10bf35768cfdb79b6823388e303c
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2018
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Behandlung von mehrere Auflösungen in CocosSharp

_Dieses Handbuch veranschaulicht das Arbeiten mit CocosSharp Spiele entwickeln, die auf Geräten von unterschiedlichen Auflösungen ordnungsgemäß angezeigt._

CocosSharp bietet Methoden für die Standardisierung Objekt Dimensionen im Spiel ungeachtet der physischen Pixel auf einem Gerät anzeigen. Bisher konnte für PCs und Spielekonsolen entwickelte Spiele eine einzelne Lösung abzielen. Moderne Spiele – und insbesondere spielen für mobile Geräte – müssen erstellt werden, um eine Vielzahl von Geräten zu ermöglichen. Diese Anleitung zeigt, wie CocosSharp standardisieren Spiele unabhängig von der Merkmale der Auflösung des physischen Bildschirms.

Das Standardverhalten für die Auflösung des CocosSharp wird entsprechend der physischen Pixel mit den Koordinaten des Spiels. Die folgende Tabelle zeigt, wie verschiedene Geräte einen Hintergrund Umgebung Sprite mit Breite und Höhe des 368 x 240 angegeben werden. Die erste Zeile ist technisch nicht um ein echtes Gerät, aber statt der erwarteten Rendering Sprite, unabhängig von der geräteauflösung:


| **Gerät** | **Bildschirmauflösung** | **Beispiel-Screenshot** |
|--- | --- |--- |
|Gewünschten Anzeige|368 x 240 (mit schwarzen Balken für Seitenverhältnis)| ![368 x 240 (mit schwarzen Balken für Seitenverhältnis)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920 x 1080| ![iPhone 6 Plus 1920x1080](resolutions-images/image3.png) |

Dieses Dokument behandelt wie CocosSharp verwenden Sie zum Beheben des Problems in der obigen Tabelle gezeigt. D. h. müssen zu einem beliebigen Gerät rendern, wie in der ersten Zeile – unabhängig von der Auflösung angezeigt wie eingegangen.


# <a name="working-with-setdesignresolutionsize"></a>Arbeiten mit SetDesignResolutionSize

Die `CCScene` Klasse wird in der Regel als Stammcontainer für alle visuellen Objekte verwendet, bietet jedoch darüber hinaus eine statische Methode namens `SetDesignResolutionSize` für die Standardgröße für alle Szenen angeben. Mit anderen Worten `SetDesignResolutionSize` -Methode ermöglicht Entwicklern Spiele entwickeln, die auf einer Vielzahl von Hardware-Auflösung ordnungsgemäß angezeigt wird. Die Projektvorlagen für CocosSharp mit dieser Methode können Sie die Standardgröße des Projekts auf 1024 x 768 festgelegt, wie im folgenden Code gezeigt:


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

Sie können ändern, die `desiredWidth` und `desiredHeight` Variablen oben, um die Anzeige entsprechend die gewünschte Auflösung des Spiel zu ändern. Der obige Code kann z. B. wie folgt geändert werden, zum Anzeigen von 368 x 240 Hintergrund als Vollbild:


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


# <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` erlaubt es uns, um anzugeben, wie das Spiele Fenster auf die gewünschte Auflösung passt. Die folgenden Abschnitte zeigen, wie eine 500 x 500-Bild angezeigt wird, mit anderen `CCSceneResolutonPolicy` Werte, die zum Übergeben der `SetDesignResolutionSize` Methode. Die folgenden Werte werden bereitgestellt, indem die `CCSceneResolutionPolicy` Enum:

 - `ShowAll` – Zeigt die gesamte angeforderte Auflösung beibehalten des Seitenverhältnisses.
 - `ExactFit` – Zeigt die gesamte angeforderte Auflösung, aber keinen Seitenverhältnis beibehält.
 - `FixedWidth` – Zeigt die gesamte angeforderte Breite beibehalten des Seitenverhältnisses.
 - `FixedHeight` – Zeigt die gesamte angeforderte Höhe beibehalten des Seitenverhältnisses.
 - `NoBorder` – Zeigt die gesamte Höhe oder die gesamte Breite, Seitenverhältnis beibehalten. Wenn das Seitenverhältnis des Geräts das Seitenverhältnis des gewünschten Auflösung größer ist, wird die gesamte Breite angezeigt. Wenn das Seitenverhältnis des Geräts kleiner als das Seitenverhältnis des gewünschten Lösung ist, wird die gesamte Höhe angezeigt.
 - `Custom` – Die Anzeige richtet sich nach der `Viewport` Eigenschaft des aktuellen `CCScene`.

Alle Screenshots werden mit iPhone 4 s Auflösung (960 x 640) im Querformat erstellt und dieses Bild verwenden:

![](resolutions-images/image4.png "Alle Screenshots werden mit iPhone 4 s Auflösung 960 x 640 im Querformat erstellt und dieses Bild verwenden")


## <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` Gibt an, dass die gesamte Spiele Auflösung auf dem Bildschirm angezeigt werden, aber möglicherweise angezeigt *Letterbox-Effekt* (schwarze Balken) für unterschiedliche Seitenverhältnissen angepasst werden müssen. Diese Richtlinie wird häufig verwendet, da diese ZS garantiert, dass die gesamte Spiele Ansicht ohne jeder Verzerrung auf dem Bildschirm angezeigt wird.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterbox-Effekt ist sichtbar, links und rechts des Bilds für das physische Seitenverhältnis wird breiter als die gewünschte Auflösung zu berücksichtigen:

![](resolutions-images/image5.png "Letterbox-Effekt ist für sichtbar links und rechts des Bilds für das physische Seitenverhältnis breiter als die gewünschte Auflösung wird berücksichtigt")


## <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` Gibt an, dass die gesamte Spiele Auflösung mit ohne Letterbox-Effekt auf dem Bildschirm angezeigt werden. Der Anzeigebereich möglicherweise verzerrt (Seitenverhältnis möglicherweise nicht beibehalten werden) gemäß dem Seitenverhältnis der Hardware.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Keine Letterbox-Effekt ist sichtbar, aber seit dem geräteauflösung rechteckig ist Spiele Ansicht verzerrt wird:

![](resolutions-images/image6.png "Keine Letterbox-Effekt wird angezeigt, jedoch seit der geräteauflösung rechteckig ist verzerrt Spiel anzeigen")


## <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Gibt an, dass die Breite der Sicht an übergebenen Breitenwert CultureInfo.CurrentCulture.DateTimeFormat.FirstDayOfWeek `SetDesignResolutionSize`, aber die anzeigbare Höhe unterliegt das Seitenverhältnis des physischen Geräts. Dem Höhenwert übergeben `SetDesignResolutionSize` wird ignoriert, da er zur Laufzeit basierend auf dem physischen Gerät Seitenverhältnis berechnet wird. Dies bedeutet, dass die berechnete Höhe kleiner ist als die gewünschte Höhe (Vortäuschen in Teilen der Spiel Ansicht wird außerhalb des Bildschirmbereichs befindet), oder die berechnete Höhe möglicherweise größer als die gewünschte Höhe (Vortäuschen mehr der Spiel Ansicht angezeigt wird) werden kann. Da dies möglicherweise weitere das Spiel, das angezeigt wird möglicherweise angezeigt, als ob Letterbox-Effekt aufgetreten ist; Allerdings wird der zusätzliche Speicherplatz nicht unbedingt, black, wenn es visuellen Objekts angezeigt wird. 

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

Das iPhone 4 s hat ein Seitenverhältnis von 3:2, daher ist die berechnete Höhe ungefähr 333 Einheiten:

![](resolutions-images/image7.png "Das iPhone 4 s hat ein Seitenverhältnis von 3:2, daher ist die berechnete Höhe ungefähr 333 Einheiten")


## <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Im Prinzip `FixedHeight` verhält sich ähnlich wie `FixedWidth` – das Spiel wird dem Höhenwert übergeben unterliegen `SetDesignResolutionSize,` werden jedoch die Breite zur Laufzeit basierend auf der physischen Auflösung berechnet wird. Wie oben erwähnt, das heißt, das die Breite des angezeigte liegen off kleiner oder größer als die gewünschte Breite, die Teil der Spiel, wodurch Bildschirm oder mehrere der angezeigt wird, bzw. Spiel.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Da der folgende Screenshot mit der app im Querformat erstellt wurde, ist die berechnete Breite größer als 500 – insbesondere 750. Diese Richtlinie behält den X-Wert von 0: linksbündig, weshalb die zusätzliche Auflösung auf der rechten Seite des Bildschirms angezeigt werden.

![](resolutions-images/image8.png "Diese Richtlinie behält den X-Wert von 0: linksbündig, weshalb die zusätzliche Auflösung auf der rechten Seite des Bildschirms angezeigt werden")


## <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` versucht, die Anwendung mit ohne Letterbox-Effekt, während das Originalseitenverhältnis beibehalten (keine Verzerrung) anzuzeigen. Wenn die angeforderte Auflösung Seitenverhältnis physischen Seitenverhältnis des Geräts übereinstimmt, wird ohne Clipping auftreten. Falls Seitenverhältnissen nicht übereinstimmen, erfolgt klicken Sie dann Clippings.

Codebeispiel:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Der folgende Screenshot zeigt die oberen und unteren Teile der Anzeige zugeschnitten, während alle 500 Pixel, der die Breite der Anzeige angezeigt werden:

![](resolutions-images/image9.png "Diese Screenshot zeigt die oberen und unteren Teile der Anzeige zugeschnitten, während alle 500 Pixel, der die Breite der Anzeige angezeigt werden")


## <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` ermöglicht es, jede `CCScene` , geben Sie einen eigenen benutzerdefinierten Viewport relativ zu der im angegebenen Auflösung `SetDesignResolutionSize`.

Szenen definieren ihre `Viewport` Eigenschaft durch das Erstellen einer `CCViewport`, das wiederum erstellt wird mit einem `CCRect`. Weitere Informationen zu `CCViewport` und `CCRect` finden Sie unter der [CocosSharp API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/). Im Kontext der Erstellung einer `CCViewport` der `CCRect` Parameter des Konstruktors angeben (in entsprechender Reihenfolge):

 - X – der Betrag horizontal, Viewports versetzt, wobei jede Einheit für eine gesamte Bildschirmbreite darstellt. Verwenden z. B. einen Wert von .5f Ergebnissen in der Szene nach rechts verschoben, um die Hälfte der Bildschirmbreite.
 - y – der Betrag für vertikal, Viewports versetzt, wobei jede Einheit für eine gesamte Bildschirmhöhe darstellt. Verwenden z. B. einen Wert von .5f Ergebnissen in der Szene nach unten verschoben, um die Hälfte der Bildschirmhöhe.
 - Breite – die horizontale Teil, der die Szene einnehmen sollte. Verwenden Sie z. B. den Wert 1 / 3. 0f führt in der Szene horizontal ein Drittel des Bildschirms einnimmt.
 - Höhe – der vertikalen Teil, der die Szene einnehmen sollte. Verwenden Sie z. B. den Wert 1 / 3. 0f führt in der Szene vertikal ein Drittel des Bildschirms einnimmt.

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

Der obige Code ergibt sich Folgendes:

![](resolutions-images/image10.png "Der Code oben in diesem Screenshot Ergebnisse")


# <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

Die `DefaultTexelToContentSizeRatio` vereinfacht die Verwendung von Texturen mit höherer Auflösung auf Geräten Bildschirme mit höherer Auflösung. Insbesondere kann diese Eigenschaft Spiele höherer Auflösung Bestand verwenden ohne die Größe oder die Positionierung von visuellen Elementen ändern. 

Beispielsweise kann ein Spiel, das ein Medienobjekt Art für ein Spiel Zeichen der 200 Pixel hoch ist, und der Bildschirm enthalten (nach den Angaben von `SetDesignResolutionSize`) möglicherweise 400 Pixel hoch. In diesem Fall wird das Zeichen die Hälfte des Bildschirms belegen. Wenn ein 400 Pixel hoch Medienobjekt für das Zeichen für Geräte mit höherer Auflösung verwendet wurden, würde das Zeichen die gesamte Höhe der Anzeige belegen. Wenn dem Zeichen, um höhere Auflösung Geräte nutzen die gleiche Größe beibehalten wird, ist zusätzlicher Code notwendig, dass Sprite Zeichen am Hälfte der Höhe des Bildschirms angezeigt werden sollen.

Das oben angeführte einfachen Beispiel stellt ein häufiges Problem bei 3D-Spielentwicklung – Hinzufügen von Bestand größeres ohne den Entwickler, führen Sie für jedes visuelle Element entsprechend geräteauflösung Größe dar. `DefaultTexelToContentSizeRatio` eine globale Eigenschaft ist für die Größenänderung visuelle Elemente verwendet werden. Dieser Wert wird alle visuellen Elemente eines bestimmten Typs von der zugewiesene Wert.

Diese Eigenschaft ist in folgenden Klassen vorhanden: 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

Visual Studio für Mac Vorlagen Satz CocosSharp `CCSprite.DefaultTexelToContentSizeRatio` entsprechend der Breite des Geräts als Beispiel dafür, wie sie verwendet werden kann. Der folgende Code befindet sich im `CCApplicationDelegate.ApplicationDidFinishLaunching`:


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


## <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio Example

Um finden Sie unter wie `DefaultTexelToContentSizeRatio` wirkt sich auf die Größe des visuellen Elemente, sollten Sie den oben aufgeführten Code:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

In der folgenden Abbildung, die mit einer Textur 500 x 500 ausbremsen:

![](resolutions-images/image5.png "In diesem Bild mithilfe einer Textur 500 x 500 ausbremsen")

Die iPad wird mit einer physischen Auflösung 2048 x 1536 Retina ausgeführt. Dies bedeutet, dass das Spiel oben angezeigten 500 Pixel über 1536 physischen Pixel gestreckt werden würde. Spiele Nutzen dieser zusätzlichen Auflösung durch das Erstellen, in der Regel als "" bezeichnet werden *hd* dienen – Anlagen denen auf Bildschirme mit höherer Auflösung ausgeführt werden. Dieses Spiel kann z. B. eine hd-Version dieser Textur bei 1000 x 1000 Größe verwenden. Allerdings mit größeren Bilds würde die `CCSprite` mit einem Breite und Höhe des 1000 Einheiten. Da unsere Spiel 500 Einheiten immer angezeigt werden (aufgrund der `SetDesignResolutioSize` aufrufen), dann unsere 1000 x 1000 Sprite wäre zu groß ist (nur im unteren linken Bereich des Zertifikats angezeigt):

![](resolutions-images/image11.png "Da das Spiel 500 Einheiten aufgrund des Aufrufs SetDesignResolutioSize immer angezeigt werden, wäre 1000 x 1000 Sprite zu groß, dass nur die unteren linken Teil davon werden angezeigt")

Legen wir `CCSprite.DefaultTexelToContentSizeRatio` wird zweimal so breit und hoch als die originaltextur 500 x 500-1000 x 1000 Textur berücksichtigen:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Beim Ausführen des Spiels wird die Textur 1000 x 1000 jetzt vollständig sichtbar sein:

![](resolutions-images/image12.png "Jetzt beim Ausführen des Spiels wird die Textur 1000 x 1000 vollständig sichtbar sein")


## <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio Details

Die `DefaultTexelToContentSizeRatio` Eigenschaft `static,` was bedeutet, dass alle Sprites in der Anwendung gemeinsam verwenden den gleichen Wert. Der typische Ansatz für Spiele mit Ressourcen für andere Lösungen hergestellt ist, einen vollständigen Satz von Ressourcen für jede Kategorie Lösung enthalten. Standardmäßig CocosSharp Visual Studio für Mac-Vorlagen bieten **%ld** und **hd** Ordner für Medienobjekte, Spiele, die zwei Sätze von Texturen Unterstützung hilfreich sein würde. Ein Beispiel Inhaltsordner mit Inhalt möglicherweise formuliert werden:

![](resolutions-images/image13.png "Sähe beispielsweise wie ein Beispielordner Inhalte mit Inhalt")

Beachten Sie, dass die Standardvorlage auch Code zum Hinzufügen von bedingt an der Anwendungsverzeichnis enthält `ContentSearchPaths`:


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

Dies bedeutet, dass die Anwendung sucht nach Dateien in Pfade entsprechend, ob er in hoher Auflösung oder reguläre Auflösung-Modus ausgeführt wird. Z. B. wenn die **hd** und **%ld** Ordner enthalten eine Datei namens **background.png** der folgende Code ausführen würden, und wählen Sie die richtige Datei für die Auflösung:


```csharp
backgroundSprite  = new CCSprite ("background");
```


# <a name="summary"></a>Zusammenfassung

In diesem Artikel wird beschrieben, wie zum Erstellen von spielen die unabhängig von der geräteauflösung richtig angezeigt. Es zeigt Beispiele für die Verwendung unterschiedliche `CCSceneResolutionPolicy` Werte für das Spiel gemäß der geräteauflösung Größe. Es bietet auch ein Beispiel dafür, wie `DefaultTexelToContentSizeRatio` können verwendet werden, um mehrere Sätze von Inhalt zu ermöglichen, ohne Sie visuelle Elemente einzeln geändert werden soll.

## <a name="related-links"></a>Verwandte Links

- [Einführung in die CocosSharp](~/graphics-games/cocossharp/first-game/index.md)
- [CocosSharp API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/)
