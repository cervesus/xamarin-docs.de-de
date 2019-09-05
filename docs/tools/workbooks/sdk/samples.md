---
title: Beispielintegrationen
description: Dieses Dokument stellt Links zu Beispielen dar, die Xamarin Workbooks Integrationen veranschaulichen. Verknüpfte Beispiele funktionieren mit Darstellungs Rendering und skiasharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: fbe471aa7f08d85a870d68505cf2c983b7e442e9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292813"
---
# <a name="sample-integrations"></a>Beispielintegrationen

Ein funktionierendes Beispiel für eine-Integration finden Sie im Beispiel " [Kitchen Sink][KitchenSink] ". Erstellen `KitchenSink.sln` Sie einfach in Visual Studio für Mac oder Visual Studio, und `KitchenSink.workbook`öffnen Sie dann.

[![Bildschirm Abbildung der Küchen Senke Integration](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Das Beispiel "Kitchen Sink" veranschaulicht beide Sätze von Konzepten:

* Die Darstellungs Teile veranschaulichen, wie `RepresentationManager` verwendet wird, um das Rendering mithilfe der integrierten Darstellungen zu verbessern.
* Das `Person` -Objekt und der zugehörige JavaScript-Renderer veranschaulichen mithilfe `ISerializableObject` von, ohne einen Darstellungs Anbieter zu durchlaufen.

Siehe auch [skiasharp][skiasharp] für ein praktisches Beispiel für eine Integration, bei der die vorhandenen von Xamarin Workbooks bereitgestellten [Darstellungen](~/tools/workbooks/sdk/representations.md) zum Rendering der zugehörigen Typen verwendet werden.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
