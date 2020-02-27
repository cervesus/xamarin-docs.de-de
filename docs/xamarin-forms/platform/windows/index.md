---
title: Features der Windows-Plattform
description: In diesem Artikel wird die Unterstützung der Windows-Plattform erläutert, die in xamarin. Forms verfügbar ist.
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 694ec24697937b114036eceab1cafd5aae3617d8
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635549"
---
# <a name="windows-platform-features"></a>Features der Windows-Plattform

Zum Entwickeln von xamarin. Forms-Anwendungen für Windows-Plattformen ist Visual Studio erforderlich. Die [Seite "Unterstützte Plattformen](~/get-started/supported-platforms.md) " enthält weitere Informationen zu den Voraussetzungen.

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>Plattformeigenschaften

Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Sichten,-Seiten und-Layouts auf der universelle Windows-Plattform (UWP) bereitgestellt:

- Festlegen einer Zugriffstaste für einen [`VisualElement`](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [visualelement Access Keys on Windows](visualelement-access-keys.md).
- Deaktivieren des Legacy Farb Modus auf einem unterstützten [`VisualElement`](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [visualelement Legacy Color Mode on Windows](legacy-color-mode.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Ansichten auf UWP bereitgestellt:

- Erkennen von Lesereihenfolge aus Text Inhalt in [`Entry`](xref:Xamarin.Forms.Entry)-, [`Editor`](xref:Xamarin.Forms.Editor)-und [`Label`](xref:Xamarin.Forms.Label) -Instanzen. Weitere Informationen finden Sie unter [inputview-Lesefolge unter Windows](inputview-reading-order.md).
- Aktivieren der TAP-Gesten Unterstützung in einem [`ListView`](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [ListView SelectionMode unter Windows](listview-selectionmode.md).
- Aktivieren der Pull-Richtung einer `RefreshView`, die geändert werden soll. Weitere Informationen finden Sie unter [pullview-pullrichtung unter Windows](refreshview-pulldirection.md).
- Aktivieren einer [`SearchBar`](xref:Xamarin.Forms.SearchBar) für die Interaktion mit der Rechtschreibprüfung. Weitere Informationen finden Sie unter [Windows-Suchleiste Rechtschreibprüfung](searchbar-spell-check.md).
- Aktivieren einer [`WebView`](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem UWP-Meldungs Dialogfeld. Weitere Informationen finden Sie unter [WebView JavaScript Alerts on Windows](webview-javascript-alert.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Seiten auf UWP bereitgestellt:

- Die [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) Navigationsleiste wird reduziert. Weitere Informationen finden Sie [in der masterdetailpage-Navigationsleiste unter Windows](masterdetailpage-navigation-bar.md).
- Festlegen von Platzierungsoptionen für die Symbolleiste. Weitere Informationen finden Sie unter [Platzierung von Seiten Symbolleisten unter Windows](page-toolbar-placement.md).
- Aktivieren von Seiten Symbolen, die auf einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) Symbolleiste angezeigt werden sollen. Weitere Informationen finden Sie unter [TabbedPage Icons on Windows](tabbedpage-icons.md) (TabbedPage-Symbole unter Windows).

Die folgende plattformspezifische Funktionalität wird für die xamarin. Forms- [`Application`](xref:Xamarin.Forms.Application) -Klasse in UWP bereitgestellt:

- Angeben des Verzeichnisses in dem Projekt, aus dem Image Assets geladen werden. Weitere Informationen finden Sie unter [Standard Image Verzeichnis unter Windows](default-image-directory.md).

## <a name="platform-support"></a>Plattformunterstützung

Die in Visual Studio verfügbaren xamarin. Forms-Vorlagen enthalten ein universelle Windows-Plattform Projekt (UWP).

> [!NOTE]
> Xamarin. Forms 1. x und 2. x unterstützen _Windows Phone 8 Silverlight_-, _Windows Phone 8,1_-und _Windows 8.1_ -Anwendungsentwicklung. Diese Projekttypen sind jedoch veraltet.

## <a name="getting-started"></a>Erste Schritte

Wechseln Sie zu **Datei > Neues > Projekt** in Visual Studio, und wählen Sie eine der Platt **Form übergreifenden > leeren App-Vorlagen (xamarin. Forms)** aus, um loszulegen.

Ältere xamarin. Forms-Lösungen oder solche, die unter macOS erstellt werden, verfügen nicht über alle oben aufgeführten Windows-Projekte (Sie müssen jedoch manuell hinzugefügt werden). Wenn die Windows-Plattform, die Sie als Ziel festlegen möchten, nicht bereits in der Projekt Mappe vorhanden ist, können Sie die [Setup Anweisungen](installation/index.md) aufrufen, um die gewünschten Windows-Projekttyp

## <a name="samples"></a>Beispiele

[Alle Beispiele für das](https://github.com/xamarin/xamarin-forms-book-preview-2) Buch zum Erstellen von [*Mobile Apps mit xamarin. Forms in*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) Charles Petzold enthalten universelle Windows-Plattform (für Windows 10)-Projekte.

Die ["Scott Hanselman"-Demo-App](https://github.com/jamesmontemagno/Hanselman.Forms) ist separat verfügbar und umfasst auch Apple Watch-und Android Wear-Projekte (mit xamarin. IOS und xamarin. Android, xamarin. Forms wird auf diesen Plattformen nicht ausgeführt).

## <a name="related-links"></a>Verwandte Links

- [Einrichten von Windows-Projekten](~/xamarin-forms/platform/windows/installation/index.md)
