---
title: Objective-C-Plattformen
description: Dieses Dokument beschreibt die verschiedenen Plattformen, die auf Einbetten von .NET bei der Arbeit mit Objective-C-Code ausgerichtet. Es wird erläutert, MacOS, iOS, TvOS und WatchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 8091fb4e8328f61f1471d061b51b4735de3c089c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108878"
---
# <a name="objective-c-platforms"></a>Objective-C-Plattformen

Einbetten von .NET können verschiedene Zielplattformen beim Generieren von Objective-C-Code:

* macOS
* iOS
* tvOS
* WatchOS [noch nicht implementiert]

Die Plattform ausgewählt ist, übergeben die `--platform=<platform>` Befehlszeilenargument für das Einbetten von .NET.

Beim Erstellen für iOS erstellt TvOS und WatchOS-Plattformen, Einbetten von .NET immer ein Framework, bei dem Xamarin.iOS-, eingebettet werden, da Xamarin.iOS viel Code der Common Language Runtime-Unterstützung enthält, die auf diesen Plattformen erforderlich ist.

Allerdings kann beim Erstellen für die MacOS-Plattform auswählen, ob das generierte Framework Xamarin.Mac oder nicht einbetten sollte. Es ist möglich, die keine Xamarin.Mac eingebettet, wenn die gebundene Assembly auf keine "xamarin.Mac.dll" verweist (entweder direkt oder indirekt), und dies, übergeben ausgewählt ist `--platform=macOS` an das Tool zum Einbetten von .NET.

Wenn die gebundene Assembly einen Verweis auf "xamarin.Mac.dll" enthält, es ist notwendig, betten Sie Xamarin.Mac und außerdem die Embeddinator muss wissen, welche die zu verwendenden Zielframeworks.

Es gibt drei mögliche Xamarin.Mac-Zielframeworks: `modern` (vorher: `mobile`), `full` und `system` (der Unterschied zwischen den einzelnen finden Sie auf der Xamarin.Mac [Zielframework] [ 1] Dokumentation), und jedes wird vom übergeben aktiviert `--platform=macOS-modern`, `--platform=macOS-full` oder `--platform=macOS-system` an das Tool zum Einbetten von .NET.

[1]: ~/mac/platform/target-framework.md
