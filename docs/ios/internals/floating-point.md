---
title: Gleitkomma
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 67fcf459747152346d32eb5836fa22b99719af12
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="floating-point"></a>Gleitkomma

Xamarin.iOS führt standardmäßig 32 Bit und 64-Bit-Gleitkommaoperationen mit 64-Bit-Präzision auf ARM.  

Während diese höhere Genauigkeit näher ist an was Entwickler von Gleitkommaoperationen in c# auf den Desktop, Mobil erwarten, kann Auswirkungen auf die Leistung erheblich sein.

Es ist möglich, 32-Bit-Gleitkommazahl Punkt Code zur Verwendung von 32-Bit-Gleitkommaoperationen zu kompilieren.  Zu diesem Zweck müssen Sie mindestens verwenden Xamarin.iOS 8.10 und Menge in Ihrer iOS-build-Optionen des Bereichs auf die "Mtouch zusätzlichen Argumente" Eintrag Zeile den folgenden Wert:

     --aot-options=-O=float32

Diese informiert die statischen Compilern (entweder Mono des integrierten statische Compiler oder dem LLVM Energieversorgung) zum Ausführen von Gleitkommaoperationen mit 32-Bit-Gleitkommazahlen.
