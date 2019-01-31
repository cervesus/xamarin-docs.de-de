---
title: Android-Plattformfeatures
description: In diesem Artikel wird erläutert, wie Android-spezifische Funktionen zu Xamarin.Forms-Anwendungen hinzufügen.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2018
ms.openlocfilehash: a90ae27bb4e0085d1cd0c5b36cf7c00fe5ebfec6
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55292219"
---
# <a name="android-platform-features"></a>Android-Plattformfeatures

Entwickeln von Xamarin.Forms-Anwendungen für Android benötigt Visual Studio. Die [Seite "speicherplatzanforderungen"](~/get-started/installation.md) enthält weitere Informationen zu den Voraussetzungen.

## <a name="platform-specifics"></a>Plattformeigenschaften

Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Ansichten, Seiten und Layouts unter Android bereitgestellt:

- Steuern der Z-Reihenfolge der visuellen Elemente um Zeichnungsreihenfolge zu bestimmen. Weitere Informationen finden Sie unter [VisualElement Erhöhung der Rechte unter Android](visualelement-elevation.md).
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [VisualElement Farbe Legacymodus unter Android](legacy-color-mode.md).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Ansichten unter Android bereitgestellt:

- Verwenden die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen. Weitere Informationen finden Sie unter [Schaltfläche Auffüllen und Schatten unter Android](button-padding-shadow.md).
- Festlegen der Eingabemethoden-Editor-Optionen für die Bildschirmtastatur für eine [ `Entry` ](xref:Xamarin.Forms.Entry). Weitere Informationen finden Sie unter [Eintrag Eingabemethoden-Editor-Optionen unter Android](entry-ime-options.md).
- Aktivieren einen Schlagschatten für eine `ImageButton`. Weitere Informationen finden Sie unter [ImageButton löschen Shadows unter Android](imagebutton-drop-shadow.md).
- Aktivieren schnelle Bildläufe einer [ `ListView` ](xref:Xamarin.Forms.ListView) Weitere Informationen finden Sie unter [ListView schnellen scrollen, unter Android](listview-fast-scrolling.md).
- Steuern, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) gemischte Inhalte anzeigen können. Weitere Informationen finden Sie unter [WebView gemischten Inhalt unter Android](webview-mixed-content.md).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Seiten unter Android bereitgestellt:

- Festlegen der Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Weitere Informationen finden Sie unter ["NavigationPage" Balkenhöhe unter Android](navigationpage-bar-height.md).
- Deaktivieren Übergangsanimationen, bei der Navigation durch Seiten in einem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter ["tabbedpage" Seite Übergangsanimationen unter Android](tabbedpage-transition-animations.md).
- Aktivieren, Wischen zwischen den Seiten einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter ["tabbedpage" der Seite Wischen unter Android](tabbedpage-page-swiping.md).
- Festlegen der Platzierung der Symbolleiste und die Farbe für eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter ["tabbedpage" Symbolleiste Platzierung und Farbe auf Android](tabbedpage-toolbar-placement-color.md).

Die folgende plattformspezifische Funktionalität wird bereitgestellt, für die Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) Klasse, die unter Android:

- Festlegen des Betriebsmodus einer Bildschirmtastatur. Weitere Informationen finden Sie unter [vorläufig Tastatur Input-Modus unter Android](soft-keyboard-input-mode.md).
- Deaktivieren der [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seite Lebenszyklusereignisse für anhalten und fortsetzen, für Anwendungen, die AppCompat verwenden. Weitere Informationen finden Sie unter [Seitenlebenszyklus-Ereignisse auf Android](page-lifecycle-events.md).

## <a name="platform-support"></a>Plattformunterstützung

Ursprünglich verwendet die standardmäßige Xamarin.Forms-Android-Projekt im älteren Format Kontrolle Rendering, die allgemeine vor Android 5.0 war. Anwendungen, die mithilfe der Vorlage erstellt wurden, besitzen`FormsApplicationActivity`als Basisklasse für die Hauptaktivität.

## <a name="material-design-via-appcompat"></a>Materialdesign über AppCompat

Xamarin.Forms-Android-Projekte verwenden `FormsAppCompatActivity` als die Basisklasse von ihrer Hauptaktivität. Diese Klasse verwendet den **AppCompat** Features von Android Material Design-Designs zu implementieren.

Befolgen Sie die [Installationsanweisungen für die AppCompat-Unterstützung](appcompat-material-design.md), um Materialdesigns zu Ihrem Xamarin.Forms-Projekt für Android hinzuzufügen.

So sieht das **Todo**-Beispiel (Aufgaben) mit dem Standardwert `FormsApplicationActivity` aus:

[![](images/before-appcompat-sml.png "Beispiel \"TODO\"-Anwendung ohne AppCompat")](images/before-appcompat.png#lightbox "Todo-Beispielanwendung ohne AppCompat")

Nachfolgend ist der gleiche Code nach dem Upgrade des Projekts auf `FormsAppCompatActivity`dargestellt (weitere Designinformationen werden ebenso hinzugefügt):

[![](images/post-appcompat-sml.png "TODO-Beispielanwendung mit AppCompat und-Design")](images/post-appcompat.png#lightbox "Todo-Beispielanwendung mit AppCompat und-Design")

> [!NOTE]
> Unter Verwendung von `FormsAppCompatActivity` können sich die [Basisklassen für einige benutzerdefinierte Android-Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) unterscheiden.

## <a name="related-links"></a>Verwandte Links

- [Hinzufügen von Unterstützung für Materialdesign](appcompat-material-design.md)
