---
title: BouncingGame-details
description: Dieses Dokument enthält eine exemplarische Vorgehensweise für die Erstellung von BouncingGame, ein einfaches springenden Ball Spiel CocosSharp integriert.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: 1a2423fcbeec0c3414b766a6c9e6f64ab53f4bf9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783106"
---
# <a name="bouncinggame-details"></a>BouncingGame-details

> [!TIP]
> Der Code für BouncingGame finden Sie der [Xamarin Samples](https://developer.xamarin.com/samples/mobile/BouncingGame/). Um optimale dieser Anleitung folgen, herunterladen Sie und untersuchen Sie den Code aus, wie im Handbuch darauf verweist.

In dieser exemplarischen Vorgehensweise wird gezeigt, wie ein einfache springenden Ball-Spiel mithilfe von CocosSharp implementiert. 

 - Entzippt Spiel Inhalt
 - Allgemeine CocosSharp visuelle Elemente
 - Visuelles Element, um hinzufügen `GameLayer`
 - Jeder Frame Logik implementieren

Unsere fertig Spiel wird wie folgt aussehen:

![BouncingGame, abgeschlossen](bouncing-game-images/image1.png "BouncingGame, abgeschlossen")

## <a name="unzipping-game-content"></a>Entzippt Spiel Inhalt

Entwickler verwenden oft den Begriff *Inhalt* um auf Dateien ohne Code zu verweisen, die in der Regel durch visual Künstler, game-Designer oder audio-Designer erstellt werden. Allgemeine Typen von Inhalten Includedateien, die zum Anzeigen von visuellen Elementen, sound wiedergeben oder KI (AI) Verhalten steuern. Aus Sicht der ein Spiel Entwicklungsteam wird Inhalt in der Regel von nicht-Programmierern erstellt. Unsere Spiel enthält zwei Typen von Inhalten:

 - PNG-Dateien definieren, wie die Kugel und Paddel angezeigt werden.
 - Eine einzelne Xnb-Datei definieren die Schriftart für die Bewertung Anzeige (ausführlicher beschrieben, wenn wir die Bewertung Anzeige unten hinzufügen)

Dieser Inhalt hier verwendete finden Sie in [Inhalte Zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Wir benötigen diese Dateien heruntergeladen und entzippt an einem Speicherort, den wir später in dieser exemplarischen Vorgehensweise zugreift.

## <a name="common-cocossharp-visual-elements"></a>Allgemeine CocosSharp visuelle Elemente

CocosSharp bietet eine Reihe von Klassen verwendet, um Visualisierungen anzuzeigen. Einige Elemente sind direkt sichtbar, während andere für die Organisation verwendet werden. Wir verwenden die folgenden im Spiel bisher:

 - `CCNode` – Basisklasse für alle visuellen Objekte in CocosSharp. Die `CCNode` Klasse enthält eine `AddChild` Funktion, die zum Erstellen einer Hierarchie über-und untergeordneten Elementen verwendet werden kann auch bezeichnet als eine *visuellen Struktur* oder *Szene Graph*. Alle unten aufgeführten Klassen erben von `CCNode`.
 - `CCScene` – Stamm der visuellen Struktur für alle CocosSharp Spiele. Alle visuellen Elemente muss Teil einer visuellen Struktur mit einem `CCScene` beim Stamm an, oder sie wird nicht angezeigt werden.
 - `CCLayer` – Container für visuelle Objekte, z. B. `CCSprite`. Wie der Name schon sagt, den `CCLayer` Klasse dient zum Steuern, wie visuelle Elemente Ebene bauen aufeinander auf.
 - `CCSprite` – Zeigt ein Bild oder einen Teil eines Bildes. `CCSprite` Instanzen können positioniert werden, geändert, und eine Reihe von visuellen Effekte bereitstellen.
 - `CCLabel` – Zeigt eine Zeichenfolge auf dem Bildschirm. Die Schriftart `CCLabel` in einer Datei Xnb definiert ist. Xnbs im folgenden ausführlicher erläutert.

Um zu verstehen, wie die verschiedenen Typen verwendet werden müssen wir gehen von einer einzelnen `CCSprite`. Visuelle Elemente hinzugefügt werden müssen eine `CCLayer`, und die visuelle Struktur muss eine `CCScene` auf der Stammebene. Aus diesem Grund die über-/unterordnungsbeziehung für eine einzelne `CCSprite` wäre `CCScene`  >  `CCLayer`  >  `CCSprite`.

## <a name="adding-visual-elements-to-gamelayer"></a>GameLayer hinzugefügt visuelle Elemente

### <a name="adding-the-paddle-sprite"></a>Hinzufügen von Paddel sprite

Anfänglich setzen wir das Spiel im Hintergrund auf Schwarz und auch hinzufügen, eine einzelne `CCSprite` Rendern auf dem Bildschirm. Ändern der `GameLayer` hinzuzufügende Klasse eine `CCSprite` wie folgt:

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

Der obige Code erstellt ein einzelnes `CCSprite` und fügt es als untergeordnetes Element von der `GameLayer`. Die `CCSprite` Konstruktor erlaubt es uns, definieren Sie die Bilddatei, die als eine Zeichenfolge verwendet. Getesteten Codes weist CocosSharp für eine Datei namens gesucht werden soll `paddle` (die Erweiterung wird weggelassen, die weiter unten in diesem Handbuch besprechen wir). CocosSharp sieht für alle Dateinamen `paddle` im Stamm-Ordners "Content" (also **Inhalt**) sowie alle Ordner hinzugefügt der `gameView.ContentManager.SearchPaths` (im vorherigen Abschnitt erläutert).

Wir fügen die Dateien direkt in das Stammverzeichnis **Content** Ordner für iOS und Android. Hierzu mit der rechten Maustaste oder Steuerelement klicken Sie auf die **Content** Ordner in der iOS-Projekt, und wählen **hinzufügen** > **Dateien hinzufügen...** Navigieren Sie zu, in denen wir zuvor den Inhalt entzippt, und wählen Sie **paddle.png**. Wenn Informationen zum Hinzufügen der das in Ordner aufgefordert werden, wählen wir die **Kopie** Option:

![Der Add File zum Dialogfeld "Ordner"](bouncing-game-images/image2.png "der Add File zum Dialogfeld \"Ordner\"")

Als Nächstes fügen wir die Datei auf das Android-Projekt. Mit der rechten Maustaste oder Steuerelement mit einem Klick auf den Inhaltsordner (Dies ist der **Bestand** Ordner auf Android-Projekte), und wählen Sie **hinzufügen** > **Dateien hinzufügen...** . Diesmal, navigieren Sie zu des iOS-Projekts **Content** Ordner. Wenn Sie Informationen zum Hinzufügen der das aufgefordert werden, wählen Sie die **einen Link hinzufügen** Option:

![Der Add File zum Dialogfeld "Ordner"](bouncing-game-images/addalink.png "der Add File zum Dialogfeld \"Ordner\"")

Wird besprochen, warum Dateien musste für beide Projekte unten hinzugefügt werden. Jedes Projekt **Content** Ordner enthalten jetzt die **paddle.png** Datei:

![Der Inhalt des Ordners "Content"](bouncing-game-images/image3.png "den Inhalt des Ordners \"Content\"")

Beim Ausführen des Spiels sehen wir die `CCSprite` gezeichnet werden:

![Das Paddel, auf dem Bildschirm gezeichnet](bouncing-game-images/image4.png "das Paddel, auf dem Bildschirm gezeichnet")

### <a name="file-details"></a>Dateidetails

Bisher haben wir eine einzelne Datei dem Projekt hinzugefügt, und der Vorgang war ziemlich selbsterklärend. Wir einfach hinzugefügt der **paddle.png** Datei wird in der **Content** Ordner, und es im Code verwiesen wird. Werfen Sie einen Moment Zeit, einige Aspekte beim Arbeiten mit Dateien im CocosSharp betrachten.

#### <a name="capitalization"></a>Großbuchstaben

Beachten Sie, dass der Dateiname und die Zeichenfolge, die wir im Code verwendet, um den Zugriff auf die Datei Kleinbuchstaben sind. Dies ist, da einige Plattformen (z. B. Windows-Desktop- und iOS-Simulator) Groß-/Kleinschreibung zu beachten sind, während andere Plattformen (z. B. Android und iOS-Gerät), Groß-/Kleinschreibung beachtet werden. Wir alle Kleinbuchstabe Dateien für den Rest dieses Lernprogramms verwenden, damit die Dateien und den Code als plattformübergreifende bleiben wie möglich.

#### <a name="extensions"></a>Erweiterungen

Der Konstruktor zum Erstellen der Sprite umfasst nicht die Erweiterung ".png" beim Verweis auf die Datei Paddel. Die Erweiterung für Dateien wird in der Regel ausgelassen werden, wenn zwischen den Plattformen arbeiten an CocosSharp Projekten als Dateierweiterungen für den gleichen Ressourcentyp unterscheiden kann. Audio-Dateien können z. B. .aiff, MP3 oder .wma Dateiformate, die abhängig von der Plattformaufrufs aufweisen. Wenn die Deaktivieren der Erweiterungs belassen, kann den gleichen Code funktioniert auf allen Plattformen unabhängig von der Erweiterung.

#### <a name="content-in-platform-specific-projects"></a>Inhalte in plattformspezifischen Projekte

Im Gegensatz zu den Großteil der Codedateien, eine einer PCL, Content-Dateien müssen jede plattformspezifisches Projekt hinzugefügt werden. CocosSharp erfordert dies für gibt es zwei Gründe:

1. Jede Plattform weist unterschiedliche **Buildvorgänge**. Inhalte hinzugefügt, die iOS-Projekten verwenden, sollten die **BundleResource** Buildvorgang. Inhalte hinzugefügt, Android-Projekte verwenden, sollten die **AndroidAsset** Buildvorgang.
2. Einige Plattformen erfordern plattformspezifischen Dateiformate. .Xnb Schriftartdateien unterscheiden sich z. B. zwischen IOS- und Android-Geräten, wie wir später in dieser exemplarischen Vorgehensweise sehen werden.

Unterscheiden sich ein Dateiformat nicht zwischen den Plattformen (z. B. PNG), können Sie dann jede Plattform dieselbe Datei, in denen eine oder mehrere Plattformen die Datei vom gleichen Speicherort verknüpfen können.

## <a name="orientation"></a>Ausrichtung

Genau wie jede andere app können CocosSharp apps im Hochformat oder Querformat Ausrichtungen ausführen. Es passen das Spiel im Hochformat Dateimodus ausgeführt. Ändern Sie zunächst den auflösungscode im Spiel bisher, einem Seitenverhältnis Hochformat zu behandeln. Zu diesem Zweck ändern die `width` und `height` Werte in der `LoadGame` Methode in der `ViewController` auf iOS-Klasse und `MainActivity` Klasse auf Android-Geräten:

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

Als Nächstes müssen wir jedes Projekt plattformspezifischen auszuführende im Hochformat zu ändern.

### <a name="ios-orientation"></a>iOS-Ausrichtung

Um die iOS-Projekt Ausrichtung zu ändern, wählen die **"Info.plist"** in der Datei die **BouncingGame.iOS** Projekt, und ändern Sie **iPhone/iPod Bereitstellungsinformationen** und die **iPad Bereitstellungsinformationen** nur Hochformat Ausrichtungen einschließen:

![Ausrichtung Optionen](bouncing-game-images/image5.png "Ausrichtung-Optionen")

### <a name="android-orientation"></a>Android-Ausrichtung

Um das Android-Projekt Ausrichtung zu ändern, öffnen Sie die MainActivity.cs-Datei im Projekt BouncingGame.Android. Ändern Sie die Aktivitätsdefinition-Attribut, sodass nur eine Hochformat wie folgt angegeben:

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>Standard-Koordinatensystem

Unsere Code instanziiert einen `CCSprite`, legt der `PositionX` und `PositionY` Werte und 100. Standardmäßig, bedeutet dies, dass die `CCSprite` Center wird auf 100 Pixel nach oben und nach rechts von der linken unteren Ecke des Bildschirms positioniert werden. Das Koordinatensystem kann geändert werden, durch das Anfügen einer `CCCamera` auf die `CCLayer`. Es wird nicht erreiche `CCCamera` in diesem Projekt, aber die Weitere Informationen zu `CCCamera` finden Sie in der [CocosSharp-API-Dokumentation](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

Die 100 Pixel erwähnten "game" Pixel im Gegensatz zu Pixel auf der Hardware. Dies bedeutet, dass das Ausführen des gleichen Spiels auf einem Gerät mit einer anderen Auflösung (z. B. einem iPad oder iPhone) Objekte positioniert wird und ordnungsgemäß dimensioniert wird relativ zum physischen Bildschirms führt. Insbesondere das Spiel sichtbaren Bereich immer 1024 Pixel hoch und 768 Pixel breit ist, wie sieht die Auflösung haben wir zuvor im angegebenen die `LoadGame` Methode.

## <a name="adding-the-ball-sprite"></a>Hinzufügen von Sprite Kugel

Nun, da wir mit den Grundlagen der Arbeit mit vertraut `CCSprite`, fügen wir die zweite `CCSprite` – eine Kugel. Wir müssen folgende Schritte, die sind sehr ähnlich wie das Paddel erstellten `CCSprite`. 

Zunächst, wir fügen die **ball.png** Image aus dem Ordner extrahiert, in der iOS-Projekt **Content** Ordner. Wählen Sie diese Option **Kopie** die Datei, die **Content** Verzeichnis. Führen Sie die gleichen Schritte wie oben, um einen Link zum Hinzufügen der **ball.png** -Datei in das Android-Projekt.

Als Nächstes erstellen Sie die `CCSprite` für diese Kugel durch Hinzufügen eines Mitglieds wird aufgerufen, `ballSprite` auf der `GameScene` Klasse als auch Instanziierungscode für die `ballSprite`. Nach Abschluss der `GameLayer` Klasse wird wie folgt aussehen:

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

## <a name="adding-the-score-label"></a>Die Bezeichnung Ergebnis hinzufügen

Diese, um das Spiel hinzugefügt wird letzten visuelle Element ist ein `CCLabel` die Anzahl der anzuzeigenden hat der Benutzer erfolgreich die Kugel abgewiesen. Die `CCLabel` verwendet eine Schriftart, die im Konstruktor angegebene Zeichenfolgen auf Bildschirm angezeigt werden sollen.

Fügen Sie den folgenden Code zum Erstellen einer `CCLabel` -Instanz im `GameLayer`. Einmal beendet der Code sollte wie folgt aussehen:

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

Beachten Sie, die die ScoreLabel verwendet ein `AnchorPoint` von `CCPoint.AnchorUpperLeft`, was bedeutet, dass die `PositionX` und `PositionY` Werte definieren die linke obere Position. Auf diese Weise Position der `scoreLabel` relativ zur linken oberen Rand des Bildschirms, ohne dass die Bezeichnung Dimensionen berücksichtigen.

## <a name="implementing-every-frame-logic"></a>Jeder Frame Logik implementieren

Bisher zeigt das Spiel statische themawechsel an Wir werden hinzufügen Logik, um zu steuern, die Verschiebung von Objekten in der Szene durch Hinzufügen von Code, der die Position der Objekte in sehr häufig aktualisiert. In diesem Fall führt der Code sechzig Male pro Sekunde – auch bezeichnet als hundert *Frames* pro Sekunde (es sei denn, die Hardware nicht dies oft zu aktualisieren behandeln kann). Insbesondere werden wir hinzufügen Logik, um die Kugel liegen und für das Paddel, verschieben Sie das Paddel gemäß Eingabe und Bewertung der Spieler zu aktualisieren, jedes Mal, wenn die Kugel das Paddel eintritt bounce vornehmen.

Die `Schedule` -Methode, die von bereitgestellte der `CCNode` Klasse, können wir das Spiel jeden Frame Logik hinzugefügt. Fügen wir den Code nach `// New code` an den Konstruktor GameLayer:

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

Erstellen Sie jetzt eine `RunGameLogic` Methode in der `GameLayer` -Klasse, die gesamte Logik jeden Frame gespeichert werden soll:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Der Parameter "float" definiert, wie lange der Rahmen in Sekunden ist. Dieser Wert wird verwendet werden bei der Implementierung der Ball bewegt.

### <a name="making-the-ball-fall"></a>Machen die Kugel liegen

Stellen wir die Kugel, die entweder manuell implementierende Schwerpunkt Code oder mithilfe der integrierten Box2D-Funktionen in CocosSharp liegen. Das Modul für Box2D physikalische Simulation steht CocosSharp Spiele. Er ist sehr leistungsfähig als auch effizient, erfordert jedoch Setupcode schreiben. Da die physikalische Simulation recht einfach ist, wird hier manuell implementiert werden.

Zum Implementieren der Schwerpunkt müssen wir zum Speichern der X- und Y-Geschwindigkeit des Balls. Wir werden zwei Elemente zum Hinzufügen der `GameLayer` Klasse:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Als Nächstes implementieren wir die Abnehmend Logik in `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>Verschieben Sie das Paddel mit Fingereingabe

Nun, da die Kugel zurück, wir fügen horizontale Verschiebung auf das Paddel mithilfe der `CCEventListenerTouchAllAtOnce` Objekt. Dieses Objekt stellt eine Anzahl von Eingabe-bezogene Ereignisse bereit. In diesem Fall möchten wir benachrichtigt werden, wenn alle Berührungspunkte zu verschieben, sodass wir die Position des das Paddel anpassen können. Die `GameLayer` bereits instanziiert einen `CCEventListenerTouchAllAtOnce`, sodass wir einfach zuweisen müssen die `OnTouchesMoved` delegieren. Zu diesem Zweck ändern Sie die Methode AddedToScene wie folgt:

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

### <a name="implementing-ball-collision"></a>Implementieren von Ball Kollisionsrate

Beim Ausführen des Spiels jetzt sehen wir, dass die Kugel über das Paddel liegt. Implementieren wir *Kollision* (Logik zum Reagieren auf sich überschneidende Spiel Objekte) im Code für jeden-Frame. Da verschieben Objekte ändern jeder Frame positioniert, Konflikte überprüfen in der Regel wird auch jeder Frame ausgeführt. Wir werden auch hinzufügen Geschwindigkeit auf der x-Achse bei der Kugel das Paddel trifft, um das Spiel einige Herausforderung hinzuzufügen, aber dies bedeutet, dass wir verhindern, dass die Kugel hinter den Rand des Bildschirms verschieben müssen. Es geschieht dies der `RunGameLogic` Code nach dem Anwenden der Geschwindigkeit, um die `ballSprite` nach `// New Code`:

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

## <a name="adding-scoring"></a>Hinzufügen von Bewertung

Nun, dass das Spiel wiedergegeben werden, ist der letzte Schritt, die Bewertungsprofile Logik hinzufügen. Diese wird Mitglied Bewertung zuerst hinzugefügt, auf die GameLayer-Klasse, die mit dem Namen `score`:

```csharp
int score;
```

Wir verwenden diese Variable zum Nachverfolgen der Spieler Bewertung und zum Anzeigen dieser mithilfe der `scoreLabel`. Soll dies fügen Sie folgenden Code in der If-Anweisung in `RunGameLogic` bei der Kugel und Paddel überschneiden:

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

Jetzt können wir führen Sie das Spiel und sehen, dass das Spiel ein Ergebnis, die erhöht werden anzeigt, wie die Kugel aus der Paddel abprallt:

![Das abgeschlossene Spiel, mit einem inkrementellen Bewertung](bouncing-game-images/image1.png "das abgeschlossene Spiel, mit einem inkrementellen Bewertung")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird dargestellt, Erstellen eines plattformübergreifenden Spiels mit Grafiken, physikalische und Eingabe mit CocosSharp. Es ist der erste Schritt beim Einstieg in die Entwicklung von CocosSharp. Erläutert einige der am häufigsten verwendeten Klassen im CocosSharp, wie Sie eine visuelle Struktur zu konstruieren, dass Objekte ordnungsgemäß gerendert und zum Implementieren von jedem Frame Spiellogik.

In dieser exemplarischen Vorgehensweise behandelt nur einen kleinen Teil der was das Spiele CocosSharp-Modul bietet. Informationen und exemplarische Vorgehensweisen auf anderen CocosSharp Themen finden Sie unter [den Rest der CocosSharp Anleitungen](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Verwandte Links

- [Abgeschlossene Spiel (Beispiel)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Inhalt (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
