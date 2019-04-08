---
title: Verwenden von UrhoSharp zum Erstellen eines 3D-Spiels
description: Dieses Dokument enthält einen Überblick über die von UrhoSharp, beschreiben Szenen, Komponenten, Formen, Kameras, Aktionen, Benutzereingaben, Sound und vieles mehr.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 5e5c4f1545d39befde6574338ec4c1ca4037ad8b
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507161"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>Verwenden von UrhoSharp zum Erstellen eines 3D-Spiels

Bevor Sie Ihr erste Spiel schreiben, möchten Sie mit den Grundlagen vertraut zu erhalten: wie Ihrer Szene einrichten, um Ressourcen zu laden (Dies enthält Ihre Grafik) und um einfache Aktivitäten für Ihr Spiel zu erstellen.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Im Hintergrund, Knoten, Komponenten und Kameras

Das Modell für die Szene kann als szenengraph komponentenbasierter beschrieben werden. Die Szene besteht aus einer Hierarchie von Szenenknoten, beginnend mit den Stammknoten, der auch die gesamte Szene darstellt. Jede [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) verfügt über eine 3D-Transformation (Position, Drehung und Skalierung), einen Namen, eine ID sowie eine beliebige Anzahl von Komponenten.  Komponenten stellen eines Knotens zum Leben, sie können eine visuelle Darstellung hinzufügen ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), können sie Sound ausgeben ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), können sie eine Kollision Grenze usw. bieten.

Sie können Ihre Szenen und die Setup-Knoten, die mit erstellen die [Urho Editor](#urhoeditor), oder Sie können Aktionen von C#-Code.  In diesem Dokument wird alles mithilfe von Code erläutert, wie sie zeigen, dass die Elemente, die erforderlichen Aufgaben auf dem Bildschirm angezeigt werden

Zusätzlich zu Ihrer Szene eingerichtet haben, müssen Sie Setup eine [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), dadurch wird bestimmt, was dem Benutzer angezeigt erhalten wird.

### <a name="setting-up-your-scene"></a>Das Einrichten Ihrer Szene

In der Regel würden Sie dieses Formular Ihrer Start-Methode erstellen:

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

### <a name="components"></a>Komponenten

Rendern 3D-Objekte, Soundwiedergabe "," Physik "und" geskriptete Logik Updates sind alle aktiviert durch das Erstellen von verschiedenen Komponenten in die Knoten durch den Aufruf [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Richten Sie z. B. Ihre Knoten und light-Komponente wie folgt:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Wir haben über einen Knoten mit dem Namen "`DirectionalLight`" und eine Richtung für, sondern nichts anderes festgelegt.  Wir können nun den oben aufgeführten Knoten auf einen Knoten leuchtende aktivieren, durch Anfügen einer [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) Komponente, mit `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Komponenten, die erstellt wurde, in der `Scene` selbst haben eine besondere Rolle: Gesamte Szene Funktionalität implementieren. Sie sollten erstellt werden, bevor alle anderen Komponenten und umfassen Folgendes:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): implementiert räumliche Partitionierung und Sichtbarkeit, Abfragen beschleunigt. Ohne diese 3D können Objekte nicht gerendert werden.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): Physiksimulation implementiert. Komponenten wie z. B. Physik [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) oder [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) kann ohne diese nicht ordnungsgemäß funktionieren.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): implementiert Debuggen Geometry rendern.

Normale Komponenten wie z. B. [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) oder [`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)
sollte nicht erstellt werden, direkt in die [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), aber eher in untergeordnete Knoten.

Die Bibliothek enthält eine Vielzahl von Komponenten, die an den Knoten für die von ihnen zum Leben zu erwecken angefügt werden können: Benutzer sichtbare Elemente (Modelle), Sounds, rigidbodys, Kollisionen Formen, Kameras, Lichtquellen, Partikel korrekturemitter ab und vieles mehr.

### <a name="shapes"></a>Formen

Zur Vereinfachung sind die verschiedenen Formen als einfache Knoten im Namespace Urho.Shapes zur Verfügung.  Dazu gehören die Felder, die Kugeln, Kegel, Touren und Ebenen.

### <a name="camera-and-viewport"></a>Kamera und Viewport

Genau wie das Licht, Kameras sind Komponenten, daher Sie die Komponente mit einem Knoten anfügen müssen, dies geschieht wie folgt aus:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Auf diese Weise haben Sie eine Kamera, und Sie haben die Kamera in der 3D-Welt platziert, der nächste Schritt besteht darin zu informieren die `Application` dies die Kamera ist, die Sie verwenden möchten, müssen dies erfolgt durch den folgenden Code:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

Und jetzt sollten Sie sehen die Ergebnisse der Erstellung Ihrer sein.

### <a name="identification-and-scene-hierarchy"></a>Identifikation und einer Szene

Im Gegensatz zu Knoten haben die Komponenten nicht Namen. Komponenten innerhalb der gleichen Knoten werden nur identifiziert, nach deren Typ und Index in der Komponentenliste des Knotens, der in der Reihenfolge der Erstellung gefüllt ist, Sie können z. B. Abrufen der [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) -Komponente aus der `lightNode` Objekt höher wie folgt aus:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Sie können auch eine Liste aller Komponenten abrufen, durch Abrufen der [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) -Eigenschaft die zurückgibt ein `IList<Component>` , die Sie verwenden können.

Beim Erstellen, erhalten beide Knoten und Komponenten Szene globalen ganzzahligen IDs. Sie können mithilfe der Funktionen aus der Szene abgefragt werden [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) und [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Dies ist wesentlich schneller, z. B. rekursive Szene Namen-basierte Knoten Abfragen ausführen.

Es gibt kein integriertes Konzept für eine Entität oder einem spielobjekt. Vielmehr ist es bis zu dem Programmierer entscheiden, die Knotenhierarchie und in welchen Knoten alle geskriptete Logik zu platzieren. Free: Verschieben von Objekten in der 3D-Welt würde in der Regel als untergeordnete Elemente des Stammknotens erstellt werden. Knoten können erstellt werden, entweder mit oder ohne mit [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Eindeutigkeit der Namen der Knoten wird nicht erzwungen.

Immer einige hierarchische Komposition vorhanden ist, es wird empfohlen (und sogar erforderlich sein, da die Komponenten keine eigene 3D-Transformationen) zum Erstellen eines untergeordneten Knotens.

Z. B. wenn ein Zeichen ein Objekt in der Hand gehalten hat, müssen das Objekt einen eigene Knoten, der des Zeichens Hand Knochenarbeit übergeordnet werden würde (auch eine [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Die Ausnahme ist die Physik [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), was offsetted und gedreht einzeln in Bezug auf den Knoten sein kann.

Beachten Sie, dass [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)der Besitzer der Transformation wird absichtlich als Optimierung ignoriert, bei der Berechnung der Welt abgeleitete Transformationen der untergeordneten Knoten, so dass die Änderung wirkt sich nicht aus, und es sollte bleiben unverändert (die Position am Ursprung keine Drehung keine Skalierung.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) Knoten können frei neu zugeordnet werden. Im Gegensatz dazu gehören Komponenten stets auf den Knoten, den sie an, den und Sie können nicht zwischen Knoten verschoben werden. Geben Sie sowohl die Knoten als auch die Komponenten einer [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) Funktion, um dies zu erreichen, ohne das übergeordnete Element durchlaufen. Sobald der Knoten entfernt wird, sind keine Vorgänge auf dem Knoten oder die betreffende Komponente nach dem Aufruf dieser Funktion sicher.

Es ist auch möglich, Sie erstellen eine `Node` , die nicht zu einer Szene gehört. Dies ist nützlich, z. B. mit einer Kamera, die in einer Szene, die geladen werden oder gespeichert haben, verschieben, da dann die Kamera wird nicht zusammen mit der tatsächlichen Szene gespeichert werden und wird nicht zerstört werden, wenn die Szene geladen wird. Beachten Sie jedoch, dass diese Komponenten nicht ordnungsgemäß funktioniert verursacht Geometry "," Physik "oder" Skript Komponenten zu einer nicht angefügten Knoten erstellen und dann später in einer Szene verschieben.

### <a name="scene-updates"></a>Updates der Szene

Eine Szene, deren Updates sind aktiviert (Standard) werden bei jeder Iteration der Hauptschleife automatisch aktualisiert werden.  Der Anwendung [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) -Ereignishandler wird aufgerufen, darauf.

Knoten und Komponenten können von der Szene Update ausgeschlossen werden, indem Sie Sie deaktivieren, finden Sie unter [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Das Verhalten hängt davon ab, auf die jeweilige Komponente, sondern z. B. das Deaktivieren einer drawable-Komponente auch es nicht sichtbar ist, während eine solide Quellkomponente deaktivieren sie schaltet stumm. Wenn ein Knoten deaktiviert ist, werden alle zugehörigen Komponenten behandelt, als unabhängig von ihren eigenen aktivieren/deaktivieren-Status deaktiviert.

## <a name="adding-behavior-to-your-components"></a>Hinzufügen des Verhaltens von Komponenten

Die beste Möglichkeit, Ihr Spiel Struktur ist, stellen eine eigene Komponente, die ein Akteur oder ein Element auf Ihr Spiel zu kapseln.  Dadurch wird die Funktion als eigenständige, über die Ressourcen, um das Verhalten angezeigt.

Die einfachste Möglichkeit zum Hinzufügen des Verhaltens zu einer Komponente ist die Verwendung von Aktionen, die Anweisungen sind, können Sie in die Warteschlange und kombinieren Sie dies mit C# Async-Programmierung.  Dadurch können das Verhalten für die Komponente, sehr hohe sein und ist es einfacher zu verstehen, was passiert.

Alternativ können Sie steuern, was genau an die Komponente geschieht durch Aktualisieren Ihrer Komponenteneigenschaften auf die einzelnen Frames (im Abschnitt framebasierte Verhalten erläutert).

### <a name="actions"></a>Aktionen

Sie können ganz einfach über Aktionen Knoten Verhalten hinzufügen.  Aktionen können verschiedene Eigenschaften des Knotens ändern und führen Sie sie über eine bestimmte Zeitspanne oder Wiederholen diese in eine Anzahl von Malen mit einer bestimmten Animation-Kurve.

Betrachten Sie beispielsweise einen Knoten "Cloud" auf die Szene, können Sie es eingeblendet, wie folgt:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Aktionen sind unveränderliche Objekte an, dem Sie die Aktion zum Steuern von verschiedenen Objekten wiederverwenden können.

Eine allgemeine Vorgehensweise besteht darin, eine Aktion zu erstellen, die den umgekehrten Vorgang ausführt:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Im folgende Beispiel wird das Objekt für die Sie über einen Zeitraum von drei Sekunden ausgeblendet wird.  Sie können auch eine Aktion ausführen und z. B. Sie könnten zunächst die Cloud verschieben und dann ausblenden:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Wenn beide Aktionen zur selben Zeit durchgeführt werden sollen, können Sie verwenden Sie die parallele Aktion, und geben alle die gewünschten Aktionen parallel ausgeführt:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

Im obigen Beispiel wird die Cloud verschieben und gleichzeitig ausblenden.

Sie werden feststellen, dass diese C#-verwenden "await", können Sie das Verhalten linear Denken Sie erreichen möchten.

### <a name="basic-actions"></a>Grundlegende Aktionen

Dies sind die Aktionen, die in von UrhoSharp unterstützt:

* Verschieben von Knoten: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Drehen von Knoten: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Skalierung von Knoten: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Überblenden von Knoten: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Farben: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* Zeitpunkt: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Schleifen: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Die Kombination aus den anderen erweiterten Features zählen die [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) und [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) Aktionen.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Beschleunigung: Steuern der Geschwindigkeit Ihrer Aktionen

Einfachere ist eine Möglichkeit, die leitet die Möglichkeit, dass die Animation wird erweitern, und sie können Animationen sehr viel angenehmer.  Standardmäßig Ihre Aktionen haben einen linearen Verhalten, z. B. eine [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) Aktion müsste eine sehr robotic Verschiebung.  Sie können Ihre Aktionen in einer Beschleunigung-Aktion zum Ändern des Verhaltens, z. B. eine, die langsam Starten der datenverschiebung, beschleunigen und langsam aufgebraucht umschließen ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Dazu müssen Sie eine vorhandene Aktion in eine Ads-Aktion, z. B. umschließen:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Es gibt viele Ads-Modi, das folgende Diagramm zeigt die verschiedenen Beschleunigungstypen und deren Verhalten auf dem Wert des Objekts, das sie über den Zeitraum, von Anfang bis Ende steuern, sind:

![Vereinfachen von Modi](using-images/easing.png "dieses Diagramm zeigt die verschiedenen Beschleunigungstypen und deren Verhalten auf dem Wert des Objekts, das sie über den Zeitraum steuern sind")

### <a name="using-actions-and-async-code"></a>Verwenden von Aktionen und Async-Code

In Ihrer [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) -Unterklasse, sollten Sie eine asynchrone Methode, die bereitet des Verhaltens der Komponente und die Funktionalität von Festplatten für sie einführen.
Und Sie diese mithilfe der C#-Methode aufrufen, würden `await` Schlüsselwort aus einem anderen Teil des Programms entweder Ihre `Application.Start` Methode oder als Reaktion auf einen Benutzer oder die Story zeigen Sie in Ihrer Anwendung.

Zum Beispiel:

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

In der `Launch` der oben genannten drei Aktionen Methoden gestartet werden: der Roboter in der Szene stammt, diese Aktion wirkt sich die Position des Knotens über einen Zeitraum von 0,6 Sekunden.  Da eine Async-Option verwendet wird, dies erfolgt gleichzeitig als die nächste Anweisung wird der Aufruf zum `MoveRandomly`.  Diese Methode wird die Position des Roboters parallel auf einem zufälligen Ort geändert werden.  Dies erfolgt durch zwei Verrunden Aktionen ausführen, die Bewegung auf einem neuen Speicherort und kehre zur ursprünglichen positionieren, und wiederholen Sie diesen Vorgang so lange der Roboter aktiv bleibt.  Und um die Dinge etwas interessanter zu machen, die Robots wird beibehalten Schießen gleichzeitig.  Die Schießen wird nur 0,1 Sekunden gestartet.

### <a name="frame-based-behavior-programming"></a>Verhalten framebasierte-Programmierung

Wenn Sie das Verhalten der Komponente pro Frame für Frame anstelle von Aktionen steuern möchten, gehen Sie wie besteht im Überschreiben der [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) Methode Ihrer [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) Unterklasse.  Diese Methode wird einmal pro Frame aufgerufen, und es wird nur aufgerufen, wenn Sie die ReceiveSceneUpdates-Eigenschaft auf "true" festlegen.

Das folgende Beispiel zeigt, wie Sie erstellen eine `Rotator` Komponente, die klicken Sie dann auf einen Knoten, wodurch den Knoten zum Drehen verbunden ist:

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

Und wie Sie diese Komponente zu einem Knoten anfügen würden:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Kombinieren von Formatvorlagen

Können Sie das Async/Aktion-basierten Modell zum Programmieren das Verhalten der eignet sich für Fire-and-forget-Stil, der Programmierung hervorragend, aber Sie können auch Optimieren Ihrer Komponente zu Verhalten führen Sie auch Code Update auf die einzelnen Frames.

Z. B. in der Demo SamplyGame Hiermit wird der `Enemy` Klasse codiert, die Aktionen der grundlegende Verhalten verwendet, aber es wird auch sichergestellt, dass die Komponenten zum Benutzer hin verweisen, durch Festlegen der Richtung des Knotens mit `Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

## <a name="loading-and-saving-scenes"></a>Laden und Speichern von Szenen

Im Hintergrund geladen, und im XML-Format gespeichert werden können; finden Sie unter den Funktionen [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) und [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Wenn eine Szene geladen wird, wird zunächst alle vorhandenen Inhalte in (untergeordnete Knoten und Komponenten) entfernt. Knoten und Komponenten, die mit temporären markiert sind die `Temporary` Eigenschaft wird nicht gespeichert werden. Das Serialisierungsprogramm behandelt alle integrierten Komponenten und Eigenschaften, aber es ist nicht intelligent genug, um benutzerdefinierte Eigenschaften als auch in Ihrer Unterklassen der Komponente definierten Felder zu behandeln. Jedoch stellt zwei virtuelle Methoden dafür bereit:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) wo Sie Sie benutzerdefinierte Zustände für die Serialisierung registrieren

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) in dem Sie Ihre gespeicherte benutzerdefinierte Status abrufen können.

In der Regel wird eine benutzerdefinierte Komponente wie folgt aussehen:

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

### <a name="object-prefabs"></a>Objekt prefabs (Vorlagen)

Nur Laden oder speichern die gesamte Szenen ist nicht flexibel genug für Spiele, in denen neue Objekte dynamisch erstellt werden müssen. Auf der anderen Seite werden komplexe Objekte erstellen und Festlegen ihrer Eigenschaften im Code auch mühsam sein. Aus diesem Grund ist es auch möglich, einen szeneknoten speichern Sie, der zugehörigen untergeordneten Knoten, die Komponenten und die Attribute enthält. Diese können später bequem als Gruppe geladen werden.  Eine solche gespeicherte Objekt wird häufig als eines prefabs bezeichnet. Hierfür gibt es drei Möglichkeiten:

- Im Code durch den Aufruf [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) auf dem Knoten
- Im Editor durch Auswahl des Knotens in das Fenster "Aufrufhierarchie" und "Save Knoten als" über das Menü "Datei".
- Mit dem Befehl "Node" in `AssetImporter`, die speichert der szenenhierarchie für Knoten und alle Modelle enthalten im eingabeasset (z. b. eine Collada-Datei)

Aufrufen, um den gespeicherten Knoten in einer Szene zu instanziieren, [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Der Knoten als untergeordnetes Element der Szene erstellt werden, aber es kann kostenlos erneut danach übergeordnet werden. Position und Drehung für das Platzieren des Knotens müssen angegeben werden. Der folgende Code zeigt, wie zum Instanziieren eines prefabs `Ninja.xm` zu einer Szene mit der gewünschten Position und Drehung:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Ereignisse

UrhoObjects Auslösen einer Anzahl von Ereignissen, diese werden als C#-Ereignisse aufgeführt, auf die verschiedenen Klassen, die sie generieren.  Zusätzlich zu den C# -Code-Basis Ereignismodell, es ist auch möglich, verwenden Sie eine der `SubscribeToXXX` Methoden, mit denen Sie zum Abonnieren und eine abonnementtoken beibehalten, die später können Sie das Abonnement kündigen.  Der Unterschied besteht darin, dass Erstere ermöglicht viele Aufrufe zum abonnieren, während das zweite Argument nur ermöglicht, ermöglicht jedoch der nützlicher Lambda-Stil-Ansatz verwendet werden soll, und kann noch, einfaches Entfernen des Abonnements.  Sie schließen sich gegenseitig aus.

Wenn Sie ein Ereignis abonnieren, müssen Sie eine Methode bereitstellen, die ein Argument mit den entsprechenden Ereignisargumenten verwendet.

Dies ist z. B. wie Sie einer Maustaste ausgelöste Ereignis abonnieren:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

Mit Lambda-Style:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Sie möchten manchmal keine Benachrichtigungen mehr empfangen, für das Ereignis in diesen Fällen speichern den Rückgabewert aus dem Aufruf von `SubscribeTo` -Methode, und rufen die Methode "Unsubscribe" auf:

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

Der Parameter empfangen, die vom Ereignishandler ist eine stark typisierte Ereignisargumentklasse, die speziell für jedes Ereignis werden und enthält die Nutzlast des Ereignisses.

## <a name="responding-to-user-input"></a>Reagieren auf Benutzereingabe

Sie können unten auf verschiedene Ereignisse wie Tastatureingaben abonnieren, durch das Abonnieren des Ereignisses und reagieren mit der Eingabe übermittelt werden:

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

Aber in vielen Szenarien, die Sie möchten Ihre Szene Aktualisierungshandler, überprüfen Sie auf den aktuellen Status der Schlüssel, sobald diese werden aktualisiert, und aktualisieren Sie Ihren Code entsprechend.  Z. B. die folgenden dienen zur basierend auf der Tastatur, geben Sie den Speicherort des Kamera aktualisiert:

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>Ressourcen (Anlagen)

Ressourcen gehören die meisten Aktionen in von UrhoSharp, die von Massenspeicher während der Initialisierung oder der Laufzeit geladen werden:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) – zum skeletal-Animationen
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -Stellt in einer Vielzahl von Grafikformate gespeicherte Bilder
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D-Modelle
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -Materialien, die zum Rendern von Modellen verwendet.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [Beschreibt](http://urho3d.github.io/documentation/1.4/_particles.html) wie ein Partikel korrekturemitter funktioniert, finden Sie unter "[Partikel](#particles)" unten.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) – benutzerdefinierten Shadern
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -Sounds wiedergeben, finden Sie unter "[Sound](#sound)" unten.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -Material-Rendering-Techniken
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2D-Textur
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D-Textur
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -Cube Textur
- `XmlFile`

Diese verwaltet werden und von geladen sind die [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) Subsystem (verfügbar als [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Die Ressourcen selbst werden durch die Pfade, relativ zum registrierten Ressourcenverzeichnisse oder Dateien identifiziert. Standardmäßig registriert die Engine die Ressourcenverzeichnisse `Data` und `CoreData`, oder die Pakete `Data.pak` und `CoreData.pak` , wenn sie vorhanden sind.

Wenn beim Laden einer Ressource fehlschlägt, wird ein Fehler protokolliert werden, und ein null-Verweis zurückgegeben.

Das folgende Beispiel zeigt eine typische Möglichkeit das Abrufen von einer Ressource aus dem Cache für die Ressource.  In diesem Fall einer Textur für die ein Element der Benutzeroberfläche, verwendet der `ResourceCache` Eigenschaft aus der `Application` Klasse.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Ressourcen können auch manuell erstellt und mit dem Ressourcen-Cache gespeichert werden, als ob sie von der Festplatte geladen wurde.

Arbeitsspeicher-Budgets pro Ressourcentyp festgelegt werden können: Wenn Ressourcen mehr Arbeitsspeicher als die zulässige verwendet, die ältesten Ressourcen entfernt werden aus dem Cache, wenn kein verwenden mehr. Standardmäßig werden die Arbeitsspeicher-Budgets auf unbegrenzte festgelegt.

### <a name="bringing-3d-models-and-images"></a>Bringen 3D-Modelle und Images

Urho3D versucht bestehenden Dateiformaten, wann immer möglich, und Definieren von benutzerdefinierten Dateiformaten nur, wenn es absolut notwendig ist z. B. für Modelle (*.mdl) und für Animationen (*.ani). Für diese Arten von Assets, bietet Urho einen Konverter - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) die können viele gängige 3D Formate wie z. B. Fbx Dae "," 3ds, "und" Obj "," usw. nutzen.

Es gibt auch eine praktische add-in für Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) exportieren, die Ihre Blender-Assets in das Format, das für die Urho3D geeignet ist.

### <a name="background-loading-of-resources"></a>Laden von Ressourcen im Hintergrund

In der Regel beim Anfordern von Ressourcen mit einer der der `ResourceCache`des `Get` -Methode, sie werden sofort im Hauptthread, der mehrere Millisekunden für alle erforderlichen Schritte dauern geladen (Datei vom Datenträger zu laden, Analysieren von Daten, in GPU hochladen, falls erforderlich ) und können daher Framerate löscht.

Wenn Sie wissen, im Voraus welche Ressourcen Sie benötigen, können Sie anfordern, werden durch den Aufruf in einem Hintergrundthread geladen werden sollen `BackgroundLoadResource()`. Sie können auf die Ressource Hintergrund geladen-Ereignis abonnieren, indem die `SubscribeToResourceBackgroundLoaded` Methode. Sie werden sehen, ob das Laden tatsächlich ein Erfolg oder Misserfolg wurde. Abhängig von der Ressource, die nur ein Teil des Ladeprozesses in einen Hintergrundthread verschoben werden kann, z. B. der abschließen GPU-Upload-Schritt immer muss im Hauptthread ausgeführt werden. Beachten Sie, dass wenn Sie eine der Methoden für eine Ressource, für die im Hintergrund geladen, in der Warteschlange wird, zum Laden von Ressource aufrufen, der Hauptthread installieren wird, bis der Ladevorgang abgeschlossen ist.

Die Funktion zum Laden von asynchronen Szene `LoadAsync()` und `LoadAsyncXML()` bietet die Option zum Laden der Hintergrund Ressourcen, die zum Laden des Inhalts der Szene zunächst vor dem fortfahren. Es kann auch nur die Ressourcen zu laden, ohne Änderung der Szene durch Angabe verwendet werden die `LoadMode.ResourcesOnly`. Dadurch wird eine Szene oder des Objekts prefab-Datei für schnelle Instanziierung vorbereiten.

Die maximale Zeit (in Millisekunden) zum Schluss für jeden Frame aufgewendete Abschluss Hintergrund geladene Ressourcen werden, durch Festlegen konfiguriert können der `FinishBackgroundResourcesMs` Eigenschaft für die `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Sound

Sound ist ein wichtiger Teil des Spiels, und die von UrhoSharp-Framework bietet eine Möglichkeit der Wiedergabe von Sound in einem Spiel.  Sound durch Anfügen einer [`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/)
Komponente einer [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) und diese wiedergeben klicken Sie dann aus Ihren Ressourcen einer benannten Datei.

Dies ist die Vorgehensweise ist:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Partikel

Partikel bieten eine einfache Möglichkeit, einige Effekte einfach und kostengünstig, Ihre Anwendung hinzuzufügen.  Sie können Partikel, die in PEX-Format gespeicherte nutzen, der mithilfe von Tools wie [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Partikel sind Komponenten, die auf einen Knoten hinzugefügt werden können.  Sie müssen zum Aufrufen des Knotens `CreateComponent<ParticleEmitter2D>` Methode, um das Partikel erstellen und konfigurieren Sie dann durch der Effect-Eigenschaft auf einen Direct2D-Effekt einstellen, des Partikels aus dem Cache für die Ressource geladen wird.

Beispielsweise können Sie diese Methode aufrufen, in der Komponente, um einige Partikel anzuzeigen, die als Zahl gerendert werden, wenn er erreicht:

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

Der obige Code erstellt einen Explosion-Knoten, der mit Ihrer aktuellen Komponente verbunden ist, die innerhalb dieses Knotens Explosion erstellen wir eine 2D Partikel korrekturemitter und konfigurieren Sie sie durch die Effect-Eigenschaft festlegen.  Wir führen Sie zwei Aktionen, eine, die den Knoten, um kleinere werden skaliert und eine, die es 0,5 Sekunden in dieser Größe verlässt.  Klicken Sie dann entfernen wir das unkontrollierte anwachsen, das wodurch auch die-partikeleffekt vom Bildschirm entfernt.

Das oben genannten Partikel Ausgabe bei Verwendung eine Kugel Textur sieht folgendermaßen aus:

![Partikel, mit einer Textur für die Kugel](using-images/image-1.png "das oben genannten Partikel Ausgabe sieht folgendermaßen aus bei Verwendung eine Textur für die Kugel")

Und wie es aussieht, wenn Sie eine Textur Blöcke verwenden:

![Partikel, mit einer Textur Feld](using-images/image-2.png "und wie es aussieht, wenn eine Textur Blöcke verwenden")

## <a name="multithreading-support"></a>Unterstützung von Multithreading

Von UrhoSharp ist eine einzelne Threads Library.  Dies bedeutet, dass Sie nicht, zum Aufrufen von Methoden in von UrhoSharp aus einem Hintergrundthread versuchen sollten, oder Sie riskieren den Anwendungszustand beschädigen und wahrscheinlich Absturz Ihrer Anwendung.

Wenn Sie Code im Hintergrund ausgeführt, und aktualisieren Sie dann die Urho-Komponenten auf die Hauptbenutzeroberfläche möchten, können Sie die [`Application.InvokeOnMain(Action)`](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain)
-Methode.  Darüber hinaus können Sie C# -Code "await" verwenden und die .NET task-APIs, um sicherzustellen, dass der Code im entsprechenden Thread ausgeführt wird.

## <a name="urhoeditor"></a>UrhoEditor

Sie können den Urho-Editor für Ihre Plattform aus der [Urho Website](http://urho3d.github.io/), wechseln Sie zu Downloads, und wählen Sie die neueste Version.

## <a name="copyrights"></a>Urheberrechte

Diese Dokumentation enthält die ursprünglichen Inhalte von Xamarin Inc., aber sehr häufig in der open-Source-Dokumentation für das Projekt Urho3D zeichnet und enthält Screenshots aus dem Projekt Cocos2D.
