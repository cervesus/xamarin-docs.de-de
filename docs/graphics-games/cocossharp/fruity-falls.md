---
title: Fruity fällt game-details
description: Dieses Handbuch führt durch das Spiel Fruity liegt, die allgemeine CocosSharp und Entwicklung von Spielen-Konzepte wie z. B. Physik, inhaltsverwaltung, Spielzustands und Spiele entwerfen abdecken.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 959f5eb149ad375d686b17a85eb3d3b8fbdf3659
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114248"
---
# <a name="fruity-falls-game-details"></a>Fruity fällt game-details

_Dieses Handbuch führt durch das Spiel Fruity liegt, die allgemeine CocosSharp und Entwicklung von Spielen-Konzepte wie z. B. Physik, inhaltsverwaltung, Spielzustands und Spiele entwerfen abdecken._

Fruity fällt kann es sich um ein simple, physikalische basierende-Spiel, in dem der Spieler rote und gelbe Obst in farbigen Buckets, um die Punkte erhalten sortiert. Ziel des Spiels ist es, wie viele Punkte wie möglich zu erhalten, ohne dass ein Obst Ablegen in der falschen "bin" Ende des Spiels.

![](fruity-falls-images/image1.png "Ziel des Spiels ist es, wie viele Punkte wie möglich zu erhalten, ohne dass ein Obst Ablegen in der falschen \"bin\" Ende des Spiels")

Fruity fällt erweitert, das in eingeführte Konzepte der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md) durch Hinzufügen des folgenden:

 - Inhalte in Form von PNGs
 - Erweiterte Physik
 - Spielzustands (Übergang zwischen Szenen)
 - Möglichkeit zum Optimieren des spielen Koeffizienten über Variablen, die in einer einzelnen Klasse enthalten
 - Integrierte visuelle Unterstützung für Remotedebuggen
 - Spiele-Entitäten mit Code-Organisation
 - Spiele Entwurfs konzentriert sich auf Spaß und Wiedergabe-Wert

Während der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md) Einführung in die grundlegenden CocosSharp Konzepte konzentrieren, Fruity fällt zeigt, wie alles zusammen in ein fertiges Spiel Produkt übernehmen. Da dieses Handbuch BouncingGame verweist, Leser sollten zuerst vertraut machen, selbst mit der [BouncingGame-Handbuch](~/graphics-games/cocossharp/bouncing-game.md) vor dem Lesen dieses Handbuchs.

Dieser Leitfaden behandelt die Implementierung und Entwurf von Fruity fällt, Einblicke, um Ihr eigenes Spiel treffen zu können. Es enthält die folgenden Themen:


- [GameController-Klasse](#gamecontroller-class)
- [Spiele-Entitäten](#game-entities)
- [Obst Grafiken](#fruit-graphics)
- [Physik](#physics)
- [Spiel Inhalt](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>GameController-Klasse

Fruity fällt PCL-Projekt enthält eine `GameController` Klasse verantwortlich für das Instanziieren des Spiels, und Verschieben zwischen Szenen. Diese Klasse wird von IOS- und Android-Projekte verwendet, um doppelten Code zu vermeiden.

Der Code innerhalb der `Initialize` Methode ähnelt der Code in die`Initialize` -Methode in einer unveränderten CocosSharp-Vorlage, aber es enthält eine Reihe von Änderungen.

In der Standardeinstellung die `GameView.ContentManager.SearchPaths` zur Auflösung der des Geräts abhängig sind. Wie unten im Detail erläutert wird, verwendet Fruity fällt die gleichen Inhalte unabhängig von der Auflösung für dieses Gerät, sodass die `Initialize` Methode fügt die `Images` Pfad (mit keine Unterordner) zu der `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Neue CocosSharp-Vorlagen enthalten eine Klasse erbt von `CCLayer`, impliziert, dass das Spiel Visualisierungen und Logik für diese Klasse hinzugefügt werden soll. Fruity fällt verwendet stattdessen mehrere `CCLayer` Instanzen steuern Zeichnen Reihenfolge. Diese `CCLayer` Instanzen befinden sich innerhalb der `GameView` -Klasse, die erbt `CCScene`. Fruity fällt enthält auch mehrere Szenen, die der ersten, die `TitleScene`. Aus diesem Grund die `Initialize` Methode instanziiert ein `TitleScene` -Instanz, die an `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Der Inhalt für Fruity fällt wurde als mit niedriger Auflösung Pixel Kunst ästhetischen Gründen erstellt. Die `GameView.DesignResolution` wird festgelegt, damit das Spiel nur Breite von 384 Einheiten und 512 hoch ist:


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Zum Schluss die `GameController` Klasse stellt eine statische Methode und von jedem aufgerufen werden kann `CCGameScene` für den Übergang zu einer anderen `CCScene`. Diese Methode wird verwendet, um zwischen Verschieben der `TitleScene` und `GameScene`.


## <a name="game-entities"></a>Spiele-Entitäten

Fruity fällt verwendet das Muster für die Entität für die meisten die Spielobjekte. Eine ausführliche Erläuterung dieses Muster finden Sie der [Entitäten in CocosSharp Anleitung](~/graphics-games/cocossharp/entities.md).

Die Implementierung von Entity Spielobjekte finden Sie im Ordner "Entitäten" in der **FruityFalls.Common** Projekt:

![](fruity-falls-images/image2.png "Der Ordner \"Entitäten\" im Projekt FruityFalls.Common")

Entitäten sind Objekte, die von erben `CCNode`, und möglicherweise Visuals, Kollisionen und Verhalten der every-Frame.

Die `Fruit` -Objekt stellt eine typische CocosSharp-Entität dar: sie enthält ein visuelles Objekt, Kollisionen und jede-Frame-Logik. Ihren Konstruktor ist verantwortlich für die Früchte initialisieren:


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>Obst Grafiken

Die `CreateFruitGraphic` -Methode erstellt eine `CCSprite` -Instanz und fügt es der `Fruit`. Die `IsAntialiased` Eigenschaft auf "false" festgelegt ist, um dem Spiel eine verpixelt dargestellt aussehen zu verleihen. Dieser Wert wird festgelegt, auf "false" für alle `CCSprite` und `CCLabel` Instanzen in der gesamten das Spiel:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Jedes Mal, wenn der Spieler betrifft einen `Fruit` -Instanz mit der `Paddle`, der Punktwert, Früchte wird um eins erhöht. Dies bietet eine zusätzliche Herausforderung darstellen, auf erfahrene Player mit erreichbares Ziel hinsichtlich zusätzliche Punkte. Der Punktwert die Früchte wird angezeigt, mit der `extraPointsLabel` Instanz.

Die `CreateExtraPointsLabel` -Methode erstellt eine `CCLabel` -Instanz und fügt es der `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity fällt umfasst zwei verschiedene Arten von Früchten – Kirschen und Zitronen. Die `CreateFruitGraphic` weist eine standardmäßige Visualisierung, jedoch die Früchte-Visuals werden über geändert können die `FruitColor` -Eigenschaft, die wiederum ruft `UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

Die erste If-Anweisung in `UpdateGraphics` aktualisiert die Debuggen-Grafiken, die verwendet werden, um Kollisionen Bereiche zu visualisieren. Diese Visuals sind deaktiviert, bis eine endgültige Version des Spiels besteht, aber auf während der Entwicklung zum Debuggen von Physik aufbewahrt werden. Der zweite Teil ändert sich die `graphic.Texture` Eigenschaft durch den Aufruf `CCTextureCache.SharedTextureCache.AddImage`. Diese Methode bietet Zugriff auf eine Textur nach Dateiname. Weitere Informationen zu dieser Methode finden Sie der [Leitfaden Zwischenspeichern von Texturen](~/graphics-games/cocossharp/texture-cache.md).

Die `extraPointsLabel` Farbe wird angepasst, um den Kontrast mit dem Image Obst beibehalten und die zugehörige `PositionY` Wert angepasst Center die `CCLabel` auf die Früchte `CCSprite`:

![](fruity-falls-images/image3.png "Die ExtraPointsLabel Farbe wird angepasst, um den Kontrast mit dem Image Obst beibehalten, und der PositionY Wert wird angepasst, um die CCLabel auf die Früchte CCSprite center")

![](fruity-falls-images/image4.png "Die ExtraPointsLabel Farbe wird angepasst, um den Kontrast mit dem Image Obst beibehalten, und der PositionY Wert wird angepasst, um die CCLabel auf die Früchte CCSprite center")


### <a name="collision"></a>Kollisionsrate

Fruity fällt implementiert eine benutzerdefinierte Kollisionen-Lösung, die mit der Objekte im Ordner "Geometry":

![](fruity-falls-images/image5.png "Fruity fällt implementiert eine benutzerdefinierte Kollisionen-Lösung, die mit der Objekte im Ordner \"Geometry\"")

Kollisionen in Fruity fällt wird implementiert, mit der `Circle` oder `Polygon` Objekt in der Regel mit einer der beiden folgenden Typen als untergeordnetes Element einer Entität hinzugefügt wird. Z. B. die `Fruit` Objekt verfügt über eine `Circle` aufgerufen `Collision` die er im initialisiert seine `CreateCollision` Methode:


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

Beachten Sie, dass die `Circle` Klasse erbt von der `CCNode` Klasse, damit es als untergeordnetes Element des Spiels Entitäten hinzugefügt werden kann:


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` Erstellung ähnelt `Circle` erstellen, siehe die `Paddle` Klasse:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

Konflikt Logik abgedeckt ist [weiter unten in diesem Handbuch](#collision).


## <a name="physics"></a>Physik

Die Physik in Fruity fällt können getrennt werden, in zwei Kategorien unterteilt: verschieben und Konflikte. 


### <a name="movement-using-velocity-and-acceleration"></a>Verschieben von mit der Geschwindigkeit und Beschleunigung

Fruity fällt verwendet `Velocity` und `Acceleration` Werten zum Steuern, der Verschiebung der Entitäten, ähnlich wie die [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Entitäten implementiert die Logik für die datenverschiebung in einer Methode namens `Activity`, die einmal pro Frame aufgerufen wird. Beispielsweise sehen die Implementierung der Verschieben von Daten in die `Fruit` Klasse `Activity` Methode:

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
Die `Fruit` -Objekt ist in seiner Implementierung von Drag & eindeutig – ein Wert, der die Geschwindigkeit in Bezug auf die Geschwindigkeit der Frucht verlangsamt wird, bewegt wird. Stellt diese Implementierung von Drag & ein *terminal Geschwindigkeit*, dies ist die maximale Geschwindigkeit, die eine Instanz Obst liegen kann. Ziehen Sie verlangsamt auch die horizontale Bewegung Früchte, wodurch das Spiel etwas leichter zu spielen.

Die `Paddle` Objekt gilt auch für `Velocity` in seine `Activity` -Methode, jedoch `Velocity` wird mit berechnet eine `desiredLocation` Wert:


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

In der Regel verwenden Sie Spiele `Velocity` legen Sie die Position des führen Sie ihre Objekte nicht direkt steuern Entität Positionen, zumindest nicht nach der Initialisierung. Statt direkt die `Paddle` Position der `Paddle.HandleInput` Methode weist die `desiredLocation` Wert:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Kollisionsrate

Fruity fällt implementiert teilweise realistische Konflikt zwischen der Obst und andere collidable Objekte wie z. B. die `Paddle` und `GameScene.Splitter`. Um Konflikte zu debuggen, Fruity fällt Kollisionen Bereiche können sichtbar gemacht werden durch Ändern der `GameCoefficients.ShowDebugInfo` in die `GameCoefficients.cs` Datei:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

Wenn dieser Wert auf `true` ermöglicht die Wiedergabe der Kollisionen Bereiche:  

![](fruity-falls-images/image6.png "Wenn dieser Wert auf \"true\" ermöglicht die Wiedergabe der Konflikt-Bereiche")

Konflikt-Logik in beginnt die `GameScene.Activity` Methode:


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` behandelt alle Konflikt zu geraten `Fruit` Instanzen mit anderen Objekten: 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` Kollisionen führt eine eigene Logik für Kollisionen statt Logik, die in einer anderen Klasse enthalten. Dieser Unterschied ist vorhanden, da die Kollision zwischen Obst und des Bildschirms Rahmen nicht perfekt solid ist: Es ist möglich, dass Obst außerhalb des Bildschirms durch eine sorgfältige Paddel Bewegung mithilfe von Push übertragen werden. Obst wird animiert, außerhalb des Bildschirms, wenn mit das Paddel erreicht, aber wenn der Spieler langsam Obst überträgt wird verschieben hinter dem Rand und vom Bildschirm verschwindet:


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

Die `FruitVsBins` Methode ist verantwortlich ist, wird überprüft, ob alle Obst in einer von zwei Klassen gefallen ist. Wenn dies der Fall ist, klicken Sie dann der Player Punkten vergeben wird, (wenn die Früchte/Bin Übereinstimmung Farben) oder das Spiel ist beendet (sofern die Farben nicht übereinstimmen):


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle und FruitPolygonCollision

Obst im Vergleich zu Paddel und einem Obst im Vergleich zu Splitter (der Bereich, trennen die beiden Klassen) Konflikte, die beide beruhen auf der `FruitPolygonCollision` Methode. Diese Methode besteht aus drei Teilen:

1. Überprüfen Sie, ob die Objekte in Konflikt stehen
1. Verschieben Sie die Objekte (nur Obst im Fruity liegt), damit sie nicht mehr sich überlappen
1. Passen Sie die Geschwindigkeit der Objekte (nur Obst im Fruity liegt), um einen Schub zu simulieren, der folgende Code wird, die Punkte veranschaulicht Oben vorgenommen:


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Fruity fällt Kollisionen Antwort einseitigen – wird nur die Früchte der Geschwindigkeit und Position angepasst werden. Es ist erwähnenswert, dass andere Spiele Kollisionen haben können, die beide Objekte wie z. B. ein Zeichen, die mithilfe von Push übertragen einer Boulder oder zwei Autos abstürzt, miteinander verbundenen Auswirkungen auf.

Der Konflikt-Logik, die in enthaltenen, Rangordnungs-die `Polygon` und `CollisionResponse` Klassen von den Rahmen dieses Handbuchs sprengen, aber als-geschrieben, diese Methoden können verwendet werden für eine Vielzahl von Spielen. Das Polygon und `CollisionResponse` Klassen unterstützen auch viereckig noch konvexe Polygone, damit dieser Code für eine Vielzahl von Spielen verwendet werden kann.

 


## <a name="game-content"></a>Spiel Inhalt

Die Technik in Fruity fällt unterscheidet sofort das Spiel von BouncingGame. Während die Spiele-Entwürfe ähnlich sind, sehen Spieler sofort einen Unterschied in der zwei Spiele wie aussehen. Spieler entscheiden oft, ob ein Spiel durch seine visuellen Elemente zu testen. Daher ist es unabdingbar, dass die Entwickler Ressourcen bei der Erstellung einer visuell ansprechend investieren spielen.

Die Kunst für Fruity liegt, die mit den folgenden Zielen erstellt wurde:

 - Einheitliches Design
 - Den Schwerpunkt nur ein einzelnes Zeichen – das Xamarin-Monkey-Objekt
 - Erleben helle Farben einer lockern angenehme erstellen
 - Vergleichen Sie zwischen die Hintergrund- und Vordergrundfarben, so dass Gaming-Objekte leicht visuell nachverfolgt werden
 - Möglichkeit, einfache visuelle Effekte ohne Ressourcen beansprucht Animationen zu erstellen.


### <a name="content-location"></a>Inhaltsort

Fruity fällt enthält alle des Inhalts im Ordner "Bilder" im Android-Projekt:

![](fruity-falls-images/image7.png "Fruity fällt enthält alle seine Inhalte im Ordner \"Bilder\" im Android-Projekt")

Diese gleichen Dateien in der iOS-Projekt, damit eine Duplizierung verhindert verknüpft sind, und so Änderungen an den Dateien wirken sich auf beide Projekte:

![](fruity-falls-images/image8.png "Diese gleichen Dateien werden in der iOS-Projekt, damit eine Duplizierung verhindert verknüpft, und so Änderungen an den Dateien wirken sich auf beide Projekte")

Es ist erwähnenswert, dass der Inhalt nicht, in enthalten ist der **%ld** oder **Hd** Ordnern, die Teil der standardmäßigen CocosSharp-Vorlage sind. Die **%ld** und **Hd** Ordner für Spiele verwendet werden, die zwei Sätze von Inhalten – eine für niedrigerer Auflösung Geräte wie Smartphones und für Geräte mit höherer Auflösung, z. B. Tablets bereitstellen sollen. Die Kunst Fruity fällt ist absichtlich mit einem pixelig ästhetischen, erstellt, sodass keine, zum Bereitstellen von Inhalt für verschiedene Bildschirmgrößen benötigt wird. Aus diesem Grund die **%ld** und **Hd** Ordner vollständig aus dem Projekt entfernt wurden.


### <a name="gamescene-layering"></a>GameScene Schichten

Wie weiter oben in diesem Handbuch erwähnt die `GameScene` ist verantwortlich für alle spielobjekt Instanziierung, positionieren und zwischen Objekt-Logik (z. B. Collision). Alle Objekte werden hinzugefügt, um eine der vier `CCLayer` Instanzen:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

Diese vier Ebenen werden erstellt, der `CreateLayers` Methode. Beachten Sie, die sie hinzugefügt wurden die `GameScene` in der Reihenfolge der Vordergrund zurück:


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

Nachdem die Ebenen erstellt wurden, werden alle visuellen Objekte, die den Ebenen mit hinzugefügt der `AddChild` Methode, wie gezeigt in der `CreateBackground` Methode:


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>Sich-Entität

Die `Vine` Entität eindeutig für Inhalt verwendet wird – es hat keine Auswirkungen auf Gaming. Sie besteht aus 20er `CCSprite` -Instanzen und eine Zahl, die durch Versuch und Irrtum ausgewählt werden, um sicherzustellen, dass die sich immer erreicht den oberen Rand des Bildschirms:


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

Die erste `CCSprite` positioniert ist, sodass der unteren Rand der sich die Position entspricht. Dies wird erreicht, indem Sie auf die AnchorPoint `new CCPoint(.5f, 0)`. Die `AnchorPoint` Eigenschaft verwendet *normalisiert Koordinaten* der Bereich zwischen 0 und 1, unabhängig von der Größe der `CCSprite`:

![](fruity-falls-images/image9.png "AnchorPoint-Eigenschaft verwendet die normalisierte Koordinaten der Bereich zwischen 0 und 1, unabhängig von der Größe der CCSprite")

Nachfolgende sich Sprites positioniert sind, mit der `graphic.ContentSize.Height` Wert, der die Höhe des Bilds in Pixeln zurückgegeben. Das folgende Diagramm zeigt, wie die Grafik sich nach oben angeordnet:

![](fruity-falls-images/image10.png "Dieses Diagramm zeigt, wie sich dies auf die Kacheln in der Grafik sich nach oben")

Da der Ursprung der der `Vine` Entität wird am unteren Rand der sich aus, und positionieren es ist einfach, wie in der `Paddle.CreateVines` Methode:


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

Die sich Instanzen der `Paddle` Entität verfügen auch interessante Drehverhalten. Da die `Paddle` Entität als Ganzes nach Player-Eingabe (die diesem Leitfaden wird im folgenden ausführlicher beschrieben), dreht die `Vine` Instanzen werden durch diese Drehung auch beeinflusst. Allerdings Drehen der `Vine` zusammen mit Instanzen der `Paddle` erzeugt visual unerwünschte Auswirkungen, wie in der folgenden Animation dargestellt:

![](fruity-falls-images/image11.gif "Allerdings ergibt die Drehung sich Instanzen sowie das Paddel visual unerwünschte Auswirkungen Siehe die folgende animation")

Um entgegenzuwirken der `Paddle` Drehung der `leftVine` und `rightVine` Instanzen gedreht werden die entgegengesetzte Richtung von das Paddel, siehe die `Paddle.Activity` Methode:


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

Beachten Sie, dass eine kleine Menge der Drehung, an der sich über hinzugefügt wird die `vineAngle` Koeffizienten. Dieser Wert kann geändert werden, um anzupassen, wie viel die Vines drehen.


## <a name="gamecoefficients"></a>GameCoefficients

Jede gutes Spiel ist das Produkt der Iteration aus, sodass Fruity fällt eine Klasse namens enthält `GameCoefficients` die steuert, wie das Spiel gespielt wird. Diese Klasse enthält ausdrucksstarke Variablen, die während des Spiels Steuerelement Physik, Layout, erzeugen und Bewertung verwendet werden.

Die Physik Frucht werden z. B. durch die folgenden Variablen gesteuert:


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

Wie die Namen bereits andeuten, können diese Variablen verwendet werden, anpassen, wie schnell Obst fällt, horizontale Verschiebung im Laufe der Zeit Paddel Bounciness verlangsamt.

Spiele Koeffizienten, die in Code oder Dateien (z. B. XML) gespeichert können besonders nützlich für Teams mit Spielen Designer sein, die keine Programmierer sind.

Die `GameCoefficients` -Klasse enthält auch Werte, die zum Aktivieren der Debug-Informationen, z. B. das Erzeugen von Informationen oder Konflikte Bereiche:


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>Schlussfolgerung

Dieses Handbuch behandelt das Spiel Fruity fällt. Sie behandelt Konzepte wie z.B. Inhalt Physik und Spiele Zustandsverwaltung.

## <a name="related-links"></a>Verwandte Links

- [CocosSharp-API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Abgeschlossenes Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
