---
title: Hinter den Kulissen in Xamarin.Mac
description: Dieses enthält Dokumentenlinks zu verschiedenen Handbüchern, die der internen Funktionsweise von Xamarin.Mac beschreiben. Verknüpfte Dokumente werden vor der Kompilierung, Xamarin.Mac-Architektur und die Registrierungsstelle Xamarin.Mac erläutert.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792488"
---
# <a name="under-the-hood-in-xamarinmac"></a>Hinter den Kulissen in Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Der Time-Kompilierung (AOT) fort](aot.md)

Zeit (AOT) ist im Voraus Kompilierung eine leistungsstarke Optimierungstechnik, die auch zur Verbesserung der Leistung beim Start an. Allerdings wirkt Sie sich auf auch die Buildzeit, Anwendungsgröße und Ausführung des Programms weitreichende Möglichkeiten, daher ist es sinnvoll Verständnis der Funktionsweise.

## <a name="mac-architecturearchitecturemd"></a>[Mac-Architektur](architecture.md)

Der Xamarin.Mac-Beziehung zu Objective-C, einschließlich Konzepten wie der Kompilierung, Selektoren Registrierungsstellen, app-Start und den Generator.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac schließt die Lücke zwischen der verwalteten Umgebung und des Kakao-Runtime verwaltete Klassen zum Aufrufen von nicht verwalteten Objective-C-Klassen und aufgerufen werden, wenn Ereignisse auftreten. Der Arbeitsaufwand zum Durchführen dieser "magische" müssen erfolgt durch die Registrierungsstelle allerdings wissen, was "hinter den Kulissen" kann in einigen Fällen hilfreich sein.
