---
title: Verwenden von UrhoSharp
description: Übersicht über das UrhoSharp-Modul
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 7d54203fe391af6acde70f4c2a073b7f71332c91
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="using-urhosharp"></a>Verwenden von UrhoSharp

_Übersicht über das UrhoSharp-Modul_

Bevor Sie Ihr erste Spiel schreiben, mit den Grundlagen beruhendes abgerufen werden sollen: zum Einrichten der Szene, wie Ressourcen geladen werden (Dies enthält Bildmaterial) und zum Erstellen von einfachen Interaktionen für das Spiel.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Szenen, die Knoten, Komponenten und Kameras

Der Szene Modell kann als ein Szenendiagramm komponentenbasierter beschrieben werden. Die Szene besteht aus einer Hierarchie von Szenenknoten, beginnend mit den Stammknoten, der auch die gesamte Szene darstellt. Jede [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) hat eine 3D-Transformation (Position, Drehung und Skalierung), einen Namen, eine ID sowie eine beliebige Anzahl von Komponenten.  Schalten Sie einen Knoten Komponenten lebendig werden lassen, können sie vornehmen, fügen eine visuelle Darstellung ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), können sie Sound ausgeben ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), können sie eine Grenze Kollision usw. bieten.

Können Ihre Szenen und Setup-Knoten, die mithilfe der [Urho Editor](#UrhoEditor), oder Sie können Aktionen aus dem C#-Code.  In diesem Dokument wird alles mithilfe von Code untersucht, wie sie zeigen, dass die Elemente, die erforderlichen Ergebnisse erzielen, auf dem Bildschirm angezeigt

Neben dem Einrichten der Szene, müssen Sie Setup eine [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), dies wird bestimmt, was dem Benutzer angezeigt abgerufen wird.

### <a name="setting-up-your-scene"></a>Einrichten der Szene

Sie würden diese Form in der Regel die Startmethode erstellen:

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

Rendern von 3D-Objekten sound-Wiedergabe, physikalische und skriptgesteuerten Logik Updates sind alle aktiviert durch das Erstellen von verschiedenen Komponenten in die Knoten durch den Aufruf [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Richten Sie z. B. Ihre Knoten und hell Komponente wie folgt:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Wir haben über einen Knoten mit dem Namen "`DirectionalLight`" und eine Richtung für, aber nichts anderes festgelegt.  Wir können jetzt den oben genannten Knoten in einem Knoten Licht ausgebende aktivieren, indem Anfügen einer [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) Komponente, mit `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Komponenten, die erstellt wurde, in der `Scene` selbst haben eine besondere Rolle: Szene Wide-Funktionalität zu implementieren. Sie sollten vor allen anderen Komponenten erstellt werden und umfassen Folgendes:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): räumliche Partitionierung implementiert und beschleunigte Sichtbarkeit Abfragen. Ohne diese 3D können Objekte nicht gerendert werden.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): physikalische Simulation implementiert. Physikalische Komponenten wie z. B. [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) oder [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) kann ohne diese nicht ordnungsgemäß funktionieren.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): implementiert Debuggen Geometrie rendern.

Gewöhnliche Komponenten wie [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) oder [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) dürfen nicht erstellt werden, direkt in die [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), sondern vielmehr in untergeordnete Knoten.

Die Bibliothek enthält eine Vielzahl von Komponenten, die Sie zu Ihrer Knoten, um diese lebendig werden lassen anfügen können: Benutzer sichtbaren Elemente (Modelle) Sounds starre Texte Kollision Formen Kameras Lichtquellen Partikel Emissionen und vieles mehr.

### <a name="shapes"></a>Formen

Zur Vereinfachung sind die verschiedenen Formen als einfache Knoten im Namespace Urho.Shapes verfügbar.  Dazu gehören Dialogfeldern, Bereichen, Kegel, Zylinder und Ebenen.

### <a name="camera-and-viewport"></a>Kamera- und Viewport

Genau wie das Licht Kameras sind Komponenten, daher Sie die Komponente an einem Knoten angefügt müssen, wird dies folgendermaßen:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Mit dieser Option können Sie eine Kamera erstellt haben und Sie haben die Kamera in 3D weltweit platziert, der nächste Schritt besteht darin zu informieren der `Application` dies die Kamera ist, die Sie verwenden möchten, müssen dies erfolgt durch den folgenden Code:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

Und nun sollten Sie sehen die Ergebnisse der Erstellung des sein.

### <a name="identification-and-scene-hierarchy"></a>Identifikation und Szene Hierarchie

Im Gegensatz zum Knoten haben Komponenten keine Namen; Komponenten innerhalb der gleichen Knoten werden nur identifiziert, von deren Typ und Index in der Komponentenliste des Knotens, der in der Reihenfolge der Erstellung gefüllt ist, Sie können z. B. Abrufen der [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) -Komponente aus der `lightNode` Objekt wie oben beschrieben:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Außerdem erhalten Sie eine Liste aller Komponenten durch Abrufen der [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) Eigenschaft die zurückgibt, eine `IList<Component>` , die Sie verwenden können.

Beim Erstellen, erhalten Knoten und Komponenten Szene globale ganzzahligen IDs. Sie können mit den Funktionen aus der Szene abgefragt werden [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) und [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Dies ist wesentlich schneller als das z. B. rekursive Namen-basierte Szene Knoten Abfragen.

Es ist kein integriertes Konzept für eine Entität oder ein Spiel-Objekt. Vielmehr ist es bis zu dem Programmierer, die die Knotenhierarchie entscheiden und in die Knoten aus, die keine skriptgesteuerte Logik zu platzieren. Frei Verschieben von Objekten in 3D weltweit würde in der Regel als untergeordnete Elemente des Stammknotens erstellt werden. Knoten können erstellt werden, entweder mit oder ohne mit [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Eindeutigkeit von Knotennamen wird nicht erzwungen.

Wenn einige hierarchische Komposition ist, ist es empfohlen (und tatsächlich erforderlich, da Komponenten nicht ihren eigenen 3D Transformationen verfügen), einen untergeordneten Knoten zu erstellen.

Z. B. wenn ein Zeichen ein Objekt in seine Hand gedrückt wurde, müssen das Objekt einen eigene Knoten aktivieren, der das Zeichen Hand Bone übergeordnet werden würde (auch eine [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Die Ausnahme ist die physikalische [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), was offsetted und gedreht einzeln in Bezug auf den Knoten sein kann.

Beachten Sie, dass [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)der Transformation wird absichtlich als Optimierung ignoriert, bei der Berechnung der Welt abgeleitete Transformationen der untergeordneten Knoten, so dass die Änderung wirkt sich nicht, und es sollten bleiben unverändert (Position Ursprungs, die keine Drehung besitzen keine Skalierung.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) Knoten können kostenlos erneut übergeordnet werden. Im Gegensatz dazu gehören Komponenten immer auf den Knoten, den sie an, den und können nicht zwischen Knoten verschoben werden. Knoten und Komponenten ermöglichen eine [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) Funktion, um dies zu erreichen ohne das übergeordnete Element durchlaufen. Sobald der Knoten entfernt wurde, sind keine Vorgänge auf dem Knoten oder die betreffende Komponente nach dem Aufrufen dieser Funktion sicher.

Es ist auch möglich, erstellen Sie eine `Node` , die nicht zu einer Szene gehört. Dies ist nützlich, z. B. mit einer Kamera verschieben in eine Szene, die geladen werden oder gespeichert, da die anschließend die Kamera wird nicht zusammen mit der tatsächlichen Szene gespeichert werden und wird nicht zerstört werden, wenn die Szene geladen wird. Beachten Sie jedoch, dass diese Komponenten nicht ordnungsgemäß erstellen Geometry, physikalische oder Skript-Komponenten zu einem Knoten getrennt und dann später in eine Szene verschieben verursacht.

### <a name="scene-updates"></a>Szene updates

Eine Szene, deren Updates sind aktiviert (Standard) werden bei jeder Iteration Hauptschleife automatisch aktualisiert werden.  Der Anwendungsverzeichnis [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) -Ereignishandler aufgerufen wird.

Knoten und Komponenten können durch die Deaktivierung der Szene Update ausgeschlossen werden, finden Sie unter [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Das Verhalten hängt davon ab, auf die jeweilige Komponente, z. B. das Deaktivieren einer zeichenbaren Komponente auch; vereinfacht jedoch nicht sichtbar, während schaltet er das Deaktivieren einer Quellkomponente sound stumm. Wenn ein Knoten deaktiviert ist, werden alle zugehörigen Komponenten behandelt, als unabhängig von ihren eigenen Zustand aktivieren/deaktivieren deaktiviert.

## <a name="adding-behavior-to-your-components"></a>Hinzufügen des Verhaltens von Komponenten

Die beste Möglichkeit, ein Spiel Struktur besteht darin, von Ihrer eigenen Komponente, die ein Akteur oder ein Element auf das Spiel zu kapseln.  Dadurch wird das Feature Self-Service enthalten, aus den Objekten verwendet, um ihn zum alten Verhalten anzuzeigen.

Die einfachste Möglichkeit zum Hinzufügen von Verhalten für eine Komponente ist die Verwendung von Aktionen, die Anweisungen sind, können Sie die Warteschlange und, die mit c# asynchrone Programmierung kombinieren.  Dies ermöglicht das Verhalten für die Komponente zu sehr hohen Grad an und macht es einfacher zu verstehen, was geschieht.

Alternativ können Sie steuern, was genau an die Komponente geschieht durch Aktualisieren die Komponenteneigenschaften für jeden Zeitrahmen (im Abschnitt framebasierte Verhalten erläutert).

### <a name="actions"></a>Aktionen

Sie können ganz einfach mit Aktionen Knoten Verhalten hinzufügen.  Aktionen können verschiedene Eigenschaften des Knotens ändern und führen Sie sie über einen Zeitraum oder Wiederholen sie eine Anzahl von Malen mit einer bestimmten Animation Kurve.

Betrachten Sie beispielsweise einen Knoten "Cloud" auf der Szene, können Sie es ausblenden, wie folgt:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Aktionen sind unveränderlich-Objekte, die Sie die Aktion für verschiedene Objekte driving wiederverwenden können.

Eine allgemeine Vorgehensweise besteht darin, eine Aktion erstellen, die umgekehrte Vorgang ausführt:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Im folgende Beispiel wird das Objekt für Sie über einen Zeitraum von drei Sekunden ausgeblendet wird.  Sie können auch eine Aktion ausführen nach dem anderen, z. B. Sie könnten zuerst die Cloud verschieben und blenden Sie sie:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Wenn Sie beide Aktionen an, die zur gleichen Zeit stattfinden soll, können Sie verwenden Sie die parallele Aktion und geben Sie alle Aktionen gewünschten parallel ausgeführt:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

Im obigen Beispiel wird die Cloud verschieben und gleichzeitig ausblenden.

Sie werden feststellen, dass diese c# mithilfe "await" können Sie das Verhalten linear vorstellen, Sie erzielen möchten.

### <a name="basic-actions"></a>Grundlegende Aktionen

Dies sind die Aktionen, die in UrhoSharp unterstützt:

* Verschieben von Knoten: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Drehen von Knoten: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Skalierung Knoten: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Ausblenden von Knoten: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Farben: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* Zeitpunkt dar: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Schleifen: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Andere erweiterten Funktionen enthalten, die Kombination der [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) und [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) Aktionen.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Einfachere - Steuern der Geschwindigkeit Ihrer Aktionen

Einfachere ist eine Möglichkeit, die leitet die Möglichkeit, dass die Animation erweitern wird und sie können Animationen viel mehr angenehmeren.  Standardmäßig Ihre Aktionen verfügt über eine lineare Verhalten, z. B. eine [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) Aktion müsste eine sehr automatische Verschiebung.  Sie können Ihre Aktionen in einer Beschleunigung Aktion so ändern Sie das Verhalten, z. B. würde langsam Starten der datenverschiebung, zu beschleunigen und das Ende langsam erreichen umschließen ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Dazu müssen Sie eine bestehende Aktion in eine Beschleunigungsfunktionen Aktion, z. B. wrapping:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Es gibt viele Beschleunigungsfunktionen Modi, das folgende Diagramm zeigt die verschiedenen Beschleunigungsfunktionen Typen und deren Verhalten auf den Wert des Objekts, das sie über den Zeitraum von Anfang bis Ende steuern:

![Einfachere Modi](using-images/easing.png "dieses Diagramm zeigt die verschiedenen Beschleunigungsfunktionen Typen und deren Verhalten auf den Wert des Objekts, das sie für den Zeitraum steuern sind")

### <a name="using-actions-and-async-code"></a>Verwenden von Aktionen und asynchronem Code

In Ihrem [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) -Unterklasse, sollten Sie eine asynchrone Methode, die Ihre Komponentenverhalten vorbereitet und die Funktionalität für ihn Laufwerke einführen.
Und Sie diese mithilfe der C#-Methode aufrufen würde `await` Schlüsselwort von einem anderen Teil des Programms, entweder Ihre `Application.Start` Methode oder als Antwort auf einen Benutzer oder Story zeigen Sie in der Anwendung.

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

In der `Launch` der oben genannten drei Aktionen Methoden gestartet werden: des Roboters stammen, in der Szene haben, diese Aktion wird die Position des Knotens über einen Zeitraum von 0,6 Sekunden ändern.  Da dies eine Async-Option ist, dies erfolgt gleichzeitig als die nächste Anweisung, die aufgerufen wird, `MoveRandomly`.  Diese Methode wird die Position des Robots parallel zu einem zufälligen Ort geändert werden.  Dies wird erreicht, indem Sie zwei Verrunden Aktionen, die Verschiebung an einen neuen Speicherort ausführen, und kehren zur ursprünglichen positionieren, und wiederholen Sie diesen Vorgang solange des Roboters aktiv ist.  Und um Dinge interessanter zu machen, des Roboters wird beibehalten behandeln gleichzeitig.  Das Behandeln von beginnt erst alle 0,1 Sekunden.

### <a name="frame-based-behavior-programming"></a>Verhalten framebasierte-Programmierung

Wenn Sie das Verhalten der Komponente auf einen Frame nach dem anderen Basis anstelle von Aktionen steuern möchten, was Sie tun ist, überschreiben die [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) Methode Ihrer [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) Unterklasse.  Diese Methode wird einmal pro Frame aufgerufen und wird aufgerufen, nur dann, wenn Sie die ReceiveSceneUpdates-Eigenschaft auf "true" festlegen.

Im folgenden wird gezeigt, wie zum Erstellen einer `Rotator` Komponente, die Sie dann auf einen Knoten zugeordnete bewirkt, den Knoten zum Drehen dass:

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

Und wie Sie diese Komponente auf einen Knoten anfügen möchten:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Kombinieren von Stilen

Können Sie das asynchrone/Action-basierte Modell für den Großteil des Verhaltens also hervorragend für auslösen und vergessen Programmierstils nach Art der Programmierung, aber Sie können auch eine Feinabstimmung durchzuführen Komponentenverhalten um auch einige Update-Code für jeden Zeitrahmen auszuführen.

Angenommen, in der Demo SamplyGame dient der `Enemy` Klasse codiert, die Aktionen der grundlegende Verhalten verwendet, aber zusätzlich wird gewährleistet, dass die Komponenten in Richtung der Benutzer verweisen, durch Festlegen der Richtung des Knotens ohne `Node.LookAt`:

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

Im Hintergrund werden geladen und im XML-Format gespeichert; finden Sie unter Funktionen [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) und [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Wenn eine Szene geladen wird, wird zunächst alle vorhandener Inhalt (untergeordnete Knoten und Komponenten) entfernt. Knoten und Komponenten, die mit temporären markiert sind die `Temporary` Eigenschaft wird nicht gespeichert werden. Der Serialisierer verarbeitet alle integrierten Komponenten und Eigenschaften jedoch nicht intelligent genug, um benutzerdefinierte Eigenschaften als auch in Ihrer Komponente Unterklassen definierten Felder zu behandeln. Jedoch gibt es zwei virtuelle Methoden dafür:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) wo Sie Sie benutzerdefinierte Status für die Serialisierung registrieren

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

### <a name="object-prefabs"></a>Objekt Prefabs

Nur beim Laden oder speichern die gesamte Szenen ist nicht flexibel genug für Spiele, bei denen neue Objekte dynamisch erstellt werden müssen. Andererseits, werden komplexe Objekte erstellen und deren Eigenschaften im Code auch zeitraubend sein. Aus diesem Grund ist es auch möglich, einen Knoten der Szene speichern seiner untergeordneten Knoten, die Komponenten und die Attribute enthalten. Diese können später bequem als Gruppe geladen werden.  Gespeicherte Objekt wird häufig als eine Prefab bezeichnet. Hierfür gibt es drei Möglichkeiten:

- Im Code durch Aufrufen von [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) auf dem Knoten
- In der Editor den Knoten im Hierarchiefenster auswählen und "speichern, indem Knoten als" über das Menü "Datei".
- Mit dem Befehl "Knoten" in `AssetImporter`, speichert der Szene-Knotenhierarchie und alle Modelle in der Eingabe-Medienobjekts (z. b. eine Collada-Datei)

Rufen Sie zum Instanziieren des gespeicherten Knotens in eine Szene [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Der Knoten wird als untergeordnetes Element der Szene erstellt werden, aber es kann kostenlos erneut danach übergeordnet werden. Position und Drehung zum Platzieren des Knotens angegeben werden müssen. Der folgende Code veranschaulicht, wie eine Prefab instanziieren `Ninja.xm` eine Szene mit der gewünschten Position und Drehung:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Ereignisse

UrhoObjects Auslösen einer Anzahl von Ereignissen, diese werden als C#-Ereignisse eingeblendet, in den verschiedenen Klassen, die sie generieren.  Zusätzlich zu den c#-Basis Ereignismodell, es ist auch möglich, verwenden Sie eine der `SubscribeToXXX` Methoden, mit denen Sie zum Abonnieren und eine abonnementtoken beibehalten, die später können Sie kündigen des Abonnements.  Der Unterschied ist, dass erstere können viele Aufrufer zu abonnieren, bei dem zweiten Ausdruck nur erlaubt es einem, jedoch ermöglicht der nützlicher Lambda-Stil für eine Methode verwendet werden soll, und noch, ermöglichen die einfache Entfernen des Abonnements.  Sie schließen sich gegenseitig aus.

Wenn Sie ein Ereignis abonnieren, müssen Sie eine Methode bereitstellen, die ein Argument mit den entsprechenden Ereignisargumente.

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

Mit Lambda-Format:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Manchmal möchten Sie den Empfang von Benachrichtigungen, für das Ereignis in diesen Fällen speichern den Rückgabewert des Aufrufs von `SubscribeTo` -Methode, und der Unsubscribe-Methode dafür aufrufen:

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

Der Parameter erhalten vom Ereignishandler ist eine stark typisierte Argumente Ereignisklasse, die speziell für jedes Ereignis werden und die Ereignisnutzlast enthält.

## <a name="responding-to-user-input"></a>Reagieren auf Benutzereingaben

Sie können verschiedene Ereignisse aus, wie Tastatureingaben unten abonnieren durch Abonnieren des Ereignisses, und Antworten auf die Eingabe, die übermittelt werden:

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

Aber in vielen Szenarien, die auf den aktuellen Status der Schlüssel kontrolliert werden, wenn sie werden aktualisiert, und aktualisieren Sie den Code entsprechend der Szene-Aktualisierungshandler werden sollen.  Beispielsweise kann die Folgendes verwendet, um basierend auf der Tastatur, geben Sie den Speicherort der Kamera aktualisieren:

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

## <a name="resources-assets"></a>Ressourcen (Elemente)

Ressourcen umfassen die meisten Aufgaben in UrhoSharp aus, die aus Massenspeicher während der Initialisierung oder der Laufzeit geladen werden:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) – verwendet für skeletal Animationen
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -Bilder in einer Vielzahl von Formaten Grafik gespeicherten darstellt
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D-Modelle
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -Materialien, die zum Rendern von Modellen verwendet.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [Beschreibt](http://urho3d.github.io/documentation/1.4/_particles.html) wie ein Partikel Metadatenemitter funktioniert, finden Sie unter "[Partikel](#particles)" unten.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -Benutzerdefinierte Shadern
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -Sound wiedergeben, finden Sie unter "[Sound](#sound)" unten.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -Material-Rendering-Techniken
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2D-Textur
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D-Struktur
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -Cube Struktur
- `XmlFile`

Verwaltet und geladen, indem die [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) Subsystem (verfügbar als [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Die Ressourcen selbst werden durch ihre Dateipfade, relativ zum registrierte Ressourcenverzeichnisse oder Paketdateien identifiziert. Standardmäßig registriert das Modul die Ressourcenverzeichnisse `Data` und `CoreData`, oder das Pakete `Data.pak` und `CoreData.pak` , wenn sie vorhanden sind.

Wenn beim Laden einer Ressource fehlschlägt, wird ein Fehler protokolliert werden, und ein null-Verweis zurückgegeben.

Das folgende Beispiel zeigt eine typische Möglichkeit Abrufen einer Ressource aus dem Cache Ressource von.  In diesem Fall eine Textur für ein Element der Benutzeroberfläche verwendet die `ResourceCache` Eigenschaft aus der `Application` Klasse.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Ressourcen können auch manuell erstellt und in den Ressourcen-Cache gespeichert werden, als ob sie von der Festplatte geladen wurde.

Arbeitsspeicher Budgets pro Ressourcentyp festgelegt werden können: Wenn Ressourcen mehr Arbeitsspeicher als die zulässige nutzen, die ältesten Ressourcen entfernt werden aus dem Cache, wenn verwendet nicht mehr. Standardmäßig werden die Arbeitsspeicher-Budgets auf unlimited festgelegt.

### <a name="bringing-3d-models-and-images"></a>Kombinieren von 3D-Modelle und Bilder

Urho3D versucht, nach Möglichkeit vorhandene Dateiformate verwenden, und definieren benutzerdefinierte Dateiformate nur, wenn dies absolut notwendig ist z. B. für Modelle (*.mdl) und für Animationen (*.ani). Für diese Arten von Elementen, Urho stellt einen Konverter - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) die viele gängige 3D Formate, z. B. Fbx, Dae, 3ds, und Obj usw. genutzt werden können.

Es gibt auch eine praktische add-in für Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) exportieren, die Ihre Medienobjekte Blender in das Format, das für die Urho3D geeignet ist.

### <a name="background-loading-of-resources"></a>Das Laden der im Hintergrund von Ressourcen

In der Regel wird beim Anfordern von Ressourcen mithilfe eines der `ResourceCache`des `Get` -Methode, sie die sofort im Hauptthread, die mehrere Millisekunden für die erforderlichen Schritte wird möglicherweise erneut geladen werden (Datei vom Datenträger laden, Daten analysieren, hochladen, GPU bei Bedarf ) und kann daher dazu führen, Framerate löscht.

Wenn Sie die im Voraus benötigten Ressourcen müssen Sie wissen, können Sie anfordern, damit durch den Aufruf in einem Hintergrundthread geladen werden `BackgroundLoadResource()`. Sie können auf die Ressource im Hintergrund geladen Ereignis abonnieren, indem die `SubscribeToResourceBackgroundLoaded` Methode. Sie werden feststellen, ob das Laden tatsächlich ein Erfolg oder Misserfolg wurde. Abhängig von der Ressource, die nur einen Teil des Ladevorgangs in einen Hintergrundthread verschoben werden kann, z. B. der migrationsbezogenen GPU Upload Schritt immer muss im Hauptthread ausgeführt werden. Beachten Sie, dass wenn Sie eine der Methoden für eine Ressource für das Laden Hintergrund in der Warteschlange befindlichen laden Ressource aufrufen, der Hauptthread installieren wird, bis der Ladevorgang abgeschlossen ist.

Der asynchrone Szene laden Funktionalität `LoadAsync()` und `LoadAsyncXML()` hat die Möglichkeit, im Hintergrund Laden der Ressourcen, die Sie zum Laden des Inhalts der Szene zunächst vor dem fortfahren. Kann auch auf die Ressourcen nur geladen, ohne Änderung der Szene durch Angabe verwendet werden die `LoadMode.ResourcesOnly`. Dies ermöglicht eine Szene oder Objekt prefab-Datei für schnelle Instanziierung vorbereiten.

Die maximale Zeit (in Millisekunden) schließlich für jeden Frame aufgewendete Schlichten Hintergrund geladene Ressourcen werden, indem konfiguriert können die `FinishBackgroundResourcesMs` Eigenschaft auf die `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Sound

Sound ist ein wichtiger Teil des Spiels, und die UrhoSharp-Framework bietet eine Möglichkeit der Wiedergabe von Tönen im Spiel.  Wiedergeben von Sounds Anfügen einer [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) -Komponente in einen [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) und diese dann aus Ihren Ressourcen eine benannte Datei wiedergeben.

Wie erfolgt sieht folgendermaßen aus:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Partikel

Partikel bieten eine einfache Möglichkeit, einige einfachen und kostengünstigen Auswirkungen auf Ihre Anwendung hinzufügen.  Sie können Partikel im PEX-Format gespeicherte nutzen, der mit Tools wie [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Partikel sind Komponenten, die zu einem Knoten hinzugefügt werden können.  Sie müssen des Knotens Aufrufen `CreateComponent<ParticleEmitter2D>` Methode, um das Partikel erstellen und konfigurieren Sie dann durch Festlegen von der Auswirkung-Eigenschaft auf einen 2D-Skalierungsvorgang Effekt, des Partikels aus dem Cache Ressource geladen wird.

Beispielsweise können Sie diese Methode aufrufen, in der Komponente, um einige Partikel anzuzeigen, die als eine Explosion gerendert werden, wenn es trifft:

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

Der obige Code erstellen einen Explosion-Knoten, der die der aktuellen Komponente angefügt ist, die innerhalb dieses Knotens Explosion wir erstellen eine Metadatenemitter 2D Partikel, und konfigurieren Sie es durch Festlegen der Eigenschaft wirksam.  Wir führen Sie zwei Aktionen, eine, die den Knoten, um kleinere werden skaliert und eine, die an diese Größe 0,5 Sekunden bleibt.  Klicken Sie dann entfernen wir die Auflösung, die wodurch auch die Partikel Auswirkungen aus dem Bildschirm entfernt.

Das oben genannten Partikel rendert wie folgt, wenn eine Textur Kugel mit:

![Mit einer Textur Kugel Partikel](using-images/image-1.png "das oben genannten Partikel wie folgt gerendert wird, bei Verwendung eine Textur Kugel")

Und wie er aussieht, wenn Sie eine Textur Blöcke verwenden:

![Partikel mit einer Textur Feld](using-images/image-2.png "und sieht wie er aussieht, wenn eine Textur Blöcke verwenden")

## <a name="multithreading-support"></a>Multithreadingunterstützung

UrhoSharp ist eine Singlethread Library.  Dies bedeutet, dass Sie nicht versuchen sollten, Aufrufen von Methoden in UrhoSharp aus einem Hintergrundthread, oder Sie besteht das Risiko, den Anwendungszustand beschädigen und wahrscheinlich, stürzt die Anwendung ab.

Wenn Sie Teil des Codes im Hintergrund ausgeführt und aktualisieren Sie dann auf die Hauptbenutzeroberfläche Urho-Komponenten möchten, können Sie mithilfe der [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) Methode.  Darüber hinaus können Sie c# "await" verwenden und die .NET task APIs verwendet, um sicherzustellen, dass der Code im entsprechenden Thread ausgeführt wird.

## <a name="urhoeditor"></a>UrhoEditor

Sie können den Urho-Editor für Ihre Plattform aus der [Urho Website](http://urho3d.github.io/), wechseln Sie zu Downloads, und wählen Sie die neueste Version.

## <a name="copyrights"></a>Urheberrechte

Diese Dokumentation umfasst den ursprünglichen Inhalt von Xamarin Inc, jedoch umfassend in der open-Source-Dokumentation für das Projekt Urho3D zeichnet und Screenshots aus dem Cocos2D-Projekt enthält.

## <a name="related-links"></a>Verwandte Links

- [Planeten Erde Arbeitsmappe](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Untersuchen die Arbeitsmappe Koordinaten](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
