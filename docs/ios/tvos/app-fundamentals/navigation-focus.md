---
title: Arbeiten mit tvos. außerdem wurden Navigations- und der Fokus in Xamarin
description: Dieser Artikel behandelt das Konzept der Fokus und deren Verwendung und Behandeln der Navigation innerhalb einer Xamarin.tvOS-app.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2af3306b605ee802b72b159d5f2759d71d292a64
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788731"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Arbeiten mit tvos. außerdem wurden Navigations- und der Fokus in Xamarin

_Dieser Artikel behandelt das Konzept der Fokus und deren Verwendung und Behandeln der Navigation innerhalb einer Xamarin.tvOS-app._


Dieser Artikel umfasst das Konzept der [Fokus](#Focus-and-Selection) und wie diese verwendet werden, behandeln [Navigation](#Navigation) in einer Xamarin.tvOS-app-Benutzeroberfläche. Untersucht, wie die integrierte Steuerelemente zur Seitennavigation tvos. außerdem wurden Fokus, Hervorhebung und Auswahl verwenden, um Ihre Xamarin.tvOS app Benutzernavigation-Schnittstelle bereitstellen.

[![](navigation-focus-images/intro01.png "tvos. außerdem wurden apps Benutzernavigation-Schnittstelle")](navigation-focus-images/intro01.png#lightbox)

Wir führen anschließend eine Betrachtung der Verwendung den Fokus mit [Parallax](#Focus-and-Parallax) und *Bilder in den Ebenen* visuelle Hinweise für den aktuellen Status der Navigation für den Endbenutzer bereitstellen.

Schließlich betrachten wir arbeiten mit [Fokus](#Working-with-Focus), [Fokus Updates](#Working-with-Focus-Updates), [Fokus Handbücher](#Working-with-Focus-Guides), [Fokus in Auflistungen](#Working-with-Focus-in-Collections) und [ Aktivieren der Parallax](#Enabling-Parallax) für Image-Sichten in Ihren apps Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Navigation

Benutzer der app Xamarin.tvOS werden nicht werden interagieren mit der Schnittstelle direkt als mit iOS, in dem sie tippen Sie auf Bilder auf dem Gerätebildschirm jedoch indirekt von über den Raum mithilfe, der [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Sie müssen Bedenken Sie dies beim Entwerfen der Benutzeroberfläche der app, damit es natürlich fließen, doch der Benutzer, die mehr über das Apple TV-Erlebnis bleibt.

Eine erfolgreiche tvos. außerdem wurden app implementiert Navigation auf eine Weise, die den Zweck der app und die Struktur der Daten, wenn, die es ohne auf die Navigation selbst vorlegt, reibungslos unterstützt. Entwerfen Sie die Navigation, sodass natürliche und vertraute das Gefühl, ohne die Benutzeroberfläche dominieren oder zeichnen den Fokus von der Inhalt und die benutzerfreundlichkeit der apps.

[![](navigation-focus-images/nav01.png "Die tvos. außerdem wurden für die app")](navigation-focus-images/nav01.png#lightbox)

Während einer Apple TV, die Benutzer in der Regel mit einem stapelsatz an Bildschirme, jeweils eine bestimmte Gruppe von Inhalt darstellen navigiert. Wiederum jeder Bildschirm für neuer führt möglicherweise zu einem oder mehreren untergeordneten Bildschirmen von Inhalten mithilfe der standardmäßigen Benutzeroberflächen-Steuerelemente wie z. B. [Schaltflächen](~/ios/tvos/user-interface/buttons.md), [Registerkarte Balken](~/ios/tvos/user-interface/tab-bars.md), Tabellen, [Auflistungsansichten](~/ios/tvos/user-interface/collection-views.md) oder [ Teilen von Ansichten](~/ios/tvos/user-interface/split-views.md).

Mit jeder Bildschirm für neue Daten wechselt der Benutzer in diesem Stapel von Bildschirmen tiefer. Mithilfe der **Menü** Schaltfläche auf der Remoteinstanz Siri diese können navigieren rückwärts durch den Stapel, um im Hauptmenü zu einer vorherigen Bildschirm zurückzukehren.

Apple empfiehlt, halten Folgendes beachten Sie beim Entwerfen der Navigation für Ihre app tvos. außerdem wurden:

- **Layout der Navigation zu stellen Suchen nach Inhalt schnell und einfach** -Benutzern Zugriff auf Inhalte innerhalb Ihrer app in die geringste Anzahl der abtaststellen, klickt und Kundenkarte wie möglich. Vereinfachen der Navigation und Organisieren von Inhalt an die minimale Anzahl der Bildschirme.
- **Erstellen Sie eine Fluid Schnittstelle mithilfe von Touch** -stellen Sie sicher, dass ein Benutzer zwischen verschieben kann _den Fokus erhalten kann Elemente_ mit minimalen Unstimmigkeiten mithilfe der geringstmöglichen Anzahl von Gesten möglich.
- **Entwurf mit dem Fokus Bedenken** -da der Benutzer mit Inhalt über den Raum interagiert, müssen sie den Fokus auf ein Element der Benutzeroberfläche zu verschieben, bevor er mithilfe von Siri Remote interagieren. Benutzer erhalten Ihre app frustriert, wenn zu viele Gesten zum Erreichen ihrer Ziele erforderlich ist.
- **Abwärtskompatibilität Navigation über die Schaltfläche "im Menü" bereitstellen,** –, erstellen eine Lösung einfache und vertraute, damit Benutzer sich über die Siri Remote navigieren **Menü** Schaltfläche. Drücken Sie die **Menü** Schaltfläche sollte immer zum vorherigen Bildschirm zurückzukehren oder zu der app-Hauptmenü zurückkehren. Drücken Sie auf die app der obersten Ebene, die **Menü** Schaltfläche sollte auf dem Bildschirm Apple TV-Startseite zurückzukehren.
- **Vermeiden Sie in der Regel eine zurück-Schaltfläche anzeigen** : Da Drücken der **Menü** Schaltfläche auf der Remoteinstanz Siri rückwärts durch den Stapel Bildschirm navigiert, vermeiden Sie eine zusätzliche Kontrolle, der ein dieses Verhalten Duplikat, anzeigen. Eine Ausnahme von dieser Regel wird für den Erwerb von Bildschirme oder Bildschirme mit destruktiven Aktionen (z. B. das Löschen von Inhalten), in dem ein **"Abbrechen"** Schaltfläche ebenfalls angezeigt werden soll.
- **Zeigen Sie auf einen einzelnen Bildschirm, anstatt viele große Sammlungen** -der Siri Remote wurde darauf ausgelegt, Verschieben über eine umfangreiche Auflistung von Inhalten, die schnell und einfach mit Gesten. Wenn Ihre app mit eine umfangreiche Auflistung von den Fokus erhalten kann Elemente funktioniert, erwägen Sie, halten sie auf einen einzelnen Bildschirm statt unterbrochen werden in vielen Bildschirme, die weitere Navigation im Rahmen des Benutzers erfordern.
- **Verwenden von Standardsteuerelementen für die Navigation** -erneut aus, um einen einfachen und vertrauten benutzererfahrung möglichst zu erstellen, verwenden integrierte `UIKit` Steuerelemente wie Seitensteuerelemente, Registerkarte Balken, segmentierte Steuerelemente, Tabellensichten, Auflistungsansichten und Teilen Ansichten für die Navigation für Ihre app. Da der Benutzer bereits mit diesen Elementen vertraut sind, werden sie intuitiv navigieren Sie Ihrer app sein.
- **Horizontale Inhaltsnavigation begünstigen** -aufgrund der Natur der Apple TV, von links nach rechts auf der Remoteinstanz Siri Streifen ist besser als oben oder unten. Erwägen Sie diese Option aus, wenn Inhalt Layouts für Ihre app zu entwerfen.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>Fokus und Auswahl

Auf der Apple TV, ein Bild, Schaltfläche oder anderen Benutzeroberflächenelement gilt _im Fokus_ wird er das Ziel der aktuellen Navigation.

[![](navigation-focus-images/focus01.png "Fokus und Auswahl-Beispiel")](navigation-focus-images/focus01.png#lightbox)

Im Gegensatz zu iOS-Geräte, auf dem der Benutzer direkt mit Elementen auf dem Gerät Touchscreen, interagieren, Benutzer interagieren mit tvos. außerdem wurden die Elemente aus, über den Raum, der mithilfe von Siri Remote. Um darzustellen, und Behandeln dieser Benutzerinteraktionen, die Apple TV verwendet eine _Fokus_ basierendes Modell.

Mit Gesten und Schaltfläche drückt, auf die [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), der Benutzer den Fokus von einem Benutzeroberflächenautomatisierungs-Element in eine andere. Sobald der Fokus auf das gewünschte Element verschoben wurde, klickt der Benutzer zum Wählen Sie den Inhalt oder aktivieren die Aktion, die durch dieses Element dargestellt wird.

Als fokusänderungen sind feine Animationen und Auswirkungen (z. B. die Parallax Auswirkung auf Bilder) verwendet, die Benutzeroberfläche Element eindeutig zu identifizieren, das gerade den Fokus besitzt.

Apple hat die folgenden Vorschläge für die Arbeit mit Fokus und Auswahl:

- **Verwenden Sie integrierte Benutzeroberflächen-Steuerelemente für Bewegungseffekte** – mit `UIKit` und der Fokus-API in der Benutzeroberfläche, das Fokus Modell gilt automatisch der Standard-Bewegung und visuelle Effekte für Ihre UI-Elemente. Dadurch sind die Meinung sind, dass app, systemeigene und Benutzern der Apple TV-Plattform vertraut und für das Verschieben von flüssig und intuitive zwischen den Fokus erhalten kann Elemente ermöglicht.
- **Verschieben des Fokus in erwartet Richtungen** -auf Apple TV fast jedes Element verwendet die indirekte Manipulation. Beispielsweise verwendet der Benutzer die Siri Remote, um den Fokus zu verschieben, und das System automatisch verschiebt die Schnittstelle, um das aktuell den Fokus Element sichtbar bleiben. Wenn Ihre app diese Art der Interaktion implementiert wird, stellen Sie sicher, dass der Fokus in Richtung der Bewegung des Benutzers. Wenn der Benutzer navigieren Sie mit rechten Maustaste auf der Fokus Siri Remote rechts verschieben sollte (wodurch den Bildschirm, um einen Bildlauf zu der linken Seite möglicherweise). Die einzige Ausnahme dieser Regel wird die Fullscreen-Elemente, die direkte Bearbeitung verwenden (wobei verschiebt streichen das Element nach oben).
- **Stellen Sie sicher, dass das Element mit Fokus Obvious ist** -da Ihre Benutzer die Interaktion mit der UI-Elemente aus der Ferne verwendet werden, ist es wichtig, dass das aktuell den Fokus Element abhebt. In der Regel Dadurch wird automatisch vom behandelt integrierte `UIKit` Elemente. Verwenden Sie für benutzerdefinierte Steuerelemente Funktionen, z. B. Elementgröße oder überschatten, um den Fokus anzuzeigen.
- **Verwenden Parallax, konzentriert sich Elemente-reaktionsfähig stellen** : "klein", zirkuläre Gesten auf der Remoteinstanz Siri führen zu umfassend, Echtzeit Verschiebung des Elements mit Fokus. Dies [Parallax Auswirkungen](#Focus-and-Parallax) ist integriert `UIKit` _Bilder in den Ebenen_ der Benutzer einen Eindruck von der Verbindung mit dem markierten Elements zugewiesen werden soll.
- **Erstellen Sie den Fokus erhalten kann Elemente der entsprechenden Größe** -große Elementen mit ausreichend Abstand sind einfacher zu auswählen und als kleinere Elemente darin navigieren.
- **Entwerfen von UI-Element, suchen Sie gute entweder mit Fokus oder Unfocused** -i. d. r. Apple TV das Element mit Fokus darstellt, durch Erhöhen der Größe. Stellen Sie sicher, dass Ihre apps Benutzeroberflächenelemente beliebiger Größe Präsentation hervorragend Aussehen und die ggf. angeben, Bestände für größere Größe Elemente.
- **Fokus Änderungen Fluidly darstellen** -Animation gleichmäßig zwischen Elementen ausgeblendet verwenden **fokussiert** und **Unfocused** Zustand Übergänge wird jedoch beibehalten.
- **Einen Cursor angezeigt** -Benutzer erwarten, dass Ihre app-Benutzeroberfläche, die über den Fokus zu navigieren und nicht durch einen Cursor auf dem Bildschirm bewegen. Ihre Benutzeroberfläche sollten immer der Fokus-Modell verwenden, um eine konsistente benutzerumgebung darzustellen.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Arbeiten mit Fokus

Es gibt möglicherweise Fälle, in denen Sie möchten ein benutzerdefiniertes Steuerelement zu erstellen, das ein Element den Fokus erhalten kann verwendet werden kann. Wenn also überschreiben die `CanBecomeFocused` Eigenschaft und die Rückgabewerte `true`, ansonsten return `false`. Zum Beispiel:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

Zu einem beliebigen Zeitpunkt können Sie die `Focused` Eigenschaft ein `UIKit` Steuerelement, um festzustellen, ob das aktuelle Element ist. Wenn `true` das UI-Element zurzeit den Fokus besitzt, andernfalls nicht. Zum Beispiel:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Während Sie den Fokus zu einem anderen Benutzeroberflächenelement direkt über Code verschieben können, können Sie angeben, welche Benutzeroberflächenelement zunächst den Fokus erhält, beim Laden eines Bildschirms durch Festlegen seiner `PreferredFocusedView` Eigenschaft `true`. Zum Beispiel:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Arbeiten mit Fokus Updates 

Wenn der Benutzer den Fokus Zuweisen von einem Benutzeroberflächenautomatisierungs-Element in eine andere (z. B. als Reaktion auf eine Geste für die Remoteinstanz Siri) verursacht eine _Fokus Aktualisierungsereignis_ an das Element den Fokus verlieren und das Element den Fokus erhalten gesendet werden.

Für benutzerdefinierte Elemente, die von erben `UIView` oder `UIViewController`, Sie können verschiedene Methoden zur Bearbeitung der Fokus Aktualisierungsereignis überschreiben:

- **DidUpdateFocus** – diese Methode wird aufgerufen, wenn Sie jederzeit die Sicht erhält, oder den Fokus verliert.
- **ShouldUpdateFocus** -mit dieser Methode können Sie definieren, in denen Fokus verschoben werden darf.

Um anzufordern, dass das Modul den Fokus verschiebt den Fokus zurück an die `PreferredFocusedView` Benutzeroberflächenelement, rufen die `SetNeedsUpdateFocus` -Methode des Controllers anzeigen.

> [!IMPORTANT]
> Aufrufen von `SetNeedsUpdateFocus` hat nur Auswirkungen, wenn die View-Controller ist für aufgerufenen die Sicht enthält, die gerade den Fokus besitzt.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Arbeiten mit Fokus Führungslinien

Das Modul für den Fokus tvos. außerdem wurden integriert eignet sich zur Behandlung von Bewegungen zwischen Elementen, die in einem Raster horizontale und vertikale liegen. In der Regel müssen Sie nichts mehr als das Hinzufügen eines der Elemente der Benutzeroberfläche auf den Schnittstellenentwurf und das Modul den Fokus automatisch den Verkehr zwischen diesen Elementen ohne Eingriffe behandelt.

Allerdings möglicherweise Zeiten, aufgrund der notwendigkeiten des UI-Entwurfs, wenn Elemente der Benutzeroberfläche nicht auf eine horizontale und vertikale Raster fallen und möglicherweise daran, dass sie miteinander diagonalen sind nicht zugegriffen werden kann. Dies liegt daran, dass das Modul den Fokus entworfen wurde, um einfache nach oben, unten, links und rechts Verschiebung zwischen Elementen der Benutzeroberfläche nur zu behandeln.

Führen Sie die folgenden Benutzeroberflächenlayout für ein Beispiel:

 [![](navigation-focus-images/guide01.png "Arbeiten mit Fokus Handbücher-Beispiel")](navigation-focus-images/guide01.png#lightbox)
 
Da die **mehr Informationen** Schaltfläche liegt nicht in einem horizontalen und vertikalen Raster mit der **kaufen** Schaltfläche wäre es nicht an den Benutzer zugegriffen werden kann. Allerdings Dies kann leicht behoben werden mithilfe einer _Fokus Handbuch_ Bewegung Hinweise an das Modul für den Fokus zu übermitteln. 

Eine Anleitung für den Fokus (`UIFocusGuide`) stellt einen nicht sichtbaren Bereich der Ansicht als Focusable an das Modul für den Fokus, wodurch den Fokus zu einer anderen Ansicht umgeleitet werden.

So, um das oben genannte Beispiel zu lösen, der folgende Code konnte hinzugefügt werden die View-Controller, erstellen Sie eine Anleitung den Fokus zwischen der **mehr Infos** und **kaufen** Schaltflächen:

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

Zunächst wird ein neues `UIFocusGuide` erstellt und hinzugefügt, der Ansicht Layout Handbuch Auflistung mithilfe der `AddLayoutGuide` Methode.

Als Nächstes im Fokus Handbuch oben, links, Breite und Höhe Anker relativ zum angepasst werden die **mehr Infos** und **kaufen** Schaltflächen, um zwischen ihnen zu positionieren. Thema

[![](navigation-focus-images/guide02.png "Beispiel für den Fokus-Handbuch")](navigation-focus-images/guide02.png#lightbox)

Es ist auch wichtig zu beachten, dass die neuen Einschränkungen werden aktiviert wird, wie sie durch Festlegen von erstellt werden ihre `Active` Eigenschaft `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Mit dem neuen Fokus Guide hergestellt und der Ansicht, die View-Controller hinzugefügt `DidUpdateFocus` Methode kann überschrieben werden, und der folgende Code hinzugefügt wird, verschieben Sie zwischen der **mehr Informationen** und **kaufen** Schaltflächen:

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

Zuerst wird dieser Code-Get der `NextFocusedView` aus der `UIFocusUpdateContext` in übergeben wurde (`context`). Wenn diese Ansicht ist `null`, keine Verarbeitung erforderlich ist, und die Methode wurde beendet.

Als Nächstes wird die `nextFocusableItem` ausgewertet wird. Sofern es entweder entspricht der **mehr Informationen** oder **kaufen** Schaltflächen, den Fokus wird gesendet, um den entgegengesetzten Schaltfläche mithilfe der Fokus Anleitung `PreferredFocusedView` Eigenschaft. Zum Beispiel:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

Wenn weder die Quelle der Schicht Fokus ist die `PreferredFocusedView` Eigenschaft gelöscht wird:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Arbeiten mit dem Fokus im Sammlungen

Bei der Entscheidung, ob ein einzelnes Element im Fokus festgelegt werden kann oder keine `UICollectionView` oder ein `UITableView`, müssen Sie Methoden überschreiben die `UICollectionViewDelegate` oder `UITableViewDelegate` bzw. Zum Beispiel:

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

Die `CanFocusItem` -Methode zurückkehrt `true` das aktuelle Element den Fokus sein kann, andernfalls gibt, `false`.

Gegebenenfalls ein `UICollectionView` oder ein `UITableView` legen Sie zum Speichern und Wiederherstellen Fokus bis zum letzten Element beim verliert und erhält den Fokus wieder, die `RemembersLastFocusedIndexPath` Eigenschaft `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Fokus und Parallax

Wie bereits erwähnt, ein Element der Benutzeroberfläche gilt _im Fokus_ Wenn es das aktuelle Element ist, während ein Navigationsereignis. In der Regel als ein Element den Fokus besitzt, wird dessen Größe leicht erhöht wird und ist visuell Erhöhung der Rechte mithilfe einer Schattenkopie. 

Wenn der Benutzer eine Geste langsame, kreisförmige auf der Remoteinstanz Siri herstellt, wird das Element mit Fokus in Echtzeit in Reaktion auf diese Bewegung sway. Wie die Schlingern auftritt, wird ein beleuchteten Sheen auf seines Abbilds machen die Oberfläche verwendet anscheinend angewendet. Nach einem bestimmten Zeitraum der Inaktivität Blendet alle Inhalte, die Out-of-Focus, und das Element fokussiert wächst auch größere. 

Diese Effekte arbeiten zusammen, um eine geistige Verbindung zwischen den Inhalt auf dem Bildschirm TV und dem Benutzer, die Interaktion mit dem Apple TV aus der Couch bereitstellen.

Auf der Apple TV wird dieser Effekt Parallax im gesamten System verwendet, um einen Eindruck von 3D Tiefe und Dynamics zu bildschärfenmodus Elemente zu übermitteln. tvos. außerdem wurden verwendet eine Reihe von transparent [Bilder in den Ebenen](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) , das verschoben und dynamisch skaliert, um diesen Effekt zu erstellen.

Bilder mit Ebenen sind erforderlich, damit Ihre tvos. außerdem wurden app-Symbol und wird für dynamische oben Regal Inhalte unterstützt. Zwar nicht erforderlich, empfiehlt Apple nachdrücklich überlappende Bilder in aller anderen Elemente den Fokus erhalten kann in der app UI implementieren.

### <a name="enabling-parallax"></a>Parallax aktivieren

Die `UIImageView` -Steuerelement (oder jedes Steuerelement, das von erbt `UIImageView`) unterstützt automatisch die Parallax Auswirkungen. Standardmäßig ist diese Unterstützung deaktiviert, zu aktivieren, verwenden Sie den folgenden Code:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Diese Eigenschaft festgelegt wird, um `true`, die Image-Sicht erhalten automatisch die Parallax Auswirkungen, wenn er, vom Benutzer und den Fokus ausgewählt ist.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Konzept der Fokus und deren Verwendung zum Behandeln der Navigation in einer Xamarin.tvOS-app-Benutzeroberfläche behandelt. Sie untersuchen, wie die integrierte Steuerelemente zur Seitennavigation tvos. außerdem wurden den Fokus, Hervorhebung und Auswahl verwenden, um die Navigation zu ermöglichen. Als Nächstes untersucht es wie den Fokus mit Parallax und Bilder in den Ebenen verwendet werden kann, um visuelle Hinweise für den aktuellen Status der Navigation für den Endbenutzer bereitzustellen. Schließlich überprüft sie arbeiten mit Fokus, Fokus Updates, den Fokus in Sammlungen und Parallax aktivieren.




## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
