---
title: Alternative Layoutansichten
description: In diesem Thema wird erläutert, wie Layouts mit versionsverwaltung durch das sein können mithilfe von ressourcenqualifizierer. Beispielsweise kann gibt es sein eine Version eines Layouts, die nur verwendet wird, wenn das Gerät im Querformatmodus ausgeführt wird und eine Layoutversion, die nur für den Hochformatmodus ausgeführt wird.
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 03b80d3fb1ed7c8db108f86b3b3923c20e1d908f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109977"
---
# <a name="alternative-layout-views"></a>Alternative Layoutansichten

_In diesem Thema wird erläutert, wie Version Layouts können Sie mit ressourcenqualifizierer. Erstellen z. B. eine Version eines Layouts, die nur verwendet wird, wenn das Gerät im Querformatmodus ausgeführt wird und eine Layoutversion, die nur für den Hochformatmodus ausgeführt wird._

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="creating-alternative-layouts"></a>Alternative Layouts erstellen

Beim Klicken auf die **Alternative Layoutansicht** Symbol (auf der linken Seite des **Gerät**), ein Vorschaufenster wird geöffnet, um die Liste der alternative Layouts, die in Ihrem Projekt verfügbar. Wenn es keine alternative Layouts sind, das **Standard** Ansicht angezeigt: 

[![Alternative Ansicht Layoutbereich](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "Ansichtsbereich alternative Layouts")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

Beim Klicken auf das grüne Pluszeichen neben **neue Version**, **Layoutvariation erstellen** Dialogfeld wird geöffnet, sodass Sie die ressourcenqualifizierer für diese layoutvariation auswählen können: 

[![Layoutvariation erstellen](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "layoutvariation erstellen")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

Im folgenden Beispiel die ressourcenqualifizierer für **Bildschirmausrichtung** nastaven NA hodnotu **Querformat**, und die **Bildschirmgröße** geändert wird, um **groß**. Dies erstellt eine neue Layoutversion mit dem Namen **große Flächen**:

[![Groß – Land Variation](alternative-layout-views-images/vs/03-large-land-sml.png "Large-Land-Variante")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

Beachten Sie, dass die Auswirkungen der Auswahl der Ressource Qualifizierer wird im Vorschaufenster auf der linken Seite angezeigt. Auf **hinzufügen** alternative Layout erstellt und wechselt den Designer auf das Layout. Die **Alternative Layoutansicht** Vorschaufenster gibt an, welches Layout in den Designer über einen kleinen Pfeil geladen wird, wie im folgenden Screenshot gezeigt: 

[![Anzeige für das geladene](alternative-layout-views-images/vs/04-new-layout-sml.png "den Indikator für geladen")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)


## <a name="editing-alternative-layouts"></a>Bearbeiten die alternative layouts

Wenn Sie alternative Layouts erstellen, ist es oft wünschenswert, eine einzelne Änderung vornehmen, die für alle abgeänderte Versionen eines Layouts gilt. Beispielsweise empfiehlt es sich um den Text der Schaltfläche in Gelb in allen Layouts zu ändern. Wenn Sie eine große Anzahl von Layouts und Sie eine einzelne Änderung für alle Versionen weitergegeben werden müssen, kann die Wartung schnell mühsam und fehleranfällig werden.

Um die Wartung mehrerer Layout-Versionen zu vereinfachen, die der Designer bietet eine **mit mehreren bearbeiten** Modus, in dem die Änderungen über einen oder mehrere Layouts hinweg verteilt. Wenn mehr als ein Layout vorhanden ist, wird die **mit mehreren bearbeiten** Symbol wird angezeigt: 

[![Symbol "mit mehreren bearbeiten"](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "mit mehreren bearbeiten-Symbol")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

Beim Klicken auf die **mit mehreren bearbeiten** Symbol Linien angezeigt werden, die angeben, dass die Layouts (siehe unten) verknüpft sind, d. h. Wenn Sie eine Änderung an einem Layout vornehmen, diese Änderung wird weitergegeben auf alle verknüpften Layouts. Sie können die Verknüpfung aller Layouts aufheben, indem Sie auf der angegeben wird, im folgenden Screenshot eingekreist: 

[![Verknüpfung aller Layouts aufheben](alternative-layout-views-images/vs/06-multi-linked-sml.png "Verknüpfung aller Layouts aufheben")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

Wenn Sie mehr als zwei Layouts haben, können Sie selektiv Umschalten der Schaltfläche "Bearbeiten" auf der linken Seite jedes Layout-Preview, um zu bestimmen, welche Layouts miteinander verknüpft sind. Beispielsweise sollten Sie eine einzelne Änderung vornehmen, die verteilt auf das erste und letzte von drei Layouts, würden Sie zuerst die mittlere Layout aufheben, wie hier gezeigt: 

[![Unlink mittleren Layout](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "mittleren Layout Verknüpfung aufheben")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)

In diesem Beispiel einer Änderung auf die **Standard** oder **lange** Layout wird weitergegeben werden, auf das Layout von anderen aber nicht für die **große Flächen** Layout.

### <a name="multi-edit-example"></a>Beispiel für die Multi-Bearbeitung 

Wenn Sie eine Änderung an einem Layout vornehmen, wird dieselbe Änderung in der Regel auf alle anderen verknüpften Layouts weitergegeben. Z. B. Hinzufügen eines neuen `TextView` Widget aus, das die **Standard** Layout und ändern den Text von string in `Portrait` bewirkt, dass die gleiche Änderung an alle verknüpften Layouts vorgenommen werden. Sieht wie es der **Standard** Layout: 

[![Hinzufügen der TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView hinzufügen")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
Die `TextView` wird ebenfalls hinzugefügt der **große Flächen** Layout anzeigen, da es verknüpft ist die **Standard** Layout: 

[![Querformat TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "TextView-Landschaft")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
Aber was geschieht, wenn Sie eine Änderung vornehmen, die nur ein Layout lokal ist (d. h. nicht sollen die Änderungen an die anderen Layouts weitergegeben werden)? Zu diesem Zweck müssen Sie das Layout Verknüpfung aufheben, das Sie möchten, ändern, bevor Sie diese zu ändern, wie unten beschrieben. 

### <a name="making-local-changes"></a>Lokale Änderungen 

Angenommen, Sie möchten beide Layouts, damit die hinzugefügte `TextView`, aber wir möchten auch ändern, die Textzeichenfolge in die **große Flächen** Layout `Landscape` statt `Portrait`. Wenn wir diese Änderung vornehmen **große Flächen** während beide Layouts verknüpft sind, wird die Änderung übermitteln, an die **Standard** Layout. Aus diesem Grund müssen wir zuerst die beiden Layouts aufheben, bevor wir die Änderung vorgenommen werden. Wir ändern, wenn den Text in **große Flächen** zu `Landscape`, der Designer kennzeichnet diese Änderung mit einem roten Rahmen um anzugeben, dass die Änderung lokal auf die **große Flächen** Layout und *nicht* weitergegeben, die an die **Standard** Layout: 

[![Lokale Änderung](alternative-layout-views-images/vs/10-local-change-sml.png "lokalen Änderung")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
Beim Klicken auf die **Standard** Layout an, die `TextView` Textzeichenfolge nastaven NA hodnotu weiterhin `Portrait`. 

## <a name="handling-conflicts"></a>Behandeln von Konflikten 

Wenn Sie sich entscheiden, so ändern Sie die Farbe des Texts in der **Standard** Layout in Grün, sehen Sie ein Warnsymbol in der verknüpften Layout angezeigt werden. Durch Klicken auf das Layout wird das Layout, um den Konflikt anzuzeigen. Das Widget, das den Konflikt verursacht hat, wird mit einem roten Rahmen hervorgehoben und die folgende Meldung angezeigt: *kürzlich vorgenommene Änderungen haben Konflikte in diesem alternativen Layout verursacht*. 

[![In Konflikt stehende Änderung](alternative-layout-views-images/vs/11-conflicting-change-sml.png "in Konflikt stehende Änderung")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
Ein *Konfliktursache* wird auf der rechten Seite des Widgets zur Erläuterung des Konflikts angezeigt: 

[![Warnung-linkkonflikt](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Der Konflikt-Feld zeigt die Liste der Eigenschaften, die geändert wurden, und ihre Werte aufgeführt. Auf **Konflikt ignorieren** gilt die Eigenschaftenänderung nur für dieses Widget. Auf **übernehmen** gilt die Eigenschaftenänderung für dieses Widget auch über das Widget Entsprechung in der verknüpften **Standard** Layout. Wenn alle eigenschaftsänderungen angewendet werden, wird der Konflikt automatisch verworfen. 

### <a name="view-group-conflicts"></a>Die Gruppe Konflikte anzeigen 

Eigenschaftenänderungen sind nicht die einzige Quelle für Konflikte. Beim Einfügen oder Entfernen von Widgets, können Konflikte erkannt werden. Z. B., wenn die **große Flächen** Layout von aufgehoben wird die **Standard** Layout und die `TextView` in die **große Flächen** Layout ist Drag & Drop über die `Button`, der Designer kennzeichnet das Widget "verschoben", um den Konflikt anzugeben:

[![Anzeigen von Gruppe-Konflikt](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "Gruppe Konflikt anzeigen")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
Allerdings besteht kein Marker auf der `Button`. Obwohl die Position des der `Button` geändert hat, die `Button` zeigt keine angewendeten Änderungen, die für spezifisch sind die **große Flächen** Konfiguration. 

Wenn eine `CheckBox` hinzugefügt wird die **Standard** Layout mit Ausrichtung von einem anderen Konflikt wird generiert und ein Warnsymbol wird angezeigt, über die **große Flächen** Layout: 

[![Kontrollkästchen-Konflikt](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "Kontrollkästchen-Konflikt")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
Klicken auf die **große Flächen** Layout zeigt den Konflikt. Die folgende Meldung angezeigt: *kürzlich vorgenommene Änderungen haben Konflikte in diesem alternativen Layout verursacht*: 

[![ALT-Layout-Konflikt](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt Layout-Konflikt")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)

Darüber hinaus wird der Konflikt-Feld die folgende Meldung angezeigt:

[![Konfliktmeldung](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Hinzufügen der `CheckBox` verursacht einen Konflikt, da die **große Flächen** Layout wurde in der `LinearLayout` , die Sie enthält. In diesem Fall das Feld Konflikt zeigt jedoch das Widget, das soeben eingefügt wurde die **Standard** Layout (die `CheckBox`).

Wenn Sie auf **Konflikt ignorieren**, der Designer löst den Konflikt, können das Widget angezeigt wird, klicken Sie im Konflikt, Drag & Drop in das Layout, in dem das Widget nicht vorhanden ist (in diesem Fall die **große Flächen**  Layout): 

[![Gruppe Konflikt aufgelöst](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "Gruppe Konflikt behoben")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)

Wie im vorherigen Beispiel mit der `Button`, `CheckBox` besitzt keine Marker Rot ändern, da nur die `LinearLayout` umfasst Änderungen, die in angewendet wurden die **große Flächen** Layout.



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="creating-alternative-layouts"></a>Alternative Layouts erstellen

Beim Klicken auf die **Alternative Layoutansicht** Symbol (auf der linken Seite des **Gerät**), ein Vorschaufenster wird geöffnet, um die Liste der alternative Layouts, die in Ihrem Projekt verfügbar. Wenn es keine alternative Layouts sind, das **Standard** Ansicht angezeigt: 

[![Alternative Ansicht Layoutbereich](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

Beim Klicken auf das grüne Pluszeichen neben **neue Version**, **Layoutvariation erstellen** Dialogfeld wird geöffnet, sodass Sie die ressourcenqualifizierer für diese layoutvariation auswählen können: 

[![Layoutvariation erstellen](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

Im folgenden Beispiel die ressourcenqualifizierer für **Bildschirmausrichtung** nastaven NA hodnotu **Querformat**, und die **Bildschirmgröße** geändert wird, um **groß**. Dies erstellt eine neue Layoutversion mit dem Namen **große Flächen**:

[![Groß – Land-Variante](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

Beachten Sie, dass die Auswirkungen der Auswahl der Ressource Qualifizierer wird im Vorschaufenster auf der linken Seite angezeigt. Auf **hinzufügen** alternative Layout erstellt und wechselt den Designer auf das Layout. Die **Alternative Layoutansicht** Vorschaufenster gibt an, welches Layout in den Designer über einen kleinen Pfeil geladen wird, wie im folgenden Screenshot gezeigt: 

[![Indikator für geladen](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>Bearbeiten die alternative layouts

Wenn Sie alternative Layouts erstellen, ist es oft wünschenswert, eine einzelne Änderung vornehmen, die für alle abgeänderte Versionen eines Layouts gilt. Beispielsweise empfiehlt es sich um den Text der Schaltfläche in Gelb in allen Layouts zu ändern. Wenn Sie eine große Anzahl von Layouts und Sie eine einzelne Änderung für alle Versionen weitergegeben werden müssen, kann die Wartung schnell mühsam und fehleranfällig werden.

Um die Wartung mehrerer Layout-Versionen zu vereinfachen, die der Designer bietet eine **mit mehreren bearbeiten** Modus, in dem die Änderungen über einen oder mehrere Layouts hinweg verteilt. Wenn mehr als ein Layout vorhanden ist, wird die **mit mehreren bearbeiten** Symbol wird angezeigt: 

[![Symbol "mit mehreren bearbeiten"](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

Beim Klicken auf die **mit mehreren bearbeiten** Symbol Linien angezeigt werden, die angeben, dass die Layouts (siehe unten) verknüpft sind, d. h. Wenn Sie eine Änderung an einem Layout vornehmen, diese Änderung wird weitergegeben auf alle verknüpften Layouts. Sie können die Verknüpfung aller Layouts aufheben, indem Sie auf der angegeben wird, im folgenden Screenshot eingekreist: 

[![Verknüpfung aller Layouts aufheben](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

Wenn Sie mehr als zwei Layouts haben, können Sie selektiv Umschalten der Schaltfläche "Bearbeiten" auf der linken Seite jedes Layout-Preview, um zu bestimmen, welche Layouts miteinander verknüpft sind. Beispielsweise sollten Sie eine einzelne Änderung vornehmen, die verteilt auf das erste und letzte von drei Layouts, würden Sie zuerst die mittlere Layout aufheben, wie hier gezeigt: 

[![Verknüpfung des mittleren Layouts aufheben](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)

In diesem Beispiel einer Änderung auf die **Standard** oder **lange** Layout wird weitergegeben werden, andere Layout aber nicht für die **große Flächen** Layout. 

### <a name="multi-edit-example"></a>Beispiel für die Multi-Bearbeitung 

Wenn Sie eine Änderung an einem Layout vornehmen, wird dieselbe Änderung in der Regel auf alle anderen verknüpften Layouts weitergegeben. Z. B. Hinzufügen eines neuen `TextView` Widget aus, das die **Standard** Layout und ändern den Text von string in `Portrait` bewirkt, dass die gleiche Änderung an alle verknüpften Layouts vorgenommen werden. Sieht wie es der **Standard** Layout: 

[![TextView hinzufügen](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)

Die `TextView` wird ebenfalls hinzugefügt der **große Flächen** Layout anzeigen, da es verknüpft ist die **Standard** Layout: 

[![Querformat TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
Aber was geschieht, wenn Sie eine Änderung vornehmen, die nur ein Layout lokal ist (d. h. nicht sollen die Änderungen an die anderen Layouts weitergegeben werden)? Zu diesem Zweck müssen Sie das Layout Verknüpfung aufheben, das Sie möchten, ändern, bevor Sie diese zu ändern, wie unten beschrieben. 

### <a name="making-local-changes"></a>Lokale Änderungen 

Angenommen, Sie möchten beide Layouts, damit die hinzugefügte `TextView`, aber wir möchten auch ändern, die Textzeichenfolge in die **große Flächen** Layout `Landscape` statt `Portrait`. Wenn wir diese Änderung vornehmen **große Flächen** während beide Layouts verknüpft sind, wird die Änderung übermitteln, an die **Standard** Layout. Aus diesem Grund müssen wir zuerst die beiden Layouts aufheben, bevor wir die Änderung vorgenommen werden. Wir ändern, wenn den Text in **große Flächen** zu `Landscape`, der Designer kennzeichnet diese Änderung mit einem roten Rahmen um anzugeben, dass die Änderung lokal auf die **große Flächen** Layout und *nicht* weitergegeben, die an die **Standard** Layout: 

[![Lokale Änderung](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)

Beim Klicken auf die **Standard** Layout an, die `TextView` Textzeichenfolge nastaven NA hodnotu weiterhin `Portrait`. 

## <a name="handling-conflicts"></a>Behandeln von Konflikten 

Wenn Sie sich entscheiden, so ändern Sie die Farbe des Texts in der **Standard** Layout in Grün, sehen Sie ein Warnsymbol in der verknüpften Layout angezeigt werden. Durch Klicken auf das Layout wird das Layout, um den Konflikt anzuzeigen. Das Widget, das den Konflikt verursacht hat, wird mit einem roten Rahmen hervorgehoben und die folgende Meldung angezeigt: *kürzlich vorgenommene Änderungen haben Konflikte in diesem alternativen Layout verursacht*. 

[![In Konflikt stehende Änderung](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)

Ein *Konfliktursache* wird auf der rechten Seite des Widgets zur Erläuterung des Konflikts angezeigt: 

[![Warnung-linkkonflikt](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Der Konflikt-Feld zeigt die Liste der Eigenschaften, die geändert wurden, und ihre Werte aufgeführt. Auf **Konflikt ignorieren** gilt die Eigenschaftenänderung nur für dieses Widget. Auf **übernehmen** gilt die Eigenschaftenänderung für dieses Widget auch über das Widget Entsprechung in der verknüpften **Standard** Layout. Wenn alle eigenschaftsänderungen angewendet werden, wird der Konflikt automatisch verworfen. 

### <a name="view-group-conflicts"></a>Die Gruppe Konflikte anzeigen 

Eigenschaftenänderungen sind nicht die einzige Quelle für Konflikte. Beim Einfügen oder Entfernen von Widgets, können Konflikte erkannt werden. Z. B., wenn die **große Flächen** Layout von aufgehoben wird die **Standard** Layout und die `TextView` in die **große Flächen** Layout ist Drag & Drop über die `Button`, der Designer kennzeichnet das Widget "verschoben", um den Konflikt anzugeben:

[![Die Gruppe Konflikt anzeigen](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
Allerdings besteht kein Marker auf der `Button`. Obwohl die Position des der `Button` geändert hat, die `Button` zeigt keine angewendeten Änderungen, die für spezifisch sind die **große Flächen** Konfiguration. 

Wenn eine `CheckBox` hinzugefügt wird die **Standard** Layout mit Ausrichtung von einem anderen Konflikt wird generiert und ein Warnsymbol wird angezeigt, über die **große Flächen** Layout: 

[![Kontrollkästchen-Konflikt](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
Klicken auf die **große Flächen** Layout zeigt den Konflikt. Die folgende Meldung angezeigt: *kürzlich vorgenommene Änderungen haben Konflikte in diesem alternativen Layout verursacht*. 

[![ALT-Layout-Konflikt](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
Darüber hinaus wird der Konflikt-Feld die folgende Meldung angezeigt:

[![Konfliktmeldung](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Hinzufügen der `CheckBox` verursacht einen Konflikt, da die **große Flächen** Layout wurde in der `LinearLayout` , die Sie enthält. In diesem Fall das Feld Konflikt zeigt jedoch das Widget, das soeben eingefügt wurde die **Standard** Layout (die `CheckBox`).

Wenn Sie auf **Konflikt ignorieren**, der Designer löst den Konflikt, können das Widget angezeigt wird, klicken Sie im Konflikt, Drag & Drop in das Layout, in dem das Widget nicht vorhanden ist (in diesem Fall die **große Flächen**  Layout): 

[![Gruppe Konflikt behoben](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
Wie im vorherigen Beispiel mit der `Button`, `CheckBox` besitzt keine Marker Rot ändern, da nur die `LinearLayout` umfasst Änderungen, die in angewendet wurden die **große Flächen** Layout.


-----


### <a name="conflict-persistence"></a>Konflikt-Persistenz

Konflikte werden als XML-Kommentare, in der Layoutdatei beibehalten, wie hier gezeigt:

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Wenn ein Projekt geschlossen und erneut geöffnet wird, alle Konflikte weiterhin müssen es &ndash; auch diejenigen, die wurden ignoriert.



