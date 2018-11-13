---
title: Leistung und visuelle Effekte mit CCRenderTexture
description: CCRenderTexture ermöglicht es Entwicklern, die Leistung ihrer CocosSharp-Spiele zu verbessern, indem Sie die Draw-Aufrufe zu reduzieren, und für das Erstellen von visuellen Effekten verwendet werden kann. Dieses Handbuch begleitet CCRenderTexture Beispiel um ein praktisches Beispiel der Verwendung dieser Klasse effektiv bereitzustellen.
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 95227689303a8367785202956a6aaef921c1c593
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526441"
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Leistung und visuelle Effekte mit CCRenderTexture

_CCRenderTexture ermöglicht es Entwicklern, die Leistung ihrer CocosSharp-Spiele zu verbessern, indem Sie die Draw-Aufrufe zu reduzieren, und für das Erstellen von visuellen Effekten verwendet werden kann. Dieses Handbuch begleitet CCRenderTexture Beispiel um ein praktisches Beispiel der Verwendung dieser Klasse effektiv bereitzustellen._

Die `CCRenderTexture` -Klasse enthält Funktionen für das Rendern mehrerer CocosSharp-Objekte, um eine einzelne Textur. Nach der Erstellung `CCRenderTexture` Instanzen können verwendet werden, um effizientes Rendering der Grafiken und visuelle Effekte zu implementieren. `CCRenderTexture` können mehrere Objekte auf einer einzelnen Struktur einmal gerendert werden soll. Klicken Sie dann diese Textur möglich wiederverwendet jedes Bild reduzieren die Gesamtzahl der Draw-Aufrufe.

Dieses Handbuch untersucht, wie Sie verwenden die `CCRenderTexture` Objekt zur Verbesserung der Leistung, der Rendering-Karten in einem sammelbare Kartenspiel (CCG). Außerdem wird veranschaulicht, wie `CCRenderTexture` kann verwendet werden, um eine gesamte Entität transparent zu gestalten. Dieses Handbuch verweist auf die `CCRenderTexture` [Beispielprojekt](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Dieses Handbuch verweist auf das Beispielprojekt CCRenderTexture")


## <a name="card--a-typical-entity"></a>Karte – eine typische Entität

Vor dem anzusehen, wie mit `CCRenderTexture` Objekt, wir werden zunächst vertraut machen uns mit den `Card` Entität, die wir in diesem Projekt verwende, um zu untersuchen der `CCRenderTexture` Klasse. Die `Card` Klasse ist eine typische Entität, die nach dem Entity-Muster beschrieben, die in der [Entität Handbuch](~/graphics-games/cocossharp/entities.md). Die Smartcard-Klasse verfügt über alle zugehörigen visuellen Komponenten (Instanzen von `CCSprite` und `CCLabel`) als Felder aufgeführt:


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

Instanzen der Karte gerendert werden können, mithilfe einer `CCRenderTexture`, oder Zeichnen jede visuelle Komponente einzeln. Obwohl jede Komponente ein unabhängiges Objekt ist, ist der `CCNode` überordnen in Entitäten verwendet wird der `Card` Verhalten sich wie ein einzelnes Objekt – zumindest in den meisten Fällen. Z. B. wenn ein `Card` Entität neu angeordnet, geändert oder gedreht wird, und klicken Sie dann alle die visuellen Objekte betroffen sind, um die Karte angezeigt werden, um ein einzelnes Objekt zu machen. Um die Karten, die als einzelnes Objekt verhält sich anzuzeigen, ändern wir die `GameLayer.AddedToScene` Methode zum Festlegen der `useRenderTextures` Variable `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

Die `GameLayer` Code verschiebt nicht jedes visuellen Element unabhängig voneinander noch jedes visuelle Element innerhalb der `Card` Entität korrekt positioniert ist:

![](ccrendertexture-images/image1.png "Der GameLayer Code verschiebt jedes visuelle Element nicht unabhängig voneinander, aber jedes visuelle Element in der Karte Entität richtig positioniert")

Im Beispiel wird codiert, um zwei Probleme verfügbar zu machen, die auftreten können, wenn sich jede visuelle Komponente rendert:

- Leistung kann aufgrund mehrerer zeichnen beeinträchtigt werden.
- Bestimmte visuelle Effekte wie Transparenz können nicht genau, implementiert werden, wie weiter unten erläutert wird


### <a name="card-draw-calls"></a>Karte Draw-Aufrufe

Eine Vereinfachung des was in einer vollständigen zu finden sind, ist der Code *sammelbare Kartenspiel* (CCG) wie z. B. "Magic-Befehl: das Sammeln von" oder "Hearthstone". Unser Spiel nur drei Karten auf einmal angezeigt und verfügt über eine kleine Anzahl von möglichen Einheiten (Blau, Grün und Orange). Im Gegensatz dazu ein vollständiger Spiel möglicherweise mehr als 20 Karten auf dem Bildschirm zu einem bestimmten Zeitpunkt und Spieler möglicherweise Hunderte von Karten auswählen, wenn ihre Decks zu erstellen. Auch wenn unser Spiel nicht aktuell von Leistungsproblemen der Fall ist, kann ein vollständiger Spiel mit gleicher Implementierung.

CocosSharp bietet ein Einblick in die Leistung beim Rendern durch Verfügbarmachen von den Draw-Aufrufen pro-Frame ausgeführt. Unsere `GameLayer.AddedToScene` Methode legt die `GameView.Stats.Enabled` zu `true`, wodurch Leistungsinformationen auf der unteren linken Ecke des Bildschirms angezeigt:

![](ccrendertexture-images/image2.png "Die GameLayer.AddedToScene-Methode legt die GameView.Stats.Enabled auf \"true\", wodurch Leistungsinformationen angezeigt werden, an der unteren linken Ecke des Bildschirms")

Beachten Sie, dass trotz drei Karten auf dem Bildschirm, wir 19 Draw-Aufrufe (jede Karte Ergebnisse in sechs zeichnen-Befehle, den Text, der die Konten der Performance-Informationen für ein weiteres anzeigen). Draw-Aufrufe haben einen erheblichen Einfluss auf die Leistung des Spiels, daher bietet von CocosSharp eine Reihe von Möglichkeiten, um sie zu reduzieren. Ein Verfahren wird beschrieben, der [CCSpriteSheet Handbuch](~/graphics-games/cocossharp/ccspritesheet.md). Eine andere Möglichkeit ist die Verwendung der `CCRenderTexture` , jede Entität zu einem Aufruf zu reduzieren, da wir in diesem Handbuch untersuchen.


### <a name="card-transparency"></a>Transparenz der Karte

Unsere `Card` Entität enthält eine `Opacity` Eigenschaft zum Steuerelement Transparenz, wie im folgenden Codeausschnitt gezeigt:


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

Beachten Sie, dass die Set-Methode unterstützt die Verwendung von Texturen oder Rendern jede Komponente einzeln gerendert. Ändern, um die Auswirkungen anzuzeigen, die `opacity` Wert `127` (ungefähr halb Undurchsichtigkeit) in `GameLayer.AddedToScene` die führt zu jeder Komponente mit einer `Opacity` Wert `127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

Das Spiel wird nun die Karten einige Transparenz, verursacht sie dunkler angezeigt werden, da der Hintergrund Schwarz ist ausgegeben:

![](ccrendertexture-images/image3.png "Das Spiel wird nun die Karten einige Transparenz, verursacht sie dunkler angezeigt werden, da der Hintergrund Schwarz ist gerendert")

Auf den ersten Blick können sie sehen, dass unsere Karten ordnungsgemäß transparent vorgenommen wurden. Der Screenshot zeigt jedoch eine Reihe von visuellen Problemen.

Da unsere Hintergrund Schwarz ist, erwarten wir, dass jeder Teil unserer Karte aufgrund der Transparenz dunkler werden sollte. D. h. die transparenter wird eine Karte, je dunkler wird. Bei einer Deckkraft von 0 (null) ein `Card` vollständig transparent (völlig Schwarz). Allerdings einige Teile unserer Karte nicht werden dunkler Wenn Durchlässigkeit, um geändert wurde `127`. Schlimmer noch, wurde einige Teile unserer Karte tatsächlich hellere, wenn sie transparenter ist. Sehen wir uns Teile unserer Karte auf dem schwarzen *vor* Kacheln Hintergrund erfolgen – insbesondere der Detail-Text und der schwarze wird erläutert, um die Data Science-ungeheuers Grafik. Wenn wir diese nebeneinander platzieren, können wir die Auswirkungen der Anwendung von Transparenz finden Sie unter:

![](ccrendertexture-images/image4.png "Wenn nebeneinander platzieren, kann die Auswirkungen der Anwendung von Transparenz angezeigt werden")

Wie bereits erwähnt, alle Teile der Karte sollten werden, je dunkler Wenn transparenter zu, aber in eine Reihe von Bereichen ist dies nicht der Fall:

- Die Robots Kontur wird heller (geht von Schwarz über grau)
- Der Beschreibungstext ist heller (geht von Schwarz über grau)
- Der grüne Anteil der Roboter weniger ausgelastet ist, aber nicht dunkler werden

Damit können visualisieren, warum dies der Fall, müssen wir bedenken, dass jedes visuelle Komponente gezeichneten unabhängig voneinander sind, jeweils ein teilweise transparente. Die erste visuelle Komponente, die gezeichnet wird der Karte Hintergrund. Nachfolgende transparente Elemente auf der Karte gezeichnet werden und sind davon betroffen, mit der Karte. Wenn wir Entfernen von Text aus unserer Karte und die Grafik Roboter nach unten verschieben, sehen wir, wie die Karte Hintergrund des Roboters wirkt sich auf. Beachten Sie, dass die orangefarbene Linie in das obere Feld für den Roboter angezeigt werden, und der Bereich des Roboters die blaue Stripeset in der Mitte der Karte überschneidet sich mit dunkler gezeichnet wird:

![](ccrendertexture-images/image5.png "Beachten Sie, dass die orangefarbene Linie in das obere Feld für den Roboter angezeigt werden, und der Bereich des Roboters die blaue Stripeset in der Mitte der Karte überschneidet sich mit dunkler gezeichnet wird")

Mit einem `CCRenderTexture` können wir die gesamte Karte transparent machen ohne Auswirkungen auf das Rendern von einzelne Komponenten innerhalb der Karte, wie wir später in dieser Anleitung sehen.


## <a name="using-ccrendertexture"></a>Verwenden von CCRenderTexture

Nun, da wir die Probleme bei der jede Komponente einzeln Rendern identifiziert haben, müssen wir Rendern einschalten einer `CCRenderTexture` und vergleichen Sie das Verhalten.

Rendering Aktivieren einer `CCRenderTexture`, Ändern der `userRenderTextures` Variable `true` in `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>Karte Draw-Aufrufe

Wenn wir das Spiel jetzt ausführen, sehen wir die Draw-Aufrufen von 19 auf vier reduziert (jede Karte, die von sechs auf eine reduziert wird):

![](ccrendertexture-images/image6.png "Wenn das Spiel nun ausgeführt wird, die Draw-Aufrufen von reduziert 19 auf vier jede Karte von sechs auf eine reduziert")

Wie bereits erwähnt haben diese Art von Reduzierung einen erheblichen Einfluss auf Spiele mit Weitere visual Entitäten auf dem Bildschirm.


### <a name="card-transparency"></a>Transparenz der Karte

Sobald die `useRenderTextures` nastaven NA hodnotu `true`, transparent Karten unterschiedlich dargestellt werden:

![](ccrendertexture-images/image7.png "Nach dem Festlegen der UseRenderTextures auf \"true\" werden transparent wird anders gerendert.")

Vergleichen wir die transparente Roboter-Karte mit Rendering von Texturen (links) oder ohne (rechts):

![](ccrendertexture-images/image8.png "Vergleichen Sie die transparente Roboter Karte verwenden Render Texturen (linken) im Vergleich ohne (rechts)")

Die offensichtlichste Unterschiede werden in den Details-Text (Schwarz anstelle von hellgrau) und dem Roboter Sprite (dunkel anstelle von Licht und entsättigte).


## <a name="ccrendertexture-details"></a>CCRenderTexture-details

Nun, da wir die Vorteile der Verwendung gesehen haben `CCRenderTexture`, werfen wir einen Blick auf die Verwendung in der `Card` Entität.

Die `CCRenderTexture` ist ein Zeichenbereich, der das Ziel, der Rendering werden kann. Er hat zwei Hauptunterschiede, im Vergleich zu dem Spiel Bildschirm:

1. Die `CCRenderTexture` weiterhin die dazwischen Frames besteht. Dies bedeutet, dass eine `CCRenderTexture` nur gerendert werden, wenn Änderungen auftreten muss. In unserem Fall die `Card` Entität sich nie ändert, sodass sie nur einmal gerendert wird. Wenn alle `Card` Komponenten geändert werden soll, dann würde müssen Sie die Karte selbst neu zeichnet seine `CCRenderTexture`. Z. B. wenn der HP-Wert (Integrität Punkt) geändert, wenn angegriffen, würde dann müssen Sie die Karte auf sich selbst entsprechend den neuen Wert für den HP zu rendern.
1. Die `CCRenderTexture` Pixeldimensionen sind nicht auf dem Bildschirm gebunden. Ein `CCRenderTexture` größer oder kleiner als die Auflösung des Geräts werden können. Die `Card` Code erstellt eine `CCRenderTexture` mithilfe des Umfangs der der Hintergrund Sprite. Die Karte enthält einen Verweis auf eine `CCRenderTexture` namens `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

Die `renderTexture` Instanz bleibt `null` bis der `UseRenderTexture` Eigenschaft zugewiesen ist, auf "true", welche Aufrufe `SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

Die `SwitchToRenderTexture` Methode kann aufgerufen werden, wenn die Textur aktualisiert werden muss. Kann aufgerufen werden kann, unabhängig davon, ob die Karte bereits ist seine `CCRenderTexture` oder ist der Wechsel zu den `CCRenderTexture` zum ersten Mal.

Die folgenden Abschnitte untersuchen der `SwitchToRenderTexture` Methode. 


### <a name="ccrendertexture-size"></a>CCRenderTexture Größe

Der CCRenderTexture-Konstruktor erfordert zwei Sätze von Dimensionen. Die erste steuert die Größe des der `CCRenderTexture` beim Zeichnen, und der zweite gibt die Breite in Pixel und die Höhe des Inhalts. Die `Card` Entität instanziiert die `CCRenderTexture` mithilfe der im Hintergrund [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Unser Spiel verfügt über eine `DesignResolution` von 512 von 384, siehe `ViewController.LoadGame` unter iOS und `MainActivity.LoadGame` unter Android:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

Die `CCRenderTexture` Konstruktor wird aufgerufen, mit der `background.ContentSize` als ersten Parameter gibt an, dass die `CCRenderTexture` sollte nur so groß wie der Hintergrund `CCSprite`. Seit der Karte Hintergrund `CCSprite` 200 Pixel hoch ist, wird die Karte wird ungefähr die Hälfte der Höhe des Bildschirms einnimmt.

Der zweite Parameter zu übergeben, um die `CCRenderTexture` Konstruktor gibt an, die Pixel-Auflösung, der die `CCRenderTexture`. Siehe die [CocosSharp-Auflösung Handbuch](~/graphics-games/cocossharp/resolutions.md), die Breite und Höhe des sichtbaren Bereichs in Spielen Einheiten ist oft nicht identisch mit den Pixel-Auflösung des Bildschirms. Auf ähnliche Weise können eine CCRenderTexture eine größeren Auflösung als die Größe, sodass schärfere auf Geräten mit hoher Auflösung angezeigt werden.

Die Auflösung ist doppelt so groß der CCRenderTexture, um zu verhindern, dass der Text aus aussehende verpixelt dargestellt:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Um zu vergleichen, können wir den PixelResolution-Wert entsprechend ändern der `background.ContentSize` (ohne verdoppelt wird) und vergleichen das Ergebnis: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Um zu vergleichen, kann die PixelResolution-Wert entsprechend den Hintergrund ändern. ContentSize ohne verdoppelt wird, und Vergleichen des Ergebnisses")


### <a name="rendering-to-a-ccrendertexture"></a>Rendern in einem CCRenderTexture

Visuelle Objekte in CocosSharp werden in der Regel nicht explizit gerendert. Stattdessen visuelle Objekte hinzugefügt werden eine `CCLayer` diese ist Teil einer `CCScene`. CocosSharp automatisch rendert die `CCScene` und dessen visuelle Hierarchie in jedem Frame ohne Renderingcode aufgerufen wird. 

Im Gegensatz dazu die `CCRenderTexture` explizit zu gezeichnet werden muss. Dieses Rendering kann in drei Schritte unterteilt werden:

1. `CCRenderTexture.BeginWithClear` wird aufgerufen, gibt an, dass die gesamte nachfolgende Rendering der aufrufenden abzielen `CCRenderTexture`.
1. Objekte erben von `CCNode` (z. B. die `Card` Entität) gerendert werden die `CCRenderTexture` durch Aufrufen von `Visit`.
1. `CCRenderTexture.End` wird aufgerufen, der angibt, des Rendern auf dem `CCRenderTexture` abgeschlossen ist.

Eine beliebige Anzahl von Objekten gerendert werden kann, um eine `CCRenderTexture` zwischen der `Begin` und `End` aufrufen. Vor dem Rendern, werden alle erforderlichen sichtbaren Objekte als untergeordnete Elemente hinzugefügt:


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

Die `renderTexture` sollte nicht Bestandteil der Karte beim Rendern, damit er entfernt wird:


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Jetzt die `Card` Instanz kann zum Rendern der `CCRenderTexture` Instanz:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

Die einzelnen Komponenten werden entfernt, wenn das Rendering abgeschlossen ist, und die `CCRenderTexture` erneut hinzugefügt wird.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt die `CCRenderTexture` Klasse, indem eine `Card` Entität, die in einer entladbaren Kartenspiel verwendet werden kann. Es wurde gezeigt, wie Sie mit der `CCRenderTexture` -Klasse zum Verbessern der Framerate und Transparenz der gesamten Entität ordnungsgemäß zu implementieren.

Obwohl dieses Handbuch verwendet eine `CCRenderTexture` innerhalb einer Entität enthalten, kann diese Klasse verwendet, um das Rendern von mehreren Entitäten oder sogar ganze sein `CCLayer` -Instanzen für den gesamten Bildschirm Auswirkungen und die Leistung verbessert.

## <a name="related-links"></a>Verwandte Links

- [CCRenderTexture-API-Referenz](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Vollständiges-Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
