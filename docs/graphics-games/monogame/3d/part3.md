---
title: 3D Koordinaten im MonoGame
description: Grundlegendes zu den 3D Koordinatensystem ist ein wichtiger Schritt bei der Entwicklung von 3D-Spielen. MonoGame bietet eine Reihe von Klassen zum Positionieren, ausrichten und Skalieren von Objekten in einem 3D-Bereich.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 0273b4f13c91fd766530ff7c0976096de3239dc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="3d-coordinates-in-monogame"></a>3D Koordinaten im MonoGame

_Grundlegendes zu den 3D Koordinatensystem ist ein wichtiger Schritt bei der Entwicklung von 3D-Spielen. MonoGame bietet eine Reihe von Klassen zum Positionieren, ausrichten und Skalieren von Objekten in einem 3D-Bereich._

Entwickeln von 3D-Spielen erfordert Kenntnisse zum Bearbeiten von Objekten in einem 3D Koordinatensystem. In dieser exemplarischen Vorgehensweise wird zum Bearbeiten der visuellen Objekte (insbesondere ein Modell) behandelt. Wir werden die Konzepte zu steuern, ein Modell zum Erstellen einer 3D Kamera-Klasse erstellen.

Die vorgestellten Konzepte stammen lineare Algebra, aber wir führen eine praktische Anleitung, damit diese Konzepte in ihrer eigenen Spiele eines beliebigen Benutzers ohne einen starken mathematische Hintergrund angewendet werden kann.

Wir müssen die folgenden Themen behandelt:

 - Erstellen eines Projekts
 - Erstellen einer Roboterentität
 - Verschieben Sie die Roboterentität
 - Matrixmultiplikation
 - Die Kamera-Entität erstellen
 - Verschieben Sie die Kamera mit Eingabe

Sobald Sie fertig sind, haben wir ein Projekt mit einem Roboter verschieben in einen Kreis und eine Kamera, die vom Fingereingabe gesteuert werden kann:

![](part3-images/image1.gif "Sobald Sie fertig sind, enthält die app ein Projekt mit einem Roboter verschieben in einen Kreis und eine Kamera, die vom Fingereingabe gesteuert werden können")


# <a name="creating-a-project"></a>Erstellen eines Projekts

In dieser exemplarischen Vorgehensweise konzentriert sich auf Objekte im 3D-Raum verschieben. Wir beginnen mit dem Projekt für Rendering-Modelle und Vertex Arrays [die finden Sie hier](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/). Nach dem Herunterladen, extrahieren Sie und öffnen Sie das Projekt, um sicherzustellen, dass er ausgeführt wird, und es sollte Folgendes angezeigt:

![](part3-images/image2.png "Nach dem Herunterladen, extrahieren Sie und öffnen Sie das Projekt, um sicherzustellen, dass er ausgeführt wird und in dieser Ansicht angezeigt werden soll")


# <a name="creating-a-robot-entity"></a>Erstellen einer Roboterentität

Bevor wir beginnen, unsere Roboter um verschieben, erstellen wir eine `Robot` Klasse, um die Logik zum Zeichnen und Bewegung enthalten. Spieleentwicklern finden Sie in diese Kapselung der Logik und die Daten als ein *Entität*.

Fügen Sie eine neue leere Klassendatei, die **MonoGame3D** Portable Klassenbibliothek (nicht die plattformspezifischen ModelAndVerts.Android). Nennen Sie sie **Roboter** , und klicken Sie auf **neu**:

![](part3-images/image3.png "Nennen Sie sie Roboter, und klicken Sie auf neu")

Ändern der `Robot` -Klasse wie folgt:


```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);
                        
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
    }
}
```

Die `Robot` Code ist im Grunde der gleiche Code in `Game1` zum Zeichnen einer `Model`. Für eine Überprüfung auf `Model` laden und zeichnen, finden Sie unter [dieses Handbuch zum Arbeiten mit Modellen](~/graphics-games/monogame/3d/part1.md). Wir können jetzt alle Entfernen der `Model` laden und Rendern von Code aus `Game1`, und Ersetzen Sie sie durch eine `Robot` Instanz:


```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;
                        
            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }                                          
}
```

Beim Ausführen des Codes wird nun wir eine Szene mit nur einem Roboter verfügen, die größtenteils unter den Tiefstwert gezeichnet wird:

![](part3-images/image4.png "Wenn der Code jetzt ausgeführt wird, wird die app eine Szene mit nur einem Roboter anzuzeigen, größtenteils unter den Tiefstwert gezeichnet wird")


# <a name="moving-the-robot"></a>Verschieben des Roboters

Nun mit dem eine `Robot` -Klasse, können wir des Roboters Bewegung Logik hinzufügen. In diesem Fall treffen wir einfach des Roboters in einem Kreis entsprechend der Uhrzeit der Spiel verschieben. Hierbei handelt es sich um eine etwas alleine nicht durchführbar Implementierung bei einem echten Spiel, da Eingabespalten reagieren ein Zeichen in der Regel oder KI, bietet jedoch eine Umgebung für uns 3D positionieren und Drehung zu untersuchen.

Die einzige Information, wir von außerhalb von benötigen, die `Robot` Klasse ist die aktuelle Spiel Zeit. Fügen wir eine `Update` Methode die dauert eine `GameTime` Parameter. Dies `GameTime` Parameter wird verwendet, um eine Variable Winkel zu erhöhen, die zum Bestimmen der letzten Position für den Robot verwendet wird.

Zunächst fügen wir das Feld Winkel der `Robot` unter Klasse die `model` Feld:


```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ... 
```

 Jetzt wir diesen Wert in erhöhen einer `Update` Funktion:


```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Wir müssen sicherstellen, dass die `Update` Methode wird aufgerufen, von `Game1.Update`:


```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
} 
```

Natürlich an diesem Punkt Feld Winkel wird keine Aktion ausgeführt – wir müssen Code schreiben, um es zu verwenden. Ändern wir das `Draw` Methode, damit wir die ganzen Welt berechnen können `Matrix` in einer dedizierten Methode: 


```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
                
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
} 
```

Als Nächstes implementieren wir die `GetWorldMatrix` Methode in der `Robot` Klasse:


```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;
    
    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
} 
```

Das Ergebnis der Ausführung dieses Codes ergibt sich der Robot in einem Kreis verschieben:

![](part3-images/image5.gif "Dieser Code Ergebnisse in des Roboters verschieben in einem Kreis ausgeführt wird.")


# <a name="matrix-multiplication"></a>Matrixmultiplikation

Der obige Code dreht des Roboters durch das Erstellen einer `Matrix` in die `GetWorldMatrix` Methode. Die `Matrix` Struktur enthält 16 "float"-Werte, die verwendet werden können, um (positionieren) umsetzen, drehen und skalieren (Größe festgelegt). Wenn wir weisen die `effect.World` -Eigenschaft, folgert, da wir das zugrunde liegende Rendern System zum Positionieren, Größe und orientieren nach Belieben wir zufällig gezeichnet werden (eine `Model` oder Geometrie aus den Scheitelpunkten). 

Glücklicherweise der `Matrix` Struktur umfasst eine Reihe von Methoden, die die Erstellung von häufig verwendete Typen von Matrizen zu vereinfachen. Die erste im obigen Code verwendet `Matrix.CreateTranslation`. Der mathematische Begriff *Übersetzung* bezieht sich auf einen Vorgang in einen Punkt (oder in unserem Fall ein Modell) vortäuschen mehr zwischen Speicherorten verschoben, ohne andere Änderungen (z. B. drehen, oder Ändern der Größe). Die Funktion akzeptiert einen X-, Y- und Z-Wert für die Übersetzung. Wenn wir unsere Szene von oben nach unten, zeigen Sie an unsere `CreateTranslation` -Methode (in Isolation) führt die folgenden:

![](part3-images/image6.png "Die Methode CreateTranslation isoliert führt diese Aktion")

Die zweite Matrix, die wir erstellen, wird eine Rotationsmatrix mithilfe der `CreateRotationZ` Matrix. Dies ist eine der drei Methoden, die zum Erstellen der Drehung verwendet werden können:

 - `CreateRotationX`
 - `CreateRoationY`
 - `CreateRotationZ`

Jede Methode erstellt eine Rotationsmatrix, indem Sie über eine angegebene Achse drehen. In unserem Fall werden wir die Z-Achse drehen "oben" verweist. Folgendes können wie Achse-basierte vorstellbar funktioniert:

![](part3-images/image7.png "Dies kann helfen, wie Achse-basierte vorstellbar Works")

Wir sind auch mithilfe der `CreateRotationZ` Methode mit dem Winkel-Feld, die mit der Zeit aufgrund erhöht unsere `Update` aufgerufenen Methode. Das Ergebnis ist, die `CreateRotationZ` Methode bewirkt, dass unsere Roboter, um den Ursprung Zeit Kreisen.

Die letzte Zeile des Codes kombiniert die beiden Matrizen in einem:


```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Dies wird als Matrixmultiplikation, bezeichnet dies unterscheidet sich etwas von regulären Multiplikation funktioniert. Die *kommutativ Eigenschaft Multiplikation* gibt an, dass die Reihenfolge der Zahlen in einen Multiplikationsvorgang das Ergebnis nicht ändert. D. h. gleich 3 * 4 4 * 3. Matrixmultiplikation unterscheidet sich insofern, dass es nicht kommutativ. Die obige Zeile kann also als gelesen werden, "Gelten die TranslationMatrix, um das Modell zu verschieben, und drehen alles, was durch Anwenden der RotationMatrix". Wir konnten die Möglichkeit visualisieren, dass die obige Zeile der Position und Drehung wie folgt beeinflusst:

![](part3-images/image8.png "Die Möglichkeit, dass die obige Zeile der Position und Drehung wirkt sich auf eine Visualisierung-pf")

Um besser zu verstehen, wie die Reihenfolge der Matrixmultiplikation das Ergebnis auswirken kann, berücksichtigen Sie Folgendes, in dem der Matrixmultiplikation umgekehrt ist:


```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Der obige Code würde drehen das Modell direkt zunächst zu übersetzen:

![](part3-images/image9.png "Der obige Code würde drehen das Modell direkt zunächst zu übersetzen")

Wenn wir den Code mit invertierten Multiplikation ausführen, sehen wir, da die Rotation zunächst gilt, er wirkt sich nur auf die Ausrichtung des Modells und die Position des Modells unverändert bleibt. Das heißt, dreht sich das Modell vorhanden:

![](part3-images/image10.gif "Das Modell dreht vorhanden")


# <a name="creating-the-camera-entity"></a>Die Kamera-Entität erstellen

Die `Camera` Entität enthält die gesamte Logik erforderlich, führen Sie die Eingabe-basierten verschieben und um Eigenschaften bereitzustellen, für die Zuweisung von Eigenschaften auf der `BasicEffect` Klasse.

Zuerst wir eine statische Kamera (keine Eingabe-Bewegung) zu implementieren und in unserem vorhandenes Projekt integrieren. Fügen Sie eine neue Klasse, die **MonoGame3D** Portable Class Library (identisch mit Projekt `Robot.cs`) und nennen Sie sie **Kamera**. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:


```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;
                
                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Der obige Code ähnelt dem Code aus `Game1` und `Robot` dem Zuweisen der Matrizen `BasicEffect`. 

Nachdem wir die neue integrieren können `Camera` Klasse in unserer vorhandenen Projekte. Ändern wir zunächst die `Robot` Klasse wird eine `Camera` -Instanz in seiner` Draw `-Methode, die viele doppelte Code beseitigt werden. Ersetzen Sie die `Robot.Draw` -Methode durch Folgendes:


```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
} 
```

Als Nächstes ändern Sie die `Game1.cs` Datei:


```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;
                        
            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
} 
```

Die Änderungen an der `Game1` aus der früheren Version (die prognostiziert mit `// New camera code` ) sind:

 - `Camera` Feld `Game1`
 - `Camera` Instanziierung in `Game1.Initialize`
 - `Camera.Update` Rufen Sie in `Game1.Update`
 - `Robot.Draw` Jetzt dauert eine `Camera` Parameter
 - `Game1.Draw` verwendet jetzt `Camera.ViewMatrix` und `Camera.ProjectionMatrix`


# <a name="moving-the-camera-with-input"></a>Verschieben Sie die Kamera mit Eingabe

Wir haben bisher hinzugefügt eine `Camera` Entität jedoch etwas, um das Laufzeitverhalten ändern dies nicht getan haben. Nun fügen wir das Verhalten, die dem Benutzer ermöglicht:

 - Tippen Sie auf der linken Seite des Bildschirms, um die Kamera an der linken Seite zu aktivieren.
 - Tippen Sie auf der rechten Seite des Bildschirms zum Deaktivieren der Kamera auf der rechten Seite
 - Tippen Sie auf die Mitte des Bildschirms die Kamera weitergehen


## <a name="making-lookat-relative"></a>LookAt Relative herstellen

Wir werden zuerst Aktualisieren der `Camera` Klasse einbeziehen ein `angle` Feld, das verwendet werden, wird der Richtung festlegen, die die `Camera` zugewandt ist. Derzeit unsere `Camera` bestimmt die Richtung, es mit, über die lokale Internetzugriff wird `lookAtVector`, zugewiesen an `Vector3.Zero`. Das heißt, unsere `Camera` immer am ursprünglichen Speicherort gesucht. Die Kamera verschoben werden, wird auch der Winkel, den die Kamera mit Internetzugriff wird geändert:

![](part3-images/image11.gif "Wenn die Kamera verschoben wird, klicken Sie dann ändert der Winkel an, dem die Kamera zeigt sich auch")

Wir möchten die `Camera` auf die gleiche Richtung unabhängig von seiner Position – mindestens verbunden ist bis wir die Logik für das Drehen von Implementieren der` Camera `mithilfe von Eingaben. Die erste Änderung werden zum Anpassen der `lookAtVector` Variable außerhalb unserer aktuellen Speicherort basieren soll, anstatt eine absolute Position suchen:


```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    } 
    ...
```

Dadurch wird die `Camera` die ganzen Welt gerade auf anzeigen. Beachten Sie, dass der ursprüngliche `position` Wert so geändert wurde, `(0, 20, 10)` also die `Camera` basiert auf der x-Achse. Das Spiel zeigt die Betriebssysteme aufgeführt:

![](part3-images/image12.png "Ausführen des Spiels werden in dieser Ansicht angezeigt.")


## <a name="creating-an-angle-variable"></a>Erstellen einen Winkel Variable

Die `lookAtVector` Variablen bestimmt den Winkel an, die unsere Kamera angezeigt wird. Derzeit ist so repariert, die negative y-Achse zu erhalten, und etwas nach unten geneigt (aus der `-.5f` Z-Wert). Wir erstellen eine `angle` Variable, die verwendet werden soll, Anpassen der `lookAtVector` Eigenschaft. 

In früheren Abschnitten dieser exemplarischen Vorgehensweise wurde gezeigt, dass Matrizen verwendet werden können, drehen, wie Objekte gezeichnet werden. Es können auch Matrizen Vektoren wie Drehen der `lookAtVector` mithilfe der `Vector3.Transform` Methode. 

Hinzufügen einer `angle` Feld und ändern die `ViewMatrix` Eigenschaft wie folgt:


```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    } 
    ...
```


## <a name="reading-input"></a>Lesen von Eingaben

Unsere `Camera` Entität kann über seine Position und Winkel Variablen jetzt vollständig gesteuert – es muss lediglich nach der Eingabe ändern.

Erstens wir kommen die `TouchPanel` Zustand zu suchen, in denen der Benutzer den Bildschirm berührt. Weitere Informationen zur Verwendung der `TouchPanel` Klasse, finden Sie unter [die Touchpad API-Referenz](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Wenn der Benutzer wird auf der linken dritte berühren, und klicken Sie dann wir passen die `angle` Wert also die `Camera` dreht links, und wenn der Benutzer auf die Rechte Dritter betrifft, fügen wir eine andere Möglichkeit drehen. Wenn der Benutzer wird im mittleren Drittel des Bildschirms berühren, wir verschieben den `Camera` weiterleiten.

Zunächst fügen Sie eine using-Anweisung qualifizieren der `TouchPanel` und `TouchCollection` Klassen im `Camera.cs`:


```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Als Nächstes ändern Sie die `Update` Methode zum Lesen von Touchscreens und passen Sie die `angle` und `position` Variablen entsprechend:


```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
} 
```

Jetzt die `Camera` Fingereingaben antwortet:

![](part3-images/image1.gif "Jetzt wird die Kamera Fingereingaben reagieren.")

Die Update-Methode beginnt mit dem Aufruf `TouchPanel.GetState`, die eine Auflistung von Fingereingaben zurückgibt. Obwohl `TouchPanel.GetState` mehrere Berührungspunkte, zurückgeben können wir nur die erste Vorlage aus Gründen der Einfachheit kümmern müssen.

Wenn der Benutzer den Bildschirm berührt, überprüft der Code um festzustellen, ob die erste Fingereingabe linke, mittlere oder rechte Rand des Bildschirms dritte wird. Dreht die linken und rechten Drittel die Kamera erhöhen oder Verringern der `angle` Variable gemäß der `TotalSeconds` Wert (so, dass das Spiel unabhängig von der Framerate verhält sich genauso).

Wenn der Benutzer das Center im dritten betrifft des Bildschirms, klicken Sie dann die Kamera wird vorwärts. Hierbei wird zuerst der vorwärtsvektor, die ursprünglich als zeigen in Richtung der y-Achse negativen definiert ist, und mit einer Matrix mit erstellt gedreht abrufen `Matrix.CreateRotationZ` und `angle` Wert. Schließlich die `forwardVector` angewendet wird, um `position` mithilfe der `unitsPerSecond` Koeffizienten.


# <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird beschrieben, wie verschoben und gedreht `Models` in 3D Speicherplatz mit `Matrices` und `BasicEffect.World` Eigenschaft. Diese Form der Bewegung bildet die Grundlage für das Verschieben von Objekten in 3D Spielen. In dieser exemplarischen Vorgehensweise wird außerdem beschrieben, wie zum Implementieren einer `Camera` Entität für die Anzeige von der ganzen Welt aus jeder Position und Winkel.

## <a name="related-links"></a>Verwandte Links

- [MonoGame-API-Verknüpfung](http://www.monogame.net/documentation/?page=api)
- [Fertig gestellten Projekts (Beispiel)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
