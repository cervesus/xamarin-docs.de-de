---
title: Objective-C-Plattformen
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8c48a14e5d58f95bf93363e0b354fcf3afd94b44
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="objective-c-platforms"></a>Objective-C-Plattformen

.NET einbetten können verschiedene Zielplattformen beim Generieren von Objective-C-Code:

* macOS
* iOS
* tvOS
* WatchOS [noch nicht implementiert]

Die Plattform ausgewählt ist, durch das Übergeben der `--platform=<platform>` Befehlszeilenargument .NET einbetten.

Beim Erstellen für iOS erstellt tvos. außerdem wurden und WatchOS Plattformen .NET einbetten immer ein Framework, in die Xamarin.iOS, eingebettet werden, da Xamarin.iOS viele Unterstützung Laufzeitcode enthält, die auf diesen Plattformen erforderlich ist.

Allerdings kann beim Erstellen für die MacOS Plattform auswählen, ob das generierte Framework Xamarin.Mac einbetten soll. Es ist möglich, die keine Xamarin.Mac eingebettet, wenn gebundene Assembly Xamarin.Mac.dll nicht verweist (direkt oder indirekt), und dies durch Übergabe ausgewählt ist `--platform=macOS` an das Tool .NET einbetten.

Wenn die gebundene Assembly einen Verweis auf Xamarin.Mac.dll enthält, es ist notwendig, Xamarin.Mac einbetten und außerdem die Embeddinator muss wissen, welche Zielframework verwenden.

Es gibt drei mögliche Xamarin.Mac Zielframeworks: `modern` (zuvor aufgerufen `mobile`), `full` und `system` (die Differenz zwischen den einzelnen wird beschrieben, in der Xamarin.Mac [Zielframework] [ 1] Dokumentation), und jede ausgewählt ist, durch das übergeben `--platform=macOS-modern`, `--platform=macOS-full` oder `--platform=macOS-system` an das Tool .NET einbetten.

[1]: ~/mac/platform/target-framework.md
