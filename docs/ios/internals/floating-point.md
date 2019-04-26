---
title: Gleitkommaoperationen in Xamarin.iOS
description: Dieses Dokument beschreibt die Behandlung von Xamarin.iOS auf 32-Bit und 64-Bit-gleitkommavorgänge mit einfacher Genauigkeit und zugehörige Auswirkungen auf die Leistung erläutert.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 28002ddbab80445757be907b5d9f1cc02fdefd9d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035446"
---
# <a name="floating-point-operations-in-xamarinios"></a>Gleitkommaoperationen in Xamarin.iOS

Xamarin.iOS führt standardmäßig 32-Bit und 64-Bit-Gleitkommaoperationen mit 64-Bit-präziser für ARM.  

Während dieses höhere Genauigkeit näher ist an, was Entwickler von Gleitkommaoperationen in erwarten C# auf dem Desktop, auf dem Mobiltelefon, kann Auswirkungen auf die Leistung erheblich sein.

Es ist möglich, den 32-Bit-Gleitkommazahl Punkt Code zur Verwendung von 32-Bit-Gleitkommaoperationen kompilieren.  Zu diesem Zweck müssen Sie mindestens verwenden Xamarin.iOS 8.10, und legen in Ihrer iOS-Optionen Bereich erstellen, auf die "Mtouch zusätzliche Argumente" Eintrag Zeile den folgenden Wert:

     --aot-options=-O=float32

Informiert die statischen Compiler (statischer Compiler von Mono integrierte, oder der LLVM-gestützte eine) mithilfe von 32-Bit-Gleitkommazahlen gleitkommavorgänge mit einfacher ausführen.
