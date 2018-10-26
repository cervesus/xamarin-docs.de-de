---
title: iOS-Gaming-APIs in Xamarin.iOS
description: Dieser Artikel beschreibt die Gaming-Verbesserungen bereitgestellt, die von iOS 9, die verwendet werden können, um Grafiken und Audiofunktionen Ihrer Xamarin.iOS-Spiels zu verbessern.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: d8a531e495a19be7437d4a600e758028594248ab
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116003"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>iOS-Gaming-APIs in Xamarin.iOS

_Dieser Artikel beschreibt die Gaming-Verbesserungen bereitgestellt, die von iOS 9, die verwendet werden können, um Grafiken und Audiofunktionen Ihrer Xamarin.iOS-Spiels zu verbessern._

Apple hat mehrere technologische Verbesserungen an der Gaming-APIs unter iOS 9 vorgenommen, die Grafiken und Audio in einer Xamarin.iOS-app implementieren zu vereinfachen.
Dazu gehören sowohl einfache Entwicklung über die allgemeine Frameworks und der leitungsfähigkeit von dem iOS-Gerät GPU für verbesserte Geschwindigkeit und grafischen Möglichkeiten.

[![](images/flocking01.png "Ein Beispiel für eine ausgeführte flocking app")](images/flocking01.png#lightbox)

Dies schließt GameplayKit "," ReplayKit "," Model e/a-"," MetalKit "und"-Metal-Leistung Shader zusammen mit neuen, erweiterten Funktionen von Metall, SceneKit und SpriteKit.

In diesem Artikel werden alle Möglichkeiten zur Verbesserung Ihrer Xamarin.iOS-Spiels mit iOS 9-Spiele Verbesserungen eingeführt:

## <a name="introducing-gameplaykit"></a>Einführung in GameplayKit

Neue Apple GameplayKit-Framework bietet eine Reihe von Technologien, die es einfach macht, Spiele für iOS-Geräte zu erstellen, indem Sie weniger sich wiederholende, gemeinsamen Code, der für die Implementierung erforderlich. GameplayKit bietet Tools zum Entwickeln von die spielemechanismen, die mit einer Grafik-Engine (z. B. SceneKit oder SpriteKit) klicken Sie dann ganz einfach kombiniert werden können schnell ein abgeschlossenes Spiel übermitteln.

GameplayKit enthält einige allgemeine, Spiel spielen Algorithmen, wie z. B.:

- Ein Verhalten basiert, Agent-Simulation, mit dem Sie zum Definieren von Bewegungen und Ziele, die der Gegner automatisch fortsetzen wird.
- Eine Minmax künstliche Intelligenz für Turn-basierte Spiels.
- Ein System, Regel für datengesteuerte Spiellogik mit fuzzy Logik, um sich entwickelnden Krisen Verhalten bereitzustellen.

Darüber hinaus nimmt GameplayKit einen Baustein-Ansatz zur Spieleentwicklung mit einer modularen Architektur, die die folgenden Funktionen bietet:

- Zustandsautomat für die Behandlung von komplexen, prozeduralen Code-basierte Systeme in Spiels.
- Tools für die Bereitstellung von zufällig Spiel und nicht vorhersagbar sind festgelegt, ohne dass beim Debuggen von Problemen.
- Eine wieder verwendbare, in Komponenten gegliederte entitätsbasierten-Architektur.

Weitere Informationen zu GameplayKit finden Sie unter Apple [Gameplaykit Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) und [GameplayKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Beispiele für die GameplayKit

Werfen wir einen kurzen Blick auf einige Mechanismen einfachen Spiels in einer Spiele-Kit mit Xamarin.iOS-app implementieren.

### <a name="pathfinding"></a>Pathfinding

Pathfinding ist die Möglichkeit für ein KI-Element für ein Spiel, die das Rätsel orientieren.
Beispiel: ein 2D Gegner, suchen Sie nach ihrem Weg durch eine Maze oder ein 3D Zeichen über ein First-Person-Shooter World Terrain.

Beachten Sie in der folgenden Schritte aus:

[![](images/gkpathfindpath.png "Eine Beispiel-Pathfinding-Zuordnung")](images/gkpathfindpath.png#lightbox)

Mit Pathfinding C# Code kann eine Möglichkeit der Karten im Detail finden:

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

### <a name="classical-expert-system"></a>Klassische Expertensystem

Der folgende Codeausschnitt C# Code zeigt, wie GameplayKit verwendet werden kann, um eine klassische Expertensystem zu implementieren:

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

Basierend auf einer bestimmten Gruppe von Regeln (`GKRule`) und einen bekannten Satz von Eingaben, das Expertensystem (`GKRuleSystem`) wird die vorhersagbare Ausgabe erstellen (`fizzbuzz` in unserem Beispiel oben).

### <a name="flocking"></a>Flocking

Flocking ermöglicht, eine Gruppe von KI-gesteuerte Spiele Entitäten wie ein Bestand, Verhalten, in dem die Gruppe auf die Bewegungen und Aktionen in einer potenziellen Kunden-Entität wie ein Bestand des Unternehmens aktiv ist, oder eine Schule Fisch geradezu reagiert.

Der folgende Codeausschnitt C# Code implementiert flocking Verhalten mit GameplayKit SpriteKit für die Grafik anzuzeigen:

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

Als Nächstes implementieren Sie diese Szene in einem View-Controller:

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

Bei der Ausführung die wenig animierte _"Boids"_ wird Schwefel auf unseren Finger Taps halten:

[![](images/flocking01.png "Die wenig animierte Boids wird rund um die Finger Klicks Schwefel halten.")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Andere Apple-Beispiele

Zusätzlich zu den oben aufgeführten Beispielen hat Apple bereitgestellt, das folgende beispielapps, die zu transcodierende können C# und Xamarin.iOS:

- [FourInARow: Verwenden der GameplayKit Minmax Strategist für Gegner AI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: Mithilfe der Agents in GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: Erstellen eines plattformübergreifenden Spiels mit SpriteKit und GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

In iOS 9 hat Apple mehrere Änderungen und Ergänzungen, die Metal, die GPU mit geringem Mehraufwand Zugriff gewähren. -Metal-Computern mit können Sie die Grafiken und computing Potenzial Ihrer iOS-Apps maximieren.

Das-Metal-Framework umfasst die folgenden neuen Features:

- Neue Private und Tiefe Schablone Texturen für OS X.
- Verbesserte Schatten Qualität mit Tiefe gebundenen und separate Front- und Back-Schablone-Werten.
- Verbesserungen der-Metal-Shading Language und Standard-Metal-Bibliothek.
- Compute-Shader unterstützen eine größere Anzahl von Pixelformate an.

### <a name="the-metalkit-framework"></a>Das Framework MetalKit

Das Framework MetalKit bietet es sich um einen Satz von hilfsprogrammklassen und Funktionen, die den Arbeitsaufwand zum-Metal-Computern in einer iOS-app verwenden, zu reduzieren. MetalKit bietet Unterstützung für in drei wichtigen Bereichen:

1. Asynchrone Textur aus einer Vielzahl von Quellen, einschließlich allgemeiner Formate wie z. B. PNG, JPEG, KTX und PVR geladen.
2. Einfache Zugriff auf die e/a-Modell basieren Ressourcen für die Behandlung von Metal bestimmtes Modell. Diese Funktionen sind hoch optimiert worden, um effiziente Datenübertragung zwischen Modell e/a-Gitter und -Metal-Puffer bereitzustellen.
3. Vordefinierte-Metal-Ansichten und Ansicht-Verwaltung, die erheblich reduzieren des Codes erforderlich, um grafische Darstellungen in eine iOS-app anzuzeigen.

Weitere Informationen zu MetalKit finden Sie unter Apple [MetalKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [-Metal-Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [Frameworkverweis Metal-](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) und [-Metal-Computern Sprachführer Schattierung](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Shader-Framework-Metal-Leistung

Das Metal-Performance-Shader-Framework bietet einen hochgradig optimierten Satz von Grafiken und COMPUTE-basierte Shader für die Verwendung in Ihrem-Metal-basierte iOS-apps. Jeder Shader im Shader Leistung Metal-Framework speziell optimiert wurde, um hohe Leistung auf-Metal-Computern bereitzustellen. unterstützt iOS GPUs.

Mithilfe der Performance-Shader-Metal-Klassen, können Sie die höchstmögliche Leistung auf jeder bestimmte iOS GPU erreichen, ohne Ziel und Verwalten einzelner Codebasen. Metal-Performance-Shader können mit-Metal-Ressourcen wie Texturen und Puffer verwendet werden.

Das Metal-Performance-Shader-Framework bietet eine Reihe von allgemeinen Shader wie z. B.:

- **Gaußsche Blur** (`MPSImageGaussianBlur`)
- **Sobel Kantenerkennung** (`MPSImageSobel`)
- **Abbildung Histogramm** (`MPSImageHistogram`)

Weitere Informationen finden Sie unter Apple [Shading Language-Guide-Metal-](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Einführung in die Modell-e/a

Apple Modell e/a-Framework bietet einen umfassenden Überblick über die 3D-Objekte (z. B. Modelle und die zugehörigen Ressourcen). Modell-e/a enthält Ihre iOS-Spiele mit physischen basierende Materialien, Modelle und die Beleuchtung, die mit GameplayKit, -Metal-Computern und SceneKit verwendet werden kann.

Mit e/a-Modell können Sie die folgenden Arten von Aufgaben unterstützt:

- Importieren Sie die Beleuchtung, Materialien, mesh-Daten, Kamera und andere Szene basierende Informationen aus einer Vielzahl von beliebten Software und Spiele-Engine-Formaten.
- Verarbeiten oder Szene Informationen, wie z. B. die Prozedural erstellen Sky Domes oder Bake Beleuchtung in einem Mesh Texturen versehenen zu generieren.
- Funktioniert mit MetalKit SceneKit und GLKit um spielressourcen effizient in der GPU-Puffer für das Rendering zu laden.
- Exportieren Sie Szene Informationen in einer Vielzahl von beliebten Software und Spiele-Engine-Format.

Weitere Informationen zum Modell-e/a finden Sie unter Apple [Modellverweis für e/a-Framework](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>Einführung in ReplayKit

Apple Framework für neue ReplayKit können Sie einfach das Hinzufügen einer Aufzeichnung des Spiels zu Ihrer iOS-Spiel und ermöglicht dem Benutzer schnell und einfach bearbeiten und Freigeben dieses Video aus innerhalb der app.

Weitere Informationen finden Sie im Apple [also in sozialen Netzwerken ReplayKit und Game Center-Videos](https://developer.apple.com/videos/wwdc/2015/?id=605) und ihre [DemoBots: Entwickeln eines Cross Platform-Spiels mit SpriteKit und GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) Beispiel-app.

## <a name="scenekit"></a>SceneKit

Szene Kit ist eine 3D-Szene-Graph-API, die die Arbeit mit 3D-Grafiken vereinfacht. Es wurde erstmals in OS X 10.8 und ist jetzt für iOS 8 stammen. Mit Szene Kit ist die immersive 3D Visualisierungen und casual 3D-Spiele erstellen keine Kenntnisse OpenGL erforderlich. Aufbauend auf allgemeine Konzepte der Szene Graph, abstrahiert Szene Kit die Komplexität der OpenGL und OpenGL-ES, können Sie es sehr einfach, Hinzufügen von 3D Inhalt zu einer Anwendung. Wenn Sie ein OpenGL-Experte sind, hat jedoch Szene Kit hervorragende Unterstützung für die Bindung in den direkt mit OpenGL ebenfalls. Außerdem umfasst zahlreiche Features, die 3D-Grafiken, z. B. Physik ergänzen und mehrere andere Apple-Frameworks, z. B. Core Animation, Core-Image und Spritekit sehr gut integriert.

Weitere Informationen finden Sie unserem [SceneKit](~/ios/platform/gaming/scenekit.md) Dokumentation.

### <a name="scenekit-changes"></a>SceneKit-Änderungen

Apple hat SceneKit für iOS 9 die folgenden neuen Features hinzugefügt:

- Xcode bietet jetzt eine Szene-Editor, der Ihnen ermöglicht, Spiele und interaktiven 3D apps schnell erstellen, durch Bearbeiten von Szenen direkt von innerhalb von Xcode.
- Die `SCNView` und `SCNSceneRenderer` Klassen können verwendet werden, um-Metal-Seiten (für unterstützte iOS-Geräte) zu aktivieren.
- Die `SCNAudioPlayer` und `SCNNode` Klassen können verwendet werden, um räumliche Audioeffekte hinzufügen, mit denen ein iOS-app eine Position des Spielers automatisch nachverfolgt.

Weitere Informationen finden Sie unsere [SceneKit-Dokumentation](~/ios/platform/introduction-to-ios8.md#scenekit) und Apple [SceneKit-Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) und [Fox: erstellen ein SceneKit-Spiel mit dem Xcode Szene Editor](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)Beispielprojekt.

## <a name="spritekit"></a>SpriteKit

Spritekit, dem 2D-spieleframework bei Apple an, verfügt über einige interessante neue Features in iOS 8 und OS X Yosemite. Dazu zählen die Integration mit Szene Kit, Unterstützung für Shader, Beleuchtung, Schatten, Einschränkungen, Normalmap-Generierung und physikalische Enhancements. Insbesondere vereinfachen die neuen Funktionen der Physik, realistische Effekte zu einem Spiel hinzufügen.

Weitere Informationen finden Sie unserem [SpriteKit](~/ios/platform/gaming/spritekit.md) Dokumentation.

### <a name="spritekit-changes"></a>SpriteKit-Änderungen

Apple hat SpriteKit für iOS 9 die folgenden neuen Features hinzugefügt:

- Räumliche Audioeffekt, die der Position des Spielers mit verfolgt automatisch die `SKAudioNode` Klasse.
- Xcode bietet nun eine Szene-Editor und die Action-Editor zur einfachen 2D Spiele- und app-Erstellung.
- Bildlauf einfach game-Support durch neue Kamera-Knoten (`SKCameraNode`) Objekte.
- Auf iOS-Geräten, die Metal unterstützen wird SpriteKit automatisch für das Rendering, verwendet, auch wenn Sie bereits benutzerdefinierte OpenGL ES-Shader verwendet haben.

Weitere Informationen finden Sie unter unseren [SpriteKit-Dokumentation](~/ios/platform/introduction-to-ios8.md#spritekit) Apple [SpriteKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) und ihre [DemoBots: erstellen ein Spiel des Cross-Plattform mit SpriteKit und GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) Beispiel-app.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt die neuen Features für Spiele, iOS 9 bietet für Ihre Xamarin.iOS-apps.
Es wurde die GameplayKit und Modell e/a-eingeführt; die wichtigsten Verbesserungen Metall; und die neuen Features von SceneKit und SpriteKit.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
