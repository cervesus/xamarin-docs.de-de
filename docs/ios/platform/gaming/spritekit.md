---
title: SpriteKit in Xamarin.iOS
description: Dieses Dokument beschreibt SpriteKit, dem Apple 2D-Grafiken-Framework, das SceneKit integriert, enthält die Physik und Animationen, bietet Unterstützung für die Beleuchtung und Schattierung und vieles mehr. SpriteKit kann verwendet werden, um 2D-Spiele zu erstellen.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: ef1e9a98b76166f4ee5638d1ab9762896d1e3bc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121645"
---
# <a name="spritekit-in-xamarinios"></a>SpriteKit in Xamarin.iOS

SpriteKit, dem die 2D-Grafiken-Framework von Apple, verfügt über einige interessante neue Features in iOS 8 und OS X Yosemite. Dazu gehören die Integration von SceneKit, Unterstützung für Shader, Beleuchtung, Schatten, Einschränkungen, Normalmap-Generierung und physikalische Enhancements. Insbesondere vereinfachen die neuen Funktionen der Physik, realistische Effekte zu einem Spiel hinzufügen.

## <a name="physics-bodies"></a>Physik Texte

SpriteKit umfasst eine 2D, rigidbody Physik API. Jedes Sprite verfügt über einen zugeordneten Physik Text (`SKPhysicsBody`), definiert die Physik-Eigenschaften wie Massenspeicher und Reibung, als auch die Geometrie des Texts in der Physik-Welt.

## <a name="creating-a-physics-body-from-a-texture"></a>Erstellen einen Physik Text aus einer Textur
SpriteKit unterstützt jetzt an, dessen Struktur mit den Physik-Text, der ein Sprite abgeleitet. Dies erleichtert die Konflikte zu implementieren, die natürliche Aussehen.

Sehen Sie sich beispielsweise in den folgenden Konflikt wie die Banana "und" Monkey-Objekt fast in der Oberfläche der einzelnen Bilder Konflikt stehen:
 
![](spritekit-images/image13.png "Die Banana \"und\" Monkey-Objekt fast in der Oberfläche der einzelnen Bilder in Konflikt stehen")

Ein SpriteKit kann erstellt werden solche Physik Text mit einer einzelnen Zeile des Codes möglich. Rufen Sie ganz einfach `SKPhysicsBody.Create` mit der Textur und die Größe: Sprite. PhysicsBody = SKPhysicsBody.Create (Sprite. Textur "," Sprite ". Die Größe);

## <a name="alpha-threshold"></a>Der Alpha-Schwellenwert

Zusätzlich zu einfach die `PhysicsBody` Eigenschaft direkt auf die Geometrie, die von der Textur abgeleitet, die Anwendungen festlegen können und alpha-Schwellenwert zu steuern, wie die Geometrie abgeleitet wird. 

Der alpha Schwellenwert definiert, der minimale Alphawert, die eine Pixel in der resultierenden Physik-Text aufgenommen werden muss. Der folgende Code verursacht z. B. in einem etwas anderen Physik Text:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Die Auswirkungen der Anpassung des alpha Schwellenwerts folgendermaßen Feinabstimmung den vorherigen Konflikt, so, dass das Monkey-Objekt greift über, wenn Sie mit der Banane Konflikt zu geraten:

![](spritekit-images/image14.png "Das Monkey-Objekt greift über, wenn mit der Banane Konflikt zu geraten.")
 
## <a name="physics-fields"></a>Felder für physikalisches Verhalten

Eine andere wunderbare Ergänzung zur SpriteKit ist das neue Physik-Feld unterstützen. Diese ermöglichen es Ihnen beispielsweise Vortex Felder hinzufügen radiale Schwerkraft und Spring-Felder, um nur einige zu nennen.

Physik-Felder werden erstellt, die mithilfe der SKFieldNode-Klasse, die in einer Szene wie jede andere hinzugefügt wird `SKNode`. Es gibt eine Vielzahl von Factorymethoden auf `SKFieldNode` Physik der verschiedenen Felder erstellen. Können Sie eine Spring-Feld erstellen, durch den Aufruf `SKFieldNode.CreateSpringField()`, ein Feld radiale Schwerkraft durch Aufrufen von `SKFieldNode.CreateRadialGravityField()`und so weiter.

`SKFieldNode` Außerdem verfügt über Eigenschaften zum Steuern von Feldattributen wie z. B. die Stärke der, den Bereich des Felds und die Abnahme Feld bewirkt, dass ein.

## <a name="spring-field"></a>Spring-Feld

Beispielsweise der folgende Code erstellt ein Spring-Feld und fügt es der Szene hinzu:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Sie können dann füge Sprites hinzu und legen Sie ihre `PhysicsBody` Eigenschaften, damit das Feld "Physik" wie im folgenden Code wird, wenn der Benutzer den Bildschirm berührt die Sprites, betreffen:

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

Dies bewirkt, dass die Bananen, wie eine Feder rund um den Feldknoten einer:

![](spritekit-images/image15.png "Die Bananen einer wie einer Feder rund um den Feldknoten")
 
## <a name="radial-gravity-field"></a>Radiale Schwerkraft-Feld

Hinzufügen eines anderen Felds ist ähnlich. Der folgende Code erstellt z. B. ein Feld für die radiale Schwerkraft:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Dies ergibt ein anderes-Force-Feld, in denen die Bananen Radial über das Feld abgerufen werden:

![](spritekit-images/image16.png "Die Bananen werden Radial um das Feld abgerufen werden.")
