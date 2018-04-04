---
title: Hinter den Kulissen
description: Ein kurzer Blick auf der internen Funktionsweise von Xamarin.Mac
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 74721e880bb0d3ada3f3940a4074d06f55601c0e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="under-the-hood"></a>Hinter den Kulissen

_Ein kurzer Blick auf der internen Funktionsweise von Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Der Time-Kompilierung (AOT) fort](aot.md)

Zeit (AOT) ist im Voraus Kompilierung eine leistungsstarke Optimierungstechnik, die auch zur Verbesserung der Leistung beim Start an. Allerdings wirkt Sie sich auf auch die Buildzeit, Anwendungsgröße und Ausführung des Programms weitreichende Möglichkeiten, daher ist es sinnvoll Verständnis der Funktionsweise.

## <a name="mac-architecturearchitecturemd"></a>[Mac-Architektur](architecture.md)

Der Xamarin.Mac-Beziehung zu Objective-C, einschließlich Konzepten wie der Kompilierung, Selektoren Registrierungsstellen, app-Start und den Generator.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac schließt die Lücke zwischen der verwalteten Umgebung und des Kakao-Runtime verwaltete Klassen zum Aufrufen von nicht verwalteten Objective-C-Klassen und aufgerufen werden, wenn Ereignisse auftreten. Der Arbeitsaufwand zum Durchführen dieser "magische" müssen erfolgt durch die Registrierungsstelle allerdings wissen, was "hinter den Kulissen" kann in einigen Fällen hilfreich sein.
