---
title: Details zu Münze-Uhrzeit-Spiel
description: Dieses Handbuch behandelt die Details zur Implementierung im Spiel Münze Zeit, einschließlich der Arbeit mit Kachel Zuordnungen, Erstellen von Entitäten, Animieren von Sprites und effiziente Kollisionen implementieren.
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: af914e10eb93aa0668743a113ffe647a939fea75
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61395897"
---
# <a name="coin-time-game-details"></a>Details zu Münze-Uhrzeit-Spiel

_Dieses Handbuch behandelt die Details zur Implementierung im Spiel Münze Zeit, einschließlich der Arbeit mit Kachel Zuordnungen, Erstellen von Entitäten, Animieren von Sprites und effiziente Kollisionen implementieren._

Münze wird eine vollständige Platformer Spiele für iOS und Android. Ziel des Spiels ist es, erfassen alle die Münzen auf einer Ebene, und klicken Sie dann die Tür beenden zugleich Feinde und Hindernisse zu erreichen.

![](cointime-images/image1.png "Das Ziel des Spiels ist, erfassen alle die Münzen auf einer Ebene, und klicken Sie dann die Tür beenden zugleich Feinde und Hindernisse zu erreichen")

Dieses Handbuch behandelt die Details der Implementierung in Münze Zeit, die folgenden Themen:

- [Arbeiten mit Tmx-Dateien](#working-with-tmx-files)
- [Ebene werden geladen](#level-loading)
- [Hinzufügen neuer Entitäten](#adding-new-entities)
- [Animierte Entitäten](#animated-entities)


## <a name="content-in-coin-time"></a>Inhalte in Münze Zeit

Münze wird ein Beispielprojekt, das darstellt, wie eine vollständige CocosSharp-Projekts organisiert werden kann. Coin Time Struktur zielt darauf ab, das Hinzufügen und die Wartung von Inhalt zu vereinfachen. Er verwendet **.tmx** von erstellten Dateien [Fläche](http://www.mapeditor.org) für Ebenen und XML-Dateien, um Animationen zu definieren. Ändern oder Hinzufügen von neuen Inhalt kann mit minimalem Aufwand erreicht werden. 

Während dadurch eine effektive Projekt lernen und experimentieren Münze Zeit ist, gibt, wieder wie Profi spielen erfolgen. Dieses Handbuch erklärt einige der Methoden zum Hinzufügen und Ändern von Inhalt zu vereinfachen.


## <a name="working-with-tmx-files"></a>Arbeiten mit Tmx-Dateien

Zeitebenen Münze werden definiert die .tmx-Dateiformat, das ausgegeben wird, indem die [Fläche](http://www.mapeditor.org) -Editor für zeichenzuordnung Kachel. Eine ausführliche Erläuterung der Arbeit mit Fläche, finden Sie unter den [Verwenden von Tiled mit Cocos Sharp Handbuch](~/graphics-games/cocossharp/tiled.md). 

Jede Ebene wird in eine eigene .tmx-Datei definiert die **CoinTime/Assets/Inhalt/-Stufen** Ordner. Alle Münze Zeitebenen Freigeben einer Tileset-Datei, die in definiert ist die **mastersheet.tsx** Datei. Diese Datei definiert die benutzerdefinierten Eigenschaften für jede Kachel, z. B., ob die Kachel solid Konflikt hat, oder gibt an, ob die Kachel "durch eine Instanz einer Entität ersetzt werden soll. Die Datei mastersheet.tsx kann Eigenschaften nur einmal definiert und auf allen Ebenen verwendet werden. 


### <a name="editing-a-tile-map"></a>Bearbeiten einer Kachel-Zuordnung

Um eine Zuordnung für die Kachel zu bearbeiten, öffnen Sie die .tmx-Datei in Fläche durch Doppelklicken auf die .tmx-Datei, oder öffnen sie im Menü "Datei" in der Fläche. Weder die Zeit, die auf der Kachel-Zuordnungen mit drei Ebenen enthalten: 

- **Entitäten** : Diese Ebene enthält Kacheln, die Instanzen der Entitäten zur Laufzeit ersetzt werden. Zählen der Spieler, Münzen, Feinde und die Ende-des-Level-Tür.
- **Terrain** : Diese Ebene enthält Kacheln, die die solid Konflikt in der Regel haben. Solid Konflikte kann den Player auf diesen Kacheln zu durchlaufen, ohne durchfallen. Kacheln mit solid Konflikt können auch als Wände und decken fungieren.
- **Hintergrund** – die Hintergrundebene enthält Kacheln, die als statische Hintergrund verwendet werden. Diese Ebene wird kein Bildlauf ausgeführt, wechselt die Kamera in der Ebene, die den Anschein von Tiefe durch Parallax erstellen.

Wie wir später untersuchen werden, erwartet der Ebene Laden von Code diese drei Schichten auf allen Ebenen der Münze Zeit.

#### <a name="editing-terrain"></a>Bearbeiten von Gelände

Kacheln platziert werden können, indem Sie auf die **Mastersheet** Tileset, und klicken Sie dann auf die Kachel zugeordnet werden. Geben Sie beispielsweise Folgendes ein, um neue Gelände in einer Ebene gezeichnet werden soll:

1. Wählen Sie die Terrain-Ebene
1. Klicken Sie auf die Kachel, um zu zeichnen
1. Klicken Sie auf oder mithilfe von Push übertragen Sie, und ziehen Sie auf der Karte, um die Kachel zu zeichnen.

    ![](cointime-images/image2.png "Klicken Sie auf die Kachel, um 1 zu zeichnen.")

Der oberen linken Ecke des der Tileset enthält alle dem Gelände rechtzeitig Münze. Terrain, denen Volltonfarbe angezeigt wird, enthält die **SolidCollision** Eigenschaft, die in den kacheleigenschaften auf der linken Seite des Bildschirms angezeigt:

![](cointime-images/image3.png "Terrain, denen Volltonfarbe angezeigt wird, enthält die SolidCollision-Eigenschaft, an, wie in den kacheleigenschaften auf der linken Seite des Bildschirms angezeigt.")

#### <a name="editing-entities"></a>Bearbeiten von Entitäten

Entitäten können hinzugefügt oder entfernt aus einer Ebene – genau wie Gelände. Die **Mastersheet** Tileset verfügt über alle Entitäten, die platziert zu während des laufenden Vorgangs horizontal aus, damit sie nicht angezeigt werden können, ohne Bildlauf nach rechts:

![](cointime-images/image4.png "Die Mastersheet Tileset verfügt über alle Entitäten, die platziert zu während des laufenden Vorgangs horizontal aus, damit sie nicht angezeigt werden können, ohne Bildlauf nach rechts")

Neue Entitäten platziert werden soll, auf die **Entitäten** Ebene.

![](cointime-images/image5.png "Neue Entitäten sollten auf der Ebene von Entitäten gespeichert werden")

CoinTime Code sucht nach dem **EntityType** beim Laden von einer Ebene auf die um Kacheln zu identifizieren, die von Entitäten ersetzt werden soll: 

![](cointime-images/image6.png "CoinTime Code sucht für den EntityType, beim Laden einer Ebene aus, um die Kacheln zu identifizieren, die von Entitäten ersetzt werden soll")

Nachdem die Datei geändert und gespeichert wurde, werden die Änderungen automatisch angezeigt, wenn das Projekt erstellt und ausgeführt wird:

![](cointime-images/image7.png "Nachdem die Datei geändert und gespeichert wurde, werden die Änderungen automatisch angezeigt, wenn das Projekt erstellt und ausgeführt wird")


### <a name="adding-new-levels"></a>Hinzufügen von neuen Ebenen

Der Vorgang des Hinzufügens von Ebenen in Münze Zeit erfordert keine Änderungen am Code und nur ein paar kleinen Änderungen auf das Projekt. So fügen Sie eine neue Ebene hinzu:

1. Öffnen Sie den Ordner sich unter <**CoinTime Root > \CoinTime\Assets\Content\levels**
1. Kopieren Sie eine der Ebenen, wie z. B. **level0.tmx**
1. Benennen Sie die neue .tmx-Datei aus, damit sie die Ebene Zahlensequenz mit vorhandenen Ebenen, wie z. B. weiterhin **level8.tmx**
1. In Visual Studio oder Visual Studio für Mac müssen fügen Sie die neue .tmx-Datei in den Android-Ebenen-Ordner hinzu. Stellen Sie sicher, dass die Datei verwendet die **AndroidAsset** Buildvorgang.

    ![](cointime-images/image8.png "Stellen Sie sicher, dass die Datei den Buildvorgang AndroidAsset verwendet.")

1. Die neue .tmx-Datei in den iOS-Ebenen-Ordner hinzufügen. Achten Sie darauf, verknüpfen Sie die Datei von ihrem ursprünglichen Speicherort, und stellen Sie sicher, dass er verwendet den **BundleResource** Buildvorgang.

    ![](cointime-images/image9.png "Achten Sie darauf, verknüpfen Sie die Datei von ihrem ursprünglichen Speicherort, und stellen Sie sicher, dass den Buildvorgang BundleResource verwendet")

Die neue Ebene sollte angezeigt werden in den Bildschirm für das Auswählen von Ebene als Ebene 9 (Dateinamen Ebene beginnen bei 0, aber die Schaltflächen für die Ebenen zu beginnen, mit der Zahl 1):

![](cointime-images/image10.png "Die neue Ebene sollte angezeigt werden in den Bildschirm für das Auswählen von Ebene als Ebene 9 auf Datei beginnen bei 0, aber die Schaltflächen für die Ebenen beginnen, mit der Zahl 1")


## <a name="level-loading"></a>Ebene werden geladen

Wie bereits erwähnt, neue Ebenen keine Änderungen im Code erforderlich sind – das Spiel erkennt automatisch die Ebenen, wenn sie mit dem Namen ordnungsgemäß und hinzugefügt werden die **Ebenen** Ordner mit den richtigen Buildvorgang (**BundleResource**oder **AndroidAsset**).

Die Logik zum Bestimmen der Anzahl der Ebenen ist Bestandteil der `LevelManager` Klasse. Wenn eine Instanz von der `LevelManager` erstellt wird (mithilfe der Singleton-Muster), die `DetermineAvailbleLevels` aufgerufen:


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

CocosSharp bietet keinen plattformübergreifenden Ansatz für das ermitteln, ob Dateien vorhanden sind, müssen wir abhängig der `TitleContainer` Klasse an, die zum Öffnen eines Datenstroms. Wenn der Code für das Öffnen eines Datenstroms, eine Ausnahme, und klicken Sie dann auf die Datei auslöst nicht vorhanden und die While-Schleife unterbrochen. Sobald die Schleife abgeschlossen ist, die `NumberOfLevels` Eigenschaft meldet, wie viele die gültigen Werte Teil eines Projekts sind.

Die `LevelSelectScene` -Klasse verwendet die `LevelManager.NumberOfLevels` um zu ermitteln, wie viele Erstellung in der `CreateLevelButtons` Methode:


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

Die `NumberOflevels` Eigenschaft wird verwendet, um zu bestimmen, welche Schaltflächen erstellt werden soll. Dieser Code betrachtet, welche Seite der Benutzer aktuell angezeigt wird, und erstellt nur maximal sechs Schaltflächen pro Seite. Beim Klicken auf die Schaltfläche "-Instanzen Aufrufen der `HandleButtonClicked` Methode:


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Diese Methode weist die `CurrentLevel` Eigenschaft, die von verwendet wird, die `GameScene` beim Laden einer Ebene. Nach dem Festlegen der `CurrentLevel`, `GoToGameScene` Methode ausgelöst wird, wechseln von der Szene aus `LevelSelectScene` zu `GameScene`.

Die `GameScene` Konstruktoraufrufe `GoToLevel`, die die Ebene laden Logik ausführt:


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

Als Nächstes werden wir uns das ansehen an in aufgerufenen Methoden `GoToLevel`.


### <a name="loadlevel"></a>LoadLevel

Die `LoadLevel` Methode dient zum Laden der .tmx-Datei und zum Hinzufügen, die `GameScene`. Diese Methode erstellt keinen interaktiven Objekte wie z. B. Konflikt oder Entitäten – einfach die visuellen Objekte für die Ebene, die so genannte erstellt die *Umgebung*.


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

Die `CCTileMap` Konstruktor nimmt einen Dateinamen ein, die erstellt wird, verwenden die Nummer die Ebene der an übergebene `LoadLevel`. Weitere Informationen zum Erstellen und Arbeiten mit `CCTileMap` -Instanzen finden Sie unter den [Verwenden von Tiled mit CocosSharp-Handbuch](~/graphics-games/cocossharp/tiled.md).

Derzeit CocosSharp lässt nicht zu neuanordnung von Schichten, ohne zu entfernen und erneut hinzugefügt wurden, zu ihrer übergeordneten `CCScene` (d.h. die `GameScene` in diesem Fall), sodass die letzten Zeilen der Methode erforderlich sind, um die Ebenen neu anzuordnen.


### <a name="createcollision"></a>CreateCollision

Die `CreateCollision` Methode erstellt eine `LevelCollision` -Instanz, die zum Ausführen *solid Kollisionen* zwischen den Player und die Umgebung.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Ohne zu diesem Konflikt ist der Spieler fiel über der Ebene und das Spiel viele. Solid Kollisionen können den Player, die auf den Boden zu durchlaufen und verhindert, dass den Spieler Wände durchlaufen oder sich über Obergrenzen springen.

Kollisionen in Münze Zeit kann ohne zusätzlichen Code – nur Änderungen an gekachelten Dateien hinzugefügt werden. 


### <a name="processtileproperties"></a>ProcessTileProperties

Sobald eine Ebene geladen wird und der Konflikt erstellt wird, `ProcessTileProperties` wird aufgerufen, um die Logik, die basierend auf der kacheleigenschaften auszuführen. Münze Zeit umfasst eine `PropertyLocation` Struktur zum Definieren von Eigenschaften und die Koordinaten der Kachel mit den folgenden Eigenschaften:


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

Diese Struktur wird zum Erstellen verwendet Instanzen der Entität zu erstellen und entfernen Sie unnötige Kacheln in der `ProcessTileProperties` Methode:


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

Die Foreach-Schleife wertet jede Kachel-Eigenschaft, die überprüft, ob der Schlüssel ist `EntityType` oder `RemoveMe`. `EntityType` Gibt an, dass eine Instanz einer Entität erstellt werden soll. `RemoveMe` Gibt an, dass die Kachel zur Laufzeit vollständig entfernt werden soll.

Wenn eine Eigenschaft mit dem `EntityType` Schlüssel gefunden wird, klicken Sie dann `TryCreateEntity` aufgerufen wird, wird versuchen, eine Entität, die mithilfe der Eigenschaft Übereinstimmungsregeln erstellen die `EntityType` Schlüssel:


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


## <a name="adding-new-entities"></a>Hinzufügen neuer Entitäten

Münze Zeit verwendet der Entity-Muster für die Spielobjekte (die finden Sie in der [Entitäten in CocosSharp Anleitung](~/graphics-games/cocossharp/entities.md)). Alle Entitäten, die von erben `CCNode`, was bedeutet, sie können als untergeordnete Elemente hinzugefügt werden die `gameplayLayer`.

Jeder Entitätstyp ist auch direkt über eine Liste oder eine einzelne Instanz auf die verwiesen wird. Z. B. die `Player` verweist auf die `player` Feld und alle `Coin` Instanzen auf die verwiesen wird einem `coins` Liste. Direkte Verweise auf Entitäten beibehalten (anstatt über die `gameLayer.Children` Liste) wird Code greift auf diese Entitäten, die einfacher zu lesen und beseitigt die Typumwandlung möglicherweise aufwendig.

Der vorhandene Code bietet es sich um eine Reihe von Entitätstypen als Beispiele zum Erstellen neuer Entitäten. Die folgenden Schritte können verwendet werden, um eine neue Entität erstellen:


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1: Definieren Sie eine neue Klasse, die mit dem Entity-Muster

Die einzige Voraussetzung für das Erstellen einer Entität ist eine Klasse erstellen, die von erbt `CCNode`. Die meisten Entitäten verfügen über eine Visualisierung, wie z. B. eine `CCSprite`, die als untergeordnetes Element der Entität in seinem Konstruktor hinzugefügt werden sollen.

CoinTime bietet die `AnimatedSpriteEntity` Klasse, die die Erstellung der animierten Entitäten vereinfacht. Animationen werden ausführlicher behandelt die [animiert Entitäten Abschnitt](#animated-entities).


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – Fügen Sie einen neuen Eintrag für die TryCreateEntity Switch-Anweisung

Instanzen der neuen Entität sollte instanziiert werden, der `TryCreateEntity`. Wenn die Entität für jeden-Frame-Logik, wie Kollisionen, künstliche Intelligenz und Lesen von Eingaben, müssen die `GameScene` einen Verweis auf das Objekt speichern muss. Wenn mehrere Instanzen erforderlich sind (z. B. `Coin` oder `Enemy` Instanzen), klicken Sie dann ein neues `List` hinzugefügt werden sollen die `GameScene` Klasse.


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3 – ändern Sie die Kachel "-Eigenschaften für die neue Entität

Nachdem der Code die Erstellung der neuen Entität unterstützt, muss die neue Entität die Tileset hinzugefügt werden. Die Tileset kann bearbeitet werden, durch Öffnen einer beliebigen Ebene `.tmx` Datei. 

Befindet sich die Tileset getrennt von den .tmx in die **mastersheet.tsx** Datei, sodass es importiert werden muss, bevor sie bearbeitet werden kann:

![](cointime-images/image11.png "Die Tileset wird getrennt von der TSX-Datei gespeichert, damit es importiert werden muss, bevor sie bearbeitet werden können")

Nachdem Sie importiert haben, können Eigenschaften für ausgewählte Kacheln, die bearbeitet werden, und der EntityType hinzugefügt werden kann:

![](cointime-images/image12.png "Sobald importiert haben, Eigenschaften für ausgewählte Kacheln, die bearbeitet werden können, und der EntityType hinzugefügt werden können")

Nachdem die Eigenschaft erstellt wurde, kann der Wert festgelegt werden, entsprechend der neuen `case` in `TryCreateEntity`:

![](cointime-images/image13.png "Nachdem die Eigenschaft erstellt wurde, kann der Wert festgelegt werden entsprechend den neuen Fall in TryCreateEntity")

Nachdem die Tileset geändert wurde, muss exportiert werden kann – dadurch werden die Änderungen nur für alle anderen Stufen verfügbar:

![](cointime-images/image14.png "Nach der Tileset geändert wurde, müssen sie die exportiert werden")

Sollte die Tileset überschreiben Sie die existierenden **mastersheet.tsx** Tileset:

![](cointime-images/image15.png "HE Tileset sollten die vorhandenen mastersheet.tsx Tileset überschrieben.")


## <a name="entity-tile-removal"></a>Entität Kachel entfernen

Beim Laden einer Kachel-Zuordnung in ein Spiel, sind die einzelnen Kacheln statische Objekte. Da Entitäten benutzerdefiniertes Verhalten, z. B. datenverschiebung erforderlich ist, entfernt Münze uhrzeitcode von Kacheln aus, wenn Entitäten erstellt werden.

`ProcessTileProperties` enthält die Logik zum Entfernen von Kacheln die Entitäten mit dem Erstellen der `RemoveTile` Methode:


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

Automatische Entfernen dieser Kacheln ist ausreichend für Entitäten, die nur eine Kachel in der Tileset, z. B. Münzen und Feinde belegt. Größere Entitäten erfordern zusätzliche Logik und Eigenschaften.

Die Tür erfordert zwei Kacheln, vollständig gezeichnet werden soll:

![](cointime-images/image16.png "Die Tür erfordert zwei Kacheln, vollständig gezeichnet werden soll")

Die Kachel unten in der Tür enthält die Eigenschaften für das Erstellen einer Entität (**EntityType** festgelegt **Tür**):

![](cointime-images/image17.png "Die Kachel unten in der Tür enthält die Eigenschaften für das Erstellen einer Entität werden die Tür EntityType fest")

Da nur die unteren Kachel in der Tür entfernt wird, wenn die Tür-Instanz erstellt wird, ist zusätzlicher Logik erforderlich, um die oberste Kachel zur Laufzeit zu entfernen. Die oberste Kachel verfügt über eine **RemoveMe** -Eigenschaftensatz auf **"true"**:

![](cointime-images/image18.png "Die oberste Kachel hat eine RemoveMe-Eigenschaft auf \"true\"")



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


## <a name="entity-offsets"></a>Entity-offsets

Entitäten, die von Kacheln erstellt haben, werden durch die Ausrichtung der Mitte der Entität mit dem Mittelpunkt der Kachel positioniert. Größere Entitäten wie `Door`, verwenden Sie zusätzliche Eigenschaften und Logik, um richtig platziert werden. 

Die unteren Tür-Kachel, die definiert die `Door` Entität Platzierung gibt eine **YOffset** Wert 4. Ohne diese Eigenschaft die `Door` Instanz befindet sich in der Mitte der Kachel:

![](cointime-images/image19.png "Ohne diese Eigenschaft wird die Tür-Instanz in der Mitte der Kachel platziert.")

 

Dies wird durch Anwenden von korrigiert die **YOffset** Wert `ProcessTileProperties`:


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

Münze Zeit enthält mehrere animierte Entitäten. Die `Player` und `Enemy` Entitäten Walk-Animationen wiederzugeben und die `Door` Entität spielt eine Animation beim Öffnen aus, nachdem alle Münzen erfasst wurden.


### <a name="achx-files"></a>.achx-Dateien

Münze Zeit Animationen werden in .achx-Dateien definiert. Jede Animation wird zwischen definiert `AnimationChain` tags, siehe die folgende Animation, die in definierten **propanimations.achx**:


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

Diese Animation enthält nur ein einzelnes Frame, wodurch die Spitze-Entität, die ein statisches Bild angezeigt. Entitäten können .achx-Dateien verwenden, ob sie einzelne oder mehrere Frame-Animationen anzeigen. Zusätzlichen Frames können .achx Dateien hinzugefügt werden, ohne Änderungen im Code. 

Frames definieren, welche anzuzeigenden Bilds die `TextureName` Parameter und die Koordinaten der Anzeige in der `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, und `BottomCoordinate` Tags. Diese repräsentieren die Pixelkoordinaten der der Frame der Animation in der Abbildung der verwendet wird – **mastersheet.png** in diesem Fall.

![](cointime-images/image20.png "Diese repräsentieren die Pixelkoordinaten der der Frame der Animation in der Abbildung")

Die `FrameLength` Eigenschaft definiert die Anzahl der Sekunden an, die der Frame einer Animation angezeigt werden soll. Single-Frame-Animationen wird dieser Wert ignoriert.

Alle anderen AnimationChain Eigenschaften, die in der Datei .achx werden nach Münze ignoriert.


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Animationslogik ist Bestandteil der `AnimatedSpriteEntity` -Klasse, die als Basisklasse verwendet werden soll, für die meisten Entitäten in verwendet die `GameScene`. Es bietet die folgenden Funktionen:

 - Laden von `.achx` Dateien
 - Animation-Cache geladen Animationen
 - CCSprite-Instanz für die Animation anzeigen
 - Logik für den aktuellen Frame ändern

Der Konstruktor Spitzen bietet ein einfaches Beispiel für das Laden und verwenden Sie Animationen an:


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

Die **propAnimations.achx** enthält nur eine Animation aus, sodass der Konstruktor dieser Animation über den Index zugreift. Wenn eine Datei .achx enthält mehrere Animationen, Animationen können den Namen verwiesen werden, siehe die `Enemy` Konstruktor:


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden behandelt die Implementierungsdetails der Münze Zeit. Münze Zeit wird erstellt, um ein Spiel abgeschlossen werden, aber es ist auch ein Projekt, das leicht geändert und erweitert werden kann. Leser werden empfohlen, verbringen alle Änderungen von Zeit zu Ebenen, neue Ebenen hinzufügen und erstellen neue Entitäten, um besser zu verstehen, wie die Münze Zeit implementiert wird.

## <a name="related-links"></a>Verwandte Links

- [-Spielprojekt (Beispiel)](https://developer.xamarin.com/samples/mobile/CoinTime/)
