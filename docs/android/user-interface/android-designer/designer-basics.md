---
title: Grundlagen von xamarin. Android Designer
description: In diesem Thema werden die Funktionen von xamarin. Android Designer vorgestellt, es wird erläutert, wie der-Designer gestartet, die Designoberfläche beschrieben wird und wie der Eigenschaften Bereich zum Bearbeiten von widgeeigenschaften verwendet wird.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/05/2018
ms.openlocfilehash: 2d5f20326de56bca77dd8fdd742515e003f996e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029513"
---
# <a name="xamarinandroid-designer-basics"></a>Grundlagen von xamarin. Android Designer

_In diesem Thema werden die Funktionen von xamarin. Android Designer vorgestellt, es wird erläutert, wie der-Designer gestartet, die Designoberfläche beschrieben wird und wie der Eigenschaften Bereich zum Bearbeiten von widgeeigenschaften verwendet wird._

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="launching-the-designer"></a>Starten des Designers

Der Designer wird automatisch gestartet, wenn ein Layout erstellt wird, oder er kann durch Doppelklicken auf eine vorhandene Layoutdatei gestartet werden. Wenn Sie z. b. im Ordner **Resources > Layout** auf **activity_main. axml** doppelklicken, wird der Designer wie in diesem Screenshot gezeigt geladen:

[Bildschirm "![-Designer" in Visual Studio](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

Ebenso können Sie ein neues Layout hinzufügen, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den **layoutordner** klicken und **> Neues Element hinzufügen auswählen. > Android-Layout**:

[Dialogfeld "Neues Element hinzufügen"![](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

Dadurch wird eine neue **axml** -Layoutdatei erstellt und in den Designer geladen.

> [!TIP]
> Neuere Releases von Visual Studio unterstützen das Öffnen von XML-Dateien in Android Designer.
>
> Sowohl AXML- als auch XML-Dateien werden in Android Designer unterstützt.

## <a name="designer-features"></a>Designer Features

Der Designer besteht aus mehreren Abschnitten, die die verschiedenen Features unterstützen, wie im folgenden Screenshot zu sehen:

[![Diagramm der Designer Bereiche](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

Wenn Sie ein Layout im Designer bearbeiten, verwenden Sie die folgenden Funktionen, um den Entwurf zu erstellen und zu strukturieren:

- **Designoberfläche** &ndash; ermöglicht die visuelle Erstellung der Benutzeroberfläche, indem Sie eine bearbeitbare Darstellung der Darstellung des Layouts auf dem Gerät erhalten. Der **Designoberfläche** wird im **Entwurfs** Bereich angezeigt (mit der **Designer-Symbolleiste** ).

- Der **Quell** Bereich &ndash; stellt eine Ansicht der zugrunde liegenden XML-Quelle bereit, die dem auf dem **Designoberfläche**dargestellten Entwurf entspricht.

- **Designer-Symbolleiste** &ndash; eine Liste der Selektoren anzeigt: **Geräte**-, **Versions** **-, Design-,** Layoutkonfiguration und Aktionsleiste Einstellungen. Die **Designer-Symbolleiste** enthält auch Symbole zum Starten des Design-Editors und zum Aktivieren des Material Design-Rasters.

- **Toolbox** &ndash; enthält eine Liste von Widgets und Layouts, die Sie per Drag & amp; Drop auf die **Designoberfläche**verschieben können.

- **Eigenschaften Fenster** &ndash; listet die Eigenschaften des ausgewählten Widgets zum Anzeigen und Bearbeiten auf.

- **Dokument** Gliederung &ndash; zeigt die Struktur der Widgets an, aus denen das Layout besteht. Sie können auf ein Element in der Struktur klicken, damit es auf dem **Designoberfläche**ausgewählt wird. Wenn Sie auf ein Element in der Struktur klicken, werden auch die Eigenschaften des Elements im **Eigenschaften** Fenster geladen.

## <a name="design-surface"></a>Entwurfsoberfläche

Der Designer ermöglicht das ziehen und Ablegen von Widgets aus der Toolbox auf die **Designoberfläche**. Wenn Sie im Designer mit Widgets interagieren (indem Sie entweder neue Widgets hinzufügen oder vorhandene neu positionieren), werden vertikale und horizontale Linien angezeigt, um die verfügbaren Einfügepunkte zu markieren. Im folgenden Beispiel wird ein neues `Button`-Widget in den **Designoberfläche**gezogen:

[![Beispiel einfügelinien in Designoberfläche](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

Außerdem können Widgets kopiert werden: Sie können das Widget mithilfe von kopieren und Einfügen kopieren, oder Sie können ein vorhandenes Widget per Drag & amp; Drop beim Drücken der <kbd>STRG</kbd> -Taste ablegen.

### <a name="designer-toolbar"></a>Designer-Symbolleiste

Die **Designer-Symbolleiste** (oberhalb der **Designoberfläche**) zeigt konfigurationsselektoren und Tool Menüs an:

[![Diagramm der Designer-Symbolleiste](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

Die **Symbolleiste des Designers** bietet Zugriff auf die folgenden Features:

- **Alternativer layoutselektor** &ndash; ermöglicht Ihnen die Auswahl aus unterschiedlichen Layoutversionen.

- Die **Geräteauswahl** &ndash; definiert eine Gruppe von Qualifizierern (z. b. Bildschirmgröße, Auflösung und Tastatur Verfügbarkeit), die einem bestimmten Gerät zugeordnet sind. Sie können auch neue Geräte hinzufügen und löschen.

- Die **Android-Versions Auswahl** &ndash; die Android-Version, auf die das Layout abzielt. Der Designer wird das Layout entsprechend der ausgewählten Android-Version Rendering.

- Design **Auswahl** &ndash; das Design der Benutzeroberfläche für das Layout auswählt.

- **Konfigurations Auswahl** &ndash; wählt die Gerätekonfiguration aus, z. b. hoch *Format oder* *quer*Format.

- **Ressourcen qualifiziereroptionen** &ndash; öffnet ein Dialogfeld, das Dropdown Menüs für die Auswahl von *Sprache*, *Benutzeroberflächen Modus*, *Nachtmodus*und Optionen für den *roundscreen anzeigt* .

- **Aktionsleiste Einstellungen** &ndash; konfiguriert die Aktionsleiste Einstellungen für das Layout.

- Der Design- **Editor** &ndash; öffnet den Design- *Editor*, sodass Sie Elemente des ausgewählten Designs anpassen können.

- **Material Design Grid** &ndash; aktiviert oder deaktiviert das *Material Design Grid*. Das Dropdown Menü Element neben dem Material Design-Raster öffnet ein Dialogfeld, in dem Sie das Raster anpassen können.

Diese Features werden in den folgenden Themen ausführlicher erläutert:

- [Ressourcen Qualifizierer und Visualisierungs Optionen](~/android/user-interface/android-designer/resource-qualifiers.md) bietet ausführliche Informationen über die **Geräteauswahl**, die **Android-Versions Auswahl**, die Design **Auswahl**, die **Konfigurations Auswahl**, **Ressourcen Qualifizierer Optionen**und **Aktionsleiste Einstellungen**.

- [Alternative Layoutansichten](~/android/user-interface/android-designer/alternative-layout-views.md) erläutert, wie die **Alternative Layoutauswahl**verwendet wird.

- [Xamarin. Android Designer Material Design Features](~/android/user-interface/android-designer/material-design-features.md) bietet eine umfassende Übersicht über den Design- **Editor** und das **Material Design-Raster**.

### <a name="context-menu-commands"></a>Kontextmenü Befehle

Ein Kontextmenü ist sowohl in der **Designoberfläche** als auch in der **Dokument**Gliederung verfügbar. Dieses Menü zeigt Befehle an, die für das ausgewählte Widget und den zugehörigen Container verfügbar sind, sodass Sie Vorgänge für Container leichter ausführen können (die nicht immer einfach auf dem **Designoberfläche**ausgewählt werden können). Im folgenden finden Sie ein Beispiel für ein Kontextmenü:

[![Beispiel Kontextmenü, wenn Sie mit der rechten Maustaste auf die Designoberfläche](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

Wenn Sie in diesem Beispiel mit der rechten Maustaste auf eine `TextView` klicken, wird ein Kontextmenü geöffnet, das mehrere Optionen enthält:

- **LinearLayout** &ndash; öffnet ein Untermenü zum Bearbeiten des `LinearLayout` übergeordneten Elements des `TextView`.

- **Löschen**, **Kopieren**und **Ausschneiden** &ndash; Vorgänge, die für den `TextView` mit Rechtsklick gelten.

### <a name="zoom-controls"></a>Zoom Steuerelemente

Der **Designoberfläche** unterstützt das Zoomen über mehrere Steuerelemente, wie hier gezeigt:

[![Diagramm der Designoberfläche Zoom Steuerelemente](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

Diese Steuerelemente erleichtern es, bestimmte Bereiche der Benutzeroberfläche im Designer anzuzeigen:

- **Markieren Sie Container** , &ndash; die Container auf dem **Designoberfläche** hervorgehoben werden, damit Sie leichter zu finden sind, während Sie zoomen und verkleinern.

- **Normal Größe** &ndash; rendert das layoutpixel für Pixel, damit Sie sehen können, wie das Layout die Auflösung des ausgewählten Geräts ansieht.

- **An Fenster anpassen** &ndash; legt die Zoomstufe so fest, dass das gesamte Layout auf dem Designoberfläche sichtbar ist.

- **Vergrößern** Sie die &ndash; zoomungen inkrementell mit jedem Klick, und vergrößern Sie das Layout.

- **Vergrößern** Sie &ndash; bei jedem Klick inkrementell Zoomen, damit das Layout auf der Designoberfläche kleiner erscheint.

Beachten Sie, dass die ausgewählte Zoomeinstellung die Benutzeroberfläche der Anwendung zur Laufzeit nicht beeinträchtigt.

## <a name="switching-between-design-and-source-panes"></a>Wechseln zwischen Entwurfs-und Quell Bereichen

Im mittleren Bereich zwischen dem **Entwurfs** -und dem **Quell** Bereich gibt es mehrere Schaltflächen, mit denen die Anzeige von **Entwurfs** -und **Quell** Bereichen geändert wird:

[Bereich der Fenster Schaltflächen in![](designer-basics-images/vs/25-pane-buttons-sml.png)](designer-basics-images/vs/25-pane-buttons.png#lightbox)

Diese Schaltflächen gehen wie folgt vor:

- **Design** &ndash; diese oberste Schaltfläche, **Entwurf**, wählt den **Entwurfs** Bereich aus. Wenn Sie auf diese Schaltfläche klicken, werden die Bereiche **Toolbox** und **Eigenschaften** aktiviert, und die **Symbolleiste Text-Editor** wird nicht angezeigt. Wenn auf **die Schalt** Fläche "reduzieren" geklickt wird (siehe unten), wird der **Entwurfs** Bereich allein ohne den **Quell** Bereich dargestellt.

- Bereiche **austauschen** &ndash; diese Schaltfläche (die zwei gegensätzlichen Pfeilen ähnelt) tauscht den **Entwurfs** -und den **Quell** Bereich aus, sodass sich der **Quell** Bereich auf der linken Seite befindet und sich der **Entwurfs** Bereich auf der rechten Seite befindet. Wenn Sie erneut auf die Schaltfläche klicken, werden diese Bereiche zurück an Ihre ursprünglichen Speicherorte

- **Quelle** &ndash; diese Schaltfläche (die zwei entgegengesetzten spitzen Klammern ähnelt) wählt den **Quell** Bereich aus. Wenn Sie auf diese Schaltfläche klicken, werden die Bereiche **Toolbox** und **Eigenschaften** deaktiviert und die **Symbolleiste Text-Editor** am oberen Rand von Visual Studio angezeigt. Wenn auf die **Schaltfläche "** reduzieren" geklickt wird (siehe unten), wird durch Klicken auf die Schaltfläche " **Quelle** " anstelle des **Entwurfs** Bereichs der **Quell** Bereich angezeigt.

- **Vertikale Aufteilung** &ndash; diese Schaltfläche (die einem vertikalen Balken ähnelt) zeigt den **Entwurfs** -und den **Quell** Bereich nebeneinander an. Dies ist die Standardanordnung.

- **Horizontale Aufteilung** &ndash; diese Schaltfläche (die einem horizontalen Balken ähnelt) zeigt den **Entwurfs** Bereich oberhalb des **Quell** Bereichs an. Sie **können auf** Auslagerungs Bereiche klicken, um den **Quell** Bereich über dem **Entwurfs** Bereich zu platzieren.

- Bereich zuklappen **&ndash; diese** Schaltfläche (die zwei nach rechts zeigenden spitzen Klammern ähnelt) "reduziert" die Dual-Pane-Anzeige des **Entwurfs** und der **Quelle** in einer einzigen Ansicht eines dieser Bereiche.
    Diese Schaltfläche wird **zur Erweiterungsbereich-Schaltfläche** (ähnlich zwei nach links zeigenden spitzen Klammern), auf die geklickt werden kann, um die Ansicht wieder in den Dual-Pane-Anzeigemodus (**Entwurf** und **Quelle**) zurückzukehren.

Beim Klicken auf das **Fenster** "reduzieren" wird nur der **Entwurfs** Bereich angezeigt. Sie können jedoch auf die **Quell** Schaltfläche klicken, um stattdessen nur den **Quell** Bereich anzuzeigen. Klicken Sie erneut auf die Schaltfläche **Entwurf** , um zum **Entwurfs** Bereich zurückzukehren.

## <a name="source-pane"></a>Quellen Bereich

Der **Quell** Bereich zeigt die XML-Quelle an, die dem auf dem **Designoberfläche**gezeigten Entwurf zugrunde liegt. Da beide Sichten gleichzeitig verfügbar sind, ist es möglich, einen Entwurf der Benutzeroberfläche zu erstellen, indem Sie zwischen einer visuellen Darstellung des Entwurfs und der zugrunde liegenden XML-Quelle für das Design hin-und herwechseln:

[![Beispiel-XML-Quelle im Quellbereich](designer-basics-images/vs/22-source-pane-w158-sml.png)](designer-basics-images/vs/22-source-pane-w158.png#lightbox)

An der XML-Quelle vorgenommene Änderungen werden sofort auf der **Designoberfläche**gerendert. Änderungen am **Designoberfläche** bewirken, dass die im **Quell** Bereich angezeigte XML-Quelle entsprechend aktualisiert wird. Wenn Sie im Bereich **Quelle** Änderungen an XML vornehmen, stehen die Funktionen für die automatische Vervollständigung und IntelliSense zur Verfügung, um die XML-basierte Benutzeroberflächen Entwicklung zu beschleunigen, wie im folgenden erläutert.

Um die Navigation bei langen XML-Dateien zu vereinfachen, unterstützt der **Quell** Bereich die Bild Lauf Leiste von Visual Studio (wie auf der rechten Seite des vorherigen Screenshots gezeigt). Weitere Informationen zur Scrollleiste finden Sie unter Gewusst [wie: Verfolgen von Code durch Anpassen der Scrollleiste](https://msdn.microsoft.com/library/dn237345.aspx).

### <a name="autocompletion"></a>Automatische Vervollständigung

Wenn Sie beginnen, den Namen eines Attributs für ein Widget einzugeben, können Sie <kbd>STRG + LEERTASTE</kbd> drücken, um eine Liste der möglichen Vervollständigungen anzuzeigen. Wenn Sie z. b. im folgenden Beispiel `android:lay` eingeben (gefolgt von <kbd>STRG + LEERTASTE</kbd>), wird die folgende Liste angezeigt:

[automatische Vervollständigung des Layoutattributs![](designer-basics-images/vs/23-autocompletion-w158-sml.png)](designer-basics-images/vs/23-autocompletion-w158.png#lightbox)

Drücken <kbd>Sie die Eingabe</kbd> Taste, um den ersten aufgeführten Abschluss zu akzeptieren, oder verwenden Sie die Pfeiltasten, um zum gewünschten Abschluss zu scrollen, und drücken <kbd>Sie die Eingabe</kbd> Alternativ können Sie mit der Maus einen Bildlauf zu durchführen und auf den gewünschten Abschluss klicken.

### <a name="intellisense"></a>IntelliSense

Nachdem Sie ein neues Attribut für ein Widget eingegeben und damit begonnen haben, ihm einen Wert zuzuweisen, wird IntelliSense nach dem Eingeben eines auslöserzeichens angezeigt und stellt eine Liste gültiger Werte bereit, die für dieses Attribut verwendet werden sollen. Wenn beispielsweise das erste doppelte Anführungszeichen für `android:layout_width` im folgenden Beispiel eingegeben wird, wird eine automatische Vervollständigungs Auswahl angezeigt, um die Liste der gültigen Optionen für diese Breite bereitzustellen:

[![IntelliSense-Beispiel für Layoutbreite](designer-basics-images/vs/24-intellisense-w158-sml.png)](designer-basics-images/vs/24-intellisense-w158.png#lightbox)

Am unteren Rand dieses Popups befinden sich zwei Schaltflächen (wie in rot im obigen Screenshot dargestellt). Wenn Sie auf der linken Seite auf die Schaltfläche **Projektressourcen** klicken, wird die Liste auf Ressourcen beschränkt, die Teil des App-Projekts sind. durch Klicken auf die Schaltfläche Frameworkressourcen auf der rechten Seite wird die Liste so eingeschränkt, dass im Framework verfügbare Ressourcen
Diese Schaltflächen werden ein-oder ausgeschaltet: Sie können erneut darauf klicken, um die von den einzelnen bereitgestellten Filter Aktionen zu deaktivieren.

## <a name="properties-pane"></a>Eigenschaften Bereich

Der Designer unterstützt die Bearbeitung von widgeeigenschaften über den **Eigenschaften** Bereich: 

![Screenshot der Eigenschaftenfenster](designer-basics-images/vs/08-property-pad.png)

Die im Bereich **Eigenschaften** aufgelisteten Eigenschaften ändern sich je nachdem, welches Widget auf dem **Designoberfläche**ausgewählt ist.

### <a name="default-values"></a>Standardwerte

Die Eigenschaften der meisten Widgets sind im **Eigenschaften** Fenster leer, da ihre Werte vom ausgewählten Android-Design erben.
Im Fenster **Eigenschaften** werden nur Werte angezeigt, die für das ausgewählte Widget explizit festgelegt sind. Es werden keine Werte angezeigt, die vom Design geerbt werden.

### <a name="referencing-resources"></a>Verweisen auf Ressourcen

Einige Eigenschaften können auf Ressourcen verweisen, die in anderen Dateien als der Datei "Layout **. axml** " definiert sind. Die häufigsten Fälle dieses Typs sind `string` und `drawable` Ressourcen. Verweise können jedoch auch für andere Ressourcen verwendet werden, z. b. `Boolean` Werte und Dimensionen. Wenn eine Eigenschaft Ressourcen Verweise unterstützt, wird neben dem Text Eintrag für die Eigenschaft ein Durchsuchen-Symbol (ein quadratisches Symbol) angezeigt. Diese Schaltfläche öffnet eine Ressourcen Auswahl, wenn darauf geklickt wird.

Der folgende Screenshot zeigt z. b. die verfügbaren Optionen, wenn Sie im Fenster **Eigenschaften** auf das `Text` Feld mit dem Textfeld rechts neben dem Textfeld klicken:

[![Beispielliste von Textoptionen](designer-basics-images/vs/09-text-options-sml.png)](designer-basics-images/vs/09-text-options.png#lightbox)

Wenn auf " **Ressource...** " geklickt wird, wird das Dialogfeld " **Ressource auswählen** " angezeigt:

[Screenshot der![Beispiel Ressourcen mit mehreren aufgelisteten Ressourcen](designer-basics-images/vs/09b-resources-w158-sml.png)](designer-basics-images/vs/09b-resources-w158.png#lightbox)

Aus dieser Liste können Sie eine Text Ressource auswählen, die für dieses Widget verwendet werden soll, anstatt den Text im Bereich " **Eigenschaften** " hart zu codieren. Im nächsten Beispiel wird die Ressourcen Auswahl für die `Src`-Eigenschaft eines `ImageView` veranschaulicht:

[![Ressourcen Auswahllisten-Symbol Ressource für eine ImageView](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

Wenn Sie auf das leere Quadrat rechts neben der Eigenschaft `Src` klicken, wird das Dialogfeld **Ressource auswählen** mit einer Liste von Ressourcen geöffnet, die von den Farben (wie oben gezeigt) bis zu drawables reichen.

### <a name="boolean-property-references"></a>Boolesche Eigenschafts Verweise

*Boolesche* Eigenschaften werden normalerweise als Häkchen neben einer Eigenschaft im Eigenschaftenfenster ausgewählt. Sie können einen `true` oder `false` Wert festlegen, indem Sie dieses Kontrollkästchen aktivieren oder deaktivieren, oder Sie können einen Eigenschafts Verweis auswählen, indem Sie auf das dunkel gefüllte Quadrat rechts neben der Eigenschaft klicken. Im folgenden Beispiel wird Text in alle Caps geändert, indem Sie auf den booleschen Eigenschafts Verweis **Text alle Caps** zeigt, der dem ausgewählten `TextView` zugeordnet ist:

![Beispiel für das Festlegen von booleschen Eigenschaften](designer-basics-images/vs/11-boolean.png)

## <a name="editing-properties-inline"></a>Inline Bearbeitung von Eigenschaften

Der Android Designer unterstützt das direkte Bearbeiten bestimmter Eigenschaften auf der **Designoberfläche** (Sie müssen daher nicht in der Eigenschaften Liste nach diesen Eigenschaften suchen). Eigenschaften, die direkt bearbeitet werden können, sind Text, Margin und Size.

### <a name="text"></a>Text

Die Texteigenschaften einiger widgets (z. b. `Button` und `TextView`) können direkt auf dem **Designoberfläche**bearbeitet werden. Wenn Sie auf ein Widget doppelklicken, wird es in den Bearbeitungsmodus versetzt, wie unten dargestellt:

![Text Ressource für die Hello-Zeichenfolge](designer-basics-images/vs/12-text-resource.png "Text Ressource")

Sie können einen neuen Textwert eingeben, oder Sie können eine neue Ressourcen Zeichenfolge eingeben. Im folgenden Beispiel wird die `@string/hello` Ressource durch den Text ersetzt, `CLICK THIS BUTTON`:

![UMSCHALT + Eingabe, um Text automatisch mit einer neuen Ressource zu verknüpfen](designer-basics-images/vs/13-shift-enter-resource.png)

Diese Änderung wird in der `text`-Eigenschaft des Widgets gespeichert. der Wert, der der `@string/hello` Ressource zugewiesen ist, wird nicht geändert.
Wenn Sie in einer neuen Text Zeichenfolge eine Taste drücken, können Sie <kbd>UMSCHALT</kbd>  +
<kbd>Eingabe</kbd> Taste drücken, um den eingegebenen Text automatisch mit einer neuen Ressource zu verknüpfen.

### <a name="margin"></a>Rand

Wenn Sie ein Widget auswählen, zeigt der Designer Handles an, die es Ihnen ermöglichen, die Größe oder den Rand des Widgets interaktiv zu ändern. Wenn Sie auf das Widget klicken, während es ausgewählt ist, wechseln Sie in den Modus für die Rand Bearbeitung und den Größen Bearbeitungsmodus.

Wenn Sie zum ersten Mal auf ein Widget klicken, werden Rand Zieh Punkte angezeigt. Wenn Sie die Maus zu einem der Handles bewegen, zeigt der Designer die Eigenschaft an, die das Handle ändert (wie unten für die `layout_marginLeft`-Eigenschaft gezeigt):

![Screenshot, der Rand Handles im Designer anzeigt](designer-basics-images/vs/15-margin-handles.png)

Wenn bereits ein Rand festgelegt wurde, werden gepunktete Linien angezeigt, die den Platz angeben, den der Rand einnimmt:

![Beispiel für gepunktete Linien, die Platz um eine Schaltfläche markieren](designer-basics-images/vs/16-margins-set.png)

### <a name="size"></a>Größe

Wie bereits erwähnt, können Sie in den Größen Bearbeitungsmodus wechseln, indem Sie auf ein Widget klicken, während es bereits ausgewählt ist. Klicken Sie auf das dreieckige handle, um die Größe für die festgelegte Dimension auf `wrap_content`festzulegen:

![Packen von Inhalten und Ändern der Größe von Handles](designer-basics-images/vs/17-wrap-content.png)

Durch Klicken auf den Umbruch **Inhalts** Handle wird das Widget in dieser Dimension verkleinert, sodass es nicht größer als notwendig ist, um den eingeschlossenen Inhalt zu umschließen. In diesem Beispiel verkleinert sich der Schaltflächen Text horizontal, wie im folgenden Screenshot gezeigt.

Wenn der Size-Wert auf **Inhalt**umschließen festgelegt ist, zeigt der Designer einen dreieckigen Handle an, der in der entgegengesetzten Richtung zum Ändern der Größe in `match_parent`zeigt:

![Übergeordnetes handle vergleichen](designer-basics-images/vs/18-match-parent.png)

Durch Klicken auf das über **geordnete** Übereinstimmungs Handle wird die Größe in dieser Dimension wieder hergestellt, sodass Sie mit dem übergeordneten Widget identisch ist.

Außerdem können Sie den Zirkel Größen Zieh Punkt (wie in den obigen Screenshots gezeigt) ziehen, um die Größe des Widgets an einen beliebigen `dp` Wert zu ändern. Wenn Sie dies tun, werden sowohl **Inhalt** einschließen als auch über **geordnete Handles vergleichen** für diese Dimension angezeigt:

![Zirkuläre Größen Zieh Punkte](designer-basics-images/vs/19-resize-dp.png)

Nicht alle Container ermöglichen das Bearbeiten der `Size` eines Widgets. Beachten Sie z. b., dass im folgenden Screenshot mit dem ausgewählten `LinearLayout` die Handles zur Größenänderung nicht angezeigt werden:

![Keine Handles zur Größenänderung](designer-basics-images/vs/20-no-resize-handles.png)

## <a name="document-outline"></a>Dokumentgliederung

Die **Dokument** Gliederung zeigt die widgehierarchie des Layouts an.
Im folgenden Beispiel wird das enthaltende `LinearLayout` Widget ausgewählt:

![Dokument Gliederungs Beispiel](designer-basics-images/vs/21-document-outline.png)

Die Gliederung des ausgewählten Widgets (in diesem Fall einer `LinearLayout`) wird auch auf der **Designoberfläche**hervorgehoben. Das ausgewählte Widget in der Dokument Gliederung bleibt mit seinem Pendant auf dem **Designoberfläche**synchron. Dies ist hilfreich bei der Auswahl von Ansichts Gruppen, die auf dem **Designoberfläche**nicht immer einfach ausgewählt werden können.

Die **Dokument** Gliederung unterstützt das Kopieren und einfügen, oder Sie können Drag & Drop verwenden. Drag & Drop wird von der **Dokument** Gliederung zum **Designoberfläche** sowie vom **Designoberfläche** bis zur **Dokument**Gliederung unterstützt. Wenn Sie mit der rechten Maustaste auf ein Element in der **Dokument** Gliederung klicken, wird auch das Kontextmenü für das Element angezeigt (das gleiche Kontextmenü, das angezeigt wird, wenn Sie im **Designoberfläche**mit der rechten Maustaste auf dasselbe Widget klicken).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="launching-the-designer"></a>Starten des Designers

Der Designer wird automatisch gestartet, wenn ein Layout erstellt wird, oder er kann durch Doppelklicken auf eine vorhandene axml-Datei gestartet werden. Wenn Sie z. b. im Ordner **Ressourcen > layoutordner** auf **Main. axml** doppelklicken, wird der Designer wie unten dargestellt geladen:

[![-Designer-Bildschirm in Visual Studio für Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

Ebenso können Sie ein neues Layout hinzufügen, indem Sie im **Lösungspad** mit der rechten Maustaste auf den **layoutordner** klicken und **> neue Datei hinzufügen > Android-> Layout**auswählen:

[Dialog!["neue Datei hinzufügen"](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

Dadurch wird eine neue axml-Datei erstellt und auf die Designoberfläche geladen.

> [!TIP]
> Neuere Releases von Visual Studio unterstützen das Öffnen von XML-Dateien in Android Designer.
>
> Sowohl AXML- als auch XML-Dateien werden in Android Designer unterstützt.

## <a name="designer-features"></a>Designer Features

Der Designer besteht aus mehreren Abschnitten, die die verschiedenen Features unterstützen, wie im folgenden Screenshot zu sehen:

[![Diagramm der Designer Bereiche](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

Wenn Sie ein Layout im Designer bearbeiten, verwenden Sie die folgenden Funktionen, um den Entwurf zu erstellen und zu strukturieren:

- **Designoberfläche** &ndash; ermöglicht die visuelle Erstellung der Benutzeroberfläche, indem Sie eine bearbeitbare Darstellung der Darstellung des Layouts auf dem Gerät erhalten.

- In der **Symbolleiste** &ndash; eine Liste der Selektoren angezeigt: **Geräte**-, **Versions** **-, Design**-, Layoutkonfiguration und Aktionsleiste Einstellungen. Die Symbolleiste enthält auch Symbole zum Starten des Design-Editors und zum Aktivieren des Material Design-Rasters.

- **Toolbox** &ndash; enthält eine Liste von Widgets und Layouts, die Sie per Drag & amp; Drop auf die Designoberfläche verschieben können.

- Eigenschaften- **Pad** &ndash; listet die Eigenschaften des ausgewählten Widgets zum Anzeigen und Bearbeiten auf.

- **Dokument** Gliederung &ndash; zeigt die Struktur der Widgets an, aus denen das Layout besteht. Sie können auf ein Element in der Struktur klicken, damit es im Designer ausgewählt wird. Wenn Sie auf ein Element in der Struktur klicken, werden auch die Eigenschaften des Elements in das Eigenschafts Pad geladen.

## <a name="toolbar"></a>ToolBar

Die Symbolleiste (oberhalb der Designoberfläche) zeigt konfigurationsselektoren und Tool Menüs an:

[![Diagramm der Designer-Symbolleiste](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

Die Symbolleiste ermöglicht den Zugriff auf die folgenden Features:

- **Alternativer layoutselektor** &ndash; ermöglicht Ihnen die Auswahl aus unterschiedlichen Layoutversionen.

- Die **Geräteauswahl** &ndash; definiert eine Gruppe von Qualifizierern, die einem bestimmten Gerät zugeordnet sind, z. b. Bildschirmgröße, Auflösung und Tastatur Verfügbarkeit. Sie können auch neue Geräte hinzufügen und löschen.

- Die **Android-Versions Auswahl** &ndash; die Android-Version, auf die das Layout abzielt. Der Designer wird das Layout entsprechend der ausgewählten Android-Version Rendering.

- Design **Auswahl** &ndash; das Design der Benutzeroberfläche für das Layout auswählt.

- **Konfigurations Auswahl** &ndash; wählt die Gerätekonfiguration aus, z. b. hoch *Format oder* *quer*Format.

- **Ressourcen qualifiziereroptionen** &ndash; öffnet ein Dialogfeld, das Dropdown Menüs für die Auswahl von *Sprache*, *Benutzeroberflächen Modus*, *Nachtmodus*und Optionen für den *roundscreen anzeigt* .

- **Aktionsleiste Einstellungen** &ndash; konfiguriert die Aktionsleiste Einstellungen für das Layout.

- Der Design- **Editor** &ndash; öffnet den Design- *Editor*, sodass Sie Elemente des ausgewählten Designs anpassen können.

- **Material Design Grid** &ndash; aktiviert oder deaktiviert das *Material Design Grid*. Das Dropdown Menü Element neben dem Material Design-Raster öffnet ein Dialogfeld, in dem Sie das Raster anpassen können.

Diese Features werden in den folgenden Themen ausführlicher erläutert:

[Ressourcen Qualifizierer und Visualisierungs Optionen](~/android/user-interface/android-designer/resource-qualifiers.md) bietet ausführliche Informationen über die **Geräteauswahl**, die **Android-Versions Auswahl**, die Design **Auswahl**, die **Konfigurations Auswahl**, **Ressourcen Qualifizierer Optionen**und **Aktionsleiste Einstellungen**.

[Alternative Layoutansichten](~/android/user-interface/android-designer/alternative-layout-views.md) erläutert, wie die **Alternative Layoutauswahl**verwendet wird.

[Material Design Features](~/android/user-interface/android-designer/material-design-features.md) bietet eine umfassende Übersicht über den Design- **Editor** und das **Material Design-Raster**.

## <a name="design-surface"></a>Entwurfsoberfläche

Der Designer ermöglicht das ziehen und Ablegen von Widgets aus der Toolbox auf die Designoberfläche. Wenn Sie im Designer mit Widgets interagieren (indem Sie entweder neue Widgets hinzufügen oder vorhandene neu positionieren), werden vertikale und horizontale Linien angezeigt, um die verfügbaren Einfügepunkte zu markieren. Im folgenden Beispiel wird ein neues `Button`-Widget in den Designoberfläche gezogen:

[![Beispiel einfügelinien in Designoberfläche](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

Außerdem können Widgets kopiert werden: Sie können das Widget mithilfe von kopieren und Einfügen kopieren, oder Sie können ein vorhandenes Widget per Drag & amp; Drop beim Drücken der <kbd>STRG</kbd> -Taste ablegen.

### <a name="context-menu-commands"></a>Kontextmenü Befehle

Ein Kontextmenü ist sowohl in der Designoberfläche als auch in der Dokument Gliederung verfügbar. Dieses Menü zeigt Befehle an, die für das ausgewählte Widget und den zugehörigen Container verfügbar sind, sodass Sie Vorgänge für Container leichter ausführen können (die nicht immer einfach auf dem Designoberfläche ausgewählt werden können). Im folgenden finden Sie ein Beispiel für ein Kontextmenü:

[![Beispiel Kontextmenü, wenn Sie mit der rechten Maustaste auf die Designoberfläche](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

Wenn Sie in diesem Beispiel mit der rechten Maustaste auf eine `Button` klicken, wird ein Kontextmenü geöffnet, das mehrere Optionen enthält:

- **LinearLayout** &ndash; öffnet ein Untermenü zum Bearbeiten des `LinearLayout` übergeordneten Elements des `Button`.

- **Ausschneide**-, **Kopier**-und **Lösch** Vorgänge &ndash; Vorgänge, die für den Rechtsklick `Button` gelten.

### <a name="zoom-controls"></a>Zoom Steuerelemente

Der Designoberfläche unterstützt das Zoomen über mehrere Steuerelemente, wie hier gezeigt:

[![Diagramm der Designoberfläche Zoom Steuerelemente](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

Diese Steuerelemente erleichtern es, bestimmte Bereiche der Benutzeroberfläche im Designer anzuzeigen:

- **Markieren Sie Container** , &ndash; die Container auf dem Designoberfläche hervorgehoben werden, damit Sie leichter zu finden sind, während Sie zoomen und verkleinern.

- **Normal Größe** &ndash; rendert das layoutpixel für Pixel, damit Sie sehen können, wie das Layout die Auflösung des ausgewählten Geräts ansieht.

- **An Fenster anpassen** &ndash; legt die Zoomstufe so fest, dass das gesamte Layout auf dem Designoberfläche sichtbar ist.

- **Vergrößern** Sie die &ndash; zoomungen inkrementell mit jedem Klick, und vergrößern Sie das Layout.

- **Vergrößern** Sie &ndash; bei jedem Klick inkrementell Zoomen, damit das Layout auf der Designoberfläche kleiner erscheint.

Beachten Sie, dass die ausgewählte Zoomeinstellung die Benutzeroberfläche der Anwendung zur Laufzeit nicht beeinträchtigt.

## <a name="property-pad"></a>Eigenschaftenpad

Der Designer unterstützt die Bearbeitung von widgeeigenschaften über das **eigenschaftenpad**. Die im eigenschaftenpad aufgelisteten Eigenschaften ändern sich je nach ausgewähltem Widget in der Designer Oberfläche. Wenn die `Button` im vorherigen Beispiel ausgewählt ist, werden die Eigenschaften für dieses `Button` Widget angezeigt:

[![Screenshot des Eigenschaften Pads](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

## <a name="property-pad-sections"></a>Eigenschaften-Pad-Abschnitte

Das eigenschaftenpad ist in verschiedene Abschnitte unterteilt, in denen ähnliche Eigenschaften gruppiert werden &ndash; dadurch wird das Auffinden der gewünschten Eigenschaften vereinfacht:

- Das **Widget** &ndash; Haupteigenschaften des Widgets, z. b. `id`, `visibility`, `text` usw. Die Eigenschaften zum Verwalten des widgeinhalts werden in der Regel hier abgelegt.

- **Stil** &ndash; Eigenschaften, die die visuelle Darstellung des Widgets ändern, z. b. `font`, `text color`, `background` usw.

- **Layout** &ndash; Eigenschaften, mit denen die Position und die Größe des Widgets festgelegt werden.

- **Scrollen** Sie &ndash; scrolleigenschaften.

- Das **Verhalten** &ndash; Flags, die festlegen, wie das Widget verhält.

### <a name="default-values"></a>Standardwerte

Die Eigenschaften der meisten Widgets sind im **eigenschaftenpad** leer, da ihre Werte vom ausgewählten Android-Design erben. Im **eigenschaftenpad** werden nur Werte angezeigt, die für das ausgewählte Widget explizit festgelegt sind. Es werden keine Werte angezeigt, die vom Design geerbt werden.

### <a name="referencing-resources"></a>Verweisen auf Ressourcen

Einige Eigenschaften können auf Ressourcen verweisen, die in anderen Dateien als der Datei "Layout **. axml** " definiert sind. Die häufigsten Fälle dieses Typs sind `string` und `drawable` Ressourcen. Verweise können jedoch auch für andere Ressourcen verwendet werden, z. b. `Boolean` Werte und Dimensionen.
Wenn eine Eigenschaft Ressourcen Verweise unterstützt, wird neben dem Text Eintrag für die Eigenschaft ein Durchsuchen-Symbol (Auslassungs Punkte, &hellip;) angezeigt.
Wenn Sie auf diese Schaltfläche klicken, wird eine Ressourcen Auswahl geöffnet.

Der folgende Screenshot zeigt z. b. die Ressourcen, die verfügbar sind, wenn Sie auf die Auslassungs Punkte rechts neben dem Textfeld für ein `Button`-Widget im **eigenschaftenpad**klicken:

[Screenshot der![Beispiel Ressourcen mit zwei aufgeführten Ressourcen](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

Im nächsten Beispiel wird die Ressourcen Auswahl für die `Src`-Eigenschaft eines `ImageView` veranschaulicht:

[![Ressourcen Auswahllisten-Symbol Ressource für eine ImageView](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

### <a name="boolean-property-references"></a>Boolesche Eigenschafts Verweise

*Boolesche* Eigenschaften werden normalerweise als Kontrollkästchen im eigenschaftenpad angezeigt. Wenn eine `Boolean`-Eigenschaft Ressourcen Verweise unterstützt, wird ein kleines Kontrollkästchen neben der-Eigenschaft angezeigt. Ein aktiviertes Kontrollkästchen bedeutet `true` und ein leeres Feld bedeutet `false`. Sie können auch direkt einen Wert eingeben, z. b. `true` oder `false`. Wenn Sie mit der Maus auf die Eingabe zeigen, wird ein kleines Textfeld Symbol angezeigt. Sie können darauf klicken, wenn Sie den Wert manuell eingeben möchten.

[![Beispiel für das Festlegen von booleschen Eigenschaften](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)

## <a name="grouped-properties"></a>Gruppierte Eigenschaften

Einige Widgets verfügen über mehrwertige Eigenschaften, die gruppiert werden (z. b. `Padding`). Diese Eigenschaftswerte werden im **eigenschaftenpad** in einer einzelnen, erweiterbaren Zeile aufgelistet. Einige dieser Eigenschaften können direkt in der gruppierten Zeile bearbeitet werden, wie z. b. der unten gezeigten `Padding`-Eigenschaft:

[![Beispiel Einstellungen für die Padding-Eigenschaft](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

## <a name="editing-properties-inline"></a>Inline Bearbeitung von Eigenschaften

Der Android Designer unterstützt das direkte Bearbeiten bestimmter Eigenschaften auf der Designoberfläche (Sie müssen daher nicht in der Eigenschaften Liste nach diesen Eigenschaften suchen). Eigenschaften, die direkt bearbeitet werden können, sind Text, Margin und Size.

### <a name="text"></a>Text

Die Texteigenschaften einiger widgets (z. b. `Button` und `TextView`) können direkt auf dem Designoberfläche bearbeitet werden. Wenn Sie auf ein Widget doppelklicken, wird es in den Bearbeitungsmodus versetzt, wie unten dargestellt:

[![Text Ressource für die "Hello"-Zeichenfolge](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

Sie können einen neuen Textwert eingeben, oder Sie können eine neue Ressourcen Zeichenfolge eingeben. Im folgenden Beispiel wird die `@string/hello` Ressource durch den Text ersetzt, `CLICK THIS BUTTON`:

[![UMSCHALT + EINGABETASTE, um Text automatisch mit einer neuen Ressource zu verknüpfen.](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

Diese Änderung wird in der `text`-Eigenschaft des Widgets gespeichert. der Wert, der der `@string/hello` Ressource zugewiesen ist, wird nicht geändert.
Wenn Sie in einer neuen Text Zeichenfolge eine Taste drücken, können Sie <kbd>UMSCHALT</kbd>  +
<kbd>Eingabe</kbd> Taste drücken, um den eingegebenen Text automatisch mit einer neuen Ressource zu verknüpfen.

### <a name="margin"></a>Rand

Wenn Sie ein Widget auswählen, zeigt der Designer Handles an, die es Ihnen ermöglichen, die Größe oder den Rand des Widgets interaktiv zu ändern. Wenn Sie auf das Widget klicken, während es ausgewählt ist, wechseln Sie in den Modus für die Rand Bearbeitung und den Größen Bearbeitungsmodus.

Wenn Sie zum ersten Mal auf ein Widget klicken, werden Rand Zieh Punkte angezeigt. Wenn Sie die Maus zu einem der Handles bewegen, zeigt der Designer die Eigenschaft an, die das Handle ändert (wie unten für die `layout_marginLeft`-Eigenschaft gezeigt):

[![Screenshot, der Rand Handles im Designer anzeigt](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

Wenn bereits ein Rand festgelegt wurde, werden gepunktete Linien angezeigt, die den Platz angeben, den der Rand einnimmt:

[![Beispiel für gepunktete Linien, die Platz um eine Schaltfläche markieren](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

### <a name="size"></a>Größe

Wie bereits erwähnt, können Sie in den Größen Bearbeitungsmodus wechseln, indem Sie auf ein Widget klicken, während es bereits ausgewählt ist. Klicken Sie auf das dreieckige handle, um die Größe für die festgelegte Dimension auf `wrap_content`festzulegen:

[![Wrap Inhalt und Handles zur Größenänderung](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

Wenn Sie auf das **Wrap-Inhalts** Handle klicken, wird das Widget in dieser Dimension verkleinert, sodass nicht größer als notwendig ist, um den eingeschlossenen Inhalt zu umschließen. In diesem Beispiel verkleinert sich der Schaltflächen Text horizontal, wie im folgenden Screenshot gezeigt.

Wenn der Size-Wert auf **Inhalt**umschließen festgelegt ist, zeigt der Designer einen dreieckigen Handle an, der in der entgegengesetzten Richtung zum Ändern der Größe in `match_parent`zeigt:

[übergeordnetes handle![Übereinstimmung](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

Durch Klicken auf das über **geordnete** Übereinstimmungs Handle wird die Größe in dieser Dimension wieder hergestellt, sodass Sie mit dem übergeordneten Widget identisch ist.

Außerdem können Sie den Zirkel Größen Zieh Punkt (wie in den obigen Screenshots gezeigt) ziehen, um die Größe des Widgets an einen beliebigen `dp` Wert zu ändern. Wenn Sie dies tun, werden sowohl **Inhalt** einschließen als auch über **geordnete Handles vergleichen** für diese Dimension angezeigt:

[![-Handles für Zirkel Größenänderung](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

Nicht alle Container ermöglichen das Bearbeiten der `Size` eines Widgets. Beachten Sie z. b., dass im folgenden Screenshot mit dem ausgewählten `LinearLayout` die Handles zur Größenänderung nicht angezeigt werden:

[![keine Handles zur Größenänderung](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

## <a name="document-outline"></a>Dokumentgliederung

Die **Dokument** Gliederung zeigt die widgehierarchie des Layouts an.
Im folgenden Beispiel wird das enthaltende `LinearLayout` Widget ausgewählt:

[Dokument Gliederung![](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

Die Gliederung des ausgewählten Widgets (in diesem Fall einer `LinearLayout`) wird auch auf der Designoberfläche hervorgehoben. Das ausgewählte Widget in der Dokument Gliederung bleibt mit seinem Pendant auf dem Designoberfläche synchron. Dies ist hilfreich bei der Auswahl von Ansichts Gruppen, die auf dem Designoberfläche nicht immer einfach ausgewählt werden können.

Die Dokument Gliederung unterstützt das Kopieren und einfügen, oder Sie können Drag & Drop verwenden. Drag & Drop wird von der Dokument Gliederung zum Designoberfläche sowie vom Designoberfläche bis zur Dokument Gliederung unterstützt. Wenn Sie mit der rechten Maustaste auf ein Element in der Dokument Gliederung klicken, wird auch das Kontextmenü für das Element angezeigt (das gleiche Kontextmenü, das angezeigt wird, wenn Sie im Designoberfläche mit der rechten Maustaste auf dasselbe Widget klicken).

-----
