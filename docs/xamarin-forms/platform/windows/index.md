---
title: Features der Windows-Plattform
description: In diesem Artikel wird die Unterstützung der Windows-Plattform erläutert, die in verfügbar ist Xamarin.Forms .
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 52246ce5d54ba97e91777f598f25c187901f89c4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137773"
---
# <a name="windows-platform-features"></a>Features der Windows-Plattform

Xamarin.FormsZum Entwickeln von Anwendungen für Windows-Plattformen ist Visual Studio erforderlich. Die [Seite "Unterstützte Plattformen](~/get-started/supported-platforms.md) " enthält weitere Informationen zu den Voraussetzungen.

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>Plattformeigenschaften

Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden.

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Sichten, Seiten und Layouts in der universelle Windows-Plattform (UWP) bereitgestellt:

- Festlegen einer Zugriffstaste für ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Weitere Informationen finden Sie unter [visualelement Access Keys on Windows](visualelement-access-keys.md).
- Deaktivieren des Legacy Farb Modus auf einem unterstützten [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Weitere Informationen finden Sie unter [visualelement Legacy Color Mode on Windows](legacy-color-mode.md).

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Ansichten auf UWP bereitgestellt:

- Erkennen von Lesereihenfolge aus Text Inhalt in [`Entry`](xref:Xamarin.Forms.Entry) [`Editor`](xref:Xamarin.Forms.Editor) -,-und- [`Label`](xref:Xamarin.Forms.Label) Instanzen. Weitere Informationen finden Sie unter [inputview-Lesefolge unter Windows](inputview-reading-order.md).
- Aktivieren der TAP-Gesten Unterstützung in einer [`ListView`](xref:Xamarin.Forms.ListView) . Weitere Informationen finden Sie unter [ListView SelectionMode unter Windows](listview-selectionmode.md).
- Aktivieren der Pull-Richtung eines-Vorgangs `RefreshView` , der geändert werden soll. Weitere Informationen finden Sie unter [pullview-pullrichtung unter Windows](refreshview-pulldirection.md).
- Aktivieren [`SearchBar`](xref:Xamarin.Forms.SearchBar) von für die Interaktion mit der Rechtschreibprüfung. Weitere Informationen finden Sie unter [Windows-Suchleiste Rechtschreibprüfung](searchbar-spell-check.md).
- Aktivieren eines [`WebView`](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem UWP-Meldungs Dialogfeld. Weitere Informationen finden Sie unter [WebView JavaScript Alerts on Windows](webview-javascript-alert.md).

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Seiten auf UWP bereitgestellt:

- Reduzieren der [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) Navigationsleiste. Weitere Informationen finden Sie [in der masterdetailpage-Navigationsleiste unter Windows](masterdetailpage-navigation-bar.md).
- Festlegen von Optionen für die Symbolleisten Platzierung Weitere Informationen finden Sie unter [Platzierung von Seiten Symbolleisten unter Windows](page-toolbar-placement.md).
- Aktivieren von Seiten Symbolen, die auf einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) Symbolleiste angezeigt werden sollen. Weitere Informationen finden Sie unter [TabbedPage Icons on Windows](tabbedpage-icons.md) (TabbedPage-Symbole unter Windows).

Die folgenden plattformspezifischen Funktionen werden für die- Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) Klasse in UWP bereitgestellt:

- Angeben des Verzeichnisses in dem Projekt, aus dem Image Assets geladen werden. Weitere Informationen finden Sie unter [Standard Image Verzeichnis unter Windows](default-image-directory.md).

## <a name="platform-support"></a>Plattformunterstützung

Die Xamarin.Forms in Visual Studio verfügbaren Vorlagen enthalten ein universelle Windows-Plattform Projekt (UWP).

> [!NOTE]
> Xamarin.Forms1. x und 2. x unterstützen _Windows Phone 8 Silverlight_-, _Windows Phone 8,1_-und _Windows 8.1_ -Anwendungsentwicklung. Diese Projekttypen sind jedoch veraltet.

## <a name="getting-started"></a>Erste Schritte

Wechseln Sie zu **Datei > neues > Projekt** in Visual Studio, und wählen Sie eine der Platt **Form übergreifenden > leeren app ( Xamarin.Forms )** -Vorlagen aus, um loszulegen.

Ältere Xamarin.Forms Lösungen, die unter macOS erstellt werden, verfügen nicht über alle oben aufgeführten Windows-Projekte (Sie müssen jedoch manuell hinzugefügt werden). Wenn die Windows-Plattform, die Sie als Ziel festlegen möchten, nicht bereits in der Projekt Mappe vorhanden ist, können Sie die [Setup Anweisungen](installation/index.md) aufrufen, um die gewünschten Windows-Projekttyp

## <a name="samples"></a>Beispiele

[Alle Beispiele für das](https://github.com/xamarin/xamarin-forms-book-preview-2) Buch zum [* Xamarin.Forms Erstellen Mobile Apps*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) von Charles Petzold mit enthalten universelle Windows-Plattform (für Windows 10)-Projekte.

Die [Demo-App "Scott Hanselman"](https://github.com/jamesmontemagno/Hanselman.Forms) ist separat verfügbar und umfasst auch Apple Watch-und Android Wear-Projekte (mit xamarin. IOS bzw. xamarin. Android), die Xamarin.Forms nicht auf diesen Plattformen ausgeführt werden.

## <a name="related-links"></a>Verwandte Links

- [Einrichten von Windows-Projekten](~/xamarin-forms/platform/windows/installation/index.md)
