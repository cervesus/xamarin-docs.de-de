---
title: Leistung und visuelle Effekte mit CCRenderTexture
description: "CCRenderTexture ermöglicht Entwicklern das verbessern die Leistung ihrer CocosSharp Spiele, durch das Reduzieren der Draw-Aufrufe und kann zum Erstellen von visuellen Effekten verwendet werden. Diese Anleitung begleitet CCRenderTexture-Beispiel, um eine praktische Beispiel dafür, wie effektive Verwendung von dieser Klasse bereitzustellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 8283c299d0e6529ef4cf8c285ec47b4d42fc682a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Leistung und visuelle Effekte mit CCRenderTexture

_CCRenderTexture ermöglicht Entwicklern das verbessern die Leistung ihrer CocosSharp Spiele, durch das Reduzieren der Draw-Aufrufe und kann zum Erstellen von visuellen Effekten verwendet werden. Diese Anleitung begleitet CCRenderTexture-Beispiel, um eine praktische Beispiel dafür, wie effektive Verwendung von dieser Klasse bereitzustellen._

Die `CCRenderTexture` -Klasse bietet eine Funktionalität für das Rendern mehrerer CocosSharp-Objekte, um eine einzelne Struktur. Nachdem dieses erstellt wurde, `CCRenderTexture` Instanzen verwendet werden können, Grafiken effizientes Rendering und visuelle Effekte zu implementieren. `CCRenderTexture` können mehrere Objekte in einer einzigen Textur einmal gerendert werden. Klicken Sie dann, diese Struktur kann jeden Frame, reduzieren die Gesamtanzahl der Draw-Aufrufe wiederverwendet.

Dieses Handbuch untersucht, wie Sie die `CCRenderTexture` Objekt zum Verbessern der Leistung der Rendering-Karten in einem sammelbare Kartenspiels (CCG). Außerdem wird veranschaulicht, wie `CCRenderTexture` können verwendet werden, um eine gesamte Entität transparent zu gestalten. Dieses Handbuch verweist auf die `CCRenderTexture` [Beispielprojekt](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Dieses Handbuch verweist auf das Beispielprojekt CCRenderTexture")


# <a name="card--a-typical-entity"></a>Karte – eine Typische Entität

Vor der Verwendung ansehen `CCRenderTexture` -Objekt, wir werden uns mit zuerst vertraut der `Card` Entität, die innerhalb dieses Projekts verwendet wird, untersuchen die `CCRenderTexture` Klasse. Die `Card` Klasse ist eine typische Entität, die die Entität Muster beschrieben, die in der [Entität Handbuch](~/graphics-games/cocossharp/entities.md). Die Smartcard-Klasse verfügt über alle visual Komponenten (Instanzen von `CCSprite` und `CCLabel`) als Felder aufgeführt:


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

Karte Instanzen gerendert werden können, mithilfe einer `CCRenderTexture`, oder durch jede visuelle Komponente einzeln zu zeichnen. Obwohl jede Komponente auf einem unabhängigen Objekt ist die `CCNode` macht Überordnung in Entitäten verwendet die `Card` Verhalten sich wie ein einzelnes Objekt – mindestens zum größten Teil. Z. B. wenn ein `Card` Entität neu angeordnet, geändert oder gedreht wird, und alle darin enthaltenen visuellen Objekte betroffen sind, um die Karte, die ein einzelnes Objekt zu sein scheinen zu machen. Um den Karten, verhalten sich wie ein einzelnes Objekt anzuzeigen, können wir ändern die `GameLayer.AddedToScene` -Methode zum Festlegen der `useRenderTextures` Variable `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

Die `GameLayer` Code verschiebt nicht jedes visuellen Element unabhängig, noch jedes visuelle Element innerhalb der `Card` Entität korrekt positioniert ist:

![](ccrendertexture-images/image1.png "Der Code GameLayer verschiebt jedes visuelle Element nicht unabhängig, aber jedes visuelle Element innerhalb der Entität Karte korrekt positioniert ist")

Im Beispiel wird die nötige um zwei Probleme verfügbar zu machen, die auftreten können, wenn jedes visuelle Komponente selbst gerendert wird:

- Aufgrund von mehreren Draw-Aufrufe kann die Leistung beeinträchtigt.
- Bestimmte visuelle Effekte wie Transparenz, können nicht genau, implementiert werden, wie wir später untersuchen


## <a name="card-draw-calls"></a>Karte Draw-Aufrufe

Getesteten Codes ist eine Vereinfachung in eine vollständige gefundenen werden möglicherweise *sammelbare Kartenspiels* (CCG) wie z. B. "Magic: das Sammeln von" oder "Hearthstone". Unsere Spiel nur drei Karten gleichzeitig angezeigt und verfügt über eine kleine Anzahl von möglichen Einheiten (Blau, Grün und Orange). Im Gegensatz dazu, eine vollständige Spiel möglicherweise mehr als zwanzig Karten auf dem Bildschirm zu einem bestimmten Zeitpunkt und Player möglicherweise Hunderte von Karten auswählen, wenn ihre Kartenstapel zu erstellen. Obwohl unsere Spiel von Leistungsproblemen derzeit nicht beeinträchtigt wird, kann eine vollständige Spiels mit ähnliche Implementierung.

CocosSharp bietet ein Einblick in die Renderingleistung, verfügbar machen, die Zeichnen-Aufrufe pro-Frame ausgeführt. Unsere `GameLayer.AddedToScene` Methode legt die `GameView.Stats.Enabled` zu `true`, wodurch Leistungsinformationen auf der linken unteren Ecke des Bildschirms angezeigt:

![](ccrendertexture-images/image2.png "Die GameLayer.AddedToScene Methode legt die GameView.Stats.Enabled auf "true", wodurch auf der linken unteren Ecke des Bildschirms angezeigten Leistungsinformationen")

Beachten Sie, dass trotz der drei Karten auf Bildschirm 19 Draw-Aufrufe (jede Karte Ergebnisse in sechs zeichnen-Befehle, die Konten der Leistung Informationen für eine oder mehrere Text). Draw-Aufrufe haben einen entscheidenden Einfluss auf die Leistung eines Spiels, damit CocosSharp eine Reihe von Verfahren zum Verringern der ihnen bietet. Ein Verfahren wird beschrieben, der [CCSpriteSheet Handbuch](~/graphics-games/cocossharp/ccspritesheet.md). Eine andere Technik ist die Verwendung der `CCRenderTexture` auf jede Entität zu einem Aufruf reduziert werden, weil wir in diesem Handbuch untersuchen.


## <a name="card-transparency"></a>Transparenz der Karte

Unsere `Card` Entität enthält ein `Opacity` Eigenschaft zum Steuerelement Transparenz wie im folgenden Codeausschnitt gezeigt:


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

Beachten Sie, dass der Setter-Methode unterstützt die Verwendung von Texturen oder jede Komponente einzeln rendering gerendert. Um ihre Auswirkungen zu sehen, Ändern der `opacity` Wert `127` (ungefähr die Hälfte Deckkraft) in `GameLayer.AddedToScene` Was führt jede Komponente weist eine `Opacity` Wert `127`:


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

Das Spiel wird nun die Karten mit einigen Transparenz, wodurch dunkler angezeigt werden, da der Hintergrund Schwarz ist gerendert:

![](ccrendertexture-images/image3.png "Das Spiel wird nun die Karten mit einigen Transparenz, wodurch dunkler angezeigt werden, da der Hintergrund Schwarz ist Rendern")

Auf den ersten Blick sieht es wie bei unserem Karten ordnungsgemäß transparent vorgenommen wurden. Der Screenshot zeigt jedoch eine Reihe von visuellen Problemen.

Da unsere Hintergrund Schwarz ist, erwarten wir, dass jeder Teil unserer Karte aufgrund der Transparenz dunkler angezeigt werden würden. Das heißt, desto transparent eine Karte wird, desto stärker. Bei einer Deckkraft 0 (null) ein `Card` vollständig transparent (vollständig schwarz). Jedoch einige Teile der unsere Karte haben nicht werden dunkler Deckkraft geändert wurde, um `127`. Einige Teile der unsere Karte wurde noch schlimmer, tatsächlich heller, wenn sie transparenter ist seit. Sehen wir uns Teile unserer Karte Schwarz wurden *vor* waren transparent – insbesondere Detail Text und die schwarzen umrissen, um die Grafik Monster. Wenn wir diese nebeneinander platzieren, sehen wir die Auswirkung der Transparenz anwenden:

![](ccrendertexture-images/image4.png "Wenn nebeneinander platziert werden, kann die Auswirkungen der Transparenz der Anwendung angezeigt werden")

Wie bereits erwähnt, alle Teile der Karte sollte werden dunkler Wenn transparenter zu, aber in verschiedenen Bereichen ist dies nicht der Fall:

- Gliederung des Roboters wird heller (wechselt von Schwarz in grau)
- Der Beschreibungstext wird heller (wechselt von Schwarz in grau)
- Der grüne Anteil des Roboters wird weniger ausgelastet ist, aber nicht dunkler werden

Damit visualisieren, warum dieser Fehler tritt, müssen es zu bedenken, dass jedes visuelle Komponente gezeichneten unabhängig sind, jedes halbtransparenten aus. Die erste visual-Komponente, die gezeichnet wird die Karte im Hintergrund. Nachfolgende transparent Elemente auf der Karte gezeichnet werden und werden mit dem Karte beeinträchtigt. Wenn wir unsere Karte Text aufheben und die Grafik Roboter nach unten verschieben, können wir sehen, wie die Karte im Hintergrund des Roboters wirkt sich auf. Beachten Sie die orange Zeile im oberen Feld auf die Roboter angezeigt werden und der Clientbereich des Roboters der blauen Stripe in der Mitte der Karte überlappt dunkler gezeichnet wird:

![](ccrendertexture-images/image5.png "Beachten Sie die orange Zeile im oberen Feld auf die Roboter angezeigt werden und der Clientbereich des Roboters der blauen Stripe in der Mitte der Karte überlappt dunkler gezeichnet wird")

Mit einem `CCRenderTexture` erlaubt es uns, die gesamte Karte transparent zu gestalten ohne Auswirkungen auf das Rendering einzelne Komponenten innerhalb der Karte, da wir später in diesem Handbuch angezeigt wird.


# <a name="using-ccrendertexture"></a>Verwenden von CCRenderTexture

Nun, dass wir die Probleme mit Rendern jede Komponente einzeln identifiziert haben, müssen wir Rendering einschalten einer `CCRenderTexture` und vergleichen Sie das Verhalten.

Rendering Aktivieren einer `CCRenderTexture`, Ändern der `userRenderTextures` Variable `true` in `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


## <a name="card-draw-calls"></a>Karte Draw-Aufrufe

Wenn wir das Spiel jetzt ausführen, sehen wir die Draw-Aufrufe von 19 auf vier reduziert (jede Karte, die von 6 aus, um eine reduziert wird):

![](ccrendertexture-images/image6.png "Wenn das Spiel nun ausgeführt wird, die Zeichnen von reduziert 19 auf vier jede Karte von 6 aus, um eine reduziert")

Wie bereits erwähnt kann diese Art der Reduzierung einen entscheidenden Einfluss auf Spiele mit mehreren visual Entitäten auf dem Bildschirm haben.


## <a name="card-transparency"></a>Transparenz der Karte

Einmal die `useRenderTextures` festgelegt ist, um `true`, transparente Karten unterschiedlich dargestellt werden:

![](ccrendertexture-images/image7.png "Nach der UseRenderTextures auf "true" werden transparent Karten festlegen werden anders gerendert.")

Vergleichen wir die transparente Roboter Karte mit Render Texturen (links) und ohne (rechts):

![](ccrendertexture-images/image8.png "Vergleichen Sie die transparente Roboter Karte mit Render Texturen (linken) Vs ohne (rechts)")

Die offensichtliche Unterschiede sind in den Details Text (schwarz statt hellgrau) und Roboter Sprite (anstelle von Licht dunkel und ohne Sättigung).


# <a name="ccrendertexture-details"></a>CCRenderTexture-Details

Nun, dass wir die Vorteile der Verwendung gesehen haben `CCRenderTexture`, werfen wir einen Blick auf die Verwendung in der `Card` Entität.

Die `CCRenderTexture` ist ein Zeichenbereich, der das Ziel des Renderings sein kann. Es sind zwei Hauptunterschiede gegenüber dem Spiel Bildschirm:

1. Die `CCRenderTexture` weiterhin dazwischen Frames besteht. Dies bedeutet, dass eine `CCRenderTexture` nur gerendert werden, wenn Änderungen auftreten muss. In unserem Fall die `Card` Entität nie ändert, sodass er nur einmal gerendert wird. Wenn alle `Card` Komponenten geändert, und klicken Sie dann die Karte müssten sich selbst neu zeichnet seine `CCRenderTexture`. Z. B. der HP-Wert (Integrität Punkt) geändert, während angegriffen, würde dann müssen Sie die Karte selbst entsprechend den neuen Wert für den HP gerendert.
1. Die `CCRenderTexture` Pixeldimensionen nicht auf dem Bildschirm gebunden sind. Ein `CCRenderTexture` kann größer oder kleiner als die Auflösung des Geräts. Die `Card` Code erstellt eine `CCRenderTexture` mithilfe des Umfangs der seine Sprite Hintergrund. Die Karte enthält einen Verweis auf eine `CCRenderTexture` aufgerufen `renderTexture`:


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

Die `SwitchToRenderTexture` Methode kann aufgerufen werden, wenn die Struktur aktualisiert werden muss. Es kann aufgerufen werden, unabhängig davon, ob die Karte bereits ist seine `CCRenderTexture` oder wechselt die `CCRenderTexture` zum ersten Mal.

Untersuchen Sie die folgenden Abschnitte der `SwitchToRenderTexture` Methode. 


## <a name="ccrendertexture-size"></a>CCRenderTexture Größe

Der Konstruktor CCRenderTexture erfordert zwei Sätze von Dimensionen. Die erste steuert die Größe des der `CCRenderTexture` Wenn es gezeichnet wird, und der zweite gibt die Breite in Pixel und die Höhe des Inhalts. Die `Card` Entität instanziiert die `CCRenderTexture` Hintergrund mit [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Unsere Spiel verfügt über eine `DesignResolution` von 512 von 384, die wie gezeigt in `ViewController.LoadGame` auf iOS- und `MainActivity.LoadGame` auf Android-Geräten:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

Die `CCRenderTexture` Konstruktor wird aufgerufen, mit der `background.ContentSize` als des ersten Parameters gibt an, dass die `CCRenderTexture` sollte nur so groß wie der Hintergrund `CCSprite`. Seit der Karte Hintergrund `CCSprite` 200 Pixel hoch ist, wird die Karte belegt ungefähr die Hälfte der Höhe des Bildschirms.

Der zweite Parameter zu übergeben, um die `CCRenderTexture` Konstruktor gibt die Pixel Auflösung an den `CCRenderTexture`. Entsprechend der Anleitung unter dem [CocosSharp Auflösung Handbuch](~/graphics-games/cocossharp/resolutions.md), die Breite und Höhe des sichtbaren Bereichs im Spiel Einheiten ist häufig nicht identisch mit den Pixel-Auflösung des Bildschirms. Auf ähnliche Weise kann ein CCRenderTexture eine höhere Auflösung über deren Größe verwenden, sodass Visualisierungen klarere auf Geräten mit hoher Auflösung angezeigt werden.

Die Pixel-Auflösung ist zweimal die Größe der CCRenderTexture Text suchen pixelig verhindern:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Um zu vergleichen, können wir ändern Sie den Wert PixelResolution entsprechend der `background.ContentSize` (ohne verdoppelt wird) und vergleichen das Ergebnis: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Um zu vergleichen, kann die PixelResolution-Wert entsprechend den Hintergrund ändern. ContentSize ohne verdoppelt wird, und vergleichen das Ergebnis")


## <a name="rendering-to-a-ccrendertexture"></a>Rendern in einem CCRenderTexture

Visuelle Objekte in CocosSharp werden in der Regel nicht explizit gerendert. Stattdessen visueller Objekte hinzugefügt werden eine `CCLayer` ist Teil einer `CCScene`. CocosSharp automatisch rendert die `CCScene` und seine visuelle Hierarchie in jeden Frame ohne Renderingcode aufgerufen werden. 

Im Gegensatz dazu, die `CCRenderTexture` explizit zu gezeichnet werden muss. Dieses Rendering kann in drei Schritte unterteilt werden:

1. `CCRenderTexture.BeginWithClear` wird aufgerufen, gibt an, dass die gesamte nachfolgende Rendering aufrufenden abzielen `CCRenderTexture`.
1. Objekte, die von erben `CCNode` (z. B. der `Card` Entität) gerendert werden die `CCRenderTexture` durch Aufrufen von `Visit`.
1. `CCRenderTexture.End` wird aufgerufen, der angibt, Rendern die `CCRenderTexture` ist abgeschlossen.

Eine beliebige Anzahl von Objekten gerendert werden kann, um eine `CCRenderTexture` zwischen seine `Begin` und `End` aufrufen. Vor dem Rendern, werden alle erforderlichen sichtbaren Objekte als untergeordnete Elemente hinzugefügt:


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

Die `renderTexture` sollte nicht Teil der Karte beim Rendern des sein, damit er entfernt wird:


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Jetzt die `Card` Instanz kann selbst zum Rendern der `CCRenderTexture` Instanz:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

Sobald die Wiedergabe abgeschlossen ist, werden die einzelnen Komponenten entfernt und die `CCRenderTexture` erneut hinzugefügt wird.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

# <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt die `CCRenderTexture` Klasse, indem eine `Card` Entität, die sich in einem entladbare Kartenspiels verwendet werden kann. Es wurde gezeigt, wie mithilfe der `CCRenderTexture` Klasse zur Verbesserung der Framerate und Transparenz der gesamten Entität ordnungsgemäß zu implementieren.

Obwohl dieses Handbuch verwendet eine `CCRenderTexture` innerhalb einer Entität enthalten, diese Klasse kann zum Rendern von mehreren Entitäten verwendet, oder sogar kompletter `CCLayer` Instanzen für den Bildschirm serverweite Effekte und leistungsverbesserungen.

## <a name="related-links"></a>Verwandte Links

- [CCRenderTexture-API-Referenz](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Vollständige Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
