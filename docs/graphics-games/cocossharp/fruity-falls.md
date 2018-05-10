---
title: Spiel Fruity greift-details
description: Dieses Handbuch prüft das Spiel Fruity fällt, die allgemeine CocosSharp und 3D-Spielentwicklung-Konzepte, wie z. B. physikalische, inhaltsverwaltung Spiel Zustand und Entwurf abdecken.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 19b57d41fdf8aa4f8a749cf4979755161a0e7e51
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="fruity-falls-game-details"></a>Spiel Fruity greift-details

_Dieses Handbuch prüft das Spiel Fruity fällt, die allgemeine CocosSharp und 3D-Spielentwicklung-Konzepte, wie z. B. physikalische, inhaltsverwaltung Spiel Zustand und Entwurf abdecken._

Fruity greift ist ein einfaches, physikalische-basiertes Spiel, in dem der Spieler rote und gelbe Obst in farbigen Buckets, um Punkte sortiert. Das Ziel des Spiels ist, so viele Punkte wie möglich zu sammeln, ohne dass ein solches Obst löschen in der falschen "bin", das Spiel zu beenden.

![](fruity-falls-images/image1.png "Das Ziel des Spiels ist, wie viele Punkte wie möglich zu sammeln, ohne dass ein solches Obst löschen in die falschen "bin", beenden das Spiel")

Fruity greift erweitert, das in eingeführte Konzepte der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md) durch Hinzufügen der folgenden Elemente:

 - Inhalt in Form von PNG-Dateien
 - Erweiterte physikalische
 - Spiel Status (Übergang zwischen Szenen)
 - Möglichkeit zum Spielen Koeffizienten über Variablen in einer einzelnen Klasse enthaltenen zu optimieren.
 - Integrierte Unterstützung für visual-debugging
 - Code-Organisation, die mithilfe von Spiel Entitäten
 - Spiel Design auf lustige und Replay Wert ausgerichtet.

Während der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md) Einführung in CocosSharp Kernkonzepte konzentriert, Fruity greift wird gezeigt, wie diese zusammen in ein Spiel fertiges Produkt zu schalten. Da dieses Handbuchs die BouncingGame verweist, Leser sollten zuerst vertraut machen selbst mit den [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md) vor dem Lesen dieses Handbuchs.

Diese Anleitung enthält die Implementierung und den Entwurf der Fruity greift auf Einblicke um Ihres eigenen Spiels treffen zu können. Es werden die folgenden Themen behandelt:


- [GameController-Klasse](#gamecontroller-class)
- [Spiel Entitäten](#game-entities)
- [Obst Grafiken](#fruit-graphics)
- [Physikalische](#physics)
- [Spiel Inhalt](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>GameController-Klasse

Das Fruity fällt PCL-Projekt enthält eine `GameController` Klasse, die für das Spiel instanziieren und Verschieben zwischen Szenen zuständig ist. Diese Klasse wird von IOS- und Android-Projekte verwendet, um doppelten Code zu vermeiden.

Der Code innerhalb der `Initialize` Methode ist vergleichbar mit der Code in der`Initialize` Methode in einer Vorlage für unverändert CocosSharp, aber es enthält eine Reihe von Änderungen.

Wird standardmäßig die `GameView.ContentManager.SearchPaths` richten sich nach der Auflösung des Geräts. Wie im folgenden ausführlicher erläutert wird, verwendet Fruity liegt den gleichen Inhalt unabhängig von der geräteauflösung, sodass der `Initialize` Methode fügt die `Images` Pfad (keine Unterordner), die `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Neue CocosSharp Vorlagen umfassen, eine Klasse erben von `CCLayer`, sagen, dass diese Klasse Spiel Visualisierungen und Logik hinzugefügt werden sollen. Stattdessen Fruity greift verwendet mehrere `CCLayer` Instanzen steuern Zeichnen Reihenfolge. Diese `CCLayer` Instanzen enthalten die `GameView` -Klasse, die erbt `CCScene`. Fruity greift enthält auch mehrere Szenen, die erste wird die `TitleScene`. Aus diesem Grund die `Initialize` -Methode instanziiert einen `TitleScene` Instanz übergeben werden `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Der Inhalt für Fruity greift wurde mit niedriger Auflösung Pixel Art Layoutgründen Gründen erstellt. Die `GameView.DesignResolution` festgelegt ist, um das Spiel nur 384 Einheiten breit und 512 hoch:


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Schließlich die `GameController` -Klasse stellt eine statische Methode, die von jedem aufgerufen werden kann `CCGameScene` für den Übergang zu einer anderen `CCScene`. Diese Methode wird verwendet, um zwischen Verschieben der `TitleScene` und `GameScene`.


## <a name="game-entities"></a>Spiel Entitäten

Fruity fällt binärencoder wird das Muster für die Entität für die meisten Spiel Objekte. Eine ausführliche Erläuterung dieses Muster finden Sie in der [Entitäten in CocosSharp geführt](~/graphics-games/cocossharp/entities.md).

Die Entität implementieren Spiel Objekte befinden sich im Ordner "Entitäten" in der **FruityFalls.Common** Projekt:

![](fruity-falls-images/image2.png "Der Ordner Entitäten im Projekt FruityFalls.Common")

Entitäten sind Objekte, die von erben `CCNode`, und visuelle Elemente, Konflikte und jeden Frame Verhalten aufweisen.

Die `Fruit` -Objekt stellt eine typische CocosSharp Entität dar: sie enthält ein visuelles Objekt, Konflikte und jeden Frame Logik. Der Konstruktor ist verantwortlich für Früchte initialisieren:


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

Die `CreateFruitGraphic` Methode erstellt eine `CCSprite` -Instanz und fügt es der `Fruit`. Die `IsAntialiased` Eigenschaft auf "false" festgelegt, um dem Spiel ein pixelig aussehen zu verleihen. Dieser Wert wird festgelegt, auf "false" auf allen `CCSprite` und `CCLabel` Instanzen während des Spiels:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Wenn der Spieler beziehen sich auf eine `Fruit` -Instanz mit der `Paddle`, der Punktwert, Früchte wird um eins erhöht. Dies bietet eine zusätzliche Herausforderung für erfahrene Spieler Obst für die zusätzliche Punkte Grenzen gesetzt sind. Der Punktwert Frucht wird angezeigt, mit der `extraPointsLabel` Instanz.

Die `CreateExtraPointsLabel` Methode erstellt eine `CCLabel` -Instanz und fügt es der `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity greift enthält zwei verschiedene Arten von Früchten – Kirschen und Zitronen. Die `CreateFruitGraphic` weist ein visuelles Element standardmäßig, jedoch die Früchte visuellen Elemente werden über geändert können die `FruitColor` -Eigenschaft, die ihrerseits `UpdateGraphics`:


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

Die erste If-Anweisung im `UpdateGraphics` aktualisiert die Debug-Grafiken, die verwendet werden, um Kollisionen Bereiche visuell darzustellen. Diese Visualisierungen sind deaktiviert, bevor eine endgültige Version des Spiels erfolgt, allerdings auf werden, während der Entwicklung beibehalten kann für physikalische Debuggen. Der zweite Teil ändert sich die `graphic.Texture` Eigenschaft durch den Aufruf `CCTextureCache.SharedTextureCache.AddImage`. Diese Methode bietet Zugriff auf eine Textur Dateinamen an. Weitere Informationen zu dieser Methode finden Sie der [Textur Zwischenspeichern Handbuch](~/graphics-games/cocossharp/texture-cache.md).

Die `extraPointsLabel` Farbe so angepasst, dass Sie um mit dem Image Obst Kontrast zu halten und seine `PositionY` Wert angepasst wird Center die `CCLabel` auf die Früchte `CCSprite`:

![](fruity-falls-images/image3.png "Die Farbe ExtraPointsLabel so angepasst, dass um mit dem Image Obst Kontrast zu halten und Objektwerts PositionY so angepasst, dass die CCLabel auf die Früchte CCSprite zentrieren")

![](fruity-falls-images/image4.png "Die Farbe ExtraPointsLabel so angepasst, dass um mit dem Image Obst Kontrast zu halten und Objektwerts PositionY so angepasst, dass die CCLabel auf die Früchte CCSprite zentrieren")


### <a name="collision"></a>Kollisionsrate

Fruity greift implementiert eine benutzerdefinierte Kollision-Lösung, die mit Objekten im Ordner "Geometry":

![](fruity-falls-images/image5.png "Fruity greift implementiert eine benutzerdefinierte Kollision-Lösung, die mit Objekten im Ordner "Geometry"")

Kollisionen in Fruity greift wird implementiert, entweder die `Circle` oder `Polygon` Objekt, in der Regel mit einer dieser zwei Typen, die als untergeordnetes Element einer Entität hinzugefügt wird. Z. B. die `Fruit` Objekt verfügt über eine `Circle` aufgerufen `Collision` die Datenbankmodells in seiner `CreateCollision` Methode:


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

`Polygon` Erstellung ähnelt `Circle` erstellen, entsprechend der `Paddle` Klasse:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

Kollisionen Logik fällt [weiter unten in diesem Handbuch](#collision).


## <a name="physics"></a>Physikalische

Die physikalische in Fruity greift getrennt werden kann, in zwei Kategorien unterteilt: verschieben und Konflikte. 


### <a name="movement-using-velocity-and-acceleration"></a>Geschwindigkeit und Beschleunigung mit Verschiebung

Fruity greift verwendet `Velocity` und `Acceleration` Werte zur Steuerung der Übertragung der protokollsicherungsdaten seine Entitäten, ähnlich wie die [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Entitäten ihre Bewegung Logik implementieren, in einer Methode namens `Activity`, die einmal pro Frame aufgerufen wird. Beispielsweise sehen wir die Implementierung der Verschiebung in die `Fruit` Klasse `Activity` Methode:

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
Die `Fruit` Objekt ist in seiner Implementierung von Drag & eindeutig – ein Wert, der die Geschwindigkeit in Bezug auf Geschwindigkeit Obst verlangsamt bewegt wird. Diese Implementierung des Drag & eine *terminal Geschwindigkeit*, dies ist die maximale Geschwindigkeit an, die eine Instanz Obst liegen kann. Ziehen Sie verlangsamt auch die horizontale Verschiebung der Früchte, wodurch das Spiel etwas einfacher zu spielen.

Die `Paddle` Objekt gilt auch `Velocity` in seine `Activity` -Methode, aber seine `Velocity` wird berechnet eine `desiredLocation` Wert:


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

In der Regel spielen die verwenden `Velocity` legen Sie die Position des ihre Objekte stimmen nicht direkt steuern Entität Positionen, zumindest nicht nach der Initialisierung. Statt direkt festlegen der `Paddle` Position der `Paddle.HandleInput` Methode weist die `desiredLocation` Wert:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Kollisionsrate

Fruity greift implementiert teilweise realistische Konflikt zwischen der Früchte und andere collidable Objekte wie z. B. die `Paddle` und `GameScene.Splitter`. Um helfen, Konflikte zu debuggen, Fruity greift Kollision Bereiche können sichtbar gemacht werden durch Ändern der `GameCoefficients.ShowDebugInfo` in die `GameCoefficients.cs` Datei:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

Wenn dieser Wert auf `true` ermöglicht die Wiedergabe der Kollisionen Bereiche:  

![](fruity-falls-images/image6.png "Wenn dieser Wert auf "true" ermöglicht die Wiedergabe der Kollisionen Bereiche")

Kollisionen Logik beginnt der `GameScene.Activity` Methode:


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

`PerformCollision` behandelt alle Kollision `Fruit` Instanzen mit anderen Objekten: 


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

`FruitVsBorders` Kollisionen führt ihrer eigenen Logik für Konflikte anstatt Logik, die in einer anderen Klasse enthalten. Dieser Unterschied ist vorhanden, da des Konflikts zwischen Obst und Rahmen des Bildschirms nicht perfekt einfarbig wird – es ist möglich, dass Obst durch eine sorgfältige Paddel Bewegung über den Rand des Bildschirms verschoben werden. Obst wird sich außerhalb des Bildschirms mit dem Paddel bei Treffer zurückspringt, jedoch, wenn der Spieler langsam Obst schiebt verschoben hinter dem Rand und außerhalb des Bildschirms:


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

Die `FruitVsBins` Methode ist verantwortlich für das Überprüfen, ob alle Obst in einer von zwei Klassen gefallen ist. Wenn dies der Fall ist, werden der Spieler Punkte (falls die Früchte/Bin Übereinstimmung Farben) vergeben wird, oder das Spiel endet (wenn die Farben nicht übereinstimmen):


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

Obst im Vergleich zu Paddel und Obst im Vergleich zu Splitter (trennen zwei Klassen Bereich) Kollision sowohl basieren auf der `FruitPolygonCollision` Methode. Diese Methode besteht aus drei Teilen:

1. Testen Sie, ob die Objekte in Konflikt stehen
1. Verschieben Sie die Objekte (nur Obst im Fruity liegt), damit sie nicht mehr überlappen
1. Passen Sie die Geschwindigkeit der Objekte (nur Obst im Fruity fällt) um einen Bounce zu simulieren, der folgende Code wird, die Punkte veranschaulicht Oben vorgenommen:


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

Fruity greift Kollision Antwort einseitigen – wird nur die Früchte Geschwindigkeit und Position angepasst werden. Es ist zu beachten, dass andere Spiele Kollision haben können, die beide Objekte beteiligt sind, z. B. ein Zeichen, die per Push übertragen einer Boulder oder zwei Autos Abstürzen in miteinander wirkt sich auf.

Die mathematischen hinter der Kollisionen Logik enthalten, die der `Polygon` und `CollisionResponse` Klassen ist, würde den Rahmen dieses Handbuchs sprengen, aber als-geschrieben, diese Methoden können verwendet werden für viele Arten von Spielen. Das Polygon und `CollisionResponse` Klassen unterstützen auch viereckig noch konvexe Polygone, damit dieser Code für viele Arten von Spiele verwendet werden kann.

 


## <a name="game-content"></a>Spiel Inhalt

Die Grafiken in Fruity liegt unterscheidet sich das Spiel sofort von der BouncingGame. Während das Spiel Entwürfe ähnlich sind, sehen Player sofort entscheidend dafür, wie die beiden Spiele aussehen. Spieler entscheiden häufig, ob ein Spiel, das durch die Visualisierungen zu versuchen. Daher ist es äußerst wichtig, dass Entwickler darin, eine visuell ansprechende Ressourcen investieren Spiel.

Die Grafiken für Fruity greift erstellt wurde mit den folgenden Zielen:

 - Einheitliches Design
 - Schwerpunkt auf genau ein Zeichen – die Xamarin-Affe
 - Helle Farben angenehme erstellen eine lockern auftreten.
 - Im Gegensatz dazu zwischen die Hintergrund- und Vordergrundfarben, sodass Spielverlauf Objekte visuell leicht sind
 - Fähigkeit, einfache visuelle Effekte ohne Ressourcen beansprucht Animationen erstellen


### <a name="content-location"></a>Inhaltsort

Fruity greift umfasst alle des Inhalts im Ordner "Bilder" im Android-Projekt:

![](fruity-falls-images/image7.png "Fruity greift umfasst alle des Inhalts im Ordner "Bilder" im Android-Projekt")

Diese gleichen Dateien, die in der iOS-Projekt, um Verdoppelungen vermeiden, verknüpft sind, und so Änderungen zu den Dateien Auswirkungen auf beide Projekte:

![](fruity-falls-images/image8.png "Diese gleichen Dateien, die in der iOS-Projekt, um Verdoppelungen vermeiden, verknüpft sind, und so Änderungen zu den Dateien Auswirkungen auf beide Projekte")

Ist anzumerken, dass der Inhalt sich nicht innerhalb befindet der **%ld** oder **Hd** Ordner, in denen die Standardvorlage CocosSharp gehören. Die **%ld** und **Hd** Ordner für Spiele verwendet werden, die zwei Sätze von Inhalt – eine für niedrigerer Auflösung Geräte wie Smartphones und eine höhere Auflösung Geräte, z. B. Tablets bereitstellen sollen. Die Kunst Fruity greift ist absichtlich mit einem pixelig Layoutgründen, erstellt, sodass keine, zum Bereitstellen von Inhalt für verschiedene Bildschirmgrößen benötigt wird. Aus diesem Grund die **%ld** und **Hd** Ordner vollständig aus dem Projekt entfernt wurden.


### <a name="gamescene-layering"></a>GameScene Strukturlayout

Wie weiter oben in diesem Handbuch erwähnt die `GameScene` ist verantwortlich für alle Spiel Objektinstanziierung, positionieren und zwischen Objekt-Logik (z. B. Kollisionen). Alle Objekte werden hinzugefügt, um eine der vier `CCLayer` Instanzen:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

Diese vier Ebenen werden erstellt, der `CreateLayers` Methode. Beachten Sie, die sie hinzugefügt werden die `GameScene` in der Reihenfolge der Back-Front:


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

Nach Ebenen erstellt werden, werden alle visuellen Objekte, die mit Ebenen hinzugefügt der `AddChild` Methode, die entsprechend der `CreateBackground` Methode:


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>Daß-Entität

Die `Vine` Entität eindeutig für Inhalt verwendet wird – sie hat keine Auswirkungen auf Spielverlauf. Es besteht aus 20 `CCSprite` -Instanzen bestehen, eine Zahl durch Ausprobieren ausgewählt werden, um sicherzustellen, dass die daß immer den oberen Rand des Bildschirms erreicht:


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

Die erste `CCSprite` positioniert ist, sodass seinem unteren Rand die Position von Reben entspricht. Dies geschieht durch Festlegen der AnchorPoint auf `new CCPoint(.5f, 0)`. Die `AnchorPoint` Eigenschaft verwendet *normalisiert Koordinaten* den Bereich zwischen 0 und 1, unabhängig von der Größe der `CCSprite`:

![](fruity-falls-images/image9.png "Die AnchorPoint Eigenschaft verwendet normalisierten Koordinaten den Bereich zwischen 0 und 1, unabhängig von der Größe der CCSprite")

Nachfolgende daß Sprites positioniert sind, mithilfe der `graphic.ContentSize.Height` Wert, der die Höhe des Bilds in Pixel zurückgegeben. Das folgende Diagramm zeigt, wie die Grafik daß nach oben angeordnet:

![](fruity-falls-images/image10.png "Dieses Diagramm zeigt, wie die Grafik daß aufrundet Kacheln")

Seit des Ursprungs von der `Vine` Entität am unteren Rand der daß handelt, positionieren Sie es einfach, wie in der `Paddle.CreateVines` Methode:


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

Die daß-Instanzen lautet in der `Paddle` Entität auch interessante Drehung-Verhalten aufweisen. Da die `Paddle` Entität als Ganzes gemäß Player-Eingabe (die dieser Anleitung im folgenden ausführlicher behandelt), dreht die `Vine` Instanzen werden auch durch diese Drehung betroffene. Drehen Sie jedoch die `Vine` -Instanzen zusammen mit der `Paddle` visual unerwünschte Auswirkungen erzeugt, wie in der folgenden Animation gezeigt:

![](fruity-falls-images/image11.gif "Allerdings ergibt die Drehung daß Instanzen sowie das Paddel visual unerwünschte Auswirkungen wie in der folgenden Animation gezeigt")

Um entgegenzuwirken der `Paddle` Rotation der `leftVine` und `rightVine` Instanzen gedreht werden die entgegengesetzte Richtung der Paddel, entsprechend der `Paddle.Activity` Methode:


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

Beachten Sie, dass eine kleine Menge an Drehung an die daß über hinzugefügt wird die `vineAngle` Koeffizienten. Dieser Wert kann geändert werden, um anzupassen, wie viel die Vines drehen.


## <a name="gamecoefficients"></a>GameCoefficients

Jedem gutes Spiel ist das Produkt der Iteration, damit Fruity greift auf eine Klasse mit dem Namen enthält `GameCoefficients` , die steuert, wie das Spiel wiedergegeben wird. Diese Klasse enthält ausdrucksbasierte Variablen, die während des Spiels zum Steuerelement physikalische, Layout, erstellen und die Bewertung verwendet werden.

Die physikalische Obst werden z. B. durch die folgenden Variablen gesteuert:


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

Wie die Namen vermuten lassen, können diese Variablen verwendet werden, wie schnell Obst liegt, wie horizontale anpassen Bewegung Verlauf der Zeit und Paddel Bounciness verlangsamt.

Spiele in Code- oder Datenmenge Dateien (z. B. XML) gespeicherten Koeffizienten können besonders wertvoll für Teams mit Spiel Designern sein, die keine Programmierer sind.

Die `GameCoefficients` Klasse enthält auch Werte für das Einschalten der Debuginformationen, wie beispielsweise das Erstellen von Informationen oder Kollisionen Bereiche:


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


## <a name="conclusion"></a>Schlussbemerkung

Dieses Handbuch untersucht das Spiel Fruity liegt. Es behandelt Konzepte, einschließlich Inhalt, physikalische und Spiel Zustandsverwaltung.

## <a name="related-links"></a>Verwandte links

- [CocosSharp API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Abgeschlossene Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
