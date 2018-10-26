---
title: BouncingGame-details
description: Dieses Dokument enthält eine exemplarische Vorgehensweise für die Erstellung von BouncingGame, eine einfache hüpfenden Ball-Spiels mit CocosSharp erstellt.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: 42155eb541849f365e7fab0a3d5c3ad583e760b7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109405"
---
# <a name="bouncinggame-details"></a>BouncingGame-details

> [!TIP]
> Der Code für BouncingGame finden Sie der [Xamarin-Beispiele](https://developer.xamarin.com/samples/mobile/BouncingGame/). Um am besten zusammen mit dieser Anleitung folgen, laden Sie, und untersuchen Sie den Code ein, wie im Handbuch darauf verweist.

Diese exemplarische Vorgehensweise veranschaulicht das Implementieren eines einfachen hüpfenden Ball-Spiels mit CocosSharp. 

 - Entzippen game-Inhalt
 - Allgemeine visual CocosSharp-Elemente
 - Hinzufügen von visuellen Elementen zu `GameLayer`
 - Every-Frame-Logik implementieren

Unser fertigen Spiel wird wie folgt aussehen:

![BouncingGame, abgeschlossen](bouncing-game-images/image1.png "BouncingGame, abgeschlossen")

## <a name="unzipping-game-content"></a>Entzippen game-Inhalt

Spieleentwickler verwenden oft den Begriff *Inhalt* um auf nicht-Codedateien zu verweisen, die in der Regel vom visual Künstler, game-Designer oder audio-Designer erstellt werden. Allgemeine Arten von Inhalten Includedateien, die zum Anzeigen von visuellen Elementen, sound wiedergeben oder künstliche Intelligenz (KI) Verhalten zu steuern. Aus Sicht des Teams eine Spiele-Entwicklung wird Inhalt in der Regel von nicht-Programmierern erstellt. Unser Spiel enthält zwei Arten von Inhalten:

 - PNG‑Dateien, die definieren, wie die Kugel und ein Paddel angezeigt werden.
 - Eine einzelne Xnb-Datei zum Definieren der Schriftart, die für die spielstandsanzeige (erläutert im Detail, wenn wir die folgenden Ergebnis-Anzeige hinzufügen)

Dieser Inhalt, der hier verwendete finden Sie im [Inhalt Zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Wir benötigen diese Dateien heruntergeladen und entzippt an einem Speicherort an, dem wir später in dieser exemplarischen Vorgehensweise zugreift.

## <a name="common-cocossharp-visual-elements"></a>Allgemeine visual CocosSharp-Elemente

CocosSharp bietet eine Reihe von Klassen verwendet, um Visualisierungen anzuzeigen. Es sind einige Elemente direkt sichtbar, während andere für die Organisation verwendet werden. Wir verwenden die folgenden im Spiel:

 - `CCNode` – Basisklasse für alle visuellen Objekte in CocosSharp. Die `CCNode` -Klasse enthält eine `AddChild` Funktion, die verwendet werden kann, erstellen Sie eine über-/unterordnungshierarchie, auch bezeichnet als eine *visuelle Struktur* oder *szenengraph*. Alle unten aufgeführten Klassen erben von `CCNode`.
 - `CCScene` – Stamm der visuellen Struktur für alle CocosSharp-Spiele. Alle visuellen Elemente müssen Mitglied einer visuellen Struktur mit einem `CCScene` beim Stamm an, oder sie wird nicht angezeigt werden.
 - `CCLayer` – Container für visuelle Objekte, z. B. `CCSprite`. Wie der Name schon sagt, den `CCLayer` Klasse wird verwendet, um die steuern, wie visuelle Elemente Ebene bauen aufeinander auf.
 - `CCSprite` – Zeigt ein Bild oder einen Teil eines Bilds. `CCSprite` Instanzen können positioniert werden, vergrößert, und eine Reihe von visuellen Effekten bereitstellen.
 - `CCLabel` – Zeigt eine Zeichenfolge auf dem Bildschirm an. Die Schriftart für `CCLabel` in eine Datei Xnb definiert ist. Xnbs im folgenden ausführlicher ist werden gleich erörtert.

Um zu verstehen, wie die verschiedenen Typen verwendet werden betrachten wir ein einzelnes `CCSprite`. Visuelle Elemente hinzugefügt werden müssen eine `CCLayer`, und die visuelle Struktur muss eine `CCScene` im Stammverzeichnis. Aus diesem Grund die über-/unterordnungsbeziehung für ein einzelnes `CCSprite` wäre `CCScene`  >  `CCLayer`  >  `CCSprite`.

## <a name="adding-visual-elements-to-gamelayer"></a>Hinzufügen von visuellen Elementen auf GameLayer

### <a name="adding-the-paddle-sprite"></a>Das Paddel Sprite hinzufügen

Zunächst legen wir das Spiel Hintergrund Schwarz, und fügen auch eine einzelne `CCSprite` auf dem Bildschirm rendern. Ändern der `GameLayer` -Klasse zum Hinzufügen einer `CCSprite` wie folgt:

```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of the drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

Der obige Code erstellt einen einzigen `CCSprite` und fügt Sie es als untergeordnetes Element der `GameLayer`. Die `CCSprite` Konstruktor ermöglicht uns die Image-Datei für die Verwendung als Zeichenfolge definieren. Unser Code weist CocosSharp, Suchen nach einer Datei mit `paddle` (die Erweiterung wird weggelassen, die später in diesem Handbuch erörtern wir). CocosSharp sucht nach Dateinamen `paddle` im Stammordner Content (d.h. **Inhalt**) sowie alle Ordner hinzugefügt der `gameView.ContentManager.SearchPaths` (im vorherigen Abschnitt erläutert).

Wir fügen die Dateien direkt in das Stammverzeichnis **Content** Ordner für iOS und Android. Zu diesem Zweck mit der rechten Maustaste oder Strg + Klick auf die **Content** Ordner im iOS-Projekt, und wählen **hinzufügen** > **Dateien hinzufügen...** Navigieren Sie zu, in dem wir die Inhalte zuvor entpackt, und wählen Sie **paddle.png**. Wenn Sie gefragt werden, dazu, wie Sie die Datei zu Ordner hinzufügen, wählen wir die **Kopie** Option:

![Die Datei hinzufügen, Dialogfeld "Ordner"](bouncing-game-images/image2.png "der Add File, Dialogfeld \"Ordner\"")

Als Nächstes fügen wir die Datei zum Android-Projekt. Mit der rechten Maustaste oder Strg + Klick auf den Ordner "Content" (dieser befindet sich in der **Assets** Ordner auf Android-Projekte), und wählen Sie **hinzufügen** > **Dateien hinzufügen...** . Diesmal, navigieren Sie zu den iOS-Projekt **Content** Ordner. Wenn Sie Informationen zum Hinzufügen der Datei aufgefordert wird, wählen Sie die **Hinzufügen eines Links** Option:

![Die Datei hinzufügen, Dialogfeld "Ordner"](bouncing-game-images/addalink.png "Datei das Hinzufügen, Dialogfeld \"Ordner\"")

Wir werden erläutern, warum Dateien an beide Projekte unten hinzugefügt werden musste. Jedes Projekts **Content** Ordner enthalten jetzt die **paddle.png** Datei:

![Der Inhalt der Ordner "Content"](bouncing-game-images/image3.png "den Inhalt der Ordner \"Content\"")

Beim Ausführen des Spiels sehen, dass die `CCSprite` gezeichnet werden:

![Das Paddel, auf dem Bildschirm gezeichnet](bouncing-game-images/image4.png "das Paddel, auf dem Bildschirm gezeichnet")

### <a name="file-details"></a>Dateidetails

Bisher haben wir eine einzelne Datei zum Projekt hinzugefügt, und der Prozess war ziemlich einfach. Wir einfach hinzugefügt, die **paddle.png** -Datei in die **Inhalt** Ordner und auf die sie im Code verwiesen wird. Werfen Sie einen Moment Zeit, einige Aspekte betrachten, bei der Arbeit mit Dateien in CocosSharp an.

#### <a name="capitalization"></a>Großbuchstaben

Beachten Sie, dass der Dateiname und die Zeichenfolge, die wir im Code verwendet, um Zugriff auf die Datei, die beide Kleinbuchstaben sind. Dies ist, da einige Plattformen (z. B. Windows-Desktop und iOS-Simulator) Groß-/Kleinschreibung sind, während andere Plattformen (z. B. Android und iOS-Gerät), Groß-/Kleinschreibung beachtet werden. Wir alle lower-case-Dateien für den Rest dieses Tutorials verwenden, damit die Dateien und den Code als plattformübergreifende bleiben wie möglich.

#### <a name="extensions"></a>Erweiterungen

Der Konstruktor für das Sprite erstellen schließt nicht die Erweiterung ".png", beim Verweis auf das Paddel-Datei. Die Erweiterung für Dateien wird in der Regel weggelassen, wenn das Arbeiten mit CocosSharp-Projekte als Dateierweiterungen für den gleichen Objekttyp zwischen Plattformen unterscheiden kann. Audiodateien können z. B. der .aiff,. MP3 oder .wma-Dateiformate, die auf der Plattform abhängig sein. Die Erweiterung für das Systemadministratorkonto ermöglicht es den gleichen Code funktioniert auf allen Plattformen, unabhängig von der Erweiterung aus.

#### <a name="content-in-platform-specific-projects"></a>Inhalte in den plattformspezifischen Projekten

Im Gegensatz zu den meisten die Codedateien, die in einer PCL werden können, müssen jedes plattformspezifische Projekt Inhaltsdateien hinzugefügt werden. CocosSharp erfordert dies für zwei Gründe:

1. Jede Plattform gibt es verschiedene **Buildvorgänge**. Inhalte hinzugefügt, die iOS-Projekte verwenden, sollten die **BundleResource** Buildvorgang. Inhalte hinzugefügt, die Android-Projekte verwenden, sollten die **AndroidAsset** Buildvorgang.
2. Einige Plattformen ist erforderlich, plattformspezifischen-Dateiformate. .Xnb Schriftartdateien unterscheiden sich z. B. zwischen iOS und Android, wie wir später in dieser exemplarischen Vorgehensweise sehen werden.

Wenn eine Datei im Format nicht zwischen verschiedenen Plattformen (z. B. PNG) unterscheidet, kann jede Plattform die gleiche Datei verwenden, wo eine oder mehrere Plattformen auf die Datei am selben Speicherort verknüpfen können.

## <a name="orientation"></a>Ausrichtung

Genau wie jede andere app können CocosSharp-apps im Hoch- oder Querformat Ausrichtungen ausführen. Passen wir das Spiel im reinen Hochformatansicht-Modus ausgeführt. Ändern Sie zunächst die auflösungscode im Spiel, einem Seitenverhältnis von Hochformat zu behandeln. Zu diesem Zweck Ändern der `width` und `height` Werte in der `LoadGame` -Methode in der die `ViewController` Klasse, die unter iOS und `MainActivity` Klasse, die unter Android:

```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    // ...
```

Als Nächstes müssen wir so ändern Sie jedes plattformspezifische Projekt im Hochformatmodus ausgeführt.

### <a name="ios-orientation"></a>iOS-Ausrichtung

Um Ausrichtung für das iOS-Projekt zu ändern, wählen Sie die **"Info.plist"** Datei die **BouncingGame.iOS** Projekt, und ändern Sie **iPhone/iPod Bereitstellungsinformationen** und die **iPad-Bereitstellungsinformationen** Hochformat-Ausrichtung nur einschließen:

![Ausrichtungsoptionen](bouncing-game-images/image5.png "Ausrichtungsoptionen")

### <a name="android-orientation"></a>Android-Ausrichtung

Um das Android-Projekt Ausrichtung zu ändern, öffnen Sie die "MainActivity.cs" im Projekt BouncingGame.Android aus. Ändern Sie die Definition der Aktivität-Attribut, sodass es nur eine Hochformat wie folgt angibt:

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>Standardkoordinatensystem

Unser Code, die instanziiert ein `CCSprite`, legt die `PositionX` und `PositionY` Werte und 100. Standardmäßig bedeutet dies, dass die `CCSprite` Center an 100 Pixel nach oben und nach rechts neben der unteren linken Ecke des Bildschirms platziert. Das Koordinatensystem kann geändert werden, durch Anfügen einer `CCCamera` auf die `CCLayer`. Unsere wird nicht mit `CCCamera` in diesem Projekt, aber die Weitere Informationen zu `CCCamera` finden Sie in der [CocosSharp-API-Dokumentation](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

Die 100 Pixel genannten "game" Pixel im Gegensatz zu Pixel auf der Hardware. Dies bedeutet, dass dasselbe Spiel auf einem Gerät von einer anderen Auflösung (z. B. einem iPad im Vergleich zu einem iPhone) ausgeführten Objekte positioniert und ordnungsgemäß dimensioniert wird relativ zum physischen Bildschirm führt. Insbesondere sichtbaren Bereich des Spiels immer 1024 Pixel Höhe und Breite von 768 Pixel, da dies die Lösung haben wir angegeben, weiter oben in der `LoadGame` Methode.

## <a name="adding-the-ball-sprite"></a>Hinzufügen der Ball sprite

Nun, da wir vertraut mit den Grundlagen der Arbeit mit `CCSprite`, fügen wir die zweite `CCSprite` – einen Ball. Wir müssen folgende Schritte sind sehr ähnlich wie wir das Paddel erstellt `CCSprite`. 

Wir werden zunächst Hinzufügen der **ball.png** Image aus dem extrahierten Ordner in der iOS-Projekt **Content** Ordner. Wählen Sie diese Option **Kopie** Datei, die die **Content** Verzeichnis. Führen Sie die gleichen Schritte wie oben, um einen Link zu der **ball.png** -Datei in das Android-Projekt.

Als Nächstes erstellen Sie die `CCSprite` für diese Balls durch Hinzufügen eines Mitglieds aufgerufen `ballSprite` auf die `GameScene` Klasse sowie die von Instanziierungscode für die `ballSprite`. Nach Abschluss der `GameLayer` Klasse sieht wie folgt:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```

## <a name="adding-the-score-label"></a>Die Bewertung Bezeichnung hinzufügen

Das letzte visuelle Element, das Spiel hinzugefügt, wird eine `CCLabel` der Benutzer hat die Anzahl der anzuzeigenden haben erfolgreich den Ball. Die `CCLabel` mithilfe eine Schriftart, die im Konstruktor angegebene Zeichenfolgen auf dem Bildschirm angezeigt.

Fügen Sie folgenden Code zum Erstellen einer `CCLabel` -Instanz im `GameLayer`. Nach Abschluss des Vorgangs der Code sollte wie folgt aussehen:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    // ...
```

Beachten Sie, die die ScoreLabel verwendet eine `AnchorPoint` von `CCPoint.AnchorUpperLeft`, was bedeutet, dass die `PositionX` und `PositionY` Werte definieren die obere linke Position. Auf diese Weise Position der `scoreLabel` relativ zur linken oberen Rand des Bildschirms, ohne dass die Maße berücksichtigt werden müssen.

## <a name="implementing-every-frame-logic"></a>Every-Frame-Logik implementieren

Bisher enthält das Spiel eine statische Szene. Wir werden hinzufügen Logik, um zu steuern, die Verschiebung der Objekte in der Szene durch Hinzufügen von Code, der die Position der Objekte häufig aktualisiert. In diesem Fall wird der Code ausgeführt, sechzig Mal pro Sekunde – auch bezeichnet als 60 *Frames* pro Sekunde (es sei denn, die Hardware nicht dies häufig zu aktualisieren verarbeiten kann). Insbesondere werden wir hinzufügen Logik, um den Ball fallen, und für das Paddel, um das Paddel gemäß Eingabe zu verschieben und das Ergebnis des Spielers zu aktualisieren, jedes Mal, wenn der Ball das Paddel eintritt springt zu machen.

Die `Schedule` -Methode, die von bereitgestellte der `CCNode` Klasse, die wir der every-Frame-Logik für das Spiel hinzufügen können. Wir fügen den Code nach `// New code` an den Konstruktor GameLayer:

```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

Erstellen Sie jetzt eine `RunGameLogic` -Methode in der die `GameLayer` -Klasse, die gesamte Logik alle Frames gespeichert werden soll:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Der "float"-Parameter definiert, wie lange der Frame in Sekunden ist. Wir verwenden diesen Wert, wenn die Bewegung des Balls zu implementieren.

### <a name="making-the-ball-fall"></a>Machen den Ball fallen

Wir können den Ball fallen entweder manuell implementieren Schwerkraft Code oder mithilfe der integrierten Funktionen der Box2D in CocosSharp vornehmen. Das Physikmodul Simulation Box2D steht CocosSharp-Spiele. Es ist sehr leistungsfähig und effizient, erfordert jedoch das Schreiben von Setupcode. Da die Physiksimulation recht einfach ist, wird hier manuell implementiert werden.

Zum Implementieren der Schwerkraft müssen wir zum Speichern der X- und Y-Geschwindigkeit des Balls. Wir werden zwei Elemente zum Hinzufügen der `GameLayer` Klasse:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Als Nächstes implementieren wir können die fallender Logik in `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>Verschieben Sie das Paddel mit Touch-Punkts

Nun, da der Ball fallen wird, hinzugefügt horizontale Verschiebung das Paddel mithilfe der `CCEventListenerTouchAllAtOnce` Objekt. Dieses Objekt stellt eine Anzahl von Eingabe-bezogene Ereignisse bereit. In diesem Fall möchten wir benachrichtigt werden, wenn alle Berührungspunkte zu verschieben, sodass wir die Position des das Paddel anpassen können. Die `GameLayer` bereits instanziiert ein `CCEventListenerTouchAllAtOnce`, daher wir einfach weisen müssen die `OnTouchesMoved` delegieren. Zu diesem Zweck ändern Sie die AddedToScene-Methode wie folgt:

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

Als Nächstes implementieren wir `HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>Implementieren der Ball-Konflikt

Wenn wir das Spiel ausführen, nachdem wir sehen, dass der Ball über das Paddel liegt. Implementieren wir *Kollisionen* (Logik zum Reagieren auf überlappende Spielobjekte) in der every-Frame-Code. Da verschieben Änderung jedes Bild positioniert, Konflikt ist in der Regel auch jedes Einzelbild ausgeführt. Wir werden auch hinzufügen Geschwindigkeit auf der x-Achse der Ball trifft das Paddel, um das Spiel eine Herausforderung hinzuzufügen, aber dies bedeutet, dass wir benötigen, um zu verhindern, dass den Ball überschreiten den Rändern des Bildschirms. Machen wir dies der `RunGameLogic` Code nach dem Anwenden der Geschwindigkeit, um die `ballSprite` nach `// New Code`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```

## <a name="adding-scoring"></a>Hinzufügen der Bewertung

Nun, das Spiel gespielt werden wird, ist der letzte Schritt, Bewertung Logik hinzufügen. Zuerst hinzugefügt Mitglied Bewertung der GameLayer-Klasse, die mit dem Namen `score`:

```csharp
int score;
```

Wir verwenden diese Variable, um zu verfolgen das Ergebnis des Spielers, und klicken Sie mit angezeigt der `scoreLabel`. Sie den folgenden Code in der If-Anweisung in Hiermit `RunGameLogic` Wenn der Ball und ein Paddel überlappen:

```csharp
// ...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
// ...
```

Jetzt können wir das Spiel ausführen und sehen, dass das Spiel eine Bewertung, die erhöht werden anzeigt, da auf das Paddel der Ball animiert werden soll:

![Das Spiel abgeschlossen mit einer inkrementierten Bewertung](bouncing-game-images/image1.png "das Spiel abgeschlossen mit einer inkrementierten Bewertung")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird dargestellt, erstellen ein plattformübergreifendes Spiel mit Grafiken, Physik und Verwenden von CocosSharp Eingabe. Es ist der erste Schritt in den ersten Schritten mit CocosSharp-Spiele-Entwicklung. Wir haben einige der am häufigsten verwendeten Klassen in CocosSharp, wie Sie eine visuelle Struktur zu erstellen, damit Objekte ordnungsgemäß gerendert und zum Implementieren von Spiellogik der every-Frame.

In dieser exemplarischen Vorgehensweise behandelt nur einen kleinen Teil des was die CocosSharp-game-Engine bietet. Weitere Informationen und exemplarische Vorgehensweisen zur anderen CocosSharp-Themen, finden Sie unter [den Rest der CocosSharp Anleitungen](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Verwandte Links

- [Abgeschlossenes Spiel (Beispiel)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Spiele-Inhalt (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
