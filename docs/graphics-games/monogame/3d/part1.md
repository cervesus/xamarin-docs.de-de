---
title: Verwenden der Modellklasse
description: Die Model-Klasse vereinfacht die komplexe 3D-Objekte gegenüber der traditionellen Methode der, Rendern von 3D-Grafiken zu rendern. Modellobjekte werden von Inhaltsdateien, ermöglicht eine einfache Integration von Inhalten ohne benutzerdefinierten Code erstellt.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4a72effc85657b4722b17eae486e81db5992a1da
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832258"
---
# <a name="using-the-model-class"></a>Verwenden der Modellklasse

_Die Model-Klasse vereinfacht die komplexe 3D-Objekte gegenüber der traditionellen Methode der, Rendern von 3D-Grafiken zu rendern. Modellobjekte werden von Inhaltsdateien, ermöglicht eine einfache Integration von Inhalten ohne benutzerdefinierten Code erstellt._

Die MonoGame-API umfasst eine `Model` geladen Klasse, die zum Speichern von Daten verwendet werden kann, aus einer Inhaltsdatei und zum ausgeben. Model-Dateien können sehr einfach ist (z. B. ein solid farbigen Dreieck) sein oder zu komplexe rendern, einschließlich von Texturen und die Beleuchtung enthalten.

Diese exemplarische Vorgehensweise verwendet [ein 3D-Modell ein Roboter](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) und umfasst Folgendes:

- Starten Sie ein neues Spiels-Projekt
- Erstellen von XNBs für das Modell und dessen Struktur
- Einschließlich der XNBs in das Spielprojekt
- Zeichnen ein 3D-Modell
- Zeichnen mehrere Modelle

Wenn Sie fertig sind, wird das Projekt wie folgt aussehen:

![Mit sechs Roboter abgeschlossenen](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>Erstellen ein leeres Projekt für die Spiele

Wir benötigen, um ein Spiel mit dem Namen zuerst MonoGame3D einzurichten. Informationen zum Erstellen eines neuen MonoGame-Projekts finden Sie unter [in dieser exemplarischen Vorgehensweise zum Erstellen eines Cross Platform Monogame-Projekts](~/graphics-games/monogame/introduction/part1.md).

Bevor wir fortfahren sollte überprüfen, ob das Projekt geöffnet wird und ordnungsgemäß bereitgestellt. Einmal bereitgestellt, sehen Sie einen blauen Bildschirm:

![Leere Spiel Bluescreen](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>Einschließlich der XNBs in das Spielprojekt

Das Dateiformat .xnb ist eine standard-Erweiterung für den erstellten Inhalt (Inhalt, der von der Erstellung der [MonoGame Pipelinetool](http://www.monogame.net/documentation/?page=Pipeline)). Alle erstellten Inhalte verfügt über eine Quelldatei (die eine Datei .fbx im Fall von unserem Modell) und eine Zieldatei (.xnb-Datei). Das .fbx-Format ist ein gängiges 3D-Modell-Format, die von Anwendungen wie z. B. erstellt werden kann [Maya](http://www.autodesk.com/products/maya/overview) und [Blender](http://www.blender.org/). 

Die `Model` Klasse konstruiert werden kann, laden Sie eine .xnb-Datei von einem Datenträger, die 3D-Geometrie Daten enthält.   Diese .xnb-Datei wird durch einen Content-Projekt erstellt. Monogame-Vorlagen enthalten einen Content-Projekt (mit der Erweiterung .mgcp) automatisch in unserer Ordner "Content". Ausführliche Informationen zum Tool MonoGame-Pipeline, finden Sie unter den [Pipeline für Bildinhalte Handbuch](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md).

In diesem Handbuch übersprungen wird, über die MonoGame Pipeline mit Tools und verwendet die. Hier enthaltenen XNB-Dateien. Beachten Sie, dass die. XNB Dateien unterscheiden sich pro Plattform durchgeführt, seien Sie sicher, dass den richtigen Satz von XNB-Dateien für den beliebigen Plattform verwenden, mit denen Sie arbeiten werden.

Wir werden Entzippen der [Content.zip Datei](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) , damit wir die Dateien enthaltenen .xnb in unser Spiel verwenden können. Wenn auf einem Android-Projekt arbeiten, rechtsklicken Sie auf die **Assets** Ordner in der **WalkingGame.Android** Projekt. Wenn ein iOS-Projekt arbeiten, rechtsklicken Sie auf die **WalkingGame.iOS** Projekt. Wählen Sie **fügen -> Dateien hinzufügen...**  , und wählen Sie beide .xnb-Dateien im Ordner für die Plattform Sie arbeiten.

Die beiden Dateien sollte Teil unseres Projekts aussehen:

![Projektmappen-Explorer-Inhaltsordner Xnb-Dateien](part1-images/xnbsinxs.png)

Visual Studio für Mac kann nicht automatisch den Buildvorgang für neu hinzugefügte XNBs festgelegt werden. IOS, mit der rechten Maustaste auf jede der Dateien, und wählen **BundleResource-Build Action >** . Android, mit der rechten Maustaste auf jede der Dateien, und wählen **AndroidAsset-Build Action >** .

## <a name="rendering-a-3d-model"></a>Rendern eines 3D-Modells

Der letzte Schritt erforderlich, um das Modell auf dem Bildschirm anzuzeigen, ist das Laden und zeichnen Code hinzufügen. Insbesondere werden wir die folgenden Aktionen durchgeführt werden:

- Definieren einer `Model` -Instanz in unserer `Game1` Klasse
- Beim Laden der `Model` -Instanz im `Game1.LoadContent`
- Zeichnen der `Model` -Instanz im `Game1.Draw`

Ersetzen Sie die `Game1.cs` -Codedatei (befindet sich in der **WalkingGame** PCL) durch Folgendes:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

Wenn wir diesen Code ausführen wird wir das Modell auf dem Bildschirm angezeigt:

![Modell, die auf dem Bildschirm angezeigten](part1-images/image8.png "Wenn dieser Code ausgeführt wird, das Modell erscheint auf dem Bildschirm")

### <a name="model-class"></a>Model-Klasse

Die `Model` Klasse ist die Hauptklasse für die Durchführung von 3D-Rendering von Inhaltsdateien (z. B. .fbx). Er enthält alle für das Rendering, die 3D-Geometrie, die Textur-Verweise, einschließlich der erforderlichen Informationen und `BasicEffect` -Instanzen, die Positionierung, Beleuchtung und Kamera-Werte zu steuern.

Die `Model` Klasse selbst direkt keine Variablen für die Positionierung von da an mehreren Standorten eine einzelnen Instanz gerendert werden kann, wie wir später in diesem Handbuch zeigen werde.

Jede `Model` besteht aus einem oder mehreren `ModelMesh` -Instanzen, die über verfügbar gemacht werden die `Meshes` Eigenschaft. Obwohl wir betrachten kann eine `Model` als ein einzelnes Spiel Objekt (z. B. ein Roboter oder ein Auto), jede `ModelMesh` gezeichnet werden können, mit anderen `BasicEffect` Werte. Beispielsweise einzelne Gitter Teile können die "Legs" von einem Roboter oder der Räder auf ein Fahrzeug darstellen, und weisen wir Sie möglicherweise die `BasicEffect` verschieben Sie die Werte für das Drehfeld Wheels oder die Beine stellen. 

### <a name="basiceffect-class"></a>BasicEffect-Klasse

Die `BasicEffect` Klasse stellt Eigenschaften zum Steuern der Renderingoptionen. Die erste Änderung, die wir, um treffen die `BasicEffect` besteht im Aufrufen der `EnableDefaultLighting` Methode. Wie der Name schon sagt, wird dadurch standardmäßig Beleuchtung, dies ist sehr nützlich zum Überprüfen, die eine `Model` im Spiel wie erwartet angezeigt. Wenn wir Auskommentieren der `EnableDefaultLighting` aufrufen, sehen wir das Modell, das gerendert wird, mit nur dessen Struktur, jedoch mit kein schattierungs- oder glänzende Leuchten:

```csharp
//effect.EnableDefaultLighting ();
```

![Das Modell, das gerendert wird, mit nur dessen Struktur, jedoch mit kein schattierungs- oder glänzende Leuchten](part1-images/image9.png "des Modells, das gerendert wird, mit nur dessen Struktur, jedoch mit kein schattierungs- oder glänzende Leuchten")

Die `World` Eigenschaft kann verwendet werden, um die Position, Drehung und Skalierung des Modells anpassen. Der Code oben verwendet die `Matrix.Identity` -Wert, der das bedeutet, dass die `Model` im Spiel genau wie angegeben in der Datei .fbx rendert. Wir müssen Matrizen und 3D-Koordinaten ausführlicher behandelt [Teil 3](~/graphics-games/monogame/3d/part3.md), aber als Beispiel können wir die Position des Ändern der `Model` durch Ändern der `World` -Eigenschaft wie folgt:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Dieser Code löst das Objekt, das nach oben verschoben wird, von 3 Welteinheiten:

![Dieser Code führt zu das Objekt, das nach oben verschoben wird, von 3 Welteinheiten der ganzen](part1-images/image10.png "dieser Code führt zu das Objekt, das nach oben verschoben wird, von 3 Welteinheiten der ganzen")

Die letzten zwei Eigenschaften, die zugewiesen werden, auf die `BasicEffect` sind `View` und `Projection`. Wir werden abgedeckt 3D Kameras in [Teil 3](~/graphics-games/monogame/3d/part3.md), aber als Beispiel ändern wir die Position der Kamera durch ändern den lokalen `cameraPosition` Variable:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Wie Sie sehen die Kamera wurde weiter zurück, wodurch die `Model` kleinere aufgrund der Perspektive angezeigt:

![Die Kamera wurde weiter zurück, wodurch das Modell kleiner aufgrund der Perspektive angezeigt](part1-images/image11.png "die Kamera wurde weiter zurück, wodurch das Modell kleiner aufgrund der Perspektive angezeigt")

## <a name="rendering-multiple-models"></a>Rendern von mehreren Modellen

Wie bereits erwähnt, ein einzelnes `Model` mehrmals gezeichnet werden können. Um diese Aufgabe erleichtern werden wir verschieben den `Model` Zeichencode in eine eigene Methode, die die gewünschte akzeptiert `Model` Position als Parameter. Nach Abschluss des Vorgangs unsere `Draw` und `DrawModel` Methoden werden wie folgt aussehen:


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

Dadurch wird der Roboter Modell sechs Mal gezeichnet werden:

![Dadurch wird der Roboter Modell gezeichnet werden bis zu sechs Mal](part1-images/image1.png "Dadurch wird der Roboter Modell gezeichnet werden bis zu sechs Mal")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde eingeführt, die MonoGame `Model` Klasse. Sie erfahren, konvertieren eine .fbx-Datei in eine .xnb, die wiederum in geladen werden eine `Model` Klasse. Außerdem wird gezeigt, wie Änderungen an einer `BasicEffect` Instanz auswirken kann `Model` zeichnen.

## <a name="related-links"></a>Verwandte Links

- [Referenz zum MonoGame-Objektmodell](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Abgeschlossenes Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
