---
title: Entitäten in CocosSharp
description: Das Muster für die Entität ist eine leistungsfähige Möglichkeit zur Spiele-Code zu organisieren. Es verbessert die Lesbarkeit, wird Code einfacher zu verwalten und nutzt integrierte über-und untergeordneten Funktionen.
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 6445d595c9d8ca47e187fdcd158cd5a801a96407
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103204"
---
# <a name="entities-in-cocossharp"></a>Entitäten in CocosSharp

_Das Muster für die Entität ist eine leistungsfähige Möglichkeit zur Spiele-Code zu organisieren. Es verbessert die Lesbarkeit, wird Code einfacher zu verwalten und nutzt integrierte über-und untergeordneten Funktionen._

Das Muster für die Entität kann eines Entwicklers anstrengungen mit CocosSharp verbesserte Code-Organisation verbessert werden. In dieser exemplarischen Vorgehensweise wird ein praktisches Beispiel zeigt, wie zwei Entitäten: eine Ship-Entität und einer Aufzählung-Entität zu erstellen. Diese Entitäten werden eigenständige Objekte, was bedeutet, dass die einmal instanziiert sie automatisch gerendert wird, und führt die datenverschiebung Logik entsprechend ihrem Typ. 

Dieses Handbuch enthält die folgenden Themen:

 - Einführung in die Spiele-Entitäten
 - Allgemeine im Vergleich zu bestimmten Entitätstypen
 - Einrichtung des Projekts
 - Erstellen von Entitätsklassen
 - Hinzufügen von Instanzen der Entität, die `GameLayer`
 - Reagieren auf Entität Logik in der `GameLayer`

Das Spiel beendet wird wie folgt aussehen:

![](entities-images/image1.png "Das Spiel beendet sieht wie folgt aus.")


## <a name="introduction-to-game-entities"></a>Einführung in die Spiele-Entitäten

Spiele Entitäten sind Klassen, die Objekte, der Rendering "," Konflikt "," Physik "oder" künstliche Intelligenz Logik benötigen definieren. Zum Glück, entsprechen die Entitäten, die vorhanden Codebasis des Spiels häufig der konzeptionellen Objekte in einem Spiel. Wenn dies zutrifft, kann das Identifizieren der Entitäten in einem Spiel benötigt leichter erreicht werden. 

Angenommen, ein Leerzeichen, die mit Design [dafür mag Sie Arbeitsweise](http://en.wikipedia.org/wiki/Shoot_%27em_up) kann die folgenden Entitäten enthalten:

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

Diese Entitäten wäre ihre eigenen Klassen im Spiel, und jede Instanz wird wenig oder keinen Setup nach der Instanziierung erforderlich.


## <a name="general-vs-specific-entity-types"></a>Allgemeine im Vergleich zu bestimmten Entitätstypen

Einer der ersten Fragen konfrontiert von Spieleentwicklern, die über ein System für die Entität ist für die Entitäten zu generalisieren. Die spezifischste Implementierungen würden Klassen für jede Art von Entität definieren, auch wenn sie sich durch einige Merkmale unterscheiden wird. Weitere allgemeine Systeme werden Kombinieren von Gruppen von Entitäten in einer Klasse, und Instanzen, die angepasst werden.

Wir können z. B. eine weltraumspiel vorstellen, die die folgenden Klassen definiert:

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

Ein Ansatz mehr generalisierten wäre erstellen Sie eine einzelne Klasse für den Player umfasst und eine einzelne Klasse für gegnerischen ausgeliefert, die konfiguriert werden kann, um verschiedene Ship-Typen unterstützt. Anpassung eventuell welches Abbild zum Laden, welche Art von Aufzählungszeichen Entitäten erstellen, wenn die Verschiebung Koeffizienten, behandeln und AI-Logik für die Gegner geliefert wird. In diesem Fall kann die Liste der Entitäten auf reduziert werden:

 - `PlayerShip`
 - `EnemyShip`

Natürlich können diese Entitätstypen weiter von können Sie die Definition der pro Instanz für die Steuerung der Bewegung, verallgemeinert werden. Player-Ship-Instanzen würden aus dem Eingabedatenstrom lesen, während gegnerischen Ship Instanzen AI Logik ausführen können. Dies bedeutet, dass die Entitäten generalisiert werden, können auch in einer einzelnen Klasse weiter:

 - `Ship`

Die Generalisierung kann sogar noch weiter – weiterhin ein Spiel eine einzige Basisklasse für alle Entitäten verwenden kann. Diese einzelne Klasse, die aufgerufen werden kann `GameEntity`, würde die Klasse, die für jede Entitätsinstanz im gesamten Spiel verwendet werden, einschließlich der Entitäten, die nicht enthalten ist, z. B. Aufzählungszeichen und entladbare Elemente (leistungsschübe).

Dies die meisten Allgemein in Systeme wird häufig als bezeichnet ein *Komponente System*. In einem solchen System können spielen Entitäten einzelne Komponenten wie z. B. Physik, KI, oder Rendern Komponenten zum Anpassen des Aussehens und Verhaltens hinzugefügt. Reine komponentenbasierter Systeme aktivieren maximale Flexibilität und können durch die Verwendung der Vererbung, z. B. komplexe Vererbungskette verursachte Probleme vermeiden. Wie bei anderen verallgemeinerungen, effektive Komponente Systeme können schwierig sein, das Einrichten und können die ausdrucksfähigkeit der Code verringern.

Die Ebene der Generalisierung verwendet, hängt von viele Überlegungen zu berücksichtigen:

 - Spielgröße: können kleinere spielen bei der um-spezifische Klassen zu erstellen, während größere Spiele möglicherweise schwierig zu verwalten, mit einer großen Anzahl von Klassen.
 - Datengesteuerte-Entwicklung: Spiele, die auf Daten (Bildern, 3D-Modellen und Datendateien wie JSON oder XML) basieren können es vorteilhaft müssen generalisiert Entitätstypen, und Konfigurieren von den Besonderheiten, die auf Daten basieren. Dies ist vor allem Import für Spiele, die voraussichtlich neuen Inhalte hinzugefügt, während der Entwicklung oder nach der Freigabe des Spiels.
 - Spiele-Engine-Muster – dringend empfohlen, einige Spiele-Engines die Verwendung der Komponente Systeme, während andere Entwickler entscheiden, wie Sie Entitäten organisieren können. CocosSharp ist kein die Verwendung eines Systems Komponente erforderlich, damit Entwickler implementieren, jede Art von Entität sind. 

Der Einfachheit halber wird eine bestimmte Klassen basierende Methode mit einer einzelnen Entität der liefern und Aufzählungszeichen für dieses Tutorial verwendet.


## <a name="project-setup"></a>Einrichtung des Projekts

Bevor wir beginnen, die Entitäten zu implementieren, müssen wir ein Projekt zu erstellen. Wir werden die CocosSharp-Projektvorlagen verwenden, um projekterstellung zu vereinfachen. [Überprüfen Sie diesen Beitrag](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) Informationen zum Erstellen eines CocosSharp-Projekts von Visual Studio für Mac-Vorlagen. Der Rest dieses Handbuchs verwendet den Namen des Projekts **EntityProject**.

Nachdem das Projekt erstellt wurde, richten wir die Auflösung der unser Spiel mit 480 x 320 ausgeführt. Zu diesem Zweck rufen `CCScene.SetDefaultDesignResolution` in die `GameAppDelegate.ApplicationDidFinishLaunching` -Methode wie folgt:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

Weitere Informationen zum Umgang mit CocosSharp-Lösungen finden Sie unserem [Anleitung zur Behandlung mehrerer Auflösungen in CocosSharp](~/graphics-games/cocossharp/resolutions.md).


## <a name="adding-content-to-the-project"></a>Hinzufügen von Inhalt zum Projekt

Nachdem das Projekt erstellt wurde, fügt es in enthaltenen Dateien [diese Inhalte Zip-Datei](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true). Zu diesem Zweck herunterladen Sie die Zip-Datei, und Entzippen Sie sie. Fügen Sie beide **ship.png** und **bullet.png** auf die **Content** Ordner. Die **Content** Ordner werden innerhalb der **Assets** Ordner unter Android und im Stammverzeichnis des Projekts auf iOS. Nachdem Sie hinzugefügt haben, sehen Sie beide Dateien in die **Content** Ordner:

![](entities-images/image2.png "Nachdem Sie hinzugefügt haben, sollte beide Dateien im Ordner \"Content\" sein.")


## <a name="creating-the-ship-entity"></a>Erstellen der Entität versenden

Die `Ship` Klasse wird unser Spiel die erste Entität sein. Hinzufügen einer `Ship` Klasse, erstellen Sie zunächst einen Ordner namens **Entitäten** auf der Stammebene des Projekts. Fügen Sie eine neue Klasse in der **Entitäten** Ordner mit dem Namen `Ship`:

![](entities-images/image3.png "Fügen Sie eine neue Klasse im Ordner \"Entitäten\" wird aufgerufen, Versand")

Die erste Änderung, die wir zum Erstellen unserer `Ship` Klasse besteht darin, sie erben die `CCNode` Klasse. `CCNode` Dient als Basisklasse für allgemeine CocosSharp-Klassen wie `CCSprite` und `CCLayer`, haben wir die folgende Funktionen:

 - `Position` die Eigenschaft für die Lieferadresse auf dem Bildschirm zu verschieben.
 - `Children` -Eigenschaft für das Hinzufügen einer `CCSprite.`
 - `Parent` Eigenschaft, die verwendet werden kann, Anfügen `Ship` zu anderen Instanzen `CCNodes`. Es wird nicht diese Funktion in diesem Tutorial verwenden, werden; größere spielen häufig nutzen Anfügen von Entitäten in anderen `CCNodes`. 
 - `AddEventListener` Methode für das Reagieren auf eine Eingabe für die Lieferadresse verschieben.
 - `Schedule` die Methode zum Schießen Aufzählungszeichen.

Wir fügen auch eine `CCSprite` Instanz, sodass unsere Lief auf dem Bildschirm angezeigt werden:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

Als Nächstes fügen wir die Lieferadresse auf unsere `GameLayer` in unser Spiel angezeigt werden angezeigt:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

Wenn wir unser Spiel ausführen, sehen wir jetzt unsere Ship-Entität:

![](entities-images/image4.png "Wenn Sie das Spiel ausführen, wird die Ship-Entität angezeigt werden")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>Warum erben Sie CCNode statt CCSprite aus?

An diesem Punkt unsere `Ship` -Klasse ist ein einfacher Wrapper für eine `CCSprite` Instanz. Da `CCSprite` erbt auch von `CCNode`, konnten wir direkt aus geerbt `CCSprite`, der eingeschränkte würde des Codes in `Ship.cs`. Darüber hinaus erben direkt von `CCSprite` reduziert die Anzahl der Objekte im Arbeitsspeicher und verbessert die Leistung, indem die Abhängigkeitsstruktur kleiner zu machen.

Trotz dieser Vorteile, die wir von geerbten `CCNode` Ausblenden einiger der `CCSprite` Eigenschaften aus jeder Instanz. Z. B. die `Texture` Eigenschaft sollte nicht geändert werden, außerhalb von den `Ship` Klasse und das erben von `CCNode` blenden diese Eigenschaft ermöglicht. Die öffentlichen Member, der die Entitäten werden besonders wichtig, ein Spiel überschreitet und zusätzliche Entwickler zu einem Team hinzugefügt werden.


## <a name="adding-input-to-the-ship"></a>Eingabe für die Lieferadresse hinzufügen

Nun, da unsere Lief auf dem Bildschirm sichtbar ist werden wir Eingabe hinzufügen. Unser Ansatz werden ähnlich wie dieser Ansatz in der [BouncingGame-Handbuch](~/graphics-games/cocossharp/bouncing-game.md), außer dass wir den Code für das Verschieben von Daten in platziert werden, wird die `Ship` Klasse und nicht im mit `CCLayer` oder `CCScene`.

Fügen Sie den Code `Ship` , verschieben es, ganz egal, wo der Benutzer den Bildschirm berührt unterstützt:


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

Viele einschießen mag sich Spiele implementieren eine maximale Geschwindigkeit, traditionellen Controller-basierte Verlagerung imitiert. Dies bedeutet, dass wir sofort verschieben, um unsere kürzeren Code zu halten einfach implementieren.


## <a name="creating-the-bullet-entity"></a>Erstellen der Entität Aufzählungszeichen

Die zweite Entität in unser einfaches Spiel ist eine Entität für die Anzeige von Aufzählungszeichen. Ebenso wie die `Ship` Entität, die `Bullet` Entität enthält eine `CCSprite` , damit er auf dem Bildschirm angezeigt wird. Die Logik für das Verschieben von unterscheidet sich, da es sich nicht auf Benutzereingaben für das Verschieben von abhängt; stattdessen `Bullet` Instanzen werden in einer geraden Linie mit geschwindigkeitseigenschaften verschoben.

Zunächst fügen wir eine neue Klassendatei mit unserer **Entitäten** Ordner und nennen Sie sie **Aufzählungszeichen**:

![](entities-images/image5.png "Fügen Sie eine neue Klassendatei zu dem Ordner Entitäten hinzu, und nennen Sie es mit Aufzählungszeichen")

Nach hinzufügen, ändern wir die `Bullet.cs` code wie folgt:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

Abgesehen von der die Datei, die zum Ändern der `CCSprite` zu `bullet.png`, den Code in `ApplyVelocity` enthält die Bewegung Logik basierend auf zwei Koeffizienten: `VelocityX` und `VelocityY`.

Die `Schedule` Methode ermöglicht das Hinzufügen von Delegaten, die alle-Frame aufgerufen werden. In diesem Fall fügen wir die `ApplyVelocity` Methode, damit unsere Aufzählungszeichen gemäß seiner Geschwindigkeitswerte verschoben wird. Die `Schedule` -Methode übernimmt eine `Action<float>`, in dem der Parameter "float" die Zeitspanne (in Sekunden) seit der letzten Frame gibt an, welche wir verwenden, um die Bewegung eines zeitbasierten zu implementieren. Seit der Wert in Sekunden gemessen wird, und klicken Sie dann unsere Geschwindigkeitswerte darzustellen, Verschieben von Daten in *Pixeln pro Sekunde*.


## <a name="adding-bullets-to-gamelayer"></a>GameLayer Aufzählungszeichen hinzufügen

Vor dem Hinzufügen einer `Bullet` Instanzen, um unserem Spiel wir veranlasst einen Container, insbesondere eine `List<Bullet>`. Ändern der `GameLayer` sodass sie eine Liste mit Aufzählungszeichen enthält:


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

Als Nächstes müssen wir zum Auffüllen der `Bullet` Liste. Die Logik zum Zeitpunkt der Erstellung einer `Bullet` enthalten sein soll, der `Ship` Entität, aber die `GameLayer` ist verantwortlich für das Speichern der Liste der Aufzählungszeichen. Wir verwenden das Factorymuster, um ermöglichen die `Ship` zu erstellenden Entität `Bullet` Instanzen. Diese Factory macht ein Ereignis verfügbar, die die `GameLayer` verarbeiten kann. 

Zu diesem Zweck zuerst hinzugefügt einen Ordner unser Projekt mit dem Namen **Factorys**, und fügen Sie dann eine neue Klasse namens `BulletFactory`:

![](entities-images/image6.png "Fügen Sie einen Ordner mit dem Namen Factorys, und fügen Sie dann eine neue Klasse namens BulletFactory hinzu")

Als Nächstes implementieren wir die `BulletFactory` Singleton-Klasse:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

Die `Ship` Entität behandelt wird, Erstellen von `Bullet` Instanzen – insbesondere behandelt es Häufigkeit `Bullet` Instanzen erstellt werden soll (d. h. wie oft das Aufzählungszeichen ausgelöst wird), ihre Position und die Geschwindigkeit.

Ändern der `Ship` Entitätskonstruktor zum Hinzufügen einer neuen `Schedule` aufrufen, und klicken Sie dann implementieren Sie diese Methode wie folgt:


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

Der letzte Schritt ist die Erstellung von neuen behandelt `Bullet` -Instanzen in der `GameLayer` Code. Hinzufügen eines ereignishandlers, um die `BulletCreated` -Ereignis, das das neu erstellte fügt `Bullet` auf den entsprechenden Listen:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

Nun können wir das Spiel ausführen und finden Sie unter den `Ship` Schießen `Bullet` Instanzen:

![](entities-images/image1.png "Führen Sie das Spiel und die Lieferadresse wird Schießen, wenn Sie Instanzen mit Aufzählungszeichen")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>Warum hat GameLayer liefern und Aufzählungszeichen Mitglieder

Unsere `GameLayer` -Klasse definiert zwei Felder zum Speichern von Verweisen auf Instanzen der Entität (`ship` und `bullets`), jedoch nicht mit ihnen. Darüber hinaus sind die Entitäten für ihr eigenes Verhalten wie z. B. das Verschieben und Schießen verantwortlich. Also warum wir hinzufügen `ship` und `bullets` Felder `GameLayer`?

Wir haben diese Member hinzugefügt ist, weil eine komplette Spiele-Implementierung Logik müsste die `GameLayer` für die Interaktion zwischen den verschiedenen Entitäten. Dieses Spiel kann z. B. weiter entwickelt werden, um Feinde einzuschließen, die durch den Player zerstört werden kann. Diese Feinde Verkaufswerten eine `List` in der `GameLayer`, und Logik, um zu testen, ob `Bullet` Instanzen in Konflikt mit Feinde ausgeführt werden würde die `GameLayer` auch. Das heißt, die `GameLayer` ist der Stamm *Besitzer* alle Entität Instanzen, und er ist verantwortlich für Interaktionen zwischen Instanzen der Entität.


## <a name="bullet-destruction-considerations"></a>Überlegungen zur Zerstörung der Aufzählungszeichen

Unser Spiel fehlt momentan der Code für das Löschen von `Bullet` Instanzen. Jede `Bullet` Instanz verfügt über eine Logik zum Verschieben von auf dem Bildschirm, aber wir noch nicht hinzugefügt, beliebigen Code, um alle außerhalb des Bildschirms zerstören `Bullet` Instanzen.

Darüber hinaus die Zerstörung des `Bullet` Instanzen möglicherweise nicht gehören `GameLayer`. Beispiel: anstatt wird zerstört, wenn außerhalb des Bildschirms, der `Bullet` Entität möglicherweise Logik selbst nach einem bestimmten Zeitraum zu zerstören. In diesem Fall die `Bullet` benötigt eine Möglichkeit, die darüber informieren, dass es auf zerstört werden soll die `GameLayer`, ähnlich wie die `Ship` Entität übermittelt die `GameLayer` , ein neues `Bullet` erstellt wurde, über die `BulletFactory`.

Die einfachste Lösung ist die Verantwortung für die Klasse zur Unterstützung der Zerstörung zu erweitern. Und dann die Factory eine Entitätsinstanz, die zerstört wird, benachrichtigt werden kann, das von anderen Objekten, z. B. behandelt werden kann die `GameLayer` entfernen die Entitätsinstanz aus seiner Listen. 

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wird gezeigt, wie CocosSharp-Entitäten zu erstellen, durch Erben von der `CCNode` Klasse. Diese Entitäten sind eigenständige Objekte, die Erstellung von eigenen visuellen Elemente und benutzerdefinierte Logik verarbeiten. Dieses Handbuch gibt Code, der innerhalb einer Entität (datenverschiebung und die Erstellung von anderen Entitäten) gehört, aus Code, der in der Entität Stammcontainer (Kollisionen und andere Entität Interaktionslogik) gehört.

## <a name="related-links"></a>Verwandte Links

- [CocosSharp-API-Dokumentation](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Inhalt der zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
