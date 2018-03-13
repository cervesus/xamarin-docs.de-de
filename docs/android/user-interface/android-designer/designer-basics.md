---
title: Designer-Grundlagen
description: "In diesem Thema führt Designer-Funktionen, wird erläutert, wie die-Designer zu starten, wird beschrieben, die Entwurfsoberfläche und erläutert, wie der Bereich \"Eigenschaften\" Widgeteigenschaften bearbeiten."
ms.topic: article
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d9342dc3a8d324f03cd31e1d03600449bfcf23f1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="designer-basics"></a>Designer-Grundlagen

_In diesem Thema führt Designer-Funktionen, wird erläutert, wie die-Designer zu starten, wird beschrieben, die Entwurfsoberfläche und erläutert, wie der Bereich "Eigenschaften" Widgeteigenschaften bearbeiten._


## <a name="launching-the-designer"></a>Den Designer wird gestartet.

Der Designer wird automatisch gestartet, wenn ein Layout erstellt wird, oder durch Doppelklicken auf eine vorhandene .axml Datei gestartet werden. Doppelklicken Sie z. B. **Main.axml** in der **Ressourcen > Layout** Ordner Laden des Designers, wie unten dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![In Visual Studio-Designer-Fenster](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Designer-Fenster in Visual Studio für Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ebenso können Sie ein neues Layout hinzufügen, indem Sie mit der rechten Maustaste die **Layout** Ordner in der **Projektmappen-Explorer** auswählen und **hinzufügen > Neues Element… > Android Layout**:

[![Dialogfeld Neues Element hinzufügen](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Ebenso können Sie ein neues Layout hinzufügen, indem Sie mit der rechten Maustaste die **Layout** Ordner in der **Lösung Pad** auswählen und **hinzufügen > neue Datei > Android > Layout**:

[![Dialogfeld "neue Datei" hinzufügen](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

-----

Dies erstellt eine neue .axml-Datei und lädt es auf die Entwurfsoberfläche.



## <a name="designer-features"></a>Designer-Funktionen

Der Designer besteht aus mehreren Abschnitten, die die verschiedenen Funktionen zu unterstützen wie im folgenden Screenshot gezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagramm der-Designerbereichen](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Diagramm der-Designerbereichen](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

-----

Wenn Sie ein Layout im Designer bearbeitet haben, verwenden Sie die folgenden Funktionen zum Erstellen und die Form des Entwurfs:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Entwurfsoberfläche** &ndash; erleichtert die grafischen Erstellung der Benutzeroberfläche ermöglicht eine bearbeitbare Darstellung wie das Layout auf dem Gerät angezeigt wird.

-   **Symbolleiste** &ndash; zeigt eine Liste von Selektoren: **Gerät**, **Version**, **Design**, Layout Konfigurations- und Aktionsleiste. Die Symbolleiste enthält auch Symbole für das Starten der Design-Editors und zum Aktivieren der Material-Entwurfsbereich.

-   **Toolbox** &ndash; enthält eine Liste von Widgets und Layouts, mit denen Sie können Drag & drop auf die Entwurfsoberfläche.

-   **Fenster "Eigenschaften"** &ndash; führt die Eigenschaften des ausgewählten Widgets zum Anzeigen und bearbeiten.

-   **Dokumentgliederung** &ndash; zeigt die Struktur des Widgets, mit denen das Layout zu erstellen. Sie können ein Element in der Struktur nach dazu führen, dass es im Designer ausgewählt werden klicken. Durch Klicken auf ein Element in der Struktur lädt außerdem die Eigenschaften des Elements in das Eigenschaftenfenster.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

-   **Entwurfsoberfläche** &ndash; erleichtert die grafischen Erstellung der Benutzeroberfläche ermöglicht eine bearbeitbare Darstellung wie das Layout auf dem Gerät angezeigt wird.

-   **Symbolleiste** &ndash; zeigt eine Liste von Selektoren: **Gerät**, **Version**, **Design**, Layout Konfigurations- und Aktionsleiste. Die Symbolleiste enthält auch Symbole für das Starten der Design-Editors und zum Aktivieren der Material-Entwurfsbereich.

-   **Toolbox** &ndash; enthält eine Liste von Widgets und Layouts, mit denen Sie können Drag & drop auf die Entwurfsoberfläche.

-   **Eigenschaft Pad** &ndash; führt die Eigenschaften des ausgewählten Widgets zum Anzeigen und bearbeiten.

-   **Dokumentgliederung** &ndash; zeigt die Struktur des Widgets, mit denen das Layout zu erstellen. Sie können ein Element in der Struktur nach dazu führen, dass es im Designer ausgewählt werden klicken. Durch Klicken auf ein Element in der Struktur lädt außerdem die Eigenschaften des Elements in der Eigenschaft aufgefüllt.

-----



## <a name="toolbar"></a>Symbolleiste

Die Symbolleiste (oberhalb der Entwurfsoberfläche positioniert) stellt Konfiguration Selektoren und Tool-Menüs:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagramm der Designer-Symbolleiste](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Diagramm der Designer-Symbolleiste](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

-----


Die Symbolleiste bietet Zugriff auf die folgenden Funktionen:

-   **Alternative Layouts Selektor** &ndash; können Sie von anderen Layout Versionen auswählen.

-   **Gerät Selektor** &ndash; definiert einen Satz von Qualifizierer, die ein bestimmtes Gerät, z. B. Bildschirmgröße und Auflösung Tastatur Verfügbarkeit zugeordnet. Sie können auch hinzufügen und löschen Sie neue Geräte.

-   **Android-Version-Selektor** &ndash; der Android-Version, die das Layout abzielt. Der Designer wird das Layout entsprechend der ausgewählten Android-Version gerendert.

-   **Design** &ndash; wählt das UI-Design für das Layout.

-   **Konfiguration Selektor** &ndash; wählt Sie die Gerätekonfiguration, z. B. *Hochformat* oder *Querformat*.

-   **Qualifizierer Ressourcenoptionen** &ndash; öffnet ein Dialogfeld, in dem Dropdown-Menüs für die Auswahl dargestellt *Sprache*, *Benutzeroberflächen-Modus*, *Nacht Modus*, und *Round Bildschirm* Optionen.

-   **Leiste von Aktionseinstellungen** &ndash; konfiguriert die Aktionsleiste-Einstellungen für das Layout.

-   **Design-Editors** &ndash; öffnet die *Design-Editors*, wodurch es möglich, damit Sie diese Elemente des ausgewählten Designs anpassen.

-   **Material Entwurfsbereich** &ndash; aktiviert oder deaktiviert die *Material Entwurfsbereich*. Das Dropdown-Menü-Element, das neben dem Entwurfsbereich Material öffnet ein Dialogfeld, das Sie im Raster anpassen kann.

Jede dieser Funktionen ist in den folgenden Themen ausführlicher erläutert:

[Ressourcenqualifizierer und zu den Visualisierungsoptionen](~/android/user-interface/android-designer/resource-qualifiers.md) enthält ausführliche Informationen zu den **Gerät Selektor**, **Android Version Selektor**, **Design Selektor**, **Konfiguration Selektor**, **Qualifikationen Ressourcenoptionen**, und **Aktionsleiste Einstellungen**.

[Alternative Layoutansichten](~/android/user-interface/android-designer/alternative-layout-views.md) erläutert, wie die **Alternative Layouts Selektor**.

[Material Entwurfsfunktionen](~/android/user-interface/android-designer/material-design-features.md) bietet eine umfassende Übersicht über die **Design-Editors** und **Material Entwurfsbereich**.



## <a name="design-surface"></a>Entwurfsoberfläche

Der Designer ermöglicht es Ihnen, Drag & drop von Widgets aus der Toolbox auf die Entwurfsoberfläche. Wenn Widgets im Designer interagieren mit (neue Widgets hinzufügen oder vorhandene Neupositionieren), werden vertikale und horizontale Linien angezeigt, um die verfügbaren einfügepunkte zu kennzeichnen. Im folgenden Beispiel ein neues `Button` Widget auf die Entwurfsoberfläche gezogen wird:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Beispiel Einfügen von Zeilen auf der Entwurfsoberfläche](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Beispiel Einfügen von Zeilen auf der Entwurfsoberfläche](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

-----

Darüber hinaus können Widgets kopiert werden: können Sie kopieren und einfügen, um das Kopieren von Widgets, oder Sie können Drag & drop eine vorhandene Widget beim Drücken der <kbd>STRG</kbd> Schlüssel.


### <a name="context-menu-commands"></a>Kontextmenübefehle

Ein Kontextmenü ist sowohl in der Entwurfsoberfläche als auch in der Dokumentgliederung verfügbar. Dieses Menü zeigt Befehle, die für das ausgewählte Widget und dessen Container, erleichtert Ihnen auszuführenden Vorgänge in Containern (die nicht immer einfach ist, wählen Sie auf der Entwurfsoberfläche sind) verfügbar sind. Hier ist ein Beispiel für ein Kontextmenü:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Beispiel im Kontextmenü der Entwurfsoberfläche mit der rechten Maustaste](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

In diesem Beispiel wird mit der rechten Maustaste ein `TextView` öffnet ein Kontextmenü, das mehrere Optionen bereitstellt:

-   **LinearLayout** &ndash; öffnet ein Untermenü für die Bearbeitung der `LinearLayout` übergeordnet der `TextView`.

-   **Löschen Sie**, **Kopie**, und **Ausschneiden** &ndash; Vorgänge, die für die webdatenverbindung gelten `TextView`.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Beispiel im Kontextmenü der Entwurfsoberfläche mit der rechten Maustaste](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

In diesem Beispiel wird mit der rechten Maustaste ein `TextView` öffnet ein Kontextmenü, das mehrere Optionen bereitstellt:

-   **RelativeLayout** &ndash; öffnet ein Untermenü für die Bearbeitung der `RelativeLayout` übergeordnet der `TextView`.

-   **LinearLayout** &ndash; öffnet ein Untermenü für die Bearbeitung der `LinearLayout` übergeordnet der `TextView`.

-   **Löschen Sie**, **Kopie**, und **Ausschneiden** &ndash; Vorgänge, die für die webdatenverbindung gelten `TextView`.

-----

In diesem Beispiel wird mit der rechten Maustaste ein `TextView` öffnet ein Kontextmenü, das mehrere Optionen bereitstellt:

-   **LinearLayout** &ndash; öffnet ein Untermenü für die Bearbeitung der `LinearLayout` übergeordnet der `TextView`.

-   **Löschen Sie**, **Kopie**, und **Ausschneiden** &ndash; Vorgänge, die für die webdatenverbindung gelten `TextView`.



### <a name="zoom-controls"></a>Zoom-Steuerelementen

Die Entwurfsoberfläche unterstützt Zoomen über mehrere Steuerelemente, wie dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagramm der Entwurfsoberfläche Zoom-Steuerelemente](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Diagramm der Entwurfsoberfläche Zoom-Steuerelemente](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

-----

Diese Steuerelemente erleichtern es bestimmte Bereiche der Benutzeroberfläche im Designer finden Sie unter:

-   **Markieren Sie Container** &ndash; hervorgehoben Container auf der Entwurfsoberfläche angezeigt, damit sie leichter zu finden, die beim Vergrößern und verkleinern sind.

-   **Normalgröße** &ndash; rendert das Layout Pixel-für-Pixel, damit Sie sehen, wie das Layout der Auflösung des ausgewählten Geräts betrachten.

-   **An Fenster anpassen** &ndash; legt die Zoomstufe, damit das gesamte Layout auf der Entwurfsoberfläche angezeigt wird.

-   **Vergrößern** &ndash; vergrößert inkrementell mit jedem klicken, das Layout zu vergrößern.

-   **Verkleinern** &ndash; inkrementell verkleinert, bei jedem Mausklick, machen das Layout auf der Entwurfsoberfläche kleiner angezeigt.

Beachten Sie, dass das ausgewählte Einstellung zoom wirkt sich nicht auf die Benutzeroberfläche der Anwendung zur Laufzeit aus.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="property-pad"></a>Eigenschaft mit Leerstellen auffüllen

Der Designer unterstützt die Bearbeitung von Widgeteigenschaften über den **Eigenschaft Pad**. Die Eigenschaften aufgeführt, die auf den Pad-Eigenschaft ändern, je nachdem, den welche Widget in der Entwurfsoberfläche ausgewählt ist. Wenn die `Button` im vorherigen Beispiel aktiviert ist, die Eigenschaften für diesen `Button` Widget werden angezeigt:

[![Screenshot von der Eigenschaft mit Leerstellen auffüllen](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>Eigenschaftenfenster

Der Designer unterstützt die Bearbeitung von Widgeteigenschaften über den **Fenster "Eigenschaften"**. Die Eigenschaften aufgeführt, die auf den Eigenschaften-Fenster ändern, je nachdem, den welche Widget auf der Entwurfsoberfläche ausgewählt ist.
Wenn die `Button` im vorherigen Beispiel aktiviert ist, die Eigenschaften für diesen `Button` Widget werden angezeigt:

![Screenshot des Eigenschaftenfensters](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="property-pad-sections"></a>Auffüllzeichen Eigenschaftenabschnitte

Das Auffüllzeichen-Eigenschaft wird in mehrere Abschnitte unterteilt, die ähnliche Eigenschaften gruppieren &ndash; Dies erleichtert es, suchen Sie die gewünschten Eigenschaften:

-   **Widget** &ndash; Haupteigenschaften des Widgets, wie z. B. `id`, `visibility`, `text`usw. Eigenschaften für die Verwaltung des Widgets Inhalte normalerweise hier platziert werden.

-   **Stil** &ndash; Eigenschaften, die die visuelle Darstellung des Widgets, wie z. B. ändern `font`, `text color`, `background`usw.

-   **Layout** &ndash; Eigenschaften, die die Position und Größe des Widgets festlegen.

-   **Führen Sie einen Bildlauf** &ndash; Scrolleigenschaften.

-   **Verhalten** &ndash; Flags, die festlegen, wie das Widget verhält.

-----



### <a name="default-values"></a>Standardwerte

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Werden die Eigenschaften von den meisten Widgets leer ist, in der **Eigenschaften** Fenster da ihre Werte aus dem ausgewählten Android Design erben.
Die **Eigenschaften** Fenster zeigt nur die Werte, die für das ausgewählte Gadget explizit festgelegt werden; er zeigt nicht die Werte, die aus dem Design geerbt werden.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Werden die Eigenschaften von den meisten Widgets leer ist, in der **Eigenschaft Pad** da ihre Werte aus dem ausgewählten Android Design erben. Die **Eigenschaft Pad** zeigt nur die Werte, die für das ausgewählte Gadget explizit festgelegt werden; er zeigt nicht die Werte, die aus dem Design geerbt werden.

-----


### <a name="referencing-resources"></a>Verweisen auf Ressourcen

Einige Eigenschaften können auf Ressourcen, die in andere Dateien als das Layout definiert sind verweisen **.axml** Datei. Die meisten gängigen Fälle dieses Typs sind `string` und `drawable` Ressourcen. Allerdings Verweise können auch verwendet werden für andere Ressourcen, z. B. `Boolean` Werte und Dimensionen.
Wenn eine Eigenschaft unterstützt Ressourcenverweise, ein Symbol "Durchsuchen" (ein Auslassungszeichen &hellip;) neben dem Eintrag "Text" für die Eigenschaft angezeigt wird.
Diese Schaltfläche wird beim Klicken auf die ein Ressourcenauswahl geöffnet.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Der folgende Screenshot zeigt die verfügbaren Ressourcen, wenn auf die Auslassungspunkte rechts neben dem Textfeld für eine `Button` Widget in der **Eigenschaften** Fenster:

[![Beispiel-Ressourcen-Screenshot mit zwei aufgeführten Ressourcen](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Der folgende Screenshot zeigt die verfügbaren Ressourcen, wenn auf die Auslassungspunkte rechts neben dem Textfeld für eine `Button` Widget in der **Eigenschaft Pad**:

[![Beispiel-Ressourcen-Screenshot mit zwei aufgeführten Ressourcen](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

-----

Das folgende Beispiel veranschaulicht die Ressourcenauswahl für die `Src` Eigenschaft ein `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ressourcenauswahl Symbolressource für eine ImageView auflisten](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Ressourcenauswahl Symbolressource für eine ImageView auflisten](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

-----



### <a name="boolean-property-references"></a>Boolesche Eigenschaftenverweise

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*Boolesche* Eigenschaften werden normalerweise aus einem Dropdownmenü im Eigenschaftenfenster ausgewählt. Sie können auswählen, eine `true` oder `false` Wert, oder Sie können ein Eigenschaftsverweis auswählen, indem Sie auf **Ressource auswählen...** . Sie können auch direkt einen Wert eingeben, z. B. `true` oder `false`.

![Beispiel für boolesche Eigenschaften festlegen](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

*Boolesche* Eigenschaften werden normalerweise als ein Kontrollkästchen in der Eigenschaft Pad angezeigt. Wenn eine `Boolean` Ressourcenverweise die Eigenschaft unterstützt wird, eine kleine Kontrollkästchen neben der Eigenschaft erscheint. Ein aktiviertes Kontrollkästchen bedeutet, dass `true` und ein leeres Feld bedeutet `false`. Sie können auch direkt einen Wert eingeben, z. B. `true` oder `false`. Bewegen des Mauszeigers über die Eingabe wird eine kleine Text-Symbol "Feld". Wenn Sie den Wert manuell eingeben möchten, können Sie darauf klicken.

[![Beispiel für boolesche Eigenschaften festlegen](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>Gruppeneigenschaften

Einige Widgets mehrwertige Eigenschaften besitzen, die zusammen gruppiert sind (z. B. `Padding`, z. B.). Diese Eigenschaftswerte sind aufgeführt, der **Eigenschaft Pad** in einer einzigen, erweiterbaren Zeile. Einige dieser Eigenschaften können bearbeitet werden direkt in der gruppierten Zeile, z. B. die `Padding` Eigenschaft fest, wie im folgenden:

[![Beispieleinstellungen für die Padding-Eigenschaft](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

-----


## <a name="editing-properties-inline"></a>Bearbeiten von Eigenschaften Inline

Die Android-Designer unterstützt die direkte Bearbeitung der bestimmte Eigenschaften auf der Entwurfsoberfläche angezeigt (Sie müssen also nicht für diese Eigenschaften in der Eigenschaftenliste suchen). Eigenschaften, die direkt bearbeitet werden können gehören Text, Rand und Größe.

### <a name="text"></a>Text

Die Eigenschaften von Text von einigen Widgets (z. B. `Button` und `TextView`), können direkt auf der Entwurfsoberfläche bearbeitet werden. Durch Doppelklicken auf ein Widget wird es in den Bearbeitungsmodus wechseln, platzieren Sie wie unten dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Text-Ressource für die Zeichenfolge Hello](designer-basics-images/vs/12-text-resource.png "Text-Ressource")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Text-Ressource für die Zeichenfolge hello](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

-----

Können Sie einen neuen Textwert eingeben, oder Sie können eine neue Ressourcenzeichenfolge eingeben. Im folgenden Beispiel die `@string/hello` Ressource wird mit dem Text ersetzt wird `CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![UMSCHALT + EINGABETASTE, um automatisch Text mit einer neuen Ressource verknüpfen](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![UMSCHALT + EINGABETASTE, um automatisch Text mit einer neuen Ressource verknüpfen](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

-----

Diese Änderung wird gespeichert, in des Widgets `text` Eigenschafts-ändert nicht den zugewiesenen Wert der `@string/hello` Ressource.
Wenn Sie in einer neuen Textzeichenfolge vergeben, drücken Sie <kbd>UMSCHALT</kbd> +
<kbd>EINGABETASTE</kbd> des eingegebenen Texts automatisch auf eine neue Ressource zu verknüpfen.


### <a name="margin"></a>Margin

Wenn Sie ein Widget auswählen, zeigt der Designer spaltenhandles, mit die Sie die Größe oder den Rand des Widgets interaktiv ändern können. Das Widget auf, während Sie diese Option ausgewählt ist, schaltet zwischen Rand Textbearbeitungsmodus und Größe Textbearbeitungsmodus.

Wenn Sie ein Widget zum ersten Mal klicken, werden die Ziehpunkte angezeigt. Wenn Sie die Maus auf einen der Ziehpunkte verschieben, wird der Designer zeigt die Eigenschaft, die das Handle geändert wird (wie unten dargestellt, für die `layout_marginLeft` Eigenschaft):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Screenshot der Rand handles im Designer](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Screenshot der Rand handles im Designer](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

-----

Wenn Sie bereits ein Rand festgelegt wurde, werden gepunktete Linien angezeigt, der angibt, des Speicherplatzes, der der Rand:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Beispiel für die gepunkteten Linien kennzeichnen Platz um eine Schaltfläche](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Beispiel für die gepunkteten Linien kennzeichnen Platz um eine Schaltfläche](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

-----



### <a name="size"></a>Größe

Wie bereits erwähnt, können Sie wechseln in den Bearbeitungsmodus mit der Größe für durch Klicken auf ein Widget, während er bereits ausgewählt ist. Klicken Sie auf die dreieckige Handle zum Festlegen der Größe für die angegebene Dimension `wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Wrap-Inhalt und zum Ändern der Größe-handles](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Wrap-Inhalt und zum Ändern der Größe-handles](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

-----

Klicken auf die **umschließen Inhalt** Handle verkleinert das Widget in dieser Dimension, dass nicht größer als notwendig, umgeben von Inhalt umschlossen werden. In diesem Beispiel wird Sie als Schaltflächentext horizontal in der nächste Screenshot gezeigten verkleinert.

Wenn die Größenwert festgelegt wird, um **Content umschließen**, zeigt der Designer ein dreieckiges Handle in die entgegengesetzte Richtung zum Ändern der Größe, zeigen `match_parent`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Das Handle des übergeordneten Übereinstimmung](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Das Handle des übergeordneten Übereinstimmung](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

-----

Klicken auf die **Übereinstimmung übergeordneten** Handle stellt die Größe in dieser Dimension, damit er als das übergeordnete Widget identisch ist.

Außerdem Sie können ziehen Sie den zirkuläre Ziehpunkt (wie in den oben genannten Screenshots gezeigt) zum Ändern der Größe des Widgets auf einen beliebigen `dp` Wert. Wenn Sie dies tun beide **Content umschließen** und **Übereinstimmung übergeordneten** Handles für diese Dimension angezeigt werden:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Zirkuläre Ziehpunkte](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Zirkuläre Ziehpunkte](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

-----

Bearbeiten von nicht von allen Containern zulassen der `Size` eines Widgets. Beachten Sie, dass z. B. im Screenshot unten mit der `LinearLayout` ausgewählt haben, werden nicht die Ziehpunkte angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Keine Ziehpunkte](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Keine Ziehpunkte](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

-----



## <a name="document-outline"></a>Dokumentgliederung

Die **Dokumentgliederung** zeigt das Widget-Hierarchie des Layouts.
Im folgenden Beispiel, das mit `LinearLayout` Widget ausgewählt ist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dokumentgliederung](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Dokumentgliederung](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

-----

Die Gliederung des ausgewählten Widgets (in diesem Fall eine `LinearLayout`) wird auf der Entwurfsoberfläche auch hervorgehoben. Das ausgewählte Widget im Dokumentgliederungsfenster bleibt synchron mit dem Gegenstück auf der Entwurfsoberfläche angezeigt. Dies ist nützlich zum Auswählen von Gruppen anzeigen, die nicht immer einfach ist, wählen Sie auf der Entwurfsoberfläche angezeigt.

Die Dokumentgliederung unterstützt das Kopieren und einfügen, oder Sie können mithilfe von Drag und drop. Drag & Drop wird unterstützt, von der Dokumentgliederung auf die Entwurfsoberfläche sowie von der Entwurfsoberfläche auf die Dokumentgliederung. Rechtsklick auf ein Element im Dokumentgliederungsfenster zeigt außerdem das Kontextmenü für das Element (das gleiche Kontextmenü, das angezeigt wird, wenn Sie diese gleichen Widget auf der Entwurfsoberfläche mit der rechten Maustaste).
