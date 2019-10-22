---
title: Ressourcenqualifizierer und Visualisierungsoptionen
description: In diesem Thema wird erläutert, wie Sie Ressourcen definieren, die nur verwendet werden, wenn einige Qualifiziererwerte abgeglichen werden. Ein einfaches Beispiel ist eine sprach qualifizierte Zeichen folgen Ressource. Eine Zeichen folgen Ressource kann als Standard definiert werden, wobei andere alternative Ressourcen für die Verwendung in weiteren Sprachen definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 6ae10e73c3f41226560beb8cf8c50608fc6adf3f
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70762620"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Ressourcen Qualifizierer und Visualisierungs Optionen

_In diesem Thema wird erläutert, wie Sie Ressourcen definieren, die nur verwendet werden, wenn einige Qualifiziererwerte abgeglichen werden. Ein einfaches Beispiel ist eine sprach qualifizierte Zeichen folgen Ressource. Eine Zeichen folgen Ressource kann als Standard definiert werden, wobei andere alternative Ressourcen für die Verwendung in weiteren Sprachen definiert werden. Alle Ressourcentypen können qualifiziert werden, einschließlich des Layouts._

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>Ressourcen qualifiziereroptionen

Sie können auf **Ressourcen qualifiziereroptionen** zugreifen, indem Sie auf das Symbol mit den Auslassungs Punkten rechts neben der Schaltfläche **quer** Format klicken:

[![Resource qualifiziereroptionen](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

Dieses Dialogfeld enthält Pulldownmenüs für die folgenden Ressourcen Qualifizierer:

- **Sprach** &ndash; zeigt verfügbare Sprachressourcen an und bietet eine Option zum Hinzufügen neuer sprach-und Regions Ressourcen.

- Im **UI-Modus** &ndash; werden Anzeigemodi (z. b. **autodock** und **Desk Dock**) sowie layoutrichtungen aufgelistet.

Jedes dieser Pulldownmenüs öffnet neue Dialogfelder, in denen Sie Ressourcen Qualifizierer auswählen und konfigurieren können (wie im folgenden erläutert).

### <a name="language"></a>Sprache

Das Dropdown Menü **Sprache** listet nur die Sprachen auf, für die Ressourcen definiert sind (oder **alle Sprachen**, die Standardeinstellung). Es gibt jedoch auch eine Option zum **Hinzufügen von Sprachen/Regionen** , mit der Sie der Liste eine neue Sprache hinzufügen können:

[![Add Sprache/Region](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

Wenn Sie auf **Sprache/Region hinzufügen...** klicken, wird das Dialogfeld **Sprache auswählen** geöffnet, in dem Dropdown Listen mit den verfügbaren Sprachen und Regionen angezeigt werden:

![Liste der Sprachen](resource-qualifiers-images/vs/10-languages.png "Liste der Sprachen")

In diesem Beispiel haben wir **fr (Französisch)** für die Sprache **gewählt und (** Belgien) für den regionalen Dialekt Französisch gewählt. Beachten Sie, dass das Feld **Region** optional ist, da viele Sprachen ohne Rücksicht auf bestimmte Regionen angegeben werden können. Wenn das **sprach** -Pulldownmenü erneut geöffnet wird, wird die neu hinzugefügte Sprache/Regions Ressource angezeigt:

![Ausgewählte Sprache und Region](resource-qualifiers-images/vs/11-language-region-added.png "Ausgewählte Sprache und Region")

Beachten Sie Folgendes: Wenn Sie eine neue Sprache hinzufügen, aber keine neuen Ressourcen dafür erstellen, wird die hinzugefügte Sprache beim nächsten Öffnen des Projekts nicht mehr angezeigt.

### <a name="ui-mode"></a>UI-Modus

Wenn Sie auf den Pulldownmenü des **UI-Modus** klicken, wird eine Liste der Modi angezeigt, wie z. b. **Normal**, **Car Dock**, **Desk Dock**, **TV**, **Appliance**und **Watch**:

[Menü "![UI Modus"](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Unterhalb dieser Liste befinden sich die Nacht Modi **nicht Nacht** und **Nacht**, gefolgt von den layoutdirections von **Links nach rechts** und von **rechts nach links** (Weitere Informationen zu den Optionen von **Links nach rechts** und von **Rechts** nach Links finden [Sie unter LayoutDirection](xref:Android.Util.LayoutDirection)).
Die letzten Elemente im Dialogfeld **Ressourcen qualifiziereroptionen** sind die **runden Bildschirme** (für die Verwendung mit Android Wear) oder **nicht die Bildschirme**.
Informationen zu runden und nicht runden Bildschirmen finden Sie unter [Layouts](https://developer.android.com/training/wearables/ui/layouts.html).
Weitere Informationen zu den Android-Benutzeroberflächen Modi finden Sie unter [uimodemanager](xref:Android.App.UiModeManager).

## <a name="action-bar-settings"></a>Aktionsleiste Einstellungen

Das Symbol Aktionsleisten- **Einstellungen** ist links neben dem Pinselsymbol (Design-Editor) verfügbar:

![Aktionsleiste Einstellungen](resource-qualifiers-images/vs/14-action-bar.png "Aktionsleiste Einstellungen")

Mit diesem Symbol wird ein Dialogfeld-popover geöffnet, das eine Möglichkeit bietet, eine der drei Aktionsleiste Modi auszuwählen:

- **Standard** &ndash; besteht aus einem Logo oder einem Symbol und einem Titeltext mit einem optionalen Untertitel.

- **Listet** &ndash; Listen Navigationsmodus auf. Anstelle von statischem Titeltext zeigt dieser Modus ein Listen Menü für die Navigation innerhalb der Aktivität an (d. h., es kann dem Benutzer als Dropdown Liste angezeigt werden).

- **Registerkarten** &ndash; Registerkarten-Navigationsmodus. Anstelle von statischem Titeltext stellt dieser Modus eine Reihe von Registerkarten für die Navigation innerhalb der Aktivität dar.

## <a name="themes"></a>Designs

Das **Dropdown** Menü "Design" zeigt alle im Projekt definierten Designs an. Wenn Sie **Weitere** Designs auswählen, wird ein Dialogfeld mit einer Liste aller im installierten Android SDK verfügbaren Themen geöffnet, wie unten dargestellt:

[![Weitere Themenliste](resource-qualifiers-images/vs/15-theme-menu-sml.png "Weitere Themenliste")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

Wenn ein Design ausgewählt ist, wird der Designoberfläche aktualisiert, um die Auswirkung des neuen Designs anzuzeigen. Beachten Sie, dass diese Änderung nur dann dauerhaft gemacht wird, wenn im **Design Dialogfeld** auf die Schaltfläche **OK** geklickt wird. Nachdem ein Design ausgewählt wurde, wird es wie unten dargestellt **in das** Dropdown Menü "Design" eingefügt:

![Das helle Design ist jetzt verfügbar.](resource-qualifiers-images/vs/16-light-theme.png "Das helle Design ist jetzt verfügbar.")

## <a name="android-version"></a>Android-Version

Die Android- **Versions** Auswahl legt die Android-Version fest, die zum Rendering des Layouts im Designer verwendet wird. Der Selektor zeigt alle Versionen an, die mit der Ziel Framework-Version des Projekts kompatibel sind:

![Liste der Android-Versionen](resource-qualifiers-images/vs/17-android-version.png "Liste der Android-Versionen")

Die Zielframeworkversion kann in den Projekteinstellungen unter **Eigenschaften > Anwendungs > mithilfe der Android-Version kompiliert**werden. Weitere Informationen zur Ziel Framework-Version finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Der Satz von Widgets, der in der Toolbox verfügbar ist, wird von der Ziel Framework-Version des Projekts bestimmt. Dies gilt auch für die im **Eigenschaften Fenster**verfügbaren Eigenschaften. Die Liste der verfügbaren Widgets wird *nicht* durch den Wert bestimmt, der in der **Versions** Auswahl der Symbolleiste ausgewählt ist. Wenn Sie z. b. die Zielversion des Projekts auf Android 4,4 festgelegt haben, können Sie in der Symbolleiste der Versions Auswahl weiterhin Android 6,0 auswählen, um zu sehen, wie das Projekt in Android 6,0 aussieht, aber Sie können keine Widgets hinzufügen, die für Android 6,0 spezifisch sind &ndash; Es ist immer noch auf die Widgets beschränkt, die in Android 4,4 verfügbar sind.

Weitere Informationen zu Ressourcentypen finden Sie unter [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>Ressourcen qualifiziereroptionen

Sie können auf **Ressourcen qualifiziereroptionen** zugreifen, indem Sie auf das Symbol mit den Auslassungs Punkten rechts neben der Schaltfläche **quer** Format klicken:

[![Resource qualifiziereroptionen](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

Dieses Dialogfeld enthält Pulldownmenüs für die folgenden Ressourcen Qualifizierer:

- **Sprach** &ndash; zeigt verfügbare Sprachressourcen an und bietet eine Option zum Hinzufügen neuer sprach-und Regions Ressourcen.

- Im **UI-Modus** &ndash; werden Anzeigemodi (z. b. **autodock** und **Desk Dock**) sowie layoutrichtungen aufgelistet.

Jedes dieser Pulldownmenüs öffnet neue Dialogfelder, in denen Sie Ressourcen Qualifizierer auswählen und konfigurieren können (wie im folgenden erläutert).

### <a name="language"></a>Sprache

Das Dropdown Menü **Sprache** listet nur die Sprachen auf, für die Ressourcen definiert sind (oder **alle Sprachen**, die Standardeinstellung). Es gibt jedoch auch eine Option zum **Hinzufügen von Sprachen/Regionen** , mit der Sie der Liste eine neue Sprache hinzufügen können:

[![Add Sprache/Region](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

Wenn Sie auf **Sprache/Region hinzufügen...** klicken, wird das Dialogfeld **Sprache auswählen** geöffnet, in dem Dropdown Listen mit den verfügbaren Sprachen und Regionen angezeigt werden:

[![List von Sprachen](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

In diesem Beispiel haben wir **fr (Französisch)** für die Sprache **gewählt und (** Belgien) für den regionalen Dialekt Französisch gewählt. Beachten Sie, dass das Feld **Region** optional ist, da viele Sprachen ohne Rücksicht auf bestimmte Regionen angegeben werden können. Wenn das **sprach** -Pulldownmenü erneut geöffnet wird, wird die neu hinzugefügte Sprache/Regions Ressource angezeigt:

[ausgewählte ![Language und Region](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

Beachten Sie Folgendes: Wenn Sie eine neue Sprache hinzufügen, aber keine neuen Ressourcen dafür erstellen, wird die hinzugefügte Sprache beim nächsten Öffnen des Projekts nicht mehr angezeigt.

### <a name="ui-mode"></a>UI-Modus

Wenn Sie auf den Pulldownmenü des **UI-Modus** klicken, wird eine Liste der Modi angezeigt, wie z. b. **Normal**, **Car Dock**, **Desk Dock**, **TV**, **Appliance**und **Watch**:

[Menü "![UI Modus"](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

Unterhalb dieser Liste befinden sich die Nacht Modi **nicht Nacht** und **Nacht**, gefolgt von den layoutrichtungen von **Links nach rechts** und von **rechts nach links**. Mit dem letzten paar von Optionen können Sie entweder **roundscreens** oder **rechteckige Bildschirme** auswählen (nützlich für Android Wear-Geräte).

Weitere Informationen zu den Android-Benutzeroberflächen Modi finden Sie unter [uimodemanager](xref:Android.App.UiModeManager).
Informationen zu den Optionen von **Links nach rechts** und von **rechts nach links** finden Sie unter [LayoutDirection](xref:Android.Util.LayoutDirection).

## <a name="action-bar-settings"></a>Aktionsleiste Einstellungen

Das Symbol Aktionsleisten- **Einstellungen** ist links neben dem Pinselsymbol (Design-Editor) verfügbar:

[Einstellungen für ![Action Leiste](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

Mit diesem Symbol wird ein Dialogfeld-popover geöffnet, das eine Möglichkeit bietet, eine der drei Aktionsleiste Modi auszuwählen:

- **Standard** &ndash; besteht aus einem Logo oder einem Symbol und einem Titeltext mit einem optionalen Untertitel.

- **Listet** &ndash; Listen Navigationsmodus auf. Anstelle von statischem Titeltext zeigt dieser Modus ein Listen Menü für die Navigation innerhalb der Aktivität an (d. h., es kann dem Benutzer als Dropdown Liste angezeigt werden).

- **Registerkarten** &ndash; Registerkarten-Navigationsmodus. Anstelle von statischem Titeltext stellt dieser Modus eine Reihe von Registerkarten für die Navigation innerhalb der Aktivität dar.

## <a name="themes"></a>Designs

Das **Dropdown** Menü "Design" zeigt alle im Projekt definierten Designs an. Wenn Sie **Weitere** Designs auswählen, wird ein Dialogfeld mit einer Liste aller im installierten Android SDK verfügbaren Themen geöffnet, wie unten dargestellt:

[Liste der ![More Designs](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

Wenn ein Design ausgewählt ist, wird der Designoberfläche aktualisiert, um die Auswirkung des neuen Designs anzuzeigen. Beachten Sie, dass diese Änderung nur dann dauerhaft gemacht wird, wenn im **Design Dialogfeld** auf die Schaltfläche **OK** geklickt wird. Nachdem ein Design ausgewählt wurde, wird es wie unten dargestellt **in das** Dropdown Menü "Design" eingefügt:

[![Light Design ist jetzt verfügbar.](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Android-Version

Die Android- **Versions** Auswahl legt die Android-Version fest, die zum Rendering des Layouts im Designer verwendet wird. Der Selektor zeigt alle Versionen an, die mit der Ziel Framework-Version des Projekts kompatibel sind:

[![List von Android-Versionen](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

Die Zielframeworkversion kann in den Projekteinstellungen im Abschnitt **Projektoptionen > Build > Allgemein** festgelegt werden. Weitere Informationen zur Ziel Framework-Version finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Der Satz von Widgets, der in der Toolbox verfügbar ist, wird von der Ziel Framework-Version des Projekts bestimmt. Dies gilt auch für die im **eigenschaftenpad**verfügbaren Eigenschaften. Die Liste der verfügbaren Widgets wird *nicht* durch den Wert bestimmt, der in der **Versions** Auswahl der Symbolleiste ausgewählt ist. Wenn Sie z. b. die Zielversion des Projekts auf Android 4,4 festgelegt haben, können Sie in der Symbolleiste der Versions Auswahl weiterhin Android 6,0 auswählen, um zu sehen, wie das Projekt in Android 6,0 aussieht, aber Sie können keine Widgets hinzufügen, die für Android 6,0 spezifisch sind &ndash; Es ist immer noch auf die Widgets beschränkt, die in Android 4,4 verfügbar sind.

Weitere Informationen zu Ressourcentypen finden Sie unter [Android-Ressourcen](~/android/app-fundamentals/resources-in-android/index.md).

-----
