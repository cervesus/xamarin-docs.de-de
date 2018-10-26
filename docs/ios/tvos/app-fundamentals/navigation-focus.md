---
title: Arbeiten mit TvOS Navigation und Fokus in Xamarin
description: Dieser Artikel beschreibt das Konzept der Fokus und wie es verwendet wird, und Behandeln der Navigation innerhalb einer Xamarin.tvOS-app.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 1cfa51b8e5434480d7d15fbf23d78f8b8735f16a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112586"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Arbeiten mit TvOS Navigation und Fokus in Xamarin

_Dieser Artikel beschreibt das Konzept der Fokus und wie es verwendet wird, und Behandeln der Navigation innerhalb einer Xamarin.tvOS-app._


Dieser Artikel behandelt das Konzept der [Fokus](#Focus-and-Selection) und wie diese verwendet werden, behandeln [Navigation](#Navigation) in einer Xamarin.tvOS-app-Benutzeroberfläche. Untersucht, wie die integrierten TvOS Navigationssteuerelemente Fokus, hervorheben und Auswahl verwenden, um Ihrer Xamarin.tvOS-app Benutzer-Schnittstelle Navigation zu ermöglichen.

[![](navigation-focus-images/intro01.png "TvOS-apps Benutzernavigation-Schnittstelle")](navigation-focus-images/intro01.png#lightbox)

Als Nächstes müssen wir uns das ansehen an, wie der Fokus mit verwendet werden kann [Parallax](#Focus-and-Parallax) und *Layered Images* um visuelle Hinweise für den aktuellen Status der Navigation für den Endbenutzer bereitzustellen.

Abschließend betrachten wir arbeiten mit [Fokus](#Working-with-Focus), [Fokus Updates](#Working-with-Focus-Updates), [Fokus Handbücher](#Working-with-Focus-Guides), [Fokus in Sammlungen](#Working-with-Focus-in-Collections) und [ Aktivieren von Parallax](#Enabling-Parallax) für Image-Ansichten in Ihren apps Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Navigation

Benutzer Ihrer Xamarin.tvOS-app werden nicht mit seinem-Schnittstelle direkt als mit iOS, in dem sie-Images auf den Bildschirm des Geräts, jedoch indirekt von über den Raum mithilfe Tap, interagiert werden die [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Sie benötigen, beachten Sie, dass beim Entwerfen der Benutzeroberfläche für die Ihrer app, sodass es auf natürliche Weise übertragen, aber dennoch der Benutzer, die in der Apple TV-Umgebung befinden bleibt.

Eine erfolgreiche TvOS-app implementiert die Navigation in einer Weise, die reibungslos unterstützt den Zweck der app und die Struktur der Daten, die es bereitstellt, ohne auf die Navigation selbst. Entwerfen Sie Ihre Navigation, sodass es natürliche und vertraute fühlt sich ohne großen der Benutzeroberfläche oder zeichnen den Fokus von den Inhalt und die benutzererfahrung für apps.

[![](navigation-focus-images/nav01.png "TvOS-Einstellungen-app")](navigation-focus-images/nav01.png#lightbox)

Während mithilfe von Apple TV, der Benutzer in der Regel über einem stapelsatz an den Bildschirmen jeweils die Darstellung einer bestimmten Gruppe von Inhalt navigiert. Wiederum ist es möglich, alle neuen Bildschirme kann dazu führen, einen oder mehrere untergeordnete Bildschirme von Inhalten mithilfe von standardmäßigen Benutzeroberflächen-Steuerelemente wie z. B. [Schaltflächen](~/ios/tvos/user-interface/buttons.md), [Registerkartenleisten](~/ios/tvos/user-interface/tab-bars.md), Tabellen, [Auflistungsansichten](~/ios/tvos/user-interface/collection-views.md) oder [ Geteilte Ansichten](~/ios/tvos/user-interface/split-views.md).

Mit jeder neuen Bildschirm Daten der Benutzer navigiert tiefer in diesem Stapel von Bildschirmen. Mithilfe der **Menü** Schaltfläche auf dem Remotecomputer Siri, sie können rückwärts durch den Stapel Navigieren zu einer vorherigen Bildschirm oder im Hauptmenü zurückgegeben.

Apple empfiehlt Folgendes Denken Sie daran beibehalten, wenn die Navigation für TvOS-app zu entwerfen:

- **Layout der Navigation zu machen Suchen von Inhalten schnell und einfach** -Benutzern Zugriff auf Inhalte in Ihrer app in die geringste Anzahl der abtaststellen, klickt und wie möglich mit einer wischbewegung. Die Navigation zu vereinfachen und Organisieren von Inhalt an die minimale Anzahl der Bildschirme.
- **Erstellen Sie eine fließende Schnittstelle mithilfe von Touch** -stellen Sie sicher, dass ein Benutzer zwischen verschieben kann _Fokussierbare Elemente_ mit minimalen reibungsverlusten möglich, mit die geringste Anzahl von Gesten möglich.
- **Entwurf mit dem Fokus im Denken Sie daran** : Da der Benutzer mit Inhalten durch das Zimmer interagiert, müssen sie den Fokus auf ein Element der Benutzeroberfläche zu verschieben, bevor interagieren sie mit der entfernten Siri. Benutzer erhalten mit Ihrer app frustriert, wenn zu viele Gesten für die sie beim Erreichen ihrer Ziele erforderlich ist.
- **Bereitstellen von Abwärtskompatibilität Navigation über der Menüschaltfläche** – erstellen eine einfache und vertraute Benutzeroberfläche, um Benutzern zu ermöglichen und Abwärtskompatibilität der Siri Remote des **Menü** Schaltfläche. Drücken Sie die **Menü** Schaltfläche immer zum vorherigen Bildschirm zurückzukehren oder an der app-Menü "Main" zurückgeben sollte. Drücken Sie auf der app der obersten Ebene, die **Menü** Schaltfläche sollte zum Bildschirm Apple TV-Startseite zurückzukehren.
- **In der Regel vermeiden, die eine zurück-Schaltfläche anzeigen** : Da Drücken der **Menü** Schaltfläche auf dem Remotecomputer Siri navigiert rückwärts durch den Stapel Bildschirm, zu vermeiden, eine zusätzliche Kontrolle, die dieses Verhalten, um Duplikate angezeigt. Eine Ausnahme von dieser Regel wird für den Kauf von Bildschirme oder Bildschirme mit schädlicher Aktionen (z. B. das Löschen von Inhalten aus), in denen eine **Abbrechen** Schaltfläche auch angezeigt werden soll.
- **Anzeigen von großen Auflistungen auf einem Bildschirm statt vieler** -die Siri Remote wurde entwickelt, durchlaufen eine umfangreiche Sammlung von Inhalten schnell und einfach anhand von Gesten. Wenn Ihre app mit einer großen Auflistung von Elementen mit Fokus funktioniert, sollten Sie sorgen für einen einzigen Bildschirm, anstatt sie in der viele Bildschirme, die weitere Navigation den Benutzer erfordern.
- **Verwenden Sie Standardsteuerelemente für die Navigation** – in diesem Fall um eine einfache und vertraute Benutzeroberfläche, wo immer dies möglich zu erstellen, verwenden integrierte `UIKit` Steuerelemente wie Steuerelemente der Seite, Registerkartenleisten, segmentierte Steuerelemente, Tabellenansichten, Auflistungsansichten und Teilen Ansichten für die Navigation von Ihrer app. Da Benutzer ist bereits mit diesen Elementen vertraut sind, werden sie intuitiv können Ihre app navigieren befinden.
- **Bevorzugen Sie horizontale Inhaltsnavigation** -aufgrund der Art des Apple TV, Wischen von links nach rechts auf dem Remotecomputer Siri ist natürlicher wirkt als hoch-und Herunterskalieren. Erwägen Sie diese Option aus, wenn Content Layouts für Ihre app zu entwerfen.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>Fokus und Auswahl

Auf Apple TV ein Bild, einer Schaltfläche oder andere Elemente der Benutzeroberfläche gilt _im Fokus_ wann ist das Ziel des die aktuelle Navigation.

[![](navigation-focus-images/focus01.png "Fokus und Auswahl-Beispiel")](navigation-focus-images/focus01.png#lightbox)

Im Gegensatz zu iOS-Geräte, in denen der Benutzer direkt mit Elementen auf Touchscreen des Geräts, interagiert, Benutzer interagieren mit TvOS-Elemente aus, durch das Zimmer Siri Remoteinstanz verwenden. Um darzustellen, und dieser Benutzerinteraktionen verarbeiten, Apple TV verwendet eine _Fokus_ basierendes Modell.

Mit Gesten und Schaltfläche drückt, auf die [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), der Benutzer den Fokus von einem UI-Element in eine andere. Sobald der Fokus auf das gewünschte Element verschoben wurde, klickt der Benutzer, wählen Sie den Inhalt, oder aktivieren die Aktion, die durch dieses Element dargestellt wird.

Als fokusänderungen sind feine Animation und Effekte (z. B. den Parallaxeneffekt auf Images) verwendet, das Element der Benutzeroberfläche eindeutig zu identifizieren, das gerade den Fokus besitzt.

Apple hat die folgenden Vorschläge für die Arbeit mit Fokus und Auswahl:

- **Verwenden Sie integrierte Steuerelemente der Benutzeroberfläche während der Übertragung Effekte** – mit `UIKit` und die API den Fokus auf der Benutzeroberfläche, das Fokus wird automatisch der standardmäßigen während der Übertragung und visuelle Effekte für Ihre UI-Elemente anzuwenden. Dies ist von Ihrer app können Sie native und Benutzern der Apple TV-Plattform vertraut sind und fließende und intuitive datenverschiebung zwischen den Fokus erhalten kann Elemente ermöglicht.
- **Verschieben des Fokus in erwartet Richtungen** -auf Apple TV, nahezu jedes Element verwendet eine indirekte Manipulation. Z. B. der Benutzer verwendet die Siri Remote, um den Fokus zu verschieben, und das System verschiebt automatisch die Schnittstelle, um das derzeit fokussierten Element sichtbar bleiben. Wenn Ihre app dieser Art der Interaktion implementiert wird, stellen Sie sicher, dass der Fokus in der Richtung des Benutzers Bewegung. Also, wenn der Benutzer direkt auf Wischen sollte den Siri Remote rechts Fokus auf (die den Bildschirm, um auf der linken Seite einen Bildlauf führen könnte). Die einzige Ausnahme dieser Regel wird im Vollbildmodus-Elemente, die direkte Bearbeitung verwenden (wobei verschiebt streichen das Element nach oben).
- **Stellen Sie sicher, dass das Element mit Fokus Obvious ist** -da Ihre Benutzer mit aus der Ferne Ihre UI-Elementen interagieren, es ist wichtig, dass das aktuell fokussierte Element sich abhebt. In der Regel Dadurch wird automatisch vom behandelt integrierte `UIKit` Elemente. Verwenden Sie für benutzerdefinierte Steuerelemente Funktionen wie Elementgröße oder Schattenkopien, um den Fokus zu anzuzeigen.
- **Parallaxen, konzentriert sich Elemente-reaktionsfähig Stellen verwenden** : klein, zirkuläre Gesten auf dem Remotecomputer Siri umfassend und in Echtzeit Bewegung des das fokussierte Element führen. Dies [Parallaxeneffekt](#Focus-and-Parallax) integriert ist `UIKit` _Layered Images_ , Benutzern einen Eindruck von der Verbindung das fokussierte Element.
- **Erstellen Sie den Fokus erhalten kann Elemente von geeigneter Größe** -große Elemente mit ausreichend Abstand sind einfacher zu wählen, und navigieren als kleinere Elemente.
- **Entwerfen von UI-Element, sehen Sie gute entweder im Zusammenhang mit oder Unfocused** – in der Regel Apple TV wird das Element mit Fokus darstellt, durch das Erhöhen der Größe. Stellen Sie sicher, dass Ihre apps die UI-Elemente hervorragend für eine beliebige Größe für die Präsentation aussehen, und, falls erforderlich, geben Sie Ressourcen für größere Größe Elemente.
- **Fokus auf Änderungen problemlos darstellen** -verwenden Sie die Animation reibungslos zwischen Elementen eingeblendet **Focused** und **Unfocused** Zustand zu Übergänge werden bedürfen.
- **Ein Cursor zeigen nicht an** -Benutzer erwarten, dass UIs Ihrer app mit den Fokus zu navigieren und nicht durch einen Cursor auf dem Bildschirm bewegen. Ihre Benutzeroberfläche sollten das Modell den Fokus immer verwenden, um eine konsistente benutzererfahrung bieten.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Arbeiten mit Fokus

Es gibt möglicherweise Fälle, in denen Sie möchten, um ein benutzerdefiniertes Steuerelement zu erstellen, das ein Element den Fokus erhalten werden können. Wenn also überschreiben die `CanBecomeFocused` Eigenschaft und die Rückgabewerte `true`, ansonsten return `false`. Zum Beispiel:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

Zu einem beliebigen Zeitpunkt können Sie die `Focused` Eigenschaft eine `UIKit` Steuerelement, um festzustellen, ob sie das aktuelle Element ist. Wenn `true` das UI-Element derzeit den Fokus besitzt, andernfalls nicht der Fall. Zum Beispiel:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Während Sie den Fokus auf ein anderes Benutzeroberflächenelement direkt über Code verschieben können, können Sie angeben, welches Element der Benutzeroberfläche zunächst den Fokus erhält, beim Laden eines Bildschirms durch Festlegen seiner `PreferredFocusedView` Eigenschaft `true`. Zum Beispiel:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Arbeiten mit Fokus Updates 

Wenn der Benutzer den Fokus Zuweisen von einem UI-Element in eine andere (z. B. als Reaktion auf Gesten auf dem Remotecomputer Siri) verursacht einen _Update Fokusereignis_ sowohl für das Element den Fokus als auch für das Element, um den Fokus erhält.

Für benutzerdefinierte Elemente, die von erben `UIView` oder `UIViewController`, Sie können verschiedene Methoden für das Fokusereignis des Updates überschreiben:

- **DidUpdateFocus** – diese Methode wird aufgerufen, wenn Sie einem beliebigen Zeitpunkt die Ansicht erhält oder verliert den Fokus.
- **ShouldUpdateFocus** -mit dieser Methode können Sie definieren, in denen Fokus verschoben werden darf.

Um anzufordern, dass die Engine den Fokus verschiebt den Fokus zurück, an die `PreferredFocusedView` Benutzeroberflächenelement, Aufruf der `SetNeedsUpdateFocus` Methode des Ansichtscontrollers.

> [!IMPORTANT]
> Aufrufen von `SetNeedsUpdateFocus` hat nur Auswirkungen, wenn es sich bei der View-Controller, die es dagegen aufgerufen wird, die Sicht enthält, die gerade den Fokus besitzt.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Arbeiten mit Fokus Führungslinien

Der Fokus-Engine integriert TvOS ist hervorragend für Verschiebungen zwischen den Elementen, die in einem Raster horizontale und vertikale liegen. In der Regel müssen Sie nichts mehr hinzufügen, dass die Elemente der Benutzeroberfläche zu Ihrem Entwurf der Benutzeroberfläche und der Fokus-Engine automatisch die Bewegung zwischen diesen Elementen ohne Eingriffe von Entwicklern behandelt.

Allerdings gibt es möglicherweise vorkommen, aufgrund der Anforderungen von Ihrem Entwurf von Benutzeroberflächen, bei der UI-Elemente in einem Raster horizontale und vertikale tappen Sie nicht und können zugegriffen werden, da sie untereinander diagonale sind. Dies geschieht, da die Engine den Fokus nach oben, nach unten, links und rechts verschieben zwischen UI-Elemente nur einfache behandeln konzipiert wurde.

Führen Sie ein Beispiel für das folgende UI-Layout:

 [![](navigation-focus-images/guide01.png "Arbeiten mit Fokus Guides-Beispiel")](navigation-focus-images/guide01.png#lightbox)
 
Da die **weiterer Informationen** Schaltfläche liegt nicht in einem horizontalen und vertikalen Raster mit der **kaufen** Schaltfläche wäre es nicht möglich, für den Benutzer. Jedoch, dies kann korrigiert werden, problemlos mit einem _Fokus Handbuch_ Bewegung Hinweise an die Engine den Fokus zu übermitteln. 

Eine Anleitung für den Fokus (`UIFocusGuide`) stellt einen nicht sichtbaren Bereich der Ansicht als Focusable der Fokus-Engine, sodass den Fokus auf die zu einer anderen Ansicht umgeleitet werden.

Also um die oben genannte Beispiel zu lösen, der folgende Code konnte hinzugefügt werden, erstellen Sie eine Anleitung den Fokus zwischen der View-Controller die **weiterer Informationen** und **kaufen** Schaltflächen:

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

Zunächst wird ein neues `UIFocusGuide` wird erstellt und hinzugefügt werden, mit der Ansicht Layoutführungslinie Auflistung der `AddLayoutGuide` Methode.

Als Nächstes oben, links, Breite und Höhe Anker des Fokus Handbuchs werden angepasst, relativ zum die **weiterer Informationen** und **kaufen** Schaltflächen, die sie zwischen ihnen zu positionieren. Thema

[![](navigation-focus-images/guide02.png "Beispiel für den Fokus Handbuch")](navigation-focus-images/guide02.png#lightbox)

Es ist auch wichtig zu beachten, dass die neuen Einschränkungen aktiviert wird, werden während der Erstellung durch Festlegen ihrer `Active` Eigenschaft `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Mit dem neuen Fokus für hergestellt, und der Ansicht, die View-Controller hinzugefügt `DidUpdateFocus` Methode kann überschrieben werden, und der folgende Code hinzugefügt, um zwischen Verschieben der **weiterer Informationen** und **kaufen** Schaltflächen:

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

Dieser Code-Get "First", die `NextFocusedView` aus der `UIFocusUpdateContext` , die übergeben wurde (`context`). Wenn diese Ansicht ist `null`keine Verarbeitung erforderlich ist und anschließend die Methode wurde beendet.

Als Nächstes die `nextFocusableItem` ausgewertet wird. Wenn diese entweder entspricht der **weiterer Informationen** oder **kaufen** Schaltflächen die, den Fokus wird gesendet, um die entgegengesetzten Schaltfläche mithilfe des Handbuchs Fokus `PreferredFocusedView` Eigenschaft. Zum Beispiel:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

Wenn keine Schaltfläche die Quelle der Schicht den Fokus, ist die `PreferredFocusedView` Eigenschaft gelöscht wird:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Arbeiten mit Fokus in Auflistungen

Bei der Entscheidung, ob ein einzelnes Element in Fokus festgelegt werden kann oder keine `UICollectionView` oder `UITableView`, müssen Sie Methoden zum Überschreiben der `UICollectionViewDelegate` oder `UITableViewDelegate` bzw. Zum Beispiel:

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

Die `CanFocusItem` Methodenrückgabe `true` das aktuelle Element im Fokus sein kann, andernfalls gibt `false`.

Wenn Sie möchten eine `UICollectionView` oder ein `UITableView` legen Sie zum Speichern und Wiederherstellen Fokus bis zum letzten Element und erhält wieder den Fokus verliert, die `RemembersLastFocusedIndexPath` Eigenschaft `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Fokus und Parallax

Wie bereits erwähnt, ein Element der Benutzeroberfläche gilt _im Fokus_ wann ist das aktuelle Element bei der ein Navigationsereignis. In der Regel wird ein Element den Fokus, dessen Größe etwas erhöht, und es ist visuell mit erweiterten Berechtigungen unter Verwendung einer Schattenkopie. 

Wenn der Benutzer eine Geste für eine langsamere, runden auf dem Remotecomputer Siri trifft, wird das Element mit Fokus als Reaktion auf diese Bewegung in Echtzeit sway. Wie die Sway auftritt, wird ein beleuchtete Glanz auf das Bild, sodass der Oberfläche angezeigt, die verwendet werden angewendet. Nach einem bestimmten Zeitraum der Inaktivität alle Inhalte für die Out-of-Focus abgeblendet, und das Focused-Element wird immer noch größer. 

Diese Effekte arbeiten zusammen, um eine geistige Verbindung zwischen dem Inhalt auf dem TV-Bildschirm und der Benutzer, die Interaktion mit Apple TV aus den Garten bereitzustellen.

Auf Apple TV wird diese Parallaxeneffekt vermitteln einen Eindruck von 3D Tiefe und Dynamics, bildschärfenmodus Elemente im gesamten System verwendet. TvOS verwendet eine Reihe von transparenten [Layered Images](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) , das verschoben und dynamisch skaliert, um diesen Effekt zu erstellen.

Mehrschichtige Abbilder sind erforderlich, damit die TvOS-app-Symbol, und es wird für dynamischen Inhalt, Top Shelf unterstützt. Zwar nicht erforderlich, empfiehlt Apple nachdrücklich Mehrschicht-Images in den Fokus erhalten kann andere Elemente in Ihrer app Benutzeroberfläche implementieren.

### <a name="enabling-parallax"></a>Parallaxen aktivieren

Die `UIImageView` Steuerelement (oder ein beliebiges Steuerelement, das von erbt `UIImageView`) unterstützt automatisch den Parallaxeneffekt. Standardmäßig ist diese Unterstützung deaktiviert, zu aktivieren, verwenden Sie den folgenden Code:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Mit dieser Eigenschaft auf festgelegt `true`, den Image View erhalten automatisch den Parallaxeneffekt, wenn sie vom Benutzer und den Fokus ausgewählt wird.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Konzept der Fokus und wie diese verwendet werden, um die Behandlung der Navigation in einer Xamarin.tvOS-app-Benutzeroberfläche behandelt. Sie untersuchen, wie die integrierten TvOS Navigationssteuerelemente Fokus, hervorheben und Auswahl verwenden, um die Navigation zu ermöglichen. Als Nächstes untersucht es wie den Fokus mit der Parallax und Bilder mit Ebenen verwendet werden kann, um visuelle Hinweise für den aktuellen Status der Navigation für den Endbenutzer bereitzustellen. Schließlich überprüft sie arbeiten mit Fokus, den Fokus Updates des Fokus in Sammlungen und Parallax aktivieren.




## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
