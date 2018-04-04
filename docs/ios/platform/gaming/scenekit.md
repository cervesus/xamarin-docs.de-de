---
title: SceneKit
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 7c00a3f6aed442eec402f34a5cea4b1895bb3685
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="scenekit"></a>SceneKit

Szene Kit ist eine 3D-Szene-Graph-API, die das Arbeiten mit 3D-Grafiken vereinfacht. Es wurde erstmals in OS X 10.8 und ist jetzt für iOS 8 stammen. Szene Kit erfordert erstellen beeindruckende 3D Visualisierungen und gelegentlichen 3D-Spielen Kenntnisse OpenGL keine. Baut auf allgemeine Konzepte der Szene-Diagramm und abstrahiert Szene Kit die Komplexitäten von OpenGL- und OpenGL-ES, die Dies erleichtert die sehr 3D hinzuzufügende Inhalt zu einer Anwendung. Wenn Sie eine OpenGL-Experte sind, hat Szene Kit allerdings bietet umfassende Unterstützung für die Bindung in den direkt mit OpenGL ebenfalls. Außerdem umfasst zahlreiche Funktionen, die 3D Grafiken, z. B. physikalische, ergänzen und mehrere andere Apple Frameworks, z. B. Animation Core, Core-Image und Sprite Kit sehr gut integriert.

Szene Kit ist äußerst einfach zur Bearbeitung. Es ist eine deklarative API, die Rendering übernimmt. Klicken Sie einfach eine Szene einrichten, fügen Eigenschaften und Szene Kit Handles das Rendering der Szene haben.

Zur Bearbeitung der Szene Kit Sie erstellen eine Szene Diagramm mit den `SCNScene` Klasse. Eine Szene enthält eine Hierarchie von Knoten, dargestellt durch Instanzen der `SCNNode`, Speicherorte in einem 3D-Bereich definieren. Jeder Knoten verfügt über Eigenschaften wie Geometry, Beleuchtung und Materialien, die seine Darstellung beeinflussen, wie in der folgenden Abbildung dargestellt:

![](scenekit-images/image7.png "Der SceneKit-Hierarchie") 

## <a name="create-a-scene"></a>Erstellen Sie eine Szene

Um eine Szene auf dem Bildschirm angezeigt werden zu gestalten, fügen Sie ihn ein `SCNView` durch die Sicht Szene Eigenschaft zuweisen. Darüber hinaus, wenn Sie Änderungen an der Szene vornehmen `SCNView` wird aktualisiert, um die Änderungen anzuzeigen.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Szenen können von Dateien, die über eine 3d Modellierungstool exportiert oder programmgesteuert aus einem geometrieprimitiv aufgefüllt werden. Dies ist z. B. zum Erstellen einer Kugel, und der Szene hinzugefügt:

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Hinzufügen von Licht

An diesem Punkt wird nicht die Kugel nichts angezeigt werden, da keine Licht vorhanden, in der Szene haben ist. Anfügen von `SCNLight` Instanzen Knoten erstellt Leuchten Szene Kit. Es gibt mehrere Typen von Leuchten, die von verschiedenen Formen von direktionale Beleuchtung bis hin zu ambient-Beleuchtung. Im folgenden Code wird z. B. ein Gehe Licht auf der Kugel erstellt:

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

Gehe Beleuchtung erzeugt eine diffuse Reflektion, die resultierende Sortierung in einer sogar Beleuchtung wie eine Taschenlampe scheint. Erstellen ambient Licht ähnelt, obwohl es keine Richtung hat, als es ebenso in allen Richtungen scheint. Denken Sie davon, wie die Stimmung Beleuchtung :) ausgeführt.

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Mit Leuchten vorhanden ist die Kugel jetzt in der Szene sichtbar.

![](scenekit-images/image8.png "Die Kugel ist in der Szene beim leuchtet sichtbar")
 
## <a name="adding-a-camera"></a>Hinzufügen einer Kamera

Hinzufügen einer Kamera (SCNCamera) in der Szene ändert der Sicht. Das Muster zum Hinzufügen der Kamera gleicht. Erstellen Sie die Kamera zu, fügen Sie es auf einen Knoten, und fügen Sie den Knoten in der Szene.

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

Wie Sie sehen, aus dem obigen Code Szene Kit Objekte erstellt werden können, verwenden von Konstruktoren oder aus der Create-Factory-Methode. Das Erstere ermöglicht mithilfe der Objektinitialisierersyntax c# zu verwendende wird zum größten Teil durch Einstellung.

Mit der Kamera vorhanden wird die gesamte Kugel für den Benutzer sichtbar:

![](scenekit-images/image9.png "Die gesamte Kugel ist für den Benutzer sichtbar")
 
Sie können zusätzliche Leuchten sowie der Szene hinzufügen. So sieht es mit ein paar weitere Gehe Leuchten illustriert:

![](scenekit-images/image10.png "Der Kugel mit ein paar weitere Gehe Leuchten")
 
Darüber hinaus durch Festlegen von `sceneView.AllowsCameraControl = true`, der Benutzer kann die Sicht mit einem Touch-Geste ändern.

### <a name="materials"></a>Material

Materialien werden mit der SCNMaterial-Klasse erstellt. Beispiel zum Hinzufügen eines Bilds auf der Kugel Oberfläche legen Sie das Bild für des Materials *breiten* Inhalt.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Diese Ebenen das Abbild auf den Knoten, wie unten dargestellt:

![](scenekit-images/image11.png "Überlagern das Abbild auf der Kugel")
 
Ein Material kann festgelegt werden, auf andere Arten von Beleuchtung zu reagieren. Beispielsweise das Objekt glänzend vorgenommen werden kann und seinen Inhalt zusätzlicher spiegelnder müssen festgelegt werden, damit zusätzlicher spiegelnder Reflexion, was zu einer hellen Stelle auf der Oberfläche an, wie unten dargestellt angezeigt:

![](scenekit-images/image12.png "Das Objekt vorgenommen glänzend mit zusätzlicher spiegelnder Reflektion, was zu einer hellen Stelle auf der Oberfläche")
 
Materialien sind sehr flexibel, sodass Sie mit nur wenigen Codezeilen viel zu erreichen. Angenommen, anstatt das Abbild auf den Diffus Inhalt festlegen, legen Sie sie auf den Reflektierend Inhalt stattdessen.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Die Affe scheinbar jetzt innerhalb der Kugel, unabhängig von der Sicht visuell befinden.

### <a name="animation"></a>Animation

Szene Kit ist für die Zusammenarbeit mit Animation. Sie können implizite oder explizite Animationen erstellen und rendern können auch eine Szene aus einer Animation Core Layer-Struktur. Beim Erstellen einer impliziten Animation bietet Szene Kit eine eigene Klasse Übergang `SCNTransaction`.

Hier ist ein Beispiel, das die Kugel dreht:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Sie können jedoch viel mehr als Drehung animieren. Viele Eigenschaften der Szene Kit sind animierbaren. Z. B. der folgende Code erstellt eine Animation des Materials `Shininess` zusätzlicher spiegelnder Reflektion zu erhöhen.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

Szene Kit ist sehr einfach zu verwenden. Er bietet eine Fülle von zusätzlichen Funktionen, z. B. Einschränkungen, physikalische, deklarativen Aktionen, 3D Text, Tiefe von Feld-Unterstützung, Sprite-Kit-Integration und Core-Image-Integration, um nur einige zu nennen.