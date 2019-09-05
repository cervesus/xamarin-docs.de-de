---
title: IOS-Gaming-APIs in xamarin. IOS
description: In diesem Artikel werden die neuen Spiele Erweiterungen von IOS 9 behandelt, mit denen Sie die Grafik-und Audiofeatures Ihres xamarin. IOS-Spiels verbessern können.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: fa78a596495b22ebb2c8b148aadb76261845ccdc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281263"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>IOS-Gaming-APIs in xamarin. IOS

_In diesem Artikel werden die neuen Spiele Erweiterungen von IOS 9 behandelt, mit denen Sie die Grafik-und Audiofeatures Ihres xamarin. IOS-Spiels verbessern können._

Apple hat einige technologische Verbesserungen an den Gaming-APIs in ios 9 vorgenommen, die die Implementierung von Spielgrafiken und Audiodaten in einer xamarin. IOS-App vereinfachen.
Diese umfassen sowohl die einfache Entwicklung durch allgemeine Frameworks als auch die Leistungsfähigkeit der GPU des IOS-Geräts, um die Geschwindigkeit und Grafikfunktionen zu verbessern.

[![](images/flocking01.png "Ein Beispiel für eine APP, die Flocking ausgeführt wird.")](images/flocking01.png#lightbox)

Dies umfasst gameplaykit, replaykit, Model I/O, Metal Kit und Metal Performance Shaders sowie neue, erweiterte Features von Metal, scenekit und spritekit.

In diesem Artikel werden alle Möglichkeiten vorgestellt, mit denen Sie Ihr xamarin. IOS-Spiel mit den neuen Spiel Erweiterungen von IOS 9 verbessern können:

## <a name="introducing-gameplaykit"></a>Einführung in gameplaykit

Das neue gameplaykit-Framework von Apple stellt eine Reihe von Technologien bereit, die das Erstellen von Spielen für IOS-Geräte vereinfachen, indem Sie die Menge des wiederkehrenden, allgemeinen Codes verringern, der für die Implementierung erforderlich ist. Gameplaykit bietet Tools zum Entwickeln der Spielmechanismen, die dann problemlos mit einem Grafik Modul (z. b. scenekit oder spritekit) kombiniert werden können, um schnell ein abgeschlossenes Spiel zu liefern.

Gameplaykit umfasst verschiedene, gängige Spiele Wiedergabe Algorithmen, wie z. b.:

- Ein Verhaltens basiertes, eine Agent-Simulation, die es Ihnen ermöglicht, Bewegungen und Ziele zu definieren, die von der Ki automatisch verfolgt werden.
- Eine MinMax-künstliche Intelligenz für Turn-basiertes Spiel spielen.
- Ein Regelsystem für datengesteuerte Spiellogik mit fuzzyargumenten zur Bereitstellung von emerstem Verhalten.

Außerdem nutzt gameplaykit einen Baustein Ansatz für die Spieleentwicklung mithilfe einer modularen Architektur, die die folgenden Features bietet:

- Zustands Automat für die Behandlung komplexer, prozeduraler Code basierter Systeme in Spiel spielen.
- Tools zum Bereitstellen von zufällisiertem Spiel spielen und Unvorhersehbarkeit ohne Probleme beim Debuggen.
- Eine wiederverwendbare, komponentenbasierte Architektur, die auf Entitäten basiert.

Weitere Informationen zu gameplaykit finden Sie unter Apple [gameplaykit Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) und [gameplaykit Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Gameplaykit-Beispiele

Werfen wir einen kurzen Blick auf die Implementierung einiger einfacher Spielmechanismen zur Spiel Wiedergabe in einer xamarin. IOS-App mithilfe des Game Play Kits.

### <a name="pathfinding"></a>Pfadsuche

Pathfind ist die Fähigkeit eines Ki-Elements eines Spiels, seine Art um das spielboard zu finden.
Beispiel: ein 2D Gegner, suchen Sie nach ihrem Weg durch eine Maze oder ein 3D Zeichen über ein First-Person-Shooter World Terrain.

Sehen Sie sich die folgende Karte an:

[![](images/gkpathfindpath.png "Ein Beispiel für eine Pfadsuche")](images/gkpathfindpath.png#lightbox)

Mithilfe der pathfind C# -Methode kann dieser Code eine Methode durch die Zuordnung finden:

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>Klassisches Expertensystem

Der folgende C# Code Ausschnitt zeigt, wie das gameplaykit verwendet werden kann, um ein klassisches Expertensystem zu implementieren:

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

Basierend auf einem bestimmten Satz von Regeln (`GKRule`) und einem bekannten Satz von Eingaben erstellt das Expertensystem (`GKRuleSystem`) eine vorhersagbare Ausgabe`fizzbuzz` (für unser Beispiel oben).

### <a name="flocking"></a>Wird Flocking

Das Flocking ermöglicht es einer Gruppe von Ki-gesteuerten Spiel Entitäten, sich als eine Bewegung zu Verhalten, bei der die Gruppe auf die Bewegungen und Aktionen einer führenden Entität antwortet, wie z. b. eine Bewegung von Vögeln im Flug oder eine Schule von Fisch schwimmen.

Der folgende Code Ausschnitt implementiert C# das flockverhalten mithilfe von gameplaykit und spritekit für die Grafik Anzeige:

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

Implementieren Sie anschließend diese Szene in einem Ansichts Controller:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

Bei der Durchführung werden die kleinen animierten _"Boids"_ um unsere Fingerspitzen herum herum angezeigt:

[![](images/flocking01.png "Die kleinen animierten Boids bewegen sich um die Fingerspitzen.")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Weitere Apple-Beispiele

Zusätzlich zu den oben dargestellten Beispielen hat Apple die folgenden Beispiel-apps bereitgestellt, die in C# und xamarin. IOS transcodiert werden können:

- [Fourinarow: Verwenden des "gameplaykit MinMax"-Strategen für die Gegner-KI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [Agentscatalog: Verwenden des Agents System in gameplaykit](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [Demobots: Entwickeln eines plattformübergreifenden Spiels mit spritekit und gameplaykit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

In ios 9 hat Apple verschiedene Änderungen und Ergänzungen zu Metal vorgenommen, um den Zugriff auf die GPU mit geringem mehr Aufwand zu ermöglichen. Mit Metal können Sie die Grafik und das Rechen Potenzial ihrer IOS-apps maximieren.

Das Metal-Framework umfasst die folgenden neuen Features:

- Neue private und Tiefe Schablonen Texturen für OS X.
- Verbesserte Schatten Qualität durch tiefe Klammer und separate Werte für Vorder-und Hintergrund Schablone.
- Verbesserungen der Metal-Schattierungs Sprache und der Metal-Standard Bibliothek
- Compushader unterstützen eine größere Anzahl von Pixel Formaten.

### <a name="the-metalkit-framework"></a>Das Metal Kit-Framework

Das Metal Kit-Framework bietet eine Reihe von Hilfsprogrammklassen und-Funktionen, die den Arbeitsaufwand verringern, der für die Verwendung von Metal in einer IOS-App erforderlich ist. Metal Kit bietet Unterstützung in drei wichtigen Bereichen:

1. Asynchrones Textur laden aus einer Vielzahl von Quellen, einschließlich allgemeiner Formate wie PNG, JPEG, KTX und PVR.
2. Einfacher Zugriff auf Modell-e/a-basierte Ressourcen für die Verarbeitung von Metal-spezifischen Modellen. Diese Features wurden stark optimiert, um eine effiziente Datenübertragung zwischen Modell-e/a-Netzen und-Metal-Puffern zu ermöglichen.
3. Vordefinierte Metal-Ansichten und Ansichts Verwaltung, mit denen die erforderliche Menge an Code zum Anzeigen von grafischen Renderings in einer IOS-App erheblich reduziert wird.

Weitere Informationen zu Metal Kit finden Sie in der Referenz zu Metal- [Frameworks](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356)von Apple, im Handbuch zur Metal- [Programmierung](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), in der [Metal Framework-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) und in der [Metal-Schattierungs Sprache](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Metal Performance Shaders-Framework

Das Metal Performance Shader-Framework bietet einen hochgradig optimierten Satz von Grafiken und Berechnungs basierten Shadern für die Verwendung in ihren Metal-basierten IOS-apps. Jeder Shader im Metal Performance Shader-Framework wurde speziell für die Bereitstellung einer hohen Leistung bei von Metal unterstützten IOS-GPUs optimiert.

Durch die Verwendung von Klassen für den Metal Performance-Shader können Sie die höchstmögliche Leistung für jede bestimmte IOS-GPU erzielen, ohne einzelne Codebasen ausrichten und warten zu müssen. Metal-leistungshader können mit jeder beliebigen Metal-Ressource wie Texturen und Puffern verwendet werden.

Das Metal Performance Shader-Framework bietet eine Reihe allgemeiner Shader wie z. b.:

- **Gaußscher Weichzeichner** (`MPSImageGaussianBlur`)
- **Sobel Edge-Erkennung** (`MPSImageSobel`)
- **Bild Histogramm** (`MPSImageHistogram`)

Weitere Informationen finden Sie im Apple- [Handbuch zur Metal-Schattierung](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Einführung in die Modell-e/a

Das Modell-e/a-Framework von Apple bietet umfassende Kenntnisse über 3D-Assets (z. b. Modelle und zugehörige Ressourcen). Die Modell-e/a stellt Ihre IOS-Spiele mit physischen Materialien, Modellen und Beleuchtungs Vorgängen bereit, die mit gameplaykit, Metal und scenekit eingesetzt werden können.

Mit der Modell-e/a können Sie die folgenden Arten von Aufgaben unterstützen:

- Importieren von Beleuchtung, Materialien, Mesh-Daten, Kameraeinstellungen und anderen Szenen basierten Informationen aus einer Vielzahl von gängigen Software-und Spiel-Engine-Formaten.
- Verarbeiten oder generieren Sie Szenen basierte Informationen, wie z. b. das Erstellen von prozeduralen texturierten Himmels Kuppeln oder das Einklappen von Beleuchtung in einem
- Funktioniert mit Metal Kit, scenekit und glkit, um Spiel Ressourcen effizient zum Rendern in GPU-Puffer zu laden.
- Exportieren Sie Szenen basierte Informationen in eine Reihe von gängigen Software-und Spiel-Engine-Formaten.

Weitere Informationen zu Modell-e/a finden Sie in der [Referenz zum Modell](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421) -e/a-Framework von Apple.

## <a name="introducing-replaykit"></a>Einführung in replaykit

Das neue replaykit-Framework von Apple ermöglicht Ihnen das einfache Hinzufügen von Spiel spielen zu Ihrem IOS-Spiel und ermöglicht dem Benutzer, dieses Video in der app schnell und einfach zu bearbeiten und freizugeben.

Weitere Informationen finden Sie unter Apple [Social Social with replaykit und Game Center Video](https://developer.apple.com/videos/wwdc/2015/?id=605) und [demobots: Entwickeln eines plattformübergreifenden Spiels mit der spritekit-und](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) gameplaykit-Beispiel-app.

## <a name="scenekit"></a>SceneKit

Das Scene Kit ist eine 3D-Szene-Graph-API, die die Arbeit mit 3D-Grafiken vereinfacht. Sie wurde erstmals in OS X 10,8 eingeführt und ist jetzt auf IOS 8. Mit dem Szene-Kit, das immersive 3D-Visualisierungen erstellt, und ungezwungene 3D-Spiele erfordert kein Know-how in OpenGL. Das Scene Kit baut auf gängigen Szenen Diagramm Konzepten auf und abstrahiert die Komplexität von OpenGL und OpenGL, sodass es sehr einfach ist, 3D-Inhalte zu einer Anwendung hinzuzufügen. Wenn Sie jedoch ein OpenGL-Experte sind, bietet das Szene-Kit auch hervorragend Unterstützung für die direkte Bindung mit OpenGL. Außerdem sind zahlreiche Features enthalten, die 3D-Grafiken (z. b. Physik) ergänzen und sehr gut in verschiedene andere Apple-Frameworks integriert sind, z. b. Core Animation, Core Image und Sprite Kit.

Weitere Informationen finden Sie in unserer [scenekit](~/ios/platform/gaming/scenekit.md) -Dokumentation.

### <a name="scenekit-changes"></a>Scenekit-Änderungen

Apple hat die folgenden neuen Features von scenekit für IOS 9 hinzugefügt:

- Xcode bietet jetzt einen Szene-Editor, mit dem Sie schnell Spiele und interaktive 3D-Apps erstellen können, indem Sie Szenen direkt in Xcode bearbeiten.
- Die `SCNView` Klassen `SCNSceneRenderer` und können verwendet werden, um das Metal-Rendering (auf unterstützten IOS-Geräten) zu aktivieren.
- Mithilfe `SCNAudioPlayer` der `SCNNode` -Klasse und der-Klasse können Sie räumliche Audioeffekte hinzufügen, die automatisch die Position eines Players an eine IOS-App nachverfolgen.

Weitere Informationen finden Sie in unserer [scenekit-Dokumentation](~/ios/platform/introduction-to-ios8.md#scenekit) und in der [scenekit Framework](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) -Referenz [zu Apple: Ein scenekit-Spiel mit dem Beispiel Projekt "Xcode-Szene-Editor](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154) " zu entwickeln.

## <a name="spritekit"></a>SpriteKit

Sprite Kit, das 2D-Spiel Framework von Apple, verfügt über einige interessante neue Features in ios 8 und OS X Yosemite. Hierzu gehören die Integration in das Szene-Kit, shaderunterstützung, Beleuchtung, Schatten, Einschränkungen, normale Zuordnungs Generierung und physikalische Erweiterungen. Insbesondere die neuen Physik Features erleichtern das Hinzufügen realistischer Effekte zu einem Spiel.

Weitere Informationen finden Sie in unserer [spritekit](~/ios/platform/gaming/spritekit.md) -Dokumentation.

### <a name="spritekit-changes"></a>Spritekit-Änderungen

Apple hat das spritekit für IOS 9 die folgenden neuen Features hinzugefügt:

- Räumlicher Audioeffekt, bei dem die Position des Players automatisch `SKAudioNode` mit der-Klasse nachverfolgt wird.
- Xcode enthält jetzt einen Szene-Editor und einen Aktions-Editor für die einfache Erstellung von 2D-Spielen und-apps.
- Einfache scrollspiel Unterstützung mit neuen Kamera Knoten (`SKCameraNode`)-Objekten.
- Auf IOS-Geräten, die Metal unterstützen, wird das spritekit automatisch für das Rendering verwendet, auch wenn Sie bereits benutzerdefinierte OpenGL es-Shader verwendet haben.

Weitere Informationen finden Sie in unserer [spritekit-Dokumentation](~/ios/platform/introduction-to-ios8.md#spritekit) zur [spritekit-frameworkreferenz](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) von [Apple und ihren demobots: Entwickeln eines plattformübergreifenden Spiels mit der spritekit-und](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) gameplaykit-Beispiel-app.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Spiele Features behandelt, die IOS 9 für Ihre xamarin. IOS-apps bereitstellt.
Es wurden gameplaykit und Modell-e/a eingeführt. die wichtigsten Verbesserungen an Metal und die neuen Features von scenekit und spritekit.



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
