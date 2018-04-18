---
title: Zeichnen 3D-Grafiken mit Scheitelpunkte in MonoGame
description: MonoGame unterstützt das Verwenden von Arrays an Scheitelpunkten definieren, wie ein 3D-Objekt regelmäßig pro Punkt gerendert wird. Benutzer können Vertex-Arrays erstellen dynamische Geometry, Spezialeffekte zu implementieren und verbessern die Effizienz ihrer Rendering über culling nutzen.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4736bedd413663af098bbad522cc56f432e36ea0
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Zeichnen 3D-Grafiken mit Scheitelpunkte in MonoGame

_MonoGame unterstützt das Verwenden von Arrays an Scheitelpunkten definieren, wie ein 3D-Objekt regelmäßig pro Punkt gerendert wird. Benutzer können Vertex-Arrays erstellen dynamische Geometry, Spezialeffekte zu implementieren und verbessern die Effizienz ihrer Rendering über culling nutzen._

Benutzer, die über Leseberechtigungen der [Leitfaden auf das Rendern von Modellen](~/graphics-games/monogame/3d/part1.md) wird mit einem 3D-Modell in MonoGame Rendern vertraut sein. Die `Model` Klasse ist eine effektive Methode zum Rendern von 3D-Grafiken beim Arbeiten mit Daten in einer Datei (z. B. .fbx) definiert, und beim Umgang mit statischen Daten. Einige Spiele erfordern 3D Geometry definiert oder dynamisch zur Laufzeit geändert werden. In diesen Fällen können wir verwenden von Arrays mit *Scheitelpunkte* definieren und Geometrie rendern. Ein Vertex ist ein allgemeiner Begriff für einen bestimmten 3D-Bereich gehört eine geordnete Liste mit der Geometrie definiert. In der Regel werden Scheitelpunkte in so, dass eine Reihe von Dreiecken definieren sortiert.

Betrachten Sie zum Visualisieren wie Scheitelpunkte zum Erstellen von 3D-Objekten verwendet werden, die folgenden Kugel:

![](part2-images/image1.png "Visuell darstellen, wie Scheitelpunkte zum Erstellen von 3D-Objekten verwendet werden, beachten Sie, um diese Kugel")

Wie oben gezeigt, besteht die Kugel deutlich mehrere Dreiecke. Wir sehen die Drahtmodell der Kugel, um festzustellen, wie der Scheitelpunkte Formular Dreiecke herstellen:

![](part2-images/image2.png "Anzeigen der Drahtmodell der Kugel, um festzustellen, wie der Scheitelpunkte Formular Dreiecke herstellen")

In dieser exemplarischen Vorgehensweise werden die folgenden Themen behandelt:

- Erstellen eines Projekts
- Erstellen die Scheitelpunkten
- Hinzufügen von Code zum Zeichnen
- Mit einer Textur Rendern
- Ändern von Texturkoordinaten
- Rendern von Scheitelpunkte mit Modellen

Fertig gestellten Projekts wird eine Karierte Etage enthalten, darunter die Verwendung eines Arrays Scheitelpunkt gezeichnet werden:

![](part2-images/image3.png "Fertig gestellten Projekts wird eine Karierte Etage enthalten, die Verwendung eines Arrays Scheitelpunkt gezeichnet wird")

## <a name="creating-a-project"></a>Erstellen eines Projekts

Heruntergeladene wir zunächst ein Projekt das als unsere Anfangspunkt dient. Wir verwenden das Modellprojekt [die finden Sie hier](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

Nachdem Sie heruntergeladen und extrahiert haben, öffnen Sie, und führen Sie das Projekt. Wir erwarten, dass sechs Roboter-Modelle, die auf dem Bildschirm gezeichnet werden angezeigt:

![](part2-images/image4.png "Sechs Roboter-Modelle, die auf dem Bildschirm gezeichnet werden")

Am Ende dieses Projekts wir müssen werden Kombinieren von eigenen benutzerdefinierten Vertex-Rendering mit des Roboters `Model`, sodass wir sind nicht Renderingcodes Roboter löschen möchten. Wir müssen stattdessen einfach löschen der `Game1.Draw` Aufruf des Zeichnens des 6 Roboter vorerst zu entfernen. Öffnen Sie hierzu die **Game1.cs** Datei, und suchen Sie die `Draw` Methode. Ändern Sie sie aus, damit sie den folgenden Code enthält:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Dies führt zu unserem Spiel einen blauen Bildschirm anzeigen:

![](part2-images/image5.png "Dies führt im Spiel bisher einen blauen Bildschirm anzeigen")

## <a name="creating-the-vertices"></a>Erstellen die Scheitelpunkten

Es wird ein Array von Scheitelpunkte unsere Geometry definieren erstellt. In dieser exemplarischen Vorgehensweise fügen wir eine 3D Ebene (ein Quadrat in einem 3D-Bereich, kein Flugzeug) erstellen. Obwohl unsere Ebene vier Seiten und die vier Ecken hat, wird er zwei Dreiecke erstellt werden, wobei bei jedem der drei Scheitelpunkte erfordert. Aus diesem Grund wird wir insgesamt sechs Punkte definiert werden.

Bisher haben wir über Scheitelpunkten in einem allgemeinen Sinn sprechen wurde, aber MonoGame bietet einige standard Strukturen, die für Scheitelpunkte verwendet werden können:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Name des Typs gibt an, die darin enthaltenen Komponenten. Beispielsweise `VertexPositionColor` enthält Werte für die Position und Farbe. Betrachten wir nun jede der Komponenten:

- Position – alle Vertextypen gehören eine `Position` Komponente. Die `Position` Werte definieren, wo die Vertex im 3D-Bereich (X, Y und Z) befindet.
- Farbe: Scheitelpunkte können optional angeben, ein `Color` Wert zum Ausführen benutzerdefinierter Farben.
- Normal – normalen definieren, wie die Oberfläche des Objekts mit Internetzugriff wird. Normale sind erforderlich, wenn ein Objekt mit Beleuchtung, seit die Richtung, das Rendern eine Oberfläche wirkt sich darauf aus welchen hell mit Internetzugriff wird empfangenen. Normale werden in der Regel angegeben, wie eine *Einheitsvektor* – 3D Vektor mit einer Länge von 1.
- Textur bezeichnet Textur Texturkoordinaten – d. h. Teil einer Textur an einen bestimmten Vertex angezeigt werden sollen. Texture-Werte sind erforderlich, wenn ein 3D-Objekt mit einer Textur zu rendern. Texturkoordinaten sind normalisierte Koordinaten, was bedeutet, dass Werte zwischen 0 und 1 liegen werden. Texturkoordinaten im Detail weiter unten in diesem Handbuch beschrieben.

Unsere Ebene dient als eine Etage, und wir möchten eine Textur gelten bei unserer rendern, daher wir verwenden die `VertexPositionTexture` zu unserem Scheitelpunkte definierenden Typs an.

Zunächst fügen wir unserer Game1-Klasse ein Element:

```csharp
VertexPositionTexture[] floorVerts; 
```

Als Nächstes definieren Sie unsere Scheitelpunkten in `Game1.Initialize`. Beachten Sie, die die bereitgestellte Vorlage, die weiter oben in diesem Artikel verwiesen wird keine enthält eine `Game1.Initialize` Methode, daher muss die gesamte Methode hinzuzufügen `Game1`:

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

Visualisieren unsere Scheitelpunkte aussehen, beachten Sie, um das folgende Diagramm:

![](part2-images/image6.png "Visualisieren der Scheitelpunkte aussehen, beachten Sie, um dieses Diagramm")

Verlassen Sie sich auf unser Diagramm an die Scheitelpunkten visuell darstellen, bis wir haben unsere Renderingcodes implementieren müssen.

## <a name="adding-drawing-code"></a>Hinzufügen von Zeichencodes

Nun, dass wir die Positionen für unsere Geometrie definiert haben, können wir unsere Renderingcode schreiben.

Wir müssen zunächst definieren Sie eine `BasicEffect` -Instanz, die Parameter für das Rendern, z. B. die Position und die Beleuchtung aufnimmt. Zu diesem Zweck fügen eine `BasicEffect` Member der `Game1` Klasse unten Where der `floorVerts` Feld definiert ist:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Als Nächstes ändern Sie die `Initialize` Methode, um die Auswirkungen zu definieren:

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
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Jetzt können Sie Code zum Ausführen der Zeichnung hinzufügen:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
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

Wir müssen Aufrufen `DrawGround` in unserer `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

Die app wird beim Ausführen folgender angezeigt:

![](part2-images/image7.png "Die app zeigt dies bei der Ausführung")

Sehen wir uns einige Details im obigen Code an.

### <a name="view-and-projection-properties"></a>Anzeigen und Projektion von Eigenschaften

Die `View` und `Projection` Eigenschaften steuern, wie es die Szene anzeigen. Wir müssen diesen Code später ändert Wenn wir Renderingcodes Modell erneut hinzufügen. Insbesondere `View` steuert den Speicherort und die Ausrichtung der Kamera und `Projection` Steuerelemente der *Blickfeld* (die genutzt werden die Kamera vergrößern).

### <a name="techniques-and-passes"></a>Techniken und übergibt

Einmal haben wir zugewiesene Eigenschaften auf unsere Auswirkungen, die ausgeführt werden kann das tatsächliche Rendern. 

Es wird nicht geändert werden die `CurrentTechnique` Eigenschaft in dieser exemplarischen Vorgehensweise jedoch erweiterte Spiele handelt es sich möglicherweise um einen einzelnen Effekt die Zeichnung auf unterschiedliche Weise (z. B. wie der Farbwert angewendet wird) ausführen kann. Jedem dieser Rendermodi kann als eine Technik dargestellt werden, die vor dem Rendering zugewiesen werden können. Darüber hinaus möglicherweise jede Technik mehrere Durchläufe auszuführen, um ordnungsgemäß zu rendern. Effekte möglicherweise mehrere Durchgänge erforderlich, wenn es sich bei komplexen visuelle Elemente, z. B. eine leuchtende Fläche oder hinterste zu rendern.

Wichtig zu bedenken ist, die die `foreach` Schleife ermöglicht den gleichen C#-Code zum Rendern von unwirksam unabhängig von der Komplexität der zugrunde liegenden `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` ist, an die Scheitelpunkten gerendert werden. Der erste Parameter gibt der Methode wie wir unsere Scheitelpunkte organisiert sind. Wir haben sie so strukturiert, dass jedes Dreieck durch drei geordnete Scheitelpunkte, wobei definiert ist, damit wir verwenden die `PrimitiveType.TriangleList` Wert.

Der zweite Parameter ist das Array der Scheitelpunkte, die wir zuvor definiert.

Der dritte Parameter gibt den ersten Index gezeichnet werden soll. Da wir unsere gesamte Vertex-Array, das gerendert werden soll, müssen wir den Wert 0 übergeben.

Schließlich geben wir an wie viele Dreiecke gerendert. Unsere Vertex-Array enthält zwei Dreiecken, übergeben Sie daher den Wert 2.

## <a name="rendering-with-a-texture"></a>Mit einer Textur Rendern

Zu diesem Zeitpunkt wird dieser app eine weiße Ebene (in der Perspektive) gerendert. Als Nächstes fügen wir eine Textur unsere Projekt beim Rendern von unserer Ebene verwendet werden soll. 

Der Einfachheit halber fügen wir der PNG direkt an unsere Projekt statt mit dem Tool MonoGame Pipeline. Zu diesem Zweck herunterladen [dieser PNG-Datei](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) auf Ihrem Computer. Nach dem Herunterladen, mit der Maustaste auf die **Content** Ordner in der Projektmappe aufgefüllt, und wählen **hinzufügen > Hinzufügen von Dateien...**  . Wenn auf Android-Geräten zu arbeiten, klicken Sie dann in diesem Ordner befinden sich unter dem **Bestand** Ordner des Projekts für Android-spezifische. Wenn es sich um iOS, wird dieser Ordner im Stammverzeichnis des Projekts für iOS sein. Navigieren Sie zu dem Verzeichnis, in dem **checkerboard.png** gespeichert wird, und wählen Sie diese Datei. Wählen Sie die Datei in das Verzeichnis kopieren.

Als Nächstes fügen wir den Code zum Erstellen unserer `Texture2D` Instanz. Fügen Sie zunächst die `Texture2D` als Mitglied der `Game1` unter der `BasicEffect` Instanz:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Ändern Sie `Game1.LoadContent` wie folgt:


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Als Nächstes ändern Sie die `DrawGround` Methode. Die einzige Änderung, die erforderlich ist, weisen Sie `effect.TextureEnabled` auf `true` und Festlegen der `effect.Texture` auf `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    // new code:
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
```

Schließlich müssen so ändern Sie die `Game1.Initialize` Methode zum Textur auch zuweisen koordiniert auf unserem Scheitelpunkte:


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

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Wenn wir den Code ausführen, sehen wir, dass unsere Ebene jetzt einem Schachbrettmuster angezeigt:

![](part2-images/image8.png "Die Ebene wird nun einem Schachbrettmuster angezeigt.")

## <a name="modifying-texture-coordinates"></a>Ändern der Textur-Koordinaten

MonoGame verwendet normalisiert Texturkoordinaten Koordinaten, die zwischen 0 und 1 statt zwischen 0 und die Textur Breite oder Höhe sind. Das folgende Diagramm können Sie normalisierte Koordinaten visualisieren:

![](part2-images/image9.png "In diesem Diagramm kann visualisieren normalisierte Koordinaten")

Normalisierte Texturkoordinaten ermöglichen Textur Größe ändern, ohne Code schreiben oder neu erstellen Modelle (z. B. .fbx-Dateien). Dies ist möglich, da normalisierte Koordinaten ein Verhältnis anstelle von bestimmten Pixel darstellen. Zum Beispiel (1,1) werden immer darstellen der unteren rechten Ecke unabhängig von der Texturgröße.

Die Zuweisung der Textur-Koordinate um eine einzelne Variable zu verwenden, für die Anzahl von Wiederholungen kann geändert werden:


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

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Dadurch wird die Textur 20-Mal wiederholen:

![](part2-images/image10.png "Dadurch wird die Textur 20-Mal wiederholen")


## <a name="rendering-vertices-with-models"></a>Rendern von Scheitelpunkte mit Modellen

Nun, dass unsere Ebene ordnungsgemäß gerendert wird, können wir die Modelle, um alles zusammen anzeigen erneut hinzufügen. Wir müssen erneut fügen Sie zunächst die Model-Code, um unsere `Game1.Draw` -Methode (mit geänderten Positionen):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Wir erstellen außerdem eine `Vector3` in `Game1` der Kamera Position angeben. Wir fügen ein Feld in unserer `checkerboardTexture` Deklaration:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

Als Nächstes entfernen den lokalen `cameraPosition` Variable aus der `DrawModel` Methode:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

Auf ähnliche Weise entfernen den lokalen `cameraPosition` Variable aus der `DrawGround` Methode:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Jetzt beim Ausführen des Codes können wir die Modelle und Grund zur gleichen Zeit finden Sie unter:

![](part2-images/image11.png "Die Modelle und Grund werden zur gleichen Zeit angezeigt.")

Wenn wir die Kameraposition ändern (z. B. infolge einer Erhöhung der X-Wert, der in diesem Fall wechselt der Kamera auf der linken Seite) ersichtlich, dass der Wert der Erdoberfläche und die Modelle wirkt sich auf:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Dieser Code ergibt sich Folgendes:

![](part2-images/image3.png "Dieser Code führt zu dieser Ansicht")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie eine Vertex-Array verwenden, um benutzerdefinierte Rendering ausführen. In diesem Fall erstellt eine Karierte Etage durch Kombinieren von unseren Vertex-basierte Rendering mit einer Textur und `BasicEffect`, aber der Code der hier vorgestellten dient als Grundlage für alle 3D-Rendering. Wir ergab auch, dass die Vertex-basierte Rendering von Modellen in der gleichen Szene kombiniert werden kann.

## <a name="related-links"></a>Verwandte Links

- [Schachbrettfeld-Datei (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Abgeschlossene Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
