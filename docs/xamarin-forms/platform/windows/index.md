---
title: Features der Windows-Plattform
description: Dieser Artikel beschreibt die Unterstützung der Windows-Plattform, die in Xamarin.Forms verfügbar ist.
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
ms.openlocfilehash: f14ea52e419ae5d639319f49ae4779de6a2eed31
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55292180"
---
# <a name="windows-platform-features"></a>Features der Windows-Plattform

Entwickeln von Xamarin.Forms-Anwendungen für Windows-Plattformen erfordert Visual Studio. Die [Seite "speicherplatzanforderungen"](~/get-started/installation.md) enthält weitere Informationen zu den Voraussetzungen.

![](images/allhanselman.png "Xamarin.Forms-Anwendungen, die unter Windows")

## <a name="platform-specifics"></a>Plattformeigenschaften

Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Ansichten, Seiten und Layouts für die universelle Windows-Plattform (UWP) bereitgestellt:

- Festlegen einer Zugriffstaste für eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [VisualElement Zugriffstasten für Windows](#visualelement-accesskeys).
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [VisualElement Farbe Legacymodus auf Windows](#legacy-color-mode).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Ansichten in UWP bereitgestellt:

- Erkennen von leserichtung von Textinhalt in [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Weitere Informationen finden Sie unter [InputView Lesefolge auf Windows](#inputview-readingorder).
- Aktivieren von Tap-gestenunterstützung im eine [ `ListView` ](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [ListView SelectionMode auf Windows](#listview-selectionmode).
- Aktivieren einer [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) für die Interaktion mit der Rechtschreibprüfungs-Check-Engine. Weitere Informationen finden Sie unter [SearchBar Rechtschreibprüfung auf Windows](#searchbar-spellcheck).
- Aktivieren einer [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem Meldungsdialogfeld UWP. Weitere Informationen finden Sie unter [WebView-JavaScript-Warnungen für Windows](#webview-javascript-alert).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Seiten in UWP bereitgestellt:

- Reduzieren der [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Navigationsleiste. Weitere Informationen finden Sie unter [MasterDetailPage Navigationsleiste Windows](#collapsable_navigation_bar).
- Festlegen von Platzierungsoptionen für die Symbolleiste. Weitere Informationen finden Sie unter [Symbolleiste-Platzierung auf der Seite auf Windows](#toolbar_placement).
- Aktivieren die Seitensymbole anzuzeigendes eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Symbolleiste. Weitere Informationen finden Sie unter ["tabbedpage" Symbole für Windows](#tabbedpage-icons).

## <a name="platform-support"></a>Plattformunterstützung

Die Xamarin.Forms-Vorlagen in Visual Studio verfügbaren enthalten ein Projekt für universelle Windows-Plattform (UWP).

> [!NOTE]
> Xamarin.Forms-Support für 1.x und 2.x _Windows Phone 8 Silverlight_, _Windows Phone 8.1_, und _Windows 8.1_ Anwendungsentwicklung. Allerdings sind diese Projekttypen veraltet.

## <a name="getting-started"></a>Erste Schritte

Wechseln Sie zu **Datei > Neu > Projekt** in Visual Studio, und wählen Sie eines der **Cross-Platform > leere App (Xamarin.Forms)** Vorlagen für den Einstieg.

Xamarin.Forms-Projektmappen, die älter sind oder unter MacOS erstellt müssen nicht alle oben aufgeführten Windows-Projekte (aber sie müssen manuell hinzugefügt werden). Wenn die Windows-Plattform, die gewünschte Zielplattform nicht, die sich bereits in der Projektmappe Vista ist die [Anweisungen zur Einrichtung des](installation/index.md) Hinzufügen der gewünschten Windows-Projekt Typ(en).

## <a name="samples"></a>Proben

[Alle Beispiele](https://github.com/xamarin/xamarin-forms-book-preview-2) Book von Charles petzolds [ *Erstellen mobiler Apps mit Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) universelle Windows-Plattform (für Windows 10)-Projekte enthalten.

Die ["Scott Hanselman" demo-app](https://github.com/jamesmontemagno/Hanselman.Forms) ist separat erhältlich und umfasst auch Projekte für Apple Watch und Android Wear (mit den entsprechenden Xamarin.iOS und Xamarin.Android, Xamarin.Forms führt nicht auf diesen Plattformen).

## <a name="related-links"></a>Verwandte Links

- [Windows-Setup-Projekten](~/xamarin-forms/platform/windows/installation/index.md)
