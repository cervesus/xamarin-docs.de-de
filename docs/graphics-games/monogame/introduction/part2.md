---
title: Teil 2 – Implementieren von walkinggame
description: In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Spiellogik und Inhalt einem leeren monogame-Projekt hinzufügen, um eine Demo eines animierten Sprite zu erstellen, das sich mit der Fingereingabe bewegt
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 2c290ac7d66147342087342bda5e5a19b4e6e6f7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763622"
---
# <a name="part-2--implementing-the-walkinggame"></a>Teil 2 – Implementieren von walkinggame

_In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Spiellogik und Inhalt einem leeren monogame-Projekt hinzufügen, um eine Demo eines animierten Sprite zu erstellen, das sich mit der Fingereingabe bewegt_

In den vorherigen Abschnitten dieser exemplarischen Vorgehensweise wurde gezeigt, wie leere monogame-Projekte erstellt werden. Wir bauen auf diese vorherigen Teile auf, indem wir eine einfache Spiele Demo durchführen. Dieser Artikel enthält folgende Abschnitte:

- Entpacken von Spielinhalten
- Monogame-Klassen Übersicht
- Rendern des ersten Sprite
- Erstellen der Entität "Merkmal"
- Hinzufügen von "Merkmal Entity" zum Spiel
- Erstellen der Animations Klasse
- Hinzufügen der ersten Animation zu "Merkmal Entität"
- Hinzufügen von Bewegung zum Zeichen
- Übereinstimmende Bewegung und Animation

## <a name="unzipping-our-game-content"></a>Entpacken von Spielinhalten

Bevor wir mit dem Schreiben von Code beginnen, möchten wir unsere Spiel *Inhalte*entzippen. Spieleentwickler verwenden häufig den Begriff *Inhalt* , um auf nicht-Code Dateien zu verweisen, die normalerweise von visuellen Künstlern, Spiel-Designern oder audiodesignern erstellt werden. Zu den gängigen Inhaltstypen gehören Dateien, die zum Anzeigen von Visuals, Abspielen von Sound oder zum Steuern von künstlicher Intelligenz Verhalten verwendet werden. Aus der Perspektive eines Spiel Entwicklungsteams wird in der Regel von nicht Programmierern erstellt.

Den hier verwendeten Inhalt finden Sie [auf GitHub](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Diese Dateien müssen an einen Speicherort heruntergeladen werden, auf den wir später in dieser exemplarischen Vorgehensweise zugreifen werden.

## <a name="monogame-class-overview"></a>Monogame-Klassen Übersicht

Zunächst werden die monogame-Klassen erläutert, die beim einfachen Rendering verwendet werden:

- `SpriteBatch`– wird verwendet, um 2D-Grafiken auf dem Bildschirm zu zeichnen. *Sprites* sind visuelle 2D-Elemente, die verwendet werden, um Bilder auf dem Bildschirm anzuzeigen. Das `SpriteBatch` -Objekt kann jeweils ein einzelnes Sprite zwischen seinen `Begin` -und- `End` Methoden zeichnen, oder es können mehrere Sprites gruppiert oder in einem *Batch*zusammengefasst werden.
- `Texture2D`– stellt ein Bild Objekt zur Laufzeit dar. `Texture2D`Instanzen werden oft aus Dateiformaten wie PNG oder bmp erstellt, obwohl Sie auch zur Laufzeit dynamisch erstellt werden können. `Texture2D`-Instanzen werden beim Rendern `SpriteBatch` mit-Instanzen verwendet.
- `Vector2`– stellt eine Position in einem 2D-Koordinatensystem dar, das häufig zum Positionieren von visuellen Objekten verwendet wird. Monogame umfasst `Vector3` auch und `Vector4` , aber wir verwenden `Vector2` nur in dieser exemplarischen Vorgehensweise.
- `Rectangle`– Ein vierseitiger Bereich mit Position, Breite und Höhe. Wir verwenden dies, um zu definieren, welcher Teil des `Texture2D` zu Rendering ist, wenn wir mit der Arbeit mit Animationen beginnen.

Beachten Sie auch, dass monogame nicht von Microsoft verwaltet wird – trotz seines Namespace. Der `Microsoft.Xna` Namespace wird in monogame verwendet, um die Migration vorhandener XNA-Projekte zu monogame zu vereinfachen.

## <a name="rendering-our-first-sprite"></a>Rendern des ersten Sprite

Als nächstes zeichnen wir eine einzelne Sprite auf dem Bildschirm, um zu zeigen, wie das 2D-Rendering in monogame durchgeführt wird.

### <a name="creating-a-texture2d"></a>Erstellen eines Texture2D

Wir müssen eine `Texture2D` -Instanz erstellen, die beim Rendern des Sprite verwendet werden soll. Alle Spielinhalte sind letztendlich in einem Ordner namens " **Content** " enthalten, der sich im plattformspezifischen Projekt befindet. Freigegebene monogame-Projekte dürfen keinen Inhalt enthalten, da der Inhalt Buildaktionen verwenden muss, die für die Plattform spezifisch sind. Den Inhalts Ordner finden Sie im IOS-Projekt und im Ordner "Assets" im Android-Projekt.

Um den Inhalt unseres Spiels hinzuzufügen, klicken Sie mit der rechten Maustaste auf den **Inhalts** Ordner, und wählen Sie **Hinzufügen > Dateien hinzufügen...** Navigieren Sie zu dem Speicherort, an dem die Datei Content. zip extrahiert wurde, und wählen Sie die Datei " **Merkmal Blatt. png** ". Wenn Sie gefragt werden, wie die Datei dem Ordner hinzugefügt werden soll, sollten Sie die Option **Kopieren** auswählen:

![Wenn Sie gefragt werden, wie die Datei dem Ordner hinzugefügt werden soll, wählen Sie die Option Kopieren aus.](part2-images/image1.png)

Der Inhalts Ordner enthält jetzt die Datei "Merkmal Blatt. png":

![Der Inhalts Ordner enthält jetzt die Datei "Merkmal Blatt. png".](part2-images/image2.png)

Als Nächstes fügen Sie Code hinzu, um die Datei "Merkmal Blatt. png" zu `Texture2D`laden und eine zu erstellen. Öffnen Sie hierzu die `Game1.cs` Datei, und fügen Sie der Game1.cs-Klasse das folgende Feld hinzu:

```csharp
Texture2D characterSheetTexture;
```

Als Nächstes erstellen wir den `characterSheetTexture` in der `LoadContent` -Methode. Vor einer beliebigen `LoadContent` Änderungs Methode sieht dies wie folgt aus:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Beachten Sie, dass das Standard Projekt bereits die `spriteBatch` Instanz für uns instanziiert. Wir verwenden dies später, aber wir werden keinen zusätzlichen Code hinzufügen, um die für `spriteBatch` die Verwendung vorzubereiten. Andererseits ist für unsere `spriteSheetTexture` eine Initialisierung erforderlich, daher wird Sie nach der `spriteBatch` Erstellung von initialisiert:

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

Nachdem wir nun über eine `SpriteBatch` -Instanz und `Texture2D` eine-Instanz verfügen, können wir den `Game1.Draw` Renderingcode zur-Methode hinzufügen:

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

Die Ausführung des Spiels zeigt nun ein einzelnes Sprite an, das die Textur anzeigt, die aus "Merkmal Sheet. png" erstellt wurde

![Beim Ausführen des Spiels wird jetzt ein einzelner Sprite angezeigt, der die Textur anzeigt, die aus "Merkmal Sheet. png"](part2-images/image3.png)

## <a name="creating-the-characterentity"></a>Erstellen der Entität "Merkmal"

Bisher haben wir Code zum Rendering eines einzelnen Sprite auf dem Bildschirm hinzugefügt. Wenn wir jedoch ein vollständiges Spiel entwickeln, ohne andere Klassen zu erstellen, würden wir uns mit Code-Organisations Problemen befassen.

### <a name="what-is-an-entity"></a>Was ist eine Entität?

Ein gängiges Muster für das Organisieren von Spiel Code besteht darin, eine neue Klasse für jeden Game- *Entitätstyp* zu erstellen. Eine Entität in der Spieleentwicklung ist ein Objekt, das einige der folgenden Eigenschaften enthalten kann (nicht alle sind erforderlich):

- Ein visuelles Element, z. b. ein Sprite-, Text-oder 3D-Modell
- Physik oder jedes Frame Verhalten, z. b. ein Einheiten Patroll für einen Set-Pfad oder ein Spieler Zeichen, das auf die Eingabe reagiert
- Kann dynamisch erstellt und zerstört werden, z. b. Wenn ein Energiespar Tool angezeigt wird und vom Player erfasst wird.

Entity Organization Systems kann komplex sein, und viele Spiel-Engines bieten Klassen zur Unterstützung der Verwaltung von Entitäten. Wir implementieren ein sehr einfaches Entitäts System. es ist also erwähnenswert, dass vollständige Spiele in der Regel mehr Organisationseinheiten im Entwickler Teil erfordern.

### <a name="defining-the-characterentity"></a>Definieren der Merkmal Entität

Unsere von uns aufzurufende `CharacterEntity`Entität weist die folgenden Eigenschaften auf:

- Die Möglichkeit, eine eigene zu laden`Texture2D`
- Die Möglichkeit, sich selbst zu Rendering, einschließlich der enthaltenden Aufruf Methoden zum Aktualisieren der Animation für das durchlaufen
- `X`und Y-Eigenschaften, um die Position des Zeichens zu steuern.
- Die Möglichkeit, sich selbst zu aktualisieren – insbesondere, um Werte vom Touchscreen zu lesen und die Position entsprechend anzupassen.

Um dem `CharacterEntity` Spiel hinzuzufügen, klicken Sie mit der rechten Maustaste auf das Projekt **walkinggame** , und klicken Sie auf **Hinzufügen > neue Datei...** . Wählen Sie die Option **leere Klasse** aus, benennen Sie die neue Datei **Merkmal Entität**, und klicken Sie dann auf **neu**.

Zuerst fügen wir die Möglichkeit `CharacterEntity` hinzu, eine `Texture2D` zu laden und selbst zu zeichnen. Die neu hinzugefügte `CharacterEntity.cs` Datei wird wie folgt geändert:

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

Der obige Code erhöht die Verantwortung für die Positionierung, das `CharacterEntity`Rendering und das Laden von Inhalten in. Nehmen wir uns einen Moment Zeit, um auf einige Überlegungen im obigen Code zu verweisen.

### <a name="why-are-x-and-y-floats"></a>Warum sind X und Y nicht verfügbar?

Entwickler, die mit der Spielprogrammierung noch nicht vertraut sind `float` , Fragen sich vielleicht, warum `int` der `double`Typ anstelle von oder verwendet wird. Der Grund hierfür ist, dass ein 32-Bit-Wert am häufigsten für die Positionierung in Renderingcode auf niedriger Ebene gilt. Der Double-Typ belegt 64 Bits für die Genauigkeit, die nur selten zum Positionieren von Objekten benötigt werden. Während ein 32-Bit-Unterschied unbedeutend erscheinen mag, enthalten viele moderne Spiele Grafiken, die die Verarbeitung von Zehntausenden von Positions Werten pro Frame erfordern (30 oder 60 mal pro Sekunde). Das Abschneiden der Menge an Arbeitsspeicher, die durch die Grafik Pipeline halbiert werden muss, kann einen erheblichen Einfluss auf die Leistung eines Spiels haben.

Der `int` -Datentyp kann eine geeignete Maßeinheit für die Positionierung sein, aber es kann Einschränkungen bei der Positionierung von Entitäten platzieren. Beispielsweise ist die Verwendung von ganzzahligen Werten schwieriger, eine reibungslose Bewegung für Spiele zu implementieren, die das Zoomen in oder 3D-Kameras `SpriteBatch`unterstützen (die von zugelassen werden).

Schließlich werden wir sehen, dass die Logik, mit der das Zeichen auf dem Bildschirm verschoben wird, die Zeit Steuerungs Werte des Spiels verwendet. Das Ergebnis der Multiplikation der Geschwindigkeit, wie viel Zeit in einem bestimmten Frame vergangen ist, führt selten zu einer ganzen Zahl, daher müssen wir `float` für `X` und `Y`verwenden.

### <a name="why-is-charactersheettexture-static"></a>Warum ist "charakterisheettexture" statisch?

`characterSheetTexture` Die`Texture2D` -Instanz ist eine Lauf Zeit Darstellung der Datei "Merkmal Blatt. png". Mit anderen Worten, Sie enthält die Farbwerte für jedes Pixel, die aus der PNG-Datei des Quell **Zeichenblatts** extrahiert wurden. Wenn unser Spiel zwei `CharacterEntity` Instanzen erstellen würde, benötigt jedes eine den Zugriff auf Informationen von "charakterisheet. png". In diesem Fall möchten wir nicht die Leistungskosten für das doppelte Laden von "Merkmal Blatt. png" verursachen, und es wäre auch nicht gewünscht, dass ein doppelter Texturspeicher im RAM gespeichert wird. Durch die `static` Verwendung eines Felds können wir das gleiche `Texture2D` für alle `CharacterEntity` Instanzen freigeben.

Die `static` Verwendung von Membern für die Lauf Zeit Darstellung von Inhalten ist eine vereinfachte Lösung und behandelt keine Probleme, die bei größeren spielen auftreten, wie z. b. das Entladen von Inhalten beim Wechsel von einer Ebene zu einer anderen. Komplexere Lösungen, die über den Rahmen dieser exemplarischen Vorgehensweise hinausgehen, beinhalten die Verwendung der Inhalts Pipeline von monogame oder das Erstellen eines benutzerdefinierten Inhalts Verwaltungssystems.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Warum wird SpriteBatch als Argument, anstatt von der Entität instanziiert?

Die `Draw` oben implementierte Methode nimmt ein `SpriteBatch` -Argument an, aber im Gegensatz `characterSheetTexture` dazu wird der von `CharacterEntity`instanziiert.

Der Grund hierfür liegt darin, dass das effizienteste Rendering möglich ist, wenn dieselbe `SpriteBatch` Instanz für alle `Draw` Aufrufe verwendet wird, und wenn alle `Draw` Aufrufe zwischen einem einzelnen Satz von-und- `Begin` `End` aufrufen durchgeführt werden. Natürlich enthält unser Spiel nur eine einzelne Entitäts Instanz, aber kompliziertere Spiele profitieren von einem Muster, das es mehreren Entitäten ermöglicht, dieselbe `SpriteBatch` Instanz zu verwenden.

## <a name="adding-characterentity-to-the-game"></a>Hinzufügen von "Merkmal Entity" zum Spiel

Nun, da wir unseren `CharacterEntity` Code für das Rendering hinzugefügt haben, können wir den Code in `Game1.cs` ersetzen, um eine Instanz dieser neuen Entität zu verwenden. Zu diesem Zweck ersetzen wir das `Texture2D` Feld durch ein `CharacterEntity` Feld in `Game1`:

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

Als Nächstes fügen wir die Instanziierung dieser Entität in `Game1.Initialize`hinzu:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Wir müssen auch die `Texture2D` Erstellung von `LoadContent` entfernen, da diese jetzt in unserer `CharacterEntity`verarbeitet wird. `Game1.LoadContent`sollte wie folgt aussehen:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Beachten Sie, dass die `LoadContent` Methode trotz ihres Namens nicht die einzige Stelle ist, an der Inhalte geladen werden können – viele Spiele implementieren das Laden von Inhalten in ihrer Initialisierungs Methode oder sogar in Ihrem Aktualisierungs Code als Inhalt. möglicherweise ist dies erst erforderlich, wenn der Player einen ein bestimmter Punkt des Spiels.

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

Wenn wir das Spiel ausführen, wird jetzt das Zeichen angezeigt. Da X und Y standardmäßig auf 0 (null) eingestellt sind, wird das Zeichen in der oberen linken Ecke des Bildschirms positioniert:

![Da X und Y standardmäßig auf 0 (null) eingestellt sind, wird das Zeichen in der oberen linken Ecke des Bildschirms positioniert.](part2-images/image4.png)

## <a name="creating-the-animation-class"></a>Erstellen der Animations Klasse

Zurzeit zeigt die Datei mit dem vollständigen **Merkmal Blatt. png** an. `CharacterEntity` Diese Anordnung mehrerer Bilder in einer Datei wird als *Sprite-Tabelle*bezeichnet. In der Regel wird ein Sprite nur einen Teil des Sprite-Blatts Rendering. Wir ändern den `CharacterEntity` , um einen Teil dieser " **Merkmal Sheet. png**" zu erzeugen, und dieser Teil ändert sich im Laufe der Zeit, um eine "Walking Animation" anzuzeigen.

Wir erstellen die `Animation` -Klasse, um die Logik und den Zustand der charakteritätsentitäts Animation zu steuern. Bei der Animations Klasse handelt es sich um eine allgemeine Klasse, die für jede Entität und `CharacterEntity` nicht nur für Animationen verwendet werden kann. Ultimate die `Animation` -Klasse stellt eine `Rectangle` bereit, `CharacterEntity` die von verwendet wird, um sich selbst zu zeichnen. Wir erstellen auch eine `AnimationFrame` -Klasse, die zum Definieren der Animation verwendet wird.

### <a name="defining-animationframe"></a>Definieren von animationframe

`AnimationFrame`enthält keine Logik im Zusammenhang mit der Animation. Wir verwenden Sie nur zum Speichern von Daten. Klicken Sie zum `AnimationFrame` hinzufügen der Klasse mit der rechten Maustaste auf das freigegebene Projekt " **walkinggame** ", und klicken Sie auf **Hinzufügen > neue Datei....** Geben Sie den Namen **animationframe** ein, und klicken Sie auf die Schaltfläche **neu** . Die `AnimationFrame.cs` Datei wird so geändert, dass Sie den folgenden Code enthält:

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

Die `AnimationFrame` -Klasse enthält zwei Informationen:

- `SourceRectangle`– Definiert den Bereich des `Texture2D` , der von der `AnimationFrame`angezeigt wird. Dieser Wert wird in Pixel gemessen, wobei die obere linke Ecke (0,0) ist.
- `Duration`– Definiert, wie lange `AnimationFrame` eine angezeigt wird, wenn Sie `Animation`in einer verwendet wird.

### <a name="defining-animation"></a>Definieren von Animationen

Die `Animation` -Klasse `List<AnimationFrame>` enthält sowie die Logik, um zu wechseln, welcher Frame aktuell angezeigt wird, je nachdem, wie viel Zeit vergangen ist.

Klicken Sie zum `Animation` hinzufügen der Klasse mit der rechten Maustaste auf das freigegebene Projekt " **walkinggame** ", und klicken Sie auf **Hinzufügen > neue Datei...** . Geben Sie die namens **Animation** ein, und klicken Sie auf die Schaltfläche **neu** Wir ändern die `Animation.cs` Datei, sodass Sie den folgenden Code enthält:

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

Sehen wir uns einige Details `Animation` der-Klasse an.

### <a name="frames-list"></a>Frames-Liste

Der `frames` Member speichert die Daten für unsere Animation. Der Code, der die Animationen instanziiert, `AnimationFrame` fügt der `frames` Liste mithilfe der `AddFrame` -Methode Instanzen hinzu. Eine umfassendere Implementierung bietet `public` möglicherweise Methoden oder Eigenschaften zum Ändern `frames`, aber wir schränken die Funktionalität ein, um Rahmen für diese exemplarische Vorgehensweise hinzuzufügen.

### <a name="duration"></a>Dauer

Duration gibt die Gesamtdauer `Animation,` der-Instanz zurück, die durch Hinzufügen der Dauer aller enthaltenen `AnimationFrame` -Instanzen abgerufen wird. Dieser Wert kann zwischengespeichert werden `AnimationFrame` , wenn ein unveränderliches Objekt wäre, aber da wir animationframe als eine Klasse implementiert haben, die nach dem Hinzufügen zur Animation geändert werden kann, müssen wir diesen Wert berechnen, wenn auf die Eigenschaft zugegriffen wird.

### <a name="update"></a>Update

Die `Update` -Methode sollte alle Frames aufgerufen werden (d. h. jedes Mal, wenn das gesamte Spiel aktualisiert wird). Der Zweck besteht darin, den `timeIntoAnimation` Member zu vergrößern, der verwendet wird, um den aktuell angezeigten Frame zurückzugeben. Durch die Logik `Update` in wird `timeIntoAnimation` verhindert, dass die Dauer der gesamten Animation überschreitet.

Da wir die `Animation` -Klasse verwenden, um eine "Walking Animation" anzuzeigen, möchten wir, dass diese Animation wiederholt wird, wenn Sie das Ende erreicht hat. Dies können Sie erreichen, indem Sie über `timeIntoAnimation` prüfen, ob größer `Duration` als der Wert ist. Wenn dies der Fall ist, werden Sie wieder auf den Anfang zurückgeführt und den Rest beibehalten. Dies führt zu einer Schleifen Animation.

### <a name="obtaining-the-current-frames-rectangle"></a>Abrufen des Rechtecks des aktuellen Frames

Der Zweck `Animation` der-Klasse besteht letztendlich darin, eine `Rectangle` bereitzustellen, die beim Zeichnen eines Sprite verwendet werden kann. Die `Animation` -Klasse enthält derzeit Logik zum Ändern `timeIntoAnimation` des Members, mit dem wir abrufen können, `Rectangle` welche angezeigt werden soll. Hierzu erstellen Sie eine `CurrentRectangle` Eigenschaft in der `Animation` Klasse. Kopieren Sie diese Eigenschaft in `Animation` die-Klasse:

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

## <a name="adding-the-first-animation-to-characterentity"></a>Hinzufügen der ersten Animation zu "Merkmal Entität"

Der `CharacterEntity` enthält Animationen für das laufen und die Position sowie einen Verweis auf den aktuellen `Animation` , der angezeigt wird.

Zuerst fügen wir unsere erste `Animation,` hinzu, die wir verwenden werden, um die Funktionalität zu testen, wie bisher geschrieben. Fügen Sie der Entitäts Klasse "Merkmal" die folgenden Member hinzu:

```csharp
Animation walkDown;
Animation currentAnimation;
```

Als nächstes definieren wir die `walkDown` Animation. Um dies zu tun, `CharacterEntity` ändern Sie den Konstruktor wie folgt:

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

Wie bereits erwähnt, müssen wir für Zeit `Animation.Update` basierte Animationen zum Abspielen aufzurufen. Wir müssen auch das `currentAnimation`zuweisen. Vorerst weisen wir den `currentAnimation` zu `walkDown`, aber wir werden diesen Code später ersetzen, wenn wir unsere Verschiebungs Logik implementieren. Wir fügen die `Update` - `CharacterEntity` Methode wie folgt hinzu:

```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Nachdem wir nun die zugewiesen `currentAnimation` und aktualisiert haben, können wir Sie zum Durchführen der Zeichnung verwenden. Wir werden wie `CharacterEntity.Draw` folgt ändern:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Der letzte Schritt zum Animieren der `CharacterEntity` Animation besteht darin, die neu hinzugefügte `Update` Methode `Game1`von aufzurufen. Ändern `Game1.Update` Sie wie folgt:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Nun wird `CharacterEntity` `walkDown` die Animation wiedergegeben:

![Nun wird die charakteritätsentität die Exemplare Exemplare Animation wiedergeben](part2-images/image5.gif)

## <a name="adding-movement-to-the-character"></a>Hinzufügen von Bewegung zum Zeichen

Als Nächstes fügen wir mithilfe von Berührungs Steuerelementen eine Bewegung zum Zeichen hinzu. Wenn der Benutzer den Bildschirm berührt, wird das Zeichen an den Punkt verschoben, an dem der Bildschirm berührt wird. Wenn keine Berührungen erkannt werden, wird das Zeichen vorhanden sein.

### <a name="defining-getdesiredvelocityfrominput"></a>Definieren von getdesiredvelocityfrominput

Wir verwenden die `TouchPanel` Klasse von monogame, die Informationen über den aktuellen Zustand des Touchscreens bereitstellt. Fügen wir eine Methode hinzu, mit der `TouchPanel` überprüft wird und die gewünschte Geschwindigkeit des Zeichens zurückgegeben wird:

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

Die `TouchPanel.GetState` -Methode gibt `TouchCollection` einen zurück, der Informationen darüber enthält, wo der Benutzer den Bildschirm berührt. Wenn der Benutzer den Bildschirm nicht berührt, ist das `TouchCollection` leer. in diesem Fall sollte das Zeichen nicht verschoben werden.

Wenn der Benutzer den Bildschirm berührt, verschieben wir das Zeichen in den ersten touchabdruck, d. h., `TouchLocation` bei Index 0. Zunächst legen wir die gewünschte Geschwindigkeit so fest, dass Sie dem Unterschied zwischen dem Speicherort des Zeichens und dem Speicherort des ersten Berührungs Zeichens entspricht:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Im folgenden finden Sie ein bisschen Mathematik, das das Zeichen mit der gleichen Geschwindigkeit bewegt. Um zu erläutern, warum dies wichtig ist, betrachten wir eine Situation, in der der Benutzer den Bildschirm 500 Pixel vom Speicherort des Zeichens berührt. Die erste Zeile, `desiredVelocity.X` in der festgelegt wird, weist den Wert 500 zu. Wenn der Benutzer den Bildschirm jedoch nur mit einem Abstand von nur 100 Einheiten vom Zeichen berührt, wird der `desiredVelocity.X` auf 100 festgelegt. Das Ergebnis wäre, dass die Verschiebungs Geschwindigkeit des Zeichens darauf zurückzuführen wäre, wie weit der Berührungspunkt vom-Zeichen entfernt ist. Da das Zeichen immer mit der gleichen Geschwindigkeit bewegt werden soll, muss die desiredvelocity geändert werden.

Die `if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` Anweisung überprüft, ob die Geschwindigkeit ungleich NULL ist – mit anderen Worten, Sie prüft, ob der Benutzer dieselbe Stelle wie die aktuelle Position des Zeichens berührt. Wenn dies nicht der Wert ist, müssen wir die Geschwindigkeit des Zeichens so festlegen, dass es konstant ist, unabhängig davon, wie weit die Berührungs Geschwindigkeit ist. Dies erreichen Sie, indem der Geschwindigkeitsvektor normalisiert wird. Dies ergibt eine Länge von 1. Ein Geschwindigkeitsvektor von 1 bedeutet, dass das Zeichen bei 1 Pixel pro Sekunde verschoben wird. Wir werden dies beschleunigen, indem wir den Wert mit der gewünschten Geschwindigkeit von 200 multiplizieren.

### <a name="applying-velocity-to-position"></a>Anwenden der Geschwindigkeit auf die Position

Die von `GetDesiredVelocityFromInput` zurückgegebene Geschwindigkeit muss auf die-und `Y` - `X` Werte des Zeichens angewendet werden, damit Sie zur Laufzeit wirksam werden. Wir ändern die `Update` Methode wie folgt:

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

Was wir hier implementiert haben, ist eine *zeitbasierte* Bewegung (im Gegensatz zur *Frame basierten* Bewegung). Die zeitbasierte Verschiebung multipliziert einen Geschwindigkeitswert (in unserem Fall die Werte, die in der `velocity` Variablen gespeichert sind) nach der Zeit, die seit dem letzten Update, das `gameTime.ElapsedGameTime.TotalSeconds`in gespeichert ist, verstrichen ist. Wenn das Spiel mit weniger Frames pro Sekunde ausgeführt wird, wird die verstrichene Zeit zwischen Frames erreicht – das Endergebnis besteht darin, dass Objekte, die zeitbasierte Verschiebungen verwenden, unabhängig von der Framerate immer dieselbe Geschwindigkeit verschieben.

Wenn wir jetzt unser Spiel ausführen, sehen wir, dass das Zeichen in die Berührungs Position wechselt:

![Das Zeichen wird an die Berührungs Position verschoben.](part2-images/image6.gif)

## <a name="matching-movement-and-animation"></a>Übereinstimmende Bewegung und Animation

Nachdem wir das Zeichen bewegt und eine einzelne Animation wiedergegeben haben, können wir den Rest unserer Animationen definieren und Sie dann verwenden, um die Bewegung des Zeichens widerzuspiegeln. Nach Abschluss der Fertigstellung haben wir insgesamt acht Animationen:

- Animationen zum nach oben, nach unten, Links und rechts
- Animationen für die Position nach oben, unten, Links und rechts

### <a name="defining-the-rest-of-the-animations"></a>Definieren der restlichen Animationen

Zuerst fügen wir die `Animation` -Instanzen der- `CharacterEntity` Klasse für alle Animationen an derselben Stelle hinzu, an der wir hinzugefügt `walkDown`haben. Nachdem dies erfolgt ist, verfügt `CharacterEntity` die über die folgenden `Animation` Member:

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

Nun definieren wir die Animationen im `CharacterEntity` Konstruktor wie folgt:

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

Beachten Sie, dass der obige Code dem `CharacterEntity` Konstruktor hinzugefügt wurde, um diese exemplarische Vorgehensweise zu verkürzen. Games trennt die Definition von Zeichen Animationen in der Regel in Ihre eigenen Klassen oder lädt diese Informationen aus einem Datenformat, wie z. b. XML oder JSON.

Als nächstes passen wir die Logik an, um die Animationen entsprechend der Richtung zu verwenden, in der das Zeichen verschoben wird, oder entsprechend der letzten Animation, wenn das Zeichen gerade beendet wurde. Zu diesem Zweck ändern wir die `Update` -Methode:

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

Der Code zum Wechseln der Animationen wird in zwei Blöcke aufgeteilt. Der erste prüft, `Vector2.Zero` ob die Geschwindigkeit nicht gleich ist – dies gibt an, ob das Zeichen verschoben wird. Wenn das Zeichen verschoben wird, können wir die `velocity.X` -und- `velocity.Y` Werte überprüfen, um zu bestimmen, welche Animation abgespielt werden soll.

Wenn das Zeichen nicht verschoben wird, möchten wir die Zeichen `currentAnimation` Folge auf eine permanente Animation festlegen – aber wir tun dies nur, `currentAnimation` wenn es sich bei um eine durch laufende Animation handelt, oder wenn eine Animation nicht festgelegt wurde. Wenn keine der vier durch laufenden Animationen `currentAnimation` ist,wirddasZeichenbereitsangezeigt,sodasskeineÄnderungenvorgenommenwerdenmüssen.`currentAnimation`

Das Ergebnis dieses Codes ist, dass das Zeichen beim Durchlaufen ordnungsgemäß animiert und anschließend die letzte Richtung angezeigt wird, in der es angehalten wurde:

![Das Ergebnis dieses Codes ist, dass das Zeichen beim Durchlaufen ordnungsgemäß animiert und anschließend die letzte Richtung angezeigt wird, in der es angehalten wurde.](part2-images/image7.gif)

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde die Verwendung von monogame zum Erstellen eines plattformübergreifenden Spiels mit Sprites, dem Verschieben von Objekten, der Eingabe Erkennung und der Animation veranschaulicht. Es wird das Erstellen einer allgemeinen Animations Klasse behandelt. Außerdem wurde gezeigt, wie eine Zeichen Entität zum Organisieren von Code Logik erstellt wird.

## <a name="related-links"></a>Verwandte Links

- [Bildressource "Merkmal Blatt" (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Durchlaufen des Spiels (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/walkinggamemg/)
