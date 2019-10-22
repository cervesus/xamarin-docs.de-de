---
title: Xamarin. Forms-Steuerelement-Klassenhierarchie
description: Entwickler sollten mit der Hierarchie von Typen vertraut sein, die zum Erstellen der Benutzeroberfläche einer xamarin. Forms-Anwendung verwendet werden.
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2019
ms.openlocfilehash: f08146d4439ff1fc22edea71ab1cbb337f64c037
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68984392"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin. Forms-Steuerelement-Klassenhierarchie

Xamarin. Forms besteht aus Hunderten von Typen und mehreren Namespaces. Entwickler sollten am häufigsten mit der Hierarchie von Typen vertraut sein, die zum Erstellen der Benutzeroberfläche einer xamarin. Forms-Anwendung verwendet wird, die sich im `Xamarin.Forms`-Namespace befindet.

Diese Typen können in Seiten, Layouts, Sichten und Zellen aufgeteilt werden. Eine xamarin. Forms-Seite nimmt im Allgemeinen den gesamten Bildschirm ein, und alle Seiten Typen werden von der [`Page`](xref:Xamarin.Forms.Page) -Klasse abgeleitet. Seiten enthalten in der Regel ein Layout, und alle Layouttypen werden von der [`Layout`](xref:Xamarin.Forms.Layout) -Klasse abgeleitet. Ein Layout enthält normalerweise Sichten und möglicherweise andere Layouts, und alle Ansichts Typen werden von der [`View`](xref:Xamarin.Forms.View) -Klasse abgeleitet. Schließlich handelt es sich bei Zellen um spezialisierte Steuerelemente, die in den [`TableView`](xref:Xamarin.Forms.TableView) -und [`ListView`](xref:Xamarin.Forms.ListView) -Steuerelementen verwendet werden. Seiten, Layouts, Sichten und Zellen werden letztendlich von der [`Element`](xref:Xamarin.Forms.Element) -Klasse abgeleitet.

Das folgende Klassendiagramm zeigt die Hierarchie von Typen, die normalerweise verwendet werden, um eine Benutzeroberfläche in xamarin. Forms zu erstellen:

[![Klassendiagramm für xamarin. Forms-Steuerelemente](class-hierarchy-images/class-diagram.png "Klassendiagramm für xamarin. Forms-Steuerelemente")](class-hierarchy-images/class-diagram-large.png#lightbox "Klassendiagramm für xamarin. Forms-Steuerelemente")

> [!NOTE]
> Eine hochauflösende Version des Klassendiagramms kann [hier](class-hierarchy-images/class-diagram-high-resolution.png)heruntergeladen werden. Beachten Sie jedoch, dass die `CarouselView`-und `CollectionView` Typen im Diagramm derzeit nicht angezeigt werden. Diese werden hinzugefügt, wenn die Steuerelemente nicht mehr in der Vorschau angezeigt werden. Außerdem zeigt das Diagramm nur einen einzigen shelltyp an.

## <a name="related-links"></a>Verwandte Links

- [Referenz zu xamarin. Forms-Steuerelementen](~/xamarin-forms/user-interface/controls/index.md)
