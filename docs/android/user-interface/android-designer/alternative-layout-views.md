---
title: Alternative Layoutansichten
description: In diesem Thema wird erläutert, wie Layouts mithilfe von Ressourcen Qualifizierern versioniert werden können. Es kann z. b. eine Version eines Layouts geben, die nur verwendet wird, wenn sich das Gerät im Querformat befindet, und eine Layoutversion, die nur für den Hochformat Modus gilt.
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 017d2d05c04dfaf2378ad1b0129eb4a75be2e777
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019490"
---
# <a name="alternative-layout-views"></a>Alternative Layoutansichten

_In diesem Thema wird erläutert, wie Layouts mithilfe von Ressourcen Qualifizierern versionslayouts durch Beispielsweise das Erstellen einer Version eines Layouts, die nur verwendet wird, wenn sich das Gerät im Querformat befindet, und eine Layoutversion, die nur für den Hochformat Modus gilt._

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="creating-alternative-layouts"></a>Erstellen alternativer Layouts

Wenn Sie auf das Symbol für die **Alternative Layoutansicht** (auf der linken Seite des **Geräts**) klicken, wird ein Vorschaubereich geöffnet, in dem die im Projekt verfügbaren Alternativen Layouts aufgeführt werden. Wenn keine alternativen Layouts vorhanden sind, wird die **Standard** Ansicht angezeigt: 

[![Alternativer layoutansichts Bereich](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "Alternativer layoutansichts Bereich")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

Wenn Sie auf das grüne Pluszeichen neben **neue Version**klicken, wird das Dialogfeld **layoutvariation erstellen** geöffnet, in dem Sie die Ressourcen Qualifizierer für diese layoutvariation auswählen können: 

[![Layoutvariation erstellen](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "Layoutvariation erstellen")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

Im folgenden Beispiel wird der Ressourcen Qualifizierer für die **Bildschirm Ausrichtung** auf **Landscape**festgelegt, und die **Bildschirmgröße** wird in **groß**geändert. Dadurch wird eine neue Layoutversion mit dem Namen **Large-Land**erstellt:

[![Groß Land Variation](alternative-layout-views-images/vs/03-large-land-sml.png "Groß Land Variation")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

Beachten Sie, dass im Vorschaubereich auf der linken Seite die Auswirkungen der Auswahl des Ressourcen Qualifizierers angezeigt werden. Wenn **Sie auf Hinzufügen** klicken, wird das alternative Layout erstellt und der Designer auf dieses Layout umgestellt. Der Kontext Bereich der **Alternativen Layoutansicht** gibt an, welches Layout in den Designer über einen kleinen rechten Zeiger geladen wird, wie im folgenden Screenshot zu sehen: 

[![Geladener layoutindikator](alternative-layout-views-images/vs/04-new-layout-sml.png "Geladener layoutindikator")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>Bearbeiten alternativer Layouts

Wenn Sie alternative Layouts erstellen, ist es häufig wünschenswert, eine einzelne Änderung vorzunehmen, die für alle verzweigten Versionen eines Layouts gilt. Beispielsweise können Sie den Schaltflächen Text in allen Layouts in Gelb ändern. Wenn Sie über eine große Anzahl von Layouts verfügen und eine einzelne Änderung an alle Versionen weitergeben müssen, kann die Wartung schnell zu umständlich und fehleranfällig werden.

Um die Wartung mehrerer Layoutversionen zu vereinfachen, stellt der Designer einen **Multi-Edit-** Modus bereit, der die Änderungen auf ein oder mehrere Layouts überträgt. Wenn mehr als ein Layout vorhanden ist, wird das **mehrfach Bearbeitungs** Symbol angezeigt: 

[![Symbol für mehrfach Bearbeitung](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "Symbol für mehrfach Bearbeitung")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

Wenn Sie auf das **mehrfach Bearbeitungs** Symbol klicken, werden Zeilen angezeigt, die darauf hinweisen, dass die Layouts verknüpft sind (wie unten dargestellt). Das heißt, wenn Sie eine Änderung an einem Layout vornehmen, wird diese Änderung an alle verknüpften Layouts weitergegeben. Sie können die Verknüpfung aller Layouts aufheben, indem Sie auf das im folgenden Screenshot angekreiste Symbol klicken: 

[![Verknüpfung aller Layouts aufheben](alternative-layout-views-images/vs/06-multi-linked-sml.png "Verknüpfung aller Layouts aufheben")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

Wenn Sie über mehr als zwei Layouts verfügen, können Sie die Bearbeitungs Schaltfläche auf der linken Seite der Layoutvorschau selektiv umschalten, um zu bestimmen, welche Layouts miteinander verknüpft sind. Wenn Sie z. b. eine einzelne Änderung vornehmen möchten, die an den ersten und den letzten drei Layouts weitergegeben wird, können Sie zuerst die Verknüpfung des mittleren Layouts aufheben, wie hier gezeigt: 

[![Verknüpfung des mittleren Layouts aufheben](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "Verknüpfung des mittleren Layouts aufheben")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)

In diesem Beispiel wird eine Änderung am **Standard** -oder **Long** -Layout an das andere Layout, aber nicht an das **große** Layout weitergegeben.

### <a name="multi-edit-example"></a>Beispiel für mehrere Bearbeitungsarbeiten 

Wenn Sie eine Änderung an einem Layout vornehmen, wird diese Änderung im Allgemeinen an alle anderen verknüpften Layouts weitergegeben. Wenn Sie z. b. ein neues `TextView` Widget zum **Standard** Layout hinzufügen und dessen Text Zeichenfolge in `Portrait` ändern, wird die gleiche Änderung an allen verknüpften Layouts vorgenommen. So sieht es im **Standard** Layout aus: 

[![TextView hinzufügen](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView hinzufügen")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)

Der `TextView` wird auch der **großen** Layoutansicht hinzugefügt, da er mit dem **Standard** Layout verknüpft ist: 

[![Querformat TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "Querformat TextView")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)

Was geschieht jedoch, wenn Sie eine Änderung vornehmen möchten, die nur ein Layout hat (d. h., Sie möchten nicht, dass die Änderung an eines der anderen Layouts weitergegeben wird)? Zu diesem Zweck müssen Sie die Verknüpfung des Layouts aufheben, das Sie ändern möchten, bevor Sie es ändern, wie im nächsten Abschnitt erläutert. 

### <a name="making-local-changes"></a>Vornehmen von lokalen Änderungen 

Angenommen, beide Layouts sollen über die hinzugefügte `TextView` verfügen, aber wir möchten auch die Text Zeichenfolge im **großen** Layout in `Landscape` anstatt `Portrait` ändern. Wenn wir diese Änderung an einem **großen Land** vornehmen, während beide Layouts verknüpft sind, wird die Änderung an das **Standard** Layout zurückgegeben. Daher müssen Sie zuerst die Verknüpfung der beiden Layouts aufheben, bevor die Änderung vorgenommen wird. Wenn wir den Text in " **Large-Land** " in "`Landscape`" ändern, markiert der Designer diese Änderung mit einem roten Rahmen, um anzugeben, dass die Änderung für das Layout des **großen Lands** lokal ist und *nicht* an das **Standard** Layout zurückgegeben wird: 

[![Lokale Änderung](alternative-layout-views-images/vs/10-local-change-sml.png "Lokale Änderung")](alternative-layout-views-images/vs/10-local-change.png#lightbox)

Wenn Sie auf das **Standard** Layout klicken, um es anzuzeigen, wird die `TextView` Text Zeichenfolge weiterhin auf `Portrait` festgelegt. 

## <a name="handling-conflicts"></a>Behandeln von Konflikten 

Wenn Sie festlegen, dass die Farbe des Texts im **Standard** Layout in Grün geändert werden soll, wird im verknüpften Layout ein Warnsymbol angezeigt. Durch Klicken auf dieses Layout wird das Layout geöffnet, um den Konflikt zu verdeutlichen. Das Widget, das den Konflikt verursacht hat, wird durch einen roten Rahmen hervorgehoben, und die folgende Meldung wird angezeigt: *Aktuelle Änderungen haben Konflikte in diesem alternativen Layout verursacht*. 

[![Widersprüchliche Änderung](alternative-layout-views-images/vs/11-conflicting-change-sml.png "Widersprüchliche Änderung")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)

Auf der rechten Seite des Widgets wird ein *Konfliktfeld* angezeigt, um den Konflikt zu erläutern: 

[Warnung zu![Konflikt](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Das Feld Konflikt zeigt die Liste der Eigenschaften an, die sich geändert haben, und listet deren Werte auf. Durch Klicken auf **Konflikt ignorieren** wird die Eigenschafts Änderung nur auf dieses Widget angewendet. Wenn Sie auf **anwenden** klicken, wird die Eigenschafts Änderung auf dieses Widget sowie auf das Pendant-Widget im verknüpften **Standard** Layout angewendet. Wenn alle Eigenschafts Änderungen angewendet werden, wird der Konflikt automatisch verworfen. 

### <a name="view-group-conflicts"></a>Gruppen Konflikte anzeigen 

Eigenschafts Änderungen sind nicht die einzige Quelle von Konflikten. Beim Einfügen oder Entfernen von Widgets können Konflikte erkannt werden. Wenn z. b. das Layout des **großen Lands** vom **Standard** Layout entfernt wird und die `TextView` im Layout des **großen Lands** über die `Button` gezogen und abgelegt wird, kennzeichnet der Designer das verschobene Widget, um den Konflikt anzuzeigen:

[![Gruppen Konflikt anzeigen](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "Gruppen Konflikt anzeigen")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)

Es gibt jedoch keinen Marker für die `Button`. Obwohl sich die Position des `Button` geändert hat, zeigt der `Button` keine angewendeten Änderungen an, die spezifisch für die **große** Konfiguration sind. 

Wenn ein `CheckBox` dem **Standard** Layout hinzugefügt wird, wird ein weiterer Konflikt generiert, und ein Warnsymbol wird über das Layout des **großen Lands** angezeigt: 

[![Kontrollkästchen-Konflikt](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "Kontrollkästchen-Konflikt")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)

Wenn Sie auf das **große** Layout klicken, wird der Konflikt angezeigt. Die folgende Meldung wird angezeigt: *Aktuelle Änderungen haben Konflikte in diesem alternativen Layout verursacht*: 

[![Alt-layoutkonflikt](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt-layoutkonflikt")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)

Außerdem wird im Konfliktfeld die folgende Meldung angezeigt:

[![Konflikt Meldung](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Durch das Hinzufügen des `CheckBox` wird ein Konflikt verursacht, da das Layout des **großen Lands** Änderungen in der `LinearLayout` enthält, in der es enthalten ist. In diesem Fall wird im Feld Konflikt jedoch das Widget angezeigt, das soeben in das **Standard** Layout (das `CheckBox`) eingefügt wurde.

Wenn Sie auf **Konflikt ignorieren**klicken, löst der Designer den Konflikt auf, sodass das im Feld Konflikt angezeigte Widget gezogen und in das Layout abgelegt werden kann, in dem das Widget fehlt (in diesem Fall das Layout für **groß Land** ): 

[![Aufgelöster Gruppen Konflikt](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "Aufgelöster Gruppen Konflikt")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)

Wie im vorherigen Beispiel mit dem `Button` gezeigt, hat der `CheckBox` keinen roten Änderungs Marker, da nur der `LinearLayout` Änderungen aufweist, die im Layout des **großen Lands** angewendet wurden.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="creating-alternative-layouts"></a>Erstellen alternativer Layouts

Wenn Sie auf das Symbol für die **Alternative Layoutansicht** (auf der linken Seite des **Geräts**) klicken, wird ein Vorschaubereich geöffnet, in dem die im Projekt verfügbaren Alternativen Layouts aufgeführt werden. Wenn keine alternativen Layouts vorhanden sind, wird die **Standard** Ansicht angezeigt: 

[Alternativer Bereich der Layoutansicht![](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

Wenn Sie auf das grüne Pluszeichen neben **neue Version**klicken, wird das Dialogfeld **layoutvariation erstellen** geöffnet, in dem Sie die Ressourcen Qualifizierer für diese layoutvariation auswählen können: 

[![layoutvariation erstellen](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

Im folgenden Beispiel wird der Ressourcen Qualifizierer für die **Bildschirm Ausrichtung** auf **Landscape**festgelegt, und die **Bildschirmgröße** wird in **groß**geändert. Dadurch wird eine neue Layoutversion mit dem Namen **Large-Land**erstellt:

[![groß Land Variation](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

Beachten Sie, dass im Vorschaubereich auf der linken Seite die Auswirkungen der Auswahl des Ressourcen Qualifizierers angezeigt werden. Wenn **Sie auf Hinzufügen** klicken, wird das alternative Layout erstellt und der Designer auf dieses Layout umgestellt. Der Kontext Bereich der **Alternativen Layoutansicht** gibt an, welches Layout in den Designer über einen kleinen rechten Zeiger geladen wird, wie im folgenden Screenshot zu sehen: 

[![geladenes layoutindikator](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>Bearbeiten alternativer Layouts

Wenn Sie alternative Layouts erstellen, ist es häufig wünschenswert, eine einzelne Änderung vorzunehmen, die für alle verzweigten Versionen eines Layouts gilt. Beispielsweise können Sie den Schaltflächen Text in allen Layouts in Gelb ändern. Wenn Sie über eine große Anzahl von Layouts verfügen und eine einzelne Änderung an alle Versionen weitergeben müssen, kann die Wartung schnell zu umständlich und fehleranfällig werden.

Um die Wartung mehrerer Layoutversionen zu vereinfachen, stellt der Designer einen **Multi-Edit-** Modus bereit, der die Änderungen auf ein oder mehrere Layouts überträgt. Wenn mehr als ein Layout vorhanden ist, wird das **mehrfach Bearbeitungs** Symbol angezeigt: 

[![Symbol für mehrfach Bearbeitung](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

Wenn Sie auf das **mehrfach Bearbeitungs** Symbol klicken, werden Zeilen angezeigt, die darauf hinweisen, dass die Layouts verknüpft sind (wie unten dargestellt). Das heißt, wenn Sie eine Änderung an einem Layout vornehmen, wird diese Änderung an alle verknüpften Layouts weitergegeben. Sie können die Verknüpfung aller Layouts aufheben, indem Sie auf das im folgenden Screenshot angekreiste Symbol klicken: 

[Aufheben der Verknüpfung aller Layouts![](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

Wenn Sie über mehr als zwei Layouts verfügen, können Sie die Bearbeitungs Schaltfläche auf der linken Seite der Layoutvorschau selektiv umschalten, um zu bestimmen, welche Layouts miteinander verknüpft sind. Wenn Sie z. b. eine einzelne Änderung vornehmen möchten, die an den ersten und den letzten drei Layouts weitergegeben wird, können Sie zuerst die Verknüpfung des mittleren Layouts aufheben, wie hier gezeigt: 

[Verknüpfung des mittleren Layouts![aufheben](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)

In diesem Beispiel wird eine Änderung am **Standard** -oder **Long** -Layout an das andere Layout, aber nicht an das **große** Layout weitergegeben. 

### <a name="multi-edit-example"></a>Beispiel für mehrere Bearbeitungsarbeiten 

Wenn Sie eine Änderung an einem Layout vornehmen, wird diese Änderung im Allgemeinen an alle anderen verknüpften Layouts weitergegeben. Wenn Sie z. b. ein neues `TextView` Widget zum **Standard** Layout hinzufügen und dessen Text Zeichenfolge in `Portrait` ändern, wird die gleiche Änderung an allen verknüpften Layouts vorgenommen. So sieht es im **Standard** Layout aus: 

[![Textansicht hinzufügen](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)

Der `TextView` wird auch der **großen** Layoutansicht hinzugefügt, da er mit dem **Standard** Layout verknüpft ist: 

[TextView für![Querformat](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)

Was geschieht jedoch, wenn Sie eine Änderung vornehmen möchten, die nur ein Layout hat (d. h., Sie möchten nicht, dass die Änderung an eines der anderen Layouts weitergegeben wird)? Zu diesem Zweck müssen Sie die Verknüpfung des Layouts aufheben, das Sie ändern möchten, bevor Sie es ändern, wie im nächsten Abschnitt erläutert. 

### <a name="making-local-changes"></a>Vornehmen von lokalen Änderungen 

Angenommen, beide Layouts sollen über die hinzugefügte `TextView` verfügen, aber wir möchten auch die Text Zeichenfolge im **großen** Layout in `Landscape` anstatt `Portrait` ändern. Wenn wir diese Änderung an einem **großen Land** vornehmen, während beide Layouts verknüpft sind, wird die Änderung an das **Standard** Layout zurückgegeben. Daher müssen Sie zuerst die Verknüpfung der beiden Layouts aufheben, bevor die Änderung vorgenommen wird. Wenn wir den Text in " **Large-Land** " in "`Landscape`" ändern, markiert der Designer diese Änderung mit einem roten Rahmen, um anzugeben, dass die Änderung für das Layout des **großen Lands** lokal ist und *nicht* an das **Standard** Layout zurückgegeben wird: 

[lokale Änderung![](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)

Wenn Sie auf das **Standard** Layout klicken, um es anzuzeigen, wird die `TextView` Text Zeichenfolge weiterhin auf `Portrait` festgelegt. 

## <a name="handling-conflicts"></a>Behandeln von Konflikten 

Wenn Sie festlegen, dass die Farbe des Texts im **Standard** Layout in Grün geändert werden soll, wird im verknüpften Layout ein Warnsymbol angezeigt. Durch Klicken auf dieses Layout wird das Layout geöffnet, um den Konflikt zu verdeutlichen. Das Widget, das den Konflikt verursacht hat, wird durch einen roten Rahmen hervorgehoben, und die folgende Meldung wird angezeigt: *Aktuelle Änderungen haben Konflikte in diesem alternativen Layout verursacht*. 

[![widersprüchliche Änderung](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)

Auf der rechten Seite des Widgets wird ein *Konfliktfeld* angezeigt, um den Konflikt zu erläutern: 

[Warnung zu![Konflikt](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Das Feld Konflikt zeigt die Liste der Eigenschaften an, die sich geändert haben, und listet deren Werte auf. Durch Klicken auf **Konflikt ignorieren** wird die Eigenschafts Änderung nur auf dieses Widget angewendet. Wenn Sie auf **anwenden** klicken, wird die Eigenschafts Änderung auf dieses Widget sowie auf das Pendant-Widget im verknüpften **Standard** Layout angewendet. Wenn alle Eigenschafts Änderungen angewendet werden, wird der Konflikt automatisch verworfen. 

### <a name="view-group-conflicts"></a>Gruppen Konflikte anzeigen 

Eigenschafts Änderungen sind nicht die einzige Quelle von Konflikten. Beim Einfügen oder Entfernen von Widgets können Konflikte erkannt werden. Wenn z. b. das Layout des **großen Lands** vom **Standard** Layout entfernt wird und die `TextView` im Layout des **großen Lands** über die `Button` gezogen und abgelegt wird, kennzeichnet der Designer das verschobene Widget, um den Konflikt anzuzeigen:

[Konflikt bei![Ansichts Gruppe](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)

Es gibt jedoch keinen Marker für die `Button`. Obwohl sich die Position des `Button` geändert hat, zeigt der `Button` keine angewendeten Änderungen an, die spezifisch für die **große** Konfiguration sind. 

Wenn ein `CheckBox` dem **Standard** Layout hinzugefügt wird, wird ein weiterer Konflikt generiert, und ein Warnsymbol wird über das Layout des **großen Lands** angezeigt: 

[![CheckBox-Konflikt](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)

Wenn Sie auf das **große** Layout klicken, wird der Konflikt angezeigt. Die folgende Meldung wird angezeigt: die *letzten Änderungen haben Konflikte in diesem alternativen Layout verursacht*. 

[![alt-layoutkonflikt](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)

Außerdem wird im Konfliktfeld die folgende Meldung angezeigt:

[![Konflikt Meldung](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Durch das Hinzufügen des `CheckBox` wird ein Konflikt verursacht, da das Layout des **großen Lands** Änderungen in der `LinearLayout` enthält, in der es enthalten ist. In diesem Fall wird im Feld Konflikt jedoch das Widget angezeigt, das soeben in das **Standard** Layout (das `CheckBox`) eingefügt wurde.

Wenn Sie auf **Konflikt ignorieren**klicken, löst der Designer den Konflikt auf, sodass das im Feld Konflikt angezeigte Widget gezogen und in das Layout abgelegt werden kann, in dem das Widget fehlt (in diesem Fall das Layout für **groß Land** ): 

[![aufgelöster Gruppen Konflikt](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)

Wie im vorherigen Beispiel mit dem `Button` gezeigt, hat der `CheckBox` keinen roten Änderungs Marker, da nur der `LinearLayout` Änderungen aufweist, die im Layout des **großen Lands** angewendet wurden.

-----

### <a name="conflict-persistence"></a>Konflikt Persistenz

Konflikte werden in der Layoutdatei als XML-Kommentare persistent gespeichert, wie hier gezeigt:

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Wenn ein Projekt geschlossen und erneut geöffnet wird, werden daher alle Konflikte immer noch &ndash; auch die ignoriert, die ignoriert wurden.
