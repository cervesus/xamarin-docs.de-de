---
title: Einführung in xamarin. Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: 871d7cad6c57cd34757ae992ce14d5f686935584
ms.sourcegitcommit: 283810340de5310f63ef7c3e4b266fe9dc2ffcaf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73662311"
---
# <a name="xamarinforms-collectionview-introduction"></a>Einführung in xamarin. Forms CollectionView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen Layoutspezifikationen. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView)bereitzustellen. Die folgenden Screenshots zeigen z. b. eine `CollectionView`, die ein vertikales Raster mit zwei Spalten verwendet und das mehrere Auswahlmöglichkeiten ermöglicht:

[![Screenshot eines vertikalen auflistungsansichts Layouts unter IOS und Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertikal Raster Layout mit Mehrfachauswahl")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertikal Raster Layout mit Mehrfachauswahl")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist in xamarin. Forms 4,3 verfügbar.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist unter IOS und Android verfügbar, aber nur teilweise auf dem universelle Windows-Plattform [verfügbar](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) .

## <a name="collectionview-and-listview-differences"></a>Unterschiede zwischen CollectionView und ListView

Obwohl die [`CollectionView`](xref:Xamarin.Forms.CollectionView) -und [`ListView`](xref:Xamarin.Forms.ListView) -APIs ähnlich sind, gibt es einige wichtige Unterschiede:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über ein flexibles Layoutmodell, das das vertikale oder horizontale darstellen von Daten in einer Liste oder einem Raster ermöglicht.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt die einfache und mehrfache Auswahl.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) automatisch die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellte Virtualisierung verwendet.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) reduziert die API-Oberfläche [`ListView`](xref:Xamarin.Forms.ListView). Viele Eigenschaften und Ereignisse aus [`ListView`](xref:Xamarin.Forms.ListView) sind in `CollectionView` nicht vorhanden.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) enthält keine integrierten Trennzeichen.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) löst eine Ausnahme aus, wenn die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) vom UI-Thread aktualisiert wird.

## <a name="move-from-listview-to-collectionview"></a>Aus ListView in CollectionView verschieben

[`ListView`](xref:Xamarin.Forms.ListView) Implementierungen in vorhandenen xamarin. Forms-Implementierungen können mithilfe der folgenden Tabelle zu [`CollectionView`](xref:Xamarin.Forms.CollectionView) Implementierungen migriert werden:

| Konzept | ListView-API | CollectionView |
|---|---|---|
| Daten | `ItemsSource` | Ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird mit Daten aufgefüllt, indem seine `ItemsSource`-Eigenschaft festgelegt wird. Weitere Informationen finden Sie unter Auffüllen [einer CollectionView mit Daten](populate-data.md#populate-a-collectionview-with-data). |
| Element Darstellung | `ItemTemplate` | Die Darstellung der einzelnen Elemente in einer [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann durch Festlegen der `ItemTemplate`-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)definiert werden. Weitere Informationen finden Sie unter [Definieren der Element](populate-data.md#define-item-appearance)Darstellung. |
| Zellen | `TextCell`ist `ImageCell`ist `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren. |
| Zeilen Trennzeichen | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) enthält keine integrierten Trennzeichen. Diese können, falls gewünscht, in der Element Vorlage angegeben werden. |
| Auswahl | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt die einfache und mehrfache Auswahl. Weitere Informationen finden Sie unter [xamarin. Forms CollectionView-Auswahl](selection.md). |
| Zeilenhöhe | `HasUnevenRows`, `RowHeight` | In einer `CollectionView` wird die Zeilenhöhe jedes Elements durch die `ItemSizingStrategy`-Eigenschaft bestimmt. Weitere Informationen finden Sie unter [Item Sizing](layout.md#item-sizing).|
| Zwischenspeicherung | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) automatisch die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellte Virtualisierung verwendet. |
| Kopf- und Fußzeilen | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) können eine Kopf-und Fußzeile darstellen, die mit den Elementen in der Liste über die Eigenschaften "`Header`", "`Footer`", "`HeaderTemplate`" und "`FooterTemplate`" scrollen. Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](layout.md#headers-and-footers). |
| Gruppieren | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt ordnungsgemäß gruppierte Daten an, indem die `IsGrouped` Eigenschaft auf `true` festgelegt wird. Gruppen Kopfzeilen und-Fußzeilen können angepasst werden, indem die Eigenschaften `GroupHeaderTemplate` und `GroupFooterTemplate` auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekte festgelegt werden. Weitere Informationen finden Sie unter [xamarin. Forms CollectionView-Gruppierung](grouping.md). |
| Pull zum Aktualisieren | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Die Funktion "Pull to refresh" wird unterstützt, indem ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) als untergeordnetes Element eines `RefreshView` festgelegt wird. Weitere Informationen finden Sie unter [Pull to refresh](populate-data.md#pull-to-refresh). |
| Kontextaktionen | `ContextActions` | Kontext Aktionen werden derzeit in `CollectionView` nicht unterstützt, werden jedoch in einer zukünftigen Version hinzugefügt. |
| Scrollen | `ScrollTo()` | in [`CollectionView`](xref:Xamarin.Forms.CollectionView) werden `ScrollTo` Methoden definiert, mit denen Elemente in der Ansicht angezeigt werden. Weitere Informationen finden Sie unter [scrollvorgänge](scrolling.md). |

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
