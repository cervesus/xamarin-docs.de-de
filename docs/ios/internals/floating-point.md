---
title: Gleitkomma
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea29132ad4ac55f6fb151ac2125ab1add82c8518
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="floating-point"></a>Gleitkomma

Xamarin.iOS führt standardmäßig 32 Bit und 64-Bit-Gleitkommaoperationen mit 64-Bit-Präzision auf ARM.  

Während diese höhere Genauigkeit näher ist an was Entwickler von Gleitkommaoperationen in c# auf den Desktop, Mobil erwarten, kann Auswirkungen auf die Leistung erheblich sein.

Es ist möglich, 32-Bit-Gleitkommazahl Punkt Code zur Verwendung von 32-Bit-Gleitkommaoperationen zu kompilieren.  Zu diesem Zweck müssen Sie mindestens verwenden Xamarin.iOS 8.10 und Menge in Ihrer iOS-build-Optionen des Bereichs auf die "Mtouch zusätzlichen Argumente" Eintrag Zeile den folgenden Wert:

     --aot-options=-O=float32

Diese informiert die statischen Compilern (entweder Mono des integrierten statische Compiler oder dem LLVM Energieversorgung) zum Ausführen von Gleitkommaoperationen mit 32-Bit-Gleitkommazahlen.
