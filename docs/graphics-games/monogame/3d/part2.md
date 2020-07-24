---
title: Zeichnen von 3D-Grafiken mit Vertices in monogame
description: Monogame unterstützt die Verwendung von Array-Scheitel Punkten, um zu definieren, wie ein 3D-Objekt pro Punkt gerendert wird. Benutzer können Vertex-Arrays nutzen, um dynamische Geometrie zu erstellen, besondere Effekte zu implementieren und die Effizienz Ihres Rendering durch die Erstellung zu verbessern.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: dfb03815f8642519cecf49ab7b626b9575821af1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937630"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Zeichnen von 3D-Grafiken mit Vertices in monogame

_Monogame unterstützt die Verwendung von Array-Scheitel Punkten, um zu definieren, wie ein 3D-Objekt pro Punkt gerendert wird. Benutzer können Vertex-Arrays nutzen, um dynamische Geometrie zu erstellen, besondere Effekte zu implementieren und die Effizienz Ihres Rendering durch die Erstellung zu verbessern._

Benutzer, die sich mit dem [Leitfaden zu renderingmodellen](~/graphics-games/monogame/3d/part1.md) vertraut gemacht haben, sind mit dem Rendern eines 3D-Modells in monogame vertraut. Die `Model` -Klasse ist eine effektive Möglichkeit, 3D-Grafiken beim Arbeiten mit Daten, die in einer Datei (z. b.. fbx) definiert sind, und im Umgang mit statischen Daten darzustellen. Einige Spiele erfordern, dass 3D-Geometrie zur Laufzeit dynamisch definiert oder bearbeitet wird. In diesen Fällen können mithilfe von *Vertices* -Arrays Geometrie definiert und dargestellt werden. Ein Scheitelpunkt ist ein allgemeiner Begriff für einen Punkt im 3D-Raum, der Teil einer geordneten Liste ist, die zum Definieren von Geometrie verwendet wird. Üblicherweise sind Scheitel Punkte so angeordnet, dass Sie eine Reihe von Dreiecken definieren.

Um die Verwendung von Scheitel Punkten beim Erstellen von 3D-Objekten zu veranschaulichen, betrachten wir die folgende Kugel:

![Um zu verdeutlichen, wie Scheitel Punkte zum Erstellen von 3D-Objekten verwendet werden, sehen Sie sich diese Kugel an](part2-images/image1.png)

Wie oben gezeigt, besteht die Kugel eindeutig aus mehreren Dreiecken. Wir können den Draht Modell der Kugel anzeigen, um zu sehen, wie die Scheitel Punkte mit Form Dreiecken verbunden werden:

![Zeigen Sie den Draht Modell der Kugel an, um zu sehen, wie die Scheitel Punkte mit Formular Dreiecken verbunden werden](part2-images/image2.png)

In dieser exemplarischen Vorgehensweise werden die folgenden Themen behandelt:

- Erstellen eines Projekts
- Erstellen der Vertices
- Hinzufügen von Zeichnungs Code
- Rendering mit einer Textur
- Ändern von Texturkoordinaten
- Rendern von Vertices mit Modellen

Das fertige Projekt enthält einen überprüfenden Boden, der mithilfe eines Scheitelpunkt Arrays gezeichnet wird:

![Das fertige Projekt enthält einen überprüfenden Boden, der mithilfe eines Scheitelpunkt Arrays gezeichnet wird.](part2-images/image3.png)

## <a name="creating-a-project"></a>Erstellen eines Projekts

Zuerst wird ein Projekt heruntergeladen, das als Ausgangspunkt fungiert. Wir verwenden das Modellprojekt [, das hier zu finden ist](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelrenderingmg/).

Öffnen Sie das Projekt, und führen Sie es aus. Es wird erwartet, dass sechs Robotermodelle auf dem Bildschirm gezeichnet werden:

![Auf dem Bildschirm gezeichnete sechs Robotermodelle](part2-images/image4.png)

Am Ende dieses Projekts kombinieren wir unser eigenes benutzerdefiniertes Scheitelpunkt Rendering mit dem Roboter `Model` , sodass wir den roboterrenderingcode nicht löschen werden. Stattdessen löschen wir einfach den-Befehl `Game1.Draw` , um das Zeichnen der 6-Roboter zu entfernen. Öffnen Sie hierzu die Datei **Game1.cs** , und suchen Sie die- `Draw` Methode. Ändern Sie die Datei so, dass Sie den folgenden Code enthält:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Dies führt dazu, dass unser Spiel einen leeren blauen Bildschirm anzeigt:

![Dies führt dazu, dass das Spiel einen leeren blauen Bildschirm anzeigt.](part2-images/image5.png)

## <a name="creating-the-vertices"></a>Erstellen der Vertices

Wir erstellen ein Array von Vertices, um unsere Geometrie zu definieren. In dieser exemplarischen Vorgehensweise wird eine 3D-Ebene erstellt (ein Quadrat in 3D-Raum, kein Flugzeug). Obwohl unsere Ebene vier Seiten und vier Ecken hat, besteht Sie aus zwei Dreiecken, von denen jede drei Vertices erfordert. Daher werden insgesamt sechs Punkte definiert.

Bisher haben wir über Vertices in allgemeiner Hinsicht gesprochen, aber monogame bietet einige Standard Strukturen, die für Vertices verwendet werden können:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Der Name jedes Typs gibt die enthaltenen Komponenten an. Beispielsweise `VertexPositionColor` enthält Werte für die Position und die Farbe. Sehen wir uns die einzelnen Komponenten an:

- Position – alle Scheitelpunkt Typen enthalten eine `Position` Komponente. Die `Position` Werte definieren, wo sich der Scheitelpunkt im 3D-Raum (X, Y und Z) befindet.
- Color – Vertices können optional einen `Color` Wert angeben, um benutzerdefinierte Tinting auszuführen.
- Normal – Normals definieren, auf welche Weise die Oberfläche des Objekts sichtbar ist. Normale sind erforderlich, wenn ein Objekt mit Beleuchtung nach der Richtung gerendert wird, in der eine Oberfläche angezeigt wird. Normale werden in der Regel als *Einheits Vektor* angegeben – ein 3D-Vektor mit einer Länge von 1.
- Textur – Textur bezieht sich auf Texturkoordinaten – d. h., welcher Teil einer Textur an einem bestimmten Scheitelpunkt angezeigt werden soll. Textur Werte sind erforderlich, wenn ein 3D-Objekt mit einer Textur gerendert wird. Texturkoordinaten sind normalisierte Koordinaten. Dies bedeutet, dass die Werte zwischen 0 und 1 liegen. Die Texturkoordinaten werden später in diesem Handbuch ausführlicher behandelt.

Unsere Ebene dient als Fußboden, und wir möchten eine Textur anwenden, wenn wir unser Rendering durchführen. Daher verwenden wir den-Typ, um unsere Scheitel Punkte `VertexPositionTexture` zu definieren.

Zuerst fügen wir der Game1-Klasse einen Member hinzu:

```csharp
VertexPositionTexture[] floorVerts;
```

Definieren Sie als nächstes die Vertices in `Game1.Initialize` . Beachten Sie, dass die bereitgestellte Vorlage, auf die weiter oben in diesem Artikel verwiesen `Game1.Initialize` wird, keine-Methode enthält, daher müssen wir die gesamte-Methode hinzufügen `Game1` :

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

Um zu verdeutlichen, wie unsere Scheitel Punkte aussehen, betrachten Sie das folgende Diagramm:

![Um zu verdeutlichen, wie die Scheitel Punkte aussehen, betrachten Sie dieses Diagramm.](part2-images/image6.png)

Wir müssen uns auf unser Diagramm stützen, um die Scheitel Punkte zu visualisieren, bis wir die Implementierung unseres Renderingcodes abgeschlossen haben.

## <a name="adding-drawing-code"></a>Hinzufügen von Zeichnungs Code

Nachdem wir nun die Positionen für die Geometrie definiert haben, können wir unseren Renderingcode schreiben.

Zuerst müssen wir eine-Instanz definieren, `BasicEffect` die Parameter für das Rendering enthält, wie z. b. Position und Beleuchtung. Fügen Sie zu diesem Zweck `BasicEffect` der Klasse unten ein Member hinzu, `Game1` in dem das `floorVerts` Feld definiert ist:

```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Ändern Sie als nächstes die- `Initialize` Methode, um den Effekt zu definieren:

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

Nun können wir Code hinzufügen, um die Zeichnung durchzuführen:

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

Wir müssen `DrawGround` in unserer `Game1.Draw` :

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

Die APP zeigt Folgendes an, wenn Sie ausgeführt wird:

![Diese wird von der App angezeigt, wenn Sie ausgeführt wird.](part2-images/image7.png)

Sehen wir uns einige Details des obigen Codes an.

### <a name="view-and-projection-properties"></a>Ansichts-und Projektions Eigenschaften

Die `View` -Eigenschaft und die-Eigenschaft `Projection` steuern die Anzeige der Szene. Wir ändern diesen Code später, wenn wir den modellrenderingcode erneut hinzufügen. Insbesondere `View` steuert die Position und Ausrichtung der Kamera und `Projection` steuert das *Feld der Ansicht* (das zum Vergrößern der Kamera verwendet werden kann).

### <a name="techniques-and-passes"></a>Techniken und Durchgänge

Nachdem wir unseren Effekten Eigenschaften zugewiesen haben, können wir das tatsächliche Rendering durchführen.

In dieser exemplarischen Vorgehensweise wird die-Eigenschaft nicht geändert `CurrentTechnique` , aber erweiterte Spiele können einen einzelnen Effekt haben, der das Zeichnen auf unterschiedliche Weise (z. b. wie der Farbwert angewendet wird) durchführen kann. Jeder dieser Renderingmodi kann als Technik dargestellt werden, die vor dem Rendering zugewiesen werden kann. Außerdem erfordert jede Technik möglicherweise, dass mehrere Durchgänge ordnungsgemäß dargestellt werden. Wenn komplexe visuelle Elemente wie eine leuchtende Oberfläche oder ein Fell gerendert werden, werden möglicherweise mehrere Durchgänge

Wichtig zu beachten ist, dass die- `foreach` Schleife es dem gleichen c#-Code ermöglicht, jede beliebige Auswirkung unabhängig von der Komplexität der zugrunde liegenden darzustellen `BasicEffect` .

### <a name="drawuserprimitives"></a>Drawuserprimitives

`DrawUserPrimitives`Gibt an, wo die Vertices gerendert werden. Der erste Parameter teilt der Methode mit, wie wir unsere Scheitel Punkte organisiert haben. Wir haben Sie so strukturiert, dass jedes Dreieck von drei geordneten Scheitel Punkten definiert wird, daher wird der `PrimitiveType.TriangleList` Wert verwendet.

Der zweite Parameter ist das Array von Vertices, das wir zuvor definiert haben.

Der dritte Parameter gibt den ersten Index an, der gezeichnet werden soll. Da das gesamte Scheitelpunkt Array gerendert werden soll, übergeben wir den Wert 0.

Schließlich geben wir an, wie viele Dreiecke zu Rendering sind. Unser Scheitelpunkt Array enthält zwei Dreiecke. übergeben Sie daher den Wert 2.

## <a name="rendering-with-a-texture"></a>Rendering mit einer Textur

An diesem Punkt rendert unsere App eine weiße Ebene (in der Perspektive). Als Nächstes fügen wir dem Projekt eine Textur hinzu, die beim Rendern unserer Ebene verwendet werden soll.

Um dies zu gewährleisten, fügen wir die PNG-Datei direkt in das Projekt ein, anstatt das monogame-Pipeline Tool zu verwenden. Laden Sie hierzu die [PNG-Datei](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) auf Ihren Computer herunter. Klicken Sie nach dem herunterladen mit der rechten Maustaste auf den **Inhalts** Ordner im lösungspad, und wählen Sie **Hinzufügen>Dateien hinzufügen** aus. Wenn Sie an Android arbeiten, befindet sich dieser Ordner unter dem Ordner **Assets** im Android-spezifischen Projekt. Wenn unter IOS, wird dieser Ordner im Stammverzeichnis des IOS-Projekts angezeigt. Navigieren Sie zu dem Verzeichnis, in dem **checkerboard.png** gespeichert ist, und wählen Sie diese Datei aus. Aktivieren Sie diese Option, um die Datei in das Verzeichnis zu kopieren.

Als Nächstes fügen wir den Code zum Erstellen der- `Texture2D` Instanz hinzu. Fügen Sie zunächst `Texture2D` als Member von `Game1` unter der- `BasicEffect` Instanz hinzu:

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

Ändern Sie als nächstes die- `DrawGround` Methode. Die einzige erforderliche Änderung ist die Zuweisung `effect.TextureEnabled` von zu und, um `true` den auf festzulegen `effect.Texture` `checkerboardTexture` :

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

Schließlich müssen wir die-Methode ändern `Game1.Initialize` , um auch Texturkoordinaten auf unseren Scheitel Punkten zuzuweisen:

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

Wenn wir den Code ausführen, sehen wir, dass unsere Ebene nun ein checkboard-Muster anzeigt:

![Die Ebene zeigt nun ein checkboard-Muster an.](part2-images/image8.png)

## <a name="modifying-texture-coordinates"></a>Ändern von Texturkoordinaten

Monogame verwendet normalisierte Texturkoordinaten, bei denen es sich um Koordinaten zwischen 0 und 1 anstelle von 0 und Breite oder Höhe der Textur handelt. Mithilfe des folgenden Diagramms können normalisierte Koordinaten visualisiert werden:

![Dieses Diagramm kann bei der Visualisierung normalisierter Koordinaten helfen.](part2-images/image9.png)

Normalisierte Texturkoordinaten ermöglichen die Größe der Textur, ohne Code neu schreiben oder Modelle neu erstellen zu müssen (z. b. fbx-Dateien). Dies ist möglich, weil normalisierte Koordinaten ein Verhältnis anstelle bestimmter Pixel darstellen. (1, 1) stellt z. b. unabhängig von der Textur Größe immer die untere rechte Ecke dar.

Wir können die Texturkoordinaten Zuweisung ändern, um eine einzelne Variable für die Anzahl der Wiederholungen zu verwenden:

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

Dies führt dazu, dass sich die Textur 20 Mal wiederholt:

![Dadurch wird die Textur 20-Mal wiederholt.](part2-images/image10.png)

## <a name="rendering-vertices-with-models"></a>Rendern von Vertices mit Modellen

Nachdem unsere Ebene ordnungsgemäß gerendert wurde, können Sie die Modelle erneut hinzufügen, um alles zusammenzusehen. Zuerst fügen wir den Modellcode zur- `Game1.Draw` Methode hinzu (mit geänderten Positionen):

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

Außerdem wird ein in erstellt `Vector3` `Game1` , um die Position der Kamera darzustellen. Wir fügen ein Feld unter der `checkerboardTexture` Deklaration hinzu:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10);
```

Entfernen Sie als nächstes die lokale `cameraPosition` Variable aus der- `DrawModel` Methode:

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

Entfernen Sie auf ähnliche Weise die lokale `cameraPosition` Variable aus der- `DrawGround` Methode:

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

Wenn wir nun den Code ausführen, sehen wir die Modelle und den Grund für die gleiche Zeit:

![Beide Modelle und der Boden werden gleichzeitig angezeigt.](part2-images/image11.png)

Wenn wir die Kameraposition ändern (z. b. indem wir den X-Wert erhöhen, der in diesem Fall die Kamera nach links verschiebt), können wir sehen, dass sich der Wert sowohl auf das Grund-als auch auf die Modelle auswirkt:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Dieser Code führt zu folgendem:

![Dieser Code führt zu dieser Ansicht.](part2-images/image3.png)

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde die Verwendung eines Scheitelpunkt Arrays zum Durchführen eines benutzerdefinierten Rendering veranschaulicht. In diesem Fall haben wir einen einstufigen Boden angelegt, indem wir unser Vertex-basiertes Rendering mit einer Textur und kombiniert `BasicEffect` haben. der hier dargestellte Code dient jedoch als Grundlage für jedes 3D-Rendering. Wir haben auch gezeigt, dass das Vertex-basierte Rendering mit Modellen in der gleichen Szene gemischt werden kann.

## <a name="related-links"></a>Verwandte Links

- [Checkboard-Datei (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Abgeschlossenes Projekt (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/)
