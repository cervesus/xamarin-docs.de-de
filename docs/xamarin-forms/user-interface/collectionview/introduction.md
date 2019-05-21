---
title: Einführung in die Xamarin.Forms-CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 2ee7b2c203251e519af088a550e7e26f30aa62c8
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971099"
---
# <a name="xamarinforms-collectionview-introduction"></a>Einführung in die Xamarin.Forms-CollectionView

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten anderes Layout-Spezifikationen verwenden. Es zielt darauf ab, die eine flexiblere, bereitstellen und leistungsfähige Alternative zu [ `ListView` ](xref:Xamarin.Forms.ListView). Z. B. die folgenden Screenshots zeigen eine `CollectionView` , die ein vertikale Raster mit zwei Spalten und dem ermöglicht Mehrfachauswahl:

[![Screenshot des einem vertikalen CollectionView Rasterlayout unter iOS und Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertikale Rasterlayout mit Mehrfachauswahl")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertikale Rasterlayout mit Mehrfachauswahl")

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist nur unter iOS und Android verfügbar.

## <a name="collectionview-and-listview-differences"></a>Unterschiede zwischen CollectionView und ListView

Während der [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) und [ `ListView` ](xref:Xamarin.Forms.ListView) APIs sind ähnlich, es gibt einige wichtige Unterschiede:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über ein flexibles Layout-Modell, die Daten vertikal oder horizontal in einer Liste oder einem Raster angezeigt werden kann.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt Einzel- und Mehrfachauswahl.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über kein Konzept für die Zellen aus. Stattdessen wird eine Datenvorlage verwendet, um die Darstellung der einzelnen Elemente der Daten in der Liste zu definieren.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) automatisch nutzt die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) reduziert die API-Oberfläche des [ `ListView` ](xref:Xamarin.Forms.ListView). Viele Eigenschaften und Ereignisse von [ `ListView` ](xref:Xamarin.Forms.ListView) sind nicht im `CollectionView`.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) umfasst keine integrierte Trennzeichen.

## <a name="move-from-listview-to-collectionview"></a>Verschieben von ListView zu CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) Implementierungen in vorhandenen Xamarin.Forms-Implementierungen können zu migriert werden [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) Implementierungen mithilfe der folgenden Tabelle:

| Konzept | ListView-API | CollectionView |
|---|---|---|
| Daten | `ItemsSource` | Ein [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) mit Daten aufgefüllt wird, durch Festlegen seiner `ItemsSource` Eigenschaft. Weitere Informationen finden Sie unter [eine CollectionView mit Daten auffüllen](populate-data.md#populate-a-collectionview-with-data). |
| Element-Darstellung | `ItemTemplate` | Die Darstellung der einzelnen Elemente in einer [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) definiert werden, indem Sie die Einstellung der `ItemTemplate` Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Weitere Informationen finden Sie unter [Element Darstellung definieren](populate-data.md#define-item-appearance). |
| Zellen | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über kein Konzept für die Zellen aus. Stattdessen wird eine Datenvorlage verwendet, um die Darstellung der einzelnen Elemente der Daten in der Liste zu definieren. |
| Zeilentrennzeichen | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) umfasst keine integrierte Trennzeichen. Diese können angegeben werden, falls gewünscht, in der Elementvorlage. |
| Auswahl | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt Einzel- und Mehrfachauswahl. Weitere Informationen finden Sie unter [Xamarin.Forms CollectionView Auswahl](selection.md). |
| Zeilenhöhe | `HasUnevenRows`, `RowHeight` | In einem `CollectionView`, die Höhe jedes Elements richtet sich nach der `ItemSizingStrategy` Eigenschaft. Weitere Informationen finden Sie unter [Element größenanpassung](layout.md#item-sizing).|
| Zwischenspeicherung | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) verwendet automatisch die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt. |
| Kopf- und Fußzeilen | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Kopf- und Fußzeilen werden derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden.|
| Gruppieren | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Gruppierung wird derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden. |
| Zum Aktualisieren nach unten ziehen | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Zum Aktualisieren nach unten ziehen wird derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden. |
| Kontextaktionen | `ContextActions` | Kontextaktionen werden derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden. |
| Scrollen | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert `ScrollTo` Methoden, die Elemente in der Ansicht zu scrollen. Weitere Informationen finden Sie unter [fortlaufender](scrolling.md). |

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
