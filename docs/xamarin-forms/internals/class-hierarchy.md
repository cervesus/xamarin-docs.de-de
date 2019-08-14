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
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984392"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin. Forms-Steuerelement-Klassenhierarchie

Xamarin. Forms besteht aus Hunderten von Typen und mehreren Namespaces. Entwickler sollten am häufigsten mit der Hierarchie von Typen vertraut sein, die zum Erstellen der Benutzeroberfläche einer xamarin. Forms-Anwendung verwendet wird `Xamarin.Forms` , die sich im-Namespace befinden.

Diese Typen können in Seiten, Layouts, Sichten und Zellen aufgeteilt werden. Eine xamarin. Forms-Seite nimmt im Allgemeinen den gesamten Bildschirm ein, und alle Seiten Typen [`Page`](xref:Xamarin.Forms.Page) werden von der-Klasse abgeleitet. Seiten enthalten in der Regel ein Layout, und alle Layouttypen werden [`Layout`](xref:Xamarin.Forms.Layout) von der-Klasse abgeleitet. Ein Layout enthält normalerweise Sichten und möglicherweise andere Layouts, und alle Ansichts Typen werden [`View`](xref:Xamarin.Forms.View) von der-Klasse abgeleitet. Schließlich handelt es sich bei Zellen um spezialisierte Steuerelemente, die in den [`TableView`](xref:Xamarin.Forms.TableView) Steuer [`ListView`](xref:Xamarin.Forms.ListView) Elementen und verwendet werden. Seiten, Layouts, Sichten und Zellen werden letztendlich von der [`Element`](xref:Xamarin.Forms.Element) -Klasse abgeleitet.

Das folgende Klassendiagramm zeigt die Hierarchie von Typen, die normalerweise verwendet werden, um eine Benutzeroberfläche in xamarin. Forms zu erstellen:

[ ![Xamarin. Forms-Steuerelemente Klassen]Diagramm(class-hierarchy-images/class-diagram.png "xamarin. Forms-Steuerelemente Klassendiagramm") ] (class-hierarchy-images/class-diagram-large.png#lightbox "Klassendiagramm für xamarin. Forms") -Steuerelemente

> [!NOTE]
> Eine hochauflösende Version des Klassendiagramms kann [hier](class-hierarchy-images/class-diagram-high-resolution.png)heruntergeladen werden. Beachten Sie jedoch, dass im Diagramm derzeit nicht die `CarouselView` Typen und `CollectionView` angezeigt werden. Diese werden hinzugefügt, wenn die Steuerelemente nicht mehr in der Vorschau angezeigt werden. Außerdem zeigt das Diagramm nur einen einzigen shelltyp an.

## <a name="related-links"></a>Verwandte Links

- [Referenz zu xamarin. Forms-Steuerelementen](~/xamarin-forms/user-interface/controls/index.md)
