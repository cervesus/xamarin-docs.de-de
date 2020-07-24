---
title: Verwenden von cocossharp inXamarin.Forms
description: Cocossharp kann verwendet werden, um einer Anwendung eine exakte Form, ein Bild und ein Text Rendering für die erweiterte Visualisierung hinzuzufügen.
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 31b82586b47ead1a851000d59c8271deec063020
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933782"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Verwenden von cocossharp inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_Cocossharp kann verwendet werden, um einer Anwendung eine exakte Form, ein Bild und ein Text Rendering für die erweiterte Visualisierung hinzuzufügen._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Weiterentwicklung 2016: Cocos # inXamarin.Forms**

## <a name="overview"></a>Übersicht

Cocossharp ist eine flexible, leistungsstarke Technologie für das Anzeigen von Grafiken, das Lesen von Berührungs Eingaben, das Abspielen von Audiodaten und das Verwalten von Inhalten. In diesem Handbuch wird erläutert, wie Sie cocossharp zu einer-Anwendung hinzufügen Xamarin.Forms .

## <a name="what-is-cocossharp"></a>Was ist cocossharp?

[Cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) ist eine Open-Source-Spiel-Engine, die auf der xamarin-Plattform verfügbar ist.
Cocossharp ist eine Lauf Zeiteffiziente Bibliothek, die die folgenden Features umfasst:

- Bild Rendering mithilfe der- `CCSprite` Klasse
- Rendern von Formen mithilfe der- `CCDrawNode` Klasse
- Jede Frame-Logik mit der- `CCNode.Schedule` Klasse
- Inhalts Verwaltung (laden und Entladen von Ressourcen wie PNG-Dateien) mithilfe der`CCTextureCache`
- Animationen mit der- `CCAction` Klasse

Der Hauptschwerpunkt von cocossharp besteht darin, die Erstellung von plattformübergreifenden 2D-Spielen zu vereinfachen. Es kann jedoch auch eine hervor artige Ergänzung zu xamarin-Formular Anwendungen sein. Da Spiele in der Regel ein effizientes Rendering und eine genaue Steuerung der visuellen Elemente erfordern, kann cocossharp verwendet werden, um leistungsstarke Visualisierungen und Effekte für nicht-Spielanwendungen hinzuzufügen.

Xamarin.Formsbasiert auf nativen, plattformspezifischen Benutzeroberflächen Systemen. Beispielsweise werden [ `Button` s](xref:Xamarin.Forms.Button) unter IOS und Android anders angezeigt und können sich je nach Betriebssystemversion unterscheiden. Im Gegensatz dazu verwendet cocossharp keine plattformspezifischen visuellen Objekte, sodass alle visuellen Objekte auf allen Plattformen identisch angezeigt werden. Natürlich unterscheiden sich die Auflösung und das Seitenverhältnis zwischen den Geräten, und dies kann sich darauf auswirken, wie cocossharp seine visuellen Elemente rendert. Diese Details werden später in diesem Handbuch erläutert.

Ausführlichere Informationen finden Sie im Abschnitt " [cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)".

## <a name="adding-the-cocossharp-nuget-packages"></a>Hinzufügen der cocossharp-nuget-Pakete

Vor der Verwendung von cocossharp müssen Entwickler an Ihrem Projekt einige Ergänzungen vornehmen Xamarin.Forms .
In diesem Handbuch wird davon ausgegangen, dass ein Xamarin.Forms Projekt mit einem IOS-, Android-und .NET Standard-Bibliotheksprojekt.
Der gesamte Code wird im .NET Standard Bibliotheksprojekt geschrieben. Bibliotheken müssen jedoch den IOS-und Android-Projekten hinzugefügt werden.

Das nuget-Paket cocossharp enthält alle-Objekte, die zum Erstellen von cocossharp-Objekten erforderlich sind.
Das nuget-Paket cocossharp. Forms enthält die- `CocosSharpView` Klasse, die verwendet wird, um cocossharp in zu hosten Xamarin.Forms .
Fügen Sie die **cocossharp. Forms** -nuget-und **cocossharp** -hinzu.
Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Pakete** im Projekt .NET Standard Bibliothek, und wählen Sie **Pakete hinzufügen...** aus. Geben Sie den Suchbegriff **cocossharp. Forms**ein, wählen Sie **cocossharp für aus Xamarin.Forms **, und klicken Sie dann auf **Paket hinzufügen**.

![Dialog Feld hinzufügen](cocossharp-images/image1.png)

Dem Projekt werden sowohl **cocossharp** -als auch **cocossharp. Forms** -nuget-Pakete hinzugefügt:

![Ordner "Pakete"](cocossharp-images/image2.png)

Wiederholen Sie die obigen Schritte für plattformspezifische Projekte (z. b. IOS und Android).

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Exemplarische Vorgehensweise: Hinzufügen von cocossharp zu einer Xamarin.Forms App

Führen Sie die folgenden Schritte aus, um einer App eine einfache cocossharp-Ansicht hinzuzufügen Xamarin.Forms :

1. [Erstellen einer xamarin Forms-Seite](#1-creating-a-xamarin-forms-page)
1. [Hinzufügen einer cocossharpview](#2-adding-a-cocossharpview)
1. [Erstellen der gamescene](#3-creating-the-gamescene)
1. [Hinzufügen eines Kreises](#4-adding-a-circle)
1. [Interagieren mit cocossharp](#5-interacting-with-cocossharp)

Nachdem Sie einer APP erfolgreich eine cocossharp-Ansicht hinzugefügt haben Xamarin.Forms , besuchen Sie die [cocossharp-Dokumentation](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) , um mehr über das Erstellen von Inhalten mit cocossharp zu erfahren.

### <a name="1-creating-a-xamarin-forms-page"></a>1. Erstellen einer xamarin Forms-Seite

Cocossharp kann in einem beliebigen Container gehostet werden Xamarin.Forms . In diesem Beispiel für diese Seite wird eine Seite mit dem Namen verwendet `HomePage` . `HomePage`wird in der Hälfte nach einem aufgeteilt `Grid` , um anzuzeigen, wie Xamarin.Forms und cocossharp gleichzeitig auf der gleichen Seite gerendert werden können.

Richten Sie zunächst die Seite so ein, dass Sie eine `Grid` und zwei `Button` Instanzen enthält:

```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

Unter IOS wird `HomePage` angezeigt, wie in der folgenden Abbildung dargestellt:

![Bildschirm Abbildung von Homepage](cocossharp-images/image3.png)

### <a name="2-adding-a-cocossharpview"></a>2. Hinzufügen einer cocossharpview

Die- `CocosSharpView` Klasse wird verwendet, um cocossharp in eine APP einzubetten Xamarin.Forms . Seit `CocosSharpView` erbt von [ Xamarin.Forms . View](xref:Xamarin.Forms.View) -Klasse, Sie stellt eine vertraute Schnittstelle für das Layout bereit und kann in Layoutcontainern wie verwendet werden [ Xamarin.Forms . Raster](xref:Xamarin.Forms.Grid). Fügen Sie dem Projekt ein neues hinzu `CocosSharpView` , indem Sie die- `CreateTopHalf` Methode abschließen:

```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

Die cocossharp-Initialisierung ist nicht sofort. registrieren Sie daher ein Ereignis für den Zeitpunkt, zu dem die `CocosSharpView` Erstellung des beendet wurde. Verwenden Sie hierzu die- `HandleViewCreated` Methode:

```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

Die- `HandleViewCreated` Methode verfügt über zwei wichtige Details, die wir betrachten werden. Der erste ist die- `GameScene` Klasse, die im nächsten Abschnitt erstellt wird. Beachten Sie, dass die APP erst kompiliert wird, wenn die `GameScene` erstellt und der `gameScene` Instanzverweis aufgelöst wurde.

Das zweite wichtige Detail ist die `DesignResolution` -Eigenschaft, die den sichtbaren Bereich des Spiels für cocossharp-Objekte definiert. Die- `DesignResolution` Eigenschaft wird nach dem Erstellen von untersucht `GameScene` .

### <a name="3-creating-the-gamescene"></a>3. Erstellen der gamescene

Die `GameScene` Klasse erbt von cocossharp `CCScene` . `GameScene`ist der erste Punkt, an dem wir uns ausschließlich mit cocossharp befassen. Der in enthaltene Code `GameScene` funktioniert in jeder cocossharp-APP, unabhängig davon, ob er in einem Projekt enthalten ist Xamarin.Forms oder nicht.

Die- `CCScene` Klasse ist das visuelle Stamm Element des gesamten cocossharp-Rendering. Alle sichtbaren cocossharp-Objekte müssen in einem enthalten sein `CCScene` . Genauer gesagt müssen visuelle Objekte zu-Instanzen hinzugefügt werden `CCLayer` , und diese `CCLayer` Instanzen müssen zu einem hinzugefügt werden `CCScene` .

Mithilfe des folgenden Diagramms kann eine typische cocossharp-Hierarchie visualisiert werden:

![Typische cocossharp-Hierarchie](cocossharp-images/image4.png)

Es kann jeweils nur eine `CCScene` aktiv sein. Die meisten Spiele verwenden mehrere- `CCLayer` Instanzen, um Inhalt zu sortieren, aber unsere Anwendung verwendet nur einen. Ebenso verwenden die meisten Spiele mehrere visuelle Objekte, aber wir verfügen nur über eine in unserer app. Eine ausführlichere Erläuterung der visuellen Hierarchie von cocossharp finden Sie in der exemplarischen Vorgehensweise [bouncinggame](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md).

Anfänglich ist die `GameScene` Klasse nahezu leer – wir erstellen Sie einfach, um den Verweis in zu erfüllen `HomePage` . Fügen Sie dem .NET Standard Bibliotheksprojekt eine neue Klasse mit dem Namen hinzu `GameScene` . Es sollte wie folgt von der- `CCScene` Klasse erben:

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Nun, das `GameScene` definiert ist, können wir zu zurückkehren `HomePage` und ein Feld hinzufügen:

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Wir können nun das Projekt kompilieren und ausführen, um die Ausführung von cocossharp anzuzeigen. Wir haben uns noch nichts hinzugefügt, `GameScene,` daher ist die obere Hälfte unserer Seite schwarz – die Standardfarbe einer cocossharp-Szene:

![Leeres gamescene](cocossharp-images/image5.png)

### <a name="4-adding-a-circle"></a>4. Hinzufügen eines Kreises

Die APP verfügt zurzeit über eine laufende Instanz der cocossharp-Engine, die einen leeren anzeigt `CCScene` . Als Nächstes fügen wir ein visuelles Objekt hinzu: einen Kreis. Die- `CCDrawNode` Klasse kann verwendet werden, um eine Vielzahl von geometrischen Formen zu zeichnen, wie im [Leitfaden Zeichnen von Geometrie mit ccdrawnode](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)beschrieben.

Fügen Sie der Klasse einen Kreis hinzu, `GameScene` und instanziieren Sie Sie im Konstruktor, wie im folgenden Code gezeigt:

```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Wenn Sie die app ausführen, wird jetzt ein Kreis auf der linken Seite des cocossharp-Anzeige Bereichs angezeigt:

![Kreis in gamescene](cocossharp-images/image6.png)

#### <a name="understanding-designresolution"></a>Grundlegendes zu Design Resolution

Nachdem nun ein Visual cocossharp-Objekt angezeigt wird, können wir die-Eigenschaft untersuchen `DesignResolution` .

Der `DesignResolution` stellt die Breite und Höhe des cocossharp-Bereichs zum Platzieren und Anpassen von Objekten dar. Die tatsächliche Auflösung des Bereichs wird in *Pixel* gemessen, während der `DesignResolution` in Welt *Einheiten*gemessen wird. Das folgende Diagramm zeigt die Auflösung verschiedener Teile der Ansicht, die auf einem iPhone 5 mit Bildschirmauflösung von 640 x 1136 Pixel angezeigt werden:

![Design Auflösung für iPhone 5S](cocossharp-images/image7.png)

Im obigen Diagramm werden Pixel Dimensionen auf der Außenseite des Bildschirms in schwarzem Text angezeigt. Einheiten werden im Inneren des Diagramms in weißem Text angezeigt. Hier sind einige wichtige Details aufgeführt, die oben angezeigt werden:

- Der Ursprung der cocossharp-Anzeige befindet sich unten links. Wenn Sie nach rechts verschieben, erhöht sich der X-Wert, und nach oben wird der Y-Wert vergrößert. Beachten Sie, dass der Y-Wert im Vergleich zu einigen anderen 2D-Layoutmodulen invertiert wird, wobei (0,0) der obere linke Bereich der Canvas ist.
- Das Standardverhalten von cocossharp besteht darin, das Seitenverhältnis der Ansicht beizubehalten. Da die erste Zeile im Raster breiter ist, als Sie hoch ist, wird die gesamte Breite der Zelle von cocossharp nicht aufgefüllt, wie durch das gepunktete weiße Rechteck dargestellt. Dieses Verhalten kann geändert werden, wie im [Leitfaden Behandlung mehrerer Auflösungen in cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)beschrieben.
- In diesem Beispiel behält cocossharp einen Anzeigebereich von 100 Einheiten breit und hoch, unabhängig von der Größe oder dem Seitenverhältnis des Geräts. Dies bedeutet, dass der Code davon ausgehen kann, dass X = 100 die Rechte gebundene Grenze der cocossharp-Anzeige darstellt, sodass das Layout auf allen Geräten konsistent bleibt.

#### <a name="ccdrawnode-details"></a>Ccdrawnode-Details

Unsere einfache APP verwendet die- `CCDrawNode` Klasse, um einen Kreis zu zeichnen. Diese Klasse kann für Business-Apps sehr nützlich sein, da Sie vektorbasiertes Geometrie Rendering bietet – eine Funktion, die in nicht vorhanden ist Xamarin.Forms . Zusätzlich zu den Kreisen kann die- `CCDrawNode` Klasse verwendet werden, um Rechtecke, Splines, Linien und benutzerdefinierte Polygone zu zeichnen. `CCDrawNode`kann auch leicht verwendet werden, da es nicht erforderlich ist, Bilddateien (z. b. png) zu verwenden. Eine ausführlichere Erläuterung von ccdrawnode finden Sie im [Handbuch Zeichnen von Geometrie mit ccdrawnode](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md).

### <a name="5-interacting-with-cocossharp"></a>5. interagieren mit cocossharp

Visuelle Elemente in cocossharp (z `CCDrawNode` . b.) erben von der- `CCNode` Klasse. `CCNode`stellt zwei Eigenschaften bereit, die verwendet werden können, um ein Objekt relativ zum übergeordneten Element zu positionieren: `PositionX` und `PositionY` . Der Code verwendet derzeit diese beiden Eigenschaften, um die Mitte des Kreises zu positionieren, wie in diesem Code Ausschnitt dargestellt:

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Es ist wichtig zu beachten, dass cocossharp-Objekte durch explizite Positionswerte positioniert werden, im Gegensatz zu den meisten Xamarin.Forms Ansichten, die automatisch entsprechend dem Verhalten ihrer übergeordneten Layoutsteuerelemente positioniert werden.

Wir fügen Code hinzu, um dem Benutzer die Möglichkeit zu geben, auf eine der beiden Schaltflächen zu klicken, um den Kreis um 10 Einheiten nach links oder nach rechts zu verschieben (nicht Pixel, da der Kreis den kocossharp-Raum für Welteinheiten zeichnet). Zuerst erstellen wir zwei öffentliche Methoden in der- `GameScene` Klasse:

```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Als Nächstes fügen wir die Handler den beiden Schaltflächen in hinzu, `HomePage` um auf Klicks zu antworten. Wenn der Vorgang abgeschlossen ist, `CreateBottomHalf` enthält die-Methode den folgenden Code:

```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Der cocossharp-Kreis wechselt nun in Reaktion auf Klicks. Wir können auch die Grenzen des cocossharp-Canvas erkennen, indem wir den Kreis weit genug nach links oder rechts bewegen:

![Gamescene mit beweglicher Kreis](cocossharp-images/image8.png)

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung wird gezeigt, wie Sie cocossharp einem vorhandenen Projekt hinzufügen Xamarin.Forms , wie Sie Interaktion zwischen Xamarin.Forms und cocossharp erstellen und verschiedene Überlegungen beim Erstellen von Layouts in cocossharp erläutern.

Die cocossharp-Spiel-Engine bietet viele Funktionen und Tiefe, sodass dieses Handbuch nur die Oberfläche von cocossharp unterstützt. Entwickler, die mehr über cocossharp erfahren möchten, finden viele Artikel im [cocossharp-Archiv](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/).

## <a name="related-links"></a>Verwandte Links

- [Cocossharpforms (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
