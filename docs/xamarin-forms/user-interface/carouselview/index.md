---
title: Xamarin. Forms carouselview
description: Carouselview ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer durch eine Auflistung von Elementen navigieren können.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 816c1b6e4ab497d0ada0f80fa3ad4800912587c3
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696986"
---
# <a name="xamarinforms-carouselview"></a>Xamarin. Forms carouselview

![](~/media/shared/preview.png "This API is currently pre-release")

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Der [`CarouselView`](xref:Xamarin.Forms.CarouselView) ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer eine Auflistung von Elementen durchlaufen können.

## <a name="datapopulate-datamd"></a>[Data](populate-data.md)

Ein [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung festgelegt wird, die `IEnumerable` implementiert. Die Darstellung der einzelnen Elemente kann durch Festlegen der [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)definiert werden.

## <a name="layoutlayoutmd"></a>[Layout](layout.md)

Standardmäßig werden die Elemente in einer [`CarouselView`](xref:Xamarin.Forms.CarouselView) in einer horizontalen Liste angezeigt. Sie hat jedoch auch Zugriff auf dieselben Layouts wie CollectionView, einschließlich vertikaler Ausrichtung.

## <a name="interactioninteractionmd"></a>[Interaktion](interaction.md)

Auf das aktuell angezeigte Element in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann über die Eigenschaften `CurrentItem` und `Position` zugegriffen werden.

## <a name="empty-viewsemptyviewmd"></a>[Leere Ansichten](emptyview.md)

In [`CarouselView`](xref:Xamarin.Forms.CarouselView)kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrollingscrollingmd"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem definiert [`CarouselView`](xref:Xamarin.Forms.CarouselView) zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.
