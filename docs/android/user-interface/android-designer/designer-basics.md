---
title: Xamarin.Android-Designer-Grundlagen
description: In diesem Thema werden Xamarin.Android-Designer-Funktionen erläutert, wird erläutert, wie Sie den Designer zu starten, wird beschrieben, die Entwurfsoberfläche und erläutert, wie Sie den Bereich "Eigenschaften" zu verwenden, um das Widgeteigenschaften zu bearbeiten.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/05/2018
ms.openlocfilehash: fe909d72f3c6d6733318b5dcbd1858a1a9e28b37
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108196"
---
# <a name="xamarinandroid-designer-basics"></a>Xamarin.Android-Designer-Grundlagen

_In diesem Thema werden Xamarin.Android-Designer-Funktionen erläutert, wird erläutert, wie Sie den Designer zu starten, wird beschrieben, die Entwurfsoberfläche und erläutert, wie Sie den Bereich "Eigenschaften" zu verwenden, um das Widgeteigenschaften zu bearbeiten._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="launching-the-designer"></a>Starten den Designer

Der Designer wird automatisch gestartet, wenn ein Layout erstellt wird, oder durch Doppelklicken auf eine vorhandene Layoutdatei gestartet werden. Doppelklicken Sie z. B. **activity_main.axml** in die **Ressourcen > Layout** Ordner wird den Designer geladen, wie im folgenden Screenshot zu sehen:

[![In Visual Studio-Designer-Fenster](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

Ebenso können Sie ein neues Layout hinzufügen, indem Sie mit der rechten Maustaste die **Layout** Ordner in der **Projektmappen-Explorer** und **hinzufügen > Neues Element… > Android-Layout**:

[![Dialogfeld Neues Element hinzufügen](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

Dies erstellt ein neues **axml** Layout-Datei und lädt sie in den Designer.

## <a name="designer-features"></a>Die Funktionen eines Designers

Der Designer besteht aus mehrere Abschnitte, die die verschiedenen Funktionen unterstützen, wie im folgenden Screenshot gezeigt:

[![Diagramm der-Designerbereichen](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

Wenn Sie ein Layout im Designer bearbeiten, verwenden Sie die folgenden Funktionen zum Erstellen und gestalten Ihren Entwurf:

-   **Entwurfsoberfläche** &ndash; erleichtert der grafischen Erstellung von der Benutzeroberfläche ermöglicht eine editierbare Darstellung wie das Layout auf dem Gerät angezeigt wird. Die **Entwurfsoberfläche** wird angezeigt, in der **Entwurfsbereich** (mit der **-Designer-Symbolleiste** positioniert darüber).

-   **Im Bereich Quelle** &ndash; bietet einen Überblick über die zugrunde liegenden XML-Quelle, die das Design präsentiert entspricht, der **Entwurfsoberfläche**.

-   **Symbolleiste des Designers** &ndash; zeigt eine Liste der Selektoren: **Gerät**, **Version**, **Design**, Layout, Konfiguration und Einstellungen für Aktionsleiste. Die **-Designer-Symbolleiste** enthält auch Symbole für den Start der Design-Editor sowie für die Aktivierung der Material-Entwurfsbereich.

-   **Toolbox** &ndash; enthält eine Liste von Widgets und Layouts, mit denen Sie ziehen können, und legen Sie auf die **Entwurfsoberfläche**.

-   **Fenster "Eigenschaften"** &ndash; enthält die Eigenschaften des ausgewählten Widgets zum Anzeigen und bearbeiten.

-   **Dokumentgliederung** &ndash; zeigt die Struktur von Widgets, die das Layout bilden. Klicken Sie auf ein Element in der Struktur, um die Auswahl dazu führen, dass die **Entwurfsoberfläche**. Darüber hinaus das Klicken auf ein Element in der Struktur lädt die Eigenschaften des Elements in der **Eigenschaften** Fenster.

## <a name="design-surface"></a>Entwurfsoberfläche

Der Designer ermöglicht es Ihnen, Drag & drop von Widgets aus der Toolbox auf die **Entwurfsoberfläche**. Bei Widgets im Designer interagieren mit (neue Widgets hinzufügen oder Neupositionieren von vorhandenen), werden vertikale und horizontale Linien angezeigt, um den verfügbaren Einfügemarken zu markieren. Im folgenden Beispiel eine neue `Button` Widget wird Daten gezogen werden, werden die **Entwurfsoberfläche**:

[![Beispiel für Einfügen von Zeilen auf der Entwurfsoberfläche](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

Widgets können darüber hinaus kopiert werden: Sie können kopieren und einfügen, um ein Widget, oder Sie kopieren kann Drag & drop eine vorhandene Widget beim Drücken der <kbd>STRG</kbd> Schlüssel.

### <a name="designer-toolbar"></a>Symbolleiste des Designers

Die **-Designer-Symbolleiste** (oben positioniert die **Entwurfsoberfläche**) stellt Configuration Selektoren und toolmenüs:

[![Diagramm der Symbolleiste des Designers](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

Die **-Designer-Symbolleiste** ermöglicht den Zugriff auf die folgenden Funktionen:

-   **Alternative Layoutauswahl** &ndash; können Sie von anderen Layout-Versionen auswählen.

-   **Geräteauswahl** &ndash; definiert einen Satz von Qualifizierern (z. B. die Größe des Bildschirms, Auflösung und Verfügbarkeit der Tastatur) mit einem bestimmten Gerät verknüpft ist. Sie können auch hinzufügen und neue Geräte löschen.

-   **Auswahl für Android-Version** &ndash; der Android-Version, die das Layout ausgerichtet ist. Der Designer wird das Layout gemäß der ausgewählten Android-Version gerendert.

-   **Designauswahl** &ndash; wählt das Design der Benutzeroberfläche für das Layout.

-   **Konfigurationsauswahl** &ndash; wählt Sie die Gerätekonfiguration, z. B. *Hochformat* oder *Querformat*.

-   **Optionen für Ressourcenqualifizierer** &ndash; öffnet ein Dialogfeld, in dem Dropdown-Menüs für die Auswahl dargestellt *Sprache*, *Benutzeroberflächenmodus*, *Nachtmodus*, und *Runder Bildschirm* Optionen.

-   **Einstellungen für Aktionsleiste** &ndash; konfiguriert die Einstellungen für Aktionsleiste für das Layout.

-   **Design-Editors** &ndash; öffnet die *Design-Editors*, wodurch es möglich, die Elemente des ausgewählten Designs anzupassen.

-   **Material Entwurfsbereich** &ndash; aktiviert oder deaktiviert die *Material Entwurfsbereich*. Das Dropdown-Menü-Element, das in den Entwurfsbereich Material angrenzende öffnet ein Dialogfeld, das Ihnen ermöglicht, die das Raster anpassen.

Jede dieser Funktionen wird in den folgenden Themen ausführlicher erläutert:

-   [Ressourcenqualifizierer und Visualisierungsoptionen](~/android/user-interface/android-designer/resource-qualifiers.md) enthält ausführliche Informationen zu den **Geräteauswahl**, **Android Version Selector**, **Designauswahl**, **Konfigurationsauswahl**, **Qualifikationen Ressourcenoptionen**, und **Einstellungen für Aktionsleiste**.

-   [Alternative Layoutansichten](~/android/user-interface/android-designer/alternative-layout-views.md) erläutert, wie die **Alternative Layoutauswahl**.

-   [Xamarin.Android-Designer Material Design-Funktionen](~/android/user-interface/android-designer/material-design-features.md) bietet eine umfassende Übersicht über die **Design-Editors** und **Material Entwurfsbereich**.

### <a name="context-menu-commands"></a>Befehle im Kontextmenü

Ein Kontextmenü steht sowohl in der **Entwurfsoberfläche** und klicken Sie in der **Dokumentgliederung**. Dieses Menü enthält Befehle, die für das ausgewählte Widget und erleichtert Ihnen die für Sie zum Ausführen von Vorgängen für Container den Container verfügbar sind (das sind nicht immer einfach, wählen Sie auf die **Entwurfsoberfläche**). Hier ist ein Beispiel für ein Kontextmenü wird angezeigt:

[![Beispiel im Kontextmenü der Entwurfsoberfläche einen Rechtsklick auf](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

In diesem Beispiel mit der rechten Maustaste ein `TextView` öffnet ein Kontextmenü, das mehrere Optionen bereitstellt:

-   **LinearLayout** &ndash; öffnet ein Untermenü für die Bearbeitung der `LinearLayout` übergeordnet der `TextView`.

-   **Löschen Sie**, **Kopie**, und **Ausschneiden** &ndash; Vorgänge, die auf die geklickt anwenden `TextView`.


### <a name="zoom-controls"></a>Zoom-Steuerelemente

Die **Entwurfsoberfläche** unterstützt das Zoomen über mehrere Steuerelemente, wie gezeigt:

[![Diagramm der Zoom-Steuerelemente der Entwurfsoberfläche](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

Diese Steuerelemente vereinfachen die bestimmte Bereiche der Benutzeroberfläche im Designer finden Sie unter:

-   **Container markieren** &ndash; Aufschluss über Container für die **Entwurfsoberfläche** , damit sie leichter finden, beim Vergrößern und verkleinern sind.

-   **Normalgröße** &ndash; rendert Sie das Layout Pixel-für-Pixel, damit Sie sehen, wie das Layout in der Auflösung des dem ausgewählten Gerät aussehen wird.

-   **An Fenster anpassen** &ndash; die Zoomstufe so festgelegt, dass das gesamte Layout auf der Entwurfsoberfläche angezeigt wird.

-   **Vergrößern** &ndash; vergrößert inkrementell mit jedem Klick wird das Layout zu vergrößern.

-   **Verkleinern** &ndash; inkrementell verkleinert und bei jedem Mausklick, sodass das Layout auf der Entwurfsoberfläche kleiner erscheinen.

Beachten Sie, dass das ausgewählte Einstellung zoom wirkt sich nicht auf die Benutzeroberfläche der Anwendung zur Laufzeit aus.

## <a name="switching-between-design-and-source-panes"></a>Umschalten zwischen Entwurfs- und Quellcodebereiche

In den Center Streifen zwischen der **Entwurf** und **Quelle** Bereiche, es gibt mehrere Schaltflächen, die verwendet werden, um zu ändern wie die **Entwurf** und **Quelle**Bereiche werden angezeigt:

[![Im Bereich Anzeige Schaltfläche Speicherorte](designer-basics-images/vs/25-pane-buttons-sml.png)](designer-basics-images/vs/25-pane-buttons.png#lightbox)

Diese Schaltflächen wie folgt:

-   **Entwurf** &ndash; diese oberste Schaltfläche **Entwurf**, wählt die **Entwurf** Bereich. Wenn diese Schaltfläche geklickt wird, die **Toolbox** und **Eigenschaften** Bereiche sind aktiviert und die **Text-Editor-Symbolleiste** wird nicht angezeigt. Wenn die **reduzieren** geklickt wird (siehe unten), die **Entwurf** Bereich allein angezeigt wird, ohne die **Quelle** Bereich.

-   **Bereiche tauschen** &ndash; tauscht diese Schaltfläche (dem zwei gegensätzliche Pfeile) die **Entwurf** und **Quelle** Bereiche, damit die **Quelle** Bereich ist auf der linken Seite und die **Entwurf** Bereich ist auf der rechten Seite. Sichern diese Bereiche erneutes Klicken Austausch von Bereitstellungen an ihren ursprünglichen Speicherorten.

-   **Quelle** &ndash; diese Schaltfläche (die zwei gegensätzliche spitzen Klammern ähnelt), wählt die **Quelle** Bereich. Wenn diese Schaltfläche geklickt wird, die **Toolbox** und **Eigenschaften** Bereiche sind deaktiviert, und die **Text-Editor-Symbolleiste** am oberen Rand der Visual Studio sichtbar gemacht wird. Bei der **reduzieren** Schaltfläche geklickt wird (siehe unten), auf die **Quelle** Schaltfläche zeigt die **Quelle** Bereich anstelle von der **Entwurf** im Bereich.

-   **Vertikale Teilung** &ndash; zeigt diese Schaltfläche (der einen senkrechten Strich hat), die **Entwurf** und **Quelle** Bereiche-Seite-an-Seite. Dies ist die Standardreihenfolge der.

-   **Horizontale Teilung** &ndash; zeigt diese Schaltfläche (die eine horizontale Leiste ähnelt), die **Entwurf** Bereich über die **Quelle** Bereich. **Bereiche tauschen** geklickt werden kann, um das Platzieren der **Quelle** Bereich über die **Entwurf** Bereich.

-   **Bereich Zuklappen** &ndash; diese Schaltfläche (der nach rechts zeigenden spitzen Klammern hat) die Dual-Bereich Anzeige von "reduziert" **Entwurf** und **Quelle** in einer einzigen Ansicht eines Diese Bereiche.
    Diese Schaltfläche wird die **erweitern Sie im Bereich** Schaltfläche (ähnlich wie die beiden nach links zeigenden spitzen Klammern), die geklickt werden kann, um die Ansicht an zweiteiligen zurückzugeben (**Entwurf** und **Quelle**) Anzeigemodus.

Wenn **Bereich reduzieren** geklickt wird, nur die **Entwurf** Bereich wird angezeigt. Sie können jedoch klicken die **Quelle** Schaltfläche, um stattdessen nur Anzeigen der **Quelle** Bereich. Klicken Sie auf die **Entwurf** wieder zurück zum die **Entwurf** Bereich.

## <a name="source-pane"></a>Bereich "Datenquelle"

Die **Quelle** Bereich zeigt die XML-Quelle, die zugrunde liegende das Design angezeigt, auf die **Entwurfsoberfläche**. Da beide Ansichten zur gleichen Zeit verfügbar sind, ist es möglich, einen UI-Entwurf zu erstellen, indem Sie hin-und hergefahren zwischen eine visuelle Darstellung des Entwurfs und der zugrunde liegenden XML-Quelle für den Entwurf:

[![Beispiel-XML-Quelle im Bereich "Datenquelle"](designer-basics-images/vs/22-source-pane-w158-sml.png)](designer-basics-images/vs/22-source-pane-w158.png#lightbox)

An die XML-Quelle vorgenommenen Änderungen werden sofort gerendert, auf die **Entwurfsoberfläche**; Änderungen auf der **Entwurfsoberfläche** dazu führen, dass die XML-Quelle angezeigt, der **Quelle** im Bereich entsprechend aktualisiert werden. Wenn Sie Änderungen vornehmen, um XML-Code in die **Quelle** Bereich, automatische Vervollständigung und IntelliSense-Funktionen stehen für Geschwindigkeit Entwicklung von XML-basierten Benutzeroberflächen wie unten beschrieben.

Für größere einfache Navigation bei der Arbeit mit lange XML-Dateien, die **Quelle** -Bereich unterstützt die Visual Studio-Bildlaufleiste (wie auf der rechten Seite im vorherigen Screenshot dargestellt). Weitere Informationen zu die Bildlaufleiste, finden Sie unter [wie Verfolgen von Code durch Anpassen der Scrollleiste](https://msdn.microsoft.com/library/dn237345.aspx).


### <a name="autocompletion"></a>Automatische Vervollständigung

Wenn Sie beginnen, geben Sie den Namen eines Attributs für ein Widget, drücken Sie <kbd>STRG + LEERTASTE</kbd> um eine Liste der möglichen vervollständigungen anzuzeigen. Z. B. nach der Eingabe `android:lay` im folgenden Beispiel (gefolgt von der Eingabe <kbd>STRG + LEERTASTE</kbd>), in der folgende Liste wird angezeigt:

[![Automatische Vervollständigung des Layout-Attributs](designer-basics-images/vs/23-autocompletion-w158-sml.png)](designer-basics-images/vs/23-autocompletion-w158.png#lightbox)

Drücken Sie die <kbd>EINGABETASTE</kbd> auf den Abschluss der ersten aufgelisteten akzeptieren, oder verwenden die Pfeiltasten, um einen Bildlauf zu der gewünschten Vervollständigung, und drücken Sie <kbd>EINGABETASTE</kbd>. Alternativ können Sie die Maus verwenden, scrollen Sie zu, und klicken Sie auf die gewünschte Vervollständigung.

### <a name="intellisense"></a>IntelliSense

Nachdem Sie geben Sie ein neues Attribut für ein Widget, und starten sie einen Wert zuzuweisen, wird IntelliSense angezeigt, nachdem ein triggerzeichen eingegeben eingegeben wird und enthält eine Liste der gültigen Werte für dieses Attribut verwenden. Beispiel: Nachdem für die erste doppelte Anführungszeichen eingegeben wird `android:layout_width` im folgenden Beispiel wird eine automatische Vervollständigung-Auswahl angezeigt, geben Sie die Liste der gültigen Werte für diese Breite:

[![Beispiel für die Layoutbreite](designer-basics-images/vs/24-intellisense-w158-sml.png)](designer-basics-images/vs/24-intellisense-w158.png#lightbox)

Am unteren Rand dieses Popups befinden sich zwei Schaltflächen (wie im obigen Screenshot Rot). Auf der **Projektressourcen** Schaltfläche auf der linken Seite die Liste beschränkt, auf Ressourcen, die Teil des app-Projekts, während Sie auf die **Frameworkressourcen** rechts auf die Schaltfläche wird die Liste, um eingeschränkt. Zeigen Sie die Ressourcen aus dem Framework verfügbar sind.
Diese Schaltflächen zu wechseln, aktivieren oder deaktivieren: Sie können diese erneut aus, um die folteraktion deaktivieren klicken, dass jede bereitstellt.



## <a name="properties-pane"></a>Bereich "Eigenschaften"

Der Designer unterstützt die Bearbeitung des Widgeteigenschaften über die **Eigenschaften** Bereich: 

![Screenshot des Fensters Eigenschaften](designer-basics-images/vs/08-property-pad.png)

Die Eigenschaften aufgeführt, die der **Eigenschaften** Bereich ändern, je nachdem, welche Widgets ausgewählt ist, auf, die **Entwurfsoberfläche**.

### <a name="default-values"></a>Standardwerte

Die Eigenschaften von den meisten Widgets werden im leeren die **Eigenschaften** Fenster da ihre Werte aus dem ausgewählten Android Design erben.
Die **Eigenschaften** Fenster wird nur angezeigt, für das ausgewählte Widget explizit festgelegt werden; er zeigt nicht die Werte, die aus dem Design geerbt werden.

### <a name="referencing-resources"></a>Verweisen auf Ressourcen

Einige Eigenschaften können auf Ressourcen, die in andere Dateien als das Layout definiert sind verweisen **axml** Datei. Die am häufigsten vorkommenden Fälle dieser Art sind `string` und `drawable` Ressourcen. Allerdings Verweise können auch verwendet werden, für andere Ressourcen, z. B. `Boolean` Werte und Dimensionen. Wenn eine Eigenschaft Ressourcenverweise unterstützt, wird ein Symbol "Durchsuchen" (ein Quadrat) neben dem Eintrag "Text" für die Eigenschaft angezeigt. Diese Schaltfläche wird beim Klicken auf eine Ressourcenauswahl geöffnet.

Der folgende Screenshot zeigt beispielsweise die verfügbaren Optionen, wenn Sie rechts neben das Textfeld für das abgedunkelten Quadrat Klicken eine `Text` Widget in der **Eigenschaften** Fenster:

[![Beispiel für Liste mit Optionen für text](designer-basics-images/vs/09-text-options-sml.png)](designer-basics-images/vs/09-text-options.png#lightbox)

Wenn **Ressource...**  geklickt wird, die **Ressource auswählen** Dialogfeld wird angezeigt:

[![Ressourcen-beispielscreenshot mit mehreren aufgelisteten Ressourcen](designer-basics-images/vs/09b-resources-w158-sml.png)](designer-basics-images/vs/09b-resources-w158.png#lightbox)

Aus dieser Liste können Sie auswählen, eine Textressource für dieses Widgets anstatt einer hartcodierung des Texts in die Verwendung der **Eigenschaften** Bereich. Das folgende Beispiel veranschaulicht die Ressourcenauswahl für die `Src` Eigenschaft eine `ImageView`:

[![Die Ressourcenauswahl Symbolressource für eine ImageView auflisten](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

Klicken Sie auf das leere Quadrat rechts neben der `Src` Eigenschaft geöffnet wird die **Ressource auswählen** Dialog mit einer Liste von Ressourcen, die im Bereich von Farben (siehe oben) bis zeichenbarer Ressourcen.


### <a name="boolean-property-references"></a>Boolesche Eigenschaftsverweisen

*Boolesche* Eigenschaften werden normalerweise als Häkchen neben einer Eigenschaft im Eigenschaftenfenster ausgewählt. Sie können festlegen, eine `true` oder `false` Wert durch Aktivieren oder Deaktivieren dieses Kontrollkästchen, oder Sie können Verweis auf eine Eigenschaft auswählen, indem Sie auf das dunkel ausgefüllte Quadrat rechts neben der Eigenschaft. Im folgenden Beispiel Text geändert, GROSSBUCHSTABEN durch Klicken auf die **Text GROSSBUCHSTABEN** boolesche Eigenschaftenverweis zugeordnet `TextView`:

![Beispiel zum Festlegen von boolescher Eigenschaften](designer-basics-images/vs/11-boolean.png)

## <a name="editing-properties-inline"></a>Bearbeiten von Eigenschaften inline

Android Designer unterstützt die direkte Bearbeitung bestimmter Eigenschaften auf der **Entwurfsoberfläche** (sodass Sie nicht für diese Eigenschaften in der Eigenschaftenliste suchen). Eigenschaften, die direkt bearbeitet werden können, enthalten Text, Rändern und der Größe.

### <a name="text"></a>Text

Die Texteigenschaften der bei einigen Widgets (z. B. `Button` und `TextView`), können bearbeitet werden, direkt auf die **Entwurfsoberfläche**. Durch Doppelklicken auf ein Widget wird es in den Bearbeitungsmodus wechseln kann, fügen Sie wie unten dargestellt:

![Textressource für die Zeichenfolge Hello](designer-basics-images/vs/12-text-resource.png "Textressource")

Sie können einen neuen Wert eingeben, oder können Sie eine neue Ressourcenzeichenfolge eingeben. Im folgenden Beispiel die `@string/hello` Ressource wird durch den Text ersetzt wird `CLICK THIS BUTTON`:

![UMSCHALT + EINGABETASTE, um Text automatisch in eine neue Ressource verknüpfen](designer-basics-images/vs/13-shift-enter-resource.png)

Befindet sich diese Änderung im daraufhin angezeigten Widget `text` Eigenschaft; er ändert nicht den zugewiesenen Wert der `@string/hello` Ressource.
Wenn Sie eine neue Zeichenfolge eingegeben werden, drücken Sie <kbd>UMSCHALT</kbd> +
<kbd>EINGABETASTE</kbd> des eingegebenen Texts automatisch mit einer neuen Ressource zu verknüpfen.

### <a name="margin"></a>Margin

Wenn Sie ein Widget auswählen, zeigt der Designer Handles, die Ihnen ermöglichen, die Größe oder den Rand des Widgets interaktiv zu ändern. Auf das Widget aus, während Sie diese Option ausgewählt ist, schaltet zwischen Rand Textbearbeitungsmodus "und" Größe Bearbeitungsmodus.

Wenn Sie ein Widget zum ersten Mal klicken, werden Ziehpunkte angezeigt. Wenn Sie die Maus auf einen der Ziehpunkte verschieben, wird der Designer zeigt die Eigenschaft, die das Handle geändert wird (wie unten dargestellt, für die `layout_marginLeft` Eigenschaft):

![Screenshot der Rand behandelt, im Designer](designer-basics-images/vs/15-margin-handles.png)

Wenn ein Rand bereits festgelegt wurde, werden gepunktete Linien angezeigt, der angibt, des der Rand belegte Speicherplatzes:

![Beispiel für die gepunkteten Linien kennzeichnen Raum auf eine Schaltfläche](designer-basics-images/vs/16-margins-set.png)

### <a name="size"></a>Größe

Wie bereits erwähnt, können Sie wechseln in den Bearbeitungsmodus mit der Größe für durch Klicken auf ein Widget, während es bereits aktiviert ist. Klicken Sie auf die dreieckige Handle zum Festlegen der Größe für die angegebene Dimension, `wrap_content`:

![Wrap-Inhalte und Ändern der Größe von handles](designer-basics-images/vs/17-wrap-content.png)

Klicken auf die **Umschließen von Inhalt** Handle Widgets in dieser Dimension verkleinert, sodass sie nicht größer als der zu umschließende des eingeschlossenen Inhalts erforderlich ist. In diesem Beispiel wird verkleinert, den Text der Schaltfläche horizontal, wie im folgenden Screenshot gezeigt.

Wenn der Wert für die Dateigröße auf festgelegt ist **Content umschließen**, zeigt ein dreieckiger Handle in die entgegengesetzte Richtung zum Ändern der Größe, zeigt der Designer `match_parent`:

![Das Handle des übergeordneten Übereinstimmung](designer-basics-images/vs/18-match-parent.png)

Klicken auf die **Übereinstimmung übergeordneten** Handle stellt die Größe in dieser Dimension, damit sie das übergeordnete Widget identisch ist.

Darüber hinaus kann durch Ziehen die zirkuläre Ziehpunkt (wie in den obigen Screenshots gezeigt) das Widget an eine beliebige Größe `dp` Wert. In diesem Fall sowohl **Content umschließen** und **Übereinstimmung übergeordneten** Handles für diese Dimension angezeigt werden:

![Zirkuläre Ziehpunkte](designer-basics-images/vs/19-resize-dp.png)

Bearbeiten oder zulassen der nicht von allen Containern der `Size` eines Widgets. Beachten Sie, dass z. B. im folgenden Screenshot mit der `LinearLayout` ausgewählt haben, werden nicht die Handles zur Größenänderung angezeigt:

![Keine Handles zur Größenänderung](designer-basics-images/vs/20-no-resize-handles.png)


## <a name="document-outline"></a>Dokumentgliederung

Die **Dokumentgliederung** zeigt die widgethierarchie des Layouts.
Im folgenden Beispiel, das mit `LinearLayout` Widget ausgewählt ist:

![Beispiel für ein Dokument Kontur](designer-basics-images/vs/21-document-outline.png)

Den Überblick über das ausgewählte Widget (in diesem Fall eine `LinearLayout`) wird ebenfalls hervorgehoben, die **Entwurfsoberfläche**. Das ausgewählte Widget in der Dokumentgliederung bleibt synchron mit dem auf die **Entwurfsoberfläche**. Dies ist nützlich für die Auswahl von Gruppen anzeigen, die nicht immer einfach, wählen Sie auf die **Entwurfsoberfläche**.

Die **Dokumentgliederung** unterstützt das Kopieren und einfügen, oder Sie können mithilfe von Drag und drop. Drag & Drop von unterstützt wird die **Dokumentgliederung** auf der **Entwurfsoberfläche** sowie von der **Entwurfsoberfläche** auf die **Dokumentgliederung**. Darüber hinaus Rechtsklick auf ein Element in der **Dokumentgliederung** zeigt das Kontextmenü für das Element (das gleiche Kontextmenü, das angezeigt wird, wenn Sie dieses gleiche Widgets mit einem Rechtsklick die **Entwurfsoberfläche**).




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="launching-the-designer"></a>Starten den Designer

Der Designer wird automatisch gestartet, wenn ein Layout erstellt wird, oder durch Doppelklicken auf eine vorhandene axml-Datei gestartet werden. Doppelklicken Sie z. B. **Main.axml** in die **Ressourcen > Layout** Ordner wird den Designer geladen, wie unten dargestellt:

[![Designer-Fenster in Visual Studio für Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

Ebenso können Sie ein neues Layout hinzufügen, indem Sie mit der rechten Maustaste die **Layout** Ordner in der **Lösungspad** und **hinzufügen > neue Datei > Android > Layout**:

[![Dialogfeld "neue Datei" hinzufügen](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

Dies erstellt eine neue axml-Datei und lädt es auf die Entwurfsoberfläche.

## <a name="designer-features"></a>Die Funktionen eines Designers

Der Designer besteht aus mehrere Abschnitte, die die verschiedenen Funktionen unterstützen, wie im folgenden Screenshot gezeigt:

[![Diagramm der-Designerbereichen](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

Wenn Sie ein Layout im Designer bearbeiten, verwenden Sie die folgenden Funktionen zum Erstellen und gestalten Ihren Entwurf:

-   **Entwurfsoberfläche** &ndash; erleichtert der grafischen Erstellung von der Benutzeroberfläche ermöglicht eine editierbare Darstellung wie das Layout auf dem Gerät angezeigt wird.

-   **Symbolleiste** &ndash; zeigt eine Liste der Selektoren: **Gerät**, **Version**, **Design**, Layout, Konfiguration und Einstellungen für Aktionsleiste. Die Symbolleiste enthält außerdem die Symbole für den Start der Design-Editor sowie für die Aktivierung der Material-Entwurfsbereich.

-   **Toolbox** &ndash; enthält eine Liste von Widgets und Layouts, mit denen Sie mit Drag & drop auf die Entwurfsoberfläche ziehen können.

-   **Das Pad "Eigenschaft"** &ndash; enthält die Eigenschaften des ausgewählten Widgets zum Anzeigen und bearbeiten.

-   **Dokumentgliederung** &ndash; zeigt die Struktur von Widgets, die das Layout bilden. Sie können ein Element in der Struktur, die dazu führen, dass sie im Designer ausgewählt klicken. Darüber hinaus werden durch das Klicken auf ein Element in der Struktur die Eigenschaften des Elements in das Pad "Eigenschaft" geladen.

## <a name="toolbar"></a>Symbolleiste

Die Symbolleiste (oberhalb der Entwurfsoberfläche positioniert) bietet Configuration Selektoren und toolmenüs:

[![Diagramm der Symbolleiste des Designers](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

Die Symbolleiste bietet Zugriff auf die folgenden Funktionen:

-   **Alternative Layoutauswahl** &ndash; können Sie von anderen Layout-Versionen auswählen.

-   **Geräteauswahl** &ndash; definiert einen Satz von Qualifizierern, die ein bestimmtes Gerät wie Bildschirmgröße, Auflösung und Tastatur Verfügbarkeit zugeordnet. Sie können auch hinzufügen und neue Geräte löschen.

-   **Auswahl für Android-Version** &ndash; der Android-Version, die das Layout ausgerichtet ist. Der Designer wird das Layout gemäß der ausgewählten Android-Version gerendert.

-   **Designauswahl** &ndash; wählt das Design der Benutzeroberfläche für das Layout.

-   **Konfigurationsauswahl** &ndash; wählt Sie die Gerätekonfiguration, z. B. *Hochformat* oder *Querformat*.

-   **Optionen für Ressourcenqualifizierer** &ndash; öffnet ein Dialogfeld, in dem Dropdown-Menüs für die Auswahl dargestellt *Sprache*, *Benutzeroberflächenmodus*, *Nachtmodus*, und *Runder Bildschirm* Optionen.

-   **Einstellungen für Aktionsleiste** &ndash; konfiguriert die Einstellungen für Aktionsleiste für das Layout.

-   **Design-Editors** &ndash; öffnet die *Design-Editors*, wodurch es möglich, die Elemente des ausgewählten Designs anzupassen.

-   **Material Entwurfsbereich** &ndash; aktiviert oder deaktiviert die *Material Entwurfsbereich*. Das Dropdown-Menü-Element, das in den Entwurfsbereich Material angrenzende öffnet ein Dialogfeld, das Ihnen ermöglicht, die das Raster anpassen.

Jede dieser Funktionen wird in den folgenden Themen ausführlicher erläutert:

[Ressourcenqualifizierer und Visualisierungsoptionen](~/android/user-interface/android-designer/resource-qualifiers.md) enthält ausführliche Informationen zu den **Geräteauswahl**, **Android Version Selector**, **Designauswahl**, **Konfigurationsauswahl**, **Qualifikationen Ressourcenoptionen**, und **Einstellungen für Aktionsleiste**.

[Alternative Layoutansichten](~/android/user-interface/android-designer/alternative-layout-views.md) erläutert, wie die **Alternative Layoutauswahl**.

[Material Design-Features](~/android/user-interface/android-designer/material-design-features.md) bietet eine umfassende Übersicht über die **Design-Editors** und **Material Entwurfsbereich**.

## <a name="design-surface"></a>Entwurfsoberfläche

Der Designer ermöglicht es Ihnen, Drag & drop von Widgets aus der Toolbox auf die Entwurfsoberfläche. Bei Widgets im Designer interagieren mit (neue Widgets hinzufügen oder Neupositionieren von vorhandenen), werden vertikale und horizontale Linien angezeigt, um den verfügbaren Einfügemarken zu markieren. Im folgenden Beispiel eine neue `Button` Widget auf die Entwurfsoberfläche gezogen wird:

[![Beispiel für Einfügen von Zeilen auf der Entwurfsoberfläche](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

Widgets können darüber hinaus kopiert werden: Sie können kopieren und einfügen, um ein Widget, oder Sie kopieren kann Drag & drop eine vorhandene Widget beim Drücken der <kbd>STRG</kbd> Schlüssel.

### <a name="context-menu-commands"></a>Befehle im Kontextmenü

Ein Kontextmenü steht sowohl in der Entwurfsoberfläche als auch in der Dokumentgliederung. Dieses Menü enthält Befehle, die für das ausgewählte Widget und erleichtert es Ihnen, Vorgänge in Containern auszuführen (die nicht immer einfach, wählen Sie auf der Entwurfsoberfläche sind) den Container verfügbar sind. Hier ist ein Beispiel für ein Kontextmenü wird angezeigt:

[![Beispiel im Kontextmenü der Entwurfsoberfläche einen Rechtsklick auf](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

In diesem Beispiel mit der rechten Maustaste ein `Button` öffnet ein Kontextmenü, das mehrere Optionen bereitstellt:

-   **LinearLayout** &ndash; öffnet ein Untermenü für die Bearbeitung der `LinearLayout` übergeordnet der `Button`.

-   **Ausschneiden**, **Kopie**, und **löschen** &ndash; Vorgänge, die auf die geklickt anwenden `Button`.

### <a name="zoom-controls"></a>Zoom-Steuerelemente

Die Entwurfsoberfläche unterstützt das Zoomen über mehrere Steuerelemente, wie gezeigt:

[![Diagramm der Zoom-Steuerelemente der Entwurfsoberfläche](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

Diese Steuerelemente vereinfachen die bestimmte Bereiche der Benutzeroberfläche im Designer finden Sie unter:

-   **Container markieren** &ndash; Container auf der Entwurfsoberfläche hervorgehoben, damit sie leichter finden, beim Vergrößern und verkleinern sind.

-   **Normalgröße** &ndash; rendert Sie das Layout Pixel-für-Pixel, damit Sie sehen, wie das Layout in der Auflösung des dem ausgewählten Gerät aussehen wird.

-   **An Fenster anpassen** &ndash; die Zoomstufe so festgelegt, dass das gesamte Layout auf der Entwurfsoberfläche angezeigt wird.

-   **Vergrößern** &ndash; vergrößert inkrementell mit jedem Klick wird das Layout zu vergrößern.

-   **Verkleinern** &ndash; inkrementell verkleinert und bei jedem Mausklick, sodass das Layout auf der Entwurfsoberfläche kleiner erscheinen.

Beachten Sie, dass das ausgewählte Einstellung zoom wirkt sich nicht auf die Benutzeroberfläche der Anwendung zur Laufzeit aus.

## <a name="property-pad"></a>Das Pad "Eigenschaft"

Der Designer unterstützt die Bearbeitung des Widgeteigenschaften über die **Pad "Eigenschaft"**. Die Eigenschaften aufgeführt, in das Pad "Eigenschaft" ändern, je nachdem, das welche Widget in der Entwurfsoberfläche ausgewählt ist. Wenn die `Button` im vorherigen Beispiel aktiviert ist, die Eigenschaften, die `Button` Widgets werden angezeigt:

[![Screenshot, der das Pad "Eigenschaft"](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

## <a name="property-pad-sections"></a>Pad Eigenschaftenabschnitte

Das Pad "Eigenschaft" ist in mehrere Abschnitte unterteilt, die ähnliche Eigenschaften zusammen gruppieren &ndash; Dies erleichtert es, wichtige Eigenschaften gesucht werden soll:

-   **Widget** &ndash; Main-Eigenschaften des Widgets, wie z. B. `id`, `visibility`, `text`usw. Eigenschaften für die Verwaltung des Widget-Inhalte wurden in der Regel hier abgelegt.

-   **Stil** &ndash; Eigenschaften, die die visuelle Darstellung des Widgets, wie z. B. ändern `font`, `text color`, `background`usw.

-   **Layout** &ndash; Eigenschaften, die die Position und Größe des Widgets festgelegt.

-   **Führen Sie einen Bildlauf** &ndash; Scrolleigenschaften.

-   **Verhalten** &ndash; Flags, die festlegen, wie das Widget verhält.

### <a name="default-values"></a>Standardwerte

Die Eigenschaften von den meisten Widgets werden im leeren die **Pad "Eigenschaft"** da ihre Werte aus dem ausgewählten Android Design erben. Die **Pad "Eigenschaft"** wird nur angezeigt, für das ausgewählte Widget explizit festgelegt werden; er zeigt nicht die Werte, die aus dem Design geerbt werden.

### <a name="referencing-resources"></a>Verweisen auf Ressourcen

Einige Eigenschaften können auf Ressourcen, die in andere Dateien als das Layout definiert sind verweisen **axml** Datei. Die am häufigsten vorkommenden Fälle dieser Art sind `string` und `drawable` Ressourcen. Allerdings Verweise können auch verwendet werden, für andere Ressourcen, z. B. `Boolean` Werte und Dimensionen.
Wenn eine Eigenschaft Ressourcenverweise ein Symbol "Durchsuchen" unterstützt (eine mit den Auslassungspunkten, &hellip;) neben dem Texteintrag für die Eigenschaft angezeigt wird.
Durch Klicken auf diese Schaltfläche geöffnet einen Selektor für die Ressource.

Der folgende Screenshot zeigt beispielsweise die verfügbaren Ressourcen, wenn auf die Auslassungspunkte rechts neben das Textfeld für eine `Button` Widget in der **Pad "Eigenschaft"**:

[![Screenshot von Beispiel-Ressourcen mit zwei Ressourcen aufgeführt](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

Das folgende Beispiel veranschaulicht die Ressourcenauswahl für die `Src` Eigenschaft eine `ImageView`:

[![Die Ressourcenauswahl Symbolressource für eine ImageView auflisten](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

### <a name="boolean-property-references"></a>Boolesche Eigenschaftsverweisen

*Boolesche* Eigenschaften werden normalerweise als ein Kontrollkästchen im Bereich "Eigenschaft" angezeigt. Wenn eine `Boolean` -Eigenschaft unterstützt die Ressourcenverweise, kleine das Kontrollkästchen neben der Eigenschaft erscheint. Bedeutet, dass ein aktiviertes Kontrollkästchen `true` und ein leeres Feld bedeutet `false`. Sie können auch direkt einen Wert eingeben, z. B. `true` oder `false`. Bewegen des Mauszeigers über die Eingabe wird eine kleine Text-Feld (Symbol). Sie können darauf klicken, wenn Sie den Wert manuell eingeben möchten.

[![Beispiel zum Festlegen von boolescher Eigenschaften](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>Gruppeneigenschaften

Bei einigen Widgets mehrwertige Eigenschaften besitzen, die zusammen gruppiert werden (z. B. `Padding`, z. B.). Diese Eigenschaftswerte finden Sie in der **Pad "Eigenschaft"** in einer einzigen, erweiterbaren Zeile. Einige dieser Eigenschaften können bearbeitet werden direkt in der Zeile gruppiert, wie z. B. die `Padding` Eigenschaft fest, wie unten:

[![Beispieleinstellungen für die Padding-Eigenschaft](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

## <a name="editing-properties-inline"></a>Bearbeiten von Eigenschaften inline

Android Designer unterstützt die direkte Bearbeitung bestimmter Eigenschaften auf der Entwurfsoberfläche (damit Sie nicht für diese Eigenschaften in der Eigenschaftenliste suchen). Eigenschaften, die direkt bearbeitet werden können, enthalten Text, Rändern und der Größe.

### <a name="text"></a>Text

Der Text von einigen Widgets-Eigenschaften (z. B. `Button` und `TextView`), können direkt auf der Entwurfsoberfläche bearbeitet werden. Durch Doppelklicken auf ein Widget wird es in den Bearbeitungsmodus wechseln kann, fügen Sie wie unten dargestellt:

[![Textressource für die Hello-Zeichenfolge](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

Sie können einen neuen Wert eingeben, oder können Sie eine neue Ressourcenzeichenfolge eingeben. Im folgenden Beispiel die `@string/hello` Ressource wird durch den Text ersetzt wird `CLICK THIS BUTTON`:

[![UMSCHALT + EINGABETASTE, um Text automatisch in eine neue Ressource verknüpfen](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

Befindet sich diese Änderung im daraufhin angezeigten Widget `text` Eigenschaft; er ändert nicht den zugewiesenen Wert der `@string/hello` Ressource.
Wenn Sie eine neue Zeichenfolge eingegeben werden, drücken Sie <kbd>UMSCHALT</kbd> +
<kbd>EINGABETASTE</kbd> des eingegebenen Texts automatisch mit einer neuen Ressource zu verknüpfen.

### <a name="margin"></a>Margin

Wenn Sie ein Widget auswählen, zeigt der Designer Handles, die Ihnen ermöglichen, die Größe oder den Rand des Widgets interaktiv zu ändern. Auf das Widget aus, während Sie diese Option ausgewählt ist, schaltet zwischen Rand Textbearbeitungsmodus "und" Größe Bearbeitungsmodus.

Wenn Sie ein Widget zum ersten Mal klicken, werden Ziehpunkte angezeigt. Wenn Sie die Maus auf einen der Ziehpunkte verschieben, wird der Designer zeigt die Eigenschaft, die das Handle geändert wird (wie unten dargestellt, für die `layout_marginLeft` Eigenschaft):

[![Screenshot der Rand behandelt, im Designer](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

Wenn ein Rand bereits festgelegt wurde, werden gepunktete Linien angezeigt, der angibt, des der Rand belegte Speicherplatzes:

[![Beispiel für die gepunkteten Linien kennzeichnen Raum auf eine Schaltfläche](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

### <a name="size"></a>Größe

Wie bereits erwähnt, können Sie wechseln in den Bearbeitungsmodus mit der Größe für durch Klicken auf ein Widget, während es bereits aktiviert ist. Klicken Sie auf die dreieckige Handle zum Festlegen der Größe für die angegebene Dimension, `wrap_content`:

[![Wrap-Inhalte und Ändern der Größe von handles](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

Klicken auf die **Umschließen von Inhalt** Handle wird das Widget in dieser Dimension so nicht größer sein als zum Umschließen des eingeschlossenen Inhalts verkleinert. In diesem Beispiel wird verkleinert, den Text der Schaltfläche horizontal, wie im folgenden Screenshot gezeigt.

Wenn der Wert für die Dateigröße auf festgelegt ist **Content umschließen**, zeigt ein dreieckiger Handle in die entgegengesetzte Richtung zum Ändern der Größe, zeigt der Designer `match_parent`:

[![Das Handle des übergeordneten Übereinstimmung](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

Klicken auf die **Übereinstimmung übergeordneten** Handle stellt die Größe in dieser Dimension, damit sie das übergeordnete Widget identisch ist.

Darüber hinaus kann durch Ziehen die zirkuläre Ziehpunkt (wie in den obigen Screenshots gezeigt) das Widget an eine beliebige Größe `dp` Wert. In diesem Fall sowohl **Content umschließen** und **Übereinstimmung übergeordneten** Handles für diese Dimension angezeigt werden:

[![Zirkuläre Ziehpunkte](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

Bearbeiten oder zulassen der nicht von allen Containern der `Size` eines Widgets. Beachten Sie, dass z. B. im folgenden Screenshot mit der `LinearLayout` ausgewählt haben, werden nicht die Handles zur Größenänderung angezeigt:

[![Keine Handles zur Größenänderung](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

## <a name="document-outline"></a>Dokumentgliederung

Die **Dokumentgliederung** zeigt die widgethierarchie des Layouts.
Im folgenden Beispiel, das mit `LinearLayout` Widget ausgewählt ist:

[![Dokumentgliederung](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

Den Überblick über das ausgewählte Widget (in diesem Fall eine `LinearLayout`) auch auf der Entwurfsoberfläche hervorgehoben ist. Das ausgewählte Widget in der Dokumentgliederung bleibt synchron mit dem auf die Entwurfsoberfläche. Dies ist nützlich für die Auswahl von Gruppen anzeigen, die nicht immer einfach, wählen Sie auf der Entwurfsoberfläche angezeigt werden.

Die Dokumentgliederung unterstützt das Kopieren und einfügen, oder Sie können mithilfe von Drag und drop. Ziehen und ablegen, wird von der Dokumentgliederung auf der Entwurfsoberfläche als auch von der Entwurfsoberfläche der dokumentgliederung unterstützt. Rechtsklick auf ein Element im Dokumentgliederungsfenster zeigt außerdem das Kontextmenü für das Element (das gleiche Kontextmenü, das angezeigt wird, wenn Sie dieses gleiche Widgets auf der Entwurfsoberfläche mit der rechten Maustaste).

-----
