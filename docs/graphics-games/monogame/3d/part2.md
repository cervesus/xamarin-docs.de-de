---
title: Zeichnen von 3D-Grafiken mit Scheitelpunkten in MonoGame
description: MonoGame unterstützt das Verwenden von Arrays von Vertices zum definieren, wie ein 3D-Objekt auf einer Basis pro Punkt gerendert wird. Benutzer profitieren von Vertex-Arrays zum Erstellen von dynamischen Geometrien, implementieren Spezialeffekte zu erzeugen und verbessern die Effizienz ihrer Rendering mit culling.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: df36c149e98e8c0cbb16de4c2cf52def5713ec13
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178588"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Zeichnen von 3D-Grafiken mit Scheitelpunkten in MonoGame

_MonoGame unterstützt das Verwenden von Arrays von Vertices zum definieren, wie ein 3D-Objekt auf einer Basis pro Punkt gerendert wird. Benutzer profitieren von Vertex-Arrays zum Erstellen von dynamischen Geometrien, implementieren Spezialeffekte zu erzeugen und verbessern die Effizienz ihrer Rendering mit culling._

Benutzer, die Lesen der [Anleitung zum Rendern von Modellen](~/graphics-games/monogame/3d/part1.md) mit rendering eines 3D-Modells in MonoGame vertraut sein. Die `Model` Klasse ist eine effektive Methode zum Rendern von 3D-Grafiken beim Arbeiten mit Daten, die in einer Datei (z. B. .fbx) definiert und beim Umgang mit statischen Daten. Einige Spiele erfordern 3D-Geometrie definiert oder dynamisch zur Laufzeit geändert werden. In diesen Fällen können wir verwenden von Arrays mit *Vertices* definieren und Geometrie rendern. Ein Vertex ist ein allgemeiner Begriff für einen Punkt im 3D-Raum, die Teil einer geordneten Liste, die zum Definieren der Geometrie ist. Vertices werden in der Regel so, definieren Sie eine Reihe von Dreiecken sortiert.

Damit können visualisieren, wie die Vertices verwendet werden, um 3D-Objekte zu erstellen, betrachten wir die folgende Kugel:

![](part2-images/image1.png "Damit visualisieren, wie die Vertices verwendet werden, um 3D-Objekte erstellen können, sollten Sie in diesem Zusammenhang")

Wie oben gezeigt, ist die Kugel eindeutig von mehreren Dreiecken bestehen. Wir sehen die Drahtmodell der Kugel angezeigt, wie die Scheitelpunkte in Form Dreiecke verbinden:

![](part2-images/image2.png "Anzeigen der Drahtmodell der Kugel angezeigt, wie die Scheitelpunkte in Form Dreiecke verbinden")

In dieser exemplarischen Vorgehensweise werden die folgenden Themen behandelt:

- Erstellen eines Projekts
- Erstellen die vertices
- Hinzufügen von Code zum Zeichnen
- Mit einer Textur Rendern
- Ändern die Texturkoordinaten
- Rendern von Vertices mit Modellen

Das fertige Projekt enthält ein kariertes Floor, die Verwendung eines Arrays Scheitelpunkt gezeichnet werden:

![](part2-images/image3.png "Das fertige Projekt wird ein kariertes Floor enthalten, die Verwendung eines Arrays Scheitelpunkt gezeichnet werden")

## <a name="creating-a-project"></a>Erstellen eines Projekts

Zunächst werden wir ein Projekt herunterladen, das als unsere Ausgangspunkt dient. Wir verwenden das Modellprojekt [finden Sie hier](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

Nachdem Sie heruntergeladen und extrahiert haben, öffnen Sie, und führen Sie das Projekt. Wir erwarten, dass sechs Roboter-Modelle, die auf dem Bildschirm gezeichnet werden, finden Sie unter:

![](part2-images/image4.png "Sechs Roboter-Modelle, die auf dem Bildschirm gezeichnet werden")

Am Ende dieses Projekts wir müssen werden unserer eigenen benutzerdefinierten Vertex-Rendering mit kombinieren des Roboters `Model`, weshalb wir sind nicht so löschen Sie den Code zum Rendern der Roboter. Stattdessen werden wir löschen Sie einfach die `Game1.Draw` Aufruf an die Zeichnung der 6 Roboter vorerst zu entfernen. Zu diesem Zweck öffnen Sie die **Game1.cs** Datei, und suchen Sie die `Draw` Methode. Ändern Sie sie aus, sodass sie den folgenden Code enthält:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Dies führt in unser Spiel, das einen blauen Bildschirm angezeigt:

![](part2-images/image5.png "Dies führt zu das Spiel, das einen blauen Bildschirm anzeigen")

## <a name="creating-the-vertices"></a>Erstellen die Vertices

Erstellen wir ein Array von Scheitelpunkten, um unsere Geometrie zu definieren. In dieser exemplarischen Vorgehensweise werden wir eine 3D-Ebene (ein Quadrat im 3D-Raum, keines Flugzeugs) erstellen. Obwohl unsere Ebene vier Seiten und vier Ecken verfügt, wird er aus zwei Dreiecken bestehen jeweils drei Vertices erfordert. Aus diesem Grund werden wir insgesamt sechs Punkte definiert.

Bisher haben wir zu den Scheitelpunkten in einem allgemeinen Sinn sprechen wurde allerdings MonoGame bietet einige standard Strukturen, die für Vertices verwendet werden können:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Namen des Typs gibt an, die darin enthaltenen Komponenten. Z. B. `VertexPositionColor` enthält Werte für die Position und Farbe. Betrachten wir nun jede der Komponenten:

- Position – alle Vertextypen gehören eine `Position` Komponente. Die `Position` Werte definieren, wo sich der Scheitelpunkt im 3D-Raum (X, Y und Z) befindet.
- Farbe – Vertices können optional angeben, ein `Color` Wert zum Ausführen benutzerdefinierter Farben.
- Normal: Normals definieren Sie, wie die Oberfläche des Objekts zeigt. Normals sind erforderlich, wenn ein Objekt mit Beleuchtung, seit die Richtung, der rendering eine Oberfläche wie hell Auswirkungen auf die zeigt, die es empfängt. Normals werden in der Regel angegeben, wie eine *Einheitsvektor* – einen 3D-Vektor, die eine Länge von 1.
- Textur – verweist die Textur auf Texturkoordinaten – d. h. der Teil einer Textur, die an einen bestimmten Vertex angezeigt werden sollen. Textur-Werte sind erforderlich, wenn ein 3D-Objekt mit einer Textur zu rendern. Texturkoordinaten sind normalisierte Koordinaten, was bedeutet, dass die Werte zwischen 0 und 1 liegen werden. Die Texturkoordinaten weiter unten in diesem Handbuch ausführlicher erläutern.

Unsere Ebene dient als eine Etage, und wir bei unseren rendern, daher wir verwenden anwenden möchten die `VertexPositionTexture` zu unserer Vertices zu definierenden Typs an.

Zunächst fügen wir einen Member auf unsere Game1-Klasse:

```csharp
VertexPositionTexture[] floorVerts; 
```

Als Nächstes definieren Sie unsere Scheitelpunkte im `Game1.Initialize`. Beachten Sie, die die bereitgestellte Vorlage, die weiter oben in diesem Artikel erwähnten keine enthält eine `Game1.Initialize` -Methode auf, daher wir die gesamte Methode zum hinzufügen müssen `Game1`:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

Damit können visualisieren, wie unsere Vertices dargestellt werden, berücksichtigen Sie die folgende Abbildung aus:

![](part2-images/image6.png "Damit können visualisieren, wie die Scheitelpunkte dargestellt werden, sollten Sie in diesem Diagramm")

In diesem Fall müssen wir eine verlassen sich auf unser Diagramm, um die Scheitelpunkte zu visualisieren, bis es abgeschlossen sind, implementieren unseren Code zum Rendern.

## <a name="adding-drawing-code"></a>Hinzufügen von Zeichencodes

Nun, wir die Positionen für unsere Geometrie definiert haben, können wir unsere Renderingcode schreiben.

Zunächst müssen wir definieren eine `BasicEffect` -Instanz, die Parameter für das Rendern, wie z. B. die Position und die Beleuchtung enthalten soll. Zu diesem Zweck fügen eine `BasicEffect` Member der `Game1` folgenden Where Klasse die `floorVerts` Feld definiert ist:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Ändern Sie als Nächstes die `Initialize` Methode, um die Auswirkungen zu definieren:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Jetzt können wir Code zum Ausführen der Zeichnung hinzufügen:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start 
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

Müssen wir Aufrufen `DrawGround` in unserer `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

Die app wird beim ausgeführt Folgendes angezeigt:

![](part2-images/image7.png "Zeigt die app diese bei Ausführung")

Wir sehen uns einige Details im oben stehenden Code.

### <a name="view-and-projection-properties"></a>Anzeigen und Eigenschaften

Die `View` und `Projection` Eigenschaften steuern, wie wir die Anzeige der Szene. Wir werden diesen Code später ändern, wenn wir den Modell-Rendering-Code erneut hinzufügen. Insbesondere `View` steuert den Speicherort und die Ausrichtung der Kamera, und `Projection` Steuerelemente der *Sichtfeld* (das kann verwendet werden, die Kamera zu vergrößern).

### <a name="techniques-and-passes"></a>Techniken und übergibt

Einmal haben wir zugewiesene Eigenschaften zu unserer Auswirkungen, die wir ausführen kann das eigentliche Rendern. 

Es wird nicht geändert werden kann die `CurrentTechnique` -Eigenschaft in dieser exemplarischen Vorgehensweise, aber mehr erweiterte Spiele handelt es sich möglicherweise um einen einzelnen Effekt der durchführen kann, Zeichnen auf unterschiedliche Weise (z. B. wie der Farbwert angewendet wird). Jedem dieser Renderingmodi kann als Verfahren dargestellt werden, die vor dem Rendern zugewiesen werden können. Darüber hinaus kann jede Technik mehrere Durchgänge um ordnungsgemäß zu rendern erfordern. Effekte möglicherweise mehrere Durchläufe aus, wenn komplexe visuelle Elemente, z. B. ein leuchtendes Oberfläche oder Fell zu rendern.

Wichtig zu bedenken ist, die die `foreach` -Schleife können Sie die gleichen C# Code zum Rendern von keinerlei Auswirkungen, unabhängig von der Komplexität der zugrunde liegenden `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` ist, in denen die Scheitelpunkte gerendert werden. Der erste Parameter gibt der Methode wie wir unsere Vertices organisiert haben. Wir haben sie so strukturiert, dass jedes Dreiecks durch drei Vertices geordnete, definiert ist, verwenden wir die `PrimitiveType.TriangleList` Wert.

Der zweite Parameter ist das Array von Vertices, die wir zuvor definiert haben.

Der dritte Parameter gibt den ersten Index zu zeichnen. Da wir unsere gesamte Vertex Array gerendert werden soll, übergeben wir den Wert 0.

Abschließend geben wir wie viele Dreiecke zu rendern. Unseren Vertex-Array enthält zwei Dreiecken, übergeben Sie daher den Wert 2.

## <a name="rendering-with-a-texture"></a>Mit einer Textur Rendern

An diesem Punkt wird die app eine weiße Fläche (perspektivisch) gerendert. Als Nächstes fügen wir eine Textur in unserem Projekt verwendet werden, wenn unsere Ebene rendern. 

Aus Gründen der Einfachheit fügen wir der PNG direkt auf das Projekt, statt mit dem Tool MonoGame-Pipeline. Zu diesem Zweck herunterladen [diese PNG-Datei](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) auf Ihrem Computer. Nach dem Herunterladen, mit der Maustaste auf die **Content** Ordner in der Projektmappe, und wählen Sie **hinzufügen > Dateien hinzufügen...**  . Wenn unter Android arbeiten möchten, klicken Sie dann in diesem Ordner befinden sich unter dem **Assets** Ordner im Android-spezifische Projekt. Wenn auf iOS, klicken Sie dann in diesem Ordner im Stammverzeichnis des iOS-Projekt befindet. Navigieren Sie zu dem Verzeichnis, in denen **checkerboard.png** wird gespeichert, und wählen Sie diese Datei. Wählen Sie die Datei in das Verzeichnis zu kopieren.

Als Nächstes fügen wir den Code zum Erstellen unserer `Texture2D` Instanz. Fügen Sie zunächst die `Texture2D` als Mitglied der `Game1` unter der `BasicEffect` Instanz:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Ändern Sie `Game1.LoadContent` wie folgt:


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Ändern Sie als Nächstes die `DrawGround` Methode. Die einzige Änderung, die erforderlich ist, weisen Sie `effect.TextureEnabled` zu `true` und Festlegen der `effect.Texture` zu `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    // new code:
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
```

Abschließend müssen wir ändern die `Game1.Initialize` Methode, um die Textur zuweisen koordiniert werden, auf unsere Vertices:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Wenn wir den Code ausführen, sehen wir, dass unsere Ebene jetzt einem Schachbrettmuster angezeigt:

![](part2-images/image8.png "Die Ebene wird jetzt ein Schachbrettmuster angezeigt.")

## <a name="modifying-texture-coordinates"></a>Ändern die Textur-Koordinaten

MonoGame verwendet normalisiert Texturkoordinaten, die Koordinaten, die zwischen 0 und 1 und nicht zwischen 0 und der Textur Breite oder Höhe sind. Im folgende Diagramm können normalisierte Koordinaten zu visualisieren:

![](part2-images/image9.png "In diesem Diagramm können normalisierte Koordinaten zu visualisieren.")

Normalisierte Texturkoordinaten ermöglichen Textur Ändern der Größe, ohne Code schreiben, oder erstellen Sie Modelle (z. B. .fbx) erneut. Dies ist möglich, da die normalisierte Koordinaten ein Verhältnis statt bestimmter Pixel darstellen. Zum Beispiel (1,1) wird immer darstellen der rechten unteren Ecke unabhängig von der Texturgröße.

Ändern wir die Zuweisung der Textur-Koordinate um eine einzelne Variable für die Anzahl von Wiederholungen zu verwenden:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

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

    base.Initialize ();
}
```

Dadurch wird in der Textur 20-Mal wiederholen:

![](part2-images/image10.png "Dadurch wird die Textur, die 20-Mal wiederholen")


## <a name="rendering-vertices-with-models"></a>Rendern von Vertices mit Modellen

Nun, da unsere Ebene ordnungsgemäß gerendert wird, können wir die Modelle aus, um alles anzuzeigen erneut hinzufügen. Wir werden erneut fügen Sie zunächst die Model-Code, um unsere `Game1.Draw` -Methode (mit geänderten Positionen):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Außerdem erstellen wir eine `Vector3` in `Game1` der Kamera Position angeben. Wir fügen ein Feld in unserer `checkerboardTexture` Deklaration:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

Entfernen Sie danach das lokale `cameraPosition` Variable aus der `DrawModel` Methode:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

Auf ähnliche Weise entfernen den lokalen `cameraPosition` Variable aus der `DrawGround` Methode:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Jetzt beim Ausführen des Codes sehen sowohl die Modelle und Grund zur gleichen Zeit wir:

![](part2-images/image11.png "Sowohl die Modelle und der Boden werden zur gleichen Zeit angezeigt.")

Wenn wir die Position (Kamera) ändern (z. B. durch das Erhöhen der X-Wert, der in diesem Fall wird der Kamera auf der linken Seite) sehen, dass der Wert wirkt sich sowohl auf dem Boden und die Modelle auf:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Dieser Code hat folgende Auswirkungen:

![](part2-images/image3.png "Dieser Code führt in dieser Ansicht")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie ein Vertex-Array verwenden, um benutzerdefiniertes Rendering durchzuführen. In diesem Fall wir ein kariertes Floor erstellt, durch die Kombination von unseren Vertex-basierte Rendering mit einer Textur und `BasicEffect`, aber der Code der hier vorgestellten dient als Grundlage für alle 3D-Rendering. Wir zeigen auch, dass die Vertex-basierte Rendering von Modellen in einer Szene gemischt werden kann.

## <a name="related-links"></a>Verwandte Links

- [Schachbretts an Datei (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Abgeschlossenes Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
