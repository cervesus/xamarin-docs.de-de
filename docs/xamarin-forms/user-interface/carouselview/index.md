---
title: Xamarin.FormsCarouselview
description: Carouselview ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer durch eine Auflistung von Elementen navigieren können.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 40b918adff523fa446e69c064029311c54d01290
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935043"
---
# <a name="xamarinforms-carouselview"></a>Xamarin.FormsCarouselview

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

## <a name="introduction"></a>[Einführung](introduction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer zum Durchlaufen einer Auflistung von Elementen schwenken können.

## <a name="data"></a>[Daten](populate-data.md)

Ein [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird mit Daten aufgefüllt, indem seine- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft auf eine beliebige Auflistung festgelegt wird, die implementiert `IEnumerable` . Die Darstellung der einzelnen Elemente kann definiert werden, indem die-Eigenschaft auf festgelegt wird [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[Layout](layout.md)

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) werden die Elemente in einer horizontalen Liste angezeigt. Sie hat jedoch auch Zugriff auf dieselben Layouts wie CollectionView, einschließlich vertikaler Ausrichtung.

## <a name="interaction"></a>[Interaktion](interaction.md)

Auf das aktuell angezeigte Element in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann über die-Eigenschaft und die-Eigenschaft zugegriffen werden `CurrentItem` `Position` .

## <a name="empty-views"></a>[Leere Ansichten](emptyview.md)

In [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrolling"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem [`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.
