---
title: Einführung in xamarin. Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 89afb0f2bfe93a5f78b0cd162f2a65e585b54b4b
ms.sourcegitcommit: 43423d4018cc0d4b0b8c98a4b3da0704495eb0cf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72303234"
---
# <a name="xamarinforms-collectionview-introduction"></a>Einführung in xamarin. Forms CollectionView

![Diese API ist zurzeit Vorabversion.](~/media/shared/preview.png)

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen Layoutspezifikationen. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView)bereitzustellen. Die folgenden Screenshots zeigen z. b. ein `CollectionView`, das ein vertikales Raster mit zwei Spalten verwendet und das mehrere Auswahlmöglichkeiten ermöglicht:

[![Screenshot des vertikalen Raster Layouts von CollectionView unter IOS und Android](introduction-images/verticalgrid-multipleselection.png "CollectionView vertikaler Raster Layout mit Mehrfachauswahl")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView vertikal Raster Layout mit Mehrfachauswahl")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist in xamarin. Forms 4,0 verfügbar. Sie ist jedoch zurzeit experimentell und kann nur verwendet werden, indem Sie die folgende Codezeile zu ihrer `AppDelegate`-Klasse unter IOS oder ihrer `MainActivity`-Klasse unter Android oder `App.xaml.cs` auf der UWP hinzufügen, bevor Sie `Forms.Init` aufrufen:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist unter IOS und Android verfügbar, aber nur teilweise auf dem universelle Windows-Plattform [verfügbar](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) .

## <a name="collectionview-and-listview-differences"></a>Unterschiede zwischen CollectionView und ListView

Obwohl die APIs [`CollectionView`](xref:Xamarin.Forms.CollectionView) und [`ListView`](xref:Xamarin.Forms.ListView) ähnlich sind, gibt es einige wichtige Unterschiede:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) verfügt über ein flexibles Layoutmodell, das die vertikale oder horizontale Darstellung von Daten in einer Liste oder einem Raster ermöglicht.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt eine einzelne und mehrere Auswahl.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) verwendet automatisch die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt wird.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) reduziert die API-Oberfläche von [`ListView`](xref:Xamarin.Forms.ListView). Viele Eigenschaften und Ereignisse von [`ListView`](xref:Xamarin.Forms.ListView) sind in `CollectionView` nicht vorhanden.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) enthält keine integrierten Trennzeichen.

## <a name="move-from-listview-to-collectionview"></a>Aus ListView in CollectionView verschieben

[`ListView`-](xref:Xamarin.Forms.ListView) Implementierungen in vorhandenen xamarin. Forms-Implementierungen können mithilfe der folgenden Tabelle zu [`CollectionView`-](xref:Xamarin.Forms.CollectionView) Implementierungen migriert werden:

| Konzept | ListView-API | CollectionView |
|---|---|---|
| Daten | `ItemsSource` | Eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird durch Festlegen der `ItemsSource`-Eigenschaft mit Daten aufgefüllt. Weitere Informationen finden Sie unter Auffüllen [einer CollectionView mit Daten](populate-data.md#populate-a-collectionview-with-data). |
| Element Darstellung | `ItemTemplate` | Die Darstellung der einzelnen Elemente in einer [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann definiert werden, indem die `ItemTemplate`-Eigenschaft auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird. Weitere Informationen finden Sie unter [Definieren der Element](populate-data.md#define-item-appearance)Darstellung. |
| Zellen | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) hat kein Konzept von Zellen. Stattdessen wird eine Daten Vorlage verwendet, um die Darstellung der einzelnen Datenelemente in der Liste zu definieren. |
| Zeilen Trennzeichen | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) enthält keine integrierten Trennzeichen. Diese können, falls gewünscht, in der Element Vorlage angegeben werden. |
| Auswahl | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) unterstützt eine einzelne und mehrere Auswahl. Weitere Informationen finden Sie unter [xamarin. Forms CollectionView-Auswahl](selection.md). |
| Zeilenhöhe | `HasUnevenRows`, `RowHeight` | In einem `CollectionView` wird die Zeilenhöhe jedes Elements durch die `ItemSizingStrategy`-Eigenschaft bestimmt. Weitere Informationen finden Sie unter [Item Sizing](layout.md#item-sizing).|
| Zwischenspeicherung | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) verwendet automatisch die Virtualisierung, die von den zugrunde liegenden systemeigenen Steuerelementen bereitgestellt wird. |
| Kopf- und Fußzeilen | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann eine Kopf-und Fußzeile darstellen, die mit den Elementen in der Liste über die Eigenschaften "`Header`", "`Footer`", "`HeaderTemplate`" und "`FooterTemplate`" scrollen. Weitere Informationen finden Sie unter [Kopf-und Fußzeilen](layout.md#headers-and-footers). |
| Gruppieren | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt ordnungsgemäß gruppierte Daten an, indem die `IsGrouped`-Eigenschaft auf `true` festgelegt wird. Gruppen Kopfzeilen und-Fußzeilen können angepasst werden, indem die Eigenschaften `GroupHeaderTemplate` und `GroupFooterTemplate` auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) -Objekte festgelegt werden. Weitere Informationen finden Sie unter [xamarin. Forms CollectionView-Gruppierung](grouping.md). |
| Pull zum Aktualisieren | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Der Pull-Vorgang zum Aktualisieren wird in `CollectionView` derzeit nicht unterstützt, wird jedoch in einer zukünftigen Version hinzugefügt. |
| Kontextaktionen | `ContextActions` | Kontext Aktionen werden derzeit in `CollectionView` nicht unterstützt, werden jedoch in einer zukünftigen Version hinzugefügt. |
| Scrollen | `ScrollTo()` | in [`CollectionView`](xref:Xamarin.Forms.CollectionView) werden `ScrollTo`-Methoden definiert, mit denen Elemente in der Ansicht angezeigt werden. Weitere Informationen finden Sie unter [scrollvorgänge](scrolling.md). |

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
