---
title: Gleit Komma Vorgänge in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie xamarin. IOS die Gleit Komma Operationen 32-Bit und 64-Bit-Genauigkeit behandelt und die damit verbundenen Auswirkungen auf die Leistung erläutert.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 11/25/2015
ms.openlocfilehash: 1ecb00fecaf14afb8c6d5c59297eb26821ed791a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291924"
---
# <a name="floating-point-operations-in-xamarinios"></a>Gleit Komma Vorgänge in xamarin. IOS

Xamarin. IOS führt standardmäßig 32-Bit-und 64-Bit-Gleit Komma Vorgänge mit der 64-Bit-Genauigkeit auf Arm aus.  

Obwohl diese höhere Genauigkeit näher an den Entwicklern liegt, die von Gleit Komma Vorgängen in C# auf dem Desktop erwartet werden, kann die Leistung der Leistung auf mobilen Geräten erheblich beeinträchtigt werden.

Es ist möglich, den 32-Bit-Gleit Komma Code für die Verwendung von 32-Bit-Gleit Komma Vorgängen zu kompilieren.  Zu diesem Zweck müssen Sie mindestens xamarin. IOS 8,10 verwenden und im Bereich der IOS-Buildoptionen auf der Eintrags Zeile "mtouchextra-Argumente" den folgenden Wert festlegen:

```
--aot-options=-O=float32
```

Dadurch werden die statischen Compiler (entweder integrierter statischer Compiler von Mono oder der llvm-gestützte eine) zum Ausführen von Gleit Komma Vorgängen mit 32-Bit-Gleit Komma Werten informiert.
