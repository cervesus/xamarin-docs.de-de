---
ms.topic: include
ms.openlocfilehash: 5c11c94956f8d56c66c50a9a480177c5b77c2643
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947433"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Sensor-Geschwindigkeit](xref:Xamarin.Essentials.SensorSpeed)

- **Am schnellsten** – so schnell wie möglich (nicht unbedingt auf UI-Thread), die Sensordaten zu erhalten.
- **Spiel** – geeignet für Spiele, die (nicht unbedingt auf UI-Thread) bewerten.
- **Normale** – der Standardwert ist für die Änderungen der bildschirmausrichtung geeignet ist.
- **Benutzeroberfläche** – geeignet für die allgemeine Benutzeroberfläche zu bewerten.

Wenn der Ereignishandler nicht unbedingt im UI-Thread ausgeführt, und wenn der Ereignishandler benötigt Zugriff auf Elemente der Benutzeroberfläche, die [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) Methode, um diesen Code im UI-Thread auszuführen.