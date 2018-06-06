---
title: Gleitkommaoperationen in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Xamarin.iOS 32-Bit und 64-Bit-Präzision Gleitkommaoperationen behandelt und zugehörige Auswirkungen auf die Leistung erläutert.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea5d69b52cbd4c76abb236bd1a272633dde440b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786160"
---
# <a name="floating-point-operations-in-xamarinios"></a>Gleitkommaoperationen in Xamarin.iOS

Xamarin.iOS führt standardmäßig 32 Bit und 64-Bit-Gleitkommaoperationen mit 64-Bit-Präzision auf ARM.  

Während diese höhere Genauigkeit näher ist an was Entwickler von Gleitkommaoperationen in c# auf den Desktop, Mobil erwarten, kann Auswirkungen auf die Leistung erheblich sein.

Es ist möglich, 32-Bit-Gleitkommazahl Punkt Code zur Verwendung von 32-Bit-Gleitkommaoperationen zu kompilieren.  Zu diesem Zweck müssen Sie mindestens verwenden Xamarin.iOS 8.10 und Menge in Ihrer iOS-build-Optionen des Bereichs auf die "Mtouch zusätzlichen Argumente" Eintrag Zeile den folgenden Wert:

     --aot-options=-O=float32

Diese informiert die statischen Compilern (entweder Mono des integrierten statische Compiler oder dem LLVM Energieversorgung) zum Ausführen von Gleitkommaoperationen mit 32-Bit-Gleitkommazahlen.
