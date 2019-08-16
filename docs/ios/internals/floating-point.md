---
title: Gleit Komma Vorgänge in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie xamarin. IOS die Gleit Komma Operationen 32-Bit und 64-Bit-Genauigkeit behandelt und die damit verbundenen Auswirkungen auf die Leistung erläutert.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: cd1bd0507f89f7b29bfcd3ef1ba0a3b1215632ce
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527376"
---
# <a name="floating-point-operations-in-xamarinios"></a>Gleit Komma Vorgänge in xamarin. IOS

Xamarin. IOS führt standardmäßig 32-Bit-und 64-Bit-Gleit Komma Vorgänge mit der 64-Bit-Genauigkeit auf Arm aus.  

Obwohl diese höhere Genauigkeit näher an den Entwicklern liegt, die von Gleit Komma Vorgängen in C# auf dem Desktop erwartet werden, kann die Leistung der Leistung auf mobilen Geräten erheblich beeinträchtigt werden.

Es ist möglich, den 32-Bit-Gleit Komma Code für die Verwendung von 32-Bit-Gleit Komma Vorgängen zu kompilieren.  Zu diesem Zweck müssen Sie mindestens xamarin. IOS 8,10 verwenden und im Bereich der IOS-Buildoptionen auf der Eintrags Zeile "mtouchextra-Argumente" den folgenden Wert festlegen:

```
--aot-options=-O=float32
```

Dadurch werden die statischen Compiler (entweder integrierter statischer Compiler von Mono oder der llvm-gestützte eine) zum Ausführen von Gleit Komma Vorgängen mit 32-Bit-Gleit Komma Werten informiert.
