---
title: Mit Kacheleffekt mit CocosSharp
description: Kacheleffekt ist ein leistungsfähiges, flexibles und ordnet ausgereifte Anwendung zum Erstellen der Kachel "orthogonale und isometrische" für Spiele. CocosSharp bietet eine integrierte Integration für systemeigene Dateiformat der Fläche.
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 8a7782097324829b8150b968c5658a864d1fab4a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921299"
---
# <a name="using-tiled-with-cocossharp"></a>Mit Kacheleffekt mit CocosSharp

_Kacheleffekt ist ein leistungsfähiges, flexibles und ordnet ausgereifte Anwendung zum Erstellen der Kachel "orthogonale und isometrische" für Spiele. CocosSharp bietet eine integrierte Integration für systemeigene Dateiformat der Fläche._

Die *Fläche* Anwendung ist ein Standard zum Erstellen von *Kachel Maps* für die Verwendung in 3D-Spielentwicklung. Diese Anleitung begleiten erläutert, wie eine vorhandene .tmx-Datei (Datei erstellt, indem Fläche) und in ein Spiel CocosSharp verwenden. Speziell für dieses Handbuch befasst sich mit:

 - Der Zweck der Kachel Zuordnungen
 - Arbeiten mit .tmx-Dateien
 - Überlegungen für das Rendern von Grafiken pixel
 - Von der kacheleigenschaften zur Laufzeit

Wenn abgeschlossen haben wir die folgenden Demo:

![](tiled-images/image1.png "Die Demo-app erstellt, indem Sie die Schritte in diesem Handbuch")


## <a name="the-purpose-of-tile-maps"></a>Der Zweck der Kachel Zuordnungen

Kachel-Zuordnungen im 3D-Spielentwicklung Jahrzehnten gewesen, jedoch werden weiterhin häufig in 2D Spiele für ihre Effizienz und Esthetics verwendet. Kachel-Karten sind erzielt eine sehr hohe Effizienz über ihre Nutzung der Kachel legt – Quellbilds von Kachel Maps verwendet. Eine Kachel-Menge ist eine Sammlung von Bildern, die in eine Datei zusammengefasst. Obwohl Kachel legt auf in der Kachel Zuordnungen verwendete Bilder zu verweisen, sind Dateien, die mehrere kleinere Bilder enthalten auch Sprite Blätter oder aufgerufen Sprite in 3D-Spielentwicklung zugeordnet. Wir können visuell darstellen, wie die Kachel Mengen verwendet werden, durch Hinzufügen eines Rasters auf die Kachel-Gruppe, die in der Demo verwendet werden:

![](tiled-images/image2.png "Einen dargestellte Überblick darüber, wie die Kachel Mengen verwendet werden, durch Hinzufügen eines Rasters auf die Kachel-Gruppe, die in dieser Demo verwendet werden")

Kachel Maps Anordnen der einzelnen Kacheln aus Kachel Zeichensätzen. Wir Beachten Sie, dass jede Kachel-Zuordnung nicht zum Speichern einer eigene Kopie der Kachel festgelegt – stattdessen mehrere Kachel Zuordnungen können verweisen, den gleichen Satz der Kachel. Dies bedeutet, dass abgesehen von der Menge Kachel Kachel Zuordnungen sehr wenig Speicher benötigen. Dies ermöglicht die Erstellung einer großen Anzahl von Karten, die Kachel, sogar, wenn sie verwendet werden, um einen Bereich große spielen, wie eine [Durchführen eines Bildlaufs Platformer](http://en.wikipedia.org/wiki/Platform_game) Umgebung. Das folgende Beispiel zeigt, mit den gleichen Kachel möglichen per Zufall neu:

![](tiled-images/image3.png "Dieses Bild zeigt die möglichen per Zufall neu, die mit den gleichen Kachel")

![](tiled-images/image4.png "Dieses Bild zeigt die möglichen per Zufall neu, die mit den gleichen Kachel")


## <a name="working-with-tmx-files"></a>Arbeiten mit .tmx-Dateien

Das .tmx-Dateiformat ist eine XML-Datei erstellt, die von der Anwendung Fläche kann u. [kostenlos heruntergeladen, auf der Website Fläche](http://www.mapeditor.org/). Das Dateiformat .tmx speichert die Informationen für die Kachel Zuordnungen. In der Regel wird ein Spiel, das eine .tmx-Datei für die einzelnen Ebenen oder separate Bereiche haben.

Dieses Handbuch konzentriert sich auf vorhandene .tmx-Dateien in CocosSharp mit; Allerdings Weitere Lernprogramme finden Sie online, einschließlich [dieser Einführung in dem Fläche Zuordnungs-Editor](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838).

Sie müssen beim Entzippen von der [Inhalt der Zip-Datei](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) für die Verwendung unserer Spiele. Erstes sollten Sie beachten ist, dass Kachel Zuordnungen beide die .tmx-Datei (dungeon.tmx) sowie eine verwenden oder mehrere Bilddateien die definieren, die Kachel Daten (dungeon_1.png) festlegen. Muss beide Dateien beim Laden der .tmx zur Laufzeit enthalten das Spiel so sowohl dem Projekt hinzufügen **Content** Ordner (dem ist Bestandteil der **Bestand** Ordner in der Android-Projekte). Insbesondere die Dateien hinzufügen, um einen Ordner namens **Tilemaps** innerhalb der **Content** Ordner:

![](tiled-images/image5.png "Fügen Sie die Dateien in einem Ordner namens Tilemaps in den Inhaltsordner")

Die **dungeon.tmx** Datei kann jetzt geladen werden, zur Laufzeit in einem `CCTileMap` Objekt. Im nächsten Schritt ändern die `GameLayer` (oder entsprechende Root-Container-Objekt) enthält eine `CCTileMap` Instanz und es als untergeordnetes Element hinzuzufügen:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

Beim Ausführen des Spiels, die wir sehen die Kachel-Zuordnung angezeigt, in der unteren linken Ecke des Bildschirms:

![](tiled-images/image6.png "Wenn das Spiel ausgeführt wird, die Zuordnung der Kachel angezeigt werden, in der unteren linken Ecke des Bildschirms")


## <a name="considerations-for-rendering-pixel-art"></a>Überlegungen für das Rendern von Grafiken pixel

Pixel-Grafiken, im Kontext der Video-Spielentwicklung, bezieht sich auf 2D visual Art von Hand ist in der Regel erstellt und ist häufig mit niedriger Auflösung. Pixel Art eingeschränkten kann zeitaufwändig zu erstellen, damit Pixel Art Kachel häufig mit niedriger Auflösung Kacheln, z. B. 16- oder 32 Pixelbreite und Höhe umfassen,. Wenn zur Laufzeit nicht skaliert wird, ist die Pixel Kunst häufig zu klein für die meisten modernen Telefone und Tablets.

Wir können die angezeigte Dimensionen in unserer Spiel GameAppDelegate.cs-Datei, in denen fügen wir einen Aufruf von anpassen `CCScene.SetDefaultDesignResolution`:


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
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
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

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

Weitere Informationen zu `CCSceneResolutionPolicy`, finden Sie auf unserer Anleitung [Behandlung von Lösungen in CocosSharp](~/graphics-games/cocossharp/resolutions.md).

Wenn wir das Spiel jetzt ausführen, sehen wir das Spiel einnehmen den gesamten Bildschirm Ihr Gerät:

![](tiled-images/image7.png "Das Spiel sich über den gesamten Bildschirm des Geräts")

Abschließend möchten wir Antialiasing auf unserem Kachel Karte zu deaktivieren. Die `Antialiased` Eigenschaft gilt eine Verwischung Auswirkung beim Rendern von Objekten das Diagramm vergrößert werden. Antialiasing kann zur Reduzierung der pixelig-Darstellung der Grafikobjekte nützlich sein, aber möglicherweise auch einen eigenen Renderingartefakte einführen. Antialiasing verwischt insbesondere den Inhalt der einzelnen Kacheln. Allerdings werden die Kanten der einzelnen Kacheln nicht unklar, wodurch die einzelnen Kacheln statt mit angrenzenden Kacheln blending hervorzuheben. Wir auch beachten, dass Pixel Art Spiele häufig ihre pixelig Erscheinungsbild verwalten beibehalten einer *retro* fühlen.

Legen Sie `Antialiased` auf `false` nach dem Erstellen der `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

Unsere Kachel-Zuordnung wird jetzt nicht unscharf angezeigt:

![](tiled-images/image8.png "Jetzt wird die Kachel-Zuordnung nicht unscharf angezeigt")


## <a name="using-tile-properties-at-runtime"></a>Von der kacheleigenschaften zur Laufzeit

Bisher haben wir eine `CCTileMap` Laden einer Datei .tmx sowie die Anzeige, aber wir haben keine Möglichkeit, die mit ihm zu interagieren. Insbesondere benötigen bestimmte Kacheln (z. B. unsere Glücksspiel Kasten) benutzerdefinierten Logik. Es werden schrittweise durchlaufen Gewusst wie: Erkennen von benutzerdefinierten kacheleigenschaften und verschiedene Möglichkeiten, auf diese Eigenschaften, die einmal zur Laufzeit identifiziert, zu reagieren.

Bevor wir keinen Code schreiben, müssen wir unsere Map Tile über Fläche Eigenschaften hinzuzufügen. Zu diesem Zweck öffnen Sie die Datei dungeon.tmx im Programm, Fläche. Achten Sie darauf, öffnen Sie die Datei, der verwendet wird, wird in das Spielprojekt.

Sobald geöffnet ist, wählen Sie die Kasten Glücksspiel in der Kachel festgelegt werden, um seine Eigenschaften anzuzeigen:

![](tiled-images/image9.png "Sobald geöffnet ist, wählen Sie die Kasten Glücksspiel in der Kachel festgelegt werden, um seine Eigenschaften anzuzeigen")

Wenn die Glücksspiel Kasten Eigenschaften nicht angezeigt werden, mit der rechten Maustaste auf die Glücksspiel Kasten, und wählen **Kacheleigenschaften**:

![](tiled-images/image10.png "Wenn die Glücksspiel Kasten Eigenschaften nicht angezeigt werden, mit der rechten Maustaste auf die Glücksspiel Kasten und Auswählen der Kacheleigenschaften")

Gekachelte Eigenschaften werden mit einem Namen und einen Wert implementiert. Um eine Eigenschaft hinzuzufügen, klicken Sie auf die **+** Schaltfläche, geben Sie den Namen **IsTreasure**, klicken Sie auf **OK**, geben Sie dann den Wert **"true"**: 

![](tiled-images/image11.png "Um eine Eigenschaft hinzuzufügen, klicken Sie auf die Schaltfläche \"", geben Sie den Namen IsTreasure, klicken Sie auf OK, und geben Sie den Wert \"true\"")

Vergessen Sie nicht zum Speichern der .tmx-Datei nach dem Ändern der Eigenschaften.

Abschließend fügen wir den Code für die neu hinzugefügten Eigenschaft gesucht werden soll. Wir wird die Schleife durchlaufen Jedes `CCTileMapLayer` (unsere Zuordnung verfügt über 2 Ebenen), klicken Sie dann über jede Zeile und Spalte für alle Kacheln gesucht, denen die `IsTreasure` Eigenschaft:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

Ein Großteil des Codes ist selbsterklärend, jedoch sollten erörtert, die Behandlung von Glücksspiel Kacheln. In diesem Fall werden wir alle Kacheln entfernen, die als Schatztruhen identifiziert werden. Dies liegt daran Schatztruhen wahrscheinlich mit benutzerdefinierten Code zur Laufzeit zu Auswirkungen Konflikten und dem Player den Inhalt der Glücksspiel beim Öffnen von Vergabe benötigen. Darüber hinaus müssen die Glücksspiel, zu reagieren, wird geöffnet (Ändern der visuellen Darstellung) und möglicherweise die Logik für nur wenn alle auf dem Bildschirm angezeigten Feinde wird außer Kraft gesetzt wurden.

Die Glücksspiel Kasten profitieren also von wird eine Entität, anstatt eine einfache Kachel in der `CCTileMap`. Weitere Informationen zum Spiel Entitäten, finden Sie unter der [Entitäten in CocosSharp geführt](~/graphics-games/cocossharp/entities.md).


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird beschrieben, wie .tmx-Dateien erstellt, indem Fläche in einer Anwendung CocosSharp laden. Es zeigt, wie so ändern Sie die app-Auflösung niedrigerer Auflösung Pixel Art berücksichtigen und Gewusst wie: Suchen von Kacheln nach deren Eigenschaften, wie das Erstellen von Instanzen der Entität, benutzerdefinierten Logik ausführen.

## <a name="related-links"></a>Verwandte links

- [Gekacheltes Website](http://www.mapeditor.org/)
- [Inhalt zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
