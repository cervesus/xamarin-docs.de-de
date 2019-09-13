---
title: Xamarin. Forms carouselview
description: Carouselview ist eine Ansicht zum Darstellen von Listen mit Daten in einem Karussell ähnlichen Layout.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 06d20341053c4f77cb72aac9e1abdc85d9f95fdb
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908331"
---
# <a name="xamarinforms-carouselview"></a>Xamarin. Forms carouselview

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

> [!IMPORTANT]
> Die Dokumentation zu carouselview befindet sich in der Entwicklungsphase, und einige Links verweisen auf die CollectionView-Dokumentation. In den meisten Fällen sollten die Informationen anwendbar sein, weil carouselview auf der Grundlage von CollectionView basiert.

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Ist [`CarouselView`](xref:Xamarin.Forms.CarouselView) eine Ansicht für die Darstellung von Daten in einem Karussell ähnlichen Layout. Die Implementierung basiert auf der von [`CollectionView`](xref:Xamarin.Forms.CollectionView).

## <a name="datacollectionviewpopulate-datamd"></a>[Daten](../collectionview/populate-data.md)

Ein [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung `IEnumerable`festgelegt wird, die implementiert. Die Darstellung der einzelnen Elemente in der Liste kann definiert werden [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), indem die-Eigenschaft auf festgelegt wird.

## <a name="layoutlayoutmd"></a>[Layout](layout.md)

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) werden die Elemente in einer horizontalen Liste angezeigt. Sie hat jedoch auch Zugriff auf dieselben Layouts wie CollectionView, einschließlich vertikaler Ausrichtung.

## <a name="empty-viewscollectionviewemptyviewmd"></a>[Leere Ansichten](../collectionview/emptyview.md)

In [`CarouselView`](xref:Xamarin.Forms.CarouselView)kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrollingcollectionviewscrollingmd"></a>[Scrollen](../collectionview/scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem [`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.
