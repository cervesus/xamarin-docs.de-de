---
title: Arbeiten mit tvos. außerdem wurden gestapelte Ansichten in Xamarin
description: Dieses Dokument beschreibt, wie tvos. außerdem wurden gestapelte Sichten in einer app mit Xamarin erstellten gearbeitet wird. Es gibt einen Überblick über gestapelte Ansichten und erläutert, automatisches Layout, Position und Größe ein gestapeltes anzeigen, häufig verwendet, Integration mit Storyboards und mehr.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7e718e525c23e78fbf846209602a07bf0f3f386e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789371"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Arbeiten mit tvos. außerdem wurden gestapelte Ansichten in Xamarin

Der Stapel Ansichtssteuerelement (`UIStackView`) nutzt die Funktionen von Automatisches Layout und Klassen Größe einen Stapel mit Unteransichten, horizontal oder vertikal zu verwalten, das dynamisch von Änderungen am Inhalt und der Bildschirmgröße des Geräts Apple TV-Werten.

Das Layout der alle Unteransichten an eine Stapelansicht angefügt vom Entwickler definierten Eigenschaften wie z. B. Achse "," Verteilung "," Ausrichtung "und" Abstand Grundlage verwaltet werden:

[![](stacked-views-images/stacked01.png "Unteransicht Layout für Diagramm")](stacked-views-images/stacked01.png#lightbox)

Bei Verwendung einer `UIStackView` in einer app Xamarin.tvOS Definieren der Entwickler kann den Unteransichten entweder in eine Storyboard in der iOS-Designer oder durch Hinzufügen und Entfernen von Unteransichten in C#-Code.

## <a name="about-stacked-view-controls"></a>Informationen zu Steuerelementen gestapelt anzeigen 

Die `UIStackView` dient als für nicht-Rendering-Containeransicht und daher ist es nicht gezeichnet auf den Zeichenbereich wie andere Unterklassen des `UIView`. Festlegen von Eigenschaften wie z. B. `BackgroundColor` oder überschreiben `DrawRect` keine visuellen Auswirkungen.

Es gibt verschiedene Eigenschaften, die steuern, wie eine Stapelansicht mitfinanziert Unteransichten anordnen möchten:

- **Achse** – bestimmt, ob die Stapelansicht entweder die Unteransichten ordnet **horizontal** oder **vertikal**.
- **Ausrichtung** – steuert, wie die Unteransichten innerhalb der Stapelansicht ausgerichtet werden.
- **Verteilung** – steuert, wie die Unteransichten in die Stapelansicht angepasst werden.
- **Abstand** – den minimalen Abstand zwischen den einzelnen Unteransicht in die Stapelansicht steuert.
- **Baseline-Relative** – Wenn `true`, der vertikale Abstand von jeder Unteransicht gegenüber der Basislinie abgeleitet wird.
- **Layout Ränder Relative** – die Unteransichten relativ zum Standardlayout Ränder platziert.

In der Regel verwenden Sie eine Stapelansicht eine kleine Anzahl von Unteransichten anordnen. Komplexere Benutzeroberflächen können erstellt werden, von einer oder mehreren Stapel Ansichten ineinander schachteln.

Sie können die Benutzeroberflächen-Darstellung weiter optimieren, indem die Unteransichten (z. B. zum Steuerelement die Höhe oder Breite) zusätzliche Einschränkungen hinzugefügt. Allerdings sollten Sie vorsichtig sein, nicht in Konflikt stehenden Einschränkungen, die durch die Stapelansicht selbst eingeführten aufzunehmen.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Automatisches Layout und Größenklassen

Wenn eine Stapelansicht eine Unteransicht hinzugefügt wird ist das zugehörige Layout vollständig durch, automatisches Layout und Klassen Größe, position und Größe der angeordneten Ansichten mit Stapelansicht gesteuert.

Wird die Stapelansicht _Pin_ den Unteransicht vor- und Nachnamen in der Auflistung, an der **oben** und **unteren** Kanten für vertikalen Stapel Ansichten oder die **Links**und **rechts** Kanten für horizontale Stapel Ansichten. Wenn Sie festlegen, die `LayoutMarginsRelativeArrangement` Eigenschaft, um `true`, und klicken Sie dann die Ansicht der Unteransichten auf die relevanten Ränder anstelle der Kante pins.

Die Stapelansicht verwendet den Unteransicht `IntrinsicContentSize` Eigenschaft beim Berechnen der Größe Unteransichten entlang des definierten `Axis` (mit Ausnahme der `FillEqually Distribution`). Die `FillEqually Distribution` ändert die Größe aller Unteransichten, damit sie die gleiche Größe sind daher füllen die Stapelansicht entlang der `Axis`.

Mit Ausnahme von der `Fill Alignment`, die Stapelansicht verwendet den Unteransicht `IntrinsicContentSize` Eigenschaft für die die Sicht Größe senkrecht zur Berechnung der angegebenen `Axis`. Für die `Fill Alignment`, alle Unteransichten haben eine Größe von, damit sie die Stapelansicht senkrecht zum Füllen der angegebenen `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Position und Größe der Stapelansicht

Während die Stapelansicht die vollständige Kontrolle über das Layout der alle Unteransicht hat (basierend auf Eigenschaften wie z. B. `Axis` und `Distribution`), müssen Sie dennoch die Stapelansicht positionieren (`UIStackView`) innerhalb ihrer übergeordneten Ansicht mit Automatisches Layout und Klassen Größe.

Im Allgemeinen bedeutet dies, mindestens zwei Kanten der Sicht ein Stapel, Erweiterung und Verkleinerung daher definieren seiner Position anheften. Ohne zusätzlichen Einschränkungen wird die Stapelansicht automatisch angepasst werden alle seine Unteransichten wie folgt:

* Die Größe entlang seiner `Axis` werden die Summe der Größen für alle Unteransicht und Leerzeichen, die zwischen jeder Unteransicht definiert wurde.
* Wenn die `LayoutMarginsRelativeArrangement` Eigenschaft ist `true`, die Größe des Stapels Ansichten gehören auch Raum für Ränder.
* Die senkrecht zur Größe der `Axis` festgelegt, um die größten Unteransicht in der Auflistung.

Darüber hinaus können Sie Einschränkungen angeben, für die Stapelansicht **Höhe** und **Breite**. In diesem Fall die Unteransichten angeordnet werden (Größe) um den durch die Stapelansicht gemäß angegebene Platz Ausfüllen der `Distribution` und `Alignment` Eigenschaften.

Wenn die `BaselineRelativeArrangement` Eigenschaft ist `true`, die Unteransichten angeordnet werden basierend auf der ersten oder letzten Unteransicht Basislinie ruht, anstatt die **oben**, **unteren** oder **Center* -  **Y** Position. Diese werden auf die Stapelansicht Inhalt wie folgt berechnet:

* Eine vertikale Stapelansicht gibt die erste Unteransicht für die erste Baseline und der letzte für den letzten zurück. Wenn eine der folgenden Unteransichten selbst Stapel Ansichten sind, wird ihre erste oder letzte Basislinie verwendet werden.
* Eine horizontale Stapelansicht wird die höchste Unteransicht des vor- und Nachnamen Basislinien verwenden. Wenn der höchste Ansicht auch ein Stapel ist, wird als Grundlage des höchsten Unteransicht verwendet.

> [!IMPORTANT]
> Baseline-Ausrichtung funktioniert gestreckt oder komprimiert Unteransicht Größen nicht wie die Grundlinie der falschen Position berechnet wird. Für Baseline-Ausrichtung sicher, dass den Unteransicht **Höhe** entspricht der systeminternen Inhaltsansicht **Höhe**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Allgemeine Verwendungsmöglichkeiten der Stapel anzeigen

Es gibt mehrere Layout-Typen, die Stapelansicht Steuerelementen geeignet. Gemäß den Apple sind hier einige der häufigeren Verwendungen:

- **Definieren der Größe entlang der Achse** – beide Ränder entlang der Stapelansicht anheften `Axis` und eine der angrenzenden Kanten festzulegende Position im Stapel, Ansicht entlang der Achse durch seine Unteransichten definierten Platz wächst.
*  **Definieren Sie den Unteransicht Position** – anheften an benachbarte Rändern eines übergeordneten Ansicht der Stapelansicht, wächst die Stapelansicht in beiden Dimensionen des enthaltenden Unteransichten anpassen.
- **Definieren Sie die Größe und Position des Stapels** – anheften alle vier Ränder der Stapelansicht für die übergeordnete Ansicht, ordnet die Stapelansicht der Unteransichten basierend auf den Speicherplatz, der in die Stapelansicht definiert.
*  **Definieren der Größe Rechtwinklige Achse** – beide senkrecht Ränder die Stapelansicht anheften `Axis` und eine der Kanten entlang der Achse zum Festlegen der Position, den Stapel, Ansicht der Achse wächst durch definierten Platz senkrecht, seine Unteransichten.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Stack-Ansichten und Storyboards

Die einfachste Möglichkeit zur Bearbeitung von Stapel Sichten in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Lösung Pad**und auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Entwerfen Sie das Layout Ihrer einzelner Elemente, die Sie der Stapelansicht hinzufügen möchten: 

    [![](stacked-views-images/layout01.png "Layout (Beispiel)")](stacked-views-images/layout01.png#lightbox)
1. Fügen Sie alle erforderlichen Einschränkungen hinzu, um die Elemente aus, um sicherzustellen, dass sie ordnungsgemäß skaliert. Dieser Schritt ist wichtig, sobald die Stapelansicht das Element hinzugefügt wird.
1. Stellen Sie die erforderliche Anzahl von Kopien (vier in diesem Fall): 

    [![](stacked-views-images/layout02.png "Die erforderliche Anzahl von Kopien")](stacked-views-images/layout02.png#lightbox)
1. Ziehen Sie eine **Stapelansicht** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](stacked-views-images/layout03.png "Eine Stapelansicht")](stacked-views-images/layout03.png#lightbox)
1. Wählen Sie in die Stapelansicht der **Registerkarte "Widget"** von der **Eigenschaften aufgefüllt** wählen **füllen** für die **Ausrichtung**, **ausfüllen Gleichermaßen** für die **Verteilung** , und geben Sie `25` für die **Abstand**: 

    [![](stacked-views-images/layout04.png "Die Registerkarte \"Widget\"")](stacked-views-images/layout04.png#lightbox)
1. Positionieren Sie die Stapelansicht auf dem Bildschirm, in dem Sie soll, und fügen die Einschränkungen, um in den erforderlichen Speicherort zu gewährleisten.
1. Wählen Sie die einzelnen Elemente, und ziehen Sie sie in die Stapelansicht: 

    [![](stacked-views-images/layout05.png "Die einzelnen Elemente in die Stapelansicht")](stacked-views-images/layout05.png#lightbox)
1. Das Layout angepasst werden, und die Elemente werden in die Stapelansicht basierend auf den oben festgelegten Attributen angeordnet werden.
1. Weisen Sie **Namen** in der **Registerkarte "Widget"** von der **Eigenschaften-Explorer** mit UI-Steuerelemente in C#-Code arbeiten.
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. In der **Projektmappen-Explorer**und auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Entwerfen Sie das Layout Ihrer einzelner Elemente, die Sie der Stapelansicht hinzufügen möchten: 

    [![](stacked-views-images/layout01.png "Beispiellayout für element")](stacked-views-images/layout01.png#lightbox)
1. Fügen Sie alle erforderlichen Einschränkungen hinzu, um die Elemente aus, um sicherzustellen, dass sie ordnungsgemäß skaliert. Dieser Schritt ist wichtig, sobald die Stapelansicht das Element hinzugefügt wird.
1. Stellen Sie die erforderliche Anzahl von Kopien (vier in diesem Fall): 

    [![](stacked-views-images/layout02.png "Die erforderliche Anzahl von Kopien")](stacked-views-images/layout02.png#lightbox)
1. Ziehen Sie eine **Stapelansicht** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](stacked-views-images/layout03-vs.png "Eine Stapelansicht")](stacked-views-images/layout03-vs.png#lightbox)
1. Wählen Sie in die Stapelansicht der **Registerkarte "Widget"** von der **Eigenschaften-Explorer** wählen **füllen** für die **Ausrichtung**, **ausfüllen Gleichermaßen** für die **Verteilung** , und geben Sie `25` für die **Abstand**: 

    [![](stacked-views-images/layout04-vs.png "Die Registerkarte \"Widget\"")](stacked-views-images/layout04-vs.png#lightbox)
1. Positionieren Sie die Stapelansicht auf dem Bildschirm, in dem Sie soll, und fügen die Einschränkungen, um in den erforderlichen Speicherort zu gewährleisten.
1. Wählen Sie die einzelnen Elemente, und ziehen Sie sie in die Stapelansicht: 

    [![](stacked-views-images/layout05-vs.png "Die einzelnen Elemente in die Stapelansicht")](stacked-views-images/layout05-vs.png#lightbox)
1. Das Layout angepasst werden, und die Elemente werden in die Stapelansicht basierend auf den oben festgelegten Attributen angeordnet werden.
1. Weisen Sie **Namen** in der **Registerkarte "Widget"** von der **Eigenschaften-Explorer** mit UI-Steuerelemente in C#-Code arbeiten.
1. Speichern Sie die Änderungen.

-----

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Aktionen wie z. B. `TouchUpInside` auf ein Element der Benutzeroberfläche (z. B. eine `UIButton`) in der iOS-Designer, wenn Sie einen Ereignishandler erstellen, nie aufgerufen wird da Apple TV Fingereingabe Bildschirm oder Berührungsereignisse unterstützen keine. Sie sollten immer die Standardeinstellung verwenden `Action Type` beim Erstellen von Aktionen für tvos. außerdem wurden Elemente der Benutzeroberfläche.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md).

Bei unserem Beispiel haben wir eine Steckdose und die Aktion für das Segment-Steuerelement und ein Ausgang für jede Karte"Player" verfügbar gemacht. Im Code werden ausblenden und anzeigen Player basierend auf das aktuelle Segment. Zum Beispiel:

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

Wenn die app ausgeführt wird, werden die vier Elemente in unserer Stapelansicht gleichmäßig verteilt werden:

[![](stacked-views-images/layout06.png "Wenn die app ausgeführt wird, werden die vier Elemente gleichmäßig in unserer Stapelansicht verteilt")](stacked-views-images/layout06.png#lightbox)

Wenn die Anzahl der Spieler verringert wird, die nicht verwendeten Ansichten ausgeblendet, und die Stapelansicht passen Sie das Layout anpassen:

[![](stacked-views-images/layout07.png "Wenn die Anzahl der Spieler verringert wird, die nicht verwendeten Ansichten ausgeblendet, und die Stapelansicht passen Sie das Layout anpassen")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Auffüllen einer Stapelansicht aus Code

Sie können zusätzlich zum Definieren von die Inhalt und das Layout einer Stapelansicht vollständig in der iOS-Designer, erstellen und dynamisch vom C#-Code entfernen.

Führen Sie das folgende Beispiel, mit denen eine Stapelansicht "Sterne" in eine Überprüfung (1 bis 5) behandelt:

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

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Inhalt dynamisch ändern

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

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Verwenden von innerhalb einer app Xamarin.tvOS gestapelt anzeigen.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
