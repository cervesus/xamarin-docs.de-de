---
title: Verwenden von CocosSharp in Xamarin.Forms
description: CocosSharp kann verwendet werden, um die genaue Form, Bild und Rendern von Text zu einer Anwendung für die erweiterte Visualisierung hinzufügen
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
ms.openlocfilehash: 8ba9e4b119384db401fc631f58c37a28cd2b8004
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724424"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Verwenden von CocosSharp in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_Cocossharp kann verwendet werden, um einer Anwendung eine exakte Form, ein Bild und ein Text Rendering für die erweiterte Visualisierung hinzuzufügen._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Entwickeln 2016: Cocos # in xamarin. Forms**

## <a name="overview"></a>Übersicht

CocosSharp ist eine flexible, leistungsstarke Technologie für die Anzeige von Grafiken, lesen die Touch-Punkts, Abspielen von Audio- und Verwalten von Inhalt an. Dieses Handbuch erklärt, wie eine Xamarin.Forms-Anwendung CocosSharp hinzugefügt. Es werden folgende Themen behandelt:

- [Was ist cocossharp?](#what)
- [Hinzufügen der cocossharp-nuget-Pakete](#nuget)
- [Exemplarische Vorgehensweise: Hinzufügen von cocossharp zu einer xamarin. Forms-App](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Was ist CocosSharp?

[Cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) ist eine Open-Source-Spiel-Engine, die auf der xamarin-Plattform verfügbar ist.
CocosSharp ist eine effiziente Common Language Runtime Bibliothek enthält die folgenden Funktionen:

- Bild Rendering mit der `CCSprite`-Klasse
- Rendern von Formen mithilfe der `CCDrawNode`-Klasse
- Jede Frame-Logik mit der `CCNode.Schedule`-Klasse
- Inhalts Verwaltung (laden und Entladen von Ressourcen wie PNG-Dateien) mithilfe der `CCTextureCache`
- Animationen mit der `CCAction`-Klasse

CocosSharp befasst sich hauptsächlich mit vereinfachen die Erstellung von plattformübergreifende 2D-Spiele; Es kann jedoch auch eine wunderbare Ergänzung zur Xamarin-Forms-Anwendungen. Da Spiele in der Regel effizientes Rendering und präzise Steuerung der visuellen Elemente erforderlich ist, kann CocosSharp verwendet werden, um leistungsstarke Visualisierung und Auswirkungen auf nicht-Game-Anwendungen hinzufügen.

Xamarin.Forms basiert auf systemeigene, plattformspezifische Benutzeroberflächen-Systeme die. [`Button`s](xref:Xamarin.Forms.Button) werden z. b. unter IOS und Android anders angezeigt und können sich je nach Betriebssystemversion unterscheiden. Im Gegensatz dazu CocosSharp keine visuellen Objekte plattformspezifische, verwendet werden alle visuellen Objekte auf allen Plattformen identisch zu sein scheinen. Natürlich, Auflösung und dem Seitenverhältnis unterscheiden sich zwischen Geräten und kann dadurch beeinträchtigt, wie die Visualisierungen in CocosSharp gerendert wird. Diese Informationen werden später in diesem Handbuch erläutert.

Ausführlichere Informationen finden Sie im Abschnitt " [cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)".

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Hinzufügen der cocossharp-nuget-Pakete

Vor dem Verwenden von CocosSharp, müssen Entwickler einige Ergänzungen zu ihrer Xamarin.Forms-Projekt vornehmen.
Dieses Handbuch setzt voraus, ein Xamarin.Forms-Projekt mit einem iOS, Android und .NET Standard Library-Projekt.
Der gesamte Code wird in der .NET Standard-Bibliotheksprojekts geschrieben werden. Allerdings müssen die Bibliotheken für die IOS- und Android-Projekte hinzugefügt werden.

Das nuget-Paket cocossharp enthält alle-Objekte, die zum Erstellen von cocossharp-Objekten erforderlich sind.
Das nuget-Paket cocossharp. Forms enthält die `CocosSharpView`-Klasse, die verwendet wird, um cocossharp in xamarin. Forms zu hosten.
Fügen Sie die **cocossharp. Forms** -nuget-und **cocossharp** -hinzu.
Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Pakete** im Projekt .NET Standard Bibliothek, und wählen Sie **Pakete hinzufügen...** aus. Geben Sie den Suchbegriff **cocossharp. Forms**ein, wählen Sie **cocossharp für xamarin. Forms aus**, und klicken Sie dann auf **Paket hinzufügen**.

![](cocossharp-images/image1.png "Add Packages Dialog")

Dem Projekt werden sowohl **cocossharp** -als auch **cocossharp. Forms** -nuget-Pakete hinzugefügt:

![](cocossharp-images/image2.png "Packages Folder")

Wiederholen Sie die oben genannten Schritte für plattformspezifische Projekte (z. B. iOS und Android) ein.

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Exemplarische Vorgehensweise: Hinzufügen von CocosSharp in einer Xamarin.Forms-app

Um eine einfache CocosSharp-Ansicht zu einer Xamarin.Forms-app hinzuzufügen, gehen Sie wie folgt vor:

1. [Erstellen einer xamarin Forms-Seite](#1)
1. [Hinzufügen einer cocossharpview](#2)
1. [Erstellen der gamescene](#3)
1. [Hinzufügen eines Kreises](#4)
1. [Interagieren mit cocossharp](#5)

Nachdem Sie einer xamarin. Forms-App erfolgreich eine cocossharp-Ansicht hinzugefügt haben, besuchen Sie die [cocossharp-Dokumentation](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) , um mehr über das Erstellen von Inhalten mit cocossharp zu erfahren.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Erstellen einer xamarin Forms-Seite

CocosSharp kann in jedem Xamarin.Forms-Container gehostet werden. In diesem Beispiel für diese Seite wird eine Seite mit dem Namen `HomePage`verwendet. `HomePage` wird in der Hälfte nach einem `Grid` aufgeteilt, um anzuzeigen, wie xamarin. Forms und cocossharp gleichzeitig auf der gleichen Seite gerendert werden können.

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

Unter IOS wird der `HomePage` wie in der folgenden Abbildung dargestellt angezeigt:

![](cocossharp-images/image3.png "HomePage Screenshot")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Hinzufügen einer cocossharpview

Die `CocosSharpView`-Klasse wird verwendet, um cocossharp in eine xamarin. Forms-App einzubetten. Da `CocosSharpView` von der [xamarin. Forms. View](xref:Xamarin.Forms.View) -Klasse erbt, stellt es eine vertraute Schnittstelle für das Layout bereit und kann in Layoutcontainern wie [xamarin. Forms. Grid](xref:Xamarin.Forms.Grid)verwendet werden. Fügen Sie dem Projekt eine neue `CocosSharpView` hinzu, indem Sie die `CreateTopHalf`-Methode abschließen:

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

Die cocossharp-Initialisierung ist nicht sofort. registrieren Sie daher ein Ereignis für den Fall, dass die `CocosSharpView` die Erstellung beendet hat. Gehen Sie dazu wie folgt `HandleViewCreated` vor:

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

Die `HandleViewCreated`-Methode hat zwei wichtige Details, die wir betrachten werden. Der erste ist die `GameScene`-Klasse, die im nächsten Abschnitt erstellt wird. Beachten Sie, dass die APP erst kompiliert wird, wenn die `GameScene` erstellt und der Verweis auf die `gameScene` Instanz aufgelöst wurde.

Das zweite wichtige Detail ist die `DesignResolution`-Eigenschaft, die den sichtbaren Bereich des Spiels für cocossharp-Objekte definiert. Die `DesignResolution`-Eigenschaft wird nach dem Erstellen `GameScene`untersucht.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Erstellen der gamescene

Die `GameScene`-Klasse erbt von der `CCScene`von cocossharp. `GameScene` ist der erste Punkt, an dem wir uns ausschließlich mit cocossharp befassen. Der in `GameScene` enthaltene Code funktioniert in jeder cocossharp-APP, unabhängig davon, ob er in einem xamarin. Forms-Projekt enthalten ist oder nicht.

Die `CCScene`-Klasse ist das visuelle Stamm Element des gesamten cocossharp-Rendering. Alle sichtbaren cocossharp-Objekte müssen in einer `CCScene`enthalten sein. Genauer gesagt müssen visuelle Objekte `CCLayer` Instanzen hinzugefügt werden, und diese `CCLayer` Instanzen müssen zu einem `CCScene`hinzugefügt werden.

Im folgende Diagramm können eine typische CocosSharp-Hierarchie zu visualisieren:

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

Es kann nur ein `CCScene` gleichzeitig aktiv sein. Die meisten Spiele verwenden mehrere `CCLayer` Instanzen, um Inhalt zu sortieren, aber unsere Anwendung verwendet nur einen. Auf ähnliche Weise die meisten Spiele verwenden mehrere visuelle Objekte, aber wir müssen nur eine in unserer app. Eine ausführlichere Erläuterung der visuellen Hierarchie von cocossharp finden Sie in der exemplarischen Vorgehensweise [bouncinggame](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md).

Anfänglich ist die `GameScene` Klasse nahezu leer – wir erstellen Sie einfach, um den Verweis in `HomePage`zu erfüllen. Fügen Sie Ihrem .NET Standard Bibliotheksprojekt mit dem Namen `GameScene`eine neue Klasse hinzu. Er sollte wie folgt von der `CCScene` Klasse erben:

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Nachdem `GameScene` definiert ist, können wir zu `HomePage` zurückkehren und ein Feld hinzufügen:

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Wir können nun unser Projekt zu kompilieren und ausführen, um mit CocosSharp finden Sie unter. Wir haben unserer `GameScene,` nichts hinzugefügt, sodass die obere Hälfte unserer Seite schwarz ist – die Standardfarbe einer cocossharp-Szene:

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Hinzufügen eines Kreises

Die APP verfügt zurzeit über eine laufende Instanz der cocossharp-Engine und zeigt eine leere `CCScene`an. Als Nächstes fügen wir ein visuelles Objekt: ein Kreis. Die `CCDrawNode`-Klasse kann verwendet werden, um eine Vielzahl von geometrischen Formen zu zeichnen, wie im [Leitfaden Zeichnen von Geometrie mit ccdrawnode](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)beschrieben.

Fügen Sie der `GameScene`-Klasse einen Kreis hinzu, und instanziieren Sie ihn im Konstruktor, wie im folgenden Code gezeigt:

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

Die app nun ausführen, zeigt einen Kreis auf der linken Seite des Anzeigebereichs CocosSharp:

![](cocossharp-images/image6.png "Circle in GameScene")

#### <a name="understanding-designresolution"></a>Grundlegendes zu DesignResolution

Nachdem nun ein Visual cocossharp-Objekt angezeigt wird, können Sie die `DesignResolution`-Eigenschaft untersuchen.

Der `DesignResolution` stellt die Breite und Höhe des cocossharp-Bereichs zum Platzieren und Anpassen von Objekten dar. Die tatsächliche Auflösung des Bereichs wird in *Pixel* gemessen, während die `DesignResolution` in Welt *Einheiten*gemessen wird. Das folgende Diagramm zeigt die Auflösung von verschiedenen Teilen der Ansicht auf ein iPhone 5 mit einer Auflösung von 640 x 1136 Pixeln angezeigt:

![](cocossharp-images/image7.png "iPhone 5s Design Resolution")

Das obige Diagramm zeigt die Pixeldimensionen außerhalb des Bildschirms als schwarzer Text. Einheiten werden innerhalb des Diagramms im weißen Text angezeigt. Hier sind einige wichtige Details über angezeigt:

- Der Ursprung der CocosSharp-Anzeige ist unten links. Rechts davon erhöht den X-Wert und eine Ebene weiter oben erhöht den Y-Wert. Beachten Sie, dass der Y-Wert umgekehrt wird im Vergleich zu einigen anderen 2D-Layout-Engines, in denen (0,0) wird oben links im Zeichenbereich.
- Das Standardverhalten von CocosSharp ist das Seitenverhältnis des seine Ansicht zu verwalten. Seit die erste Zeile im Raster breiter ist als hoch ist, wird CocosSharp die gesamte Breite der Zelle nicht aufgefüllt, wie der gepunktete weiße Rechteck dargestellt. Dieses Verhalten kann geändert werden, wie im [Leitfaden Behandlung mehrerer Auflösungen in cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)beschrieben.
- In diesem Beispiel wird die CocosSharp Seitenverhältnis des Geräts oder eines Anzeigebereichs dargestellt von 100 Einheiten breit und hoch, unabhängig von der Größe beibehalten. Dies bedeutet, dass Code, X = 100 stellt gebunden ganz rechts von der CocosSharp angezeigt werden, bleiben Layout auf allen Geräten einheitlich annehmen kann.

#### <a name="ccdrawnode-details"></a>CCDrawNode-Details

Unsere einfache APP verwendet die `CCDrawNode`-Klasse, um einen Kreis zu zeichnen. Diese Klasse kann für Unternehmen-apps sehr nützlich sein, da sie die Geometrie vektorbasierte Rendering – ein Feature fehlt in Xamarin.Forms bereitstellt. Zusätzlich zu den Kreisen kann die `CCDrawNode`-Klasse verwendet werden, um Rechtecke, Splines, Linien und benutzerdefinierte Polygone zu zeichnen. `CCDrawNode` ist auch einfach zu verwenden, da es nicht erforderlich ist, Bilddateien (z. b. png) zu verwenden. Eine ausführlichere Erläuterung von ccdrawnode finden Sie im [Handbuch Zeichnen von Geometrie mit ccdrawnode](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. interagieren mit cocossharp

Visuelle cocossharp-Elemente (z. b. `CCDrawNode`) erben von der `CCNode`-Klasse. `CCNode` bietet zwei Eigenschaften, die verwendet werden können, um ein Objekt relativ zum übergeordneten Element zu positionieren: `PositionX` und `PositionY`. Unser Code verwendet diese beiden Eigenschaften derzeit um den Mittelpunkt des Kreises, zu positionieren, wie im folgenden Codeausschnitt gezeigt:

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Es ist wichtig zu beachten, dass CocosSharp-Objekte von expliziten Positionswerten, im Gegensatz zu den meisten Xamarin.Forms-Ansichten können positioniert sind, die automatisch entsprechend das Verhalten ihrer übergeordneten Layout-Steuerelemente positioniert werden.

Wir fügen Code aus, um dem Benutzer ermöglichen, klicken Sie auf eines der zwei Schaltflächen, verschieben Sie den Kreis nach links oder rechts um 10 Einheiten (nicht in Pixel, da der Kreis im einheitsraum CocosSharp zeichnet). Zuerst erstellen wir zwei öffentliche Methoden in der `GameScene`-Klasse:

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

Als Nächstes fügen wir den beiden Schaltflächen in `HomePage` Handler hinzu, um auf Klicks zu antworten. Wenn der Vorgang abgeschlossen ist, enthält unsere `CreateBottomHalf`-Methode den folgenden Code:

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

Der CocosSharp-Kreis bewegt sich jetzt als Reaktion auf Klicks. Wir sehen außerdem klar die Grenzen des Zeichenbereichs CocosSharp, indem Sie den Kreis weit genug in links oder rechts verschieben:

![](cocossharp-images/image8.png "GameScene with Moving Circle")

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch veranschaulicht das Hinzufügen von CocosSharp in einer vorhandenen Xamarin.Forms-Projekt zum Erstellen der Interaktion zwischen Xamarin.Forms und CocosSharp, und behandelt die verschiedenen Überlegungen beim Erstellen von Layouts in CocosSharp.

Die CocosSharp-game-Engine bietet viele Funktionen und Tiefe, damit dieses Handbuch nur oberflächlich von CocosSharp Möglichkeiten. Entwickler, die mehr über cocossharp erfahren möchten, finden viele Artikel im [cocossharp-Archiv](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/).

## <a name="related-links"></a>Verwandte Links

- [Cocossharpforms (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
