---
title: SceneKit in Xamarin.iOS
description: Dieses Dokument beschreibt SceneKit, eine 3D-Szene-Graph-API, die Arbeit mit 3D-Grafiken zu vereinfacht, indem Sie die Komplexität der OpenGL außer acht gelassen.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 0944978b34c8e164acd6e829db177bf4fd72dea9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109860"
---
# <a name="scenekit-in-xamarinios"></a>SceneKit in Xamarin.iOS

SceneKit ist eine 3D-Szene-Graph-API, die die Arbeit mit 3D-Grafiken vereinfacht. Es wurde erstmals in OS X 10.8 und ist jetzt für iOS 8 stammen. Mit SceneKit erfordert immersive 3D Visualisierungen und casual 3D-Spiele erstellen Fachwissen in OpenGL keine. Aufbauend auf allgemeine Konzepte der Szene Graph, abstrahiert SceneKit die Komplexität der OpenGL und OpenGL-ES, können Sie es sehr einfach, Hinzufügen von 3D Inhalt zu einer Anwendung. Wenn Sie ein OpenGL-Experte sind, hat jedoch SceneKit hervorragende Unterstützung für die Bindung in den direkt mit OpenGL ebenfalls. Außerdem umfasst zahlreiche Features, die 3D-Grafiken, z. B. Physik ergänzen und mehrere andere Apple-Frameworks, z. B. Core Animation, Core-Image und Spritekit sehr gut integriert.

SceneKit ist sehr einfach zu. Es ist eine deklarative API, der Rendering übernimmt. Klicken Sie einfach eine Szene einrichten, Eigenschaften, und ein SceneKit-Handles das Rendering der Szene hinzufügen.

Arbeiten mit SceneKit Erstellen einer Szene Diagramm mit den `SCNScene` Klasse. Eine Szene enthält eine Hierarchie von Knoten, dargestellt durch Instanzen der `SCNNode`, Speicherorte im 3D-Raum definieren. Jeder Knoten verfügt über Eigenschaften, z. B. Geometrie, Beleuchtung und Materialien, die Darstellung zu beeinflussen, wie in der folgenden Abbildung dargestellt:

![](scenekit-images/image7.png "Der SceneKit-Hierarchie") 

## <a name="create-a-scene"></a>Erstellen einer Szene

Um eine Szene, die auf dem Bildschirm angezeigt werden zu machen, fügen Sie es ein `SCNView` durch die ansichtseigenschaft Szene zuweisen. Darüber hinaus, wenn Sie Änderungen, um der Szene vornehmen `SCNView` wird aktualisiert, um die Änderungen anzuzeigen.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Im Hintergrund können aus Dateien, die über ein 3D-Diagrammlayout Modellierungstool exportiert oder programmgesteuert aus einem geometrieprimitiv aufgefüllt werden. Dies ist z. B. das Erstellen von einer Kugel, und fügen ihn in die Szene:

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Hinzufügen von Licht

An diesem Punkt wird nicht die Kugel nichts angezeigt werden, da keine Licht in der Szene vorhanden ist. Anfügen von `SCNLight` Instanzen auf Knoten Lichter in SceneKit erstellt. Es gibt mehrere Arten von Licht, die von verschiedenen Formen der direktionalen Beleuchtung bis hin zu umgebenden Beleuchtung. Der folgende Code erstellt z.B. eine omnidirektionale Licht im Zweifelsfall die Kugel:

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

Omnidirektionale Beleuchtung erzeugt eine diffuse Reflektion zu einer sogar Beleuchtung, Art wie eine Taschenlampe fort. Erstellen von Umgebungslicht ist ähnlich, obwohl es keine Richtung hat, da es gleichmäßig in alle Richtungen seine ganze Stärke aus. Stellen Sie sich wie Stimmung Beleuchtung :)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Mit die Lichter vorhanden wird die Kugel jetzt in der Szene angezeigt.

![](scenekit-images/image8.png "Die Kugel ist sichtbar, in der Szene, wenn markiert")
 
## <a name="adding-a-camera"></a>Hinzufügen einer Kamera

Hinzufügen einer Kamera (SCNCamera) zu einer Szene ändert der Sicht. Das Muster, um die Kamera hinzuzufügen ist ähnlich. Erstellen Sie die Kamera, fügen Sie ihn an einen Knoten aus, und fügen Sie den Knoten, um der Szene.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Wie Sie sehen können, aus dem Code oben SceneKit-Objekte mithilfe von Konstruktoren erstellt werden können oder von der Factory-Create-Methode. Ersteres ermöglicht die Verwendung von C# welche größtenteils eine Sache von Vorlieben ist, aber die Syntax des Objektinitialisierers.

Mit der Kamera vorhanden ist wird die gesamte Kugel für den Benutzer sichtbar:

![](scenekit-images/image9.png "Die gesamte Kugel ist für den Benutzer sichtbar")
 
Sie können zusätzliche Beleuchtung sowie der Szene hinzufügen. So sieht es wie mit ein paar weitere omnidirektionale Licht:

![](scenekit-images/image10.png "Die Kugel mit ein paar weitere omnidirektionale Beleuchtung")
 
Darüber hinaus durch Festlegen von `sceneView.AllowsCameraControl = true`, der Benutzer kann die Sicht eine Touch-Geste ändern.

### <a name="materials"></a>Material

Materialien werden mit der SCNMaterial-Klasse erstellt. Legen Sie beispielsweise zum Hinzufügen eines Bilds auf der Kugel Oberfläche der Abbildung auf des Materials *diffuse* Inhalt.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Diese Ebenen des Images auf den Knoten, wie unten dargestellt:

![](scenekit-images/image11.png "Das Image auf der Kugel Schichten")
 
Ein Material kann festgelegt werden, um auf andere Arten von Beleuchtung zu reagieren. Beispielsweise das Objekt kann shiny vorgenommen werden, und haben der glänzenden Festlegung des Inhalts glänzende Reflektion, was zu einer hellen Stelle auf der Oberfläche, wie unten dargestellt angezeigt:

![](scenekit-images/image12.png "Das Objekt vorgenommen glänzend mit Glanzlichtern, was zu einer hellen Stelle auf der Oberfläche")
 
Materialien sind sehr flexibel, sodass Sie viel mit sehr wenig Code erreichen. Angenommen, statt festgelegt das Bild, das den Inhalt der diffusen es auf den reflektierende Inhalt stattdessen.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Jetzt scheint das Monkey-Objekt innerhalb der Kugel, unabhängig von der Sicht visuell befinden.

### <a name="animation"></a>Animation

SceneKit dient auch bei der Animation ordnungsgemäß funktionieren. Sie können implizite oder explizite Animationen erstellen und können sogar rendern eine Szene aus einer Core Animation Layer-Struktur. Beim Erstellen einer impliziten animationsengine bietet SceneKit eine eigene Klasse Übergang `SCNTransaction`.

Hier ist ein Beispiel, das die Kugel rotiert:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Sie können jedoch viel mehr Drehung animieren. Viele Eigenschaften von SceneKit sind animierbaren. Beispielsweise der folgende Code erstellt eine Animation des Materials `Shininess` die glänzende Reflektion zu erhöhen.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit ist sehr einfach zu verwenden. Es bietet eine Fülle zusätzlicher Funktionen, einschließlich Einschränkungen "," Physik "," deklarative Aktionen "," 3D-Text "," Tiefe feldunterstützung "," Spritekit-Integration "und" Core-Image-Integration, um nur einige zu nennen.