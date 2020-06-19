---
title: Xamarin.FormsControls-Klassenhierarchie
description: Entwickler sollten mit der Hierarchie von Typen vertraut sein, die zum Erstellen der Benutzeroberfläche einer-Anwendung verwendet werden Xamarin.Forms .
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0087e2bb81c7c9204a782519a9eeb9891adc297a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138637"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin.FormsControls-Klassenhierarchie

Xamarin.Formsbesteht aus Hunderten von Typen, über mehrere Namespaces. Entwickler sollten am häufigsten mit der Hierarchie von Typen vertraut sein, die zum Erstellen der Benutzeroberfläche einer-Anwendung verwendet werden Xamarin.Forms , die sich im- `Xamarin.Forms` Namespace befinden.

Diese Typen können in Seiten, Layouts, Sichten und Zellen aufgeteilt werden. Eine Xamarin.Forms Seite nimmt in der Regel den gesamten Bildschirm ein, und alle Seiten Typen werden von der- [`Page`](xref:Xamarin.Forms.Page) Klasse abgeleitet. Seiten enthalten in der Regel ein Layout, und alle Layouttypen werden von der- [`Layout`](xref:Xamarin.Forms.Layout) Klasse abgeleitet. Ein Layout enthält normalerweise Sichten und möglicherweise andere Layouts, und alle Ansichts Typen werden letztendlich von der- [`View`](xref:Xamarin.Forms.View) Klasse abgeleitet. Schließlich handelt es sich bei Zellen um spezialisierte Steuerelemente, die in den Steuer [`TableView`](xref:Xamarin.Forms.TableView) Elementen und verwendet werden [`ListView`](xref:Xamarin.Forms.ListView) . Seiten, Layouts, Sichten und Zellen werden letztendlich von der-Klasse abgeleitet [`Element`](xref:Xamarin.Forms.Element) .

Das folgende Klassendiagramm zeigt die Hierarchie von Typen, die in der Regel verwendet werden, um eine Benutzeroberfläche in zu erstellen Xamarin.Forms :

[![Xamarin.FormsSteuerelement-Klassendiagramm](class-hierarchy-images/class-diagram.png "[! Schel. Klassendiagramm für No-Loc (xamarin. Forms)]-Steuerelemente")](class-hierarchy-images/class-diagram-large.png#lightbox "[! Schel. Klassendiagramm für No-Loc (xamarin. Forms)]-Steuerelemente")

> [!NOTE]
> Eine hochauflösende Version des Klassendiagramms kann [hier](class-hierarchy-images/class-diagram-high-resolution.png)heruntergeladen werden. Beachten Sie jedoch, dass das Diagramm nur einen einzigen shelltyp anzeigt.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsSteuerelement Referenz](~/xamarin-forms/user-interface/controls/index.md)
