---
title: Im Hintergrund in Xamarin.Mac
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, die die interne Funktionsweise von Xamarin.Mac zu beschreiben. Verknüpfte Dokumente werden vor der Time-Kompilierung, Xamarin.Mac-Architektur und der Registrierungsstelle Xamarin.Mac erläutert.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 872f26febf3abbe4d659773d2bf2d27348c64513
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118772"
---
# <a name="under-the-hood-in-xamarinmac"></a>Im Hintergrund in Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Jetzt der Time-Kompilierung (AOT)](aot.md)

Zeit (AOT) ist jetzt Kompilierung eine leistungsstarke Optimierungstechnik, die auch zur Verbesserung der Leistung beim Start an. Allerdings wirkt sich auf sie auch Ihre Buildzeit, Anwendungsgröße und Ausführung des Programms in fundierter Weise, daher ist es sinnvoll, ihn zu verstehen.

## <a name="mac-architecturearchitecturemd"></a>[Mac-Architektur](architecture.md)

Xamarin.Mac Beziehung mit Objective-C, einschließlich Konzepten wie Kompilierung, Selektoren, Registrierungsstellen, app-Start und des Generators.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac schließt die Lücke zwischen der verwalteten Welt und Cocoa Laufzeit ermöglicht verwaltete Klassen zum Aufrufen von nicht verwalteter Objective-C-Klassen und aufgerufen werden, wenn Ereignisse auftreten. Der erforderlichen Aufwand zum Durchführen dieser "magischen" wird von der Registrierungsstelle verarbeitet, aber zu verstehen, was "hinter den Kulissen" kann in einigen Fällen hilfreich sein.
