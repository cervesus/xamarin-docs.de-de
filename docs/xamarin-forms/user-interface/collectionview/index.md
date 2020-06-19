---
title: 'Xamarin.Forms: CollectionView'
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a2c9fd9e6e48192bc2237d6b451b533fcee6e6ed
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136447"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms: CollectionView

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

Der [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen layoutspezifikationen.

## <a name="data"></a>[Daten](populate-data.md)

Ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird mit Daten aufgefüllt, indem seine- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft auf eine beliebige Auflistung festgelegt wird, die implementiert `IEnumerable` . Die Darstellung der einzelnen Elemente in der Liste kann definiert werden, indem die-Eigenschaft auf festgelegt wird [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[Layout](layout.md)

Standardmäßig [`CollectionView`](xref:Xamarin.Forms.CollectionView) werden die Elemente in einer vertikalen Liste angezeigt. Es können jedoch vertikale und horizontale Listen und Raster angegeben werden.

## <a name="selection"></a>[Auswahl](selection.md)

Die [`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl ist standardmäßig deaktiviert. Die Auswahl von einzeln und mehrfach kann jedoch aktiviert werden.

## <a name="empty-views"></a>[Leere Ansichten](emptyview.md)

In [`CollectionView`](xref:Xamarin.Forms.CollectionView) kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrolling"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem [`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.

## <a name="grouping"></a>[Bündelung](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann ordnungsgemäß gruppierte Daten anzeigen, indem Sie die- `IsGrouped` Eigenschaft auf festlegen `true` .
