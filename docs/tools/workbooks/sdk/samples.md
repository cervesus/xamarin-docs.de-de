---
title: Beispielintegrationen
description: Dieses Dokument enthält Links zu Beispielen, die die Integration von Xamarin Workbooks zu veranschaulichen. Verknüpfte Beispiele funktionieren mit Ihrer Darstellung in Rendering und SkiaSharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: e35577b116180d2745e2f6afb792547f63873214
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61424600"
---
# <a name="sample-integrations"></a>Beispielintegrationen

Finden Sie unter den [Küche Senke] [ KitchenSink] Beispiel ein funktionsfähiges Beispiel eines Integration. Erstellen Sie einfach `KitchenSink.sln` in Visual Studio für Mac oder Visual Studio und öffnen Sie dann `KitchenSink.workbook`.

[![Bildschirmabbildung von Küche Senke-Integration](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Die Senke Küche veranschaulicht beide Sätze von Konzepten:

* Die Teile der Darstellung wird veranschaulicht, wie mit `RepresentationManager` , der Rendering zu verbessern, indem Sie die integrierte Darstellungen.
* Die `Person` -Objekt und seine zugeordneten JavaScript-Renderer veranschaulicht die Verwendung `ISerializableObject` ohne Umweg über einen Anbieter für die Darstellung.

Siehe auch [SkiaSharp] [ skiasharp] für ein praktisches Beispiel eines Integration, die die vorhandene verwendet [Darstellungen](~/tools/workbooks/sdk/representations.md) von Xamarin Workbooks zum Rendern seiner Typen bereitgestellt.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
