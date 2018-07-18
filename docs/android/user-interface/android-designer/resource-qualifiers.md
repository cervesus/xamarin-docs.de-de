---
title: Ressourcenqualifizierer und Visualisierungsoptionen
description: In diesem Thema wird erläutert, wie Ressourcen definiert werden, die nur bei einige Qualifiziererwerte entsprechen verwendet werden wird. Ein einfaches Beispiel ist eine Sprache qualifizierten Zeichenfolgenressource. Eine Zeichenfolgenressource kann mit anderen alternativen Ressourcen definiert, um für weitere Sprachen verwendet werden als Standardwert definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts selbst.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 7bc8c1066e557085c1bf34f77765edbb2259ba7a
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403298"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Ressourcenqualifizierer und Visualisierungsoptionen

_In diesem Thema wird erläutert, wie Ressourcen definiert werden, die nur bei einige Qualifiziererwerte entsprechen verwendet werden wird. Ein einfaches Beispiel ist eine Sprache qualifizierten Zeichenfolgenressource. Eine Zeichenfolgenressource kann mit anderen alternativen Ressourcen definiert, um für weitere Sprachen verwendet werden als Standardwert definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts selbst._


## <a name="custom-device-configurations"></a>Benutzerdefinierte Konfigurationen

Android ist auf eine Vielzahl von Geräten und bildschirmauflösungen verfügbar.
Zum Entwerfen von Benutzeroberflächen zu unterstützen, die viele Geräte abzielen, stellt der Designer eine Reihe von Gerätekonfigurationen integriert. Es unterstützt auch das Hinzufügen von zusätzlichen Gerätekonfigurationen; Diese Konfigurationen basieren auf *Qualifizierer* , die Sie angeben, um eine Gerätekonfiguration von einem anderen zu unterscheiden. Es gibt viele verschiedene Arten von Qualifizierern. Weitere Informationen zu dieser Ressourcentypen finden Sie unter [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md).

Am unteren Rand der Geräteauswahl Menü ist ein **anpassen** option wie folgt:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Auswahl im Menü](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Auswahl im Menü](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


Auswählen von **anpassen** wird ein Dialogfeld angezeigt, die Sie zum Durchsuchen der verfügbaren Konfigurationen verwenden können. Beim Klicken auf die **Gerätedefinitionen** Registerkarte, eine Liste aller bekannten Gerät Definitionen wird angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Geräte, die vorkonfigurierten im Designer werden nicht geändert. Sie können jedoch klicken **Gerät erstellen...**  um eine benutzerdefinierte Gerätedefinition zu definieren. Sie können Alternativ wählen Sie eine vorhandene Definition und auf **klonen...**  als Ausgangspunkt für eine neue Definition verwendet.
Wählen Sie z. B. die **Nexus 5** Definition und auf **Klon...**  zeigt das folgende Dialogfeld angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Gerät Klonen](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Gerät Klonen](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


Im nächsten Screenshot wird der Name geändert, um **Nexus 5 Custom** und Geräteparameter werden geändert, um eine neue benutzerdefinierte Gerätedefinition zu erstellen. In diesem Beispiel **Hochformat** ist deaktiviert, sodass die Gerätedefinition nur das Querformat wird:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefiniertes Gerät](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefiniertes Gerät](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Sobald Sie auf **Clone Device** (Gerät klonen) klicken, wird die neue Gerätedefinition erstellt und in der Liste **Device Definitions** (Gerätedefinitionen) angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Aktualisierte Gerätedefinitionen](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Aktualisierte Gerätedefinitionen](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Beachten Sie, dass jeder Benutzer erstellte Gerätedefinition mit einem grünen Symbol angezeigt wird, wie oben gezeigt. Wenn Sie zurückkehren, um die **Gerät** Auswahl im Menü der neuen benutzerdefinierten Gerätedefinition erhält im obersten Abschnitt der Liste (sofern Sie nicht angezeigt, werden Ihre benutzerdefinierte Gerätekonfiguration in dieser Liste, starten Sie die IDE neu):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefiniertes Gerät wird in der Geräteliste angezeigt.](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefiniertes Gerät wird in der Geräteliste angezeigt.](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


Wählen diese Gerätekonfiguration ändert das Layout entsprechen, die Anpassungen, die zuvor (in diesem Fall ist, nur das Querformat-Modus) erstellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefiniertes Gerät in Verwendung](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefiniertes Gerät in Verwendung](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Optionen für Ressourcenqualifizierer

**Optionen für Ressourcenqualifizierer** zugegriffen werden kann, indem Sie auf die drei Auslassungspunkte rechts neben der **Gerätekonfiguration** Optionen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Optionen für Ressourcenqualifizierer](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Optionen für Ressourcenqualifizierer](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


Dieses Dialogfeld zeigt Pulldown-Menüs für die folgenden ressourcenqualifizierer:

-   **Sprache** &ndash; zeigt die verfügbaren Ressourcen und bietet eine Option zum Hinzufügen der neuen Sprache/Region-Ressourcen.

-   **UI-Modus** &ndash; Listen von Anzeigemodi (z. B. **Auto Dock** und **Helpdesk Dock**) sowie layoutausrichtungen.

Jede dieser Pulldown-Menüs wird geöffnet, neue Dialogfelder angezeigt wird, können Sie auswählen und Konfigurieren von ressourcenqualifizierer (wie unten beschrieben).



### <a name="language"></a>Sprache

Die **Sprache** Pulldownmenü enthält nur die Sprachen, die Ressourcen definiert haben (oder **alle Sprachen**, dies ist die Standardeinstellung). Es ist jedoch auch eine **Sprache/Region hinzufügen...**  Option aus, die Ihnen ermöglicht, eine neue Sprache zur Liste hinzuzufügen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sprache/Region hinzufügen](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Sprache/Region hinzufügen](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


Beim Klicken auf **Sprache/Region hinzufügen...** , **Sprache auswählen** Dialogfeld wird geöffnet, um Dropdownlisten der verfügbaren Sprachen und Regionen anzuzeigen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Liste der Sprachen](resource-qualifiers-images/vs/10-languages.png "Liste der Sprachen")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Liste der Sprachen](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


In diesem Beispiel haben wir **fr (Französisch)** für die Sprache und **BE** (Belgien) für das Land/Region Regionalsprache Französisch. Beachten Sie, dass die **Region** Feld ist optional, da viele Sprachen können, ohne dass für bestimmte Regionen angegeben werden. Wenn die **Sprache** Pulldownmenü erneut geöffnet wird, wird die neu hinzugefügte Sprache/Region-Ressource:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sprache und Region ausgewählt](resource-qualifiers-images/vs/11-language-region-added.png "Sprache und Region ausgewählt")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Sprache und Region ausgewählt](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Beachten Sie, dass wenn Sie eine neue Sprache hinzufügen, aber Sie erstellen keine neue Ressourcen für sie die hinzugefügte Sprache wird nicht mehr auf das nächste Mal angezeigt werden Sie das Projekt zu öffnen.



### <a name="ui-mode"></a>UI-Modus

Beim Klicken auf die **Benutzeroberflächenmodus** Pulldown-Menü, eine Liste der Modi angezeigt wird, wie z. B. **Normal**, **Auto Dock**, **Helpdesk Dock**, **Fernsehen**, **Appliance**, und **Watch**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menü für UI-Modus](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Unterhalb dieser Liste sind die Modi Nacht **nicht Nacht** und **Nacht**, gefolgt von der layoutausrichtungen **von links nach rechts** und **von rechts nach links** (für Informationen zu **von links nach rechts** und **von rechts nach links** Optionen finden Sie [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Die letzten Elemente im der **Optionen für Ressourcenqualifizierer** Dialogfeld die **runden Bildschirme** (zur Verwendung mit Android Wear) oder **Bildschirme nicht gerundet** (Informationen zu runden und nicht rund Bildschirme, finden Sie unter [Layouts](https://developer.android.com/training/wearables/ui/layouts.html)).
Weitere Informationen zu Android-Benutzeroberflächen-Modi finden Sie unter [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Menü für UI-Modus](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Unterhalb dieser Liste sind die Modi Nacht **nicht Nacht** und **Nacht**, gefolgt von der layoutausrichtungen **von links nach rechts** und **von rechts nach links**. Weitere Informationen zu Android-Benutzeroberflächen-Modi finden Sie unter [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Informationen zu **von links nach rechts** und **von rechts nach links** Optionen finden Sie [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Runder Bildschirm

Das letzte Element in der **Optionen für Ressourcenqualifizierer** Dialog ist die **Round Bildschirm** Menü. Dieses Menü können Sie wählen Sie entweder **runden Bildschirme** (zur Verwendung mit Android Wear) oder **rechteckigen Bildschirme**:

[![Das Menü Bildschirm "Round"](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Einstellungen für Aktionsleiste

Die **Einstellungen für Aktionsleiste** Symbol links neben das Pinselsymbol (Design-Editor) verfügbar ist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Einstellungen für Aktionsleiste](resource-qualifiers-images/vs/14-action-bar.png "Einstellungen für Aktionsleiste")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Einstellungen für Aktionsleiste](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Dieses Symbol öffnet ein Dialogfeld im Popover, die eine Möglichkeit bietet, wählen Sie aus einem von drei Modi der Aktionsleiste:

-   **Standard** &ndash; besteht aus einem Logo oder Symbol und dem Titel Text mit einem optionalen Untertitel.

-   **Liste** &ndash; Liste Navigationsmodus befindet. Anstelle von statischen Titeltext, bietet dieser Modus einem Listenmenü für die Navigation innerhalb der Aktivität (d. h. es kann angezeigt werden dem Benutzer als ein Dropdown-Liste).

-   **Registerkarten** &ndash; Registerkarte Navigationsmodus befindet. Anstelle von statischen Titeltext enthält in diesem Modus eine Reihe von Registerkarten für die Navigation innerhalb der Aktivität.



## <a name="themes"></a>Designs

Die **Design** Dropdown-Menü zeigt alle der im Projekt definierten Designs. Auswählen von **mehr Designs** öffnet ein Dialogfeld mit einer Liste von allen Designs, die über das installierte Android SDK verfügbar, wie unten dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Weitere Themen Liste](resource-qualifiers-images/vs/15-theme-menu-sml.png "mehr Designs-Liste")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Weitere Themen-Liste](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Wenn ein Design aktiviert ist, wird der Entwurfsoberfläche aktualisiert, um den Effekt des neuen Designs anzeigen. Beachten Sie, dass diese Änderung dauerhaft erfolgt nur, wenn die **OK** geklickt wird die **Design** Dialogfeld. Nachdem ein Design ausgewählt wurde, wird es in enthalten die **Design** Dropdown-Menü wie unten:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Design "hell" steht jetzt](resource-qualifiers-images/vs/16-light-theme.png "Design \"hell\" ist jetzt verfügbar")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Design "hell" ist jetzt verfügbar](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Android-Version

Die Android **Version** Auswahl legt fest, die Android-Version, die zum Rendern des Layouts im Designer verwendet wird. Der Selektor zeigt alle Versionen, die mit Framework-Zielversion des Projekts kompatibel sind:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Liste der Android-Versionen](resource-qualifiers-images/vs/17-android-version.png "Liste von Android-Versionen")

Framework-Zielversion kann festgelegt werden, in den projekteinstellungen unter **Eigenschaften > Anwendung > Kompilieren mit der Android-Version**. Weitere Informationen zu Framework-Zielversion, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Die Menge von Widgets, die in der Toolbox verfügbar, wird von Framework-Zielversion des Projekts bestimmt. Dies gilt auch für die Eigenschaften der **Fenster "Eigenschaften"**. Ist die Liste der Widgets *nicht* durch die im ausgewählten Wert bestimmt die **Version** Auswahl der Symbolleiste. Z. B. Wenn Sie die Zielversion des Projekts auf Android 4.4 festlegen, Sie können Android 6.0 dennoch auswählen, in der Symbolleiste-Versionsauswahl, um festzustellen, wie das Projekt in Android 6.0 aussieht, aber nicht möglich, Widgets hinzufügen, die auf Android 6.0 spezifisch sind &ndash;  Sie können weiterhin die Widgets stehen, die in Android 4.4 verfügbar sind.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Liste der Android-Versionen](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

Framework-Zielversion kann festgelegt werden, in die projekteinstellungen in die **Projektoptionen > Erstellen > Allgemein** Abschnitt. Weitere Informationen zu Framework-Zielversion, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Die Menge von Widgets, die in der Toolbox verfügbar, wird von Framework-Zielversion des Projekts bestimmt. Dies gilt auch für die Eigenschaften der **Pad "Eigenschaft"**. Ist die Liste der Widgets *nicht* durch die im ausgewählten Wert bestimmt die **Version** Auswahl der Symbolleiste. Z. B. Wenn Sie die Zielversion des Projekts auf Android 4.4 festlegen, Sie können Android 6.0 dennoch auswählen, in der Symbolleiste-Versionsauswahl, um festzustellen, wie das Projekt in Android 6.0 aussieht, aber nicht möglich, Widgets hinzufügen, die auf Android 6.0 spezifisch sind &ndash;  Sie können weiterhin die Widgets stehen, die in Android 4.4 verfügbar sind.

-----
