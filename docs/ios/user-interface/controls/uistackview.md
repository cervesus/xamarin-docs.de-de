---
title: Stack-Ansichten in Xamarin.iOS
description: Dieser Artikel befasst sich mit dem neuen UIStackView-Steuerelement in einer Xamarin.iOS-app zum Verwalten einer Gruppe von Unteransichten entweder in einem Stapel horizontal oder vertikal angeordnet.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e9c18920386cb58f152d7631c52240b4b5b72ff9
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977844"
---
# <a name="stack-views-in-xamarinios"></a>Stack-Ansichten in Xamarin.iOS

_Dieser Artikel befasst sich mit dem neuen UIStackView-Steuerelement in einer Xamarin.iOS-app zum Verwalten einer Gruppe von Unteransichten entweder in einem Stapel horizontal oder vertikal angeordnet._

> [!IMPORTANT]
> Bitte beachten Sie, dass während StackView im iOS-Designer unterstützt wird, Verwendbarkeit Fehler auftreten können, wenn den stabilen Kanal zu verwenden. Die Beta- oder Alpha-Kanäle umschalten, sollte dieses Problem verringern. Wir haben uns entschieden, stellen diese exemplarische Vorgehensweise mit Xcode, bis die erforderlichen Korrekturen im stabilen Kanal implementiert werden.

Die Stack-Steuerelement (`UIStackView`) nutzt die Leistungsfähigkeit von Automatisches Layout und Größenklassen einen Stapel von Unteransichten entweder horizontal oder vertikal zu verwalten, die dynamisch auf die Ausrichtung und der Bildschirmgröße die Größe des iOS-Geräts reagiert.

Das Layout der alle Unteransichten, die an eine Stapelansicht angefügt werden verwaltet, werden basierend auf Entwickler definiert Eigenschaften, z. B. Achse "," Verteilungspunkt "," Ausrichtung "und" Abstand:

[![](uistackview-images/stacked01.png "Stapel anzeigen Layout für Diagramm")](uistackview-images/stacked01.png#lightbox)

Bei Verwendung einer `UIStackView` in einer Xamarin.iOS-app kann der Entwickler die Unteransichten entweder in einem Storyboard in der iOS-Designer oder durch Hinzufügen und Entfernen von Unteransichten in c#-Code entweder definieren.

Dieses Dokument besteht aus zwei Teilen: einen schnellen Einstieg in die Sie unterstützen, implementieren, die Ihre erste Stack anzeigen, und klicken Sie dann weitere technische Details dazu, wie es funktioniert.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView video**

## <a name="uistackview-quickstart"></a>UIStackView-Schnellstart

Als eine kurze Einführung in die `UIStackView` -Steuerelement, sind wir erstellen eine einfache Schnittstelle, die dem Benutzer ermöglichen, eine Bewertung von 1 bis 5 eingeben. Wir verwenden zwei Stack-Ansichten: eine, die die Schnittstelle auf des Geräts die Bildschirm- und eine Bewertung von 1 bis 5 Symbole horizontal auf dem Bildschirm anordnen vertikal anordnen.

### <a name="define-the-ui"></a>Definieren der Benutzeroberflächenautomatisierungs

Starten Sie ein neues Xamarin.iOS-Projekt, und Bearbeiten der **"Main.Storyboard"** -Datei in Interface Builder von Xcode. Ziehen Sie zuerst ein einzelnes **vertikale Stapelansicht** auf die **Ansichtscontroller**:

[![](uistackview-images/quick01.png "Ziehen Sie eine einzelne vertikale Stapel-Ansicht im Ansicht-Controller")](uistackview-images/quick01.png#lightbox)

In der **Attributinspektor**, die folgenden Optionen festlegen:

[![](uistackview-images/quick02.png "Festlegen der Optionen Stapelansicht")](uistackview-images/quick02.png#lightbox)

Ort:

- **Achse** – bestimmt, ob die Stapelansicht die Unteransichten entweder ordnet **horizontal** oder **vertikal**.
- **Ausrichtung** – steuert, wie die Unteransichten innerhalb der Stapelansicht ausgerichtet werden.
- **Verteilung** – steuert, wie die Unteransichten in die Stapelansicht groß sind.
- **Der Abstand** – steuert den minimalen Abstand zwischen jeder Unteransicht in die Stapelansicht.
- **Baseline relativ** – Wenn aktiviert, der vertikale Abstand der einzelnen Unteransicht von seinem Baseline abgeleitet werden.
- **Layout Ränder Relative** – platziert die Unteransichten relativ zu den Rändern Standardlayout.

Bei der Arbeit mit einem Stapel-Ansicht können Sie sich vorstellen der **Ausrichtung** als die **X** und **Y** Speicherort, der die Unteransicht und **Verteilung** wie die **Höhe** und **Breite**.

> [!IMPORTANT]
> `UIStackView` Dient als für nicht-Rendering-Containeransicht und ist daher nicht Zeichnen auf den Zeichenbereich wie andere Unterklassen von `UIView`. Festlegen von Eigenschaften so wie z. B. `BackgroundColor` oder überschreiben `DrawRect` keine visuellen Auswirkungen.

Weiterhin Layout der app-Benutzeroberfläche durch eine Bezeichnung, ImageView, zwei Schaltflächen und einem horizontalen Stapel-Ansicht hinzufügen, sodass sie die folgenden ähnelt:

[![](uistackview-images/quick03.png "Anordnen von der Benutzeroberfläche der Stapel-Ansicht")](uistackview-images/quick03.png#lightbox)

Konfigurieren der horizontalen Stapel-Ansicht mit den folgenden Optionen:

[![](uistackview-images/quick04.png "Konfigurieren Sie die Optionen für die horizontalen Stapel-Ansicht")](uistackview-images/quick04.png#lightbox)

Da wir nicht das Symbol für jede "Point" möchten, in der Bewertung gedehnt werden, wenn es hinzugefügt wird der horizontalen Stapel-Ansicht festgelegt haben, wir die **Ausrichtung** zu **Center** und  **Verteilung** zu **gleichermaßen füllen**.

Schließlich werden die folgenden socialsharing.Share **Outlets** und **Aktionen**:

[![](uistackview-images/quick05.png "Die Stack Ansicht Outlets und Aktionen")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Auffüllen einer UIStackView aus Code

Zurück zu Visual Studio für Mac, und Bearbeiten der **ViewController.cs** Datei, und fügen Sie den folgenden Code hinzu:

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

Werfen wir einen Blick auf einige Teile dieses Codes im Detail. Verwenden Sie als Erstes eine `if` Anweisungen, um zu überprüfen, dass es nicht mehr als fünf "Sternen" oder kleiner als 0 (null).

Hinzufügen einer neuen "Star" laden wir das Bild, und legen seine **Modus Inhalt** zu **Seitengröße skalieren Aspekt**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Dadurch wird verhindert, dass das Symbol für "Sterne" verzerrt wird, wenn sie die Stapelansicht hinzugefügt wird.

Als Nächstes fügen wir das neue Symbol für "Stern" auf die Stapelansicht-Auflistung von Unteransichten hinzu:

```csharp
RatingView.AddArrangedSubview(icon);
```

Sie werden feststellen, dass wir hinzugefügt, die `UIImageView` auf der `UIStackView`des `ArrangedSubviews` Eigenschaft und nicht auf die `SubView`. Einer beliebigen Ansicht, dass der Stapel-Ansicht zur Steuerung des Layouts sollen muss hinzugefügt werden, um die `ArrangedSubviews` Eigenschaft.

Um eine Unteransicht aus einer Sicht Stapel zu entfernen, rufen wir zunächst die Unteransicht entfernen:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Wir müssen sie sowohl Entfernen der `ArrangedSubviews` Auflistung und die Super-Ansicht:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Entfernen eine Unteransicht aus lediglich die `ArrangedSubviews` Auflistung dauert es nicht die Stapelansicht Kontrolle, jedoch nicht vom Bildschirm entfernt wird.

### <a name="testing-the-ui"></a>Testen der UI

Mit allen erforderlichen Elemente der Benutzeroberfläche und Code vorhanden können Sie jetzt ausführen und testen die Schnittstelle. Wenn die Benutzeroberfläche angezeigt wird, werden alle Elemente in der vertikalen Stapel-Ansicht von oben nach unten gleichmäßigem Abstand angeordnet werden.

Wenn der Benutzer tippt der **Bewertung erhöhen** Schaltfläche eine andere "Star" wird hinzugefügt, auf dem Bildschirm (bis zu maximal 5):

[![](uistackview-images/intro01.png "Führen Sie die Beispiel-app")](uistackview-images/intro01.png#lightbox)

Die "Sternen" automatisch zentriert und gleichmäßig verteilt, in der horizontalen Stapel-Ansicht. Wenn der Benutzer tippt der **Wertung verringern** Schaltfläche ein "Star" wird entfernt (bis keine Links sind).

## <a name="stack-view-details"></a>Details zum Stack

Nun, da wir einen allgemeinen Überblick über Was haben die `UIStackView` Steuerelement ist und wie es funktioniert, werfen wir einen tieferen Einblick in einige der Features und Details.

### <a name="auto-layout-and-size-classes"></a>Automatisches Layout und Größenklassen

Wie wir oben gesehen haben, wenn eine Unteransicht hinzugefügt wird eine Stapelansicht Layouts wird vollständig durch die dieser Stapel-Ansicht verwenden Automatisches Layout und Größenklassen zum Positionieren und ihre Größe des angeordneten Ansichten gesteuert.

Der Stapel-Ansicht wird _Pin_ die vor- und Nachnamen Unteransicht in ihrer Sammlung, um die **oben** und **unten** Kanten für die vertikalen Stapel Ansichten oder **Links**und **rechts** Kanten für die horizontale Stack-Ansichten. Setzen Sie die `LayoutMarginsRelativeArrangement` Eigenschaft `true`, und klicken Sie dann die Ansicht die Unteransichten den entsprechenden Rand anstelle der Rand fixiert.

Verwendet die Stack-Sicht, der Unteransicht des `IntrinsicContentSize` Eigenschaft beim Berechnen der Größe Unteransichten entlang der definierten `Axis` (mit Ausnahme der `FillEqually Distribution`). Die `FillEqually Distribution` alle Unteransichten ändert, sodass sie die gleiche Größe, werden daher füllen die Stapelansicht entlang der `Axis`.

Mit Ausnahme von der `Fill Alignment`, verwendet die Stack-Sicht, der Unteransicht des `IntrinsicContentSize` -Eigenschaft für die Berechnung der Ansicht Größe senkrecht zur der angegebenen `Axis`. Für die `Fill Alignment`, alle Unteransichten werden angepasst, sodass sie die senkrecht zur Stapelansicht Ausfüllen der angegebenen `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Positionierung und Größenanpassung der Stapelansicht

Während die Stapelansicht vollständige Kontrolle über das Layout der alle Unteransicht hat (basierend auf Eigenschaften wie z. B. `Axis` und `Distribution`), müssen Sie immer noch zum Positionieren der Ansicht Stack (`UIStackView`) innerhalb ihrer übergeordneten Ansicht mit Automatisches Layout und die Größenklassen.

Im Allgemeinen bedeutet dies, mindestens zwei Kanten der Sicht ein Stapel, Erweiterung und Verkleinerung, definieren daher seine Position anheften. Ohne zusätzlichen Einschränkungen werden die Stapelansicht automatische größenanpassung werden alle zugehörigen Unteransichten wie folgt:

 - Die Größe entlang der `Axis` wird die Summe der Größen für alle Unteransicht und eine beliebige Stelle, die zwischen einzelnen Unteransicht definiert wurde.
 - Wenn die `LayoutMarginsRelativeArrangement` Eigenschaft `true`, die Größe des Stapels Ansichten umfasst auch Raum für Ränder.
 - Die senkrecht zur Größe der `Axis` wird auf die größten Unteransicht in der Auflistung festgelegt werden.

Darüber hinaus können Sie Einschränkungen angeben, für die Stapelansicht **Höhe** und **Breite**. In diesem Fall die Unteransichten angeordnet werden (Größe) den Platz gemäß der Stapelansicht gemäß der `Distribution` und `Alignment` Eigenschaften.

Wenn die `BaselineRelativeArrangement` Eigenschaft `true`, die Unteransichten angeordnet werden basierend auf der ersten oder letzten Unteransicht Baseline, anstatt die **oben**, **unten** oder **Center** -  **Y** Position. Diese werden in die Stapelansicht Inhalt wie folgt berechnet:

 - Einem vertikalen Stapel-Ansicht wird der erste Unteransicht für die erste Baseline und der letzte für die letzte zurückgegeben. Wenn eine der folgenden Unteransichten selbst Stack Ansichten sind, wird die erste oder letzte Baseline verwendet werden.
 - Einem horizontalen Stapel-Ansicht wird der höchste Unteransicht für die erste und letzte Baseline verwendet werden. Wenn der höchste Ansicht auch ein Stapel ist, wird als Grundlage des höchsten Unteransicht verwendet.

> [!IMPORTANT]
> Baseline-Ausrichtung funktioniert nicht für gestreckt oder komprimiert Unteransicht Größen, wie die Baseline der falschen Position berechnet werden sollen. Für Baseline-Ausrichtung, sicherstellen, dass der Unteransicht des **Höhe** entspricht der systeminterne Inhaltsansicht **Höhe**.

### <a name="common-stack-view-uses"></a>Einsatzmöglichkeiten von Stapel anzeigen

Es gibt mehrere Typen aus dem Layout, die gut mit Stapelansicht Steuerelemente funktionieren. Hier sind einige der häufigeren Verwendungsarten, gemäß den Apple:

- **Definieren Sie die Größe entlang der Achse** – durch Anheften von beide Ränder entlang der Stapelansicht `Axis` und einen der angrenzenden Ränder die Position im Stapel festlegen Ansicht wächst die Achse von untergeordneten Ansichten definierte Raum zu passen.
 -  **Definieren Sie die Unteransicht des Position** – durch anheften an benachbarte Ränder Stack Ansicht, um die übergeordnete Ansicht, wächst die Stapelansicht in beiden Dimensionen des enthaltenden Unteransichten anpassen.
- **Definieren Sie die Größe und Position des Stapels** – durch Anheften von allen vier Kanten der Stack Ansicht für die übergeordnete Ansicht, ordnet die Stapelansicht die Unteransichten basierend auf den Speicherplatz, der in die Stapelansicht definiert.
 -  **Definieren der Größe senkrecht der Achse** – beide Ränder senkrecht an den Stapel-Ansicht anheften `Axis` und einen der Ränder auf der Achse, um die Position im Stapel festzulegen Ansicht senkrecht zur Achse wächst von definierte Raum zu passen die Unteransichten.

### <a name="managing-the-appearance"></a>Verwalten Sie die Darstellung von

Die `UIStackView` dient als für nicht-Rendering-Containeransicht und ist daher nicht Zeichnen auf den Zeichenbereich wie andere Unterklassen von `UIView`. Festlegen von Eigenschaften wie z. B. `BackgroundColor` oder überschreiben `DrawRect` keine visuellen Auswirkungen.

Es gibt mehrere Eigenschaften, die steuern, wie ein Stapel-Ansicht ihrer Auflistung von Unteransichten anordnen, wird ein:

- **Achse** – bestimmt, ob die Stapelansicht die Unteransichten entweder ordnet **horizontal** oder **vertikal**.
- **Ausrichtung** – steuert, wie die Unteransichten innerhalb der Stapelansicht ausgerichtet werden.
- **Verteilung** – steuert, wie die Unteransichten in die Stapelansicht groß sind.
- **Der Abstand** – steuert den minimalen Abstand zwischen jeder Unteransicht in die Stapelansicht.
- **Baseline relativ** – Wenn `true`, der vertikale Abstand der einzelnen Unteransicht von seinem Baseline abgeleitet werden.
- **Layout Ränder Relative** – platziert die Unteransichten relativ zu den Rändern Standardlayout.

In der Regel verwenden Sie einen Stapel-Ansicht um eine kleine Anzahl von Unteransichten anzuordnen. Komplexere Benutzeroberflächen erstellt werden, indem Sie eine oder mehrere Stack Ansichten ineinander schachteln (wie der [UIStackView Quickstart](#uistackview-quickstart) oben).

Sie können die Darstellung von Benutzeroberflächen weiter optimieren, durch das Hinzufügen von zusätzlicher Einschränkungen, die Unteransichten (z. B. um die Höhe oder Breite-Steuerelement). Allerdings sollte darauf geachtet werden, nicht in Konflikt stehenden Einschränkungen auf solche, die selbst von der Sicht Stack aufzunehmen.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Verwalten von angeordnet, Ansichten und untergeordnete Ansichten Konsistenz

Die Stapelansicht stellt sicher, dass die `ArrangedSubviews` Eigenschaft ist immer eine Teilmenge der `Subviews` Eigenschaft mithilfe der folgenden Regeln:

 - Wenn eine Unteransicht hinzugefügt wird die `ArrangedSubviews` -Auflistung, er wird automatisch hinzugefügt werden die `Subviews` Auflistung (sofern es bereits Teil dieser Sammlung darstellt).
 - Wenn aus eine Unteransicht entfernt wird die `Subviews` Auflistung (aus der Anzeige entfernt), wird er auch aus entfernt die `ArrangedSubviews` Auflistung.
 - Entfernen einer Unteransicht aus der `ArrangedSubviews` Auflistung wird nicht entfernt, von der `Subviews` Auflistung. Also nicht mehr von der Stapel-Ansicht angeordnet, sondern werden weiterhin auf dem Bildschirm angezeigt.

Die `ArrangedSubviews` Auflistung ist immer eine Teilmenge der `Subview` -Auflistung, jedoch die Reihenfolge der einzelnen Unteransichten in jeder Sammlung getrennt und kontrollierten folgendermaßen ist:

 - Die Reihenfolge der Unteransichten innerhalb der `ArrangedSubviews` Auflistung bestimmen der angezeigten Reihenfolge innerhalb des Stapels.
 - Die Reihenfolge der Unteransichten innerhalb der `Subview` Auflistung bestimmt die Z-Reihenfolge (oder Schichten) in der Ansicht in den Vordergrund zurück.

### <a name="dynamically-changing-content"></a>Inhalt dynamisch ändern

Eine Stapelansicht werden das Layout der Unteransichten automatisch angepasst, wenn eine Unteransicht hinzugefügt, entfernt oder ausgeblendet wird. Das Layout wird auch angepasst werden, wenn eine Eigenschaft der Ansicht Stack angepasst wird (z. B. die `Axis`).

Änderungen am Layout können animiert werden, indem Sie sie z. B. innerhalb einer Animation-Blocks platzieren:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Viele der Eigenschaften der Stack-Sicht können Größenklassen in einem Storyboard angegeben werden. Diese Eigenschaften werden automatisch animiert die Reaktion auf Änderungen der Größe oder Ausrichtung ist.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen behandelt `UIStackView` (für iOS 9) zum Verwalten einer Gruppe von Unteransichten in einem horizontal oder vertikal angeordneten Stapel in einer Xamarin.iOS-app steuern.
Er begann mit einem einfachen Beispiel der Verwendung von Stack-Ansichten zum Erstellen einer Benutzeroberflächenautomatisierungs und eine ausführlichere Betrachtung Stack Ansichten und ihre Eigenschaften und Funktionen abgeschlossen.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Einführung in UIStackView (Video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
