---
title: Einführung in xamarin. Forms carouselview
description: Carouselview ist eine Ansicht für die Darstellung von Daten in einem Karussell ähnlichen Layout.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 7979f6ed362c580d9cf80f19b3bc0ea7550ca70c
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985974"
---
# <a name="xamarinforms-carouselview-introduction"></a>Einführung in xamarin. Forms carouselview

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)ist eine Ansicht zur Darstellung von Informationen auf Karussell ähnliche Weise.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)ist in xamarin. Forms 4,3 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem der- `AppDelegate` Klasse unter IOS oder `MainActivity` der-Klasse unter Android die folgende Codezeile hinzugefügt wird, bevor `Forms.Init`aufgerufen wird:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

_Hinweis: Zum Zeitpunkt der Erstellung dieses Artikels verwendet carouselview weiterhin das "CollectionView"-Flag, um seine Funktionalität zu aktivieren._

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)ist unter IOS und Android verfügbar, einige Funktionen sind jedoch möglicherweise nur teilweise auf dem universelle Windows-Plattform verfügbar.

## <a name="when-to-use-carouselview"></a>Verwendung von carouselview

Obwohl die carouselview einen Großteil ihrer Implementierung auf CollectionView basiert, ist Ihr Zweck eher fokussiert. Obwohl die Verwendung von CollectionView und carouselview nach eigenem Ermessen verwendet wird, werden in der Regel die über Ladungen in Apps verwendet, um Informationen hervorzuheben, und Sie sind in der Gesamtzahl der verwendeten Elemente beschränkt.
