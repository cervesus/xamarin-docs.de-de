---
title: Teil 2 – Implementieren von WalkingGame
description: In dieser exemplarischen Vorgehensweise veranschaulicht fügen Sie die Spiellogik und Inhalte, um ein leeres MonoGame-Projekt zum Erstellen einer animierten Sprite verschoben wird, wobei einer Demo Fingereingaben.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 941b88f9109cf2f3a3485311c52b1250bd08e53f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61162169"
---
# <a name="part-2--implementing-the-walkinggame"></a>Teil 2 – Implementieren von WalkingGame

_In dieser exemplarischen Vorgehensweise veranschaulicht fügen Sie die Spiellogik und Inhalte, um ein leeres MonoGame-Projekt zum Erstellen einer animierten Sprite verschoben wird, wobei einer Demo Fingereingaben._

Die vorherigen Teilen der in dieser exemplarischen Vorgehensweise wurde gezeigt, wie leere MonoGame-Projekte zu erstellen. Wir werden auf diese Teile des vorherigen erstellen Sie dazu ein einfaches Spiels Demo. Dieser Artikel enthält folgende Abschnitte:

- Entzippen unsere Spiele Inhalte
- Übersicht über MonoGame-Klasse
- Unsere erste Sprite rendern
- Erstellen die CharacterEntity
- Das Spiel CharacterEntity hinzugefügt
- Erstellen der Animation-Klasse
- Hinzufügen von der ersten Animation zu CharacterEntity
- Hinzufügen von Bewegung auf das Zeichen
- Übereinstimmende verschieben und animation


## <a name="unzipping-our-game-content"></a>Entzippen unsere Spiele Inhalte

Bevor wir das Schreiben von Code beginnen, sollten wir unser Spiel Entzippen *Inhalt*. Spieleentwickler verwenden oft den Begriff *Inhalt* um auf nicht-Codedateien zu verweisen, die in der Regel vom visual Künstler, game-Designer oder audio-Designer erstellt werden. Allgemeine Arten von Inhalten Includedateien, die zum Anzeigen von visuellen Elementen, sound wiedergeben oder künstliche Intelligenz (KI) Verhalten zu steuern. Von einem Spiel wird Teams Perspektive Inhalt in der Regel von nicht-Programmierern erstellt.

Finden Sie der Inhalt, der hier verwendete [auf Github](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Wir benötigen diese Dateien an einen Speicherort an, dem wir später in dieser exemplarischen Vorgehensweise zugreifen wird heruntergeladen.

## <a name="monogame-class-overview"></a>Übersicht über MonoGame-Klasse

Zunächst einmal wird die grundlegende Rendering verwendeten MonoGame-Klassen erläutert:

- `SpriteBatch` – verwendet, um die 2D-Grafiken auf dem Bildschirm gezeichnet. *Sprites* 2D visuelle Elemente, die verwendet werden, um Bilder auf dem Bildschirm angezeigt werden. Die `SpriteBatch` -Objekt kann ein einzelnes Sprite zeichnen, zu einem Zeitpunkt zwischen der `Begin` und `End` Methoden oder mehrere Sprites können gruppiert werden, oder *im Batchmodus*.
- `Texture2D` – ein Image-Objekt zur Laufzeit darstellt. `Texture2D` Instanzen werden häufig von Dateiformaten wie z. B. PNG oder BMP, erstellt, obwohl sie auch dynamisch zur Laufzeit erstellt werden können. `Texture2D` Instanzen werden verwendet, wenn das rendering mit `SpriteBatch` Instanzen.
- `Vector2` – Stellt eine Position in einem 2D-Koordinatensystem, die häufig verwendet wird, für die Positionierung von visuellen Objekten dar. MonoGame enthält auch `Vector3` und `Vector4` wird aber nur `Vector2` in dieser exemplarischen Vorgehensweise.
- `Rectangle` – einem Bereich vier Seiten mit der Position, Breite und Höhe. Wir verwenden diese zum definieren, welcher Teils unserer `Texture2D` rendern, sofern wir mit Animationen arbeiten.

Wir sollten außerdem Beachten Sie, dass MonoGame nicht von Microsoft –, ungeachtet dessen Namespace verwaltet wird. Die `Microsoft.Xna` -Namespace werden in MonoGame zum Migrieren von vorhandener XNA-Projekten monogame erleichtern.

## <a name="rendering-our-first-sprite"></a>Unsere erste Sprite rendern

Als Nächstes zeichnen wir eine einzelne Sprites auf dem Bildschirm, um zu zeigen, wie 2D-Rendering in MonoGame ausführen können.

### <a name="creating-a-texture2d"></a>Erstellen ein Texture2D

Müssen wir erstellen eine `Texture2D` Instanz zur Verwendung beim Rendern von unserem Sprite. Alle Spiele Inhalt befindet sich letztlich in einem Ordner namens **Inhalt** im plattformspezifischen Projekt befindet. MonoGame freigegebenen Projekte können keinen Inhalt, enthalten, wie der Inhalt auf Buildvorgänge, die spezifisch für die Plattform verwenden muss. CocosSharp-Entwickler findet den Ordner "Content" ein Konzept vertraut sind – sie befinden sich am selben Ort in CocosSharp und MonoGame Projekten. Der Ordner "Content" finden Sie im iOS-Projekt, und klicken Sie im Ordner "Assets" in der Android-Projekt.

Zum Hinzufügen des Spiels Inhalt mit der Maustaste auf die **Inhalt** Ordner, und wählen **hinzufügen > Dateien hinzufügen...** Navigieren Sie zum Speicherort, in denen die content.zip-Datei extrahiert wurde, und wählen Sie die **charactersheet.png** Datei. Wenn Sie gefragt werden, dazu, wie Sie die Datei zu Ordner hinzufügen, wählen wir die **Kopie** Option:

![](part2-images/image1.png "Wenn Sie gefragt werden, dazu, wie Sie die Datei zu Ordner hinzufügen, wählen Sie die Option zum Kopieren")

Der Ordner "Content" enthält nun die charactersheet.png-Datei:

![](part2-images/image2.png "Der Ordner \"Content\" enthält jetzt die charactersheet.png-Datei")

Als Nächstes fügen wir Code zum Laden Sie die Datei charactersheet.png und erstellen eine `Texture2D`. Öffnen Sie dafür die `Game1.cs` Datei, und fügen Sie das folgende Feld den Game1.cs Klasse hinzu:

```csharp
Texture2D characterSheetTexture;
```

Als Nächstes erstellen wir die `characterSheetTexture` in die `LoadContent` Methode. Bevor Sie Änderungen `LoadContent` Methode sieht wie folgt aus:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Sollten wir darauf hinweisen, dass das Standardprojekt bereits instanziiert die `spriteBatch` Instanz für uns. Wir verwenden dies später noch Mal, aber wir hinzufügen wird nicht die zusätzlichen Code zum Vorbereiten der `spriteBatch` für die Verwendung. Andererseits, unsere `spriteSheetTexture` Initialisierung ist erforderlich, damit wir ihn nach dem Initialisieren wird der `spriteBatch` wird erstellt:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Nun, wir haben eine `SpriteBatch` Instanz und ein `Texture2D` Instanz können wir unser Renderingcode zum Hinzufügen der `Game1.Draw` Methode:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Ausführen des Spiels jetzt zeigt ein einzelnes Sprite, das Anzeigen der Textur aus charactersheet.png erstellt:

![](part2-images/image3.png "Ausführen des Spiels jetzt wird ein einzelnes Sprite, das Anzeigen der Textur aus charactersheet.png erstellt")

## <a name="creating-the-characterentity"></a>Erstellen die CharacterEntity

Bisher haben wir Code zum Rendern eines einzelnen Sprites auf dem Bildschirm hinzugefügt; würden wir eine vollständige Spiel zu entwickeln, ohne andere Klassen bilden, würden wir jedoch Code Organisation Probleme auftreten.

### <a name="what-is-an-entity"></a>Was ist eine Entität?

Ein allgemeines Muster für die Organisation von spielcode ist, erstellen Sie eine neue Klasse für jedes Spiel *Entität* Typ. Eine Entität in der Entwicklung von Spielen, ist ein Objekt, das einige der folgenden Eigenschaften enthalten kann (nicht alle sind erforderlich):

- Ein visuelles Element, z. B. ein Sprite, Text oder 3D-Modell
- Physik oder jeder Frame-Verhalten, z. B. eine Einheit patrolling einen Set-Pfad oder ein Player-Zeichen, das Reagieren auf eine Eingabe
- Erstellt und zerstört dynamisch, z. B.-Power-Up angezeigt werden und vom Player erfasst werden können

Entität Organisation Systeme können komplex sein und viele Spiel-Engines bieten Klassen zum Verwalten von Entitäten. Wir werden ein sehr einfaches Entity-System, implementieren, sodass es zu beachten, dass vollständige Spiele in der Regel weitere Organisation auf des Entwicklers erforderlich ist.


### <a name="defining-the-characterentity"></a>Definieren der CharacterEntity

Unsere-Entität, die wir den Namen `CharacterEntity`, müssen Sie die folgenden Eigenschaften:

- Die Fähigkeit, eine eigene laden `Texture2D`
- Die Möglichkeit, selbst rendern, einschließlich mit Aufrufen von Methoden zum Aktualisieren der walkingstrecken animation
- `X` und Y-Eigenschaften zum Steuern der Position des Zeichens.
- Die Möglichkeit, sich selbst – insbesondere zu aktualisieren, um Werte aus der die Berührung Bildschirm, und passen Sie die Position entsprechend zu lesen.

Hinzufügen der `CharacterEntity` auf unser Spiel, mit der rechten Maustaste oder Strg + Klick auf die **WalkingGame** Projekt, und wählen **hinzufügen > neue Datei...** . Wählen Sie die **leere Klasse** aus, und nennen Sie die neue Datei **CharacterEntity**, klicken Sie dann auf **neu**.

Zunächst fügen wir die Möglichkeit, dass die `CharacterEntity` zum Laden einer `Texture2D` sowie um sich selbst zu zeichnen. Ändern wir die neu hinzugefügte `CharacterEntity.cs` -Datei wie folgt:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Der obige Code fügt die Verantwortung für die Positionierung, rendern und Laden von Inhalt an die `CharacterEntity`. Werfen Sie einen Moment Zeit, einige Überlegungen getroffen, die im obigen Code hinweisen.

### <a name="why-are-x-and-y-floats"></a>Warum werden von X und Y-Gleitkommazahlen?

Entwickler, die in der game-Programmierung sind vielleicht, warum die `float` Typ wird im Gegensatz zu `int` oder `double`. Der Grund ist, dass es sich bei 32-Bit-Wert wird am häufigsten für die Positionierung im Code auf niedriger Ebene rendern. Die double-Typ nimmt ein 64-Bit für die Genauigkeit, die nur selten benötigt wird, für die Positionierung von Objekten. Während ein 32-Bit-Unterschied unbedeutend erscheinen, enthalten viele moderne Spiele Grafiken, die Zehntausende von Positionswerte verarbeitet werden müssen und jeden Frame (30 oder 60-Mal pro Sekunde). Verkürzt der Menge an Arbeitsspeicher, die verschieben muss über die Grafik Pipeline, um die Hälfte haben erhebliche Auswirkungen auf die Leistung von des Spiels.

Die `int` -Datentyp kann eine entsprechende Maßeinheit für die Positionierung, aber sie können Einschränkungen auf die Möglichkeit, die Entitäten werden positioniert platzieren. Z. B. mithilfe von ganzzahligen Werten erschwert es gleichmäßige Drehbewegung für Spiele zu implementieren, die Unterstützung von vergrößern oder 3D Kameras (die zulässig sind, indem `SpriteBatch`).

Abschließend sehen wir, dass die Logik, die unsere Zeichen auf dem Bildschirm verschoben wird dies mithilfe des Spiels zeitliche Steuerungswerte werden auch tun. Das Ergebnis der Multiplikation der Geschwindigkeit mit wie viel Zeit, in einem bestimmten Frame verstrichen ist führt nur selten in eine ganze Zahl, daher wir verwenden müssen `float` für `X` und `Y`.

### <a name="why-is-charactersheettexture-static"></a>Warum CharacterSheetTexture ist statisch?

Die `characterSheetTexture` `Texture2D` Instanz ist eine laufzeitdarstellung der charactersheet.png-Datei. Das heißt, sie Farbwerte für jedes Pixel enthält, wie aus der Datenquelle extrahiert **charactersheet.png** Datei. Wenn unser Spiel erstellen zwei `CharacterEntity` Instanzen, und klicken Sie dann jeweils Zugriff auf Informationen von charactersheet.png benötigen würde. In diesem Fall möchten wir wäre nicht fallen die Leistungseinbußen durch charactersheet.png zweimal geladen und in unserem Beispiel sollen doppelte Texturspeicher im Arbeitsspeicher gespeichert haben. Mit einem `static` Feld ermöglicht uns, die denselben `Texture2D` in allen `CharacterEntity` Instanzen.

Mithilfe von `static` Elemente für die Common Language Runtime-Darstellung des Inhalts ist eine vereinfachte Lösung, und es behandelt nicht die Probleme, die in größeren Spiele wie z. B. entladen Inhalt beim Verschieben von einer Ebene auf einen anderen. Ausgereiftere Projektmappen, die über den Rahmen dieser exemplarischen Vorgehensweise hinaus sind, gehören MonoGames-inhaltspipelines verwenden, oder erstellen ein benutzerdefiniertes Content Managementsystem.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Warum SpriteBatch übergeben als ein Argument Instead Of instanziiert, indem die Entität ist?

Die `Draw` Methode wie oben verwendet implementiert eine `SpriteBatch` Argument, aber im Gegensatz dazu die `characterSheetTexture` instanziiert, indem die `CharacterEntity`.

Der Grund dafür ist, da das am effizientesten Rendern möglich Wenn ist gleich `SpriteBatch` Instanz wird verwendet, für alle `Draw` aufrufen, und wann alle `Draw` werden Aufrufe zwischen einem einzelnen Satz von `Begin` und `End` Aufrufe. Natürlich unser Spiel enthält nur eine einzelne Entitätsinstanz, aber etwas komplizierteren Spiele profitieren davon, Muster, die mehrere Entitäten in den gleichen ermöglicht `SpriteBatch` Instanz.


## <a name="adding-characterentity-to-the-game"></a>Das Spiel CharacterEntity hinzugefügt

Nun, wir hinzugefügt haben unsere `CharacterEntity` mit Code zum Rendern von sich selbst ersetzen wir den Code in `Game1.cs` eine Instanz dieser neuen Entität verwenden. Hierzu wir ersetzen die `Texture2D` Feld mit einem `CharacterEntity` Feld `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Als Nächstes fügen wir die Instanziierung dieser Entität in `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Wir müssen auch Entfernen der `Texture2D` erstellen `LoadContent` da, in der jetzt behandelt wird unsere `CharacterEntity`. `Game1.LoadContent` sollte wie folgt aussehen:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Es ist erwähnenswert, die trotz seines Namens der `LoadContent` Methode ist nicht der einzige Ort, in dem Inhalte geladen werden kann – viele Spiele implementieren, die Inhalt geladen wird, in deren Initialize-Methode oder sogar in ihren Update-Code als Inhalt möglicherweise nicht erforderlich, bis der Player erreicht, eine bestimmte Punkt des Spiels.

Schließlich können wir die Draw-Methode wie folgt ändern:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Wenn wir das Spiel ausführen, sehen wir jetzt das Zeichen. Da 0 X und Y standardmäßig verwenden, klicken Sie dann das Zeichen für den oberen linken Ecke des Bildschirms befindet sich:

![](part2-images/image4.png "Da 0 X und Y standardmäßig verwenden, klicken Sie dann das Zeichen für den oberen linken Ecke des Bildschirms befindet")

## <a name="creating-the-animation-class"></a>Erstellen der Animationsklasse

Derzeit unsere `CharacterEntity` zeigt die vollständige **charactersheet.png** Datei. Diese Anordnung von mehrere Abbilder in einer Datei wird als bezeichnet ein *Sprite-Sheet*. Ein Sprite wird in der Regel nur einen Teil des Sprite-Sheet gerendert. Ändern wir die `CharacterEntity` zum Rendern eines Teils davon **charactersheet.png**, und dieses Bereichs ändert sich im Laufe der Zeit, um eine Animation walkingstrecken anzuzeigen.

Wir erstellen den `Animation` Klasse, um die Logik und Zustand der CharacterEntity Animation zu steuern. Die Animation-Klasse wird eine allgemeine Klasse, die für jede Entität, nicht nur verwendet werden können `CharacterEntity` Animationen. Ultimate die `Animation` Klasse bereit eine `Rectangle` die der `CharacterEntity` verwenden, wenn sich selbst zu zeichnen. Wir erstellen auch eine `AnimationFrame` Klasse, die zum Definieren der Animation verwendet wird.


### <a name="defining-animationframe"></a>Definieren von AnimationFrame

`AnimationFrame` enthält keine Logik, die im Zusammenhang mit der Animation nicht. Wir werden ihn verwenden, nur zum Speichern von Daten. Hinzufügen der `AnimationFrame` -Klasse, mit der rechten Maustaste oder Strg + Klick auf die **WalkingGame** freigegebenen Projekt, und wählen **hinzufügen > neue Datei...** Geben Sie den Namen **AnimationFrame** , und klicken Sie auf die **neu** Schaltfläche. Ändern wir die `AnimationFrame.cs` Datei, sodass sie den folgenden Code enthält:


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

Die `AnimationFrame` -Klasse enthält zwei Angaben:

- `SourceRectangle` : Definiert den Bereich des der `Texture2D` die angezeigt, auf die `AnimationFrame`. Dieser Wert wird in Pixel, wobei der oberen linken (0, 0) gemessen.
- `Duration` : Definiert, wie lange ein `AnimationFrame` wird angezeigt, bei der Verwendung in einer `Animation`.

### <a name="defining-animation"></a>Definieren der Animation

Die `Animation` Klasse enthält eine `List<AnimationFrame>` sowie die Logik zum Wechseln der Frame gerade angezeigt wird, entsprechend, wie viel Zeit verstrichen ist.

Hinzufügen der `Animation` -Klasse, mit der rechten Maustaste oder Strg + Klick auf die **WalkingGame** freigegebenen Projekt, und wählen **hinzufügen > neue Datei...** . Geben Sie den Namen **Animation** , und klicken Sie auf die **neu** Schaltfläche. Ändern wir die `Animation.cs` Datei, sodass sie den folgenden Code enthält:


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Sehen wir uns einige Details über die `Animation` Klasse.

### <a name="frames-list"></a>Liste von Frames

Die `frames` angehört wie der Datenspeicher für unsere Animation. Fügen der Code mit dem die Animationen instanziiert `AnimationFrame` auf Instanzen der `frames` durch Auflisten der `AddFrame` Methode. Eine umfassendere Implementierung bieten möglicherweise `public` Methoden oder Eigenschaften zum Ändern von `frames`, aber beschränken wir uns die Funktionalität zum Hinzufügen von Frames in dieser exemplarischen Vorgehensweise.

### <a name="duration"></a>Dauer

Dauer gibt die gesamte Dauer des der `Animation,` erhalten Sie durch die Dauer aller enthaltenen hinzufügen `AnimationFrame` Instanzen. Dieser Wert zwischengespeichert werden kann, wenn `AnimationFrame` ein unveränderliches Objekt wurden, aber da wir AnimationFrame als eine Klasse die geändert werden können implementiert, nachdem die Animation hinzugefügt wird, müssen wir diesen Wert zu berechnen, wenn die Eigenschaft zugegriffen wird.

### <a name="update"></a>Update

Die `Update` -Methode jedes Einzelbild (d. h. jedes Mal, das ganze Spiel wird aktualisiert) aufgerufen werden soll. Dient zum Erhöhen der `timeIntoAnimation` Member, die verwendet wird, um den aktuell angezeigten Frame zurückzugeben. Die Logik in `Update` wird verhindert, dass die `timeIntoAnimation` jemals nicht größer als die Dauer der gesamten Animation.

Da wir verwenden die `Animation` Klasse, um eine Animation walkingstrecken anzuzeigen, und wir möchten diese Animation wiederholt werden, wenn sie das Ende erreicht hat. Wir können dies erreichen, indem Sie überprüfen, ob die `timeIntoAnimation` ist größer als die `Duration` Wert. Wenn dies der Fall ist es wieder von vorn durchlaufen und den Rest der, in einer Schleife Animation beibehalten.

### <a name="obtaining-the-current-frames-rectangle"></a>Abrufen des aktuellen Frames Rechteck

Der Zweck der `Animation` Klasse ist letztendlich zu einer `Rectangle` verwendet werden kann, wenn ein Sprite zeichnen. Derzeit die `Animation` Klasse enthält Logik zum Ändern der `timeIntoAnimation` Member, mit der die zum Abrufen der `Rectangle` angezeigt werden soll. Hierzu müssen wir erstellen eine `CurrentRectangle` -Eigenschaft in der `Animation` Klasse. Kopieren Sie diese Eigenschaft in der `Animation` Klasse:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime = new TimeSpan();
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>Hinzufügen von der ersten Animation zu CharacterEntity

Die `CharacterEntity` enthält Animationen für durchlaufen und ständigen sowie einen Verweis auf die aktuelle `Animation` angezeigt wird.

Zunächst fügen wir unserer ersten `Animation,` mit der die die Funktionalität testen, wie Sie bereits geschrieben. Fügen Sie die folgenden Member der Klasse CharacterEntity an:

```csharp
Animation walkDown;
Animation currentAnimation;
```

Als Nächstes definieren wir die `walkDown` Animation. Ändern Sie die `CharacterEntity` Konstruktor wie folgt:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Wie bereits erwähnt, müssen wir Aufrufen `Animation.Update` für zeitbasierten Animationen zum Abspielen. Wir müssen auch weisen die `currentAnimation`. Für jetzt wir zuweisen werde die `currentAnimation` zu `walkDown`, jedoch werden werden ersetzt diesen Code später beim Implementieren wir unsere Logik verschieben. Wir fügen die `Update` Methode, um `CharacterEntity` wie folgt:


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Nun, wir haben die `currentAnimation` wird zugewiesen, und aktualisiert, wir können sie um unsere Zeichnung zu erstellen. Ändern wir `CharacterEntity.Draw` wie folgt:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Der letzte Schritt zum Abrufen der `CharacterEntity` Animation besteht darin, die neu hinzugefügte aufzurufen `Update` aus `Game1`. Ändern Sie `Game1.Update` wie folgt:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Jetzt die `CharacterEntity` spielen die `walkDown` Animation:

![](part2-images/image5.gif "Die CharacterEntity wird jetzt die WalkDown Animation wiedergegeben.")

## <a name="adding-movement-to-the-character"></a>Hinzufügen von Bewegung auf das Zeichen

Als Nächstes werden wir Bewegung in unserer Zeichen mithilfe der Fingereingabe-Steuerelemente hinzufügen. Wenn der Benutzer den Bildschirm berührt, wird das Zeichen für den Zeitpunkt verschoben, in dem der Bildschirm berührt wird. Wenn keine Workflows erkannt werden, wird das Zeichen an Stelle stehen.

### <a name="defining-getdesiredvelocityfrominput"></a>Definieren von GetDesiredVelocityFromInput

Wir verwenden die MonoGame `TouchPanel` -Klasse, die Informationen zu den aktuellen Zustand des den Touchscreen bereitstellt. Fügen Sie eine Methode, die überprüft, die `TouchPanel` und gewünschten Geschwindigkeit des Zeichens zurück:


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

Die `TouchPanel.GetState` Methode gibt eine `TouchCollection` enthält Informationen, in denen der Benutzer den Bildschirm berührt. Wenn der Benutzer den Bildschirm nicht berührt die `TouchCollection` werden leer ist, wir sollten nicht in diesem Fall das Zeichen verschoben.

Wenn der Benutzer den Bildschirm berührt, werden wir das Zeichen für die erste Berührung, anders ausgedrückt: verschoben, die `TouchLocation` am Index 0. Zunächst legen Sie die gewünschte Geschwindigkeit auf den Unterschied zwischen des Zeichens Speicherort und die erste Fingereingabe-Speicherort:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Im folgenden finden etwas Mathematik, die das Verschieben von Zeichen mit derselben Geschwindigkeit erhalten bleiben. Um zu erklären, warum dies wichtig ist, sehen Sie sich eine Situation, in denen der Benutzer den Bildschirm 500 Pixel berührt von auf dem sich das Zeichen befindet. Die erste Zeile Where `desiredVelocity.X` wird den Wert 500 würden zuzuweisen. Jedoch, wenn der Benutzer den Bildschirm in einer Entfernung von nur 100 Einheiten aus dem Zeichen berühren wurden die `desiredVelocity.X `würde auf 100 festgelegt sein. Das Ergebnis wäre, dass die Bewegung Geschwindigkeit des Zeichens, wie weit entfernt reagieren würden, dass die Touch-Punkt vom Zeichen ist. Da wir das Zeichen, das immer mit derselben Geschwindigkeit verschieben möchten, müssen wir die DesiredVelocity zu ändern.

Die `if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` Anweisung überprüft, wenn die Geschwindigkeit nicht-0 (null) – mit anderen Worten ist, um sicherzustellen, dass der Benutzer nicht die gleichen Position wie die aktuelle Position des Zeichens berührt überprüfen. Wenn nicht der Fall, dann wir das Zeichen, die Geschwindigkeit konstant, unabhängig davon, wie weit entfernt sein. Legen Sie die Fingereingabe müssen ist. Dies erreichen wir durch Normalisieren des Geschwindigkeitsvektors, was davon eine Länge von 1 führt. Ein Vektor der Geschwindigkeit von 1 bedeutet, dass das Zeichen bei 1 Pixel pro Sekunde verschoben wird. Wir werden dies beschleunigen, indem der Wert multipliziert wird durch die gewünschte Geschwindigkeit von 200.


### <a name="applying-velocity-to-position"></a>Anwenden von Geschwindigkeit auf Position

Die Geschwindigkeit von zurückgegebenen `GetDesiredVelocityFromInput` muss auf des Zeichens, die angewendet werden `X` und `Y` Werte zur Laufzeit eine Auswirkung hat. Ändern wir die `Update` -Methode wie folgt:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Was wir hier implementiert haben, heißt *zeitbasierte* Bewegung (im Gegensatz zu *framebasierte* datenverschiebung). Verschieben eines zeitbasierten multipliziert einen Wert für die Geschwindigkeit (in unserem Fall die Werte, in gespeichert der `velocity` Variable) durch, wie viel Zeit seit der letzten Aktualisierung verstrichen ist, die in gespeichert ist `gameTime.ElapsedGameTime.TotalSeconds`. Wenn das Spiel mit weniger Bildern pro Sekunde ausgeführt wird, verlängert sich, die verstrichene Zeit zwischen den Frames – das Endergebnis ist, dass Objekte mit zeitbasierten Bewegung immer mit derselben Geschwindigkeit unabhängig von der Einzelbildrate verschoben werden.

Wenn wir jetzt unser Spiel ausführen, sehen wir, dass das Zeichen in Richtung Touch-Position verschoben werden:

![](part2-images/image6.gif "Das Zeichen wird in Richtung Touch-Position verschoben werden.")

## <a name="matching-movement-and-animation"></a>Übereinstimmende verschieben und Animation

Sobald wir unsere Zeichen verschieben und die Wiedergabe einer einzelnen Animation haben, können wir definieren die restlichen unsere Animationen und dann verwenden, um die Bewegung des Zeichens widerzuspiegeln. Wenn abgeschlossen haben wir acht Animationen insgesamt:

- Animationen für Schritt für Schritt nach oben, unten, links und rechts
- Animationen für die Bereitstellung weiterhin und nach oben, unten, links und rechts

### <a name="defining-the-rest-of-the-animations"></a>Definieren die Rest-Animationen

Wir fügen zunächst die `Animation` auf Instanzen der `CharacterEntity` -Klasse für alle unsere Animationen am selben Ort, an dem wir hinzugefügt `walkDown`. Sobald wir Sie dazu die `CharacterEntity` müssen die folgenden `Animation` Mitglieder:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Nachdem ich, die Animationen in definieren möchte der `CharacterEntity` Konstruktor wie folgt:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Sollten wir darauf hinweisen, dass der obige Code hinzugefügt wurde die `CharacterEntity` Konstruktor, um in dieser exemplarischen Vorgehensweise kürzer beibehalten. Spiele werden in der Regel die Definition von Animationen von Zeichen in ihre eigenen Klassen zu trennen oder laden diese Informationen aus der ein Format wie z. B. XML oder JSON.

Als Nächstes passen wir die Logik zum Verwenden von Animationen entsprechend der Richtung, die das Zeichen verschoben wird oder nach der letzten Animation, wenn das Zeichen gerade beendet hat. Zu diesem Zweck ändern wir die `Update` Methode:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Der Code, um die Animationen zu wechseln, wird in zwei Blöcke unterteilt. Die erste überprüft wird, wenn der Velocity nicht gleich ist `Vector2.Zero` – Dies gibt an, ob das Zeichen verschoben werden. Wenn das Zeichen bewegt wird, sehen wir die `velocity.X` und `velocity.Y` Werte, um zu bestimmen, welche walkingstrecken Animation wiedergegeben.

Wenn das Zeichen ist nicht das Verschieben, möchten wird für des Zeichens des `currentAnimation` zu einem ständigen-Animation –, aber wir nur tun, wenn die `currentAnimation` ist ein Schritt für Schritt Animation oder wenn eine Animation nicht festgelegt wurde. Wenn die `currentAnimation` ist keiner der vier walkingstrecken Animationen, und klicken Sie dann das Zeichen bereits verfügbar ist, sodass wir nicht ändern müssen `currentAnimation`.

Das Ergebnis dieses Codes ist, dass das Zeichen ordnungsgemäß zu animieren, wenn durchlaufen wird, und klicken Sie dann stehen vor der letzten Richtung, in die sie durchlaufen wurde, nachdem dieser gestoppt wurde:

![](part2-images/image7.gif "Das Ergebnis dieses Codes ist, dass das Zeichen ordnungsgemäß zu animieren, wenn durchlaufen wird, und klicken Sie dann stehen vor der letzten Richtung, in die sie durchlaufen wurde, nachdem dieser gestoppt wurde")

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie Arbeiten mit MonoGame erstellen Sie eine plattformübergreifende Spiele mit Sprites, das Verschieben von Objekten, eingabeerkennung und Animation. Es wird beschrieben, wie eine allgemeines Animation-Klasse. Es hat auch gezeigt, wie Sie eine Zeichenentität für die Organisation von der Codelogik zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [CharacterSheet Bildressource (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Durchlaufen eines Spiels abgeschlossen (Beispiel)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
