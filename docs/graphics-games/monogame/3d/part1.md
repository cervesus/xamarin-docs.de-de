---
title: Verwenden der Model-Klasse
description: Die Modell Klasse vereinfacht das Rendern komplexer 3D-Objekte im Vergleich zu der herkömmlichen Methode zum Rendern von 3D-Grafiken erheblich. Modell Objekte werden aus Inhalts Dateien erstellt und ermöglichen so eine einfache Integration von Inhalten ohne benutzerdefinierten Code.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 35df6e4ca799a875bcd7db50adbc7a300460885c
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680967"
---
# <a name="using-the-model-class"></a>Verwenden der Model-Klasse

_Die Modell Klasse vereinfacht das Rendern komplexer 3D-Objekte im Vergleich zu der herkömmlichen Methode zum Rendern von 3D-Grafiken erheblich. Modell Objekte werden aus Inhalts Dateien erstellt und ermöglichen so eine einfache Integration von Inhalten ohne benutzerdefinierten Code._

Die monogame-API enthält `Model` eine-Klasse, die zum Speichern von Daten verwendet werden kann, die aus einer Inhalts Datei geladen wurden, und um Rendering auszuführen. Modelldateien können sehr einfach sein (z. b. ein farbig farbiges Dreieck) oder Informationen für das komplexe Rendering enthalten, einschließlich der Texturierung und der Beleuchtung.

In dieser exemplarischen Vorgehensweise wird [ein 3D-Modell eines Roboters](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) verwendet und Folgendes behandelt:

- Starten eines neuen Spiel Projekts
- Erstellen von xnsb für das Modell und seine Textur
- Einschließen der xnsb im Spiel Projekt
- Zeichnen eines 3D-Modells
- Zeichnen von mehreren Modellen

Wenn Sie fertig sind, wird das Projekt wie folgt angezeigt:

![Fertiggestelltes Beispiel mit sechs Robotern](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>Erstellen eines leeren Spiel Projekts

Wir müssen zuerst ein Spiel Projekt mit dem Namen MonoGame3D einrichten. Weitere Informationen zum Erstellen eines neuen monogame-Projekts finden Sie in dieser exemplarischen Vorgehensweise [zum Erstellen eines plattformübergreifenden monogame-Projekts](~/graphics-games/monogame/introduction/part1.md).

Bevor Sie fortfahren, sollten Sie überprüfen, ob das Projekt ordnungsgemäß geöffnet und bereitgestellt wird. Nach der Bereitstellung sollte ein leerer blauer Bildschirm angezeigt werden:

![Leerer blauer Spielbildschirm](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>Einschließen der xnsb im Spiel Projekt

Das xnb-Dateiformat ist eine Standard Erweiterung für erstellten Inhalt (Inhalt, der vom [monogame-Pipeline Tool](http://www.monogame.net/documentation/?page=Pipeline)erstellt wurde). Alle erstellten Inhalte verfügen über eine Quelldatei (bei unserem Modell eine FBX-Datei) und eine Zieldatei (xnb-Datei). Das. f BX-Format ist ein gängiges 3D-Modell Format, das von Anwendungen wie [Maya](http://www.autodesk.com/products/maya/overview) und [Blender](http://www.blender.org/)erstellt werden kann. 

Die `Model` -Klasse kann erstellt werden, indem eine xnb-Datei von einem Datenträger geladen wird, der 3D-Geometriedaten enthält.   Diese xnb-Datei wird durch ein Inhalts Projekt erstellt. Monogame-Vorlagen enthalten automatisch ein Inhalts Projekt (mit der Erweiterung MGCP) in unserem Inhalts Ordner. Eine ausführliche Erläuterung zum monogame-Pipeline Tool finden Sie im [Leitfaden zur Inhalts Pipeline](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md).

In dieser Anleitung überspringen wir die Verwendung des monogame-Pipeline Tools und verwenden den. Hier sind die xnb-Dateien enthalten. Beachten Sie, dass die. Xnb-Dateien sind je nach Plattform unterschiedlich. Achten Sie daher darauf, dass Sie den richtigen Satz von xnb-Dateien für die Plattform verwenden, mit der Sie arbeiten.

Die [Datei "Content. zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) " wird entzippt, damit wir die enthaltenen xnb-Dateien im Spiel verwenden können. Wenn Sie an einem Android-Projekt arbeiten, klicken Sie mit der rechten Maustaste auf den Ordner **Assets** im Projekt **walkinggame. Android** . Wenn Sie an einem IOS-Projekt arbeiten, klicken Sie mit der rechten Maustaste auf das Projekt " **walkinggame. IOS** ". Wählen Sie **Add-> Dateien hinzufügen...** aus, und wählen Sie im Ordner für die Plattform, an der Sie arbeiten, beide xnb-Dateien aus.

Die beiden Dateien sollten jetzt Teil des Projekts sein:

![Inhalt des Projektmappen-Explorers mit xnb-Dateien](part1-images/xnbsinxs.png)

Visual Studio für Mac kann die Buildaktion für neu hinzugefügte xnsb nicht automatisch festlegen. Klicken Sie für IOS mit der rechten Maustaste auf jede der Dateien, und wählen Sie **Aktion erstellen-> bundleresource**aus. Klicken Sie für Android mit der rechten Maustaste auf jede der Dateien, und wählen Sie **Build Action-> androidasset**aus.

## <a name="rendering-a-3d-model"></a>Rendern eines 3D-Modells

Der letzte Schritt, der zum Anzeigen des Modells auf dem Bildschirm erforderlich ist, besteht darin, den Lade-und Zeichnungs Code hinzuzufügen. Insbesondere gehen wir wie folgt vor:

- Definieren einer `Model` -Instanz in `Game1` unserer Klasse
- Laden der `Model` Instanz in`Game1.LoadContent`
- Zeichnen der `Model` Instanz in`Game1.Draw`

Ersetzen Sie `Game1.cs` die Codedatei (die sich in der " **walkinggame** PCL" befindet) durch Folgendes:

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

Wenn wir diesen Code ausführen, wird das Modell auf dem Bildschirm angezeigt:

![Auf dem Bildschirm angezeigtes Modell](part1-images/image8.png "Wenn dieser Code ausgeführt wird, wird das Modell auf dem Bildschirm angezeigt") .

### <a name="model-class"></a>Modell Klasse

Die `Model` -Klasse ist die Basisklasse für das 3D-Rendering aus Inhalts Dateien (z. b. b. b.). Sie enthält alle Informationen, die für das Rendering erforderlich sind, einschließlich der 3D-Geometrie, der Textur `BasicEffect` Verweise und Instanzen, die Positions-, Beleuchtungs-und Kamera Werte steuern.

Die `Model` Klasse selbst verfügt nicht direkt über Variablen zur Positionierung, da eine einzelne Modell Instanz an mehreren Speicherorten gerendert werden kann, wie weiter unten in diesem Handbuch gezeigt wird.

Jede `Model` besteht aus einer oder mehreren `ModelMesh` -Instanzen, die über die `Meshes` -Eigenschaft verfügbar gemacht werden. Obwohl ein `Model` einzelnes Spielobjekt (z. b. ein Roboter oder ein Fahrzeug) als einzelnes Spielobjekt betrachtet `ModelMesh` werden kann, kann jedes `BasicEffect` mit unterschiedlichen Werten gezeichnet werden. Einzelne Gitter Teile können z. b. die Beine eines Roboters oder der Räder eines Autos darstellen, und wir können die `BasicEffect` Werte zuweisen, um die Räder zu drehen oder die Beine zu verschieben. 

### <a name="basiceffect-class"></a>Basiceffect-Klasse

Die `BasicEffect` -Klasse stellt Eigenschaften zum Steuern von Renderingoptionen bereit. Die erste Änderung, die wir an `BasicEffect` der vornehmen, besteht `EnableDefaultLighting` darin, die-Methode aufzurufen. Wie der Name schon sagt, wird eine Standardbeleuchtung ermöglicht. Dies ist sehr praktisch, um `Model` zu überprüfen, ob ein im Spiel erwartungsgemäß angezeigt wird. Wenn wir den `EnableDefaultLighting` -Befehl auskommentieren, sehen wir, dass das Modell mit nur seiner Textur gerendert wird, aber ohne Schattierung oder Glanzlicht:

```csharp
//effect.EnableDefaultLighting ();
```

![Das Modell, das nur mit seiner Textur gerendert wird, jedoch ohne Schattierung oder Glanz Glanz](part1-images/image9.png "Das Modell, das nur mit seiner Textur gerendert wird, jedoch ohne Schattierung oder Glanz Glanz")

Die `World` -Eigenschaft kann verwendet werden, um die Position, die Drehung und die Skalierung des Modells anzupassen. Der obige Code verwendet den `Matrix.Identity` -Wert, d. h `Model` ., der wird im Spiel genau so dargestellt, wie er in der. f-Datei angegeben ist. Matrizen und 3D-Koordinaten werden in [Teil 3](~/graphics-games/monogame/3d/part3.md)ausführlicher behandelt, aber als Beispiel können wir die Position von `Model` ändern, indem wir die `World` -Eigenschaft wie folgt ändern:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Dieser Code führt dazu, dass das Objekt um drei World-Einheiten nach oben verschoben wird:

![Dieser Code führt dazu, dass das Objekt um 3 World-Einheiten nach oben verschoben wird] . (part1-images/image10.png "Dieser Code führt dazu, dass das Objekt um 3 World-Einheiten nach oben verschoben wird") .

Die letzten zwei Eigenschaften, die auf `BasicEffect` dem `View` zugewiesen `Projection`werden, sind und. Wir befassen uns mit 3D-Kameras in [Teil 3](~/graphics-games/monogame/3d/part3.md), aber als Beispiel können wir die Position der Kamera ändern, indem wir die lokale `cameraPosition` Variable ändern:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Wir sehen, dass die Kamera noch weiter bewegt wurde, was dazu `Model` führt, dass die-Kamera aufgrund der Perspektive kleiner erscheint:

![Die Kamera ist weiter zurückgerückt, was dazu führt, dass das Modell aufgrund der Perspektive kleiner erscheint] . (part1-images/image11.png "Die Kamera ist weiter zurückgerückt, was dazu führt, dass das Modell aufgrund der Perspektive kleiner erscheint") .

## <a name="rendering-multiple-models"></a>Rendern von mehreren Modellen

Wie bereits erwähnt, kann ein `Model` einzelner mehrmals gezeichnet werden. Um dies zu vereinfachen, verschieben wir den `Model` Zeichnungs Code in eine eigene Methode, die die gewünschte `Model` Position als Parameter annimmt. Nach Abschluss des Vorgangs `Draw` sehen `DrawModel` unsere-und-Methoden wie folgt aus:


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

Dies führt dazu, dass das Robotermodell sechs Mal gezeichnet wird:

![Dies führt dazu, dass das Robotermodell sechs Mal gezeichnet wird] . (part1-images/image1.png "Dies führt dazu, dass das Robotermodell sechs Mal gezeichnet wird") .

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde `Model` die Klasse von monogame eingeführt. Sie behandelt das Umwandeln einer FBX-Datei in eine xnb-Datei, die wiederum in eine `Model` Klasse geladen werden kann. Außerdem wird gezeigt, wie sich Änderungen `BasicEffect` an einer- `Model` Instanz auf das Zeichnen auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Monogame-Modell Referenz](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Abgeschlossenes Projekt (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelrenderingmg/)
