---
title: Xamarin.Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 73e68f29c61661019cfc56f8caa26c6a0b1ce4ba
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971009"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Die [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.

## <a name="datapopulate-datamd"></a>[Data](populate-data.md)

Ein [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) mit Daten aufgefüllt wird, durch Festlegen seiner [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`. Die Darstellung der einzelnen Elemente in der Liste definiert werden, indem Sie die Einstellung der [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Layout](layout.md)

Standardmäßig eine [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) werden die Elemente in einer vertikalen Liste angezeigt. Allerdings können vertikale und horizontale Listen und Raster angegeben werden.

## <a name="selectionselectionmd"></a>[Auswahl](selection.md)

In der Standardeinstellung [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) Auswahl ist deaktiviert. Allerdings kann die Auswahl von einzelnen und mehreren aktiviert werden.

## <a name="empty-viewsemptyviewmd"></a>[Leere Ansichten](emptyview.md)

In [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), eine leere Ansicht kann angegeben werden, Feedback an den Benutzer bereitstellt, wenn keine Daten für die Anzeige verfügbar sind. Die leere Sicht kann es sich um eine Zeichenfolge, eine Sicht oder mehrere Ansichten sein.

## <a name="scrollingscrollingmd"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, kann die Endposition des Bildlaufs gesteuert werden, damit Elemente vollständig angezeigt werden. Darüber hinaus [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) definiert zwei [ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die programmgesteuert Elemente in der Ansicht zu scrollen. Eine der Überladungen führt einen Bildlauf durch das Element am angegebenen Index in der Ansicht, während die andere das angegebene Element in der Ansicht einen Bildlauf durchführt.
