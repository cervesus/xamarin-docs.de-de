---
title: Ziel-C-Plattformen
description: In diesem Dokument werden die verschiedenen Plattformen beschrieben, die .net-Einbettungen beim Arbeiten mit Ziel-C-Code als Ziel haben. Hier werden macOS, Ios, tvos und watchos erläutert.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: f97b595f129cb1ad1ea56e3ae43b0f0a477fef5a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282732"
---
# <a name="objective-c-platforms"></a>Ziel-C-Plattformen

.Net-Einbettungen können bei der Erstellung von Ziel-C-Code auf verschiedene Plattformen abzielen

* macOS
* iOS
* tvOS
* watchos [noch nicht implementiert]

Die Plattform wird durch Übergeben des `--platform=<platform>` Befehlszeilen Arguments an die .net-Einbettung ausgewählt.

Beim Erstellen für IOS-, tvos-und watchos-Plattformen erstellt .net-Einbettungen immer ein Framework, das xamarin. IOS einbettet, da xamarin. IOS viele Lauf Zeit Unterstützungs Codes enthält, die auf diesen Plattformen erforderlich sind.

Bei der Erstellung für die macOS-Plattform ist es jedoch möglich, auszuwählen, ob das generierte Framework xamarin. Mac einbetten sollte oder nicht. Xamarin. Mac kann nicht eingebettet werden, wenn die gebundene Assembly nicht auf xamarin. Mac. dll (direkt oder indirekt) verweist. diese Option wird durch übergeben `--platform=macOS` des .net-Einbettungs Tools ausgewählt.

Wenn die gebundene Assembly einen Verweis auf xamarin. Mac. dll enthält, muss xamarin. Mac eingebettet werden, und der embeddinator muss darüber hinaus wissen, welches Ziel Framework verwendet werden soll.

Es gibt drei mögliche xamarin. Mac-Ziel Framework `modern` `full` -Frameworks `mobile`: (früher `system` aufgerufen) und (der Unterschied zwischen den einzelnen ist in der [zielframeworkdokumentation][1] von xamarin. Mac beschrieben) und jeder wird ausgewählt, `--platform=macOS-full` indem `--platform=macOS-modern`oder `--platform=macOS-system` an das .net-Einbettungs Tool übergeben wird.

[1]: ~/mac/platform/target-framework.md
