---
title: Entitäten in CocosSharp
description: Das Muster für die Entität ist eine leistungsfähige Möglichkeit zur spielcode zu organisieren. Es verbessert die Lesbarkeit, wird Code einfacher zu verwalten und nutzt die Funktionen der integrierten über-und untergeordneten Elementen.
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 58a8d4e6fcb8a2165fafad74a5c59481d1550351
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921910"
---
# <a name="entities-in-cocossharp"></a>Entitäten in CocosSharp

_Das Muster für die Entität ist eine leistungsfähige Möglichkeit zur spielcode zu organisieren. Es verbessert die Lesbarkeit, wird Code einfacher zu verwalten und nutzt die Funktionen der integrierten über-und untergeordneten Elementen._

Das Muster für die Entität kann ein Entwickler den Aufwand mit CocosSharp über verbesserte Code Organisation verbessert werden. In dieser exemplarischen Vorgehensweise werden eine praktische Beispiel zeigt, wie zwei Entitäten – eine Lieferadresse-Entität und eine Aufzählungszeichen-Entität erstellen. Diese Entitäten werden eigenständige Objekte, d. h., die einmal instanziiert diese wird automatisch gerendert werden und die Verschiebung Logik entsprechend für ihren Typ ausgeführt wird. 

Dieses Handbuch enthält die folgenden Themen:

 - Einführung in Spiel Entitäten
 - Allgemeine im Vergleich zu bestimmten Entitätstypen
 - Projekterstellung
 - Erstellen von Entitätsklassen
 - Hinzufügen von Entitätsinstanzen auf die `GameLayer`
 - Das reagieren auf Entität Logik in der `GameLayer`

Das Spiel abgeschlossene wird wie folgt aussehen:

![](entities-images/image1.png "Das Spiel abgeschlossene wird wie folgt aussehen.")


## <a name="introduction-to-game-entities"></a>Einführung in Spiel Entitäten

Spiele Entitäten sind Klassen, die Objekte, benötigen Rendering, Konflikt, physikalische oder KI Logik definieren. Glücklicherweise entsprechen die Entitäten in einem Spiel Codebasis vorhanden häufig der konzeptionellen Objekte in einem Spiel. Wenn dies zutrifft, kann das Identifizieren der Entitäten in einem Spiel benötigt mehr problemlos ausgeführt werden. 

Z. B. ein Leerzeichen Designs [Schießen angegebene Geviertgröße oben Spiel](http://en.wikipedia.org/wiki/Shoot_%27em_up) kann die folgenden Elemente enthalten:

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

Diese Entitäten würde ihre eigenen Klassen im Spiel bisher und jede Instanz nur wenig oder keine Setup über das hinaus Instanziierung erfordern würde.


## <a name="general-vs-specific-entity-types"></a>Allgemeine im Vergleich zu bestimmten Entitätstypen

Eine der ersten Fragen gegenüberstehen Spieleentwicklern mit einer Entität ist wie viel die Entitäten zu generalisieren. Die spezifischste Implementierungen würden Klassen für jeden Typ der Entität, definieren, auch wenn sie durch einige Merkmale unterscheiden wird. Weitere allgemeine Systeme werden Kombinieren von Gruppen von Entitäten in einer Klasse, und Instanzen, die angepasst werden.

Nehmen wir z. B. ein Spiel Speicherplatz, der die folgenden Klassen definiert:

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

Ein weitere generalisierten Ansatz wäre, erstellen Sie eine einzelne Klasse für Player umfasst und eine einzelne Klasse für Gegners umfasst, die zur Unterstützung der verschiedenen Ship Typen konfiguriert werden konnte. Anpassung eventuell welches Abbild zum Laden, welche Art von Aufzählungszeichen Entitäten erstellen, wenn die Verschiebung Koeffizienten, behandeln und AI-Logik für die Gegner geliefert wird. In diesem Fall kann die Liste der Entitäten auf reduziert werden:

 - `PlayerShip`
 - `EnemyShip`

Natürlich können diese Entitätstypen Weitere verallgemeinert werden pro Instanz Anpassung zum Steuern der Verschiebung zu ermöglichen. Player Ship Instanzen würden Eingabe gelesen, während Gegners Ship Instanzen AI-Logik ausführen können. Dies bedeutet, dass die Entitäten verallgemeinert werden könnte auch weiterhin in eine einzelne Klasse:

 - `Ship`

Der Generalisierung kann noch weiter – weiterhin ein Spiel, das eine einzelne Basisklasse für alle Entitäten verwenden kann. Diese einzelne Klasse, die aufgerufen werden kann `GameEntity`, würde der Klasse für jede Entitätsinstanz in der gesamten Spiel verwendet werden, einschließlich der Entitäten, die nicht bereitgestellt wird, z. B. Aufzählungszeichen und entladbarer Objekte (Power-USV).

Dies die meisten Allgemein Systeme wird häufig als bezeichnet eine *Komponente System*. In einem solchen System können Spiele Entitäten einzelne Komponenten, z. B. physikalische AI, oder Rendern von Komponenten zum Anpassen des Aussehens und Verhaltens hinzugefügt. Reine Komponente-basierte Systeme aktivieren Sie die maximale Flexibilität und durch die Verwendung der Vererbung, z. B. komplexe Vererbung Ketten verursachte Probleme vermeiden können. Wie bei anderen verallgemeinerungen, effektive Komponente Systeme können schwierig sein, einrichten und die Ausdruckskraft des Codes reduzieren.

Die Ebene der generalize-Option verwendet, hängt von viele Aspekte zu berücksichtigen, einschließlich:

 - Spiel Größe – können kleinere Spiele leisten, bestimmte Klassen zu erstellen, während größere Spiele möglicherweise schwierig, mit einer großen Anzahl von Klassen zu verwalten.
 - Datengesteuerte Entwicklung – Spiele, die auf Daten (Bilder, 3D-Modelle und Datendateien wie JSON oder XML) beruhen profitieren möglicherweise von Entitätstypen, dass generalisiert und konfigurieren die Einzelheiten basierend auf Daten. Dies ist besonders für Spiele, die auf neue Inhalte hinzufügen, während der Entwicklung oder nach der Freigabe des Spiels erwarten importieren.
 - Spiel-Engine-Muster – empfehlen einige Spiel Module die Verwendung von Systemen, Komponente, während andere Entwickler zu entscheiden, wie Entitäten organisieren können. CocosSharp ist nicht die Verwendung eines Systems Komponente erforderlich, sodass Entwickler frei, um jeden Typ von Element zu implementieren sind. 

Der Einfachheit halber wird einen bestimmten Klasse basierenden Ansatz mit eine Einheit liefern und Aufzählungszeichen für dieses Lernprogramm verwendet werden.


## <a name="project-setup"></a>Projekterstellung

Bevor wir beginnen, unsere Entitäten implementieren, müssen wir ein Projekt zu erstellen. Wir müssen die CocosSharp-Projektvorlagen verwenden, um projekterstellung zu vereinfachen. [Überprüfen Sie diesen Beitrag](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) Informationen zum Erstellen eines CocosSharp-Projekts von Visual Studio für Mac-Vorlagen. Der Rest dieses Handbuchs verwendet den Namen des Projekts **EntityProject**.

Sobald unsere Projekt erstellt wurde müssen wir die Auflösung der unsere Spiel 480 x 320 Ausführungsintervall festgelegt. Zu diesem Zweck rufen `CCScene.SetDefaultDesignResolution` in die `GameAppDelegate.ApplicationDidFinishLaunching` Methode wie folgt:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

Weitere Informationen zum Umgang mit CocosSharp Lösungen finden Sie unsere [Administratorhandbuch zur Behandlung von mehrere Auflösungen in CocosSharp](~/graphics-games/cocossharp/resolutions.md).


## <a name="adding-content-to-the-project"></a>Hinzufügen von Inhalt zum Projekt

Nachdem das Projekt erstellt wurde, wird es in enthaltenen Dateien hinzufügen [diese Inhalte Zip-Datei](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true). Zu diesem Zweck herunterladen Sie die Zip-Datei und Entpacken Sie es. Fügen Sie beide **ship.png** und **bullet.png** auf die **Content** Ordner. Die **Content** Ordner werden innerhalb der **Bestand** Ordner auf Android-Geräten und im Stammverzeichnis des Projekts unter iOS werden. Nachdem hinzugefügt, sehen wir beide Dateien in den **Content** Ordner:

![](entities-images/image2.png "Nachdem hinzugefügt, sollten beide Dateien im Ordner \"Content\" sein.")


## <a name="creating-the-ship-entity"></a>Die Ship-Entität erstellen

Die `Ship` Klasse des Spiels erste Entität sein wird. Hinzufügen einer `Ship` Klasse, erstellen Sie zunächst einen Ordner namens **Entitäten** auf der Stammebene des Projekts. Hinzufügen einer neuen Klasse in der **Entitäten** Ordner mit dem Namen `Ship`:

![](entities-images/image3.png "Fügen Sie eine neue Klasse im Ordner \"Entitäten\" Ship aufgerufen")

Die erste Änderung stellen wir zu unserem `Ship` Klasse ist, können sie die von erben die `CCNode` Klasse. `CCNode` Dient als Basisklasse für Klassen für allgemeine CocosSharp wie `CCSprite` und `CCLayer`, und erhalten Sie die folgende Funktionalität:

 - `Position` die Eigenschaft für den Liefernamen auf dem Bildschirm verschieben.
 - `Children` -Eigenschaft für das Hinzufügen einer `CCSprite.`
 - `Parent` Eigenschaft, die verwendet werden kann, für die Verbindung `Ship` Instanzen anderer `CCNodes`. Diese Funktion wird nicht in diesem Lernprogramm verwendet werden; größere Spiele häufig nutzen Anfügen von Entitäten in anderen `CCNodes`. 
 - `AddEventListener` die Methode zu reagieren, um die Eingabe für den Liefernamen verschieben.
 - `Schedule` Methode zum Behandeln von Aufzählungszeichen.

Wir fügen auch eine `CCSprite` Instanz fest, damit unser Ship auf dem Bildschirm angezeigt werden kann:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

Als Nächstes fügen wir die Lieferadresse unsere `GameLayer` zur Anzeige in unserer Spiel angezeigt:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

Wenn wir unsere Spiel auszuführen sehen wir nun unsere Ship-Entität:

![](entities-images/image4.png "Bei der Ausführung des Spiels, wird die Ship-Entität angezeigt werden")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>Warum das Vererben der CCNode statt CCSprite?

An diesem Punkt unsere `Ship` ist eine einfache Wrapper für eine `CCSprite` Instanz. Da `CCSprite` erbt auch von `CCNode`, konnten wir geerbt direkt aus `CCSprite`, der eingeschränkte würde des Codes in `Ship.cs`. Darüber hinaus erben direkt von `CCSprite` reduziert die Anzahl der Objekte im Arbeitsspeicher und kann die Leistung erhöht, die Abhängigkeitsstruktur zu verkleinern.

Trotz dieser Vorteile, die wir von geerbten `CCNode` Ausblenden einiger der `CCSprite` Eigenschaften aus jeder Instanz. Z. B. die `Texture` Eigenschaft sollte nicht geändert werden, der außerhalb der `Ship` Klasse und das erben von `CCNode` erlaubt es uns, die diese Eigenschaft blenden. Die öffentlichen Member des unsere Entitäten sind besonders wichtig, wie ein Spiel überschreitet und zusätzliche Entwickler ein Team hinzugefügt werden.


## <a name="adding-input-to-the-ship"></a>Eingabe für den Liefernamen hinzufügen

Nun, dass unsere Ship auf dem Bildschirm sichtbar ist werden wir Eingabe hinzufügen. Unser Ansatz werden ähnlich wie im Ansatz der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md), außer dass wir den Code für die Verschiebung im platziert werden, wird die `Ship` Klasse anstatt im mit `CCLayer` oder `CCScene`.

Fügen Sie den Code um `Ship` Umlagerung, wo der Benutzer den Bildschirm berührt unterstützen:


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

Viele Schießen angegebene Geviertgröße oben Spiele implementieren eine maximale Geschwindigkeit, wobei herkömmlichen Controller-basierten verschieben. Dies bedeutet, dass wir einfach sofortige Bewegung zum getesteten Codes kürzer beibehalten implementieren.


## <a name="creating-the-bullet-entity"></a>Erstellen die Entität Aufzählungszeichen

Die zweite Entität in unserem einfachen Spiel ist eine Entität für die Anzeige von Aufzählungszeichen. Ebenso wie die `Ship` Entität, die `Bullet` Entität enthält eine `CCSprite` , damit es auf dem Bildschirm angezeigt wird. Die Logik für das Verschieben von unterscheidet sich insofern, dass es nicht von Benutzereingaben für das Verschieben von abhängt. stattdessen `Bullet` Instanzen werden in einer geraden Linie mit der Geschwindigkeit Eigenschaften verschoben.

Zunächst fügen wir eine neue Klassendatei zu unserem **Entitäten** Ordner, und rufen sie **Aufzählungszeichen**:

![](entities-images/image5.png "Fügen Sie eine neue Klassendatei zu dem Ordner Entitäten hinzu, und nennen Sie es mit Aufzählungszeichen")

Nach dem Hinzufügen, ändern wir das `Bullet.cs` code wie folgt:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

Abgesehen von der Datei, die zum Ändern der `CCSprite` auf `bullet.png`, den Code in `ApplyVelocity` bietet Bewegung Logik basierend auf zwei Koeffizienten: `VelocityX` und `VelocityY`.

Die `Schedule` Methode ermöglicht das Hinzufügen von Delegaten, die jedem Frame aufgerufen wird. In diesem Fall wir fügen die `ApplyVelocity` Methode so, dass unsere Aufzählungszeichen gemäß seiner Geschwindigkeitswerte verschiebt. Die `Schedule` -Methode übernimmt ein `Action<float>`, in dem der Parameter "float" die Zeitdauer (in Sekunden) seit dem letzten Frame gibt an, welche wir verwenden, um die zeitbasierte Bewegung zu implementieren. Seit dem Wert in Sekunden gemessen wird, und klicken Sie dann unsere Geschwindigkeitswerte darstellen, die Verschiebung im *Pixel pro Sekunde*.


## <a name="adding-bullets-to-gamelayer"></a>Hinzufügen von Aufzählungszeichen zu GameLayer

Bevor wir jeden hinzufügen `Bullet` -Instanzen, die unsere Spiel wird stellen wir einen Container, insbesondere eine `List<Bullet>`. Ändern der `GameLayer` damit es sich um eine Liste mit Aufzählungszeichen einbezieht:


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

Als Nächstes benötigen wir zum Auffüllen der `Bullet` Liste. Die Logik zum Zeitpunkt der Erstellung einer `Bullet` enthalten sein soll, der `Ship` Entität, aber die `GameLayer` ist verantwortlich für das Speichern der Liste der Aufzählungszeichen. Wir verwenden die Factorymuster, um ermöglichen die `Ship` zu erstellenden Entität `Bullet` Instanzen. Diese Factory wird ein Ereignis verfügbar zu machen, die die `GameLayer` behandeln können. 

Zu diesem Zweck zunächst, wir fügen einen Ordner zu unserem Projekt mit der Bezeichnung **Factorys**, und fügen Sie dann eine neue Klasse namens `BulletFactory`:

![](entities-images/image6.png "Das Projekt mit der Bezeichnung Factorys einen Ordner hinzu, und fügen Sie eine neue Klasse namens BulletFactory hinzu")

Als Nächstes implementieren wir die `BulletFactory` Singleton-Klasse:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

Die `Ship` Entität behandelt erstellen `Bullet` Instanzen – insbesondere behandelt es Häufigkeit `Bullet` Instanzen erstellt werden soll (d. h. wie oft das Aufzählungszeichen ausgelöst wird), ihre Position und die Geschwindigkeit.

Ändern der `Ship` Entität-Konstruktor zum Hinzufügen einer neuen `Schedule` aufrufen und implementieren Sie diese Methode wie folgt:


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

Im letzten Schritt wird die Erstellung neuer behandeln `Bullet` -Instanzen lautet in der `GameLayer` Code. Fügen Sie einen Ereignishandler an das `BulletCreated` Ereignis, das das neu erstellte fügt `Bullet` auf den entsprechenden Listen:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

Wir können jetzt das Spiel ausführen und finden Sie unter der `Ship` behandeln `Bullet` Instanzen:

![](entities-images/image1.png "Führen Sie das Spiel und der Liefernamen Aufzählungszeichen Instanzen behandeln sein wird")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>Warum verfügt über GameLayer liefern und Aufzählungszeichen Member

Unsere `GameLayer` Klasse definiert zwei Felder zum Verweisen auf Instanzen der Entität (`ship` und `bullets`), jedoch nicht mit ihnen. Darüber hinaus sind Entitäten für ihr eigenes Verhalten, z. B. Verschiebung und Behandeln von verantwortlich. Also warum wir hinzufügen `ship` und `bullets` Felder `GameLayer`?

Der Grund, die wir diese Member hinzugefügt ist, da eine komplette Spiele Implementierung Logik müsste die `GameLayer` für die Interaktion zwischen den verschiedenen Entitäten. Dieses Spiel kann z. B. weiter entwickelt werden, um Feinde einzuschließen, die vom Player zerstört werden können. Diese Feinde Verkaufswerten einer `List` in der `GameLayer`, und eine Logik zum Testen, ob `Bullet` Instanzen in Konflikt stehen, mit der Feinde durchgeführt werden, der `GameLayer` ebenfalls. Das heißt, die `GameLayer` ist der Stamm *Besitzer* der Entität, alle Instanzen und ist zuständig für die Interaktionen zwischen Instanzen der Entität.


## <a name="bullet-destruction-considerations"></a>Bullet Zerstörung Überlegungen

Unsere Spiel fehlt derzeit Code zerstören `Bullet` Instanzen. Jede `Bullet` Instanz verfügt über Logik für das Verschieben in einen Bildschirm, aber wir noch keine Code aus, um alle außerhalb des Bildschirms zerstören hinzugefügt `Bullet` Instanzen.

Darüber hinaus die Zerstörung des `Bullet` Instanzen möglicherweise nicht in gehören `GameLayer`. Z. B. anstelle eines zerstört wird, wenn außerhalb des Bildschirmbereichs befindet, die `Bullet` Entität möglicherweise Logik, selbst nach einem bestimmten Zeitraum zu zerstören. In diesem Fall die `Bullet` benötigt eine Möglichkeit, um mitzuteilen, dass er auf zerstört werden soll die `GameLayer`, ähnlich wie die `Ship` Entität mitgeteilt der `GameLayer` , ein neues `Bullet` erstellt wurde, über die `BulletFactory`.

Die einfachste Lösung besteht darin, die Verantwortung der Factoryklasse zur Zerstörung Unterstützung zu erweitern. Die Factory einer Entitätsinstanz zerstört wird, erhält kann von anderen Objekten wie z. B. behandelt werden können die `GameLayer` die Entitätsinstanz aus der Liste entfernen. 

## <a name="summary"></a>Zusammenfassung

Diese Anleitung zeigt, wie CocosSharp Entitäten durch Erben von der `CCNode` Klasse. Diese Entitäten sind eigenständige Objekte, die Behandlung von Erstellung ihrer eigenen visuelle Elemente und benutzerdefinierte Logik. Dieses Handbuch kennzeichnet Code, der innerhalb einer Entität (Bewegung und Erstellung von anderen Entitäten) gehört, aus Code, der in der Entität Stammcontainer (Kollisionen und andere Entität Interaktion-Logik) gehört.

## <a name="related-links"></a>Verwandte links

- [CocosSharp API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Inhalt zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
