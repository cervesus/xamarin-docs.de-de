---
title: Xamarin.Android-Designer-Material Design-Features
description: Dieses Thema beschreibt die Designer-Funktionen, die für Entwickler zum Erstellen von Material Design-kompatible Layouts zu erleichtern. In diesem Abschnitt werden vorgestellt und erläutert, wie das Material-Raster, der Material-Farben-Palette, die typografischen Skalierung und die Design-Editors.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: eb636c3b7a41adbab9162e192ead65def377a1a0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118980"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Xamarin.Android-Designer Material Design-features

_Dieses Thema beschreibt die Designer-Funktionen, die für Entwickler zum Erstellen von Material Design-kompatible Layouts zu erleichtern. In diesem Abschnitt werden vorgestellt und erläutert, wie das Material-Raster, der Material-Farben-Palette, die typografischen Skalierung und die Design-Editors._

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Weiterentwicklung 2016: Alle Benutzer kann ansprechende Apps Material Design erstellen**

## <a name="overview"></a>Übersicht

Der Xamarin.Android-Designer enthält Funktionen, die Ihnen die Erstellung von Material Design-kompatible Layouts erleichtern. Wenn Sie nicht mit Material Design vertraut sind, finden Sie unter den [Material Design-Einführung](https://material.io/design/introduction).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

In diesem Handbuch haben wir einen Blick auf die folgenden Designer-Funktionen:

-   *Material Raster* &ndash; Überlagerung auf der Entwurfsoberfläche, die ein Raster, Abstand und Konturen zu platzieren layoutwidgets gemäß Material Design-Richtlinien stehen Ihnen angezeigt.

-   *Design-Editors* &ndash; ein kleines Farbe Ressourcen-Editor mit der Sie können festlegen, Farbinformationen für eine Teilmenge eines Designs. Sie können z. B. Vorschau und Material Farben ändern, wie z. B. `colorPrimary`, `colorPrimaryDark`, und `colorAccent`.

Wir sehen Sie sich die einzelnen Features und bieten Beispiele für deren Verwendung.

## <a name="material-design-grid"></a>Materialentwurfsbereich

Klicken Sie im Entwurfsbereich Material steht auf der Symbolleiste am oberen Rand der Designer zur Verfügung:

[![Material Design-Raster](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

Wenn Sie das Material Design-Grid-Symbol klicken, zeigt der Designer eine Überlagerung auf der Entwurfsoberfläche, die die folgenden Elemente enthält:

-   Konturen (orangefarbene Linien)

-   Abstand (grüne Bereiche)

-   Ein Raster (blaue Linien)

Diese Elemente können im vorherigen Screenshot angezeigt werden. Jedes dieser Elemente Overlay ist konfigurierbar. Wenn Sie die Auslassungspunkte neben das Menü "Material Design-Grid" klicken, öffnet ein Dialogfeld im Popover, mit dem Sie das Raster aktivieren/deaktivieren, konfigurieren die Platzierung von Konturen und stilverwandte festgelegt. Beachten Sie, die alle Werte in ausgedrückt werden `dp` (Dichte unabhängigen Pixeln):

[![Raster, Führungslinie und Abstand-Konfiguration](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

Eine neue Führungslinie hinzufügen möchten, geben Sie einen neuen Offset-Wert in der **Offset** wählen einen Speicherort (**linken**, **oben**, **rechten**, oder  **unten**), und klicken Sie auf das Symbol zum Hinzufügen der neuen Führungslinie "+". Auf ähnliche Weise einen neuen Abstand hinzufügen, geben Sie die Größe und den Offset (dp) in der **Größe** und **Offset** Felder. Wählen Sie einen Speicherort (**linken**, **oben**, **rechten**, oder **unten**), und klicken Sie auf die + -Symbol, um den neuen Abstand hinzuzufügen.

Wenn Sie diese Konfigurationswerte ändern, werden sie in der Layout-XML-Datei gespeichert und wiederverwendet werden, wenn Sie das Layout erneut öffnen.


## <a name="theme-editor"></a>Design-Editors

Die **Design-Editors** ermöglicht Ihnen das Anpassen von Farbinformationen für eine Teilmenge der Design-Attribute. Zum Öffnen der **Design-Editors**, klicken Sie auf das Pinsel-Symbol auf der Symbolleiste:

[![Design-Editor-Symbol](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

Obwohl die **Design-Editors** ist verfügbar auf der Symbolleiste für alle Android-Versionen und API-Ebenen, als Ziel nur eine Teilmenge der unten beschriebenen Funktionen sind verfügbar, wenn die Ziel-API-Ebene niedriger als API 21 (Android 5.0 ist Lollipop).

Im linken Bereich, der die **Design-Editors** zeigt die Liste der Farben, die das derzeit ausgewählte Design zu bilden (in diesem Beispiel verwenden wir die `Default Theme`):

[![Design-Editors](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

Wenn Sie eine Farbe auf der linken Seite auswählen, enthält der rechte Bereich die folgenden Registerkarten, damit Sie die entsprechende Farbe bearbeiten können:

-   **Erben** &ndash; zeigt ein Diagramm des Stil-Vererbung für die ausgewählte Farbe an, und listet die aufgelösten Farbe und die Farbcode, Farbdesign zugewiesen.

-   **Farbauswahl** &ndash; können Sie die ausgewählte Farbe auf einen beliebigen Wert zu ändern.

-   **Materialpalette** &ndash; können Sie die ausgewählte Farbe ändern, auf einen Wert, der Material Design entspricht.

-   **Ressourcen** &ndash; können Sie die ausgewählte Farbe auf einen der anderen vorhandenen Farbressourcen im Design ändern.

Sehen wir uns auf jeden einzelnen dieser Registerkarten im Detail.

### <a name="inherit-tab"></a>Registerkarte "erben

Wie im folgenden Beispiel gezeigt die **erben** Registerkarte aufgelistet, die stilvererbung für die **Hintergrund** Farbe der **Design "Standard"**:

[![Registerkarte "erben](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

In diesem Beispiel die **Design "Standard"** erbt ein Format, das verwendet `@color/background_material_light` , aber überschreibt es mit `color/material_grey_50`, Code den Farbwert IValidator.h `#fffafafa`.
Weitere Informationen zu stilvererbung, finden Sie unter [Formatvorlagen und Designs](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).

### <a name="color-picker"></a>Farbauswahl

Der folgende Screenshot veranschaulicht die **Farbauswahl**:

[![Farbauswahl](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

In diesem Beispiel die **Hintergrund** Farbe auf einen beliebigen Wert auf verschiedene Weise geändert werden kann:

-   Wenn Sie eine Farbe direkt auf.
-   Farbton, Sättigung und Helligkeit-Werte eingeben.
-   Eingabe (Rot, Grün, Blau) RGB-Werte im Dezimalformat.
-   Festlegen der Alphaversion (Deckkraft) für die ausgewählte Farbe an.
-   Sie können Sie dem hexadezimalfarbcode direkt eingeben.

Die Farbe, die Sie, in der Farbauswahl auswählen ist *nicht* eingeschränkt oder auf den Satz der verfügbaren Farbressourcen Material Design-Richtlinien.

### <a name="resources"></a>Ressourcen

Die **Ressourcen** Registerkarte bietet eine Liste der Color-Ressourcen, die bereits im Design vorhanden sind:

[![Ressourcen](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

Mithilfe der **Ressourcen** Registerkarte wird Ihre Auswahl auf dieser Liste von Farben beschränkt. Beachten Sie, dass wenn Sie eine Farbressource, die bereits zu einem anderen Teil des Designs zugewiesen ist auswählen, zwei benachbarte Elemente der Benutzeroberfläche "zusammen" ausführen können (da sie die gleiche Farbe haben) und schwierig für den Benutzer aus, um zu unterscheiden.

### <a name="material-palette"></a>Materialpalette

Die **Material Palette** Registerkarte öffnet die **Material Design-Farbpalette**. Wählen Sie einen Farbwert aus dieser Palette schränkt Ihre Farbauswahl, damit sie mit den materialentwurfsrichtlinien konsistent sind:

[![Materialpalette](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

Am Anfang der Farben-Palette zeigt primäre Material Design-Farben an, während der Palette am Ende einen Bereich von Blasen angeordnet, für die ausgewählte primäre Farbe angezeigt. Wenn Sie z. B. auswählen **Indigo**, eine Auflistung von **Indigo** Farbtöne wird am unteren Rand des Dialogfelds angezeigt.
Wenn Sie eine Hue auswählen, wird die Farbe der Eigenschaft in der ausgewählten Farbton geändert. Im folgenden Beispiel die `Background Tint` der Schaltfläche geändert wird *Indigo 500*:

![Wählen Sie Indigo 500](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint` nastaven NA hodnotu der Farbcode für *Indigo 500* (`#ff3f51b5`), und der Designer aktualisiert die Hintergrundfarbe, um diese Änderung zu übernehmen:

[![Hintergrund Farbton geändert](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

Weitere Informationen zu der Material Design-Farben-Palette, finden Sie unter dem Material Design [Color Palette Guide](https://material.io/design/color/).

### <a name="creating-a-new-theme"></a>Erstellen ein neues Design

Im folgenden Beispiel verwenden wir die Palette Material zum Erstellen eines neuen benutzerdefinierten Designs. Zuerst ändern wir die **Hintergrund** Farbe *blaue 900*:

![Ändern von Hintergrund in Blau 900](material-design-features-images/vs/12-change-background-to-blue-w158.png)

Wenn eine Farbressource geändert wird, wird eine Meldung angezeigt mit der Meldung *das aktuelle Design enthält nicht gespeicherte Änderungen*:

[![Nicht gespeicherte Änderungen, die Warnung](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

Die **Hintergrund** Farbe im Designer auf die neue Farbe Auswahl geändert hat, aber diese Änderung wurde noch nicht gespeichert. An diesem Punkt können Sie eine der folgenden Aktionen ausführen:

-   Klicken Sie auf **Änderungen verwerfen** verwerfen die neue Farbe Ihrer Wahl (oder Auswahl) und das Design auf den ursprünglichen Zustand wiederherstellen. 

-   Drücken Sie <kbd>STRG + S</kbd> zum Speichern der Änderungen, die derzeit Design. 

Im folgenden Beispiel <kbd>STRG + S</kbd> gedrückt wurde, damit die Änderungen gespeichert wurden, um **AppTheme**:

[![Änderungen, die auf AppTheme gespeichert](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Thema beschrieben, die in der Xamarin.Android-Designer verfügbaren Material Design-Features. Es wurde erklärt, wie aktivieren und Konfigurieren der Material-Entwurfsbereich, und es wurde erklärt, wie der Design-Editor zu verwenden, um neue benutzerdefinierte Designs erstellen, die Material Design-Richtlinien entsprechen.
Weitere Informationen zu Xamarin.Android-Unterstützung für Material Design, finden Sie unter [Material Design](~/android/user-interface/material-theme.md).




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

In diesem Handbuch wir eine sehen Sie sich die folgenden Designer-Funktionen:

-   *Material Entwurfsbereich* &ndash; Überlagerung auf der Entwurfsoberfläche, die ein Raster, Abstand und Konturen zu platzieren layoutwidgets gemäß Material Design-Richtlinien stehen Ihnen angezeigt.

-   *Material Design-Farbpalette* &ndash; Pad "eine Eigenschaft"-Dialogfeld, das Ihnen dabei hilft, Auswählen einer Farbe aus der Palette der offiziellen Material Design.

-   *Typografische Skalierung* &ndash; Pad "eine Eigenschaft"-Dialogfeld, das Ihnen, zahlreiche Material Design-kompatible Einstellungen für bietet die `textAppearance` Eigenschaft von Textfeldern.

-   *Design-Editors* &ndash; ein kleines Farbe Ressourcen-Editor mit der Sie können festlegen, Farbinformationen für eine Teilmenge eines Designs. Sie können z. B. Vorschau und Material Farben ändern, wie z. B. `colorPrimary`, `colorPrimaryDark`, und `colorAccent`.

Wir sehen Sie sich die einzelnen Features und bieten Beispiele für deren Verwendung.

## <a name="material-design-grid"></a>Materialentwurfsbereich

Klicken Sie im Entwurfsbereich Material steht auf der Symbolleiste am oberen Rand der Designer zur Verfügung:

[![Material Design-Raster](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

Wenn Sie das Material Design-Grid-Symbol klicken, zeigt der Designer eine Überlagerung auf der Entwurfsoberfläche, die die folgenden Elemente enthält:

-   Konturen (orangefarbene Linien)

-   Abstand (grüne Bereiche)

-   Ein Raster (blaue Linien)

Diese Elemente können im folgenden Screenshot angezeigt werden:

[![Führungslinie, Abstand und Raster](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

Jedes dieser Elemente Overlay ist konfigurierbar. Beim Klicken auf die Auslassungspunkte (&hellip;) neben im Entwurfsbereich der Material-Menü öffnet ein Dialogfeld im Popover, mit dem Sie zum Aktivieren/Deaktivieren des Rasters, konfigurieren die Platzierung von Konturen und legen Sie die stilverwandte. Beachten Sie, die alle Werte in ausgedrückt werden `dp` (Dichte unabhängigen Pixeln):

[![Raster, Führungslinie und Abstand-Konfiguration](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

Eine neue Führungslinie hinzufügen möchten, geben Sie einen neuen Offset-Wert in der **Offset** wählen einen Speicherort (**linken**, **oben**, **rechten**, oder  **unten**), und klicken Sie auf das Symbol (die angezeigt wird, wird die richtige, wenn ein Wert eingegeben wird) "+" dem Führungslinie neue hinzufügen. Auf ähnliche Weise einen neuen Abstand hinzufügen, geben Sie die Größe und den Offset (dp) in der **Größe** und **Offset** Felder. Wählen Sie einen Speicherort (**linken**, **oben**, **rechten**, oder **unten**), und klicken Sie auf die + -Symbol, um den neuen Abstand hinzuzufügen.

Wenn Sie diese Konfigurationswerte ändern, werden sie in der Layout-XML-Datei gespeichert und wiederverwendet werden, wenn Sie das Layout erneut öffnen.

## <a name="material-design-color-palette"></a>Material Design-Farben-Palette

Jedes Eigenschaftenelement für den Bereich, der eine Farbe jetzt akzeptiert verfügt über eine zusätzliche Palette-Symbol, das Sie verwenden können, um der Material Design-Farben-Palette zu öffnen, wie im folgenden Screenshot gezeigt:

[![Symbol "ordnerfarbe"](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

Wenn Sie dieses Symbol klicken, öffnet ein Dialogfeld im Popover, die Sie konfigurieren, die Farbe der Eigenschaft aus der Farbpalette Material Design ermöglicht:

[![Material Design-Farben-palette](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

Am Anfang der Farben-Palette zeigt primäre Material Design-Farben an, während der Palette am Ende einen Bereich von Blasen angeordnet, für die ausgewählte primäre Farbe angezeigt. Wenn Sie z. B. auswählen **Indigo**, eine Auflistung von **Indigo** Farbtöne wird am unteren Rand des Dialogfelds angezeigt.
Wenn Sie eine Hue auswählen, wird die Farbe der Eigenschaft in der ausgewählten Farbton geändert. Im folgenden Beispiel die `Background Tint` der Schaltfläche geändert wird *Indigo 500*:

[![Wählen Sie die Indigo 500](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` nastaven NA hodnotu der Farbcode für *Indigo 500* (`#ff3f51b5`), und der Designer aktualisiert die Hintergrundfarbe der Schaltfläche entsprechend anpassen:

[![Hintergrund Tint-Änderungen](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

Weitere Informationen zu der Material Design-Farben-Palette, finden Sie unter dem Material Design [Color Palette Guide](https://material.io/design/color/).

## <a name="typographic-scale"></a>Typografische skalieren

Die **Textdarstellung** Teil der **Eigenschaft** Pad **Stil** Registerkarte verfügt über ein Symbol, das Ihnen ermöglicht wählen Sie aus einer `TextAppearance` Stil, der für das Material Design entspricht Spezifikation:

[![Registerkarte "Style"](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

Wenn Sie dieses Symbol klicken, öffnet der **typografischen Skalierung** Dialogfeld im Popover die eine Liste der Formatvorlagen vorkonfiguriert, dass Text bietet, die Sie auswählen können:

[![Text-Format-Auswahl](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

Im folgenden Beispiel auf **Anzeige 1** ändert sich der Text der Schaltfläche für die größere Schriftart der **Anzeige 1**:

[![Anzeigestil 1](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

Das Textformat in die **typografischen Skalierung** Dialog entspricht dem **Design** Einstellung. Z. B. wenn die **Licht** Design wird ausgewählt, im Designer die Liste der verfügbaren Text-Stile Spiegel der **Licht** Design:

[![Design "hell"](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>Design-Editors

Die **Design-Editors** ermöglicht Ihnen das Anpassen von Farbinformationen für eine Teilmenge der Design-Attribute. Zum Öffnen der **Design-Editors**, klicken Sie auf das Pinsel-Symbol auf der Symbolleiste:

[![Design-Editor-Symbol](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

Obwohl die **Design-Editors** ist verfügbar auf der Symbolleiste für alle Android-Versionen und API-Ebenen, als Ziel nur eine Teilmenge der unten beschriebenen Funktionen sind verfügbar, wenn die Ziel-API-Ebene niedriger als API 21 (Android 5.0 ist Lollipop).

Im linken Bereich, der die **Design-Editors** zeigt die Liste der Farben, die das derzeit ausgewählte Design zu bilden (in diesem Beispiel verwenden wir die `Default Theme`):

[![Design-Editors](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

Wenn Sie eine Farbe auf der linken Seite auswählen, enthält der rechte Bereich die folgenden Registerkarten, damit Sie die entsprechende Farbe bearbeiten können:

-   **Erben** &ndash; zeigt ein Diagramm des Stil-Vererbung für die ausgewählte Farbe an, und listet die aufgelösten Farbe und die Farbcode, Farbdesign zugewiesen.

-   **Farbauswahl** &ndash; können Sie die ausgewählte Farbe auf einen beliebigen Wert zu ändern.

-   **Materialpalette** &ndash; können Sie die ausgewählte Farbe ändern, auf einen Wert, der Material Design entspricht.

-   **Ressourcen** &ndash; können Sie die ausgewählte Farbe auf einen der anderen vorhandenen Farbressourcen im Design ändern.

Sehen wir uns auf jeden einzelnen dieser Registerkarten im Detail.

### <a name="inherit-tab"></a>Registerkarte "erben

Wie im folgenden Beispiel gezeigt die **erben** Registerkarte aufgelistet, die stilvererbung für die **Hintergrund** Farbe der **Design "Standard"**:

[![Registerkarte "erben](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

In diesem Beispiel die **Design "Standard"** erbt ein Format, das verwendet `@color/background_material_dark` , aber überschreibt es mit `color/material_grey_850`, Code den Farbwert IValidator.h `#ff303030`.
Weitere Informationen zu stilvererbung, finden Sie unter [Formatvorlagen und Designs](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).

### <a name="color-picker"></a>Farbauswahl

Der folgende Screenshot veranschaulicht die **Farbauswahl**:

[![Farbauswahl](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)


In diesem Beispiel die **Hintergrund** Farbe auf einen beliebigen Wert auf verschiedene Weise geändert werden kann:

-   Wenn Sie eine Farbe direkt auf.
-   Farbton, Sättigung und Helligkeit-Werte eingeben.
-   Eingabe (Rot, Grün, Blau) RGB-Werte im Dezimalformat.
-   Festlegen der Alphaversion (Deckkraft) für die ausgewählte Farbe an.
-   Sie können Sie dem hexadezimalfarbcode direkt eingeben.

Die Farbe, die Sie, in der Farbauswahl auswählen ist *nicht* eingeschränkt oder auf den Satz der verfügbaren Farbressourcen Material Design-Richtlinien.

### <a name="resources"></a>Ressourcen

Die **Ressourcen** Registerkarte bietet eine Liste der Color-Ressourcen, die bereits im Design vorhanden sind:

[![Ressourcen](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

Mithilfe der **Ressourcen** Registerkarte wird Ihre Auswahl auf dieser Liste von Farben beschränkt. Beachten Sie, dass wenn Sie eine Farbressource, die bereits zu einem anderen Teil des Designs zugewiesen ist auswählen, zwei benachbarte Elemente der Benutzeroberfläche "zusammen" ausführen können (da sie die gleiche Farbe haben) und schwierig für den Benutzer aus, um zu unterscheiden.

### <a name="material-palette"></a>Materialpalette

Die **Material Palette** Registerkarte geöffnet wird die **Material Design-Farbpalette** beschrieben [früheren](#material_palette). Wählen Sie einen Farbwert aus dieser Palette schränkt Ihre Farbauswahl, damit sie mit den materialentwurfsrichtlinien konsistent sind.

[![Materialpalette](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>Erstellen ein neues Design

Im folgenden Beispiel verwenden wir die Palette Material zum Erstellen eines neuen benutzerdefinierten Designs. Zuerst ändern wir die **Hintergrund** Farbe *blaue 900*:

[![Ändern von Hintergrund in Blau 900](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

Wenn eine Farbressource geändert wird, wird eine Meldung angezeigt mit der Meldung *das aktuelle Design enthält nicht gespeicherte Änderungen*:

[![Nicht gespeicherte Änderungen, die Warnung](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

Die Änderung der Farbe im Designer durchgeführt wurde, aber diese Änderung wurde noch nicht gespeichert. An diesem Punkt können Sie eine der folgenden Aktionen ausführen:

-   Klicken Sie auf **Änderungen verwerfen** verwerfen die neue Farbe Ihrer Wahl (oder Auswahl) und das Design auf den ursprünglichen Zustand wiederherstellen. 

-   Drücken Sie  **&#8984; + S** wird aufgerufen, um die Änderungen speichern, um ein neues Design **benutzerdefinierte**. 


## <a name="summary"></a>Zusammenfassung

In diesem Thema beschrieben, die in der Xamarin.Android-Designer verfügbaren Material Design-Features. Er erläutert, wie aktivieren und Konfigurieren der Material-Entwurfsbereich, wie Sie mit der Material Design-Farben-Palette an Farbeigenschaften bearbeiten und wie Sie den Selektor typografischen Skalierung verwenden, um Eigenschaften von Text zu konfigurieren. Es veranschaulicht auch, wie der Design-Editor zu verwenden, um neue benutzerdefinierte Designs erstellen, die Material Design-Richtlinien entsprechen. Weitere Informationen zu Xamarin.Android-Unterstützung für Material Design, finden Sie unter [Material Design](~/android/user-interface/material-theme.md).

-----


## <a name="related-links"></a>Verwandte Links

- [Materialdesign](~/android/user-interface/material-theme.md)
- [Material Design-Einführung](https://material.io/design/introduction)
