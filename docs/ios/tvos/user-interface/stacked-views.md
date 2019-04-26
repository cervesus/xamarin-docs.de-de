---
title: Arbeiten mit TvOS gestapelt Ansichten in Xamarin
description: Dieses Dokument beschreibt, wie Sie arbeitet mit TvOS gestapelte Ansichten in einer app mit Xamarin erstellt wurde. Es bietet einen Überblick über gestapelte Ansichten und automatisches Layout, Position und Größe eine gestapelte Ansicht, häufig verwendet, -Integration mit Storyboards und vieles mehr erläutert.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f51ed3d6dbbfc8a7e430c2949485838a7471e545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61161466"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Arbeiten mit TvOS gestapelt Ansichten in Xamarin

Die Stack-Steuerelement (`UIStackView`) nutzt die Leistungsfähigkeit von Automatisches Layout und Größenklassen einen Stapel von Unteransichten entweder horizontal oder vertikal zu verwalten, die dynamisch auf Änderungen am Inhalt und der Bildschirmgröße des Geräts Apple TV reagiert.

Das Layout der alle Unteransichten, die an eine Stapelansicht angefügt werden verwaltet, werden basierend auf Entwickler definiert Eigenschaften, z. B. Achse "," Verteilungspunkt "," Ausrichtung "und" Abstand:

[![](stacked-views-images/stacked01.png "Unteransicht Layout für Diagramm")](stacked-views-images/stacked01.png#lightbox)

Bei Verwendung einer `UIStackView` in einer app Xamarin.tvOS Definieren der Entwickler kann die Unteransichten entweder in einem Storyboard in der iOS-Designer oder durch Hinzufügen und Entfernen von Unteransichten in C# Code.

## <a name="about-stacked-view-controls"></a>Informationen zu gestapelten-Steuerelemente 

Die `UIStackView` dient als für nicht-Rendering-Containeransicht und ist daher nicht Zeichnen auf den Zeichenbereich wie andere Unterklassen von `UIView`. Festlegen von Eigenschaften wie z. B. `BackgroundColor` oder überschreiben `DrawRect` keine visuellen Auswirkungen.

Es gibt mehrere Eigenschaften, die steuern, wie ein Stapel-Ansicht ihrer Auflistung von Unteransichten anordnen, wird ein:

- **Achse** – bestimmt, ob die Stapelansicht die Unteransichten entweder ordnet **horizontal** oder **vertikal**.
- **Ausrichtung** – steuert, wie die Unteransichten innerhalb der Stapelansicht ausgerichtet werden.
- **Verteilung** – steuert, wie die Unteransichten in die Stapelansicht groß sind.
- **Der Abstand** – steuert den minimalen Abstand zwischen jeder Unteransicht in die Stapelansicht.
- **Baseline relativ** – Wenn `true`, der vertikale Abstand der einzelnen Unteransicht von seinem Baseline abgeleitet werden.
- **Layout Ränder Relative** – platziert die Unteransichten relativ zu den Rändern Standardlayout.

In der Regel verwenden Sie einen Stapel-Ansicht um eine kleine Anzahl von Unteransichten anzuordnen. Eine oder mehrere Stack Ansichten ineinander schachteln können komplexere Benutzeroberflächen erstellt werden.

Sie können die Darstellung von Benutzeroberflächen weiter optimieren, durch das Hinzufügen von zusätzlicher Einschränkungen, die Unteransichten (z. B. um die Höhe oder Breite-Steuerelement). Allerdings sollte darauf geachtet werden, nicht in Konflikt stehenden Einschränkungen auf solche, die selbst von der Sicht Stack aufzunehmen.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Automatisches Layout und Größenklassen

Wenn eine Stapelansicht eine Unteransicht hinzugefügt wird, wird der Layouts vollständig durch, automatisches Layout und die Größenklassen zu positionieren und ihre Größe des angeordneten Ansichten mit Stapelansicht gesteuert.

Der Stapel-Ansicht wird _Pin_ die vor- und Nachnamen Unteransicht in ihrer Sammlung, um die **oben** und **unten** Kanten für die vertikalen Stapel Ansichten oder **Links**und **rechts** Kanten für die horizontale Stack-Ansichten. Setzen Sie die `LayoutMarginsRelativeArrangement` Eigenschaft `true`, und klicken Sie dann die Ansicht die Unteransichten den entsprechenden Rand anstelle der Rand fixiert.

Verwendet die Stack-Sicht, der Unteransicht des `IntrinsicContentSize` Eigenschaft beim Berechnen der Größe Unteransichten entlang der definierten `Axis` (mit Ausnahme der `FillEqually Distribution`). Die `FillEqually Distribution` alle Unteransichten ändert, sodass sie die gleiche Größe, werden daher füllen die Stapelansicht entlang der `Axis`.

Mit Ausnahme von der `Fill Alignment`, verwendet die Stack-Sicht, der Unteransicht des `IntrinsicContentSize` -Eigenschaft für die Berechnung der Ansicht Größe senkrecht zur der angegebenen `Axis`. Für die `Fill Alignment`, alle Unteransichten werden angepasst, sodass sie die senkrecht zur Stapelansicht Ausfüllen der angegebenen `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Positionierung und Größenanpassung der Stapelansicht

Während die Stapelansicht vollständige Kontrolle über das Layout der alle Unteransicht hat (basierend auf Eigenschaften wie z. B. `Axis` und `Distribution`), müssen Sie immer noch zum Positionieren der Ansicht Stack (`UIStackView`) innerhalb ihrer übergeordneten Ansicht mit Automatisches Layout und die Größenklassen.

Im Allgemeinen bedeutet dies, mindestens zwei Kanten der Sicht ein Stapel, Erweiterung und Verkleinerung, definieren daher seine Position anheften. Ohne zusätzlichen Einschränkungen werden die Stapelansicht automatische größenanpassung werden alle zugehörigen Unteransichten wie folgt:

* Die Größe entlang der `Axis` wird die Summe der Größen für alle Unteransicht und eine beliebige Stelle, die zwischen einzelnen Unteransicht definiert wurde.
* Wenn die `LayoutMarginsRelativeArrangement` Eigenschaft `true`, die Größe des Stapels Ansichten umfasst auch Raum für Ränder.
* Die senkrecht zur Größe der `Axis` wird auf die größten Unteransicht in der Auflistung festgelegt werden.

Darüber hinaus können Sie Einschränkungen angeben, für die Stapelansicht **Höhe** und **Breite**. In diesem Fall die Unteransichten angeordnet werden (Größe) den Platz gemäß der Stapelansicht gemäß der `Distribution` und `Alignment` Eigenschaften.

Wenn die `BaselineRelativeArrangement` Eigenschaft `true`, die Unteransichten angeordnet werden basierend auf der ersten oder letzten Unteransicht Baseline, anstatt die **oben**, **unten** oder **Center* -  **Y** Position. Diese werden in die Stapelansicht Inhalt wie folgt berechnet:

* Einem vertikalen Stapel-Ansicht wird der erste Unteransicht für die erste Baseline und der letzte für die letzte zurückgegeben. Wenn eine der folgenden Unteransichten selbst Stack Ansichten sind, wird die erste oder letzte Baseline verwendet werden.
* Einem horizontalen Stapel-Ansicht wird der höchste Unteransicht für die erste und letzte Baseline verwendet werden. Wenn der höchste Ansicht auch ein Stapel ist, wird als Grundlage des höchsten Unteransicht verwendet.

> [!IMPORTANT]
> Baseline-Ausrichtung funktioniert nicht für gestreckt oder komprimiert Unteransicht Größen, wie die Baseline der falschen Position berechnet werden sollen. Für Baseline-Ausrichtung, sicherstellen, dass der Unteransicht des **Höhe** entspricht der systeminterne Inhaltsansicht **Höhe**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Einsatzmöglichkeiten von Stapel anzeigen

Es gibt mehrere Typen aus dem Layout, die gut mit Stapelansicht Steuerelemente funktionieren. Hier sind einige der häufigeren Verwendungsarten, gemäß den Apple:

- **Definieren Sie die Größe entlang der Achse** – durch Anheften von beide Ränder entlang der Stapelansicht `Axis` und einen der angrenzenden Ränder die Position im Stapel festlegen Ansicht wächst die Achse von untergeordneten Ansichten definierte Raum zu passen.
*  **Definieren Sie die Unteransicht des Position** – durch anheften an benachbarte Ränder Stack Ansicht, um die übergeordnete Ansicht, wächst die Stapelansicht in beiden Dimensionen des enthaltenden Unteransichten anpassen.
- **Definieren Sie die Größe und Position des Stapels** – durch Anheften von allen vier Kanten der Stack Ansicht für die übergeordnete Ansicht, ordnet die Stapelansicht die Unteransichten basierend auf den Speicherplatz, der in die Stapelansicht definiert.
*  **Definieren der Größe senkrecht der Achse** – beide Ränder senkrecht an den Stapel-Ansicht anheften `Axis` und einen der Ränder auf der Achse, um die Position im Stapel festzulegen Ansicht senkrecht zur Achse wächst von definierte Raum zu passen die Unteransichten.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Stack-Ansichten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Stack Ansichten in einer Xamarin.tvOS-app ist so fügen sie der app Benutzeroberfläche, die Verwendung des iOS Designers hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**und auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Entwerfen Sie das Layout der einzelnen Elemente, die auf den Stapel-Ansicht hinzugefügt werden: 

    [![](stacked-views-images/layout01.png "Element-Layout-Beispiel")](stacked-views-images/layout01.png#lightbox)
1. Fügen Sie alle erforderlichen Einschränkungen auf die Elemente aus, um sicherzustellen, dass sie ordnungsgemäß skaliert werden. Dieser Schritt ist wichtig, nachdem der Stapel-Ansicht das Element hinzugefügt wurde.
1. Stellen Sie die erforderliche Anzahl von Kopien (vier in diesem Fall): 

    [![](stacked-views-images/layout02.png "Die erforderliche Anzahl von Kopien")](stacked-views-images/layout02.png#lightbox)
1. Ziehen Sie eine **Stapelansicht** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](stacked-views-images/layout03.png "Ein Stapelansicht")](stacked-views-images/layout03.png#lightbox)
1. Wählen Sie in die Stapelansicht der **Widget-Registerkarte** von der **Pad "Eigenschaften"** wählen **füllen** für die **Ausrichtung**, **füllen Gleichermaßen** für die **Verteilung** , und geben Sie `25` für die **Abstand**: 

    [![](stacked-views-images/layout04.png "Die Registerkarte \"Widget\"")](stacked-views-images/layout04.png#lightbox)
1. Positionieren Sie die Stack-Sicht, auf dem Bildschirm, in dem Sie soll, und fügen die Einschränkungen, damit es in den erforderlichen Speicherort bleibt.
1. Wählen Sie die einzelnen Elemente, und ziehen Sie diese in die Stapelansicht: 

    [![](stacked-views-images/layout05.png "Die einzelnen Elemente in die Stapelansicht")](stacked-views-images/layout05.png#lightbox)
1. Das Layout wird angepasst, und die Elemente in die Stapelansicht basierend auf den Attributen, die, denen Sie, oben festlegen, angeordnet.
1. Weisen Sie **Namen** in die **Widget Registerkarte** von der **Eigenschaften-Explorer** zum Arbeiten mit UI-Steuerelemente in C# Code.
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. In der **Projektmappen-Explorer**und auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Entwerfen Sie das Layout der einzelnen Elemente, die auf den Stapel-Ansicht hinzugefügt werden: 

    [![](stacked-views-images/layout01.png "Beispiellayout-element")](stacked-views-images/layout01.png#lightbox)
1. Fügen Sie alle erforderlichen Einschränkungen auf die Elemente aus, um sicherzustellen, dass sie ordnungsgemäß skaliert werden. Dieser Schritt ist wichtig, nachdem der Stapel-Ansicht das Element hinzugefügt wurde.
1. Stellen Sie die erforderliche Anzahl von Kopien (vier in diesem Fall): 

    [![](stacked-views-images/layout02.png "Die erforderliche Anzahl von Kopien")](stacked-views-images/layout02.png#lightbox)
1. Ziehen Sie eine **Stapelansicht** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](stacked-views-images/layout03-vs.png "Ein Stapelansicht")](stacked-views-images/layout03-vs.png#lightbox)
1. Wählen Sie in die Stapelansicht der **Widget-Registerkarte** von der **Eigenschaften-Explorer** wählen **füllen** für die **Ausrichtung**, **füllen Gleichermaßen** für die **Verteilung** , und geben Sie `25` für die **Abstand**: 

    [![](stacked-views-images/layout04-vs.png "Die Registerkarte \"Widget\"")](stacked-views-images/layout04-vs.png#lightbox)
1. Positionieren Sie die Stack-Sicht, auf dem Bildschirm, in dem Sie soll, und fügen die Einschränkungen, damit es in den erforderlichen Speicherort bleibt.
1. Wählen Sie die einzelnen Elemente, und ziehen Sie diese in die Stapelansicht: 

    [![](stacked-views-images/layout05-vs.png "Die einzelnen Elemente in die Stapelansicht")](stacked-views-images/layout05-vs.png#lightbox)
1. Das Layout wird angepasst, und die Elemente in die Stapelansicht basierend auf den Attributen, die, denen Sie, oben festlegen, angeordnet.
1. Weisen Sie **Namen** in die **Widget Registerkarte** von der **Eigenschaften-Explorer** zum Arbeiten mit UI-Steuerelemente in C# Code.
1. Speichern Sie die Änderungen.

-----

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Aktionen wie z. B. `TouchUpInside` auf ein Element der Benutzeroberfläche (z. B. eine `UIButton`) in der iOS-Designer, wenn Sie einen Ereignishandler zu erstellen, wird nie aufgerufen da Apple TV besitzt eine Fingereingabe Bildschirm oder Touch-Ereignissen zu unterstützen. Sie sollten immer die Standardeinstellung verwenden `Action Type` beim Erstellen von Aktionen für TvOS Elemente der Benutzeroberfläche.

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md).

Bei unserem Beispiel haben wir eine Outlet und Aktion für das Segment-Steuerelement und eines Outlets für jede Karte"Player" verfügbar gemacht. Im Code müssen wir ausblenden und Player basierend auf das aktuelle Segment anzuzeigen. Zum Beispiel:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

Wenn die app ausgeführt wird, werden vier Elemente in unserer Stapelansicht gleichmäßig verteilt werden:

[![](stacked-views-images/layout06.png "Wenn die app ausgeführt wird, werden die vier Elemente gleichmäßig in unserer Stapelansicht verteilt")](stacked-views-images/layout06.png#lightbox)

Wenn die Anzahl der Spieler verringert wird, wird die nicht verwendeten Ansichten ausgeblendet sind, und die Stapelansicht passen Sie das Layout anpassen:

[![](stacked-views-images/layout07.png "Die Anzahl der Spieler verringert wird, die nicht verwendeten Ansichten sind ausgeblendet, und die Stapelansicht passen Sie das Layout anpassen")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Auffüllen einer Stapelansicht aus Code

Zusätzlich zum Definieren von Inhalt und Layout einer Stapel-Ansicht vollständig im iOS-Designer, Sie erstellen und entfernen Sie sie dynamisch aus C# Code.

Führen Sie das folgende Beispiel, mit denen eine Stapelansicht "Sternen" in einem Review (1 bis 5) behandelt:

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

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Inhalt dynamisch ändern

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

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit gestapelte Ansicht, in ein Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
