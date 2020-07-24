---
title: Stapel Ansichten in xamarin. IOS
description: In diesem Artikel wird beschrieben, wie Sie das neue uistackview-Steuerelement in einer xamarin. IOS-App verwenden, um eine Reihe von unter Ansichten entweder in einem horizontal oder vertikal angeordneten Stapel zu verwalten.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: f38aee099cd2551ab2bdbaa94a8a3f9c0e1cf869
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939554"
---
# <a name="stack-views-in-xamarinios"></a>Stapel Ansichten in xamarin. IOS

_In diesem Artikel wird beschrieben, wie Sie das neue uistackview-Steuerelement in einer xamarin. IOS-App verwenden, um eine Reihe von unter Ansichten entweder in einem horizontal oder vertikal angeordneten Stapel zu verwalten._

> [!IMPORTANT]
> Beachten Sie, dass StackView im IOS-Designer unterstützt wird, wenn Sie den stabilen Kanal verwenden. Wenn Sie die Beta-oder Alpha Kanäle umschalten, sollte dieses Problem behoben werden. Wir haben uns entschieden, diese exemplarische Vorgehensweise mithilfe von Xcode zu präsentieren, bis die erforderlichen Korrekturen im stabilen Kanal implementiert werden.

Das Stapel Ansicht-Steuerelement ( `UIStackView` ) nutzt die Leistungsfähigkeit von automatischen layoutklassen und Größenklassen, um einen Stapel von unter Ansichten entweder horizontal oder vertikal zu verwalten, der dynamisch auf die Ausrichtung und Bildschirmgröße des IOS-Geräts antwortet.

Das Layout aller untergeordneten Sichten, die an eine Stapel Ansicht angefügt sind, wird von der Anwendung basierend auf vom Entwickler definierten Eigenschaften wie Achse, Verteilung, Ausrichtung und Abstand verwaltet:

[![Layoutdiagramm der Stapel Ansicht](uistackview-images/stacked01.png)](uistackview-images/stacked01.png#lightbox)

Wenn Sie `UIStackView` in einer xamarin. IOS-App verwenden, kann der Entwickler entweder die unter Ansichten entweder innerhalb eines Storyboards im IOS-Designer oder durch Hinzufügen und Entfernen von unter Sichten in c#-Code definieren.

Dieses Dokument besteht aus zwei Teilen: einem Schnellstart, mit dem Sie Ihre erste Stapel Ansicht implementieren können, und einigen weiteren technischen Details zur Funktionsweise.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**Uistackview-Video**

## <a name="uistackview-quickstart"></a>Uistackview-Schnellstart

Als schnelle Einführung in das `UIStackView` Steuerelement erstellen wir eine einfache Schnittstelle, die es dem Benutzer ermöglicht, eine Bewertung von 1 bis 5 einzugeben. Wir verwenden zwei Stapel Ansichten: eine, um die Schnittstelle vertikal auf dem Bildschirm des Geräts anzuordnen, und eine, um die 1-5-Bewertungs Symbole horizontal auf dem Bildschirm anzuordnen.

### <a name="define-the-ui"></a>Definieren der Benutzeroberfläche

Starten Sie ein neues xamarin. IOS-Projekt, und bearbeiten Sie die Datei " **Main. Storyboard** " in der Interface Builder von Xcode. Ziehen Sie zunächst eine einzelne **vertikale Stapel Ansicht** auf den **Ansichts Controller**:

[![Ziehen Sie eine einzelne vertikale Stapel Ansicht auf den Ansichts Controller.](uistackview-images/quick01.png)](uistackview-images/quick01.png#lightbox)

Legen Sie im **Attribut Inspektor**die folgenden Optionen fest:

[![Festlegen der Optionen für die Stapel Ansicht](uistackview-images/quick02.png)](uistackview-images/quick02.png#lightbox)

Hierbei gilt:

- **Axis** – bestimmt, ob die untergeordneten Sichten in der Stapel Ansicht entweder **Horizontal** oder **vertikal angeordnet**werden.
- **Alignment** – steuert, wie die untergeordneten Sichten innerhalb der Stapel Ansicht ausgerichtet werden.
- **Distribution** – steuert, wie die untergeordneten Sichten in der Stapel Ansicht skaliert werden.
- **Abstand** – steuert den minimalen Abstand zwischen jeder unter Ansicht in der Stapel Ansicht.
- **Relative Baseline** – wenn diese Option aktiviert ist, wird der vertikale Abstand der einzelnen untergeordneten Sichten von der Basislinie abgeleitet.
- **Layoutränder relativ** – platziert die untergeordneten Sichten in Relation zu den standardmäßigen Layouträndern.

Beim Arbeiten mit einer Stapel Ansicht können Sie sich die **Ausrichtung** als **X** -und **Y** -Position der untergeordneten Ansicht und der **Verteilung** als **Höhe** und **Breite**vorstellen.

> [!IMPORTANT]
> `UIStackView`ist als nicht Rendering-Container Ansicht konzipiert und wird daher nicht wie andere Unterklassen von in den Zeichenbereich gezeichnet `UIView` . Das Festlegen von Eigenschaften wie z `BackgroundColor` `DrawRect` . b. oder überschreiben hat also keinen visuellen Effekt.

Fahren Sie mit dem Layout der App-Schnittstelle fort, indem Sie eine Bezeichnung, eine ImageView, zwei Schaltflächen und eine horizontale Stapel Ansicht hinzufügen, sodass Sie der folgenden ähnelt:

[![Festlegen der Benutzeroberfläche der Stapel Ansicht](uistackview-images/quick03.png)](uistackview-images/quick03.png#lightbox)

Konfigurieren Sie die horizontale Stapel Ansicht mit den folgenden Optionen:

[![Konfigurieren der Optionen für die horizontale Stapel Ansicht](uistackview-images/quick04.png)](uistackview-images/quick04.png#lightbox)

Da das Symbol, das die einzelnen "Punkte" in der Bewertung darstellt, nicht gestreckt werden soll, wenn es der horizontalen Stapel Ansicht hinzugefügt wird, haben wir die **Ausrichtung** auf "zentriert" und die **Verteilung** so fest **gelegt, dass** Sie **gleichmäßig ausgefüllt**wird.

Richten Sie schließlich die folgenden **Outlets** und **Aktionen**ein:

[![Die Stapel Ansichts Outlets und Aktionen](uistackview-images/quick05.png)](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Auffüllen einer uistackview aus dem Code

Kehren Sie zu Visual Studio für Mac zurück, und bearbeiten Sie die Datei **ViewController.cs** , und fügen Sie folgenden Code hinzu:

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Werfen wir einen Blick auf einige Teile dieses Codes im Detail. Zuerst verwenden wir eine- `if` Anweisung, um zu überprüfen, ob es nicht mehr als fünf "Sterne" oder kleiner als 0 (null) ist.

Zum Hinzufügen eines neuen "Stars" laden wir das zugehörige Bild und legen den **Inhalts Modus** auf " **skalieren" anpassen**fest:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Dadurch wird das Symbol "Stern" nicht mehr verzerrt, wenn es der Stapel Ansicht hinzugefügt wird.

Als Nächstes fügen wir das neue "Star"-Symbol der Auflistung von untergeordneten Sichten der Stapel Ansicht hinzu:

```csharp
RatingView.AddArrangedSubview(icon);
```

Sie werden feststellen, dass der `UIImageView` `UIStackView` -Eigenschaft der- `ArrangedSubviews` Eigenschaft und nicht der-Eigenschaft hinzugefügt wurde `SubView` . Jede Ansicht, deren Layout in der Stapel Ansicht gesteuert werden soll, muss der-Eigenschaft hinzugefügt werden `ArrangedSubviews` .

Zum Entfernen einer unter Ansicht aus einer Stapel Ansicht wird zunächst die zu entfern gende unter Ansicht angezeigt:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Anschließend müssen wir Sie aus der Auflistung `ArrangedSubviews` und der Super Ansicht entfernen:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Wenn Sie eine unter Ansicht aus nur der Auflistung entfernen `ArrangedSubviews` , wird Sie aus dem Steuerelement der Stapel Ansicht entfernt, jedoch nicht vom Bildschirm entfernt.

### <a name="testing-the-ui"></a>Testen der Benutzeroberfläche

Wenn alle erforderlichen Benutzeroberflächen Elemente und Code vorhanden sind, können Sie die Schnittstelle jetzt ausführen und testen. Wenn die Benutzeroberfläche angezeigt wird, werden alle Elemente in der vertikalen Stapel Ansicht von oben nach unten gleichmäßig verteilt.

Wenn der Benutzer auf die Schaltfläche " **Bewertung erhöhen** " tippt, wird dem Bildschirm ein weiterer "Star" hinzugefügt (maximal 5):

[![Ausführen der Beispiel-App](uistackview-images/intro01.png)](uistackview-images/intro01.png#lightbox)

Die "Sterne" wird automatisch zentriert und in der horizontalen Stapel Ansicht gleichmäßig verteilt. Wenn der Benutzer auf die Schaltfläche " **Bewertung verringern** " tippt, wird ein "Star" entfernt (bis kein Wert mehr vorhanden ist).

## <a name="stack-view-details"></a>Details der Stapel Ansicht

Nun, da wir eine allgemeine Vorstellung davon haben, was das `UIStackView` Steuerelement ist und wie es funktioniert, sehen wir uns einige seiner Features und Details genauer an.

### <a name="auto-layout-and-size-classes"></a>Klassen für automatisches Layout und Größe

Wie bereits erwähnt, wird beim Hinzufügen einer unter Ansicht zu einer Stapel Ansicht das Layout vollständig von dieser Stapel Ansicht gesteuert, indem die Klassen für automatisches Layout und Größe verwendet werden, um die angeordneten Sichten zu positionieren und zu verkleinern.

In _der Stapel Ansicht wird die_ erste und letzte untergeordnete Sicht in der Auflistung an den **oberen** und **unteren** Rand für vertikale Stapel Ansichten oder den **linken** und **rechten** Rand für horizontale Stapel Ansichten angeheftet. Wenn Sie die- `LayoutMarginsRelativeArrangement` Eigenschaft auf festlegen `true` , werden die untergeordneten Sichten in der Ansicht an die relevanten Ränder anstatt an den Rand angeheftet.

Die Stapel Ansicht verwendet die-Eigenschaft der untergeordneten Ansicht `IntrinsicContentSize` beim Berechnen der untergeordneten Ansichts Größe entlang der definierten `Axis` (mit Ausnahme von `FillEqually Distribution` ). Die `FillEqually Distribution` ändert die Größe aller untergeordneten Sichten, sodass Sie die gleiche Größe haben. Dadurch wird die Stapel Ansicht entlang des aufgefüllt `Axis` .

Mit Ausnahme von wird in `Fill Alignment` der Stapel Ansicht die-Eigenschaft der unter Ansicht verwendet, `IntrinsicContentSize` um die Größe der Sicht senkrecht zum angegebenen zu berechnen `Axis` . Bei `Fill Alignment` sind alle untergeordneten Sichten so groß, dass Sie die Stapel Ansicht senkrecht zum angegebenen Auffüllen `Axis` .

### <a name="positioning-and-sizing-the-stack-view"></a>Positionieren und Anpassen der Stapel Ansicht

Die Stapel Ansicht hat die gesamte Kontrolle über das Layout einer beliebigen unter Ansicht (basierend auf Eigenschaften wie `Axis` und `Distribution` ), aber Sie müssen die Stapel Ansicht ( `UIStackView` ) in der übergeordneten Ansicht weiterhin mithilfe der Klassen für automatisches Layout und Größe positionieren.

Im Allgemeinen bedeutet dies, dass mindestens zwei Ränder der Stapel Ansicht angeheftet werden, um Sie zu erweitern und zu verkleinern und so die Position zu definieren. Ohne zusätzliche Einschränkungen wird die Stapel Ansicht automatisch so angepasst, dass Sie alle untergeordneten Sichten wie folgt einfügt:

- Die Größe der `Axis` untergeordneten Ansichts Größe und des zwischen den einzelnen untergeordneten Sichten definierten Speicherplatzes ist gleich.
- Wenn die- `LayoutMarginsRelativeArrangement` Eigenschaft ist `true` , enthalten die Stapel Ansichts Größe auch Platz für die Ränder.
- Die Größe, die senkrecht zum ist, `Axis` wird auf die größte unter Ansicht in der Auflistung festgelegt.

Darüber hinaus können Sie Einschränkungen für die **Höhe** und **Breite**der Stapel Ansicht angeben. In diesem Fall werden die untergeordneten Sichten angeordnet (mit der Größenangabe), um den von der Stapel Ansicht festgelegten Platz entsprechend der `Distribution` -Eigenschaft und der-Eigenschaft auszufüllen `Alignment` .

Wenn die- `BaselineRelativeArrangement` Eigenschaft `true` auf festgelegt ist, werden die untergeordneten Sichten basierend auf der Baseline der ersten oder letzten unter Ansicht angelegt, anstatt die **obere**, **untere** oder **zentrierte** -  **Y** -Position zu verwenden. Diese werden im Inhalt der Stapel Ansicht wie folgt berechnet:

- Eine vertikale Stapel Ansicht gibt die erste unter Ansicht für die erste Baseline und die letzte für den letzten zurück. Wenn eine dieser unter Sichten selbst Stapel Ansichten ist, wird Ihre erste oder letzte Baseline verwendet.
- In einer horizontalen Stapel Ansicht wird die höchste unter Ansicht für die erste und die letzte Baseline verwendet. Wenn die höchste Ansicht auch eine Stapel Ansicht ist, wird Sie mit der höchsten unter Ansicht als Baseline verwendet.

> [!IMPORTANT]
> Die Baseline-Ausrichtung funktioniert nicht bei gestreckten oder komprimierten untergeordneten Ansichts Größen, da die Baseline an die falsche Position berechnet wird. Stellen Sie bei der Baseline-Ausrichtung sicher, dass die **Höhe** der unter Ansicht mit der **Höhe**der intrinsischen Inhaltsansicht übereinstimmt.

### <a name="common-stack-view-uses"></a>Allgemeine Stapel Ansicht verwendet

Es gibt mehrere Layouttypen, die gut mit Stapel Ansicht-Steuerelementen funktionieren. Nachfolgend finden Sie einige der gängigeren Verwendungsmöglichkeiten:

- **Definieren Sie die Größe entlang der Achse** – indem Sie beide Ränder entlang der Stapel Ansicht `Axis` und einen der angrenzenden Ränder anheten, um die Position festzulegen, vergrößert sich die Stapel Ansicht entlang der Achse, damit Sie an den durch die untergeordneten Sichten definierten Platz passt.
- **Legen Sie die Position der untergeordneten Ansicht** fest – indem Sie an angrenzende Ränder der Stapel Ansicht an die übergeordnete Ansicht angehefteten, wird die Stapel Ansicht in beiden Dimensionen vergrößert, sodass Sie mit den untergeordneten Sichten verknüpft ist.
- **Definieren der Größe und Position des Stapels** – durch anheten aller vier Ränder der Stapel Ansicht an die übergeordnete Ansicht ordnet die Stapel Ansicht die unter Ansichten auf der Grundlage des in der Stapel Ansicht definierten Speicherplatzes an.
- **Definieren Sie die Größe der Achse senkrecht** – indem Sie beide Ränder senkrecht an die Stapel Ansicht `Axis` und einen der Kanten entlang der Achse anheken, um die Position festzulegen, vergrößert sich die Stapel Ansicht senkrecht zur Achse, damit Sie an den von den untergeordneten Sichten definierten Bereich angepasst wird.

### <a name="managing-the-appearance"></a>Verwalten des Erscheinungs Bilds

Der `UIStackView` ist als nicht Rendering-Container Ansicht konzipiert und wird daher nicht wie andere Unterklassen von in den Zeichenbereich gezeichnet `UIView` . Das Festlegen von Eigenschaften, z `BackgroundColor` . b. oder überschreiben, `DrawRect` hat keinen visuellen Effekt.

Es gibt mehrere Eigenschaften, mit denen gesteuert wird, wie eine Stapel Ansicht ihre Auflistung von untergeordneten Sichten anordnet:

- **Axis** – bestimmt, ob die untergeordneten Sichten in der Stapel Ansicht entweder **Horizontal** oder **vertikal angeordnet**werden.
- **Alignment** – steuert, wie die untergeordneten Sichten innerhalb der Stapel Ansicht ausgerichtet werden.
- **Distribution** – steuert, wie die untergeordneten Sichten in der Stapel Ansicht skaliert werden.
- **Abstand** – steuert den minimalen Abstand zwischen jeder unter Ansicht in der Stapel Ansicht.
- **Baseline relative** – Wenn `true` der Wert ist, wird der vertikale Abstand jeder unter Ansicht von der Basislinie abgeleitet.
- **Layoutränder relativ** – platziert die untergeordneten Sichten in Relation zu den standardmäßigen Layouträndern.

In der Regel wird eine Stapel Ansicht verwendet, um eine kleine Anzahl von unter Sichten anzuordnen. Komplexere Benutzeroberflächen können erstellt werden, indem eine oder mehrere Stapel Ansichten ineinander geschachtelt werden (wie im obigen [Schnellstart für uistackview](#uistackview-quickstart) beschrieben).

Sie können die Benutzeroberflächen Darstellung weiter optimieren, indem Sie den unter Ansichten zusätzliche Einschränkungen hinzufügen (z. b. um die Höhe oder Breite zu steuern). Allerdings sollte darauf geachtet werden, dass in Konflikt stehende Einschränkungen nicht zu den von der Stapel Ansicht selbst eingeführten Einschränkungen gehören.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Beibehalten der Konsistenz von angeordneten Ansichten und unter Ansichten

Die Stapel Ansicht stellt sicher, dass die- `ArrangedSubviews` Eigenschaft immer eine Teilmenge der zugehörigen-Eigenschaft ist. `Subviews` dabei werden die folgenden Regeln verwendet:

- Wenn der Auflistung eine unter Ansicht hinzugefügt wird `ArrangedSubviews` , wird Sie automatisch der-Auflistung hinzugefügt `Subviews` (sofern Sie nicht bereits Teil dieser Auflistung ist).
- Wenn eine untergeordnete Ansicht aus der Auflistung entfernt wird `Subviews` (aus der Anzeige entfernt), wird Sie auch aus der `ArrangedSubviews` Sammlung entfernt.
- Wenn Sie eine untergeordnete Sicht aus der Auflistung entfernen, `ArrangedSubviews` wird Sie nicht aus der Auflistung entfernt `Subviews` . Daher wird Sie nicht mehr von der Stapel Ansicht angelegt, sondern weiterhin auf dem Bildschirm angezeigt.

Die Auflistung `ArrangedSubviews` ist immer eine Teilmenge der Auflistung. `Subview` die Reihenfolge der einzelnen untergeordneten Sichten in den einzelnen Sammlungen ist jedoch getrennt und wird durch Folgendes gesteuert:

- Die Reihenfolge der untergeordneten Sichten in der Auflistung `ArrangedSubviews` bestimmt die Anzeigereihenfolge innerhalb des Stapels.
- Die Reihenfolge der untergeordneten Sichten in der Auflistung `Subview` bestimmt die Z-Reihenfolge (oder die Ebenenreihenfolge) innerhalb der Sicht zurück zum Vordergrund.

### <a name="dynamically-changing-content"></a>Dynamisches Ändern von Inhalten

Eine Stapel Ansicht passt automatisch das Layout der untergeordneten Sichten an, wenn eine unter Ansicht hinzugefügt, entfernt oder ausgeblendet wird. Das Layout wird auch angepasst, wenn eine Eigenschaft der Stapel Ansicht angepasst wird (z `Axis` . b. die).

Layoutänderungen können animiert werden, indem Sie in einem Animations Block platziert werden, z. b.:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Viele der Eigenschaften der Stapel Ansicht können mithilfe von Größenklassen innerhalb eines Storyboards angegeben werden. Diese Eigenschaften werden automatisch animiert, wenn die Größe oder die Ausrichtung geändert wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das neue `UIStackView` Steuerelement (IOS 9) behandelt, um eine Reihe von unter Ansichten entweder in einem horizontal oder vertikal angeordneten Stapel in einer xamarin. IOS-APP zu verwalten.
Es wurde ein einfaches Beispiel für die Verwendung von Stapel Ansichten verwendet, um eine Benutzeroberfläche zu erstellen, und mit einer ausführlicheren Betrachtung der Stapel Ansichten und ihrer Eigenschaften und Features.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uistackview-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Einführung in uistackview (Video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
