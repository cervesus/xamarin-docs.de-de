---
title: Arbeiten mit gestapelten Ansichten von tvos in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit tvos-gestapelten Ansichten in einer mit xamarin erstellten App arbeiten. Er bietet eine allgemeine Übersicht über gestapelte Ansichten und erläutert das automatische Layout, die Positionierung und die Größe einer gestapelten Ansicht, häufige Verwendungsmöglichkeiten, die Integration in Storyboards und vieles mehr.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 646a26b4c9b2d44595315a6a32b7294b18c42d7a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648962"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Arbeiten mit gestapelten Ansichten von tvos in xamarin

Das Stapel Ansicht-Steuer`UIStackView`Element () nutzt die Leistungsfähigkeit von automatischen layoutklassen und Größenklassen, um einen Stapel von unter Ansichten entweder horizontal oder vertikal zu verwalten, der dynamisch auf Inhalts Änderungen und Bildschirmgröße des Apple TV-Geräts antwortet.

Das Layout aller untergeordneten Sichten, die an eine Stapel Ansicht angefügt sind, wird von der Anwendung basierend auf vom Entwickler definierten Eigenschaften wie Achse, Verteilung, Ausrichtung und Abstand verwaltet:

[![](stacked-views-images/stacked01.png "Subview-Layoutdiagramm")](stacked-views-images/stacked01.png#lightbox)

Wenn Sie `UIStackView` in einer xamarin. tvos-App verwenden, kann der Entwickler entweder die unter Ansichten entweder innerhalb eines Storyboards im IOS-Designer oder durch Hinzufügen und Entfernen von unter Ansichten C# im Code definieren.

## <a name="about-stacked-view-controls"></a>Informationen zu gestapelten Sicht Steuerelementen 

Der `UIStackView` ist als nicht Rendering-Container Ansicht konzipiert und wird daher nicht wie andere Unterklassen von `UIView`in den Zeichenbereich gezeichnet. Das Festlegen von Eigenschaften `BackgroundColor` , z `DrawRect` . b. oder überschreiben, hat keinen visuellen Effekt.

Es gibt mehrere Eigenschaften, mit denen gesteuert wird, wie eine Stapel Ansicht ihre Auflistung von untergeordneten Sichten anordnet:

- **Axis** – bestimmt, ob die untergeordneten Sichten in der Stapel Ansicht entweder **Horizontal** oder **vertikal angeordnet**werden.
- **Alignment** – steuert, wie die untergeordneten Sichten innerhalb der Stapel Ansicht ausgerichtet werden.
- **Distribution** – steuert, wie die untergeordneten Sichten in der Stapel Ansicht skaliert werden.
- **Abstand** – steuert den minimalen Abstand zwischen jeder unter Ansicht in der Stapel Ansicht.
- **Baseline relative** – Wenn `true`der Wert ist, wird der vertikale Abstand jeder unter Ansicht von der Basislinie abgeleitet.
- **Layoutränder relativ** – platziert die untergeordneten Sichten in Relation zu den standardmäßigen Layouträndern.

In der Regel wird eine Stapel Ansicht verwendet, um eine kleine Anzahl von unter Sichten anzuordnen. Komplexere Benutzeroberflächen können erstellt werden, indem eine oder mehrere Stapel Ansichten ineinander geschachtelt werden.

Sie können die Benutzeroberflächen Darstellung weiter optimieren, indem Sie den unter Ansichten zusätzliche Einschränkungen hinzufügen (z. b. um die Höhe oder Breite zu steuern). Allerdings sollte darauf geachtet werden, dass in Konflikt stehende Einschränkungen nicht zu den von der Stapel Ansicht selbst eingeführten Einschränkungen gehören.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Klassen für automatisches Layout und Größe

Wenn eine unter Ansicht zu einer Stapel Ansicht hinzugefügt wird, wird das Layout vollständig von dieser Stapel Ansicht gesteuert, wobei die Klassen für automatisches Layout und Größe verwendet werden, um die angeordneten Sichten zu positionieren und deren Größe

In der Stapel Ansicht _wird die_ erste und letzte untergeordnete Sicht in der Auflistung an den **oberen** und **unteren** Rand für vertikale Stapel Ansichten oder den **linken** und **rechten** Rand für horizontale Stapel Ansichten angeheftet. Wenn Sie die `LayoutMarginsRelativeArrangement` -Eigenschaft auf `true`festlegen, werden die untergeordneten Sichten in der Ansicht an die relevanten Ränder anstatt an den Rand angeheftet.

Die Stapel Ansicht verwendet die-Eigenschaft der `IntrinsicContentSize` untergeordneten Ansicht beim Berechnen der untergeordneten Ansichts `Axis` Größe entlang der definierten `FillEqually Distribution`(mit Ausnahme von). Die `FillEqually Distribution` ändert die Größe aller untergeordneten Sichten, sodass Sie die gleiche Größe haben. Dadurch wird die Stapel Ansicht `Axis`entlang des aufgefüllt.

Mit Ausnahme von `Fill Alignment`wird in der Stapel Ansicht die- `IntrinsicContentSize` Eigenschaft der unter Ansicht verwendet, um die Größe der Sicht senkrecht zum angegebenen `Axis`zu berechnen. Bei sind alle untergeordneten Sichten so groß, dass Sie die Stapel Ansicht senkrecht zum angegebenen `Axis`auffüllen. `Fill Alignment`

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Positionieren und Anpassen der Stapel Ansicht

Die Stapel Ansicht hat die gesamte Kontrolle über das Layout einer beliebigen unter Ansicht (basierend auf Eigenschaften wie `Axis` und `Distribution`), aber Sie müssen die Stapel Ansicht (`UIStackView`) in der übergeordneten Ansicht weiterhin mithilfe der Klassen für automatisches Layout und Größe positionieren.

Im Allgemeinen bedeutet dies, dass mindestens zwei Ränder der Stapel Ansicht angeheftet werden, um Sie zu erweitern und zu verkleinern und so die Position zu definieren. Ohne zusätzliche Einschränkungen wird die Stapel Ansicht automatisch so angepasst, dass Sie alle untergeordneten Sichten wie folgt einfügt:

* Die Größe der unter `Axis` geordneten Ansichts Größe und des zwischen den einzelnen untergeordneten Sichten definierten Speicherplatzes ist gleich.
* Wenn die `LayoutMarginsRelativeArrangement` -Eigenschaft `true`ist, enthalten die Stapel Ansichts Größe auch Platz für die Ränder.
* Die Größe, die senkrecht `Axis` zum ist, wird auf die größte unter Ansicht in der Auflistung festgelegt.

Darüber hinaus können Sie Einschränkungen für die **Höhe** und **Breite**der Stapel Ansicht angeben. In diesem Fall werden die untergeordneten Sichten angeordnet (mit der Größenangabe), um den von der Stapel Ansicht festgelegten Platz entsprechend der `Distribution` - `Alignment` Eigenschaft und der-Eigenschaft auszufüllen.

Wenn die `BaselineRelativeArrangement` -Eigenschaft `true`auf festgelegt ist, werden die untergeordneten Sichten basierend auf der Baseline der ersten oder letzten unter Ansicht angelegt, anstatt die **obere**, **untere** oder **zentrierte*- **Y** -Position zu verwenden. Diese werden im Inhalt der Stapel Ansicht wie folgt berechnet:

* Eine vertikale Stapel Ansicht gibt die erste unter Ansicht für die erste Baseline und die letzte für den letzten zurück. Wenn eine dieser unter Sichten selbst Stapel Ansichten ist, wird Ihre erste oder letzte Baseline verwendet.
* In einer horizontalen Stapel Ansicht wird die höchste unter Ansicht für die erste und die letzte Baseline verwendet. Wenn die höchste Ansicht auch eine Stapel Ansicht ist, wird Sie mit der höchsten unter Ansicht als Baseline verwendet.

> [!IMPORTANT]
> Die Baseline-Ausrichtung funktioniert nicht bei gestreckten oder komprimierten untergeordneten Ansichts Größen, da die Baseline an die falsche Position berechnet wird. Stellen Sie bei der Baseline-Ausrichtung sicher, dass die **Höhe** der unter Ansicht mit der **Höhe**der intrinsischen Inhaltsansicht übereinstimmt.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Allgemeine Stapel Ansicht verwendet

Es gibt mehrere Layouttypen, die gut mit Stapel Ansicht-Steuerelementen funktionieren. Nachfolgend finden Sie einige der gängigeren Verwendungsmöglichkeiten:

- **Definieren Sie die Größe entlang der Achse** – indem Sie beide Ränder entlang der Stapel Ansicht `Axis` und einen der angrenzenden Ränder anheten, um die Position festzulegen, vergrößert sich die Stapel Ansicht entlang der Achse, damit Sie an den durch die untergeordneten Sichten definierten Platz passt.
*  **Legen Sie die Position der untergeordneten Ansicht** fest – indem Sie an angrenzende Ränder der Stapel Ansicht an die übergeordnete Ansicht angehefteten, wird die Stapel Ansicht in beiden Dimensionen vergrößert, sodass Sie mit den untergeordneten Sichten verknüpft ist.
- **Definieren der Größe und Position des Stapels** – durch anheten aller vier Ränder der Stapel Ansicht an die übergeordnete Ansicht ordnet die Stapel Ansicht die unter Ansichten auf der Grundlage des in der Stapel Ansicht definierten Speicherplatzes an.
*  **Definieren Sie die Größe der Achse senkrecht** – indem Sie beide Ränder senkrecht an die Stapel Ansicht `Axis` und einen der Kanten entlang der Achse anheken, um die Position festzulegen, vergrößert sich die Stapel Ansicht senkrecht zur Achse, damit Sie an den von den untergeordneten Sichten definierten Bereich angepasst wird.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Stapel Ansichten und Storyboards

Die einfachste Möglichkeit, mit Stapel Ansichten in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Entwerfen Sie das Layout der einzelnen Elemente, die der Stapel Ansicht hinzugefügt werden sollen: 

    [![](stacked-views-images/layout01.png "Beispiel für Element Layout")](stacked-views-images/layout01.png#lightbox)
1. Fügen Sie den Elementen alle erforderlichen Einschränkungen hinzu, um sicherzustellen, dass Sie korrekt skaliert werden Dieser Schritt ist wichtig, wenn das Element der Stapel Ansicht hinzugefügt wird.
1. Nehmen Sie die erforderliche Anzahl von Kopien (in diesem Fall vier) vor: 

    [![](stacked-views-images/layout02.png "Die erforderliche Anzahl von Kopien")](stacked-views-images/layout02.png#lightbox)
1. Ziehen Sie eine **Stapel Ansicht** aus der **Toolbox** , und legen Sie Sie in der Ansicht ab: 

    [![](stacked-views-images/layout03.png "Eine Stapel Ansicht")](stacked-views-images/layout03.png#lightbox)
1. Wählen Sie die Stapel Ansicht aus, und wählen Sie auf der **Registerkarte widget** des **Eigenschaftenpad** für die **Ausrichtung** **Ausfüllen** aus , und geben `25` Sie für die Verteilung **gleichermaßen** **ein:** 

    [![](stacked-views-images/layout04.png "Die Widget-Registerkarte")](stacked-views-images/layout04.png#lightbox)
1. Positionieren Sie die Stapel Ansicht auf dem Bildschirm, wo Sie Sie möchten, und fügen Sie Einschränkungen hinzu, um Sie am erforderlichen Speicherort zu speichern.
1. Wählen Sie die einzelnen Elemente aus, und ziehen Sie Sie in die Stapel Ansicht: 

    [![](stacked-views-images/layout05.png "Die einzelnen Elemente in der Stapel Ansicht")](stacked-views-images/layout05.png#lightbox)
1. Das Layout wird angepasst, und die Elemente werden in der Stapel Ansicht basierend auf den Attributen, die Sie oben festgelegt haben, angeordnet.
1. Weisen Sie im **Eigenschaften-Explorer** auf der **Registerkarte widget** **Namen** zu, um mit Ihren C# UI-Steuerelementen im Code zu arbeiten.
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Entwerfen Sie das Layout der einzelnen Elemente, die der Stapel Ansicht hinzugefügt werden sollen: 

    [![](stacked-views-images/layout01.png "Beispiel für Element Layout")](stacked-views-images/layout01.png#lightbox)
1. Fügen Sie den Elementen alle erforderlichen Einschränkungen hinzu, um sicherzustellen, dass Sie korrekt skaliert werden Dieser Schritt ist wichtig, wenn das Element der Stapel Ansicht hinzugefügt wird.
1. Nehmen Sie die erforderliche Anzahl von Kopien (in diesem Fall vier) vor: 

    [![](stacked-views-images/layout02.png "Die erforderliche Anzahl von Kopien")](stacked-views-images/layout02.png#lightbox)
1. Ziehen Sie eine **Stapel Ansicht** aus der **Toolbox** , und legen Sie Sie in der Ansicht ab: 

    [![](stacked-views-images/layout03-vs.png "Eine Stapel Ansicht")](stacked-views-images/layout03-vs.png#lightbox)
1. Wählen Sie die Stapel Ansicht aus, wählen Sie im **Eigenschaften-Explorer** auf der **Registerkarte widget** die Option **Ausfüllen** für die **Ausrichtung**aus **, und geben** `25` Sie für den **Abstand** **gleich** ein: 

    [![](stacked-views-images/layout04-vs.png "Die Widget-Registerkarte")](stacked-views-images/layout04-vs.png#lightbox)
1. Positionieren Sie die Stapel Ansicht auf dem Bildschirm, wo Sie Sie möchten, und fügen Sie Einschränkungen hinzu, um Sie am erforderlichen Speicherort zu speichern.
1. Wählen Sie die einzelnen Elemente aus, und ziehen Sie Sie in die Stapel Ansicht: 

    [![](stacked-views-images/layout05-vs.png "Die einzelnen Elemente in der Stapel Ansicht")](stacked-views-images/layout05-vs.png#lightbox)
1. Das Layout wird angepasst, und die Elemente werden in der Stapel Ansicht basierend auf den Attributen, die Sie oben festgelegt haben, angeordnet.
1. Weisen Sie im **Eigenschaften-Explorer** auf der **Registerkarte widget** **Namen** zu, um mit Ihren C# UI-Steuerelementen im Code zu arbeiten.
1. Speichern Sie die Änderungen.

-----

> [!IMPORTANT]
> Obwohl es möglich ist, Aktionen wie z `TouchUpInside` . b. einem Benutzeroberflächen Element (z `UIButton`. b.) im IOS-Designer zuzuweisen, wenn ein Ereignis Handler erstellt wird, wird er nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Beim Erstellen von Aktionen für tvos-Benutzeroberflächen Elemente sollte immer der Standard `Action Type` Wert verwendet werden.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

Im vorliegenden Beispiel haben wir ein Outlet und eine Aktion für das Segment Steuerelement und ein Outlet für jede "Player Karte" verfügbar gemacht. Im Code wird der Spieler basierend auf dem aktuellen Segment ausgeblendet und angezeigt. Beispiel:

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

Wenn die app ausgeführt wird, werden die vier Elemente gleichmäßig in der Stapel Ansicht verteilt:

[![](stacked-views-images/layout06.png "Wenn die app ausgeführt wird, werden die vier Elemente gleichmäßig in der Stapel Ansicht verteilt.")](stacked-views-images/layout06.png#lightbox)

Wenn die Anzahl der Spieler verringert wird, werden die nicht verwendeten Sichten ausgeblendet, und in der Stapel Ansicht wird das Layout angepasst und angepasst:

[![](stacked-views-images/layout07.png "Wenn die Anzahl der Spieler verringert wird, werden die nicht verwendeten Sichten ausgeblendet, und die Stapel Ansicht passt das Layout an")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Auffüllen einer Stapel Ansicht aus Code

Zusätzlich zur vollständig Definition der Inhalte und des Layouts einer Stapel Ansicht im IOS-Designer können Sie diese dynamisch aus C# dem Code erstellen und entfernen.

Verwenden Sie das folgende Beispiel, in dem eine Stapel Ansicht verwendet wird, um "Stars" in einer Überprüfung (1 bis 5) zu behandeln:

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

Werfen wir einen Blick auf einige Teile dieses Codes im Detail. Zuerst verwenden wir eine `if` -Anweisung, um zu überprüfen, ob es nicht mehr als fünf "Sterne" oder kleiner als 0 (null) ist.

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

Sie werden feststellen, dass der `UIImageView` `UIStackView`-Eigenschaft der `ArrangedSubviews` -Eigenschaft und nicht `SubView`der-Eigenschaft hinzugefügt wurde. Jede Ansicht, deren Layout in der Stapel Ansicht gesteuert werden soll, muss der `ArrangedSubviews` -Eigenschaft hinzugefügt werden.

Zum Entfernen einer unter Ansicht aus einer Stapel Ansicht wird zunächst die zu entfern gende unter Ansicht angezeigt:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Anschließend müssen wir Sie aus der `ArrangedSubviews` Auflistung und der Super Ansicht entfernen:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Wenn Sie eine unter Ansicht aus nur `ArrangedSubviews` der Auflistung entfernen, wird Sie aus dem Steuerelement der Stapel Ansicht entfernt, jedoch nicht vom Bildschirm entfernt.

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Dynamisches Ändern von Inhalten

Eine Stapel Ansicht passt automatisch das Layout der untergeordneten Sichten an, wenn eine unter Ansicht hinzugefügt, entfernt oder ausgeblendet wird. Das Layout wird auch angepasst, wenn eine Eigenschaft der Stapel Ansicht angepasst wird (z `Axis`. b. die).

Layoutänderungen können animiert werden, indem Sie in einem Animations Block platziert werden, z. b.:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Viele der Eigenschaften der Stapel Ansicht können mithilfe von Größenklassen innerhalb eines Storyboards angegeben werden. Diese Eigenschaften werden automatisch animiert, wenn die Größe oder die Ausrichtung geändert wird.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit der gestapelten Ansicht in einer xamarin. tvos-App behandelt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
