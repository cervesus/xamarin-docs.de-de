---
title: Verwenden von Tiled mit CocosSharp
description: Kacheleffekt ist ein leistungsfähiges, flexibles und ordnet ausgereifte Anwendung zum Erstellen der Kachel "orthogonal und isometrischen" für Spiele. CocosSharp bietet eine integrierte Integration für systemeigene Dateiformat der Fläche.
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4582b59a8a441c9e22761d498126898e66db08c1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117927"
---
# <a name="using-tiled-with-cocossharp"></a>Verwenden von Tiled mit CocosSharp

_Kacheleffekt ist ein leistungsfähiges, flexibles und ordnet ausgereifte Anwendung zum Erstellen der Kachel "orthogonal und isometrischen" für Spiele. CocosSharp bietet eine integrierte Integration für systemeigene Dateiformat der Fläche._

Die *Fläche* Anwendung ist ein Standard zum Erstellen von *Kachel Maps* für die Verwendung in der Entwicklung von Spielen. Dieses Handbuch führt durch die eine vorhandene .tmx-Datei (Datei Fläche erstellt) und in ein CocosSharp-Spiel verwenden. Speziell für dieses Handbuch befasst sich:

 - Der Zweck der Kachel-Zuordnungen
 - Arbeiten mit .tmx-Dateien
 - Überlegungen zum Rendern der Pixel-Grafik
 - Mithilfe der kacheleigenschaften zur Laufzeit

Wenn abgeschlossen haben wir die folgende Demo:

![](tiled-images/image1.png "Die Demo-app, die anhand der Schritte in dieser Anleitung erstellt")


## <a name="the-purpose-of-tile-maps"></a>Der Zweck der Kachel-Zuordnungen

Kachel-Zuordnungen in der Entwicklung von Spielen für mehrere Jahrzehnte gewesen, aber werden immer noch häufig in 2D-Spiele für ihre Effizienz und Esthetics verwendet. Kachel-Karten sind ein sehr hohes Maß an Effizienz durch seine Nutzung der Kachel legt – das Quellbild von Kachel Maps verwendet erreichen. Eine Kachel-Gruppe ist eine Auflistung von Bildern in einer Datei zusammengefasst. Kachel-Sätze in Kachel Zuordnungen verwendete verweisen, Dateien, die mehrere kleinere Bilder enthalten sind so genannte Sprite-Sheets oder Sprite-Zuordnungen in der Entwicklung von Spielen. Wir können visualisieren, wie die Kachel legt verwendet werden, durch das Hinzufügen von einem Raster auf die Kachel, die wir in unserem Beispiel verwenden:

![](tiled-images/image2.png "Einen visualisiert Überblick darüber, wie die Kachel legt verwendet werden, durch das Hinzufügen von einem Raster auf die Kachel, die in der Demo verwendet werden")

Kachel Maps ordnen Sie die einzelnen Kacheln Kachel ab. Wir sollten beachten, dass jede Kachel Zuordnung nicht zum Speichern einer eigene Kopie der Kachel festgelegt – stattdessen mehrere Kachel-Zuordnungen können verweisen, den gleichen kachelsatz. Dies bedeutet, dass abgesehen von der Menge Kachel Kachel Zuordnungen sehr wenig Speicher erforderlich. Dies ermöglicht die Erstellung einer großen Anzahl von Karten, die Kachel, selbst wenn sie verwendet werden, erstellen Sie einen großen Spiels-Bereich, z. B. eine [Durchführen eines Bildlaufs Platformer](http://en.wikipedia.org/wiki/Platform_game) Umgebung. Das folgende Beispiel zeigt möglichen Anordnungen neu mit denselben Kachel:

![](tiled-images/image3.png "In dieser Abbildung werden die möglichen Anordnungen neu mit denselben Kachel")

![](tiled-images/image4.png "In dieser Abbildung werden die möglichen Anordnungen neu mit denselben Kachel")


## <a name="working-with-tmx-files"></a>Arbeiten mit .tmx-Dateien

Das Format der .tmx ist eine XML-Datei erstellt, die von der Anwendung Fläche, die möglicherweise [kostenlos heruntergeladen, auf der Website Fläche](http://www.mapeditor.org/). Das Dateiformat .tmx speichert die Informationen für die Kachel "-Zuordnungen. Ein Spiel in der Regel müssen eine .tmx-Datei für die einzelnen Ebene oder separate Bereiche.

Dieser Leitfaden konzentriert sich zur Verwendung von vorhandenen .tmx-Dateien in CocosSharp; allerdings weitere Tutorials finden Sie online, einschließlich [dieser Einführung in die Fläche-Editor für zeichenzuordnung](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838).

Müssen Sie Sie Entpacken die [Inhalt der Zip-Datei](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) für die Verwendung in unser Spiel. Die erste, was zu beachten ist, dass die Kachel-Zuordnungen verwenden beide die .tmx-Datei (dungeon.tmx) sowie einen oder mehr Bilddateien, die definieren, die Kachel "festlegen, dass Daten (dungeon_1.png). Muss das Spiel enthalten beide Dateien den .tmx zur Laufzeit zu laden, die beide des Projekts, also hinzufügen **Content** Ordner (die ist Bestandteil der **Assets** Ordner im Android-Projekte). Fügen Sie insbesondere die Dateien in einen Ordner namens **Tilemaps** innerhalb der **Content** Ordner:

![](tiled-images/image5.png "Fügen Sie die Dateien in einen Ordner namens Tilemaps im Ordner \"Content\" hinzu.")

Die **dungeon.tmx** Datei kann jetzt geladen werden, zur Laufzeit in einem `CCTileMap` Objekt. Ändern Sie als Nächstes die `GameLayer` (oder entsprechende Stammcontainerobjekt) enthält eine `CCTileMap` Instanz und fügen Sie es als untergeordnetes Element:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

Beim Ausführen des Spiels entnehmen wir werden die Kachel "-Zuordnung in der unteren linken Ecke des Bildschirms angezeigt:

![](tiled-images/image6.png "Wenn das Spiel ausgeführt wird, wird die Kachel \"-Zuordnung in der unteren linken Ecke des Bildschirms angezeigt")


## <a name="considerations-for-rendering-pixel-art"></a>Überlegungen zum Rendern der Pixel-Grafik

Pixel-Kunst, im Kontext der Video-Spieleentwicklung, bezieht sich auf 2D visual Kunst, die in der Regel von Hand erstellt und ist häufig mit niedriger Auflösung. Pixel-Grafiken kann restriktiv sein zu erstellen, damit Pixel Art Kachel häufig mit niedriger Auflösung Kacheln, z. B. 16 oder 32 Pixelbreite und Höhe umfassen, recht viel Zeit. Wenn zur Laufzeit nicht skaliert wird, ist Pixel häufig zu klein für die meisten modernen Smartphones und Tablets.

Wir können die angezeigten Dimensionen in des Spiels GameAppDelegate.cs-Datei, in denen fügen wir einen Aufruf von anpassen `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

Weitere Informationen zu `CCSceneResolutionPolicy`, finden Sie in unserem Handbuch auf [behandeln Auflösungen in CocosSharp](~/graphics-games/cocossharp/resolutions.md).

Wenn wir das Spiel jetzt ausführen, sehen wir der Spiel dauert, um den gesamten Bildschirm unser Gerät:

![](tiled-images/image7.png "Der Spiel dauert, um den gesamten Bildschirm des Geräts")

Zum Schluss möchten wir Antialiasing auf die Kachel "-Karte zu deaktivieren. Die `Antialiased` Eigenschaft gilt eine verschwimmenden Auswirkung beim Rendern von Objekten der vergrößert werden. Antialiasing kann zur Reduzierung der pixelig-Darstellung der grafischen Objekte nützlich sein, aber möglicherweise auch eine eigene Renderingartefakte eingeführt. Antialiasing wird insbesondere den Inhalt der einzelnen Kacheln verwischt. Allerdings sind die Ränder der einzelnen Kacheln nicht verwischt, wodurch die einzelnen Kacheln, die statt mit angrenzenden Kacheln blending abheben. Wir sollten auch Beachten Sie, dass die Pixel Art spielen häufig ihre verpixelt dargestellt aussehen verwalten beibehalten einer *retro* fühlen.

Legen Sie `Antialiased` zu `false` nach dem Erstellen der `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

Jetzt wird die Kachel "-Karte nicht unscharf angezeigt:

![](tiled-images/image8.png "Jetzt wird die Kachel \"-Zuordnung nicht unscharf angezeigt")


## <a name="using-tile-properties-at-runtime"></a>Mithilfe der kacheleigenschaften zur Laufzeit

Bisher haben wir eine `CCTileMap` Laden einer Datei .tmx sowie die Anzeige, wir haben aber keine Möglichkeit zur Interaktion. Insbesondere benötigen bestimmte Kacheln (z. B. unsere Schatzkiste Kasten) benutzerdefinierten Logik. Wir werden beim Erkennen der benutzerdefinierten kacheleigenschaften und verschiedene Möglichkeiten zum Reagieren auf diese Eigenschaften, die einmal zur Laufzeit identifiziert durchlaufen.

Bevor wir keinen Code schreiben, müssen wir unsere Kachel Map über Fläche Eigenschaften hinzu. Zu diesem Zweck öffnen Sie die dungeon.tmx-Datei in das Programm Fläche aus. Achten Sie darauf, dass Sie die Datei der verwendet wird, in das Spielprojekt öffnen.

Nachdem geöffnet ist, wählen Sie die wahre Schatzkiste Kasten, in der Kachel festgelegt werden, um seine Eigenschaften anzuzeigen:

![](tiled-images/image9.png "Wählen Sie nach geöffnet ist, die die wahre Schatzkiste Kasten auf der Kachel festgelegt werden, um seine Eigenschaften anzuzeigen.")

Wenn die wahre Schatzkiste Kasten Eigenschaften nicht angezeigt werden, mit der rechten Maustaste auf die wahre Schatzkiste Kasten, und wählen **Kacheleigenschaften**:

![](tiled-images/image10.png "Wenn die wahre Schatzkiste Kasten Eigenschaften nicht angezeigt werden, mit der rechten Maustaste auf die wahre Schatzkiste Kasten, und wählen Sie die Kacheleigenschaften")

Gekachelte Eigenschaften werden mit einem Namen und einen Wert implementiert. Um eine Eigenschaft hinzuzufügen, klicken Sie auf die **+** Schaltfläche, geben Sie den Namen **IsTreasure**, klicken Sie auf **OK**, dann geben Sie den Wert **"true"**: 

![](tiled-images/image11.png "Um eine Eigenschaft hinzuzufügen, klicken Sie auf die Schaltfläche \", geben Sie den Namen IsTreasure, klicken Sie auf OK, und geben Sie dann den Wert true")

Vergessen Sie nicht zum Speichern der .tmx-Datei, nach dem Ändern der Eigenschaften.

Abschließend fügen wir Code für unsere neu hinzugefügte Eigenschaft zu suchen. Schleife wird durch die einzelnen `CCTileMapLayer` (Karte hat 2 Ebenen), klicken Sie dann über die einzelnen Zeilen und Spalten für alle Kacheln suchen, die die `IsTreasure` Eigenschaft:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

Sollten erläutert den Umgang mit wahrer Kacheln Großteil des Codes ist selbsterklärend. In diesem Fall werden wir alle Kacheln entfernen, die als Schatztruhen identifiziert werden. Dies ist da Schatztruhen wahrscheinlich über benutzerdefinierten Code zur Laufzeit Auswirkungen Kollisionen und dem Spieler den Inhalt der wahre Schatzkiste, die beim Öffnen Erhalt des Awards benötigen. Darüber hinaus die wahre Schatzkiste, zu reagieren, müssen möglicherweise geöffnet (dessen visuelle Darstellung ändern) und hat möglicherweise die Logik für nur wenn alle auf dem Bildschirm angezeigt werden Feinde außer Kraft gesetzt wurden.

Das heißt, die wahre Schatzkiste Kasten profitieren davon, wird eine Entität, anstatt eine einfache Kachel in der `CCTileMap`. Weitere Informationen zum Spiel Entitäten, finden Sie unter den [Entitäten in CocosSharp Anleitung](~/graphics-games/cocossharp/entities.md).


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird beschrieben, wie .tmx mit erstellte Dateien Fläche in einer CocosSharp-Anwendung geladen wird. Sie erfahren, wie so ändern Sie die app-Lösung an niedrigerer Auflösung Pixel Art zu berücksichtigen, und wie Sie Kacheln von deren Eigenschaften, benutzerdefinierten Logik, wie das Erstellen von Instanzen der Entität ausführen.

## <a name="related-links"></a>Verwandte Links

- [Gekachelter Website](http://www.mapeditor.org/)
- [Inhalt der zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
