---
title: Spiel münzwurfs Zeit-details
description: Dieses Handbuch erläutert Implementierungsdetails Münzwurfs Zeit-Spiele, einschließlich arbeiten mit Kachel Zuordnungen, Erstellen von Entitäten, animieren Sprites und effiziente Kollision implementieren.
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 07a43dbf5f3095d1735d57fdbb13499532bfe415
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="coin-time-game-details"></a>Spiel münzwurfs Zeit-details

_Dieses Handbuch erläutert Implementierungsdetails Münzwurfs Zeit-Spiele, einschließlich arbeiten mit Kachel Zuordnungen, Erstellen von Entitäten, animieren Sprites und effiziente Kollision implementieren._

Münzwurfs ist eine vollständige Platformer Spiele für iOS und Android. Das Ziel des Spiels ist, erfassen alle die Münzen in einer Ebene, und klicken Sie dann die Tür beenden zu erreichen, und vermeidet Feinde und Hindernisse verursacht.

![](cointime-images/image1.png "Das Ziel des Spiels wird Sammeln aller der Münzen in einer Ebene, und klicken Sie dann die Tür beenden zu erreichen, und vermeidet Feinde und Hindernisse verursacht")

Dieses Handbuch werden die Implementierungsdetails in Münzwurfs Zeit, die folgenden Themen erläutert:

- [Arbeiten mit Tmx-Dateien](#working-with-tmx-files)
- [Beim Laden der Ebene](#level-loading)
- [Hinzufügen von neuen Entitäten](#adding-new-entities)
- [Animierte Entitäten](#animated-entities)


## <a name="content-in-coin-time"></a>Inhalte in Münzwurfs Zeit

Münzwurfs Zeit ist ein Beispielprojekt, das darstellt, wie ein Gesamtprojekt CocosSharp organisiert werden kann. Münzwurfs Uhrzeit Struktur zielt auf das Hinzufügen und die Wartung von Inhalten zu vereinfachen. Er verwendet **.tmx** erstellten Dateien [Fläche](http://www.mapeditor.org) für Ebenen und XML-Dateien Animationen definieren. Ändern oder Hinzufügen von neuen Inhalt kann mit minimalem Aufwand erreicht werden. 

Trotz aller bemühungen dieser Ansatz Münzwurfs Mal ein effektive Projekt zum Lernen und experimentieren, damit auch wie professionelle Spiele wiedergegeben werden. Dieses Handbuch erläutert einige der Methoden zum Hinzufügen und Ändern von Inhalt zu vereinfachen.


## <a name="working-with-tmx-files"></a>Arbeiten mit Tmx-Dateien

Münzwurfs Zeitebenen werden definiert über die .tmx-Dateiformat, das durch ausgegeben wird die [Fläche](http://www.mapeditor.org) Kachel Zuordnungs-Editor. Eine ausführliche Erläuterung der Arbeit mit Fläche, finden Sie unter der [mit Cocos Spitze Handbuch gekachelt mithilfe](~/graphics-games/cocossharp/tiled.md). 

Jede Ebene wird in einem eigenen .tmx-Datei definiert die **CoinTime/Bestand/Inhalt/Ebenen** Ordner. Alle Münzwurfs Zeitebenen freigeben eine Tileset-Datei, definiert in der **mastersheet.tsx** Datei. Diese Datei definiert die benutzerdefinierten Eigenschaften für jede Kachel, z. B., ob die Kachel einfarbig Kollision verfügt, oder gibt an, ob die Kachel durch eine Entitätsinstanz ersetzt werden soll. Die Datei mastersheet.tsx kann Eigenschaften nur einmal definiert und auf allen Ebenen verwendet werden. 


### <a name="editing-a-tile-map"></a>Bearbeiten einer Kachel-Zuordnung

Um eine Kachel-Zuordnung bearbeiten möchten, öffnen Sie die .tmx-Datei in Fläche durch Doppelklicken auf die Datei .tmx oder öffnen es über das Dateimenü in Fläche. Münzwurfs Zeit Ebene Kachel Zuordnungen mit drei Ebenen enthalten: 

- **Entitäten** – diese Ebene enthält Kacheln, die mit Instanzen von Entitäten, die zur Laufzeit ersetzt werden. Zählen der Player, Münzen Feinde und die Tür der Ende-des-Ebene.
- **Terrain** – diese Ebene enthält Kacheln, die die durchgehende Konflikte in der Regel haben. Einfarbig Kollision ermöglicht den Player auf diesen Kacheln ohne durchfallen zu durchlaufen. Kacheln mit Volltonfarbe Konflikte können auch als Wände und decken fungieren.
- **Hintergrund** – die Hintergrundebene enthält Kacheln, die als statische Hintergrund verwendet werden. Diese Ebene Bildlauf nicht wenn die Kamera in der Ebene befinden, erstellen die Darstellung der Tiefe über Parallax bewegt wird.

Wie wir später untersuchen, erwartet der Ebene Automatisches Laden von Code diese drei Ebenen in allen Münzwurfs Zeit Ebenen.

#### <a name="editing-terrain"></a>Bearbeiten von terrain

Kacheln platziert werden können, indem Sie auf der **Mastersheet** Tileset klicken und dann auf die Kachel zuordnen. Geben Sie beispielsweise Folgendes ein, um neue Terrain in einer Ebene Paint-Ereignis:

1. Wählen Sie die Terrain-Ebene
1. Klicken Sie auf die Kachel gezeichnet werden soll
1. Klicken Sie auf oder mithilfe von Push übertragen Sie, und ziehen Sie auf der Karte, um die Kachel zeichnen

    ![](cointime-images/image2.png "Klicken Sie auf die Kachel 1 gezeichnet werden soll.")

Oben links von der Tileset enthält alle im Gelände zeitlich Münzwurfs. Terrain, d. h. solid, enthält die **SolidCollision** Eigenschaft, die in den kacheleigenschaften auf der linken Seite des Bildschirms angezeigt:

![](cointime-images/image3.png "Terrain, d. h. solid, beinhaltet die SolidCollision-Eigenschaft an, wie in der kacheleigenschaften auf der linken Seite des Bildschirms dargestellt")

#### <a name="editing-entities"></a>Bearbeiten von Entitäten

Entitäten können hinzugefügt oder aus einer Ebene – genau wie Terrain entfernt werden. Die **Mastersheet** Tileset verfügt über alle Entitäten platziert zu während des laufenden Vorgangs horizontal aus, damit sie nicht angezeigt werden können, ohne Bildlauf nach rechts:

![](cointime-images/image4.png "Die Mastersheet Tileset verfügt über alle Entitäten platziert zu während des laufenden Vorgangs horizontal aus, damit sie nicht angezeigt werden können, ohne Bildlauf nach rechts")

Neue Entitäten platziert werden soll, auf die **Entitäten** Ebene.

![](cointime-images/image5.png "Neue Entitäten sollten in der Ebene von Entitäten gespeichert werden")

CoinTime Code sucht den **EntityType** beim Laden einer Ebene um Kacheln zu identifizieren, die durch Entitäten ersetzt werden soll: 

![](cointime-images/image6.png "CoinTime Code sucht für den EntityType, wenn eine Ebene geladen wird, um Kacheln zu identifizieren, die durch Entitäten ersetzt werden soll")

Nachdem die Datei geändert und gespeichert wurde, werden die Änderungen automatisch angezeigt, wenn das Projekt erstellt und ausgeführt wird:

![](cointime-images/image7.png "Nachdem die Datei geändert und gespeichert wurde, werden die Änderungen automatisch angezeigt, wenn das Projekt erstellt und ausgeführt wird")


### <a name="adding-new-levels"></a>Hinzufügen von neuen Ebenen

Der Vorgang des Hinzufügens von Ebenen in Münzwurfs Zeit erfordert keine Änderungen am Code und nur einige kleinere Änderungen am Projekt. So fügen Sie eine neue Ebene hinzu:

1. Der Ordner öffnen finden Sie unter <**CoinTime Root > \CoinTime\Assets\Content\levels**
1. Kopieren Sie eine der Ebenen, wie z. B. **level0.tmx**
1. Benennen Sie die neue .tmx-Datei aus, damit die Sequenz die Ebene Nummer mit vorhandenen Ebenen, wie z. B. weiterhin **level8.tmx**
1. In Visual Studio oder Visual Studio für Mac wird fügen Sie die neue .tmx-Datei in den Ordner Android Ebenen hinzu. Stellen Sie sicher, dass die Datei verwendet das **AndroidAsset** Buildvorgang.

    ![](cointime-images/image8.png "Stellen Sie sicher, dass die Datei über den Buildvorgang AndroidAsset verwendet")

1. Die neue .tmx-Datei auf den Ordner "iOS Ebenen" hinzufügen. Achten Sie darauf, verknüpfen Sie die Datei vom ursprünglichen Speicherort, und stellen Sie sicher, dass er verwendet die **BundleResource** Buildvorgang.

    ![](cointime-images/image9.png "Achten Sie darauf, verknüpfen Sie die Datei vom ursprünglichen Speicherort, und stellen Sie sicher, dass er die Buildaktion BundleResource verwendet")

Die neue Ebene sollte angezeigt werden in den Bildschirm für das Auswählen von Ebene als Ebene 9 (Ebene Dateinamen beginnen bei 0, aber die Ebenen Schaltflächen beginnen mit der Zahl 1):

![](cointime-images/image10.png "Die neue Ebene sollte auf Ebene wählen Ebene 9-Datei beginnen bei 0 auftreten, während die Ebenen Schaltflächen mit der Zahl 1 beginnen")


## <a name="level-loading"></a>Beim Laden der Ebene

Wie oben beschrieben, neue Ebenen erfordern keine Änderungen im Code – das Spiel erkennt automatisch die Ebenen, wenn sie mit dem Namen ordnungsgemäß und hinzugefügt werden die **Ebenen** Ordner mit den richtigen Buildvorgang (**BundleResource**oder **AndroidAsset**).

Die Logik zum Bestimmen der Anzahl von Ebenen befindet sich der `LevelManager` Klasse. Wenn eine Instanz von der `LevelManager` erstellt wird (von Singleton-Muster), die `DetermineAvailbleLevels` -Methode aufgerufen wird:


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

CocosSharp bietet keinen plattformübergreifende Ansatz zum erkennen, wenn Dateien vorhanden sind, damit wir haben auf der `TitleContainer` Klasse versucht, die zum Öffnen eines Datenstroms. Wenn der Code für das Öffnen eines Datenstroms, eine Ausnahme, und klicken Sie dann auf die Datei auslöst nicht vorhanden und die While-Schleife unterbrochen. Sobald die Schleife beendet wurde, die `NumberOfLevels` Eigenschaft meldet, wie viele gültige Schweregrade Teil des Projekts sind.

Die `LevelSelectScene` -Klasse verwendet die `LevelManager.NumberOfLevels` um zu bestimmen, wie viele Schaltflächen zum Erstellen in der `CreateLevelButtons` Methode:


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

Die `NumberOflevels` Eigenschaft wird verwendet, um zu bestimmen, welche Schaltflächen erstellt werden soll. Dieser Code berücksichtigt, welche Seite der Benutzer aktuell angezeigt wird und nur maximal sechs Schaltflächen pro Seite erstellt. Beim Klicken auf die Schaltfläche mit der Instanzen, Aufruf der `HandleButtonClicked` Methode:


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Diese Methode weist der `CurrentLevel` Eigenschaft von dient der `GameScene` beim Laden einer Ebene. Nach dem Festlegen der `CurrentLevel`, `GoToGameScene` Methode ausgelöst wird, Wechseln der Szene aus `LevelSelectScene` auf `GameScene`.

Die `GameScene` Konstruktoraufrufe `GoToLevel`, das die Ebene Automatisches Laden von Logik ausführt:


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

Wir führen anschließend eine Betrachtung in aufgerufenen Methoden `GoToLevel`.


### <a name="loadlevel"></a>LoadLevel

Die `LoadLevel` Methode ist verantwortlich für das Laden der Datei .tmx und Hinzufügen zu den `GameScene`. Diese Methode erstellt keine interaktive Objekte wie Konflikte oder Entitäten – einfach wird erstellt, die visuellen Elemente für die Ebene, die auch als bezeichnet den *Umgebung*.


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

Die `CCTileMap` Konstruktor akzeptiert einen Dateinamen, die erstellt wird, verwenden die Nummer die Ebene der an übergebene `LoadLevel`. Weitere Informationen zum Erstellen und Arbeiten mit `CCTileMap` Instanzen finden Sie unter der [mit CocosSharp Handbuch gekachelt mithilfe](~/graphics-games/cocossharp/tiled.md).

Derzeit CocosSharp lässt keine Ebenen neu anordnen, ohne zu entfernen und erneut hinzugefügt wurden, mit deren übergeordneten `CCScene` (also die `GameScene` in diesem Fall), sodass die letzten zwei Zeilen der Methode erforderlich sind, um die Ebenen neu anzuordnen.


### <a name="createcollision"></a>CreateCollision

Die `CreateCollision` -Methode erstellt eine `LevelCollision` -Instanz, die zum Ausführen *einfarbig Kollision* zwischen dem Player und Umgebung.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Ohne diese Konflikte würde der Spieler über der Ebene fallen, und das Spiel würde nicht wiedergegeben werden. Einfarbig Kollisionen können der Spieler auf dem Boden zu durchlaufen und verhindert, dass der Spieler aus Wände durchlaufen oder sich über Obergrenzen springen.

Kollisionen Münzwurfs zeitlich kann ohne zusätzlichen Code – nur Änderungen an gekachelten Dateien hinzugefügt werden. 


### <a name="processtileproperties"></a>ProcessTileProperties

Sobald eine Ebene geladen wird und der Konflikt erstellt wird, `ProcessTileProperties` wird aufgerufen, um die Logik, die basierend auf der kacheleigenschaften ausführen. Münzwurfs Zeitspanne beinhaltet eine `PropertyLocation` Struktur, Eigenschaften und die Koordinaten der Kachel mit diesen Eigenschaften zu definieren:


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

Diese Struktur wird zur Erstellung verwendet Entitätsinstanzen erstellen und entfernen Sie unnötige Kacheln in der `ProcessTileProperties` Methode:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

Die foreach-Schleife wertet jede Kachel-Eigenschaft, und überprüfen, ob der Schlüssel entweder ist `EntityType` oder `RemoveMe`. `EntityType` Gibt an, dass eine Entitätsinstanz erstellt werden soll. `RemoveMe` Gibt an, dass die Kachel zur Laufzeit vollständig entfernt werden soll.

Wenn eine Eigenschaft mit dem `EntityType` Schlüssel gefunden wird, klicken Sie dann `TryCreateEntity` aufgerufen wird, wird versuchen, eine Entität mit der Eigenschaft übereinstimmende erstellen die `EntityType` Schlüssel:


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


## <a name="adding-new-entities"></a>Hinzufügen von neuen Entitäten

Münzwurfs Zeit mithilfe des Entity-Musters für seine Spiel Objekte (die fällt die [Entitäten in CocosSharp geführt](~/graphics-games/cocossharp/entities.md)). Alle Entitäten, die von erben `CCNode`, d. h., sie als untergeordnete Elemente hinzugefügt werden können die `gameplayLayer`.

Jeder Entitätstyp ist auch direkt über eine Liste oder eine einzelne Instanz auf die verwiesen wird. Z. B. die `Player` verweist auf die `player` Feld und alle `Coin` Instanzen werden auf die verwiesen wird eine `coins` Liste. Halten direkte Verweise auf Entitäten (im Gegensatz zum Verweisen auf diese über die `gameLayer.Children` Liste) macht Code greift auf diese Entitäten, die einfacher zu lesen und möglicherweise kostspielige Typumwandlung entfällt.

Der vorhandene Code bietet es sich um eine Reihe von Entitätstypen als Beispiele zum Erstellen neuer Entitäten. Die folgenden Schritte können verwendet werden, um eine neue Entität zu erstellen:


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - definieren Sie eine neue Klasse, die unter Verwendung des Musters Entität

Die einzige Voraussetzung zum Erstellen einer Entität ist eine Klasse erstellen, die erbt `CCNode`. Die meisten Entitäten haben einige visuelle Element, z. B. eine `CCSprite`, die als untergeordnetes Element der Entität in seinem Konstruktor hinzugefügt werden sollen.

CoinTime bietet die `AnimatedSpriteEntity` Klasse, die die Erstellung der animierten Entitäten vereinfacht. Animationen in ausführlicher behandelt werden die [animiert Entitäten Abschnitt](#animated-entities).


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – Fügen Sie einen neuen Eintrag für die TryCreateEntity Switch-Anweisung

Instanzen der neuen Entität instanziiert werden sollte, der `TryCreateEntity`. Wenn die Entität für jeden-Frame-Logik, wie Konflikte, AI oder Lesen von Eingaben, müssen die `GameScene` muss einen Verweis auf das Objekt zu halten. Wenn mehrere Instanzen erforderlich sind (z. B. `Coin` oder `Enemy` Instanzen), klicken Sie dann ein neues `List` hinzugefügt werden sollen die `GameScene` Klasse.


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3 – Ändern der kacheleigenschaften für die neue Entität

Nachdem der Code die Erstellung der neuen Entität unterstützt, muss die neue Entität die Tileset hinzugefügt werden. Die Tileset kann bearbeitet werden, durch Öffnen einer beliebigen Ebene `.tmx` Datei. 

Befindet sich die Tileset getrennt von den .tmx in der **mastersheet.tsx** Datei, damit es importiert werden muss, bevor sie bearbeitet werden kann:

![](cointime-images/image11.png "Die Tileset wird getrennt von der Datei .tsx gespeichert, damit es importiert werden muss, bevor sie bearbeitet werden können")

Sobald importiert, auf die ausgewählten Kacheln Eigenschaften bearbeitet werden können, und der EntityType hinzugefügt werden kann:

![](cointime-images/image12.png "Sobald importiert, auf die ausgewählten Kacheln Eigenschaften bearbeitet werden können und EntityType hinzugefügt werden können")

Nachdem die Eigenschaft erstellt wurde, kann seine Wert festgelegt werden, entsprechend der neuen `case` in `TryCreateEntity`:

![](cointime-images/image13.png "Nachdem die Eigenschaft erstellt wurde, kann der Wert festgelegt werden entsprechend der neuen Fall in TryCreateEntity")

Nachdem die Tileset geändert wurde, muss exportiert werden – dies stellt die Änderungen für alle anderen Ebenen zur Verfügung:

![](cointime-images/image14.png "Nachdem die Tileset geändert wurde, muss exportiert werden")

Die Tileset sollten überschreibt das vorhandene **mastersheet.tsx** Tileset:

![](cointime-images/image15.png "HE Tileset sollte die vorhandene mastersheet.tsx Tileset überschreiben.")


## <a name="entity-tile-removal"></a>Entität Kachel entfernen

Beim Laden einer Kachel-Zuordnung in ein Spiel, werden die einzelnen Kacheln statische Objekte. Da Entitäten benutzerdefiniertes Verhalten, z. B. Bewegung benötigen, entfernt Münzwurfs Zeitcode Kacheln aus, wenn Entitäten erstellt werden.

`ProcessTileProperties` enthält die Logik, um Kacheln zu entfernen, die Erstellen von Entitäten, die mithilfe der `RemoveTile` Methode:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

Diese automatische Löschung von Kacheln reicht für Entitäten an, die nur eine Kachel in Tileset, z. B. Münzen und Feinde belegt. Größere Entitäten erfordern zusätzliche Logik und die Eigenschaften an.

Die Tür erfordert zwei Kacheln, vollständig gezeichnet werden soll:

![](cointime-images/image16.png "Die Tür erfordert zwei Kacheln, vollständig gezeichnet werden soll")

Die unteren-Kachel in der Tür enthält die Eigenschaften zum Erstellen einer Entität (**EntityType** festgelegt **Tür**):

![](cointime-images/image17.png "Die unteren-Kachel in der Tür enthält die Eigenschaften zum Erstellen einer Entität, die Tür EntityType fest")

Da nur die unteren Kachel die Tür entfernt wird, wenn die Tür-Instanz erstellt wird, ist zusätzliche Logik erforderlich, um die obersten Kachel zur Laufzeit zu entfernen. Der oberste Kachel weist eine **RemoveMe** -Eigenschaftensatz auf **"true"**:

![](cointime-images/image18.png "Der oberste Kachel weist eine RemoveMe-Eigenschaft auf "true"")



Diese Eigenschaft wird verwendet, um das Entfernen von Kacheln in `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


## <a name="entity-offsets"></a>Entität offsets

Entitäten, die in Form von Kacheln erstellt werden durch die Ausrichtung der Mitte der Entität mit dem Mittelpunkt der Kachel positioniert. Größere Entitäten wie `Door`, verwenden Sie zusätzliche Eigenschaften und Logik ordnungsgemäß abgelegt werden soll. 

Die unteren Tür Kachel, die definiert die `Door` Entität Platzierung gibt eine **YOffset** Wert 4. Ohne diese Eigenschaft die `Door` Instanz befindet sich in der Mitte der Kachel:

![](cointime-images/image19.png "Ohne diese Eigenschaft wird die Tür-Instanz in der Mitte der Kachel platziert.")

 

Dies wurde behoben, durch Anwenden der **YOffset** Wert in `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


## <a name="animated-entities"></a>Animierte Entitäten

Münzwurfs Zeit enthält mehrere animierte Entitäten. Die `Player` und `Enemy` Entitäten Walk Animationen wiedergeben und die `Door` Entität spielt eine Animation öffnen, nachdem alle Münzen erfasst wurden.


### <a name="achx-files"></a>.achx-Dateien

Münzwurfs Zeit Animationen werden in .achx-Dateien definiert. Jede Animation wird zwischen definiert `AnimationChain` tags, wie in der folgenden Animation in definierten gezeigt **propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

Diese Animation enthält nur einen einzelnen Frame, was in der Sammlung-Entität, die ein statisches Bild angezeigt. Entitäten können .achx-Dateien verwenden, ob sie einzelne oder mehrere Frame Animationen anzeigen. Zusätzliche Frames können ohne Änderungen im Code .achx Dateien hinzugefügt werden. 

Frames definieren, welche das im anzuzeigende Bild der `TextureName` Parameter und die Koordinaten der Anzeige in der `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, und `BottomCoordinate` Tags. Diese repräsentieren die Pixelkoordinaten des Frames Animation in der Abbildung der verwendet wird – **mastersheet.png** in diesem Fall.

![](cointime-images/image20.png "Diese repräsentieren die Pixelkoordinaten des Frames Animation in der Abbildung")

Die `FrameLength` Eigenschaft definiert die Anzahl von Sekunden an, die ein Frame in einer Animation angezeigt werden soll. Single-Frame-Animationen wird dieser Wert ignoriert.

Alle anderen AnimationChain Eigenschaften in der Datei .achx von Münzwurfs Zeit ignoriert.


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Animationslogik ist Bestandteil der `AnimatedSpriteEntity` Klasse, die als Basisklasse dient, für die meisten Entitäten in verwendet das `GameScene`. Es bietet die folgenden Funktionen:

 - Laden von `.achx` Dateien
 - Animation-Cache geladen Animationen
 - CCSprite-Instanz für die Animation anzeigen
 - Logik für den aktuellen Frame ändern

Der Konstruktor Spitzen bietet ein einfaches Beispiel zum Laden und verwenden Animationen:


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

Die **propAnimations.achx** enthält nur eine Animation aus, sodass der Konstruktor dieser Animation durch Index zugreift. Wenn eine .achx-Datei enthält mehrere Animationen, Animationen können den Namen verwiesen werden, entsprechend der `Enemy` Konstruktor:


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden behandelt die Implementierungsdetails der Würfe Zeit. Münzwurfs Zeit wird erstellt, um ein Spiel abgeschlossen werden, aber es ist auch ein Projekt, das einfach geändert und erweitert werden kann. Leser werden empfohlen, verbringen Zeit und Änderungen an Ebenen, neue Ebenen hinzufügen und Erstellen von neuen Entitäten um vertieft Ihr Verständnis wie Münzwurfs implementiert wird.

## <a name="related-links"></a>Verwandte links

- [Spielprojekt (Beispiel)](https://developer.xamarin.com/samples/mobile/CoinTime/)
