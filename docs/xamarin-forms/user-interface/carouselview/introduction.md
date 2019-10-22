---
title: Einführung in xamarin. Forms carouselview
description: Carouselview ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer durch eine Auflistung von Elementen navigieren können.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 2fe4d984f36880493a9a04d99b63876551366477
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696974"
---
# <a name="xamarinforms-carouselview-introduction"></a>Einführung in xamarin. Forms carouselview

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer eine Auflistung von Elementen durchlaufen können. Standardmäßig werden die Elemente `CarouselView` in horizontaler Ausrichtung angezeigt. Auf dem Bildschirm wird ein einzelnes Element angezeigt, das eine Schwenkbewegung und eine rückwärts Navigation durch die Auflistung von Elementen zur Folge hat.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) ist in xamarin. Forms 4,3 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, wenn Sie Ihrer `AppDelegate`-Klasse unter IOS oder ihrer `MainActivity`-Klasse unter Android die folgende Codezeile hinzufügen, bevor Sie `Forms.Init` aufrufen:

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) ist unter IOS und Android verfügbar, aber einige Funktionen sind möglicherweise nur teilweise auf dem universelle Windows-Plattform verfügbar.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) einen Großteil seiner Implementierung mit [`CollectionView`](xref:Xamarin.Forms.CollectionView). Die beiden Steuerelemente haben jedoch unterschiedliche Anwendungsfälle. `CollectionView` wird in der Regel verwendet, um Listen von Daten in beliebiger Länge darzustellen, wohingegen `CarouselView` normalerweise verwendet wird, um Informationen in einer Liste mit begrenzter Länge hervorzuheben.
