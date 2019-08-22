---
title: Xamarin. Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mithilfe unterschiedlicher layoutspezifikationen.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: b3841ed1287c980212ce37078f38f4984393c414
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888591"
---
# <a name="xamarinforms-collectionview"></a>Xamarin. Forms CollectionView

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Der [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine flexible und leistungsfähige Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen layoutspezifikationen.

## <a name="datapopulate-datamd"></a>[Daten](populate-data.md)

Ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung `IEnumerable`festgelegt wird, die implementiert. Die Darstellung der einzelnen Elemente in der Liste kann definiert werden [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), indem die-Eigenschaft auf festgelegt wird.

## <a name="layoutlayoutmd"></a>[Layout](layout.md)

Standardmäßig [`CollectionView`](xref:Xamarin.Forms.CollectionView) werden die Elemente in einer vertikalen Liste angezeigt. Es können jedoch vertikale und horizontale Listen und Raster angegeben werden.

## <a name="selectionselectionmd"></a>[Auswahl](selection.md)

Die [`CollectionView`](xref:Xamarin.Forms.CollectionView) Auswahl ist standardmäßig deaktiviert. Die Auswahl von einzeln und mehrfach kann jedoch aktiviert werden.

## <a name="empty-viewsemptyviewmd"></a>[Leere Ansichten](emptyview.md)

In [`CollectionView`](xref:Xamarin.Forms.CollectionView)kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrollingscrollingmd"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem [`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.

## <a name="groupinggroupingmd"></a>[Gruppierung](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann ordnungsgemäß gruppierte Daten anzeigen, `IsGrouped` indem Sie `true`die-Eigenschaft auf festlegen.
