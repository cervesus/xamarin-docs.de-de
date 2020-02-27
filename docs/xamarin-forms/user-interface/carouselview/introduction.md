---
title: Einführung in xamarin. Forms carouselview
description: Carouselview ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer durch eine Auflistung von Elementen navigieren können.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 2c3e15ce68ad1507318a1d8155f9ab03095ea409
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635580"
---
# <a name="xamarinforms-carouselview-introduction"></a>Einführung in xamarin. Forms carouselview

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer eine Auflistung von Elementen durchlaufen können. Standardmäßig werden die Elemente `CarouselView` in horizontaler Ausrichtung angezeigt. Auf dem Bildschirm wird ein einzelnes Element angezeigt, das eine Schwenkbewegung und eine rückwärts Navigation durch die Auflistung von Elementen zur Folge hat. Außerdem können Indikatoren angezeigt werden, die die einzelnen Elemente im `CarouselView`darstellen:

[![Screenshot von "carouselview" und "indikatorview" unter IOS und Android](populate-data-images/indicators.png "Sichorview-Kreise")](populate-data-images/indicators-large.png#lightbox "Sichorview-Kreise")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) ist in xamarin. Forms 4,3 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, wenn Sie Ihrer `AppDelegate`-Klasse unter IOS oder ihrer `MainActivity`-Klasse unter Android die folgende Codezeile hinzufügen, bevor Sie `Forms.Init`aufrufen:

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) ist unter IOS und Android verfügbar, aber einige Funktionen sind möglicherweise nur teilweise auf dem universelle Windows-Plattform verfügbar.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) einen Großteil seiner Implementierung mit [`CollectionView`](xref:Xamarin.Forms.CollectionView). Die beiden Steuerelemente haben jedoch unterschiedliche Anwendungsfälle. `CollectionView` wird in der Regel verwendet, um Listen mit Daten beliebiger Länge darzustellen, wohingegen `CarouselView` normalerweise verwendet wird, um Informationen in einer Liste mit begrenzter Länge hervorzuheben.
