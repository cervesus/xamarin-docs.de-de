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
ms.openlocfilehash: b4a8507d4d1497964f6b60307622ca3e1dc4cd90
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021796"
---
# <a name="stack-views-in-xamarinios"></a>Stapel Ansichten in xamarin. IOS

_In diesem Artikel wird beschrieben, wie Sie das neue uistackview-Steuerelement in einer xamarin. IOS-App verwenden, um eine Reihe von unter Ansichten entweder in einem horizontal oder vertikal angeordneten Stapel zu verwalten._

> [!IMPORTANT]
> Beachten Sie, dass StackView im IOS-Designer unterstützt wird, wenn Sie den stabilen Kanal verwenden. Wenn Sie die Beta-oder Alpha Kanäle umschalten, sollte dieses Problem behoben werden. Wir haben uns entschieden, diese exemplarische Vorgehensweise mithilfe von Xcode zu präsentieren, bis die erforderlichen Korrekturen im stabilen Kanal implementiert werden.

Das Stapel Ansichts Steuerelement (`UIStackView`) nutzt die Leistungsfähigkeit von Klassen für automatisches Layout und Größe, um einen Stapel von untergeordneten Sichten entweder horizontal oder vertikal zu verwalten, der dynamisch auf die Ausrichtung und Bildschirmgröße des IOS-Geräts antwortet.

Das Layout aller untergeordneten Sichten, die an eine Stapel Ansicht angefügt sind, wird von der Anwendung basierend auf vom Entwickler definierten Eigenschaften wie Achse, Verteilung, Ausrichtung und Abstand verwaltet:

[![](uistackview-images/stacked01.png "Stack View layout diagram")](uistackview-images/stacked01.png#lightbox)

Wenn Sie eine `UIStackView` in einer xamarin. IOS-App verwenden, kann der Entwickler entweder die unter Ansichten entweder innerhalb eines Storyboards im IOS-Designer oder durch Hinzufügen und Entfernen von unter C# Ansichten im Code definieren.

Dieses Dokument besteht aus zwei Teilen: einem Schnellstart, mit dem Sie Ihre erste Stapel Ansicht implementieren können, und einigen weiteren technischen Details zur Funktionsweise.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**Uistackview-Video**

## <a name="uistackview-quickstart"></a>Uistackview-Schnellstart

Als schnelle Einführung in das `UIStackView`-Steuerelement erstellen wir eine einfache Schnittstelle, die es dem Benutzer ermöglicht, eine Bewertung von 1 bis 5 einzugeben. Wir verwenden zwei Stapel Ansichten: eine, um die Schnittstelle vertikal auf dem Bildschirm des Geräts anzuordnen, und eine, um die 1-5-Bewertungs Symbole horizontal auf dem Bildschirm anzuordnen.

### <a name="define-the-ui"></a>Definieren der Benutzeroberfläche

Starten Sie ein neues xamarin. IOS-Projekt, und bearbeiten Sie die Datei " **Main. Storyboard** " in der Interface Builder von Xcode. Ziehen Sie zunächst eine einzelne **vertikale Stapel Ansicht** auf den **Ansichts Controller**:

[![](uistackview-images/quick01.png "Drag a single Vertical Stack View on the View Controller")](uistackview-images/quick01.png#lightbox)

Legen Sie im **Attribut Inspektor**die folgenden Optionen fest:

[![](uistackview-images/quick02.png "Set the Stack View options")](uistackview-images/quick02.png#lightbox)

Ort:

- **Axis** – bestimmt, ob die untergeordneten Sichten in der Stapel Ansicht entweder **Horizontal** oder **vertikal angeordnet**werden.
- **Alignment** – steuert, wie die untergeordneten Sichten innerhalb der Stapel Ansicht ausgerichtet werden.
- **Distribution** – steuert, wie die untergeordneten Sichten in der Stapel Ansicht skaliert werden.
- **Abstand** – steuert den minimalen Abstand zwischen jeder unter Ansicht in der Stapel Ansicht.
- **Relative Baseline** – wenn diese Option aktiviert ist, wird der vertikale Abstand der einzelnen untergeordneten Sichten von der Basislinie abgeleitet.
- **Layoutränder relativ** – platziert die untergeordneten Sichten in Relation zu den standardmäßigen Layouträndern.

Beim Arbeiten mit einer Stapel Ansicht können Sie sich die **Ausrichtung** als **X** -und **Y** -Position der untergeordneten Ansicht und der **Verteilung** als **Höhe** und **Breite**vorstellen.

> [!IMPORTANT]
> `UIStackView` ist als nicht renderingcontaineransicht konzipiert und wird daher nicht wie andere Unterklassen von `UIView`auf den Zeichenbereich gezeichnet. Das Festlegen von Eigenschaften, z. b. `BackgroundColor` oder überschreiben `DrawRect`, hat also keinen visuellen Effekt.

Fahren Sie mit dem Layout der App-Schnittstelle fort, indem Sie eine Bezeichnung, eine ImageView, zwei Schaltflächen und eine horizontale Stapel Ansicht hinzufügen, sodass Sie der folgenden ähnelt:

[![](uistackview-images/quick03.png "Laying out the Stack View UI")](uistackview-images/quick03.png#lightbox)

Konfigurieren Sie die horizontale Stapel Ansicht mit den folgenden Optionen:

[![](uistackview-images/quick04.png "Configure the Horizontal Stack View options")](uistackview-images/quick04.png#lightbox)

Da das Symbol, das die einzelnen "Punkte" in der Bewertung darstellt, nicht gestreckt werden soll, wenn es der horizontalen Stapel Ansicht hinzugefügt wird, haben wir die **Ausrichtung** auf "zentriert" und die **Verteilung** so fest **gelegt, dass** Sie **gleichmäßig ausgefüllt**wird.

Richten Sie schließlich die folgenden **Outlets** und **Aktionen**ein:

[![](uistackview-images/quick05.png "The Stack View Outlets and Actions")](uistackview-images/quick05.png#lightbox)

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

Werfen wir einen Blick auf einige Teile dieses Codes im Detail. Zuerst verwenden wir eine `if`-Anweisung, um zu überprüfen, ob es nicht mehr als fünf "Sterne" oder kleiner als 0 (null) ist.

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

Sie werden feststellen, dass die `UIImageView` der `ArrangedSubviews`-Eigenschaft `UIStackView`und nicht dem `SubView`hinzugefügt wurde. Jede Ansicht, die der Stapel Ansicht das Layout Steuern soll, muss der `ArrangedSubviews`-Eigenschaft hinzugefügt werden.

Zum Entfernen einer unter Ansicht aus einer Stapel Ansicht wird zunächst die zu entfern gende unter Ansicht angezeigt:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Anschließend müssen wir Sie sowohl aus der `ArrangedSubviews` Auflistung als auch aus der Super Ansicht entfernen:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Wenn Sie eine unter Ansicht nur aus der `ArrangedSubviews` Auflistung entfernen, wird Sie aus dem Steuerelement der Stapel Ansicht entfernt, jedoch nicht vom Bildschirm entfernt.

### <a name="testing-the-ui"></a>Testen der Benutzeroberfläche

Wenn alle erforderlichen Benutzeroberflächen Elemente und Code vorhanden sind, können Sie die Schnittstelle jetzt ausführen und testen. Wenn die Benutzeroberfläche angezeigt wird, werden alle Elemente in der vertikalen Stapel Ansicht von oben nach unten gleichmäßig verteilt.

Wenn der Benutzer auf die Schaltfläche " **Bewertung erhöhen** " tippt, wird dem Bildschirm ein weiterer "Star" hinzugefügt (maximal 5):

[![](uistackview-images/intro01.png "The sample app run")](uistackview-images/intro01.png#lightbox)

Die "Sterne" wird automatisch zentriert und in der horizontalen Stapel Ansicht gleichmäßig verteilt. Wenn der Benutzer auf die Schaltfläche " **Bewertung verringern** " tippt, wird ein "Star" entfernt (bis kein Wert mehr vorhanden ist).

## <a name="stack-view-details"></a>Details der Stapel Ansicht

Nun, da wir eine allgemeine Vorstellung davon haben, was das `UIStackView`-Steuerelement ist und wie es funktioniert, sehen wir uns einige Features und Details genauer an.

### <a name="auto-layout-and-size-classes"></a>Klassen für automatisches Layout und Größe

Wie bereits erwähnt, wird beim Hinzufügen einer unter Ansicht zu einer Stapel Ansicht das Layout vollständig von dieser Stapel Ansicht gesteuert, indem die Klassen für automatisches Layout und Größe verwendet werden, um die angeordneten Sichten zu positionieren und zu verkleinern.

In _der Stapel Ansicht wird die_ erste und letzte untergeordnete Sicht in der Auflistung an den **oberen** und **unteren** Rand für vertikale Stapel Ansichten oder den **linken** und **rechten** Rand für horizontale Stapel Ansichten angeheftet. Wenn Sie die `LayoutMarginsRelativeArrangement`-Eigenschaft auf `true`festlegen, werden die unter Ansichten in der Ansicht an die relevanten Ränder anstatt an den Rand angeheftet.

In der Stapel Ansicht wird die `IntrinsicContentSize`-Eigenschaft der untergeordneten Ansicht verwendet, wenn die Größe der untergeordneten Sichten entlang der definierten `Axis` (mit Ausnahme des `FillEqually Distribution`) berechnet wird. Der `FillEqually Distribution` ändert die Größe aller untergeordneten Sichten, sodass Sie die gleiche Größe haben. Dadurch wird die Stapel Ansicht entlang des `Axis`aufgefüllt.

Mit Ausnahme der `Fill Alignment`verwendet die Stapel Ansicht die `IntrinsicContentSize`-Eigenschaft der unter Ansicht, um die Größe der Sicht senkrecht zum angegebenen `Axis`zu berechnen. Für das `Fill Alignment`werden alle untergeordneten Sichten so groß, dass Sie die Stapel Ansicht senkrecht zum angegebenen `Axis`ausfüllen.

### <a name="positioning-and-sizing-the-stack-view"></a>Positionieren und Anpassen der Stapel Ansicht

Die Stapel Ansicht hat die gesamte Kontrolle über das Layout einer beliebigen unter Ansicht (basierend auf Eigenschaften wie `Axis` und `Distribution`), aber Sie müssen die Stapel Ansicht (`UIStackView`) in der übergeordneten Ansicht weiterhin mithilfe der Klassen für automatisches Layout und Größe positionieren.

Im Allgemeinen bedeutet dies, dass mindestens zwei Ränder der Stapel Ansicht angeheftet werden, um Sie zu erweitern und zu verkleinern und so die Position zu definieren. Ohne zusätzliche Einschränkungen wird die Stapel Ansicht automatisch so angepasst, dass Sie alle untergeordneten Sichten wie folgt einfügt:

- Die Größe des `Axis` ist die Summe aller untergeordneten Ansichts Größen zuzüglich des zwischen den einzelnen untergeordneten Sichten definierten Speicherplatzes.
- Wenn die `LayoutMarginsRelativeArrangement`-Eigenschaft `true`ist, enthalten die Stapel Ansichts Größe auch Platz für die Ränder.
- Die Größe, die senkrecht zum `Axis` ist, wird auf die größte unter Ansicht in der Auflistung festgelegt.

Darüber hinaus können Sie Einschränkungen für die **Höhe** und **Breite**der Stapel Ansicht angeben. In diesem Fall werden die untergeordneten Sichten angelegt (mit Größenangabe), um den von der Stapel Ansicht festgelegten Platz entsprechend den Eigenschaften `Distribution` und `Alignment` auszufüllen.

Wenn die `BaselineRelativeArrangement`-Eigenschaft `true`ist, werden die untergeordneten Sichten basierend auf der Baseline der ersten oder letzten untergeordneten Ansicht angelegt, anstatt die **obere**, **untere** oder **zentrierte**- **Y** -Position zu verwenden. Diese werden im Inhalt der Stapel Ansicht wie folgt berechnet:

- Eine vertikale Stapel Ansicht gibt die erste unter Ansicht für die erste Baseline und die letzte für den letzten zurück. Wenn eine dieser unter Sichten selbst Stapel Ansichten ist, wird Ihre erste oder letzte Baseline verwendet.
- In einer horizontalen Stapel Ansicht wird die höchste unter Ansicht für die erste und die letzte Baseline verwendet. Wenn die höchste Ansicht auch eine Stapel Ansicht ist, wird Sie mit der höchsten unter Ansicht als Baseline verwendet.

> [!IMPORTANT]
> Die Baseline-Ausrichtung funktioniert nicht bei gestreckten oder komprimierten untergeordneten Ansichts Größen, da die Baseline an die falsche Position berechnet wird. Stellen Sie bei der Baseline-Ausrichtung sicher, dass die **Höhe** der unter Ansicht mit der **Höhe**der intrinsischen Inhaltsansicht übereinstimmt.

### <a name="common-stack-view-uses"></a>Allgemeine Stapel Ansicht verwendet

Es gibt mehrere Layouttypen, die gut mit Stapel Ansicht-Steuerelementen funktionieren. Nachfolgend finden Sie einige der gängigeren Verwendungsmöglichkeiten:

- **Definieren der Größe entlang der Achse** – durch anheten beider Kanten entlang der `Axis` der Stapel Ansicht und eines der angrenzenden Ränder, um die Position festzulegen, wird die Stapel Ansicht entlang der Achse vergrößert, sodass Sie an den von den untergeordneten Sichten definierten Platz passt.
- **Legen Sie die Position der untergeordneten Ansicht** fest – indem Sie an angrenzende Ränder der Stapel Ansicht an die übergeordnete Ansicht angehefteten, wird die Stapel Ansicht in beiden Dimensionen vergrößert, sodass Sie mit den untergeordneten Sichten verknüpft ist.
- **Definieren der Größe und Position des Stapels** – durch anheten aller vier Ränder der Stapel Ansicht an die übergeordnete Ansicht ordnet die Stapel Ansicht die unter Ansichten auf der Grundlage des in der Stapel Ansicht definierten Speicherplatzes an.
- **Definieren Sie die Größe der Achse senkrecht** – indem Sie beide Ränder senkrecht zum `Axis` der Stapel Ansicht und einen der Kanten entlang der Achse anheken, um die Position festzulegen, vergrößert sich die Stapel Ansicht senkrecht zur Achse, damit Sie dem durch die untergeordneten Sichten definierten Platz entspricht.

### <a name="managing-the-appearance"></a>Verwalten des Erscheinungs Bilds

Der `UIStackView` ist als nicht Rendering-Container Ansicht konzipiert und wird daher nicht wie andere Unterklassen von `UIView`auf den Zeichenbereich gezeichnet. Das Festlegen von Eigenschaften, z. b. `BackgroundColor` oder überschreiben `DrawRect`, hat keinen visuellen Effekt.

Es gibt mehrere Eigenschaften, mit denen gesteuert wird, wie eine Stapel Ansicht ihre Auflistung von untergeordneten Sichten anordnet:

- **Axis** – bestimmt, ob die untergeordneten Sichten in der Stapel Ansicht entweder **Horizontal** oder **vertikal angeordnet**werden.
- **Alignment** – steuert, wie die untergeordneten Sichten innerhalb der Stapel Ansicht ausgerichtet werden.
- **Distribution** – steuert, wie die untergeordneten Sichten in der Stapel Ansicht skaliert werden.
- **Abstand** – steuert den minimalen Abstand zwischen jeder unter Ansicht in der Stapel Ansicht.
- **Relative Baseline** – wenn `true`, wird der vertikale Abstand der einzelnen untergeordneten Sichten von der Basislinie abgeleitet.
- **Layoutränder relativ** – platziert die untergeordneten Sichten in Relation zu den standardmäßigen Layouträndern.

In der Regel wird eine Stapel Ansicht verwendet, um eine kleine Anzahl von unter Sichten anzuordnen. Komplexere Benutzeroberflächen können erstellt werden, indem eine oder mehrere Stapel Ansichten ineinander geschachtelt werden (wie im obigen [Schnellstart für uistackview](#uistackview-quickstart) beschrieben).

Sie können die Benutzeroberflächen Darstellung weiter optimieren, indem Sie den unter Ansichten zusätzliche Einschränkungen hinzufügen (z. b. um die Höhe oder Breite zu steuern). Allerdings sollte darauf geachtet werden, dass in Konflikt stehende Einschränkungen nicht zu den von der Stapel Ansicht selbst eingeführten Einschränkungen gehören.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Beibehalten der Konsistenz von angeordneten Ansichten und unter Ansichten

Die Stapel Ansicht stellt sicher, dass die `ArrangedSubviews`-Eigenschaft immer eine Teilmenge ihrer `Subviews`-Eigenschaft ist. dabei werden die folgenden Regeln verwendet:

- Wenn der `ArrangedSubviews` Auflistung eine untergeordnete Ansicht hinzugefügt wird, wird Sie automatisch der `Subviews` Auflistung hinzugefügt (sofern Sie nicht bereits Teil dieser Sammlung ist).
- Wenn eine untergeordnete Ansicht aus der `Subviews` Auflistung entfernt (aus der Anzeige entfernt) wird, wird Sie auch aus der `ArrangedSubviews` Auflistung entfernt.
- Wenn Sie eine unter Ansicht aus der `ArrangedSubviews` Sammlung entfernen, wird Sie nicht aus der `Subviews` Auflistung entfernt. Daher wird Sie nicht mehr von der Stapel Ansicht angelegt, sondern weiterhin auf dem Bildschirm angezeigt.

Die `ArrangedSubviews` Sammlung ist immer eine Teilmenge der `Subview` Auflistung. die Reihenfolge der einzelnen untergeordneten Sichten in den einzelnen Sammlungen ist jedoch getrennt und wird durch Folgendes gesteuert:

- Die Reihenfolge der untergeordneten Sichten innerhalb der `ArrangedSubviews` Auflistung bestimmt die Anzeigereihenfolge innerhalb des Stapels.
- Die Reihenfolge der untergeordneten Sichten innerhalb der `Subview` Auflistung bestimmt Ihre Z-Reihenfolge (oder Schicht) innerhalb der Sicht zurück zum Vordergrund.

### <a name="dynamically-changing-content"></a>Dynamisches Ändern von Inhalten

Eine Stapel Ansicht passt automatisch das Layout der untergeordneten Sichten an, wenn eine unter Ansicht hinzugefügt, entfernt oder ausgeblendet wird. Das Layout wird auch angepasst, wenn eine Eigenschaft der Stapel Ansicht angepasst wird (z. b. die `Axis`).

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

In diesem Artikel wurde das neue `UIStackView` Control (IOS 9) behandelt, um eine Reihe von unter Ansichten entweder in einem horizontal oder vertikal angeordneten Stapel in einer xamarin. IOS-APP zu verwalten.
Es wurde ein einfaches Beispiel für die Verwendung von Stapel Ansichten verwendet, um eine Benutzeroberfläche zu erstellen, und mit einer ausführlicheren Betrachtung der Stapel Ansichten und ihrer Eigenschaften und Features.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uistackview-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Einführung in uistackview (Video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
