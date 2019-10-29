---
title: 3D-Koordinaten in monogame
description: Das Verständnis des 3D-Koordinatensystems ist ein wichtiger Schritt bei der Entwicklung von 3D-spielen. Monogame bietet eine Reihe von Klassen zum Positionieren, ausrichten und Skalieren von Objekten im 3D-Raum.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: a38f4b81f684d6d416e6abe017bc463e3097c6b1
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980857"
---
# <a name="3d-coordinates-in-monogame"></a>3D-Koordinaten in monogame

_Das Verständnis des 3D-Koordinatensystems ist ein wichtiger Schritt bei der Entwicklung von 3D-spielen. Monogame bietet eine Reihe von Klassen zum Positionieren, ausrichten und Skalieren von Objekten im 3D-Raum._

Die Entwicklung von 3D-Spielen erfordert ein Verständnis der Bearbeitung von Objekten in einem 3D-Koordinatensystem. In dieser exemplarischen Vorgehensweise wird erläutert, wie visuelle Objekte (insbesondere ein Modell) bearbeitet werden. Wir bauen auf den Konzepten zum Steuern eines Modells auf, um eine 3D-Kamera Klasse zu erstellen.

Die dargestellten Konzepte stammen aus der linearen Algebra, aber wir werden einen praktischen Ansatz heranziehen, damit jeder Benutzer, der keinen starken mathematischen Hintergrund hat, diese Konzepte in eigenen spielen anwenden kann.

In den folgenden Themen werden die folgenden Themen behandelt:

- Erstellen eines Projekts
- Erstellen einer Roboter Entität
- Verschieben der Roboterentität
- Matrix Multiplikation
- Erstellen der Kamera-Entität
- Verschieben der Kamera mit Eingabe

Sobald der Vorgang abgeschlossen ist, verfügen wir über ein Projekt mit einem Roboter, der sich in einem Kreis und einer Kamera bewegt, die per Fingereingabe gesteuert werden kann:

![](part3-images/image1.gif "Once finished, the app will include a project with a robot moving in a circle and a camera which can be controlled by touch input")

## <a name="creating-a-project"></a>Erstellen eines Projekts

Diese exemplarische Vorgehensweise konzentriert sich auf das Verschieben von Objekten in 3D-Raum. Wir beginnen mit dem Projekt zum Rendern von Modellen und Vertex-Arrays [, die hier zu finden sind](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/). Nach dem herunterladen entzippen und öffnen Sie das Projekt, um sicherzustellen, dass es ausgeführt wird. es sollte Folgendes angezeigt werden:

![](part3-images/image2.png "Once downloaded, unzip and open the project to make sure it runs and this view should be displayed")

## <a name="creating-a-robot-entity"></a>Erstellen einer Roboter Entität

Bevor wir mit dem Verschieben unseres Roboters beginnen, erstellen wir eine `Robot` Klasse, die Logik für zeichnen und Bewegung enthält. Spieleentwickler verweisen auf diese Kapselung von Logik und Daten als *Entität*.

Fügen Sie der **MonoGame3D** portable-Klassenbibliothek (nicht der plattformspezifischen modelandverts. Android) eine neue leere Klassendatei hinzu. Nennen Sie es **Roboter** , und klicken Sie auf **neu**:

![](part3-images/image3.png "Name it Robot and click New")

Ändern Sie die `Robot`-Klasse wie folgt:

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

Der `Robot` Code ist im Wesentlichen derselbe Code in `Game1` zum Zeichnen eines `Model`. Einen Review zum Laden und Zeichnen von `Model` finden Sie [in diesem Handbuch zum Arbeiten mit Modellen](~/graphics-games/monogame/3d/part1.md). Nun können Sie alle `Model` Lade-und Renderingcode aus `Game1`entfernen und durch eine `Robot` Instanz ersetzen:

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

Wenn wir den Code jetzt ausführen, haben wir eine Szene mit nur einem Roboter, der größtenteils in der Etage gezeichnet wird:

![](part3-images/image4.png "If the code is run now, the app will display a scene with only one robot which is drawn mostly under the floor")

## <a name="moving-the-robot"></a>Verschieben des Roboters

Nachdem wir nun über eine `Robot`-Klasse verfügen, können wir dem Roboter Verschiebungs Logik hinzufügen. In diesem Fall wird der Roboter einfach entsprechend der Spiel Zeit in einem Kreis bewegt. Dies ist eine etwas unpraktische Implementierung für ein echtes Spiel, da ein Zeichen in der Regel auf Eingaben oder künstliche Intelligenz reagieren kann, aber es bietet eine Umgebung, in der wir die 3D-Positionierung und-Rotation erkunden können.

Die einzige Information, die wir von außerhalb der `Robot` Klasse benötigen, ist die aktuelle Spiel Zeit. Wir fügen eine `Update`-Methode hinzu, die einen `GameTime`-Parameter annimmt. Dieser `GameTime` Parameter wird verwendet, um eine Angle-Variable zu erhöhen, die wir verwenden werden, um die endgültige Position für den Roboter zu bestimmen.

Zuerst fügen wir das Feld "Angle" der `Robot`-Klasse unter dem `model` Feld hinzu:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Nun können wir diesen Wert in einer `Update`-Funktion erhöhen:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Wir müssen sicherstellen, dass die `Update`-Methode von `Game1.Update`aufgerufen wird:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Zu diesem Zeitpunkt führt das Feld "Angle" nichts aus – wir müssen Code schreiben, um es zu verwenden. Wir ändern die `Draw` Methode, damit wir die Welt `Matrix` in einer dedizierten Methode berechnen können: 

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

Als nächstes implementieren wir die `GetWorldMatrix`-Methode in der `Robot`-Klasse:

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

Das Ergebnis der Ausführung dieses Codes führt dazu, dass sich der Roboter in einem Kreis bewegt:

![](part3-images/image5.gif "Running this code results in the robot moving in a circle")

## <a name="matrix-multiplication"></a>Matrix Multiplikation

Der obige Code dreht den Roboter, indem er eine `Matrix` in der `GetWorldMatrix`-Methode erstellt. Die `Matrix` Struktur enthält 16 float-Werte, die verwendet werden können, um (Position festlegen), drehen und Skalieren (Größe festlegen) zu übersetzen. Wenn Sie die `effect.World`-Eigenschaft zuweisen, teilen wir dem zugrunde liegenden Renderingsystem mit, wie alles, was gezeichnet werden soll (ein `Model` oder eine Geometrie aus Scheitel Punkten). 

Glücklicherweise enthält die `Matrix` Struktur eine Reihe von Methoden, die die Erstellung gängiger Matrizen vereinfachen. Der erste im obigen Code verwendete `Matrix.CreateTranslation`. Der mathematische Begriff " *Translation* " bezieht sich auf einen Vorgang, bei dem ein Punkt (oder in unserem Fall ein Modell) ohne weitere Änderung (z. b. Drehen oder Ändern der Größe) von einem Speicherort zu einem anderen verschoben wird. Die Funktion nimmt einen X-, Y-und Z-Wert für die Übersetzung an. Wenn unsere Szene von oben nach unten angezeigt wird, führt unsere `CreateTranslation` Methode (isoliert) Folgendes aus:

![](part3-images/image6.png "The CreateTranslation method in isolation performs this action")

Die zweite Matrix, die wir erstellen, ist eine Rotations Matrix, die die `CreateRotationZ` Matrix verwendet. Dies ist eine von drei Methoden, die zum Erstellen der Drehung verwendet werden können:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Jede Methode erstellt eine Rotations Matrix, indem Sie sich über eine bestimmte Achse dreht. In unserem Fall drehen wir uns um die Z-Achse, die auf "nach oben" zeigt. Die folgenden Informationen können Ihnen dabei helfen, die Funktionsweise der Achsen basierten Rotation zu veranschaulichen:

![](part3-images/image7.png "This can help visualize how axis-based rotation works")

Wir verwenden auch die `CreateRotationZ`-Methode mit dem Angle-Feld, das im Laufe der Zeit Inkremente ist, weil unsere `Update` Methode aufgerufen wird. Das Ergebnis ist, dass die `CreateRotationZ`-Methode bewirkt, dass der Roboter den Ursprung durchläuft, wenn die Zeit verstrichen ist.

Die letzte Codezeile kombiniert die beiden Matrizen zu einem:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Dies wird als Matrix Multiplikation bezeichnet, die sich etwas von der regulären Multiplikation unterscheidet. Die *kommutativ-Eigenschaft der Multiplikation* gibt an, dass die Reihenfolge der Zahlen in einem Multiplikations Vorgang das Ergebnis nicht ändert. Das heißt, 3 \* 4 entspricht 4 \* 3. Die Matrix Multiplikation unterscheidet sich darin, dass Sie nicht commutativ ist. Das heißt, dass die obige Zeile als "übernehmen der translationmatrix zum Verschieben des Modells und anschließendes Drehen von Elementen durch Anwenden der RotationMatrix" gelesen werden kann. Wir könnten die Art und Weise visualisieren, wie sich die obige Zeile wie folgt auf die Position und Drehung auswirkt:

![](part3-images/image8.png "A visualization pf the way that the above line affects the position and rotation")

Um zu verstehen, wie sich die Reihenfolge der Matrix Multiplikation auf das Ergebnis auswirken kann, sollten Sie Folgendes beachten, wenn die Matrix Multiplikation invertiert wird:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Der obige Code dreht zuerst das Modell direkt und übersetzt dann Folgendes:

![](part3-images/image9.png "The code above would first rotate the model in-place, then translate it")

Wenn Sie den Code mit der invertierten Multiplikation ausführen, werden wir bemerken, dass sich die Drehung zuerst auf die Ausrichtung des Modells auswirkt und die Position des Modells unverändert bleibt. Anders ausgedrückt: das Modell dreht sich an Ort und Stelle:

![](part3-images/image10.gif "The model rotates in place")

## <a name="creating-the-camera-entity"></a>Erstellen der Kamera-Entität

Die `Camera`-Entität enthält die gesamte erforderliche Logik, um eine Eingabe basierte Bewegung auszuführen und Eigenschaften zum Zuweisen von Eigenschaften für die `BasicEffect`-Klasse bereitzustellen.

Zuerst implementieren wir eine statische Kamera (keine Eingabe basierte Bewegung) und integrieren Sie in unser vorhandenes Projekt. Fügen Sie der **MonoGame3D** portable-Klassenbibliothek (dem gleichen Projekt mit `Robot.cs`) eine neue Klasse hinzu, und nennen Sie Sie **Kamera**. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

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

Der obige Code ähnelt dem Code aus `Game1` und `Robot`, der die Matrizen auf `BasicEffect`zuweist. 

Nun können wir die neue `Camera`-Klasse in unsere vorhandenen Projekte integrieren. Zunächst ändern wir die `Robot`-Klasse, um eine `Camera`-Instanz in der `Draw`-Methode zu verwenden, die einen großen doppelten Code ausschließt. Ersetzen Sie die `Robot.Draw`-Methode durch Folgendes:

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

Ändern Sie als nächstes die `Game1.cs` Datei:

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

Die Änderungen am `Game1` aus der früheren Version (die mit `// New camera code` identifiziert werden) lauten wie folgt:

- `Camera` Feld in `Game1`
- `Camera` Instanziierung in `Game1.Initialize`
- in `Game1.Update` `Camera.Update` aufgerufen werden.
- `Robot.Draw` jetzt einen `Camera` Parameter
- `Game1.Draw` verwendet nun `Camera.ViewMatrix` und `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Verschieben der Kamera mit Eingabe

Bisher haben wir eine `Camera` Entität hinzugefügt, aber noch nichts damit getan, um das Laufzeitverhalten zu ändern. Wir fügen das Verhalten hinzu, das dem Benutzer folgende Aktionen ermöglicht:

- Tippen Sie auf die linke Seite des Bildschirms, um die Kamera in der linken Ecke zu drehen.
- Tippen Sie auf die Rechte Seite des Bildschirms, um die Kamera auf der rechten Seite zu ändern.
- Den Mittelpunkt des Bildschirms berühren, um die Kamera vorwärts zu bewegen

### <a name="making-lookat-relative"></a>Erstellen eines relativen lookat

Zuerst aktualisieren wir die `Camera` Klasse so, dass Sie ein `angle` Feld enthält, das verwendet wird, um die Richtung festzulegen, der die `Camera` steht. Zurzeit bestimmt unsere `Camera` die Richtung, mit der Sie über den lokalen `lookAtVector`, der `Vector3.Zero`zugewiesen ist, zuweist. Das heißt, unser `Camera` untersucht immer den Ursprung. Wenn die Kamera verschoben wird, wird auch der Winkel geändert, dem die Kamera ausgesetzt ist:

![](part3-images/image11.gif "If the Camera moves, then the angle that the camera is facing will also change")

Wir möchten, dass die `Camera` unabhängig von ihrer Position mit der gleichen Richtung übereinstimmen – zumindest bis die Logik zum Rotieren der `Camera` mithilfe von Eingaben implementiert wird. Die erste Änderung besteht darin, dass die `lookAtVector` Variable so angepasst wird, dass Sie auf dem aktuellen Speicherort basiert, anstatt eine absolute Position zu betrachten:

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

Dies führt dazu, dass die `Camera` die Welt direkt anzeigen. Beachten Sie, dass der anfängliche `position` Wert geändert wurde, um `(0, 20, 10)`, sodass der `Camera` auf der X-Achse zentriert ist. Beim Ausführen des Spiels wird Folgendes angezeigt:

![](part3-images/image12.png "Running the game displays this view")

### <a name="creating-an-angle-variable"></a>Erstellen einer Winkel Variablen

Die `lookAtVector` Variable steuert den Winkel, den die Kamera anzeigen wird. Zurzeit ist es so festgelegt, dass die negative Y-Achse angezeigt wird, und etwas nach unten (vom `-.5f` Z-Wert). Wir erstellen eine `angle` Variable, die zum Anpassen der `lookAtVector`-Eigenschaft verwendet wird. 

In den vorherigen Abschnitten dieser exemplarischen Vorgehensweise haben wir gezeigt, dass Matrizen verwendet werden können, um zu drehen, wie Objekte gezeichnet werden. Wir können auch Matrizen verwenden, um Vektoren wie die `lookAtVector` mithilfe der `Vector3.Transform`-Methode zu drehen. 

Fügen Sie ein `angle` Feld hinzu, und ändern Sie die `ViewMatrix`-Eigenschaft wie folgt:

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

### <a name="reading-input"></a>Lesen der Eingabe

Unsere `Camera` Entität kann nun vollständig über Ihre Positions-und Winkel Variablen gesteuert werden – wir müssen Sie nur entsprechend der Eingabe ändern.

Zuerst erhalten wir den `TouchPanel` Status, um zu ermitteln, wo der Benutzer den Bildschirm berührt. Weitere Informationen zur Verwendung der `TouchPanel`-Klasse finden Sie in [der API-Referenz zu Touchpanel](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Wenn der Benutzer auf der linken dritten Seite berührt, passen wir den `angle` Wert so an, dass die `Camera` nach links drehen, und wenn der Benutzer auf der rechten dritten Seite berührt, drehen wir die andere Methode. Wenn sich der Benutzer in der Mitte des Bildschirms befindet, verschieben wir den `Camera` vorwärts.

Fügen Sie zunächst eine using-Anweisung hinzu, um die Klassen `TouchPanel` und `TouchCollection` in `Camera.cs`zu qualifizieren:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Ändern Sie als nächstes die `Update`-Methode, um den touchbereich zu lesen, und passen Sie die `angle` und `position` Variablen entsprechend an:

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

Nun antwortet der `Camera` auf die Berührungs Eingabe:

![](part3-images/image1.gif "Now the Camera will respond to touch input")

Die Update-Methode beginnt mit dem Aufrufen von `TouchPanel.GetState`, das eine Auflistung von Berührungen zurückgibt. Obwohl `TouchPanel.GetState` mehrere Berührungspunkte zurückgeben kann, machen wir uns nur für die erste Gedanken um die Einfachheit.

Wenn der Benutzer den Bildschirm berührt, prüft der Code, ob sich der erste Fingerabdruck im linken, mittleren oder rechten Drittel des Bildschirms befindet. Die linke und die Rechte Drittel drehen die Kamera, indem Sie die `angle` Variable entsprechend der `TotalSeconds` Werts vergrößern oder verkleinern (sodass sich das Spiel unabhängig von der Framerate verhält).

Wenn der Benutzer den Mittelpunkt des dritten Bildschirms berührt, wird die Kamera fortfahren. Dies wird zuerst erreicht, indem der vorwärts Vektor, der anfänglich als Verweis auf die negative Y-Achse definiert ist, und dann durch eine mit `Matrix.CreateRotationZ` und dem `angle` Wert erstellte Matrix gedreht wird. Schließlich wird der `forwardVector` mithilfe des `unitsPerSecond` Koeffizienten auf `position` angewendet.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie `Models` im 3D-Raum mithilfe `Matrices` und der `BasicEffect.World`-Eigenschaft verschieben und drehen. Diese Form der Bewegung stellt die Grundlage für das Verschieben von Objekten in 3D-Spielen dar. In dieser exemplarischen Vorgehensweise wird auch erläutert, wie eine `Camera` Entität zum Anzeigen der Welt aus beliebigen Positionen und Winkeln implementiert wird.

## <a name="related-links"></a>Verwandte Links

- [Monogame-API-Link](http://www.monogame.net/documentation/?page=api)
- [Fertiges Projekt (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/monogame3dcamera)
