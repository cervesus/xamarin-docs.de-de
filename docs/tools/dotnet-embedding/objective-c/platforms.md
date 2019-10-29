---
title: Ziel-C-Plattformen
description: In diesem Dokument werden die verschiedenen Plattformen beschrieben, die .net-Einbettungen beim Arbeiten mit Ziel-C-Code als Ziel haben. Hier werden macOS, Ios, tvos und watchos erläutert.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: dbc2c40e06f14ec636b4aa4cc11fc4f2d230ee6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006422"
---
# <a name="objective-c-platforms"></a>Ziel-C-Plattformen

.Net-Einbettungen können bei der Erstellung von Ziel-C-Code auf verschiedene Plattformen abzielen

* macOS
* iOS
* tvOS
* watchos [noch nicht implementiert]

Die Plattform wird ausgewählt, indem das `--platform=<platform>` Befehlszeilenargument an .net Einbettungen übergeben wird.

Beim Erstellen für IOS-, tvos-und watchos-Plattformen erstellt .net-Einbettungen immer ein Framework, das xamarin. IOS einbettet, da xamarin. IOS viele Lauf Zeit Unterstützungs Codes enthält, die auf diesen Plattformen erforderlich sind.

Bei der Erstellung für die macOS-Plattform ist es jedoch möglich, auszuwählen, ob das generierte Framework xamarin. Mac einbetten sollte oder nicht. Xamarin. Mac kann nicht eingebettet werden, wenn die gebundene Assembly nicht auf xamarin. Mac. dll (direkt oder indirekt) verweist. diese Option wird ausgewählt, indem `--platform=macOS` an das .net-Einbettungs Tool übergeben wird.

Wenn die gebundene Assembly einen Verweis auf xamarin. Mac. dll enthält, muss xamarin. Mac eingebettet werden, und der embeddinator muss darüber hinaus wissen, welches Ziel Framework verwendet werden soll.

Es gibt drei mögliche xamarin. Mac-Ziel-Frameworks: `modern` (früher als `mobile`bezeichnet), `full` und `system` (der Unterschied zwischen den einzelnen wird in der [zielframeworkdokumentation][1] von xamarin. Mac beschrieben), und jede wird durch Übergabe von ausgewählt. `--platform=macOS-modern`, `--platform=macOS-full` oder `--platform=macOS-system` zum .net-Einbettungs Tool.

[1]: ~/mac/platform/target-framework.md
