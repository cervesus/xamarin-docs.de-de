---
title: Android-Plattformfeatures
description: In diesem Artikel wird erläutert, wie Sie xamarin. Forms-Anwendungen Android-spezifische Funktionen hinzufügen.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: ce8d0b834cff5b2eee46b4ace5de4a95d196726d
ms.sourcegitcommit: a3b7e016fb25584dbf57bae89b64a9f98031e7c9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76549994"
---
# <a name="android-platform-features"></a>Android-Plattformfeatures

Zum Entwickeln von xamarin. Forms-Anwendungen für Android ist Visual Studio erforderlich. Die [Seite "Unterstützte Plattformen](~/get-started/supported-platforms.md) " enthält weitere Informationen zu den Voraussetzungen.

## <a name="platform-specifics"></a>Plattformspezifische Besonderheiten

Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Ansichten,-Seiten und-Layouts unter Android bereitgestellt:

- Steuern der Z-Reihenfolge der visuellen Elemente um Zeichnungsreihenfolge zu bestimmen. Weitere Informationen finden Sie unter [visualelement](visualelement-elevation.md)-Rechte Erweiterung unter Android.
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [visualelement Legacy Color Mode on Android](legacy-color-mode.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Ansichten unter Android bereitgestellt:

- Verwenden die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen. Weitere Informationen finden Sie unter Schaltflächen Auffüll Zeichen [und Schatten unter Android](button-padding-shadow.md).
- Festlegen der Eingabemethoden-Editor-Optionen für die Bildschirmtastatur für eine [ `Entry` ](xref:Xamarin.Forms.Entry). Weitere Informationen finden Sie unter Eingabe [Methoden-Editor-Optionen für den Eintrag unter Android](entry-ime-options.md).
- Aktivieren einen Schlagschatten für eine `ImageButton`. Weitere Informationen finden Sie unter [ImageButton Drop Shadows on Android](imagebutton-drop-shadow.md).
- Zum Aktivieren des schnellen Bildlaufs in einer [`ListView`](xref:Xamarin.Forms.ListView) Weitere Informationen finden Sie unter [ListView fast Scroll on Android](listview-fast-scrolling.md).
- Steuern des Übergangs, der beim Öffnen einer `SwipeView`verwendet wird. Weitere Informationen finden Sie unter [swipeer View-Swipe-Übergangsmodus](swipeview-swipetransitionmode.md).
- Steuern, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) gemischte Inhalte anzeigen können. Weitere Informationen finden Sie unter [WebView Mixed Content on Android](webview-mixed-content.md).
- Aktivieren von Zoom für einen [`WebView`](xref:Xamarin.Forms.WebView). Weitere Informationen finden Sie unter [WebView Zoom unter Android](webview-zoom-controls.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Zellen unter Android bereitgestellt:

- Aktivieren [`ViewCell`](xref:Xamarin.Forms.ViewCell) Kontext Aktionen Legacy Modus, sodass das Kontextmenü Menü nicht aktualisiert wird, wenn sich das ausgewählte Element in einer [`ListView`](xref:Xamarin.Forms.ListView) ändert. Weitere Informationen finden Sie unter [viewcell-Kontext Aktionen unter Android](viewcell-context-actions.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Seiten unter Android bereitgestellt:

- Festlegen der Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Weitere Informationen finden Sie unter die [Höhe der Navigations Seitenleiste unter Android](navigationpage-bar-height.md).
- Deaktivieren Übergangsanimationen, bei der Navigation durch Seiten in einem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter " [tabbedpage Page Transition Animation on Android](tabbedpage-transition-animations.md)".
- Aktivieren, Wischen zwischen den Seiten einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter [tabbedpage Page swiping on Android](tabbedpage-page-swiping.md).
- Festlegen der Platzierung der Symbolleiste und die Farbe für eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter [tabbedpage Toolbar Placement und Color on Android](tabbedpage-toolbar-placement-color.md).

Die folgende plattformspezifische Funktionalität wird für die xamarin. Forms- [`Application`](xref:Xamarin.Forms.Application) -Klasse unter Android bereitgestellt:

- Festlegen des Betriebsmodus einer Bildschirmtastatur. Weitere Informationen finden Sie unter [Soft Keyboard Input Mode on Android](soft-keyboard-input-mode.md).
- Deaktivieren der [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seite Lebenszyklusereignisse für anhalten und fortsetzen, für Anwendungen, die AppCompat verwenden. Weitere Informationen finden Sie unter [Seiten Lebenszyklus-Ereignisse unter Android](page-lifecycle-events.md).

## <a name="platform-support"></a>Plattformunterstützung

Ursprünglich hat das xamarin. Forms Android-Standard Projekt eine ältere Art von Steuerelement Rendering verwendet, das vor Android 5,0 üblich war. Anwendungen, die mithilfe der Vorlage erstellt wurden, besitzen`FormsApplicationActivity`als Basisklasse für die Hauptaktivität.

## <a name="material-design-via-appcompat"></a>Material Design per AppCompat

Xamarin. Forms Android-Projekte verwenden jetzt `FormsAppCompatActivity` als Basisklasse der Hauptaktivität. Diese Klasse verwendet die **AppCompat** -Funktionen von Android zur Implementierung von Material Design Designs.

Befolgen Sie die [Installationsanweisungen für die AppCompat-Unterstützung](appcompat-material-design.md), um Materialdesigns zu Ihrem Xamarin.Forms-Projekt für Android hinzuzufügen.

So sieht das **Todo**-Beispiel (Aufgaben) mit dem Standardwert `FormsApplicationActivity` aus:

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

Nachfolgend ist der gleiche Code nach dem Upgrade des Projekts auf `FormsAppCompatActivity`dargestellt (weitere Designinformationen werden ebenso hinzugefügt):

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> Unter Verwendung von `FormsAppCompatActivity` können sich die [Basisklassen für einige benutzerdefinierte Android-Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) unterscheiden.

## <a name="related-links"></a>Verwandte Links

- [Unterstützung für Material Entwurf hinzufügen](appcompat-material-design.md)
