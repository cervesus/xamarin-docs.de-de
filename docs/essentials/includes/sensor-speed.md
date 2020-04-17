---
ms.topic: include
ms.openlocfilehash: b82ebeaeb28195e3ec0f5a0200c0448b6b76196a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "61259475"
---
## <a name="sensor-speed"></a>[Sensorgeschwindigkeit](xref:Xamarin.Essentials.SensorSpeed)

- **Schnellste**: Ruft die Sensordaten so schnell wie möglich ab (in einem UI-Thread ist eine Rückgabe nicht garantiert).
- **Spiel**: Für Spiele geeignete Übertragungsrate (in einem UI-Thread ist eine Rückgabe nicht garantiert).
- **Standard**: Für Änderungen der Bildschirmausrichtung geeignete Standardübertragungsrate.
- **UI**: Für allgemeine Benutzeroberflächenaktionen geeignete Übertragungsrate.

Wenn für Ihren Ereignishandler die Ausführung im UI-Thread nicht garantiert werden kann und der Ereignishandler auf Benutzeroberflächenelemente zugreifen muss, verwenden Sie die Methode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md), um diesen Code im UI-Thread auszuführen.
