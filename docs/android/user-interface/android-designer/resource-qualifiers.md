---
title: Ressourcenqualifizierer und Visualisierungsoptionen
description: In diesem Thema wird erläutert, wie Ressourcen definiert werden, die nur bei einige Qualifiziererwerte entsprechen verwendet werden wird. Ein einfaches Beispiel ist eine Sprache qualifizierten Zeichenfolgenressource. Eine Zeichenfolgenressource kann mit anderen alternativen Ressourcen definiert, um für weitere Sprachen verwendet werden als Standardwert definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts selbst.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 9d99e6a59b57b59d585b32befdadc0890d41448c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115628"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Ressourcenqualifizierer und visualisierungsoptionen

_In diesem Thema wird erläutert, wie Ressourcen definiert werden, die nur bei einige Qualifiziererwerte entsprechen verwendet werden wird. Ein einfaches Beispiel ist eine Sprache qualifizierten Zeichenfolgenressource. Eine Zeichenfolgenressource kann mit anderen alternativen Ressourcen definiert, um für weitere Sprachen verwendet werden als Standardwert definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts selbst._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>Optionen für ressourcenqualifizierer

**Optionen für ressourcenqualifizierer** zugegriffen werden kann, indem Sie auf die Auslassungspunkte rechts neben der **Querformat** Schaltfläche für Hilfemodus:

[![Optionen für Ressourcenqualifizierer](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

Dieses Dialogfeld zeigt Pulldown-Menüs für die folgenden ressourcenqualifizierer:

-   **Sprache** &ndash; zeigt die verfügbaren Ressourcen und bietet eine Option zum Hinzufügen der neuen Sprache/Region-Ressourcen.

-   **UI-Modus** &ndash; Listen von Anzeigemodi (z. B. **Auto Dock** und **Helpdesk Dock**) sowie layoutausrichtungen.

Jede dieser Pulldown-Menüs wird geöffnet, neue Dialogfelder angezeigt wird, können Sie auswählen und Konfigurieren von ressourcenqualifizierer (wie unten beschrieben).

### <a name="language"></a>Sprache

Die **Sprache** Pulldownmenü enthält nur die Sprachen, die Ressourcen definiert haben (oder **alle Sprachen**, dies ist die Standardeinstellung). Es ist jedoch auch eine **Sprache/Region hinzufügen...**  Option aus, die Ihnen ermöglicht, eine neue Sprache zur Liste hinzuzufügen:

[![Sprache/Region hinzufügen](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

Beim Klicken auf **Sprache/Region hinzufügen...** , **Sprache auswählen** Dialogfeld wird geöffnet, um Dropdownlisten der verfügbaren Sprachen und Regionen anzuzeigen:

![Liste der Sprachen](resource-qualifiers-images/vs/10-languages.png "Liste der Sprachen")

In diesem Beispiel haben wir **fr (Französisch)** für die Sprache und **BE** (Belgien) für das Land/Region Regionalsprache Französisch. Beachten Sie, dass die **Region** Feld ist optional, da viele Sprachen können, ohne dass für bestimmte Regionen angegeben werden. Wenn die **Sprache** Pulldownmenü erneut geöffnet wird, wird die neu hinzugefügte Sprache/Region-Ressource:

![Sprache und Region ausgewählt](resource-qualifiers-images/vs/11-language-region-added.png "Sprache und Region ausgewählt")

Beachten Sie, dass wenn Sie eine neue Sprache hinzufügen, aber Sie erstellen keine neue Ressourcen für sie die hinzugefügte Sprache wird nicht mehr auf das nächste Mal angezeigt werden Sie das Projekt zu öffnen.

### <a name="ui-mode"></a>UI-Modus

Beim Klicken auf die **Benutzeroberflächenmodus** Pulldown-Menü, eine Liste der Modi angezeigt wird, wie z. B. **Normal**, **Auto Dock**, **Helpdesk Dock**, **Fernsehen**, **Appliance**, und **Watch**:


[![Menü für UI-Modus](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Unterhalb dieser Liste sind die Modi Nacht **nicht Nacht** und **Nacht**, gefolgt von der layoutausrichtungen **von links nach rechts** und **von rechts nach links** (für Informationen zu **von links nach rechts** und **von rechts nach links** Optionen finden Sie [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)).
Die letzten Elemente im der **Optionen für Ressourcenqualifizierer** Dialogfeld werden die **runden Bildschirme** (zur Verwendung mit Android Wear) oder **nicht runder Bildschirme**.
Informationen zu von round und nicht rund Bildschirme, finden Sie unter [Layouts](https://developer.android.com/training/wearables/ui/layouts.html).
Weitere Informationen zu Android-Benutzeroberflächen-Modi finden Sie unter [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

## <a name="action-bar-settings"></a>Einstellungen für Aktionsleiste

Die **Einstellungen für Aktionsleiste** Symbol links neben das Pinselsymbol (Design-Editor) verfügbar ist:

![Einstellungen für Aktionsleiste](resource-qualifiers-images/vs/14-action-bar.png "Einstellungen für Aktionsleiste")

Dieses Symbol öffnet ein Dialogfeld im Popover, die eine Möglichkeit bietet, wählen Sie aus einem von drei Modi der Aktionsleiste:

-   **Standard** &ndash; besteht ein Logo oder ein Symbol und dem Titel Text mit einem optionalen Untertitel aus.

-   **Liste** &ndash; Liste Navigationsmodus befindet. Anstelle von statischen Titeltext, bietet dieser Modus einem Listenmenü für die Navigation innerhalb der Aktivität (d. h. es kann angezeigt werden dem Benutzer als ein Dropdown-Liste).

-   **Registerkarten** &ndash; Registerkarte Navigationsmodus befindet. Anstelle von statischen Titeltext enthält in diesem Modus eine Reihe von Registerkarten für die Navigation innerhalb der Aktivität.

## <a name="themes"></a>Designs

Die **Design** Dropdown-Menü zeigt alle der im Projekt definierten Designs. Auswählen von **mehr Designs** öffnet ein Dialogfeld mit einer Liste von allen Designs, die über das installierte Android SDK verfügbar, wie unten dargestellt:

[![Weitere Themen Liste](resource-qualifiers-images/vs/15-theme-menu-sml.png "mehr Designs-Liste")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

Wenn ein Design aktiviert ist, wird der Entwurfsoberfläche aktualisiert, um den Effekt des neuen Designs anzeigen. Beachten Sie, dass diese Änderung dauerhaft erfolgt nur, wenn die **OK** geklickt wird die **Design** Dialogfeld. Nachdem ein Design ausgewählt wurde, wird es in enthalten die **Design** Dropdown-Menü wie unten:

![Design "hell" steht jetzt](resource-qualifiers-images/vs/16-light-theme.png "Design \"hell\" ist jetzt verfügbar")

## <a name="android-version"></a>Android-version

Die Android **Version** Auswahl legt fest, die Android-Version, die zum Rendern des Layouts im Designer verwendet wird. Der Selektor zeigt alle Versionen, die mit Framework-Zielversion des Projekts kompatibel sind:

![Liste der Android-Versionen](resource-qualifiers-images/vs/17-android-version.png "Liste von Android-Versionen")

Framework-Zielversion kann festgelegt werden, in den projekteinstellungen unter **Eigenschaften > Anwendung > Kompilieren mit der Android-Version**. Weitere Informationen zu Framework-Zielversion, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Die Menge von Widgets, die in der Toolbox verfügbar, wird von Framework-Zielversion des Projekts bestimmt. Dies gilt auch für die Eigenschaften der **Fenster "Eigenschaften"**. Ist die Liste der Widgets *nicht* durch die im ausgewählten Wert bestimmt die **Version** Auswahl der Symbolleiste. Z. B. Wenn Sie die Zielversion des Projekts auf Android 4.4 festlegen, Sie können Android 6.0 dennoch auswählen, in der Symbolleiste-Versionsauswahl, um festzustellen, wie das Projekt in Android 6.0 aussieht, aber nicht möglich, Widgets hinzufügen, die auf Android 6.0 spezifisch sind &ndash;  Sie können weiterhin die Widgets stehen, die in Android 4.4 verfügbar sind.

Weitere Informationen zu Ressourcentypen finden Sie unter [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md).



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>Optionen für ressourcenqualifizierer

**Optionen für ressourcenqualifizierer** zugegriffen werden kann, indem Sie auf die Auslassungspunkte rechts neben der **Querformat** Schaltfläche für Hilfemodus:

[![Optionen für ressourcenqualifizierer](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

Dieses Dialogfeld zeigt Pulldown-Menüs für die folgenden ressourcenqualifizierer:

-   **Sprache** &ndash; zeigt die verfügbaren Ressourcen und bietet eine Option zum Hinzufügen der neuen Sprache/Region-Ressourcen.

-   **UI-Modus** &ndash; Listen von Anzeigemodi (z. B. **Auto Dock** und **Helpdesk Dock**) sowie layoutausrichtungen.

Jede dieser Pulldown-Menüs wird geöffnet, neue Dialogfelder angezeigt wird, können Sie auswählen und Konfigurieren von ressourcenqualifizierer (wie unten beschrieben).

### <a name="language"></a>Sprache

Die **Sprache** Pulldownmenü enthält nur die Sprachen, die Ressourcen definiert haben (oder **alle Sprachen**, dies ist die Standardeinstellung). Es ist jedoch auch eine **Sprache/Region hinzufügen...**  Option aus, die Ihnen ermöglicht, eine neue Sprache zur Liste hinzuzufügen:

[![Sprache/Region hinzufügen](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

Beim Klicken auf **Sprache/Region hinzufügen...** , **Sprache auswählen** Dialogfeld wird geöffnet, um Dropdownlisten der verfügbaren Sprachen und Regionen anzuzeigen:

[![Liste der Sprachen](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

In diesem Beispiel haben wir **fr (Französisch)** für die Sprache und **BE** (Belgien) für das Land/Region Regionalsprache Französisch. Beachten Sie, dass die **Region** Feld ist optional, da viele Sprachen können, ohne dass für bestimmte Regionen angegeben werden. Wenn die **Sprache** Pulldownmenü erneut geöffnet wird, wird die neu hinzugefügte Sprache/Region-Ressource:

[![Sprache und Region ausgewählt](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

Beachten Sie, dass wenn Sie eine neue Sprache hinzufügen, aber Sie erstellen keine neue Ressourcen für sie die hinzugefügte Sprache wird nicht mehr auf das nächste Mal angezeigt werden Sie das Projekt zu öffnen.

### <a name="ui-mode"></a>UI-Modus

Beim Klicken auf die **Benutzeroberflächenmodus** Pulldown-Menü, eine Liste der Modi angezeigt wird, wie z. B. **Normal**, **Auto Dock**, **Helpdesk Dock**, **Fernsehen**, **Appliance**, und **Watch**:

[![Menü für UI-Modus](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

Unterhalb dieser Liste sind die Modi Nacht **nicht Nacht** und **Nacht**, gefolgt von der layoutausrichtungen **von links nach rechts** und **von rechts nach links**. Die letzten paar Optionen können Sie entweder **runden Bildschirme** oder **rechteckigen Bildschirme** (nützlich für Android Wear-Geräte).

Weitere Informationen zu Android-Benutzeroberflächen-Modi finden Sie unter [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Informationen zu **von links nach rechts** und **von rechts nach links** Optionen finden Sie [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).


## <a name="action-bar-settings"></a>Einstellungen für Aktionsleiste

Die **Einstellungen für Aktionsleiste** Symbol links neben das Pinselsymbol (Design-Editor) verfügbar ist:

[![Einstellungen für Aktionsleiste](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

Dieses Symbol öffnet ein Dialogfeld im Popover, die eine Möglichkeit bietet, wählen Sie aus einem von drei Modi der Aktionsleiste:

-   **Standard** &ndash; besteht aus einem Logo oder Symbol und dem Titel Text mit einem optionalen Untertitel.

-   **Liste** &ndash; Liste Navigationsmodus befindet. Anstelle von statischen Titeltext, bietet dieser Modus einem Listenmenü für die Navigation innerhalb der Aktivität (d. h. es kann angezeigt werden dem Benutzer als ein Dropdown-Liste).

-   **Registerkarten** &ndash; Registerkarte Navigationsmodus befindet. Anstelle von statischen Titeltext enthält in diesem Modus eine Reihe von Registerkarten für die Navigation innerhalb der Aktivität.

## <a name="themes"></a>Designs

Die **Design** Dropdown-Menü zeigt alle der im Projekt definierten Designs. Auswählen von **mehr Designs** öffnet ein Dialogfeld mit einer Liste von allen Designs, die über das installierte Android SDK verfügbar, wie unten dargestellt:

[![Weitere Themen-Liste](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

Wenn ein Design aktiviert ist, wird der Entwurfsoberfläche aktualisiert, um den Effekt des neuen Designs anzeigen. Beachten Sie, dass diese Änderung dauerhaft erfolgt nur, wenn die **OK** geklickt wird die **Design** Dialogfeld. Nachdem ein Design ausgewählt wurde, wird es in enthalten die **Design** Dropdown-Menü wie unten:

[![Design "hell" ist jetzt verfügbar](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Android-version

Die Android **Version** Auswahl legt fest, die Android-Version, die zum Rendern des Layouts im Designer verwendet wird. Der Selektor zeigt alle Versionen, die mit Framework-Zielversion des Projekts kompatibel sind:

[![Liste der Android-Versionen](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

Framework-Zielversion kann festgelegt werden, in die projekteinstellungen in die **Projektoptionen > Erstellen > Allgemein** Abschnitt. Weitere Informationen zu Framework-Zielversion, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Die Menge von Widgets, die in der Toolbox verfügbar, wird von Framework-Zielversion des Projekts bestimmt. Dies gilt auch für die Eigenschaften der **Pad "Eigenschaft"**. Ist die Liste der Widgets *nicht* durch die im ausgewählten Wert bestimmt die **Version** Auswahl der Symbolleiste. Z. B. Wenn Sie die Zielversion des Projekts auf Android 4.4 festlegen, Sie können Android 6.0 dennoch auswählen, in der Symbolleiste-Versionsauswahl, um festzustellen, wie das Projekt in Android 6.0 aussieht, aber nicht möglich, Widgets hinzufügen, die auf Android 6.0 spezifisch sind &ndash;  Sie können weiterhin die Widgets stehen, die in Android 4.4 verfügbar sind.

Weitere Informationen zu Ressourcentypen finden Sie unter [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md).

-----
