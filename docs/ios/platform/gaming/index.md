---
title: iOS-APIs spielen
description: "Dieser Artikel behandelt die neuen Gaming-Erweiterungen bereitgestellt, die von iOS 9, die verwendet werden können, um Ihr Xamarin.iOS Spiels Grafiken und audio Features zu verbessern."
ms.topic: article
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: d0a66d4cfdb3050c7ad791d24e24d6917a031ee1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="ios-gaming-apis"></a>iOS-APIs spielen

_Dieser Artikel behandelt die neuen Gaming-Erweiterungen bereitgestellt, die von iOS 9, die verwendet werden können, um Ihr Xamarin.iOS Spiels Grafiken und audio Features zu verbessern._

Apple hat mehrere technologische Verbesserungen an der Gaming-APIs in iOS 9, die zur Implementierung von Spiel Grafiken und Audio in einem Xamarin.iOS-app zu vereinfachen.
Dazu gehören sowohl einfache Entwicklung über allgemeine Frameworks und nutzen die Leistungsfähigkeit von dem iOS-Gerät GPU für verbesserte Geschwindigkeit und Grafik bietet.

[![](images/flocking01.png "Ein Beispiel für eine Anwendung flocking")](images/flocking01.png#lightbox)

Dies schließt GameplayKit, ReplayKit, Modell-e/a, MetalKit und Metall Leistung Shader zusammen mit der neuen, verbesserten Funktionen von Metall, SceneKit und SpriteKit.

In diesem Artikel werden alle die Möglichkeiten zur Verbesserung der Xamarin.iOS ein Spiel mit iOS 9 Spiele Verbesserungen vorgestellt:

## <a name="introducing-gameplaykit"></a>Einführung in GameplayKit

Apple neue GameplayKit Framework bietet eine Reihe von Technologien, die zum Erstellen von Spielen für iOS-Geräte durch Reduzieren des wiederholte, allgemeine Codes für die Implementierung erforderlich erleichtert. GameplayKit bietet Tools zum Entwickeln von die spielen Mechanismen, die dann problemlos mit einem grafischen Modul (z. B. SceneKit oder SpriteKit) kombiniert werden können, um schnell eine abgeschlossene Spiel übermitteln.

GameplayKit enthält einige allgemeine, Spiel Algorithmen wie z. B.:

- Ein Verhalten basiert, Agent-Simulation, mit dem Sie zum Definieren von Bewegungen und Ziele, die die AI automatisch erhalten wird.
- Eine Minmax KI für Turn-basierten spielen.
- Eine Regel System für datengesteuerte Spiellogik mit fuzzy Ansatzpunkt um unvorhergesehene Verhalten bereitzustellen.

Darüber hinaus akzeptiert GameplayKit Baustein-Ansatz zum Entwickeln von Spielen mit einer modularen Architektur, die die folgenden Features bietet:

- Zustandsautomat für die Behandlung von komplexen, prozeduralen Code-basierte Systeme im Spiel.
- Tools für die Bereitstellung von zufällige Spiel und nicht vorhersagbar sind, ohne debugging Probleme verursacht.
- Eine wieder verwendbare, als Komponente aufbereitete entitätsbasierten-Architektur.

Weitere Informationen zum GameplayKit finden Sie unter der Apple- [Gameplaykit Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) und [GameplayKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>GameplayKit-Beispiele

Werfen wir einen kurzen Blick auf einige einfache Spiel Mechanismen in einer Xamarin.iOS-app mithilfe von Spiel Kit implementieren.

### <a name="pathfinding"></a>Pathfinding

Pathfinding ist für ein Element AI eines Spiels die Möglichkeit, die Methode zur Umgehung Spielfelds zu suchen.
Beispiel: ein 2D Feind, suchen Sie nach ihrem Weg durch eine Maze oder ein 3D Zeichen über ein First-Person-Shooter World Terrain.

Beachten Sie in der folgenden Schritte aus:

[![](images/gkpathfindpath.png "Ein Beispiel Pathfinding-Karte")](images/gkpathfindpath.png#lightbox)

Mit Pathfinding kann eine Möglichkeit, über die Zuordnung dieses C#-Code finden:

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

### <a name="classical-expert-system"></a>Klassisches Expert-System

Der folgende Codeausschnitt der C#-Code zeigt, wie GameplayKit verwendet werden kann, um ein klassisches Expert-System zu implementieren:

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

Basierend auf einem angegebenen Satz von Regeln (`GKRule`) und einem bekannten Satz von Eingaben, die das System Expert (`GKRuleSystem`) vorhersagbare Ausgabe erstellen (`fizzbuzz` in unserem Beispiel oben).

### <a name="flocking"></a>Flocking

Flocking ermöglicht, dass eine Gruppe von AI gesteuert Spiele Entitäten wie ein Bestand, Verhalten, in die Gruppe auf die Bewegungen und Aktionen in einer potenziellen Kunden-Entität wie einem Vogelschwarm aktiv ist, oder Schule Fish schwimmend reagiert.

Der folgende Codeausschnitt der C#-Code implementiert flocking Verhalten GameplayKit und SpriteKit für die Anzeige von Grafiken verwenden:

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

Als Nächstes implementieren Sie diese Szene in einem Controller anzeigen:

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

Wann ausgeführt, die etwas animierte _"Boids"_ wird Schwefel halten, um unsere Finger tippen:

[![](images/flocking01.png "Wenig animierte Boids wird um die Finger Taps Schwefel halten.")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Weitere Beispiele für das Apple

Zusätzlich zu den oben aufgeführten Beispielen bereitgestellt Apple hat das folgende Beispiel-apps, die in c# und Xamarin.iOS transcodiert werden können:

- [FourInARow: Verwenden der GameplayKit Minmax-Strategie für Gegner AI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: Mithilfe der Agents in GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: Erstellen eines plattformübergreifenden Spiels mit SpriteKit und GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

In iOS 9 Apple mehrere Änderungen und Ergänzungen an Metall mit geringem Verwaltungsaufwand Zugriff auf die GPU ermöglicht vorgenommen. Mit Metall können Sie die Grafiken maximieren und computing Potenzial von iOS-apps verwenden.

Das-Metal-Framework beinhaltet die folgenden neuen Features:

- Neue Private und Tiefe Schablone Texturen für OS X an.
- Verbesserte Schatten Qualität mit Tiefe-gebundenen und separate vorderen und Back-Schablone-Werte.
- Metal Schattierung Sprache und die Standardbibliothek Metall Verbesserungen.
- Rechnerischen Shader unterstützen eine größere Anzahl an Pixelformate.

### <a name="the-metalkit-framework"></a>Das Framework MetalKit

Das MetalKit-Framework bietet eine Reihe von hilfsprogrammklassen und Funktionen, die den Arbeitsaufwand beim Metall in einer iOS-app verwenden, zu reduzieren. MetalKit bietet Unterstützung für in drei wichtigen Bereichen:

1. Asynchrone Textur aus einer Vielzahl von Quellen, einschließlich allgemeiner Formate wie z. B. PNG, JPEG, KTX und PVR laden.
2. Einfache Zugriff auf die e/a-Modell basiert Bestand für die Behandlung von Metal bestimmten Modell. Diese Funktionen sind hoch optimiert worden, um effiziente Datenübertragung zwischen Modell e/a-Netzen und Metal Puffer bereitzustellen.
3. Vordefinierte-Metal-Ansichten und Ansicht-Verwaltung, die den Umfang des Codes für die Anzeige von Grafiken Renderings innerhalb einer iOS-app erforderlichen erheblich reduzieren.

Weitere Informationen zum MetalKit finden Sie unter der Apple- [MetalKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [Metall Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [Metall Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) und [Metall Sprachführer Schattierung](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Shader-Framework-Metal-Leistung

Die Metall Leistung Shader-Framework stellt einen Satz stark optimiert, Grafiken und rechnerischen basierte Shader zur Verwendung in Ihrem Metall basierend iOS-apps. Jede Shader im Framework speziell optimiert wurden, und ermöglicht damit hohe Leistung auf Metall Metall Leistung Shader unterstützt iOS GPUs.

Mithilfe von Metall Leistung Shader-Klassen können Sie die höchste Leistung für jeden bestimmten iOS GPU möglich erreichen, ohne abzielen und einzelne Codebasen verwalten zu müssen. Bei allen anderen-Metal-Ressourcen wie z. B. Texturen und Puffer kann-Metal-Performance-Shaders verwendet werden.

Die Metall Leistung Shader-Framework bietet eine Reihe von allgemeinen Shader wie z. B.:

- **Gaussian Blur** (`MPSImageGaussianBlur`)
- **Sobel Edge-Erkennung** (`MPSImageSobel`)
- **Abbildung Histogramm** (`MPSImageHistogram`)

Weitere Informationen finden Sie in der Apple- [Metall Schattierung Sprachführer](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Einführung in Modell-e/a

Apple Modell e/a-Framework bietet ein tiefgreifendes Verständnis des 3D Ressourcen (wie z. B. Modelle und deren verknüpfte Ressourcen). Modell-e/a enthält Ihre iOS-Spielen mit physischen basierende Materialien, Modelle und die Beleuchtung, die mit GameplayKit, Metall und SceneKit verwendet werden kann.

Mit e/a-Modell können Sie die folgenden Arten von Aufgaben unterstützen:

- Importieren Sie die Beleuchtung ausgeführt, Materialien, mesh Daten, Kameraeinstellungen und andere Szene basierende Informationen aus einer Vielzahl von gängigen Software und Spiel-Engine-Formaten.
- Verarbeiten oder Szene basierende Informationen wie prozeduralen erstellen Sky Domes oder integrieren Beleuchtung in einem Netz strukturierten zu generieren.
- Funktioniert mit MetalKit, SceneKit und GLKit, um spielressourcen effizient in GPU-Puffer für das Rendering zu laden.
- Exportieren Sie Szene basierende Informationen in eine Vielzahl von beliebten Software und Spiel-Engine-Formate.

Weitere Informationen zum Modell e/a finden Sie unter Apple [Referenz zum e/a-Framework-Objektmodell](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>Einführung in ReplayKit

Apple neue ReplayKit Framework können Sie einfach Ihr iOS-Spiel Aufzeichnung des Spiels hinzu, und ermöglicht dem Benutzer schnell und einfach bearbeiten und Teilen dieses Video aus innerhalb der app.

Weitere Informationen finden Sie in der Apple- [sozialen passiert mit ReplayKit und Game Center-Video](https://developer.apple.com/videos/wwdc/2015/?id=605) und ihre [DemoBots: Erstellen eines Cross Platform-Spiels mit SpriteKit und GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) Beispiel-app.

## <a name="scenekit"></a>SceneKit

Szene Kit ist eine 3D-Szene-Graph-API, die das Arbeiten mit 3D-Grafiken vereinfacht. Es wurde erstmals in OS X 10.8 und ist jetzt für iOS 8 stammen. Szene Kit erfordert erstellen beeindruckende 3D Visualisierungen und gelegentlichen 3D-Spielen Kenntnisse OpenGL keine. Baut auf allgemeine Konzepte der Szene-Diagramm und abstrahiert Szene Kit die Komplexitäten von OpenGL- und OpenGL-ES, die Dies erleichtert die sehr 3D hinzuzufügende Inhalt zu einer Anwendung. Wenn Sie eine OpenGL-Experte sind, hat Szene Kit allerdings bietet umfassende Unterstützung für die Bindung in den direkt mit OpenGL ebenfalls. Außerdem umfasst zahlreiche Funktionen, die 3D Grafiken, z. B. physikalische, ergänzen und mehrere andere Apple Frameworks, z. B. Animation Core, Core-Image und Sprite Kit sehr gut integriert.

Weitere Informationen finden Sie unter unsere [SceneKit](~/ios/platform/gaming/scenekit.md) Dokumentation.

### <a name="scenekit-changes"></a>SceneKit Änderungen

Apple hat SceneKit für iOS 9 die folgenden neuen Funktionen hinzugefügt:

- Xcode bietet jetzt eine Szene-Editor, der Ihnen ermöglicht, Spiele und Interaktive 3D apps durch Bearbeiten der Szenen direkte innerhalb von Xcode schnell zu erstellen.
- Die `SCNView` und `SCNSceneRenderer` Klassen können verwendet werden, um Metal rendern (auf unterstützten iOS-Geräte) zu aktivieren.
- Die `SCNAudioPlayer` und `SCNNode` Klassen dienen zum räumliche audio Effekte hinzufügen, mit denen eine Player-Position einer iOS-app automatisch nachverfolgt.

Weitere Informationen finden Sie unter unsere [SceneKit Dokumentation](~/ios/platform/introduction-to-ios8.md#scenekit) und Apple [SceneKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) und [Fox: Erstellen eines Spiels SceneKit mit Xcode Szene Editor](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)Beispielprojekt.

## <a name="spritekit"></a>SpriteKit

Sprite Kit, das Spiel 2D Framework von Apple, verfügt über einige interessante neue Features in iOS 8 und OS X Yosemite. Dazu zählen die Integration in Szene Kit, Shader-Unterstützung, Beleuchtung, Schatten, Einschränkungen, normale Zuordnung Generation und physikalische Erweiterungen. Insbesondere sollten es die neuen Funktionen für physikalische sehr einfach, ein Spiel realistische Effekte hinzufügen.

Weitere Informationen finden Sie unter unsere [SpriteKit](~/ios/platform/gaming/spritekit.md) Dokumentation.

### <a name="spritekit-changes"></a>SpriteKit Änderungen

Apple hat SpriteKit für iOS 9 die folgenden neuen Funktionen hinzugefügt:

- Räumliche audio Auswirkungen, die der Spieler Position mit automatisch Nachverfolgen der `SKAudioNode` Klasse.
- Xcode bietet jetzt eine Szene-Editor und Aktionen-Editor zur einfachen 2D Spiel- und app-Erstellung.
- Einfache Bildlauf Spiel Unterstützung durch neue Kamera-Knoten (`SKCameraNode`) Objekte.
- Auf iOS-Geräten, die Metall unterstützen, wird SpriteKit automatisch für das Rendering, verwendet, auch wenn Sie benutzerdefinierte OpenGL ES-Shader bereits verwendet haben.

Weitere Informationen finden Sie unter unsere [SpriteKit Dokumentation](~/ios/platform/introduction-to-ios8.md#spritekit) Apple [SpriteKit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) und ihre [DemoBots: Erstellen eines Cross Platform-Spiels mit SpriteKit und GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) Beispiel-app.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat die neue abgedeckt Gaming-Funktionen, iOS 9 für Ihre apps Xamarin.iOS bereitstellt.
Er eingeführt GameplayKit und Modell e/a; zu den hauptsächlichen Erweiterungen zu Metall; und die neuen Funktionen von SceneKit und SpriteKit.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
