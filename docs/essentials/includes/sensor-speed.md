---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353268"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Sensor-Geschwindigkeit](xref:Xamarin.Essentials.SensorSpeed)

- **Am schnellsten** – so schnell wie möglich (nicht unbedingt auf UI-Thread), die Sensordaten zu erhalten.
- **Spiel** – geeignet für Spiele, die (nicht unbedingt auf UI-Thread) bewerten.
- **Normale** – der Standardwert ist für die Änderungen der bildschirmausrichtung geeignet ist.
- **Benutzeroberfläche** – geeignet für die allgemeine Benutzeroberfläche zu bewerten.

Wenn der Ereignishandler nicht unbedingt im UI-Thread ausgeführt, und wenn der Ereignishandler benötigt Zugriff auf Elemente der Benutzeroberfläche, die [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) Methode, um diesen Code im UI-Thread auszuführen.