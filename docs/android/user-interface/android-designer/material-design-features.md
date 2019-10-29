---
title: Xamarin. Android Designer Material Design Features
description: In diesem Thema werden Designer Features beschrieben, die Entwicklern das Erstellen von Material Design-kompatiblen Layouts erleichtern. In diesem Abschnitt wird erläutert, wie das Material Raster, die Material Farb Palette, die typografische Skala und der Design-Editor verwendet werden.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 43397fb855bdf872cf17b315044f34a468c22d00
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029454"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Xamarin. Android Designer Material Design Features

_In diesem Thema werden Designer Features beschrieben, die Entwicklern das Erstellen von Material Design-kompatiblen Layouts erleichtern. In diesem Abschnitt wird erläutert, wie das Material Raster, die Material Farb Palette, die typografische Skala und der Design-Editor verwendet werden._

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Weiterentwickeln 2016: jeder kann mit Material Design tolle Apps erstellen**

## <a name="overview"></a>Übersicht

Die xamarin. Android Designer enthält Funktionen, die Ihnen das Erstellen von Material-Design-kompatiblen Layouts erleichtern. Wenn Sie mit dem Material Entwurf nicht vertraut sind, finden Sie weitere Informationen unter [Einführung in Material Design](https://material.io/design/introduction).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

In diesem Handbuch werden die folgenden Designer Features erläutert:

- *Material Grid* &ndash; eine Überlagerung auf der Designoberfläche, die ein Raster, einen Abstand und keylines anzeigt, die Ihnen helfen, layoutgadgets gemäß den Material Design-Richtlinien zu platzieren.

- Der Design- *Editor* &ndash; ein kleiner Farb Ressourcen-Editor, mit dem Sie Farbinformationen für eine Teilmenge eines Designs festlegen können. Sie können z. b. Material Farben (z. b. `colorPrimary`, `colorPrimaryDark`und `colorAccent`anzeigen und ändern.

Wir sehen uns die einzelnen Features an und geben Beispiele dafür an, wie Sie verwendet werden können.

## <a name="material-design-grid"></a>Material Entwurfs Raster

Das Menü Material Design Grid ist über die Symbolleiste am oberen Rand des Designers verfügbar:

[Entwurfs Raster für![Material](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

Wenn Sie auf das Material Design Grid-Symbol klicken, zeigt der Designer eine Überlagerung auf der Designoberfläche an, die die folgenden Elemente enthält:

- Keylines (orangefarbene Linien)

- Abstand (grüne Bereiche)

- Ein Raster (blaue Linien)

Diese Elemente sind im vorherigen Screenshot zu sehen. Jedes dieser Überlagerungs Elemente ist konfigurierbar. Wenn Sie auf das Auslassungs Zeichen neben dem Raster Menü Material Design klicken, wird ein Dialogfeld popover geöffnet, das Ihnen das Deaktivieren/Aktivieren des Rasters, das Konfigurieren der Platzierung von Tastatur Zeilen und das Festlegen von Abstände ermöglicht. Beachten Sie, dass alle Werte `dp` (Dichte unabhängige Pixel) ausgedrückt werden:

[![Raster-, keyline-und Abstands Konfiguration](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

Um eine neue keyline hinzuzufügen, geben Sie einen neuen Offset Wert in das Feld **Offset** ein, wählen Sie einen Speicherort (**Links**, **oben**, **Rechts**oder **unten**) aus, und klicken Sie auf das Symbol +, um die neue keyline hinzuzufügen. Wenn Sie einen neuen Abstand hinzufügen möchten, geben Sie die Größe und den Offset (in DP) in die **Größen** -bzw. **Offset** Felder ein. Wählen Sie einen Speicherort (**Links**, **oben**, **Rechts**oder **unten**) aus, und klicken Sie auf das Symbol "+", um den neuen Abstand hinzuzufügen.

Wenn Sie diese Konfigurationswerte ändern, werden Sie in der XML-Layoutdatei gespeichert und wieder verwendet, wenn Sie das Layout erneut öffnen.

## <a name="theme-editor"></a>Design-Editor

Mit dem Design- **Editor** können Sie Farbinformationen für eine Teilmenge von Design Attributen anpassen. Klicken Sie zum Öffnen des Design- **Editors**auf das Pinselsymbol auf der Symbolleiste:

[Symbol "![Design-Editor"](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

Obwohl der Design- **Editor** über die Symbolleiste für alle Android-Ziel Versionen und API-Ebenen zugänglich ist, ist nur eine Teilmenge der unten beschriebenen Funktionen verfügbar, wenn die Ziel-API-Ebene älter als API 21 (Android 5,0 Lollipop) ist.

Im linken Bereich des Design- **Editors** wird die Liste der Farben angezeigt, aus denen das aktuell ausgewählte Design besteht (in diesem Beispiel verwenden wir das `Default Theme`):

[Design-Editor für![](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

Wenn Sie eine Farbe auf der linken Seite auswählen, werden im rechten Bereich die folgenden Registerkarten angezeigt, die Ihnen bei der Bearbeitung dieser Farbe helfen:

- **Erben** &ndash; zeigt ein diagrammvererbungs Diagramm für die ausgewählte Farbe an und listet den aufgelösten Farb-und Farbcode auf, der der Design Farbe zugewiesen ist.

- Mit der **Farb** Auswahl &ndash; können Sie die ausgewählte Farbe in einen beliebigen Wert ändern.

- Mithilfe der **Material Palette** &ndash; können Sie die ausgewählte Farbe in einen Wert ändern, der dem Material Entwurf entspricht.

- Mithilfe von **Ressourcen** &ndash; können Sie die ausgewählte Farbe in eine der anderen vorhandenen Farb Ressourcen im Design ändern.

Sehen wir uns diese Registerkarten ausführlich an.

### <a name="inherit-tab"></a>Registerkarte erben

Wie im folgenden Beispiel gezeigt, listet die Registerkarte **erben** die Stil Vererbung für die **Hintergrund** Farbe des **Standard**Designs auf:

[![Registerkarte erben](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

In diesem Beispiel erbt das **Standard** Design von einem Stil, der `@color/background_material_light` verwendet, aber überschreibt ihn mit `color/material_grey_50`, der über einen Farbcodewert von `#fffafafa`verfügt.
Weitere Informationen zur Stil Vererbung finden Sie unter [Stile und](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)Designs.

### <a name="color-picker"></a>Farbauswahl

Der folgende Screenshot veranschaulicht die **Farb**Auswahl:

[![Farbauswahl](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

In diesem Beispiel kann die **Hintergrund** Farbe auf verschiedene Weise in einen beliebigen Wert geändert werden:

- Direktes klicken auf eine Farbe.
- Eingeben von Farbton-, Sättigungs-und Helligkeits Werten.
- Eingeben von RGB-Werten (rot, grün, blau) in Dezimalzahlen.
- Das Alpha (Deckkraft) für die ausgewählte Farbe wird festgelegt.
- Direktes Eingeben des hexadezimalen Farbcodes.

Die Farbe, die Sie in der Farbauswahl auswählen, ist *nicht* auf die Richtlinien für den Material Entwurf oder auf die Menge der verfügbaren Farb Ressourcen beschränkt.

### <a name="resources"></a>Ressourcen

Die Registerkarte **Ressourcen** enthält eine Liste mit Farb Ressourcen, die im Design bereits vorhanden sind:

[![Ressourcen](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

Mithilfe der Registerkarte **Ressourcen** können Sie Ihre Auswahl auf diese Liste von Farben beschränken. Beachten Sie, dass bei Auswahl einer Farb Ressource, die bereits einem anderen Teil des Designs zugewiesen ist, zwei angrenzende Elemente der Benutzeroberfläche möglicherweise zusammengeführt werden (da Sie dieselbe Farbe aufweisen) und der Benutzer schwer zu unterscheiden ist.

### <a name="material-palette"></a>Material Palette

Die Registerkarte **Materialpalette** öffnet die **Farbpalette Material Design**. Durch die Auswahl eines Farbwerts aus dieser Palette wird die gewünschte Farbe eingeschränkt, damit Sie mit den Material Design-Richtlinien konsistent ist:

[![Material Palette](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

Am oberen Rand der Farbpalette werden die Entwurfs Farben für das primäre Material angezeigt, während der untere Teil der Palette einen Bereich von Farben für die ausgewählte Primärfarbe anzeigt. Wenn Sie z. b. **Indigo**auswählen, wird am unteren Rand des Dialog Felds eine Auflistung von **Indigo** -Farben angezeigt.
Wenn Sie einen Farbton auswählen, wird die Farbe der Eigenschaft in den ausgewählten Farbton geändert. Im folgenden Beispiel wird der `Background Tint` der Schaltfläche in *Indigo 500*geändert:

![Auswählen von Indigo 500](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint` ist auf den Farbcode für *Indigo 500* (`#ff3f51b5`) festgelegt, und der Designer aktualisiert die Hintergrundfarbe, um diese Änderung widerzuspiegeln:

[![Hintergrund-Tönungs geändert](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

Weitere Informationen zur Material Design-Farbpalette finden Sie im Leitfaden Material Design [Color Palette](https://material.io/design/color/).

### <a name="creating-a-new-theme"></a>Erstellen eines neuen Designs

Im folgenden Beispiel wird die Material Palette verwendet, um ein neues benutzerdefiniertes Design zu erstellen. Zuerst ändern wir die **Hintergrund** Farbe in *blau 900*:

![Hintergrund in blau 900 ändern](material-design-features-images/vs/12-change-background-to-blue-w158.png)

Wenn eine Farb Ressource geändert wird, wird eine Meldung mit der Meldung angezeigt, *das aktuelle Design weist ungespeicherte Änderungen*auf:

[Warnung![nicht gespeicherten Änderungen](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

Die **Hintergrund** Farbe im Designer wurde in die neue Farbauswahl geändert, diese Änderung wurde jedoch noch nicht gespeichert. An diesem Punkt können Sie einen der folgenden Schritte ausführen:

- Klicken Sie auf **Änderungen verwerfen** , um die neue Farbauswahl (oder Auswahl) zu verwerfen und das Design in seinen ursprünglichen Zustand zurückzusetzen.

- Drücken Sie <kbd>STRG + S</kbd> , um die Änderungen am aktuellen Design zu speichern.

Im folgenden Beispiel wurde <kbd>STRG + S</kbd> gedrückt, sodass die Änderungen in **apptheme**gespeichert wurden:

[in apptheme gespeicherte![Änderungen](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Thema wurden die in xamarin. Android Designer verfügbaren Material Design Features beschrieben. Es wurde erläutert, wie Sie das Material Design Grid aktivieren und konfigurieren. Außerdem wurde erläutert, wie Sie mit dem Design-Editor neue benutzerdefinierte Designs erstellen, die den Material Design-Richtlinien entsprechen.
Weitere Informationen zur xamarin. Android-Unterstützung für Material Design finden Sie unter [Material](~/android/user-interface/material-theme.md)Design.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

In diesem Handbuch sehen wir uns die folgenden Designer Features an:

- *Material Design Grid* &ndash; ein Overlay auf der Designoberfläche, das ein Raster, einen Abstand und Tastatur Linien anzeigt, mit denen Sie layoutgadgets gemäß den Richtlinien für den Material Entwurf platzieren können.

- Die *Farb Palette der Material Entwürfe* &ndash; einem Eigenschaften Auffüll Dialogfeld, das Sie bei der Auswahl einer Farbe aus der offiziellen Material Entwurfs Palette unterstützt.

- *Typografische Skalierung* &ndash; ein eigenschaftenpad-Dialogfeld, das Ihnen eine Auswahl von Material Design-kompatiblen Einstellungen für die `textAppearance`-Eigenschaft von Textfeldern ermöglicht.

- Der Design- *Editor* &ndash; ein kleiner Farb Ressourcen-Editor, mit dem Sie Farbinformationen für eine Teilmenge eines Designs festlegen können. Sie können z. b. Material Farben (z. b. `colorPrimary`, `colorPrimaryDark`und `colorAccent`anzeigen und ändern.

Wir sehen uns die einzelnen Features an und geben Beispiele dafür an, wie Sie verwendet werden können.

## <a name="material-design-grid"></a>Material Entwurfs Raster

Das Menü Material Design Grid ist über die Symbolleiste am oberen Rand des Designers verfügbar:

[Entwurfs Raster für![Material](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

Wenn Sie auf das Material Design Grid-Symbol klicken, zeigt der Designer eine Überlagerung auf der Designoberfläche an, die die folgenden Elemente enthält:

- Keylines (orangefarbene Linien)

- Abstand (grüne Bereiche)

- Ein Raster (blaue Linien)

Diese Elemente sind im folgenden Screenshot zu sehen:

[![keyline, Abstände und Raster](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

Jedes dieser Überlagerungs Elemente ist konfigurierbar. Wenn Sie auf das Auslassungs Zeichen (&hellip;) neben dem Raster Menü Material Design klicken, wird ein Dialogfeld popover geöffnet, in dem Sie das Raster deaktivieren/aktivieren, die Platzierung von keylines konfigurieren und die Abstände festlegen können. Beachten Sie, dass alle Werte `dp` (Dichte unabhängige Pixel) ausgedrückt werden:

[![Raster-, keyline-und Abstands Konfiguration](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

Um eine neue keyline hinzuzufügen, geben Sie einen neuen Offset-Wert in das **Offset** Feld ein, wählen Sie einen Speicherort (**Links**, **oben**, **Rechts**oder **unten**) aus, und klicken Sie auf das Symbol + (das rechts angezeigt wird, wenn ein Wert eingegeben wird), um die neue keyline hinzuzufügen. Wenn Sie einen neuen Abstand hinzufügen möchten, geben Sie die Größe und den Offset (in DP) in die **Größen** -bzw. **Offset** Felder ein. Wählen Sie einen Speicherort (**Links**, **oben**, **Rechts**oder **unten**) aus, und klicken Sie auf das Symbol "+", um den neuen Abstand hinzuzufügen.

Wenn Sie diese Konfigurationswerte ändern, werden Sie in der XML-Layoutdatei gespeichert und wieder verwendet, wenn Sie das Layout erneut öffnen.

## <a name="material-design-color-palette"></a>Farb Palette für Material Design

Jedes Eigenschaften Bereichs Element, das eine Farbe annimmt, verfügt jetzt über ein zusätzliches palettensymbol, das Sie zum Öffnen der Farbpalette für Material Design verwenden können, wie in diesem Screenshot gezeigt:

[Symbol "![Farbe"](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

Wenn Sie auf dieses Symbol klicken, wird ein Dialogfeld mit dem popover geöffnet, das es Ihnen ermöglicht, die Farbe dieser Eigenschaft von der Farbpalette für Material Design zu konfigurieren:

[Farbpalette für![Material Design](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

Am oberen Rand der Farbpalette werden die Entwurfs Farben für das primäre Material angezeigt, während der untere Teil der Palette einen Bereich von Farben für die ausgewählte Primärfarbe anzeigt. Wenn Sie z. b. **Indigo**auswählen, wird am unteren Rand des Dialog Felds eine Auflistung von **Indigo** -Farben angezeigt.
Wenn Sie einen Farbton auswählen, wird die Farbe der Eigenschaft in den ausgewählten Farbton geändert. Im folgenden Beispiel wird der `Background Tint` der Schaltfläche in *Indigo 500*geändert:

[Wählen Sie![Indigo 500 aus.](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` ist auf den Farbcode für *Indigo 500* (`#ff3f51b5`) festgelegt, und der Designer aktualisiert die Hintergrundfarbe der Schaltfläche, um diese Änderung widerzuspiegeln:

[![Hintergrund-Tönungs-Änderungen](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

Weitere Informationen zur Material Design-Farbpalette finden Sie im Leitfaden Material Design [Color Palette](https://material.io/design/color/).

## <a name="typographic-scale"></a>Typografische Skalierung

Der Abschnitt **Text** Darstellung der Registerkarte **Stil** des **Eigenschaften** Auffüllens verfügt über ein Symbol, mit dem Sie einen `TextAppearance` Stil auswählen können, der der Material Entwurfs Spezifikation entspricht:

[Registerkarte "!["](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

Wenn Sie auf dieses Symbol klicken, wird das Dialogfeld **typografische Skalierung** geöffnet, das eine Liste vorkonfigurierter Text Stile enthält, aus denen Sie auswählen können:

[![Text Stil Auswahl](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

Im folgenden Beispiel wird durch Klicken auf **Anzeige 1** der Text der Schaltfläche in die größere Schriftart der **Anzeige 1**geändert:

[![Anzeige 1 Stil](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

Der Textstil im Dialogfeld **typografische Skalierung** folgt der **Design Einstellung.** Wenn das **helle** Design z. b. im Designer ausgewählt ist, spiegelt die Liste der verfügbaren Text Stile das **helle** Design wider:

[Design "![hell"](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>Design-Editor

Mit dem Design- **Editor** können Sie Farbinformationen für eine Teilmenge von Design Attributen anpassen. Klicken Sie zum Öffnen des Design- **Editors**auf das Pinselsymbol auf der Symbolleiste:

[Symbol "![Design-Editor"](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

Obwohl der Design- **Editor** über die Symbolleiste für alle Android-Ziel Versionen und API-Ebenen zugänglich ist, ist nur eine Teilmenge der unten beschriebenen Funktionen verfügbar, wenn die Ziel-API-Ebene älter als API 21 (Android 5,0 Lollipop) ist.

Im linken Bereich des Design- **Editors** wird die Liste der Farben angezeigt, aus denen das aktuell ausgewählte Design besteht (in diesem Beispiel verwenden wir das `Default Theme`):

[Design-Editor für![](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

Wenn Sie eine Farbe auf der linken Seite auswählen, werden im rechten Bereich die folgenden Registerkarten angezeigt, die Ihnen bei der Bearbeitung dieser Farbe helfen:

- **Erben** &ndash; zeigt ein diagrammvererbungs Diagramm für die ausgewählte Farbe an und listet den aufgelösten Farb-und Farbcode auf, der der Design Farbe zugewiesen ist.

- Mit der **Farb** Auswahl &ndash; können Sie die ausgewählte Farbe in einen beliebigen Wert ändern.

- Mithilfe der **Material Palette** &ndash; können Sie die ausgewählte Farbe in einen Wert ändern, der dem Material Entwurf entspricht.

- Mithilfe von **Ressourcen** &ndash; können Sie die ausgewählte Farbe in eine der anderen vorhandenen Farb Ressourcen im Design ändern.

Sehen wir uns diese Registerkarten ausführlich an.

### <a name="inherit-tab"></a>Registerkarte erben

Wie im folgenden Beispiel gezeigt, listet die Registerkarte **erben** die Stil Vererbung für die **Hintergrund** Farbe des **Standard**Designs auf:

[![Registerkarte erben](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

In diesem Beispiel erbt das **Standard** Design von einem Stil, der `@color/background_material_dark` verwendet, aber überschreibt ihn mit `color/material_grey_850`, der über einen Farbcodewert von `#ff303030`verfügt.
Weitere Informationen zur Stil Vererbung finden Sie unter [Stile und](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)Designs.

### <a name="color-picker"></a>Farbauswahl

Der folgende Screenshot veranschaulicht die **Farb**Auswahl:

[![Farbauswahl](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

In diesem Beispiel kann die **Hintergrund** Farbe auf verschiedene Weise in einen beliebigen Wert geändert werden:

- Direktes klicken auf eine Farbe.
- Eingeben von Farbton-, Sättigungs-und Helligkeits Werten.
- Eingeben von RGB-Werten (rot, grün, blau) in Dezimalzahlen.
- Das Alpha (Deckkraft) für die ausgewählte Farbe wird festgelegt.
- Direktes Eingeben des hexadezimalen Farbcodes.

Die Farbe, die Sie in der Farbauswahl auswählen, ist *nicht* auf die Richtlinien für den Material Entwurf oder auf die Menge der verfügbaren Farb Ressourcen beschränkt.

### <a name="resources"></a>Ressourcen

Die Registerkarte **Ressourcen** enthält eine Liste mit Farb Ressourcen, die im Design bereits vorhanden sind:

[![Ressourcen](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

Mithilfe der Registerkarte **Ressourcen** können Sie Ihre Auswahl auf diese Liste von Farben beschränken. Beachten Sie, dass bei Auswahl einer Farb Ressource, die bereits einem anderen Teil des Designs zugewiesen ist, zwei angrenzende Elemente der Benutzeroberfläche möglicherweise zusammengeführt werden (da Sie dieselbe Farbe aufweisen) und der Benutzer schwer zu unterscheiden ist.

### <a name="material-palette"></a>Material Palette

Die Registerkarte **Material Palette** öffnet die [oben beschriebene](#material-design-color-palette) **Material Design-Farbpalette** . Durch die Auswahl eines Farbwerts aus dieser Palette wird die gewünschte Farbe eingeschränkt, damit Sie mit den Material Design-Richtlinien konsistent ist.

[![Material Palette](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>Erstellen eines neuen Designs

Im folgenden Beispiel wird die Material Palette verwendet, um ein neues benutzerdefiniertes Design zu erstellen. Zuerst ändern wir die **Hintergrund** Farbe in *blau 900*:

[![Hintergrund in blau 900 ändern](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

Wenn eine Farb Ressource geändert wird, wird eine Meldung mit der Meldung angezeigt, *das aktuelle Design weist ungespeicherte Änderungen*auf:

[Warnung![nicht gespeicherten Änderungen](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

Die Farbänderung im Designer wurde vorgenommen, diese Änderung wurde jedoch noch nicht gespeichert. An diesem Punkt können Sie einen der folgenden Schritte ausführen:

- Klicken Sie auf **Änderungen verwerfen** , um die neue Farbauswahl (oder Auswahl) zu verwerfen und das Design in seinen ursprünglichen Zustand zurückzusetzen.

- Drücken  **&#8984; Sie + S** , um die Änderungen an einem neuen Design namens **Custom**zu speichern.

## <a name="summary"></a>Zusammenfassung

In diesem Thema wurden die in xamarin. Android Designer verfügbaren Material Design Features beschrieben. Es wurde erläutert, wie Sie das Material Design Grid aktivieren und konfigurieren, wie Sie die Farb Palette für Material Design verwenden, um Farbeigenschaften zu bearbeiten, und wie Sie die typografische Skalierungs Auswahl verwenden, um Texteigenschaften zu konfigurieren. Außerdem wurde erläutert, wie der Design-Editor verwendet wird, um neue benutzerdefinierte Designs zu erstellen, die den Material Design-Richtlinien entsprechen. Weitere Informationen zur xamarin. Android-Unterstützung für Material Design finden Sie unter [Material](~/android/user-interface/material-theme.md)Design.

-----

## <a name="related-links"></a>Verwandte Links

- [Materialdesign](~/android/user-interface/material-theme.md)
- [Einführung in Material Design](https://material.io/design/introduction)
