---
title: Stapelansicht
description: Dieser Artikel behandelt dem neuen UIStackView-Steuerelement in einem Xamarin.iOS-app zum Verwalten einer Gruppe von Unteransichten entweder in einer horizontal oder vertikal angeordnet Stack verwenden.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: aec1c4ceb562f528d6bcef7e4de0f2708952d2a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="stack-view"></a>Stapelansicht

_Dieser Artikel behandelt dem neuen UIStackView-Steuerelement in einem Xamarin.iOS-app zum Verwalten einer Gruppe von Unteransichten entweder in einer horizontal oder vertikal angeordnet Stack verwenden._

> [!IMPORTANT]
> Beachten Sie, dass während StackView in der iOS-Designer unterstützt wird, Sie Verwendbarkeit Fehler auftreten, wenn stabilen Kanal verwenden. Wechseln von der Beta- oder Alpha-Kanäle sollte dieses Problem beheben. Wir haben sich entschieden, an dieser exemplarischen Vorgehensweise mithilfe von Xcode, bis die erforderlichen Korrekturen im stabilen Kanal implementiert werden.

Der Stapel Ansichtssteuerelement (`UIStackView`) nutzt die Funktionen von Automatisches Layout und Klassen Größe einen Stapel mit Unteransichten, horizontal oder vertikal zu verwalten, die dynamisch auf die Ausrichtung und der Bildschirmgröße Größe der iOS-Gerät antwortet.

Das Layout der alle Unteransichten an eine Stapelansicht angefügt vom Entwickler definierten Eigenschaften wie z. B. Achse "," Verteilung "," Ausrichtung "und" Abstand Grundlage verwaltet werden:

[![](uistackview-images/stacked01.png "Stapel Layout-Diagramm")](uistackview-images/stacked01.png#lightbox)

Bei Verwendung einer `UIStackView` in einer app Xamarin.iOS Definieren der Entwickler kann den Unteransichten entweder in eine Storyboard in der iOS-Designer oder durch Hinzufügen und Entfernen von Unteransichten in C#-Code.

Dieses Dokument besteht aus zwei Teilen: einen schnellen Einstieg können Sie Sie implementieren, die Ihre erste Stapel anzeigen und dann einige weitere technische Details zur Funktionsweise.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, von [Xamarin University](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView-Schnellstart

Als eine kurze Einführung in die `UIStackView` -Steuerelement, werden wir erstellen eine einfache Oberfläche, mit denen den Benutzer eine Bewertung von 1 bis 5 eingeben können. Wir müssen zwei Stapel Ansichten verwenden: eine für die Schnittstelle auf des Geräts Bildschirm- und eine zum Anordnen von 1 bis 5 Bewertung Symbole horizontal auf dem Bildschirm vertikal anordnen.

### <a name="define-the-ui"></a>Definieren der Benutzeroberflächenautomatisierungs

Starten Sie ein neues Projekt für Xamarin.iOS und Bearbeiten der **Main.storyboard** Datei in Xcodes Benutzeroberflächen-Generator. Ziehen Sie zuerst ein einzelnes **vertikale Stapelansicht** auf die **Modellansichtcontroller**:

[![](uistackview-images/quick01.png "Ziehen Sie eine einzelne vertikalen Stapelansicht auf die View-Controller")](uistackview-images/quick01.png#lightbox)

In der **Attribut Inspektor**, die folgenden Optionen festlegen:

[![](uistackview-images/quick02.png "Festlegen der Optionen Stapelansicht")](uistackview-images/quick02.png#lightbox)

Ort:

- **Achse** – bestimmt, ob die Stapelansicht entweder die Unteransichten ordnet **horizontal** oder **vertikal**.
- **Ausrichtung** – steuert, wie die Unteransichten innerhalb der Stapelansicht ausgerichtet werden.
- **Verteilung** – steuert, wie die Unteransichten in die Stapelansicht angepasst werden.
- **Abstand** – den minimalen Abstand zwischen den einzelnen Unteransicht in die Stapelansicht steuert.
- **Baseline-Relative** – Wenn dieses Kontrollkästchen aktiviert, wird der vertikale Abstand von jeder Unteransicht gegenüber der Basislinie abgeleitet werden.
- **Layout Ränder Relative** – die Unteransichten relativ zum Standardlayout Ränder platziert.

Bei der Arbeit mit einem Stapelansicht können Sie vorstellen, der die **Ausrichtung** als die **X** und **Y** Speicherort, der den Unteransicht und die **Verteilung** wie die **Höhe** und **Breite**.

> [!IMPORTANT]
> `UIStackView` Dient als für nicht-Rendering-Containeransicht und daher ist es nicht gezeichnet auf den Zeichenbereich wie andere Unterklassen des `UIView`. Festlegen von Eigenschaften also z. B. `BackgroundColor` oder überschreiben `DrawRect` keine visuellen Auswirkungen.

Weiterhin Layout der app-Schnittstelle durch eine Bezeichnung, ImageView, zwei Schaltflächen und eine horizontale Stapelansicht hinzufügen, sodass sie die folgenden ähnelt:

[![](uistackview-images/quick03.png "Layout der Benutzeroberfläche der Stapel anzeigen")](uistackview-images/quick03.png#lightbox)

Konfigurieren Sie die horizontale Stapelansicht mit den folgenden Optionen aus:

[![](uistackview-images/quick04.png "Konfigurieren Sie die Optionen für horizontale Stapelansicht")](uistackview-images/quick04.png#lightbox)

Da wir nicht das Symbol, das jedes "Punkt" stellt möchten, in die Bewertung für die Streckung Wenn es hinzugefügt wird, die horizontale Stapelansicht legen wir haben die **Ausrichtung** auf **Center** und  **Verteilung** auf **gleichermaßen füllen**.

Schließlich Folgendes verknüpfen **Steckdosen** und **Aktionen**:

[![](uistackview-images/quick05.png "Der Stapel anzeigen Steckdosen und Aktionen")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Auffüllen einer UIStackView aus Code

Zurück zu Visual Studio für Mac und Bearbeiten der **ViewController.cs** Datei, und fügen Sie den folgenden Code hinzu:

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

Werfen wir einen Blick auf einige Teile dieses Codes im Detail. Wir verwenden Sie zuerst eine `if` Anweisungen, um zu überprüfen, dass es nicht mehr als fünf "Sterne" oder kleiner als 0 (null).

So fügen Sie einen neuen "Stern" hinzu, die wir laden seines Abbilds und legen Sie dessen **Modus Inhalt** auf **Seitengröße skalieren Aspekt**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Dadurch wird verhindert, dass das "Sternsymbol" verzerrt wird, wenn die Stapelansicht hinzugefügt wird.

Fügen Sie anschließend das Symbol "Neu"Stern"" auf die Stapelansicht Auflistung von Unteransichten:

```csharp
RatingView.AddArrangedSubview(icon);
```

Sie werden feststellen, dass wir hinzugefügt der `UIImageView` auf die `UIStackView`des `ArrangedSubviews` Eigenschaft und nicht auf die `SubView`. Eine beliebige Ansicht, die Stapelansicht sollen steuern das Layout muss hinzugefügt werden die `ArrangedSubviews` Eigenschaft.

Zum Entfernen einer Unteransicht aus einem Stapelansicht erhalten wir zuerst den Unteransicht zu entfernen:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Wir müssen es nicht mit den beiden Entfernen der `ArrangedSubviews` Auflistung und die Super-Ansicht:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Entfernen einer Unteransicht aus nur die `ArrangedSubviews` Auflistung aus dem Stapel Ansichtssteuerelement dauert, jedoch nicht aus dem Bildschirm entfernen.

### <a name="testing-the-ui"></a>Testen der Benutzeroberflächenautomatisierungs

Sie können mit allen erforderlichen Elemente der Benutzeroberfläche und Code an Stelle jetzt ausführen und Testen der Benutzeroberfläche. Wenn die Benutzeroberfläche angezeigt wird, werden alle Elemente in der vertikalen Stapelansicht von oben nach unten gleichmäßigem Abstand angeordnet werden.

Wenn der Benutzer tippt der **erhöhen Bewertung** Schaltfläche, eine andere "Star" wird hinzugefügt, auf dem Bildschirm (bis zu maximal 5):

[![](uistackview-images/intro01.png "Führen Sie die Beispiel-app")](uistackview-images/intro01.png#lightbox)

Die "Sterne" werden automatisch zentriert und in der horizontalen Stapelansicht gleichmäßig verteilt werden. Wenn der Benutzer tippt der **verringern Bewertung** Schaltfläche, einen "Stern" wird entfernt (bis keine bleiben).

## <a name="stack-view-details"></a>Details der Stapel anzeigen

Nun mit dem eine allgemeine Vorstellung davon, was die `UIStackView` Steuerelement ist und wie es funktioniert, werfen wir einen tieferen Einblick in einige der Features und Details.

### <a name="auto-layout-and-size-classes"></a>Automatisches Layout und Größenklassen

Wie oben wurde erläutert, wenn eine Unteransicht hinzugefügt wird eine Stapelansicht das Layout wird durch diese Stapelansicht Automatisches Layout und Klassen Größe, position und Größe der angeordneten Ansichten mit völlig gesteuert.

Wird die Stapelansicht _Pin_ den Unteransicht vor- und Nachnamen in der Auflistung, an der **oben** und **unteren** Kanten für vertikalen Stapel Ansichten oder die **Links**und **rechts** Kanten für horizontale Stapel Ansichten. Wenn Sie festlegen, die `LayoutMarginsRelativeArrangement` Eigenschaft, um `true`, und klicken Sie dann die Ansicht der Unteransichten auf die relevanten Ränder anstelle der Kante pins.

Die Stapelansicht verwendet den Unteransicht `IntrinsicContentSize` Eigenschaft beim Berechnen der Größe Unteransichten entlang des definierten `Axis` (mit Ausnahme der `FillEqually Distribution`). Die `FillEqually Distribution` ändert die Größe aller Unteransichten, damit sie die gleiche Größe sind daher füllen die Stapelansicht entlang der `Axis`.

Mit Ausnahme von der `Fill Alignment`, die Stapelansicht verwendet den Unteransicht `IntrinsicContentSize` Eigenschaft für die die Sicht Größe senkrecht zur Berechnung der angegebenen `Axis`. Für die `Fill Alignment`, alle Unteransichten haben eine Größe von, damit sie die Stapelansicht senkrecht zum Füllen der angegebenen `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Position und Größe der Stapelansicht

Während die Stapelansicht die vollständige Kontrolle über das Layout der alle Unteransicht hat (basierend auf Eigenschaften wie z. B. `Axis` und `Distribution`), müssen Sie dennoch die Stapelansicht positionieren (`UIStackView`) innerhalb ihrer übergeordneten Ansicht mit Automatisches Layout und Klassen Größe.

Im Allgemeinen bedeutet dies, mindestens zwei Kanten der Sicht ein Stapel, Erweiterung und Verkleinerung daher definieren seiner Position anheften. Ohne zusätzlichen Einschränkungen wird die Stapelansicht automatisch angepasst werden alle seine Unteransichten wie folgt:

 - Die Größe entlang seiner `Axis` werden die Summe der Größen für alle Unteransicht und Leerzeichen, die zwischen jeder Unteransicht definiert wurde.
 - Wenn die `LayoutMarginsRelativeArrangement` Eigenschaft ist `true`, die Größe des Stapels Ansichten gehören auch Raum für Ränder.
 - Die senkrecht zur Größe der `Axis` festgelegt, um die größten Unteransicht in der Auflistung.

Darüber hinaus können Sie Einschränkungen angeben, für die Stapelansicht **Höhe** und **Breite**. In diesem Fall die Unteransichten angeordnet werden (Größe) um den durch die Stapelansicht gemäß angegebene Platz Ausfüllen der `Distribution` und `Alignment` Eigenschaften.

Wenn die `BaselineRelativeArrangement` Eigenschaft ist `true`, die Unteransichten angeordnet werden basierend auf der ersten oder letzten Unteransicht Basislinie ruht, anstatt die **oben**, **unteren** oder **Center* -  **Y** Position. Diese werden auf die Stapelansicht Inhalt wie folgt berechnet:

 - Eine vertikale Stapelansicht gibt die erste Unteransicht für die erste Baseline und der letzte für den letzten zurück. Wenn eine der folgenden Unteransichten selbst Stapel Ansichten sind, wird ihre erste oder letzte Basislinie verwendet werden.
 - Eine horizontale Stapelansicht wird die höchste Unteransicht des vor- und Nachnamen Basislinien verwenden. Wenn der höchste Ansicht auch ein Stapel ist, wird als Grundlage des höchsten Unteransicht verwendet.

> [!IMPORTANT]
> Baseline-Ausrichtung funktioniert gestreckt oder komprimiert Unteransicht Größen nicht wie die Grundlinie der falschen Position berechnet wird. Für Baseline-Ausrichtung sicher, dass den Unteransicht **Höhe** entspricht der systeminternen Inhaltsansicht **Höhe**.

### <a name="common-stack-view-uses"></a>Allgemeine Verwendungsmöglichkeiten der Stapel anzeigen

Es gibt mehrere Layout-Typen, die Stapelansicht Steuerelementen geeignet. Gemäß den Apple sind hier einige der häufigeren Verwendungen:

- **Definieren der Größe entlang der Achse** – beide Ränder entlang der Stapelansicht anheften `Axis` und eine der angrenzenden Kanten festzulegende Position im Stapel, Ansicht entlang der Achse durch seine Unteransichten definierten Platz wächst.
 -  **Definieren Sie den Unteransicht Position** – anheften an benachbarte Rändern eines übergeordneten Ansicht der Stapelansicht, wächst die Stapelansicht in beiden Dimensionen des enthaltenden Unteransichten anpassen.
- **Definieren Sie die Größe und Position des Stapels** – anheften alle vier Ränder der Stapelansicht für die übergeordnete Ansicht, ordnet die Stapelansicht der Unteransichten basierend auf den Speicherplatz, der in die Stapelansicht definiert.
 -  **Definieren der Größe Rechtwinklige Achse** – beide senkrecht Ränder die Stapelansicht anheften `Axis` und eine der Kanten entlang der Achse zum Festlegen der Position, den Stapel, Ansicht der Achse wächst durch definierten Platz senkrecht, seine Unteransichten.

### <a name="managing-the-appearance"></a>Verwalten Sie die Darstellung von

Die `UIStackView` dient als für nicht-Rendering-Containeransicht und daher ist es nicht gezeichnet auf den Zeichenbereich wie andere Unterklassen des `UIView`. Festlegen von Eigenschaften wie z. B. `BackgroundColor` oder überschreiben `DrawRect` keine visuellen Auswirkungen.

Es gibt verschiedene Eigenschaften, die steuern, wie eine Stapelansicht mitfinanziert Unteransichten anordnen möchten:

- **Achse** – bestimmt, ob die Stapelansicht entweder die Unteransichten ordnet **horizontal** oder **vertikal**.
- **Ausrichtung** – steuert, wie die Unteransichten innerhalb der Stapelansicht ausgerichtet werden.
- **Verteilung** – steuert, wie die Unteransichten in die Stapelansicht angepasst werden.
- **Abstand** – den minimalen Abstand zwischen den einzelnen Unteransicht in die Stapelansicht steuert.
- **Baseline-Relative** – Wenn `true`, der vertikale Abstand von jeder Unteransicht gegenüber der Basislinie abgeleitet wird.
- **Layout Ränder Relative** – die Unteransichten relativ zum Standardlayout Ränder platziert.

In der Regel verwenden Sie eine Stapelansicht eine kleine Anzahl von Unteransichten anordnen. Komplexere Benutzeroberflächen erstellt werden, indem Sie eine oder mehrere Stapel Ansichten ineinander schachteln (wie der [UIStackView Quickstart](#UIStackView-Quickstart) oben).

Sie können die Benutzeroberflächen-Darstellung weiter optimieren, indem die Unteransichten (z. B. zum Steuerelement die Höhe oder Breite) zusätzliche Einschränkungen hinzugefügt. Allerdings sollten Sie vorsichtig sein, nicht in Konflikt stehenden Einschränkungen, die durch die Stapelansicht selbst eingeführten aufzunehmen.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Verwalten von angeordnet, Ansichten und untergeordnete Ansichten Konsistenz

Die Stapelansicht wird sichergestellt, dass seine `ArrangedSubviews` Eigenschaft ist immer eine Teilmenge seiner `Subviews` Eigenschaft mithilfe der folgenden Regeln:

 - Wenn eine Unteransicht hinzugefügt wird die `ArrangedSubviews` -Auflistung, es wird automatisch hinzugefügt werden die `Subviews` Auflistung (es sei denn, es bereits Teil dieser Sammlung ist).
 - Wenn eine Unteransicht aus entfernt wird die `Subviews` -Auflistung (aus der Anzeige entfernt), wird es auch aus entfernt die `ArrangedSubviews` Auflistung.
 - Entfernen einer Unteransicht aus der `ArrangedSubviews` Sammlung nicht entfernt, von der `Subviews` Auflistung. Daher nicht mehr durch die Stapelansicht angeordnet, aber immer noch auf dem Bildschirm angezeigt werden.

Die `ArrangedSubviews` Sammlung ist stets eine Teilmenge von der `Subview` -Auflistung, jedoch ist die Reihenfolge der in jeder Auflistung der einzelnen Unteransichten getrennte und durch Folgendes gesteuert:

 - Die Reihenfolge der der Unteransichten innerhalb der `ArrangedSubviews` Auflistung bestimmen der angezeigten Reihenfolge in den Stapel.
 - Die Reihenfolge der der Unteransichten innerhalb der `Subview` Auflistung bestimmt die Z-Reihenfolge (oder Strukturlayout) innerhalb der Ansicht hinten nach vorne.

### <a name="dynamically-changing-content"></a>Inhalt dynamisch ändern

Eine Stapelansicht wird das Layout der Unteransichten automatisch angepasst, sobald eine Unteransicht hinzugefügt, entfernt oder ausgeblendet wird. Das Layout auch angepasst werden, wenn eine Eigenschaft der Stapelansicht angepasst wird (z. B. seine `Axis`).

Änderungen am Layout können animiert werden, indem sie z. B. innerhalb eines Blocks Animation platziert:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Viele der Eigenschaften der Stapel-Sicht können mit Größe-Klassen in einem Storyboard angegeben werden. Diese Eigenschaften werden automatisch als Reaktion auf Größe oder Ausrichtung Änderungen animiert wird.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat die neue abgedeckt `UIStackView` (für iOS 9) zum Verwalten einer Gruppe von Unteransichten entweder ein horizontal oder vertikal angeordnet Stapel in einem Xamarin.iOS-app steuern.
Es beginnt mit der ein einfaches Beispiel mit der Stapel Ansichten zum Erstellen einer Benutzeroberflächenautomatisierungs und mit einem ausführlicheren Blick auf den Stapel Ansichten und ihre Eigenschaften und Funktionen wurde beendet.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Einführung in UIStackView (Video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
