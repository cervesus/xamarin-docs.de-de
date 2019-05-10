---
title: Einführung in die Xamarin.Forms-CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 23de838a395b68f656f95c5665dc97a1d928bece
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054650"
---
# <a name="xamarinforms-collectionview-introduction"></a>Einführung in die Xamarin.Forms-CollectionView

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` ist eine Ansicht für die Darstellung von Listen mit Daten anderes Layout-Spezifikationen verwenden. Es zielt darauf ab, die eine flexiblere, bereitstellen und leistungsfähige Alternative zu [ `ListView` ](xref:Xamarin.Forms.ListView). Z. B. die folgenden Screenshots zeigen eine `CollectionView` , die ein vertikale Raster mit zwei Spalten und dem ermöglicht Mehrfachauswahl:

[![Screenshot des einem vertikalen CollectionView Rasterlayout unter iOS und Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertikale Rasterlayout mit Mehrfachauswahl")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertikale Rasterlayout mit Mehrfachauswahl")

`CollectionView` ist in der Xamarin.Forms-4.0-Vorabversionen verfügbar. Allerdings es ist derzeit experimentell und kann nur verwendet werden, durch die folgende Zeile des Codes zum Hinzufügen Ihrer `AppDelegate` Klasse unter iOS, oder um Ihre `MainActivity` Klasse, die unter Android werden vor dem Aufruf `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> `CollectionView` ist nur unter iOS und Android verfügbar.

## <a name="collectionview-and-listview-differences"></a>Unterschiede zwischen CollectionView und ListView

Während der `CollectionView` und `ListView` APIs sind ähnlich, es gibt einige wichtige Unterschiede:

- `CollectionView` verfügt über ein flexibles Layout-Modell, die Daten vertikal oder horizontal in einer Liste oder einem Raster angezeigt werden kann.
- `CollectionView` unterstützt Einzel- und Mehrfachauswahl.
- `CollectionView` verfügt über kein Konzept für die Zellen aus. Stattdessen wird eine Datenvorlage verwendet, um die Darstellung der einzelnen Elemente der Daten in der Liste zu definieren.
- `CollectionView` automatisch nutzt die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt.
- `CollectionView` reduziert die API-Oberfläche des [ `ListView` ](xref:Xamarin.Forms.ListView). Viele Eigenschaften und Ereignisse von `ListView` sind nicht im `CollectionView`.
- `CollectionView` umfasst keine integrierte Trennzeichen.

## <a name="move-from-listview-to-collectionview"></a>Verschieben von ListView zu CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) Implementierungen in vorhandenen Xamarin.Forms-Implementierungen können zu migriert werden `CollectionView` Implementierungen mithilfe der folgenden Tabelle:

| Konzept | ListView-API | CollectionView |
|---|---|---|
| Daten | `ItemsSource` | Ein `CollectionView` mit Daten aufgefüllt wird, durch Festlegen seiner `ItemsSource` Eigenschaft. Weitere Informationen finden Sie unter [eine CollectionView mit Daten auffüllen](populate-data.md#populate-a-collectionview-with-data). |
| Element-Darstellung | `ItemTemplate` | Die Darstellung der einzelnen Elemente in einer `CollectionView` definiert werden, indem Sie die Einstellung der `ItemTemplate` Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Weitere Informationen finden Sie unter [Element Darstellung definieren](populate-data.md#define-item-appearance). |
| Zellen | `TextCell`, `ImageCell`, `ViewCell` | `CollectionView` verfügt über kein Konzept für die Zellen aus. Stattdessen wird eine Datenvorlage verwendet, um die Darstellung der einzelnen Elemente der Daten in der Liste zu definieren. |
| Zeilentrennzeichen | `SeparatorColor`, `SeparatorVisibility` | `CollectionView` umfasst keine integrierte Trennzeichen. Diese können angegeben werden, falls gewünscht, in der Elementvorlage. |
| Auswahl | `SelectionMode`, `SelectedItem` | `CollectionView` unterstützt Einzel- und Mehrfachauswahl. Weitere Informationen finden Sie unter [Xamarin.Forms CollectionView Auswahl](selection.md). |
| Zeilenhöhe | `HasUnevenRows`, `RowHeight` | In einem `CollectionView`, die Höhe jedes Elements richtet sich nach der `ItemSizingStrategy` Eigenschaft. Weitere Informationen finden Sie unter [Element größenanpassung](layout.md#item-sizing).|
| Zwischenspeicherung | `CachingStrategy` | `CollectionView` verwendet automatisch die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt. |
| Kopf- und Fußzeilen | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Kopf- und Fußzeilen werden derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden.|
| Gruppieren | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Gruppierung wird derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden. |
| Zum Aktualisieren nach unten ziehen | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Zum Aktualisieren nach unten ziehen wird derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden. |
| Kontextaktionen | `ContextActions` | Kontextaktionen werden derzeit nicht unterstützt `CollectionView`, jedoch wird in einer zukünftigen Version hinzugefügt werden. |
| Scrollen | `ScrollTo()` | `CollectionView` definiert `ScrollTo` Methoden, die Elemente in der Ansicht zu scrollen. Weitere Informationen finden Sie unter [fortlaufender](scrolling.md). |

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
