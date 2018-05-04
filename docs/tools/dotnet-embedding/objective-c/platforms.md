---
title: Objective-C-Plattformen
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 22652baa941debf7ded2d43cfda8c95ec474816f
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
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
