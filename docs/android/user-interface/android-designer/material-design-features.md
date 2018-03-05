---
title: Material Entwurfsfunktionen
description: "Dieses Thema beschreibt die Designer-Funktionen, die Entwicklern das Erstellen von Material Design-kompatible Layouts zu erleichtern. Dieser Abschnitt führt und erklärt, wie im Raster Material, Material-Farben-Palette typografische Skala und Design-Editors."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: c0b5fa3e7eacb9f7fd8aa133a290d0e7654972ce
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="material-design-features"></a>Material Entwurfsfunktionen

_Dieses Thema beschreibt die Designer-Funktionen, die Entwicklern das Erstellen von Material Design-kompatible Layouts zu erleichtern. Dieser Abschnitt führt und erklärt, wie im Raster Material, Material-Farben-Palette typografische Skala und Design-Editors._

<a name="overview" />

## <a name="overview"></a>Übersicht

Der Xamarin.Android-Designer umfasst Funktionen, die Ihnen die Erstellung von Material-Design-kompatible Layouts erleichtern. Wenn Sie nicht mit Material-Entwurf vertraut sind, finden Sie unter der [Material Entwurf Einführung](https://www.google.com/design/spec/material-design/introduction.html).

In diesem Handbuch haben wir einen Blick auf die folgenden Designer-Funktionen:

-   *Material Raster* &ndash; Overlay auf der Entwurfsoberfläche angezeigt, die ein Raster, Abstand und Konturen können Sie das Layout Widgets entsprechend Material Entwurfsrichtlinien platzieren angezeigt.

-   *Material Farbpalette* &ndash; ein Eigenschaft Pad-Dialogfeld, die Ihnen dabei hilft, Auswählen einer Farbe aus der Palette der offiziellen Material Entwurf.

-   *Typografische Skalierung* &ndash; ein Eigenschaft Pad-Dialogfeld, die mithilfe einer Auswahl von Material Design-kompatible Einstellungen für stellt die `textAppearance` Eigenschaft von Textfeldern.

-   *Design-Editors* &ndash; ein kleine Farbe Ressourcen-Editor, in dem Sie können festlegen, Farbinformationen für eine Teilmenge eines Designs. Sie können z. B. in der Vorschau anzeigen und Material Farben ändern, wie z. B. `colorPrimary`, `colorPrimaryDark`, und `colorAccent`.

Wir haben Betrachtung der einzelnen Features und enthalten Beispiele für deren Verwendung.


<a name="material_grid" />

## <a name="material-design-grid"></a>Material Entwurfsbereich

Klicken Sie im Entwurfsbereich Material steht auf der Symbolleiste am oberen Rand der Designer zur Verfügung:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Material Entwurfsbereich](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Material Entwurfsbereich](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png)

-----

Wenn Sie das Symbol "Entwurfsbereich Material" klicken, zeigt der Designer Overlay auf der Entwurfsoberfläche angezeigt, die die folgenden Elemente enthält:

-   Konturen (orange Linien)

-   Abstand (grüne Bereiche)

-   Ein Raster (blaue Linien)

Diese Elemente können im folgenden Screenshot angezeigt werden:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Führungslinie, Abstand und Raster](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Führungslinie, Abstand und Raster](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jedes dieser Elemente Overlay ist konfigurierbar. Wenn Sie die Auslassungspunkte neben im Entwurfsbereich Material Menü klicken, wird ein Dialogfeld Popover, mit dem Sie das Raster deaktivieren/aktivieren, konfigurieren die Platzierung von Konturen und Festlegen der Spacings geöffnet. Beachten Sie, die alle Werte ausgedrückt werden `dp` (Dichte unabhängig in Pixel):

![Raster, Führungslinie und Abstand-Konfiguration](material-design-features-images/vs/03-grid-configuration.png)

Um eine neue Führungslinie hinzuzufügen, geben Sie einen neuen Offset-Wert in der **Offset** wählen einen Speicherort (**linken**, **oben**, **rechten**, oder  **unteren**), und klicken Sie auf das Symbol zum Hinzufügen von neuen Führungslinie "+".

Auf ähnliche Weise, um eine neue Abstand hinzuzufügen, geben Sie die Größe und einem festen Offset (in dp) in der **Größe** und **Offset** Felder. Wählen Sie einen Speicherort (**linken**, **oben**, **rechten**, oder **unteren**), und klicken Sie auf das Symbol zum Hinzufügen von neuen Abstands "+".

Wenn Sie diese Werte ändern, werden sie in der Layout-XML-Datei gespeichert und wiederverwendet, wenn Sie das Layout erneut öffnen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Jedes dieser Elemente Overlay ist konfigurierbar. Wenn Sie das unten Dreieck neben der Entwurfsbereich Materialien, die im Menü klicken, wird ein Dialogfeld Popover, mit dem Sie das Raster deaktivieren/aktivieren, konfigurieren die Platzierung von Konturen und Festlegen der Spacings geöffnet. Beachten Sie, die alle Werte ausgedrückt werden `dp` (Dichte unabhängig in Pixel):

[![Raster, Führungslinie und Abstand-Konfiguration](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png)

Um eine neue Führungslinie hinzuzufügen, geben Sie einen neuen Offset-Wert in der **Offset** wählen einen Speicherort (**linken**, **oben**, **rechten**, oder  **unteren**), und klicken Sie auf das Symbol zum Hinzufügen von neuen Führungslinie "+".

Auf ähnliche Weise, um eine neue Abstand hinzuzufügen, geben Sie die Größe und einem festen Offset (in dp) in der **Größe** und **Offset** Felder. Wählen Sie einen Speicherort (**linken**, **oben**, **rechten**, oder **unteren**), und klicken Sie auf das Symbol zum Hinzufügen von neuen Abstands "+".

Wenn Sie diese Werte ändern, werden sie in der Layout-XML-Datei gespeichert und wiederverwendet, wenn Sie das Layout erneut öffnen.


## <a name="material-design-color-palette"></a>Material Design-Farben-Palette

Jedes Element der Eigenschaft-Bereich, der eine Farbe jetzt akzeptiert hat ein zusätzliches Symbol, das Sie zum Öffnen der Material Design-Farben-Palette verwenden können, wie in diesem Screenshot dargestellt:

[![Symbol "ordnerfarbe"](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png)

Wenn Sie dieses Symbol klicken, öffnet ein Dialogfeld Popover, die Sie so konfigurieren Sie die Farbe der Eigenschaft aus der Farbpalette Material Entwurf vereinfacht:

[![Material Design-Farben-palette](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png)

Am Anfang der Farbpalette zeigt primäre Material Design-Farben, während unten auf der Palette einen Bereich von Farbtöne für die ausgewählte primäre Farbe wird angezeigt. Wenn Sie z. B. auswählen **Indigo**, eine Auflistung von **Indigo** Farbtöne wird unten im Dialogfeld angezeigt.
Wenn Sie einen Farbton auswählen, wird die Farbe der Eigenschaft in der ausgewählten Farbton geändert. Im folgenden Beispiel die `Background Tint` der Schaltfläche geändert, um *Indigo 500*:

[![Wählen Sie Indigo 500](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png)

`Background Tint` auf der Farbcode für festgelegt *Indigo 500* (`#ff3f51b5`), und der Designer aktualisiert die Hintergrundfarbe der Schaltfläche entsprechend anpassen:

[![Hintergrund Farbton Änderungen](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png)

Weitere Informationen über die Farbpalette Material Entwurf finden Sie unter den Entwurf Material [Farbe Palette Handbuch](http://www.google.com/design/spec/style/color.html#color-color-palette).

## <a name="typographic-scale"></a>Typografische Skalierung

Der **Darstellung von Text** Teil der **Eigenschaft** Pad **Stil** Registerkarte verfügt über ein Symbol, das Ihnen ermöglicht wählen Sie aus einer `TextAppearance` Format, das dem Material Entwurf entspricht Spezifikation:

[![Registerkarte "Format"](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png)

Wenn Sie dieses Symbol klicken, öffnet der **typografische Skalierung** Dialogfeld Popover, das eine Liste der Textstile vorkonfiguriert, dass darstellt, aus denen Sie auswählen können:

[![Text-Format-Auswahl](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png)

Im folgenden Beispiel auf **Anzeige 1** ändert sich der Text der Schaltfläche wird die größere Schriftart des **Anzeige 1**:

[![Anzeigestil 1](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png)

Das Textformat in die **typografische Skalierung** Dialog entspricht der **Design** Einstellung. Z. B. wenn die **Licht** Design wird im Designer, die Liste der verfügbaren Text-Stile Spiegel ausgewählt der **Licht** Design:

[![Design "hell"](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png)

-----


<a name="theme_editor" />

## <a name="theme-editor"></a>Design-Editors

Die **Design-Editors** ermöglicht Ihnen das Anpassen von Farbinformationen für eine Teilmenge der Design-Attribute. So öffnen die **Design-Editors**, klicken Sie auf das Pinsel-Symbol auf der Symbolleiste:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Symbol "Design-Editors"](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[ ![Symbol "Design-Editors"](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png)

-----

Obwohl die **Design-Editors** ist zugegriffen werden kann, über die Symbolleiste für alle Android-Versionen und API-Ebenen, als Ziel nur eine Teilmenge der unten beschriebenen Funktionen sind verfügbar, wenn die Ziel-API-Ebene API 21 (Android 5.0 vor Lollipop).

Der linke Bereich des der **Design-Editors** zeigt die Liste der Farben, die das ausgewählte Design bilden (in diesem Beispiel verwenden wir die `Default Theme`):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Design-Editors](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Design-Editors](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png)

-----

Wenn Sie eine Farbe auf der linken Seite auswählen, enthält der rechte Bereich die folgenden Registerkarten, damit Sie diese Farbe bearbeiten können:

-   **Erben** &ndash; wird ein Stil Vererbung-Diagramm für die ausgewählte Farbe und aufgelöst Farbe und Farbcode zugewiesen, Farbdesign aufgelistet.

-   **Farbauswahl** &ndash; können Sie die ausgewählte Farbe auf einen beliebigen Wert zu ändern.

-   **Material Palette** &ndash; können Sie der ausgewählten Farbe ändern möchten, ein Wert, der Material Entwurf entspricht.

-   **Ressourcen** &ndash; können Sie ändern die ausgewählte Farbe auf einen anderen vorhandenen Farbressourcen im Design.

Sehen wir uns auf jeweils diese Registerkarten im Detail.


<a name="theme_edit_inherit_tab" />

### <a name="inherit-tab"></a>Registerkarte "erben

Wie im folgenden Beispiel dargestellt die **erben** Registerkarte aufgelistet, die Vererbung Stil für die **Hintergrund** Farbe der **Standarddesign**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Registerkarte "erben](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Registerkarte "erben](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png)

-----

In diesem Beispiel wird die **Standarddesign** erbt ein Format, das verwendet `@color/background_material_dark` aber überschreibt es mit `color/material_grey_850`, die über einen Codewert Farbe verfügt `#ff303030`.
Weitere Informationen über die Formatvorlage Vererbung finden Sie unter [Formatvorlagen und Designs](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).


<a name="theme_edit_color_picker" />

### <a name="color-picker"></a>Farbauswahl

Der folgende Screenshot veranschaulicht die **Farbauswahl**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Farbauswahl](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Farbauswahl](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png)

-----

In diesem Beispiel wird die **Hintergrund** Farbe kann auf einen beliebigen Wert auf verschiedene Weise geändert werden:

-   Wenn Sie eine Farbe direkt auf.
-   Eingeben von Werten für Farbton, Sättigung und Helligkeit.
-   Decimal RGB (Rot, Grün, Blau) Werte eingeben.
-   Festlegen der Alpha (Deckkraft) für die ausgewählte Farbe an.
-   Der Hexadezimal-Farbcode einzugeben direkt.

Die Farbe, die Sie, in dem Farbwähler auswählen wird *nicht* Material Entwurfsrichtlinien oder auf den Satz der verfügbaren Farbressourcen eingeschränkt.

<a name="theme_edit_resources" />

### <a name="resources"></a>Ressourcen

Die **Ressourcen** Registerkarte bietet eine Liste der Color-Ressourcen, die bereits in das Design vorhanden sind:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ressourcen](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Ressourcen](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png)

-----

Mithilfe der **Ressourcen** Registerkarte schränkt Ihre Auswahl auf dieser Liste von Farben. Beachten Sie, dass wenn Sie eine Farbressource, die bereits auf einen anderen Teil des Designs zugewiesen ist auswählen, zwei benachbarte Elemente der Benutzeroberfläche "zusammen" ausführen können (da sie die gleiche Farbe aufweisen) und werden nur schwer für den Benutzer zu unterscheiden.


<a name="theme_edit_material_pallette" />

### <a name="material-palette"></a>Material Palette

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Die **Material Palette** Registerkarte öffnet der **Material Design-Farben-Palette**. Einen Farbwert aus dieser Palette auswählen schränkt die Farbauswahl, damit sie mit Richtlinien für den Entwurf von Material konsistent sind.

[![Material Palette](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png)

Am Anfang der Farbpalette zeigt primäre Material Design-Farben, während unten auf der Palette einen Bereich von Farbtöne für die ausgewählte primäre Farbe wird angezeigt. Wenn Sie z. B. auswählen **Indigo**, eine Auflistung von **Indigo** Farbtöne wird unten im Dialogfeld angezeigt.
Wenn Sie einen Farbton auswählen, wird die Farbe der Eigenschaft in der ausgewählten Farbton geändert. Im folgenden Beispiel die `Background Tint` der Schaltfläche geändert, um *Indigo 500*:

![Wählen Sie Indigo 500](material-design-features-images/vs/10-indigo.png)

`Background Tint` auf der Farbcode für festgelegt *Indigo 500* (`#ff3f51b5`), und der Designer aktualisiert die Hintergrundfarbe, damit diese Änderung angezeigt:

[![Hintergrund Farbton geändert](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png)

Weitere Informationen über die Farbpalette Material Entwurf finden Sie unter den Entwurf Material [Farbe Palette Handbuch](http://www.google.com/design/spec/style/color.html#color-color-palette).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die **Material Palette** Registerkarte geöffnet der **Material Design-Farben-Palette** beschriebenen [früheren](#material_palette). Einen Farbwert aus dieser Palette auswählen schränkt die Farbauswahl, damit sie mit Richtlinien für den Entwurf von Material konsistent sind.

[![Material Palette](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png)

-----


<a name="theme_create" />

### <a name="creating-a-new-theme"></a>Erstellen ein neues Design

Im folgenden Beispiel wird der Palette Material verwendet, um ein neues benutzerdefiniertes Design erstellen. Ändern wir zunächst die **Hintergrund** Farbe *Blau 900*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ändern Sie im Hintergrund in Blau 900](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Ändern Sie im Hintergrund in Blau 900](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png)

-----


Wenn eine Farbressource geändert wird, wird eine Meldung angezeigt mit der folgenden Meldung *das aktuelle Design hat ungespeicherte Änderungen*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alle nicht gespeicherten Änderungen Warnung](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Alle nicht gespeicherten Änderungen Warnung](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png)

-----


Die **Hintergrund** Farbe im Designer auf die neue Farbauswahl geändert hat, aber diese Änderung wurde noch nicht gespeichert. Für die Änderung zu behandeln sind zwei Optionen angeboten:

-   **Änderungen verwerfen** &ndash; verwirft die neue Farbauswahl (oder Auswahl) und das Design auf den ursprünglichen Zustand zurückgesetzt.

-   **Neues Design erstellen** &ndash; erstellt ein neues Design, das stammt aus dem aktuell ausgewählten Design und die Farbe Außerkraftsetzungen in vorgenommenen verwendet die **Design-Editors**.

Beim Klicken auf **neues Design erstellen**, eines der folgenden ausgeführt, je nach dem ausgewählten Design:

-   Wenn das ausgewählte Design im Designer kein Design "Projekt" ist, werden mit der Option zum Erstellen einer neuen Ressourcendatei mit dem benutzerdefinierten Design (mit dem ausgewählten Design als übergeordnetes Element des benutzerdefinierten Designs) angezeigt. Der Designer wechselt zur angepasstes Design, nachdem er erstellt wurde.

-   Ist das aktuell ausgewählte Design ein Design "Projekt", die an einem zentralen Ort definiert ist, werden Sie angezeigt, mit der **Design "Update"** -Option durch Auswählen dieser Option wird aktualisiert, das aktuell ausgewählte Design, anstatt ein neues zu erstellen.

-   Wenn das ausgewählte Design ein Design "Projekt" ist, die an mehreren Stellen definiert ist (z. B. in anders-Suffix **Werte** Ordner z. B. **Werte v21**), ein Dialogfeld mit der Frage angezeigt für einen neuen Speicherort zum Speichern der benutzerdefinierten Designs.

Im vorherige Beispiel, das Sie auf zu Fortfahren **neues Design erstellen** aufgerufen führt zur Erstellung eines neuen Designs **benutzerdefinierte**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefinierte Designs hinzugefügt](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefinierte Designs hinzugefügt](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png)

-----


Da das aktuell ausgewählte Design kein Design "Projekt" ist, ist kein Dialogfenster, um das ausgewählte Design zu aktualisieren oder um einen neuen Speicherort anzugeben.

<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Thema beschriebenen die Material Entwurfsfunktionen im Xamarin.Android Designer verfügbar. Diese erläutert zum Aktivieren und konfigurieren im Entwurfsbereich Material, wie der Material Design-Farben-Palette verwenden, um Farbe Eigenschaften zu bearbeiten und mit dem Selektor typografische Skalierung so konfigurieren Sie die Eigenschaften von Text. Es wird veranschaulicht, wie der Design-Editors zu verwenden, um die neue benutzerdefinierte Designs erstellen, die Entwurfsrichtlinien für Material entsprechen. Weitere Informationen zur Unterstützung von Xamarin.Android für Material Entwurf finden Sie unter [Design "Material"](~/android/user-interface/material-theme.md).



## <a name="related-links"></a>Verwandte Links

- [Materialdesign](~/android/user-interface/material-theme.md)
- [Einführung in die grundlegenden Entwurf](https://www.google.com/design/spec/material-design/introduction.html)
