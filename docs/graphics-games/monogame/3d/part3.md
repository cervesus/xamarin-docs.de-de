---
title: 3D-Koordinaten in monogame
description: Das Verständnis des 3D-Koordinatensystems ist ein wichtiger Schritt bei der Entwicklung von 3D-spielen. Monogame bietet eine Reihe von Klassen zum Positionieren, ausrichten und Skalieren von Objekten im 3D-Raum.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 54a4c6e32059b6ff32b3a93abf5fd30c65f16b5f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936629"
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

![Sobald der Vorgang abgeschlossen ist, enthält die APP ein Projekt mit einem Roboter, der in einem Kreis bewegt wird, und eine Kamera, die per Fingereingabe gesteuert werden kann.](part3-images/image1.gif)

## <a name="creating-a-project"></a>Erstellen eines Projekts

Diese exemplarische Vorgehensweise konzentriert sich auf das Verschieben von Objekten in 3D-Raum. Wir beginnen mit dem Projekt zum Rendern von Modellen und Vertex-Arrays [, die hier zu finden sind](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/). Nach dem herunterladen entzippen und öffnen Sie das Projekt, um sicherzustellen, dass es ausgeführt wird. es sollte Folgendes angezeigt werden:

![Nach dem herunterladen entzippen und öffnen Sie das Projekt, um sicherzustellen, dass es ausgeführt wird und diese Ansicht angezeigt werden soll.](part3-images/image2.png)

## <a name="creating-a-robot-entity"></a>Erstellen einer Roboter Entität

Bevor wir mit dem Verschieben unseres Roboters beginnen, wird eine Klasse erstellt, `Robot` die Logik für zeichnen und Bewegung enthält. Spieleentwickler verweisen auf diese Kapselung von Logik und Daten als *Entität*.

Fügen Sie der **MonoGame3D** portable-Klassenbibliothek (nicht der plattformspezifischen modelandverts. Android) eine neue leere Klassendatei hinzu. Nennen Sie es **Roboter** , und klicken Sie auf **neu**:

![Benennen Sie es Roboter, und klicken Sie auf neu](part3-images/image3.png)

Ändern Sie die- `Robot` Klasse wie folgt:

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

Im `Robot` wesentlichen ist der Code in `Game1` zum Zeichnen eines-Codes identisch `Model` . Weitere Informationen zum `Model` Laden und Zeichnen finden Sie [in diesem Handbuch zum Arbeiten mit Modellen](~/graphics-games/monogame/3d/part1.md). Nun können Sie den gesamten `Model` Lade-und Renderingcode aus entfernen `Game1` und durch eine- `Robot` Instanz ersetzen:

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

![Wenn der Code jetzt ausgeführt wird, wird in der App eine Szene mit nur einem Roboter angezeigt, der größtenteils unter der Etage gezeichnet wird.](part3-images/image4.png)

## <a name="moving-the-robot"></a>Verschieben des Roboters

Nachdem wir nun über eine `Robot` Klasse verfügen, können wir dem Roboter Verschiebungs Logik hinzufügen. In diesem Fall wird der Roboter einfach entsprechend der Spiel Zeit in einem Kreis bewegt. Dies ist eine etwas unpraktische Implementierung für ein echtes Spiel, da ein Zeichen in der Regel auf Eingaben oder künstliche Intelligenz reagieren kann, aber es bietet eine Umgebung, in der wir die 3D-Positionierung und-Rotation erkunden können.

Die einzige Information, die wir von außerhalb der Klasse benötigen, `Robot` ist die aktuelle Spiel Zeit. Wir fügen eine- `Update` Methode hinzu, die einen- `GameTime` Parameter annimmt. Dieser `GameTime` Parameter wird verwendet, um eine Angle-Variable zu erhöhen, die wir verwenden werden, um die endgültige Position für den Roboter zu bestimmen.

Zuerst fügen wir das Feld "Angle" der `Robot` Klasse unter dem `model` Feld hinzu:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Nun können wir diesen Wert in einer Funktion erhöhen `Update` :

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Wir müssen sicherstellen, dass die- `Update` Methode von aufgerufen wird `Game1.Update` :

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

Als nächstes implementieren wir die- `GetWorldMatrix` Methode in der- `Robot` Klasse:

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

![Wenn Sie diesen Code ausführen, wird der Roboter in einem Kreis bewegt.](part3-images/image5.gif)

## <a name="matrix-multiplication"></a>Matrix Multiplikation

Der obige Code dreht den Roboter, indem er `Matrix` in der- `GetWorldMatrix` Methode erstellt. Die `Matrix` Struktur enthält 16 float-Werte, die verwendet werden können, um (Position festlegen), drehen und Skalieren (Größe festlegen) zu übersetzen. Wenn wir die `effect.World` Eigenschaft zuweisen, weisen wir das zugrunde liegende Renderingsystem an, wie Sie die Position, Größe und Ausrichtung der Zeichen, die wir gerade zeichnen (eine `Model` oder Geometrie aus Scheitel Punkten), positionieren, anpassen und ausrichten 

Glücklicherweise enthält die `Matrix` Struktur eine Reihe von Methoden, die die Erstellung gängiger Matrizen vereinfachen. Der erste im obigen Code verwendete ist `Matrix.CreateTranslation` . Der mathematische Begriff " *Translation* " bezieht sich auf einen Vorgang, bei dem ein Punkt (oder in unserem Fall ein Modell) ohne weitere Änderung (z. b. Drehen oder Ändern der Größe) von einem Speicherort zu einem anderen verschoben wird. Die Funktion nimmt einen X-, Y-und Z-Wert für die Übersetzung an. Wenn unsere Szene von oben nach unten angezeigt wird, `CreateTranslation` führt unsere Methode (isoliert) Folgendes aus:

![Diese Aktion wird von der Methode "kreatetranslation" isoliert ausgeführt.](part3-images/image6.png)

Die zweite Matrix, die wir erstellen, ist eine Rotations Matrix, die die `CreateRotationZ` Matrix verwendet. Dies ist eine von drei Methoden, die zum Erstellen der Drehung verwendet werden können:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Jede Methode erstellt eine Rotations Matrix, indem Sie sich über eine bestimmte Achse dreht. In unserem Fall drehen wir uns um die Z-Achse, die auf "nach oben" zeigt. Die folgenden Informationen können Ihnen dabei helfen, die Funktionsweise der Achsen basierten Rotation zu veranschaulichen:

![Dies kann helfen, die Funktionsweise der Achsen basierten Rotation zu veranschaulichen.](part3-images/image7.png)

Wir verwenden auch die- `CreateRotationZ` Methode mit dem Angle-Feld, das im Laufe der Zeit Inkremente ist, weil unsere `Update` Methode aufgerufen wird. Das Ergebnis ist, dass die-Methode bewirkt, dass sich der `CreateRotationZ` Roboter um den Ursprung umschließt, wenn die Zeit vergeht.

Die letzte Codezeile kombiniert die beiden Matrizen zu einem:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Dies wird als Matrix Multiplikation bezeichnet, die sich etwas von der regulären Multiplikation unterscheidet. Die *kommutativ-Eigenschaft der Multiplikation* gibt an, dass die Reihenfolge der Zahlen in einem Multiplikations Vorgang das Ergebnis nicht ändert. Das heißt, 3 \* 4 entspricht 4 \* 3. Die Matrix Multiplikation unterscheidet sich darin, dass Sie nicht commutativ ist. Das heißt, dass die obige Zeile als "übernehmen der translationmatrix zum Verschieben des Modells und anschließendes Drehen von Elementen durch Anwenden der RotationMatrix" gelesen werden kann. Wir könnten die Art und Weise visualisieren, wie sich die obige Zeile wie folgt auf die Position und Drehung auswirkt:

![Eine Visualisierung PF, wie sich die obige Zeile auf die Position und Drehung auswirkt](part3-images/image8.png)

Um zu verstehen, wie sich die Reihenfolge der Matrix Multiplikation auf das Ergebnis auswirken kann, sollten Sie Folgendes beachten, wenn die Matrix Multiplikation invertiert wird:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Der obige Code dreht zuerst das Modell direkt und übersetzt dann Folgendes:

![Der obige Code würde zuerst das Modell direkt drehen und dann übersetzen.](part3-images/image9.png)

Wenn Sie den Code mit der invertierten Multiplikation ausführen, werden wir bemerken, dass sich die Drehung zuerst auf die Ausrichtung des Modells auswirkt und die Position des Modells unverändert bleibt. Anders ausgedrückt: das Modell dreht sich an Ort und Stelle:

![Das Modell rotiert an Ort und Stelle](part3-images/image10.gif)

## <a name="creating-the-camera-entity"></a>Erstellen der Kamera-Entität

Die `Camera` -Entität enthält die gesamte erforderliche Logik, um eine Eingabe basierte Bewegung auszuführen und Eigenschaften für das Zuweisen von Eigenschaften für die Klasse bereitzustellen `BasicEffect` .

Zuerst implementieren wir eine statische Kamera (keine Eingabe basierte Bewegung) und integrieren Sie in unser vorhandenes Projekt. Fügen Sie der **MonoGame3D** portable-Klassenbibliothek (dem gleichen Projekt mit) eine neue Klasse hinzu, `Robot.cs` und nennen Sie Sie **Kamera**. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

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

Der obige Code ähnelt dem Code von `Game1` und `Robot` , der die Matrizen zuweist `BasicEffect` . 

Nun können wir die neue `Camera` Klasse in unsere vorhandenen Projekte integrieren. Zuerst ändern wir die- `Robot` Klasse, um eine- `Camera` Instanz in der-Methode zu verwenden `Draw` , die einen großen doppelten Code ausschließt. Ersetzen Sie die- `Robot.Draw` Methode durch Folgendes:

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

Ändern Sie anschließend die `Game1.cs` Datei:

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

Die Änderungen an der `Game1` früheren Version (die mit identifiziert werden) lauten wie folgt `// New camera code` :

- `Camera`Feld in`Game1`
- `Camera`Instanziierung in`Game1.Initialize`
- `Camera.Update`Anrufen in`Game1.Update`
- `Robot.Draw`nimmt nun einen `Camera` Parameter an
- `Game1.Draw`verwendet nun `Camera.ViewMatrix` und`Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Verschieben der Kamera mit Eingabe

Bisher haben wir eine-Entität hinzugefügt, `Camera` aber noch nichts damit getan, um das Laufzeitverhalten zu ändern. Wir fügen das Verhalten hinzu, das dem Benutzer folgende Aktionen ermöglicht:

- Tippen Sie auf die linke Seite des Bildschirms, um die Kamera in der linken Ecke zu drehen.
- Tippen Sie auf die Rechte Seite des Bildschirms, um die Kamera auf der rechten Seite zu ändern.
- Den Mittelpunkt des Bildschirms berühren, um die Kamera vorwärts zu bewegen

### <a name="making-lookat-relative"></a>Erstellen eines relativen lookat

Zuerst aktualisieren wir die- `Camera` Klasse so, dass Sie ein-Feld enthält, `angle` das verwendet wird, um die Richtung festzulegen, mit der die-Klasse `Camera` mit dem Derzeit wird `Camera` die Richtung bestimmt, mit der Sie über das lokale `lookAtVector` , das zugewiesen wird, bestimmt wird `Vector3.Zero` . Das heißt, `Camera` der Ursprung wird immer durchsucht. Wenn die Kamera verschoben wird, wird auch der Winkel geändert, dem die Kamera ausgesetzt ist:

![Wenn die Kamera verschoben wird, wird auch der Winkel geändert, dem die Kamera ausgesetzt ist.](part3-images/image11.gif)

Wir möchten, dass die `Camera` Richtung unabhängig von ihrer Position mit der gleichen Richtung identisch ist – zumindest so lange, bis die Logik zum Drehen der using-Eingabe implementiert ist `Camera` . Die erste Änderung besteht darin, dass die Variable so angepasst wird, `lookAtVector` dass Sie auf dem aktuellen Speicherort basiert, anstatt eine absolute Position zu betrachten:

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

Dies führt dazu, dass die `Camera` Welt direkt angezeigt wird. Beachten Sie, dass der Anfangs `position` Wert in geändert wurde, `(0, 20, 10)` sodass der `Camera` auf der X-Achse zentriert ist. Beim Ausführen des Spiels wird Folgendes angezeigt:

![Die Ausführung des Spiels zeigt diese Ansicht an.](part3-images/image12.png)

### <a name="creating-an-angle-variable"></a>Erstellen einer Winkel Variablen

Die `lookAtVector` Variable steuert den Winkel, den die Kamera anzeigen wird. Zurzeit ist es so festgelegt, dass die negative Y-Achse angezeigt wird, und etwas nach unten (aus dem `-.5f` Z-Wert). Wir erstellen eine `angle` Variable, die zum Anpassen der Eigenschaft verwendet wird `lookAtVector` . 

In den vorherigen Abschnitten dieser exemplarischen Vorgehensweise haben wir gezeigt, dass Matrizen verwendet werden können, um zu drehen, wie Objekte gezeichnet werden. Wir können auch Matrizen verwenden, um Vektoren wie die `lookAtVector` mithilfe der-Methode zu drehen `Vector3.Transform` . 

Fügen Sie ein Feld hinzu, `angle` und ändern Sie die `ViewMatrix` Eigenschaft wie folgt:

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

Zuerst erhalten wir den Zustand, in dem der `TouchPanel` Benutzer den Bildschirm berührt. Weitere Informationen zur Verwendung der- `TouchPanel` Klasse finden Sie in [der API-Referenz zu Touchpanel](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Wenn sich der Benutzer auf der linken dritten Seite befindet, passen wir den `angle` Wert so an `Camera` , dass er nach links dreht, und wenn der Benutzer auf der rechten dritten Seite berührt, drehen wir den Wert auf andere Weise. Wenn sich der Benutzer in der Mitte des Bildschirms befindet, verschieben wir den `Camera` Forward.

Fügen Sie zunächst eine using-Anweisung hinzu, um die `TouchPanel` Klassen und in zu qualifizieren `TouchCollection` `Camera.cs` :

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Ändern Sie als nächstes die `Update` -Methode, um den touchbereich zu lesen, und passen Sie die `angle` `position` Variablen und entsprechend an:

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

Der `Camera` antwortet nun auf die Berührungs Eingabe:

![Nun antwortet die Kamera auf Berührungs Eingaben.](part3-images/image1.gif)

Die Update-Methode beginnt mit dem Aufrufen von `TouchPanel.GetState` , wodurch eine Auflistung von Berührungen zurückgegeben wird. Obwohl `TouchPanel.GetState` mehrere Berührungspunkte zurückgeben kann, machen wir uns nur für die erste Gedanken um die Einfachheit.

Wenn der Benutzer den Bildschirm berührt, prüft der Code, ob sich der erste Fingerabdruck im linken, mittleren oder rechten Drittel des Bildschirms befindet. Die linke und die Rechte Drittel drehen die Kamera, indem Sie die `angle` Variable entsprechend dem Wert vergrößern oder verkleinern `TotalSeconds` (sodass das Spiel unabhängig von der Framerate unverändert verhält).

Wenn der Benutzer den Mittelpunkt des dritten Bildschirms berührt, wird die Kamera fortfahren. Dies wird zuerst erreicht, indem der vorwärts Vektor erreicht wird, der anfänglich als Verweis auf die negative Y-Achse definiert ist, und dann durch eine mit `Matrix.CreateRotationZ` und dem Wert erstellte Matrix gedreht wird `angle` . Schließlich `forwardVector` wird der auf die `position` Verwendung des `unitsPerSecond` Koeffizienten angewendet.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird erläutert, wie `Models` Sie in 3D-Raum mithilfe von `Matrices` und der- `BasicEffect.World` Eigenschaft verschieben und drehen Diese Form der Bewegung stellt die Grundlage für das Verschieben von Objekten in 3D-Spielen dar. In dieser exemplarischen Vorgehensweise wird auch erläutert, wie eine `Camera` Entität zum Anzeigen der Welt aus beliebigen Positionen und Winkeln implementiert wird.

## <a name="related-links"></a>Verwandte Links

- [Monogame-API-Link](http://www.monogame.net/documentation/?page=api)
- [Fertiges Projekt (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/monogame3dcamera)
