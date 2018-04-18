---
title: Objective-C-Plattformen
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 7dc4dc4ea0ab55ff603b9e8d6fa905336159222a
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="objective-c-platforms"></a>Objective-C-Plattformen


.NET einbetten können verschiedene Zielplattformen beim Generieren von Objective-C-Code:

* macOS
* iOS
* tvOS
* WatchOS [noch nicht implementiert]

Die Plattform ist ausgewählt, durch Übergeben der `--platform=<platform>` Befehlszeilenargument die Embeddinator.

Beim Erstellen für iOS, tvos. außerdem wurden und WatchOS-Plattformen erstellt die Embeddinator immer ein Framework, in die Xamarin.iOS, eingebettet werden, da Xamarin.iOS viele Unterstützung Laufzeitcode enthält, die auf diesen Plattformen erforderlich ist.

Allerdings kann beim Erstellen für die MacOS Plattform auswählen, ob das generierte Framework Xamarin.Mac einbetten soll. Es ist möglich, die keine Xamarin.Mac eingebettet, wenn gebundene Assembly Xamarin.Mac.dll nicht verweist (direkt oder indirekt), und dies durch Übergabe ausgewählt ist `--platform=macOS` auf die Embeddinator.

Wenn die gebundene Assembly einen Verweis auf Xamarin.Mac.dll enthält, es ist notwendig, Xamarin.Mac einbetten und außerdem die Embeddinator muss wissen, welche Zielframework verwenden.

Es gibt drei mögliche Xamarin.Mac Zielframeworks: `modern` (zuvor aufgerufen `mobile`), `full` und `system` (die Differenz zwischen den einzelnen wird beschrieben, in der Xamarin.Mac [Zielframework] [ 1] Dokumentation), und jede ausgewählt ist, durch das übergeben `--platform=macOS-modern`, `--platform=macOS-full` oder `--platform=macOS-system` auf die Embeddinator.

[1]: ~/mac/platform/target-framework.md
