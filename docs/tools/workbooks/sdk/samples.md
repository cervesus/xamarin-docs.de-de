---
title: Beispielintegrationen
description: Dieses Dokument stellt Links zu Beispielen dar, die Xamarin Workbooks Integrationen veranschaulichen. Verknüpfte Beispiele funktionieren mit Darstellungs Rendering und skiasharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: fa66ee2b2b469900381e2d31dc5dec49eb42c4f6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018119"
---
# <a name="sample-integrations"></a>Beispielintegrationen

Ein funktionierendes Beispiel für eine-Integration finden Sie im Beispiel " [Kitchen Sink][KitchenSink] ". Erstellen Sie einfach `KitchenSink.sln` in Visual Studio für Mac oder Visual Studio, und öffnen Sie dann `KitchenSink.workbook`.

[Bildschirm Abbildung der![Küche-Senke](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Das Beispiel "Kitchen Sink" veranschaulicht beide Sätze von Konzepten:

* Die Darstellungs Teile veranschaulichen, wie `RepresentationManager` verwendet werden, um das Rendering mithilfe der integrierten Darstellungen zu verbessern.
* Das `Person`-Objekt und der zugehörige JavaScript-Renderer veranschaulichen die Verwendung `ISerializableObject`, ohne einen Darstellungs Anbieter durchlaufen zu müssen.

Siehe auch [skiasharp][skiasharp] für ein praktisches Beispiel für eine Integration, bei der die vorhandenen von Xamarin Workbooks bereitgestellten [Darstellungen](~/tools/workbooks/sdk/representations.md) zum Rendering der zugehörigen Typen verwendet werden.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
