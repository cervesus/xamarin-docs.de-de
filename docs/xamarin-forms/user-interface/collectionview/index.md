---
title: Xamarin. Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 8050d952556ce0b55a7ce72bc5f25de903fee6e5
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697000"
---
# <a name="xamarinforms-collectionview"></a>Xamarin. Forms CollectionView

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Der [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.

## <a name="datapopulate-datamd"></a>[Data](populate-data.md)

Ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung festgelegt wird, die `IEnumerable` implementiert. Die Darstellung der einzelnen Elemente in der Liste kann durch Festlegen der [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)definiert werden.

## <a name="layoutlayoutmd"></a>[Layout](layout.md)

Standardmäßig zeigt ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) seine Elemente in einer vertikalen Liste an. Es können jedoch vertikale und horizontale Listen und Raster angegeben werden.

## <a name="selectionselectionmd"></a>[Auswahl](selection.md)

Standardmäßig ist [`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl deaktiviert. Die Auswahl von einzeln und mehrfach kann jedoch aktiviert werden.

## <a name="empty-viewsemptyviewmd"></a>[Leere Ansichten](emptyview.md)

In [`CollectionView`](xref:Xamarin.Forms.CollectionView)kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrollingscrollingmd"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem definiert [`CollectionView`](xref:Xamarin.Forms.CollectionView) zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.

## <a name="groupinggroupingmd"></a>[Gruppierung](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können ordnungsgemäß gruppierte Daten anzeigen, indem Sie die `IsGrouped`-Eigenschaft auf `true` festlegen.
