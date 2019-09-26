---
title: Einführung in xamarin. Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 14abf2e7eff64d2e3e9656bf1ca76f4cee615408
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "69986077"
---
# <a name="xamarinforms-collectionview-introduction"></a>Einführung in xamarin. Forms CollectionView

![Diese API ist zurzeit Vorabversion.](~/media/shared/preview.png)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen Layoutspezifikationen. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView)bereitzustellen. Die folgenden Screenshots zeigen z. b. `CollectionView` ein, das ein vertikales Raster mit zwei Spalten verwendet und das mehrere Auswahlmöglichkeiten ermöglicht:

[![Screenshot eines vertikalen auflistungsansichts Layouts unter IOS und Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertikal Raster Layout mit Mehrfachauswahl")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertikal Raster Layout mit Mehrfachauswahl")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ist in xamarin. Forms 4,0 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem der- `AppDelegate` Klasse unter IOS oder `MainActivity` der-Klasse unter Android die folgende Codezeile hinzugefügt wird, bevor `Forms.Init`aufgerufen wird:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)ist unter IOS und Android verfügbar, ist aber nur [teilweise](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) auf dem universelle Windows-Plattform verfügbar.

## <a name="collectionview-and-listview-differences"></a>Unterschiede zwischen CollectionView und ListView

Obwohl die [`CollectionView`](xref:Xamarin.Forms.CollectionView) - [`ListView`](xref:Xamarin.Forms.ListView) und-APIs ähnlich sind, gibt es einige wichtige Unterschiede:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)verfügt über ein flexibles Layoutmodell, das das vertikale oder horizontale darstellen von Daten in einer Liste oder einem Raster ermöglicht.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt die einfache und mehrfache Auswahl.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwendet automatisch die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellte Virtualisierung.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)verringert die API-Ober [`ListView`](xref:Xamarin.Forms.ListView)Fläche von. Viele Eigenschaften und Ereignisse von [`ListView`](xref:Xamarin.Forms.ListView) sind in `CollectionView`nicht vorhanden.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)enthält keine integrierten Trennzeichen.

## <a name="move-from-listview-to-collectionview"></a>Aus ListView in CollectionView verschieben

[`ListView`](xref:Xamarin.Forms.ListView)Implementierungen in vorhandenen xamarin. Forms-Implementierungen können mithilfe der [`CollectionView`](xref:Xamarin.Forms.CollectionView) folgenden Tabelle zu-Implementierungen migriert werden:

| Konzept | ListView-API | CollectionView |
|---|---|---|
| Daten | `ItemsSource` | Eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird durch Festlegen der `ItemsSource` -Eigenschaft mit Daten aufgefüllt. Weitere Informationen finden Sie unter Auffüllen [einer CollectionView mit Daten](populate-data.md#populate-a-collectionview-with-data). |
| Element Darstellung | `ItemTemplate` | Die Darstellung der einzelnen Elemente in einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann definiert werden, indem die `ItemTemplate` -Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird. Weitere Informationen finden Sie unter [Definieren der Element](populate-data.md#define-item-appearance)Darstellung. |
| Zellen | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren. |
| Zeilen Trennzeichen | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)enthält keine integrierten Trennzeichen. Diese können, falls gewünscht, in der Element Vorlage angegeben werden. |
| Auswahl | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt die einfache und mehrfache Auswahl. Weitere Informationen finden Sie unter [xamarin. Forms CollectionView-Auswahl](selection.md). |
| Zeilenhöhe | `HasUnevenRows`, `RowHeight` | In einem `CollectionView`wird die Zeilenhöhe jedes Elements durch die `ItemSizingStrategy` -Eigenschaft bestimmt. Weitere Informationen finden Sie unter [Item Sizing](layout.md#item-sizing).|
| Zwischenspeicherung | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)verwendet automatisch die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellte Virtualisierung. |
| Kopf- und Fußzeilen | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)kann eine Kopfzeile und eine Fußzeile darstellen, die mit den Elementen in der Liste über `Header`die `Footer`Eigenschaften `HeaderTemplate`,, `FooterTemplate` und scrollen. Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](layout.md#headers-and-footers). |
| Gruppierung | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)zeigt ordnungsgemäß gruppierte Daten durch `IsGrouped` Festlegen der `true`-Eigenschaft auf an. Gruppen Kopfzeilen und-Fußzeilen können durch Festlegen der- `GroupHeaderTemplate` Eigenschaft `GroupFooterTemplate` und der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) -Eigenschaft auf-Objekte angepasst werden. Weitere Informationen finden Sie unter [xamarin. Forms CollectionView-Gruppierung](grouping.md). |
| Pull zum Aktualisieren | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Der Pull-Vorgang zum Aktualisieren wird in `CollectionView`derzeit nicht unterstützt, wird jedoch in einer zukünftigen Version hinzugefügt. |
| Kontextaktionen | `ContextActions` | Kontext Aktionen werden in `CollectionView`derzeit nicht unterstützt, werden jedoch in einer zukünftigen Version hinzugefügt. |
| Scrollen | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert `ScrollTo` Methoden, die Elemente in der Ansicht scrollen. Weitere Informationen finden Sie unter [scrollvorgänge](scrolling.md). |

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
