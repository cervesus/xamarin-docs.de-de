---
title: Verwenden der Modellklasse
description: Die Modell-Klasse vereinfacht die komplexe 3D-Objekten gegenüber der traditionellen Methode der Rendern 3D-Grafiken rendern. Modellobjekte werden in Inhaltsdateien gespeichert, sodass für die einfache Integration von Inhalt ohne benutzerdefinierten Code erstellt.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: e108bc6098d2c754428bf35e7312ebdc89459501
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783466"
---
# <a name="using-the-model-class"></a>Verwenden der Modellklasse

_Die Modell-Klasse vereinfacht die komplexe 3D-Objekten gegenüber der traditionellen Methode der Rendern 3D-Grafiken rendern. Modellobjekte werden in Inhaltsdateien gespeichert, sodass für die einfache Integration von Inhalt ohne benutzerdefinierten Code erstellt._

Die MonoGame-API umfasst eine `Model` Klasse, die verwendet werden können, zum Speichern von Daten geladen werden, aus einer Inhaltsdatei und Rendering ausführen. Modelldateien möglicherweise sehr einfach ist (z. B. eine solide farbige Dreieck) oder enthalten, die Informationen für komplexe Rendering, einschließlich Texturen und die Beleuchtung ausgeführt.

Diese exemplarische Vorgehensweise verwendet [ein 3D-Modell der Robot](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) und umfasst Folgendes:

- Starten Sie ein neues Spiels-Projekt
- Erstellen von XNBs für das Modell und dessen Struktur
- Die XNBs einschließen in das Spielprojekt
- Zeichnen ein 3D-Modell
- Zeichnen mehrere Modelle

Wenn Sie fertig sind, werden unsere Projekt wie folgt aussehen:

![Fertig gestellten Beispiel mit sechs Roboter](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>Erstellen ein Spiels leeres Projekt

Wir benötigen ein Spiel Projekt zuerst aufgerufen MonoGame3D einrichten. Informationen zum Erstellen eines neuen MonoGame-Projekts finden Sie unter [in dieser exemplarischen Vorgehensweise zum Erstellen eines Cross Platform Monogame Projekts](~/graphics-games/monogame/introduction/part1.md).

Bevor Sie fortfahren sollen wir überprüfen, dass das Projekt geöffnet wird und ordnungsgemäß bereitgestellt. Einmal bereitgestellten wir einen blauen Bildschirm sollte angezeigt werden:

![Leere blauen game-Bildschirm](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>Die XNBs einschließen in das Spielprojekt

Das Dateiformat .xnb ist eine standard-Erweiterung für den integrierten Inhalt (Inhalte mithilfe von erstellt wurden die [MonoGame Pipelinetool](http://www.monogame.net/documentation/?page=Pipeline)). Alle integrierter Inhalt verfügt über eine Quelldatei (also eine Datei .fbx bei unserem Modell) und eine Zieldatei (.xnb-Datei). Das Format .fbx ist ein gängiges 3D-Modell-Format die z. B. von Anwendungen erstellt werden können [Maya](http://www.autodesk.com/products/maya/overview) und [Blender](http://www.blender.org/). 

Die `Model` Klasse durch eine .xnb-Datei von einem Datenträger, die 3D Geometriedaten enthält laden konstruiert werden kann.   Diese .xnb-Datei wird durch ein Content-Projekt erstellt. Monogame Vorlagen umfassen ein Content Projekt (mit der Erweiterung .mgcp) automatisch in unserer Ordners "Content". Eine ausführliche Erläuterung auf das pipelinetool MonoGame, finden Sie unter der [Content Pipeline Handbuch](~/graphics-games/cocossharp/content-pipeline/index.md).

Tool für dieses Handbuch überspringen wir müssen mithilfe der MonoGame-Pipeline und Verwenden der. XNB-Dateien enthalten. Beachten Sie, dass die. XNB Dateien plattformspezifischen unterscheiden Sie daher sicher, dass den richtigen Satz von Dateien XNB für je Plattform zu verwenden, mit dem Sie arbeiten.

Wir müssen Entzippen Sie die [Content.zip Datei](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) , damit wir unsere Spiele enthaltenen .xnb Dateien verwenden können. Wenn auf einem Android-Projekt arbeiten, rechtsklicken Sie auf die **Bestand** Ordner in der **WalkingGame.Android** Projekt. Wenn auf einem iOS-Projekt arbeiten, rechtsklicken Sie auf die **WalkingGame.iOS** Projekt. Wählen Sie **hinzufügen -> Hinzufügen von Dateien...**  , und wählen Sie beide .xnb-Dateien in den Ordner für die Plattform, auf dem Sie arbeiten.

Die beiden Dateien sollte Teil unserer Projekts aussehen:

![Projektmappen-Explorer Ordners "Content" mit Xnb-Dateien](part1-images/xnbsinxs.png)

Visual Studio für Mac kann nicht automatisch den Buildvorgang für die neu hinzugefügten XNBs festlegen. IOS, mit der rechten Maustaste auf jede der Dateien, und wählen **Buildaktion -> BundleResource**. Android-Geräten mit der rechten Maustaste auf jede der Dateien, und wählen **Buildaktion -> AndroidAsset**.

## <a name="rendering-a-3d-model"></a>Rendern eines 3D-Modells

Der letzte Schritt notwendig, um das Modell auf dem Bildschirm sehen, ist das Laden und zeichnen Code hinzu. Insbesondere müssen wir die folgenden Aktionen durchgeführt werden:

- Definieren einer `Model` -Instanz in unserer `Game1` Klasse
- Beim Laden der `Model` -Instanz im `Game1.LoadContent`
- Zeichnen der `Model` -Instanz im `Game1.Draw`

Ersetzen Sie die `Game1.cs` -Codedatei (befindet sich der **WalkingGame** PCL) mit den folgenden:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

Wenn wir diesen Code ausführen, sehen das Modell auf dem Bildschirm:

![Modell, die auf dem Bildschirm angezeigten](part1-images/image8.png "Wenn dieser Code ausgeführt wird, das Modell erscheint auf dem Bildschirm")

### <a name="model-class"></a>Modellklasse

Die `Model` Klasse ist die Hauptklasse für das 3D-Rendering von Inhaltsdateien (z. B. .fbx-Dateien) durchführen. Er enthält alle Informationen, die für das Rendering, einschließlich der 3D Geometrie, die Verweise Textur erforderlich und `BasicEffect` -Instanzen, die Positionierung, Beleuchtung und Kamera Werte zu steuern.

Die `Model` Klasse selbst direkt keine Variablen zum Positionieren, da eine einzelne Modellinstanz an mehreren Speicherorten gerendert werden kann, wie wir später in diesem Handbuch zeige.

Jede `Model` besteht aus einem oder mehreren `ModelMesh` -Instanzen, die durch verfügbar gemacht werden die `Meshes` Eigenschaft. Obwohl wir gehen möglicherweise eine `Model` als einzelne Spiel Objekt (z. B. einem Roboter oder ein Auto) jedes `ModelMesh` gezeichnet werden können, mit anderen `BasicEffect` Werte. Z. B. einzelne Netz Teile der Abschnitte von einem Roboter oder Räder auf Auto darstellen können, und wir möglicherweise weisen die `BasicEffect` Werte dem Drehfeld Räder oder die Abschnitte zu verschieben. 

### <a name="basiceffect-class"></a>BasicEffect-Klasse

Die `BasicEffect` Klasse enthält Eigenschaften zum Steuern von Renderingoptionen. Die erste Änderung, die wir nehmen an der `BasicEffect` besteht im Aufrufen der `EnableDefaultLighting` Methode. Wie der Name schon sagt, wird dadurch Standard-Beleuchtung ist sehr praktisch, wenn Sie überprüfen, ob eine `Model` Spiels wie erwartet angezeigt. Wenn wir Auskommentieren der `EnableDefaultLighting` aufzurufen, dann sehen Sie das Modell nur auf dessen Struktur, aber keine Schattierung oder zusätzlicher spiegelnder Leuchten gerendert werden:

```csharp
//effect.EnableDefaultLighting ();
```

![Das Modell nur auf dessen Struktur, aber keine Schattierung oder zusätzlicher spiegelnder Leuchten gerendert](part1-images/image9.png "das Modell nur auf dessen Struktur, aber keine Schattierung oder zusätzlicher spiegelnder Leuchten gerendert")

Die `World` Eigenschaft kann verwendet werden, um die Position, Drehung und Skalierung des Modells anpassen. Der Code oben verwendet die `Matrix.Identity` Wert bedeutet, dass die `Model` im Spiel genau wie angegeben in der Datei .fbx gerendert wird. Wir müssen Matrizen und 3D Koordinaten in ausführlicher abdecken [Teil 3](~/graphics-games/monogame/3d/part3.md), aber ändern wir als Beispiel die Position des der `Model` durch Ändern der `World` Eigenschaft wie folgt:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Dieser Code ergibt sich das Objekt durch 3 globalen Einheiten verschoben werden:

![Dieser Code führt zu das Objekt wird nach oben verschoben, um 3 Einheiten von World](part1-images/image10.png "dieser Code führt zu das Objekt durch 3 globalen Einheiten verschoben wird")

Die letzten zwei Eigenschaften, die zugewiesen, auf die `BasicEffect` sind `View` und `Projection`. Wir müssen in 3D Kameras abdecken [Teil 3](~/graphics-games/monogame/3d/part3.md), aber als Beispiel können wir die Kameraposition ändern, indem Sie die Änderung der lokale `cameraPosition` Variablen:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Sehen wir die Kamera wurde weiter hinten verschoben führt die `Model` kleinere aufgrund der Perspektive angezeigt:

![Die Kamera weiteren zurück, was im Modell angezeigten kleinere aufgrund der Perspektive verschoben wurde](part1-images/image11.png "die Kamera weiteren zurück, was im Modell angezeigten kleinere aufgrund der Perspektive verschoben wurde")

## <a name="rendering-multiple-models"></a>Rendern von mehreren Modellen

Wie bereits erwähnt, ein einzelnes `Model` mehrmals gezeichnet werden können. Um dies zu vereinfachen werden wir verschieben den `Model` Zeichnen von Code in eine eigene Methode, die den gewünschten akzeptiert `Model` Position als Parameter. Einmal fertig sind, unsere `Draw` und `DrawModel` Methoden sieht wie folgt:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;
            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;
            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

Daraus ergibt sich der Robot Modell sechs Mal gezeichnet werden:

![Dies führt zu des Roboters Modell gezeichnete sechs Mal](part1-images/image1.png "dadurch des Roboters Modell gezeichnete sechs Mal")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise eingeführt des MonoGame `Model` Klasse. Konvertieren einer .fbx-Datei in eine .xnb behandelt die kann wiederum in geladen werden eine `Model` Klasse. Außerdem wird gezeigt, wie Änderungen an einer `BasicEffect` Instanz beeinträchtigt `Model` zeichnen.

## <a name="related-links"></a>Verwandte Links

- [Referenz zum MonoGame-Objektmodell](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Abgeschlossene Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
