---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 676c5a6795ab4896ccd1b288424bf2040b1208aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2018
ms.locfileid: "52294819"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Sensorgeschwindigkeit](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste**: Ruft die Sensordaten so schnell wie möglich ab (in einem UI-Thread ist eine Rückgabe nicht garantiert).
- **Spiel**: Für Spiele geeignete Übertragungsrate (in einem UI-Thread ist eine Rückgabe nicht garantiert).
- **Normal**: Für Änderungen der Bildschirmausrichtung geeignete Standardübertragungsrate.
- **UI**: Für allgemeine Benutzeroberflächenaktionen geeignete Übertragungsrate.

Wenn für Ihren Ereignishandler die Ausführung im UI-Thread nicht garantiert werden kann und der Ereignishandler auf Benutzeroberflächenelemente zugreifen muss, verwenden Sie die Methode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md), um diesen Code im UI-Thread auszuführen.