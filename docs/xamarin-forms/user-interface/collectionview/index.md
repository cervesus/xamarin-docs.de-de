---
title: Xamarin.Forms CollectionView
description: Die CollectionView ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a6cb6e695a4c19627b183060a7636320f8083ee2
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048143"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Die `CollectionView` ist eine flexible und leistungsfähige Ansicht zur Darstellung von Listen mit Daten, die mit anderen Layout-Spezifikationen.

## <a name="datapopulate-datamd"></a>[Data](populate-data.md)

Ein `CollectionView` mit Daten aufgefüllt wird, durch Festlegen seiner `ItemsSource` Eigenschaft, um eine beliebige Sammlung, die implementiert `IEnumerable`. Die Darstellung der einzelnen Elemente in der Liste definiert werden, indem Sie die Einstellung der `ItemTemplate` Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Layout](layout.md)

Standardmäßig eine `CollectionView` werden die Elemente in einer vertikalen Liste angezeigt. Allerdings können vertikale und horizontale Listen und Raster angegeben werden.

## <a name="selectionselectionmd"></a>[Auswahl](selection.md)

In der Standardeinstellung `CollectionView` Auswahl ist deaktiviert. Allerdings kann die Auswahl von einzelnen und mehreren aktiviert werden.

## <a name="empty-viewsemptyviewmd"></a>[Leere Ansichten](emptyview.md)

In `CollectionView`, eine leere Ansicht kann angegeben werden, Feedback an den Benutzer bereitstellt, wenn keine Daten für die Anzeige verfügbar sind. Die leere Sicht kann es sich um eine Zeichenfolge, eine Sicht oder mehrere Ansichten sein.

## <a name="scrollingscrollingmd"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, kann die Endposition des Bildlaufs gesteuert werden, damit Elemente vollständig angezeigt werden. Darüber hinaus `CollectionView` definiert zwei `ScrollTo` Methoden, die programmgesteuert Elemente in der Ansicht zu scrollen. Eine der Überladungen führt einen Bildlauf durch das Element am angegebenen Index in der Ansicht, während die andere das angegebene Element in der Ansicht einen Bildlauf durchführt.
