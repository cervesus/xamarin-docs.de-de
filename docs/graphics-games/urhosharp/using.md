---
title: Verwenden von urhusharp zum Erstellen eines 3D-Spiels
description: Dieses Dokument bietet eine Übersicht über urhosharp und beschreibt Szenen, Komponenten, Formen, Kameras, Aktionen, Benutzereingaben, Sound und vieles mehr.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: f9ea9c4f6f2c920d903bd6f136dd0b98ddc7bf57
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574326"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>Verwenden von urhusharp zum Erstellen eines 3D-Spiels

Bevor Sie Ihr erstes Spiel schreiben, sollten Sie sich mit den Grundlagen vertraut machen: Einrichten der Szene, Laden von Ressourcen (enthält Ihre Grafik) und so erstellen Sie einfache Interaktionen für Ihr Spiel.

<a name="scenenodescomponentsandcameras"></a>

## <a name="scenes-nodes-components-and-cameras"></a>Szenen, Knoten, Komponenten und Kameras

Das Szenen Modell kann als komponentenbasiertes Szenen Diagramm beschrieben werden. Die Szene besteht aus einer Hierarchie von Szenen Knoten, beginnend mit dem Stamm Knoten, der auch die gesamte Szene darstellt. Jede `Node` verfügt über eine 3D-Transformation (Position, Drehung und Skala), einen Namen, eine ID sowie eine beliebige Anzahl von Komponenten.  Komponenten bringen einen Knoten in den Lebenszyklus, Sie können eine visuelle Darstellung ( `StaticModel` ) hinzufügen, Sie können Sound ( `SoundSource` ) ausgeben, Sie können eine Kollisions Grenze usw. bereitstellen.

Sie können Ihre Szenen und Setup Knoten mithilfe des [Urho-Editors](#urhoeditor)erstellen, oder Sie können in Ihrem c#-Code Aktionen ausführen.  In diesem Dokument erfahren Sie, wie Sie mithilfe von Code festlegen, welche Elemente erforderlich sind, um auf dem Bildschirm angezeigt zu werden.

Zusätzlich zum Einrichten der Szene müssen Sie einen einrichten `Camera` , der bestimmt, was dem Benutzer angezeigt wird.

### <a name="setting-up-your-scene"></a>Einrichten der Szene

In der Regel erstellen Sie dieses Formular mit ihrer Start Methode:

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

Das Rendern von 3D-Objekten, Audiowiedergabe, Physik und Skripterstellung für Logik-Updates wird aktiviert, indem verschiedene Komponenten in den Knoten durch Aufrufen von erstellt werden `CreateComponent<T>()` .  Richten Sie z. b. den Knoten und die Light-Komponente wie folgt ein:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Wir haben oberhalb eines Knotens mit dem Namen " `DirectionalLight` " erstellt und eine Richtung dafür festgelegt, aber nichts anderes.  Nun können wir den obigen Knoten in einen Licht ausgebenden Knoten umwandeln, indem wir ihm eine `Light` Komponente anfügen, mit `CreateComponent` :

```csharp
var light = lightNode.CreateComponent<Light>();
```

Komponenten, die in den selbst erstellt werden `Scene` , haben eine besondere Rolle:, um eine Szene weite Funktionalität zu implementieren. Sie sollten vor allen anderen Komponenten erstellt werden und umfassen Folgendes:

 `Octree`: implementiert räumliche Partitionierung und beschleunigte Sichtbarkeits Abfragen. Ohne diese 3D-Objekte können nicht gerendert werden.
 `PhysicsWorld`: Implementiert die Physik-Simulation. Physik Komponenten wie `RigidBody` oder `CollisionShape` können ohne diesen nicht ordnungsgemäß funktionieren.
 `DebugRenderer`: Implementiert das Debug-Geometrie Rendering.

Normale Komponenten wie `Light` , `Camera` oder`StaticModel`
sollte nicht direkt in der `Scene` , sondern in untergeordneten Knoten erstellt werden.

Die Bibliothek enthält eine Vielzahl von Komponenten, die Sie an Ihre Knoten anfügen können, um Sie in das Leben zu bringen: Benutzer sichtbare Elemente (Modelle), Sounds, starre Körper, Kollisions Formen, Kameras, Lichtquellen, Partikelemitter und vieles mehr.

### <a name="shapes"></a>Formen

Zur Unterstützung stehen verschiedene Formen als einfache Knoten im Urho. Shapes-Namespace zur Verfügung.  Dazu zählen Felder, Bereiche, Kegel, Zylinder und Flächen.

### <a name="camera-and-viewport"></a>Kamera und Viewport

Ähnlich wie bei der Beleuchtung sind Kameras Komponenten, daher müssen Sie die Komponente wie folgt an einen Knoten anfügen:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Damit haben Sie eine Kamera erstellt, und Sie haben die Kamera in der 3D-Welt abgelegt. der nächste Schritt besteht darin, die Kamera darüber zu informieren, dass `Application` es sich um die Kamera handelt, die Sie verwenden möchten. Dies geschieht mit folgendem Code:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

Und jetzt sollten Sie in der Lage sein, die Ergebnisse ihrer Erstellung anzuzeigen.

### <a name="identification-and-scene-hierarchy"></a>Identifizierungs-und Szenen Hierarchie

Im Gegensatz zu Knoten haben Komponenten keine Namen. Komponenten innerhalb desselben Knotens werden nur durch ihren Typ und Index in der Komponentenliste des Knotens identifiziert, der in der Erstellungs Reihenfolge ausgefüllt ist. beispielsweise können Sie die `Light` Komponente wie folgt aus dem `lightNode` obigen Objekt abrufen:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Sie können auch eine Liste aller Komponenten abrufen, indem Sie die- `Components` Eigenschaft abrufen, die ein-Objekt zurückgibt `IList<Component>` , das Sie verwenden können.

Wenn Sie erstellt werden, erhalten sowohl Knoten als auch Komponenten Szene-globale ganzzahlige IDs. Sie können mithilfe der Funktionen und aus der Szene abgefragt werden `GetNode(uint id)` `GetComponent(uint id)` . Dies ist viel schneller als z. b. Durchführung von rekursiven namensbasierten Szenen Knoten Abfragen.

Es gibt kein integriertes Konzept für eine Entität oder ein Spielobjekt. Vielmehr ist es dem Programmierer überlassen, die Knoten Hierarchie zu entscheiden, und in welchen Knoten die Skript Logik platziert werden soll. In der Regel werden frei verschiebende Objekte in der 3D-Welt als untergeordnete Elemente des Stamm Knotens erstellt. Knoten können mit oder ohne Namen mithilfe von erstellt werden `CreateChild` . Die Eindeutigkeit von Knoten Namen wird nicht erzwungen.

Jedes Mal, wenn eine hierarchische Komposition vorhanden ist, wird empfohlen (und tatsächlich notwendig, da Komponenten keine eigenen 3D-Transformationen aufweisen), um einen untergeordneten Knoten zu erstellen.

Wenn z. b. ein Zeichen in der Hand ein Objekt enthält, sollte das Objekt über einen eigenen Knoten verfügen, der dem Hand Strich des Zeichens (auch ein) übergeordnet wird `Node` .  Bei der Ausnahme handelt es sich um die Physik `CollisionShape` , die im Verhältnis zum Knoten einzeln offsetted und rotiert werden kann.

Beachten Sie, dass `Scene` die eigene Transformation als Optimierung beim Berechnen von von der Welt abgeleiteten Transformationen von untergeordneten Knoten absichtlich ignoriert wird. das Ändern der Transformation hat daher keine Auswirkungen und sollte unverändert bleiben (Position bei Ursprung, keine Drehung, keine Skalierung).

`Scene`Knoten können frei neu zugeordnet werden. Im Gegensatz dazu gehören Komponenten immer zu dem Knoten, an den Sie angefügt sind, und können nicht zwischen Knoten verschoben werden. Sowohl Knoten als auch Komponenten bieten eine `Remove` Funktion, um dies zu erreichen, ohne das übergeordnete Element durchlaufen zu müssen. Nachdem der Knoten entfernt wurde, sind keine Vorgänge auf dem betreffenden Knoten oder dieser Komponente sicher, nachdem diese Funktion aufgerufen wurde.

Es ist auch möglich, einen `Node` zu erstellen, der nicht zu einer Szene gehört. Dies ist beispielsweise nützlich, wenn eine Kamera in eine Szene wechselt, die geladen oder gespeichert werden kann, da die Kamera nicht zusammen mit der eigentlichen Szene gespeichert wird und nicht zerstört wird, wenn die Szene geladen wird. Beachten Sie jedoch, dass die Erstellung von Geometrie-, Physik-oder Skript Komponenten zu einem nicht angefügten Knoten und das anschließende verschieben in eine Szene zu einem späteren Zeitpunkt dazu führt, dass diese Komponenten nicht ordnungsgemäß funktionieren.

### <a name="scene-updates"></a>Szenen Aktualisierungen

Eine Szene, deren Updates aktiviert sind (Standard), wird bei jeder Hauptschleifen Iterations Funktion automatisch aktualisiert.  Der `SceneUpdate` Ereignishandler der Anwendung wird darauf aufgerufen.

Knoten und Komponenten können aus dem Szenen Update ausgeschlossen werden, indem Sie deaktiviert werden. Informationen hierzu finden Sie unter `Enabled` .  Das Verhalten hängt von der jeweiligen Komponente ab, aber wenn Sie eine drawable-Komponente deaktivieren, wird Sie auch unsichtbar, während Sie durch Debuggen einer Sound Quell Komponente nicht mehr sichtbar ist. Wenn ein Knoten deaktiviert ist, werden alle zugehörigen Komponenten unabhängig von Ihrem eigenen Aktivierungs-/deaktivitätsstatus als deaktiviert behandelt.

## <a name="adding-behavior-to-your-components"></a>Hinzufügen von Verhalten zu ihren Komponenten

Die beste Möglichkeit, Ihr Spiel zu strukturieren, besteht darin, eine eigene Komponente zu erstellen, die einen Akteur oder ein Element in Ihrem Spiel kapseltes.  Dadurch wird das Feature in das zugehörige Verhalten von den Assets, die für seine Anzeige verwendet werden, selbst enthalten.

Das einfachste Verfahren zum Hinzufügen von Verhalten zu einer Komponente besteht in der Verwendung von Aktionen, bei denen es sich um Anweisungen handelt, die Sie mit der asynchronen c#-Programmierung in eine Warteschlange  Dadurch kann das Verhalten Ihrer Komponente sehr hoch sein, und es ist einfacher zu verstehen, was passiert.

Alternativ können Sie genau steuern, was mit der Komponente passiert, indem Sie die Komponenteneigenschaften für jeden Frame aktualisieren (im Abschnitt Frame-basiertes Verhalten erläutert).

### <a name="actions"></a>Aktionen

Mithilfe von Aktionen können Sie Knoten sehr einfach hinzufügen.  Mithilfe von Aktionen können verschiedene Knoten Eigenschaften geändert und über einen bestimmten Zeitraum ausgeführt werden, oder Sie können mit einer bestimmten Animations Kurve mehrmals wiederholt werden.

Wenn Sie z. b. einen "Cloud"-Knoten in Ihrer Szene sehen, können Sie ihn wie folgt ausblenden:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Aktionen sind unveränderliche Objekte, die es Ihnen ermöglichen, die Aktion zum Verwenden von unterschiedlichen Objekten wiederzuverwenden.

Eine gängige Vorgehensweise besteht darin, eine Aktion zu erstellen, die den umgekehrten Vorgang ausführt:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Im folgenden Beispiel wird das-Objekt für Sie über einen Zeitraum von drei Sekunden ausgeblendet.  Sie können eine Aktion auch nach einem anderen ausführen, beispielsweise könnten Sie zuerst die Cloud verschieben und dann ausblenden:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Wenn beide Aktionen gleichzeitig ausgeführt werden sollen, können Sie die parallele Aktion verwenden und alle Aktionen bereitstellen, die parallel ausgeführt werden sollen:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

Im obigen Beispiel wird die Cloud gleichzeitig verschoben und ausgeblendet.

Sie werden feststellen, dass Sie c# verwenden `await` , sodass Sie linear über das gewünschte Verhalten nachzudenken können.

### <a name="basic-actions"></a>Grundlegende Aktionen

Dies sind die Aktionen, die in urhosharp unterstützt werden:

- Verschieben von Knoten: `MoveTo` , `MoveBy` , `Place` , `BezierTo` , `BezierBy` , `JumpTo` ,`JumpBy`
- Rotierende Knoten: `RotateTo` ,`RotateBy`
- Skalieren von Knoten: `ScaleTo` ,`ScaleBy`
- Ausblenden von Knoten: `FadeIn` , `FadeTo` , `FadeOut` , `Hide` ,`Blink`
- Tinting: `TintTo` ,`TintBy`
- Instants: `Hide` , `Show` , `Place` , `RemoveSelf` ,`ToggleVisibility`
- Schleifen: `Repeat` , `RepeatForever` ,`ReverseTime`

Andere erweiterte Funktionen umfassen die Kombination der `Spawn` `Sequence` Aktionen und.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Beschleunigung: Steuern der Geschwindigkeit Ihrer Aktionen

Die Beschleunigung ist eine Methode, die die Art und Weise der Animation der Animation anweist und die Animationen viel angenehmer machen kann.  Standardmäßig haben ihre Aktionen ein lineares Verhalten, z. b `MoveTo` . eine Aktion, die eine sehr robotische Bewegung hat.  Sie können Ihre Aktionen in eine Beschleunigungs Aktion einschließen, um das Verhalten zu ändern, z. b. eine, die die Bewegung langsam startet, das Ende beschleunigt und langsam erreicht ( `EasyInOut` ).

Hierzu können Sie eine vorhandene Aktion in eine Beschleunigungs Aktion umwickeln, z. b.:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Es gibt viele Beschleunigungs Modi, das folgende Diagramm zeigt die verschiedenen Beschleunigungs Typen und deren Verhalten auf dem Wert des Objekts, das Sie über den Zeitraum steuern, von Anfang bis Ende:

![Beschleunigungs Modi](using-images/easing.png "Dieses Diagramm zeigt die verschiedenen Beschleunigungs Typen und deren Verhalten auf dem Wert des Objekts, das Sie im Laufe der Zeit steuern.")

### <a name="using-actions-and-async-code"></a>Verwenden von Aktionen und Async-Code

In Ihrer `Component` Unterklasse sollten Sie eine Async-Methode einführen, die das Verhalten der Komponente vorbereitet und die Funktionalität dafür steuert.
Dann würden Sie diese Methode mithilfe des c#- `await` Schlüssel Worts aus einem anderen Teil des Programms aufrufen, entweder Ihrer `Application.Start` Methode oder als Reaktion auf einen Benutzer oder Story Point in der Anwendung.

Beispiel:

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

In der `Launch` obigen Methode werden drei Aktionen gestartet: der Roboter kommt in die Szene, diese Aktion ändert die Position des Knotens über einen Zeitraum von 0,6 Sekunden.  Da es sich hierbei um eine async-Option handelt, erfolgt dies gleichzeitig als nächste Anweisung, bei der es sich um den Aufrufen von handelt `MoveRandomly` .  Diese Methode ändert die Position des Roboters parallel zu einem zufälligen Speicherort.  Dies wird erreicht, indem zwei zusammengesetzte Aktionen durchgeführt werden, die Bewegung zu einer neuen Position und die ursprüngliche Position wiederholt wird, solange der Roboter aktiv bleibt.  Um die Dinge interessanter zu gestalten, wird der Roboter gleichzeitig auf dem Laufenden gehalten.  Die Aufnahme wird nur alle 0,1 Sekunden gestartet.

### <a name="frame-based-behavior-programming"></a>Frame basierte Verhaltens Programmierung

Wenn Sie das Verhalten der Komponente Frame Weise steuern möchten, anstatt Aktionen zu verwenden, sollten Sie die- `OnUpdate` Methode der `Component` Unterklasse überschreiben.  Diese Methode wird einmal pro Frame aufgerufen und nur aufgerufen, wenn Sie die receivesceneupdates-Eigenschaft auf "true" festlegen.

Das folgende Beispiel zeigt, wie Sie eine- `Rotator` Komponente erstellen, die dann an einen-Knoten angefügt wird, wodurch der Knoten gedreht wird:

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
            RotationSpeed.X  timeStep,
            RotationSpeed.Y  timeStep,
            RotationSpeed.Z  timeStep),
            TransformSpace.Local);
    }
}
```

Und so fügen Sie diese Komponente an einen Knoten an:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Kombinieren von Stilen

Sie können das Async/Action-basierte Modell zum Programmieren eines sehr großen Verhaltens verwenden, das für das Auslösen und vergessen der Programmierung ideal ist, aber Sie können auch das Verhalten der Komponente optimieren, um auch Update Code für jeden Frame auszuführen.

In der samplygame-Demo wird diese z. b. in der- `Enemy` Klasse verwendet, um das grundlegende Verhalten von Aktionen zu codieren, aber es wird auch sichergestellt, dass die Komponenten auf den Benutzer verweisen, indem Sie die Richtung des Knotens mit festlegen `Node.LookAt`

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

Szenen können im XML-Format geladen und gespeichert werden. Weitere Informationen finden Sie unter Funktionen `LoadXml` und `SaveXML` . Wenn eine Szene geladen wird, wird der gesamte vorhandene Inhalt (untergeordnete Knoten und Komponenten) zuerst entfernt. Knoten und Komponenten, die mit der-Eigenschaft als temporär gekennzeichnet sind, werden `Temporary` nicht gespeichert. Das Serialisierungsprogramm verarbeitet alle integrierten Komponenten und Eigenschaften, aber es ist nicht intelligent genug, um benutzerdefinierte Eigenschaften und Felder zu verarbeiten, die in den Komponenten Unterklassen definiert sind. Hierfür stehen jedoch zwei virtuelle Methoden zur Verfügung:

 `OnSerialize`Hier können Sie benutzerdefinierte Zustände für die Serialisierung registrieren.

 `OnDeserialized`Hier können Sie Ihre gespeicherten benutzerdefinierten Zustände abrufen.

In der Regel sieht eine benutzerdefinierte Komponente wie folgt aus:

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

### <a name="object-prefabs"></a>Objekt präfabs

Das Laden oder speichern ganzer Szenen ist nicht flexibel genug für Spiele, bei denen neue Objekte dynamisch erstellt werden müssen. Andererseits ist das Erstellen komplexer Objekte und das Festlegen ihrer Eigenschaften im Code ebenfalls mühsam. Aus diesem Grund ist es auch möglich, einen Szene Knoten zu speichern, der seine untergeordneten Knoten, Komponenten und Attribute enthält. Diese können später bequem als Gruppe geladen werden.  Ein solches gespeichertes Objekt wird häufig als vorfab bezeichnet. Hierzu stehen drei Möglichkeiten zur Verfügung:

- Im Code durch Aufrufen von `Node.SaveXml` für den Knoten
- Wählen Sie im Editor den Knoten im Fenster Hierarchie aus, und wählen Sie im Menü "Datei" die Option "Knoten speichern unter" aus.
- Mithilfe des Befehls "Node" in `AssetImporter` , der die Szenen Knoten Hierarchie und alle im Eingabemedien Objekt enthaltenen Modelle speichert (z. b. eine Collada-Datei)

Um den gespeicherten Knoten in einer Szene zu instanziieren, wird aufgerufen `InstantiateXml` . Der Knoten wird als untergeordnetes Element der Szene erstellt, kann jedoch nach diesem Vorgang frei neu zugeordnet werden. Position und Drehung zum Platzieren des Knotens müssen angegeben werden. Der folgende Code veranschaulicht, wie Sie eine vorfab `Ninja.xm` in eine Szene mit gewünschter Position und Drehung instanziieren:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Ereignisse

Urhuobjects gibt eine Reihe von Ereignissen aus, die in den verschiedenen Klassen, die Sie generieren, als c#-Ereignisse angezeigt werden.  Zusätzlich zum c#-basierten Ereignis Modell ist es auch möglich, eine der Methoden zu verwenden, die es `SubscribeToXXX` Ihnen ermöglichen, ein Abonnement Token zu abonnieren und beizubehalten, das Sie später zum Kündigen des Abonnements verwenden können.  Der Unterschied besteht darin, dass das erste von vielen Aufrufern abonniert werden kann, während das zweite nur ein Abonnement zulässt, aber den einfacheren Lambda-Stil ermöglicht, aber auch das einfache Entfernen des Abonnements ermöglicht.  Sie schließen sich gegenseitig aus.

Wenn Sie ein Ereignis abonnieren, müssen Sie eine Methode bereitstellen, die ein Argument mit den entsprechenden Ereignis Argumenten annimmt.

So abonnieren Sie z. b. ein Mausbutton-Down-Ereignis:

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

Mit Lambda-Stil:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

In manchen Fällen möchten Sie den Empfang von Benachrichtigungen für das Ereignis beenden, in diesen Fällen den Rückgabewert des Aufrufs der `SubscribeTo` -Methode speichern und die Methode "Abmelden" aufrufen:

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

Der vom Ereignishandler empfangene Parameter ist eine stark typisierte Ereignis Argument Klasse, die für jedes Ereignis spezifisch ist und die Ereignis Nutzlast enthält.

## <a name="responding-to-user-input"></a>Reagieren auf Benutzereingaben

Sie können verschiedene Ereignisse abonnieren, wie z. b. Tastatureingaben, indem Sie das Ereignis abonnieren und auf die übermittelte Eingabe Antworten:

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

In vielen Szenarios möchten Sie jedoch, dass Ihre Szenen Update Handler den aktuellen Status der Schlüssel bei der Aktualisierung überprüfen und Ihren Code entsprechend aktualisieren.  Beispielsweise kann Folgendes verwendet werden, um die Kameraposition basierend auf der Tastatureingabe zu aktualisieren:

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f)  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f)  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX  moveSpeed  timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>Ressourcen (Assets)

Zu den Ressourcen gehören die meisten Elemente in urhusharp, die während der Initialisierung oder Laufzeit aus dem Massenspeicher geladen werden:

- `Animation`-für Skelett Animationen verwendet
- `Image`-stellt Bilder dar, die in einer Vielzahl von Grafikformaten gespeichert sind
- `Model`-3D-Modelle
- `Material`-Material, das zum Rendering von Modellen verwendet wird.
- `ParticleEffect`- [beschreibt](https://urho3d.github.io/documentation/1.4/_particles.html) , wie ein Partikelemitter funktioniert, siehe "[Partikel](#particles)" weiter unten.
- `Shader`-benutzerdefinierte Shader
- `Sound`-Sounds für die Wiedergabe, siehe "[Sound](#sound)" weiter unten.
- `Technique`-Material Rendering-Techniken
- `Texture2D`-2D-Textur
- `Texture3D`-3D-Textur
- `TextureCube`-Cube-Textur
- `XmlFile`

Sie werden vom Subsystem verwaltet und geladen `ResourceCache` (verfügbar als `Application.ResourceCache` ).

Die Ressourcen selbst werden durch ihre Dateipfade bezogen auf die registrierten Ressourcen Verzeichnisse oder Paketdateien identifiziert. Standardmäßig registriert die Engine die Ressourcen Verzeichnisse `Data` und `CoreData` oder die Pakete, `Data.pak` `CoreData.pak` sofern diese vorhanden sind.

Wenn beim Laden einer Ressource ein Fehler auftritt, wird ein Fehler protokolliert, und es wird ein NULL-Verweis zurückgegeben.

Das folgende Beispiel zeigt eine typische Methode zum Abrufen einer Ressource aus dem Ressourcen Cache.  In diesem Fall wird eine Textur für ein UI-Element verwendet, die die- `ResourceCache` Eigenschaft aus der- `Application` Klasse verwendet.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Ressourcen können auch manuell erstellt und im Ressourcen Cache gespeichert werden, als ob Sie vom Datenträger geladen wurden.

Arbeitsspeicher Budgets können pro Ressourcentyp festgelegt werden: Wenn Ressourcen mehr Speicher beanspruchen als zulässig belegen, werden die ältesten Ressourcen aus dem Cache entfernt, wenn Sie nicht mehr verwendet werden. Standardmäßig sind die Speicher Budgets auf unbegrenzt festgelegt.

### <a name="bringing-3d-models-and-images"></a>Einbringen von 3D-Modellen und-Bildern

Urho3D versucht, nach Möglichkeit vorhandene Dateiformate zu verwenden, und definiert benutzerdefinierte Dateiformate nur, wenn dies unbedingt erforderlich ist, z. b. für Modelle (. MDL) und für Animationen (. ani). Für diese Arten von Assets bietet Urho einen Konverter- [assetimporter](https://urho3d.github.io/documentation/1.4/_tools.html) an, der viele beliebte 3D-Formate wie z. b. "f", "DAE", "3ds" und "obj" usw. nutzen kann.

Es gibt auch ein praktisches Add-in für Blender [https://github.com/reattiva/Urho3D-Blender](https://github.com/reattiva/Urho3D-Blender) , das Ihre Blender-Assets in dem für Urho3D geeigneten Format exportieren kann.

### <a name="background-loading-of-resources"></a>Laden von Ressourcen im Hintergrund

Wenn Ressourcen mit einer der-Methoden angefordert werden `ResourceCache` `Get` , werden Sie normalerweise sofort im Haupt Thread geladen. Dies kann bei Bedarf mehrere Millisekunden in Anspruch nehmen (Datei vom Datenträger laden, Daten analysieren, bei Bedarf auf GPU hochladen) und somit zu Framerate führen.

Wenn Sie im Voraus wissen, welche Ressourcen Sie benötigen, können Sie anfordern, dass Sie in einem Hintergrund Thread geladen werden, indem Sie aufrufen `BackgroundLoadResource` . Mithilfe der-Methode können Sie das Ereignis mit dem Ressourcen Hintergrund laden abonnieren `SubscribeToResourceBackgroundLoaded` . Es wird feststellt, ob das Laden tatsächlich erfolgreich oder fehlerhaft war. Abhängig von der Ressource kann nur ein Teil des Ladevorgangs in einen Hintergrund Thread verschoben werden, z. b. muss der Schritt zum Abschließen des GPU-Uploads immer im Haupt Thread erfolgen. Beachten Sie Folgendes: Wenn Sie eine der Methoden zum Laden von Ressourcen für eine Ressource aufzurufen, die für das Laden im Hintergrund in die Warteschlange gestellt wird, wird der Haupt Thread so lange angehalten, bis das Laden

Die asynchrone Funktionalität zum Laden von Szenen `LoadAsync` und `LoadAsyncXML` bietet die Möglichkeit, die Ressourcen zuerst zu laden, bevor der Inhalt der Szene geladen wird. Sie kann auch verwendet werden, um nur die Ressourcen zu laden, ohne die Szene zu ändern, indem Sie angeben `LoadMode.ResourcesOnly` . Dadurch kann eine Szene oder Objekt-vorfab-Datei für die schnelle Instanziierung vorbereitet werden.

Schließlich kann die maximale Zeit (in Millisekunden), die für jeden Frame zum Abschließen von im Hintergrund geladenen Ressourcen aufgewendet wurde, durch Festlegen der- `FinishBackgroundResourcesMs` Eigenschaft im konfiguriert werden `ResourceCache` .

<a name="sound"></a>

## <a name="sound"></a>Sound

Sound ist ein wichtiger Teil der Spiel Wiedergabe, und das urhusharp-Framework bietet eine Möglichkeit, Klänge in Ihrem Spiel zu spielen.  Sie spielen Sounds durch Anhängen eines`SoundSource`
Komponente in eine `Node` und Wiedergabe einer benannten Datei aus ihren Ressourcen.

Dies geschieht wie folgt:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"></a>

## <a name="particles"></a>Partikel

Mit Partikeln können Sie einfache und kostengünstige Effekte zu Ihrer Anwendung hinzufügen.  Sie können im Pex-Format gespeicherte Partikel mithilfe von Tools wie verarbeiten [http://onebyonedesign.com/flash/particleeditor/](http://onebyonedesign.com/flash/particleeditor/) .

Partikel sind Komponenten, die einem Knoten hinzugefügt werden können.  Sie müssen die-Methode des Knotens aufrufen `CreateComponent<ParticleEmitter2D>` , um das Partikel zu erstellen, und dann das Partikel konfigurieren, indem Sie die Effect-Eigenschaft auf einen 2D-Effekt festlegen, der aus dem Ressourcen Cache geladen wird.

Sie können diese Methode z. b. für die Komponente aufrufen, um einige Partikel anzuzeigen, die bei einem Anstieg als Explosion gerendert werden:

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

Mit dem obigen Code wird ein Explosions Knoten erstellt, der an die aktuelle Komponente angefügt wird. in diesem Explosions Knoten wird ein 2D-Partikelemitter erstellt und konfiguriert, indem die Eigenschaft "Effect" festgelegt wird.  Wir führen zwei Aktionen aus, eine, die den Knoten so skaliert, dass er kleiner ist, und einen, der diesen Wert für 0,5 Sekunden verlässt.  Dann entfernen wir die Explosion, wodurch auch der Partikeleffekt aus dem Bildschirm entfernt wird.

Das obige Partikel rendert bei Verwendung einer Kugel Textur wie folgt:

![Partikel mit einer Kugel Textur](using-images/image-1.png "Das obige Partikel rendert bei Verwendung einer Kugel Textur wie folgt.")

Und das ist das, was Sie sieht, wenn Sie eine grobe-Textur verwenden:

![Partikel mit einer boxtextur](using-images/image-2.png "Und dies ist das, was bei der Verwendung einer grobe-Textur aussieht.")

## <a name="multi-threading-support"></a>Multithreading-Unterstützung

Urhusharp ist eine Single Thread-Bibliothek.  Dies bedeutet, dass Sie nicht versuchen sollten, Methoden in urhusharp von einem Hintergrund Thread aufzurufen, oder dass Sie den Anwendungs Zustand beschädigen und den Anwendungs Zustand wahrscheinlich abstürzen.

Wenn Sie Code im Hintergrund ausführen und dann die Urho-Komponenten auf der Hauptbenutzer Oberfläche aktualisieren möchten, können Sie das`Application.InvokeOnMain(Action)`
-Methode.  Darüber hinaus können Sie die c#-Warnungs-und .net-Task-APIs verwenden, um sicherzustellen, dass der Code im richtigen Thread ausgeführt wird.

## <a name="urhoeditor"></a>Urhueditor

Sie können den Urho-Editor für Ihre Plattform von der [Urho-Website](http://urho3d.github.io/)herunterladen. wechseln Sie zu Downloads, und wählen Sie die neueste Version aus.

## <a name="copyrights"></a>Urheberrechte

Diese Dokumentation enthält Originalinhalte von xamarin Inc, zeichnet sich jedoch in der Open-Source-Dokumentation für das Urho3D-Projekt aus und enthält Screenshots aus dem Cocos2D-Projekt.
