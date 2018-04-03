---
title: Verwenden von CocosSharp in Xamarin.Forms
description: CocosSharp kann verwendet werden, um präzise Form, Bild und Textrendering zu einer Anwendung für die erweiterte Visualisierung hinzuzufügen
ms.topic: article
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 83852b6b2d7324ae6aaf6b1dbf86a6ef7f9ac509
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>Verwenden von CocosSharp in Xamarin.Forms

_CocosSharp kann verwendet werden, um präzise Form, Bild und Textrendering zu einer Anwendung für die erweiterte Visualisierung hinzuzufügen_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Entwickeln 2016: Cocos # in Xamarin.Forms**

## <a name="overview"></a>Übersicht

CocosSharp ist eine flexible und leistungsstarke Technologie für die Anzeige von Grafiken, lesen die Fingereingabe, Wiedergeben von Audio-und verwalten. Dieses Handbuch erläutert, wie eine Anwendung Xamarin.Forms CocosSharp hinzugefügt. Es werden folgende Themen behandelt:

* [Was ist CocosSharp?](#what)
* [Hinzufügen von CocosSharp Nuget-Paketen](#nuget)
* [Exemplarische Vorgehensweise: Hinzufügen von CocosSharp mit einer Xamarin.Forms-app](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Was ist CocosSharp?

[CocosSharp](~/graphics-games/cocossharp/index.md) ist ein Spiel open-Source-Modul, das auf der Xamarin-Plattform verfügbar ist.
CocosSharp ist einer effizienten Common Language Runtime-Bibliothek umfasst die folgenden Funktionen:

* Mithilfe von Image-Rendering der [CCSprite-Klasse](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Mithilfe der Form Rendering der [CCDrawNode-Klasse](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Jeder Frame Ausführungslogik mit der [CCNode.Schedule-Methode](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* Inhaltsverwaltung (laden und Entladen von Ressourcen, z. B. PNG-Dateien) mit der [CCTextureCache-Klasse](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Animationen mithilfe der [CCAction-Klasse](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Erhalten Sie schwerpunktmäßig CocosSharp des ist vereinfachen das Erstellen von plattformübergreifenden 2D Spiele. Es kann jedoch auch eine hervorragende Ergänzung zu Xamarin-Forms-Anwendungen. Da Spiele in der Regel effizienter Rendering- und präzise Steuerung der visuellen Elemente erforderlich ist, kann CocosSharp verwendet werden, nicht Spiel Clientanwendungen leistungsfähige Visualisierung und Effekte hinzufügen.

Xamarin.Forms basiert auf systemeigenen, plattformspezifische UI-Systeme. Beispielsweise [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) auf IOS- und Android unterschiedlich angezeigt werden und möglicherweise sogar unterscheiden sich durch die Version des Betriebssystems. Dagegen wird CocosSharp alle plattformspezifische visuelle Objekte, nicht verwendet, daher alle visuellen Objekte angezeigt, die auf allen Plattformen identisch. Natürlich Auflösung und Seitenverhältnis unterscheiden sich zwischen den Geräten, und wie CocosSharp gerendert wird, dessen visuelle Elemente beeinträchtigt. Diese Details werden weiter unten in diesem Handbuch besprochen.

Ausführlichere Informationen finden Sie der [CocosSharp Abschnitt](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Hinzufügen von CocosSharp Nuget-Paketen

Vor der Verwendung CocosSharp, müssen Entwickler einige Zusätze zu ihrem Projekt Xamarin.Forms vornehmen.
Dieses Handbuch setzt voraus, eine Xamarin.Forms-Projekt für iOS, Android und PCL Projekt.
Der gesamte Code wird in der PCL-Projekt geschrieben werden. Allerdings müssen die Bibliotheken für den IOS- und Android-Projekte hinzugefügt werden.

Das CocosSharp Nuget-Paket enthält alle Objekte, die zum Erstellen von Objekten CocosSharp erforderlich.
Das CocosSharp.Forms NuGet-Paket enthält die `CocosSharpView` -Klasse, die zum Host CocosSharp in Xamarin.Forms verwendet wird.
Hinzufügen der **CocosSharp.Forms** NuGet und **CocosSharp** wird werden automatisch ebenfalls hinzugefügt.
Zu diesem Zweck mit der Maustaste, auf der PCL <span class="UIItem">Pakete</span> Ordner, und wählen <span class="UIItem">Pakete hinzufügen... </span>. Geben Sie den Suchbegriff <span class="UIItem">CocosSharp.Forms</span>Option <span class="UIItem">CocosSharp für Xamarin.Forms</span>, klicken Sie dann auf <span class="UIItem">Paket hinzufügen</span>.

![](cocossharp-images/image1.png "Pakete-Dialogfeld "hinzufügen"")

Beide **CocosSharp** und **CocosSharp.Forms** NuGet-Pakete werden dem Projekt hinzugefügt werden:

![](cocossharp-images/image2.png "Pakete (Ordner)")

Wiederholen Sie die oben genannten Schritte für plattformspezifischen Projekte (z. B. iOS und Android) aus.

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Exemplarische Vorgehensweise: Hinzufügen von CocosSharp mit einer Xamarin.Forms-app

Führen Sie diese Schritte, um eine einfache CocosSharp-Sicht mit einer Xamarin.Forms-app hinzuzufügen:

1. [Erstellen eine Xamarin Forms-Seite](#1)
1. [Hinzufügen einer CocosSharpView](#2)
1. [Erstellen die GameScene](#3)
1. [Hinzufügen eines Kreises](#4)
1. [Interaktion mit CocosSharp](#5)

Nachdem Sie erfolgreich eine CocosSharp-Sicht mit einer Xamarin.Forms-app hinzugefügt haben, besuchen Sie die [CocosSharp Dokumentation](~/graphics-games/cocossharp/index.md) Weitere Informationen zum Erstellen von Inhalten mit CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Erstellen eine Xamarin Forms-Seite

CocosSharp kann in jedem Container Xamarin.Forms gehostet werden. In diesem Beispiel für diese Seite wird eine Seite mit `HomePage`. `HomePage` wird in der Mitte von Teilen einer `Grid` soll zeigen, wie Sie Xamarin.Forms und CocosSharp gleichzeitig auf derselben Seite gerendert werden kann.

Richten Sie zuerst die Seite also es enthält einen `Grid` und zwei `Button` Instanzen:


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

Bei iOS kann die `HomePage` angezeigt wird, wie in der folgenden Abbildung gezeigt:

![](cocossharp-images/image3.png "Screenshot der HomePage")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Hinzufügen einer CocosSharpView

Die `CocosSharpView` Klasse wird verwendet, um CocosSharp in einer Xamarin.Forms-app einzubetten. Da `CocosSharpView` erbt von der [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) -Klasse, er bietet eine vertraute Schnittstelle für das Layout und können Sie z. B. innerhalb von Layoutcontainern verwendet werden [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Fügen Sie einen neuen `CocosSharpView` zum Projekt abschließen, indem die `CreateTopHalf` Methode:


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

CocosSharp Initialisierung erfolgt nicht sofort, registrieren Sie daher ein Ereignis für die Ausführung der `CocosSharpView` seiner Erstellung wurde beendet. Führen Sie dies in der `HandleViewCreated` Methode:


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

Die `HandleViewCreated` Methode verfügt über zwei wichtige Details, die von uns betrachtete werden müssen. Die erste ist die `GameScene` -Klasse, die im nächsten Abschnitt erstellt wird. Es ist wichtig zu beachten, dass die app nicht erst kompiliert, die `GameScene` wird erstellt und die `gameScene` Instanzenverweis aufgelöst wird.

Die zweite wichtige Details wird die `DesignResolution` Eigenschaft, die das Spiel sichtbaren Bereich für CocosSharp Objekte definiert. Die `DesignResolution` Eigenschaft wurde nach der Erstellung untersucht `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Erstellen die GameScene

Die `GameScene` Klasse erbt von der CocosSharp `CCScene`. `GameScene` ist der erste Punkt, in denen wir rein CocosSharp zu verarbeiten. Code in den `GameScene` funktioniert in einer beliebigen app CocosSharp, ob es in einem Projekt Xamarin.Forms oder nicht untergebracht ist.

Die `CCScene` Klasse ist der visuellen Stamm der gesamte CocosSharp Rendering. Jedes sichtbar CocosSharp-Objekt enthalten sein muss, innerhalb einer `CCScene`. Genauer gesagt, visueller Objekte müssen hinzugefügt werden `CCLayer` Instanzen und die `CCLayer` Instanzen müssen hinzugefügt werden, um eine `CCScene`.

Das folgende Diagramm können Sie eine typische CocosSharp Hierarchie visualisieren:

![](cocossharp-images/image4.png "Typische CocosSharp-Hierarchie")

Nur ein `CCScene` gleichzeitig aktiv sein können. Die meisten Spiele verwenden Sie mehrere `CCLayer` Instanzen zu sortieren Inhalt, aber die Anwendung verwendet nur einen. Auf ähnliche Weise die meisten Spiele mehrere visuelle Objekte verwenden, aber wir müssen nur einen in dieser app. Eine ausführlichere Erläuterung zu den CocosSharp visuellen Hierarchie Sie in finden der [BouncingGame Exemplarische Vorgehensweise](~/graphics-games/cocossharp/bouncing-game.md).

Zunächst die `GameScene` Klasse wird nahezu leer sein – wir einfach erstellen, um den Verweis in erfüllen `HomePage`. Fügen Sie eine neue Klasse, um Ihre Plc `GameScene`. Sie sollten erbt von der `CCScene` -Klasse wie folgt:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Nun, dass `GameScene` wird definiert, kann es zum Zurückgeben `HomePage` und fügen Sie ein Feld hinzu:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Wir können jetzt unsere Projekt zu kompilieren und führen Sie es zum CocosSharp ausführen finden Sie unter. Wir noch nicht hinzugefügt, nichts unsere `GameScene,` daher ist die obere Hälfte des unserer Seite Schwarz – die Standardfarbe CocosSharp themawechsel:

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Hinzufügen eines Kreises

Die app gerade besitzt eine ausgeführte Instanz des Datenbankmoduls CocosSharp, Anzeigen von ein leerer `CCScene`. Als Nächstes fügen wir ein visuelles Objekt: ein Kreis. Die `CCDrawNode` -Klasse kann eine Vielzahl von geometrische Formen gezeichnet werden soll verwendet werden, wie in der [Zeichnung Geometry mit CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

Hinzufügen ein Kreises zu unserem `GameScene` -Klasse und in den Konstruktor zu instanziieren, wie im folgenden Code gezeigt:


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

Ausführen der app nun wird einen Kreis auf der linken Seite des Anzeigebereichs CocosSharp gezeigt:

![](cocossharp-images/image6.png "Kreis in GameScene")


#### <a name="understanding-designresolution"></a>Grundlegendes zu DesignResolution

Nun, dass ein visuelles CocosSharp Objekt angezeigt wird, können wir untersuchen das `DesignResolution` Eigenschaft.

Die `DesignResolution` darstellt, die Breite und Höhe des Clientbereichs CocosSharp zum platzieren und Größe von Objekten. Die tatsächliche Auflösung des Bereichs wird in gemessen *Pixel* während der `DesignResolution` wird in der ganzen Welt gemessen *Einheiten*. Das folgende Diagramm zeigt die Auflösung von verschiedenen Teilen der Ansicht auf einem iPhone 5 mit einer Bildschirmauflösung von 640 x 1136 Pixel angezeigt:

![](cocossharp-images/image7.png "iPhone 5s Auflösung")

Das obige Diagramm zeigt Pixelmaßen an der Außenseite des Bildschirms als schwarzer Text. Einheiten werden innerhalb des Diagramms im weißen Text angezeigt. Hier sind einige wichtige Details über angezeigt:

* Der Ursprung der Anzeige CocosSharp ist unten links. Nach rechts verschieben erhöht den X-Wert und verschieben Sie den Y-Wert. Beachten Sie, dass der Y-Wert umgekehrt ist im Vergleich zu einigen anderen 2D Layoutmodule, wobei (0,0) wird von der oberen linken Ecke des Zeichenbereichs.
* Das Standardverhalten des CocosSharp ist das Seitenverhältnis des seinen Ansichtszustand zu verwalten. Da die erste Zeile im Raster breiter ist als hoch ist, ist CocosSharp die gesamte Breite der Zelle nicht aufgefüllt werden wie durch die gepunkteten weißen Rechteck dargestellt. Dieses Verhalten kann geändert werden, wie beschrieben in der [Behandeln mehrerer Lösungen im CocosSharp geführt](~/graphics-games/cocossharp/resolutions.md).
* In diesem Beispiel wird CocosSharp eines Anzeigebereichs dargestellt von 100 Einheiten Breite und Höhe unabhängig von der Größe oder das Seitenverhältnis des Geräts verwalten. Dies bedeutet, dass Code X = 100 stellt gebunden ganz rechts von der CocosSharp angezeigt werden, durch die Beibehaltung Layout konsistent auf allen Geräten annehmen kann.


#### <a name="ccdrawnode-details"></a>CCDrawNode-Details

Unsere einfache app verwendet die `CCDrawNode` -Klasse zum Zeichnen eines Kreises. Diese Klasse kann sehr nützlich für Branchen-apps sein, da sie vektorbasierte Geometrie Rendern – ein Feature in Xamarin.Forms fehlen bereitstellt. Zusätzlich zu den Kreise die `CCDrawNode` -Klasse zum Zeichnen von Rechtecken, Splines, Linien und Polygone benutzerdefinierte verwendet werden kann. `CCDrawNode` Dieser ist ist auch einfach zu verwenden, da es nicht, dass die Verwendung von Bilddateien (z. B. PNG erfordert). Eine ausführlichere Erläuterung der CCDrawNode finden Sie in der [Zeichnung Geometry mit CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Interaktion mit CocosSharp

CocosSharp visuelle Elemente (z. B. `CCDrawNode`) erben die `CCNode` Klasse. `CCNode` enthält zwei Eigenschaften, die verwendet werden können, um ein Objekt relativ zu seinem übergeordneten positionieren: `PositionX` und `PositionY`. Getesteten Codes verwendet diese beiden Eigenschaften derzeit um den Mittelpunkt des Kreises, zu positionieren, wie in diesem Codeausschnitt gezeigt:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Es ist wichtig zu beachten, dass CocosSharp Objekte von expliziten Positionswerten, im Gegensatz zu den meisten Xamarin.Forms Ansichten positioniert sind, die automatisch gemäß dem Verhalten ihrer übergeordneten Layout-Steuerelemente positioniert werden.

Wir fügen Code aus, um die Benutzer klicken Sie auf eines der zwei Schaltflächen, verschieben Sie den Kreis nach links oder rechts, um 10 Einheiten (nicht in Pixel, da der Kreis in die CocosSharp World einheitsraum zeichnet). Zunächst erstellen wir zwei öffentliche Methoden in der `GameScene` Klasse:


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Diese wird als Nächstes Handler hinzugefügt, um die beiden Schaltflächen in `HomePage` auf Klicks reagiert. Wenn Sie fertig sind, unsere `CreateBottomHalf` -Methode enthält den folgenden Code:


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Der Kreis CocosSharp wird jetzt als Antwort auf Klicks verschoben. Wir können die Grenzen des Zeichenbereichs CocosSharp auch klar erkennen, indem Sie den Kreis weit genug nach links oder rechts verschieben:

![](cocossharp-images/image8.png "GameScene Kreis verschieben")

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wird gezeigt, wie eine vorhandene Xamarin.Forms CocosSharp hinzuzufügende Projekt zum Erstellen der Interaktion zwischen Xamarin.Forms und CocosSharp, und erläutert verschiedene Überlegungen beim Erstellen von Layouts in CocosSharp.

Das Spiele CocosSharp-Modul bietet viele Funktionalität und Tiefe, damit diese Anleitung nur ausführlichen CocosSharp Möglichkeiten. Entwickler, die mehr über CocosSharp interessiert finde viele Artikel in der [CocosSharp Abschnitt](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Verwandte Links

- [CocosSharp-APIs](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
