---
title: Teil 2 – Implementieren der WalkingGame
description: In dieser exemplarischen Vorgehensweise veranschaulicht die Spiellogik und Inhalt in ein leeres MonoGame-Projekt zum Erstellen einer Demo von eine animierte Sprite verschieben mit Fingereingaben hinzufügen.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 87678d9d77f75bccc68a667d3fb0f35b641b937c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="part-2--implementing-the-walkinggame"></a>Teil 2 – Implementieren der WalkingGame

_In dieser exemplarischen Vorgehensweise veranschaulicht die Spiellogik und Inhalt in ein leeres MonoGame-Projekt zum Erstellen einer Demo von eine animierte Sprite verschieben mit Fingereingaben hinzufügen._

Die frühere Teile des in dieser exemplarischen Vorgehensweise wurde gezeigt, wie leere MonoGame Projekte erstellen. Durch Erstellen eines einfachen spielen Demo wird auf diese frühere Teile erstellen. Dieser Artikel enthält folgende Abschnitte:

- Entzippen unsere Spiel Inhalte
- Übersicht über die MonoGame-Klasse
- Rendern von unseren ersten Sprite
- Erstellen die CharacterEntity
- Das Spiel CharacterEntity hinzugefügt
- Erstellen der Animation-Klasse
- Die erste Animation hinzufügen CharacterEntity
- Das Zeichen Bewegung hinzugefügt
- Übereinstimmende Verschiebung und animation


## <a name="unzipping-our-game-content"></a>Entzippen unsere Spiel Inhalte

Bevor wir mit dem Schreiben von Code beginnen, möchten wir unsere Spiel unzip *Inhalt*. Entwickler verwenden oft den Begriff *Inhalt* um auf Dateien ohne Code zu verweisen, die in der Regel durch visual Künstler, game-Designer oder audio-Designer erstellt werden. Allgemeine Typen von Inhalten Includedateien, die zum Anzeigen von visuellen Elementen, sound wiedergeben oder KI (AI) Verhalten steuern. Teams Perspektive Inhalt wird durch nicht-Programmierer in der Regel aus einer 3D-Spielentwicklung erstellt.

Der Inhalt, der hier verwendete verwendbaren [auf Github](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Wir benötigen diese Dateien an einen Speicherort, den wir später in dieser exemplarischen Vorgehensweise zugreift heruntergeladen.

## <a name="monogame-class-overview"></a>Übersicht über die MonoGame-Klasse

Zunächst wird die MonoGame-Klassen, die grundlegende Rendering verwendet vorgestellt:

- `SpriteBatch` – 2D Grafiken auf dem Bildschirm gezeichnet. *Sprites* 2D visuelle Elemente, die verwendet werden, Bilder auf dem Bildschirm angezeigt werden. Die `SpriteBatch` -Objekt kann ein einzelnes Sprite zu einem Zeitpunkt zwischen zeichnen seiner `Begin` und `End` Methoden oder mehrere Sprites können gruppiert werden, oder *im Batchmodus*.
- `Texture2D` – ein Image-Objekt zur Laufzeit darstellt. `Texture2D` Instanzen werden häufig von Dateiformaten wie PNG- oder BMP, erstellt, obwohl sie auch dynamisch zur Laufzeit erstellt werden können. `Texture2D` Instanzen werden verwendet, beim rendering mit `SpriteBatch` Instanzen.
- `Vector2` – Stellt eine Position in einem 2D Koordinatensystem das häufig verwendet wird, Positionieren visueller Objekte dar. Ereignissteuerung MonoGame `Vector3` und `Vector4` jedoch werden nur verwendet, `Vector2` in dieser exemplarischen Vorgehensweise.
- `Rectangle` – ein vier Spitzen Bereich mit der Position, Breite und Höhe. Wir müssen verwenden dies auf definieren, welcher Teil unserer `Texture2D` rendern, sofern wir mit Animationen arbeiten.

Es sollte auch Beachten Sie, dass MonoGame nicht von Microsoft – ungeachtet dessen Namespace verwaltet wird. Die `Microsoft.Xna` Namespace werden in MonoGame zum Migrieren von vorhandener XNA-Projekten zu MonoGame erleichtern.

## <a name="rendering-our-first-sprite"></a>Rendern von unseren ersten Sprite

Als Nächstes zeichnen wir ein einzelnes Sprite auf dem Bildschirm zu veranschaulichen, wie 2D Rendering in MonoGame durchzuführen.

### <a name="creating-a-texture2d"></a>Erstellen eine Texture2D

Wir erstellen müssen eine `Texture2D` Instanz zur Verwendung beim Rendern von unseren Sprite. Alle Spiel Inhalt befindet sich letztlich in einem Ordner namens **Inhalte** in das plattformspezifische Projekt befindet. MonoGame freigegebene Projekte darf keine Inhalte enthalten, wie der Inhalt auf Buildvorgänge, die spezifisch für die Plattform verwenden muss. CocosSharp Entwickler findet dem Inhaltsordner vertraut: sie befinden sich am gleichen Ort in CocosSharp und MonoGame-Projekten. Der Ordner kann im iOS-Projekt, und der Ordner "Assets" in der Android-Projekt gefunden werden.

Zum Hinzufügen des Spiels Inhalt mit der Maustaste auf die **Inhalt** Ordner, und wählen **hinzufügen > Hinzufügen von Dateien...** Navigieren Sie zu dem Speicherort, in dem die Datei content.zip extrahiert wurde, und wählen Sie die **charactersheet.png** Datei. Wenn Informationen zum Hinzufügen der das in Ordner aufgefordert werden, wählen wir die **Kopie** Option:

![](part2-images/image1.png "Wählen Sie bei Aufforderung zur Vorgehensweise beim Hinzufügen der das in Ordner die kopieren-option")

Die Ordners "Content" enthält jetzt die charactersheet.png-Datei:

![](part2-images/image2.png "Die Ordners "Content" enthält jetzt die charactersheet.png-Datei")

Als Nächstes fügen wir Code zum Laden Sie die Datei charactersheet.png und erstellen Sie eine `Texture2D`. Möchten Sie diese öffnen die `Game1.cs` Datei, und fügen Sie das folgende Feld den Game1.cs Klasse hinzu:

```csharp
Texture2D characterSheetTexture;
```

Als Nächstes erstellen wir die `characterSheetTexture` in die `LoadContent` Methode. Bevor Sie Änderungen `LoadContent` -Methode sieht folgendermaßen aus:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Sollten wir darauf hinweisen, dass das Standardprojekt bereits instanziiert die `spriteBatch` Instanz für uns. Wir müssen werden mithilfe dieser später aber wir hinzufügen wird nicht keinen zusätzlichen Code zum Vorbereiten der `spriteBatch` für die Verwendung. Andererseits, unsere `spriteSheetTexture` Initialisierung ist erforderlich, damit wir ihn nach dem Initialisieren wird der `spriteBatch` wird erstellt:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Nun mit dem eine `SpriteBatch` Instanz und ein `Texture2D` Instanz können wir unsere Renderingcodes zum Hinzufügen der `Game1.Draw` Methode:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Das Spiel gerade ausgeführten zeigt ein einzelnes Sprite die Textur erstellt aus charactersheet.png anzeigen:

![](part2-images/image3.png "Das Spiel gerade ausgeführten zeigt ein einzelnes Sprite die Textur erstellt aus charactersheet.png anzeigen")

## <a name="creating-the-characterentity"></a>Erstellen die CharacterEntity

Bisher haben wir den Code aus, um ein einzelnes Sprite auf dem Bildschirm gerendert hinzugefügt; würden wir eine vollständige Spiel zu entwickeln, ohne jede andere Klasse erstellen, würden wir jedoch Organisation Codeprobleme auftreten.

### <a name="what-is-an-entity"></a>Was ist eine Entität?

Ein allgemeines Muster für die Strukturierung der spielcode wird zum Erstellen einer neuen Klasse für jedes Spiel *Entität* Typ. Eine Entität in 3D-Spielentwicklung ist ein Objekt, das folgenden Merkmale enthalten kann (nicht alle sind erforderlich):

- Ein visuelles Element, z. B. eine Sprite, Text oder 3D-Modell
- Physikalische oder jeder Frame Verhalten wie eine Einheit patrolling einen Satz Pfad oder ein Player Zeichen "aktiv" damit Eingabe
- Erstellt und zerstört dynamisch, z. B. ein Hochfahren angezeigt werden und das vom Player erfasst werden können

Entität Organisation Systeme können komplex sein, und viele Spiel Module bieten Klassen zur einfachen Verwaltung von Entitäten. Wir müssen eine sehr einfache Entität System implementiert werden, ist Folgendes zu beachten, dass vollständige Spiele in der Regel weitere Organisation auf der Hand des Entwicklers Teil erfordern.


### <a name="defining-the-characterentity"></a>Definieren der CharacterEntity

Unsere Entität, das wir nenne `CharacterEntity`, müssen die folgenden Eigenschaften:

- Die Fähigkeit, einen eigenen laden `Texture2D`
- Die Möglichkeit, sich selbst darzustellen, einschließlich mit Aufrufen von Methoden zum Aktualisieren der Stackwalk animation
- `X` und Y-Eigenschaften zum Steuern der Position des Zeichens.
- Die Fähigkeit, selbst – insbesondere aktualisieren, lesen Sie Werte aus einem Bildschirm und die Position entsprechend anzupassen.

Hinzufügen der `CharacterEntity` unsere Spiel, mit der rechten Maustaste oder Steuerelement klicken Sie auf die **WalkingGame** Projekt, und wählen Sie **hinzufügen > neue Datei...** . Wählen Sie die **leere Klasse** aus, und nennen Sie die neue Datei **CharacterEntity**, klicken Sie dann auf **neu**.

Zunächst fügen wir die Möglichkeit für die `CharacterEntity` zum Laden einer `Texture2D` sowie um sich selbst zu zeichnen. Wir ändern die neu hinzugefügten `CharacterEntity.cs` Datei wie folgt:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Der obige Code fügt die Verantwortung des Positionierung, rendern und Laden von Inhalt an die `CharacterEntity`. Werfen Sie einen Moment Zeit, um die Stelle sei darauf hingewiesen einige Aspekte, die im obigen Code ausgeführt.

### <a name="why-are-x-and-y-floats"></a>Warum sind von X und Y erscheinen?

Entwickler, die noch nicht mit der Programmierung von Spielen sich vielleicht, warum die `float` Typs statt verwendet wird `int` oder `double`. Der Grund ist, dass ein 32-Bit-Wert wird am häufigsten für die Positionierung im Code Rendern auf unterster Ebene. Der Typ "double" belegt 64 Bits für die Genauigkeit, die nur selten erforderlich sind, für die Positionierung von Objekten. Während ein 32-Bit-Unterschied bedeutungslose erscheinen, enthalten viele moderne Spiele Grafiken, die Bearbeitung Zehntausenden von Positionswerte erfordern alle Frames (30 oder 60 Mal pro Sekunde). Schneiden der Größe des Arbeitsspeichers, die verschieben muss über die Graphics Pipeline, um die Hälfte einen entscheidenden Einfluss auf die Leistung eines Spiels haben.

Die `int` -Datentyp kann einer entsprechenden Maßeinheit für die Positionierung, sondern sie können Einschränkungen platzieren, auf dem Weg Entitäten positioniert werden. Z. B. mithilfe von ganzzahligen Werten macht es schwieriger smooth Bewegung für Spiele implementieren, die vergrößern oder 3D Kameras unterstützt (die zulässig sind, indem Sie `SpriteBatch`).

Schließlich sehen wir, dass die Logik, die unsere Zeichen auf dem Bildschirm bewegt müssen daher das Spiel zeitliche Steuerungswerte verwenden wird. Das Ergebnis der Multiplikation Geschwindigkeit durch, wie viel Zeit, in einem bestimmten Frame verstrichen ist führt nur selten in eine ganze Zahl, daher muss verwenden `float` für `X` und `Y`.

### <a name="why-is-charactersheettexture-static"></a>Warum ist der CharacterSheetTexture statische?

Die `characterSheetTexture` `Texture2D` Instanz ist eine laufzeitdarstellung des charactersheet.png-Datei. Das heißt, sie Farbwerte für jedes Pixels enthält, wie aus der Quelle extrahiert **charactersheet.png** Datei. Wenn unsere Spiel erstellen zwei `CharacterEntity` -Instanzen bestehen, jeweils von charactersheet.png Zugriff auf Informationen erforderlich wäre. In diesem Fall möchten wir wäre nicht charactersheet.png zweimal laden die Leistungseinbußen verursachen, noch möchten wir doppelte Textur Speicher im Arbeitsspeicher gespeichert haben. Mit einem `static` Feld erlaubt es uns, die denselben `Texture2D` auf allen `CharacterEntity` Instanzen.

Mithilfe von `static` Elemente für die Common Language Runtime-Darstellung des Inhalts ist eine vereinfachte Lösung, und es befasst sich Probleme bei größeren spielen, z. B. entladen Inhalt beim Verschieben von einer Ebene auf eine andere nicht. Ausgereiftere Projektmappen, die Gegenstand dieser exemplarischen Vorgehensweise sind, enthalten die MonoGames Content-Pipeline verwenden, oder erstellen eine benutzerdefinierte Inhaltsverwaltungssystem.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Warum SpriteBatch übergeben als ein Argument Instead Of instanziiert, indem die Entität ist?

Die `Draw` Methode wie oben nimmt implementiert eine `SpriteBatch` Argument, aber im Gegensatz dazu, die `characterSheetTexture` instanziiert, indem die `CharacterEntity`.

Der Grund dafür ist, da die effizienteste Rendering möglich beim ist identisch `SpriteBatch` Instanz dient für alle `Draw` aufruft, und wann alle `Draw` wird Aufrufe zwischen einem einzelnen Satz von `Begin` und `End` aufrufen. Natürlich unsere Spiel enthält nur eine einzelne Entitätsinstanz, aber etwas komplizierteren Spiele profitieren von der Muster, die mehrere Entitäten, die gleiche ermöglicht `SpriteBatch` Instanz.


## <a name="adding-characterentity-to-the-game"></a>Das Spiel CharacterEntity hinzugefügt

Nun, dass es hinzugefügt haben unsere `CharacterEntity` mit Code zum Rendern von selbst, können wir ersetzen Sie den Code in `Game1.cs` eine Instanz dieser neuen Entität verwenden. Zu diesem Zweck wird ersetzt die `Texture2D` -Feld durch eine `CharacterEntity` Feld `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Als Nächstes fügen wir die Instanziierung dieser Entität im `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Müssen ebenfalls Entfernen der `Texture2D` Erstellung von `LoadContent` seit, jetzt innerhalb eines behandelt wird unsere `CharacterEntity`. `Game1.LoadContent` sollte wie folgt aussehen:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Es ist zu beachten, dass trotz seines Namens der `LoadContent` Methode ist nicht der einzige Ort, auf dem Inhalt geladen werden kann – viele Spiele implementieren Content laden, in der Initialize-Methode oder sogar in Codeprobleme Update als Inhalt ist möglicherweise nicht erforderlich, bis der Spieler erreicht, eine bestimmten Punkt des Spiels.

Schließlich können wir die Draw-Methode wie folgt ändern:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Wenn wir das Spiel ausführen, sehen wir jetzt das Zeichen. Da 0 X und Y standardmäßig verwenden, wird das Zeichen anhand der oberen linken Ecke des Bildschirms positioniert:

![](part2-images/image4.png "Da 0 X und Y standardmäßig, klicken Sie dann das Zeichen anhand der oberen linken Ecke des Bildschirms befindet")

## <a name="creating-the-animation-class"></a>Erstellen der Animationsklasse

Derzeit unsere `CharacterEntity` zeigt die vollständige **charactersheet.png** Datei. Diese Anordnung von mehrere Bilder in eine Datei wird als bezeichnet eine *Sprite Blatt*. In der Regel wird ein Sprite nur einen Teil des Blatts Sprite gerendert. Wir ändern die `CharacterEntity` zum Rendern eines Teils davon **charactersheet.png**, und dieses Bereichs ändert sich im Zeitverlauf an eine Stackwalk Animation angezeigt.

Erstellen Sie die `Animation` Klasse, um die Logik und die Status der Animation CharacterEntity steuern. Animation-Klasse wird eine allgemeine Klasse, die für alle Entitäten verwenden, nicht nur verwendet werden können `CharacterEntity` Animationen. Ultimate der `Animation` Klasse gebe eine `Rectangle` die der `CharacterEntity` verwenden, wenn Sie selbst zeichnen. Wir erstellen außerdem eine `AnimationFrame` Klasse, die verwendet werden soll, auf die Animation definieren.


### <a name="defining-animationframe"></a>Definieren von AnimationFrame

`AnimationFrame` enthält keine Logik, die im Zusammenhang mit der Animation nicht. Wir müssen es verwenden werden nur zum Speichern von Daten. Hinzufügen der `AnimationFrame` -Klasse, mit der rechten Maustaste oder Steuerelement klicken Sie auf die **WalkingGame** freigegebenen Projekt und wählen Sie **hinzufügen > neue Datei...** Geben Sie den Namen **AnimationFrame** , und klicken Sie auf die **neu** Schaltfläche. Ändern wir das `AnimationFrame.cs` Datei, sodass sie den folgenden Code enthält:


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

Die `AnimationFrame` Klasse enthält zwei Angaben:

- `SourceRectangle` – Definiert den Bereich von der `Texture2D` der angezeigt, auf die `AnimationFrame`. Dieser Wert wird in Pixel, wobei der oberen linken (0, 0) gemessen.
- `Duration` – Definiert, wie lange ein `AnimationFrame` wird angezeigt, bei der Verwendung in einer `Animation`.

### <a name="defining-animation"></a>Definieren von Animationen

Die `Animation` Klasse enthält eine `List<AnimationFrame>` sowie die Logik zum wechseln, welche Frames derzeit angezeigt wird, entsprechend, wie viel Zeit verstrichen ist.

Hinzufügen der `Animation` -Klasse, mit der rechten Maustaste oder Steuerelement klicken Sie auf die **WalkingGame** freigegebenen Projekt und wählen Sie **hinzufügen > neue Datei...** . Geben Sie den Namen **Animation** , und klicken Sie auf die **neu** Schaltfläche. Ändern wir das `Animation.cs` Datei, damit sie den folgenden Code enthält:


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Sehen wir uns einige Details über die `Animation` Klasse.

### <a name="frames-list"></a>Frames Liste

Die `frames` angehört wie die Daten für unsere Animation speichert. Der Code, der die Animationen instanziiert wird hinzugefügt `AnimationFrame` auf Instanzen der `frames` durch Auflisten der `AddFrame` Methode. Eine vollständige Implementierung bieten möglicherweise `public` Methoden und Eigenschaften für das Ändern von `frames`, aber wir müssen die entsprechende Funktionalität zum Hinzufügen von Rahmen in dieser exemplarischen Vorgehensweise zu beschränken.

### <a name="duration"></a>Dauer

Dauer gibt die Gesamtdauer der `Animation,` erhalten Sie durch die Dauer aller enthaltenen hinzufügen `AnimationFrame` Instanzen. Dieser Wert kann zwischengespeichert werden, wenn `AnimationFrame` ein unveränderliches Objekt wurden, aber da wir AnimationFrame als eine Klasse, die geändert werden können implementiert, nach der Animation hinzugefügt wird, müssen wir diesen Wert zu berechnen, wenn die Eigenschaft zugegriffen wird.

### <a name="update"></a>Update

Die `Update` Methode sollte jeder Frame (d. h. jedes Mal aktualisiert das gesamte Spiel) aufgerufen werden. Dieses dient zum Erhöhen der `timeIntoAnimation` Member, die verwendet wird, um die aktuell angezeigte Frame zurückzugeben. Die Logik in `Update` wird verhindert, dass die `timeIntoAnimation` jemals nicht größer als die Dauer der gesamten Animation.

Da wir verwenden die `Animation` Klasse, um ein Stackwalk Animation anzuzeigen, und klicken Sie dann möchten wir haben diese Animation wiederholen, wenn sie das Ende erreicht hat. Hierfür können wir überprüfen, ob die `timeIntoAnimation` ist größer als die `Duration` Wert. Wenn dies der Fall ist sie zurück zum Anfang der Reihe nach und den Rest in einer Schleife Animation beibehalten.

### <a name="obtaining-the-current-frames-rectangle"></a>Abrufen des aktuellen Frames Rechteck

Der Zweck der `Animation` Klasse ist es letztlich bereitstellen eine `Rectangle` beim Zeichnen eines Sprites verwendet werden kann. Derzeit die `Animation` Klasse enthält die Logik zum Ändern der `timeIntoAnimation` Element, das zum Abrufen der verwendet wird. `Rectangle` angezeigt werden soll. Hierzu fügen wir erstellen eine `CurrentRectangle` Eigenschaft in der `Animation` Klasse. Kopieren Sie diese Eigenschaft in der `Animation` Klasse:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>Die erste Animation hinzufügen CharacterEntity

Die `CharacterEntity` enthält Animationen für durchlaufen und stehende sowie einen Verweis auf die aktuelle `Animation` angezeigt wird.

Zunächst fügen wir unser `Animation,` das verwendet wird. um die Funktionalität zu testen, wie bisher geschrieben. Fügen Sie die folgenden Member der Klasse CharacterEntity:

```csharp
Animation walkDown;
Animation currentAnimation;
```

Als Nächstes definieren Sie die `walkDown` Animation. Führen Sie dies ändern die `CharacterEntity` Konstruktor wie folgt:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Wie bereits erwähnt, müssen wir Aufrufen `Animation.Update` für zeitbasierte Animationen zum Abspielen. Wir müssen auch Zuweisen der `currentAnimation`. Für die wir jetzt zuweise der `currentAnimation` zu `walkDown`, aber wir müssen ersetzenden dieser Code später, wenn wir unsere Bewegung Logik implementieren. Wir fügen die `Update` Methode, um `CharacterEntity` wie folgt:


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Da nun die `currentAnimation` wird zugewiesen, und aktualisiert, können wir es verwenden, um unsere Zeichnung zu erstellen. Ändern wir `CharacterEntity.Draw` wie folgt:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Der letzte Schritt zum Abrufen der `CharacterEntity` animieren ist, das die neu hinzugefügte Aufrufen `Update` Methode `Game1`. Ändern Sie `Game1.Update` wie folgt:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Jetzt die `CharacterEntity` wiedergeben wird seine `walkDown` Animation:

![](part2-images/image5.gif "Die CharacterEntity wird jetzt die Animation WalkDown wiedergegeben.")

## <a name="adding-movement-to-the-character"></a>Das Zeichen Bewegung hinzugefügt

Als Nächstes werden wir Verschiebung in unserer Zeichen mit Touch-Steuerelemente hinzufügen. Wenn der Benutzer den Bildschirm berührt, wird das Zeichen für den Punkt verschoben, in dem der Bildschirm verwendet wird. Wenn keine Fingereingaben erkannt werden, wird das Zeichen an Stelle stehen.

### <a name="defining-getdesiredvelocityfrominput"></a>Definieren von GetDesiredVelocityFromInput

Wir müssen mithilfe des MonoGame `TouchPanel` -Klasse, die Informationen zu den aktuellen Status der Touchscreen bereitstellt. Fügen Sie eine Methode, die überprüft die `TouchPanel` und gewünschte Geschwindigkeit des Zeichens zurück:


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

Die `TouchPanel.GetState` Methode gibt ein `TouchCollection` enthält Informationen zu, in dem der Benutzer den Bildschirm berührt. Wenn der Benutzer nicht den Bildschirm berührt und dann die `TouchCollection` wird kann leer sein, es sollte nicht in diesem Fall das Zeichen verschieben.

Wenn der Benutzer den Bildschirm berührt, werden wir das Zeichen für die erste Fingereingabe, also verschoben, die `TouchLocation` am Index 0. Legen Sie zunächst die gewünschte Geschwindigkeit gleich der Differenz zwischen das Zeichen als auch die erste Fingereingabe Speicherort:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Im folgenden ist ein wenig mathematische Funktionen, die das Zeichen, das Verschieben der gleichen Geschwindigkeit beibehalten werden sollen. Um zu erläutern, warum dies wichtig ist, betrachten wir eine Situation, in denen der Benutzer die 500 Pixel betrifft auf dem sich das Zeichen befindet. Die erste Zeile Where `desiredVelocity.X` ist würde den Wert 500 zuzuweisen. Jedoch, wenn der Benutzer den Bildschirm auf eine Distanz von nur 100 Einheiten aus dem Zeichen berühren wurden die `desiredVelocity.X `auf 100 festgelegt. Das Ergebnis wäre, dass das Zeichen Bewegung Geschwindigkeit, wie weit reagieren soll, dass der Touch-Punkt mit dem Zeichen ist. Da wir das Zeichen immer mit der gleichen Geschwindigkeit verschieben möchten, müssen wir die DesiredVelocity ändern.

Die `if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` Anweisung überprüft, wenn die Geschwindigkeit nicht-0 (null) – anders ausgedrückt ist, um sicherzustellen, dass der Benutzer nicht die gleiche Stelle als die aktuelle Zeichenposition berührt überprüfen. Wenn nicht, dann wir das Zeichen Geschwindigkeit als konstant, unabhängig davon, wie weit die Fingereingabe festgelegt müssen ist. Hierfür müssen wir die Folge sein, dass eine Länge von 1 es normalverteilt normalisieren. Eine normalverteilt 1 bedeutet, dass das Zeichen bei 1 Pixel pro Sekunde verschoben werden. Wir müssen dies beschleunigen, indem der Wert mit der gewünschten Geschwindigkeit von 200 multipliziert.


### <a name="applying-velocity-to-position"></a>Anwenden von Geschwindigkeit Position

Die Geschwindigkeit von zurückgegebenen `GetDesiredVelocityFromInput` muss auf des Zeichens angewendet werden `X` und `Y` Werte wirkt sich nur zur Laufzeit. Ändern wir das `Update` Methode wie folgt:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Wir hier implementiert haben heißt *zeitbasierte* Bewegung (im Gegensatz zu *framebasierte* Bewegung). Zeitbasierte Bewegung multipliziert den Wert einer Geschwindigkeit (in unserem Fall die Werte, in gespeichert der `velocity` Variable) durch, wie viel Zeit seit dem letzten Update verstrichen ist, die in gespeichert ist `gameTime.ElapsedGameTime.TotalSeconds`. Wenn das Spiel an weniger Frames pro Sekunde ausgeführt wird, steigt die verstrichene Zeit zwischen Frames – das Endergebnis ist, dass Objekte, die mit zeitbasierten Bewegung immer mit der gleichen Geschwindigkeit unabhängig von der Framerate verschoben werden.

Wenn wir unsere Spiel jetzt ausführen, sehen wir, dass das Zeichen für die Fingereingabe Speicherort verschoben werden:

![](part2-images/image6.gif "Das Zeichen wird in Richtung der Touch-Speicherort verschieben.")

## <a name="matching-movement-and-animation"></a>Übereinstimmende Verschiebung und Animation

Sobald wir unsere Zeichen verschieben und beim Spielen von einer einzelnen Animation haben, können wir die restliche unsere Animationen definieren und anschließend verwenden, um die Verschiebung des Zeichens widerzuspiegeln. Wenn abgeschlossen haben wir acht Animationen insgesamt:

- Animationen am, unten, links und rechts
- Animationen für stehende noch und nach oben, unten, links und rechts

### <a name="defining-the-rest-of-the-animations"></a>Definieren die Rest-Animationen

Zunächst fügen wir der `Animation` um Instanzen der `CharacterEntity` Klasse für alle unsere Animationen am gleichen Ort, an dem wir hinzugefügt `walkDown`. Sobald wir Sie dazu die `CharacterEntity` müssen die folgenden `Animation` Elemente:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Jetzt wir die Animationen in definieren werde der `CharacterEntity` Konstruktor wie folgt:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Es sollten beachten, dass der obige Code hinzugefügt wurde die `CharacterEntity` Konstruktor, um diese exemplarische Vorgehensweise kürzere bleiben. Spiele werden in der Regel die Definition der Zeichen Animationen in ihren eigenen Klassen zu trennen oder laden diese Informationen aus ein Format wie z. B. XML oder JSON.

Als Nächstes müssen wir passen Sie die Logik die Animationen entsprechend die Richtung, die das Zeichen verschoben wird oder nach der letzten Animation verwendet wird, wenn das Zeichen einfach beendet wurde. Zu diesem Zweck ändern wir die `Update` Methode:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Der Code zum Wechseln von Animationen ist in zwei Blöcke aufgeteilt. Die erste überprüft, ob die Geschwindigkeit nicht gleich `Vector2.Zero` – Dies gibt an, ob das Zeichen verschoben werden. Wenn das Zeichen bewegt wird, können wir sehen Sie sich die `velocity.X` und `velocity.Y` Werte, um zu bestimmen, welche Stackwalk Animation wiedergegeben.

Wenn das Zeichen wird nicht verschoben, und klicken Sie dann des Zeichens festgelegt werden soll `currentAnimation` auf eine ständige Animation –, aber wir nur tun, wenn die `currentAnimation` ist eine durchlaufen Animation oder wenn eine Animation nicht festgelegt wurde. Wenn die `currentAnimation` ist keiner der vier Animationen durchlaufen, und klicken Sie dann das Zeichen bereits ständigen ist, daher brauchen wir ändern `currentAnimation`.

Das Ergebnis dieses Codes ist, dass die Zeichen ordnungsgemäß auf die Animation beim durchlaufen werden soll, und, klicken Sie dann die letzten Richtung, die sie durchlaufen wurde stoßen, wenn er beendet wird:

![](part2-images/image7.gif "Das Ergebnis dieses Codes ist, dass die Zeichen ordnungsgemäß auf die Animation beim durchlaufen werden soll, und, klicken Sie dann die letzten Richtung, die sie durchlaufen wurde stoßen, wenn er beendet wird")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie mit MonoGame zum Erstellen einer plattformübergreifenden Spiels mit Sprites, Verschieben von Objekten, Erkennung von Eingabe- und Animation. Es umfasst das Erstellen einer allgemeinen Animationsklasse. Es wurde auch gezeigt, wie eine Zeichenentität für die Strukturierung der Codelogik zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [CharacterSheet Bildressource (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Walking Spiel abgeschlossen (Beispiel)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
