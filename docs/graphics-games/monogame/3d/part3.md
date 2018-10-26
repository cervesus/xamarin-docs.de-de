---
title: 3D-Koordinaten in MonoGame
description: Grundlegendes zu den 3D-Koordinatensystem ist ein wichtiger Schritt bei der Entwicklung von 3D-Spiele. MonoGame bietet eine Reihe von Klassen für die Positionierung, ausrichten und Skalieren von Objekten im 3D-Raum.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: dc21228f1daa74a90ff8f0ea346bc01b109f0987
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107638"
---
# <a name="3d-coordinates-in-monogame"></a>3D-Koordinaten in MonoGame

_Grundlegendes zu den 3D-Koordinatensystem ist ein wichtiger Schritt bei der Entwicklung von 3D-Spiele. MonoGame bietet eine Reihe von Klassen für die Positionierung, ausrichten und Skalieren von Objekten im 3D-Raum._

Entwickeln 3D-Spiele erfordert einen Überblick über die Vorgehensweise für die Objekte in einem 3D-Koordinatensystem bearbeiten. In dieser exemplarischen Vorgehensweise wird das Bearbeiten der visuellen Objekte (insbesondere ein Modell) behandelt. Erstellen wir auf den Konzepten der Steuern ein Modell zum Erstellen einer 3D Kamera-Klasse.

Die vorgestellten Konzepte stammen lineare Algebra, aber werfen wir einen praktischen Ansatz, damit alle Benutzer ohne eine starke Math-Hintergrund diese Konzepte in ihre eigenen Spiele anwenden kann.

Wir werden in den folgenden Themen behandelt:

- Erstellen eines Projekts
- Erstellen einer Roboterentität
- Verschieben Sie die Roboterentität
- Matrizenmultiplikation
- Erstellen der Kamera-Entität
- Verschieben Sie die Kamera mit der Eingabe

Sobald Sie fertig sind, haben wir ein Projekt mit einem Roboter verschieben in einen Kreis und eine Kamera, die von Touch-Punkts gesteuert werden kann:

![](part3-images/image1.gif "Sobald Sie fertig sind, wird die app enthalten ein Projekt mit einem Roboter verschieben in einen Kreis und eine Kamera, die von Touch-Punkts gesteuert werden kann")


## <a name="creating-a-project"></a>Erstellen eines Projekts

In dieser exemplarischen Vorgehensweise konzentriert sich auf Objekte im 3D-Raum zu verschieben. Wir beginnen mit dem Projekt für renderingmodelle und Vertex-Arrays [finden Sie hier](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/). Nach dem Herunterladen, entpacken Sie aus, und öffnen Sie das Projekt aus, um sicherzustellen, dass er ausgeführt wird, und sehen Sie Folgendes:

![](part3-images/image2.png "Nach dem Herunterladen, extrahieren Sie und öffnen Sie das Projekt aus, um sicherzustellen, dass er ausgeführt wird und in dieser Ansicht angezeigt werden soll")


## <a name="creating-a-robot-entity"></a>Erstellen einer Roboterentität

Bevor wir beginnen, unsere Roboter zu verschieben, erstellen wir eine `Robot` Klasse, um die Logik für die Zeichnung und-Verschiebung enthalten. Spieleentwickler finden Sie unter diesem Kapselung von Logik und Daten als ein *Entität*.

Fügen Sie eine neue leere Klassendatei, die **MonoGame3D** Portable Class Library (nicht die plattformspezifische ModelAndVerts.Android). Nennen Sie sie **Roboter** , und klicken Sie auf **neu**:

![](part3-images/image3.png "Benennen Sie diese Roboter, und klicken Sie auf neu")

Ändern der `Robot` -Klasse wie folgt:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

Die `Robot` Code ist im Wesentlichen den gleichen Code in `Game1` zum Zeichnen einer `Model`. Eine Überprüfung auf `Model` laden und zeichnen, finden Sie unter [dieser Anleitung zum Arbeiten mit Modellen](~/graphics-games/monogame/3d/part1.md). Wir können jetzt alle Entfernen der `Model` laden und Rendern von Code aus `Game1`, und ersetzen es durch ein `Robot` Instanz:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Wenn wir den Code ausführen werden wir eine Szene mit nur einem Roboter haben das größtenteils unter dem Boden gezeichnet wird:

![](part3-images/image4.png "Wenn der Code nun ausgeführt wird, wird die app eine Szene mit nur einem Roboter anzuzeigen, größtenteils unter dem Boden gezeichnet wird")

## <a name="moving-the-robot"></a>Verschieben des Roboters

Nun, wir haben eine `Robot` -Klasse, können wir der Roboter Bewegung Logik hinzufügen. In diesem Fall erstellen wir einfach des Roboters in einem Kreis entsprechend der Uhrzeit der Spiele zu verschieben. Dies ist eine ziemlich unpraktisch Implementierung für einem richtigen Spiel, da es sich bei Eingabe reagieren ein Zeichen in der Regel oder künstliche Intelligenz, sondern bietet eine Umgebung für uns 3D Positionierung und Drehung zu untersuchen.

Die einzige Information, wir aus außerhalb von benötigen, den `Robot` Klasse ist die aktuelle Uhrzeit des Spiels. Wir fügen eine `Update` -Methode die dauert eine `GameTime` Parameter. Dies `GameTime` Parameter wird verwendet, um eine Variable des Winkels zu erhöhen, die wir verwenden werden, um die letzte Position für den Roboter zu bestimmen.

Zunächst fügen wir das Feld Winkel der `Robot` -Klasse der `model` Feld:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Nun wir diesen Wert in erhöhen können eine `Update` Funktion:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Wir müssen sicherstellen, dass die `Update` Methode wird aufgerufen, von `Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Natürlich an diesem Punkt das Feld Winkel hat keine Auswirkungen – wir müssen Code schreiben, um es zu verwenden. Ändern wir die `Draw` Methode, damit berechnen wir auf die ganzen Welt `Matrix` in einer dedizierten Methode: 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

Als Nächstes implementieren wir die `GetWorldMatrix` -Methode in der die `Robot` Klasse:

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Das Ergebnis der Ausführung dieses Codes führt zu dem Roboter, die in einem Kreis verschieben:

![](part3-images/image5.gif "Ausführen dieses Codes führt in der Roboter, die in einem Kreis verschieben")

## <a name="matrix-multiplication"></a>Matrizenmultiplikation

Der obige Code wird den Roboter durch das Erstellen einer `Matrix` in die `GetWorldMatrix` Methode. Die `Matrix` Struktur enthält 16 "float"-Werte die verwendet werden kann, um (Satz Position) zu übersetzen, drehen und skalieren (Größe festgelegt). Wenn weisen wir die `effect.World` -Eigenschaft, weisen wir den zugrunde liegenden Rendern System wie an eine position und Größe vertraut machen, was uns hier unten gezeichnet werden (eine `Model` oder einer Geometrie von Vertices). 

Glücklicherweise die `Matrix` Struktur umfasst eine Reihe von Methoden, die die Erstellung von allgemeinen Matrizen zu vereinfachen. Die erste verwendet, die im obigen Code ist `Matrix.CreateTranslation`. Der mathematische Begriff *Übersetzung* bezieht sich auf einen Vorgang, der Ergebnis in einem Punkt (oder in unserem Fall ein Modell) von einem Speicherort in einen anderen ohne weitere Änderung (z. B. drehen, oder Ändern der Größe) verschieben. Die Funktion erfordert einen X, Y und Z-Wert für die Übersetzung. Wenn wir unsere Szene von oben nach unten, zeigen unsere `CreateTranslation` -Methode (in der Isolation) führt die folgende:

![](part3-images/image6.png "Die Methode CreateTranslation isoliert führt diese Aktion")

Die zweite Matrix, die wir erstellen, wird eine Rotationsmatrix mithilfe der `CreateRotationZ` Matrix. Dies ist einer der drei Methoden, die verwendet werden kann, um Rotation zu erstellen:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Jede Methode erstellt eine Rotationsmatrix, indem Sie über eine angegebene Achse drehen. In diesem Fall sind wir die Z-Achse drehen "up" verweist. Die folgende können wie Axis-basierte vorstellbar funktioniert:

![](part3-images/image7.png "Dies kann helfen, wie Achsen-basierte vorstellbar funktioniert")

Wir verwenden zudem die `CreateRotationZ` Methode mit dem Winkel-Feld, damit im Laufe der Zeit aufgrund von erhöht unsere `Update` aufgerufenen Methode. Das Ergebnis ist, die `CreateRotationZ` Methode bewirkt, dass unsere Roboter, um den Ursprung orbit, mit der Zeit.

Die letzte Zeile des Codes kombiniert die beiden Matrizen in einem:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Dies wird als Matrizenmultiplikation, bezeichnet dies etwas anders als reguläre Multiplikation funktioniert. Die *kommutativ-Eigenschaft der Multiplikation* gibt an, dass die Reihenfolge der Zahlen in einen Multiplikationsvorgang das Ergebnis nicht ändert. 3 * 4 entspricht, also 4 * 3. Matrixmultiplikation unterscheidet sich insofern, dass es nicht kommutativ. Die obige Zeile kann, also als gelesen werden, "Gelten die TranslationMatrix, um das Modell zu verschieben, dann Drehen alles, was durch Anwenden der RotationMatrix". Wir konnten die Möglichkeit visualisieren Sie, dass die obige Zeile wie folgt die Position und Drehung auswirkt:

![](part3-images/image8.png "Die Möglichkeit, dass die obige Zeile der Position und Drehung wirkt sich auf eine Visualisierung-pf")

Um besser zu verstehen, wie die Reihenfolge der Matrixmultiplikation das Ergebnis auswirken kann, berücksichtigen Sie Folgendes, in denen die Matrixmultiplikation umgekehrt wird:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Der obige Code würde drehen das Modell direkt zunächst übersetzen:

![](part3-images/image9.png "Der obige Code würde drehen das Modell direkt zunächst übersetzen")

Wenn wir den Code mit invertierten Multiplikation ausführen, sehen wir, da die Drehung zuerst angewendet wird, es wirkt sich nur auf die Ausrichtung des Modells und die Position des Modells unverändert bleibt. Das heißt, dreht sich das Modell vorhanden:

![](part3-images/image10.gif "Das Modell wird eingerichtet")

## <a name="creating-the-camera-entity"></a>Erstellen der Kamera-Entität

Die `Camera` Entität enthält die gesamte Logik erforderlich, um Benutzereingaben basierenden Bewegung auszuführen und zu Eigenschaften, zum Zuweisen von Eigenschaften auf der `BasicEffect` Klasse.

Zuerst wir Aufnahmen statischen Kameras (keine Benutzereingaben basierenden Bewegung) zu implementieren und in unsere vorhandenen Projekt integrieren. Fügen Sie eine neue Klasse, die **MonoGame3D** Portable Class Library (das gleiche Projekt mit `Robot.cs`) und nennen Sie sie **Kamera**. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Der obige Code ähnelt dem Code aus `Game1` und `Robot` dem Zuweisen der Matrizen `BasicEffect`. 

Nachdem wir die neue Integration `Camera` Klasse in unserer vorhandenen Projekte. Zuerst ändern wir die `Robot` Klasse zu einer `Camera` -Instanz im seine `Draw` -Methode, die eine Menge von doppelt vorhandenem Code beseitigt werden. Ersetzen Sie die `Robot.Draw` -Methode durch Folgendes:

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

Ändern Sie als Nächstes die `Game1.cs` Datei:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Die Änderungen an der `Game1` aus der früheren Version (mit dem identifiziert `// New camera code` ) sind:

- `Camera` Feld in `Game1`
- `Camera` Instanziierung in `Game1.Initialize`
- `Camera.Update` Rufen Sie in `Game1.Update`
- `Robot.Draw` verwendet jetzt eine `Camera` Parameter
- `Game1.Draw` verwendet jetzt `Camera.ViewMatrix` und `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Verschieben Sie die Kamera mit der Eingabe

Wir haben bisher hinzugefügt eine `Camera` Entität, aber die nichts damit Laufzeitverhalten ändern dies nicht getan haben. Fügen wir Verhalten dem der Benutzer kann auf:

- Tippen Sie auf der linken Seite des Bildschirms, um die Kamera auf der linken Seite aktivieren
- Tippen Sie auf der rechten Seite des Bildschirms, um die Kamera auf der rechten Seite zu aktivieren.
- Tippen Sie auf der Mitte des Bildschirms, um die Kamera nach vorne verschieben

### <a name="making-lookat-relative"></a>LookAt relativ machen

Wir werden zunächst aktualisieren die `Camera` Klasse einbeziehen, ein `angle` Feld, das verwendet werden, wird der Richtung festlegen, die die `Camera` sind aufgetreten. Derzeit unsere `Camera` bestimmt die Richtung, es befindet sich in einer durch die lokale `lookAtVector`, zugewiesen an `Vector3.Zero`. Das heißt, unsere `Camera` immer am ursprünglichen Speicherort gesucht. Wenn die Kamera verschoben wird, wird der Winkel, den die Kamera zeigt auch geändert werden:

![](part3-images/image11.gif "Wenn die Kamera verschoben wird, klicken Sie dann ändert der Winkel, den die Kamera zeigt sich auch")

Wir möchten die `Camera` , mindestens die gleiche Richtung unabhängig von seiner Position – verbunden ist bis wir die Logik für das Drehen von Implementieren der `Camera` verwendet für die Eingabe. Die erste Änderung werden zum Anpassen der `lookAtVector` Variable außerhalb unserer aktuellen Position basieren soll, anstatt eine absolute Position anzeigen:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Dadurch wird die `Camera` Anzeigen der ganzen Welt direkt an. Beachten Sie, dass der ursprüngliche `position` Wert so geändert wurde, `(0, 20, 10)` daher `Camera` basiert auf der x-Achse. Das Spiel zeigt das ausführen:

![](part3-images/image12.png "Diese Ansicht zeigt Ausführen des Spiels")

### <a name="creating-an-angle-variable"></a>Erstellen eines Winkels Variable

Die `lookAtVector` Variable legt den Winkel an, die unsere Kamera angezeigt wird. Derzeit auf der negativen y-Achse zu erhalten, festgelegt und leicht nach unten geneigt (aus der `-.5f` Z-Wert). Wir erstellen eine `angle` Variable, die verwendet wird, passen Sie die `lookAtVector` Eigenschaft. 

In früheren Abschnitten in dieser exemplarischen Vorgehensweise haben wir gezeigt, dass die Matrizen verwendet werden können, zu drehen, wie Objekte gezeichnet werden. Wir können auch Matrizen, Vektoren wie Drehen der `lookAtVector` mithilfe der `Vector3.Transform` Methode. 

Hinzufügen einer `angle` Felds und ändern Sie die `ViewMatrix` -Eigenschaft wie folgt:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Lesen von Eingaben

Unsere `Camera` Entität kann jetzt vollständig über gesteuert werden seine Position und Winkel Variablen – es muss lediglich diese je nach der Eingabe zu ändern.

Wir rufen zuerst die `TouchPanel` Zustand zu finden, in denen der Benutzer den Bildschirm berührt. Weitere Informationen zur Verwendung der `TouchPanel` Klasse, finden Sie unter [die TouchPanel-API-Referenz](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Wenn die Berührung auf der linken dritte passen wir die `angle` Wert also die `Camera` dreht sich links, und wenn der Benutzer auf die richtige dritte betrifft, werden wir andere Art und Weise drehen. Wenn der Benutzer wird im mittleren Drittel des Bildschirms durch berühren, anschließend verschieben wir die `Camera` weiterleiten.

Fügen Sie zunächst eine using-Anweisung qualifizieren der `TouchPanel` und `TouchCollection` Klassen im `Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Ändern Sie als Nächstes die `Update` Methode zum Lesen des Touch-Bereichs, und passen Sie die `angle` und `position` Variablen entsprechend:

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Jetzt die `Camera` Fingereingaben antwortet:

![](part3-images/image1.gif "Nun wird die Kamera Fingereingaben reagieren.")

Die Update-Methode beginnt mit dem Aufruf `TouchPanel.GetState`, die eine Auflistung von Workflows zurückgibt. Obwohl `TouchPanel.GetState` können zurückgeben mehrerer Touch-Punkte müssen wir nur die erste kopiersitzung für Einfachheit fürchten.

Wenn der Benutzer den Bildschirm berührt, überprüft der Code zum Prüfen, ob die erste Fingereingabe in der linken, mittleren oder rechten Rand des Bildschirms dritten ist. Die linken und rechten Drittel der Kamera drehen, durch erhöhen oder Verringern der `angle` Variablen gemäß der `TotalSeconds` Wert (sodass das Spiel unabhängig von der Einzelbildrate verhält sich genauso).

Wenn der Benutzer das Center Drittens berührt des Bildschirms, klicken Sie dann die Kamera wird fortfahren. Dies geschieht zuerst durch das Abrufen der vorwärtsvektor, die zunächst als auf der negativen y-Achse definiert, wird mit einer Matrix mit erstellt gedreht `Matrix.CreateRotationZ` und `angle` Wert. Zum Schluss die `forwardVector` gilt für `position` mithilfe der `unitsPerSecond` Koeffizienten.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird beschrieben, wie verschoben und gedreht `Models` in 3D Speicherplatz mit `Matrices` und `BasicEffect.World` Eigenschaft. Diese Form der Bewegung bildet die Grundlage zum Verschieben von Objekten in 3D-Spiele ein. In dieser exemplarischen Vorgehensweise wird auch beschrieben, wie zum Implementieren einer `Camera` Entität für die Anzeige von der ganzen Welt von der Position und Winkel.

## <a name="related-links"></a>Verwandte Links

- [MonoGame-API-Link](http://www.monogame.net/documentation/?page=api)
- [Fertiges Projekt (Beispiel)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
