---
title: Spritekit in xamarin. IOS
description: Dieses Dokument beschreibt das spritekit, das 2D-Grafik Framework von Apple, das in scenekit integriert ist, die Physik und Animation umfasst, Unterstützung für Beleuchtung und Schattierung bietet und vieles mehr. Spritekit kann zum Erstellen von 2D-Spielen verwendet werden.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 9e897fbd35dc48e6e51e6a7df33759ac120d1e57
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938722"
---
# <a name="spritekit-in-xamarinios"></a>Spritekit in xamarin. IOS

Spritekit, das 2D-Grafik Framework von Apple, verfügt über einige interessante neue Features in ios 8 und OS X Yosemite. Dazu zählen die Integration in scenekit, shaderunterstützung, Beleuchtung, Schatten, Einschränkungen, normale Zuordnungs Generierung und physikalische Erweiterungen. Insbesondere die neuen Physik Features erleichtern das Hinzufügen realistischer Effekte zu einem Spiel.

## <a name="physics-bodies"></a>Physik Körper

Spritekit enthält eine 2D-, starre Body-Physik-API. Jedes Sprite verfügt über einen zugeordneten Physik Text ( `SKPhysicsBody` ), der die Physik Eigenschaften definiert, wie z. b. Masse und Reibung, sowie die Geometrie des Texts in der Physik-Welt.

## <a name="creating-a-physics-body-from-a-texture"></a>Erstellen eines Physik Texts aus einer Textur
Spritekit unterstützt jetzt das Ableiten des Physik Texts eines Sprite aus seiner Textur. Dadurch wird die Implementierung von Kollisionen, die natürlicher aussehen, vereinfacht.

Beachten Sie z. b. im folgenden Konflikt, wie sich der Banane und der Affe beinahe auf der Oberfläche jedes Bilds Konflikten:

![Die Banane und der Affe kollidieren fast auf der Oberfläche jedes Bilds.](spritekit-images/image13.png)

Spritekit ermöglicht das Erstellen eines solchen Physik Texts mit einer einzelnen Codezeile. Nennen Sie einfach `SKPhysicsBody.Create` mit der Textur und der Größe: Sprite. Physicsbody = skphysicsbody. Create (Sprite. Textur, Sprite. Größe);

## <a name="alpha-threshold"></a>Alpha Schwellenwert

Wenn Sie die `PhysicsBody` -Eigenschaft nicht nur direkt auf die von der Textur abgeleitete Geometrie festlegen, können Anwendungen einen Alpha Schwellenwert festlegen, um zu steuern, wie die Geometrie abgeleitet wird. 

Der Alpha-Schwellenwert definiert den minimalen Alpha Wert, den ein Pixel aufweisen muss, damit er in den resultierenden Physik Körper eingeschlossen werden muss. Der folgende Code führt z. b. zu einem etwas anderen Physik Körper:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Durch die Optimierung des Alpha Schwellenwerts, wie dies der Fall ist, wird der vorherige Konflikt so optimiert, dass der Affe bei einem Konflikt mit der Banane überschritten wird:

![Der Affe wird bei einem Konflikt mit der Banane überschritten.](spritekit-images/image14.png)

## <a name="physics-fields"></a>Physik-Felder

Eine weitere hervor artige Ergänzung zu spritekit ist die neue Unterstützung für Physik-Felder. Diese ermöglichen es Ihnen, Elemente wie z. b. die Felder "Vortex", "Radiale Schwerkraft" und "Spring Fields" hinzuzufügen.

Physik Felder werden mithilfe der skfieldnode-Klasse erstellt, die einer Szene wie jeder anderen hinzugefügt wird `SKNode` . Es gibt eine Vielzahl von Factorymethoden zum `SKFieldNode` Erstellen verschiedener Physik Felder. Sie können ein Spring-Feld erstellen, indem Sie aufrufen `SKFieldNode.CreateSpringField()` , ein radiales Schweregrad Feld durch Aufrufen von `SKFieldNode.CreateRadialGravityField()` usw.

`SKFieldNode`verfügt auch über Eigenschaften zum Steuern von Feld Attributen, wie z. b. die Feldstärke, den Feld Bereich und die Dämpfung von Feld Erzwingung.

## <a name="spring-field"></a>Spring Feld

Mit dem folgenden Code wird beispielsweise ein Spring Field erstellt und der Szene hinzugefügt:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Anschließend können Sie Sprites hinzufügen und Ihre `PhysicsBody` Eigenschaften so festlegen, dass sich das Feld "Physik" auf die Sprites auswirkt, wie der folgende Code bewirkt, wenn der Benutzer den Bildschirm berührt:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

Dies bewirkt, dass die Bananen wie eine Spring um den Feld Knoten laufen:

![Die Bananen laufen wie eine Spring um den Feld Knoten.](spritekit-images/image15.png)

## <a name="radial-gravity-field"></a>Radiales Schweregrad Feld

Das Hinzufügen eines anderen Felds ist ähnlich. Mit dem folgenden Code wird beispielsweise ein radiales Schwerpunkt Feld erstellt:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Dies führt zu einem anderen Feld "Force", in dem die Bananen durch radiale Informationen über das Feld gezogen werden:

![Die Bananen werden um das Feld herumgezogen.](spritekit-images/image16.png)
