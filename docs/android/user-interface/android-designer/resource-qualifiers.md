---
title: Ressourcenqualifizierer und zu den Visualisierungsoptionen
description: "In diesem Thema wird erläutert, wie Ressourcen definiert werden, die verwendet wird, nur, wenn die Qualifiziererwerte abgeglichen werden wird. Ein einfaches Beispiel ist eine Sprache qualifiziert Zeichenfolgenressource. Eine Zeichenfolgenressource kann mit anderen alternativen Ressourcen, die definiert, um für zusätzliche Sprachen verwendet werden als Standard definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts selbst."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: bff6d917fc4ce65daed329f15d6648bbfe0dd069
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="resource-qualifiers-and-visualization-options"></a>Ressourcenqualifizierer und zu den Visualisierungsoptionen

_In diesem Thema wird erläutert, wie Ressourcen definiert werden, die verwendet wird, nur, wenn die Qualifiziererwerte abgeglichen werden wird. Ein einfaches Beispiel ist eine Sprache qualifiziert Zeichenfolgenressource. Eine Zeichenfolgenressource kann mit anderen alternativen Ressourcen, die definiert, um für zusätzliche Sprachen verwendet werden als Standard definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts selbst._


## <a name="custom-device-configurations"></a>Benutzerdefinierte Konfigurationen

Android ist auf eine Fülle an Geräte und bildschirmauflösungen verfügbar.
Entwerfen von Benutzeroberflächen zu unterstützen, die auf viele Geräte abzielen, enthält der Designer eine Vielzahl von Gerätekonfigurationen integriert. Sie unterstützt auch das Hinzufügen von zusätzlichen Gerätekonfigurationen; Diese Konfigurationen basieren auf *Qualifizierer* , dass Sie angeben, dass eine Gerätekonfiguration voneinander zu unterscheiden. Es gibt viele verschiedene Arten von Qualifizierer. Weitere Informationen zu diesen Ressourcen finden Sie unter [Android Ressourcen](~/android/app-fundamentals/resources-in-android/index.md).

Am unteren Rand der Selektor Gerät Menü ist ein **anpassen** option wie folgt:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Gerät Selektor Menü](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Gerät Selektor Menü](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


Auswählen von **anpassen** wird ein Dialogfeld angezeigt, die Sie zum Durchsuchen der verfügbaren Gerätekonfigurationen verwenden können. Beim Klicken auf die **Gerätedefinitionen** Registerkarte eine Liste von Definitionen für alle bekannten Gerät erhält:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Geräte, die vorkonfigurierte im Designer werden nicht geändert. Sie können jedoch klicken **Gerät erstellen...**  um eine benutzerdefinierte Definition zu definieren. Alternativ können Sie die Definition eine vorhandene auswählen und klicken Sie auf **Klon...**  als Ausgangspunkt für eine neue Definition verwendet.
Z. B. Auswahl der **Nexus 5** Definition und auf **Klon...**  zeigt das folgende Dialogfeld:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Klon-Gerät](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Klon-Gerät](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


In der nächste Screenshot wird der Name geändert, um **Nexus 5 benutzerdefinierte** und die Geräteparameter werden geändert, um eine neue benutzerdefinierte geräteeinstellungen-Definition erstellt. In diesem Beispiel **Hochformat** ist deaktiviert, damit die Gerätedefinition nur Querformat ist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefinierte Geräteeinstellungen](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefinierte Geräteeinstellungen](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Sobald Sie auf **Clone Device** (Gerät klonen) klicken, wird die neue Gerätedefinition erstellt und in der Liste **Device Definitions** (Gerätedefinitionen) angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Aktualisierte Device-Definitionen](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Aktualisierte Device-Definitionen](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Beachten Sie, dass jedes benutzerdefiniertes Gerätedefinition mit ein grünes Symbol angezeigt wird, wie oben gezeigt. Wenn Sie zurückkehren, um die **Gerät** Auswahl im Menü die neuen Gerätedefinition der benutzerdefinierten erhält im obersten Abschnitt der Liste (Wenn Sie Ihre benutzerdefinierte Gerätekonfiguration in dieser Liste, starten Sie die IDE nicht angezeigt):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefinierte Gerät wird in der Geräteliste angezeigt.](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefinierte Gerät wird in der Geräteliste angezeigt.](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


Auswählen der Gerätekonfiguration von diesem ändert das Layout, um die Anpassungen, die zuvor (in diesem Modus Groß-/Kleinschreibung, werden nur Querformat) erstellt entsprechen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzerdefinierte Gerät verwendet](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzerdefinierte Gerät verwendet](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Qualifizierer Ressourcenoptionen

**Qualifizierer Ressourcenoptionen** möglich, indem Sie auf den nach-unten Dreieckssymbol rechts neben der **Gerätekonfiguration** Optionen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Qualifizierer Ressourcenoptionen](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Qualifizierer Ressourcenoptionen](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


Dieses Dialogfeld zeigt Pulldownliste Menüs für die folgenden ressourcenqualifizierer dargestellt:

-   **Sprache** &ndash; zeigt die verfügbaren Ressourcen und bietet eine Option zum Hinzufügen von Ressourcen für neue Sprache bzw. der Region.

-   **Benutzeroberflächen-Modus** &ndash; Listen Anzeigemodi (z. B. **Auto Andocken** und **Helpdesk Andocken**) sowie layoutausrichtungen.

Aller mithilfe dieser Menüs Pulldownliste wird geöffnet, in dem Sie auswählen und konfigurieren Sie (wie weiter erläutert) ressourcenqualifizierer können, neue Dialogfelder.



### <a name="language"></a>Sprache

Die **Sprache** Pulldownmenü Listet nur die Sprachen, die Ressourcen definiert haben (oder **alle Sprachen**, dies ist die Standardeinstellung). Es ist jedoch auch eine **Sprache/Region hinzufügen...**  Option, die Sie der Liste eine neue Sprache hinzufügen kann:

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


In diesem Beispiel haben wir **fr (Französisch)** für die Sprache und **BE** (Belgien) für das Land/Region Dialekt Französisch. Beachten Sie, dass die **Region** Feld ist optional, da viele Sprachen ohne Beachtung für bestimmte Regionen angegeben werden können. Wenn die **Sprache** Pulldownmenü erneut geöffnet wird, die neu hinzugefügten Sprache/Region-Ressource wird angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sprache und Region ausgewählt](resource-qualifiers-images/vs/11-language-region-added.png "Sprache und Region ausgewählt")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Sprache und Region ausgewählt](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Beachten Sie, dass wenn Sie eine neue Sprache hinzufügen, aber neue Ressourcen werden nicht erstellt werden, für die sie die hinzugefügte Sprache wird nicht mehr auf das nächste Mal angezeigt werden Sie das Projekt zu öffnen.



### <a name="ui-mode"></a>Benutzeroberflächen-Modus

Beim Klicken auf die **Benutzeroberflächen-Modus** Pulldownmenü, eine Liste der Modi angezeigt wird, wie z. B. **Normal**, **Auto Andocken**, **Helpdesk Andocken**, **Fernsehgerät**, **Appliance**, und **Überwachen**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Benutzeroberflächen-Modus-Menü](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Unterhalb dieser Liste sind die Modi Nacht **nicht Nacht** und **Nacht**, gefolgt von der layoutausrichtungen **von links nach rechts** und **von rechts nach links** (für Informationen zu **von links nach rechts** und **von rechts nach links** Optionen finden Sie [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Die letzten Elemente im die **Qualifizierer Ressourcenoptionen** Dialogfeld werden die **runden Bildschirme** (zur Verwendung mit Android Dach) oder **nicht runden Bildschirme** (Informationen zu runden und nicht-Round-Bildschirme, finden Sie unter [Layouts](https://developer.android.com/training/wearables/ui/layouts.html)).
Weitere Informationen zu Android-UI-Modi finden Sie unter [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benutzeroberflächen-Modus-Menü](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Unterhalb dieser Liste sind die Modi Nacht **nicht Nacht** und **Nacht**, gefolgt von der layoutausrichtungen **von links nach rechts** und **von rechts nach links**. Weitere Informationen zu Android-UI-Modi finden Sie unter [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Informationen zu **von links nach rechts** und **von rechts nach links** Optionen finden Sie [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Round-Bildschirm

Das letzte Element in der **Qualifizierer Ressourcenoptionen** Dialog ist die **Round Bildschirm** Menü. Dieses Menü können Sie entweder auswählen **runden Bildschirme** (zur Verwendung mit Android Dach) oder **rechteckigen Bildschirme**:

[![Round-Bildschirm-Menü](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Aktionsleiste-Einstellungen

Die **Leiste Aktionseinstellungen** Symbol links neben das Pinselsymbol (Design-Editors) verfügbar ist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Einstellungen der Aktionsleiste](resource-qualifiers-images/vs/14-action-bar.png "Aktionsleiste-Einstellungen")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Aktionsleiste-Einstellungen](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Dieses Symbol wird ein Dialogfeld Popover, die eine Möglichkeit bietet, wählen Sie in einem von drei Aktionsleiste Modi geöffnet:

-   **Standard** &ndash; besteht ein Logo oder Symbol und Titel Text mit einem optionalen Untertitel aus.

-   **Liste** &ndash; Navigationsmodus Liste. Anstelle von statischen Titeltext, zeigt dieser Modus eine Listenmenü für die Navigation innerhalb der Aktivität (d. h., es kann werden dem Benutzer angezeigt als eine Dropdown-Liste).

-   **Registerkarten** &ndash; Navigationsmodus Registerkarte. Anstelle von statischen Titeltext enthält dieser Modus eine Reihe von Registerkarten für die Navigation innerhalb der Aktivität.



## <a name="themes"></a>Designs

Die **Design** Dropdown-Menü zeigt alle der im Projekt definierten Designs. Auswählen von **mehr Designs** öffnet ein Dialogfeld mit einer Liste aller Designs aus dem installierte Android-SDK verfügbar, wie unten dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Weitere Designs Liste](resource-qualifiers-images/vs/15-theme-menu-sml.png "mehr Designs-Liste")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Weitere Designs-Liste](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Wenn ein Design ausgewählt ist, wird der Entwurfsoberfläche aktualisiert, um die Auswirkung der das neue Design angezeigt. Beachten Sie, dass diese Änderung dauerhaft erfolgt nur, wenn die **OK** geklickt wird der **Design** Dialogfeld. Nachdem ein Design ausgewählt wurde, wird er in enthalten die **Design** Dropdown-Menü wie unten:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Design "hell" steht jetzt](resource-qualifiers-images/vs/16-light-theme.png "Design "hell" ist jetzt verfügbar")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Design "hell" ist jetzt verfügbar](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Android-Version

Die Android **Version** Selektor wird die Android-Version, die zum Rendern des Layouts im Designer verwendet wird. Die Auswahl enthält alle Versionen, die mit der Framework-Zielversion des Projekts kompatibel sind:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Liste der Android-Versionen](resource-qualifiers-images/vs/17-android-version.png "Liste von Android-Versionen")

Framework-Zielversion kann festgelegt werden, in den projekteinstellungen unter **Eigenschaften > Anwendung > Compile using Android Version**. Weitere Informationen zu Framework-Zielversion, finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Die Sammlung von Widgets in der Toolbox verfügbaren richtet sich nach der Framework-Zielversion des Projekts. Dies gilt auch für die Eigenschaften der **Fenster "Eigenschaften"**. Ist die Liste die verfügbare Widgets *nicht* im ausgewählten Wert gemäß der **Version** Selektor der Symbolleiste. Z. B. Wenn Sie die Zielversion des Projekts auf Android 4.4 festlegen, können Sie dennoch Android 6.0 auswählen, in dem Selektor Symbolleiste Version, um festzustellen, wie das Projekt in Android 6.0 aussieht, aber nicht möglich, fügen Sie Gadgets hinzu, die spezifisch für Android 6.0 sind &ndash;  Sie können weiterhin auf die Widgets beschränkt werden, die in Android 4.4 verfügbar sind.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Liste der Android-Versionen](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

Framework-Zielversion kann festgelegt werden, in den projekteinstellungen unter der **Projektoptionen > Erstellen > Allgemein** Abschnitt. Weitere Informationen zu Framework-Zielversion, finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Die Sammlung von Widgets in der Toolbox verfügbaren richtet sich nach der Framework-Zielversion des Projekts. Dies gilt auch für die Eigenschaften der **Eigenschaft Pad**. Ist die Liste die verfügbare Widgets *nicht* im ausgewählten Wert gemäß der **Version** Selektor der Symbolleiste. Z. B. Wenn Sie die Zielversion des Projekts auf Android 4.4 festlegen, können Sie dennoch Android 6.0 auswählen, in dem Selektor Symbolleiste Version, um festzustellen, wie das Projekt in Android 6.0 aussieht, aber nicht möglich, fügen Sie Gadgets hinzu, die spezifisch für Android 6.0 sind &ndash;  Sie können weiterhin auf die Widgets beschränkt werden, die in Android 4.4 verfügbar sind.

-----
