---
title: SpriteKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 79f7fbf52b544416223d225457d3148d68bc29e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="spritekit"></a>SpriteKit

Sprite Kit, das Spiel 2D Framework von Apple, verfügt über einige interessante neue Features in iOS 8 und OS X Yosemite. Dazu zählen die Integration in Szene Kit, Shader-Unterstützung, Beleuchtung, Schatten, Einschränkungen, normale Zuordnung Generation und physikalische Erweiterungen. Insbesondere sollten es die neuen Funktionen für physikalische sehr einfach, ein Spiel realistische Effekte hinzufügen.

## <a name="physics-bodies"></a>Physikalische stellen

Sprite-Kit umfasst eine 2D, feste Text physikalische API. Jede Sprite verfügt über einen zugeordneten physikalische Text (`SKPhysicsBody`), die physikalische Eigenschaften wie z. B. Masse und Unstimmigkeiten sowie die Geometrie des Texts in der Welt physikalische definiert.

## <a name="creating-a-physics-body-from-a-texture"></a>Erstellen einen physikalische Text aus einer Textur
Sprite Kit unterstützt jetzt die Textur physikalische Hauptteil einer Sprite abgeleitet. Dies vereinfacht die Konflikte zu implementieren, die natürlicher aussehen.

Angenommen, beachten Sie in der folgenden Konflikt wie die Banane und Affe nahezu auf die Oberfläche des jedes Image in Konflikt stehen:
 
![](spritekit-images/image13.png "Die Banane und Affe in Konflikt stehen, nahezu auf die Oberfläche des jedes Bild")

Sprite Kit ermöglicht Erstellen solcher physikalische Text mit einer einzigen Codezeile. Rufen Sie einfach `SKPhysicsBody.Create` mit der Struktur und die Größe: Sprite. PhysicsBody = SKPhysicsBody.Create (Sprite. Textur, Sprite. Größe);

## <a name="alpha-threshold"></a>Alpha-Schwellenwert

Zusätzlich zum Festlegen von einfach die `PhysicsBody` Eigenschaft direkt auf die Geometrie, die in der Textur abgeleitet, Anwendungen können festgelegt und alpha-Schwellenwert zu steuern, wie die Geometrie abgeleitet ist. 

Alpha-Schwellenwert definiert den alpha Mindestwert, den eine Pixel benötigen, in der resultierenden physikalische Text eingeschlossen werden sollen. Der folgende Code verursacht z. B. in einem etwas anderen physikalische Text:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Die Auswirkung des Rollouts alpha-Schwellenwerts wie folgt Feinabstimmung den vorherigen Konflikt so, dass die Affe fällt über, wenn mit der Banane Kollision:

![](spritekit-images/image14.png "Die Affe fällt über, wenn mit der Banane Kollision")
 
## <a name="physics-fields"></a>Physikalische Felder

Eine andere hervorragende Ergänzung Sprite Kit ist das neue physikalische Feld unterstützen. Damit können Sie Aktionen wie das Vortex-Felder hinzufügen radiale Schwerpunkt und Spring-Felder, um nur einige zu nennen.

Physikalische Felder werden erstellt, die mithilfe der SKFieldNode-Klasse, die eine Szene genau wie alle anderen hinzugefügt wird `SKNode`. Es gibt eine Vielzahl von Factorymethoden auf `SKFieldNode` andere physikalische Felder erstellen. Sie können ein Feld für die Steifheit erstellen, durch den Aufruf `SKFieldNode.CreateSpringField()`, ein Feld radiale Schwerpunkt durch Aufrufen `SKFieldNode.CreateRadialGravityField()`usw.

`SKFieldNode` Außerdem verfügt über Eigenschaften zum Steuern von Feldattributen wie z. B. die Stärke der Feldbereich und die Dämpfung von Feld bewirkt, dass ein.

## <a name="spring-field"></a>Spring-Feld

Beispielsweise wird der folgende Code erstellt ein Feld für die Steifheit und der Szene hinzugefügt:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Sie können dann hinzufügen Sprites und ihre `PhysicsBody` Eigenschaften, damit der physikalische-Feld der Sprites auswirkt, wie im folgenden Code wird, wenn der Benutzer den Bildschirm berührt:

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

Dies bewirkt, dass die Bananen, wie eine Spring umgehen der Feldknoten oszilliert:

![](spritekit-images/image15.png "Die Bananen oszilliert, wie eine Spring um den Feldknoten")
 
## <a name="radial-gravity-field"></a>Radiale Schwerpunkt-Feld

Hinzufügen von einem anderen Feld entspricht. Im folgenden Code wird z. B. ein Feld für die radiale Schwerpunkt erstellt:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Daraus ergibt sich ein anderes Feld zu erzwingen, denen die Bananen Radial zu dem Feld per Pull abgerufen werden:

![](spritekit-images/image16.png "Die Bananen Raumfahrt Radial um das Feld")