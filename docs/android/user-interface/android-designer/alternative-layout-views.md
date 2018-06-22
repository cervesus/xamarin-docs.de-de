---
title: Alternative Layoutansichten
description: In diesem Thema wird erläutert, wie Layouts mit Versionsangabe sein können mithilfe von ressourcenqualifizierer. Beispielsweise treten möglicherweise eine Version eines Layouts, das nur verwendet wird, wenn das Gerät im Querformat ist und eine Layout-Version, die nur für Hochformat ist.
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: d2228169ed5d8575c9e332c85d963fca0400dea8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771160"
---
# <a name="alternative-layout-views"></a>Alternative Layoutansichten

_In diesem Thema wird erläutert, wie Layouts mit Versionsangabe sein können mithilfe von ressourcenqualifizierer. Beispielsweise treten möglicherweise eine Version eines Layouts, das nur verwendet wird, wenn das Gerät im Querformat ist und eine Layout-Version, die nur für Hochformat ist._


## <a name="creating-alternative-layouts"></a>Alternative Layouts erstellen

Beim Klicken auf die **Alternative Layoutansicht** Symbol (auf der linken Seite des **Gerät**), ein Vorschaufenster wird geöffnet, um die Liste der alternative Layouts in Ihrem Projekt zur Verfügung. Wenn es keine alternative Layouts sind der **Standard** Ansicht angezeigt wird: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alternative Ansicht Layoutbereich](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "Alternative Ansicht Layoutbereich")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Alternative Ansicht Layoutbereich](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

-----

Beim Klicken auf die grüne Pluszeichens (+) neben **neue Version**, **erstellen Layout Variation** Dialogfeld wird geöffnet, sodass Sie die ressourcenqualifizierer für die Variation Layout auswählen können: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Layout Variation erstellen](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "Layout Variation erstellen")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Layout Variation erstellen](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

-----


Im folgenden Beispiel der ressourcenqualifizierer für **Bildschirmausrichtung** festgelegt ist, um **Querformat**, und die **Bildschirmgröße** geändert wird, um **groß**. Dies erstellt eine neue Layout-Version, die mit dem Namen **großen Flächen**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Große Flächen Variation](alternative-layout-views-images/vs/03-large-land-sml.png "großen Flächen Variante")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Große-Land-Variante](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

-----


Beachten Sie, dass die Auswirkungen der Auswahl der Ressource Qualifizierer wird auf der linken Seite im Vorschaufenster angezeigt. Auf **hinzufügen** alternative Layout erstellt, und wechselt den Designer auf das Layout. Die **Alternative Layoutansicht** Vorschaufenster gibt an, welche Anordnung in den Designer über eine kleine Zeiger nach rechts geladen wird, wie im folgenden Screenshot dargestellt: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Geladene Layout Indikator](alternative-layout-views-images/vs/04-new-layout-sml.png "geladenen Layout-Indikator")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Indikator für geladen](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

-----



## <a name="editing-alternative-layouts"></a>Bearbeiten die Alternative Layouts

Wenn Sie alternative Layouts erstellen, ist es häufig wünschenswert, eine einzelne Änderung vorzunehmen, die für alle verzweigter Versionen eines Layouts gilt. Beispielsweise empfiehlt es sich so ändern Sie den Text der Schaltfläche in Gelb alle Layouts. Wenn Sie eine große Anzahl von Layouts haben, kann schnell Wartung werden, mühsam und fehleranfällig, wenn Sie eine einzelne Änderung für alle Versionen weitergegeben werden müssen. 

Zur Vereinfachung der Wartung mehrerer Layout-Versionen der Designer bietet eine **mit mehreren bearbeiten** Modus, in dem die Änderungen an eine oder mehrere Layouts propagiert. Wenn mehr als ein Layout vorhanden ist, wird die **mit mehreren bearbeiten** -Symbol wird angezeigt: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Symbol "mit mehreren bearbeiten"](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "mit mehreren bearbeiten-Symbol")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Symbol "mit mehreren bearbeiten"](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

-----


Beim Klicken auf die **mit mehreren bearbeiten** Symbol Linien angezeigt werden, die die Layouts verknüpft sind (wie unten gezeigt) angeben; d. h., wenn Sie ein Layout ändern, diese Änderung wird übernommen, alle verknüpften Layouts. Sie können alle Layouts aufheben, indem Sie auf das Symbol "Kreis" im folgenden Screenshot angegeben: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Aufheben der Verknüpfung aller Layouts](alternative-layout-views-images/vs/06-multi-linked-sml.png "alle Layouts aufheben")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Alle Layouts aufheben](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

-----


Wenn Sie mehr als zwei Layouts haben, können Sie selektiv wechseln die Schaltfläche "Bearbeiten" auf der linken Seite des jedes Layoutvorschau, um zu bestimmen, welche Layouts miteinander verknüpft werden. Angenommen, eine einzelne Änderung vornehmen, die verteilt werden sollen auf das erste und letzte von drei Layouts, würden Sie zuerst die mittlere Layout aufheben, wie hier gezeigt: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Unlink mittleren Layout](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "mittleren Layout Verknüpfung aufheben")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Aufheben der Verknüpfung mittleren layout](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)
 
-----
 

In diesem Beispiel wird eine Änderung vorgenommen, entweder die **Standard** oder **lange** Layout wird weitergegeben werden, andere Layout aber nicht für die **großen Flächen** Layout. 



### <a name="multi-edit-example"></a>Bearbeiten von Multi-Beispiel 

Wenn Sie ein Layout ändern, wird im Allgemeinen die gleiche Änderung auf alle anderen verknüpften Layouts weitergegeben. Z. B. Hinzufügen eines neuen `TextView` Widget auf die **Standard** Layout und ändern den Text-Zeichenfolge zum `Portrait` führt dazu, dass die gleiche Änderung an alle verknüpften Layouts vorgenommen werden müssen. Hier wird die Anzeige der **Standard** Layout: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Hinzufügen von TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView hinzufügen")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![TextView hinzufügen](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)
 
-----
 

Die `TextView` wird ebenfalls hinzugefügt der **großen Flächen** Layout anzeigen, da es verknüpft ist die **Standard** Layout: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Querformat TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "TextView Querformat")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Querformat TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
-----
 

Aber was geschieht, wenn Sie möchten, eine Änderung vorzunehmen, die nur ein Layout lokal ist (d. h. nicht sollen die Änderung an einem beliebigen anderen Layouts weitergegeben werden)? Zu diesem Zweck müssen Sie das Layout aufheben, das ändern, bevor Sie diese zu ändern, wie im folgenden erklärt werden soll. 



### <a name="making-local-changes"></a>Lokale Änderungen 

Angenommen, wir möchten, dass beide Layouts der hinzugefügten haben `TextView`, aber wir möchten auch so ändern Sie die Zeichenfolge in der **großen Flächen** Layout `Landscape` statt `Portrait`. Wenn wir diese Änderung vornehmen **großen Flächen** während beide Layouts verknüpft sind, wird die Änderung weitergegeben werden zurück an die **Standard** Layout. Wir müssen daher zuerst zwei Layouts aufheben, bevor wir die Änderung vorgenommen werden. Wenn wir ändern Sie den Text in **großen Flächen** auf `Landscape`, Designer markiert diese Änderung mit einem roten Rahmen um anzugeben, dass die Änderung für lokal ist die **großen Flächen** Layout und *nicht* weitergegeben, die an die **Standard** Layout: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Lokale](alternative-layout-views-images/vs/10-local-change-sml.png "lokale ändern")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Lokale ändern](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)
 
-----
 

Beim Klicken auf die **Standard** Layout an, die `TextView` Textzeichenfolge noch festgelegt ist, um `Portrait`. 



## <a name="handling-conflicts"></a>Lösen von Konflikten 

Wenn Sie sich entscheiden, so ändern Sie die Farbe des Texts in der **Standard** Layout in Grün, sehen Sie ein Warnsymbol auf den verknüpften Layout angezeigt werden. Durch Klicken auf das Layout wird das Layout, um den Konflikt anzuzeigen. Das Widget dem den Konflikt verursachenden wird mit einem roten Rahmen hervorgehoben und die folgende Meldung wird angezeigt: *kürzlich vorgenommene Änderungen Konflikte verursacht sein, in diesem alternativen Layout*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konflikt verursachenden Änderung](alternative-layout-views-images/vs/11-conflicting-change-sml.png "Konflikt verursachenden Änderung")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Widersprüchliche Änderung](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)
 
-----
 

Ein *Konflikt* wird angezeigt, auf der rechten Seite des Widgets, um den Konflikt zu erläutern: 

[![Warnung-Konflikt](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Der Konflikt-Feld enthält die Liste der Eigenschaften, die geändert wurden, und ihre Werte aufgeführt. Auf **Konflikt ignorieren** gilt die Eigenschaftenänderung nur für dieses Widget. Auf **übernehmen** gilt die Eigenschaftenänderung für dieses Widget auch hinsichtlich des Widgets Gegenstück in der verknüpften **Standard** Layout. Wenn alle eigenschaftsänderungen angewendet werden, wird der Konflikt automatisch verworfen. 


### <a name="view-group-conflicts"></a>Gruppe "Ansicht" Konflikte 

Eigenschaftenänderungen sind nicht die einzige Quelle von Konflikten. Beim Einfügen oder Entfernen von Widgets, können Konflikte erkannt werden. Z. B., wenn die **großen Flächen** Layout von aufgehoben wird der **Standard** Layout und die `TextView` in der **großen Flächen** Layout ist gezogen und abgelegt und über die `Button`, Designer kennzeichnet das verschobene Widget um den Konflikt anzugeben:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Anzeigen der Gruppe Konflikt](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "Konflikt Gruppe anzeigen")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Die Gruppe Konflikt anzeigen](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
-----
 

Besteht jedoch kein Marker auf der `Button`. Obwohl die Position des der `Button` hat sich geändert, die `Button` zeigt keine angewendeten Änderungen, die für spezifisch sind die **großen Flächen** Konfiguration. 

Wenn eine `CheckBox` hinzugefügt wird die **Standard** Layout, eine andere Konflikt wird generiert, und ein Warnsymbol wird angezeigt, über die **großen Flächen** Layout: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![CheckBox-Konflikt](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "Checkbox-Konflikt")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![CheckBox-Konflikt](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
-----
 

Klicken auf die **großen Flächen** Layout ergibt den Konflikt. Die folgende Meldung angezeigt: *kürzlich vorgenommene Änderungen Konflikte verursacht sein, in diesem alternativen Layout*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ALT Layout Konflikt](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt Layout-Konflikt")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![ALT-Layout-Konflikt](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
-----
 

Der Konflikt-Feld zeigt außerdem die folgende Meldung:

[![Konfliktmeldung](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Hinzufügen der `CheckBox` verursacht einen Konflikt, da die **großen Flächen** Layout wurde in der `LinearLayout` , die Sie enthält. In diesem Fall das Feld Konflikt zeigt jedoch das Widget, das soeben eingefügt wurde die **Standard** Layout (die `CheckBox`).

Wenn Sie auf **Konflikt ignorieren**, Designer löst den Konflikt zwischen, sodass Widgets angezeigt, die im Konflikt Feld per Drag & Drop in das Layout, in dem das Widget fehlt (in diesem Fall die **großen Flächen**  Layout): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Gruppe Konflikt aufgelöst](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "Gruppe Konflikt aufgelöst")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Gruppe Konflikt aufgelöst](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
-----
 

Wie im vorherigen Beispiel mit der `Button`, die `CheckBox` ein rotes Änderung Marker verfügt nicht über nur die `LinearLayout` wurde geändert, die in angewendet wurden die **großen Flächen** Layout.



### <a name="conflict-persistence"></a>Konflikt-Persistenz

Konflikte werden als XML-Kommentare, in der Layoutdatei beibehalten, wie hier gezeigt:

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Wenn ein Projekt geschlossen und erneut geöffnet wird, alle Konflikte weiterhin müssen es &ndash; Dies gilt auch für diejenigen, die wurden ignoriert.

