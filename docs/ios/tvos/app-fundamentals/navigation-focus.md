---
title: Arbeiten mit der tvos-Navigation und dem Fokus in xamarin
description: In diesem Artikel wird das Konzept des Fokus behandelt und erläutert, wie es verwendet wird, um die Navigation in einer xamarin. tvos-App darzustellen und zu verarbeiten.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: d9e8d91b03a5a82373012da215bd29a747e67d3e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939450"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Arbeiten mit der tvos-Navigation und dem Fokus in xamarin

_In diesem Artikel wird das Konzept des Fokus behandelt und erläutert, wie es verwendet wird, um die Navigation in einer xamarin. tvos-App darzustellen und zu verarbeiten._

In diesem Artikel wird das Konzept des [Fokus](#Focus-and-Selection) behandelt und erläutert, wie es verwendet wird, um die [Navigation](#Navigation) in einer xamarin. tvos-App-Benutzeroberfläche zu verarbeiten. Wir untersuchen, wie die integrierten tvos-Navigations Steuerelemente Fokus, Hervorhebung und Auswahl verwenden, um die Navigation in der Benutzeroberfläche der xamarin. tvos-App bereitzustellen.

[![Navigation in der Benutzeroberfläche von tvos apps](navigation-focus-images/intro01.png)](navigation-focus-images/intro01.png#lightbox)

Als nächstes sehen wir uns an, wie der [Fokus mit unter](#Focus-and-Parallax) schiedlichen und *geschichteten Bildern* verwendet werden kann, um visuelle Hinweise für den aktuellen Navigations Zustand für den Endbenutzer bereitzustellen.

Schließlich sehen wir uns die Arbeit mit dem [Fokus](#Working-with-Focus), [Fokus Updates](#Working-with-Focus-Updates), [Fokus](#Working-with-Focus-Guides)Handbücher, den [Fokus auf](#Working-with-Focus-in-Collections) Auflistungen und das Aktivieren von " [Parser](#enabling-parallax) " in der xamarin. tvos-APP an.

<a name="Navigation"></a>

## <a name="navigation"></a>Navigation

Benutzer Ihrer xamarin. tvos-App interagieren nicht direkt mit ihrer Schnittstelle wie IOS, wo Sie auf Bilder auf dem Bildschirm des Geräts tippen, aber indirekt über den Raum mithilfe von [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Sie müssen dies berücksichtigen, wenn Sie die Benutzeroberfläche Ihrer APP so entwerfen, dass Sie sich auf natürliche Weise weiterentwickelt, während der Benutzer die Apple TV-Oberfläche beibehält.

Eine erfolgreiche tvos-App implementiert die Navigation auf eine Weise, die den Zweck der APP und die Struktur der Daten, die Sie darstellt, problemlos unterstützt, ohne dass auf die Navigation selbst geachtet wird. Entwerfen Sie Ihre Navigation so, dass Sie sich auf natürliche und vertraute Weise Verhalten kann, ohne die Benutzeroberfläche zu dominieren oder den Fokus von der Inhalte und der App-Benutzeroberfläche zu ziehen.

[![Die tvos-Einstellungs-APP](navigation-focus-images/nav01.png)](navigation-focus-images/nav01.png#lightbox)

Bei der Verwendung eines Apple TV navigiert der Benutzer in der Regel durch eine gestapelte Reihe von Bildschirmen, die jeweils einen bestimmten Satz von Inhalten darstellen. Jeder neue Bildschirm kann wiederum zu einem oder mehreren untergeordneten Bildschirmen mit Standard Steuerelementen der Benutzeroberfläche, z. b. [Schalt](~/ios/tvos/user-interface/buttons.md)Flächen, [Register](~/ios/tvos/user-interface/tab-bars.md)Karten, Tabellen, [Sammlungs Ansichten](~/ios/tvos/user-interface/collection-views.md) oder [geteilte Sichten](~/ios/tvos/user-interface/split-views.md), führen.

Mit jedem neuen Bildschirm navigiert der Benutzer tiefer zu diesem Stapel von Bildschirmen. Mithilfe der **Menü** Schaltfläche auf der Siri-Remote Navigation können Sie rückwärts durch den Stapel navigieren, um zu einem vorherigen Bildschirm oder Hauptmenü zurückzukehren.

Apple empfiehlt Folgendes, wenn Sie die Navigation für Ihre tvos-App entwerfen:

- **Layout Ihrer Navigation, um das Auffinden von Inhalten schnell und einfach zu gestalten** , wenn Sie mit der geringsten Anzahl von Abzweigungen auf Inhalte in ihrer App zugreifen möchten, auf die Sie wie möglich klickt. Vereinfachen Sie die Navigation, und organisieren Sie Inhalte für die minimale Anzahl von Bildschirmen.
- **Erstellen einer fließenden Benutzeroberfläche mithilfe der Toucheingabe** : Stellen Sie sicher, dass ein Benutzer mit minimaler Reibung zwischen _fokussierbaren Elementen_ wechseln kann, indem er die geringste Anzahl von Gesten ermöglicht.
- **Design mit Fokus** : da der Benutzer über den Raum mit Inhalten interagiert, muss er den Fokus auf ein Benutzeroberflächen Element verschieben, bevor es mit der Siri-Remote Interaktion interagiert. Benutzer werden mit Ihrer APP frustriert, wenn Sie zu viele Gesten für Sie benötigen, um Ihre Ziele zu erreichen.
- **Bereitstellen der rückwärts Navigation über die Menü Schaltfläche** : um eine einfache und vertraute Benutzerfunktion zu erstellen, können Benutzer mithilfe der **Menü** Schaltfläche der Siri-Remote Navigation rückwärts navigieren. Wenn Sie die **Menü** Schaltfläche drücken, müssen Sie immer zum vorherigen Bildschirm zurückkehren oder zum Hauptmenü der APP zurückkehren. Auf der obersten Ebene der APP sollte durch Drücken der **Menü** Schaltfläche zum Apple TV-Startbildschirm zurückzukehren.
- **Vermeiden Sie die Anzeige der Schaltfläche "zurück** ", da durch Drücken der **Menü** Schaltfläche auf der Siri-Remote Seite der Bildschirm Stapel rückwärts navigiert wird. vermeiden Sie ein zusätzliches Steuerelement, das dieses Verhalten dupliziert. Eine Ausnahme von dieser Regel ist das erwerben von Bildschirmen oder Bildschirmen mit destruktiven Aktionen (z. b. das Löschen von Inhalt), wenn auch eine Schaltfläche **Abbrechen** angezeigt werden soll.
- Zeigen Sie große Auflistungen **auf einem einzelnen Bildschirm anstelle von vielen** an. die Siri-Remote wurde so entworfen, dass Sie eine umfangreiche Sammlung von Inhalten schnell und einfach mithilfe von Gesten durchläuft. Wenn Ihre APP mit einer großen Auflistung von Fokus verwendbaren Elementen funktioniert, sollten Sie Sie auf einem einzelnen Bildschirm aufbewahren, anstatt Sie in viele Bildschirme zu unterteilen, die eine größere Navigation im Benutzer Teil erfordern.
- **Verwenden Sie Standard Steuerelemente für die Navigation** . verwenden Sie zum Erstellen einer einfachen und vertrauten Benutzer Darstellung, soweit möglich, integrierte Steuer `UIKit` Elemente wie z. b. Seiten Steuerelemente, Registerkarten, segmentierte Steuerelemente, Tabellen Sichten, Sammlungs Ansichten und geteilte Ansichten für die Navigation Ihrer APP. Da Benutzer bereits mit diesen Elementen vertraut sind, können Sie intuitiv durch Ihre APP navigieren.
- **Bevorzugen der horizontalen Inhalts Navigation** : aufgrund der Art von Apple TV ist das Schwenken von links nach rechts auf der Siri-Remote Seite natürlicher als nach oben und unten. Beachten Sie diese Option, wenn Sie Inhalts Layouts für Ihre APP entwerfen.

<a name="Focus-and-Selection"></a>

## <a name="focus-and-selection"></a>Fokus und Auswahl

Im Apple TV wird ein Bild, eine Schaltfläche oder ein anderes UI-Element als _Fokus_ betrachtet, wenn es das Ziel der aktuellen Navigation ist.

[![Beispiel für Fokus und Auswahl](navigation-focus-images/focus01.png)](navigation-focus-images/focus01.png#lightbox)

Anders als bei IOS-Geräten, bei denen der Benutzer direkt mit Elementen auf dem Touchscreen des Geräts interagiert, interagieren Benutzer mit tvos-Elementen über den Raum mithilfe von Siri Remote. Zum präsentieren und Verarbeiten dieser Benutzerinteraktion verwendet das Apple TV ein _Fokus_ basiertes Modell.

Wenn Sie Gesten und Schaltflächen auf der [Siri-Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)Maustaste verwenden, verschiebt der Benutzer den Fokus von einem Benutzeroberflächen Element zu einem anderen. Nachdem der Fokus auf das gewünschte Element verschoben wurde, klickt der Benutzer auf die Option, um den Inhalt auszuwählen oder die durch dieses Element dargestellte Aktion zu aktivieren.

Als Fokus Änderungen werden die Benutzeroberflächen Elemente, die derzeit den Fokus haben, durch feine Animationen und Effekte (z. b. die Auswirkungen auf Bilder) eindeutig identifiziert.

Apple hat die folgenden Vorschläge zum Arbeiten mit Fokus und Auswahl:

- **Verwenden integrierter UI-Steuerelemente für Bewegungs Effekte** : indem Sie `UIKit` und die Fokus-API in Ihrer Benutzeroberfläche verwenden, wendet das Fokus Modell automatisch die Standard Bewegung und visuelle Effekte auf die Benutzeroberflächen Elemente an. Dies sorgt dafür, dass Ihre APP für die Benutzer der Apple TV-Plattform nativ und vertraut ist und eine flüssige und intuitive Bewegung zwischen Fokus baren Elementen ermöglicht.
- **Verschieben des Fokus in den erwarteten Richtungen** : in Apple TV verwendet fast jedes Element indirekte Manipulationen. Beispielsweise verwendet der Benutzer die Siri-Remote-, um den Fokus zu verschieben, und das System führt automatisch einen Bildlauf zur Schnittstelle durch, um das aktuell fokussierte Element Wenn Ihre APP diese Art von Interaktion implementiert, stellen Sie sicher, dass sich der Fokus in die Richtung der Benutzer Bewegung verschiebt. Wenn der Benutzer also auf den Siri-Remote Fokus rechts bewegt, sollte er nach rechts verschoben werden (was dazu führen kann, dass der Bildschirm nach links scrollen würde). Die einzige Ausnahme von dieser Regel sind voll Bildelemente, die eine direkte Bearbeitung verwenden (wobei das Element durch Schwenken nach oben verschoben wird).
- **Stellen Sie sicher, dass das fokussierte Element offensichtlich ist** . da die Benutzer von weitem mit ihren Benutzeroberflächen Elementen interagieren, ist es wichtig, dass das aktuell fokussierte Element angezeigt wird. In der Regel wird dies automatisch durch integrierte Elemente gehandhabt `UIKit` . Verwenden Sie für benutzerdefinierte Steuerelemente Features wie Elementgröße oder Schatten, um den Fokus anzuzeigen.
- **Verwenden Sie "Parser", um fokussierte Elemente reaktionsfähig zu machen** : kleine, zirkuläre Gesten auf dem Siri-Remote Ergebnis in sanfter echtzeitbewegung des Fokus Elements. Dieser Teil des [Effekts](#Focus-and-Parallax) ist in `UIKit` _geschichtete Bilder_ integriert, um dem Benutzer einen Eindruck von der Verbindung mit dem fokussierten Element zu vermitteln.
- **Erstellen Sie Fokussier Bare Elemente der entsprechenden Größe** : große Elemente mit ausreichendem Abstand sind einfacher auszuwählen und können als kleinere Elemente navigiert werden.
- **Design der Benutzeroberfläche, um entweder fokussiert oder nicht fokussiert zu** sein. in der Regel stellt Apple TV das fokussierte Element dar, indem es seine Größe vergrößert. Stellen Sie sicher, dass die Benutzeroberflächen Elemente Ihrer Apps in jeder Präsentations Größe hervorragend aussehen, und stellen Sie, falls erforderlich, Ressourcen für Elemente mit größerer Größe bereit.
- **Stellen Sie die Fokus Änderungen** in der Animation auf fließende Weise ein, um **zwischen Elementen mit Fokus und** **ohne Fokus** zu wechseln, um die Übergänge von jarringvorgängen zu erhalten.
- **Cursor nicht anzeigen** : Benutzer erwarten, dass Sie die Benutzeroberfläche Ihrer App mithilfe des Fokus navigieren, und nicht durch Verschieben eines Cursors auf den Bildschirm. Die Benutzeroberfläche sollte immer das Fokus Modell verwenden, um eine konsistente Benutzeroberfläche zu bieten.

<a name="Working-with-Focus"></a>

### <a name="working-with-focus"></a>Arbeiten mit dem Fokus

Es kann vorkommen, dass Sie ein benutzerdefiniertes Steuerelement erstellen möchten, das zu einem Fokus verwendbaren Element werden kann. Wenn Sie also die `CanBecomeFocused` -Eigenschaft überschreiben und zurückgeben `true` , geben Sie zurück `false` . Beispiel:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

Sie können die- `Focused` Eigenschaft eines- `UIKit` Steuer Elements jederzeit verwenden, um festzustellen, ob es sich um das aktuelle Element handelt. `true`, Wenn das UI-Element derzeit den Fokus besitzt, andernfalls. Beispiel:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Obwohl Sie den Fokus nicht direkt über Code auf ein anderes UI-Element verschieben können, können Sie angeben, welches UI-Element zuerst den Fokus erhält, wenn ein Bildschirm durch Festlegen der- `PreferredFocusedView` Eigenschaft auf geladen wird `true` . Beispiel:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates"></a>

### <a name="working-with-focus-updates"></a>Arbeiten mit Fokus Updates 

Wenn der Benutzer den Fokus auf die Verschiebung von einem Benutzeroberflächen Element zu einem anderen bewirkt (z. b. als Reaktion auf eine Geste auf der Siri-Remote), wird ein _Fokus Aktualisierungs Ereignis_ an das Element gesendet, das den Fokus verliert, und das Element erhält den Fokus.

Für benutzerdefinierte Elemente, die von `UIView` oder Erben `UIViewController` , können Sie mehrere Methoden überschreiben, um mit dem Fokus Aktualisierungs Ereignis zu arbeiten:

- **Didupdatefocus** : Diese Methode wird immer dann aufgerufen, wenn die Ansicht den Fokus erhält oder verliert.
- **Schulter dupdatefocus** : Verwenden Sie diese Methode, um zu definieren, wohin der Fokus verschoben werden darf.

Um anzufordern, dass die Fokus-Engine den Fokus wieder auf das `PreferredFocusedView` UI-Element verschiebt, nennen Sie die- `SetNeedsUpdateFocus` Methode des Ansichts Controllers.

> [!IMPORTANT]
> Der Aufruf von `SetNeedsUpdateFocus` hat nur Auswirkungen, wenn der Ansichts Controller, für den er aufgerufen wird, die Ansicht enthält, die gerade den Fokus besitzt.

<a name="Working-with-Focus-Guides"></a>

### <a name="working-with-focus-guides"></a>Arbeiten mit Fokus Handbüchern

Die in tvos integrierte Fokus-Engine eignet sich hervorragend zum Verarbeiten von Bewegungen zwischen Elementen, die auf ein horizontales und Vertikales Raster fallen. In der Regel müssen Sie nichts mehr tun, als die Benutzeroberflächen Elemente zum Schnittstellendesign hinzuzufügen, und die Fokus-Engine behandelt automatisch die Bewegung zwischen diesen Elementen ohne Entwickler Eingriff.

Es kann jedoch vorkommen, dass es aufgrund der Grundbedürfnisse des UI-Entwurfs vorkommen kann, dass Benutzeroberflächen Elemente nicht in ein horizontales und Vertikales Raster fallen und möglicherweise nicht mehr auf Sie zugegriffen werden kann, da Sie diagonal zueinander sind. Dies ist darauf zurückzuführen, dass die Fokus-Engine so konzipiert wurde, dass eine einfache aufwärts-, nach-unten-, linke und Rechte Bewegung zwischen Elementen der

Betrachten Sie das folgende Benutzeroberflächen Layout für ein Beispiel:

 [![Beispiel für das Arbeiten mit Fokus Handbüchern](navigation-focus-images/guide01.png)](navigation-focus-images/guide01.png#lightbox)

Da die Schaltfläche " **Weitere Informationen** " nicht auf ein horizontales und Vertikales Raster mit der Schaltfläche " **kaufen** " fällt, ist der Benutzer nicht darauf zugreifen können. Dies kann jedoch problemlos mithilfe eines _Schwerpunkt Handbuchs_ korrigiert werden, um Bewegungs Hinweise für die Fokus-Engine bereitzustellen. 

Ein Fokus Leit Faden ( `UIFocusGuide` ) macht einen nicht sichtbaren Bereich der Ansicht als fokussierbar für die Fokus-Engine verfügbar, sodass der Fokus in eine andere Ansicht umgeleitet werden kann.

Zum Lösen des oben genannten Beispiels könnte der folgende Code dem Ansichts Controller hinzugefügt werden, um einen Fokus Leit Faden zwischen den Schaltflächen " **Weitere Informationen** " und " **kaufen** " zu erstellen:

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

Zuerst wird ein neuer `UIFocusGuide` erstellt und mithilfe der-Methode der layoutführungsauflistung der Ansicht hinzugefügt `AddLayoutGuide` .

Im nächsten Schritt werden die Anker der oberen, linken, breiten und Höhen der Vordergrund Linie relativ zu den Schaltflächen " **Weitere Informationen** " und " **kaufen** " angepasst, um Sie zwischen Ihnen zu positionieren. Siehe:

[![Leitfaden zum Beispiel Fokus](navigation-focus-images/guide02.png)](navigation-focus-images/guide02.png#lightbox)

Es ist auch wichtig zu beachten, dass die neuen Einschränkungen durch Festlegen der-Eigenschaft auf aktiviert werden, wenn Sie erstellt werden `Active` `true` :

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Wenn das neue Fokus Handbuch festgelegt und der Ansicht hinzugefügt wurde, kann die-Methode des Ansichts Controllers `DidUpdateFocus` überschrieben werden, und der folgende Code wurde hinzugefügt, um zwischen den Schaltflächen **Weitere Informationen** und **kaufen** zu wechseln:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

Zuerst wird dieser Code `NextFocusedView` vom aus dem, das `UIFocusUpdateContext` in () übermittelt wurde, angezeigt `context` . Wenn diese Ansicht ist `null` , wird keine Verarbeitung benötigt, und die Methode wurde beendet.

Anschließend `nextFocusableItem` wird ausgewertet. Wenn Sie entweder mit den Schaltflächen " **Weitere Informationen** " oder " **kaufen** " übereinstimmt, wird der Fokus mithilfe der-Eigenschaft des Fokus Handbuchs an die Schaltfläche "gegen `PreferredFocusedView` Beispiel:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

Wenn keine Schaltfläche die Quelle der Fokus Verschiebung ist, wird die- `PreferredFocusedView` Eigenschaft gelöscht:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections"></a>

### <a name="working-with-focus-in-collections"></a>Arbeiten mit dem Fokus in Auflistungen

Wenn Sie entscheiden, ob ein einzelnes Element in einem oder einem verwendet werden kann `UICollectionView` `UITableView` , überschreiben Sie die Methoden von `UICollectionViewDelegate` `UITableViewDelegate` bzw.. Beispiel:

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

Die `CanFocusItem` Methode gibt zurück `true` , wenn das aktuelle Element im Fokus sein kann, andernfalls wird zurückgegeben `false` .

Wenn Sie möchten, dass sich ein `UICollectionView` oder ein `UITableView` dem letzten Element merken kann, wenn es den Fokus verliert und wiederherstellt, legen Sie die- `RemembersLastFocusedIndexPath` Eigenschaft auf fest `true` .

<a name="Focus-and-Parallax"></a>

## <a name="focus-and-parallax"></a>Fokus und Element

Wie bereits erwähnt, wird ein Benutzeroberflächen Element als _Fokus_ betrachtet, wenn es das aktuelle Element während eines Navigations Ereignisses ist. In der Regel wird die Größe eines Elements, das sich im Fokus befindet, geringfügig erhöht und mithilfe eines Schattens visuell erhöht. 

Wenn der Benutzer eine langsame, zirkuläre Bewegung für die Siri-Remote Bewegung ausführt, wandelt sich das fokussierte Element in Reaktion auf diese Bewegung in Echtzeit durch. Wenn der Einfluss auftritt, wird ein beleuchteter Glanz auf das Bild angewendet, sodass die Oberfläche scheint. Nach einer bestimmten Menge an Inaktivität werden alle nicht Fokus bezogenen Inhalts-und das fokussierte Element noch größer. 

Diese Effekte können zusammen verwendet werden, um eine geistige Verbindung zwischen dem Inhalt auf dem TV-Bildschirm und dem Benutzer, der mit dem Apple TV von der Couch interagiert, bereitzustellen.

Im Apple TV wird dieser Teil des Systems im gesamten System verwendet, um einen Eindruck von 3D-Tiefe und Dynamics für Elemente im Fokus zu vermitteln. tvos verwendet eine Reihe von transparenten, [geschichteten Bildern](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) , die zum Erstellen dieses Effekts dynamisch verschoben und skaliert werden.

Für das Symbol ihrer tvos-APP sind geschichtete Bilder erforderlich und werden für dynamische Top-Shelf-Inhalte unterstützt. Obwohl es nicht erforderlich ist, schlägt Apple nachdrücklich vor, geschichtete Images in beliebigen anderen fokussierbaren Elementen in der Benutzeroberfläche Ihrer APP zu implementieren.

### <a name="enabling-parallax"></a>Aktivieren von "Parser"

Das- `UIImageView` Steuerelement (oder ein beliebiges Steuerelement, das von erbt `UIImageView` ) unterstützt automatisch den-Effekt. Diese Unterstützung ist standardmäßig deaktiviert. verwenden Sie den folgenden Code, um Sie zu aktivieren:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Wenn diese Eigenschaft auf festgelegt `true` ist, erhält die Bildansicht automatisch den Effekt, wenn Sie vom Benutzer ausgewählt und im Fokus angezeigt wird.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Konzept des Fokus behandelt und erläutert, wie es für die Navigation in der Benutzeroberfläche einer xamarin. tvos-App verwendet wird. Es wird untersucht, wie die integrierten tvos-Navigations Steuerelemente Fokus, Hervorhebung und Auswahl verwenden, um die Navigation bereitzustellen. Im nächsten Schritt wird erläutert, wie der Fokus mit unterschiedlichen und geschichteten Bildern verwendet werden kann, um visuelle Hinweise für den aktuellen Navigations Zustand für den Endbenutzer bereitzustellen. Schließlich wurde die Unterstützung der Arbeit mit dem Fokus, dem Fokus auf Aktualisierungen, dem Fokus auf Auflistungen und dem Aktivieren von "Parser

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
